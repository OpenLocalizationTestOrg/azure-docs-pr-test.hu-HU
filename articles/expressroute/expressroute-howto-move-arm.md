---
title: "ExpressRoute-Kapcsolatcsoportok áthelyezése klasszikus tooResource Manager: PowerShell: Azure |} Microsoft Docs"
description: "Ez a lap ismerteti, hogyan toomove egy klasszikus expressroute toohello erőforrás-kezelő telepítési modell PowerShell használatával."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 08152836-23e7-42d1-9a56-8306b341cd91
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/03/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 8dcadafca5e4f40773902cec5786eba1dbe133eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-expressroute-circuits-from-hello-classic-toohello-resource-manager-deployment-model-using-powershell"></a>ExpressRoute-Kapcsolatcsoportok áthelyezése hello klasszikus toohello Resource Manager üzembe helyezési modellben PowerShell használatával

toouse ExpressRoute-kapcsolatcsoportot hello klasszikus és Resource Manager üzembe helyezési modellel, át kell helyezni hello áramkör toohello Resource Manager üzembe helyezési modellben. hello következő részek segítséget nyújtanak a kapcsolatcsoport áthelyezése a PowerShell használatával.

## <a name="before-you-begin"></a>Előkészületek

* Ellenőrizze, hogy rendelkezik-e hello legújabb verziójának hello Azure PowerShell-modulok (legalább 1.0-s verzió). További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).
* Győződjön meg arról, hogy átolvasta hello [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.
* Tekintse át a megadott hello információkat [ExpressRoute-kapcsolatcsoportot áthelyezése klasszikus tooResource Manager](expressroute-move.md). Győződjön meg arról, hogy megértette hello korlátai és korlátozásai.
* Ellenőrizze, hogy teljesen működőképes hello klasszikus üzembe helyezési modellel hello körön.
* Győződjön meg arról, hogy rendelkezik-e hello Resource Manager üzembe helyezési modellel létrehozott erőforráscsoport.

## <a name="move-an-expressroute-circuit"></a>Helyezze át az ExpressRoute-kapcsolatcsoportot

### <a name="step-1-gather-circuit-details-from-hello-classic-deployment-model"></a>1. lépés: Gyűjtsön áramkör részletek hello klasszikus telepítési modell

Jelentkezzen be a klasszikus Azure-környezetéhez toohello és hello szolgáltatás kulcs összegyűjtése.

1. Bejelentkezés tooyour Azure-fiók.

  ```powershell
  Add-AzureAccount
  ```

2. Válassza ki a hello megfelelő Azure-előfizetést.

  ```powershell
  Select-AzureSubscription "<Enter Subscription Name here>"
  ```

3. Hello PowerShell modul importálása az Azure és az ExpressRoute.

  ```powershell
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
  ```

4. Hello parancsmag tooget hello szolgáltatás kulcsok alatt az ExpressRoute-Kapcsolatcsoportok használni. Hello kulcsok beolvasása, után másolja a hello **szolgáltatás kulcs** hello áramkör, amelyet az toomove toohello Resource Manager üzembe helyezési modellben.

  ```powershell
  Get-AzureDedicatedCircuit
  ```

### <a name="step-2-sign-in-and-create-a-resource-group"></a>Jelentkezzen be a 2. lépés:, És hozzon létre egy erőforráscsoportot

Jelentkezzen be toohello Resource Manager-környezetben, és hozzon létre egy új erőforráscsoportot.

1. Jelentkezzen be tooyour Azure Resource Manager-környezetben.

  ```powershell
  Login-AzureRmAccount
  ```

2. Válassza ki a hello megfelelő Azure-előfizetést.

  ```powershell
  Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription
  ```

3. Ha még nem rendelkezik egy erőforráscsoport, módosítsa a hello részlet toocreate új erőforráscsoport alatt.

  ```powershell
  New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"
  ```

### <a name="step-3-move-hello-expressroute-circuit-toohello-resource-manager-deployment-model"></a>3. lépés: Helyezze át a hello ExpressRoute körön toohello Resource Manager telepítési modell

Ön most már készen áll a toomove az ExpressRoute-kapcsolatcsoportot hello klasszikus üzembe helyezési modell toohello Resource Manager üzembe helyezési modellben a rendszer. A folytatás előtt ellenőrizze a található hello információk [ExpressRoute-kapcsolatcsoportot áthelyezése hello klasszikus toohello Resource Manager üzembe helyezési modellben](expressroute-move.md).

a kapcsolatcsoport toomove módosíthatja, és futtassa a következő kódrészletet hello:

```powershell
Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"
```

> [!NOTE]
> Hello áthelyezés befejeződését követően hello új neve szerepel-e hello előző parancsmag lesz használt tooaddress hello erőforrás. hello áramkör lényegében lehet átnevezni.
> 

## <a name="modify-circuit-access"></a>Módosítsa a kapcsolatcsoport hozzáférés

### <a name="tooenable-expressroute-circuit-access-for-both-deployment-models"></a>ExpressRoute körön hozzáférés mindkét tooenable

Helyezze át a klasszikus ExpressRoute körön toohello Resource Manager telepítési modell, engedélyezheti a hozzáférést tooboth üzembe helyezési modellek. Futtassa a következő parancsmagok tooenable hozzáférés tooboth üzembe helyezési modellel hello:

1. Ezzel a hello áramkör adatokat.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. Állítsa be a "Klasszikus műveletek engedélyezése" tooTRUE.

  ```powershell
  $ckt.AllowClassicOperations = $true
  ```

3. Hello áramkör frissítése. Után ez a művelet sikeresen befejeződött, fogja tudni tooview hello áramkör hello klasszikus üzembe helyezési modellben.

  ```powershell
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

4. Futtassa a következő parancsmag tooget hello adatok az ExpressRoute-kapcsolatcsoportot hello hello. Képes toosee hello szolgáltatás kulcs felsorolt kell lennie.

  ```powershell
  get-azurededicatedcircuit
  ```

5. Hivatkozások ExpressRoute-kapcsolatcsoportot toohello parancsokkal hello klasszikus üzembe helyezési modell a hagyományos Vneteket, és hello erőforrás-kezelő parancsok az erőforrás-kezelő Vnetek kezelheti. hello következő cikkek segítenek hivatkozások toohello ExpressRoute-kapcsolatcsoportot kezelheti:

    * [A virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot hello Resource Manager üzembe helyezési modellel hivatkozás](expressroute-howto-linkvnet-arm.md)
    * [A virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot hello klasszikus üzembe helyezési modellel hivatkozás](expressroute-howto-linkvnet-classic.md)

### <a name="toodisable-expressroute-circuit-access-toohello-classic-deployment-model"></a>toodisable ExpressRoute körön hozzáférés toohello klasszikus telepítési modell

Futtassa a következő parancsmagok toodisable hozzáférés toohello klasszikus üzembe helyezési modellel hello.

1. Hello ExpressRoute-kapcsolatcsoport részleteinek beolvasása.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. Állítsa be a "Klasszikus műveletek engedélyezése" tooFALSE.

  ```powershell
  $ckt.AllowClassicOperations = $false
  ```

3. Hello áramkör frissítése. Után ez a művelet sikeresen befejeződött, nem fogja tudni tooview hello áramkör hello klasszikus üzembe helyezési modellben.

  ```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

## <a name="next-steps"></a>Következő lépések

* [Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoport esetében routing](expressroute-howto-routing-arm.md)
* [A virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot hivatkozás](expressroute-howto-linkvnet-arm.md)
