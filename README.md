# Apartment Hunter

A scheduled agent that searches for a short-term Manhattan sublet and emails a daily
report of anything new that fits.

## How it works
- **`CRITERIA.md`** — the actual search requirements (location, dates, budget, the
  gender-restriction filter, reference listings). Edit this file to change what counts
  as a match; no code changes needed.
- **`AGENT_INSTRUCTIONS.md`** — the step-by-step operational checklist the agent follows
  every morning: which sites to check, how to dedupe, how to write the email.
- **`seen_listings.json`** — every listing the agent has already reviewed (matched,
  borderline, or excluded), so nothing is reported twice. Updated and committed by every
  run.
- **`reports/`** — a dated copy of every email sent, for a record you can scroll back
  through without digging through your inbox.

## Schedule
A Claude Code Routine fires every morning, reads `AGENT_INSTRUCTIONS.md`, runs the
search, and emails the report to brendanbennett31@gmail.com. "No additional finds" is
the expected result on most days — the agent does not pad the report with near-misses.

## Known limitations
- Listings that only circulate in private Facebook groups or word-of-mouth aren't
  reachable by this agent.
- A listing search engine doesn't always index a brand-new post the same day it goes up,
  so "new today" really means "newly visible to search today."
- If a source site is unreachable on a given run, the email says so rather than silently
  reporting zero results for it.

## Changing things
- Tighten/loosen budget, dates, or neighborhoods: edit `CRITERIA.md`.
- Change delivery time or recipient: update the Routine (ask Claude, or use the
  Claude Code Remote trigger tools directly).
