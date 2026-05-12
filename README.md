# claude-skills

Personal Claude Code skills library for use across projects.

## What this is

Each folder is a skill — a reusable behavioral instruction set that Claude Code
can load during a session. Skills are stored as `SKILL.md` files and installed
into `.claude/skills/` in your project or globally at `~/.claude/skills/`.

## Skills

| Skill | Description |
|-------|-------------|
| [task-observer](./task-observer/SKILL.md) | Meta-skill that watches your work sessions and continuously improves your skill library. Logs corrections, gaps, and patterns. Also known as "One Skill to Rule Them All" by [Eoghan Henn](https://rebelytics.com). |

## Installing a skill

### Globally (available in all projects)

```bash
cp -r task-observer ~/.claude/skills/
```

### In a specific project

```bash
cp -r task-observer /path/to/your/project/.claude/skills/
```

## Activating task-observer automatically

Add this to your `~/.claude/CLAUDE.md` (or the project's `CLAUDE.md`):

```
At the start of any task-oriented session — any interaction where you will
use tools and produce deliverables — invoke the task-observer skill before
beginning work. This ensures skill improvement opportunities are captured
throughout the session.

When loading any skill, check the observation log for OPEN observations
tagged to that skill. Apply their insights to the current work, even if
the skill file hasn't been updated yet.
```

## Adding a new skill

1. Create a folder with the skill name
2. Add a `SKILL.md` file inside it
3. Commit and push
4. Install it globally or per-project as above
