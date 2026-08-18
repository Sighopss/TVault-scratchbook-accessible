# Parallel agents (one machine)

One agent = one id = one exclusive path set. Shared mutable state is `contracts/` only (read-only after hour 0).

## Identities

| Agent id | Exclusive paths | Done when |
|---|---|---|
| `trevor-sdk` | `sdk/` | Golden span validates against `contracts/span.schema.json` |
| `trevor-demo` | `demo-app/` | Demo emits one RAG+tool+LLM flight via the SDK |
| `trevor-scripts` | `scripts/` | `demo_pii_flight.sh` runs (email + fake SSN in the prompt) |
| `trevor-infra` | `infra/` | `terraform plan` for API GW, Lambda stubs, S3, DDB, Cognito, CloudFront, WAF, KMS, TTL=7d |
| `trevor-ci` | `.github/`, `Makefile` | `gitleaks` + `trivy` on every PR; path filters; OIDC; deploy `main` only |

Do not start a second agent on a path that already has a live lease.

## Leases

Gitignored file at repo root: `.agent-leases.json`

```json
{
  "leases": [
    {
      "agent": "trevor-sdk",
      "paths": ["sdk/"],
      "host": "local",
      "started": "ISO-8601"
    }
  ]
}
```

1. Overlapping path + lease younger than 4h → **do not write**. Pick a free id or stop.
2. Release the object when done.
3. Never lease `contracts/`, `vault/`, or `web/`.
4. Human Trevor session may hold `trevor-infra` + `trevor-ci` together. Subagents may not.

## Worktrees

`.worktrees/<agent-id>/` (gitignored). Branch: `trevor/<agent-id>/<slug>`. Merge via PR. No shared dirty tree.

```bash
git worktree add .worktrees/trevor-sdk -b trevor/sdk/golden-span
```

If already in a worktree, do not nest another.

## Dispatch

After hour-0 schema exists, **parallel**: `trevor-sdk`, `trevor-infra`, `trevor-ci`.

**Then**: `trevor-demo` (needs SDK import), `trevor-scripts` (needs demo entrypoint).

Integrate by merging PRs, not by copying files across worktrees.
