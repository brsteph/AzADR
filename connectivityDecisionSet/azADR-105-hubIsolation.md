# Architecture Design Record for Hub Isolation

* AzADR-105
* Status: {reviewing | accepted | adopted | deprecated } <!-- optional -->
* Deciders: {list everyone involved in the decision} <!-- optional -->
* Date: {YYYY-MM-DD when the decision was last updated} <!-- optional -->

**Reason for Decision**: Azure can allow for a single hub with routing segments, or multiple hubs based on environments.  Selecting which model aligns with responsibility delegation and networking requirements is important for managing your services.

## Overview
