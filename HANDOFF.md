# qaun.homes — build brief

Production build of an approved demo. `reference/demo.html` is a working single-file
prototype with all real content and images wired in. It is the reference for behaviour and
visual language. **Rebuild it properly in Astro. Do not redesign it.**

---

## 1. What this site is

Portfolio for **Qaun Huyen** — Vietnamese form **Quản Huyền**, legal name **Vũ Lê Hoàng**.
Interdisciplinary artist, b. 2002, based in Hanoi, working across moving image, sound and
real-time systems.

The work is about **decentralisation** — systems with no center — read through Vietnamese
vernacular gambling. Two concepts drive every design decision:

- **suýt** — the near-miss. An outcome landing one position short. Engineered, not accidental.
- **gỡ** — playing on to recover a loss. The belief that the field owes you.

The interface is the argument, not decoration on top of it:

- The homepage is a **00–99 lottery number board** (bảng đề). Most numbers hold nothing.
- Every load **deals a different arrangement**. Nothing persists between visits.
- On load a highlight walks the board and **stops one cell short of a work**. That is the suýt.
- Content opens in **draggable windows** that stack. The visitor arranges their own layout;
  no reading order is authored. Decentralisation at the interface level.
- Non-work cells carry **readings** — 30 aphorisms assigned by number, the way numbers carry
  meaning in a dream book (sổ mơ). The board reshuffles; the readings stay with their numbers.

**Do not** convert this into a conventional grid portfolio.

---

## 2. Stack

| Layer | Choice |
|---|---|
| Framework | **Astro** |
| Styling | **Plain CSS** — no Tailwind |
| Content | **Content collections** from `content/*.json` — nothing hardcoded |
| Interactivity | Vanilla JS islands, board + windows only. No React. |
| Hosting | **Vercel**, GitHub auto-deploy |
| Domain | **qaun.homes** |

---

## 3. Design system — do not change

```css
--paper:#FFFFFF; --ink:#000000; --mid:#5C5C5C; --dim:#C2C2C2;
--edge:rgba(0,0,0,.15); --edge-2:rgba(0,0,0,.06);
```

**Strictly black and white.** No accent colour anywhere. State is communicated by inversion,
weight and rule thickness only.

Type: **Archivo** (400/500/600/700) for structure; **Martian Mono** (300/500) for numbers,
labels and metadata, uppercase with wide tracking. Both from Google Fonts.

**Verify Vietnamese diacritics render**: `Bật Rễ`, `Thoả`, `Quẩy Rồng`, `Vũ Lê Hoàng`.

Windows use macOS-style chrome with **outlined** traffic lights, no colour. 1px black border,
soft drop shadow.

---

## 4. Board rules

100 cells `00`–`99`, 10×10, square, hairline rules. States:

| State | Appearance | Behaviour |
|---|---|---|
| **work** | black, 500, 2px underline | opens Works window at that row |
| **upcoming** | black, 500, 1px grey underline, narrower | same |
| **fragment** | grey, no rule | shows a reading in the readout |
| **empty** | `--dim` | shows a reading on hover |

Hover/focus on a live cell inverts it. Assignment is randomised per load — do not persist.

Roughly **one third of cells live**. If most fill up the board becomes a menu and the concept
dies. Do not pad with invented entries.

### The suýt animation
After ~700ms a highlight walks pseudo-randomly across cells, decelerating, then lands on a cell
**adjacent to** a randomly chosen work, which flashes inverted at the same moment. Readout reads
"One short". Both clear after ~2.6s. Skip entirely under `prefers-reduced-motion`.

---

## 5. Windows

**About** — document layout: right-aligned email, large name, role lines, `STATEMENT` and
`BIOGRAPHY`, one portrait image. Vietnamese terms set in weight 500 inline.

**Works** — table, columns `Medium / Role / Project / Date`, large type, hairline rules.
Three labelled groups: `Upcoming` → `Works and performances` → `Platforms and studios`.
Clicking a row expands a panel: title, year, medium, link and credits left; description right;
a **horizontal image carousel** below.

Carousel: fixed height, width by native aspect ratio (no cropping — portrait and landscape sit
side by side), `scroll-snap-type: x mandatory`, prev/next buttons, image count in mono.
Single-image projects hide the buttons.

**Contact**
```
Email      info@qaun.homes
Instagram  @01.shenzi  ·  @shenzi.00
Website    qaun.homes
Line       (+84) 898 555 443
```

---

## 6. Content

Already extracted, ready to load. Do not retype.

- `content/works.json` — 10 works, full descriptions, credits, links, 106 image references
- `content/readings.json` — 30 board readings (`heading`, `body`)
- `content/profile.md` — statement and biography source
- `images/<slug>-NN.jpg` — 109 images, flat folder, no subdirectories, max 2000px long edge, ~28 MB total

Slugs: `ttc` `chia` `nguoc-xuoi` `bat-re` `metal-box` `virtual-bleeps` `quay-rong` `thoa`
`netchua` `34-studio` `portrait`

**`nguoc-xuoi` is the slug for the exhibition titled `→←`.** The display title is the symbol;
the slug must never be. Same rule for any future symbol title.

Works schema:
```ts
{ g:0|1|2, up?:1, m:string, r:string, p:string, y:string, slug:string,
  d:string, c:string, link:string, url?:string, med:[file,width,height][] }
```

---

## 7. Outstanding — resolve before launch

| # | Item |
|---|---|
| 1 | **Fragment titles** — 24 board cells still read `Test render 01` etc. Either give them real titles, point them at real images, or remove the fragment tier entirely. Do not ship generic names. |
| 2 | **Video** — none embedded. Metal Box has YouTube, Mixcloud and SoundCloud links in `works.json`; other live sets have no recordings linked yet. |
| 3 | **34 Studio residency count** — source text reads `[NUMBER] artists join us on residency each year`. Unfilled, currently omitted from the site copy. |
| 4 | **Image weight** — 28 MB total. Lazy-loaded and only fetched when a panel opens, so first paint is light, but a visitor opening all ten projects pulls nearly all of it. Convert to WebP/AVIF with responsive `srcset` — expect ~60% reduction. |
| 5 | **Name resolution** — site displays `Qaun Huyen`; published credits at NetChùa! and Aaja Radio read `Quản Huyền`. JSON-LD `alternateName` already covers both. Keep it. |

---

## 8. Build order

1. Scaffold Astro, wire fonts, port CSS custom properties.
   **Show the folder structure and content collection schemas, then stop and wait for approval.**
2. Load `content/*.json` as collections. No hardcoded arrays.
3. Board as a JS island — port randomisation, readings and the suýt animation from the demo.
4. Window system as a second island — drag, z-order, Esc, focus trap.
5. Carousel — native scroll-snap, buttons, no library.
6. Accessibility: board is a labelled group; live cells are real `<button>`s; windows are
   `role="dialog"`. **Readings on empty cells are currently hover-only — unreachable by keyboard
   and by touch. Fix this.**
7. Responsive images, lazy loading below the fold.
8. Meta, OG, favicon, robots, sitemap — all already written in `reference/demo.html`, port them.

---

## 9. First prompt

> I'm rebuilding an approved demo as a production Astro site. Read `HANDOFF.md` and
> `reference/demo.html` before doing anything.
>
> The design is approved and must not be redesigned. Strictly black and white. The number-board
> homepage, the per-load randomisation, the near-miss animation, the readings and the draggable
> windows are load-bearing concepts, not decoration.
>
> Content is already extracted in `content/`. Do not retype it.
>
> Start by proposing the Astro project structure and the content collection schemas. Explain each
> directory. Do not write component code until I approve the structure.
