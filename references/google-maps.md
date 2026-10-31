# Google Maps basemap, navigation, and free fallback

Prefer `assets/template-google.html` when its local browser key is configured.
Reuse a configuration the current user has explicitly provided. When the user chooses
free mode, or no Google key is configured, use `assets/template.html` with
Leaflet/OpenStreetMap and keyless Google Maps navigation links. In free mode,
do not load Maps JavaScript API or reuse Google map tiles through Leaflet.

## Google Maps mode

Use `assets/template-google.html` for a full Google basemap with day
switching, Advanced Markers, popups, and navigation links. Configure
`GOOGLE_MAPS_CONFIG.apiKey` and `mapId` from the user's Google Cloud project.
The public template contains placeholders and does not include a Google Cloud key.
Set up your own restricted browser key and map ID before enabling this mode.
Cards and navigation remain usable when map loading fails.

- Enable Maps JavaScript API in a project with an active billing account.
- Use a browser key restricted to the actual preview/deployment website
  origins and Maps JavaScript API. Keys in frontend HTML remain visible.
- Create a JavaScript map ID; `DEMO_MAP_ID` may be used for initial testing.
- Google Cloud may require two-step verification before project access.
  Let the user complete account authentication and payment details.
- Preview through localhost or HTTPS. Confirm successful live map loading;
  isolated syntax/logic checks do not prove key or billing configuration.
- Keep credentials out of traveler memory, screenshots, and final replies.
  Do not print a full key in routine tool output.

See [API setup](https://developers.google.com/maps/documentation/javascript/get-api-key)
and [Advanced Markers setup](https://developers.google.com/maps/documentation/javascript/advanced-markers/start).

## Basemap setup and usage

`assets/template.html` pins Leaflet 1.9.4 and exposes the basemap settings:

```js
const MAP_CONFIG = {
  tileUrl: 'https://tile.openstreetmap.org/{z}/{x}/{y}.png',
  attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
  maxZoom: 19
};
```

The default public tiles are for normal personal-trip viewing, subject to
the [OpenStreetMap tile policy](https://operations.osmfoundation.org/policies/tiles/):

- Keep OpenStreetMap attribution visible; never hide or fade it below readability.
- Use the exact HTTPS tile URL and normal browser caching. Do not force
  `no-cache`, bypass caches, or add cache-busting query strings.
- Preview through HTTP/HTTPS so tile requests carry a valid Referer.
  The template uses `strict-origin-when-cross-origin`; do not change it to
  `no-referrer` or `same-origin` or strip the Referer in a proxy.
- Load only the visible map. Do not add bulk downloads, background tile
  prefetching, or offline map features using this public tile service.
- Public tiles have limited capacity and no availability guarantee. For
  large audiences or stronger availability needs, choose a tile provider
  whose terms cover the use and update the URL and attribution together.
  Do not promise unlimited free hosting or tile usage.

Cards render independently of the map library. A failed library load keeps
the itinerary and Google navigation usable. Failed tile requests show a
small notice while markers, day switching, and navigation remain usable.
Browser geolocation requires HTTPS or a trustworthy localhost origin and
permission. No Google Geolocation API is called.

## Coordinates and navigation

- Use verified WGS84 coordinates: `lat` and `lng` are numbers in valid
  ranges. Zero is valid. Unknown values are `null`; their cards remain
  visible, but they are omitted from the embedded map.
- A verified optional `placeId` can disambiguate a branch in Google Maps.
  Use an ID only when already available; do not require Places API calls.
- Navigation prefers coordinates; without them, use `gmap` or the location
  name plus `address`. Never append a hard-coded destination city.
- `gmap` is a plain query or legacy URL-encoded query, not a full URL.
- Google Maps navigation uses `https://www.google.com/maps/dir/?api=1`
  with `destination` and optional `destination_place_id`. These links need
  no API key. Omit origin and mode so travelers can choose a route from
  their current location. Google Maps is the first action-sheet option;
  Apple Maps and 高德 remain alternatives when coordinates are available.
- Day polylines connect planned stops in order. They are not calculated
  street routes or travel times. Alternatives have markers but no line.

## Validation

Use realistic data to check:

1. Overview shows planned stops across all days and the hotel. Switching
   days removes the previous markers/line while retaining the hotel.
2. A day shows alternatives, excludes them from its line, and fits all
   visible stops. A single location centers at a sensible zoom. Zero
   coordinates work and unknown coordinates keep their cards.
3. Popups and cards navigate to the correct Google Maps destination;
   special characters and legacy encoded queries survive URL encoding.
4. Current-location success draws an accuracy circle and recenters the map;
   refusal leaves the map and cards usable.
5. Library load failure preserves cards/navigation. Tile failure preserves
   map markers with a short notice. Switching tabs during library loading
   shows the current day when the map is ready.
6. In free mode, no Google API script, API key, map ID, or cloud billing setup is required.
   Keep attribution visible. Test UI with intercepted/mock tiles instead
   of automated panning/zooming against public OSM tiles.

Official references:

- [Leaflet API](https://leafletjs.com/reference.html)
- [OpenStreetMap tile policy](https://operations.osmfoundation.org/policies/tiles/)
- [Google Maps URLs](https://developers.google.com/maps/documentation/urls/get-started)
