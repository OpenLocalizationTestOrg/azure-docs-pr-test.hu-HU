---
title: "aaaAzure CLI parancsfájl minta - egy függvény-alkalmazás létrehozása az App Service-csomagot |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - egy függvény-alkalmazás létrehozása az App Service-csomagot"
services: functions
documentationcenter: functions
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 04/11/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: c0ffbbbf022e5680e5ae3141e784e7c7bced0bc0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-in-an-app-service-plan"></a><span data-ttu-id="16ecc-103">Egy függvény-alkalmazás létrehozása az App Service-csomagot</span><span class="sxs-lookup"><span data-stu-id="16ecc-103">Create a Function App in an App Service plan</span></span>

<span data-ttu-id="16ecc-104">Ez a parancsfájlpélda hoz létre egy Azure függvény alkalmazást, mert a függvények tárolója.</span><span class="sxs-lookup"><span data-stu-id="16ecc-104">This sample script creates an Azure Function App, which is a container for your functions.</span></span> <span data-ttu-id="16ecc-105">a kijelölt App Service-csomag, ami azt jelenti, hogy a kiszolgáló erőforrásainak mindig szerepelnek. hello függvény App létre.</span><span class="sxs-lookup"><span data-stu-id="16ecc-105">hello Function App is created using a dedicated App Service plan, which means your server resources are always on.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="16ecc-106">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="16ecc-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="16ecc-107">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="16ecc-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="16ecc-108">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="16ecc-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="16ecc-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="16ecc-109">Sample script</span></span>

<span data-ttu-id="16ecc-110">Ezt a parancsfájlt hoz létre az Azure-függvény alkalmazást egy dedikált [App Service-csomag](../functions-scale.md#app-service-plan).</span><span class="sxs-lookup"><span data-stu-id="16ecc-110">This script creates an Azure Function app using a dedicated [App Service plan](../functions-scale.md#app-service-plan).</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-app-service-plan/create-function-app-app-service-plan.sh "Create an Azure Function on an App Service plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="16ecc-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="16ecc-111">Script explanation</span></span>

<span data-ttu-id="16ecc-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="16ecc-112">Each command in hello table links toocommand specific documentation.</span></span> <span data-ttu-id="16ecc-113">A parancsfájl a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="16ecc-113">This script uses hello following commands:</span></span>

| <span data-ttu-id="16ecc-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="16ecc-114">Command</span></span> | <span data-ttu-id="16ecc-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="16ecc-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="16ecc-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="16ecc-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="16ecc-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="16ecc-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="16ecc-118">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="16ecc-118">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="16ecc-119">Létrehoz egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="16ecc-119">Creates an Azure Storage account.</span></span> |
| [<span data-ttu-id="16ecc-120">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="16ecc-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appserviceplan#create) | <span data-ttu-id="16ecc-121">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="16ecc-121">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="16ecc-122">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="16ecc-122">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#delete) | <span data-ttu-id="16ecc-123">Létrehoz egy Azure függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="16ecc-123">Creates an Azure Function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="16ecc-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="16ecc-124">Next steps</span></span>

<span data-ttu-id="16ecc-125">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="16ecc-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="16ecc-126">További Azure Functions CLI parancsfájl minták hello található [dokumentáció az Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="16ecc-126">Additional Azure Functions CLI script samples can be found in hello [Azure Functions documentation](../functions-cli-samples.md).</span></span>
