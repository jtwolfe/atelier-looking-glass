# Atelier Looking Glass

On-device photo or short-clip try-on. “What would I look like in this?”

It **skins a Studio bake**. It does not simulate cloth. If you need a new drape, go back to [Studio](https://github.com/jtwolfe/atelier-studio).

This is **product 3** of the Atelier trio.

| Product | Repo | Role |
|---|---|---|
| Table | [atelier-table](https://github.com/jtwolfe/atelier-table) | Paper ↔ cloth |
| Studio | [atelier-studio](https://github.com/jtwolfe/atelier-studio) | Drape and bake |
| **Looking Glass** | this repo | Wear a bake on a photo / 3 s clip |

## Who it is for

- A shopper on a product page.
- You, checking a make, on a laptop camera.
- A brand embedding a snippet.

## What v1 does

1. Load a `bake.v1` folder (local, or a URL a shop already hosts).
2. Guided **photo** (full body, typed height). Optional **2–5 s clip**.
3. MediaPipe Pose + person mask, **in the browser**, on-device.
4. Pick nearest size. Skin the GLB to the detected pose. Composite: back pieces → photo → front pieces. Head stays the photo’s.
5. Art modes: catalog, clay/seams, editorial.
6. No upload by default.

## How you run it

```bash
# local / desktop
atelier-looking-glass serve --open

# or host the same binary on a small box and open it from the laptop
atelier-looking-glass serve --bind 0.0.0.0:8080
```

Store embed (later): the `web/` bundle as a Web Component, pointed at a CDN of GLBs. The Rust binary is optional there — pose is already WASM.

## Important split

The camera in Looking Glass **is** the user’s device. `getUserMedia` is correct here. That is the opposite of Table, where the daemon owns a webcam bolted to a cutting mat.

## Status

Documentation and contracts. No application code yet.

- [Goals](docs/GOALS.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Try-on](docs/TRYON.md)
- [Research](docs/RESEARCH.md)
- [Stack](docs/STACK.md)
- [garment.v1](spec/garment.v1.md) (context only — we consume bakes)

Apache-2.0.
