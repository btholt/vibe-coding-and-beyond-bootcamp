---
description: "A bonus build for the ambitious: a weather dashboard in React, featuring the course's first chained API calls, a controlled search input, and a lookup-table puzzle. Different muscles than pet-swiping — search, forms, and requests that feed each other."
---

Entirely optional, entirely on your own time, and entirely worth it if you've got the appetite: a weather dashboard. Type a city, get the forecast. It sounds like less app than pet tinder, and it's more — because it exercises a set of muscles the pet saga never touched: a search box (your first controlled input in the wild), an API where one request's answer feeds the next request (your first _chain_), and a response that arrives in code numbers your app has to translate into human words. These three patterns are everywhere in real apps, and none of them have come up in a project yet. That's why this doc exists.

The data comes from [Open-Meteo][openmeteo], which is a great free API for getting weather data (and frequently used as an example app.)

## Rehearse the chain — by hand

Console rehearsal, as always — but this API needs _two_ rehearsals, because the app you're building performs a two-step. Step one, geocoding — turning a city name into coordinates:

```javascript
const response = await fetch(
  "https://geocoding-api.open-meteo.com/v1/search?name=Seattle&count=5",
);
const data = await response.json();
console.log(data);
```

Quick vocabulary while you're looking at that URL: everything after the `?` is **query parameters** — the URL's own little key-value pairs (`name` is Seattle, `count` is 5, joined by `&`). You've been _seeing_ these in URLs your whole life; now you know they're the request's arguments, riding along in the address. This API does all its configuration this way.

Read the shape that comes back: a `results` array of candidate cities, each with `latitude` and `longitude`. Write down the dot-path. Now step two — take those coordinates and ask for actual weather, _by hand_:

```javascript
const response2 = await fetch(
  "https://api.open-meteo.com/v1/forecast?latitude=47.6&longitude=-122.33&current=temperature_2m,weather_code&daily=temperature_2m_max,temperature_2m_min,weather_code&timezone=auto",
);
const weather = await response2.json();
console.log(weather);
```

Feel what you just did: you were the chain. Request one answered a question ("where is Seattle?") that request two needed asked-and-answered before it could even be formed. Your app is going to automate exactly this — search result's coordinates flowing into the forecast request — and having performed it manually, you'll recognize the plumbing instantly when you read it in code. This request-feeds-request shape is enormously common (log in, _then_ fetch the user's data; find the record, _then_ fetch its details), and this is its teaching debut.

One more thing to notice in the forecast response before you leave the console: the temperatures are numbers, fine — but the _weather_ is also a number. `weather_code: 3`. Which brings us to the puzzle.

[See the NOAA table of codes][noaa] if you want to see more, but your agent will know these (or find them.)

## The lookup puzzle

Open-Meteo describes conditions in WMO weather codes — an international standard where `0` is clear sky, `3` is overcast, `61` is rain, `71` is snow, `95` is a thunderstorm, and so on. Your app has to translate number to words (and, if you're having fun, to an emoji — `71` is begging to be 🌨️). That translation is a **lookup**: given a code, produce a label. You've read this pattern before — the calculator's `flushOperation` was a lookup wearing an if-chain — and your agent will likely reach for the other classic costume, an object with the codes as keys. Either way, when the generated code contains a big block mapping numbers to descriptions, you'll know exactly what it is and why it exists: the API speaks in codes; humans don't.

## The spec

Yours to write, and by now I'll just list what it should decide, not how to phrase it: a search input for the city; what happens with multiple matches (show the top few to pick from, or just take the first — your call, but _make_ the call); a current-conditions display plus a few days of forecast, with condition words or emoji via the lookup; **loading states for both hops** — the search wait and the forecast wait are separate gaps, and you know gaps are real now; an error state for a city that doesn't exist (rehearse it: geocode `asdfghjk` and look at what comes back — that's the shape your app has to survive); the attribution line in the footer; and the stack and anti-goals from the React pet tinder, unchanged — React with Vite, one page, no routing, no state or component libraries.

Repo, commit, prompt. You know the liturgy.

## Reading duties

- **Find the chain in the code.** Somewhere, the geocoding response's coordinates flow into the forecast request — find the exact line where data from await number one enters the URL of await number two (odds are it's a template literal building that URL, which should feel like meeting two old friends at once).
- **Read the search box as a controlled input.** There's the loop from the React lesson, in the wild: `value` tied to state, `onChange` feeding the setter. Confirm the loop is closed — and appreciate that you can now diagnose the frozen-input bug this pattern risks.
- **Find the lookup** and skim its coverage: what does it do with a code it doesn't recognize? (If the answer is "crash" or "show undefined," that's a worthy follow-up prompt — you know what `undefined` on screen means and how it got there.)
- **Check your units.** Look at the temperature your app shows and ask whether it's the unit you actually want — this API speaks Celsius unless asked otherwise. If it's wrong for you, don't touch the code: prompt the fix, then read the diff and find _where the change landed_. Spoiler: it's one query parameter, and there's something quietly profound in seeing a user-facing feature turn out to be four characters in a URL.

> 🤖 **Ask your AI Assistant.** Edge-case interrogation, the skill this project adds to your kit: ask "What happens in this app if the user searches for a city while the previous search is still loading? Walk me through it in this code." Then ask "What _should_ happen?" The answers may differ, and the gap between them is where real apps get their polish — you're learning to ask about the moments _between_ the happy paths, which is where an enormous share of actual bugs live. Whether you prompt a fix or just file the knowledge, you've done something most people never do with generated code: asked what it does when things get weird.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">A QA engineer walks into a bar. Orders a beer. Orders 0 beers. Orders 99999999999 beers. Orders a lizard. Orders -1 beers. Orders a ueicbksjdhd. <br><br>First real customer walks in and asks where the bathroom is. The bar bursts into flames, killing everyone.</p>&mdash; Brenan Keller (@brenankeller) <a href="https://x.com/brenankeller/status/1068615953989087232?ref_src=twsrc%5Etfw">November 30, 2018</a></blockquote> <script async src="https://platform.x.com/widgets.js" charset="utf-8"></script>

Ship it to your notes, commit it, and — as with everything you've built — keep it. A weather app that talks to two real endpoints, translates an international meteorological standard, and handles its own loading and error states is not a toy; it's a portfolio piece with a build step. You made that on your own time, for fun. Take the win.

Weather data by [Open-Meteo][openmeteo] (CC BY 4.0) — see, the attribution's easy.

[openmeteo]: https://open-meteo.com/
[noaa]: https://www.nodc.noaa.gov/archive/arc0021/0002199/1.1/data/0-data/HTML/WMO-CODE/WMO4677.HTM
