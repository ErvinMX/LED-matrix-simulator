# LED Matrix Design Studio

A browser-based design-planning tool for exploring LED matrix layouts using the **IS31FL3741A** LED driver (**39 CS × 9 SW = up to 351 individually addressable LED positions**).

Use it to sketch LED arrangements, compare alternative shapes, estimate board dimensions and preliminary two-layer routing congestion, and share design proposals before committing to a KiCad layout.

> **Important:** This is a conceptual planning tool, **not** a PCB autorouter, circuit simulator, optical simulator, thermal solver, or manufacturing validation tool. All PCB dimensions and routing indicators must be verified against the actual components, enclosure, design rules, and a routed PCB in KiCad.

## Try it

**No installation or server is required.**

1. Download `LED_Matrix_Design_Studio_Import_Compare.html` from this repository.
2. Open the HTML file in a modern desktop browser.
3. Use the tabs to create a layout, generate a CS/SW mapping, inspect preliminary PCB estimates, or compare saved designs.

The app runs locally in the browser. Project files are saved and loaded as JSON.

## Features

| Tab | What it does |
| --- | --- |
| **Layout** | Choose grid dimensions and LED pitch; add/remove physical LEDs; create circular masks; set light colour and brightness. Larger underlying grids are supported, while the installed LED count is limited to 351. |
| **Image** | Preview an uploaded image on the installed LED positions using brightness, threshold, and dithering controls. |
| **Physical** | Calculate the LED-array envelope from grid dimensions, pitch, package size, and edge margin; check against an entered available area. |
| **Animation** | Preview simple patterns and scrolling text. |
| **Compare** | Import two saved project JSON files and compare their actual LED masks and pitches at the **same physical scale**, with a metrics table and PNG export. Manual comparison is also available. |
| **Electrical** | Assign unique CS/SW pairs using row-major, column-major, serpentine, or physical-row-group strategies; inspect or export mappings. |
| **PCB Estimator** | Estimate a rectangular LED-section envelope, bottom controller extension, theoretical trace capacity between footprints, abstract net congestion, and via fit for a **two-copper-layer** board. |
| **Board Shape** | Generate candidate rectangular or stepped board outlines around the installed LEDs, retaining a bottom controller/connector extension; export SVG, vertices CSV, or PNG. |
| **Export** | Save/load project JSON, export pattern or provisional KiCad placement CSV, and download a preview image. |

## Comparing two proposed designs

1. Configure the first design in **Layout**, generate its mapping in **Electrical**, and evaluate it in **PCB Estimator**.
2. In **Export**, select **Download project JSON** and give the file a descriptive name (for example, `Design_A_Rounded.json`).
3. Repeat for the second design and save another JSON file (for example, `Design_B_Circular.json`).
4. Open **Compare** and import the two JSON files under **Design A** and **Design B**.
5. Review the side-by-side previews and comparison table, then select **Download comparison PNG** to share the visual comparison.

Importing files in **Compare** does **not** overwrite the current working design. Use **Export → Load saved project** if you want to reopen a design for editing.

### Example design candidates

These are preliminary examples from a design exploration, **not verified or recommended production layouts**:

| Metric | Rounded arrangement | Circular arrangement |
| --- | ---: | ---: |
| Underlying grid | 19 × 18 | 23 × 23 |
| Installed LEDs | 330 | 351 |
| Pitch | 6.0 mm | 6.5 mm |
| Mapping approach | Physical row groups | Column-major |
| Estimated maximum corridor demand | 33% | 59% |
| Estimated controller-neck demand | 61% | 63% |

These percentages are **model-specific proxies** and cannot be interpreted as measured copper utilisation or guaranteed routing margin. The designs differ in LED count, pitch, geometry, and mapping, so the numbers do not isolate the effect of board shape alone.

## Understanding the PCB estimates

- **Unused channels:** Unpopulated CS/SW combinations out of the driver's 351 available positions. These are *not* unused individual IC pins.
- **LED array envelope:** Estimated rectangular width and height containing the grid, LED package dimensions, and specified edge margins. It is not a routed PCB outline.
- **T-shaped board:** The estimator's simplified model of a wide rectangular LED section with a narrower **bottom controller extension**. Its neck width and height are user-entered assumptions; confirm the real connector, controller, mounting, and enclosure dimensions before finalising.
- **Controller neck fanout / capacity:** A simplified estimate of how heavily the narrow region between the controller and LED matrix is used by connections. It does not model actual driver pad escape.
- **Via body fits clear gap:** A geometric check of whether the selected via diameter could fit between LED footprints. It does not place vias or check every pad/trace clearance.
- **Two-layer corridor demand:** A theoretical demand-to-capacity comparison for nets travelling between LED positions, using approximate net paths. It does **not** draw traces or prove that the board is routable.
- **Board Shape outline:** A geometric suggestion that preserves a bottom connector extension. The auto selection balances outline area and complexity; it is **not** based on real autorouting.

The **Physical** tab's *maximum pitch that fits* depends on the available width/height entered there. If those are placeholders, the displayed maximum pitch is not a fixed constraint imposed by the product.

## Before manufacturing

Verify the following in the actual hardware design:

- LED and driver datasheets, including forward voltage, multiplexed current, power consumption, and thermal limits.
- Actual LED footprint, pad geometry, trace widths, clearances, drill/via sizes, and fabrication capabilities.
- CS/SW net mapping, real two-layer trace routing, driver fanout, power delivery, and ground return paths.
- Bottom extension geometry, magnetic connector footprint/location, enclosure fit, and mechanical mounting.
- Optical appearance under the real diffuser and worst-case temperatures at the intended brightness.
- KiCad electrical-rule checks, design-rule checks, and manufacturing outputs.

## Files

- `LED_Matrix_Design_Studio_Import_Compare.html` — standalone browser simulator.
- `README.md` — usage guide and limitations.
- Optional `*.json` — example project files saved using the simulator's **Export** tab.

## Project status

**Prototype / design exploration.** Calculations and visuals are intended to help compare concepts, not certify electrical, thermal, optical, or manufacturing feasibility.
