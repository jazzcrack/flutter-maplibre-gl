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
