# Contributing to OpenManus AI

Thank you for helping improve OpenManus AI. This guide describes the expected
workflow for issues, design proposals, documentation, and pull requests.

## Contribution Principles

- Keep changes focused and easy to review.
- Prefer small, incremental pull requests over broad rewrites.
- Discuss major product, architecture, security, or API changes in an issue
  before implementation.
- Include tests, examples, or documentation updates when behavior changes.
- Do not include secrets, tokens, private prompts, customer data, or generated
  credentials in commits, issues, or screenshots.

## Good First Contribution Areas

- Product documentation and onboarding flows
- Architecture diagrams and design decision records
- API contracts, examples, and error-response documentation
- Security hardening checklists and threat-model notes
- Accessibility and internationalization review
- Test plans for agent workflows, tool use, and collaboration features

## Issue Workflow

Before opening a new issue:

1. Search existing issues and pull requests.
2. Confirm whether the change is a bug report, feature request, question, or
   design proposal.
3. Include enough context for maintainers to reproduce, evaluate, or scope the
   request.

For bugs, include:

- What happened
- What you expected to happen
- Reproduction steps
- Relevant logs or screenshots with secrets removed
- Browser, operating system, and deployment context when relevant

For feature proposals, include:

- User problem and target audience
- Suggested workflow
- Security or privacy considerations
- Alternatives considered
- Minimum viable version of the feature

## Pull Request Workflow

1. Fork the repository.
2. Create a descriptive branch name.
3. Make the smallest change that solves the issue.
4. Update documentation when user-facing behavior changes.
5. Run relevant tests and checks.
6. Open a pull request with a clear summary and validation notes.

Pull request descriptions should answer:

- What changed?
- Why is it needed?
- How was it tested or reviewed?
- Does it introduce new security, privacy, or operational risk?

## Security-Sensitive Changes

Security-sensitive changes need extra care. Open an issue first for hardening
ideas unless the change is a private vulnerability report. Do not publicly
disclose exploit details, secrets, bypass steps, or proof-of-concept payloads
before maintainers have triaged the issue.

Areas that require careful review include:

- Authentication and authorization
- API key storage, rotation, and display
- Agent tool permissions
- Prompt-injection controls
- File, browser, network, and shell access
- Audit logging and privacy controls
- Multi-user workspace separation

See [SECURITY.md](SECURITY.md) for private vulnerability reporting guidance.

## Documentation Style

- Write for product users first, then implementers.
- Prefer concrete examples over abstract claims.
- Explain security and privacy tradeoffs when features involve user data,
  external tools, or model outputs.
- Keep terminology consistent with the architecture and API design docs.

## License

By contributing, you agree that your contributions are licensed under the
Apache License 2.0 unless a different license is explicitly agreed by the
project maintainers.
