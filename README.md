# MapLibre v6: a custom layer that binds a uniform buffer breaks the layers above it

Minimal reproduction, MapLibre GL JS 6.9.0 only, no bundler.

A custom layer binds its own buffer to uniform binding point 2 and draws nothing else.
MapLibre keeps its per-frame uniform block on that binding point, fills it once per frame
and does not rebind it after the custom layer, so every layer drawn afterwards reads the
wrong values. Labels disappear.

| Before | Custom layer on | Workaround on |
| --- | --- | --- |
| ![](0-base.png) | ![](1-custom.png) | ![](2-workaround.png) |

The workaround checkbox makes the custom layer restore the previous binding after it
renders, using public WebGL calls only. That is what every custom layer has to do until
MapLibre rebinds its own buffers.

## Run locally

Any static file server works, for example:

```sh
python3 -m http.server 8000
```

Then open <http://localhost:8000/>. Opening `index.html` from disk does not work because
the page imports MapLibre as an ES module from unpkg.

## Deploy

The folder is a static site. With the Vercel CLI installed:

```sh
vercel
```
