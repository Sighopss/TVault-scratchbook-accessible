# Handoffs

You do not implement Alexis or Michael. You leave them a file they can drop into their agent.

Write `handoffs/FROM-trevor-<id>.md` (gitignored or committed only if the human asked). Template:

```markdown
# Handoff from trevor-<id>
Date:
Blocked on: Alexis | Michael | Human

## What I shipped
- paths:
- outputs / env vars:

## What I need
- ingest URL:
- header name:
- web dist dir for CloudFront:

## Contract reminder
POST /v1/traces  body: { "spans": [ TraceVaultSpan, ... ] }
Header: X-Tenant-Key
```

## Direction table

| Direction | Payload | Other owner |
|---|---|---|
| Trevor → Alexis | Schema-valid spans, HTTPS, tenant key | ingest Lambda |
| Alexis → Trevor | `TRACEVAULT_INGEST_URL`, 401/403 JSON | Alexis |
| Trevor → Michael | Fixture-shaped JSON; later CloudFront URL; Cognito users | `web/` |
| Michael → Trevor | Static export path for CloudFront origin | Michael |

UI labels = schema names: `trace_id`, `tenant_id`, `prompt_preview`, `cost_usd`.
