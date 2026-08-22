# Handoff — trevor-docs — handbook-cycle

- Date: 2026-08-21
- Human: Trevor
- Agent id: trevor-docs
- Branch: trevor/docs/handbook-cycle
- PR: TBD
- Mission file: PLAN Handbook Cycle + Rubric 100 (scratchpad)

## Claimed paths (collision)

```
PLAN.md
START.md
AGENTS.md
README.md
handoffs/
.github/pull_request_template.md
skills/FILL-CONSTITUTION.md
skills/lane-constitution/workflow.md
skills/lane-constitution/progress.md
skills/trevor-recorder/SKILL.md
skills/trevor-recorder/workflow.md
contracts/README.md
```

## Do not touch

```
vault/
web/
sdk/
skills/alexis-*
skills/michael-*
```

## Safe to run in parallel with

Alexis FILL-CONSTITUTION / Michael FILL-CONSTITUTION (their folders only).

## Handbook evidence (required — 2026 workbook)

- Lifecycle stage: Discover (process lock so later stages cannot skip)
- P-ids this PR moves: P-05, P-06, P-07, P-08, P-09, P-11, P-15 (process + evidence fields; product tests still D1)
- Rubric rows (pts): DevOps 10, Security 15, AI Governance 10, Team & AI-tool 5, Presentation 5 (via demo notes on every PR)
- Tests / attack shown: none in this PR — gates require Alexis 7 SSN/403 and Trevor CI when product repo exists
- Stub/live (P-15): this scratchpad is plan only; product URL is live after Trevor 7
- Judge bar (`JUDGE.md`): never-kill intact. `/health` wording changed to match what AWS actually allows — the judged outcome (`200 {"ok":true}`, no Lambda) is unchanged.

## What I shipped

- files: Cycle gates + Rubric 100 in PLAN; 48h Stage column; START/FILL/workflows/PR template/progress require Handbook evidence
- drift fixes after auditing product PRs #26–#31 against this scratchbook:
  - Tree + `contracts/README.md`: `tenant-b-pii.json` → **`tenant-b-forbidden.json`** (the name hour 0 actually locked)
  - **HTTP + auth**: `/health` is an `HTTP_PROXY` to `health.json` on the CloudFront origin, not an API Gateway mock — HTTP API (v2) has no `MOCK` integration type. No Lambda either way. Product side: PR #26
- outputs / env **names** (no secret values): none
- tests: none (docs)

## What I need

- from whom: Alexis/Michael fill constitutions including their Handbook rows
- **Trevor decision (open):** `skills/trevor-recorder/agents/sdk.md` says "Still send the body; Alexis redacts again." The locked `span.schema.json` is `additionalProperties: false` with no raw-prompt field, so the SDK cannot send it — shipped SDK sends `prompt_hash` + masked `prompt_preview` only. Presidio at ingest then only ever sees `attributes`. Not changed here: it moves where fail-closed redaction is demonstrated, so it is your call, not an edit.
- contract / URL / header / path: product repo still Trevor hour −1

## Blocked on

nobody

## Contract reminder

Scratchpad process only. No second product. Grafana is not the UI.

## Pickup prompt (paste into the other LLM)

```
Read this handoff and PLAN.md Handbook Cycle + Rubric 100.
Do not edit the claimed paths above.
Fill your constitution with your P-ids and tests from the Handbook.
Every later PR: Handbook evidence on the handoff.
Do not merge to main — Trevor merges.
```
