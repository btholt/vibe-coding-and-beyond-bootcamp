If you haven't already, go install [Visual Studio Code](https://code.visualstudio.com). We are going to start writing some code together!

Welcome back. Hopefully you came out of yesterday with something cool shipped, a few bruises from the spots where the agent didn't quite get what you wanted, and a starting feel for what it's like to prompt your way to a working app. Now we're going to switch gears. The next few days are about peeking under the hood of what the agent has been writing for you, and that starts with us doing a little of it ourselves.

Don't panic. We're going to write maybe a dozen lines of code in this lesson. The point isn't to turn you into a frontend engineer in fifteen minutes — the point is to demystify what's in those files Replit has been generating. By the end of this lesson you'll have written a webpage by hand, opened it in your browser, and we'll have chatted about what HTML actually is. Then we keep building.

Go grab VS Code from [code.visualstudio.com](https://code.visualstudio.com) if you haven't already. Yes, Microsoft makes it. Yes, I work at Microsoft. No, I won't be offended if you want to use something else later in the course — there are great alternatives — but for now let's all be on the same page.

## A quick tour of what VS Code can do

Open up VS Code. It might look intimidating at first because there's a lot going on, but you've already been working in Replit's IDE for a day, and the good news is VS Code is the same shape of thing. They both have:

- A **file tree** on the left where you see your project's files.
- An **editor pane** in the middle where you actually look at and edit code.
- A **terminal** (Replit calls it Shell) where you can type commands directly to your computer.
- A **search** that lets you find anything in your project.

The big difference is that VS Code runs locally on your computer, against files on your computer, using your computer's terminal. Whatever you do here, you do for real. No sandbox. That's both the power and the responsibility, and we're going to use that power throughout the course.

A few things in VS Code I want to point at now:

- **The command palette.** Hit `Cmd+Shift+P` on Mac or `Ctrl+Shift+P` on Windows. This is the search-for-everything bar. Want to format your file? Open a setting? Run an extension command? Always start here. I use this hundreds of times a day, no exaggeration.
- **Extensions.** The square icon on the left sidebar. This is where you add features to VS Code — things like GitHub Copilot, themes, language support, the works. We'll come back here in the next lesson to install Copilot.
- **The integrated terminal.** Open it with `Ctrl+` `` ` `` (that's the backtick, usually above Tab on US keyboards). Same idea as Replit's Shell, just on your machine. We'll use this later.

> Don't try to memorize all of this. Just know it exists. You'll get muscle memory for the bits you actually use, and the rest you can Google or ask Copilot about when you need it.

## Let's write a little HTML

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

Now go back and look at the Replit project you built yesterday. Open one of the files — it might be a `.html` file, or you might see `.jsx` or `.tsx` files, which we'll cover next. You'll see a lot more tags than you wrote here, and some of them will look weirder. But the foundation is the same: tags labeling content, nested inside other tags, all sitting in a text file. The agent didn't write something secret. It wrote HTML.

That's where we're going to keep building from. In the next lesson we'll add some CSS to make this look like something, and then we'll start poking at the JavaScript that makes things actually do stuff.
