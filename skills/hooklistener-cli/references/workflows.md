# Hooklistener CLI Workflows

Use these recipes to translate user intent into CLI steps. Check `hooklistener <command> --help` when exact syntax matters.

## Choose CLI Or MCP

Use CLI when:

- The task touches localhost or the user's shell.
- The user needs a public tunnel URL for a local server.
- The user wants a reproducible terminal recipe.
- The user needs diagnostics, completions, updates, org selection, or config.
- The workflow needs `hooklistener listen` or `hooklistener tunnel`.

Use MCP when:

- An AI assistant should inspect Hooklistener state directly.
- The user asks the agent to create endpoints, inspect captured requests, diagnose failures, compare requests, manage monitors, or wait for events without manual shell steps.
- The task is primarily workspace state, not local process wiring.

## First-Time CLI Setup

1. Check the installed binary:

   ```bash
   hooklistener --version
   hooklistener --help
   ```

2. If missing, recommend one install path:

   ```bash
   brew tap hooklistener/tap && brew install hooklistener
   ```

   Alternatives:

   ```bash
   npm install -g hooklistener
   cargo install hooklistener-cli
   ```

3. Authenticate:

   ```bash
   hooklistener login
   ```

4. If the account has multiple organizations:

   ```bash
   hooklistener org list --json
   hooklistener org use <organization-id>
   ```

## Debug A Local Webhook Handler With A Public URL

Use this when the provider can send to any public URL and the user wants traffic forwarded to localhost.

1. Start the local app.
2. Open a tunnel:

   ```bash
   hooklistener tunnel --port 3000
   ```

3. Copy the public HTTPS URL printed by the CLI into the provider webhook settings.
4. Watch requests arrive in the terminal. Use `--json` only if the command is being scripted.
5. For a persistent paid-plan subdomain:

   ```bash
   hooklistener static-tunnel create stripe-dev --name "Stripe dev"
   hooklistener tunnel --slug stripe-dev --port 3000
   ```

## Debug An Existing Hooklistener Endpoint Locally

Use this when the user already has a Hooklistener debug endpoint slug and wants captured endpoint traffic delivered to localhost.

```bash
hooklistener listen <endpoint-slug> --target http://localhost:3000
```

Then trigger the provider webhook or replay a captured request.

## Create Endpoint And Inspect Requests

Use JSON output for agent-readable state:

```bash
hooklistener endpoint create "Stripe dev" --json
hooklistener endpoint list --json
hooklistener endpoint requests <endpoint-id> --json
hooklistener endpoint request <endpoint-id> <request-id> --json
```

Keep both endpoint ID and endpoint slug clear:

- ID is used by most `endpoint` subcommands.
- Slug is used by `hooklistener listen <endpoint-slug>`.

## Replay A Captured Request To Localhost

Use this after identifying a request ID:

```bash
hooklistener endpoint forward-request <endpoint-id> <request-id> http://localhost:3000/webhook --json
```

Optionally override method:

```bash
hooklistener endpoint forward-request <endpoint-id> <request-id> http://localhost:3000/webhook --method POST --json
```

Inspect forward attempts:

```bash
hooklistener endpoint forwards <endpoint-id> <request-id> --json
hooklistener endpoint forward <forward-id> --json
```

## Run Saved Cases Against A Local Listener

Use this when saved cases already exist for an endpoint:

```bash
hooklistener listen <endpoint-slug> --target http://localhost:3000
hooklistener cases run <endpoint-id> --target cli --wait --timeout 60s --json
```

Use `--target-url` instead when replaying directly to a URL:

```bash
hooklistener cases run <endpoint-id> --target-url http://localhost:3000/webhook --wait --json
```

## Anonymous Temporary Endpoint

Use anonymous endpoints for quick tests without login. Preserve the viewer token from creation.

```bash
hooklistener anon create --ttl 86400 --json
hooklistener anon events --token <viewer-token> <endpoint-id> --json
hooklistener anon event --token <viewer-token> <endpoint-id> <event-id> --json
```

Do not expose the viewer token in public logs or chat transcripts.

## Share Evidence

Create a short-lived public share for a captured request:

```bash
hooklistener share create <debug-request-id> --expires-in-hours 24 --include-forwards --json
```

List or revoke shares:

```bash
hooklistener share list <debug-request-id> --json
hooklistener share revoke <share-token> --json
```

Confirm before revoking. If password protection is needed, avoid asking the user to paste the password into chat.

## Monitor A URL

Create a simple health monitor:

```bash
hooklistener monitor create "API health" https://api.example.com/health --expected-status 200 --interval 5 --json
```

For POST health checks:

```bash
hooklistener monitor create "Webhook health" https://api.example.com/webhook \
  --method post \
  --expected-status 204 \
  --body '{"ping":true}' \
  --json
```

Inspect checks:

```bash
hooklistener monitor list --json
hooklistener monitor show <monitor-id> --json
hooklistener monitor checks <monitor-id> --json
```

Confirm before deleting monitors.

## Troubleshooting

- Auth failure: run `hooklistener login`.
- Wrong organization: run `hooklistener org list --json`, then `hooklistener org use <organization-id>`.
- Need reproducible output: add `--json`.
- Need debug logs: add `--log-level debug`, `--log-stdout`, or a custom `--log-dir`.
- Need support bundle: run `hooklistener diagnostics`.
- CLI syntax uncertainty: run `hooklistener <command> --help` and use that output over this static reference.
