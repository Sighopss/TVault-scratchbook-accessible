# Judge bar

**Lives only in this scratchbook.** Do not create `JUDGE.md` or `PLAN.md` in the product repo. Design and code in the product repo still **read this file** (scratchbook clone or GitHub). Source: *DevOps for GenAI Hackathon 2026* + `PLAN.md` Handbook.

If a change would break a **Never-kill**, stop.

## What judges click

1. Public AWS URL. Welcome `/`. Sign in **tenant-a**. One flight: spans, hops, tokens, `$`.
2. That flight’s prompt had email/SSN. Stored payload masked. Hash only. UI `REDACTED`.
3. Sign in **tenant-b**. Same `trace_id` → **403**. List has no tenant-a rows.
4. Audit row: who opened the trace, when. TTL mentioned.
5. `scripts/demo_pii_flight.sh` re-runs live (`tenant-a`, PII in the prompt).

## Four metrics (P-03)

1. Public URL; `GET /health` → `200 {"ok":true}`
2. One flight: waterfall + hops + tokens + `$`
3. Synthetic email/SSN in the prompt → **zero** raw PII at rest; UI `REDACTED`
4. `tenant-b` + tenant-a `trace_id` → **403**

## Never-kill

Redaction. 403 (not 404). HTTPS URL. Fixture UI (Day 1). `/health`. CORS = CloudFront only. JWT `custom:tenant_id`. One retrieve tool. Ingest key ≠ user JWT.

Welcome copy may die. Extra charts may die. **Never** kill the list above.

## P-15 — say stubs

| Piece | Say so if |
|---|---|
| Fixtures | Explorer Day 1, no live GET |
| `TRACEVAULT_FAKE_BEDROCK=1` | Not live Bedrock |
| Ingest unset | `.last-flight.json` only — not the product |

Undisclosed mocks are a handbook red flag.

## Before you claim a design or PR done

- [ ] Still can walk the click path (or you named exactly which step is blocked and on whom).
- [ ] No new agent tools. No raw prompt/PII in logs, git, or UI.
- [ ] Handoff **Handbook evidence** filled: stage, P-ids, rubric pts, tests/attack, stub/live.
- [ ] You did not add `PLAN.md` / this file / START to the product repo.

Full P-01–P-15 map, threat model, system card, Rubric 100: `PLAN.md` **Handbook (2026)**.
