# Conventions (Aider)

> **Read and follow all rules in [AGENTS.md](./AGENTS.md) — it is the central source of truth for this repository.**

This file is read by Aider to understand project conventions. All security, privacy, access,
testing, accessibility, and CI/CD rules are defined in AGENTS.md and apply here.

## Security

- NEVER hardcode secrets, API keys, tokens, or passwords.
- NEVER read .env files (.env.example is allowed).
- Validate all user input at system boundaries.
- Use parameterized queries for database operations.
- Follow OWASP Top 10 guidelines.

## Privacy

- Never log PII in application logs.
- Follow data minimization principles.
- Include consent mechanisms for data collection.

## Testing

- Run tests before every commit.
- All new code must have tests.
- Bug fixes must include regression tests.

## Code Quality

- Follow existing code patterns and conventions.
- Write self-documenting code.
- Handle errors explicitly.
- Prefer readability over cleverness.

## Dependencies

- Never add dependencies without user approval.
- Verify package names to avoid typosquatting.
- Pin exact versions.

## Accessibility

- Use semantic HTML.
- Maintain WCAG 2.1 AA compliance.
- Ensure keyboard accessibility.
