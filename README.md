# ExTorrent for exteraGram

ExTorrent is an experimental exteraGram plugin project for adding a lightweight torrent-download interface to the Telegram client.

## Goals

- A subtle, transparent torrent icon beside the search control, matching the current theme.
- A compact downloads panel with progress, download/upload speed, pause/resume and remove controls.
- Add downloads from magnet links and `.torrent` files.
- Persist the queue and download location across app restarts.
- Clear diagnostics and safe cleanup when the plugin unloads.

## Technical plan

1. Verify the target exteraGram build and locate the actual search/action-bar view before hooking it. Do not guess private method signatures or silently patch unrelated views.
2. Build and test a small UI-only prototype first.
3. Evaluate whether a native BitTorrent engine can run reliably in the plugin runtime. If not, document the required native component or optional local/remote engine instead of shipping a fake downloader.
4. Add torrent metadata parsing, transfer lifecycle, storage permissions, and persistence only after the UI hook and engine are verified.

## Compatibility

The initial target is exteraGram with the Python plugin SDK. SDK documentation describes lifecycle hooks, menu items, and Xposed-style method hooks, but a toolbar button beside search needs build-specific verification.

- SDK docs: https://plugins.exteragram.app/docs
- Plugin class and hooks: https://plugins.exteragram.app/docs/plugin-class
- Xposed method hooking: https://plugins.exteragram.app/docs/xposed-hooking

## Status

Early development. No downloads are advertised as working until the torrent engine and UI integration are tested on-device.
