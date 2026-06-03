# Anthropic's MCP Registry

ADP acts as an open, protocol-agnostic discovery envelope designed to ingest, wrap, and index the **Model Context Protocol (MCP)** tool ecosystem.

---

## The envelope mapping

The Model Context Protocol (MCP) standardizes model-to-data/tool connections. ADP wraps MCP tool server endpoints under the standard `application/mcp-server+json` media type:

```json
{
  "identifier": "urn:ai:acme.com:server:weather",
  "displayName": "Acme Weather Telemetry Server",
  "type": "application/mcp-server+json",
  "url": "https://api.acme.com/mcp/weather.json",
  "capabilities": ["WeatherTool", "ForecastTool"],
  "representativeQueries": [
    "what is the current wind speed in Chicago",
    "get the 5-day forecast for Seattle"
  ]
}
```

---

## Developer value

*   **Federated scaling**: Developers of private or enterprise MCP tools can bypass the centralized `modelcontextprotocol.io` registry altogether and host their servers sovereignly on their own FQDNs.
*   **Zero prompt bloat**: By indexing `representativeQueries` outside the prompt, clients (like Claude Code) can run a semantic search against a discovery service first, feeding the model only the top matching augment schemas dynamically at runtime.
