---
title: "ExpressRoute társított virtuális hálózatok át klasszikus tooResource Manager: Azure: PowerShell |} Microsoft Docs"
description: "Ez a lap ismerteti, hogyan toomigrate tartozó helyezze át a kapcsolatcsoport virtuális hálózatok tooResource Manager."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: e64506c6909296f98c5dd23b1437bc0b81f31c75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-expressroute-associated-virtual-networks-from-classic-tooresource-manager"></a>Az ExpressRoute társított virtuális hálózatok át klasszikus tooResource Manager

Ez a cikk azt ismerteti, hogyan toomigrate Azure ExpressRoute tartozó helyezze át az ExpressRoute-kapcsolatcsoportot hello klasszikus telepítési modell toohello Azure Resource Manager telepítési modell virtuális hálózatokat. 


## <a name="before-you-begin"></a>Előkészületek
* Győződjön meg arról, hogy rendelkezik-e hello hello Azure PowerShell-modulok legújabb verzióját. További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).
* Győződjön meg arról, hogy átolvasta hello [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.
* Tekintse át a megadott hello információkat [ExpressRoute-kapcsolatcsoportot áthelyezése klasszikus tooResource Manager](expressroute-move.md). Győződjön meg arról, hogy megértette hello korlátai és korlátozásai.
* Ellenőrizze, hogy teljesen működőképes hello klasszikus üzembe helyezési modellel hello körön.
* Győződjön meg arról, hogy rendelkezik-e hello Resource Manager üzembe helyezési modellel létrehozott erőforráscsoport.
* Tekintse át a következő erőforrás-áttelepítési dokumentáció hello:

    * [IaaS-erőforrásokra, erőforrás-kezelő klasszikus tooAzure a platform által támogatott áttelepítése](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus tooAzure erőforrás-kezelő](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
    * [Gyakori kérdések: IaaS-erőforrásokra, erőforrás-kezelő klasszikus tooAzure a Platform által támogatott áttelepítése](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Tekintse át a leggyakrabban előforduló áttelepítést hibákat, és azok mérséklési](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="supported-and-unsupported-scenarios"></a>Támogatott és nem támogatott forgatókönyvek

* Egy ExpressRoute-kapcsolatcsoportot hello klasszikus toohello erőforrás-kezelő környezetből leállítás nélkül helyezheti át. Hello klasszikus toohello állásidő nélkül Resource Manager környezetet át lehet ExpressRoute-kapcsolatcsoportot. Hello utasításait követve [ExpressRoute-Kapcsolatcsoportok áthelyezése hello klasszikus toohello Resource Manager üzembe helyezési modellben PowerShell használatával](expressroute-howto-move-arm.md). Ez az előfeltételként szükséges toomove erőforrások csatlakoztatott toohello virtuális hálózatot.
* Virtuális hálózatok, az átjárók és a társított központi telepítések hello virtuális hálózaton belül, amelyek ugyanahhoz az előfizetéshez lehet hello az ExpressRoute-kapcsolatcsoportot csatolt tooan át toohello erőforrás-kezelő környezet állásidő nélkül. Hello lépéseket ismertetett újabb toomigrate erőforrások, például a telepített hello virtuális hálózaton belüli virtuális gépek, virtuális hálózatok és átjárók követésével. Gondoskodnia kell arról, hogy hello virtuális hálózatok helyesen vannak konfigurálva az áttelepítés előtt. 
* Virtuális hálózatok, az átjárók és a társított központi telepítések, amelyek nincsenek a hello virtuális hálózaton belül, ExpressRoute-kapcsolatcsoportot hello némi állásidőt toocomplete hello át kell hello ugyanahhoz az előfizetéshez. hello dokumentum utolsó szakasza hello hello lépéseit követni toobe toomigrate erőforrások ismerteti.
* Egy virtuális hálózaton, ExpressRoute-átjáró és a VPN-átjáró nem telepíthető át.

## <a name="move-an-expressroute-circuit-from-classic-tooresource-manager"></a>Klasszikus tooResource Manager ExpressRoute-kapcsolatcsoportot áthelyezése
Egy ExpressRoute-kapcsolatcsoportot hello klasszikus toohello erőforrás-kezelő környezet előtt toomigrate erőforrásokat, amelyek a csatolt toohello ExpressRoute-kapcsolatcsoportot kell áthelyeznie. tooaccomplish ennek a feladatnak, tekintse meg a következő cikkek hello:

* Tekintse át a megadott hello információkat [ExpressRoute-kapcsolatcsoportot áthelyezése klasszikus tooResource Manager](expressroute-move.md).
* [Expressroute-kapcsolatcsoporthoz áthelyezése az Azure PowerShell Manager klasszikus tooResource](expressroute-howto-move-arm.md).
* Hello Azure szolgáltatásfelügyeleti portált használja. Követésével hello munkafolyamat túl[hozzon létre egy új ExpressRoute-kapcsolatcsoportot](expressroute-howto-circuit-portal-resource-manager.md) hello importálása lehetőséget válassza. 

Ez a művelet nem jár állásidővel. A helyszíni és a Microsoft közötti tootransfer adatokat is folytatni, amíg hello áttelepítés van folyamatban.

## <a name="migrate-virtual-networks-gateways-and-associated-deployments"></a>Telepítse át a virtuális hálózatok, az átjárók és a társított központi telepítések

hello hajtsa végre a toomigrate függ, hogy az erőforrások vannak-e a hello ugyanahhoz az előfizetéshez, különböző előfizetésekhez vagy mindkettőt.

### <a name="migrate-virtual-networks-gateways-and-associated-deployments-in-hello-same-subscription-as-hello-expressroute-circuit"></a>Telepítse át a virtuális hálózatok, átjárók, és a társított központi telepítések hello ugyanahhoz az előfizetéshez, ExpressRoute-kapcsolatcsoportot hello
Ez a szakasz ismerteti a hello lépéseit követni toobe toomigrate egy virtuális hálózati átjáró és a társított központi telepítések hello ugyanahhoz az előfizetéshez, ExpressRoute-kapcsolatcsoportot hello. Nincs leállás az áttelepítés tartozik. Folytatás toouse hello áttelepítési folyamatot, az összes erőforrást. hello felügyeleti vezérlősík zárolva van, amíg hello áttelepítés van folyamatban. 

1. Győződjön meg arról, hogy hello ExpressRoute-kapcsolatcsoportot hello klasszikus toohello erőforrás-kezelő környezetből át lett helyezve.
2. Győződjön meg arról, hogy hello virtuális hálózati készült megfelelően hello áttelepítési.
3. Az előfizetés regisztrálása a erőforrás áttelepítése. tooregister erőforrás áttelepítési, a következő PowerShell-részlet használata hello előfizetését:

  ```powershell 
  Select-AzureRmSubscription -SubscriptionName <Your Subscription Name>
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  ```
4. Ellenőrizze, előkészítéséhez és áttelepítése. toomove hello virtuális hálózat, a következő PowerShell-részlet használata hello:

  ```powershell
  Move-AzureVirtualNetwork -Prepare $vnetName  
  Move-AzureVirtualNetwork -Commit $vnetName
  ```

  Áttelepítési hello a következő PowerShell-parancsmag futtatásával is megszakítja:

  ```powershell
  Move-AzureVirtualNetwork -Abort $vnetName
  ```

## <a name="next-steps"></a>Következő lépések
* [IaaS-erőforrásokra, erőforrás-kezelő klasszikus tooAzure a platform által támogatott áttelepítése](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus tooAzure erőforrás-kezelő](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
* [Gyakori kérdések: IaaS-erőforrásokra, erőforrás-kezelő klasszikus tooAzure a Platform által támogatott áttelepítése](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Tekintse át a leggyakrabban előforduló áttelepítést hibákat, és azok mérséklési](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
