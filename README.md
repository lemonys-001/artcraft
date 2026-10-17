# ArtCraft (self-hosted)

Static hosting for the [ArtCraft](https://getartcraft.com) open-source creative
suite, served at **https://ac.lemonyss.dpdns.org**.

Each app is a self-contained WebAssembly build that runs entirely in the browser.
Nothing a user opens here is uploaded to a server. LemonOS embeds these pages in
sandboxed iframes.

## Layout

```
/                 hub landing page, links to each app
/_headers         Cloudflare Pages header rules (wasm MIME type, caching)
/photocraft/      PhotoCraft — photo / raster editor
/gridcraft/       GridCraft  — spreadsheets
/deckcraft/       DeckCraft  — presentations
/cadcraft/        CADCraft   — CAD drawings
/figcraft/        FigCraft   — interface / diagram design
/soundcraft/      SoundCraft — audio editing
```

Each app folder is unmodified ArtCraft output (`index.html` + hashed `.js` +
`.wasm` + font/license assets). All asset URLs are relative, so any subpath works.

## Deployment

This repo is static — no build step. Point a Cloudflare Pages project at it
(connect this repo, build command empty, output directory `/`).

### 25 MiB per-file limit

Cloudflare Pages rejects any single file over **25 MiB**. Six ArtCraft web builds
embed a WebAssembly bundle larger than that, so they are **not** included here:

| App         | wasm size |
|-------------|-----------|
| LightCraft  | 26.0 MiB  |
| PdfCraft    | 36.3 MiB  |
| DesignCraft | 37.7 MiB  |
| WordCraft   | 37.9 MiB  |
| EffectCraft | 59.6 MiB  |
| VectorCraft | 61.7 MiB  |

Adding any of these would make the whole Pages deployment fail. To host them,
use an origin without the per-file cap (e.g. an R2 bucket or a VPS) and reverse
proxy it under the same domain.

## Updating an app

Download the latest `<app>-web-<version>.zip` from
`https://github.com/storytold/<app>/releases`, then replace the app folder with
the archive contents (drop the versioned wrapper directory, `.htaccess`, and the
per-app `_headers`).

## Licenses

ArtCraft is Apache-2.0. Each app folder keeps its upstream `LICENSE-APACHE`,
`LICENSE-MIT`, `README.md`, and attribution files. No source is modified.
