# Frequently Asked Questions (FAQ)

## Is ARDP a replacement for MCP or OpenAPI?

**No.** ARDP is a **discovery protocol (an envelope)**, not an execution protocol. It wraps existing execution standards (like MCP, A2A, and OpenAPI) using standard and proposed IANA media types so clients can find agentic resources dynamically. Once discovered, the client connects to and invokes the agentic resource using its native protocol (e.g., JSON-RPC for MCP).

---

## How does discovery work without consuming context window tokens?

Traditional tool selection requires stuffing every available schema into the system prompt. ARDP moves this calculation outside the LLM into a dedicated discovery service (`POST /search`). The orchestrator queries the service with natural language, and it returns only the top two or three most relevant schemas to inject into the prompt.

---

## Do I need to register my agentic resources on a central directory?

**No.** You have absolute publishing sovereignty. You host `ai-catalog.json` on your own domain (`yourdomain.com/.well-known/ai-catalog.json`) to advertise your agentic resources. Any compliant discovery service can find and index your endpoint organically, without requiring permission.

---

## What is the difference between ARDP and Agent Finder?

ARDP is the **protocol**. **Agent Finder** is a product — one discovery service built on ARDP, among the many the protocol makes possible. A publisher describes an agentic resource once; any number of discovery services, named or not, can then choose to index and serve it.

---

## Where does trust come from in ARDP?

**From the curation of the registry, not from the protocol.** ARDP does not make any agentic resource trustworthy — it gives publishers a way to *assert* verifiable identity and provenance, and clients a way to *verify* it. The actual trust decision lives in two places the protocol does not own: the **registry that curates what it indexes and serves**, and the **client that checks the signals before invoking**. ARDP's job is to communicate trust, not to confer it.

---

## What trust signals can ARDP communicate?

The protocol's role here is narrow and deliberate: it gives a publisher a **mechanism to assert verifiable claims**, and a registry or client a **standard way to check them**. ARDP itself verifies nothing and vouches for no one. An entry can carry:

- **Domain-anchored identity** — a mechanism to declare a publisher domain in the entry's URN (`urn:ai:acme.com:...`), so identity is rooted in DNS rather than a self-asserted label and can be checked by whoever consumes the entry.
- **Verified publishers** — a mechanism for a publisher to *demonstrate* they are who they claim: a `trustManifest.identity` (e.g. `did:web`, SPIFFE) that a registry or client cryptographically verifies against the publisher's domain. The protocol carries the claim; the registry or client performs the verification.
- **Signed metadata** — a mechanism to attach a detached JWS `signature` over the trust manifest, so a client can confirm the record was not altered in transit or by an intermediary.
- **Provenance** — a mechanism to declare lineage (`derivedFrom`, `publishedFrom`) that records where a resource came from.
- **Attestations** — a mechanism to reference verifiable compliance artifacts (SOC2, HIPAA, GDPR, …) so they can be fetched and checked.

In every case ARDP *enables the communication* of a trust signal. Whether the signal checks out, and what weight to give it, is the registry's and the client's decision — not the protocol's.

---

## What makes a registry trustworthy?

A registry is a **trust boundary defined by its curation policy** — what it chooses to index, whom it verifies, and what it refuses to serve. A curated registry vouches for what it returns; an enterprise registry serves only internally approved resources; a vendor registry exposes one ecosystem. Clients decide which registries to query, and registries compose, so an organization can merge its own curated answer set with an upstream one. The protocol carries the evidence; the registry supplies the judgment.

---

## Does ARDP verify that a tool or agent is safe?

**No — and it does not claim to.** Safety is not a property a discovery protocol can compute. What ARDP does is let the relevant facts travel so others can judge. It enables a publisher to **communicate a verifiable identity** (a domain-anchored URN), to **show they are a verified publisher** (a `trustManifest.identity` such as `did:web` or SPIFFE that a registry or client checks against their domain), to ship **signed metadata** (a detached JWS signature proving the record is unaltered), and to declare **provenance** for where the resource came from. The safety call is then made by the **registry that curated the result** and the **client that verifies these signals** before invoking — not by the protocol. The relevance `score` returned by a search is *relevance only* and **MUST NOT** be read as a trust, compliance, or safety rating.

---

## Could ARDP make it easier for agents to discover malicious tools?

Discovery is only as permissive as the registry you ask. Because **whoever runs the registry controls the answer set**, a curated or enterprise registry returns only resources it has vetted — a bad actor cannot inject an entry into a registry that did not choose to index it.

ARDP also makes impersonation harder than the status quo, on several concrete fronts:

- **Identity** is anchored to a publisher's domain (`urn:ai:google.com:...`), not a self-asserted label.
- **Verified publishers**: a registry MUST verify that a manifest is actually hosted on — or cryptographically bound (via `did:web`/SPIFFE) to — the domain it claims, so a manifest on `untrusted.com` cannot pose as `urn:ai:google.com:...`.
- **Signed metadata** lets a client confirm a record was not altered in transit, and **provenance** lets it trace where a resource came from.

Compared with today's ad hoc wiring — copy-pasted endpoints with no identity or provenance — ARDP raises the floor by giving every result a verifiable origin. It does not, on its own, decide what is safe; it **communicates the identity, verification, and provenance signals** that let curated registries and clients refuse what is not.
