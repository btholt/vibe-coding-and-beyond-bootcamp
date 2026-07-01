Now that you've hand-written some HTML and CSS, let's do something you couldn't do a week ago: prompt an AI to generate some code, and actually understand what it hands back.

If you don't have GitHub Copilot enabled in VS Code yet, take a minute to do that now. Open the Extensions panel (the square icon on the left sidebar), search for "GitHub Copilot," and install it. Sign in with your GitHub account when prompted — the free tier is generous and works fine for what we're doing here.

Copilot has two main ways to help. **Inline suggestions** appear as you type; you accept them with Tab or ignore them. **Copilot Chat** opens a side panel where you can have a conversation and ask it to generate whole files. We're going to use the chat for this exercise.

## The prompt

Open Copilot Chat and paste in this prompt:

> I want to code a very basic calculator that looks like the iOS calculator. I want you to only use two files, an HTML file and a CSS file. I don't want you to code any JavaScript yet. Please generate me the CSS and HTML for this project.

Let it run. You should get back an HTML file and a CSS file for a calculator that looks a lot like the one on your phone. Save the two files into a new folder, open the HTML in your browser, and take a look. It'll probably be pretty close on the first shot.

## Why this prompt works

Nothing about this prompt is magic. But every word in it is doing work. Let's pull it apart:

- **"a very basic calculator"** — clear goal. Not a spreadsheet, not a scientific calculator, not a graphing tool. A simple calculator.
- **"that looks like the iOS calculator"** — a reference model. The agent knows what the iOS calculator looks like, and now it has a design target. This is worth infinite words of description.
- **"only use two files, an HTML file and a CSS file"** — a structural constraint. Without this, the agent might reach for a framework, a build tool, and seventeen npm dependencies. Two files means two files.
- **"I don't want you to code any JavaScript yet"** — an anti-goal. You're explicitly telling the agent what _not_ to do. This is one of the highest-leverage prompt patterns you'll ever learn — most bad AI output comes from the AI doing more than you asked for.
- **"Please generate me the CSS and HTML"** — a clear ask for the deliverable. You're not asking a question; you're placing an order.

Notice what this prompt _isn't_. It isn't fifty lines. It isn't a formal spec. It isn't any of the fancy prompting frameworks. But it has a goal, a reference, constraints, an anti-goal, and a clear ask — and that turns out to be enough for a huge chunk of what you'll build with AI.

## Your turn

Now poke at it. Try a few variations:

- **Change the reference.** "Looks like a 1970s Braun calculator." "Looks like the Windows XP calculator." "Looks like a Casio watch from 1989." See how much the visual design changes when you swap that one phrase.
- **Change the constraint.** "Use flexbox for the layout." "Use CSS Grid for the button grid." "Use only inline styles instead of an external CSS file." Watch the underlying code structure change.
- **Change the scope.** Ask for a very basic to-do list instead. A simple color picker. A pomodoro timer countdown. Keep the "no JavaScript" constraint on so they stay static.

For each one, look at what the agent generated. You can now _read_ the HTML and CSS it wrote. You know what the tags mean. You know what the selectors are doing. You know whether it's using semantic HTML or a pile of divs. You know whether the CSS is well-organized or a mess.

That's the whole point of the last several lessons. You went from "the agent's output is a black box" to "the agent's output is code I can inspect." When it does something you don't like, you can find the file, find the offending line, and change it yourself — or prompt the agent to fix it in a targeted way instead of "just fix it please."

In the next chapter we'll get into JavaScript, so that when the agent adds behavior to your projects, you can supervise that too.
