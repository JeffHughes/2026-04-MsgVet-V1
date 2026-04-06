# MsgVet (msgvet.ai)

> Policy-enforced messaging platform — kid safety, adult guardrails, and corporate governance in one codebase.

**Policy Engine + Vet AI + Channel Adapters + First-Party Messaging = MsgVet**

## What is MsgVet?

MsgVet vets inbound and outbound communications with configurable rule matrices, approvals, delegation, quarantine, and audit-grade logging. Three tiers, one codebase:

| Tier | Audience | Core Value |
|------|----------|------------|
| **Kid Safe** | Families | Stop harmful content reaching kids; parent-controlled outbound |
| **Adult Guard** | Consumers | Prevent regret-sends, tone-shape, ex/drunk/public guardrails |
| **Corporate** | Organizations | Delegation, quorum approvals, compliance, audit exports |

## Technology Stack

| Layer | Technology |
|-------|-----------|
| **Backend** | .NET 9 Minimal APIs (Azure-first) |
| **Database** | Azure Cosmos DB |
| **AI** | Azure OpenAI (GPT-4o-mini + GPT-4o, two-tier) |
| **Messaging** | Azure Communication Services (Chat + Voice + SMS) |
| **iOS** | SwiftUI + Combine |
| **Android** | Jetpack Compose + Kotlin |
| **Web Console** | Angular 21 |
| **Infrastructure** | Azure Container Apps, Service Bus, Key Vault, Redis |

## Project Structure

```
MsgVet/
├── docs/                  # HTML + MD documentation
│   ├── index.html         # Documentation home
│   ├── PRD.html           # Product Requirements (HTML)
│   ├── PRD.md             # Product Requirements (Markdown)
│   └── architecture.html  # System architecture
├── context/               # Living context docs
│   └── MV-CONTEXT.md      # Architecture decisions
├── rules/                 # Hard rules and conventions
│   └── hard-rules.md      # Non-negotiable constraints
├── .factory/              # Agent configuration
│   └── AGENTS.md          # Droid agent setup
└── README.md              # This file
```

## Build Order (24-week MVP)

1. **Milestone A (Weeks 1-8):** Kid-first foundation — family accounts, inbound/outbound filtering, quarantine, parent dashboard, first-party chat
2. **Milestone B (Weeks 9-14):** Adult consumer — Drunk/Ex Guard, regret controls, intent capture, scheduling, VoIP
3. **Milestone C (Weeks 15-20):** Corporate — delegation, quorum approvals, audit exports, admin console
4. **Milestone D (Weeks 21-24):** Hardening — load testing, security audit, COPPA review, app store submissions, beta

## Documentation

- [Home](docs/index.html) — Overview and key metrics
- [PRD](docs/PRD.html) — Full product requirements document
- [Architecture](docs/architecture.html) — System design and data flow

## Status

**Pre-development** — PRD v1.0 complete, architecture defined, ready for implementation.
