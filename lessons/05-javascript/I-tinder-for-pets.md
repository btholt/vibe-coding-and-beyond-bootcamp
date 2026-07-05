---
description: "Homework: prompt your AI assistant to build Tinder for pets — and practice writing a spec, constraining the tech, and reading what comes back. Keep the repo when you're done; it gets superpowers later in the course."
---

For this homework you're building Tinder, but for pets, which is to say Tinder except good. One pet photo on screen at a time. Two buttons: **Like** and **Pass**. Either way, the next pet appears. When you've judged every pet, the app tells you who made the cut.

And here's the twist after a day of hand-building: **this one is a prompted project.** You're not writing this code — your AI assistant is, agentically, the way you built the calculator. Your job is everything around the code: writing a spec good enough to get what you want, constraining the tech so you can read the result, and then actually reading it. That division of labor is the whole game we're training for, and homework is where you get reps without me hovering.

## Your dating pool

I host pet photos for exactly this kind of thing, and you get to lean into your passions — pick your species, or mix them:

- Dogs: `https://pets-images.dev-apis.com/pets/dog1.jpg` through `dog39.jpg`
- Cats: `cat1.jpg` through `cat14.jpg`
- Birds: `bird1.jpg` through `bird9.jpg`
- Rabbits: `rabbit1.jpg` through `rabbit3.jpg`
- Reptiles: `reptile1.jpg` and `reptile2.jpg` (a boutique selection)

These URL patterns and ranges go _in your prompt_ — the assistant can't guess them, and handing an agent the exact data it needs is half of what separates a good spec from a vibe.

## Write the spec

Before you type a word to your assistant, write the spec — in your notes, in a scratch file, wherever. It should cover:

1. **What it is:** a pet-rating app. One photo at a time, each pet gets a made-up name displayed with it, Like and Pass buttons advance to the next pet, and when the pets run out, show a results screen: how many you liked and their names.
2. **The data:** the URL patterns and ranges above, and which species you want in the pool.
3. **The anti-goals**, just like the calculator: **plain HTML, CSS, and JavaScript in three files. No frameworks, no libraries, no build step, no localStorage.** This constraint isn't aesthetic — it keeps the output inside the JavaScript you can now read. An unconstrained agent will happily hand you React, and you're a few lessons away from reading React.
4. **Anything you care about:** how it should look, an undo button, keyboard shortcuts — your call. It's your app.

Then set up the project like it's real, because it is: new folder, **Initialize Repository**, and let the agent loose on your spec. You know the rule by now — with a fresh empty repo, your first commit _is_ the save point, so commit the skeleton or the first working version before you start iterating.

## Reading duties

The code arrives; now the actual homework starts. Before any follow-up prompts:

- **Find the state.** Somewhere near the top there'll be the app's memory — likely a counter for which pet you're on and an array collecting the liked ones. Find both. If the assistant used an array method you haven't met (`.push()` is a likely suspect — it adds an item to the end of an array), that's a vocabulary word, not a wall.
- **Trace one Like click** end to end, exactly like you traced the calculator's 7: listener → handler → state change → rerender. Write the route in your notes.
- **Ask one why-question** about the strangest thing in the file. There will be a strangest thing. There always is.

Then iterate with follow-up prompts and read each diff before accepting: make it beautiful, add an undo, add a like-percentage stat, arrow keys to swipe. Commit between rounds. If a round goes sideways, you know exactly where the discard button is — that calm is the point of all those save points.

> If the assistant ignores an anti-goal — sneaks in a library, adds a build step — don't fix its code; fix your prompt and regenerate. "You used X; I said plain HTML/CSS/JS in three files, no libraries — redo it within those constraints" works. Holding the line on constraints is a prompting skill, and agents respect a firm restatement far more often than you'd think.

## Do not delete this repo

I mean it. Push comes to shove, this is the most important instruction on the page: when you're done, keep the repo. Your app currently knows exactly the pets you told it about — a fixed guest list, hardcoded in a file. Soon we're going to teach your apps to go get things from the internet on their own, and this little app is going to be the first one you upgrade. Fixed guest list now; every pet on the internet once your app learns to fetch.
