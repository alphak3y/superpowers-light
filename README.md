# ⚡ Superpowers Light

A lean, opinionated software development workflow for AI coding agents. Inspired by [obra/superpowers](https://github.com/obra/superpowers), stripped down to the essentials, and adapted for [OpenClaw](https://github.com/openclaw/openclaw).

No dogma. No 500-line skill files. Just the parts that actually make agents write better code.

## What It Does

| Phase | What | Why |
|-------|------|-----|
| **Design** | Spec-before-code via collaborative Q&A | Prevents "agent builds the wrong thing" |
| **Plan** | Bite-sized tasks with exact file paths and complete code | Zero-context engineers can follow it |
| **Execute** | Fresh subagent per task + two-stage review (spec → quality) | Clean context, high quality |
| **Debug** | 4-phase root cause investigation before any fix | Stops random "let me try this" patches |

## What's Different from Superpowers

Superpowers is excellent — thorough, battle-tested, and well-documented. But it was built for Claude Code and can be heavy for other setups. This fork:

- **Drops git worktrees** — useful for some workflows, unnecessary complexity for most
- **Drops rigid TDD enforcement** — testing is strongly recommended, not "delete your code if you wrote it before a test"
- **Drops the meta-skill layer** — no "invoke skill before ANY response, even at 1% relevance"
- **Adapts subagent patterns** — uses `sessions_spawn` (OpenClaw) instead of Claude Code's `Task` tool
- **Caps review loops** — 3 iterations max, then escalate (vs. unbounded)
- **Flexible testing** — strict TDD for APIs/business logic, visual verification OK for UI work

The result is ~120 lines of core workflow + 3 reference files, vs. thousands of lines across 14 skills.

## Installation

### OpenClaw

Copy the `skills/dev-workflow` directory into your workspace's `skills/` folder:

```bash
cp -r skills/dev-workflow ~/.openclaw/workspace/skills/
```

OpenClaw agents will automatically discover and use it.

### Other Platforms

The skill is a standard markdown-based workflow. Drop `skills/dev-workflow/SKILL.md` into your agent's context (AGENTS.md, CLAUDE.md, system prompt, etc.) and reference the files in `references/` as needed.

## Skill Structure

```
skills/dev-workflow/
├── SKILL.md                        # Core workflow (~120 lines)
└── references/
    ├── implementer-prompt.md       # Subagent task dispatch template
    ├── reviewer-prompts.md         # Spec compliance + code quality review templates
    └── debugging.md                # Systematic debugging process
```

## The Philosophy

1. **Spec before code.** Even "simple" things. The spec can be 3 sentences — but write it down.
2. **Fresh context per task.** Subagents don't accumulate confusion from prior tasks.
3. **Two-stage review.** First: did you build what was asked? Second: is it clean?
4. **Root cause before fix.** No guessing. Investigate, hypothesize, test, then fix.
5. **Right-sized rigor.** TDD for business logic, visual checks for UI. Match the tool to the job.

## Credits

Heavy inspiration from [Jesse Vincent's Superpowers](https://github.com/obra/superpowers). If you use AI coding agents professionally, check out the original — it's the most comprehensive agentic dev workflow out there. Consider [sponsoring Jesse's work](https://github.com/sponsors/obra).

## License

MIT
