---
title: "Az Azure CLI-parancsfájlt minta - webalkalmazás csatlakozni a redis gyorsítótár |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - webalkalmazás csatlakozni a redis gyorsítótár"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: b697c8508a6c3422b6b0d0ca36843a9c884b505f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-a-web-app-to-a-redis-cache"></a><span data-ttu-id="78e8f-103">Egy webes alkalmazás csatlakoztatása a redis gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="78e8f-103">Connect a web app to a redis cache</span></span>

<span data-ttu-id="78e8f-104">Ebben a forgatókönyvben, megtudhatja, hogyan egy Azure redis cache és az Azure-webalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="78e8f-104">In this scenario you will learn how to create an Azure redis cache and an Azure web app.</span></span> <span data-ttu-id="78e8f-105">Ezután a redis gyorsítótárt összekapcsolja a web app alkalmazást beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="78e8f-105">Then you will link the redis cache to the web app using app settings.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="78e8f-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="78e8f-106">Sample script</span></span>

<span data-ttu-id="78e8f-107">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis Cache")]</span><span class="sxs-lookup"><span data-stu-id="78e8f-107">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis Cache")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="78e8f-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="78e8f-108">Script explanation</span></span>

<span data-ttu-id="78e8f-109">Ezt a parancsfájlt a következő parancsok használatával hozzon létre egy erőforráscsoportot, webalkalmazás, redis gyorsítótárat, és minden kapcsolódó erőforrás.</span><span class="sxs-lookup"><span data-stu-id="78e8f-109">This script uses the following commands to create a resource group, web app, redis cache and all related resources.</span></span> <span data-ttu-id="78e8f-110">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="78e8f-110">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="78e8f-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="78e8f-111">Command</span></span> | <span data-ttu-id="78e8f-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="78e8f-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="78e8f-113">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="78e8f-113">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="78e8f-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="78e8f-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="78e8f-115">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="78e8f-115">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="78e8f-116">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="78e8f-116">Creates an App Service plan.</span></span> <span data-ttu-id="78e8f-117">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="78e8f-117">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="78e8f-118">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="78e8f-118">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="78e8f-119">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="78e8f-119">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="78e8f-120">az redis létrehozása</span><span class="sxs-lookup"><span data-stu-id="78e8f-120">az redis create</span></span>](https://docs.microsoft.com/en-us/cli/azure/redis#create) | <span data-ttu-id="78e8f-121">Hozzon létre új Redis Cache példányt.</span><span class="sxs-lookup"><span data-stu-id="78e8f-121">Create new Redis Cache instance.</span></span> <span data-ttu-id="78e8f-122">Ez az adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="78e8f-122">This is where the data will be stored.</span></span> |
| [<span data-ttu-id="78e8f-123">az-redis-listázása</span><span class="sxs-lookup"><span data-stu-id="78e8f-123">az redis list-keys</span></span>](https://docs.microsoft.com/en-us/cli/azure/redis#list-keys) | <span data-ttu-id="78e8f-124">A redis cache példány az elérési kulcsainak listázása.</span><span class="sxs-lookup"><span data-stu-id="78e8f-124">Lists the access keys for the redis cache instance.</span></span> |
| [<span data-ttu-id="78e8f-125">az alkalmazás kulcs appsettings konfiguráció</span><span class="sxs-lookup"><span data-stu-id="78e8f-125">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="78e8f-126">Létrehozza vagy frissíti az Azure-webalkalmazás Alkalmazásbeállítás.</span><span class="sxs-lookup"><span data-stu-id="78e8f-126">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="78e8f-127">Alkalmazásbeállítások az alkalmazások környezeti változóként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="78e8f-127">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="78e8f-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="78e8f-128">Next steps</span></span>

<span data-ttu-id="78e8f-129">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="78e8f-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="78e8f-130">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="78e8f-130">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
