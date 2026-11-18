---
name: agent-coding-principles
description: Enforce agent-friendly codebase discipline (lint over prose, root-cause fixes, verification artifacts) on any coding task
---

# Agent Coding Principles

Use this skill for any coding task in any repo. Full doctrine lives in this repo's `PRINCIPLES.md`;
this file is the operative subset.

## Before touching code

1. Read the repo's existing patterns in the target area. Copy the good one. Never create a parallel pattern.
2. Check what is already enforced: lint config, tsconfig strictness, tests, dependency boundaries.
3. Read any existing `AGENTS.md` / `CLAUDE.md` / `.cursorrules` in the repo root and obey it.
4. If the target area has no guardrail and a cheap one exists (lint rule, type, test), add it first.

## During implementation

- Fix root causes. A suppression (`eslint-disable`, `@ts-ignore`, `# noqa`, `// eslint-disable-next-line`,
  cast to `any`) is a failure unless a human explicitly approved it.
- Do not add comments that justify a shortcut. Nearly all explanatory comments are banned.
  If the lesson matters, it belongs in a lint rule with a teaching message.
- Do not use APIs the repo bans, or APIs known to require nuance (`useEffect` in product code,
  imperative timers as patches, cross-layer imports).
- Do not leave "temporary" workarounds, dead code, or abandoned experiments. Delete them.
- Do not over-rotate: one human correction is scoped, not a universal law. Do not refactor the world.
- Prefer deletion over addition. The best code is absent.

## Before claiming success

Produce verification artifacts, not assurances:

- test output (actual run, not "should pass")
- screenshot / video / trace for UI changes
- benchmark numbers for perf-sensitive changes
- state chart / diagram for flow changes

Then ask: is there a deterministic check that would have caught the mistake I almost made? Add it.

## When correcting another agent or yourself

In this order:
1. Lint rule / type / test
2. API design or architecture change that makes the error impossible
3. Prose (last resort — unenforceable, decays silently)

## Escalation

If the correct fix is architectural, say so and propose the change instead of patching locally.
If a rule seems wrong, argue it with evidence and propose a narrower version. Never silently bypass.
