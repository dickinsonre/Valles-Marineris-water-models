# Valles Marineris Water Models

An interactive, single-file browser app that works through what it would take to settle Mars's Valles Marineris canyon by the year 3000 CE — and treats the water side of it as a real hydraulic-engineering problem, not hand-waving.

**Live demo:** open `index.html` (or the GitHub Pages link once Pages is enabled).

## Why this exists

The canyon is ~7 km deep and up to ~200 km wide — the Grand Canyon would vanish inside it. That depth is the whole point: descend far enough and the atmosphere thickens, radiation drops, and liquid water becomes thermodynamically plausible at the floor. Once you have water flowing, you have the same problems I've spent fifty years modeling on Earth — a pressurized distribution network and a gravity collection network — just under Martian pressure and gravity. This app builds those problems out, with working solvers, and lets you size them.

It started as a depth/width comparison and grew, tab by tab, into a full resource model: water, atmosphere, power, weather, and the two hydraulic engines that tie them together.

## What it does

Sixteen tabs, in five groups:

**Narrative** — Why the Canyon, Depth Profile (pressure vs. depth on a scale-height model), and a dated 1000-year timeline.

**Vision** — Districts & People (100 million across 12 districts, 5 billion theoretical cap) and ten year-3000 engineering systems.

**Physics calculators** — each is a live calculator with sliders, computed metrics, and an inline diagram:
- **Habitat Designer** — dome sizing, population, air volume, skin-stress savings from the canyon's higher floor pressure
- **Canyon Lid** — treats a roof as a pressure vessel; computes upward air force and the wall anchor tension, showing why narrow side-canyon spans beat a full-width lid
- **Cost & Feasibility** — material selector (steel through carbon-nanotube), build cost, and the required-vs-available tether strength
- **Water & Population** — the water balance: population = fresh ice-melt supply ÷ per-capita makeup after recycling, plus a Mars-reserves readout
- **Atmosphere Balance** — where breathable air comes from, coupling oxygen production to the water loop via electrolysis
- **Weather** — psychrometric dew-point model showing when a roofed section would actually rain
- **Power & Atmosphere** — required rim pressure for a breathable floor, and why surface solar can't be the backbone on Mars

**Hydraulic engines** — the core:
- **EPANET · Distribution** — a looped pressurized network solved with Hardy-Cross iterative balancing and the Hazen-Williams head-loss law
- **SWMM · Stormwater** — a time-stepped nonlinear-reservoir runoff model with Manning conduit routing, producing a real hydrograph
- **Model Scale** — estimates how large the full-canyon EPANET and SWMM5 models would be (element counts, .inp size, runtime), shows why you must decompose into district models, and **exports real, openable `.inp` files**

**Documentation** — a "How It Works" tab with wireframe diagrams of the app's own architecture.

## The .inp exports

The Model Scale tab generates standards-compliant input files:
- **EPANET 2.2** — `[JUNCTIONS]`, `[RESERVOIRS]`, `[TANKS]`, `[PIPES]`, `[PUMPS]`, `[CURVES]`, `[OPTIONS]`, `[COORDINATES]`
- **SWMM 5.2** — `[SUBCATCHMENTS]`, `[SUBAREAS]`, `[INFILTRATION]`, `[JUNCTIONS]`, `[OUTFALLS]`, `[CONDUITS]`, `[XSECTIONS]`, `[TIMESERIES]`

Two scopes: a small sample network that opens instantly, and a full-district build at the real engine ceiling (~100,000 elements, ~10–18 MB) that opens and runs on a desktop. The full canyon is deliberately *not* exported as one file — at ~33 million conduits it's a 7.5 GB monster that no current engine can open. That's the point the Model Scale tab makes: decompose into districts, couple at the trunk interceptor.

Ready-to-open sample files are in [`examples/`](examples/).

## How it works

One self-contained `index.html`: Tailwind via CDN, vanilla JavaScript, hand-drawn inline SVG. No build step, no external data, no backend. It runs offline and deploys to any static host. Every tab is a set of controls plus a `render()` function — move a slider, the numbers and diagram recompute live.

The hydraulic figures use the genuine governing equations (Hazen-Williams, Manning, the SWMM5 nonlinear reservoir, Mars gravity 3.71 m/s², a ~11 km atmospheric scale height). The speculative figures are clearly framed as illustrative. The whole app deliberately holds both the grounded physics and the year-3000 vision side by side.

## Running it

- **Locally** — open `index.html` in any modern browser.
- **GitHub Pages** — Settings → Pages → Deploy from a branch → `main` / root. Live at `https://dickinsonre.github.io/Valles-Marineris-water-models/`.
- **Any static host** — drop `index.html` anywhere.

## Credits

Built by Robert (Bob) Dickinson. The hydraulic engine logic draws on EPANET and SWMM5, which are public-domain models from the US EPA. This is an independent educational and thought-experiment tool — not an official Autodesk, Innovyze, or EPA product, and not a calibrated engineering design.

MIT licensed — see [LICENSE](LICENSE).
