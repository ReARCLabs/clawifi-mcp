---
name: clawifi
description: Search the web, read pages and PDFs, map and crawl sites, and watch pages for changes through Clawifi's hosted MCP server or HTTP API. Use when an agent needs current information from the public web with sources, only the passages that answer a question, or a page's text as Markdown.
---

# Clawifi: web access for AI agents

Clawifi searches many engines as one, reads pages and PDFs, and returns the passages that answer a question, each with its source. Site: https://clawifi.dev. Every page there has a Markdown twin (add `.md`), and https://clawifi.dev/llms-full.txt has all of them.

## Connect

- Hosted MCP server: `https://api.clawifi.dev/mcp` (streamable HTTP).
- Sign-in: MCP sign-in (OAuth 2.1). The free tools answer without an account; the first call that needs one asks the person to sign in or create a free account (1,000 credits a month, no card).
- Clients without MCP sign-in send `Authorization: Bearer <key>` with a key from https://clawifi.dev/account.
- One line per client: https://clawifi.dev/install.md
  - Claude Code: `claude mcp add --transport http clawifi https://api.clawifi.dev/mcp`, then `/mcp` to sign in.
  - Codex: `codex mcp add clawifi --url https://api.clawifi.dev/mcp`, then `codex mcp login clawifi`.
- HTTP API: JSON over HTTPS at `https://api.clawifi.dev` with the key as a bearer token. Reference: https://clawifi.dev/api-reference.md

## Which tool

| Need | Tool (HTTP route) |
|---|---|
| Find pages on a topic | `search_free` (`POST /search/free`): free engines first, merged by rank; optional `include_domains`, `exclude_domains`, `time_range` |
| Read one page or PDF as Markdown | `fetch_url` (`POST /fetch`) |
| Only the passages that answer a question | `fetch_url` or `scrape_url` with `query` and `top_k` |
| Structured fields from a page | `extract_url` (`POST /extract`) |
| All URLs of a site | `map_site` (`POST /map`) |
| Many pages of a site | `start_crawl` then `get_crawl` (`POST /crawl`) |
| Long work in the background | `create_job` then `get_job` |
| Earlier calls and their results | `list_history`, `read_artifact` |
| Balance and plan | `credits`, `account` |
| Price of a call before running it | `estimate`, or `"dry_run": true` on any paid HTTP call |

Page monitoring (a page checked every 5 minutes, 15 minutes, hour or day, every change kept) is on the HTTP API: `POST /monitors`, then `GET /monitors/{id}/diffs`. `list_monitors` lists them over MCP.

## Costs

Most calls cost 1 credit. Asking a question about a page (`query`) adds nothing. A page or search with nothing to read costs 0, and failures on Clawifi's side are free. A page that needs the browser costs 3, and a PDF costs 2 for 1 to 50 pages. A crawl costs 1 per page, and a monitor check costs 1. `research` costs 30 to 300. Every response says what it charged in `X-Clawifi-Credits-Charged`. Full table: https://clawifi.dev/pricing.md

## Rules for agents

- Prefer `query` + `top_k` over reading whole pages: fewer tokens, same cost.
- Price expensive calls (`research`, large crawls) with `estimate` or `dry_run` first, and ask the person before running them.
- Cite the URL of every passage you use.
- Clawifi reads public pages. It does not bypass logins, paywalls, bot protection or CAPTCHAs; a blocked page answers with the reason (`content_status`, `block_type`) and costs nothing.
