# Architecture Design Record for Shared Services
<!-- Fill in all code blocked items - example: `J. Doe` -->
* *esADR-101*
* **Status:** {``reviewing | accepted | adopted | deprecated`` } <!-- Status of the decision -->
* **Deciders:**
  * **Decision Owner**: `J. Doe` <!-- Team members who are accountable for this decision -->
  * **Architecture and Engineering**: `J. Doe` <!-- Technical team members who contributed to the decision -->
* **Date:**  `YYYY-MM-DD` <!-- {YYYY-MM-DD when the decision was last updated} -->

## Overview

When deploying Azure workloads, many organizations leverage a set of shared services that each workload can consume.  These shared services can be centrally managed and be scaled across numerous workloads.

In the Enterprise-scale methodology, shared services are segmented in to the following:

* **Management** resources, such as Log Analytics workspaces, Key Vaults, and Storage Accounts
* **Connectivity** resources, such as hub virtual networking, VPN connections, DNS zones, and networking appliances such as firewalls
* **Identity** resources such as domain controllers and their underlying infrastructure (virtual machines and networking)

While any organization should ensure that they have the management services provided, some may or may not need connectivity and identity resources.  Organizations may instead build each workload to be autonomous, with no network connectivity between workloads, and independent identity services.  Others still may struggle with the additional complexity, and may not need to scale to those model for some time.

This Architectural Decision Record captures your organizations decision on their approach to these shared services.

## Decision Drivers

Which topology is right for you depends on various factors, such as:

* If your planned workloads will require Active Directory resources used
* If your workloads will need to have centrally managed firewalls
* If you wish to have private networking to have workloads in different virtual networks interact
* If there are shared networking resources, like public DNS zones, that managed access to the workloads

## Evaluation and Selection

<!-- For each [ ] instance, convert it to a [x] to mark if it is of interest; this "checks" the box when viewed.  Features should be checked if the feature is needed or desireable; Limitations should be checked if they prevent desired outcomes or are otherwise undesirable.  While each Feature or Limit may matter differently, by understanding which items are important will help you make your decision. -->

Items marked with **High Importance** are often of critical import for an organization, and should be given significant weight in the comparison.  Effectively, they mark items that should be thought of in a *must* or *should* level of criticality.

Items marked with **Low Importance** are worth keeping in mind, but often are not critical to the decision process.  While they have an impact to the over all decision, by themself they are not often important enough to lead to one option than the other.  Effectively, they mark items that are *nice to have* or *could* be useful.

### Connectivity Shared Services

This section details out the considerations for using a shared set of connectivity services vs. having networking resources be located with the workload in the workload's landing zone.

In general, the use of connectivity shared services is the default.  While the below break down helps you understand the consequences of each decision, you should begin with asking yourself *"is there a strong benefit to not using connectivity shared services?"* and use the notes below to help make sure you are not overlooking an aspect.

**Important Note**: Regardless of how you allocate the resources, a given workload needs to have network security in place.  While that can be answered in different ways, it is worth calling out that this decision is *not* about the use of networking services like an Azure Firewall.

* [ ] **Use Connectivity Shared Services**
  * Features & Benefits
    * [ ] Grants the ability to more clearly assign RBAC permissions to networking resources
    * [ ] Allows for a central hub of networking connectivity (network virtual appliances, firewalls, and VPN/ExpressRoute connectivity) that can be used by workloads in different virtual networks - **High Importance**
    * [ ] Allows for better auditing and change processes  of networking security controls - **High Importance**
    * [ ] More cost effective than deploying individual instances of network security tools
    * [ ] Allows you to scale effectively as you add workload landing zones and additional resources - **High Importance**
  * Limitations & Consequences
    * [ ] Additional cost for traffic that uses the shared connectivity services; in most organizations this cost is negligible compared to the cost of multiple network security tools, but for workloads moving very large amounts of data, it should be investigated for awareness - **Low Importance**
    * [ ] Seperate virtual networking tools need to be deployed in each workload - **High Importance**

* [ ] **Do not use Connectivity Shared Services, and instead locate them with the workload**
  * Features & Benefits
    * [ ] Reduced number of subscriptions
    * [ ] Reduces overhead cost and administration if workloads are PaaS based and do not need to have inspection between different services
    * [ ] Allows for quick deployment of test workloads and experimentation before the adoption of an enterprise solution
  * Limitations & Consequences
    * [ ] Increases cost and administration if workloads need inspection between services - **High Importance**
    * [ ] Can increase administrative overhead if attempting to use virtual machines as part of one large virtual network, due to needing to create segmentation at the subnet level instead of virtual network levels - **High Importance**
    * [ ] Will create challenges as you add workload landing zones or additional resources

### Identity Shared Services

This section details out the considerations for using a shared set of identity services, vs. having stand alone identity considerations as a part of each workload.

In general, the use of identity shared services is the default when you have one or more workloads that rely heavily on virtual machines that need to be a part of an Active Directory domain to operate.  This means that it is the default assumption for organizations that are performing rehost or replatforming efforts around existing VM workloads.  They are also used when organizations have another centralized identity provider that needs virtual machines to operate.

Organizations who do not have identity shared services in Azure are generally working with PaaS services, or other design patterns which leverage others services for secrets and identity (such as workload specific key vaults and Azure AD).

* [ ] **Use Identity Shared Services**
  * Features and Benefits
    * [ ] Grants the ability to extend your Active Directory environment in to an Azure region
    * [ ] Reduces number of VMs and other resources needed to provide identity services in workload landing zones
    * [ ] Provides an Active Directory domain for VM resources to join

* [ ] **Do not use Identity Shared Services**
  * Limitations & Consequences
    * [ ] Virtual machine based workloads will need to have identity addressed through another solution (which can include workload specific solutions)
    * [ ] Migrated virtual machines will need to have domain controllers in their local virtual networks if needed

## Notes on Decision

`` Add any additional notes needed here, or remove this section ``

## Links

[Connectivity to Azure](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/connectivity-to-azure)

[Plan for inbound and outbound internet connectivity](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/plan-for-inbound-and-outbound-internet-connectivity)

[Plan for landing zone network segmentation](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/plan-for-landing-zone-network-segmentation)

[Plan for traffic inspection](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/plan-for-traffic-inspection)

[Identity and Access Management in Azure](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/design-area/identity-access)

`Add any additional links here`
