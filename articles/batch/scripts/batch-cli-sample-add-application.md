---
title: "Az Azure CLI parancsfájl minta - alkalmazás hozzáadása a kötegben |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - kötegben alkalmazás hozzáadása"
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
ms.openlocfilehash: 5d057eaf32867aedc95d58c5185e2be1f9385ec0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="adding-applications-to-azure-batch-with-azure-cli"></a><span data-ttu-id="e4dfd-103">Az Azure CLI Azure Batch-alkalmazások hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e4dfd-103">Adding applications to Azure Batch with Azure CLI</span></span>

<span data-ttu-id="e4dfd-104">Ez a parancsfájl bemutatja, hogyan állíthatja be az Azure Batch-készlet vagy a feladat használható alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e4dfd-104">This script demonstrates how to set up an application for use with an Azure Batch pool or task.</span></span> <span data-ttu-id="e4dfd-105">Alkalmazás beállítása a csomag a végrehajtható fájlt, és függőségeit, .zip fájlba.</span><span class="sxs-lookup"><span data-stu-id="e4dfd-105">To set up an application, package your executable, together with any dependencies, into a .zip file.</span></span> <span data-ttu-id="e4dfd-106">Ebben a példában a végrehajtható fájl zip-fájl neve "saját-alkalmazás-exe.zip".</span><span class="sxs-lookup"><span data-stu-id="e4dfd-106">In this example the executable zip file is called 'my-application-exe.zip'.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4dfd-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e4dfd-107">Prerequisites</span></span>

- <span data-ttu-id="e4dfd-108">Telepítse az Azure parancssori felület használatával a utasításokat a [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli), ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="e4dfd-108">Install the Azure CLI using the instructions provided in the [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="e4dfd-109">Batch-fiók létrehozása, ha még nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="e4dfd-109">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="e4dfd-110">Lásd: [Batch-fiók létrehozása az Azure parancssori felülettel](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) számára egy fiókot hoz létre parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="e4dfd-110">See [Create a Batch account with the Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>

## <a name="sample-script"></a><span data-ttu-id="e4dfd-111">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="e4dfd-111">Sample script</span></span>

<span data-ttu-id="e4dfd-112">[!code-azurecli[fő](../../../cli_scripts/batch/add-application/add-application.sh "alkalmazás hozzáadása")]</span><span class="sxs-lookup"><span data-stu-id="e4dfd-112">[!code-azurecli[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]</span></span>

## <a name="clean-up-application"></a><span data-ttu-id="e4dfd-113">Alkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="e4dfd-113">Clean up application</span></span>

<span data-ttu-id="e4dfd-114">Miután lefuttatta a fenti minta parancsfájlt, a következő parancsokat az alkalmazások és a feltöltött alkalmazáscsomagok eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="e4dfd-114">After you run the above sample script, run the following commands to remove the application and all of its uploaded application packages.</span></span>

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a><span data-ttu-id="e4dfd-115">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="e4dfd-115">Script explanation</span></span>

<span data-ttu-id="e4dfd-116">A parancsfájl a következő parancsok töltsön fel egy alkalmazáscsomagot, és hozzon létre egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e4dfd-116">This script uses the following commands to create an application and upload an application package.</span></span>
<span data-ttu-id="e4dfd-117">Minden egyes parancsa a tábla-parancs-specifikus dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="e4dfd-117">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="e4dfd-118">Parancs</span><span class="sxs-lookup"><span data-stu-id="e4dfd-118">Command</span></span> | <span data-ttu-id="e4dfd-119">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="e4dfd-119">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e4dfd-120">az kötegelt alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4dfd-120">az batch application create</span></span>](https://docs.microsoft.com/cli/azure/batch/application#create) | <span data-ttu-id="e4dfd-121">Alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e4dfd-121">Creates an application.</span></span>  |
| [<span data-ttu-id="e4dfd-122">az kötegelt alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="e4dfd-122">az batch application set</span></span>](https://docs.microsoft.com/cli/azure/batch/application#set) | <span data-ttu-id="e4dfd-123">Alkalmazás tulajdonságainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="e4dfd-123">Updates properties of an application.</span></span>  |
| [<span data-ttu-id="e4dfd-124">az kötegelt alkalmazáscsomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4dfd-124">az batch application package create</span></span>](https://docs.microsoft.com/cli/azure/batch/application/package#create) | <span data-ttu-id="e4dfd-125">A megadott alkalmazás ad hozzá egy alkalmazáscsomagot.</span><span class="sxs-lookup"><span data-stu-id="e4dfd-125">Adds an application package to the specified application.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="e4dfd-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4dfd-126">Next steps</span></span>

<span data-ttu-id="e4dfd-127">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e4dfd-127">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="e4dfd-128">További kötegelt CLI parancsfájl minták megtalálhatók a [Azure Batch CLI dokumentáció](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e4dfd-128">Additional Batch CLI script samples can be found in the [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
