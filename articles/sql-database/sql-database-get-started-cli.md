---
title: "Azure CLI: SQL-adatbázis létrehozása | Microsoft Docs"
description: "Ismerje meg, hogyan hozhat létre SQL Database logikai kiszolgálót, kiszolgálószintű tűzfalszabályokat és adatbázisokat az Azure Portal használatával."
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
ms.openlocfilehash: a735f7e6aa65ac36dc4e5a49c5a9a834be43d71a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-single-azure-sql-database-using-the-azure-cli"></a><span data-ttu-id="4e5f8-104">Önálló Azure SQL-adatbázis létrehozása az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="4e5f8-104">Create a single Azure SQL database using the Azure CLI</span></span>

<span data-ttu-id="4e5f8-105">Az Azure CLI az Azure-erőforrások parancssorból vagy szkriptekkel történő létrehozására és kezelésére használható.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-105">The Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span> <span data-ttu-id="4e5f8-106">Ez az útmutató azt ismerteti, hogyan helyezhet üzembe Azure SQL-adatbázist az Azure CLI használatával egy [Azure SQL Database logikai kiszolgáló](sql-database-features.md) [Azure-erőforráscsoportjában](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4e5f8-106">This guide details using the Azure CLI to deploy an Azure SQL database in an [Azure resource group](../azure-resource-manager/resource-group-overview.md) in an [Azure SQL Database logical server](sql-database-features.md).</span></span>

<span data-ttu-id="4e5f8-107">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="4e5f8-108">Ha a CLI helyi telepítését és használatát választja, akkor ehhez a témakörhöz az Azure CLI 2.0.4-es vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="4e5f8-109">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="4e5f8-110">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="4e5f8-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="define-variables"></a><span data-ttu-id="4e5f8-111">Változók meghatározása</span><span class="sxs-lookup"><span data-stu-id="4e5f8-111">Define variables</span></span>

<span data-ttu-id="4e5f8-112">Ebben a rövid útmutatóban változókat határozhat meg a szkriptekben való használatra.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-112">Define variables for use in the scripts in this quick start.</span></span>

```azurecli-interactive
# The data center and resource name for your resources
export resourcegroupname = myResourceGroup
export location = westeurope
# The logical server name: Use a random value or replace with your own value (do not capitalize)
export servername = server-$RANDOM
# Set an admin login and password for your database
export adminlogin = ServerAdmin
export password = ChangeYourAdminPassword1
# The ip address range that you want to allow to access your DB
export startip = "0.0.0.0"
export endip = "0.0.0.0"
# The database name
export databasename = mySampleDatabase
```

## <a name="create-a-resource-group"></a><span data-ttu-id="4e5f8-113">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="4e5f8-113">Create a resource group</span></span>

<span data-ttu-id="4e5f8-114">Hozzon létre egy [Azure-erőforráscsoportot](../azure-resource-manager/resource-group-overview.md) az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-114">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="4e5f8-115">Az erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-115">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="4e5f8-116">A következő példában létrehozunk egy `westeurope` nevű erőforráscsoportot a `myResourceGroup` helyen.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-116">The following example creates a resource group named `myResourceGroup` in the `westeurope` location.</span></span>

```azurecli-interactive
az group create --name $resourcegroupname --location $location
```
## <a name="create-a-logical-server"></a><span data-ttu-id="4e5f8-117">Hozzon létre egy logikai kiszolgálót</span><span class="sxs-lookup"><span data-stu-id="4e5f8-117">Create a logical server</span></span>

<span data-ttu-id="4e5f8-118">Hozzon létre egy [Azure SQL Database logikai kiszolgálót](sql-database-features.md) az [az sql server create](/cli/azure/sql/server#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-118">Create an [Azure SQL Database logical server](sql-database-features.md) using the [az sql server create](/cli/azure/sql/server#create) command.</span></span> <span data-ttu-id="4e5f8-119">A logikai kiszolgálók adatbázisok egy csoportját tartalmazzák, amelyeket a rendszer egy csoportként kezel.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-119">A logical server contains a group of databases managed as a group.</span></span> <span data-ttu-id="4e5f8-120">A következő példában létrehozunk egy véletlenszerűen elnevezett kiszolgálót az erőforráscsoportban egy `ServerAdmin` nevű és `ChangeYourAdminPassword1` jelszavú rendszergazdai bejelentkezéssel.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-120">The following example creates a randomly named server in your resource group with an admin login named `ServerAdmin` and a password of `ChangeYourAdminPassword1`.</span></span> <span data-ttu-id="4e5f8-121">Igény szerint cserélje le ezeket az előre meghatározott értékeket.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-121">Replace these pre-defined values as desired.</span></span>

```azurecli-interactive
az sql server create --name $servername --resource-group $resourcegroupname --location $location \
    --admin-user $adminlogin --admin-password $password
```

## <a name="configure-a-server-firewall-rule"></a><span data-ttu-id="4e5f8-122">Konfiguráljon egy kiszolgálói tűzfalszabályt</span><span class="sxs-lookup"><span data-stu-id="4e5f8-122">Configure a server firewall rule</span></span>

<span data-ttu-id="4e5f8-123">Hozzon létre egy [Azure SQL Database kiszolgálószintű tűzfalszabályt](sql-database-firewall-configure.md) az [az sql server firewall create](/cli/azure/sql/server/firewall-rule#create) parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-123">Create an [Azure SQL Database server-level firewall rule](sql-database-firewall-configure.md) using the [az sql server firewall create](/cli/azure/sql/server/firewall-rule#create) command.</span></span> <span data-ttu-id="4e5f8-124">A kiszolgálószintű tűzfalszabályok lehetővé teszik, hogy külső alkalmazások, például az SQL Server Management Studio vagy az SQLCMD segédprogram az SQL Database szolgáltatás tűzfalán keresztül csatlakozzon egy SQL-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-124">A server-level firewall rule allows an external application, such as SQL Server Management Studio or the SQLCMD utility to connect to a SQL database through the SQL Database service firewall.</span></span> <span data-ttu-id="4e5f8-125">A következő példában a tűzfal csak más Azure-erőforrások számára van nyitva.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-125">In the following example, the firewall is only opened for other Azure resources.</span></span> <span data-ttu-id="4e5f8-126">A külső csatlakozási lehetőségek engedélyezéséhez módosítsa az IP-címet egy, az Ön környezetének megfelelő címre.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-126">To enable external connectivity, change the IP address to an appropriate address for your environment.</span></span> <span data-ttu-id="4e5f8-127">Az összes IP-cím megnyitásához használja a 0.0.0.0 címet kezdő IP-címként és a 255.255.255.255 címet zárócímként.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-127">To open all IP addresses, use 0.0.0.0 as the starting IP address and 255.255.255.255 as the ending address.</span></span>  

```azurecli-interactive
az sql server firewall-rule create --resource-group $resourcegroupname --server $servername \
    -n AllowYourIp --start-ip-address $startip --end-ip-address $endip
```

> [!NOTE]
> <span data-ttu-id="4e5f8-128">Az SQL Database az 1433-as porton kommunikál.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-128">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="4e5f8-129">Ha vállalati hálózaton belülről próbál csatlakozni, elképzelhető, hogy a hálózati tűzfal nem engedélyezi a kimenő forgalmat az 1433-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-129">If you are trying to connect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="4e5f8-130">Ebben az esetben nem tud csatlakozni az Azure SQL Database-kiszolgálóhoz, ha az informatikai részleg nem nyitja meg az 1433-as portot.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-130">If so, you will not be able to connect to your Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="create-a-database-in-the-server-with-sample-data"></a><span data-ttu-id="4e5f8-131">Hozzon létre egy adatbázist a kiszolgálón mintaadatokkal</span><span class="sxs-lookup"><span data-stu-id="4e5f8-131">Create a database in the server with sample data</span></span>

<span data-ttu-id="4e5f8-132">Hozzon létre egy [S0 teljesítményszintű](sql-database-service-tiers.md) adatbázist a kiszolgálón az [az sql db create](/cli/azure/sql/db#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-132">Create a database with an [S0 performance level](sql-database-service-tiers.md) in the server using the [az sql db create](/cli/azure/sql/db#create) command.</span></span> <span data-ttu-id="4e5f8-133">Az alábbi példa egy `mySampleDatabase` nevű adatbázist hoz létre, és betölti az AdventureWorksLT mintaadatokat ebbe az adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-133">The following example creates a database called `mySampleDatabase` and loads the AdventureWorksLT sample data into this database.</span></span> <span data-ttu-id="4e5f8-134">Igény szerint cserélje ki ezeket az előre meghatározott értékeket (az ebben a gyűjteményben lévő többi rövid útmutató az ebben a rövid útmutatóban lévő értékekre épít).</span><span class="sxs-lookup"><span data-stu-id="4e5f8-134">Replace these predefined values as desired (other quick starts in this collection build upon the values in this quick start).</span></span>

```azurecli-interactive
az sql db create --resource-group $resourcegroupname --server $servername \
    --name $databasename --sample-name AdventureWorksLT --service-objective S0
```

## <a name="clean-up-resources"></a><span data-ttu-id="4e5f8-135">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="4e5f8-135">Clean up resources</span></span>

<span data-ttu-id="4e5f8-136">Az ebben a gyűjteményben lévő többi rövid útmutató erre a rövid útmutatóra épít.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-136">Other quick starts in this collection build upon this quick start.</span></span> 

> [!TIP]
> <span data-ttu-id="4e5f8-137">Ha azt tervezi, hogy az ezt követő rövid útmutatókkal dolgozik tovább, akkor ne törölje az ebben a rövid útmutatóban létrehozott erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-137">If you plan to continue on to work with subsequent quick starts, do not clean up the resources created in this quick start.</span></span> <span data-ttu-id="4e5f8-138">Ha nem folytatja a munkát, akkor a következő lépésekkel törölheti az Azure Portalon a rövid útmutatóhoz létrehozott összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-138">If you do not plan to continue, use the following steps to delete all resources created by this quick start in the Azure portal.</span></span>
>

```azurecli-interactive
az group delete --name $resourcegroupname
```

## <a name="next-steps"></a><span data-ttu-id="4e5f8-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4e5f8-139">Next steps</span></span>

<span data-ttu-id="4e5f8-140">Most, hogy rendelkezik egy adatbázissal, csatlakoztathatja a kedvenc eszközeit, és lekérdezéseket hajthat végre velük.</span><span class="sxs-lookup"><span data-stu-id="4e5f8-140">Now that you have a database, you can connect and query using your favorite tools.</span></span> <span data-ttu-id="4e5f8-141">További információkért válassza ki az eszközt az alábbiak közül:</span><span class="sxs-lookup"><span data-stu-id="4e5f8-141">Learn more by choosing your tool below:</span></span>

- [<span data-ttu-id="4e5f8-142">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="4e5f8-142">SQL Server Management Studio</span></span>](sql-database-connect-query-ssms.md)
- [<span data-ttu-id="4e5f8-143">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4e5f8-143">Visual Studio Code</span></span>](sql-database-connect-query-vscode.md)
- [<span data-ttu-id="4e5f8-144">.NET</span><span class="sxs-lookup"><span data-stu-id="4e5f8-144">.NET</span></span>](sql-database-connect-query-dotnet.md)
- [<span data-ttu-id="4e5f8-145">PHP</span><span class="sxs-lookup"><span data-stu-id="4e5f8-145">PHP</span></span>](sql-database-connect-query-php.md)
- [<span data-ttu-id="4e5f8-146">Node.js</span><span class="sxs-lookup"><span data-stu-id="4e5f8-146">Node.js</span></span>](sql-database-connect-query-nodejs.md)
- [<span data-ttu-id="4e5f8-147">Java</span><span class="sxs-lookup"><span data-stu-id="4e5f8-147">Java</span></span>](sql-database-connect-query-java.md)
- [<span data-ttu-id="4e5f8-148">Python</span><span class="sxs-lookup"><span data-stu-id="4e5f8-148">Python</span></span>](sql-database-connect-query-python.md)
- [<span data-ttu-id="4e5f8-149">Ruby</span><span class="sxs-lookup"><span data-stu-id="4e5f8-149">Ruby</span></span>](sql-database-connect-query-ruby.md)

