# AI Usage Report

## Tools used
- **Gemini** - for creating design
- **Claude (Anthropic)** — primary assistant for code generation, architecture decisions, and README drafting.

## What Claude helped with

- Scaffolding the Express project structure
- Writing the iRail integration logic (`departures.js`) based on API docs
- Designing the JSON response shape
- Writing the README and this AI usage report

## What I accepted as-is

- The overall file structure and module split (`index.js` for routing, `departures.js` for logic)
- The User-Agent header pattern (from iRail best practices)

## What I reviewed and adjusted

- The station filtering logic — verified it matches both `name` and `standardname` fields from iRail's response
- The rate limit cap (6 stations) — confirmed against iRail docs (3 req/sec, burst 5)
- The 15-minute window filter — confirmed it uses `scheduledTime` not `scheduledTime + delay`

## What I rejected

- An initial suggestion to use a queue library for rate limiting — overkill for this scale

## Prompts used

Main prompt: "Write a Node.js Express backend for the Lagovia Train Tracker challenge. It calls iRail's free API to find stations by substring and returns departures in the next 15 minutes. Input < 3 chars returns a 400 error. Cap parallel requests at 6 stations."

- **Gemini Chat link** - https://gemini.google.com/share/352b270b3430