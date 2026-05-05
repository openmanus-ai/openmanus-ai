# Security and Safety

AI agent platforms combine user data, model output, external tools, and
automation. OpenManus AI should assume that model output, web content, uploaded
files, and integration responses may contain malicious or misleading
instructions.

## Threat Model

Important threat categories:

- Account takeover
- API key leakage
- Prompt injection from external content
- Unauthorized tool execution
- Cross-workspace data exposure
- Over-broad agent permissions
- Unsafe generated outputs
- Audit log tampering or omission
- Excessive data retention
- Abuse of public or shared agents

## Security Baseline

OpenManus AI should default to:

- Strong authentication and optional multi-factor authentication.
- Role-based workspace access.
- Scoped credentials per workspace and integration.
- Encrypted secret storage.
- Explicit tool allowlists.
- Human approval for high-impact actions.
- Rate limiting and abuse detection.
- Structured audit logs with secret redaction.
- Safe defaults for sharing and public links.
- Clear data retention controls.

## Credential Handling

Credentials should never be returned in full after creation. The platform
should:

- Store secrets in a dedicated secrets manager or encrypted store.
- Show only labels and last-used metadata in the UI.
- Rotate credentials without requiring agent re-creation.
- Restrict credential use to allowed tools, agents, and workspaces.
- Log credential creation, rotation, and deletion without logging secret
  values.

## Prompt-Injection Controls

Prompt injection cannot be solved by prompts alone. Controls should combine:

- Clear separation between system instructions, user requests, and retrieved
  content.
- Source attribution for retrieved documents and web content.
- Tool policies that do not rely solely on model self-restraint.
- Approval prompts for irreversible or externally visible actions.
- Output validation for structured formats.
- Blocking or escalation when external content requests secrets, tool changes,
  credential access, or policy overrides.

## Tool Risk Levels

Tools should be classified by risk:

| Level | Examples | Default control |
| --- | --- | --- |
| Low | Read-only search, local formatting, summarization | Allow with logging |
| Medium | Reading workspace files, querying private systems | Allow with scoped policy |
| High | Sending email, editing external records, publishing content | Require approval |
| Critical | Payments, credential changes, destructive deletes, shell execution | Disable by default |

The same tool can have different risk depending on destination, parameters, and
workspace policy.

## Human Approval

Approval prompts should include:

- Requested action
- Destination account or system
- Data that will be sent or modified
- Agent and run identifiers
- Risk label
- Consequence of approval

Approvals should expire and should be bound to the exact action parameters
shown to the user.

## Workspace Isolation

Workspace boundaries should apply to:

- Agent definitions
- Credentials
- Uploaded files
- Memories and retrieval indexes
- Run logs and outputs
- Audit events
- Billing and usage records

Cross-workspace sharing should be explicit and logged.

## Audit Logging

Security-relevant events should be tamper-evident where possible. Required
events include:

- Login, logout, and failed authentication attempts
- MFA enrollment and recovery changes
- Workspace role changes
- Credential lifecycle events
- Agent policy changes
- Tool invocation requests and decisions
- Approval decisions
- Public sharing changes
- Administrative exports and deletions

Logs should redact secrets and avoid storing full sensitive payloads unless
retention policy explicitly allows it.

## Safety Review Checklist

Before shipping a new feature, reviewers should ask:

- What user data does this feature read, write, or transmit?
- Can model output trigger an external action?
- What is the narrowest tool permission needed?
- Does the user see a clear approval prompt for high-impact actions?
- Are credentials scoped and hidden after creation?
- Is the behavior visible in audit logs?
- Can a malicious document, web page, or message influence this flow?
- How does the feature behave across workspaces and shared agents?
- What happens on retry, timeout, cancellation, or partial failure?

## Operational Monitoring

Operators should be able to monitor:

- Run volume and failure rates
- Tool usage by workspace and agent
- Provider latency and cost
- Authentication anomalies
- Credential errors
- Approval patterns
- Abuse signals and rate-limit events

Security monitoring should be designed from the first implementation rather
than added after production use.
