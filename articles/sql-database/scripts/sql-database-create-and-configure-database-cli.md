---
title: "Parancssori felület példa-egy Azure SQL-adatbázis létrehozása |} Microsoft Docs"
description: "Az Azure parancssori felület a példaként megadott parancsfájlt az SQL-adatbázis létrehozása"
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
ms.openlocfilehash: 908898ca691d2b53b9f54afa60c41e091163bd50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-cli-to-create-a-single-azure-sql-database-and-configure-a-firewall-rule"></a><span data-ttu-id="d5d08-103">Parancssori felület használatával hozzon létre egy Azure SQL-adatbázis és a tűzfalszabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d5d08-103">Use CLI to create a single Azure SQL database and configure a firewall rule</span></span>

<span data-ttu-id="d5d08-104">Az Azure CLI mintaparancsfájl egy Azure SQL Database adatbázist hoz létre, és egy kiszolgálószintű tűzfalszabály konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="d5d08-104">This Azure CLI script example creates an Azure SQL database and configure a server-level firewall rule.</span></span> <span data-ttu-id="d5d08-105">Miután a parancsfájl sikeresen lefutott, az SQL-adatbázis elérhető az összes Azure-szolgáltatások és a konfigurált IP-címet.</span><span class="sxs-lookup"><span data-stu-id="d5d08-105">Once the script has been successfully run, the SQL Database can be accessed from all Azure services and the configured IP address.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d5d08-106">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="d5d08-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="d5d08-107">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="d5d08-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="d5d08-108">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d5d08-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="d5d08-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="d5d08-109">Sample script</span></span>

<span data-ttu-id="d5d08-110">[!code-azurecli-interactive[fő](../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh?highlight=9-10 "SQL-adatbázis létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="d5d08-110">[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh?highlight=9-10 "Create SQL Database")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="d5d08-111">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d5d08-111">Clean up deployment</span></span>

<span data-ttu-id="d5d08-112">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d5d08-112">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="d5d08-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="d5d08-113">Script explanation</span></span>

<span data-ttu-id="d5d08-114">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="d5d08-114">This script uses the following commands.</span></span> <span data-ttu-id="d5d08-115">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="d5d08-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="d5d08-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="d5d08-116">Command</span></span> | <span data-ttu-id="d5d08-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d5d08-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d5d08-118">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5d08-118">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="d5d08-119">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d5d08-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d5d08-120">az sql-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5d08-120">az sql server create</span></span>](/cli/azure/sql/server#create) | <span data-ttu-id="d5d08-121">Az SQL-adatbázist futtató logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d5d08-121">Creates a logical server that hosts the SQL Database.</span></span> |
| [<span data-ttu-id="d5d08-122">az sql server tűzfal létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5d08-122">az sql server firewall create</span></span>](/cli/azure/sql/server/firewall-rule#create) | <span data-ttu-id="d5d08-123">Engedélyezi a kiszolgálón lévő összes SQL-adatbázis elérését a megadott IP-címtartomány egy tűzfalszabályt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d5d08-123">Creates a firewall rule to allow access to all SQL Databases on the server from the entered IP address range.</span></span> |
| [<span data-ttu-id="d5d08-124">az sql-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5d08-124">az sql db create</span></span>](/cli/azure/sql/db#create) | <span data-ttu-id="d5d08-125">Az SQL-adatbázis létrehozása a logikai kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="d5d08-125">Creates the SQL Database in the logical server.</span></span> |
| [<span data-ttu-id="d5d08-126">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="d5d08-126">az group delete</span></span>](/cli/azure/resource#delete) | <span data-ttu-id="d5d08-127">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="d5d08-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d5d08-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d5d08-128">Next steps</span></span>

<span data-ttu-id="d5d08-129">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d5d08-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d5d08-130">További SQL-adatbázis CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="d5d08-130">Additional SQL Database CLI script samples can be found in the [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>

