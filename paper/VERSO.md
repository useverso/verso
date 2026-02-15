# VERSO

## A Development Framework for the Agentic Era

**Validate -- Engineer -- Review -- Ship -- Observe**

*The first development methodology designed for developers working with AI coding agents.*

---

## 1. The Problem

Every development methodology in wide use today was designed to solve one fundamental problem: **coordinating humans**.

Scrum invented standups so people could synchronize. Sprints exist so teams can commit to shared deadlines. Retrospectives exist because humans forget lessons. Story points exist because human output is variable and needs predicting. Kanban boards exist because humans lose track of what others are doing.

These are real problems. They deserve real solutions. And for teams of humans writing code by hand, these methodologies work.

But a solo developer working with AI coding agents has **none of these problems**. There is no one to synchronize with. There are no estimation games to play -- an agent either completes the task or it does not. There is no communication overhead -- the agent has perfect recall of every conversation. There is no context-switching penalty -- each agent carries its own context window.

What this developer has instead is a **new problem that no existing methodology addresses**: the bottleneck has moved from execution to decision-making.

An AI agent can write a feature in minutes. But it needs someone to tell it *what* to write, *approve* that the output is correct, and *decide* what comes next. If the developer is busy making trivial decisions the AI could handle autonomously, the entire system stalls. If the developer is absent, work stops completely -- not because there is nothing to do, but because no one is authorized to decide.

Traditional methodologies assume:

- **Multiple humans coordinating** -- a solo dev has no coordination problem
- **Humans doing the work** -- AI agents do the work
- **Communication is expensive** -- AI has perfect memory and infinite patience
- **Context switching has cost** -- each agent session is isolated by design

None of these assumptions hold. We are trying to run a Formula 1 car on horse-carriage road rules. What we need is a framework designed from scratch for the world we actually live in.

---

## 2. The Paradigm Shift

VERSO is built on a single core thesis: **optimize for decision throughput**.

Decision throughput means: maximize correct decisions per hour of developer time, and minimize unnecessary decisions entirely. Every minute the developer spends on a decision an AI could have made is a minute the system is underperforming. Every decision the AI makes that should have been the developer's is a risk.

Three principles govern VERSO:

### The developer is brain, not hands

The developer decides WHAT to build and WHEN to ship. Never HOW to implement it. The moment you catch yourself typing code that an agent could write, you are misallocating your most scarce resource -- your judgment.

### Trust is progressive

Not every decision needs human approval. A trivial config change does not deserve the same scrutiny as a database migration. VERSO makes trust configurable: the more predictable the outcome, the less intervention required. Trust starts low and grows as the system proves itself.

### The board is the engine

In traditional workflows, the project board is a passive tracker -- a mirror of reality that someone has to manually update. In VERSO, the board IS the reality. It is a formal state machine that governs what can happen next, who can trigger it, and what guards must pass. No agent can skip a state. No work item can regress without a reason. The board does not reflect the process -- it *is* the process.

The developer becomes two things: a **Product Brain** (deciding what matters) and a **Quality Gate** (deciding what ships). Everything else -- research, implementation, testing, code review, documentation -- is delegated to AI agents operating under the board's authority.

---

## 3. The VERSO Cycle

Five macro-phases. Continuous flow. No sprints.

### V -- Validate

*"Does this idea deserve to exist?"*

This is the creative fuzzy front end. The developer and AI act as partners: brainstorming, researching, checking feasibility, detecting duplicates.

- **EXPLORE**: Ideation, competitive analysis, technical feasibility
- **DEFINE**: Spec writing, acceptance criteria, scope boundaries

Ideas can die here -- and that is the system working correctly. A killed idea costs nothing. A built-and-discarded feature costs everything.

**Output**: Either nothing (idea rejected) or a captured work item with enough context for an agent to build it.

### E -- Engineer

*"Build the solution."*

- **DESIGN**: UX/UI wireframes, technical architecture decisions
- **PLAN**: Decomposition into sub-tasks, dependency mapping
- **BUILD**: AI Builder agents implement in isolated worktrees, writing tests alongside code

One feature branch. One PR per work item. Sub-tasks are commits, not separate reviewable units.

### R -- Review

*"Is this good enough to ship?"*

- **TEST**: Automated checks -- type checking, unit tests, linting, integration tests
- **VERIFY**: AI Reviewer agent reads the diff against the original spec

The Reviewer writes an informational comment on the PR. Not a formal GitHub approval -- that mechanism was designed for human teams and adds friction for solo developers. The human makes the final judgment.

### S -- Ship

*"Deliver it."*

- **DELIVER**: The developer merges the PR. This is the ONLY action that closes a work item. No agent ever closes issues manually -- `pr_merged` is the sole trigger.
- **LAUNCH**: Release tagging, changelog generation (automated from PR descriptions), documentation updates

### O -- Observe

*"What did we learn?"*

- Metrics collection: throughput, cycle time, first-pass rate, cost per feature
- Automated retrospective at milestone completion
- Learnings fed back into agent prompts -- the Builder and Reviewer get smarter over time
- User feedback flows into the next Validate cycle

The cycle is continuous. Work flows through V-E-R-S-O without batching or sprints. Observe feeds directly back into Validate.

### Shortcuts by Work Type

Not everything needs the full cycle:

| Type | Path | Rationale |
|------|------|-----------|
| Feature | V - E - R - S - O | Full cycle, full rigor |
| Enhancement | V - E - R - S - O | Full cycle, existing context |
| Bug | V - E - R - S | Skip O unless systemic |
| Hotfix | E - R - S | Skip V -- urgency overrides |
| Chore | E - S | Skip V and R -- low risk |
| Refactor | V - E - R - S - O | V = scope approval |

### 3.1 Cross-cutting Concerns

These are quality dimensions that span multiple VERSO phases. They are not separate phases — they are embedded into the existing cycle as gates, checklist items, and configuration.

**CI/CD Integration**

CI/CD is not a phase — it is the infrastructure that enforces transitions. In VERSO, the CI pipeline is a transition guard: the state machine cannot move from Building to Verifying unless CI passes. The Builder agent is responsible for ensuring the pipeline is green before creating a PR. The Reviewer agent validates that all checks pass before writing its review comment.

Configuration in `.verso/config.yaml`:

```yaml
ci:
  required_checks:
    - typecheck
    - tests
    - lint
  block_transition: true    # Building → Verifying requires all checks green
```

**Security**

Security is a Review-phase gate. The Reviewer agent's checklist includes: dependency vulnerability scanning, secrets detection, OWASP basics (injection, XSS, auth issues). At startup scale and above, security becomes a formal gate with dedicated tooling. At solo dev scale, it is a checklist item the Reviewer evaluates.

**Accessibility**

Accessibility follows the same pattern as security: a checklist item in Review at solo dev scale, a formal gate at enterprise scale. The Reviewer checks: keyboard navigation, semantic HTML, ARIA attributes, color contrast. Configuration determines whether accessibility issues block the transition or are flagged as warnings.

**Testing Strategy**

Testing is split across two phases. The Builder writes tests during Engineer (unit tests, integration tests alongside implementation). The Reviewer validates test coverage and quality during Review. Minimum coverage thresholds can be configured:

```yaml
quality:
  security_gate: warn        # warn | block
  accessibility_gate: warn   # warn | block
  min_coverage: 80           # minimum test coverage percentage
  require_tests: true        # Builder must include tests
```

### 3.2 Parallel Processes

These processes run continuously alongside the VERSO cycle. They are not triggered by work items — they generate work items.

**Monitoring and Analytics (Post-Ship)**

After Ship, the Observe phase includes monitoring whether shipped features actually work in production. Analytics data (error rates, performance metrics, user behavior) feeds back into the Validate phase. If a shipped feature causes errors, the monitoring system auto-captures a bug work item in Captured state.

This closes the loop: Ship → Observe → monitoring detects issue → new work item in Validate → cycle continues.

**Dependency Management**

Dependencies are a continuous background process. Tools like Dependabot or Renovate auto-generate PRs for dependency updates. In VERSO, these enter the board as Chore-type work items with autonomy level 4 (auto) by default. If CI passes, the Pilot can auto-merge without developer intervention. If CI fails, the item escalates to the developer.

```yaml
dependencies:
  auto_capture: true         # auto-create work items from dependency alerts
  default_type: chore
  default_autonomy: 4        # auto-merge if CI passes
  security_patches: hotfix   # security updates treated as hotfixes
```

**Feature Flags**

Feature flags enable partial releases — shipping code that is not yet active for all users. In VERSO, feature flags are an Engineer-phase decision: the Pilot determines whether a work item requires a flag based on risk, size, or the developer's preference. The Builder implements the flag. Ship activates the flag for a percentage of users. Observe monitors the rollout. Full activation is a separate Chore-type work item.

### 3.3 External Entry Points

Not all work originates from the developer saying "I want to build X." Some work enters the system from external sources. VERSO defines how each source maps to the board.

**User Feedback**

Users report bugs, request features, and provide feedback through various channels (support tickets, GitHub issues, social media, app reviews). The Pilot classifies incoming feedback:

- Bug report → work item in Captured, type Bug
- Feature request → work item in Captured, type Feature (evaluated during Validate)
- Noise / duplicate → Cancelled

At solo dev scale, the developer forwards feedback to the Pilot conversationally. At team scale, integrations (Intercom, Zendesk, GitHub Issues) can auto-capture directly into the board.

**Incident Response**

When production goes down, speed matters more than process. VERSO handles incidents as fast-tracked Hotfixes:

1. Alert fires (monitoring, error tracking, user report)
2. Work item auto-captured as Hotfix in Captured state
3. Hotfix path activates: skip Validate, go directly to Engineer
4. Builder agent fixes the issue in a dedicated worktree
5. Reviewer does a minimal review (focus: does the fix work, does it break anything else)
6. Developer merges immediately

Incident severity can override autonomy settings:

```yaml
incidents:
  severity_override: true
  critical:
    autonomy: 3              # skip spec approval, approve only PR
    wip_override: true        # ignore WIP limits for critical incidents
  major:
    autonomy: 3
    wip_override: false
```

**Automated Alerts**

Dependency vulnerabilities, certificate expirations, infrastructure alerts, and scheduled maintenance all generate work items automatically. These enter the board as the appropriate type (Chore, Bug, or Hotfix depending on severity) and follow their respective shortcuts through the VERSO cycle.

---

## 4. The State Machine

VERSO is governed by a formal finite state machine. This is what prevents the inconsistencies and dropped-ball failures that plague ad-hoc agentic workflows.

### States

```mermaid
stateDiagram-v2
    Captured --> Refined : spec_written
    Captured --> Queued : simple_work
    Captured --> Cancelled : rejected_or_duplicate
    Refined --> Queued : breakdown_complete
    Queued --> Building : builder_spawned
    Building --> Verifying : pr_created
    Building --> Queued : build_failed
    Verifying --> PR_Ready : reviewer_commented
    Verifying --> Building : issues_found
    PR_Ready --> Done : pr_merged
    PR_Ready --> Building : dev_requested_changes
```

Mapping to VERSO phases:

| Phase | States |
|-------|--------|
| Validate | Captured, Refined |
| Engineer | Queued, Building |
| Review | Verifying, PR Ready |
| Ship | Done (merged/released) |
| Observe | Periodic activity, not a board state |

### Transitions

Every transition has a **trigger**, a **guard**, and an **actor**:

| From | To | Trigger | Guard | Actor |
|------|----|---------|-------|-------|
| Captured | Refined | spec_written | dev_approved (if autonomy <= 2) | Pilot |
| Captured | Queued | simple_work | none | Pilot |
| Captured | Cancelled | rejected_or_duplicate | none | Pilot |
| Refined | Queued | breakdown_complete | dev_approved (if autonomy <= 2) | Pilot |
| Queued | Building | builder_spawned | wip_limit_ok | Pilot |
| Building | Verifying | pr_created | none | Builder |
| Building | Queued | build_failed | retries_remaining | Pilot |
| Verifying | PR Ready | reviewer_commented | none | Reviewer |
| Verifying | Building | issues_found | none | Reviewer - Pilot - Builder |
| PR Ready | Done | pr_merged | none | Developer (ONLY) |
| PR Ready | Building | dev_requested_changes | none | Developer - Pilot - Builder |

**The golden rule: no agent can skip states.** The Pilot enforces this. Only `pr_merged` triggers Done. No agent ever closes issues manually.

**Note on the Actor column:** The Actor indicates whose action *triggers* the transition, not who updates the board. The Pilot is always the agent that executes board state changes. When the Builder creates a PR (triggering `pr_created`), the Pilot observes this and moves the item from Building to Verifying. When the Reviewer posts a comment (triggering `reviewer_commented`), the Pilot reads the verdict and moves the item accordingly. This ensures a single point of state management and prevents race conditions.

### WIP Limits

- **Building**: configurable, default 2 concurrent items
- **PR Ready**: configurable, default 5 items awaiting merge

WIP limits prevent the system from producing more output than the developer can review. An agent that opens 20 PRs while the developer sleeps is not being productive -- it is creating a review backlog that degrades quality.

---

## 5. Roles

VERSO has exactly three roles. This is deliberate.

### The Captain (Developer)

The human. Sets direction, makes decisions, approves quality.

- Expresses intent in natural language
- Approves at quality gates (frequency configured via the Autonomy Dial)
- Merges PRs -- the only irreversible action in the system
- Never writes code unless they choose to

### The Pilot (AI Orchestrator)

The developer's conversational partner. The main AI session that runs continuously.

- Classifies developer intent automatically (no commands to remember)
- Routes work to the correct VERSO phase
- Manages the board state machine -- enforces transitions, guards, WIP limits
- Spawns and coordinates Builder and Reviewer agents
- Reports status proactively
- **Never writes code itself**

The Pilot loads a dedicated system prompt from `.verso/agents/pilot/` (e.g., `solo-dev.md` for solo developers, `tech-lead.md` for tech leads) defining routing rules, autonomy configuration, and state machine enforcement. A shared `core.md` file contains the common logic all Pilot variants use.

### The Crew (AI Agents)

Ephemeral, specialized agents spawned by the Pilot:

**Builder Agent**: Receives an issue with spec and acceptance criteria. Produces a PR with code, tests, and documentation. Works in an isolated worktree. Loads `.verso/agents/builder.md` as its prompt.

**Reviewer Agent**: Receives a PR and the original spec. Reads the entire diff, validates against acceptance criteria, runs automated checks. Writes an informational review comment. Loads `.verso/agents/reviewer.md` as its prompt.

Two agent types. One orchestrator. The Pilot never writes code. The Crew never makes product decisions.

### The Zero-Command Interface

The developer does not memorize commands. They talk:

- *"I want users to export data as CSV"* -- Pilot classifies as Feature, starts Validate
- *"The login page crashes on Safari"* -- Pilot classifies as Bug, routes accordingly
- *"Production is down"* -- Pilot classifies as Hotfix, fast-tracks to Engineer
- *"What's the status?"* -- Pilot reads the board, reports
- *"Start the next thing"* -- Pilot picks highest priority Queued item, spawns Builder
- *"Ship it"* -- Pilot identifies PR Ready items

Intent classification happens in the Pilot's system prompt via routing rules, not a separate tool or model.

---

## 6. The Autonomy Dial

Not all decisions need human approval. The Autonomy Dial is a per-work-type configuration that controls how much trust the developer places in the AI:

```mermaid
flowchart LR
    L1["1\nFull Control"] --- L2["2\nDefault"] --- L3["3\nPR Only"] --- L4["4\nFull Auto"]
```

| Level | Developer Approves |
|-------|-------------------|
| 1 | Spec, plan, every commit, PR |
| 2 | Spec and PR (DEFAULT) |
| 3 | Only PR |
| 4 | Just merges (or auto-merge with confidence threshold) |

Configuration is per work type:

```yaml
autonomy:
  feature: 2       # approve spec + PR
  bug: 3           # approve only PR
  hotfix: 3        # fast-track, approve only PR
  refactor: 2      # approve scope + PR
  chore: 4         # auto -- just merge when ready
  enhancement: 2   # approve spec + PR
```

### Trust builds over time

Start at level 2. Observe the results. Adjust:

- The Builder handles CSS tasks well -- raise `bug` autonomy to 3
- Specs are consistently accurate -- raise `feature` autonomy to 3
- The Reviewer catches real issues -- raise `chore` autonomy to 4

The Autonomy Dial creates a spectrum from paranoid oversight to full delegation. The developer adjusts based on comfort, project maturity, and risk tolerance. There is no correct setting -- only the setting that matches reality.

---

## 7. Milestones and Roadmap

### Why sprints fail for agentic development

- Time estimation is meaningless when an agent can do in 1 hour what took 1 week
- Sprint commitments assume stable velocity; AI velocity is unpredictable
- Sprint ceremonies are overhead for a solo developer

### Goal-based milestones

A milestone is a **statement that becomes true**:

```yaml
milestone:
  name: "MVP Core"
  goal: "A developer can have a productive coding session"
  criteria:
    - "Session lifecycle works end-to-end"
    - "Chat with Claude renders correctly"
    - "Terminal panel is functional"
    - "Core keyboard shortcuts work"
  exit_criteria:
    - "Zero critical bugs"
    - "TypeScript compiles clean"
```

The milestone completes when ALL criteria are met. Not when a date arrives.

### Three-horizon roadmap

```mermaid
flowchart LR
    NOW["NOW\nCurrent milestone, actively building"] --> NEXT["NEXT\nPlanned, not yet refined"] --> LATER["LATER\nBig ideas, captured but not planned"]
```

No dates. No quarters. The developer always knows what is NOW, NEXT, and LATER.

### Milestone-driven releases

When a milestone completes:
- Version number calculated from rules (milestone = minor bump, hotfix = patch)
- Changelog auto-generated from merged PRs
- Git tag created, GitHub release published
- The Pilot proposes; the developer approves

The Pilot is milestone-aware at all times: it prioritizes work that closes milestone criteria, alerts when a milestone is achievable, and warns about scope creep.

---

## 8. Metrics and ROI

### What to measure

| Metric | Indicates | Improve By |
|--------|-----------|------------|
| Throughput | Items done per week | Raise autonomy, reduce WIP |
| Cycle time | Captured to Done | Find state machine bottlenecks |
| Gate time | Time at decision points | Raise autonomy, better context |
| First-pass rate | PRs merged without rework | Improve agent prompts |
| Debt ratio | Debt items / total | Maintain near 20% |
| Milestone velocity | Criteria met per week | Focus on milestone-closing work |
| Unplanned ratio | Hotfixes+bugs / total | Indicates codebase stability |

### Cost metrics -- unique to agentic development

This is where VERSO produces insights no traditional methodology can. Every work item carries measurable cost:

**Per work item:**
- API tokens consumed (input + output)
- API cost in dollars
- Number of agent sessions (builder + reviewer + retries)
- Agent wall time
- Developer gate time (time at decisions)
- Developer review time (time reviewing the PR)

**ROI calculation:**

```
Example milestone report -- MVP Core (22 items):

  AI costs:        $18.30 (avg $0.83/item)
  Dev time:        9.5 hours
  Total cost:      $730 (at $75/hr dev rate)

  Traditional:     $13,500 (180 dev-hours, ~1 month elapsed)
  ROI:             94.6% cost reduction
```

### Tech debt as a first-class metric

VERSO recommends a **20% debt ratio** -- roughly 1 in 5 work items should address debt. The Pilot tracks this and suggests debt work when the ratio drops.

Agentic development introduces debt categories traditional methodologies do not recognize:

- **Agent-generated debt**: shortcuts the AI took that a human would not
- **Knowledge debt**: code that works but no one understands why
- **Intentional debt**: shipped for the milestone, scheduled for later
- **Drift**: dependencies outdating, patterns diverging across the codebase

### Learning feedback loop

After each milestone, the Pilot auto-generates a retrospective:
- Statistics (throughput, cycle time, first-pass rate, total cost)
- Patterns (what caused rework, what shipped cleanly)
- Suggested improvements (prompt changes, autonomy adjustments, checklist updates)

Learnings are incorporated into Builder and Reviewer prompts. The system gets smarter with every cycle.

---

## 9. Scaling

VERSO was designed for solo developers but scales without changing its core. The state machine stays the same. What changes is roles, permissions, documentation requirements, and ceremony level.

### The same state machine, different permissions

```
                  SOLO DEV     SMALL TEAM     STARTUP       ENTERPRISE
Roles              1 (all)      2-3            5+            10+
Pilot instances    1            per dev        per dev       per dev
Board              local        shared/synced  per team      hierarchy
Code review        AI only      AI + human     human + AI    formal process
QA                 checklist    basic plan     QA role       QA team
Docs required      minimal      moderate       comprehensive full suite
Autonomy default   2-3          2              1-2           1
```

### Role permissions by scale

| Phase | Solo Dev | Team Dev | Tech Lead | PM/PO |
|-------|----------|----------|-----------|-------|
| Validate | full | receives work | full | full |
| Engineer | full | full | -- | -- |
| Review | full (AI) | full (human + AI) | full | -- |
| Ship | full | -- | full | -- |
| Observe | full | -- | full | full |

### Team workflow

In a team, each developer has their own Pilot with role-appropriate permissions:

- **PM/PO's Pilot**: Validate phase -- capture ideas, write PRDs, prioritize the backlog
- **Tech Lead's Pilot**: Validate + Engineer planning -- architecture, breakdown, assignment
- **Team Dev's Pilot**: Engineer + Review -- build assigned tasks, create PRs
- **QA's Pilot**: Review phase -- testing, verification, sign-off

Each role loads a different prompt from `.verso/agents/pilot/` (e.g., `team-dev.md`, `tech-lead.md`, `pm.md`), all sharing `core.md` for common logic. Same state machine, different access levels.

### Documentation expectations by scale

| Document | Solo Dev | Small Team | Startup | Enterprise |
|----------|----------|------------|---------|------------|
| Vision | 1 page | 1 page | 2-3 pages | Full document |
| Roadmap | Simple yaml | Shared yaml | With OKRs | Multi-team |
| PRD | No (spec in issue) | Big features | Always | Formal template |
| Design doc | Optional | Expected | Required + flows | + User research |
| ADRs | Light | Yes | Yes | Formal process |
| Test plan | Checklist | Checklist | QA plan | Full strategy |
| Launch plan | Post online | Checklist | Full plan | GTM strategy |

---

## 10. Getting Started

### For solo developers

VERSO is designed for incremental adoption. You do not need everything on day one.

#### Step 1: Add the .verso directory

Scaffold the starter files into your project using the CLI:

```bash
npx @useverso/cli init
```

This gives your project:

```
your-project/
  .verso/
    config.yaml             # Autonomy levels, WIP limits, scale
    board.yaml              # Local board (work items)
    roadmap.yaml            # Milestones and horizons
    state-machine.yaml      # Transition rules (use the default)
    releases.yaml           # Versioning and release rules
    agents/
      pilot/
        core.md             # Shared Pilot logic
        solo-dev.md         # Solo developer Pilot prompt
        team-dev.md         # Team developer Pilot prompt
        tech-lead.md        # Tech lead Pilot prompt
        pm.md               # PM/PO Pilot prompt
      builder.md            # Builder agent instructions
      reviewer.md           # Reviewer agent instructions
    templates/
      issue-feature.md      # Template for feature issues
      issue-bug.md          # Template for bug reports
      issue-hotfix.md       # Template for hotfix issues
      issue-chore.md        # Template for chore items
      spec.md               # Specification template
      pr.md                 # Pull request template
```

#### Step 2: Set up your board

The local board (`.verso/board.yaml`) is ready to use out of the box -- no external service required. Your work items live in version control, right next to your code.

If you prefer an external provider, configure it in `config.yaml`:

```yaml
board:
  provider: local              # local | github | linear
```

The `local` provider is the default. GitHub Projects and Linear are optional sync targets -- you can push your local board to them at any time via `verso sync`. All VERSO examples work identically regardless of which provider you choose.

#### Step 3: Start working

Open your AI coding assistant. Load the Pilot prompt. Start talking.

*"I want to add user authentication."*

The Pilot takes it from there.

### For teams

1. Set your scale in `config.yaml`: `solo`, `small-team`, `startup`, or `enterprise`
2. Create role-specific Pilot prompts in `.verso/agents/pilot/`
3. Each team member loads their role's prompt
4. Set up a shared board -- use `provider: github` or `provider: linear` in `config.yaml`, then run `verso sync` to keep local and remote boards in sync
5. Work flows through V-E-R-S-O with role-appropriate gates

### The VERSO ecosystem

- **[Framework](https://github.com/useverso/verso)**: The specification and documentation. Free forever. Framework-agnostic, tool-agnostic.
- **[CLI](https://github.com/useverso/cli)**: Scaffolds the `.verso/` directory into your project, manages board sync, and more. Includes all starter templates. Works with Claude Code, Cursor, Copilot, or any AI coding tool.

---

*VERSO is not a process to follow. It is a machine to run. Configure it, start it, and let it do what machines do best -- execute relentlessly while you do what humans do best -- decide wisely.*

---

*Crafted with ❤️ by [Boudy de Geer](https://linkedin.com/in/boudydegeer)*

[LinkedIn](https://linkedin.com/in/boudydegeer) · [X/Twitter](https://x.com/boudydegeer)
