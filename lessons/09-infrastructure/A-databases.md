---
description: "Your API forgot everything when it restarted; here's the fix. Brian covers what a database conceptually is, SQLite versus the server-shaped databases like Postgres, just enough SQL to recognize it, Drizzle as your ORM of choice, and the thirty-second version of migrations."
---

You felt the problem personally: you stopped your API, started it again, and everything anyone had posted was gone — because your data lived in a variable, and variables live in a process, and processes end. Every real app has the same problem, and they all solve it the same way: a **database** — a program whose entire job is remembering, safely, permanently, and in a way that's fast to search even when there are millions of things to remember.

The mental model that serves best is a spreadsheet. A database is like one spreadsheet _file_; inside it, a **table** is like one of the sheet tabs; each **row** is one record — one pet, one user, one order — and each **column** is one field, with a name and a type at the top. Your pets, as a table:

```
pets
id | name    | breed     | age
---+---------+-----------+----
1  | Luna    | Havanese  | 14
2  | Biscuit | Corgi     | 3
3  | Mochi   | Shiba Inu | 5
```

If you ever used Microsoft Access back in the day, this is going to feel like a reunion — that's genuinely what Access was, a database wearing office clothes. The difference between this and an actual spreadsheet is what surrounds the grid: instead of scrolling and clicking, you extract data with specialized _queries_ — precise, repeatable questions like "every pet older than four, sorted by name" — and the database answers them instantly whether the table holds ten rows or ten million.

## A file or a server?

Here's a fact that should feel familiar: **most databases are shaped exactly like the app you just built.** They're server programs — they start, refuse to end, and sit listening for requests; your app connects and sends queries the way Bruno sent requests to your API. Postgres, the database you'll most likely end up with in production (MySQL is another perfectly valid pick from the same family), works precisely this way: a never-ending program, usually on its own machine, answering questions over a connection.

We're starting you somewhere deliberately simpler: **SQLite**, a database that isn't a server at all. It's a library that reads and writes _one file_ on disk — your entire database is a single file sitting in your project folder, no second program to run, no connection to configure, nothing to install beyond the package. It is the easiest possible on-ramp, it's a real database used in serious software (it's in your phone right now, many times over), and for learning and local projects it's perfect. The one thing to know — and you don't need to care deeply yet — is that you normally won't _go to production_ with it, precisely because it doesn't run as a server that many app instances can share (unless you use a server-flavored take on it, like Turso). To be clear about how to hold this: SQLite is where we're _starting_ because it's the gentlest introduction, not step one of a two-step you should plan on — swapping databases in the middle of a project means changing SQL dialects mid-flight, which is a genuine pain I don't wish on you. The honest arc is: learn on SQLite now, and once databases feel comfortable, just _start_ your production-bound projects on Postgres from day one. If SQLite charms you — it charms a lot of people — my [SQLite course][sqlite] goes properly deep.

## Reading SQL, not writing it

The queries themselves are written in **SQL** — Structured Query Language — and consistent with everything in this course, your goal is to _recognize and read_ it, not author it. The two operations that cover most of life, and most of what agents generate:

Putting a row in:

```sql
INSERT INTO pets (name, breed, age) VALUES ('Luna', 'Havanese', 14);
```

Getting rows out:

```sql
SELECT name, breed, age FROM pets WHERE age > 4;
```

Notice you can basically just... read those. That's by design — SQL was built in the 1970s with the explicit dream that non-programmers could use it, and the dream half-worked: it reads like stilted English. `INSERT INTO` this table, these columns, these `VALUES` — and look at the SELECT: the same column names again, this time as _which fields to give back_. `SELECT` these columns `FROM` this table `WHERE` this condition holds — and that `WHERE` clause is a comparison, the boolean-question shape you've known since the JavaScript section. (Since we asked for every column the table has, we could have written the shorthand `SELECT * FROM pets` — `*` means "all the columns," and you'll see it constantly.) When a wall of SQL scrolls past in generated code or an agent's explanation, read it as English with capital letters and you'll get remarkably far.

One warning for anyone who does end up typing SQL by hand: the grammar is _strict about order_. JavaScript is often forgiving about how you arrange things — declare your functions above or below where they're used, put statements in whatever order makes sense to you. SQL is not like that: the clauses must come in their prescribed sequence — `SELECT`, then `FROM`, then `WHERE` — and writing `FROM pets SELECT name` gets you a syntax error, not partial credit. It reads like English, but it's English with an extremely rigid word order. Reading, this never matters (the order you'll see is always the legal one); authoring, it's the first thing that bites people. That's the depth we need; when you want the real discipline — and it's a genuinely valuable one, especially with Postgres — my [SQL course][sql] is the deep end.

> Bigger picture, filed for completeness: SQL databases are one _flavor_ — the dominant one, and the right default — but other kinds exist with different strengths: databases organized around documents, key-value pairs, graphs of relationships. Different problems, different shapes of memory. If you're curious how the whole landscape fits together, my [Complete Intro to Databases][dbs] is the tour.

## Drizzle: the database from JavaScript

Your app, of course, is JavaScript — so how does JavaScript talk to a SQL database? You _can_ send raw SQL strings from Node, but the standard move in generated code is an **ORM** — it stands for Object-Relational Mapper, because people always ask, and the name is honest for once: it maps between your code's _objects_ and the database's _relational_ tables, letting you query in the language you're already in. There are several ORMs in the ecosystem; the one I recommend, and the one you'll use in this section's project, is **Drizzle** — modern, popular with agents, and unusually readable, which is the quality this course prizes above all.

There's a deeper reason Drizzle is ideal for the agent era specifically, and it's a pattern you've seen before: with Drizzle, the entire structure of your database lives _in code, as types_. That schema file below isn't just documentation — it's TypeScript, which means the tireless proofreader from the TypeScript lesson now checks database code too. If the agent writes a query asking for a column that doesn't exist, or tries to stuff a string into a number field, the type checker flags it _the moment it's typed_, before anything ever touches the database. Your agent gets instant feedback on whether its database code is correct for _your_ tables — the same write-check-fix loop that makes agents fast at TypeScript, extended all the way down to the data. A tireless intern with a proofreader who has memorized your filing system.

Two pieces of Drizzle to learn on sight. First, the **schema** — the file where your tables are defined _in TypeScript_:

```typescript
export const pets = sqliteTable("pets", {
  id: integer("id").primaryKey(),
  name: text("name").notNull(),
  breed: text("breed"),
  age: integer("age"),
});
```

Read it with your types-as-documentation eyes, because that's exactly what it is: the shape of the data, field by field, type by type — except this documentation is enforced by the _database_ itself. When you open a generated project and want to know what data it keeps, the schema file is the single highest-value read in the codebase — it's the app's memory, described in one place. (`primaryKey` marks the id column as each row's unique name tag — the same job `key` did in your React lists — and `notNull` means "this field is required.")

Second, the queries, which read like sentences assembled from dots:

```typescript
const allPets = await db.select().from(pets);
const grownups = await db.select().from(pets).where(gt(pets.age, 4));
await db.insert(pets).values({ name: "Biscuit", breed: "Corgi", age: 3 });
```

Squint and you can see the SQL underneath — `select from where`, `insert values` — because that's literally what Drizzle does: builds the SQL for you. And notice every one of those lines starts with `await`. Databases live on disks and across networks; talking to them takes time, so **database calls are async, always** — which means your triage signal just got a second meaning. In server code, `async` marks where the internet happens _or_ where the database happens, and either way it marks where the interesting data comes from.

> One more shape to recognize, because you're about to see it everywhere: **tables can reference other tables.** The mechanism is humble — a column that holds another table's id — but it's how databases model everything connected: a `likes` table with a `user_id` column and a `pet_id` column records _which user liked which pet_, each row a thread between two other tables. In a schema you'll see it as something like `userId: integer("user_id").references(() => users.id)`, which reads exactly like it sounds. This referencing is the whole reason these are called _relational_ databases — the R in ORM — and it's why one app's data is usually several tables holding hands rather than one giant grid. Reading move: when you meet a new schema, the `.references()` lines are the wiring diagram — they tell you how the app's nouns connect. (And yes: "each user's liked pets, remembered per-user" is a shape you'll be needing very shortly.)

## Migrations, in thirty seconds

One more thing you'll see in generated projects, kept as brief as I can make it because it's confusing and you mostly just need to not be alarmed by it. Your tables' shapes change over time — the app grows, and `pets` needs a new `photo` column. Changing a table's shape _while keeping the existing data and everyone's copy of the database consistent_ is a genuinely fiddly problem, and the tooling solution is **migrations**: small, versioned change-scripts for the database's shape, applied in order, so any copy of the database can be walked from any old shape to the current one. Drizzle generates and runs these for you. Reading-level takeaway, complete: a `migrations` folder in a project is _the version history of the database's shape_, agents manage it, and when an agent says "I've created a migration," it means "I changed what the tables look like and wrote it down properly." That's it. Moving on.

## Someone else runs it

Last piece of the map. Just as you didn't rack a server in your closet to host an API, most teams don't run their production database themselves — they use a **managed database**: Postgres (usually), operated by a cloud provider who handles the machines, the backups, the security patching, and the 3 a.m. problems, while you get a connection string and go build your app. Full disclosure of bias: this is literally what I work on — my day job is a Microsoft-managed Postgres service on Azure — and it's a market I've seen from several sides over my career; every major cloud runs one, and there's a healthy field of independents. For you, the takeaway is the shape of the arrangement: production databases are usually _rented, not run_, your app knows its database only as a `DATABASE_URL` in the environment — a connection string, which you can now recognize as a credential from the secrets lesson, gitignored like one — and the SQLite file in your project folder is the local, zero-ceremony stand-in for all of that while you build.

> 🤖 **Ask your AI Assistant.** The translation exercise, one more time, because it demystified TypeScript and it'll demystify this: paste the Drizzle queries from this lesson and ask "Show me the exact SQL each of these produces." Confirm the squint — `select from where` was in there all along, and the ORM is a translator, not a mystery. Then the read that pays forward: paste the schema and ask "Describe this app's data in plain English, and what's one column you'd expect this app to need that's missing?" You're practicing the schema-first read — the single fastest way to understand what any app actually _is_ — and auditing whether the assistant's suggestion makes sense is a preview of every schema conversation you'll ever have with an agent.

## Recap

- A database is a program whose job is remembering — permanently, searchably, at any scale. Spreadsheet model: database = the file, table = a sheet, rows are records, columns are typed fields.
- Most databases are server-shaped, like the API you built: always running, answering queries over a connection. Postgres is the production default (MySQL is fine too); SQLite is the deliberately simpler on-ramp — a real database in a single file, perfect for learning and local projects. Don't plan on swapping databases mid-project (changing SQL dialects mid-flight is a pain); learn on SQLite, then start production-bound projects on Postgres from day one.
- SQL reads like stilted English by design: `INSERT INTO ... VALUES ...`, `SELECT ... FROM ... WHERE ...` (and `*` is shorthand for "all the columns"). Recognize it; don't sweat authoring it.
- Drizzle is your ORM — a translator from JavaScript to SQL. The schema file is the app's memory described in one place, the highest-value read in any codebase; queries are await-ed sentences of dots. `async` now marks the database, too.
- A migrations folder is the version history of the database's shape. Agents manage it. Don't be alarmed. That's the whole lesson on migrations.
- Production databases are usually rented, not run — a managed Postgres and a `DATABASE_URL`, which is a credential and lives in the environment.

Next: the other thing your assistant said your API was missing — who, exactly, is allowed to DELETE?

[sqlite]: https://master.dev/courses/sqlite/
[sql]: https://master.dev/courses/sql/
[dbs]: https://master.dev/courses/databases-v2/
