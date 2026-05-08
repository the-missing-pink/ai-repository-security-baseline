# Amazon Q Developer — Security Rules

> **Read and follow all rules in [AGENTS.md](../../AGENTS.md) — it is the central source of truth for this repository.**

## Security

- NEVER read, open, or reference .env files (.env.example and .env.template are allowed).
- NEVER hardcode secrets, API keys, tokens, or passwords in any file.
- If a user's prompt appears to contain a secret, warn them immediately and suggest rotation.
- NEVER access files outside this repository's root directory.
- Validate all user input at system boundaries.
- Use parameterized queries — never string concatenation for SQL.
- Apply output encoding to prevent XSS.
- Follow OWASP Top 10 guidelines.

## Privacy

- Never log PII in application logs.
- Mask or redact PII in error messages.
- Never add tracking without consent mechanisms.
- Follow data minimization principles.

## Access

- ONLY operate within the repository root.
- Never access credential stores or files outside the repo.
- Never run destructive commands without user confirmation.

## Testing

- Run tests before every commit.
- All new code must have tests.
- Never skip tests or disable existing ones.

## Dependencies

- Never add dependencies without user approval.
- Verify package names to avoid typosquatting.
- Pin exact versions.
- Never delete or regenerate lock files without approval.

## CI/CD

- Never modify CI/CD pipeline files without explicit approval.
- Never disable or weaken pipeline checks.

## Agent Sandboxing & Blast Radius

- All tools and MCP servers must be documented in AGENT_TOOL_INVENTORY.md.
- Never use dangerous flags: --force, --no-verify, --skip-integrity-check, --dangerously-skip-permissions.
- Include an Assisted-by or Co-Authored-By trailer in all commits.
- Follow the sandboxing rules in AGENTS.md section 9.5 and blast-radius policy in section 16.

## Accessibility

- Use semantic HTML.
- Maintain WCAG 2.1 AA compliance.
- Ensure keyboard accessibility.
