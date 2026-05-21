Low-Level Design — Azure Disaster Recovery Framework


1. Purpose

This Low-Level Design provides the technical implementation detail required to build and operate the Azure Disaster Recovery environment. It expands the High-Level Design into specific configuration areas including networking, replication, identity, security, monitoring, recovery sequencing, and operational controls.


2. Regional Design

Area	Primary Region	Secondary Region
Production Workloads	Active	Passive / Standby
DR Workloads	Replicated from primary	Activated during failover
Recovery Vault	Configured for replication	Target recovery location
Monitoring	Active	Active
Security Controls	Active	Active
Backup	Enabled	Recovery capable

The secondary region should be selected based on Azure regional pairing, data residency, latency, compliance, and business risk requirements.


3. Network Design

The DR network mirrors the production network structure to reduce recovery complexity.

Core network components
Hub VNet in the primary region.
Hub VNet in the DR region.
Application spoke VNets.
Database spoke VNets.
Management subnet.
Azure Firewall or approved Network Virtual Appliance.
VPN or ExpressRoute connectivity.
Route tables and User Defined Routes.
Network Security Groups.
Private DNS zones.
Recommended subnet structure
Subnet	Purpose
AzureFirewallSubnet	Azure Firewall deployment.
GatewaySubnet	VPN / ExpressRoute gateway.
ManagementSubnet	Bastion, jump hosts, admin tooling.
AppSubnet	Application servers.
DataSubnet	Database workloads.
PrivateEndpointSubnet	Private endpoint services.
ASRSubnet	Recovered workloads where required.


4. Azure Site Recovery Configuration

Azure Site Recovery should be configured for all protected virtual machines according to workload tier.

Configuration items
Recovery Services Vault created in the appropriate region.
Replication policy defined per workload tier.
Target resource group mapped for DR workloads.
Target VNet and subnet mapped.
Managed disks replicated to the DR region.
Recovery plans created for each application group.
Boot order configured based on application dependencies.
Test failover network created separately from production DR network.


5. Recovery Plan Sequencing

A standard recovery plan should start services in the following order:

Core network services.
Domain controllers and identity services.
DNS and private DNS services.
Security services and firewalls.
Database servers.
Middleware and integration services.
Application servers.
Web and front-end services.
Monitoring and validation scripts.
Business user acceptance testing.


6. Identity and Access Control

The DR environment must use the same enterprise identity and access model as production.

Required controls
Microsoft Entra ID integration.
Role-Based Access Control for Azure resources.
Privileged Identity Management for elevated access.
Break-glass accounts for emergency access.
Conditional Access policies for administrative access.
Key Vault access policies or RBAC controls.
Audit logging for all DR operations.


7. Security Design

Security controls must remain active before, during, and after failover.

Required security components
Azure Firewall or approved NVA in both regions.
Network Security Groups on all workload subnets.
Microsoft Defender for Cloud enabled.
Log Analytics workspace integration.
Key Vault for certificates, secrets, and keys.
Disk encryption for protected workloads.
Just-in-time VM access where appropriate.
Private endpoints for supported PaaS services.


8. Monitoring and Alerting

Monitoring should cover both production and DR environments.

Required monitoring
Replication health alerts.
Backup success and failure alerts.
VM availability alerts.
Network connectivity monitoring.
Firewall and NSG flow logs.
Application health checks.
Recovery Services Vault alerts.
Azure Monitor dashboards.


9. DR Testing

DR testing must be performed without impacting production workloads.

Test activities
Test failover using isolated test network.
Validate VM boot order.
Validate application dependencies.
Confirm DNS and routing changes.
Confirm user access.
Capture actual RTO and RPO achieved.
Document gaps and remediation actions.


10. Operational Deliverables

The implementation should produce the following outputs:

Approved DR architecture diagram.
Application dependency map.
RTO/RPO tiering register.
Azure Site Recovery configuration workbook.
DR failover runbook.
DR rollback runbook.
DR test report.
Cost model.
Security control checklist.

