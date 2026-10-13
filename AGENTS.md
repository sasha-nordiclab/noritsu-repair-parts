# Noritsu QSF-V50 roller — handoff

## Purpose

Rack transport roller for the Noritsu QSF-V50 film processor. Document: `NoritsuV50_Roller.fcstd`.
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

Roller: ProfileSketch → RollerRevolve → CavitySketch (XY) → CavityPocket (through-all up)
→ CavityPattern (mirror X, Y) → RibSketch → RibPad (L_roll) → RibPattern (polar 4).
RollerPrint: PrintProfile (XZ, 14-point poly, symmetric flanges) → PrintRevolve
→ PrintCavityLowSketch / PrintCavityLow (two sides: through-all down + 3.4 tapered roof up)
→ PrintCavityHighSketch / PrintCavityHigh (mirror image by params) → PrintCavityPattern (polar 4)
→ PrintRibSketch (trapezoid) → PrintRibPad (L_roll) → PrintRibPattern (polar p_rib_n)
→ PrintEndRecessSketch / PrintEndRecess (groove: annulus p_hub..p_cav clear to end_depth at
both ends; spokes start at the hub at end_depth and slope out to the shell).
FreeCAD 26.3 pocket API: SideType 'Two sides', Type / Type2, TaperAngle2 (negative = inward).

## Decisions (2026-10-13)

- Original cleanup: removed generated-face external refs, the unintended 1 mm web, DatumPlane +
  mid mirror, hidden Body001.
- Print copy rev 2 (user): both flanges symmetric (cone under each), mid web restored but printable
  (tapered roofs ≤ 40°, residual flat roof 4.75 mm² per cavity), all walls multiples of 0.6, all
  feature Z levels on the 0.2 grid, 8 trapezoid grip ribs for a rubber (TPU) sleeve to go on next.
- Rev 3 (user): original end recesses restored (clear 5 mm around the pin at both ends, same
  depth top and bottom — unknown mating structures); hub wall 2.4 for a stiffer pin.
- Only asymmetry about z = 30: the bore elephant-foot chamfer at the bed (5 mm³).

## Verified

- Roller: valid single solid, V 14719.1 mm³, bbox 30 × 30 × 70.5.
- RollerPrint: valid single solid, V 19501.2 mm³, bbox 30 × 30 × 70.4, worst overhang 39.7°,
  recesses verified empty z 0–5 and 55–60. Orca (A1 NordicLab, 0.20mm NordicLab 0.6 Ballance,
  AzureFilm PETG HF Black): 1 h 25 min, 24.6 g.

## Next / open

- Rubber wheel/sleeve over the drum between the flanges: radial room above d_roll = 1.75 mm,
  ribs take 0.6 of it. Flange rim 1.75 is not a 0.6 multiple (d_fl 30 kept as machine size).
- Hub stub now Ø11.3 (was 9.4): confirm it fits its mating part.
- Free-spinning or fixed on the shaft? p_bore 6.5 assumes free running on Ø6.

## Resume

fdmkit MCP `run(expr)`; `tree(); chk(); P()` to orient (tree lists the active Body).
