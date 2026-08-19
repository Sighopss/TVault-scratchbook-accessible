# Handoffs

You do not implement Alexis or Michael. You leave a **per-PR file** their LLM can load.

Team protocol: repo [`handoffs/README.md`](../../handoffs/README.md). Template: [`handoffs/PR.example.md`](../../handoffs/PR.example.md).

Every PR commits `handoffs/trevor-<id>-<slug>.md` and pastes it into the PR body. **Not gitignored.** Not chat-only.

HTTP is `contracts/http.draft.md` until hour 0, then `contracts/http.md`. Do not invent routes.

If you need vault or web changed: fill the handoff, stop. Do not implement their tree.

## Direction table

| Direction | Payload | Other owner |
|---|---|---|
| Trevor → Alexis | `api_url`, table, bucket, KMS, secret ARNs, JWT issuer | two Lambdas |
| Alexis → Trevor | `vault/handlers/*.py` so zips are not empty stubs | Trevor `lambda.tf` |
| Trevor → Michael | `cloudfront_url`, `NEXT_PUBLIC_*` (api, pool, client, domain, region) | `web/` |
| Michael → Trevor | static export directory for `s3 sync` | `deploy.yml` |
| Alexis → Michael | `401/403` JSON as contracted; list/get/audit shapes | fetcher |

UI labels = schema names: `trace_id`, `tenant_id`, `prompt_preview`, `cost_usd`.
