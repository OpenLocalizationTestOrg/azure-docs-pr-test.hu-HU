---
title: "Első lépések: Azure-adatbázis létrehozása MySQL-kiszolgálóhoz - Azure CLI | Microsoft Docs"
description: "A gyors üzembe helyezés ismerteti, hogyan toouse hello Azure CLI toocreate egy Azure-adatbázis egy Azure-erőforráscsoportot a MySQL-kiszolgáló."
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
ms.openlocfilehash: 708d0cce12e812cb464adcf7e83e6f85c196bafe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a><span data-ttu-id="de5a7-103">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="de5a7-103">Create an Azure Database for MySQL server using Azure CLI</span></span>
<span data-ttu-id="de5a7-104">A gyors üzembe helyezés ismerteti, hogyan toouse hello Azure CLI toocreate egy Azure-adatbázis egy Azure-erőforráscsoportot a MySQL-kiszolgáló körülbelül öt perc múlva.</span><span class="sxs-lookup"><span data-stu-id="de5a7-104">This quickstart describes how toouse hello Azure CLI toocreate an Azure Database for MySQL server in an Azure resource group in about five minutes.</span></span> <span data-ttu-id="de5a7-105">hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="de5a7-105">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span>

<span data-ttu-id="de5a7-106">Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.</span><span class="sxs-lookup"><span data-stu-id="de5a7-106">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="de5a7-107">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="de5a7-107">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="de5a7-108">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="de5a7-108">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="de5a7-109">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="de5a7-109">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="de5a7-110">Ha több előfizetéssel rendelkezik, válassza ki a megfelelő előfizetés hello hello erőforrás létezik-e vagy a lesz számlázva.</span><span class="sxs-lookup"><span data-stu-id="de5a7-110">If you have multiple subscriptions, choose hello appropriate subscription in which hello resource exists or is billed for.</span></span> <span data-ttu-id="de5a7-111">Válasszon ki egy megadott előfizetés-azonosítót a fiókja alatt az [az account set](/cli/azure/account#set) parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="de5a7-111">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="de5a7-112">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="de5a7-112">Create a resource group</span></span>
<span data-ttu-id="de5a7-113">Hozzon létre egy [Azure erőforráscsoport](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) hello segítségével [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="de5a7-113">Create an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) using hello [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> <span data-ttu-id="de5a7-114">Az erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="de5a7-114">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span>

<span data-ttu-id="de5a7-115">hello alábbi példa létrehoz egy erőforráscsoportot `myresourcegroup` a hello `westus` helyét.</span><span class="sxs-lookup"><span data-stu-id="de5a7-115">hello following example creates a resource group named `myresourcegroup` in hello `westus` location.</span></span>

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="de5a7-116">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="de5a7-116">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="de5a7-117">Hozzon létre egy Azure-adatbázis MySQL kiszolgáló hello **az mysql kiszolgáló létrehozni** parancsot.</span><span class="sxs-lookup"><span data-stu-id="de5a7-117">Create an Azure Database for MySQL server with hello **az mysql server create** command.</span></span> <span data-ttu-id="de5a7-118">Egy kiszolgáló több adatbázist is tud kezelni.</span><span class="sxs-lookup"><span data-stu-id="de5a7-118">A server can manage multiple databases.</span></span> <span data-ttu-id="de5a7-119">Általában külön adatbázissal rendelkezik minden projekt vagy felhasználó.</span><span class="sxs-lookup"><span data-stu-id="de5a7-119">Typically, a separate database is used for each project or for each user.</span></span>

<span data-ttu-id="de5a7-120">hello alábbi példa adatbázist hoz létre Azure található MySQL kiszolgáló `westus` hello erőforráscsoportban `myresourcegroup` nevű `myserver4demo`.</span><span class="sxs-lookup"><span data-stu-id="de5a7-120">hello following example creates an Azure Database for MySQL server located in `westus` in hello resource group `myresourcegroup` with name `myserver4demo`.</span></span> <span data-ttu-id="de5a7-121">hello kiszolgáló rendelkezik egy rendszergazdanaplójában rögzíti a nevű `myadmin` és a jelszó `Password01!`.</span><span class="sxs-lookup"><span data-stu-id="de5a7-121">hello server has an administrator log in named `myadmin` and password `Password01!`.</span></span> <span data-ttu-id="de5a7-122">hello kiszolgáló jön létre a **alapvető** teljesítményszinttel és **50** hello kiszolgáló összes hello adatbázisának között megosztott egységek számítási.</span><span class="sxs-lookup"><span data-stu-id="de5a7-122">hello server is created with **Basic** performance tier and **50** compute units shared between all hello databases in hello server.</span></span> <span data-ttu-id="de5a7-123">Méretezheti számítási és tárolási felfelé vagy lefelé hello alkalmazás igényeinek.</span><span class="sxs-lookup"><span data-stu-id="de5a7-123">You can scale compute and storage up or down depending on hello application needs.</span></span>

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name myserver4demo --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a><span data-ttu-id="de5a7-124">Tűzfalszabály konfigurálása</span><span class="sxs-lookup"><span data-stu-id="de5a7-124">Configure firewall rule</span></span>
<span data-ttu-id="de5a7-125">A MySQL kiszolgálószintű tűzfalszabály hello használata Azure-adatbázis létrehozása **az mysql-tűzfalszabály létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="de5a7-125">Create an Azure Database for MySQL server-level firewall rule using hello **az mysql server firewall-rule create** command.</span></span> <span data-ttu-id="de5a7-126">Egy kiszolgálószintű tűzfalszabályt lehetővé teszi, hogy a külső alkalmazás, például a hello **mysql.exe** parancssori eszköz vagy a MySQL munkaterület tooconnect tooyour server hello Azure MySQL szolgáltatás tűzfalon keresztül.</span><span class="sxs-lookup"><span data-stu-id="de5a7-126">A server-level firewall rule allows an external application, such as hello **mysql.exe** command-line tool or MySQL Workbench tooconnect tooyour server through hello Azure MySQL service firewall.</span></span> 

<span data-ttu-id="de5a7-127">hello alábbi példa létrehoz egy előre meghatározott-címtartományt, amely ebben a példában hello teljes lehetséges az IP-címek egy tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="de5a7-127">hello following example creates a firewall rule for a predefined address range, which in this example is hello entire possible range of IP addresses.</span></span>

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server myserver4demo --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a><span data-ttu-id="de5a7-128">Az SSL-beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="de5a7-128">Configure SSL settings</span></span>
<span data-ttu-id="de5a7-129">Alapértelmezés szerint a kiszolgáló és az ügyfélalkalmazások közti SSL-kapcsolatok kényszerítve vannak.</span><span class="sxs-lookup"><span data-stu-id="de5a7-129">By default, SSL connections between your server and client applications are enforced.</span></span>  <span data-ttu-id="de5a7-130">Ez biztosítja, hogy "a-mozgási" biztonsági adatok keresztül hello adatfolyam titkosításával hello internet.</span><span class="sxs-lookup"><span data-stu-id="de5a7-130">This ensures security of "in-motion" data by encrypting hello data stream over hello internet.</span></span>  <span data-ttu-id="de5a7-131">a gyors üzembe helyezési könnyebb toomake, akkor nem a kiszolgáló SSL-kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="de5a7-131">toomake this quick start easier, we disable SSL connections for your server.</span></span>  <span data-ttu-id="de5a7-132">Ez éles kiszolgálók esetében nem javasolt.</span><span class="sxs-lookup"><span data-stu-id="de5a7-132">This is not recommended for production servers.</span></span>  <span data-ttu-id="de5a7-133">További információkért lásd: [konfigurálása az SSL-kapcsolat az alkalmazás toosecurely a MySQL adatbázis tooAzure kapcsolati](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="de5a7-133">For more information, see [Configure SSL connectivity in your application toosecurely connect tooAzure Database for MySQL](./howto-configure-ssl.md).</span></span>

<span data-ttu-id="de5a7-134">hello következő példa letiltja végrehajtó SSL a MySQL-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="de5a7-134">hello following example disables enforcing SSL on your MySQL server.</span></span>
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name myserver4demo -g -n --ssl-enforcement Disabled
 ```

## <a name="get-hello-connection-information"></a><span data-ttu-id="de5a7-135">Hello kapcsolatadatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="de5a7-135">Get hello connection information</span></span>

<span data-ttu-id="de5a7-136">tooconnect tooyour kiszolgáló, tooprovide állomás információkat és a hozzáférési hitelesítő adatok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="de5a7-136">tooconnect tooyour server, you need tooprovide host information and access credentials.</span></span>

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name myserver4demo
```

<span data-ttu-id="de5a7-137">hello eredménye JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="de5a7-137">hello result is in JSON format.</span></span> <span data-ttu-id="de5a7-138">Jegyezze fel a hello **fullyQualifiedDomainName** és **administratorLogin**.</span><span class="sxs-lookup"><span data-stu-id="de5a7-138">Make a note of hello **fullyQualifiedDomainName** and **administratorLogin**.</span></span>
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

## <a name="connect-toohello-server-using-hello-mysqlexe-command-line-tool"></a><span data-ttu-id="de5a7-139">Csatlakoztassa a kiszolgálót hello mysql.exe parancssori eszközzel toohello</span><span class="sxs-lookup"><span data-stu-id="de5a7-139">Connect toohello server using hello mysql.exe command-line tool</span></span>
<span data-ttu-id="de5a7-140">Csatlakoztassa a kiszolgálót hello segítségével tooyour **mysql.exe** parancssori eszközt.</span><span class="sxs-lookup"><span data-stu-id="de5a7-140">Connect tooyour server using hello **mysql.exe** command-line tool.</span></span> <span data-ttu-id="de5a7-141">A MySQL-t [innen](https://dev.mysql.com/downloads/) töltheti le és telepítheti számítógépén.</span><span class="sxs-lookup"><span data-stu-id="de5a7-141">You can download MySQL from [here](https://dev.mysql.com/downloads/) and install it on your computer.</span></span> <span data-ttu-id="de5a7-142">Ehelyett gombra hello **, próbálja** mintakódjainak megtekintése gombra, vagy hello `>_` hello jobb felső eszköztáron hello Azure-portálon, és az indítási hello gomb **Azure Cloud rendszerhéj**.</span><span class="sxs-lookup"><span data-stu-id="de5a7-142">Instead you may also click hello **Try It** button on code samples, or hello  `>_` button from hello upper right toolbar in hello Azure portal, and launch hello **Azure Cloud Shell**.</span></span>

<span data-ttu-id="de5a7-143">Írja be következő parancsokat hello:</span><span class="sxs-lookup"><span data-stu-id="de5a7-143">Type hello next commands:</span></span> 

1. <span data-ttu-id="de5a7-144">Csatlakozás toohello server használatával **mysql** parancssori eszköz:</span><span class="sxs-lookup"><span data-stu-id="de5a7-144">Connect toohello server using **mysql** command-line tool:</span></span>
```azurecli-interactive
 mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

2. <span data-ttu-id="de5a7-145">Kiszolgáló állapotának megtekintése:</span><span class="sxs-lookup"><span data-stu-id="de5a7-145">View server status:</span></span>
```sql
 mysql> status
```
<span data-ttu-id="de5a7-146">Ha minden megfelelően működik, hello parancssori eszköz a következő szöveg hello kell kimenetre:</span><span class="sxs-lookup"><span data-stu-id="de5a7-146">If everything goes well, hello command-line tool should output hello following text:</span></span>

```dos
C:\Users\>mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
Enter password: ***********
Welcome toohello MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' tooclear hello current input statement.

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
> <span data-ttu-id="de5a7-147">További parancsokért lásd: [az MySQL 5.7 referenciaútmutatójának 4.5.1-es fejezetét](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).</span><span class="sxs-lookup"><span data-stu-id="de5a7-147">For additional commands, see [MySQL 5.7 Reference Manual - Chapter 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).</span></span>

## <a name="connect-toohello-server-using-hello-mysql-workbench-gui-tool"></a><span data-ttu-id="de5a7-148">Csatlakoztassa a kiszolgálót hello MySQL munkaterület grafikus felhasználói Felülettel eszközzel toohello</span><span class="sxs-lookup"><span data-stu-id="de5a7-148">Connect toohello server using hello MySQL Workbench GUI tool</span></span>
1.  <span data-ttu-id="de5a7-149">Indítsa el a hello MySQL munkaterület alkalmazás az ügyfélszámítógépre.</span><span class="sxs-lookup"><span data-stu-id="de5a7-149">Launch hello MySQL Workbench application on your client computer.</span></span> <span data-ttu-id="de5a7-150">A MySQL Workbench-et [innen](https://dev.mysql.com/downloads/workbench/) töltheti le és telepítheti.</span><span class="sxs-lookup"><span data-stu-id="de5a7-150">You can download and install MySQL Workbench from [here](https://dev.mysql.com/downloads/workbench/).</span></span>

2.  <span data-ttu-id="de5a7-151">A hello **új kapcsolat beállítása** párbeszédpanelen adja meg következő információ hello **paraméterek** lapon:</span><span class="sxs-lookup"><span data-stu-id="de5a7-151">In hello **Setup New Connection** dialog box, enter hello following information on **Parameters** tab:</span></span>

   ![új kapcsolat beállítása](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

| <span data-ttu-id="de5a7-153">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="de5a7-153">**Setting**</span></span> | <span data-ttu-id="de5a7-154">**Ajánlott érték**</span><span class="sxs-lookup"><span data-stu-id="de5a7-154">**Suggested Value**</span></span> | <span data-ttu-id="de5a7-155">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="de5a7-155">**Description**</span></span> |
|---|---|---|
|   <span data-ttu-id="de5a7-156">Kapcsolat neve</span><span class="sxs-lookup"><span data-stu-id="de5a7-156">Connection Name</span></span> | <span data-ttu-id="de5a7-157">My Connection</span><span class="sxs-lookup"><span data-stu-id="de5a7-157">My Connection</span></span> | <span data-ttu-id="de5a7-158">Adjon meg egy címkét a kapcsolathoz (tetszőlegesen kiválasztható)</span><span class="sxs-lookup"><span data-stu-id="de5a7-158">Specify a label for this connection (this can be anything)</span></span> |
| <span data-ttu-id="de5a7-159">Kapcsolati módszer</span><span class="sxs-lookup"><span data-stu-id="de5a7-159">Connection Method</span></span> | <span data-ttu-id="de5a7-160">válassza a Standard (TCP/IP) lehetőséget</span><span class="sxs-lookup"><span data-stu-id="de5a7-160">choose Standard (TCP/IP)</span></span> | <span data-ttu-id="de5a7-161">TCP/IP protokoll tooconnect tooAzure adatbázis használata a MySQL ></span><span class="sxs-lookup"><span data-stu-id="de5a7-161">Use TCP/IP protocol tooconnect tooAzure Database for MySQL></span></span> |
| <span data-ttu-id="de5a7-162">Gazdanév</span><span class="sxs-lookup"><span data-stu-id="de5a7-162">Hostname</span></span> | <span data-ttu-id="de5a7-163">myserver4demo.mysql.database.azure.com</span><span class="sxs-lookup"><span data-stu-id="de5a7-163">myserver4demo.mysql.database.azure.com</span></span> | <span data-ttu-id="de5a7-164">A korábban feljegyzett kiszolgálónév.</span><span class="sxs-lookup"><span data-stu-id="de5a7-164">Server name you previously noted.</span></span> |
| <span data-ttu-id="de5a7-165">Port</span><span class="sxs-lookup"><span data-stu-id="de5a7-165">Port</span></span> | <span data-ttu-id="de5a7-166">3306</span><span class="sxs-lookup"><span data-stu-id="de5a7-166">3306</span></span> | <span data-ttu-id="de5a7-167">MySQL-hello alapértelmezett portját használja.</span><span class="sxs-lookup"><span data-stu-id="de5a7-167">hello default port for MySQL is used.</span></span> |
| <span data-ttu-id="de5a7-168">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="de5a7-168">Username</span></span> | myadmin@myserver4demo | <span data-ttu-id="de5a7-169">hello kiszolgáló-rendszergazdai bejelentkezés korábban feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="de5a7-169">hello server admin login you previously noted.</span></span> |
| <span data-ttu-id="de5a7-170">Jelszó</span><span class="sxs-lookup"><span data-stu-id="de5a7-170">Password</span></span> | **** | <span data-ttu-id="de5a7-171">Használja a korábban konfigurált hello rendszergazdai fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="de5a7-171">Use hello admin account password you configured earlier.</span></span> |

<span data-ttu-id="de5a7-172">Kattintson a **kapcsolat tesztelése** tootest, ha az összes paraméter megfelelően vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="de5a7-172">Click **Test Connection** tootest if all parameters are correctly configured.</span></span>
<span data-ttu-id="de5a7-173">Most, rákattinthat hello kapcsolat toosuccessfully toohello kiszolgálóhoz csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="de5a7-173">Now, you can click hello connection toosuccessfully connect toohello server.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="de5a7-174">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="de5a7-174">Clean up resources</span></span>
<span data-ttu-id="de5a7-175">Ha ezekkel az erőforrásokkal egy másik gyorsindítási/oktatóanyaghoz nincs szüksége, törölheti azokat hello parancs a következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="de5a7-175">If you don't need these resources for another quickstart/tutorial, you can delete them by doing hello following command:</span></span> 

```azurecli-interactive
az group delete --name myresourcegroup
```

## <a name="next-steps"></a><span data-ttu-id="de5a7-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="de5a7-176">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="de5a7-177">MySQL-adatbázis tervezése az Azure CLI-vel</span><span class="sxs-lookup"><span data-stu-id="de5a7-177">Design a MySQL Database with Azure CLI</span></span>](./tutorial-design-database-using-cli.md)
