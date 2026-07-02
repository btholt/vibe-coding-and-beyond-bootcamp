---
description: "Brian covers the kinds of things JavaScript can put in its labeled boxes — numbers, strings, booleans, and the infamous undefined — plus how to read const, let, and the backtick strings agents love to write."
---

Last lesson we made a labeled box called `monthlyRent` and put `500` in it. Now let's talk about what else can go in the boxes, because when you're reading a file of agent-written code, an enormous amount of what you're looking at is just this: boxes being made, boxes being filled, and boxes being read from. If you can glance at a line and go "ah, that's a box with text in it," you're reading JavaScript.

Keep your console open for this whole lesson. Every time you see a snippet, type it in (or a variation of it — changing my examples to your own is even better). This stuff sticks through fingers, not eyeballs.

## Numbers

Numbers are refreshingly boring. They look like numbers.

```javascript
const teslasInDriveway = 2;
const nightlyRate = 189.99;
const degreesInSeattle = -4;
```

Whole numbers, decimals, negatives — JavaScript treats them all as just "number." Some languages make you care about the difference between integers and decimals. JavaScript does not, and for our purposes that's one less thing.

Math works the way you'd hope: `+`, `-`, `*` for multiply, `/` for divide. There's also `%`, which is called _modulo_ and gives you the remainder after division — `10 % 3` is `1`. That one looks bizarre the first time you see it in generated code, so I'm flagging it now: it's usually there to answer questions like "is this number even?" or "have we hit every third item?"

## Strings

A string is text. The name comes from "a string of characters," like beads on a string, and yes, it's a weird name, and no, we're not going to fix it now — it's forty years too late.

Strings go in quotes, and JavaScript accepts three different kinds:

```javascript
const first = "double quotes";
const second = "single quotes";
const third = `backticks`;
```

The first two are identical in every way that matters; it's a style choice, and your AI assistant will pick one and be consistent about it. The third one — backticks, that key hiding above Tab that you've never had a reason to press — is different, and it is _everywhere_ in agent-written code, so let's give it real attention.

Backtick strings are called _template literals_, and their superpower is that you can embed values directly inside them with `${}`:

```javascript
const name = "Reid";
const age = 4;
const greeting = `${name} is ${age} years old`;
console.log(greeting); // "Reid is 4 years old"
```

Whatever's inside the `${}` gets evaluated and dropped into the string. Compare that to the old way of gluing strings together with `+`:

```javascript
const greeting = name + " is " + age + " years old";
```

Both work. But the template literal reads like the sentence it produces, which is exactly why agents emit them constantly. When you're scanning generated code and you see backticks with `${}` sprinkled through them, translate it in your head as "a fill-in-the-blank sentence." That's all it is. Mad Libs, but for computers. (Foreshadowing.)

Go make a template literal about yourself in the console right now. Something like:

```javascript
const profession = "attorney";
const yearsExperience = 12;
console.log(
  `I have been an ${profession} for ${yearsExperience} years and now I read JavaScript.`,
);
```

## Booleans

A boolean is a value that is either `true` or `false`. That's it. That's the whole type.

```javascript
const isLoggedIn = true;
const hasPaid = false;
```

Booleans feel almost too simple to bother with, but they're the hinges the entire program swings on: every "if the user is logged in, show the dashboard; otherwise, show the login page" decision comes down to a boolean. Next lesson is entirely about those decisions, so for today just learn to recognize them. Agent-written code loves boolean variables named like yes/no questions — `isLoading`, `hasError`, `canSubmit` — and that naming convention is a gift to you, the reader. `isLoading` contains `true` or `false`, and you knew that without reading anything else. Good names really are the documentation.

## const, let, and reading intent

You've been typing `const` this whole time, so let's actually explain it, along with its sibling `let`.

```javascript
const birthYear = 1984; // this box's contents will never be swapped
let score = 0; // this box's contents will change
score = 10; // see? changed. no `let` needed the second time — the box already exists
```

`const` is short for _constant_: once you put something in the box, you can't swap it for something else. `let` means the contents are allowed to change later. If you try to reassign a `const`, JavaScript throws an error and refuses. (You'll do this on purpose in a minute.)

Here's why this matters for a code _reader_: `const` versus `let` is the author telling you their intentions. When an agent writes `const taxRate = 0.101;`, it's telling you "this value is settled; nothing below this line messes with it — don't worry about it." When it writes `let attempts = 0;`, it's telling you "keep an eye on this one; something further down is going to change it." That's a genuinely useful reading signal, and you get it for free on nearly every line. Modern style — and agent style — is `const` for almost everything, `let` only when the value actually needs to change.

> You will also occasionally see `var` in older code and older tutorials. It's the ancestor of `let` from before 2015. It has some gross quirks we're not going to cover; just read it as "old-timey let" and move on. If an agent emits `var` in new code, that's actually a mild yellow flag that it's imitating outdated examples.

## undefined: the empty box

One more value, and it's the one you're going to see the most — usually in an error message, usually at an inconvenient moment.

`undefined` is what's in a box that exists but was never filled:

```javascript
let winner;
console.log(winner); // undefined
```

There's also `null`, which is a box someone _deliberately_ filled with nothing — "I checked, and there's no value here, on purpose." The distinction is subtle and honestly even professionals mostly treat them as flavors of the same idea: nothing is home.

Here's why I'm telling you about a value that means nothing: **the most famous error in all of JavaScript is `Cannot read properties of undefined`.** You will see it. Your app will break with it. And when it happens, it almost always means the same thing: some code expected a box to have something in it, and the box was empty. Maybe the data hadn't arrived from the server yet; maybe someone misspelled a name and got a box that doesn't exist. When we get to debugging as a discipline later in the course, this error is going to be an old friend. For now, just file it away: `undefined` = empty box, and code that assumes a box is full when it's empty falls over.

## Break something on purpose

Time to cause your first intentional error, because reading error messages calmly is a superpower and the only way to get there is exposure. In your console:

```javascript
const rent = 500;
rent = 600;
```

JavaScript will yell at you: `Uncaught TypeError: Assignment to constant variable.` Congratulations — nothing is broken, nothing is on fire, your computer is fine. The error message told you exactly what happened: you tried to reassign a constant. Errors are not the program insulting you; they're the program filing a very specific complaint. Learning to read the complaint is most of debugging.

> 🤖 **Ask your AI Assistant.** Copy that exact error message — `Uncaught TypeError: Assignment to constant variable.` — paste it into your AI assistant, and ask "I got this error in my JavaScript. What does it mean and what usually causes it?" Read the answer and check it against what you know (you literally just caused this error on purpose, so you know the ground truth). This paste-the-error-and-ask move is one of the highest-value habits in this entire course, and today you got to try it with the answer key already in hand. Bonus round: ask it "when should I use let instead of const?" and see if its answer matches the reading-intent framing from this lesson.

## Recap

- Numbers are numbers. `%` is remainder; it shows up in generated code more than you'd expect.
- Strings are text in quotes. Backtick strings with `${}` are template literals — fill-in-the-blank sentences — and agents write them constantly.
- Booleans are `true`/`false`, and boolean variables named like yes/no questions (`isLoading`) are self-documenting.
- `const` means "settled, stop worrying about it"; `let` means "watch this one." That's the author signaling intent to you, the reader.
- `undefined` is the empty box, and "Cannot read properties of undefined" is the most common way JavaScript apps fall over.
- Errors are specific complaints, not insults. Read them, then ask your assistant about them.

Next lesson: booleans get their moment — how code makes decisions and repeats itself.
