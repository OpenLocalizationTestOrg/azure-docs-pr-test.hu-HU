---
title: "Első lépések: Azure-adatbázis létrehozása MySQL-kiszolgálóhoz - Azure CLI | Microsoft Docs"
description: "Ez a rövid útmutató bemutatja, hogyan hozhat létre Azure-adatbázist MySQL-kiszolgálóhoz az Azure CLI használatával az Azure-erőforráscsoportban."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: hero-article
ms.date: 06/13/2017
ms.custom: mvc
ms.openlocfilehash: 04fc441aee7a4c8adc4f02d5e51b2d9e64400f55
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a><span data-ttu-id="37a30-103">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="37a30-103">Create an Azure Database for MySQL server using Azure CLI</span></span>
<span data-ttu-id="37a30-104">Ez a rövid útmutató bemutatja, hogyan hozhat létre öt perc alatt egy Azure-adatbázist MySQL-kiszolgálóhoz az Azure CLI használatával az Azure-erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="37a30-104">This quickstart describes how to use the Azure CLI to create an Azure Database for MySQL server in an Azure resource group in about five minutes.</span></span> <span data-ttu-id="37a30-105">Az Azure CLI az Azure-erőforrások parancssorból vagy szkriptekkel történő létrehozására és kezelésére használható.</span><span class="sxs-lookup"><span data-stu-id="37a30-105">The Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span>

<span data-ttu-id="37a30-106">Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.</span><span class="sxs-lookup"><span data-stu-id="37a30-106">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="37a30-107">Ha a CLI helyi telepítését és használatát választja, akkor ehhez a témakörhöz az Azure CLI 2.0-s vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="37a30-107">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="37a30-108">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="37a30-108">Run `az --version` to find the version.</span></span> <span data-ttu-id="37a30-109">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="37a30-109">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="37a30-110">Ha több előfizetéssel rendelkezik, válassza a megfelelő előfizetést, amelyen az erőforrás megtalálható vagy terhelve van.</span><span class="sxs-lookup"><span data-stu-id="37a30-110">If you have multiple subscriptions, choose the appropriate subscription in which the resource exists or is billed for.</span></span> <span data-ttu-id="37a30-111">Válasszon ki egy megadott előfizetés-azonosítót a fiókja alatt az [az account set](/cli/azure/account#set) parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="37a30-111">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="37a30-112">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="37a30-112">Create a resource group</span></span>
<span data-ttu-id="37a30-113">Hozzon létre egy [Azure-erőforráscsoportot](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) az [az group create](https://docs.microsoft.com/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="37a30-113">Create an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) using the [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> <span data-ttu-id="37a30-114">Az erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="37a30-114">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span>

<span data-ttu-id="37a30-115">A következő példában létrehozunk egy `westus` nevű erőforráscsoportot a `myresourcegroup` helyen.</span><span class="sxs-lookup"><span data-stu-id="37a30-115">The following example creates a resource group named `myresourcegroup` in the `westus` location.</span></span>

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="37a30-116">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="37a30-116">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="37a30-117">Hozzon létre egy Azure-adatbázist MySQL-kiszolgálóhoz az **az mysql server create** paranccsal.</span><span class="sxs-lookup"><span data-stu-id="37a30-117">Create an Azure Database for MySQL server with the **az mysql server create** command.</span></span> <span data-ttu-id="37a30-118">Egy kiszolgáló több adatbázist is tud kezelni.</span><span class="sxs-lookup"><span data-stu-id="37a30-118">A server can manage multiple databases.</span></span> <span data-ttu-id="37a30-119">Általában külön adatbázissal rendelkezik minden projekt vagy felhasználó.</span><span class="sxs-lookup"><span data-stu-id="37a30-119">Typically, a separate database is used for each project or for each user.</span></span>

<span data-ttu-id="37a30-120">A következő példában létrehozunk egy `myserver4demo` nevű Azure-adatbázist MySQL-kiszolgálóhoz a `myresourcegroup` erőforráscsoportban a `westus`-ben.</span><span class="sxs-lookup"><span data-stu-id="37a30-120">The following example creates an Azure Database for MySQL server located in `westus` in the resource group `myresourcegroup` with name `myserver4demo`.</span></span> <span data-ttu-id="37a30-121">A kiszolgáló rendelkezik egy `myadmin` nevű rendszergazdai fiókkal, amelyhez a jelszó `Password01!`.</span><span class="sxs-lookup"><span data-stu-id="37a30-121">The server has an administrator log in named `myadmin` and password `Password01!`.</span></span> <span data-ttu-id="37a30-122">A kiszolgáló **Alapszintű** teljesítményszinttel van létrehozva, valamint **50** számítási egység van megosztva a kiszolgálón lévő összes adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="37a30-122">The server is created with **Basic** performance tier and **50** compute units shared between all the databases in the server.</span></span> <span data-ttu-id="37a30-123">Az alkalmazás szükségleteitől függően csökkentheti vagy növelheti a számítási egységeket és a tárterületet.</span><span class="sxs-lookup"><span data-stu-id="37a30-123">You can scale compute and storage up or down depending on the application needs.</span></span>

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name myserver4demo --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a><span data-ttu-id="37a30-124">Tűzfalszabály konfigurálása</span><span class="sxs-lookup"><span data-stu-id="37a30-124">Configure firewall rule</span></span>
<span data-ttu-id="37a30-125">Hozzon létre egy Azure-adatbázis MySQL-kiszolgálóhoz-szintű tűzfalszabályt az **az mysql server firewall-rule create** parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="37a30-125">Create an Azure Database for MySQL server-level firewall rule using the **az mysql server firewall-rule create** command.</span></span> <span data-ttu-id="37a30-126">Egy kiszolgálószintű tűzfalszabály lehetővé teszi olyan külső alkalmazások használatát, mint a **mysql.exe** parancssori eszköz vagy a MySQL Workbench, amelyekkel kapcsolódhat a kiszolgálóhoz az Azure MySQL szolgáltatás tűzfalán keresztül.</span><span class="sxs-lookup"><span data-stu-id="37a30-126">A server-level firewall rule allows an external application, such as the **mysql.exe** command-line tool or MySQL Workbench to connect to your server through the Azure MySQL service firewall.</span></span> 

<span data-ttu-id="37a30-127">A következő példában létrehozunk egy tűzfalszabályt egy előre meghatározott címtartományhoz, amely ebben a példában az IP-címek teljes lehetséges tartományát lefedi.</span><span class="sxs-lookup"><span data-stu-id="37a30-127">The following example creates a firewall rule for a predefined address range, which in this example is the entire possible range of IP addresses.</span></span>

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server myserver4demo --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a><span data-ttu-id="37a30-128">Az SSL-beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="37a30-128">Configure SSL settings</span></span>
<span data-ttu-id="37a30-129">Alapértelmezés szerint a kiszolgáló és az ügyfélalkalmazások közti SSL-kapcsolatok kényszerítve vannak.</span><span class="sxs-lookup"><span data-stu-id="37a30-129">By default, SSL connections between your server and client applications are enforced.</span></span>  <span data-ttu-id="37a30-130">Ez biztosítja a „mozgó” adatok biztonságát az adatfolyam interneten keresztüli titkosításával.</span><span class="sxs-lookup"><span data-stu-id="37a30-130">This ensures security of "in-motion" data by encrypting the data stream over the internet.</span></span>  <span data-ttu-id="37a30-131">Ahhoz, hogy ez a gyorsútmutató egyszerűbb legyen, kiszolgálóján letiltjuk az SSL-kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="37a30-131">To make this quick start easier, we disable SSL connections for your server.</span></span>  <span data-ttu-id="37a30-132">Ez éles kiszolgálók esetében nem javasolt.</span><span class="sxs-lookup"><span data-stu-id="37a30-132">This is not recommended for production servers.</span></span>  <span data-ttu-id="37a30-133">További információkért lásd [Az SSL-kapcsolatok a MySQL-hez készült Azure Database-hez való kapcsolódásra az alkalmazásban való konfigurálását](./howto-configure-ssl.md) bemutató cikket.</span><span class="sxs-lookup"><span data-stu-id="37a30-133">For more information, see [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md).</span></span>

<span data-ttu-id="37a30-134">A következő példában letiltjuk az SSL kényszerítését a MySQL-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="37a30-134">The following example disables enforcing SSL on your MySQL server.</span></span>
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name myserver4demo -g -n --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a><span data-ttu-id="37a30-135">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="37a30-135">Get the connection information</span></span>

<span data-ttu-id="37a30-136">A kiszolgálóhoz való kapcsolódáshoz meg kell adnia a gazdagép adatait és a hozzáférési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="37a30-136">To connect to your server, you need to provide host information and access credentials.</span></span>

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name myserver4demo
```

<span data-ttu-id="37a30-137">Az eredmény JSON formátumban van.</span><span class="sxs-lookup"><span data-stu-id="37a30-137">The result is in JSON format.</span></span> <span data-ttu-id="37a30-138">Jegyezze fel a következőket: **fullyQualifiedDomainName** és **administratorLogin**.</span><span class="sxs-lookup"><span data-stu-id="37a30-138">Make a note of the **fullyQualifiedDomainName** and **administratorLogin**.</span></span>
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "myserver4demo.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/servers/myserver4demo",
  "location": "westus",
  "name": "myserver4demo",
  "resourceGroup": "myresourcegroup",
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

## <a name="connect-to-the-server-using-the-mysqlexe-command-line-tool"></a><span data-ttu-id="37a30-139">Csatlakozás a kiszolgálóhoz a mysql.exe parancssori eszköz használatával</span><span class="sxs-lookup"><span data-stu-id="37a30-139">Connect to the server using the mysql.exe command-line tool</span></span>
<span data-ttu-id="37a30-140">Csatlakozzon kiszolgálójához a **mysql.exe** parancssori eszközzel.</span><span class="sxs-lookup"><span data-stu-id="37a30-140">Connect to your server using the **mysql.exe** command-line tool.</span></span> <span data-ttu-id="37a30-141">A MySQL-t [innen](https://dev.mysql.com/downloads/) töltheti le és telepítheti számítógépén.</span><span class="sxs-lookup"><span data-stu-id="37a30-141">You can download MySQL from [here](https://dev.mysql.com/downloads/) and install it on your computer.</span></span> <span data-ttu-id="37a30-142">Vagy kattinthat a kódmintákban található **Kipróbálom** gombra vagy az Azure Portal jobb felső eszköztárán található `>_` gombra is az **Azure Cloud Shell** megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="37a30-142">Instead you may also click the **Try It** button on code samples, or the  `>_` button from the upper right toolbar in the Azure portal, and launch the **Azure Cloud Shell**.</span></span>

<span data-ttu-id="37a30-143">Írja be a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="37a30-143">Type the next commands:</span></span> 

1. <span data-ttu-id="37a30-144">Csatlakozás a kiszolgálóhoz a **mysql** parancssori eszköz használatával:</span><span class="sxs-lookup"><span data-stu-id="37a30-144">Connect to the server using **mysql** command-line tool:</span></span>
```azurecli-interactive
 mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

2. <span data-ttu-id="37a30-145">Kiszolgáló állapotának megtekintése:</span><span class="sxs-lookup"><span data-stu-id="37a30-145">View server status:</span></span>
```sql
 mysql> status
```
<span data-ttu-id="37a30-146">Ha minden megfelelően működik, a parancssori eszköz a következő szöveget jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="37a30-146">If everything goes well, the command-line tool should output the following text:</span></span>

```dos
C:\Users\>mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.6.35, for Win64 (x86_64)

Connection id:          65512
Current database:
Current user:           myadmin@116.230.243.143
SSL:                    Not in use
Using delimiter:        ;
Server version:         5.6.26.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             myserver4demo.mysql.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    gbk
Conn.  characterset:    gbk
TCP port:               3306
Uptime:                 2 days 9 hours 47 min 20 sec

Threads: 4  Questions: 34833  Slow queries: 2  Opens: 84  Flush tables: 4  Open tables: 1  Queries per second avg: 0.167
--------------

mysql>
```

> [!TIP]
> <span data-ttu-id="37a30-147">További parancsokért lásd: [az MySQL 5.7 referenciaútmutatójának 4.5.1-es fejezetét](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).</span><span class="sxs-lookup"><span data-stu-id="37a30-147">For additional commands, see [MySQL 5.7 Reference Manual - Chapter 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).</span></span>

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a><span data-ttu-id="37a30-148">Csatlakozás a kiszolgálóhoz a MySQL Workbench GUI eszköz használatával</span><span class="sxs-lookup"><span data-stu-id="37a30-148">Connect to the server using the MySQL Workbench GUI tool</span></span>
1.  <span data-ttu-id="37a30-149">Indítsa el a MySQL Workbench alkalmazást az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="37a30-149">Launch the MySQL Workbench application on your client computer.</span></span> <span data-ttu-id="37a30-150">A MySQL Workbench-et [innen](https://dev.mysql.com/downloads/workbench/) töltheti le és telepítheti.</span><span class="sxs-lookup"><span data-stu-id="37a30-150">You can download and install MySQL Workbench from [here](https://dev.mysql.com/downloads/workbench/).</span></span>

2.  <span data-ttu-id="37a30-151">Az **Új kapcsolat létrehozása** párbeszédpanelen adja meg a következő információkat a **Paraméterek** lapon:</span><span class="sxs-lookup"><span data-stu-id="37a30-151">In the **Setup New Connection** dialog box, enter the following information on **Parameters** tab:</span></span>

   ![új kapcsolat beállítása](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

| <span data-ttu-id="37a30-153">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="37a30-153">**Setting**</span></span> | <span data-ttu-id="37a30-154">**Ajánlott érték**</span><span class="sxs-lookup"><span data-stu-id="37a30-154">**Suggested Value**</span></span> | <span data-ttu-id="37a30-155">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="37a30-155">**Description**</span></span> |
|---|---|---|
|   <span data-ttu-id="37a30-156">Kapcsolat neve</span><span class="sxs-lookup"><span data-stu-id="37a30-156">Connection Name</span></span> | <span data-ttu-id="37a30-157">My Connection</span><span class="sxs-lookup"><span data-stu-id="37a30-157">My Connection</span></span> | <span data-ttu-id="37a30-158">Adjon meg egy címkét a kapcsolathoz (tetszőlegesen kiválasztható)</span><span class="sxs-lookup"><span data-stu-id="37a30-158">Specify a label for this connection (this can be anything)</span></span> |
| <span data-ttu-id="37a30-159">Kapcsolati módszer</span><span class="sxs-lookup"><span data-stu-id="37a30-159">Connection Method</span></span> | <span data-ttu-id="37a30-160">válassza a Standard (TCP/IP) lehetőséget</span><span class="sxs-lookup"><span data-stu-id="37a30-160">choose Standard (TCP/IP)</span></span> | <span data-ttu-id="37a30-161">Csatlakozzon a MySQL-hez készült Azure Database-hez a TCP/IP protokollal></span><span class="sxs-lookup"><span data-stu-id="37a30-161">Use TCP/IP protocol to connect to Azure Database for MySQL></span></span> |
| <span data-ttu-id="37a30-162">Gazdanév</span><span class="sxs-lookup"><span data-stu-id="37a30-162">Hostname</span></span> | <span data-ttu-id="37a30-163">myserver4demo.mysql.database.azure.com</span><span class="sxs-lookup"><span data-stu-id="37a30-163">myserver4demo.mysql.database.azure.com</span></span> | <span data-ttu-id="37a30-164">A korábban feljegyzett kiszolgálónév.</span><span class="sxs-lookup"><span data-stu-id="37a30-164">Server name you previously noted.</span></span> |
| <span data-ttu-id="37a30-165">Port</span><span class="sxs-lookup"><span data-stu-id="37a30-165">Port</span></span> | <span data-ttu-id="37a30-166">3306</span><span class="sxs-lookup"><span data-stu-id="37a30-166">3306</span></span> | <span data-ttu-id="37a30-167">A rendszer a MySQL alapértelmezett portját használja.</span><span class="sxs-lookup"><span data-stu-id="37a30-167">The default port for MySQL is used.</span></span> |
| <span data-ttu-id="37a30-168">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="37a30-168">Username</span></span> | myadmin@myserver4demo | <span data-ttu-id="37a30-169">A korábban feljegyzett kiszolgáló-rendszergazdai bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="37a30-169">The server admin login you previously noted.</span></span> |
| <span data-ttu-id="37a30-170">Jelszó</span><span class="sxs-lookup"><span data-stu-id="37a30-170">Password</span></span> | **** | <span data-ttu-id="37a30-171">Használja a korábban beállított rendszergazdaifiók-jelszót.</span><span class="sxs-lookup"><span data-stu-id="37a30-171">Use the admin account password you configured earlier.</span></span> |

<span data-ttu-id="37a30-172">Kattintson a **Kapcsolat tesztelése** lehetőségre, hogy tesztelje, minden paraméter helyesen lett-e konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="37a30-172">Click **Test Connection** to test if all parameters are correctly configured.</span></span>
<span data-ttu-id="37a30-173">Ezután a kapcsolatra való kattintva sikeresen csatlakozhat a kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="37a30-173">Now, you can click the connection to successfully connect to the server.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="37a30-174">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="37a30-174">Clean up resources</span></span>
<span data-ttu-id="37a30-175">Ha ezekre az erőforrásokra már nincs szüksége más gyorsútmutatókhoz/oktatóanyagokhoz, a következő paranccsal törölheti őket:</span><span class="sxs-lookup"><span data-stu-id="37a30-175">If you don't need these resources for another quickstart/tutorial, you can delete them by doing the following command:</span></span> 

```azurecli-interactive
az group delete --name myresourcegroup
```

## <a name="next-steps"></a><span data-ttu-id="37a30-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="37a30-176">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="37a30-177">MySQL-adatbázis tervezése az Azure CLI-vel</span><span class="sxs-lookup"><span data-stu-id="37a30-177">Design a MySQL Database with Azure CLI</span></span>](./tutorial-design-database-using-cli.md)
