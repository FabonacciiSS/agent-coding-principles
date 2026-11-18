# Checklist

Print this. Tape it to the monitor.

## Before you write code

- [ ] Read the existing pattern in this area. Do not invent a parallel one.
- [ ] Check what's already enforced: lint, types, tests, dependency boundaries.
- [ ] If the area has no guardrails and a cheap one exists, add it first.
- [ ] Am I about to write a "temporary" workaround? → fix the architecture instead.
- [ ] Am I about to use a banned API (`useEffect` in product code, cross-layer imports, imperative timers as patches)?

## Before you submit

- [ ] No new comments justifying shortcuts. Lesson recorded as a lint rule instead?
- [ ] No `eslint-disable` / `@ts-ignore` / `# noqa` / suppression added.
- [ ] Root cause fixed, not the symptom.
- [ ] Scope not widened: one correction ≠ global rewrite.
- [ ] Verification artifacts exist: test output, screenshot, video, state chart, benchmark, trace.
- [ ] Did this change make the codebase a better or worse input for the next agent?

## When reviewing another agent's work

- [ ] Does the diff copy an established good pattern, or introduce a new one?
- [ ] Any workaround that will become precedent?
- [ ] Any comment that will go stale or excuse a shortcut?
- [ ] Does the verification artifact actually demonstrate the claimed behavior, or just that the process ran?
- [ ] Is the change trying to satisfy too broad a rule from too narrow a signal?

## Every correction → one of these

1. Lint rule / type / test
2. API design or architecture change that eliminates the possibility
3. Prose (last resort, and know it is not enforceable)
