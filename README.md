# Machine Telemetry - Reference Architecture 

Architecture and data modeling reference documents for Machine Telemtry platform.

## Documents

| File | Description |
|------|-------------|
| [Architecture-Diagrams-L1-L2.html](Architecture-Diagrams-L1-L2.html) | L1 System Context + L2 Container diagrams for the Machine Telemetry Platform. Includes MDM integration pattern, phased Ontology build (Approaches 3 & 4), and multi-year roadmap. |
| [Palantir-Foundry-Telemetry-Alignment.html](Palantir-Foundry-Telemetry-Alignment.html) | Telemetry domain alignment to Palantir Foundry — Bronze/Silver/Gold dataset paths, enrichment strategy, pipeline scheduling, and data ownership by team. |
| [Palantir-Ontology.html](Palantir-Ontology.html) | Full Foundry Ontology definition — 11 object types, 19 link types, 12 action types, property type registry, IAM access matrix, and SVG visual diagram. |
| [NBO-NBA-Data-Model.html](NBO-NBA-Data-Model.html) | Next Best Offer / Next Best Action data model — urgency scoring formula, suppression rules, ranking logic, NBO/NBA by domain, sample JSON payloads, and closed-loop feedback. |
| [Palantir-vs-Snowflake.html](Palantir-vs-Snowflake.html) | Palantir Foundry vs. Snowflake architecture comparison — feature-by-feature, decision matrix, and hybrid architecture pattern. |

## Platform Architecture Overview

**Machine Telemetry Platform** — migrates legacy machine telemetry to Palantir Foundry and drives operational NBO/NBA decisions through a Customer Portal and Commerce Platform integration.

```
Machine Fleet (Kafka) → Magritte → Bronze → Silver → Gold → Ontology → Machine Domain APIs
MDM System (External)  ──────────────────────────────────────────────────────────────────↑
                                                                   ↓
                                                    NBO/NBA Engine → Customer Portal → Commerce Platform
```

### Key Design Decisions
- **MDM as external golden record**: Machine/Customer/Dealer/Part master data synced daily via Magritte. MDM keys are canonical IDs in the Ontology.
- **Incremental Ontology build (Approach 3)**: Design full Ontology on paper in Year 1; populate Machine + Alert only. Add objects phase by phase.
- **Event-sourced path (Approach 4)**: Engine + DPF domains bypass batch Gold in Year 3 — Kafka → Foundry AIP Streaming → Ontology direct write (~30 sec latency).
- **Urgency score formula**: `(wear% × 0.35) + (fault_severity × 0.30) + (hours_remaining × 0.20) + (health_inverse × 0.10) + (historical_adj × 0.05)`

---
*Prepared by [Harsh Shah](https://harshshah85.github.io/about-me/) — May 2026*
