# Looking Glass — research and prior art

## Consumer try-on, in the wild

| Thing | What it actually is | Lesson |
|---|---|---|
| Shopify / brand AR try-ons | Often 2D overlay or a dressed mannequin, not a fit oracle | Marketing ≠ tailoring. Don’t promise fit we didn’t bake. |
| Ready Player Me | Avatar platform | Fine for a stylised body, wrong as a measurement source. |
| Browzwear Stylezone | Browser *review* of a CLO-class bake | The commercial version of “Looking Glass consumes Studio.” Reviewers do not simulate. |
| Snap / Instagram try-on | Live lens, face/upper-body | Live is a different product. Defer. |
| 3DLOOK × Seamly | Body scan → measurement CSV emailed into Seamly | Measurements belong in Studio’s avatar path, not here. We may *type* those numbers to pick a size. |

## On-device body

**MediaPipe Pose Landmarker** (Google AI Edge) — 33 landmarks, image or video, Web JS + WASM. This is the v1 spine. Docs: [Pose landmark detection](https://developers.google.com/edge/mediapipe/solutions/vision/pose_landmarker).

**Selfie / interactive segmenter** — person mask. Enough for v1 layering. Body-part segmentation if a later MediaPipe task stays small enough for phones.

**SMPL-X** — Studio binds to it; we retarget to it. Do not invent a third skeleton.

We will not run a full SMPL-X shape optimisation on-device in v1. Height + chart size + pose is the v1 body. A distilled beta regressor is a later upgrade.

## Rendering

Three.js (or a thinner WebGL/WebGPU layer) for glTF, skinning, IBL. Art modes are materials + a fullscreen pass. Do not pull in a game engine.

If WebGPU skinning becomes worth it, it is an implementation swap behind the same composite.

## Privacy / AU APP / GDPR

Default: no image upload. Document it in the embed snippet so a shop’s lawyer can read one paragraph. If a shop wants a lookbook batch, that is the Rust binary on *their* machine, not our cloud.

## What we refuse to become

A live cloth demo. A face-only beauty filter. A place that reimplements Studio “just for this pose.”
