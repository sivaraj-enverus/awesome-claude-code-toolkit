# Getting Started with Claude Code Toolkit

Welcome to the **Awesome Claude Code Toolkit** - the most comprehensive collection of productivity tools for Claude Code. This guide will help you understand what this toolkit is, why you need it, and how to get started quickly.

## What is Claude Code Toolkit?

Claude Code Toolkit is a curated collection of 796 files designed to supercharge your development workflow with Claude Code. It includes:

- **120 Plugins**: Domain-specific capabilities for everything from API design to security auditing
- **135 Agents**: Specialized AI agents for different development tasks
- **35 Skills**: Curated knowledge modules for best practices and patterns
- **42 Commands**: Slash commands for common workflows
- **19 Hooks**: Automation scripts that trigger at different lifecycle events
- **15 Rules**: Coding standards and conventions
- **7 Templates**: Ready-to-use CLAUDE.md templates
- **6 MCP Configs**: Model Context Protocol server configurations
- **5 Contexts**: Different working modes for Claude Code
- **3 Examples**: Real-world usage walkthroughs

## Why Use This Toolkit?

### Problems It Solves

1. **Inconsistent Code Quality**: Rules and hooks enforce standards automatically
2. **Repetitive Tasks**: Commands and plugins automate common workflows
3. **Context Switching**: Agents specialize in specific domains, providing expert guidance
4. **Configuration Overhead**: Templates and configs get you started quickly
5. **Knowledge Gaps**: Skills provide best practices and patterns for various technologies

### Benefits

- ⚡ **Faster Development**: Automate repetitive tasks with commands and plugins
- 🎯 **Better Quality**: Enforce standards with rules and hooks
- 🧠 **Expert Guidance**: Leverage specialized agents for domain-specific tasks
- 📚 **Learn Best Practices**: Access curated skills and patterns
- 🔧 **Consistent Setup**: Use templates for new projects

## Key Concepts

### 1. Plugins

Plugins are self-contained extensions that add specific capabilities to Claude Code. Each plugin lives in its own directory and includes:
- Command files (`.md`) that define slash commands
- A `plugin.json` manifest describing the plugin
- Optional configuration and documentation

**Example**: The `bug-detective` plugin provides `/debug` and `/trace` commands for systematic debugging.

### 2. Agents

Agents are specialized AI personalities with specific expertise. They use particular tools, models, and instructions to excel in their domain.

**Example**: The `backend-developer` agent is a Node.js expert that builds APIs using Express/Fastify with best practices.

### 3. Skills

Skills are knowledge modules that teach best practices, patterns, and techniques for specific technologies or domains.

**Example**: The `tdd-mastery` skill teaches test-driven development practices.

### 4. Commands

Commands are slash commands (like `/commit`, `/tdd`) that trigger specific workflows. They're defined as markdown files with instructions.

**Example**: `/commit` guides you through creating a conventional commit with proper formatting.

### 5. Hooks

Hooks are automation scripts that run at specific lifecycle events:
- **SessionStart**: When you start a Claude Code session
- **SessionEnd**: When you end a session
- **PreToolUse**: Before Claude uses a tool
- **PostToolUse**: After Claude uses a tool
- **Stop**: When you stop Claude's execution

**Example**: The `post-edit-check.js` hook runs your linter automatically after file edits.

### 6. Rules

Rules are coding standards and conventions that Claude follows. They define how to write code, structure projects, handle errors, etc.

**Example**: The `coding-style.md` rule enforces naming conventions and file organization.

### 7. Templates

Templates are starter CLAUDE.md files for different project types. CLAUDE.md is a special file that gives Claude context about your project.

**Example**: The `fullstack-app.md` template is for Next.js + API applications.

### 8. MCP Configs

MCP (Model Context Protocol) configurations define which external servers Claude can connect to for additional capabilities.

**Example**: The `fullstack.json` config enables GitHub, PostgreSQL, Redis, and Puppeteer servers.

### 9. Contexts

Contexts are working modes that configure Claude's behavior for different tasks.

**Example**: The `dev.md` context optimizes for fast iteration, while `review.md` focuses on finding issues.

## Quick Start

### Prerequisites

- Claude Code installed and running
- Basic familiarity with command line
- Git installed (for manual installation)

### Installation

Choose one of three methods:

#### Method 1: Plugin Marketplace (Recommended)

```bash
/plugin marketplace add rohitg00/awesome-claude-code-toolkit
```

#### Method 2: Manual Clone

```bash
git clone https://github.com/rohitg00/awesome-claude-code-toolkit.git ~/.claude/plugins/claude-code-toolkit
```

#### Method 3: One-Liner Script

```bash
curl -fsSL https://raw.githubusercontent.com/rohitg00/awesome-claude-code-toolkit/main/setup/install.sh | bash
```

### Next Steps

1. **Read**: [SETUP.md](SETUP.md) for detailed installation and configuration
2. **Explore**: [USAGE_GUIDE.md](USAGE_GUIDE.md) for practical examples
3. **Learn**: [CONCEPTS.md](CONCEPTS.md) for deeper understanding
4. **Try**: Check out the [examples/](examples/) directory for real-world workflows

## Your First Session

After installation, try this simple workflow:

1. **Load a context**:
   ```
   /context load dev
   ```

2. **Use a command**:
   ```
   /commit
   ```

3. **Ask an agent for help**:
   ```
   I need to build a REST API with authentication. Can the backend-developer agent help me plan this?
   ```

4. **Check what's loaded**:
   ```
   What rules, hooks, and MCP servers are active in this project?
   ```

## Common Use Cases

### Starting a New Project

1. Copy a CLAUDE.md template for your project type
2. Add relevant rules to `.claude/rules/`
3. Copy hooks to `.claude/hooks/`
4. Configure MCP servers if needed

See [examples/project-setup.md](examples/project-setup.md) for a complete walkthrough.

### Daily Development

1. Start session with `/context load dev`
2. Use commands like `/tdd`, `/commit`, `/docs` for common tasks
3. Leverage agents for specialized work
4. Let hooks enforce quality automatically

See [examples/session-workflow.md](examples/session-workflow.md) for a typical session.

### Code Review

1. Switch to review context: `/context load review`
2. Use the `code-review-assistant` plugin
3. Let the reviewer agent inspect changes
4. Address feedback systematically

### Debugging

1. Load debug context: `/context load debug`
2. Use `/debug` command from bug-detective plugin
3. Follow the systematic debugging workflow
4. Add regression tests

## Learning Path

### Beginner (Week 1)
- Install the toolkit
- Try basic commands (`/commit`, `/docs`)
- Use one template for a project
- Explore 2-3 plugins

### Intermediate (Week 2-4)
- Configure hooks for your workflow
- Use agents for specialized tasks
- Add custom rules for your team
- Experiment with MCP servers

### Advanced (Month 2+)
- Create custom plugins
- Chain multiple agents together
- Customize hooks and contexts
- Integrate with your CI/CD

## Troubleshooting

### Commands Not Working

- Verify installation: Check that files exist in `~/.claude/commands/`
- Restart Claude Code after installation
- Check command syntax in the `.md` files

### Hooks Not Triggering

- Verify `hooks.json` is in `~/.claude/`
- Check script paths are absolute
- Ensure Node.js is installed for JavaScript hooks

### Agents Not Available

- Agents are context-specific, not slash commands
- Reference them by name in your prompts
- Check the agent `.md` file for correct usage

## Getting Help

1. **Documentation**: Read the detailed guides in this repository
2. **Examples**: Check the [examples/](examples/) directory
3. **Issues**: Open an issue on GitHub
4. **Contributing**: See [CONTRIBUTING.md](CONTRIBUTING.md)

## What's Next?

- 📖 **[SETUP.md](SETUP.md)** - Detailed installation and configuration guide
- 🎓 **[USAGE_GUIDE.md](USAGE_GUIDE.md)** - Practical examples and workflows
- 🏗️ **[CONCEPTS.md](CONCEPTS.md)** - Deep dive into architecture and design
- 🤝 **[CONTRIBUTING.md](CONTRIBUTING.md)** - How to contribute to the toolkit

---

**Ready to supercharge your development workflow?** Continue to [SETUP.md](SETUP.md) for detailed installation instructions.
