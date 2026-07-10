---
description: "The destination lesson: a 1,500-line full-stack codebase you've never seen, read end to end. Brian draws the full where-code-runs map, formalizes the five-stop walk order, and tours the Family Recipe Box — strangers, joins, and all."
---

Every reading skill in this course has been building to this lesson. Clone the repo — [btholt/family-recipe-box-sample-app][repo] — because we're going to walk the whole thing: a complete full-stack application, about fifteen hundred lines across thirty-five files, that you have never seen before. Full disclosure of provenance, because it's instructive: I didn't hand-write this app — I wrote a _spec_ and an agent built it, which makes it precisely the kind of codebase you'll be inheriting for the rest of your building life. It's a recipe box: sign up, post recipes, edit your own, favorite anyone's, search. You know every organ in it. You've just never seen this body.

## The whole map, drawn once

Before the walk, the synthesis this course has been assembling piece by piece — because this one repo contains _every place code runs_, and you can now point at each:

**The browser** — `src/`: the React app, downloaded by every visitor, running on their machines. **The build step** — `vite.config.ts` and the `build` script: the translator that turns `src/`'s TypeScript-and-JSX into the plain files browsers speak. **The server** — `server/`: the Express app, running forever on a machine you control, never shipped to anyone, holding the secrets and the authority. **The database** — `drizzle/` (the migrations — the shape's version history) plus a rented Postgres somewhere, holding the memory. One repo, four worlds, connected by fetches and a connection string — and every app you will ever prompt into existence is some arrangement of exactly these four. That's the map. Now let's walk the territory.

## The walk order

Here's the sequence, formalized, and it's the single most reusable artifact of this lesson — the same five stops work on this repo, on your capstone, and on the quarter-million-line codebase at work:

**Stop 1: the README** — what is this and how do I run it? Ours answers both in a screenful, including the setup ritual you know cold: rent a Postgres, `.env` the connection string, install, migrate, seed, run. File that away — we're deliberately walking the code _cold_ first and bringing it to life at the end, because reading-before-running is the order you'll want in codebases too big to run casually. And notice the README's neighbor, `CLAUDE.md`: that's an _agent instruction file_ — in this case, literally the spec I fed the agent, committed to the repo. Modern codebases increasingly carry these; they're documentation _for your assistant_, they're readable provenance, and finding one means you can see what the code was _asked_ to be. Read it after the walk and grade the agent's homework.

**Stop 2: `package.json`** — the ID card, and today's has two new words worth decoding. The `dev` script uses `concurrently` to run _two_ programs at once — `dev:server` and `dev:client`, color-coded blue and green in your terminal — because development mode is genuinely two servers: Vite serving the frontend with hot reload, Express answering `/api` (peek at `vite.config.ts` and find the _proxy_ — the line that makes the frontend's `/api` fetches quietly forward to Express at port 3001). And `tsx` is the tool that runs TypeScript directly without a build step — dev convenience; the real build still happens for production. The rest you know: `seed`, `db:migrate`, `db:studio`, and a `start` that flips on production mode.

**Stop 3: the schema** — `server/schema.ts`, the app's nouns, and the file's own banner comment says what you'd have worked out: two groups. Better Auth's four tables (`user`, `session`, `account`, `verification` — "we don't read or write these ourselves"), then ours: `recipes`, with an `authorId` wearing a `.references()` to `user` — there's the wiring — and `favorites`, which is a pure _relationship table_: a `userId`, a `recipeId`, and almost nothing else, each row a thread between a person and a dish. One detail worth savoring: the `unique().on(table.userId, table.recipeId)` constraint at the bottom — that's the _database itself_ enforcing "you can't favorite the same recipe twice." A business rule, expressed as schema, impossible to violate even by buggy code. Ten seconds in this file and you know what the entire app is about.

**Stop 4: the route tables, plus the hallway** — `server/index.ts` first, because it's the spine, and reading it top to bottom rewards you four times: the `requestLogger` middleware you were promised (every request prints method, path, status — free flight recorder); a genuinely important _ordering_ comment — Better Auth's routes are mounted **before** `express.json()`, because Better Auth insists on reading request bodies itself, and this is exactly the kind of load-bearing line whose comment saves a future debugging afternoon; the two routers mounted at `/api/recipes` and `/api/favorites`; and at the bottom, the production block — `express.static` serving `dist` (the one-roof arrangement from the hosting lesson, in the flesh) plus a catch-all that returns `index.html` for any unhandled route. Hold that catch-all in mind; it's about to make sense.

Then the routers, read with the two-column habit: **what does it offer, and who may ask?** In `recipes.ts`: the GETs are public — anyone browses, and the list route reads `req.query.q` into an `ilike` for search (query params and stilted-English SQL, together at last) — while POST, PUT, and DELETE wear `requireAuth`. And inside PUT and DELETE, the second door: fetch the recipe, then `if (existing.authorId !== req.user!.id) return 403` — _authorization_, explicit and readable, exactly where the auth lesson said it would live. In `favorites.ts`, everything requires login, POST uses `.onConflictDoNothing()` (a graceful shrug when you double-favorite — the unique constraint's polite handler), and then, the headliner:

```typescript
const rows = await db
  .select({ id: recipes.id, title: recipes.title, ... authorName: user.name })
  .from(favorites)
  .innerJoin(recipes, eq(favorites.recipeId, recipes.id))
  .innerJoin(user, eq(recipes.authorId, user.id))
  .where(eq(favorites.userId, req.user!.id));
```

Your first **join**, and you can read it: start _from_ the favorites table, stitch each row to its recipe (matching `recipeId` to the recipe's `id`), stitch each recipe to its author (for the name), keep only _this_ user's rows. Three tables holding hands, exactly as the relations note promised, producing the My Favorites page in one query. The `select({...})` on top is just shaping — choosing which fields come back, an object shape you can read. Joins have a reputation; this one took a paragraph.

**Stop 5: one trace, end to end.** The ritual, on foreign soil. Click a heart on a recipe card: `RecipeCard.tsx`'s `onClick` → a function in `src/api.ts` (note the app centralizes its fetches in one file — a pattern worth stealing) → `POST /api/favorites/:recipeId` → across the proxy → `requestLogger` notes it → `requireAuth` glances at the wristband → `req.user` exists → `insert ... onConflictDoNothing` → 201 → the heart fills via a state setter. You've run this trace on your own apps; running it on a stranger's and having it _hold_ is the point of the entire course.

## Meeting the strangers

A real codebase always contains something you weren't taught, and this one has a deliberate one: open `src/App.tsx` and meet **react-router** — `<Route path="/recipes/:id" element={<RecipePage />} />`, a library this course never covered. Now watch the protocol for meeting the unknown, because the protocol is the skill: **first, read the shape before reaching for help.** A stack of `<Route>` elements mapping paths to components... that's a _route table_ — the backend's table of contents has a frontend sibling, mapping URLs to screens instead of URLs to handlers. You just read an untaught library by recognizing its shape. Second, _ask your assistant_ to fill in the mechanics ("what does react-router do and how do these Route elements work?"). Third, notice the payoff cascade: client-side routing is why the server needs that catch-all — refresh the page on `/recipes/3` and the _server_ gets asked for a URL only the _frontend_ knows; the catch-all hands back `index.html` so React can wake up and take it from there. A stranger, a shape-read, one question, and a mystery from stop four resolved. That's the whole method.

(Two smaller strangers, thirty seconds each: the `!` in `req.user!.id` is TypeScript's "trust me, it's there" — the developer overriding the proofreader's caution, safe here because `requireAuth` guarantees it; and `tsx`/`concurrently` you met at stop two. Log them in your notes as met.)

## Bring it to life

You've read a codebase you've never run — now run a codebase you've only read, because that is its own deeply instructive act. Go get a fresh free Postgres from Render (the ritual, one more time — or savor the fact that you could hand even _that_ off soon), then give the agent the whole job: _"Here's my database connection string. Get this project running locally."_ Then watch it work through the universal onboarding recipe on your behalf — `.env`, install, migrate, seed, dev — reading the terminal as it goes, because you can. When the `concurrently` output starts streaming blue and green, hit **localhost:5173**.

And now the payoff round: use the app _as someone who has read it_. Sign up, and know which four tables just gained rows. Post a recipe, and picture the guard clauses your request just walked past. Favorite something and refresh My Favorites, and see the three-table join render as a webpage. Search for "soup" and know there's an `ilike` doing that. All the while, the request logger is scrolling in the terminal — every click you make, printed as method-path-status, the flight recorder narrating your session in a dialect you speak. There is a specific, quiet thrill in software holding zero mysteries, and this is it. (And if something _doesn't_ come up cleanly — a migration hiccup, a port conflict, a missing env var — genuinely, good: reading an error on unfamiliar-but-understood code is a preview of the entire next act of this course, and you have every tool the situation requires.)

> 🤖 **Ask your AI Assistant.** The docent protocol, which turns any codebase into a guided museum: with the repo open, ask "Give me a tour of this codebase — what are the major pieces and how do they talk to each other?" and grade it against the walk you just did (you now know when a tour guide is being vague). Then the pointed follow-ups you'll use forever: "Where exactly is the ownership check for deleting a recipe?" — verify by reading the line it cites — and the deep one: "Walk me through what happens, in this code, when a user refreshes the page while on /recipes/3 in production." If you can follow its answer through the catch-all, the static serving, and React re-mounting the route, you haven't just read a codebase — you've understood one, which was the destination all along.

## Recap

- The full map: browser (`src/`), build step (`vite.config.ts` → `dist`), server (`server/`), database (`drizzle/` + rented Postgres). Every app is these four worlds; every repo is some arrangement of them.
- The walk order, reusable forever: **README → package.json scripts → schema → route tables (with the who-may-ask column) → one full trace.**
- Agent instruction files (`CLAUDE.md` and kin) are documentation for your assistant and provenance for you.
- The join reads as stitching: from one table, innerJoin the next on the matching ids, keep only whose rows you want. Three tables holding hands is a paragraph, not a monster.
- Meeting the unknown: read the shape first (react-router's Routes are a route table for screens), ask the assistant second, log it as met. Strangers are the normal condition of real codebases; the protocol makes them brief.
- Running a stranger's codebase is its own lesson: rent the database, hand the agent the connection string and the job, read the terminal as it onboards, then use the app _as someone who has read it_ — every click a trace you already know.

Next: the codebase stops being a museum. You're going to change it.

[repo]: https://github.com/btholt/family-recipe-box-sample-app
