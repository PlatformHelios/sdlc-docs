---
title: Tooling Evaluation
description: Capability map, criteria, and plan for evaluating SDLC tooling.
weight: 1
draft: true
---

| | |
|---|---|
| **Status** | Draft v0.1 |
| **Source** | SDLC v1 Draft (AI-Ready SDLC) |
| **Current platform** | Azure DevOps (partial adoption) |
| **Decision needed** | Platform direction + tool selection per SDLC control |

## 1. Purpose

The SDLC v1 draft defines 13 phases of controls. This document:

1. Maps each SDLC control to the tooling capability needed to **enforce it and produce audit evidence**
2. Lists candidate tools for each capability: Azure DevOps-native, GitHub-native, and third-party
3. Defines the criteria and scoring we'll use to choose
4. Proposes an evaluation plan that ends in a recommendation

The guiding principle: **a control is only real if tooling enforces it or records evidence of it automatically.** If a control depends on people remembering to do something, it will fail audit.

---

## 2. The platform decision comes first

Most tool choices depend on which core platform we use. These are the realistic options:

| Option | Description | Strengths | Weaknesses |
|---|---|---|---|
| **A. Stay on Azure DevOps** | Expand ADO usage, add GitHub Advanced Security for Azure DevOps + third-party tools | No migration. Strong Boards and work-item traceability. Mature pipeline approvals and checks. Team already knows it | Microsoft's main investment has moved to GitHub, and ADO gets only minor updates. The newest Copilot/AI features land on GitHub first |
| **B. Hybrid: Azure Boards + GitHub** | Keep Azure Boards for requirements and work tracking. Move repos and CI/CD to GitHub Enterprise Cloud | Microsoft supports this pattern. Keeps the strongest part of ADO. Native GHAS and Copilot. Lets us migrate in stages | Two platforms to administer. Traceability crosses a system boundary (work item ↔ PR linking via `AB#`) |
| **C. Full GitHub Enterprise** | Everything on GitHub (Issues/Projects for work tracking) | One platform. Where Microsoft is investing. Deepest AI tooling | GitHub Projects is weaker than Azure Boards for formal requirements and traceability. Largest migration |
| **D. GitLab Ultimate** | Single DevSecOps platform | Built-in compliance frameworks, security scanners and audit events in one product | Full migration off the Microsoft ecosystem. Least alignment with existing skills |

**Working hypothesis (to be tested, not decided):** Option A or B. Option A is lowest-risk short term. Option B is likely the better 3–5 year position. The evaluation should answer one question: **does ADO + add-ons meet every required control, and at what cost compared with migrating?**

### Inventory needed before deciding

- [ ] Azure DevOps **Services** (cloud) or Azure DevOps **Server** (on-prem)? Some add-ons, including GitHub Advanced Security for ADO, are cloud-only. Verify for each tool.
- [ ] Which ADO modules are in use: Boards, Repos, Pipelines (classic or YAML), Artifacts, Test Plans
- [ ] Number of projects, repos, pipelines, and active committers (drives licensing)
- [ ] Current branch policies and pipeline approvals in place
- [ ] Current licensing and renewal dates (ADO, Microsoft EA, any security tools)
- [ ] Existing ITSM/change tool (e.g., ServiceNow) and SIEM (e.g., Sentinel, Splunk)
- [ ] AI tools developers already use, sanctioned or not

---

## 3. Capability map: SDLC control → tooling

Legend: **ADO** = Azure DevOps native or add-on, **GH** = GitHub native, **3P** = third-party.

### 3.1 Plan (Phases 1–2)

| SDLC control | Capability needed | ADO | GH | 3P |
|---|---|---|---|---|
| Requirements, risk assessment, data classification | Work tracking with custom fields (system risk tier, data class) | Azure Boards (custom process) | Issues + Projects (custom fields) | Jira, ServiceNow SPM |
| Traceability: requirement → code → test → release | End-to-end linking | Boards ↔ Repos ↔ Pipelines (native) | Issues ↔ PRs ↔ deployments | Jira + integration |
| Architecture Review Board approvals | Recorded approval workflow | Boards work item states + required approvers | Issue forms + approvals | ServiceNow, Confluence |
| Architecture diagrams | Diagrams stored as code, versioned | Repo + Wiki (Mermaid) | Repo (Mermaid) | Structurizr, Lucid |
| Threat modeling *(gap in draft)* | Repeatable threat model | — | — | Microsoft Threat Modeling Tool, OWASP Threat Dragon, IriusRisk |

### 3.2 Build (Phases 3–4)

| SDLC control | Capability needed | ADO | GH | 3P |
|---|---|---|---|---|
| Approved AI models only | Enterprise AI assistant with admin policy, SSO, no training on our data | Copilot works in IDE against ADO repos | Copilot Business/Enterprise (deepest integration) | Claude Code (Enterprise), Cursor (Business/Enterprise), internal LLM via Azure AI Foundry |
| Prompt logging | Export of prompts, responses, tool calls to our log store | — | Check Copilot session export to an audit destination | Claude Enterprise Compliance API / data export + OpenTelemetry for Claude Code |
| AI code attribution | Mark AI-generated changes | PR template checkbox + commit trailer | PR template + commit trailer | — |
| Peer review cannot be bypassed | Enforced reviewers | Branch policies: min reviewers, "prohibit most recent pusher from approving", reset on new push | Rulesets: required reviews, dismiss stale approvals, CODEOWNERS | — |
| Security/compliance review | Required reviewers by path or area | Branch policy: required reviewers by path | CODEOWNERS | — |
| Linked work item required | Every change traced to a request | Branch policy: require linked work item | Ruleset + `AB#` or issue-link check | — |

**AI tooling note:** Prompt logging is the hardest control in the draft. Vendors have been adding session export, but coverage varies by tool, plan tier, and client (IDE, CLI, web). Each AI tool must show **what is logged, where it goes, and how long it is kept** before we approve it. Treat vendor claims as unverified until a proof of concept confirms them.

### 3.3 Verify (Phases 5–9)

| SDLC control | Capability needed | ADO | GH | 3P |
|---|---|---|---|---|
| Unit / integration / regression testing, 80–90% coverage | Test execution + coverage gate in CI | Pipelines + coverage publishing; Test Plans for manual/UAT | Actions + coverage reporting | SonarQube quality gates, Codecov |
| SAST | Source scanning, blocking on severity | GitHub Code Security for ADO (CodeQL) | GitHub Code Security (CodeQL) | SonarQube, Checkmarx One, Veracode, Semgrep |
| Dependency scanning (SCA) | Vulnerable/licensed OSS detection | GitHub Code Security for ADO (dependency scanning) | Dependabot | Snyk, Mend, OWASP Dependency-Check |
| SBOM | SBOM generated per build (CycloneDX/SPDX) | Pipeline task (Microsoft `sbom-tool`) | Dependency graph export / Action | Syft, CycloneDX tools, Snyk, Mend |
| Secrets scanning + push protection *(gap in draft)* | Block secrets before they're committed | GitHub Secret Protection for ADO | GitHub Secret Protection | GitGuardian, gitleaks |
| IaC + container scanning *(gap in draft)* | Misconfig and image vulnerability scanning | Defender for Cloud DevOps security | Defender for Cloud / GHAS | Trivy, Checkov, Wiz, Prisma Cloud |
| DAST | Scan running apps and APIs | Pipeline task | Action | OWASP ZAP, Burp Suite Enterprise, Invicti, StackHawk |
| Pen testing | Scheduled by risk tier | — | — | Third-party firm (service) + finding tracker |
| AI red teaming | Prompt injection, jailbreak, data leakage tests | — | — | Microsoft PyRIT, garak, promptfoo, plus a service firm |
| Unified security view | One place for findings across tools | Defender for Cloud DevOps security | GHAS security overview | ASPM tools (e.g., Wiz, Apiiro, Snyk AppRisk) |

### 3.4 Release & Operate (Phases 10–13)

| SDLC control | Capability needed | ADO | GH | 3P |
|---|---|---|---|---|
| UAT / business sign-off | Recorded business approval | Test Plans + environment approval | Environment required reviewers | ServiceNow |
| Change record + CAB | Change ticket linked to the release, with evidence attached | Pipelines ServiceNow change-management check | Actions + ServiceNow DevOps integration | ServiceNow, Jira Service Management |
| Emergency change path *(gap in draft)* | Fast path with after-the-fact review | Separate pipeline approval policy | Separate environment rules | ServiceNow emergency change |
| Separation of duties | Developer can't approve own prod deploy | Environment approvals + checks; self-approval blocked | Environment required reviewers; prevent self-review | — |
| Deployment validation | Config, secrets, logging checks | Pipeline gates + Azure Key Vault | Environment protection rules + Key Vault/OIDC | — |
| Artifact integrity | Signed, immutable build artifacts | Azure Artifacts | GitHub Packages + artifact attestations | JFrog Artifactory, Sigstore/cosign |
| Monitoring | Errors, security events, user activity, AI interactions | Azure Monitor / App Insights | — | Microsoft Sentinel, Splunk, Datadog |
| Platform audit logs | Who changed policies, permissions, pipelines | ADO auditing + streaming to Log Analytics/Splunk | Enterprise audit log streaming | SIEM |
| **7-year evidence retention** | Immutable evidence store | — | — | Azure Blob immutable (WORM) storage, GRC tool |

**Evidence retention note:** Neither ADO nor GitHub keeps pipeline runs, audit logs, or scan results for 7 years by default. Whichever platform we choose, we need an **evidence pipeline**: each release exports its approvals, test results, scan results, SBOM, and change record to immutable storage. This should be designed once and work for any platform.

---

## 4. Evaluation criteria

Each tool is scored 1–5 on every criterion. The weighted total drives selection.

| # | Criterion | Weight | What "5" looks like |
|---|---|---|---|
| 1 | **Control coverage** | 25% | Enforces the control (blocks), not just reports |
| 2 | **Audit evidence** | 20% | Exportable, timestamped evidence that ties to work item, commit, and release |
| 3 | **Platform integration** | 15% | Native in ADO and GitHub, so we aren't locked in either way |
| 4 | **Vendor & data risk** | 15% | SOC 2 Type II, SSO/SCIM, data residency, contractual no-training, passes our third-party risk review |
| 5 | **Developer experience** | 10% | Fast, low false positives, findings shown in the PR |
| 6 | **3-year total cost** | 10% | Licensing + implementation + admin effort |
| 7 | **Operational burden** | 5% | SaaS or low maintenance |

**Hard requirements (pass/fail before scoring):**

- SSO with the corporate identity provider
- Passes vendor risk management and InfoSec review
- For AI tools: contractual no-training on our data, admin-enforced policy, and a verified prompt/session logging path

---

## 5. Evaluation plan

| Step | Activities | Output |
|---|---|---|
| **1. Requirements** | Finalize the risk-tier control matrix (which controls apply to Critical, High, Moderate, Low). Complete the inventory in §2 | Approved control matrix + current-state inventory |
| **2. Platform analysis** | Gap analysis of ADO against the capability map. Migration cost estimate for Option B | Platform recommendation (A, B, C or D) |
| **3. Shortlist** | 2–3 candidates per capability, after hard-requirement screening | Shortlist |
| **4. Proof of concept** | Pilot on 2 real apps (one High, one Moderate). Build a reference pipeline with all gates and evidence export | Working reference pipeline + scored results |
| **5. Recommendation** | Weighted scores, 3-year cost, rollout plan | Decision package for ARB / InfoSec / leadership |

**Suggested priority:** if budget forces phasing, start with the controls an examiner will test first:

1. Branch policies and separation of duties (free, already in ADO)
2. Secrets scanning + SAST + SCA/SBOM
3. Change record linkage + evidence retention
4. AI tool approval + logging
5. DAST, IaC scanning, AI red teaming

---

## 6. Open questions

1. Is ADO **Services** or **Server**?
2. Is there an existing enterprise agreement with Microsoft/GitHub, or existing licenses for SonarQube, Snyk, Checkmarx, etc.?
3. Which ITSM tool runs CAB today?
4. Which AI coding tools are developers already using, and on which plans?
5. Who owns the budget and the final decision: Architecture Review Board, CISO, or CIO?
6. Are there data-residency requirements that limit SaaS options?

---

## Sources

- [Directions on Microsoft – Azure DevOps roadmap](https://www.directionsonmicrosoft.com/roadmaps/ref/azure-devops/)
- [Microsoft DevOps Blog – GitHub Secret Protection and Code Security for Azure DevOps](https://devblogs.microsoft.com/devops/github-secret-protection-and-github-code-security-for-azure-devops/)
- [GitHub Changelog – Introducing GitHub Secret Protection and GitHub Code Security](https://github.blog/changelog/2025-03-04-introducing-github-secret-protection-and-github-code-security/)
- [GitHub Docs – Reviewing audit logs for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/review-audit-logs)
- [RunReveal – Anthropic Compliance Activity Logs](https://docs.runreveal.com/sources/source-types/anthropic-compliance)

*Vendor pricing and features change often. Confirm everything in this document with vendors before relying on it.*
