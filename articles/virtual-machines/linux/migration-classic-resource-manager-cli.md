---
title: "aaaMigrate virtuális gépek tooResource Manager Azure parancssori felületével |} Microsoft Docs"
description: "Ez a cikk végigvezeti hello platform által támogatott áttelepítési erőforrások klasszikus tooAzure erőforrás-kezelő az Azure parancssori felület használatával"
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
ms.openlocfilehash: 0b11f4bb1b4658b3e88bf7629147ed953b678556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-iaas-resources-from-classic-tooazure-resource-manager-by-using-azure-cli"></a><span data-ttu-id="c1118-103">IaaS-erőforrásokra át klasszikus tooAzure erőforrás-kezelő Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="c1118-103">Migrate IaaS resources from classic tooAzure Resource Manager by using Azure CLI</span></span>
<span data-ttu-id="c1118-104">Ezek a lépések bemutatják, hogyan toouse Azure parancssori felület (CLI) parancsok a szolgáltató (IaaS) erőforrásként hello klasszikus telepítési modell toohello Azure Resource Manager telepítési modellből toomigrate infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="c1118-104">These steps show you how toouse Azure command-line interface (CLI) commands toomigrate infrastructure as a service (IaaS) resources from hello classic deployment model toohello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="c1118-105">hello cikk szükséges hello [Azure CLI](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="c1118-105">hello article requires hello [Azure CLI](../../cli-install-nodejs.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c1118-106">Az itt ismertetett összes hello művelet idempotent.</span><span class="sxs-lookup"><span data-stu-id="c1118-106">All hello operations described here are idempotent.</span></span> <span data-ttu-id="c1118-107">Ha a probléma nem támogatott szolgáltatása vagy konfigurációs hiba, azt javasoljuk, hogy újra hello előkészítése, megszakítása vagy véglegesítése a műveletet.</span><span class="sxs-lookup"><span data-stu-id="c1118-107">If you have a problem other than an unsupported feature or a configuration error, we recommend that you retry hello prepare, abort, or commit operation.</span></span> <span data-ttu-id="c1118-108">hello platform fogják megpróbálni hello művelet újrapróbálása.</span><span class="sxs-lookup"><span data-stu-id="c1118-108">hello platform will then try hello action again.</span></span>
> 
> 

<br>
<span data-ttu-id="c1118-109">Ez a folyamatábra tooidentify hello sorrendet, amelyben lépést kell végrehajtani az áttelepítési folyamat során toobe</span><span class="sxs-lookup"><span data-stu-id="c1118-109">Here is a flowchart tooidentify hello order in which steps need toobe executed during a migration process</span></span>

![Képernyőkép a hello áttelepítés lépései](../windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-prepare-for-migration"></a><span data-ttu-id="c1118-111">1. lépés: Felkészülés az áttelepítésre</span><span class="sxs-lookup"><span data-stu-id="c1118-111">Step 1: Prepare for migration</span></span>
<span data-ttu-id="c1118-112">Az alábbiakban néhány gyakorlati tanácsok, amely értékeli a klasszikus tooResource Manager áttelepítése IaaS-erőforrásokra, javasoljuk:</span><span class="sxs-lookup"><span data-stu-id="c1118-112">Here are a few best practices that we recommend as you evaluate migrating IaaS resources from classic tooResource Manager:</span></span>

* <span data-ttu-id="c1118-113">Olvassa végig hello [listája nem támogatott konfigurációk vagy szolgáltatások](../windows/migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c1118-113">Read through hello [list of unsupported configurations or features](../windows/migration-classic-resource-manager-overview.md).</span></span> <span data-ttu-id="c1118-114">Ha még nem támogatott konfigurációk vagy szolgáltatások használó virtuális gépek, azt javasoljuk, várja meg a hello szolgáltatást vagy a konfigurációs támogatás toobe jelentette be.</span><span class="sxs-lookup"><span data-stu-id="c1118-114">If you have virtual machines that use unsupported configurations or features, we recommend that you wait for hello feature/configuration support toobe announced.</span></span> <span data-ttu-id="c1118-115">Azt is megteheti távolítsa el a szolgáltatást, vagy a konfigurációs tooenable áttelepítést kilépni, ha az igényeknek.</span><span class="sxs-lookup"><span data-stu-id="c1118-115">Alternatively, you can remove that feature or move out of that configuration tooenable migration if it suits your needs.</span></span>
* <span data-ttu-id="c1118-116">Ha olyan parancsfájlok, amelyek központi telepítése az infrastruktúra és az alkalmazások ma rendelkezik automatikus, próbálja toocreate egy hasonló vizsgálat beállítása az áttelepítés ezen parancsfájlok használatával.</span><span class="sxs-lookup"><span data-stu-id="c1118-116">If you have automated scripts that deploy your infrastructure and applications today, try toocreate a similar test setup by using those scripts for migration.</span></span> <span data-ttu-id="c1118-117">Másik lehetőségként állíthat be minta környezetek hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="c1118-117">Alternatively, you can set up sample environments by using hello Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c1118-118">Alkalmazásátjárót jelenleg nem támogatottak az áttelepítést a klasszikus tooResource Manager.</span><span class="sxs-lookup"><span data-stu-id="c1118-118">Application Gateways are not currently supported for migration from classic tooResource Manager.</span></span> <span data-ttu-id="c1118-119">toomigrate Alkalmazásátjáró, klasszikus virtuális hálózat hello átjáró előkészítési művelet toomove hello hálózati futtatása előtt távolítsa el.</span><span class="sxs-lookup"><span data-stu-id="c1118-119">toomigrate a classic virtual network with an Application gateway, remove hello gateway before running a Prepare operation toomove hello network.</span></span> <span data-ttu-id="c1118-120">Hello áttelepítés befejezése után újra hello átjáró az Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c1118-120">After you complete hello migration, reconnect hello gateway in Azure Resource Manager.</span></span> 
>
><span data-ttu-id="c1118-121">Csatlakozás tooExpressRoute Kapcsolatcsoportok egy másik előfizetésben található ExpressRoute-átjárók nem telepíthetők át automatikusan.</span><span class="sxs-lookup"><span data-stu-id="c1118-121">ExpressRoute gateways connecting tooExpressRoute circuits in another subscription cannot be migrated automatically.</span></span> <span data-ttu-id="c1118-122">Ilyen esetekben hello ExpressRoute-átjáró eltávolítása, telepítse át a virtuális hálózati hello, és hozza létre újra a hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="c1118-122">In such cases, remove hello ExpressRoute gateway, migrate hello virtual network and recreate hello gateway.</span></span> <span data-ttu-id="c1118-123">Ellenőrizze a [áttelepítése ExpressRoute áramkörök és társított virtuális hálózatokat hello klasszikus toohello Resource Manager üzembe helyezési modellben](../../expressroute/expressroute-migration-classic-resource-manager.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="c1118-123">Please see [Migrate ExpressRoute circuits and associated virtual networks from hello classic toohello Resource Manager deployment model](../../expressroute/expressroute-migration-classic-resource-manager.md) for more information.</span></span>
> 
> 

## <a name="step-2-set-your-subscription-and-register-hello-provider"></a><span data-ttu-id="c1118-124">2. lépés: Az előfizetés beállítása és hello szolgáltató regisztrálása</span><span class="sxs-lookup"><span data-stu-id="c1118-124">Step 2: Set your subscription and register hello provider</span></span>
<span data-ttu-id="c1118-125">Áttelepítési forgatókönyvek esetén mindkét klasszikus környezet szüksége tooset és erőforrás-kezelő.</span><span class="sxs-lookup"><span data-stu-id="c1118-125">For migration scenarios, you need tooset up your environment for both classic and Resource Manager.</span></span> <span data-ttu-id="c1118-126">[Az Azure parancssori felület telepítése](../../cli-install-nodejs.md) és [jelölje ki az előfizetését](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="c1118-126">[Install Azure CLI](../../cli-install-nodejs.md) and [select your subscription](../../xplat-cli-connect.md).</span></span>

<span data-ttu-id="c1118-127">Bejelentkezési tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="c1118-127">Sign-in tooyour account.</span></span>

    azure login

<span data-ttu-id="c1118-128">Válassza ki a hello Azure-előfizetés hello a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="c1118-128">Select hello Azure subscription by using hello following command.</span></span>

    azure account set "<azure-subscription-name>"

> [!NOTE]
> <span data-ttu-id="c1118-129">Regisztráció az csak egyszer módosítható. lépés:, de igények toobe migrálás megkísérlése előtt egyszer történik.</span><span class="sxs-lookup"><span data-stu-id="c1118-129">Registration is a one time step but it needs toobe done once before attempting migration.</span></span> <span data-ttu-id="c1118-130">Regisztrálása nélkül látni fogja a hibaüzenet a következő hello</span><span class="sxs-lookup"><span data-stu-id="c1118-130">Without registering you'll see hello following error message</span></span> 
> 
> <span data-ttu-id="c1118-131">*BadRequest: Az előfizetés nincs regisztrálva az áttelepítéshez.*</span><span class="sxs-lookup"><span data-stu-id="c1118-131">*BadRequest : Subscription is not registered for migration.*</span></span> 
> 
> 

<span data-ttu-id="c1118-132">Hello áttelepítési erőforrás-szolgáltató regisztrálása hello a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="c1118-132">Register with hello migration resource provider by using hello following command.</span></span> <span data-ttu-id="c1118-133">Vegye figyelembe, hogy néhány esetben ez a parancs végrehajtásának időkorlátja. Hello regisztrációs azonban sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="c1118-133">Note that in some cases, this command times out. However, hello registration will be successful.</span></span>

    azure provider register Microsoft.ClassicInfrastructureMigrate

<span data-ttu-id="c1118-134">Kis türelmet hello regisztrációs toofinish öt percet.</span><span class="sxs-lookup"><span data-stu-id="c1118-134">Please wait five minutes for hello registration toofinish.</span></span> <span data-ttu-id="c1118-135">Hello jóváhagyási hello állapotának hello a következő parancs használatával ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="c1118-135">You can check hello status of hello approval by using hello following command.</span></span> <span data-ttu-id="c1118-136">Győződjön meg arról, hogy van-e RegistrationState `Registered` folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="c1118-136">Make sure that RegistrationState is `Registered` before you proceed.</span></span>

    azure provider show Microsoft.ClassicInfrastructureMigrate

<span data-ttu-id="c1118-137">Most már a CLI toohello kapcsoló `asm` mód.</span><span class="sxs-lookup"><span data-stu-id="c1118-137">Now switch CLI toohello `asm` mode.</span></span>

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-hello-azure-region-of-your-current-deployment-or-vnet"></a><span data-ttu-id="c1118-138">3. lépés: Ellenőrizze, hogy elegendő Azure Resource Manager virtuális gép magok a hello Azure-régió, a jelenlegi üzemelő példány vagy virtuális hálózaton</span><span class="sxs-lookup"><span data-stu-id="c1118-138">Step 3: Make sure you have enough Azure Resource Manager Virtual Machine cores in hello Azure region of your current deployment or VNET</span></span>
<span data-ttu-id="c1118-139">Ebben a lépésben szüksége lesz tooswitch túl`arm` mód.</span><span class="sxs-lookup"><span data-stu-id="c1118-139">For this step you'll need tooswitch too`arm` mode.</span></span> <span data-ttu-id="c1118-140">Ugyanezt a műveletet a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="c1118-140">Do this with hello following command.</span></span>

```
azure config mode arm
```

<span data-ttu-id="c1118-141">A következő parancssori felület parancs toocheck hello aktuális mennyisége az Azure Resource Manager rendelkezik magok hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c1118-141">You can use hello following CLI command toocheck hello current amount of cores you have in Azure Resource Manager.</span></span> <span data-ttu-id="c1118-142">toolearn kvótákat, az alapvető kapcsolatos további információkért lásd: [korlátozásai és hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)</span><span class="sxs-lookup"><span data-stu-id="c1118-142">toolearn more about core quotas, see [Limits and hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)</span></span>

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

<span data-ttu-id="c1118-143">Miután végzett ezzel a lépéssel ellenőrzése, váltson vissza túl`asm` mód.</span><span class="sxs-lookup"><span data-stu-id="c1118-143">Once you're done verifying this step, you can switch back too`asm` mode.</span></span>

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a><span data-ttu-id="c1118-144">4. lépés: 1. lehetőség – a felhőszolgáltatásban található virtuális gépek áttelepítése</span><span class="sxs-lookup"><span data-stu-id="c1118-144">Step 4: Option 1 - Migrate virtual machines in a cloud service</span></span>
<span data-ttu-id="c1118-145">Hello listájának beszerzése felhőszolgáltatások hello a következő parancs használatával, majd mentse hello felhőalapú szolgáltatás, amelyet az toomigrate.</span><span class="sxs-lookup"><span data-stu-id="c1118-145">Get hello list of cloud services by using hello following command, and then pick hello cloud service that you want toomigrate.</span></span> <span data-ttu-id="c1118-146">Vegye figyelembe, hogy ha hello virtuális gépek hello a felhőszolgáltatásban található egy virtuális hálózatot, vagy webes vagy feldolgozói szerepkörök rendelkeznek, elérhetővé válik egy hibaüzenetet.</span><span class="sxs-lookup"><span data-stu-id="c1118-146">Note that if hello VMs in hello cloud service are in a virtual network or if they have web/worker roles, you will get an error message.</span></span>

    azure service list

<span data-ttu-id="c1118-147">Hello hello részletes kimenet tooget hello telepítési parancsnév hello felhőszolgáltatás követően futtassa.</span><span class="sxs-lookup"><span data-stu-id="c1118-147">Run hello following command tooget hello deployment name for hello cloud service from hello verbose output.</span></span> <span data-ttu-id="c1118-148">A legtöbb esetben hello telepítési neve hello ugyanaz, mint a hello felhőszolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="c1118-148">In most cases, hello deployment name is hello same as hello cloud service name.</span></span>

    azure service show <serviceName> -vv

<span data-ttu-id="c1118-149">Először ellenőrzi, hogy hello felhőalapú szolgáltatás a következő parancsok hello segítségével telepíthet át:</span><span class="sxs-lookup"><span data-stu-id="c1118-149">First, validate if you can migrate hello cloud service using hello following commands:</span></span>

```shell
azure service deployment validate-migration <serviceName> <deploymentName> new "" "" ""
```

<span data-ttu-id="c1118-150">Készítse elő a hello virtuális gépek áttelepítésre hello felhőszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="c1118-150">Prepare hello virtual machines in hello cloud service for migration.</span></span> <span data-ttu-id="c1118-151">A két beállítások toochoose rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c1118-151">You have two options toochoose from.</span></span>

<span data-ttu-id="c1118-152">Toomigrate hello virtuális gépek tooa platform által létrehozott virtuális hálózaton, használja a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="c1118-152">If you want toomigrate hello VMs tooa platform-created virtual network, use hello following command.</span></span>

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

<span data-ttu-id="c1118-153">Ha azt szeretné, hogy a meglévő virtuális hálózat hello Resource Manager üzembe helyezési modellel toomigrate tooan, használja a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="c1118-153">If you want toomigrate tooan existing virtual network in hello Resource Manager deployment model, use hello following command.</span></span>

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> <subnetName> <vnetName>

<span data-ttu-id="c1118-154">Hello előkészítése után a művelet sikeres, nézze át a hello részletes kimenet tooget hello áttelepítési állapotának hello virtuális gépeket, és győződjön meg arról, hogy vannak-e a hello `Prepared` állapotát.</span><span class="sxs-lookup"><span data-stu-id="c1118-154">After hello prepare operation is successful, you can look through hello verbose output tooget hello migration state of hello VMs and ensure that they are in hello `Prepared` state.</span></span>

    azure vm show <vmName> -vv

<span data-ttu-id="c1118-155">A parancssori felületen vagy a hello Azure portál segítségével erőforrások előkészített hello hello konfigurációjának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="c1118-155">Check hello configuration for hello prepared resources by using either CLI or hello Azure portal.</span></span> <span data-ttu-id="c1118-156">Ha nem az áttelepítéshez, és azt szeretné, hogy toogo hátsó toohello régi állapot, használja a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="c1118-156">If you are not ready for migration and you want toogo back toohello old state, use hello following command.</span></span>

    azure service deployment abort-migration <serviceName> <deploymentName>

<span data-ttu-id="c1118-157">Ha hello előkészített konfiguráció megfelelőnek tűnik, előre, és véglegesíti hello erőforrások hello a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="c1118-157">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command.</span></span>

    azure service deployment commit-migration <serviceName> <deploymentName>



## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a><span data-ttu-id="c1118-158">4. lépés: 2. lehetőség – a virtuális hálózatban lévő virtuális gépek áttelepítése</span><span class="sxs-lookup"><span data-stu-id="c1118-158">Step 4: Option 2 -  Migrate virtual machines in a virtual network</span></span>
<span data-ttu-id="c1118-159">Mentse hello virtuális hálózatot, hogy szeretné-e toomigrate.</span><span class="sxs-lookup"><span data-stu-id="c1118-159">Pick hello virtual network that you want toomigrate.</span></span> <span data-ttu-id="c1118-160">Vegye figyelembe, hogy ha hello virtuális hálózati webes vagy feldolgozói szerepköröket és a virtuális gépeken nem támogatott konfigurációjú, üzenetet fog kapni egy érvényesítési hiba.</span><span class="sxs-lookup"><span data-stu-id="c1118-160">Note that if hello virtual network contains web/worker roles or VMs with unsupported configurations, you will get a validation error message.</span></span>

<span data-ttu-id="c1118-161">A következő parancs hello segítségével könnyebben nyerhet hello előfizetés összes hello virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="c1118-161">Get all hello virtual networks in hello subscription by using hello following command.</span></span>

    azure network vnet list

<span data-ttu-id="c1118-162">hello kimeneti következőhöz hasonlóan fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="c1118-162">hello output will look something like this:</span></span>

![Képernyőfelvétel a hello parancssori hello teljes virtuális hálózati név kiemelve.](../media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

<span data-ttu-id="c1118-164">A fenti példa hello, hello **virtualNetworkName** hello teljes név **"Csoport classicubuntu16 classicubuntu16"**.</span><span class="sxs-lookup"><span data-stu-id="c1118-164">In hello above example, hello **virtualNetworkName** is hello entire name **"Group classicubuntu16 classicubuntu16"**.</span></span>

<span data-ttu-id="c1118-165">Először ellenőrzi, hogy áttelepítheti a virtuális hálózat hello hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="c1118-165">First, validate if you can migrate hello virtual network using hello following command:</span></span>

```shell
azure network vnet validate-migration <virtualNetworkName>
```

<span data-ttu-id="c1118-166">Virtuális hálózati hello az Ön által választott Felkészülés az áttelepítésre hello a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="c1118-166">Prepare hello virtual network of your choice for migration by using hello following command.</span></span>

    azure network vnet prepare-migration <virtualNetworkName>

<span data-ttu-id="c1118-167">A virtuális gépek operációs parancssori felületen vagy a hello Azure-portál használatával hello hello konfigurációjának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="c1118-167">Check hello configuration for hello prepared virtual machines by using either CLI or hello Azure portal.</span></span> <span data-ttu-id="c1118-168">Ha nem az áttelepítéshez, és azt szeretné, hogy toogo hátsó toohello régi állapot, használja a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="c1118-168">If you are not ready for migration and you want toogo back toohello old state, use hello following command.</span></span>

    azure network vnet abort-migration <virtualNetworkName>

<span data-ttu-id="c1118-169">Ha hello előkészített konfiguráció megfelelőnek tűnik, előre, és véglegesíti hello erőforrások hello a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="c1118-169">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command.</span></span>

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a><span data-ttu-id="c1118-170">5. lépés: A storage-fiókok áttelepítése</span><span class="sxs-lookup"><span data-stu-id="c1118-170">Step 5: Migrate a storage account</span></span>
<span data-ttu-id="c1118-171">Ha elkészült hello virtuális gépeinek áttelepítését, ajánlott hello tárfiók az áttelepítést.</span><span class="sxs-lookup"><span data-stu-id="c1118-171">Once you're done migrating hello virtual machines, we recommend you migrate hello storage account.</span></span>

<span data-ttu-id="c1118-172">Hello tárfiók előkészítése az áttelepítésre hello a következő parancs használatával</span><span class="sxs-lookup"><span data-stu-id="c1118-172">Prepare hello storage account for migration by using hello following command</span></span>

    azure storage account prepare-migration <storageAccountName>

<span data-ttu-id="c1118-173">A tárfiók parancssori felületen vagy a hello Azure portál segítségével előkészített hello hello konfigurációjának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="c1118-173">Check hello configuration for hello prepared storage account by using either CLI or hello Azure portal.</span></span> <span data-ttu-id="c1118-174">Ha nem az áttelepítéshez, és azt szeretné, hogy toogo hátsó toohello régi állapot, használja a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="c1118-174">If you are not ready for migration and you want toogo back toohello old state, use hello following command.</span></span>

    azure storage account abort-migration <storageAccountName>

<span data-ttu-id="c1118-175">Ha hello előkészített konfiguráció megfelelőnek tűnik, előre, és véglegesíti hello erőforrások hello a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="c1118-175">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command.</span></span>

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a><span data-ttu-id="c1118-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c1118-176">Next steps</span></span>

* [<span data-ttu-id="c1118-177">IaaS-erőforrásokra, erőforrás-kezelő klasszikus tooAzure a platform által támogatott áttelepítésének áttekintése</span><span class="sxs-lookup"><span data-stu-id="c1118-177">Overview of platform-supported migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="c1118-178">Műszaki részletes bemutatója a platform által támogatott áttelepítési a klasszikus tooAzure erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="c1118-178">Technical deep dive on platform-supported migration from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="c1118-179">IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének tervezése</span><span class="sxs-lookup"><span data-stu-id="c1118-179">Planning for migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="c1118-180">PowerShell toomigrate IaaS erőforrásainak klasszikus tooAzure erőforrás-kezelő használata</span><span class="sxs-lookup"><span data-stu-id="c1118-180">Use PowerShell toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="c1118-181">IaaS-erőforrásokra, a klasszikus tooAzure erőforrás-kezelő áttelepítésének védelmével kapcsolatos közösségi eszközök</span><span class="sxs-lookup"><span data-stu-id="c1118-181">Community tools for assisting with migration of IaaS resources from classic tooAzure Resource Manager</span></span>](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="c1118-182">A leggyakoribb áttelepítési hibák áttekintése</span><span class="sxs-lookup"><span data-stu-id="c1118-182">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="c1118-183">Felülvizsgálati hello legtöbb kapcsolatos gyakori kérdések a klasszikus tooAzure erőforrás-kezelő áttelepítése IaaS-erőforrásokra</span><span class="sxs-lookup"><span data-stu-id="c1118-183">Review hello most frequently asked questions about migrating IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
