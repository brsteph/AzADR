# Architecture Design Record for Shared Services Subscription Segregation
<!-- Fill in all code blocked items - example: `J. Doe` -->
* *esADR-103*
* **Status:** {``reviewing | accepted | adopted | deprecated`` } <!-- Status of the decision -->
* **Deciders:**
  * **Decision Owner**: `J. Doe` <!-- Team members who are accountable for this decision -->
  * **Architecture and Engineering**: `J. Doe` <!-- Technical team members who contributed to the decision -->
* **Date:**  `YYYY-MM-DD` <!-- {YYYY-MM-DD when the decision was last updated} -->

## Overview

*In the previous decision records, you decided that your organization needed to have some set of shared connectivity and identity shared services, in addition to the management services.  In addition, you decided that you would be using virtual networks for your hub connectivity.*

When deploying the shared services for your organization, you have the option of separating their deployments out in to seperate subscriptions, or deploying them as part of different resources groups in the same subscription.

This Architectural Decision Record captures your organizations decision on your approach to the subscription topology for this deployment.

## Decision Drivers

Which topology is right for you depends on various factors, such as:

* How many teams you have operating in Azure
* Which teams manage which resources
* The amount of roles and separation of duties to consider
* The amount of management overhead you are willing to accept
* The amount of growth of teams and resources you expect

## Evaluation and Selection

<!-- For each [ ] instance, convert it to a [x] to mark if it is of interest; this "checks" the box when viewed.  Features should be checked if the feature is needed or desireable; Limitations should be checked if they prevent desired outcomes or are otherwise undesirable.  While each Feature or Limit may matter differently, by understanding which items are important will help you make your decision. -->

* [ ] **Use seperate subscriptions for Management, Connectivity, and Identity**
  * Features
    * [ ] Allows for the deployment of RBAC at the subscription level for roles involved in these topics
    * [ ] Allows for easier management of fine grained access and sub-roles, such as network administrators who can manage firewall services vs. network operators who can manage VPN/Gateway services
    * [ ] Allows for better scaling and adding of resources
  * Limitations
    * [ ] Additional administrative effort to manage the subscriptions
    * [ ] If the same users are getting similar roles in all subscriptions, it can create redundant access that can further increase the difficulty in managing.

* [ ] **Use one subscription for Management, Connectivity, and Identity**
  * Features
    * [ ] Allows for a quicker start, as there are less permissions that need to be assigned.
    * [ ] Allows for one team to be assigned broad permissions without separation of duties
  * Limitations
    * [ ] Limits the ability to seperate access between team members
    * [ ] As teams, roles, and responsibilities grow, will require upgrade to expand out to multiple subscriptions to allow for fine grained access controls
    * [ ] Scaling for additional resources can be more complex

## Notes on Decision

`` Add any additional notes needed here, or remove this section ``

## Links

`Add any additional links here`
