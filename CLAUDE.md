# CLAUDE.md

Guidance for Claude Code (and any other agent) working in this repository.

## What this is

One app built on [SACRVM APPKIT](https://github.com/SACRVM/sacrvm-appkit).
**One repo, one app** — the repo *is* the app: `app.json` (the manifest a
desktop reads), `app.js` (one custom element, one classic script), `app.css`,
and `index.html` as a standalone harness. Read `README.md` before changing the
shape of any of them.

## Non-negotiable

- **No build step.** Vanilla custom elements, plain CSS, `npx serve .` and F5.
  Never introduce a bundler, node_modules, TypeScript, or a framework.
- **One custom element per repo**, defined through `sac.app.define(tag, class)`.
  Helper classes are fine; a second registered element means a second repo.
- **Light DOM.** No shadow root — the kit's stylesheet has to reach the markup.
- **Tokens only.** Style the app's own element, never `:root`. No raw colour
  literals: use the kit's tokens, and one `--accent` seed if the app wants an
  identity of its own.
- **Don't vendor the kit.** The host provides it. `index.html` borrows it from
  the appkit's Pages purely so the app can run alone.
- **The kit's API only** — what the style guide documents. Never reach into a
  component's shadow root or private fields.

## The three hooks

`build()` writes markup once. `onMount(context)` runs when the app is really on
screen — subscriptions, measuring, first render. `onUnmount()` undoes exactly
what `onMount` did: every unsubscribe kept, `context.sidebar.clear()` if the
rail was filled.

Anything on `sac` beyond `sac.app` belongs to the host and is optional — guard
it (`typeof sac.toast === "function"`), never assume it.

## Publishing

GitHub Pages serves this repo from the root, and a desktop installs the app by
reading `app.json` from that origin. Whatever is committed here is what people
install: there is no release step, and every push is immediately live.
