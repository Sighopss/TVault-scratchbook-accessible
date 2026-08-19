# Skills index

Load **one lane**. Format is `lane-constitution/` for everyone. Content is per owner. HTTP is [contracts/http.draft.md](../contracts/http.draft.md) for all three — do not invent routes.

First clone / “ok let’s start”: [START.md](../START.md). Parallel PRs: [handoffs/README.md](../handoffs/README.md) (one file per PR, claimed paths). Alexis and Michael **fill their own** folders via [FILL-CONSTITUTION.md](FILL-CONSTITUTION.md). Do not copy [trevor-recorder/](trevor-recorder/).

| Lane | Owner | Load |
|---|---|---|
| Format (empty) | all three | [lane-constitution/](lane-constitution/) — copy, then fill. |
| How to fill | Alexis, Michael | [FILL-CONSTITUTION.md](FILL-CONSTITUTION.md) |
| Recorder + AWS | Trevor | [trevor-recorder/SKILL.md](trevor-recorder/SKILL.md) then one [agents/*.md](trevor-recorder/agents/). **Trevor only.** |
| Vault | Alexis | **Not in this repo until Alexis’s LLM writes it.** Copy `lane-constitution/`. Until then: FILL + `PLAN.md` Alexis + HTTP. |
| Explorer | Michael | **Not in this repo until Michael’s LLM writes it.** Copy `lane-constitution/`. Until then: FILL + `PLAN.md` Michael + Look. |

When a lane folder exists, replace the “not in this repo” row with a link to that `SKILL.md`.

## Trevor missions (parallel)

Trevor’s filled missions. Other lanes do not use these ids.

| File | Agent id | Content |
|---|---|---|
| [agents/sdk.md](trevor-recorder/agents/sdk.md) | `trevor-sdk` | Python package, span client, tests |
| [agents/demo.md](trevor-recorder/agents/demo.md) | `trevor-demo` | Bedrock RAG/agent that emits a flight |
| [agents/scripts.md](trevor-recorder/agents/scripts.md) | `trevor-scripts` | `demo_pii_flight.sh` |
| [agents/infra.md](trevor-recorder/agents/infra.md) | `trevor-infra` | Terraform AWS |
| [agents/ci.md](trevor-recorder/agents/ci.md) | `trevor-ci` | Actions + Makefile |

Supporting (Trevor only): [ownership.md](trevor-recorder/ownership.md), [parallel.md](trevor-recorder/parallel.md), [stack.md](trevor-recorder/stack.md), [enterprise.md](trevor-recorder/enterprise.md), [handoffs.md](trevor-recorder/handoffs.md), [workflow.md](trevor-recorder/workflow.md), [span.schema.draft.json](trevor-recorder/span.schema.draft.json), [fixtures/](trevor-recorder/fixtures/).

Paste templates: repo [AGENTS.md](../AGENTS.md).
