# TraceVault — 3-person hackathon plan

**Team:** Trevor, Alexis, Michael  
**Product:** AI Application Flight Recorder  
**Track:** Unified AI Observability  
**Constraint:** Observability must not become a data-leakage mechanism.

This is the shareable plan. Nothing is built yet. If a task is not on the judge path, it does not get built.

**This GitHub repo is a scratchpad, not the product.** `https://github.com/Sighopss/TVault-scratchbook-accessible` holds plan, brand, and skills so humans and LLMs can read them. Officially we will **not** use it as the app repository. Do not implement, merge, or deploy `sdk/`, `demo-app/`, `vault/`, `web/`, `infra/`, or CI here.

**Product repo (separate):** Trevor creates a new GitHub repository. That is the only place application code lives. One product repo, one span contract, three packages. Feature-branch rules below apply **there**, not here.

---

## Languages and tools (locked)

| Layer | Language / runtime | Package manager | Why |
|---|---|---|---|
| Span contract | JSON Schema 2020-12 | — | All three code against the same file |
| SDK | Python 3.12 | `uv` | Same language as vault + demo |
| Demo RAG/agent | Python 3.12 | `uv` | Emits real Bedrock flights |
| Vault (ingest, redact, store, read API) | Python 3.12 on AWS Lambda | `uv` | Presidio exists here; Node does not |
| Explorer UI | TypeScript 5.8 + Next.js 15 (App Router) | `pnpm` | Michael ships a waterfall fast |
| IaC | Terraform >= 1.9 (HCL) | — | GenA11y used Terraform; all three can review |
| CI | GitHub Actions YAML | — | OIDC to AWS, no AKIA keys |
| Agent shell | GNU coreutils (`grep`, `sed`, `awk`, `find`, `head`, `tail`) | — | Stops Cursor/LLMs wasting tokens on PowerShell |

**Pinned libraries**

- Python: `pydantic>=2`, `boto3`, `presidio-analyzer`, `presidio-anonymizer`, `opentelemetry-api` (span *shape* only), `pytest`, `bandit`
- Web: Next.js 15, React 19, TypeScript strict, Playwright
- AWS: API Gateway HTTP API, Lambda, S3 (SSE-KMS), DynamoDB + TTL, Cognito, CloudFront, WAF, Secrets Manager, CloudTrail
- LLM for the demo: Amazon Bedrock (Claude or Nova). Stay on AWS.

**Hard no:** EKS/AKS, OpenSearch, Langfuse/Phoenix as the backend, Streamlit, Grafana as the product UI, X-Ray SDK (maintenance mode; ADOT only if the *platform* is traced), stock OTLP export (it will persist raw prompts).

---

## What last year’s winners did, what we steal, what we beat

| Winner | What they actually shipped | Steal (operating system) | Improve (our edge) |
|---|---|---|---|
| **HemoStat** | 4 Python agents, Redis JSON protocol, `docker compose up`, CPU-spike demo script, cooldown/audit/dry-run, `uv` + Makefile + team setup guide | One package per person. Frozen JSON contract *before* code. `scripts/demo_pii_flight.sh` as the CPU-spike equivalent. Fail-closed safety. `make quality`. | Their dashboard was Streamlit. Ours is a real Next.js Flight Recorder. Their safety was container restarts. Ours is **write-time redaction + tenant 403**. No Prometheus theater. |
| **Vulnerability-Resolution-Agent** | FastAPI ingest separate from MCP. HMAC webhook tests (`test_hmac.py`). Slack. Terraform (they pointed it at AKS). | Isolated ingest service. Security as a **runnable test** judges can watch fail. HMAC-style negative tests. | Do not copy AKS or the leftover Next.js component dump. Point Terraform at **serverless AWS**. Tests prove **PII never at rest** and **cross-tenant 403**, not just webhook signatures. |
| **InnerAI** (same observability track) | FastAPI middleware wrapping LLM calls. Metrics + dashboard + fake user. Local `fastapi dev`. CSV / `gemini_calls.log`. | SDK wraps the model call (`APIWrapper` idea). Four-module split: wrapper, vault, UI, demo user. | They logged **plaintext prompts**. We hash + mask **before S3/Dynamo**. They were localhost. We have a **public AWS URL**. They had no tenants. We demo two Cognito users. |
| **InsightAI Minions** | OTel metrics → VictoriaMetrics → Grafana. GitHub Actions redeploys the LLM under test. Slack alerts. | CI deploys the **workload being observed**, not just the platform. Use `gen_ai.*` attribute names. | They shipped KPIs (latency, tokens). We ship a **reconstructed flight** (spans, RAG hops, tools). Grafana is not our UI. |
| **GenA11yHelper** | Terraform + EC2 + Docker + Streamlit + public IP in an 8h window. S3 for prompt versions. | IaC from hour 0. Tiny scope. URL on the slide. | No EC2. No SSH `:22` open to the world. Serverless + Cognito. Prompt testing is not our product; **trace governance** is. |

**The trap:** InnerAI already occupied “middleware around the LLM.” Minions already occupied Grafana KPIs. Shipping a wrapper + dashboards with no redaction is a weaker InnerAI. Governance is the only new scoring surface on this brief.

---

## Equal split

One Flight Recorder. Challenge ideas are **views**, not three extra products.

| | Trevor — Recorder + AWS | Alexis — Vault | Michael — Explorer |
|---|---|---|---|
| Challenge ideas | Flight Recorder, agent/RAG **emit** | PII masking, prompt redaction, access-controlled logs, retention, audit, secure telemetry, tenant isolation | AI Debugger, RAG observability **view**, cost overlay, thin RCA, Agent Trace Explorer UI |
| Language | Python 3.12 + Terraform + GHA YAML | Python 3.12 | TypeScript + Next.js 15 |
| Owns in git | `sdk/`, `demo-app/`, `infra/`, `scripts/`, `.github/`, `Makefile` | `vault/` | `web/` |
| Shared | `contracts/` (all three, hour 0) | same | same |
| Unblocks | Live traces + public URL | Storage that cannot leak | The screen judges stare at |

### Trevor — exact work

1. Create a **new** product GitHub repo (not this scratchpad). Branch protection on `main` (Trevor-only merge), `CODEOWNERS`, GitHub OIDC role into one AWS account. Copy `PLAN.md` + skills into it as read-only constitution; do not keep building the app in `TVault-scratchbook-accessible`.
2. Empty CI that already fails on `gitleaks` and `trivy`.
3. `sdk/tracevault/`: `start_span` / `end_span` wrapping Bedrock `converse` and one RAG retrieve. Emits OTel-shaped JSON (`trace_id`, `span_id`, `parent_id`, `tenant_id`, `kind`: `llm|tool|rag|http`, `gen_ai.request.model`, tokens, `cost_usd`). **Does not send raw prompts** if the caller marked them sensitive; still Alexis redacts at the door.
4. `demo-app/`: 3–5 markdown docs in S3, embed, top-k retrieve, one tool call, one LLM answer. Two tenant API keys.
5. `scripts/demo_pii_flight.sh`: runs the demo with `user@example.com` and a fake SSN in the prompt. This is HemoStat’s CPU spike.
6. Terraform: API Gateway, Lambda (Alexis wires handlers), S3, DynamoDB, Cognito user pool (`viewer`/`admin`), CloudFront for `web/`, WAF, KMS, TTL=7d.
7. GitHub Actions: path-filtered tests; deploy `main` → `dev` then `prod`.
8. Keep the AWS URL alive for judging.

### Alexis — exact work

0. **Before vault code:** copy `skills/lane-constitution/` into **your own** folder (`skills/<your-lane>/`). Fill it with your paths and missions. Do not copy `trevor-recorder/`. Do not run parallel agents against a single `PLAN.md` blob.
1. `vault/ingest/`: HTTP API, Cognito/API-key auth, JSON Schema validate, enqueue.
2. `vault/redact/`: Presidio + deny-list (SSN, email, AWS keys, `sk-` tokens). Prompt body → `prompt_hash` + `prompt_preview` (masked). **Fail closed:** if redaction errors, drop the payload, do not store raw.
3. `vault/store/`: S3 SSE-KMS under `s3://…/{tenant_id}/{trace_id}/`. DynamoDB PK=`tenant_id`, SK=`trace_id`. IAM condition keys on tenant.
4. `vault/read/`: list/get spans. Cognito JWT. Cross-tenant get → **403**.
5. `vault/audit/`: every GET of a trace writes `{actor, tenant_id, trace_id, ts}`.
6. Tests (VRA `test_hmac.py` pattern): fixture with SSN must not appear in S3/Dynamo; tenant-a token cannot read tenant-b; missing auth → 401.
7. Cost fields: persist `cost_usd` and token counts Alexis does not invent — Trevor emits them, Alexis stores them redacted.

### Michael — exact work

0. **Before Next.js:** copy `skills/lane-constitution/` into **your own** folder, fill it, then Impeccable (`PRODUCT.md`). Do not copy `trevor-recorder/`. Do not open `web/` until both exist.
1. Before the weekend: Impeccable prep (section below). Do not open Next.js until `PRODUCT.md` exists.
2. `web/`: Next.js App Router. Four screens: sign-in (Cognito hosted UI), flight list, flight waterfall, audit/tenant strip.
3. Day 1 renders `contracts/fixtures/tenant-a-rag.json` with **no API**. Day 2 swaps the fetcher to Alexis’s read API.
4. Waterfall: llm / rag / tool / http spans, parent-child, latency, tokens, `$`.
5. RAG hop panel: query (masked), retrieved doc ids, scores.
6. Badges: `REDACTED`, tenant id, TTL remaining.
7. Tenant switcher. Direct ID fetch as the other user must show 403 in the UI.
8. Thin RCA: if a span `status=error`, one panel lists the failing span + siblings. Optional one Bedrock call over **already-redacted** spans. Cut this first if time slips.
9. Playwright: fixture A renders; fixture B shows masked preview not the SSN.

---

## Skills constitution (format, not Trevor’s lane)

Alexis and Michael each keep a **separate** skills folder. Do not clone `skills/trevor-recorder/`. That directory is Trevor’s filled work (his missions, paths, APIs, env). Your work is different. Copying it will leak the wrong constitution into your agents.

Copy **`skills/lane-constitution/`** instead. Placeholders only: file names, load order, leases, worktrees, one-mission-per-agent, handoffs. You fill `<your-lane>`, `<write-paths>`, missions, stack, tests. You choose how to split **your** slice. Do not split the way Trevor split his.

The point: several LLMs on one laptop without collisions, and three humans handing off without editing the wrong tree.

Live copy belongs in the **product** repo. This scratchpad is the format + Trevor’s own lane.

### File set (mandatory)

Each lane is a folder of markdown, not a vibe. Start from `skills/lane-constitution/`:

```text
skills/<your-lane>/
  SKILL.md              # constitution: product, write paths, never-write paths, agent ids
  ownership.md          # CODEOWNERS + “if the task is theirs, stop”
  parallel.md           # .agent-leases.json, worktrees, one id → one path
  stack.md              # your runtime/libs only
  enterprise.md         # your security bar only
  handoffs.md           # FROM-<you>.md the other two paste into their agents
  workflow.md           # load order, your done checklist
  agents/
    <mission>.md        # exact files, APIs, tests, bans — one agent opens exactly one
```

Mirror `SKILL.md` into `.cursor/skills/<your-lane>/`, `.claude/skills/<your-lane>/`, `.kiro/skills/<your-lane>/`. Repo-root `AGENTS.md` gets a paste block per **your** mission. Register the folder in `skills/INDEX.md`.

### Parallel on one computer

Protocol is in `skills/lane-constitution/parallel.md` (not Trevor’s filled `parallel.md`):

- One agent id, one mission file, one path set. Subagents do not implement the entire lane.
- Lease paths in **repo-root** `.agent-leases.json` (shared across all three humans). If a lease overlaps, stop. Do not delete someone else’s lease.
- Worktree per agent: `.worktrees/<id>/` on `<your-name>/<id>/<slug>`. Open a PR. **Do not merge — Trevor merges.**
- Do not copy files between worktrees. Do not `git stash` another agent’s tree.

### Parallel as a team

- **Never write another lane.** Paths are in the equal-split table above. Need something from another person → `handoffs/FROM-<you>.md` and stop.
- Handoff files are the API between humans. Paste them into the other person’s agent. Do not Slack a paragraph and hope.
- Shared hour 0 (`contracts/`) is the only tree all three touch, and only together.

Until your folder exists, you are flying blind and will collide.

---

## Repo layout

Target tree of the **product** repo (not this scratchpad):

```text
TraceVault/
  contracts/
    span.schema.json
    fixtures/
      tenant-a-rag.json
      tenant-b-pii.json
  sdk/                 # Trevor   Python 3.12
  demo-app/            # Trevor   Python 3.12
  vault/               # Alexis   Python 3.12
    ingest/
    redact/
    store/
    read/
    audit/
  web/                 # Michael  TypeScript / Next.js 15
  infra/               # Trevor   Terraform
  scripts/
    demo_pii_flight.sh
  .github/workflows/
  Makefile
  PLAN.md              # this file — team plan
  PRODUCT.md           # Impeccable — Michael, hour -1
  DESIGN.md            # Impeccable — after first UI surface
  AGENTS.md            # paste blocks for parallel agents
  skills/
    INDEX.md
    lane-constitution/ # FORMAT to copy — not Trevor’s filled lane
    trevor-recorder/   # Trevor only. Do not copy.
    <alexis-lane>/     # Alexis copies lane-constitution here and fills it
    <michael-lane>/    # Michael copies lane-constitution here and fills it
```

Hour 0 (all three, 90 minutes): lock `span.schema.json` + both fixtures. No lane code before that file exists. This is HemoStat’s `API_PROTOCOL.md`.

---

## Preparation (do this before the clock starts)

### All three — skills first, then coreutils

Alexis and Michael: copy `skills/lane-constitution/` into your own folder **this week**, before vault/Next.js. Do not copy `trevor-recorder/`. If you skip the constitution, parallel agents will overwrite each other.

### All three — coreutils and agent time

Cursor/LLMs on Windows will burn tokens on PowerShell (`Get-ChildItem`, `Select-String`). Kill that.

- GNU coreutils installed at `C:\Program Files\coreutils\bin` (already on Trevor’s machine; Alexis and Michael install the same).
- Cursor session hook prepends that path. Pre-shell hook strips aliases for `ls`, `cat`, `cp`, `mv`, `rm`.
- Prefer `grep`, `sed`, `awk`, `find`, `head`, `tail`, `wc`, `tr`, `cut`, `xargs`.
- Runs/builds/tests: **WSL2** (`/mnt/c/Users/.../TraceVault`), not PowerShell.
- Pin in CI: Python 3.12, Node 22, Terraform 1.9, `uv`, `pnpm@9`.
- Repo `Makefile` targets (`make test`, `make redact-check`, `make web`, `make demo`) so nobody pastes 20-line commands into chat.

That saves LLM time because agents stop generating broken PowerShell and start matching CI.

### Michael — Impeccable (frontend), before writing components

Impeccable is the Cursor design skill. Use it so the model does not invent a cream Inter dashboard for eight hours.

1. Confirm the Impeccable skill is loaded in Cursor.
2. `/impeccable init` → write `PRODUCT.md` (draft below). Mode for this product is **Operate** (task UI, not a marketing landing page).
3. Enable Impeccable hooks: `/impeccable hooks on` so UI edits get the detector instead of a late redesign.
4. `/impeccable shape` the four screens (list, waterfall, RAG hops, audit) **before** `create-next-app` components proliferate.
5. Brand is already decided by the TraceVault mark. Do not let the model pick a new aesthetic.
   - Background: `#000000`
   - Text: `#F8F8F8`
   - Vault / TRACE-VAULT blue: `rgb(0, 8, 248)`
   - Cyan accent (Gnosis / live / redacted-safe): `rgb(0, 248, 248)`
   - Dark operate surface. SREs, dim room. Restrained palette: neutrals + cyan accent, blue for security state.
6. Ban (craft floor): eyebrow kickers, metric-card walls, gradient text, glassmorphism, Inter-as-display, cream+serif “AI dashboard”, emoji icons, Streamlit look.
7. After the first real page exists: `/impeccable document` → `DESIGN.md` so later LLM passes reuse tokens instead of restyling.
8. Finish pass: `/impeccable harden` (empty, 403, loading, error) then `/impeccable audit`.
9. Optional live iteration: `/impeccable live` on the waterfall, not on a blank app.

Trevor and Alexis do not drive Impeccable. They consume `PRODUCT.md` so SDK field names match UI labels (`trace_id`, `tenant_id`, `prompt_preview`, `cost_usd`).

### Draft `PRODUCT.md` (paste, then `/impeccable init` can refine)

```markdown
# Product

<!-- impeccable:product-schema 1 -->

## Platform
web

## Stack
Next.js 15 App Router, TypeScript strict, Cognito hosted UI, fixture JSON until the read API exists.

## Users
On-call ML/SRE engineers reconstructing one failed or expensive AI request. They already know traces. They do not want a marketing site.

## Product Purpose
Replay what happened inside one AI request (LLM, tools, RAG hops, cost, errors) without storing raw prompts or PII.

## Positioning
A flight recorder that redacts at write time and isolates tenants. Not a Grafana clone. Not Langfuse.

## Constraints
- Raw prompts never persist.
- Cross-tenant reads return 403.
- Every trace view is audited.
- Retention via DynamoDB TTL.
- Dark operate UI using TraceVault black / blue / cyan.

## Terminology
Flight = one trace. Span kinds: llm, tool, rag, http. Vault = ingest+redact+store.
```

### Alexis — before the weekend

- AWS account access (least privilege, not root in Cursor).
- Presidio hello-world on WSL: mask one SSN, one email, one AWS key.
- Write the deny-list on paper: what is hashed, masked, dropped.
- `pytest` layout ready: `vault/tests/test_redact_pii.py`, `test_tenant_isolation.py`.

### Trevor — before the weekend

- AWS account + IAM user/role for Terraform + GitHub OIDC.
- Bedrock model access enabled in the region (us-east-1 unless told otherwise).
- Repo + `CODEOWNERS` + empty workflows.
- Confirm Alexis and Michael have coreutils + WSL + Cursor Impeccable (Michael).

---

## 48h clock

| Window | Trevor | Alexis | Michael |
|---|---|---|---|
| Hour −1 (prep) | Repo, OIDC, Bedrock enable | Presidio spike, deny-list | Impeccable init + tokens + shape |
| Hour 0 (90m, together) | Schema file in git | Redaction rules on the schema | Fixture waterfall wireframe |
| Day 1 AM | SDK + demo emits fixture-shaped JSON | Ingest + Dynamo/S3 persist | Waterfall + RAG hops on fixtures |
| Day 1 PM | Demo → API Gateway | Presidio + audit writes | Cost overlay + tenant switcher |
| Night | First Terraform apply | Isolation tests green | Empty/403/error states (`harden`) |
| Day 2 AM | Prod URL, WAF, two Cognito users | Leak tests against live S3 | Point UI at read API, Playwright |
| Day 2 PM | Keep URL alive | Judge questions on governance | Click-through |

**Kill order if time slips:** RCA panel → cost charts (keep a single `$` total) → extra span kinds. **Never kill:** redaction, tenant 403, live AWS URL, fixture-backed UI.

---

## Judge path (must work)

1. Public AWS URL. Sign in as tenant-a. One recorded RAG/agent flight: spans, hops, tokens, `$`.
2. That flight’s user prompt contained email/SSN. Stored payload is masked. Hash only. UI shows `REDACTED`.
3. Sign in as tenant-b. Same `trace_id` → 403. List does not include tenant-a.
4. Audit row: who opened the trace, when. TTL mentioned.
5. `scripts/demo_pii_flight.sh` can be re-run live.

---

## Git: feature branches, Trevor merges

**Scratchpad (`Sighopss/TVault-scratchbook-accessible`):** not the app. No product PRs, no deploys.

**Product repo (the new GitHub repo Trevor creates):** this is mandatory for humans and for parallel LLMs.

- **`main` is protected.** Nobody pushes commits onto `main`. Nobody merges to `main` except **Trevor**.
- **All work lives on feature branches.** Alexis, Michael, Trevor, and every coding agent open a branch and a pull request. Agents must not `git merge` into `main`, must not `git push origin main`, and must not click Merge.
- Branch names:
  - Humans: `alexis/<slug>`, `michael/<slug>`, `trevor/<slug>`
  - Trevor’s parallel agents: `trevor/<id>/<slug>` (`sdk`, `demo`, `scripts`, `infra`, `ci`)
- PRs target `main`. CI must be green. Schema changes still need all three people to sign off in the PR thread. **Trevor is the only merger.**
- Deploy runs only after Trevor merges to `main`. Feature branches never deploy.

---

## CODEOWNERS

```
/contracts/  @trevor @alexis @michael
/sdk/        @trevor
/demo-app/   @trevor
/infra/      @trevor
/.github/    @trevor
/vault/      @alexis
/web/        @michael
```

PRs need the owning lane plus one other reviewer. Do not merge schema changes without all three. **Only Trevor merges the PR into `main`.**

---

## CI (Trevor owns, all three must pass)

| Trigger | Job |
|---|---|
| Any PR | `gitleaks`, `trivy fs` |
| `vault/**` | `pytest` + `bandit` — SSN fixture must fail-closed |
| `web/**` | lint + Playwright against fixtures |
| `sdk/**` `demo-app/**` | golden span matches `span.schema.json` |
| `infra/**` | `terraform plan` |
| Merge to `main` | apply `dev`, then `prod` |

No deploy from feature branches. No CodePipeline. No per-PR AWS stacks. No agent or teammate merge to `main` — Trevor only.

---

## Brand assets

Open this file as markdown. Marks live in `assets/`.

![TraceVault enterprise mark](assets/Hackathon_Trace%20Vault_1.1_Summer%202026_enterprise.png)

| File | What it is |
|---|---|
| `assets/Hackathon_Trace Vault_1.1_Summer 2026.jpg` | Original submitted mark |
| `assets/tracevault-approved-source.png` | Approved source |
| `assets/Hackathon_Trace Vault_1.1_Summer 2026_enterprise.png` | Production PNG |
| `assets/Hackathon_Trace Vault_1.1_Summer 2026_enterprise.jpg` | Production JPG |
| `assets/Hackathon_Trace Vault_1.1_Summer 2026_enterprise.pdf` | Production PDF |

