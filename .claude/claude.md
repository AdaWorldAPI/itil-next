# ITIL-NEXT Orchestrator

## Project Vision

```
┌─────────────────────────────────────────────────────────────────┐
│                    ITIL-NEXT: THE FUSION                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   "The ticket system that thinks like a person,                 │
│    not a form processor."                                       │
│                                                                  │
│   CORE PRINCIPLE:                                                │
│   ═══════════════                                                │
│   Agent = OWNER (sacred bond until resolved)                    │
│   Escalation = PARALLEL ASSIST (expertise joins, not replaces)  │
│                                                                  │
│   This is ITIL v4 thinking: value streams over silos.           │
│   The customer sees ONE face. Always.                           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Source Systems

| System      | Contribution           | What We Take                          |
|-------------|------------------------|---------------------------------------|
| Valuemation | ITIL compliance spine  | Incident/Change/Problem/CMDB links    |
| Orchestra   | Workflow automation    | Rules engine, SLA timers, routing     |
| xSuite      | UX patterns            | Clean UI, intuitive flows, mobile     |
| HubSpot     | Ticket DNA             | Timeline, ownership, pipeline stages  |
| MS Graph    | Email backbone         | Read/write/track, shared mailboxes    |
| MS Teams    | Notifications          | Real-time alerts, adaptive cards      |
| Jira        | Kanban patterns        | Boards, WIP limits, filters, views    |
| OpenProject | PM patterns            | Work packages, backlog management     |
| KIAS (Vodafone) | To excavate        | Jan's tribal knowledge                |
| Sony System | Escalation envelope    | The parallel assist UI pattern!       |
| MS System   | To excavate            | Jan's tribal knowledge                |

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         ITIL-NEXT                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                    UI LAYER (xSuite DNA)                 │   │
│  │  • Timeline-centric view (HubSpot pattern)               │   │
│  │  • Kanban pipelines per process                          │   │
│  │  • Escalation visible as "parallel thread"               │   │
│  │  • Mobile-first, keyboard shortcuts                      │   │
│  └──────────────────────────────────────────────────────────┘   │
│                              │                                   │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                 ENGINE LAYER (itil-next-engine)          │   │
│  │  • Ticket lifecycle (Valuemation ITIL)                   │   │
│  │  • Ownership model (agent = immutable owner)             │   │
│  │  • Escalation as parallel assist                         │   │
│  │  • SLA engine (Orchestra)                                │   │
│  │  • Rules/routing (Orchestra)                             │   │
│  └──────────────────────────────────────────────────────────┘   │
│                              │                                   │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │              CONNECTOR LAYER (itil-next-connectors)      │   │
│  │  • MS Graph (email read/write/track)                     │   │
│  │  • HubSpot (contact/company context)                     │   │
│  │  • CMDB (Valuemation asset links)                        │   │
│  │  • Active Directory (user/group resolution)              │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Repository Map

```
itil-next/                      ← YOU ARE HERE (Orchestrator)
├── .claude/
│   └── claude.md               ← This file (blackboard)
├── VISION.md                   ← Why we're building this
├── ROADMAP.md                  ← Phased delivery
├── prompts/                    ← Agent prompts
│   ├── architect.md
│   ├── excavator.md
│   └── integrator.md
└── decisions/                  ← ADRs (Architecture Decision Records)

itil-next-engine/               ← Core ticket logic
├── .claude/claude.md           ← Engine-specific context
├── src/
│   ├── models/                 ← Ticket, Agent, Escalation
│   ├── services/               ← Ownership, SLA, Routing
│   └── rules/                  ← Orchestra rule patterns
└── tests/

itil-next-connectors/           ← External integrations  
├── .claude/claude.md           ← Connector-specific context
├── msgraph/                    ← Email read/write
├── hubspot/                    ← CRM context
└── cmdb/                       ← Asset links
```

## The Escalation Model (CORE INNOVATION)

```
TRADITIONAL (broken):
  Agent A owns ticket → Escalates → Agent B owns ticket
  Result: Customer bounced, context lost, A disengaged

ITIL-NEXT (parallel assist):
  Agent A owns ticket → Escalates → Expert B joins as ASSIST
  
  ┌─────────────────────────────────────────────────────────────┐
  │  TICKET #1234                                               │
  │  ═══════════════════════════════════════════════════════════│
  │  OWNER: Agent A (immutable until resolution)                │
  │                                                              │
  │  TIMELINE:                                                   │
  │  ├── [A] Created from email                                 │
  │  ├── [A] First response to customer                         │
  │  ├── [A] Escalated to Network Team (parallel)               │
  │  │   └── [B] Network Expert: "Check VLAN config"            │
  │  │   └── [B] Network Expert: "Found issue, patch applied"   │
  │  ├── [A] Updated customer with resolution                   │
  │  └── [A] Resolved                                           │
  │                                                              │
  │  Customer sees: Agent A handled it start to finish.         │
  │  Reality: Expert B provided expertise without taking over.  │
  └─────────────────────────────────────────────────────────────┘
```

## Current Session State

```yaml
session_id: "itil-next-bootstrap"
phase: "vision-expanded"
progress: 0.2

active_agents:
  - architect (defining structure)
  
decisions_made:
  - 3 repos: orchestrator, engine, connectors
  - Parallel escalation model (envelope pattern from Sony!)
  - MS Graph for email (not SMTP/IMAP)
  - MS Teams for notifications (not email spam)
  - HubSpot patterns for timeline UX
  - Jira patterns for Kanban boards
  - Dynamic priority scoring (base × urgency multipliers)
  - First-class reminder system with SLA auto-warnings

features_confirmed:
  - Kanban board with multiple views (status, priority, team)
  - Priority/urgency matrix with auto-calculation
  - Reminder dashboard (overdue, upcoming, scheduled)
  - Notification channel switching (Teams preferred)
  - Escalation envelope (Amdocs sub-case + Sony tab UI)
  - Work queue sorted by calculated priority
  - WIP limits on Kanban columns
  - TAB-BASED envelope UI (Sony pattern - all in same view)
  - Empowerment tiers (agent €100 / team lead €500 / manager)
  - Resolution documentation (what/why/when/where)
  - Case flags (physical damage, social media, VIP, legal, repeat)
  - Calibration workflow (weekly review of edge cases)
  - Excel/CSV export for backlog management

source_systems:
  excavated:
    - KIAS 2000 / Amdocs Ensemble (sub-case pattern = our envelope)
    - Sony/Sykes/Sitel (Excel export, ownership concept)
    - Microsoft portal (3-panel, AI suggestions, activity feed)
    - Valuemation (THE ANTI-PATTERN - 15-20 clicks, ping-pong, junkyard)
  to_research:
    - Zendesk (macros, automations)
    - Freshdesk (gamification)
    - SolarWinds (monitoring integration)

anti_patterns_identified:
  - 15-20 clicks to escalate (must be 1-click)
  - Search in different place (must be in-context)
  - Email blocks editing (must be non-blocking)
  - Multi-tab wrong-customer emails (must have clear context)
  - Slow performance (must be fast/reactive)
  - TICKET PING-PONG (must enforce ownership)
  - 2nd level as junkyard (must be focused assist)

next_steps:
  - [ ] Push repos to GitHub (AdaWorldAPI or new org?)
  - [ ] Define ticket data model (engine) - MOSTLY DONE
  - [ ] Define envelope tab UI wireframes
  - [ ] Define empowerment/resolution workflow
  - [ ] MS Graph connector scaffold
  - [ ] MS Teams connector scaffold
  - [ ] Start engine implementation (Python/FastAPI)
```

## Prompts

### 🏛️ Architect Agent

When spawned, the Architect focuses on:
- Data model decisions
- API contract definitions
- Integration boundaries
- Performance considerations

Spawn with: "Ask the architect about {topic}"

### 🏺 Excavator Agent  

When spawned, the Excavator digs into source systems:
- Valuemation: ITIL process flows, ticket states
- Orchestra: Rule syntax, SLA definitions
- xSuite: UI patterns, component library
- HubSpot: API structure, timeline model

Spawn with: "Excavate {system} for {pattern}"

### 🔌 Integrator Agent

When spawned, the Integrator handles:
- MS Graph authentication flow
- HubSpot webhook setup
- Data sync strategies
- Error handling patterns

Spawn with: "Integrate {source} with {target}"

---

## Meta-AGI Hooks

This project uses the meta-agi-programming pattern:
- Learning moments captured as resonance vectors
- Concepts extracted to knowledge graph
- Cross-session persistence via ai_flow

```python
# Capture a learning moment
await meta_agi.capture_moment(
    content="Parallel escalation requires separate timeline entries with same parent",
    qualia={"novelty": 0.8, "effort": 0.4, "satisfaction": 0.9}
)
```
