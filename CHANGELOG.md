# Changelog

All notable changes to the One Oracle Developer theme and its companion `ood-` snippets in `.obsidian/snippets/`.

## Unreleased

## 1.1.0 – 2026-04-26

- A) Add `ood-left-sidebar-tabs` snippet – Files + Bookmarks left-aligned on a single row, Search rendered icon-only floating right. Overrides the theme's stacked column layout for `.mod-top-left-space` containers; sidebar collapse toggle stays at the top.
- B) Restyle blockquote – heading-family italic font (`var(--ood-font-heading)`), 16px size. Removed the visible left bar in both Reading view (kill `::before` pseudo) and Live Preview (zero `.HyperMD-quote` left/right borders, hide stacked `>` markers via `cm-formatting`). Restored grey fill explicitly with `background-color: var(--background-secondary)`. Padding is 20px left / 16px right on every quote line. Italic-serif typography applies only to prose (`blockquote > p` in Reading view, non-`HyperMD-list-line` quote lines in Live Preview) so lists inside blockquotes still render as lists.

- C) Blockquote multi-paragraph spacing – convention is blank `>` separators (`> A` / `>` / `> B`). Reading view: `margin-block: 0.9em` on `<p>` children with `html body` specificity to beat Baseline's `.markdown-rendered p` reset; first/last paragraph drop their outer margin so the blockquote's own padding-block holds. Live Preview: zero `padding-block` on `.cm-line.HyperMD-quote` – the empty `>` line's own line-height is the inter-paragraph gap. Embedded-HTML `<p>` chunks inside blockquotes also get zeroed inner margins (with first/last reverted) so HTML quotes match native ones.

- D) Blockquote caret fix – zero-width space pseudo (`content: "\200B"`) on every `.cm-line.HyperMD-quote::before` so the cursor on an empty `>` line keeps full height. Without this, CM6 has no inline content to derive caret height from (the `>` glyph is hidden) and the caret shrinks to a dot or vanishes. The `body` prefix beats the bar-killing `::before` reset.

- E) Table headers bottom-aligned – `vertical-align: bottom` on `thead tr` / `th` so multi-line column labels sit flush with single-line ones above the data row. Reading view works directly because `<th>` is `display: table-cell`. Live Preview wraps each th's content in a block-level `.table-cell-wrapper` div, so the wrapper itself is set to `display: inline-block; vertical-align: bottom; width: 100%` – inline-block lets the th's own `vertical-align: bottom` actually reposition it, and dropping `height: 100%` avoids inflating the row. The th keeps `display: table-cell` to preserve column geometry.

- F) Active table header editor – when editing a header cell, Obsidian sets the original `.table-cell-wrapper` to `display: none` and injects a second wrapper (with `data-ignore-swipe="true"`) containing a live CM6 editor. Selector is scoped with `:not([style*="display: none"])` so the inline-block override doesn't un-hide the original (which would render as a duplicate above the editor). Editor wrapper itself is also bottom-aligned, and its CM6 layers (`.cm-s-obsidian`, `.cm-editor`, `.cm-scroller`, `.cm-content`, `.cm-line`) are forced transparent so the editor blends with the header background instead of showing CM6's default dark-grey fill.

- G) Table header click – kill the grey flash that appeared during mousedown on a `<th>` cell in Edit mode. Existing transparent-bg rule covered the editor at rest but not the brief `:active` state on the th and wrapper, nor `.cm-active` / `.cm-activeLine` inside. Selector list now also targets `th:hover/:active/:focus/:focus-within`, the same pseudo-classes on `.table-cell-wrapper`, plus `.cm-editor.cm-focused` and the active-line classes – all forced to transparent so clicking a header is visually inert.
