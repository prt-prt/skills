---
name: browser-cli
description: Drive a local headed Chrome via ~/.browser-cli/browse.js (Playwright over CDP) to navigate, inspect, fill forms, and screenshot web apps. Use when the user asks to open/navigate/check pages in a browser, walk a user-test journey, or verify frontend behavior in a running dev stack.
---

# browser-cli

Headed Chrome controlled over CDP by `~/.browser-cli/browse.js` (Playwright). The browser
stays open between commands; each invocation connects, acts, disconnects. It runs a
dedicated profile on port 9223, so it coexists with the user's normal Chrome without
sharing sessions.

Every command prints `[page] <url>` after acting, so state is always visible in output.

## Bootstrap

If `~/.browser-cli/browse.js` is missing, create it:

```sh
mkdir -p ~/.browser-cli && cd ~/.browser-cli
pnpm init && pnpm add playwright
```

No Chromium download is needed; it drives the installed Google Chrome.

## Commands

Run as `node ~/.browser-cli/browse.js <command>`:

- `start` — launch or reach the managed Chrome
- `goto <url> [--new]` — navigate the active tab, or open a new one
- `snap` — aria snapshot (YAML) of the active tab; how you read the page
- `text` — visible body text, first 8000 chars
- `shot [file] [--full]` — screenshot PNG (default: temp dir); read it to verify visuals
- `click <selector>` — click, then prints the resulting URL
- `type <selector> <text> [Enter]` — fill a field
- `eval <js>` — run JS in the page, result printed as JSON; use for DOM mapping and state verification
- `pages` — list open tabs
- `close` — quit the managed Chrome

## Selectors

Playwright syntax throughout: `role=link[name="Team"]`, `role=button[name=/regex/]`,
`css=#id`, `text=...`. Prefer `role=` for interaction and unique `#id`s for inputs.

## Workflow rules (learned from real runs)

1. Read `snap` before acting; act with `role=` selectors when possible.
2. Prefer unique `#id`s. With duplicate names or labels, map the real inputs first via
   `eval` over `document.querySelectorAll('input')` (id, name, placeholder, visibility,
   value) before typing anything.
3. Re-verify filled values with a batched `eval` before submitting. Composite widgets
   (phone pickers, address fields) often render a visible input plus a hidden twin with
   the same name; filling the wrong one fails silently and only surfaces at submit.
4. Pair `snap` with `shot` when checking form or validation behavior: native HTML5
   validation tooltips (e.g. "Fülle dieses Feld aus.") never appear in the aria tree.
5. SPA pages may report an empty `[title]` right after load until hydration settles;
   trust headings and content, not the title.
6. Secondary pages (Mailpit on :8025, logs) open with `--new`. The active tab is tracked
   in `~/.browser-cli/state.json`; `goto` reuses it, so `goto` the app URL again to
   return there.
7. For mail in dev stacks, Mailpit's API beats its UI for link extraction:
   `curl localhost:8025/api/v1/messages` and `/api/v1/message/<ID>` (field `HTML`/`Text`).
8. For multi-step journeys, keep a todo list and run one step per invocation; the
   observe-decide-act loop is the point.
