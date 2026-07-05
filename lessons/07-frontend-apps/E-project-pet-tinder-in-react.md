---
description: "The pet app rides again — same app, third form, React all the way down. A prompted rebuild with the training wheels off: TypeScript, Tailwind, and a build step welcome, because now you can read all of it."
---

Same app, third time, and this time it's a milestone worth pausing on. Every prompted project so far carried the same anti-goal: _plain HTML, CSS, and JavaScript — no frameworks, no build step._ That constraint was never aesthetic; it was training wheels, keeping the agent's output inside the dialects you could read. As of this section, you read TypeScript, JSX, Tailwind, and the anatomy of a built project. **The training wheels come off.** You're going to prompt for the pet app in full modern dress — React, Vite, the works — and then do what you're becoming genuinely good at: read what comes back.

The reason it's the _same_ app matters. You hand-wrote this app's logic. You upgraded it to a live API and traced the fetch. You know exactly what it does down to the individual click, which means every difference you see in the React version is _pure React_ — not new features, not new behavior, just the same machine rebuilt in the new architecture. There's no better way to make a paradigm legible than holding it next to something you built with your own hands.

## The spec

Spec first, as always. This one's mostly a port, so much of it is "what my current app does" — but written down, because "make it do what the old one did" is a vibe, not a spec:

1. **What it is:** the pet-rating app, rebuilt in React. One pet at a time from the [Pets API][petsapi] — photo, name, breed, age. Like and Pass advance to the next pet; when the pets run out, show the results: how many liked, and their names. Include your design decision from before (per-swipe fetching or a batch up front) — or take the rebuild as a chance to switch.
2. **The stack, stated:** React, scaffolded with Vite. TypeScript and Tailwind are welcome — say so explicitly, or say nothing and watch the agent reach for them anyway.
3. **The new anti-goals** — because the old constraint's job (keeping output readable) still needs doing, just at the new level: **one page, no routing library, no state-management library, no component libraries.** The agent doesn't need Redux for two buttons and a counter, and telling it so keeps the output honest. Scope constraints are the new training wheels — lighter, but still yours to set.
4. **Component shape, optional but recommended:** if you did the architecture exercise from the React lesson — where your assistant proposed components and their state for exactly this app — put that proposal _in the spec_. You're about to find out how the plan survives contact with reality, which is a professional experience in miniature.

New folder, and note the scaffolding may handle git for you this time — Vite templates and agents often initialize the repo themselves. Check the Source Control panel; if there's no repo, you know the button. Either way: a commit before iteration begins. Some habits don't care what framework you're in.

## Getting it running

Here's where the previous lesson stops being theory. Depending on how agentic your assistant is, it may run the setup commands itself — fine, but _watch the terminal while it does_, because you can read that output now. Whether it drives or you do, the universal recipe applies, and I want you to at least once run it with your own hands: `npm install`, open `package.json`, read the scripts, `npm run dev`, find the URL in the terminal, open it. Congratulations — you just onboarded onto a modern JavaScript project, which is a sentence that would've been gibberish to you at the start of this course.

While the dev server's running, do the hot-reload party trick deliberately: open a component, change some visible text, save, and watch the browser update before your eyes leave the editor. That loop is where you and your agent now live.

## Reading duties

This is the richest read yet, because every skill from this section reports for duty:

- **Read `package.json` first.** There they are — react, vite, tailwind, maybe typescript — the dependencies you now know are other people's projects your app stands on. The manifest should hold zero surprises.
- **Find the state.** The React rule: find the `useState`s, find the memory. There should be state that smells exactly like your vanilla version — the current pet or pet list, the liked collection. Same memory, new container. If the agent used TypeScript, enjoy reading `useState<Pet[]>([])` like it's nothing.
- **Find your old friends.** Somewhere in there: a fetch with the two-await rhythm (likely inside a `useEffect` — render first, _then_ the side-task, and now you know why it lives there), an `onClick` handing over a recipe, and a `pets.map(...)` or conditional render painting the screen. Every load-bearing piece of your vanilla app exists here; the job is finding where each one moved.
- **Walk the doors.** Start at the root component and use the capitalization signal: every Capitalized tag is a door — Go to Definition through it, sketch the component tree in your notes. Then the reckoning: how close was the architecture your assistant proposed in the React lesson to what actually got built? Where they differ, ask it why — the answer is usually a genuine design consideration, and sometimes it's "no reason," which is also worth knowing agents will say.
- **Trace one Like click.** The course ritual, React edition: click → `onClick` → handler → `setSomething` → re-render → new pet on screen. Notice what you _didn't_ find: your old `rerender()` function. Nobody wrote it. That absence is React's entire pitch, visible in a diff of one.
- **Sight-read one component's Tailwind** with the grammar — property-dash-amount, colons as "when" — and then prompt one styling iteration ("make the card feel warmer") and read which words changed.

## Then compare

The payoff round. Open the vanilla app's `script.js` and the React version side by side — same movie, third cast — and hunt the same organs across both: where state lives, where the fetch happens, where clicks are handled, where the screen gets painted. Write the mapping in your notes, honestly: which version is easier for _you_ to follow right now? (There's no wrong answer — the vanilla version is likely still more legible to you, and that's fine and expected; React's advantages compound with app size, and this app is deliberately small.) You now hold something rare even among professional developers: the same application, written by your own hand, by an agent in vanilla JavaScript, and by an agent in React. That's not a toy comparison — that's a genuine basis for the "which tool for which problem" judgment this whole course keeps banging on about.

> 🤖 **Ask your AI Assistant.** The comparison, weaponized: paste your vanilla `script.js` and point the assistant at the React version, then ask "What did the React version buy me over this vanilla version, and what did it cost? Be specific to these two codebases, not generic." You've read both, so you can audit every claim it makes — push back where it's being generic, and ask for evidence in the actual files where it's being vague. Interrogating an assistant's _judgment_, not just its explanations, is the next level of the skill, and you've never been better armed for it than with an app you've now met three times.

Commit the final state, keep the repo — the pet saga has at least one act left. And if you've got appetite for one more build with a different shape entirely — search, forms, chained API calls — a bonus project follows.

[petsapi]: https://pet.btholt.workers.dev/pets/random
