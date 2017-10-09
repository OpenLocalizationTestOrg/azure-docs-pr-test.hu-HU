---
title: "parancssori felület parancsfájl minta - fut egy feladat kötegelt aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: f73a7cb341e550fd1c92f92201e20b27fa20d238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="running-jobs-on-azure-batch-with-azure-cli"></a><span data-ttu-id="80754-103">Azure Batch Azure parancssori felülettel futó feladatok</span><span class="sxs-lookup"><span data-stu-id="80754-103">Running jobs on Azure Batch with Azure CLI</span></span>

<span data-ttu-id="80754-104">Ezt a parancsfájlt hoz létre egy kötegelt, és hozzáadja a feladatok toohello feladat sorozata.</span><span class="sxs-lookup"><span data-stu-id="80754-104">This script creates a Batch job and adds a series of tasks toohello job.</span></span> <span data-ttu-id="80754-105">Azt is bemutatja, hogyan toomonitor egy feladatot, és a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="80754-105">It also demonstrates how toomonitor a job and its tasks.</span></span> <span data-ttu-id="80754-106">Végül azt illusztrálja, hogyan tooquery hello hatékonyan hello feladat feladatokkal kapcsolatos adatokat a Batch szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="80754-106">Finally, it shows how tooquery hello Batch service efficiently for information about hello job's tasks.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="80754-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="80754-107">Prerequisites</span></span>

- <span data-ttu-id="80754-108">Telepítse az Azure parancssori felület használatával hello hello utasításokat hello [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli), ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="80754-108">Install hello Azure CLI using hello instructions provided in hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="80754-109">Batch-fiók létrehozása, ha még nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="80754-109">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="80754-110">Lásd: [Batch-fiók létrehozása az Azure CLI hello](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) számára egy fiókot hoz létre parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="80754-110">See [Create a Batch account with hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>
- <span data-ttu-id="80754-111">Még nem még fel, ha egy alkalmazás toorun az a kezdő tevékenység konfigurálása</span><span class="sxs-lookup"><span data-stu-id="80754-111">Configure an application toorun from a start task if you haven't yet done so.</span></span> <span data-ttu-id="80754-112">Lásd: [alkalmazások tooAzure kötegelt az Azure parancssori felület Hozzáadás](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) egy minta parancsfájlt, amelynek alkalmazást hoz létre, és egy alkalmazás csomag tooAzure feltöltését.</span><span class="sxs-lookup"><span data-stu-id="80754-112">See [Adding applications tooAzure Batch with Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-add-application) for a sample script that creates an application and uploads an application package tooAzure.</span></span>
- <span data-ttu-id="80754-113">Konfigurálja a címkészletet, mely hello feladat futni fog.</span><span class="sxs-lookup"><span data-stu-id="80754-113">Configure a pool on which hello job will run.</span></span> <span data-ttu-id="80754-114">Lásd: [kezelése Azure Batch készletek Azure parancssori felülettel](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) egy parancsfájlt hoz létre a készlet egy felhőalapú szolgáltatás konfigurációja vagy a virtuálisgép-konfiguráció számára.</span><span class="sxs-lookup"><span data-stu-id="80754-114">See [Managing Azure Batch pools with Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-sample-manage-pool) for a sample script that creates a pool with either a Cloud Service Configuration or a Virtual Machine Configuration.</span></span>

## <a name="sample-script"></a><span data-ttu-id="80754-115">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="80754-115">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]

## <a name="clean-up-job"></a><span data-ttu-id="80754-116">Feladat tisztítása</span><span class="sxs-lookup"><span data-stu-id="80754-116">Clean up job</span></span>

<span data-ttu-id="80754-117">Miután lefuttatta a fenti mintaparancsfájl hello, futtassa a következő parancs tooremove hello a feladatot, és a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="80754-117">After you run hello above sample script, run hello following command tooremove the job and all of its tasks.</span></span> <span data-ttu-id="80754-118">Vegye figyelembe, hogy hello készlet toobe külön-külön törölni kell.</span><span class="sxs-lookup"><span data-stu-id="80754-118">Note that hello pool will need toobe deleted separately.</span></span> <span data-ttu-id="80754-119">Lásd: [kezelése Azure Batch készletek Azure parancssori felülettel](./batch-cli-sample-manage-pool.md) létrehozása és törlése készletek olvashat.</span><span class="sxs-lookup"><span data-stu-id="80754-119">See [Managing Azure Batch pools with Azure CLI](./batch-cli-sample-manage-pool.md) for more information on creating and deleting pools.</span></span>

```azurecli
az batch job delete --job-id myjob
```

## <a name="script-explanation"></a><span data-ttu-id="80754-120">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="80754-120">Script explanation</span></span>

<span data-ttu-id="80754-121">A parancsfájl a következő parancsok toocreate hello a kötegelt és a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="80754-121">This script uses hello following commands toocreate a Batch job and tasks.</span></span> <span data-ttu-id="80754-122">Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="80754-122">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="80754-123">Parancs</span><span class="sxs-lookup"><span data-stu-id="80754-123">Command</span></span> | <span data-ttu-id="80754-124">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="80754-124">Notes</span></span> |
|---|---|
| [<span data-ttu-id="80754-125">az batch-fiók bejelentkezési</span><span class="sxs-lookup"><span data-stu-id="80754-125">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="80754-126">Batch-fiók hitelesítése.</span><span class="sxs-lookup"><span data-stu-id="80754-126">Authenticate against a Batch account.</span></span>  |
| [<span data-ttu-id="80754-127">hozza létre az kötegelt</span><span class="sxs-lookup"><span data-stu-id="80754-127">az batch job create</span></span>](https://docs.microsoft.com/cli/azure/batch/job#create) | <span data-ttu-id="80754-128">Létrehoz egy kötegelt feladatot.</span><span class="sxs-lookup"><span data-stu-id="80754-128">Creates a Batch job.</span></span>  |
| [<span data-ttu-id="80754-129">az kötegelt feladat beállítása</span><span class="sxs-lookup"><span data-stu-id="80754-129">az batch job set</span></span>](https://docs.microsoft.com/cli/azure/batch/job#set) | <span data-ttu-id="80754-130">A kötegelt frissítések tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="80754-130">Updates properties of a Batch job.</span></span>  |
| [<span data-ttu-id="80754-131">az kötegelt feladat megjelenítése</span><span class="sxs-lookup"><span data-stu-id="80754-131">az batch job show</span></span>](https://docs.microsoft.com/cli/azure/batch/job#show) | <span data-ttu-id="80754-132">Lekérdezi a megadott kötegelt részleteit.</span><span class="sxs-lookup"><span data-stu-id="80754-132">Retrieves details of a specified Batch job.</span></span>  |
| [<span data-ttu-id="80754-133">az kötegelt feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="80754-133">az batch task create</span></span>](https://docs.microsoft.com/cli/azure/batch/task#create) | <span data-ttu-id="80754-134">Egy feladat toohello megadott kötegelt hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="80754-134">Adds a task toohello specified Batch job.</span></span>  |
| [<span data-ttu-id="80754-135">az kötegelt feladat megjelenítése</span><span class="sxs-lookup"><span data-stu-id="80754-135">az batch task show</span></span>](https://docs.microsoft.com/cli/azure/batch/task#show) | <span data-ttu-id="80754-136">Egy feladat hello lekéri hello részleteit megadott kötegelt.</span><span class="sxs-lookup"><span data-stu-id="80754-136">Retrieves hello details of a task from hello specified Batch job.</span></span>  |
| [<span data-ttu-id="80754-137">az kötegelt tevékenység listája</span><span class="sxs-lookup"><span data-stu-id="80754-137">az batch task list</span></span>](https://docs.microsoft.com/cli/azure/batch/task#list) | <span data-ttu-id="80754-138">Hello megadott feladathoz hozzárendelt hello feladatokat foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="80754-138">Lists hello tasks associated with hello specified job.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="80754-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80754-139">Next steps</span></span>

<span data-ttu-id="80754-140">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="80754-140">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="80754-141">További kötegelt CLI parancsfájl minták hello található [Azure Batch CLI dokumentáció](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="80754-141">Additional Batch CLI script samples can be found in hello [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
