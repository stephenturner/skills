---
name: funding-search
description: >
  Search for new funding opportunities in data science, AI, computational science, and related
  fields announced within the last week. Covers federal agencies (NSF, NIH, DOE, DARPA, DOD,
  USDA, DOEd, ARPA-H, State Department, DHS) and major foundations (Sloan, Schmidt Sciences,
  Gates, Hewlett, Simons, Moore, Burroughs Wellcome, MacArthur, Mellon, Carnegie, Russell Sage,
  Templeton, Chan Zuckerberg Initiative). Use this skill whenever the user asks to find new
  grants, funding opportunities, RFAs, RFPs, NOFOs, FOAs, or solicitations relevant to a
  School of Data Science, or asks to check what new funding has been announced recently.
  Also trigger when the user says "funding search," "new grants," "grant scan,"
  "what's new in funding," "check for new RFAs," or similar.
---

# Funding Opportunity Search

This skill performs a systematic web search for funding opportunities announced within the
last ~7 days that are relevant to data science, artificial intelligence, computational science,
statistics, and adjacent fields. The output is a formatted summary suitable for inclusion in a
weekly research newsletter.

## Context

The user is the Assistant Dean for Research at UVA's School of Data Science. Faculty research
spans computational genomics, applied statistics, AI/ML, NLP, network science, computer
vision, data ethics and policy, AI safety, biosecurity, digital twins, public health informatics,
and responsible AI. Opportunities of interest include those funding research, training grants,
infrastructure, workshops, and centers in these areas.

## Search Strategy

Run web searches in three waves. Use today's date to anchor recency. For each search, include
a time qualifier like "2026" or "this week" or "March 2026" (adjusted to the current date) to
bias results toward recent announcements. Expect to run 10-20 searches total.

### Wave 1: Federal Agency Announcements

Search for new solicitations from each of these agencies. Use queries like:

- `NSF new funding opportunity 2026` or `NSF dear colleague letter [current month] 2026`
- `NIH new RFA [current month] 2026` or `NIH notice of funding opportunity data science`
- `DOE funding opportunity announcement [current month] 2026`
- `DARPA broad agency announcement 2026`
- `ARPA-H funding opportunity 2026`
- `grants.gov new opportunities data science AI [current month] 2026`
- `USDA NIFA funding opportunity 2026`
- `NSF dear colleague letter 2026` (DCLs often announce new priority areas)

Also search grants.gov directly for recent postings:

- `site:grants.gov artificial intelligence [current month] 2026`
- `site:grants.gov data science [current month] 2026`

For NSF specifically, also check for new program solicitations:

- `site:nsf.gov new funding opportunity [current month] 2026`
- `site:nsf.gov program solicitation [current month] 2026`

Use web_fetch on promising results pages (e.g., grants.gov search results, NSF new funding
page) to extract details.

### Wave 2: Foundations and Private Funders

Search for new opportunities from foundations known to fund data science, AI, and
computational research:

- `Sloan Foundation new grant program 2026`
- `Schmidt Sciences funding opportunity 2026` or `Schmidt Futures RFP 2026`
- `Gates Foundation funding opportunity 2026`
- `Simons Foundation funding 2026`
- `Moore Foundation data science grant 2026`
- `Burroughs Wellcome Fund new program 2026`
- `Chan Zuckerberg Initiative funding 2026`
- `Hewlett Foundation grant opportunity 2026`
- `Templeton Foundation funding 2026`
- `MacArthur Foundation grant 2026`
- `Russell Sage Foundation RFP 2026`
- `Mellon Foundation grant 2026`
- `Carnegie Corporation grant 2026`
- `Google.org impact challenge 2026`

Adjust the year and month to match the current date.

### Wave 3: Aggregators and Broad Searches

Run broader searches to catch anything missed in Waves 1 and 2:

- `new funding opportunity artificial intelligence research [current month] 2026`
- `new grant data science research [current month] 2026`
- `new federal funding computational science 2026`
- `new RFP machine learning research 2026`
- `funding opportunity digital twins 2026`
- `funding opportunity AI safety 2026`
- `funding opportunity biosecurity bioinformatics 2026`

## Filtering

After collecting candidate opportunities, filter for:

1. **Recency**: Only include opportunities announced or posted within approximately the last
   7 days. If the posting date is unclear, include it but flag the uncertainty.
2. **Relevance**: Include opportunities where a data science, AI, statistics, or computational
   science researcher could plausibly be PI or co-PI. Cast a reasonably wide net, but skip
   opportunities that are clearly outside scope (e.g., pure humanities with no computational
   angle, clinical trials with no data science component, equipment-only grants for wet labs).
3. **Deduplication**: If the same opportunity appears in multiple searches, include it only once.
4. **New announcements only**: Only include opportunities that were announced or posted
   within the last ~7 days. Do not include a "previously noted" or "still open" section for
   older opportunities. The goal is a clean list of what is new this week.

## Output Format

Present results as a markdown list. For each opportunity, include:

- **Funder and program name** with a link to the solicitation
- **One to three sentences** describing what the program funds and why it may interest
  SDS faculty
- **Funding amount** if available (per-award range or total program budget)
- **Deadline** if available
- **Posted/announced date** if you can determine it

Use this template for each entry:

```
**[Program Name](URL)** (Funder)
Brief description of the opportunity and its relevance to data science faculty.
Amount: $X–$Y per award. Deadline: Month Day, Year.
```

### Example output:

**[Pathways to Enable Secure Open-Source Ecosystems (PESOSE)](https://new.nsf.gov/funding/opportunities/pesose)** (NSF)
Supports translation of open-source science and engineering research products into
sustainable ecosystems. Relevant to SDS faculty working on open-source software tools for
data analysis, ML frameworks, or reproducible research infrastructure.
Amount: up to $750,000. Deadline: September 1, 2026.

**[Science of Trustworthy AI](https://www.schmidtsciences.org/trustworthy-ai/)** (Schmidt Sciences)
Funds technical research to understand, predict, and control risks from frontier AI systems.
Directly relevant to SDS faculty in AI safety, responsible AI, and ML robustness.
Amount: varies. Deadline: May 17, 2026.

## Closing the Report

After listing all opportunities found, add a brief summary line stating how many new
opportunities were found and the date range searched. If a search wave returned nothing
new, say so. Example:

> Found 8 new funding opportunities posted between March 19–26, 2026. Federal agency
> searches returned 5 results; foundation searches returned 3. No new DOE or ARPA-H
> solicitations were identified this week.

Always render results in the conversation first. Then save the same content to a markdown
file at `/mnt/user-data/outputs/funding-opportunities-YYYY-MM-DD.md` (using today's date)
and present it to the user so they can download it.

## Important Notes

- Prioritize accuracy. If you cannot confirm a deadline or amount from the source, say
  "deadline TBD" or "amount not specified" rather than guessing.
- If a URL leads to a page that has been taken down or is behind a login wall, note this.
- Do not fabricate opportunities. Every entry must come from a real search result with a
  verifiable URL.
- When in doubt about relevance, include the opportunity with a note about which SDS
  research areas it might fit.