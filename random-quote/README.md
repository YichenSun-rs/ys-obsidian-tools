# Random Quote Module (Templater)

A plug-and-play Templater module that inserts a random quote into any note.

- Reads quotes from a single “quote library” note (you can move the file anywhere in your vault).
- Only extracts **Markdown list items** (`-`, `*`, `+`, or `1.`).
- Supports optional author using: `Quote text -- Author`
  - If no author is provided, it falls back to the quote note name.

## Setup

1. Install and enable the **Templater** plugin in Obsidian.
2. Put the module file into your Templater templates folder:
   - `random-quote-module.md`
3. Create a quote library note (example below).  
   Default file name used by the script: `🕯️引语摘录集`  
   You can change this in the script by editing `baseName`.

## Usage (Plug-and-play)

In any note, place the cursor where you want the quote and run:

- `Templater: Insert template` → choose `random-quote-module`

If you inserted the code but it didn’t execute, run:
- `Templater: Replace templates in the active file`

## Quote Library Format

Write quotes as list items:

```md
- The unexamined life is not worth living. -- Socrates
- Action is better than perfection.
- What does not kill me makes me stronger. -- Nietzsche
