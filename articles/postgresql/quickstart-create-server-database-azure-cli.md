---
title: "Azure-adatbázis létrehozása PostgreSQL-kiszolgálóhoz az Azure CLI használatával | Microsoft Docs"
description: "Rövid útmutató az Azure-adatbázis PostgreSQL-kiszolgálóhoz való létrehozásához és kezeléséhez az Azure CLI (parancssori felület) használatával."
services: postgresql
author: sanagama
ms.author: sanagama
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 06/13/2017
ms.openlocfilehash: d78243abc140c7b3f0b99bdf56821b7920568550
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-azure-database-for-postgresql-using-the-azure-cli"></a><span data-ttu-id="b67b8-103">Azure-adatbázis létrehozása PostgreSQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="b67b8-103">Create an Azure Database for PostgreSQL using the Azure CLI</span></span>
<span data-ttu-id="b67b8-104">A PostgreSQL-hez készült Azure Database felügyelt szolgáltatás, amely lehetővé teszi a magas rendelkezésre állású PostgreSQL-adatbázisok futtatását, kezelését és skálázását a felhőben.</span><span class="sxs-lookup"><span data-stu-id="b67b8-104">Azure Database for PostgreSQL is a managed service that enables you to run, manage, and scale highly available PostgreSQL databases in the cloud.</span></span> <span data-ttu-id="b67b8-105">Az Azure CLI az Azure-erőforrások parancssorból vagy szkriptekkel történő létrehozására és kezelésére használható.</span><span class="sxs-lookup"><span data-stu-id="b67b8-105">The Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span> <span data-ttu-id="b67b8-106">Ez a rövid útmutató bemutatja, hogyan hozhat létre Azure-adatbázist PostgreSQL-kiszolgálóhoz az Azure CLI használatával az [Azure-erőforráscsoportban](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="b67b8-106">This quickstart shows you how to create an Azure Database for PostgreSQL server in an [Azure resource group](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) using the Azure CLI.</span></span>

<span data-ttu-id="b67b8-107">Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.</span><span class="sxs-lookup"><span data-stu-id="b67b8-107">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="b67b8-108">Ha a CLI helyi telepítését és használatát választja, akkor ehhez a témakörhöz az Azure CLI 2.0-s vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="b67b8-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="b67b8-109">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="b67b8-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="b67b8-110">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b67b8-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="b67b8-111">Ha több előfizetéssel rendelkezik válassza a megfelelő előfizetést, amelyen az erőforrás terhelve lesz.</span><span class="sxs-lookup"><span data-stu-id="b67b8-111">If you have multiple subscriptions, choose the appropriate subscription in which the resource will be billed.</span></span> <span data-ttu-id="b67b8-112">Válasszon ki egy megadott előfizetés-azonosítót a fiókja alatt az [az account set](/cli/azure/account#set) parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="b67b8-112">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="b67b8-113">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="b67b8-113">Create a resource group</span></span>

<span data-ttu-id="b67b8-114">Hozzon létre egy [Azure-erőforráscsoportot](../azure-resource-manager/resource-group-overview.md) az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="b67b8-114">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="b67b8-115">Az erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="b67b8-115">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="b67b8-116">A következő példában létrehozunk egy `westus` nevű erőforráscsoportot a `myresourcegroup` helyen.</span><span class="sxs-lookup"><span data-stu-id="b67b8-116">The following example creates a resource group named `myresourcegroup` in the `westus` location.</span></span>
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="b67b8-117">Azure-adatbázis létrehozása PostgreSQL-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="b67b8-117">Create an Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="b67b8-118">Hozzon létre egy [Azure-adatbázist PostgreSQL- kiszolgálóhoz](overview.md) az [az postgres server create](/cli/azure/postgres/server#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="b67b8-118">Create an [Azure Database for PostgreSQL server](overview.md) using the [az postgres server create](/cli/azure/postgres/server#create) command.</span></span> <span data-ttu-id="b67b8-119">A kiszolgáló adatbázisok egy csoportját tartalmazza, amelyeket a rendszer egy csoportként kezel.</span><span class="sxs-lookup"><span data-stu-id="b67b8-119">A server contains a group of databases managed as a group.</span></span> 

<span data-ttu-id="b67b8-120">A következő példában létrehozunk egy `mypgserver-20170401` nevű kiszolgálót a `myresourcegroup` erőforráscsoportban `mylogin` kiszolgálói rendszergazdai bejelentkezéssel.</span><span class="sxs-lookup"><span data-stu-id="b67b8-120">The following example creates a server named `mypgserver-20170401` in your resource group `myresourcegroup` with server admin login `mylogin`.</span></span> <span data-ttu-id="b67b8-121">A kiszolgáló neve DNS-névbe van leképezve, ezért globálisan egyedinek kell lennie az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b67b8-121">The name of a server maps to DNS name and is thus required to be globally unique in Azure.</span></span> <span data-ttu-id="b67b8-122">A `<server_admin_password>` helyére írja be saját értékét.</span><span class="sxs-lookup"><span data-stu-id="b67b8-122">Substitute the `<server_admin_password>` with your own value.</span></span>
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401  --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> <span data-ttu-id="b67b8-123">A kiszolgáló itt megadott rendszergazdai bejelentkezési nevét és jelszavát kell majd használnia a rövid útmutató későbbi szakaszaiban a kiszolgálóra és az adatbázisaira való bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="b67b8-123">The server admin login and password that you specify here are required to log in to the server and its databases later in this quick start.</span></span> <span data-ttu-id="b67b8-124">Jegyezze meg vagy jegyezze fel ezt az információt későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="b67b8-124">Remember or record this information for later use.</span></span>

<span data-ttu-id="b67b8-125">Alapértelmezés szerint a **postgres** adatbázis a kiszolgáló alatt jön létre.</span><span class="sxs-lookup"><span data-stu-id="b67b8-125">By default, **postgres** database gets created under your server.</span></span> <span data-ttu-id="b67b8-126">A [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) adatbázis egy alapértelmezett adatbázis, amelyet a felhasználók, segédprogramok és külső féltől származó alkalmazások általi használatra szántak.</span><span class="sxs-lookup"><span data-stu-id="b67b8-126">The [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) database is a default database meant for use by users, utilities, and third-party applications.</span></span> 


## <a name="configure-a-server-level-firewall-rule"></a><span data-ttu-id="b67b8-127">Kiszolgálószintű tűzfalszabály konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b67b8-127">Configure a server-level firewall rule</span></span>

<span data-ttu-id="b67b8-128">Hozzon létre egy Azure PostgreSQL kiszolgálószintű tűzfalszabályt az [az sql server firewall create](/cli/azure/postgres/server/firewall-rule#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="b67b8-128">Create an Azure PostgreSQL server-level firewall rule with the [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) command.</span></span> <span data-ttu-id="b67b8-129">Egy kiszolgálószintű tűzfalszabály lehetővé teszi olyan külső alkalmazások számára, mint a [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html), vagy a [PgAdmin](https://www.pgadmin.org/), hogy kapcsolódjon a kiszolgálóhoz az PostgreSQL szolgáltatás tűzfalán keresztül.</span><span class="sxs-lookup"><span data-stu-id="b67b8-129">A server-level firewall rule allows an external application, such as [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) or [PgAdmin](https://www.pgadmin.org/) to connect to your server through the Azure PostgreSQL service firewall.</span></span> 

<span data-ttu-id="b67b8-130">Beállíthat egy olyan tűzfalszabályt, amely lefed egy IP-címtartományt, annak érdekében, hogy csatlakozni tudjon a saját hálózatából.</span><span class="sxs-lookup"><span data-stu-id="b67b8-130">You can set a firewall rule that covers an IP range to be able to connect from your network.</span></span> <span data-ttu-id="b67b8-131">A követező példában az [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) parancsot használjuk az IP-címtartományhoz tartozó `AllowAllIps` tűzfalszabály létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b67b8-131">The following example uses [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) to create a firewall rule `AllowAllIps` for an IP address range.</span></span> <span data-ttu-id="b67b8-132">Az összes IP-cím megnyitásához használja a 0.0.0.0 címet kezdő IP-címként és a 255.255.255.255 címet zárócímként.</span><span class="sxs-lookup"><span data-stu-id="b67b8-132">To open all IP addresses, use 0.0.0.0 as the starting IP address and 255.255.255.255 as the ending address.</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="b67b8-133">Azure PostgreSQL-kiszolgáló az 5432-es porton keresztül kommunikál.</span><span class="sxs-lookup"><span data-stu-id="b67b8-133">Azure PostgreSQL server communicates over port 5432.</span></span> <span data-ttu-id="b67b8-134">Ha vállalati hálózaton belülről próbál csatlakozni, elképzelhető, hogy a hálózati tűzfal nem engedélyezi a kimenő forgalmat az 5432-es porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="b67b8-134">When connecting from within a corporate network, outbound traffic over port 5432 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="b67b8-135">Kérje meg az informatikai részleget, hogy nyissa meg az 5432-es portot az Azure SQL Database-kiszolgálóhoz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="b67b8-135">Have your IT department open port 5432 to connect to your Azure SQL Database server.</span></span>

## <a name="get-the-connection-information"></a><span data-ttu-id="b67b8-136">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="b67b8-136">Get the connection information</span></span>

<span data-ttu-id="b67b8-137">A kiszolgálóhoz való kapcsolódáshoz meg kell adnia a gazdagép adatait és a hozzáférési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="b67b8-137">To connect to your server, you need to provide host information and access credentials.</span></span>
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

<span data-ttu-id="b67b8-138">Az eredmény JSON formátumban van.</span><span class="sxs-lookup"><span data-stu-id="b67b8-138">The result is in JSON format.</span></span> <span data-ttu-id="b67b8-139">Jegyezze fel a következőket: **administratorLogin** és **fullyQualifiedDomainName**.</span><span class="sxs-lookup"><span data-stu-id="b67b8-139">Make a note of the **administratorLogin** and **fullyQualifiedDomainName**.</span></span>
```json
{
  "administratorLogin": "mylogin",
  "fullyQualifiedDomainName": "mypgserver-20170401.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mypgserver-20170401",
  "location": "westus",
  "name": "mypgserver-20170401",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "PGSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 51200,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "9.6"
}
```

## <a name="connect-to-postgresql-database-using-psql"></a><span data-ttu-id="b67b8-140">Csatlakozás a PostgreSQL-adatbázishoz a psql használatával</span><span class="sxs-lookup"><span data-stu-id="b67b8-140">Connect to PostgreSQL database using psql</span></span>

<span data-ttu-id="b67b8-141">Ha az ügyfélszámítógépen telepítve van a PostgreSQL, akkor használhatja a [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) helyi példányát az Azure PostgreSQL-kiszolgálóhoz való csatalakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="b67b8-141">If your client computer has PostgreSQL installed, you can use a local instance of [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) to connect to an Azure PostgreSQL server.</span></span> <span data-ttu-id="b67b8-142">Használjuk a psql parancssori segédprogramot az Azure PostgreSQL-kiszolgálóhoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="b67b8-142">Let's now use the psql command-line utility to connect to the Azure PostgreSQL server.</span></span>

1. <span data-ttu-id="b67b8-143">Futtassa a következő psql parancsot az Azure-adatbázis PostgreSQL-kiszolgálóhoz való kapcsolódáshoz</span><span class="sxs-lookup"><span data-stu-id="b67b8-143">Run the following psql command to connect to an Azure Database for PostgreSQL server</span></span>
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  <span data-ttu-id="b67b8-144">Például a következő parancs a **postgres** nevű alapértelmezett adatbázishoz kapcsolódik a PostgreSQL-kiszolgálón **mypgserver-20170401.postgres.database.azure.com** a hozzáférési hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="b67b8-144">For example, the following command connects to the default database called **postgres** on your PostgreSQL server **mypgserver-20170401.postgres.database.azure.com** using access credentials.</span></span> <span data-ttu-id="b67b8-145">Adja meg a `<server_admin_password>` kiszolgálói rendszergazdai jelszót, amelyet a jelszó megadásakor választott.</span><span class="sxs-lookup"><span data-stu-id="b67b8-145">Enter the `<server_admin_password>` you chose when prompted for password.</span></span>
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
```

2.  <span data-ttu-id="b67b8-146">Miután csatlakozott a kiszolgálóhoz, hozzon létre egy üres adatbázist, amikor a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="b67b8-146">Once you are connected to the server, create a blank database at the prompt.</span></span>
```sql
CREATE DATABASE mypgsqldb;
```

3.  <span data-ttu-id="b67b8-147">Amikor a rendszer kéri, hajtsa végre a következő parancsot, hogy az újonnan létrehozott **mypgsqldb** adatbázishoz kapcsolódhasson:</span><span class="sxs-lookup"><span data-stu-id="b67b8-147">At the prompt, execute the following command to switch connection to the newly created database **mypgsqldb**:</span></span>
```sql
\c mypgsqldb
```

## <a name="connect-to-postgresql-database-using-pgadmin"></a><span data-ttu-id="b67b8-148">Csatlakozás a PostgreSQL-adatbázishoz a pgAdmin használatával</span><span class="sxs-lookup"><span data-stu-id="b67b8-148">Connect to PostgreSQL database using pgAdmin</span></span>

<span data-ttu-id="b67b8-149">Kapcsolódás az Azure PostgreSQL-kiszolgálóhoz a _pgAdmin_ GUI-eszköz használatával</span><span class="sxs-lookup"><span data-stu-id="b67b8-149">To connect to Azure PostgreSQL server using the GUI tool _pgAdmin_</span></span>
1.  <span data-ttu-id="b67b8-150">Indítsa el a MySQL _pgAdmin_ alkalmazást az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="b67b8-150">Launch the _pgAdmin_ application on your client computer.</span></span> <span data-ttu-id="b67b8-151">A _pgAdmin-t_ http://www.pgadmin.org/ oldalról telepítheti.</span><span class="sxs-lookup"><span data-stu-id="b67b8-151">You can install _pgAdmin_ from http://www.pgadmin.org/.</span></span>
2.  <span data-ttu-id="b67b8-152">Válassza az **Új kiszolgáló hozzáadása** lehetőséget a **Gyorshivatkozások** menüből.</span><span class="sxs-lookup"><span data-stu-id="b67b8-152">Choose **Add New Server** from the **Quick Links** menu.</span></span>
3.  <span data-ttu-id="b67b8-153">A **Kiszolgáló létrehozása** párbeszédpanel **Általános** lapján adjon egy egyedi rövid nevet a kiszolgálónak.</span><span class="sxs-lookup"><span data-stu-id="b67b8-153">In the **Create - Server** dialog box **General** tab, enter a unique friendly Name for the server.</span></span> <span data-ttu-id="b67b8-154">Például **Azure PostgreSQL Server**.</span><span class="sxs-lookup"><span data-stu-id="b67b8-154">Say **Azure PostgreSQL Server**.</span></span>
 <span data-ttu-id="b67b8-155">![pgAdmin tool - create - server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)</span><span class="sxs-lookup"><span data-stu-id="b67b8-155">![pgAdmin tool - create - server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)</span></span>
4.  <span data-ttu-id="b67b8-156">A **Kiszolgáló létrehozása** párbeszédpanel **Kapcsolat** lapján:</span><span class="sxs-lookup"><span data-stu-id="b67b8-156">In the **Create - Server** dialog box, **Connection** tab:</span></span>
    - <span data-ttu-id="b67b8-157">Az **Állomás neve/címe** mezőben adja meg a kiszolgáló teljes nevét (például **mypgserver-20170401.postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="b67b8-157">Enter the fully qualified server name (for example, **mypgserver-20170401.postgres.database.azure.com**) in the **Host Name/ Address** box.</span></span> 
    - <span data-ttu-id="b67b8-158">A **Port** mezőben adja meg az 5432-es portot.</span><span class="sxs-lookup"><span data-stu-id="b67b8-158">Enter port 5432 into the **Port** box.</span></span> 
    - <span data-ttu-id="b67b8-159">Adja meg a rövid útmutató során korábban beszerzett **kiszolgálói-rendszergazda bejelentkezési nevet (user@mypgserver)**, és a kiszolgáló létrehozásakor megadott jelszót a **Felhasználónév** és a **Jelszó** mezőkben.</span><span class="sxs-lookup"><span data-stu-id="b67b8-159">Enter the **Server admin login (user@mypgserver)** obtained earlier in this quickstart and password you entered when you created the server into the **Username** and **Password** boxes, respectively.</span></span>
    - <span data-ttu-id="b67b8-160">Az **SSL-mód** elemnél válassza a **Kötelező** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="b67b8-160">Select **SSL Mode** as **Require**.</span></span> <span data-ttu-id="b67b8-161">Alapértelmezés szerint a rendszer minden Azure PostgreSQL-kiszolgálót az SSL-kényszerítéssel bekapcsolva hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b67b8-161">By default, all Azure PostgreSQL servers are created with SSL enforcing turned ON.</span></span> <span data-ttu-id="b67b8-162">Részletek az SSL-kényszerítés kikapcsolásáról: [SSL kényszerítése](./concepts-ssl-connection-security.md).</span><span class="sxs-lookup"><span data-stu-id="b67b8-162">To turn OFF SSL enforcing, see details in [Enforcing SSL](./concepts-ssl-connection-security.md).</span></span>

    ![pgAdmin - Create - Server](./media/quickstart-create-server-database-azure-cli/2-pgadmin-create-server.png)
5.  <span data-ttu-id="b67b8-164">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b67b8-164">Click **Save**.</span></span>
6.  <span data-ttu-id="b67b8-165">A baloldali Böngésző panelen bontsa ki a **Kiszolgáló csoportok** elemet.</span><span class="sxs-lookup"><span data-stu-id="b67b8-165">In the Browser left pane, expand the **Server Groups**.</span></span> <span data-ttu-id="b67b8-166">Válassza ki az **Azure PostgreSQL Server** kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="b67b8-166">Choose your server **Azure PostgreSQL Server**.</span></span>
7.  <span data-ttu-id="b67b8-167">Válassza ki a **Kiszolgálót**, amelyhez csatlakozott, majd azon belül válassza az **Adatbázisok** elemet.</span><span class="sxs-lookup"><span data-stu-id="b67b8-167">Choose the **Server** you connected to, and then choose **Databases** under it.</span></span> 
8.  <span data-ttu-id="b67b8-168">Az adatbázis létrehozásához kattintson jobb gombbal az **Adatbázisok** elemre.</span><span class="sxs-lookup"><span data-stu-id="b67b8-168">Right-click on **Databases** to Create a Database.</span></span>
9.  <span data-ttu-id="b67b8-169">Válasszon egy adatbázisnevet (**mypgsqldb**) és a tulajdonosát kiszolgálói rendszergazdai bejelentkezéssel (**mylogin**).</span><span class="sxs-lookup"><span data-stu-id="b67b8-169">Choose a database name **mypgsqldb** and the owner for it as server admin login **mylogin**.</span></span>
10. <span data-ttu-id="b67b8-170">Az üres adatbázis létrehozásához kattintson a **Mentés** elemre.</span><span class="sxs-lookup"><span data-stu-id="b67b8-170">Click **Save** to create a blank database.</span></span>
11. <span data-ttu-id="b67b8-171">A **Böngészőben** bontsa ki a **Kiszolgálók** csoportot.</span><span class="sxs-lookup"><span data-stu-id="b67b8-171">In the **Browser**, expand the **Servers** group.</span></span> <span data-ttu-id="b67b8-172">Bontsa ki a létrehozott kiszolgálót, ekkor megjelenik a **mypgsqldb** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b67b8-172">Expand the server you created, and see the database **mypgsqldb** under it.</span></span>
 <span data-ttu-id="b67b8-173">![pgAdmin - Create - Database](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)</span><span class="sxs-lookup"><span data-stu-id="b67b8-173">![pgAdmin - Create - Database](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)</span></span>


## <a name="clean-up-resources"></a><span data-ttu-id="b67b8-174">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="b67b8-174">Clean up resources</span></span>

<span data-ttu-id="b67b8-175">Távolítsa el a rövid útmutató során létrehozott összes erőforrást az [Azure-erőforráscsoport](../azure-resource-manager/resource-group-overview.md) törlésével.</span><span class="sxs-lookup"><span data-stu-id="b67b8-175">Clean up all resources you created in the quickstart by deleting the [Azure resource group](../azure-resource-manager/resource-group-overview.md).</span></span>

> [!TIP]
> <span data-ttu-id="b67b8-176">Az ebben a gyűjteményben lévő többi rövid útmutató erre a rövid útmutatóra épül.</span><span class="sxs-lookup"><span data-stu-id="b67b8-176">Other quickstarts in this collection build upon this quick start.</span></span> <span data-ttu-id="b67b8-177">Ha azt tervezi, hogy az ezt követő rövid útmutatókkal dolgozik tovább, akkor ne törölje az ebben a rövid útmutatóban létrehozott erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="b67b8-177">If you plan to continue on to work with subsequent quickstarts, do not clean up the resources created in this quickstart.</span></span> <span data-ttu-id="b67b8-178">Ha nem folytatja a munkát, akkor a következő lépésekkel törölheti az Azure CLI-ben a rövid útmutatóhoz létrehozott összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="b67b8-178">If you do not plan to continue, use the following steps to delete all resources created by this quickstart in the Azure CLI.</span></span>

```azurecli-interactive
az group delete --name myresourcegroup
```

<span data-ttu-id="b67b8-179">Ha csak az újonnan létrehozott kiszolgálót szeretné törölni, futtathatja az [az postgres server delete](/cli/azure/postgres/server#delete) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b67b8-179">If you just would like to delete the one newly created server, you can run [az postgres server delete](/cli/azure/postgres/server#delete) command.</span></span>
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mypgserver-20170401
```

## <a name="next-steps"></a><span data-ttu-id="b67b8-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b67b8-180">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b67b8-181">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="b67b8-181">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
