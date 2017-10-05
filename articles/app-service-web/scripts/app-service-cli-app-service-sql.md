---
title: "Az Azure CLI-parancsfájlt minta - webes alkalmazás csatlakoztatása SQL-adatbázishoz |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - webes alkalmazás csatlakoztatása SQL-adatbázishoz"
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
ms.openlocfilehash: ec5e22bfacc12a89f1fb5882487df4829369777c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-a-web-app-to-a-sql-database"></a><span data-ttu-id="92502-103">Webes alkalmazás csatlakoztatása SQL-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="92502-103">Connect a web app to a SQL database</span></span>

<span data-ttu-id="92502-104">Ebben a forgatókönyvben, megtudhatja, hogyan Azure SQL-adatbázis és az Azure-webalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="92502-104">In this scenario you will learn how to create an Azure SQL database and an Azure web app.</span></span> <span data-ttu-id="92502-105">Az SQL-adatbázis majd összekapcsolja a webes alkalmazás alkalmazás-beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="92502-105">Then you will link the SQL database to the web app using app settings.</span></span>


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="92502-106">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="92502-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="92502-107">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="92502-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="92502-108">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="92502-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="92502-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="92502-109">Sample script</span></span>

<span data-ttu-id="92502-110">[!code-azurecli-interactive[fő](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "SQL-adatbázis")]</span><span class="sxs-lookup"><span data-stu-id="92502-110">[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "SQL Database")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="92502-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="92502-111">Script explanation</span></span>

<span data-ttu-id="92502-112">A parancsfájl a következő parancsokat egy erőforráscsoport, webalkalmazás, SQL-adatbázis és az összes kapcsolódó erőforrások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="92502-112">This script uses the following commands to create a resource group, web app, SQL database and all related resources.</span></span> <span data-ttu-id="92502-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="92502-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="92502-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="92502-114">Command</span></span> | <span data-ttu-id="92502-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="92502-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="92502-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="92502-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="92502-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="92502-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="92502-118">az App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="92502-118">az appservice plan create</span></span>](https://docs.microsoft.com/cli/azure/appservice/plan#create) | <span data-ttu-id="92502-119">App Service-csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="92502-119">Creates an App Service plan.</span></span> <span data-ttu-id="92502-120">Ez olyan, mintha egy kiszolgálófarmon, az Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="92502-120">This is like a server farm for your Azure web app.</span></span> |
| [<span data-ttu-id="92502-121">az alkalmazás-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="92502-121">az webapp create</span></span>](https://docs.microsoft.com/cli/azure/webapp#create) | <span data-ttu-id="92502-122">Létrehoz egy Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="92502-122">Creates an Azure web app.</span></span> |
| [<span data-ttu-id="92502-123">az sql-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="92502-123">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="92502-124">Létrehoz egy SQL adatbázis-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="92502-124">Creates a SQL Database Server.</span></span>  |
| [<span data-ttu-id="92502-125">az sql-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="92502-125">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="92502-126">Létrehoz egy új adatbázist az SQL-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="92502-126">Creates a new database with the SQL Database Server.</span></span> |
| [<span data-ttu-id="92502-127">az alkalmazás kulcs appsettings konfiguráció</span><span class="sxs-lookup"><span data-stu-id="92502-127">az webapp config appsettings set</span></span>](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | <span data-ttu-id="92502-128">Létrehozza vagy frissíti az Azure-webalkalmazás Alkalmazásbeállítás.</span><span class="sxs-lookup"><span data-stu-id="92502-128">Creates or updates an app setting for an Azure web app.</span></span> <span data-ttu-id="92502-129">Alkalmazásbeállítások az alkalmazások környezeti változóként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="92502-129">App settings are exposed as environment variables for your app.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="92502-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="92502-130">Next steps</span></span>

<span data-ttu-id="92502-131">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="92502-131">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="92502-132">További App Service CLI parancsfájl minták megtalálhatók a [Azure App Service-dokumentáció](../app-service-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="92502-132">Additional App Service CLI script samples can be found in the [Azure App Service documentation](../app-service-cli-samples.md).</span></span>
