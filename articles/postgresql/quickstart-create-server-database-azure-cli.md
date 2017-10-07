---
title: "Hozzon létre egy Azure-adatbázis PostgreSQL hello Azure parancssori felület használatával |} Microsoft Docs"
description: "Gyors útmutató toocreate start, és kezelheti az Azure-adatbázis PostgreSQL-kiszolgáló Azure CLI-vel (parancssori felülettel)."
services: postgresql
author: sanagama
ms.author: sanagama
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 06/13/2017
ms.openlocfilehash: 946aa3cbf5ff9f5ac4e51248412d3da5d718141e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-postgresql-using-hello-azure-cli"></a><span data-ttu-id="12c38-103">Hozzon létre egy Azure-adatbázis PostgreSQL hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="12c38-103">Create an Azure Database for PostgreSQL using hello Azure CLI</span></span>
<span data-ttu-id="12c38-104">Azure PostgreSQL-adatbázishoz egy felügyelt szolgáltatás, amely lehetővé teszi toorun, kezeléséhez, és magas rendelkezésre állású PostgreSQL-adatbázisok hello felhőben méretezése.</span><span class="sxs-lookup"><span data-stu-id="12c38-104">Azure Database for PostgreSQL is a managed service that enables you toorun, manage, and scale highly available PostgreSQL databases in hello cloud.</span></span> <span data-ttu-id="12c38-105">hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="12c38-105">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="12c38-106">A gyors üzembe helyezés bemutatja, hogyan toocreate egy Azure-adatbázis a PostgreSQL-kiszolgáló egy [Azure erőforráscsoport](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) Azure parancssori felület használatával hello.</span><span class="sxs-lookup"><span data-stu-id="12c38-106">This quickstart shows you how toocreate an Azure Database for PostgreSQL server in an [Azure resource group](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) using hello Azure CLI.</span></span>

<span data-ttu-id="12c38-107">Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.</span><span class="sxs-lookup"><span data-stu-id="12c38-107">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="12c38-108">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="12c38-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="12c38-109">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="12c38-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="12c38-110">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="12c38-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="12c38-111">Ha több előfizetéssel rendelkezik, válassza ki azt a hello megfelelő előfizetés, amelyben hello erőforrás lesz terhelve.</span><span class="sxs-lookup"><span data-stu-id="12c38-111">If you have multiple subscriptions, choose hello appropriate subscription in which hello resource will be billed.</span></span> <span data-ttu-id="12c38-112">Válasszon ki egy megadott előfizetés-azonosítót a fiókja alatt az [az account set](/cli/azure/account#set) parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="12c38-112">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="12c38-113">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="12c38-113">Create a resource group</span></span>

<span data-ttu-id="12c38-114">Hozzon létre egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hello segítségével [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="12c38-114">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="12c38-115">Az erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="12c38-115">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="12c38-116">hello alábbi példa létrehoz egy erőforráscsoportot `myresourcegroup` a hello `westus` helyét.</span><span class="sxs-lookup"><span data-stu-id="12c38-116">hello following example creates a resource group named `myresourcegroup` in hello `westus` location.</span></span>
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="12c38-117">Azure-adatbázis létrehozása PostgreSQL-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="12c38-117">Create an Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="12c38-118">Hozzon létre egy [PostgreSQL-kiszolgáló Azure-adatbázis](overview.md) hello segítségével [az postgres kiszolgáló létrehozni](/cli/azure/postgres/server#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="12c38-118">Create an [Azure Database for PostgreSQL server](overview.md) using hello [az postgres server create](/cli/azure/postgres/server#create) command.</span></span> <span data-ttu-id="12c38-119">A kiszolgáló adatbázisok egy csoportját tartalmazza, amelyeket a rendszer egy csoportként kezel.</span><span class="sxs-lookup"><span data-stu-id="12c38-119">A server contains a group of databases managed as a group.</span></span> 

<span data-ttu-id="12c38-120">hello alábbi példakód létrehozza a kiszolgáló nevű `mypgserver-20170401` az erőforráscsoportban `myresourcegroup` a kiszolgáló-rendszergazdai bejelentkezés `mylogin`.</span><span class="sxs-lookup"><span data-stu-id="12c38-120">hello following example creates a server named `mypgserver-20170401` in your resource group `myresourcegroup` with server admin login `mylogin`.</span></span> <span data-ttu-id="12c38-121">hello kiszolgáló nevét, amelyhez tooDNS neve képezi le, és ezért szükséges toobe globálisan egyedi az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="12c38-121">hello name of a server maps tooDNS name and is thus required toobe globally unique in Azure.</span></span> <span data-ttu-id="12c38-122">Helyettesítő hello `<server_admin_password>` saját értékkel.</span><span class="sxs-lookup"><span data-stu-id="12c38-122">Substitute hello `<server_admin_password>` with your own value.</span></span>
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401  --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> <span data-ttu-id="12c38-123">hello kiszolgáló-rendszergazdai bejelentkezés és a jelszót, amely az itt megadott a szükséges toolog toohello Server és az adatbázisok a gyors üzembe helyezési későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="12c38-123">hello server admin login and password that you specify here are required toolog in toohello server and its databases later in this quick start.</span></span> <span data-ttu-id="12c38-124">Jegyezze meg vagy jegyezze fel ezt az információt későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="12c38-124">Remember or record this information for later use.</span></span>

<span data-ttu-id="12c38-125">Alapértelmezés szerint a **postgres** adatbázis a kiszolgáló alatt jön létre.</span><span class="sxs-lookup"><span data-stu-id="12c38-125">By default, **postgres** database gets created under your server.</span></span> <span data-ttu-id="12c38-126">Hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) egy alapértelmezett adatbázis való használatra szolgál, a felhasználók, segédprogramok és harmadik féltől származó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="12c38-126">hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) database is a default database meant for use by users, utilities, and third-party applications.</span></span> 


## <a name="configure-a-server-level-firewall-rule"></a><span data-ttu-id="12c38-127">Kiszolgálószintű tűzfalszabály konfigurálása</span><span class="sxs-lookup"><span data-stu-id="12c38-127">Configure a server-level firewall rule</span></span>

<span data-ttu-id="12c38-128">Hozzon létre egy Azure PostgreSQL kiszolgálószintű tűzfalszabály hello [az postgres-tűzfalszabály létrehozása](/cli/azure/postgres/server/firewall-rule#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="12c38-128">Create an Azure PostgreSQL server-level firewall rule with hello [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) command.</span></span> <span data-ttu-id="12c38-129">Egy kiszolgálószintű tűzfalszabályt lehetővé teszi a külső alkalmazás, például a [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) vagy [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server hello Azure PostgreSQL szolgáltatás tűzfalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="12c38-129">A server-level firewall rule allows an external application, such as [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) or [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server through hello Azure PostgreSQL service firewall.</span></span> 

<span data-ttu-id="12c38-130">Beállíthat egy tűzfalszabályt, amely hozzá van rendelve egy IP tartomány toobe képes tooconnect a hálózatról.</span><span class="sxs-lookup"><span data-stu-id="12c38-130">You can set a firewall rule that covers an IP range toobe able tooconnect from your network.</span></span> <span data-ttu-id="12c38-131">hello alábbi példában [az postgres-tűzfalszabály létrehozása](/cli/azure/postgres/server/firewall-rule#create) toocreate tűzfalszabály `AllowAllIps` egy IP-címtartományt.</span><span class="sxs-lookup"><span data-stu-id="12c38-131">hello following example uses [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) toocreate a firewall rule `AllowAllIps` for an IP address range.</span></span> <span data-ttu-id="12c38-132">tooopen összes IP-cím, IP-cím és 255.255.255.255 indítása zárócíme hello hello 0.0.0.0 használják.</span><span class="sxs-lookup"><span data-stu-id="12c38-132">tooopen all IP addresses, use 0.0.0.0 as hello starting IP address and 255.255.255.255 as hello ending address.</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="12c38-133">Azure PostgreSQL-kiszolgáló az 5432-es porton keresztül kommunikál.</span><span class="sxs-lookup"><span data-stu-id="12c38-133">Azure PostgreSQL server communicates over port 5432.</span></span> <span data-ttu-id="12c38-134">Ha vállalati hálózaton belülről próbál csatlakozni, elképzelhető, hogy a hálózati tűzfal nem engedélyezi a kimenő forgalmat az 5432-es porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="12c38-134">When connecting from within a corporate network, outbound traffic over port 5432 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="12c38-135">Rendelkezik az IT-részleg, nyissa meg a port 5432 tooconnect tooyour Azure SQL Database-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="12c38-135">Have your IT department open port 5432 tooconnect tooyour Azure SQL Database server.</span></span>

## <a name="get-hello-connection-information"></a><span data-ttu-id="12c38-136">Hello kapcsolatadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="12c38-136">Get hello connection information</span></span>

<span data-ttu-id="12c38-137">tooconnect tooyour kiszolgáló, tooprovide állomás információkat és a hozzáférési hitelesítő adatok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="12c38-137">tooconnect tooyour server, you need tooprovide host information and access credentials.</span></span>
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

<span data-ttu-id="12c38-138">hello eredménye JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="12c38-138">hello result is in JSON format.</span></span> <span data-ttu-id="12c38-139">Jegyezze fel a hello **administratorLogin** és **fullyQualifiedDomainName**.</span><span class="sxs-lookup"><span data-stu-id="12c38-139">Make a note of hello **administratorLogin** and **fullyQualifiedDomainName**.</span></span>
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

## <a name="connect-toopostgresql-database-using-psql"></a><span data-ttu-id="12c38-140">Csatlakozás tooPostgreSQL adatbázist psql használatával</span><span class="sxs-lookup"><span data-stu-id="12c38-140">Connect tooPostgreSQL database using psql</span></span>

<span data-ttu-id="12c38-141">Ha az ügyfélszámítógép PostgreSQL telepítve rendelkezik, egy helyi példányát használhatja [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooconnect tooan Azure PostgreSQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="12c38-141">If your client computer has PostgreSQL installed, you can use a local instance of [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooconnect tooan Azure PostgreSQL server.</span></span> <span data-ttu-id="12c38-142">Most már a hello psql parancssori segédprogram tooconnect toohello Azure PostgreSQL kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="12c38-142">Let's now use hello psql command-line utility tooconnect toohello Azure PostgreSQL server.</span></span>

1. <span data-ttu-id="12c38-143">Futtassa a következő psql parancs tooconnect tooan Azure adatbázis PostgreSQL-kiszolgáló hello</span><span class="sxs-lookup"><span data-stu-id="12c38-143">Run hello following psql command tooconnect tooan Azure Database for PostgreSQL server</span></span>
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  <span data-ttu-id="12c38-144">Például a következő parancs hello csatlakozik toohello alapértelmezett adatbázis **postgres** a PostgreSQL kiszolgálón **mypgserver-20170401.postgres.database.azure.com** hozzáférési hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="12c38-144">For example, hello following command connects toohello default database called **postgres** on your PostgreSQL server **mypgserver-20170401.postgres.database.azure.com** using access credentials.</span></span> <span data-ttu-id="12c38-145">Adja meg a hello `<server_admin_password>` úgy döntött, hogy amikor a jelszó megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="12c38-145">Enter hello `<server_admin_password>` you chose when prompted for password.</span></span>
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
```

2.  <span data-ttu-id="12c38-146">Ha csatlakoztatott toohello kiszolgáló, hozzon létre egy üres adatbázist hello parancssorba.</span><span class="sxs-lookup"><span data-stu-id="12c38-146">Once you are connected toohello server, create a blank database at hello prompt.</span></span>
```sql
CREATE DATABASE mypgsqldb;
```

3.  <span data-ttu-id="12c38-147">Hello parancssorba hajtható végre a következő parancs tooswitch kapcsolat található, újonnan létrehozott toohello adatbázis hello **mypgsqldb**:</span><span class="sxs-lookup"><span data-stu-id="12c38-147">At hello prompt, execute hello following command tooswitch connection toohello newly created database **mypgsqldb**:</span></span>
```sql
\c mypgsqldb
```

## <a name="connect-toopostgresql-database-using-pgadmin"></a><span data-ttu-id="12c38-148">Csatlakozás tooPostgreSQL adatbázist pgAdmin használatával</span><span class="sxs-lookup"><span data-stu-id="12c38-148">Connect tooPostgreSQL database using pgAdmin</span></span>

<span data-ttu-id="12c38-149">tooconnect tooAzure PostgreSQL kiszolgáló grafikus felhasználói Felülettel hello eszközzel _pgAdmin_</span><span class="sxs-lookup"><span data-stu-id="12c38-149">tooconnect tooAzure PostgreSQL server using hello GUI tool _pgAdmin_</span></span>
1.  <span data-ttu-id="12c38-150">Indítsa el a hello _pgAdmin_ alkalmazás az ügyfélszámítógépre.</span><span class="sxs-lookup"><span data-stu-id="12c38-150">Launch hello _pgAdmin_ application on your client computer.</span></span> <span data-ttu-id="12c38-151">A _pgAdmin-t_ http://www.pgadmin.org/ oldalról telepítheti.</span><span class="sxs-lookup"><span data-stu-id="12c38-151">You can install _pgAdmin_ from http://www.pgadmin.org/.</span></span>
2.  <span data-ttu-id="12c38-152">Válasszon **új kiszolgáló hozzáadása** a hello **Gyorshivatkozások** menü.</span><span class="sxs-lookup"><span data-stu-id="12c38-152">Choose **Add New Server** from hello **Quick Links** menu.</span></span>
3.  <span data-ttu-id="12c38-153">A hello **- kiszolgáló létrehozása** párbeszédpanel **általános** lapra, adja meg a hello kiszolgáló egyedi rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="12c38-153">In hello **Create - Server** dialog box **General** tab, enter a unique friendly Name for hello server.</span></span> <span data-ttu-id="12c38-154">Például **Azure PostgreSQL Server**.</span><span class="sxs-lookup"><span data-stu-id="12c38-154">Say **Azure PostgreSQL Server**.</span></span>
 <span data-ttu-id="12c38-155">![pgAdmin tool - create - server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)</span><span class="sxs-lookup"><span data-stu-id="12c38-155">![pgAdmin tool - create - server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)</span></span>
4.  <span data-ttu-id="12c38-156">A hello **- kiszolgáló létrehozása** párbeszédpanelen **kapcsolat** lapon:</span><span class="sxs-lookup"><span data-stu-id="12c38-156">In hello **Create - Server** dialog box, **Connection** tab:</span></span>
    - <span data-ttu-id="12c38-157">Adja meg a hello teljes kiszolgálónév (például **mypgserver-20170401.postgres.database.azure.com**) hello a **állomásnév / cím** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="12c38-157">Enter hello fully qualified server name (for example, **mypgserver-20170401.postgres.database.azure.com**) in hello **Host Name/ Address** box.</span></span> 
    - <span data-ttu-id="12c38-158">Meg kell adnia port 5432 hello **Port** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="12c38-158">Enter port 5432 into hello **Port** box.</span></span> 
    - <span data-ttu-id="12c38-159">Adja meg a hello **kiszolgáló-rendszergazdai bejelentkezés (user@mypgserver)** korábban beszerzett a gyors üzembe helyezés és hello server hello történő létrehozásakor a megadott jelszó **felhasználónév** és **jelszó** dobozban, illetve.</span><span class="sxs-lookup"><span data-stu-id="12c38-159">Enter hello **Server admin login (user@mypgserver)** obtained earlier in this quickstart and password you entered when you created hello server into hello **Username** and **Password** boxes, respectively.</span></span>
    - <span data-ttu-id="12c38-160">Az **SSL-mód** elemnél válassza a **Kötelező** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="12c38-160">Select **SSL Mode** as **Require**.</span></span> <span data-ttu-id="12c38-161">Alapértelmezés szerint a rendszer minden Azure PostgreSQL-kiszolgálót az SSL-kényszerítéssel bekapcsolva hoz létre.</span><span class="sxs-lookup"><span data-stu-id="12c38-161">By default, all Azure PostgreSQL servers are created with SSL enforcing turned ON.</span></span> <span data-ttu-id="12c38-162">tooturn ki SSL kényszerítése, a részletek megtekintéséhez [kényszerítése SSL](./concepts-ssl-connection-security.md).</span><span class="sxs-lookup"><span data-stu-id="12c38-162">tooturn OFF SSL enforcing, see details in [Enforcing SSL](./concepts-ssl-connection-security.md).</span></span>

    ![pgAdmin - Create - Server](./media/quickstart-create-server-database-azure-cli/2-pgadmin-create-server.png)
5.  <span data-ttu-id="12c38-164">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="12c38-164">Click **Save**.</span></span>
6.  <span data-ttu-id="12c38-165">Hello böngésző bal oldali ablaktáblán bontsa ki a hello **kiszolgálócsoportok**.</span><span class="sxs-lookup"><span data-stu-id="12c38-165">In hello Browser left pane, expand hello **Server Groups**.</span></span> <span data-ttu-id="12c38-166">Válassza ki az **Azure PostgreSQL Server** kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="12c38-166">Choose your server **Azure PostgreSQL Server**.</span></span>
7.  <span data-ttu-id="12c38-167">Válassza ki a hello **Server** csatlakozik, és válassza a **adatbázisok** rajta.</span><span class="sxs-lookup"><span data-stu-id="12c38-167">Choose hello **Server** you connected to, and then choose **Databases** under it.</span></span> 
8.  <span data-ttu-id="12c38-168">Kattintson a jobb gombbal a **adatbázisok** tooCreate adatbázis.</span><span class="sxs-lookup"><span data-stu-id="12c38-168">Right-click on **Databases** tooCreate a Database.</span></span>
9.  <span data-ttu-id="12c38-169">Válassza ki az adatbázis nevét **mypgsqldb** és hello tulajdonosa hozzá, a kiszolgáló-rendszergazdai bejelentkezés **mylogin**.</span><span class="sxs-lookup"><span data-stu-id="12c38-169">Choose a database name **mypgsqldb** and hello owner for it as server admin login **mylogin**.</span></span>
10. <span data-ttu-id="12c38-170">Kattintson a **mentése** toocreate egy üres adatbázist.</span><span class="sxs-lookup"><span data-stu-id="12c38-170">Click **Save** toocreate a blank database.</span></span>
11. <span data-ttu-id="12c38-171">A hello **böngésző**, bontsa ki a hello **kiszolgálók** csoport.</span><span class="sxs-lookup"><span data-stu-id="12c38-171">In hello **Browser**, expand hello **Servers** group.</span></span> <span data-ttu-id="12c38-172">Bontsa ki a létrehozott hello kiszolgáló, és tekintse meg a hello adatbázis **mypgsqldb** rajta.</span><span class="sxs-lookup"><span data-stu-id="12c38-172">Expand hello server you created, and see hello database **mypgsqldb** under it.</span></span>
 <span data-ttu-id="12c38-173">![pgAdmin - Create - Database](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)</span><span class="sxs-lookup"><span data-stu-id="12c38-173">![pgAdmin - Create - Database](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)</span></span>


## <a name="clean-up-resources"></a><span data-ttu-id="12c38-174">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="12c38-174">Clean up resources</span></span>

<span data-ttu-id="12c38-175">Hello törlésével hello gyors üzembe helyezés létrehozott összes erőforrás karbantartása [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="12c38-175">Clean up all resources you created in hello quickstart by deleting hello [Azure resource group](../azure-resource-manager/resource-group-overview.md).</span></span>

> [!TIP]
> <span data-ttu-id="12c38-176">Az ebben a gyűjteményben lévő többi rövid útmutató erre a rövid útmutatóra épül.</span><span class="sxs-lookup"><span data-stu-id="12c38-176">Other quickstarts in this collection build upon this quick start.</span></span> <span data-ttu-id="12c38-177">Ha toocontinue, a következő toowork quickstarts, így nem karbantartáshoz hello a gyors üzembe helyezés a létrejött erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="12c38-177">If you plan toocontinue on toowork with subsequent quickstarts, do not clean up hello resources created in this quickstart.</span></span> <span data-ttu-id="12c38-178">Ha nem tervezi toocontinue, a következő lépéseket toodelete hello az összes erőforrást felhasználják hozta létre a gyors üzembe helyezés az Azure CLI hello.</span><span class="sxs-lookup"><span data-stu-id="12c38-178">If you do not plan toocontinue, use hello following steps toodelete all resources created by this quickstart in hello Azure CLI.</span></span>

```azurecli-interactive
az group delete --name myresourcegroup
```

<span data-ttu-id="12c38-179">Ha most szeretné toodelete hello egy újonnan létrehozott kiszolgálón, futtassa [az postgres kiszolgáló törlése](/cli/azure/postgres/server#delete) parancsot.</span><span class="sxs-lookup"><span data-stu-id="12c38-179">If you just would like toodelete hello one newly created server, you can run [az postgres server delete](/cli/azure/postgres/server#delete) command.</span></span>
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mypgserver-20170401
```

## <a name="next-steps"></a><span data-ttu-id="12c38-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="12c38-180">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="12c38-181">Adatbázis migrálása exportálással és importálással</span><span class="sxs-lookup"><span data-stu-id="12c38-181">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
