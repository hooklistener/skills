# Hooklistener CLI Commands

Captured from installed `hooklistener 1.2.0`.

## Global

```bash
hooklistener [OPTIONS] [COMMAND]
```

Global options:

- `--json`: output non-interactive command responses as JSON.
- `--log-level <trace|debug|info|warn|error>`: set log level, default `info`.
- `--log-dir <LOG_DIR>`: write logs to a custom directory.
- `--log-stdout`: also output logs to stdout.
- `--help`, `--version`.

Core commands:

- `login`
- `listen`
- `tunnel`
- `endpoint`
- `cases`
- `anon`
- `share`
- `monitor`
- `static-tunnel`
- `org`
- `config`
- `diagnostics`
- `clean-logs`
- `logout`
- `completions`
- `update`

## Auth

```bash
hooklistener login [--force] [--json]
hooklistener logout [--json]
```

Use `--force` to start a fresh device-flow login even when a token is already valid.

## Local Traffic

Open an HTTP tunnel to a local server:

```bash
hooklistener tunnel --port 3000
hooklistener tunnel --host localhost --port 8080
hooklistener tunnel --slug <static-slug> --port 3000
```

Options:

- `--port <PORT>` or `-p`, default `3000`.
- `--host <HOST>`, default `localhost`.
- `--org <ORG>` or `-o`.
- `--slug <SLUG>` or `-s`, static tunnel slug on paid plans.
- `--json`.

Forward webhooks from an existing debug endpoint slug to a local target:

```bash
hooklistener listen <endpoint-slug>
hooklistener listen <endpoint-slug> --target http://localhost:3000
```

Options:

- `--target <TARGET>` or `-t`, default `http://localhost:3000`.
- `--ws-url <WS_URL>`, defaults to production.
- `--json`.

## Endpoints And Requests

Create, list, show, or delete debug endpoints:

```bash
hooklistener endpoint create "Stripe dev" --json
hooklistener endpoint create "Stripe dev" --slug stripe-dev --json
hooklistener endpoint list --json
hooklistener endpoint show <endpoint-id> --json
hooklistener endpoint delete <endpoint-id> --json
```

Most endpoint commands accept `--org <ORG>`.

List and inspect captured requests:

```bash
hooklistener endpoint requests <endpoint-id> --page 1 --page-size 50 --json
hooklistener endpoint request <endpoint-id> <request-id> --json
hooklistener endpoint delete-request <endpoint-id> <request-id> --json
```

Replay a captured request to a target URL:

```bash
hooklistener endpoint forward-request <endpoint-id> <request-id> http://localhost:3000/webhook --json
hooklistener endpoint forward-request <endpoint-id> <request-id> http://localhost:3000/webhook --method POST --json
```

Inspect forwarding attempts:

```bash
hooklistener endpoint forwards <endpoint-id> <request-id> --json
hooklistener endpoint forward <forward-id> --json
```

## Saved Cases

Run saved cases for an endpoint:

```bash
hooklistener cases run <endpoint-id> --target cli --wait --json
hooklistener cases run <endpoint-id> --target-url http://localhost:3000/webhook --wait --timeout 60s --json
hooklistener cases run <endpoint-id> --target-id <saved-target-id> --json
```

Useful options:

- `--target <TARGET>`: target URL, saved target ID, or `cli`.
- `--target-url <TARGET_URL>`.
- `--target-id <TARGET_ID>`.
- `--target-name <TARGET_NAME>`.
- `--wait`.
- `--timeout <TIMEOUT>` such as `60`, `60s`, `2m`, `1h`.
- `--timeout-ms <TIMEOUT_MS>`.
- `--interval-ms <INTERVAL_MS>`.
- `--org <ORG>`.
- `--json`.

## Anonymous Endpoints

Anonymous endpoints do not require login. Preserve the viewer token returned by create.

```bash
hooklistener anon create --ttl 86400 --json
hooklistener anon show --token <viewer-token> <endpoint-id> --json
hooklistener anon events --token <viewer-token> <endpoint-id> --json
hooklistener anon event --token <viewer-token> <endpoint-id> <event-id> --json
```

For `anon events`, pagination options are `--page` and `--page-size`.

## Sharing Captured Requests

```bash
hooklistener share create <debug-request-id> --expires-in-hours 24 --json
hooklistener share create <debug-request-id> --include-forwards --json
hooklistener share list <debug-request-id> --json
hooklistener share show <share-token> --json
hooklistener share revoke <share-token> --json
```

`share create` options:

- `--expires-in-hours <HOURS>`.
- `--password <PASSWORD>`.
- `--include-forwards`.
- `--org <ORG>`.
- `--json`.

Avoid asking the user to paste passwords into chat. If a password is needed, tell them where to place it in the command.

## Monitors

```bash
hooklistener monitor create "API health" https://api.example.com/health --json
hooklistener monitor list --json
hooklistener monitor show <monitor-id> --json
hooklistener monitor checks <monitor-id> --json
hooklistener monitor update <monitor-id> --expected-status 204 --json
hooklistener monitor delete <monitor-id> --json
```

`monitor create` options:

- `--method <get|post|put|patch|delete|head>`, default `get`.
- `--expected-status <STATUS>`, default `200`.
- `--interval <1|5|10|30|60>`, minutes, default `5`.
- `--body-contains <TEXT>`.
- `--body <BODY>`.
- `--failure-threshold <N>`, default `2`.
- `--email`.
- `--org <ORG>`.
- `--json`.

## Static Tunnels

```bash
hooklistener static-tunnel list --json
hooklistener static-tunnel create <slug> --name "Dev tunnel" --json
hooklistener static-tunnel delete <slug-id> --json
hooklistener tunnel --slug <slug> --port 3000
```

Static tunnel commands accept `--org <ORG>` and `--json`.

## Organization And Config

```bash
hooklistener org list --json
hooklistener org use <organization-id> --json
hooklistener org clear --json
hooklistener config show --json
hooklistener config set selected_organization_id <organization-id> --json
```

Prefer `org use` over direct `config set` when setting the default organization.

## Utilities

```bash
hooklistener diagnostics
hooklistener clean-logs
hooklistener completions zsh
hooklistener completions bash
hooklistener completions fish
hooklistener update
```

`completions` supports `bash`, `zsh`, `fish`, `power-shell`, and `elvish`.
