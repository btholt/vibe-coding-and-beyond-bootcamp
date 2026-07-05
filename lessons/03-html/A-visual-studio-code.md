If you haven't already, go install [Visual Studio Code](https://code.visualstudio.com). We are going to start writing some code together!

Welcome back. Hopefully you came out of the earlier part of the course with something cool shipped, a few bruises from the spots where the agent didn't quite get what you wanted, and a starting feel for what it's like to prompt your way to a working app. Now we're going to switch gears. The next few days are about peeking under the hood of what the agent has been writing for you, and that starts with us doing a little of it ourselves.

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
