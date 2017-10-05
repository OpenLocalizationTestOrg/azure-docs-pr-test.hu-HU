---
title: "Az Azure CLI parancsfájl méretű Azure adatbázis a PostgreSQL |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - skálázási Azure adatbázis PostgreSQL-kiszolgáló a metrikák lekérdezése után más-más teljesítménybeli szintjét."
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
ms.openlocfilehash: b847abb336cce5dd5516469dca58002d3ba265f0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a><span data-ttu-id="277bf-103">Figyelheti és méretezhető egy PostgreSQL egykiszolgálós Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="277bf-103">Monitor and scale a single PostgreSQL server using Azure CLI</span></span>
<span data-ttu-id="277bf-104">Ez a parancsfájlpélda CLI PostgreSQL-kiszolgáló egy másik teljesítményszintre egy Azure-adatbázis méretezi a metrikák lekérdezése után.</span><span class="sxs-lookup"><span data-stu-id="277bf-104">This sample CLI script scales a single Azure Database for PostgreSQL server to a different performance level after querying the metrics.</span></span> 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="277bf-105">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="277bf-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="277bf-106">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="277bf-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="277bf-107">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="277bf-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="277bf-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="277bf-108">Sample script</span></span>
<span data-ttu-id="277bf-109">Ez a parancsfájlpélda módosítsa a kijelölt sorok testre szabhatja a rendszergazda felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="277bf-109">In this sample script, change the highlighted lines to customize the admin username and password.</span></span> <span data-ttu-id="277bf-110">Cserélje le a saját előfizetés-azonosító az figyelő utasítással használt előfizetés-azonosító. [!code-azurecli-interactive [fő](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "létrehozása és a skála PostgreSQL az Azure-adatbázis.")]</span><span class="sxs-lookup"><span data-stu-id="277bf-110">Replace the subscription id used in the az monitor commands with your own subscription id. [!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="277bf-111">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="277bf-111">Clean up deployment</span></span>
<span data-ttu-id="277bf-112">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="277bf-112">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>
<span data-ttu-id="277bf-113">[!code-azurecli-interactive[fő](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "az erőforráscsoportot.")]</span><span class="sxs-lookup"><span data-stu-id="277bf-113">[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete the resource group.")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="277bf-114">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="277bf-114">Script explanation</span></span>
<span data-ttu-id="277bf-115">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="277bf-115">This script uses the following commands.</span></span> <span data-ttu-id="277bf-116">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="277bf-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="277bf-117">**A parancs**</span><span class="sxs-lookup"><span data-stu-id="277bf-117">**Command**</span></span> | <span data-ttu-id="277bf-118">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="277bf-118">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="277bf-119">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="277bf-119">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="277bf-120">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="277bf-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="277bf-121">az postgres kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="277bf-121">az postgres server create</span></span>](/cli/azure/postgres/server#create) | <span data-ttu-id="277bf-122">Az adatbázisokat üzemeltető PostgreSQL-kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="277bf-122">Creates a PostgreSQL server that hosts the databases.</span></span> |
| [<span data-ttu-id="277bf-123">az a figyelő metrikák listája</span><span class="sxs-lookup"><span data-stu-id="277bf-123">az monitor metrics list</span></span>](/cli/azure/monitor/metrics#list) | <span data-ttu-id="277bf-124">Listázza az erőforrásokra vonatkozó Átjárómetrika értékeként.</span><span class="sxs-lookup"><span data-stu-id="277bf-124">List the metric value for the resources.</span></span> |
| [<span data-ttu-id="277bf-125">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="277bf-125">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="277bf-126">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="277bf-126">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="277bf-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="277bf-127">Next steps</span></span>
- <span data-ttu-id="277bf-128">További információt az Azure parancssori felület: [Azure CLI-dokumentáció](/cli/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="277bf-128">Read more information on the Azure CLI: [Azure CLI documentation](/cli/azure/overview)</span></span>
- <span data-ttu-id="277bf-129">Próbálja meg a további parancsfájlok: [PostgreSQL Azure-adatbázis Azure CLI-minták](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="277bf-129">Try additional scripts: [Azure CLI samples for Azure Database for PostgreSQL](../sample-scripts-azure-cli.md)</span></span>
- <span data-ttu-id="277bf-130">További információt a méretezés: [szolgáltatásszintek](../concepts-service-tiers.md) és [egységek számítási és tárolási egység](../concepts-compute-unit-and-storage.md)</span><span class="sxs-lookup"><span data-stu-id="277bf-130">Read more information on scaling: [Service Tiers](../concepts-service-tiers.md) and [Compute Units and Storage Units](../concepts-compute-unit-and-storage.md)</span></span>
