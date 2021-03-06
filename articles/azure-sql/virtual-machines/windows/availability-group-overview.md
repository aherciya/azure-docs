---
title: Overview of availability groups
description: "This article introduces SQL Server Availability Groups on Azure virtual machines."
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management

ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql

ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: "01/13/2017"
ms.author: mikeray
ms.custom: "seo-lt-2019"

---

# Introducing SQL Server availability groups on Azure virtual machines
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

This article introduces SQL Server availability groups on Azure Virtual Machines. 

Always On availability groups on Azure Virtual Machines are similar to Always On availability groups on premises. For more information, see [Always On Availability Groups (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx). 

The diagram illustrates the parts of a complete SQL Server Availability Group in Azure Virtual Machines.

![Availability Group](./media/availability-group-overview/00-EndstateSampleNoELB.png)

The key difference for an Availability Group in Azure Virtual Machines is that the Azure virtual machines, require a [load balancer](../../../load-balancer/load-balancer-overview.md). The load balancer holds the IP addresses for the availability group listener. If you have more than one availability group each group requires a listener. One load balancer can support multiple listeners.

Additionally, on an Azure IaaS VM guest failover cluster, we recommend a single NIC per server (cluster node) and a single subnet. Azure networking has physical redundancy which makes additional NICs and subnets unnecessary on an Azure IaaS VM guest cluster. Although the cluster validation report will issue a warning that the nodes are only reachable on a single network, this warning can be safely ignored on Azure IaaS VM guest failover clusters. 

To increase redundancy and high availability, the SQL Server VMs should either be in the same [availability set](availability-group-manually-configure-prerequisites-tutorial.md#create-availability-sets), or different [availability zones](/azure/availability-zones/az-overview). 

|  | Windows Server Version | SQL Server Version | SQL Server Edition | WSFC Quorum Config | DR with Multi-region | Multi-subnet support | Support for an existing AD | DR with multi-zone same region | Dist-AG support with no AD domain | Dist-AG support with no cluster |  
| :------ | :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----|
| [SQL VM CLI](availability-group-az-cli-configure.md) | 2016 | 2017 </br>2016   | Ent | Cloud witness | No | Yes | Yes | Yes | No | No |
| [Quickstart Templates](availability-group-quickstart-template-configure.md) | 2016 | 2017</br>2016  | Ent | Cloud witness | No | Yes | Yes | Yes | No | No |
| [Manual](availability-group-manually-configure-prerequisites-tutorial.md) | All | All | All | All | Yes | Yes | Yes | Yes | Yes | Yes |
| &nbsp; | &nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |

The **SQL Server AlwaysOn Cluster (preview)** template has been removed from the Azure Marketplace and is no longer available. 

When you are ready to build a SQL Server availability group on Azure Virtual Machines, refer to these tutorials.

## Manually with Azure CLI
Using Azure CLI to configure and deploy an availability group is the recommended option, as it's the best in terms of simplicity and speed of deployment. With Azure CLI, the creation of the Windows Failover Cluster, joining SQL Server VMs to the cluster, as well as the creation of the listener and Internal Load Balancer can all be achieved in under 30 minutes. This option still requires a manual creation of the availability group, but automates all the other necessary configuration steps. 

For more information, see [Use Azure SQL VM CLI to configure Always On availability group for SQL Server on an Azure VM](availability-group-az-cli-configure.md). 

## Automatically with Azure Quickstart Templates
The Azure Quickstart Templates utilize the SQL VM resource provider to deploy the Windows Failover Cluster, join SQL Server VMs to it, create the listener, and configure the Internal Load Balancer. This option still requires a manual creation of the availability group, and Internal Load Balancer (ILB) but automates and simplifies all the other necessary configuration steps (including the configuration of the ILB). 

For more information, see [Use Azure Quickstart Template to configure Always On availability group for SQL Server on an Azure VM](availability-group-quickstart-template-configure.md).


## Automatically with an Azure portal template

[Configure Always On availability group in Azure VM automatically - Resource Manager](availability-group-azure-marketplace-template-configure.md)


## Manually in Azure portal

You can also create the virtual machines yourself without the template. First, complete the prerequisites, then create the availability group. See the following topics: 

- [Configure prerequisites for SQL Server Always On availability groups on Azure Virtual Machines](availability-group-manually-configure-prerequisites-tutorial.md)

- [Create Always On Availability Group to improve availability and disaster recovery](availability-group-manually-configure-tutorial.md)

## Next steps

[Configure a SQL Server Always On Availability Group on Azure Virtual Machines in Different Regions](availability-group-manually-configure-multiple-regions.md)
