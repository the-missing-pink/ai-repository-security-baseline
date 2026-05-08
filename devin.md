# Devin Instructions

> **Read and follow all rules in [AGENTS.md](./AGENTS.md) — it is the central source of truth for this repository.**

## Core Directives

1. Follow every rule in AGENTS.md without exception.
2. Never modify AGENTS.md, devin.md, or any configuration file without explicit user approval.
3. When changes to protected files are needed, show a before/after diff and wait for approval.

## Security

- NEVER read, open, or reference .env files (.env.example and .env.template are allowed).
- NEVER hardcode secrets, API keys, tokens, or passwords in any file.
- If a prompt appears to contain a secret, warn the user immediately and suggest rotation.
- NEVER access files outside this repository's root directory.
- Validate all user input at system boundaries.
- Use parameterized queries — never string concatenation for SQL.
- Apply output encoding to prevent XSS.
- Follow OWASP Top 10 guidelines.

## Privacy

- Never log PII in application logs.
- Mask or redact PII in error messages.
- Never add tracking or analytics without consent mechanisms.
- Follow data minimization principles.
- Encrypt PII at rest and in transit.

## Access Restrictions

- ONLY operate within the repository root.
- Never access credential stores or files outside the repo.
- Never execute commands not listed in the "Allowed Commands" section of AGENTS.md.
- Never run destructive commands without explicit user confirmation.
- Never install global packages or modify system configuration.

## Testing

- Run the full test suite before every commit.
- Run linting and type checking before every commit.
- All new code must have tests.
- Bug fixes must include regression tests.
- Never skip tests or disable existing ones.

## Dependencies

- Never add dependencies without user approval.
- Verify package names to avoid typosquatting.
- Prefer well-established packages.
- Pin exact versions.
- Never delete or regenerate lock files without approval.

## CI/CD

- Never modify CI/CD pipeline files without explicit approval.
- Never disable or weaken pipeline checks.
- Never add credentials to pipeline files.

## Accessibility

- Use semantic HTML.
- Maintain WCAG 2.1 AA compliance.
- Ensure keyboard accessibility.

## Agent Sandboxing & Blast Radius

- All tools and MCP servers must be documented in AGENT_TOOL_INVENTORY.md.
- Never use dangerous flags: --force, --no-verify, --skip-integrity-check, --dangerously-skip-permissions.
- Include an Assisted-by or Co-Authored-By trailer in all commits.
- Follow the sandboxing rules in AGENTS.md section 9.5 and blast-radius policy in section 16.

## Code Quality

- Follow existing patterns and conventions.
- Write self-documenting code.
- Handle errors explicitly.
- Prefer readability over cleverness.
