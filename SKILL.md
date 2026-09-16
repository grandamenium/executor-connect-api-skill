---
name: executor-connect-api
description: Safely connect a new API, API key, token, or OAuth service through Executor without exposing the credential to the agent. Use when a user wants Executor to add or test a service connection, including a custom API without a catalog connector.
---

# Connect an API with Executor

Create a working Executor connection while keeping the raw credential out of prompts, tool arguments, logs, files, and the agent's context.

## Discover the current Executor surface

Find the available Executor MCP tools and read their current descriptions and input schemas before acting. Identify the tools for integrations, connections, handoffs, OpenAPI, GraphQL, MCP, and—when available—toolkits or policies. Do not assume tool names, namespaces, arguments, or return shapes from this skill.

If Executor is unavailable, explain that it must be connected to the current agent. Stop instead of bypassing Executor with local secret storage.

## Build the connection

1. Clarify the service and the minimum capabilities the user needs. Treat a request to connect a named service as authorization for the narrow Executor integration and connection changes needed to do that, not for unrelated account or service mutations.
2. List or inspect existing integrations and connection metadata first. Reuse the intended objects when safe; do not create duplicates merely because names differ.
3. Classify the upstream correctly:
   - Use MCP only for an actual MCP server.
   - Use OpenAPI for a REST or HTTP API.
   - Use GraphQL only for an actual GraphQL endpoint and schema. GraphQL is not a generic wrapper for arbitrary API keys.
4. For REST APIs, prefer the provider's official OpenAPI document. If none exists or it is excessively broad, author the smallest valid specification containing only the needed server, authentication scheme, and operations. Do not import unrelated administrative, destructive, or paid operations.
5. Use Executor's current preview or inspection capability before adding a custom integration. Summarize the detected server, authentication method, and exposed operations. Resolve ambiguity before creating anything. If the user asked only for advice—or the target, ownership, or specification is materially unclear—ask before mutating Executor's catalog.
6. Add the integration using the live schema. For a key, token, or interactive account authorization, use Executor's current connection handoff flow. Give the user the handoff URL or open it when supported, then pause for them to enter the credential or complete OAuth themselves.

## Protect the secret boundary

Never ask the user to paste a key, token, password, or authorization code into chat. Never accept, retrieve, echo, transform, display, copy, or persist the raw credential. Do not put it in a shell command, environment variable, source file, example, specification, or ordinary tool argument. The agent may handle only non-secret connection metadata and the handoff URL.

If a secret is accidentally exposed, do not repeat it. Advise the user to rotate it before continuing.

## Verify safely

After the user confirms that the handoff is complete:

1. Refresh or list connection metadata and verify only the expected service, account label or owner, status, and available tool surface. Never attempt to read the stored credential.
2. Choose the lowest-impact read-only operation that proves authentication. Prefer a profile, metadata, health, or small list call with tight limits.
3. Explain the exact proposed test before any operation that can incur meaningful cost, publish, write, delete, send, purchase, or affect real people. Run such a test only after explicit user confirmation. A successful connection does not authorize service-side mutations.
4. Report what was proven and any uncertainty. Do not call a connection verified merely because it exists in metadata when no authenticated operation succeeded.

## Scope the result

Recommend a least-privilege toolkit or policy after verification: include only the required connection and operations, allow safe reads, require approval for necessary writes, and block or omit dangerous and irrelevant operations.

Explain the boundaries accurately:

- A toolkit endpoint enforces its selected tool and connection subset on the Executor side.
- Project or folder configuration controls where an MCP client loads that endpoint. It is useful scoping, but it is not a hard operating-system security boundary.
- A broad workspace endpoint can expose more capabilities than a toolkit endpoint. If both are configured for the same agent session, the broad endpoint defeats the narrower capability design.
- For client-specific work, avoid loading the broad workspace endpoint into that agent. Configure only the intended toolkit endpoint. For stronger isolation, use a separate Executor identity or workspace plus an operating-system, container, or equivalent execution boundary.

Do not create broader toolkits, organization-wide connections, policies, or project access unless the user requested that scope.
