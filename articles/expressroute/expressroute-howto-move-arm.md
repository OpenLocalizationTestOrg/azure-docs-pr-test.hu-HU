---
title: "Klasszikus ExpressRoute-Kapcsolatcsoportok áthelyezéséhez az erőforrás-kezelő: PowerShell: Azure |} Microsoft Docs"
description: "Ez a lap ismerteti a klasszikus expressroute-kapcsolatcsoporthoz áthelyezése a Resource Manager üzembe helyezési modellel PowerShell használatával."
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
ms.openlocfilehash: c407e01e6d881cb8adcfe55faa246468669be883
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model-using-powershell"></a><span data-ttu-id="b78a0-103">A klasszikus ExpressRoute-Kapcsolatcsoportok áthelyezése a Resource Manager üzembe helyezési modellel PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="b78a0-103">Move ExpressRoute circuits from the classic to the Resource Manager deployment model using PowerShell</span></span>

<span data-ttu-id="b78a0-104">Az ExpressRoute-kapcsolatcsoportot használja a klasszikus és Resource Manager üzembe helyezési modellel, át kell helyeznie a kapcsolatcsoport a Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="b78a0-104">To use an ExpressRoute circuit for both the classic and Resource Manager deployment models, you must move the circuit to the Resource Manager deployment model.</span></span> <span data-ttu-id="b78a0-105">Az alábbi szakaszok segítséget nyújtanak a kapcsolatcsoport áthelyezése a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="b78a0-105">The following sections help you move your circuit by using PowerShell.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b78a0-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b78a0-106">Before you begin</span></span>

* <span data-ttu-id="b78a0-107">Ellenőrizze, hogy rendelkezik-e a legújabb verzióját az Azure PowerShell-modulok (legalább 1.0-s verzió).</span><span class="sxs-lookup"><span data-stu-id="b78a0-107">Verify that you have the latest version of the Azure PowerShell modules (at least version 1.0).</span></span> <span data-ttu-id="b78a0-108">További információt [az Azure PowerShell telepítésével és konfigurálásával](/powershell/azure/overview) foglalkozó témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="b78a0-108">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="b78a0-109">Győződjön meg arról, hogy tekintse át a [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="b78a0-109">Make sure that you have reviewed the [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="b78a0-110">Tekintse át az adatokat a megadott [klasszikus ExpressRoute-kapcsolatcsoportot áthelyezést az erőforrás-kezelő](expressroute-move.md).</span><span class="sxs-lookup"><span data-stu-id="b78a0-110">Review the information that is provided under [Moving an ExpressRoute circuit from classic to Resource Manager](expressroute-move.md).</span></span> <span data-ttu-id="b78a0-111">Győződjön meg arról, hogy megértette a korlátai és korlátozásai.</span><span class="sxs-lookup"><span data-stu-id="b78a0-111">Make sure that you fully understand the limits and limitations.</span></span>
* <span data-ttu-id="b78a0-112">Győződjön meg arról, hogy a kapcsolatcsoport teljesen működőképes, a klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="b78a0-112">Verify that the circuit is fully operational in the classic deployment model.</span></span>
* <span data-ttu-id="b78a0-113">Győződjön meg arról, hogy rendelkezik-e a Resource Manager üzembe helyezési modellel létrehozott erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="b78a0-113">Ensure that you have a resource group that was created in the Resource Manager deployment model.</span></span>

## <a name="move-an-expressroute-circuit"></a><span data-ttu-id="b78a0-114">Helyezze át az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="b78a0-114">Move an ExpressRoute circuit</span></span>

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a><span data-ttu-id="b78a0-115">1. lépés: A klasszikus üzembe helyezési modellel áramkör részletek összegyűjtése</span><span class="sxs-lookup"><span data-stu-id="b78a0-115">Step 1: Gather circuit details from the classic deployment model</span></span>

<span data-ttu-id="b78a0-116">Jelentkezzen be a klasszikus Azure-környezetéhez, és a szolgáltatási kulcs összegyűjtése.</span><span class="sxs-lookup"><span data-stu-id="b78a0-116">Sign in to the Azure classic environment and gather the service key.</span></span>

1. <span data-ttu-id="b78a0-117">Jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="b78a0-117">Sign in to your Azure account.</span></span>

  ```powershell
  Add-AzureAccount
  ```

2. <span data-ttu-id="b78a0-118">Válassza ki a megfelelő Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="b78a0-118">Select the appropriate Azure subscription.</span></span>

  ```powershell
  Select-AzureSubscription "<Enter Subscription Name here>"
  ```

3. <span data-ttu-id="b78a0-119">A PowerShell modul importálása az Azure és az ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b78a0-119">Import the PowerShell modules for Azure and ExpressRoute.</span></span>

  ```powershell
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
  Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
  ```

4. <span data-ttu-id="b78a0-120">Az alábbi parancsmag segítségével a szolgáltatás kulcsok lekérése az ExpressRoute-Kapcsolatcsoportok mindegyikét.</span><span class="sxs-lookup"><span data-stu-id="b78a0-120">Use the cmdlet below to get the service keys for all of your ExpressRoute circuits.</span></span> <span data-ttu-id="b78a0-121">A kulcsok beolvasása után másolja a **szolgáltatás kulcs** a kapcsolat a Resource Manager üzembe helyezési modellel áthelyezni kívánt.</span><span class="sxs-lookup"><span data-stu-id="b78a0-121">After retrieving the keys, copy the **service key** of the circuit that you want to move to the Resource Manager deployment model.</span></span>

  ```powershell
  Get-AzureDedicatedCircuit
  ```

### <a name="step-2-sign-in-and-create-a-resource-group"></a><span data-ttu-id="b78a0-122">Jelentkezzen be a 2. lépés:, És hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="b78a0-122">Step 2: Sign in and create a resource group</span></span>

<span data-ttu-id="b78a0-123">Jelentkezzen be a Resource Manager-környezetben, és hozzon létre egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b78a0-123">Sign in to the Resource Manager environment and create a new resource group.</span></span>

1. <span data-ttu-id="b78a0-124">Jelentkezzen be az Azure Resource Manager-környezetben.</span><span class="sxs-lookup"><span data-stu-id="b78a0-124">Sign in to your Azure Resource Manager environment.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

2. <span data-ttu-id="b78a0-125">Válassza ki a megfelelő Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="b78a0-125">Select the appropriate Azure subscription.</span></span>

  ```powershell
  Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription
  ```

3. <span data-ttu-id="b78a0-126">Hozzon létre egy új erőforráscsoportot, ha még nem rendelkezik egy erőforráscsoportot az alábbi részlet módosítása.</span><span class="sxs-lookup"><span data-stu-id="b78a0-126">Modify the snippet below to create a new resource group if you don't already have a resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"
  ```

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a><span data-ttu-id="b78a0-127">3. lépés: A Resource Manager üzembe helyezési modellel helyezze át az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="b78a0-127">Step 3: Move the ExpressRoute circuit to the Resource Manager deployment model</span></span>

<span data-ttu-id="b78a0-128">Most már készen áll a klasszikus telepítési modellből az ExpressRoute-kapcsolatcsoportot áthelyezése a Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="b78a0-128">You are now ready to move your ExpressRoute circuit from the classic deployment model to the Resource Manager deployment model.</span></span> <span data-ttu-id="b78a0-129">A folytatás előtt ellenőrizze a foglalt információk [klasszikus ExpressRoute-kapcsolatcsoportot áthelyezését a Resource Manager üzembe helyezési modellel](expressroute-move.md).</span><span class="sxs-lookup"><span data-stu-id="b78a0-129">Before proceeding, review the information provided in [Moving an ExpressRoute circuit from the classic to the Resource Manager deployment model](expressroute-move.md).</span></span>

<span data-ttu-id="b78a0-130">A kapcsolatcsoport áthelyezéséhez módosíthatja, és futtassa a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="b78a0-130">To move your circuit, modify and run the following snippet:</span></span>

```powershell
Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"
```

> [!NOTE]
> <span data-ttu-id="b78a0-131">Az áthelyezés után, az új név szerepel-e az előző parancsmag használandó cím az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b78a0-131">After the move has finished, the new name that is listed in the previous cmdlet will be used to address the resource.</span></span> <span data-ttu-id="b78a0-132">A kapcsolatcsoport lényegében lehet átnevezni.</span><span class="sxs-lookup"><span data-stu-id="b78a0-132">The circuit will essentially be renamed.</span></span>
> 

## <a name="modify-circuit-access"></a><span data-ttu-id="b78a0-133">Módosítsa a kapcsolatcsoport hozzáférés</span><span class="sxs-lookup"><span data-stu-id="b78a0-133">Modify circuit access</span></span>

### <a name="to-enable-expressroute-circuit-access-for-both-deployment-models"></a><span data-ttu-id="b78a0-134">Mindkét ExpressRoute-kapcsolatcsoport hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b78a0-134">To enable ExpressRoute circuit access for both deployment models</span></span>

<span data-ttu-id="b78a0-135">Az a Resource Manager üzembe helyezési modellel, helyezze át a klasszikus ExpressRoute-kapcsolatcsoportot, akkor lehetővé teszik, hogy két üzembe helyezési modell.</span><span class="sxs-lookup"><span data-stu-id="b78a0-135">After moving your classic ExpressRoute circuit to the Resource Manager deployment model, you can enable access to both deployment models.</span></span> <span data-ttu-id="b78a0-136">Két üzembe helyezési modell való hozzáférés engedélyezése a következő parancsmagok futtatásához:</span><span class="sxs-lookup"><span data-stu-id="b78a0-136">Run the following cmdlets to enable access to both deployment models:</span></span>

1. <span data-ttu-id="b78a0-137">Ezzel a kapcsolatcsoport adatokat.</span><span class="sxs-lookup"><span data-stu-id="b78a0-137">Get the circuit details.</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. <span data-ttu-id="b78a0-138">Állítsa be a "Klasszikus műveletek engedélyezése" TRUE.</span><span class="sxs-lookup"><span data-stu-id="b78a0-138">Set "Allow Classic Operations" to TRUE.</span></span>

  ```powershell
  $ckt.AllowClassicOperations = $true
  ```

3. <span data-ttu-id="b78a0-139">A kapcsolatcsoport frissítése.</span><span class="sxs-lookup"><span data-stu-id="b78a0-139">Update the circuit.</span></span> <span data-ttu-id="b78a0-140">Ez a művelet sikeresen befejeződött, miután lesz a kapcsolatcsoport megtekintése a klasszikus üzembe helyezési modellel.</span><span class="sxs-lookup"><span data-stu-id="b78a0-140">After this operation has finished successfully, you will be able to view the circuit in the classic deployment model.</span></span>

  ```powershell
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

4. <span data-ttu-id="b78a0-141">Futtassa a következő parancsmagot, hogy megkapja az ExpressRoute-kapcsolatcsoport részleteit.</span><span class="sxs-lookup"><span data-stu-id="b78a0-141">Run the following cmdlet to get the details of the ExpressRoute circuit.</span></span> <span data-ttu-id="b78a0-142">A felsorolt Szolgáltatáskulcs látni kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b78a0-142">You must be able to see the service key listed.</span></span>

  ```powershell
  get-azurededicatedcircuit
  ```

5. <span data-ttu-id="b78a0-143">Kezelheti a klasszikus üzembe helyezési modell parancsok használatát a hagyományos Vneteket, és a Resource Manager parancsok erőforrás-kezelő Vnetek ExpressRoute-kapcsolatcsoportot mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b78a0-143">You can now manage links to the ExpressRoute circuit using the classic deployment model commands for classic VNets, and the Resource Manager commands for Resource Manager VNets.</span></span> <span data-ttu-id="b78a0-144">A következő cikkek az ExpressRoute-kapcsolatcsoport mutató hivatkozások kezeléséhez nyújt segítséget:</span><span class="sxs-lookup"><span data-stu-id="b78a0-144">The following articles help you manage links to the ExpressRoute circuit:</span></span>

    * [<span data-ttu-id="b78a0-145">A virtuális hálózat csatolása a Resource Manager üzembe helyezési modellben az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="b78a0-145">Link your virtual network to your ExpressRoute circuit in the Resource Manager deployment model</span></span>](expressroute-howto-linkvnet-arm.md)
    * [<span data-ttu-id="b78a0-146">A virtuális hálózat csatolása a klasszikus üzembe helyezési modellben az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="b78a0-146">Link your virtual network to your ExpressRoute circuit in the classic deployment model</span></span>](expressroute-howto-linkvnet-classic.md)

### <a name="to-disable-expressroute-circuit-access-to-the-classic-deployment-model"></a><span data-ttu-id="b78a0-147">A klasszikus üzembe helyezési modellel ExpressRoute-kapcsolatcsoport elérésének letiltása</span><span class="sxs-lookup"><span data-stu-id="b78a0-147">To disable ExpressRoute circuit access to the classic deployment model</span></span>

<span data-ttu-id="b78a0-148">A klasszikus üzembe helyezési modellben való hozzáférés letiltása a következő parancsmagok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="b78a0-148">Run the following cmdlets to disable access to the classic deployment model.</span></span>

1. <span data-ttu-id="b78a0-149">Az ExpressRoute-kapcsolatcsoport részleteinek beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b78a0-149">Get details of the ExpressRoute circuit.</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"
  ```

2. <span data-ttu-id="b78a0-150">Állítsa be a "Klasszikus műveletek engedélyezése" FALSE.</span><span class="sxs-lookup"><span data-stu-id="b78a0-150">Set "Allow Classic Operations" to FALSE.</span></span>

  ```powershell
  $ckt.AllowClassicOperations = $false
  ```

3. <span data-ttu-id="b78a0-151">A kapcsolatcsoport frissítése.</span><span class="sxs-lookup"><span data-stu-id="b78a0-151">Update the circuit.</span></span> <span data-ttu-id="b78a0-152">Ez a művelet sikeresen befejeződött, után nem kezelhetők a kapcsolatcsoport megtekintheti a klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="b78a0-152">After this operation has finished successfully, you will not be able to view the circuit in the classic deployment model.</span></span>

  ```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

## <a name="next-steps"></a><span data-ttu-id="b78a0-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b78a0-153">Next steps</span></span>

* [<span data-ttu-id="b78a0-154">Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoport esetében routing</span><span class="sxs-lookup"><span data-stu-id="b78a0-154">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-arm.md)
* [<span data-ttu-id="b78a0-155">A virtuális hálózat csatolása az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="b78a0-155">Link your virtual network to your ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)