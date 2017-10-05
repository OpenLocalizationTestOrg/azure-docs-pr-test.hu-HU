---
title: "Örökölt Azure virtuális hálózati átjáró termékváltozatok |} Microsoft Docs"
description: "A régi virtuális hálózati átjáró-termékváltozat."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 3b2126b1ecd1613950bbf311ae08fafd4af0d51f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a><span data-ttu-id="0db85-103">Virtuális hálózati átjáró termékváltozatok (örökölt SKU) használata</span><span class="sxs-lookup"><span data-stu-id="0db85-103">Working with virtual network gateway SKUs (legacy SKUs)</span></span>

<span data-ttu-id="0db85-104">Ez a cikk az örökölt (régi) virtuális hálózati átjáró termékváltozatok információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0db85-104">This article contains information about the legacy (old) virtual network gateway SKUs.</span></span> <span data-ttu-id="0db85-105">Az örökölt termékváltozatok továbbra is működni a két üzembe helyezési modell, amely már létre van hozva VPN-átjárók.</span><span class="sxs-lookup"><span data-stu-id="0db85-105">The legacy SKUs still work in both deployment models for VPN gateways that have already been created.</span></span> <span data-ttu-id="0db85-106">Hagyományos VPN-átjárók továbbra is a hagyományos termékváltozatok meglévő átjáró, valamint az új átjáró.</span><span class="sxs-lookup"><span data-stu-id="0db85-106">Classic VPN gateways continue to use the legacy SKUs, both for existing gateways, and for new gateways.</span></span> <span data-ttu-id="0db85-107">Új erőforrás-kezelő VPN átjáró létrehozása esetén az új átjáró termékváltozatok használatához.</span><span class="sxs-lookup"><span data-stu-id="0db85-107">When creating new Resource Manager VPN gateways, use the new gateway SKUs.</span></span> <span data-ttu-id="0db85-108">Az új termékváltozatok kapcsolatos információkért lásd: [VPN-átjáró](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="0db85-108">For information about the new SKUs, see [About VPN Gateway](vpn-gateway-about-vpngateways.md).</span></span>

## <span data-ttu-id="0db85-109"><a name="gwsku"></a>Átjáró-termékváltozatok</span><span class="sxs-lookup"><span data-stu-id="0db85-109"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <span data-ttu-id="0db85-110"><a name="agg"></a>A Termékváltozat becsült összesített átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="0db85-110"><a name="agg"></a>Estimated aggregate throughput by SKU</span></span>

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <span data-ttu-id="0db85-111"><a name="config"></a>Termékváltozat- és VPN-típus által támogatott konfigurációk</span><span class="sxs-lookup"><span data-stu-id="0db85-111"><a name="config"></a>Supported configurations by SKU and VPN type</span></span>

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <span data-ttu-id="0db85-112"><a name="resize"></a>Automatikus oszlopszélesség egy átjáró (egy átjáró SKU módosítása)</span><span class="sxs-lookup"><span data-stu-id="0db85-112"><a name="resize"></a>Resize a gateway (change a gateway SKU)</span></span>

<span data-ttu-id="0db85-113">Egy átjáró SKU belül az azonos SKU-család is átméretezhetők.</span><span class="sxs-lookup"><span data-stu-id="0db85-113">You can resize a gateway SKU within the same SKU family.</span></span> <span data-ttu-id="0db85-114">Például ha egy Standard Termékváltozat, átméretezheti a HighPerformance másikra.</span><span class="sxs-lookup"><span data-stu-id="0db85-114">For example, if you have a Standard SKU, you can resize to a HighPerformance SKU.</span></span> <span data-ttu-id="0db85-115">A VPN-átjárók között a régi termékváltozatok és a új SKU-családja nem méretezhető át.</span><span class="sxs-lookup"><span data-stu-id="0db85-115">You can't resize your VPN gateways between the old SKUs and the new SKU families.</span></span> <span data-ttu-id="0db85-116">Például nem folytatható a szabványos Termékváltozatáról egy VpnGw2 Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="0db85-116">For example, you can't go from a Standard SKU to a VpnGw2 SKU.</span></span> 

<span data-ttu-id="0db85-117">A klasszikus üzembe helyezési modell átjáró SKU átméretezéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0db85-117">To resize a gateway SKU for the classic deployment model, use the following command:</span></span>

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

<span data-ttu-id="0db85-118">A Resource Manager üzembe helyezési modellel átjárót SKU átméretezéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0db85-118">To resize a gateway SKU for the Resource Manager deployment model, use the following command:</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <span data-ttu-id="0db85-119"><a name="migrate"></a>Telepítse át az új átjáró termékváltozatok</span><span class="sxs-lookup"><span data-stu-id="0db85-119"><a name="migrate"></a>Migrate to the new gateway SKUs</span></span>

<span data-ttu-id="0db85-120">Dolgozunk a Resource Manager üzembe helyezési modellel, ha az új átjáróra Termékváltozatok áttelepítheti.</span><span class="sxs-lookup"><span data-stu-id="0db85-120">If you are working with the Resource Manager deployment model, you can migrate to the new gateway SKUS.</span></span> <span data-ttu-id="0db85-121">Dolgozunk a klasszikus üzembe helyezési modellel, ha az új SKU nem telepíthető át, és helyette továbbra is a hagyományos SKU.</span><span class="sxs-lookup"><span data-stu-id="0db85-121">If you are working with the classic deployment model, you can't migrate to the new SKUs and must instead continue to use the legacy SKUs.</span></span>

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a><span data-ttu-id="0db85-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0db85-122">Next steps</span></span>

<span data-ttu-id="0db85-123">Az új átjáró-termékváltozat kapcsolatos további információkért lásd: [Gateway SKU-n](vpn-gateway-about-vpngateways.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="0db85-123">For more information about the new Gateway SKUs, see [Gateway SKUs](vpn-gateway-about-vpngateways.md#gwsku).</span></span>

<span data-ttu-id="0db85-124">Konfigurációs beállításaival kapcsolatos további információkért lásd: [VPN-átjáró konfigurációs beállítások](vpn-gateway-about-vpn-gateway-settings.md).</span><span class="sxs-lookup"><span data-stu-id="0db85-124">For more information about configuration settings, see [About VPN Gateway configuration settings](vpn-gateway-about-vpn-gateway-settings.md).</span></span>