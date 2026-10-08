---
name: zenrows-getting-started
description: First-run setup and troubleshooting for the Zenrows plugin in OpenClaw. Use when the plugin was just installed, scraping is not working, a Zenrows tool is missing or "not available", Zenrows asks for a login, or the user asks whether Zenrows is set up. Not for normal scraping tasks once the plugin works (use zenrows-scrape).
---

# Getting started

Run the checks below. Do the work; do not just print instructions. The Zenrows plugin connects through OAuth, so there is no API key to set.

## 1. Probe the connection

First confirm the Zenrows MCP tools (`scrape`, `browser_*`) are available in this session.

- Tools missing entirely: the MCP server is not connected. Ask the user to run `openclaw plugins inspect zenrows` and check that the plugin is installed and enabled. If it is, Zenrows is probably not connected yet: OpenClaw leaves an OAuth server out of the session until it is authorized. Go to the login step below.
- Tools present: call `scrape(url='https://httpbin.io/get', response_type='plaintext', mode='auto')` and branch:
  - 200 with a body: Zenrows is working. Go to step 2.
  - 401, or an `AUTH` error: the Zenrows login is missing or expired. Ask the user to open the OpenClaw Control UI (`openclaw dashboard`), go to the Zenrows plugin page, choose **Connect** under **Accounts**, sign in to Zenrows and approve access, then start a new session and retry. With the manual MCP setup instead of the plugin, the command is `openclaw mcp login zenrows`. Do not ask for an API key.

## 2. First successful scrape

Run `scrape(url='https://www.scrapingcourse.com/ecommerce/', mode='auto', response_type='markdown')` and show the user the first 10 lines. This confirms the full path works.

## 3. Where to go next

Offer three copy-ready prompts:

- "Fetch the docs at <url> and summarize the key points."
- "Get all the product links from <url>."
- "Map the URLs on <host>."

## Notes

- Error-code reference: search the code (for example `AUTH002` or `REQS002`) at https://docs.zenrows.com.
- Once the plugin is working, hand normal tasks to the capability skills: `zenrows-scrape`, `zenrows-extract`, `zenrows-batch`, `zenrows-crawl`, `zenrows-map`, `zenrows-browser`, `zenrows-capture-api-calls`.
