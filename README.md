# US Import Records Buyer Finder MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-us-import-records-buyer-finder/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-us-import-records-buyer-finder) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-us-import-records-buyer-finder%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-us-import-records-buyer-finder&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-us-import-records-buyer-finder)](https://www.npmjs.com/package/@mambalabsdev/mcp-us-import-records-buyer-finder) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-us-import-records-buyer-finder)](https://www.npmjs.com/package/@mambalabsdev/mcp-us-import-records-buyer-finder) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-us-import-records-buyer-finder)](https://github.com/mambalabsdev/mcp-us-import-records-buyer-finder/blob/main/LICENSE)

An MCP server for the Mamba Labs US Import Records Buyer Finder actor on Apify. Find US importers and their overseas suppliers from customs records.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Search company names and get one row per US importer or overseas supplier, with shipment counts and last shipment dates. Ask for profiles to add trading partners, HS codes, product descriptions, and recent shipments, up to 20 a run.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-us-import-records-buyer-finder": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-us-import-records-buyer-finder"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `find_us_importers` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Find US importers with pickleball in their name that shipped this year."
- "Who supplies Pickleball Central, and from which countries?"
- "Which overseas suppliers named Zhejiang ship the most to the US?"

## Tool

`find_us_importers`. One tool. Example arguments:

```json
{
  "search": [
    "pickleball"
  ],
  "max_results": 20
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `search` | array | no | Company name words, for example pickleball or walmart. The site matches company names, not what a company imports. Up to 50 terms. |
| `company_urls` | array | no | ImportYeti company or supplier links to read in full, up to 20 a run. |
| `mode` | string | no | importers: US companies importing. suppliers: overseas companies shipping to the US. both: either. |
| `date_from` | string | no | Optional, YYYY-MM-DD. Drops companies whose last shipment is older. |
| `min_shipments` | number | no | Optional. Drops companies with fewer shipments in total. |
| `max_results` | number | no | Up to 1000. Default 100. The site served about 140 rows per term in testing, so a broad term can stop sooner. |
| `include_profiles` | boolean | no | true adds trading partners, their countries, HS codes, product descriptions, and recent shipments to the first rows, up to Maximum profiles. Priced as a profile row. |
| `max_profiles` | number | no | Up to 20, because the site limits page views per address and this actor stops there. Each profile adds about 7 seconds. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `company-row`, one company from search | $0.0025 | $0.002375 | $0.00225 | $0.002125 |
| `company-profile-row`, one company with its profile read | $0.008 | $0.0076 | $0.0072 | $0.0068 |
| `apify-actor-start`, per run start | $0.002 | $0.002 | $0.002 | $0.002 |

Summary rows, refused input, filtered rows, and refused pages are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/us-import-records-buyer-finder](https://apify.com/mambalabs/us-import-records-buyer-finder)

## License

MIT. See [LICENSE](LICENSE).
