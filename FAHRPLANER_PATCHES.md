# fahrplaner.de patches

Fork of maplibre/flutter-maplibre-gl. Branch `fahrplaner-patches-0.28` is
based on tag **v0.28.0**. Used by the Fahrplaner app via a git dependency
(package `maplibre_gl`). Not intended for upstream.

The older branch `fahrplaner-patches` (based on v0.26.2) stays unchanged for
older app builds.

## Status on v0.28.0 (12.11.2026): no code patches needed

Both patches of the v0.26.2 branch are covered by upstream 0.28.0. This
branch therefore carries **no code changes**, only this file. It is kept so
that a new patch can be added quickly when needed.

### Patch 1 – `attributionButtonEnabled` (24.10.2026) – dropped

Upstream added `MapLibreMap.attributionEnabled` (#1092): Android
`UiSettings.setAttributionEnabled`, iOS `attributionButton.isHidden`, the same
behaviour as our patch. The app now passes `attributionEnabled: false` and
shows its own attribution ("© OpenStreetMap", sheet with source, licence and
map style).

### Patch 2 – iOS offline download robust against single tile errors (28.10.2026) – dropped

Covered by upstream:

- `onPackDownloadError` only logs: MapLibre re-requests failed resources,
  the pack keeps downloading and is no longer deleted mid-download (the
  cause of the crash with ~21,800 tiles).
- The `FlutterResult` handed to the downloader comes from `pigeonResult`
  (PigeonResult.swift), which delivers only the first answer, so repeated
  replies are harmless.
- Progress events are throttled (100 ms); success and errors are guarded by
  `isCompleted`.

Difference to our patch: upstream never gives up on its own (our patch
suspended the pack after 50 errors in a row without progress). A download
that makes no progress (e.g. tile server down) stays pending; the progress
in the app stops moving and the user can delete the region. In the app,
offline maps come primarily as packages from our own server (T264); the
MapLibre region download is only the fallback.
