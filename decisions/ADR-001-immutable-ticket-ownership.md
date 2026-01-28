# ADR-001: Immutable Ticket Ownership

## Status

**Accepted**

## Context

Traditional ticket systems allow ownership transfer ("escalation" = handoff). This creates:
- Customer bouncing between agents
- Context loss at each transfer
- Diffused accountability
- Agents disengaging after handoff

We need a model that maintains accountability while still allowing expertise to flow to tickets.

## Decision

**Ticket ownership is immutable after acceptance.**

Once an agent accepts a ticket, they own it until resolution. The `owner_id` field cannot be changed through normal operations.

### Escalation ≠ Transfer

Escalation creates a **parallel assist thread**, not a transfer of ownership:

```
Agent A (owner) → requests help from Network Team
  └── Expert B joins ticket as ASSIST
  └── Expert B contributes to escalation thread
  └── Expert B completes their assist
  └── Agent A (still owner) continues to resolution
```

### Transfer Exceptions

The ONLY scenarios where ownership can change:

1. **Agent termination** - Employee leaves the company
2. **Extended leave** - Agent unavailable >30 days (medical, sabbatical)

These require system/admin override, not agent action.

## Consequences

### Good

- Single point of accountability for customer
- Context never lost (owner has full history)
- Agents invested in outcome
- Customer relationship maintained
- Clear metrics (owner = accountable for resolution)

### Bad

- Agent stuck with difficult tickets (mitigated by parallel assist)
- Capacity imbalance possible (mitigated by routing improvements)
- Requires culture shift from "escalate to get rid of"

### Risks

- Agent burnout if assists are slow
- Gaming possible (avoid accepting tough tickets)
- Edge cases: what if expert knows more?

### Mitigations

- SLA on assist response time
- Visibility into queue before accept
- Assist contributions visible in metrics
- Expert can advise, owner decides

## Implementation

```python
class Ticket:
    owner_id: UUID  # Set on accept(), immutable
    
class OwnershipService:
    async def accept(self, ticket_id, agent_id) -> Ticket:
        ticket = await self.repo.get(ticket_id)
        if ticket.owner_id:
            raise TicketAlreadyOwnedError()
        ticket.owner_id = agent_id
        return await self.repo.save(ticket)
    
    async def transfer(self, ticket_id, new_owner_id, reason: TransferReason):
        if reason not in [TransferReason.AGENT_TERMINATED, 
                          TransferReason.EXTENDED_LEAVE]:
            raise InvalidTransferReasonError()
        # ... admin override logic
```

## Related Decisions

- ADR-002: Parallel Escalation Model (to be written)
- ADR-003: Timeline Visibility Scoping (to be written)

## References

- ITIL v4: Value streams over silos
- HubSpot: Deal ownership model
- Vision document: `VISION.md`
