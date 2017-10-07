---
title: "aaaCLI példa-figyelő-skálázási-egyetlen Azure SQL adatbázis |} Microsoft Docs"
description: "Az Azure CLI példa parancsfájl toomonitor és a skála egy Azure SQL-adatbázis"
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
ms.openlocfilehash: 36031ddd46a947a80fe37884858a84eb66217270
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-toomonitor-and-scale-a-single-sql-database"></a><span data-ttu-id="9ccd2-103">Parancssori felület toomonitor használja, és egy SQL-adatbázis méretezése</span><span class="sxs-lookup"><span data-stu-id="9ccd2-103">Use CLI toomonitor and scale a single SQL database</span></span>

<span data-ttu-id="9ccd2-104">Az Azure CLI mintaparancsfájl Azure SQL adatbázis tooa más-más teljesítménybeli egyszintű arányosan után hello információi hello adatbázis lekérdezésére.</span><span class="sxs-lookup"><span data-stu-id="9ccd2-104">This Azure CLI script example scales a single Azure SQL database tooa different performance level after querying hello size information of hello database.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="9ccd2-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="9ccd2-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="9ccd2-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="9ccd2-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="9ccd2-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9ccd2-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="9ccd2-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="9ccd2-108">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale single SQL Database")]

## <a name="clean-up-deployment"></a><span data-ttu-id="9ccd2-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="9ccd2-109">Clean up deployment</span></span>

<span data-ttu-id="9ccd2-110">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="9ccd2-110">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="9ccd2-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="9ccd2-111">Script explanation</span></span>

<span data-ttu-id="9ccd2-112">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="9ccd2-112">This script uses hello following commands.</span></span> <span data-ttu-id="9ccd2-113">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="9ccd2-113">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="9ccd2-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="9ccd2-114">Command</span></span> | <span data-ttu-id="9ccd2-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="9ccd2-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9ccd2-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ccd2-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="9ccd2-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9ccd2-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="9ccd2-118">az sql-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ccd2-118">az sql server create</span></span>](https://docs.microsoft.com/cli/azure/sql/server#create) | <span data-ttu-id="9ccd2-119">Egy adatbázist tároló logikai kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9ccd2-119">Creates a logical server that hosts a database.</span></span> |
| [<span data-ttu-id="9ccd2-120">az sql db megjelenítése-használat</span><span class="sxs-lookup"><span data-stu-id="9ccd2-120">az sql db show-usage</span></span>](https://docs.microsoft.com/cli/azure/sql/db#show-usage) | <span data-ttu-id="9ccd2-121">Az adatbázis hello mérete használati információit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9ccd2-121">Shows hello size usage information for a database.</span></span> |
| [<span data-ttu-id="9ccd2-122">az sql-adatbázis frissítése</span><span class="sxs-lookup"><span data-stu-id="9ccd2-122">az sql db update</span></span>](https://docs.microsoft.com/cli/azure/sql/db#update) | <span data-ttu-id="9ccd2-123">Frissíti az adatbázis-tulajdonságai (például hello szolgáltatási szint vagy a teljesítmény szint), vagy azokat, kívüli vagy rugalmas készletek között adatbázis helyezi.</span><span class="sxs-lookup"><span data-stu-id="9ccd2-123">Updates database properties (such as hello service tier or performance level) or moves a database into, out of, or between elastic pools.</span></span> |
| [<span data-ttu-id="9ccd2-124">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="9ccd2-124">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="9ccd2-125">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="9ccd2-125">Deletes a resource group including all nested resources.</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="9ccd2-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ccd2-126">Next steps</span></span>

<span data-ttu-id="9ccd2-127">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9ccd2-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="9ccd2-128">További SQL-adatbázis CLI parancsfájl minták hello található [dokumentáció az Azure SQL Database](../sql-database-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9ccd2-128">Additional SQL Database CLI script samples can be found in hello [Azure SQL Database documentation](../sql-database-cli-samples.md).</span></span>
