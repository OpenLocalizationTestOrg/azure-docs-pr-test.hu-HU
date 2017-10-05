---
title: "Konfigurálása terheléselosztó SQL mindig |} Microsoft Docs"
description: "Terheléselosztó együttműködni SQL mindig és hogyan használhatók ki a terheléselosztó SQL végrehajtása létrehozásához powershell konfigurálása"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: d7bc3790-47d3-4e95-887c-c533011e4afd
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 68aad6253f185d53fdd7f11c8660c7287ef12655
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-load-balancer-for-sql-always-on"></a><span data-ttu-id="f0527-103">Konfigurálása terheléselosztó SQL mindig</span><span class="sxs-lookup"><span data-stu-id="f0527-103">Configure load balancer for SQL always on</span></span>

<span data-ttu-id="f0527-104">SQL Server AlwaysOn rendelkezésre állási csoportok futtatható a Példánynak.</span><span class="sxs-lookup"><span data-stu-id="f0527-104">SQL Server AlwaysOn Availability Groups can now be run with ILB.</span></span> <span data-ttu-id="f0527-105">Rendelkezésre állási csoport egy SQL Server flagship megoldás magas rendelkezésre állási és vészhelyreállítási helyreállításhoz.</span><span class="sxs-lookup"><span data-stu-id="f0527-105">Availability Group is SQL Server's flagship solution for high availability and disaster recovery.</span></span> <span data-ttu-id="f0527-106">A rendelkezésre állási csoport figyelőjének ügyfélalkalmazások teszi lehetővé az elsődleges másodpéldány, függetlenül a konfigurációban a replikák száma zökkenőmentesen csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="f0527-106">The Availability Group Listener allows client applications to seamlessly connect to the primary replica, irrespective of the number of the replicas in the configuration.</span></span>

<span data-ttu-id="f0527-107">A figyelő (DNS) nevet egy elosztott terhelésű IP-cím van leképezve, és Azure terheléselosztója irányítja a bejövő forgalom csak az elsődleges kiszolgálóra a replika.</span><span class="sxs-lookup"><span data-stu-id="f0527-107">The listener (DNS) name is mapped to a load-balanced IP address and Azure's load balancer directs the incoming traffic to only the primary server in the replica set.</span></span>

<span data-ttu-id="f0527-108">Az SQL Server AlwaysOn (figyelő) végpontokhoz ILB támogatási is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f0527-108">You can use ILB support for SQL Server AlwaysOn (listener) endpoints.</span></span> <span data-ttu-id="f0527-109">Most a figyelő a kisegítő szabályozhatják, és az elosztott terhelésű IP-cím közül választhatnak a megadott alhálózat a virtuális hálózat (VNet).</span><span class="sxs-lookup"><span data-stu-id="f0527-109">You now have control over the accessibility of the listener and can choose the load-balanced IP address from a specific subnet in your Virtual Network (VNet).</span></span>

<span data-ttu-id="f0527-110">A figyelő az SQL server endpoint ILB használatával (pl. Server = tcp:ListenerName, 1433; adatbázis = DatabaseName) csak által elérhető:</span><span class="sxs-lookup"><span data-stu-id="f0527-110">By using ILB on the listener, the SQL server endpoint (e.g. Server=tcp:ListenerName,1433;Database=DatabaseName) is accessible only by:</span></span>

* <span data-ttu-id="f0527-111">Szolgáltatások és az azonos virtuális hálózatban lévő virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="f0527-111">Services and VMs in the same Virtual network</span></span>
* <span data-ttu-id="f0527-112">Szolgáltatások és virtuális gépek csatlakoztatott helyszíni hálózatról</span><span class="sxs-lookup"><span data-stu-id="f0527-112">Services and VMs from connected on-premises network</span></span>
* <span data-ttu-id="f0527-113">Szolgáltatások és az összekapcsolt Vnetek virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="f0527-113">Services and VMs from interconnected VNets</span></span>

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

<span data-ttu-id="f0527-115">1. ábra – az SQL AlwaysOn konfigurálva az Internet felé néző terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="f0527-115">Figure 1 - SQL AlwaysOn configured with Internet-facing load balancer</span></span>

## <a name="add-internal-load-balancer-to-the-service"></a><span data-ttu-id="f0527-116">A szolgáltatás belső terheléselosztó hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f0527-116">Add Internal Load Balancer to the service</span></span>

1. <span data-ttu-id="f0527-117">A következő példában azt egy virtuális hálózatot, amely tartalmazza a "Alhálózat-1" nevű alhálózat konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="f0527-117">In the following example, we will configure a Virtual network that contains a subnet  called 'Subnet-1':</span></span>

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. <span data-ttu-id="f0527-118">Elosztott terhelésű végpont-hozzáadáshoz a Példánynak az egyes virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="f0527-118">Add load balanced endpoints for ILB on each VM</span></span>

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    <span data-ttu-id="f0527-119">A fenti példában rendelkezik 2 VM hívott "sqlsvc1" és "sqlsvc2" fut a felhőben található "Sqlsvc" szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f0527-119">In the example above, you have 2 VM's called "sqlsvc1" and "sqlsvc2" running in the cloud service "Sqlsvc".</span></span> <span data-ttu-id="f0527-120">Miután létrehozta a ILB `DirectServerReturn` kapcsoló ad hozzá elosztott terhelésű végpont a Példánynak, hogy az SQL rendelkezésre állási csoportok figyelői konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f0527-120">After creating the ILB with `DirectServerReturn` switch, you add load balanced endpoints to the ILB to allow SQL to configure the listeners for the availability groups.</span></span>

<span data-ttu-id="f0527-121">Az SQL AlwaysOn szolgáltatással kapcsolatos további információkért lásd: [belső terheléselosztót egy AlwaysOn rendelkezésre állási csoport konfigurálása az Azure-](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="f0527-121">For more information about SQL AlwaysOn, see [Configure an internal load balancer for an AlwaysOn availability group in Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="f0527-122">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f0527-122">See Also</span></span>
[<span data-ttu-id="f0527-123">Ismerkedés az Internet felé néző terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f0527-123">Get started configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="f0527-124">Ismerkedés a belső terheléselosztók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f0527-124">Get started configuring an Internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="f0527-125">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f0527-125">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="f0527-126">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f0527-126">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
