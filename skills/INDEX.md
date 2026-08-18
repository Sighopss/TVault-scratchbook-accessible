# Skills index

Load **one lane**. Copy Trevor’s folder shape. Do not run a lane off `PLAN.md` alone.

| Lane | Owner | Load |
|---|---|---|
| Recorder + AWS | Trevor | [trevor-recorder/SKILL.md](trevor-recorder/SKILL.md) then one [agents/*.md](trevor-recorder/agents/) |
| Vault | Alexis | `skills/alexis-vault/` — **not written yet**. Clone `trevor-recorder/` (see PLAN.md “Skills constitution”). Until it exists, `PLAN.md` only. |
| Explorer | Michael | `skills/michael-explorer/` — **not written yet**. Clone `trevor-recorder/` + Impeccable. Until it exists, `PLAN.md` + Impeccable only. |

## Trevor missions (parallel)

| File | Agent id | Content |
|---|---|---|
| [agents/sdk.md](trevor-recorder/agents/sdk.md) | `trevor-sdk` | Python package, span client, tests |
| [agents/demo.md](trevor-recorder/agents/demo.md) | `trevor-demo` | Bedrock RAG/agent that emits a flight |
| [agents/scripts.md](trevor-recorder/agents/scripts.md) | `trevor-scripts` | `demo_pii_flight.sh` |
| [agents/infra.md](trevor-recorder/agents/infra.md) | `trevor-infra` | Terraform AWS |
| [agents/ci.md](trevor-recorder/agents/ci.md) | `trevor-ci` | Actions + Makefile |

Supporting: [ownership.md](trevor-recorder/ownership.md), [parallel.md](trevor-recorder/parallel.md), [stack.md](trevor-recorder/stack.md), [enterprise.md](trevor-recorder/enterprise.md), [handoffs.md](trevor-recorder/handoffs.md), [workflow.md](trevor-recorder/workflow.md), [span.schema.draft.json](trevor-recorder/span.schema.draft.json), [fixtures/](trevor-recorder/fixtures/).

Paste templates: repo [AGENTS.md](../AGENTS.md).
