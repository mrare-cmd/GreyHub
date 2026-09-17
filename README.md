# MAMF AI — Node Network

The hub for every internal tool the team has built. One page, no build step, no dependencies.

A frosted glass core reading `MAMF AI` sits at the centre. Each of the six categories is wired to the
core by a single spoke — they do not cross-link to each other — on a ring that turns once every ten
minutes. Data packets run the spokes in and out of the core. Every category carries its own tools in a
tight orbit around it, so the count is readable at a glance without hovering anything.

**Hovering a category promotes it.** It draws in toward the middle of the map (to `R_FOCUS`, not all the
way to the centre) and grows; its tools swing out into a wide orbit around it; the core fades and yields;
and the other five categories swing round the circle to a tight 104° arc on the opposite side, shrinking
as they go. Everything is interpolated frame by frame, so it reads as one continuous motion in both
directions. Hovering a tool lifts the whole set, the one under the cursor furthest.

**Clicking a category flashes its tools** — an expanding pulse ring and a double blink — and does nothing
else. There is no side panel. The only click-through targets are the tools themselves, which open in a
new tab.

The full directory is a dropdown off the top-right of the masthead, so it overlays rather than taking
room from the map. The map is sized off the viewport height (`--stage`), so the whole network is visible
without scrolling.

Behind all of it, a constellation of ~60 faint points drifts the opposite way at a third the speed. It is
decoration only: no pointer events, well below the wires in brightness, and it dims further when a node is
active. It exists to give the field depth, not to imply connections.

Light steel blue (`#B0C4DE`) is the only hue on the page — nothing is colour-coded by category.

## Editing

Everything lives in the `CATEGORIES` block near the top of the `<script>` in `index.html`.

```js
{
  name:"Prospecting",
  blurb:"One line shown on the card and in the side panel.",
  tools:[
    { name:"PBS8 Database",
      url:"https://mrare-cmd.github.io/HAP-Database/",
      gated:false,                  // true marks it as needing a sign-in
      note:"One line shown in the side panel." }
  ]
}
```

- Leave `url:""` and the tool renders as **link pending** — useful for parking something before it ships.
- A category with an empty `tools:[]` still gets a node; it renders hollow and reads as unmapped.
- Categories are spaced evenly around the ring automatically — order in the array is clockwise from the top.
- The geometry block below the config controls everything spatial. `R_CAT` / `R_MINI` set the resting
  layout; `R_FOCUS`, `FOCUS_R`, `BACK_R`, `R_BACK`, `BACK_ARC` and `TOOL_ORBIT` set the focused one — how big the
  promoted category gets, how small and how far out the others go, how wide their arc is, and where the
  tools ring the centre. `SPIN` and `MINI_SPIN` are revolution times in seconds.
- Every position is interpolated toward a target each frame (categories in polar coordinates, so they
  swing along the circle rather than cutting across it), which is why the layout reads as one motion
  instead of two states.

## Current contents

| Category | Tool | Link |
| --- | --- | --- |
| Underwriting | OpEx Dashboard | `gs-expense-comps.pages.dev` (Cloudflare Access) |
| Client Tools | Inspection Assistant | `mrare-cmd.github.io/walkthrough/app.html` |
| Prospecting | PBS8 Database | `mrare-cmd.github.io/HAP-Database/` |
| Prospecting | TOPA Dashboard | `mrare-cmd.github.io/mrare/` |
| Prospecting | Priority Dashboard | `mrare-cmd.github.io/greysteel-hampton-roads/` |
| Comparables | — | awaiting links |
| Production | — | awaiting links |
| Miscellaneous | — | awaiting links |

## Publishing to GitHub Pages

```bash
git push -u origin main
```

The `origin` remote is already set to `https://github.com/mrare-cmd/GreyHub.git`.

Then in **Settings → Pages**, set Source to `Deploy from a branch`, branch `main`, folder `/ (root)`.
The hub lands at `https://mrare-cmd.github.io/GreyHub/`.

## Notes

- `index.html` plus the icon set. The only external request is the Archivo / Instrument Sans webfonts
  from Google Fonts.
- `logo.jpg` is the source mark. The tab and bookmark icons are generated from it: `favicon.ico`
  (16/32/48), `icon-192.png`, `icon-512.png` and `apple-touch-icon.png`. They are cropped to the mark's
  bounding box plus 10%, so it fills the tile instead of floating in the original's padding. Replace
  `logo.jpg` and regenerate to change them.
- The OpEx Dashboard points at the app's own domain rather than a Cloudflare Access login URL — those
  login links carry a short-lived token and stop working within minutes.
- Respects `prefers-reduced-motion`: the ring holds still and the packets are not created at all.
- Keyboard accessible — each node is focusable, so tabbing through the map promotes each category in
  turn; Enter or Space flashes its tools.
- One `requestAnimationFrame` loop drives everything positional: the spoke group's rotation, each
  category's place on the ring, each tool's interpolated position and radius, and the tether geometry.
  Nothing rotates that carries text, so labels never need counter-rotating.
