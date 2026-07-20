---
description: "You shipped. Now: ways to grow this app (domains, AI inference, real users), full permission to stop right here, and the paths forward if you don't want to — plus one last request from Brian."
---

Take stock for a second, because the inventory is worth reading out loud. You started this course unable to read a `console.log`. You are ending it as someone who specs applications, directs an agent through milestone builds, reads every line it writes, debugs from a user's complaint, guards a codebase's consistency, provisions infrastructure through tools, and ships working software to a URL with your name on the repo. That happened. Recently. To you.

This last lesson is two things: a menu of ways to keep going, and a proper goodbye.

## Ways to grow this app

Your capstone has a backlog and a maintainer — here's what I'd put on the backlog, roughly in order of joy-per-effort:

**A custom domain.** Right now your app lives at a `.onrender.com` address; twelve-ish dollars a year makes it live at a name you chose. Buy the domain from a registrar — Porkbun, Hover, and Cloudflare are three I've used and recommend — and connect it in Render's dashboard (or, you know, ask your agent how). There is a disproportionate psychological upgrade in your app having its own name. Cheapest magic on this list.

**Real users.** Send the URL to three people who'd actually use the thing, and ask them to file GitHub Issues when something confuses or breaks. Nothing — not this course, not any course — teaches like a real user's backlog, and you are now fully equipped to work one.

**AI inference.** Remember the secrets lesson, where I called OpenAI from a server on camera and you watched a key stay hidden? That pattern is now _yours to build_ — a thin server route holding the key, and suddenly your app summarizes, suggests, or chats. Claude, OpenAI, and Gemini are all great; OpenRouter is a fun one that lets you switch among all of them behind a single key. The serious caveat, in bold because it's real: **this is not free, and it is easy to rack up costs** — set a spending cap in the provider's dashboard _before_ the key goes in your `.env`, keep the key server-side always, and treat "the agent added a feature that calls the API in a loop" as a thing your diff review exists to catch.

**More APIs, generally.** Weather, sports scores, holidays, air quality — the world is full of free and cheap data faucets, and you know the whole pattern: rehearse in Bruno, wire the fetch, render the shape.

**Design.** Your app works; making it _beautiful_ is its own rewarding rabbit hole. Tools like Figma (design the screens, then hand the agent screenshots of what you want) and Claude Design (describe it, get designs back) turn "I wish it looked better" into an iteration loop rather than a wish.

**Issues + the GitHub MCP, as a workflow.** Here's a genuinely great way to run a project long-term: keep filing your ideas and bugs as GitHub Issues, and then start work sessions with _"let's fix issue #7."_ The agent reads the issue through its GitHub tools, you refine the plan, the PR says "Fixes #7," the backlog burns down. It's TASKS.md in the cloud — persistent, shareable, and exactly how real open-source projects run.

**Other tools.** You built this course's muscle in VS Code + Copilot, but the muscle is portable — that was always the point. Go back to Replit with your new eyes (it'll feel completely different when you can read what it's doing), try Cursor, try Antigravity. Different tools for different problems; you're qualified to have opinions now.

## Or: stop right here

Real talk, because it deserves saying plainly: **it is completely legitimate to stop at this point and say "I can vibe code now."** You can. You have the skills to build genuinely cool projects — tools for your job, apps for your family, ideas that have been waiting in your head for a builder — and doing exactly that, on your own schedule, with no further curriculum, is a full and honorable version of where this ends. Not every reader needs to become a deeper technician. Go build your things.

## Or: keep growing

If you felt the pull, though — if some lesson made you want to know what's _under_ the thing under the thing — then Master.dev is your best friend, and [holt.fyi/beginner][beginner] is the path built for exactly where you're standing. The next course on it covers similar ground to this one but goes much deeper, and here's the twist: **it uses no AI at all** — you craft every line of HTML, CSS, and JavaScript by hand. That's not a step backward; it's this course's own first-principles logic applied to itself: everything you build by hand makes you sharper at directing the intern who builds it for you. And good news — it's still in Master.dev's free content.

Some others to tackle soon, each for a specific reason: **[Linux and the CLI][linux]** — you're going to be reading a lot of Linux and command-line output for as long as you do this, because that's simply how dev works, and this course makes that terminal feel like home. **[React][react]** — if the JavaScript has felt comfortable, this goes much deeper into React, which is the language agents speak when they build frontends these days. **[Coding with AI][aicode]** — a whole path on the many ways to build with AI, from instructors I'd put my name next to.

And the other axis: **start a new project, and change one variable.** We did JavaScript and TypeScript — try Python. We deployed to Render — try Azure or AWS, or Vercel, Netlify, even GitHub Pages. We used Postgres — try MySQL, SQL Server, or go document-flavored with MongoDB ([holt.fyi/db][db] will be your friend there too). Each swap teaches you which of your skills were about the tool and which were about _building_ — spoiler: mostly the second — and practice makes perfect in the most literal way: you'll get more ambitious precisely as you learn what's possible. One more thing we never got to, worth exploring when you're ready: **CI/CD with GitHub Actions** — the setup where your tests, lint, and types run automatically on every commit _in the cloud_, and what's on GitHub auto-deploys when checks pass. It's the milestone rhythm, fully automated: push code, machines verify, software ships. You'll recognize every piece of it.

> 🤖 **Ask your AI Assistant.** One last beat, in the tradition: "Read this repo — the README, the code, the closed issues — and draft a short post announcing what I built and what I learned building it. Write it warm and specific, not braggy." Then do what you've done all course: edit it for truth, because you're the only one who knows which parts were hard. You're going to want that post for the next part.

## One last thing

Share what you've built. Genuinely — tell me, tell Master.dev, post the URL and the repo. Partly because sharing is the last skill of shipping, partly because seeing what people build from this course is, for the people who made it, the entire reward. This was a _lot_ to take in — twelve sections, four worlds, an intern, a bestiary of errors, a shipped application — and you got all the way here. Amazing job.

Now go build something only you could have thought of. And when it breaks — read the complaint, walk the trail, and stay calm. You know how.

[beginner]: https://holt.fyi/beginner
[linux]: https://holt.fyi/linux
[react]: https://holt.fyi/react
[aicode]: https://holt.fyi/ai-code
[db]: https://holt.fyi/db
