# AI Agent Instructions

> **This file is the central source of truth for all AI coding agents, assistants, and IDE plugins working in this repository.**
> Tool-specific configuration files (CLAUDE.md, .cursorrules, copilot-instructions.md, etc.) reference this file for shared rules.

> **First-time setup:** If you are an AI agent reading this for the first time and the sections below still contain `[PLACEHOLDER]` markers, you are permitted to help the user fill them in. Ask the user for the required information, or infer it from the repository contents, then propose changes showing a clear before/after diff. **Never modify these instructions without explicit user approval.**

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Architecture](#2-architecture)
3. [Tech Stack & Languages](#3-tech-stack--languages)
4. [File Structure](#4-file-structure)
5. [Naming Conventions](#5-naming-conventions)
6. [Git Workflow](#6-git-workflow)
7. [Security Rules](#7-security-rules)
   - 7.5 [Secret Lifecycle & Credential Federation](#75-secret-lifecycle--credential-federation)
8. [Privacy Rules](#8-privacy-rules)
9. [Access Restrictions](#9-access-restrictions)
   - 9.5 [Agent Sandboxing & Tool-Surface Restrictions](#95-agent-sandboxing--tool-surface-restrictions)
10. [Testing Requirements](#10-testing-requirements)
11. [Accessibility Standards](#11-accessibility-standards)
12. [CI/CD & Pipeline Rules](#12-cicd--pipeline-rules)
    - 12.4 [CI/CD Runtime Hardening](#124-cicd-runtime-hardening)
    - 12.5 [Workflow Trust Boundaries](#125-workflow-trust-boundaries)
13. [Dependency & Supply Chain Security](#13-dependency--supply-chain-security)
14. [Code Quality Standards](#14-code-quality-standards)
15. [Agent Self-Governance](#15-agent-self-governance)
16. [Blast-Radius Policy & Monitoring](#16-blast-radius-policy--monitoring)

---

## 1. Project Overview

<!--
  INSTRUCTIONS FOR AI AGENTS:
  If this section contains [PLACEHOLDER], ask the user to describe their project
  or analyze the repository to propose a description. Show a before/after diff
  before making any changes.
-->

**Project name:** [PLACEHOLDER - e.g., "MyApp"]
**Description:** [PLACEHOLDER - Brief description of what this project does]
**Primary purpose:** [PLACEHOLDER - e.g., "SaaS platform for inventory management"]
**Target audience:** [PLACEHOLDER - e.g., "Small business owners"]
**Repository type:** [PLACEHOLDER - e.g., "Monorepo", "Single application", "Library/Package"]

---

## 2. Architecture

<!--
  INSTRUCTIONS FOR AI AGENTS:
  Analyze the codebase to infer architecture if this is still a placeholder.
  Propose your findings to the user for approval.
-->

**Architecture pattern:** [PLACEHOLDER - e.g., "Microservices", "Monolith", "Serverless", "MVC", "Event-driven"]
**Key architectural decisions:**

- [PLACEHOLDER - e.g., "Event sourcing for order processing"]
- [PLACEHOLDER - e.g., "CQRS pattern for read/write separation"]

**System diagram:** [PLACEHOLDER - Link to architecture diagram or describe key components]

**Key components:**

| Component | Purpose | Location |
|-----------|---------|----------|
| [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] |

---

## 3. Tech Stack & Languages

<!--
  INSTRUCTIONS FOR AI AGENTS:
  You may detect the tech stack from package.json, requirements.txt, go.mod,
  Cargo.toml, or similar files. Propose your findings for approval.
-->

**Primary language(s):** [PLACEHOLDER - e.g., "TypeScript, Python"]
**Framework(s):** [PLACEHOLDER - e.g., "Next.js 14, FastAPI"]
**Database(s):** [PLACEHOLDER - e.g., "PostgreSQL 16, Redis"]
**Runtime(s):** [PLACEHOLDER - e.g., "Node.js 20 LTS, Python 3.12"]
**Package manager(s):** [PLACEHOLDER - e.g., "pnpm, pip"]
**Infrastructure:** [PLACEHOLDER - e.g., "AWS (ECS, RDS, S3), Terraform"]

---

## 4. File Structure

<!--
  INSTRUCTIONS FOR AI AGENTS:
  You may scan the directory tree to propose this section. Show the user
  what you found and ask for confirmation.
-->

```
[PLACEHOLDER - Describe the key directories and their purposes, e.g.:]
/
├── src/                  # Application source code
│   ├── components/       # Reusable UI components
│   ├── services/         # Business logic
│   ├── models/           # Data models
│   └── utils/            # Utility functions
├── tests/                # Test files
├── docs/                 # Documentation
├── infra/                # Infrastructure as code
├── .github/              # GitHub workflows and config
└── scripts/              # Build and deployment scripts
```

---

## 5. Naming Conventions

<!--
  INSTRUCTIONS FOR AI AGENTS:
  You may analyze existing code to detect patterns. Propose your findings
  and ask the user to confirm or adjust.
-->

**Files:** [PLACEHOLDER - e.g., "kebab-case for files (my-component.tsx)"]
**Components/Classes:** [PLACEHOLDER - e.g., "PascalCase (MyComponent)"]
**Functions/Methods:** [PLACEHOLDER - e.g., "camelCase (getUserById)"]
**Variables:** [PLACEHOLDER - e.g., "camelCase, UPPER_SNAKE_CASE for constants"]
**Database tables:** [PLACEHOLDER - e.g., "snake_case (user_accounts)"]
**API endpoints:** [PLACEHOLDER - e.g., "kebab-case (/api/user-accounts)"]
**Branch names:** [PLACEHOLDER - e.g., "type/description (feature/add-login, fix/null-pointer)"]
**Commit messages:** [PLACEHOLDER - e.g., "Conventional Commits (feat: add login page)"]

---

## 6. Git Workflow

**Branching strategy:** GitFlow
**Main branch:** `main` (production-ready code)
**Develop branch:** `develop` (integration branch for features)
**Branch types:**

- `feature/*` — new features (branched from `develop`, merged back to `develop`)
- `bugfix/*` — non-critical bug fixes (branched from `develop`, merged back to `develop`)
- `hotfix/*` — critical production fixes (branched from `main`, merged to both `main` and `develop`)
- `release/*` — release preparation (branched from `develop`, merged to `main` and `develop`)
- `chore/*` — maintenance, tooling, dependency updates (branched from `develop`)

**Branch naming:** [PLACEHOLDER — user decides on naming convention beyond the prefix, e.g., `feature/short-description`, `feature/JIRA-123-short-description`, `feature/123-short-description`]

**Branch protection:** [PLACEHOLDER — define protection rules for `main` and `develop`]
**Merge strategy:** [PLACEHOLDER — e.g., "Merge commit (preserves GitFlow history)" or "Squash and merge"]
**Release process:** [PLACEHOLDER — e.g., "Semantic versioning, tagged releases from `main`"]

### Git rules for AI agents

- Always create a new branch for changes; never commit directly to the main branch.
- Write clear, descriptive commit messages following the project's commit convention.
- Keep commits atomic: one logical change per commit.
- Never force-push to shared branches.
- Never rewrite history on branches that others may have pulled.
- Always pull the latest changes before starting new work.

### 6.4 AI Commit Attribution

- All commits authored or co-authored by an AI agent **must** include a git trailer identifying the agent:
  ```
  Assisted-by: agent-name <agent-noreply-email>
  ```
  or the equivalent `Co-Authored-By:` trailer.
- Pre-commit hooks and CI will verify this trailer on commits from known AI agent identities.
- This enables reviewers to identify AI-generated code and apply appropriate scrutiny.
- [PLACEHOLDER — Define the list of known AI agent email addresses/identifiers for your project]

---

## 7. Security Rules

> **CRITICAL: These rules are mandatory for all AI agents, assistants, and plugins. Violations are not acceptable under any circumstances.**

### 7.1 Secrets & Credentials Protection

- **NEVER read, open, display, or include the contents of `.env` files** (except `.env.example` or `.env.template`).
- **NEVER hardcode secrets, API keys, tokens, passwords, or connection strings** in source code.
- **NEVER include secrets in commit messages, comments, documentation, or log output.**
- **NEVER echo, print, or log environment variables** that may contain secrets.
- If a user's prompt or message appears to contain a secret (API key, password, token, connection string), **immediately warn the user** and do not process or repeat the secret. Suggest they rotate it.
- Always use environment variables or a dedicated secret manager for credentials.
- Reference `.env.example` for the expected variable names, never the actual values.

### 7.2 Secret Patterns to Watch For

Flag and warn the user if you detect any of these patterns in code, prompts, or content:

- API keys: strings matching `(sk|pk|api|key|token|secret|password|credential)[-_]?[a-zA-Z0-9]{16,}`
- AWS keys: `AKIA[0-9A-Z]{16}`
- Private keys: `-----BEGIN (RSA |EC |DSA |OPENSSH )?PRIVATE KEY-----`
- Connection strings: `(mongodb|postgres|mysql|redis|amqp):\/\/[^\s]+`
- JWT tokens: `eyJ[A-Za-z0-9-_]+\.eyJ[A-Za-z0-9-_]+`
- Generic high-entropy strings in assignment context (e.g., `password = "..."` with 20+ characters)

### 7.3 Secure Coding Practices

- **Input validation:** Validate and sanitize all user input at system boundaries.
- **Output encoding:** Apply context-appropriate encoding to prevent XSS.
- **SQL injection:** Use parameterized queries or ORM methods; never concatenate user input into SQL.
- **Authentication:** Never implement custom cryptography; use established libraries.
- **Authorization:** Enforce least-privilege access checks on every endpoint and operation.
- **CSRF protection:** Ensure state-changing operations verify origin/tokens.
- **File handling:** Never construct file paths from user input without validation and sandboxing.
- **Deserialization:** Never deserialize untrusted data without validation.
- **Error handling:** Never expose stack traces, internal paths, or system details to end users.
- **OWASP Top 10:** All code must be written with awareness of the current OWASP Top 10 vulnerabilities.

### 7.4 Protected Files

The following files and directories must **never be modified** by AI agents without explicit, per-change user approval. When approval is needed, show a clear before/after diff.

- `.env`, `.env.*` (except `.env.example`, `.env.template`)
- `*.pem`, `*.key`, `*.cert`, `*.p12`, `*.pfx`, `*.jks`
- `.github/workflows/` (CI/CD pipeline definitions)
- `Dockerfile`, `docker-compose.yml`, `docker-compose.*.yml`
- `Makefile`, `Justfile`, `Taskfile.yml`
- `.npmrc`, `.pypirc`, `pip.conf` (package registry configuration)
- `terraform.tfstate`, `*.tfvars`
- `.git/` (never touch Git internals)
- `CODEOWNERS`
- Infrastructure-as-code files (`*.tf`, `serverless.yml`, `cdk.json`, `pulumi.*`)
- Kubernetes manifests (`*.yaml` in `k8s/`, `deploy/`, `manifests/`)
- [PLACEHOLDER - Add project-specific protected files]

### 7.5 Secret Lifecycle & Credential Federation

> Assume any credential an AI agent's environment can read will eventually be exfiltrated — by prompt injection, a poisoned dependency, or a malicious MCP server. Design accordingly.

#### OIDC & Workload-Identity Federation

- **Prefer OIDC / workload-identity federation** over static secrets for all CI-to-cloud and CI-to-registry authentication (e.g., GitHub Actions OIDC → AWS, GCP, Azure).
- No static secret in the runner means nothing to steal. Where OIDC is supported, static keys are not acceptable.

#### Static Secret Controls

- Where static secrets are unavoidable, gate them behind **environment protection rules** that require manual approval before secrets are materialized into the runner.
- **Separate publish/deploy credentials from build credentials.** An agent performing routine build and test work must never hold a token that can publish a package, push to `main`, or deploy to production.
- Store secrets in your CI/CD platform's secret manager (GitHub Actions Secrets, Vault, etc.) — never in environment variables baked into images or config files.

#### Token Lifetime

- All tokens must have the **shortest practical TTL**. Prefer minutes over hours, hours over days.
- Access tokens and refresh tokens must never both be accessible within the same scope. If both live where the agent can see them, the expiry is theatrical.
- Service account keys with no expiry are prohibited; use time-bound credentials.

#### Credential Inventory

Maintain a register of all credentials the CI/CD pipeline and AI agents can access:

| Credential | Purpose | Type | TTL | Rotation Schedule | Owner |
|------------|---------|------|-----|-------------------|-------|
| [PLACEHOLDER] | [PLACEHOLDER] | OIDC / static / service-account | [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] |

---

## 8. Privacy Rules

> **All code written or modified by AI agents must follow privacy-by-design principles.**

### 8.1 Data Minimization

- Only collect, store, and process data that is strictly necessary for the feature.
- Prefer anonymous or pseudonymous identifiers over personally identifiable information (PII).
- Set retention limits; do not store data indefinitely without justification.

### 8.2 PII Handling

- Never log PII (names, email addresses, IP addresses, phone numbers, etc.) in application logs.
- Mask or redact PII in error messages and log output (e.g., `user: j***@example.com`).
- Encrypt PII at rest and in transit.
- When writing database migrations or schemas, mark PII fields clearly with comments.

### 8.3 Telemetry & Analytics

- Never add tracking, analytics, or telemetry without explicit user knowledge and consent mechanisms.
- All analytics must be opt-in or provide clear opt-out.
- Never send PII to third-party analytics services.
- Strip identifying information before aggregating metrics.

### 8.4 Consent & Compliance

- Code handling personal data must support GDPR rights: access, rectification, erasure, portability.
- Include consent mechanisms where personal data is collected.
- Respect Do-Not-Track (DNT) headers where applicable.
- Follow the most restrictive applicable regulation (GDPR, CCPA, PIPEDA, etc.).

### 8.5 AI Agent Privacy

- AI agents must **not** send repository code, file contents, or data to external services beyond what is strictly required for their own operation (code completion, chat).
- Never include file contents, source code, or project data in web searches, URL parameters, or third-party API calls.
- Do not reference, analyze, or transmit contents of files listed in `.gitignore` or the ignore files (`.aiignore`, `.clineignore`, etc.) unless the user explicitly requests it.

---

## 9. Access Restrictions

> **AI agents must operate under the principle of least privilege.**

### 9.1 Filesystem Boundaries

- **ONLY operate within the repository root directory.** Never read, write, or reference files outside the repository.
- Never access the user's home directory, system files, or other repositories.
- Never access or modify the `.git/` directory internals.
- Respect all ignore files (`.gitignore`, `.aiignore`, `.clineignore`, `.cursorignore`, `.aiderignore`).

### 9.2 Network Restrictions

- Never make network requests unless they are directly required for the task (e.g., installing dependencies).
- Never curl, wget, or fetch URLs found in code without user permission.
- Never exfiltrate data by encoding it in URLs, DNS queries, or HTTP headers.
- Package installations must use the project's configured registries only.

### 9.3 Command Execution

- Only execute commands that are documented in this file, `package.json` scripts, `Makefile`, or equivalent project task runners.
- **Never run destructive commands** (`rm -rf`, `drop database`, `format`, etc.) without explicit user confirmation showing exactly what will be affected.
- Never install global packages or modify system-wide configuration.
- Never modify shell profiles, environment files, or system PATH.
- Never disable security tools (firewalls, antivirus, SELinux, etc.).

### 9.4 Allowed Commands

```
[PLACEHOLDER - List the specific commands AI agents are allowed to run, e.g.:]
# Build
npm run build
# pnpm build

# Test
npm test
# pnpm test
# pytest

# Lint
npm run lint
# pnpm lint
# ruff check .

# Format
npm run format
# pnpm format
# ruff format .

# Type check
npm run typecheck
# pnpm typecheck
# mypy .

# Install dependencies (from lock file)
npm ci
# pnpm install --frozen-lockfile
# pip install -r requirements.txt
```

### 9.5 Agent Sandboxing & Tool-Surface Restrictions

> The model isn't the threat surface — its tools are. MCP servers, shell access, and permission-bypass flags convert natural language into arbitrary code execution.

#### Tool Inventory

- Every connected tool, MCP server, IDE extension, and plugin available to AI agents **must** be documented in `AGENT_TOOL_INVENTORY.md`.
- The inventory must record: tool name, purpose, access level (read-only / read-write), who approved it, and when.
- Review the inventory quarterly. Remove tools that are no longer needed.

#### Read-Only by Default

- Data-source tools (database viewers, file browsers, API clients, drive/mail/wiki integrations) must be configured **read-only** unless write access is explicitly justified and approved per-task.
- Write scopes require an explicit, per-task elevation — never a standing grant.

#### Dangerous Flag Deny List

AI agents must **never** use the following flags or their equivalents, regardless of context:

- `--force`, `-f` (on destructive operations)
- `--no-verify` (skips pre-commit hooks and other safety checks)
- `--skip-integrity-check`
- `--dangerously-skip-permissions`
- `--disable-security`
- `--trust` (blanket trust flags)
- `--ignore-scripts` bypass overrides (e.g., removing `--ignore-scripts` from safe install commands)

Wrapper scripts should strip these flags rather than relying on agents or users to remember.

#### Disposable Environments

- AI agent work should occur in **disposable, isolated environments** where practical: devcontainers, ephemeral VMs, or sandboxed shells.
- Agent environments must not have access to: host SSH keys, production `.env` files, persistent home directories with credentials, or cloud credential stores (`~/.aws/`, `~/.config/gcloud/`, `~/.kube/`).
- See `.devcontainer/devcontainer.json` for the baseline sandbox configuration.

#### MCP Server Security

- MCP server configuration files must **never** be committed with embedded credentials.
- MCP config files containing secrets must be listed in `.aiignore` and all tool-specific ignore files.
- Each MCP server's permissions must follow least-privilege: grant only the scopes the agent actually needs.

#### Network Allow/Deny List

```
[PLACEHOLDER — Define the approved network destinations for AI agent tools]
# Allowed destinations
# - registry.npmjs.org (package installs)
# - pypi.org (package installs)
# - api.github.com (GitHub API)
# - [Add project-specific endpoints]

# Denied patterns (catch-all deny for unlisted endpoints)
# - * (all unlisted endpoints are denied by default)
```

---

## 10. Testing Requirements

> **All code changes must be covered by tests. Tests must pass before any commit or push.**

### 10.1 Test-Before-Commit Policy

- **Run the full test suite before every commit.** If tests fail, fix them before committing.
- **Run linting and type checking before every commit.** Fix all errors before committing.
- Never skip tests with `--no-verify`, `skip`, `xfail`, or equivalent without documenting a reason.
- Never delete or disable existing tests to make new code pass.

### 10.2 Test Coverage

- All new functions, methods, and endpoints must have corresponding tests.
- Bug fixes must include a regression test that reproduces the bug.
- Edge cases, error paths, and boundary conditions must be tested.
- Aim for meaningful coverage of business logic, not arbitrary coverage percentages.

### 10.3 Test Quality

- Tests must be deterministic: no random data, no time-dependent assertions, no flaky network calls.
- Use mocks and stubs for external services; never call real APIs in tests.
- Test names must clearly describe the scenario and expected behavior.
- Follow the Arrange-Act-Assert pattern.

### 10.4 Testing Commands

```
[PLACEHOLDER - Define the exact test commands for this project, e.g.:]
# Unit tests
npm test
# pytest tests/unit/

# Integration tests
npm run test:integration
# pytest tests/integration/

# End-to-end tests
npm run test:e2e

# All tests
npm run test:all

# Coverage report
npm run test:coverage
```

---

## 11. Accessibility Standards

> **All user-facing code must meet WCAG 2.1 AA standards at minimum.**

### 11.1 Semantic HTML

- Use semantic HTML elements (`<nav>`, `<main>`, `<article>`, `<section>`, `<button>`, etc.).
- Never use `<div>` or `<span>` for interactive elements; use `<button>` and `<a>`.
- Ensure correct heading hierarchy (`<h1>` through `<h6>` in order).

### 11.2 ARIA & Screen Readers

- Add ARIA labels, roles, and descriptions where semantic HTML is insufficient.
- All images must have meaningful `alt` attributes (or `alt=""` for decorative images).
- Form inputs must have associated `<label>` elements.
- Dynamic content updates must use ARIA live regions.

### 11.3 Keyboard Navigation

- All interactive elements must be reachable and operable via keyboard.
- Maintain a logical tab order.
- Provide visible focus indicators.
- Support Escape to close modals and dialogs.

### 11.4 Visual Design

- Maintain a minimum color contrast ratio of 4.5:1 for normal text, 3:1 for large text.
- Never convey information through color alone.
- Support user font-size preferences; use relative units (`rem`, `em`), not `px` for text.
- Support `prefers-reduced-motion` and `prefers-color-scheme` media queries.

### 11.5 Testing Accessibility

- Run automated accessibility checks (axe, lighthouse) as part of the test suite.
- Test with keyboard-only navigation.
- Verify screen reader compatibility for critical user flows.

---

## 12. CI/CD & Pipeline Rules

> **AI agents must never modify CI/CD configurations without explicit approval.**

### 12.1 Pipeline Protection

- **NEVER modify files in `.github/workflows/`, `.gitlab-ci.yml`, `Jenkinsfile`, `.circleci/`, `azure-pipelines.yml`, `bitbucket-pipelines.yml`**, or any other CI/CD configuration without explicit per-change user approval with a before/after diff.
- Never add steps that download or execute external scripts in pipelines.
- Never add credentials, tokens, or secrets to pipeline files (use CI/CD secret management).
- Never disable, skip, or weaken existing pipeline checks or gates.

### 12.2 Pipeline Standards

- All pipelines must include: linting, type checking, testing, and security scanning steps.
- Build artifacts must be reproducible (use lock files, pinned versions).
- Deployment steps must require manual approval for production environments.
- Pipeline definitions should be reviewed with the same rigor as application code.

### 12.3 Deployment Safety

- Never auto-deploy to production without human approval.
- Ensure rollback mechanisms exist before deploying new features.
- Use blue-green or canary deployment strategies for critical services.
- [PLACEHOLDER - Define project-specific deployment rules]

### 12.4 CI/CD Runtime Hardening

> Most 2025–2026 AI-agent breaches (tj-actions, Cline, Shai-Hulud, prt-scan, Comment-and-Control) succeeded at the runner level, not the model level. Lock down the runtime.

#### SHA Pinning

- **Pin all third-party GitHub Actions by full commit SHA**, never by tag. Tags are mutable; digests are not.
  ```yaml
  # Correct — pinned by SHA with human-readable version in comment
  uses: actions/checkout@b4ffde65f46336ab88eb53be808477a3936bae11  # v4.1.1

  # Wrong — mutable tag, vulnerable to supply-chain attacks
  uses: actions/checkout@v4
  ```
- **Pin container images by SHA256 digest**, not by tag.
  ```dockerfile
  # Correct
  FROM node@sha256:abc123...

  # Wrong
  FROM node:20-alpine
  ```
- Use Dependabot or Renovate to automate SHA pin updates with human-reviewed PRs.

#### Default-Deny Egress

- Runners should have **default-deny outbound network access** with an explicit allowlist of domains the workflow actually needs.
- Anomalous outbound calls are the single most reliable signal of a compromised dependency or action.
- Consider [StepSecurity Harden-Runner](https://github.com/step-security/harden-runner) or equivalent for egress monitoring.
- [PLACEHOLDER — Define your workflow egress allowlist]

#### Least-Privilege CI Tokens

- Every workflow and every job must declare an explicit `permissions:` block. Never rely on the default (which may be `write-all`).
- Grant elevated scopes **only on the specific job** that needs them, not at the workflow level.
- Use `persist-credentials: false` on `actions/checkout` unless the job explicitly needs to push.
- Never pass `secrets.GITHUB_TOKEN` with write permissions to jobs that only need to read.

#### Secrets in CI

- **Never let AI agents read untrusted input** (issues, PR comments, external docs) **in a workflow that has access to secrets.**
- Split it into two workflows: one privileged (with secrets), one that reads untrusted content with no secrets attached.
- See `.github/workflows/untrusted-pr.yml` for the boilerplate untrusted-content workflow.

### 12.5 Workflow Trust Boundaries

#### Trusted vs. Untrusted Workflows

- **Trusted workflows** (`pull_request`, `push`): Run on code from collaborators. May access repository secrets. Use for main-branch CI and collaborator PRs.
- **Untrusted workflows** (`pull_request_target`): Run on code from forks and external contributors. Must **never** access secrets. Must checkout the PR HEAD explicitly with `persist-credentials: false`.

#### First-Time Contributor Gating

- PRs from users with `author_association` of `FIRST_TIME_CONTRIBUTOR` or `NONE` must be **labeled for manual security review** before any AI-touched workflow with secret access runs.
- External PRs must not trigger AI-agent workflows that have access to secrets without manual approval.

#### Instruction-File Guard

- CI must **detect and flag** PRs that modify any agent instruction file (AGENTS.md, CLAUDE.md, .clinerules, etc.) or CODEOWNERS.
- These files are a known persistence mechanism — a one-time prompt injection that edits them becomes a durable behavior change.
- See `.github/workflows/untrusted-pr.yml` for the boilerplate instruction-file guard job.

---

## 13. Dependency & Supply Chain Security

> **Protecting against supply chain attacks is a top priority.**

### 13.1 Dependency Management

- **NEVER add new dependencies without user approval.** When proposing a dependency, include:
  - Package name and exact version
  - Purpose and justification
  - Download count / popularity metrics
  - Last updated date
  - Number of maintainers
  - License compatibility
  - Known vulnerabilities (check advisories)
- Prefer well-established, widely-used packages over obscure ones.
- Pin exact versions in lock files (`package-lock.json`, `pnpm-lock.yaml`, `poetry.lock`, etc.).
- Never run `npm install <package>` or equivalent without user approval.

### 13.2 Dependency Verification

- Verify package names carefully to avoid typosquatting (e.g., `lodash` vs. `l0dash`).
- Check that the package source matches the expected repository.
- Be suspicious of packages with very few downloads, very new publish dates, or single maintainers for critical functionality.
- Never install packages from URLs, git repos, or local paths without user review.

### 13.3 Lock File Integrity

- **NEVER delete or regenerate lock files** without explicit user approval.
- If a lock file conflict occurs, resolve it carefully; do not simply regenerate.
- Lock file changes must be reviewed as carefully as code changes.

### 13.4 Vulnerability Response

- If you discover a known vulnerability in an existing dependency, **warn the user immediately**.
- Propose the minimal update needed to resolve the vulnerability.
- Never ignore or suppress vulnerability warnings.

---

## 14. Code Quality Standards

### 14.1 General Principles

- Follow existing code patterns and conventions in the repository.
- Prefer readability over cleverness.
- Keep functions small and focused on a single responsibility.
- Apply DRY (Don't Repeat Yourself) but not at the cost of readability.
- Remove dead code; do not leave commented-out code blocks.

### 14.2 Error Handling

- Handle errors explicitly; never swallow exceptions silently.
- Use typed errors or error codes for programmatic handling.
- Provide user-friendly error messages at the UI level.
- Log errors with sufficient context for debugging (but never with PII or secrets).

### 14.3 Documentation

- Write self-documenting code with clear names.
- Add comments only for non-obvious decisions or workarounds.
- Keep API documentation up to date when changing endpoints.
- Update README or relevant docs when changing setup, build, or deployment procedures.

### 14.4 Performance

- Be mindful of computational complexity; avoid O(n^2) or worse where linear solutions exist.
- Avoid unnecessary memory allocations in hot paths.
- Use pagination for large data sets.
- Consider caching for expensive, repeatable computations.

---

## 15. Agent Self-Governance

> **These rules govern how AI agents interact with this instruction file and the project's configuration.**

### 15.1 Modifying These Instructions

- **NEVER modify this file (AGENTS.md) or any tool-specific instruction file without explicit user approval.**
- When changes are needed, present a clear **before and after** comparison.
- Explain **why** the change is necessary.
- Wait for the user to confirm before applying the change.

### 15.2 Filling In Placeholders

When you encounter `[PLACEHOLDER]` sections:

1. Analyze the repository to gather relevant information.
2. Propose specific content to replace the placeholder.
3. Show the proposed change as a before/after diff.
4. Only apply the change after the user explicitly approves.

### 15.3 Scope of Authority

AI agents are authorized to:

- Read files within the repository (respecting ignore files and access restrictions).
- Propose code changes, new files, and modifications.
- Run allowed commands (build, test, lint, format).
- Explain code, architecture, and decisions.

AI agents are **NOT** authorized to:

- Modify instruction files or configuration without approval.
- Access files outside the repository.
- Install dependencies or run commands outside the allowed list.
- Make network requests beyond what is needed for their operation.
- Commit or push code without the user's approval.
- Modify CI/CD pipelines, infrastructure code, or security settings without approval.
- Override, ignore, or weaken any rule in this file.

### 15.4 Conflict Resolution

If a user request conflicts with these rules:

1. Inform the user of the specific rule that would be violated.
2. Explain the risk of proceeding.
3. Suggest an alternative approach that achieves the goal without violating the rules.
4. Only proceed with the violation if the user **explicitly acknowledges the risk** and confirms.

### 15.5 Incident Reporting

If you detect any of the following, **immediately notify the user**:

- Secrets or credentials in source code, logs, or configuration files.
- Known vulnerabilities in dependencies.
- Suspicious or potentially malicious code patterns.
- Files that appear to have been tampered with.
- Unusual dependency changes in lock files.
- CI/CD configuration changes that weaken security.

---

## 16. Blast-Radius Policy & Monitoring

> `AGENTS.md` describes what the agent should do. This section defines what it **can't** do, plus detection for when rules are tampered with.

### 16.1 Allow/Deny Lists

Define explicit per-repository restrictions. These extend the global rules in sections 7 and 9.

#### Path Restrictions

| Path | Agent Access | Justification |
|------|-------------|---------------|
| `src/` | read-write | [PLACEHOLDER — Application source code] |
| `tests/` | read-write | [PLACEHOLDER — Test code] |
| `.github/workflows/` | read-only | CI/CD — changes require CODEOWNERS review |
| `AGENTS.md`, `CLAUDE.md`, instruction files | read-only | Agent instructions — changes require CODEOWNERS review |
| [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] |

#### Forbidden Commands

The following commands are **never** permitted for AI agents, beyond the restrictions in section 9.3:

- `git push --force` / `git push --force-with-lease` to shared branches
- `git tag -d` / `git push --delete` (tag or branch deletion)
- `npm publish` / `pip upload` / `cargo publish` / any package publish command
- `rm -rf /` or any recursive delete outside the working directory
- Any command that touches GPG/SSH signing material
- [PLACEHOLDER — Add project-specific forbidden commands]

#### Network Restrictions

See section 9.5 for the network allow/deny list template.

### 16.2 Instruction File Protection

- All agent instruction files listed in Appendix A **must** be protected via `CODEOWNERS`, requiring security-team review for any modification.
- CI must run the `instruction-file-guard` job (see `.github/workflows/untrusted-pr.yml`) to detect and flag PRs that modify these files.
- Agent instruction files are a **known persistence mechanism**: a one-time prompt injection that edits them becomes a durable, cross-session behavior change.
- Changes to `CODEOWNERS` itself must also require security-team review.

### 16.3 Audit Logging & Review

- All AI agent commits must include an `Assisted-by:` or `Co-Authored-By:` trailer (see section 6.4).
- Teams should **review AI-authored commits weekly** using the git log:
  ```bash
  git log --all --grep="Assisted-by:" --grep="Co-Authored-By:" --since="1 week ago"
  ```
- Review checklist for weekly audits:
  - [ ] Any unusual commit timing (outside working hours, high volume)?
  - [ ] Any first-time file paths touched by AI agents?
  - [ ] Any modifications to instruction files, CI configs, or CODEOWNERS?
  - [ ] Any anomalous egress destinations in CI logs?
  - [ ] Any new dependencies added without corresponding approval?
- Pipe CI runner logs to a centralized log store for long-term AI action tracing where practical.

### 16.4 Kill-Switch Procedure

Every team member with repository access should know how to disable AI agent access within minutes. See `SECURITY.md` for the full runbook.

**Summary of kill-switch steps:**

1. **Revoke CI/CD tokens** — Disable GitHub App installations, revoke PATs, rotate `GITHUB_TOKEN` permissions to `read-only`.
2. **Disable AI agent workflows** — Disable workflows in `.github/workflows/` via the GitHub UI or by pushing a commit that removes workflow triggers.
3. **Remove MCP server connections** — Disconnect all MCP servers from agent configurations.
4. **Rotate affected credentials** — Rotate any secrets that may have been exposed.
5. **Review recent AI-authored commits** — Audit the last 48 hours of AI-attributed commits for suspicious changes.

**Test the kill-switch quarterly.** A kill-switch you've never tested is a kill-switch that doesn't work.

---

## Appendix A: Tool-Specific Configuration Files

This repository includes configuration files for the following AI tools. Each file references this `AGENTS.md` as the source of truth.

| Tool | Configuration File(s) | Format |
|------|----------------------|--------|
| Claude Code | `CLAUDE.md`, `.claude/settings.json` | Markdown, JSON |
| Cursor | `.cursor/rules/*.mdc` | MDC (frontmatter + Markdown) |
| GitHub Copilot | `.github/copilot-instructions.md` | Markdown |
| Windsurf (Codeium) | `.windsurfrules` | Markdown |
| Cline | `.clinerules` | Markdown |
| Continue.dev | `.continuerules` | Markdown |
| Aider | `.aider.conf.yml`, `CONVENTIONS.md` | YAML, Markdown |
| Amazon Q | `.amazonq/rules/*.md` | Markdown |
| JetBrains Junie | `.junie/guidelines.md` | Markdown |
| Devin | `devin.md` | Markdown |
| Sourcegraph Cody | `.sourcegraph/cody.json` | JSON |
| OpenAI Codex | `AGENTS.md` (this file) | Markdown |

### Security Hardening Files

| File | Purpose |
|------|---------|
| `SECURITY_CHECKLIST.md` | Step-by-step adoption checklist for the five security pillars |
| `SECURITY.md` | Vulnerability reporting, kill-switch procedure, credential rotation runbook |
| `CODEOWNERS` | Enforces human review for security-sensitive paths |
| `.pre-commit-config.yaml` | Pre-commit hooks for secret scanning and SAST |
| `.github/workflows/ai-security-scan.yml` | CI pipeline: SAST, SCA, secret scanning, lockfile integrity |
| `.github/workflows/untrusted-pr.yml` | Restricted pipeline for fork and first-time contributor PRs |
| `AGENT_TOOL_INVENTORY.md` | Template for documenting agent tools and MCP servers |
| `.devcontainer/devcontainer.json` | Sandboxed development environment template |

### Ignore Files

| File | Purpose |
|------|---------|
| `.aiignore` | Universal AI agent ignore patterns |
| `.cursorignore` | Cursor-specific ignore patterns |
| `.clineignore` | Cline-specific ignore patterns |
| `.aiderignore` | Aider-specific ignore patterns |

---

## Appendix B: Quick Reference for AI Agents

### Before writing any code, verify:

- [ ] Are you operating within the repository boundaries?
- [ ] Have you checked the testing requirements?
- [ ] Are you following the project's naming conventions?
- [ ] Is your code free of hardcoded secrets?
- [ ] Have you validated all user input?
- [ ] Are you using parameterized queries for database operations?
- [ ] Is PII properly handled (masked in logs, encrypted at rest)?
- [ ] Are new dependencies approved by the user?
- [ ] Does your code meet accessibility standards?
- [ ] Have you run the linter and type checker?

### Before committing, verify:

- [ ] All tests pass.
- [ ] Linting passes with no errors.
- [ ] Type checking passes.
- [ ] No secrets in the diff.
- [ ] No files outside the allowed scope were modified.
- [ ] Commit message follows project conventions.
- [ ] Changes are on a feature branch, not the main branch.
- [ ] Commit includes an `Assisted-by:` or `Co-Authored-By:` trailer identifying the AI agent.
- [ ] No agent instruction files were modified without showing a diff and getting approval.
- [ ] No dangerous CLI flags (`--force`, `--no-verify`, etc.) were used.
- [ ] All tools and MCP servers used are documented in `AGENT_TOOL_INVENTORY.md`.
