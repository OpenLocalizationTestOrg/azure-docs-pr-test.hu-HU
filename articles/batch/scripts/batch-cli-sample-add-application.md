---
title: "aaaAzure CLI parancsfájl minta - alkalmazás hozzáadása a kötegben |} Microsoft Docs"
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
ms.openlocfilehash: cb33b3a7b30610011b19954a987995cc5f0257c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="adding-applications-tooazure-batch-with-azure-cli"></a><span data-ttu-id="cd0e5-103">Az Azure parancssori felület kötegben. alkalmazásokat tooAzure hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cd0e5-103">Adding applications tooAzure Batch with Azure CLI</span></span>

<span data-ttu-id="cd0e5-104">Ez a parancsfájl bemutatja, hogyan tooset az alkalmazáshoz egy Azure Batch-készlet vagy feladat való használatra.</span><span class="sxs-lookup"><span data-stu-id="cd0e5-104">This script demonstrates how tooset up an application for use with an Azure Batch pool or task.</span></span> <span data-ttu-id="cd0e5-105">az alkalmazás tooset csomag a végrehajtható fájlt, és függőségeit, .zip fájlba.</span><span class="sxs-lookup"><span data-stu-id="cd0e5-105">tooset up an application, package your executable, together with any dependencies, into a .zip file.</span></span> <span data-ttu-id="cd0e5-106">A példa hello végrehajtható zip a fájl neve "saját-alkalmazás-exe.zip".</span><span class="sxs-lookup"><span data-stu-id="cd0e5-106">In this example hello executable zip file is called 'my-application-exe.zip'.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd0e5-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cd0e5-107">Prerequisites</span></span>

- <span data-ttu-id="cd0e5-108">Telepítse az Azure parancssori felület használatával hello hello utasításokat hello [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli), ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="cd0e5-108">Install hello Azure CLI using hello instructions provided in hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="cd0e5-109">Batch-fiók létrehozása, ha még nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="cd0e5-109">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="cd0e5-110">Lásd: [Batch-fiók létrehozása az Azure CLI hello](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) számára egy fiókot hoz létre parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="cd0e5-110">See [Create a Batch account with hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>

## <a name="sample-script"></a><span data-ttu-id="cd0e5-111">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="cd0e5-111">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-application"></a><span data-ttu-id="cd0e5-112">Alkalmazás törlése</span><span class="sxs-lookup"><span data-stu-id="cd0e5-112">Clean up application</span></span>

<span data-ttu-id="cd0e5-113">Miután lefuttatta a fenti mintaparancsfájl hello, futtassa a következő parancsok tooremove hello az alkalmazások és a feltöltött alkalmazás csomagokat.</span><span class="sxs-lookup"><span data-stu-id="cd0e5-113">After you run hello above sample script, run hello following commands tooremove the application and all of its uploaded application packages.</span></span>

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a><span data-ttu-id="cd0e5-114">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="cd0e5-114">Script explanation</span></span>

<span data-ttu-id="cd0e5-115">A parancsfájl a következő parancsok toocreate hello, az alkalmazás és az alkalmazáscsomag feltöltése.</span><span class="sxs-lookup"><span data-stu-id="cd0e5-115">This script uses hello following commands toocreate an application and upload an application package.</span></span>
<span data-ttu-id="cd0e5-116">Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="cd0e5-116">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="cd0e5-117">Parancs</span><span class="sxs-lookup"><span data-stu-id="cd0e5-117">Command</span></span> | <span data-ttu-id="cd0e5-118">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="cd0e5-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cd0e5-119">az kötegelt alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd0e5-119">az batch application create</span></span>](https://docs.microsoft.com/cli/azure/batch/application#create) | <span data-ttu-id="cd0e5-120">Alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="cd0e5-120">Creates an application.</span></span>  |
| [<span data-ttu-id="cd0e5-121">az kötegelt alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="cd0e5-121">az batch application set</span></span>](https://docs.microsoft.com/cli/azure/batch/application#set) | <span data-ttu-id="cd0e5-122">Alkalmazás tulajdonságainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="cd0e5-122">Updates properties of an application.</span></span>  |
| [<span data-ttu-id="cd0e5-123">az kötegelt alkalmazáscsomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd0e5-123">az batch application package create</span></span>](https://docs.microsoft.com/cli/azure/batch/application/package#create) | <span data-ttu-id="cd0e5-124">Hozzáad egy alkalmazás csomag toohello megadott alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cd0e5-124">Adds an application package toohello specified application.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="cd0e5-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cd0e5-125">Next steps</span></span>

<span data-ttu-id="cd0e5-126">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cd0e5-126">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="cd0e5-127">További kötegelt CLI parancsfájl minták hello található [Azure Batch CLI dokumentáció](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="cd0e5-127">Additional Batch CLI script samples can be found in hello [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
