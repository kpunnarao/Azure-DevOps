# Azure DevOps Services versus Azure DevOps Server

> Chapter 1 — DevOps Principles and Azure DevOps Architecture

[← Previous](03-continuous-integration-delivery-and-deployment.md) · [Chapter home](README.md) · [Next →](05-organizations-projects-teams-and-resources.md)

## Purpose

Azure DevOps is available as a Microsoft-hosted cloud service and as a customer-managed server product. The feature names are similar, but responsibility, identity, networking, upgrades, resilience, and operating cost differ significantly.

## High-level comparison

| Dimension | Azure DevOps Services | Azure DevOps Server |
|---|---|---|
| Hosting | Microsoft cloud | Customer-managed infrastructure |
| Top-level collaboration scope | Organization | Server/collection |
| Upgrades | Continuous service updates | Planned and executed by customer |
| Identity | Microsoft Entra ID or Microsoft accounts | Active Directory and supported identity configuration |
| Availability platform | Operated by Microsoft | Designed and operated by customer |
| Internet dependency | Cloud connectivity required | Can support isolated/on-premises networks |
| Feature delivery | New capabilities generally arrive earlier | Features arrive through server releases |
| Capacity operations | Service limits and purchased capacity | Infrastructure sizing and SQL/server administration |
| Customization | Cloud-supported models | Additional on-premises options may exist |
| Cost model | Service licensing and consumption | Licenses plus infrastructure and operations |

Always verify the current product documentation and version compatibility before making a platform decision.

## Choose Services when

- Cloud use is permitted
- The organization wants reduced platform maintenance
- Microsoft Entra ID integration fits the identity strategy
- Teams want current service capabilities
- Distributed access is important
- The organization prefers consumption/licensing over owning server infrastructure

## Choose Server when

- Data must remain in a controlled on-premises environment
- Networks are disconnected or highly restricted
- Regulatory or contractual requirements prohibit the cloud service
- A required integration or customization depends on the server product
- The organization can operate SQL Server, backups, upgrades, monitoring, identity, and disaster recovery

“Existing on-premises systems” alone is not sufficient justification. Evaluate long-term operating effort and product direction.

## Responsibility model

With Services, Microsoft operates the service platform, but the customer still owns:

- Organization and project design
- User lifecycle and permissions
- Repository and branch controls
- Pipeline identities and service connections
- Agent security, particularly self-hosted agents
- Data classification and retention decisions
- Extensions and integrations
- Work processes and audit review

With Server, the customer owns all of the above plus infrastructure, databases, patching, upgrades, availability, backup, restore, and capacity.

## Decision framework

Document:

1. Data location and regulatory constraints
2. Connectivity and latency requirements
3. Identity and conditional-access requirements
4. Integration and extension dependencies
5. Recovery-time and recovery-point objectives
6. Upgrade and maintenance capability
7. Required feature timeline
8. Total cost over several years
9. Migration and exit considerations

## Migration considerations

A migration is not merely moving repositories. Inventory:

- Projects, teams, processes, and custom work item types
- Users, groups, access levels, and permissions
- Git and TFVC repositories
- Pipelines, agents, environments, and service connections
- Variable groups, secrets, secure files, and certificates
- Artifacts and feeds
- Test plans and attachments
- Extensions, service hooks, APIs, and reporting
- Retention, history, and audit requirements

Use a rehearsal, validate representative data, freeze changes when necessary, and retain a rollback or recovery plan.

## Common mistakes

- Comparing only license price
- Ignoring the labor required to operate Server
- Assuming cloud hosting removes customer security responsibility
- Migrating code while forgetting identities, feeds, pipelines, and traceability
- Designing around a feature without checking the specific Server version
- Giving self-hosted agents broad network access because the control plane is trusted

## Interview preparation

**Q: Why would an organization choose Azure DevOps Server?**  
Typical reasons include data residency, disconnected networks, regulatory requirements, or necessary on-premises integrations. The organization must accept responsibility for operation, upgrades, availability, and recovery.

**Q: Is Azure DevOps Services maintenance-free for customers?**  
No. Microsoft operates the platform, while customers still govern identities, permissions, data, pipelines, agents, service connections, extensions, and processes.

**Q: What is a hidden migration risk?**  
Teams often focus on Git history and overlook work-item customization, permissions, service connections, artifacts, test data, extensions, and external integrations.

## Further reading

- [What is Azure DevOps?](https://learn.microsoft.com/en-us/azure/devops/user-guide/what-is-azure-devops)
- [Azure DevOps Server documentation](https://learn.microsoft.com/en-us/azure/devops/server/)
