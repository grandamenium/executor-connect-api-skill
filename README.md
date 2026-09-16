# Connect an API with Executor

A reusable Codex skill for connecting APIs through [Executor](https://executor.sh) without asking the user to paste credentials into the agent's context.

The skill guides an agent through a conservative workflow:

1. Inspect Executor's live tools and schemas.
2. Reuse an existing integration when appropriate.
3. Choose MCP, OpenAPI, or GraphQL based on the upstream service.
4. Preview a minimal custom integration before creating it.
5. Send credential entry through Executor's human handoff flow.
6. Verify authentication with the smallest safe, read-only call.
7. Recommend a least-privilege toolkit or policy.

The skill does not contain credentials, create an Executor account, or install the Executor MCP server for you.

## Prerequisite

Connect Executor to the MCP-compatible agent where you want to use this skill. Executor can expose a broad workspace endpoint or a narrower toolkit endpoint. Prefer the toolkit endpoint when you need a limited capability surface.

See [Executor's documentation](https://executor.sh/docs) for current setup instructions.

## Install

Ask Codex to install this repository:

```text
$skill-installer install the skill from https://github.com/grandamenium/executor-connect-api-skill
```

Or install it manually as a user skill:

```bash
git clone https://github.com/grandamenium/executor-connect-api-skill \
  "$HOME/.agents/skills/executor-connect-api"
```

Codex detects newly installed skills automatically. If it does not appear, restart Codex. See OpenAI's [skill documentation](https://learn.chatgpt.com/docs/build-skills) for current locations and behavior.

## Use

Invoke it explicitly when you want an agent to add or verify an API connection:

```text
$executor-connect-api connect the Acme REST API through Executor. Use a human handoff for the API key, expose only the read-only account endpoint, and test it without showing me or yourself the key.
```

The skill can also activate automatically for requests to connect an API key, token, OAuth service, or custom API through Executor.

## Security model

This workflow is designed to keep the raw credential out of prompts, ordinary tool arguments, source files, shell history, and project `.env` files. It does not make an authorized capability harmless: an agent can still use the operations and permissions you grant it.

Use restricted upstream credentials, narrow toolkits, policies, approval gates, and spending limits where available.

Toolkit scoping has an important boundary:

- Executor enforces the subset exposed by a toolkit endpoint.
- Your MCP client decides where that endpoint is loaded.
- If the same agent also has Executor's broad workspace endpoint, it may still reach capabilities outside the toolkit.

For client-specific work, configure only the intended toolkit endpoint for that agent or project. Use separate Executor identities or workspaces and a stronger execution boundary when the risk requires hard isolation.

## Skill files

```text
.
├── SKILL.md
└── agents
    └── openai.yaml
```

`SKILL.md` is the complete workflow. `agents/openai.yaml` supplies Codex display metadata and the default invocation prompt. The remaining repository files provide installation guidance and the MIT license.

## License and status

MIT licensed. This is an independent community skill and is not affiliated with or endorsed by Executor.
