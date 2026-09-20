# Daily Run Instructions

This file is what the scheduled Routine actually follows each morning. It starts from a
fresh session with no memory of prior runs, so do every step below in full each time.

## Steps

1. **Sync.** `cd` into the Apartment-Hunter repo, `git pull origin claude/manhattan-rental-agent-9bxmr2`
   so you have the latest `seen_listings.json` (previous runs update it).

2. **Read `CRITERIA.md`** in full. That file, not this one, is the source of truth for
   what counts as a match, the budget, the dates, and the gender-restriction filter.

3. **Read `seen_listings.json`.** It's a JSON array of every listing already reviewed by
   a previous run (matched, borderline, or excluded), each with a `url`. Never re-report
   a `url` already in this file.

4. **Search these sources** for Manhattan short-term sublets/rooms, using web search and
   by fetching each site's current listings pages:
   - ListingsProject.com (Manhattan / NYC section — rooms & sublets)
   - Craigslist New York (newyork.craigslist.org) — "sublets / temporary" and
     "rooms / shared" sections, Manhattan only
   - Leasebreak.com
   - StreetEasy.com — filtered to furnished / short-term rentals, Manhattan
   - Blueground.com, Kopa.com, Anyplace.com — furnished short-term, Manhattan only
   - Any other legitimate NYC short-term sublet source you come across

   Note which sources you actually managed to check. If a site blocks fetching or search
   turns up nothing usable for it, say so in the email rather than pretending it was
   checked and came back empty.

5. **For each candidate listing not already in `seen_listings.json`:**
   - Confirm it's actually in Manhattan — check the stated address/neighborhood, don't
     assume from the title.
   - Confirm it's a short-term sublet, not a lease takeover or a listing demanding a
     long-term commitment.
   - Confirm the availability window overlaps Oct 2026–Jan 2027 (ideally Oct 4–30, 2026,
     but any short window in that range is worth surfacing).
   - Check price against the budget rules in `CRITERIA.md`. Flag as borderline rather
     than excluding if it's close but not a clean fit.
   - Check the listing text for gender-restrictive language. If present, exclude from
     the main matches and log it as `excluded_gender` — mention it in a short separate
     line in the email, don't just drop it silently.
   - Skip and don't re-analyze anything already in `seen_listings.json`.

6. **Compose the email.**
   - Subject: `Manhattan Sublet Report — <today's date>`
   - **No new qualifying matches:** body is literally "No additional finds." plus one
     short line listing which sources were checked (and which failed, if any). Do not
     pad this with a "here's something close" listing — that defeats the point.
   - **Matches found:** for each, give title, neighborhood, availability dates, price,
     term length, the link, and one line on why it fits.
   - If there were borderline or gender-excluded listings, add a brief separate section
     for those — clearly labeled as not recommendations, just visibility into what the
     filter is doing.

7. **Send the email** via Gmail to brendanbennett31@gmail.com.

8. **Update `seen_listings.json`.** Append every listing reviewed today — matched,
   borderline, or excluded — with `url`, `date_seen`, and `status`
   (`matched` / `borderline` / `excluded_gender` / `excluded_other`), so nothing is
   ever re-reported.

9. **Log the report.** Write a copy of today's email body to `reports/<YYYY-MM-DD>.md`.

10. **Commit and push.** Commit the updated `seen_listings.json` and the new report file
    to `claude/manhattan-rental-agent-9bxmr2` and push.

## Hard rules
- Never fabricate a listing or stretch a bad fit to avoid an empty report. "No
  additional finds" is a valid and expected outcome most days.
- Never re-surface the two reference listings already in `CRITERIA.md` — the user
  already contacted those owners.
- If every source fails to load, say that plainly in the email instead of sending
  "No additional finds" as if the search actually ran.
