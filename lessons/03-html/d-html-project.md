---
title: "HTML Project"
---

We've spent the last while looking at HTML — writing some, touring the tags. Before we move on to CSS, there's one more conceptual thing I want to sit on, because it informs a lot of what comes next: HTML is _static_.

By static I mean: the HTML file you wrote is just a file. It sits on your computer. When you double-click it, the browser opens it, reads it top to bottom, and renders what it sees. That's it. The file doesn't know who you are. It doesn't know what time it is. It doesn't have access to a database. It can't change itself based on who's looking at it. It's a document, the same way a Word doc is a document.

That's surprisingly powerful, and also surprisingly limited. Powerful because you can write one HTML file, throw it on a web server, and it'll serve infinitely fast to as many people as want to look at it, costing you basically nothing. Limited because if you want anything that _changes_ — a list of blog posts pulled from a database, a profile page that says hi to the user by name — you need more than HTML. That's where servers, JavaScript, and databases come in. We'll get to those.

For now, the mental model is simple: **one HTML file = one webpage.** Your `index.html` is one page. If you want another page, you make another HTML file.

## Adding more pages

Let's say you want a second page on your site — an "about" page that's separate from your home page. The way you do that is exactly what it sounds like. You make another HTML file, sitting right next to your `index.html`.

In your `hello-html` folder from the previous lesson, create a new file called `about.html`. Put some basic HTML in it:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>About Me</title>
  </head>
  <body>
    <h1>About Me</h1>
    <p>Hi, I'm a person on the internet.</p>
  </body>
</html>
```

Save it. If you double-click `about.html`, it opens in your browser — same as `index.html` did. It's just another file in the same folder, and the browser doesn't care which one you open.

Now we need a way to get from `index.html` to `about.html` without typing the file path into the address bar like a maniac. That's what links are for. Open up your `index.html` and add a link inside the body:

```html
<a href="about.html">About me</a>
```

Save, refresh `index.html` in your browser, and click the link. Boom — you're on the about page.

A few things worth noticing:

- The `href` is just `about.html`, not a full URL with `https://` and everything. That's called a _relative path_ — it means "look for a file with this name, in the same folder as the current page." Relative paths are how most links inside your own site will work.
- The browser didn't do anything magical. It just loaded a different HTML file. The two pages are completely separate documents that don't know about each other.
- If you want a link back from `about.html` to `index.html`, you'd add `<a href="index.html">Home</a>` over there.

That's the whole model. A multi-page site is a folder of HTML files that link to each other. That's how the web worked for the first decade of its existence and it still works fine for a huge number of real websites today.

> When you eventually use a framework like Next.js — or whatever the agent ships you — "pages" are still a thing. You'll see files in a `pages/` or `app/` folder and each one becomes a page on the site. The framework does fancier stuff in the middle (rendering on the server, sharing layouts between pages, putting data into the page before it loads) but the mental model of "each page will be a file" is still is pretty close to true.

## Your first project: a blog

Time to put this all together. We're going to make your personal blog. Use the `hello-html` folder from last lesson, or start a new one — your call.

Here are your requirements:

- Two pages: `index.html` and `about.html`.
- Your `index.html` needs a title at the top and a quick intro paragraph.
- Your `index.html` needs a link to `about.html`.
- Your blog must have **five posts** on it. They can all live on `index.html` — no need to break them into separate files.
- Each post needs:
  - A title
  - An author (you, presumably)
  - The date it was posted (feel free to make these up)
  - The body of the post. Bonus points for multiple paragraphs. Feel free to use [Lorem Ipsum](https://loremipsum.io) if you want placeholder text.
  - Bonus points for images.
- Your `about.html` needs:
  - The name of your blog
  - A few paragraphs about you
  - A list of your recent jobs, schools, or accomplishments
- Your `about.html` should also link back to `index.html` so people can get home.

Quick note on images: if you want to include one, drop the image file into the same folder as your HTML files, and then reference it like this: `<img src="my-photo.jpg" alt="a text description of the image for sight-impaired folks" />`. Match the filename exactly — `my-photo.jpg` won't find `MyPhoto.JPG`.

This is going to look mostly black and white and that's okay. We'll come back and dress it up with CSS in the next chapter.

When you finish, you'll have written, by hand, a small two-page website. No agent, no framework, no magic. That's a meaningful thing to have done.
