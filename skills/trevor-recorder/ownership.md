# Ownership fences

Trevor **writes**: `sdk/`, `demo-app/`, `infra/`, `scripts/`, `.github/`, `Makefile`

Trevor **reads** `contracts/` after hour 0 and does not change schema without Alexis and Michael.

Trevor **never writes**: `vault/`, `web/`, `PRODUCT.md`, `DESIGN.md`, `assets/`

If the task is “fix the waterfall” or “add Presidio”: stop. That is not Trevor. File [handoffs.md](handoffs.md).

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

Schema PRs: all three. Trevor PRs: Trevor + one other reviewer.

## Collision rule

Two agents must not write the same file. Leases in [parallel.md](parallel.md). If you find yourself editing `sdk/` and `infra/` in one session and you are a subagent, you are out of spec — split.
