---
description: "The graduation build begins — with no code at all. Brian covers what makes an idea capstone-shaped, three blessed project ideas plus the build-your-own checklist, the spec process at full ceremony, and the constitution you commit before the first line exists."
---

Here's what the capstone is: **an app that's yours** — your idea, your data, your decisions — built from blank page to live URL using everything this course installed in you. And here's what this first lesson is: the part of that build where _no code gets written_, because you now know the secret that separates people who ship from people who thrash — the quality of a build is mostly decided before it starts. Spec first, plan second, constitution third, code later. Trust the sequence; it's the one you rehearsed.

## What makes an idea capstone-shaped

Not every good idea is a good _capstone_. Run your candidates through this checklist:

- **A resource you genuinely care about.** The apps that get finished are the ones whose data you actually want to look at. You may have already met yours: if the API you built in the your-own-API project still charms you — the golf rounds, the houseplants, the board-game nights — congratulations, you started your capstone weeks ago without knowing it.
- **Full CRUD on that resource.** Create, read, update, delete — the complete verb set, which you can now say in REST.
- **Accounts, with per-user data.** Better Auth, people own their stuff, and at least one thing only its owner may do. You've built this; now it's furniture.
- **One interesting twist** beyond the pet-tinder shape, chosen from a menu you understand: a join-powered view (two tables holding hands to make a screen), a public/private split (browse anonymously, act when signed in), a search or filter, or a **free public API** woven in. On that last one, a scope ruling: no keyed or paid APIs required anywhere in this capstone — free, keyless ones are welcome (you know a weather API and a pets API personally), and keyed services live on the stretch menu at the very end, where they belong. This is simplify things for you. - I'd hate for you to get stuck here - feel free to break this rule, but do so at your own risk.
- **Days, not weeks.** Milestone-sized ambition. The twist is _one_ twist.

Stuck at the blank page? Three blessed ideas: **a reading shelf** (books you've read and are reading, ratings, everyone's shelves browsable, favorites as the join, think Goodreads); **a trip planner** (trips and their stops, the join is trips-to-stops, and the twist writes itself — you know a free weather API that would love to forecast each stop, think TripIt); **a game-night scoreboard** (your group's games and plays, with a leaderboard as the join-powered view). Or bring your own through the checklist. Caring about it outranks cleverness.

## The spec, at full ceremony

You know this method; today it gets performed with the seriousness of a real kickoff. **Brain dump** everything — the idea, the features, who can do what logged out versus in, the twist, and every piece of doctrine you're about to make law. Then **prompt for your prompt**: "Don't write any code yet. Organize everything below into a build plan of small, individually verifiable milestones. Each milestone ends with something I can check and all checks passing. Show me the plan for approval before doing anything."

Then the part that makes you the editor-in-chief: **read the plan like the diff it's about to become.** Is milestone one small enough to read? Did your doctrine survive intact — Better Auth present, nothing hand-rolled? Did the agent invent features you never asked for (it loves to gift you a comments section)? Are the verification steps real — "you can sign up and log out" — rather than vibes? Push back, reorder, cut. A healthy capstone plan usually lands at five-ish milestones: skeleton serving hello, database connected with schema, auth working, the core resource CRUD, the twist. Yours may differ; smallness matters more than the count. I guarantee you should at least push back once - the agents never get it right on the first try. Yours may differ; smallness matters more than the count.

One more planning artifact, for when the moving parts multiply: a TASKS.md. Have the agent break each milestone down into its individual tasks as a checklist in a committed file — and then, this is the operative part, have the agent actually check the boxes off as it completes them. "Maintain TASKS.md: break the current milestone into tasks, and check each one off as you finish it" goes in the prompt (or the constitution). It's not always required — a five-milestone capstone with clean milestones may never need it — but when a milestone has a lot of moving parts, it does something subtle and valuable: it stops you from assuming the agent will remember to do something. Agents lose threads in long sessions the way anyone does; a written checklist externalizes the memory, and an unchecked box at milestone's end is a to-do that didn't silently evaporate — it's sitting right there, visible, in a file you can read in your repo. Plans plan the milestones; TASKS.md makes sure the milestones actually happen.

## The constitution

Before the first line of code, commit the **CLAUDE.md** — the agent instruction file you met on the codebase walk and learned to weaponize in the prevention lesson. This is where standing law lives so you never have to restate it, and yours should contain: the stack, pinned (Express + Vite React + TypeScript, Drizzle + Postgres, Better Auth for all auth — never hand-rolled — no Next.js or other meta-frameworks, no component or state libraries); the quality block from the prevention lesson, pasted whole (minimal issues-only ESLint, `{}` Prettier, Vitest with tests as features are built, E2E for core flows only and never flaky, **every milestone ends with everything green**, suite stays pruned); and your working rules (work one milestone at a time; don't change code outside the current milestone; ask before adding anything not in the plan). House rules, posted at the door, before any intern walks in.

Last piece of setup: rent your dev Postgres and get the `DATABASE_URL` into `.env`. You've done this by hand and you know why you did — so if you'd like to let the agent try it through the Render MCP this time, consider it a dress rehearsal for what's coming on ship day. Either way: repo created, pushed to GitHub, plan approved, constitution committed, database humming. That's the deliverable, and notice what it is — a project any professional would recognize as _properly started_, containing zero features and all of the decisions.

> 🤖 **Ask your AI Assistant.** The red-team, before you commit to the plan: "Here is my spec and milestone plan. Attack it: what's ambiguous enough that you'd have to guess? Which milestone is riskiest, and why? What am I likely to discover mid-build that I should decide now?" This is the adversarial read — the same skill as reviewing a diff, applied to a plan — and it's cheap insurance: every ambiguity it finds now is a mid-build derailment that never happens. Fold the good catches back into the spec, ignore the paranoid ones (it will suggest edge cases your app doesn't need — you're the editor), and _then_ approve your own plan. Tomorrow, milestone one.

## Recap

- Capstone-shaped: a resource you care about, full CRUD, accounts with ownership, exactly one twist, days not weeks, IP-clean. Your your-own-API resource may already be the answer.
- Three blessed ideas if the page is blank — reading shelf, trip planner, game-night scoreboard — or bring your own through the checklist.
- The ceremony: brain dump → prompt for your prompt → read the plan as editor-in-chief → five-ish small milestones with real verification steps.
- The constitution ships before the code: CLAUDE.md with the pinned stack, the whole quality block, and the working rules. Standing law, stated once.
- Red-team the plan, fold in the catches, approve it yourself. A properly started project contains zero features and all of the decisions.

Next: the build — the rhythm, the discipline, and what to do when it wobbles.
