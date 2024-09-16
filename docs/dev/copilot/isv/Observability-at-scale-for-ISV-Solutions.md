---
title: Observability at scale for ISV Solutions on the Microsoft Cloud
description: Learn how to observe and monitor your Microsoft Cloud application across different deployment types using different strategies and Microsoft tools, such as the Azure Monitor Metrics Data Plane API.
author: miglaros
ms.author: miglaros
ms.service: cloud-for-industries
ms.topic: overview
ms.date: 09/16/2024

#CustomerIntent: As an ISV developing solutions on the Microsoft Cloud, I want to learn about ways to observe my solution and monitor it for potential issues after deployment.
---

## Observability at scale for ISV Solutions on the Microsoft Cloud

If you’re an independent software vendor (ISV) building applications on the Microsoft Cloud, you likely require insight into your applications’ data after deployment. Ensuring visibility into the performance of your application—no matter where it’s running—is critical to maintaining and improving it. If you’ve deployed large-scale solutions on Azure, you might know the [Azure Monitor Metrics Data Plane API](https://techcommunity.microsoft.com/t5/azure-observability-blog/azure-monitor-announcing-general-availability-of-azure-monitor/ba-p/4041394). It helps you query metrics across subscriptions and meet your observability needs.

When deploying a Microsoft Cloud application, there are several [landing zones and deployment models](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/isv-landing-zone?tabs=mg-env-no%2Cminimal) ISVs can choose from. The ways that you practice observability varies depending on the deployment pattern and platforms you choose. For instance, it’s typically easier for an ISV to monitor their own SaaS platform than to gain visibility into software that a customer deploys themselves.

## Observability challenges and considerations

When a solution is developed using Microsoft Cloud capabilities across platforms such as Azure, Teams, Fabric, and more, the challenge becomes effectively monitoring large deployments at scale. Monitoring large-scale AI application deployments requires you, as a ISV, to aggregate metrics from various cloud subscriptions, identify and isolate issues, and then begin the troubleshooting process.

Some of the considerations that ISVs can address through effective monitoring include:

- Evaluating core infrastructure availability for potential issues
- Identifying a need for scalability for their applications
- Processing data quickly and effectively
- Efficiently responding to errors

If you’re building generative AI applications, you may have even more challenges and considerations for observability, as AI introduces complexity into topics like data regulation, ethical use, and privacy issues. For information on metrics and telemetry considerations specific to generative AI, visit this guide on [Observability for generative AI](Observability-for-AI-Learn-Page.md).

## Enabling observability at scale through Azure Monitor

The [Azure Monitor Metrics Data Plane API](https://github.com/microsoft/mcsa-observability?tab=readme-ov-file#readme) can improve resource insight gathering by unlocking higher querying capacity and increased efficiency for ISVs like you.

This API can, with a single batch call, retrieve up to 50 resource IDs, gather customer information, and provide essential clues for effective troubleshooting. These capabilities help you improve query throughput, manage throttling risks, and improve your experience as you gather insight into your solution.

The API requires code modifications by the ISV for some deployment methods but is ready to use for certain pure SaaS applications.

### Gathering metrics through Azure Monitor

Through APIs like the Azure Monitor Metrics Data Plane API, ISVs are able to query essential information from various sources in real time.

#### Metrics supported

Through built-in querying capabilities and options for revision and customization, the API is able to provide ISVs with various metrics. It’s capable of retrieving Azure resource metrics. Some of the metrics you can use the API to retrieve include:

- Firewall health
- Storage availability
- Log analytics rates
- Load balancer availability
- Azure Kubernetes Service (AKS) Server Node status
- Key vault availability
- Container registry success
- Cosmos DB Availability

#### Sources

Azure Monitor can gather data from a wide range of sources and application types, whether they are hosted in Azure, other cloud environments, or on-premises.

You can use the API to gather data from sources such as:

- Apps
- Workloads
- Databases
- Infrastructure
- Guest operating systems
- Azure Platform
- Custom Sources integrating with Azure Monitor APIs

#### Data storage

Azure monitor is capable of retaining information in data stores. If you’re looking to archive data for a longer amount of time, you can export your data to Azure Storage.

#### Consumption

Consuming your data in useful ways is the most important phase of observability. Azure Monitor is equipped to help you visualize, analyze, respond to, and gather insights into your data.

| Azure Monitor Capability   | Tools and Options                                      |
|----------------------------| -------------------------------------------------------|
| Insights                   | Azure Monitor is equipped with many insights into applications, containers, networks, and more. These insights can provide comprehensive information for the performance and health of everything from your Kubernetes clusters to your Linux VMs.  
| Visualize                  | Helpful workbooks and dashboards allow you to to turn data into charts, tables, and graphs that can be shared across teams. Grafana and Power BI are also integrated into the Azure Monitor portal.
| Analyze                    | Through tools like metric explorer, log analytics, and change analysis you can query for trends and issues in your metric values, log data, and resource changes respectively.
| Respond                    | Azure Monitor has the capability to not only alert you of critical conditions, but to act on issues as well. Azure Monitor’s capabilities can be further enhanced with AI services that automate tasks and autoscaling features that manage resource loads efficiently.

#### Presentation

You can use tools like Grafana for easy visibility into your solution’s health and performance. The dashboard is customizable for ISVs who want to display extra metrics.

## Observability based on deployment patterns

### Pure SaaS deployments

If you’re deploying your application as a pure SaaS subscription, practicing observability is easily enabled within your own infrastructure. While you are able to query data directly, it is important to maintain consistent supervision over your environment to ensure it is able to perform well.

#### Key observability considerations for pure SaaS deployments:

- Applications deployed into your own Azure subscription can use the Azure Monitor Application Insights capabilities.
- You can also the Azure Monitor Metrics Data Plane API to query metrics at scale.

### Customer-deployed

When your customer deploys the solution within their own environment, they may be able to see benefits such as greater control and security. You can provide them with observability solutions, encourage them to build their own, or obtain customer consent to gather query data from external sources. 

#### Key observability considerations for customer-deployed applications:

- Your customers use an observability solution that you provide to them or they can build their own observability solutions in situations that may require unique integration, privacy, or security considerations.
- Custom-built solutions may allow you to capture telemetry and metrics from customers’ subscriptions and enable metric data transfer after your customer consents.

### Dual-deployment SaaS

This deployment pattern allows some pieces of the component to run within the customer’s subscription, while others run within your infrastructure. For example, you could run the backend services required for processing data in Azure while the components from D365 may run in your customer’s tenant.

#### Key observability considerations for dual-deployment SaaS applications:

- You can use Azure Monitor to oversee any infrastructure within your own subscription, but you won’t have visibility into components deployed in your customers’ subscriptions.
- You may obtain other data from consenting customers by using custom solutions similar to the customer-deployed model.

## Next steps

Given these deployment scenarios, you can use the [MCSA-Observability](https://aka.ms/mcsa-observability) solution accelerator to kick start your observability journey. This solution accelerator utilizes the capabilities of the Azure Monitor Metrics Data Place API solution and is available to all ISVs for use in monitoring your solutions at scale.

## Related links

- [Azure Monitor overview - Azure Monitor | Microsoft Learn](https://learn.microsoft.com/en-us/azure/azure-monitor/overview) - In-depth guidance on Azure monitor and its many capabilities.
- [Independent software vendor (ISV) considerations for Azure landing zones - Cloud Adoption Framework | Microsoft Learn](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/isv-landing-zone?tabs=mg-env-yes%2Cminimal#isv-deployment-models) - An ISV guide to different landing zones within Azure.
- [Azure Monitor- Announcing General Availability of Azure Monitor Metrics Data Plane API - Microsoft Community Hub](https://techcommunity.microsoft.com/t5/azure-observability-blog/azure-monitor-announcing-general-availability-of-azure-monitor/ba-p/4041394) - A quick introduction to the Azure Monitor Metrics Data Plane API. 
- [Observability at scale – Azure Monitor Metrics Data Plane API - Microsoft Community Hub](https://techcommunity.microsoft.com/t5/azure-observability-blog/observability-at-scale-azure-monitor-metrics-data-plane-api/ba-p/4085850) - An in-depth review of the Azure Monitor Metrics Data Plane API and how it related to Observability at scale. 
- [Metrics Batch - Batch - REST API (Azure Monitor) | Microsoft Learn](https://learn.microsoft.com/en-us/rest/api/monitor/metrics-batch/batch?view=rest-monitor-2023-10-01&tabs=HTTP) - Specific information on the Azure Monitor Metrics Data Plane API, such as URI Parameters, request bodies, and more.
