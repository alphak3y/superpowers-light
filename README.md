# ⚡ Superpowers Light

A lean, opinionated software development workflow for AI coding agents. Inspired by [obra/superpowers](https://github.com/obra/superpowers), stripped down to the essentials, and adapted for [OpenClaw](https://github.com/openclaw/openclaw).

No dogma. No 500-line skill files. Just the parts that actually make agents write better code.

## Skills

```
skills/
├── using-superpowers-light/    # How the skills connect — start here
├── brainstorming/              # Spec-before-code via collaborative design
├── writing-plans/              # Break specs into bite-sized tasks
├── subagent-development/       # Fresh agent per task + two-stage review
│   └── references/
│       ├── implementer-prompt.md
│       ├── spec-reviewer-prompt.md
│       └── quality-reviewer-prompt.md
├── code-review/                # Spec compliance + code quality verification
└── systematic-debugging/       # Root cause investigation before fixes
```

### Workflow

```
User request → brainstorming → writing-plans → subagent-development
                                                      ↓
                                               code-review (per task)
                                                      ↓
                                                    Done

Bug/issue → systematic-debugging → fix → code-review
```

| Skill | When It Triggers |
|-------|-----------------|
| `brainstorming` | Building something new, adding features, modifying behavior |
| `writing-plans` | Approved spec exists, need implementation tasks |
| `subagent-development` | Plan with independent tasks ready to execute |
| `code-review` | Implementation complete, needs verification |
| `systematic-debugging` | Bug, test failure, unexpected behavior |

## What's Different from Superpowers

Superpowers is excellent — thorough, battle-tested, and well-documented. But it was built for Claude Code and can be heavy for other setups. This fork:

- **6 focused skills** instead of 14 — merged related concerns, dropped what most teams don't need
- **Drops git worktrees** — useful for some workflows, unnecessary complexity for most
- **Drops rigid TDD enforcement** — testing is strongly recommended, not "delete your code if you wrote it before a test"
- **Drops the meta-skill layer** — no "invoke skill before ANY response, even at 1% relevance"
- **Adapts for OpenClaw** — subagent patterns use `sessions_spawn` instead of Claude Code's `Task` tool
- **Caps review loops** — 3 iterations max, then escalate (vs. unbounded)
- **Flexible testing** — strict TDD for APIs/business logic, visual verification OK for UI work

~120 lines per skill. Total repo under 500 lines of skill content.

## Installation

### OpenClaw

Copy the skills you want into your workspace:

```bash
# All skills
cp -r skills/* ~/.openclaw/workspace/skills/

# Or cherry-pick
cp -r skills/brainstorming ~/.openclaw/workspace/skills/
cp -r skills/subagent-development ~/.openclaw/workspace/skills/
```

OpenClaw agents discover skills automatically.

### Other Platforms

These are standard markdown skills. Drop the `SKILL.md` files into your agent's context (AGENTS.md, CLAUDE.md, system prompt, etc.) and reference the files in `references/` as needed.

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
