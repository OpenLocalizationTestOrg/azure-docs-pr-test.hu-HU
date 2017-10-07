---
title: "parancssori felület parancsfájl minta - aaaAzure az ASP.NET Core webalkalmazás létrehozása az egy Docker-tároló |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – az ASP.NET Core webalkalmazás létrehozása az egy Docker-tároló"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 3a2d1983-ff7b-476a-ac44-49ec2aabb31a
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 23106345bfbbf1f68757d99010db98e7c9a7da49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-core-web-app-in-a-docker-container"></a><span data-ttu-id="8c994-103">Az ASP.NET Core webalkalmazás létrehozása az egy Docker-tároló</span><span class="sxs-lookup"><span data-stu-id="8c994-103">Create an ASP.NET Core web app in a Docker container</span></span>

<span data-ttu-id="8c994-104">Ebben a forgatókönyvben megtanulhatja, hogyan toocreate egy erőforráscsoport, Linux app service terv és a webes alkalmazás, illetve alkalmazás üzembe helyezése az ASP.NET Core segítségével egy Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="8c994-104">In this scenario you will learn how toocreate a resource group, Linux app service plan, and web app, and deploy an ASP.NET Core application using a Docker Container.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="8c994-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="8c994-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="8c994-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="8c994-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="8c994-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="8c994-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="sample-script"></a><span data-ttu-id="8c994-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="8c994-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-linux-docker/deploy-linux-docker.sh?highlight=6 "Linux Docker")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="8c994-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="8c994-109">Script explanation</span></span>

<span data-ttu-id="8c994-110">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a web app és az összes kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="8c994-110">This script uses hello following commands toocreate a resource group, web app, and all related resources.</span></span> <span data-ttu-id="8c994-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="8c994-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="8c994-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="8c994-112">Command</span></span> | <span data-ttu-id="8c994-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="8c994-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8c994-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c994-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="8c994-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8c994-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8c994-116">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c994-116">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="8c994-117">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8c994-117">Creates an App Service plan.</span></span> <span data-ttu-id="8c994-118">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="8c994-118">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="8c994-119">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c994-119">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="8c994-120">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8c994-120">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="8c994-121">az alkalmazás kulcs tároló konfiguráció</span><span class="sxs-lookup"><span data-stu-id="8c994-121">az webapp config container set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/container#set) | <span data-ttu-id="8c994-122">Beállítja a Docker-tároló hello hello Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="8c994-122">Sets hello Docker container for hello Azure web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8c994-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8c994-123">Next steps</span></span>

<span data-ttu-id="8c994-124">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8c994-124">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="8c994-125">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="8c994-125">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
