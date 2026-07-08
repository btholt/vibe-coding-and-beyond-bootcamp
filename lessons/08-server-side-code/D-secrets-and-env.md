---
title: "Secrets and Environment Variables"
description: "The lesson the section has been building toward: why a secret can never live in frontend code, how servers keep keys, what .env files are, and the one mistake that costs real money — plus Brian calls OpenAI on camera so nobody else has to pay."
---

Back in the fetch lesson, your assistant showed you code for calling OpenAI, and I pointed you at the one part you couldn't read — an API key riding in a header — and told you the answer to "what happens if I put this in my browser JavaScript?" would turn out to be the entire reason servers exist for people like you. That answer has waited long enough.

First, what an API key _is_: a password for programs. When your code calls OpenAI or Stripe or a maps service, the key in the request is how the provider knows who's asking — which account to meter, and crucially, **which credit card to bill**. Your OpenAI key is your payment method rendered as a string of characters. It has cousins — database connection strings, signing secrets, service tokens — and the family name is _credentials_: strings that grant power, and therefore strings that must never get loose.

## Why the frontend can never hold one

Now cash the corollary you filed away at the start of this section: **every visitor gets a complete copy of your frontend code.** All of it, downloaded to their machine, readable in dev tools — you've read other sites' code yourself. So a key in browser JavaScript isn't hidden; it's _published_. And it's worse than that: even if the key were somehow buried in the code, the browser has to _send_ it to OpenAI on every request — and you know a tool that displays every request a page makes, headers and all, because you've used it. The Network tab shows your key to anyone who opens it. There is no clever place to hide a secret in the browser, because the browser's whole deal is running in public, on machines you don't control.

Two follow-ups before anyone gets clever. The build step doesn't save you: `dist` is _minified_, not encrypted — squished for size, trivially searchable, and key-shaped strings are exactly what bots search for. And the stakes aren't hypothetical: automated scanners crawl public code constantly, extracted keys get used within minutes, and "someone found my key" stories end with bills in the thousands. This is the single most expensive mistake available to a new builder, which is why it gets its own lesson.

## The pattern: your server holds the key

The fix is the client-server split doing exactly the job it was built for. The secret lives on the server — code that _never ships_, remember — and the browser talks to _your_ server instead of to OpenAI:

```
browser  →  your server (key lives here)  →  OpenAI
browser  ←  your server                   ←  OpenAI
```

The client sends the question, keyless. Your server attaches the credential, makes the real call, and passes the answer back. Only results ever travel to the browser — never code, never keys. Every serious app you've ever used works this way for every secret it holds, and now the fetch lesson's why-question has its full answer: the key goes in a header _server-side_, and putting that code in the browser means publishing your credit card. This proxy pattern is, for your purposes, the reason server-side code exists.

## Where the server keeps it: environment variables

So the key lives on the server — but _where_ on the server? Not in the code, it turns out, and the reason is a familiar villain: the code goes in git, git gets pushed to GitHub, and a key committed to a repo is a key published to yet another public place. (Bots scan GitHub specifically. A committed key is typically found and abused faster than you can say "wait." And deleting the commit doesn't help — git remembers everything; that was the whole pitch. A leaked key isn't cleaned up; it's _revoked and replaced_, immediately, at the provider's dashboard.)

The solution is to keep secrets in the machine's _environment_ rather than in the source: **environment variables** — named values a program reads at startup from the world around it instead of from its own files. In Node, they arrive on an object you can now read fluently:

```javascript
const apiKey = process.env.OPENAI_API_KEY;
```

During development, they're conventionally kept in a file called `.env` at the project root — just names and values:

```
OPENAI_API_KEY=sk-abc123...
DATABASE_URL=postgres://...
```

— and two pieces of discipline make the whole system safe. One: **`.env` goes in `.gitignore`, always** — it's the one file in the project that must never be committed, and agents know this convention cold (scaffolds usually pre-ignore it; verifying takes five seconds and is worth the habit). Two: the polite convention for teammates and future-you is a committed `.env.example` listing the _names_ with the values blanked, so anyone setting up the project knows which secrets to acquire without any secret being shared. In production, hosting platforms have a settings screen where you enter the same variables — same `process.env` on the reading end, no file involved. That's the whole apparatus: the code says _which_ secret it needs; the environment supplies it; the source stays clean enough to publish.

> Mechanics you'll recognize in the wild: something has to actually _load_ the `.env` file into the program, and for years the standard answer was a tiny package called **dotenv** — you'll see `require("dotenv").config()` or `import "dotenv/config"` near the top of countless Node apps, and now you know what it's doing. Newer Node versions can do it without the package: `node --env-file=.env server.js` reads the file natively, so fresher generated code may skip dotenv entirely. And the file is only the _usual_ delivery mechanism, not the only one — environment variables can be handed to a program right at launch (`OPENAI_API_KEY=sk-... node server.js`, or set by the hosting platform), no file involved. The constant across all of it is the reading end: when you spot `process.env.SOMETHING` in Node code, a secret or config value is being consumed there, and one of these mechanisms — dotenv, `--env-file`, or the environment itself — is where it came from.

## Watch a secret stay secret

Time for the demonstration this course owes you — the LLM call, done right, on my dime. Watch-me rules: a tiny Express server with one route,

```javascript
app.post("/haiku", async (req, res) => {
  const answer = await callOpenAI(req.body.topic, process.env.OPENAI_API_KEY);
  res.json({ haiku: answer });
});
```

a `.env` holding my real key (which you will notice I do not show on camera — practice what you preach), and a plain frontend that fetches `/haiku` with nothing but a topic in the body. I ask it for a haiku about, let's say, a Havanese; the answer comes back; and then the part that matters: **open the Network tab on the client.** Look at the request the browser actually sent. A topic. No key. No key in the source, no key in the bundle, no key in any request the visitor's machine ever makes — the secret stayed home, and the browser got only results. That's the pattern, witnessed.

You don't need to reproduce this — LLM APIs cost money and change shape, which is exactly why it's a demonstration — but file the shape away, because it is _the_ template: any keyed service you ever want your app to use (payments, email, maps, AI) gets wrapped in precisely this kind of thin server route. When the capstone comes and you want your app to do something powerful, "add a server route that holds the key" is the move, and your agent knows how to build it — now you know how to check its work.

> One trap specific to modern frontend tooling, worth its own box: **an environment variable in frontend code is not a secret.** Vite (and its cousins) will hand env vars to browser code if you prefix them — `VITE_SOMETHING` — and that mechanism exists for harmless _configuration_, not credentials, because anything the frontend reads gets baked into the public bundle. The bright line is where the variable is _read_: `process.env` in server code stays secret; anything reaching browser code is published, env var or not. If you ever see an agent put a key behind a `VITE_` prefix, that's a hard stop and a corrected prompt.

> 🤖 **Ask your AI Assistant.** The security audit, which from now on you run on everything: point your assistant at any project — the React pet app is fine — and ask "List every credential or secret this app uses, where each one lives, and whether any of them could reach the client." Today the answer should be "none, and none" (your apps have used my keyless APIs, by design — one more thing that was quietly on purpose). But you've just rehearsed the exact question that matters enormously the day an app of yours holds a real key, and the day your agent builds something you're about to make public. Bonus, answer key in hand: ask "I accidentally committed my .env file to a public GitHub repo — what do I do, in order?" and grade the response against this lesson: revoke and replace first, scrub the repo second, feel bad about it never — it happens to professionals weekly.

## Recap

- An API key is a password for programs — your account, your meter, your credit card, as a string. Family name: credentials.
- Frontend code can never hold one: every visitor gets a full copy of the code, and the Network tab shows every header. Minified is not encrypted. There is no hiding place in a building made of glass.
- The pattern: your server holds the key and proxies the call — browser sends the question, server attaches the secret, only results travel back. This is the reason servers exist for people like you.
- Secrets live in environment variables, read via `process.env`, kept in a `.env` file that is _always_ gitignored, documented by a committed `.env.example`, and entered into a settings screen in production.
- A leaked key is revoked and replaced, not deleted — git remembers, and the bots are faster than you.
- Env vars that reach frontend code (`VITE_`-prefixed) are configuration, not secrets — the bright line is where the value is _read_.

Your apps can keep a secret now. Next they need a memory — because every variable you've ever set dies when the process ends, and real apps have to remember. Databases, ahoy.
