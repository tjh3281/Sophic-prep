# CSS Grid — Full Notes (Day 3)

> Same golden rule as flexbox: **grid properties go on the PARENT (container).**
> The children just flow into the cells you define.

```
.card-grid   ← display: grid goes HERE
 ├── .card   ← items auto-place themselves into cells
 ├── .card
 └── ...
```

**Flexbox vs Grid in one line:**
flexbox = a queue of items in **one direction**;
grid = a **table of cells** you design first, then items fill it.

---

## 1. The mental model: draw the skeleton first

With grid you don't arrange items — you **define columns (and rows), then
items pour into the cells automatically**, left→right, top→bottom:

```
grid-template-columns: repeat(3, 1fr);

┌────────┬────────┬────────┐
│ card 1 │ card 2 │ card 3 │   ← fills across first...
├────────┼────────┼────────┤
│ card 4 │ card 5 │ card 6 │   ← ...then wraps to a new row BY ITSELF
└────────┴────────┴────────┘
```

6 items ÷ 3 columns = 2 rows. You never wrote "2 rows" — grid made them.
(Auto-created rows are called *implicit* rows.)

---

## 2. Container properties (your 95%)

### `display: grid`
Turns it on. Alone it does almost nothing visible — one column, items
stacked. The magic starts when you define columns.

### `grid-template-columns` — THE property of grid
Each value = one column, in order:

```css
grid-template-columns: 200px 200px 200px;  /* 3 fixed columns */
grid-template-columns: 1fr 1fr 1fr;        /* 3 equal flexible columns */
grid-template-columns: repeat(3, 1fr);     /* same, less typing */
grid-template-columns: 2fr 1fr;            /* 1st twice as wide as 2nd */
grid-template-columns: 250px 1fr;          /* fixed sidebar + fluid main */
```

### `fr` — the "fraction" unit
"1 share of the leftover space." Grid subtracts fixed sizes and gaps
first, then splits what's left by shares:

```
1fr 1fr 1fr   →  ⅓ ⅓ ⅓
2fr 1fr       →  ⅔ ⅓
250px 1fr     →  250px, rest
```

### `gap`
Identical to flexbox — space between cells, never at the edges:
```css
gap: 24px;        /* rows and columns */
gap: 32px 16px;   /* row-gap column-gap */
```

### `grid-template-rows` (rarely needed)
Set explicit row heights: `grid-template-rows: 80px 1fr 60px;`
For card grids, skip it — auto rows size to their content.

### `justify-items` / `align-items` (occasional)
Position content *inside each cell* (horizontal / vertical).
Default `stretch` fills the cell — usually what you want for cards
(it's why all cards in a row end up equal height).

---

## 3. Item properties — spanning cells

Sometimes one item should be bigger than one cell:

```css
.featured {
  grid-column: span 2;   /* take up 2 columns */
  grid-row: span 2;      /* take up 2 rows */
}
```

```
┌────────────────┬────────┐
│                │ card 2 │
│  featured      ├────────┤
│  (span 2 cols, │ card 3 │
│   span 2 rows) ├────────┤
│                │ card 4 │
└────────────────┴────────┘
```

You can also pin items to exact grid lines (`grid-column: 1 / 3` =
"from line 1 to line 3"). Lines are numbered from 1, **between** the
columns — 3 columns have 4 lines. File under "know it exists."

---

## 4. Recipes you'll reuse forever

### Card grid (today's task)
```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
```
3 lines. That's the whole thing.

### The responsive one-liner (preview of Day 4)
```css
grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
```
"As many ≥250px columns as fit; stretch them to fill." Resize the
window and columns add/remove themselves — zero media queries.

### Classic page shell
```css
.page {
  display: grid;
  grid-template-columns: 250px 1fr;   /* sidebar + content */
}
```

### Footer columns
```css
.footer-columns {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 32px;
}
```
(Flexbox also works here — 1-row layouts are the overlap zone where
both are fine.)

---

## 5. Gotchas

1. **`display: grid` alone looks like nothing happened.** One column
   is the default — you must declare `grid-template-columns`.
2. **Wrong parent, again.** Grid on `.services` would arrange the
   `h2` and `.card-grid` into columns — not the cards. The cards'
   parent is `.card-grid`.
3. **Old margins fight the gap.** If items still have
   `margin-bottom` from the stacked layout, spacing looks uneven —
   delete the margin and let `gap` do the job alone.
4. **`repeat(3, 1fr)` ≠ "max 3 items."** It means 3 *columns*; extra
   items wrap to new rows automatically.
5. **Only direct children become grid items** — same as flexbox.
   Grandchildren (the `h3` inside a card) are untouched.
6. **Don't put `display: grid` and `display: flex` on the same
   element** — one `display` per element; last one wins. (An item
   *inside* a grid can itself be a flex container though — nesting
   is normal and encouraged.)

---

## 6. How to master this

- **DevTools grid overlay:** F12 → Elements → click `.card-grid` →
  press the little `grid` badge — it draws the columns, gaps, and
  line numbers right on your page. Best grid-learning tool there is.
- **Play:** [Grid Garden](https://cssgridgarden.com) — the sister
  game to Flexbox Froggy. ~20 min, drills columns/rows/spans.
- **Reference:** [CSS-Tricks Grid Guide](https://css-tricks.com/snippets/css/complete-guide-grid/)
  — keep it open like the flexbox one.
- Decision habit: **one direction → flex, two directions → grid.**
  Cards/galleries/page shells → grid. Navbars/centering/button rows → flex.
