---
title: "REST"
description: "The grammar behind nearly every API: URLs as nouns, methods as verbs, and one endpoint meaning several things depending on how you ask. Plus Bruno, a friendly GUI for poking at APIs — and the truth that these are conventions, not laws."
---

The route tables agents generate aren't arranged at random — there's a grammar, it's called **REST**, and nearly every API on the internet speaks it, including every one you've called in this course. The acronym stands for REpresentational State Transfer, which has never helped a single person, so ignore it; what matters is the convention, and the convention is small enough to learn in one sitting: **URLs are nouns, methods are verbs, and a request is a sentence.**

You've seen the nouns: `/pets` is a collection, `/pets/42` is one specific pet within it. And you know two verbs — GET and POST. The idea you don't have yet, and the one this lesson exists for, is this: **the same URL means different things depending on the verb you send it with.** Watch one noun conjugate:

```
GET    /pets      → give me the list of pets
GET    /pets/42   → give me pet 42
POST   /pets      → here's a new pet; add it
PUT    /pets/42   → here's pet 42, revised; replace it
DELETE /pets/42   → remove pet 42
```

Five different actions, two URLs. In Express, each of those is its own entry in the route table — `app.get("/pets/:id", ...)`, `app.delete("/pets/:id", ...)` — same path, different method, different handler. (That `:id` is a placeholder that matches any value in that slot, arriving in the handler as `req.params.id`. Now you can read that too.) This is why you can _predict_ a backend before opening it: if an app has pets, its API almost certainly has some subset of that exact table, because everyone shapes their sentences the same way.

## Conventions, not laws

Now the part people get religious about, and the truth that keeps you sane. Developers _love_ to assign deep meaning to the verbs — POST is for creating! PUT replaces the whole thing but PATCH updates part of it! There are blog wars about this. Here's the ground truth: **nothing enforces any of it.** The verb is just a label on the request, and the server does whatever its code says. You could — please don't, but you _could_ — write `example.com/api/create` and have it delete things. No error, no REST police, nothing but the judgment of your peers. The conventions are etiquette, not physics: valuable precisely because everyone voluntarily follows them, which is what makes APIs predictable, agents' output consistent, and your guesses about unopened backends mostly right. But when reading real code, the posture is _expect the convention, verify in the handler_ — what a route actually does is defined by the code inside it, nowhere else.

And a frequency reality check, so you weight your attention correctly: **every app uses GET and POST.** A decent number also use DELETE. PUT and PATCH exist, agents will emit them for full update features, and the POST-versus-PUT philosophy debate will rage on without requiring your participation. GET and POST are ninety percent of your reading life.

> One more clarification worth making, since we've been living in Express: REST is a _pattern_, not a package. It isn't a Node.js thing, it isn't a library you install — it's a way of shaping an API, and you can build REST APIs in any language on the map from earlier: Java, Python, Ruby, whatever your server speaks. And REST isn't the only pattern out there — you may hear names like RPC, SOAP, or GraphQL, which are different philosophies for how clients and servers should talk. They exist, they have their niches, and if a codebase you meet uses one, your assistant can orient you. (Except SOAP. Please don't do SOAP. Friends don't let friends do SOAP.) But for your purposes — and for the overwhelming majority of what agents generate — you'll want to stick with REST, and everything you read and build will speak it.

## Bruno: a nicer rehearsal space

You've been rehearsing APIs in the browser console, which works but gets cramped — especially for POSTs, where you're hand-typing `JSON.stringify` boilerplate every time. Time to meet the tool: **[Bruno][bruno]**, a free, open-source desktop app for making requests with a GUI. (It's the indie, local-first cousin of a tool called Postman you may hear about — Bruno keeps everything on your machine, no account required, which is exactly the energy this course endorses.)

Install it, make a new collection, and look at the request screen: a URL bar, a _dropdown of verbs_ — there's the lesson, rendered as a UI element — a body tab for JSON, and a Send button. Rehearse what you know: GET my pets API's `/pets/random`; POST to the words API's `/validate-word` with `{ "word": "BILLY" }` in the body (note how much nicer that was than the console version). Then do the instructive one: flip the dropdown to DELETE and send it at `/pets/random`. Read what comes back — a server that doesn't support a verb _tells you so_, with a status code and usually a message, and now you've personally confirmed that the verb only means what the server's code decided it means. Save the requests; a Bruno collection is a rehearsal space that remembers, and by the capstone you'll have a little library of every API you talk to.

> 🤖 **Ask your AI Assistant.** Design practice, because REST is also how _you'll_ spec backends: pick a resource from your own life — recipes, clients, tee times, decks — and ask "Design RESTful routes for managing [resource]. Just the method + path table with one line each, no code." Read the table it returns against the grammar: nouns plural? IDs in the path? Verbs conventional? Then the follow-up that connects it to your future: "Which of these would you actually build for a v1, and which are you including out of completeness?" Because knowing that a full REST table exists and knowing your app needs three of its rows — that's the difference between a spec and a wishlist.

## Recap

- REST is the grammar of nearly every API: URLs are nouns (`/pets`, `/pets/42`), methods are verbs, a request is a sentence — and the same URL means different things under different verbs.
- In the route table, that's same path, different method, different handler. `:id` is a placeholder, delivered as `req.params`.
- Conventions, not laws: nothing enforces the verb meanings — a server does what its handlers say. Expect the convention; verify in the code.
- Frequency reality: GET and POST are everywhere, DELETE is common enough, PUT and PATCH are the long tail.
- Bruno is your GUI rehearsal space — the verb dropdown makes the whole lesson tangible, and saved collections beat re-typing console fetches forever.

Next: the lesson this section has been building toward — why you can't put a secret in frontend code, and what servers do about it.

[bruno]: https://www.usebruno.com/
