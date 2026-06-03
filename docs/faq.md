# Frequently Asked Questions (FAQ)

## Is ADP a replacement for MCP or OpenAPI?

**No.** ADP is a **discovery protocol (an envelope)**, not an execution protocol. It wraps existing execution standards (like MCP, A2A, and OpenAPI) using standard and proposed IANA media types so clients can find augments dynamically. Once discovered, the client connects to the augment using its native protocol (e.g., JSON-RPC for MCP). ADP finds; the augment invokes.

---

## How does discovery work without consuming context window tokens?

Traditional tool selection requires stuffing every available schema into the system prompt. ADP moves this calculation outside the LLM into a dedicated discovery service (`POST /search`). The orchestrator queries the service with natural language, and it returns only the top two or three most relevant schemas to inject into the prompt.

---

## Do I need to register my augments on a central directory?

**No.** You have absolute publishing sovereignty. You host `ai-catalog.json` on your own domain (`yourdomain.com/.well-known/ai-catalog.json`) to advertise your augments. Any compliant discovery service can find and index your endpoint organically, without requiring permission.

---

## What is the difference between ADP and Agent Finder?

ADP is the **protocol**. **Agent Finder** is a product — one discovery service built on ADP, among the many the protocol makes possible. A publisher describes an augment once; any number of discovery services, named or not, can then choose to index and serve it.
