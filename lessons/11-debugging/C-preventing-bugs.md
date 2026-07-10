---
description: "The best debugging session is the one that never happens. Brian covers the proofreader family — Prettier, ESLint, and your old friend tsc — plus unit tests as tripwires, end-to-end tests for the flows that matter, and the Playwright MCP that lets your agent verify its own work in a real browser."
---

At the end of the last lesson I left you with a tedium: after every fix, a lap around the neighboring features to make sure nothing else quietly broke. You know this course's oldest pattern — feel the tedium, then meet the tool — and this whole lesson is the tool, plural. Prevention is a family of machines that check your code _before_ a human or, worse, a user has to. And the through-line for this course specifically, the reason prevention matters double in the agent era: **every checker is a feedback loop your agent self-corrects against.** You saw this dynamic once already — the TypeScript lesson's tireless proofreader, flagging mistakes the instant they're typed so the agent fixes them before you ever see them. This lesson is that idea, generalized: the more machines checking the work, the more the agent catches its own mistakes, and the less of the catching falls to you.

## The proofreader family

Three tools, nearly universal, and your generated projects likely already carry some of them.

**tsc**, the TypeScript checker, you know: labels on boxes, wrong-kind-of-thing caught before it ran, can refuse to build. Positioned in the family: tsc checks _kinds_ — is this the right sort of value?

**Prettier** checks nothing at all — it just _makes decisions_. Prettier is a formatter: it takes every argument humans have ever had about code style — tabs or spaces, where the braces go, how long a line may be — and settles all of them, permanently, by rewriting your files into one canonical style on every save. It is deliberately near-unconfigurable, and that's the entire point: **the goal is to stop having opinions about formatting so nobody ever spends attention on it again.** For you, the reader, there's a second gift: consistent formatting means your diffs contain only _meaning_ — no noise where someone's editor re-indented a file — and code that all looks the same is code where the _unusual_ jumps out, which is half of consistency review done for free.

**ESLint** is the interesting one: a _suspicious-pattern detector_. Where tsc checks kinds, ESLint checks habits — it reads your code looking for shapes that are legal but bug-scented. A variable declared and never used (was it supposed to do something?). A `==` where house style says `===`. A missing `await` on something async — which you can now appreciate as a genuinely evil bug: the code runs, nothing errors, and the result just... isn't there yet, a perfectly silent failure that a linter catches in the editor before it ever runs. (And a callback for the React-curious: those hooks-ordering rules I mentioned agents never violate? Part of the reason is an ESLint rule yelling at anyone who tries.) ESLint findings appear as squiggles in VS Code, run as an npm script, and — the through-line — get read and obeyed by agents, which is how a linted codebase trains its own intern.

An honest admission before we move on, because if you're feeling like these three blur together, you're not wrong and you're not alone: **their capabilities genuinely overlap.** ESLint can flag some things tsc also catches; ESLint historically had formatting rules that fight with Prettier; there are unused-variable checks in more than one of them. People get confused about where one ends and the next begins because the borders really are fuzzy — but the _jobs_ are distinct, and holding the jobs is enough: **tsc guards the kinds, ESLint smells the habits, Prettier ends the formatting conversation.** When two of them squabble over the same territory (it happens), the standard configs agents generate have already brokered the peace, and "ESLint and Prettier are fighting about semicolons" is a solved problem your assistant fixes in one prompt. Know the three jobs; let the tooling sort out the borders.

> The cloud checkpoint, decoded while we're here: when you push or open a PR on GitHub, you'll often see a green checkmark or a red ✗ appear next to your commit. That's **CI** — continuous integration — which is simply this lesson's checkers (plus tests, coming right up) running automatically on a neutral machine, on every push, forever. A `.github/workflows` folder in a repo is the tell. When an agent says "CI is failing," it means one of these machines objected in the cloud, and the fix is the same as when it objects locally: read the complaint.

## Unit tests: tripwires

The proofreader family checks the code's _form_. Tests check its _behavior_ — and the concept is beautifully literal: **a test is code that runs your code and checks the answers.**

```javascript
import { scaleIngredient } from "./scale";

test("doubles an ingredient amount for double the servings", () => {
  expect(scaleIngredient(2, { amount: 1.5 })).toBe(3);
});
```

Read it — you can, entirely: call the function with known inputs, `expect` the result `toBe` the known-correct answer. English in function clothes. Run `npm test` and every one of these executes; any answer that's wrong fails loudly, with the expected-versus-actual gap printed — the very gap you learned to describe, generated by a machine.

Why this matters for _you_, specifically, is the tripwire framing: as a codebase grows and an agent keeps changing it, tests are what make change **safe**. That lap around the neighbors after every fix? A test suite _is_ that lap, automated, run in seconds, every time — a field of tripwires around everything that's ever worked, so that when a change breaks something three files away, the tripwire fires _now_, loudly, at the moment of the mistake. Fail loud, fail fast, industrialized. And the labor economics are on your side: **agents write good tests, cheaply** — the professional move is simply to ask for them with the feature ("...and add unit tests for the scaling logic"), and a spec line like "all existing tests must still pass" turns your whole suite into a constraint the agent works within.

Calibration, so you don't gold-plate: tests are code too, with maintenance costs, and not every line deserves one. The priorities are logic with _right answers_ (calculations, filters, transformations — anything where correct is checkable) and anything that's bitten you before. Which foreshadows the bug backlog ahead: the professional bug-fix ritual is _write a test that reproduces the bug and fails, then fix until it passes_ — red, then green — so every bug you ever fix leaves a permanent tripwire on its grave.

## End-to-end tests, and the agent gets browser hands

Unit tests check pieces in isolation; nothing so far checks that the _whole_ thing works the way a user experiences it. That's **end-to-end (E2E) testing**: a tool — **Playwright** is the standard — drives a _real browser_ like a real user: opens the page, types into the sign-up form, clicks the button, and asserts that "Welcome, Reid" actually appears. It tests the entire stack at once, browser to database and back, which makes it both the highest-confidence test there is and the most expensive to run and maintain — so the calibration is stark: **reserve E2E for mission-critical flows.** Sign up, log in, the money path, the one thing your app exists to do — the flows where breakage would page somebody. A handful of these, not hundreds.

And here's the part that makes this lesson current rather than traditional: **the Playwright MCP.** Same idea as your GitHub and Render servers — a tool plugged into your assistant — except this one gives the agent _hands on a browser_. Two workflows fall out, and both change how you build. First, self-verification: "build the feature, **then open the app in the browser and verify it actually works** — sign up, click through it, tell me what you saw." The agent stops declaring victory from the code alone and starts checking its work the way you would, which catches the entire genre of looks-right-doesn't-work before you ever load the page. Second, test generation from plain description: "write a Playwright test: a user can sign up, post a recipe, and see it on the list" — and a permanent, runnable, mission-critical tripwire comes back, authored from a sentence. Testing, as conversation.

## How to prompt for all of this

Knowing the tools is half; the other half is what you actually say to the agent, and here I'll just hand you the opinions, earned the long way. These are the defaults I'd give anyone:

**For ESLint: minimal, issues-only.** Ask for "a minimalistic ESLint config that checks for real problems — unused variables, likely bugs — and not coding-style opinions." The linter's job in your projects is smelling bugs, not enforcing someone's aesthetic preferences; style opinions in lint configs are where the tool-squabbling lives, and you've got Prettier for the only style question that matters.

**For Prettier: the empty config.** Literally `{}` — a `.prettierrc` containing nothing. That's not laziness; it's the philosophy taken seriously: accept every default, have zero opinions, never think about it again. The empty object is the whole point of the tool, written down.

**For unit tests: Vitest, and test as you build.** Vitest is the test runner that fits this stack (it's Vite's sibling — same family as your build tool, zero config friction). The instruction that matters is in the spec, not the setup: _"as you build each feature, add unit tests covering the common use cases and the edge cases."_ Tests written alongside the feature, by the agent that just built it, while the intent is fresh — not bolted on later as a chore.

**For E2E: few, solid, and core-only.** "Only write end-to-end tests for flows core to the product functioning — logging in, the central business logic. Do not write flaky tests; when in doubt, leave it out." I mean that last clause literally, as policy: **fewer E2E tests that are rock-solid beat more that are flaky or test the unimportant.** A flaky test — one that fails sometimes for no real reason — is worse than no test, because it trains everyone (you _and_ the agent) to ignore red, and ignoring red is how real failures sail through.

**Tie it to the milestones.** The rule that makes all of the above operational: _"run the linter, the type checker, and all tests at the end of every milestone — the milestone is not finished until everything passes."_ This slots straight into the plan-approval workflow you already run: every milestone's definition of done now includes green.

**And keep the garden weeded.** "Add new unit tests for any new functionality, and remove any unit or E2E tests that no longer apply." Tests are code; stale tests that check deleted features are noise with a maintenance bill, and a suite you trust is a suite that's been pruned.

Put together, that's a paste-able quality block for every spec you write from here on — the capstone very much included:

> Use Vitest for unit tests; as you build each feature, add unit tests covering common use cases and edge cases. Add a minimal ESLint config that checks for real issues (unused variables, likely bugs), not style opinions. Add Prettier with an empty config. Only write E2E tests for flows core to the product working (e.g., logging in, the core business logic) — no flaky tests; when in doubt, leave it out. At the end of every milestone, run the linter, type checker, and all tests — the milestone isn't finished until everything passes. Add tests for new functionality; remove tests that no longer apply.

And the graduation move, cashing something you met on the codebase walk: rules this standing don't belong in every individual prompt — they belong in the **agent instruction file** (`CLAUDE.md` and its cousins). Put the quality block there once, and every session in that project inherits it automatically — house rules, posted at the door, read by every intern who ever walks in.

> 🤖 **Ask your AI Assistant.** Prevention, felt — three beats on the React pet tinder. First, install the family: "Add Prettier, ESLint, and TypeScript checking to this project, fix everything they flag, and explain each thing you fixed and why the tool cared." Read the explanations — the flags are a tour of your codebase's near-bugs, and there are always a few. Second, one tripwire: "Add a unit test for [the app's most logic-y function — the like counter, a filter, whatever yours has]." Read it; confirm you can. Third, the moment that makes tests real: **break the function on purpose** — change one character in its logic — and run `npm test`. Watch the tripwire fire: red, loud, immediate, expected-versus-actual printed like a bug report written by a robot. Fix it back, watch green return, and understand in your hands what a suite of these means: a codebase that _tells you_ when something's wrong, the instant it's wrong. That's the whole lesson, felt in ninety seconds.

## Recap

- Prevention is machines checking code before humans do, and every checker doubles as a feedback loop the agent self-corrects against — the TypeScript dynamic, generalized. More machines, more self-catching, less falling to you.
- The family: tsc checks kinds; Prettier deletes formatting as a topic (and cleans your diffs down to pure meaning); ESLint smells bug-shaped patterns — unused variables, missing awaits — before they run.
- CI is these same machines running in the cloud on every push; green check, red ✗, `.github/workflows` is the tell, and "CI is failing" means read the complaint.
- A test is code that runs your code and checks the answers. A suite is the lap-around-the-neighbors, automated: tripwires that make change safe as agents keep changing things. Ask for tests with every feature; test logic with right answers, not every line.
- E2E drives a real browser through mission-critical flows — a precious few of them. The Playwright MCP gives your agent browser hands: self-verifying its own features, and generating permanent tests from plain sentences.
- The house defaults, paste-able into every spec: minimal issues-only ESLint, empty-object Prettier, Vitest with tests written as features are built (common cases and edges), E2E for core flows only — no flaky tests, when in doubt leave it out — every milestone ends green, and the suite stays pruned. Post it once in the agent instruction file and every session inherits it.

Next: you inherit a bug backlog. A repo full of other people's bugs, tickets and all — and every fix leaves a tripwire behind.
