---
title: "aaaCLI példa-Azure SQL-adatbázis létrehozása |} Microsoft Docs"
description: "Az Azure CLI például parancsfájl-toocreate SQL-adatbázis"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 0d54e284e19f16387813e24d7beb7ab048a39263
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-toocreate-a-single-azure-sql-database-and-configure-a-firewall-rule"></a><span data-ttu-id="45496-103">CLI toocreate egy Azure SQL-adatbázis használata és a tűzfalszabályok beállítása</span><span class="sxs-lookup"><span data-stu-id="45496-103">Use CLI toocreate a single Azure SQL database and configure a firewall rule</span></span>

<span data-ttu-id="45496-104">Az Azure CLI mintaparancsfájl egy Azure SQL Database adatbázist hoz létre, és egy kiszolgálószintű tűzfalszabály konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="45496-104">This Azure CLI script example creates an Azure SQL database and configure a server-level firewall rule.</span></span> <span data-ttu-id="45496-105">Hello parancsfájl sikeres futtatása után hello elérhető SQL-adatbázis az összes Azure-szolgáltatások és hello konfigurált IP-címét.</span><span class="sxs-lookup"><span data-stu-id="45496-105">Once hello script has been successfully run, hello SQL Database can be accessed from all Azure services and hello configured IP address.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="45496-106">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="45496-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="45496-107">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="45496-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="45496-108">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="45496-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="45496-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="45496-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh?highlight=9-10 "Create SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="45496-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="45496-110">Clean up deployment</span></span>

<span data-ttu-id="45496-111">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="45496-111">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="45496-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="45496-112">Script explanation</span></span>

<span data-ttu-id="45496-113">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="45496-113">This script uses hello following commands.</span></span> <span data-ttu-id="45496-114">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="45496-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="45496-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="45496-115">Command</span></span> | <span data-ttu-id="45496-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="45496-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="45496-117">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="45496-117">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="45496-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="45496-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="45496-119">az sql-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="45496-119">az sql server create</span></span>](/cli/azure/sql/server#create) | <span data-ttu-id="45496-120">Hogy a gazdagépek hello SQL Database logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="45496-120">Creates a logical server that hosts hello SQL Database.</span></span> |
| [<span data-ttu-id="45496-121">az sql server tűzfal létrehozása</span><span class="sxs-lookup"><span data-stu-id="45496-121">az sql server firewall create</span></span>](/cli/azure/sql/server/firewall-rule#create) | <span data-ttu-id="45496-122">Létrehoz egy tűzfal szabály tooallow hozzáférés tooall SQL-adatbázisok hello megadott IP-címtartomány a hello kiszolgálóján.</span><span class="sxs-lookup"><span data-stu-id="45496-122">Creates a firewall rule tooallow access tooall SQL Databases on hello server from hello entered IP address range.</span></span> |
| [<span data-ttu-id="45496-123">az sql-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="45496-123">az sql db create</span></span>](/cli/azure/sql/db#create) | <span data-ttu-id="45496-124">Hello SQL Database logikai kiszolgáló hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="45496-124">Creates hello SQL Database in hello logical server.</span></span> |
| [<span data-ttu-id="45496-125">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="45496-125">az group delete</span></span>](/cli/azure/resource#delete) | <span data-ttu-id="45496-126">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="45496-126">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="45496-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="45496-127">Next steps</span></span>

<span data-ttu-id="45496-128">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="45496-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="45496-129">További SQL-adatbázis CLI parancsfájl minták hello található [dokumentáció az Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="45496-129">Additional SQL Database CLI script samples can be found in hello [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>

