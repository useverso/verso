# VERSO Project Roadmap

> Last updated: 2026-02-16

This is the VERSO project's own roadmap, structured using VERSO's three-horizon roadmap.

### Principles

VERSO uses goal-based milestones instead of time-based sprints (see [ADR-007](DECISIONS.md#adr-007-goal-based-milestones-not-time-based-sprints) and [Section 10](../paper/VERSO.md#10-milestones-and-roadmap) of the paper). Each horizon is defined by an outcome, not a date: **NOW** (actively building), **NEXT** (planned), **LATER** (aspirational).

## Repository Structure

VERSO is split across two repositories:

- [`useverso/verso`](https://github.com/useverso/verso) — The framework paper, documentation, and brand assets
- [`useverso/cli`](https://github.com/useverso/cli) — CLI tool for initializing and managing VERSO projects

---

## NOW -- Foundation

The minimum viable framework: paper, templates, and repository structure.

- [ ] CLI tool (`npx @useverso/cli init`) — [`useverso/cli`](https://github.com/useverso/cli)
- [ ] Brand assets (logo, colors, typography) — `useverso/verso`; partial, `brand/` has header image, still needs logo and typography

**Milestone:** "A developer can read the paper, run `npx @useverso/cli init`, and add .verso/ to their project."

---

## NEXT -- Launch

Public release and developer adoption.

- [ ] Landing page at useverso.dev (domain TBD)
- [ ] CLI published to NPM as @useverso/cli
- [ ] Update OSS templates with all config.yaml sections from the paper
- [ ] ADR for Refactor template strategy (dedicated template vs. shared with Feature)
- [ ] Observe-phase templates (retrospective checklist, metrics snapshot)
- [ ] Example project showing VERSO in action
- [ ] Launch: Hacker News, Reddit, Twitter, Dev.to

**Milestone:** "VERSO is publicly available, documented, and discoverable by developers searching for agentic workflow frameworks."

---

## LATER -- Ecosystem

Community growth and integrations.

- [ ] Community contributions and feedback
- [ ] First stable release (VERSO 1.0) with formal migration tooling
- [ ] Translations of the paper
- [ ] Video content / talks
- [ ] Case studies from real projects
- [ ] Team-scale adoption guide and multi-Pilot coordination patterns ([Section 12](../paper/VERSO.md#12-scaling))
- [ ] **Per-item cost measurement** — The framework already defines per-item cost metrics (tokens, API cost, agent sessions, wall time — see Section 11 of the paper). The gap is tooling: most AI coding agents don't yet expose token consumption programmatically. When tool APIs mature, the Pilot should log actual token counts per work item automatically.
- [ ] **Production monitoring integration** — The framework defines monitoring as an Observe-phase input that auto-captures bugs. This requires external tooling integration (Sentry webhooks, Datadog alerts → GitHub Issues). Currently, the developer reports production issues conversationally and the Pilot classifies them. A future `verso monitor` CLI command or GitHub Action could automate this.
- [ ] **Automated metrics aggregation** — While retrospectives capture metrics at milestone boundaries, continuous metrics tracking (daily throughput, rolling cycle time) would require persistent storage and a reporting mechanism. A future `verso metrics` CLI command could read `.verso/retros/` and aggregate historical data.

**Milestone:** "VERSO has an active community, multiple integrations, and proven real-world usage."

---

## Done

Completed items from earlier horizons, preserved for project history.

- [x] Framework paper (13 sections) — `useverso/verso`
- [x] OSS template files (.verso/ directory — Refactor uses the Feature template) — `useverso/cli`
- [x] README as landing page — `useverso/verso`
- [x] GitHub repos (`useverso/verso`, `useverso/cli`)
