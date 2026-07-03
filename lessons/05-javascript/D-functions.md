---
description: "Functions are how code gets a name. Brian covers defining versus calling, parameters, return, arrow functions (the costume agents prefer), and just enough scope to read confidently."
---

Here's the problem functions solve for you as a reader. A vibe-coded app is thousands of lines. If reading it meant reading every line, top to bottom, you'd never finish, and this course would be a cruel joke. Functions are the escape hatch: a function takes a chunk of code and gives it a _name_, and once code has a good name you can often skip reading its insides entirely. A file full of functions named `calculateTotal`, `sendWelcomeEmail`, and `formatPhoneNumber` can be skimmed like a table of contents. You read the headlines; you only open the article when something smells wrong.

So: functions are how you stop reading code line-by-line and start reading it in paragraphs. Let's learn the shape of one.

## Defining versus calling

```javascript
function makeCoffee() {
  console.log("Grinding beans...");
  console.log("Brewing...");
  console.log("Coffee's ready!");
}

makeCoffee();
```

There are two completely different things happening here, and telling them apart is the single most important reading skill in this lesson.

The first five lines are the _definition_ — we're teaching JavaScript a recipe and naming it `makeCoffee`. Here's the critical part: **defining a function runs nothing.** If our whole file were just the definition, the console would stay silent. We wrote down the recipe; nobody cooked.

The last line is the _call_ (you'll also hear _invoke_ or _execute_ — same thing). Those parentheses after the name are the "do it now" button. `makeCoffee` is pointing at the recipe; `makeCoffee()` is cooking it. Two characters of difference, entirely different meaning — and generated code relies on you knowing which one you're looking at.

Type all of that into the console and run it. Then call `makeCoffee()` twice more. Recipe once, cook as many times as you want — that's the other superpower: write it once, reuse it forever.

Look at this code and tell me what you think will be logged out?

```javascript
function makeCoffee() {
  console.log("Grinding beans...");
  console.log("Brewing...");
  console.log("Coffee's ready!");
}

makeCoffee();
makeCoffee();
```

If you said

```
Grinding beans...
Brewing...
Coffee's ready!
Grinding beans...
Brewing...
Coffee's ready!
```

then you'd be right! We invoked the functions twice so one function _fully_ runs, and then the same function _fully_ runs again.

## Parameters: the blanks in the recipe

A recipe that makes exactly one thing is only so useful. Functions can take inputs:

```javascript
function greet(name) {
  console.log(`Nice to meet you, ${name}!`);
}

greet("Reid");
greet("Rory");
```

`name` is a _parameter_ — a labeled box that's empty in the definition and gets filled at call time. When we call `greet("Reid")`, the string `"Reid"` (the _argument_) lands in the `name` box, and the function runs with it. Same recipe, different ingredient, different output.

> Parameter versus argument: the parameter is the blank in the recipe; the argument is what you actually pour in. People (me included) use the words interchangeably in conversation and nobody has ever been harmed by it.

Functions can take multiple inputs, separated by commas — `function addToCart(item, quantity)` — and when you read a call like `addToCart("oat milk", 2)`, the values match up to the parameters in order. First goes in the first box, second in the second.

## return: the function's answer

`console.log` prints things for humans to see, but most functions in real code don't print anything — they compute something and hand the answer back to whoever called them. That handing-back is `return`:

```javascript
function double(num) {
  return num * 2;
}

const result = double(21);
console.log(result); // 42
```

Read the middle line inside-out: `double(21)` runs, hits the `return`, and the whole expression `double(21)` _becomes_ `42` — which then gets stored in `result`. A function call in the middle of a line is just a placeholder for whatever the function returns. That's why you can even do this:

```javascript
console.log(double(double(5))); // 20
```

Inner call first: `double(5)` becomes 10, then `double(10)` becomes 20. When generated code nests calls like this and your eyes cross, just evaluate from the inside out, one layer at a time.

One more thing about `return`, and you've already seen it: **return also means "exit immediately."** The moment a function hits a `return`, it's done — nothing below runs. That's the machinery behind the guard clauses from last lesson: `if (!user) { return; }` is "no user? We're done here." Bouncer checks ID, bouncer says leave.

## Arrow functions: same actor, different costume

Everything above used the `function` keyword. But a huge share of the functions in agent-written code look like this instead:

```javascript
const greet = (name) => {
  console.log(`Nice to meet you, ${name}!`);
};
```

This is an _arrow function_, and here's the thing I need you to internalize: **it's the same actor in a different costume.** A box named `greet` holding a recipe. Parameters in the parentheses, body in the braces, called exactly the same way: `greet("Reid")`. The `=>` (the "arrow") replaces the word `function` and moves things around a bit. That's it. When you see one, your brain should just say "function" and keep reading.

There's one more variation you'll see constantly, because agents adore it — the one-line arrow with no braces:

```javascript
const double = (num) => num * 2;
```

No braces, no `return` — and here's the rule: **when an arrow function has no braces, whatever comes after the arrow is automatically returned.** That line is exactly our `double` from before, just compressed. It's dense the first ten times you read it and invisible after that.

Why do two costumes exist? History, mostly — arrows arrived in 2015 with some technical differences that will not matter to you in this course. What matters is frequency: your assistant will emit both, often in the same file, and you now read both.

The place arrow functions really earn their keep — and the reason I'm making sure they're solid _now_ — is that JavaScript constantly passes functions to other functions as instructions for later: "hey browser, when someone clicks this button, run _this_." That "this" is almost always an arrow function. It's the beating heart of the DOM lesson coming up, so file the shape away: a function that isn't called right now, but handed to someone else to call later.

## Scope: who can see what

Last piece, and we only need it at squint level. Variables have territory:

```javascript
const shopName = "Herkimer Coffee";

function makeCoffee() {
  const beans = "Guatemalan";
  console.log(`${shopName} is brewing ${beans}`); // works fine
}

makeCoffee();
console.log(beans); // 💥 Uncaught ReferenceError: beans is not defined
```

The rule: **a variable lives inside the braces where it was born.** Code inside a function can see outward (the function can use `shopName`), but code outside can't see inward (`beans` only exists inside `makeCoffee`, so the last line explodes). Rooms have one-way windows: you can look out, nobody can look in.

That's genuinely all the scope theory this course needs, and here's the practical payoff: `ReferenceError: beans is not defined` joins last lesson's "Cannot read properties of undefined" in your growing catalog of famous errors. When you see "is not defined," a very common cause is exactly this — code trying to use a variable from a room it's not standing in (the other big one being a plain old typo in the name). Both are questions your AI assistant answers well when you paste the error _and_ the function it happened in.

## Reading practice

Predict before you run — every piece of this is from lessons B through D:

```javascript
function double(num) {
  return num * 2;
}

const isBig = (num) => num > 100;

let total = 0;

for (let i = 0; i < 3; i++) {
  total = total + double(i);
}

console.log(`Total: ${total}`);
console.log(isBig(total));
```

Got your prediction? Run it. If it surprised you, trace it lap by lap: i is 0, double(0) returns 0, total is 0... The mix of a `function` costume and an arrow costume in the same snippet is deliberate — that's what real files look like.

> 🤖 **Ask your AI Assistant.** Paste in this function and ask nothing about what it does yet: `const mystery = (w) => w.length >= 8;` — first, _you_ guess what it does and what you'd name it (hint: `.length` on a string tells you how many characters it has — a little preview of next lesson). Then ask your assistant "What does this function do, and what would you name it?" and compare. Naming a function well requires completely understanding it, which makes "what would you name this?" one of the best comprehension checks there is — on yourself, and on your assistant. If you're building a Wordle-shaped hunch about why I picked this particular mystery function, no comment.

## Recap

- Functions give code a name so you can read files like a table of contents — headlines first, insides only when something smells wrong.
- Defining teaches the recipe and runs nothing; the parentheses are the "do it now" button. `makeCoffee` points, `makeCoffee()` cooks.
- Parameters are blanks in the recipe; arguments fill them, in order, at call time.
- A call becomes its return value — evaluate nested calls inside-out. And `return` also means "exit now," which powers guard clauses.
- Arrow functions are the same actor in a different costume; no braces after the arrow means auto-return. Both costumes appear in every generated file.
- Variables live inside the braces where they were born. "Is not defined" usually means wrong room or a typo.

Next: objects and arrays — how data gets a shape, and secretly the most important reading lesson in the section.
