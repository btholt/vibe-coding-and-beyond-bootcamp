---
title: "More HTML Tags"
---

In the last lesson you wrote a webpage by hand and opened it in your browser. The HTML we wrote was tiny — a heading, a paragraph, a list. But you'll notice the file already had some weird-looking tags at the top: `<!DOCTYPE html>`, `<html>`, `<head>`, `<body>`. We mostly glossed over those. Let's circle back and talk about what those actually are, and then we'll tour through the rest of the most common HTML tags so when you crack open an agent-generated file you can recognize what you're looking at.

The goal of this lesson isn't to make you fluent in HTML. The goal is to give you enough HTML that you can read an `.html` file (or a `.jsx` or `.tsx` file later) and have a working idea of what each tag is doing. Recognition, not authorship.

## The wrapper tags

Here's the skeleton you typed last lesson, with a couple of additions you'll see in basically every HTML file you ever open:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My First Webpage</title>
  </head>
  <body>
    <!-- all your visible content goes here -->
  </body>
</html>
```

Let's go top to bottom and pick these apart.

- `<!DOCTYPE html>` — There have been several versions of HTML over the years. The current one (which is just called HTML, but you'll sometimes see it referred to as "HTML5" since it was the fifth major revision) came out in 2008 and nothing new is brewing. This line just tells the browser "I'm using the modern version of HTML." Always put it at the top. Don't think about it.
- `<html lang="en">` — The absolute outermost tag of the document. Everything else lives inside this. The `lang="en"` tells the browser, search engines, and screen readers what language the page is in — English here. If you were writing a Japanese site you'd write `lang="ja"`.
- `<head>` — The head contains all the _meta data_ about the page — stuff that isn't directly displayed but tells the browser (and search engines, and link previews on social media) what this page is about. Title, character encoding, viewport behavior, links to CSS, links to JavaScript — all here. Nothing visible to the user ever lives in the head.
- `<meta charset="UTF-8" />` — Tells the browser your document uses the UTF-8 character set, which is what lets you use non-English alphabets, emojis, accented letters, and so on. Just always include this and don't think about it.
- `<meta name="viewport" content="..." />` — Tells mobile browsers how to handle the page. Without it, your phone will render a weird zoomed-out version of your site that's hard to read. Just always include this and don't think about it.
- `<title>` — What shows up on the browser tab and as the headline in Google search results.
- `<body>` — Where everything that's actually _visible_ to the user goes. Your headings, paragraphs, images, buttons, all of it.

This wrapper is so boilerplate that it's basically identical in every HTML file ever written. Every framework — React, Next.js, whatever the agent generated for you — ultimately produces one of these for the browser to consume, even if you never write it yourself. Now you know what it is when you see it.

> Notice the `<!-- ... -->` syntax I used inside the body? That's an HTML comment. The browser ignores anything between `<!--` and `-->`. People use comments to leave notes for themselves and other developers. You'll see them in agent-generated code all the time.

## A whirlwind tour of common tags

Now let's run through the tags you'll see most often inside the body. I'm not going to be exhaustive — there are over 100 HTML tags and you'd lose your mind trying to memorize them all. These ones cover maybe 90% of what you'll encounter.

**Headings — `<h1>` through `<h6>`.** Six levels of headings, with `<h1>` being the most important and `<h6>` being the least. Browsers render them in decreasing size by default, but the visual size isn't the point — it's the _hierarchy_. Screen readers use heading levels to help blind users navigate. Search engines use them to figure out what your page is about. Some people will tell you each page should only have one `<h1>`. I'm of the opinion that you should use them as feels appropriate.

**Paragraphs — `<p>`.** A block of text. The browser puts some space around paragraphs by default to separate them visually. Only text (and inline tags like `<em>` and `<span>`) goes inside a `<p>`.

**Anchors — `<a>`.** A link. Every `<a>` needs an `href` attribute that says where the link goes. Looks like `<a href="https://frontendmasters.com">Frontend Masters</a>`. If the `href` starts with `http://` or `https://` it's a full URL that goes anywhere on the web. If it starts with `/` it's a link to somewhere on the same site.

**Emphasis — `<em>` and `<strong>`.** Both add emphasis to text inline. `<em>` traditionally renders as italics, `<strong>` as bold, but again, the visual is secondary. Semantically, `<em>` means "emphasized" and `<strong>` means "this is important." Use these inside paragraphs to make a word or phrase stand out.

**Span — `<span>`.** A generic inline container for small bits of text. It doesn't change anything by itself, but it lets you attach styles or behavior to a chunk of text inside a paragraph. Think of `<span>` like a Ziploc bag: a wrapper around a few words you want to treat as a unit. We'll come back to it in a bit.

**Images — `<img />`.** Self-closing. Two attributes you'll see on basically every image tag: `src` (where the image lives) and `alt` (a text description of the image for screen readers and for when the image fails to load). Looks like `<img src="puppy.jpg" alt="a very good dog" />`.

**Buttons — `<button>`.** Looks like `<button>Click me</button>`. A button doesn't do anything on its own — JavaScript or a form needs to be wired up to make it actually respond. Think of it like a doorbell: you can put one on your wall, but if it's not connected to the buzzer, pressing it doesn't ring anything.

**Inputs — `<input />`.** Self-closing. The browser already has a bunch of built-in input types: text, number, email, date, color, checkbox, radio, file. You'll see `<input type="text" />` and `<input type="email" />` constantly in agent-generated forms. They do a surprising amount of work for you for free.

**Lists — `<ul>`, `<ol>`, and `<li>`.** Both `<ul>` and `<ol>` represent lists, and both contain `<li>` (list item) tags. A `<ul>` is an _unordered_ list — bullet points, where the order doesn't really matter (teams in a league, characters in a show). An `<ol>` is an _ordered_ list — numbered, where the order does matter (the ten most populous cities, the steps in a recipe). Same children, different markers.

**Tables — `<table>`, `<tr>`, `<td>`.** Tables are for _tabular data_ — the kind of thing you'd put in a spreadsheet. The `<table>` is the whole table. Each `<tr>` is a row. Each `<td>` is one cell in the row. You'll also see `<th>` for header cells and `<thead>` / `<tbody>` to group rows. Some old tutorials will tell you to never use tables — that's because back in the dark ages people used them to lay out entire web pages, which was awful. But for actual tables of data, they're exactly what you want.

**Forms — `<form>`.** A container for input tags that gets used to gather data from a user. The form tag itself doesn't show anything; it just groups the inputs together so they can be submitted as a unit. You'll see forms wrapping a bunch of `<input />`s and a `<button>`.

There are many, many more tags. Audio players, video players, progress bars, expandable details/summary sections, dialogs, all sorts of stuff. But honestly, this list covers the great majority of what you'll see day to day. If you encounter something weird — `<details>`, `<picture>`, `<dialog>` — just Google it or ask Copilot. The browser has a name for just about everything.

## div, section, article, and semantic HTML

There's one more tag worth talking about because you'll see it _everywhere_: `<div>`.

A `<div>` is a container. It doesn't have any inherent meaning — it's just a cardboard box for grouping other stuff together. You'll see HTML files with `<div>`s nested five or six deep. They're the workhorse of HTML layout. (If `<div>` is a cardboard box, `<span>` from earlier is the Ziploc bag — same idea, but for grouping small inline things instead of bigger blocks.)

Here's the thing about `<div>`s: they're _too generic_. When you're reading code with twenty nested `<div>`s, you have no idea what any of them are without reading the contents. So HTML has evolved to give you more specific options. Here are the ones you'll see most:

- `<header>` — The header of a page or a section. Usually has the logo, the navigation, the page title.
- `<nav>` — A navigation bar.
- `<main>` — The main content of the page. There should be exactly one of these per page.
- `<section>` — A self-contained section of a page. Like a chapter.
- `<article>` — A self-contained piece of content that could stand on its own — a blog post, a news article, a social media post, a product listing.
- `<aside>` — Sidebar content, or anything tangential to the main content.
- `<footer>` — The footer of a page or a section.

All of these are functionally identical to `<div>` from the browser's perspective. They render the same. They behave the same. The difference is that they _tell you what they are_. When you (or an agent, or another developer) read `<article class="blog-post">`, you instantly know you're looking at a self-contained piece of content. When you read `<div class="blog-post">`, you have to figure that out from context.

This idea is called **semantic HTML** — using tags that describe what the content _is_, not just where it sits on the page. A few reasons it matters:

- **Screen readers** and other assistive tech use semantic tags to help users navigate. A blind user can jump straight to `<nav>`, or skip past it to get to `<main>`.
- **Search engines** use them to understand the structure of your content. An `<article>` gets different treatment than a random `<div>`.
- **You and your collaborators** can read the code faster. The next person to open the file — including future you, including an AI agent — has more context to work with.

Don't be a purist about it. A `<div>` is fine when you really do just need a generic container, and you'll see plenty of them in real code (and in agent-generated code). But when you see `<article>` or `<section>` or `<nav>`, you now know they're not exotic — they're just `<div>`s with a job title.

That rounds out HTML enough for our purposes. In the next lesson we'll add some style to all of this with CSS.
