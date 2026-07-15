# FFmpeg LGPL compliance — MasterStep

This repo hosts the LGPL FFmpeg binaries MasterStep bundles, **and their
corresponding source**, so distributing FFmpeg inside the MasterStep installer
meets the LGPL v2.1 obligations. This file records what we ship and why it's
compliant, for the paid-launch review.

## What we distribute

| Asset | Platform | Origin | License |
| --- | --- | --- | --- |
| `ffmpeg-darwin-arm64` | macOS arm64 | Built from FFmpeg 6.1.3 source, `--disable-gpl` static (see `build-macos.md`) | LGPL v2.1 |
| `ffmpeg-win32-x64` | Windows x64 | BtbN FFmpeg-Builds `autobuild-2025-08-31-13-00`, `win64-lgpl` static | LGPL v2.1 |
| `ffmpeg-6.1.3-source.tar.xz` | — | Upstream FFmpeg 6.1.3 source (the corresponding source) | — |

FFmpeg **6.1.3**, GNU **LGPL v2.1**. No GPL (`--enable-gpl`) or nonfree
(`--enable-nonfree`) components — no libx264/libx265/libfdk. Verified via
`ffmpeg -buildconf` (shows `--disable-gpl`).

## Why LGPL is sufficient for our use

MasterStep uses FFmpeg only for: audio **decode** (mp3/wav/flac/mpeg, Opus, M4A),
the **`loudnorm`/`ebur128`** filters, **stream-copy** cuts (`-c copy`), and
**PCM** re-encode (`pcm_s16le`). None of these require any GPL-only or external
encoder, so a stock `--disable-gpl` build is functionally identical to the GPL one.

## How we meet the LGPL obligations

FFmpeg's own [legal checklist](https://ffmpeg.org/legal.html) for distributing
binaries asks to distribute the source, host it alongside the binary, and give
attribution. We do all three:

1. **Source available, next to the binary.** The FFmpeg source tarball is a
   release asset in the same release as the binary — exactly *"Host the FFmpeg
   source code on the same webserver as the binary you are distributing."*
2. **License text included.** `COPYING.LGPLv2.1` is in this repo.
3. **In-app attribution.** MasterStep's Settings → About names FFmpeg + version,
   states it's used under LGPL v2.1, and links here.

### On static linking + how we invoke FFmpeg

The FFmpeg checklist's *"use dynamic linking"* guidance governs linking the
libav\* **libraries into a proprietary program**. MasterStep does **not** do that:
it ships a **standalone, wholly-LGPL `ffmpeg` executable** and invokes it as a
**separate process** (command line / subprocess), not by linking libav\* into app
code. When the entire binary is LGPL and its source is provided (it is), building
it statically is fine — the dynamic-linking rule exists so users can replace the
LGPL part when it's combined with closed code, and there is no closed code inside
this binary.

Per the [FSF GPL FAQ](https://www.gnu.org/licenses/gpl-faq.html#MereAggregation),
communication via *"pipes, sockets and command-line arguments"* indicates
**separate programs**, so the app and the FFmpeg CLI are not a combined work.
(We still chose LGPL over relying solely on this, since separateness is
ultimately a legal judgment.)

## Background

MasterStep previously bundled the GPL
[`ffmpeg-static`](https://www.npmjs.com/package/ffmpeg-static) build
(`GPL-3.0-or-later`). We switched to this LGPL build to remove any
"open-source-your-app" ambiguity for a closed/paid product (MasterStep tracking:
MAS-316 / backlog 7.6).

## Sources

- FFmpeg — Legal / License: https://ffmpeg.org/legal.html
- FSF GPL FAQ — Mere Aggregation: https://www.gnu.org/licenses/gpl-faq.html#MereAggregation
- ffmpeg-static (the prior GPL build): https://www.npmjs.com/package/ffmpeg-static
- BtbN FFmpeg-Builds (Windows binary): https://github.com/BtbN/FFmpeg-Builds

_Not legal advice — engineering compliance notes. Confirm with counsel before
paid launch._
