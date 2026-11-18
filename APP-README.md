# GeoStamp Field Camera

One HTML file. Open it on your phone and every photo you take gets the date, local
time + timezone, GPS coordinates (decimal + degrees/minutes/seconds), accuracy,
elevation, street address, and a pinned OpenStreetMap inset **burned into the image
itself** — plus a saved log and a map of every pin.

## Getting it onto your phone

The camera and GPS only work on a **secure page** (`https://` or `localhost`).
Opening the file straight from Files/Downloads will show the gate screen but the
camera will stay blocked. Pick one:

**A. GitHub Pages (free, ~3 min, permanent URL)**
1. Create a repo, put this file in it as `index.html`.
2. Settings → Pages → Branch: `main` / root → Save.
3. Open `https://<you>.github.io/<repo>/` on the phone.

**B. Netlify Drop (fastest, no account needed to start)**
Go to `app.netlify.com/drop` on a computer, drag the folder in, open the URL it
gives you on the phone.

**C. Your own machine on the same Wi-Fi**
`python3 -m http.server 8080` then visit `http://<computer-ip>:8080` — note that
Chrome/Safari treat plain `http://` on a LAN IP as insecure, so A or B is better.

Then: **Share → Add to Home Screen.** It launches full-screen like an app.

## Using it

- **Enable camera & GPS** — grant both when prompted. The chips top-right show
  `GPS ±8m` / `CAM LIVE` (blue = good fix, amber = rough, red = blocked).
- **SHOOT** — stamps and shows the result. **Save photo** writes it to the on-device
  log *and* to your downloads. **Retake** discards it.
- **Flip** switches front/back camera. **Import** stamps a photo you already have
  (or shoots with the phone's own camera app, if the in-page camera is blocked) —
  it stamps with your *current* position and time, not the original photo's.
- **Log** tab — every photo with its coordinates, a Google Maps pin link, copy
  coordinates, re-download, delete, and **Export CSV** of the whole set.
- **Map** tab — all your pins on one map; tap a pin for the time, address, and a
  Google Maps link.

## Notes

- Photos and the log live **only on the phone**, in the browser's storage for this
  site. Nothing is uploaded anywhere. Clearing site data erases them, so keep the
  downloaded JPEGs for anything that matters.
- The address lookup uses OpenStreetMap's free Nominatim service and the map inset
  uses OpenStreetMap tiles — no API keys. Offline, the stamp simply drops the
  address and the map inset; time and coordinates still burn in.
- Accuracy is whatever the phone reports. Indoors it can be 50m+; `±` is stamped on
  the photo so the number is never implied to be better than it is.
