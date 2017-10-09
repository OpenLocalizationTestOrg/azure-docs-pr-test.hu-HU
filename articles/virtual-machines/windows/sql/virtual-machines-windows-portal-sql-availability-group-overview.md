---
title: "Kiszolgáló rendelkezésre állási csoportok – Azure virtuális gépek – áttekintés aaaSQL |} Microsoft Docs"
description: "Ez a cikk Azure virtuális gépeken futó SQL Server rendelkezésre állási csoportok be."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/13/2017
ms.author: mikeray
ms.openlocfilehash: ecac8b8c5073021af2aa22a05490bb8c4c20ed17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a><span data-ttu-id="32b98-103">SQL Server Always On rendelkezésre állási csoportok az Azure virtuális gépeken bemutatása</span><span class="sxs-lookup"><span data-stu-id="32b98-103">Introducing SQL Server Always On availability groups on Azure virtual machines</span></span> #

<span data-ttu-id="32b98-104">Ez a cikk be az SQL Server rendelkezésre állási csoportok Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="32b98-104">This article introduces SQL Server availability groups on Azure Virtual Machines.</span></span> 

<span data-ttu-id="32b98-105">Always On rendelkezésre állási csoportok Azure virtuális gépeken a rendelkezésre állási csoportok a helyszíni hasonló tooAlways.</span><span class="sxs-lookup"><span data-stu-id="32b98-105">Always On availability groups on Azure Virtual Machines are similar tooAlways On availability groups on premises.</span></span> <span data-ttu-id="32b98-106">További információkért lásd: [Always On rendelkezésre állási csoportok (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).</span><span class="sxs-lookup"><span data-stu-id="32b98-106">For more information, see [Always On Availability Groups (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).</span></span> 

<span data-ttu-id="32b98-107">hello ábra szemlélteti a teljes SQL Server rendelkezésre állási csoport az Azure virtuális gépek hello részeit.</span><span class="sxs-lookup"><span data-stu-id="32b98-107">hello diagram illustrates hello parts of a complete SQL Server Availability Group in Azure Virtual Machines.</span></span>

![Rendelkezésre állási csoport](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

<span data-ttu-id="32b98-109">hello legfontosabb különbség az Azure virtuális gépek rendelkezésre állási csoport, amely az Azure virtuális gépek hello szükséges egy [terheléselosztó](../../../load-balancer/load-balancer-overview.md).</span><span class="sxs-lookup"><span data-stu-id="32b98-109">hello key difference for an Availability Group in Azure Virtual Machines is that hello Azure virtual machines, require a [load balancer](../../../load-balancer/load-balancer-overview.md).</span></span> <span data-ttu-id="32b98-110">hello terheléselosztó hello rendelkezésre állási csoport figyelőjének hello IP-címet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="32b98-110">hello load balancer holds hello IP addresses for hello availability group listener.</span></span> <span data-ttu-id="32b98-111">Ha egynél több rendelkezésre állási csoport minden egyes csoport szükséges egy figyelő.</span><span class="sxs-lookup"><span data-stu-id="32b98-111">If you have more than one availability group each group requires a listener.</span></span> <span data-ttu-id="32b98-112">Egy terheléselosztó több figyelői képes támogatni.</span><span class="sxs-lookup"><span data-stu-id="32b98-112">One load balancer can support multiple listeners.</span></span>

<span data-ttu-id="32b98-113">Amikor készen áll a toobuild egy SQL Server rendelkezésre állási aroup Azure virtuális gépeken, tekintse meg a toothese oktatóanyagok.</span><span class="sxs-lookup"><span data-stu-id="32b98-113">When you are ready toobuild a SQL Server availability aroup on Azure Virtual Machines, refer toothese tutorials.</span></span>

## <a name="automatically-create-an-availability-group-from-a-template"></a><span data-ttu-id="32b98-114">Rendelkezésre állási csoport automatikusan létrehozása sablonból</span><span class="sxs-lookup"><span data-stu-id="32b98-114">Automatically create an availability group from a template</span></span>

[<span data-ttu-id="32b98-115">Always On rendelkezésre állási csoport konfigurálásához Azure virtuális gép automatikusan - erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="32b98-115">Configure Always On availability group in Azure VM automatically - Resource Manager</span></span>](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)

## <a name="manually-create-an-availability-group-in-azure-portal"></a><span data-ttu-id="32b98-116">Hozzon létre manuálisan egy rendelkezésre állási csoport Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="32b98-116">Manually create an availability group in Azure portal</span></span>

<span data-ttu-id="32b98-117">Is létrehozhat hello virtuális gépek saját kezűleg hello sablon nélkül.</span><span class="sxs-lookup"><span data-stu-id="32b98-117">You can also create hello virtual machines yourself without hello template.</span></span> <span data-ttu-id="32b98-118">Első lépésként hello Előfeltételek befejeződését, majd hello rendelkezésre állási csoport létrehozására.</span><span class="sxs-lookup"><span data-stu-id="32b98-118">First, complete hello prerequisites, then create hello availability group.</span></span> <span data-ttu-id="32b98-119">Tekintse meg a következő témakörök hello:</span><span class="sxs-lookup"><span data-stu-id="32b98-119">See hello following topics:</span></span> 

- [<span data-ttu-id="32b98-120">Előfeltételek az SQL Server Always On rendelkezésre állási csoportok konfigurálása Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="32b98-120">Configure prerequisites for SQL Server Always On availability groups on Azure Virtual Machines</span></span>](virtual-machines-windows-portal-sql-availability-group-prereq.md)

- [<span data-ttu-id="32b98-121">Mindig a rendelkezésre állási csoport létrehozása tooimprove rendelkezésre állási és katasztrófa-helyreállítás</span><span class="sxs-lookup"><span data-stu-id="32b98-121">Create Always On Availability Group tooimprove availability and disaster recovery</span></span>](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a><span data-ttu-id="32b98-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="32b98-122">Next steps</span></span>

<span data-ttu-id="32b98-123">[SQL-kiszolgáló mindig konfigurálása az Azure virtuális gépeken különböző régiókban rendelkezésre állási csoporton](virtual-machines-windows-portal-sql-availability-group-dr.md).</span><span class="sxs-lookup"><span data-stu-id="32b98-123">[Configure a SQL Server Always On Availability Group on Azure Virtual Machines in Different Regions](virtual-machines-windows-portal-sql-availability-group-dr.md).</span></span>
