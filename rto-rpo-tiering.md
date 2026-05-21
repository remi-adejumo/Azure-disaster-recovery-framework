RTO/RPO Tiering Model — Azure Disaster Recovery Framework

1. Purpose

This document defines the Recovery Time Objective and Recovery Point Objective tiering model for applications protected by the Azure Disaster Recovery Framework. The purpose is to classify workloads by business criticality and align each workload to the appropriate recovery architecture, replication method, operational priority, and cost model.

2. Definitions

Term	Meaning
RTO	Recovery Time Objective. The maximum acceptable time to restore a service after an outage.
RPO	Recovery Point Objective. The maximum acceptable amount of data loss measured in time.
Criticality	Business importance of the application or service.
Recovery Tier	Classification used to determine DR design and recovery priority.

3. Tiering Model

ier	Business Criticality	Target RTO	Target RPO	Recovery Pattern	Example Workloads
Tier 0	Mission Critical	0–30 minutes	0–5 minutes	Hot standby / active-active	Identity, core payments, customer portals, security operations.
Tier 1	Business Critical	1–4 hours	15–60 minutes	Warm standby / ASR replication	ERP, CRM, finance systems, production databases.
Tier 2	Important	4–24 hours	4–12 hours	Pilot light / backup restore	Departmental applications, reporting systems, internal tools.
Tier 3	Standard	24–72 hours	24 hours	Cold recovery / backup restore	Non-critical systems, archives, dev/test workloads.
Tier 4	Low Priority	Best effort	Best effort	Rebuild when required	Sandbox, temporary environments, non-production labs.

4. Application Classification Criteria

Applications should be assessed using the following criteria:

Revenue impact if unavailable.
Customer impact if unavailable.
Regulatory or compliance impact.
Operational dependency on the application.
Data sensitivity.
Number of users affected.
Upstream and downstream dependencies.
Manual workaround availability.
Cost of downtime.
Technical complexity of recovery.

5. Recovery Priority

During a major incident, workloads should be recovered in this order:

Identity and access services.
Core network and security services.
Monitoring and management services.
Tier 0 applications.
Tier 1 applications.
Tier 2 applications.
Tier 3 applications.
Tier 4 applications.

6. Tier-to-Technology Mapping

Tier	Recommended Azure Services
Tier 0	Multi-region architecture, active-active design, Azure Front Door, geo-replicated databases, automated failover.
Tier 1	Azure Site Recovery, Recovery Services Vault, warm standby infrastructure, automated recovery plans.
Tier 2	Azure Backup, infrastructure-as-code redeployment, selective ASR replication.
Tier 3	Backup restore, Terraform rebuild, manual recovery process.
Tier 4	Rebuild from source control or image templates.

7. RTO/RPO Register Template

Application	Owner	Business Function	Tier	RTO	RPO	Recovery Method	Dependencies	Validation Owner
Example CRM	Sales	Customer management	Tier 1	4 hrs	1 hr	ASR failover	SQL, Entra ID, DNS	Head of Sales
Example Payroll	HR	Payroll processing	Tier 2	24 hrs	12 hrs	Backup restore	Database, file share	HR Manager

8. Review Frequency

The RTO/RPO tiering model should be reviewed:

At least twice per year.
After any major application change.
After DR testing.
After a business impact assessment update.
After onboarding new critical workloads.
