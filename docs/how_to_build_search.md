# How to Build a Discovery Service

A discovery service is an active service that crawls static manifests, indexes augments semantically, and exposes the standard `POST /search` endpoint to clients.

Building your own discovery service is entirely optional. If you only want to publish augments, you only need to host a static manifest (see [How to publish](how_to_publish.md)). To query augments, your client can connect to any existing public or private discovery service (see [Implementations](implementations.md) for active endpoints).

---

## Step 1: The ingestion & crawling pipeline

The service populates its database by crawling the web for static `ai-catalog.json` manifests.

1.  **Crawling loop**: Regularly scan known domains, fetch `https://<domain>/.well-known/ai-catalog.json`, and parse the `entries` array.
2.  **Domain-ownership security**: Before indexing an entry, you MUST verify domain authority to prevent spoofing:
    *   Extract the domain root from the logical URN identifier (e.g., `urn:ai:google.com:tax-agent` ➜ `google.com`).
    *   Verify that the FQDN hosting the manifest matches this domain, OR cryptographically verify the `trustManifest.identity` using `did:web` rules.
    *   **Rule**: If `untrusted.com` publishes a manifest containing an entry for `urn:ai:google.com:tax-agent`, the service MUST reject it.

---

## Step 2: The semantic vector index

To enable semantic natural language search, the service must index augments using vector embeddings.

```
[ai-catalog.json] ──> [Embed representativeQueries] ──> [Store in Vector DB]
                                                               │
                                                               ▼
[POST /search Query] ──> [Embed Query Text] ──> [Cosine Similarity Search]
```

### 1. Indexing (write path)
1.  For each ingested entry, extract the `representativeQueries` array and the `description`.
2.  Pass these text strings through a standard embedding model (e.g., `text-embedding-004`) to generate dense vector representations.
3.  Store the resulting vectors in a vector database (such as `pgvector`, `Qdrant`, or `Pinecone`) mapped to the augment's URN `identifier` and endpoint `url`.

### 2. Searching (read path)
1.  When a client calls `POST /search` with a natural-language `text` query:
    *   Generate an embedding vector for the incoming query string.
    *   Perform a **cosine similarity search** in the vector database to find the closest matching augment embeddings.
2.  Convert the mathematical similarity distance to the standard **`score` (0–100)**.
3.  Return the ranked results array.

---

## Step 3: Executing federation (`auto` mode)

If a client requests `federation: auto` in their search query, the service acts as a federated coordinator:

1.  **Local query**: Execute the similarity search against the service's local database.
2.  **Upstream relay**: In parallel, forward the exact `POST /search` request to other discovery services listed in the local referral index.
3.  **Merge & re-rank**:
    *   Collect all result sets.
    *   De-duplicate entries by their primary key URN `identifier` (giving preference to the highest score or verified signature).
    *   Re-sort the merged list by `score` and return the unified results to the client.

This is what gives ADP its [DNS-like composition property](introduction.md): services forward to upstream services and merge results, so an enterprise can run its own while still participating in the larger shared ecosystem.
