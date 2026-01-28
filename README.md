# ITIL-NEXT

> The ticket system that thinks like a person, not a form processor.

## Core Principle

**Agent = OWNER** (sacred bond until resolved)  
**Escalation = PARALLEL ASSIST** (expertise joins, not replaces)

## Repository Structure

This is the **orchestrator repository** containing:
- Vision and strategy documents
- Agent prompts for AI-assisted development
- Architecture Decision Records (ADRs)
- Project coordination files

### Related Repositories

| Repository | Purpose |
|------------|---------|
| `itil-next` | Orchestrator (this repo) |
| `itil-next-engine` | Core ticket logic, ownership, escalation |
| `itil-next-connectors` | MS Graph, HubSpot, CMDB integrations |

## Quick Links

- [Vision](./VISION.md) - Why we're building this
- [Roadmap](./ROADMAP.md) - Phased delivery plan
- [Decisions](./decisions/) - Architecture Decision Records
- [Prompts](./prompts/) - AI agent definitions

## The Parallel Escalation Model

```
TRADITIONAL (broken):
  Agent A → escalates → Agent B takes over
  Customer bounced, context lost

ITIL-NEXT:
  Agent A → escalates → Expert B joins as ASSIST
  Agent A stays owner, Expert B contributes
  Customer sees one face throughout
```

## Development

This project uses the **meta-agi-programming** pattern:
- `.claude/claude.md` files maintain context across sessions
- Learning moments captured as resonance vectors
- Multi-agent prompts for specialized tasks

### Agent Prompts

- 🏛️ **Architect** - Data models, API contracts, system boundaries
- 🏺 **Excavator** - Source system analysis, pattern discovery
- 🔌 **Integrator** - External system connections, error handling

## License

Proprietary - DATAGROUP SE
