# AI Agent Tool Inventory

> Document every tool, MCP server, IDE extension, and plugin available to AI agents in this project.
> This inventory is required by AGENTS.md §9.5. Review quarterly and remove tools that are no longer needed.

---

## Why This Inventory Exists

The model isn't the threat surface — its tools are. MCP servers, shell access, and permission-bypass flags convert natural language into arbitrary code execution. This inventory ensures you know exactly what tools each agent has access to, what those tools can do, and who approved that access.

---

## Inventory

### Claude Code

| Tool / MCP Server | Purpose | Access Level | Config Location | Approved By | Date |
|-------------------|---------|-------------|-----------------|-------------|------|
| [PLACEHOLDER] | [PLACEHOLDER] | read-only / read-write | [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] |

### Cursor

| Extension / Tool | Purpose | Access Level | Config Location | Approved By | Date |
|-----------------|---------|-------------|-----------------|-------------|------|
| [PLACEHOLDER] | [PLACEHOLDER] | read-only / read-write | [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] |

### GitHub Copilot

| Feature / Integration | Purpose | Access Level | Config Location | Approved By | Date |
|----------------------|---------|-------------|-----------------|-------------|------|
| [PLACEHOLDER] | [PLACEHOLDER] | read-only / read-write | [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] |

### Windsurf (Codeium)

| Extension / Tool | Purpose | Access Level | Config Location | Approved By | Date |
|-----------------|---------|-------------|-----------------|-------------|------|
| [PLACEHOLDER] | [PLACEHOLDER] | read-only / read-write | [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] |

### Cline

| Tool / MCP Server | Purpose | Access Level | Config Location | Approved By | Date |
|-------------------|---------|-------------|-----------------|-------------|------|
| [PLACEHOLDER] | [PLACEHOLDER] | read-only / read-write | [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] |

### Aider

| Feature / Tool | Purpose | Access Level | Config Location | Approved By | Date |
|---------------|---------|-------------|-----------------|-------------|------|
| [PLACEHOLDER] | [PLACEHOLDER] | read-only / read-write | [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] |

### [PLACEHOLDER — Add Other Agents]

| Tool / MCP Server | Purpose | Access Level | Config Location | Approved By | Date |
|-------------------|---------|-------------|-----------------|-------------|------|
| [PLACEHOLDER] | [PLACEHOLDER] | read-only / read-write | [PLACEHOLDER] | [PLACEHOLDER] | [PLACEHOLDER] |

---

## Dangerous Capabilities Checklist

Answer every question below. If the answer is "yes", document the justification and ensure the access follows least-privilege principles.

- [ ] **Shell access:** Can any agent execute arbitrary shell commands? If yes, which agents and why?
  - _Agents:_ [PLACEHOLDER]
  - _Justification:_ [PLACEHOLDER]
  - _Mitigations:_ [PLACEHOLDER — e.g., "Commands restricted to allowed list in AGENTS.md §9.4"]

- [ ] **Network access:** Can any agent make outbound network requests? If yes, to which endpoints?
  - _Agents:_ [PLACEHOLDER]
  - _Allowed endpoints:_ [PLACEHOLDER]
  - _Mitigations:_ [PLACEHOLDER]

- [ ] **File system writes:** Can any agent modify files outside the repository? If yes, why?
  - _Agents:_ [PLACEHOLDER]
  - _Scope:_ [PLACEHOLDER]
  - _Mitigations:_ [PLACEHOLDER]

- [ ] **Credential access:** Can any agent access secrets, tokens, or credentials? If yes, how is this scoped?
  - _Agents:_ [PLACEHOLDER]
  - _Credentials accessible:_ [PLACEHOLDER]
  - _Mitigations:_ [PLACEHOLDER — e.g., "OIDC only, no static secrets"]

- [ ] **Read-only defaults:** Are all data-source tools (database, drive, mail, wiki) set to read-only?
  - _Status:_ [PLACEHOLDER — yes/no]
  - _Exceptions:_ [PLACEHOLDER]

- [ ] **Dangerous flags stripped:** Are wrapper scripts in place to strip `--force`, `--no-verify`, `--dangerously-skip-permissions`, and similar flags?
  - _Status:_ [PLACEHOLDER — yes/no]
  - _Wrapper locations:_ [PLACEHOLDER]

- [ ] **MCP config security:** Are all MCP server config files free of embedded credentials? Are they listed in `.aiignore`?
  - _Status:_ [PLACEHOLDER — yes/no]
  - _Files checked:_ [PLACEHOLDER]

---

## Review Log

| Date | Reviewer | Changes Made | Next Review |
|------|----------|-------------|-------------|
| [PLACEHOLDER] | [PLACEHOLDER] | Initial inventory | [PLACEHOLDER — 3 months from now] |
