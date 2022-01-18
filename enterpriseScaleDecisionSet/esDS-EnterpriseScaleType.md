# Decision Set for Enterprise Scale Deployments
<!-- Fill in all code blocked items - example: `J. Doe` -->
* esDS
* Status: {``reviewing | accepted | adopted | deprecated`` } <!-- Status of the decision -->
* Deciders:
  * **Decision Owner**: `J. Doe`<!-- Team members who are accountable for this decision -->
  * **Architecture and Engineering**: `J. Doe`<!-- Technical team members who contributed to the decision -->
* Date:  `YYYY-MM-DD` <!-- {YYYY-MM-DD when the decision was last updated} -->

**Reason for Decision Set**: As Part of Microsoft's Cloud Adoption Framework, tools are given to enable customers to [Start with enterprise scale](https://docs.microsoft.com//azure/cloud-adoption-framework/ready/enterprise-scale/).  This approach provides design principles and tools for the deployment of enterprise-scale landing zones.  

Included in this are [reference implementation templates](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/enterprise-scale/implementation#reference-implementation) can be used to rapidly deploy enterprise-scale landing zones in Azure.  However, some organizations are unsure which of these options are the best fit for their needs.

This decision set is made up of individual decisions, and then this "cover page" to act as a roll up to make the selection of the landing zone.  Each decision offers multiple approaches to solving a problem, documented as an [architectural decision record](https://adr.github.io/). These ADRs contain key criteria for the selection, and features and limitations of the two options.

## Landing Zone Selection

**Note:** *Azure Government* options are **removed** forom consideration for this MVP.

### Options

While detailed more fully in the [reference implementation templates](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/enterprise-scale/implementation#reference-implementation) and each implementations repository readme,  the table below gives an overview.  Included is the nickname for each implementation, often used in the readmes to discuss specific scenarios.

| Example Deployment | Summary of Deployment | "Nickname" |
| - | - | - |
| Enterprise-scale foundation | Management group and subscription structure, workload Landing Zones, and shared management resources | Wingtip |
| Enterprise-scale hub and spoke | Management group and subscription structure, a connectivity hub virtual network with Azure network appliances, workload Landing Zones, and shared management resources | Adventureworks |
| Enterprise-scale Virtual WAN | Management group and subscription structure, a virtual WAN and virtual hub with Azure network appliances, workload Landing Zones, and shared management resources | Contoso|
| Enterprise-scale for small enterprises | Simplified management group and subscription structure, a connectivity hub virtual network with Azure network appliances, workload Landing Zones, and shared management resources | TreyResearch |

### Decisions

<!-- For each decision option in the column, select which one applies to your individual decisions, or create a new entry as appropriate to capture your decision if you came to a conclusion -->
| Architecture Decision Record | Topic | Decision |
| - | - | - |
| [esADR-101](./esADR-101.md) | "Should we deploy shared connectivity and identity resources?" | We need connectivity and/or identity resources **OR** We do not need neither connectivity nor identity resources |
| [esADR-102](./esADR-102) | "Should we use Azure VWAN or Azure Virtual Networks for our connectivity resources?" | We need to use Azure VWAN Hubs **OR** We need to use Azure Virtual Network Hubs **OR** We need to use Azure VWAN and Azure Virtual Network Hubs **OR** We do not need connectivity resources (*see esADR-101*) |
| [esADR-103](./esADR-103) | "Should we separate our management, connectivity, and identity subscriptions?" | We need to separate our management, connectivity, and identity subscriptions **OR** We need to not separate our management, connectivity, and identity subscriptions **OR** We do not need connectivity and identity resources (*see esADR-101*) **OR** We are using Azure Virtual Wan (*see esADR-102*) |

### Evaluation and Selection

<!-- For each [ ] instance in each sub item, convert it to a [x] to mark which decisions you made above.  If you made a custom decision, include it where appropriate.  Once you have selected all sub items, select the appropriate top level item for your over all landing zone decision  -->

* [ ] Enterprise-scale foundation
  * [ ] **esADR-101:** We do not need neither connectivity nor identity resources
  * [ ] **esADR-102:** We do not need connectivity resources
  * [ ] **esADR-103:** We do not need connectivity and identity resources
  * [ ] **Other:** *We have custom decisions and need flexibility to implement*

* [ ] Enterprise-scale hub and spoke
  * [ ] **esADR-101:** We need connectivity and/or identity resources
  * [ ] **esADR-102:** We need to use Azure Virtual Network Hubs
  * [ ] **esADR-103:** We need to separate our management, connectivity, and identity subscriptions

* [ ] Enterprise-scale Virtual WAN
  * [ ] **esADR-101:** We need connectivity and/or identity resources
  * [ ] **esADR-102:** We need to use Azure VWAN Hubs
  * [ ] **esADR-103:** We need to separate our management, connectivity, and identity subscriptions *or* We are using Azure Virtual Wan

* [ ] Enterprise-scale for small enterprises
  * [ ] **esADR-101:** We need connectivity and/or identity resources
  * [ ] **esADR-102:** We need to use Azure Virtual Network Hubs
  * [ ] **esADR-103:** We need to not separate our management, connectivity, and identity

## Notes on Decisions and Selection

`` Add any additional notes needed here, or remove this section ``

## Links

`Add any additional notes here`
