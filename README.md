# MAMF AI — Orbit

The hub for every internal tool the team has built. One page, no build step, no dependencies.

`MAMF AI` sits at the center. Six categories orbit it. Hovering a category enlarges it and fans out the
tools that belong to it; clicking opens a side panel with descriptions and links. A plain directory of
everything sits below the map for anyone who just wants the list.

## Editing

Everything lives in the `CATEGORIES` block near the top of the `<script>` in `index.html`.

```js
{
  key:"prospecting",
  name:"Prospecting",
  color:"var(--p-prospecting)",     // token defined in :root
  blurb:"One line shown on the card and in the side panel.",
  orbit:2, phase:0.68,              // which ring, and where on it (0–1)
  tools:[
    { name:"PBS8 Database",
      url:"https://mrare-cmd.github.io/HAP-Database/",
      gated:false,                  // true adds a "sign-in" badge
      note:"One line shown in the side panel." }
  ]
}
```

- Leave `url:""` and the tool renders as **link pending** — useful for parking something before it ships.
- A category with an empty `tools:[]` still gets a planet; it just reads "no tools yet".
- `ORBITS` below the config sets each ring's radius (as a fraction of the map) and orbital period.
- To keep two planets from crowding each other, put them on the same `orbit` with `phase` values
  half a turn apart (e.g. `0.18` and `0.68`).

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

- A single self-contained `index.html`. The only external request is the IBM Plex webfont from Google Fonts.
- The OpEx Dashboard points at the app's own domain rather than a Cloudflare Access login URL — those
  login links carry a short-lived token and stop working within minutes.
- Respects `prefers-reduced-motion`: orbits hold still, everything stays usable.
- Keyboard accessible — planets are buttons, so tabbing through the map reveals each category's tools.
