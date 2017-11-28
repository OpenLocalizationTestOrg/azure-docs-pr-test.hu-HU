---
title: a PowerShell-lel Manager aaaMigrate tooResource |} Microsoft Docs
description: "Ez a cikk végigvezeti hello platform által támogatott áttelepítési IaaS-erőforrások, például a virtuális gépek (VM), a virtuális hálózatokon (Vnetek) és a storage-fiókok klasszikus tooAzure Resource Managerrel (ARM) az Azure PowerShell-parancsok segítségével"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2b3dff9b-2e99-4556-acc5-d75ef234af9c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: b5b2987da292f1c241be71a354b0c2e1a96a07c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-iaas-resources-from-classic-tooazure-resource-manager-by-using-azure-powershell"></a><span data-ttu-id="c7ac1-103">IaaS-erőforrásokra át klasszikus tooAzure erőforrás-kezelő Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="c7ac1-103">Migrate IaaS resources from classic tooAzure Resource Manager by using Azure PowerShell</span></span>
<span data-ttu-id="c7ac1-104">Ezen lépések bemutatják, hogyan toouse Azure PowerShell-parancsok toomigrate infrastruktúra hello klasszikus telepítési modell toohello Azure Resource Manager telepítési modellből a szolgáltató (IaaS) erőforrásként.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-104">These steps show you how toouse Azure PowerShell commands toomigrate infrastructure as a service (IaaS) resources from hello classic deployment model toohello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="c7ac1-105">Ha azt szeretné, is áttelepítheti erőforrások hello segítségével [Azure parancssori felület (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c7ac1-105">If you want, you can also migrate resources by using hello [Azure Command Line Interface (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).</span></span>

* <span data-ttu-id="c7ac1-106">A támogatott áttelepítési forgatókönyvek háttér, lásd: [IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének Platform által támogatott](migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c7ac1-106">For background on supported migration scenarios, see [Platform-supported migration of IaaS resources from classic tooAzure Resource Manager](migration-classic-resource-manager-overview.md).</span></span>
* <span data-ttu-id="c7ac1-107">Részletes útmutatást és egy áttelepítési forgatókönyv: [műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus tooAzure erőforrás-kezelő](migration-classic-resource-manager-deep-dive.md).</span><span class="sxs-lookup"><span data-stu-id="c7ac1-107">For detailed guidance and a migration walkthrough, see [Technical deep dive on platform-supported migration from classic tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md).</span></span>
* [<span data-ttu-id="c7ac1-108">A leggyakoribb áttelepítési hibák áttekintése</span><span class="sxs-lookup"><span data-stu-id="c7ac1-108">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md)

<br>
<span data-ttu-id="c7ac1-109">Ez a folyamatábra tooidentify hello sorrendet, amelyben lépést kell végrehajtani az áttelepítési folyamat során toobe</span><span class="sxs-lookup"><span data-stu-id="c7ac1-109">Here is a flowchart tooidentify hello order in which steps need toobe executed during a migration process</span></span>

![Képernyőkép a hello áttelepítés lépései](media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-plan-for-migration"></a><span data-ttu-id="c7ac1-111">1. lépés: Az áttelepítés tervezése</span><span class="sxs-lookup"><span data-stu-id="c7ac1-111">Step 1: Plan for migration</span></span>
<span data-ttu-id="c7ac1-112">Az alábbiakban néhány gyakorlati tanácsok, amely értékeli a klasszikus tooResource Manager áttelepítése IaaS-erőforrásokra, javasoljuk:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-112">Here are a few best practices that we recommend as you evaluate migrating IaaS resources from classic tooResource Manager:</span></span>

* <span data-ttu-id="c7ac1-113">Olvassa végig hello [támogatott és nem támogatott a szolgáltatásnak és konfigurálásnak](migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c7ac1-113">Read through hello [supported and unsupported features and configurations](migration-classic-resource-manager-overview.md).</span></span> <span data-ttu-id="c7ac1-114">Ha még nem támogatott konfigurációk vagy szolgáltatások használó virtuális gépek, azt javasoljuk, várja meg a hello konfiguráció/szolgáltatás támogatási toobe jelentette be.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-114">If you have virtual machines that use unsupported configurations or features, we recommend that you wait for hello configuration/feature support toobe announced.</span></span> <span data-ttu-id="c7ac1-115">Azt is megteheti Ha azt az igényeinek megfelelő, távolítsa el a szolgáltatást, vagy konfigurációs tooenable áttelepítést kilépni.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-115">Alternatively, if it suits your needs, remove that feature or move out of that configuration tooenable migration.</span></span>
* <span data-ttu-id="c7ac1-116">Ha olyan parancsfájlok, amelyek központi telepítése az infrastruktúra és az alkalmazások ma rendelkezik automatikus, próbálja toocreate egy hasonló vizsgálat beállítása az áttelepítés ezen parancsfájlok használatával.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-116">If you have automated scripts that deploy your infrastructure and applications today, try toocreate a similar test setup by using those scripts for migration.</span></span> <span data-ttu-id="c7ac1-117">Másik lehetőségként állíthat be minta környezetek hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-117">Alternatively, you can set up sample environments by using hello Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c7ac1-118">Alkalmazásátjárót jelenleg nem támogatottak az áttelepítést a klasszikus tooResource Manager.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-118">Application Gateways are not currently supported for migration from classic tooResource Manager.</span></span> <span data-ttu-id="c7ac1-119">toomigrate Alkalmazásátjáró, klasszikus virtuális hálózat hello átjáró előkészítési művelet toomove hello hálózati futtatása előtt távolítsa el.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-119">toomigrate a classic virtual network with an Application gateway, remove hello gateway before running a Prepare operation toomove hello network.</span></span> <span data-ttu-id="c7ac1-120">Hello áttelepítés befejezése után újra hello átjáró az Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-120">After you complete hello migration, reconnect hello gateway in Azure Resource Manager.</span></span>
>
><span data-ttu-id="c7ac1-121">Csatlakozás tooExpressRoute Kapcsolatcsoportok egy másik előfizetésben található ExpressRoute-átjárók nem telepíthetők át automatikusan.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-121">ExpressRoute gateways connecting tooExpressRoute circuits in another subscription cannot be migrated automatically.</span></span> <span data-ttu-id="c7ac1-122">Ilyen esetekben hello ExpressRoute-átjáró eltávolítása, telepítse át a virtuális hálózati hello, és hozza létre újra a hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-122">In such cases, remove hello ExpressRoute gateway, migrate hello virtual network and recreate hello gateway.</span></span> <span data-ttu-id="c7ac1-123">Ellenőrizze a [áttelepítése ExpressRoute áramkörök és társított virtuális hálózatokat hello klasszikus toohello Resource Manager üzembe helyezési modellben](../../expressroute/expressroute-migration-classic-resource-manager.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-123">Please see [Migrate ExpressRoute circuits and associated virtual networks from hello classic toohello Resource Manager deployment model](../../expressroute/expressroute-migration-classic-resource-manager.md) for more information.</span></span>
>
>

## <a name="step-2-install-hello-latest-version-of-azure-powershell"></a><span data-ttu-id="c7ac1-124">2. lépés: Hello Azure PowerShell legújabb verziójának telepítése</span><span class="sxs-lookup"><span data-stu-id="c7ac1-124">Step 2: Install hello latest version of Azure PowerShell</span></span>
<span data-ttu-id="c7ac1-125">Nincsenek a két fő lehetőség közül választhat tooinstall Azure PowerShell: [PowerShell-galériában](https://www.powershellgallery.com/profiles/azure-sdk/) vagy [Webplatform-telepítővel (WebPI)](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="c7ac1-125">There are two main options tooinstall Azure PowerShell: [PowerShell Gallery](https://www.powershellgallery.com/profiles/azure-sdk/) or [Web Platform Installer (WebPI)](http://aka.ms/webpi-azps).</span></span> <span data-ttu-id="c7ac1-126">WebPI havi frissítések kap.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-126">WebPI receives monthly updates.</span></span> <span data-ttu-id="c7ac1-127">PowerShell-galériában folyamatosan frissítések kap.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-127">PowerShell Gallery receives updates on a continuous basis.</span></span> <span data-ttu-id="c7ac1-128">Ez a cikk az Azure PowerShell verzió 2.1.0 alapul.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-128">This article is based on Azure PowerShell version 2.1.0.</span></span>

<span data-ttu-id="c7ac1-129">A telepítési utasításokért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c7ac1-129">For installation instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<br>

## <a name="step-3-ensure-that-you-are-an-administrator-for-hello-subscription-in-azure-portal"></a><span data-ttu-id="c7ac1-130">3. lépés: Ellenőrizze, hogy a rendszergazda hello előfizetés Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c7ac1-130">Step 3: Ensure that you are an administrator for hello subscription in Azure portal</span></span>
<span data-ttu-id="c7ac1-131">tooperform az áttelepítés meg kell adni a hello hello előfizetés társadminisztrátoraként [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c7ac1-131">tooperform this migration, you must be added as a co-administrator for hello subscription in hello [Azure portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="c7ac1-132">Jelentkezzen be a hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c7ac1-132">Sign into hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c7ac1-133">Hello központ menüben válassza ki a **előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-133">On hello Hub menu, select **Subscription**.</span></span> <span data-ttu-id="c7ac1-134">Ha nem látja, válassza ki a **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-134">If you don't see it, select **More services**.</span></span>
3. <span data-ttu-id="c7ac1-135">Megfelelő hello bejegyzés található, akkor nézze meg hello **saját SZEREPKÖR** mező.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-135">Find hello appropriate subscription entry, then look at hello **MY ROLE** field.</span></span> <span data-ttu-id="c7ac1-136">Egy közös rendszergazda hello értéket kell _fiókadminisztrátor_.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-136">For a co-administrator, hello value should be _Account admin_.</span></span>

<span data-ttu-id="c7ac1-137">Ha nem tudja tooadd társadminisztrátorának, majd lépjen kapcsolatba a szolgáltatás-rendszergazdai vagy társadminisztrátori hello előfizetés tooget saját kezűleg a hozzá tartozó.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-137">If you are not able tooadd a co-administrator, then contact a service administrator or co-administrator for hello subscription tooget yourself added.</span></span>   

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a><span data-ttu-id="c7ac1-138">4. lépés: Állítsa be az előfizetéshez, és regisztráljon áttelepítése</span><span class="sxs-lookup"><span data-stu-id="c7ac1-138">Step 4: Set your subscription and sign up for migration</span></span>
<span data-ttu-id="c7ac1-139">Először egy PowerShell-parancssorba.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-139">First, start a PowerShell prompt.</span></span> <span data-ttu-id="c7ac1-140">Az áttelepítéshez, mindkét klasszikus környezet szüksége tooset és erőforrás-kezelő.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-140">For migration, you need tooset up your environment for both classic and Resource Manager.</span></span>

<span data-ttu-id="c7ac1-141">Jelentkezzen be a fiók tooyour hello Resource Manager modellt.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-141">Sign in tooyour account for hello Resource Manager model.</span></span>

```powershell
    Login-AzureRmAccount
```

<span data-ttu-id="c7ac1-142">Hello az elérhető előfizetések beolvasása hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-142">Get hello available subscriptions by using hello following command:</span></span>

```powershell
    Get-AzureRMSubscription | Sort Name | Select Name
```

<span data-ttu-id="c7ac1-143">Állítsa be az Azure-előfizetéshez hello az aktuális munkamenet.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-143">Set your Azure subscription for hello current session.</span></span> <span data-ttu-id="c7ac1-144">Ez a példa beállítása hello alapértelmezett előfizetés neve túl**saját Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-144">This example sets hello default subscription name too**My Azure Subscription**.</span></span> <span data-ttu-id="c7ac1-145">A saját cserélje le a hello példa előfizetés nevét.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-145">Replace hello example subscription name with your own.</span></span>

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> <span data-ttu-id="c7ac1-146">Regisztrációs egy egyszeri lépést, de egyszer a migrálás megkezdése előtt kell tennie.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-146">Registration is a one-time step, but you must do it once before attempting migration.</span></span> <span data-ttu-id="c7ac1-147">Ha nem regisztrálja, hello a következő hibaüzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-147">Without registering, you see hello following error message:</span></span>
>
> <span data-ttu-id="c7ac1-148">*BadRequest: Az előfizetés nincs regisztrálva az áttelepítéshez.*</span><span class="sxs-lookup"><span data-stu-id="c7ac1-148">*BadRequest : Subscription is not registered for migration.*</span></span>
>
>

<span data-ttu-id="c7ac1-149">Hello áttelepítési erőforrás-szolgáltató regisztrálása hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-149">Register with hello migration resource provider by using hello following command:</span></span>

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

<span data-ttu-id="c7ac1-150">Kis türelmet hello regisztrációs toofinish öt percet.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-150">Please wait five minutes for hello registration toofinish.</span></span> <span data-ttu-id="c7ac1-151">Hello jóváhagyási hello állapotának hello a következő parancs használatával ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-151">You can check hello status of hello approval by using hello following command:</span></span>

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

<span data-ttu-id="c7ac1-152">Győződjön meg arról, hogy van-e RegistrationState `Registered` folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-152">Make sure that RegistrationState is `Registered` before you proceed.</span></span>

<span data-ttu-id="c7ac1-153">Most jelentkezzen be a klasszikus modellt hello tooyour fiókjával.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-153">Now sign in tooyour account for hello classic model.</span></span>

```powershell
    Add-AzureAccount
```

<span data-ttu-id="c7ac1-154">Hello az elérhető előfizetések beolvasása hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-154">Get hello available subscriptions by using hello following command:</span></span>

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

<span data-ttu-id="c7ac1-155">Állítsa be az Azure-előfizetéshez hello az aktuális munkamenet.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-155">Set your Azure subscription for hello current session.</span></span> <span data-ttu-id="c7ac1-156">Ez a példa beállítja hello alapértelmezett előfizetés túl**saját Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-156">This example sets hello default subscription too**My Azure Subscription**.</span></span> <span data-ttu-id="c7ac1-157">A saját cserélje le a hello példa előfizetés nevét.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-157">Replace hello example subscription name with your own.</span></span>

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-5-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-hello-azure-region-of-your-current-deployment-or-vnet"></a><span data-ttu-id="c7ac1-158">5. lépés: Ellenőrizze, hogy elegendő Azure Resource Manager virtuális gép magok a hello Azure-régió, a jelenlegi üzemelő példány vagy virtuális hálózaton</span><span class="sxs-lookup"><span data-stu-id="c7ac1-158">Step 5: Make sure you have enough Azure Resource Manager Virtual Machine cores in hello Azure region of your current deployment or VNET</span></span>
<span data-ttu-id="c7ac1-159">A következő PowerShell parancs toocheck hello aktuális magok száma rendelkezik az Azure Resource Manager hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-159">You can use hello following PowerShell command toocheck hello current number of cores you have in Azure Resource Manager.</span></span> <span data-ttu-id="c7ac1-160">toolearn kvótákat, az alapvető kapcsolatos további információkért lásd: [korlátozásai és hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).</span><span class="sxs-lookup"><span data-stu-id="c7ac1-160">toolearn more about core quotas, see [Limits and hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).</span></span>

<span data-ttu-id="c7ac1-161">Ez a példa hello hello rendelkezésre állását ellenőrzi. **USA nyugati régiója** régióban.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-161">This example checks hello availability in hello **West US** region.</span></span> <span data-ttu-id="c7ac1-162">A saját cserélje le a hello például régió neve.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-162">Replace hello example region name with your own.</span></span>

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-6-run-commands-toomigrate-your-iaas-resources"></a><span data-ttu-id="c7ac1-163">6. lépés: Futtassa a parancsokat toomigrate az IaaS-erőforrások</span><span class="sxs-lookup"><span data-stu-id="c7ac1-163">Step 6: Run commands toomigrate your IaaS resources</span></span>
> [!NOTE]
> <span data-ttu-id="c7ac1-164">Az itt ismertetett összes hello művelet idempotent.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-164">All hello operations described here are idempotent.</span></span> <span data-ttu-id="c7ac1-165">Ha a probléma nem támogatott szolgáltatása vagy konfigurációs hiba, azt javasoljuk, hogy újra hello előkészítése, megszakítása vagy véglegesítése a műveletet.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-165">If you have a problem other than an unsupported feature or a configuration error, we recommend that you retry hello prepare, abort, or commit operation.</span></span> <span data-ttu-id="c7ac1-166">hello platform hello művelet újra próbálkozik.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-166">hello platform then tries hello action again.</span></span>
>
>

## <a name="step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a><span data-ttu-id="c7ac1-167">6.1. lépés: 1. lehetőség – (nem része virtuális hálózatnak) felhőszolgáltatás a virtuális gépek áttelepítése</span><span class="sxs-lookup"><span data-stu-id="c7ac1-167">Step 6.1: Option 1 - Migrate virtual machines in a cloud service (not in a virtual network)</span></span>
<span data-ttu-id="c7ac1-168">Hello listájának beszerzése felhőszolgáltatások hello a következő parancs használatával, majd mentse hello felhőalapú szolgáltatás, amelyet az toomigrate.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-168">Get hello list of cloud services by using hello following command, and then pick hello cloud service that you want toomigrate.</span></span> <span data-ttu-id="c7ac1-169">Ha hello virtuális gépek hello a felhőszolgáltatásban található egy virtuális hálózatot, vagy webes vagy feldolgozói szerepköröket rendelkeznek, hello parancs hibaüzenetet ad vissza.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-169">If hello VMs in hello cloud service are in a virtual network or if they have web or worker roles, hello command returns an error message.</span></span>

```powershell
    Get-AzureService | ft Servicename
```

<span data-ttu-id="c7ac1-170">Hello telepítési neve hello felhőszolgáltatás beolvasása.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-170">Get hello deployment name for hello cloud service.</span></span> <span data-ttu-id="c7ac1-171">Ebben a példában hello szolgáltatás neve megkülönbözteti a **saját szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-171">In this example, hello service name is **My Service**.</span></span> <span data-ttu-id="c7ac1-172">Cserélje le a saját szolgáltatásnév hello példa szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-172">Replace hello example service name with your own service name.</span></span>

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

<span data-ttu-id="c7ac1-173">Készítse elő a hello virtuális gépek áttelepítésre hello felhőszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-173">Prepare hello virtual machines in hello cloud service for migration.</span></span> <span data-ttu-id="c7ac1-174">A két beállítások toochoose rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-174">You have two options toochoose from.</span></span>

* <span data-ttu-id="c7ac1-175">**1. lehetőség. Hello virtuális gépek tooa platform által létrehozott virtuális hálózat áttelepítése**</span><span class="sxs-lookup"><span data-stu-id="c7ac1-175">**Option 1. Migrate hello VMs tooa platform-created virtual network**</span></span>

    <span data-ttu-id="c7ac1-176">Először ellenőrzi, hogy hello felhőalapú szolgáltatás a következő parancsok hello segítségével telepíthet át:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-176">First, validate if you can migrate hello cloud service using hello following commands:</span></span>

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    <span data-ttu-id="c7ac1-177">hello előző parancs megjeleníti a figyelmeztetések és hibák, amelyek blokkolják az áttelepítés.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-177">hello preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="c7ac1-178">Ha az ellenőrzés nem jelez hibát, majd továbbléphet toohello **Prepare** . lépés:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-178">If validation is successful, then you can move on toohello **Prepare** step:</span></span>

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* <span data-ttu-id="c7ac1-179">**2. lehetőség. Tooan meglévő virtuális hálózat hello Resource Manager üzembe helyezési modellel áttelepítése**</span><span class="sxs-lookup"><span data-stu-id="c7ac1-179">**Option 2. Migrate tooan existing virtual network in hello Resource Manager deployment model**</span></span>

    <span data-ttu-id="c7ac1-180">Ebben a példában beállítása az erőforráscsoport neve túl hello**myResourceGroup**, virtuális hálózat neve túl hello**myVirtualNetwork** és alhálózati név túl hello**mySubNet**.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-180">This example sets hello resource group name too**myResourceGroup**, hello virtual network name too**myVirtualNetwork** and hello subnet name too**mySubNet**.</span></span> <span data-ttu-id="c7ac1-181">Hello nevek hello példában cserélje le a saját erőforrások hello nevét.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-181">Replace hello names in hello example with hello names of your own resources.</span></span>

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    <span data-ttu-id="c7ac1-182">Először ellenőrzi, hogy áttelepítheti a virtuális hálózat hello hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-182">First, validate if you can migrate hello virtual network using hello following command:</span></span>

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    <span data-ttu-id="c7ac1-183">hello előző parancs megjeleníti a figyelmeztetések és hibák, amelyek blokkolják az áttelepítés.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-183">hello preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="c7ac1-184">Ha az ellenőrzés nem jelez hibát, majd folytassa a hello előkészítési lépés a következő:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-184">If validation is successful, then you can proceed with hello following Prepare step:</span></span>

    ```powershell
        Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

<span data-ttu-id="c7ac1-185">Után hello előkészítési művelet sikeres hello megelőző beállítások valamelyikével, a lekérdezés hello áttelepítési állapotának hello virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-185">After hello Prepare operation succeeds with either of hello preceding options, query hello migration state of hello VMs.</span></span> <span data-ttu-id="c7ac1-186">Győződjön meg arról, hogy vannak-e a hello `Prepared` állapotát.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-186">Ensure that they are in hello `Prepared` state.</span></span>

<span data-ttu-id="c7ac1-187">Ez a példa beállítása virtuális gép neve túl hello**myVM**.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-187">This example sets hello VM name too**myVM**.</span></span> <span data-ttu-id="c7ac1-188">Hello példa neve cserélje le a saját virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-188">Replace hello example name with your own VM name.</span></span>

```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
```

<span data-ttu-id="c7ac1-189">A PowerShell vagy a hello Azure portál segítségével erőforrások előkészített hello hello konfigurációjának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-189">Check hello configuration for hello prepared resources by using either PowerShell or hello Azure portal.</span></span> <span data-ttu-id="c7ac1-190">Ha nem az áttelepítéshez, és azt szeretné, hogy toogo hátsó toohello régi állapot, használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-190">If you are not ready for migration and you want toogo back toohello old state, use hello following command:</span></span>

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

<span data-ttu-id="c7ac1-191">Ha hello előkészített konfiguráció megfelelőnek tűnik, előre, és véglegesíti hello erőforrások hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-191">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command:</span></span>

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

## <a name="step-61-option-2---migrate-virtual-machines-in-a-virtual-network"></a><span data-ttu-id="c7ac1-192">6.1. lépés: 2. lehetőség – a virtuális hálózatban lévő virtuális gépek áttelepítése</span><span class="sxs-lookup"><span data-stu-id="c7ac1-192">Step 6.1: Option 2 - Migrate virtual machines in a virtual network</span></span>

<span data-ttu-id="c7ac1-193">toomigrate virtuális gépek virtuális hálózatban, hello virtuális hálózat áttelepítése.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-193">toomigrate virtual machines in a virtual network, you migrate hello virtual network.</span></span> <span data-ttu-id="c7ac1-194">hello virtuális gépek automatikusan áttelepíti a hello virtuális hálózattal.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-194">hello virtual machines automatically migrate with hello virtual network.</span></span> <span data-ttu-id="c7ac1-195">Mentse hello virtuális hálózatot, hogy szeretné-e toomigrate.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-195">Pick hello virtual network that you want toomigrate.</span></span>
> [!NOTE]
> <span data-ttu-id="c7ac1-196">[Egyetlen klasszikus virtuális gép áttelepítése](migrate-single-classic-to-resource-manager.md) hozzon létre egy új erőforrás-kezelő virtuális gép hello (az operációs rendszer és data) virtuális merevlemezfájlokat hello virtuális gép használatával felügyelt lemezzel.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-196">[Migrate single classic virtual machine](migrate-single-classic-to-resource-manager.md) by creating a new Resource Manager virtual machine with Managed Disks using hello VHD (OS and data) files of hello virtual machine.</span></span>
<br>

> [!NOTE]
> <span data-ttu-id="c7ac1-197">hello virtuálishálózat-név eltérhet hello megjelenő új portált.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-197">hello virtual network name might be different from what is shown in hello new Portal.</span></span> <span data-ttu-id="c7ac1-198">hello új Azure-portál megjeleníti hello néven `[vnet-name]` hello tényleges virtuális hálózati név típusú, de `Group [resource-group-name] [vnet-name]`.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-198">hello new Azure Portal displays hello name as `[vnet-name]` but hello actual virtual network name is of type `Group [resource-group-name] [vnet-name]`.</span></span> <span data-ttu-id="c7ac1-199">Mielőtt áttelepítené a keresési tényleges virtuálishálózat-név hello hello paranccsal `Get-AzureVnetSite | Select -Property Name` , illetve hello régi Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-199">Before migrating, lookup hello actual virtual network name using hello command `Get-AzureVnetSite | Select -Property Name` or view it in hello old Azure Portal.</span></span> 

<span data-ttu-id="c7ac1-200">Ez a példa beállítása hello virtuálishálózat-név túl**myVnet**.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-200">This example sets hello virtual network name too**myVnet**.</span></span> <span data-ttu-id="c7ac1-201">A saját cserélje le a hello példa a virtuális hálózat nevére.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-201">Replace hello example virtual network name with your own.</span></span>

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> <span data-ttu-id="c7ac1-202">Hello virtuális hálózat nem támogatott konfigurációjú webes vagy feldolgozói szerepköröket, vagy a virtuális gépeket tartalmaz, ha egy érvényesítési hibaüzenet kap.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-202">If hello virtual network contains web or worker roles, or VMs with unsupported configurations, you get a validation error message.</span></span>
>
>

<span data-ttu-id="c7ac1-203">Először ellenőrzi, hogy a virtuális hálózati hello hello a következő parancs használatával telepíthet át:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-203">First, validate if you can migrate hello virtual network by using hello following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

<span data-ttu-id="c7ac1-204">hello előző parancs megjeleníti a figyelmeztetések és hibák, amelyek blokkolják az áttelepítés.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-204">hello preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="c7ac1-205">Ha az ellenőrzés nem jelez hibát, majd folytassa a hello előkészítési lépés a következő:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-205">If validation is successful, then you can proceed with hello following Prepare step:</span></span>

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

<span data-ttu-id="c7ac1-206">A virtuális gépek Azure PowerShell vagy az Azure-portálon hello segítségével előkészített hello hello konfigurációjának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-206">Check hello configuration for hello prepared virtual machines by using either Azure PowerShell or hello Azure portal.</span></span> <span data-ttu-id="c7ac1-207">Ha nem az áttelepítéshez, és azt szeretné, hogy toogo hátsó toohello régi állapot, használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-207">If you are not ready for migration and you want toogo back toohello old state, use hello following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

<span data-ttu-id="c7ac1-208">Ha hello előkészített konfiguráció megfelelőnek tűnik, előre, és véglegesíti hello erőforrások hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-208">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

## <a name="step-62-migrate-a-storage-account"></a><span data-ttu-id="c7ac1-209">A tárfiók lépés 6.2 áttelepítése</span><span class="sxs-lookup"><span data-stu-id="c7ac1-209">Step 6.2 Migrate a storage account</span></span>
<span data-ttu-id="c7ac1-210">Ha elkészült hello virtuális gépeinek áttelepítését, ajánlott hello tárfiókok az áttelepítést.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-210">Once you're done migrating hello virtual machines, we recommend you migrate hello storage accounts.</span></span>

<span data-ttu-id="c7ac1-211">Hello tárfiók az áttelepítés előtt hajtson végre megelőző előfeltétel-ellenőrzések:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-211">Before you migrate hello storage account, please perform preceding prerequisite checks:</span></span>

* <span data-ttu-id="c7ac1-212">**Klasszikus, amelynek lemezek hello storage-fiókban tárolt virtuális gépek áttelepítése**</span><span class="sxs-lookup"><span data-stu-id="c7ac1-212">**Migrate classic virtual machines whose disks are stored in hello storage account**</span></span>

    <span data-ttu-id="c7ac1-213">Minden hello klasszikus virtuális gépek lemezei RoleName és DiskName tulajdonságainak hello tárfiók parancs megelőző adja vissza.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-213">Preceding command returns RoleName and DiskName properties of all hello classic VM disks in hello storage account.</span></span> <span data-ttu-id="c7ac1-214">RoleName hello neve hello virtuális gép toowhich lemez van csatolva.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-214">RoleName is hello name of hello virtual machine toowhich a disk is attached.</span></span> <span data-ttu-id="c7ac1-215">Ha parancs megelőző lemezek majd győződjön meg arról, hogy ezek a lemezek vannak csatolva hozzá virtuális gépek toowhich települnek át áttelepítése előtt hello storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-215">If preceding command returns disks then ensure that virtual machines toowhich these disks are attached are migrated before migrating hello storage account.</span></span>
    ```powershell
     $storageAccountName = 'yourStorageAccountName'
      Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
      DiskName | Format-List -Property RoleName, DiskName

    ```
* <span data-ttu-id="c7ac1-216">**Hello storage-fiókban tárolt választani klasszikus virtuális gépek lemezei törlése**</span><span class="sxs-lookup"><span data-stu-id="c7ac1-216">**Delete unattached classic VM disks stored in hello storage account**</span></span>

    <span data-ttu-id="c7ac1-217">Található, nem csatolt klasszikus virtuális gépek lemezei hello tárolt fiók használatával a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-217">Find unattached classic VM disks in hello storage account using following command:</span></span>

    ```powershell
        $storageAccountName = 'yourStorageAccountName'
        Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Where-Object -Property AttachedTo -EQ $null | Format-List -Property DiskName  

    ```
    <span data-ttu-id="c7ac1-218">Ha a fenti parancs beolvasása lemezek törölje ezeket a következő parancs használatával lemezeket:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-218">If above command returns disks then delete these disks using following command:</span></span>

    ```powershell
       Remove-AzureDisk -DiskName 'yourDiskName'
    ```
* <span data-ttu-id="c7ac1-219">**Hello storage-fiókban tárolt Virtuálisgép-rendszerképek törlése**</span><span class="sxs-lookup"><span data-stu-id="c7ac1-219">**Delete VM images stored in hello storage account**</span></span>

    <span data-ttu-id="c7ac1-220">Előző parancs adja vissza minden hello Virtuálisgép-rendszerképek hello storage-fiókban tárolt operációsrendszer-lemezzel.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-220">Preceding command returns all hello VM images with OS disk stored in hello storage account.</span></span>
     ```powershell
        Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                } | Select-Object -Property ImageName, ImageLabel
     ```
     <span data-ttu-id="c7ac1-221">Adatlemezek hello storage-fiókban tárolt összes hello Virtuálisgép-rendszerképek parancs megelőző adja vissza.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-221">Preceding command returns all hello VM images with data disks stored in hello storage account.</span></span>
     ```powershell

        Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                         -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                        } | Select-Object -Property ImageName, ImageLabel
     ```
    <span data-ttu-id="c7ac1-222">Törölje az előző parancs használata parancsok fent által visszaadott összes hello VM lemezképet:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-222">Delete all hello VM images returned by above commands using preceding command:</span></span>
    ```powershell
    Remove-AzureVMImage -ImageName 'yourImageName'
    ```

<span data-ttu-id="c7ac1-223">Minden tárfiók az áttelepítés ellenőrzése hello a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-223">Validate each storage account for migration by using hello following command.</span></span> <span data-ttu-id="c7ac1-224">Ebben a példában hello tárfiókneve **myStorageAccount**.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-224">In this example, hello storage account name is **myStorageAccount**.</span></span> <span data-ttu-id="c7ac1-225">Hello példa neve helyére a saját tárfiók hello nevét.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-225">Replace hello example name with hello name of your own storage account.</span></span>

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Validate -StorageAccountName $storageAccountName
```

<span data-ttu-id="c7ac1-226">Következő lépés az tooPrepare hello storage-fiók áttelepítése</span><span class="sxs-lookup"><span data-stu-id="c7ac1-226">Next step is tooPrepare hello storage account for migration</span></span>

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

<span data-ttu-id="c7ac1-227">Az Azure PowerShell vagy az Azure-portálon hello segítségével tárfiók előkészített hello hello konfigurációjának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="c7ac1-227">Check hello configuration for hello prepared storage account by using either Azure PowerShell or hello Azure portal.</span></span> <span data-ttu-id="c7ac1-228">Ha nem az áttelepítéshez, és azt szeretné, hogy toogo hátsó toohello régi állapot, használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-228">If you are not ready for migration and you want toogo back toohello old state, use hello following command:</span></span>

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

<span data-ttu-id="c7ac1-229">Ha hello előkészített konfiguráció megfelelőnek tűnik, előre, és véglegesíti hello erőforrások hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="c7ac1-229">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command:</span></span>

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a><span data-ttu-id="c7ac1-230">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c7ac1-230">Next steps</span></span>
* [<span data-ttu-id="c7ac1-231">IaaS-erőforrásokra, erőforrás-kezelő klasszikus tooAzure a platform által támogatott áttelepítésének áttekintése</span><span class="sxs-lookup"><span data-stu-id="c7ac1-231">Overview of platform-supported migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="c7ac1-232">Műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus tooAzure erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="c7ac1-232">Technical deep dive on platform-supported migration from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="c7ac1-233">IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének tervezése</span><span class="sxs-lookup"><span data-stu-id="c7ac1-233">Planning for migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="c7ac1-234">Parancssori felület toomigrate IaaS erőforrásainak klasszikus tooAzure erőforrás-kezelő használata</span><span class="sxs-lookup"><span data-stu-id="c7ac1-234">Use CLI toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="c7ac1-235">IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének védelmével kapcsolatos közösségi eszközök</span><span class="sxs-lookup"><span data-stu-id="c7ac1-235">Community tools for assisting with migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="c7ac1-236">A leggyakoribb áttelepítési hibák áttekintése</span><span class="sxs-lookup"><span data-stu-id="c7ac1-236">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="c7ac1-237">Felülvizsgálati hello legtöbb kapcsolatos gyakori kérdések a klasszikus tooAzure erőforrás-kezelő áttelepítése IaaS-erőforrásokra</span><span class="sxs-lookup"><span data-stu-id="c7ac1-237">Review hello most frequently asked questions about migrating IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
