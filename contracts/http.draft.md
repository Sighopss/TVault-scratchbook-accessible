# HTTP + auth contract (draft)

Hour 0: copy this file to the **product** repo as `contracts/http.md` and lock it with the span schema. HemoStat shipped `API_PROTOCOL.md` before agents coded. We do the same for HTTP, not only spans.

Alexis and Michael: put these routes in **your** missions. Do not invent a second API. Do not copy Trevor’s Python.

## 48h SaaS bar

Public HTTPS. Two tenants. Cognito sign-in. Write + read APIs. PII never at rest. Secrets not in git. Deploy from `main` only. UI and API both live. `GET /health` 200. 5xx alarm exists. Redeploy = re-run last green `deploy.yml`.

## Not in 48h (explicit)

Custom domain/ACM, multi-region, PITR, CloudTrail-as-SIEM, billing, SOC2, pager/SNS, RCA via Bedrock.

## Auth (pick is locked)

| Surface | Auth | Who |
|---|---|---|
| `POST /v1/traces` | `X-Tenant-Key` only. Key → `tenant-a` or `tenant-b` via Secrets Manager. | Trevor provisions secrets + API. Alexis validates and maps key → `tenant_id`. |
| `GET /v1/traces*` | `Authorization: Bearer <Cognito access token>` | Trevor: pool, app client, hosted UI domain, callback = CloudFront URL, custom attribute `custom:tenant_id`. Alexis: JWT + `custom:tenant_id` must match stored tenant. Mismatch → **403** (judge path, not 404). |
| `GET /health` | none | Trevor: API Gateway mock. No Lambda. |

Ingest is **not** Cognito. One auth mode per surface so agents do not invent dual stacks.

Cognito users `tenant-a` and `tenant-b` each have `custom:tenant_id` equal to their username. Password via `TF_VAR_*`, not git. Force-change-on-first-login off for the demo (judges must sign in once).

## Error JSON (all vault JSON errors)

```json
{ "error": { "code": "unauthorized", "message": "auth required" } }
```

`code` is one of: `unauthorized` (401), `forbidden` (403), `not_found` (404), `invalid` (400), `redaction_failed` (400, fail-closed, nothing stored). `message` never contains PII, prompts, or keys.

## Routes

| Method | Path | Auth | Owner (code) | Owner (AWS) | Success |
|---|---|---|---|---|---|
| `GET` | `/health` | none | — | Trevor | `200 {"ok":true}` |
| `POST` | `/v1/traces` | tenant key | Alexis ingest+redact+store | Trevor HTTP API + `vault-ingest` Lambda | `202 {"accepted":true,"trace_id":"<id>"}` |
| `GET` | `/v1/traces?limit=50` | JWT | Alexis read | Trevor HTTP API + `vault-read` Lambda | `200 {"flights":[...]}` |
| `GET` | `/v1/traces/{trace_id}` | JWT | Alexis read | same | `200 {"trace_id","tenant_id","expires_at","spans":[...]}` |
| `GET` | `/v1/traces/{trace_id}/audit` | JWT | Alexis audit | same | `200 {"events":[{"actor","tenant_id","trace_id","ts"}]}` |
| `OPTIONS` | those paths | CORS | — | Trevor | 204 |

List `flights[]` fields: `trace_id`, `tenant_id`, `start_time`, `end_time`, `cost_usd`, `status`, `prompt_preview`. List is tenant-scoped from JWT. No pagination token in 48h; `limit` max 50.

Get returns the same span objects as `contracts/span.schema.json`. Audit GET also **writes** an audit row (Alexis).

## CORS (Trevor)

API Gateway CORS: allow origin = CloudFront URL only (not `*`). Methods `GET,POST,OPTIONS`. Headers `Authorization,Content-Type,X-Tenant-Key`. Credentials off.

## Two Lambdas, five packages

Alexis still writes `vault/{ingest,redact,store,read,audit}/`. Trevor does **not** create five functions.

- `vault-ingest` handler: `vault.handlers.ingest.handler` (calls ingest → redact → store)
- `vault-read` handler: `vault.handlers.read.handler` (list/get + audit write/read)

Trevor `archive_file` from `vault/`. Alexis owns handler files. If `vault/` is missing, stubs stay empty and plan still validates.

## Web (Michael)

`output: 'export'`. No dynamic `[trace_id]` routes — use `?trace_id=`. Cognito hosted UI redirect; tokens in memory or sessionStorage, never localStorage if you can avoid it. Env:

```
NEXT_PUBLIC_API_URL
NEXT_PUBLIC_COGNITO_REGION
NEXT_PUBLIC_COGNITO_USER_POOL_ID
NEXT_PUBLIC_COGNITO_CLIENT_ID
NEXT_PUBLIC_COGNITO_DOMAIN
```

Trevor outputs those. Michael does not hardcode CloudFront or pool IDs.

## Redeploy

`deploy.yml` on `main`: `terraform apply` (dev, then prod if environment approval) → `pnpm` build in `web/` if present → `aws s3 sync` → CloudFront invalidation. Rollback = re-run the previous successful deploy workflow.
