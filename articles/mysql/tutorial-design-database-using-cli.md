---
title: "az első Azure adatbázis a MySQL-adatbázis - Azure CLI aaaDesign |} Microsoft Docs"
description: "Ez az oktatóanyag azt ismerteti, hogyan toocreate és kezelése az Azure-adatbázis MySQL-kiszolgáló és az adatbázis-Azure CLI 2.0 használatával hello parancssorból."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.custom: mvc
ms.openlocfilehash: 6339913c2af58e0e4c4eabb69097a5c9c245781c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a><span data-ttu-id="842f3-103">Az első Azure-adatbázis MySQL-adatbázis megtervezése</span><span class="sxs-lookup"><span data-stu-id="842f3-103">Design your first Azure Database for MySQL database</span></span>

<span data-ttu-id="842f3-104">A MySQL az Azure adatbázisa a hello MySQL Community Edition adatbázis-kezelő a Microsoft felhőalapú relációs adatbázis-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="842f3-104">Azure Database for MySQL is a relational database service in hello Microsoft cloud based on MySQL Community Edition database engine.</span></span> <span data-ttu-id="842f3-105">Ebben az oktatóanyagban használhatja az Azure CLI (parancssori felület) és egyéb segédprogramok toolearn hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="842f3-105">In this tutorial, you use Azure CLI (command-line interface) and other utilities toolearn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="842f3-106">A MySQL az Azure-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="842f3-106">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="842f3-107">Hello kiszolgáló tűzfal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="842f3-107">Configure hello server firewall</span></span>
> * <span data-ttu-id="842f3-108">Használjon [mysql parancssori eszköz](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate adatbázis</span><span class="sxs-lookup"><span data-stu-id="842f3-108">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate a database</span></span>
> * <span data-ttu-id="842f3-109">Mintaadatok betöltése</span><span class="sxs-lookup"><span data-stu-id="842f3-109">Load sample data</span></span>
> * <span data-ttu-id="842f3-110">Adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="842f3-110">Query data</span></span>
> * <span data-ttu-id="842f3-111">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="842f3-111">Update data</span></span>
> * <span data-ttu-id="842f3-112">Adatok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="842f3-112">Restore data</span></span>

<span data-ttu-id="842f3-113">Azure Cloud rendszerhéj hello hello böngészőben használhatja vagy [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli) a saját számítógép toorun hello kódblokkok ebben az oktatóanyagban.</span><span class="sxs-lookup"><span data-stu-id="842f3-113">You may use hello Azure Cloud Shell in hello browser, or [Install Azure CLI 2.0]( /cli/azure/install-azure-cli) on your own computer toorun hello code blocks in this tutorial.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="842f3-114">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="842f3-114">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="842f3-115">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="842f3-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="842f3-116">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="842f3-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="842f3-117">Ha több előfizetéssel rendelkezik, válassza ki a megfelelő előfizetés hello hello erőforrás létezik-e vagy a lesz számlázva.</span><span class="sxs-lookup"><span data-stu-id="842f3-117">If you have multiple subscriptions, choose hello appropriate subscription in which hello resource exists or is billed for.</span></span> <span data-ttu-id="842f3-118">Válasszon ki egy megadott előfizetés-azonosítót a fiókja alatt az [az account set](/cli/azure/account#set) parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="842f3-118">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="842f3-119">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="842f3-119">Create a resource group</span></span>
<span data-ttu-id="842f3-120">Hozzon létre egy [Azure erőforráscsoport](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) rendelkező [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="842f3-120">Create an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) with [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> <span data-ttu-id="842f3-121">Az erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="842f3-121">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span>

<span data-ttu-id="842f3-122">hello alábbi példa létrehoz egy erőforráscsoportot `mycliresource` a hello `westus` helyét.</span><span class="sxs-lookup"><span data-stu-id="842f3-122">hello following example creates a resource group named `mycliresource` in hello `westus` location.</span></span>

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="842f3-123">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="842f3-123">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="842f3-124">Azure-adatbázis létrehozása hello az mysql kiszolgálóval MySQL kiszolgálóra létre parancsot.</span><span class="sxs-lookup"><span data-stu-id="842f3-124">Create an Azure Database for MySQL server with hello az mysql server create command.</span></span> <span data-ttu-id="842f3-125">Egy kiszolgáló több adatbázist is tud kezelni.</span><span class="sxs-lookup"><span data-stu-id="842f3-125">A server can manage multiple databases.</span></span> <span data-ttu-id="842f3-126">Általában külön adatbázissal rendelkezik minden projekt vagy felhasználó.</span><span class="sxs-lookup"><span data-stu-id="842f3-126">Typically, a separate database is used for each project or for each user.</span></span>

<span data-ttu-id="842f3-127">hello alábbi példa adatbázist hoz létre Azure található MySQL kiszolgáló `westus` hello erőforráscsoportban `mycliresource` nevű `mycliserver`.</span><span class="sxs-lookup"><span data-stu-id="842f3-127">hello following example creates an Azure Database for MySQL server located in `westus` in hello resource group `mycliresource` with name `mycliserver`.</span></span> <span data-ttu-id="842f3-128">hello kiszolgáló rendelkezik egy rendszergazdanaplójában rögzíti a nevű `myadmin` és a jelszó `Password01!`.</span><span class="sxs-lookup"><span data-stu-id="842f3-128">hello server has an administrator log in named `myadmin` and password `Password01!`.</span></span> <span data-ttu-id="842f3-129">hello kiszolgáló jön létre a **alapvető** teljesítményszinttel és **50** hello kiszolgáló összes hello adatbázisának között megosztott egységek számítási.</span><span class="sxs-lookup"><span data-stu-id="842f3-129">hello server is created with **Basic** performance tier and **50** compute units shared between all hello databases in hello server.</span></span> <span data-ttu-id="842f3-130">Méretezheti számítási és tárolási felfelé vagy lefelé hello alkalmazás igényeinek.</span><span class="sxs-lookup"><span data-stu-id="842f3-130">You can scale compute and storage up or down depending on hello application needs.</span></span>

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver
--location westus --user myadmin --password Password01!
--performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a><span data-ttu-id="842f3-131">Tűzfalszabály konfigurálása</span><span class="sxs-lookup"><span data-stu-id="842f3-131">Configure firewall rule</span></span>
<span data-ttu-id="842f3-132">Azure-adatbázis létrehozása a MySQL kiszolgálószintű tűzfalszabályt a hello az mysql-tűzfalszabály létrehozása parancs.</span><span class="sxs-lookup"><span data-stu-id="842f3-132">Create an Azure Database for MySQL server-level firewall rule with hello az mysql server firewall-rule create command.</span></span> <span data-ttu-id="842f3-133">Egy kiszolgálószintű tűzfalszabályt lehetővé teszi a külső alkalmazás, például a **mysql** parancssori eszköz vagy a MySQL munkaterület tooconnect tooyour server hello Azure MySQL szolgáltatás tűzfalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="842f3-133">A server-level firewall rule allows an external application, such as **mysql** command-line tool or MySQL Workbench tooconnect tooyour server through hello Azure MySQL service firewall.</span></span> 

<span data-ttu-id="842f3-134">hello alábbi példa létrehoz egy előre meghatározott címtartományba egy tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="842f3-134">hello following example creates a firewall rule for a predefined address range.</span></span> <span data-ttu-id="842f3-135">Ez a példa bemutatja a hello teljes lehetséges az IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="842f3-135">This example shows hello entire possible range of IP addresses.</span></span>

```azurecli-interactive
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="get-hello-connection-information"></a><span data-ttu-id="842f3-136">Hello kapcsolatadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="842f3-136">Get hello connection information</span></span>

<span data-ttu-id="842f3-137">tooconnect tooyour kiszolgáló, tooprovide állomás információkat és a hozzáférési hitelesítő adatok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="842f3-137">tooconnect tooyour server, you need tooprovide host information and access credentials.</span></span>
```azurecli-interactive
az mysql server show --resource-group mycliresource --name mycliserver
```

<span data-ttu-id="842f3-138">hello eredménye JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="842f3-138">hello result is in JSON format.</span></span> <span data-ttu-id="842f3-139">Jegyezze fel a hello **fullyQualifiedDomainName** és **administratorLogin**.</span><span class="sxs-lookup"><span data-stu-id="842f3-139">Make a note of hello **fullyQualifiedDomainName** and **administratorLogin**.</span></span>
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mycliserver.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mycliresource/providers/Microsoft.DBforMySQL/servers/mycliserver",
  "location": "westus",
  "name": "mycliserver",
  "resourceGroup": "mycliresource",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "MYSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

## <a name="connect-toohello-server-using-mysql"></a><span data-ttu-id="842f3-140">Csatlakoztassa a kiszolgálót a mysql használata toohello</span><span class="sxs-lookup"><span data-stu-id="842f3-140">Connect toohello server using mysql</span></span>
<span data-ttu-id="842f3-141">Használjon [mysql parancssori eszköz](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) tooestablish MySQL-kiszolgáló kapcsolati tooyour Azure-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="842f3-141">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) tooestablish a connection tooyour Azure Database for MySQL server.</span></span> <span data-ttu-id="842f3-142">Ebben a példában a rendszer hello paranccsal:</span><span class="sxs-lookup"><span data-stu-id="842f3-142">In this example, hello command is:</span></span>
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a><span data-ttu-id="842f3-143">Hozzon létre egy üres adatbázist</span><span class="sxs-lookup"><span data-stu-id="842f3-143">Create a blank database</span></span>
<span data-ttu-id="842f3-144">Ha már csatlakoztatott toohello kiszolgáló, egy üres adatbázis létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="842f3-144">Once you’re connected toohello server, create a blank database.</span></span>
```sql
mysql> CREATE DATABASE mysampledb;
```

<span data-ttu-id="842f3-145">Hello a parancssorból futtassa a következő parancs tooswitch hello kapcsolat található, újonnan létrehozott toothis adatbázis hello:</span><span class="sxs-lookup"><span data-stu-id="842f3-145">At hello prompt, run hello following command tooswitch hello connection toothis newly created database:</span></span>
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-hello-database"></a><span data-ttu-id="842f3-146">Táblázatok létrehozása hello adatbázis</span><span class="sxs-lookup"><span data-stu-id="842f3-146">Create tables in hello database</span></span>
<span data-ttu-id="842f3-147">Most, hogy tudja, hogyan tooconnect toohello Azure adatbázis MySQL-adatbázis, azt is ismerteti hogyan toocomplete néhány alapvető műveleteket.</span><span class="sxs-lookup"><span data-stu-id="842f3-147">Now that you know how tooconnect toohello Azure Database for MySQL database, we can go over how toocomplete some basic tasks.</span></span>

<span data-ttu-id="842f3-148">Először hogy hozzon létre egy táblát, és töltse be adatokkal.</span><span class="sxs-lookup"><span data-stu-id="842f3-148">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="842f3-149">Hozzon létre egy tábla tárolja a leltáradatokat.</span><span class="sxs-lookup"><span data-stu-id="842f3-149">Let's create a table that stores inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-hello-tables"></a><span data-ttu-id="842f3-150">Adatok betöltése az hello táblák</span><span class="sxs-lookup"><span data-stu-id="842f3-150">Load data into hello tables</span></span>
<span data-ttu-id="842f3-151">Most, hogy a tábla, azt beilleszthet néhány adat azt.</span><span class="sxs-lookup"><span data-stu-id="842f3-151">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="842f3-152">Hello nyissa meg a parancssort időszakban futtassa a következő lekérdezés tooinsert hello néhány sornyi adatot.</span><span class="sxs-lookup"><span data-stu-id="842f3-152">At hello open command prompt window, run hello following query tooinsert some rows of data.</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="842f3-153">Most már rendelkezik a korábban létrehozott hello táblába mintaadatok két sorát.</span><span class="sxs-lookup"><span data-stu-id="842f3-153">Now you have two rows of sample data into hello table you created earlier.</span></span>

## <a name="query-and-update-hello-data-in-hello-tables"></a><span data-ttu-id="842f3-154">Hello táblázatok lekérdezési és frissítési hello adatait</span><span class="sxs-lookup"><span data-stu-id="842f3-154">Query and update hello data in hello tables</span></span>
<span data-ttu-id="842f3-155">Hajtható végre a következő lekérdezés tooretrieve információ hello adatbázis táblából hello.</span><span class="sxs-lookup"><span data-stu-id="842f3-155">Execute hello following query tooretrieve information from hello database table.</span></span>
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="842f3-156">Hello hello-táblázatok adatait is frissítheti.</span><span class="sxs-lookup"><span data-stu-id="842f3-156">You can also update hello data in hello tables.</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="842f3-157">az adatok hello sor ennek megfelelően frissül.</span><span class="sxs-lookup"><span data-stu-id="842f3-157">hello row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a><span data-ttu-id="842f3-158">Egy adatbázis tooa korábbi időpontra időbeli visszaállítása</span><span class="sxs-lookup"><span data-stu-id="842f3-158">Restore a database tooa previous point in time</span></span>
<span data-ttu-id="842f3-159">Képzelje el ezt a táblázatot véletlenül törölt.</span><span class="sxs-lookup"><span data-stu-id="842f3-159">Imagine you have accidentally deleted this table.</span></span> <span data-ttu-id="842f3-160">Ez a valami nem egyszerűen állíthat helyre.</span><span class="sxs-lookup"><span data-stu-id="842f3-160">This is something you cannot easily recover from.</span></span> <span data-ttu-id="842f3-161">Azure MySQL-adatbázis lehetővé teszi toogo hátsó tooany pont hello időpont utolsó too35 nap mentése és visszaállítása ezen a pontján tooa új kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="842f3-161">Azure Database for MySQL allows you toogo back tooany point in time in hello last up too35 days and restore this point in time tooa new server.</span></span> <span data-ttu-id="842f3-162">Az új kiszolgáló toorecover használhatja a törölt adatokat.</span><span class="sxs-lookup"><span data-stu-id="842f3-162">You can use this new server toorecover your deleted data.</span></span> <span data-ttu-id="842f3-163">hello lépéseket visszaállítási hello minta kiszolgáló tooa pont követően hello tábla hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="842f3-163">hello following steps restore hello sample server tooa point before hello table was added.</span></span>

<span data-ttu-id="842f3-164">Hello visszaállítási szükséges információkat a következő hello:</span><span class="sxs-lookup"><span data-stu-id="842f3-164">For hello Restore you need hello following information:</span></span>

- <span data-ttu-id="842f3-165">Visszaállítási pont: Válasszon egy pont időponthoz kötött, amely hello server módosítása előtt következik be.</span><span class="sxs-lookup"><span data-stu-id="842f3-165">Restore point: Select a point-in-time that occurs before hello server was changed.</span></span> <span data-ttu-id="842f3-166">Nagyobb, mint vagy egyenlő toohello source adatbázis legrégebbi biztonsági mentési értékével.</span><span class="sxs-lookup"><span data-stu-id="842f3-166">Must be greater than or equal toohello source database's Oldest backup value.</span></span>
- <span data-ttu-id="842f3-167">Célkiszolgáló: Adjon meg egy új kiszolgálónevet azt szeretné, hogy toorestore</span><span class="sxs-lookup"><span data-stu-id="842f3-167">Target server: Provide a new server name you want toorestore to</span></span>
- <span data-ttu-id="842f3-168">Forráskiszolgáló: hello nevezze azt szeretné, hogy a toorestore hello kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="842f3-168">Source server: Provide hello name of hello server you want toorestore from</span></span>
- <span data-ttu-id="842f3-169">Hely: Nem választhat ki hello régió, alapértelmezés szerint ugyanaz, mint a forráskiszolgálón hello</span><span class="sxs-lookup"><span data-stu-id="842f3-169">Location: You cannot select hello region, by default it is same as hello source server</span></span>

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

<span data-ttu-id="842f3-170">toorestore hello kiszolgáló és [tooa pont időponthoz kötött visszaállítás](./howto-restore-server-portal.md) előtt hello tábla törölve lett.</span><span class="sxs-lookup"><span data-stu-id="842f3-170">toorestore hello server and [restore tooa point-in-time](./howto-restore-server-portal.md) before hello table was deleted.</span></span> <span data-ttu-id="842f3-171">A kiszolgáló tooa különböző pont visszaállítása időben ismétlődő új kiszolgáló létrehozása hello eredeti kiszolgálóként hello pont időben ad meg, feltéve, hogy a hello megőrzési időtartamon belül a [szolgáltatásréteg](./concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="842f3-171">Restoring a server tooa different point in time creates a duplicate new server as hello original server as of hello point in time you specify, provided that it is within hello retention period for your [service tier](./concepts-service-tiers.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="842f3-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="842f3-172">Next Steps</span></span>
<span data-ttu-id="842f3-173">Ez az oktatóanyag megtanulta, hogy:</span><span class="sxs-lookup"><span data-stu-id="842f3-173">In this tutorial you learned to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="842f3-174">A MySQL az Azure-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="842f3-174">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="842f3-175">Hello kiszolgáló tűzfal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="842f3-175">Configure hello server firewall</span></span>
> * <span data-ttu-id="842f3-176">Használjon [mysql parancssori eszköz](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate adatbázis</span><span class="sxs-lookup"><span data-stu-id="842f3-176">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate a database</span></span>
> * <span data-ttu-id="842f3-177">Mintaadatok betöltése</span><span class="sxs-lookup"><span data-stu-id="842f3-177">Load sample data</span></span>
> * <span data-ttu-id="842f3-178">Adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="842f3-178">Query data</span></span>
> * <span data-ttu-id="842f3-179">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="842f3-179">Update data</span></span>
> * <span data-ttu-id="842f3-180">Adatok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="842f3-180">Restore data</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="842f3-181">Azure-adatbázis a MySQL - Azure CLI-minták</span><span class="sxs-lookup"><span data-stu-id="842f3-181">Azure Database for MySQL - Azure CLI samples</span></span>](./sample-scripts-azure-cli.md)
