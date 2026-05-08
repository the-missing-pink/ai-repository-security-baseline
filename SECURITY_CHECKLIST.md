# AI Agent Security Checklist

> Walk through this checklist when adopting the security baseline into your repository.
> Each item is tagged with its type and priority level.
>
> - **`[TEMPLATE]`** — A boilerplate file is included; customize and enable it.
> - **`[MANUAL]`** — Requires project-specific configuration; no boilerplate provided.
> - **`P0`** — Critical. Complete before AI agents are used in the repository.
> - **`P1`** — High. Complete within the first week of adoption.
> - **`P2`** — Medium. Complete within the first month; review quarterly.

---

## How to Use This Checklist

1. Copy this repository's files into your project (see `README.md`).
2. Work through the items below in priority order.
3. Fill in all `[PLACEHOLDER]` markers in the configuration files.
4. Check off each item as you complete it.
5. Re-review this checklist quarterly to catch drift.

---

## Pillar 1: CI/CD Runtime Security

_Lock down the runtime where damage actually happens. See AGENTS.md §12.4–12.5._

- [ ] **Pin all GitHub Actions by full SHA digest** `[TEMPLATE]` `P0`
  _What:_ Replace all tag-based action references with commit SHA pins.
  _Where:_ `.github/workflows/ai-security-scan.yml`, `.github/workflows/untrusted-pr.yml`
  _Verify:_ `grep -r "uses:" .github/workflows/ | grep -v "@[a-f0-9]\{40\}"` returns no results.

- [ ] **Pin container images by SHA256 digest** `[MANUAL]` `P0`
  _What:_ Replace all tag-based image references in Dockerfiles and CI with digest pins.
  _Where:_ All `Dockerfile` and `docker-compose*.yml` files.
  _Verify:_ `grep -r "FROM " Dockerfile* | grep -v "@sha256:"` returns no results.

- [ ] **Set explicit `permissions:` on every workflow and job** `[TEMPLATE]` `P0`
  _What:_ Declare least-privilege permissions; never rely on default `write-all`.
  _Where:_ `.github/workflows/*.yml`
  _Verify:_ Every workflow file has a top-level `permissions:` block.

- [ ] **Use `persist-credentials: false` on checkout** `[TEMPLATE]` `P0`
  _What:_ Prevent the `GITHUB_TOKEN` from being persisted in the local git config.
  _Where:_ All `actions/checkout` steps in workflows.
  _Verify:_ `grep -A5 "actions/checkout" .github/workflows/*.yml` shows `persist-credentials: false` on untrusted workflows.

- [ ] **Configure default-deny egress on runners** `[MANUAL]` `P1`
  _What:_ Restrict outbound network access to an explicit allowlist.
  _Where:_ CI runner configuration or StepSecurity Harden-Runner integration.
  _Verify:_ Test that a `curl` to an unlisted domain fails in CI.

- [ ] **Split trusted and untrusted workflows** `[TEMPLATE]` `P0`
  _What:_ Ensure fork/external PRs run in a workflow with no secrets access.
  _Where:_ `.github/workflows/untrusted-pr.yml` (untrusted), `.github/workflows/ai-security-scan.yml` (trusted).
  _Verify:_ Untrusted workflow has no `secrets:` references and uses `pull_request_target`.

- [ ] **Gate first-time contributor PRs** `[TEMPLATE]` `P1`
  _What:_ Label PRs from first-time contributors for manual review before AI workflows run with secrets.
  _Where:_ `.github/workflows/untrusted-pr.yml`
  _Verify:_ Open a test PR from a non-collaborator account; verify it receives the `needs-security-review` label.

- [ ] **Enable Dependabot or Renovate for action SHA updates** `[MANUAL]` `P1`
  _What:_ Automate PRs to update pinned action SHAs when new versions are released.
  _Where:_ `.github/dependabot.yml` or `renovate.json`
  _Verify:_ Dependabot/Renovate creates PRs when an action has a new release.

---

## Pillar 2: Short-Lived, Revocable Secrets

_Design for credential exfiltration as an inevitability. See AGENTS.md §7.5._

- [ ] **Configure OIDC for CI-to-cloud authentication** `[MANUAL]` `P0`
  _What:_ Use GitHub Actions OIDC provider instead of static cloud credentials.
  _Where:_ Cloud provider IAM configuration + workflow files.
  _Verify:_ `grep -r "aws-access-key\|AZURE_CREDENTIALS\|GCP_SA_KEY" .github/workflows/` returns no results.

- [ ] **Set environment protection rules for production secrets** `[MANUAL]` `P0`
  _What:_ Require manual approval before production secrets are available to a workflow run.
  _Where:_ GitHub repository Settings → Environments.
  _Verify:_ Production environment shows "Required reviewers" in GitHub settings.

- [ ] **Separate publish/deploy credentials from build credentials** `[MANUAL]` `P1`
  _What:_ Build jobs use read-only tokens; publish/deploy jobs use separate, scoped tokens.
  _Where:_ CI/CD secret configuration.
  _Verify:_ Build jobs cannot publish packages or deploy to production.

- [ ] **Enforce short TTLs on all tokens** `[MANUAL]` `P1`
  _What:_ Set the shortest practical expiry on all service account tokens and API keys.
  _Where:_ Identity provider / secret manager configuration.
  _Verify:_ Audit credential inventory table in AGENTS.md §7.5; no token has TTL > 24h without justification.

- [ ] **Complete the credential inventory** `[MANUAL]` `P1`
  _What:_ Document every credential the CI/CD pipeline and AI agents can access.
  _Where:_ AGENTS.md §7.5 credential inventory table.
  _Verify:_ Table has no `[PLACEHOLDER]` entries.

- [ ] **Remove long-lived PATs and service account keys** `[MANUAL]` `P1`
  _What:_ Replace personal access tokens and static service account keys with OIDC or short-lived alternatives.
  _Where:_ GitHub settings, cloud provider IAM.
  _Verify:_ `gh api /repos/{owner}/{repo}/actions/secrets` lists only secrets backed by OIDC or short-lived tokens.

---

## Pillar 3: Agent Tool-Surface Sandboxing

_The model isn't the threat surface — its tools are. See AGENTS.md §9.5._

- [ ] **Complete the agent tool inventory** `[TEMPLATE]` `P0`
  _What:_ Document every MCP server, extension, and tool each AI agent has access to.
  _Where:_ `AGENT_TOOL_INVENTORY.md`
  _Verify:_ File has no `[PLACEHOLDER]` entries; all agents are covered.

- [ ] **Set data-source tools to read-only by default** `[MANUAL]` `P1`
  _What:_ Configure database viewers, file browsers, API clients as read-only.
  _Where:_ MCP server configs, IDE extension settings.
  _Verify:_ Attempt a write operation from a data-source tool; confirm it is denied.

- [ ] **Configure devcontainer for isolated agent work** `[TEMPLATE]` `P1`
  _What:_ Use the sandboxed devcontainer for AI agent sessions.
  _Where:_ `.devcontainer/devcontainer.json`
  _Verify:_ Open the devcontainer; confirm no host credentials are accessible.

- [ ] **Strip dangerous CLI flags via wrapper scripts** `[MANUAL]` `P2`
  _What:_ Create wrapper scripts that filter out `--force`, `--no-verify`, etc.
  _Where:_ Project `scripts/` directory or shell aliases.
  _Verify:_ Run `./scripts/safe-npm.sh install --force`; confirm `--force` is stripped.

- [ ] **Remove MCP credentials from committed files** `[MANUAL]` `P0`
  _What:_ Ensure no MCP server config files contain embedded secrets.
  _Where:_ All MCP config files; add paths to `.aiignore`.
  _Verify:_ `grep -r "api_key\|token\|password\|secret" **/mcp*.json` returns no results.

- [ ] **Run the dangerous capabilities checklist** `[TEMPLATE]` `P1`
  _What:_ Answer every question in the `AGENT_TOOL_INVENTORY.md` dangerous capabilities section.
  _Where:_ `AGENT_TOOL_INVENTORY.md`
  _Verify:_ All checklist items are checked and have documented justifications.

---

## Pillar 4: AI-Generated Code as Untrusted Input

_More code, more vulnerabilities per developer, less human review per line. See AGENTS.md §6.4, §12.2._

- [ ] **Install pre-commit hooks** `[TEMPLATE]` `P0`
  _What:_ Set up gitleaks, detect-private-key, and large-file checks locally.
  _Where:_ `.pre-commit-config.yaml`
  _Verify:_ `pip install pre-commit && pre-commit install && pre-commit run --all-files`

- [ ] **Enable CI secret scanning** `[TEMPLATE]` `P0`
  _What:_ TruffleHog runs on every PR and push to catch committed secrets.
  _Where:_ `.github/workflows/ai-security-scan.yml` → `secret-scan` job.
  _Verify:_ Commit a test secret (e.g., `AKIAIOSFODNN7EXAMPLE`); confirm CI fails.

- [ ] **Enable CI SAST scanning** `[TEMPLATE]` `P1`
  _What:_ CodeQL and/or Semgrep run on every PR to catch vulnerability patterns.
  _Where:_ `.github/workflows/ai-security-scan.yml` → `sast` job.
  _Verify:_ Introduce a known vulnerable pattern (e.g., SQL concatenation); confirm CI flags it.

- [ ] **Enable CI SCA / dependency review** `[TEMPLATE]` `P1`
  _What:_ Dependency-review-action checks for vulnerable or license-incompatible dependencies on PRs.
  _Where:_ `.github/workflows/ai-security-scan.yml` → `sca` job.
  _Verify:_ Add a dependency with a known CVE; confirm CI flags it.

- [ ] **Configure CODEOWNERS for security-sensitive paths** `[TEMPLATE]` `P0`
  _What:_ Require security-team review for instruction files, CI configs, CODEOWNERS, auth code.
  _Where:_ `CODEOWNERS`
  _Verify:_ Open a PR that modifies `AGENTS.md`; confirm the security team is requested for review.

- [ ] **Tag AI-authored commits with trailers** `[MANUAL]` `P1`
  _What:_ Ensure AI agents include `Assisted-by:` or `Co-Authored-By:` trailers.
  _Where:_ Agent configuration files.
  _Verify:_ `git log --grep="Assisted-by:" --oneline` returns AI-authored commits.

- [ ] **Enable lockfile integrity checks in CI** `[TEMPLATE]` `P1`
  _What:_ Verify lockfiles are consistent with manifests on every PR.
  _Where:_ `.github/workflows/ai-security-scan.yml` → `lockfile-check` job.
  _Verify:_ Modify `package.json` without updating the lockfile; confirm CI fails.

---

## Pillar 5: Blast-Radius Policy & Monitoring

_Define what agents can't do, and detect when rules get tampered with. See AGENTS.md §16._

- [ ] **Define allow/deny lists for paths and commands** `[MANUAL]` `P1`
  _What:_ Fill in the path restrictions and forbidden commands tables.
  _Where:_ AGENTS.md §16.1
  _Verify:_ Tables have no `[PLACEHOLDER]` entries.

- [ ] **Protect instruction files via CODEOWNERS** `[TEMPLATE]` `P0`
  _What:_ Require security-team review for all files listed in Appendix A.
  _Where:_ `CODEOWNERS`
  _Verify:_ PR modifying any instruction file triggers a required review from the security team.

- [ ] **Enable instruction-file guard in CI** `[TEMPLATE]` `P1`
  _What:_ CI flags PRs that modify agent instruction files.
  _Where:_ `.github/workflows/untrusted-pr.yml` → `instruction-file-guard` job.
  _Verify:_ PR modifying `AGENTS.md` triggers a CI warning annotation.

- [ ] **Set up weekly AI commit audit** `[MANUAL]` `P2`
  _What:_ Schedule a weekly review of AI-authored commits using the audit query.
  _Where:_ Calendar reminder + AGENTS.md §16.3 review checklist.
  _Verify:_ Run the audit query; confirm it returns expected results.

- [ ] **Document the kill-switch procedure** `[TEMPLATE]` `P0`
  _What:_ Customize the kill-switch runbook with your project's specific tokens and apps.
  _Where:_ `SECURITY.md`
  _Verify:_ Every team member can describe the kill-switch steps from memory.

- [ ] **Test the kill-switch** `[MANUAL]` `P2`
  _What:_ Practice the full kill-switch procedure in a non-production environment.
  _Where:_ Staging/test environment.
  _Verify:_ All AI agent access is revoked within 5 minutes of initiating the kill-switch.

- [ ] **Configure audit logging for AI bot identities** `[MANUAL]` `P2`
  _What:_ Pipe CI runner logs and VCS audit logs to a centralized log store for AI action tracing.
  _Where:_ CI/CD platform log configuration.
  _Verify:_ Query the log store for AI bot commits from the past week; confirm results match `git log`.

---

## Post-Setup Verification

Run these checks after completing the checklist:

```bash
# 1. Verify no placeholder markers remain in critical files
grep -r "\[PLACEHOLDER" AGENTS.md SECURITY.md CODEOWNERS .github/workflows/ .pre-commit-config.yaml

# 2. Verify all workflow files use SHA-pinned actions
grep -r "uses:" .github/workflows/ | grep -v "@[a-f0-9]\{40\}"

# 3. Verify CODEOWNERS covers all instruction files
# (manually confirm each file in Appendix A has a CODEOWNERS entry)

# 4. Verify pre-commit hooks are installed and pass
pre-commit run --all-files

# 5. Verify CI workflows have explicit permissions blocks
grep -L "permissions:" .github/workflows/*.yml
# (should return no results)

# 6. Verify no secrets in the repository
# (run gitleaks or trufflehog locally)
gitleaks detect --source . --verbose
```
