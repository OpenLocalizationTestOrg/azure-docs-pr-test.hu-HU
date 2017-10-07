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
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Virtuális hálózati átjáró termékváltozatok (örökölt SKU) használata

Ez a cikk hello örökölt (régi) virtuális hálózati átjáró termékváltozatok információkat tartalmaz. hello örökölt termékváltozatok továbbra is működni a két üzembe helyezési modell, amely már létre van hozva VPN-átjárók. Hagyományos VPN-átjárók toouse továbbra is a hagyományos SKU, meglévő átjáró, valamint az új átjáró hello. Új erőforrás-kezelő VPN átjáró létrehozása esetén hello új átjáró termékváltozatok használatára. Hello információ új termékváltozatok: [VPN-átjáró](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>Átjáró-termékváltozatok

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

## <a name="agg"></a>A Termékváltozat becsült összesített átviteli sebesség

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>Termékváltozat- és VPN-típus által támogatott konfigurációk

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Automatikus oszlopszélesség egy átjáró (egy átjáró SKU módosítása)

Egy átjárót SKU hello átméretezheti azonos SKU-család. Például ha egy Standard Termékváltozat, átméretezheti tooa HighPerformance Termékváltozat. Nem lehet átméretezni a VPN-átjárók közötti hello régi termékváltozatok és új SKU-családok hello. Például a egy Standard Termékváltozat tooa VpnGw2 Termékváltozat nem léphet. 

egy átjáró SKU hello klasszikus telepítési modell a következő parancs használata hello tooresize:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

átjáró SKU a Resource Manager üzembe helyezési modelljével hello tooresize hello a következő parancsot használja:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="migrate"></a>Új átjáró toohello termékváltozatok áttelepítése

Hello Resource Manager üzembe helyezési modellben dolgozik, toohello új átjáró Termékváltozatok áttelepíthető. Dolgozunk a hello klasszikus üzembe helyezési modellel, ha toohello nem telepíthető át új termékváltozatok és helyette továbbra is a toouse örökölt termékváltozatok hello.

[!INCLUDE [Migrate SKU](../../includes/vpn-gateway-migrate-legacy-sku-include.md)]

## <a name="next-steps"></a>Következő lépések

További információ a hello új Gateway SKU-n, lásd: [Gateway SKU-n](vpn-gateway-about-vpngateways.md#gwsku).

Konfigurációs beállításaival kapcsolatos további információkért lásd: [VPN-átjáró konfigurációs beállítások](vpn-gateway-about-vpn-gateway-settings.md).