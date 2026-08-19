# Handoffs

You do not implement another lane. You leave a **per-PR file** the other LLM can load.

Team protocol (collision + pickup): repo [`handoffs/README.md`](../../handoffs/README.md). Template: [`handoffs/PR.example.md`](../../handoffs/PR.example.md).

Every PR commits `handoffs/<your-name>-<id>-<slug>.md` and pastes it into the PR body. Not gitignored. Not chat-only.

Write the file **on your feature branch** before or with the code. If you need another lane changed: fill the handoff, stop. Do not implement their tree.

## Direction table

Fill rows for **your** I/O with the other two humans. After hour 0, HTTP is `contracts/http.md` — do not invent routes. UI labels = schema names from `contracts/` after hour 0.
