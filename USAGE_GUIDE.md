# Usage Guide

This comprehensive guide shows you how to use every component of the Claude Code Toolkit effectively.

## Table of Contents

- [Using Plugins](#using-plugins)
- [Using Agents](#using-agents)
- [Using Commands](#using-commands)
- [Using Skills](#using-skills)
- [Using Hooks](#using-hooks)
- [Using Rules](#using-rules)
- [Using Templates](#using-templates)
- [Using MCP Servers](#using-mcp-servers)
- [Using Contexts](#using-contexts)
- [Real-World Workflows](#real-world-workflows)

## Using Plugins

Plugins extend Claude Code with domain-specific capabilities. Each plugin contains slash commands and specialized knowledge.

### Available Plugin Categories

- **Development**: API design, backend, frontend, mobile
- **Quality**: Testing, code review, debugging
- **Security**: Vulnerability scanning, compliance checking
- **DevOps**: CI/CD, deployment, monitoring
- **Documentation**: API docs, changelog generation
- **Architecture**: ADR writing, diagram generation

### How to Use Plugins

#### 1. Discovery

List available plugins:
```bash
ls ~/.claude/plugins/claude-code-toolkit/plugins/
```

#### 2. Reading Plugin Documentation

Each plugin has commands in its directory:
```bash
ls ~/.claude/plugins/claude-code-toolkit/plugins/bug-detective/commands/
# Output: debug.md, trace.md
```

#### 3. Using Plugin Commands

Commands from plugins are available as slash commands:

```
/debug
/trace
/tdd
/commit
/doc-gen
```

### Example: Bug Detective Plugin

**Purpose**: Systematic debugging with root cause analysis

**Commands**:
- `/debug` - Debug an issue systematically
- `/trace` - Trace execution path

**Usage**:

```
> /debug

> Bug: User login fails with 500 error
> Symptoms: POST /api/login returns 500, happens only for certain email addresses
> Expected: Should return 200 with JWT token
> Actual: Returns 500 Internal Server Error
```

Claude will follow the systematic debugging workflow:
1. Gather symptoms
2. Identify entry point
3. Trace execution path
4. Form hypotheses
5. Test each hypothesis
6. Implement fix
7. Add regression test

### Example: API Architect Plugin

**Purpose**: Design, document, and test APIs

**Commands**:
- `/api-design` - Design API endpoints
- `/api-docs` - Generate OpenAPI specs
- `/api-test` - Create API test suites

**Usage**:

```
> /api-design

> I need a REST API for managing blog posts with CRUD operations.
> Requirements:
> - Authentication with JWT
> - Rate limiting
> - Pagination for list endpoints
> - Full-text search
```

Claude will design endpoints, schemas, error responses, and authentication.

### Popular Plugins

| Plugin | When to Use | Key Commands |
|--------|-------------|--------------|
| `bug-detective` | Debugging production issues | `/debug`, `/trace` |
| `tdd-helper` | Test-driven development | `/tdd`, `/test-first` |
| `code-review-assistant` | Reviewing pull requests | `/review`, `/security-check` |
| `api-architect` | Building APIs | `/api-design`, `/api-docs` |
| `deploy-pilot` | Deployment automation | `/deploy`, `/rollback` |
| `doc-forge` | Documentation | `/doc-gen`, `/api-docs` |
| `security-scanner` | Security auditing | `/audit`, `/vulnerability-scan` |

## Using Agents

Agents are specialized AI experts with deep knowledge in specific domains. They act as consultants you can ask for guidance.

### How Agents Work

- Each agent has a specific role and expertise
- Agents use particular tools and models
- They follow domain-specific principles and best practices
- Reference them by name in your prompts

### Available Agent Categories

```
agents/
├── core-development/          # Backend, frontend, fullstack
├── language-experts/          # Python, Go, Rust, TypeScript
├── infrastructure/            # Docker, Kubernetes, AWS
├── quality-assurance/         # Testing, security, performance
├── data-ai/                   # ML, data engineering, analytics
├── developer-experience/      # DevOps, tooling, automation
├── specialized-domains/       # Mobile, desktop, embedded
├── business-product/          # Product management, analytics
├── orchestration/             # Multi-agent coordination
└── research-analysis/         # Technical research, evaluation
```

### Using Agents in Prompts

#### Method 1: Direct Reference

```
> Can the backend-developer agent help me design a REST API with authentication?
```

#### Method 2: Implicit Invocation

Describe your task and Claude will suggest the right agent:

```
> I need to optimize my PostgreSQL queries. They're taking too long on large tables.
```

Claude will engage the database optimization expert automatically.

#### Method 3: Explicit Agent Selection

```
> Load the rust-systems-engineer agent and help me implement a concurrent hash map.
```

### Example: Backend Developer Agent

**Expertise**: Node.js backend with Express/Fastify

**Usage**:

```
> I need the backend-developer agent to help me build a user authentication service.
> Requirements:
> - JWT tokens with refresh
> - Email verification
> - Password reset flow
> - Rate limiting on login attempts
```

**Agent Response**:
```
I'll help you build a robust authentication service using Fastify with these components:

1. Project Structure
2. Database Schema (users, sessions, verification_tokens)
3. Service Layer (AuthService with bcrypt hashing)
4. API Endpoints (POST /register, POST /login, POST /refresh)
5. Rate Limiting Middleware
6. Email Service Integration
7. Tests for all flows

Let's start with the database schema...
```

### Example: React Architect Agent

**Expertise**: React applications with modern patterns

**Usage**:

```
> React-architect agent: Review my component architecture.
> I have a shopping cart feature with Cart, CartItem, CartSummary, and CheckoutForm components.
> They all share cart state. How should I structure this?
```

**Agent Response**:
```
I recommend this architecture:

1. Context Layer: CartContext with useCart hook
2. State Management: useReducer for cart operations
3. Component Hierarchy:
   - CartProvider (context boundary)
   - Cart (container)
   - CartItemList (presentational)
   - CartItem (presentational)
   - CartSummary (presentational with useMemo)
   - CheckoutForm (separate feature, accesses context)

Let me show you the implementation...
```

### Agent Best Practices

1. **Be Specific**: Provide context and requirements
2. **Ask for Planning**: Let agents plan before implementing
3. **Follow Their Guidance**: Agents follow proven patterns
4. **Review Output**: Agents are experts but verify critical code
5. **Chain Agents**: Use different agents for different phases

### Multi-Agent Workflows

Chain multiple agents for complex tasks:

```
> 1. Python-architect agent: Design the ML training pipeline
> 2. Database-expert agent: Design the feature store schema  
> 3. DevOps-engineer agent: Create the deployment pipeline
```

See [examples/multi-agent-pipeline.md](examples/multi-agent-pipeline.md) for detailed walkthrough.

## Using Commands

Commands are slash commands that trigger specific workflows.

### Command Categories

| Category | Commands | Purpose |
|----------|----------|---------|
| Git | `/commit`, `/pr`, `/branch` | Git workflow automation |
| Testing | `/tdd`, `/test`, `/coverage` | Test creation and execution |
| Documentation | `/doc-gen`, `/api-docs`, `/onboard` | Documentation generation |
| Security | `/audit`, `/vulnerability-scan` | Security analysis |
| Architecture | `/adr`, `/diagram`, `/design-review` | Architecture decisions |
| Refactoring | `/extract`, `/rename`, `/simplify` | Code refactoring |
| DevOps | `/deploy`, `/monitor`, `/ci-config` | Deployment and operations |
| Workflow | `/checkpoint`, `/wrap-up`, `/orchestrate` | Session management |

### Using Commands

#### Basic Syntax

```
/command-name
```

#### With Context

Commands work better with context:

```
> /commit

> Changes:
> - Added user authentication endpoints
> - Implemented JWT token generation
> - Added password hashing with bcrypt
```

Claude will create a properly formatted commit message following conventions.

### Example Workflows

#### 1. Test-Driven Development

```
> /tdd src/utils/validator.ts

> I need an email validator function that:
> - Returns true for valid emails
> - Returns false for invalid emails
> - Handles edge cases like missing @ or domain
```

Claude will:
1. Write failing tests first
2. Implement the validator
3. Verify tests pass
4. Suggest edge cases
5. Add additional tests

#### 2. Git Commit Workflow

```
> /commit

Claude reviews changes:
- What changed?
- Why?
- Any breaking changes?

Then creates commit message:
feat(auth): add JWT token authentication

- Implement JWT token generation with 15min expiry
- Add refresh token flow with 7 day expiry
- Secure token storage in httpOnly cookies

BREAKING CHANGE: Auth endpoints now require JWT header
```

#### 3. Documentation Generation

```
> /doc-gen

> Generate API documentation for the auth service in src/services/auth.ts
```

Claude will:
1. Analyze the code
2. Generate JSDoc comments
3. Create API reference markdown
4. Update README with examples

#### 4. Security Audit

```
> /audit

> Audit the authentication service for security issues
```

Claude checks for:
- SQL injection vulnerabilities
- XSS vulnerabilities
- Insecure password storage
- Missing input validation
- Exposed secrets
- Insecure dependencies

### Creating Custom Commands

Create your own commands:

1. Create a `.md` file in `~/.claude/commands/custom/`:
   ```bash
   mkdir -p ~/.claude/commands/custom
   ```

2. Write the command definition:
   ```markdown
   # Deploy to Staging
   
   Deploy the current branch to staging environment.
   
   ## Steps
   
   1. Verify all tests pass locally
   2. Push changes to remote
   3. Trigger staging deployment via CI
   4. Wait for deployment to complete
   5. Run smoke tests
   6. Report deployment status
   ```

3. Use it:
   ```
   /deploy-staging
   ```

## Using Skills

Skills are knowledge modules that teach best practices and patterns.

### Available Skills

- **Testing**: `tdd-mastery`, `testing-strategies`
- **Architecture**: `microservices-design`, `api-design-patterns`
- **Security**: `security-hardening`, `authentication-patterns`
- **Performance**: `performance-optimization`, `database-optimization`
- **Cloud**: `aws-cloud-patterns`, `kubernetes-operations`
- **Languages**: `typescript-advanced`, `python-best-practices`, `rust-systems`, `golang-idioms`
- **Frameworks**: `react-patterns`, `nextjs-mastery`, `django-patterns`, `springboot-patterns`
- **Tools**: `docker-best-practices`, `git-advanced`
- **Emerging**: `llm-integration`, `mcp-development`, `prompt-engineering`

### Using Skills

Skills are automatically available to agents. You can also reference them explicitly:

```
> Using the tdd-mastery skill, help me write tests for my shopping cart logic.
```

```
> Apply the security-hardening skill to review this authentication code.
```

```
> I'm new to GraphQL. Use the graphql-design skill to teach me best practices.
```

### Skill Structure

Each skill contains:
- Core principles
- Best practices
- Anti-patterns to avoid
- Code examples
- Tool recommendations

### Example: TDD Mastery Skill

**When to use**: Writing tests before code

**Principles**:
1. Red: Write a failing test
2. Green: Make it pass with minimal code
3. Refactor: Clean up while keeping tests green

**Usage**:

```
> Use TDD to build a URL shortener service
```

Claude (using TDD mastery skill):
```
Let's follow TDD. First test:

test('generateShortCode creates 6-character code', () => {
  const code = generateShortCode();
  expect(code).toHaveLength(6);
  expect(code).toMatch(/^[a-zA-Z0-9]+$/);
});

This fails because generateShortCode doesn't exist yet.
Now let's implement just enough to pass...
```

## Using Hooks

Hooks automate quality checks and actions at specific lifecycle events.

### Hook Types

| Event | When It Runs | Use Cases |
|-------|--------------|-----------|
| SessionStart | Beginning of session | Load context, check git status |
| SessionEnd | End of session | Save state, log learnings |
| PreToolUse | Before Claude uses a tool | Validate actions, block unsafe operations |
| PostToolUse | After Claude uses a tool | Run linters, execute tests |
| Stop | When Claude is stopped | Remind about pending tasks |

### Commonly Used Hooks

#### 1. Pre-Commit Check (PreToolUse - Bash)

**Purpose**: Validate commit messages follow conventions

**File**: `commit-guard.js`

**Behavior**:
```bash
# Invalid commit - blocked
git commit -m "fixed bug"
# ❌ Blocked: Commit message must follow conventional format

# Valid commit - allowed
git commit -m "fix(auth): resolve token expiry bug"
# ✓ Commit allowed
```

#### 2. Post-Edit Linting (PostToolUse - Write/Edit)

**Purpose**: Run linter after file edits

**File**: `post-edit-check.js`

**Behavior**:
```
[File edited: src/utils/parser.ts]
Running eslint on src/utils/parser.ts...
✓ No linting errors
```

#### 3. Auto-Test Runner (PostToolUse - Write/Edit)

**Purpose**: Run relevant tests after code changes

**File**: `auto-test.js`

**Behavior**:
```
[File edited: src/services/auth.ts]
Running tests: npm test -- auth.test.ts
✓ All 12 tests passed
```

#### 4. Secret Scanner (PreToolUse - Write/Edit)

**Purpose**: Block files containing secrets

**File**: `secret-scanner.js`

**Behavior**:
```
[Attempting to write config.ts]
❌ Blocked: File contains potential secrets:
  - Line 5: API_KEY="sk_live_..."
  - Line 8: DATABASE_PASSWORD="..."

Use environment variables instead.
```

### Configuring Hooks

Edit `~/.claude/hooks.json`:

```json
{
  "SessionStart": {
    "command": "node ~/.claude/hooks/scripts/session-start.js",
    "timeout": 5000
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
  },
  "PreToolUse": {
    "Bash": {
      "command": "node ~/.claude/hooks/scripts/pre-push-check.js ${command}",
      "timeout": 3000
    }
  }
}
```

### Custom Hooks

Create your own hooks:

```javascript
// ~/.claude/hooks/scripts/custom-check.js
const fs = require('fs');

const filePath = process.argv[2];
const content = fs.readFileSync(filePath, 'utf8');

// Your custom logic
if (content.includes('TODO:')) {
  console.log(`⚠️ Warning: ${filePath} contains TODO comments`);
  process.exit(1); // Non-zero exit blocks the action
}

process.exit(0); // Zero exit allows the action
```

Add to `hooks.json`:
```json
{
  "PostToolUse": {
    "Write": {
      "command": "node ~/.claude/hooks/scripts/custom-check.js ${filePath}"
    }
  }
}
```

## Using Rules

Rules define coding standards that Claude automatically follows.

### Available Rules

| Rule | What It Enforces |
|------|------------------|
| `coding-style.md` | Naming conventions, file organization |
| `git-workflow.md` | Branch naming, commit format |
| `testing.md` | Test structure, coverage requirements |
| `security.md` | Input validation, secret management |
| `error-handling.md` | Exception handling patterns |
| `api-design.md` | REST conventions, status codes |
| `documentation.md` | JSDoc, inline comments |
| `performance.md` | Optimization guidelines |
| `accessibility.md` | WCAG compliance |
| `database.md` | Query patterns, migrations |

### How Rules Work

Rules in `~/.claude/rules/` are automatically loaded and applied to all interactions.

### Example: Testing Rule

**Content of `testing.md`**:
```markdown
# Testing Rules

## Structure
- Tests go in `__tests__/` or `.test.ts` alongside source
- One test file per source file
- Group related tests with describe blocks

## Naming
- Test files: `*.test.ts` or `*.spec.ts`
- Test names: "should [expected behavior] when [condition]"

## Coverage
- Minimum 80% line coverage
- 100% coverage for critical paths (auth, payment)

## Best Practices
- Test behavior, not implementation
- Use test.each for multiple similar cases
- Mock external dependencies
- No shared state between tests
```

**Effect on Claude**:

When you ask Claude to write tests, it automatically:
- Creates tests in `__tests__/` directory
- Uses descriptive test names
- Groups with describe blocks
- Aims for 80%+ coverage
- Follows all specified patterns

### Custom Rules

Create project-specific rules:

```bash
# Project-level rule
echo "# API Response Format

All API endpoints must return responses in this format:
\`\`\`typescript
{
  success: boolean;
  data?: T;
  error?: { code: string; message: string; };
}
\`\`\`
" > .claude/rules/api-format.md
```

Claude will now enforce this format for all API endpoints.

## Using Templates

Templates provide starter CLAUDE.md files for different project types.

### Available Templates

| Template | Project Type | Contents |
|----------|--------------|----------|
| `minimal.md` | Small scripts, prototypes | Basic context |
| `standard.md` | Most projects | Rules, commands, structure |
| `comprehensive.md` | Large codebases | Detailed conventions, multiple services |
| `fullstack-app.md` | Next.js + API | Frontend + backend specifics |
| `python-project.md` | FastAPI/Django | Python tooling, virtual envs |
| `monorepo.md` | Turborepo/Nx | Multi-package management |
| `enterprise.md` | Large teams | Compliance, SSO, governance |

### Using Templates

#### 1. Copy Template

```bash
cd your-project/
cp ~/.claude/plugins/claude-code-toolkit/templates/claude-md/standard.md ./CLAUDE.md
```

#### 2. Customize

Edit `CLAUDE.md` with your project details:

```markdown
# Project: My SaaS App

## Tech Stack
- Frontend: Next.js 14, TailwindCSS, Radix UI
- Backend: Node.js, Fastify, Prisma
- Database: PostgreSQL
- Deployment: Vercel

## Project Structure
\`\`\`
src/
  app/          - Next.js app router pages
  components/   - React components
  lib/          - Utilities and helpers
  server/       - API routes and services
\`\`\`

## Development Commands
\`\`\`bash
pnpm dev          # Start dev server
pnpm test         # Run tests
pnpm lint         # Run linter
pnpm db:migrate   # Run database migrations
\`\`\`

## Coding Conventions
- Use TypeScript strict mode
- Prefer server components by default
- API routes in src/app/api/
- Database queries in repositories only
```

#### 3. Keep Updated

Update CLAUDE.md as your project evolves.

### What Goes in CLAUDE.md

**Essential**:
- Project purpose
- Tech stack
- Project structure
- Development commands
- Testing approach

**Optional but Recommended**:
- Deployment process
- API conventions
- Database schema overview
- Environment variables
- Known issues or gotchas

## Using MCP Servers

MCP servers give Claude access to external tools and services.

### Available Server Types

- **Version Control**: GitHub, GitLab
- **Databases**: PostgreSQL, MySQL, SQLite, Redis
- **Cloud**: AWS, Google Cloud, Azure
- **Tools**: Puppeteer, Playwright, Figma
- **AI/ML**: Jupyter, TensorFlow
- **DevOps**: Docker, Kubernetes, Terraform

### Configuration Example

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    },
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

### Using MCP Servers

Once configured, Claude can use these servers automatically:

```
> Show me open pull requests in this repository
[Uses GitHub MCP server to fetch PRs]

> Query the users table and show me the last 10 signups
[Uses PostgreSQL MCP server to run query]

> Take a screenshot of the homepage at /
[Uses Puppeteer MCP server to capture screenshot]
```

### Security Best Practices

1. **Use Environment Variables**:
   ```json
   "env": {
     "API_KEY": "${MY_API_KEY}"
   }
   ```

2. **Never Commit Credentials**:
   Add to `.gitignore`:
   ```
   .claude/mcp.json
   .mcp.json
   ```

3. **Use Read-Only Tokens When Possible**:
   ```
   GITHUB_TOKEN=ghp_readOnlyToken123
   ```

4. **Scope Permissions Minimally**:
   Only grant necessary permissions to MCP servers.

## Using Contexts

Contexts configure Claude's behavior for different tasks.

### Available Contexts

| Context | Focus | Use When |
|---------|-------|----------|
| `dev.md` | Fast iteration, patterns | Building features |
| `review.md` | Finding issues, security | Reviewing code |
| `research.md` | Evaluation, comparison | Choosing technologies |
| `debug.md` | Root cause analysis | Fixing bugs |
| `deploy.md` | Safety, checklists | Releasing code |

### Loading Contexts

```
/context load dev
/context load review
/context load debug
```

### Context Examples

#### Development Context

```
/context load dev

> Build a user settings page with form validation

Claude (in dev mode):
- Follows existing patterns
- Tests alongside implementation
- Prioritizes shipping over perfection
- Creates minimal viable solution first
```

#### Review Context

```
/context load review

> Review the authentication changes

Claude (in review mode):
- Checks for security issues
- Verifies error handling
- Looks for edge cases
- Validates test coverage
- Flags potential bugs
```

#### Debug Context

```
/context load debug

> The payment processing fails intermittently

Claude (in debug mode):
- Systematically gathers symptoms
- Forms testable hypotheses
- Adds logging/debugging
- Reproduces the issue
- Fixes root cause
- Adds regression test
```

### Custom Contexts

Create your own:

```markdown
# Performance Optimization Context

When in this context, prioritize performance:

1. Profile before optimizing
2. Focus on bottlenecks only
3. Measure improvements
4. Don't sacrifice readability without significant gains
5. Use appropriate data structures
6. Cache expensive operations
7. Lazy load when possible
```

Save as `.claude/contexts/performance.md` and use with `/context load performance`.

## Real-World Workflows

### Workflow 1: Building a New Feature

```
# 1. Start with context
/context load dev

# 2. Plan the feature
> Let's build user profile editing. Plan the implementation.

# 3. TDD approach
/tdd src/services/profile.ts

> Implement updateProfile function with validation

# 4. Build incrementally
> Implement the service layer
> Now add the API endpoint
> Create the frontend form

# 5. Test as you go
> Run the tests
> Fix any failures

# 6. Review your work
/context load review
> Review all changes for security and edge cases

# 7. Document
/doc-gen
> Generate API docs for the profile endpoints

# 8. Commit
/commit
> Properly formatted commit message

# 9. Create PR
> Create a PR for this feature
```

### Workflow 2: Debugging Production Issue

```
# 1. Load debug context
/context load debug

# 2. Use debug command
/debug

> Issue: Users report 500 error on checkout
> Symptom: POST /api/checkout fails intermittently
> Logs show: "TypeError: Cannot read property 'id' of undefined"

# 3. Claude traces execution path
# 4. Forms hypotheses
# 5. Tests each hypothesis
# 6. Identifies root cause
# 7. Implements fix
# 8. Adds regression test

# 9. Verify fix
> Run all checkout-related tests

# 10. Deploy carefully
/context load deploy
> Deploy to staging first
```

### Workflow 3: Code Review

```
# 1. Load review context
/context load review

# 2. Get the changes
> Show me all changes in PR #123

# 3. Review systematically
> Review the authentication changes for security issues
> Check error handling in the payment flow
> Verify test coverage is adequate

# 4. Provide feedback
> Create review comments for the issues found

# 5. Verify fixes
> Review the updated code after fixes
```

### Workflow 4: Starting New Project

See [examples/project-setup.md](examples/project-setup.md) for complete walkthrough:

```
# 1. Choose template
cp ~/.claude/plugins/claude-code-toolkit/templates/claude-md/fullstack-app.md ./CLAUDE.md

# 2. Add rules
mkdir -p .claude/rules
cp ~/.claude/rules/*.md .claude/rules/

# 3. Configure hooks
cp ~/.claude/hooks.json .claude/hooks.json

# 4. Set up MCP
cp ~/.claude/mcp.json .claude/mcp.json

# 5. Start developing
/context load dev
> Initialize Next.js project with TypeScript and TailwindCSS
```

## Tips and Best Practices

### General Tips

1. **Start Simple**: Don't use everything at once
2. **Be Explicit**: Tell Claude which agents, commands, or contexts to use
3. **Provide Context**: More context = better results
4. **Iterate**: Refine your approach over time
5. **Customize**: Adapt templates, rules, and commands to your needs

### Performance Tips

1. **Use Contexts**: They optimize Claude's focus
2. **Chain Commands**: Combine related commands
3. **Leverage Agents**: They're optimized for specific tasks
4. **Hooks for Automation**: Let hooks handle repetitive checks

### Quality Tips

1. **Enable Relevant Rules**: Start with 3-5 core rules
2. **Use Pre-Commit Hooks**: Catch issues early
3. **Review Before Deploy**: Always use review context
4. **Test Alongside Code**: Use TDD commands

### Team Tips

1. **Standardize Templates**: Create team CLAUDE.md templates
2. **Share Custom Rules**: Keep rules in version control
3. **Document Workflows**: Create workflow guides for team
4. **Review Hook Config**: Ensure hooks match team standards

## Next Steps

- **[CONCEPTS.md](CONCEPTS.md)** - Understand the architecture and design
- **[examples/](examples/)** - See complete workflow examples
- **[CONTRIBUTING.md](CONTRIBUTING.md)** - Contribute your own plugins and agents

---

**Need help?** Open an issue on [GitHub](https://github.com/rohitg00/awesome-claude-code-toolkit/issues).
