---
title: "Azure CLI: SQL-adatbázis létrehozása | Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy SQL Database logikai kiszolgálóhoz kiszolgálószintű tűzfalszabályt és használó adatbázisok hello Azure parancssori felület."
keywords: "oktatóanyag az SQL Database használatához, SQL-adatbázis létrehozása"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
ms.openlocfilehash: 9b1ffb17eabeb70a000ff0c997128832b07aa4fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-single-azure-sql-database-using-hello-azure-cli"></a><span data-ttu-id="d4bdc-104">Hozzon létre egy Azure SQL-adatbázis hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="d4bdc-104">Create a single Azure SQL database using hello Azure CLI</span></span>

<span data-ttu-id="d4bdc-105">hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-105">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="d4bdc-106">Ez útmutató segítségével hello Azure CLI toodeploy részletei az Azure SQL-adatbázis egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) a egy [Azure SQL Database logikai kiszolgáló](sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="d4bdc-106">This guide details using hello Azure CLI toodeploy an Azure SQL database in an [Azure resource group](../azure-resource-manager/resource-group-overview.md) in an [Azure SQL Database logical server](sql-database-features.md).</span></span>

<span data-ttu-id="d4bdc-107">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d4bdc-108">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez a témakör van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="d4bdc-109">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="d4bdc-110">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d4bdc-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="define-variables"></a><span data-ttu-id="d4bdc-111">Változók meghatározása</span><span class="sxs-lookup"><span data-stu-id="d4bdc-111">Define variables</span></span>

<span data-ttu-id="d4bdc-112">A gyors üzembe helyezési parancsfájlok hello változókat ad meg.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-112">Define variables for use in hello scripts in this quick start.</span></span>

```azurecli-interactive
# hello data center and resource name for your resources
export resourcegroupname = myResourceGroup
export location = westeurope
# hello logical server name: Use a random value or replace with your own value (do not capitalize)
export servername = server-$RANDOM
# Set an admin login and password for your database
export adminlogin = ServerAdmin
export password = ChangeYourAdminPassword1
# hello ip address range that you want tooallow tooaccess your DB
export startip = "0.0.0.0"
export endip = "0.0.0.0"
# hello database name
export databasename = mySampleDatabase
```

## <a name="create-a-resource-group"></a><span data-ttu-id="d4bdc-113">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="d4bdc-113">Create a resource group</span></span>

<span data-ttu-id="d4bdc-114">Hozzon létre egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hello segítségével [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-114">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="d4bdc-115">Az erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-115">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="d4bdc-116">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westeurope` helyét.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-116">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` location.</span></span>

```azurecli-interactive
az group create --name $resourcegroupname --location $location
```
## <a name="create-a-logical-server"></a><span data-ttu-id="d4bdc-117">Hozzon létre egy logikai kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="d4bdc-117">Create a logical server</span></span>

<span data-ttu-id="d4bdc-118">Hozzon létre egy [Azure SQL Database logikai kiszolgáló](sql-database-features.md) hello segítségével [létrehozása az sql server](/cli/azure/sql/server#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-118">Create an [Azure SQL Database logical server](sql-database-features.md) using hello [az sql server create](/cli/azure/sql/server#create) command.</span></span> <span data-ttu-id="d4bdc-119">A logikai kiszolgálók adatbázisok egy csoportját tartalmazzák, amelyeket a rendszer egy csoportként kezel.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-119">A logical server contains a group of databases managed as a group.</span></span> <span data-ttu-id="d4bdc-120">hello alábbi példa hoz létre egy véletlenszerűen előállított nevű kiszolgálót az erőforráscsoportban egy rendszergazdai bejelentkezés nevű `ServerAdmin` és egy jelszót `ChangeYourAdminPassword1`.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-120">hello following example creates a randomly named server in your resource group with an admin login named `ServerAdmin` and a password of `ChangeYourAdminPassword1`.</span></span> <span data-ttu-id="d4bdc-121">Igény szerint cserélje le ezeket az előre meghatározott értékeket.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-121">Replace these pre-defined values as desired.</span></span>

```azurecli-interactive
az sql server create --name $servername --resource-group $resourcegroupname --location $location \
    --admin-user $adminlogin --admin-password $password
```

## <a name="configure-a-server-firewall-rule"></a><span data-ttu-id="d4bdc-122">Konfiguráljon egy kiszolgálói tűzfalszabályt</span><span class="sxs-lookup"><span data-stu-id="d4bdc-122">Configure a server firewall rule</span></span>

<span data-ttu-id="d4bdc-123">Hozzon létre egy [Azure SQL Database kiszolgálószintű tűzfalszabály](sql-database-firewall-configure.md) hello segítségével [létrehozása az sql server tűzfal](/cli/azure/sql/server/firewall-rule#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-123">Create an [Azure SQL Database server-level firewall rule](sql-database-firewall-configure.md) using hello [az sql server firewall create](/cli/azure/sql/server/firewall-rule#create) command.</span></span> <span data-ttu-id="d4bdc-124">Egy kiszolgálószintű tűzfalszabályt lehetővé teszi, hogy a külső alkalmazás, például az SQL Server Management Studio vagy hello SQLCMD segédprogram tooconnect tooa SQL-adatbázis hello SQL Database szolgáltatás tűzfalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-124">A server-level firewall rule allows an external application, such as SQL Server Management Studio or hello SQLCMD utility tooconnect tooa SQL database through hello SQL Database service firewall.</span></span> <span data-ttu-id="d4bdc-125">A következő példa hello hello tűzfal van csak megnyitva más Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-125">In hello following example, hello firewall is only opened for other Azure resources.</span></span> <span data-ttu-id="d4bdc-126">külső kapcsolatot tooenable, módosítás hello IP-cím tooan megfelelő címet a környezetben.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-126">tooenable external connectivity, change hello IP address tooan appropriate address for your environment.</span></span> <span data-ttu-id="d4bdc-127">tooopen összes IP-cím, IP-cím és 255.255.255.255 indítása zárócíme hello hello 0.0.0.0 használják.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-127">tooopen all IP addresses, use 0.0.0.0 as hello starting IP address and 255.255.255.255 as hello ending address.</span></span>  

```azurecli-interactive
az sql server firewall-rule create --resource-group $resourcegroupname --server $servername \
    -n AllowYourIp --start-ip-address $startip --end-ip-address $endip
```

> [!NOTE]
> <span data-ttu-id="d4bdc-128">Az SQL Database az 1433-as porton kommunikál.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-128">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="d4bdc-129">Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a 1433-as port kimenő forgalmát.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-129">If you are trying tooconnect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="d4bdc-130">Ha igen, csak akkor tudja tooconnect tooyour Azure SQL adatbázis-kiszolgáló kivéve, ha az IT-részleg megnyitja az 1433-as port.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-130">If so, you will not be able tooconnect tooyour Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="create-a-database-in-hello-server-with-sample-data"></a><span data-ttu-id="d4bdc-131">Hozzon létre egy adatbázist mintaadatokkal hello kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="d4bdc-131">Create a database in hello server with sample data</span></span>

<span data-ttu-id="d4bdc-132">Hozzon létre egy adatbázist, egy [S0 teljesítményszint szükséges](sql-database-service-tiers.md) hello segítségével hello Server [az sql-adatbázis létrehozása](/cli/azure/sql/db#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-132">Create a database with an [S0 performance level](sql-database-service-tiers.md) in hello server using hello [az sql db create](/cli/azure/sql/db#create) command.</span></span> <span data-ttu-id="d4bdc-133">hello alábbi példa létrehoz egy nevű adatbázist `mySampleDatabase` és terhelések hello AdventureWorksLT mintaadatokat az adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-133">hello following example creates a database called `mySampleDatabase` and loads hello AdventureWorksLT sample data into this database.</span></span> <span data-ttu-id="d4bdc-134">Cserélje le ezeket az előre megadott értékek a kívánt módon (a gyűjtemény build a gyors üzembe helyezési hello értékek esetén más gyors üzembe helyezések).</span><span class="sxs-lookup"><span data-stu-id="d4bdc-134">Replace these predefined values as desired (other quick starts in this collection build upon hello values in this quick start).</span></span>

```azurecli-interactive
az sql db create --resource-group $resourcegroupname --server $servername \
    --name $databasename --sample-name AdventureWorksLT --service-objective S0
```

## <a name="clean-up-resources"></a><span data-ttu-id="d4bdc-135">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d4bdc-135">Clean up resources</span></span>

<span data-ttu-id="d4bdc-136">Az ebben a gyűjteményben lévő többi rövid útmutató erre a rövid útmutatóra épít.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-136">Other quick starts in this collection build upon this quick start.</span></span> 

> [!TIP]
> <span data-ttu-id="d4bdc-137">Ha az ezt követő gyors üzembe helyezések toowork toocontinue tervezi, karbantartása hello erőforrások létrehozása a gyors indításának mellőzése.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-137">If you plan toocontinue on toowork with subsequent quick starts, do not clean up hello resources created in this quick start.</span></span> <span data-ttu-id="d4bdc-138">Ha nem tervezi toocontinue, használja a hello által a hello Azure-portálon a gyors üzembe helyezési lépéseket toodelete összes erőforrás létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-138">If you do not plan toocontinue, use hello following steps toodelete all resources created by this quick start in hello Azure portal.</span></span>
>

```azurecli-interactive
az group delete --name $resourcegroupname
```

## <a name="next-steps"></a><span data-ttu-id="d4bdc-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d4bdc-139">Next steps</span></span>

<span data-ttu-id="d4bdc-140">Most, hogy rendelkezik egy adatbázissal, csatlakoztathatja a kedvenc eszközeit, és lekérdezéseket hajthat végre velük.</span><span class="sxs-lookup"><span data-stu-id="d4bdc-140">Now that you have a database, you can connect and query using your favorite tools.</span></span> <span data-ttu-id="d4bdc-141">További információkért válassza ki az eszközt az alábbiak közül:</span><span class="sxs-lookup"><span data-stu-id="d4bdc-141">Learn more by choosing your tool below:</span></span>

- [<span data-ttu-id="d4bdc-142">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="d4bdc-142">SQL Server Management Studio</span></span>](sql-database-connect-query-ssms.md)
- [<span data-ttu-id="d4bdc-143">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d4bdc-143">Visual Studio Code</span></span>](sql-database-connect-query-vscode.md)
- [<span data-ttu-id="d4bdc-144">.NET</span><span class="sxs-lookup"><span data-stu-id="d4bdc-144">.NET</span></span>](sql-database-connect-query-dotnet.md)
- [<span data-ttu-id="d4bdc-145">PHP</span><span class="sxs-lookup"><span data-stu-id="d4bdc-145">PHP</span></span>](sql-database-connect-query-php.md)
- [<span data-ttu-id="d4bdc-146">Node.js</span><span class="sxs-lookup"><span data-stu-id="d4bdc-146">Node.js</span></span>](sql-database-connect-query-nodejs.md)
- [<span data-ttu-id="d4bdc-147">Java</span><span class="sxs-lookup"><span data-stu-id="d4bdc-147">Java</span></span>](sql-database-connect-query-java.md)
- [<span data-ttu-id="d4bdc-148">Python</span><span class="sxs-lookup"><span data-stu-id="d4bdc-148">Python</span></span>](sql-database-connect-query-python.md)
- [<span data-ttu-id="d4bdc-149">Ruby</span><span class="sxs-lookup"><span data-stu-id="d4bdc-149">Ruby</span></span>](sql-database-connect-query-ruby.md)

