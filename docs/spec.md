# ADP Specification

The Augment Discovery Protocol pins down two things and leaves the rest open: **how an augment describes itself**, and **how a client asks the discovery question and reads the answer.** Everything else — how a discovery service crawls, ranks, or composes — is an implementation choice.

---

## The two halves of the protocol

### 1. Description — the AI Catalog

A publisher describes its augments in a static `ai-catalog.json` manifest, hosted at a well-known location on its own domain. This is the [AI Catalog standard](ai_catalog_spec.md): an artifact-agnostic envelope that wraps any augment type (MCP server, A2A card, Skill, REST API) using standard and proposed IANA media types, with a progressive trust layer for identity, attestations, and signatures.

```http
https://<publisher-domain>/.well-known/ai-catalog.json
```

### 2. Query — the search contract

A client asks a discovery service the discovery question over a standard `POST /search` contract and reads back a ranked list of matching augments, each with its type, endpoint URL, and relevance score. See [How clients discover](how_agents_search.md) for the request/response shapes and the mandatory trust-verification steps.

---

## Conformance at a glance

| Role | MUST |
| :--- | :--- |
| **Publisher** | Host a valid `ai-catalog.json`; use domain-anchored URN identifiers; serve over HTTPS with permissive CORS. |
| **Discovery service** | Verify domain ownership before indexing; expose `POST /search`; never present `score` as a trust or safety rating. |
| **Client** | Independently verify publisher trust before invoking; invoke each augment over its own native protocol. |

---

## Full specification

The complete normative specification — schema definitions, media-type registrations, trust manifest rules, and federation semantics — lives in the protocol repository.

[View the specification on GitHub :material-github:](https://github.com/agentfinder-project/adp-docs){ .md-button .md-button--primary }
