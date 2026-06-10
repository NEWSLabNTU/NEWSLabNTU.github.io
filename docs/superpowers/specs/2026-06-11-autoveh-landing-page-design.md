# AutoVeh Landing Page — Design

**Date:** 2026-06-11
**Repo:** `NEWSLabNTU.github.io`
**Deploy URL:** https://newslabntu.github.io/autoveh/ (the NEWSLab AWF poster QR points here)
**Status:** Approved (design), ready for implementation plan

## Goal

A public landing page for the NEWSLab (NTU CSIE) Autonomous Vehicle team and its
collaboration with the Autoware Foundation. One scroll. Project-focused: the
vehicles, the research topics, and the open-source repos/books — not people,
publications, or news. It is the public face the poster QR resolves to.

## Scope

**In scope**
- A single static page at `/autoveh/` in the existing GitHub Pages repo.
- Sections: hero (sliding photos + overlay text) → Vehicles (3 columns) →
  Topics (full sections, each with intro prose + related repos/books) → footer
  (NTU × AWF logo lockup + links).
- Links out to the project repos, the AutoSDV / nano-ros books, and Prof. Shih's page.

**Out of scope (explicitly cut during brainstorming)**
- Team / members grid, publications list, news feed, demo-reel section.
- Any interactive command-line / heavy animation.
- Touching the repo root `index.html` (still redirects to the wiki) or the
  existing book/rustdoc submodules.
- `autoware_sentinel` or any internal-only material (see project constraints).

## Deployment & hosting

- The repo `NEWSLabNTU.github.io` is **plain static GitHub Pages** — no Jekyll,
  no build system. Submodules render mdBooks as static dirs.
- The page lives in a new subdirectory: `autoveh/`. GitHub Pages serves
  `autoveh/index.html` at `…github.io/autoveh/`.
- **No build step.** Hand-authored `index.html` + `style.css` + a tiny inline
  JS slideshow + an `assets/` folder. This matches the repo's existing pattern
  and keeps the page trivially maintainable.
- All asset paths are **relative** (`assets/…`) so the page works under the
  `/autoveh/` subpath without base-URL juggling.

## Layout (approved — mockup `v8-nohdr`)

Single column, full-width sections, vertical scroll. No sticky top nav (removed
so the hero + vehicle columns are visible on landing).

```
┌─────────────────────────────────────────────┐
│ HERO  (h=300px, full-bleed sliding photos)    │
│   overlay (left): eyebrow / H1 / lede         │
│   slide dots (bottom-left)                     │
├─────────────────────────────────────────────┤
│ VEHICLES   eyebrow "Platforms" + H2           │
│   [ AutoSDV ] [ Golf Cart ] [ YunTech Bus ]   │  3 equal columns: photo, h4, blurb
├──────────────── hairline divider ────────────┤
│ TOPICS     eyebrow + H2                        │
│   each topic = kicker / H3 / intro paragraph   │
│   + repo & book buttons                        │
│     01 Software-Defined Vehicle Platform       │
│     02 ROS on MCU & the Safety Island          │
│     03 Launch Orchestration & Deployment       │
│     04 Sensor Calibration & Coop Perception    │
│     05 GPU-Accelerated Localization & 5G       │
├─────────────────────────────────────────────┤
│ FOOTER (navy)  NTU × AWF logo lockup + label   │
│   © row + project list                         │
└─────────────────────────────────────────────┘
```

## Visual system (treatment "T2")

- **Fonts** (Google Fonts via `<link>`):
  - Headings (hero H1, section H2, topic H3, vehicle h4): **Source Serif 4**, 700.
  - Body, eyebrows, kickers, buttons: **Inter**, 400/600.
  - Fallbacks: `Georgia, serif` and `system-ui, sans-serif`.
- **Palette**
  | Token | Hex | Use |
  |-------|-----|-----|
  | Cream bg | `#F6F4EE` | page background |
  | Ink | `#252320` | body text |
  | Body-muted | `#3D3A33` | topic paragraphs |
  | Navy | `#1C1850` | headings, links, footer bg |
  | Tan-muted | `#7C7565` / `#9A9384` | eyebrows, kickers |
  | Hairline | `#E6E2D8` | dividers, card/button borders |
  | Button bg | `#FFFDF8` | repo/book buttons |
  | Burnt-orange | `#B1430A` | "book" link icon accent only |
  | Scrim | `rgba(20,18,14,.74)→.1` | left-dark hero gradient over photos |
- **Type scale:** hero H1 36px/1.08 · section H2 27px · topic H3 23px ·
  body 15px/1.78 · vehicle blurb 13px · eyebrow/kicker 11–12px uppercase, letter-spacing ~2px.
- **Restraint** (per Anthropic / Logitech refs): no card shadows, no chrome
  gradients beyond the hero scrim, hairline dividers only. Color comes from the
  photos and logos, not the UI.
- **Section padding:** ~36–50px. Footer navy `#1C1850`.

## Hero slideshow

- 4 slides cross-fading; `opacity` transition 1.1s; advance every 3.5s via a
  ~12-line vanilla-JS `setInterval` (no library).
- Slide images (resized to ≤1600px, q82): driving shot, golf cart, cooperative
  perception, AutoSDV platform.
- Overlay text + dot indicators sit above the scrim. Overlay text is **static**;
  only the background photos cross-fade (animation must not steal focus).
- Accessibility: respect `prefers-reduced-motion` → freeze on the first slide.

## Content

### Hero
- Eyebrow: `NTU NEWSLab × Autoware Foundation`
- H1: **Autoware as a Practical Research Platform**
- Lede: "Software-defined autonomous vehicles built on Autoware — cross-compiled,
  deployed, and driven on real platforms from a research cart to a full-size bus."

### Vehicles (3 columns)
| Vehicle | Blurb |
|---------|-------|
| AutoSDV | Software-defined vehicle reference platform — our flagship Autoware testbed. |
| Golf Cart | NTU × NTUST autonomous campus shuttle running the full Autoware stack. |
| YunTech Bus | Full-size bus — scaling the stack toward public-road class vehicles. |

### Topics (intro prose + links) — copy is draft, to be wordsmithed by author
1. **Software-Defined Vehicle Platform** → AutoSDV (GitHub) · AutoSDV Book
2. **ROS on Microcontrollers & the Safety Island** → nano-ros (GitHub) · nano-ros Book
3. **Launch Orchestration & Deployment** → play_launch (GitHub) · colcon2deb (GitHub)
4. **Sensor Calibration & Cooperative Perception** → LCTK (GitHub)
5. **GPU-Accelerated Localization & 5G** → cuda_ndt_matcher (GitHub)

### Links (all verified to exist, HTTP 200)
- AutoSDV repo: https://github.com/NEWSLabNTU/autosdv
- AutoSDV Book: https://newslabntu.github.io/autosdv-book/
- nano-ros repo: https://github.com/NEWSLabNTU/nano-ros
- nano-ros Book: https://newslabntu.github.io/nano-ros-book/
- play_launch: https://github.com/NEWSLabNTU/play_launch
- LCTK: https://github.com/NEWSLabNTU/LCTK
- colcon2deb: https://github.com/NEWSLabNTU/colcon2deb
- autoware-localrepo: https://github.com/NEWSLabNTU/autoware-localrepo
- cuda_ndt_matcher: https://github.com/NEWSLabNTU/cuda_ndt_matcher
- Prof. Chi-Sheng Shih: https://www.csie.ntu.edu.tw/~cshih/ (linked in footer)

### Footer
- NTU × AWF logo lockup + label "National Taiwan University · NEWSLab × Autoware Foundation"
- © row + project list. Prof. Shih link.

## Assets

Source from the poster pool `deliverables/02-poster-newslab-awf/assets/` (NTU repo
`IV26_Workshop`); copy into `autoveh/assets/` and resize for web (≤1600px, q82).

| File | Source |
|------|--------|
| `logo/ntu_emblem_t.png` | poster `assets/logo/ntu_emblem_t.png` |
| `logo/autoware_foundation.png` | poster `assets/logo/autoware_foundation.png` |
| hero/vehicle: AutoSDV | `assets/autosdv/coss_driving.jpg`, `vehicle_right.jpg`, `IMG_3008.jpg` |
| golf cart | `assets/golfcart/cart_front.jpg` |
| YunTech bus | `assets/bus/yuntech_bus.jpg` |
| coop perception | `assets/perception/coop_perception_rgb.jpg` |

Final image selection per slot to be confirmed at build; favicon optional.

## File structure (new)

```
NEWSLabNTU.github.io/
└── autoveh/
    ├── index.html      # the page
    ├── style.css       # all styling (T2 system above)
    └── assets/
        ├── logo/{ntu_emblem_t.png, autoware_foundation.png}
        ├── hero/{slide1..4}.jpg
        └── vehicles/{autosdv,golfcart,bus}.jpg
```

(JS slideshow is small enough to inline at the bottom of `index.html`.)

## Non-goals / constraints

- Do **not** modify repo root `index.html`, `.gitmodules`, or submodules.
- Do **not** reference `autoware_sentinel` or confidential DBW specs.
- Add `.superpowers/` to repo `.gitignore` (brainstorm scratch).
- Keep total page weight modest; all images pre-resized.

## Open items (resolve at build, non-blocking)
- Final photo per hero slide / vehicle card.
- Topic prose wording (author to polish; technical claims already sanity-checked:
  AGX-class compute, V2X, AWF reference-design WG).
- Whether to also surface autoware-localrepo as a button under Topic 3 (currently colcon2deb).
