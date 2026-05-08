# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in this project, please report it responsibly.

**Contact:** [PLACEHOLDER — security contact email address]
**PGP Key:** [PLACEHOLDER — PGP key fingerprint or link to public key]
**Expected Response Time:** [PLACEHOLDER — e.g., "We will acknowledge within 48 hours and provide a detailed response within 5 business days."]

Please include:
- A description of the vulnerability and its potential impact.
- Steps to reproduce the issue.
- Any relevant logs, screenshots, or proof-of-concept code.

**Do not** open a public GitHub issue for security vulnerabilities.

---

## Supported Versions

| Version | Supported |
|---------|-----------|
| [PLACEHOLDER] | [PLACEHOLDER] |

---

## AI Agent Security

This repository uses the [AI Repository Security Baseline](./README.md) to govern how AI coding agents interact with the codebase. The baseline includes:

- **Agent behavioral rules** in `AGENTS.md` — the central source of truth.
- **CI/CD security scanning** — SAST, SCA, and secret scanning on every PR.
- **CODEOWNERS enforcement** — human review required for security-sensitive paths.
- **Pre-commit hooks** — local secret scanning and safety checks.

For the full adoption checklist, see `SECURITY_CHECKLIST.md`.

---

## Kill-Switch Procedure

> Use this procedure to immediately disable all AI agent access to the repository. Every team member with repository access should be familiar with these steps.

### When to Activate

- Suspected credential exfiltration by an AI agent or compromised dependency.
- Detection of unauthorized changes to agent instruction files.
- Anomalous outbound network traffic from CI runners.
- Any other suspected compromise involving AI agent access.

### Steps

#### 1. Revoke CI/CD Tokens (Immediate — within 2 minutes)

- [ ] **GitHub Apps:** Go to repository Settings → Integrations → GitHub Apps → Suspend or uninstall any AI-related apps.
- [ ] **Personal Access Tokens:** Revoke any PATs used by AI agents. Go to github.com → Settings → Developer settings → Personal access tokens → Delete affected tokens.
- [ ] **CI/CD Secrets:** Rotate or delete compromised secrets. Go to repository Settings → Secrets and variables → Actions → Delete or update affected secrets.
- [ ] **OIDC Providers:** Revoke trust for the repository's OIDC identity in your cloud provider's IAM settings.

#### 2. Disable AI Agent Workflows (Within 5 minutes)

- [ ] **GitHub Actions:** Go to repository → Actions → Select each AI-related workflow → Click "..." → Disable workflow.
- [ ] **Alternative:** Push an emergency commit that removes or comments out workflow trigger events.
- [ ] **Branch protection:** Temporarily require admin approval for all workflow runs.

#### 3. Disconnect MCP Servers and Tools (Within 5 minutes)

- [ ] Remove or disconnect all MCP server configurations from AI agent settings.
- [ ] Revoke API keys for any connected services (databases, APIs, cloud resources).
- [ ] Disable IDE extension integrations that provide AI agents with tool access.

#### 4. Rotate Affected Credentials (Within 1 hour)

- [ ] Rotate all secrets that were accessible in the CI/CD environment.
- [ ] Rotate cloud provider credentials (even if OIDC was used, review for lateral movement).
- [ ] Rotate package registry tokens (npm, PyPI, etc.).
- [ ] Rotate database credentials if any were accessible.
- [ ] Update `AGENTS.md` §7.5 credential inventory to reflect new credentials.

#### 5. Audit and Investigate (Within 24 hours)

- [ ] Review AI-authored commits from the past 48 hours:
  ```bash
  git log --all --grep="Assisted-by:" --grep="Co-Authored-By:" --since="48 hours ago" --patch
  ```
- [ ] Review CI runner logs for anomalous outbound connections.
- [ ] Check for modifications to agent instruction files:
  ```bash
  git log --all --since="48 hours ago" -- AGENTS.md CLAUDE.md .clinerules .windsurfrules .continuerules .cursor/rules/ .github/copilot-instructions.md devin.md .amazonq/ .junie/ .sourcegraph/ CODEOWNERS
  ```
- [ ] Review dependency changes in the same period:
  ```bash
  git log --all --since="48 hours ago" -- package-lock.json pnpm-lock.yaml yarn.lock poetry.lock Cargo.lock go.sum
  ```
- [ ] Document findings in an incident report.

### After the Incident

- [ ] Conduct a post-mortem and document root cause.
- [ ] Update the kill-switch procedure based on lessons learned.
- [ ] Re-enable AI agent access only after the root cause is addressed and credentials are rotated.
- [ ] Notify affected parties per your organization's incident response policy.

### Kill-Switch Test Schedule

**Test this procedure quarterly.** Use a staging or test environment.

| Date | Tester | Duration | Issues Found | Resolved |
|------|--------|----------|-------------|----------|
| [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] |

---

## Credential Rotation Runbook

| Credential | Where to Rotate | Steps | Owner |
|------------|----------------|-------|-------|
| GitHub PATs | github.com → Settings → Developer settings → PATs | Delete old token, generate new, update CI secrets | [PLACEHOLDER] |
| Cloud provider keys | Cloud IAM console | Rotate key, update CI secrets, verify OIDC preferred | [PLACEHOLDER] |
| Package registry tokens | Registry settings (npmjs.com, pypi.org) | Revoke old token, generate new, update CI secrets | [PLACEHOLDER] |
| Database credentials | Database admin console | Rotate password, update connection strings in secret manager | [PLACEHOLDER] |
| [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] |

---

## Security Scanning

This repository includes automated security scanning:

| Scanner | Type | Trigger | Configuration |
|---------|------|---------|---------------|
| TruffleHog | Secret scanning | Every PR and push to main | `.github/workflows/ai-security-scan.yml` |
| CodeQL | SAST | Every PR and push to main | `.github/workflows/ai-security-scan.yml` |
| Semgrep | SAST | Every PR and push to main | `.github/workflows/ai-security-scan.yml` |
| dependency-review-action | SCA | Every PR | `.github/workflows/ai-security-scan.yml` |
| gitleaks | Secret scanning (local) | Pre-commit hook | `.pre-commit-config.yaml` |

For details, see the workflow files and `SECURITY_CHECKLIST.md`.
