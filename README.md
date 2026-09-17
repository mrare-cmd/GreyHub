# MAMF AI — Node Network

The hub for every internal tool the team has built. One page, no build step, no dependencies.

A frosted glass core reading `MAMF AI` sits at the centre. Each of the six categories is wired to the
core by a single spoke — they do not cross-link to each other — on a ring that turns once every ten
minutes. Data packets run the spokes in and out of the core.

Every category carries its own tools in a tight orbit around it, so the count is readable at a glance
without hovering anything. Hovering a category holds the ring still, lights its spoke, and flings those
tools out to the satellite ring, where they grow, pick up labels and stay tethered to their parent.
Hovering any one of them lifts the whole set, with the one under the cursor lifting furthest. Clicking a
category opens a side panel with descriptions and links. The full directory is a dropdown off the
top-right of the masthead, so it overlays rather than taking room from the map.

The map is sized off the viewport height (`--stage`), so the whole network is visible without scrolling.

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
- The geometry block below the config controls everything spatial: `R_CAT` / `R_SAT` / `R_MINI` are the
  category ring, the pushed-out ring, and the little orbit a tool keeps at rest; `CAT_R` / `SAT_R` /
  `MINI_R` are the three bead sizes; `SPIN` and `MINI_SPIN` are revolution times in seconds.
- A tool's rest position and its pushed-out position are interpolated every frame by a smoothed `t`, so
  the flight out and back is one continuous motion rather than two states.

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

- `index.html` plus `favicon.svg`. The only external request is the Archivo / Instrument Sans webfonts
  from Google Fonts.
- `favicon.svg` is the GreyHub mark redrawn as vector — a node ring around a glowing G on a blue-violet
  gradient — so it stays sharp at every tab size and costs ~4KB. Replace that one file to change it.
- The OpEx Dashboard points at the app's own domain rather than a Cloudflare Access login URL — those
  login links carry a short-lived token and stop working within minutes.
- Respects `prefers-reduced-motion`: the ring holds still and the packets are not created at all.
- Keyboard accessible — each node is focusable, so tabbing through the map reveals each category's tools.
- One `requestAnimationFrame` loop drives everything positional: the spoke group's rotation, each
  category's place on the ring, each tool's interpolated position and radius, and the tether geometry.
  Nothing rotates that carries text, so labels never need counter-rotating.
