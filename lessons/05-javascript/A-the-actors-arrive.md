---
description: "The theater analogy pays off: JavaScript is the actors performing your page live. Brian introduces what code actually is, the browser console as your rehearsal space, and your first script tag."
---

Time to complete our theater. So far you've written the script (HTML) and you've designed the set, the lighting, and the costumes (CSS). And honestly? That's a real production. Plenty of useful websites are just a script and a set — your blog is proof. But nothing on that stage _moves_. Nobody reacts when the audience shouts something. The curtain goes up, the audience looks at a beautiful, motionless stage, and that's the whole show.

JavaScript is the actors acting - it's the blocking, the lighting, the **action**. When something on a page _happens_ — a button responds to a click, new content appears without the page reloading, an error message pops up because you typed your email wrong — that's JavaScript performing, live, in front of the audience, every single time the page runs.

Remember the calculator you had Copilot build? Remember how they didn't work - they were just nice looking buttons on page? We are going to use JavaScript to make the web page actually _do_ something.

Over the next several lessons we're going to fix that. By the end of this section you'll be able to write that calculator's code and trace exactly what happens between your finger hitting the 7 button and a 7 showing up on screen. No magic. Just actors following directions.

## What is code, actually?

Here is something that surprises people: code is for humans first and computers second. Code is a set of notes on how to solve a problem, written precisely enough that a computer can follow along. The computer would be equally happy with an unreadable pile of symbols. The readable part is for _us_.

This used to be important because some poor developer (usually future-you) had to come back and re-read the code later. It is now important for a second, bigger reason: **your intern writes most of the code, and you have to check their work.** The world's fastest intern is enthusiastic, tireless, and occasionally (or frequently lol) confidently wrong. When you ask Copilot to build something and it hands you back a hundred lines of JavaScript, the question is no longer "can I write this?" — it's "can I read enough of this to know whether it does what I asked?"

That is the skill we're building. Not fluency. Recognition. You don't need to conjugate every verb in Italian to know when the waiter is telling you the kitchen is closed. (🤌 La cucina è chiusa. 🤌)

## One line at a time

The single most useful thing to know about how JavaScript runs: it does one thing at a time, in order. Line 1, then line 2, then line 3. That's it. That's the whole mental model for now.

> There's a technical term for this — _single threaded_ — and there are asterisks we'll get to when we talk to servers later. You do not need the term or the asterisks today. "Top to bottom, one line at a time" will carry you a very long way.

This matters because it means you can read JavaScript the way you read anything else: start at the top, go down, and at each line ask "okay, what just happened?" Agents write code fast, but the code they write still runs one line at a time, and it still reads top to bottom. Your reading speed doesn't have to match your intern's writing speed. It just has to exist.

## The rehearsal space

Before we put anything on stage, I want to show you the rehearsal space: the browser console. This is a place where you can type a line of JavaScript, hit enter, and watch it run immediately. No files, no saving, no audience. It's the single best place to answer the question "wait, what does this line do?" — which, as a professional code-reader, is a question you are going to ask constantly.

Open your browser's dev tools ([here's how][devtools] on every browser) and click on the **Console** tab. You'll have a blinking cursor. You are now talking directly to JavaScript.

Type this and hit enter:

```javascript
1 + 1;
```

The console answers `2`. Groundbreaking, I know. But notice what happened: you gave JavaScript an instruction, it did the work, it gave you back an answer. That loop — instruction in, result out — is all programming is. Everything else is stacking that loop taller.

Try a few more, one at a time:

```javascript
10 * 12;
("Brian");
"Brian" + " was here";
```

A couple of things worth noticing. Numbers are just numbers. Text has to go in quotes — quoted text is called a _string_ (as in a string of characters, like beads on a string). And `+` does double duty: with numbers it adds, with strings it glues them together. Is that a little weird? Yes. Welcome to JavaScript; the weirdness is part of the charm, like a beloved community theater where one of the lights always flickers.

> This is called a REPL - **R**ead, **E**valuate, **P**rint, **L**oop, and it's where Replit got its name from.

## Putting it in the show

The console is rehearsal. To put JavaScript in the actual show, we attach it to an HTML page with a `script` tag. Make a new folder (mine's going on the desktop, called `js-experiments`), open it in VS Code, and create an `index.html`:

```display-html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>JavaScript Experiments</title>
  </head>
  <body>
    <h1>JavaScript Experiments!</h1>
    <script src="./experiments.js"></script>
  </body>
</html>
```

That `script` tag is the stage door. It tells the browser "go load the actors from this file." So let's give the actors something to do — make a second file in the same folder called `experiments.js` (the name only matters insofar as it has to match the script tag) and put this in it:

```display-javascript
const monthlyRent = 500;

const yearlyRent = monthlyRent * 12;
console.log(yearlyRent);
```

Open the **HTML** file in your browser (not the JS file — the HTML is the show, the JS is loaded by it), open your console, and you should see `6000`. Congrats! That's your JavaScript, running in your page, from a file you made.

Let's read what we just ran, top to bottom, one line at a time — because reading it is the whole point.

**Line 1** creates a _variable_ called `monthlyRent` and stores `500` in it. `const` is the keyword that says "I'm declaring a variable." Think of a variable as a labeled box: we put 500 in a box and wrote "monthlyRent" on the label. Variable names can't have spaces, so we squish the words together and capitalize the humps — `monthlyRent` — which is called _camel casing_, and it's what basically all JavaScript does by convention. When you're reading agent-written code and you see `userEmail` or `totalPrice` or `isLoggedIn`, that's just camel casing doing its job: telling you what's in the box.

**Line 3** makes another box, `yearlyRent`, and fills it with `monthlyRent * 12`. The `*` means multiply. Notice we used the _name_ of the first box instead of typing `500` again. This is one of those "code is for humans" things — `monthlyRent * 12` tells you exactly what's being calculated; `500 * 12` tells you nothing.

**Line 4**, `console.log(yearlyRent)`, prints whatever is in the box to the console. `console.log` is going to be your best friend in this course. It is the flashlight you point at code to see what's actually happening inside it, and I promise you that professional developers with decades of experience still use it hundreds of times a day. There is no graduating past `console.log`.

Oh, and the semicolons at the end of each line: that's JavaScript's period at the end of a sentence. "I have completed this thought." Agents put them everywhere, so you'll see a lot of them.

> 🤖 **Ask your AI Assistant.** Paste those three lines of `experiments.js` into your AI assistant of choice — Copilot Chat, Claude, ChatGPT, whatever you like — and ask: "Explain this code to me line by line like I'm new to JavaScript." Read the answer and check it against what we just walked through. Then change `const yearlyRent = monthlyRent * 12;` to `const yearlyRent = monthlyRent * 52;` and ask it to explain again — did it notice the code now calculates something that isn't yearly rent at all? Getting an LLM to explain code, and then _verifying the explanation against your own read_, is the core loop of this entire course. We're going to do this in every lesson until it's a reflex.

## Recap

- JavaScript is the actors: it's what makes pages _do_ things, live, while the audience watches.
- Code is for humans first — and now that your intern writes most of it, reading it is your job.
- JavaScript runs top to bottom, one line at a time. Read it the same way.
- The console is your rehearsal space. `console.log` is your flashlight. Neither is ever beneath you.
- A `script` tag is how a page loads JavaScript, the same way `link` loaded your CSS.

Next up: what kinds of things fit in those labeled boxes.

[devtools]: https://webmasters.stackexchange.com/questions/8525/how-do-i-open-the-javascript-console-in-different-browsers
