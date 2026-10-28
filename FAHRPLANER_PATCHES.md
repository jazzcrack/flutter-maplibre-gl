# fahrplaner.de patches

Fork of maplibre/flutter-maplibre-gl, based on tag v0.26.2. Used by the
Fahrplaner app via a git dependency (package `maplibre_gl`). Not intended
for upstream.

## Patch 1 – `attributionButtonEnabled` (24.10.2026)

`MapLibreMap(attributionButtonEnabled: false)` hides the native
attribution button. The app shows its own attribution ("© OpenStreetMap",
opens a sheet with source, licence and map style), so the native (i) button
was a duplicate.

- Dart: `MapLibreMap.attributionButtonEnabled` (default `true`), passed as
  option `attributionButtonEnabled`.
- Android: `MapLibreMapOptions.attributionEnabled()` at creation,
  `UiSettings.setAttributionEnabled()` on update.
- iOS: `MLNMapView.attributionButton.isHidden`.

## Patch 2 – iOS offline download robust against single tile errors (28.10.2026)

The Fahrplaner app crashed on iOS while downloading a large offline region
(~21,800 tiles). `OfflinePackDownloader` treated every
`MLNOfflinePackError` notification as fatal. These notifications are posted
for single failed resources (timeouts, HTTP 429/5xx), and MapLibre retries
those resources. On each one the downloader deleted the pack in the middle
of the download and answered the same `FlutterResult` again (after it had
already answered with the region at start, then again on success, error
and delete).

- `FlutterResult` is answered only once (`reply()`); later outcomes are
  reported through the event channel, as before.
- Single resource errors are logged and the download continues. Only after
  50 errors in a row without progress is the pack suspended, reported once
  as an error and removed.
- Error and success events are reported only once (`isCompleted` guard).
  No progress events are sent after completion.
