# Pack format, version 2

`catalog.json` is UTF-8 JSON with `schema: 2`, `voiceRevision: "supertonic3-v1"`, and a `packs` array. It is served directly from this repository's `main` branch. The app ships an offline copy and saves a validated refresh locally.

The expanded catalogue requires Teum 1.3 or newer. Version 1.3 also accepts schema 1. Older apps keep their existing downloads and bundled catalogue; update the APK to browse the growing library.

Only complete, uploaded, checksum-verified packs are added to the catalogue. `library-status.json` records rollout progress; it is informational and is not needed for app playback.

Each pack declares:

- `id`: stable lowercase letters, digits, and hyphens. Progress is keyed by Korean sentence text, not pack position.
- `revision`: positive integer, increased when a pack changes.
- `kind`: `lessons` or `voice`.
- `title`, `description`: the audio shelf's display text.
- `url`: immutable HTTPS release-asset URL under `github.com/ganeshapp/teum-assets/releases/download/`.
- `sha256`, `downloadBytes`: checksum and exact size of the ZIP.
- `installedBytes`: sum of uncompressed file sizes.
- `files`: exact archive paths mapped to byte counts. No parent directory is wrapped around these entries. No extra files or directory entries.
- `lessons`: up to 25 lesson objects for lesson packs; empty for a voice model. Each has `ko`, `en`, `title`, `topic`, `chunks`, `words`, and optional `source`/`register` and `curated`. The original 25 authored lessons have `curated: true` (the default); expanded lessons use `curated: false` so the Reader’s Start here collection stays focused. Chunks and words have `ko`, `en`, and optional `note`. Expanded lesson chunks may have empty `en`; the supplied whole-sentence translation and matching dictionary glosses are preserved without inventing fragment translations. Lessons remain readable without audio downloaded.

## Recordings

Lesson ZIP entries are `audio/<clip-key>.m4a`. A clip key is the lowercase hexadecimal SHA-256 of UTF-8:

```text
supertonic3-v1|0|0.90|ko|<trimmed Korean text>
supertonic3-v1|0|1.00|en|<trimmed English text>
```

Voice A is speaker 0. Audio is AAC-LC, mono 44.1 kHz, 64 kbps, with the original speech lead-in/tail preserved. Every unique prompt needed by the pack's one-lesson and multi-lesson sessions must be included, including delayed-recall prompts. Both quiet and coached lesson sequences use the same recordings. Growing Korean phrases may combine recorded chunks with short joins; whole sentences have dedicated natural recordings. Each pack is self-contained; a small amount of shared coaching audio is duplicated across packs.

The app verifies ZIP size and SHA-256 before extracting. It checks exact entry names and sizes, then renames a completed staging folder into place. Failed or cancelled downloads do not replace the installed revision. It decodes recordings locally into temporary PCM for the continuous speech-and-silence track. No audio is streamed during lessons.

## Model

The optional `voice-supertonic3` pack has the seven Supertonic model/config files at the ZIP root, plus `LICENSE` and `MODEL_LICENSE`. Runtime files use the same filenames as the upstream converted model. A different model generation requires a compatible app release and catalogue schema/revision handling.

## Building

In the Teum app source checkout, generate WAV source clips with `scripts/precache_audio.dart`, then run:

```sh
mkdir -p artifacts/asset-packs
dart run scripts/export_pack_plan.dart > artifacts/asset-packs/plan.json
python3 scripts/build_asset_packs.py
```

The Python script uses FFmpeg, writes deterministic ZIPs and checksums into catalogue metadata, copies the offline catalogue into the app, and creates the small bundled starter recording set. Upload the ZIPs first, then publish the corresponding catalogue. Keep the app and published catalogue snapshots in sync for a release.

## Full sentence library

The app source also contains `scripts/plan_sentence_library.dart`, `scripts/verify_sentence_plan.dart`, `scripts/generate_sentence_clips.dart`, and `scripts/package_sentence_library.py`. They preserve the existing first pack and optional model, prepare the remaining source sentences, validate text and clip coverage, and publish new immutable packs incrementally under `sentence-library-v1`.

The packager's `--publish --watch` mode uploads and verifies each ready ZIP before updating the public catalogue, README count, and `library-status.json`. It never republishes an existing asset with different bytes. The app release is independent of this generation batch; tap refresh in Teum to pick up new packs.
