# Geekflare agent skill

Official [Geekflare](https://geekflare.com) skill for AI agents. It covers web search (including Google SERP), page scraping, metadata, brand data, screenshots, URL-to-PDF, and website diagnostics (uptime, DNS, DNSSEC, TLS, redirects, broken links, load time, Lighthouse).

[![skills.sh](https://skills.sh/b/geekflare/geekflare-skills)](https://skills.sh/geekflare/geekflare-skills)

You need a Geekflare API key: <https://dash.geekflare.com>. Store it as the `GEEKFLARE_API_KEY` environment variable.

## Install

**Claude Code, Cursor, Codex, Copilot, Windsurf, and other agents** (via [skills.sh](https://skills.sh)):

```bash
npx skills add geekflare/geekflare-skills
```

Then connect the Geekflare MCP server in your agent. [`SKILL.md`](skills/geekflare/SKILL.md#setup) has config snippets that read the key from `GEEKFLARE_API_KEY`. Without MCP, the skill falls back to the REST API.

**Gemini CLI** (installs the skill and the MCP server, and prompts for the key):

```bash
gemini extensions install https://github.com/geekflare/geekflare-skills
```

**Kiro** (as a power): in Kiro, open the **Powers** panel, choose **Add Custom Power** → **Import power from GitHub**, and enter `https://github.com/geekflare/geekflare-skills`. The power runs the Geekflare MCP server locally with `npx @geekflare/mcp` (Node.js 18+), so set `GEEKFLARE_API_KEY` in your environment before starting Kiro.

**OpenClaw** (via [ClawHub](https://clawhub.ai/geekflare/skills/geekflare)):

```bash
openclaw skills install @geekflare/geekflare
```

## Layout

- [`skills/geekflare/SKILL.md`](skills/geekflare/SKILL.md): when to use each tool, setup per client, working rules, errors, and the REST API.
- [`skills/geekflare/references/tools.md`](skills/geekflare/references/tools.md): per-tool parameter reference.
- [`gemini-extension.json`](gemini-extension.json): Gemini CLI extension manifest. It registers the remote MCP server and the `GEEKFLARE_API_KEY` setting, and Gemini loads the skill from `skills/`.
- [`plugin.json`](plugin.json) and [`mcp.json`](mcp.json): [Agent Plugins](https://agent-plugins.org) manifest and MCP config, used by Kiro powers. The MCP server is `npx @geekflare/mcp`, with the key passed from `GEEKFLARE_API_KEY`.

## Data, privacy, and support

The skill and MCP server send the queries, URLs, and options you ask about to Geekflare's API (`mcp.geekflare.com` / `api.geekflare.com`) under your Geekflare account, and use your account's credits. See the [Geekflare privacy policy](https://geekflare.com/privacy/).

Support: [support@geekflare.com](mailto:support@geekflare.com) or [open an issue](https://github.com/geekflare/geekflare-skills/issues).

## Maintainers: releasing

Keep `version` in `skills/geekflare/SKILL.md` and `gemini-extension.json` in sync.

**skills.sh** has no publish step. Listing and ranking come from `npx skills add` install telemetry, so pushing to `main` is the release.

**Gemini CLI gallery** lists the repo automatically. It must be public, have `gemini-extension.json` at the root, and carry the `gemini-cli-extension` topic, which you can set from the repo's About settings or with `gh repo edit geekflare/geekflare-skills --add-topic gemini-cli-extension`. The gallery is crawled daily.

**ClawHub.** Log in with an account in the Geekflare org (`npx clawhub login`), then publish. Preview with `--dry-run` first:

```bash
npx clawhub skill publish ./skills/geekflare --owner geekflare --slug geekflare --name "Geekflare" --version <version> --categories research,integrations --source-repo geekflare/geekflare-skills --source-commit $(git rev-parse HEAD) --source-path skills/geekflare --changelog "<what changed>"
```

New ClawHub releases stay hidden until its security scans pass. Published skills are MIT-0 licensed.
