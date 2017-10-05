---
title: "Az Azure CLI-parancsfájlt minta - webalkalmazás csatlakozzon Cosmos Adatbázishoz |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – Cosmos DB webes alkalmazás csatlakoztatása"
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
ms.openlocfilehash: ff5e7a794033cc51120831e09b055a86affb28a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-a-web-app-to-cosmos-db"></a><span data-ttu-id="fb32c-103">Webes alkalmazás csatlakoztatása az Cosmos-Adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="fb32c-103">Connect a web app to Cosmos DB</span></span>

<span data-ttu-id="fb32c-104">Ebben a forgatókönyvben, megtudhatja, hogyan hozzon létre egy Azure Cosmos DB fiókot és egy Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="fb32c-104">In this scenario you will learn how to create an Azure Cosmos DB account and an Azure web app.</span></span> <span data-ttu-id="fb32c-105">A Cosmos DB majd összekapcsolja a webes alkalmazás alkalmazás-beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="fb32c-105">Then you will link the Cosmos DB to the web app using app settings.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="fb32c-106">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="fb32c-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="fb32c-107">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="fb32c-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="fb32c-108">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="fb32c-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="fb32c-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="fb32c-109">Sample script</span></span>

<span data-ttu-id="fb32c-110">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]</span><span class="sxs-lookup"><span data-stu-id="fb32c-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="fb32c-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="fb32c-111">Script explanation</span></span>

<span data-ttu-id="fb32c-112">A parancsfájl a következő parancsok segítségével hozzon létre egy erőforráscsoportot, webalkalmazás, Cosmos DB, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="fb32c-112">This script uses the following commands to create a resource group, web app, Cosmos DB and all related resources.</span></span> <span data-ttu-id="fb32c-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="fb32c-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="fb32c-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="fb32c-114">Command</span></span> | <span data-ttu-id="fb32c-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="fb32c-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="fb32c-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="fb32c-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="fb32c-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fb32c-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="fb32c-118">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="fb32c-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="fb32c-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fb32c-119">Creates an App Service plan.</span></span> <span data-ttu-id="fb32c-120">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="fb32c-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="fb32c-121">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="fb32c-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="fb32c-122">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="fb32c-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="fb32c-123">az cosmosdb létrehozása</span><span class="sxs-lookup"><span data-stu-id="fb32c-123">az cosmosdb create</span></span>](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#create) | <span data-ttu-id="fb32c-124">Létrehoz egy Cosmos-DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="fb32c-124">Creates a Cosmos DB account.</span></span> <span data-ttu-id="fb32c-125">Ez az adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="fb32c-125">This is where the data will be stored.</span></span> |
| [<span data-ttu-id="fb32c-126">az cosmosdb lista-kulcsok</span><span class="sxs-lookup"><span data-stu-id="fb32c-126">az cosmosdb list-keys</span></span>](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#list-keys) | <span data-ttu-id="fb32c-127">A megadott Cosmos DB-fiók elérési kulcsainak listázása.</span><span class="sxs-lookup"><span data-stu-id="fb32c-127">Lists the access keys for the specified Cosmos DB account.</span></span> |
| [<span data-ttu-id="fb32c-128">az alkalmazás kulcs appsettings konfiguráció</span><span class="sxs-lookup"><span data-stu-id="fb32c-128">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="fb32c-129">Létrehozza vagy frissíti az Azure-webalkalmazás Alkalmazásbeállítás.</span><span class="sxs-lookup"><span data-stu-id="fb32c-129">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="fb32c-130">Alkalmazásbeállítások az alkalmazások környezeti változóként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="fb32c-130">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="fb32c-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fb32c-131">Next steps</span></span>

<span data-ttu-id="fb32c-132">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fb32c-132">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="fb32c-133">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="fb32c-133">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
