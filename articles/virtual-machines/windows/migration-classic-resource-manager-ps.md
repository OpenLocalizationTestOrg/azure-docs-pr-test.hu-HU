---
title: "Telepítse át az erőforrás-kezelő a PowerShell-lel |} Microsoft Docs"
description: "Ez a cikk végigvezeti a platform által támogatott áttelepítési IaaS-erőforrások, például a virtuális gépek (VM), a virtuális hálózatokon (Vnetek) és a storage-fiókok a klasszikus Azure Resource Managerrel (ARM) Azure PowerShell-parancsok segítségével"
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
ms.openlocfilehash: 489e6cc6bd3c5b36635f5f7e398d08fed681d2e7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a><span data-ttu-id="a1649-103">Telepítse át IaaS-erőforrásokra a klasszikus Azure Resource Manager Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="a1649-103">Migrate IaaS resources from classic to Azure Resource Manager by using Azure PowerShell</span></span>
<span data-ttu-id="a1649-104">Ezeket a lépéseket mutatja be Azure PowerShell-parancsok használatával telepítse át az infrastruktúra erőforrásként egy szolgáltatási (IaaS) a klasszikus telepítési modellből az Azure Resource Manager telepítési modellhez.</span><span class="sxs-lookup"><span data-stu-id="a1649-104">These steps show you how to use Azure PowerShell commands to migrate infrastructure as a service (IaaS) resources from the classic deployment model to the Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="a1649-105">Ha azt szeretné, is áttelepítheti erőforrások használatával a [Azure parancssori felület (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).</span><span class="sxs-lookup"><span data-stu-id="a1649-105">If you want, you can also migrate resources by using the [Azure Command Line Interface (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).</span></span>

* <span data-ttu-id="a1649-106">A támogatott áttelepítési forgatókönyvek háttér, lásd: [Platform által támogatott áttelepítési IaaS erőforrások a klasszikus Azure Resource Manager](migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a1649-106">For background on supported migration scenarios, see [Platform-supported migration of IaaS resources from classic to Azure Resource Manager](migration-classic-resource-manager-overview.md).</span></span>
* <span data-ttu-id="a1649-107">Részletes útmutatást és egy áttelepítési forgatókönyv: [műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus Azure Resource Manager](migration-classic-resource-manager-deep-dive.md).</span><span class="sxs-lookup"><span data-stu-id="a1649-107">For detailed guidance and a migration walkthrough, see [Technical deep dive on platform-supported migration from classic to Azure Resource Manager](migration-classic-resource-manager-deep-dive.md).</span></span>
* [<span data-ttu-id="a1649-108">A leggyakoribb áttelepítési hibák áttekintése</span><span class="sxs-lookup"><span data-stu-id="a1649-108">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md)

<br>
<span data-ttu-id="a1649-109">Ez a sorrendet, amelyben lépéseket kell végrehajtani az áttelepítési folyamat során azonosításához folyamatábra</span><span class="sxs-lookup"><span data-stu-id="a1649-109">Here is a flowchart to identify the order in which steps need to be executed during a migration process</span></span>

![Képernyőkép a migrálási lépésekről](media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-plan-for-migration"></a><span data-ttu-id="a1649-111">1. lépés: Az áttelepítés tervezése</span><span class="sxs-lookup"><span data-stu-id="a1649-111">Step 1: Plan for migration</span></span>
<span data-ttu-id="a1649-112">Az alábbiakban néhány gyakorlati tanácsok, azt javasoljuk, áttelepítése IaaS-erőforrásokra a hagyományos erőforrás-kezelő értékeli:</span><span class="sxs-lookup"><span data-stu-id="a1649-112">Here are a few best practices that we recommend as you evaluate migrating IaaS resources from classic to Resource Manager:</span></span>

* <span data-ttu-id="a1649-113">Olvassa végig a [támogatott és nem támogatott a szolgáltatásnak és konfigurálásnak](migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a1649-113">Read through the [supported and unsupported features and configurations](migration-classic-resource-manager-overview.md).</span></span> <span data-ttu-id="a1649-114">Ha még nem támogatott konfigurációk vagy szolgáltatások használó virtuális gépek, azt javasoljuk, hogy a konfiguráció/szolgáltatás támogatási bejelentések várja.</span><span class="sxs-lookup"><span data-stu-id="a1649-114">If you have virtual machines that use unsupported configurations or features, we recommend that you wait for the configuration/feature support to be announced.</span></span> <span data-ttu-id="a1649-115">Azt is megteheti Ha azt az igényeinek megfelelő, távolítsa el a szolgáltatást, vagy helyezze át kívül, hogy a konfigurálás engedélyezze az áttelepítést.</span><span class="sxs-lookup"><span data-stu-id="a1649-115">Alternatively, if it suits your needs, remove that feature or move out of that configuration to enable migration.</span></span>
* <span data-ttu-id="a1649-116">Ha olyan parancsfájlok, amelyek központi telepítése az infrastruktúra és az alkalmazások ma rendelkezik automatikus, hozzon létre egy hasonló vizsgálat beállítása az áttelepítés ezen parancsfájlok használatával.</span><span class="sxs-lookup"><span data-stu-id="a1649-116">If you have automated scripts that deploy your infrastructure and applications today, try to create a similar test setup by using those scripts for migration.</span></span> <span data-ttu-id="a1649-117">Másik lehetőségként állíthat be minta környezetekben az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="a1649-117">Alternatively, you can set up sample environments by using the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a1649-118">Alkalmazásátjárót jelenleg nem támogatottak az áttelepítéshez a klasszikus az erőforrás-kezelő.</span><span class="sxs-lookup"><span data-stu-id="a1649-118">Application Gateways are not currently supported for migration from classic to Resource Manager.</span></span> <span data-ttu-id="a1649-119">A klasszikus virtuális hálózatot az Alkalmazásátjáró át, a hálózati áthelyezése egy előkészítési művelet futtatása előtt távolítsa el az átjáró.</span><span class="sxs-lookup"><span data-stu-id="a1649-119">To migrate a classic virtual network with an Application gateway, remove the gateway before running a Prepare operation to move the network.</span></span> <span data-ttu-id="a1649-120">Az áttelepítés befejezése után csatlakoztassa újra az átjárót az Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a1649-120">After you complete the migration, reconnect the gateway in Azure Resource Manager.</span></span>
>
><span data-ttu-id="a1649-121">Kapcsolódás egy másik előfizetésben található ExpressRoute-Kapcsolatcsoportok ExpressRoute-átjárók nem telepíthetők át automatikusan.</span><span class="sxs-lookup"><span data-stu-id="a1649-121">ExpressRoute gateways connecting to ExpressRoute circuits in another subscription cannot be migrated automatically.</span></span> <span data-ttu-id="a1649-122">Ebben az esetben távolítsa el az ExpressRoute-átjárót, telepítse át a virtuális hálózaton, és hozza létre újra az átjárót.</span><span class="sxs-lookup"><span data-stu-id="a1649-122">In such cases, remove the ExpressRoute gateway, migrate the virtual network and recreate the gateway.</span></span> <span data-ttu-id="a1649-123">Ellenőrizze a [áttelepítése ExpressRoute áramkörök, és a Resource Manager üzembe helyezési modellel klasszikus virtuális hálózatok társított](../../expressroute/expressroute-migration-classic-resource-manager.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="a1649-123">Please see [Migrate ExpressRoute circuits and associated virtual networks from the classic to the Resource Manager deployment model](../../expressroute/expressroute-migration-classic-resource-manager.md) for more information.</span></span>
>
>

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a><span data-ttu-id="a1649-124">2. lépés: Az Azure PowerShell legújabb verziójának telepítése</span><span class="sxs-lookup"><span data-stu-id="a1649-124">Step 2: Install the latest version of Azure PowerShell</span></span>
<span data-ttu-id="a1649-125">Azure PowerShell telepítése két fő lehetőség: [PowerShell-galériában](https://www.powershellgallery.com/profiles/azure-sdk/) vagy [Webplatform-telepítővel (WebPI)](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="a1649-125">There are two main options to install Azure PowerShell: [PowerShell Gallery](https://www.powershellgallery.com/profiles/azure-sdk/) or [Web Platform Installer (WebPI)](http://aka.ms/webpi-azps).</span></span> <span data-ttu-id="a1649-126">WebPI havi frissítések kap.</span><span class="sxs-lookup"><span data-stu-id="a1649-126">WebPI receives monthly updates.</span></span> <span data-ttu-id="a1649-127">PowerShell-galériában folyamatosan frissítések kap.</span><span class="sxs-lookup"><span data-stu-id="a1649-127">PowerShell Gallery receives updates on a continuous basis.</span></span> <span data-ttu-id="a1649-128">Ez a cikk az Azure PowerShell verzió 2.1.0 alapul.</span><span class="sxs-lookup"><span data-stu-id="a1649-128">This article is based on Azure PowerShell version 2.1.0.</span></span>

<span data-ttu-id="a1649-129">A telepítési utasításokért lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a1649-129">For installation instructions, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<br>

## <a name="step-3-ensure-that-you-are-an-administrator-for-the-subscription-in-azure-portal"></a><span data-ttu-id="a1649-130">3. lépés: Ellenőrizze, hogy a rendszergazda az előfizetéshez tartozó Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a1649-130">Step 3: Ensure that you are an administrator for the subscription in Azure portal</span></span>
<span data-ttu-id="a1649-131">Az áttelepítés végrehajtásához meg kell adni az előfizetés társadminisztrátoraként a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a1649-131">To perform this migration, you must be added as a co-administrator for the subscription in the [Azure portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="a1649-132">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a1649-132">Sign into the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a1649-133">A központ menüben válassza ki a **előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="a1649-133">On the Hub menu, select **Subscription**.</span></span> <span data-ttu-id="a1649-134">Ha nem látja, válassza ki a **további szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="a1649-134">If you don't see it, select **More services**.</span></span>
3. <span data-ttu-id="a1649-135">Keresse meg a megfelelő előfizetés bejegyzést, majd tekintse meg a **saját SZEREPKÖR** mező.</span><span class="sxs-lookup"><span data-stu-id="a1649-135">Find the appropriate subscription entry, then look at the **MY ROLE** field.</span></span> <span data-ttu-id="a1649-136">Egy közös rendszergazda értékének kell _fiókadminisztrátor_.</span><span class="sxs-lookup"><span data-stu-id="a1649-136">For a co-administrator, the value should be _Account admin_.</span></span>

<span data-ttu-id="a1649-137">Ha nem tud társadminisztrátorának hozzáadni, majd kérje meg, egy szolgáltatás-rendszergazda vagy a közös rendszergazdát az előfizetéshez tartozó saját kezűleg hozzá.</span><span class="sxs-lookup"><span data-stu-id="a1649-137">If you are not able to add a co-administrator, then contact a service administrator or co-administrator for the subscription to get yourself added.</span></span>   

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a><span data-ttu-id="a1649-138">4. lépés: Állítsa be az előfizetéshez, és regisztráljon áttelepítése</span><span class="sxs-lookup"><span data-stu-id="a1649-138">Step 4: Set your subscription and sign up for migration</span></span>
<span data-ttu-id="a1649-139">Először egy PowerShell-parancssorba.</span><span class="sxs-lookup"><span data-stu-id="a1649-139">First, start a PowerShell prompt.</span></span> <span data-ttu-id="a1649-140">Az áttelepítéshez, be kell állítania a környezetet az mindkét klasszikus és Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a1649-140">For migration, you need to set up your environment for both classic and Resource Manager.</span></span>

<span data-ttu-id="a1649-141">Jelentkezzen be a fiókjának a Resource Manager modellt.</span><span class="sxs-lookup"><span data-stu-id="a1649-141">Sign in to your account for the Resource Manager model.</span></span>

```powershell
    Login-AzureRmAccount
```

<span data-ttu-id="a1649-142">Az elérhető előfizetések beolvasása a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="a1649-142">Get the available subscriptions by using the following command:</span></span>

```powershell
    Get-AzureRMSubscription | Sort Name | Select Name
```

<span data-ttu-id="a1649-143">Állítsa be az Azure-előfizetéshez az aktuális munkamenet.</span><span class="sxs-lookup"><span data-stu-id="a1649-143">Set your Azure subscription for the current session.</span></span> <span data-ttu-id="a1649-144">Ebben a példában a alapértelmezett előfizetés nevének beállítása **saját Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="a1649-144">This example sets the default subscription name to **My Azure Subscription**.</span></span> <span data-ttu-id="a1649-145">Cserélje le a példában előfizetés nevét a saját.</span><span class="sxs-lookup"><span data-stu-id="a1649-145">Replace the example subscription name with your own.</span></span>

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> <span data-ttu-id="a1649-146">Regisztrációs egy egyszeri lépést, de egyszer a migrálás megkezdése előtt kell tennie.</span><span class="sxs-lookup"><span data-stu-id="a1649-146">Registration is a one-time step, but you must do it once before attempting migration.</span></span> <span data-ttu-id="a1649-147">Ha nem regisztrálja, a következő hibaüzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a1649-147">Without registering, you see the following error message:</span></span>
>
> <span data-ttu-id="a1649-148">*BadRequest: Az előfizetés nincs regisztrálva az áttelepítéshez.*</span><span class="sxs-lookup"><span data-stu-id="a1649-148">*BadRequest : Subscription is not registered for migration.*</span></span>
>
>

<span data-ttu-id="a1649-149">Az áttelepítés erőforrás-szolgáltató regisztrálása a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="a1649-149">Register with the migration resource provider by using the following command:</span></span>

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

<span data-ttu-id="a1649-150">Kis türelmet, a regisztráció befejezéséhez öt perc.</span><span class="sxs-lookup"><span data-stu-id="a1649-150">Please wait five minutes for the registration to finish.</span></span> <span data-ttu-id="a1649-151">A jóváhagyási állapotát a következő paranccsal ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="a1649-151">You can check the status of the approval by using the following command:</span></span>

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

<span data-ttu-id="a1649-152">Győződjön meg arról, hogy van-e RegistrationState `Registered` folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="a1649-152">Make sure that RegistrationState is `Registered` before you proceed.</span></span>

<span data-ttu-id="a1649-153">Most jelentkezzen be a Klasszikus modell esetében a fiókjába.</span><span class="sxs-lookup"><span data-stu-id="a1649-153">Now sign in to your account for the classic model.</span></span>

```powershell
    Add-AzureAccount
```

<span data-ttu-id="a1649-154">Az elérhető előfizetések beolvasása a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="a1649-154">Get the available subscriptions by using the following command:</span></span>

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

<span data-ttu-id="a1649-155">Állítsa be az Azure-előfizetéshez az aktuális munkamenet.</span><span class="sxs-lookup"><span data-stu-id="a1649-155">Set your Azure subscription for the current session.</span></span> <span data-ttu-id="a1649-156">Ez a példa állítja be az alapértelmezett előfizetés **saját Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="a1649-156">This example sets the default subscription to **My Azure Subscription**.</span></span> <span data-ttu-id="a1649-157">Cserélje le a példában előfizetés nevét a saját.</span><span class="sxs-lookup"><span data-stu-id="a1649-157">Replace the example subscription name with your own.</span></span>

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-5-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a><span data-ttu-id="a1649-158">5. lépés: Ellenőrizze, hogy elegendő Azure Resource Manager virtuális gép magok Azure-régióban a jelenlegi üzemelő példány vagy virtuális hálózaton</span><span class="sxs-lookup"><span data-stu-id="a1649-158">Step 5: Make sure you have enough Azure Resource Manager Virtual Machine cores in the Azure region of your current deployment or VNET</span></span>
<span data-ttu-id="a1649-159">A következő PowerShell-parancs segítségével ellenőrizze, rendelkezik az Azure Resource Manager magok száma.</span><span class="sxs-lookup"><span data-stu-id="a1649-159">You can use the following PowerShell command to check the current number of cores you have in Azure Resource Manager.</span></span> <span data-ttu-id="a1649-160">Core kvóták kapcsolatos további információkért lásd: [korlátozásai és az Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).</span><span class="sxs-lookup"><span data-stu-id="a1649-160">To learn more about core quotas, see [Limits and the Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).</span></span>

<span data-ttu-id="a1649-161">Ebben a példában elérhetőségét ellenőrzi a **USA nyugati régiója** régióban.</span><span class="sxs-lookup"><span data-stu-id="a1649-161">This example checks the availability in the **West US** region.</span></span> <span data-ttu-id="a1649-162">Cserélje le a példában régió neve a saját.</span><span class="sxs-lookup"><span data-stu-id="a1649-162">Replace the example region name with your own.</span></span>

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-6-run-commands-to-migrate-your-iaas-resources"></a><span data-ttu-id="a1649-163">6. lépés: Futtassa a parancsokat az IaaS-erőforrások áttelepítése</span><span class="sxs-lookup"><span data-stu-id="a1649-163">Step 6: Run commands to migrate your IaaS resources</span></span>
> [!NOTE]
> <span data-ttu-id="a1649-164">Itt leírt műveletek idempotent.</span><span class="sxs-lookup"><span data-stu-id="a1649-164">All the operations described here are idempotent.</span></span> <span data-ttu-id="a1649-165">Ha a probléma nem támogatott szolgáltatása vagy konfigurációs hiba, azt javasoljuk, hogy a prepare újra, vagy megszakításra műveletet.</span><span class="sxs-lookup"><span data-stu-id="a1649-165">If you have a problem other than an unsupported feature or a configuration error, we recommend that you retry the prepare, abort, or commit operation.</span></span> <span data-ttu-id="a1649-166">A platform majd próbálja megismételni a műveletet.</span><span class="sxs-lookup"><span data-stu-id="a1649-166">The platform then tries the action again.</span></span>
>
>

## <a name="step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a><span data-ttu-id="a1649-167">6.1. lépés: 1. lehetőség – (nem része virtuális hálózatnak) felhőszolgáltatás a virtuális gépek áttelepítése</span><span class="sxs-lookup"><span data-stu-id="a1649-167">Step 6.1: Option 1 - Migrate virtual machines in a cloud service (not in a virtual network)</span></span>
<span data-ttu-id="a1649-168">A felhőszolgáltatások listájának lekérdezése a következő paranccsal, és ezután válassza ki a felhőalapú szolgáltatás, amely az áttelepíteni kívánt.</span><span class="sxs-lookup"><span data-stu-id="a1649-168">Get the list of cloud services by using the following command, and then pick the cloud service that you want to migrate.</span></span> <span data-ttu-id="a1649-169">Ha a felhőszolgáltatás a virtuális gépek egy virtuális hálózatot, vagy ha webes vagy feldolgozói szerepköröket, a parancs hibaüzenetet ad vissza.</span><span class="sxs-lookup"><span data-stu-id="a1649-169">If the VMs in the cloud service are in a virtual network or if they have web or worker roles, the command returns an error message.</span></span>

```powershell
    Get-AzureService | ft Servicename
```

<span data-ttu-id="a1649-170">A központi telepítés nevét, a felhőszolgáltatás beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a1649-170">Get the deployment name for the cloud service.</span></span> <span data-ttu-id="a1649-171">Ebben a példában a szolgáltatás neve megkülönbözteti **saját szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="a1649-171">In this example, the service name is **My Service**.</span></span> <span data-ttu-id="a1649-172">A példa szolgáltatásnév cserélje le a saját szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="a1649-172">Replace the example service name with your own service name.</span></span>

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

<span data-ttu-id="a1649-173">Készítse elő a virtuális gépek áttelepítése a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a1649-173">Prepare the virtual machines in the cloud service for migration.</span></span> <span data-ttu-id="a1649-174">Rendelkezik két lehetőség közül választhat.</span><span class="sxs-lookup"><span data-stu-id="a1649-174">You have two options to choose from.</span></span>

* <span data-ttu-id="a1649-175">**1. lehetőség. A virtuális gépek áttelepítése platform által létrehozott virtuális hálózathoz**</span><span class="sxs-lookup"><span data-stu-id="a1649-175">**Option 1. Migrate the VMs to a platform-created virtual network**</span></span>

    <span data-ttu-id="a1649-176">Először ellenőrzi, hogy áttelepítheti a felhőalapú szolgáltatás, a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="a1649-176">First, validate if you can migrate the cloud service using the following commands:</span></span>

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    <span data-ttu-id="a1649-177">Az előző parancs megjeleníti a figyelmeztetések és hibák, amelyek blokkolják az áttelepítés.</span><span class="sxs-lookup"><span data-stu-id="a1649-177">The preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="a1649-178">Ha az ellenőrzés nem jelez hibát, majd áthelyezheti a a **Prepare** . lépés:</span><span class="sxs-lookup"><span data-stu-id="a1649-178">If validation is successful, then you can move on to the **Prepare** step:</span></span>

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* <span data-ttu-id="a1649-179">**2. lehetőség. A Resource Manager üzembe helyezési modellel meglévő virtuális hálózat áttelepítése**</span><span class="sxs-lookup"><span data-stu-id="a1649-179">**Option 2. Migrate to an existing virtual network in the Resource Manager deployment model**</span></span>

    <span data-ttu-id="a1649-180">Ebben a példában az erőforráscsoport neve állítja **myResourceGroup**, a virtuális hálózatok nevét, **myVirtualNetwork** és a alhálózati név **mySubNet**.</span><span class="sxs-lookup"><span data-stu-id="a1649-180">This example sets the resource group name to **myResourceGroup**, the virtual network name to **myVirtualNetwork** and the subnet name to **mySubNet**.</span></span> <span data-ttu-id="a1649-181">Cserélje le a példában szereplő erőforrásnevek a saját.</span><span class="sxs-lookup"><span data-stu-id="a1649-181">Replace the names in the example with the names of your own resources.</span></span>

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    <span data-ttu-id="a1649-182">Először ellenőrzi, hogy áttelepítheti a virtuális hálózat, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a1649-182">First, validate if you can migrate the virtual network using the following command:</span></span>

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    <span data-ttu-id="a1649-183">Az előző parancs megjeleníti a figyelmeztetések és hibák, amelyek blokkolják az áttelepítés.</span><span class="sxs-lookup"><span data-stu-id="a1649-183">The preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="a1649-184">Ha az ellenőrzés nem jelez hibát, majd folytathatja a következő előkészítési lépés:</span><span class="sxs-lookup"><span data-stu-id="a1649-184">If validation is successful, then you can proceed with the following Prepare step:</span></span>

    ```powershell
        Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

<span data-ttu-id="a1649-185">Után az előkészítési művelet sikeres, az előző beállítások valamelyikével, a lekérdezés a virtuális gépek áttelepítésének állapotát.</span><span class="sxs-lookup"><span data-stu-id="a1649-185">After the Prepare operation succeeds with either of the preceding options, query the migration state of the VMs.</span></span> <span data-ttu-id="a1649-186">Győződjön meg arról, hogy a `Prepared` állapotát.</span><span class="sxs-lookup"><span data-stu-id="a1649-186">Ensure that they are in the `Prepared` state.</span></span>

<span data-ttu-id="a1649-187">Ebben a példában a virtuális gép nevének beállítása **myVM**.</span><span class="sxs-lookup"><span data-stu-id="a1649-187">This example sets the VM name to **myVM**.</span></span> <span data-ttu-id="a1649-188">A példa neve cserélje le a saját virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="a1649-188">Replace the example name with your own VM name.</span></span>

```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
```

<span data-ttu-id="a1649-189">Ellenőrizze a konfigurációt, az előkészített erőforrások PowerShell vagy az Azure portál segítségével.</span><span class="sxs-lookup"><span data-stu-id="a1649-189">Check the configuration for the prepared resources by using either PowerShell or the Azure portal.</span></span> <span data-ttu-id="a1649-190">Ha nem az áttelepítéshez, és térjen vissza a régi állapot szeretne, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a1649-190">If you are not ready for migration and you want to go back to the old state, use the following command:</span></span>

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

<span data-ttu-id="a1649-191">Az előkészített konfiguráció megfelelőnek tűnik, ha előre, és véglegesíti az erőforrásokat a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="a1649-191">If the prepared configuration looks good, you can move forward and commit the resources by using the following command:</span></span>

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

## <a name="step-61-option-2---migrate-virtual-machines-in-a-virtual-network"></a><span data-ttu-id="a1649-192">6.1. lépés: 2. lehetőség – a virtuális hálózatban lévő virtuális gépek áttelepítése</span><span class="sxs-lookup"><span data-stu-id="a1649-192">Step 6.1: Option 2 - Migrate virtual machines in a virtual network</span></span>

<span data-ttu-id="a1649-193">A virtuális hálózatban lévő virtuális gépek áttelepítéséhez telepítse át a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="a1649-193">To migrate virtual machines in a virtual network, you migrate the virtual network.</span></span> <span data-ttu-id="a1649-194">A virtuális gépek automatikusan áttelepíti a virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="a1649-194">The virtual machines automatically migrate with the virtual network.</span></span> <span data-ttu-id="a1649-195">Válassza ki az áttelepíteni kívánt virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="a1649-195">Pick the virtual network that you want to migrate.</span></span>
> [!NOTE]
> <span data-ttu-id="a1649-196">[Egyetlen klasszikus virtuális gép áttelepítése](migrate-single-classic-to-resource-manager.md) hozzon létre egy új erőforrás-kezelő virtuális gépet felügyelt merevlemezzel a virtuális gép virtuális merevlemez (az operációs rendszer és data) fájlokat használja.</span><span class="sxs-lookup"><span data-stu-id="a1649-196">[Migrate single classic virtual machine](migrate-single-classic-to-resource-manager.md) by creating a new Resource Manager virtual machine with Managed Disks using the VHD (OS and data) files of the virtual machine.</span></span>
<br>

> [!NOTE]
> <span data-ttu-id="a1649-197">Lehet, hogy a virtuális hálózat neve eltér az új portálon is látható.</span><span class="sxs-lookup"><span data-stu-id="a1649-197">The virtual network name might be different from what is shown in the new Portal.</span></span> <span data-ttu-id="a1649-198">Az új Azure-portál megjeleníti a nevet a következőként `[vnet-name]` a tényleges virtuális hálózati név típusú, de `Group [resource-group-name] [vnet-name]`.</span><span class="sxs-lookup"><span data-stu-id="a1649-198">The new Azure Portal displays the name as `[vnet-name]` but the actual virtual network name is of type `Group [resource-group-name] [vnet-name]`.</span></span> <span data-ttu-id="a1649-199">Mielőtt áttelepítené, keresés a tényleges virtuális hálózat neve, a parancs segítségével `Get-AzureVnetSite | Select -Property Name` vagy tekintse meg a régi Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="a1649-199">Before migrating, lookup the actual virtual network name using the command `Get-AzureVnetSite | Select -Property Name` or view it in the old Azure Portal.</span></span> 

<span data-ttu-id="a1649-200">Ebben a példában a virtuális hálózat nevének beállítása **myVnet**.</span><span class="sxs-lookup"><span data-stu-id="a1649-200">This example sets the virtual network name to **myVnet**.</span></span> <span data-ttu-id="a1649-201">Cserélje le a példa a virtuális hálózat nevére a saját.</span><span class="sxs-lookup"><span data-stu-id="a1649-201">Replace the example virtual network name with your own.</span></span>

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> <span data-ttu-id="a1649-202">Ha a virtuális hálózat nem támogatott konfigurációjú webes vagy feldolgozói szerepköröket, vagy a virtuális gépeket tartalmaz, egy érvényesítési hibaüzenet kap.</span><span class="sxs-lookup"><span data-stu-id="a1649-202">If the virtual network contains web or worker roles, or VMs with unsupported configurations, you get a validation error message.</span></span>
>
>

<span data-ttu-id="a1649-203">Először ellenőrzi, hogy a virtuális hálózati telepíthet át a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="a1649-203">First, validate if you can migrate the virtual network by using the following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

<span data-ttu-id="a1649-204">Az előző parancs megjeleníti a figyelmeztetések és hibák, amelyek blokkolják az áttelepítés.</span><span class="sxs-lookup"><span data-stu-id="a1649-204">The preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="a1649-205">Ha az ellenőrzés nem jelez hibát, majd folytathatja a következő előkészítési lépés:</span><span class="sxs-lookup"><span data-stu-id="a1649-205">If validation is successful, then you can proceed with the following Prepare step:</span></span>

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

<span data-ttu-id="a1649-206">Ellenőrizze az előkészített virtuális gépek konfigurációját az Azure PowerShell vagy az Azure portál segítségével.</span><span class="sxs-lookup"><span data-stu-id="a1649-206">Check the configuration for the prepared virtual machines by using either Azure PowerShell or the Azure portal.</span></span> <span data-ttu-id="a1649-207">Ha nem az áttelepítéshez, és térjen vissza a régi állapot szeretne, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a1649-207">If you are not ready for migration and you want to go back to the old state, use the following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

<span data-ttu-id="a1649-208">Az előkészített konfiguráció megfelelőnek tűnik, ha előre, és véglegesíti az erőforrásokat a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="a1649-208">If the prepared configuration looks good, you can move forward and commit the resources by using the following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

## <a name="step-62-migrate-a-storage-account"></a><span data-ttu-id="a1649-209">A tárfiók lépés 6.2 áttelepítése</span><span class="sxs-lookup"><span data-stu-id="a1649-209">Step 6.2 Migrate a storage account</span></span>
<span data-ttu-id="a1649-210">Miután befejezte a virtuális gépek áttelepítéséhez, azt javasoljuk, telepíti át, hogy a storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="a1649-210">Once you're done migrating the virtual machines, we recommend you migrate the storage accounts.</span></span>

<span data-ttu-id="a1649-211">A tárfiók az áttelepítés előtt hajtson végre megelőző előfeltétel-ellenőrzések:</span><span class="sxs-lookup"><span data-stu-id="a1649-211">Before you migrate the storage account, please perform preceding prerequisite checks:</span></span>

* <span data-ttu-id="a1649-212">**Klasszikus, amelynek lemezek a storage-fiókban tárolt virtuális gépek áttelepítése**</span><span class="sxs-lookup"><span data-stu-id="a1649-212">**Migrate classic virtual machines whose disks are stored in the storage account**</span></span>

    <span data-ttu-id="a1649-213">A tárfiók a klasszikus virtuális gép lemezeivel RoleName és DiskName tulajdonságainak parancs megelőző adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a1649-213">Preceding command returns RoleName and DiskName properties of all the classic VM disks in the storage account.</span></span> <span data-ttu-id="a1649-214">RoleName esetén a virtuális gép, amely a lemez csatolva van.</span><span class="sxs-lookup"><span data-stu-id="a1649-214">RoleName is the name of the virtual machine to which a disk is attached.</span></span> <span data-ttu-id="a1649-215">Ha az előző parancs visszaadja lemezek majd győződjön meg arról, hogy, amelyhez ezeket a lemezek vannak csatolva hozzá virtuális gépek áttelepítése a tárfiók áttelepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="a1649-215">If preceding command returns disks then ensure that virtual machines to which these disks are attached are migrated before migrating the storage account.</span></span>
    ```powershell
     $storageAccountName = 'yourStorageAccountName'
      Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
      DiskName | Format-List -Property RoleName, DiskName

    ```
* <span data-ttu-id="a1649-216">**A storage-fiókban tárolt választani klasszikus virtuális gépek lemezei törlése**</span><span class="sxs-lookup"><span data-stu-id="a1649-216">**Delete unattached classic VM disks stored in the storage account**</span></span>

    <span data-ttu-id="a1649-217">Keresse meg a tárolóban nem csatolt klasszikus virtuális gépek lemezei fiók használatával a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a1649-217">Find unattached classic VM disks in the storage account using following command:</span></span>

    ```powershell
        $storageAccountName = 'yourStorageAccountName'
        Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Where-Object -Property AttachedTo -EQ $null | Format-List -Property DiskName  

    ```
    <span data-ttu-id="a1649-218">Ha a fenti parancs beolvasása lemezek törölje ezeket a következő parancs használatával lemezeket:</span><span class="sxs-lookup"><span data-stu-id="a1649-218">If above command returns disks then delete these disks using following command:</span></span>

    ```powershell
       Remove-AzureDisk -DiskName 'yourDiskName'
    ```
* <span data-ttu-id="a1649-219">**A storage-fiókban tárolt Virtuálisgép-rendszerképek törlése**</span><span class="sxs-lookup"><span data-stu-id="a1649-219">**Delete VM images stored in the storage account**</span></span>

    <span data-ttu-id="a1649-220">A Virtuálisgép-lemezképek parancs megelőző adja vissza a storage-fiókban tárolt operációsrendszer-lemezzel.</span><span class="sxs-lookup"><span data-stu-id="a1649-220">Preceding command returns all the VM images with OS disk stored in the storage account.</span></span>
     ```powershell
        Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                } | Select-Object -Property ImageName, ImageLabel
     ```
     <span data-ttu-id="a1649-221">A storage-fiókban tárolt adatok lemezzel parancs megelőző adja vissza a Virtuálisgép-lemezképeket.</span><span class="sxs-lookup"><span data-stu-id="a1649-221">Preceding command returns all the VM images with data disks stored in the storage account.</span></span>
     ```powershell

        Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                         -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                        } | Select-Object -Property ImageName, ImageLabel
     ```
    <span data-ttu-id="a1649-222">Törölje az előző parancs használata parancsok fent által visszaadott összes virtuális gép lemezképet:</span><span class="sxs-lookup"><span data-stu-id="a1649-222">Delete all the VM images returned by above commands using preceding command:</span></span>
    ```powershell
    Remove-AzureVMImage -ImageName 'yourImageName'
    ```

<span data-ttu-id="a1649-223">Áttelepítés minden tárfiók érvényesítése a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="a1649-223">Validate each storage account for migration by using the following command.</span></span> <span data-ttu-id="a1649-224">Ebben a példában a tárfiók neve van **myStorageAccount**.</span><span class="sxs-lookup"><span data-stu-id="a1649-224">In this example, the storage account name is **myStorageAccount**.</span></span> <span data-ttu-id="a1649-225">A példa neve cserélje le a saját storage-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="a1649-225">Replace the example name with the name of your own storage account.</span></span>

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Validate -StorageAccountName $storageAccountName
```

<span data-ttu-id="a1649-226">Következő lépés az, hogy a tárfiók Felkészülés az áttelepítésre</span><span class="sxs-lookup"><span data-stu-id="a1649-226">Next step is to Prepare the storage account for migration</span></span>

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

<span data-ttu-id="a1649-227">Ellenőrizze az előkészített tárfiók konfigurációját az Azure PowerShell vagy az Azure portál segítségével.</span><span class="sxs-lookup"><span data-stu-id="a1649-227">Check the configuration for the prepared storage account by using either Azure PowerShell or the Azure portal.</span></span> <span data-ttu-id="a1649-228">Ha nem az áttelepítéshez, és térjen vissza a régi állapot szeretne, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a1649-228">If you are not ready for migration and you want to go back to the old state, use the following command:</span></span>

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

<span data-ttu-id="a1649-229">Az előkészített konfiguráció megfelelőnek tűnik, ha előre, és véglegesíti az erőforrásokat a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="a1649-229">If the prepared configuration looks good, you can move forward and commit the resources by using the following command:</span></span>

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a><span data-ttu-id="a1649-230">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1649-230">Next steps</span></span>
* [<span data-ttu-id="a1649-231">IaaS-erőforrásokra a klasszikus Azure Resource Manager platform által támogatott áttelepítésének áttekintése</span><span class="sxs-lookup"><span data-stu-id="a1649-231">Overview of platform-supported migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="a1649-232">Műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus Azure Resource Managerbe</span><span class="sxs-lookup"><span data-stu-id="a1649-232">Technical deep dive on platform-supported migration from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="a1649-233">Az IaaS-erőforrások klasszikusból Azure Resource Manager-alapú környezetbe való áttelepítésének megtervezése</span><span class="sxs-lookup"><span data-stu-id="a1649-233">Planning for migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="a1649-234">IaaS-erőforrások áttelepítése a klasszikus Azure Resource Manager parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="a1649-234">Use CLI to migrate IaaS resources from classic to Azure Resource Manager</span></span>](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="a1649-235">IaaS-erőforrásokra a klasszikus Azure Resource Manager áttelepítésének védelmével kapcsolatos közösségi eszközök</span><span class="sxs-lookup"><span data-stu-id="a1649-235">Community tools for assisting with migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="a1649-236">A leggyakoribb áttelepítési hibák áttekintése</span><span class="sxs-lookup"><span data-stu-id="a1649-236">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="a1649-237">A leggyakrabban feltett kérdésekre áttelepítése IaaS-erőforrásokra a klasszikus Azure Resource Manager áttekintése</span><span class="sxs-lookup"><span data-stu-id="a1649-237">Review the most frequently asked questions about migrating IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
