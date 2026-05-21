# Azure Disaster Recovery Framework (Enterprise)

This repository contains an enterprise-grade DR framework aligned to the Azure Well-Architected Framework.
It includes reference architecture, RTO/RPO tiering, runbooks, and cost-model templates.

## What’s inside
- Reference DR architecture (multi-region)
- RTO/RPO tiering model
- DR test plan + failover/failback runbooks
- Cost model template for compute, storage, replication, network

## Diagram (Mermaid)
```mermaid
flowchart LR
  U[Users] --> DNS[DNS/Traffic Manager]
  DNS --> P[Primary Region]
  DNS --> S[Secondary Region]

  subgraph P[Primary Region]
    FW1[Firewall / NVA]
    HUB1[Hub VNet]
    SPOKE1[Spoke VNets: Apps + Data]
    ASR1[ASR / Replication]
    FW1 --> HUB1 --> SPOKE1 --> ASR1
  end

  subgraph S[Secondary Region]
    FW2[Firewall / NVA]
    HUB2[Hub VNet]
    SPOKE2[Spoke VNets: Apps + Data]
    FW2 --> HUB2 --> SPOKE2
  end

  EXR[ExpressRoute / VPN] --> HUB1
  EXR --> HUB2
  ASR1 --> SPOKE2
How to use
Start with:
1.	architecture/high-level-design.md
2.	architecture/rto-rpo-tiering.md
3.	runbooks/dr-test-plan.md
Author
Remi Adejumo

### `architecture/high-level-design.md`
```markdown
# High-Level Design — Azure DR Framework

## Objectives
- Define a repeatable enterprise DR approach for hybrid workloads
- Standardize tiering (RTO/RPO), replication, failover orchestration, and DR testing

## Core Principles
- **Tiered resilience**: not all workloads require the same RTO/RPO
- **Automation-first**: failover/failback steps documented + scripted where possible
- **Security preserved during DR**: identity, segmentation, logging, and secrets remain enforced
- **Governance**: policy, RBAC, tagging, and cost attribution applied consistently

## Reference Components
- Networking: Hub-and-Spoke, NVA/Firewall, DNS/Traffic Manager
- Replication: Azure Site Recovery (or storage replication patterns where applicable)
- Identity: Entra ID + least privilege + break-glass accounts
- Observability: Log Analytics / SIEM + health checks
- Recovery Orchestration: runbooks + test cadence

## DR Operating Model
- Owner: Service / Application team
- DR Platform: Cloud Platform team
- Security: controls, exceptions, validation
- Compliance: evidence capture (test results, change records)

## Evidence Pack (for audits)
- DR test plan + results
- RTO/RPO mapping
- Runbook approval history
- Monitoring + alerting screenshots
<img width="468" height="637" alt="image" src="https://github.com/user-attachments/assets/b7a424b4-2e65-4397-bead-5f1122e90f23" />

