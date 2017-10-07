---
title: "aaaAbout ExpressRoute virtuális hálózati átjárók |} Microsoft Docs"
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
ms.openlocfilehash: 4daf4f96b4fadb00683d8e536e51734853008c50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="about-virtual-network-gateways-for-expressroute"></a><span data-ttu-id="4a4cb-103">Az ExpressRoute virtuális hálózati átjáróinak ismertetése</span><span class="sxs-lookup"><span data-stu-id="4a4cb-103">About virtual network gateways for ExpressRoute</span></span>
<span data-ttu-id="4a4cb-104">A virtuális hálózati átjáró használatos toosend hálózati forgalmat egy Azure virtuális hálózatot és a helyszíni helyek között.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-104">A virtual network gateway is used toosend network traffic between Azure virtual networks and on-premises locations.</span></span> <span data-ttu-id="4a4cb-105">Amikor konfigurál egy ExpressRoute-kapcsolatot, hozzon létre és konfigurálnia kell a virtuális hálózati átjáró és a virtuális hálózati átjáró kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-105">When you configure an ExpressRoute connection, you must create and configure a virtual network gateway and a virtual network gateway connection.</span></span>

<span data-ttu-id="4a4cb-106">Egy virtuális hálózati átjáró létrehozásakor több beállítást is meg kell adnia.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-106">When you create a virtual network gateway, you specify several settings.</span></span> <span data-ttu-id="4a4cb-107">Hello szükséges beállítások egyikének határozza meg, hogy hello átjáró expressroute-on vagy a telephelyek közötti VPN-forgalomhoz használható.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-107">One of hello required settings specifies whether hello gateway will be used for ExpressRoute or Site-to-Site VPN traffic.</span></span> <span data-ttu-id="4a4cb-108">Hello Resource Manager üzembe helyezési modellel, hello beállítás "-GatewayType".</span><span class="sxs-lookup"><span data-stu-id="4a4cb-108">In hello Resource Manager deployment model, hello setting is '-GatewayType'.</span></span>

<span data-ttu-id="4a4cb-109">Amikor a hálózati adatforgalom a kapcsolatot, hello átjáró típusa "ExpressRoute" használhatja.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-109">When network traffic is sent on a private connection, you use hello gateway type 'ExpressRoute'.</span></span> <span data-ttu-id="4a4cb-110">Ez a hivatkozott tooas egy ExpressRoute-átjárót is.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-110">This is also referred tooas an ExpressRoute gateway.</span></span> <span data-ttu-id="4a4cb-111">Ha hálózati adatforgalom titkosított között hello a nyilvános interneten, hello átjáró típusa "Vpn" használhatja.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-111">When network traffic is sent encrypted across hello public Internet, you use hello gateway type 'Vpn'.</span></span> <span data-ttu-id="4a4cb-112">Ez a hivatkozott tooas VPN-átjáró.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-112">This is referred tooas a VPN gateway.</span></span> <span data-ttu-id="4a4cb-113">A hely–hely, pont–hely és a virtuális hálózatok közötti kapcsolat kapcsolatok mind VPN-átjárót használnak.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-113">Site-to-Site, Point-to-Site, and VNet-to-VNet connections all use a VPN gateway.</span></span>

<span data-ttu-id="4a4cb-114">Mindegyik virtuális hálózat csak egy virtuális hálózati átjáróval rendelkezhet átjárótípusonként.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-114">Each virtual network can have only one virtual network gateway per gateway type.</span></span> <span data-ttu-id="4a4cb-115">Rendelkezhet például egy virtuális hálózati átjáróval, amely a -GatewayType Vpn típust, és egy másikkal, amelyik a -GatewayType ExpressRoute típust használja.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-115">For example, you can have one virtual network gateway that uses -GatewayType Vpn, and one that uses -GatewayType ExpressRoute.</span></span> <span data-ttu-id="4a4cb-116">Ez a cikk hello ExpressRoute virtuális hálózati átjáró összpontosít.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-116">This article focuses on hello ExpressRoute virtual network gateway.</span></span>

## <span data-ttu-id="4a4cb-117"><a name="gwsku"></a>Átjáró-termékváltozatok</span><span class="sxs-lookup"><span data-stu-id="4a4cb-117"><a name="gwsku"></a>Gateway SKUs</span></span>
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

<span data-ttu-id="4a4cb-118">Ha azt szeretné, tooupgrade az átjáró tooa nagyobb teljesítményű átjáró Termékváltozat, a legtöbb esetben használhatja hello "Átméretezési-AzureRmVirtualNetworkGateway" PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-118">If you want tooupgrade your gateway tooa more powerful gateway SKU, in most cases you can use hello 'Resize-AzureRmVirtualNetworkGateway' PowerShell cmdlet.</span></span> <span data-ttu-id="4a4cb-119">Ez a frissítés tooStandard és HighPerformance termékváltozatok fog működni.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-119">This will work for upgrades tooStandard and HighPerformance SKUs.</span></span> <span data-ttu-id="4a4cb-120">Azonban tooupgrade toohello UltraPerformance SKU, szüksége lesz toorecreate hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-120">However, tooupgrade toohello UltraPerformance SKU, you will need toorecreate hello gateway.</span></span>

### <span data-ttu-id="4a4cb-121"><a name="aggthroughput"></a>Összesített becsült gateway SKU</span><span class="sxs-lookup"><span data-stu-id="4a4cb-121"><a name="aggthroughput"></a>Estimated aggregate throughput by gateway SKU</span></span>
<span data-ttu-id="4a4cb-122">hello következő táblázatban hello átjárótípusok és hello becsült összesített teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-122">hello following table shows hello gateway types and hello estimated aggregate throughput.</span></span> <span data-ttu-id="4a4cb-123">Ez a táblázat tooboth hello Resource Manager és klasszikus üzembe helyezési modellre vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-123">This table applies tooboth hello Resource Manager and classic deployment models.</span></span>

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="4a4cb-124">Alkalmazás átviteli sebességére több tényezőtől függ, többek között hello végpontok közötti késés, és a forgalom hello száma forgalomáramlás hello elindul.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-124">Application throughput depends on multiple factors, such as hello end-to-end latency, and hello number of traffic flows hello application opens.</span></span> <span data-ttu-id="4a4cb-125">hello tábla jelentik hello felső korlátja, hogy hello alkalmazás is theorectically hello számok elérése ideális környezetben.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-125">hello numbers in hello table represent hello upper limit that hello application can theorectically achieve in an ideal environment.</span></span> 
> 
>

## <span data-ttu-id="4a4cb-126"><a name="resources"></a>REST API-k és a PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="4a4cb-126"><a name="resources"></a>REST APIs and PowerShell cmdlets</span></span>
<span data-ttu-id="4a4cb-127">További technikai erőforrások és a konkrét szintaxis használatával kapcsolatos követelményeket REST API-k és a PowerShell-parancsmagok a virtuális hálózati átjáró konfigurációjában tekintse meg a következő lapok hello:</span><span class="sxs-lookup"><span data-stu-id="4a4cb-127">For additional technical resources and specific syntax requirements when using REST APIs and PowerShell cmdlets for virtual network gateway configurations, see hello following pages:</span></span>

| <span data-ttu-id="4a4cb-128">**Klasszikus**</span><span class="sxs-lookup"><span data-stu-id="4a4cb-128">**Classic**</span></span> | <span data-ttu-id="4a4cb-129">**Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="4a4cb-129">**Resource Manager**</span></span> |
| --- | --- |
| [<span data-ttu-id="4a4cb-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4a4cb-130">PowerShell</span></span>](https://msdn.microsoft.com/library/mt270335.aspx) |[<span data-ttu-id="4a4cb-131">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4a4cb-131">PowerShell</span></span>](https://msdn.microsoft.com/library/mt163510.aspx) |
| [<span data-ttu-id="4a4cb-132">REST API</span><span class="sxs-lookup"><span data-stu-id="4a4cb-132">REST API</span></span>](https://msdn.microsoft.com/library/jj154113.aspx) |[<span data-ttu-id="4a4cb-133">REST API</span><span class="sxs-lookup"><span data-stu-id="4a4cb-133">REST API</span></span>](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a><span data-ttu-id="4a4cb-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4a4cb-134">Next steps</span></span>
<span data-ttu-id="4a4cb-135">Lásd: [ExpressRoute – áttekintés](expressroute-introduction.md) használható kapcsolat konfigurációival kapcsolatos további információkért.</span><span class="sxs-lookup"><span data-stu-id="4a4cb-135">See [ExpressRoute Overview](expressroute-introduction.md) for more information about available connection configurations.</span></span> 

