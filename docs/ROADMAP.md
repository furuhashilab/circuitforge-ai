# Roadmap & Scope

All features below are **planned** unless a merged PR and reproducible demonstration establish completion. No deadlines have been committed.

**How to execute the roadmap step by step:** [実作業ガイド / WORKFLOW.md](WORKFLOW.md) — starting actions, Claude Code instructions, GitHub branch/PR cycle, and stage-by-stage completion gates.

## P0 — Get a credible first 3D demo
| Order | Work | GitHub |
|---|---|---|
| 1 | Finalize research question, AOI, measurable acceptance criteria | [#1](https://github.com/furuhashilab/circuitforge-ai/issues/1) |
| 1 | Confirm OSM/PLATEAU/3D asset/AI training rights before use | [#9](https://github.com/furuhashilab/circuitforge-ai/issues/9) |
| 2 | Dev environment, tests, contributor documentation | [#2](https://github.com/furuhashilab/circuitforge-ai/issues/2) |
| 3 | Download a bounded OSM graph and export GeoJSON | [#3](https://github.com/furuhashilab/circuitforge-ai/issues/3) |
| 3 | Route contract and valid manual route fixture | [#4](https://github.com/furuhashilab/circuitforge-ai/issues/4) |
| 4 | CesiumJS city/terrain + optional PLATEAU overlay | [#5](https://github.com/furuhashilab/circuitforge-ai/issues/5) |
| 5 | Generic 3D car follows route with camera controls | [#6](https://github.com/furuhashilab/circuitforge-ai/issues/6) |

**P0 demo definition:** Open a known, credited route in a 3D scene and follow a moving generic car across the course; no AI generation required.

## P1 — Automatic circuit generation & evaluation
- [#7](https://github.com/furuhashilab/circuitforge-ai/issues/7): Valid closed-loop candidate generation over an OSM graph with reproducible random seed.
- [#8](https://github.com/furuhashilab/circuitforge-ai/issues/8): Structured natural language preferences, feature metrics, baseline ranking, controlled comparisons.
- Expand geography only after end-to-end tests and meaningful data validation.

## P2 — Independent AI research
- [#10](https://github.com/furuhashilab/circuitforge-ai/issues/10): If license and scope permit, distill candidate ranking from teacher labels into a smaller student; compare quality/latency/cost on held-out AOIs.
- Explicitly distinguish true model training from simply prompting a general-purpose LLM.

## Future (not thesis MVP commitments)
- **Rally Mode:** open-ended stages, road-surface properties, gradient / mountains / terrain, forest-road quality, specific rally camera/vehicle effects.
- **More creative edits:** route constraints, landmark priorities, manual segment adjustments, iterative regeneration.
- **Simulation integration:** physically modeled handling or export to driving simulators, only after investigating feasible formats and intellectual-property rights.
- **International reach:** multiple regions, data completeness differences, language/localization, global deployment.

## Proposed priority rule
Scientific baseline > beautiful demo > personalized AI ranking > distilled student > physically realistic racing game. Reprioritize only after recording the tradeoff in [DECISIONS.md](DECISIONS.md).
