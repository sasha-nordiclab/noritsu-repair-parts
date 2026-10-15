# Noritsu QSF-V50 rack roller — handoff

Replacement film transport roller for the Noritsu QSF-V50 racks: PETG core on a Ø6 mm shaft plus a TPU
sleeve printed flat and wrapped on. Document `roller/cad/NoritsuV50_Roller.FCStd`, STEP in `roller/step/`,
renders in `roller/docs/img/`. Published at github.com/sasha-nordiclab/noritsu-repair-parts (latest
release `roller-v1.5`). Everything is driven by the `params` sheet. English only in this repo.

## Tree (folders)

| Folder | Bodies (labels) | Notes |
| --- | --- | --- |
| 01 Cores | `Roller`, `RollerPrint`, `RollerPrintDovetail` | original design · print core · print core + dovetail slot (BaseFeature = RollerPrint) |
| 02 Grid sleeve | `SleeveFlatGrid`, `SleeveFlatGridOrig`, `SleeveFittedGrid`, `SleeveFittedGridOrig`, groups `SleeveGridTexture`, `SleeveGridTextureOrig` | flat print sheets; fitted = base only, grid shown by the link groups |
| 03 Smooth sleeve | `SleeveFlatSmooth`, `SleeveFlatSmoothOrig`, `SleeveFittedSmooth`, `SleeveFittedSmoothOrig` | plain 1.75 mm, flush with the Ø30 flanges |
| 04 Dovetail sleeve | `SleeveFlatSmoothDovetail`, `SleeveFittedSmoothDovetail` | smooth sheet with half-dovetail keys at both ends |
| 05 Grid tiles | `GridCell`, `GridLow`, `GridHigh`, `GridTopOrig` | hidden link sources for the fitted grid |

"Orig" = for the original `Roller` (flat Ø30 × 0.5 flanges, 4 rectangular ribs). Object Names differ from labels
(e.g. `Body` = Roller, `SleeveFlat` = SleeveFlatGrid, `SleeveTexture` = SleeveGridTexture).

## Key parameters

- Core: L_roll 60, d_roll 26.5, d_fl 30, d_bore 6 (shaft) / p_bore 6.5 (printed), p_hub 11.3, p_cav 21.7,
  p_spoke 1.8, p_web 2 (mid partition), p_fl 0.6 + fl_cone_h 2.2 (40° cones), 8 trapezoid ribs 1.8/1.2 × 0.6.
  The original `Roller` keeps its 1 mm mid partition (`web`) and Ø6 bore.
- Grid sleeve: base t_sl 1.2 + grid h_tx 1.0, grid_w 1.2 (2 × 0.6 lines) at ±45°, kn_n 21 cells around,
  kn_s_fix 4.264 (pitch), sl_len = kn_n·kn_s_fix = 89.55 (neutral layer of base + grid, no preload).
  Widths sl_w_in 54.0 (40° bevel) / o_w_in 58.6 (square). Rib grooves 0.1 clearance, seam on a rib.
- Smooth sleeve: t_smooth = (d_fl − d_roll)/2 = 1.75, sm_len = 2π(d_roll/2 + t_smooth/2) = 88.75.
- Dovetail: dt_neck 2.4, dt_base 3.6, dt_depth 1.2 slot over a spoke, open through the top flange, stop at
  z 2.8; keys with dt_clear 0.15. Sheet prints outer face down, keys up (keys lean 30°).

## Verified (2026-10-15)

All bodies one valid solid. Fitted sleeves: overlap 0 and void 0 against their cores. Grid tiles do not overlap
cores or sleeves; copies tile without overlap. Sheet seams: cross-sections at both ends identical. Orca (A1,
AzureFilm): PETG core 1 h 25 min 24.6 g; TPU 0.6 nozzle — grid 1 h 17 min 10.7 g, smooth 1 h 3 min 10.2 g,
dovetail 1 h 4 min 10.9 g. The user printed the core and the v1.1 grid sheet (sheet was 4 mm short → fixed).

## Open

- Print-test the corrected sheet lengths; calibrate with the measured gap (grid: change kn_n per 4.26 mm cell or
  kn_s_fix; smooth: t_smooth/sm_len).
- Dovetail tube must stretch over the Ø30 top flange when slid on; reduce that flange if too hard.
- Hub stub Ø11.3 (original 9.4): confirm it fits its mating part.

## Working rules learned

- Heavy steps (patterns of crossing ridges, helices) first in a headless `freecadcmd` copy with a subprocess
  timeout (stdin=DEVNULL, os._exit(0)); GUI calls time out at 120 s and cannot be cancelled.
- After a param change, recompute body by body (`o.recompute()` per feature) and `purgeTouched()`; a full
  `d.recompute()` of this document takes > 7 min. Re-recomputing a helix tile is slower than rebuilding it.
- Flat grid sheets: trim after the first ridge direction, before the second (crossings stay inside the sheet).
- Fitted grid = one tile × App::Link PlacementList (not expression-driven). Rebuild after changing kn_n, kn_s,
  p_fl, t_fl or L_roll: Low [0], Mid 1…kn_rows−2, High [kn_rows−1] at z = p_fl + k·kn_s, rotation m·kn_a;
  Orig: Main 0…o_rows−2 from z = t_fl, Top [o_rows−1].
- Set body colours after the features exist; hide App::Origin children after activating bodies; renders: fitAll,
  then scale the ortho camera height by w/h when the window is portrait, saveImage(1600, 1200, 'White').

## Resume

fdmkit MCP `run(expr)`; `tree(); chk(); P()` to orient. Names avoid unit tokens (`H`, `L` are rejected aliases).
