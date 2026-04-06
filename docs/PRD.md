# Message Vet (msgvet.ai) — Product Requirements Document

**Version:** 1.0.0  
**Date:** April 6, 2026  
**Status:** Draft v1.0 (Functional)  
**Build order:** Kid → Adult consumer → Corporate (same MVP release train; features gated by roles/plans)

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Problem Statement](#2-problem-statement)
3. [Goals / Non-goals / Success](#3-goals--non-goals--success)
4. [Personas & User Journeys](#4-personas--user-journeys)
5. [Core Concepts (Platform Primitives)](#5-core-concepts-platform-primitives)
6. [Product Surfaces](#6-product-surfaces)
7. [Authentication, Roles & Tenancy](#7-authentication-roles--tenancy)
8. [Kid-First: Family Safety System](#8-kid-first-family-safety-system)
9. [Adult Consumer: Guardrails & Intent Shaping](#9-adult-consumer-guardrails--intent-shaping)
10. [Corporate: Governance, Approvals & Audit](#10-corporate-governance-approvals--audit)
11. [Quarantine System](#11-quarantine-system)
12. [Policy Engine & Rule Matrix](#12-policy-engine--rule-matrix)
13. [Sensitive Groups & Alias System](#13-sensitive-groups--alias-system)
14. [Vetting Engine & Visualization](#14-vetting-engine--visualization)
15. [Approval Workflows](#15-approval-workflows)
16. [Messaging Architecture](#16-messaging-architecture)
17. [Alias Phone Numbers & Proxy Routing](#17-alias-phone-numbers--proxy-routing)
18. [Scheduling & Delivery Assurance](#18-scheduling--delivery-assurance)
19. [Logging, Auditing & Privacy](#19-logging-auditing--privacy)
20. [API Design & Pagination](#20-api-design--pagination)
21. [Billing & Usage Metering](#21-billing--usage-metering)
22. [LLM Strategy & Cost Optimization](#22-llm-strategy--cost-optimization)
23. [System Architecture](#23-system-architecture)
24. [Technology Stack](#24-technology-stack)
25. [Data Model](#25-data-model)
26. [Security Architecture](#26-security-architecture)
27. [Build Order & Milestones](#27-build-order--milestones)
28. [High-Leverage Ideas](#28-high-leverage-ideas)
29. [Risks & Mitigations](#29-risks--mitigations)
30. [Open Questions](#30-open-questions)
31. [Glossary](#31-glossary)

---

## 1. Executive Summary

Message Vet is a **policy-enforced messaging platform** that vets inbound and outbound communications with configurable rule matrices, approvals, delegation, quarantine, and audit-grade logging. It operates in three tiers within one codebase:

| Tier | Audience | Core Value |
|------|----------|------------|
| **Kid Safe** | Families (under-13, teens) | Stop harmful content reaching kids; parent-controlled outbound |
| **Adult Guard** | Consumers | Prevent regret-sends, tone-shape, ex/drunk/public guardrails |
| **Corporate** | Organizations | Delegation, quorum approvals, compliance, audit exports |

### The Formula

```
Policy Engine + Vet AI + Channel Adapters + First-Party Messaging = MsgVet
```

MsgVet can:
- **Connect** to external channels (SMS/email/X/etc.) where APIs allow
- **Run first-party** chat + voice directly for consistent enforcement
- **Vet** every message (inbound and outbound) against configurable policy matrices
- **Quarantine** flagged content with structured review workflows
- **Approve/delegate** via AI gates, human gates, mixed, or quorum

---

## 2. Problem Statement

### For Kids
- Children receive harmful content (bullying, predatory, explicit) with no real-time guardrails.
- Parents lack visibility into messaging content without invasive surveillance.
- No platform offers both inbound filtering AND outbound safety in one product.

### For Adults
- Regret-sends cost relationships, jobs, and reputations daily.
- No tool prevents messages to specific contacts during high-risk states (drinking, anger, late night).
- Public posts lack pre-send review for tone, compliance, or appropriateness.

### For Organizations
- Executive communications require delegation with audit trails.
- Compliance teams need message vetting without reading every message.
- No platform combines governance + messaging + AI vetting in one system.

### The Cost of Doing Nothing
| Scenario | Estimated Impact |
|----------|-----------------|
| Kid exposed to predatory messaging | Immeasurable harm |
| Drunk text to ex | Relationship damage, possible legal issues |
| Executive sends unvetted public statement | Stock price, brand, legal |
| Employee sends privileged info externally | Regulatory fines, litigation |

---

## 3. Goals / Non-goals / Success

### Goals
1. **Stop bad messages before they go out** — tone, content, time, audience, compliance enforcement.
2. **Stop bad messages on the way in** — quarantine + parent/admin review + alerts (especially for kids).
3. **Unify communications** across accounts/channels with consistent visualization and enforcement.
4. **Support approvals + delegation** (AI, human, quorum) with a complete audit trail.
5. **Offer first-party chat + voice** to bypass external channel constraints and reduce per-message cost.
6. **COPPA compliance by design** — verifiable parental consent, data minimization, age-gated features.
7. **Cost-optimized AI** — two-tier inference, deterministic rules first, LLM second.

### Non-goals (Explicit)
- Reading all iOS SMS/iMessage system-wide (not possible for third-party apps).
- Covert monitoring of other people's accounts without consent.
- Deceptive auto-replies (assistant must self-identify).
- Replacing existing enterprise messaging (Slack/Teams) — MsgVet governs, not replaces.
- End-to-end encryption for all channels (phase 2; first-party chat encryption is MVP).

### Success Metrics

| Metric | Target (6 months post-launch) |
|--------|-------------------------------|
| Regret-send reduction (user-reported) | > 60% of active users report fewer regrets |
| Blocked messages confirmed "good catch" | > 75% |
| Kid safety: flagged content caught | > 95% recall on severe categories |
| Parent satisfaction (NPS) | > 50 |
| Approval latency (p95) | < 5 minutes for AI-only; < 30 min for human |
| Cost per 1,000 vetted messages | < $0.15 (cheap model tier) |
| Monthly active families (MAF) | 10,000 by month 6 |

---

## 4. Personas & User Journeys

### 4.1 Kid Account (Primary MVP Start)

**Profile:** Ages 8-17, managed by parent, needs protection from harmful inbound AND safe outbound.

**Journey: Kid receives a suspicious message**
1. Message arrives from unknown sender on MsgVet Chat
2. Vet engine classifies: bullying language + unknown sender = HIGH risk
3. Message routed to quarantine (kid never sees it)
4. Parent receives push notification: "1 message quarantined for [Child Name]"
5. Parent reviews in Family Dashboard, chooses: release / block sender / report
6. If released: kid sees message with optional "flagged" indicator

**Journey: Kid tries to send inappropriate content**
1. Kid composes message with profanity targeting a classmate
2. Vet engine catches: harassment language + minor sender = BLOCK
3. Kid sees: "This message was held. A parent will review it."
4. Parent sees in approval queue, can: reject with note / approve / edit and send

### 4.2 Adult Consumer

**Profile:** Ages 18+, wants guardrails for high-risk communication scenarios.

**Journey: Drunk Guard activation**
1. User enables "Drunk Guard" policy (manual toggle or time-based auto)
2. User composes text to ex at 2 AM
3. Vet engine: ex-contact + drunk-guard-active + late-hour = HOLD
4. User sees: "Message held until tomorrow 10 AM. Want to review then?"
5. Next morning: user reviews, sees vet report (tone: angry, urgency: false), deletes message
6. System logs: 1 regret-send prevented

### 4.3 Corporate / Executive

**Profile:** C-suite, PR, Legal — needs delegation and compliance.

**Journey: CEO delegate sends public statement**
1. EA drafts statement on CEO's behalf (delegated draft permission)
2. Message requires quorum: PR + Legal must both approve
3. PR approves with minor edit suggestion
4. Legal approves
5. Quorum met — message auto-sends via scheduled slot
6. Full audit trail: drafter, approvers, timestamps, policy version, vet scores

---

## 5. Core Concepts (Platform Primitives)

| Primitive | Description |
|-----------|-------------|
| **Channel** | External (SMS/email/X) or Internal (MsgVet Chat/Voice) |
| **Account** | A connected identity (Gmail inbox, X handle, MsgVet ID) |
| **Alias Identity** | Label + optional phone/handle for compartmentalization |
| **Group** | Relationship buckets (public/family/work) + sensitive via aliases |
| **Policy Profile** | Named ruleset (Kid Safe, Drunk Guard, C-suite, etc.) |
| **Rule Matrix** | Conditional rules: account x channel x group x time x location x state |
| **Vet Report** | Scores + tags + pass/warn/block/approve + rewrite suggestions |
| **Quarantine** | Holding area for blocked messages + structured review workflow |
| **Approval Workflow** | AI gate, human gate, mixed gate, quorum, SLA/escalation |
| **Delegation** | Draft/send/approve permissions per account/profile |
| **Tenant** | Family (parent admin + child users) or Org (roles hierarchy) |

### Entity Relationship (Simplified)

```
Tenant (Family | Org)
 ├── Users (with roles)
 │    ├── Accounts (connected channels)
 │    ├── Alias Identities
 │    └── Group Memberships
 ├── Policy Profiles
 │    └── Rule Matrix Entries
 ├── Conversations
 │    ├── Messages (vetted)
 │    │    ├── Vet Reports
 │    │    └── Quarantine Records
 │    └── Approval Workflows
 └── Audit Log (append-only)
```

---

## 6. Product Surfaces

### 6.1 Mobile (iOS / Android) — Primary

| Screen | Description |
|--------|-------------|
| **Unified Inbox** | External + internal threads, color-coded by vet status |
| **Compose** | Profile selector, templates, real-time vet preview, rewrite suggestions |
| **Quarantine** | Inbound + outbound held messages, reason codes, release/reject actions |
| **Approvals** | Pending approval queue for approvers/delegates |
| **Family Dashboard** | Kid activity summary, quarantine queue, policy controls (parent-only) |
| **Settings** | Aliases, policies, billing, privacy, connected accounts |
| **Voice** | VoIP calling (phase: PSTN bridge) |

### 6.2 Web (MVP for Admin/Ops)

| Screen | Description |
|--------|-------------|
| **Parent Console** | Full family dashboard with analytics and policy management |
| **Corporate Console** | Policies, users, delegation, audit logs, exports |
| **Admin Panel** | Support tools, abuse reports, appeals, system health |

### 6.3 Platform APIs

| API Surface | Consumers |
|-------------|-----------|
| **REST API** | Mobile apps, web console, third-party integrations |
| **WebSocket** | Real-time message delivery, typing indicators, presence |
| **Webhook** | External system notifications (alerts, approvals) |

---

## 7. Authentication, Roles & Tenancy

### 7.1 Authentication
- **Sign-in methods:** Email + password, OAuth (Apple/Google/Microsoft), passkeys (phase 2)
- **MFA:** Optional for consumer; required by policy for corporate tenants
- **Session:** Device trust registry + biometric unlock (Face ID / fingerprint)
- **Kid accounts:** No direct OAuth; parent creates and manages credentials

### 7.2 Roles

#### Family Tenant Roles

| Role | Capabilities |
|------|-------------|
| **Parent (Admin)** | Full control: policies, quarantine release, kid account management, billing |
| **Teen** | Send with vetting, limited policy visibility, no quarantine release |
| **Child (Under-13)** | Send with vetting + approval required, no settings access, COPPA-compliant |

#### Org Tenant Roles

| Role | Capabilities |
|------|-------------|
| **Owner** | Full org control, billing, user management, policy override |
| **Admin** | User management, policy management, audit access |
| **Approver** | Approve/reject messages in approval queues |
| **Sender** | Send messages (may require approval per policy) |
| **Drafter** | Draft messages on behalf of others (delegation) |
| **Auditor** | Read-only access to audit logs, exports, analytics |

### 7.3 Tenancy Model
- **Family tenant:** Parent admin + child restricted users. One family = one tenant.
- **Org tenant:** Hierarchical roles. SSO optional (phase 2 for SAML/OIDC).
- **Cross-tenant:** Not supported. Users can belong to multiple tenants with separate contexts.

---

## 8. Kid-First: Family Safety System

> **Build order priority:** This is Milestone A. Everything else depends on proving kid safety works.

### 8.1 Kid Account Types

| Type | Age Range | COPPA Trigger | Defaults |
|------|-----------|---------------|----------|
| **Child** | Under 13 | YES — verifiable parental consent required | All outbound requires approval, strictest inbound filtering, minimal data retention |
| **Teen** | 13-17 | Parent-managed, relaxed thresholds | Outbound vetted (not all require approval), moderate inbound filtering |

### 8.2 Inbound Filtering Pipeline

Every inbound message to a kid account executes this pipeline:

```
Message Received
  │
  ├─ Step 1: Deterministic Filters
  │   ├─ Allow list (trusted contacts) → ALLOW
  │   ├─ Deny list (blocked senders) → HARD BLOCK
  │   ├─ Time window check (e.g., no messages after bedtime) → QUARANTINE
  │   └─ Unknown sender rules → QUARANTINE or escalate
  │
  ├─ Step 2: LLM Classification (if not already resolved)
  │   ├─ Categories: bullying, sexual content, self-harm, threats, scams, PII exposure
  │   ├─ Severity: LOW / MEDIUM / HIGH / CRITICAL
  │   └─ Confidence score
  │
  └─ Step 3: Decision
      ├─ ALLOW (trusted + clean)
      ├─ ALLOW + FLAG (minor concern, log for parent review)
      ├─ QUARANTINE (default for HIGH/CRITICAL; parent must release)
      └─ HARD BLOCK + quarantine metadata (optional, for known threats)
```

### 8.3 Parent Notifications

| Severity | Notification | Timing |
|----------|-------------|--------|
| CRITICAL (self-harm, threats) | Immediate push + optional SMS | Real-time |
| HIGH (bullying, sexual) | Push notification | Within 1 minute |
| MEDIUM (mild language, unknown sender) | Batched digest | Hourly or configurable |
| LOW (minor flags) | Daily summary | End of day |

**Crisis escalation:** If self-harm or threat signals detected, quarantine + parent alert + optional link to crisis resources (988 Lifeline, Crisis Text Line).

### 8.4 Kid Outbound Safety

Every outbound message from a kid runs:

| Check | Action on Fail |
|-------|----------------|
| Language analysis (profanity, threats, harassment) | Block + hold for parent |
| Sexual content detection | Block + hold for parent |
| Self-harm signal detection | Quarantine + CRITICAL parent alert |
| Doxxing / PII exposure (address, phone, SSN patterns) | Block + parent alert |
| Time-of-day restrictions (bedtime enforcement) | Block with "Try again tomorrow" |
| Unknown recipient restriction | Hold for parent approval |
| Message frequency limits (anti-spam) | Throttle + notify |

### 8.5 Parent Review Workflow

```
Quarantined Message
  │
  ├─ Parent opens Family Dashboard / Quarantine tab
  ├─ Sees: sender, timestamp, reason code, severity, vet report summary
  │   (message content visible only if parent opts in; default: metadata-only for privacy)
  │
  └─ Actions:
      ├─ Release to kid
      ├─ Release with note to kid ("Be careful with this person")
      ├─ Reject (message deleted from kid view)
      ├─ Block sender (add to deny list)
      ├─ Report sender (abuse pipeline)
      └─ Adjust policy (e.g., add to trusted contacts, change thresholds)
```

---

## 9. Adult Consumer: Guardrails & Intent Shaping

### 9.1 Policy Packs (Shipped in MVP)

| Pack | Description | Key Rules |
|------|-------------|-----------|
| **Drunk Guard** | Prevents impulsive sends during high-risk hours/states | Time gates (e.g., 11PM-8AM), specific contacts blocked, mandatory cooldown |
| **Ex Guard / No-Contact** | Blocks all communication to specified contacts | Hard block on send, optional hard block on receive |
| **Professional Mode** | Ensures workplace-appropriate tone | Profanity filter, tone analysis, formality enforcement |
| **Public Posting Guard** | Reviews before posting to public channels | Mandatory vet report review, rewrite suggestions, cooldown |
| **Warm & Empathetic** | Shapes messages toward empathetic tone | Tone rewrite, sentiment enforcement |
| **Short, Neutral, No Sarcasm** | Strips emotion, keeps factual | Length limits, sarcasm detection, neutrality enforcement |
| **No Sensitive Topics** | Blocks configurable topic categories | Topic classifier, configurable blocklist |

### 9.2 Regret Controls

| Control | Description |
|---------|-------------|
| **Cooldown timer** | Configurable delay (30s - 24h) before send, per contact/group/time |
| **Scheduled send** | Force messages to queue for future delivery |
| **Undo-send window** | Recall window (where channel supports it); for first-party chat: up to 5 min |
| **Buddy check** | Escalate to designated approver for specific contacts/times |
| **State toggle** | User manually activates modes (drunk, angry, upset) for enhanced guardrails |

### 9.3 Tone & Intent Capture

**Compose flow:**
1. User selects intent: apology / decline / request / update / congratulation / complaint
2. User writes draft (or uses template)
3. Vet engine analyzes against intent + active policy profile
4. System suggests rewrites aligned to intent + constraints:
   - Minimal edit (closest to original)
   - More professional
   - More empathetic
   - Shorter
   - "Safe & boring" (maximum risk reduction)
5. User picks version or edits further
6. Final vet check before send

---

## 10. Corporate: Governance, Approvals & Audit

### 10.1 Delegation Model

| Permission | Description | Example |
|------------|-------------|---------|
| **Draft** | Create messages on behalf of another user | EA drafts for CEO |
| **Send** | Send messages from another user's account | EA sends approved drafts |
| **Approve** | Approve/reject messages in approval queues | Legal reviews public statements |
| **Manage** | Modify policies and settings for delegated accounts | Admin manages exec communications |

Delegation is per-account, per-profile, with audit logging on every action.

### 10.2 Quorum Approvals

- Configurable M-of-N approval (e.g., 2 of 3: PR + Legal + CEO)
- Parallel or sequential approval chains
- SLA per approval step (e.g., Legal has 2 hours)
- Escalation on SLA breach (notify backup approver)
- Emergency bypass: biometric + reason + hard audit flag + notify all approvers

### 10.3 Audit & Compliance

| Feature | Description |
|---------|-------------|
| **Immutable event log** | Append-only, tamper-evident hash chain |
| **Retention policies** | Configurable per org (30 days to indefinite) |
| **Legal hold** | Freeze deletion for specified users/threads (phase 2) |
| **Export** | Structured exports (JSON/CSV) for compliance review |
| **Role-based audit access** | Auditor role sees logs without message content (configurable) |

---

## 11. Quarantine System

> **Platform-wide rule:** All blocked content (inbound or outbound) lands in quarantine unless explicitly configured as silent drop.

### 11.1 Quarantine Record Structure

```
QuarantineRecord:
  id: uuid
  tenantId: uuid
  direction: INBOUND | OUTBOUND
  originalMessage:
    sender: AccountRef
    recipient: AccountRef
    channel: ChannelRef
    content: encrypted blob (or hash if content-free mode)
    timestamp: ISO 8601
  vetReport:
    modelVersion: string
    scores: { tone, urgency, professionalism, risk }
    tags: string[]
    decision: QUARANTINE | HARD_BLOCK
    reasonCodes: string[]
    rewriteSuggestions: RewriteOption[] (if outbound)
  status: PENDING | RELEASED | REJECTED | EXPIRED
  reviewedBy: UserRef | null
  reviewedAt: ISO 8601 | null
  reviewNote: string | null
  expiresAt: ISO 8601 (auto-purge after retention window)
```

### 11.2 Quarantine UX

| Element | Description |
|---------|-------------|
| **List view** | Sortable by severity, date, sender, direction |
| **Reason codes** | Human-readable: "Blocked sender," "Unsafe category: bullying," "Time window violation" |
| **Vet report summary** | Inline scores, tags, confidence |
| **Actions** | Release / Approve with rewrite / Reject + note / Block sender / Report / Escalate |
| **Batch actions** | Multi-select: release all from trusted sender, reject all expired |
| **For kid accounts** | Parent-only release (default; teen can self-release LOW severity if policy allows) |

---

## 12. Policy Engine & Rule Matrix

### 12.1 Policy Profile Structure

```
PolicyProfile:
  id: uuid
  name: string (e.g., "Kid Safe Core")
  description: string
  tier: KID | ADULT | CORPORATE
  isSystem: boolean (shipped packs vs user-created)
  rules: RuleMatrixEntry[]
  priority: number (for conflict resolution)
  active: boolean
```

### 12.2 Rule Matrix Entry

```
RuleMatrixEntry:
  conditions:
    accounts: AccountRef[] | ALL
    channels: ChannelType[] | ALL
    targets: GroupRef[] | ContactRef[] | ALL
    timeWindows: TimeWindow[] | ALL
    locations: GeoFence[] | ALL (opt-in)
    userStates: StateToggle[] | ALL (drunk, angry, quiet, court, etc.)
    direction: INBOUND | OUTBOUND | BOTH
  action:
    type: ALLOW | WARN | BLOCK | REQUIRE_APPROVAL | REQUIRE_QUORUM | SCHEDULE_ONLY | REWRITE_REQUIRED
    params:
      cooldownSeconds: number | null
      approverRoles: string[] | null
      quorumConfig: { required: number, pool: UserRef[] } | null
      rewriteMode: MINIMAL | PROFESSIONAL | EMPATHETIC | SHORT | SAFE | null
  priority: number
  enabled: boolean
```

### 12.3 Conflict Resolution

1. **Most specific wins:** Rule matching account + contact + time beats rule matching only account.
2. **Stricter wins on conflict:** If two rules at same specificity conflict, the more restrictive action applies.
3. **Explicit override:** Rules flagged `override: true` can relax restrictions (requires admin role).
4. **Audit on every resolution:** Log which rules matched, which won, and why.

### 12.4 Policy Packs Shipped in MVP

| Pack | Tier | Key Rules |
|------|------|-----------|
| Kid Safe (Core) | KID | Strictest: all inbound vetted, outbound requires approval, unknown senders quarantined |
| Teen Safe | KID | Moderate: inbound vetted, most outbound auto-approved, unknown senders flagged |
| Drunk Guard | ADULT | Time-based + state-based blocks, specific contacts, mandatory cooldown |
| Ex Guard / No-Contact | ADULT | Hard block on specified contacts, both directions |
| C-level / Executive | CORPORATE | All external sends require approval, delegation enabled |
| PR Safe / Brand Safe | CORPORATE | Public posts require vet + approval, tone enforcement |
| Sales / Customer Success | CORPORATE | Professional tone, no commitment language, compliance tags |
| Legal-safe | CORPORATE | No speculation, no commitments, privilege markers |
| Warm & Empathetic | ADULT | Tone shaping toward empathy |
| Short, Neutral, No Sarcasm | ADULT | Length limits, sarcasm detection |
| No Sensitive Topics | ALL | Configurable topic blocklist |

---

## 13. Sensitive Groups & Alias System

### 13.1 Design Principles
- Canonical internal group IDs are never user-visible.
- User-visible labels are **aliases** (e.g., "VIP-Red," "Private-2").
- Default: alias labels stay local on device; cloud stores only group ID + hashed handles.
- Notifications in "stealth mode" hide names and message previews.

### 13.2 Alias Identity

```
AliasIdentity:
  id: uuid
  userId: uuid
  label: string (user-visible, local only)
  phoneNumber: string | null (provisioned proxy number)
  handle: string | null (for first-party chat)
  linkedAccounts: AccountRef[]
  policies: PolicyProfileRef[]
  stealthMode: boolean
```

### 13.3 Privacy Architecture
- Contact aliases encrypted at rest with user-specific keys.
- Cloud sync: only encrypted blob + group ID. Server cannot read alias labels.
- Export: alias labels included only in local export; cloud export uses group IDs.

---

## 14. Vetting Engine & Visualization

### 14.1 Vet Report Structure

Every message (inbound and outbound) produces a vet report:

```
VetReport:
  messageId: uuid
  direction: INBOUND | OUTBOUND
  modelVersion: string
  modelTier: CHEAP | STRONG
  scores:
    tone: { value: string, confidence: float }
      // calm, angry, sarcastic, flirty, dismissive, apologetic, threatening, neutral
    urgency: { value: LOW | MEDIUM | HIGH, actuallyUrgent: boolean, confidence: float }
    professionalism: { score: 0-100, confidence: float }
    relationshipAppropriateness: { score: 0-100, confidence: float }
  riskTags: string[]
    // harassment, threat, doxxing, compliance, reputational, scam, self-harm, sexual, PII
  decision: PASS | WARN | BLOCK | REQUIRE_APPROVAL
  matchedRules: RuleRef[]
  rewriteSuggestions: (outbound only)
    - type: MINIMAL_EDIT
      text: string
    - type: MORE_PROFESSIONAL
      text: string
    - type: MORE_EMPATHETIC
      text: string
    - type: SHORTER
      text: string
    - type: SAFE_AND_BORING
      text: string
  processingTimeMs: number
  cost: { inputTokens: number, outputTokens: number, estimatedCostUsd: float }
```

### 14.2 Visualization

| Element | Description |
|---------|-------------|
| **Message list chips** | Color-coded: green (pass), yellow (warn), red (blocked), blue (approval pending) |
| **Tone indicator** | Emoji-free icon + label on each message |
| **Urgency badge** | "Actually urgent" vs "feels urgent" classifier |
| **Thread timeline** | Tone drift graph over conversation (escalation detection) |
| **Vet report drawer** | Slide-out panel with full scores, tags, matched rules |
| **Cost indicator** | Per-message AI cost (admin view) |

---

## 15. Approval Workflows

### 15.1 Approval Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| **AI Gate Only** | Auto-approve if vet score passes thresholds | Low-risk adult consumer sends |
| **Human Always** | Every message requires human approval | Under-13 outbound, CEO public statements |
| **Mixed** | AI must pass AND human approves on triggers | Corporate external sends |
| **Quorum** | M-of-N approvers must approve | PR + Legal for public comms |

### 15.2 SLA & Escalation

```
ApprovalWorkflow:
  id: uuid
  messageId: uuid
  mode: AI_ONLY | HUMAN_ALWAYS | MIXED | QUORUM
  requiredApprovers: UserRef[]
  quorumRequired: number | null
  slaMinutes: number (per step)
  escalationChain: UserRef[] (fallback approvers)
  status: PENDING | APPROVED | REJECTED | ESCALATED | EXPIRED
  approvals: ApprovalAction[]
  createdAt: ISO 8601
  slaDeadline: ISO 8601
```

### 15.3 Emergency Bypass
- Biometric authentication required
- Mandatory reason text (logged)
- Hard audit flag on the send event
- Immediate notification to all approvers: "[User] bypassed approval for [message]. Reason: [text]"
- Review required within 24 hours

---

## 16. Messaging Architecture

### 16.1 External Adapters

| Channel | Integration Method | Limitations |
|---------|-------------------|-------------|
| **Email** | IMAP/SMTP + OAuth (Gmail, Outlook) | Full send/receive |
| **SMS (Android)** | Default SMS handler registration | Requires user to set MsgVet as default SMS app |
| **SMS (iOS)** | Compose-through + share sheet | Cannot intercept; compose-only |
| **X (Twitter)** | API v2 | Rate limits, API access tiers |
| **Meta (Instagram, FB)** | Graph API | Business accounts only |
| **WhatsApp** | Business API | Business accounts only |

### 16.2 First-Party MsgVet Chat + Voice

> **Recommended core.** Consistent enforcement, lower dependency, better kid safety, cheaper.

| Feature | MVP | Phase 2 |
|---------|-----|---------|
| 1:1 chat | Yes | — |
| Group chat | Yes | — |
| Read receipts (configurable) | Yes | — |
| Typing indicators | Yes | — |
| File attachments | — | Yes |
| Voice (VoIP) | Yes | — |
| Voice (PSTN bridge) | — | Yes |
| Video calling | — | Phase 3 |
| Message reactions | Yes | — |
| Message editing (within window) | Yes | — |

### 16.3 Azure Communication Services (ACS)

Primary candidate for first-party messaging infrastructure:
- Chat SDK for real-time messaging
- Calling SDK for VoIP
- SMS capability for external bridge
- Phone number provisioning for alias numbers

---

## 17. Alias Phone Numbers & Proxy Routing

### 17.1 Requirements
- Users provision alias phone numbers for compartmentalization.
- Inbound to alias → routed to correct user/group/policy.
- Outbound from alias → uses provisioned number (no spoofing).
- Number lifecycle: provision → active → suspend → rotate → recycle.

### 17.2 A2P Compliance
- A2P 10DLC registration required for app-originated SMS to US numbers.
- Campaign registration with TCR (The Campaign Registry).
- Separate short codes for transactional (OTP, alerts) vs promotional.

### 17.3 Cost Optimization Strategy

| Strategy | Description | Impact |
|----------|-------------|--------|
| **In-platform messaging** | Both parties on MsgVet = near-zero cost | Highest impact |
| **Pluggable provider abstraction** | Swap providers without code changes | Flexibility |
| **Least-cost routing (LCR)** | Route by region, carrier, content type | 20-40% savings |
| **SMS as fallback only** | Use SMS for invites, OTP, external recipients | Reduces SMS volume |
| **Segment awareness** | Warn user when message will split into multiple SMS segments | Cost transparency |
| **Provider competition** | Multi-provider routing based on real-time pricing | Ongoing optimization |

---

## 18. Scheduling & Delivery Assurance

### 18.1 Scheduler Service Pipeline

```
Schedule Request
  │
  ├─ 1. Validate: auth, policy snapshot, idempotency key
  ├─ 2. Preflight vet (policy version pinned at schedule time)
  ├─ 3. Enqueue send job (Service Bus queue)
  ├─ 4. At scheduled time:
  │   ├─ Re-vet (if policy changed since schedule)
  │   ├─ Provider send call (channel adapter)
  │   └─ Capture provider messageId / postId
  ├─ 5. Verification job:
  │   ├─ Check delivery receipt (where available)
  │   ├─ Poll status (for APIs without webhooks)
  │   └─ Retry with exponential backoff (max 3 attempts)
  └─ 6. Final status:
      ├─ DELIVERED (confirmed)
      ├─ SENT (no receipt available)
      ├─ FAILED (dead-letter after max retries)
      └─ EXPIRED (past delivery window)
```

### 18.2 Idempotency
- Every scheduled send has a unique idempotency key.
- Duplicate submissions return existing job status.
- Provider-level dedup where supported.

### 18.3 Azure Building Blocks

| Component | Purpose |
|-----------|---------|
| **Azure Service Bus** | Job queues + dead-letter queues |
| **Durable Functions** | Orchestration with retry policies |
| **Cosmos DB** | Job state storage (partition by tenant) |
| **Key Vault** | Provider credentials + encryption keys |
| **App Insights + Log Analytics** | Observability, correlation IDs |

---

## 19. Logging, Auditing & Privacy

### 19.1 Event Types (Log Everything)

| Category | Events |
|----------|--------|
| **Auth/Session** | login, logout, MFA challenge, device trust grant/revoke |
| **Account Linking** | OAuth grant, scope change, token refresh, disconnect |
| **Policy Changes** | create, update, delete, rule matrix diffs, pack assignment |
| **Vetting** | input hash, model version, scores, decision, processing time, cost |
| **Quarantine** | create, release, reject, expire, reviewer actions |
| **Approvals** | request, approve, reject, escalate, bypass, quorum events |
| **Sends** | attempted, sent, failed, retried, verified, dead-lettered |
| **Delegation** | grant, revoke, role changes, permission changes |
| **Billing** | usage events, invoice generation, payment, budget alerts |
| **Kid Safety** | crisis escalation, parent notifications, contact list changes |

### 19.2 Audit Requirements
- **Immutable append-only event store** with tamper-evident hash chain.
- **Per-event retention policies:** kid data (minimal), adult (configurable), corporate (policy-driven).
- **Structured exports:** JSON + CSV for compliance teams.
- **Correlation IDs:** client → service → provider, end-to-end traceability.

### 19.3 COPPA Compliance (Kid Privacy)

| Requirement | Implementation |
|-------------|---------------|
| Verifiable parental consent | Email + credit card verification OR signed consent form upload |
| Notice of data practices | In-app privacy notice + website privacy policy |
| Data minimization | Content storage off by default; metadata-only mode available |
| Parental access/delete | Parent dashboard: view all kid data, delete on request |
| No behavioral advertising | No ad targeting on kid accounts, ever |
| Data retention | Auto-purge kid data after configurable window (default: 90 days) |

---

## 20. API Design & Pagination

### 20.1 Pagination Standard

All list APIs support cursor pagination:

```
GET /api/v1/{resource}?pageSize=25&cursor={nextCursor}

Response:
{
  "data": [...],
  "pagination": {
    "pageSize": 25,
    "nextCursor": "eyJ0IjoxNjk...",
    "hasMore": true,
    "totalEstimate": 1234  // approximate, not guaranteed
  }
}
```

**Allowed page sizes:** 25 (default), 50, 100, 500, 1000.

### 20.2 Batch Operations

| Endpoint | Description |
|----------|-------------|
| `POST /api/v1/messages/batch-read` | Bulk mark-read for message IDs |
| `POST /api/v1/quarantine/batch-action` | Bulk release/reject quarantined items |
| `POST /api/v1/messages/search` | Search messages by thread, time window, severity |
| `GET /api/v1/audit/export` | Stream audit events for time range |

### 20.3 Client Behavior
- Cancel outstanding fetches on navigation away.
- Progressive rendering with backpressure.
- Optimistic UI updates with server reconciliation.

---

## 21. Billing & Usage Metering

### 21.1 Metered Units

| Unit | Description | Pricing Model |
|------|-------------|---------------|
| **AI vet action** | Classification + optional rewrite | Per action (tiered) |
| **External send** | SMS/email/social post | Pass-through + markup |
| **First-party message** | MsgVet Chat message | Free tier + per-message above |
| **Voice minute** | VoIP calling | Per minute |
| **PSTN minute** | Bridged phone calls | Per minute (pass-through) |
| **Storage** | Attachments, logs beyond free tier | Per GB/month |
| **Alias number** | Provisioned phone number | Per number/month |

### 21.2 Plans

| Plan | Target | Included | Price Range |
|------|--------|----------|-------------|
| **Family Free** | Trial families | 100 vet actions/mo, 1 kid account, MsgVet Chat only | $0 |
| **Family Safe** | Active families | 2,000 vet actions/mo, 5 accounts, basic SMS | $9.99/mo |
| **Adult Guard** | Individual adults | 1,000 vet actions/mo, all policy packs, scheduled sends | $4.99/mo |
| **Corporate** | Organizations | Custom vet volume, delegation, audit, SLA | $29.99/seat/mo |
| **Enterprise** | Large orgs | Unlimited, SSO, custom policies, dedicated support | Custom |

### 21.3 Controls
- Wallet/credits system with budgets per family/org.
- Hard caps + configurable warnings (50%, 80%, 95% usage).
- Invoice-grade immutable ledger (audit requirement).
- Real-time usage dashboard.

---

## 22. LLM Strategy & Cost Optimization

### 22.1 Two-Tier Inference

| Tier | Model Class | Use Cases | Estimated Cost |
|------|-------------|-----------|----------------|
| **Cheap** | GPT-4o-mini, Gemini Flash, Haiku | Standard classification, basic rewrites, kid inbound filter | ~$0.01-0.03 per 1K messages |
| **Strong** | GPT-4o, Gemini Pro, Sonnet | Edge cases, high-severity review, legal/PR vetting, polished rewrites | ~$0.10-0.30 per 1K messages |

### 22.2 Cost Controls

| Strategy | Description | Savings |
|----------|-------------|---------|
| **Deterministic rules first** | Regex, allowlists, denylists before any LLM call | 30-50% of messages need no LLM |
| **Prompt caching** | Cache system prompts + policy definitions | 50-90% on cached tokens |
| **Batch processing** | Non-interactive jobs (daily digests, analytics) use batch API | 50% batch discount |
| **Minimal context** | Summaries + embeddings, not full conversation history | 60-80% token reduction |
| **Rewrite-on-demand** | Don't generate rewrites unless user requests or policy requires | Eliminates ~70% of rewrite costs |
| **Model routing** | Route to cheap model; escalate to strong only on low confidence | 80% cheap, 20% strong |
| **Response caching** | Cache vet results for identical/near-identical messages | Variable |

### 22.3 Model Routing by Scenario

| Scenario | Model Tier | Escalation Trigger |
|----------|------------|-------------------|
| Kid inbound classifier | Cheap + deterministic filters | Confidence < 0.8 → Strong |
| Kid outbound check | Cheap | Any severe category detected → Strong |
| Adult standard rewrite | Cheap | User requests "high polish" → Strong |
| Corporate internal comms | Cheap | — |
| Corporate public/PR | Strong (always) | — |
| Legal triggers | Strong (always) | — |
| Self-harm/threat detection | Cheap → Strong (always escalate) | Always |

---

## 23. System Architecture

### 23.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     CLIENT LAYER                            │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────────┐ │
│  │ iOS App  │  │Android   │  │ Web      │  │ Admin      │ │
│  │(Swift UI)│  │(Compose) │  │ Console  │  │ Console    │ │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └─────┬──────┘ │
└───────┼──────────────┼──────────────┼──────────────┼────────┘
        │              │              │              │
        └──────────────┴──────┬───────┴──────────────┘
                              │
┌─────────────────────────────┼───────────────────────────────┐
│                     API GATEWAY                             │
│  ┌──────────────────────────┼────────────────────────────┐  │
│  │ Azure API Management / App Gateway                    │  │
│  │ • Auth (JWT + API keys)                               │  │
│  │ • Rate limiting                                       │  │
│  │ • Request routing                                     │  │
│  │ • TLS termination                                     │  │
│  └──────────────────────────┼────────────────────────────┘  │
└─────────────────────────────┼───────────────────────────────┘
                              │
┌─────────────────────────────┼───────────────────────────────┐
│                     SERVICE LAYER                           │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────────┐ │
│  │ Auth     │  │ Messaging│  │ Vet      │  │ Policy     │ │
│  │ Service  │  │ Service  │  │ Engine   │  │ Engine     │ │
│  └──────────┘  └──────────┘  └──────────┘  └────────────┘ │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────────┐ │
│  │ Quarant. │  │ Approval │  │ Scheduler│  │ Channel    │ │
│  │ Service  │  │ Service  │  │ Service  │  │ Adapters   │ │
│  └──────────┘  └──────────┘  └──────────┘  └────────────┘ │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────────┐ │
│  │ Billing  │  │ Audit    │  │ Notif.   │  │ Number     │ │
│  │ Service  │  │ Service  │  │ Service  │  │ Provisioner│ │
│  └──────────┘  └──────────┘  └──────────┘  └────────────┘ │
└─────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────┼───────────────────────────────┐
│                     DATA LAYER                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────────┐ │
│  │ Cosmos DB│  │ Service  │  │ Blob     │  │ Redis      │ │
│  │ (primary)│  │ Bus      │  │ Storage  │  │ (cache)    │ │
│  └──────────┘  └──────────┘  └──────────┘  └────────────┘ │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                  │
│  │ Key Vault│  │App       │  │ ACS      │                  │
│  │ (secrets)│  │Insights  │  │ (comms)  │                  │
│  └──────────┘  └──────────┘  └──────────┘                  │
└─────────────────────────────────────────────────────────────┘
```

### 23.2 Service Responsibilities

| Service | Responsibility |
|---------|---------------|
| **Auth Service** | Authentication, session management, device trust, MFA, kid account creation |
| **Messaging Service** | First-party chat/voice, message routing, delivery, presence, typing |
| **Vet Engine** | Message classification, scoring, rewrite generation, model routing |
| **Policy Engine** | Rule matrix evaluation, conflict resolution, policy pack management |
| **Quarantine Service** | Quarantine CRUD, review workflows, expiry management |
| **Approval Service** | Approval workflows, quorum tracking, SLA enforcement, escalation |
| **Scheduler Service** | Scheduled sends, idempotency, delivery verification, retry |
| **Channel Adapters** | External channel integration (email, SMS, social), protocol translation |
| **Billing Service** | Usage metering, invoicing, budget enforcement, payment processing |
| **Audit Service** | Event ingestion, immutable logging, export, retention enforcement |
| **Notification Service** | Push notifications, SMS alerts, email digests, webhook delivery |
| **Number Provisioner** | Alias phone number lifecycle, A2P compliance, provider abstraction |

---

## 24. Technology Stack

### 24.1 Backend (Azure-first)

| Component | Technology | Notes |
|-----------|-----------|-------|
| **Runtime** | .NET 9 Minimal APIs | Following IC pattern; high performance, Azure-native |
| **Database** | Azure Cosmos DB | Partition by tenantId; global distribution optional |
| **Cache** | Azure Cache for Redis | Session, vet result cache, rate limiting |
| **Queue** | Azure Service Bus | Job queues, dead-letter, topic subscriptions |
| **Orchestration** | Azure Durable Functions | Scheduler, approval workflows, retry policies |
| **Secrets** | Azure Key Vault | Provider credentials, encryption keys |
| **Storage** | Azure Blob Storage | Attachments, exports, audit archives |
| **Observability** | Application Insights + Log Analytics | Traces, metrics, correlation IDs |
| **AI** | Azure OpenAI + fallback to direct API | GPT-4o-mini (cheap), GPT-4o (strong) |
| **Communications** | Azure Communication Services | Chat SDK, Calling SDK, SMS, phone numbers |

### 24.2 Frontend

| Component | Technology | Notes |
|-----------|-----------|-------|
| **Mobile (iOS)** | SwiftUI + Combine | Native iOS experience |
| **Mobile (Android)** | Jetpack Compose + Kotlin | Native Android experience |
| **Web Console** | Angular 21 (following IC) or React | Admin/parent console |
| **Real-time** | SignalR / WebSocket | Message delivery, presence, typing |

### 24.3 Infrastructure

| Component | Technology |
|-----------|-----------|
| **CI/CD** | GitHub Actions |
| **Container** | Azure Container Apps |
| **DNS/CDN** | Azure Front Door |
| **Monitoring** | Azure Monitor + PagerDuty |
| **Compliance** | Azure Policy + Defender for Cloud |

---

## 25. Data Model

### 25.1 Core Entities

```
Tenant
  ├── id: uuid (partition key)
  ├── type: FAMILY | ORG
  ├── name: string
  ├── plan: PlanRef
  ├── settings: TenantSettings
  └── createdAt: ISO 8601

User
  ├── id: uuid
  ├── tenantId: uuid (partition key)
  ├── email: string (hashed for kid accounts)
  ├── role: PARENT | TEEN | CHILD | OWNER | ADMIN | APPROVER | SENDER | DRAFTER | AUDITOR
  ├── policyProfiles: PolicyProfileRef[]
  ├── deviceTrust: DeviceRef[]
  └── createdAt: ISO 8601

Account (connected channel identity)
  ├── id: uuid
  ├── userId: uuid
  ├── tenantId: uuid
  ├── channelType: EMAIL | SMS | X | META | MSGVET_CHAT | MSGVET_VOICE
  ├── credentials: encrypted blob (Key Vault ref)
  ├── status: ACTIVE | SUSPENDED | DISCONNECTED
  └── linkedAt: ISO 8601

Message
  ├── id: uuid
  ├── tenantId: uuid (partition key)
  ├── conversationId: uuid
  ├── senderId: AccountRef
  ├── recipientId: AccountRef
  ├── channel: ChannelType
  ├── direction: INBOUND | OUTBOUND
  ├── content: encrypted blob
  ├── vetReportId: uuid
  ├── status: SENT | DELIVERED | FAILED | QUARANTINED | PENDING_APPROVAL | SCHEDULED
  ├── scheduledAt: ISO 8601 | null
  └── createdAt: ISO 8601

AuditEvent
  ├── id: uuid
  ├── tenantId: uuid (partition key)
  ├── eventType: string
  ├── actorId: UserRef
  ├── targetId: string
  ├── payload: json
  ├── previousHash: string (chain integrity)
  ├── hash: string
  └── timestamp: ISO 8601
```

---

## 26. Security Architecture

### 26.1 Encryption

| Layer | Method |
|-------|--------|
| **In transit** | TLS 1.3 everywhere |
| **At rest** | Azure Storage Service Encryption + customer-managed keys (corporate tier) |
| **Message content** | AES-256 per-tenant encryption; key in Key Vault |
| **Alias labels** | User-specific encryption; server cannot read |
| **Credentials** | Never stored in DB; Key Vault references only |

### 26.2 Access Control

- **Tenant isolation:** Every query filtered by tenantId at data layer. No cross-tenant access.
- **Role-based:** Actions gated by role (see Section 7.2).
- **Resource-level:** Kid accounts cannot access quarantine, settings, or other kids' data.
- **API keys:** Per-integration, scoped, rotatable.
- **Rate limiting:** Per-tenant, per-user, per-endpoint.

### 26.3 Threat Model (Key Threats)

| Threat | Mitigation |
|--------|-----------|
| Parent account compromise → kid data exposed | MFA + device trust + biometric unlock |
| LLM prompt injection in message content | Input sanitization, model output validation, no tool-use in vet pipeline |
| Data exfiltration via export API | Role-gated, rate-limited, audit-logged exports |
| Provider credential leak | Key Vault + short-lived tokens + rotation alerts |
| Insider access to kid data | Audit logs on all data access, principle of least privilege |

---

## 27. Build Order & Milestones

### Milestone A: Kid-First Foundation (Weeks 1-8)

| Deliverable | Details |
|-------------|---------|
| Family tenant + parent/child accounts | Auth, roles, COPPA consent flow |
| Kid policy engine | Rule matrix for inbound/outbound, deterministic filters |
| Inbound filtering pipeline | LLM classification, quarantine routing |
| Outbound safety checks | Language, PII, time-of-day, unknown recipient |
| Quarantine system | CRUD, parent review workflow, release/reject |
| Parent dashboard | Activity summary, quarantine queue, policy controls |
| First-party chat (MVP) | 1:1 messaging, basic presence, read receipts |
| Logging & audit (foundation) | Event store, kid-specific retention |
| Pagination & batch ops | All list APIs cursor-paginated |
| Billing scaffolding | Usage tracking, free tier enforcement |
| Push notifications | Parent alerts (severity-based) |

### Milestone B: Adult Consumer Layer (Weeks 9-14)

| Deliverable | Details |
|-------------|---------|
| Adult accounts + policy packs | Drunk Guard, Ex Guard, Professional Mode, etc. |
| Regret controls | Cooldown, scheduled send, undo-send, buddy check |
| Intent capture + rewrite | Compose flow with intent selection + AI rewrites |
| Scheduling service | Scheduled sends with delivery verification |
| Alias groups + stealth mode | Sensitive group management, privacy |
| First-party voice (VoIP) | Basic VoIP calling via ACS |
| Email adapter | IMAP/SMTP integration (Gmail, Outlook) |
| Alias phone numbers | Proxy number provisioning, routing |

### Milestone C: Corporate Layer (Weeks 15-20)

| Deliverable | Details |
|-------------|---------|
| Org tenants + role hierarchy | Owner, Admin, Approver, Sender, Drafter, Auditor |
| Delegation model | Draft/send/approve permissions, per-account |
| Quorum approvals | M-of-N, SLA, escalation chains |
| Audit exports + retention | JSON/CSV export, configurable retention, legal hold prep |
| Corporate policy templates | C-level, PR Safe, Legal-safe, Sales packs |
| Admin console (web) | Policies, users, delegation, audit logs |
| SSO prep | SAML/OIDC integration points (full SSO phase 2) |

### Milestone D: Hardening & Launch (Weeks 21-24)

| Deliverable | Details |
|-------------|---------|
| Load testing | Target: 10K concurrent users, 100K messages/hour |
| Security audit | Pen test, COPPA compliance review, threat model validation |
| Cost optimization | LLM routing tuning, provider negotiation, cache hit rates |
| App store submissions | iOS App Store + Google Play review |
| Documentation | API docs, integration guides, parent guides |
| Beta program | 500 families, 50 corporate accounts |

---

## 28. High-Leverage Ideas

| # | Idea | Impact | Effort |
|---|------|--------|--------|
| 1 | **Conversation style fingerprint** — learn normal tone/length per relationship; flag anomalies | HIGH | MEDIUM |
| 2 | **Regret window everywhere** — default 30-120s delayed send for configurable targets | HIGH | LOW |
| 3 | **Safety escalations** — self-harm/threat → quarantine + parent alert + crisis resources | CRITICAL | LOW |
| 4 | **Policy simulation / dry-run** — "if we enable PR Safe, what would have been blocked last week?" | HIGH | MEDIUM |
| 5 | **Red-team mode** — admins test policies with adversarial prompts, see failure modes | HIGH | MEDIUM |
| 6 | **Least-privilege + local-first** — contact aliases stay local; cloud stores hashes only | MEDIUM | LOW |
| 7 | **Provider segmentation controls** — show "cost estimate" before send for multi-segment SMS | MEDIUM | LOW |
| 8 | **Kid "report" button** — kids can flag messages themselves, triggering parent review | HIGH | LOW |
| 9 | **Weekly safety digest** — automated parent report: contacts, tone trends, flags | HIGH | MEDIUM |
| 10 | **Peer comparison (anonymized)** — "Your child's messaging patterns are within normal range" | MEDIUM | HIGH |

---

## 29. Risks & Mitigations

| Risk | Severity | Mitigation |
|------|----------|-----------|
| iOS SMS interception not possible | HIGH | Prioritize first-party chat; iOS = compose-through + share sheet |
| COPPA compliance failure | CRITICAL | Legal review pre-launch, verifiable consent flow, minimal data retention |
| LLM false positives (over-blocking) | HIGH | Configurable thresholds, human review, "boring safe" fallback, parent override |
| LLM false negatives (missed threats) | CRITICAL | Deterministic filters first, always-escalate for severe categories, regular eval |
| Messaging costs explode | HIGH | LCR routing, in-platform comms, cost preview, segment awareness, hard caps |
| Kids circumvent controls | MEDIUM | Device trust, app-level enforcement, behavioral anomaly detection |
| Provider API changes/shutdowns | MEDIUM | Pluggable adapter pattern, multi-provider support, first-party as primary |
| Parent privacy concerns ("you're spying") | MEDIUM | Transparency: kid knows messages are vetted; metadata-only mode default |
| Scale challenges (real-time vetting latency) | HIGH | Cheap model + caching + deterministic pre-filter = sub-200ms p95 |

---

## 30. Open Questions

| # | Question | Options | Impact |
|---|----------|---------|--------|
| 1 | Target kid age bands | Under-13 only vs Under-13 + 13-17 with different UX | UX complexity, COPPA scope |
| 2 | Default retention settings | 30/90/365 days per tier | Storage costs, compliance |
| 3 | MVP comms scope | Chat-only vs Chat + Voice | Development timeline |
| 4 | Must-have external adapters | Email only vs Email + SMS | Integration effort |
| 5 | Billing model | Pure metered vs subscription + included usage | Revenue predictability |
| 6 | First-party chat encryption | E2E encryption in MVP or phase 2 | Security vs complexity |
| 7 | Android default SMS handler | Pursue immediately or phase 2 | Google Play policy review |
| 8 | Crisis escalation scope | Link to resources only vs direct integration with crisis services | Liability, partnerships |
| 9 | Web console technology | Angular (following IC) vs React/Next.js | Team skills, ecosystem |
| 10 | Multi-language support | English-only MVP vs i18n from start | Market scope |

---

## 31. Glossary

| Term | Definition |
|------|-----------|
| **A2P** | Application-to-Person messaging (SMS sent by apps, not humans) |
| **ACS** | Azure Communication Services |
| **Alias** | User-defined label for a contact group or phone number, stored locally |
| **COPPA** | Children's Online Privacy Protection Act (US, under-13 protections) |
| **DLQ** | Dead Letter Queue (failed messages after max retries) |
| **Idempotency Key** | Unique token ensuring a send operation executes exactly once |
| **LCR** | Least-Cost Routing (choosing cheapest provider per message) |
| **LLM** | Large Language Model (AI used for classification and rewriting) |
| **Policy Pack** | Pre-configured rule set for a specific use case (Kid Safe, Drunk Guard, etc.) |
| **Quarantine** | Holding area for messages that failed vetting, awaiting review |
| **Rule Matrix** | Conditional rules evaluated per message: who x where x when x what = action |
| **TCR** | The Campaign Registry (US A2P SMS registration body) |
| **Tenant** | Top-level organizational unit (Family or Org) |
| **Vet Report** | AI-generated analysis of a message: scores, tags, decision, rewrites |
| **10DLC** | 10-Digit Long Code (standard US phone numbers used for A2P SMS) |

---

*Document generated: April 6, 2026 | MsgVet v1.0.0 PRD | Draft for Review*
