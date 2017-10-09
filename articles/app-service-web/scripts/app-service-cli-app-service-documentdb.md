---
title: "aaaAzure CLI parancsfájl minta - csatlakozás a webes alkalmazás tooCosmos DB |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - csatlakozás a webes alkalmazás tooCosmos DB"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bbbdbc42-efb5-4b4f-8ba6-c03c9d16a7ea
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 1f2123378b9d5812fa793730f7fa5a5bc9ab63c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-toocosmos-db"></a><span data-ttu-id="7de18-103">Csatlakozás egy webes alkalmazás tooCosmos DB</span><span class="sxs-lookup"><span data-stu-id="7de18-103">Connect a web app tooCosmos DB</span></span>

<span data-ttu-id="7de18-104">Ebben a forgatókönyvben megtudhatja, hogyan toocreate egy Azure Cosmos DB fiókot és egy Azure webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7de18-104">In this scenario you will learn how toocreate an Azure Cosmos DB account and an Azure web app.</span></span> <span data-ttu-id="7de18-105">Majd hello Cosmos DB toohello web app használatával Alkalmazásbeállítások csatolást.</span><span class="sxs-lookup"><span data-stu-id="7de18-105">Then you will link hello Cosmos DB toohello web app using app settings.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="7de18-106">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="7de18-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="7de18-107">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="7de18-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="7de18-108">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7de18-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="7de18-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="7de18-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="7de18-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="7de18-110">Script explanation</span></span>

<span data-ttu-id="7de18-111">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, webalkalmazás, Cosmos adatbázis és az összes kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="7de18-111">This script uses hello following commands toocreate a resource group, web app, Cosmos DB and all related resources.</span></span> <span data-ttu-id="7de18-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="7de18-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="7de18-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="7de18-113">Command</span></span> | <span data-ttu-id="7de18-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="7de18-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7de18-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="7de18-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="7de18-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7de18-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7de18-117">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="7de18-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="7de18-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7de18-118">Creates an App Service plan.</span></span> <span data-ttu-id="7de18-119">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="7de18-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="7de18-120">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="7de18-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="7de18-121">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7de18-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="7de18-122">az cosmosdb létrehozása</span><span class="sxs-lookup"><span data-stu-id="7de18-122">az cosmosdb create</span></span>](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#create) | <span data-ttu-id="7de18-123">Létrehoz egy Cosmos-DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="7de18-123">Creates a Cosmos DB account.</span></span> <span data-ttu-id="7de18-124">Ez a hello adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="7de18-124">This is where hello data will be stored.</span></span> |
| [<span data-ttu-id="7de18-125">az cosmosdb lista-kulcsok</span><span class="sxs-lookup"><span data-stu-id="7de18-125">az cosmosdb list-keys</span></span>](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#list-keys) | <span data-ttu-id="7de18-126">Listák hello elérési kulcsainak hello Cosmos DB fiókot adott meg.</span><span class="sxs-lookup"><span data-stu-id="7de18-126">Lists hello access keys for hello specified Cosmos DB account.</span></span> |
| [<span data-ttu-id="7de18-127">az alkalmazás kulcs appsettings konfiguráció</span><span class="sxs-lookup"><span data-stu-id="7de18-127">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="7de18-128">Létrehozza vagy frissíti az Azure-webalkalmazás Alkalmazásbeállítás.</span><span class="sxs-lookup"><span data-stu-id="7de18-128">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="7de18-129">Alkalmazásbeállítások az alkalmazások környezeti változóként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="7de18-129">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7de18-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7de18-130">Next steps</span></span>

<span data-ttu-id="7de18-131">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7de18-131">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="7de18-132">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="7de18-132">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
