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

Read repo-root `PLAN.md` first. If it is missing, stop.

This folder is the **only** Trevor skill. Sibling files are progressive disclosure — open them when the table says so. Do not load the entire folder into one prompt unless the human asked for a full brief.

Many agents may run on one computer. Alexis owns persistence. Michael owns the UI. Trevor emits schema-valid flights onto AWS and does not leak raw prompts in logs.

## Always on

- Write only: `sdk/`, `demo-app/`, `infra/`, `scripts/`, `.github/`, `Makefile`
- Read-only after hour 0: `contracts/`
- Never write: `vault/`, `web/`, `PRODUCT.md`, `DESIGN.md`
- Never: Grafana/Streamlit as UI, Langfuse as backend, EKS/AKS, OpenSearch, stock OTLP of raw prompts, AKIA in git, force-push `main`, commit unless asked, start long-running servers

Hour 0 (whole team): `contracts/span.schema.json` + fixtures. **No Trevor lane code until those exist.**

## Open next

| If the task is… | Read |
|---|---|
| Unsure who owns a path | [ownership.md](ownership.md) |
| Parallel agents / worktrees / leases | [parallel.md](parallel.md) |
| SDK, demo, Terraform, pins | [stack.md](stack.md) |
| Secrets, tenants, IAM, CI | [enterprise.md](enterprise.md) |
| Talking to Alexis or Michael | [handoffs.md](handoffs.md) |
| Execution order and done-list | [workflow.md](workflow.md) |

Canonical path: `skills/trevor-recorder/`. Tool stubs under `.cursor/`, `.claude/`, `.kiro/` must not drift — they only point here.
