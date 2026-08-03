# Qảun Huyền — website build brief

Handoff for building the production site. The file `index-a-bangde.html` is an approved
single-file demo. It is the reference for behaviour and visual language. Rebuild it properly;
do not redesign it.

---

## 1. What this site is

Portfolio for **Qảun Huyền** (legal name Vũ Lê Hoàng), an interdisciplinary artist in Hanoi
working across moving image, sound and real-time systems.

The artist's work is about **decentralisation** — systems with no center — and Vietnamese
vernacular gambling. Two concepts drive every design decision:

- **suýt** — the near-miss. An outcome landing one position short. Engineered, not accidental.
- **gỡ** — playing on to recover a loss. The belief that the field owes you.

The interface is not decoration on top of this. It *is* the argument:
- The homepage is a **00–99 lottery number board** (bảng đề). Most numbers hold nothing.
- On every load the board **deals a different arrangement**. Nothing persists between visits.
- On load an animation runs across the board and **stops one cell short of a work**. This is
  the suýt. It must survive the rebuild.
- Content opens in **draggable windows** that stack. The visitor arranges their own layout;
  no reading order is authored. This is decentralisation at the interface level.

**Do not** "improve" any of the above into a conventional grid portfolio.

---

## 2. Stack

| Layer | Choice | Note |
|---|---|---|
| Framework | **Astro** | Content site. Ship zero JS on pages that don't need it. |
| Styling | **Plain CSS** | No Tailwind. Custom properties already defined in the demo. |
| Content | **Markdown / content collections** | Works and fragments as data, not hardcoded. |
| Interactivity | Vanilla JS islands | Board + windows only. No React. |
| Hosting | Vercel or Netlify | Free tier, GitHub auto-deploy. |
| Domain | **qaun.homes** | Already owned. |

---

## 3. Design system — do not change these

```css
--paper: #FFFFFF;
--ink:   #000000;
--mid:   #5C5C5C;   /* fragments, secondary text */
--dim:   #C2C2C2;   /* empty cells */
--edge:  rgba(0,0,0,.15);
--edge-2:rgba(0,0,0,.06);
```

**Strictly black and white.** No accent colour anywhere. State is communicated by
inversion, weight and rule thickness only.

Type:
- **Archivo** — everything structural (400 / 500 / 600 / 700)
- **Martian Mono** — numbers, labels, metadata (300 / 500), uppercase, wide tracking

Both from Google Fonts. **Verify Vietnamese diacritic rendering** — the name contains `ả`, `ề`,
and work titles contain `Bật Rễ`, `Thoả`, `Quẩy Rồng`. If Martian Mono lacks Vietnamese
coverage it is only used for digits and Latin labels, which is acceptable — but check.

Windows use a macOS-style chrome with **outlined** traffic lights (no colour). 1px black
border, soft drop shadow.

---

## 4. Board rules

- 100 cells, `00`–`99`, 10×10 grid, square aspect, hairline rules.
- Three states:
  - **work** — black, weight 500, 2px underline rule. Clickable, opens Works window at that row.
  - **upcoming** — black, weight 500, 1px grey underline rule (narrower). Not yet realised.
  - **fragment** — grey (`--mid`), no rule. Clickable, shows in readout.
  - **empty** — `--dim`, not interactive.
- Hover / focus on any live cell inverts it (black fill, white text).
- Cell assignment is **randomised per page load**. Do not persist it.
- Ratio matters: roughly **one third of cells live**. If most cells fill up, the board becomes a
  menu and the concept dies. Currently 10 works + 24 fragments. Adjust `FRAGS` if the real
  fragment count differs — do not pad with invented entries.

### The suýt animation
On load, after ~700ms, a highlight walks pseudo-randomly across cells, decelerating, then
lands on a cell **adjacent to** a randomly chosen work. The work cell flashes inverted
simultaneously. The readout reads "One short". Both clear after ~2.6s.
Respect `prefers-reduced-motion`: skip the animation entirely.

---

## 5. Windows

Three: **About**, **Works**, **Contact**.

- Fixed position, draggable by the title bar, z-index raises on mousedown.
- Close via the first traffic light or `Esc`.
- On viewports under 720px they go full-screen.
- Opened from the nav or from a board cell.

**About** — reproduces the artist statement and biography, laid out like a document:
right-aligned email, large name, role lines, then `STATEMENT` and `BIOGRAPHY` sections.
Vietnamese terms (`xóc đĩa`, `suýt`, `gỡ` …) are set in weight 500 inline.

**Works** — a table, columns `Medium / Role / Project / Date`, large type, hairline rules.
Three labelled groups in this order:
1. **Upcoming** — TTC (2026), Chia (2027)
2. **Works and performances** — 8 realised entries, reverse chronological
3. **Platforms and studios** — NetChùa! (2026), 34 Studio (2021)

Clicking a row expands a panel below it: title + year + medium and credits on the left,
description on the right, then a media grid underneath. Upcoming entries carry an outlined
badge reading `In development — demo material only`.

**Contact**
```
Email      info@qaun.homes
Instagram  @shenzi.01
Website    qaun.homes
Line       (+84) 898 555 443
```

---

## 6. Content model

```ts
// works
{
  slug: string          // URL-safe. See warning below.
  group: 0 | 1 | 2      // upcoming | works | platforms
  upcoming?: boolean
  medium: string        // SIMULATION, LIVE A/V, EXHIBITION, BROADCAST, PLATFORM…
  role: string          // Artist | Co-founder
  title: string
  year: string
  description: string
  credits: string       // venue, city, collaborators — multiline
  link?: { label: string; url?: string }
  media: { src: string; alt: string; caption: string; credit?: string }[]
}
```

```ts
// fragments — test renders, broken states, stills kept rather than corrected
{ id: string; title: string; note: string; src: string }
```

---

## 7. Outstanding work — must be resolved before launch

| # | Item | Status |
|---|---|---|
| 1 | **Images** for the 8 realised works | Artist supplying a zip. Placeholders currently render as hatched boxes. |
| 2 | **Fragment titles** | Currently generic (`Test render 01`). Must be replaced with real titles or the fragment count reduced. Generic names read as padding. |
| 3 | **`→←` slug** | The exhibition title is a symbol. It will break in URLs, filenames and metadata. Needs a safe slug — suggest `nguoc-xuoi`. Display title stays `→←`. |
| 4 | **Photo credits** | Every documentation image needs a photographer credit. Non-negotiable in an artist portfolio. |
| 5 | **Name / search mismatch** | Site displays `Qảun Huyền`; people will search `Quản Huyền`. Add both to meta description and structured data so search resolves. Use `Qaun Huyen` as the official unaccented form for international forms and music credits. |
| 6 | **Phone number exposure** | The Line number is public and will be scraped. Artist's call — flag once, then respect the decision. |
| 7 | **Video** | Not in the zip. Host unlisted on Vimeo/YouTube and embed. |

---

## 8. Build order

1. Scaffold Astro, wire fonts, port the CSS custom properties. **Show the folder structure and
   wait for approval before writing components.**
2. Move works and fragments into content collections. No hardcoded arrays.
3. Board as a JS island. Port the randomisation and the suýt animation verbatim from the demo.
4. Window system as a second island — drag, z-order, Esc, focus trap.
5. Accessibility: the board is a labelled group; live cells are real `<button>`s; windows are
   `role="dialog"` with keyboard escape; verify focus order.
6. Performance budget: **under 3s on 4G**. Images responsive and lazy-loaded below the fold.
7. Meta, Open Graph, favicon, sitemap.

---

## 9. First prompt to use

> I'm rebuilding an approved single-file demo (`index-a-bangde.html`) as a production Astro
> site. `HANDOFF.md` in this repo is the spec — read both before doing anything.
>
> The design is approved and must not be redesigned. Strictly black and white. The number-board
> homepage, the per-load randomisation, the near-miss animation and the draggable windows are
> load-bearing concepts, not decoration.
>
> Start by proposing the Astro project structure and the content collection schemas.
> Explain each directory. Do not write component code until I approve the structure.
