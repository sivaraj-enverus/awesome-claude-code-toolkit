# Setup Guide

This guide covers everything you need to install, configure, and verify the Claude Code Toolkit.

## Table of Contents

- [System Requirements](#system-requirements)
- [Installation Methods](#installation-methods)
- [Post-Installation Configuration](#post-installation-configuration)
- [Verification](#verification)
- [Project-Specific Setup](#project-specific-setup)
- [Troubleshooting](#troubleshooting)

## System Requirements

### Required

- **Claude Code**: Latest version installed and running
- **Operating System**: macOS or Linux (Windows via WSL2)
- **Command Line**: Terminal access with bash

### Optional (for full functionality)

- **Node.js**: v18+ (for hooks and some plugins)
- **Git**: v2.20+ (for git-related commands)
- **Package Managers**: npm, pnpm, or yarn
- **Docker**: For containerization plugins
- **MCP Servers**: Specific requirements per server

## Installation Methods

### Method 1: Plugin Marketplace (Recommended)

The easiest way to install is through Claude Code's plugin marketplace.

**Steps:**

1. Open Claude Code
2. Run the marketplace command:
   ```bash
   /plugin marketplace add rohitg00/awesome-claude-code-toolkit
   ```
3. Claude will download and install the toolkit automatically
4. Restart Claude Code to activate

**Pros:**
- Simplest method
- Automatic updates
- Integrated with Claude Code

**Cons:**
- Requires internet connection
- May not have the latest unreleased features

### Method 2: Manual Clone

For more control and to access the latest development version.

**Steps:**

1. Clone the repository:
   ```bash
   git clone https://github.com/rohitg00/awesome-claude-code-toolkit.git ~/.claude/plugins/claude-code-toolkit
   ```

2. Verify the installation:
   ```bash
   ls ~/.claude/plugins/claude-code-toolkit
   ```

3. Restart Claude Code

**Pros:**
- Full control over version
- Easy to update with `git pull`
- Can modify files locally

**Cons:**
- Manual updates required
- Requires git knowledge

### Method 3: Automated Installer

Use the provided installation script for interactive setup.

**Steps:**

1. Download and run the installer:
   ```bash
   curl -fsSL https://raw.githubusercontent.com/rohitg00/awesome-claude-code-toolkit/main/setup/install.sh | bash
   ```

2. Follow the interactive prompts:
   - Install commands? (Y/n)
   - Install hooks? (Y/n)
   - Install rules? (Y/n)
   - Install templates? (Y/n)
   - Copy MCP configs? (Y/n)

3. Review the installation summary

**What it does:**
- Creates `~/.claude/` directory structure
- Copies selected components
- Sets up proper permissions
- Provides next steps

**Pros:**
- Interactive and guided
- Selective installation
- Creates proper directory structure

**Cons:**
- Requires curl and bash
- Less control than manual method

## Post-Installation Configuration

After installation, configure the toolkit for your needs.

### 1. Configure Commands

Commands are installed to `~/.claude/commands/` by category:

```bash
~/.claude/commands/
├── architecture/
├── devops/
├── documentation/
├── git/
├── refactoring/
├── security/
├── testing/
└── workflow/
```

**To use commands:**
```
/commit           # Git commands
/tdd              # Testing commands
/doc-gen          # Documentation commands
/audit            # Security commands
```

**To customize:**
Edit the `.md` files in each category directory.

### 2. Configure Hooks

Hooks are installed with the `hooks.json` configuration file.

**Setup:**

1. Copy hooks configuration:
   ```bash
   cp ~/.claude/plugins/claude-code-toolkit/hooks/hooks.json ~/.claude/hooks.json
   ```

2. Copy hook scripts:
   ```bash
   mkdir -p ~/.claude/hooks/scripts
   cp ~/.claude/plugins/claude-code-toolkit/hooks/scripts/* ~/.claude/hooks/scripts/
   ```

3. Update paths in `hooks.json` to use absolute paths:
   ```json
   {
     "SessionStart": {
       "command": "node /home/username/.claude/hooks/scripts/session-start.js"
     }
   }
   ```

**Available Hooks:**

| Event | Purpose | Scripts |
|-------|---------|---------|
| SessionStart | Load context, check status | `session-start.js`, `context-loader.js` |
| SessionEnd | Save state, log learnings | `session-end.js`, `learning-log.js` |
| PreToolUse | Validate before actions | `pre-push-check.js`, `commit-guard.js`, `secret-scanner.js` |
| PostToolUse | Verify after actions | `post-edit-check.js`, `auto-test.js`, `lint-fix.js` |
| Stop | Final checks | `stop-check.js` |

### 3. Configure Rules

Rules enforce coding standards automatically.

**Setup:**

1. Create rules directory:
   ```bash
   mkdir -p ~/.claude/rules
   ```

2. Copy rules you want:
   ```bash
   cp ~/.claude/plugins/claude-code-toolkit/rules/coding-style.md ~/.claude/rules/
   cp ~/.claude/plugins/claude-code-toolkit/rules/security.md ~/.claude/rules/
   cp ~/.claude/plugins/claude-code-toolkit/rules/testing.md ~/.claude/rules/
   ```

**Recommended Rules:**
- `coding-style.md` - Naming and formatting
- `git-workflow.md` - Commit and branch conventions
- `testing.md` - Test structure and coverage
- `security.md` - Security best practices
- `error-handling.md` - Error handling patterns

Claude Code automatically loads and applies all rules in `~/.claude/rules/`.

### 4. Configure MCP Servers

MCP servers provide Claude with access to external tools and services.

**Setup:**

1. Choose a configuration:
   ```bash
   # For full-stack development
   cp ~/.claude/plugins/claude-code-toolkit/mcp-configs/fullstack.json ~/.claude/mcp.json
   
   # For frontend only
   cp ~/.claude/plugins/claude-code-toolkit/mcp-configs/frontend.json ~/.claude/mcp.json
   
   # For DevOps
   cp ~/.claude/plugins/claude-code-toolkit/mcp-configs/devops.json ~/.claude/mcp.json
   ```

2. Edit `~/.claude/mcp.json` with your credentials:
   ```json
   {
     "mcpServers": {
       "github": {
         "command": "npx",
         "args": ["-y", "@modelcontextprotocol/server-github"],
         "env": {
           "GITHUB_PERSONAL_ACCESS_TOKEN": "your_token_here"
         }
       }
     }
   }
   ```

3. **Security Note**: Never commit real credentials. Use environment variables:
   ```json
   "env": {
     "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
   }
   ```

**Available Configurations:**

| Config | Use Case | Servers |
|--------|----------|---------|
| `recommended.json` | General development | 14 essential servers |
| `fullstack.json` | Web applications | GitHub, PostgreSQL, Redis, Puppeteer |
| `frontend.json` | UI development | Puppeteer, Figma, Storybook |
| `devops.json` | Infrastructure | AWS, Docker, GitHub, Terraform |
| `data-science.json` | Data work | Jupyter, SQLite, PostgreSQL |
| `kubernetes.json` | Container orchestration | kubectl, Docker, GitHub |

### 5. Configure Contexts

Contexts change Claude's behavior for different working modes.

**Setup:**

```bash
mkdir -p ~/.claude/contexts
cp ~/.claude/plugins/claude-code-toolkit/contexts/* ~/.claude/contexts/
```

**Using Contexts:**

```
/context load dev        # Development mode
/context load review     # Code review mode
/context load debug      # Debugging mode
/context load deploy     # Deployment mode
/context load research   # Research mode
```

Each context optimizes Claude's responses for specific tasks.

## Verification

Verify your installation is working correctly.

### 1. Check File Structure

```bash
ls -la ~/.claude/
```

You should see:
```
.claude/
├── commands/          # Command files
├── hooks.json         # Hook configuration
├── hooks/
│   └── scripts/       # Hook scripts
├── rules/             # Rule files
├── contexts/          # Context files
├── templates/         # CLAUDE.md templates
└── mcp.json           # MCP configuration (optional)
```

### 2. Test Commands

Open Claude Code and try:

```
/commit
```

You should see the commit workflow guidance.

### 3. Test Contexts

```
/context load dev
```

Claude should acknowledge the context change.

### 4. Verify Rules

Ask Claude:

```
What coding rules are currently active?
```

Claude should list the rules from `~/.claude/rules/`.

### 5. Check MCP Servers

```
What MCP servers are available?
```

Claude should list the configured servers.

### 6. Test Hooks (if configured)

Start a new session. If `session-start.js` is configured, you should see initialization output.

## Project-Specific Setup

To use the toolkit in a specific project:

### 1. Create CLAUDE.md

Copy a template:

```bash
cd your-project/
cp ~/.claude/plugins/claude-code-toolkit/templates/claude-md/standard.md ./CLAUDE.md
```

Edit `CLAUDE.md` to describe your project:
- Tech stack
- Project structure
- Development commands
- Testing approach
- Deployment process

### 2. Add Project Rules

```bash
mkdir -p .claude/rules
cp ~/.claude/rules/coding-style.md .claude/rules/
cp ~/.claude/rules/testing.md .claude/rules/
# Add more as needed
```

### 3. Configure Project Hooks

```bash
mkdir -p .claude/hooks/scripts
cp ~/.claude/hooks.json .claude/hooks.json
cp ~/.claude/hooks/scripts/* .claude/hooks/scripts/
```

Update paths in `.claude/hooks.json` to use relative paths.

### 4. Set Up MCP for Project

```bash
cp ~/.claude/mcp.json .claude/mcp.json
```

Edit `.claude/mcp.json` with project-specific settings.

**Important**: Add `.claude/mcp.json` to `.gitignore` if it contains credentials.

### 5. Choose Project Contexts

```bash
mkdir -p .claude/contexts
cp ~/.claude/contexts/dev.md .claude/contexts/
cp ~/.claude/contexts/review.md .claude/contexts/
```

## Troubleshooting

### Commands Not Found

**Problem**: `/commit` shows "command not found"

**Solutions**:
1. Check installation:
   ```bash
   ls ~/.claude/commands/git/commit.md
   ```
2. Restart Claude Code
3. Verify file permissions:
   ```bash
   chmod -R 755 ~/.claude/commands/
   ```

### Hooks Not Running

**Problem**: Hooks aren't triggering

**Solutions**:
1. Verify `hooks.json` exists:
   ```bash
   cat ~/.claude/hooks.json
   ```
2. Check script paths are absolute and correct
3. Test script manually:
   ```bash
   node ~/.claude/hooks/scripts/session-start.js
   ```
4. Verify Node.js is installed:
   ```bash
   node --version
   ```

### Rules Not Applied

**Problem**: Claude isn't following rules

**Solutions**:
1. Check rules directory:
   ```bash
   ls ~/.claude/rules/
   ```
2. Verify file contents aren't corrupted
3. Ask Claude to list rules:
   ```
   What rules are active?
   ```
4. Restart Claude Code

### MCP Servers Failing

**Problem**: "MCP server connection failed"

**Solutions**:
1. Verify credentials in `mcp.json`
2. Test server availability:
   ```bash
   npx -y @modelcontextprotocol/server-github
   ```
3. Check environment variables are set
4. Review server logs

### Permission Issues

**Problem**: "Permission denied" errors

**Solutions**:
1. Fix ownership:
   ```bash
   chown -R $USER ~/.claude/
   ```
2. Set correct permissions:
   ```bash
   chmod -R 755 ~/.claude/
   chmod 644 ~/.claude/**/*.md
   chmod 755 ~/.claude/**/*.js
   ```

### Conflicts with Existing Setup

**Problem**: Toolkit conflicts with existing configuration

**Solutions**:
1. Back up existing configuration:
   ```bash
   mv ~/.claude ~/.claude.backup
   ```
2. Install fresh
3. Merge configurations manually
4. Use project-specific setup instead of global

## Updating the Toolkit

### Plugin Marketplace Installation

Updates happen automatically, or manually trigger:

```bash
/plugin update claude-code-toolkit
```

### Manual Installation

```bash
cd ~/.claude/plugins/claude-code-toolkit
git pull origin main
```

### After Updating

1. Review changelog for breaking changes
2. Update your configurations if needed
3. Restart Claude Code
4. Test critical workflows

## Uninstalling

### Remove Plugin

```bash
rm -rf ~/.claude/plugins/claude-code-toolkit
```

### Remove Configuration

```bash
rm -rf ~/.claude/commands/
rm -rf ~/.claude/hooks/
rm -rf ~/.claude/rules/
rm -rf ~/.claude/contexts/
rm -rf ~/.claude/templates/
rm ~/.claude/hooks.json
rm ~/.claude/mcp.json
```

Or keep `~/.claude/` for other plugins and only remove toolkit-specific files.

## Next Steps

Now that your toolkit is installed and configured:

1. **[USAGE_GUIDE.md](USAGE_GUIDE.md)** - Learn how to use each component
2. **[CONCEPTS.md](CONCEPTS.md)** - Understand the architecture
3. **[examples/](examples/)** - Try real-world workflows
4. Start using the toolkit in your projects!

---

**Having issues?** Open an issue on [GitHub](https://github.com/rohitg00/awesome-claude-code-toolkit/issues) or check the [troubleshooting section](#troubleshooting).
