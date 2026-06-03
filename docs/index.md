---
title: Augment Discovery Protocol — Discovery for AI Augments
hide:
  - toc
---

# Augment Discovery Protocol

The **Augment Discovery Protocol (ADP)** is an open standard for discovering **augments** — the tools, Skills, MCP servers, APIs, workflows, and agents that extend an AI client. It pins down how an augment describes itself and how a client asks *"what can help with this task?"*, so any compliant discovery service can answer for any client.

[Get started](introduction.md){ .md-button .md-button--primary } [Read the specification](spec.md){ .md-button }

---

## Why ADP?

An augment no one can find is an augment no one can use. Wiring augments in by hand works for a handful of well-known tools; it breaks down once every product, team, and vendor is publishing its own. Even with basic search, stuffing thousands of schemas into the context window degrades both accuracy and latency. ADP moves discovery out of the prompt and into a service the client asks — the step that turned a web of bookmarks into web search.

### What an AI client should be able to do
1.  **Global access** — reach the world's growing catalog of augments, not just what was wired up in advance.
2.  **Zero prompt bloat** — keep the active toolset minimal so the context window stays clear.
3.  **Discovery at scale** — find the exact augment for a task out of millions.
4.  **Decentralized trust** — establish cryptographic trust in authors and sources.
5.  **Runtime use** — discover, connect to, and invoke augments dynamically, at the moment of need.

### What ADP enables
*   **🔌 Composable discovery services** — stand up a discovery service over any kind of augment (MCP, Skills, A2A, custom APIs), public or private, and compose it with others.
*   **🌐 Self-sovereign publishing** — host a standard `ai-catalog.json` at your own domain's `/.well-known/` to advertise your augments.
*   **⚡ Cross-client portability** — let any client from any harness discover, verify, and use your augments.

---

## Quick preview

Here is how a publisher advertises an MCP server using the standard `ai-catalog.json` manifest:

```json
{
  "specVersion": "1.0",
  "host": {
    "displayName": "Acme AI",
    "identifier": "did:web:acme.com"
  },
  "entries": [
    {
      "identifier": "urn:ai:acme.com:agent:trading",
      "displayName": "Finance Trading Agent",
      "type": "application/mcp-server+json",
      "url": "https://api.acme.com/agent.json",
      "representativeQueries": [
        "analyze market trend for 2026"
      ]
    }
  ]
}
```
