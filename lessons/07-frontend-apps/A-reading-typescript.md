---
title: "Reading TypeScript"
description: "The colons are labels. Brian covers how to read TypeScript without learning to write it — types as documentation you can trust, the squint rule, why agents love the stuff, and what to do when the red squiggles come for you."
---

Open almost any generated project and the files won't end in `.js` — they'll end in `.ts` or `.tsx`, and the code inside will be sprinkled with colons, angle brackets, and words like `string` and `number` in places you've never seen them. That's TypeScript, it is overwhelmingly the default output of modern coding agents, and this lesson exists so that the first time you see it, your reaction is a shrug instead of a spiral.

Here's the entire secret up front: **TypeScript is JavaScript with labels on the boxes.** Everything you learned in the JavaScript section is still true, still there, still running the show. TypeScript just adds annotations — declarations of what _kind_ of thing each box is allowed to hold — and those annotations are for checking your work, not for doing work. The program is the JavaScript; the types are sticky notes.

## Reading the labels

Side by side. JavaScript you can already read:

```javascript
const age = 6;

function greet(name) {
  return `Nice to meet you, ${name}!`;
}
```

The same thing in TypeScript:

```typescript
const age: number = 6;

function greet(name: string): string {
  return `Nice to meet you, ${name}!`;
}
```

Read a colon as "and it's a." `age: number` — a box called age, _and it's a_ number. `name: string` — the parameter name, _and it's a_ string. The one after the parentheses is the function's return type: `greet` gives back a string. Nothing about what the code _does_ changed; the labels just say out loud what you'd have inferred anyway.

Which gives you the universal decoding move, the **squint rule**: to read TypeScript as JavaScript, ignore everything from a colon up to the next `=`, `,`, `)`, or `{`. What remains is JavaScript you already read fluently. Practice it on any generated file and watch the intimidation evaporate.

## Types are documentation you can trust

Here's where TypeScript goes from "tolerable" to "actively useful to you, the reader." TypeScript can describe entire object shapes:

```typescript
type Pet = {
  name: string;
  species: string;
  breed: string;
  age: number;
  image: string;
};
```

Look at that with your objects-and-arrays eyes. That's not code that runs — it's a _description of a shape_, and you've seen that exact shape before: it's a pet from the pets API. When a generated codebase has a `type` or `interface` block like this, it's answering the question you'd otherwise spend ten minutes on: _what does this data look like?_ Every field, every kind. And unlike a comment — which can rot into a lie while the code changes around it — types are _checked_. If the code stops matching the label, the tools complain immediately. Types are the only documentation in a codebase that is mechanically prevented from lying to you, which makes the type definitions one of the first places worth looking when you open something unfamiliar.

One more shape you'll see constantly — the angle brackets:

```typescript
const [pets, setPets] = useState<Pet[]>([]);
```

Don't worry about `useState` yet (next lesson, and it's a good one). The angle brackets read as "of": that's a useState _of_ an array of Pets. `Pet[]` is "Pets, plural" — a type, wearing the array brackets you already know. When angle brackets appear, something is being told which type it's working with. That reading — "of whatever's in the brackets" — will carry you through the vast majority of what agents generate.

What I'm trying to say, types in TypeScript can genuinely help you read it and understand it, but it can also be super dense (like our example above here.) If it helps, use it. If it's confusing and slowing you down, it's okay to just parse that as "hey this React's useState and it expects pets" and move on.

## Why agents love this stuff

It's worth knowing _why_ your assistant defaults to TypeScript, because it's not fashion. Labels on every box mean mistakes get caught the instant they're typed rather than the moment a user clicks the wrong button: pass a string where a number belongs and the tools flag it immediately, no running required. For a human that's nice. For an agent it's oxygen — the agent writes code, the type checker instantly reports every mismatch, the agent fixes them, repeat. TypeScript gives your tireless intern a tireless proofreader, and the two of them iterate faster than either would alone. When you let an agent loose on a TypeScript project, a whole category of its mistakes gets caught before you ever see them. You benefit from the labels without writing a single one.

**You should use TypeScript on every AI-assisted app**. It will genuinely help you ship more reliable software more quickly.

## When the red squiggles come for you

The one time TypeScript stops being background noise: the error. In VS Code it's a red squiggle; in full it reads something like

```
Type 'string' is not assignable to type 'number'.
```

Add it to your famous-errors catalog, because the translation is always the same: **something tried to put the wrong kind of thing in a labeled box.** A string headed for a number-shaped hole. And unlike most of your catalog, this error fires _before the code ever runs_ — often it means the mistake was caught early, not that something exploded in front of a user. Two things to know about it: first, these errors can block a project from building at all, which sounds harsh but is the proofreader refusing to let a known mistake ship (more on building soon — it's this section's finale). Second, your playbook doesn't change: read the error, find the box and the mislabeled thing, and if it's opaque, paste the full error _and the line it points at_ into your assistant. Type errors are the single genre of error assistants resolve most reliably, because all the information is right there in the message. VS Code often will even let you just right click and say "hey make Copilot look at this".

You are not going to author types in this course. The agent writes them, the checker checks them, and you read them as the trustworthy documentation they are. If you ever _want_ to author them — it's a deep and genuinely rewarding discipline — [Frontend Masters has a whole TypeScript learning path][ts] for the day reading stops being enough.

> 🤖 **Ask your AI Assistant.** Two-parter. First: paste in a random pet from the pets API (grab one fresh from the console — you know how) and ask "Write a TypeScript type describing this JSON." Compare what comes back to the `Pet` type above — you just watched documentation get generated from a shape. Then flip it: paste any TypeScript snippet from this lesson and ask "Convert this to plain JavaScript." Look at what got _removed_ — labels — and what survived — everything. That survival is the lesson: under every TypeScript file is a JavaScript file you can already read.

## Recap

- TypeScript is JavaScript with labels on the boxes. The colon reads as "and it's a"; the program underneath is unchanged.
- The squint rule: ignore colon-to-punctuation and what's left is JavaScript.
- `type`/`interface` blocks describe data shapes — documentation that's mechanically prevented from lying, and the first thing worth reading in a strange codebase.
- Angle brackets read as "of": `useState<Pet[]>` is a useState of Pet-arrays.
- Agents default to TypeScript because the type checker is a tireless proofreader for a tireless intern — you get the benefit without writing a label yourself.
- "Type X is not assignable to type Y" joins the famous errors: wrong kind of thing, labeled box, caught before it ran. Paste it and the line it points at; no error genre resolves faster.

Next: the biggest reading upgrade in this section — what all those `.tsx` files actually are, and why there's HTML living inside the JavaScript.

[ts]: https://holt.fyi/ts
