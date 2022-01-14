# Architecture Design Record for VWan vs. Virtual Network Deployment
<!-- Fill in all code blocked items - example: `J. Doe` -->
* esADR-103
* Status: {``reviewing | accepted | adopted | deprecated`` } <!-- Status of the decision -->
* Deciders:
  * **Decision Owner**: `J. Doe`<!-- Team members who are accountable for this decision -->
  * **Architecture and Engineering**: `J. Doe`<!-- Technical team members who contributed to the decision -->
* Date:  `YYYY-MM-DD` <!-- {YYYY-MM-DD when the decision was last updated} -->

**Reason for Decision**: Azure provides multiple ways to provide for hub infrastructure, which provides a set of networking services to allow for traffic between virtual networks, and between Azure and on-prem networks.  It is important to understand the differences and to select one which aligns with your strategy.

## Overview

Enterprise-scale identifies two primary sets of hub infrastructure:

* **Azure Virtual WAN** which allows for Microsoft managed hub infrastructure that scales for multiple VPN tunnels and branches, as well as cross regional functionality.  The hub can be deployed in a secure fashion with integrated Azure firewalls or select third party network virtual appliances (NVAs), but doing so can limit its routing functionality.
* **Azure Virtual Networks in a Hub and Spoke configuration** which allows for customer managed hub infrastructure that has more manual scaling, but offers more fine grained controls over configuration.  It can work with a wider variety of third party NVAs.

In addition, an organization may have a driver for having a connectivity model that needs to leverage both of these technologies.  They might have a security spoke that contains third party NVAs that is accessed by traversing through the Virtual WAN hub, or they might have a virtual network hub and spoke that have a VWAN hub configured for gateway access only.

Further, an organization may have custom solutions made up of third-party solutions for core Hub Infrastructure.  These patterns need to be driven by specific detailed requirements for performance, integration, and management, and follow the vendor patterns for deployment.

This decision is for the technologies to support the deployment of hubs.  An additional description about how many hubs to have is based on deployment regions, and having multiple hubs for different environments is covered in azADR-105: [Hub Isolation](.\AzADR-105-hubIsolation.md)

## Decision Drivers

Which set of services is right for you depends on a variety of factors:

* The network appliances you will use as part of the connectivity infrastructure
* How secure connections (VPN, ExpressRoute) will operate
  * Volume of connections should be considered
  * Necessary speed and bandwidth should be considered
* Routing scenarios
<!-- these decisions will be covered in AzADR-101 - 103 -->

## Evaluation and Selection

<!-- For each [ ] instance, convert it to a [x] to mark if it is of interest; this "checks" the box when viewed.  Features should be checked if the feature is needed or desireable; Limitations should be checked if they prevent desired outcomes or are otherwise undesirable.  While each Feature or Limit may matter differently, by understanding which items are important will help you make your decision. -->

### Core Solutions

Core solutions are architectures, patterns, and services that are most often used.  They tend to be more streamlined than complex solutions, but still work for the majority of use cases.  They tend to be easier to support due to having viewer failure points, and their more scalable and simple design leads to reduced management overhead.

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

* [ ] **Azure Virtual WAN**
  * Features
    * [ ] Reduced management of hub resources; Microsoft managed the infrastructure
    * [ ] Automation of spoke setup and configuration, such as publishing routes to the spokes
    * [ ] 20-Gbps of aggregate throughput in our hub
    * [ ] Supports large scale VPN implementations (Up to 1,000 branch connections per Virtual Hub) - individual tunnels for site to site VPN are still capped at 1GBps
    * [ ] Allows for the deployment of Azure Firewall as part of a secure hub deployment
    * [ ] Allows for the deployment of the following Security-as-a-Service partner products:
    * Zscaler
    * Checkpoint
    * iboss
    * [ ] Enables branch to branch traversal through hub
    * [ ] Enables region to region traversal through hub
    * [ ] Baseline security enabled by default
  * Limitations
    * [ ] Other third-party NVAs are not able to be deployed in to the Hub
    * [ ] Using a secure hub prevents branch to branch and hub to hub routing
    * [ ] Individual site to site VPN tunnels are capped at 1 Gbps
    * [ ] Virtual network gateways are not able to be created in spokes of a Virtual WAN hub; network gateways can only be deployed in the hub

* [ ] **Azure Virtual Networks in a Hub and Spoke Configuration**
  * Features
    * [ ] Greater control over hub; able to place multiple VM-based services in to the hub
    * [ ] Ability to deploy more complex routing scenarios
    * [ ] Ability to use third party network virtual appliances in the hub
    * [ ] Ability to support custom routing scenarios through multiple virtual appliances
  * Limitations
    * [ ] Customer managed hub infrastructure, resulting in more management
    * [ ] Spokes must be manually managed, or use separate automation; this includes routing information
    * [ ] The hub can support, in general, 30 tunnels without needing for more complex deployments
    * [ ] More complex management needed for multi-region traffic
    * [ ] Less than 20 Gbps of aggregate throughput through hub
    * [ ] Baseline security managed by customer; higher security risk to design

<!-- ### Complex Solutions

Complex solutions are architectures, patterns, and services that are used rarely, based on specific needs.  They are most often used when standard solutions are not appropriate, and tend to contain multiple services integrated together.  They tend to have a higher administrative overhead, and more potential failure points.  *These should be used only if requirements preclude the use of Core Solutions*

* [ ] **Azure Virtual WAN with Indirect Spokes**
  * [ ] We wish to have greater control over the resources in the hub
  * [ ] We wish to be able to configure complex routing in each spoke virtual network
  * [ ] We wish to either manually manage, or use our own automation, for spoke management
  * [ ] We need to use a third-party NVA and cannot use the third party NVA patterns in Azure Virtual WAN
  * [ ] We need to have 20-Gbps of aggregate throughput in our hub
  * [ ] We wish to have a large-scale VPN (>30 tunnels)
  * [ ] We need to ...

* [ ] **Azure Virtual WAN with Azure Virtual Networks with Network Resource Spokes** -->

  <!-- Detail your specific requirements here -->

## Notes on Decision <!-- optional -->

`` Any specific notes ``
<!-- Add any additional notes needed here -->

## Links <!-- optional -->

* Reference to [VWAN Global Transit Network Architecture](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-global-transit-network-architecture)
* Reference to [Hub-Spoke VWAN Architecture](https://docs.microsoft.com/azure/architecture/networking/hub-spoke-vwan-architecture)
* Reference for how to [Migrate to Azure Virtual WAN](https://docs.microsoft.com/azure/virtual-wan/migrate-from-hub-spoke-topology)
