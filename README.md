# MAMF AI — Node Network

The hub for every internal tool the team has built. One page, no build step, no dependencies.

A frosted glass core reading `MAMF AI` sits at the centre. Each of the six categories is wired to the
core by a single spoke — they do not cross-link to each other — on a ring that turns once every ten
minutes. Data packets run the spokes in and out of the core. Hovering a node holds the ring still, lights
its spoke, and pushes its tools outward onto a second circumference; hovering any one of those tools
lifts the whole set, with the one under the cursor lifting furthest. Clicking opens a side panel with
descriptions and links. A plain directory sits below the map for anyone who just wants the list.

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
- `R_CAT`, `R_SAT`, `R_CORE_IN`, `FAN`, `SPIN` and `PACKETS` below the config control the geometry, the
  rotation period in seconds, and how much traffic rides the spokes.

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
git remote add origin https://github.com/<owner>/mamf-ai-hub.git
git push -u origin main
```

Then in **Settings → Pages**, set Source to `Deploy from a branch`, branch `main`, folder `/ (root)`.
The hub lands at `https://<owner>.github.io/mamf-ai-hub/`.

## Notes

- A single self-contained `index.html`. The only external request is the Archivo / Instrument Sans webfonts from Google Fonts.
- The OpEx Dashboard points at the app's own domain rather than a Cloudflare Access login URL — those
  login links carry a short-lived token and stop working within minutes.
- Respects `prefers-reduced-motion`: the ring holds still and the packets are not created at all.
- Keyboard accessible — each node is focusable, so tabbing through the map reveals each category's tools.
- The rotation is driven by one `requestAnimationFrame` loop that moves a single mesh transform plus
  eleven node holders, so labels never rotate and hover never has to chase a moving target.
