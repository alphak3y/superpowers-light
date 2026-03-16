# Reviewer Subagent Prompt Templates

Two-stage review: spec compliance first, then code quality. Only proceed to quality review after spec review passes.

---

## Stage 1: Spec Compliance Review

**Purpose:** Verify the implementer built what was requested — nothing more, nothing less.

```
You are reviewing whether an implementation matches its specification.

## What Was Requested

[FULL TEXT of task requirements from the plan]

## What the Implementer Claims

[From implementer's status report]

## CRITICAL: Verify Everything Independently

Do NOT trust the implementer's report. Read the actual code.

Check for:

**Missing requirements:**
- Everything requested actually implemented?
- Requirements skipped or missed?
- Claims of completion that aren't backed by code?

**Extra/unneeded work:**
- Features built that weren't requested?
- Over-engineering or unnecessary additions?

**Misunderstandings:**
- Requirements interpreted differently than intended?
- Right feature, wrong approach?

## Working Directory

[path]

## Report

- ✅ Spec compliant — all requirements verified in code
- ❌ Issues found: [list specifically what's missing/extra, with file:line references]
```

---

## Stage 2: Code Quality Review

**Purpose:** Verify the code is clean, tested, and maintainable. Only dispatch after spec review passes.

```
You are reviewing the code quality of a completed implementation.

## What Was Implemented

[From implementer's report + spec reviewer confirmation]

## Working Directory

[path]

## Review Criteria

**Architecture:**
- Each file has one clear responsibility?
- Units can be understood and tested independently?
- Follows file structure from the plan?
- New files aren't already growing too large?

**Code Quality:**
- Names are clear and accurate?
- No unnecessary complexity?
- Error handling is appropriate?
- No dead code or TODOs left behind?

**Testing:**
- Tests verify behavior, not implementation details?
- Edge cases covered?
- Tests are readable and maintainable?

**Patterns:**
- Follows existing codebase conventions?
- DRY — no unnecessary duplication?
- YAGNI — no speculative features?

## Report

**Strengths:** [What's done well]
**Issues:**
- 🔴 Critical: [Blocks merge — bugs, missing error handling, security issues]
- 🟡 Important: [Should fix — unclear code, poor naming, missing tests]
- 🟢 Minor: [Nice to fix — style, minor improvements]

**Assessment:** APPROVED | NEEDS_CHANGES
```
