---
title: "AAA \"Azure CLI Script – a MySQL az Azure-adatbázis létrehozása |} Microsoft dokumentumok\""
description: "Ez a parancsfájlpélda CLI adatbázist hoz létre Azure MySQL-kiszolgáló, és konfigurálja egy kiszolgálószintű tűzfalszabályt."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 05/31/2017
ms.openlocfilehash: 1d619ee0547efd8275eaf7c1347b6c3427025c3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-mysql-server-and-configure-a-firewall-rule-using-hello-azure-cli"></a><span data-ttu-id="10a4f-103">MySQL-kiszolgáló létrehozása és konfigurálása a tűzfalszabályok hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="10a4f-103">Create a MySQL server and configure a firewall rule using hello Azure CLI</span></span>
<span data-ttu-id="10a4f-104">Ez a parancsfájlpélda CLI adatbázist hoz létre Azure MySQL-kiszolgáló, és konfigurálja egy kiszolgálószintű tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="10a4f-104">This sample CLI script creates an Azure Database for MySQL server and configures a server-level firewall rule.</span></span> <span data-ttu-id="10a4f-105">Miután hello parancsprogram sikeresen lefut, hello MySQL kiszolgáló elérhető-e az összes Azure-szolgáltatások és hello konfigurált IP-címet.</span><span class="sxs-lookup"><span data-stu-id="10a4f-105">Once hello script runs successfully, hello MySQL server is accessible by all Azure services and hello configured IP address.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="10a4f-106">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="10a4f-106">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="10a4f-107">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="10a4f-107">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="10a4f-108">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="10a4f-108">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="10a4f-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="10a4f-109">Sample script</span></span>
<span data-ttu-id="10a4f-110">Ez a parancsfájlpélda szerkeszteni hello kiemelt sorok toocustomize hello rendszergazda felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="10a4f-110">In this sample script, edit hello highlighted lines toocustomize hello admin username and password.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/create-mysql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for MySQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a><span data-ttu-id="10a4f-111">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="10a4f-111">Clean up deployment</span></span>
<span data-ttu-id="10a4f-112">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="10a4f-112">After hello script sample has been run, hello following command can be used tooremove hello resource group and all resources associated with it.</span></span>
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/delete-mysql.sh "Delete hello resource group.")]

## <a name="script-explanation"></a><span data-ttu-id="10a4f-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="10a4f-113">Script explanation</span></span>
<span data-ttu-id="10a4f-114">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="10a4f-114">This script uses hello following commands.</span></span> <span data-ttu-id="10a4f-115">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="10a4f-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="10a4f-116">**A parancs**</span><span class="sxs-lookup"><span data-stu-id="10a4f-116">**Command**</span></span> | <span data-ttu-id="10a4f-117">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="10a4f-117">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="10a4f-118">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="10a4f-118">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="10a4f-119">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="10a4f-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="10a4f-120">az mysql-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="10a4f-120">az mysql server create</span></span>](/cli/azure/mysql/server#create) | <span data-ttu-id="10a4f-121">Létrehoz egy MySQL hello adatbázisokat üzemeltető kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="10a4f-121">Creates a MySQL server that hosts hello databases.</span></span> |
| [<span data-ttu-id="10a4f-122">az mysql kiszolgáló tűzfal létrehozása</span><span class="sxs-lookup"><span data-stu-id="10a4f-122">az mysql server firewall create</span></span>](/cli/azure/mysql/server/firewall-rule#create) | <span data-ttu-id="10a4f-123">Hoz létre egy tűzfal szabály tooallow toohello kiszolgálót, és tartozó adatbázisok hello megadott IP-címtartományt.</span><span class="sxs-lookup"><span data-stu-id="10a4f-123">Creates a firewall rule tooallow access toohello server and databases under it from hello entered IP address range.</span></span> |
| [<span data-ttu-id="10a4f-124">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="10a4f-124">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="10a4f-125">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="10a4f-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="10a4f-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10a4f-126">Next steps</span></span>
- <span data-ttu-id="10a4f-127">További információt az Azure CLI hello: [Azure CLI dokumentáció](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="10a4f-127">Read more information on hello Azure CLI: [Azure CLI documentation](/cli/azure/overview).</span></span>
- <span data-ttu-id="10a4f-128">Próbálja meg a további parancsfájlok: [MySQL az Azure-adatbázis Azure CLI-minták](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="10a4f-128">Try additional scripts: [Azure CLI samples for Azure Database for MySQL](../sample-scripts-azure-cli.md)</span></span>
