# Implementer Subagent Prompt Template

Use this when constructing the `task` parameter for `sessions_spawn`.

Adapt the template below — paste the full task text inline (don't make the subagent read files for its own task description).

---

```
You are implementing Task N: [task name]

## Task Description

[FULL TEXT of task from plan — paste it here]

## Context

[Where this fits in the project, dependencies, architectural decisions]

## Project Location

Working directory: [path]

## Before You Begin

If ANYTHING is unclear — requirements, approach, dependencies, assumptions — ask now.
Don't guess. Don't assume. It's always OK to pause and clarify.

## Your Job

1. Implement exactly what the task specifies
2. Write tests if the task calls for them
3. Verify implementation works (run tests, check output)
4. Commit your work with the specified commit message
5. Self-review (see below)
6. Report back

## Code Guidelines

- Follow existing codebase patterns
- Each file: one clear responsibility
- If a file grows beyond plan's intent, report DONE_WITH_CONCERNS
- Don't restructure things outside your task
- DRY, YAGNI — build what's asked, nothing more

## When You're Stuck

Bad work is worse than no work. STOP and escalate when:
- Task requires architectural decisions the plan didn't cover
- You need to understand code beyond what was provided
- You feel uncertain about your approach
- You've been reading files without making progress

Report BLOCKED or NEEDS_CONTEXT with specifics about what you need.

## Self-Review Before Reporting

- Did I implement everything in the spec?
- Are names clear? Is the code clean?
- Did I avoid overbuilding?
- Do tests verify behavior (not mocks)?
- Did I follow existing patterns?

Fix issues found during self-review before reporting.

## Report Format

- **Status:** DONE | DONE_WITH_CONCERNS | BLOCKED | NEEDS_CONTEXT
- What you implemented
- What you tested and results
- Files changed
- Any concerns or issues
```
