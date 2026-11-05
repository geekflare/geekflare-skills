# Geekflare agent skill

Official [Geekflare](https://geekflare.com) skill for AI agents. It covers web search (including Google SERP), page scraping, metadata, brand data, screenshots, URL-to-PDF, and website diagnostics (uptime, DNS, TLS, redirects, broken links, load time, Lighthouse).

[![skills.sh](https://skills.sh/b/geekflare/geekflare-skills)](https://skills.sh/geekflare/geekflare-skills)

You need a [Geekflare account](https://dash.geekflare.com). Claude and Cursor connect through browser sign-in, so there is no API key to manage. Other clients use an API key from the dashboard, stored as the `GEEKFLARE_API_KEY` environment variable.

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

**Claude Code** (as a plugin; installs the skill and the hosted MCP server):

```bash
claude plugin marketplace add geekflare/geekflare-skills
claude plugin install geekflare@geekflare
```

Run `/mcp` and sign in with your Geekflare account.

**Claude (claude.ai, Claude Desktop):** go to **Customize > Connectors**, choose **Add custom connector**, enter `https://mcp.geekflare.com/mcp`, and sign in with your Geekflare account.

**Cursor:** install **Geekflare** from the [Cursor Marketplace](https://cursor.com/marketplace) under Customize. It adds the skill and the hosted Geekflare MCP server, and you sign in with your Geekflare account in the browser, so there's no API key to paste.

**Kiro** (as a power): in Kiro, open the **Powers** panel, choose **Add Custom Power** → **Import power from GitHub**, and enter `https://github.com/geekflare/geekflare-skills`. The power runs the Geekflare MCP server locally with `npx @geekflare/mcp` (Node.js 18+), so set `GEEKFLARE_API_KEY` in your environment before starting Kiro.

**OpenClaw** (via [ClawHub](https://clawhub.ai/geekflare/skills/geekflare)):

```bash
openclaw skills install @geekflare/geekflare
```

## Layout

- [`skills/geekflare/SKILL.md`](skills/geekflare/SKILL.md): when to use each tool, setup per client, working rules, errors, and the REST API.
- [`skills/geekflare/references/tools.md`](skills/geekflare/references/tools.md): per-tool parameter reference.
- [`gemini-extension.json`](gemini-extension.json): Gemini CLI extension manifest. It registers the remote MCP server and the `GEEKFLARE_API_KEY` setting, and Gemini loads the skill from `skills/`.
- [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json) and [`.mcp.json`](.mcp.json): Claude plugin manifest. Claude loads the skill from `skills/` and connects to the OAuth MCP endpoint `https://mcp.geekflare.com/mcp`.
- [`.cursor-plugin/plugin.json`](.cursor-plugin/plugin.json) and [`.cursor-plugin/mcp.json`](.cursor-plugin/mcp.json): Cursor plugin manifest. Cursor loads the skill from `skills/` and connects to the OAuth MCP endpoint `https://mcp.geekflare.com/mcp`.
- [`plugin.json`](plugin.json) and [`mcp.json`](mcp.json): [Agent Plugins](https://agent-plugins.org) manifest and MCP config, used by Kiro powers. The MCP server is `npx @geekflare/mcp`, with the key passed from `GEEKFLARE_API_KEY`.

## Data, privacy, and support

The plugins contain no executable code or hooks. They add a skill (instructions for the agent) and connect to the hosted MCP server at `mcp.geekflare.com`. The skill and MCP server send the queries, URLs, and options you ask about to Geekflare's API (`mcp.geekflare.com` / `api.geekflare.com`) under your Geekflare account, and use your account's credits. Browser sign-in goes through `dash.geekflare.com`. If no MCP server is connected and `GEEKFLARE_API_KEY` is set, the skill can call `api.geekflare.com` with `curl` using that key, and it sends the key nowhere else. Every tool is read-only. See the [Geekflare privacy policy](https://geekflare.com/privacy/).

Support: [support@geekflare.com](mailto:support@geekflare.com) or [open an issue](https://github.com/geekflare/geekflare-skills/issues).

## Maintainers: releasing

Keep `version` in `skills/geekflare/SKILL.md`, `gemini-extension.json`, `plugin.json`, `.claude-plugin/plugin.json`, `.cursor-plugin/plugin.json`, and `.claude-plugin/marketplace.json` in sync.

**skills.sh** has no publish step. Listing and ranking come from `npx skills add` install telemetry, so pushing to `main` is the release.

**Gemini CLI gallery** lists the repo automatically. It must be public, have `gemini-extension.json` at the root, and carry the `gemini-cli-extension` topic, which you can set from the repo's About settings or with `gh repo edit geekflare/geekflare-skills --add-topic gemini-cli-extension`. The gallery is crawled daily.

**ClawHub.** Log in with an account in the Geekflare org (`npx clawhub login`), then publish. Preview with `--dry-run` first:

```bash
npx clawhub skill publish ./skills/geekflare --owner geekflare --slug geekflare --name "Geekflare" --version <version> --categories research,integrations --source-repo geekflare/geekflare-skills --source-commit $(git rev-parse HEAD) --source-path skills/geekflare --changelog "<what changed>"
```

New ClawHub releases stay hidden until its security scans pass. Published skills are MIT-0 licensed.
