---
title: "Az Azure CLI-parancsfájlt minta - egy függvény-alkalmazás létrehozása az App Service-csomagot |} Microsoft Docs"
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
ms.openlocfilehash: 40c3fa6fa6c07d59e4bf55531e116ba50aa92b91
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-function-app-in-an-app-service-plan"></a><span data-ttu-id="51c0b-103">Egy függvény-alkalmazás létrehozása az App Service-csomagot</span><span class="sxs-lookup"><span data-stu-id="51c0b-103">Create a Function App in an App Service plan</span></span>

<span data-ttu-id="51c0b-104">Ez a parancsfájlpélda hoz létre egy Azure függvény alkalmazást, mert a függvények tárolója.</span><span class="sxs-lookup"><span data-stu-id="51c0b-104">This sample script creates an Azure Function App, which is a container for your functions.</span></span> <span data-ttu-id="51c0b-105">A kijelölt App Service-csomag, ami azt jelenti, hogy a kiszolgáló erőforrásainak mindig szerepelnek. a függvény App létre.</span><span class="sxs-lookup"><span data-stu-id="51c0b-105">The Function App is created using a dedicated App Service plan, which means your server resources are always on.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="51c0b-106">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="51c0b-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="51c0b-107">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="51c0b-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="51c0b-108">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="51c0b-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="51c0b-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="51c0b-109">Sample script</span></span>

<span data-ttu-id="51c0b-110">Ezt a parancsfájlt hoz létre az Azure-függvény alkalmazást egy dedikált [App Service-csomag](../functions-scale.md#app-service-plan).</span><span class="sxs-lookup"><span data-stu-id="51c0b-110">This script creates an Azure Function app using a dedicated [App Service plan](../functions-scale.md#app-service-plan).</span></span>

<span data-ttu-id="51c0b-111">[!code-azurecli-interactive[fő](../../../cli_scripts/azure-functions/create-function-app-app-service-plan/create-function-app-app-service-plan.sh "az App Service-csomagot az Azure-függvény létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="51c0b-111">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-app-service-plan/create-function-app-app-service-plan.sh "Create an Azure Function on an App Service plan")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="51c0b-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="51c0b-112">Script explanation</span></span>

<span data-ttu-id="51c0b-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="51c0b-113">Each command in the table links to command specific documentation.</span></span> <span data-ttu-id="51c0b-114">Ezt a parancsfájlt az alábbi parancsokat használja:</span><span class="sxs-lookup"><span data-stu-id="51c0b-114">This script uses the following commands:</span></span>

| <span data-ttu-id="51c0b-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="51c0b-115">Command</span></span> | <span data-ttu-id="51c0b-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="51c0b-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="51c0b-117">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="51c0b-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="51c0b-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="51c0b-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="51c0b-119">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="51c0b-119">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="51c0b-120">Létrehoz egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="51c0b-120">Creates an Azure Storage account.</span></span> |
| [<span data-ttu-id="51c0b-121">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="51c0b-121">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appserviceplan#create) | <span data-ttu-id="51c0b-122">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="51c0b-122">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="51c0b-123">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="51c0b-123">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#delete) | <span data-ttu-id="51c0b-124">Létrehoz egy Azure függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="51c0b-124">Creates an Azure Function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="51c0b-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51c0b-125">Next steps</span></span>

<span data-ttu-id="51c0b-126">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="51c0b-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="51c0b-127">További Azure Functions CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="51c0b-127">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>
