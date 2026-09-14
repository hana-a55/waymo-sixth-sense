# Waymo Sixth Sense

A concept operations layer for autonomous ride-hail fleets in the Austin metro area — routing that anticipates congestion the way a human driver does, a corridor scorecard, a staging optimizer, and a live fleet-coordination simulation that keeps vehicles positioned where demand actually is.

**Live demo:** https://claude.ai/code/artifact/392296b3-43ff-4ccd-8f31-a9ea74097920

This is an independent portfolio/concept project. It is not affiliated with, endorsed by, or built on any non-public data from Waymo, Uber, or the City of Austin.

## What it does

- **Landscape** — grounds the whole project in Austin's real transit system: Capital Metro's actual MetroRapid corridors and ridership, Project Connect's rail timeline, peer-reviewed research on whether ride-hail helps or hurts transit ridership, the empty-miles/VMT problem in robotaxi deadheading, and where Austin's own transit-access gaps are. Sourced from public data, not modeled — see the citations at the bottom of that tab. One finding from this research pass directly corrected the corridor model: the original "preferred" downtown route sat on CapMetro's dedicated transit-priority bus lanes, which is now fixed in the Corridors tab.
- **Anticipation rulebook** — 15 rules across five families (queue anticipation, curb & access, positioning & staging, energy discipline, transit awareness), each written as a human instinct, a measurable machine trigger, an action, and a sensor-degraded fallback.
- **Corridor scorecard** — 16 real Austin street segments scored on a six-term Segment Efficiency Index (volatility, pedestrian conflict, signal delay, curb deficit, energy cost, turn exposure), computed rather than hand-labeled.
- **Energy model** — accounts for the fact that Waymo's Austin fleet is electric (Jaguar I-PACE): propulsion drag plus a per-second sensor/compute/climate overhead term, which puts the fleet's cheapest mile at 30–35 mph rather than on the freeway or in gridlock.
- **Staging optimizer** — ranks legal waiting positions by a Staging Position Score (local demand, reachability, deadhead cost, curb quality, event bonus) across time-of-day, season, and event context.
- **Live Coverage** — a running (~1.5s tick) simulation of fleet-to-fleet coordination: 48 simulated vehicles report position, a coordinator computes per-zone coverage targets from live demand, matches the nearest surplus vehicle to each deficit, and explicitly avoids double-dispatching a zone that already has a vehicle en route. Includes a live public-conditions report form that shifts coverage targets in real time.
- **Integration** — message schemas (`RouteAdvisory`, `StagingDirective`, `CoverageDirective`) and a table mapping every synthetic input to the real production data source that would replace it.

## What's real vs. simulated

The routing rules, the corridor scoring formula, the energy model, and the fleet-coordination algorithm are all genuinely computed from the stated logic in the page — nothing is faked or hardcoded per scenario (the "South Congress needs 4+ free cars" example, for instance, falls out of the general coverage-target formula, not a special case for that street).

What isn't real: no external telemetry from Waymo, Uber, or the City of Austin backs any of it — that data isn't public at this granularity. The Live Coverage tab's 48 vehicles are a simulated fleet running entirely in your browser, and the public condition reports are stored per-browser (not shared across viewers) — both are clearly labeled as such on the page, with the Integration tab spelling out the production architecture each would map onto.

## Running it

Single self-contained file, no build step:

```
sixth-sense.html
```

Open it directly in a browser, or serve the folder with any static file server.

## Stack

Vanilla HTML/CSS/JS, inline SVG for all charts and maps, no dependencies or build tooling.
