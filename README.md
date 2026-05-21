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
<img width="468" height="445" alt="image" src="https://github.com/user-attachments/assets/12c14867-ce52-428e-b9e3-bd7fc1370eeb" />
