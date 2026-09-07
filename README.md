# Customer Support Ticket Routing Network

## Overview

Customer Support Ticket Routing Network is an AAOSA (Agent-to-Agent Orchestration) multi-agent system built using NeuroSAN Studio.

The network automates customer support operations by validating customer requests, classifying issues, routing tickets to support teams, escalating critical incidents, and sending customer notifications.

The system uses a Frontman Agent called **CustomerSupportCoordinator** which serves as the single customer-facing agent while coordinating specialized downstream agents behind the scenes.

---

# Business Problem

Customer support organizations often face challenges such as:

- Manual ticket triage
- Incorrect issue categorization
- Delayed support assignment
- SLA violations
- Escalation delays
- Inconsistent customer communication

These challenges increase operational effort and reduce customer satisfaction.

---

# Solution

The Customer Support Ticket Routing Network automates the ticket handling lifecycle through coordinated AI agents.

## Key Capabilities

- Customer Validation
- Ticket Classification
- Intelligent Routing
- SLA-Based Escalation
- Automated Notifications
- Enterprise System Integration

---

# Network Architecture

```text
Customer
(Email / Chat / Web Portal)
            │
            ▼
┌─────────────────────────────────┐
│  CustomerSupportCoordinator     │
│        Frontman Agent           │
└──────────────┬──────────────────┘
               │
               ▼
┌─────────────────────────────────┐
│      Ticket Intake Agent        │
│                                 │
│ validate_customer.py            │
│ customer_db_mcp                 │
└──────────────┬──────────────────┘
               │
               ▼
┌─────────────────────────────────┐
│     Classification Agent        │
│                                 │
│ classify_ticket.py              │
│ knowledge_base_mcp              │
└──────────────┬──────────────────┘
               │
               ▼
┌─────────────────────────────────┐
│         Routing Agent           │
│                                 │
│ route_ticket.py                 │
│ servicenow_mcp                  │
└───────┬───────────────────┬─────┘
        │                   │
        ▼                   ▼

 High/Urgent          Low/Medium

┌─────────────────┐
│ Escalation Agent│
│                 │
│escalate_ticket.py
│servicenow_mcp   │
└────────┬────────┘
         │
         ▼

┌─────────────────┐
│Notification Agent│
│                 │
│send_notification.py
│email_mcp        │
└────────┬────────┘
         │
         ▼

 Customer Updated
```

---

# AAOSA Agent Hierarchy

```text
CustomerSupportCoordinator
│
├── Ticket Intake Agent
├── Classification Agent
├── Routing Agent
├── Escalation Agent
└── Notification Agent
```

The CustomerSupportCoordinator is the only customer-facing agent.

All other agents operate internally and collaborate through AAOSA orchestration.

---

# Agent Responsibilities

## 1. CustomerSupportCoordinator

### Role

Frontman Agent

### Responsibilities

- Receive customer requests
- Coordinate specialist agents
- Manage workflow execution
- Return final customer response

### Customer Visibility

Yes

---

## 2. Ticket Intake Agent

### Role

Ticket Validation

### Responsibilities

- Validate customer information
- Validate support request
- Gather missing information
- Retrieve customer information

### Tools

```text
validate_customer.py
customer_db_mcp
```

### Outputs

```text
Validated Ticket
Customer Profile
```

---

## 3. Classification Agent

### Role

Issue Classification

### Responsibilities

- Determine issue category
- Assign business priority
- Identify urgency

### Tools

```text
classify_ticket.py
knowledge_base_mcp
```

### Categories

```text
Billing
Technical
Account
General Inquiry
```

### Priorities

```text
Low
Medium
High
Urgent
```

---

## 4. Routing Agent

### Role

Ticket Assignment

### Responsibilities

- Route tickets to support teams
- Create ServiceNow incidents
- Determine escalation requirements

### Tools

```text
route_ticket.py
servicenow_mcp
```

### Escalation Triggers

```text
Production Outage
Account Lockout
Business Critical Incident
Security Incident
Urgent Customer Impact
```

---

## 5. Escalation Agent

### Role

Critical Incident Management

### Responsibilities

- Apply SLA policies
- Escalate incidents
- Assign specialists
- Track critical issues

### Tools

```text
escalate_ticket.py
servicenow_mcp
```

---

## 6. Notification Agent

### Role

Customer Communication

### Responsibilities

- Send acknowledgements
- Send routing updates
- Send escalation updates
- Send resolution notifications

### Tools

```text
send_notification.py
email_mcp
```

---

# Tool Inventory

## Validation Tool

```text
validate_customer.py
```

Purpose:

- Customer validation
- Request completeness verification

---

## Classification Tool

```text
classify_ticket.py
```

Purpose:

- Category assignment
- Priority determination

---

## Routing Tool

```text
route_ticket.py
```

Purpose:

- Support queue selection
- Assignment logic

---

## Escalation Tool

```text
escalate_ticket.py
```

Purpose:

- SLA enforcement
- Escalation handling

---

## Notification Tool

```text
send_notification.py
```

Purpose:

- Customer communication
- Status updates

---

# MCP Integrations

## Customer Database MCP

```text
customer_db_mcp
```

Provides:

- Customer lookup
- Customer verification

---

## Knowledge Base MCP

```text
knowledge_base_mcp
```

Provides:

- Knowledge retrieval
- Product support guidance

---

## ServiceNow MCP

```text
servicenow_mcp
```

Provides:

- Incident creation
- Ticket updates
- Escalation tracking

---

## Email MCP

```text
email_mcp
```

Provides:

- Email communication
- Customer notification delivery

---

# End-to-End Workflow

## Customer Input

```text
My production account is locked and business operations are blocked.
```

## Execution Flow

```text
CustomerSupportCoordinator
        ↓
Ticket Intake Agent
        ↓
Classification Agent
        ↓
Routing Agent
        ↓
Escalation Agent
        ↓
Notification Agent
```

## Classification Result

```text
Category: Account
Priority: Urgent
```

## Final Outcome

```text
Support Queue: Account Support
Escalated: Yes
Customer Notified: Yes
```

---

# Test Scenarios

## Billing Request

Input:

```text
I need a copy of last month's invoice for customer CUST-1001.
```

Expected:

```text
Category: Billing
Priority: Low
Escalation: No
```

---

## Technical Outage

Input:

```text
The production application is unavailable for all users.
```

Expected:

```text
Category: Technical
Priority: Urgent
Escalation: Yes
```

---

## Account Lockout

Input:

```text
My production account is locked and business operations are blocked.
```

Expected:

```text
Category: Account
Priority: Urgent
Escalation: Yes
```

---

# Hackathon Demonstration

## Step 1

Open the network:

```text
customer_support