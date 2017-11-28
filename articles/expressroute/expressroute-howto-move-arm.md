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
# <a name="move-expressroute-circuits-from-hello-classic-toohello-resource-manager-deployment-model-using-powershell"></a><span data-ttu-id="14518-103">ExpressRoute-Kapcsolatcsoportok áthelyezése hello klasszikus toohello Resource Manager üzembe helyezési modellben PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="14518-103">Move ExpressRoute circuits from hello classic toohello Resource Manager deployment model using PowerShell</span></span>

<span data-ttu-id="14518-104">toouse ExpressRoute-kapcsolatcsoportot hello klasszikus és Resource Manager üzembe helyezési modellel, át kell helyezni hello áramkör toohello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="14518-104">toouse an ExpressRoute circuit for both hello classic and Resource Manager deployment models, you must move hello circuit toohello Resource Manager deployment model.</span></span> <span data-ttu-id="14518-105">hello következő részek segítséget nyújtanak a kapcsolatcsoport áthelyezése a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="14518-105">hello following sections help you move your circuit by using PowerShell.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="14518-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="14518-106">Before you begin</span></span>

* <span data-ttu-id="14518-107">Ellenőrizze, hogy rendelkezik-e hello legújabb verziójának hello Azure PowerShell-modulok (legalább 1.0-s verzió).</span><span class="sxs-lookup"><span data-stu-id="14518-107">Verify that you have hello latest version of hello Azure PowerShell modules (at least version 1.0).</span></span> <span data-ttu-id="14518-108">További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="14518-108">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="14518-109">Győződjön meg arról, hogy átolvasta hello [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="14518-109">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="14518-110">Tekintse át a megadott hello információkat [ExpressRoute-kapcsolatcsoportot áthelyezése klasszikus tooResource Manager](expressroute-move.md).</span><span class="sxs-lookup"><span data-stu-id="14518-110">Review hello information that is provided under [Moving an ExpressRoute circuit from classic tooResource Manager](expressroute-move.md).</span></span> <span data-ttu-id="14518-111">Győződjön meg arról, hogy megértette hello korlátai és korlátozásai.</span><span class="sxs-lookup"><span data-stu-id="14518-111">Make sure that you fully understand hello limits and limitations.</span></span>
* <span data-ttu-id="14518-112">Ellenőrizze, hogy teljesen működőképes hello klasszikus üzembe helyezési modellel hello körön.</span><span class="sxs-lookup"><span data-stu-id="14518-112">Verify that hello circuit is fully operational in hello classic deployment model.</span></span>
* <span data-ttu-id="14518-113">Győződjön meg arról, hogy rendelkezik-e hello Resource Manager üzembe helyezési modellel létrehozott erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="14518-113">Ensure that you have a resource group that was created in hello Resource Manager deployment model.</span></span>

## <a name="move-an-expressroute-circuit"></a><span data-ttu-id="14518-114">Helyezze át az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="14518-114">Move an ExpressRoute circuit</span></span>

### <a name="step-1-gather-circuit-details-from-hello-classic-deployment-model"></a><span data-ttu-id="14518-115">1. lépés: Gyűjtsön áramkör részletek hello klasszikus telepítési modell</span><span class="sxs-lookup"><span data-stu-id="14518-115">Step 1: Gather circuit details from hello classic deployment model</span></span>

<span data-ttu-id="14518-116">Jelentkezzen be a klasszikus Azure-környezetéhez toohello és hello szolgáltatás kulcs összegyűjtése.</span><span class="sxs-lookup"><span data-stu-id="14518-116">Sign in toohello Azure classic environment and gather hello service key.</span></span>

1. <span data-ttu-id="14518-117">Bejelentkezés tooyour Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="14518-117">Sign in tooyour Azure account.</span></span>

  ```powershell
  Add-AzureAccount
  ```

2. <span data-ttu-id="14518-118">Válassza ki a hello megfelelő Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="14518-118">Select hello appropriate Azure subscription.</span></span>

  ```powershell
  Select-AzureSubscription "<Enter Subscription Name here>"
  ```

3. <span data-ttu-id="14518-119">Hello PowerShell modul importálása az Azure és az ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="14518-119">Import hello PowerShell modules for Azure and ExpressRoute.</span></span>

  ```powershell
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
  ```

4. <span data-ttu-id="14518-120">Hello parancsmag tooget hello szolgáltatás kulcsok alatt az ExpressRoute-Kapcsolatcsoportok használni.</span><span class="sxs-lookup"><span data-stu-id="14518-120">Use hello cmdlet below tooget hello service keys for all of your ExpressRoute circuits.</span></span> <span data-ttu-id="14518-121">Hello kulcsok beolvasása, után másolja a hello **szolgáltatás kulcs** hello áramkör, amelyet az toomove toohello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="14518-121">After retrieving hello keys, copy hello **service key** of hello circuit that you want toomove toohello Resource Manager deployment model.</span></span>

  ```powershell
  Get-AzureDedicatedCircuit
  ```

### <a name="step-2-sign-in-and-create-a-resource-group"></a><span data-ttu-id="14518-122">Jelentkezzen be a 2. lépés:, És hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="14518-122">Step 2: Sign in and create a resource group</span></span>

<span data-ttu-id="14518-123">Jelentkezzen be toohello Resource Manager-környezetben, és hozzon létre egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="14518-123">Sign in toohello Resource Manager environment and create a new resource group.</span></span>

1. <span data-ttu-id="14518-124">Jelentkezzen be tooyour Azure Resource Manager-környezetben.</span><span class="sxs-lookup"><span data-stu-id="14518-124">Sign in tooyour Azure Resource Manager environment.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

2. <span data-ttu-id="14518-125">Válassza ki a hello megfelelő Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="14518-125">Select hello appropriate Azure subscription.</span></span>

  ```powershell
  Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription
  ```

3. <span data-ttu-id="14518-126">Ha még nem rendelkezik egy erőforráscsoport, módosítsa a hello részlet toocreate új erőforráscsoport alatt.</span><span class="sxs-lookup"><span data-stu-id="14518-126">Modify hello snippet below toocreate a new resource group if you don't already have a resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"
  ```

### <a name="step-3-move-hello-expressroute-circuit-toohello-resource-manager-deployment-model"></a><span data-ttu-id="14518-127">3. lépés: Helyezze át a hello ExpressRoute körön toohello Resource Manager telepítési modell</span><span class="sxs-lookup"><span data-stu-id="14518-127">Step 3: Move hello ExpressRoute circuit toohello Resource Manager deployment model</span></span>

<span data-ttu-id="14518-128">Ön most már készen áll a toomove az ExpressRoute-kapcsolatcsoportot hello klasszikus üzembe helyezési modell toohello Resource Manager üzembe helyezési modellben a rendszer.</span><span class="sxs-lookup"><span data-stu-id="14518-128">You are now ready toomove your ExpressRoute circuit from hello classic deployment model toohello Resource Manager deployment model.</span></span> <span data-ttu-id="14518-129">A folytatás előtt ellenőrizze a található hello információk [ExpressRoute-kapcsolatcsoportot áthelyezése hello klasszikus toohello Resource Manager üzembe helyezési modellben](expressroute-move.md).</span><span class="sxs-lookup"><span data-stu-id="14518-129">Before proceeding, review hello information provided in [Moving an ExpressRoute circuit from hello classic toohello Resource Manager deployment model](expressroute-move.md).</span></span>

<span data-ttu-id="14518-130">a kapcsolatcsoport toomove módosíthatja, és futtassa a következő kódrészletet hello:</span><span class="sxs-lookup"><span data-stu-id="14518-130">toomove your circuit, modify and run hello following snippet:</span></span>

```powershell
Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"
```

> [!NOTE]
> <span data-ttu-id="14518-131">Hello áthelyezés befejeződését követően hello új neve szerepel-e hello előző parancsmag lesz használt tooaddress hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="14518-131">After hello move has finished, hello new name that is listed in hello previous cmdlet will be used tooaddress hello resource.</span></span> <span data-ttu-id="14518-132">hello áramkör lényegében lehet átnevezni.</span><span class="sxs-lookup"><span data-stu-id="14518-132">hello circuit will essentially be renamed.</span></span>
> 

## <a name="modify-circuit-access"></a><span data-ttu-id="14518-133">Módosítsa a kapcsolatcsoport hozzáférés</span><span class="sxs-lookup"><span data-stu-id="14518-133">Modify circuit access</span></span>

### <a name="tooenable-expressroute-circuit-access-for-both-deployment-models"></a><span data-ttu-id="14518-134">ExpressRoute körön hozzáférés mindkét tooenable</span><span class="sxs-lookup"><span data-stu-id="14518-134">tooenable ExpressRoute circuit access for both deployment models</span></span>

<span data-ttu-id="14518-135">Helyezze át a klasszikus ExpressRoute körön toohello Resource Manager telepítési modell, engedélyezheti a hozzáférést tooboth üzembe helyezési modellek.</span><span class="sxs-lookup"><span data-stu-id="14518-135">After moving your classic ExpressRoute circuit toohello Resource Manager deployment model, you can enable access tooboth deployment models.</span></span> <span data-ttu-id="14518-136">Futtassa a következő parancsmagok tooenable hozzáférés tooboth üzembe helyezési modellel hello:</span><span class="sxs-lookup"><span data-stu-id="14518-136">Run hello following cmdlets tooenable access tooboth deployment models:</span></span>

1. <span data-ttu-id="14518-137">Ezzel a hello áramkör adatokat.</span><span class="sxs-lookup"><span data-stu-id="14518-137">Get hello circuit details.</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. <span data-ttu-id="14518-138">Állítsa be a "Klasszikus műveletek engedélyezése" tooTRUE.</span><span class="sxs-lookup"><span data-stu-id="14518-138">Set "Allow Classic Operations" tooTRUE.</span></span>

  ```powershell
  $ckt.AllowClassicOperations = $true
  ```

3. <span data-ttu-id="14518-139">Hello áramkör frissítése.</span><span class="sxs-lookup"><span data-stu-id="14518-139">Update hello circuit.</span></span> <span data-ttu-id="14518-140">Után ez a művelet sikeresen befejeződött, fogja tudni tooview hello áramkör hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="14518-140">After this operation has finished successfully, you will be able tooview hello circuit in hello classic deployment model.</span></span>

  ```powershell
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

4. <span data-ttu-id="14518-141">Futtassa a következő parancsmag tooget hello adatok az ExpressRoute-kapcsolatcsoportot hello hello.</span><span class="sxs-lookup"><span data-stu-id="14518-141">Run hello following cmdlet tooget hello details of hello ExpressRoute circuit.</span></span> <span data-ttu-id="14518-142">Képes toosee hello szolgáltatás kulcs felsorolt kell lennie.</span><span class="sxs-lookup"><span data-stu-id="14518-142">You must be able toosee hello service key listed.</span></span>

  ```powershell
  get-azurededicatedcircuit
  ```

5. <span data-ttu-id="14518-143">Hivatkozások ExpressRoute-kapcsolatcsoportot toohello parancsokkal hello klasszikus üzembe helyezési modell a hagyományos Vneteket, és hello erőforrás-kezelő parancsok az erőforrás-kezelő Vnetek kezelheti.</span><span class="sxs-lookup"><span data-stu-id="14518-143">You can now manage links toohello ExpressRoute circuit using hello classic deployment model commands for classic VNets, and hello Resource Manager commands for Resource Manager VNets.</span></span> <span data-ttu-id="14518-144">hello következő cikkek segítenek hivatkozások toohello ExpressRoute-kapcsolatcsoportot kezelheti:</span><span class="sxs-lookup"><span data-stu-id="14518-144">hello following articles help you manage links toohello ExpressRoute circuit:</span></span>

    * [<span data-ttu-id="14518-145">A virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot hello Resource Manager üzembe helyezési modellel hivatkozás</span><span class="sxs-lookup"><span data-stu-id="14518-145">Link your virtual network tooyour ExpressRoute circuit in hello Resource Manager deployment model</span></span>](expressroute-howto-linkvnet-arm.md)
    * [<span data-ttu-id="14518-146">A virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot hello klasszikus üzembe helyezési modellel hivatkozás</span><span class="sxs-lookup"><span data-stu-id="14518-146">Link your virtual network tooyour ExpressRoute circuit in hello classic deployment model</span></span>](expressroute-howto-linkvnet-classic.md)

### <a name="toodisable-expressroute-circuit-access-toohello-classic-deployment-model"></a><span data-ttu-id="14518-147">toodisable ExpressRoute körön hozzáférés toohello klasszikus telepítési modell</span><span class="sxs-lookup"><span data-stu-id="14518-147">toodisable ExpressRoute circuit access toohello classic deployment model</span></span>

<span data-ttu-id="14518-148">Futtassa a következő parancsmagok toodisable hozzáférés toohello klasszikus üzembe helyezési modellel hello.</span><span class="sxs-lookup"><span data-stu-id="14518-148">Run hello following cmdlets toodisable access toohello classic deployment model.</span></span>

1. <span data-ttu-id="14518-149">Hello ExpressRoute-kapcsolatcsoport részleteinek beolvasása.</span><span class="sxs-lookup"><span data-stu-id="14518-149">Get details of hello ExpressRoute circuit.</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. <span data-ttu-id="14518-150">Állítsa be a "Klasszikus műveletek engedélyezése" tooFALSE.</span><span class="sxs-lookup"><span data-stu-id="14518-150">Set "Allow Classic Operations" tooFALSE.</span></span>

  ```powershell
  $ckt.AllowClassicOperations = $false
  ```

3. <span data-ttu-id="14518-151">Hello áramkör frissítése.</span><span class="sxs-lookup"><span data-stu-id="14518-151">Update hello circuit.</span></span> <span data-ttu-id="14518-152">Után ez a művelet sikeresen befejeződött, nem fogja tudni tooview hello áramkör hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="14518-152">After this operation has finished successfully, you will not be able tooview hello circuit in hello classic deployment model.</span></span>

  ```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

## <a name="next-steps"></a><span data-ttu-id="14518-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="14518-153">Next steps</span></span>

* [<span data-ttu-id="14518-154">Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoport esetében routing</span><span class="sxs-lookup"><span data-stu-id="14518-154">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-arm.md)
* [<span data-ttu-id="14518-155">A virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot hivatkozás</span><span class="sxs-lookup"><span data-stu-id="14518-155">Link your virtual network tooyour ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)
