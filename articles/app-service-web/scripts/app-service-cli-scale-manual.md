---
title: "aaaAzure CLI parancsfájl minta - segítségével manuálisan Azure CLI 2.0 webalkalmazás skálázása |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - skálázási egy webalkalmazást, manuálisan az Azure CLI 2.0 használatával"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 251d9074-8fff-4121-ad16-9eca9556ac96
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 64464c8a44522fdc2c8f3d0192388302a1d12667
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-web-app-manually"></a><span data-ttu-id="e171a-103">A webalkalmazás skálázása manuálisan</span><span class="sxs-lookup"><span data-stu-id="e171a-103">Scale a web app manually</span></span>

<span data-ttu-id="e171a-104">Ebben a forgatókönyvben egy erőforráscsoport, toocreate megtanulhatja app service-csomag és a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e171a-104">In this scenario you will learn toocreate a resource group, app service plan and web app.</span></span> <span data-ttu-id="e171a-105">Ezután a hello App Service-csomag egy egypéldányos toomultiple példányokból lesz skálázva.</span><span class="sxs-lookup"><span data-stu-id="e171a-105">You will then scale hello App Service Plan from a single instance toomultiple instances.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="e171a-106">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="e171a-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="e171a-107">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="e171a-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="e171a-108">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e171a-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="e171a-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="e171a-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-manual/scale-manual.sh "Manual Scale")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="e171a-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="e171a-110">Script explanation</span></span>

<span data-ttu-id="e171a-111">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a web app és az összes kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="e171a-111">This script uses hello following commands toocreate a resource group, web app, and all related resources.</span></span> <span data-ttu-id="e171a-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="e171a-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="e171a-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="e171a-113">Command</span></span> | <span data-ttu-id="e171a-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="e171a-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e171a-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="e171a-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="e171a-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e171a-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="e171a-117">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="e171a-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="e171a-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e171a-118">Creates an App Service plan.</span></span> <span data-ttu-id="e171a-119">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="e171a-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="e171a-120">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="e171a-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="e171a-121">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e171a-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="e171a-122">az App Service-csomag frissítése</span><span class="sxs-lookup"><span data-stu-id="e171a-122">az appservice plan update</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#update) | <span data-ttu-id="e171a-123">App Service-csomag hello frissítések tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="e171a-123">Updates properties of hello App Service plan.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e171a-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e171a-124">Next steps</span></span>

<span data-ttu-id="e171a-125">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e171a-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="e171a-126">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e171a-126">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
