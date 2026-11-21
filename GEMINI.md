# Clawifi

The `clawifi` MCP server (https://api.clawifi.dev/mcp) gives you web search and page reading. Docs: https://clawifi.dev/docs

- Search: `search_free` returns ranked results (url, title, snippet, rank); read a result with `fetch_url`.
- Read a page or PDF as Markdown: `fetch_url`. Add `query` (and `top_k`) to get only the passages that answer a question.
- Structured fields: `extract_url`. A site's URLs: `map_site`. Several pages: `start_crawl`, then `get_crawl`.
- Costs: most calls cost 1 credit. Price a call first with `estimate`; `credits` shows the balance.
- Without an account only `search_free`, `fetch_url`, `scrape_url` and `map_site` work, with low limits. When a tool asks for sign-in, tell the user to run `/mcp auth clawifi` (a free account has 1,000 credits a month).
- Cite the URL of every passage you use. Clawifi reads public pages; it does not bypass logins, paywalls or CAPTCHAs.
