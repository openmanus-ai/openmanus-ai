# API Design

This document outlines API conventions for OpenManus AI. It is a starting point
for implementation discussions, not a finalized contract.

## API Principles

- Use predictable resource-oriented paths.
- Require authentication for all workspace resources.
- Return stable error shapes.
- Support idempotency keys for non-read operations.
- Prefer explicit pagination over unbounded lists.
- Stream long-running agent events instead of polling where possible.
- Keep user-visible identifiers opaque and non-sequential.

## Authentication

All authenticated API requests should include a bearer token or session-backed
authorization context. Tokens should be scoped and revocable.

Administrative APIs should require elevated roles and should be logged with:

- Actor identifier
- Workspace identifier
- Request identifier
- Target resource
- Before and after summary when practical

## Resource Model

Suggested primary resources:

| Resource | Purpose |
| --- | --- |
| `users` | Human accounts and profile settings |
| `workspaces` | Collaboration and access-control boundary |
| `memberships` | Workspace roles and invitations |
| `agents` | Editable agent definitions |
| `agent_versions` | Immutable agent configurations used by runs |
| `runs` | Agent execution records |
| `run_events` | Streamed model, tool, approval, and status events |
| `tools` | Available capabilities and configured integrations |
| `credentials` | Stored provider and integration secrets |
| `approvals` | Human decisions for high-impact actions |
| `audit_events` | Administrative and security-relevant history |

## Endpoint Sketch

```text
GET    /v1/workspaces
POST   /v1/workspaces
GET    /v1/workspaces/{workspace_id}
PATCH  /v1/workspaces/{workspace_id}

GET    /v1/workspaces/{workspace_id}/agents
POST   /v1/workspaces/{workspace_id}/agents
GET    /v1/agents/{agent_id}
PATCH  /v1/agents/{agent_id}
POST   /v1/agents/{agent_id}/publish-version

POST   /v1/agents/{agent_id}/runs
GET    /v1/runs/{run_id}
GET    /v1/runs/{run_id}/events
POST   /v1/runs/{run_id}/cancel

GET    /v1/workspaces/{workspace_id}/tools
POST   /v1/workspaces/{workspace_id}/credentials
DELETE /v1/credentials/{credential_id}

GET    /v1/workspaces/{workspace_id}/audit-events
```

## Run Event Stream

Agent runs should expose an event stream that can drive both the UI and
developer integrations. Event examples:

```json
{ "type": "run.started", "run_id": "run_...", "created_at": "2025-01-01T00:00:00Z" }
{ "type": "model.output.delta", "run_id": "run_...", "text": "Drafting..." }
{ "type": "tool.requested", "run_id": "run_...", "tool": "email.send" }
{ "type": "approval.required", "run_id": "run_...", "approval_id": "appr_..." }
{ "type": "run.completed", "run_id": "run_...", "status": "succeeded" }
```

Events should be append-only. Consumers should be able to reconnect with a
cursor to resume from the last event they processed.

## Error Shape

All API errors should use a consistent response body:

```json
{
  "error": {
    "code": "workspace_not_found",
    "message": "Workspace not found.",
    "request_id": "req_...",
    "details": {}
  }
}
```

Error messages should be safe to display to users and should not reveal
secrets, internal stack traces, or authorization policy internals.

## Idempotency

State-changing requests that may be retried should accept an `Idempotency-Key`
header. Examples:

- Creating a run
- Sending a generated email
- Creating a credential record
- Publishing an agent version
- Approving a high-impact action

The server should return the original result for duplicate keys within a
defined retention window.

## Pagination

List responses should be paginated:

```json
{
  "data": [],
  "next_cursor": "cursor_..."
}
```

Cursor pagination is preferred over page-number pagination for audit events,
run events, and other append-heavy resources.

## Rate Limits

Rate limits should apply to:

- Authentication attempts
- Agent run creation
- Streaming connections
- Credential operations
- Tool invocation endpoints

Responses should include enough headers for clients to back off cleanly without
revealing sensitive quota details across tenants.

## Versioning

The initial API namespace should use `/v1`. Breaking changes should be
introduced through a new version or an explicit preview flag with migration
notes.
