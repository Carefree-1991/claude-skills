---
name: build-skills
description: Activate skill-aware development mode for the entire session. Say "build skills" or /build-skills at the start of any work session. Once active, proactively surfaces the right skill before each task — asking whether to use it before proceeding. Works as a concierge for the full skill library: alignment, design, testing, debugging, code review, browser testing, and workflow tools.
---

# Build Skills — Skill-Aware Development Mode

When this skill is invoked, you enter **skill-aware mode** for the entire session.

This means: before starting any non-trivial task, you will check whether a skill applies, name it, explain what it will do, and give the user a quick accept/skip option. You will never silently use a skill — you will always surface it and let the user decide.

---

## On Activation

Acknowledge activation with a brief summary:

```
Skill-aware mode active. I'll offer relevant skills before each task.

Available this session:
  Design      → /frontend-design, /shadcn-ui-feel
  Alignment   → /align
  Dev process → /test-driven-development, /writing-plans, /executing-plans,
                /requesting-code-review, /receiving-code-review,
                /verification-before-completion
  Debugging   → /systematic-debugging, /gsd-debug
  Workflow    → /brainstorming, /dispatching-parallel-agents,
                /gsd-plan-phase, /gsd-execute-phase, /gsd-ship
  Integration → /browser-use, /claude-api
  Memory      → /elephant, /task-observer

Ready. What are we building?
```

---

## Proactive Suggestion Protocol

Before starting any task, scan the request against the trigger patterns below. If a skill applies, surface it using this exact format:

```
💡 [Skill name] available
[One sentence on what the skill will do for this specific task.]
→ "use it" to activate | "skip" to proceed without | "what is it?" for details
```

Wait for the user's response before proceeding. Do not start the task while waiting.

If the user says **"use it"** or equivalent → invoke the skill with the Skill tool, then proceed.
If the user says **"skip"** or equivalent → proceed without the skill, no further mention.
If the user says **"what is it?"** → give a 3–4 sentence explanation, then re-offer.
If the user says **"go"** or **"just do it"** → proceed without offering skills for that specific task.

---

## Trigger Patterns

Match the user's request against these patterns and surface the corresponding skill:

### Alignment
| Pattern | Offer |
|---------|-------|
| Vague request ("make it better", "add a feature", "refactor this") | `/align` |
| Multiple interpretations possible | `/align` |
| Scope or behavior not specified | `/align` |

### Design
| Pattern | Offer |
|---------|-------|
| Building any web UI, page, component, or landing page | `/frontend-design` |
| Building with shadcn/ui, Tailwind, React, Next.js | `/shadcn-ui-feel` |
| Request mentions "not look AI" or "look designed" | `/shadcn-ui-feel` + `/frontend-design` |
| Starting any creative or visual output | `/brainstorming` |

### Development Process
| Pattern | Offer |
|---------|-------|
| Implementing a new feature or bugfix | `/test-driven-development` |
| Complex task spanning multiple files or systems | `/writing-plans` |
| Have a written plan, ready to execute | `/executing-plans` |
| Multiple independent tasks in one session | `/dispatching-parallel-agents` |
| About to commit or create a PR | `/verification-before-completion` |
| Feature complete, needs review | `/requesting-code-review` |
| Received code review feedback | `/receiving-code-review` |

### Debugging
| Pattern | Offer |
|---------|-------|
| Bug report, test failure, or unexpected behavior | `/systematic-debugging` |
| Bug persists across multiple sessions | `/gsd-debug` |

### Integration & Browser
| Pattern | Offer |
|---------|-------|
| "Test this in the browser", "does this work", "check the UI" | `/browser-use` |
| Connecting to GitHub, Slack, Jira, Linear, or any external API | `/browser-use` |
| Code imports `anthropic` or uses Claude API | `/claude-api` |

### GSD Workflow (Large Projects)
| Pattern | Offer |
|---------|-------|
| Starting a brand new project from scratch | `/gsd-new-project` |
| Planning a phase of work | `/gsd-plan-phase` |
| Executing a GSD plan | `/gsd-execute-phase` |
| Done with a phase, shipping | `/gsd-ship` |

---

## Session Awareness Rules

1. **Never offer the same skill twice in one session** unless the user re-asks.
2. **Never offer more than one skill at a time** — if multiple apply, pick the most impactful one.
3. **Never offer a skill for trivial tasks** — answering a question, reading a file, small one-liner edits don't need skills.
4. **Never interrupt mid-task** to offer a skill — surface it before starting, not during.
5. **If the user invokes a skill directly** (`/skill-name`), don't re-offer it.
6. **If the user says "no more suggestions"** — disable proactive offers for the rest of the session and just do the work.

---

## Quick Reference

The user can also ask directly at any point:
- **"What skills do I have?"** → show the full categorized list above
- **"What's `/skill-name` for?"** → explain that specific skill in 3–4 sentences
- **"Use `/skill-name`"** → invoke it immediately without asking
- **"No skills"** → disable proactive offers for the rest of the session
