---
title: "aaaAzure CLI parancsfájl minta - készletek kezelése a Batch |} Microsoft Docs"
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
ms.openlocfilehash: 6c9ca9515565aff42752231a080943be8e4c810b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-batch-pools-with-azure-cli"></a><span data-ttu-id="8c5b5-103">Az Azure CLI Azure Batch-készletek kezelése</span><span class="sxs-lookup"><span data-stu-id="8c5b5-103">Managing Azure Batch pools with Azure CLI</span></span>

<span data-ttu-id="8c5b5-104">A parancsfájl bemutatja az egyes hello eszközök hello Azure CLI toocreate érhető el, és hello Azure Batch szolgáltatás a számítási csomópontok készleteinek kezelése.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-104">These script demonstrates some of hello tools available in hello Azure CLI toocreate and manage pools of compute nodes in hello Azure Batch service.</span></span>

> [!NOTE]
> <span data-ttu-id="8c5b5-105">Ez a példa hello parancsok létrehozása az Azure virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-105">hello commands in this sample create Azure virtual machines.</span></span> <span data-ttu-id="8c5b5-106">Futó virtuális gépek fog keletkeznek költségek tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-106">Running VMs will accrue charges tooyour account.</span></span> <span data-ttu-id="8c5b5-107">toominimize ezeket a díjakat hello virtuális gépek törléséhez Miután befejezte a futó hello minta.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-107">toominimize these charges, delete hello VMs once you're done running hello sample.</span></span> <span data-ttu-id="8c5b5-108">Lásd: [készletek tisztítása](#clean-up-pools).</span><span class="sxs-lookup"><span data-stu-id="8c5b5-108">See [Clean up pools](#clean-up-pools).</span></span>

<span data-ttu-id="8c5b5-109">Kötegelt készletek a felhő konfigurálása (csak Windows), vagy a virtuálisgép-konfiguráció (Windows és Linux) két módon konfigurálhatók.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-109">Batch pools can be configured in two ways, either with a Cloud Services configuration (Windows only), or a Virtual Machine configuration (Windows and Linux).</span></span> <span data-ttu-id="8c5b5-110">az alábbi hello Mintaparancsfájlok megjelenítése, hogyan toocreate mindkét konfigurációjú készletbe.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-110">hello sample scripts below show how toocreate pools with both configurations.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8c5b5-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8c5b5-111">Prerequisites</span></span>

- <span data-ttu-id="8c5b5-112">Telepítse az Azure parancssori felület használatával hello hello utasításokat hello [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli), ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-112">Install hello Azure CLI using hello instructions provided in hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="8c5b5-113">Batch-fiók létrehozása, ha még nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-113">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="8c5b5-114">Lásd: [Batch-fiók létrehozása az Azure CLI hello](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) számára egy fiókot hoz létre parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-114">See [Create a Batch account with hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>
- <span data-ttu-id="8c5b5-115">Még nem még fel, ha egy alkalmazás toorun az a kezdő tevékenység konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8c5b5-115">Configure an application toorun from a start task if you haven't yet done so.</span></span> <span data-ttu-id="8c5b5-116">Lásd: [alkalmazások tooAzure kötegelt az Azure parancssori felület Hozzáadás](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) egy minta parancsfájlt, amelynek alkalmazást hoz létre, és egy alkalmazás csomag tooAzure feltöltését.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-116">See [Adding applications tooAzure Batch with Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) for a sample script that creates an application and uploads an application package tooAzure.</span></span>

## <a name="pool-with-cloud-service-configuration-sample-script"></a><span data-ttu-id="8c5b5-117">A felhőalapú szolgáltatás konfigurációs mintaparancsfájl készlet</span><span class="sxs-lookup"><span data-stu-id="8c5b5-117">Pool with cloud service configuration sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Cloud Services Pools")]

## <a name="pool-with-virtual-machine-configuration-sample-script"></a><span data-ttu-id="8c5b5-118">A virtuális gép konfigurációs mintaparancsfájl készlet</span><span class="sxs-lookup"><span data-stu-id="8c5b5-118">Pool with virtual machine configuration sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Virtual Machine Pools")]

## <a name="clean-up-pools"></a><span data-ttu-id="8c5b5-119">Készletek tisztítása</span><span class="sxs-lookup"><span data-stu-id="8c5b5-119">Clean up pools</span></span>

<span data-ttu-id="8c5b5-120">Miután lefuttatta a fenti mintaparancsfájl hello, futtassa a következő parancs toodelete hello készletek hello.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-120">After you run hello above sample script, run hello following command toodelete hello pools.</span></span>
```azurecli
az batch pool delete --pool-id mypool-windows
az batch pool delete --pool-id mypool-linux
```

## <a name="script-explanation"></a><span data-ttu-id="8c5b5-121">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="8c5b5-121">Script explanation</span></span>

<span data-ttu-id="8c5b5-122">Ezt a parancsfájlt használja a következő parancsok toocreate hello és kötegelt készletek kezelését.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-122">This script uses hello following commands toocreate and manipulate Batch pools.</span></span>
<span data-ttu-id="8c5b5-123">Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-123">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="8c5b5-124">Parancs</span><span class="sxs-lookup"><span data-stu-id="8c5b5-124">Command</span></span> | <span data-ttu-id="8c5b5-125">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="8c5b5-125">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8c5b5-126">az batch-fiók bejelentkezési</span><span class="sxs-lookup"><span data-stu-id="8c5b5-126">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="8c5b5-127">Batch-fiók hitelesítése.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-127">Authenticate against a Batch account.</span></span>  |
| [<span data-ttu-id="8c5b5-128">az kötegelt alkalmazás összefoglaló listája</span><span class="sxs-lookup"><span data-stu-id="8c5b5-128">az batch application summary list</span></span>](https://docs.microsoft.com/cli/azure/batch/application/summary#list) | <span data-ttu-id="8c5b5-129">A Batch-fiók hello hello a rendelkezésre álló alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-129">List hello available applications in hello Batch account.</span></span>  |
| [<span data-ttu-id="8c5b5-130">az batch-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c5b5-130">az batch pool create</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#create) | <span data-ttu-id="8c5b5-131">Hozzon létre virtuális gépek készletét.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-131">Create a pool of VMs.</span></span>  |
| [<span data-ttu-id="8c5b5-132">az kötegelt készlet beállítása</span><span class="sxs-lookup"><span data-stu-id="8c5b5-132">az batch pool set</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#set) | <span data-ttu-id="8c5b5-133">Egy készlet tulajdonságainak frissítésére.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-133">Update properties of a pool.</span></span>  |
| [<span data-ttu-id="8c5b5-134">az kötegelt készlet csomópont-ügynök-SKU listája</span><span class="sxs-lookup"><span data-stu-id="8c5b5-134">az batch pool node-agent-skus list</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/node-agent-skus#list) | <span data-ttu-id="8c5b5-135">Lista elérhető csomópont ügynök SKU és lemezkép információkat.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-135">List available node agent SKUs and image information.</span></span>  |
| [<span data-ttu-id="8c5b5-136">az kötegelt készlet átméretezése</span><span class="sxs-lookup"><span data-stu-id="8c5b5-136">az batch pool resize</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#resize) | <span data-ttu-id="8c5b5-137">A megadott alkalmazáskészlet átméretezési hello száma a futó virtuális gépek hello.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-137">Resize hello number of running VMs in hello specified pool.</span></span>  |
| [<span data-ttu-id="8c5b5-138">az kötegelt készlet megjelenítése</span><span class="sxs-lookup"><span data-stu-id="8c5b5-138">az batch pool show</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#show) | <span data-ttu-id="8c5b5-139">Egy készlet hello tulajdonságok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-139">Display hello properties of a pool.</span></span>  |
| [<span data-ttu-id="8c5b5-140">az batch-készlet törlése</span><span class="sxs-lookup"><span data-stu-id="8c5b5-140">az batch pool delete</span></span>](https://docs.microsoft.com/cli/azure/batch/pool#delete) | <span data-ttu-id="8c5b5-141">Törölje a megadott hello készlet.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-141">Delete hello specified pool.</span></span>  |
| [<span data-ttu-id="8c5b5-142">az kötegelt készlet automatikus skálázás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8c5b5-142">az batch pool autoscale enable</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#enable) | <span data-ttu-id="8c5b5-143">Engedélyezze az automatikus skálázás egy címkészlet, és egy képletet.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-143">Enable auto-scaling on a pool and apply a formula.</span></span>  |
| [<span data-ttu-id="8c5b5-144">az kötegelt készlet automatikus skálázás letiltása</span><span class="sxs-lookup"><span data-stu-id="8c5b5-144">az batch pool autoscale disable</span></span>](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#disable) | <span data-ttu-id="8c5b5-145">Tiltsa le az automatikus skálázást a készlet.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-145">Disable auto-scaling on a pool.</span></span>  |
| [<span data-ttu-id="8c5b5-146">az kötegelt csomópont listája</span><span class="sxs-lookup"><span data-stu-id="8c5b5-146">az batch node list</span></span>](https://docs.microsoft.com/cli/azure/batch/node#list) | <span data-ttu-id="8c5b5-147">Listázza az összes hello számítási csomópont hello lévő megadott készlet.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-147">List all hello compute node in hello specified pool.</span></span>  |
| [<span data-ttu-id="8c5b5-148">az kötegelt csomópont újraindul</span><span class="sxs-lookup"><span data-stu-id="8c5b5-148">az batch node reboot</span></span>](https://docs.microsoft.com/cli/azure/batch/node#reboot) | <span data-ttu-id="8c5b5-149">Hello megadott számítási csomópont újraindítása.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-149">Reboot hello specified compute node.</span></span>  |
| [<span data-ttu-id="8c5b5-150">az batch munkaterület törlése</span><span class="sxs-lookup"><span data-stu-id="8c5b5-150">az batch node delete</span></span>](https://docs.microsoft.com/cli/azure/batch/node#delete) | <span data-ttu-id="8c5b5-151">Törlés felsorolt hello csomópontok hello a megadott készlet.</span><span class="sxs-lookup"><span data-stu-id="8c5b5-151">Delete hello listed nodes from hello specified pool.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="8c5b5-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8c5b5-152">Next steps</span></span>

<span data-ttu-id="8c5b5-153">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8c5b5-153">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="8c5b5-154">További kötegelt CLI parancsfájl minták hello található [Azure Batch CLI dokumentáció](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8c5b5-154">Additional Batch CLI script samples can be found in hello [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>

