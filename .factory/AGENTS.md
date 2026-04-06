<coding_guidelines>
# MsgVet Agent Configuration

## Project Memory
Architecture decisions, domain knowledge, and project history are documented in `context/MV-CONTEXT.md`.
Always check this file before making significant architectural or design decisions.

## Hard Rules
All coding invariants and constraints are in `rules/hard-rules.md`. These are non-negotiable.

## Reference Project
MsgVet follows patterns from InfiniteCabinet (D:\software\InfiniteCabinet):
- .NET 9 Minimal APIs for backend
- Azure-first cloud services
- NX monorepo structure (when web console is added)
- Dark-themed HTML documentation in docs/

## Key Conventions
- Backend: .NET 9 Minimal APIs, Cosmos DB with tenantId partition key
- Mobile: SwiftUI (iOS), Jetpack Compose (Android)
- Web: Angular 21 (following InfiniteCabinet patterns)
- AI: Two-tier LLM (cheap + strong) with deterministic rules first
- All APIs: cursor pagination, REST + WebSocket
- Secrets: Azure Key Vault only, never in code or DB
</coding_guidelines>
