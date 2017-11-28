---
title: "Az első Azure-adatbázis kialakítása a PostgreSQL Azure parancssori felületével |} Microsoft Docs"
description: "Ez az oktatóanyag bemutatja hogyan tooDesign az első Azure-adatbázis PostgreSQL az Azure parancssori felület használatával."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 06/13/2017
ms.openlocfilehash: 7914925c090e0b6f3e7c8a999eedb0b2baf83d7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-database-for-postgresql-using-azure-cli"></a><span data-ttu-id="58d24-103">Az első Azure-adatbázis kialakítása a PostgreSQL Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="58d24-103">Design your first Azure Database for PostgreSQL using Azure CLI</span></span> 
<span data-ttu-id="58d24-104">Ebben az oktatóanyagban használhatja az Azure CLI (parancssori felület) és egyéb segédprogramok toolearn hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="58d24-104">In this tutorial, you use Azure CLI (command-line interface) and other utilities toolearn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="58d24-105">Azure-adatbázis létrehozása PostgreSQL-hez</span><span class="sxs-lookup"><span data-stu-id="58d24-105">Create an Azure Database for PostgreSQL</span></span>
> * <span data-ttu-id="58d24-106">Hello kiszolgáló tűzfal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="58d24-106">Configure hello server firewall</span></span>
> * <span data-ttu-id="58d24-107">Használjon [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) segédprogram toocreate adatbázis</span><span class="sxs-lookup"><span data-stu-id="58d24-107">Use [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility toocreate a database</span></span>
> * <span data-ttu-id="58d24-108">Mintaadatok betöltése</span><span class="sxs-lookup"><span data-stu-id="58d24-108">Load sample data</span></span>
> * <span data-ttu-id="58d24-109">Adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="58d24-109">Query data</span></span>
> * <span data-ttu-id="58d24-110">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="58d24-110">Update data</span></span>
> * <span data-ttu-id="58d24-111">Adatok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="58d24-111">Restore data</span></span>

<span data-ttu-id="58d24-112">Azure Cloud rendszerhéj hello hello böngészőben használhatja vagy [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli) a saját számítógép toorun hello kódblokkok ebben az oktatóanyagban.</span><span class="sxs-lookup"><span data-stu-id="58d24-112">You may use hello Azure Cloud Shell in hello browser, or [Install Azure CLI 2.0]( /cli/azure/install-azure-cli) on your own computer toorun hello code blocks in this tutorial.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="58d24-113">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="58d24-113">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="58d24-114">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="58d24-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="58d24-115">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="58d24-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="58d24-116">Ha több előfizetéssel rendelkezik, válassza ki a megfelelő előfizetés hello hello erőforrás létezik-e vagy a lesz számlázva.</span><span class="sxs-lookup"><span data-stu-id="58d24-116">If you have multiple subscriptions, choose hello appropriate subscription in which hello resource exists or is billed for.</span></span> <span data-ttu-id="58d24-117">Válasszon ki egy megadott előfizetés-azonosítót a fiókja alatt az [az account set](/cli/azure/account#set) parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="58d24-117">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="58d24-118">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="58d24-118">Create a resource group</span></span>
<span data-ttu-id="58d24-119">Hozzon létre egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hello segítségével [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="58d24-119">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="58d24-120">Az erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="58d24-120">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="58d24-121">hello alábbi példa létrehoz egy erőforráscsoportot `myresourcegroup` a hello `westus` helyét.</span><span class="sxs-lookup"><span data-stu-id="58d24-121">hello following example creates a resource group named `myresourcegroup` in hello `westus` location.</span></span>
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="58d24-122">Azure-adatbázis létrehozása PostgreSQL-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="58d24-122">Create an Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="58d24-123">Hozzon létre egy [PostgreSQL-kiszolgáló Azure-adatbázis](overview.md) hello segítségével [az postgres kiszolgáló létrehozni](/cli/azure/postgres/server#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="58d24-123">Create an [Azure Database for PostgreSQL server](overview.md) using hello [az postgres server create](/cli/azure/postgres/server#create) command.</span></span> <span data-ttu-id="58d24-124">A kiszolgáló adatbázisok egy csoportját tartalmazza, amelyeket a rendszer egy csoportként kezel.</span><span class="sxs-lookup"><span data-stu-id="58d24-124">A server contains a group of databases managed as a group.</span></span> 

<span data-ttu-id="58d24-125">hello alábbi példakód létrehozza nevű kiszolgáló `mypgserver-20170401` az erőforráscsoportban `myresourcegroup` a kiszolgáló-rendszergazdai bejelentkezés `mylogin`.</span><span class="sxs-lookup"><span data-stu-id="58d24-125">hello following example creates a server called `mypgserver-20170401` in your resource group `myresourcegroup` with server admin login `mylogin`.</span></span> <span data-ttu-id="58d24-126">A kiszolgáló nevét tooDNS neve képezi le, és ezért szükséges toobe globálisan egyedi az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="58d24-126">Name of a server maps tooDNS name and is thus required toobe globally unique in Azure.</span></span> <span data-ttu-id="58d24-127">Helyettesítő hello `<server_admin_password>` saját értékkel.</span><span class="sxs-lookup"><span data-stu-id="58d24-127">Substitute hello `<server_admin_password>` with your own value.</span></span>
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401 --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> <span data-ttu-id="58d24-128">hello kiszolgáló-rendszergazdai bejelentkezés és a jelszót, amely az itt megadott a szükséges toolog toohello Server és az adatbázisok a gyors üzembe helyezési későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="58d24-128">hello server admin login and password that you specify here are required toolog in toohello server and its databases later in this quick start.</span></span> <span data-ttu-id="58d24-129">Jegyezze meg vagy jegyezze fel ezt az információt későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="58d24-129">Remember or record this information for later use.</span></span>

<span data-ttu-id="58d24-130">Alapértelmezés szerint a **postgres** adatbázis a kiszolgáló alatt jön létre.</span><span class="sxs-lookup"><span data-stu-id="58d24-130">By default, **postgres** database gets created under your server.</span></span> <span data-ttu-id="58d24-131">Hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) egy alapértelmezett adatbázis való használatra szolgál, a felhasználók, segédprogramok és harmadik féltől származó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="58d24-131">hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) database is a default database meant for use by users, utilities, and third-party applications.</span></span> 


## <a name="configure-a-server-level-firewall-rule"></a><span data-ttu-id="58d24-132">Kiszolgálószintű tűzfalszabály konfigurálása</span><span class="sxs-lookup"><span data-stu-id="58d24-132">Configure a server-level firewall rule</span></span>

<span data-ttu-id="58d24-133">Hozzon létre egy Azure PostgreSQL kiszolgálószintű tűzfalszabály hello [az postgres-tűzfalszabály létrehozása](/cli/azure/postgres/server/firewall-rule#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="58d24-133">Create an Azure PostgreSQL server-level firewall rule with hello [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) command.</span></span> <span data-ttu-id="58d24-134">Egy kiszolgálószintű tűzfalszabályt lehetővé teszi a külső alkalmazás, például a [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) vagy [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server hello Azure PostgreSQL szolgáltatás tűzfalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="58d24-134">A server-level firewall rule allows an external application, such as [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) or [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server through hello Azure PostgreSQL service firewall.</span></span> 

<span data-ttu-id="58d24-135">Beállíthat egy tűzfalszabályt, amely hozzá van rendelve egy IP tartomány toobe képes tooconnect a hálózatról.</span><span class="sxs-lookup"><span data-stu-id="58d24-135">You can set a firewall rule that covers an IP range toobe able tooconnect from your network.</span></span> <span data-ttu-id="58d24-136">hello alábbi példában [az postgres-tűzfalszabály létrehozása](/cli/azure/postgres/server/firewall-rule#create) toocreate tűzfalszabály `AllowAllIps` egy IP-címtartományt.</span><span class="sxs-lookup"><span data-stu-id="58d24-136">hello following example uses [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) toocreate a firewall rule `AllowAllIps` for an IP address range.</span></span> <span data-ttu-id="58d24-137">tooopen összes IP-cím, IP-cím és 255.255.255.255 indítása zárócíme hello hello 0.0.0.0 használják.</span><span class="sxs-lookup"><span data-stu-id="58d24-137">tooopen all IP addresses, use 0.0.0.0 as hello starting IP address and 255.255.255.255 as hello ending address.</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="58d24-138">Azure PostgreSQL-kiszolgáló az 5432-es porton keresztül kommunikál.</span><span class="sxs-lookup"><span data-stu-id="58d24-138">Azure PostgreSQL server communicates over port 5432.</span></span> <span data-ttu-id="58d24-139">Ha vállalati hálózaton belülről próbál csatlakozni, elképzelhető, hogy a hálózati tűzfal nem engedélyezi a kimenő forgalmat az 5432-es porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="58d24-139">When connecting from within a corporate network, outbound traffic over port 5432 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="58d24-140">Rendelkezik az IT-részleg, nyissa meg a port 5432 tooconnect tooyour Azure SQL Database-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="58d24-140">Have your IT department open port 5432 tooconnect tooyour Azure SQL Database server.</span></span>
>

## <a name="get-hello-connection-information"></a><span data-ttu-id="58d24-141">Hello kapcsolatadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="58d24-141">Get hello connection information</span></span>

<span data-ttu-id="58d24-142">tooconnect tooyour kiszolgáló, tooprovide állomás információkat és a hozzáférési hitelesítő adatok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="58d24-142">tooconnect tooyour server, you need tooprovide host information and access credentials.</span></span>
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

<span data-ttu-id="58d24-143">hello eredménye JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="58d24-143">hello result is in JSON format.</span></span> <span data-ttu-id="58d24-144">Jegyezze fel a hello **administratorLogin** és **fullyQualifiedDomainName**.</span><span class="sxs-lookup"><span data-stu-id="58d24-144">Make a note of hello **administratorLogin** and **fullyQualifiedDomainName**.</span></span>
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

## <a name="connect-tooazure-database-for-postgresql-database-using-psql"></a><span data-ttu-id="58d24-145">Kapcsolódás adatbázis tooAzure psql használatával PostgreSQL-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="58d24-145">Connect tooAzure Database for PostgreSQL database using psql</span></span>
<span data-ttu-id="58d24-146">Ha az ügyfélszámítógép PostgreSQL telepítve rendelkezik, egy helyi példányát használhatja [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), vagy hello Azure Cloud Console tooconnect tooan Azure PostgreSQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="58d24-146">If your client computer has PostgreSQL installed, you can use a local instance of [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), or hello Azure Cloud Console tooconnect tooan Azure PostgreSQL server.</span></span> <span data-ttu-id="58d24-147">Most már a hello psql parancssori segédprogram tooconnect toohello Azure adatbázis PostgreSQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="58d24-147">Let's now use hello psql command-line utility tooconnect toohello Azure Database for PostgreSQL server.</span></span>

1. <span data-ttu-id="58d24-148">Futtassa a következő psql parancs tooconnect tooan Azure adatbázis PostgreSQL-kiszolgáló hello</span><span class="sxs-lookup"><span data-stu-id="58d24-148">Run hello following psql command tooconnect tooan Azure Database for PostgreSQL server</span></span>
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  <span data-ttu-id="58d24-149">Például a következő parancs hello csatlakozik toohello alapértelmezett adatbázis **postgres** a PostgreSQL kiszolgálón **mypgserver-20170401.postgres.database.azure.com** hozzáférési hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="58d24-149">For example, hello following command connects toohello default database called **postgres** on your PostgreSQL server **mypgserver-20170401.postgres.database.azure.com** using access credentials.</span></span> <span data-ttu-id="58d24-150">Adja meg a hello `<server_admin_password>` úgy döntött, hogy amikor a jelszó megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="58d24-150">Enter hello `<server_admin_password>` you chose when prompted for password.</span></span>
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 ---dbname=postgres
```

2.  <span data-ttu-id="58d24-151">Ha csatlakoztatott toohello kiszolgáló, hozzon létre egy üres adatbázist hello parancssorba.</span><span class="sxs-lookup"><span data-stu-id="58d24-151">Once you are connected toohello server, create a blank database at hello prompt.</span></span>
```sql
CREATE DATABASE mypgsqldb;
```

3.  <span data-ttu-id="58d24-152">Hello parancssorba hajtható végre a következő parancs tooswitch kapcsolat található, újonnan létrehozott toohello adatbázis hello **mypgsqldb**:</span><span class="sxs-lookup"><span data-stu-id="58d24-152">At hello prompt, execute hello following command tooswitch connection toohello newly created database **mypgsqldb**:</span></span>
```sql
\c mypgsqldb
```

## <a name="create-tables-in-hello-database"></a><span data-ttu-id="58d24-153">Táblázatok létrehozása hello adatbázis</span><span class="sxs-lookup"><span data-stu-id="58d24-153">Create tables in hello database</span></span>
<span data-ttu-id="58d24-154">Most, hogy tudja, hogyan tooconnect toohello PostgreSQL az Azure-adatbázis, azt is ismerteti hogyan toocomplete néhány alapvető műveleteket.</span><span class="sxs-lookup"><span data-stu-id="58d24-154">Now that you know how tooconnect toohello Azure Database for PostgreSQL, we can go over how toocomplete some basic tasks.</span></span>

<span data-ttu-id="58d24-155">Először hogy hozzon létre egy táblát, és töltse be adatokkal.</span><span class="sxs-lookup"><span data-stu-id="58d24-155">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="58d24-156">Hozzon létre egy táblát, amely nyomon követi a leltáradatokat.</span><span class="sxs-lookup"><span data-stu-id="58d24-156">Let's create a table that tracks inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

<span data-ttu-id="58d24-157">Újonnan létrehozott tábla hello listáját táblákat most beírásával hello látható:</span><span class="sxs-lookup"><span data-stu-id="58d24-157">You can see hello newly created table in hello list of tables now by typing:</span></span>
```sql
\dt
```

## <a name="load-data-into-hello-tables"></a><span data-ttu-id="58d24-158">Adatok betöltése az hello táblák</span><span class="sxs-lookup"><span data-stu-id="58d24-158">Load data into hello tables</span></span>
<span data-ttu-id="58d24-159">Most, hogy a tábla, azt beilleszthet néhány adat azt.</span><span class="sxs-lookup"><span data-stu-id="58d24-159">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="58d24-160">Hello nyissa meg a parancssort időszakban futtassa a következő lekérdezés tooinsert hello néhány sornyi adat</span><span class="sxs-lookup"><span data-stu-id="58d24-160">At hello open command prompt window, run hello following query tooinsert some rows of data</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="58d24-161">Lehetősége van a korábban létrehozott hello táblába mintaadatok most két sorát.</span><span class="sxs-lookup"><span data-stu-id="58d24-161">You have now two rows of sample data into hello table you created earlier.</span></span>

## <a name="query-and-update-hello-data-in-hello-tables"></a><span data-ttu-id="58d24-162">Hello táblázatok lekérdezési és frissítési hello adatait</span><span class="sxs-lookup"><span data-stu-id="58d24-162">Query and update hello data in hello tables</span></span>
<span data-ttu-id="58d24-163">Hajtható végre a következő lekérdezés tooretrieve információ hello adatbázis táblából hello.</span><span class="sxs-lookup"><span data-stu-id="58d24-163">Execute hello following query tooretrieve information from hello database table.</span></span> 
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="58d24-164">Hello hello-táblázatok adatait is frissítheti</span><span class="sxs-lookup"><span data-stu-id="58d24-164">You can also update hello data in hello tables</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="58d24-165">az adatok hello sor ennek megfelelően frissül.</span><span class="sxs-lookup"><span data-stu-id="58d24-165">hello row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a><span data-ttu-id="58d24-166">Egy adatbázis tooa korábbi időpontra időbeli visszaállítása</span><span class="sxs-lookup"><span data-stu-id="58d24-166">Restore a database tooa previous point in time</span></span>
<span data-ttu-id="58d24-167">Tegyük fel, hogy véletlenül törölt egy tábla.</span><span class="sxs-lookup"><span data-stu-id="58d24-167">Imagine you have accidentally deleted a table.</span></span> <span data-ttu-id="58d24-168">Ez a valami nem egyszerűen állíthat helyre.</span><span class="sxs-lookup"><span data-stu-id="58d24-168">This is something you cannot easily recover from.</span></span> <span data-ttu-id="58d24-169">Azure PostgreSQL-adatbázishoz lehetővé teszi (a legutóbbi too7 nap (alapszintű) és 35 napon (általános) hello) toogo hátsó tooany-időpontban, és állítsa vissza a időpontban tooa új kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="58d24-169">Azure Database for PostgreSQL allows you toogo back tooany point-in-time (in hello last up too7 days (Basic) and 35 days (Standard)) and restore this point-in-time tooa new server.</span></span> <span data-ttu-id="58d24-170">Az új kiszolgáló toorecover használhatja a törölt adatokat.</span><span class="sxs-lookup"><span data-stu-id="58d24-170">You can use this new server toorecover your deleted data.</span></span> <span data-ttu-id="58d24-171">hello lépéseket visszaállítási hello minta kiszolgáló tooa pont követően hello tábla hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="58d24-171">hello following steps restore hello sample server tooa point before hello table was added.</span></span>

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

<span data-ttu-id="58d24-172">Hello `az postgres server restore` parancsot kell a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="58d24-172">hello `az postgres server restore` command needs hello following parameters:</span></span>
| <span data-ttu-id="58d24-173">Beállítás</span><span class="sxs-lookup"><span data-stu-id="58d24-173">Setting</span></span> | <span data-ttu-id="58d24-174">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="58d24-174">Suggested value</span></span> | <span data-ttu-id="58d24-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="58d24-175">Description</span></span>  |
| --- | --- | --- |
| <span data-ttu-id="58d24-176">--Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="58d24-176">--resource-group</span></span> |  <span data-ttu-id="58d24-177">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="58d24-177">myResourceGroup</span></span> |  <span data-ttu-id="58d24-178">Az erőforráscsoport, mely hello forráskiszolgáló található.</span><span class="sxs-lookup"><span data-stu-id="58d24-178">The resource group in which hello source server exists.</span></span>  |
| <span data-ttu-id="58d24-179">--neve</span><span class="sxs-lookup"><span data-stu-id="58d24-179">--name</span></span> | <span data-ttu-id="58d24-180">mypgserver visszaállítása</span><span class="sxs-lookup"><span data-stu-id="58d24-180">mypgserver-restored</span></span> | <span data-ttu-id="58d24-181">új kiszolgáló hello hello visszaállítási parancs által létrehozott hello neve.</span><span class="sxs-lookup"><span data-stu-id="58d24-181">hello name of hello new server that is created by hello restore command.</span></span> |
| <span data-ttu-id="58d24-182">visszaállítás--időpontban</span><span class="sxs-lookup"><span data-stu-id="58d24-182">restore-point-in-time</span></span> | <span data-ttu-id="58d24-183">2017-04-13T13:59:00Z</span><span class="sxs-lookup"><span data-stu-id="58d24-183">2017-04-13T13:59:00Z</span></span> | <span data-ttu-id="58d24-184">Válassza ki a időpontban toorestore számára.</span><span class="sxs-lookup"><span data-stu-id="58d24-184">Select a point-in-time toorestore to.</span></span> <span data-ttu-id="58d24-185">A dátum és idő hello forráskiszolgáló biztonsági mentés megőrzési időn belül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="58d24-185">This date and time must be within hello source server's backup retention period.</span></span> <span data-ttu-id="58d24-186">Használjon ISO8601 dátum és idő formátumban.</span><span class="sxs-lookup"><span data-stu-id="58d24-186">Use ISO8601 date and time format.</span></span> <span data-ttu-id="58d24-187">Például használhatja a saját helyi időzóna, például a `2017-04-13T05:59:00-08:00`, vagy használjon UTC Zulu formátum `2017-04-13T13:59:00Z`.</span><span class="sxs-lookup"><span data-stu-id="58d24-187">For example, you may use your own local timezone, such as `2017-04-13T05:59:00-08:00`, or use UTC Zulu format `2017-04-13T13:59:00Z`.</span></span> |
| <span data-ttu-id="58d24-188">--forrás-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="58d24-188">--source-server</span></span> | <span data-ttu-id="58d24-189">mypgserver-20170401</span><span class="sxs-lookup"><span data-stu-id="58d24-189">mypgserver-20170401</span></span> | <span data-ttu-id="58d24-190">hello nevét vagy hello forrás server toorestore az azonosítója.</span><span class="sxs-lookup"><span data-stu-id="58d24-190">hello name or ID of hello source server toorestore from.</span></span> |

<span data-ttu-id="58d24-191">Egy kiszolgáló tooa-időpontban visszaállítása hoz létre a megadott időben frissítésétől hello pont hello eredeti kiszolgálóként másolása az új kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="58d24-191">Restoring a server tooa point-in-time creates a new server, copying as hello original server as of hello point in time you specify.</span></span> <span data-ttu-id="58d24-192">hello helyét, és vissza hello server tartozó díjszabási hello ugyanaz, mint a forráskiszolgálón hello történik.</span><span class="sxs-lookup"><span data-stu-id="58d24-192">hello location and pricing tier values for hello restored server are hello same as hello source server.</span></span>

<span data-ttu-id="58d24-193">hello parancs szinkron, és visszatér hello kiszolgáló helyreállítása után.</span><span class="sxs-lookup"><span data-stu-id="58d24-193">hello command is synchronous, and will return after hello server is restored.</span></span> <span data-ttu-id="58d24-194">Miután hello visszaállítás befejezését, keresse meg a hello létrehozott új kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="58d24-194">Once hello restore finishes, locate hello new server that was created.</span></span> <span data-ttu-id="58d24-195">Ellenőrizze a hello adatok helyreállt a várt módon.</span><span class="sxs-lookup"><span data-stu-id="58d24-195">Verify hello data was restored as expected.</span></span>


## <a name="next-steps"></a><span data-ttu-id="58d24-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="58d24-196">Next steps</span></span>
<span data-ttu-id="58d24-197">Ebben az oktatóanyagban, megtudta, hogyan toouse Azure CLI (parancssori felület) és egyéb segédprogramok:</span><span class="sxs-lookup"><span data-stu-id="58d24-197">In this tutorial, you learned how toouse Azure CLI (command-line interface) and other utilities to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="58d24-198">Azure-adatbázis létrehozása PostgreSQL-hez</span><span class="sxs-lookup"><span data-stu-id="58d24-198">Create an Azure Database for PostgreSQL</span></span>
> * <span data-ttu-id="58d24-199">Hello kiszolgáló tűzfal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="58d24-199">Configure hello server firewall</span></span>
> * <span data-ttu-id="58d24-200">Használjon [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) segédprogram toocreate adatbázis</span><span class="sxs-lookup"><span data-stu-id="58d24-200">Use [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility toocreate a database</span></span>
> * <span data-ttu-id="58d24-201">Mintaadatok betöltése</span><span class="sxs-lookup"><span data-stu-id="58d24-201">Load sample data</span></span>
> * <span data-ttu-id="58d24-202">Adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="58d24-202">Query data</span></span>
> * <span data-ttu-id="58d24-203">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="58d24-203">Update data</span></span>
> * <span data-ttu-id="58d24-204">Adatok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="58d24-204">Restore data</span></span>

<span data-ttu-id="58d24-205">A következő megtudhatja, hogyan toouse hello Azure portál toodo hasonló feladatokat, tekintse át az oktatóanyag: [kialakítást PostgreSQL használatával hello Azure-portálon az első Azure-adatbázis](tutorial-design-database-using-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="58d24-205">Next, learn how toouse hello Azure portal toodo similar tasks, review this tutorial: [Design your first Azure Database for PostgreSQL using hello Azure portal](tutorial-design-database-using-azure-portal.md)</span></span>
