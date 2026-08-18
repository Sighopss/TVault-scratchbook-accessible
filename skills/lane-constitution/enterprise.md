# Enterprise bar

Your slice of PLAN.md’s constraint: observability must not become a data-leakage mechanism.

Write the controls **you** own. Do not copy another lane’s checklist.

- No secrets in git. Env / Secrets Manager. `.env` gitignored.
- Fail closed on your safety-critical path. Do not store or render data you cannot defend.
- Do not log or persist raw sensitive fields if PLAN says they must be masked first.
- Do not implement another human’s slice to “just make it work.”

Time-slip cuts: list them. Never-cut: list them.
