---
title: "aaaAzure CLI parancsfájl minta - csatlakozás a webes alkalmazás tooa redis gyorsítótár |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - csatlakozás a webes alkalmazás tooa redis gyorsítótár"
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
ms.openlocfilehash: b911e6643591b8f07aeb64d4d62876c0fa156a8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-redis-cache"></a><span data-ttu-id="48410-103">Csatlakozás a webes alkalmazás tooa redis gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="48410-103">Connect a web app tooa redis cache</span></span>

<span data-ttu-id="48410-104">Ebben a forgatókönyvben megtudhatja, hogyan toocreate az Azure redis cache és az Azure-webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="48410-104">In this scenario you will learn how toocreate an Azure redis cache and an Azure web app.</span></span> <span data-ttu-id="48410-105">Majd hello redis gyorsítótár toohello web app használatával Alkalmazásbeállítások csatolást.</span><span class="sxs-lookup"><span data-stu-id="48410-105">Then you will link hello redis cache toohello web app using app settings.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="48410-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="48410-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="48410-107">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="48410-107">Script explanation</span></span>

<span data-ttu-id="48410-108">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, webalkalmazás, redis gyorsítótárat, és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="48410-108">This script uses hello following commands toocreate a resource group, web app, redis cache and all related resources.</span></span> <span data-ttu-id="48410-109">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="48410-109">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="48410-110">Parancs</span><span class="sxs-lookup"><span data-stu-id="48410-110">Command</span></span> | <span data-ttu-id="48410-111">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="48410-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="48410-112">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="48410-112">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="48410-113">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="48410-113">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="48410-114">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="48410-114">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="48410-115">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="48410-115">Creates an App Service plan.</span></span> <span data-ttu-id="48410-116">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="48410-116">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="48410-117">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="48410-117">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="48410-118">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="48410-118">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="48410-119">az redis létrehozása</span><span class="sxs-lookup"><span data-stu-id="48410-119">az redis create</span></span>](https://docs.microsoft.com/en-us/cli/azure/redis#create) | <span data-ttu-id="48410-120">Hozzon létre új Redis Cache példányt.</span><span class="sxs-lookup"><span data-stu-id="48410-120">Create new Redis Cache instance.</span></span> <span data-ttu-id="48410-121">Ez a hello adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="48410-121">This is where hello data will be stored.</span></span> |
| [<span data-ttu-id="48410-122">az-redis-listázása</span><span class="sxs-lookup"><span data-stu-id="48410-122">az redis list-keys</span></span>](https://docs.microsoft.com/en-us/cli/azure/redis#list-keys) | <span data-ttu-id="48410-123">Felsorolja a hello redis cache példány hello hozzáférési kulcsainak listázása.</span><span class="sxs-lookup"><span data-stu-id="48410-123">Lists hello access keys for hello redis cache instance.</span></span> |
| [<span data-ttu-id="48410-124">az alkalmazás kulcs appsettings konfiguráció</span><span class="sxs-lookup"><span data-stu-id="48410-124">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="48410-125">Létrehozza vagy frissíti az Azure-webalkalmazás Alkalmazásbeállítás.</span><span class="sxs-lookup"><span data-stu-id="48410-125">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="48410-126">Alkalmazásbeállítások az alkalmazások környezeti változóként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="48410-126">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="48410-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="48410-127">Next steps</span></span>

<span data-ttu-id="48410-128">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="48410-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="48410-129">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="48410-129">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
