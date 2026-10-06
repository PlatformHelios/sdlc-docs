---
title: Vision & Principles
description: Why we are modernizing the engineering platform, and the principles guiding it.
weight: 1
---

| | |
|---|---|
| **Status** | Draft v0.1 |
| **Owner** | Principal Platform Engineer |
| **Related** | SDLC v1 Draft (AI-Ready SDLC) |

## Why we're doing this

Leadership has asked for four things:

1. **Faster delivery:** ship changes in days, not months
2. **Stronger audit and compliance:** controls that hold up to an examiner
3. **Safe AI adoption:** use AI to go faster without adding risk
4. **Less tech debt:** a stack we can hire for, secure, and support

Today's environment makes all four hard:

- .NET applications running directly on Windows servers
- Batch processing built on Windows Scheduled Tasks
- No containers and very little cloud
- Limited observability: we often learn about problems from users
- Build, release and approval steps are largely manual, which makes them slow and hard to evidence

None of this is unusual for a financial institution. But it means we can't solve speed, compliance and AI adoption with policy alone. **We need a modern engineering platform.**

## Vision

> **A secure, self-service engineering platform where the compliant path is also the fastest path.**
>
> Teams get from idea to production quickly. Security, testing and audit evidence are built into the platform instead of added at the end. Humans stay accountable for everything that reaches production.

## Greenfield platform, brownfield applications

We can build the **platform** from scratch: pipelines, hosting, observability, guardrails. The **applications** are a different case. They run the business today and must keep running.

So the strategy has two tracks:

| Track | What it means |
|---|---|
| **Build the paved road** | A new standard way to build, test, secure, deploy and run software. New work starts here. |
| **Migrate onto it incrementally** | Move existing apps over one by one, by risk and value. Wrap and replace pieces over time (the "strangler fig" pattern). **No big-bang rewrite.** |

## Guiding principles

1. **Paved road, not a gate.** The platform provides golden paths (standard templates and pipelines) that are easier to use than going around them. Following standards should take less effort than not.
2. **Compliance as code.** Every SDLC control is either enforced automatically or produces evidence automatically. If a control relies on someone remembering, it isn't a control.
3. **Everything as code.** Infrastructure, pipelines, policies, configuration and documentation are versioned, reviewed and repeatable.
4. **Secure by default.** Scanning, secrets management, least privilege and separation of duties are built into the golden path, not opt-in.
5. **Observable by default.** Every app ships with logs, metrics, traces and alerts from day one. We use open standards so we aren't tied to one vendor.
6. **Cloud-ready and container-first for new work.** New services are built to run anywhere. Existing apps move when it makes sense, not because of a mandate.
7. **Incremental over big-bang.** Small, reversible steps that deliver value at each stage.
8. **AI-assisted, human-accountable.** AI speeds up design, coding and testing. Approved tools only, everything traceable, and a human approves every production change.
9. **Platform as a product.** Development teams are our customers. We measure adoption and satisfaction, not just delivery.
10. **Decide on outcomes, then tools.** We choose tools against the capabilities we need, not the other way around.

## How we'll measure success

| Outcome | Measures |
|---|---|
| **Speed** | Deployment frequency, lead time for changes (DORA metrics) |
| **Stability** | Change failure rate, time to restore service (DORA metrics) |
| **Compliance** | % of SDLC controls automated; time to produce audit evidence |
| **Visibility** | % of apps with standard observability; % of incidents detected before users report them |
| **Adoption** | % of apps on the paved road; developer satisfaction |
| **Tech debt** | % of apps on supported runtimes; number of scheduled tasks retired |

We'll take a baseline during discovery so improvement can be shown.

## Roadmap at a glance

| Phase | Focus | Example outcomes |
|---|---|---|
| **0. Discover** | Current-state assessment, stakeholder interviews, app inventory, baseline metrics | Shared understanding of where we are; prioritized pain points |
| **1. Foundation** | Source control and pipeline standards, identity and secrets, observability baseline, AI tool policy | Every app built and deployed through a pipeline; basic monitoring on critical systems |
| **2. Paved road** | Golden-path templates, hosting platform, automated security and evidence, 1–2 pilot apps | First apps running end-to-end on the new platform |
| **3. Scale** | App-by-app migration, retiring scheduled tasks and legacy hosting, self-service | Most new and changed work runs on the platform |

Timelines will be set once discovery is complete.

## What we need from leadership

- **Sponsorship:** a clear mandate that new work uses the paved road
- **Partnership:** InfoSec, Risk and Audit involved from the start, so controls are designed in rather than reviewed at the end
- **Pilot teams:** one or two app teams willing to go first
- **Funding decisions:** platform and tool choices will follow the capability model and discovery
