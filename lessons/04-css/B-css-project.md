---
title: "CSS Project"
---

You now have a blog with some structure and a working conceptual model of CSS. Time to put them together. In this lesson, we're going to dress up the blog you wrote by hand a few lessons back.

We're not going to give you a specific design to copy. This is your blog — make it look like you. The goal is to end this section with a personal blog you'd actually be happy to share the URL of. Something that looks intentional.

## Setup

If you haven't already, in your blog folder, create a file called `styles.css` sitting right next to your `index.html` and `about.html`. Then link it from the `<head>` of both HTML files:

```html
<link rel="stylesheet" href="styles.css" />
```

Save both HTML files. When you refresh the browser now, nothing looks different — that's because your `styles.css` is empty. Let's fix that.

## What to build

Here are your requirements. They're intentionally loose. Read them as "your blog needs to look like these things are true" rather than "do these specific things."

- **A font that isn't Times New Roman.** Pick something readable. If you want to use a Google Font (see the tips below), go for it.
- **A reasonable color palette.** At minimum: a background color, a text color, and one accent color for links and highlights. It doesn't have to be fancy — a plain white background with dark text and one accent color is a real design.
- **Room to breathe.** Nothing should be jammed against the edge of the browser. Use padding on the body and margin between paragraphs.
- **A max-width for readable line length.** Long lines of text are hard to read. Constrain the main content to something like `max-width: 700px;` and center it.
- **Distinct blog posts.** Each of your five posts should feel like its own thing — a border, some vertical space between them, a distinct heading style, whatever you want.
- **Styled headings.** Your `h1`, `h2`, and `h3` should look intentional. Different sizes, weights, maybe colors.
- **Styled links.** The default blue-underlined link is fine, but not great. Pick a link color that fits your palette.
- **A navigation area** on both pages so a reader can get between index and about without hitting the browser back button.
- **Style the about page too.** Same `styles.css`, applied to both pages. That's the whole point of an external stylesheet.

Bonus points:

- A responsive design that works on your phone. Open the page on your phone and see how it looks.
- Hover styles on your links.
- A subtle box shadow on your posts to make them feel like cards. It's called `box-shadow`.
- Rounded corners somewhere. It's called `border-radius`.

## Tips and hints

**Use a Google Font.** Web fonts are one of the highest-impact-per-effort things you can do to make a page feel less generic. Head to [Google Fonts][google-fonts], pick one you like, and follow their embed instructions. It's typically two steps: a `<link>` tag in the head of your HTML, and a `font-family` value in your CSS. If you don't want to browse, some good starters are Inter, Merriweather, Work Sans, and Playfair Display.

**Stealing colors is fine.** You are not being graded on originality. If you see a color palette you like on another site, grab the hex codes with a browser extension. If you'd rather generate one, [Coolors][coolors] is a nice tool for that.

**Use DevTools to experiment.** In your browser, right-click and pick "Inspect." You can edit CSS live in the DevTools panel and see the change instantly. Nothing gets saved to your file, but it's the fastest way to try a hundred different `padding` values in ten seconds. Once you like what you see, copy the value into your `styles.css` for real.

**One selector at a time.** Don't try to write all your CSS at once. Start with the `body`. Get the font, background, and text color right. Then style your headings. Then your paragraphs. Then work through each section. Iterating is faster than trying to picture the whole thing in your head.

**Semantic HTML helps here.** If your blog is a bunch of `<div>`s, styling gets harder. If it's `<article>`s and `<section>`s and `<nav>`s, you have obvious selectors to use. If your HTML from the last chapter used generic divs, this is a good moment to go back and tighten it up.

## Show me what you did

There's no single right answer. If your blog looks intentional, is readable, and shows a bit of thought, you've nailed it. Take a screenshot when you're done — you'll want to compare it to what your blog looks like at the end of the course when we've added some real polish.

If you want inspiration, browse a few personal blogs from developers you like. You'll notice most of them are pretty simple. A good font, a comfortable line length, real spacing, one accent color. That's most of what makes a page feel considered.

Next chapter, we're going to leave the visual world behind and start looking at what actually makes web pages _do_ things: JavaScript.

[google-fonts]: https://fonts.google.com
[coolors]: https://coolors.co
