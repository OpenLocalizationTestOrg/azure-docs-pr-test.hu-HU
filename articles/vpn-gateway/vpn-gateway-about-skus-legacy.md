---
title: "aaaLegacy Azure virtuális hálózati átjáró termékváltozatok |} Microsoft Docs"
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
ms.openlocfilehash: 710417581423d2fbc62827cab7949f2e137c5996
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a><span data-ttu-id="4be39-103">Virtuális hálózati átjáró termékváltozatok (örökölt SKU) használata</span><span class="sxs-lookup"><span data-stu-id="4be39-103">Working with virtual network gateway SKUs (legacy SKUs)</span></span>

<span data-ttu-id="4be39-104">Ez a cikk hello örökölt (régi) virtuális hálózati átjáró termékváltozatok információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4be39-104">This article contains information about hello legacy (old) virtual network gateway SKUs.</span></span> <span data-ttu-id="4be39-105">hello örökölt termékváltozatok továbbra is működni a két üzembe helyezési modell, amely már létre van hozva VPN-átjárók.</span><span class="sxs-lookup"><span data-stu-id="4be39-105">hello legacy SKUs still work in both deployment models for VPN gateways that have already been created.</span></span> <span data-ttu-id="4be39-106">Hagyományos VPN-átjárók toouse továbbra is a hagyományos SKU, meglévő átjáró, valamint az új átjáró hello.</span><span class="sxs-lookup"><span data-stu-id="4be39-106">Classic VPN gateways continue toouse hello legacy SKUs, both for existing gateways, and for new gateways.</span></span> <span data-ttu-id="4be39-107">Új erőforrás-kezelő VPN átjáró létrehozása esetén hello új átjáró termékváltozatok használatára.</span><span class="sxs-lookup"><span data-stu-id="4be39-107">When creating new Resource Manager VPN gateways, use hello new gateway SKUs.</span></span> <span data-ttu-id="4be39-108">Hello információ új termékváltozatok: [VPN-átjáró](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="4be39-108">For information about hello new SKUs, see [About VPN Gateway](vpn-gateway-about-vpngateways.md).</span></span>

## <span data-ttu-id="4be39-109"><a name="gwsku"></a>Átjáró-termékváltozatok</span><span class="sxs-lookup"><span data-stu-id="4be39-109"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <span data-ttu-id="4be39-110"><a name="agg"></a>A Termékváltozat becsült összesített átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="4be39-110"><a name="agg"></a>Estimated aggregate throughput by SKU</span></span>

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <span data-ttu-id="4be39-111"><a name="config"></a>Termékváltozat- és VPN-típus által támogatott konfigurációk</span><span class="sxs-lookup"><span data-stu-id="4be39-111"><a name="config"></a>Supported configurations by SKU and VPN type</span></span>

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <span data-ttu-id="4be39-112"><a name="resize"></a>Automatikus oszlopszélesség egy átjáró (egy átjáró SKU módosítása)</span><span class="sxs-lookup"><span data-stu-id="4be39-112"><a name="resize"></a>Resize a gateway (change a gateway SKU)</span></span>

<span data-ttu-id="4be39-113">Egy átjárót SKU hello átméretezheti azonos SKU-család.</span><span class="sxs-lookup"><span data-stu-id="4be39-113">You can resize a gateway SKU within hello same SKU family.</span></span> <span data-ttu-id="4be39-114">Például ha egy Standard Termékváltozat, átméretezheti tooa HighPerformance Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="4be39-114">For example, if you have a Standard SKU, you can resize tooa HighPerformance SKU.</span></span> <span data-ttu-id="4be39-115">Nem lehet átméretezni a VPN-átjárók közötti hello régi termékváltozatok és új SKU-családok hello.</span><span class="sxs-lookup"><span data-stu-id="4be39-115">You can't resize your VPN gateways between hello old SKUs and hello new SKU families.</span></span> <span data-ttu-id="4be39-116">Például a egy Standard Termékváltozat tooa VpnGw2 Termékváltozat nem léphet.</span><span class="sxs-lookup"><span data-stu-id="4be39-116">For example, you can't go from a Standard SKU tooa VpnGw2 SKU.</span></span> 

<span data-ttu-id="4be39-117">egy átjáró SKU hello klasszikus telepítési modell a következő parancs használata hello tooresize:</span><span class="sxs-lookup"><span data-stu-id="4be39-117">tooresize a gateway SKU for hello classic deployment model, use hello following command:</span></span>

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

<span data-ttu-id="4be39-118">átjáró SKU a Resource Manager üzembe helyezési modelljével hello tooresize hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="4be39-118">tooresize a gateway SKU for hello Resource Manager deployment model, use hello following command:</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <span data-ttu-id="4be39-119"><a name="migrate"></a>Új átjáró toohello termékváltozatok áttelepítése</span><span class="sxs-lookup"><span data-stu-id="4be39-119"><a name="migrate"></a>Migrate toohello new gateway SKUs</span></span>

<span data-ttu-id="4be39-120">Hello Resource Manager üzembe helyezési modellben dolgozik, toohello új átjáró Termékváltozatok áttelepíthető.</span><span class="sxs-lookup"><span data-stu-id="4be39-120">If you are working with hello Resource Manager deployment model, you can migrate toohello new gateway SKUS.</span></span> <span data-ttu-id="4be39-121">Dolgozunk a hello klasszikus üzembe helyezési modellel, ha toohello nem telepíthető át új termékváltozatok és helyette továbbra is a toouse örökölt termékváltozatok hello.</span><span class="sxs-lookup"><span data-stu-id="4be39-121">If you are working with hello classic deployment model, you can't migrate toohello new SKUs and must instead continue toouse hello legacy SKUs.</span></span>

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a><span data-ttu-id="4be39-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4be39-122">Next steps</span></span>

<span data-ttu-id="4be39-123">További információ a hello új Gateway SKU-n, lásd: [Gateway SKU-n](vpn-gateway-about-vpngateways.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="4be39-123">For more information about hello new Gateway SKUs, see [Gateway SKUs](vpn-gateway-about-vpngateways.md#gwsku).</span></span>

<span data-ttu-id="4be39-124">Konfigurációs beállításaival kapcsolatos további információkért lásd: [VPN-átjáró konfigurációs beállítások](vpn-gateway-about-vpn-gateway-settings.md).</span><span class="sxs-lookup"><span data-stu-id="4be39-124">For more information about configuration settings, see [About VPN Gateway configuration settings](vpn-gateway-about-vpn-gateway-settings.md).</span></span>