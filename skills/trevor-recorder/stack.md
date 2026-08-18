# Stack

Locked. Do not substitute.

| Piece | Choice |
|---|---|
| SDK / demo | Python 3.12, `uv` |
| IaC | Terraform >= 1.9, AWS provider ~> 5 |
| CI | GitHub Actions, OIDC |
| Model | Amazon Bedrock Claude or Nova, `us-east-1` |
| Shell | GNU coreutils, WSL2, no PowerShell |
| Python libs | `pydantic>=2`, `boto3`, `httpx`, `opentelemetry-api` (shape only), `pytest`, `bandit` |

Not in Trevor’s stack: Next.js (Michael), Presidio (Alexis), Grafana, Langfuse, EKS, OpenSearch, CDK.

## Span payload

POST `/v1/traces`:

```json
{ "spans": [ { "...TraceVaultSpan" : true } ] }
```

Required span keys: `trace_id`, `span_id`, `tenant_id`, `kind`, `name`, `status`, `start_time`, `end_time`.

`kind`: `llm` | `tool` | `rag` | `http`.

`tenant_id`: `tenant-a` | `tenant-b`.

Draft schema: `span.schema.draft.json`. Promote to `contracts/span.schema.json` at hour 0.

## Env

```
TRACEVAULT_INGEST_URL
TRACEVAULT_TENANT_KEY
TRACEVAULT_TENANT_ID
AWS_REGION
BEDROCK_MODEL_ID
BEDROCK_EMBED_MODEL_ID
TRACEVAULT_FAKE_BEDROCK
```

Never hardcode. Never commit values.
