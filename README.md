# Horoscope, Tarot and Numerology MCP Server by DivineAPI

![Horoscope, Tarot and Numerology MCP Server by DivineAPI](https://raw.githubusercontent.com/DivineAPI/DivineAPI/main/assets/mcp-horoscope-numerology.png)

Connect Claude, Cursor, VS Code or any MCP client to DivineAPI's horoscope, tarot and numerology data, returned as JSON. Tools cover daily to yearly horoscopes for the 12 signs, tarot readings, Chaldean and core numerology, mobile number numerology, love calculators and white-label PDF reports.

[![Docs](https://img.shields.io/badge/docs-developers.divineapi.com-blue)](https://developers.divineapi.com/horoscope-and-tarot-api)
[![Trial](https://img.shields.io/badge/14--day%20trial-start-green)](https://divineapi.com/start-trial)
[![MCP](https://img.shields.io/badge/MCP-streamable%20HTTP-purple)](https://divineapi.com/mcp)
[![PyPI](https://img.shields.io/badge/pypi-divineapi--horoscope--numerology--mcp-orange)](https://pypi.org/project/divineapi-horoscope-numerology-mcp/)
[![Status](https://img.shields.io/badge/status-status.divineapi.com-brightgreen)](https://status.divineapi.com)
[![License: MIT](https://img.shields.io/badge/license-MIT-lightgrey)](LICENSE)

Hosted server URL:

```
https://mcp.divineapi.com/horoscope/mcp
```

One-click install (adds the hosted server with placeholder credentials):

[![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/install-mcp?name=divineapi-horoscope-numerology&config=eyJ1cmwiOiJodHRwczovL21jcC5kaXZpbmVhcGkuY29tL2hvcm9zY29wZS9tY3AiLCJoZWFkZXJzIjp7IlgtRGl2aW5lLUFwaS1LZXkiOiJZT1VSX0FQSV9LRVkiLCJYLURpdmluZS1BdXRoLVRva2VuIjoiWU9VUl9BVVRIX1RPS0VOIn19)
[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_Server-0098FF?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=divineapi-horoscope-numerology&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fmcp.divineapi.com%2Fhoroscope%2Fmcp%22%2C%22headers%22%3A%7B%22X-Divine-Api-Key%22%3A%22YOUR_API_KEY%22%2C%22X-Divine-Auth-Token%22%3A%22YOUR_AUTH_TOKEN%22%7D%7D)

After installing, replace `YOUR_API_KEY` and `YOUR_AUTH_TOKEN` in the server's headers with your own values. In VS Code you can use the `inputs` version of the config below instead, so the values are stored securely.

## Connect in 1 minute

**1. Get your credentials.** Start the [14-day free trial](https://divineapi.com/start-trial) (credit card required to activate the trial), then copy your **API key** and **auth token** from the DivineAPI dashboard.

**2. Add the server to your client.** The hosted server accepts your credentials in one of two ways:

- **Request headers:** `X-Divine-Api-Key` and `X-Divine-Auth-Token` (Cursor, VS Code, Claude Code).
- **Sign-in page (OAuth):** the client opens a DivineAPI page where you paste the API key and auth token once (Claude.ai and Claude Desktop custom connectors).

### Claude.ai and Claude Desktop (custom connector)

Custom connectors added in Claude.ai also appear in Claude Desktop.

1. Open **Customize > Connectors**, click **+ Add**, then **Add custom connector**.
2. Name: `DivineAPI Horoscope and Numerology`. URL: `https://mcp.divineapi.com/horoscope/mcp`. Click **Continue**.
3. Keep the detected sign-in settings and click **Add**. When you connect, a DivineAPI sign-in page opens: paste your API key and auth token and click **Connect**.

If your Claude plan offers request headers for custom connectors, you can instead choose no sign-in and add the two headers `X-Divine-Api-Key` and `X-Divine-Auth-Token`.

Team and Enterprise plans: an owner adds the connector under **Organization settings > Connectors** first. Steps from [Claude's custom connector guide](https://support.claude.com/en/articles/11175166).

### Cursor

`~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):

```json
{
  "mcpServers": {
    "divineapi-horoscope-numerology": {
      "url": "https://mcp.divineapi.com/horoscope/mcp",
      "headers": {
        "X-Divine-Api-Key": "YOUR_API_KEY",
        "X-Divine-Auth-Token": "YOUR_AUTH_TOKEN"
      }
    }
  }
}
```

### VS Code (GitHub Copilot agent mode)

`.vscode/mcp.json`. VS Code asks for both values on first start and stores them securely:

```json
{
  "inputs": [
    { "type": "promptString", "id": "divine-api-key", "description": "DivineAPI API key", "password": true },
    { "type": "promptString", "id": "divine-auth-token", "description": "DivineAPI auth token", "password": true }
  ],
  "servers": {
    "divineapi-horoscope-numerology": {
      "type": "http",
      "url": "https://mcp.divineapi.com/horoscope/mcp",
      "headers": {
        "X-Divine-Api-Key": "${input:divine-api-key}",
        "X-Divine-Auth-Token": "${input:divine-auth-token}"
      }
    }
  }
}
```

### Claude Code

```bash
claude mcp add --transport http divineapi-horoscope-numerology https://mcp.divineapi.com/horoscope/mcp \
  --header "X-Divine-Api-Key: YOUR_API_KEY" \
  --header "X-Divine-Auth-Token: YOUR_AUTH_TOKEN"
```

Run `/mcp` inside Claude Code to check the connection.

### Clients that send only one credential field

If a client can send only a single bearer token and no custom headers, send `Authorization: Bearer YOUR_API_KEY:YOUR_AUTH_TOKEN` (API key, a colon, then the auth token). The server splits it into the two credentials.

## Example prompts

- "What is today's horoscope for Leo? Then give me the weekly one too."
- "Draw a yes or no tarot card for: should I take the new job?"
- "My name is Priya Sharma, born 12 August 1994. Calculate my core numbers and my Lo Shu grid."
- "Analyse the mobile number 9876543210 with numerology."
- "Run the love calculator for Rahul and Simran."
- "Generate a Sampoorna Kundali PDF for this birth data with my company name, logo and footer."

## Tool groups

Tool counts below are counted from the `@mcp.tool` definitions in [`server.py`](server.py) of this repository (version 1.4.2). The hosted server is updated more often and can list more tools; your MCP client always shows the live list.

| Group | Tools | What they cover | Host |
|---|---|---|---|
| Horoscopes | 6 | Daily, weekly, monthly and yearly horoscopes by sign, Chinese horoscope, numerology horoscope | astroapi-5 |
| Tarot and readings | 23 | Yes or no tarot, daily tarot, past-present-future spread, love compatibility by sign, in-depth love, love triangle, heartbreak, career daily reading, fortune cookie, coffee cup reading, wisdom reading and other card readings | astroapi-5 |
| Chaldean numerology | 12 | Lo Shu grid, zodiac planet number, luck numerology, name number, birthday number, missing numbers, driver and conductor numbers, two- and three-number arrows, repeating numbers, yearly prediction, numerology gemstones | astroapi-7 |
| Core numbers | 1 | `divine_core_numbers`: all core numbers from full name and birth date | astroapi-4 |
| Mobile numerology | 2 | Analyse a mobile number, suggest a new mobile number | astroapi-7 |
| Calculators | 2 | FLAMES calculator, love calculator | astroapi-7 |
| Lifestyle | 3 | Zodiac gift guru, beauty by the stars, astro chic picks | astroapi-7 |
| PDF reports: Vedic | 9 | Kundali Sampoorna, Ananta and Prakash, matchmaking, government job, foreign travel and settlement, 5-, 10- and 15-year Vedic predictions | pdf |
| PDF reports: Western and numerology | 5 | Natal report, couple report, numerology prediction report, numerology report, Reports V2 (`divine_pdf_reports_v2`) | pdf |

All tools are read-only (`readOnlyHint: true`) and each tool call makes one request to the DivineAPI REST API.

### Inputs the model needs to know

- Zodiac signs are lowercase (`aries` to `pisces`).
- Daily horoscope and lifestyle tools take `h_day` = `today`, `tomorrow` or `yesterday`. Weekly, monthly and yearly tools take `current`, `prev` or `next`, not calendar dates.
- `tzone` is a decimal UTC offset, for example `5.5`.
- Numerology tools take `full_name` plus the birth date as separate `day`, `month`, `year` fields. Numerology responses are in English.
- `lan` sets the response language for horoscope and tarot tools (default `en`).
- PDF report tools need a `report_code` plus the branding fields the API requires on every PDF request: `company_name`, `company_url`, `company_email`, `company_mobile`, `company_bio`, `logo_url` and `footer_text` (a few legacy reports, such as the natal report, also need `company_landline`). Send `company_mobile` even where a tool marks it optional. The natal PDF also needs a `theme`. See [report samples](https://reports.divineapi.com/reports).

## Self-hosting

You only need this if you want to run the server yourself instead of using the hosted URL. The server reads your credentials from two environment variables: `DIVINE_API_KEY` and `DIVINE_AUTH_TOKEN`.

### Local (stdio) with uv or pip

```bash
# uv (no install step)
uvx divineapi-horoscope-numerology-mcp

# or pip
pip install divineapi-horoscope-numerology-mcp
divineapi-horoscope-numerology-mcp
```

Claude Desktop local config (`claude_desktop_config.json`; on macOS `~/Library/Application Support/Claude/`, on Windows `%APPDATA%\Claude\`):

```json
{
  "mcpServers": {
    "divineapi-horoscope-numerology": {
      "command": "uvx",
      "args": ["divineapi-horoscope-numerology-mcp"],
      "env": {
        "DIVINE_API_KEY": "YOUR_API_KEY",
        "DIVINE_AUTH_TOKEN": "YOUR_AUTH_TOKEN"
      }
    }
  }
}
```

To run exactly this repository's code:

```bash
git clone https://github.com/DivineAPI/mcp-horoscope-numerology.git
cd mcp-horoscope-numerology
pip install .
DIVINE_API_KEY=YOUR_API_KEY DIVINE_AUTH_TOKEN=YOUR_AUTH_TOKEN divineapi-horoscope-numerology-mcp
```

### Remote (streamable HTTP) with Docker

```bash
docker build -t divineapi-horoscope-mcp .
docker run -p 8000:8000 -e MCP_JWT_SECRET=a-long-random-string divineapi-horoscope-mcp
```

The MCP endpoint is then `http://localhost:8000/mcp`. Clients send their own `X-Divine-Api-Key` and `X-Divine-Auth-Token` headers. `docker compose up` maps the same server to port 8003 and passes the variables below from your shell.

| Variable | Used for |
|---|---|
| `DIVINE_API_KEY`, `DIVINE_AUTH_TOKEN` | Your DivineAPI credentials. Required in stdio mode; in HTTP mode, per-request headers or the sign-in page override them |
| `MCP_TRANSPORT` | `stdio` (default) or `http` (set by the Dockerfile) |
| `MCP_HOST` | Public host name for the OAuth sign-in flow and allowed hosts (default `mcp.divineapi.com`) |
| `MCP_JWT_SECRET` | Secret that signs session tokens. Set a fixed value so sign-ins survive restarts |

The OAuth sign-in flow expects HTTPS on `MCP_HOST` behind a reverse proxy that serves the server under `/horoscope/`. Header authentication works without it.

## Which plan includes this

The tools call the Horoscope, Tarot and Numerology endpoints, so they use the matching DivineAPI plans (Horoscope Starter or Pro, Tarot Basic, Essentials or Gold, Numerology API); a tool works when your plan includes its endpoint. Compare plans at [divineapi.com/pricing](https://divineapi.com/pricing).

## FAQ

**Do I need a DivineAPI account for horoscope, tarot and numerology tools?**
Yes. Horoscope and tarot tools call `astroapi-5`, numerology tools call `astroapi-4` and `astroapi-7`, and PDF tools call the `pdf` host, each with your API key and auth token. Start with the [14-day free trial](https://divineapi.com/start-trial) (credit card required to activate the trial); plans and prices are at [divineapi.com/pricing](https://divineapi.com/pricing).

**Can I use it for a daily horoscope bot in Claude or Cursor?**
Yes, in any client with remote MCP over streamable HTTP (Claude.ai, Claude Desktop, Claude Code, Cursor, VS Code): ask for "today's horoscope for Leo" and the client calls the horoscope tool. Local-only clients can run `divineapi-horoscope-numerology-mcp` over stdio.

**Can I ask for a horoscope by date?**
No. Horoscope tools take keyword selectors (`today`, `tomorrow`, `yesterday`, or `current`, `prev`, `next`), so ask for "today", "next week" or "this month".

**What do the PDF tools return?**
Links to a generated, white-label PDF with your branding. The REST API offers 125+ PDF report types; this server exposes the 14 report tools listed above. Samples: [reports.divineapi.com/reports](https://reports.divineapi.com/reports).

**I see tarot or PDF tools that the table does not list. Why?**
The counts come from version 1.4.2 of this repository's `server.py`. The hosted server and the PyPI package can expose newer tools.

**I want the REST API, not MCP.**
The same data is available over plain HTTP: see [horoscope-api](https://github.com/DivineAPI/horoscope-api), [tarot-api](https://github.com/DivineAPI/tarot-api), [numerology-api](https://github.com/DivineAPI/numerology-api) and the [API reference](https://developers.divineapi.com). SDKs: `pip install divineapi`, `npm install divineapi`, `composer require divineapi/divineapi`.

## Related repos

| Repo | What it is |
|---|---|
| [horoscope-api](https://github.com/DivineAPI/horoscope-api) | Horoscope REST API quickstart |
| [tarot-api](https://github.com/DivineAPI/tarot-api) | Tarot REST API quickstart |
| [numerology-api](https://github.com/DivineAPI/numerology-api) | Numerology REST API quickstart |
| [astrology-api](https://github.com/DivineAPI/astrology-api) | Overview of all DivineAPI domains |
| [mcp-indian-astrology](https://github.com/DivineAPI/mcp-indian-astrology) | Vedic astrology MCP server |
| [mcp-western-astrology](https://github.com/DivineAPI/mcp-western-astrology) | Western astrology MCP server |
| [divineapi-python](https://github.com/DivineAPI/divineapi-python) | Python SDK (`pip install divineapi`) |
| [divineapi-node](https://github.com/DivineAPI/divineapi-node) | Node.js / TypeScript SDK (`npm install divineapi`) |
| [divineapi-php](https://github.com/DivineAPI/divineapi-php) | PHP SDK (`composer require divineapi/divineapi`) |

## Support

- MCP overview: [divineapi.com/mcp](https://divineapi.com/mcp)
- API reference: [horoscope and tarot](https://developers.divineapi.com/horoscope-and-tarot-api), [numerology](https://developers.divineapi.com/numerology-apis), [PDF reports](https://developers.divineapi.com/pdf-report-api)
- Help center: [support.divineapi.com](https://support.divineapi.com)
- API status: [status.divineapi.com](https://status.divineapi.com)

## License

MIT. See [LICENSE](LICENSE).
