# GeoStamp Field Camera — project handoff

Paste this into a local Claude Code session (or keep it as CLAUDE.md in the
project folder) so it knows what this is without re-deriving it.

## What this is

A single-file web app, `index.html`. No build step, no dependencies to install,
no API keys, no backend. Open the file, that's the whole project.

It is a phone camera that burns provenance into each photo: date, local time with
timezone, GPS latitude/longitude in both decimal and degrees-minutes-seconds,
position accuracy in metres, elevation, reverse-geocoded street address, and a
map inset with a dropped pin and an accuracy ring. It also keeps an on-device log
of every shot and plots them all on a map.

## How it's built

- Camera: `getUserMedia`, rear-facing by default, with a `<input type=file
  capture>` fallback for when the in-page camera is blocked
- Position: `navigator.geolocation.watchPosition`, high accuracy
- Address: OpenStreetMap Nominatim reverse geocode, re-queried only after moving
  30m, failures swallowed (the stamp just omits the address)
- Map inset: OSM raster tiles composited onto a canvas at zoom 16, pin and
  accuracy ring drawn by hand; the whole thing is best-effort and the text stamp
  still lands if tiles fail
- Stamp: everything drawn to a `<canvas>`, exported as JPEG at quality 0.92
- Storage: IndexedDB (`geostamp` database, `photos` store), on-device only,
  nothing is ever uploaded
- Map tab: Leaflet 1.9.4 from cdnjs

## Design tokens

Defined in `:root` at the top of the `<style>` block. Deep navy structure,
signal-orange for actions, steel-blue for data:

    --bg:#081425  --panel:#0d1f36  --panel-2:#14304f  --line:#1d4269
    --orange:#ff7a1a  --orange-soft:#ffa259  --ink:#1a0d02
    --blue:#3d8ef0  --blue-deep:#1b5fb0

These were a stand-in for "Atlas IX colors" — the real brand hexes were never
supplied. If you have them, swapping the token values recolors the whole app,
but note the canvas stamp uses literal hex strings (`"#ff7a1a"`, `"#07101f"`,
`"#ff9a4d"`) that CSS tokens don't reach — those need changing too.

## Hard constraint: it needs HTTPS

Browsers refuse camera and geolocation on anything but `https://` or
`localhost`. Opening the file from disk shows the UI but the camera stays
blocked. For local development:

    cd <project folder>
    python3 -m http.server 8080
    # then http://localhost:8080 — localhost counts as secure

For the phone, it has to be on a real HTTPS host. GitHub Pages at
`https://jeepguy19.github.io/geostamp-camera/` was the intended home.

## State of things

Done and verified: renders correctly at 390px phone width with no horizontal
scroll; the script passes a syntax check; one real bug was found and fixed during
review (`display` rules were overriding the browser's `[hidden]` default, so
hidden panels rendered).

Never tested: the camera, GPS, geocoding, tile-fetching and IndexedDB paths. The
build container had no camera and no location, so every one of those is unproven
on real hardware. That is the first thing to exercise.

## Likely next steps

- Test on a real phone and fix whatever breaks
- Real brand colors
- Possible additions: compass heading / direction-faced in the stamp, a What3Words
  or plus-code line, batch export as a ZIP, an offline tile cache so the map inset
  works without signal
