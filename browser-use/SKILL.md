---
name: browser-use
description: Test web applications live using browser automation, or integrate Claude with external services via MCP connectors. Use when asked to verify UI behavior end-to-end, check responsive layouts, test form flows, or connect to GitHub, Slack, Jira, Linear, and other external APIs. Covers agent-browser CLI for headless browser control and MCP server setup for service integrations.
---

# Browser Use & Integrations

Two capabilities in one skill: **browser automation** for testing live web UIs, and **MCP integrations** for connecting Claude to external services.

---

## Part 1 — Browser Automation (agent-browser)

`agent-browser` is a Rust-based CLI that controls Chrome via the DevTools Protocol. It uses stable element refs (`@e1`, `@e2`) rather than fragile CSS selectors — ideal for AI-driven testing.

### Install

```bash
npx agent-browser --help          # One-shot, no install
npm install -g agent-browser      # Persistent install
```

Or install as a Claude skill:
```bash
npx skills add vercel-labs/agent-browser
```

### Core Workflow

```bash
# 1. Open the app
agent-browser open https://localhost:3000

# 2. Snapshot interactive elements — returns stable @eN refs
agent-browser snapshot -i

# 3. Interact using refs (not selectors)
agent-browser click @e2
agent-browser fill @e3 "test@example.com"
agent-browser select @e4 "Option A"

# 4. Re-snapshot after navigation/state change
agent-browser snapshot -i

# 5. Capture visual proof
agent-browser screenshot --full-page
agent-browser screenshot --selector "#dashboard"
```

### Semantic Element Discovery (When Refs Aren't Enough)

```bash
# Find by ARIA role + name
agent-browser find role button click --name "Submit"
agent-browser find role textbox fill "hello@test.com" --name "Email"

# Find by label
agent-browser find label "Password" fill "secret123"

# Find by text content
agent-browser find text "Sign in" click
```

### Authentication State

```bash
# Save auth across sessions — avoid re-logging in
agent-browser auth save --name "dev-user"
agent-browser auth load --name "dev-user"

# Multiple isolated sessions
agent-browser session new --name "admin"
agent-browser session new --name "viewer"
agent-browser session use "admin"
```

### Responsive Testing

```bash
agent-browser viewport 375 812    # iPhone 14 Pro
agent-browser viewport 768 1024   # iPad
agent-browser viewport 1280 800   # Desktop standard
agent-browser viewport 1920 1080  # Large desktop

agent-browser screenshot --full-page  # Capture at each viewport
```

### Testing Patterns

**Happy path (standard test sequence):**
1. `open` the URL
2. `snapshot -i` — see all interactive elements with refs
3. Fill forms and click buttons by ref
4. `snapshot -i` again — verify expected state
5. `screenshot` — capture evidence

**Form validation test:**
1. Submit with empty required fields → verify error messages appear
2. Submit with invalid format → verify inline errors
3. Submit valid data → verify success state

**Auth flow test:**
1. Load auth state or log in fresh
2. Navigate to protected route → verify access
3. Log out → verify redirect to login
4. Navigate directly to protected route → verify redirect

### When to Use browser-use vs. Reading Code

| Situation | Use browser-use | Read code instead |
|-----------|----------------|-------------------|
| Verify UI after implementing | Yes | No |
| Check responsive layout | Yes | No |
| Test form validation | Yes | No |
| Understand a bug | Sometimes | Yes |
| Server-side only feature | No | Yes |
| Dev server not running | No | Yes |

---

## Part 2 — MCP Integrations

MCP (Model Context Protocol) gives Claude authenticated tool access to external services — GitHub, Slack, Linear, Jira, and 80+ others.

### Setup — Direct MCP Servers

Add to `.claude/settings.json` in your project (or `~/.claude/settings.json` for global):

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_your_token_here"
      }
    },
    "slack": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-slack"],
      "env": {
        "SLACK_BOT_TOKEN": "xoxb-your-token",
        "SLACK_TEAM_ID": "T0XXXXXXX"
      }
    },
    "linear": {
      "command": "npx",
      "args": ["-y", "@linear/mcp-server"],
      "env": {
        "LINEAR_API_KEY": "lin_api_your_key"
      }
    }
  }
}
```

Restart Claude Code after editing settings for MCP servers to connect.

### Setup — Composio (78+ Services, One Auth)

Composio handles OAuth and token management for 78+ SaaS tools. One setup, all services:

```bash
npm install -g composio-core
composio login                        # OAuth browser flow
composio add github slack jira linear # Connect services
```

Add to `.claude/settings.json`:
```json
{
  "mcpServers": {
    "composio": {
      "command": "composio",
      "args": ["mcp", "start"]
    }
  }
}
```

### Common Integration Tasks

**GitHub:**
- Create issues from test failures or bug reports
- Open PRs and request reviews
- Check CI status before merging
- Comment on PRs with implementation notes
- Merge approved PRs

**Slack:**
- Post deployment notifications to channels
- Send DMs on task completion
- Read thread context before responding
- Search message history for decisions

**Linear / Jira:**
- Update ticket status when implementation completes
- Create bugs from browser-use test failures
- Link commits and PRs to tickets
- Move items through workflow stages

**GitHub Actions via MCP:**
- Trigger workflows
- Check run status
- Re-run failed jobs

### Security Rules

- Store all API tokens in environment variables — never hardcode in settings files
- Use project-level `.claude/settings.json` for project-specific integrations
- Use `~/.claude/settings.json` only for tools you use across all projects
- Scope Composio permissions to the minimum needed
- Rotate tokens when team members leave
- Never commit `.claude/settings.json` if it contains tokens — add to `.gitignore`

### Verify Integration is Working

```bash
# Check which MCP servers are connected
claude mcp list

# Test a specific server
claude mcp test github
```

If a server shows disconnected, check: token validity, command is installed (`npx` available), and network access.
