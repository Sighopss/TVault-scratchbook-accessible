# Mission: trevor-sdk

**Id:** `trevor-sdk`  
**Write:** `sdk/` only  
**Read:** `PLAN.md`, `skills/trevor-recorder/SKILL.md`, this file, `span.schema.draft.json`, fixtures in this folder, `contracts/` if present.

You are implementing the Python client that **emits** TraceVault spans. You are not the vault. You are not the UI.

## Goal

A package `sdk/tracevault/` that other Trevor code imports. It starts/ends spans, nests parent/child, attaches tenant, model, tokens, cost, and POSTs a batch to ingest when `flush()` runs.

## Files to create

```
sdk/
  pyproject.toml
  README.md
  src/tracevault/
    __init__.py
    client.py      # TraceVaultClient
    span.py        # start_span / end_span
    context.py     # contextvars for trace_id, span_id, tenant_id
    schema.py      # load schema path
    redact_hint.py # optional sha256 of prompt when sensitive=True
  tests/
    test_golden_span.py
    test_nesting.py
    test_no_raw_log.py
    golden/root_span.json
```

Use `uv` and Python 3.12. Pins: `pydantic>=2`, `httpx`, `opentelemetry-api` (types/shape only — do not export OTLP).

## Behavior

```python
from tracevault import TraceVaultClient, start_span

client = TraceVaultClient.from_env()  # TRACEVAULT_INGEST_URL, TRACEVAULT_TENANT_KEY
with start_span(client, kind="llm", name="chat", model="...", sensitive=True, prompt=user_text):
    ...
client.flush()
```

- `kind` must be one of `llm|tool|rag|http`.
- `tenant_id` from env `TRACEVAULT_TENANT_ID` default `tenant-a`.
- `trace_id` generated once per client/flight (32 hex). `span_id` 16 hex.
- Timestamps UTC ISO-8601 with `Z`.
- If `sensitive=True`, set `prompt_hash` = sha256(utf-8 prompt), `prompt_preview` = masked (replace emails and `\d{3}-\d{2}-\d{4}` with `[EMAIL]` / `[SSN]`). Still send the body; Alexis redacts again. **Never** `print` or log the raw prompt.
- If ingest URL unset: write the JSON list to `sdk/.last-flight.json` and return. Do not crash.
- HTTP: `POST {INGEST}/v1/traces` JSON `{ "spans": [ ... ] }` header `X-Tenant-Key`. Timeouts 5s. On 4xx/5xx raise a typed error after flush retry once.
- Cost: `cost_usd` float; if unknown use `0.0`. Token fields integers.

Validate every span against `contracts/span.schema.json` if present, else `skills/trevor-recorder/span.schema.draft.json`.

## Tests (must exist and pass)

`test_golden_span.py`: build a span matching `fixtures/tenant-a-root-span.json` shape (ids may differ, required keys must exist).

`test_nesting.py`: parent http span, child rag, child llm; `parent_id` of children equals parent `span_id`; same `trace_id`.

`test_no_raw_log.py`: `sensitive=True` with prompt `reach me at user@example.com ssn 123-45-6789`. Captured logging/stdout must not contain `user@example.com` or `123-45-6789`. `prompt_preview` must not contain them either.

Do not call Bedrock in unit tests.

## Do not

Touch `demo-app/`, `infra/`, `vault/`, `web/`. Do not add LangChain. Do not implement Presidio (Alexis). Do not open ports.

## Done

- [ ] `uv run pytest sdk/tests` green
- [ ] Package importable as `tracevault`
- [ ] README: env vars only, no secrets
- [ ] Lease released
