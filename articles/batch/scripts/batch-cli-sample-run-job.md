---
title: "Az Azure CLI parancsfájl minta - fut egy feladat kötegelt |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - kötegelt feladat fut"
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
ms.openlocfilehash: 5fe1e3595d9459e60b2fd54d6f17f6822731f453
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="running-jobs-on-azure-batch-with-azure-cli"></a><span data-ttu-id="1e10f-103">Azure Batch Azure parancssori felülettel futó feladatok</span><span class="sxs-lookup"><span data-stu-id="1e10f-103">Running jobs on Azure Batch with Azure CLI</span></span>

<span data-ttu-id="1e10f-104">Ezt a parancsfájlt hoz létre egy kötegelt és feladatok sorozata ad hozzá a feladatot.</span><span class="sxs-lookup"><span data-stu-id="1e10f-104">This script creates a Batch job and adds a series of tasks to the job.</span></span> <span data-ttu-id="1e10f-105">Azt is bemutatja, hogyan kell egy feladat és a feladatok figyelése.</span><span class="sxs-lookup"><span data-stu-id="1e10f-105">It also demonstrates how to monitor a job and its tasks.</span></span> <span data-ttu-id="1e10f-106">Végül azt jeleníti meg a Batch szolgáltatás hatékonyan kapcsolatos feladatokról további információk a feladat lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="1e10f-106">Finally, it shows how to query the Batch service efficiently for information about the job's tasks.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e10f-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1e10f-107">Prerequisites</span></span>

- <span data-ttu-id="1e10f-108">Telepítse az Azure parancssori felület használatával a utasításokat a [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli), ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="1e10f-108">Install the Azure CLI using the instructions provided in the [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="1e10f-109">Batch-fiók létrehozása, ha még nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1e10f-109">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="1e10f-110">Lásd: [Batch-fiók létrehozása az Azure parancssori felülettel](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) számára egy fiókot hoz létre parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="1e10f-110">See [Create a Batch account with the Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>
- <span data-ttu-id="1e10f-111">Ha még nem még meg egy kezdő tevékenység-ről futtatva alkalmazások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="1e10f-111">Configure an application to run from a start task if you haven't yet done so.</span></span> <span data-ttu-id="1e10f-112">Lásd: [hozzáadása az Azure CLI Azure Batch-alkalmazások](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) egy minta parancsfájlt, amelynek alkalmazást hoz létre, és egy alkalmazáscsomag feltöltését az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="1e10f-112">See [Adding applications to Azure Batch with Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) for a sample script that creates an application and uploads an application package to Azure.</span></span>
- <span data-ttu-id="1e10f-113">Konfigurálja a készletben, amelyen a feladat futni fog.</span><span class="sxs-lookup"><span data-stu-id="1e10f-113">Configure a pool on which the job will run.</span></span> <span data-ttu-id="1e10f-114">Lásd: [kezelése Azure Batch készletek Azure parancssori felülettel](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) egy parancsfájlt hoz létre a készlet egy felhőalapú szolgáltatás konfigurációja vagy a virtuálisgép-konfiguráció számára.</span><span class="sxs-lookup"><span data-stu-id="1e10f-114">See [Managing Azure Batch pools with Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) for a sample script that creates a pool with either a Cloud Service Configuration or a Virtual Machine Configuration.</span></span>

## <a name="sample-script"></a><span data-ttu-id="1e10f-115">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="1e10f-115">Sample script</span></span>

<span data-ttu-id="1e10f-116">[!code-azurecli[fő](../../../cli_scripts/batch/run-job/run-job.sh "feladat futtatása")]</span><span class="sxs-lookup"><span data-stu-id="1e10f-116">[!code-azurecli[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]</span></span>

## <a name="clean-up-job"></a><span data-ttu-id="1e10f-117">Feladat tisztítása</span><span class="sxs-lookup"><span data-stu-id="1e10f-117">Clean up job</span></span>

<span data-ttu-id="1e10f-118">Miután lefuttatta a fenti minta parancsfájlt, a következő paranccsal távolítsa el a feladatot, és a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="1e10f-118">After you run the above sample script, run the following command to remove the job and all of its tasks.</span></span> <span data-ttu-id="1e10f-119">Vegye figyelembe, hogy a készlet külön törölni kell.</span><span class="sxs-lookup"><span data-stu-id="1e10f-119">Note that the pool will need to be deleted separately.</span></span> <span data-ttu-id="1e10f-120">Lásd: [kezelése Azure Batch készletek Azure parancssori felülettel](./batch-cli-sample-manage-pool.md) létrehozása és törlése készletek olvashat.</span><span class="sxs-lookup"><span data-stu-id="1e10f-120">See [Managing Azure Batch pools with Azure CLI](./batch-cli-sample-manage-pool.md) for more information on creating and deleting pools.</span></span>

```azurecli
az batch job delete --job-id myjob
```

## <a name="script-explanation"></a><span data-ttu-id="1e10f-121">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="1e10f-121">Script explanation</span></span>

<span data-ttu-id="1e10f-122">A parancsfájl a következő parancsokat egy kötegelt és a feladatok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1e10f-122">This script uses the following commands to create a Batch job and tasks.</span></span> <span data-ttu-id="1e10f-123">Minden egyes parancsa a tábla-parancs-specifikus dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="1e10f-123">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="1e10f-124">Parancs</span><span class="sxs-lookup"><span data-stu-id="1e10f-124">Command</span></span> | <span data-ttu-id="1e10f-125">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="1e10f-125">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1e10f-126">az batch-fiók bejelentkezési</span><span class="sxs-lookup"><span data-stu-id="1e10f-126">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="1e10f-127">Batch-fiók hitelesítése.</span><span class="sxs-lookup"><span data-stu-id="1e10f-127">Authenticate against a Batch account.</span></span>  |
| [<span data-ttu-id="1e10f-128">hozza létre az kötegelt</span><span class="sxs-lookup"><span data-stu-id="1e10f-128">az batch job create</span></span>](https://docs.microsoft.com/cli/azure/batch/job#create) | <span data-ttu-id="1e10f-129">Létrehoz egy kötegelt feladatot.</span><span class="sxs-lookup"><span data-stu-id="1e10f-129">Creates a Batch job.</span></span>  |
| [<span data-ttu-id="1e10f-130">az kötegelt feladat beállítása</span><span class="sxs-lookup"><span data-stu-id="1e10f-130">az batch job set</span></span>](https://docs.microsoft.com/cli/azure/batch/job#set) | <span data-ttu-id="1e10f-131">A kötegelt frissítések tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="1e10f-131">Updates properties of a Batch job.</span></span>  |
| [<span data-ttu-id="1e10f-132">az kötegelt feladat megjelenítése</span><span class="sxs-lookup"><span data-stu-id="1e10f-132">az batch job show</span></span>](https://docs.microsoft.com/cli/azure/batch/job#show) | <span data-ttu-id="1e10f-133">Lekérdezi a megadott kötegelt részleteit.</span><span class="sxs-lookup"><span data-stu-id="1e10f-133">Retrieves details of a specified Batch job.</span></span>  |
| [<span data-ttu-id="1e10f-134">az kötegelt feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e10f-134">az batch task create</span></span>](https://docs.microsoft.com/cli/azure/batch/task#create) | <span data-ttu-id="1e10f-135">A megadott kötegelt ad hozzá egy feladatot.</span><span class="sxs-lookup"><span data-stu-id="1e10f-135">Adds a task to the specified Batch job.</span></span>  |
| [<span data-ttu-id="1e10f-136">az kötegelt feladat megjelenítése</span><span class="sxs-lookup"><span data-stu-id="1e10f-136">az batch task show</span></span>](https://docs.microsoft.com/cli/azure/batch/task#show) | <span data-ttu-id="1e10f-137">Lekérdezi a megadott kötegelt feladat részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="1e10f-137">Retrieves the details of a task from the specified Batch job.</span></span>  |
| [<span data-ttu-id="1e10f-138">az kötegelt tevékenység listája</span><span class="sxs-lookup"><span data-stu-id="1e10f-138">az batch task list</span></span>](https://docs.microsoft.com/cli/azure/batch/task#list) | <span data-ttu-id="1e10f-139">A megadott feladathoz tartozó feladatokat sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="1e10f-139">Lists the tasks associated with the specified job.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="1e10f-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1e10f-140">Next steps</span></span>

<span data-ttu-id="1e10f-141">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1e10f-141">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="1e10f-142">További kötegelt CLI parancsfájl minták megtalálhatók a [Azure Batch CLI dokumentáció](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="1e10f-142">Additional Batch CLI script samples can be found in the [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
