---
title: "HTML"
---

Alright, enough tour, let's actually write something. Anywhere you'd like — your Desktop is fine — create a new folder called `hello-html`. Open that folder in VS Code via **File → Open Folder**, or just drag the folder onto the VS Code icon.

Now create a new file in that folder called `index.html`. You can do this in VS Code by clicking the "new file" icon at the top of the file tree, or by right-clicking in the tree and picking **New File**.

Type this into the file:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My First Webpage</title>
  </head>
  <body>
    <h1>Hi, I'm a webpage</h1>
    <p>My author wrote me by hand. Can you believe it?</p>
    <ul>
      <li>I'm a list item</li>
      <li>So am I</li>
      <li>And me too</li>
    </ul>
  </body>
</html>
```

Save it with `Cmd+S` on Mac or `Ctrl+S` on Windows.

That's it. That's a webpage.

> There's a lot of initial structure to get the HTML doc working e.g. the `<head>` tag, the `<body>` tag, etc. – you can just ignore those for now.

Before we look at it, notice that VS Code was doing things for you while you typed. It probably colored your tags, indented when you hit enter, and maybe auto-closed some tags. That's the editor helping out. It's not magic, it's just a feature, and we'll lean on more of these as we go.

## Let's open it in a browser

Find the `index.html` file you just made. Double-click it.

It opens in your browser. You should see a webpage with your heading, your paragraph, and your list.

Congratulations — you wrote a website. Right-click the page and pick **View Page Source** (or **View Source**) and you'll see the exact same HTML you typed. There's nothing else there. No magic, no server, no agent. Just the file you wrote.

> Try this: change the text in your `<h1>`, save the file in VS Code, and then refresh your browser. You should see the change. This is the inner loop of frontend development — edit, save, refresh, repeat. We'll automate the refresh later, but this is the foundation.

## Chat a bit about what is actually happening and what HTML is

So what just happened?

HTML stands for HyperText Markup Language. The name is older than I am and basically nobody says the full thing day to day. What matters is what it does: HTML is how you tell a web browser what each piece of content on a page is. That `<h1>` you wrote tells the browser "this is a top-level heading." The `<p>` says "this is a paragraph." The `<ul>` and `<li>` together say "this is a list with items in it." That's it. HTML is a vocabulary of tags that label your content.

Think of HTML sorta like Word when you can see the formatting. Instead of seeing **bold text**, you just see it written `<bold>bold text</bold>` where what's between the `<>` describes what sort of text it is.

It's even more flexible than that because you say things like

```html
<article>
  <title>My Article Title</title>
  <paragraph>The body of text of my article</paragraph>
</article>
```

> I used `<paragraph>` to make it extra clear here, but the actual tag is `<p>`. You kinda just have to learn what each of them stand for, or at least know how to look them up.

See? These **tags** (as they're called) just describe what's inside of them, and they can be nested. This is the code that gets shipped to your browser to describe what text goes on the page.

The browser is the thing doing all the work. It reads your HTML file top to bottom, figures out what each tag means, and renders it. Headings get bigger and bold. Paragraphs get reasonable spacing. Lists get bullet points. None of that styling came from you — it came from the browser's built-in defaults. We'll change those defaults with CSS in a later lesson.

A few things I want you to notice:

- **HTML is just text.** It's a text file. You could write HTML in Notepad, in TextEdit, on a napkin you photograph and OCR. VS Code makes it nicer to write but it's not required. The browser doesn't care where the text came from.
- **Tags come in pairs.** Open with `<h1>`, close with `</h1>`. The slash means "close." Whatever is between them is the content of that tag. A few tags don't have content and don't need a closing tag (like `<img />` for images), but most do.
- **Tags can contain other tags.** Your `<body>` contains an `<h1>`, a `<p>`, and a `<ul>`. The `<ul>` contains three `<li>` tags. This nesting is how you build up a document.
- **The browser is forgiving.** If you mess up the HTML, the browser will usually try to render _something_ anyway. That's great when you're learning. It also hides bugs. We'll get into reading other people's HTML later in the course.

Now go back and look at the Replit project you built earlier. Open one of the files — it might be a `.html` file, or you might see `.jsx` or `.tsx` files, which we'll cover next. You'll see a lot more tags than you wrote here, and some of them will look weirder. But the foundation is the same: tags labeling content, nested inside other tags, all sitting in a text file. The agent didn't write something secret. It wrote HTML.

That's where we're going to keep building from. In the next lesson we'll add some CSS to make this look like something, and then we'll start poking at the JavaScript that makes things actually do stuff.
