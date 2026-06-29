---
name: hooklistener-cli
description: >-
  Use when Codex needs to operate the installed Hooklistener CLI or teach a user how to use it: discover `hooklistener` command syntax, authenticate with device login, open local tunnels, listen to endpoint webhooks, create/list/show/delete debug endpoints, inspect or replay captured requests, run saved cases, create anonymous endpoints, share requests, manage uptime monitors/static tunnels/org/config, generate diagnostics, completions, logs, or decide when Hooklistener MCP is a better fit.
---

# Hooklistener CLI

Use the installed `hooklistener` CLI as the source of truth. Before giving exact command syntax, prefer checking the local binary:

```bash
hooklistener --help
hooklistener <command> --help
```

The command surface documented in this skill was captured from `hooklistener 1.2.0`.

## Operating Rules

- Use `--json` when an agent will parse command output.
- Use CLI for local-machine actions: `login`, `tunnel`, `listen`, diagnostics, completions, static tunnel use, and reproducible shell workflows.
- Use MCP when the user wants an AI assistant to inspect or manage Hooklistener workspace state directly without shell command orchestration.
- Do not ask users to paste access tokens, refresh tokens, OAuth tokens, or API keys. Prefer `hooklistener login` or MCP OAuth.
- Confirm before destructive actions: `endpoint delete`, `endpoint delete-request`, `monitor delete`, `share revoke`, `static-tunnel delete`, and organization/config changes.
- If a command fails because auth is missing or expired, run or recommend `hooklistener login`.

## Quick Command Map

- Authenticate: `hooklistener login`
- Open a public HTTPS tunnel to a local server: `hooklistener tunnel --port 3000`
- Forward an existing endpoint slug to localhost: `hooklistener listen <endpoint-slug> --target http://localhost:3000`
- List endpoints: `hooklistener endpoint list --json`
- Create an endpoint: `hooklistener endpoint create "Stripe dev" --json`
- List requests: `hooklistener endpoint requests <endpoint-id> --json`
- Inspect a request: `hooklistener endpoint request <endpoint-id> <request-id> --json`
- Replay a request: `hooklistener endpoint forward-request <endpoint-id> <request-id> http://localhost:3000/webhook --json`
- Run saved cases: `hooklistener cases run <endpoint-id> --target cli --wait --json`
- Create an anonymous endpoint: `hooklistener anon create --json`
- Share a request: `hooklistener share create <debug-request-id> --expires-in-hours 24 --json`
- Create a monitor: `hooklistener monitor create "API health" https://api.example.com/health --json`

## References

- Read `references/commands.md` for the installed CLI command reference.
- Read `references/workflows.md` for task recipes and CLI vs MCP decision guidance.
