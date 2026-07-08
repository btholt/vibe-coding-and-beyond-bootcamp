---
title: "Your First Node.js Server"
description: "From five lines of vanilla Node to Express, with the reveal the course has been building toward: the words API you've been calling all along is a couple dozen lines of exactly this. Routes are just if-statements for URLs."
---

Here's the promise for this lesson: by the end of it, you will read an entire web server, top to bottom, and find nothing in it you can't account for. Not a toy either — the words API you've been calling since the fetch lesson, the one that's answered you hundreds of times, is a couple dozen lines of what you're about to see. Servers have a reputation as the deep, scary end of programming. The reputation is wrong, and the fastest way to prove it is to build one from almost nothing and ramp up. Watch-me rules apply as usual — follow along if you like, but your job is reading.

First, what makes a server program _different_. Every program you've seen Node run so far did its work and exited — `hello.js` said hello and was gone. A server is a program that **starts and then refuses to end**: it sits there, listening, waiting for requests to arrive over the network, answering each one, forever — or until someone stops it. If that "sits there waiting for something to happen, then runs a handler" shape sounds familiar, it should: a server is an event listener, except the events come from the internet instead of from clicks. You already have the mental model. We're just changing what knocks.

## Five lines

The smallest real server, in Node with zero packages:

```javascript
const http = require("http");

const server = http.createServer((request, response) => {
  response.end("Hello from my server!");
});

server.listen(3000);
```

Read it with everything you have. Import Node's built-in `http` module. Create a server, handing it — look at the shape — _a recipe to be called later_, once per incoming request, exactly like the recipe you handed `addEventListener`. The recipe gets two arguments: the `request` (the envelope that arrived — who's asking, for what) and the `response` (your toolkit for answering), and this one answers everything the same way: some text, done. Then `listen(3000)` — hold that thought — turns it on.

Run it — `node server.js` — and the first lesson is what the terminal _doesn't_ do: it doesn't give you your prompt back. No output, no exit, just... sitting there. **That's a server working.** The program is alive and listening; the terminal is occupied for the duration. (When you want it back: Ctrl+C, the universal "stop the running program.") Now open a browser to `http://localhost:3000` and there's your text. `localhost` means "this very machine" — you didn't go out to the internet; your browser knocked on your own computer's door. And the `3000` is a **port**: if your computer's address is the building, ports are the apartment numbers, letting one machine run many listening programs at once. The dev server you've used lives at apartment 5173; this one's at 3000.

Congratulations, sincerely: client and server, both yours, talking on one laptop.

> A little more apartment-building lore, since ports come up constantly. Ports below 1024 are the reserved floors — your operating system requires special permission to listen on them, which is why development servers all pick roomy numbers up high: 3000, 5173, 8080. Two of the reserved ones are worth knowing by name, because they're where production web servers actually live: **80** is the default port for HTTP and **443** is the default for HTTPS — they're why you don't type a port when you visit a website; the browser assumes 443 for any `https://` address, which is how my words API answers you without ever mentioning an apartment number. You won't need to configure any of this yourself — hosting platforms handle it — but "the app listens on 3000 in dev and the platform maps it to 443 in production" is a sentence that will cross your path, and now it parses.

## Ramp one: answering different things

One answer for every question is a doorbell, not an API. (Or it's [Yo][yo], the app that literally just sent the word "Yo" — and raised $1.5 million at a $10 million valuation in 2014, so what do I know. Build your doorbell; dream big.) Real servers look at _what was asked_ — and the envelope has it, in `request.url`:

```javascript
const server = http.createServer((request, response) => {
  if (request.url === "/pets") {
    response.end(JSON.stringify([{ name: "Luna", age: 6 }]));
  } else if (request.url === "/hello") {
    response.end("Hello from my server!");
  } else {
    response.statusCode = 404;
    response.end("Not found");
  }
});
```

Stop and appreciate what you just read, because it's the thesis of this whole lesson: **routes are just if-statements for URLs.** Ask for `/pets`, take the pets branch; ask for `/hello`, take that one; ask for anything else and — look at it — _you_ are now the one sending the 404. The famous status code from your catalog, and here's the other side of the counter: it was never a mysterious force. It's an `else` branch, written by a person, politely declining. `JSON.stringify` you also know — pack the object for shipping — because the JSON your frontends unwrap has to get wrapped by someone, and that someone is this line.

## Ramp two: Express, because tedium

Keep hand-rolling this and the tedium arrives on schedule: parsing URLs with query params, telling GET from POST, reading request bodies, setting headers correctly — all doable with if-statements, all miserable. You know this course's pattern: feel the tedium, then meet the tool. The tool is **Express**, an npm package (a dependency! other people's code! the machinery you know!) that's been the standard way to write Node servers for over a decade, and it is overwhelmingly what agents emit. The same server, industrialized:

```javascript
const express = require("express");
const app = express();

const pets = [{ name: "Luna", age: 6 }];

app.get("/pets", (request, response) => {
  response.json(pets);
});

app.get("/hello", (request, response) => {
  response.send("Hello from my server!");
});

app.listen(3000);
```

The if-chain became a **route table**, and each route reads as three parts: the _method_ (`app.get` — and yes, `app.post`, `app.put`, `app.delete` exist, your GET-versus-POST knowledge meeting its server half), the _path_ (`"/pets"`), and the _handler_ (the recipe, same `(request, response)` shape as before — you'll usually see it abbreviated `(req, res)`, and now you can read that convention forever). `response.json(pets)` is `JSON.stringify` plus the right headers in one call — the exact mirror of the `response.json()` your frontends call to unwrap. One side packs, the wire carries, the other side unpacks. You've now read both ends of every fetch you've ever made.

And the route table is more than convenient — it's the **table of contents of a backend**. When your agent generates a server, scanning for `app.get`/`app.post` lines tells you everything the API offers, the way headings tell you what's in a document. That's your first move in any server file from now on.

## The reveal

So here, at last, is roughly what the mighty words API looks like:

```javascript
app.get("/word-of-the-day", (req, res) => {
  res.json({ word: getTodaysWord(), puzzleNumber: getPuzzleNumber() });
});

app.post("/validate-word", (req, res) => {
  const guess = req.body.word.toLowerCase();
  res.json({ word: guess, validWord: dictionary.includes(guess) });
});
```

That's it. That has been it the whole time. A route table with two entries: a GET that packages up the day's word, and a POST that reads the guess out of the request body (`req.body` — the envelope's contents, the very thing your `JSON.stringify({ word: "BILLY" })` shipped) and answers with an `includes` check against a word list. The server you've treated as infrastructure since the fetch lesson is a couple dozen lines of if-statements-for-URLs wearing Express. This is the demystification the whole course has been driving at: it was never magic. It was always just code, and now you read it.

> One famous error to collect before we go, because everyone meets it in their first week of server life: `Error: listen EADDRINUSE: address already in use :::3000`. Translation: some program is already living in apartment 3000 — almost always _your own previous server_, still running in a terminal you forgot about. The fix is finding that terminal and Ctrl+C-ing it (or asking your assistant for the command that evicts whatever's on the port). File next to the others: scary name, mundane cause, thirty-second fix.

> 🤖 **Ask your AI Assistant.** The sandwich, articulated: paste in both versions of the pets server — the vanilla `http` one with the if-chain and the Express one — and ask "What is Express actually saving me here? Be specific about what the http version would need to grow to match." The answer is a tour of exactly the tedium you were spared, which is the best way to appreciate a tool: knowing precisely what it eats. Then the practical follow-up you'll use for real: paste any Express server and ask "List every endpoint this server offers, its method, and what it returns" — you're teaching yourself to demand the route table as documentation, which works on servers of any size.

## Recap

- A server is a program that starts and refuses to end: an event listener whose events arrive from the network. The occupied terminal is the sign of life; Ctrl+C is the off switch.
- `localhost` is this very machine; ports are apartment numbers, so many programs can listen on one computer.
- Routes are just if-statements for URLs — and 404 was always an `else` branch written by a person.
- Express industrializes the pattern into a route table: method, path, handler `(req, res)`. The route table is the backend's table of contents — scan for it first in any server file.
- `res.json` packs what your frontend's `response.json()` unpacks; `req.body` receives what your `JSON.stringify` shipped. You now read both ends of the wire.
- The words API is a couple dozen lines of this. It was never magic.

Next: the grammar of how route tables get designed — because nearly every API in the world follows the same conventions, and once you know them, you can predict a backend's shape before you even open it.

[yo]: https://techcrunch.com/2014/07/18/yo-raises-1-5m-in-funding-at-a-10m-valuation-investors-include-betaworks-and-pete-cashmore/
