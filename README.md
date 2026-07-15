# master-step-ffmpeg

LGPL builds of FFmpeg bundled by [MasterStep](https://github.com/adarkadosh/master-step),
plus their corresponding source — the "source available" home for MasterStep's
FFmpeg LGPL compliance.

MasterStep invokes FFmpeg as a standalone command-line binary (a separate process,
not linked) for audio decode, the `loudnorm` filter, stream-copy cuts, and PCM
re-encode. All LGPL; no GPL-only or nonfree components.

## Releases

Each release tag (e.g. `ffmpeg-6.1.3-lgpl`) provides:

- `ffmpeg-darwin-arm64` — macOS Apple-Silicon, built from source (see `build-macos.md`)
- `ffmpeg-win32-x64` — Windows x64, from [BtbN FFmpeg-Builds](https://github.com/BtbN/FFmpeg-Builds) `win64-lgpl` (static)
- `ffmpeg-<ver>-source.tar.xz` — the exact FFmpeg source used

License: GNU LGPL v2.1 — see `COPYING.LGPLv2.1`.
