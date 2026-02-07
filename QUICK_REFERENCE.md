# Quick Reference

This is a quick reference guide for common tasks and commands in the Claude Code Toolkit.

## Installation

```bash
# Method 1: Plugin Marketplace (Recommended)
/plugin marketplace add rohitg00/awesome-claude-code-toolkit

# Method 2: Manual Clone
git clone https://github.com/rohitg00/awesome-claude-code-toolkit.git ~/.claude/plugins/claude-code-toolkit

# Method 3: Automated Installer
curl -fsSL https://raw.githubusercontent.com/rohitg00/awesome-claude-code-toolkit/main/setup/install.sh | bash
```

## Most Used Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `/commit` | Create conventional commit | `/commit` then describe changes |
| `/tdd` | Test-driven development | `/tdd src/utils/parser.ts` |
| `/debug` | Systematic debugging | `/debug` then describe the bug |
| `/doc-gen` | Generate documentation | `/doc-gen` for current file |
| `/review` | Code review | `/review` for current changes |
| `/audit` | Security audit | `/audit` for security scan |
| `/deploy` | Deploy to environment | `/deploy staging` |

## Context Switching

```bash
/context load dev        # Development mode - fast iteration
/context load review     # Review mode - find issues
/context load debug      # Debug mode - fix bugs
/context load deploy     # Deploy mode - safe releases
/context load research   # Research mode - evaluate options
```

## Common Agents

| Agent | Expertise | Example Usage |
|-------|-----------|---------------|
| `backend-developer` | Node.js, Express, Fastify | Building REST APIs |
| `frontend-architect` | React, Next.js | Component architecture |
| `database-expert` | PostgreSQL, optimization | Query optimization |
| `security-specialist` | Security, vulnerabilities | Security review |
| `devops-engineer` | CI/CD, deployment | Pipeline setup |
| `python-expert` | Python, FastAPI, Django | Python development |
| `rust-systems-engineer` | Rust, systems programming | Performance-critical code |

**Usage**: Just mention the agent in your prompt:
```
Can the backend-developer agent help me build a user authentication API?
```

## File Locations

```bash
~/.claude/                           # Global configuration
├── commands/                        # Slash commands
├── rules/                          # Coding standards
├── hooks.json                      # Hook configuration
├── hooks/scripts/                  # Hook scripts
├── contexts/                       # Working contexts
├── templates/                      # CLAUDE.md templates
└── mcp.json                        # MCP server config

your-project/                        # Project-specific
├── CLAUDE.md                       # Project context file
└── .claude/                        # Project overrides
    ├── rules/                      # Project rules
    ├── hooks.json                  # Project hooks
    └── mcp.json                    # Project MCP config
```

## Project Setup Checklist

```bash
# 1. Copy a template
cd your-project/
cp ~/.claude/plugins/claude-code-toolkit/templates/claude-md/standard.md ./CLAUDE.md

# 2. Customize CLAUDE.md with your project details
# Edit: tech stack, structure, commands, conventions

# 3. Add rules (optional)
mkdir -p .claude/rules
cp ~/.claude/rules/coding-style.md .claude/rules/
cp ~/.claude/rules/testing.md .claude/rules/

# 4. Configure hooks (optional)
mkdir -p .claude/hooks/scripts
cp ~/.claude/hooks.json .claude/hooks.json
cp ~/.claude/hooks/scripts/* .claude/hooks/scripts/

# 5. Set up MCP (optional)
cp ~/.claude/mcp.json .claude/mcp.json
# Edit with project-specific credentials
```

## Hook Configuration

Common hook patterns in `hooks.json`:

```json
{
  "SessionStart": {
    "command": "node ~/.claude/hooks/scripts/session-start.js",
    "timeout": 5000
  },
  "PreToolUse": {
    "Bash": {
      "command": "node ~/.claude/hooks/scripts/pre-push-check.js ${command}",
      "timeout": 3000
    }
  },
  "PostToolUse": {
    "Write": {
      "command": "node ~/.claude/hooks/scripts/post-edit-check.js ${filePath}",
      "timeout": 10000
    },
    "Edit": {
      "command": "node ~/.claude/hooks/scripts/post-edit-check.js ${filePath}",
      "timeout": 10000
    }
  }
}
```

## Rules Setup

Recommended rules to start with:

```bash
# Essential rules
cp ~/.claude/rules/coding-style.md ~/.claude/rules/
cp ~/.claude/rules/testing.md ~/.claude/rules/
cp ~/.claude/rules/security.md ~/.claude/rules/
cp ~/.claude/rules/git-workflow.md ~/.claude/rules/
cp ~/.claude/rules/error-handling.md ~/.claude/rules/
```

## MCP Server Quick Setup

**GitHub Integration**:
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**PostgreSQL Integration**:
```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "POSTGRES_CONNECTION_STRING": "${DATABASE_URL}"
      }
    }
  }
}
```

## Common Workflows

### Feature Development
```
1. /context load dev
2. Plan the feature with agent
3. /tdd src/feature.ts
4. Implement incrementally
5. /context load review
6. Self-review
7. /commit
8. Create PR
```

### Debugging
```
1. /context load debug
2. /debug
3. Describe symptoms
4. Follow systematic debugging
5. Add regression test
6. /commit
```

### Code Review
```
1. /context load review
2. Review changes for issues
3. Check security, edge cases
4. Verify tests
5. Provide feedback
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Commands not found | Check `~/.claude/commands/` exists, restart Claude |
| Hooks not running | Verify `~/.claude/hooks.json` exists with absolute paths |
| Rules not applied | Check `~/.claude/rules/*.md` files exist |
| MCP errors | Verify credentials in `mcp.json`, check server logs |
| Permission denied | Run `chmod -R 755 ~/.claude/` |

## Getting Help

| Resource | Link |
|----------|------|
| Full Documentation | [GETTING_STARTED.md](GETTING_STARTED.md) |
| Setup Instructions | [SETUP.md](SETUP.md) |
| Usage Examples | [USAGE_GUIDE.md](USAGE_GUIDE.md) |
| Architecture | [CONCEPTS.md](CONCEPTS.md) |
| Examples | [examples/](examples/) |
| Issues | [GitHub Issues](https://github.com/rohitg00/awesome-claude-code-toolkit/issues) |
| Contributing | [CONTRIBUTING.md](CONTRIBUTING.md) |

## Useful Prompts

### Starting a Session
```
What rules, hooks, and MCP servers are active?
Load the dev context and show me the project status.
```

### Planning
```
Plan the implementation for [feature] before we start coding.
What's the best approach for [problem]? Consider pros and cons.
```

### Code Quality
```
Review my changes for security issues, edge cases, and test coverage.
Run linter and fix any issues automatically.
```

### Documentation
```
Generate API documentation for [file/service].
Update the README with usage examples for the new feature.
```

### Debugging
```
Trace the execution path for [issue].
What are the possible causes for [symptom]?
```

## Keyboard Shortcuts

When using Claude Code with this toolkit:

- **Load context quickly**: Create aliases for context switches
- **Quick commands**: Use command history (up arrow)
- **Multi-command**: Chain commands with `&&`

```bash
# Example: Lint, test, and commit
Run linter && run tests && create commit
```

## Environment Variables

Recommended environment variables for MCP servers:

```bash
# GitHub
export GITHUB_TOKEN="your_token"

# Databases
export DATABASE_URL="postgresql://..."
export REDIS_URL="redis://..."

# AWS
export AWS_ACCESS_KEY_ID="..."
export AWS_SECRET_ACCESS_KEY="..."

# Others
export OPENAI_API_KEY="..."
```

## Tips & Tricks

1. **Start Simple**: Begin with 2-3 commands, add more as needed
2. **Use Contexts**: Switch contexts frequently for better results
3. **Leverage Agents**: Mention agents by name for expert guidance
4. **Chain Commands**: Combine related commands in one prompt
5. **Customize**: Adapt templates and rules to your team's needs
6. **Document**: Keep CLAUDE.md updated as project evolves
7. **Automate**: Use hooks to enforce quality checks
8. **Review**: Always review with `/context load review` before PRs

## Version Check

To verify your installation:

```bash
# Check files
ls -la ~/.claude/

# Verify plugins
ls ~/.claude/plugins/claude-code-toolkit/

# Test a command
/commit
```

## Update Toolkit

```bash
# Plugin marketplace
/plugin update claude-code-toolkit

# Manual installation
cd ~/.claude/plugins/claude-code-toolkit
git pull origin main
```

---

**Pro tip**: Bookmark this page for quick reference during development sessions!

For comprehensive guides, see:
- 📖 [Getting Started](GETTING_STARTED.md)
- 🔧 [Setup Guide](SETUP.md)
- 🎓 [Usage Guide](USAGE_GUIDE.md)
- 🏗️ [Concepts](CONCEPTS.md)
