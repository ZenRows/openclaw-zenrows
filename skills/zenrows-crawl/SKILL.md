---
name: zenrows-crawl
description: Collect the content of many pages under one site or section by following internal links from a starting URL. Use when the user wants more than one page, for example "all the docs under /guide" or "every blog post on this site". Not for listing URLs without fetching their content (use zenrows-map), not for a single page (use zenrows-scrape), and not for a list of URLs the user already has (use zenrows-batch).
---

# Crawl from a seed URL

Discover and fetch internal pages starting from one seed, staying on the same host and path prefix. The agent runs the loop in conversation.

## Defaults

- `max_pages`: 25
- `max_depth`: 2
- Scope: same host and same path prefix as the seed.

## Instructions

1. Confirm the seed URL and, if the user implied limits, the `max_pages` and `max_depth` to use.
2. Fetch the seed: `scrape(url, mode='auto', outputs='links', response_type='markdown')`. This returns the page content and its links in one call.
3. Filter the returned links to the same host and the same path prefix as the seed. Drop off-site links, anchors, and already-seen URLs.
4. Enqueue the surviving links. Repeat steps 2 and 3 for each, increasing depth, until you reach `max_pages` or `max_depth`.
5. Write each fetched page as one JSON line to `./.zenrows/crawl-<host>-<timestamp>.jsonl`, with at least `{ "url", "depth", "content" }`. Create `./.zenrows/` if it does not exist.
6. Do not read the whole output file back into context. Summarize from what you fetched, or read it incrementally with `grep` and `head`.

## Notes

- A crawl multiplies requests, so `max_pages` and `max_depth` are the cost control. Keep them as low as the task allows.
- Pass `mode='auto'` on every fetch so JavaScript-heavy pages still return content.
- For URL discovery without fetching content, use the `zenrows-map` skill instead. It is far cheaper.
- Once the URL list is known (for example from `zenrows-map`), use the `zenrows-batch` skill rather than this loop. Batch submits them as one managed job with server-side retries.
