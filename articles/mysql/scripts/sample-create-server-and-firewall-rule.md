---
title: "Az Azure CLI Script – a MySQL az Azure-adatbázis létrehozása |} Microsoft Docs"
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
ms.openlocfilehash: 201db294ce362ef3e09cbe62f48bd51c8ea94dbb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-mysql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a><span data-ttu-id="42b4a-103">A MySQL-kiszolgáló létrehozása és konfigurálása egy tűzfalszabályt, az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="42b4a-103">Create a MySQL server and configure a firewall rule using the Azure CLI</span></span>
<span data-ttu-id="42b4a-104">Ez a parancsfájlpélda CLI adatbázist hoz létre Azure MySQL-kiszolgáló, és konfigurálja egy kiszolgálószintű tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="42b4a-104">This sample CLI script creates an Azure Database for MySQL server and configures a server-level firewall rule.</span></span> <span data-ttu-id="42b4a-105">Miután a parancsfájl sikeresen fut, a MySQL-kiszolgáló elérhető-e az összes Azure-szolgáltatások és a konfigurált IP-cím.</span><span class="sxs-lookup"><span data-stu-id="42b4a-105">Once the script runs successfully, the MySQL server is accessible by all Azure services and the configured IP address.</span></span>

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="42b4a-106">Ha a parancssori felület helyi telepítése és használata mellett dönt, a témakörben leírt lépésekhez az Azure parancssori felületének 2.0-s vagy annál újabb verzióját kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="42b4a-106">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="42b4a-107">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="42b4a-107">Run `az --version` to find the version.</span></span> <span data-ttu-id="42b4a-108">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="42b4a-108">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="42b4a-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="42b4a-109">Sample script</span></span>
<span data-ttu-id="42b4a-110">Ez a parancsfájlpélda szerkeszteni a kijelölt sorok testre szabhatja a rendszergazda felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="42b4a-110">In this sample script, edit the highlighted lines to customize the admin username and password.</span></span>
<span data-ttu-id="42b4a-111">[!code-azurecli-interactive[fő](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/create-mysql-server-and-firewall-rule.sh?highlight=15-16 "Azure-adatbázis létrehozása a MySQL és a kiszolgálószintű tűzfalszabály.")]</span><span class="sxs-lookup"><span data-stu-id="42b4a-111">[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/create-mysql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for MySQL, and server-level firewall rule.")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="42b4a-112">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="42b4a-112">Clean up deployment</span></span>
<span data-ttu-id="42b4a-113">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot és a vele társított összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="42b4a-113">After the script sample has been run, the following command can be used to remove the resource group and all resources associated with it.</span></span>
<span data-ttu-id="42b4a-114">[!code-azurecli-interactive[fő](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/delete-mysql.sh "az erőforráscsoportot.")]</span><span class="sxs-lookup"><span data-stu-id="42b4a-114">[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/delete-mysql.sh "Delete the resource group.")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="42b4a-115">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="42b4a-115">Script explanation</span></span>
<span data-ttu-id="42b4a-116">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="42b4a-116">This script uses the following commands.</span></span> <span data-ttu-id="42b4a-117">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="42b4a-117">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="42b4a-118">**A parancs**</span><span class="sxs-lookup"><span data-stu-id="42b4a-118">**Command**</span></span> | <span data-ttu-id="42b4a-119">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="42b4a-119">**Notes**</span></span> |
|---|---|
| [<span data-ttu-id="42b4a-120">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="42b4a-120">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="42b4a-121">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="42b4a-121">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="42b4a-122">az mysql-kiszolgáló létrehozása</span><span class="sxs-lookup"><span data-stu-id="42b4a-122">az mysql server create</span></span>](/cli/azure/mysql/server#create) | <span data-ttu-id="42b4a-123">Az adatbázisokat üzemeltető MySQL kiszolgáló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="42b4a-123">Creates a MySQL server that hosts the databases.</span></span> |
| [<span data-ttu-id="42b4a-124">az mysql kiszolgáló tűzfal létrehozása</span><span class="sxs-lookup"><span data-stu-id="42b4a-124">az mysql server firewall create</span></span>](/cli/azure/mysql/server/firewall-rule#create) | <span data-ttu-id="42b4a-125">Létrehoz egy tűzfalszabály engedélyezése a megadott IP-címtartomány a kiszolgáló és a tartozó adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="42b4a-125">Creates a firewall rule to allow access to the server and databases under it from the entered IP address range.</span></span> |
| [<span data-ttu-id="42b4a-126">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="42b4a-126">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="42b4a-127">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="42b4a-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="42b4a-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42b4a-128">Next steps</span></span>
- <span data-ttu-id="42b4a-129">További információt az Azure parancssori felület: [Azure CLI dokumentáció](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="42b4a-129">Read more information on the Azure CLI: [Azure CLI documentation](/cli/azure/overview).</span></span>
- <span data-ttu-id="42b4a-130">Próbálja meg a további parancsfájlok: [MySQL az Azure-adatbázis Azure CLI-minták](../sample-scripts-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="42b4a-130">Try additional scripts: [Azure CLI samples for Azure Database for MySQL](../sample-scripts-azure-cli.md)</span></span>
