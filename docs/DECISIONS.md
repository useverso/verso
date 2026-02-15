# VERSO Architecture Decision Records

This document captures key architectural decisions made during the design of VERSO. Each record follows the ADR format: context, decision, alternatives, and consequences.

---

## ADR-001: Framework name -- VERSO

**Status:** Accepted

**Context:** The agentic coding framework needed a name that communicates the workflow concept, is memorable, and works as both a brand and a technical identifier.

**Decision:** VERSO -- each letter maps to a phase: Validate, Engineer, Review, Ship, Observe. The word reads as a natural workflow sequence.

**Alternatives considered:**
- TEMPO -- domain conflicts, music connotation
- CREW -- too generic, existing products
- HELM -- Kubernetes conflict
- DEFT -- weak brand potential
- STRIDE -- security methodology conflict
- FLUX -- overused in tech
- VENTO -- weaker acronym mapping

**Consequences:** Strong brand identity. The name doubles as a mnemonic for the workflow phases. "verso" (Latin: "to turn") reinforces the cyclical nature of the framework.

---

## ADR-002: Config directory -- .verso/ not .pilot/

**Status:** Accepted

**Context:** The configuration directory was initially named `.pilot/` after the Pilot role (the AI orchestrator). This created confusion about whether the directory was specific to the Pilot agent or the framework as a whole.

**Decision:** Renamed to `.verso/` for brand consistency, following the convention established by other tools: `.git` for Git, `.github` for GitHub, `.verso` for VERSO.

**Consequences:** Better discoverability. Clear framework identification. Anyone seeing `.verso/` in a repository immediately knows it uses the VERSO framework.

---

## ADR-003: Agent prompts inside .verso/ not separate prompts/

**Status:** Accepted

**Context:** Agent prompt files (pilot.md, builder.md, reviewer.md) were initially in a separate top-level `prompts/` directory, separate from the configuration in `.verso/`.

**Decision:** Moved all agent prompts into `.verso/agents/` so the entire framework lives in a single directory.

**Consequences:** Portability -- copying one directory gives you everything needed to adopt VERSO. No risk of forgetting to copy the prompts directory. The `.verso/` directory is the complete, self-contained unit of adoption.

---

## ADR-004: No formal GitHub PR reviews

**Status:** Accepted

**Context:** Solo developers cannot approve their own pull requests when branch protection rules require reviews. This creates a broken workflow where the developer is both the author and the only possible reviewer.

**Decision:** The Reviewer agent writes informational comments on the PR (analysis, suggestions, potential issues) but does not submit a formal GitHub review with approve/reject status.

**Consequences:** Eliminates the broken solo-dev review flow. The developer reads the Reviewer's analysis and merges when satisfied. At higher team scales, formal reviews can be layered on top of VERSO's informational review.

---

## ADR-005: Only PR merge closes issues

**Status:** Accepted

**Context:** Agents were closing issues manually after completing work, causing state inconsistencies between the board, the code, and the state machine. Issues could appear "done" before the code was actually merged.

**Decision:** `pr_merged` is the ONLY trigger for the Done state. No agent ever closes issues. GitHub's native auto-close mechanism via "Closes #N" in the PR body handles the transition.

**Consequences:** Zero state inconsistency. The board always reflects reality. If a PR is abandoned, the issue remains open. If a PR is merged, the issue closes automatically. The state machine and GitHub are always in sync.

---

## ADR-006: Tool-agnostic framework

**Status:** Accepted

**Context:** VERSO could be tightly coupled to a specific AI coding tool or positioned as an independent, tool-agnostic framework. Tight coupling would simplify integration but limit adoption.

**Decision:** VERSO is an independent framework. It works with any AI coding tool (Claude Code, Cursor, Copilot, Aider, etc.). Specific tools can integrate as first-class citizens, but VERSO does not depend on or require any particular one.

**Rationale:** Wider adoption. A tool-agnostic methodology attracts developers regardless of their preferred AI coding tool. This follows the Shape Up (methodology) + Basecamp (product) model -- the methodology stands on its own.

**Consequences:** VERSO must be tool-agnostic in its design. Agent prompts reference generic capabilities, not tool-specific APIs. Any tool integration is an optional enhancement, not a requirement.

---

## ADR-007: Goal-based milestones, not time-based sprints

**Status:** Accepted

**Context:** Traditional sprint estimation assumes predictable velocity. AI agent velocity is highly unpredictable -- a task might take 30 seconds or 30 minutes depending on complexity, context, and tool behavior. Time-boxed sprints become meaningless.

**Decision:** Milestones are defined as statements that become true (e.g., "Users can sign in with OAuth") rather than time-boxed iterations. The roadmap uses three horizons: NOW (current focus), NEXT (planned), LATER (aspirational).

**Consequences:** No sprint estimation overhead. No velocity tracking. Progress is measured by milestone completion, not story points burned. The three-horizon model naturally prioritizes without creating artificial deadlines.

---

## ADR-008: Autonomy Dial -- progressive trust

**Status:** Accepted

**Context:** Not all development decisions require human approval, but some absolutely do. A binary "manual vs. automatic" toggle is too coarse -- teams need granular control over which decisions they delegate to AI agents.

**Decision:** A 4-level autonomy dial, configurable per work type:
1. Supervised -- human approves every transition
2. Guided -- human approves spec and plan
3. Trusted -- human approves spec only
4. Autonomous -- everything automatic, human reviews PR before merge

**Consequences:** Teams start at level 1-2 and increase autonomy as trust builds. Different work types can have different levels (e.g., bugs at level 3, features at level 2). The system gracefully scales from "AI as assistant" to "AI as autonomous developer" without changing the underlying framework.

---

## ADR-009: Pilot is the sole board state manager

**Status:** Accepted

**Context:** The state machine in the paper defines an "Actor" column for each transition. The Reviewer agent is listed as actor for `reviewer_commented` (Verifying → PR Ready) and `issues_found` (Verifying → Building). However, the Reviewer prompt says "Never move items on the board. The Pilot manages state transitions." This is a contradiction.

**Decision:** The "Actor" column means "whose action triggers the transition", NOT "who updates the board". The Pilot is always the one who executes board state updates. Other agents (Builder, Reviewer) cause transitions through their actions (creating PRs, posting comments), and the Pilot observes and updates accordingly.

**Rationale:**
1. The Pilot is described as "the guardian of the state machine" -- having multiple agents update board state breaks this guarantee
2. Single point of board management prevents race conditions and ensures consistency
3. Separation of concerns -- the Reviewer is a code quality expert, not a project management tool
4. The Verifying → Building transition requires the Pilot anyway (to re-spawn the Builder with feedback)
5. In Claude Code's architecture, subagents (Builder/Reviewer) naturally return results to the Pilot, which then acts on them
6. The Pilot can add additional logic when transitioning (milestone checks, debt ratio, WIP limits)

**Consequences:** The paper's state machine "Actor" column should be read as "trigger actor" not "board actor". Reviewer prompt correctly says "report verdict to Pilot" -- Pilot handles the board update. Pilot prompts need explicit handlers for when Reviewer returns with its verdict. Builder prompt describes itself as "ephemeral" which is consistent -- it doesn't manage board state either.

---

## ADR-010: Template repo deprecated -- CLI bundles templates as single source of truth

**Status:** Accepted

**Context:** The useverso/template repo and the cli/templates/ directory both contained the `.verso/` starter files. Over time the two copies diverged significantly -- the CLI's embedded templates had grown by ~814 lines of content that the template repo was missing. The CLI already scaffolds from its own local templates and never references the template repo at runtime. Maintaining two copies creates sync bugs and confusion about which source is authoritative.

**Decision:** Deprecate the useverso/template repo. The CLI (useverso/cli) is the single source of truth for all template files. The template repo README is replaced with a deprecation notice pointing users to the CLI. Existing template files remain in the repo as historical reference but are no longer maintained.

**Rationale:**
1. The CLI already embeds templates locally -- the template repo was never consumed programmatically
2. A ~814-line divergence proves dual maintenance is unsustainable
3. Single source of truth eliminates an entire class of sync bugs
4. Users interact with templates exclusively through `npx @useverso/cli init`, not by cloning the template repo
5. Keeping historical files in the deprecated repo preserves git history for reference

**Consequences:** Template updates happen only in useverso/cli. The template repo is archived with a clear deprecation notice. Contributors know exactly where to submit template changes. The risk of users accidentally using stale template files from the deprecated repo is mitigated by the README redirect.
