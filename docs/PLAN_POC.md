# Junction — Proof of Concept Plan

## Overview

**Junction** is an open platform designed to connect public sector services, data sources, and stakeholders — serving as the integration layer ("junction point") between civic systems that today operate in silos.

This document outlines a focused proof of concept (PoC) to validate the core premise: that a lightweight, open-source integration hub can meaningfully reduce friction between public works systems while remaining simple enough for civic developers to adopt and extend.

---

## Problem Statement

Public works agencies and civic organizations manage a wide variety of data and services — permits, asset registries, service requests, infrastructure monitoring — that rarely communicate with each other. The result is:

- Manual data reconciliation across systems
- Delayed response to service needs
- Barriers to civic tech innovation due to fragmented APIs and data formats
- High cost of custom one-off integrations

Junction aims to be the connective tissue: a composable, AI-augmented integration hub for public infrastructure data.

---

## PoC Goals

The proof of concept should demonstrate one narrow but complete slice of value:

> **"A user can connect two public-facing civic data sources, define a simple data mapping, and receive a unified, queryable endpoint — without writing custom integration code."**

### Success Criteria

- [ ] At least two real-world civic data sources can be registered and connected
- [ ] A non-engineer can configure a basic mapping via a simple interface (config file or minimal UI)
- [ ] A unified API endpoint serves merged/joined data from both sources
- [ ] The system handles schema mismatches gracefully
- [ ] End-to-end setup takes less than 30 minutes for a developer familiar with the stack

---

## Scope

### In Scope

- Core connector framework (2 adapters minimum)
- Simple schema mapping DSL or config format
- Unified REST API output
- Basic authentication for the API endpoint
- Developer documentation for adding a new connector

### Out of Scope (Post-PoC)

- Production-grade scalability
- Full UI / admin dashboard
- Real-time streaming / webhooks
- Multi-tenancy
- Compliance and data governance tooling

---

## Proposed Architecture

```
┌─────────────────────────────────────────────────────────┐
│                        Junction                          │
│                                                          │
│  ┌──────────┐    ┌──────────────┐    ┌───────────────┐  │
│  │ Source A │───▶│   Connector  │───▶│               │  │
│  │ (Civic   │    │   Layer      │    │  Mapping &    │  │
│  │  API)    │    │              │    │  Transform    │──▶ Unified API
│  └──────────┘    └──────────────┘    │  Engine       │  │
│                                      │               │  │
│  ┌──────────┐    ┌──────────────┐    └───────────────┘  │
│  │ Source B │───▶│   Connector  │                        │
│  │ (Open    │    │   Layer      │                        │
│  │  Dataset)│    │              │                        │
│  └──────────┘    └──────────────┘                        │
└─────────────────────────────────────────────────────────┘
```

### Components

| Component | Description |
|-----------|-------------|
| **Connector** | Adapter for a specific data source (REST API, CSV, database) |
| **Mapping Engine** | Declarative field mappings between source schemas |
| **Unified API** | Output endpoint serving normalized, joined data |
| **Config Layer** | YAML/JSON config to define sources, mappings, and output |

---

## Candidate Data Sources for PoC

To keep the PoC concrete, we propose using two publicly available civic datasets:

1. **311 Service Requests** — Open311 GeoReport v2 (available from many US/EU cities)
2. **Public Asset Registry** — GeoJSON asset catalog (e.g., streetlights, parks, infrastructure)

These two sources have overlapping geographic keys (lat/lng, address) but different schemas — a realistic integration challenge.

---

## Technology Choices (Proposed)

| Concern | Proposal | Rationale |
|---------|----------|-----------|
| Language | TypeScript (Node.js) | Broad civic dev adoption, good ecosystem |
| Config format | YAML | Human-readable, widely understood |
| API framework | Hono or Fastify | Lightweight, fast |
| Data fetching | Fetch API + adapters | Simple, no heavy ORM needed for PoC |
| Testing | Vitest | Fast, TS-native |
| CI | GitHub Actions | Already in use |
| AI assistance | Claude Code (via existing workflows) | Already integrated |

These are proposals — the tech stack should be confirmed with the team before implementation begins.

---

## Milestones

### Milestone 1 — Foundation (Week 1–2)
- [ ] Define connector interface / contract
- [ ] Implement Source A connector (311 API)
- [ ] Implement Source B connector (Asset Registry)
- [ ] Basic CLI to fetch and print raw data from both sources

### Milestone 2 — Mapping & Transform (Week 3)
- [ ] Design YAML mapping schema
- [ ] Implement mapping engine (field rename, type coerce, join on shared key)
- [ ] Unit tests for mapping logic

### Milestone 3 — Unified API (Week 4)
- [ ] Stand up REST API server
- [ ] Wire connector output through mapping engine to API response
- [ ] Add basic API key auth
- [ ] Integration test: end-to-end query returns unified data

### Milestone 4 — Documentation & Demo (Week 5)
- [ ] Write "Add a connector" developer guide
- [ ] Write "Configure a mapping" guide
- [ ] Record or write a demo walkthrough
- [ ] Collect feedback from at least one external civic dev

---

## Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| Civic data source APIs are unreliable or rate-limited | Medium | Medium | Use cached/static snapshots for PoC testing |
| Schema mapping complexity explodes | Medium | High | Limit PoC to flat (non-nested) schemas |
| Lack of clarity on project goals | High | High | Use this PoC to validate direction, not commit to it |
| Connector abstraction is too rigid | Low | Medium | Keep connector interface thin and iterative |

---

## Open Questions

- What is the primary user persona for Junction — civic developer, government IT staff, data analyst?
- Should Junction be purely developer-tooling, or does it need an eventual UI for non-technical configurators?
- Is real-time data (streaming/webhooks) a near-term requirement, or is periodic pull sufficient?
- Are there existing open-source tools (e.g., Apache Camel, Airbyte, dbt) we should evaluate before building from scratch?
- What licensing model best fits the public works / open civic mission?

---

## Next Steps

1. Review and discuss this plan with the team
2. Confirm or revise the technology stack
3. Answer the open questions above
4. Create GitHub issues for Milestone 1 tasks
5. Begin implementation

---

*This document is a living plan. Update it as the project evolves and open questions are resolved.*
