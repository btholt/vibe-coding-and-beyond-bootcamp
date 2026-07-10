For the ambitious, the itch gets scratched: you know the word-of-the-day endpoint, you know the validate-word endpoint, you've POSTed to it with your own hands. You have everything required to prompt a full Wordle clone — six guesses, five letters, green/yellow/gray tiles, real dictionary validation via my API.

Fair warning: it's a genuinely meatier build, which makes the _reading_ meatier too, and that's the actual assignment. Spec it with the same anti-goals, then earn it: find the state (where do the guesses live? what tracks the current row?), trace one keypress end to end, find the POST that validates a guess and watch it in the Network tab, and ask one why-question about the letter-coloring logic — duplicate letters make it sneakier than it looks, and "why is this comparison done in two passes?" tends to unlock a genuinely interesting answer.

> Want the artisanal version? Building Wordle from scratch, by hand, every line yours, is the capstone project of my [Complete Intro to Web Dev v3][webdev] — free to watch, and a great "what does authoring actually feel like" follow-up once this course has you reading fluently.

[webdev]: https://holt.fyi/web-dev
