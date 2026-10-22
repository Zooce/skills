---
name: first-principles-design
description: >
  Software is data and the functions that transform it. Use before any design, coding, or code review, in any language.
when-to-use: >
  Any design, coding, or code review. Writing code, editing code, refactoring, planning a code change, adding or splitting types, functions, or files, reviewing a diff, or when the user mentions changeability, design for change, data-oriented design, first-principles design, decoupling, or /first-principles-design.
---

# First-principles design

Software is fundamentally a set of data transformation routines.

There is a clear distinction between data and the transformations on that data. Data is the values. Functions perform the transformations.

This is true for virtually every programming language and domain.

## Thinking

Start from what the program is: the values and the functions. Every other choice follows from those.

Name the values that go in and the values that come out. Those values are what the program works with.

A record is a set of named values. Many of the same record go in a list. Point at another record with an id. Pick among those three by how the values are used.

Name each function by what it takes and what it returns or changes. Pass in every value it reads or changes. If a value is part of the work, it is an argument.

Keep two steps in one function when both use values that no other code sees. Split when a caller can use one result and never the other. The cut is in the data.

A write takes the data and where it is written. A send takes the message and where it is sent.

When the work is on many records, pass the list. When the work is on one record, pass that record. When the work depends on the case, put a kind on the value. One function branches on the kind. How many, and which case, come from the values.

A function keeps a value only when that function uses it. Put values next to each other when one function uses them together.

Need another input? Add a parameter. More than one step? Call functions and pass the values.

Files and folders organize the code. Split a file when the smaller piece is easier to find.
