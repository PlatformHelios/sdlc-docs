---
title: The Four Pillars of IT Operations
linkTitle: Four Pillars
description: Every production system should be Secured, Monitored, Documented and Recoverable.
weight: 2
---

> **Every production system should stand on all four pillars. If one is missing, the whole structure is at risk.**

{{< cards cols="4" >}}
  {{< card link="#1-secured" title="01 · Secured" icon="shield-check" subtitle="Who can touch it, and can we prove what they did?" >}}
  {{< card link="#2-monitored" title="02 · Monitored" icon="chart-bar" subtitle="Is it healthy right now, and how do we know?" >}}
  {{< card link="#3-documented" title="03 · Documented" icon="document-text" subtitle="Could someone new run this tomorrow?" >}}
  {{< card link="#4-recoverable" title="04 · Recoverable" icon="refresh" subtitle="If it breaks, how fast are we back, and what do we lose?" >}}
{{< /cards >}}

The pillars are the minimum bar for anything running in production, whether it is a new service on the paved road or a legacy app on a Windows server. They give teams, auditors and leadership one shared question to ask about any system: *does it stand on all four?*

## 1. Secured

**Who can touch it, and can we prove what they did?**

Security settings and controls that protect systems and data, with a trustworthy record of every change.

**Key practices**

- Least-privilege access and MFA
- Hardened security baselines
- Audit logs, retained and reviewed
- Patching and vulnerability management
- Encryption in transit and at rest

**Related:** [Security Validation](../../sdlc/security-validation/) · [Supply Chain Security](../../sdlc/supply-chain-security/) · [Change Management](../../sdlc/change-management/)

## 2. Monitored

**Is it healthy right now, and how do we know?**

Metrics, logs and traces that show how systems behave, so problems surface before users report them.

**Key practices**

- Metrics: capacity, latency, errors
- Centralized, searchable logs
- Distributed traces across services
- Actionable alerts with clear owners
- Dashboards and health checks

**Related:** [Monitoring & Audit](../../sdlc/monitoring-audit/)

## 3. Documented

**Could someone new run this tomorrow?**

Current, findable knowledge of how systems are built, configured and operated, so it doesn't live in one person's head.

**Key practices**

- Architecture and dependency diagrams
- Runbooks and standard procedures
- Configuration and asset inventory
- Change records and decision logs
- Owners and escalation contacts

**Related:** [Architecture Review](../../sdlc/architecture-review/) · [Change Management](../../sdlc/change-management/)

## 4. Recoverable

**If it breaks, how fast are we back, and what do we lose?**

Backups and resilient design that let systems survive failure and return to service within agreed targets.

**Key practices**

- Scheduled backups, restore-tested
- Defined RPO and RTO targets
- Redundancy and automatic failover
- Disaster recovery plan, exercised
- Post-incident reviews

**Related:** [Deployment](../../sdlc/deployment/)

## Using the pillars

| Where | How the pillars apply |
|---|---|
| **Discovery** | Score each app in the inventory against all four pillars to find the biggest gaps and prioritize migration. |
| **Paved road** | The golden path delivers all four by default, so new services meet the bar without extra effort. |
| **Architecture review** | A design is not approved until it shows how it will meet each pillar. |
| **Production readiness** | A system does not go live until it stands on all four. |
