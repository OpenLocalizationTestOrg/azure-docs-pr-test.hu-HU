---
title: "aaaCLI például egy SQL rugalmas készlet-Azure SQL Database méretezi |} Microsoft Docs"
description: "Az Azure CLI például parancsfájl-tooscale a rugalmas SQL-készletet, az Azure SQL-adatbázis"
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
ms.openlocfilehash: 436128b8183213f78b9abc2ec46efe2a3ed3c37c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-tooscale-a-sql-elastic-pool-in-azure-sql-database"></a><span data-ttu-id="adbd9-103">Parancssori felület tooscale egy SQL Azure SQL Database rugalmas készlet használata</span><span class="sxs-lookup"><span data-stu-id="adbd9-103">Use CLI tooscale a SQL elastic pool in Azure SQL Database</span></span>

<span data-ttu-id="adbd9-104">Az Azure CLI mintaparancsfájl hoz létre SQL rugalmas készletek, készletezett adatbázisok áthelyezése, és a rugalmas készlet teljesítményszintet vált.</span><span class="sxs-lookup"><span data-stu-id="adbd9-104">This Azure CLI script example creates SQL elastic pools, moves pooled databases, and changes elastic pool performance levels.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="adbd9-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="adbd9-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="adbd9-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="adbd9-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="adbd9-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="adbd9-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="adbd9-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="adbd9-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/scale-pool/scale-pool.sh "Move database between pools")]

## <a name="clean-up-deployment"></a><span data-ttu-id="adbd9-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="adbd9-109">Clean up deployment</span></span>

<span data-ttu-id="adbd9-110">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="adbd9-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="adbd9-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="adbd9-111">Script explanation</span></span>

<span data-ttu-id="adbd9-112">A parancsfájl a következő parancsok toocreate erőforráscsoport, a logikai kiszolgáló, az SQL-adatbázis és a tűzfalszabályok hello.</span><span class="sxs-lookup"><span data-stu-id="adbd9-112">This script uses hello following commands toocreate a resource group, logical server, SQL Database, and firewall rules.</span></span> <span data-ttu-id="adbd9-113">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="adbd9-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="adbd9-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="adbd9-114">Command</span></span> | <span data-ttu-id="adbd9-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="adbd9-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="adbd9-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="adbd9-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="adbd9-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="adbd9-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="adbd9-118">az sql-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="adbd9-118">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="adbd9-119">Hogy a gazdagépek hello SQL Database logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="adbd9-119">Creates a logical server that hosts hello SQL Database.</span></span> |
| [<span data-ttu-id="adbd9-120">az sql rugalmas-címkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="adbd9-120">az sql elastic-pools create</span></span>](https://docs.microsoft.com/cli/azure/sql/elastic-pool#create) | <span data-ttu-id="adbd9-121">A rugalmas adatbáziskészlet hello logikai kiszolgáló belül hoz létre.</span><span class="sxs-lookup"><span data-stu-id="adbd9-121">Creates an elastic database pool within hello logical server.</span></span> |
| [<span data-ttu-id="adbd9-122">az sql-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="adbd9-122">az sql db create</span></span>](https://docs.microsoft.com/cli/azure/sql/db#create) | <span data-ttu-id="adbd9-123">Hello SQL Database logikai kiszolgáló hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="adbd9-123">Creates hello SQL Database in hello logical server.</span></span> |
| [<span data-ttu-id="adbd9-124">az sql rugalmas-gyűjtők frissítése</span><span class="sxs-lookup"><span data-stu-id="adbd9-124">az sql elastic-pools update</span></span>](https://docs.microsoft.com/cli/azure/sql/elastic-pool#update) | <span data-ttu-id="adbd9-125">A rugalmas adatbáziskészlet, ez például módosítások hello eDTU hozzárendelt a frissíti.</span><span class="sxs-lookup"><span data-stu-id="adbd9-125">Updates an elastic database pool, in this example changes hello assigned eDTU.</span></span> |
| [<span data-ttu-id="adbd9-126">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="adbd9-126">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="adbd9-127">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="adbd9-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="adbd9-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="adbd9-128">Next steps</span></span>

<span data-ttu-id="adbd9-129">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="adbd9-129">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="adbd9-130">További SQL-adatbázis CLI parancsfájl minták hello található [dokumentáció az Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="adbd9-130">Additional SQL Database CLI script samples can be found in hello [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>
