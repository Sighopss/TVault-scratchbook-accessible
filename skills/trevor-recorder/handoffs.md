# Handoffs

You do not implement Alexis or Michael. You leave them a file they can drop into their agent.

HTTP is `contracts/http.draft.md` until hour 0, then `contracts/http.md`. Do not invent routes.

Write `handoffs/FROM-trevor-<id>.md` (gitignored unless the human asked). Template:

```markdown
# Handoff from trevor-<id>
Date:
Blocked on: Alexis | Michael | Human

## What I shipped
- paths:
- outputs / env vars:

## What I need
- vault handlers present? (ingest.py / read.py)
- web export dir (`web/out` or whatever `next export` wrote):

## Contract reminder
See contracts/http.md
POST /v1/traces  X-Tenant-Key
GET  /v1/traces*  Authorization Bearer (Cognito)
GET  /health
```

## Direction table

| Direction | Payload | Other owner |
|---|---|---|
| Trevor → Alexis | `api_url`, table, bucket, KMS, secret ARNs, JWT issuer | two Lambdas |
| Alexis → Trevor | `vault/handlers/*.py` so zips are not empty stubs | Trevor `lambda.tf` |
| Trevor → Michael | `cloudfront_url`, `NEXT_PUBLIC_*` (api, pool, client, domain, region) | `web/` |
| Michael → Trevor | static export directory for `s3 sync` | `deploy.yml` |
| Alexis → Michael | `401/403` JSON as contracted; list/get/audit shapes | fetcher |

UI labels = schema names: `trace_id`, `tenant_id`, `prompt_preview`, `cost_usd`.
