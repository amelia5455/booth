# Booth

Find a photobooth near you, wherever the night goes.

A single-file prototype: analog photobooth map (Seattle-first, city by city),
a strips feed, saves, and a full passwordless onboarding flow. Everything —
markup, styles, data, and logic — lives in `index.html`. There is no build
step and no backend; open the file in a browser and it runs.

## Running it

Open `index.html` in Chrome or Safari. On a desktop window it renders inside
a true-size iPhone 16 Pro frame; on a phone it fills the screen.

## Demo mode

`const DEMO = true` near the top of the script gates every prototype fiction.
With it on, the flows can be walked deterministically:

| Trigger | Where | What it shows |
| --- | --- | --- |
| number ending `0000` | phone step | SMS delivery failure |
| code `000000` | code step | wrong code → 3-strike lockout |
| code `111111` | code step | instant code expiry (same state the 5:00 clock reaches) |
| name `ren` | name step | taken display name + suggestions |
| name `offline` | name step | availability check network failure |
| shift-tap "Use my location" | location step | the "Not here yet" uncovered-city state |
| the word `offline` in a caption or comment | compose / comments | failed send that preserves the user's work |

The triggers are deliberately invisible in the UI — this table is their
only documentation. Flip `DEMO` to `false` and the trigger codes, the
"offline" keyword, and the canned taken-name list disappear. The
behaviors they simulate then need a real backend.

Login is canned (any number + any code restores a sample account) regardless
of the flag — passwordless auth requires a server to be anything else.

## Design system notes

- **Chrome is ink, paper, and glass.** `--ink` and `--paper`/`--ground` are
  the palette. The chrome may use two hues, both in the same desaturated
  register: `--err` (rust, WCAG AA on both surfaces) for failures, and
  `--heart` (like-red, one step warmer) for likes, the same red for every
  profile. Country-flag emoji in the phone picker are the one full-color
  exception. Pigment otherwise belongs exclusively to the user's chosen
  booth color (`--me` / `--me-deep`), which defaults to ink until a choice
  is made.
- **Errors are inline and calm.** Wrong input shakes; expiry and denial do
  not (the user did nothing). Denial of permissions is neutral copy, never
  the error hue. Copy never confirms whether a phone number has an account.
- **Covered city** = nearest `CITIES` entry within 80 km that has booths in
  `BOOTHS`. Data-driven: a city goes live the moment booths land in it.
- Onboarding is namespaced under `#ob`; the app hands off display name and
  booth color at the end of the flow. Session-only by design.

## Roadmap to the App Store (known, deliberate gaps)

This file is the frontend seed and design source of truth, not the app.
Before submission it needs, roughly in order:

1. **Vendor dependencies** — Leaflet and the Gloock / IBM Plex Mono fonts
   currently load from CDNs. Bundle them locally (offline behavior,
   supply-chain hygiene, and the Google Fonts CDN privacy leak).
2. **Backend** — SMS auth, display-name registry, booth/strip storage,
   the city waitlist behind the "we'll text you" promise.
3. **Native shell** — Capacitor/WKWebView wrapper earning its keep with
   camera, real geolocation, and push (Apple guideline 4.2 rejects thin
   wrappers).
4. **Map tiles** — the free CARTO basemap is fine for a prototype; a
   production app needs a proper tile plan and a key kept out of this repo.
5. **Privacy labels** matching the in-app promises ("your location never
   leaves this phone" must stay true or change).

## License

All rights reserved — see LICENSE.txt. This is a product, not a library.
