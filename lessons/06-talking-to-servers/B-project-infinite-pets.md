---
description: "The promised upgrade: your pet app's fixed guest list becomes a live API. Rehearse the endpoint in the console, prompt the modification, read what changed — and for the ambitious, there's a Wordle-shaped bonus."
---

Told you not to delete that repo. Your pet app currently knows exactly the pets you hardcoded into it — a fixed guest list. In this homework, you upgrade it to fetch pets from a live API, which means the guest list gets outsourced to a server, and your little app joins the ranks of software that actually _does_ something with the world.

There's a professional pattern hiding in this assignment, so let me name it: **build v1 with fake data, wire up the real data later.** That's not a beginner shortcut — that's how experienced teams ship. Hardcoded data let you get the whole swiping experience working without the internet's complications; now the experience is proven and you're swapping the data source underneath it. You'll do this exact two-step in every substantial project you ever build, so enjoy the first rep.

## Rehearse the API before you cast it

The pets are coming from the [Pets API][petsapi] — which I run, specifically for this. Before you prompt anything, I want you to talk to it yourself, in the console, the way we did with the words API:

```javascript
const response = await fetch("https://pet.btholt.workers.dev/pets/random");
const data = await response.json();
console.log(data);
```

You'll get back a full pet profile — name, species, breed, age, and a photo URL. Find the image in the shape that comes back and write down the dot-path to reach it. That's a fact your prompt is going to need, and now it's a fact you've _verified_ rather than assumed.

While you're rehearsing, try the API's other moves — each of these is a design option for your app:

```javascript
// a batch of ten at once
await fetch("https://pet.btholt.workers.dev/pets/random/10");

// lean into your passions
await fetch("https://pet.btholt.workers.dev/pets/random/5?species=cat");
```

Console rehearsal is a rule worth tattooing next to the git one: **never prompt an agent to build against an API you haven't personally called once in the console.** Assistants will confidently generate code against APIs that are dead, renamed, or shaped differently than they remember — their knowledge of the internet has an expiration date and no warning label. Thirty seconds of rehearsal converts "the agent says this API works" into "I watched this API answer."

> Why am I running my own pet API instead of pointing you at one of the internet's free ones? Because while writing this very course, the beloved community-run dog API I'd planned to use went down — 520 errors, the "server's own fault" flavor you now know how to read — right as I was writing this homework. It came back, but the lesson stuck: **third-party APIs go down, change, and disappear**, which is precisely why you rehearse before you build, and why an app you actually ship needs to survive its API having a bad day. File that under "things the pros learned the hard way."

## The spec

You know the drill by now — spec first, then prompt. Yours should cover:

1. **The change:** this is a modification of an existing app, and your prompt should say so — "here is my working app; change where the pets come from, keep everything else working." Replace the hardcoded array with pets fetched from the API.
2. **The API facts you verified:** the URL (including any `?species=` preference — your app, your passions) and the dot-paths to the fields. Handing over verified facts is what separates your prompt from a guess.
3. **A design decision — yours to make:** fetch one new pet per swipe (`/pets/random`), or fetch a batch up front and swipe through it (`/pets/random/10`)? Both are legitimate; per-swipe is simpler but every swipe waits on the internet, batch feels snappier but front-loads the wait. Pick one, put it in the spec. (Deciding things like this _is_ the job. The agent can implement either; it shouldn't be choosing for you. Welcome to product management lol.)
4. **Show off the profile:** the API sends more than a photo — name, breed, age. Your spec should say how the card displays them. "Winston, 9, Cattle Dog Mix" hits different than an anonymous photo; this is a dating app, after all.
5. **The anti-goals, unchanged:** plain HTML/CSS/JS, three files, no frameworks, no libraries.

And because this is your first time letting an agent modify a _working_ app — the exact scenario the whole git habit exists for — you know what comes before the prompt. Commit. This is the boss fight the save point was invented for.

## Reading duties

The diff arrives. Before accepting, and after:

- **Find the `async`.** You learned the triage signal: `async` marks where the internet happens. There should now be at least one in your file — find it, find the two-await rhythm inside it.
- **Open the Network tab and swipe.** Watch your own app's requests appear in real time — one per swipe, or one batch, depending on your design call. Click a request and read the raw JSON coming back. That's your app, talking to the world, observed in flight.
- **Trace one Like click** again, like always — except the route now crosses the internet: click → handler → fetch → await → response → `img.src`. Note where the _waiting_ happens in the code.
- **Interrogate the `catch`.** Is there one? Is it silent? Ask the app the subway question: "what does this do if the network fails mid-swipe?" If the answer is "fails silently" or "no idea," that's your follow-up prompt: add friendly error handling. You know why silent catches are sinister.
- **Notice the gap.** Swipes take time now — that's not a bug, that's the speed of light and some routers. Did the agent handle the in-between moment (a loading state, a spinner, anything)? If a slow connection would show a broken-image icon, prompting for a loading state is a worthy iteration round. Every real app you've ever used has solved this; now you know it was ever a problem to solve.

Whatever you build: commit it, keep it. The pet app in particular isn't done growing — it's got at least two more transformations ahead of it in this course.

[petsapi]: https://pet.btholt.workers.dev/pets/random
