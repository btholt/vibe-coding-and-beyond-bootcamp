---
description: "Nothing new to learn — everything to apply. Brian coaches the build: the per-milestone rhythm, what to do with mid-build ideas (file them, don't chase them), the wobble protocols for when it goes sideways, and the mid-build audit that keeps a codebase honest."
---

This lesson is a coaching session, not a class — there is nothing left to teach you about building this app, only things worth saying while you do it. Keep it open in a tab; I'll be brief, because you have work to do.

## The rhythm

Every milestone, the same four beats. The rhythm _is_ the discipline — protect it especially when you're excited, because excited is when it slips.

**Open with the blast radius.** "We're on milestone two. Do only milestone two — don't change anything else. Here's the plan again for reference." The constitution's working rules say this too, but saying it per-prompt keeps the intern honest. While the agent works, watch the terminal — you read that dialect now, and drift is visible early to anyone watching.

**Close with the reading.** The standing law doesn't bend for your own project — _especially_ not for your own project: every line, every diff, before anything else happens. Then the milestone's verification step, exactly as the plan wrote it — and for anything with a UI, this is where the Playwright MCP earns its keep: "open the app in the browser and verify this milestone yourself — sign up, click through it, tell me what you saw." Make the agent check its work the way you would; read its report skeptically anyway.

**Green means green.** The milestone is not finished until the linter, the type checker, and every test pass — your constitution says so, which means the agent already knows, which means your job is just refusing to move on until it's true. No "I'll fix the tests in the next milestone." That sentence is how suites die.

**Commit and push.** Every milestone, minimum. And if you want the full professional ceremony — I'd encourage it, gently — do each milestone on a branch and PR it to yourself: reviewing your own milestone in the Files-changed tab is genuinely better reading than the editor gives you, the merge is a satisfying little ritual, and the resulting history reads like a story. But ceremony is optional; the commit is not.

## The ideas will come — file them

Somewhere around milestone three, mid-flow, it will happen: _"oh — what if it also had..."_ A genuinely good idea, arriving at the worst possible moment. Here's the entire discipline in one move: **open a GitHub Issue on your own repo and write the idea down.** Thirty seconds, then back to the milestone. You're building a **backlog** — yes, the same artifact you burned down last section, now seen from the other side: every product's backlog is just this, good ideas filed instead of chased. Ideas are cheap when captured and expensive when they mutate a milestone; the plan is the plan, the backlog is the parking lot, and when you ship, you'll own a groomed list of what's next — which is a professional asset, not a pile of regrets.

(A plan _changing_ is different from a plan _drifting_, and the difference is intent. If mid-build you discover the schema genuinely needs another column, that's not scope creep — that's learning. Update the plan deliberately, note it, migrate, proceed. Changing the plan on purpose is engineering; the plan changing by accident is drift.)

## When it wobbles

It will, somewhere. The protocols, indexed by wobble:

**A bug appears.** You have a whole section for this: reproduce, triage, gap report — and if it's a real bug in finished work, the ritual applies: red test, fix, green, tripwire on the grave.

**The agent goes sideways** — a mega-diff, changes sprawling past the milestone, a "refactor" nobody ordered. Don't ride it down. Reject the changes, and this is precisely why every milestone ended in a commit: your worst case is always _reset to the last save point and re-prompt with tighter constraints_. Git makes you fearless; use the fearlessness.

**You don't understand what it built.** Stop. Explain-mode: "walk me through what this does and why." The law has a contrapositive — never merge what you couldn't review — and honoring it costs ten minutes while violating it costs your grip on the codebase, which is the only thing this course actually gave you.

**You're slower than the plan.** You're not behind; everything takes longer than its plan, everywhere, always, and the milestones don't expire. The rhythm doesn't care what day it is.

## Spot-checks, milestone by milestone

Brief, because you know all of this: at the _skeleton_, confirm green-from-day-one (the tooling runs and passes before there's anything to check — cheapest it will ever be). At the _schema_, read it like the highest-value file it is; those are your nouns, get them right early because migrations are easier than regrets. At _auth_, run the two-account test immediately — don't wait for the CRUD to discover ownership is broken. At the _core resource_, Bruno the API before trusting the UI, and try the 403 on purpose. And at the _twist_ — the milestone that makes it yours — when it works, run the full trace end to end and write it down, because tracing your own twist through your own stack is this course's final exam, self-administered.

> 🤖 **Ask your AI Assistant.** The mid-build audit, somewhere past halfway: "Review this repo against my CLAUDE.md and my milestone plan. Where is the code drifting from the house rules? What's accumulating — untested logic, inconsistent patterns, TODOs — that will bite later?" This is the consistency review from the stranger's-code project, turned on your own house while there's still time to act on it. Fix the real findings as a small tidy-up commit, file the debatable ones to the backlog, and notice the shape of what you just did: a code review, of an agent's work, against written standards, on a codebase you fully understand. That's not a student exercise. That's the job, and you just did it unprompted.

## Recap

- The rhythm, every milestone: blast-radius prompt → read every line → verify (Playwright MCP for UI) → green means green → commit, push. Branch-and-PR ceremony encouraged, commit non-negotiable.
- Mid-build ideas get filed to your own backlog in thirty seconds, not chased. Deliberate plan changes are engineering; accidental ones are drift.
- Wobble protocols: bugs get the ritual; sideways agents get reset-and-re-constrain (git makes you fearless); code you don't understand gets explained before it gets merged; slow is normal and milestones don't expire.
- Spot-checks: green from the skeleton, nouns right at the schema, two accounts at auth, Bruno before UI at the CRUD, full trace at the twist.
- The mid-build audit is a real code review against written standards — the job itself, performed on your own house.

Next: ship day. The agent gets the keys it's been training for.
