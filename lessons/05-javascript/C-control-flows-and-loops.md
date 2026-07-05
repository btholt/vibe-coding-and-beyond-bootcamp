---
description: "How code makes decisions and repeats itself. Brian covers comparisons, if/else, the ternaries agents love to squish things into, and for and while loops — with an emphasis on reading them at a glance."
---

So far our code has run top to bottom, every line, exactly once. Real programs almost never work that way. Real programs make decisions ("if the user is logged in, show the dashboard") and repeat themselves ("for every dog in this list, show a photo"). Those two ideas — branching and looping — are called _control flow_, and once you can read them, you can follow the plot of almost any file your AI assistant hands you. This is the lesson where code stops being a list of instructions and starts being a story with "meanwhile" and "or else" in it.

Console open. Fingers ready.

## Comparisons: questions with boolean answers

Last lesson I said booleans are the hinges programs swing on. Here's how booleans get made — by asking questions:

```javascript
const age = 4;
console.log(age > 3); // true
console.log(age < 3); // false
console.log(age >= 4); // true
console.log(age === 4); // true
console.log(age !== 10); // true
```

The comparison operators mostly read like math class: greater than, less than, greater-or-equal. The two that need explaining are the last ones.

`===` asks "are these the same?" and yes, it's three equals signs. One `=` means "put this in the box" (assignment); `===` means "are these equal?" (a question). Squint-level reading rule: **one equals sign is a statement, three is a question.** And `!==` is the opposite question — "are these different?" The `!` means "not" in JavaScript, and you'll see it in front of all kinds of things: `!isLoggedIn` reads as "not logged in."

> JavaScript also has `==` with two equals signs, which is a looser, more forgiving comparison with rules nobody fully remembers. Modern style is to always use `===`, and your AI assistant will too. If you see `==` in generated code, read it the same as `===` and carry on — the difference almost never matters for you, and when it does, that's a great question for your assistant.

## if and else: the fork in the road

```javascript
const hour = 14;

if (hour < 12) {
  console.log("Good morning!");
} else if (hour < 18) {
  console.log("Good afternoon!");
} else {
  console.log("Good evening!");
}
```

Read it exactly like English: **if** this question is true, run the code in the curly braces; **else if** gives it another question to try; **else** is the catch-all when nothing else matched. Only one of the three blocks runs. The curly braces `{}` mark where each branch starts and stops — think of them as the walls of a room, and the code inside only runs if you enter that room.

Run that snippet in the console, then change `hour` and run it again. Then change it again. You now control the flow.

Here's what this looks like in the wild, because agent-written code is absolutely stuffed with a particular flavor of `if` called a _guard_ — a check at the top of some code that bails out early if things aren't right:

```javascript
if (!user) {
  return;
}
```

Read: "if there's no user, stop here." (Remember `!` means "not," and `return` — which we'll properly meet in the functions lesson — means "we're done, exit now.") Guards are the bouncers of code: they stand at the door and turn away anything that shouldn't come in. When you're scanning a generated file and see a stack of little `if`s at the top of something, that's not the interesting logic — that's the bouncer checking IDs. Skim past them to the main event.

## Ternaries: the squished if/else

Agents love this next one, so even though it looks like a ransom note, you need to be able to read it:

```javascript
const hour = 14;
const greeting = hour < 12 ? "Good morning!" : "Good afternoon!";
```

That second line is a _ternary_, and it's just an if/else squished onto one line: **question, then `?`, then the value if true, then `:`, then the value if false.** The line above reads "is hour less than 12? If so, greeting is 'Good morning!'; otherwise it's 'Good afternoon!'"

You don't need to write these. You absolutely need to recognize them, because generated code uses them everywhere a value depends on a condition, and the `? :` punctuation is meaningless until someone tells you the pattern — and now someone has. When you hit one, mentally unfold it back into an if/else and it'll read fine.

## Loops: doing it again

The other half of control flow is repetition. Say I want to log a countdown:

```javascript
console.log(3);
console.log(2);
console.log(1);
console.log("Liftoff!");
```

Fine for 3. Miserable for 100. Impossible when you don't know the number in advance — "show a card for every dog the server sends back" might be 5 dogs or 500. Enter the loop:

```javascript
let countdown = 3;

while (countdown > 0) {
  console.log(countdown);
  countdown = countdown - 1;
}
console.log("Liftoff!");
```

A `while` loop reads: "while this question is still true, keep running the code in the braces." Each lap, we print the number and then knock it down by one; when `countdown` hits 0, the question `countdown > 0` becomes false and the loop ends. Notice `countdown` is a `let` — its contents change, and now you've seen why `let` exists.

> If the question never becomes false, the loop never stops — an _infinite loop_, and your browser tab will freeze. Every programmer has done this. When you eventually do it (on purpose, right now, if you're feeling brave: `while (true) { }` — and be ready to close the tab), you'll have joined an ancient and universal club.

The loop you'll actually see most often in generated code is the `for` loop, which is the same idea with the bookkeeping bundled into the top line:

```javascript
for (let i = 0; i < 5; i = i + 1) {
  console.log(`This is lap number ${i}`);
}
```

That top line is three statements separated by semicolons, and they always play the same three roles: **start here** (`let i = 0`), **keep going while this is true** (`i < 5`), **do this after every lap** (`i = i + 1`). The `i` is short for _index_ — it's the lap counter, and single-letter `i` is a convention so old it has grandkids now and so universal that fighting it is pointless.

> Notice we started counting at 0, not 1. Programmers almost always count from zero — the first lap is lap 0, the first item in a list is item 0. It feels wrong for about a week and then it feels normal. For now just know that when you see let i = 0, nothing is off — that's the standard starting line — and a loop that runs while i < 5 runs five times: laps 0, 1, 2, 3, and 4. This pays off big when we get to arrays, where the first item really does live at position 0.

Two bits of shorthand you'll see constantly, both meaning "add one to i": `i++` and `i += 1`. Same for `i--` (subtract one). Agents use `i++` reflexively. It's just `i = i + 1` wearing a smaller coat.

Run the loop. Change the `5` to `10`. Change `i = 0` to `i = 5`. Predict what'll happen before each run — the predicting is where the learning is.

Why does a loop counter matter? Because the real pattern you'll read over and over is "loop over a list and do something with each item" — for every to-do in the list, make a checkbox; for every message in the chat, render a bubble. We need to meet lists (JavaScript calls them _arrays_) before that clicks fully, and we'll cover that more soon. When we get there, the `for` loop is the engine under it.

> One heads-up so future-you isn't confused: JavaScript has several other loop flavors — `for...of`, and array methods like `.forEach()` and `.map()` that loop without looking like loops. Your assistant will sometimes emit those. Don't panic when you see them; they're all "do this once per item" in different outfits, and we'll wave at them when we get to arrays. Recognizing "this is some kind of loop" is 90% of the value.

## Reading practice

Before we talk to our AI assistant, read this — don't run it yet — and predict what it logs:

```javascript
let losses = 0;

console.log("Welcome to the game");

for (let i = 0; i < 10; i++) {
  if (i % 3 === 0) {
    console.log(`Game ${i}: won!`);
  } else {
    losses++;
  }
}

console.log(`Total losses: ${losses}`);
```

Everything in there is from this lesson or the last one: a `for` loop, an `if/else`, the `%` remainder trick answering "is this number even?", `++`, and a template literal. Made your prediction? Now run it and check yourself. If it surprised you, walk it one lap at a time — i is 0, is 0 % 2 equal to 0, yes, so... That slow lap-by-lap trace is exactly the muscle this course is building.

> 🤖 **Ask your AI Assistant.** Paste that same snippet in and ask: "Walk me through this code one loop iteration at a time and tell me what it logs." Compare its walkthrough to yours. Then ask it: "Rewrite this so it counts wins instead of losses" and read the diff between what it gives you and the original — spotting _what changed between two versions of code_ is a skill you'll lean on hard during the debugging days, and this is a low-stakes place to start practicing it.

## Recap

- Comparisons (`===`, `!==`, `<`, `>=`) are questions with boolean answers. One equals sign is a statement; three is a question. `!` means "not."
- `if / else if / else` is a fork in the road; exactly one branch runs. Little `if`s that bail out early are guards — bouncers, not plot.
- A ternary (`? :`) is a squished if/else. Unfold it in your head and move on.
- `while` repeats until its question is false; `for` bundles start / keep-going / after-each-lap into one line. `i++` is "add one to i."
- Other loop shapes exist (`.forEach()`, `.map()`, `for...of`); recognize them as loops and don't sweat the details yet.
- Predict, then run, then trace the surprise. That loop is how you learn to read code.

Next up: functions — how code gets a name, so you can stop reading every line and start reading in paragraphs.
