---
description: "The section project: build Mad Libs from a blank file with your own hands — inputs, a button, a story — then commit it and hand the tedious parts to your assistant. The first-principles sandwich, fully assembled."
---

Way back in the strings lesson I said template literals were "Mad Libs, but for computers" and typed the word _foreshadowing_. The bill has come due. You're going to build Mad Libs: a page with a few word inputs, a button, and a story that assembles itself from whatever ridiculous words got typed in. It exercises every single thing from this section — variables, template literals, functions, finding elements, changing elements, listening for clicks — and nothing you haven't learned.

Here's the structure, and I want to be transparent about the pedagogy because you're adults and it'll work better if you're in on it. **Part one you build by hand, from a blank file, and your AI assistant stays on the bench.** Not because tools are cheating — the entire back half of this course is tools — but because you cannot appreciate what delegation saves you until you've personally paid the cost. Every tool you'll ever love, you love because you remember the tedium it killed. So first we do the tedium. Then, part two, we kill it.

## Set up your own repo

New folder — mine's `mad-libs` — opened in VS Code. And this time it's _your_ project, so it gets its own save-point system: open the Source Control panel and click **Initialize Repository**. That's it; that's how a plain folder becomes a repo. Cloning gets you someone else's repo; initializing starts your own. You now know both doors into git.

Create your `index.html` and `script.js`, wire them together with a `script` tag like we did back in earlier, and commit the skeleton — "start mad libs" — before writing anything real. Empty save point, clean slate, net strung up.

## The spec

Your page needs:

1. Four text inputs, asking for: a **name**, an **adjective**, a **noun**, and a **verb**. (More if you're feeling literary.)
2. A button that says something like **Tell my story**.
3. When the button is clicked, a story appears below — any story you want, with the typed words dropped into the blanks. Mine involves a Havanese, but I don't want to constrain your art.
4. Clicking again with different words replaces the story with a fresh one.

Stretch goal, if the base version falls quickly: if any input is empty when the button is clicked, show a scolding message instead of the story. (Everything you need for that is in lessons B and C — think about what an empty string is, and which keyword makes decisions.)

## The one new tool

You need exactly one thing this section hasn't taught, so here it is. Text inputs look like this in HTML:

```html
<input id="noun-input" placeholder="a noun" />
```

The `placeholder` is the gray hint text. The `id` is like a class but for one specific element — and you already know how to point at it, because you learned it in CSS: the selector is `#noun-input`, hash for id, dot for class. Same selectors, still paying dividends.

And to read what the user typed into an input, you don't use `textContent` — inputs keep their contents in a compartment called `value`:

```javascript
const nounInput = document.querySelector("#noun-input");
console.log(nounInput.value); // whatever's typed in the box, right now
```

That's the whole toolbox addition. Find the inputs, read their `value` compartments at click time, assemble a template literal, put it somewhere with `textContent`. You have everything.

## Build it

Go. Genuinely — stop reading, go build. Struggle is where this stuff moves from "seen it" to "know it," and the struggle is dramatically more valuable than the finished page.

Ground rules for when you're stuck, because you will be and that's the design:

- `console.log` everything suspicious. Is the variable holding what you think? Log it. Is the function even running? Log at the top of it.
- Read errors out loud. You know the famous ones now — "Cannot read properties of undefined" means a chain link came up empty (did your selector find nothing? typo in the id?).
- If you're stuck more than fifteen minutes on the same wall, your assistant can come off the bench for _hints only_. The prompt matters: "I'm building X. Y is happening instead of Z. Give me a hint about where to look, but do not write the code for me." Assistants honor that instruction shockingly well, and knowing how to ask for a nudge instead of an answer is a skill you'll use forever — it's the difference between having a tutor and having a ghostwriter.

A reference solution follows, but it's worth infinitely more after your attempt than instead of it.

## A reference solution

Seriously, last chance to go struggle first.

Okay. `index.html`, the relevant parts:

```display-html
<h1>Mad Libs</h1>

<input id="name-input" placeholder="a name" />
<input id="adjective-input" placeholder="an adjective" />
<input id="noun-input" placeholder="a noun" />
<input id="verb-input" placeholder="a verb" />

<button id="story-button">Tell my story</button>

<p id="story"></p>

<script src="./script.js"></script>
```

And `script.js`:

```display-javascript
const nameInput = document.querySelector("#name-input");
const adjectiveInput = document.querySelector("#adjective-input");
const nounInput = document.querySelector("#noun-input");
const verbInput = document.querySelector("#verb-input");
const storyButton = document.querySelector("#story-button");
const story = document.querySelector("#story");

storyButton.addEventListener("click", () => {
  const name = nameInput.value;
  const adjective = adjectiveInput.value;
  const noun = nounInput.value;
  const verb = verbInput.value;

  if (name === "" || adjective === "" || noun === "" || verb === "") {
    story.textContent = "Fill in all the blanks! This story deserves your best work.";
    return;
  }

  story.textContent = `One day, ${name} found a very ${adjective} ${noun} on the sidewalk. Naturally, the only thing to do was ${verb} all the way home.`;
});
```

Read it with your lesson-G eyes: found things at the top, one listener, a guard clause playing bouncer (that `||` is new-ish — it means "or," so the guard reads "if any of these is empty, scold and exit"), a template literal doing the Mad Libs, `textContent` painting the result. Your version doesn't need to match this — it needs to work, and you need to be able to trace yours the way you can now trace mine.

Whatever state yours is in: **commit it.** "Mad libs works by hand" is one of the better commit messages you'll ever get to write.

## Part two: kill the tedium

Now the sandwich flips. You've paid full price for every input, every `querySelector`, every wire. Time to feel what delegation buys — open your assistant, in whatever agentic mode you've been using, and hand it real work on _your_ code. Some worthy asks, in rough order of ambition:

1. **"Add a Surprise Me button that fills all the inputs with random words."** Watch what comes back — it'll likely involve arrays of words (you read arrays now) and something shaped like `Math.floor(Math.random() * words.length)`, which is new. Perfect why-question fodder.
2. **"Make it beautiful."** Let it write the CSS. You spent a whole lesson on CSS; skim its choices and notice you can actually follow them.
3. **"Add three different story templates and pick one at random each time."** Arrays again, and watch how it restructures your template literal.

The rules of engagement, and these are the real lesson:

- **Commit before each ask.** You know why.
- **Read the diff before you accept anything.** The Source Control panel shows exactly what changed. That skill you practiced in the loops lesson — spotting what changed between two versions — is now operational.
- **Trace one interaction through the new code** the way you traced the calculator's 7: click Surprise Me, follow the route, find where the random word gets chosen.
- **Ask one why-question** about anything unfamiliar in what it wrote. "Why `Math.floor` and not just `Math.random`?" is a great one.

If the assistant wrecks something — and eventually one will — you get to feel the other payoff: discard changes, back to your save point, sharpen the prompt, go again. That calm is what you've been building toward all section.

> 🤖 **Ask your AI Assistant.** One more, reflective this time. Paste in your final `script.js` — the one you and the assistant built together — and ask: "What's the weakest part of this code? What would break first if I kept adding features?" You may not fully follow every point in the answer yet, and that's fine; what you're practicing is treating your assistant as a code _reviewer_, not just a code writer. That move — asking for critique of code you already have — might be the single most underused prompt in all of vibe coding, and you now know enough to actually read the review.

## Recap

- You built an interactive page from a blank file with your bare hands: inputs, `#id` selectors, `.value`, a listener, a template literal, a guard clause. Every bit of it earned.
- Then you delegated — with save points before every experiment, diff-reading before every acceptance, one traced interaction, and one why-question. That loop _is_ AI-assisted development. Everything else is refinement.
- Hints, not answers: "tell me where to look, don't write it for me" turns your assistant into a tutor.
- The assistant-as-reviewer move — "what's the weakest part of this code?" — is criminally underused. Use it.

That's the section. You came in unable to read a line of JavaScript; you're leaving having traced, instrumented, modified, hand-built, and code-reviewed real programs. Next up, the thing every one of your future apps will do within its first ten minutes of existence: talking to servers.
