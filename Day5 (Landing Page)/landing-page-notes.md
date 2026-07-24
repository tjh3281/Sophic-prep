# Full Landing Page — Notes (Day 5)

> Days 1–4 gave you parts: semantic HTML, a palette, flexbox, grid, media
> queries. Today you stop building _components_ and build a **page** —
> nav → hero → proof → offer → CTA → footer, top to bottom, on one canvas.
>
> Almost no new CSS properties today. The new skill is **assembly**:
> consistent spacing, one button system, one set of colours, sections that
> all obey the same rhythm.

```
Day 3  →  a navbar          a card grid       a footer
Day 4  →  ...that survive a phone
Day 5  →  10 sections stacked into something a company would actually ship
```

---

## THE POINT OF TODAY (read this if you read nothing else)

**You learn zero new CSS properties today. You learn consistency.**

The difference between a page that looks designed and one that looks
pasted together is not talent — it's that every section shares:

| One…             | Which means                                            |
| ---------------- | ------------------------------------------------------ |
| set of colours   | `:root` tokens, no raw hex anywhere else (§3)           |
| spacing scale    | `--space-sm/md/lg`, never a random 37px (§3)            |
| content width    | `.container` on every section (§4)                      |
| vertical rhythm  | `.section` on every section (§4)                        |
| button           | `.btn` + a variant, not a new style per section (§5)    |

Those five lines are Day 5. Sections 0–2 and 6–10 below are context and
recipes — useful, but skimmable. **§3, §4 and §5 are the lesson.**

If you only have 30 minutes, jump straight to §10.

---

## 0. What a landing page IS

A landing page has **one job**: get one specific action from one specific
visitor. For a B2B site like Sophic that action is "contact us / request a
quote", not "browse everything".

That single goal drives every decision:

- The CTA ("Contact Us", "Talk to an Engineer") appears **3–4 times** —
  nav, hero, mid-page band, contact section. Same words every time.
- Every section either **builds trust** or **removes a doubt**. If a
  section does neither, delete it.
- Copy answers, in order: _What is it? Why should I care? Can I trust
  you? What do I do next?_

Rule of thumb: a visitor should understand what you do in **5 seconds**
from the hero alone.

---

## 1. Anatomy — the section stack

This is the standard B2B order. It's a story, not a random pile:

| #   | Section        | Job it does                                  |
| --- | -------------- | -------------------------------------------- |
| 1   | Nav            | Orientation + always-visible CTA              |
| 2   | Hero           | What you do, for whom, + primary CTA          |
| 3   | Trust bar      | "Real companies use this" (logos / clients)   |
| 4   | Services grid  | What you actually sell                        |
| 5   | How it works   | Removes "this looks complicated" doubt        |
| 6   | Stats          | Proof in numbers                              |
| 7   | Testimonial    | Proof in a human voice                        |
| 8   | CTA band       | Catch the reader who's convinced already      |
| 9   | Contact        | The conversion point                          |
| 10  | Footer         | Links, address, legal, the exits              |

You already own #1, #4, #10 from Day 3/4. Today you add the middle.

**Alternate wide/narrow, light/dark.** Two identical white sections in a
row read as one long blur. Give the stats band and CTA band a dark
background and the page instantly gets rhythm.

---

## 2. Copy hierarchy — write it before you style it

Each section is the same three-part shape:

```
eyebrow    small, uppercase, coloured   →  "WHAT WE DO"
headline   the big h2                   →  "Automation that ships on time"
body       one or two lines of grey     →  the supporting sentence
```

Headline formula that works for B2B: **outcome + who it's for**.
"We Make Machines Sing" is a slogan; "Test engineering for semiconductor
lines in Penang" is a headline. A hero can carry both — slogan as `h1`,
clarity in the paragraph underneath.

Write placeholder copy in the HTML **first**, style **second**. Styling
lorem ipsum lies to you about line lengths.

---

## 3. Design tokens — CSS custom properties (the one new thing)

You've been retyping `#102a43` since Day 2. Ten sections in, that's a
maintenance trap. Define your palette **once** as variables:

```css
:root {
  --navy-900: #102a43;
  --navy-700: #243b53;
  --blue-400: #4bafdd;
  --grey-600: #486581;
  --surface: #f5f7fa;

  --space-sm: 12px;
  --space-md: 24px;
  --space-lg: 48px;
  --radius: 8px;
}
```

Use them with `var()`:

```css
.navbar {
  background-color: var(--navy-900);
  padding: var(--space-md) var(--space-lg);
}
```

- `:root` = the `<html>` element. Declaring there makes them global.
- Custom properties **must** start with `--`. `var(--navy-900)` reads the
  value; a fallback is `var(--navy-900, #102a43)`.
- Change the brand colour later → **edit one line**, whole page follows.

This is the closest CSS gets to `static final` constants in Java. Same
reason you use them there: one source of truth.

---

## 4. Container + section rhythm

Two utility classes carry the whole layout. Learn these two and every
section becomes three lines of CSS.

```css
/* Keeps content from stretching to 2000px on a wide monitor */
.container {
  max-width: 1100px;
  margin: 0 auto; /* auto left+right = horizontally centred */
  padding: 0 24px; /* breathing room on small screens */
}

/* Consistent vertical space between sections */
.section {
  padding: 72px 0;
}
```

```
|<-- auto -->|<------ 1100px content ------>|<-- auto -->|
```

**Why this matters:** amateur pages look amateur because every section
has a different padding — 40px, then 55px, then 80px. Pick ONE vertical
rhythm (`--space-lg`) and use it everywhere. Consistency reads as design.

Pattern: outer element = full-bleed background colour, inner
`.container` = constrained content.

```html
<section class="section stats">
  <!-- dark background, edge to edge -->
  <div class="container">...</div>
  <!-- text stays centred and readable -->
</section>
```

---

## 5. One button system

Don't restyle a button per section. Define a base + variants:

```css
.btn {
  /* shared: padding, radius, weight, inline-block */
}
.btn-primary {
  /* filled blue */
}
.btn-ghost {
  /* transparent + border, for the secondary action */
}
```

```html
<a href="#contact" class="btn btn-primary">Talk to an Engineer</a>
<a href="#services" class="btn btn-ghost">See Our Services</a>
```

Two classes on one element — the base does the shape, the variant does
the colour. **One primary CTA per screen**; if everything shouts, nothing
does.

---

## 6. Semantics + accessibility (this is the professional part)

You did semantic tags on Day 1. On a full page they matter more:

- **Landmarks:** `<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`.
  Everything between nav and footer belongs inside a single `<main>`.
- **One `h1` per page** (the hero). Sections use `h2`, cards `h3`. Never
  skip a level to get a smaller font — that's what CSS is for.
- **Skip link** as the first element in the body, so keyboard users can
  jump past the nav:
  ```html
  <a href="#main" class="skip-link">Skip to content</a>
  ```
- **Every input needs a `<label for="...">`** matching the input's `id`.
  A placeholder is not a label — it vanishes when you type.
- **Focus styles:** never `outline: none` without a replacement. Use
  `:focus-visible` to show a ring for keyboard users only.
- **Contrast:** grey text on a coloured background is where this usually
  fails. Check with DevTools → inspect element → the contrast reading in
  the colour picker.

Tab through the finished page with the keyboard. If you can't see where
you are, it's broken.

---

## 7. Recipes for today's sections

### Sticky nav

```css
.navbar {
  position: sticky;
  top: 0;
  z-index: 10;
}
```

`sticky` = normal in flow until it hits `top: 0`, then it pins. Needs
`top` to be set or it does nothing. `z-index` keeps it above the content
scrolling under it.

### Hero with two CTAs

```css
.hero-actions {
  display: flex;
  gap: 16px;
  flex-wrap: wrap;
  justify-content: center;
}
```

`flex-wrap` means the second button drops below instead of overflowing on
a phone — no media query needed (Day 4, §4).

### Trust bar / logo strip

```css
.trust-list {
  display: flex;
  flex-wrap: wrap;
  gap: 32px;
  justify-content: center;
  opacity: 0.7;
}
```

### 3-step "How it works"

```css
.steps {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
  counter-reset: step; /* start a counter at 0 */
}
.step::before {
  counter-increment: step; /* +1 per step */
  content: counter(step); /* draw the number */
}
```

`::before` invents an element that isn't in your HTML — perfect for
decorative numbers, which shouldn't be content.

### Stats band

```css
.stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  text-align: center;
}
```

Big number = `font-size: 40px` + bold; label underneath = small and
muted. That size contrast IS the design.

### Testimonial

`<blockquote>` + `<cite>` are the correct tags. A large quote mark via
`::before` and an oversized `font-size` on the quote does the rest.

### Contact form layout

```css
.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr; /* name | email */
  gap: 16px;
}
input,
textarea {
  width: 100%;
  padding: 12px;
  border: 1px solid var(--grey-300);
  border-radius: var(--radius);
  font: inherit; /* forms DON'T inherit fonts by default */
}
```

`font: inherit` on inputs is the one everyone forgets — without it your
form looks like it's from 1998.

No JavaScript today. `<form>` with `required` on the inputs gives you
free browser validation; real handling is Day 8.

---

## 8. The responsive pass (Day 4, applied)

Build desktop first, then do **one** narrowing pass at the end:

```css
@media (max-width: 900px) {
  .steps,
  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 600px) {
  .steps,
  .stats-grid,
  .form-row {
    grid-template-columns: 1fr;
  }
  .section {
    padding: 48px 0;
  }
  .hero h1 {
    font-size: 30px;
  }
}
```

Note the shortcut: comma-separated selectors collapse four sections into
one rule. And shrinking `.section` padding on a phone is the single
highest-value line — desktop vertical space feels enormous on a 375px
screen.

---

## 9. Gotchas

1. **Styling before the HTML is complete.** Write all ten sections'
   markup first, then style top to bottom. Otherwise you rewrite CSS
   every time you add a section.
2. **A new colour per section.** Stick to the tokens in `:root`. If you
   need a new shade, add it there — never inline a raw hex.
3. **Inconsistent section padding.** The #1 tell of a hand-built page.
   One `.section` class, used everywhere.
4. **Multiple `h1`s** because they look nice big. One `h1`, then `h2`.
5. **`position: fixed` nav without compensating.** A `fixed` nav is
   removed from flow and covers your hero's top. `sticky` avoids this —
   prefer it.
6. **Anchor links jumping under a sticky nav.** Fix with
   `html { scroll-behavior: smooth; }` and `section { scroll-margin-top: 80px; }`.
7. **A form with no `<label>`s.** Fails accessibility and looks lazy in a
   portfolio review.
8. **Images with no `width`/`height` or `alt`.** Causes layout shift
   (Day 14 will measure you on it).

---

## 10. The 30-minute version

You are not building ten sections today. The nav, hero, services grid and
footer are already in `index.html`. You need to add **two** new sections —
that's enough to prove you can extend a system instead of restyling a page.

| Time  | Do this                                                                  |
| ----- | ------------------------------------------------------------------------ |
| 0–5   | Open `styles.css`, read the `:root` block. That's §3. Don't type yet.     |
| 5–8   | Open the page in the browser. Change `--blue-400` to `#e11d48`. Watch every button, eyebrow and link change at once. Change it back. **That is the lesson.** |
| 8–20  | Build the **stats band** (task 3) — HTML + CSS. Dark background, 4 numbers. |
| 20–26 | Build the **CTA band** (task 5) — three elements, reuses `.btn`.          |
| 26–30 | Fill the two media queries at the bottom. Commit.                         |

Why those two sections: the stats band is the one that teaches
`.container` inside a full-bleed background (§4) and forces you to use the
dark tokens; the CTA band takes 5 minutes and proves the button system
(§5) means you write **zero** button CSS to add a new call to action.

The trust bar, steps, testimonial and contact form are marked **OPTIONAL**
in the files. Skip them today with a clear conscience — the form comes
back properly on Day 8 anyway.

**Self-check before you commit (60 seconds):**

- [ ] Ctrl+F your new CSS for `#` — zero raw hex outside `:root`.
- [ ] Every new section is `<section class="section …">` with a
      `<div class="container">` inside it.
- [ ] You wrote no new button CSS.
- [ ] DevTools device mode at 375px: no horizontal scrollbar.

**Reference:**

- [MDN — Using CSS custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)
- [MDN — HTML sectioning elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Element#content_sectioning)
- [MDN — position: sticky](https://developer.mozilla.org/en-US/docs/Web/CSS/position#sticky)
- Steal shamelessly: open stripe.com, vercel.com, or a competitor of
  Sophic's, and count their sections. It's always this list.
