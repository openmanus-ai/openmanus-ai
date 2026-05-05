# Security Policy

OpenManus AI is intended to orchestrate AI agents, user data, external tools,
and model providers. Security reports are taken seriously because weaknesses in
agent platforms can expose credentials, private data, or high-impact actions.

## Reporting a Vulnerability

Please do not open a public issue for suspected vulnerabilities.

Use the project website or the repository owner contact path to report security
issues privately. Include enough detail for maintainers to reproduce and assess
impact:

- Affected component or workflow
- Reproduction steps
- Expected and actual behavior
- Impact assessment
- Suggested remediation, if available
- Logs or screenshots with secrets removed

Maintainers should acknowledge valid reports, triage severity, and coordinate a
fix before public disclosure.

## Security Scope

Security-sensitive areas include:

- Authentication, sessions, and account recovery
- API authorization and workspace access control
- Credential storage, encryption, rotation, and display
- Agent tool execution, approvals, and file access
- Prompt-injection and indirect instruction handling
- Model provider request/response handling
- Audit logging, data retention, and privacy controls
- Administrative configuration and deployment surfaces

## Responsible Disclosure Expectations

Please avoid:

- Publicly posting exploit payloads before a fix is available
- Accessing data that does not belong to you
- Persisting, exfiltrating, or sharing secrets
- Running destructive tests against production systems
- Social engineering users, maintainers, or infrastructure providers

Good-faith testing against your own account, local deployments, or explicit test
environments is welcome when performed safely.

## Security Design Baseline

OpenManus AI should treat model output and external content as untrusted. Agent
actions should be constrained through explicit user intent, scoped tools, strong
credential isolation, and auditable policy decisions. See
[Security and safety](docs/security-and-safety.md) for the recommended platform
baseline.
