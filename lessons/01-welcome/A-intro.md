# Beyond Vibe Coding — Workshop Curriculum Outline

_Working draft for Brian + Brandon handshake and FEM coordination. Topic-first; day mapping to follow once content is locked._

---

## Premise & audience

A 5-day bootcamp partnership between Frontend Masters and Replit. The student walks in tech-enabled but not coding — adjacent to engineering (law, medicine, accounting, business, data) and likely has poked at Lovable, v0, or Replit and gotten stuck the first time an agent couldn't immediately solve their problem.

The arc takes them from "I want to vibe code in Replit" to "I can read code, work alongside AI in a real editor, supervise an agent with intent, and ship something I'm proud of." The course is a clean on-ramp to deeper Frontend Masters material — not a substitute for it.

**Brandon (Replit, head of education) takes day 1 in full and owns the complete Replit-native AI app-building arc** — ship something, hit a wall, debug and prompt better, ship something better. Students leave day 1 with a deployed Replit app they care about.

**Brian takes days 2–5** and owns the engineering on-ramp — code literacy, AI-assisted coding in VS Code, technical prompting, code-aware debugging, and a capstone where students build something new with the full toolkit.

The course will be recorded and posted free as marketing for both companies. Content is concept-heavy and tool-light wherever possible — the goal is a 5+ year shelf life.

Replit is providing students free premium accounts for one year. Brian's portion uses VS Code + GitHub Copilot + Copilot CLI as the primary toolchain (generous free tiers), with Cursor, Gemini CLI, Claude Code, and Codex covered as alternatives. No student should be required to spend money to take the course.

---

## Narrative arc

1. **Brandon, day 1.** Ship something cool with Replit. Hit a wall. Learn to debug and prompt better. Ship something cooler. The complete Replit experience, end to end.
2. **Brian, day 2.** Bridge — let's look at what code actually says. Survey HTML, CSS, JS, TS just enough to read what an agent ships.
3. **Brian, day 3.** AI-assisted coding in a real editor. VS Code, Copilot, Copilot CLI, the wider landscape. Git and GitHub. Walking a real vibe-coded codebase.
4. **Brian, day 4.** Now that you can read code, prompt and debug like it. Technical prompts, code-aware debugging, databases through code, secrets and APIs done with intent.
5. **Brian, day 5.** Capstone — build something new, end to end, with the full toolkit.

The arc says: Replit gives you one capable environment; coding alongside AI in a local editor gives you another. Knowing which to reach for, and being able to read what either produces, is the actual skill.

---

## Threading concepts (recurring throughout)

- **"The world's fastest intern" mental model.** Established by Brandon on day 1. Brian reinforces throughout — same intern, more languages spoken to it.
- **Different tools for different shapes of problems.** Not a graduation from Replit — students leave with both Replit and a local coding toolkit, and the real skill is knowing which to reach for. Brian uses Replit for plenty of work he could code from scratch; the choice is about fit, not capability. Anecdote material for live delivery.
- **Code-reading exercises distributed.** Three flavors threaded through the reading modules: explain this function in plain English, find the bug, do you trust this diff?
- **Tool landscape, principles first.** Brian primarily teaches VS Code + Copilot + Copilot CLI. Cursor, Gemini CLI, Claude Code, Codex covered as alternatives. Students use what they want or what they're already paying for. Principles transfer.

---

## Brandon's day — outcomes Brian needs

Brandon owns day 1 in full. His current draft ("Impactful Apps with Replit Agent") covers Replit workspace orientation, a quick-win Design Canvas build, MVP scoping (self → friends/coworkers → world), prompting with context, agent workflow (rapid iteration vs. hands-off barbell model), debugging (Reproduce → Isolate → Verify), data persistence, deployment, and maintenance.

For Brian's portion to land cleanly, day 1 needs to produce:

- Every student has a deployed Replit app they personally care about
- Students are fluent in the Replit workspace, agent flows, and checkpoints
- Students have a working mental model for the agent and a starting vocabulary for prompting
- Students have at least felt agents hitting walls — Brandon names this honestly
- Bridge handed off: "Brian is going to teach you what's actually inside the apps you just built"

Outstanding handshake items with Brandon listed in the Decisions section below.

---

## Daily homework cadence

Brian's portion is half-to-two-thirds days, leaving real time in the evening. Each day ends with a prompt:

- **After Day 2 (Foundations).** A short code-reading exercise on a fresh small snippet. Reps for the skill they just learned, not their own project, not a canned project — just "read this, explain it."
- **After Day 3 (Tooling).** The marquee homework: pull your Day 1 Replit app down, open it in VS Code, push it to a fresh GitHub repo, add a small feature with Copilot. Concrete proof that "your code is yours, it's just code, it moves." Lands the moment they have all the tools to do it.
- **After Day 4 (Prompting & debugging with code knowledge).** A deliberately broken small repo to clone, diagnose, and fix using code-aware debugging. Push the fix to GitHub. This is the debugging payoff that the dropped "three states of one project" callback used to provide.
- **Day 5 has no homework** — the capstone is the deliverable.

The Replit-to-VS-Code move on Day 3 is the load-bearing one; the others reinforce the day's skill in lower-stakes contexts.

---

## Module list — Brian's days

Roughly 16–20 instructional hours across 4 half-to-two-thirds days.

### Section 1 — Bridge & code foundations

#### F1 — The Bridge

Open with one of the apps students shipped yesterday. Open a file. Read it together. "This is what code actually says. By the end of the week you'll know what most of these lines mean and how to change them." Sets the table for the rest of the course.

#### F2 — JavaScript in the Browser vs JavaScript on the Server

The thing that confuses everyone learning Node. What runs where, why a `console.log` shows up in the terminal sometimes and the browser sometimes, why the agent generates two flavors of file.

#### F3 — HTML & CSS Survey

Reading-oriented. DOM, tags, attributes, selectors, what Tailwind classes compile to. Light and fast. Pointer to Complete Intro to Web Dev v3 for fluency.

#### F4 — JavaScript Survey for Reading

Variables, functions, control flow, async/await at a recognition level. Modules and imports — what `import` lines tell you about a file's dependencies. **Trace data flow** as the primary skill, not write fluency. This is the segment that earns the course's promise.

#### F5 — The TypeScript Elephant

15 minutes. "TS is JS with type annotations you can mostly ignore while reading." Read past the colons. Note when types are actually load-bearing (return types of functions you're calling).

#### F6 — Code Reading Workshop (distributed)

Practiced exercises threaded through F3–F5, not a standalone block:

- **Explain this:** read a function, write what it does in plain English
- **Find the bug:** here's broken code, locate the problem
- **Do you trust this diff?** the agent proposed this change; should you accept it?

### Section 2 — AI-assisted coding in a real editor

#### T1 — VS Code Tour & Setup

Install, layout, extensions, command palette, integrated terminal, source control panel. Replit's web IDE is one capable environment; here's another with a different shape, and why having both in their toolkit matters.

#### T2 — GitHub Copilot & Copilot CLI

Primary tools for Brian's portion. Inline suggestions, Copilot chat, Copilot CLI for terminal-side agent work. Free tier on both. Practical: when each is the right reach.

#### T3 — The AI Coding Tool Landscape

"You're not married to Microsoft tools." Honest tour of Cursor, Gemini CLI, Claude Code, Codex. Different shapes of problem suit different tools. Brian shares what he uses and when. Students use whatever they already have or pay for. Principles transfer.

#### T4 — Git & GitHub

"When the agent breaks everything, how do you go back?" Conceptual `git status` / `git commit` / branches / push / pull / PRs. Recognition, not fluency. Students leave with a GitHub profile and at least one repo of their own. Load-bearing for their coding life going forward.

#### T5 — Walking a Real Vibe-Coded Codebase

Clone a real public repo that's been vibe-coded into chaos. Show students how to navigate it, read it, and tame it with AI assistance. Two honest failures on display: AI assistant gets lost without context, student gets lost navigating. Motivates section 3.

### Section 3 — Better prompting & debugging with code knowledge

#### P1 — Technical Prompting: Speaking the Language

Now that students know words like Next.js, Tailwind, Drizzle, useState, async, they can prompt with intent. The same idea prompted casually vs prompted technically — show the delta. Specifying tests, design system, ORM, error handling becomes natural.

#### P2 — Code-Aware Debugging

Read the error message first. Read the actual code path. Check the console, network tab, terminal. **What context to feed back to the LLM — separating signal from noise**, now informed by being able to read the code yourself. When to revert, when to push forward. Arguably the highest-leverage segment in the week.

#### P3 — Databases Through Code

"A database is a spreadsheet you can ask questions of with SQL." Now extend: what an ORM is (Drizzle, Prisma), what a migration is, what a schema file says. Open the DB, run a SELECT, but also trace the data from the schema file to the API call to the UI. Pointer to Complete Intro to Databases v2 for depth.

#### P4 — Secrets, APIs, and Environment in VS Code

`.env` files, `.gitignore`, what env vars are at the OS level vs framework level. Why you don't commit keys. Stubbing out APIs you don't want to pay for. The same workarounds students used in Replit, now done with intent and visible to them.

### Section 4 — Capstone

#### C1 — Plan Something New

Start fresh. Apply the full prompting workflow with all the new vocabulary. Write a spec with technical opinions. Students use whatever AI coding tool they're most comfortable with.

#### C2 — Build It

Brian roams, helping individuals get unstuck. VS Code + Copilot is the default; alternatives welcome.

#### C3 — Deploy It

Deploy to Netlify. Free tier, no credit card, and Netlify DB ships managed Postgres so students don't have to wire up a separate database provider — keeps the pedagogy clean right after the database module. Connect the GitHub repo from T4, push, deploy. Mention Vercel as an equivalent alternative students can choose later.

#### C4 — Launch & Analytics

Light analytics (Plausible, Vercel Analytics) so they know if anyone's using it. "Launch and learn" mentality — don't polish forever. Send the URL to mom.

---

## Decisions needed before locking content

1. **Brandon handshake on day 1 outcomes.** Confirm the bullets in the "Brandon's day" section above, plus the bridge handoff language for Brian's day 2 opener.
2. **Pre-work final list.** Replit account (provided), GitHub account, VS Code installed, GitHub Copilot enabled (free tier), Copilot CLI installed, chat AI account of choice. Probably needs a 10-minute pre-work video to get everyone synced before day 2.
3. **Canned project ideas for structured teaching.** A short menu of 3–4 small project shapes students can pick from when Brian wants the class in lockstep on examples.
4. **Capstone scope guidelines.** Same "multi-user, persistent data, multi-page, no real payments, no real third-party integrations" frame as Brandon's day 1, applied to the capstone. Plus a stub-out snippet library.
5. **The vibe-coded codebase for T5.** Pick a real public repo for the medium-large project tour. Needs to be chaotic enough to be instructive, small enough to navigate in 60 minutes.
6. **The broken repo for Day 4 homework.** Author or find a deliberately broken small repo for the debugging homework. Bugs need to be code-aware-debuggable, not just "the agent fixes it."
7. **Day mapping.** Pending content lock — map modules to Brian's four halves with rough time per module.
8. **Recording vs live cadence.** Daily homework framing for the recording, likely visible "do this, come back tomorrow" gaps with timestamps, matching Brian's existing bootcamp pattern.
