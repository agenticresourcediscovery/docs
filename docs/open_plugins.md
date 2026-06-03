# Open-Plugins Ecosystem

ADP acts as an open, protocol-agnostic discovery envelope designed to ingest, wrap, and index the community **Open Plugins** standard.

---

## The envelope mapping

Open Plugins describe bundles of MCP servers, Skills, or REST tools. ADP wraps these plugin definitions natively inside the `application/ai-catalog+json` nested catalog envelope:

```json
{
  "identifier": "urn:ai:acme.com:plugin:finance-suite",
  "displayName": "Finance Tool Bundle",
  "type": "application/ai-catalog+json",
  "data": {
    "specVersion": "1.0",
    "entries": [
      {
        "identifier": "urn:ai:acme.com:finance:mcp",
        "displayName": "Finance Trading MCP",
        "type": "application/mcp-server+json",
        "url": "https://api.acme.com/mcp/finance.json"
      }
    ]
  }
}
```

---

## Developer value

*   **Federated discoverability**: Open Plugins inherit ADP's decentralized domain-anchored indexing without requiring manual registration in centralized directories.
*   **Cryptographic trust**: Plugin bundles inherit `trustManifest` attestation and signature verification, guaranteeing tamper-proof deployment across federated networks.
