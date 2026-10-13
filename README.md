# Noritsu QSF-V50 replacement parts

FreeCAD (PartDesign) models of 3D-printable replacement parts for the Noritsu QSF-V50
C-41 film processor. Designed for FDM on a Bambu Lab A1 (0.4 mm nozzle, 0.6 mm lines,
0.2 mm layers).

| File | Part |
| --- | --- |
| `NoritsuV50_Roller.fcstd` | Rack transport roller: design reference (`Roller`), print-optimised PETG core (`RollerPrint`), TPU sleeve as a flat printable sheet (`SleeveFlat`) and as fitted on the core (`SleeveFitted` + knurl texture as linked tiles in `SleeveTexture`) |
| `Heater.FCStd` | Heater part |
| `LockRing.FCStd` | Lock ring |
| `rack_leader_pusher.FCStd` | Rack leader pusher |

All sizes of the roller are driven by the `params` spreadsheet in its document.
Design notes and the current verified state are in `AGENTS.md`.

## Printing (roller)

- Core: PETG, bottom end face on the bed, no supports.
- Rubber sleeve: TPU, printed flat as a sheet and wrapped onto the core.
