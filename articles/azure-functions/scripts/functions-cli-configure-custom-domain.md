---
title: "aaaAzure CLI parancsfájl minta - leképezés egy egyéni tartomány tooa függvény app |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - leképezés egy egyéni tartomány tooa függvény alkalmazást az Azure-ban."
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
ms.openlocfilehash: c7cb0a3e132b491250623b945aecf6aea4f57c4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="map-a-custom-domain-tooa-function-app"></a><span data-ttu-id="d7d8e-103">Egy egyéni tartomány tooa függvény app leképezése</span><span class="sxs-lookup"><span data-stu-id="d7d8e-103">Map a custom domain tooa function app</span></span>

<span data-ttu-id="d7d8e-104">Ez a parancsfájlpélda kapcsolódó erőforrások egy függvény alkalmazást hoz létre, és majd leképezi `www.<yourdomain>` tooit.</span><span class="sxs-lookup"><span data-stu-id="d7d8e-104">This sample script creates a function app with related resources, and then maps `www.<yourdomain>` tooit.</span></span> <span data-ttu-id="d7d8e-105">toomap tooa egyéni tartományt, a függvény app léteznie kell az App Service-csomagot, és nem egy fogyasztás tervben.</span><span class="sxs-lookup"><span data-stu-id="d7d8e-105">toomap tooa custom domain, your function app must be created in an App Service plan and not in a consumption plan.</span></span> <span data-ttu-id="d7d8e-106">Az Azure Functions csak akkor támogatja a leképezés az A rekord egyéni tartományt.</span><span class="sxs-lookup"><span data-stu-id="d7d8e-106">Azure Functions only supports mapping a custom domain using an A record.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d7d8e-107">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d7d8e-107">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="d7d8e-108">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="d7d8e-108">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="d7d8e-109">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d7d8e-109">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="d7d8e-110">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="d7d8e-110">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain tooa function app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="d7d8e-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="d7d8e-111">Script explanation</span></span>

<span data-ttu-id="d7d8e-112">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="d7d8e-112">This script uses hello following commands.</span></span> <span data-ttu-id="d7d8e-113">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="d7d8e-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="d7d8e-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="d7d8e-114">Command</span></span> | <span data-ttu-id="d7d8e-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d7d8e-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d7d8e-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7d8e-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="d7d8e-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d7d8e-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d7d8e-118">az storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7d8e-118">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="d7d8e-119">Hello függvény alkalmazás által igényelt tárfiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d7d8e-119">Creates a storage account required by hello function app.</span></span> |
| [<span data-ttu-id="d7d8e-120">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7d8e-120">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="d7d8e-121">Az App Service-csomag szükséges toomap hoz létre egyéni tartományt.</span><span class="sxs-lookup"><span data-stu-id="d7d8e-121">Creates an App Service plan required toomap a custom domain.</span></span> |
| [<span data-ttu-id="d7d8e-122">az functionapp létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7d8e-122">az functionapp create</span></span>]() | <span data-ttu-id="d7d8e-123">Létrehoz egy függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d7d8e-123">Creates a function app.</span></span> |
| [<span data-ttu-id="d7d8e-124">az App Service web config állomásnév hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d7d8e-124">az appservice web config hostname add</span></span>](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#add) | <span data-ttu-id="d7d8e-125">A Maps egy egyéni tartomány tooa függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d7d8e-125">Maps a custom domain tooa function app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d7d8e-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d7d8e-126">Next steps</span></span>

<span data-ttu-id="d7d8e-127">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d7d8e-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d7d8e-128">További funkciók CLI parancsfájl minták hello található [dokumentáció az Azure Functions]().</span><span class="sxs-lookup"><span data-stu-id="d7d8e-128">Additional Functions CLI script samples can be found in hello [Azure Functions documentation]().</span></span>
