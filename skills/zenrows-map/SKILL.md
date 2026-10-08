---
name: zenrows-map
description: Discover the URLs that exist on a site without fetching page content, usually from its sitemap. Use when the user wants a list of a site's pages, its structure, or "what's on this site". Not for retrieving page content (use zenrows-scrape, zenrows-batch, or zenrows-crawl).
---

# Map a site's URLs

Return a flat list of discovered URLs and nothing else. This costs 1 to 3 requests, not a full crawl.

## Instructions

1. Try the sitemap first: `scrape(url='https://<host>/sitemap.xml', mode='auto')`. If it returns a sitemap, parse out the URLs and stop. Most sites that have one expose it here.
2. If the sitemap is missing or empty, fetch `scrape(url='https://<host>/robots.txt', response_type='plaintext', mode='auto')` and read any `Sitemap:` lines, then fetch those sitemaps.
3. If both fail, do a single `scrape(url='https://<host>/', outputs='links', mode='auto')` on the homepage and dedupe to the same host.
4. Return the deduplicated URL list. Do not fetch the content of the discovered pages; that is what `zenrows-crawl` is for.

## Notes

- Output is a list of URLs only. If the user then wants page content, hand off to `zenrows-scrape` (one page) or `zenrows-batch` (the discovered list, as one managed job).
- For a large list, write it to `./.zenrows/map-<host>-<timestamp>.txt` and report the count rather than pasting everything into the conversation.
