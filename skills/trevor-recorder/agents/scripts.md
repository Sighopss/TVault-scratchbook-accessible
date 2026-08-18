# Mission: trevor-scripts

**Id:** `trevor-scripts`  
**Write:** `scripts/` only  
**Depends on:** `demo-app` entrypoint.

You own the **judge button**. One command must reproduce the PII flight.

## Files to create

```
scripts/demo_pii_flight.sh
scripts/check_unix.sh
```

## `demo_pii_flight.sh`

```bash
#!/usr/bin/env bash
set -euo pipefail
# cd repo root (script may be invoked from anywhere)
# require python/uv
# export TRACEVAULT_TENANT_ID=tenant-a
# run: uv run python -m demo_app.main --pii --tenant tenant-a --question "What is retention?"
# print "flight emitted" and ingest URL (not keys)
```

- Unix only. No PowerShell. No secrets in argv or echo.
- `chmod +x` in git (`git update-index --chmod=+x` if needed).
- If demo-app missing, exit 2 with a one-line message naming `trevor-demo`.

## `check_unix.sh`

Sanity: `command -v grep sed awk` exist. Used by CI later; keep it 10 lines.

## Do not

Rewrite the demo. Do not curl the vault with raw PII as a substitute for the demo. Do not embed AWS keys.

## Done

- [ ] Script exists, `set -euo pipefail`, `--pii` path
- [ ] Lease released
