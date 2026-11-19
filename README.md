# Shiptrail MCP

Live AIS ship tracking for AI agents: any vessel, the ships near any place, port arrivals and trends at sea. Find any vessel by name, MMSI or IMO, read her position, voyage, particulars and photo, and read any port's arrivals and departures.

Shiptrail is a free live map of ships at [shiptrail.io](https://shiptrail.io), built from AIS broadcasts heard by receivers near coasts. This is a hosted remote MCP server: there is nothing to install or run.

- **Endpoint:** `https://shiptrail.io/mcp`
- **Transport:** Streamable HTTP. Protocol 2026-07-28 (stateless, with `server/discover`), and 2025-11-25, 2025-06-18 and 2025-03-26 for clients that open with `initialize`
- **Auth:** none. No key, account or sign-in.
- **Cost:** free
- **Docs:** [shiptrail.io/agents](https://shiptrail.io/agents)

## Listed in

- The official MCP Registry (registry.modelcontextprotocol.io) as `io.shiptrail/shiptrail`
- [Glama](https://glama.ai/mcp/connectors/io.shiptrail/shiptrail)

[![Shiptrail MCP connector](https://glama.ai/mcp/connectors/io.shiptrail/shiptrail/badges/score.svg)](https://glama.ai/mcp/connectors/io.shiptrail/shiptrail)

## Tools

All tools are read-only. Each returns a plain-language sentence plus structured JSON.

| Tool | What it does |
|---|---|
| `search_ships` | Find a ship by name, MMSI or IMO |
| `get_ship` | Read one ship by MMSI: position, voyage, particulars, photo |
| `search_ports` | Find a port by name or UN/LOCODE |
| `get_port` | Read a port's arrivals and departures |
| `get_ships_near` | Ships near a port or a point |
| `get_trends` | What the fleet shows now: queues at anchor, busy straits, false positions |

## Connect

**Claude Code**

```bash
claude mcp add --transport http shiptrail https://shiptrail.io/mcp
```

**Claude Desktop and claude.ai:** Settings > Connectors > Add custom connector, name it Shiptrail, paste `https://shiptrail.io/mcp`.

**ChatGPT:** add a custom connector in developer mode with `https://shiptrail.io/mcp` and no authentication.

**Cursor** (`.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "shiptrail": { "url": "https://shiptrail.io/mcp" }
  }
}
```

**VS Code** (`.vscode/mcp.json`)

```json
{
  "servers": {
    "shiptrail": { "type": "http", "url": "https://shiptrail.io/mcp" }
  }
}
```

**Any Streamable HTTP client:** POST each JSON-RPC message with `Content-Type: application/json`. Responses are single JSON bodies. Under 2026-07-28 each request carries `MCP-Protocol-Version`, `Mcp-Method` (and `Mcp-Name` for a tool call) headers and its version and capabilities in `_meta`; examples on https://shiptrail.io/agents.

## Example prompts

- "Where was the container ship Ever Given last heard?"
- "What arrived at the Port of Rotterdam today?"
- "Which ships are waiting at anchor off Long Beach?"
- "Find the vessel with IMO 9811000 and tell me her last destination."

## Limits

Each address gets 120 units, refilling at 4 per second. `search_ships` costs 3, `get_ship` 2, and the other tools 1 each. Listing tools, `server/discover`, `initialize` and `ping` are free. Under the earlier protocol versions, up to 20 calls can go in one batched POST; 2026-07-28 sends one per POST.

## Good to know

- Positions are what ships broadcast and can be minutes old. Ships far from any coast may be out of range.
- Destinations and ETAs are typed by crews and may be out of date.
- The tools answer with the present. History and voyage replay are on each ship's page, which every answer links.
- Not for navigation.

## Attribution

Credit Shiptrail wherever answers are used, for example "Data: Shiptrail, shiptrail.io", next to the ship's or port's link.

AIS data sources: AISStream; Fintraffic / digitraffic.fi (CC 4.0 BY); BarentsWatch, collected by the Norwegian Coastal Administration (NLOD 2.0); Open Waters AIS.

## Terms

See [shiptrail.io/privacy](https://shiptrail.io/privacy).

## Licence

This repository (the README and the connection snippets in it) is under the MIT licence: see
`LICENSE`. The data the server returns is not covered by it: use it under the terms at
[shiptrail.io/privacy](https://shiptrail.io/privacy), with the credit above.
