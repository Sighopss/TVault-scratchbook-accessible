# Handoffs

| Direction | Contract | Other owner |
|---|---|---|
| Trevor → Alexis | POST `/v1/traces` body = `span.schema.json`; `X-Tenant-Key` or Cognito; HTTPS | Alexis ingest |
| Alexis → Trevor | Ingest base URL, key header name, 4xx shapes | Alexis |
| Trevor → Michael | Fixture-shaped JSON; later public CloudFront URL; two Cognito demo users | Michael UI |
| Michael → Trevor | `web/` output dir for CloudFront. Do not edit `web/` | Michael |

If ingest URL is unknown, SDK uses env `TRACEVAULT_INGEST_URL`. Never hardcode.

UI field names must match the schema: `trace_id`, `tenant_id`, `prompt_preview`, `cost_usd`. Trevor does not invent labels.

Judge trigger Trevor unblocks: `scripts/demo_pii_flight.sh` emits a flight whose user prompt contained email/SSN. Persistence/redaction is Alexis. Trevor still emits `tenant_id`, tokens, `cost_usd`, schema-valid body.
