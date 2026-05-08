# Claude Code Instructions

> **Read and follow all rules in [AGENTS.md](./AGENTS.md) — it is the central source of truth for this repository.**

This file contains Claude Code-specific configuration that supplements the shared rules in AGENTS.md.

## Primary Directives

1. Follow every rule in `AGENTS.md` without exception.
2. When `AGENTS.md` and this file conflict, `AGENTS.md` takes precedence.
3. Never modify `AGENTS.md`, `CLAUDE.md`, or `.claude/settings.json` without explicit user approval with a before/after diff.

## Security

- NEVER read, open, or reference `.env` files (`.env.example` and `.env.template` are allowed).
- NEVER hardcode secrets, API keys, tokens, or passwords in any file.
- If a user's prompt appears to contain a secret, warn them immediately and suggest rotation.
- NEVER access files outside this repository's root directory.
- NEVER access `~/.ssh/`, `~/.aws/`, `~/.config/gcloud/`, or any credential store.

## File Access Restrictions

### Never read or write

- `.env`, `.env.*` (except `.env.example`, `.env.template`)
- `*.pem`, `*.key`, `*.cert`, `*.p12`, `*.pfx`, `*.jks`
- `.git/` internals
- Any file outside the repository root

### Require explicit approval before modifying

- `.github/workflows/*`
- `Dockerfile`, `docker-compose*.yml`
- `Makefile`, `Justfile`, `Taskfile.yml`
- `*.tf`, `*.tfvars`, `serverless.yml`
- `package-lock.json`, `pnpm-lock.yaml`, `yarn.lock`, `poetry.lock`, `Cargo.lock`, `go.sum`
- `CODEOWNERS`
- `.npmrc`, `.pypirc`

## Testing

- Run the full test suite before every commit.
- Run linting and type checking before every commit.
- Never skip tests or use `--no-verify`.
- Never delete or disable existing tests to make changes pass.

## Commands

Use only the commands defined in the "Allowed Commands" section of `AGENTS.md`.

## Dependencies

- NEVER add new dependencies without user approval.
- When proposing a dependency, include: name, version, purpose, popularity, license, and known vulnerabilities.
- Verify package names carefully to avoid typosquatting.

## Code Quality

- Follow existing patterns and conventions in the codebase.
- Write self-documenting code; add comments only when the "why" is non-obvious.
- Handle errors explicitly; never swallow exceptions.
- Validate all input at system boundaries.
- Use parameterized queries for database operations.

## Privacy

- Never log PII in application logs.
- Mask PII in error messages.
- Never add tracking or analytics without consent mechanisms.
- Follow the privacy rules defined in AGENTS.md section 8.

## Accessibility

- Use semantic HTML elements.
- Ensure WCAG 2.1 AA compliance for all UI changes.
- All interactive elements must be keyboard accessible.
- Follow the accessibility standards in AGENTS.md section 11.

## CI/CD

- NEVER modify CI/CD pipeline files without explicit approval and a before/after diff.
- NEVER disable, skip, or weaken pipeline checks.
- Follow the CI/CD rules in AGENTS.md section 12.

## Agent Sandboxing & Blast Radius

- All tools and MCP servers you use must be documented in `AGENT_TOOL_INVENTORY.md`.
- Never use dangerous flags: `--force`, `--no-verify`, `--skip-integrity-check`, `--dangerously-skip-permissions`.
- Include an `Assisted-by:` or `Co-Authored-By:` trailer in all commits you author.
- Follow the sandboxing rules in AGENTS.md section 9.5 and blast-radius policy in section 16.

## Placeholder Sections

If you encounter `[PLACEHOLDER]` markers in `AGENTS.md`, you may help the user fill them in:

1. Analyze the repository to gather relevant information.
2. Propose specific content with a before/after diff.
3. Only apply changes after the user explicitly approves.
