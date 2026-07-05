---
description: "The section finale where every hang-on-until-later gets answered: what a build step is, why generated projects all have one, src for humans versus dist for browsers, and the tells that let you recognize — and start — any built project you're handed."
---

Several times now I've shown you something, admitted the browser can't actually run it, and asked you to hang on until later. TypeScript — browsers don't know what a type label is. JSX — HTML inside JavaScript is a syntax error. Tailwind's stylesheet — doesn't exist until something generates it. Those module-flavored floating `await`s. It's later. All four of those hanging questions have the same one-word answer, and it's the reason generated apps work at all: something between the code your agent writes and the code your browser runs has been quietly translating this whole time.

That something is the **build step**, and the concept is beautifully simple once you connect it to a principle from your very first JavaScript lesson: _code is for humans first and computers second._ The build step is that principle made physical. A built project has two versions of itself: the `src` folder — the source, written in the human-friendly dialects, the one you and your agent read and edit — and the built output, translated and compressed into the plain JavaScript and CSS the browser actually speaks. Humans work on one side of the machine; browsers are served from the other. Every one of those hanging questions was just a dialect that lives on the human side and gets translated before shipping.

If it feels strange that we'd deliberately write in languages the browser can't run, notice that programming has _always_ worked exactly this way. Your computer's processor doesn't speak JavaScript — deep down, it speaks something like this:

```
01001000 01100101 01101100 01101100 01101111
```

(That says "Hello," for what it's worth.) Nobody writes that. Ones and zeros are for the machine, so humanity invented programming languages for the people and translators to bridge the gap — vastly easier to write, to read, to maintain, and the machine never knows the difference. TypeScript and JSX are the same trade, one level up: dialects that are better to author in, translated down to the basic JavaScript that every browser on earth speaks. You get the best of both worlds — humans write in the good language, browsers receive the universal one, and the build step is the bridge between them.

## Watch one get born

You are not going to be hand-configuring build tools in this course — this is a watch-me lesson, though you're welcome to follow along. The tool of the moment is called **Vite** (French for "fast," pronounced "veet"), and it's what agents overwhelmingly reach for. Creating a project is one command:

```bash
npm create vite@latest
```

It asks a few questions — project name, which framework (React), which dialect (TypeScript) — and scaffolds a folder. Then two more commands: `npm install` (hold that thought for a moment) and `npm run dev`, which produces terminal output like:

```
  VITE v6.0.3  ready in 312 ms

  ➜  Local:   http://localhost:5173/
```

Get comfortable reading little terminal reports like this one, because your agent generates them constantly and they're usually friendlier than they look: the tool announces itself, says it's ready, and hands you a URL. (If the terminal itself still feels like hostile territory, my [Complete Intro to Linux and the Command-Line][linux] turns it into home turf — worth it for anyone planning to spend real time around code.) That URL is a **dev server** — a tiny local web server running the translation machinery _live_. Open it, and there's the app. Edit a file in `src`, and here's the magic worth seeing once: the browser updates _the instant you save_, no reload. That's called hot reloading, and it's a huge piece of why agents iterate so fast — the write-look-adjust loop runs in milliseconds.

So while you're working: `npm run dev`, leave it running, and the translator handles everything on the fly. Which raises the question of what happens when you're _done_ working.

## Dev is for working; build is for shipping

The other command is `npm run build`, and it runs the translation once, completely, producing a folder — conventionally `dist`, for distribution. Open a file in there and behold:

```javascript
(function(){const t=document.createElement("div");t.className="flex items-center gap-4"...
```

...one enormous line of gibberish. That's your readable code after the translator ate it: TypeScript labels stripped, JSX converted to plain ol' JavaScript, Tailwind's stylesheet generated with only the utilities you used, everything squished into as few bytes as possible. It's unreadable _on purpose_ — browsers don't benefit from readability, only humans do, and every space and good variable name costs download time. `src` is for humans; `dist` is for machines; never the twain shall be edited by you. (If you ever catch yourself — or an agent — editing a file in `dist`, something has gone philosophically wrong. Changes go in `src`; the machine regenerates the rest.)

Deploying an app, it turns out, is mostly "run the build, put `dist` on a server" — which is exactly what you'll do when the capstone ships. And one more promise from earlier gets kept here: remember that TypeScript's proofreader can _block a build_? This is where. A type error means `npm run build` refuses to produce output — the machine declining to ship a known mistake. When an agent says "the build is failing," this machinery is what it means, and the terminal output names the file and line every time.

## The tells of a built project

Here's this lesson's core reading skill: recognizing a built project on sight, because every project your agent generates from now on will be one. Clone anything, open it in VS Code, and check for the tells:

**`package.json`** — the project's ID card, and the first file to read in _any_ JavaScript project. It's JSON (you read JSON) with three sections that matter: the name, the `scripts` (the project's named commands — this is where `dev` and `build` are defined, and reading this section answers "how do I start this thing" for any project on earth), and `dependencies`. Dependencies deserve a real beat: these are usually open-source projects that someone else made and shared with the world, and that you get to stand on for free. React is one — a project, made by people, that millions of apps depend on. So are Vite and Tailwind; you've already used three dependencies without ceremony. These projects usually live on GitHub (the same GitHub your repos live on — open source is just repos other people published), and `package.json` is the **manifest**: exactly which projects, at exactly which versions, this app depends on. When your agent "adds a library," this list is what grew.

**`node_modules`** — the folder with forty thousand files in it. If `package.json` is the manifest, this is the warehouse: the _actual code_ of every dependency, pulled down from where it's hosted so it's on your machine and usable — dependencies-of-dependencies and all the way down, which is where the forty thousand files come from. Rules of engagement: **these are to be used, never modified.** Any edit you make in here gets blown away the next time anyone runs `npm install`, because the whole folder is disposable by design — `npm install` reads the manifest and rebuilds the warehouse from scratch, which is exactly what that command was doing earlier. Never commit it to git either (it's gigantic, and regenerable beats stored). And never be afraid of it: deleted node_modules? Weird errors? `npm install` again. It's the "have you tried turning it off and on" of JavaScript, and it legitimately works.

**`src/`** — the human side: your `.tsx` files, your components, the code you and your agent actually touch. One honesty note here — `src` is a _convention_, not a law. Somebody chose that name; agents just happen to choose it very frequently. You'll also meet projects where it's `app/` or `frontend/` or `client/`, and that's fine — the layout of a project is up to you and your agent. The pattern to recognize isn't the name; it's the role: _the folder where the human-dialect code lives_, whatever someone decided to call it.

**Config files in the root** — `vite.config.ts`, `tsconfig.json`, maybe an `eslint` something. These are the machine's settings. Recognition-level rule: they exist, agents configure them, you read them only when an error points there.

**A nearly empty `index.html`** — open it and find almost nothing: a root div and one script tag pointing into `src`. After the care you put into your blog's HTML this feels wrong, but it's the React model made visible — the markup isn't written in advance; it's produced by your components at runtime. The HTML file is just the stage the app gets mounted on.

Put together, the tells give you the **universal onboarding recipe** for any JavaScript project you're ever handed: clone it, `npm install`, open `package.json`, read the scripts, run the one called `dev` (occasionally `start`), and read the terminal for the URL. That five-step recipe works on effectively every modern project in existence, including everything your agent will ever generate. You'll use it in the very next lesson.

## npm, briefly

I've now used npm four times without introducing it, so, briefly: **npm is the app store for code.** A public registry of millions of packages — free, installable chunks of other people's code — plus the command-line tool that installs them. React is a package. Vite is a package. Tailwind is a package. When your agent adds a feature and says "I've added such-and-such library," it edited `package.json` and ran `npm install`, same as you watched. There is a much fuller story here — npm is also how _server-side_ JavaScript gets its powers, and it's coming when we get to servers. For now: app store, `package.json` is the receipt, `node_modules` is the downloads folder.

> One honest closing note for perspective: your calculator and your Mad Libs had none of this. Three files, a script tag, open in browser — and they were _real_. The build step isn't a graduation requirement; it's the price of admission for the fancier dialects, and agents pay it by default because TypeScript, JSX, and Tailwind are worth it to them. Both worlds remain legitimate. Small things are allowed to stay small.

> 🤖 **Ask your AI Assistant.** Take the `package.json` from any generated project (or the Vite one from this lesson) and run the onboarding interview: "Explain each script in this package.json, tell me the difference between dependencies and devDependencies, and tell me exactly what commands to run to get this project running locally." The answer to that last part should match the universal recipe — and now you've practiced the single most useful question to ask about any codebase you've just been handed. Bonus: pick the weirdest-named dependency in the list and ask what it does and whether anything would break without it. Knowing what your project stands on is not paranoia; it's ownership.

## Recap

- The build step is "code is for humans first" made physical: `src` in human dialects, translated output in browser dialects. TypeScript, JSX, Tailwind, and floating awaits all live on the human side of the machine.
- `npm run dev` runs the translator live with hot reload — the working mode. `npm run build` translates once into `dist` — the shipping mode. Never edit the shipped side.
- Type errors can block builds: the proofreader refusing to ship a known mistake. The terminal names the file and line.
- The tells: `package.json` (ID card — read it first, scripts answer "how do I start this"), `node_modules` (regenerable, never edit, never commit), `src/`, config files, a nearly empty `index.html`.
- The universal onboarding recipe: clone, `npm install`, read the scripts, `npm run dev`, read the terminal for the URL.
- npm is the app store for code — fuller story when we get to servers.

Next: the pet app rides again — same app, third form, and this time it's React all the way down.

[linux]: https://master.dev/courses/linux-command-line/
