# Smart Video Compressor

Compress any common video to MP4 (H.264 + AAC) in the browser. Choose a target size (20/30/40/50 MB or custom) or a quality level, lower the resolution or frame rate, and optionally fast-forward long recordings (1.5×–32×).

The video never leaves the computer. There is no server: everything runs in the browser tab.

## Engines

| Engine | Used for | Speed |
|---|---|---|
| **Hardware (WebCodecs)** | MP4, MOV, MKV, WebM with H.264/HEVC/VP8/VP9/AV1 in Chrome, Edge or a recent Safari | Uses the graphics chip; usually several times faster than FFmpeg |
| **FFmpeg (WebAssembly)** | AVI, WMV, FLV, MPEG and other formats, remux, subtitles, sped-up audio at 1.5×/2× | Multi-threaded (up to 8 threads) on GitHub Pages, single-threaded elsewhere |

"Auto" picks the hardware engine when it can and falls back to FFmpeg automatically, including when hardware encoding fails part-way.

## Publish on GitHub Pages

1. Put the contents of this folder at the root of the repository (or in `/docs`):
   ```
   index.html
   coi-serviceworker.js
   ffmpeg/            (7 small .js files)
   ```
2. **Settings → Pages → Build and deployment → Deploy from a branch**, then choose the branch and folder.
3. Open `https://<user>.github.io/<repo>/`.

On the first visit the page reloads itself once. That is `coi-serviceworker.js` enabling cross-origin isolation, which GitHub Pages can't turn on through server headers, and which multi-threaded FFmpeg needs.

The page also works when `index.html` is opened directly from disk, with single-threaded FFmpeg.

## What gets downloaded

All engines are fetched from public CDNs at pinned versions, and only when needed:

- `mediabunny@1.61.6` from jsDelivr, about 0.3 MB, for the hardware engine.
- `@ffmpeg/core-mt@0.12.10` (multi-threaded) or `@ffmpeg/core@0.12.6` from unpkg, about 31 MB, only if the FFmpeg engine is used. The browser caches it afterwards.

## Third-party files in this folder

- `ffmpeg/`: the ESM build of [`@ffmpeg/ffmpeg`](https://github.com/ffmpegwasm/ffmpeg.wasm) 0.12.15, MIT licence. It is served from this site so that its worker is same-origin.
- `coi-serviceworker.js`: [coi-serviceworker](https://github.com/gzuidhof/coi-serviceworker) 0.1.7, MIT licence.
