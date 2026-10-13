# Noritsu QSF-V50 rack roller

Replacement film transport roller for the racks of the Noritsu QSF-V50 film processor. It is made of two printed parts:

- **Core (PETG):** the roller body on a Ø6 mm shaft, with end flanges and eight anti-rotation ribs.
- **Sleeve (TPU):** the rubber covering with a diamond knurl, printed flat as a sheet and wrapped onto the core.

> **Status:** designed and checked in CAD (fits, overhangs, slicing). Not yet print-tested in the machine.

<p align="center">
  <img src="docs/img/assembly.png" width="380" alt="Assembled roller: PETG core with knurled TPU sleeve">
  <img src="docs/img/core_petg.png" width="380" alt="PETG core with eight grip ribs">
</p>

## Files

| File | Use |
|---|---|
| [`step/Noritsu_V50_Roller_Core_PETG.step`](step/Noritsu_V50_Roller_Core_PETG.step) | Print: PETG core |
| [`step/Noritsu_V50_Roller_Sleeve_TPU_Flat.step`](step/Noritsu_V50_Roller_Sleeve_TPU_Flat.step) | Print: TPU sleeve as a flat sheet |
| [`step/Noritsu_V50_Roller_Assembly.step`](step/Noritsu_V50_Roller_Assembly.step) | Reference: core with the fitted, textured sleeve |
| [`step/Noritsu_V50_Roller_Sleeve_TPU_Fitted.step`](step/Noritsu_V50_Roller_Sleeve_TPU_Fitted.step) | Reference: the sleeve as it sits on the core |
| [`step/Noritsu_V50_Roller_Design_Reference.step`](step/Noritsu_V50_Roller_Design_Reference.step) | Reference: the original roller geometry before print optimisation |
| [`cad/NoritsuV50_Roller.FCStd`](cad/NoritsuV50_Roller.FCStd) | Parametric FreeCAD model with all of the above |

## Main dimensions

| | |
|---|---|
| Shaft / bore | Ø6 mm shaft, Ø6.5 mm printed bore (free running) |
| Running diameter | Ø26.5 mm core, about Ø30.9 mm over the knurl tips |
| Flanges | Ø30 mm, 0.6 mm thick, 40° cones underneath |
| Length | 60 mm between flange faces, 70.4 mm with the hub stub |
| Hub | Ø11.3 mm, 2.4 mm wall; free for 5 mm at both ends (end recesses) |
| Grip ribs | 8 × trapezoid, 1.8 mm base, 1.2 mm top, 0.6 mm high |
| Sleeve | 1.2 mm base + 1 mm diamond knurl, 90° pyramids at 2 mm pitch |

## Core (PETG)

<p align="center">
  <img src="docs/img/core_petg_bottom.png" width="380" alt="Core seen from the bottom end: end recess around the hub and spokes">
  <img src="docs/img/design_reference.png" width="380" alt="Original roller design before print optimisation">
</p>

- **Orientation:** bottom end face on the bed, roller axis vertical. No supports: every overhang is 40° or less from vertical.
- **Inside:** four spokes join the hub to a 2.4 mm shell. A 2 mm web at mid-length closes the cavities; its underside is a tapered roof, so it prints without bridging. The 5 mm end recesses around the hub are the same depth at both ends.
- **Slicing (measured):** OrcaSlicer, Bambu Lab A1 0.4 nozzle, *0.20mm NordicLab 0.6 Ballance*, AzureFilm PETG HF: **1 h 25 min, 24.6 g**. The walls are all perimeters, so forcing solid walls does not change the result.

## Sleeve (TPU)

<p align="center">
  <img src="docs/img/sleeve_flat_tpu.png" width="380" alt="Flat TPU sheet with diamond knurl">
  <img src="docs/img/sleeve_flat_tpu_bottom.png" width="380" alt="Underside of the TPU sheet with eight rib grooves">
</p>

- **Print flat:** inner face on the bed, knurl up. The sheet is 54.0 mm wide at the inner face and 85.28 mm long: the circumference at the middle of the base, minus 2 % so the sheet sits under slight tension.
- **Rib grooves:** eight grooves on the underside, with a half groove at each end so the seam lands on a rib. They have 0.1 mm clearance per side; the bevelled sheet ends leave 0.2 mm to each flange cone.
- **Slicing (measured):** *0.20mm NordicLab 0.6 Slow TPU*, AzureFilm TPU: **1 h 21 min, 8.2 g**.
- **Assembly:** wrap the sheet onto the core with the grooves over the ribs and the seam on a rib. The flange cones hold both edges.

<p align="center">
  <img src="docs/img/sleeve_fitted_tpu.png" width="380" alt="The sleeve as fitted on the core, with knurl">
  <img src="docs/img/sleeve_fitted_tpu_inside.png" width="380" alt="Fitted sleeve seen along the axis: eight rib grooves">
</p>

The fitted sleeve (`SleeveFitted`) is a reference model with an exact, zero-clearance fit on the core. Its knurl is built from one computed tile, repeated 630 times as linked copies, so the reference STEP files contain 631 solids. Use the flat sheet for printing.

## Model notes

- All sizes are in the `params` spreadsheet. The print-specific values start with `p_`; the sleeve values start with `sl_`, `kn_` and `fit_`.
- `Roller` is the original design, `RollerPrint` the print-optimised core. `SleeveFlat` is the printable sheet; `SleeveFitted` and the `SleeveTexture` group are the fitted reference.
- The knurl links are placed by a script, not by expressions. After changing `kn_n`, `kn_s`, `p_fl` or `L_roll`, regenerate them as described in [`AGENTS.md`](../AGENTS.md).
