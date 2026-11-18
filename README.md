# agent-coding-principles

Operating rules for coding agents, distilled from Lauren ("Potato")'s talk
**"Turning slop into gold"** (Graphbot, SpaceX).

The claim of that talk, in one line: **you don't make agents reliable by trusting them more — you make them
reliable by constraining their environment so the correct action is the default.**

## Files

| File | Purpose |
|---|---|
| `PRINCIPLES.md` | Full doctrine: trust pyramid, garden model, enforceable architecture, verification layers, ordering, outcomes, checklist, anti-patterns |
| `AGENTS.md` | Drop-in file for the root of any repo. Point your coding agent here. |
| `CHECKLIST.md` | Short pre-flight / pre-submit checklist, printable |
| `skill/SKILL.md` | Reusable skill wrapper for agents that support skills |

## How to use this in a local repo

Fastest path — make your coding agent read it every session:

```bash
# in your project root
curl -o AGENTS.md https://raw.githubusercontent.com/FabonacciiSS/agent-coding-principles/main/AGENTS.md
```

Or, if you already have an `AGENTS.md` / `CLAUDE.md` / `.cursorrules`, don't overwrite it. Append a pointer:

```markdown
## Coding principles

Follow https://github.com/FabonacciiSS/agent-coding-principles — in short:
enforce in code (lint/types/tests), not in prose; fix root causes, never leave
workarounds; comments are an escape hatch, prefer lint rules with teaching messages.
```

Claude Code users can symlink: `ln -s AGENTS.md CLAUDE.md`.

## The five rules that matter most

1. Every time you correct the agent, try to turn the correction into a **deterministic check** rather than
   another line of prose. Prose is advice; code is law.
2. **Never leave a workaround in the codebase.** Agents copy whatever pattern is already there.
3. **Ban APIs that require nuance** (e.g. `useEffect` in product code) instead of reviewing for their misuse.
4. **Almost no comments.** Encode the lesson as a lint rule with an error message that names the root cause.
5. **Verification, not diff review.** Ship artifacts: test runs, screenshots, videos, state charts, benchmarks.
   Expect the majority of token spend to go here.

## What this buys you

Graphbot's reported numbers after six months of this discipline: ~11,000 PRs/month, a stable release about
once a day, ~227 contributors/month, revert rate *down* to ~0.5%, and improved startup/typing/chat-switch
performance. Most merged product code no longer needs deep human review.

## Credit

All ideas are Lauren's. This repo is a written-down operating manual for them, not original research.
Video: https://www.youtube.com/watch?v=O_zcc6Xl12g
