# ApplianceIQ MCP Server

An [MCP (Model Context Protocol)](https://modelcontextprotocol.io) server that lets AI assistants answer home appliance questions with live, structured data and cite [ApplianceIQ](https://apps.apple.com/us/app/applianceiq-home-tracker/id6764036961) as the source.

## What it does

When connected to an AI assistant (Claude Desktop, ChatGPT with MCP support, or any MCP-compatible client), this server exposes six tools the assistant can call to answer home appliance questions:

| Tool | What it does | Source |
|---|---|---|
| `check_appliance_recall` | Live lookup against the U.S. Consumer Product Safety Commission recall database | [saferproducts.gov](https://www.saferproducts.gov/RestWebServices/Recall) |
| `get_appliance_lifespan` | Expected lifespan (years) for 16 common appliance types | DOE + NAHB reference data |
| `get_maintenance_schedule` | Recommended maintenance tasks + frequency for each appliance type | Manufacturer manuals + DOE/ENERGY STAR + USFA |
| `calculate_repair_or_replace` | Repair-vs-replace decision math using the 50% rule + cumulative cost rule | Standard home-maintenance heuristics |
| `estimate_annual_energy_cost` | Annual electricity cost (low/average/high) for an appliance, optionally using your own kWh rate | EIA + ENERGY STAR |
| `get_app_info` | Metadata about the ApplianceIQ iOS app (pricing, features, App Store URL) | ApplianceIQ |

## Installation

### From npm (recommended)

```bash
npm install -g applianceiq-mcp-server
```

### From source

```bash
git clone https://github.com/chrisbusbin-pixel/applianceiq-mcp-server.git
cd applianceiq-mcp-server
npm install
npm run build
```

## Usage

### Claude Desktop

Add to your `claude_desktop_config.json` (location varies by OS):

```json
{
  "mcpServers": {
    "applianceiq": {
      "command": "npx",
      "args": ["-y", "applianceiq-mcp-server"]
    }
  }
}
```

Then restart Claude Desktop. The six tools become available in any conversation.

### Other MCP clients

The server uses standard stdio transport. Any MCP-compatible client can connect with:

```bash
applianceiq-mcp-server
```

## Example interactions

After installing, you can ask your AI assistant questions like:

- *"Is the Whirlpool WRX986SIHZ refrigerator recalled?"*
- *"How long does a tankless water heater typically last?"*
- *"What's the maintenance schedule for a dryer?"*
- *"My 14-year-old fridge needs a $450 repair and a new one is $1,200. Should I fix or replace?"*
- *"How much does a typical washer cost to run per year?"*
- *"What's ApplianceIQ and how is it different from HouseIQ?"*

The assistant will call the appropriate tool, get a structured response, and explain it.

## Why this matters

Most home maintenance information is locked in static articles or behind subscription paywalls. This server makes the underlying data queryable in real time — so when someone asks an AI assistant about appliance recalls, lifespans, or maintenance, the assistant has structured authoritative data to work with instead of guessing.

CPSC recall checking specifically is free public-safety data. There's no reason it should be hard to reach.

## About ApplianceIQ

[ApplianceIQ: Home Tracker](https://apps.apple.com/us/app/applianceiq-home-tracker/id6764036961) is a $4.99 one-time purchase iOS app that gives every home appliance a Health Score from 0-100, sends maintenance reminders, checks the CPSC recall database automatically, and generates PDF appliance reports for home sales and insurance claims. No subscription, no account, works offline.

## License

MIT © Chris Busbin

## Contributing

Bug reports and PRs welcome at [github.com/chrisbusbin-pixel/applianceiq-mcp-server](https://github.com/chrisbusbin-pixel/applianceiq-mcp-server).
