# TraceVault — hackathon plan

**Team:** Trevor, Alexis, Michael  
**Product:** AI Application Flight Recorder (Unified AI Observability)  
**Rule:** Observability must not become a data-leakage mechanism. If it is not on the judge path, do not build it.

This file is the team plan. Everything below is in here on purpose.

| Section | What it is |
|---|---|
| Done-bar | 48h production SaaS yes/no |
| Judge path | What judges click |
| Winners steal | What we copy from last year |
| Stack | Languages, AWS, brand tokens |
| Three-person work | Who builds what |
| HTTP + auth | Routes, JWT, CORS, two Lambdas |
| Skills format | Parallel agents, same files, your content |
| Tree | Product repo layout |
| Git / CI | Branches, Trevor merges, checks |
| Prep | Before the clock |
| 48h table | Hour-by-hour |
| Brand | Mark files |

**This GitHub repo is a scratchpad.** `https://github.com/Sighopss/TVault-scratchbook-accessible` is plan, brand, and skills. No `sdk/`, `vault/`, `web/`, `infra/`, or deploy here.

**Product repo:** Trevor creates a new GitHub repo. That is the only app. Copy this plan + skills into it. Git/CI rules apply **there**.

Hour 0 also writes `contracts/http.md` by copying **HTTP + auth** below, and `contracts/span.schema.json` from [`skills/trevor-recorder/span.schema.draft.json`](skills/trevor-recorder/span.schema.draft.json).

---

## Done-bar

Public HTTPS. Two Cognito tenants. Sign-in. Write + read APIs. PII never at rest. Secrets not in git. UI and API both live. `GET /health` 200. 5xx alarm. Rollback = re-run last green `deploy.yml`.

**Do not build:** custom domain, multi-region, PITR, CloudTrail, billing, SOC2, pager, RCA-via-Bedrock.

Kill if time slips: RCA → extra cost charts (keep one `$`) → extra span kinds. **Never kill:** redaction, 403, HTTPS URL, fixture UI, `/health`, CORS, JWT→`custom:tenant_id`.

---

## Judge path

1. Public AWS URL. Sign in as **tenant-a**. One RAG/agent flight: spans, hops, tokens, `$`.
2. That flight’s prompt had email/SSN. Stored payload masked. Hash only. UI shows `REDACTED`.
3. Sign in as **tenant-b**. Same `trace_id` → **403**. List does not include tenant-a.
4. Audit row: who opened the trace, when. TTL mentioned.
5. `scripts/demo_pii_flight.sh` re-runs live (`tenant-a`, PII in the prompt).

---

## Winners steal

InnerAI already did “wrap the LLM and dump logs.” Minions already did Grafana KPIs. We steal their **operating system**, not their UI.

| Steal from | What we copy | What we do not copy |
|---|---|---|
| **HemoStat** | One package per person. Freeze span JSON **and** HTTP (this file’s HTTP + auth = their `API_PROTOCOL.md`) before lane code. Fail-closed. One demo script. `uv` + Makefile. | Streamlit. Prometheus theater. |
| **VRA** | Isolated ingest. Security as a test judges watch fail (PII at rest, cross-tenant 403). | AKS. Leftover Next dump. |
| **InnerAI** | SDK wraps the model call. Four-way split: emit, vault, UI, demo user. | Plaintext prompts. Localhost-only. No tenants. |
| **Minions** | CI deploys the **demo** and the **UI**, not just infra. `gen_ai.*` names. | Grafana as the product. |
| **GenA11y** | Terraform from hour 0. Public URL on the slide. Tiny scope. | EC2. SSH `:22`. |

Trap: a wrapper + dashboards with no redaction is a weaker InnerAI. Governance is the scoring surface.

---

## Stack

| Layer | Choice |
|---|---|
| Contract | JSON Schema 2020-12 + HTTP + auth (this file) |
| SDK + demo + vault | Python 3.12, `uv` |
| UI | TypeScript 5.8, Next.js 15 App Router, `pnpm`, `output: 'export'` |
| IaC | Terraform >= 1.9, AWS, `us-east-1` |
| CI | GitHub Actions, OIDC, no AKIA |
| Demo LLM | Bedrock Claude or Nova |
| Shell | GNU coreutils, WSL2, no PowerShell |

Python pins: `pydantic>=2`, `boto3`, `presidio-analyzer`, `presidio-anonymizer`, `opentelemetry-api` (shape only), `httpx`, `pytest`, `bandit`. Web: React 19, Playwright. AWS: HTTP API, two Lambdas, S3 SSE-KMS, DynamoDB TTL 7d, Cognito, CloudFront, WAF, Secrets Manager. PITR off. CloudTrail off.

**Hard no:** EKS/AKS, OpenSearch, Langfuse/Phoenix store, Streamlit, Grafana UI, X-Ray SDK, stock OTLP (persists raw prompts), SSH `:22`.

---

## Three-person work

Challenge ideas are **views**, not extra apps. One flight = one `trace_id` with spans `llm|tool|rag|http`.

| | Trevor | Alexis | Michael |
|---|---|---|---|
| Lane | Recorder + AWS | Vault | Explorer |
| Git | `sdk/` `demo-app/` `infra/` `scripts/` `.github/` `Makefile` | `vault/` | `web/` `PRODUCT.md` `DESIGN.md` |
| Language | Python + Terraform + GHA | Python 3.12 Lambda | TypeScript / Next.js 15 |
| Unblocks | Live traces + URL | Storage that cannot leak | The screen judges stare at |

Shared, hour 0 only, all three: `contracts/`. After that, that tree needs all three on the PR.

### Trevor

1. New product GitHub repo. Branch protection. OIDC. Copy plan + skills. Bootstrap Terraform remote state (S3 + lock) once.
2. CI: `gitleaks`, `trivy`, `sdk.yml`, `vault.yml`, `web.yml`, `infra.yml`, `deploy.yml` (main → apply → `web/` sync → CloudFront invalidation). Rollback = re-run last green deploy.
3. `sdk/tracevault/`: `start_span` / `end_span` around Bedrock converse + one RAG retrieve. Schema fields including tokens and `cost_usd`. `sensitive=True` hashes/masks before logs. Alexis still redacts at ingest.
4. `demo-app/`: small corpus, retrieve, one tool, one LLM answer. Two tenant keys.
5. `scripts/demo_pii_flight.sh`: `tenant-a`, email + fake SSN in the prompt.
6. Terraform per **HTTP + auth**: CORS, JWT authorizer, API key ingest, `/health` mock, two Lambdas, Cognito `custom:tenant_id`, CloudFront + CSP, WAF, KMS, secrets placeholders, logs 7d, 5xx alarm. Outputs: `api_url`, `cloudfront_url`, Cognito ids/domain for `NEXT_PUBLIC_*`.
7. Keep the URL alive.

### Alexis

0. Copy `skills/lane-constitution/` → `skills/<your-lane>/`. Write **your** `SKILL.md` and missions. Do not copy `trevor-recorder/`.
1. `vault/ingest|redact|store|read|audit/` plus `vault/handlers/ingest.py` and `read.py` (the two entrypoints Trevor zips).
2. Implement **HTTP + auth** below exactly.
3. Presidio + deny-list (SSN, email, AWS keys, `sk-`). Prompt → `prompt_hash` + masked `prompt_preview`. Persist Trevor’s `cost_usd` / tokens; do not invent them. S3 `…/{tenant_id}/{trace_id}/`. Dynamo PK `tenant_id` SK `trace_id`.
4. Tests (VRA style): SSN never in S3/Dynamo; tenant-a JWT cannot read tenant-b; missing auth → contracted `401` JSON.

### Michael

0. Same constitution copy. Then Impeccable: `/impeccable init` → `PRODUCT.md` (Operate), hooks on, `/impeccable shape` the four screens **before** components. Draft at the end of this section. Do not open `web/` until `PRODUCT.md` exists.
1. Next.js 15, `output: 'export'`. Screens: Cognito hosted UI, flight list, waterfall, audit/tenant strip. Detail via `?trace_id=` (no dynamic `[id]`).
2. Day 1: `contracts/fixtures/tenant-a-rag.json` only. Day 2: fetcher → `GET /v1/traces*` below. Env from Trevor outputs: `NEXT_PUBLIC_API_URL`, `NEXT_PUBLIC_COGNITO_*`. No hardcoded URLs.
3. Waterfall (parent-child, latency, tokens, `$`), RAG hops (masked query, doc ids, scores), badges `REDACTED` / tenant / TTL, tenant switcher, 403 from contracted error JSON.
4. Playwright: fixture A renders; fixture B hides SSN; live tenant-b 403.
5. After first real page: `/impeccable document` → `DESIGN.md`. Finish: `/impeccable harden` then `audit`.

```markdown
# Product
<!-- impeccable:product-schema 1 -->
## Platform
web
## Stack
Next.js 15 App Router, output export, Cognito hosted UI, fixtures then GET /v1/traces*.
## Users
On-call ML/SRE reconstructing one AI request. Not a marketing site.
## Product Purpose
Replay one request (LLM, tools, RAG, cost, errors) without storing raw prompts or PII.
## Positioning
Write-time redaction, tenant isolation. Not Grafana. Not Langfuse.
## Constraints
Raw prompts never persist. Cross-tenant 403. Every view audited. DynamoDB TTL. TraceVault black/blue/cyan.
## Terminology
Flight = one trace. Kinds: llm, tool, rag, http.
```

---

## HTTP + auth

Hour 0: copy this whole section into the product repo as `contracts/http.md` (also kept as [`contracts/http.draft.md`](contracts/http.draft.md)). Do not invent a second API.

### Auth

| Surface | Auth | Who |
|---|---|---|
| `POST /v1/traces` | `X-Tenant-Key` only. Key → `tenant-a` or `tenant-b` via Secrets Manager. | Trevor provisions. Alexis validates and maps key → `tenant_id`. |
| `GET /v1/traces*` | `Authorization: Bearer <Cognito access token>` | Trevor: pool, app client, hosted UI domain, callback = CloudFront URL, `custom:tenant_id`. Alexis: JWT `custom:tenant_id` must match stored tenant. Mismatch → **403** (not 404). |
| `GET /health` | none | Trevor: API Gateway mock. No Lambda. |

Ingest is **not** Cognito. Users `tenant-a` and `tenant-b` have `custom:tenant_id` = username. Passwords via `TF_VAR_*`, not git. No force-change-on-first-login (judges sign in once).

### Error JSON

```json
{ "error": { "code": "unauthorized", "message": "auth required" } }
```

`code`: `unauthorized` (401), `forbidden` (403), `not_found` (404), `invalid` (400), `redaction_failed` (400, fail-closed, nothing stored). `message` never contains PII, prompts, or keys.

### Routes

| Method | Path | Auth | Code | AWS | Success |
|---|---|---|---|---|---|
| `GET` | `/health` | none | — | Trevor mock | `200 {"ok":true}` |
| `POST` | `/v1/traces` | tenant key | Alexis ingest→redact→store | `vault-ingest` | `202 {"accepted":true,"trace_id":"<id>"}` |
| `GET` | `/v1/traces?limit=50` | JWT | Alexis read | `vault-read` | `200 {"flights":[...]}` |
| `GET` | `/v1/traces/{trace_id}` | JWT | Alexis read | `vault-read` | `200 {"trace_id","tenant_id","expires_at","spans":[...]}` |
| `GET` | `/v1/traces/{trace_id}/audit` | JWT | Alexis audit | `vault-read` | `200 {"events":[{"actor","tenant_id","trace_id","ts"}]}` |
| `OPTIONS` | those paths | CORS | — | Trevor | 204 |

List `flights[]`: `trace_id`, `tenant_id`, `start_time`, `end_time`, `cost_usd`, `status`, `prompt_preview`. Tenant-scoped from JWT. `limit` max 50. Get spans = span schema. Audit GET also **writes** a row.

### CORS (Trevor)

Allow origin = CloudFront URL only (not `*`). Methods `GET,POST,OPTIONS`. Headers `Authorization,Content-Type,X-Tenant-Key`. Credentials off.

### Two Lambdas, five packages

Alexis writes `vault/{ingest,redact,store,read,audit}/`. Trevor does **not** create five functions.

- `vault-ingest` → `vault.handlers.ingest.handler`
- `vault-read` → `vault.handlers.read.handler`

Trevor zips `vault/`. Alexis owns the handler files.

### Web env (Michael)

```
NEXT_PUBLIC_API_URL
NEXT_PUBLIC_COGNITO_REGION
NEXT_PUBLIC_COGNITO_USER_POOL_ID
NEXT_PUBLIC_COGNITO_CLIENT_ID
NEXT_PUBLIC_COGNITO_DOMAIN
```

Tokens in memory or sessionStorage. Trevor outputs these values. Michael does not hardcode URLs.

---

## Skills format

Same **format** for all three. Different **content**. That is what enforces parallel LLMs and three-person handoffs.

Copy [`skills/lane-constitution/`](skills/lane-constitution/) to `skills/<your-lane>/`. Keep every filename. Fill placeholders. Write **your** `SKILL.md` and `agents/<mission>.md`. Do **not** clone [`skills/trevor-recorder/`](skills/trevor-recorder/) (Trevor’s missions, paths, APIs).

```text
skills/<your-lane>/
  SKILL.md
  ownership.md
  parallel.md
  stack.md
  enterprise.md
  handoffs.md
  workflow.md
  agents/<mission>.md
```

Mirror `SKILL.md` to `.cursor/skills/<your-lane>/`, `.claude/skills/<your-lane>/`, `.kiro/skills/<your-lane>/`. Add paste blocks to `AGENTS.md`. Register in `skills/INDEX.md`.

**Parallel (do not rewrite):**

- One agent id, one mission file, one path set.
- Lease in repo-root `.agent-leases.json` (all lanes share it). Overlap + started < 4h ago → stop. Do not delete someone else’s lease.
- `git worktree add .worktrees/<name>-<id> -b <name>/<id>/<slug>`
- Open a PR. **Do not merge — Trevor merges.**
- Need another lane → `handoffs/FROM-<you>.md` and stop.

Paste:

```
You are <your-name>-<id>.
Read PLAN.md and skills/<your-lane>/SKILL.md.
Execute skills/<your-lane>/agents/<id>.md only.
Do not commit unless I ask. Do not merge to main — Trevor merges.
Do not edit paths PLAN.md assigns to someone else.
```

Trevor’s agent ids (his folder only): `trevor-sdk`, `trevor-demo`, `trevor-scripts`, `trevor-infra`, `trevor-ci`.

---

## Tree

```text
contracts/span.schema.json
contracts/http.md
contracts/fixtures/tenant-a-rag.json    # full flight, not one span
contracts/fixtures/tenant-b-pii.json
sdk/                 # Trevor
demo-app/            # Trevor
vault/{ingest,redact,store,read,audit,handlers}/   # Alexis
web/                 # Michael
infra/               # Trevor
scripts/demo_pii_flight.sh
.github/workflows/
Makefile
PLAN.md  PRODUCT.md  DESIGN.md  AGENTS.md
skills/INDEX.md
skills/lane-constitution/
skills/trevor-recorder/
skills/<alexis-lane>/
skills/<michael-lane>/
```

---

## Git / CI

`main` protected. Feature branch + PR. Humans: `alexis/<slug>`, `michael/<slug>`, `trevor/<slug>`. Trevor agents: `trevor/<id>/<slug>`. **Only Trevor merges.** Schema/`http.md` PRs: all three in the thread. No push to `main`. No deploy from feature branches. No CodePipeline. No per-PR stacks.

```
/contracts/  @trevor @alexis @michael
/sdk/ /demo-app/ /infra/ /scripts/ /Makefile /.github/  @trevor
/vault/      @alexis
/web/ /PRODUCT.md /DESIGN.md  @michael
```

| Trigger | Job |
|---|---|
| Any PR | gitleaks, trivy |
| `vault/**` | pytest + bandit (SSN fail-closed) |
| `web/**` | lint + Playwright |
| `sdk/**` `demo-app/**` | golden span vs schema |
| `infra/**` | terraform plan |
| `main` | apply dev (prod if approved) → sync web → invalidate |

---

## Prep

**All three:** GNU coreutils on PATH (`C:\Program Files\coreutils\bin`). WSL2 for runs. Cursor hook strips PowerShell `ls`/`cat` aliases. CI pins: Python 3.12, Node 22, Terraform 1.9, `uv`, `pnpm@9`.

**Trevor:** AWS account + OIDC role. Bedrock enabled `us-east-1`. Product repo + protection + empty CI. Remote-state bucket + lock table. Confirm Alexis/Michael have coreutils, WSL, Impeccable (Michael).

**Alexis:** Least-privilege AWS (not root in Cursor). Presidio hello-world (SSN, email, AWS key). Deny-list on paper. Skill folder filled. HTTP + auth section read.

**Michael:** Impeccable skill loaded. `PRODUCT.md` written. Skill folder filled.

**Hour 0 (90 min, together):** lock `span.schema.json` + `http.md` (copy HTTP + auth) + both **full flight** fixtures. No lane code before that.

---

## 48h table

| Window | Trevor | Alexis | Michael |
|---|---|---|---|
| −1 | Repo, OIDC, Bedrock | Presidio, deny-list, skills | Impeccable init + shape |
| 0 | Schema + http.md; callback URL | Redaction + 403 cases on contract | Fixture wireframe; `NEXT_PUBLIC_*` names |
| D1 AM | SDK + demo emit | Ingest + persist | Waterfall + hops on fixtures |
| D1 PM | CORS + two Lambdas | Presidio + audit GET | Cost + tenant switcher |
| Night | Apply: `/health`, alarm, state | Isolation tests | Harden 403/empty; export builds |
| D2 AM | URL + web sync + two users | Live S3 leak tests | Live API + Playwright 403 |
| D2 PM | URL alive; re-run deploy | Judge governance Qs | Click-through |

---

## Brand

Background `#000000`. Text `#F8F8F8`. Blue `rgb(0, 8, 248)`. Cyan `rgb(0, 248, 248)`. Impeccable **Operate**. No cream Inter dashboard, metric-card walls, glassmorphism, Streamlit look.

![TraceVault enterprise mark](assets/Hackathon_Trace%20Vault_1.1_Summer%202026_enterprise.png)

| File | What |
|---|---|
| `assets/Hackathon_Trace Vault_1.1_Summer 2026.jpg` | Original submitted mark |
| `assets/tracevault-approved-source.png` | Approved source |
| `assets/Hackathon_Trace Vault_1.1_Summer 2026_enterprise.png` | Production PNG |
| `assets/Hackathon_Trace Vault_1.1_Summer 2026_enterprise.jpg` | Production JPG |
| `assets/Hackathon_Trace Vault_1.1_Summer 2026_enterprise.pdf` | Production PDF |
