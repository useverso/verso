# VERSO Project Roadmap

This is the VERSO project's own roadmap, structured using VERSO's three-horizon model.

## Repository Structure

VERSO is split across three repositories:

- [`useverso/verso`](https://github.com/useverso/verso) — The framework paper, documentation, and brand assets
- [`useverso/cli`](https://github.com/useverso/cli) — CLI tool for initializing and managing VERSO projects
---

## NOW -- Foundation
The minimum viable framework: paper, templates, and repository structure.

- [x] Framework paper (10 chapters + 3 subsections)
- [x] OSS template files (.verso/ directory)
- [x] README as landing page
- [x] GitHub repos (`useverso/verso`, `useverso/cli`)
- [ ] CLI tool (`npx useverso init`) — [`useverso/cli`](https://github.com/useverso/cli)
- [ ] Brand assets (logo, colors, typography)

**Milestone:** "A developer can read the paper, get the template from useverso/template, and add .verso/ to their project."

---

## NEXT -- Launch

Public release and developer adoption.

- [ ] Landing page at useverso.dev
- [ ] CLI published to NPM as @useverso/cli
- [ ] Update OSS templates with all config sections from paper (ci, quality, dependencies, incidents)
- [ ] Example project showing VERSO in action
- [ ] Launch: Hacker News, Reddit, Twitter, Dev.to

**Milestone:** "VERSO is publicly available, documented, and discoverable by developers searching for agentic workflow frameworks."

---

## LATER -- Ecosystem

Community growth and integrations.

- [ ] Community contributions and feedback
- [ ] Framework versioning (VERSO 1.0, 2.0)
- [ ] Translations of the paper
- [ ] Video content / talks
- [ ] Case studies from real projects
- [ ] **Per-item cost measurement** — Currently, cost tracking is estimation-based. Real per-item token usage requires AI tool APIs to expose consumption data programmatically, which most tools don't support yet. When tooling matures, the Pilot should log actual token counts per work item.
- [ ] **Production monitoring integration** — The framework defines monitoring as an Observe-phase input that auto-captures bugs. This requires external tooling integration (Sentry webhooks, Datadog alerts → GitHub Issues). Currently, the developer reports production issues conversationally and the Pilot classifies them. A future `verso monitor` CLI command or GitHub Action could automate this.
- [ ] **Automated metrics aggregation** — While retrospectives capture metrics at milestone boundaries, continuous metrics tracking (daily throughput, rolling cycle time) would require persistent storage and a reporting mechanism. A future `verso metrics` CLI command could read `.verso/retros/` and aggregate historical data.

**Milestone:** "VERSO has an active community, multiple integrations, and proven real-world usage."
