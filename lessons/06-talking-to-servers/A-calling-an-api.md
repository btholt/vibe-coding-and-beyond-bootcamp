---
title: "Calling an API"
description: "Every app you'll ever build talks to the internet within its first ten minutes of existence. Brian covers fetch, async and await, GET versus POST against a real live API, and the Network tab — your new flashlight."
---

Every single thing we've built so far has been a sealed terrarium: the page loads, the JavaScript runs, and nothing from the outside world ever gets in. Real apps don't live like that. Real apps ask a weather service for the forecast, ask OpenAI for a paragraph, ask a database for the user's saved pets — and every one of those conversations happens the same way: your JavaScript sends a request across the internet to a **server**, and the server sends back an answer. This lesson is that conversation, end to end, against a real server that I personally keep running.

A server, for our purposes right now, is just another computer that answers questions over the internet. (What it looks like _from the inside_ is its own whole section, coming later — for now we're staying on our side of the counter.) When a server's job is answering with raw data rather than web pages, we call it an **API**. Your apps will talk to APIs constantly, from their very first prompted feature. Time to learn the protocol.

> A word about the word, because it's one of the most overloaded terms in all of programming. API stands for _Application Programming Interface_, and in the broadest sense it means "the set of things some piece of software lets you ask it to do." People use it for all kinds of things — the browser has APIs, libraries have APIs, your assistant will say "the DOM API" and mean querySelector and friends. That's all legitimate usage and it can be genuinely confusing. For now, when this course says API, we mean the flavor in this lesson: **a server somewhere exposing a function for us to call remotely.** We can't run the word-of-the-day function ourselves — it lives on my server — but we can call it from anywhere on earth and get the answer back. That's the magic trick, and the rest is bookkeeping.

## The problem of time

First, the complication that shapes everything. Way back in your first JavaScript lesson I said JavaScript runs one line at a time, and that there were asterisks we'd get to later. It's later.

The internet is slow. Not slow like a bad restaurant — slow like _hundreds of milliseconds_, which to you is an eyeblink but to a computer executing millions of operations per second is an aeon. If JavaScript truly stopped dead on its current line while waiting for a server across the ocean to answer, your whole page would freeze — no scrolling, no clicking, nothing — for every single request.

So JavaScript does what you do at a coffee shop. You order, they take your name, and you go sit down — you don't stand rigidly at the register blocking the line until your drink materializes. You wait _productively_, and when your name gets called, you pick up where you left off. JavaScript's word for "order placed, call my name when it's ready" is `await`.

## Your first fetch

Open the console — top-level `await` works right there, which makes the console the perfect rehearsal space for this. Type these two lines:

```javascript
const response = await fetch("https://words.dev-apis.com/word-of-the-day");
const data = await response.json();
```

> 🐛 You might see some error about a security policy violation. If that's so, just try a different web page - some sites restrict the ability to make requests, even from the console, on them. I tried example.com and it works great.

Then look inside:

```javascript
console.log(data);
```

You just talked to a server. That URL is a real API — it hands out a secret five-letter word that changes daily (why five letters? patience) — and what came back should look something like:

```json
{ "word": "shirt", "puzzleNumber": 841 }
```

> Because this is a GET (we'll get into that later) request, you can actually do it from your browser directly. Open [https://words.dev-apis.com/word-of-the-day]() directly in your browser and see the same result. This only works on some API requests, not all.

You know how to read that. It's an object — the shape you learned in the objects and arrays lesson, arriving over the wire exactly as promised. `data.word` is the word. Dots still mean "go inside," even when the object traveled an ocean to get to you.

Now let's read those two lines properly, because they're the most important two lines in this section:

**Line 1:** `fetch(url)` places the order — it fires the request off across the internet. `await` is "wait here for the answer, but let the browser keep working" — the page stays alive, and when the response arrives, this line finishes and hands you the result. What you get back is a `response` object: the envelope, with postal details like whether the request succeeded.

**Line 2:** `response.json()` opens the envelope and parses the letter inside — takes the raw text the server sent and turns it into a real JavaScript object you can dot into. Opening the envelope _also_ takes a moment, hence the second `await`. Two awaits per fetch is the standard rhythm; you'll see this exact pair thousands of times in generated code, and now it reads as "order, wait; unwrap, wait."

## The async wrapper

One wrinkle before this leaves the console. In a script file, `await` isn't allowed to just float anywhere — it has to live inside a function marked `async`:

```javascript
async function loadWord() {
  const response = await fetch("https://words.dev-apis.com/word-of-the-day");
  const data = await response.json();
  console.log(`Today's word is ${data.word}`);
}

loadWord();
```

The `async` keyword in front of a function is the author telling you — there's that reading signal idea again — "this function talks to something slow; expect `await`s inside." When you're scanning a generated file, every `async` is a little flag that says _the internet happens here_. That's genuinely useful triage: in a file of twenty functions, the `async` ones are where the data comes from.

> You will also see an older style in generated code: `.then()` chained onto fetches, like `fetch(url).then((response) => ...)`. It's the same coffee shop with different choreography — a recipe handed over to run when the order's ready, a shape you know from event listeners. Read `.then(...)` as "and when that finishes, do this." Agents mostly write `await` these days, but `.then` shows up enough that it shouldn't startle you.

## The Network tab: your new flashlight

Time for the second flashlight of the course. Open your dev tools and click the **Network** tab, then run the `loadWord` snippet again. A row appears — that's your request, captured in flight. Click it.

Everything about the conversation is in there: the **Headers** section shows what was asked and of whom; the **Response** section shows the exact raw JSON that came back, before your code touched it. This matters enormously for the debugging life ahead of you, because it splits every "my app isn't working" into two cleanly separated questions: **did the server send the right thing?** (look at the Network tab) and **did my code handle it right?** (look at the console). Knowing which side of the counter the problem is on is half of every fix.

While you're in there, look at the **Status** column: `200`. Status codes are the server's one-number mood report, and three of them are famous enough to memorize on the spot: **200** means all good; **404** means "no such thing here" (you've seen this one in the wild your whole life); anything starting with **5** means the _server itself_ fell over — not your fault, nothing your code did wrong. These numbers join your famous-errors catalog, and unlike the others, they come with the reassuring property that a 500 is explicitly not your bug.

## Sending data: POST

Everything so far has been _asking_. Sometimes you need to _tell_ — send the server data: a chat message, a form, a guess. Asking is called a **GET** request (fetch's default), and sending is called a **POST**. My words API happens to have a POST endpoint that checks whether five letters are a real English word — try it in the console:

```javascript
const response = await fetch("https://words.dev-apis.com/validate-word", {
  method: "POST",
  body: JSON.stringify({ word: "BILLY" }),
});
const data = await response.json();
console.log(data);
```

Same two-await rhythm, but now `fetch` takes a second argument: an object of options — a shape you can read — saying this is a POST, and here's the `body`, the package we're shipping. And `JSON.stringify` is the mirror twin of `response.json()`: one packs a JavaScript object into JSON text for shipping, the other unpacks arriving JSON text into an object. Pack to send, unpack on receipt. Every app you ever build will do both, thousands of times.

(Is BILLY a valid word? The server says yes, delightfully. Try some others. Try your name. This endpoint will matter again very soon, and if the phrase "secret five-letter word that changes daily" has been itching at you this whole lesson — yes. It's Wordle. I've been running a Wordle API this entire time, and now you can speak to it fluently.)

## When the internet says no

The internet fails. Wifi drops, servers crash, URLs get typo'd. Generated code deals with this using a structure you need at read level, because your assistant will wrap nearly every fetch it writes in one:

```javascript
try {
  const response = await fetch("https://words.dev-apis.com/word-of-the-day");
  const data = await response.json();
  console.log(data.word);
} catch (error) {
  console.log("Something went wrong talking to the server:", error);
}
```

`try` reads as "attempt everything in these braces," and `catch` as "if _anything_ in there explodes, don't crash — jump here instead, with the error handed to you." It's a net under the risky part. You don't need to author these; you need to not be surprised by them, and to know that when you're hunting a bug, the `catch` block is where the failure's evidence gets delivered — which is why a `catch` that quietly does nothing (agents write these sometimes, and it's a bad habit of theirs) can make a broken app fail in eerie silence. If something's failing with no error anywhere, go find the `catch` and see if it's swallowing the scream.

> 🤖 **Ask your AI Assistant.** No key required, pure reading: ask "Show me the JavaScript fetch code an app would use to call the OpenAI chat API. Don't execute anything, just show me the code." Look at what comes back and count how much of it you can already read: `async`, `await fetch`, `method: "POST"`, `JSON.stringify` packing the message, the two-await unwrap. It should be nearly all of it — the calls your prompted apps make to AI models are this lesson's exact shapes with a fancier address. The one genuinely new thing will be a `headers` section carrying an API key. Ask the natural why-question — "why does the key go in a header, and what happens if I put this code in my browser JavaScript?" — and remember the answer, because when we get to server-side code, that question turns out to be the entire reason servers exist for people like you.

## Recap

- A server is another computer that answers over the internet; an API is a server that answers in data. `fetch` is how JavaScript starts the conversation.
- `await` is the coffee-shop wait: order placed, page keeps breathing, resume when it's ready. Two awaits per fetch: order-and-wait, unwrap-and-wait.
- `async` on a function is a reading signal: _the internet happens here._
- The Network tab shows the raw conversation, splitting every bug into "server's side or my side?" And status codes are the server's mood: 200 good, 404 no-such-thing, 5xx the server's own fault.
- GET asks, POST tells; `JSON.stringify` packs for shipping, `response.json()` unpacks on receipt.
- `try/catch` is the net under the risky part — and a silent `catch` is where errors go to hide.

Up next, your pet app finds out about all of this — and its fixed little guest list becomes the whole internet.
