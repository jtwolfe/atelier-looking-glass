# Looking Glass — goals

## Job

Put a person in a garment they have not sewn or bought, from a still or a short clip, on a laptop or a phone, without sending the photo anywhere.

## Non-goals

- Live dressing-room mirror, WebXR, 60 fps cloth.
- Any cloth solver (Studio’s job).
- Webcam-on-a-mat, projection (Table).
- Inferring height from a photo of unknown distance. We ask.

## User promises

1. First result is a still. A 3 s clip is the encore, processed *after* recording.
2. The photo stays on the device unless they explicitly export.
3. A store can embed this as a script tag plus a GLB URL.
4. Bad bakes (`sewErrorMm.max > 3`) are refused, not hidden under a pretty composite.
5. Art modes are materials and a post stack, not a second product.

## Success tests

| Test | Pass |
|---|---|
| Still | Guided photo → dressed still in < 5 s on a recent laptop |
| Occlusion | Forearm in front of torso does not show a floating sleeve *through* the arm |
| Privacy | Devtools network tab: GLB fetch only, no image upload |
| Clip | 3 s 720p turnaround, processed in < 25 s, shareable WebM |
| Embed | A static HTML page with the component and a bake URL works without the Rust binary |
| Phone | 390 px wide capture guide usable (v1.1, designed for in v1) |

## Product boundaries

Looking Glass reads bakes and writes pictures/clips. It does not write IR. It does not call Studio at request time.
