---
description: "You've logged into a million sites; time your site could do that. Brian covers the two words hiding inside 'auth', why you never hand-roll it, Better Auth as the open-source pick, how logging in actually works, and the bouncer's hallway where it all runs."
---

You have logged into a million sites. Typed the email, typed the password, clicked the button, been welcomed by name — the single most familiar ritual on the internet. This lesson is about your apps being on the _other_ side of that ritual: letting a user log in and out of your site, remembering who they are between visits, and deciding what they're allowed to do — or, for APIs, making sure only callers holding a key get answered. It's also the last item on the list your assistant gave you after the API project: _who, exactly, is allowed to DELETE?_ Time to answer properly.

## Two words wearing one trench coat

"Auth" is actually two different jobs sharing a nickname, and telling them apart makes everything downstream clearer:

**Authentication** is verifying you are who you say you are. The login form, the password check, "sign in with Google" — all authentication. The question it answers: _who is this?_

**Authorization** is deciding, now that we know who you are, whether you can do the thing you're trying to do. The question: _is this person allowed?_

Reddit makes the split vivid. You can log into your account — that's authentication; Reddit now knows you're you. But you can only edit _your own_ posts — that's authorization; being logged in doesn't make everyone's posts yours. And roles live in the authorization half too: a subreddit's moderators and Reddit's admins can edit and remove things a normal user can't, not because they're _more authenticated_ — everyone logged in is equally authenticated — but because their role authorizes more. When this course says **auth**, we mean both jobs together, and here's a preview of why the distinction pays: the two have their own famous error codes. A **401** means authentication failed — "who even are you?" A **403** means authorization failed — "I know exactly who you are, and no." Two new entries for the catalog, and now you'll never confuse them.

> With this many status codes collected, it's worth a quick aside on the system behind them. Codes are organized by their first digit, and the first digit tells you the family: **2xx** means success (200, your old friend); **3xx** means "what you want moved — go over there" (redirects; the browser follows them without your involvement); **4xx** means _the asker_ did something wrong (404 asked for a thing that isn't there, 401 asked without identifying, 403 asked above their station, 405 asked with a verb the server doesn't serve); and **5xx** means _the server_ fell over — genuinely not your fault. Four families, one digit. From now on, any status code you've never seen sorts itself before you even look it up: 429? Starts with 4, so the asker did something wrong (asked too often, it turns out). You never meet a truly unfamiliar status code again — just unfamiliar members of families you know. (Mostly. There is also **418: I'm a teapot** — added to the official standard as an April Fools' joke in 1998, indicating the server is a teapot and therefore cannot brew coffee. It's real, servers really send it, and by family rules it means the _asker_ did something wrong, which — fair. You asked a teapot for coffee.)

## The rule: you don't build this

Auth looks easy. A users table, a password column, an if-statement — how hard could it be? Famously, catastrophically hard, is the answer. Passwords can never be _stored_, only mathematically scrambled in ways that resist very smart attackers with very fast computers. Sessions have to be unforgeable. There are edge cases stacked to the ceiling — password resets, account recovery, timing attacks, session theft — and every one of them has a graveyard of breached companies behind it. Which produces this course's firmest doctrine, and I want it stated in bold: **never let an agent hand-roll your auth.** An agent will _happily_ generate a users table with a password column and an if-statement, and it will look plausible, and it will be a liability with a login form on it. This is the one place in your stack where plausible-looking generated code is most dangerous, because the failures are invisible until they're catastrophic.

What you do instead is stand on work that's been attacked and hardened for years, and there are two good shapes of that:

**Rent it entirely.** Services like Clerk (Auth0 and others live here too) do auth _as a product_ — your app calls their API, they handle the users, the passwords, the login screens, the security patching. They're genuinely great, they're the maximum-outsourcing option, and for plenty of apps they're the right call.

**Build in a battle-tested library.** This is our pick: **[Better Auth][betterauth]**, an open-source library your agent builds _into_ your app. The crucial difference from the rented option: all the data — your users, their sessions — lives in _your_ database, right next to your other tables, rather than in someone else's service you reach over an API. Your users stay yours, there's no per-user bill, and everything is inspectable with the reading skills you already have. The trade is that it's in your codebase, which is exactly why the next two sections of this lesson exist.

## How logging in actually works

Recognition-level mechanics, because you're about to read this machinery in generated code. The puzzle auth solves is one you can now appreciate: HTTP has no memory. Every request arrives at your server as a total stranger — nothing about request #500 says it came from the same person as request #499. So how does "logged in" exist at all?

The wristband. Think of a venue: you show your ID once at the door, they check it carefully, and then they put a wristband on you — and for the rest of the night, nobody re-checks your ID; they glance at the wristband. Logging in works identically: you authenticate once (email, password, carefully verified), and the server issues a **session** — a long unguessable token, its wristband — which it also records in the database. The browser stores the token in a **cookie**, and here's the part that makes the whole system run: _the browser automatically presents that cookie with every subsequent request to that site._ Each request arrives wearing the wristband; the server glances at it, looks it up, and knows who you are — no re-authentication, no memory required of HTTP itself. Logging out is the venue cutting the wristband off: the server deletes the session, and the token in your cookie now points at nothing.

## Better Auth's two halves

Better Auth arrives in two portions, matching the two sides of every app you've built:

**The server portion** mounts into your backend and takes over a chunk of the route table — a family of routes under `/api/auth/` that handle sign-up, sign-in, sign-out, and session checks. And it keeps its records where you'd expect by now: **in your database, as tables** — a `user` table, a `session` table, and friends, created via migrations (there's that word, meaning exactly what you learned: it changed what the tables look like and wrote it down properly). Open the schema after the agent installs it and read the new arrivals with your schema-first habit; you'll find `.references()` lines stitching sessions to users — the wiring diagram, live.

**The client portion** is a set of helpers for your frontend — prebuilt functions and React hooks for the login form, the sign-up flow, the log-out button, and the ever-present question "who's logged in right now?" (a hook shaped like `useSession()`, which you can read: a hook, of the session). You'll still prompt your agent to build the actual screens, but these helpers mean the screens are thin UI over hardened machinery rather than fresh inventions — which is the entire point.

## The bouncer's hallway

Last piece, and it's the payoff of a plant from the API project: _middleware — the hallway before the rooms — and auth lives in that hallway._ Protecting a route means putting a check in front of its handler, and in generated code it reads like this:

```javascript
app.delete("/pets/:id", requireUser, async (req, res) => {
  // by the time we're here, req.user exists — the bouncer let them in
});
```

That `requireUser` sitting between the path and the handler is middleware: a function every request to this route must pass through first. Inside it lives a shape you've known since the control-flow lesson — a guard clause, now wearing a badge: check the wristband; no valid session, respond `401` and stop; valid session, attach the user to the request (`req.user` — which is how handlers everywhere know who's asking) and wave them through. Authorization is the same hallway, one door deeper: _does this pet belong to `req.user`? No? `403`._ When you read a generated backend from now on, the route table tells you what the API offers, and the middleware between path and handler tells you **who's allowed to ask** — the second column of the table of contents.

(And the API-key flavor from the intro is this exact hallway with a different wristband: instead of a session cookie, the bouncer checks for a key in a header — the very kind of key you learned to keep server-side. Humans get sessions; programs get keys; the hallway treats both the same way.)

> 🤖 **Ask your AI Assistant.** The trace, one level up: ask "Walk me through everything that happens, step by step, between a user clicking 'Log In' and seeing their dashboard — client, server, and database." Grade it against the wristband: credentials verified once, session created and stored, cookie issued, cookie auto-presented, middleware glancing at wristbands ever after. If any step comes back vague, push — "where exactly is the session stored?" is a great follow-up. Then the question whose answer explains half of modern security news: "Why can't passwords be stored as-is in the database, and what happens if a users table leaks?" You won't be implementing the answer — Better Auth already did — but understanding it is what makes the never-hand-roll doctrine feel like wisdom instead of superstition.

## Recap

- Auth is two jobs in a trench coat: authentication (_who is this?_ — the login) and authorization (_are they allowed?_ — your posts, not everyone's; roles like Reddit mods live here too). 401 means authentication failed; 403 means authorization failed.
- **Never let an agent hand-roll auth.** It will happily generate something plausible-looking, and plausible-looking is the danger. Stand on hardened work: rent it entirely (Clerk and friends) or build in a battle-tested open-source library.
- Our pick is Better Auth: open source, built into your app by your agent, with all user and session data living in _your_ database rather than someone else's service.
- Logging in is the wristband: authenticate once, get a session token in a cookie, the browser presents it automatically forever after, the server glances and knows. Logging out cuts the wristband.
- Better Auth has a server half (routes under `/api/auth/`, its own tables via migrations) and a client half (helpers and hooks for the login/logout UI).
- Middleware is the bouncer's hallway: `requireUser` between path and handler, `req.user` for whoever got through, 401 at the door, 403 one room deeper. Route table = what the API offers; middleware = who's allowed to ask.

Next: where all of this — app, database, secrets, sessions — actually _lives_ when real people use it.

[betterauth]: https://www.better-auth.com/
