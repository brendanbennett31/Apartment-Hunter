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
  reachable by this agent. The "NYC Sublets & Apartments" Facebook group is reportedly
  high-signal for this kind of search — worth checking manually, not automatable here.
- A listing search engine doesn't always index a brand-new post the same day it goes up,
  so "new today" really means "newly visible to search today."
- If a source site is unreachable on a given run, the email says so rather than silently
  reporting zero results for it.
- **WebFetch is blocked in this environment** by network policy (confirmed with raw
  `curl` too, so it's not a tool-specific issue) — the agent can't open listing pages
  directly, only search summaries. Fixing the environment's network access level (see
  code.claude.com/docs/en/claude-code-on-the-web) would let it verify listings directly
  instead of triangulating through search.
- **Craigslist specifically resists automation.** It dropped native RSS support, and
  third-party RSS-bridge tools (tried: Open RSS) get blocked with a 503 rather than
  producing a feed. Two real alternatives if you want live Craigslist coverage:
  - **Claude for Chrome** (a separate Anthropic browser extension, research preview) —
    runs in your own browser on your own machine/network, so it isn't subject to this
    environment's block. Good for you checking Craigslist interactively yourself; it's
    a different Claude session from this one and can't be wired into the automated
    daily email.
  - Manually spot a listing yourself and paste the link into this chat — a pasted link
    with real text doesn't need any fetching, so it can be vetted properly on the spot.
- **StreetEasy has native saved-search email alerts** (no scraping needed) — worth
  setting up directly on streeteasy.com (search, hit Save, enable email notifications).
  If set up, the agent checks Gmail for those alerts too, which carry real first-party
  links.
- **Leaseswap.nyc** is a paid third-party alert service that already monitors
  StreetEasy, LeaseBreak, NYBits, Reddit, and building sites together — worth evaluating
  directly if you want broader coverage than this agent alone provides.

## Changing things
- Tighten/loosen budget, dates, or neighborhoods: edit `CRITERIA.md`.
- Change delivery time or recipient: update the Routine (ask Claude, or use the
  Claude Code Remote trigger tools directly).
