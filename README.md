# AI Repository Security Baseline

A ready-to-use boilerplate of security-focused instruction files for AI coding agents, assistants, and IDE plugins. Drop these files into any repository to establish a consistent security baseline across all AI tools your team uses.

## The Problem

AI coding agents (Copilot, Cursor, Claude Code, Codex, Cline, Windsurf, etc.) are powerful but introduce new risks:

- **Secret leakage** -- agents reading `.env` files and exposing credentials in suggestions or logs
- **Supply chain attacks** -- agents suggesting malicious or typosquatted packages
- **Data exfiltration** -- agents accessing files outside the repository or sending code to external services
- **Pipeline tampering** -- agents modifying CI/CD configurations to weaken security
- **Privacy violations** -- agents generating code that logs PII or adds unauthorized tracking
- **Prompt injection** -- malicious instructions hidden in code comments or PR descriptions

## The Solution

This repository provides a **single source of truth** (`AGENTS.md`) with comprehensive security, privacy, access control, testing, accessibility, and CI/CD rules, plus tool-specific configuration files that reference it.

Every AI agent reads its own config file format, but they all point back to the same shared rules.

## Supported Tools

| Tool | Config File(s) | Auto-loaded |
|------|---------------|-------------|
| **OpenAI Codex** | [`AGENTS.md`](AGENTS.md) | Yes |
| **Claude Code** | [`CLAUDE.md`](CLAUDE.md) | Yes |
| **Cursor** | [`.cursor/rules/*.mdc`](.cursor/rules/) | Yes |
| **GitHub Copilot** | [`.github/copilot-instructions.md`](.github/copilot-instructions.md) | Yes |
| **Windsurf (Codeium)** | [`.windsurfrules`](.windsurfrules) | Yes |
| **Cline** | [`.clinerules`](.clinerules) | Yes |
| **Continue.dev** | [`.continuerules`](.continuerules) | Yes |
| **Aider** | [`.aider.conf.yml`](.aider.conf.yml), [`CONVENTIONS.md`](CONVENTIONS.md) | Yes |
| **Amazon Q** | [`.amazonq/rules/`](.amazonq/rules/) | Yes |
| **JetBrains Junie** | [`.junie/guidelines.md`](.junie/guidelines.md) | Yes |
| **Devin** | [`devin.md`](devin.md) | Yes |
| **Sourcegraph Cody** | [`.sourcegraph/cody.json`](.sourcegraph/cody.json) | Yes |

### Ignore Files

| File | Tool |
|------|------|
| [`.aiignore`](.aiignore) | Universal reference (all tools) |
| [`.cursorignore`](.cursorignore) | Cursor |
| [`.clineignore`](.clineignore) | Cline |
| [`.aiderignore`](.aiderignore) | Aider |

## What's Covered

The baseline covers six security domains:

### 1. Security
- Secret/credential protection (`.env` files, API keys, tokens, passwords)
- Secret pattern detection with regex patterns for common formats
- Secure coding practices (OWASP Top 10, input validation, output encoding)
- Protected file lists (CI/CD configs, infrastructure, lock files)

### 2. Privacy
- Data minimization and PII handling
- Logging and telemetry restrictions
- GDPR/CCPA compliance considerations
- AI agent data boundary rules

### 3. Access Control
- Filesystem sandboxing (repository-only access)
- Network restriction guidance
- Command execution allowlists/blocklists
- Credential store isolation

### 4. Testing
- Test-before-commit policy
- Coverage requirements for new code
- Regression test requirements for bug fixes
- Test quality standards

### 5. Accessibility
- WCAG 2.1 AA compliance standards
- Semantic HTML requirements
- Keyboard navigation rules
- Color contrast and visual design standards

### 6. CI/CD & Supply Chain
- Pipeline protection (no unauthorized modifications)
- Dependency approval workflow
- Typosquatting prevention
- Lock file integrity

## Quick Start

### 1. Copy files to your repository

```bash
# Clone this boilerplate
git clone https://github.com/your-org/ai-repository-security-baseline.git /tmp/ai-baseline

# Copy all files to your project
cp -r /tmp/ai-baseline/AGENTS.md your-project/
cp -r /tmp/ai-baseline/CLAUDE.md your-project/
cp -r /tmp/ai-baseline/CONVENTIONS.md your-project/
cp -r /tmp/ai-baseline/devin.md your-project/
cp -r /tmp/ai-baseline/.windsurfrules your-project/
cp -r /tmp/ai-baseline/.clinerules your-project/
cp -r /tmp/ai-baseline/.continuerules your-project/
cp -r /tmp/ai-baseline/.aider.conf.yml your-project/
cp -r /tmp/ai-baseline/.aiignore your-project/
cp -r /tmp/ai-baseline/.cursorignore your-project/
cp -r /tmp/ai-baseline/.clineignore your-project/
cp -r /tmp/ai-baseline/.aiderignore your-project/
cp -r /tmp/ai-baseline/.cursor your-project/
cp -r /tmp/ai-baseline/.github your-project/
cp -r /tmp/ai-baseline/.amazonq your-project/
cp -r /tmp/ai-baseline/.junie your-project/
cp -r /tmp/ai-baseline/.sourcegraph your-project/
```

### 2. Customize AGENTS.md

Open `AGENTS.md` and fill in the `[PLACEHOLDER]` sections:

- **Project Overview** -- name, description, purpose
- **Architecture** -- patterns, components, decisions
- **Tech Stack** -- languages, frameworks, databases
- **File Structure** -- key directories and their purposes
- **Naming Conventions** -- files, classes, functions, variables
- **Git Workflow** -- branching strategy, merge policy
- **Allowed Commands** -- build, test, lint commands for your project
- **Testing Commands** -- exact test commands for your project
- **Protected Files** -- add project-specific protected files

> **Tip:** Most AI agents will offer to help fill in these sections by analyzing your repository. They'll propose changes with a before/after diff for your approval.

### 3. Customize .aider.conf.yml

Uncomment and set the test and lint commands for your project.

### 4. Remove unused tool configs

If your team doesn't use certain tools, you can safely remove their config files. The remaining configs will continue to work independently.

### 5. Commit and push

```bash
git add -A
git commit -m "Add AI agent security baseline configuration"
git push
```

## Only Use Some Tools?

Pick only what you need:

| If you use... | Copy these files |
|---------------|-----------------|
| **Any AI tool** | `AGENTS.md`, `.aiignore` |
| + Claude Code | `CLAUDE.md` |
| + Cursor | `.cursor/rules/`, `.cursorignore` |
| + GitHub Copilot | `.github/copilot-instructions.md` |
| + Windsurf | `.windsurfrules` |
| + Cline | `.clinerules`, `.clineignore` |
| + Continue.dev | `.continuerules` |
| + Aider | `.aider.conf.yml`, `CONVENTIONS.md`, `.aiderignore` |
| + Amazon Q | `.amazonq/rules/` |
| + JetBrains Junie | `.junie/guidelines.md` |
| + Devin | `devin.md` |
| + Sourcegraph Cody | `.sourcegraph/cody.json` |

## File Structure

```
your-project/
├── AGENTS.md                          # Central source of truth (all agents read this)
├── CLAUDE.md                          # Claude Code specific config
├── CONVENTIONS.md                     # Aider conventions file
├── devin.md                           # Devin instructions
├── .windsurfrules                     # Windsurf rules
├── .clinerules                        # Cline rules
├── .continuerules                     # Continue.dev rules
├── .aider.conf.yml                    # Aider configuration
├── .aiignore                          # Universal AI ignore patterns
├── .cursorignore                      # Cursor ignore patterns
├── .clineignore                       # Cline ignore patterns
├── .aiderignore                       # Aider ignore patterns
├── .cursor/
│   └── rules/
│       ├── 00-core.mdc                # Core rules (always applied)
│       ├── 01-security.mdc            # Security rules (always applied)
│       ├── 02-privacy.mdc             # Privacy rules (always applied)
│       ├── 03-testing.mdc             # Testing rules (test files)
│       ├── 04-accessibility.mdc       # Accessibility rules (UI files)
│       └── 05-dependencies.mdc        # Supply chain rules (package files)
├── .github/
│   └── copilot-instructions.md        # GitHub Copilot instructions
├── .amazonq/
│   └── rules/
│       └── security.md                # Amazon Q security rules
├── .junie/
│   └── guidelines.md                  # JetBrains Junie guidelines
└── .sourcegraph/
    └── cody.json                      # Sourcegraph Cody config
```

## Design Principles

1. **Single source of truth** -- `AGENTS.md` contains all shared rules. Tool-specific files reference it.
2. **Defense in depth** -- Rules are enforced at multiple layers: instruction files, ignore files, and the rules themselves tell agents to self-restrict.
3. **Secure by default** -- Everything is restricted unless explicitly allowed.
4. **Customizable** -- Placeholder sections make it easy to adapt to any project.
5. **Agent self-governance** -- Agents must ask permission before modifying any rules, and must show before/after diffs.
6. **Privacy by design** -- Privacy rules are baked in, not bolted on.

## Security Model

```
┌─────────────────────────────────────────────────┐
│                   AGENTS.md                      │
│            (Central Source of Truth)              │
│                                                   │
│  Security | Privacy | Access | Testing | A11y     │
│  CI/CD | Dependencies | Code Quality             │
├─────────────────────────────────────────────────┤
│                                                   │
│  ┌──────────┐ ┌──────────┐ ┌──────────────────┐ │
│  │ CLAUDE.md│ │.cursor/  │ │copilot-          │ │
│  │          │ │rules/    │ │instructions.md   │ │
│  └──────────┘ └──────────┘ └──────────────────┘ │
│  ┌──────────┐ ┌──────────┐ ┌──────────────────┐ │
│  │.windsurf │ │.cline    │ │.continue         │ │
│  │rules     │ │rules     │ │rules             │ │
│  └──────────┘ └──────────┘ └──────────────────┘ │
│  ┌──────────┐ ┌──────────┐ ┌──────────────────┐ │
│  │devin.md  │ │.amazonq/ │ │.junie/           │ │
│  │          │ │rules/    │ │guidelines.md     │ │
│  └──────────┘ └──────────┘ └──────────────────┘ │
│                                                   │
├─────────────────────────────────────────────────┤
│              Ignore Files Layer                   │
│  .aiignore | .cursorignore | .clineignore | ...  │
│  (Prevent agents from reading sensitive files)    │
└─────────────────────────────────────────────────┘
```

## Contributing

Contributions are welcome. When proposing changes:

1. Ensure changes maintain consistency across all tool-specific files.
2. Update `AGENTS.md` first, then propagate to tool-specific files.
3. Test with at least one AI tool to verify the rules are picked up correctly.
4. Follow the security model: restrictive by default, explicit allowlisting.

## References

- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [OWASP Agentic AI Threats](https://owasp.org/www-project-agentic-ai-threats/)
- [SLSA Framework](https://slsa.dev/)
- [OpenSSF Scorecard](https://securityscorecards.dev/)
- [NIST AI Risk Management Framework](https://www.nist.gov/artificial-intelligence/risk-management-framework)

## License

Apache License 2.0 -- see [LICENSE](LICENSE) for details.
