---
description: "Data has shape. Brian covers objects (labeled compartments), arrays (numbered lists), the dots and brackets you use to reach inside them, and the reveal that you've secretly been learning to read JSON — the format every API speaks."
---

I said this one is secretly the most important reading lesson in the section, so let me back that up. Strip away the buttons and the colors, and almost every app you'll ever vibe-code is the same app: **a list of things with details, flowing from somewhere to your screen.** A list of dogs with names and photos. A list of messages with senders and timestamps. A list of properties with nightly rates and reviews. When you read agent-written code, most of what's flowing through it is data shaped exactly like that — and this lesson is where you learn to read the shape.

Single boxes got us this far. Now we need containers.

## Objects: labeled compartments

An object is a box with labeled compartments:

```javascript
const dog = {
  name: "Luna",
  breed: "Havanese",
  age: 14,
  isGoodDog: true,
};
```

Curly braces to open and close, then `label: value` pairs separated by commas. The labels are called _keys_ (or _properties_) and each holds a value — and the values can be anything from earlier lessons: strings, numbers, booleans. One variable, `dog`, now carries four related facts that travel together, which beats juggling `dogName`, `dogBreed`, `dogAge`, and `dogIsGood` as separate boxes.

To reach into a compartment, you use a dot:

```javascript
console.log(dog.name); // "Luna"
console.log(`${dog.name} is ${dog.age}`); // "Luna is 6"
```

**Read dots as "go inside."** `dog.name` is "go inside dog, grab name." And compartments can hold entire objects, so dots chain:

```javascript
const owner = {
  name: "Brian",
  address: {
    city: "Seattle",
    state: "WA",
  },
};

console.log(owner.address.city); // "Seattle"
```

`owner.address.city` reads left to right: inside owner, inside address, grab city. Generated code is full of chains three and four dots long, and every one of them is just directions: this room, then that drawer, then that envelope.

Now — remember our most famous error? **This is where "Cannot read properties of undefined" is born.** If you ask for `owner.addres.city` (typo!), then `owner.addres` is an empty box — `undefined` — and asking undefined for its `.city` is the exact moment JavaScript throws that error. When you see it in the wild, read the chain in the error's vicinity and ask: which link came up empty?

## Methods: verbs that come with the noun

Compartments can also hold _functions_. A function living inside an object gets a special name — a _method_ — but you already know how to read it, because it's just "go inside, grab the function, press the do-it button":

```javascript
const word = "hello";
console.log(word.toUpperCase()); // "HELLO"
console.log(word.length); // 5
```

Strings come with built-in methods — verbs that every string knows how to do. Same with numbers and arrays and nearly everything else in JavaScript. And here's a fun retroactive reveal: `console.log()` — the thing you've typed fifty times — was a method all along. `console` is an object the browser gives you; `log` is a function inside it; the parentheses cook it. You've been reading this pattern a lot now.

(Also, that `.length` from last lesson's mystery function? Not a method — no parentheses — just a regular compartment holding a number. Dot means "go inside" either way.)

## Arrays: numbered lists

Objects handle "one thing with details." Arrays handle "many things in order":

```javascript
const dogs = ["Luna", "Biscuit", "Mochi"];
```

Square brackets, items separated by commas. And to grab an item, you use its position — which, as promised back in the loops lesson, starts at zero:

```javascript
console.log(dogs[0]); // "Luna"
console.log(dogs[2]); // "Mochi"
console.log(dogs.length); // 3
```

`dogs[0]` is the first dog. There is no `dogs[3]` — that's one past the end, and asking for it gets you (say it with me) `undefined`. The zero-start plus `.length` combo is exactly why the standard `for` loop looks the way it does:

```javascript
for (let i = 0; i < dogs.length; i++) {
  console.log(`${dogs[i]} is a good dog`);
}
```

That's the payoff of two lessons of setup: start at 0, go while `i < dogs.length`, and `dogs[i]` grabs a different item each lap. "Loop over the list, do something with each item" — the single most common pattern in application code, now fully readable to you.

> As promised, other costumes exist for this same idea. `for (const d of dogs) { ... }` reads "for each d of dogs" and skips the counter bookkeeping entirely — agents like it when they don't need the position number. And you'll see `dogs.forEach(...)` and `dogs.map(...)`, which are methods that take a function (a recipe handed over to be called once per item — the "handed to someone else to call later" shape from last lesson). Different outfits, same dance: once per item. Recognize and move on. Ask you AI assistant to explain more when you need to debug it.

## The shape of every app: arrays of objects

Now combine them, because this is the shape:

```javascript
const dogs = [
  { name: "Luna", breed: "Havanese", age: 6 },
  { name: "Biscuit", breed: "Corgi", age: 3 },
  { name: "Mochi", breed: "Shiba Inu", age: 5 },
];

console.log(dogs[1].name); // "Biscuit"
```

A numbered list where every item is a labeled box. Read `dogs[1].name` left to right: inside dogs, grab item 1, go inside it, grab name. Brackets and dots mix freely, and each one is a single step of the directions.

Every list-of-things-with-details I opened this lesson with — messages, properties, search results — is this shape in the code. When you crack open a generated file and see a wall of `someArray[i].something.somethingElse`, you're not looking at complexity. You're looking at directions into a data shape, and you can now follow them step by step.

## The reveal: you've been learning JSON

One more thing, and it's the reason this lesson punches above its weight. There's a universal format that servers, APIs, and config files use to describe data. It's called **JSON** — JavaScript Object Notation — and it looks like this:

```json
{
  "word": "BILLY",
  "puzzleNumber": 841,
  "validWords": ["BILLY", "SILLY", "HILLY"]
}
```

Look familiar? It's objects and arrays with slightly stricter dress code (keys always in quotes, no trailing commas). When your app asks OpenAI a question, the answer comes back as JSON. When it asks a weather service for the forecast: JSON. The `package.json` file sitting in every JavaScript project you'll ever open: JSON. The notation you just spent a lesson learning to read is, almost by accident, the notation the entire internet uses to pass data around. Soon, when we start talking to real servers, the responses are going to look like — well, exactly like that snippet, and that's not a hypothetical example either. (Foreshadowing remains my favorite literary device. That and irony.)

## Reading practice

Predict before you run:

```javascript
const games = [
  { opponent: "Inter", goalsFor: 2, goalsAgainst: 1 },
  { opponent: "Juventus", goalsFor: 0, goalsAgainst: 3 },
  { opponent: "Napoli", goalsFor: 1, goalsAgainst: 1 },
  { opponent: "Cagliari", goalsFor: 7, goalsAgainst: 0 }, // can you tell I'm a Cagliari fan
];

let wins = 0;

for (let i = 0; i < games.length; i++) {
  if (games[i].goalsFor > games[i].goalsAgainst) {
    wins++;
  }
}

console.log(`Record: ${wins} wins out of ${games.length}`);
```

Every piece is from lessons B through E: an array of objects, a zero-indexed loop bounded by `.length`, bracket-then-dot directions, a comparison, `++`, and a template literal. Predict the output, run it, trace any surprise lap by lap.

> 🤖 **Ask your AI Assistant.** Ask your assistant: "Give me a realistic example of the JSON a weather API might return for a 3-day forecast." Look at what comes back and, before asking anything else, write the directions yourself: what would you type to reach tomorrow's high temperature? (Something shaped like `forecast.days[1].high` — yours will differ.) Then verify: "If this were in a variable called forecast, how would I access tomorrow's high temperature?" and see if the assistant's path matches yours. Navigating a data shape you've never seen before is precisely what you'll do constantly in real projects — the shapes change, the dots-and-brackets reading never does.

## Recap

- Objects are labeled compartments: `{ name: "Luna", age: 14 }`. Dots mean "go inside," and they chain.
- "Cannot read properties of undefined" is born when a link mid-chain comes up empty — typo'd key, missing data, or a box that hasn't been filled yet.
- Methods are functions living inside objects — verbs that come with the noun. `console.log()` was one all along.
- Arrays are numbered lists starting at 0; `arr[i]` plus `.length` plus a `for` loop is the most common pattern in app code.
- Arrays of objects are the shape of nearly all application data, and brackets-and-dots are just step-by-step directions into them.
- JSON is this same notation with a stricter dress code, and it's how every API on the internet talks. You can now read it.

Next: we get the calculator's actual code onto your machine — which means it's time to meet git and GitHub.
