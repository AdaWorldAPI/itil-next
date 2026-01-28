# ITIL-NEXT: Claude Code Project Prompt

## Setup

```bash
# Clone all repos
git clone https://github.com/AdaWorldAPI/itil-next.git
git clone https://github.com/AdaWorldAPI/itil-next-engine.git
git clone https://github.com/AdaWorldAPI/itil-next-connectors.git

# Open orchestrator as main project
cd itil-next
```

## Project Context for Claude Code

Copy this into your Claude Code system prompt or project instructions:

---

```markdown
# ITIL-NEXT Project Context

You are working on ITIL-NEXT, a modern ticket management system.

## Repositories

| Repo | Purpose | Location |
|------|---------|----------|
| `itil-next` | Orchestrator, docs, feature specs | Main project |
| `itil-next-engine` | Core Python business logic | ../itil-next-engine |
| `itil-next-connectors` | External integrations | ../itil-next-connectors |

## Core Innovation: IMMUTABLE OWNERSHIP

After a ticket is accepted, the `owner_id` CANNOT be changed.
This is the antidote to "ticket ping-pong" where nobody owns the outcome.

```python
# In OwnershipService.accept_ticket()
ticket.owner_id = agent_id  # Set ONCE, then IMMUTABLE
```

## Key Concepts

1. **Envelope** (not "escalation")
   - Owner creates envelope to get help
   - Expert works IN the envelope
   - Owner stays owner
   - Source: Amdocs "sub-case" pattern

2. **Empowerment Tiers** (Sony pattern)
   - Agent: €100 discretion
   - Team Lead: €100-500 approval
   - Manager: >€500 + calibration

3. **Alert Escalation** (SolarWinds pattern)
   - 3 conditions: NOT_ASSIGNED, NOT_UPDATED, NOT_COMPLETED
   - 3 levels: Tech → Tech+Super → Everyone
   - Business time aware

## Anti-Patterns (NEVER do these)

- ❌ 15-20 clicks to escalate (must be 1-click)
- ❌ Search in different place (everything in context)
- ❌ Email blocks editing (non-blocking preview)
- ❌ Ticket ping-pong (sacred ownership)
- ❌ 2nd level as junkyard (focused assist)

## File Locations

When working on:
- **Features/specs**: Edit `itil-next/FEATURE_MAP.md`
- **Models**: Edit `itil-next-engine/src/models/ticket.py`
- **Services**: Edit `itil-next-engine/src/services/`
- **Connectors**: Edit `itil-next-connectors/`

## Before Making Changes

1. Read relevant CLAUDE.md in the repo
2. Check FEATURE_MAP.md for context
3. Follow existing patterns
4. Update .claude/claude.md with decisions

## Commands

```bash
# Engine tests
cd ../itil-next-engine && pytest

# Type checking
mypy src/

# Format
black src/ && isort src/
```
```

---

## Quick Reference Card

```
┌─────────────────────────────────────────────────────────────────┐
│                    ITIL-NEXT QUICK REFERENCE                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  REPOS:                                                          │
│  ├── itil-next              Orchestrator, docs                  │
│  ├── itil-next-engine       Python core logic                   │
│  └── itil-next-connectors   MS Graph, Teams, HubSpot            │
│                                                                  │
│  DNA SOURCES:                                                    │
│  ├── HubSpot      Object model, timeline                        │
│  ├── Amdocs/KIAS  Envelope pattern (sub-case)                   │
│  ├── Sony/Sykes   Tab UI, empowerment, calibration              │
│  ├── SolarWinds   Alert escalation matrix                       │
│  └── Valuemation  ANTI-PATTERNS (what NOT to do)                │
│                                                                  │
│  CORE RULE:                                                      │
│  Owner is IMMUTABLE after accept()                              │
│  Escalation = Envelope (help IN), not transfer (hand OFF)       │
│                                                                  │
│  KEY SERVICES:                                                   │
│  ├── OwnershipService  Sacred ownership                         │
│  ├── EnvelopeService   Parallel assist                          │
│  ├── AlertService      Escalation matrix                        │
│  └── ResolutionService Empowerment tiers                        │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## GitHub Links

- **Orchestrator**: https://github.com/AdaWorldAPI/itil-next
- **Engine**: https://github.com/AdaWorldAPI/itil-next-engine
- **Connectors**: https://github.com/AdaWorldAPI/itil-next-connectors
