---
title: "parancssori felület aaaAzure minták tooscale egy MySQL-kiszolgálóhoz tartozó Azure-adatbázis |} Microsoft Docs"
description: "A parancsfájlpéldát CLI Azure-adatbázis méretezi a MySQL server tooa különböző teljesítményszintet hello metrikák lekérdezése után."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 05/31/2017
ms.openlocfilehash: 721ef9db35a5f3be7a38438c1abb724187b18b75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a><span data-ttu-id="f2c41-103">Figyelheti és az Azure parancssori felület használatával MySQL kiszolgálóhoz tartozó Azure-adatbázis méretezése</span><span class="sxs-lookup"><span data-stu-id="f2c41-103">Monitor and scale an Azure Database for MySQL server using Azure CLI</span></span>
<span data-ttu-id="f2c41-104">Ez a parancsfájlpélda CLI MySQL server tooa különböző teljesítményszintet egy Azure-adatbázis arányosan hello metrikák lekérdezése után.</span><span class="sxs-lookup"><span data-stu-id="f2c41-104">This sample CLI script scales a single Azure Database for MySQL server tooa different performance level after querying hello metrics.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="f2c41-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="f2c41-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="f2c41-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="f2c41-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="f2c41-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f2c41-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="f2c41-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="f2c41-108">Sample script</span></span>
<span data-ttu-id="f2c41-109">Ez a parancsfájlpélda módosítsa a kijelölt hello sorok toocustomize hello rendszergazda felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="f2c41-109">In this sample script, change hello highlighted lines toocustomize hello admin username and password.</span></span> <span data-ttu-id="f2c41-110">Hello az figyelő-parancsokat a saját előfizetés-azonosító szerepel hello előfizetés-azonosító cseréje.[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]</span><span class="sxs-lookup"><span data-stu-id="f2c41-110">Replace hello subscription id used in hello az monitor commands with your own subscription id. [!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="f2c41-111">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f2c41-111">Clean up deployment</span></span>
<span data-ttu-id="f2c41-112">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="f2c41-112">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete hello resource group.")]

## <a name="script-explanation"></a><span data-ttu-id="f2c41-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="f2c41-113">Script explanation</span></span>
<span data-ttu-id="f2c41-114">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="f2c41-114">This script uses hello following commands.</span></span> <span data-ttu-id="f2c41-115">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="f2c41-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="f2c41-116">**A parancs**</span><span class="sxs-lookup"><span data-stu-id="f2c41-116">**Command**</span></span> | <span data-ttu-id="f2c41-117">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="f2c41-117">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="f2c41-118">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2c41-118">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="f2c41-119">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f2c41-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f2c41-120">az mysql-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2c41-120">az mysql server create</span></span>](/cli/azure/mysql/server#create) | <span data-ttu-id="f2c41-121">Létrehoz egy MySQL hello adatbázisokat üzemeltető kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="f2c41-121">Creates a MySQL server that hosts hello databases.</span></span> |
| [<span data-ttu-id="f2c41-122">az a figyelő metrikák listája</span><span class="sxs-lookup"><span data-stu-id="f2c41-122">az monitor metrics list</span></span>](/cli/azure/monitor/metrics#list) | <span data-ttu-id="f2c41-123">Hello metrika listaértéket hello erőforrások.</span><span class="sxs-lookup"><span data-stu-id="f2c41-123">List hello metric value for hello resources.</span></span> |
| [<span data-ttu-id="f2c41-124">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="f2c41-124">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="f2c41-125">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="f2c41-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f2c41-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f2c41-126">Next steps</span></span>
- <span data-ttu-id="f2c41-127">További információt az Azure CLI hello: [Azure CLI dokumentáció](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f2c41-127">Read more information on hello Azure CLI: [Azure CLI documentation](/cli/azure/overview).</span></span>
- <span data-ttu-id="f2c41-128">Próbálja meg a további parancsfájlok: [MySQL az Azure-adatbázis Azure CLI-minták](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="f2c41-128">Try additional scripts: [Azure CLI samples for Azure Database for MySQL](../sample-scripts-azure-cli.md)</span></span>
- <span data-ttu-id="f2c41-129">A méretezés további információkért lásd: [szolgáltatásszintek](../concepts-service-tiers.md) és [egységek számítási és tárolási egység](../concepts-compute-unit-and-storage.md).</span><span class="sxs-lookup"><span data-stu-id="f2c41-129">For more information on scaling, see [Service Tiers](../concepts-service-tiers.md) and [Compute Units and Storage Units](../concepts-compute-unit-and-storage.md).</span></span>
