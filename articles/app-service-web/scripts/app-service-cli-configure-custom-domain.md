---
title: "aaaAzure CLI parancsfájl minta - leképezés egy egyéni tartomány tooa webalkalmazás |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - leképezés egy egyéni tartomány tooa webalkalmazás"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 5ac4a680-cc73-4578-bcd6-8668c08802c2
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 49d6be092e438a63c0a43e3207080ca4cd5ff3fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="map-a-custom-domain-tooa-web-app"></a><span data-ttu-id="350dc-103">Az egyéni tartomány tooa webalkalmazás leképezése</span><span class="sxs-lookup"><span data-stu-id="350dc-103">Map a custom domain tooa web app</span></span>

<span data-ttu-id="350dc-104">Ez a parancsfájlpélda hoz létre egy webalkalmazást az App Service azok kapcsolódó erőforrásait, és majd leképezi `www.<yourdomain>` tooit.</span><span class="sxs-lookup"><span data-stu-id="350dc-104">This sample script creates a web app in App Service with its related resources, and then maps `www.<yourdomain>` tooit.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="350dc-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="350dc-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="350dc-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="350dc-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="350dc-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="350dc-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="350dc-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="350dc-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain tooa web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="350dc-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="350dc-109">Script explanation</span></span>

<span data-ttu-id="350dc-110">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="350dc-110">This script uses hello following commands.</span></span> <span data-ttu-id="350dc-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="350dc-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="350dc-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="350dc-112">Command</span></span> | <span data-ttu-id="350dc-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="350dc-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="350dc-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="350dc-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="350dc-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="350dc-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="350dc-116">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="350dc-116">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="350dc-117">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="350dc-117">Creates an App Service plan.</span></span> |
| [<span data-ttu-id="350dc-118">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="350dc-118">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="350dc-119">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="350dc-119">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="350dc-120">az alkalmazás kulcs config állomásnév hozzáadása</span><span class="sxs-lookup"><span data-stu-id="350dc-120">az webapp config hostname add</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/hostname#add) | <span data-ttu-id="350dc-121">Az egyéni tartomány tooa webalkalmazás rendeli.</span><span class="sxs-lookup"><span data-stu-id="350dc-121">Maps a custom domain tooa web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="350dc-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="350dc-122">Next steps</span></span>

<span data-ttu-id="350dc-123">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="350dc-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="350dc-124">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="350dc-124">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
