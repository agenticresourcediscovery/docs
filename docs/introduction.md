# Augment Discovery Protocol

> An augment no one can find is an augment no one can use.

AI clients are no longer limited to what the model knows. They can reach external capabilities — tools, Skills, MCP servers, APIs, workflows, and other agents. We call these **augments**.

The number of augments is about to grow quickly. Some are public, some come from vendors, some are built inside a company; some are narrow tools for a single task, others are full agents or workflows that know how to get something done. That growth creates a deceptively simple problem: **how does an AI client know what is available?**

Today the answer is mostly manual. A user, developer, or IT admin has to find the augment, judge whether it is useful and trustworthy, connect it to the client, and keep that wiring current. This works when there are a handful of well-known tools. It breaks down once every product, team, vendor, and organization is publishing augments of its own. The bottleneck is no longer invocation — it is **discovery**. A client cannot use a capability it does not know exists, a user cannot ask for an augment they have never heard of, and no enterprise can expect every employee to track which internal tools, approved vendor services, and private workflows apply to each task.

This is the problem the **Augment Discovery Protocol (ADP)** solves. ADP lets a client ask one question: *what augment can help with this task?* The answer is not the result of running anything — it is a set of matching capabilities, describing what each does, who provides it, where it lives, and how to reach it. The client then invokes the augment it picks through that augment's own protocol, whether that is MCP, an API, an agent protocol, or a workflow system.

!!! note "Find, don't invoke"
    ADP finds; the augment invokes. Discovery returns which augment fits and where to reach it. Invocation happens over that augment's own protocol.

ADP is a protocol, not a product. This site specifies it. Any number of discovery services can be built on top of it — **Agent Finder** is one such service — but ADP itself is the thing that lets a published augment be found by many clients, and a client reach augments far beyond its own pre-connected set.

---

## Why discovery is different from web search

One might expect the answer to look like web search. It cannot, and the reason is worth understanding.

Web search worked because the web already had a discoverable surface. Pages linked to other pages, HTML described content in a form browsers could render for people, and HTTP gave crawlers and browsers a common way to retrieve it. Finding something on the web in 1994 still meant a bookmark you had stumbled onto or a hand-kept list someone else maintained — but the surface was there for search engines to build on, and they did. Discovery moved off the user and into a service you ask, and that is what unlocked the scale.

Augments have no equivalent surface yet. A capability is not a document to read; it is something that can act. To discover it safely and usefully, a client needs to know what it does, what tasks it is for, what inputs it expects, what permissions it may need, who provides it, how it is invoked, and whether it is appropriate for this user or organization. Without a standard way to express that, discovery falls back to manual wiring, private catalogs, ad hoc registries, and hard-coded integrations — none of which scale. ADP gives augments the surface they are missing.

---

## The next step

The discovery problem is well recognized, and the work so far comes in two kinds. Foundry's Toolbox and the various MCP registries provide ways to assemble curated sets — a vetted collection a developer can point at. ToolLLM (Qin et al., 2023) and Claude's tool-search take a different step, making selection cheaper by moving it out of the context window: retrieving the relevant augments per query rather than injecting all of them.

ADP is an **abstraction layer over both.** It pins down just two things and leaves the rest open: how an augment describes itself, and how a client asks the discovery question and reads the answer. The service that answers can be built however its provider chooses — including with the very techniques above. A curated set becomes something any client can call uniformly, an augment published once is found by many clients, and a client reaches augments far beyond its own pre-connected set.

---

## Not one catalog

The goal is not a single global catalog of every augment. Because ADP is a protocol, we expect **many discovery services**, each defined by what it indexes and how it ranks. On the public web some will compete on coverage, indexing everything they can find to maximize recall, and others on curation — a smaller, vetted index with a high bar for quality and trust. A client gets different answers depending on which service it asks, the same way different search engines return different results.

What makes this more than a matter of preference is control: **whoever runs the discovery service controls the answer set.** That is what the enterprise case turns on.

---

## Enterprise discovery

The enterprise case makes the stakes clear. A company may have hundreds or thousands of useful capabilities — internal APIs, data tools, workflow automations, support agents, compliance and engineering tools, HR and finance systems, paid vendor services — most of them useful only if a client can discover them at the right moment. But the company also needs to decide which augments are approved, which users may see them, which data they may touch, and which vendor services are allowed.

Neither manual integration nor asking every employee to browse a catalog solves this. What the company needs is a single discovery point where approved capabilities are made visible to AI clients, and nothing else is. An ADP discovery service is exactly that. Because the client discovers only through that service, discovery becomes the enforcement point for procurement, security, and compliance — the inverse of the open-web model, and deliberately so.

---

## Composition

Discovery services need not be isolated. A company may want one view that includes its private augments, selected vendor augments, and selected public ones; a public service may index many publishers; a specialized service may index a single ecosystem. ADP lets these services **compose** — an enterprise can combine private and public sources into one answer set while still controlling what its users see. An IT manager can define their service as "everything GitHub's discovery service serves, plus our internal augments" — a union of a public endpoint and a private one, queried as a single view.

!!! tip "The DNS property"
    This is a property DNS has and web search does not. DNS resolvers compose: they forward to upstream servers and merge the global namespace with local zones, so every organization runs its own resolver without leaving the shared system. Web search has no equivalent — you query one engine's index, with no standard way to say "these results plus my company's, merged." ADP inherits the DNS property, so an enterprise extends the public ecosystem rather than walling itself off from it.

---

## Publishing

For an augment to be discovered, it has to be described. A publisher describes an augment once — a single tool or skill, or a whole internal set — in a standard catalog file, hosts it on its own domain, and advertises it via a well-known URL, a `robots.txt` entry, an HTML tag, or DNS. Any ADP discovery service can then choose to index it. The catalog is published once and reachable by every service that includes it, rather than integrated separately into each client.

Publishing does not mean every client will see the augment; it means the augment is available to be discovered by services that choose to include it. That separation matters: **publishers make capabilities available, discovery services decide what to serve, and clients decide which discovery services to trust.**

---

## The basic idea

ADP answers one question: *how does an AI client find the right external capability for a task?* It does not replace MCP, APIs, Skills, workflows, or agent protocols — it sits before them, helping the client discover which capability to use and then letting that capability's own protocol handle invocation.

The shift is from manual wiring to discovery. Instead of asking "what has this client already been connected to?", the client asks "what augment can help with this task?" That is the layer ADP creates — and an augment no one can find is an augment no one can use.

[View the protocol on GitHub :material-github:](https://github.com/agentfinder-project/adp-docs){ .md-button .md-button--primary }
