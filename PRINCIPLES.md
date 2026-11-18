# Agent Coding Principles

> Source: talk by Lauren ("Potato"), Graphbot @ SpaceX, *Turning slop into gold*.
> These are operating rules for any coding agent (or human) working in a repo.
> Read this before you write code. Follow the priority order literally.

---

## 0. The one-line thesis

The meta-skill of a software engineer is **maintaining or improving code quality while massively increasing throughput**.
You do not get there by trusting agents more. You get there by investing in the agent's *environment* so the agent
does the right thing **by default**.

> If your inner coding loop produces slop, automating it just produces more slop — and you burn more tokens cleaning up.

---

## 1. The trust pyramid (spend effort bottom-up)

| Layer | Contents | Nature | How much to use |
|---|---|---|---|
| 1. Code | API design, architecture, refactoring, lint rules, type systems, compilers, tests | Deterministic | Most of your effort |
| 2. Verification | Runtime checks, integration/e2e, property-based tests, formal verification | Semi-deterministic | Heavy, and worth the tokens |
| 3. Prose | rules files, skills, AGENTS.md, comments | Judgment, unenforceable | Least; last resort |

**The iron rule:** every time you correct the agent, ask *"can this become a deterministic check?"*
Before adding one more line to AGENTS.md.

Prose is advice. Code is law. Prefer law.

---

## 2. The codebase is a garden

Your codebase is the largest input the agent sees. It will copy whatever pattern it finds, including
workarounds, anti-patterns, deprecated code, and dead tech debt — because it looks "established."

Therefore:

- Keep the codebase as pristine as possible. Agents copy the good stuff only if the good stuff is what's there.
- When you find a weed, remove the pattern everywhere, then add a lint rule so it cannot regrow.
- Never leave a "temporary" workaround in place. In an agent-driven repo, temporary becomes permanent by default.

---

## 3. Enforceable architecture

### 3.1 Ban APIs that require nuance
Anything that needs taste to use correctly will be misused. Classic example: `useEffect`.
Solution: **ban it in product code**, allow it only inside the framework/internal layer.

This converts a recurring code-review judgment call into a categorically impossible mistake.

### 3.2 Directory structure is context
Make the architecture visible in the filesystem, then let tooling enforce it.

- Two top-level dirs (`electron/main`, `electron/renderer`) → cross imports banned by
  dependency-graph analyzer + lint rule.
- One simple lint rule removes one whole class of review reasoning.

### 3.3 Architecture quality is agent performance
A bad architecture choice (imperative VSCode style vs. declarative React style) caused agents to patch
over the clash with `setTimeout` / `requestAnimationFrame` / `requestIdleCallback`. Rapid timer
creation/destruction → major GC → jank.

The architecture made both the human-written client *and* the agent worse. Fix the architecture, not the symptom.

---

## 4. Comments are not enforcement — ban almost all of them

Why comments are dangerous in an agent codebase:

1. They go stale fast.
2. Humans do not read comment slop.
3. They are an **escape hatch**: agents write a comment justifying a shortcut, then cite it as precedent.
4. They are **not enforceable**. A well-intentioned "don't do X here" is worth nothing.

Instead:

- Encode the lesson as a **lint rule with a helpful error message that points at the root cause**,
  telling the agent to fix the real problem rather than patch around it.
- Ban ~99% of comments in application code. Allowed only where a rule genuinely cannot express the intent
  (e.g. public API doc contracts, links to specs).

**Watch for over-rotation:** agents will turn one small correction into an absolute, eternal hard rule
(e.g. globally rewriting the architecture to avoid one local issue). When you review, correct scope
creep, and prefer narrowing the rule over widening it.

---

## 5. Verification is where the tokens go

Code review shrinks into **reviewing verification artifacts**: videos, screenshots, state charts,
diagrams, formal proofs. Expect ~60-70% of token spend to be verification. That is correct and cheap
compared to shipping bugs.

Layers to build, in order:

1. **Runtime verification** — let the agent run the code and use the same debugging tools humans use
   (e.g. Chrome DevTools Protocol, headless browser, log capture).
2. **Feature map** — a user-facing sitemap of every feature ("the thingy in the left sidebar" becomes a
   shared noun). Build shared vocabulary between human and agent.
3. **Custom CLIs** — give the agent a reusable driver instead of ad-hoc scripts each time. Cheaper tokens,
   testable, consistent.
4. **Property-based UI testing** — generator-driven assertions instead of hand-written cases.
5. **Formal verification** — model checking (TLA+) for multi-device/multi-platform flows, proof
   assistants (Lean) for algorithmic invariants (e.g. no flicker/jitter in a virtualization lib).
   Not a silver bullet; one more layer.

These are not novel techniques. They were simply too expensive to write and maintain before. Agents
collapsed the cost — this is the golden age of testing.

---

## 6. Order of operations (do not skip)

1. Improve the codebase (API design, architecture, lint) — **before** any automation.
2. Add a lint rule to stop new occurrences of an anti-pattern.
3. Clean up existing occurrences of that pattern.
4. Make the verification story strong.
5. Only then build the outer loop (software factory): orchestration, cloud agents, auto-fix, auto-merge.

Automating a slop-producing loop multiplies slop.

Example of a healthy outer loop once foundations exist: on every release, spin up an isolated project,
fuzz the whole build by using the app like a real user, open a fix PR per bug found, auto-merge.
This only works because PRs are good by default.

---

## 7. Reference outcomes (Graphbot)

- ~11,000 PRs shipped to production per month by one team.
- A new stable release roughly once a day.
- ~227 people contributed at least one PR in a month, including low-context drive-by contributors.
- Revert rate *fell* and stabilized around **0.5%**.
- Startup, typing latency, chat open/switch all improved despite the velocity.
- Most product code landed no longer requires deep human review.

---

## 8. Quick checklist for any coding task

Before writing code:

- [ ] Is there an existing pattern in this repo? Copy the good one; do not invent a parallel one.
- [ ] Does this repo have the lint/type/test guardrails for the area I'm touching? If not, is adding one cheap?
- [ ] Am I about to solve this with a "temporary" workaround? Can I fix the architecture instead?

Before submitting:

- [ ] Did I add a comment to justify a shortcut? → Replace with a lint rule or fix the root cause.
- [ ] Did I widen a correction into a global rule? → Narrow the scope.
- [ ] Do I have verification artifacts (test run, screenshot, video, state chart, benchmark)?
- [ ] Can a deterministic check catch the mistake I almost made? Add it.

---

## 9. Anti-patterns to refuse

| Anti-pattern | Why it fails |
|---|---|
| "Please don't use X" in AGENTS.md | Unenforceable, decays silently |
| Leaving a broken pattern in place "for now" | Agents will replicate it as canonical |
| Comments explaining why a workaround exists | Becomes precedent for more workarounds |
| Automating before foundations exist | Multiplies slop, burns tokens |
| Reviewing diffs instead of verification artifacts | Cannot see behavior; scales to 0 at high velocity |
| Banning a tool globally after one local misuse | Over-rotation; prefer scoping and lint messages that teach |
