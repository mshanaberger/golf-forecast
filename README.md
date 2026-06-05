# Golf Forecast by ZIP Code

A single-file, client-side Golf Forecast web app that uses a U.S. ZIP Code to generate a golf-focused weather forecast, including current conditions, active alerts, sunrise/sunset, total daylight, daily golf scores, and daytime hourly golf conditions.

The app is designed to be GitHub Pages friendly. It does not require a backend, login, API keys, database, build system, or external JavaScript framework.

## Features

- ZIP Code search for any valid 5-digit U.S. ZIP Code
- Current weather conditions
- Current Golf Score
- Active NWS alerts
- Sunrise, sunset, and total daylight
- Daily Golf Score Forecast
- Calendar-style daily score display
- Detailed daily forecast table
- Daytime hourly forecast grouped by day
- Hourly UV Index
- Hourly wind, precipitation, temperature, conditions, and golf score
- Responsive layout for desktop, tablet, and mobile browsers

## Data Sources

This app uses public weather APIs.

### ZIP Code Lookup

ZIP Codes are converted to latitude and longitude using:

```text
https://api.zippopotam.us/us/{zip}
```

### National Weather Service

Forecast data comes from the National Weather Service API:

```text
https://api.weather.gov/points/{lat},{lon}
```

The app then uses the NWS-provided endpoints for:

- Daily forecast
- Hourly forecast
- Active alerts
- Observation stations
- Latest observations

### Open-Meteo

Sunrise, sunset, and total daylight are powered by Open-Meteo:

```text
https://api.open-meteo.com/v1/forecast?latitude={lat}&longitude={lon}&daily=sunrise,sunset&timezone=auto
```

Hourly UV Index comes from Open-Meteo Air Quality API:

```text
https://air-quality-api.open-meteo.com/v1/air-quality?latitude={lat}&longitude={lon}&hourly=uv_index&timezone=auto
```

If Open-Meteo fails, the app still loads the main NWS forecast. Missing sunrise/sunset or UV data will show `--` or `N/A` instead of breaking the page.

## Golf Score Logic

The Golf Score is a 0.0 to 10.0 score designed to estimate golf comfort and playability.

The score considers:

- Temperature
- Precipitation chance
- Thunderstorm/rain risk
- Wind speed
- Humidity
- Sky conditions
- Daylight availability

The ideal golf setup favors:

- Temperatures near 65–75°F
- Dry weather
- Light wind
- Lower humidity
- No thunderstorm threat
- Playable daylight hours

Hot, humid, windy, rainy, or stormy conditions reduce the score.

## Score Categories

| Score Range | Label |
|---:|---|
| 8.5–10.0 | Excellent |
| 7.0–8.4 | Good |
| 5.5–6.9 | Fine |
| 3.5–5.4 | Poor |
| 0.0–3.4 | Awful |

## Daylight-Based Forecast Windows

The app generally uses a 7 AM to 7 PM golf window.

However, when sunrise and sunset data is available, the app limits the golf forecast window to daylight:

- Start time is the later of 7 AM or sunrise
- End time is the earlier of 7 PM or sunset

If Open-Meteo sunrise/sunset data is unavailable, the app safely falls back to 7 AM to 7 PM.

## UV Index Handling

Open-Meteo hourly UV timestamps with `timezone=auto` are treated as local wall-clock time.

The app intentionally does not convert Open-Meteo hourly UV timestamp strings through `Date()` when building the UV lookup map, because doing so can shift the hour.

The UV map is built using:

```javascript
String(time).slice(0, 13)
```

NWS hourly rows are matched to Open-Meteo UV values using local `YYYY-MM-DDTHH` keys.

## Running Locally

Because this is a single-file app, you can open the file directly in a browser:

```text
index.html
```

For best results, especially when testing browser security behavior around API requests, run it through a simple local server.

Example using Python:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

## GitHub Pages Deployment

1. Create a GitHub repository.
2. Add the app file as:

```text
index.html
```

3. Commit and push the file.
4. Go to repository settings.
5. Open **Pages**.
6. Set the source branch, usually `main`.
7. Save the settings.
8. Open the GitHub Pages URL once deployment finishes.

No build step is required.

## File Structure

The app can run with only one file:

```text
index.html
```

Optional repository structure:

```text
/
├── index.html
└── README.md
```

## Browser Support

The app is intended for modern browsers, including:

- Chrome
- Edge
- Safari
- Firefox

It is responsive and designed to work on desktop, tablet, and mobile devices.

## Notes

- The app depends on public API availability from NWS, Zippopotam.us, and Open-Meteo.
- NWS forecasts are point-based. The entered ZIP Code is converted to latitude/longitude before requesting forecast data.
- Golf Score is an estimate and should not replace official weather alerts or personal judgment.
- Thunderstorm risk, lightning, extreme heat, strong wind, or poor air quality should always be taken seriously when planning outdoor activity.

## Credits

Weather data provided by:

- National Weather Service
- Open-Meteo
- Zippopotam.us
