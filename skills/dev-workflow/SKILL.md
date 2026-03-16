---
name: dev-workflow
description: "Structured software development workflow for OpenClaw agents. Use when building features, fixing bugs, or implementing multi-step code changes. Enforces spec-before-code, bite-sized planning, subagent execution with review loops, and systematic debugging. Inspired by obra/superpowers, adapted for OpenClaw sessions_spawn."
---

# Dev Workflow

Structured development process: design → plan → execute → review. Prevents agents from jumping straight into code without understanding the problem.

## When to Use

- Building new features or components
- Multi-file code changes
- Bug fixes that aren't trivial one-liners
- Any task where "just start coding" would likely produce rework

## The Three Phases

### Phase 1: Design (Spec Before Code)

**No code without a signed-off spec.** Even "simple" projects need one — it can be short.

1. **Explore context** — read relevant files, docs, recent changes
2. **Ask clarifying questions** — one at a time, prefer multiple choice
3. **Propose 2-3 approaches** — with trade-offs and a recommendation
4. **Present design in digestible sections** — get approval after each
5. **Write spec** — save to project's `docs/specs/YYYY-MM-DD-<topic>.md`

Design for isolation: small units, clear interfaces, one responsibility per file. In existing codebases, follow established patterns.

**Anti-pattern:** "This is too simple for a spec." That's where unexamined assumptions cause the most rework. The spec can be 3 sentences — but write it down.

### Phase 2: Plan (Bite-Sized Tasks)

Break the approved spec into tasks an engineer with zero codebase context could follow.

**Task granularity:** Each task = 2-10 minutes of work. Include:
- Exact file paths (create/modify)
- Complete code (not "add validation")
- Verification steps with expected output
- Commit message

Save plan to `docs/plans/YYYY-MM-DD-<feature>.md`.

**Plan header template:**

```markdown
# [Feature] Implementation Plan

**Goal:** [One sentence]
**Architecture:** [2-3 sentences]
**Tech Stack:** [Key technologies]

---
```

**Task template:**

```markdown
### Task N: [Name]

**Files:** Create `path/to/file.ts` | Modify `path/to/existing.ts`

- [ ] Step 1: [Action with complete code]
- [ ] Step 2: [Verify — command + expected output]
- [ ] Step 3: [Commit]
```

Principles: DRY, YAGNI. Don't plan features nobody asked for.

### Phase 3: Execute (Subagent-Driven Development)

Dispatch a fresh subagent per task via `sessions_spawn`. Fresh context prevents confusion accumulation.

**Execution loop per task:**

```
1. Spawn implementer subagent (see references/implementer-prompt.md)
2. If DONE → spawn spec reviewer (see references/reviewer-prompts.md)
3. If spec review passes → spawn quality reviewer
4. If quality review passes → mark task complete, next task
5. If any review fails → fix and re-review (max 3 iterations, then escalate)
```

**Handling implementer status:**
- **DONE** → proceed to review
- **DONE_WITH_CONCERNS** → read concerns, address if substantive, then review
- **NEEDS_CONTEXT** → provide missing info, re-dispatch
- **BLOCKED** → try: more context, more capable model, smaller task, or escalate to human

**Model selection (cost optimization):**
- Mechanical tasks (1-2 files, clear spec) → fast/cheap model
- Integration tasks (multi-file, pattern matching) → standard model
- Architecture/review tasks → most capable model

**After all tasks complete:** Spawn a final quality reviewer for the entire implementation.

## Debugging Mode

When hitting bugs, **stop and investigate before fixing.** See `references/debugging.md` for the full process.

Quick version:
1. **Investigate** — read errors carefully, reproduce, check recent changes, trace data flow
2. **Analyze** — find working examples, compare differences, understand dependencies
3. **Hypothesize** — form one specific hypothesis, test with smallest possible change
4. **Fix** — address root cause (not symptom), verify fix, check for regressions

**Iron law:** No fix without root cause. "Quick patches" create new bugs.

## Testing Guidelines

Tests are strongly recommended. The workflow supports TDD when appropriate:

- **New features:** Write test → verify it fails → implement → verify it passes
- **Bug fixes:** Write test reproducing the bug → verify it fails → fix → verify it passes
- **Refactoring:** Ensure existing tests pass before and after

For marketing sites and UI-heavy work, visual verification + integration tests are acceptable. Strict TDD is best suited for libraries, APIs, and business logic.

## Integration with OpenClaw Agency

When working within the Pixel/Stack/Signal agency:

- **Pixel** uses Phase 1 (design) for UI/UX specs
- **Stack** uses all three phases for implementation
- **Signal** uses Phase 1 for research specs, Phase 2 for action plans
- Handoffs between agents should include the spec document path
- Shared specs live in the project's `docs/specs/` directory
