# Hooklistener Skills

Agent skills for operating Hooklistener from AI coding assistants.

This repository contains reusable instructions that teach agents how to use Hooklistener surfaces correctly: CLI, MCP, and workflow-specific references.

## Available Skills

### `hooklistener-cli`

Use this skill when an agent needs to operate the installed `hooklistener` CLI or guide a user through terminal workflows.

It covers:

- CLI authentication with `hooklistener login`
- Local HTTP tunnels with `hooklistener tunnel`
- Endpoint forwarding with `hooklistener listen`
- Endpoint and captured request inspection
- Request replay and saved case runs
- Anonymous endpoints, request sharing, monitors, static tunnels, org/config, diagnostics, and completions
- When to use Hooklistener MCP instead of shell commands

## Install

Ask Codex to install the skill from this repository:

```text
Use $skill-installer to install https://github.com/hooklistener/skills/tree/main/hooklistener-cli
```

Restart Codex after installation, then invoke it explicitly:

```text
Use $hooklistener-cli to debug this webhook integration from the terminal.
```

## Repository Layout

```text
hooklistener-cli/
  SKILL.md
  agents/openai.yaml
  references/commands.md
  references/workflows.md
```

`SKILL.md` stays small so agents load only the core behavior first. Detailed command syntax and task recipes live in `references/`.
