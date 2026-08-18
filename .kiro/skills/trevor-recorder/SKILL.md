---
name: trevor-recorder
description: >-
  Owns the TraceVault Trevor lane (Python SDK, Bedrock demo agent, Terraform,
  GitHub Actions) and coordinates parallel agents on one machine with Alexis
  (vault) and Michael (web). Use when working in sdk/, demo-app/, infra/,
  scripts/, .github/, Makefile, AWS deploy, CI/CD, git worktrees, or team
  handoffs for the AI Application Flight Recorder.
compatibility: Cursor, Claude Code, Kiro, any Agent Skills loader
metadata:
  owner: Trevor
  product: TraceVault
  lane: recorder-aws
---

# Trevor lane — Recorder + AWS

You are a coding agent on **Trevor’s lane** of TraceVault (AI Application Flight Recorder).

Read repo-root `PLAN.md` and this file before any edit. Then open **exactly one** mission file under `agents/` that matches your assigned id. Execute that mission. Do not wander into another mission.

If `PLAN.md` is missing, stop.

## Product (do not invent another)

One request = one **flight** = one `trace_id` with child spans (`llm`, `tool`, `rag`, `http`). Trevor **creates** those spans from a tiny Bedrock RAG/agent and ships them to AWS. Alexis **stores** only redacted payloads. Michael **renders** them in Next.js. Grafana is not the product. Langfuse is not the backend.

Judge trigger Trevor owes: `scripts/demo_pii_flight.sh` runs a demo whose user prompt contains an email and a fake SSN. Trevor still emits schema-valid JSON with `tenant_id`, tokens, `cost_usd`. Alexis redacts at ingest. Michael shows `REDACTED`.

## Hard fences (non-negotiable)

**Write:** `sdk/`, `demo-app/`, `infra/`, `scripts/`, `.github/`, `Makefile`

**Read-only after hour 0:** `contracts/` (schema + fixtures). Hour 0 is the three humans. Until `contracts/span.schema.json` exists, implement against `skills/trevor-recorder/span.schema.draft.json` and copy it into `contracts/` only if the human said hour 0 is done.

**Never write:** `vault/` (Alexis), `web/` (Michael), `PRODUCT.md`, `DESIGN.md`, `assets/`

**Never:** Streamlit/Grafana UI, Langfuse/Phoenix store, EKS/AKS, OpenSearch, stock OTLP export of raw prompts, commit AWS keys, force-push `main`, commit unless the human asked, start long-running servers, log raw prompts.

If you need a vault or web change: write `handoffs/FROM-trevor.md` using the template in [handoffs.md](handoffs.md) and stop.

## Parallel (one computer, many agents)

Claim a lease in `.agent-leases.json` (gitignored) before writing. One id, one path set. Details: [parallel.md](parallel.md).

| Your id | Mission file | Paths |
|---|---|---|
| `trevor-sdk` | [agents/sdk.md](agents/sdk.md) | `sdk/` |
| `trevor-demo` | [agents/demo.md](agents/demo.md) | `demo-app/` |
| `trevor-scripts` | [agents/scripts.md](agents/scripts.md) | `scripts/` |
| `trevor-infra` | [agents/infra.md](agents/infra.md) | `infra/` |
| `trevor-ci` | [agents/ci.md](agents/ci.md) | `.github/`, `Makefile` |

Human Trevor may hold infra+ci together. Subagents must not.

Worktree: `.worktrees/<id>/` on branch `trevor/<id>/<slug>`. Do not nest worktrees. Do not share a dirty tree.

## Stack (locked)

Python 3.12 + `uv`. Terraform >= 1.9. GitHub Actions. Bedrock Claude or Nova, `us-east-1`. GNU coreutils, no PowerShell, prefer WSL2.

Span JSON fields (must match schema): `trace_id`, `span_id`, `parent_id`, `tenant_id`, `kind`, `name`, `status`, `start_time`, `end_time`, `gen_ai.request.model`, `gen_ai.usage.input_tokens`, `gen_ai.usage.output_tokens`, `cost_usd`, `attributes`, `events`.

Env: `TRACEVAULT_INGEST_URL`, `TRACEVAULT_TENANT_KEY`, `AWS_REGION`, `BEDROCK_MODEL_ID`. Never hardcode URLs or keys.

Full pin list: [stack.md](stack.md). Security bar: [enterprise.md](enterprise.md). Team I/O: [handoffs.md](handoffs.md). Steps: [workflow.md](workflow.md).

## After the mission

Release the lease. Leave a 10-line handoff: what files, what env vars, what is blocked on Alexis/Michael. Do not start the next mission unless the human named it.
