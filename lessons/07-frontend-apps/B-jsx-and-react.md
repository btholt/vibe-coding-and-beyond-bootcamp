---
title: "JSX and React"
description: "The biggest reading upgrade in the course: what React is, why every generated frontend uses it, and how everything in it — components, props, useState, onClick — is something you already know wearing a new costume."
---

Prompt an agent to build you almost any app and what comes back is React. Not sometimes — overwhelmingly. React is the dominant dialect of generated frontend code, which makes this lesson the single biggest reading upgrade in the course: after it, the wall of `.tsx` files in a generated project stops being a wall.

> Real talk before we start: this lesson is _dense_, on purpose, and I do not expect you to walk away from it able to write React. Nobody could — React is a deep subject, and I have over fifteen hours of recorded content on it between my [Complete Intro to React][react] and [Intermediate React][intermediate] courses, which exist for exactly that. The goal here is different and much more achievable: to read _enough_ React to understand what your agent hands you, and to ask it intelligent questions about what you see. Recognition, not fluency — if a concept doesn't fully click on the first pass, that's the expected experience, not a failure. Keep reading; it all gets re-encountered with your own app shortly.

Here's the good news, and I mean this literally: **you have already built React from scratch.** You just called it something else. Think back to the calculator: three state variables at the top, handler functions that changed the state, and a `rerender()` function that made the screen match. State changes, screen follows. You built that pattern with your bare hands, which means you understand the problem React exists to solve — because the calculator only worked since `rerender` was _one line_. One thing on screen ever changed. Now imagine the pet app, the chat app, the dashboard: dozens of things on screen, each depending on different pieces of state, all needing to stay in sync as state changes. Hand-writing the "make the screen match" code for that is miserable and bug-prone, and _that misery is React's entire reason to exist_. React's deal with you: **describe what the screen should look like for any given state, and React figures out the re-rendering.** You write the "what"; React does the "when" and "how."

## JSX: the script writes itself

To describe what the screen should look like, React uses a syntax that will look genuinely illegal the first time you see it:

```jsx
function PetCard() {
  return (
    <div className="card">
      <h2>Biscuit</h2>
      <p>Golden Retriever, 12</p>
    </div>
  );
}
```

That's HTML. Inside JavaScript. Being _returned from a function_. This dialect is called **JSX**, and it's what the `x` in all those `.tsx` files stands for — TypeScript plus JSX, the two dialects agents love most, sharing a file extension.

Why would anyone glue these two languages together? Because of what React asks you to do: describe what the screen should look like. Your description of a screen naturally _is_ markup — that's what markup is for — but which markup, with what filled in, depends on JavaScript values. So JSX puts the description where the logic lives: a function runs, and it returns the markup for the current state of the world. Different state, different markup, and React reconciles the page to match. It's the DOM lesson's "find and change" verbs industrialized — you stopped hand-modifying the page; now you describe pages and React does the modifying.

Two reading rules make JSX legible immediately:

**Rule one: curly braces are a hatch back to JavaScript.** Inside JSX, `{}` means "evaluate this as JavaScript and drop the result in right here":

```jsx
function PetCard() {
  const pet = { name: "Biscuit", breed: "Golden Retriever", age: 12 };
  return (
    <div className="card">
      <h2>{pet.name}</h2>
      <p>
        {pet.breed}, {pet.age}
      </p>
    </div>
  );
}
```

If `{pet.name}` gives you template-literal déjà vu, trust it — this is `${}` without the dollar sign, fill-in-the-blank markup instead of fill-in-the-blank strings. Same idea, new address.

**Rule two: it's almost HTML, with a few renames.** The visible one: `className` instead of `class`, because `class` already means something in JavaScript. And a callback you were promised: remember how HTML shrugged about the slash on self-closing tags? **JSX has opinions.** Every tag must close — `<img />` with the slash, always, or it's a hard error. Old HTML shrugs; JSX does not.

One honest note before we go further: unlike every snippet so far in this course, you _can't_ paste JSX into the console — the browser has no idea what this syntax is. Something has to translate it first, and that mystery — which has now come up with TypeScript, with module-flavored `await`, and again here — gets fully resolved at the end of this section. Let it accumulate a little longer.

## Components: set pieces with capital letters

That `PetCard` function is a **component**: a function that returns JSX. Components are the set pieces of React — build the "card" piece once, use it everywhere, and when the design changes you change it in one place. A React app is components inside components inside components, all the way up to one root component that is the whole app.

And here's the reading signal, one of the best in this whole course: **capitalization tells you what's whose.** In JSX, `<div>` — lowercase — is a plain HTML tag, the ones you toured ages ago. `<PetCard />` — Capitalized — is _someone's component_, defined somewhere in this codebase as a function by that exact name. When you're reading a generated file and hit a Capitalized tag, that's your cue: this thing has a definition, and finding it (VS Code: right-click, Go to Definition) is how you walk deeper into the app. Lowercase is vocabulary; Capitalized is a door.

## Props: attributes that are secretly arguments

A card that only ever shows Biscuit isn't a set piece; it's a portrait. Components take inputs, passed as attributes and received as parameters:

```jsx
function PetCard(props) {
  return (
    <div className="card">
      <h2>{props.name}</h2>
      <p>{props.breed}, {props.age}</p>
    </div>
  );
}

// used elsewhere, likely many times:
<PetCard name="Biscuit" breed="Golden Retriever" age={12} />
<PetCard name="Mochi" breed="Shiba Inu" age={5} />
```

These are called **props**, and the reading translation is direct: _attributes on the tag become an object in the function's parameter._ `name="Biscuit"` on the tag; `props.name` inside. It's a function call wearing HTML's clothes — because that's literally what it is. (The `{12}` is rule one again: a hatch into JavaScript, because `age="12"` would be the string.) You'll also constantly see the parameter written as `function PetCard({ name, breed, age })` — that's _destructuring_, a shortcut that unpacks the object right in the parentheses, and agents use it reflexively. Read it as "grab these compartments out of props" and move on.

## useState: buffer, reborn

Now the load-bearing one. State in React looks like this:

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>Liked {count} times</button>
  );
}
```

Translate `const [count, setCount] = useState(0)` through the calculator: this is `let buffer = "0"` _and_ `rerender()` fused into one purchase. You get back a pair — the current value, and a setter function — and the deal is absolute: **you never assign to state directly; you call the setter, and the setter re-renders.** `setCount(5)` is `buffer = "5"; rerender();` from your calculator, except React wrote the rerender and it updates everything on screen that depends on the value, however many places that is. That's the misery, deleted.

This gives you the React version of the file-reading strategy you learned on the calculator. Back then: state at the top, wiring at the bottom, trace an interaction through the middle. In React: **find the `useState` lines and you've found the component's memory.** Everything else in the component exists to display that state or change it through setters. Different architecture, same first move.

(Seeing `useState<number>(0)` in TypeScript projects? You can already read those angle brackets: a useState _of_ numbers.)

And look at that `onClick` while we're here — `addEventListener` in a costume. CamelCase attribute, and inside the braces, a function being _handed over, not called_ — the recipe-for-later shape you've known since the functions lesson. Same bouncer, new uniform.

## Inputs: the state loop, closed

One more useState pattern, because every generated app with a search box, a form, or a text field uses it, and it looks like a riddle until someone says it out loud:

```jsx
function Search() {
  const [query, setQuery] = useState("");

  return (
    <input value={query} onChange={(event) => setQuery(event.target.value)} />
  );
}
```

Read the two attributes as a loop. `value={query}` says the input _displays_ the state — not whatever was typed, the state. `onChange` says every keystroke calls the setter with what the user typed (`event.target` — the thing that changed, an old friend from the calculator's listener). So: key pressed → setter called → state updates → re-render → input displays the new state. The screen you see went all the way around through React.

This is called a **controlled input** — the state owns the input, rather than the input owning itself like it did in your Mad Libs. Why the ceremony? Because now the typed value lives in state, where the rest of the component can react to it as it changes — filter a list on every keystroke, validate as they type, enable the button only when the field's filled. And here's the reading payoff, plus a bug you can now diagnose on sight: an input with a `value` but _no_ `onChange` is frozen solid — it displays state nothing ever updates, so typing does literally nothing. When an agent-built form mysteriously won't accept input, this is almost always why.

## Lists: the map payoff

Way back in the loops lesson I waved at `.map()` and said "a loop in a different outfit, we'll get there." We're there, because this next line is — no exaggeration — the most common pattern in all of generated React:

```jsx
function PetList({ pets }) {
  return (
    <div>
      {pets.map((pet) => (
        <PetCard key={pet.id} name={pet.name} breed={pet.breed} age={pet.age} />
      ))}
    </div>
  );
}
```

Read it with everything you have: a hatch into JavaScript, and inside it, `pets.map(...)` — _for each pet, make a PetCard_. An array of data becomes an array of components, and React renders them all. Every list you have ever seen in a modern app — every feed, every search result, every chat history — is this exact line with different nouns. When you rebuild the pet app in React shortly, this line is where your pets will live.

That `key={pet.id}` is a name tag React demands on list items so it can track them between re-renders efficiently. Agents include it automatically; if one forgets, you get a console warning — `Warning: Each child in a list should have a unique "key" prop` — which hereby joins your famous-errors catalog as the gentlest member: a warning, not a crash, and the fix is always "give each item its id."

## What you'll see that we're not covering

Generated React contains more React than this — most visibly `useEffect(...)`, which you can read as "render first, so the user sees something — then go do this side-task." Fetching a component's initial data from an API is the classic use, and it's why you'll almost always find a fetch living inside one. There's real depth there, and it is exactly the depth this survey is designed to skip. One more sight worth a single line: in older codebases you may meet `class Something extends React.Component` — those are class components, the pre-2019 way of writing what you now know as component functions. Same idea, older costume; agents don't write new ones, but the old ones are everywhere and nothing about your reading strategy changes. When reading fluency stops being enough and you want to _build_ with this stuff properly, my [Complete Intro to React][react] is the deep end — this lesson is roughly its first day, compressed and pointed at reading.

> 🤖 **Ask your AI Assistant.** An architecture read, to set up the coming project: paste your vanilla pet app's `script.js` in and ask, "If this were rebuilt in React, what components would you break it into, and what state would each hold? Just names and responsibilities — no code." Read the answer with this lesson's eyes: can you predict which parts of your current file become which components? Where does your pet array end up — whose `useState` is it? You're practicing reading an app's _architecture_ before its code exists, which is precisely the position you'll be in constantly as a person who prompts for a living.

## Recap

- React solves the problem you met personally in the calculator: keeping the screen in sync with state. You describe the "what"; React does the re-rendering.
- JSX is HTML inside JavaScript — the description of the screen living where the logic lives. Curly braces are a hatch back to JavaScript; `{pet.name}` is `${}` energy without the dollar sign.
- JSX demands closed tags — the self-closing slash stopped being optional — and says `className` where HTML said `class`.
- Components are functions returning JSX. Lowercase tags are vocabulary; Capitalized tags are doors — someone defined that, and you can Go to Definition through it.
- Props are function arguments wearing attribute clothes. Destructured parameters read as "grab these compartments."
- `useState` is your calculator's state variable and `rerender()` fused: never assign, always call the setter, and the setter repaints. Find the useStates, find the memory.
- `pets.map((pet) => <PetCard ... />)` is the most common line in generated frontends: for each item, a component. `key` is its name tag; the missing-key warning is the friendliest famous error.
- `onClick` is addEventListener in a costume, still taking a recipe-for-later.
- Controlled inputs close the loop: `value` displays state, `onChange` updates it, the re-render brings it back around. An input with a `value` and no `onChange` is frozen — a bug you can now spot on sight.

Next: making all of this beautiful without writing a stylesheet — the utility-class approach your generated apps almost certainly already use.

[react]: https://master.dev/courses/complete-react-v9/
[intermediate]: https://master.dev/courses/intermediate-react-v6/
