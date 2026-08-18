# Try-on — still and short clip

## Why not live AR

Live AR needs a cloth LOD, an occlusion strategy that holds 60 fps, and Safari to cooperate. We already decided Studio is a bake. The honest consumer product is: **take a picture, wait a moment, see yourself.** A 3 s clip is the same idea with a share button.

## Still (v1)

**Capture guide.** Full body, 2–3 m back, even light, arms slightly away. On-screen silhouette. We do not try to recover height; we ask.

**Size.** Shop chart, or typed bust/waist/hip mapped to the bake’s `size` list. Nearest `status: ok` asset.

**Pose retarget.** MediaPipe 33 → SMPL-X joints. Good enough for a standing photo. We are not doing dance.

**Occlusion is the quality problem**, not physics.

- Body-part mask (torso / upper arm / forearm / head) if the segmenter gives it; else facing-split the garment.
- Layer: garment-back, person, garment-front.
- Always keep the photo’s head and hair.
- If the forearm mask says “in front,” those pixels of sleeve are not drawn.

A stiff bake in a pose far from A-pose/relaxed will look like a mannequin. That is acceptable. The capture guide exists to keep them near those poses.

## Short clip (v1.5)

2–5 s, 720p. Record, *then* process. Silhouette only while recording.

- Sample 12–15 fps in a Worker.
- One shape, one size, many poses.
- 1€ filter on joints.
- Same composite.
- Encode WebM/MP4; if `VideoEncoder` is missing, keep the hero still plus a frame strip.

Budget: < 25 s for 3 s of video on a laptop. Phone is v1.1, same code.

## Art modes

Same mesh, different stack.

| Mode | What |
|---|---|
| Catalog | IBL, fabric UV, default for shops |
| Clay | Grey + seam overlay (the designer’s check inside a consumer shell) |
| Editorial | Illustration / watercolor post — the shareable one |
| Flat | 2D technical drawing from IR, if the bake folder still has `garment.v1.json`; else hide |

## Failure modes we show, not swallow

- No full body in frame → recapture, don’t guess.
- Bake `failed` → “this size did not drape, try another.”
- Pose too far from bake pose → “stand like the outline.”
- Segmentation junk (couch reads as skirt) → show the mask toggle so the user can see we are confused.
