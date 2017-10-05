---
title: "Kapcsolatos az ExpressRoute virtuális hálózati átjárók |} Microsoft Docs"
description: "További információk a virtuális hálózati átjárók az ExpressRoute."
services: expressroute
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: 7e0d9658-bc00-45b0-848f-f7a6da648635
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: cherylmc
ms.openlocfilehash: a6363fa380d0bab05d7500141cc6019d1d3f68b8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="about-virtual-network-gateways-for-expressroute"></a><span data-ttu-id="ff3c6-103">Az ExpressRoute virtuális hálózati átjáróinak ismertetése</span><span class="sxs-lookup"><span data-stu-id="ff3c6-103">About virtual network gateways for ExpressRoute</span></span>
<span data-ttu-id="ff3c6-104">A virtuális hálózati átjáró küldhető Azure virtuális hálózatok közötti hálózati forgalom és a helyszíni helyek.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-104">A virtual network gateway is used to send network traffic between Azure virtual networks and on-premises locations.</span></span> <span data-ttu-id="ff3c6-105">Amikor konfigurál egy ExpressRoute-kapcsolatot, hozzon létre és konfigurálnia kell a virtuális hálózati átjáró és a virtuális hálózati átjáró kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-105">When you configure an ExpressRoute connection, you must create and configure a virtual network gateway and a virtual network gateway connection.</span></span>

<span data-ttu-id="ff3c6-106">Egy virtuális hálózati átjáró létrehozásakor több beállítást is meg kell adnia.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-106">When you create a virtual network gateway, you specify several settings.</span></span> <span data-ttu-id="ff3c6-107">A szükséges beállítások egyikének határozza meg, hogy az átjáró expressroute-on vagy a telephelyek közötti VPN-forgalomhoz használható.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-107">One of the required settings specifies whether the gateway will be used for ExpressRoute or Site-to-Site VPN traffic.</span></span> <span data-ttu-id="ff3c6-108">A Resource Manager üzembe helyezési modellel, az érték "-GatewayType".</span><span class="sxs-lookup"><span data-stu-id="ff3c6-108">In the Resource Manager deployment model, the setting is '-GatewayType'.</span></span>

<span data-ttu-id="ff3c6-109">Amikor a hálózati adatforgalom a kapcsolatot, az átjáró típusa "ExpressRoute" használhatja.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-109">When network traffic is sent on a private connection, you use the gateway type 'ExpressRoute'.</span></span> <span data-ttu-id="ff3c6-110">Ezt ExpressRoute-átjárónak is hívják.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-110">This is also referred to as an ExpressRoute gateway.</span></span> <span data-ttu-id="ff3c6-111">Amikor a hálózati adatforgalom titkosított a nyilvános interneten keresztül, használhatja az átjáró típusa "Vpn".</span><span class="sxs-lookup"><span data-stu-id="ff3c6-111">When network traffic is sent encrypted across the public Internet, you use the gateway type 'Vpn'.</span></span> <span data-ttu-id="ff3c6-112">Ez VPN-átjáróként ismert.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-112">This is referred to as a VPN gateway.</span></span> <span data-ttu-id="ff3c6-113">A hely–hely, pont–hely és a virtuális hálózatok közötti kapcsolat kapcsolatok mind VPN-átjárót használnak.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-113">Site-to-Site, Point-to-Site, and VNet-to-VNet connections all use a VPN gateway.</span></span>

<span data-ttu-id="ff3c6-114">Mindegyik virtuális hálózat csak egy virtuális hálózati átjáróval rendelkezhet átjárótípusonként.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-114">Each virtual network can have only one virtual network gateway per gateway type.</span></span> <span data-ttu-id="ff3c6-115">Rendelkezhet például egy virtuális hálózati átjáróval, amely a -GatewayType Vpn típust, és egy másikkal, amelyik a -GatewayType ExpressRoute típust használja.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-115">For example, you can have one virtual network gateway that uses -GatewayType Vpn, and one that uses -GatewayType ExpressRoute.</span></span> <span data-ttu-id="ff3c6-116">Ez a cikk foglalkozik az ExpressRoute virtuális hálózati átjáróhoz.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-116">This article focuses on the ExpressRoute virtual network gateway.</span></span>

## <span data-ttu-id="ff3c6-117"><a name="gwsku"></a>Átjáró-termékváltozatok</span><span class="sxs-lookup"><span data-stu-id="ff3c6-117"><a name="gwsku"></a>Gateway SKUs</span></span>
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

<span data-ttu-id="ff3c6-118">Ha szeretné frissíteni az átjárót egy nagyobb teljesítményű átjáró SKU, a legtöbb esetben használhatja a "Átméretezési-AzureRmVirtualNetworkGateway" PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-118">If you want to upgrade your gateway to a more powerful gateway SKU, in most cases you can use the 'Resize-AzureRmVirtualNetworkGateway' PowerShell cmdlet.</span></span> <span data-ttu-id="ff3c6-119">Ez a Standard és a HighPerformance termékváltozatok frissítésekre fog működni.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-119">This will work for upgrades to Standard and HighPerformance SKUs.</span></span> <span data-ttu-id="ff3c6-120">Azonban a UltraPerformance SKU frissítéséhez, akkor hozza létre újra az átjárót.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-120">However, to upgrade to the UltraPerformance SKU, you will need to recreate the gateway.</span></span>

### <span data-ttu-id="ff3c6-121"><a name="aggthroughput"></a>Összesített becsült gateway SKU</span><span class="sxs-lookup"><span data-stu-id="ff3c6-121"><a name="aggthroughput"></a>Estimated aggregate throughput by gateway SKU</span></span>
<span data-ttu-id="ff3c6-122">Az alábbi táblázatban az átjárótípusok és azok becsült összesített átviteli sebessége tekinthető meg.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-122">The following table shows the gateway types and the estimated aggregate throughput.</span></span> <span data-ttu-id="ff3c6-123">Ez a tábla a Resource Managerre és a klasszikus üzembe helyezési modellre is érvényes.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-123">This table applies to both the Resource Manager and classic deployment models.</span></span>

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="ff3c6-124">Alkalmazás átviteli sebességére több tényezőtől függ, többek között a végpontok közötti késés, és megnyílik az alkalmazás forgalom számát.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-124">Application throughput depends on multiple factors, such as the end-to-end latency, and the number of traffic flows the application opens.</span></span> <span data-ttu-id="ff3c6-125">A táblázatban szereplő számok jelölik a felső korlátja, amely az alkalmazás is theorectically elérése ideális környezetben.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-125">The numbers in the table represent the upper limit that the application can theorectically achieve in an ideal environment.</span></span> 
> 
>

## <span data-ttu-id="ff3c6-126"><a name="resources"></a>REST API-k és a PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="ff3c6-126"><a name="resources"></a>REST APIs and PowerShell cmdlets</span></span>
<span data-ttu-id="ff3c6-127">További technikai erőforrások és konkrét szintaxis használatával kapcsolatos követelményeket REST API-k és a PowerShell-parancsmagok a virtuális hálózati átjáró konfigurációjában: a következő lapok:</span><span class="sxs-lookup"><span data-stu-id="ff3c6-127">For additional technical resources and specific syntax requirements when using REST APIs and PowerShell cmdlets for virtual network gateway configurations, see the following pages:</span></span>

| <span data-ttu-id="ff3c6-128">**Klasszikus**</span><span class="sxs-lookup"><span data-stu-id="ff3c6-128">**Classic**</span></span> | <span data-ttu-id="ff3c6-129">**Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="ff3c6-129">**Resource Manager**</span></span> |
| --- | --- |
| [<span data-ttu-id="ff3c6-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ff3c6-130">PowerShell</span></span>](https://msdn.microsoft.com/library/mt270335.aspx) |[<span data-ttu-id="ff3c6-131">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ff3c6-131">PowerShell</span></span>](https://msdn.microsoft.com/library/mt163510.aspx) |
| [<span data-ttu-id="ff3c6-132">REST API</span><span class="sxs-lookup"><span data-stu-id="ff3c6-132">REST API</span></span>](https://msdn.microsoft.com/library/jj154113.aspx) |[<span data-ttu-id="ff3c6-133">REST API</span><span class="sxs-lookup"><span data-stu-id="ff3c6-133">REST API</span></span>](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a><span data-ttu-id="ff3c6-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ff3c6-134">Next steps</span></span>
<span data-ttu-id="ff3c6-135">Lásd: [ExpressRoute – áttekintés](expressroute-introduction.md) használható kapcsolat konfigurációival kapcsolatos további információkért.</span><span class="sxs-lookup"><span data-stu-id="ff3c6-135">See [ExpressRoute Overview](expressroute-introduction.md) for more information about available connection configurations.</span></span> 

