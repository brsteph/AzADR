# Architecture Design Record for Hub Infrastructure

* AzADR-104
* Status: {reviewing | accepted | adopted | deprecated } <!-- Status of the decision.  Make the selected option bold -->
* Deciders:
  * **Decision Owner**: _____<!-- Team members who are accountable for this decision -->
  * **Architecture and Engineering**: _____<!-- Technical team members who contributed to the decision -->
* Date:  YYYY-MM-DD <!-- {YYYY-MM-DD when the decision was last updated} -->

**Reason for Decision**: Azure provides multiple ways to provide for hub infrastructure, which provides a set of networking services to allow for traffic between virtual networks, and between Azure and on-prem networks.  It is important to understand the differences and to select one which aligns with your strategy.

## Overview

Enterprise-scale identifies two primary sets of hub infrastructure:

* **Azure Virtual WAN** which allows for Microsoft managed hub Infrastructure...
* **Azure Virtual Networks in a Hub and Spoke configuration** which allows for...

In addition, an organization may have a driver for having a connectivity model that needs to leverage both of these technologies.

Further, an organization may have custom solutions made up of third-party solutions for core Hub Infrastructure.  These patterns need to be driven by specific detailed requirements for performance, integration, and management, and follow the vendor patterns for deployment.

Having multiple hubs for different environments is covered in azADR-105: [Hub Isolation](.\AzADR-105-hubIsolation.md)

## Decision Drivers

Which set of services is right for you depends on a variety of factors:

* The network appliances you will use as part of the connectivity infrastructure
* How secure connections (VPN, ExpressRoute) will operate
  * Volume of connections should be considered
  * Necessary speed and bandwidth should be considered
* Routing scenarios
<!-- these decisions will be covered in AzADR-101 - 103 -->

## Evaluation and Selection

* [ ] **Azure Virtual WAN**
  * Features
    * [ ] Reduced management of hub resources; Microsoft managed the infrastructure
    * [ ] Automation of spoke setup and configuration, such as publishing routes to the spokes
    * [ ] 20-Gbps of aggregate throughput in our hub
    * [ ] Supports large scale VPN implementations (Up to 1,000 branch connections per Virtual Hub) - individual tunnels for site to site VPN are still capped at 1GBps
    * [ ] Enables branch to branch traversal through hub
    * [ ] Enables region to region traversal through hub
  * Limitations
    * [ ] Third party virtual network appliances (NVAs) have limited integration with the hub; only specific vendors can be deployed out as part of a secure hub
    * [ ] Individual site to site VPN tunnels are capped at 1Gbps
    * [ ] Virtual network gateways are not able to be created in spokes of a Virtual WAN hub
    * [ ] May not support all complex routing scenarios


| Stage      | Time to complete  | Current Status | Finished                       | 
|------------|---------------|----------------|------------------------------------|
| Development| 2 days    | Work in progress | <ul><li>- [x] completed</li><li>- [ ] todo</li></ul>
| QA     |3 days |  Work in progress | <ul><li>[x] done</li><li>[ ] tobedone</li></ul>


* [ ] **Azure Virtual Networks in a Hub and Spoke Configuration**
  * Features
    * [ ] We wish to have greater control over the resources in the hub
    * [ ] We wish to be able to configure complex routing in each spoke virtual network
    * [ ] We wish to either manually manage, or use our own automation, for spoke management
    * [ ] We need to use a third-party NVA and cannot use the third party NVA patterns in Azure Virtual WAN
    * [ ] We need less than 20-Gbps of aggregate throughput in our hub
    * [ ] We wish to use a large-scale VPN (>30 tunnels)
    * [ ] We do not need frequent multi-region traffic
  * Limitations
    * >30 tunnels


* [ ] **Azure Virtual Networks in Mesh Configuration**
  * [ ] We wish to have greater control over the resources in the hub
  * [ ] We wish to be able to configure complex routing in each spoke virtual network
  * [ ] We wish to either manually manage, or use our own automation, for spoke management
  * [ ] We need to use a third-party NVA and cannot use the third party NVA patterns in Azure Virtual WAN
  * [ ] We need less than 20-Gbps of aggregate throughput in our hub
  * [ ] We wish to use a large-scale VPN (>30 tunnels)

* [ ] **Azure Virtual WAN with Azure Virtual Networks in a Hub and Spoke Configuration**
  * [ ] We wish to have greater control over the resources in the hub
  * [ ] We wish to be able to configure complex routing in each spoke virtual network
  * [ ] We wish to either manually manage, or use our own automation, for spoke management
  * [ ] We need to use a third-party NVA and cannot use the third party NVA patterns in Azure Virtual WAN
  * [ ] We need to have 20-Gbps of aggregate throughput in our hub
  * [ ] We wish to have a large-scale VPN (>30 tunnels)
  * [ ] We need to ...

* [ ] **Azure Virtual WAN with Azure Virtual Networks with Network Resource Spokes**

* [ ] **Custom Networking Topology** (*not recommended*)
  <!-- Detail your specific requirements here -->

## Notes on Decision <!-- optional -->

<!-- Add any additional notes needed here -->

## Links <!-- optional -->

* Reference to [VWAN Global Transit Network Architecture](https://docs.microsoft.com/en-us/azure/virtual-wan/virtual-wan-global-transit-network-architecture)
* Reference to [Hub-Spoke VWAN Architecture](https://docs.microsoft.com/en-us/azure/architecture/networking/hub-spoke-vwan-architecture)
* Reference for how to [Migrate to Azure Virtual WAN](https://docs.microsoft.com/en-us/azure/virtual-wan/migrate-from-hub-spoke-topology)
