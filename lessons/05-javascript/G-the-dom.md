---
title: "The DOM and Events"
description: "The lesson the whole section has been building toward: what the DOM is, how events work, and a guided read of the real calculator — tracing one button press from click to screen."
---

Everything so far has been JavaScript in a vacuum — the console, little files, snippets. Useful, but none of it explains the thing you actually came here to understand: how does clicking a button on a web page _do something_? This is the lesson where the actors finally touch the set. By the end of it you'll have traced a real button press through the real calculator, from the moment your finger clicks to the moment the screen changes, with no steps skipped and no magic invoked.

Open the calculator repo you cloned last lesson in VS Code, and open its `index.html` in your browser. Both at once, side by side if your monitor allows — we're going to be looking at the wiring and the working machine together.

## The DOM: the performance in progress

Here's the missing piece of the theater. HTML is the script — a text file, inert, sitting on disk. But when the browser opens that file, it doesn't just display it; it builds a living model of everything on the page — every element, nested inside each other just like the tags were. That living model is called the **DOM** (Document Object Model, a name only a committee could love).

The distinction matters because the DOM can _change_ after the page loads, and the screen updates instantly to match. The HTML file is the script; the DOM is tonight's performance, happening live, and JavaScript is allowed to walk on stage mid-scene and rearrange things. When the calculator's screen changes from `0` to `7`, nobody rewrote `index.html` — JavaScript changed the DOM, and the browser repainted.

JavaScript's doorway into all of this is an object the browser hands you for free, called `document`. And after the objects lesson, you know exactly how to read what comes next: dots mean "go inside."

## Verb one: finding things

Before JavaScript can change something on the page, it has to get ahold of it. Look at the top of the calculator's `script.js`:

```javascript
const screen = document.querySelector(".display");
```

`document.querySelector(...)` is "go inside the document, run the find-me-an-element function." And what does it take as an argument? **A CSS selector.** The same selectors you learned in the CSS lessons — `.display` means "the element with the class display" — get reused here as JavaScript's way of pointing at things. Your CSS knowledge just became DOM knowledge at no extra charge; that string could just as easily be `"h1"` or `"#total"` or anything else you could write in a stylesheet.

So after that line runs, the variable `screen` holds the actual display element from the page — not a copy, not a description, a live handle on the real thing. Which sets up verb two.

## Verb two: changing things

Scroll to the bottom half of `script.js` and find the shortest function in the file:

```javascript
function rerender() {
  screen.textContent = buffer;
}
```

One line, and it's the entire "changing the page" story: go inside `screen`, set its `textContent` compartment to whatever's in `buffer`. `textContent` is exactly what it sounds like — the text inside an element — and _assigning_ to it changes the page, right now, live. Every single thing you've ever seen appear, disappear, or update on a website without a page reload is, at the bottom of the stack, some flavor of this line: find an element, change something about it.

Find and change. Two verbs. That's the DOM.

## Events: the page is listening

But something still has to _trigger_ all this, and pages spend most of their lives doing nothing — waiting. Waiting for a click, a keypress, a scroll. Each of those happenings is called an **event**, and the way JavaScript deals with them is beautifully consistent with something you already know: you write a recipe, and instead of cooking it yourself, you hand it to the browser with instructions — "when this event happens on this element, cook this."

Look at the bottom of `script.js`:

```javascript
function init() {
  document.querySelector(".buttons").addEventListener("click", (event) => {
    buttonClick(event.target.textContent);
  });
}

init();
```

Read it with everything you've got. `init` is defined, then immediately called on the last line — so this runs once, when the script loads. Inside: find the element with class `buttons` (the whole button pad), go inside it, and call `addEventListener` with two arguments. The first is which event to listen for — `"click"`. The second is — look at the shape — **an arrow function that nobody is calling.** No parentheses cooking it. It's the pattern I planted back in the functions lesson: a recipe handed to someone else to call later. The browser takes it, files it away, and from now on, every click anywhere on that button pad cooks this recipe.

And when the browser cooks it, it fills in that `event` parameter with an object describing what just happened — an incident report. The report has a lot of compartments, but the one we care about is `event.target`: _the specific element that was clicked._ So `event.target.textContent` reads, dots left to right: the thing that was clicked → its text. Click the `7` button and that's `"7"`. Click `+` and it's `"+"`. That string gets passed to `buttonClick`, and the machine takes it from there.

> Notice the listener is on the whole button pad, not on each of the nineteen buttons. One bouncer watching the door of the club, instead of nineteen bouncers each guarding one guest — and `event.target` is the bouncer telling you exactly who moved. This pattern is called _event delegation_, agents use it constantly, and now you can recognize it: a listener on a container, with `event.target` inside sorting out the details. The nice thing to this approach is that if we add or subtract a button, the same bouncer would catch it. However you absolutely could assign one listener per button - totally valid. Our approach of letting the event "bubble up" is called event bubbling!

## The full trace: press 7

Time for the marquee moment. You're going to follow one button press through the entire machine, function by function. Here's the route, and I want you to physically follow along in the file, cursor on each line:

1. You click **7**. The browser cooks the listener's recipe, with `event.target` being the 7 button. `event.target.textContent` is `"7"`, so we call `buttonClick("7")`.
2. `buttonClick` is the traffic cop. `isNaN(parseInt("7"))` — new vocabulary, worth pausing on: `parseInt` tries to turn a string into a number, and `isNaN` asks "did that fail?" (NaN = Not a Number). For `"7"` the conversion succeeds, so the check is false, and we take the `else` road: `handleNumber("7")`.
3. `handleNumber` looks at `buffer`, which is `"0"` right now — the calculator's starting state — so it replaces it: `buffer = "7"`.
4. Back in `buttonClick`, the last line runs: `rerender()`.
5. `rerender` sets `screen.textContent` to `buffer`. The DOM changes. The browser repaints. Your eye sees **7**.

Click → listener → traffic cop → handler → state change → rerender → screen. Every button in the calculator takes this same road; the only thing that varies is which handler the traffic cop routes to. Press **+** and `parseInt("+")` fails the number test, so it goes to `handleSymbol`, which routes it to `handleMath` — go trace that one yourself right now, and jot the route in your `notes.md`. (That's what the field notebook is for.)

Don't just trust my trace — instrument it. Add this as the first line inside `buttonClick`:

```javascript
console.log(`buttonClick got: ${value}`);
```

Save, reload the page in the browser, open the console, and mash some buttons. There it is — your flashlight, pointed at a running machine, showing you the traffic cop receiving every click in real time. This is not a toy exercise; sprinkling `console.log` into unfamiliar code to watch it run is exactly how professionals get their bearings in a codebase, and it works just as well on ten thousand lines as it does on a hundred.

> While you're in there, a careful-reading gotcha that will bite someone eventually: the calculator's operator buttons use the typographic symbols `×`, `−`, and `÷` — not the keyboard's `x`, `-`, and `/`. Look at `flushOperation`: it compares against those exact fancy characters. If you ever retype one of them from your keyboard while editing, the comparison silently stops matching and that operator stops working — no error, no crash, just a button that quietly does nothing. Characters that look alike but aren't alike is a genre of bug that has humbled every developer alive, and agents introduce these too. When something "just doesn't work" with no error, look closer at the characters.

## The state, the whole time

One more reading skill, and it's how you should approach _any_ unfamiliar file from now on. Look at the top of `script.js`: three variables — `buffer`, `runningTotal`, `previousOperator` — with comments explaining each. That's the calculator's entire memory, its _state_, and every function below exists to either change the state or paint it onto the screen. This top-state, bottom-wiring, handlers-in-the-middle shape isn't a coincidence; it's the skeleton of nearly every interactive app, including whatever Copilot builds you. **When you open a strange file: find the state at the top, find the wiring at the bottom, then trace one interaction through the middle.** That reading strategy is worth more than any individual thing in this lesson.

Now prove it generalizes: open your own Copilot-built calculator from the earlier project, side by side with this one. Different code — different names, maybe different structure, that's the point — but hunt for the same organs. Where's its state? Where does it attach listeners? Which function touches the DOM? Same movie, different cast. Write what you find in your notes.

## Get your hands dirty

First — say it with me — **commit before you experiment.** Your `console.log` change is worth a save point: stage it, commit it ("add logging to buttonClick"), and now you're free to be reckless.

Then try these, in ascending order of courage, no AI assistance for the first two:

1. Make the calculator start up saying `HELLO` instead of `0`. (One character... where? The trace you did tells you which variable becomes the screen.)
2. Make the `C` button also log `"cleared!"` to the console.
3. Change what the `←` button does when only one character remains — make it show `0.00` instead of `0`, or anything else you like. Trace first, then change.

If you break it — good, honestly — you know exactly what to do: Source Control panel, discard changes, back to your last save point. The net is under you. That was the whole point of last lesson.

> 🤖 **Ask your AI Assistant.** Paste in the entire `handleSymbol` function and ask a _why_ question, not a _what_ question: "Why does the backspace branch check whether buffer.length is 1, instead of just chopping the last character every time?" You can verify the answer yourself — delete that check (after committing!) and backspace your way past the last digit to see what an empty screen feels like. What-questions get you explanations; why-questions get you design decisions, and design decisions are what you actually need to understand before letting an agent change code. Get in the habit of asking code _why_ it is the way it is.

## Recap

- The DOM is the live performance of your HTML; JavaScript can walk on stage mid-scene. `document` is the doorway.
- Two verbs: find (`document.querySelector`, which speaks CSS selectors — your CSS knowledge transferred for free) and change (`screen.textContent = ...`).
- Events are happenings; `addEventListener("click", ...)` hands the browser a recipe to cook later, and the `event` object is the incident report — `event.target` is who got clicked.
- One listener on a container plus `event.target` inside is event delegation: one bouncer, not nineteen.
- The universal reading strategy: state at the top, wiring at the bottom, trace one interaction through the middle. Works on this file; works on files a hundred times bigger.
- `console.log` in unfamiliar code is professional behavior, not training wheels. And commit before you experiment — the net only works if you string it up first.

> 🤖 **Ask your AI Assistant.** This JavaScript has a pretty subtle bug around situations where your result ends up being zero (e.g. 5 - 5 or 7 \* 0) - ask your assistant to help you debug a bug around users complaining that strange behavior happens when the answer is 0 and see if it helps you fix it.

Next lesson: you build something of your own from a blank file — and then hand the tedious parts to your assistant, now that you know exactly what it's doing on your behalf.
