# Roadmap

This roadmap organizes OpenManus AI development into delivery phases. It should
be revised as maintainers finalize product scope, implementation choices, and
community priorities.

## Phase 1: Product Foundation

- Define the core user journeys for creating and running agents.
- Finalize the workspace, account, and role model.
- Establish project contribution and security-reporting processes.
- Document the initial architecture and API conventions.
- Choose the first supported model providers.
- Create a minimal design system for the web UI.

## Phase 2: Agent Runtime

- Implement agent configuration and immutable agent versions.
- Add run creation, run state, and event streaming.
- Integrate model provider adapters.
- Add tool-call mediation through a policy layer.
- Persist structured run events for debugging and audit.
- Support cancellation, retry, and failure recovery.

## Phase 3: Web Experience

- Build guided agent creation and editing.
- Add run monitoring, event timelines, and output review.
- Add workspace member management.
- Add credential setup and rotation flows.
- Add approval prompts for high-impact tool actions.
- Add accessibility and responsive-layout review.

## Phase 4: Tools and Integrations

- Define a stable tool interface.
- Add first-party read-only tools.
- Add controlled write-capable tools behind approval policy.
- Add webhook or plugin extension points.
- Add integration health checks.
- Document tool risk levels and operator controls.

## Phase 5: Security and Operations

- Add multi-factor authentication for privileged accounts.
- Add credential encryption and rotation.
- Add audit log export and retention settings.
- Add rate limiting and abuse detection.
- Add deployment reference architecture.
- Add backup, recovery, and incident-response runbooks.

## Phase 6: Collaboration

- Add shared workspaces and role-based access.
- Add team-visible run history.
- Add agent sharing and template workflows.
- Add review and approval delegation.
- Add organization-level policy defaults.

## Phase 7: Developer Platform

- Publish stable API documentation.
- Add SDKs or generated client examples.
- Add webhook event subscriptions.
- Add integration testing guidance.
- Add sample applications and reference agents.

## Release Readiness Checklist

Before a public release, the project should have:

- Installation and onboarding documentation
- Working authentication and workspace access control
- Clear security policy and private reporting path
- Reproducible local development workflow
- Automated tests for critical flows
- Basic observability for production operation
- Explicit data retention and privacy behavior
- Backup and recovery plan
