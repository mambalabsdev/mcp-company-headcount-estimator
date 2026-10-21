# Company Headcount Estimator MCP Server

[![Smithery](https://smithery.ai/badge/mambabuilt/mcp-company-headcount-estimator)](https://smithery.ai/servers/mambabuilt/mcp-company-headcount-estimator) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-company-headcount-estimator/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-company-headcount-estimator) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-company-headcount-estimator%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-company-headcount-estimator&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-company-headcount-estimator)](https://www.npmjs.com/package/@mambalabsdev/mcp-company-headcount-estimator) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-company-headcount-estimator)](https://www.npmjs.com/package/@mambalabsdev/mcp-company-headcount-estimator) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-company-headcount-estimator)](https://github.com/mambalabsdev/mcp-company-headcount-estimator/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-company-headcount-estimator)

An MCP server for the Mamba Labs Company Headcount Estimator actor on Apify. Give it a company domain or a list. Each domain returns an employee count band with the source it came from and a confidence score.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Your keys](#your-keys)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

For each domain the actor reads, in order: the LinkedIn company size as Google prints it in its search results (a result counts only when it shows the domain as the company website; no request goes to linkedin.com), the employee count on Wikidata for the item whose official website is the domain, and the `numberOfEmployees` the homepage declares. It returns `employee_band`, `employee_band_min`, `employee_band_max`, an exact `employee_count` when a source gives one, `estimate_source`, `confidence` (0 to 1), and `sources_agree`, plus GitHub public members and open jobs as context. Rows with no size come back free with `row_status` `no_data`.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-company-headcount-estimator": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-company-headcount-estimator"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `estimate_company_headcount` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.
- Optional: `GITHUB_TOKEN`, your own free GitHub token, for GitHub member counts on large batches.

## Example prompts

- "How many employees does linear.app have?"
- "Estimate the headcount band for these 20 domains and keep only the ones with 51 to 500 staff."
- "Which of these companies have more than 1,000 employees, and how sure is each answer?"

## Tool

`estimate_company_headcount`. One tool. Example arguments:

```json
{
  "domains": ["Linear | linear.app", "posthog.com", "clay.com"]
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Description |
|---|---|---|
| `domain` | string | One company domain |
| `company_name` | string | The name for that one domain; empty reads it from the homepage |
| `domains` | array | Up to 500 domains; `Name \| domain.com` passes the name |
| `sources` | array | Any of `linkedin`, `wikidata`, `homepage`, `github`, `jobs`; empty means all |
| `skip_cache` | string | true or false. Default false; a row from the last 7 days comes back free |
| `max_charge_usd` | string | Optional cost cap for the run |

The actor's `github_token` input is filled from this server's `GITHUB_TOKEN` environment variable when it is set.

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `company-estimated`, per row with an employee band | $0.036 | $0.0342 | $0.0324 | $0.0306 |
| `apify-actor-start`, per run start (one per GB of run memory) | $0.0015 | $0.0015 | $0.0015 | $0.0015 |

No data, invalid input, error, and cached rows are free.

## Your keys

The actor needs no key. A GitHub token is optional and only raises the GitHub limit for member counts. Proxy traffic and Google result pages add nothing to your price: Mamba Labs pays the platform costs of the run.

## Full actor documentation

[apify.com/mambalabs/company-headcount-estimator](https://apify.com/mambalabs/company-headcount-estimator)

## License

MIT. See [LICENSE](LICENSE).
