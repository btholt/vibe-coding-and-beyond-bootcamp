---
description: "Before we crack open the calculator's code, we need a way to get code — and a safety net for changing it. Brian introduces repos, git versus GitHub, cloning in VS Code, and the single most important habit in AI-assisted coding: commit before you let the agent loose."
title: "git and GitHub"
---

Next lesson we're going to crack open a real calculator's code and trace it end to end. Which raises a practical question: how does code get from someone else's project onto your machine? And it raises a second, sneakier question that turns out to be the most important one in this whole course: once you and your AI assistant start _changing_ code, what happens when a change makes everything worse?

Both questions have the same answer, and it's called git.

## A folder with a memory

A _repository_ (everyone says _repo_, you can think of it as a project) is just a folder of code that remembers its own history. Every time you tell it "save this moment" — that's called a _commit_ — it takes a snapshot of every file. Later you can look back at any snapshot, see exactly what changed between any two of them, or roll the entire folder back to how it was last Tuesday at 2pm.

If you've ever played a video game with save points, you already understand git. Commit before the boss fight. If things go badly, you don't start the game over — you reload the save. (Save scumming is encouraged here.)

Two names to untangle immediately, because everyone conflates them at first:

- **git** is the save-point system itself — a tool that runs on your machine, tracking snapshots of a folder. You can use git without GitHub, but few people do. Created by the creator of Linux.
- **GitHub** is a website that hosts copies of repos in the cloud — where you back yours up, and where you go to get everyone else's. A startup that was eventually bought by Microsoft.

Git is the game's save system; GitHub is the cloud your saves sync to. (GitHub is also where basically all of the world's open source code lives, which is why "the code is on GitHub" is a sentence you've probably heard even if you didn't know what it meant.)

> Remember on day one when Replit made checkpoints as the agent worked, and you could roll back when it went sideways? That was this. Replit was doing git-shaped things for you behind the curtain. VS Code is another environment, not a graduation — but here, _you_ hold the save button, and that turns out to be a lot of power.

## Why this is the most important habit in vibe coding

Here's the AI-era pitch, and I want to be dramatic about it because it's earned: **an AI agent can improve your app in thirty seconds, and it can wreck your app in thirty seconds.** Sometimes the same thirty seconds. You ask for a small change to the header, and the agent — enthusiastically, confidently — also "cleans up" three other files, and now nothing renders and you don't know why.

Without git, that's a catastrophe. You're left trying to remember what the code looked like before, manually un-picking changes across files you barely know.

With git, it's a shrug. You committed right before you asked. Roll back the save. Try a different prompt.

So here's the habit, and I'd get it tattooed somewhere if I were you: **commit before you let the agent loose.** Every time you're about to hand your intern something meaty, save first. Working checkpoint, then experiment, and the experiment can never cost you more than itself. This one habit is the difference between vibe coding feeling like a trapeze act with a net and without one. If you're really worried, commit _and_ push it to GitHub.

## Let's get you the calculator

Enough theory — let's actually clone a repo. _Cloning_ is downloading a full copy of a repo, history and all, onto your machine. Cloning does not modify the repo itself, just nabs a copy of it.

First: if you don't have a GitHub account yet, go make one at [github.com][github] — the free tier is all you'll ever need for this course. We'll spend real quality time with GitHub later; today you just need to exist there.

Now, in VS Code:

1. Open the Command Palette — Cmd-Shift-P on Mac, Ctrl-Shift-P on Windows. (This is VS Code's "just type what you want" box, and it's how I do half of everything.)
2. Type "clone" and pick **Git: Clone**.
3. Paste in the calculator repo's address: `https://github.com/btholt/vibe-coding-bootcamp-calculator`
4. Pick a spot for it (I keep a folder just for code projects), and when it asks, open the cloned repo.

That's it. You now have the complete calculator project — the same kind of code your Copilot-built calculator is made of — sitting on your machine with its full history attached. If VS Code asks you to sign in to GitHub along the way, let it; that handshake pays dividends later.

> If any of this throws an error about git not being installed, that's a one-time setup thing — [git-scm.com][gitscm] has installers, and VS Code will usually offer to help. And if you hit a wall, this is a perfect ask-your-AI-assistant moment: paste the exact error and say what you were doing. Environment setup is nobody's favorite genre, but assistants are remarkably good at it.

One more option worth knowing about: [GitHub Desktop][ghdesktop] is a free, friendly point-and-click app for exactly the things in this lesson — cloning, seeing what changed, committing. Everything we do in VS Code's Source Control panel can be done there too, and some people simply like it better. If VS Code's git integration ever feels fussy, it's another great tool; use whichever one keeps you committing.

Many people will just use the command line git. I do, but I've also spent 15 years mastering. I used to tell everyone to spend time mastering git but in the age of AI, you can always ask AI to commit for you and/or to fix your mistakes. Very few mistakes can you not recover from - see [ohshitgit.com][ohshitgit].

## Your first save point

Let's make a commit so the save button is muscle memory before the stakes exist. Make a new file in the repo called `notes.md` and write a line or two in it — your name, today's date, one thing from this section that surprised you. This file is going to be your field notebook: next lesson we walk through the calculator's code together, and this is where your observations and questions will live. Save the file.

(Why a new file instead of editing something? Your clone is your own copy — you can change anything — but if you edit _my_ files, your version and mine drift apart, and when I update the repo later, git will make you reconcile the two. A file only you touch never has that problem. This tension between your copy and the original is a real and interesting topic, and it's exactly the pull-requests-and-branches material we're saving for the GitHub deep dive.)

Now look at the left edge of VS Code: the **Source Control** icon (it looks like a little branching diagram) has a badge on it. Click it. VS Code is showing you exactly what changed — a brand-new file, in this case. Git noticed. Git always notices.

Two clicks to make it a save point:

1. Hover over your changed file and click the **+** to _stage_ it. Staging is packing the box — choosing which changes go in this snapshot. (With one file changed it feels like pointless ceremony; with ten files changed it's how you save related things together.) Do note that when you _stage_ something, it gets staged _as is_. If you stage something then change it, it's still the old version staged. If you want change something that's staged and commit the new version, just stage the file again.
2. Type a short message in the box describing the change — "start my field notes" — and hit **Commit**.

The message matters more than it seems: a repo's history reads like a diary of the project, one commit message at a time, and "fix stuff" tells future-you nothing. Present tense, short, says what the snapshot does. Agents write pretty good commit messages, by the way — asking your assistant to draft one from your changes is a legitimate move you'll use on tooling day.

And that's the whole loop for now: change, stage, commit. Save points on demand. We are deliberately _not_ covering pushing (syncing your commits up to GitHub), branches, or pull requests today — those get real coverage soon, when we tour GitHub properly. Right now you have exactly what we need: the code on your machine, and the save button under your thumb.

> 🤖 **Ask your AI Assistant.** Git has fifty years of accumulated jargon and your assistant is genuinely great at translating it. Two questions to ask it now: first, "What's the difference between saving a file and committing in git? Explain it to someone who's never used version control" — check the answer against this lesson. Second, and better: "I'm going to be using AI coding agents to modify my projects. When should I commit, and why?" If its answer isn't some version of _commit before you let the agent loose_, I'll be shocked — but read its reasoning, because hearing the same advice justified a second way is how advice becomes conviction.

## Recap

- A repo is a folder with a memory; a commit is a save point; git is the save system on your machine and GitHub is the cloud it syncs with.
- Replit was making checkpoints for you on day one. Now you hold the save button yourself.
- The habit that pays for this whole lesson: **commit before you let the agent loose.** An experiment can never cost you more than itself if you saved first.
- Clone via the Command Palette, read changes in the Source Control panel, stage (pack the box), commit (label it honestly).
- Pushing, branches, and pull requests are real and coming soon — not today.

Next lesson, the one this whole section has been building toward: we open the calculator and trace a button click from HTML to JavaScript to the screen. The actors take the stage.

[github]: https://github.com/signup
[gitscm]: https://git-scm.com/downloads
[ghdesktop]: https://desktop.github.com/
[ohshitgit]: https://ohshitgit.com/
