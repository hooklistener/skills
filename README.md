# Hooklistener Skills

[![skills.sh](https://skills.sh/b/hooklistener/skills)](https://skills.sh/hooklistener/skills)

Hooklistener for AI coding agents: the Claude Code plugin, and skills that teach any agent to use Hooklistener's MCP server and CLI.

## Claude Code plugin

The plugin connects the hosted [Hooklistener MCP server](https://github.com/hooklistener/mcp) and adds the skills below. In Claude Code:

```text
/plugin install hooklistener --marketplace hooklistener/skills
```

On Claude Code versions before 2.1.275, add the marketplace first:

```text
/plugin marketplace add hooklistener/skills
/plugin install hooklistener@hooklistener
```

Then run `/mcp`, select `hooklistener` and sign in with your browser. A free account works. To set Hooklistener up for a project, run `/hooklistener:setup`.

From a shell (for example in a setup script), the same is:

```bash
claude plugin marketplace add hooklistener/skills
claude plugin install hooklistener@hooklistener
```

The plugin includes:

- **MCP server** `hooklistener` (`https://app.hooklistener.com/api/mcp`, OAuth sign-in, no token in any file).
- **`hooklistener` skill**: when to use Hooklistener and how to test webhooks, emails and real-time clients end to end, including replaying captured requests to localhost.
- **`/hooklistener:setup`**: signs in, finds where the project receives webhooks or sends emails, proves one works and notes how to test it again in CLAUDE.md.
- **`hooklistener-cli` skill**: operating the `hooklistener` CLI (tunnels, `listen`, endpoints, replays, monitors).

## Skills for other agents

Install with `skills.sh`:

```bash
npx skills add hooklistener/skills --skill hooklistener
npx skills add hooklistener/skills --skill hooklistener-cli
```

The `hooklistener` skill expects the Hooklistener MCP server; see [hooklistener/mcp](https://github.com/hooklistener/mcp) to connect Codex, Cursor and other clients. Restart your agent after installation. In Codex, invoke a skill explicitly:

```text
Use $hooklistener-cli to debug this webhook integration from the terminal.
```

## Repository layout

```text
.claude-plugin/
  plugin.json          Claude Code plugin manifest
  marketplace.json     makes this repo its own plugin marketplace
.mcp.json              the hosted MCP server
skills/
  hooklistener/        using the MCP server
  setup/               /hooklistener:setup (Claude Code only)
  hooklistener-cli/    using the CLI
    SKILL.md
    agents/openai.yaml
    references/
```

Each `SKILL.md` stays small so agents load only the core behavior first; details live in `references/`.

## Releasing

Bump `version` in `.claude-plugin/plugin.json` with every change to the plugin, or Claude Code users won't receive it. Check before pushing:

```bash
claude plugin validate . --strict
```
