# Contributing

Contributions are welcome. The bar is simple: **every snippet must solve a real, recurring problem** — not a hypothetical one.

## Adding a Snippet

1. Fork the repo and create a branch: `git checkout -b snippet/your-snippet-name`.
2. Add your snippet to `typescript.json`.
3. Update the snippet table and add a detail section in `README.md` (follow the existing format).
4. Open a pull request with a short description of what the snippet generates and when you'd actually use it.

## Snippet Quality Checklist

- **Prefix is short and memorable** — something you'd type without looking it up.
- **Tab stops are ordered logically** — the most important placeholder is `$1`, cursor lands at `$0` in the right place.
- **No unnecessary imports** — only import what the generated code actually uses.
- **Matches the project's style** — `import type` for type-only imports, async/await over callbacks, `??` over `||` for nullish coalescing.
- **Focused on backend TypeScript** — frontend, React, or framework-specific snippets belong in a separate file.

## Snippet Format Reference

```json
"Snippet Name": {
  "prefix": "trigger",
  "body": [
    "line one",
    "line two with ${1:placeholder}",
    "$0"
  ],
  "description": "One-line description shown in VS Code intellisense"
}
```

- Each element in `body` is a line. Use `""` for blank lines.
- Placeholders: `${1:label}` for named tab stops, `$0` for final cursor position.
- Escape literal `$` as `\\$` inside the JSON string.

## Reporting Issues

If a snippet generates incorrect or outdated code, open an issue with the prefix and a description of what's wrong.
