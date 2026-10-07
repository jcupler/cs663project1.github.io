---
layout: page
title: 3. Bodies Estimation
permalink: /bodies/
---
A complete estimation system can be decomposed into three linked problems: infer subject-specific shape, recover temporally coherent motion, and estimate internal or anatomical structure where visible evidence is indirect.

Avatar tracking fuses low-dimensional kinematic models; HUMOS conditions generated motion on body shape; Kim et al. reconstruct a body from four depth directions to estimate a centerline; and SIMSPINE provides simulated vertebral labels for benchmarked learning.

These problems should share information without being collapsed into one opaque output.

One useful architecture is staged estimation: detect coarse landmarks, fit a body model, refine the surface or depth alignment, and then estimate specialized structures such as the spine. Joint optimization can exploit more interactions, but staged methods are often easier to diagnose and less sensitive to initialization.

Parametric body models separate pose θ from shape β, then generate a surface and joints through a mapping such as:

```text
M(β, θ) → {vertices V, joints J}
```

This makes it possible to fit body identity while preserving a coherent relationship between limb proportions, joints, and surface.

HUMOS argues that body shape should also condition motion generation. Different mass distributions and proportions can make the same requested action look different. Its conditional generative model combines identity information with cycle consistency and differentiable stability and physics terms to discourage implausible dynamics.

The important modeling choice is the separation between identity and pose, not the exact name of the body model. If β changes proportions while θ stays fixed, the system can test whether motion remains natural for that identity. If shape and pose are entangled, editing one can unintentionally change the other.
