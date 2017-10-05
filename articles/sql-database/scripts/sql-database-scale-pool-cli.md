---
title: "Parancssori felület például egy SQL rugalmas készlet-Azure SQL Database méretezi |} Microsoft Docs"
description: "Az Azure parancssori felület a példaként megadott parancsfájlt a rugalmas SQL-készletet, az Azure SQL-adatbázis méretezése"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 888d2b7b7c118fede82d39881570a3b3d7b09961
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-cli-to-scale-a-sql-elastic-pool-in-azure-sql-database"></a><span data-ttu-id="6b022-103">A rugalmas SQL-készletet, az Azure SQL-adatbázis méretezése parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="6b022-103">Use CLI to scale a SQL elastic pool in Azure SQL Database</span></span>

<span data-ttu-id="6b022-104">Az Azure CLI mintaparancsfájl hoz létre SQL rugalmas készletek, készletezett adatbázisok áthelyezése, és a rugalmas készlet teljesítményszintet vált.</span><span class="sxs-lookup"><span data-stu-id="6b022-104">This Azure CLI script example creates SQL elastic pools, moves pooled databases, and changes elastic pool performance levels.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="6b022-105">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="6b022-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="6b022-106">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="6b022-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="6b022-107">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6b022-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="6b022-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="6b022-108">Sample script</span></span>

<span data-ttu-id="6b022-109">[!code-azurecli-interactive[fő](../../../cli_scripts/sql-database/scale-pool/scale-pool.sh "adatbázis áthelyezése készletek között")]</span><span class="sxs-lookup"><span data-stu-id="6b022-109">[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/scale-pool/scale-pool.sh "Move database between pools")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="6b022-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6b022-110">Clean up deployment</span></span>

<span data-ttu-id="6b022-111">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="6b022-111">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="6b022-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="6b022-112">Script explanation</span></span>

<span data-ttu-id="6b022-113">A parancsfájl a következő parancsokat egy erőforráscsoportot, a logikai kiszolgáló, az SQL-adatbázis és a tűzfalszabályok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6b022-113">This script uses the following commands to create a resource group, logical server, SQL Database, and firewall rules.</span></span> <span data-ttu-id="6b022-114">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="6b022-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="6b022-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="6b022-115">Command</span></span> | <span data-ttu-id="6b022-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="6b022-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="6b022-117">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b022-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="6b022-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6b022-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="6b022-119">az sql-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b022-119">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="6b022-120">Az SQL-adatbázist futtató logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6b022-120">Creates a logical server that hosts the SQL Database.</span></span> |
| [<span data-ttu-id="6b022-121">az sql rugalmas-címkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b022-121">az sql elastic-pools create</span></span>](https://docs.microsoft.com/cli/azure/sql/elastic-pool#create) | <span data-ttu-id="6b022-122">A logikai kiszolgálón belül a rugalmas adatbáziskészlet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6b022-122">Creates an elastic database pool within the logical server.</span></span> |
| [<span data-ttu-id="6b022-123">az sql-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b022-123">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="6b022-124">Az SQL-adatbázis létrehozása a logikai kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="6b022-124">Creates the SQL Database in the logical server.</span></span> |
| [<span data-ttu-id="6b022-125">az sql rugalmas-gyűjtők frissítése</span><span class="sxs-lookup"><span data-stu-id="6b022-125">az sql elastic-pools update</span></span>](https://docs.microsoft.com/cli/azure/sql/elastic-pool#update) | <span data-ttu-id="6b022-126">A rugalmas adatbáziskészlet frissíti, ebben a példában a hozzárendelt edtu-ra módosítja.</span><span class="sxs-lookup"><span data-stu-id="6b022-126">Updates an elastic database pool, in this example changes the assigned eDTU.</span></span> |
| [<span data-ttu-id="6b022-127">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="6b022-127">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="6b022-128">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="6b022-128">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6b022-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6b022-129">Next steps</span></span>

<span data-ttu-id="6b022-130">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6b022-130">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="6b022-131">További SQL-adatbázis CLI parancsfájl minták megtalálhatók a [dokumentáció az Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="6b022-131">Additional SQL Database CLI script samples can be found in the [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>
