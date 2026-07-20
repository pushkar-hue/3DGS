# 3DGS

This repository contains my submission for the SuGaR mesh reconstruction assignment. The objective was to reconstruct a textured 3D mesh of a shoe starting from a multi-view image capture using COLMAP, Gaussian Splatting, and SuGaR.

## Directory Structure

```
.
├── final-mesh/              # Extracted SuGaR meshes
├── gaussian-render/
│   ├── iteration_7000.ply   # Initial Gaussian Splatting model
│   └── iteration_30000.ply  # Final Gaussian Splatting model
├── images/                  # Input image sequence
├── 15000.pt                 # Final SuGaR checkpoint
├── cameras.json             # Camera parameters
├── images.zip               # Original image archive
├── shoe.mp4                 # Capture video
└── README.md
```

## Pipeline

The reconstruction pipeline consisted of the following stages:

1. Captured approximately 146 images of the shoe from multiple viewpoints.
2. Estimated camera poses and sparse reconstruction using COLMAP.
3. Trained a baseline 3D Gaussian Splatting model.
4. Initialized SuGaR from the trained Gaussian representation.
5. Optimized the coarse SuGaR model for 15,000 iterations.
6. Extracted textured meshes using the trained checkpoint.

## Outputs

### Gaussian Render

The `gaussian-render` directory contains exported Gaussian Splatting point clouds from different training stages.

- `iteration_7000.ply` – Initial Gaussian representation used to initialize SuGaR.
- `iteration_30000.ply` – Final Gaussian representation after training.

### Final Meshes

The `final-mesh` directory contains meshes extracted from the trained SuGaR model at different extraction levels and decimation settings.

These meshes contain baked vertex colors and can be viewed using MeshLab or Blender.

## Observations

The pipeline successfully reconstructed the overall geometry of the shoe, including fine structures such as the laces and upper surface.

Since the capture was performed in an indoor environment without masking the object, the Gaussian representation also learned portions of the surrounding scene. Consequently, the extracted meshes contain background geometry in addition to the shoe. This is expected behavior because SuGaR reconstructs the complete Gaussian scene rather than performing foreground segmentation.

## Challenges

The primary challenges during implementation involved understanding the dependencies between COLMAP, Gaussian Splatting, and SuGaR, debugging runtime issues during coarse optimization, and resolving checkpoint compatibility issues during mesh extraction. Most of these were addressed through systematic debugging and rerunning the pipeline from a clean environment.

## Future Improvements

Given additional time, I would repeat the reconstruction using masked images to remove the background before training. This would allow the Gaussian model to focus entirely on the shoe and likely produce a cleaner extracted mesh. I would also evaluate the pipeline on rigid objects with simpler geometry to compare reconstruction quality across different object types and better understand the strengths and limitations of SuGaR under different capture conditions.