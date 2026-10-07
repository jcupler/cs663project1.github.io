---
layout: page
title: Models 
permalink: /model
---
An RGB image provides color and texture cues, while a depth image provides an estimate of distance for each pixel. Neither guarantees a complete body model: surfaces may be occluded, depth may be noisy, and internal anatomy is usually not directly visible.

A practical pipeline combines observation with a prior. The prior may be a motion library, a parametric body model, a multi-view geometric constraint, or a biomechanics model. The strongest systems make these priors explicit so failure can be traced to sensing, registration, or model mismatch.

The projection model also matters. A perspective camera maps 3D points to image coordinates using depth-dependent scaling, so a short limb can look long when it is closer to the camera. A depth sensor reduces this ambiguity but introduces missing pixels, quantization, reflective-surface errors, and a limited field of view. Preprocessing and confidence masks should be treated as part of the algorithm.

With calibrated cameras, corresponding observations can be triangulated or fused into a point cloud. Multiple views reduce single-view ambiguity and reveal surfaces hidden from one camera, although self-occlusion and calibration errors still matter.

Ye et al. demonstrate a single-depth strategy that first matches a noisy depth map to pre-captured motion exemplars. The match proposes a body configuration and semantic point-cloud labels; a refinement stage then fits the configuration directly to the observation.

```text
input depth → denoise → exemplar retrieval → pose + labels → geometric refinement
```

Single-view RGB methods can also predict depth or 3D pose from learned priors, but their output is more dependent on training distribution. Fewer sensors simplify capture, while stronger priors carry more responsibility for resolving ambiguity.

The retrieve-then-refine design is useful beyond this specific paper. Retrieval supplies a basin of attraction for nonlinear optimization, while geometric fitting adapts the candidate to the actual frame. Robust losses are important because depth maps contain outliers around silhouettes and occlusions. An unseen pose may still be forced toward the nearest available exemplar.


Sastry and Zhou discuss multi-camera reconstruction systems such as DynamicFusion and DoubleFusion as examples of using multiple cameras to build fuller body representations.

1. Calibrate camera intrinsics and extrinsics.
2. Synchronize frames and establish correspondences.
3. Triangulate or fuse depth into a common coordinate system.
4. Register the evolving body surface across time.

The result can be a richer body representation, but the setup is more expensive and less portable than a single sensor.

Calibration error propagates directly into the reconstruction. If camera extrinsics are slightly wrong, surfaces from different views fail to align and the fused body may become visibly thick or doubled. Robust registration, confidence weighting, and temporal filtering can reduce the effect, but they cannot recover information that all views miss.

Model information can appear as a loss, a constraint, an initialization, or a representation. A fitting objective might combine data error with temporal smoothness, joint limits, surface consistency, and physical stability:

```text
E = E_data + λ_t E_temporal + λ_a E_anatomy + λ_p E_physics
```

Adding a term is not automatically beneficial. A constraint can improve plausibility while suppressing unusual but real motion. The weights and failure cases should be evaluated on the intended population and environment.

During validation, inspect the data residual, temporal residual, anatomical violations, and stability measure separately rather than only the final score. This distinguishes a reconstruction supported by the image from one that is merely being pulled toward a plausible-looking prior.

