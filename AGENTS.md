# Noritsu QSF-V50 roller — handoff

## Purpose

Rack transport roller for the Noritsu QSF-V50 film processor. Document: `roller/cad/NoritsuV50_Roller.FCStd` (STEP in `roller/step/`, renders in `roller/docs/img/`).
Two Bodies share one `params` sheet:
- `Roller` — the design as intended (nominal geometry), hidden.
- `RollerPrint` — print-optimised copy, visible. Printed bottom end down (z = 0 face on the bed,
  axis = print Z), no supports. PETG (warm C-41 bath).

## Params

Shared: L_roll 60, H_total 70.5, d_roll 26.5, d_fl 30, t_fl 0.5, d_hub 9.4, d_bore 6 (shaft Ø6,
confirmed by user), d_cav 22, end_depth 5, spoke 1, rib_w 1, rib_h 0.5, rib_embed 0.2.
Print only (tuned to 0.6 mm lines / 0.2 mm layers): p_bore 6.5, p_hub_wall 2.4 → p_hub 11.3,
p_shell 2.4 → p_cav 21.7, p_spoke 1.8, p_fl 0.6, fl_cone_h 2.2 (38.5° cone under both flanges),
p_H_total 70.4, p_web 2.0 (mid web), cav_taper 36 / cav_taper_h 3.4 (tapered cavity roofs),
p_rib_n 8, p_rib_base 1.8, p_rib_w 1.2 (top), p_rib_h 0.6, bed_ch 0.3 × bed_ch_h 0.4,
bore_ch 0.4 × bore_ch_h 0.6, spoke_ch_h 6.4 (spoke end slope, 39.1°), end_open 1
(groove sketch overshoot past the end faces). ovh is unused.
Names avoid unit tokens (`H`, `L` are rejected as aliases).

## Trees

Roller: ProfileSketch → RollerRevolve → CavityLowSketch (XY) / CavityLowPocket (up, (L_roll − web)/2)
→ CavityHighSketch (XY + L_roll) / CavityHighPocket (down, same length) → CavityPattern (mirror X, Y) → RibSketch → RibPad (L_roll) → RibPattern (polar 4).
RollerPrint: PrintProfile (XZ, 14-point poly, symmetric flanges) → PrintRevolve
→ PrintCavityLowSketch / PrintCavityLow (two sides: through-all down + 3.4 tapered roof up)
→ PrintCavityHighSketch / PrintCavityHigh (mirror image by params) → PrintCavityPattern (polar 4)
→ PrintRibSketch (trapezoid) → PrintRibPad (L_roll) → PrintRibPattern (polar p_rib_n)
→ PrintEndRecessSketch / PrintEndRecess (groove: annulus p_hub..p_cav clear to end_depth at
both ends; spokes start at the hub at end_depth and slope out to the shell).
FreeCAD 26.3 pocket API: SideType 'Two sides', Type / Type2, TaperAngle2 (negative = inward).

## Decisions (2026-10-13)

- Original cleanup: removed generated-face external refs, DatumPlane + mid mirror, hidden Body001.
  The 1 mm mid web (partition, z 29.5–30.5, param `web`) was first removed by mistake and restored
  on 2026-10-14 at the user's request: it is part of the original design.
- Print copy rev 2 (user): both flanges symmetric (cone under each), mid web restored but printable
  (tapered roofs ≤ 40°, residual flat roof 4.75 mm² per cavity), all walls multiples of 0.6, all
  feature Z levels on the 0.2 grid, 8 trapezoid grip ribs for a rubber (TPU) sleeve to go on next.
- Rev 3 (user): original end recesses restored (clear 5 mm around the pin at both ends, same
  depth top and bottom — unknown mating structures); hub wall 2.4 for a stiffer pin.
- Only asymmetry about z = 30: the bore elephant-foot chamfer at the bed (5 mm³).

## Verified

- Roller: valid single solid, V 15004.6 mm³ (incl. the 285.5 mm³ mid web), bbox 30 × 30 × 70.5.
- RollerPrint: valid single solid, V 19501.2 mm³, bbox 30 × 30 × 70.4, worst overhang 39.7°,
  recesses verified empty z 0–5 and 55–60. Orca (A1 NordicLab, 0.20mm NordicLab 0.6 Ballance,
  AzureFilm PETG HF Black): 1 h 25 min, 24.6 g.

## Status (2026-10-15): finished, release roller-v1.4 (length fixed, smooth and dovetail variants)

Core, both sleeves (flat print sheets + fitted references), STEP files, renders, README and Ko-fi text
are complete and published (github.com/sasha-nordiclab/noritsu-repair-parts). Not yet print-tested.

## Next / open

- Print test: core in PETG, sheet in TPU with a 0.6 mm nozzle; check sheet tension and seam on the rib.
- Hub stub now Ø11.3 (was 9.4): confirm it fits its mating part.
- Free-spinning or fixed on the shaft? p_bore 6.5 assumes free running on Ø6.

## Rubber sleeve (TPU), 2026-10-14

> Texture superseded: the pyramid knurl (`Knurl*` tiles, `kn_n` 30) below was replaced by the diamond
> grid — see "Texture rev 2". Fits, grooves and sheet sizes below are still current.

Two Bodies, same params (`t_sl` 1.2 base, `h_tx` 1.0 knurl, `kn_n` 30, `preload` 0.02,
`sl_clear` 0.1, `sl_end_gap` 0.2, `sl_bevel` 40; derived `sl_w_in` 54.0, `sl_len` 85.28,
`kn_s` 2.843, `kn_pp` 2.010, groove `gr_b` 2.0 / `gr_t` 1.4 / `gr_h` 0.7).
- `SleeveFlat` — print part. Flat TPU sheet, inner face on the bed, placed beside the roller
  (Placement x = d_fl + 10). SheetProfile/SheetPad (40° bevelled ends) → RibGroove pattern
  (8 full + half grooves at both ends, so the seam lands on a rib) → KnurlA/KnurlB: 90° V-grooves
  at ±45° (sketches rotated by AttachmentOffset), linear patterns `kn_cnt` 83 each. Verified: one
  valid solid, V 6714.2; knurl area fractions 0.81/0.25/0.01 at 10/50/90 % height (exact square
  pyramids); cross-sections at both seam ends identical.
- `SleeveFitted` — fit model on `RollerPrint`: rubber base only, outer Ø28.5 (`fit_rb` =
  sl_rb − kn_skin). Exact zero-tolerance fit: inner = drum Ø26.5, grooves = exact rib trapezoid,
  ends lie on the flange cones (`fit_z_in` 2.8, `fit_z_out` 1.543). Verified: overlap and void
  between core and sleeve 0.0000 mm³ (checked before the skin change; skin only moved the outer).
- Fitted texture (group `SleeveTexture`), visual model: the knurl is invariant under a 12°
  rotation (`kn_a` = 360/kn_n) and a `kn_s` 2.843 axial shift, so only one tile is computed:
  `KnurlCell` = 12° × kn_s ring piece r 14.25–15.45 (0.2 skin `kn_skin` under the pyramids keeps it
  one solid) cut by 2 × 7 short SubtractiveHelix pieces (polar 12° steps). `KnurlLow` / `KnurlHigh`
  are the same tile trimmed to the flange cone + flange plane (`kn_top` = 1.946 local height of the
  last row). Copies are App::Link arrays with PlacementList (no booleans): KnurlTextureLow 30,
  KnurlTextureMid 570 (rows 1…kn_rows−2), KnurlTextureHigh 30. Each tile builds in 10–15 s.
  Verified: tiles do not overlap core or sleeve and touch the sleeve; seam sampling covers each
  point exactly once; texture V 2818.1 mm³ (tile 4.626, low 3.710, high 2.332).
  The PlacementList is not expression-driven: after changing kn_n, kn_s, p_fl or L_roll, rebuild
  the three links (script in the handoff below).
- Reliability rule learned: never pattern long helices around a full ring in PartDesign (60
  crossing helices hung FreeCAD > 16 min). Trial heavy steps in a headless `freecadcmd` copy with a
  subprocess timeout (stdin=DEVNULL and os._exit(0), or freecadcmd waits at its console) before
  running them in the GUI.
- Open: rubber over the rib grooves is only 0.5 mm (t_sl 1.2 − gr_h 0.7); t_sl 1.4 would give 0.7.
  Knurl tips reach Ø30.9 (user accepted texture past the Ø30 flanges).

## Sleeve for the original roller (2026-10-14)

> Texture superseded by the grid (`OrigGrid*` links, `o_rows` 14, `o_top` 3.567); everything else current.

Same sleeve concept on `Roller` (flat Ø30 × 0.5 flanges, 4 rectangular ribs 1 × 0.5 from r 13.05 to 13.75).
Params: `o_w_in` 58.6, `o_gr_w` 1.2, `o_gr_h` 0.6, `o_rows` 21, `o_top` 2.146.
- `SleeveFlatOrig` (Placement x = 2·(d_fl + 10) + 25): OrigSheetProfile/Pad (square ends) →
  OrigRibGroove pattern (PolarPattern.Occurrences + 1, seam on a rib) → OrigKnurlA/B (same as SleeveFlat).
  Verified: one solid, V 7491.9; knurl fractions 0.81/0.249/0.01; seam sections identical.
  Orca *0.20mm NordicLab 0.6 Slow TPU*, AzureFilm TPU: 1 h 21 min, 9.1 g.
- `SleeveFittedOrig`: inner Ø26.5, outer fit_rb 14.25, z t_fl … L_roll − t_fl, grooves = exact rib
  rectangle, polar = PolarPattern.Occurrences. Verified: overlap 0.0000, void 0.0000, 4 ribs in grooves.
- Texture `SleeveOrigTexture`: `OrigKnurlTextureMain` = KnurlCell × (rows 0…o_rows−2) × kn_n = 600 links
  (from z = t_fl), `OrigKnurlTextureTop` = `KnurlTopOrig` (tile trimmed at local o_top) × 30. Links
  coloured by OverrideMaterial + ShapeAppearance. Verified: no overlap with roller or sleeve, z 0.5–59.5,
  V 2880.3.

## Texture rev 2: diamond grid (2026-10-14)

Pyramid knurl replaced by a raised diamond grid for a 0.6 mm nozzle: `kn_n` 20 (kn_s 4.264, kn_pp 3.015,
kn_a 18°), `grid_w` 1.2 (2 × 0.6 lines), `h_tx` 1.0, `grid_embed` 0.2, `grid_in` 0.5, `grid_x` trim reach.
- Flat sheets: base pad only t_sl thick → GridA ridges (+45°, rotated XZ sketch, pad sym, LinearPattern over
  lattice k = gA_k0…gA_k1 that strictly intersects the sheet) → TrimA side/end pockets → GridB (−45°) →
  TrimB → rib grooves last. Trimming between the two directions keeps the crossing booleans inside the
  sheet (a full-size base slab with crossings outside hung and crashed FreeCAD). Each step < 60 s.
  SleeveFlat V 8194.4, SleeveFlatOrig V 9024.9, one solid each.
- Fitted tiles GridCell/GridLow/GridHigh/GridTopOrig: full thin ring (z −kn_s…2kn_s) → 2 × 7 AdditiveHelix
  ridges → groove operation='common' to the kn_a × kn_s wedge (→ clip). ~7 s each. Links:
  GridTexture{Low,Mid,High} 20 + 240 + 20, OrigGridTexture{Main,Top} 260 + 20. Verified: no overlap with
  roller or sleeve, tiles touch the sleeve, rotated/shifted copies do not overlap, ridge fill 0.638 = expected.
- Orca with system *Bambu Lab A1 0.6 nozzle* + *0.20mm NordicLab 0.6 Slow TPU*, AzureFilm TPU:
  SleeveFlat 1 h 13 min 10.2 g, SleeveFlatOrig 1 h 16 min 11.2 g (0.4 nozzle: 1 h 25 / 1 h 21).
- Render note: the FreeCAD window is portrait (1022 × 1348); after fitAll scale the ortho camera height by
  w/h before saveImage(1600, 1200), or 4:3 images come out small. Hide App::Origin children after
  activating bodies.

## Sheet length fix (2026-10-15, after the user's print)

v1.1 sheets were too short to close: `sl_len` was 2π·(d_roll/2 + t_sl/2)·(1 − 0.02) = 85.28, i.e. the middle
of the base minus 2 % preload, ignoring the 1 mm grid that moves the neutral layer outward. Now
`preload` 0, `kn_s_fix` 4.264084 (grid pitch kept), `kn_n` 21, `sl_len` = kn_n·kn_s_fix = 89.55 mm
(neutral radius of base + 64 %-filled grid ≈ 14.23 → 89.43; rounded up to whole cells so the seam
matches). Both flat sheets 89.55 long, seam sections identical; tiles rebuilt for kn_a 17.14°, links
21 per row (GridTexture 21 + 252 + 21, OrigGridTexture 273 + 21), no overlaps. Calibrate with the next
print: change `kn_n` only if the gap/overlap is about one cell (4.26 mm); finer changes need `kn_s_fix`.
GUI note: re-recomputing an existing helix tile took ~2 min, rebuilding it from scratch 7 s; after a
param change recompute features body by body and `purgeTouched()`, never a full `d.recompute()`.
Orca 0.6 nozzle: SleeveFlat 1 h 17 min 10.7 g, SleeveFlatOrig 1 h 19 min 11.8 g.

## Smooth sleeve variant (2026-10-15)

User asked for a version without the grid on top. Params `t_smooth` 1.6 (8 layers), `sm_rn` = d_roll/2 + t_smooth/2
(14.05), `sm_len` = 2π·sm_rn = 88.28 (plain sheet: neutral layer at mid-thickness, no preload), `sm_ro` 14.85
(Ø29.7, inside the Ø30 flanges), `sm_z_out` (cone height at sm_ro).
- `SleeveFlatSmooth` (Placement x = d_fl + 10, y = −(sl_len + 10)): SmoothSheetProfile (40° bevel) → pad sm_len →
  8 trapezoid rib grooves (+ half at the seam). V 7297.6, 56.7 × 88.28 × 1.6. 59 min, 9.2 g (0.6 nozzle).
- `SleeveFlatSmoothOrig` (x = 2·(d_fl + 10) + 25, same y): square ends, 4 rectangular grooves. V 8108.2,
  58.6 × 88.28 × 1.6. 1 h 1 min, 10.1 g.
- `SleeveFittedSmooth` / `SleeveFittedSmoothOrig`: exact fits on RollerPrint / Roller, overlap 0, void 0.
- Colours (cyan / peach) must be set after the body's features exist, or they reset to grey.

## Dovetail version (2026-10-15)

User request: a separate print core with a dovetail slot; the flat sleeve's ends, rolled up, form a dovetail key
and the tube slides on from one side. Params `dt_neck` 2.4, `dt_base` 3.6, `dt_depth` 1.2, `dt_clear` 0.15.
- `RollerPrintDovetail`: Body with BaseFeature = RollerPrint → DovetailSlotSketch (XY + fit_z_in) → DovetailSlot
  pocket up through all at +Y (over a spoke; removes the rib there). Open through the top flange, blind at
  z 2.8 (axial stop). V 19243.2, one solid. 1 h 24 min, 24.3 g PETG.
- `SleeveFlatSmoothDovetail` (print orientation: outer face on the bed): 40° bevelled profile → pad sm_len →
  DtKeys (two half-dovetail keys, pad sym sl_w_in) → 7 rib grooves (seam at the slot, no rib there).
  V 7515.6, 56.7 × 88.28 × 2.65. Keys lean 29.7° from vertical. 58 min, 9.7 g TPU, 0.6 nozzle.
  The grid sleeve cannot carry keys: it must print inner face down, where the keys would hang in the air.
- `SleeveFittedSmoothDovetail`: smooth fitted + exact key in the slot; overlap 0, void 0.
- Open: the tube must stretch over the Ø30 top flange when slid on; if that is too hard, reduce or remove
  the top flange of this core.

## Resume

Rebuild texture links (now GridTexture*/OrigGridTexture*, z0 = p_fl or t_fl): for each (link, rows) in (Low, [0]), (Mid, 1…kn_rows−2), (High, [kn_rows−1]):
PlacementList = [Placement((0, 0, p_fl + k·kn_s), Rotation(Z, m·kn_a)) for k in rows for m in range(kn_n)].

fdmkit MCP `run(expr)`; `tree(); chk(); P()` to orient (tree lists the active Body).
