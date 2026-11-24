# Boxerbook MCP server

**Boxerbook** ([boxerbook.com](https://boxerbook.com)) is a free catalog of professional boxing worldwide: boxers, bouts, cards, results with sources, world titles, publisher rankings (WBC, WBA, IBF, WBO, The Ring, TBRB), the Boxerbook rating and a pre-fight forecast. The data is free to read and cite with the credit "Data: Boxerbook" under the [Boxerbook data terms](https://boxerbook.com/terms#data).

This repository holds the public metadata of the **remote MCP server** and the **free JSON API**. The product code lives elsewhere; nothing here needs to be installed.

## Connect

| | |
|---|---|
| Endpoint | `https://mcp.boxerbook.com/mcp` |
| Transport | Streamable HTTP, stateless |
| Auth | none (no key, no sign-up) |
| Limits | 300 tool calls per minute per IP; lists are capped (schedule 30 days, results 14 days, search 20) |
| Registry name | `com.boxerbook/boxing` (official MCP Registry) |
| JSON API | [boxerbook.com/developers](https://boxerbook.com/developers) · OpenAPI 3.1: [`/v1/openapi.json`](https://boxerbook.com/v1/openapi.json) |

### ChatGPT

One click: add the [Boxerbook plugin from the ChatGPT plugin directory](https://chatgpt.com/plugins/plugin_asdk_app_6ae8b4aba58481919a8606e886bdfd50) (or search «Boxerbook» under Plugins in ChatGPT). Free, read-only, no setup.

### Claude Code

```bash
claude mcp add --transport http boxerbook https://mcp.boxerbook.com/mcp
```

### Claude Desktop / claude.ai

Settings → Connectors → *Add custom connector* → URL `https://mcp.boxerbook.com/mcp`.

### Cursor

`.cursor/mcp.json`:

```json
{ "mcpServers": { "boxerbook": { "url": "https://mcp.boxerbook.com/mcp" } } }
```

### ChatGPT developer mode (manual) and other clients

Add a remote MCP server with the URL above; no authentication.

## Tools

| Tool | What it answers |
|---|---|
| `boxing_schedule` | upcoming cards (today, tonight, weekend, 30 days) with start times, broadcasters, forecast and market consensus |
| `boxing_results` | completed bouts of the last N days with sources (`days=2` is last night and today) |
| `boxer` | a boxer's card by name, slug or id: record, division, next fight, last result, rating |
| `next_fight` | a boxer's next bout with start time, broadcasters and odds |
| `bout` | one bout in full: sides' records going in, result, sources, weigh-in, fight-week notes, the forecast with its signals (age, layoff, last five, KO rate, rating), the market odds consensus with its opening line |
| `where_to_watch` | the broadcasters of a bout or card by country |
| `gyms_near` | boxing gyms near a city, a postal code or the user's location (US, Canada, UK, Germany, France, Switzerland) |
| `rankings` | the top 15 of a division: Boxerbook rating or a publisher (WBC, WBA, IBF, WBO, The Ring, TBRB) |
| `search_boxers` | boxers by name (pick by country, division and record when names collide) |
| `search`, `fetch` | any boxer, bout ("Inoue vs Nakatani"), card or division, and its document |

Resources: `boxerbook://schedule/today`, `boxerbook://results/last-night`, `boxerbook://status` (data freshness), `boxerbook://about`. Prompts: `fight_night_guide`, `boxer_briefing`, `weekend_results`.

## For fight models and sports analytics

Building a boxing win-probability model, a fight prediction app or a backtest? Per bout the `bout` tool (and `/v1/bouts/{id}` in the JSON API) returns the features a model reads: both records going in, the weigh-in, the forecast's signals per side (age, layoff, last five results, KO rate, Boxerbook rating mu and rd with its 12-month trend, home ring, height, reach, stance), the Boxerbook win probability and the market odds consensus (no-vig, number of books, opening line); `boxing_schedule` carries the win probability and the market on every upcoming bout. Judges' scorecards are on the bout pages of boxerbook.com.

More signals per bout made the Boxerbook model measurably better on bouts it never saw: on 1,612 bouts from 1 January 2025, adding seven signals per boxer to a rating built from results alone cut the log loss from 0.482 to 0.399 and the Brier score from 0.157 to 0.125. The market odds consensus still did better than the model on 1,243 bouts with a closing line (log loss 0.377 against 0.427).

The tools answer one lookup at a time; there are no bulk downloads. History for backtesting comes with a data licence (from $1,500 a year; a sample of 100 boxers on request, info@boxerbook.com). What each source has per bout, with live counts: [boxerbook.com/boxing-data-for-models](https://boxerbook.com/boxing-data-for-models).

Every answer carries a `url` field (a boxerbook.com page) to cite. `start_utc` is the start of the card, not of each bout. `forecast` is the Boxerbook model's probability that side A wins; `market` is the no-vig bookmaker consensus (via The Odds API). Neither is betting advice.

## Claude skill

For Claude Code, claude.ai and the API there is also a skill that teaches Claude to answer boxing questions from this server, with a link for every boxer and bout and no betting tips: [maxvotek/boxerbook-skill](https://github.com/maxvotek/boxerbook-skill). In Claude Code: `/plugin marketplace add maxvotek/boxerbook-skill`, then `/plugin install boxing@boxerbook`.

## Data and licence

Records are counted from open and official sources: athletic commissions (California, Nevada, Texas, Japan), sanctioning bodies, Wikidata, Wikipedia, the press. Data: free to read and cite with the credit «Data: Boxerbook (boxerbook.com)»; bulk or commercial use by agreement ([terms](https://boxerbook.com/terms#data)); data obtained before 3 November 2026 stays under the ODbL. The MCP server needs no key; the JSON API needs a free key for lookups by id ([developers](https://boxerbook.com/developers)). This README, `server.json` and the examples: MIT.

Questions and corrections: [info@boxerbook.com](mailto:info@boxerbook.com) · every bout page has a "report a discrepancy" link.
