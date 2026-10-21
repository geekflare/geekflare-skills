# Geekflare skill for ClawHub

Official Geekflare skill for [OpenClaw](https://docs.openclaw.ai), published on [ClawHub](https://clawhub.ai). The skill lives in [`geekflare/`](geekflare/):

- [`geekflare/SKILL.md`](geekflare/SKILL.md): when to use each Geekflare tool, setup (hosted MCP server + `GEEKFLARE_API_KEY`), working rules, errors, REST fallback.
- [`geekflare/references/tools.md`](geekflare/references/tools.md): per-tool parameter reference.

Everything in `geekflare/` is published; this README and anything outside that folder is not. Published skills are licensed MIT-0 by ClawHub.

## Publish

Install the CLI (if `npm i -g` fails with EACCES, use `npx clawhub` instead):

```bash
npm i -g clawhub
```

Log in with the account that can publish for the Geekflare org on ClawHub:

```bash
clawhub login
```

Preview first:

```bash
clawhub skill publish ./geekflare --owner geekflare --slug geekflare --name "Geekflare" --version 1.0.0 --categories research,integrations --dry-run
```

Then publish by running the same command without `--dry-run`. Bump `version` in `geekflare/SKILL.md` and `--version` for each later release, and add `--changelog "..."`.

Notes:

- `--owner geekflare` assumes the ClawHub org handle is `geekflare`. Create the org (or claim the namespace: <https://docs.openclaw.ai/clawhub/namespace-claims>) before the first publish so the listing shows as official.
- The slug `geekflare` was unclaimed on ClawHub as of 2026-10-21.
- Categories mirror Apify's listing (`research`, `integrations`); adjust if the ClawHub UI offers a better fit.
- Optional: publish from a public GitHub repo and pass `--source-repo`, `--source-commit`, `--source-path geekflare` so the listing links to its source.

## Test locally before publishing

1. Set `GEEKFLARE_API_KEY` in `~/.openclaw/.env`.
2. Add the MCP server as described in `SKILL.md` and run `openclaw mcp probe geekflare`.
3. Copy `geekflare/` into your OpenClaw skills directory and try prompts such as "check the TLS certificate of example.com" or "show the Google results for 'best crm' in the UK".
