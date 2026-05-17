---
name: align
description: Clarify requirements before implementing anything. Use this skill when starting any ambiguous task — feature requests, UI changes, refactors, architectural decisions, or any work where the desired outcome isn't crystal clear. Asks concise numbered/lettered clarifying questions, waits for answers, confirms understanding, then proceeds. Prevents rework by ensuring perfect alignment before a single line of code is written.
---

# Align — Clarify Before You Build

Never implement before you understand. This skill creates a structured Q&A checkpoint between the user's request and any actual work.

## When to Invoke

Use this skill at the start of any task where:
- The requirement uses vague language ("make it better", "refactor this", "add a feature")
- Multiple valid interpretations exist
- Edge case behavior is unspecified
- Design or UX decisions are implied but not stated
- The scope boundary is unclear

## The Three-Phase Process

### Phase 1 — Analyze Silently

Before asking anything, identify:
1. What is the core deliverable?
2. What is ambiguous or underspecified?
3. Which decisions would most affect the outcome?
4. What would you assume by default — and should you?

Do not write code or make changes during this phase.

### Phase 2 — Ask Clarifying Questions

Present questions as a labeled list. **Maximum 5 questions.** Each question must:
- Have a clear label: **A.** **B.** **C.** etc.
- Be a single, concrete question — no compound questions
- Include the default assumption in parentheses when applicable: *(default: X)*
- Cover the most impactful unknowns first (most critical question = A)

**Required format:**

```
Before I start, a few quick questions:

**A.** [Most impactful question] *(default: [assumption I'd make])*

**B.** [Second question] *(default: [assumption])*

**C.** [Third question — no default if truly open-ended]*

Answer any you know — I'll use sensible defaults for the rest.
```

### Phase 3 — Confirm Understanding

After receiving answers, before touching any code or files:

1. Summarize what you'll build in 3–5 bullet points
2. State any remaining assumptions explicitly
3. End with: *"Does this match your intent?"* or *"Ready to proceed — say go."*

Only start implementation after the user confirms or explicitly says to proceed.

## Rules

- **5 questions max** — prioritize ruthlessly; every extra question costs the user time
- **Always offer defaults** — so the user can answer in one word or skip entirely
- **Don't ask what you can read** — never ask about things discoverable from the codebase
- **Don't ask the obvious** — don't confirm things clearly stated in the request
- **No compound questions** — "Do you want X and also Y?" counts as two questions; split them
- **No implementation while waiting** — don't start any work until Phase 3 is complete
- **If user says "just do it"** — state your assumptions upfront in a short block, then proceed

## Anti-Patterns to Avoid

- Asking questions just to show you read the prompt
- Multi-part questions disguised as single questions
- Asking about things you can infer from context or code
- Starting implementation "in the background" while asking questions
- Asking the same clarifying question twice with different wording
- More than 5 questions regardless of task complexity

## Quick Reference

| Situation | Action |
|-----------|--------|
| Clear, specific request | Skip this skill — implement directly |
| Vague or ambiguous request | Ask A–C (max 3 questions) |
| Complex multi-part request | Ask A–E (max 5 questions) |
| User says "just go" | State assumptions, proceed |
| Answers received | Summarize → confirm → implement |
