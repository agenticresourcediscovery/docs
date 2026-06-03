# AI Catalog Standard

**The foundation for federated augment discovery**

ADP is built directly on top of the **[ai-catalog](https://github.com/Agent-Card/ai-catalog)** specification. The `ai-catalog` standard establishes the base artifact-agnostic data model, progressive trust layer, and verification rules that enable dynamic runtime augment discovery.

---

## Standard manifest location

A host advertises its augments by publishing a well-known static JSON manifest at:

```http
https://<publisher-domain>/.well-known/ai-catalog.json
```

This allows secure, decentralized, and cacheable ingestion by crawlers and discovery services worldwide.

---

## Core envelope schema

A baseline `ai-catalog.json` manifest includes the following root elements:

* **`specVersion`**: The version of the catalog format (e.g., `"1.0"`).
* **`host`**: Information about the entity operating the catalog (display name, DID/domain identifier).
* **`entries`**: An array of individual augment card definitions.
* **`collections`**: Links to sub-catalogs or related departmental feeds.

### Example baseline entry

```json
{
  "specVersion": "1.0",
  "host": {
    "displayName": "Acme Systems",
    "identifier": "acme.com"
  },
  "entries": [
    {
      "identifier": "urn:ai:acme.com:tool:ocr",
      "displayName": "OCR Text Extractor",
      "type": "application/mcp-server+json",
      "url": "https://tools.acme.com/ocr/mcp.json",
      "description": "Extracts plain text and structured tables from scanned PDFs and PNGs."
    }
  ]
}
```

---

## Progressive trust & compliance

The `ai-catalog` standard decouples complex security and compliance data from lightweight metadata, using a `trustManifest` object within each entry. This facilitates:

1. **Workload identity**: Binding augments to SPIFFE IDs, decentralized identifiers (DIDs), or standard HTTPS domains.
2. **Compliance attestations**: Standardizing links to SOC2, HIPAA, GDPR, or ISO audits.
3. **Provenance links**: Recording lineage such as derived relationships (`derivedFrom`, `publishedFrom`) to enable supply-chain tracking.
4. **Cryptographic signatures**: Verifiable detached JWS signatures computed over the trust metadata to prevent tampering.
