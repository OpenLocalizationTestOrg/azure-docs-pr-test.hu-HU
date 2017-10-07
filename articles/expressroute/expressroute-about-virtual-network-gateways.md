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
# <a name="about-virtual-network-gateways-for-expressroute"></a>Az ExpressRoute virtuális hálózati átjáróinak ismertetése
A virtuális hálózati átjáró használatos toosend hálózati forgalmat egy Azure virtuális hálózatot és a helyszíni helyek között. Amikor konfigurál egy ExpressRoute-kapcsolatot, hozzon létre és konfigurálnia kell a virtuális hálózati átjáró és a virtuális hálózati átjáró kapcsolat.

Egy virtuális hálózati átjáró létrehozásakor több beállítást is meg kell adnia. Hello szükséges beállítások egyikének határozza meg, hogy hello átjáró expressroute-on vagy a telephelyek közötti VPN-forgalomhoz használható. Hello Resource Manager üzembe helyezési modellel, hello beállítás "-GatewayType".

Amikor a hálózati adatforgalom a kapcsolatot, hello átjáró típusa "ExpressRoute" használhatja. Ez a hivatkozott tooas egy ExpressRoute-átjárót is. Ha hálózati adatforgalom titkosított között hello a nyilvános interneten, hello átjáró típusa "Vpn" használhatja. Ez a hivatkozott tooas VPN-átjáró. A hely–hely, pont–hely és a virtuális hálózatok közötti kapcsolat kapcsolatok mind VPN-átjárót használnak.

Mindegyik virtuális hálózat csak egy virtuális hálózati átjáróval rendelkezhet átjárótípusonként. Rendelkezhet például egy virtuális hálózati átjáróval, amely a -GatewayType Vpn típust, és egy másikkal, amelyik a -GatewayType ExpressRoute típust használja. Ez a cikk hello ExpressRoute virtuális hálózati átjáró összpontosít.

## <a name="gwsku"></a>Átjáró-termékváltozatok
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Ha azt szeretné, tooupgrade az átjáró tooa nagyobb teljesítményű átjáró Termékváltozat, a legtöbb esetben használhatja hello "Átméretezési-AzureRmVirtualNetworkGateway" PowerShell-parancsmagot. Ez a frissítés tooStandard és HighPerformance termékváltozatok fog működni. Azonban tooupgrade toohello UltraPerformance SKU, szüksége lesz toorecreate hello átjáró.

### <a name="aggthroughput"></a>Összesített becsült gateway SKU
hello következő táblázatban hello átjárótípusok és hello becsült összesített teljesítményt. Ez a táblázat tooboth hello Resource Manager és klasszikus üzembe helyezési modellre vonatkozik.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Alkalmazás átviteli sebességére több tényezőtől függ, többek között hello végpontok közötti késés, és a forgalom hello száma forgalomáramlás hello elindul. hello tábla jelentik hello felső korlátja, hogy hello alkalmazás is theorectically hello számok elérése ideális környezetben. 
> 
>

## <a name="resources"></a>REST API-k és a PowerShell-parancsmagok
További technikai erőforrások és a konkrét szintaxis használatával kapcsolatos követelményeket REST API-k és a PowerShell-parancsmagok a virtuális hálózati átjáró konfigurációjában tekintse meg a következő lapok hello:

| **Klasszikus** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://msdn.microsoft.com/library/mt270335.aspx) |[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx) |
| [REST API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Következő lépések
Lásd: [ExpressRoute – áttekintés](expressroute-introduction.md) használható kapcsolat konfigurációival kapcsolatos további információkért. 

