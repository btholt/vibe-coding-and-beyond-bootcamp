---
description: "Debugging is the other half of building, and it runs on a loop you can learn. Brian covers what a bug actually is, the reproduce-first doctrine, the two questions that triage everything — and formally assembles the famous-errors catalog you've been collecting all course."
---

Things break. Not sometimes — reliably, constantly, as a structural feature of building software, and _especially_ of building software with an enthusiastic intern who types faster than anyone can think. If the first half of this course made you a reader and the middle made you a builder, this section makes you the thing that actually separates people who ship from people who quit: **someone who stays calm and effective when it's broken.** The good news is enormous: debugging is not a talent. It's a loop, the loop is learnable, and you have — without quite being told — already collected most of its equipment.

Start with what a bug actually _is_, because the definition does real work: **a bug is a gap between what you meant and what you wrote.** The computer is never wrong — it executes exactly what the code says, one line at a time, with perfect obedience; you've known that since the first JavaScript lesson. So when the app misbehaves, the misbehavior is _information_: somewhere, the code says something different from the intention, and debugging is the hunt for that somewhere. This is also why debugging generated code is philosophically identical to debugging your own — the gap is just between what you _asked for_ and what got written, and the hunt is the same hunt.

## The loop

Every debugging session, from a typo to a production outage, is the same scientific loop:

**Observe, precisely.** Not "it's broken" — _what exactly happens, versus what exactly should happen?_ "Clicking the heart does nothing, and I expected it to fill" is a debuggable observation. Vague symptoms produce vague hunts.

**Reproduce, before anything else.** Make the bug happen _on demand_ — this is the non-negotiable first move, and here's the cold logic: if you can't make it happen reliably, you cannot know whether anything you try actually fixed it. A fix without a reproduction is a superstition. Find the steps — click this, type that, boom — and write them down; they're about to be your test rig, and (foreshadowing a coming lesson) they may literally become a test.

**Hypothesize, then test the smallest possible thing.** Not "maybe the whole favorites system is wrong" — "maybe the recipe id isn't reaching the server." Small hypotheses have small tests: one `console.log`, one glance at the Network tab, one row checked in the database.

**Narrow like a binary search.** Every trace you've ever done — click → handler → fetch → middleware → query → response → render — is now a _search space_, and the professional move is to probe the _middle_ of it: is the data correct at the halfway point? Yes → the bug lives downstream; no → upstream. Each probe cuts the territory in half, which is why pros find bugs in long chains fast — not sharper eyes, better halving.

## Triage question one: which world?

Before probing anything, sort the bug into the map you already own — browser, build step, server, database — because each world has its own instruments, and knowing which toolbox to grab is half the diagnosis. Browser-world: the console and the Network tab. Build-world: the terminal where the build ran (these bugs announce themselves — the proofreader refuses to ship). Server-world: the terminal logs, your request logger's method-path-status lines. Database-world: the Postgres extension, where you look at what's _actually stored_ instead of what you assume is.

And the boundaries between worlds are themselves checkable, which is the real power of the map: the Network tab audits the browser↔server boundary (did the request leave correctly? did the response return correctly?); the request logger audits arrival on the server; the database extension audits what the queries truly did. "Is it the server's side or my side?" — the question the Network tab taught you — generalizes into the master triage: _walk the boundaries until you find the one where good data goes in and wrong data comes out._ The bug lives between those two checkpoints.

## Triage question two: loud or silent?

**Loud bugs come with an error message, and an error message is a gift.** You've known this since you broke a `const` on purpose: errors are specific complaints, not insults, and by now you have the full protocol — read the complaint, walk the trail, find the topmost line that's yours. Loud bugs are the easy mode of debugging, and the only skill they truly demand is the one you've practiced all course: _actually reading the message_ instead of flinching past it.

**Silent bugs are the hard mode: nothing crashes, something is simply wrong.** The favorites page shows the wrong recipes; the total is off by one; the button does nothing at all. No complaint, no trail — so you _build_ the trail: instrument the path with flashlights (`console.log` at each step of the trace, the Network tab open, the logger scrolling) and compare expectation to reality at every checkpoint until you find the exact step where they diverge. The divergence point is the bug's home address. You've met the famous silent genres already: the lookalike character that matches nothing, the silent `catch` where errors go to hide, the logic that runs perfectly and computes the wrong thing.

Which points at a design philosophy worth adopting, not just for debugging but for building: **fail loud, and fail fast.** Loud, because an error is a gift and silence is a search party; fast, because an error at the moment of the mistake points straight at its cause, while a wrong value discovered three steps downstream points at nothing. This is why the silent `catch` is a sin — it takes a loud failure and _converts it_ to a silent one — and it's why, when you're specifying behavior to an agent, "if something's wrong, throw an error immediately" is better instruction than "handle errors gracefully" for anything still under construction. And it names the worst genre of all: the **intermittent bug** — the weird edge case that only surfaces occasionally, works fine when you're watching, and fails on Tuesdays. Intermittent bugs are hard mode's hard mode for a precise reason: reproduction is the non-negotiable first step, and for these, _reproduction itself is the battle._ When you meet one, the entire hunt is "find the conditions that make it reliable" — and when you eventually build things, every fail-loud, fail-fast choice you make is one fewer of these lying in wait.

## The catalog, assembled

Now the retrospective moment this section owes you. Since the values lesson, this course has been handing you "famous errors" one at a time — and you've been building a bestiary without ever being told that's what it was. Here it is, on one page, each with its one-line translation:

- **`Assignment to constant variable`** — you tried to refill a `const` box. The very first one you caused on purpose.
- **`Cannot read properties of undefined`** — a link mid-chain came up empty: typo'd key, data not arrived yet, a box that was never filled. The most common crash in JavaScript, and you know where it's born.
- **`X is not defined`** — wrong room (scope) or a typo in the name.
- **`Type 'X' is not assignable to type 'Y'`** — wrong kind of thing, labeled box, caught _before_ it ran. The friendliest timing an error can have.
- **`Each child in a list should have a unique "key" prop`** — React wants name tags on list items. A warning, not a crash; the gentlest member.
- **Stack traces** (`ENOENT` and friends) — complaint on top, trail below, find the topmost line that's yours.
- **`EADDRINUSE`** — someone's already living in that apartment; it's almost always your own forgotten server. Ctrl+C the other terminal.
- **`req.body` is `undefined`** — the unpacker (`express.json()`) is missing or mounted in the wrong order. A famously confusing first-server bug.
- **401 vs 403** — "who even are you?" versus "I know exactly who you are, and no." And the whole status-code family system: 4xx the asker's problem, 5xx the server's own fault.
- **Merge conflict markers** — two timelines touched the same lines; git wants a human to pick. Scary name, mundane cause.
- **The silent catch** — not an error but an error's _hiding place_; when something fails with no message anywhere, go see what the catch is swallowing.

And one new admission, because you're due to meet it and it deserves a proper introduction rather than an ambush: the **CORS error** — a wall of red console text containing the phrase "blocked by CORS policy." It's the padded room doing its job: browsers restrict which _other_ sites a page's JavaScript may call, and a server has to explicitly say "browsers may call me from anywhere" in its response headers (my pets API says exactly that, which is why you've never hit this). When you see it: the request was blocked by _policy_, not by a bug in your logic — usually the server didn't send the allow headers, or you're calling the wrong URL entirely. It's also why the recipe box's dev setup proxies `/api` through Vite instead of fetching across ports: same-origin requests never trigger the policy at all. File it in the catalog: dramatic appearance, boring cause, and now it can't ambush you.

Read the assembled catalog once more and notice the pattern that makes it powerful: **every entry is "scary name, mundane cause, known move."** That's not a coincidence of these particular errors — it's what _all_ errors turn out to be once someone translates them, and the catalog will keep growing that way for the rest of your building life.

> 🤖 **Ask your AI Assistant.** Sabotage roulette — the loop, practiced with a perfect answer key. Open any working project of yours (pet tinder is ideal), **commit** (the net, always the net), and then: "Introduce one small, subtle bug somewhere in this app. Don't tell me what or where. Make it realistic — the kind of mistake that actually happens." Then debug it for real: observe precisely, reproduce, sort it — which world? loud or silent? — probe the middle of the trace, narrow. When you've found it (or when you're ready to concede), the Source Control panel holds the answer key: the diff shows _exactly_ what changed. Grade your hunt, note which triage question did the most work, and run it again on a different project if it was fun — and it's usually fun, which is the best-kept secret about debugging.

## Recap

- A bug is a gap between what you meant and what you wrote. The computer is never wrong; the misbehavior is information pointing at the gap.
- The loop: observe precisely → reproduce on demand (non-negotiable — a fix without a reproduction is a superstition) → small hypothesis, smallest test → narrow by halving the trace.
- Triage one — which world? Browser, build, server, database, each with its instruments; walk the boundaries until good data goes in and wrong data comes out.
- Triage two — loud or silent? Loud comes with a gift: read the complaint. Silent means you build the trail yourself and find where expectation and reality diverge.
- The famous-errors catalog is assembled, CORS newly admitted, and every entry obeys the same law: scary name, mundane cause, known move.

Next: debugging as a two-player game — what to hand your agent, when to let it drive, and how to tell a real fix from a bandage.
