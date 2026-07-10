---
title: "MCP"
description: "Your assistant has been words-in, code-out this whole course. MCP gives it hands and eyes — the ability to actually check your GitHub, read your server logs, and operate your infrastructure. Brian covers what it is, how to wire it up, and the one thought experiment that keeps it safe."
---

Everything your AI assistant has done in this course, it has done with words. You paste code in, it explains; you describe an app, it generates. Even in agentic mode, where it edits files and runs commands, it's been confined to your project folder. It cannot _check_ your GitHub. It cannot _see_ your Render dashboard. It cannot read your production logs, provision a database, or find out whether your deploy actually worked. For all of that, you've been the go-between — copying error messages in, clicking dashboards, relaying between the assistant and the rest of your world.

**MCP** ends the go-between era. It stands for Model Context Protocol, and it's a standard way to plug _tools_ into your assistant — giving it hands and eyes on the actual systems you use. And here's the demystification, which requires no new concepts whatsoever, because you already learned this one in the API lesson: an API is _a server somewhere exposing a function for us to call remotely_. Well — **an MCP server is exactly that, except the caller is your assistant instead of your code.** A server, exposing functions ("tools," in MCP vocabulary), each with a name and a description; your assistant reads the descriptions, and when a conversation calls for one, it calls it. That's the entire idea. People wrap MCP in a great deal of mystique, and having built several, I can report: it is a server with a menu. The mystique is silly.

## The cast, and the flow

Three words cover the vocabulary. The **server** is the tool provider — GitHub publishes one, Render publishes one, thousands exist. The **tools** are the functions each server offers — `list_services`, `get_logs`, `create_pull_request` — each with a description your assistant can read like documentation. The **client** is your assistant itself — VS Code with Copilot, in our case — which connects to servers and gains their tools.

The flow, when it works, feels like this: you ask a normal question ("did my deploy succeed?"), the assistant recognizes that one of its tools could actually _answer_ that rather than guess, it asks **your permission** to call it, you approve, the tool runs, and the answer comes back grounded in reality instead of vibes. That permission step is not ceremony — hold that thought for the end of the lesson, because it's the whole safety model.

## Why tools, though?

Fair question: agents can already run commands and write code — why hand them predefined tools at all? The answer is _repeatability_. Some actions are done the same way every single time: you file a GitHub issue the same way, you create a Render service the same way. When an action is that regular, it's better _defined in code once_ than re-derived by an agent every time — the agent calls the tool with the proper inputs and gets a **guaranteed-correct outcome**, instead of you relying on it to figure out the steps correctly on this particular Tuesday. It's the difference between handing your intern a laminated procedure and asking them to improvise the procedure from memory each time. Enthusiastic improvisers, remember.

Where this earns its keep most is operations with _cascading steps in a required order_. Take a database migration — a thing you know from the databases lesson: to do it right you define the change, test it, iterate until it's correct, and only then output a proper migration file, in that order, no skipping. That's a sequence with several places to fumble, and an agent freestyling it will occasionally fumble one. A migration _tool_ performs the same choreography identically every time. Multi-step, order-sensitive, consequences-if-wrong: that's the profile of a great tool.

And an honest calibration, because this course doesn't sell you weather: as models improve, they're getting genuinely better at nailing these multi-step procedures unaided — which is why the ecosystem is actually seeing _fewer_ MCP servers and tools over time, not more. The ones that endure are the ones with real substance behind them: first-party access to a platform's own machinery, like GitHub's and Render's — which, not coincidentally, are exactly the two we're installing.

## Wiring it up

Watch-me with follow-along strongly encouraged, because you'll want these for the rest of the course. VS Code has first-class MCP support: you add servers to its MCP configuration (the Command Palette's "MCP: Add Server" flow, or an `mcp.json` — your assistant can walk you through either, which is pleasantly self-referential), authenticate to the service, and the tools simply appear in your agent's repertoire. We're installing two:

**The GitHub MCP server.** Your repos, issues, and pull requests become promptable. First round-trip, live: _"List my repositories."_ Then: _"What did I change in the last three commits to my pet tinder repo?"_ — and watch it actually go look, instead of guessing from whatever's open in the editor. The next lesson leans on this one hard.

**The Render MCP server.** Your infrastructure becomes promptable. Round-trip: _"What services do I have on Render, and what state are they in?"_ Then the one that changes your debugging life: _"Show me the recent logs from my Postgres database."_ The dashboard you clicked through to provision that database is now a conversation — and here's the promise this sets up: when the capstone comes, the agent will do the provisioning _itself_. You rented a database by hand, in the browser, on purpose, exactly once — and this is why. Now when the agent says "I've created the database and set the environment variable," you know precisely what it did, because you've personally done it. Delegation with comprehension: the whole course, in one moment that's coming shortly.

Two installed servers is the right number today. Every MCP server you add puts more tool descriptions in front of your assistant on every request — more to consider, more to get confused by. Add tools when a task calls for them, not for collection's sake.

## The part you must not skim

MCP tools act **with your permissions**. The GitHub server acts as _you_ on GitHub; the Render server acts as _you_ on Render — including the parts of Render where creating things **costs money**. Which is why this section exists, and why it borrows the most important idea from my MCP course: the **Paperclip Golden Retriever**. The classic thought experiment imagines an AI told to maximize paperclips that pursues the goal to absurd, destructive lengths — and the domesticated version is your reality: agents will do _exactly_ what you tell them, with golden-retriever enthusiasm, right past trade-offs you never intended. An agent with Render tools, told vaguely to "make the app faster," might scale up a paid service. Told to "clean up," it might delete a database. Not malice — _fetch_.

The defenses are habits you mostly already have, now with real stakes:

- **The permission prompt is a diff review.** When the assistant asks to call a tool, read _which_ tool with _what_ arguments — the read-versus-write question takes two seconds: `get_logs` is a glance; `create_service` and `delete_database` are commitments. Never auto-approve write tools. The approval click is your bouncer's hallway, and you're the bouncer.
- **Be specific, then verify** — the prompting discipline you've practiced all course, applied to actions: constrain the blast radius ("read the logs; don't change anything") and check the result afterward, in the dashboard if money's involved.
- **Vet your servers.** An MCP server is code acting on your behalf, so its provenance matters exactly like a dependency's — you install `react` comfortably because you trust who publishes it, and the same logic applies here. Ours are the official GitHub and Render servers: trusted publishers, first-party access to their own platforms. A random third-party server that wants your credentials deserves the same squint as a package with a suspicious name. When better verification tooling matures — it's coming — this gets easier; until then, official servers and healthy suspicion.

> 🤖 **Ask your AI Assistant.** The capability audit, which you should rerun any time your tool loadout changes: with both servers installed, ask "List every tool you currently have available, and classify each one: does it only read information, or can it change or create things? Flag anything that could cost money or delete data." Read the inventory — it's the menu of what your assistant can now _do_, sorted by stakes — and notice you've just applied the schema-first habit to your own agent: before working with a system, read what it's capable of. Then take the new hands for a spin on something harmless: "Using your tools, tell me the status of everything I have deployed and anything in the logs that looks unhappy."

This lesson is deliberately the _user's_ tour — enough to wield MCP for the rest of this course. The other side, building MCP servers of your own (they're genuinely simple — a server with a menu, remember), is its own satisfying rabbit hole, and my [Complete Intro to MCP][mcp] is that rabbit hole, properly excavated.

## Recap

- MCP plugs tools into your assistant — hands and eyes on your actual systems. An MCP server is a thing you already understand: a server exposing functions to call remotely, except the caller is your assistant.
- The cast: servers provide tools, tools are described functions, your assistant is the client that calls them — with your permission, every time.
- Tools exist for repeatability: actions done the same way every time (filing an issue, running a migration) are better defined in code once than improvised by the agent each time — guaranteed-correct outcomes, especially for multi-step, order-sensitive operations. Models are getting better at these unaided, which is why the tool ecosystem is consolidating around the substantial ones — like the two we installed.
- GitHub MCP makes repos, issues, and PRs promptable; Render MCP makes your infrastructure promptable — including provisioning, which the agent will do itself at the capstone, comprehensibly, because you did it by hand first.
- Tools act with _your_ permissions, including the expensive ones. The Paperclip Golden Retriever will fetch exactly what you asked for, so: read every permission prompt like a diff, never auto-approve writes, constrain then verify, and only install servers whose publishers you'd trust with the keys — because that's literally what you're doing.

Next: GitHub, for real this time — push, branches, and pull requests, which turn out to be the native language of working with agents.

[mcp]: https://holt.fyi/mcp
