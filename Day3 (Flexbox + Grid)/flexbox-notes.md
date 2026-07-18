# Flexbox — Full Notes (Day 3)

> The one rule: **flexbox properties go on the PARENT (container).**
> The children (items) get arranged automatically.

```
.container   ← display: flex goes HERE
 ├── item
 ├── item
 └── item
```

---

## 1. The two axes

The moment you write `display: flex`, the container gets two invisible axes:

- **Main axis** — the direction items flow (set by `flex-direction`)
- **Cross axis** — the perpendicular one

```
flex-direction: row (default)        flex-direction: column
main axis ───────────▶               main axis
[A] [B] [C]                            │   [A]
        │                              │   [B]
        ▼ cross axis                   ▼   [C]
                                     cross axis ─────▶
```

Why this matters: `justify-content` works on the **main** axis,
`align-items` works on the **cross** axis. When direction changes,
their jobs rotate with it.

**Mnemonic:** **J**ustify = **J**ourney direction (the flow).
**A**lign = **A**cross (the other one).

---

## 2. Container properties (the ones you set 95% of the time)

### `display: flex`
Turns it on. Nothing arranges without this line.

### `flex-direction`
| Value | Meaning |
|---|---|
| `row` | left → right (default) |
| `column` | top → bottom |
| `row-reverse` / `column-reverse` | same, flipped |

### `justify-content` — "where does the leftover space go?" (main axis)
```
flex-start      [A][B][C]...................   space after (default)
flex-end        ...................[A][B][C]   space before
center          .........[A][B][C].........    split both sides
space-between   [A].........[B].........[C]    all space BETWEEN items
space-around    ...[A]......[B]......[C]...    equal space around each
space-evenly    ....[A]....[B]....[C]....      perfectly even gaps
```

### `align-items` — cross-axis alignment
For a `row`, this is the **vertical** position:
```
stretch (default)   center      flex-start   flex-end
[A][B][C]           ........    [A][B][C]    ........
[A][B][C]           [A][B][C]   ........     ........
[A][B][C]           ........    ........     [A][B][C]
```
`stretch` is why nav links/cards grow to equal height by default.

### `gap`
Space **between** items (not at the edges). Replaces margin hacks.
```css
gap: 16px;          /* both directions */
gap: 16px 24px;     /* row-gap column-gap */
```

### `flex-wrap`
By default items **squish onto one line** forever. `flex-wrap: wrap`
lets them break to the next line when they run out of room.
(Key for responsive design — Day 4.)

---

## 3. Item properties (used occasionally, on the children)

### `flex-grow` — "can I take extra space?"
```css
.item { flex-grow: 1; }   /* default is 0 = don't grow */
```
Give one item `flex-grow: 1` and it eats all leftover space —
useful for a search bar that fills the middle of a navbar.

### `flex-shrink` — "can I be squashed?" (default 1 = yes)

### `flex-basis` — starting size before growing/shrinking

### Shorthand you'll see everywhere:
```css
flex: 1;            /* = grow 1, shrink 1, basis 0 → equal-width items */
```

### `align-self` — override `align-items` for ONE item
```css
.special { align-self: flex-end; }
```

### `order` — reorder visually without touching HTML (default 0)

---

## 4. Recipes you'll reuse forever

### Navbar (logo left, links right)
```css
.navbar {
  display: flex;
  justify-content: space-between;  /* opposite ends */
  align-items: center;             /* vertical centering */
}
.nav-links {
  display: flex;   /* nested container for the links themselves */
  gap: 24px;
}
```

### Perfect centering (hero, modal, loading screen)
```css
.hero {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
```
This 4-liner replaced a decade of CSS centering hacks. Memorize it.

### Row of equal-width buttons/cards
```css
.row  { display: flex; gap: 16px; }
.row > * { flex: 1; }
```

### Footer columns
```css
.footer-columns {
  display: flex;
  justify-content: space-between;
  gap: 32px;
}
```

---

## 5. Gotchas (things that will bite you)

1. **`display: flex` on the wrong element.** It goes on the *parent*
   of the things you want to arrange. If nothing moved, you probably
   styled the item, not the container.
2. **Only direct children become flex items.** Grandchildren don't care.
   That's why `.navbar` arranging `.nav-links` doesn't arrange the
   links *inside* it — you need a second flex container (nesting).
3. **`display: block` / `inline` on flex items is ignored.** The
   parent's flexbox takes over their layout.
4. **`justify-content: center` didn't center vertically?** Check your
   `flex-direction` — in a row, vertical is `align-items`' job.
5. **Items overflow instead of wrapping?** Flexbox never wraps unless
   you say `flex-wrap: wrap`.
6. **Centering looks off?** A leftover `margin` on a child (e.g. from
   a reset you forgot) shoves it around. DevTools → hover the element
   → orange = margin.

---

## 6. How to actually master this

- **Debug visually:** F12 → Elements → click a flex container → the
  little `flex` badge next to it toggles an overlay showing the axes.
- **Change values live** in DevTools' Styles panel instead of guessing
  in the editor. Refresh wipes it — your file is safe.
- **Play:** [Flexbox Froggy](https://flexboxfroggy.com) — ~20 min game
  that drills every property above. Do it tonight.
- **Reference:** [CSS-Tricks Flexbox Guide](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
  — the diagram cheat-sheet everyone keeps open.
- Ask "**where do I want the empty space?**" before writing
  `justify-content`. The answer *is* the value.
