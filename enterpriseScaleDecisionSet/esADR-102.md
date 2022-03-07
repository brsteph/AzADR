# Architecture Design Record for VWan vs. Virtual Network Deployment
<!-- Fill in all code blocked items - example: `J. Doe` -->
* *esADR-102*
* **Status:** {``reviewing | accepted | adopted | deprecated`` } <!-- Status of the decision -->
* **Deciders:**
  * **Decision Owner**: `J. Doe` <!-- Team members who are accountable for this decision -->
  * **Architecture and Engineering**: `J. Doe` <!-- Technical team members who contributed to the decision -->
* **Date:**  `YYYY-MM-DD` <!-- {YYYY-MM-DD when the decision was last updated} -->

## Overview

*In the previous decision record, you decided that your organization needed a set of shared connectivity services, in addition to the management services.*

Azure provides multiple ways to provide for shared connectivity services in a hub fashion, which provides a set of networking services to allow for traffic between virtual networks, and between Azure and on-prem networks.  It is important to understand the differences and to select one which aligns with your strategy.

Enterprise-scale identifies two primary sets of hub infrastructure:

* **Azure Virtual WAN** which allows for Microsoft managed hub infrastructure that scales for multiple VPN tunnels and branches, as well as cross regional functionality.  The hub can be deployed in a secure fashion with integrated Azure firewalls or select third party network virtual appliances (NVAs), but doing so can limit its routing functionality.
* **Azure Virtual Networks in a Hub and Spoke configuration** which allows for customer managed hub infrastructure that has more manual scaling, but offers more fine grained controls over configuration.  It can work with a wider variety of third party NVAs.

In addition, an organization may have a driver for having a connectivity model that needs to leverage both of these technologies.  They might have a security spoke that contains third party NVAs that is accessed by traversing through the Virtual WAN hub, or they might have a virtual network hub and spoke that have a VWAN hub configured for gateway access only.

Further, an organization may have custom solutions made up of third-party solutions for core Hub Infrastructure.  These patterns need to be driven by specific detailed requirements for performance, integration, and management, and follow the vendor patterns for deployment.

## Decision Drivers

Which set of services is right for you depends on a variety of factors:

* The network appliances you will use as part of the connectivity infrastructure
* How secure connections (VPN, ExpressRoute) will operate
  * Volume of connections should be considered
  * Necessary speed and bandwidth should be considered
* Routing scenarios

### High Level Comparison

✅ - Indicates a clear advantage

☑️ - Indicates a situational advantage

| Feature | Azure Virtual Wan (Virtual Hub) | Azure Virtual Networks (Hub and Spoke)|
|-|-|-|
| Management Overhead| ✅ Lower | Greater
| Automation of Spoke Deployment (including Routes) | ✅ Yes | No (Custom only) |
| Integration with Azure Firewall | ✅ Yes (Automated) | Yes (Manual) |
| Managed Branch to Branch Traversal | ☑️ Yes, unless Secure Hub is used | No |
| Managed Region to Region Traversal | ☑️ Yes, unless Secure Hub is used | No |
| Hub VPN Throughput | ✅ 20 Gbps | 10 Gbps |
| Site to Site VPN Connections | ✅ Up to 1,000 | Up to 100 (Varies by Sku) |
| Point to Site VPN Connections | ✅ Up to 10,000 | ✅ Up to 10,000 |
| Third Party NVA Integration | Limited to specific partners (Zscaler, Checkpoint,  and iboss) | ✅ Any VM based NVA |
| DDoS Protection Standard Support | Spoke Only | ✅ Spoke and Hub |

[Comparison of Vhubs and Vnets](https://docs.microsoft.com/azure/firewall-manager/vhubs-and-vnets#comparison)

## Evaluation and Selection

<!-- For each [ ] instance, convert it to a [x] to mark if it is of interest; this "checks" the box when viewed.  Features should be checked if the feature is needed or desireable; Limitations should be checked if they prevent desired outcomes or are otherwise undesirable.  While each Feature or Limit may matter differently, by understanding which items are important will help you make your decision. -->

* [ ] **Azure Virtual WAN**
  * Features & Benefits
    * [ ] 20-Gbps of aggregate throughput in our hub - **High Importance**
    * [ ] Supports large scale VPN implementations (Up to 1,000 branch connections per Virtual Hub) - individual tunnels for site to site VPN are still capped at 1GBps - **High Importance**
    * [ ] Reduced management of hub resources; Microsoft managed the infrastructure
    * [ ] Automation of spoke setup and configuration, such as publishing routes to the spokes
    * [ ] Allows for the deployment of Azure Firewall as part of a secure hub deployment
    * [ ] Allows for the deployment of the following Security-as-a-Service partner products:
    * Zscaler
    * Checkpoint
    * iboss
    * [ ] Enables branch to branch traversal through hub
    * [ ] Enables region to region traversal through hub
    * [ ] Baseline security able to be enabled on spokes, regardless of spoke configuration
  * Limitations & Consequences
    * [ ] Other third-party NVAs are not able to be deployed in to the Hub - **High Importance**
    * [ ] Baseline security able to be enabled on spokes, regardless of spoke configuration
    * [ ] Using a secure hub prevents branch to branch and hub to hub routing
    * [ ] Individual site to site VPN tunnels are capped at 1 Gbps
    * [ ] Virtual network gateways are not able to be created in spokes of a Virtual WAN hub; network gateways can only be deployed in the hub - **Low Importance**

* [ ] **Azure Virtual Networks in a Hub and Spoke Configuration**
  * Features & Benefits
    * [ ] Greater control over hub; able to place multiple VM-based services in to the hub - **High Importance**
    * [ ] Ability to use third party network virtual appliances in the hub - **High Importance**
    * [ ] Ability to deploy more complex routing scenarios
    * [ ] Ability to support custom routing scenarios through multiple virtual appliances
  * Limitations & Consequences
    * [ ] Less than 20 Gbps of aggregate throughput through hub - **High Importance**
    * [ ] The hub can support, in general, 30 tunnels without needing for more complex deployments - **High Importance**
    * [ ] Customer managed hub infrastructure, resulting in more management
    * [ ] Spokes must be manually managed, or use separate automation; this includes routing information
    * [ ] More complex management needed for multi-region traffic
    * [ ] Baseline security managed by customer; higher security risk to design

## Notes on Decision

`` Add any additional notes needed here, or remove this section ``

## Links

[VWAN Global Transit Network Architecture](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-global-transit-network-architecture)

[Hub-Spoke VWAN Architecture](https://docs.microsoft.com/azure/architecture/networking/hub-spoke-vwan-architecture)

[Migrate to Azure Virtual WAN](https://docs.microsoft.com/azure/virtual-wan/migrate-from-hub-spoke-topology)

[Comparison of Vhubs and Vnets](https://docs.microsoft.com/azure/firewall-manager/vhubs-and-vnets#comparison)

`Add any additional links here`
