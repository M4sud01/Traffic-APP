# TRAFFIQ — Smart Traffic Congestion & Route Recommendation Console

A single-file web app (`index.html`) — no build step, no server required.

## Site Live ::
traffic-app-nine.vercel.app

## Quick start
1. Double-click `index.html` (or open it in any browser).
2. On the connect screen, either:
   - Paste a Google Maps API key and click **Connect Google Maps** for real live traffic + real routing, or
   - Click **"Skip for now — use the free OpenStreetMap version instead"** to run immediately with zero setup (uses OpenStreetMap tiles + free OSRM routing).

## Getting a Google Maps API key (2026 pricing model)
1. Open https://console.cloud.google.com/google/maps-apis and create/select a project.
2. Under **APIs & Services → Library**, enable:
   - Maps JavaScript API
   - Directions API
   - Places API
3. Go to **Billing** and link a billing account (required by Google even for free usage).
   Note: Google retired the pooled $200/month credit in March 2025. Each API now has
   its own free monthly allowance (roughly 10,000 map loads/month for Maps JavaScript API),
   which normally covers a demo comfortably.
4. Go to **APIs & Services → Credentials → Create Credentials → API key**, copy it.
5. Recommended: restrict the key to your domain (Application restrictions → HTTP referrers)
   before deploying it anywhere public.

## Features
- Real-time traffic visualization (Google TrafficLayer when connected)
- AI-based congestion prediction — a transparent heuristic model blending time-of-day/day-of-week
  patterns, per-corridor bias, and (when connected to Google) the real duration_in_traffic signal,
  projected out to +30/+60/+90 minutes
- Best route recommendation — ranks alternative routes by live travel time + predicted congestion
- Accident alerts — click "+ Report" then click the map to add one; click any marker to resolve it;
  incidents also appear on their own periodically to simulate a live feed
- Heatmap dashboard — congestion heatmap layer, toggle on/off, auto-refreshes
- Google Maps integration — Places autocomplete, real Directions, real traffic layer

## How routing works
- **Free mode:** OpenStreetMap tiles (via CartoDB dark basemap) + OSRM public routing server +
  Nominatim geocoding — all free, no key, best for demos and light use.
- **Google mode:** Google Maps JavaScript API, Directions API (with live traffic), Places API
  autocomplete — requires your own API key as described above.

## Notes for further development
- This is a frontend-only demo — no backend or database. Incidents and route history reset on page reload.
- The prediction model is a heuristic, not a trained ML model. A natural next step is training a
  real model (e.g. regression or LSTM) on a historical traffic dataset and swapping it in behind
  the same `predictCongestion()` function signature.
- Live incident feeds (Waze, city traffic authorities) typically require paid data partnerships —
  worth noting as a production roadmap item.
- The Google API key is entered client-side for portability. For a production deployment, proxy
  Google Maps calls through a backend to keep the key off the client entirely.
