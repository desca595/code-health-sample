# The code-health rules page

One standalone page, `index.html`, that draws whatever `rules.json` beside it says: a title, tags, then one row per rule with its name, a line on why, the code that fails and the code that passes. Give an AI this file and the sample `rules.json`, ask for a JSON of the same shape about your own rules, and drop it in.

## Open it

- From any web server: put `index.html`, `tokens.css` and your `rules.json` in one folder. Another file: `index.html?src=other.json`.
- From the disk (`file://`): a browser will not let a page read a file by itself, so the page asks you to choose or drop the JSON.

## The JSON

```json
{
  "eyebrow": "Code health",
  "title": "Code stays small and used",
  "description": "One or two sentences under the title.",
  "tags": [{ "label": "Blocks", "tone": "good" }, { "label": "On push", "tone": "info" }],
  "labels": { "fail": "Fails", "pass": "Passes" },
  "rules": [
    {
      "name": "A file stays short",
      "limit": "≤ 500 lines",
      "description": "Why the rule exists, in a line.",
      "fail": {
        "file": "store/InventoryView.tsx",
        "lines": [{ "code": "export default InventoryView;", "note": "812 lines", "mark": "bad" }],
        "message": "✗ what the check prints"
      },
      "pass": {
        "file": "store/inventory/",
        "lines": [{ "code": "Filters.tsx", "note": "142 lines", "mark": "ok" }]
      }
    }
  ]
}
```

- `tone` (a tag's colour): `good`, `info`, `warn`, `danger` or `neutral`.
- `mark` (a line's ground): `bad` red, `ok` green, or left out.
- `note` is drawn after the line as a grey `// comment`; a line with only a note is a comment line, an empty line is a gap.
- `message` (either side) is the line the check prints, under the code.
- `labels` names the two sides (default Fails and Passes).
- Any number of rules. Every word is text from the JSON, never HTML.

## Its look

`tokens.css` holds the colours, sizes and type, light theme first, and is generated from the design tokens; never edit it by hand. For dark, change `data-ds-theme="light"` on `<body>` to `dark`.
