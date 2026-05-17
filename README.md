# Claude Code Skills Library

A curated collection of Claude Code skills that extend Claude's capabilities during development.
Installed globally at `~/.claude/skills/`.

**GitHub:** [Carefree-1991/claude-skills](https://github.com/Carefree-1991/claude-skills)

## Repo Structure

| Folder | Type | Invoked with |
|--------|------|-------------|
| `skills/` (and subfolders) | Skills | `/skill-name` or `Skill tool` |
| `commands/` | Slash commands | `/command-name` |

---

## Quick Start

The fastest way to get started: invoke `/build-skills` at the beginning of any work session.
Claude will acknowledge all available skills and then **proactively offer the right skill before each task** — you just say "use it" or "skip."

```
You: build skills
Claude: Skill-aware mode active. I'll offer relevant skills before each task...
```

---

## When to Use Which

| Situation | Skill to Use |
|-----------|-------------|
| Starting a work session | `/build-skills` |
| Request is vague or could go multiple ways | `/align` |
| Building any web UI or component | `/frontend-design` |
| Building with shadcn/ui or Tailwind | `/shadcn-ui-feel` |
| Implementing a feature or fixing a bug | `/test-driven-development` |
| Complex multi-step task | `/writing-plans` → `/executing-plans` |
| Found a bug or unexpected behavior | `/systematic-debugging` |
| About to commit or ship | `/verification-before-completion` |
| Feature complete, needs review | `/requesting-code-review` |
| Got code review feedback | `/receiving-code-review` |
| Testing the live UI in a browser | `/browser-use` |
| Connecting to GitHub, Slack, or Jira | `/browser-use` |
| Large project with phases | GSD workflow (see below) |
| End of every work session | `/close` |
| Save a key decision across sessions | `/elephant` |
| Auto-improve skills over time | `task-observer` (background) |

---

## Skill Categories

### Session Management

---

#### `/build-skills`
**What it does:** Activates skill-aware development mode for the entire session. Once active, Claude proactively offers the right skill before each task — surfacing the name, explaining what it will do for that specific task, and waiting for you to say "use it" or "skip" before proceeding.

**Best for:** Any development session where you want Claude to be smarter about using its tools rather than just diving in.

**How to invoke:** Say "build skills" or `/build-skills` at the start of a session.

**What to expect:**
```
💡 /align available
This request is a bit open-ended — align will ask 3 quick questions upfront
to make sure I build exactly what you have in mind.
→ "use it" to activate | "skip" to proceed without | "what is it?" for details
```

---

#### `/close`
**Source:** [github.com/fogarasy/close-skill](https://github.com/fogarasy/close-skill)

**What it does:** A session-closing ritual. Updates MEMORY.md with decisions made, flags loose ends, writes a handoff document so the next session starts with full context.

**Best for:** End of every work session, especially on projects spanning multiple days.

**How to invoke:** `/close` when you're done for the day.

---

#### `task-observer`
**Source:** [github.com/rebelytics/one-skill-to-rule-them-all](https://github.com/rebelytics/one-skill-to-rule-them-all)

**What it does:** Runs silently in the background, watching for patterns that could improve the skill library. Logs observations during the session and surfaces them at the end. Over time, your skill library improves from real usage.

**Best for:** Every task-oriented session (invoke once, runs silently throughout).

**Setup:** Add to `CLAUDE.md`:
```
At the start of any task-oriented session, invoke the task-observer skill.
```

---

### Alignment & Context

---

#### `/align`
**What it does:** Forces Claude to ask clarifying questions **before** writing a single line of code. Presents up to 5 labeled questions (A, B, C...) with default assumptions included, waits for answers, summarizes understanding, then confirms before proceeding. Eliminates the rework cycle caused by misunderstood requirements.

**Best for:**
- Vague or multi-interpretation requests ("make it better", "add a feature")
- When scope, edge cases, or design decisions are unspecified
- Any task where being wrong costs significant rework

**How to invoke:** `/align` before describing the task, or tell Claude "align before you start."

**What to expect:**
```
Before I start, a few quick questions:

A. Should this work on mobile or desktop only? (default: both)
B. Do you want this to persist across sessions? (default: yes, using localStorage)
C. Which users should have access — all users or admins only?

Answer any you know — I'll use sensible defaults for the rest.
```

---

### Design Skills

---

#### `/frontend-design`
**Source:** [github.com/anthropics/skills](https://github.com/anthropics/skills) (official Anthropic skill)

**What it does:** Forces intentional design thinking before writing any frontend code. Establishes a bold aesthetic direction (purpose, tone, differentiation) and enforces rules for typography, color, motion, and spatial composition that prevent generic AI-generated output.

**Best for:**
- Any web UI, component, landing page, or dashboard
- When the output needs to feel visually crafted, not generated
- HTML/CSS artifacts, React components, Next.js pages

**How to invoke:** `/frontend-design` before starting any UI work.

**What to avoid (this skill prevents):**
- Inter/Roboto/Arial as the only fonts
- Purple-to-blue gradients on white backgrounds
- Predictable grid layouts with no character
- Cookie-cutter design that looks the same across projects

---

#### `/shadcn-ui-feel`
**What it does:** Specifically targets shadcn/ui projects. Forces four design decisions before any component code: color axis, type pairing, border-radius personality, and density. Then rewrites the CSS variable layer and applies component-level patterns that make the UI feel crafted rather than default. Includes a 12-item anti-pattern checklist to run before calling any UI complete.

**Best for:**
- Any project using shadcn/ui, Tailwind CSS, or React/Next.js
- When you want shadcn to look like YOUR brand, not the shadcn demo
- Restyling existing shadcn UIs that feel generic

**How to invoke:** `/shadcn-ui-feel` before building or restyling any shadcn component or page.

**The four decisions it forces:**
1. **Color axis** — a dominant hue that isn't blue, purple, or gray
2. **Type pairing** — two fonts, neither should be Inter as primary
3. **Radius personality** — sharp (0rem) or soft (0.75rem+), never the 0.5rem default
4. **Density** — compact or spacious, never the invisible middle

---

### Development Process

---

#### `/test-driven-development`
**Source:** [github.com/obra/superpowers](https://github.com/obra/superpowers)

**What it does:** Enforces TDD methodology before writing implementation code. Writes the test first, confirms it fails for the right reason, then implements the minimum code to make it pass. Prevents the common pattern of writing code first and tests as an afterthought.

**Best for:**
- Any feature implementation or bugfix
- When test coverage matters
- When you want to catch regressions early

**How to invoke:** `/test-driven-development` before describing what to implement.

---

#### `/writing-plans`
**Source:** [github.com/obra/superpowers](https://github.com/obra/superpowers)

**What it does:** Turns a spec or requirement into a detailed, step-by-step implementation plan before touching any code. Identifies dependencies, sequences tasks correctly, and flags risks. The plan becomes the contract for implementation.

**Best for:**
- Any task touching 3+ files
- Before starting a new feature
- When the implementation path isn't obvious

**How to invoke:** `/writing-plans` then describe what needs to be built.

---

#### `/executing-plans`
**Source:** [github.com/obra/superpowers](https://github.com/obra/superpowers)

**What it does:** Executes a written plan with review checkpoints between phases. Confirms each step before moving to the next, flags deviations, and keeps the work aligned to the original plan.

**Best for:** After `/writing-plans` has produced a plan and you're ready to implement.

**How to invoke:** `/executing-plans` then paste or reference the plan.

---

#### `/brainstorming`
**Source:** [github.com/obra/superpowers](https://github.com/obra/superpowers)

**What it does:** A design-first exploration session before any implementation. Explores intent, generates multiple approaches, considers trade-offs, and produces a direction — before a single line of code is written.

**Best for:**
- Any creative work: new features, UI designs, architecture decisions
- When you're not sure of the best approach
- Required before building components or adding functionality

**How to invoke:** `/brainstorming` then describe what you're trying to create.

---

#### `/dispatching-parallel-agents`
**Source:** [github.com/obra/superpowers](https://github.com/obra/superpowers)

**What it does:** When facing 2+ independent tasks, dispatches them to parallel subagents rather than handling them sequentially. Significantly speeds up work that doesn't have shared state or sequential dependencies.

**Best for:**
- Multiple independent investigations or implementations
- Research tasks that can run simultaneously
- Parallelizable refactors

**How to invoke:** `/dispatching-parallel-agents` then describe the parallel tasks.

---

#### `/subagent-driven-development`
**Source:** [github.com/obra/superpowers](https://github.com/obra/superpowers)

**What it does:** Executes implementation plans by dispatching independent subagents for individual tasks, with code quality review checkpoints between iterations.

**Best for:** Executing plans where tasks can be distributed across subagents in the current session.

---

### Code Review

---

#### `/requesting-code-review`
**Source:** [github.com/obra/superpowers](https://github.com/obra/superpowers)

**What it does:** Reviews completed work before committing or creating a PR. Checks that code meets requirements, follows team standards, catches security issues, and verifies test coverage.

**Best for:**
- Before every commit on significant changes
- Before creating a PR
- When implementation is complete and needs a final check

**How to invoke:** `/requesting-code-review` when implementation is done.

---

#### `/receiving-code-review`
**Source:** [github.com/obra/superpowers](https://github.com/obra/superpowers)

**What it does:** When you've received code review feedback, this skill processes it with technical rigor — verifying that suggestions are correct, flagging feedback that seems questionable, and implementing only changes that are actually improvements. Prevents blind acceptance of review comments.

**Best for:** After receiving a code review, before implementing suggestions.

**How to invoke:** `/receiving-code-review` then paste the review feedback.

---

#### `/gsd-code-review`
**Source:** [github.com/gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done)

**What it does:** Deeper code review of all files changed during a GSD phase. Checks for bugs, security vulnerabilities, and code quality issues. Produces a structured findings report.

**Best for:** At the end of a GSD phase before shipping.

---

#### `/verification-before-completion`
**Source:** [github.com/obra/superpowers](https://github.com/obra/superpowers)

**What it does:** Requires running actual verification commands and confirming output **before** claiming any work is done, fixed, or passing. Prevents the common failure mode of claiming success without evidence.

**Best for:**
- Before saying "it's working"
- Before creating a PR
- Before committing a bugfix

**How to invoke:** `/verification-before-completion` before making any success claims.

---

### Debugging

---

#### `/systematic-debugging`
**Source:** [github.com/obra/superpowers](https://github.com/obra/superpowers)

**What it does:** Applies scientific method to debugging — forms hypotheses, tests them one at a time, narrows the root cause, and verifies the fix. Prevents the random-change approach that creates new bugs.

**Best for:**
- Any bug or unexpected behavior
- Test failures
- "This used to work" situations

**How to invoke:** `/systematic-debugging` then describe the bug and what you've tried.

---

#### `/gsd-debug`
**Source:** [github.com/gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done)

**What it does:** Persistent debug sessions with state management across context resets. For bugs that span multiple sessions or require extended investigation. Uses checkpoints to preserve findings.

**Best for:** Complex bugs that don't yield quickly or require investigation across multiple sessions.

---

### Browser & Integrations

---

#### `/browser-use`
**What it does:** Two capabilities in one:
1. **Browser automation** — controls a live Chrome session via agent-browser CLI, using stable element refs instead of fragile selectors. Tests form flows, responsive layouts, auth flows, and UI behavior end-to-end.
2. **MCP integrations** — connects Claude to GitHub, Slack, Linear, Jira, and 70+ other services via MCP servers or Composio.

**Best for:**
- Testing a UI after implementing it ("does this actually work?")
- Checking responsive behavior across viewports
- Connecting Claude to external services for automated workflows

**How to invoke:** `/browser-use` then describe what to test or which service to connect.

**Browser automation quick start:**
```bash
npx agent-browser open https://localhost:3000
npx agent-browser snapshot -i        # Get element refs
npx agent-browser click @e2           # Interact by ref
npx agent-browser screenshot --full-page
```

**MCP quick start** (add to `.claude/settings.json`):
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..." }
    }
  }
}
```

---

### Memory & Persistence

---

#### `/elephant`
**Source:** [github.com/tonone-ai/elephant](https://github.com/tonone-ai/elephant)

**What it does:** Persistent cross-session memory committed to the repository. Key facts, decisions, and context survive across conversations and can be shared with the team.

**Best for:**
- Saving architectural decisions
- Preserving important context across sessions
- Team-shared knowledge that should travel with the repo

**Commands:**
```
/elephant save "We're using Railway for deployment, not Vercel"
/elephant save !! "CRITICAL: Never commit .env files"
/elephant show
/elephant compact
```

---

### GSD Workflow (Large Projects)

GSD (Get Shit Done) is a full project management workflow for complex, multi-phase projects. 66 skills covering the full lifecycle.

**Source:** [github.com/gsd-build/get-shit-done](https://github.com/gsd-build/get-shit-done)

**Best for:** Projects with multiple phases, long timelines, teams, or complex requirements.

**Core workflow:**
```
/gsd-new-project          → Initialize with deep context gathering
  ↓
/gsd-discuss-phase        → Gather context for a phase
/gsd-spec-phase           → Clarify what the phase delivers
/gsd-plan-phase           → Create detailed implementation plan
/gsd-execute-phase        → Execute with wave-based parallelization
/gsd-verify-work          → Validate features through UAT
/gsd-ship                 → PR creation, review, merge
```

**Other frequently used GSD skills:**
| Skill | Purpose |
|-------|---------|
| `/gsd-progress` | Check status, advance workflow |
| `/gsd-debug` | Debug sessions with persistent state |
| `/gsd-code-review` | Phase-level code review |
| `/gsd-sketch` | Throwaway HTML mockups for UI ideas |
| `/gsd-resume-work` | Restore context from previous session |
| `/gsd-pause-work` | Handoff when pausing mid-phase |
| `/gsd-help` | Show all GSD commands |

---

## Installation

Skills are loaded from `~/.claude/skills/` automatically by Claude Code.

**Clone this repo:**
```bash
git clone https://github.com/Carefree-1991/claude-skills.git
```

**Copy skills to Claude's skills directory:**
```bash
# Copy all skill folders to ~/.claude/skills/
cp -r gsd/* ~/.claude/skills/
cp -r superpowers/* ~/.claude/skills/
cp -r elephant/elephant ~/.claude/skills/
cp -r qa ~/.claude/skills/
cp -r close ~/.claude/skills/
cp -r task-observer ~/.claude/skills/
cp -r frontend-design ~/.claude/skills/
cp -r shadcn-ui-feel ~/.claude/skills/
cp -r align ~/.claude/skills/
cp -r browser-use ~/.claude/skills/
cp -r build-skills ~/.claude/skills/
```

**Recommended CLAUDE.md additions:**
```markdown
## Skill Activation

At the start of any task-oriented session, invoke the task-observer skill before beginning work.

When loading any skill, check the observation log for OPEN observations tagged to that skill.
```

---

## Skill Interaction Patterns

Some skills work best together:

**Design workflow:**
```
/align → /brainstorming → /frontend-design or /shadcn-ui-feel → implement → /requesting-code-review
```

**Feature development:**
```
/align → /writing-plans → /test-driven-development → /executing-plans → /verification-before-completion → /requesting-code-review
```

**Bug fixing:**
```
/systematic-debugging → fix → /verification-before-completion → /requesting-code-review
```

**Large project phase:**
```
/gsd-discuss-phase → /gsd-spec-phase → /gsd-plan-phase → /gsd-execute-phase → /gsd-verify-work → /gsd-ship
```

---

## Contributing

To add a skill to this library:
1. Create a folder: `<skill-name>/SKILL.md` (or `<category>/<skill-name>/SKILL.md`)
2. Add `SKILL.md` with YAML frontmatter (`name`, `description`) and instructions
3. Copy to `~/.claude/skills/<skill-name>/` to make it active
4. Document it in this README under the appropriate category
5. Add its trigger pattern to `build-skills/SKILL.md` so it gets surfaced automatically

Skill format spec: [github.com/anthropics/skills/spec](https://github.com/anthropics/skills/tree/main/spec)
