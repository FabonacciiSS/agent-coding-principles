# AGENTS.md — Agent Coding Principles

> Drop this file in the root of a repo (or append it to your existing `AGENTS.md` / `CLAUDE.md`).
> Full doctrine: https://github.com/FabonacciiSS/agent-coding-principles/blob/main/PRINCIPLES.md

You are working in a repo where **the environment constrains you, not your memory of the rules.**
Follow the priority order below. When rules conflict, the lower layer wins.

## Priority order (bottom-up)

1. **Code is law.** Lint rules, types, compiler, tests, architecture boundaries. Obey them.
2. **Verification is evidence.** Runtime checks, e2e, property tests, model checking.
3. **Prose is advice.** This file, comments, docs. Lowest authority, and decays.

## Hard rules

1. **Copy the established good pattern.** Before writing new code, find how this repo already does it.
   Never introduce a parallel pattern for the same problem.
2. **Never leave a workaround.** No "temporary" hacks, no dead code kept "for later," no
   deprecated pattern left in place. If the correct fix is architectural, say so and fix the architecture.
3. **Do not use APIs this repo bans** (e.g. `useEffect` in product code, imperative timers as patches,
   cross-layer imports). If you believe you need one, stop and explain why before proceeding.
4. **Do not write explanatory comments.** Nearly all comments are banned. If a lesson is worth recording,
   it belongs in a lint rule with a message that names the root cause. Allowed exceptions: public API
   contracts, links to specifications, non-obvious algorithms that cannot be made obvious.
5. **Fix the root cause, never patch the symptom.** If a lint rule fires, change the design so the
   rule is satisfied — do not add a suppression, `eslint-disable`, `@ts-ignore`, `# noqa`, or
   equivalent unless the repo has an explicit, documented escape hatch and a human approved it.
6. **Do not over-rotate.** One correction from a human is a scoped signal, not a universal law.
   Do not refactor the world to satisfy one local fix. Narrow rules beat broad rewrites.
7. **Verify before you claim success.** Produce artifacts: test output, screenshots, videos, state
   charts, benchmarks, traces. "It should work" is not verification.

## When you correct yourself or another agent

Ask, in this order:

1. Can this become a **lint rule / type / test**? Do that.
2. Can this be **categorically eliminated** by better API design or architecture? Do that.
3. Only then: write it down as prose.

## When you disagree with a rule

Say so explicitly, with evidence, and propose the narrower rule or the better check.
Do not silently bypass it.

## Scope reminders

- Small, reviewable changes. One concern per change.
- Delete more than you add when possible. The best code is absent.
- If a change makes the codebase a worse input for the next agent, it is a bad change — even if it works.

---

Source: Lauren ("Potato"), Graphbot @ SpaceX, *Turning slop into gold*.
