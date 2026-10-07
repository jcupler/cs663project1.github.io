---
layout: page
title: 5. Applications and Pitfalls
permalink: /applications/
---
---
#### Gait

Gait analysis uses temporal structure that a single pose cannot provide: stride and stance timing, foot contact, joint trajectories, trunk motion, and center-of-mass behavior.

A skeleton provides many of these signals, but shape-conditioned models can explain why similar joint trajectories produce different visual or dynamic outcomes. Evaluation must separate model quality from capture conditions such as camera viewpoint, clothing, footwear, fatigue, and subject morphology.

A robust system should report uncertainty and test across identities rather than treating one normalized gait as universal.

A basic pipeline can detect heel-strike and toe-off events, normalize each stride to a common phase, and compare trajectories or derived features. Normalization helps comparison but can also hide meaningful timing differences. Shape-conditioned generation suggests modeling expected identity variation before labeling remaining variation as an anomaly.

---
#### Spine

Kim et al. estimate a spinal centerline after reconstructing a 3D body from depth images captured from four directions. Their pipeline uses global and fine registration, adaptive vertex reduction, and a level-of-detail ensemble to balance mesh reliability with angle stability.

SIMSPINE takes a different route: it derives vertebral keypoints from musculoskeletal simulation and augments motion data with anatomically consistent labels. It provides a benchmark for 2D, monocular 3D, and multi-view estimation, while warning that simulation-derived labels are not in-vivo clinical measurements.

**Modeling lesson:** a centerline and vertebra-level kinematics are different outputs, so they require different labels and validation.

For a centerline represented by ordered points p₁ … pₙ, local bending can be approximated from adjacent segment vectors:

```text
uᵢ = pᵢ₊₁ − pᵢ
αᵢ = arccos((uᵢ · uᵢ₊₁) / (||uᵢ|| ||uᵢ₊₁||))
```

Because angle estimates amplify small point errors, smoothing, confidence weighting, and a clear coordinate convention are essential. A centerline angle should be reported as an estimate with a measurement protocol, not as a diagnosis.

---
#### Healtcare

Vision-based posture and motion systems can support rehabilitation, screening workflows, and longitudinal observation when validated against an appropriate reference. A spine centerline may be useful for posture feedback, while vertebral motion requires a more specific anatomical target.

The major limitation is interpretive: an estimated posture is not automatically a diagnosis. Clinical use requires subject diversity, calibrated error analysis, privacy safeguards, and a clear handoff to qualified professionals. 

---
#### Exercise

An exercise coach can use skeleton landmarks for fast alignment cues, then add surface or torso estimates when volume and trunk orientation affect the movement. Temporal analysis can identify repetition phases, contact events, and drift across a set.

A useful interface avoids pretending to see what the sensor cannot see. It can present a confidence indicator, ask the user to reposition, or frame its output as a coaching cue rather than a medical judgment.

An implementation can divide a repetition into phases, compute features such as joint-angle range and torso inclination, and compare the current repetition with the user’s own baseline. That personalized baseline is often more meaningful than comparison with a single “ideal” pose, especially when body shape or mobility differs across users.

---
#### Animation

Skeletons are excellent control rigs, but a character’s silhouette and dynamics depend on proportions. A shape-conditioned motion model can generate or retarget movement while preserving identity-specific relationships between body shape and action.

For production, controllability matters as much as realism. Artists need editable parameters, stable temporal behavior, and predictable failure modes. Physics and contact terms can improve plausibility, but they should not remove intentional stylization.

An animation pipeline can separate retargeting from surface deformation: first map motion into a character’s kinematic rig, then solve for a shape-consistent surface. This preserves artist control while allowing a richer body model to handle silhouette, proportions, and collision checks.

---
#### ISO 11226

ISO 11226 addresses the assessment of static working postures. A computer vision system intended to support that kind of assessment must preserve the relevant body angles, posture duration, and task context—not merely output a visually convincing skeleton.

The standard is a design reminder: application requirements determine model variables. Before deployment, confirm the current standard text, measurement protocol, and acceptable uncertainty. A research prototype should not be presented as compliance certification.

This also illustrates why timestamps matter. A posture that is acceptable for a brief transition may not have the same ergonomic interpretation when held statically. The system must retain frame timing and define how it aggregates posture estimates instead of reporting only the most extreme frame.

---
#### Generative Systems

Generative motion models can fill in missing observations, synthesize variations, or create animation. HUMOS demonstrates the value of conditioning motion on body shape instead of assuming one average body. Physics and stability terms help constrain the generated sequence, but learned systems remain dependent on data coverage and the definition of plausibility.

A responsible pipeline keeps capture, estimation, constraint, and action distinct. Evaluate generated results for temporal coherence, identity preservation, contact, anatomy, and out-of-distribution behavior. More realistic output is not automatically more truthful output.

For reproducibility, record the conditioning variables, random seed or sampling policy, model version, and constraint weights. Compare generated motion against both data-driven metrics and human or biomechanical review. A model can reduce pixel or joint error while producing repetitive, implausible, or identity-inconsistent motion, so evaluation must match the intended use.
