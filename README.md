# 🚆 Lagovia Train Tracker — Backend API

A Node.js/Express API that returns real-time Belgian train departures, powered by the free [iRail API](https://docs.irail.be/).

---

## How to install and run locally

### Prerequisites
- Node.js >= 18 ([download](https://nodejs.org))
- Git

### Steps

```bash
# 1. Clone the repo
git clone https://github.com/YOUR_USERNAME/lagovia-api.git
cd lagovia-api

# 2. Install dependencies
npm install

# 3. Start the server
npm start
# Running on http://localhost:3000

# For development with auto-restart:
npm run dev
```

### Try it

```bash
curl "http://localhost:3000/departures?q=Bru"
curl "http://localhost:3000/health"
curl "http://localhost:3000/departures?q=Br"  # error: too short
```

---

## API Reference

### GET /departures?q=<query>

Returns departures (next 15 min) from all stations matching the substring.

**Success 200:**
```json
{
  "query": "Bru",
  "generatedAt": "2024-06-01T10:30:00.000Z",
  "windowMinutes": 15,
  "stationsFound": 8,
  "stationsQueried": 6,
  "stations": [
    {
      "id": "BE.NMBS.008891009",
      "name": "Brussel-Centraal",
      "departures": [
        {
          "train": "IC 1234",
          "destination": "Gent-Sint-Pieters",
          "scheduledTime": "10:35",
          "scheduledTimestamp": 1717234500,
          "delayMinutes": 2,
          "canceled": false,
          "platform": "3"
        }
      ]
    }
  ]
}
```

**Error 400 — query too short:**
```json
{
  "error": "QUERY_TOO_SHORT",
  "message": "Query must be at least 3 characters.",
  "minLength": 3,
  "received": 2
}
```

### GET /health

```json
{ "status": "ok", "timestamp": "2024-06-01T10:30:00.000Z" }
```

---

## Decisions & trade-offs

- **Station search**: Fetches the full iRail station list and filters client-side. iRail caches this via ETags so it's cheap.
- **Rate limits**: Capped at 6 parallel liveboard requests to stay within iRail's 3 req/sec + 5 burst limit.
- **15-minute window**: Filtered on `scheduledTime` (not scheduledTime + delay), matching the spec exactly.
- **No caching**: Fresh per request. A Redis 30s cache per station would help in production.

## Known limitations

- Results capped at 6 stations per search
- No fuzzy search (bonus feature not implemented)

## Time spent

~2 hours: 30min docs, 60min coding, 30min README.

## AI Usage

See [AI_USAGE.md](./AI_USAGE.md).
