# Stack (locked)

- Python 3.12 + `uv` — `sdk/`, `demo-app/`
- Terraform >= 1.9 — `infra/`
- GitHub Actions YAML — `.github/workflows/`
- Amazon Bedrock (Claude or Nova), default region `us-east-1`
- AWS: API Gateway HTTP API, Lambda (handlers are Alexis), S3, DynamoDB, Cognito, CloudFront, WAF, KMS, Secrets Manager, CloudTrail
- Shell: GNU coreutils. No PowerShell. Prefer WSL2 for runs.
- Pins: `pydantic>=2`, `boto3`, `opentelemetry-api` (span **shape** only), `pytest`, `bandit`

## Span shape

SDK wraps Bedrock `converse` and one RAG retrieve. POST OTel-shaped JSON, not a vendor backend:

`trace_id`, `span_id`, `parent_id`, `tenant_id`, `kind` in `llm|tool|rag|http`, `gen_ai.request.model`, tokens, `cost_usd`, timestamps, status.

OTel is not the database. POST to Alexis ingest. If ingest is down, write golden JSON beside fixtures; keep HTTP behind an interface.

## Defaults

**SDK:** package `sdk/tracevault/`. `start_span` / `end_span` / `flush`. Contextvar for `trace_id` + `parent_id`. pytest golden vs schema.

**Demo:** 3–5 markdown docs, Bedrock embeddings, in-memory top-k, one tool, one `converse`. Not a product.

**Script:** `scripts/demo_pii_flight.sh` — `set -euo pipefail`, no secrets in argv.

**Terraform:** stacks `dev` and `prod`. Lambda handler stub until Alexis supplies the real one; still wire IAM, env, API route. CloudFront origin empty until Michael’s build exists.

**Makefile:** `make test`, `make demo`, `make plan`. Unix recipes.
