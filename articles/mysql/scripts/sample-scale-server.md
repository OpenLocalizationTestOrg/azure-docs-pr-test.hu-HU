---
title: "Egy MySQL-kiszolgálóhoz tartozó Azure-adatbázis méretezése Azure CLI-minták |} Microsoft Docs"
description: "Ez a parancsfájlpélda CLI Azure Database MySQL-kiszolgáló egy másik teljesítményszintre méretezi a metrikák lekérdezése után."
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
ms.openlocfilehash: 33316ff3db382d25a444d55772c6ee4d7b7ac418
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a><span data-ttu-id="ce1c8-103">Figyelheti és az Azure parancssori felület használatával MySQL kiszolgálóhoz tartozó Azure-adatbázis méretezése</span><span class="sxs-lookup"><span data-stu-id="ce1c8-103">Monitor and scale an Azure Database for MySQL server using Azure CLI</span></span>
<span data-ttu-id="ce1c8-104">Ez a parancsfájlpélda CLI méretezi a MySQL-kiszolgáló egy másik teljesítményszintre egy Azure-adatbázis a metrikák lekérdezése után.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-104">This sample CLI script scales a single Azure Database for MySQL server to a different performance level after querying the metrics.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ce1c8-105">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-105">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="ce1c8-106">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-106">Run `az --version` to find the version.</span></span> <span data-ttu-id="ce1c8-107">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ce1c8-107">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="ce1c8-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="ce1c8-108">Sample script</span></span>
<span data-ttu-id="ce1c8-109">Ez a parancsfájlpélda módosítsa a kijelölt sorok testre szabhatja a rendszergazda felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-109">In this sample script, change the highlighted lines to customize the admin username and password.</span></span> <span data-ttu-id="ce1c8-110">Cserélje le a saját előfizetés-azonosító az figyelő utasítással használt előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-110">Replace the subscription id used in the az monitor commands with your own subscription id.</span></span>
<span data-ttu-id="ce1c8-111">[!code-azurecli-interactive[fő](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "létrehozása és a skála MySQL az Azure-adatbázis.")]</span><span class="sxs-lookup"><span data-stu-id="ce1c8-111">[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="ce1c8-112">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="ce1c8-112">Clean up deployment</span></span>
<span data-ttu-id="ce1c8-113">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-113">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>
<span data-ttu-id="ce1c8-114">[!code-azurecli-interactive[fő](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "az erőforráscsoportot.")]</span><span class="sxs-lookup"><span data-stu-id="ce1c8-114">[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete the resource group.")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="ce1c8-115">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="ce1c8-115">Script explanation</span></span>
<span data-ttu-id="ce1c8-116">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-116">This script uses the following commands.</span></span> <span data-ttu-id="ce1c8-117">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-117">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="ce1c8-118">**A parancs**</span><span class="sxs-lookup"><span data-stu-id="ce1c8-118">**Command**</span></span> | <span data-ttu-id="ce1c8-119">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="ce1c8-119">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="ce1c8-120">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce1c8-120">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="ce1c8-121">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-121">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ce1c8-122">az mysql-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce1c8-122">az mysql server create</span></span>](/cli/azure/mysql/server#create) | <span data-ttu-id="ce1c8-123">Az adatbázisokat üzemeltető MySQL kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-123">Creates a MySQL server that hosts the databases.</span></span> |
| [<span data-ttu-id="ce1c8-124">az a figyelő metrikák listája</span><span class="sxs-lookup"><span data-stu-id="ce1c8-124">az monitor metrics list</span></span>](/cli/azure/monitor/metrics#list) | <span data-ttu-id="ce1c8-125">Listázza az erőforrásokra vonatkozó Átjárómetrika értékeként.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-125">List the metric value for the resources.</span></span> |
| [<span data-ttu-id="ce1c8-126">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="ce1c8-126">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="ce1c8-127">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="ce1c8-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ce1c8-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ce1c8-128">Next steps</span></span>
- <span data-ttu-id="ce1c8-129">További információt az Azure parancssori felület: [Azure CLI dokumentáció](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ce1c8-129">Read more information on the Azure CLI: [Azure CLI documentation](/cli/azure/overview).</span></span>
- <span data-ttu-id="ce1c8-130">Próbálja meg a további parancsfájlok: [MySQL az Azure-adatbázis Azure CLI-minták](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="ce1c8-130">Try additional scripts: [Azure CLI samples for Azure Database for MySQL](../sample-scripts-azure-cli.md)</span></span>
- <span data-ttu-id="ce1c8-131">A méretezés további információkért lásd: [szolgáltatásszintek](../concepts-service-tiers.md) és [egységek számítási és tárolási egység](../concepts-compute-unit-and-storage.md).</span><span class="sxs-lookup"><span data-stu-id="ce1c8-131">For more information on scaling, see [Service Tiers](../concepts-service-tiers.md) and [Compute Units and Storage Units](../concepts-compute-unit-and-storage.md).</span></span>
