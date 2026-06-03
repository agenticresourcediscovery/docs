# Comparisons

ADP is designed as a **superset** of existing agent and tool discovery approaches. It is not a replacement for other registries; rather, it acts as a federated overlay that unites them.

**Q. Why not X?**
**A. Why not both?**

*   **Universal envelope**: Anything can get an AI Catalog "card" — an MCP server, a Skill, an ACP agent, an A2A agent, a traditional REST API, or whatever comes next.
*   **Federated indexing**: Once a card is published, it can be indexed by any ADP discovery service. You don't have to choose between registries; a discovery service can consume all of them, or you can curate your own subset.

---

## What about...

### What about the MCP Registry

The entire list of public MCP servers can be indexed natively. The official MCP Registry (`registry.modelcontextprotocol.io`) can remain exactly as it is today while also acting as a federated source of truth that is crawled and made queryable through ADP `POST /search` endpoints. Nothing about the existing registry has to change — ADP simply makes its contents reachable, alongside everything else, through a uniform discovery question.

---

### What about the ACP Agent Registry

The list of ACP agents in [ACP's Agent Registry](https://agentclientprotocol.com/get-started/registry) is already structurally close to the AI Catalog specification. ACP registries can export their directory manifests as standard `ai-catalog.json` feeds, enabling instant web-scale discovery for editor-context agents — without those agents having to be re-registered anywhere.

---

### What about Open Plugins

A plugin under the [Open Plugins](https://open-plugins.com/) standard is essentially a bundle of MCP servers, Skills, or other tools. By describing these bundles inside the standard AI Catalog specification, Open Plugins can leverage ADP's federated infrastructure for instant decentralized deployment, crawling, and semantic indexing. See [Open Plugins](open_plugins.md) for the envelope mapping.
