---
title: "aaaCLI példa-áthelyezési Azure SQL adatbázis-SQL rugalmas készlet |} Microsoft Docs"
description: "Az Azure CLI például parancsfájl-toomove SQL adatbázishoz a rugalmas SQL-készlet"
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
ms.date: 07/05/2017
ms.author: janeng
ms.openlocfilehash: 841eb57d2d49612c3fadd3a6424a2b0309c69719
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-toomove-an-azure-sql-database-in-a-sql-elastic-pool"></a><span data-ttu-id="e477a-103">A rugalmas SQL-készlet CLI toomove Azure SQL-adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="e477a-103">Use CLI toomove an Azure SQL database in a SQL elastic pool</span></span>

<span data-ttu-id="e477a-104">Az Azure CLI mintaparancsfájl hoz létre két rugalmas készletek és egy rugalmas SQL-készlet egy Azure SQL Database adatbázist áthelyezi egy másik rugalmas SQL-készletet, és majd hello adatbázis kimozdul a rugalmas készlet tooa egyetlen Azure-adatbázis teljesítményszintjét.</span><span class="sxs-lookup"><span data-stu-id="e477a-104">This Azure CLI script example creates two elastic pools and moves an Azure SQL database from one SQL elastic pool into another SQL elastic pool, and then moves hello database out of elastic pool tooa single Azure database performance level.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="e477a-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="e477a-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="e477a-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="e477a-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="e477a-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e477a-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="e477a-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="e477a-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/move-database-between-pools/move-database-between-pools.sh "Move database between pools")]

## <a name="clean-up-deployment"></a><span data-ttu-id="e477a-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="e477a-109">Clean up deployment</span></span>

<span data-ttu-id="e477a-110">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="e477a-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="e477a-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="e477a-111">Script explanation</span></span>

<span data-ttu-id="e477a-112">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="e477a-112">This script uses hello following commands.</span></span> <span data-ttu-id="e477a-113">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="e477a-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="e477a-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="e477a-114">Command</span></span> | <span data-ttu-id="e477a-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="e477a-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e477a-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="e477a-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="e477a-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e477a-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="e477a-118">az sql-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="e477a-118">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="e477a-119">Adatbázis vagy a rugalmas készlet tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e477a-119">Creates a logical server that hosts a database or elastic pool.</span></span> |
| [<span data-ttu-id="e477a-120">az sql rugalmas-címkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="e477a-120">az sql elastic-pools create</span></span>](https://docs.microsoft.com/cli/azure/sql/elastic-pool#create) | <span data-ttu-id="e477a-121">Létrehoz egy rugalmas készlet hello logikai kiszolgálón belül.</span><span class="sxs-lookup"><span data-stu-id="e477a-121">Creates an elastic pool within hello logical server.</span></span> |
| [<span data-ttu-id="e477a-122">az sql-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="e477a-122">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="e477a-123">Adatbázist hoz létre egy logikai egyetlen vagy egy készletezett adatbázis-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="e477a-123">Creates a database in a logical server as a single or a pooled database.</span></span> |
| [<span data-ttu-id="e477a-124">az sql-adatbázis frissítése</span><span class="sxs-lookup"><span data-stu-id="e477a-124">az sql db update</span></span>](https://docs.microsoft.com/cli/azure/sql/db#update) | <span data-ttu-id="e477a-125">Frissíti az adatbázis-tulajdonságai, vagy azokat, kívüli vagy rugalmas készletek között adatbázis helyezi.</span><span class="sxs-lookup"><span data-stu-id="e477a-125">Updates database properties or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="e477a-126">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="e477a-126">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="e477a-127">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="e477a-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e477a-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e477a-128">Next steps</span></span>

<span data-ttu-id="e477a-129">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e477a-129">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="e477a-130">További SQL-adatbázis CLI parancsfájl minták hello található [dokumentáció az Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e477a-130">Additional SQL Database CLI script samples can be found in hello [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>


