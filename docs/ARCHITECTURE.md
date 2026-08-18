# Looking Glass — architecture

## Shape

```text
bake.v1 / GLB  ──►  web app (TS + MediaPipe WASM + WebGL)
                         │
                         ├─ getUserMedia  (this device)
                         ├─ pose + mask
                         ├─ pick size, skin GLB
                         └─ composite + art mode
                         │
atelier-looking-glass ───┴─ optional Axum host for local files / desktop
```

Two deploy shapes, one frontend:

1. **Binary** — `serve --open`. Loads local bake folders, no CDN. Designer and home use.
2. **Static embed** — `web/dist` on a shop origin. GLBs on a CDN. Rust is not in the request path.

Do not fork the UI.

## Why pose is not Rust

[MediaPipe Pose Landmarker](https://developers.google.com/edge/mediapipe/solutions/vision/pose_landmarker) already runs in the browser (WASM), 33 landmarks, world-ish 3D, with a segmenter. Rewriting that in Rust is a year. The exception is documented in [STACK.md](STACK.md).

Rust still earns its keep in the binary: serving local bakes, a tiny GLB inspector, future server-side batch (“dress these 40 photos overnight” for a lookbook — that is *not* v1).

## Camera ownership

Opposite of Table.

| Product | Camera belongs to |
|---|---|
| Table | The daemon machine (nokhwa) |
| Looking Glass | The browser (`getUserMedia`) |

If we ever open Looking Glass as a Tauri window, use the same MediaPipe path, not a Rust webcam, so the embed and the desktop app cannot drift.

## Data flow (still)

1. User types height. Optional bust/waist/hip, or a shop size.
2. Silhouette guide. One full-body photo, arms slightly off the torso.
3. Pose + mask.
4. Retarget SMPL-X skeleton to MediaPipe landmarks (a fixed map, plus height scale).
5. Skin the nearest `status: ok` GLB.
6. Split triangles by facing / by body-part mask.
7. Draw back → photo → front. Photo head wins.
8. Soft contact shadow. Art-mode post.

Clip: the same per sampled frame (12–15 fps), one body-shape fit across frames, temporal filter on joints, then `VideoEncoder`.

## Privacy

Default network: GET the GLB (and this JS). The image is a canvas. Export is a download. If a brand wants server-side beauty later, that is an opt-in and a different code path.
