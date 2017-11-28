---
title: "AAA \"Azure CLI parancsfájl méretű Azure adatbázis a PostgreSQL |} Microsoft dokumentumok\""
description: "Az Azure CLI parancsfájl minta - skálázási Azure adatbázis PostgreSQL server tooa más-más teljesítménybeli szint hello metrikák lekérdezése után."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 05/31/2017
ms.openlocfilehash: 678b28941dbb4334cb374d4888991a00b44966b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a><span data-ttu-id="d4715-103">Figyelheti és méretezhető egy PostgreSQL egykiszolgálós Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="d4715-103">Monitor and scale a single PostgreSQL server using Azure CLI</span></span>
<span data-ttu-id="d4715-104">Ez a parancsfájlpélda CLI PostgreSQL server tooa különböző teljesítményszintet egy Azure-adatbázis arányosan hello metrikák lekérdezése után.</span><span class="sxs-lookup"><span data-stu-id="d4715-104">This sample CLI script scales a single Azure Database for PostgreSQL server tooa different performance level after querying hello metrics.</span></span> 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d4715-105">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d4715-105">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="d4715-106">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="d4715-106">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="d4715-107">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d4715-107">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="d4715-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="d4715-108">Sample script</span></span>
<span data-ttu-id="d4715-109">Ez a parancsfájlpélda módosítsa a kijelölt hello sorok toocustomize hello rendszergazda felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="d4715-109">In this sample script, change hello highlighted lines toocustomize hello admin username and password.</span></span> <span data-ttu-id="d4715-110">Hello az figyelő-parancsokat a saját előfizetés-azonosító szerepel hello előfizetés-azonosító cseréje.[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]</span><span class="sxs-lookup"><span data-stu-id="d4715-110">Replace hello subscription id used in hello az monitor commands with your own subscription id. [!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="d4715-111">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d4715-111">Clean up deployment</span></span>
<span data-ttu-id="d4715-112">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d4715-112">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete hello resource group.")]

## <a name="script-explanation"></a><span data-ttu-id="d4715-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="d4715-113">Script explanation</span></span>
<span data-ttu-id="d4715-114">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="d4715-114">This script uses hello following commands.</span></span> <span data-ttu-id="d4715-115">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="d4715-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="d4715-116">**A parancs**</span><span class="sxs-lookup"><span data-stu-id="d4715-116">**Command**</span></span> | <span data-ttu-id="d4715-117">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="d4715-117">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="d4715-118">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="d4715-118">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="d4715-119">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d4715-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d4715-120">az postgres kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="d4715-120">az postgres server create</span></span>](/cli/azure/postgres/server#create) | <span data-ttu-id="d4715-121">Létrehoz egy PostgreSQL hello adatbázisokat üzemeltető kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="d4715-121">Creates a PostgreSQL server that hosts hello databases.</span></span> |
| [<span data-ttu-id="d4715-122">az a figyelő metrikák listája</span><span class="sxs-lookup"><span data-stu-id="d4715-122">az monitor metrics list</span></span>](/cli/azure/monitor/metrics#list) | <span data-ttu-id="d4715-123">Hello metrika listaértéket hello erőforrások.</span><span class="sxs-lookup"><span data-stu-id="d4715-123">List hello metric value for hello resources.</span></span> |
| [<span data-ttu-id="d4715-124">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="d4715-124">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="d4715-125">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="d4715-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d4715-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d4715-126">Next steps</span></span>
- <span data-ttu-id="d4715-127">További információt az Azure CLI hello: [Azure CLI-dokumentáció](/cli/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="d4715-127">Read more information on hello Azure CLI: [Azure CLI documentation](/cli/azure/overview)</span></span>
- <span data-ttu-id="d4715-128">Próbálja meg a további parancsfájlok: [PostgreSQL Azure-adatbázis Azure CLI-minták](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="d4715-128">Try additional scripts: [Azure CLI samples for Azure Database for PostgreSQL](../sample-scripts-azure-cli.md)</span></span>
- <span data-ttu-id="d4715-129">További információt a méretezés: [szolgáltatásszintek](../concepts-service-tiers.md) és [egységek számítási és tárolási egység](../concepts-compute-unit-and-storage.md)</span><span class="sxs-lookup"><span data-stu-id="d4715-129">Read more information on scaling: [Service Tiers](../concepts-service-tiers.md) and [Compute Units and Storage Units](../concepts-compute-unit-and-storage.md)</span></span>
