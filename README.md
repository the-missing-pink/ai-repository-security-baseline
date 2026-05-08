# AI Repository Security Baseline

## Why this exists

AI coding agents are everywhere now. Cursor, Copilot, Claude Code, Codex, Windsurf, Cline, Junie, Devin — every developer has at least one open in their editor, and most teams have several in rotation. They're useful, fast, and frankly hard to live without once you've gotten used to them.

But they also read your code. They read your config files. They read whatever you point them at. And without a bit of guidance, they will happily peek into your `.env` file, suggest a package that doesn't actually exist (and that an attacker has helpfully published under that exact name), edit your CI pipeline to "fix" a build, or paste a database connection string into a comment because you asked it to "document this function."

None of that is the agent being malicious. They're following instructions and trying to be helpful. The problem is that the instructions usually don't tell them what *not* to do.

This repository is a starting point for fixing that.

## What it is

A drop-in set of instruction files for the most common AI agents and IDE plugins. Every file points at one shared document — `AGENTS.md` — that lays out the rules the project follows: don't touch `.env` files, don't add dependencies without asking, don't modify the CI pipeline, write tests, follow the privacy rules, and so on.

You drop the files into your repo, fill in the placeholders for your project, commit, and you're done. From that point on, whichever agent a developer happens to be using will read its own preferred config file and end up working from the same playbook as everyone else.

The goal isn't to make agents less useful. It's to take ten minutes once, set sensible defaults, and stop thinking about it.

## How to use it

The short version:

1. Copy the files from this repo into your project.
2. Open `AGENTS.md` and fill in the parts marked `[PLACEHOLDER]` (project name, tech stack, naming conventions, etc.). Most agents will offer to help you do this — they'll read your code and propose values for you to approve.
3. Delete any tool configs you don't use. If your team only uses Cursor and Copilot, there's no reason to keep the Aider or Devin files around.
4. Commit and push.

That's it. The agents will pick up the files automatically the next time someone uses them in the repo.

If you only want the bare minimum: keep `AGENTS.md` and `.aiignore`. Everything else is per-tool sugar that points back to those two.

## What's in the box

### The hub

- **`AGENTS.md`** — the source of truth. Fifteen sections covering project info, security rules, privacy rules, access restrictions, testing requirements, accessibility, CI/CD policies, dependency rules, and how agents themselves should behave. This is the file everything else points to.

### Tool-specific configs

Each of these files exists because the tool in question reads its own filename. They all delegate the actual rules to `AGENTS.md` so you don't have to maintain twelve copies of the same content.

- `CLAUDE.md` — Claude Code
- `.cursor/rules/*.mdc` — Cursor (six rule files, scoped by file type)
- `.github/copilot-instructions.md` — GitHub Copilot
- `.windsurfrules` — Windsurf
- `.clinerules` — Cline
- `.continuerules` — Continue.dev
- `.aider.conf.yml` + `CONVENTIONS.md` — Aider
- `.amazonq/rules/` — Amazon Q Developer
- `.junie/guidelines.md` — JetBrains Junie
- `devin.md` — Devin
- `.sourcegraph/cody.json` — Sourcegraph Cody
- (OpenAI Codex reads `AGENTS.md` directly, no extra file needed)

### Ignore files

These tell agents which files not to read at all — `.env`, private keys, cloud credentials, SSH keys, that kind of thing. Same idea as `.gitignore`, but for AI tools.

- `.aiignore` — the canonical list, used as a reference for the others
- `.cursorignore`, `.clineignore`, `.aiderignore` — tool-specific copies

### Security hardening

Beyond agent behavior rules, the baseline includes hardening across five areas grounded in real 2025–2026 AI-agent supply-chain breaches:

1. **CI/CD runtime** — SHA-pinned actions, least-privilege tokens, default-deny egress, split trusted/untrusted workflows, first-time contributor gating.
2. **Secret management** — OIDC/workload-identity federation, short-lived tokens, environment protection rules, credential separation.
3. **Agent sandboxing** — Tool inventory, devcontainer isolation, dangerous-flag stripping, read-only defaults for data-source tools.
4. **Code scanning** — Pre-commit hooks (gitleaks), CI-mandatory SAST (CodeQL + Semgrep), SCA (dependency-review), lockfile integrity checks, AI commit attribution auditing.
5. **Blast-radius control** — Allow/deny lists per repo, CODEOWNERS enforcement on instruction files, audit logging, documented kill-switch procedure.

See `SECURITY_CHECKLIST.md` for the step-by-step adoption guide.

The hardening files:

- `SECURITY_CHECKLIST.md` — step-by-step checklist for adopting all five pillars
- `SECURITY.md` — vulnerability reporting, kill-switch procedure, credential rotation runbook
- `CODEOWNERS` — enforces human review for instruction files, CI configs, and security-sensitive paths
- `.pre-commit-config.yaml` — local hooks for secret scanning (gitleaks) and safety checks
- `.github/workflows/ai-security-scan.yml` — CI pipeline: SAST, SCA, secret scanning, lockfile integrity
- `.github/workflows/untrusted-pr.yml` — restricted pipeline for fork and first-time contributor PRs
- `AGENT_TOOL_INVENTORY.md` — template for documenting agent tools and MCP servers
- `.devcontainer/devcontainer.json` — sandboxed development environment template

## What the rules actually cover

The instructions in `AGENTS.md` are organized around six main themes. Here's what each one is trying to prevent.

### Security

The big one. The rules tell agents:

- Don't read `.env` or any other secrets file. Ever. (`.env.example` is fine.)
- Don't hardcode API keys, tokens, passwords, or connection strings.
- If a user accidentally pastes a secret into a prompt, warn them and suggest they rotate it.
- Watch for common secret patterns (AWS keys, JWTs, private keys, database URLs) and flag them when they appear.
- Use parameterized queries, validate input, encode output. The usual OWASP Top 10 hygiene.
- Don't touch certain files (CI configs, Dockerfiles, lock files, infrastructure code, `CODEOWNERS`) without explicit permission and a before/after diff.

### Privacy

Less talked about than security, but just as important:

- Don't log personally identifiable information.
- Mask or redact PII in error messages.
- Don't add tracking, telemetry, or analytics without a real consent mechanism.
- Encrypt sensitive data at rest and in transit.
- Code should support the obvious GDPR rights — access, deletion, export.

### Access

What the agent is allowed to touch:

- Stay inside the repository. No reaching into `~/.ssh`, `~/.aws`, or the user's home directory.
- No making outbound network calls beyond what's strictly needed for the task.
- No running destructive commands (`rm -rf`, dropping databases, force-pushing) without confirmation.
- No installing global packages or modifying system config.

### Testing

A simple rule: tests run before every commit. New code gets new tests. Bug fixes get regression tests. Don't disable existing tests to make new code pass. Don't use `--no-verify` to skip the hooks.

### Accessibility

If the project has a UI, it follows WCAG 2.1 AA. Semantic HTML, keyboard navigation, proper contrast, alt text — the standard expectations, written down so the agent has them in mind when generating components.

### CI/CD and supply chain

The two areas where agents can do the most damage if they go off-script:

- Don't modify pipeline files (`.github/workflows/`, `.gitlab-ci.yml`, etc.) without explicit approval.
- Don't add dependencies without asking. When proposing one, include the version, the popularity, the license, and any known vulnerabilities.
- Verify package names — typosquatting is a real attack vector and AI agents are particularly susceptible to it because they sometimes invent package names that don't exist.
- Don't regenerate lock files. Don't suppress vulnerability warnings.

## How the rules are structured

Three things make this work:

**One source of truth.** Every tool-specific file is short and ends with "for the actual rules, read `AGENTS.md`." When you want to change something, you change it in one place. No drift.

**Defense in depth.** The rules tell agents what to do. The ignore files prevent agents from reading sensitive content even if they tried. And the rules also tell agents how to behave when they're unsure — ask, show a diff, wait for approval.

**Placeholders, not assumptions.** The boilerplate doesn't pretend to know what your project is. Things like the tech stack, naming conventions, allowed commands, and branching strategy are marked `[PLACEHOLDER]`. The first time an agent reads the file, it'll usually offer to fill those in by analyzing your repo. You approve the changes (or don't), and from that point on the file describes your project specifically.

## Customizing for your project

When you first open `AGENTS.md`, you'll see placeholder sections for things like:

- Project overview and architecture
- Languages, frameworks, and databases
- File structure
- Naming conventions
- Git workflow and branch types
- Allowed commands (build, test, lint)
- Project-specific protected files

You don't have to fill them in by hand. Open the project in your AI tool of choice and ask it to help. It'll read the codebase, propose values, show you a diff, and wait for your sign-off. The rules require it to do this rather than just rewriting things on its own.

You can also strip out anything you don't want. The accessibility rules are pointless if you're building a backend service. The CI/CD rules are pointless if you don't have CI yet. Delete what doesn't apply.

## A note on what this isn't

This is a baseline, not a fortress. It won't stop a determined attacker who has compromised your machine. It won't catch every possible secret leak. It won't make your code GDPR-compliant on its own. It won't replace a proper security review.

What it does is set sensible defaults so that the most common, most boring failure modes — the agent reading `.env`, the agent installing `requets` instead of `requests`, the agent quietly editing your GitHub Actions workflow — don't happen by accident. Most "AI security incidents" are exactly that kind of thing. This file makes them harder to do.

## Contributing

If you find a rule that's missing, a tool that should be supported, or a pattern that could be tightened, open a PR. The only real expectation is consistency: changes should land in `AGENTS.md` first, then propagate to the tool-specific files.

## License

Apache 2.0. Use it however you like.

## Further reading

If you want to go deeper on the security side:

- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [OWASP Agentic AI Threats](https://owasp.org/www-project-agentic-ai-threats/)
- [SLSA Framework](https://slsa.dev/) (supply chain integrity)
- [OpenSSF Scorecard](https://securityscorecards.dev/)
- [NIST AI Risk Management Framework](https://www.nist.gov/artificial-intelligence/risk-management-framework)
