---
title: "Az Azure CLI parancsfájl minta - kötegben készletek kezelése |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - kötegben készletek kezelése"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: 2556b02459886390b803407c5cb828687229a44e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-batch-pools-with-azure-cli"></a><span data-ttu-id="45915-103">Az Azure CLI Azure Batch-készletek kezelése</span><span class="sxs-lookup"><span data-stu-id="45915-103">Managing Azure Batch pools with Azure CLI</span></span>

<span data-ttu-id="45915-104">Ezen parancsfájl bemutatja az egyes létrehozásához és az Azure Batch szolgáltatás a számítási csomópontok készleteinek kezelése az Azure parancssori felület elérhető eszközök.</span><span class="sxs-lookup"><span data-stu-id="45915-104">These script demonstrates some of the tools available in the Azure CLI to create and manage pools of compute nodes in the Azure Batch service.</span></span>

> [!NOTE]
> <span data-ttu-id="45915-105">Ez a példa a parancsok létrehozása az Azure virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="45915-105">The commands in this sample create Azure virtual machines.</span></span> <span data-ttu-id="45915-106">Futó virtuális gépek fog keletkeznek költségek a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="45915-106">Running VMs will accrue charges to your account.</span></span> <span data-ttu-id="45915-107">Ezek a költségek minimalizálása érdekében törölje a virtuális gépek Miután befejezte a minta futtatása.</span><span class="sxs-lookup"><span data-stu-id="45915-107">To minimize these charges, delete the VMs once you're done running the sample.</span></span> <span data-ttu-id="45915-108">Lásd: [készletek tisztítása](#clean-up-pools).</span><span class="sxs-lookup"><span data-stu-id="45915-108">See [Clean up pools](#clean-up-pools).</span></span>

<span data-ttu-id="45915-109">Kötegelt készletek a felhő konfigurálása (csak Windows), vagy a virtuálisgép-konfiguráció (Windows és Linux) két módon konfigurálhatók.</span><span class="sxs-lookup"><span data-stu-id="45915-109">Batch pools can be configured in two ways, either with a Cloud Services configuration (Windows only), or a Virtual Machine configuration (Windows and Linux).</span></span> <span data-ttu-id="45915-110">Az alábbi minta parancsfájlok létrehozását mutatják be készletek mindkét konfigurációjú.</span><span class="sxs-lookup"><span data-stu-id="45915-110">The sample scripts below show how to create pools with both configurations.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="45915-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="45915-111">Prerequisites</span></span>

- <span data-ttu-id="45915-112">Telepítse az Azure parancssori felület használatával a utasításokat a [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli), ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="45915-112">Install the Azure CLI using the instructions provided in the [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="45915-113">Batch-fiók létrehozása, ha még nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="45915-113">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="45915-114">Lásd: [Batch-fiók létrehozása az Azure parancssori felülettel](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) számára egy fiókot hoz létre parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="45915-114">See [Create a Batch account with the Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>
- <span data-ttu-id="45915-115">Ha még nem még meg egy kezdő tevékenység-ről futtatva alkalmazások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="45915-115">Configure an application to run from a start task if you haven't yet done so.</span></span> <span data-ttu-id="45915-116">Lásd: [hozzáadása az Azure CLI Azure Batch-alkalmazások](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) egy minta parancsfájlt, amelynek alkalmazást hoz létre, és egy alkalmazáscsomag feltöltését az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="45915-116">See [Adding applications to Azure Batch with Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) for a sample script that creates an application and uploads an application package to Azure.</span></span>

## <a name="pool-with-cloud-service-configuration-sample-script"></a><span data-ttu-id="45915-117">A felhőalapú szolgáltatás konfigurációs mintaparancsfájl készlet</span><span class="sxs-lookup"><span data-stu-id="45915-117">Pool with cloud service configuration sample script</span></span>

<span data-ttu-id="45915-118">[!code-azurecli[fő](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "felhő készletek kezelése")]</span><span class="sxs-lookup"><span data-stu-id="45915-118">[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Cloud Services Pools")]</span></span>

## <a name="pool-with-virtual-machine-configuration-sample-script"></a><span data-ttu-id="45915-119">A virtuális gép konfigurációs mintaparancsfájl készlet</span><span class="sxs-lookup"><span data-stu-id="45915-119">Pool with virtual machine configuration sample script</span></span>

<span data-ttu-id="45915-120">[!code-azurecli[fő](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "virtuálisgép-készletek kezelése")]</span><span class="sxs-lookup"><span data-stu-id="45915-120">[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Virtual Machine Pools")]</span></span>

## <a name="clean-up-pools"></a><span data-ttu-id="45915-121">Készletek tisztítása</span><span class="sxs-lookup"><span data-stu-id="45915-121">Clean up pools</span></span>

<span data-ttu-id="45915-122">Miután lefuttatta a fenti minta parancsfájlt, a következő parancsot a készleteket.</span><span class="sxs-lookup"><span data-stu-id="45915-122">After you run the above sample script, run the following command to delete the pools.</span></span>
```azurecli
az batch pool delete --pool-id mypool-windows
az batch pool delete --pool-id mypool-linux
```

## <a name="script-explanation"></a><span data-ttu-id="45915-123">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="45915-123">Script explanation</span></span>

<span data-ttu-id="45915-124">A parancsfájl a következő parancsok létrehozása és módosítása a Batch-készletek.</span><span class="sxs-lookup"><span data-stu-id="45915-124">This script uses the following commands to create and manipulate Batch pools.</span></span>
<span data-ttu-id="45915-125">Minden egyes parancsa a tábla-parancs-specifikus dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="45915-125">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="45915-126">Parancs</span><span class="sxs-lookup"><span data-stu-id="45915-126">Command</span></span> | <span data-ttu-id="45915-127">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="45915-127">Notes</span></span> |
|---|---|
| [<span data-ttu-id="45915-128">az batch-fiók bejelentkezési</span><span class="sxs-lookup"><span data-stu-id="45915-128">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="45915-129">Batch-fiók hitelesítése.</span><span class="sxs-lookup"><span data-stu-id="45915-129">Authenticate against a Batch account.</span></span>  |
| [<span data-ttu-id="45915-130">az kötegelt alkalmazás összefoglaló listája</span><span class="sxs-lookup"><span data-stu-id="45915-130">az batch application summary list</span></span>](https://docs.microsoft.com/cli/azure/batch/application/summary#list) | <span data-ttu-id="45915-131">A Batch-fiók a rendelkezésre álló alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="45915-131">List the available applications in the Batch account.</span></span>  |
| [<span data-ttu-id="45915-132">az batch-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="45915-132">az batch pool create</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#create) | <span data-ttu-id="45915-133">Hozzon létre virtuális gépek készletét.</span><span class="sxs-lookup"><span data-stu-id="45915-133">Create a pool of VMs.</span></span>  |
| [<span data-ttu-id="45915-134">az kötegelt készlet beállítása</span><span class="sxs-lookup"><span data-stu-id="45915-134">az batch pool set</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#set) | <span data-ttu-id="45915-135">Egy készlet tulajdonságainak frissítésére.</span><span class="sxs-lookup"><span data-stu-id="45915-135">Update properties of a pool.</span></span>  |
| [<span data-ttu-id="45915-136">az kötegelt készlet csomópont-ügynök-SKU listája</span><span class="sxs-lookup"><span data-stu-id="45915-136">az batch pool node-agent-skus list</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#list) | <span data-ttu-id="45915-137">Lista elérhető csomópont ügynök SKU és lemezkép információkat.</span><span class="sxs-lookup"><span data-stu-id="45915-137">List available node agent SKUs and image information.</span></span>  |
| [<span data-ttu-id="45915-138">az kötegelt készlet átméretezése</span><span class="sxs-lookup"><span data-stu-id="45915-138">az batch pool resize</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#resize) | <span data-ttu-id="45915-139">A program átméretezi a futó virtuális gépek a megadott készlet számát.</span><span class="sxs-lookup"><span data-stu-id="45915-139">Resize the number of running VMs in the specified pool.</span></span>  |
| [<span data-ttu-id="45915-140">az kötegelt készlet megjelenítése</span><span class="sxs-lookup"><span data-stu-id="45915-140">az batch pool show</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#show) | <span data-ttu-id="45915-141">Egy készlet tulajdonságok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="45915-141">Display the properties of a pool.</span></span>  |
| [<span data-ttu-id="45915-142">az batch-készlet törlése</span><span class="sxs-lookup"><span data-stu-id="45915-142">az batch pool delete</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#delete) | <span data-ttu-id="45915-143">A megadott készlet törlése.</span><span class="sxs-lookup"><span data-stu-id="45915-143">Delete the specified pool.</span></span>  |
| [<span data-ttu-id="45915-144">az kötegelt készlet automatikus skálázás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="45915-144">az batch pool autoscale enable</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#enable) | <span data-ttu-id="45915-145">Engedélyezze az automatikus skálázás egy címkészlet, és egy képletet.</span><span class="sxs-lookup"><span data-stu-id="45915-145">Enable auto-scaling on a pool and apply a formula.</span></span>  |
| [<span data-ttu-id="45915-146">az kötegelt készlet automatikus skálázás letiltása</span><span class="sxs-lookup"><span data-stu-id="45915-146">az batch pool autoscale disable</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#disable) | <span data-ttu-id="45915-147">Tiltsa le az automatikus skálázást a készlet.</span><span class="sxs-lookup"><span data-stu-id="45915-147">Disable auto-scaling on a pool.</span></span>  |
| [<span data-ttu-id="45915-148">az kötegelt csomópont listája</span><span class="sxs-lookup"><span data-stu-id="45915-148">az batch node list</span></span>](https://docs.microsoft.com/cli/azure/batch/node#list) | <span data-ttu-id="45915-149">A megadott készlet a számítási csomópont listában.</span><span class="sxs-lookup"><span data-stu-id="45915-149">List all the compute node in the specified pool.</span></span>  |
| [<span data-ttu-id="45915-150">az kötegelt csomópont újraindul</span><span class="sxs-lookup"><span data-stu-id="45915-150">az batch node reboot</span></span>](https://docs.microsoft.com/cli/azure/batch/node#reboot) | <span data-ttu-id="45915-151">A megadott számítási csomópont újraindítása.</span><span class="sxs-lookup"><span data-stu-id="45915-151">Reboot the specified compute node.</span></span>  |
| [<span data-ttu-id="45915-152">az batch munkaterület törlése</span><span class="sxs-lookup"><span data-stu-id="45915-152">az batch node delete</span></span>](https://docs.microsoft.com/cli/azure/batch/node#delete) | <span data-ttu-id="45915-153">A felsorolt csomópontok törlése a megadott készlet.</span><span class="sxs-lookup"><span data-stu-id="45915-153">Delete the listed nodes from the specified pool.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="45915-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="45915-154">Next steps</span></span>

<span data-ttu-id="45915-155">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="45915-155">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="45915-156">További kötegelt CLI parancsfájl minták megtalálhatók a [Azure Batch CLI dokumentáció](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="45915-156">Additional Batch CLI script samples can be found in the [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>

