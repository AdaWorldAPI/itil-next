# ITIL-NEXT Roadmap

## Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    DELIVERY PHASES                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   PHASE 1: Foundation     ████████░░░░░░░░░░░░  ~4 weeks        │
│   PHASE 2: Intelligence   ░░░░░░░░░░░░░░░░░░░░  ~4 weeks        │
│   PHASE 3: Experience     ░░░░░░░░░░░░░░░░░░░░  ~4 weeks        │
│   PHASE 4: Ecosystem      ░░░░░░░░░░░░░░░░░░░░  ~4 weeks        │
│                                                                  │
│   MVP at end of Phase 1: Ticket + Email + Parallel Escalation   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Phase 1: Foundation (Weeks 1-4)

**Goal:** Working ticket system with email integration and parallel escalation.

### Week 1: Core Models

- [ ] **Engine:** Ticket aggregate root
  - `models/ticket.py` - Ticket, TimelineEntry
  - `models/agent.py` - Agent with capacity
  - `models/escalation.py` - Parallel escalation model
  
- [ ] **Engine:** Core services
  - `services/ownership.py` - Sacred ownership logic
  - `services/escalation.py` - Parallel assist creation
  
- [ ] **Database:** PostgreSQL schema
  - Migrations setup (Alembic)
  - Initial tables

### Week 2: MS Graph Integration

- [ ] **Connectors:** MS Graph client
  - `msgraph/client.py` - Auth, token management
  - `msgraph/email.py` - Read, list, send
  
- [ ] **Connectors:** Webhook setup
  - `msgraph/webhooks.py` - Subscription, renewal
  - Endpoint for receiving notifications
  
- [ ] **Engine:** Email processing
  - `services/email_service.py` - Incoming email → ticket
  - Thread linking via conversationId

### Week 3: API & Basic UI

- [ ] **Engine:** REST API
  - `api/tickets.py` - CRUD, status transitions
  - `api/escalations.py` - Create, complete
  - `api/timeline.py` - Add entries
  
- [ ] **UI:** Minimal interface
  - Ticket list view
  - Ticket detail with timeline
  - Escalation panel
  - Reply composer

### Week 4: Integration & Testing

- [ ] **End-to-end flow**
  - Email → Ticket → Escalate → Reply → Resolve
  
- [ ] **Testing**
  - Unit tests for services
  - Integration tests for Graph
  - E2E smoke tests
  
- [ ] **Deployment**
  - Railway setup (or similar)
  - Environment configuration
  - Monitoring basics

**Phase 1 Deliverable:** Working MVP with email-based ticket creation, parallel escalation, and reply capability.

---

## Phase 2: Intelligence (Weeks 5-8)

**Goal:** Smart routing, SLA enforcement, and automation rules.

### Week 5: SLA Engine

- [ ] **Engine:** SLA model
  - `models/sla.py` - SLA definitions
  - `services/sla_service.py` - Timer, breach detection
  
- [ ] **Rules:** SLA assignment
  - Based on ticket type, priority, customer tier
  
- [ ] **UI:** SLA indicators
  - Time remaining badge
  - Breach warnings

### Week 6: Rules Engine (Orchestra Pattern)

- [ ] **Engine:** Rules framework
  - `rules/engine.py` - Rule evaluation
  - `rules/actions.py` - Available actions
  - `rules/conditions.py` - Condition types
  
- [ ] **Rules:** Initial ruleset
  - Auto-route critical incidents
  - Escalation timeout warnings
  - Auto-assign based on skills

### Week 7: Smart Routing

- [ ] **Engine:** Routing service
  - `services/routing.py` - Assignment logic
  - Skill matching
  - Capacity awareness
  
- [ ] **Engine:** Queue management
  - Unassigned ticket queue
  - Team workload view

### Week 8: Notifications & Alerts

- [ ] **Engine:** Notification service
  - `services/notifications.py`
  - Email alerts for agents
  - Escalation notifications
  
- [ ] **Monitoring:** Operational dashboards
  - Open tickets by team
  - SLA compliance
  - Escalation rates

**Phase 2 Deliverable:** Intelligent routing, SLA enforcement, rules-based automation.

---

## Phase 3: Experience (Weeks 9-12)

**Goal:** Polished UI with xSuite patterns, mobile support.

### Week 9: UI Overhaul

- [ ] **UI:** xSuite patterns
  - Component library implementation
  - Design system (colors, typography)
  - Responsive layout
  
- [ ] **UI:** Enhanced timeline
  - Rich email rendering
  - Inline attachments
  - Threaded escalations

### Week 10: Agent Experience

- [ ] **UI:** Agent dashboard
  - My tickets view
  - Quick actions
  - Keyboard shortcuts
  
- [ ] **UI:** Productivity features
  - Canned responses
  - Templates
  - Bulk actions

### Week 11: HubSpot Integration

- [ ] **Connectors:** HubSpot client
  - `hubspot/client.py`
  - `hubspot/contacts.py`
  
- [ ] **Engine:** Contact enrichment
  - Customer tier lookup
  - Company context
  - History summary

### Week 12: Mobile & Polish

- [ ] **UI:** Mobile app (PWA)
  - Core ticket operations
  - Push notifications
  
- [ ] **Polish:** UX refinements
  - Loading states
  - Error handling
  - Accessibility audit

**Phase 3 Deliverable:** Beautiful, productive agent experience with CRM context.

---

## Phase 4: Ecosystem (Weeks 13-16)

**Goal:** Full ITIL integration, enterprise features.

### Week 13: CMDB Integration

- [ ] **Connectors:** Valuemation CMDB
  - Asset lookup
  - CI linking to tickets
  
- [ ] **Engine:** Asset context
  - Affected CI on ticket
  - Dependency view

### Week 14: Change Management

- [ ] **Engine:** Change model
  - `models/change.py`
  - Approval workflow
  - CAB integration
  
- [ ] **Links:** Incident → Change
  - Create change from incident
  - Track implementation

### Week 15: Problem Management

- [ ] **Engine:** Problem model
  - `models/problem.py`
  - Root cause tracking
  
- [ ] **Links:** Incident → Problem
  - Link recurring incidents
  - Known error database

### Week 16: Enterprise Features

- [ ] **Reporting:** Analytics
  - Custom dashboards
  - Export capabilities
  
- [ ] **Admin:** Configuration
  - Rule editor UI
  - SLA configuration
  - Team management

**Phase 4 Deliverable:** Complete ITIL-aligned service management platform.

---

## Success Criteria

### Phase 1 (MVP)
- [ ] Create ticket from email
- [ ] Agent accepts and owns ticket
- [ ] Create parallel escalation
- [ ] Expert contributes without taking ownership
- [ ] Reply to customer (email out)
- [ ] Resolve ticket

### Phase 2 (Intelligence)
- [ ] SLA timers active
- [ ] Auto-routing working
- [ ] Rules engine processing
- [ ] Alerts firing

### Phase 3 (Experience)
- [ ] Mobile access
- [ ] HubSpot context visible
- [ ] Keyboard shortcuts
- [ ] <3s page loads

### Phase 4 (Ecosystem)
- [ ] CMDB linked
- [ ] Change workflow live
- [ ] Problem tracking active
- [ ] Reporting available

---

## Risk Log

| Risk | Impact | Mitigation |
|------|--------|------------|
| MS Graph rate limits | High | Caching, batching, queue |
| Valuemation API gaps | Medium | Direct DB if needed |
| Parallel escalation confusion | Medium | Clear UI, training |
| Data migration complexity | High | Phased rollout, parallel run |
