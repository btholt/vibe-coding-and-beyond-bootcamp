---
title: "A Whirlwind Tour of CSS"
---

We've spent three lessons on HTML. You wrote a blog, and if you opened it in your browser, you noticed it looks like it was made in 1996. Times New Roman, black text on white, everything jammed against the left edge, no room to breathe. That's the browser's default styling — technically correct but visually unpleasant.

CSS is how we fix that. But before we get into the mechanics, here's a mental model for how a whole webpage is put together.

Think of a webpage like a theater production. There are three separable pieces:

- **The script** is the HTML. It's the content — the words being communicated. A table read of the script alone still tells you the story.
- **The set, costumes, and lighting** are the CSS. It's how the production looks — the visuals, the atmosphere, the mood.
- **The actors performing live** are the JavaScript. It's the behavior — how the show responds, how things happen in the moment.

You can pull any of these apart and still have something. A script by itself is a play you can read. A script with staging is a museum diorama — visual but not alive. A full production has all three working together. Web pages are the same. You can serve HTML alone and it works. You can add CSS and it looks good. You add JavaScript and it starts to _do things_.

We finished the script last chapter. Now we're adding the set and lighting.

CSS stands for Cascading Style Sheets. Just like HTML, the name is older than the tech scene and basically nobody says the full expansion in day-to-day work. What CSS actually does: it tells the browser how to _style_ the HTML on your page. Colors, sizes, fonts, spacing, layout — all of it.

Same rules as before: our goal is recognition, not fluency. CSS is enormous — you could spend a whole career becoming great at just CSS. What I want you to leave this lesson with is enough vocabulary that when an agent hands you a CSS file (or a component full of styles), you can read it, change it, and reason about it.

## What CSS is

Like HTML, CSS is not a programming language. It's a list of rules you give the browser. You'll write things like "all `h1`s will be colored red" and the browser will do what you say.

Here's the anatomy of a CSS rule:

```css
h1 {
  color: red;
}
```

That's the whole thing. Let's pull it apart:

- `h1` — the **selector**. This is what you're targeting. Anything that matches this selector gets the rules inside the curly braces applied to it. In this case, every `<h1>` on the page.
- `color` — the **property**. There are about 350 CSS properties. Realistically you'll use fifty or so regularly. This particular one changes the color of text.
- `red` — the **value**. What we want the property to be. CSS understands about 150 named colors and also hex codes (`#ff0000`), rgb (`rgb(255, 0, 0)`), and hsl (`hsl(0, 100%, 50%)`). They all describe the same red. You don't need to know the difference right now — I've been writing CSS for over a decade and I still Google specific hex codes.
- `;` — the semicolon. Every property-value pair ends in one. Think of it as a period at the end of a sentence.

You can have as many rules inside one selector as you want:

```css
h1 {
  color: limegreen;
  font-size: 60px;
  font-weight: normal;
  text-decoration: underline;
}
```

## Attaching CSS to your HTML

There are three ways to get CSS onto your HTML page, and you'll see all three in the wild.

**External stylesheet.** By far the most common. You put your CSS in its own file (usually named `styles.css`) and reference it from your HTML head with a `<link>` tag:

```html
<head>
  <link rel="stylesheet" href="styles.css" />
</head>
```

This is what you should do. It keeps your CSS separate from your HTML, makes it reusable across multiple pages, and is the pattern basically every real website uses.

**Style tag.** You can also put CSS directly inside a `<style>` tag in the head:

```html
<head>
  <style>
    h1 {
      color: red;
    }
  </style>
</head>
```

Fine for tiny examples or one-off pages. Less common in real code.

**Inline style.** You can attach CSS directly to an HTML tag with the `style` attribute:

```html
<h1 style="color: red;">Hello</h1>
```

You'll see this occasionally, especially in agent-generated code where one specific element needs a quick tweak. Not the pattern to reach for by default, but not evil either.

## Selectors

The `h1` selector we've been using selects every h1 on the page. That's often not what you want — usually you want to style just one specific thing. That's what classes are for.

You add a class to an HTML tag with the `class` attribute:

```html
<p class="story-lede">This is the opening paragraph of the post.</p>
```

> TIL that "lede" is what you call the leading bit of a story - e.g. you don't want to "bury the lede".

Then you target that class in your CSS by prefixing the name with a period:

```css
.story-lede {
  font-size: 20px;
  font-weight: bold;
}
```

Only elements with `class="story-lede"` get those styles. You can put the same class on as many elements as you want — that's the whole point.

There's also an `id` attribute that works similarly but is targeted with a `#` and is meant to only appear once per page:

```html
<div id="site-header">...</div>
```

```css
#site-header {
  background-color: black;
}
```

You'll see IDs sometimes, but classes are far more common. When in doubt, use a class.

> A quick word on nesting: if a parent element has a color set, the children inside it will inherit that color by default. So if you set `body { color: navy; }`, every piece of text on the page will be navy unless something more specific overrides it. This is where the "Cascading" in Cascading Style Sheets comes from. It gets more complicated than that — there's a whole set of rules for which style wins when two rules target the same element — but the intuition of "styles flow down from parents to children" gets you a long way. [Complete Intro to Web Dev v3 has a whole lesson on this if you want the full picture][cascade].

## The properties that cover 80% of what you'll see

Here's the shortlist of the properties you'll see over and over. Recognize these and you can read most CSS files.

- **`color`** — text color.
- **`background-color`** — background color of the element.
- **`font-family`** — which typeface to use. You'll see things like `font-family: "Helvetica", sans-serif;`. The comma-separated list is a fallback chain — if the first font isn't available, try the next.
- **`font-size`** — how big the text is. Measured in `px`, `em`, or `rem` most of the time.
- **`font-weight`** — how bold the text is. Common values: `normal`, `bold`, or a number like `400` (normal), `600` (semibold), `700` (bold).
- **`text-align`** — `left`, `right`, `center`, `justify`.
- **`padding`** — space _inside_ the element, between its content and its border.
- **`margin`** — space _outside_ the element, between it and its neighbors.
- **`border`** — a line around the element. Written like `border: 1px solid black;` — width, style, color.
- **`display`** — how the element behaves in the layout. More on this in a minute.

Try changing a few of these on your blog. Add a `<link rel="stylesheet" href="styles.css" />` to the head of your `index.html`, create an empty `styles.css` file right next to it, and put some rules in there. Refresh the browser and watch things change.

## The box model

Every HTML element on your page is a box. Even a `<span>` is a box, just a small one. Every box has four layers, from innermost to outermost:

1. **Content** — the text, image, or whatever is inside the element.
2. **Padding** — space between the content and the border. Set with `padding: 20px;` or specific sides like `padding-top: 10px;`.
3. **Border** — the line around the element. Set with `border: 1px solid black;`.
4. **Margin** — space between this element and its neighbors. Set with `margin: 20px;`.

When you look at a webpage that has good spacing between things, you're mostly looking at the effects of padding and margin. Those two are the workhorse duo for making things look breathable instead of cramped.

> Handy way to see this: open your browser's DevTools (right-click anywhere on the page and pick "Inspect"), select an element in the panel, and look at the box model diagram in the sidebar. The browser shows you content, padding, border, and margin as concentric rectangles. It's an excellent tool for figuring out why something looks off. I use it _constantly_.

## Flexbox

The other thing you'll see all over modern CSS is **flexbox**. Flexbox is how you arrange things next to each other in a row (or a column). Before flexbox existed, laying out a horizontal row of items was an actual nightmare — Google "vertically center CSS" some time and see decades of anguish on StackOverflow. Now it's a few lines.

Here's the pattern. Say you have some HTML like this:

```html
<nav class="menu">
  <a href="/">Home</a>
  <a href="/about">About</a>
  <a href="/contact">Contact</a>
</nav>
```

By default, those links stack sideways in an awkward, cramped way. To lay them out in a nice row with even spacing, put `display: flex` on the parent:

```css
.menu {
  display: flex;
  gap: 20px;
}
```

Two things happened. `display: flex` told the browser "arrange this element's children in a row." `gap: 20px` added 20 pixels of space between each child. That's it — you have a horizontal navigation bar.

Flexbox has a few properties worth knowing:

- **`justify-content`** — how children are spaced along the row. Common values: `flex-start` (default, all pushed to the left), `center` (centered), `space-between` (first item all the way left, last item all the way right, everything else evenly spaced in the middle).
- **`align-items`** — how children are aligned across the row. `center` vertically centers everything, which used to require twelve StackOverflow tabs and now is one line. Count your lucky stars.
- **`flex-direction`** — set to `column` and the row becomes a column instead.
- **`gap`** — space between children.

That's not the whole story of flexbox — there's `flex-wrap`, `flex-grow`, `align-self`, and more — but those four properties will get you through most of what you'll see. If you want to go deep, Jen Kramer's [CSS Grid + Flexbox course on Master.dev][grid] is excellent.

## What we're skipping

CSS is enormous and I've cut a lot. Things you'll bump into eventually that I haven't touched:

- **CSS Grid** — like flexbox but for two-dimensional layouts. Rows _and_ columns at the same time.
- **Positioning** — `position: absolute`, `position: fixed`, and friends for taking elements out of the normal flow.
- **Media queries** — for making the page look different on phones vs. desktops.
- **Animations and transitions** — for making things move.
- **Specificity** — the rules that determine which style wins when two rules target the same element.

You don't need any of these to get through this course. When you run into them in agent-generated code, Google them or ask Copilot. Complete Intro to Web Dev v3 covers all of them in depth if you want to go further.

[cascade]: https://btholt.github.io/complete-intro-to-web-dev-v3/lessons/css/selectors-and-the-cascade
[grid]: https://master.dev/courses/css-grid-flexbox-v2/
