---
title: "Telepítse át a virtuális gépek a Resource Manager Azure parancssori felületével |} Microsoft Docs"
description: "Ez a cikk végigvezeti a platform által támogatott áttelepítési erőforrások a klasszikus Azure Resource Manager Azure parancssori felület használatával"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d6f5a877-05b6-4127-a545-3f5bede4e479
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: a63d758570b09b37b8e51c639267f729521d9ae0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a><span data-ttu-id="bf947-103">Telepítse át IaaS-erőforrásokra a klasszikus Azure Resource Manager Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="bf947-103">Migrate IaaS resources from classic to Azure Resource Manager by using Azure CLI</span></span>
<span data-ttu-id="bf947-104">Ezeket a lépéseket mutatja be az Azure parancssori felület (CLI) parancsok használatával telepítse át az infrastruktúra erőforrásként egy szolgáltatási (IaaS) a klasszikus telepítési modellből az Azure Resource Manager telepítési modellhez.</span><span class="sxs-lookup"><span data-stu-id="bf947-104">These steps show you how to use Azure command-line interface (CLI) commands to migrate infrastructure as a service (IaaS) resources from the classic deployment model to the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="bf947-105">A cikk igényel a [Azure CLI](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="bf947-105">The article requires the [Azure CLI](../../cli-install-nodejs.md).</span></span>

> [!NOTE]
> <span data-ttu-id="bf947-106">Itt leírt műveletek idempotent.</span><span class="sxs-lookup"><span data-stu-id="bf947-106">All the operations described here are idempotent.</span></span> <span data-ttu-id="bf947-107">Ha a probléma nem támogatott szolgáltatása vagy konfigurációs hiba, azt javasoljuk, hogy a prepare újra, vagy megszakításra műveletet.</span><span class="sxs-lookup"><span data-stu-id="bf947-107">If you have a problem other than an unsupported feature or a configuration error, we recommend that you retry the prepare, abort, or commit operation.</span></span> <span data-ttu-id="bf947-108">A platform fog majd próbálja megismételni a műveletet.</span><span class="sxs-lookup"><span data-stu-id="bf947-108">The platform will then try the action again.</span></span>
> 
> 

<br>
<span data-ttu-id="bf947-109">Ez a sorrendet, amelyben lépéseket kell végrehajtani az áttelepítési folyamat során azonosításához folyamatábra</span><span class="sxs-lookup"><span data-stu-id="bf947-109">Here is a flowchart to identify the order in which steps need to be executed during a migration process</span></span>

![Képernyőkép a migrálási lépésekről](../windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-prepare-for-migration"></a><span data-ttu-id="bf947-111">1. lépés: Felkészülés az áttelepítésre</span><span class="sxs-lookup"><span data-stu-id="bf947-111">Step 1: Prepare for migration</span></span>
<span data-ttu-id="bf947-112">Az alábbiakban néhány gyakorlati tanácsok, azt javasoljuk, áttelepítése IaaS-erőforrásokra a hagyományos erőforrás-kezelő értékeli:</span><span class="sxs-lookup"><span data-stu-id="bf947-112">Here are a few best practices that we recommend as you evaluate migrating IaaS resources from classic to Resource Manager:</span></span>

* <span data-ttu-id="bf947-113">Olvassa végig a [listája nem támogatott konfigurációk vagy szolgáltatások](../windows/migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bf947-113">Read through the [list of unsupported configurations or features](../windows/migration-classic-resource-manager-overview.md).</span></span> <span data-ttu-id="bf947-114">Ha még nem támogatott konfigurációk vagy szolgáltatások használó virtuális gépek, azt javasoljuk, hogy a szolgáltatás vagy a konfigurációs támogatási bejelentések várja.</span><span class="sxs-lookup"><span data-stu-id="bf947-114">If you have virtual machines that use unsupported configurations or features, we recommend that you wait for the feature/configuration support to be announced.</span></span> <span data-ttu-id="bf947-115">Alternatív megoldásként távolítsa el a szolgáltatást, vagy helyezze át kívül, hogy a konfigurálás engedélyezze az áttelepítést, ha az igényeinek megfelelő.</span><span class="sxs-lookup"><span data-stu-id="bf947-115">Alternatively, you can remove that feature or move out of that configuration to enable migration if it suits your needs.</span></span>
* <span data-ttu-id="bf947-116">Ha olyan parancsfájlok, amelyek központi telepítése az infrastruktúra és az alkalmazások ma rendelkezik automatikus, hozzon létre egy hasonló vizsgálat beállítása az áttelepítés ezen parancsfájlok használatával.</span><span class="sxs-lookup"><span data-stu-id="bf947-116">If you have automated scripts that deploy your infrastructure and applications today, try to create a similar test setup by using those scripts for migration.</span></span> <span data-ttu-id="bf947-117">Másik lehetőségként állíthat be minta környezetekben az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="bf947-117">Alternatively, you can set up sample environments by using the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bf947-118">Alkalmazásátjárót jelenleg nem támogatottak az áttelepítéshez a klasszikus az erőforrás-kezelő.</span><span class="sxs-lookup"><span data-stu-id="bf947-118">Application Gateways are not currently supported for migration from classic to Resource Manager.</span></span> <span data-ttu-id="bf947-119">A klasszikus virtuális hálózatot az Alkalmazásátjáró át, a hálózati áthelyezése egy előkészítési művelet futtatása előtt távolítsa el az átjáró.</span><span class="sxs-lookup"><span data-stu-id="bf947-119">To migrate a classic virtual network with an Application gateway, remove the gateway before running a Prepare operation to move the network.</span></span> <span data-ttu-id="bf947-120">Az áttelepítés befejezése után csatlakoztassa újra az átjárót az Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="bf947-120">After you complete the migration, reconnect the gateway in Azure Resource Manager.</span></span> 
>
><span data-ttu-id="bf947-121">Kapcsolódás egy másik előfizetésben található ExpressRoute-Kapcsolatcsoportok ExpressRoute-átjárók nem telepíthetők át automatikusan.</span><span class="sxs-lookup"><span data-stu-id="bf947-121">ExpressRoute gateways connecting to ExpressRoute circuits in another subscription cannot be migrated automatically.</span></span> <span data-ttu-id="bf947-122">Ebben az esetben távolítsa el az ExpressRoute-átjárót, telepítse át a virtuális hálózaton, és hozza létre újra az átjárót.</span><span class="sxs-lookup"><span data-stu-id="bf947-122">In such cases, remove the ExpressRoute gateway, migrate the virtual network and recreate the gateway.</span></span> <span data-ttu-id="bf947-123">Ellenőrizze a [áttelepítése ExpressRoute áramkörök, és a Resource Manager üzembe helyezési modellel klasszikus virtuális hálózatok társított](../../expressroute/expressroute-migration-classic-resource-manager.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="bf947-123">Please see [Migrate ExpressRoute circuits and associated virtual networks from the classic to the Resource Manager deployment model](../../expressroute/expressroute-migration-classic-resource-manager.md) for more information.</span></span>
> 
> 

## <a name="step-2-set-your-subscription-and-register-the-provider"></a><span data-ttu-id="bf947-124">2. lépés: Állítsa be az előfizetéshez, és regisztrálja a szolgáltatót</span><span class="sxs-lookup"><span data-stu-id="bf947-124">Step 2: Set your subscription and register the provider</span></span>
<span data-ttu-id="bf947-125">Áttelepítési forgatókönyvek esetén, be kell állítania a környezetet az mindkét klasszikus és Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="bf947-125">For migration scenarios, you need to set up your environment for both classic and Resource Manager.</span></span> <span data-ttu-id="bf947-126">[Az Azure parancssori felület telepítése](../../cli-install-nodejs.md) és [jelölje ki az előfizetését](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="bf947-126">[Install Azure CLI](../../cli-install-nodejs.md) and [select your subscription](../../xplat-cli-connect.md).</span></span>

<span data-ttu-id="bf947-127">Jelentkezzen be fiókjába.</span><span class="sxs-lookup"><span data-stu-id="bf947-127">Sign-in to your account.</span></span>

    azure login

<span data-ttu-id="bf947-128">Válassza ki az Azure-előfizetés a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="bf947-128">Select the Azure subscription by using the following command.</span></span>

    azure account set "<azure-subscription-name>"

> [!NOTE]
> <span data-ttu-id="bf947-129">A regisztráció egy az idő a lépést, de az azért van szükség, egyszer a migrálás megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="bf947-129">Registration is a one time step but it needs to be done once before attempting migration.</span></span> <span data-ttu-id="bf947-130">Ha nem regisztrálja látni fogja, a következő hibaüzenet</span><span class="sxs-lookup"><span data-stu-id="bf947-130">Without registering you'll see the following error message</span></span> 
> 
> <span data-ttu-id="bf947-131">*BadRequest: Az előfizetés nincs regisztrálva az áttelepítéshez.*</span><span class="sxs-lookup"><span data-stu-id="bf947-131">*BadRequest : Subscription is not registered for migration.*</span></span> 
> 
> 

<span data-ttu-id="bf947-132">Az áttelepítés erőforrás-szolgáltató regisztrálása a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="bf947-132">Register with the migration resource provider by using the following command.</span></span> <span data-ttu-id="bf947-133">Vegye figyelembe, hogy néhány esetben ez a parancs végrehajtásának időkorlátja.</span><span class="sxs-lookup"><span data-stu-id="bf947-133">Note that in some cases, this command times out.</span></span> <span data-ttu-id="bf947-134">Azonban a regisztráció sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="bf947-134">However, the registration will be successful.</span></span>

    azure provider register Microsoft.ClassicInfrastructureMigrate

<span data-ttu-id="bf947-135">Kis türelmet, a regisztráció befejezéséhez öt perc.</span><span class="sxs-lookup"><span data-stu-id="bf947-135">Please wait five minutes for the registration to finish.</span></span> <span data-ttu-id="bf947-136">A jóváhagyási állapotát a következő paranccsal ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="bf947-136">You can check the status of the approval by using the following command.</span></span> <span data-ttu-id="bf947-137">Győződjön meg arról, hogy van-e RegistrationState `Registered` folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="bf947-137">Make sure that RegistrationState is `Registered` before you proceed.</span></span>

    azure provider show Microsoft.ClassicInfrastructureMigrate

<span data-ttu-id="bf947-138">Most a parancssori felület a kapcsoló a `asm` mód.</span><span class="sxs-lookup"><span data-stu-id="bf947-138">Now switch CLI to the `asm` mode.</span></span>

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a><span data-ttu-id="bf947-139">3. lépés: Ellenőrizze, hogy elegendő Azure Resource Manager virtuális gép magok Azure-régióban a jelenlegi üzemelő példány vagy virtuális hálózaton</span><span class="sxs-lookup"><span data-stu-id="bf947-139">Step 3: Make sure you have enough Azure Resource Manager Virtual Machine cores in the Azure region of your current deployment or VNET</span></span>
<span data-ttu-id="bf947-140">Ebben a lépésben kell váltani `arm` mód.</span><span class="sxs-lookup"><span data-stu-id="bf947-140">For this step you'll need to switch to `arm` mode.</span></span> <span data-ttu-id="bf947-141">Ehhez a következő paranccsal.</span><span class="sxs-lookup"><span data-stu-id="bf947-141">Do this with the following command.</span></span>

```
azure config mode arm
```

<span data-ttu-id="bf947-142">A következő parancssori parancsot segítségével mag, hogy az Azure Resource Manager aktuális mennyiségének ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="bf947-142">You can use the following CLI command to check the current amount of cores you have in Azure Resource Manager.</span></span> <span data-ttu-id="bf947-143">Core kvóták kapcsolatos további információkért lásd: [korlátozásai és az Azure erőforrás-kezelő](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)</span><span class="sxs-lookup"><span data-stu-id="bf947-143">To learn more about core quotas, see [Limits and the Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)</span></span>

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

<span data-ttu-id="bf947-144">Miután befejezte ellenőrzése ezt a lépést, is visszavált `asm` mód.</span><span class="sxs-lookup"><span data-stu-id="bf947-144">Once you're done verifying this step, you can switch back to `asm` mode.</span></span>

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a><span data-ttu-id="bf947-145">4. lépés: 1. lehetőség – a felhőszolgáltatásban található virtuális gépek áttelepítése</span><span class="sxs-lookup"><span data-stu-id="bf947-145">Step 4: Option 1 - Migrate virtual machines in a cloud service</span></span>
<span data-ttu-id="bf947-146">A felhőszolgáltatások listájának lekérdezése a következő paranccsal, és ezután válassza ki a felhőalapú szolgáltatás, amely az áttelepíteni kívánt.</span><span class="sxs-lookup"><span data-stu-id="bf947-146">Get the list of cloud services by using the following command, and then pick the cloud service that you want to migrate.</span></span> <span data-ttu-id="bf947-147">Vegye figyelembe, hogy a beállítást, ha a felhőszolgáltatás a virtuális gépek virtuális hálózatban, vagy ha webes vagy feldolgozói szerepköröket, hibaüzenetet kap-e.</span><span class="sxs-lookup"><span data-stu-id="bf947-147">Note that if the VMs in the cloud service are in a virtual network or if they have web/worker roles, you will get an error message.</span></span>

    azure service list

<span data-ttu-id="bf947-148">A következő parancsot a központi telepítés nevét, a felhőszolgáltatás lekérése a részletes kimenet.</span><span class="sxs-lookup"><span data-stu-id="bf947-148">Run the following command to get the deployment name for the cloud service from the verbose output.</span></span> <span data-ttu-id="bf947-149">A legtöbb esetben a telepítés neve megegyezik a felhőszolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="bf947-149">In most cases, the deployment name is the same as the cloud service name.</span></span>

    azure service show <serviceName> -vv

<span data-ttu-id="bf947-150">Először ellenőrzi, hogy áttelepítheti a felhőalapú szolgáltatás, a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="bf947-150">First, validate if you can migrate the cloud service using the following commands:</span></span>

```shell
azure service deployment validate-migration <serviceName> <deploymentName> new "" "" ""
```

<span data-ttu-id="bf947-151">Készítse elő a virtuális gépek áttelepítése a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="bf947-151">Prepare the virtual machines in the cloud service for migration.</span></span> <span data-ttu-id="bf947-152">Rendelkezik két lehetőség közül választhat.</span><span class="sxs-lookup"><span data-stu-id="bf947-152">You have two options to choose from.</span></span>

<span data-ttu-id="bf947-153">Ha szeretne áttelepíteni a virtuális gépek platform által létrehozott virtuális hálózathoz, a következő paranccsal.</span><span class="sxs-lookup"><span data-stu-id="bf947-153">If you want to migrate the VMs to a platform-created virtual network, use the following command.</span></span>

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

<span data-ttu-id="bf947-154">Ha szeretné áttelepíteni meglévő virtuális hálózat a Resource Manager üzembe helyezési modellel, az alábbi paranccsal.</span><span class="sxs-lookup"><span data-stu-id="bf947-154">If you want to migrate to an existing virtual network in the Resource Manager deployment model, use the following command.</span></span>

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> <subnetName> <vnetName>

<span data-ttu-id="bf947-155">Az előkészítési művelet befejezését követően a virtuális gépek áttelepítésének állapotát, és győződjön meg arról, hogy vannak-e a részletes kimenet keresztül megtekintheti a `Prepared` állapotát.</span><span class="sxs-lookup"><span data-stu-id="bf947-155">After the prepare operation is successful, you can look through the verbose output to get the migration state of the VMs and ensure that they are in the `Prepared` state.</span></span>

    azure vm show <vmName> -vv

<span data-ttu-id="bf947-156">Ellenőrizze a konfigurációt, az előkészített erőforrások CLI vagy az Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="bf947-156">Check the configuration for the prepared resources by using either CLI or the Azure portal.</span></span> <span data-ttu-id="bf947-157">Ha nem az áttelepítéshez, és térjen vissza a régi állapot szeretne, használja a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="bf947-157">If you are not ready for migration and you want to go back to the old state, use the following command.</span></span>

    azure service deployment abort-migration <serviceName> <deploymentName>

<span data-ttu-id="bf947-158">Az előkészített konfiguráció megfelelőnek tűnik, ha előre, és véglegesíti az erőforrásokat a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="bf947-158">If the prepared configuration looks good, you can move forward and commit the resources by using the following command.</span></span>

    azure service deployment commit-migration <serviceName> <deploymentName>



## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a><span data-ttu-id="bf947-159">4. lépés: 2. lehetőség – a virtuális hálózatban lévő virtuális gépek áttelepítése</span><span class="sxs-lookup"><span data-stu-id="bf947-159">Step 4: Option 2 -  Migrate virtual machines in a virtual network</span></span>
<span data-ttu-id="bf947-160">Válassza ki az áttelepíteni kívánt virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="bf947-160">Pick the virtual network that you want to migrate.</span></span> <span data-ttu-id="bf947-161">Vegye figyelembe, hogy ha a virtuális hálózat nem támogatott konfigurációjú webes vagy feldolgozói szerepkörök vagy a virtuális gépeket tartalmaz, üzenetet fog kapni egy érvényesítési hiba.</span><span class="sxs-lookup"><span data-stu-id="bf947-161">Note that if the virtual network contains web/worker roles or VMs with unsupported configurations, you will get a validation error message.</span></span>

<span data-ttu-id="bf947-162">Az alábbi parancs segítségével könnyebben nyerhet a virtuális hálózatok az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="bf947-162">Get all the virtual networks in the subscription by using the following command.</span></span>

    azure network vnet list

<span data-ttu-id="bf947-163">A kimenet ehhez hasonló fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="bf947-163">The output will look something like this:</span></span>

![Képernyőkép a kiemelt teljes virtuális hálózat nevét a parancssorban.](../media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

<span data-ttu-id="bf947-165">A fenti példában a **virtualNetworkName** teljes neve **"Csoport classicubuntu16 classicubuntu16"**.</span><span class="sxs-lookup"><span data-stu-id="bf947-165">In the above example, the **virtualNetworkName** is the entire name **"Group classicubuntu16 classicubuntu16"**.</span></span>

<span data-ttu-id="bf947-166">Először ellenőrzi, hogy áttelepítheti a virtuális hálózat, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="bf947-166">First, validate if you can migrate the virtual network using the following command:</span></span>

```shell
azure network vnet validate-migration <virtualNetworkName>
```

<span data-ttu-id="bf947-167">Az Ön által választott virtuális hálózatot Felkészülés az áttelepítésre az alábbi paranccsal.</span><span class="sxs-lookup"><span data-stu-id="bf947-167">Prepare the virtual network of your choice for migration by using the following command.</span></span>

    azure network vnet prepare-migration <virtualNetworkName>

<span data-ttu-id="bf947-168">Ellenőrizze az előkészített virtuális gépek konfigurációját a CLI vagy az Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="bf947-168">Check the configuration for the prepared virtual machines by using either CLI or the Azure portal.</span></span> <span data-ttu-id="bf947-169">Ha nem az áttelepítéshez, és térjen vissza a régi állapot szeretne, használja a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="bf947-169">If you are not ready for migration and you want to go back to the old state, use the following command.</span></span>

    azure network vnet abort-migration <virtualNetworkName>

<span data-ttu-id="bf947-170">Az előkészített konfiguráció megfelelőnek tűnik, ha előre, és véglegesíti az erőforrásokat a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="bf947-170">If the prepared configuration looks good, you can move forward and commit the resources by using the following command.</span></span>

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a><span data-ttu-id="bf947-171">5. lépés: A storage-fiókok áttelepítése</span><span class="sxs-lookup"><span data-stu-id="bf947-171">Step 5: Migrate a storage account</span></span>
<span data-ttu-id="bf947-172">Miután befejezte a virtuális gépek áttelepítéséhez, azt javasoljuk, telepíti át a tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="bf947-172">Once you're done migrating the virtual machines, we recommend you migrate the storage account.</span></span>

<span data-ttu-id="bf947-173">A tárfiók előkészítése az áttelepítésre az alábbi parancs használatával</span><span class="sxs-lookup"><span data-stu-id="bf947-173">Prepare the storage account for migration by using the following command</span></span>

    azure storage account prepare-migration <storageAccountName>

<span data-ttu-id="bf947-174">Ellenőrizze a konfigurációt, az előkészített tárfiók CLI vagy az Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="bf947-174">Check the configuration for the prepared storage account by using either CLI or the Azure portal.</span></span> <span data-ttu-id="bf947-175">Ha nem az áttelepítéshez, és térjen vissza a régi állapot szeretne, használja a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="bf947-175">If you are not ready for migration and you want to go back to the old state, use the following command.</span></span>

    azure storage account abort-migration <storageAccountName>

<span data-ttu-id="bf947-176">Az előkészített konfiguráció megfelelőnek tűnik, ha előre, és véglegesíti az erőforrásokat a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="bf947-176">If the prepared configuration looks good, you can move forward and commit the resources by using the following command.</span></span>

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a><span data-ttu-id="bf947-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bf947-177">Next steps</span></span>

* [<span data-ttu-id="bf947-178">IaaS-erőforrásokra a klasszikus Azure Resource Manager platform által támogatott áttelepítésének áttekintése</span><span class="sxs-lookup"><span data-stu-id="bf947-178">Overview of platform-supported migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="bf947-179">Műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus Azure Resource Managerbe</span><span class="sxs-lookup"><span data-stu-id="bf947-179">Technical deep dive on platform-supported migration from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="bf947-180">Az IaaS-erőforrások klasszikusból Azure Resource Manager-alapú környezetbe való áttelepítésének megtervezése</span><span class="sxs-lookup"><span data-stu-id="bf947-180">Planning for migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="bf947-181">IaaS-erőforrások áttelepítése a klasszikus Azure Resource Manager PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="bf947-181">Use PowerShell to migrate IaaS resources from classic to Azure Resource Manager</span></span>](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="bf947-182">IaaS-erőforrásokra a klasszikus Azure Resource Manager áttelepítésének védelmével kapcsolatos közösségi eszközök</span><span class="sxs-lookup"><span data-stu-id="bf947-182">Community tools for assisting with migration of IaaS resources from classic to Azure Resource Manager</span></span>](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="bf947-183">A leggyakoribb áttelepítési hibák áttekintése</span><span class="sxs-lookup"><span data-stu-id="bf947-183">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="bf947-184">A leggyakrabban feltett kérdésekre áttelepítése IaaS-erőforrásokra a klasszikus Azure Resource Manager áttekintése</span><span class="sxs-lookup"><span data-stu-id="bf947-184">Review the most frequently asked questions about migrating IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
