| Repair (`repairAudioFile`) | re-encode to `pcm_s16le` | native PCM encoder (LGPL) |
| Duration probe | decode only | decoders (LGPL) |

So MasterStep needs **zero GPL-only and zero external (libmp3lame/libfdk_aac)
encoders**. A stock LGPL build covers 100% of what the app does. (Confirmed:
`src/main/ipc/ffmpeg.ts` — repair uses `-c:a pcm_s16le`, cut uses `-c copy`.)

**No runtime app code changes.** `getFfmpegPath()` + the capability probe already
run whatever binary lands in `resources/bin`. Work = produce LGPL binaries, host
them, repoint the fetch, license paperwork.

**macOS is arm64-only** (`electron-builder.yml`) → only a darwin-arm64 binary is
needed; ignore the darwin-x64 slot.

**Version note:** today we ship eugeneware `b6.1.1` (ffmpeg 6.0). This plan targets
ffmpeg **6.1.1** source for mac and a matching **6.1.x** BtbN build for Windows — a
small, deliberate version bump; keep both platforms on the same 6.1.x line.

---

## Owner split

- 🧑 **You (Mac + GitHub):** Parts A, B, C — build the mac binary, grab the Windows
  binary, create the public repo + release, upload assets. These need your machine,
  your GitHub account, and hardware I don't have.
- 🤖 **Me (in-repo PR):** Parts D & E — repoint the fetch/runtime + paste checksums,
  and build the in-app licenses view. I can do E (licenses UI) independently right
  now if you want; D needs the repo URL + hashes from Part C first.
- 🧑🤖 **Together:** Part F — I run the gate + `build:mac` and inspect the packaged
  binary; you do the on-device smoke test + Windows.

---

