# Azure Architecture Decision Records (AzADR)

## Decision Sets

[Decision Set: Select an Enterprise Scale Landing Zone Reference Architecture](./enterpriseScaleDecisionSet/esDS-EnterpriseScaleType.md)


## What is the AzADR Toolkit?

The AzADR toolkit is a repository of standard architecture decision records for common Azure decisions. By providing templates with many considerations already populated, users of this repository can be guided through complex decisions.

Organizations can use these decision records to rapidly guide them through critical decisions in their Azure adoption.  These documents are more aimed at technical architecture, not organizational alignment.

This is intended as a supplement to other documents, such as [Microsoft's Cloud Adoption Framework](https://docs.microsoft.com/azure/cloud-adoption-framework/).  It is not intended to replace these frameworks, but to rather help guide and capture through key decisions.

## Why use AzADR?  Or ADRs at all, for that matter?

There are a few principles driving this effort:

1. Decision-centric architecture, as a practice, allows for the creating of links between the design of architecture and the requirements; this process makes selecting the right tools easier, as well as provides a way to understand when the requirements have changed sufficiently that the previous architecture is no longer appropriate.
1. There is no one size fits all recommendation for Azure - an organization's best-fit architecture will come from considering needs and requirements, and the capabilities and limitations of the Azure services that might meet the need.
1. It can be difficult to find comparisons between services that detail the specific capabilities and limitations of Azure services.
1. Organizations adopting Azure for migrations or new product development may not have decision-centric architecture practices in place.

In short, this toolkit aspires to provide architects with pre-built decision records in order to allow them to quickly adopt decision-centric architecture for their Azure implementation.

### Where can I go to learn more about ADRs and decision-centric architecture?

Here are some good places to get started:

[Intro to architectural decision records](https://adr.github.io/)

[Sustainable Architectural Design Decisions](https://www.infoq.com/articles/sustainable-architectural-design-decisions/)

[Architecture Decisions: Demystifying Architecture](https://personal.utdallas.edu/~chung/SA/zz-Impreso-architecture_decisions-tyree-05.pdf)

## What is contained in the AzADR Toolkit?

The decision records in this toolkit are organized into decision sets.

A decision set is a broad topic or matter that will require several decisions to be answered in order to conclusively resolve.  Topics like "Security for PCI workloads" or "Strategy for scalable compute" would be decision sets.

Each decision set then contains one or more architecture decision records; the actual ADRs in the toolkit.  These cover well defined requirements, needs, or challenges, and can be answered individually.  For example, for the decision set of "Strategy for scalable compute," you might have an ADR for "Kubernetes vs. App Services" that will detail out the comparible strengths and weakness of customer managed Kubernetes clusters, Azure Kubernetes Services, Azure web apps, and Azure functions.

By answering the individual decision records and coming to a conclusion in the ADRs, the bigger matter can be answered.  As a result, Decision Sets contain summaries of their child decisions, to act as a roll up.

## How do I get started?

* Make a fork of this repository for your own use
* Identify which decision sets you are concerned with
* Customize the decision sets with your notes and selections
* Begin detailed architecture and build