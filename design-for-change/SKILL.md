---
name: design-for-change
description: >
  Apply data-first design so code stays easy to change: understand input and output data shapes before behavior, keep work that must stay together behind small interfaces, and do only the work the problem requires. Use whenever writing, structuring, refactoring, or reviewing code of any size; when adding types, files, APIs, layers, or services; when splitting or merging work; when a function's parameters or locals are growing; or when the user mentions changeability, design for change, data first, isolation of work, necessary responsibility, or runs /design-for-change.
---

# Design for Change

Software changes. Make it easy to change. The small interfaces between parts should stay the same; the work behind them can change.

Use this whenever you write, structure, refactor, or review code, no matter how big or small.

## Order of thinking

Always data first. Then behavior. Then how you split the work.

If you do not understand the data, you do not understand the problem. If you do not understand the problem, you cannot understand a good solution. All software is data transformation, so all software problems are data problems.

Before you add a type, file, layer, API, parameter, or pattern, answer these:

1. What data are we working with? What is the shape of the inputs? What is the shape of the outputs?
2. What data transformations turn those inputs into those outputs? If you only have a step name (parse, validate, send) and cannot say which data it turns into which data, you are not ready to add structure.
3. Which parts care about this data? Those parts should use it directly. Other parts should not pass it along.
4. If you split work apart: what extra coordination did that create? If the coordination costs more than the work, the split is bad.
5. Can this part do its own work without depending on how the rest of the system coordinates it?
6. Does this addition solve a problem you have right now?

If the answer to any of these is no, go back to the data and change the design. Do not keep a bad split and try to save it by passing more data through, adding more parameters, or applying a design pattern.

## Design around data, not behavior

Do not start from steps, services, layers, or methods. Start from the data.

Name the data. Behavior is just how one shape of data becomes another. There are many good ways to do the same transformation. The input and output data change much less often than the how.

How parts nest or call each other is not the same as how data is shared. Do not pass data through parts that do not care about it. The parts that care should share that data directly. The parts in between should keep doing their own jobs.

If a part can be reused with different values, pass those values in as inputs. That is not the same as sharing changing state between parts.

## Isolation of Inseparable Work

Group work that must stay together. Connect those groups with small, specific interfaces. Keep the interfaces. Let the groups change.

A unit of work is a job and the way other jobs talk to it. It is not a file, a line count, or a rule that each piece may only do one thing.

A reasonable starting point is one unit of work per data transformation.

If the coordination between the split parts outweighs the work itself, it is a bad split. Do not split just so each piece has one responsibility. If you then need extra data or a middle piece only to keep them in sync, you split too far.

Keep two transformations in the same unit when the data in the middle is not something you actually care about, and changing one side would always mean changing the other.

Split two transformations when the data in the middle is real, named, and a stable interface.

Give a part of the system the power to do its own work without depending on how the rest of the system coordinates it.

## Necessary Responsibility

Do the work the problem requires. How many "responsibilities" a piece has does not matter. What matters is whether the work is necessary.

Solve the problem directly. Do not add a type, layer, file, or pattern that only sits in front of work you already have, or that solves a problem you do not have right now.

A part that only lays out or connects other parts should not also own and pass around the data those parts share. Layout is one job. The parts that care about the shared data should use it themselves.

## Stop doing these

- **Functions with many parameters, many local variables, or many free variables, and growing.** They are flexible, but they carry so much state they become hard to change. Go back to the data: name the shapes, then split the work around those shapes.
- **Passing data through parts that do not care about it.** Those parts now have to coordinate data that is not their job.
- **A middle piece that holds other jobs' data or does other jobs' work.** Give those jobs the shared data and the power to do the work themselves.
- **Design patterns that do not solve a problem you have right now** (Builder is a common one). Do not add them for a problem you might never have. Many of these patterns come from a time when GUIs were still being figured out. A lot of them are noise today.

## Examples

### Tabs and content

The data that matters is the current content type (one value per kind of content).

- The tabs and the content area care about that value. They share it directly. Clicking a tab sets the value. The content area reads the value and shows the matching content.
- The outer layout does not own the value, does not listen for clicks to change it, and does not pass it through.
- Each tab still takes a label and which content type it stands for as inputs, because the tab is reusable for different labels and types.

You could have the parent own the value, listen for clicks, and switch content. That still works. Focusing on the shared content type gives a better design: layout does layout; tabs and the content area do their own work.

### CSV user import

Data that jumps out: CSV, user, error, audit log, email (a message you send, not an email address).

Behavior that jumps out: parse, validate, write to a database, log, generate email, send email.

That behavior only makes sense as transformations of the data, for example:

- `CSV → users | errors`
- `users → database`
- `users | errors → summary`
- `summary → email` (generate)
- `email → sent` (send)

Do not start with an `import(file)` function on a service that holds the file, the rows, the errors, the created users, and the summary, and does every step. That is designing around the steps. The service has to carry all of that state.

Keep parse and validate as one unit when "parsed row" is not data you care about. Both would have to know every CSV column and every user field, so the extra coordination costs more than the work. Split generate-email and send-email because **email** (the message) is named data. You can change how you write the message without changing how you send it, and the other way around.
