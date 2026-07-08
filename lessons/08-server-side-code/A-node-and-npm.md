---
title: "Node.js and npm"
description: "JavaScript escapes the browser. Brian covers what Node actually is, the reveal that you've been using it all along, npm's fuller story, and the headline skill of the section: reading terminal output and stack traces without flinching."
---

Every line of JavaScript in this course so far has run in one place: a browser. That made sense — we were building things for browsers. But the language itself isn't chained to the browser, and hasn't been since 2009, when some enterprising people took the JavaScript engine out of Chrome and made it run anywhere: your laptop's terminal, a server in a data center, a Raspberry Pi in somebody's closet. That liberated runtime is **Node.js**, and it's the reason the phrase "server-side JavaScript" exists — the same language you've spent this course learning to read, running in an entirely different building.

## Two places code can run

Before Node makes sense, you need the map — because the deep divide in software isn't between languages or frameworks; it's between _where the code executes_.

Everything you've built in this course so far runs on your **visitors'** machines. Browser JavaScript works like an app from the App Store: someone comes to your site, their browser downloads your HTML, CSS, and JavaScript, and then _their device_ — their laptop, their phone — executes it. Not your server. Your server's involvement ends the instant the files finish downloading; from then on, the entire show runs on hardware you've never seen and don't control. (File away a corollary that becomes very important shortly: every visitor gets a complete copy of your frontend code. All of it. Anyone can open dev tools and read it — you've done it yourself.)

Server-side code is the opposite arrangement. It runs on _your_ computer — realistically, a computer you rent from AWS or Netlify or the like — and the code itself is **never sent to anyone**. Clients send requests and get back only _results_. You've been on the receiving end of this deal all course: you've called the words API hundreds of times and gotten JSON answers every time, but you've never once received the code that produces them. That code stays home.

So why split things across two places at all? Games make both halves of the answer vivid. Think about something like World of Warcraft or League of Legends or Madden: the game _has_ to download and run on your device, because the alternative — sending every button press to a server, computing the result there, and streaming it back — would be unbearably slow. You'd press "pass" and, half a second later, at best, the pass would happen. For anything that needs to _feel instant_, the code must run where the player is. But here's the other half: the client can't be trusted. If the game simply believed whatever clients told it, cheating would be effortless — your client just announces "I have all the best loot," and there's no way to verify otherwise. So the _truth_ lives on the server: a central authority that reads and writes the database, arbitrates what's real, and runs logic you'd rather keep secret — which is only possible server-side, because (see the corollary above) browsers inherently get a copy of your code, and servers inherently don't share theirs.

The everyday web version of the same principle is a friendship on Facebook. It's not enough for my browser to simply declare "I'm friends with this person" — my client sends a friend request _to the server_, the other person tells the server "yes," and the _server_ records that we're friends. The server sits in the middle precisely so there's someone to verify both halves and store the result. That central, trustworthy repository of state — who's friends with whom, who owns what, who paid, who's allowed — is the single biggest reason servers exist.

So, the rule-of-thumb map: things that need to feel instant and things that are presentation run on the client; truth, secrets, and anything requiring a central authority run on the server. Every app you'll ever prompt into existence is some arrangement of these two halves talking over the fetch calls you already know how to read.

Worth placing on this map before we go on: the _browser_ half is JavaScript by necessity — it's the language browsers speak, full stop. The _server_ half is anybody's game. Servers are just computers you control, so they'll run whatever language you like, and the world's backends are written in Java, Python, Ruby, C#, Go, PHP, and plenty more — if you've heard those names around the office, this is where they live: server-side, doing exactly the truth-and-authority jobs described above. Writing your server in JavaScript is simply _one of the options_, and Node.js is what makes it possible. Which clears up a naming confusion worth settling now: **Node.js is not a language.** The language is JavaScript, on both sides of the divide. Node is the _engine_ — the runtime — that executes JavaScript on a server the way a browser executes it on a client. One language, two engines, two buildings. And the appeal of picking JavaScript for the server half is exactly what this course has been banking on: it's the same language, so everything you can read on one side, you can read on the other.

One more asymmetry between the two buildings, and then we can meet Node properly. Because browsers execute _strangers'_ code every time you visit a website, browser JavaScript is deliberately locked in a padded room — it gets `document`, `querySelector`, and click events, but it's forbidden from touching your files or your operating system. And thank goodness: you wouldn't want to live in a world where clicking a link meant worrying that the website could rifle through your tax returns or read your photos off the disk. Visiting a page runs that page's code on _your_ machine, remember — the only reason that's a safe thing to casually do a hundred times a day is that the room is padded. Node is the opposite: none of the page stuff — no DOM, no `document`, nothing to click — and none of the padding. It can read and write files, talk to databases, listen for network requests, and run for months without a "page" existing at all. Same grammar, same `const` and `await` and objects and arrays — every reading skill you have transfers — but the vocabulary of what it touches changes from "elements on a page" to "the machine itself."

## The reveal: you've been using it all along

Here's this course's favorite move one more time. When you ran `npm create vite@latest` — that was a Node program. The dev server that hot-reloads your React app? A Node program, running on your machine right now if it's still up. The build step, the TypeScript checker, the thing that generates Tailwind's stylesheet, very likely chunks of your AI coding agent's own tooling: Node programs, all the way down. You haven't been _about to meet_ Node; you've been living in a house it built. The frontend world runs its entire workshop on server-side JavaScript, even when the product being built is browser JavaScript — which is a strange and true fact that quietly answers "why does my frontend project need Node installed."

Watching it run directly is almost anticlimactic. A file, `hello.js`:

```javascript
const name = "Node";
console.log(`Hello from ${name}!`);
```

And in the terminal:

```
$ node hello.js
Hello from Node!
```

No HTML file, no script tag, no browser — `node` runs the file, top to bottom, one line at a time, like always. And notice where `console.log` went: **the terminal is the console now.** Same flashlight, new wall. When your agent writes server code and says "check the logs," this is where they come out.

To show you the kind of thing browser JavaScript could never do, here's Node reading a file off the disk — two lines:

```javascript
const fs = require("fs");
console.log(fs.readFileSync("hello.js", "utf8"));
```

A program that reads its own source code off your hard drive. Harmless here, unthinkable in the padded room. (That `require` is Node's classic way of importing — you'll see it in older code; newer Node uses the `import` syntax your React files use. Read both as "go get this module.")

This is a watch-me section, per the usual deal — you're not becoming a Node author, and when server code needs writing, your agent writes it and runs it. Your job is what it's been all course: read what it wrote, and read what the terminal says back. Which brings us to the headline skill.

## Reading the terminal without flinching

Server-side work happens in the terminal, so the terminal's dialect becomes required reading — and the single most important genre in it is the **stack trace**, which is what Node prints when something dies. Here's one, from a program that tried to read a file that doesn't exist:

```
Error: ENOENT: no such file or directory, open 'petz.json'
    at Object.openSync (node:fs:596:3)
    at Object.readFileSync (node:fs:464:35)
    at loadPets (/Users/brian/pets-api/data.js:12:19)
    at Object.<anonymous> (/Users/brian/pets-api/server.js:4:15)
```

People see a wall like this and panic. Here's the decoder, and it makes stack traces nearly friendly:

**The first line is the complaint.** Same as always — errors are specific complaints, not insults — and this one even includes the evidence: `no such file or directory, open 'petz.json'`. (Squint at that filename. The bug is already visible.)

**Everything under it is the trail** — the chain of function calls that led to the death, most recent first. Each line: a function, a file, a line number, a column. You can read that — it's functions calling functions, the thing you've traced a dozen times, printed as a receipt.

**The triage skill: find the topmost line that's _yours_.** The first two frames live in `node:fs` — that's Node's own internals, not your problem, skim past (in bigger apps this is where `node_modules` paths appear — libraries, same rule: not where your bug is). The first _your-code_ frame is `loadPets` at `data.js:12` — and that's where you open the editor. The trace literally hands you the file and line. Complaint, trail, topmost-yours: that's the whole method, and it works identically on the browser console's red errors, which — surprise — have been stack traces all along, just usually shorter.

## npm: the fuller story, as promised

You know npm as the app store for code; here's the rest, briefly. The registry hosts millions of packages, and the reason it matters _more_ on the server side is that Node code leans on it harder — a browser app might have a dozen direct dependencies; a server app strings together packages for the web framework, the database, the auth, the everything. A few working facts:

**`npm install <thing>`** adds a package to your project — writes it into the `package.json` manifest and pulls the code into the `node_modules` warehouse. This is what your agent runs when it "adds a library," and the manifest diff is how you catch it happening.

**Those version numbers** next to each dependency — `"express": "^4.19.2"` — are worth thirty seconds of reading skill. Three numbers: major.minor.patch, roughly meaning big-changes.new-features.bug-fixes. The caret (`^`) means "this version or compatible newer ones." Recognition-level takeaway: when an agent or an error message talks about a "version mismatch" or "breaking change in v5," this notation is what it's referring to.

**`devDependencies`** — the answer to the question from the build-step interview — are packages needed to _build and develop_ the app (Vite, TypeScript, the checker) but not to _run_ the finished thing. Regular `dependencies` ship with the app; dev ones stay in the workshop.

**`npx <thing>`** runs a package without permanently installing it — agents use it constantly for one-off tools, and you already have: `npm create vite` is this machinery in a party hat.

> A heads-up worth its weight: the packages are _other people's code_, and `npm install` executes with your full user powers — no padded room out here. The overwhelming majority of the registry is fine, but supply-chain attacks (malicious packages with names one typo away from real ones) are a real genre. The habit that costs nothing: when an agent adds a dependency you've never heard of, ask it what the package is and why it chose it. You did exactly this in the build-step interview; now you know the security reason it wasn't just trivia.

> 🤖 **Ask your AI Assistant.** Stack-trace practice with the answer key in hand: paste the ENOENT trace from this lesson into your assistant and ask "Where's the bug, and which file and line should I open?" You already did the triage — complaint, trail, topmost-yours, and that suspicious `petz.json` — so audit the answer. Then the question that upgrades your future debugging: "When I paste stack traces to you, what _else_ should I include to help you fix things faster?" The answer — usually the code around the named line, and what you were doing when it died — is a prompting lesson straight from the source, and it'll pay off every time something breaks from here to forever.

## Recap

- The deep divide is _where code executes_. Browser JavaScript is an App Store deal: visitors download your code and run it on their devices — and everyone gets a full, readable copy. Server-side code runs on machines you control and never ships; clients get only results.
- The split exists for a reason on each side: instant-feeling things must run where the user is (the game-latency problem), while truth, secrets, and central authority must live where users can't reach (the loot-cheating problem, the friend-request arbitration).
- Node.js is JavaScript outside the browser: same language, every reading skill intact, but the world changes from "elements on a page" to "files, networks, the machine itself" — and no padded room.
- You've been using it all along: Vite, the dev server, the build step, the agent's tooling — the frontend workshop runs on Node.
- The terminal is the console now. Stack traces are the headline genre: first line is the complaint, the rest is the trail, and the move is _find the topmost line that's yours_ — skim past `node:` and `node_modules` frames.
- npm, fuller: install writes the manifest and fills the warehouse; `^major.minor.patch` is readable version notation; devDependencies stay in the workshop; npx runs without installing.
- Dependencies are other people's code running with your full powers — when an agent adds one you don't recognize, ask what it is and why.

Next: we build a server. A real one, from almost nothing — and you find out my words API has been a couple dozen lines of this stuff all along.
