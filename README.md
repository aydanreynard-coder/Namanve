# AGL Shift Simulator

A 3D browser simulation of one planned 12-hour day shift (06:00–18:00) at the AGL Logistics Hub, Namanve: 40 lorries through security, the weighbridges, the marshals, the docks, the container yard and the Bühler coffee plant. Play it at 1×–300×, pause, or drag to any moment.

`index.html` and `agl-site-data/` are byte-identical copies of the published claude.ai artifact "AGL Shift Simulator" (version 1790607868-2292, updated 2026-09-28).

## Run it

The page loads its data with `fetch()`, so it has to be served over HTTP. Opening `index.html` straight from disk stops at "Loading drawing data…".

```sh
python3 -m http.server 8000
# open http://localhost:8000/
```

Any static host works (GitHub Pages, Netlify, an internal web server) as long as `index.html` and the `agl-site-data/` folder are uploaded together.

It needs internet access for:

- **three.js 0.160.0** and **es-module-shims 1.10.0** from `cdn.jsdelivr.net`. These are required; the page won't start without them.
- **Archivo** and **Inter** from Google Fonts. These are optional; without them it falls back to system fonts.

To run it on a network that blocks jsDelivr, download those two npm packages (same versions) and point the import map near the bottom of `index.html` at the local copies.

## Files

| Path | Contents |
|---|---|
| `index.html` | The whole app: UI, 3D scene build, shift timetable, player. One ES module, about 2,300 lines. |
| `agl-site-data/site.json` | Site geometry from the AGL site layout: boundary, gates, weighbridges, container slots, racks, lights, trees, parked vehicles. |
| `agl-site-data/plant.json` | Bühler plant geometry: walls, columns, slabs, stairs, machines, pits, silo legs. |
| `agl-site-data/ground_n.jpg`, `ground_s.jpg` | Ground textures for the north and south halves of the site. |
| `agl-site-data/ground_mini.jpg` | Minimap image. |

## Where to change things in `index.html`

| Line | What |
|---|---|
| ~1135 | `THE SHIFT`: the timetable. Every lorry, person and machine is planned as a function of the shift clock. |
| ~1521 | Bühler plant throughput, `PLANT_TPH` (6 t/h). |
| ~1532 | Bulk export: blower rate `BLOW_TPH` (24 t/h), container load `CONT_T` (21 t), opening bulk-bin stock `S0` (40 t). |
| ~1921 | Shift player: clock, speed, scrubbing, the log, daylight that follows the clock. |
| ~1968 | `PLACES`: the "Go to" camera views. |
| ~1990 | `TOUR`: the guided-tour route. |

## Known quirks

- The subtitle in the HTML says "34 lorries". On load it's replaced with the real count from the timetable (40).
- "Blown into containers" (84 t by 18:00) is more than "Coffee processed" (72 t) because the bulk bin starts the shift holding 40 t (`S0`). The bin level is shown only on the plant's 3D label, and the "How the shift works" panel doesn't mention the opening stock.
