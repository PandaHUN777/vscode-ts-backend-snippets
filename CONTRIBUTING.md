# Contributing

Thanks for your interest in contributing. This project has one rule that shapes everything else: **every snippet must solve a real, recurring problem in Express 5 + TypeScript backends, with safe defaults.** A snippet that saves typing but generates fragile code does more harm than good, because people trust what their editor writes for them.

## Ways to Contribute

- **Report a bug**: a snippet generates code that is incorrect, outdated or unsafe.
- **Improve an existing snippet**: better defaults, clearer tab stops, a missed edge case.
- **Add a new snippet**: see the [open issues](../../issues) first. Items from the README's Planned Additions are tracked there, and issues labelled `good first issue` are a good place to start.
- **Improve the docs**: unclear explanations, missing context, typos.

For a new snippet that isn't already an issue, please open an issue first describing the problem it solves and when you'd reach for it. That way we can agree on the approach before you spend time on it.

## Naming Scheme

Every prefix follows `xp-<category>-<variant>`, so typing a category lists the whole group:

| Category   | Examples                                 |
| ---------- | ---------------------------------------- |
| `xp-mw`    | `xp-mw-auth`, `xp-mw-error`, `xp-mw-log` |
| `xp-error` | `xp-error-base`, `xp-error-not-found`    |
| `xp-test`  | `xp-test-unit`, `xp-test-api`            |

Top-level building blocks without variants use a single word: `xp-app`, `xp-server`, `xp-route`, `xp-controller`, `xp-service`. If your snippet doesn't fit an existing category, suggest a new one in the issue.

## Adding or Changing a Snippet

1. Fork the repo and create a branch from `main`, for example `snippet/xp-mw-validate` or `fix/xp-mw-auth-header-parsing`.
2. Run `npm install` (Node 22 or later).
3. Add or edit the snippet in `typescript.json`. Start the `description` with the prefix, for example `"xp-mw-log: request logger middleware..."`, to match the existing entries.
4. Add the snippet's expected file location to `SNIPPET_PATHS` in `scripts/verify-snippets.ts`. If it imports a sibling file by a placeholder name, add a stub to `STUBS` in the same script.
5. Update `README.md`: add a row to the Snippets table and a detail section that follows the existing format (what it generates, why the defaults are what they are, and its tab stops).
6. Run `npm run verify` and make sure it passes with no warnings.
7. Open a pull request. Explain what the snippet generates, the problem it solves, and any trade-off or limitation you chose.

CI runs `npm run verify` on every pull request, so a PR that doesn't pass it can't be merged.

## Quality Checklist

Before opening a pull request, check that your snippet:

- **Follows the project conventions**: ESM with NodeNext, relative imports ending in `.js`, `import type` for type-only imports, and strict TypeScript with no `any`.
- **Lets errors flow to the error middleware**: no try/catch that only logs or converts errors into responses. Throw an `AppError` for expected failures, and use `{ cause }` when adding context to a lower-level error.
- **Has safe defaults**: no wide-open CORS, no secrets or credentials in placeholders, no internal details leaked to clients.
- **States its limitations**: if the generated code isn't safe for every environment (for example, an in-memory store that only works in a single process), say so in a code comment and in the README.
- **Has logical tab stops**: the most important placeholder is `$1`, related placeholders are mirrored, and `$0` leaves the cursor where the developer will type next.
- **Imports only what it uses.**

## Snippet Format Reference

```json
"Snippet Name": {
  "prefix": "xp-category-variant",
  "body": [
    "line one",
    "line two with ${1:placeholder}",
    "$0"
  ],
  "description": "xp-category-variant: one-line description shown in IntelliSense"
}
```

- Each element in `body` is one line. Use `""` for a blank line.
- `${1:label}` is a tab stop with a default value, and `$0` is the final cursor position.
- A literal `$` must be escaped as `\\$` in the JSON string. For example, a template literal like `` `${PORT}` `` must be written as `` `\\${PORT}` ``, otherwise VS Code treats it as a snippet variable. The verify script catches this.

## Reporting a Bug

Open an issue with:

- The snippet prefix.
- What it generated, and what you expected instead.
- Your TypeScript, Express and Node versions, if the problem is a compile or runtime error.

## Licence

By contributing, you agree that your contributions will be licensed under the project's [MIT License](LICENSE).
