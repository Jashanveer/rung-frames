# Rung Frames Web Ordering

Static website for Phase 6 physical poster requests. Single self-contained
`index.html`, no build step, no dependencies.

Run locally:

```sh
python3 -m http.server 5173 --directory poster-order-site
```

Then open `http://localhost:5173`. Signed-out visitors see a labeled sample
year; sign in with a Rung account to load real data.

## Configuration (`CONFIG` at the top of the inline script)

| Key | Purpose |
|---|---|
| `backendUrl` | Rung backend origin (default `https://rung-backend-dev.fly.dev`) |
| `appleClientId` | Apple **Services ID** for Sign in with Apple on the web. Register the site URL as a Return URL for it. |
| `testflightUrl` | TestFlight public link (`https://testflight.apple.com/join/XXXXXXXX`). When set, a "Get the app" button appears in the toolbar. |

Backend env vars that pair with this site (Fly secrets on `rung-backend-dev`):

- `PRINT_CHECKOUT_BASE_URL=https://<site-domain>/checkout` — the backend appends
  the request params and returns it to the app as `checkoutUrl`.
- `APP_APPLE_BUNDLE_IDS=jashanveer.Rung,jashanveer.Rung-iOS,jashanveer.Rung.watchkitapp,<services-id>` —
  append the web Services ID so the token verifier accepts web sign-ins.
- `CORS_ALLOWED_ORIGINS=https://<site-domain>` — allow the site origin.

The site calls:

- `POST /api/auth/apple`
- `GET /api/progress?year=…` (and `&habitId=…` for month-scoped top habits)

## Printing

"Print at home" is free and always available. The print stylesheet renders the
poster as an exact 1180×800 page (`@page { size: 1180px 800px }`), forces the
light theme and `print-color-adjust: exact`, and strips all screen chrome, so
"Save as PDF" produces a pixel-identical poster from either screen theme.

Useful query params: `?theme=light|dark` forces a theme, `?period=2025` picks a year.
