# Noritsu Repair Parts

3D-printable replacement parts for the **Noritsu QSF-V50** C-41 film processor. Each part is a parametric FreeCAD model with printing notes, STEP files and renders.

## Downloads

Each link downloads the file directly. STEP files open in any CAD program or slicer. The FCStd file is the parametric FreeCAD model.

| Part | Print files (STEP) | FreeCAD model | Extras |
|---|---|---|---|
| **Rack roller** | [PETG core](https://github.com/sasha-nordiclab/noritsu-repair-parts/raw/main/roller/step/Noritsu_V50_Roller_Core_PETG.step) · [TPU sleeve, flat sheet](https://github.com/sasha-nordiclab/noritsu-repair-parts/raw/main/roller/step/Noritsu_V50_Roller_Sleeve_TPU_Flat.step) · [TPU sleeve for the original roller](https://github.com/sasha-nordiclab/noritsu-repair-parts/raw/main/roller/step/Noritsu_V50_Original_Sleeve_TPU_Flat.step) | [FCStd](https://github.com/sasha-nordiclab/noritsu-repair-parts/raw/main/roller/cad/NoritsuV50_Roller.FCStd) | [Assembly](https://github.com/sasha-nordiclab/noritsu-repair-parts/raw/main/roller/step/Noritsu_V50_Roller_Assembly.step) · [Fitted sleeve](https://github.com/sasha-nordiclab/noritsu-repair-parts/raw/main/roller/step/Noritsu_V50_Roller_Sleeve_TPU_Fitted.step) · [Design reference](https://github.com/sasha-nordiclab/noritsu-repair-parts/raw/main/roller/step/Noritsu_V50_Roller_Design_Reference.step) · [Original roller with sleeve](https://github.com/sasha-nordiclab/noritsu-repair-parts/raw/main/roller/step/Noritsu_V50_Original_Assembly.step) |

Latest release: [Rack roller v1.1](https://github.com/sasha-nordiclab/noritsu-repair-parts/releases/tag/roller-v1.1) (ZIP with model, STEP files, renders and instructions). The whole repository as a zip: [main.zip](https://github.com/sasha-nordiclab/noritsu-repair-parts/archive/refs/heads/main.zip). Print settings and assembly notes for each part are in its README.

## Parts

| Part | What it does | Folder |
|---|---|---|
| **Rack roller** | Film transport roller for the QSF-V50 racks: a PETG core on a Ø6 mm shaft with a grid-textured TPU sleeve keyed onto eight anti-rotation ribs. | [`roller/`](roller/) |

<p align="center"><img src="roller/docs/img/assembly.png" width="420" alt="Noritsu V50 rack roller: PETG core with grid-textured TPU sleeve"></p>

## Common notes

- **Material:** PETG for rigid parts in the warm (38 °C) C-41 baths; TPU for the rubber sleeve. PLA softens and creeps at 38–40 °C.
- **Printer:** designed for a Bambu Lab A1 with 0.6 mm line width and 0.2 mm layers (0.4 mm nozzle for the PETG core; the TPU sleeve grid is made for a 0.6 mm nozzle). Walls are multiples of 0.6 mm and heights sit on the 0.2 mm layer grid.
- **Models:** FreeCAD 1.x (made with a 26.3 development build). The STEP files open in any CAD program or slicer.
- **Parameters:** every model is driven by its `params` spreadsheet. Change the values there and recompute.
- **Readable diffs:** `.FCStd` files are zip archives. After cloning, run this once so `git diff` shows the parameters:

      git config diff.fcstd.textconv "python3 tools/fcstd-textconv.py"

## License

[CC BY-NC-SA 4.0](LICENSE). You may share and adapt these designs with attribution, not for commercial use, and under the same license.
