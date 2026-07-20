# Responsive Design — Full Notes (Day 4)

> One page, every screen. The layout you built in Day 3 is desktop-only.
> Today you make the **same HTML** reflow for phones and tablets — with
> almost no new CSS, just the CSS you know wrapped in `@media`.

```
Desktop rules (the base)  ← always apply
        +
@media (max-width: 768px) { ...phone overrides... }  ← ALSO apply when small
```

---

## 0. The one tag nothing works without

Put this in every page's `<head>`:

```html
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

Without it, a phone lies and says it's ~980px wide, renders the desktop
layout, then shrinks the whole thing to fit — tiny, unreadable, and your
media queries **never fire**. This is the #1 "why isn't it responsive?"
bug. `width=device-width` = "use the phone's real width." Always.

---

## 1. The mental model: media queries are just "when…"

A media query is a normal CSS rule wrapped in a condition:

```css
@media (max-width: 768px) {
  .card-grid { grid-template-columns: 1fr; }
}
```

Read it out loud: **"when the screen is 768px wide or narrower, ALSO make
the card grid one column."** Same property, same value syntax you already
know. The only new thing is the `@media (...) { }` wrapper.

- `max-width: 768px` → applies at 768px **and below** (small screens).
- `min-width: 768px` → applies at 768px **and above** (big screens).

Later rules win, so put your overrides **after** the base rule.

---

## 2. Desktop-first vs mobile-first

Two directions, both valid:

| Approach         | Base CSS is for… | Media queries use… |
| ---------------- | ---------------- | ------------------ |
| **Desktop-first**| big screens      | `max-width` (shrink down) |
| **Mobile-first** | phones           | `min-width` (grow up)     |

Your Day 3 page is already a desktop layout, so **desktop-first + `max-width`**
is the natural move for today — you're adding "when it gets small, do this."
(Mobile-first is what pros usually start with on a fresh build; note it,
don't sweat it yet.)

---

## 3. Breakpoints — where to cut

Breakpoints are just the widths you chose to change the layout. Don't
chase specific phone models; pick round numbers where **your** design
starts to look bad:

```
~1024px   tablet / small laptop   → 3 cols becomes 2
~768px    tablet portrait / big phone → 2 cols becomes 1, nav stacks
~480px    small phone             → final trims (padding, font)
```

Golden habit: **drag the browser narrower slowly. The moment it looks
cramped is a breakpoint.** The design tells you where, not a chart.

---

## 4. The responsive toolkit (beyond media queries)

Media queries are the hammer, but three tools reduce how many you need:

### `max-width` instead of `width`
```css
.card { max-width: 400px; }   /* never wider than 400, but shrinks freely */
```
`width: 400px` is rigid and overflows small screens. `max-width` = "up to,
but flexible." Same trick as `img { max-width: 100%; }` so images never
blow out their container.

### `flex-wrap` — let rows break by themselves
```css
.nav-links { display: flex; flex-wrap: wrap; }
```
When items don't fit, they drop to the next line — no media query needed.

### `auto-fit` + `minmax()` — the zero-media-query grid
```css
.card-grid {
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
}
```
"Fit as many columns as you can, each at least 250px, then stretch them to
fill." Resize the window and columns add/remove themselves. This one line
can replace tasks 1 and 2 of today's exercise entirely. Understand it, then
choose: explicit media queries (more control) or this (less code).

### Relative units
`%`, `rem`, `vw` flex with the screen; `px` is frozen. Use `px` for borders
and tiny fixed things, relative units for layout widths and (increasingly)
font sizes.

---

## 5. Recipes for today's tasks

### Card grid: 3 → 2 → 1
```css
/* base = 3 (already there) */
@media (max-width: 1024px) {
  .card-grid { grid-template-columns: repeat(2, 1fr); }
}
@media (max-width: 768px) {
  .card-grid { grid-template-columns: 1fr; }
}
```

### Navbar that stacks on a phone
```css
@media (max-width: 768px) {
  .navbar { flex-direction: column; gap: 12px; }
  .nav-links { flex-wrap: wrap; justify-content: center; }
}
```

### Smaller hero type + padding
```css
@media (max-width: 768px) {
  .hero { padding: 32px 20px; min-height: auto; }
  .hero h1 { font-size: 28px; }
}
```

### Footer columns → stacked
```css
@media (max-width: 768px) {
  .footer-columns { flex-direction: column; }
}
```

---

## 6. Gotchas

1. **No viewport meta = nothing works.** If media queries seem ignored,
   check the `<head>` first. (See §0.)
2. **Overrides must come AFTER the base**, or lower in the file, or they
   lose the specificity/order tie. Keep all `@media` blocks at the bottom.
3. **`max-width` vs `min-width` mix-ups.** Desktop-first uses `max-width`.
   Writing `min-width: 768px` and expecting phone styles is a classic flip.
4. **Testing zoomed-out on a laptop lies.** Use DevTools device mode
   (F12 → phone icon) or a real phone. Actual pixels matter.
5. **Overflow / horizontal scroll on mobile** usually = a fixed `width`,
   a huge unbreakable word, or an image without `max-width: 100%`.
6. **Too many breakpoints = unmaintainable.** Two or three is plenty for
   a page like this. Add one only when the design actually breaks.

---

## 7. How to master this

- **DevTools device mode:** F12 → the phone/tablet icon (top-left of the
  panel). Pick "Responsive" and drag the handle — watch each breakpoint
  fire. This is your main tool today.
- **The drag test:** with the page open, slowly narrow the window from
  full to phone width. Every ugly moment is a breakpoint you haven't
  handled yet.
- **Reference:** [MDN — Responsive design](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design)
  and [MDN — Media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries).
- Decision habit: **reach for `flex-wrap` / `auto-fit` first; add a media
  query only when the layout needs a real structural change** (columns →
  rows, hiding an element, resizing type).
