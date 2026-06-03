# How Clients Discover

This guide outlines how AI client orchestrators (such as Claude Code, Gemini, Copilot) query ADP discovery services at runtime to find and load augments.

---

## Step 1: The search request (`POST /search`)

When a user asks for a capability that is not pre-installed, the orchestrator queries a discovery service using the standard `POST /search` contract:

```json
{
  "query": {
    "text": "book me a flight to Tokyo",
    "type": "application/mcp-server+json",
    "federation": "referrals"
  },
  "pageSize": 3
}
```

*   **`text`**: Natural language intent extracted from the user query.
*   **`type`**: Filter by specific proposed media types (e.g., `application/mcp-server+json`).
*   **`federation`**: Set to `auto` (the service merges upstream results) or `referrals` (the service returns alternative discovery-service URLs for the client to follow).

---

## Step 2: The search response

The discovery service returns a ranked list of catalog entries satisfying the semantic criteria:

```json
{
  "results": [
    {
      "identifier": "urn:ai:acme.com:travel:concierge",
      "displayName": "Travel Concierge",
      "type": "application/mcp-server+json",
      "url": "https://api.acme.com/mcp/travel.json",
      "score": 95
    }
  ],
  "referrals": [
    {
      "identifier": "urn:ai:finder.org:registry",
      "type": "application/ai-registry",
      "url": "https://finder.org/search"
    }
  ]
}
```

!!! warning "Important: score interpretation"
    The `score` parameter (0–100) is strictly an informational relevance metric computed semantically by the discovery service. Runtimes MUST NOT interpret this as a cryptographic trust, security compliance, or safety rating. Trust verification must be executed independently.

---

## Step 3: Client-side trust verification

Before connecting to or invoking any discovered augment, the client orchestrator MUST verify the publisher's credentials:

1.  **Extract domain**: Parse the logical FQDN authority domain embedded in the URN identifier (e.g., `urn:ai:acme.com:travel:...` ➜ `acme.com`).
2.  **Verify identity**: Fetch the manifest and cross-reference the `trustManifest.identity` (e.g., SPIFFE ID or `did:web`) to confirm the publisher controls the claimed FQDN.
3.  **Audit compliance**: Parse the `attestations` array inside `trustManifest` to verify SOC2, HIPAA, or GDPR certificate links.
4.  **Verify signature**: Verify the detached JWS cryptographic signature block computed over the trust metadata to ensure no tampering has occurred in transit.

Discovery returns *which* augment fits and *where* to reach it; invocation then happens over that augment's own protocol.
