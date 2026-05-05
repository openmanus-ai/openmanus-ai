# OpenManus AI Architecture

This document proposes a platform architecture for OpenManus AI. It is intended
to help contributors align on component boundaries before implementation grows
around implicit assumptions.

## Design Goals

- Make agent creation usable for non-technical users.
- Keep advanced workflows extensible for developers and teams.
- Preserve clear boundaries between users, workspaces, agents, tools, and model
  providers.
- Treat model output and external content as untrusted until policy allows an
  action.
- Provide observable, auditable execution for agent plans, tool calls, and
  administrative changes.

## High-Level Components

```text
Browser UI
  |
  v
API Gateway
  |
  +--> Authentication and account service
  +--> Workspace and billing service
  +--> Agent configuration service
  +--> Orchestration service
          |
          +--> Model provider adapter
          +--> Tool execution broker
          +--> Memory and retrieval service
          +--> Policy and approval engine
          +--> Event log and audit pipeline
```

## Component Responsibilities

### Browser UI

The web UI should provide a guided experience for:

- Creating and editing agents
- Selecting model providers and capability profiles
- Connecting tools and data sources
- Reviewing plans and approval requests
- Monitoring runs, outputs, costs, and failures
- Managing workspace members and roles

The UI should not expose raw credentials after initial entry. Sensitive actions
should use explicit confirmation flows and clear audit labels.

### API Gateway

The API gateway is the public application boundary. It should:

- Enforce authentication and authorization consistently.
- Apply rate limits and request-size limits.
- Normalize error responses.
- Attach request identifiers for tracing.
- Reject malformed input before it reaches business logic.
- Route long-running agent work into the orchestration layer.

### Authentication and Account Service

Authentication should support:

- Email or social login
- Multi-factor authentication for administrators
- Session rotation and revocation
- Organization and workspace membership
- Role-based access control

Administrative role changes, credential edits, and workspace sharing changes
should be audit logged.

### Agent Configuration Service

Agent configuration describes what an agent is allowed to do. A configuration
should include:

- Name, description, and owner workspace
- Model provider and model policy
- System instructions and style preferences
- Tool allowlist and per-tool constraints
- Approval policy for high-impact actions
- Memory and retrieval settings
- Output formatting and delivery preferences

Agent versions should be immutable once used for a run, so past behavior can be
explained during audits or debugging.

### Orchestration Service

The orchestration service coordinates agent runs. It should:

- Create plans and execution steps.
- Call models through provider adapters.
- Request tool execution through the tool broker.
- Pause for approval when policy requires it.
- Persist run state and events.
- Recover safely from worker restarts.
- Enforce workspace and agent boundaries at each step.

Long-running work should be queued. Idempotent step execution is important so
retries do not duplicate emails, payments, file writes, or external actions.

### Model Provider Adapter

Provider adapters should isolate vendor-specific request formats, streaming
events, rate limits, and error codes. The rest of the platform should depend on
a normalized interface that supports:

- Chat and responses-style generation
- Tool-call requests
- Streaming output
- Usage accounting
- Timeout and retry policy
- Provider-specific safety metadata when available

### Tool Execution Broker

The tool broker is a high-risk boundary. It should mediate all external actions:

- File access
- Web browsing
- Email and calendar operations
- Databases and business systems
- Custom webhooks
- Code or shell execution, if ever supported

Each tool call should be checked against the active agent policy, workspace
role, user approval state, and destination-specific constraints.

### Memory and Retrieval Service

Memory should be scoped and explainable. Suggested scopes:

- User-private memory
- Workspace-shared memory
- Agent-specific memory
- Run-local context

Retrieval output should be attributed to sources. Sensitive documents should
inherit workspace access controls and retention policies.

### Policy and Approval Engine

The policy engine should decide whether an action is allowed, denied, or needs
human approval. Policy inputs should include:

- Actor and workspace role
- Agent version
- Requested tool and parameters
- Destination system
- Data sensitivity
- Prior approvals in the current run
- Risk level of the action

Approval prompts should summarize the action in user language, not only raw
JSON.

### Event Log and Audit Pipeline

The platform should record structured events for:

- Login and session changes
- Agent configuration changes
- Credential and integration changes
- Run creation and completion
- Model calls and tool calls
- Approval decisions
- Delivery actions and failures

Logs should redact secrets by default and support retention controls.

## Data Flow for an Agent Run

1. A user starts a run from the UI or API.
2. The API validates the request and creates a run record.
3. The orchestrator loads the immutable agent version.
4. The orchestrator asks the model to plan or respond.
5. Tool requests go through the policy engine.
6. Low-risk allowed actions execute through the tool broker.
7. High-risk actions pause for human approval.
8. Results stream back to the UI and persist to the event log.
9. Final outputs are saved according to workspace retention policy.

## Deployment Model

A production deployment should separate:

- Public web/API tier
- Background workers
- Database
- Queue
- Object storage
- Secrets manager
- Observability stack

Workers that execute external tools should run with the smallest network,
filesystem, and credential access needed for the active task.

## Open Questions

- Which model providers are required for the first public release?
- Which tools are first-party versus plugin-based?
- What is the minimum workspace role model?
- Which actions require human approval by default?
- How long should run logs, outputs, and intermediate artifacts be retained?
