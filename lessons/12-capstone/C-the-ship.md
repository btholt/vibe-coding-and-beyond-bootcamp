---
description: "Ship day. The agent provisions production itself — and you understand every step, because you once did them by hand. Brian covers the pre-flight, the three classic first-deploy failures, debugging a machine you can't see, and why the README is part of the product."
---

Today the sentence changes from "it works on my laptop" to "here's the URL." Everything between those two sentences is this lesson, and none of it is new — it's the hosting lesson, the secrets lesson, the MCP lesson, and the debugging section, performed in sequence on the thing you just built. Shipping is a skill, and like every skill in this course, it's mostly a sequence plus the calm to follow it when step three hiccups. (Step three usually hiccups. It's in the lesson.)

## Pre-flight

Three checks before anything touches production. **First, rehearse production locally** — this is the pro move that saves the most grief: `npm run build`, then `npm start`, and visit the production-mode app on your own machine. That's the _same code path Render will run_ — the built `dist`, served by Express, no Vite in sight — and any build error or serving bug is infinitely nicer to meet on your laptop than in a deploy log. **Second, green across the board** — the suite, the linter, the types; you would not believe how many deploys of broken code are preceded by "I'll just skip the checks this once." **Third, committed and pushed** — Render deploys from GitHub, so GitHub needs the goods.

## The provisioning: the sandwich closes

Now the moment this course has been arranging since you clicked New → Postgres in a browser, by hand, on purpose. Tell your agent, through the Render MCP:

> "Provision this app on Render: a web service connected to my GitHub repo (build with `npm run build`, start with `npm start`), plus a Postgres database. Set DATABASE_URL and the Better Auth environment variables on the service. Walk me through each step before you do it."

And then do the thing only you can do: **watch with comprehension.** Every tool call that asks your permission is a step you have personally performed — create the database (you clicked that), copy the connection string into the environment (you pasted that), point the service at the repo and the build command (you typed those into a form once). Read each permission prompt like the diff it is — these are _write_ tools, some of them cost money, and the golden retriever is extremely eager today — approve the right ones, and watch your infrastructure assemble itself from a paragraph. This is the whole course in one minute: delegation, with comprehension, verified by someone qualified to verify. The intern got hands; you stayed the adult.

(Cost honesty, one last time: the free web service naps between visitors, the free Postgres is a thirty-day trial, and about a coffee a month keeps both properly alive. For a capstone, free is a fine place to start — and if the deployment someday lapses, the repo is forever, which is the part that was always yours.)

## First deploy, and the three classic failures

Push the button (or let the agent trigger it) and watch the deploy log stream — the build running on Render's machine, the same output you rehearsed locally. And now, pre-named so they're recognized rather than feared, **the three ways first deploys classically fail**, which are exactly the three movable parts of any deployment — code, secrets, data:

**The missing env var** (secrets didn't make the trip): the app crashes on boot, and the log says so — something is `undefined` that should be a connection string or an auth secret. Fix: set it in the service's environment, redeploy. **The unrun migration** (data didn't make the trip): the app boots, then the first database query dies with _"relation does not exist"_ — your production Postgres is brand new and has never heard of your tables. Fix: run your migrations against the production `DATABASE_URL` (the agent knows how; make sure _you_ know it happened). **The hardcoded localhost** (code assumed home): everything deploys, and fetches fail weirdly for anyone who isn't you — because a `localhost:3001` buried in frontend code points at _the visitor's_ machine, exactly as the hosting lesson warned. Fix: relative URLs (`/api/...`), which is what the one-roof architecture wanted all along. And a fourth non-failure to file: the free tier's first visit after a nap takes half a minute. That's not broken; that's the price tag.

When something else goes wrong — and sometimes it does — the frame is the inventory: _which of the three parts didn't arrive?_ Then the debugging section takes over, with one upgrade:

## Debugging a machine you can't see

Production is the first computer you've built on that you can't watch directly — no terminal of your own, no dev tools on someone else's phone. But you gave your agent eyes for exactly this: **"Read the recent logs from my Render service and tell me what happened around the time of the crash."** The logs are the terminal you can't see — and scrolling through them is your old friend the request logger, faithfully narrating production traffic, method-path-status, exactly as it did on localhost since the codebase walk. Reproduce against the live URL, pull the logs through MCP, write the gap report, proceed as trained. Debugging didn't change; only the distance did.

## Verify like it's real — because it is

The lap, on the live URL, as a stranger would: sign up (a real account, on your real app, in a real database you're renting), create, edit, favorite, search — the twist, traced live. Run the two-account test one final time _in production_. Then the test that no checklist captures: **open it on your phone.** Not the dev tools device emulator — your actual phone, on cellular, standing in your kitchen. There's a moment there, and you've earned it. Then send the URL to someone you love, which is both the true E2E test and the entire point of shipping.

## Docs are part of shipping

Last mile, and it's not optional garnish: **the README is part of the product.** You've spent a whole course learning that a README is a repo's front door — now build a good one for yours: what this app is, in two sentences a stranger understands; the live URL, right at the top; a screenshot (front doors have windows); and the local setup ritual, accurate — including a truthful `.env.example` — so that future-you, or anyone else, can stand it up from a fresh clone. Have the agent draft it; edit it for truth yourself, because you're the only one who knows which promises the repo actually keeps. A deployed app with a clean README and a groomed backlog of issues is not a student project. It's a _maintained piece of software_, and it has a maintainer, and the maintainer is you.

> 🤖 **Ask your AI Assistant.** The post-deploy audit, through the tools: "Using your Render tools: what's the status of my service and database, what do the last fifty log lines show, and is anything unhappy? Then list every environment variable set on the service — is anything missing that the code reads, or set that nothing uses?" That last question is the secrets lesson's audit, performed against _production_ — the place it always mattered most — and a clean answer is the real finish line of ship day: running, observed, and holding exactly the secrets it should.

## Recap

- Pre-flight: rehearse production locally (`npm run build && npm start` — Render's exact code path), everything green, everything pushed.
- The sandwich closes: the agent provisions the service and database through the Render MCP while you watch with comprehension — every permission prompt a step you've done by hand. Delegation, verified by someone qualified.
- The three classic first-deploy failures are the three movable parts: missing env var (secrets), unrun migration (data), hardcoded localhost (code). Plus the nap, which is a price, not a bug.
- Production debugging is the same loop at a distance: logs through MCP are the terminal you can't see, and your request logger has been rehearsing for this since the walk repo.
- Verify live — two accounts, the twist, your actual phone — then ship the docs: URL up top, screenshot, truthful setup. A README, a backlog, and a maintainer make it software, not homework.

Next: the victory lap — where this app goes from here, and where you do.
