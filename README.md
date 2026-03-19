# Micro-Manager BeanShell Automation Scripts

A collection of BeanShell scripts for automating microscopy experiments in [Micro-Manager](https://micro-manager.org/), covering stage control (X, Y, Z), camera configuration, image acquisition, and 2D spectral mapping. Written to support real experimental workflows, these scripts are designed as a practical starting point for anyone looking to automate their microscope with Micro-Manager's scripting panel.

Related publication: [Optics Express 28(19), 28656 (2020)](https://opg.optica.org/oe/fulltext.cfm?uri=oe-28-19-28656)

Useful API reference: [Micro-Manager API for BeanShell](https://nicost.github.io/I2K-MM/mm_api_b.html#/8/0/0)

> **Tip:** Navigating Micro-Manager documentation and version compatibility can be tricky. LLMs (Claude, GPT, etc.) are surprisingly useful for debugging BeanShell scripts, as they have good coverage of the MM API.

---

## Repository Structure

```
├── MM-1.4/          # Scripts for Micro-Manager 1.4 — stage motion & position lists
└── MM-2.0/
    ├── Basic/       # Camera setup, single acquisition, 2D motor scanning
    ├── Advanced/    # Multi-camera acquisition, spectral mapping workflows
    ├── SpectralMapping-1CAM.bsh      # Full 2D spectral mapping, single camera
    └── SpectralMapping_2CAM.bsh      # Full 2D spectral mapping, dual camera
```

---

## MM-1.4 — Stage Motion & Position Lists

Scripts originally developed for system calibration (see paper above). Each script builds a `PositionList` that can be used with Micro-Manager's MDA (Multi-Dimensional Acquisition).

| Script | Description |
|---|---|
| `oneDimensionalStageMotion.bsh` | Steps along a single axis (X, Y, or Z) by a fixed step size |
| `twoDimensionalStageMotion.bsh` | Steps in a plane (XY, XZ, or YZ) while holding one axis fixed |
| `ArbitraryMotion.bsh` | Increments all three axes simultaneously by independent step sizes |
| `3DHelicalStageMotion.bsh` | Generates a helical (corkscrew) trajectory in XYZ — used for 3D system calibration |
| `SetCamera.bsh` | Configures two DCAM cameras (exposure + ROI) simultaneously |

---

## MM-2.0 — Basic Examples

Foundational scripts for getting up and running with Micro-Manager 2.0's updated API.

| Script | Description |
|---|---|
| `CameraSpectraSetup.bsh` | Sets up a NIRvana HS camera with exposure and a narrow ROI (for spectrometer slit imaging) |
| `DualCamSetup_Andor_NIRvana.bsh` | Configures two cameras (NIRvana HS + Andor) with independent exposure and ROI settings |
| `simple1FrameAcq_datastore.bsh` | Snaps a single image and saves it to disk using the MM2.0 Datastore API (RAM → TIFF) |
| `2DStageScanning.bsh` | Generates a serpentine (bidirectional) XY position grid and steps through it with the stage |

---

## MM-2.0 — Advanced Examples

More complete acquisition workflows combining stage motion with image capture and saving.

| Script | Description |
|---|---|
| `2DScanning&Camera_implementation_1.bsh` | Steps through a position list and snaps an image at each position, saving to a multipage TIFF datastore |
| `2DScanning&Camera_implementation_2.bsh` | Same workflow using MM2.0's `SequenceSettings` builder and `runAcquisitionWithSettings` for tighter integration with the MDA engine |
| `Spectral Mapping.bsh` | Full 2D spectral mapping with configurable unidirectional or serpentine scanning, auto-incrementing save paths, and acquisition via the sequence engine |

---

## MM-2.0 — Spectral Mapping (Top-Level)

These two scripts represent the most complete workflows in the repository, intended for spectral mapping experiments where the XY stage scans a sample while a camera images the spectrometer output at each position.

### `SpectralMapping-1CAM.bsh`
Single-camera 2D spectral mapping. The stage performs a grid scan (unidirectional or serpentine), acquiring one spectrum per position. Positions are built into a `PositionList` and acquisition is handled by `runAcquisitionWithSettings`. The stage returns to center after acquisition.

### `SpectralMapping_2CAM.bsh`
Dual-camera variant that manually loops through positions and alternately acquires from two cameras (NIRvana HS + Andor) with independent ROI and exposure settings. Each camera's images are accumulated in a separate RAM `Datastore` and saved independently at the end.

**Key advantage:** By manually handling each camera sequentially rather than using MM's multi-camera mode, this script can handle cameras with **different ROI sizes** — something that Micro-Manager's built-in multi-camera support does not allow. The trade-off is that acquisition is slower than hardware-synchronized triggering.

---

## Notes on Micro-Manager Version Compatibility

The MM1.4 and MM2.0 APIs differ significantly. In particular:
- `PositionList` / `MultiStagePosition` imports changed between versions
- The `Datastore` and `Coordinates` APIs are MM2.0-only
- `SequenceSettings.Builder` is MM2.0 only; MM1.4 uses a different acquisition interface

If you are adapting these scripts, make sure your imports match your installed MM version.
