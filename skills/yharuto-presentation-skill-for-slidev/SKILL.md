---
name: yharuto-presentation-skill-for-slidev
description: Generate Slidev decks (web-based, animated developer presentations built on Vite/Vue) using the same pastel-accent design system as the custom-marp-presentation skill — pastel top/bottom border accents, Noto Sans JP + Fira Code fonts, a bar-style header on every content slide, and a gradient underline on the title slide. Use this whenever the user asks for a Slidev deck, wants the Marp presentation design "in Slidev too" / "as a Slidev version" / "with animations", or wants an interactive/exportable presentation with this specific look — even if they don't spell out the design and just say something like "make this into a Slidev deck" or "same design but with slidev".
---

# Custom Slidev Presentation Generator

Port of the `custom-marp-presentation` design system to Slidev. Use this when the user wants that same pastel-accent look but needs Slidev's features — click animations, live code, Vue components, presenter mode, PDF/PPTX export — that Marp can't do.

## Why this isn't just "copy the Marp file"

Marp decks are one self-contained Markdown file: frontmatter + a `<style>` block + slides, and the `<style>` block automatically applies to every slide. Slidev decks are Vite/Vue **projects**. A `<style>` tag written inside `slides.md` is scoped to that single slide only (see the general `slidev` skill's `style-scoped` reference) — so an inline `<style>` block copied straight from the Marp version would only style the one slide it's written on, silently breaking the design everywhere else.

The design has to live in **`styles/index.css`** at the project root, which Slidev loads globally across all slides. That's the one non-obvious step in this whole skill — everything else follows naturally once the CSS is in the right place.

For Slidev syntax, CLI commands, and features beyond this design system, defer to the general `slidev` skill — this skill only covers the design port and where the design-specific files go.

## Setup

1. **Confirm a Slidev project exists.** Look for `slides.md` and a `package.json` with `slidev` as a dependency in the working directory. If there isn't one, scaffold it first (`pnpm create slidev`, or npm/yarn equivalent per the user's preference) before applying the design.

2. **Install the global stylesheet.** Copy this skill's `assets/index.css` to `<project>/styles/index.css` (create the `styles/` directory if needed). This one file carries the entire visual design — borders, fonts, heading treatment, code blocks, the cover-slide gradient.

3. **(Optional) Install the page counter.** Copy `assets/global-bottom.vue` to the project root as `global-bottom.vue`. This reproduces Marp's `paginate: true` — Slidev has no equivalent frontmatter flag, so it's done via a global layer component instead. Skip this if the user doesn't want page numbers.

4. **Set the headmatter, combined with slide 1's own frontmatter.** This is the one Slidev-specific trap: the *first* `---...---` block in `slides.md` is both the deck-wide headmatter (`theme`, `fonts`, `title`, ...) **and** slide 1's per-slide frontmatter (`layout`, `class`, ...) at the same time — there is only one block, not two. Writing a second `---layout: cover---` block right after the headmatter creates an empty slide 1 followed by the real title slide as slide 2 (verified by actually running this — it's an easy mistake). Since slide 1 is always the title slide in this design, `layout: cover` and `class: cover` belong in this same block:

```yaml
---
theme: default
colorSchema: light
fonts:
  sans: 'Noto Sans JP'
  mono: 'Fira Code'
  provider: google
title: <presentation title>
layout: cover
class: cover
---

# プレゼンテーションタイトル

サブタイトルや発表者名など
```

`colorSchema: light` matters here: the design assumes a light background and dark headings, and pins Shiki (Slidev's code highlighter) to a light theme so code blocks don't end up with a dark Shiki background clashing with the light `pre`/`code` styling in `index.css`.

`class: cover` is set explicitly alongside `layout: cover` as a defensive measure — Slidev's built-in layouts expose their name as a class automatically (`.slidev-layout.cover`), which is what the CSS actually targets, but pinning it in frontmatter guarantees the design still applies even if that automatic behavior ever changes.

## Slide patterns

**Content slides** (everything after slide 1) — same heading convention as the Marp version: h1 is reserved for the cover slide, so every content slide uses `## ` for its bar-style header. No `layout:` key needed (`default` is Slidev's default), and each one gets its own `---` separator:

````md
---

## セクション見出し

- 箇条書き1
- 箇条書き2

---

## コード例

```ts
const hello = 'world'
```
````

If the deck uses `<Toc />` and clean nesting matters, add `title: "スライド名"` to a content slide's frontmatter — Slidev's ToC reads that instead of inferring from the h2, so nesting stays correct even though the visible heading is h2, not h1.

## Layering on Slidev features

The design only touches base HTML elements (`h1`–`h3`, `ul`/`ol`/`li`, `pre`/`code`, `strong`) plus the `.cover` layout, so Slidev's animation and component system works on top of it with no extra styling needed — `<v-click>`, `<v-clicks>`, Monaco code blocks, Mermaid diagrams, etc. all inherit the surrounding fonts/colors automatically. Consult the general `slidev` skill's references for how to use these.

## Verification

Per the user's standing instruction to check frontend UI changes in a browser before calling them done: run `pnpm run dev` and open `http://localhost:3030`, then check:
- The cover slide shows the big left-aligned title, the thick pink top border, and the gradient bar at the bottom edge.
- Content slides show the pink-underlined header bar pinned near the top, with body content starting below it (not overlapping).
- Code blocks render with the light `#f8f9fa` background and pink left border — if Shiki is instead rendering a dark code background, double check `colorSchema: light` is set in the headmatter.
- List markers are pink, `**bold**` text has a pink underline.

If padding or borders don't seem to apply at all, the active theme's own styles are likely winning the cascade — the `!important` flags already in `index.css` should cover this, but confirm the file actually landed at `<project>/styles/index.css` and not somewhere Slidev won't auto-load it.

## Bundled files

- `assets/index.css` — the full design system, copy to `<project>/styles/index.css`.
- `assets/global-bottom.vue` — optional page-number footer, copy to `<project>/global-bottom.vue`.
