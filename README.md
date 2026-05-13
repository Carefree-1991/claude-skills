# claude-skills

Personal Claude Code skills and commands library — install on any machine to keep your workflow consistent.

## Structure

| Folder | Type | Invoked with |
|--------|------|-------------|
| `skills/` | Skills | `/skill-name` or listed in CLAUDE.md |
| `commands/` | Slash commands | `/command-name` |

---

## Commands

### `/close` — Session Closing Ritual

Runs a structured end-of-session ritual: checks for stale files, updates project memory, writes a `close-handoff.md` so the next session picks up exactly where you left off.

**Install globally (works in all projects):**

```bash
cp ~/.claude/commands/close.md /path/downloaded/commands/close.md
# or directly:
curl -o ~/.claude/commands/close.md \
  https://raw.githubusercontent.com/Carefree-1991/claude-skills/main/commands/close.md
```

**What it does:**
1. Scans for redundant/stale files — flags only, never deletes
2. Updates `~/.claude/projects/.../memory/MEMORY.md` with durable session facts
3. Catches any markdown update requests you made that did not get applied
4. Checks for dirty state (temp files, scratch work)
5. Writes `close-handoff.md` in the project root with: what was done, what is deferred, key decisions, and exact resume instructions
6. Self-checks that resume instructions match the deferred list

---

## Skills

### `task-observer` — Continuous Skill Improvement

Meta-skill that watches your sessions and logs correction opportunities. Also known as "One Skill to Rule Them All" by [Eoghan Henn](https://rebelytics.com).

**Install globally:**

```bash
cp -r task-observer ~/.claude/skills/
```

**Activate automatically** — add to `~/.claude/CLAUDE.md`:

```
At the start of any task-oriented session — any interaction where you will
use tools and produce deliverables — invoke the task-observer skill before
beginning work. This ensures skill improvement opportunities are captured
throughout the session.
```

---

## Installing everything at once

```bash
# Clone the repo
git clone https://github.com/Carefree-1991/claude-skills.git

# Install command(s)
cp claude-skills/commands/close.md ~/.claude/commands/close.md

# Install skill(s)
cp -r claude-skills/task-observer ~/.claude/skills/
```

## Adding a new skill or command

1. Add your file to `skills/<name>/SKILL.md` or `commands/<name>.md`
2. Commit and push
3. Pull and install on any machine with the commands above

