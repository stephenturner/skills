# funding-search

A Claude skill that searches for new funding opportunities announced in the last ~7 days, filtered for relevance to a School of Data Science.

## What it does

Runs three waves of web searches covering federal agencies (NSF, NIH, DOE, DARPA, ARPA-H, USDA, DHS, etc.) and major foundations (Sloan, Schmidt Sciences, Gates, Simons, Moore, Burroughs Wellcome, CZI, Hewlett, Templeton, and others). Results are filtered for recency, relevance, and deduplicated, then formatted as a concise report with program names, links, descriptions, amounts, and deadlines.

## Usage

Say any of the following:

- "funding search"
- "check for new grants"
- "what new funding opportunities were announced this week?"
- "grant scan"
- "new RFAs"

Output renders in conversation and saves to `funding-opportunities-YYYY-MM-DD.md`.

## Installation

Copy `SKILL.md` into your skills directory:

```
~/.claude/skills/funding-search/SKILL.md
```

## Notes

- Expects web search tool access. Won't work in offline or restricted-network environments.
- Typically runs 10-20 searches per invocation. Expect it to take a minute or two.
- NIH moved all NOFOs to Grants.gov in FY2026, which makes individual new postings harder to catch via general web search. Federal aggregator searches partially compensate.