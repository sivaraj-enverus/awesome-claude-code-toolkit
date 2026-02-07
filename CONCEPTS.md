# Concepts and Architecture

This guide provides a deep understanding of how the Claude Code Toolkit works, its design principles, and architectural decisions.

## Table of Contents

- [Design Philosophy](#design-philosophy)
- [Architecture Overview](#architecture-overview)
- [Component Deep Dive](#component-deep-dive)
- [How Components Interact](#how-components-interact)
- [Extension Points](#extension-points)
- [Best Practices](#best-practices)
- [Advanced Patterns](#advanced-patterns)

## Design Philosophy

### Core Principles

1. **Modularity**: Each component is self-contained and composable
2. **Convention over Configuration**: Sensible defaults with customization options
3. **Progressive Enhancement**: Start simple, add complexity as needed
4. **Developer Experience**: Optimize for productivity and ease of use
5. **Quality by Default**: Automate quality checks and best practices

### Design Goals

- **Discoverability**: Easy to find and understand what's available
- **Consistency**: Predictable structure across all components
- **Flexibility**: Adapt to different workflows and preferences
- **Performance**: Fast execution with minimal overhead
- **Maintainability**: Clear organization and documentation

## Architecture Overview

### System Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                       Claude Code                            │
│                                                              │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐            │
│  │  Context   │  │   Rules    │  │   Hooks    │            │
│  │   Layer    │  │   Engine   │  │  Runtime   │            │
│  └────────────┘  └────────────┘  └────────────┘            │
│         │               │               │                    │
│         └───────────────┴───────────────┘                    │
│                         │                                    │
│              ┌──────────┴──────────┐                        │
│              │                     │                        │
│         ┌────▼────┐          ┌────▼────┐                   │
│         │ Agents  │          │Commands │                   │
│         │         │          │         │                   │
│         └────┬────┘          └────┬────┘                   │
│              │                     │                        │
│              └──────────┬──────────┘                        │
│                         │                                    │
│                    ┌────▼────┐                              │
│                    │ Plugins │                              │
│                    │         │                              │
│                    └────┬────┘                              │
│                         │                                    │
└─────────────────────────┼────────────────────────────────────┘
                          │
                    ┌─────▼─────┐
                    │    MCP    │
                    │  Servers  │
                    └───────────┘
```

### Component Hierarchy

```
Toolkit (Root)
│
├── Configuration Layer
│   ├── Rules (coding standards)
│   ├── Contexts (working modes)
│   └── Templates (project starters)
│
├── Execution Layer
│   ├── Hooks (lifecycle automation)
│   ├── Commands (slash commands)
│   └── Agents (specialized experts)
│
├── Extension Layer
│   ├── Plugins (domain capabilities)
│   └── Skills (knowledge modules)
│
└── Integration Layer
    └── MCP Configs (external services)
```

## Component Deep Dive

### Plugins

**Purpose**: Self-contained extensions that add domain-specific capabilities

**Structure**:
```
plugin-name/
├── .claude-plugin/
│   └── plugin.json          # Metadata
├── commands/
│   ├── command1.md          # Command definitions
│   └── command2.md
└── README.md                # Documentation
```

**Loading Mechanism**:
1. Claude Code scans `~/.claude/plugins/`
2. Reads each plugin's `plugin.json`
3. Loads command files from `commands/` directory
4. Makes commands available as slash commands

**Design Pattern**: Plugin architecture with dependency injection

**When to Create a Plugin**:
- Grouping related commands (e.g., all git workflows)
- Domain-specific tooling (e.g., API testing)
- Reusable across projects

### Agents

**Purpose**: Specialized AI personalities with domain expertise

**Structure**:
```markdown
---
name: agent-name
description: What the agent does
tools: ["Read", "Write", "Edit", "Bash"]
model: opus
---

# Agent Instructions

Core principles, patterns, and best practices...
```

**Key Components**:
1. **Frontmatter**: Metadata (name, description, tools, model)
2. **Instructions**: Domain-specific knowledge and patterns
3. **Principles**: Core beliefs and approaches
4. **Examples**: Common scenarios and solutions

**Agent Selection**:
- Explicit: User names the agent in prompt
- Implicit: Claude chooses based on task context
- Hierarchical: Orchestration agents delegate to specialists

**Design Pattern**: Strategy pattern with role-based behavior

### Commands

**Purpose**: Reusable workflow templates triggered by slash commands

**Structure**:
```markdown
# Command Title

Brief description of what the command does.

## Steps

1. Step one
2. Step two
3. Step three

## Format

Expected output format

## Rules

- Rule 1
- Rule 2
```

**Execution Flow**:
1. User invokes `/command-name`
2. Claude Code loads the `.md` file
3. Claude follows the instructions
4. User provides context/inputs
5. Claude executes the workflow

**Design Pattern**: Template method pattern

### Skills

**Purpose**: Transferable knowledge modules

**Structure**:
```
skill-name/
└── SKILL.md                 # Knowledge content
```

**Content Organization**:
- Core concepts
- Best practices
- Common patterns
- Anti-patterns
- Tool recommendations
- Code examples

**Knowledge Transfer**:
Skills inform agent behavior and provide context for decision-making.

**Design Pattern**: Knowledge repository pattern

### Hooks

**Purpose**: Automated actions at lifecycle events

**Structure**:
```json
{
  "EventName": {
    "command": "node script.js ${variable}",
    "timeout": 5000
  }
}
```

**Lifecycle Events**:

```
Session Start ──> User Prompt ──> PreToolUse ──> Tool Execution
                       │              │                │
                       │              │                ▼
                       │              │         PostToolUse
                       │              │                │
                       │              ▼                │
                       │      User Prompt Submit      │
                       │                               │
                       └───────────┬───────────────────┘
                                   │
                            Session End / Stop
```

**Hook Categories**:
1. **Lifecycle**: SessionStart, SessionEnd
2. **Validation**: PreToolUse (gates)
3. **Verification**: PostToolUse (checks)
4. **Monitoring**: Notification, Stop

**Design Pattern**: Event-driven architecture with middleware

### Rules

**Purpose**: Declarative coding standards

**Structure**:
```markdown
# Rule Category

## What This Rule Covers

Description

## Standards

- Standard 1
- Standard 2

## Examples

Good:
```code```

Bad:
```code```
```

**Application**:
Rules are injected into Claude's context and influence all responses.

**Precedence**:
1. Project-level rules (`.claude/rules/`)
2. Global rules (`~/.claude/rules/`)
3. Toolkit defaults

**Design Pattern**: Policy pattern with composition

### Templates

**Purpose**: Starter configurations for common project types

**Structure**:
```markdown
# Project: [Name]

## Tech Stack
...

## Project Structure
...

## Development Workflow
...

## Conventions
...
```

**Customization Points**:
- Tech stack specifics
- Project structure
- Development commands
- Team conventions
- Deployment process

**Design Pattern**: Template pattern with slots

### Contexts

**Purpose**: Working mode configurations

**Structure**:
```markdown
# Context Name

When in this context:

## Priorities
1. Priority 1
2. Priority 2

## Behaviors
- Behavior 1
- Behavior 2

## Constraints
- Constraint 1
- Constraint 2
```

**Context Switching**:
```
/context load dev       # Switch to development mode
/context load review    # Switch to review mode
```

**State Management**:
Contexts modify Claude's behavior temporarily without permanent changes.

**Design Pattern**: State pattern

### MCP Configs

**Purpose**: External service integration

**Structure**:
```json
{
  "mcpServers": {
    "service-name": {
      "command": "executable",
      "args": ["arg1", "arg2"],
      "env": {
        "ENV_VAR": "value"
      }
    }
  }
}
```

**Protocol**:
Model Context Protocol enables Claude to communicate with external services.

**Security Model**:
- Credentials via environment variables
- Minimal permission scopes
- Optional read-only mode

**Design Pattern**: Adapter pattern with protocol abstraction

## How Components Interact

### Example: Feature Development Flow

```
1. Session Start
   └─> SessionStart hook runs
       └─> Loads CLAUDE.md context
       └─> Checks git status
       └─> Displays project state

2. User: "/context load dev"
   └─> Context loaded
       └─> Claude optimizes for iteration

3. User: "/tdd src/auth.ts"
   └─> Command loaded
       └─> Claude follows TDD workflow
       └─> Uses backend-developer agent knowledge
       └─> Applies testing.md rules
       └─> Writes test first

4. Claude uses Write tool
   └─> PreToolUse hook (secret-scanner)
       └─> Checks for credentials
       └─> Allows if safe
   └─> File written
   └─> PostToolUse hook (post-edit-check)
       └─> Runs linter
       └─> Runs tests
       └─> Reports results

5. User: "/commit"
   └─> Command loaded
       └─> git-workflow.md rules applied
       └─> Conventional commit format generated

6. Claude uses Bash tool (git commit)
   └─> PreToolUse hook (commit-guard)
       └─> Validates commit message
       └─> Allows if valid
   └─> Commit executed

7. User: "stop"
   └─> Stop hook runs
       └─> Checks for uncommitted changes
       └─> Reminds to run tests
   └─> SessionEnd hook runs
       └─> Saves session state
```

### Interaction Patterns

#### Pattern 1: Layered Enhancement

```
Base Claude
  ↓
+ Rules (standards)
  ↓
+ Context (mode)
  ↓
+ Agent (expertise)
  ↓
+ Command (workflow)
  ↓
Enhanced Claude
```

Each layer adds specificity and constraints.

#### Pattern 2: Hook Pipeline

```
User Action
  ↓
PreToolUse Hook(s) ──┐
  ↓                   │ (Any failure blocks action)
Tool Execution       │
  ↓                   │
PostToolUse Hook(s) ─┘
  ↓                   
Result
```

Hooks form a middleware pipeline.

#### Pattern 3: Agent Delegation

```
User Request
  ↓
Primary Agent
  ├─> Delegates to Backend Agent
  ├─> Delegates to Database Agent
  └─> Delegates to Security Agent
       ↓
Integrated Response
```

Orchestration agents coordinate specialists.

## Extension Points

### Creating Custom Plugins

**Template**:

```bash
mkdir -p ~/.claude/plugins/my-plugin/commands
mkdir -p ~/.claude/plugins/my-plugin/.claude-plugin
```

**plugin.json**:
```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "My custom plugin",
  "author": "Your Name",
  "license": "MIT"
}
```

**Command** (`commands/mycommand.md`):
```markdown
# My Command

Description of what it does.

## Steps

1. Do this
2. Then this
3. Finally this
```

### Creating Custom Agents

**Template** (`~/.claude/agents/my-agent.md`):

```markdown
---
name: my-specialist
description: Expert in domain X
tools: ["Read", "Write", "Bash"]
model: opus
---

# My Specialist Agent

You are an expert in [domain].

## Principles

1. Principle 1
2. Principle 2

## Approach

- Best practice 1
- Best practice 2

## Common Patterns

Pattern examples...
```

### Creating Custom Rules

**Template** (`~/.claude/rules/my-rule.md`):

```markdown
# My Custom Rule

## Scope

What this rule covers.

## Requirements

- Requirement 1
- Requirement 2

## Examples

### Good
\`\`\`
good code example
\`\`\`

### Bad
\`\`\`
bad code example
\`\`\`

## Rationale

Why this rule exists.
```

### Creating Custom Hooks

**Template** (`~/.claude/hooks/scripts/my-hook.js`):

```javascript
#!/usr/bin/env node

// Parse arguments
const args = process.argv.slice(2);

// Your logic here
const result = checkSomething(args);

if (result.ok) {
  console.log('✓ Check passed');
  process.exit(0);
} else {
  console.error('✗ Check failed:', result.error);
  process.exit(1);
}

function checkSomething(args) {
  // Implementation
}
```

**Register in hooks.json**:
```json
{
  "PreToolUse": {
    "Write": {
      "command": "node ~/.claude/hooks/scripts/my-hook.js ${filePath}"
    }
  }
}
```

## Best Practices

### Composition Over Inheritance

**Good**: Compose multiple small rules
```
rules/
├── naming.md
├── formatting.md
└── testing.md
```

**Avoid**: One large rule file covering everything

### Single Responsibility

**Good**: Each plugin focuses on one domain
```
plugins/
├── api-tester/      # Only API testing
├── doc-forge/       # Only documentation
└── security-audit/  # Only security
```

**Avoid**: Monolithic plugins doing everything

### Convention Over Configuration

**Good**: Use standard paths
```
.claude/
├── rules/
├── hooks.json
└── mcp.json
```

**Avoid**: Custom paths requiring configuration

### Progressive Disclosure

**Good**: Start simple
```
1. Use 2-3 commands
2. Add 1-2 rules
3. Enable 1 hook
4. Gradually expand
```

**Avoid**: Installing everything at once

### Documentation as Code

**Good**: Keep docs with components
```
plugin/
├── commands/
├── README.md        # Plugin documentation
└── examples/        # Usage examples
```

**Avoid**: Separate documentation

## Advanced Patterns

### Pattern 1: Multi-Agent Pipelines

**Architecture**:
```
Planner Agent (Research Context)
  ↓ (Creates plan)
Builder Agent (Dev Context)
  ↓ (Implements)
Reviewer Agent (Review Context)
  ↓ (Validates)
Deployer Agent (Deploy Context)
  ↓ (Releases)
```

**Implementation**:
Each agent produces artifacts (plans, code, reviews) consumed by the next.

**Benefits**:
- Specialization
- Clear handoffs
- Parallel execution possible

### Pattern 2: Contextual Rules

**Architecture**:
```
.claude/
├── rules/
│   ├── global/          # Always active
│   ├── dev/             # Only in dev context
│   ├── review/          # Only in review context
│   └── deploy/          # Only in deploy context
```

**Implementation**:
Contexts can specify which rules to load.

**Benefits**:
- Context-appropriate standards
- Reduced cognitive load
- Flexible enforcement

### Pattern 3: Hook Chains

**Architecture**:
```
PostToolUse(Write)
  ↓
1. Format code (prettier)
  ↓
2. Lint code (eslint)
  ↓
3. Type check (tsc)
  ↓
4. Run tests (jest)
  ↓
5. Update docs (typedoc)
```

**Implementation**:
Sequential execution with failure propagation.

**Benefits**:
- Automated quality pipeline
- Early failure detection
- Consistent enforcement

### Pattern 4: Skill Composition

**Architecture**:
```
Agent: Fullstack Engineer
  Uses Skills:
    - React Patterns
    - Node.js Best Practices
    - PostgreSQL Optimization
    - Docker Deployment
```

**Implementation**:
Agents reference multiple skills for comprehensive knowledge.

**Benefits**:
- Reusable knowledge
- Consistent patterns
- Cross-domain expertise

### Pattern 5: Dynamic Templates

**Architecture**:
```
Template Selection
  ↓
Analyze project characteristics
  ↓
Choose appropriate template
  ↓
Customize based on context
  ↓
Generate CLAUDE.md
```

**Implementation**:
Template selection based on detected stack, team size, complexity.

**Benefits**:
- Personalized setup
- Reduced manual configuration
- Faster onboarding

## Performance Considerations

### Optimization Strategies

1. **Lazy Loading**: Load components only when needed
2. **Caching**: Cache parsed rules and commands
3. **Parallel Execution**: Run independent hooks in parallel
4. **Selective Loading**: Load only relevant skills per agent
5. **Timeout Management**: Set appropriate timeouts for hooks

### Resource Management

**Memory**:
- Limit concurrent agent instances
- Unload unused contexts
- Cache frequently accessed files

**CPU**:
- Optimize hook scripts
- Use efficient parsing
- Batch operations when possible

**Network**:
- Cache MCP server responses
- Use connection pooling
- Implement retry logic

## Security Considerations

### Credential Management

1. **Environment Variables**: Store secrets in env vars
2. **Secret Scanning**: Use hooks to prevent secret commits
3. **Read-Only Tokens**: Use minimal permissions
4. **Rotation**: Regularly rotate credentials

### Validation

1. **Input Validation**: Validate all user inputs in hooks
2. **Command Injection**: Sanitize bash command arguments
3. **Path Traversal**: Validate file paths in hooks
4. **XSS Prevention**: Sanitize outputs in web contexts

### Access Control

1. **Principle of Least Privilege**: Grant minimal permissions
2. **MCP Server Isolation**: Isolate server access
3. **Hook Sandboxing**: Run hooks in restricted environments

## Troubleshooting Patterns

### Pattern: Debug Output

Add debug output to hooks:
```javascript
const DEBUG = process.env.CLAUDE_DEBUG === 'true';

function log(...args) {
  if (DEBUG) console.log('[DEBUG]', ...args);
}
```

### Pattern: Graceful Degradation

Hooks should fail gracefully:
```javascript
try {
  performCheck();
} catch (error) {
  console.warn('⚠️  Check failed but allowing:', error.message);
  process.exit(0); // Don't block
}
```

### Pattern: Validation Layers

Multiple validation stages:
```
1. Syntax validation (fast, always)
2. Type checking (medium, optional)
3. Integration tests (slow, selective)
```

## Extensibility Guidelines

### Plugin Design

1. **Clear Purpose**: One domain per plugin
2. **Minimal Dependencies**: Reduce external requirements
3. **Good Defaults**: Work out of the box
4. **Documentation**: Clear usage examples
5. **Versioning**: Semantic versioning

### Agent Design

1. **Specific Expertise**: Deep knowledge in one area
2. **Clear Boundaries**: Know when to delegate
3. **Consistent Patterns**: Follow established conventions
4. **Self-Contained**: Don't depend on other agents
5. **Testable**: Provide example scenarios

### Hook Design

1. **Fast Execution**: < 5 seconds typically
2. **Clear Feedback**: Informative messages
3. **Fail Safely**: Don't break workflows
4. **Idempotent**: Safe to run multiple times
5. **Observable**: Log important decisions

## Conclusion

The Claude Code Toolkit is designed for:
- **Modularity**: Easy to add/remove components
- **Composability**: Components work together seamlessly
- **Extensibility**: Easy to create custom components
- **Performance**: Fast and efficient
- **Quality**: Built-in best practices

Understanding these concepts helps you:
- Use the toolkit effectively
- Customize for your needs
- Create your own extensions
- Troubleshoot issues
- Optimize performance

## Further Reading

- **[GETTING_STARTED.md](GETTING_STARTED.md)** - Introduction and quick start
- **[SETUP.md](SETUP.md)** - Installation and configuration
- **[USAGE_GUIDE.md](USAGE_GUIDE.md)** - Practical usage examples
- **[examples/](examples/)** - Real-world workflows
- **[CONTRIBUTING.md](CONTRIBUTING.md)** - Contributing guidelines

---

**Questions?** Open an issue on [GitHub](https://github.com/rohitg00/awesome-claude-code-toolkit/issues).
