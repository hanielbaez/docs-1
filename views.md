---
description: Accessing data and insights from simulation runs
---

# Views

**HASH lets users access their simulation outputs in a variety of ways.** These include custom viewers for the 3D rendering, geospatial display, and charting/plotting of outputted simulation state.

Using the playbar at the bottom of the right-hand view pane you can scrub forwards and backwards in time through any existing simulation state.

## 3D

**Support for a full range of 3D models in Core's 3D viewer is coming soon.** In the meantime, the following primitives can be used to give form to agents: `box`, `cone`, `cylinder`, `dodecahedron` , `icosahedron`, `octahedron`, `plane`, `sphere`, `tetrahedron`, `torus`, `torusknot`

## Geospatial

**Agents can be rendered on a map-view of the world by providing them with latlong co-ordinates.**  Various examples in the HASH Index showcase this \(e.g. [Local Competition](https://hash.ai/index/5e55694e10a3261046bfa8a6/local-competition)\)

## Plots

**Charts and plots are in many cases the best way to view simulation outputs in HASH.** When you're not interested in the behavior of an individual agent, but aggregate trends over time or outcomes observed across the entire system, graphing variables of interest can enable you to quickly identify and analyze outcomes.

## Raw Output

**The underlying JSON of any individual step is accessible from the 'Raw Output' tab.** This can be useful for debugging models during development, and allows for the raw state of simulations to be exported for further analysis.

