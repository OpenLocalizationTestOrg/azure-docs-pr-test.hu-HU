---
title: "Létrehozásához és módosításához ExpressRoute-kapcsolatcsoportot: PowerShell: klasszikus Azure portál |} Microsoft Docs"
description: "Ez a cikk végigvezeti a létrehozásához, és a kiépítés ExpressRoute-kapcsolatcsoportot. Ez a cikk is bemutatja, hogyan ellenőrizze az állapot, update vagy delete, és a kapcsolatcsoport kiosztásának megszüntetése."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0134d242-6459-4dec-a2f1-4657c3bc8b23
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 3b12bbb21ebf6a0160227c4a281c420cf192d6f7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell-classic"></a><span data-ttu-id="e140a-104">Létrehozása és módosítása a powershellel (klasszikus) ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="e140a-104">Create and modify an ExpressRoute circuit using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e140a-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e140a-105">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="e140a-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e140a-106">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="e140a-107">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e140a-107">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="e140a-108">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e140a-108">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="e140a-109">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="e140a-109">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="e140a-110">Ez a cikk végigvezeti az Azure ExpressRoute-kapcsolatcsoportot létrehozása a PowerShell-parancsmagok és a klasszikus telepítési modell használatával.</span><span class="sxs-lookup"><span data-stu-id="e140a-110">This article walks you through the steps to create an Azure ExpressRoute circuit by using PowerShell cmdlets and the classic deployment model.</span></span> <span data-ttu-id="e140a-111">Ez a cikk is bemutatja, hogyan ellenőrizze az állapot, frissítési vagy törlési és kiosztásának megszüntetése ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="e140a-111">This article also shows you how to check the status, update, or delete and deprovision an ExpressRoute circuit.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]


<span data-ttu-id="e140a-112">**Tudnivalók az Azure üzembehelyezési modellekről**</span><span class="sxs-lookup"><span data-stu-id="e140a-112">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a><span data-ttu-id="e140a-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="e140a-113">Before you begin</span></span>
### <a name="step-1-review-the-prerequisites-and-workflow-articles"></a><span data-ttu-id="e140a-114">1. lépés</span><span class="sxs-lookup"><span data-stu-id="e140a-114">Step 1.</span></span> <span data-ttu-id="e140a-115">Nézze át az Előfeltételek és a munkafolyamat cikkek</span><span class="sxs-lookup"><span data-stu-id="e140a-115">Review the prerequisites and workflow articles</span></span>
<span data-ttu-id="e140a-116">Győződjön meg arról, hogy tekintse át a [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="e140a-116">Make sure that you have reviewed the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>  

### <a name="step-2-install-the-latest-versions-of-the-azure-service-management-sm-powershell-modules"></a><span data-ttu-id="e140a-117">2. lépés</span><span class="sxs-lookup"><span data-stu-id="e140a-117">Step 2.</span></span> <span data-ttu-id="e140a-118">A legújabb verzióját az Azure Service Management (SM) PowerShell-modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="e140a-118">Install the latest versions of the Azure Service Management (SM) PowerShell modules</span></span>
<span data-ttu-id="e140a-119">Kövesse az utasításokat a [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/overview) kapcsolatos részletes útmutatás az Azure PowerShell-modulok használata a számítógép konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e140a-119">Follow the instructions in [Getting started with Azure PowerShell cmdlets](/powershell/azure/overview) for step-by-step guidance on how to configure your computer to use the Azure PowerShell modules.</span></span>

### <a name="step-3-log-in-to-your-azure-account-and-select-a-subscription"></a><span data-ttu-id="e140a-120">3. lépés</span><span class="sxs-lookup"><span data-stu-id="e140a-120">Step 3.</span></span> <span data-ttu-id="e140a-121">Jelentkezzen be az Azure-fiókjával, és válasszon egy előfizetést</span><span class="sxs-lookup"><span data-stu-id="e140a-121">Log in to your Azure account and select a subscription</span></span>
1. <span data-ttu-id="e140a-122">Nyissa meg emelt szintű jogosultságokkal a PowerShell konzolt, és csatlakozzon a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="e140a-122">Open your PowerShell console with elevated rights and connect to your account.</span></span> <span data-ttu-id="e140a-123">A következő példa segít a kapcsolódásban:</span><span class="sxs-lookup"><span data-stu-id="e140a-123">Use the following example to help you connect:</span></span>

        Login-AzureRmAccount

2. <span data-ttu-id="e140a-124">Keresse meg a fiókot az előfizetésekben.</span><span class="sxs-lookup"><span data-stu-id="e140a-124">Check the subscriptions for the account.</span></span>

        Get-AzureRmSubscription

3. <span data-ttu-id="e140a-125">Ha egynél több előfizetéssel rendelkezik, akkor válassza ki azt, amelyiket használni szeretné.</span><span class="sxs-lookup"><span data-stu-id="e140a-125">If you have more than one subscription, select the subscription that you want to use.</span></span>

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. <span data-ttu-id="e140a-126">Ezután az alábbi parancsmaggal adja hozzá az Azure-előfizetéshez a PowerShell a a klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="e140a-126">Next, use the following cmdlet to add your Azure subscription to PowerShell for the classic deployment model.</span></span>

        Add-AzureAccount

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="e140a-127">Hozzon létre, és helyezze üzembe az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="e140a-127">Create and provision an ExpressRoute circuit</span></span>
### <a name="step-1-import-the-powershell-modules-for-expressroute"></a><span data-ttu-id="e140a-128">1. lépés</span><span class="sxs-lookup"><span data-stu-id="e140a-128">Step 1.</span></span> <span data-ttu-id="e140a-129">A PowerShell-modulok importálása az ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="e140a-129">Import the PowerShell modules for ExpressRoute</span></span>
 <span data-ttu-id="e140a-130">Ha még nem tette meg, importálnia kell az Azure és az ExpressRoute modulok a PowerShell-munkamenetben megkezdéséhez az ExpressRoute-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="e140a-130">If you have not already done so, you must import the Azure and ExpressRoute modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="e140a-131">A modulok importálása a helyre, amely korábban telepítette őket a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e140a-131">You import the modules from the location that they were installed to on your local computer.</span></span> <span data-ttu-id="e140a-132">A modulok telepítéséhez is használt módszernek a hely eltérhetnek az alábbi példában látható.</span><span class="sxs-lookup"><span data-stu-id="e140a-132">Depending on the method you used to install the modules, the location may be different than the following example shows.</span></span> <span data-ttu-id="e140a-133">Szükség esetén módosítsa a példa.</span><span class="sxs-lookup"><span data-stu-id="e140a-133">Modify the example if necessary.</span></span>  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="step-2-get-the-list-of-supported-providers-locations-and-bandwidths"></a><span data-ttu-id="e140a-134">2. lépés</span><span class="sxs-lookup"><span data-stu-id="e140a-134">Step 2.</span></span> <span data-ttu-id="e140a-135">A támogatott szolgáltatók, a helyek és a sávszélesség listájának lekérdezése</span><span class="sxs-lookup"><span data-stu-id="e140a-135">Get the list of supported providers, locations, and bandwidths</span></span>
<span data-ttu-id="e140a-136">ExpressRoute-kapcsolatcsoportot létrehozni, meg kell támogatott kapcsolat szolgáltatókat, a helyek és a sávszélesség-beállítások listája.</span><span class="sxs-lookup"><span data-stu-id="e140a-136">Before you create an ExpressRoute circuit, you need the list of supported connectivity providers, locations, and bandwidth options.</span></span>

<span data-ttu-id="e140a-137">A PowerShell-parancsmag `Get-AzureDedicatedCircuitServiceProvider` adja vissza ezt az információt fogja használni a későbbi lépésekben:</span><span class="sxs-lookup"><span data-stu-id="e140a-137">The PowerShell cmdlet `Get-AzureDedicatedCircuitServiceProvider` returns this information, which you’ll use in later steps:</span></span>

    Get-AzureDedicatedCircuitServiceProvider

<span data-ttu-id="e140a-138">Ellenőrizze, hogy ha a kapcsolat szolgáltatójánál nem szerepel-e.</span><span class="sxs-lookup"><span data-stu-id="e140a-138">Check to see if your connectivity provider is listed there.</span></span> <span data-ttu-id="e140a-139">Jegyezze fel a következő adatokat, mert azt később szüksége expressroute-kapcsolatcsoporthoz létrehozásakor:</span><span class="sxs-lookup"><span data-stu-id="e140a-139">Make a note of the following information because you'll need it later when you create a circuit:</span></span>

* <span data-ttu-id="e140a-140">Név</span><span class="sxs-lookup"><span data-stu-id="e140a-140">Name</span></span>
* <span data-ttu-id="e140a-141">PeeringLocations</span><span class="sxs-lookup"><span data-stu-id="e140a-141">PeeringLocations</span></span>
* <span data-ttu-id="e140a-142">BandwidthsOffered</span><span class="sxs-lookup"><span data-stu-id="e140a-142">BandwidthsOffered</span></span>

<span data-ttu-id="e140a-143">Most már készen áll az ExpressRoute-kapcsolatcsoportot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e140a-143">You're now ready to create an ExpressRoute circuit.</span></span>         

### <a name="step-3-create-an-expressroute-circuit"></a><span data-ttu-id="e140a-144">3. lépés</span><span class="sxs-lookup"><span data-stu-id="e140a-144">Step 3.</span></span> <span data-ttu-id="e140a-145">ExpressRoute-kapcsolatcsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="e140a-145">Create an ExpressRoute circuit</span></span>
<span data-ttu-id="e140a-146">A következő példa bemutatja, hogyan szilícium Valley egy 200 MB/s Equinix keresztül ExpressRoute-kapcsolatcsoportot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e140a-146">The following example shows how to create a 200-Mbps ExpressRoute circuit through Equinix in Silicon Valley.</span></span> <span data-ttu-id="e140a-147">Különböző szolgáltatók és más beállítások használata, amikor a kérést helyettesítse be ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="e140a-147">If you're using a different provider and different settings, substitute that information when you make your request.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e140a-148">Az ExpressRoute-kapcsolatcsoportot abban a pillanatban a szolgáltatási kulcs kiadott lesz terhelve.</span><span class="sxs-lookup"><span data-stu-id="e140a-148">Your ExpressRoute circuit will be billed from the moment a service key is issued.</span></span> <span data-ttu-id="e140a-149">Győződjön meg arról, hogy a művelet végrehajtása, ha a kapcsolat szolgáltatójánál kiépíteni a kapcsolat készen áll-e.</span><span class="sxs-lookup"><span data-stu-id="e140a-149">Ensure that you perform this operation when the connectivity provider is ready to provision the circuit.</span></span>
> 
> 

<span data-ttu-id="e140a-150">A következő egy példa egy kérelem egy új szolgáltatás kulcs:</span><span class="sxs-lookup"><span data-stu-id="e140a-150">The following is an example request for a new service key:</span></span>

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

<span data-ttu-id="e140a-151">Vagy, ha azt szeretné, a prémium szintű bővítmény ExpressRoute-kapcsolatcsoportot létrehozni, használja a következő példát.</span><span class="sxs-lookup"><span data-stu-id="e140a-151">Or, if you want to create an ExpressRoute circuit with the premium add-on, use the following example.</span></span> <span data-ttu-id="e140a-152">Tekintse meg a [ExpressRoute – gyakori kérdések](expressroute-faqs.md) a prémium szintű bővítmény kapcsolatos további részletekért.</span><span class="sxs-lookup"><span data-stu-id="e140a-152">Refer to the [ExpressRoute FAQ](expressroute-faqs.md) for more details about the premium add-on.</span></span>

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


<span data-ttu-id="e140a-153">A válasz a szolgáltatás kulcsot fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="e140a-153">The response will contain the service key.</span></span> <span data-ttu-id="e140a-154">Részletes leírását, a paraméterek kaphat a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e140a-154">You can get detailed descriptions of all the parameters by running the following:</span></span>

    get-help new-azurededicatedcircuit -detailed

### <a name="step-4-list-all-the-expressroute-circuits"></a><span data-ttu-id="e140a-155">4. lépés</span><span class="sxs-lookup"><span data-stu-id="e140a-155">Step 4.</span></span> <span data-ttu-id="e140a-156">Az ExpressRoute-Kapcsolatcsoportok felsorolása</span><span class="sxs-lookup"><span data-stu-id="e140a-156">List all the ExpressRoute circuits</span></span>
<span data-ttu-id="e140a-157">Futtathatja a `Get-AzureDedicatedCircuit` parancs használatával beszerezheti az összes létrehozott ExpressRoute-Kapcsolatcsoportok listáját:</span><span class="sxs-lookup"><span data-stu-id="e140a-157">You can run the `Get-AzureDedicatedCircuit` command to get a list of all the ExpressRoute circuits that you created:</span></span>

    Get-AzureDedicatedCircuit

<span data-ttu-id="e140a-158">A válasz az alábbi példához hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="e140a-158">The response will be something similar to the following example:</span></span>

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="e140a-159">Ezt az információt bármikor használatával kérheti le a `Get-AzureDedicatedCircuit` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e140a-159">You can retrieve this information at any time by using the `Get-AzureDedicatedCircuit` cmdlet.</span></span> <span data-ttu-id="e140a-160">A következő hívással paraméterek nélkül a Kapcsolatcsoportok sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="e140a-160">Making the call without any parameters lists all the circuits.</span></span> <span data-ttu-id="e140a-161">A szolgáltatás kulcs megjelenik a *ServiceKey* mező.</span><span class="sxs-lookup"><span data-stu-id="e140a-161">Your service key will be listed in the *ServiceKey* field.</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="e140a-162">Részletes leírását, a paraméterek kaphat a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e140a-162">You can get detailed descriptions of all the parameters by running the following:</span></span>

    get-help get-azurededicatedcircuit -detailed

### <a name="step-5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a><span data-ttu-id="e140a-163">5. lépés</span><span class="sxs-lookup"><span data-stu-id="e140a-163">Step 5.</span></span> <span data-ttu-id="e140a-164">A szolgáltatás kulcs küldése a kapcsolat szolgáltatójánál történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="e140a-164">Send the service key to your connectivity provider for provisioning</span></span>
<span data-ttu-id="e140a-165">*ServiceProviderProvisioningState* információt nyújt a jelenlegi állapotában a szolgáltatói oldalon kiépítés.</span><span class="sxs-lookup"><span data-stu-id="e140a-165">*ServiceProviderProvisioningState* provides information on the current state of provisioning on the service-provider side.</span></span> <span data-ttu-id="e140a-166">*Állapot* állapotát biztosít a Microsoft oldalon.</span><span class="sxs-lookup"><span data-stu-id="e140a-166">*Status* provides the state on the Microsoft side.</span></span> <span data-ttu-id="e140a-167">Kiépítés állapotok áramkör kapcsolatos további információkért tekintse meg a [munkafolyamatok](expressroute-workflows.md#expressroute-circuit-provisioning-states) cikk.</span><span class="sxs-lookup"><span data-stu-id="e140a-167">For more information about circuit provisioning states, see the [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="e140a-168">Amikor létrehoz egy új ExpressRoute-kapcsolatcsoportot, a kapcsolatcsoport lesz a következő állapotot okozta:</span><span class="sxs-lookup"><span data-stu-id="e140a-168">When you create a new ExpressRoute circuit, the circuit will be in the following state:</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="e140a-169">A kapcsolatcsoport halad át a következő állapotot, ha a kapcsolat szolgáltatójánál folyamatban van, lehetővé téve az Ön:</span><span class="sxs-lookup"><span data-stu-id="e140a-169">The circuit will go to the following state when the connectivity provider is in the process of enabling it for you:</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

<span data-ttu-id="e140a-170">Egy ExpressRoute-kapcsolatcsoportot is használni tudja a következő állapotban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="e140a-170">An ExpressRoute circuit must be in the following state for you to be able to use it:</span></span>

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="step-6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a><span data-ttu-id="e140a-171">6. lépés</span><span class="sxs-lookup"><span data-stu-id="e140a-171">Step 6.</span></span> <span data-ttu-id="e140a-172">Ellenőrizze rendszeresen a kapcsolatcsoport kulcs állapotát és az állapot</span><span class="sxs-lookup"><span data-stu-id="e140a-172">Periodically check the status and the state of the circuit key</span></span>
<span data-ttu-id="e140a-173">Ez lehetővé teszi, hogy amikor a szolgáltató a kapcsolatcsoport engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="e140a-173">This lets you know when your provider has enabled your circuit.</span></span> <span data-ttu-id="e140a-174">A kapcsolat konfigurálása után *ServiceProviderProvisioningState* állapottal jelenik meg *kiépítve* a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="e140a-174">After the circuit has been configured, *ServiceProviderProvisioningState* will display as *Provisioned* as shown in the following example:</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="step-7-create-your-routing-configuration"></a><span data-ttu-id="e140a-175">7. lépés</span><span class="sxs-lookup"><span data-stu-id="e140a-175">Step 7.</span></span> <span data-ttu-id="e140a-176">Az útválasztó-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="e140a-176">Create your routing configuration</span></span>
<span data-ttu-id="e140a-177">Tekintse meg a [ExpressRoute-áramkör útválasztási konfigurációja (létrehozásához és módosításához a kapcsolatcsoport esetében)](expressroute-howto-routing-classic.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="e140a-177">Refer to the [ExpressRoute circuit routing configuration (create and modify circuit peerings)](expressroute-howto-routing-classic.md) article for step-by-step instructions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e140a-178">Ezek az utasítások csak a szolgáltatók által biztosított réteg 2 internetkapcsolati szolgáltatás használatával létrehozott kapcsolatok vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="e140a-178">These instructions only apply to circuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="e140a-179">A szolgáltató által kezelt használata réteg (általában az IP VPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.</span><span class="sxs-lookup"><span data-stu-id="e140a-179">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="step-8-link-a-virtual-network-to-an-expressroute-circuit"></a><span data-ttu-id="e140a-180">8. lépés.</span><span class="sxs-lookup"><span data-stu-id="e140a-180">Step 8.</span></span> <span data-ttu-id="e140a-181">Virtuális hálózat összekapcsolása egy ExpressRoute-kapcsolatcsoporttal</span><span class="sxs-lookup"><span data-stu-id="e140a-181">Link a virtual network to an ExpressRoute circuit</span></span>
<span data-ttu-id="e140a-182">A következő csatolja az ExpressRoute-kapcsolatcsoportot egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="e140a-182">Next, link a virtual network to your ExpressRoute circuit.</span></span> <span data-ttu-id="e140a-183">Tekintse meg [virtuális hálózatokhoz való csatolás ExpressRoute Kapcsolatcsoportok](expressroute-howto-linkvnet-classic.md) részletes útmutatásait.</span><span class="sxs-lookup"><span data-stu-id="e140a-183">Refer to [Linking ExpressRoute circuits to virtual networks](expressroute-howto-linkvnet-classic.md) for step-by-step instructions.</span></span> <span data-ttu-id="e140a-184">Ha egy virtuális hálózat létrehozása a klasszikus üzembe helyezési modellel az ExpressRoute című kell [hozzon létre egy virtuális hálózatot az ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span><span class="sxs-lookup"><span data-stu-id="e140a-184">If you need to create a virtual network using the classic deployment model for ExpressRoute, see [Create a virtual network for ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span></span>

## <a name="getting-the-status-of-an-expressroute-circuit"></a><span data-ttu-id="e140a-185">Egy ExpressRoute-kapcsolatcsoport állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="e140a-185">Getting the status of an ExpressRoute circuit</span></span>
<span data-ttu-id="e140a-186">Ezt az információt bármikor használatával kérheti le a `Get-AzureCircuit` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e140a-186">You can retrieve this information at any time by using the `Get-AzureCircuit` cmdlet.</span></span> <span data-ttu-id="e140a-187">A következő hívással paraméterek nélkül a Kapcsolatcsoportok sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="e140a-187">Making the call without any parameters lists all the circuits.</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="e140a-188">Egy adott ExpressRoute-kapcsolatcsoportot tájékoztatást kaphat úgy, hogy a szolgáltatás kulcs paraméterként a hívást.</span><span class="sxs-lookup"><span data-stu-id="e140a-188">You can get information on a specific ExpressRoute circuit by passing the service key as a parameter to the call.</span></span>

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


<span data-ttu-id="e140a-189">Futtassa az alábbi példa is ki lehet részletes leírását, a Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="e140a-189">You can get detailed descriptions of all the parameters by running the following example:</span></span>

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a><span data-ttu-id="e140a-190">Egy ExpressRoute-kapcsolatcsoportot módosítása</span><span class="sxs-lookup"><span data-stu-id="e140a-190">Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="e140a-191">ExpressRoute-kapcsolatcsoportot egyes tulajdonságait módosíthatja kapcsolat befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="e140a-191">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="e140a-192">Állásidő nélkül a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="e140a-192">You can do the following with no downtime:</span></span>

* <span data-ttu-id="e140a-193">Engedélyezi vagy letiltja az ExpressRoute-kapcsolatcsoportot prémium ExpressRoute bővítményt.</span><span class="sxs-lookup"><span data-stu-id="e140a-193">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="e140a-194">Növelje a ExpressRoute-kapcsolatcsoportot sávszélességét, feltéve, hogy kapacitás érhető el a port.</span><span class="sxs-lookup"><span data-stu-id="e140a-194">Increase the bandwidth of your ExpressRoute circuit provided there is capacity available on the port.</span></span> <span data-ttu-id="e140a-195">Vegye figyelembe, hogy a alacsonyabb verziójúra változtatása a sávszélességet a kapcsolat nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="e140a-195">Note that downgrading the bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="e140a-196">Díjköteles adatforgalom korlátlan adatokhoz a mérési terv módosítása</span><span class="sxs-lookup"><span data-stu-id="e140a-196">Change the metering plan from Metered Data to Unlimited Data.</span></span> <span data-ttu-id="e140a-197">Vegye figyelembe, hogy a mérési terv módosítása az korlátlan adatforgalom díjköteles adatok nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="e140a-197">Note that changing the metering plan from Unlimited Data to Metered Data is not supported.</span></span>
* <span data-ttu-id="e140a-198">Engedélyezheti és letilthatja *klasszikus műveletek engedélyezése*.</span><span class="sxs-lookup"><span data-stu-id="e140a-198">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="e140a-199">Tekintse meg a [ExpressRoute – gyakori kérdések](expressroute-faqs.md) korlátai és korlátozásai olvashat.</span><span class="sxs-lookup"><span data-stu-id="e140a-199">Refer to the [ExpressRoute FAQ](expressroute-faqs.md) for more information on limits and limitations.</span></span>

### <a name="to-enable-the-expressroute-premium-add-on"></a><span data-ttu-id="e140a-200">A prémium szintű ExpressRoute-bővítmény engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e140a-200">To enable the ExpressRoute premium add-on</span></span>
<span data-ttu-id="e140a-201">A prémium szintű ExpressRoute-bővítmény a következő PowerShell-parancsmag segítségével engedélyezheti a meglévő kapcsolat:</span><span class="sxs-lookup"><span data-stu-id="e140a-201">You can enable the ExpressRoute premium add-on for your existing circuit by using the following PowerShell cmdlet:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

<span data-ttu-id="e140a-202">A kapcsolatcsoport most lesz engedélyezett ExpressRoute prémium bővítmény funkciók.</span><span class="sxs-lookup"><span data-stu-id="e140a-202">Your circuit will now have the ExpressRoute premium add-on features enabled.</span></span> <span data-ttu-id="e140a-203">Vegye figyelembe, hogy azt megkezdődik, amint sikeresen futtatta a parancsot számlázást, a prémium szintű bővítmény képességhez.</span><span class="sxs-lookup"><span data-stu-id="e140a-203">Note that we will start billing you for the premium add-on capability as soon as the command has successfully run.</span></span>

### <a name="to-disable-the-expressroute-premium-add-on"></a><span data-ttu-id="e140a-204">A prémium szintű ExpressRoute-bővítmény letiltása</span><span class="sxs-lookup"><span data-stu-id="e140a-204">To disable the ExpressRoute premium add-on</span></span>
> [!IMPORTANT]
> <span data-ttu-id="e140a-205">A művelet sikertelen lesz, amely nagyobb, mint mi a szabványos kör megengedett erőforrások használata.</span><span class="sxs-lookup"><span data-stu-id="e140a-205">This operation can fail if you're using resources that are greater than what is permitted for the standard circuit.</span></span>
> 
> 

#### <a name="considerations"></a><span data-ttu-id="e140a-206">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="e140a-206">Considerations</span></span>

* <span data-ttu-id="e140a-207">Meg kell győződnie arról, hogy a kapcsolatcsoport kapcsolódó virtuális hálózatok száma kisebb, mint 10 szabvány prémiumról visszaminősítését előtt.</span><span class="sxs-lookup"><span data-stu-id="e140a-207">You must ensure that the number of virtual networks linked to the circuit is less than 10 before you downgrade from premium to standard.</span></span> <span data-ttu-id="e140a-208">Ha nem így tesz, a frissítési kérelem sikertelen lesz, és lesz számlázva a prémium szintű sebességet.</span><span class="sxs-lookup"><span data-stu-id="e140a-208">If you don't do this, your update request will fail, and you'll be billed the premium rates.</span></span>
* <span data-ttu-id="e140a-209">Minden virtuális hálózat más geopolitikai régiókban kell választható.</span><span class="sxs-lookup"><span data-stu-id="e140a-209">You must unlink all virtual networks in other geopolitical regions.</span></span> <span data-ttu-id="e140a-210">Ha nem így tesz, a frissítési kérelem sikertelen lesz, és lesz számlázva a prémium szintű sebességet.</span><span class="sxs-lookup"><span data-stu-id="e140a-210">If you don't do this, your update request will fail, and you'll be billed the premium rates.</span></span>
* <span data-ttu-id="e140a-211">Az útvonaltábla a magánhálózati társviszony-létesítés kisebb, mint 4000 útvonalait kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e140a-211">Your route table must be less than 4,000 routes for private peering.</span></span> <span data-ttu-id="e140a-212">Ha az útvonal tábla mérete nagyobb, mint 4000 útvonalakat, a BGP-munkamenetet eldobja, és nem fog újra engedélyezve, amíg a hirdetett számához nem 4000 éri el.</span><span class="sxs-lookup"><span data-stu-id="e140a-212">If your route table size is greater than 4,000 routes, the BGP session will drop and won't be reenabled until the number of advertised prefixes goes below 4,000.</span></span>

#### <a name="disable-the-premium-add-on"></a><span data-ttu-id="e140a-213">A prémium szintű bővítmény letiltása</span><span class="sxs-lookup"><span data-stu-id="e140a-213">Disable the premium add-on</span></span>
<span data-ttu-id="e140a-214">A prémium szintű ExpressRoute-bővítmény a következő PowerShell-parancsmag használatával kikapcsolhatja a meglévő expressroute:</span><span class="sxs-lookup"><span data-stu-id="e140a-214">You can disable the ExpressRoute premium add-on for your existing circuit by using the following PowerShell cmdlet:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a><span data-ttu-id="e140a-215">Az ExpressRoute-kapcsolatcsoport sávszélessége frissítése</span><span class="sxs-lookup"><span data-stu-id="e140a-215">To update the ExpressRoute circuit bandwidth</span></span>
<span data-ttu-id="e140a-216">Ellenőrizze a [ExpressRoute – gyakori kérdések](expressroute-faqs.md) támogatott sávszélesség-beállítások a szolgáltatóhoz.</span><span class="sxs-lookup"><span data-stu-id="e140a-216">Check the [ExpressRoute FAQ](expressroute-faqs.md) for supported bandwidth options for your provider.</span></span> <span data-ttu-id="e140a-217">Bármely mérete nagyobb, mint a meglévő expressroute mérete, amíg a fizikai port (amely a kapcsolatcsoport létre van hozva) lehetővé teszi, hogy ki tudja választani.</span><span class="sxs-lookup"><span data-stu-id="e140a-217">You can pick any size that is greater than the size of your existing circuit as long as the physical port (on which your circuit is created) allows.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e140a-218">Előfordulhat, hogy újra létrehozni az ExpressRoute-kapcsolatcsoport, ha nincs elég kapacitás a meglévő porton.</span><span class="sxs-lookup"><span data-stu-id="e140a-218">You may have to recreate the ExpressRoute circuit if there is inadequate capacity on the existing port.</span></span> <span data-ttu-id="e140a-219">A kapcsolat nem frissíthető, ha nincsenek további kapacitást érhető el az adott helyhez.</span><span class="sxs-lookup"><span data-stu-id="e140a-219">You cannot upgrade the circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="e140a-220">Nem csökkenti a sávszélesség az ExpressRoute-kapcsolatcsoportot megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="e140a-220">You cannot reduce the bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="e140a-221">Alacsonyabb verziójúra változtatása a sávszélesség szükséges, hogy az ExpressRoute-kapcsolatcsoport kiosztásának megszüntetése és majd építenie az új ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="e140a-221">Downgrading bandwidth requires you to deprovision the ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 
> 

#### <a name="resize-a-circuit"></a><span data-ttu-id="e140a-222">Expressroute-kapcsolatcsoporthoz átméretezése</span><span class="sxs-lookup"><span data-stu-id="e140a-222">Resize a circuit</span></span>

<span data-ttu-id="e140a-223">Miután eldöntötte, hogy milyen méretű van szüksége, a következő paranccsal méretezze át a kapcsolatcsoport:</span><span class="sxs-lookup"><span data-stu-id="e140a-223">After you decide what size you need, you can use the following command to resize your circuit:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="e140a-224">A kapcsolatcsoport fog rendelkezik lett méretű a Microsoft oldalon.</span><span class="sxs-lookup"><span data-stu-id="e140a-224">Your circuit will have been sized up on the Microsoft side.</span></span> <span data-ttu-id="e140a-225">A kapcsolat szolgáltatójánál konfigurációit, a módosítás megfelelő a kiszolgálóoldali frissítéséhez forduljon.</span><span class="sxs-lookup"><span data-stu-id="e140a-225">You must contact your connectivity provider to update configurations on their side to match this change.</span></span> <span data-ttu-id="e140a-226">Vegye figyelembe, hogy azt megkezdődik az ettől a frissített sávszélesség beállítás számlázási meg.</span><span class="sxs-lookup"><span data-stu-id="e140a-226">Note that we will start billing you for the updated bandwidth option from this point on.</span></span>

<span data-ttu-id="e140a-227">Ha megjelenik a következő hiba, amikor a kapcsolatcsoport sávszélessége növelése, az azt jelenti nincs nincs elegendő sávszélesség a fizikai porton, a meglévő expressroute létrehozási helyének bal.</span><span class="sxs-lookup"><span data-stu-id="e140a-227">If you see the following error when increasing the circuit bandwidth, it means there is no sufficient bandwidth left on the physical port where your existing circuit is created.</span></span> <span data-ttu-id="e140a-228">Hogy a kapcsolatcsoport törlése, és hozzon létre egy új kapcsolat van szüksége a mérete.</span><span class="sxs-lookup"><span data-stu-id="e140a-228">You have to delete this circuit and create a new circuit of the size you need.</span></span> 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="e140a-229">Megszüntetés és az ExpressRoute-kapcsolatcsoport törlése</span><span class="sxs-lookup"><span data-stu-id="e140a-229">Deprovisioning and deleting an ExpressRoute circuit</span></span>

### <a name="considerations"></a><span data-ttu-id="e140a-230">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="e140a-230">Considerations</span></span>

* <span data-ttu-id="e140a-231">Az ExpressRoute-kapcsolatcsoport esetében ez a művelet sikeres legyen az összes virtuális hálózatot kell választható.</span><span class="sxs-lookup"><span data-stu-id="e140a-231">You must unlink all virtual networks from the ExpressRoute circuit for this operation to succeed.</span></span> <span data-ttu-id="e140a-232">Ellenőrizze, hogy van-e bármely virtuális hálózatot, amely a kapcsolatcsoport van csatolva, ha ez a művelet sikertelen.</span><span class="sxs-lookup"><span data-stu-id="e140a-232">Check to see if you have any virtual networks that are linked to the circuit if this operation fails.</span></span>
* <span data-ttu-id="e140a-233">Ha az ExpressRoute-kapcsolatcsoport szolgáltatás szolgáltató üzembe helyezési állapota **kiépítési** vagy **kiépítve** kiosztásának megszüntetése a kapcsolatcsoport az oldalon, hogy a szolgáltató együttműködve.</span><span class="sxs-lookup"><span data-stu-id="e140a-233">If the ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider to deprovision the circuit on their side.</span></span> <span data-ttu-id="e140a-234">Folytatjuk erőforrásokat és kiszámlázni Önnek, amíg a szolgáltató befejeződött, a kapcsolat megszüntetés, és értesítést küld nekünk.</span><span class="sxs-lookup"><span data-stu-id="e140a-234">We will continue to reserve resources and bill you until the service provider completes deprovisioning the circuit and notifies us.</span></span>
* <span data-ttu-id="e140a-235">Ha a szolgáltató rendelkezik platformelőfizetés a kapcsolatcsoport (üzembe helyezési állapota szolgáltató értéke **nincs kiépítve**) a kapcsolatcsoport törölhet.</span><span class="sxs-lookup"><span data-stu-id="e140a-235">If the service provider has deprovisioned the circuit (the service provider provisioning state is set to **Not provisioned**) you can then delete the circuit.</span></span> <span data-ttu-id="e140a-236">Ez a kapcsolat számlázási leáll.</span><span class="sxs-lookup"><span data-stu-id="e140a-236">This will stop billing for the circuit.</span></span>

#### <a name="delete-a-circuit"></a><span data-ttu-id="e140a-237">A kapcsolat törlése</span><span class="sxs-lookup"><span data-stu-id="e140a-237">Delete a circuit</span></span>

<span data-ttu-id="e140a-238">Az ExpressRoute-kapcsolatcsoport törlése a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e140a-238">You can delete your ExpressRoute circuit by running the following command:</span></span>

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a><span data-ttu-id="e140a-239">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e140a-239">Next steps</span></span>
<span data-ttu-id="e140a-240">Miután létrehozta a kapcsolatcsoport, győződjön meg arról, hogy akkor tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e140a-240">After you create your circuit, make sure that you do the following:</span></span>

* [<span data-ttu-id="e140a-241">Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoport esetében routing</span><span class="sxs-lookup"><span data-stu-id="e140a-241">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-classic.md)
* [<span data-ttu-id="e140a-242">A virtuális hálózat csatolása az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="e140a-242">Link your virtual network to your ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-classic.md)

