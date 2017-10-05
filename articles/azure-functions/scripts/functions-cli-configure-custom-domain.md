---
title: "Az Azure CLI parancsfájl minta - leképezés funkció alkalmazásokhoz az egyéni tartomány |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - leképezés funkció alkalmazásokhoz az Azure-ban egyéni tartományt."
services: functions
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 6fcea6d32f9dd25b0fafb4f895f60d8320ac9df8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="map-a-custom-domain-to-a-function-app"></a><span data-ttu-id="a5029-103">Egyéni tartomány leképezése egy függvény alkalmazást</span><span class="sxs-lookup"><span data-stu-id="a5029-103">Map a custom domain to a function app</span></span>

<span data-ttu-id="a5029-104">Ez a parancsfájlpélda kapcsolódó erőforrások egy függvény alkalmazást hoz létre, és majd leképezi `www.<yourdomain>` rá.</span><span class="sxs-lookup"><span data-stu-id="a5029-104">This sample script creates a function app with related resources, and then maps `www.<yourdomain>` to it.</span></span> <span data-ttu-id="a5029-105">Hozzárendelése egyéni tartományhoz, a függvény app léteznie kell az App Service-csomagot, és nem egy fogyasztás tervben.</span><span class="sxs-lookup"><span data-stu-id="a5029-105">To map to a custom domain, your function app must be created in an App Service plan and not in a consumption plan.</span></span> <span data-ttu-id="a5029-106">Az Azure Functions csak akkor támogatja a leképezés az A rekord egyéni tartományt.</span><span class="sxs-lookup"><span data-stu-id="a5029-106">Azure Functions only supports mapping a custom domain using an A record.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a5029-107">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="a5029-107">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="a5029-108">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="a5029-108">Run `az --version` to find the version.</span></span> <span data-ttu-id="a5029-109">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a5029-109">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="a5029-110">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="a5029-110">Sample script</span></span>

<span data-ttu-id="a5029-111">[!code-azurecli-interactive[fő](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "egy egyéni tartomány leképezése egy függvény alkalmazást")]</span><span class="sxs-lookup"><span data-stu-id="a5029-111">[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a function app")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="a5029-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="a5029-112">Script explanation</span></span>

<span data-ttu-id="a5029-113">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="a5029-113">This script uses the following commands.</span></span> <span data-ttu-id="a5029-114">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="a5029-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="a5029-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="a5029-115">Command</span></span> | <span data-ttu-id="a5029-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="a5029-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a5029-117">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="a5029-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="a5029-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a5029-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a5029-119">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="a5029-119">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="a5029-120">Létrehoz egy tárfiókot, a függvény alkalmazás által igényelt.</span><span class="sxs-lookup"><span data-stu-id="a5029-120">Creates a storage account required by the function app.</span></span> |
| [<span data-ttu-id="a5029-121">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="a5029-121">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="a5029-122">Az App Service-csomag kell rendelni egy egyéni tartományt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a5029-122">Creates an App Service plan required to map a custom domain.</span></span> |
| [<span data-ttu-id="a5029-123">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="a5029-123">az functionapp create</span></span>]() | <span data-ttu-id="a5029-124">Létrehoz egy függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a5029-124">Creates a function app.</span></span> |
| [<span data-ttu-id="a5029-125">az App Service web config állomásnév hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a5029-125">az appservice web config hostname add</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | <span data-ttu-id="a5029-126">Egy függvény app rendeli hozzá egyéni tartományt.</span><span class="sxs-lookup"><span data-stu-id="a5029-126">Maps a custom domain to a function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a5029-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a5029-127">Next steps</span></span>

<span data-ttu-id="a5029-128">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a5029-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="a5029-129">További funkciók CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure Functions]().</span><span class="sxs-lookup"><span data-stu-id="a5029-129">Additional Functions CLI script samples can be found in the [Azure Functions documentation]().</span></span>
