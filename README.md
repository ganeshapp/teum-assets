# Teum audio packs

Downloadable Korean and English audio for **틈 · Teum**.

Made by [gapp](https://www.gapp.in).

The Android app reads [catalog.json](catalog.json) and downloads versioned ZIPs from [Releases](https://github.com/ganeshapp/teum-assets/releases). No GitHub login or API token is required. Downloaded packs work offline.

## Available packs

The sentence library is expanding in packs of up to 25. Use **Teum 1.3 or newer** and tap **Your audio shelf → refresh** to see new packs as they finish. The APK does not need to change for each batch. This expansion covers sentence lessons only.

- **First little conversations:** 25 guided lessons in Voice A, about 6.5 MB to download. Whole sentence, meaning, key words, phrase practice, sentence build-up, and recall. Recordings are 64 kbps AAC-LC, mono, 44.1 kHz in M4A containers.
- **Speak any Korean:** optional Supertonic 3 int8 model, about 124 MB to download / 145 MB installed. Enables arbitrary words, pasted text, custom lessons, and Voice B entirely on the device. Prepared lesson packs work without it.

This repository intentionally contains public workplace Korean lesson content. Learning progress and text pasted into the app are never uploaded here.

## Adding packs

See [FORMAT.md](FORMAT.md). Publish each new ZIP under a new release tag or revisioned filename, then update the catalogue. Do not overwrite an existing revision: installed apps depend on the recorded SHA-256 and file sizes. Keep old release assets available for older app catalogues.

The app bundles a catalogue snapshot and a tiny first lesson. It only checks this repository when the user asks to refresh or download. Deleting audio from the phone preserves learning progress.

## Voice notices

Model: [Supertonic 3](https://huggingface.co/Supertone/supertonic-3), converted by [sherpa-onnx](https://k2-fsa.github.io/sherpa/onnx/tts/all/Korean/supertonic-3-ko.html). The voice ZIP includes the model's BigScience Open RAIL-M license and upstream MIT code notice. These notices are also available in the app's open-source licenses.

## Sentence library rollout

<!-- library-progress:start -->
**2,650 sentence lessons in 106 packs are available now.** The full batch covers 5,314 lessons in 213 packs. More packs are being generated and added as they finish.

Requires **Teum 1.3 or newer**. Open **Your audio shelf → refresh** in the app to see newly published packs. Download only the packs you want to keep offline.
<!-- library-progress:end -->
