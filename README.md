# TVault scratchbook

**Scratchpad only.** This GitHub repo is not the product. Plan, brand, and Trevor-lane skills. Application code goes in a separate repo Trevor creates — see [PLAN.md](PLAN.md).

## Humans

Open [PLAN.md](PLAN.md) for the three-person product plan.

## LLMs — load in this order

1. This file
2. [AGENTS.md](AGENTS.md) — paste blocks for parallel agents
3. [PLAN.md](PLAN.md) — product, fences, 48h clock
4. [skills/INDEX.md](skills/INDEX.md) — which folder
5. [skills/trevor-recorder/SKILL.md](skills/trevor-recorder/SKILL.md) — constitution
6. **One** mission under [skills/trevor-recorder/agents/](skills/trevor-recorder/agents/)

| Agent id | Mission | Writes |
|---|---|---|
| `trevor-sdk` | [agents/sdk.md](skills/trevor-recorder/agents/sdk.md) | `sdk/` |
| `trevor-demo` | [agents/demo.md](skills/trevor-recorder/agents/demo.md) | `demo-app/` |
| `trevor-scripts` | [agents/scripts.md](skills/trevor-recorder/agents/scripts.md) | `scripts/` |
| `trevor-infra` | [agents/infra.md](skills/trevor-recorder/agents/infra.md) | `infra/` |
| `trevor-ci` | [agents/ci.md](skills/trevor-recorder/agents/ci.md) | `.github/`, `Makefile` |

Each mission names exact files, APIs, tests, and bans. Do not open all five in one context.

Draft span schema (until hour 0): [span.schema.draft.json](skills/trevor-recorder/span.schema.draft.json)

## Brand

[assets/](assets/) — marks only, not code.

## Not in this repo yet

Alexis and Michael lane folders. Copy [skills/lane-constitution/](skills/lane-constitution/), not [skills/trevor-recorder/](skills/trevor-recorder/). Until those folders exist, they use `PLAN.md` only.
