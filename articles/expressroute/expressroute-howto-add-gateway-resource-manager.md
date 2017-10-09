---
title: "Vegyen fel egy virtuális hálózati átjáró tooa VNet ExpressRoute: PowerShell: Azure |} Microsoft Docs"
description: "Ez a cikk végigvezeti egy már létrehozott ExpressRoute az erőforrás-kezelő VNet hálózatok átjáró tooan hozzáadása."
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 63e0bd60-abad-4963-8e27-3aa973e0d968
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2017
ms.author: charwen
ms.openlocfilehash: 8983430b426ad7c4af766294fa16427c5e9df5c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell"></a>Az ExpressRoute virtuális hálózati átjáróinak konfigurálása a PowerShell-lel
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager – PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasszikus - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure-portálon](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Ez a cikk bemutatja, hogyan hello lépéseket tooadd, méretezze át, és távolítsa el a virtuális hálózathoz (VNet) átjáró egy már meglévő vnet. hello ehhez a konfigurációhoz lépésekre kifejezetten a Vnetek hello Resource Manager telepítési modell ExpressRoute konfigurációban használt használatával létrehozott. További információ a virtuális hálózati átjárók és az átjáró konfigurációs beállításainak ExpressRoute: [kapcsolatos az ExpressRoute virtuális hálózati átjárók](expressroute-about-virtual-network-gateways.md). 


## <a name="before-beginning"></a>Mielőtt hozzálát
Győződjön meg arról, hogy telepítette-e az hello legújabb Azure PowerShell-parancsmagokat. Ha még nem telepítette a hello legújabb parancsmagok, szükség van-e toodo Igen hello konfigurációs lépések megkezdése előtt. További információk: [Az Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

## <a name="next-steps"></a>Következő lépések
Hello hálózatok átjáró létrehozása után a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot társíthatja. Lásd: [csatolni a virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-arm.md).

