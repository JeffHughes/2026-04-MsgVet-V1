# MsgVet — Living Context Document

## Project Identity
- **Name:** Message Vet (msgvet.ai)
- **Type:** Policy-enforced messaging platform
- **Tiers:** Kid Safe, Adult Guard, Corporate Governance
- **Status:** PRD v1.0 complete, pre-development

## Architecture Decisions

### AD-001: Azure-first cloud strategy
- **Decision:** All infrastructure on Azure (Cosmos DB, Service Bus, ACS, OpenAI, Container Apps)
- **Rationale:** Consistent ecosystem, ACS provides chat/voice/SMS in one platform, Azure OpenAI for AI
- **Date:** 2026-04-06

### AD-002: .NET 9 Minimal APIs for backend
- **Decision:** Follow InfiniteCabinet pattern with .NET 9
- **Rationale:** High performance, Azure-native, team familiarity from IC project
- **Date:** 2026-04-06

### AD-003: Two-tier LLM strategy
- **Decision:** Cheap model (GPT-4o-mini) for 80% of classification, strong model (GPT-4o) for edge cases
- **Rationale:** Cost target <$0.15 per 1K vetted messages requires aggressive routing
- **Date:** 2026-04-06

### AD-004: First-party messaging as primary channel
- **Decision:** MsgVet Chat/Voice via ACS is the primary channel; external adapters are secondary
- **Rationale:** iOS SMS interception impossible, consistent enforcement, lower per-message cost
- **Date:** 2026-04-06

### AD-005: Kid-first build order
- **Decision:** Build kid safety (Milestone A) before adult/corporate features
- **Rationale:** Highest impact, proves core vet pipeline, establishes COPPA compliance early
- **Date:** 2026-04-06

## Open Items
- Technology choice for web console (Angular vs React)
- E2E encryption scope for MVP
- Android default SMS handler timing
- Multi-language support timeline
