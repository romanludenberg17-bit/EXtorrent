# ExTorrent for exteraGram

ExTorrent is an experimental exteraGram plugin project for a compact torrent interface.

## Current status — 0.1.3

The first UI prototype is committed in `extorrent.plugin`.

- Adds an **ExTorrent** entry to the exteraGram side drawer (`DRAWER_MENU`).
- Opens a Telegram-style panel showing the in-memory queue.
- Accepts magnet URIs and direct HTTP(S) URLs whose path ends in `.torrent`.
- Validates input and displays readable status messages.
- Removes its menu entry when the plugin unloads.

**Important:** this is only a UI prototype. It does not download data, resolve magnet metadata, persist the queue, or run a background service yet. Do not treat queued links as active downloads.

## Target experience

- A small transparent/monochrome torrent icon beside the search control.
- A modern download panel with progress, speeds, size, ETA, pause/resume and removal.
- Add magnet links, local `.torrent` files and direct torrent URLs.
- Persist queue and save location across restarts.
- Reliable background transfers with lifecycle-safe cleanup.

## Implementation plan

1. **Toolbar integration:** the side-drawer entry is now implemented through the documented `DRAWER_MENU` placement. The separate translucent icon beside search is still not implemented; locating that private view requires build-specific inspection.
2. **Torrent engine:** evaluate a compatible native BitTorrent engine. The Python plugin runtime does not itself provide libtorrent or native JNI libraries. A real engine needs compatible packaged native code or a supported engine bridge; the UI must never report fake progress.
3. **Download lifecycle:** implement start/pause/resume/cancel, storage permissions, queue persistence and cleanup.
4. **Testing:** install on the target app version, verify plugin enable/disable, toolbar placement, input validation, app restart and actual transfer metrics.

## Compatibility references

- exteraGram SDK overview: https://plugins.exteragram.app/docs
- Plugin class, lifecycle and menu items: https://plugins.exteragram.app/docs/plugin-class
- Xposed-style method hooks: https://plugins.exteragram.app/docs/xposed-hooking
- Alert dialog API: https://plugins.exteragram.app/docs/alert-dialog-builder

## Development notes

- Target app metadata currently requires exteraGram `>=12.10.6`.
- SDK metadata currently requires `>=1.4.5.0`.
- Reflection hooks are build-sensitive; never assume a private method exists without verifying it on the target build.
- No API keys or external services are required for the UI prototype.


### Drawer registration diagnostics (0.1.3)

The plugin now assigns a stable drawer item ID (`extorrent_drawer`), logs registration success/failure, and exposes the latest registration state in plugin settings. This helps distinguish a failed registration from a registered item that the app UI does not render. The status is diagnostic only; it does not prove that the item is visible. Device/UI behavior has not been verified.
