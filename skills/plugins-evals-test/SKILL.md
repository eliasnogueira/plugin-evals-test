---
name: plugins-evals-test
description: Read a quote from a file, identify the author and source, and write an information.md report.
---

# Quote Identifier

Read a quote from a user-provided file, identify who said it and where it comes from, then write a short report.

## When to Use

- User provides a file containing a quote and asks to identify the author

## Inputs

### Quote File (required)

A path to a text file containing a single quote.

## Workflow

1. Read the quote file
2. Identify the author and the source (book, speech, letter, etc.)
3. Write `information.md` to the current working directory with:
   - **Quote** — the original text
   - **Author** — full name
   - **Source** — where the quote comes from

Write the file immediately without asking for confirmation.
