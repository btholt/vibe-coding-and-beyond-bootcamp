---
title: "Project Stranger's Code"
description: "The museum becomes a workshop: a ten-question scavenger hunt through the recipe box (no assistant allowed), then a real feature added to code you didn't write — forked, branched, PR'd, and reviewed like you own it. Because now you do."
---

Time to do the actual job. Not a metaphor for the job, not a warm-up shaped like it — the thing itself: **take a codebase you didn't write, understand it well enough to answer hard questions about it, and then change it without breaking it.** That's what a developer inherits on day one at a new company, it's what you inherit every time an agent hands you a project, and it's what this course has been building you toward since the first `console.log`. Two parts: a reading test, then a change.

## Setup: fork it

One new GitHub word first, and it's a small one. You can't push branches to _my_ repo (thank goodness — nobody should be able to push to repos they don't own), so GitHub has a button for this exact situation: **Fork**, top-right of the [recipe box repo][repo]. A fork is your own full copy of someone else's repo, living under _your_ GitHub account, where you can push, branch, and PR to your heart's content — it's the primitive underneath all of open source, and now you have one. Clone _your fork_, and if you didn't already bring it to life in the last lesson, run the ritual (or delegate it): fresh Render Postgres, connection string to the agent, "get this running."

## Part one: the scavenger hunt

Ten questions, and here are the rules that make it worth doing: **answers go in your notes, with the file (and roughly where in it) that proves each one — and your assistant starts on the bench.** Give each question a genuine unassisted shot first, because this is the one place in the back half of the course where the reading is the whole point, and every answer is findable with the walk order and the habits you own. But if you get genuinely stuck — stuck-stuck, not thirty-seconds-uncomfortable — bring the assistant off the bench without guilt. I'm not your mom, and this is about learning, not scorekeeping: if talking it through is how you'll actually learn, talk it through. Just change _what you ask for_ — don't ask for the answer; ask it to **fully explain the question**: "Walk me through what question 6 is really asking, what concepts are involved, and where in a codebase like this I'd even start looking." I do not care whether you get the answer; I care whether the thing behind the question makes sense to you when you're done. An answer copied teaches nothing; a question understood teaches even if you never write the answer down. Budget maybe half an hour; enjoying it is permitted.

1. Which file decides what port the API server runs on, and what's the default?
2. What stops a user from favoriting the same recipe twice — and is the enforcement in the code or in the database itself?
3. What exact status code do you get for trying to edit someone else's recipe, and which lines produce it?
4. When you search recipes, what makes the match case-insensitive?
5. Why must Better Auth's routes be mounted _before_ `express.json()`? (The code will tell you.)
6. A visitor refreshes their browser while on `/recipes/3` in production — which lines of server code answer that request, and with what?
7. Which component decides whether a heart shows filled or empty, and which API route feeds it that information?
8. If a user's account is deleted, what happens to the recipes they authored — and where is that decided?
9. In development, the frontend runs on port 5173 but its fetches go to `/api/...` — what forwards those to the Express server on 3001?
10. Where would you look to learn what this app was _asked_ to be?

When you're done, _then_ bring the assistant back and have it grade you — "here are my answers with citations; check them against the code" — which is a better use of it than asking it the questions would have been.

## Part two: change a stranger's code

Now the feature. Pick one — each is deliberately small, and each exercises a different vertical slice:

**The recommended default: a cook-time filter.** "Show me recipes under 30 minutes" — a `maxMinutes` query parameter on the list route, a `where` condition alongside the existing search, and a small input in the UI. It's the search feature's fraternal twin, which means the codebase already _shows you the pattern to follow_ — that's not a training-wheels observation, that's how professionals scope work in unfamiliar code: find the nearest existing example of the thing you want.

**The mirror: a "My Recipes" page.** Everything you cooked, one place. Simpler query than favorites (no join needed — why not? if you can answer that, you've internalized the schema), a new route, a new page, a nav link.

**The ambitious one: a servings field.** How many people does this recipe feed? This one touches the _schema_ — which means you'll watch the agent generate a real **migration**, run it against your database, and thread the new field through the form, the routes, and the display. The full vertical, including the one layer no other option touches. Pick this if the word "migration" stopped being scary and started being interesting.

The method is everything you've rehearsed, now on foreign soil: **brain dump → prompt-for-your-prompt → milestones** (a feature this size is likely two), and — the payoff of the GitHub lesson — **the work happens on a branch, delivered as a pull request to your fork.** Review it in the PR UI like it arrived from a cloud agent, because that's precisely the muscle: Files changed, every green and red line, comments on anything worth remarking. Merge it, pull it, use it.

## The new reading duty: does it belong?

Everything else in the reading duties you know — trace the new feature end to end, check the who-may-ask column on any new route — but changing a stranger's code introduces one genuinely new review skill, and it's a senior one: **consistency.** The question isn't only "does this code work?" but "does this code look like it _belongs here_?" This codebase has a house style: fetches centralized in `src/api.ts`, guard clauses at the top of handlers, section-banner comments, errors as `{ error: "..." }` JSON. Did the agent's new code follow the house patterns, or did it drop a fetch directly into a component and invent a new error shape? Inconsistency isn't cosmetic — it's how codebases rot, one each-diff-does-its-own-thing at a time, and agents _will_ drift from house style unless the spec or the reviewer holds the line. You now hold the line.

> 🤖 **Ask your AI Assistant.** The review, at its final form: with your feature's PR open, ask "Review this pull request as if you were the original author of this codebase. Does the new code follow the existing patterns and conventions? Point to specific deviations." Then make it fix any real ones — on the same branch, watching the PR update. You've just run a two-agent workflow: one wrote the code, one reviewed it in character, and _you_ adjudicated — which, minus the ceremony, is a plausible description of how a lot of software gets built now. The adjudicator is the load-bearing role. It's yours.

Merge, pull, commit your notes, and take stock of what this section made true: you can be handed a repository you've never seen, map it, answer pointed questions about it, run it, and change it — safely, reviewably, in its own style. There's a name for someone who can do that to a codebase, and the name doesn't care how the code gets typed.

Next: things start breaking. On purpose, and then — inevitably — not.

[repo]: https://github.com/btholt/family-recipe-box-sample-app
