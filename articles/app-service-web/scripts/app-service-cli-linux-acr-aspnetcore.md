---
title: "Az Azure CLI-parancsfájlt minták – az ASP.NET Core webalkalmazás létrehozása az Azure-tároló beállításjegyzékből egy Docker-tároló |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – az ASP.NET Core webalkalmazás létrehozása az Azure-tároló beállításjegyzékből egy Docker-tároló"
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
ms.openlocfilehash: 2556947d7cdd1475ae82ac2e1d61ad30ebd0d29f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-aspnet-core-web-app-in-a-docker-container-from-azure-container-registry"></a><span data-ttu-id="90d58-103">Az ASP.NET Core webalkalmazás létrehozása az Azure-tároló beállításjegyzékből egy Docker-tároló</span><span class="sxs-lookup"><span data-stu-id="90d58-103">Create an ASP.NET Core web app in a Docker container from Azure Container Registry</span></span>

<span data-ttu-id="90d58-104">Ebben a forgatókönyvben, megtudhatja, hogyan hozzon létre egy erőforráscsoportot, a Linux app service-csomag és a webes alkalmazás, és egy Docker-tároló használata az Azure-tároló regisztrációs ASP.NET Core alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="90d58-104">In this scenario you will learn how to create a resource group, Linux app service plan, and web app, and deploy an ASP.NET Core application using a Docker Container from the Azure Container Registry.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="90d58-105">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="90d58-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="90d58-106">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="90d58-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="90d58-107">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="90d58-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="90d58-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="90d58-108">Sample script</span></span>

<span data-ttu-id="90d58-109">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/deploy-linux-acr/deploy-linux-acr.sh?highlight=6-9 "Linux Azure tároló beállításjegyzék")]</span><span class="sxs-lookup"><span data-stu-id="90d58-109">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-linux-acr/deploy-linux-acr.sh?highlight=6-9 "Linux Azure Container Registry")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="90d58-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="90d58-110">Script explanation</span></span>

<span data-ttu-id="90d58-111">A parancsfájl a következő parancsokat egy erőforráscsoport, a web app és az összes kapcsolódó erőforrások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="90d58-111">This script uses the following commands to create a resource group, web app, and all related resources.</span></span> <span data-ttu-id="90d58-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="90d58-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="90d58-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="90d58-113">Command</span></span> | <span data-ttu-id="90d58-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="90d58-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="90d58-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="90d58-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="90d58-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="90d58-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="90d58-117">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="90d58-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="90d58-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="90d58-118">Creates an App Service plan.</span></span> <span data-ttu-id="90d58-119">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="90d58-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="90d58-120">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="90d58-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="90d58-121">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="90d58-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="90d58-122">az alkalmazás kulcs tároló konfiguráció</span><span class="sxs-lookup"><span data-stu-id="90d58-122">az webapp config container set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/container#set) | <span data-ttu-id="90d58-123">Beállítja a Docker-tároló az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="90d58-123">Sets the Docker container for the Azure web app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="90d58-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="90d58-124">Next steps</span></span>

<span data-ttu-id="90d58-125">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="90d58-125">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="90d58-126">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="90d58-126">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
