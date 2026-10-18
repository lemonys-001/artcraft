# ArtCraft Suite — cPanel self-host

All 12 ArtCraft web apps, served as static files from `ac.lemony.dpdns.org` via cPanel.

## Layout

```
public_html/            (or wherever the domain's docroot is)
├── index.html          hub page listing all 12 apps
├── .htaccess           Apache: wasm MIME type, gzip, caching
├── _headers            (unused by Apache — kept for parity with Cloudflare)
├── cadcraft/
├── deckcraft/
├── designcraft/
├── effectcraft/
├── figcraft/
├── gridcraft/
├── lightcraft/
├── pdfcraft/
├── photocraft/
├── soundcraft/
├── vectorcraft/
└── wordcraft/
```

Each app folder is unmodified ArtCraft output (`index.html` + hashed `.js` + `.wasm` + font/license files). All asset URLs inside each `index.html` are relative, so any subpath works.

## Deploy (cPanel)

1. Extract the zip into the domain's document root (usually `public_html`).
2. Ensure Apache has `mod_mime` loaded (it does by default on cPanel). The root `.htaccess` sets `application/wasm` for `.wasm` files — without this, browsers refuse to stream-compile WebAssembly and the apps load slowly or not at all.
3. No PHP, no server-side code. Nothing to build.

## Large-file upload note

Some `.wasm` files exceed cPanel's default PHP upload cap (often 64M). If the file manager refuses to upload, either:
- raise `upload_max_filesize` / `post_max_size` in `php.ini`, or
- use FTP/SFTP (no PHP limit), or
- extract the zip on the server via SSH (`unzip artcraft-suite-12-apps.zip`).

## Updating an app

Download the latest `<app>-web-<version>.zip` from `https://github.com/storytold/<app>/releases`, replace the app folder with the archive contents (drop the versioned wrapper directory and `.htaccess` — the root `.htaccess` already covers all apps).

## Licenses

ArtCraft is Apache-2.0. Each app folder keeps its upstream `LICENSE-APACHE`, `LICENSE-MIT`, `README.md`, and attribution files. No source is modified.
