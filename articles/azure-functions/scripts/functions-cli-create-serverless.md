---
title: "Az Azure CLI-parancsfájlt minták – a kiszolgáló nélküli végrehajtáshoz függvény-alkalmazás létrehozása |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – a kiszolgáló nélküli végrehajtáshoz függvény-alkalmazás létrehozása"
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
ms.openlocfilehash: 37046967bd5ab0d797d1c66690db7200fb4805e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-function-app-for-serverless-execution"></a><span data-ttu-id="c862f-103">A kiszolgáló nélküli végrehajtáshoz függvény-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c862f-103">Create a Function App for serverless execution</span></span>

<span data-ttu-id="c862f-104">Ez a parancsfájlpélda hoz létre egy Azure függvény alkalmazást, mert a függvények tárolója.</span><span class="sxs-lookup"><span data-stu-id="c862f-104">This sample script creates an Azure Function App, which is a container for your functions.</span></span> <span data-ttu-id="c862f-105">A függvény App létre kell hozni a [fogyasztás terv](../functions-scale.md#consumption-plan), ami ideális eseményvezérelt kiszolgáló nélküli munkaterheléseket.</span><span class="sxs-lookup"><span data-stu-id="c862f-105">The Function App is created using the [consumption plan](../functions-scale.md#consumption-plan), which is ideal for event-driven serverless workloads.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c862f-106">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="c862f-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="c862f-107">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="c862f-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="c862f-108">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c862f-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="c862f-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="c862f-109">Sample script</span></span>

<span data-ttu-id="c862f-110">Ezt a parancsfájlt hoz létre egy Azure-függvény alkalmazás használata a [fogyasztás terv](../functions-scale.md#consumption-plan).</span><span class="sxs-lookup"><span data-stu-id="c862f-110">This script creates an Azure Function app using the [consumption plan](../functions-scale.md#consumption-plan).</span></span>

<span data-ttu-id="c862f-111">[!code-azurecli-interactive[fő](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "fogyasztás tervezze egy Azure-függvény létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="c862f-111">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "Create an Azure Function on a consumption plan")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="c862f-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="c862f-112">Script explanation</span></span>

<span data-ttu-id="c862f-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="c862f-113">Each command in the table links to command specific documentation.</span></span> <span data-ttu-id="c862f-114">Ezt a parancsfájlt az alábbi parancsokat használja:</span><span class="sxs-lookup"><span data-stu-id="c862f-114">This script uses the following commands:</span></span>

| <span data-ttu-id="c862f-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="c862f-115">Command</span></span> | <span data-ttu-id="c862f-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c862f-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c862f-117">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="c862f-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="c862f-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c862f-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c862f-119">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c862f-119">az storage account create</span></span>](/cli/azure/storage/account#create) | <span data-ttu-id="c862f-120">Létrehoz egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="c862f-120">Creates an Azure Storage account.</span></span> |
| [<span data-ttu-id="c862f-121">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="c862f-121">az functionapp create</span></span>](https://docs.microsoft.com/cli/azure/functionapp#create) | <span data-ttu-id="c862f-122">Létrehoz egy Azure-függvényt.</span><span class="sxs-lookup"><span data-stu-id="c862f-122">Creates an Azure Function.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c862f-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c862f-123">Next steps</span></span>

<span data-ttu-id="c862f-124">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c862f-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c862f-125">További Azure Functions CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure Functions](../functions-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c862f-125">Additional Azure Functions CLI script samples can be found in the [Azure Functions documentation](../functions-cli-samples.md).</span></span>
