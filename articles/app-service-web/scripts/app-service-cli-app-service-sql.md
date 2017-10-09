---
title: "aaaAzure CLI parancsfájl minta - web app tooa SQL adatbázis csatlakoztatása |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - web app tooa SQL adatbázis csatlakoztatása"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 7c2efdd0-f553-4038-a77a-e953021b3f77
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: adee42cd659d977b49e71d974d240324f68f67f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-web-app-tooa-sql-database"></a><span data-ttu-id="c0115-103">Webes alkalmazás tooa SQL adatbázis csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="c0115-103">Connect a web app tooa SQL database</span></span>

<span data-ttu-id="c0115-104">Ebben a forgatókönyvben megtudhatja, hogyan toocreate Azure SQL-adatbázis és egy Azure webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c0115-104">In this scenario you will learn how toocreate an Azure SQL database and an Azure web app.</span></span> <span data-ttu-id="c0115-105">Majd hello SQL adatbázis toohello web app használatával Alkalmazásbeállítások csatolást.</span><span class="sxs-lookup"><span data-stu-id="c0115-105">Then you will link hello SQL database toohello web app using app settings.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c0115-106">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="c0115-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="c0115-107">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="c0115-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="c0115-108">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c0115-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="c0115-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="c0115-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "SQL Database")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="c0115-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="c0115-110">Script explanation</span></span>

<span data-ttu-id="c0115-111">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, webalkalmazás, SQL-adatbázis és az összes kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="c0115-111">This script uses hello following commands toocreate a resource group, web app, SQL database and all related resources.</span></span> <span data-ttu-id="c0115-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="c0115-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="c0115-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="c0115-113">Command</span></span> | <span data-ttu-id="c0115-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c0115-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c0115-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0115-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="c0115-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c0115-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c0115-117">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0115-117">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="c0115-118">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c0115-118">Creates an App Service plan.</span></span> <span data-ttu-id="c0115-119">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="c0115-119">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="c0115-120">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0115-120">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="c0115-121">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c0115-121">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="c0115-122">az sql-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0115-122">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="c0115-123">Létrehoz egy SQL adatbázis-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="c0115-123">Creates a SQL Database Server.</span></span>  |
| [<span data-ttu-id="c0115-124">az sql-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0115-124">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="c0115-125">Hello SQL adatbázis-kiszolgáló egy új adatbázist hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c0115-125">Creates a new database with hello SQL Database Server.</span></span> |
| [<span data-ttu-id="c0115-126">az alkalmazás kulcs appsettings konfiguráció</span><span class="sxs-lookup"><span data-stu-id="c0115-126">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="c0115-127">Létrehozza vagy frissíti az Azure-webalkalmazás Alkalmazásbeállítás.</span><span class="sxs-lookup"><span data-stu-id="c0115-127">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="c0115-128">Alkalmazásbeállítások az alkalmazások környezeti változóként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="c0115-128">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c0115-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c0115-129">Next steps</span></span>

<span data-ttu-id="c0115-130">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c0115-130">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c0115-131">További App Service CLI parancsfájl minták hello található [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c0115-131">Additional App Service CLI script samples can be found in hello [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
