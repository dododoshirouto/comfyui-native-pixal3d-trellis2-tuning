# Changelog

## [0.1.1] - 2026-09-07

- Updated the workflow to the 121-node revision with five additional render-preview checkpoints.
- Added full-workflow and tuning-control screenshots.
- Rewrote the feature list from a direct JSON comparison with the 66-node official template.
- Separated actual modifications from functionality inherited unchanged from the official workflow.

## [0.1.0] - 2026-09-07

- Initial public repository structure.
- Added a tuning-oriented variant of the official native Pixal3D/TRELLIS.2 workflow.
- Added separate CFG and step controls for Pixal3D and TRELLIS.2.
- Added stage-specific seed controls.
- Added manual/MoGe FOV switching.
- Added Shape Upscale bypass routing.
- Exposed remesh, smoothing, decimation, and face-count controls.
- Added render/map previews and standard GLB saving.
- Preserved the official 4096 texture default.
- Preserved Normal Map Cage Distance `0.02` through Math Expression to avoid Primitive Float rounding.
