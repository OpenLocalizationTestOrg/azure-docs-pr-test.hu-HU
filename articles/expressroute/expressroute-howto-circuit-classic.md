---
title: "Létrehozásához és módosításához ExpressRoute-kapcsolatcsoportot: PowerShell: klasszikus Azure portál |} Microsoft Docs"
description: "Ez a cikk végigvezeti hello létrehozásához és a kiépítés ExpressRoute-kapcsolatcsoportot. Ez a cikk is bemutatja, hogyan toocheck hello állapotát, frissítéséhez vagy törlése és a kapcsolatcsoport kiosztásának megszüntetése."
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
ms.openlocfilehash: 9897c88776a2153ba22aa9ff328becb9f12b660b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell-classic"></a><span data-ttu-id="0feab-104">Létrehozása és módosítása a powershellel (klasszikus) ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="0feab-104">Create and modify an ExpressRoute circuit using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0feab-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0feab-105">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="0feab-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0feab-106">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="0feab-107">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0feab-107">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="0feab-108">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0feab-108">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="0feab-109">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="0feab-109">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="0feab-110">Ez a cikk bemutatja, hogyan hello lépéseket toocreate Azure ExpressRoute-kapcsolatcsoportot PowerShell-parancsmagok és hello klasszikus telepítési modell használatával.</span><span class="sxs-lookup"><span data-stu-id="0feab-110">This article walks you through hello steps toocreate an Azure ExpressRoute circuit by using PowerShell cmdlets and hello classic deployment model.</span></span> <span data-ttu-id="0feab-111">Ez a cikk is bemutatja, hogyan toocheck hello állapota, frissítéséhez vagy törlése és kiosztásának megszüntetése ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="0feab-111">This article also shows you how toocheck hello status, update, or delete and deprovision an ExpressRoute circuit.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]


<span data-ttu-id="0feab-112">**Tudnivalók az Azure üzembe helyezési modelljeiről**</span><span class="sxs-lookup"><span data-stu-id="0feab-112">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a><span data-ttu-id="0feab-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="0feab-113">Before you begin</span></span>
### <a name="step-1-review-hello-prerequisites-and-workflow-articles"></a><span data-ttu-id="0feab-114">1. lépés</span><span class="sxs-lookup"><span data-stu-id="0feab-114">Step 1.</span></span> <span data-ttu-id="0feab-115">Tekintse át a hello Előfeltételek és a munkafolyamat cikkek</span><span class="sxs-lookup"><span data-stu-id="0feab-115">Review hello prerequisites and workflow articles</span></span>
<span data-ttu-id="0feab-116">Győződjön meg arról, hogy átolvasta hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="0feab-116">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>  

### <a name="step-2-install-hello-latest-versions-of-hello-azure-service-management-sm-powershell-modules"></a><span data-ttu-id="0feab-117">2. lépés</span><span class="sxs-lookup"><span data-stu-id="0feab-117">Step 2.</span></span> <span data-ttu-id="0feab-118">Hello legújabb verziói hello Azure Service Management (SM) PowerShell-modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="0feab-118">Install hello latest versions of hello Azure Service Management (SM) PowerShell modules</span></span>
<span data-ttu-id="0feab-119">Hello utasításait követve [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/overview) kapcsolatos részletes útmutatás tooconfigure a számítógép toouse hello Azure PowerShell-modulok.</span><span class="sxs-lookup"><span data-stu-id="0feab-119">Follow hello instructions in [Getting started with Azure PowerShell cmdlets](/powershell/azure/overview) for step-by-step guidance on how tooconfigure your computer toouse hello Azure PowerShell modules.</span></span>

### <a name="step-3-log-in-tooyour-azure-account-and-select-a-subscription"></a><span data-ttu-id="0feab-120">3. lépés</span><span class="sxs-lookup"><span data-stu-id="0feab-120">Step 3.</span></span> <span data-ttu-id="0feab-121">Jelentkezzen be Azure-fiók tooyour, és válasszon egy előfizetést</span><span class="sxs-lookup"><span data-stu-id="0feab-121">Log in tooyour Azure account and select a subscription</span></span>
1. <span data-ttu-id="0feab-122">Nyissa meg a PowerShell-konzolt emelt szintű jogosultságokkal, és csatlakozzon a tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="0feab-122">Open your PowerShell console with elevated rights and connect tooyour account.</span></span> <span data-ttu-id="0feab-123">A következő példa toohelp csatlakozás hello használata:</span><span class="sxs-lookup"><span data-stu-id="0feab-123">Use hello following example toohelp you connect:</span></span>

        Login-AzureRmAccount

2. <span data-ttu-id="0feab-124">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="0feab-124">Check hello subscriptions for hello account.</span></span>

        Get-AzureRmSubscription

3. <span data-ttu-id="0feab-125">Ha egynél több előfizetéssel rendelkezik, válassza ki a megjeleníteni kívánt toouse hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="0feab-125">If you have more than one subscription, select hello subscription that you want toouse.</span></span>

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. <span data-ttu-id="0feab-126">Ezután használhatja a következő parancsmag tooadd hello az Azure-előfizetés tooPowerShell hello klasszikus telepítési modell.</span><span class="sxs-lookup"><span data-stu-id="0feab-126">Next, use hello following cmdlet tooadd your Azure subscription tooPowerShell for hello classic deployment model.</span></span>

        Add-AzureAccount

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="0feab-127">Hozzon létre, és helyezze üzembe az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="0feab-127">Create and provision an ExpressRoute circuit</span></span>
### <a name="step-1-import-hello-powershell-modules-for-expressroute"></a><span data-ttu-id="0feab-128">1. lépés</span><span class="sxs-lookup"><span data-stu-id="0feab-128">Step 1.</span></span> <span data-ttu-id="0feab-129">Az ExpressRoute hello PowerShell modul importálása</span><span class="sxs-lookup"><span data-stu-id="0feab-129">Import hello PowerShell modules for ExpressRoute</span></span>
 <span data-ttu-id="0feab-130">Ha még nem tette meg, importálnia kell hello Azure és az ExpressRoute modulok hello PowerShell-munkamenetet a rendelés toostart hello ExpressRoute-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="0feab-130">If you have not already done so, you must import hello Azure and ExpressRoute modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="0feab-131">Hello modul importálása a hello helyre, amely voltak telepítve tooon helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0feab-131">You import hello modules from hello location that they were installed tooon your local computer.</span></span> <span data-ttu-id="0feab-132">Hello módszertől függően tooinstall hello modulok használják, hello helye a következő példa azt mutatja meg hello eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="0feab-132">Depending on hello method you used tooinstall hello modules, hello location may be different than hello following example shows.</span></span> <span data-ttu-id="0feab-133">Ha szükséges, módosítsa a hello példa.</span><span class="sxs-lookup"><span data-stu-id="0feab-133">Modify hello example if necessary.</span></span>  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="step-2-get-hello-list-of-supported-providers-locations-and-bandwidths"></a><span data-ttu-id="0feab-134">2. lépés</span><span class="sxs-lookup"><span data-stu-id="0feab-134">Step 2.</span></span> <span data-ttu-id="0feab-135">Támogatott szolgáltatók, a helyek és a sávszélesség hello listájának beolvasása</span><span class="sxs-lookup"><span data-stu-id="0feab-135">Get hello list of supported providers, locations, and bandwidths</span></span>
<span data-ttu-id="0feab-136">ExpressRoute-kapcsolatcsoportot létrehozni, meg kell hello támogatott kapcsolat szolgáltatókat, a helyek és a sávszélesség-beállítások listája.</span><span class="sxs-lookup"><span data-stu-id="0feab-136">Before you create an ExpressRoute circuit, you need hello list of supported connectivity providers, locations, and bandwidth options.</span></span>

<span data-ttu-id="0feab-137">PowerShell-parancsmag hello `Get-AzureDedicatedCircuitServiceProvider` adja vissza ezt az információt fogja használni a későbbi lépésekben:</span><span class="sxs-lookup"><span data-stu-id="0feab-137">hello PowerShell cmdlet `Get-AzureDedicatedCircuitServiceProvider` returns this information, which you’ll use in later steps:</span></span>

    Get-AzureDedicatedCircuitServiceProvider

<span data-ttu-id="0feab-138">Ellenőrizze a toosee, ha a kapcsolat szolgáltatójánál van szó.</span><span class="sxs-lookup"><span data-stu-id="0feab-138">Check toosee if your connectivity provider is listed there.</span></span> <span data-ttu-id="0feab-139">Jegyezze fel a következő információ, mert azt később szüksége expressroute-kapcsolatcsoporthoz létrehozásakor hello:</span><span class="sxs-lookup"><span data-stu-id="0feab-139">Make a note of hello following information because you'll need it later when you create a circuit:</span></span>

* <span data-ttu-id="0feab-140">Név</span><span class="sxs-lookup"><span data-stu-id="0feab-140">Name</span></span>
* <span data-ttu-id="0feab-141">PeeringLocations</span><span class="sxs-lookup"><span data-stu-id="0feab-141">PeeringLocations</span></span>
* <span data-ttu-id="0feab-142">BandwidthsOffered</span><span class="sxs-lookup"><span data-stu-id="0feab-142">BandwidthsOffered</span></span>

<span data-ttu-id="0feab-143">Most már készen áll a toocreate ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="0feab-143">You're now ready toocreate an ExpressRoute circuit.</span></span>         

### <a name="step-3-create-an-expressroute-circuit"></a><span data-ttu-id="0feab-144">3. lépés</span><span class="sxs-lookup"><span data-stu-id="0feab-144">Step 3.</span></span> <span data-ttu-id="0feab-145">ExpressRoute-kapcsolatcsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="0feab-145">Create an ExpressRoute circuit</span></span>
<span data-ttu-id="0feab-146">hello a következő példa bemutatja, hogyan toocreate egy 200 MB/s ExpressRoute áramkör a szilícium Valley Equinix keresztül.</span><span class="sxs-lookup"><span data-stu-id="0feab-146">hello following example shows how toocreate a 200-Mbps ExpressRoute circuit through Equinix in Silicon Valley.</span></span> <span data-ttu-id="0feab-147">Különböző szolgáltatók és más beállítások használata, amikor a kérést helyettesítse be ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="0feab-147">If you're using a different provider and different settings, substitute that information when you make your request.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0feab-148">Az ExpressRoute-kapcsolatcsoportot jelenik meg a szolgáltatási kulcs hello pillanattól lesz terhelve.</span><span class="sxs-lookup"><span data-stu-id="0feab-148">Your ExpressRoute circuit will be billed from hello moment a service key is issued.</span></span> <span data-ttu-id="0feab-149">Győződjön meg arról, hogy készen áll a tooprovision hello áramkör hello kapcsolat szolgáltatójánál esetén elvégzi ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="0feab-149">Ensure that you perform this operation when hello connectivity provider is ready tooprovision hello circuit.</span></span>
> 
> 

<span data-ttu-id="0feab-150">hello az alábbiakban látható egy példa egy kérelem egy új szolgáltatás kulcs:</span><span class="sxs-lookup"><span data-stu-id="0feab-150">hello following is an example request for a new service key:</span></span>

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

<span data-ttu-id="0feab-151">Vagy, ha azt szeretné, hogy toocreate hello prémium bővítmény ExpressRoute-kapcsolatcsoportot, hello használata a következő példa.</span><span class="sxs-lookup"><span data-stu-id="0feab-151">Or, if you want toocreate an ExpressRoute circuit with hello premium add-on, use hello following example.</span></span> <span data-ttu-id="0feab-152">Tekintse meg a toohello [ExpressRoute – gyakori kérdések](expressroute-faqs.md) hello prémium szintű bővítmény kapcsolatos további részletekért.</span><span class="sxs-lookup"><span data-stu-id="0feab-152">Refer toohello [ExpressRoute FAQ](expressroute-faqs.md) for more details about hello premium add-on.</span></span>

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


<span data-ttu-id="0feab-153">hello válasz hello szolgáltatás kulcsot fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="0feab-153">hello response will contain hello service key.</span></span> <span data-ttu-id="0feab-154">Részletes leírását, az összes hello paraméterek kaphat hello következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="0feab-154">You can get detailed descriptions of all hello parameters by running hello following:</span></span>

    get-help new-azurededicatedcircuit -detailed

### <a name="step-4-list-all-hello-expressroute-circuits"></a><span data-ttu-id="0feab-155">4. lépés</span><span class="sxs-lookup"><span data-stu-id="0feab-155">Step 4.</span></span> <span data-ttu-id="0feab-156">A lista összes hello ExpressRoute-Kapcsolatcsoportok</span><span class="sxs-lookup"><span data-stu-id="0feab-156">List all hello ExpressRoute circuits</span></span>
<span data-ttu-id="0feab-157">Hello futtatása `Get-AzureDedicatedCircuit` tooget az összes létrehozott ExpressRoute-Kapcsolatcsoportok hello parancsot:</span><span class="sxs-lookup"><span data-stu-id="0feab-157">You can run hello `Get-AzureDedicatedCircuit` command tooget a list of all hello ExpressRoute circuits that you created:</span></span>

    Get-AzureDedicatedCircuit

<span data-ttu-id="0feab-158">hello válasz valami hasonló toohello, például a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="0feab-158">hello response will be something similar toohello following example:</span></span>

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="0feab-159">Ezt az információt bármikor hello használatával lekérhető `Get-AzureDedicatedCircuit` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0feab-159">You can retrieve this information at any time by using hello `Get-AzureDedicatedCircuit` cmdlet.</span></span> <span data-ttu-id="0feab-160">Összes hello áramkör paraméterek nélkül hívható hello így sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="0feab-160">Making hello call without any parameters lists all hello circuits.</span></span> <span data-ttu-id="0feab-161">A szolgáltatás kulcs megjelenik hello *ServiceKey* mező.</span><span class="sxs-lookup"><span data-stu-id="0feab-161">Your service key will be listed in hello *ServiceKey* field.</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="0feab-162">Részletes leírását, az összes hello paraméterek kaphat hello következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="0feab-162">You can get detailed descriptions of all hello parameters by running hello following:</span></span>

    get-help get-azurededicatedcircuit -detailed

### <a name="step-5-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a><span data-ttu-id="0feab-163">5. lépés</span><span class="sxs-lookup"><span data-stu-id="0feab-163">Step 5.</span></span> <span data-ttu-id="0feab-164">Kapcsolat kulcs tooyour hello szolgáltató küldése történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="0feab-164">Send hello service key tooyour connectivity provider for provisioning</span></span>
<span data-ttu-id="0feab-165">*ServiceProviderProvisioningState* információt nyújt a hello aktuális állapotának kiépítés hello szolgáltatói oldalán.</span><span class="sxs-lookup"><span data-stu-id="0feab-165">*ServiceProviderProvisioningState* provides information on hello current state of provisioning on hello service-provider side.</span></span> <span data-ttu-id="0feab-166">*Állapot* hello Microsoft ügyféloldali hello állapot biztosít.</span><span class="sxs-lookup"><span data-stu-id="0feab-166">*Status* provides hello state on hello Microsoft side.</span></span> <span data-ttu-id="0feab-167">Kiépítés állapotok áramkör kapcsolatos további információkért lásd: hello [munkafolyamatok](expressroute-workflows.md#expressroute-circuit-provisioning-states) cikk.</span><span class="sxs-lookup"><span data-stu-id="0feab-167">For more information about circuit provisioning states, see hello [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="0feab-168">Amikor létrehoz egy új ExpressRoute-kapcsolatcsoportot, hello áramkör hello állapota a következő lehet:</span><span class="sxs-lookup"><span data-stu-id="0feab-168">When you create a new ExpressRoute circuit, hello circuit will be in hello following state:</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="0feab-169">hello áramkör kerül toohello állapotát követő hello folyamatán, amely lehetővé teszi a hello kapcsolat hitelesítésszolgáltató esetén:</span><span class="sxs-lookup"><span data-stu-id="0feab-169">hello circuit will go toohello following state when hello connectivity provider is in hello process of enabling it for you:</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

<span data-ttu-id="0feab-170">ExpressRoute-kapcsolatcsoportot kell hogy toobe képes toouse állapota a következő hello azt:</span><span class="sxs-lookup"><span data-stu-id="0feab-170">An ExpressRoute circuit must be in hello following state for you toobe able toouse it:</span></span>

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="step-6-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a><span data-ttu-id="0feab-171">6. lépés</span><span class="sxs-lookup"><span data-stu-id="0feab-171">Step 6.</span></span> <span data-ttu-id="0feab-172">Rendszeres időközönként ellenőrizze a hello állapotát és hello áramkör kulcs hello állapota</span><span class="sxs-lookup"><span data-stu-id="0feab-172">Periodically check hello status and hello state of hello circuit key</span></span>
<span data-ttu-id="0feab-173">Ez lehetővé teszi, hogy amikor a szolgáltató a kapcsolatcsoport engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="0feab-173">This lets you know when your provider has enabled your circuit.</span></span> <span data-ttu-id="0feab-174">Hello áramkör konfigurálása után *ServiceProviderProvisioningState* állapottal jelenik meg *kiépítve* a hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="0feab-174">After hello circuit has been configured, *ServiceProviderProvisioningState* will display as *Provisioned* as shown in hello following example:</span></span>

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="step-7-create-your-routing-configuration"></a><span data-ttu-id="0feab-175">7. lépés</span><span class="sxs-lookup"><span data-stu-id="0feab-175">Step 7.</span></span> <span data-ttu-id="0feab-176">Az útválasztó-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="0feab-176">Create your routing configuration</span></span>
<span data-ttu-id="0feab-177">Tekintse meg a toohello [ExpressRoute-áramkör útválasztási konfigurációja (létrehozásához és módosításához a kapcsolatcsoport esetében)](expressroute-howto-routing-classic.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="0feab-177">Refer toohello [ExpressRoute circuit routing configuration (create and modify circuit peerings)](expressroute-howto-routing-classic.md) article for step-by-step instructions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0feab-178">Ezek az utasítások csak a szolgáltatók által biztosított réteg 2 internetkapcsolati szolgáltatás használatával létrehozott toocircuits vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="0feab-178">These instructions only apply toocircuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="0feab-179">A szolgáltató által kezelt használata réteg (általában az IP VPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.</span><span class="sxs-lookup"><span data-stu-id="0feab-179">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="step-8-link-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="0feab-180">8. lépés.</span><span class="sxs-lookup"><span data-stu-id="0feab-180">Step 8.</span></span> <span data-ttu-id="0feab-181">Hivatkozásra egy virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="0feab-181">Link a virtual network tooan ExpressRoute circuit</span></span>
<span data-ttu-id="0feab-182">A következő hivatkozás egy virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="0feab-182">Next, link a virtual network tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="0feab-183">Tekintse meg a túl[Linking ExpressRoute áramkörök toovirtual hálózatok](expressroute-howto-linkvnet-classic.md) részletes útmutatásait.</span><span class="sxs-lookup"><span data-stu-id="0feab-183">Refer too[Linking ExpressRoute circuits toovirtual networks](expressroute-howto-linkvnet-classic.md) for step-by-step instructions.</span></span> <span data-ttu-id="0feab-184">Ha az ExpressRoute hello klasszikus üzembe helyezési modellt használó virtuális hálózatot kell toocreate, lásd: [hozzon létre egy virtuális hálózatot az ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span><span class="sxs-lookup"><span data-stu-id="0feab-184">If you need toocreate a virtual network using hello classic deployment model for ExpressRoute, see [Create a virtual network for ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span></span>

## <a name="getting-hello-status-of-an-expressroute-circuit"></a><span data-ttu-id="0feab-185">Egy ExpressRoute-kapcsolatcsoportot hello állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="0feab-185">Getting hello status of an ExpressRoute circuit</span></span>
<span data-ttu-id="0feab-186">Ezt az információt bármikor hello használatával lekérhető `Get-AzureCircuit` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="0feab-186">You can retrieve this information at any time by using hello `Get-AzureCircuit` cmdlet.</span></span> <span data-ttu-id="0feab-187">Összes hello áramkör paraméterek nélkül hívható hello így sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="0feab-187">Making hello call without any parameters lists all hello circuits.</span></span>

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

<span data-ttu-id="0feab-188">Egy adott ExpressRoute-kapcsolatcsoportot tájékoztatást kaphat paraméter toohello hívásként hello szolgáltatás kulcs átadásával.</span><span class="sxs-lookup"><span data-stu-id="0feab-188">You can get information on a specific ExpressRoute circuit by passing hello service key as a parameter toohello call.</span></span>

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


<span data-ttu-id="0feab-189">Minden hello paraméterek részletes leírását a következő példa hello futtatásával kaphat:</span><span class="sxs-lookup"><span data-stu-id="0feab-189">You can get detailed descriptions of all hello parameters by running hello following example:</span></span>

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a><span data-ttu-id="0feab-190">Egy ExpressRoute-kapcsolatcsoportot módosítása</span><span class="sxs-lookup"><span data-stu-id="0feab-190">Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="0feab-191">ExpressRoute-kapcsolatcsoportot egyes tulajdonságait módosíthatja kapcsolat befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="0feab-191">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="0feab-192">Mindent hello állásidő nélkül a következő:</span><span class="sxs-lookup"><span data-stu-id="0feab-192">You can do hello following with no downtime:</span></span>

* <span data-ttu-id="0feab-193">Engedélyezi vagy letiltja az ExpressRoute-kapcsolatcsoportot prémium ExpressRoute bővítményt.</span><span class="sxs-lookup"><span data-stu-id="0feab-193">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="0feab-194">Az ExpressRoute-kapcsolatcsoportot növekedése hello sávszélesség megadott érhető el kapacitás hello porton.</span><span class="sxs-lookup"><span data-stu-id="0feab-194">Increase hello bandwidth of your ExpressRoute circuit provided there is capacity available on hello port.</span></span> <span data-ttu-id="0feab-195">Vegye figyelembe, hogy a alacsonyabb verziójúra változtatása hello sávszélesség, a kapcsolat nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="0feab-195">Note that downgrading hello bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="0feab-196">Hello mérési adatok díjköteles tooUnlimited adatokat a terv módosítása</span><span class="sxs-lookup"><span data-stu-id="0feab-196">Change hello metering plan from Metered Data tooUnlimited Data.</span></span> <span data-ttu-id="0feab-197">Vegye figyelembe az adatok nem támogatott adatforgalmi tooMetered változó hello mérési terv.</span><span class="sxs-lookup"><span data-stu-id="0feab-197">Note that changing hello metering plan from Unlimited Data tooMetered Data is not supported.</span></span>
* <span data-ttu-id="0feab-198">Engedélyezheti és letilthatja *klasszikus műveletek engedélyezése*.</span><span class="sxs-lookup"><span data-stu-id="0feab-198">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="0feab-199">Tekintse meg a toohello [ExpressRoute – gyakori kérdések](expressroute-faqs.md) korlátai és korlátozásai olvashat.</span><span class="sxs-lookup"><span data-stu-id="0feab-199">Refer toohello [ExpressRoute FAQ](expressroute-faqs.md) for more information on limits and limitations.</span></span>

### <a name="tooenable-hello-expressroute-premium-add-on"></a><span data-ttu-id="0feab-200">tooenable hello ExpressRoute prémium szintű bővítmény</span><span class="sxs-lookup"><span data-stu-id="0feab-200">tooenable hello ExpressRoute premium add-on</span></span>
<span data-ttu-id="0feab-201">Hello ExpressRoute prémium szintű bővítmény hello a következő PowerShell-parancsmag segítségével engedélyezheti a meglévő kapcsolat:</span><span class="sxs-lookup"><span data-stu-id="0feab-201">You can enable hello ExpressRoute premium add-on for your existing circuit by using hello following PowerShell cmdlet:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

<span data-ttu-id="0feab-202">A kapcsolatcsoport most kell hello ExpressRoute prémium bővítmény szolgáltatások engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="0feab-202">Your circuit will now have hello ExpressRoute premium add-on features enabled.</span></span> <span data-ttu-id="0feab-203">Vegye figyelembe, hogy azt megkezdődik, amint hello parancs sikeresen lefutott számlázási meg hello prémium bővítmény képességhez.</span><span class="sxs-lookup"><span data-stu-id="0feab-203">Note that we will start billing you for hello premium add-on capability as soon as hello command has successfully run.</span></span>

### <a name="toodisable-hello-expressroute-premium-add-on"></a><span data-ttu-id="0feab-204">toodisable hello ExpressRoute prémium szintű bővítmény</span><span class="sxs-lookup"><span data-stu-id="0feab-204">toodisable hello ExpressRoute premium add-on</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0feab-205">Ez a művelet sikertelen lehet erőforrásokat, amelyek nagyobbak, mint a megengedett hello szabványos kapcsolat használata.</span><span class="sxs-lookup"><span data-stu-id="0feab-205">This operation can fail if you're using resources that are greater than what is permitted for hello standard circuit.</span></span>
> 
> 

#### <a name="considerations"></a><span data-ttu-id="0feab-206">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="0feab-206">Considerations</span></span>

* <span data-ttu-id="0feab-207">Meg kell győződnie arról, hogy virtuális hálózatok csatolt toohello áramkör hello száma kisebb, mint 10, a prémium szintű toostandard visszaminősítését előtt.</span><span class="sxs-lookup"><span data-stu-id="0feab-207">You must ensure that hello number of virtual networks linked toohello circuit is less than 10 before you downgrade from premium toostandard.</span></span> <span data-ttu-id="0feab-208">Ha nem így tesz, a frissítési kérelem sikertelen lesz, és számlázott hello díjait lesz.</span><span class="sxs-lookup"><span data-stu-id="0feab-208">If you don't do this, your update request will fail, and you'll be billed hello premium rates.</span></span>
* <span data-ttu-id="0feab-209">Minden virtuális hálózat más geopolitikai régiókban kell választható.</span><span class="sxs-lookup"><span data-stu-id="0feab-209">You must unlink all virtual networks in other geopolitical regions.</span></span> <span data-ttu-id="0feab-210">Ha nem így tesz, a frissítési kérelem sikertelen lesz, és számlázott hello díjait lesz.</span><span class="sxs-lookup"><span data-stu-id="0feab-210">If you don't do this, your update request will fail, and you'll be billed hello premium rates.</span></span>
* <span data-ttu-id="0feab-211">Az útvonaltábla a magánhálózati társviszony-létesítés kisebb, mint 4000 útvonalait kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0feab-211">Your route table must be less than 4,000 routes for private peering.</span></span> <span data-ttu-id="0feab-212">Ha az útvonal tábla mérete nagyobb, mint 4000 útvonalakat, hello BGP-munkamenetet eldobja, és nem fog újra engedélyezve, amíg a hirdetett hello számához nem 4000 éri el.</span><span class="sxs-lookup"><span data-stu-id="0feab-212">If your route table size is greater than 4,000 routes, hello BGP session will drop and won't be reenabled until hello number of advertised prefixes goes below 4,000.</span></span>

#### <a name="disable-hello-premium-add-on"></a><span data-ttu-id="0feab-213">Prémium szintű hello-bővítmény letiltása</span><span class="sxs-lookup"><span data-stu-id="0feab-213">Disable hello premium add-on</span></span>
<span data-ttu-id="0feab-214">A meglévő kör hello ExpressRoute prémium szintű bővítmény letilthatja hello a következő PowerShell-parancsmag segítségével:</span><span class="sxs-lookup"><span data-stu-id="0feab-214">You can disable hello ExpressRoute premium add-on for your existing circuit by using hello following PowerShell cmdlet:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="tooupdate-hello-expressroute-circuit-bandwidth"></a><span data-ttu-id="0feab-215">tooupdate hello ExpressRoute-kapcsolatcsoport sávszélessége</span><span class="sxs-lookup"><span data-stu-id="0feab-215">tooupdate hello ExpressRoute circuit bandwidth</span></span>
<span data-ttu-id="0feab-216">Ellenőrizze a hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md) támogatott sávszélesség-beállítások a szolgáltatóhoz.</span><span class="sxs-lookup"><span data-stu-id="0feab-216">Check hello [ExpressRoute FAQ](expressroute-faqs.md) for supported bandwidth options for your provider.</span></span> <span data-ttu-id="0feab-217">Bármely mérete nagyobb, mint a meglévő expressroute hello mérete, amíg hello tartozó fizikai port (amely a kapcsolatcsoport létre van hozva) lehetővé teszi, hogy ki tudja választani.</span><span class="sxs-lookup"><span data-stu-id="0feab-217">You can pick any size that is greater than hello size of your existing circuit as long as hello physical port (on which your circuit is created) allows.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0feab-218">Előfordulhat, hogy toorecreate hello ExpressRoute-kapcsolatcsoportot esetén nincs elég kapacitás hello meglévő porton.</span><span class="sxs-lookup"><span data-stu-id="0feab-218">You may have toorecreate hello ExpressRoute circuit if there is inadequate capacity on hello existing port.</span></span> <span data-ttu-id="0feab-219">Hello kapcsolatcsoport nem frissíthető, ha nincsenek további kapacitást érhető el az adott helyhez.</span><span class="sxs-lookup"><span data-stu-id="0feab-219">You cannot upgrade hello circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="0feab-220">Nem csökken a hello sávszélesség az ExpressRoute-kapcsolatcsoportot megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="0feab-220">You cannot reduce hello bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="0feab-221">Alacsonyabb verziójúra változtatása sávszélesség toodeprovision hello ExpressRoute-kapcsolatcsoportot meg, és majd építenie az új ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="0feab-221">Downgrading bandwidth requires you toodeprovision hello ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 
> 

#### <a name="resize-a-circuit"></a><span data-ttu-id="0feab-222">Expressroute-kapcsolatcsoporthoz átméretezése</span><span class="sxs-lookup"><span data-stu-id="0feab-222">Resize a circuit</span></span>

<span data-ttu-id="0feab-223">Miután eldöntötte, hogy milyen méretű van szüksége, használhatja a kapcsolatcsoport a következő parancs tooresize hello:</span><span class="sxs-lookup"><span data-stu-id="0feab-223">After you decide what size you need, you can use hello following command tooresize your circuit:</span></span>

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="0feab-224">A kapcsolatcsoport fog rendelkezik lett méretű hello Microsoft oldalán.</span><span class="sxs-lookup"><span data-stu-id="0feab-224">Your circuit will have been sized up on hello Microsoft side.</span></span> <span data-ttu-id="0feab-225">Vegye fel a kapcsolatot a kapcsolat szolgáltató tooupdate konfigurációit, az ügyféloldali toomatch ezt a módosítást.</span><span class="sxs-lookup"><span data-stu-id="0feab-225">You must contact your connectivity provider tooupdate configurations on their side toomatch this change.</span></span> <span data-ttu-id="0feab-226">Vegye figyelembe, hogy azt indítása számlázást, a hello frissítve ettől a sávszélesség beállítást.</span><span class="sxs-lookup"><span data-stu-id="0feab-226">Note that we will start billing you for hello updated bandwidth option from this point on.</span></span>

<span data-ttu-id="0feab-227">Ha megjelenik a következő a hiba, ha hello kapcsolatcsoport sávszélessége növelése hello, az azt jelenti nincs nincs elegendő sávszélesség hello fizikai port a meglévő expressroute létrehozási helyének bal.</span><span class="sxs-lookup"><span data-stu-id="0feab-227">If you see hello following error when increasing hello circuit bandwidth, it means there is no sufficient bandwidth left on hello physical port where your existing circuit is created.</span></span> <span data-ttu-id="0feab-228">Toodelete a kapcsolatcsoport rendelkezik, és hozzon létre egy új kapcsolat hello méretű van szüksége.</span><span class="sxs-lookup"><span data-stu-id="0feab-228">You have toodelete this circuit and create a new circuit of hello size you need.</span></span> 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available tooperform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="0feab-229">Megszüntetés és az ExpressRoute-kapcsolatcsoport törlése</span><span class="sxs-lookup"><span data-stu-id="0feab-229">Deprovisioning and deleting an ExpressRoute circuit</span></span>

### <a name="considerations"></a><span data-ttu-id="0feab-230">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="0feab-230">Considerations</span></span>

* <span data-ttu-id="0feab-231">Ez a művelet toosucceed ExpressRoute-kapcsolatcsoportot hello minden virtuális hálózatokat kell választható.</span><span class="sxs-lookup"><span data-stu-id="0feab-231">You must unlink all virtual networks from hello ExpressRoute circuit for this operation toosucceed.</span></span> <span data-ttu-id="0feab-232">Ellenőrizze toosee, ha a virtuális hálózatok, amelyek kapcsolódó toohello áramkör, ha ez a művelet sikertelen.</span><span class="sxs-lookup"><span data-stu-id="0feab-232">Check toosee if you have any virtual networks that are linked toohello circuit if this operation fails.</span></span>
* <span data-ttu-id="0feab-233">Ha hello ExpressRoute körön szolgáltató üzembe helyezési állapota **kiépítési** vagy **kiépítve** együttműködve kell a service provider toodeprovision hello kapcsolatcsoport az oldalon.</span><span class="sxs-lookup"><span data-stu-id="0feab-233">If hello ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider toodeprovision hello circuit on their side.</span></span> <span data-ttu-id="0feab-234">Rendszer továbbra is tooreserve erőforrások és kiszámlázni Önnek, amíg hello szolgáltató megszüntetési hello áramkör befejeződik, és értesíti a számunkra.</span><span class="sxs-lookup"><span data-stu-id="0feab-234">We will continue tooreserve resources and bill you until hello service provider completes deprovisioning hello circuit and notifies us.</span></span>
* <span data-ttu-id="0feab-235">Ha hello szolgáltató rendelkezik platformelőfizetés hello áramkör (üzembe helyezési állapota hello szolgáltató értéke túl**nincs kiépítve**) hello áramkör törölhet.</span><span class="sxs-lookup"><span data-stu-id="0feab-235">If hello service provider has deprovisioned hello circuit (hello service provider provisioning state is set too**Not provisioned**) you can then delete hello circuit.</span></span> <span data-ttu-id="0feab-236">Ezzel leállítja a számlázási hello kör.</span><span class="sxs-lookup"><span data-stu-id="0feab-236">This will stop billing for hello circuit.</span></span>

#### <a name="delete-a-circuit"></a><span data-ttu-id="0feab-237">A kapcsolat törlése</span><span class="sxs-lookup"><span data-stu-id="0feab-237">Delete a circuit</span></span>

<span data-ttu-id="0feab-238">Az ExpressRoute-kapcsolatcsoport törlése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="0feab-238">You can delete your ExpressRoute circuit by running hello following command:</span></span>

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a><span data-ttu-id="0feab-239">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0feab-239">Next steps</span></span>
<span data-ttu-id="0feab-240">Miután létrehozta a kapcsolatcsoport, győződjön meg arról, hogy Ön hello a következő:</span><span class="sxs-lookup"><span data-stu-id="0feab-240">After you create your circuit, make sure that you do hello following:</span></span>

* [<span data-ttu-id="0feab-241">Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoport esetében routing</span><span class="sxs-lookup"><span data-stu-id="0feab-241">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-classic.md)
* [<span data-ttu-id="0feab-242">A virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot hivatkozás</span><span class="sxs-lookup"><span data-stu-id="0feab-242">Link your virtual network tooyour ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-classic.md)

