# Zed's ACP Agent Registry

ADP acts as an open, protocol-agnostic discovery envelope designed to ingest, wrap, and index **Zed's Agent Client Protocol (ACP)** capabilities.

---

## The envelope mapping

Zed's ACP standardizes editor-to-agent UI integrations. ADP wraps ACP endpoints under the proposed `application/acp-agent+json` media type:

```json
{
  "identifier": "urn:ai:acme.com:editor:refactor",
  "displayName": "Acme Workspace Refactoring Agent",
  "type": "application/acp-agent+json",
  "url": "https://api.acme.com/acp/endpoint"
}
```

---

## Developer value

*   **Cross-harness discoverability**: ACP-compliant agents gain permissionless discoverability outside of the Zed editor ecosystem, making them searchable by any harness (e.g., Gemini, Claude Code).
*   **Zero prompt bloat**: Editor agents are dynamically discovered and loaded into the client's environment only when a workspace refactoring task matches the user's active intent.
