# ITIL-NEXT: Claude Code Orchestrator Guide

## Project Overview

ITIL-NEXT is a modern ticket management system built from excavated patterns of production systems:

| Source | Pattern Stolen |
|--------|----------------|
| **HubSpot** | Object model, associations, timeline/engagements |
| **Amdocs/KIAS** | Sub-case = Envelope (parallel assist without ownership transfer) |
| **Sony/Sykes** | Tab UI for experts, empowerment tiers, calibration workflow |
| **SolarWinds** | Alert escalation matrix (3 conditions × 3 levels) |
| **Microsoft** | 3-panel layout, AI suggestions panel |
| **Valuemation** | ANTI-PATTERNS (what NOT to do) |

## The Core Innovation

```
VALUEMATION (BAD):  "Escalate" = "Get rid of this problem"
ITIL-NEXT (GOOD):   "Escalate" = "Help me WITH this problem"
```

**Owner is IMMUTABLE after accept.** This is the antidote to ticket ping-pong.

## Repository Structure

```
itil-next/              # Orchestrator (you are here)
├── CLAUDE.md           # This file - orchestrator guide
├── FEATURE_MAP.md      # Complete feature specifications
├── .claude/claude.md   # Project context for Claude
└── docs/               # Architecture documentation

itil-next-engine/       # Core business logic (Python)
├── CLAUDE.md           # Engine-specific guide
├── src/models/         # Pydantic models
├── src/services/       # Business logic services
└── src/repositories/   # Data access (to be implemented)

itil-next-connectors/   # External integrations
├── CLAUDE.md           # Connector-specific guide
├── msgraph/            # Microsoft Graph (email, calendar)
├── hubspot/            # HubSpot CRM sync
└── teams/              # MS Teams notifications
```

## Key Concepts

### 1. Ticket Ownership (Sacred)

```python
# In OwnershipService
async def accept_ticket(ticket_id, agent_id):
    # After this, owner_id is IMMUTABLE
    ticket.owner_id = agent_id
    ticket.owner_accepted_at = datetime.utcnow()
```

**Rules:**
- `owner_id` set ONCE on `accept()`
- Cannot be changed through normal operations
- Only exceptional transfers: termination, extended leave (>30 days)
- Escalation creates ENVELOPE, not ownership transfer

### 2. Envelopes (Amdocs Sub-case)

```python
# Owner creates envelope to get help
envelope = await envelope_service.create_envelope(
    ticket_id=ticket.id,
    requested_by=owner.id,  # The owner
    team_id=network_team.id,
    reason="Need help with VLAN configuration"
)

# Expert works in envelope, owner stays owner
# Customer sees: one agent handling their issue
# Reality: multiple experts contributed
```

### 3. Alert Escalation (SolarWinds Pattern)

```
Level 1: Tech only
Level 2: Tech + Supervisor  
Level 3: Tech + Supervisor + Manager

Conditions:
- NOT_ASSIGNED: Ticket sitting in queue
- NOT_UPDATED: Ticket going stale
- NOT_COMPLETED: Approaching/past due
```

### 4. Empowerment Tiers (Sony Pattern)

```
Agent:     €100 discretion (just document)
Team Lead: €100-500 (quick approval)
Manager:   >€500 (calibration review)
```

## Working with This Codebase

### Adding a New Feature

1. **Update FEATURE_MAP.md** with specification
2. **Add models** in `itil-next-engine/src/models/`
3. **Add service** in `itil-next-engine/src/services/`
4. **Add connector** if external integration needed
5. **Update .claude/claude.md** with decisions

### Code Style

- Python 3.11+
- Pydantic for models
- Async/await throughout
- Type hints required
- Docstrings explain "why", not "what"

### Testing Philosophy

```python
# Test the business rules, not the framework
async def test_owner_cannot_be_changed():
    ticket = await service.accept_ticket(ticket_id, agent_a)
    
    with pytest.raises(OwnershipError):
        ticket.owner_id = agent_b  # This must fail
```

## Anti-Patterns to Avoid (Valuemation Lessons)

| ❌ Don't | ✅ Do |
|----------|-------|
| 15-20 clicks to escalate | 1-click envelope creation |
| Search in different place | Everything in context |
| Email blocks editing | Non-blocking preview |
| Multi-tab wrong-customer | Clear context indicators |
| Ping-pong ownership | Sacred ownership |
| 2nd level as junkyard | 2nd level as focused assist |

## Environment Setup

```bash
# Clone all repos
git clone https://github.com/OWNER/itil-next.git
git clone https://github.com/OWNER/itil-next-engine.git
git clone https://github.com/OWNER/itil-next-connectors.git

# Engine setup
cd itil-next-engine
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Run tests
pytest

# Connectors setup
cd ../itil-next-connectors
pip install -r requirements.txt
```

## Current Status

- [x] Core models defined (Ticket, Envelope, Task, Timeline)
- [x] OwnershipService implemented
- [x] EnvelopeService implemented
- [x] AlertService with escalation matrix
- [x] ResolutionService with empowerment tiers
- [ ] Repository layer (database)
- [ ] API layer (FastAPI)
- [ ] MS Graph connector
- [ ] HubSpot connector
- [ ] MS Teams connector
- [ ] Frontend (future)

## Quick Reference

```python
# Accept a ticket (ownership established)
ticket = await ownership_service.accept_ticket(ticket_id, agent_id)

# Create envelope (parallel assist)
envelope = await envelope_service.create_envelope(
    ticket_id, owner_id, "Need network help", team_id=network_team
)

# Expert accepts envelope
await envelope_service.accept_envelope(envelope_id, expert_id)

# Complete envelope
await envelope_service.complete_envelope(
    envelope_id, expert_id, "Fixed VLAN loop on switch 3"
)

# Track resolution with empowerment
resolution = await resolution_service.create_resolution(
    ticket_id, 
    what_went_wrong="Product defect",
    why_eligible="Within warranty",
    resolution_type=ResolutionType.REPLACEMENT,
    amount=150.00  # Auto-routes to team_lead approval
)
```
