---
title: "az always on SQL aaaConfigure terheléselosztó |} Microsoft Docs"
description: "SQL mindig a, és hogyan tooleverage powershell toocreate terheléselosztóját hello SQL végrehajtása Load balancer toowork konfigurálása"
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
ms.openlocfilehash: ac6200b18f725dadee2555b80055327d379417d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-load-balancer-for-sql-always-on"></a><span data-ttu-id="42b91-103">Konfigurálása terheléselosztó SQL mindig</span><span class="sxs-lookup"><span data-stu-id="42b91-103">Configure load balancer for SQL always on</span></span>

<span data-ttu-id="42b91-104">SQL Server AlwaysOn rendelkezésre állási csoportok futtatható a Példánynak.</span><span class="sxs-lookup"><span data-stu-id="42b91-104">SQL Server AlwaysOn Availability Groups can now be run with ILB.</span></span> <span data-ttu-id="42b91-105">Rendelkezésre állási csoport egy SQL Server flagship megoldás magas rendelkezésre állási és vészhelyreállítási helyreállításhoz.</span><span class="sxs-lookup"><span data-stu-id="42b91-105">Availability Group is SQL Server's flagship solution for high availability and disaster recovery.</span></span> <span data-ttu-id="42b91-106">rendelkezésre állási csoport figyelőjének hello lehetővé teszi, hogy ügyfél alkalmazások tooseamlessly csatlakozás toohello elsődleges másodpéldány, függetlenül hello replikák hello konfigurációban hello száma.</span><span class="sxs-lookup"><span data-stu-id="42b91-106">hello Availability Group Listener allows client applications tooseamlessly connect toohello primary replica, irrespective of hello number of hello replicas in hello configuration.</span></span>

<span data-ttu-id="42b91-107">hello figyelőjének (DNS) nevével csatlakoztatott tooa elosztott terhelésű IP-cím, Azure terheléselosztója hello replikakészlethez hello bejövő forgalom tooonly hello elsődleges kiszolgáló gazdagépére továbbítja.</span><span class="sxs-lookup"><span data-stu-id="42b91-107">hello listener (DNS) name is mapped tooa load-balanced IP address and Azure's load balancer directs hello incoming traffic tooonly hello primary server in hello replica set.</span></span>

<span data-ttu-id="42b91-108">Az SQL Server AlwaysOn (figyelő) végpontokhoz ILB támogatási is használhatja.</span><span class="sxs-lookup"><span data-stu-id="42b91-108">You can use ILB support for SQL Server AlwaysOn (listener) endpoints.</span></span> <span data-ttu-id="42b91-109">Most hello kisegítő hello figyelő szabályozhatják, és választhat hello elosztott terhelésű IP-cím a megadott alhálózat a virtuális hálózat (VNet).</span><span class="sxs-lookup"><span data-stu-id="42b91-109">You now have control over hello accessibility of hello listener and can choose hello load-balanced IP address from a specific subnet in your Virtual Network (VNet).</span></span>

<span data-ttu-id="42b91-110">Hello figyelő ILB használatával hello az SQL server endpoint (pl. Server = tcp:ListenerName, 1433; adatbázis = DatabaseName) csak úgy érhető el:</span><span class="sxs-lookup"><span data-stu-id="42b91-110">By using ILB on hello listener, hello SQL server endpoint (e.g. Server=tcp:ListenerName,1433;Database=DatabaseName) is accessible only by:</span></span>

* <span data-ttu-id="42b91-111">Szolgáltatások és a virtuális gépek hello azonos virtuális hálózaton</span><span class="sxs-lookup"><span data-stu-id="42b91-111">Services and VMs in hello same Virtual network</span></span>
* <span data-ttu-id="42b91-112">Szolgáltatások és virtuális gépek csatlakoztatott helyszíni hálózatról</span><span class="sxs-lookup"><span data-stu-id="42b91-112">Services and VMs from connected on-premises network</span></span>
* <span data-ttu-id="42b91-113">Szolgáltatások és az összekapcsolt Vnetek virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="42b91-113">Services and VMs from interconnected VNets</span></span>

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

<span data-ttu-id="42b91-115">1. ábra – az SQL AlwaysOn konfigurálva az Internet felé néző terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="42b91-115">Figure 1 - SQL AlwaysOn configured with Internet-facing load balancer</span></span>

## <a name="add-internal-load-balancer-toohello-service"></a><span data-ttu-id="42b91-116">Belső terheléselosztó toohello szolgáltatás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="42b91-116">Add Internal Load Balancer toohello service</span></span>

1. <span data-ttu-id="42b91-117">A következő példa hello hogy egy virtuális hálózatot, amely tartalmazza a "Alhálózat-1" nevű alhálózat konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="42b91-117">In hello following example, we will configure a Virtual network that contains a subnet  called 'Subnet-1':</span></span>

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. <span data-ttu-id="42b91-118">Elosztott terhelésű végpont-hozzáadáshoz a Példánynak az egyes virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="42b91-118">Add load balanced endpoints for ILB on each VM</span></span>

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    <span data-ttu-id="42b91-119">Hello a fenti példában fel kell 2 VM hívott "sqlsvc1" és "sqlsvc2" fut a felhőben hello szolgáltatást a "Sqlsvc".</span><span class="sxs-lookup"><span data-stu-id="42b91-119">In hello example above, you have 2 VM's called "sqlsvc1" and "sqlsvc2" running in hello cloud service "Sqlsvc".</span></span> <span data-ttu-id="42b91-120">Létrehozása után a ILB hello `DirectServerReturn` váltani, hozzáadhat betölteni az elosztott terhelésű végpont toohello ILB tooallow SQL tooconfigure hello figyelői hello rendelkezésre állási csoportok számára.</span><span class="sxs-lookup"><span data-stu-id="42b91-120">After creating hello ILB with `DirectServerReturn` switch, you add load balanced endpoints toohello ILB tooallow SQL tooconfigure hello listeners for hello availability groups.</span></span>

<span data-ttu-id="42b91-121">Az SQL AlwaysOn szolgáltatással kapcsolatos további információkért lásd: [belső terheléselosztót egy AlwaysOn rendelkezésre állási csoport konfigurálása az Azure-](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="42b91-121">For more information about SQL AlwaysOn, see [Configure an internal load balancer for an AlwaysOn availability group in Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="42b91-122">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="42b91-122">See Also</span></span>
[<span data-ttu-id="42b91-123">Ismerkedés az Internet felé néző terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="42b91-123">Get started configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="42b91-124">Ismerkedés a belső terheléselosztók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="42b91-124">Get started configuring an Internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="42b91-125">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="42b91-125">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="42b91-126">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="42b91-126">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
