---
title: "A virtuális hálózat átjárót konfigurálhatja a PowerShell használatával ExpressRoute: klasszikus: Azure |} Microsoft Docs"
description: "Klasszikus üzembe helyezés hálózatok átjáró konfigurálásához modellhez tartozó virtuális hálózaton, ExpressRoute-konfigurációhoz a PowerShell használatával."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 85ee0bc1-55be-4760-bfb4-34d9f2c96f30
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: charwen
ms.openlocfilehash: 6f37d4d9cba546b5416ab99040f5ef6dae273380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>A virtuális hálózati átjáró konfigurálása az ExpressRoute (klasszikus) PowerShell használatával
> [!div class="op_single_selector"]
> * [Resource Manager – PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klasszikus - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure-portálon](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Ez a cikk részletesen ismerteti keresztül hello lépéseket tooadd, méretezze át, és távolítsa el a virtuális hálózathoz (VNet) átjáró egy már meglévő vnet. hello ehhez a konfigurációhoz lépésekre kifejezetten a Vnetek hello használatával létrehozott **klasszikus üzembe helyezési modellel** és fogja tárolni egy ExpressRoute-konfigurációja használható. 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Tudnivalók az Azure üzembe helyezési modelljeiről**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Mielőtt hozzálát
Győződjön meg arról, hogy telepítette-e az ehhez a konfigurációhoz szükséges hello Azure PowerShell-parancsmagok (1.0.2-es vagy újabb). Ha még nem telepítette a hello parancsmagok, szüksége lesz a toodo Igen hello konfigurációs lépések megkezdése előtt. További információ az Azure PowerShell telepítése: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>Következő lépések
Hello hálózatok átjáró létrehozása után a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot társíthatja. Lásd: [csatolni a virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot](expressroute-howto-linkvnet-classic.md).

