# Mission: trevor-demo

**Id:** `trevor-demo`  
**Write:** `demo-app/` only  
**Depends on:** `sdk/` import `tracevault` (if SDK is missing, stub the import behind a `DemoEmitter` protocol and leave a handoff).  
**Read:** `PLAN.md`, `SKILL.md`, this file, `agents/sdk.md` (do not edit sdk).

You are building the **instrumented RAG/agent** that generates a real flight. It is not a product. Judges never use it except via `scripts/demo_pii_flight.sh`. **One tool only** (`get_doc_metadata` / retrieve). No write, delete, or shell tools. If `TRACEVAULT_FAKE_BEDROCK=1`, say so in `demo-app/README.md` (handbook P-15).

## Goal

A CLI that:

1. Loads 3–5 markdown docs from `demo-app/corpus/`.
2. Embeds them with Bedrock embeddings (or a documented fake embedder if `TRACEVAULT_FAKE_BEDROCK=1` for CI).
3. Retrieves top-k for a user question.
4. Calls one dummy tool (`get_doc_metadata`).
5. Calls Bedrock `converse`.
6. Emits nested spans via the SDK: `http` (root) → `rag` → `tool` → `llm`.
7. Exits 0.

## Files to create

```
demo-app/
  pyproject.toml          # depends on ../sdk
  README.md
  src/demo_app/main.py
  src/demo_app/rag.py
  src/demo_app/bedrock.py
  corpus/01-overview.md
  corpus/02-retention.md
  corpus/03-tenancy.md
  tests/test_span_graph.py
```

## CLI

```bash
uv run python -m demo_app.main --question "..." --tenant tenant-a
```

Env: `AWS_REGION`, `BEDROCK_MODEL_ID`, `BEDROCK_EMBED_MODEL_ID`, `TRACEVAULT_INGEST_URL`, `TRACEVAULT_TENANT_KEY`, `TRACEVAULT_TENANT_ID`, `TRACEVAULT_FAKE_BEDROCK`.

`--pii` flag: prepend the question with `Contact user@example.com SSN 123-45-6789. ` and mark the llm span `sensitive=True`. This is what the demo script will pass.

## Spans

| kind | name | parent |
|---|---|---|
| http | `demo.ask` | none |
| rag | `demo.retrieve` | http |
| tool | `demo.get_doc_metadata` | http |
| llm | `demo.converse` | http |

Fill tokens/cost on the llm span when Bedrock returns usage. If fake mode: tokens `1`/`1`, cost `0`.

## Tests

`test_span_graph.py` with `TRACEVAULT_FAKE_BEDROCK=1`: after run, `.last-flight.json` or captured spans has 4 spans, one trace_id, correct parent_id. No AWS network.

## Do not

Write `scripts/` (that is `trevor-scripts`). Do not persist to S3 yourself except reading corpus from disk. Do not build a web UI. Do not log the `--pii` prompt.

## Done

- [ ] Fake-bedrock test green
- [ ] README: how to run with `--pii`
- [ ] Lease released
