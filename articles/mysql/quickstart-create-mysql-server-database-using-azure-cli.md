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
# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a>Azure-adatbázis létrehozása MySQL-kiszolgálóhoz az Azure CLI használatával
A gyors üzembe helyezés ismerteti, hogyan toouse hello Azure CLI toocreate egy Azure-adatbázis egy Azure-erőforráscsoportot a MySQL-kiszolgáló körülbelül öt perc múlva. hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez.

Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

Ha több előfizetéssel rendelkezik, válassza ki a megfelelő előfizetés hello hello erőforrás létezik-e vagy a lesz számlázva. Válasszon ki egy megadott előfizetés-azonosítót a fiókja alatt az [az account set](/cli/azure/account#set) parancs segítségével.
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot
Hozzon létre egy [Azure erőforráscsoport](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) hello segítségével [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) parancsot. Az erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat.

hello alábbi példa létrehoz egy erőforráscsoportot `myresourcegroup` a hello `westus` helyét.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Azure-adatbázis létrehozása MySQL-kiszolgálóhoz
Hozzon létre egy Azure-adatbázis MySQL kiszolgáló hello **az mysql kiszolgáló létrehozni** parancsot. Egy kiszolgáló több adatbázist is tud kezelni. Általában külön adatbázissal rendelkezik minden projekt vagy felhasználó.

hello alábbi példa adatbázist hoz létre Azure található MySQL kiszolgáló `westus` hello erőforráscsoportban `myresourcegroup` nevű `myserver4demo`. hello kiszolgáló rendelkezik egy rendszergazdanaplójában rögzíti a nevű `myadmin` és a jelszó `Password01!`. hello kiszolgáló jön létre a **alapvető** teljesítményszinttel és **50** hello kiszolgáló összes hello adatbázisának között megosztott egységek számítási. Méretezheti számítási és tárolási felfelé vagy lefelé hello alkalmazás igényeinek.

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name myserver4demo --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Tűzfalszabály konfigurálása
A MySQL kiszolgálószintű tűzfalszabály hello használata Azure-adatbázis létrehozása **az mysql-tűzfalszabály létrehozása** parancsot. Egy kiszolgálószintű tűzfalszabályt lehetővé teszi, hogy a külső alkalmazás, például a hello **mysql.exe** parancssori eszköz vagy a MySQL munkaterület tooconnect tooyour server hello Azure MySQL szolgáltatás tűzfalon keresztül. 

hello alábbi példa létrehoz egy előre meghatározott-címtartományt, amely ebben a példában hello teljes lehetséges az IP-címek egy tűzfalszabályt.

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server myserver4demo --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a>Az SSL-beállítások konfigurálása
Alapértelmezés szerint a kiszolgáló és az ügyfélalkalmazások közti SSL-kapcsolatok kényszerítve vannak.  Ez biztosítja, hogy "a-mozgási" biztonsági adatok keresztül hello adatfolyam titkosításával hello internet.  a gyors üzembe helyezési könnyebb toomake, akkor nem a kiszolgáló SSL-kapcsolatokat.  Ez éles kiszolgálók esetében nem javasolt.  További információkért lásd: [konfigurálása az SSL-kapcsolat az alkalmazás toosecurely a MySQL adatbázis tooAzure kapcsolati](./howto-configure-ssl.md).

hello következő példa letiltja végrehajtó SSL a MySQL-kiszolgálón.
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name myserver4demo -g -n --ssl-enforcement Disabled
 ```

## <a name="get-hello-connection-information"></a>Hello kapcsolatadatok beolvasása

tooconnect tooyour kiszolgáló, tooprovide állomás információkat és a hozzáférési hitelesítő adatok szükségesek.

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name myserver4demo
```

hello eredménye JSON formátumban. Jegyezze fel a hello **fullyQualifiedDomainName** és **administratorLogin**.
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

## <a name="connect-toohello-server-using-hello-mysqlexe-command-line-tool"></a>Csatlakoztassa a kiszolgálót hello mysql.exe parancssori eszközzel toohello
Csatlakoztassa a kiszolgálót hello segítségével tooyour **mysql.exe** parancssori eszközt. A MySQL-t [innen](https://dev.mysql.com/downloads/) töltheti le és telepítheti számítógépén. Ehelyett gombra hello **, próbálja** mintakódjainak megtekintése gombra, vagy hello `>_` hello jobb felső eszköztáron hello Azure-portálon, és az indítási hello gomb **Azure Cloud rendszerhéj**.

Írja be következő parancsokat hello: 

1. Csatlakozás toohello server használatával **mysql** parancssori eszköz:
```azurecli-interactive
 mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

2. Kiszolgáló állapotának megtekintése:
```sql
 mysql> status
```
Ha minden megfelelően működik, hello parancssori eszköz a következő szöveg hello kell kimenetre:

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
> További parancsokért lásd: [az MySQL 5.7 referenciaútmutatójának 4.5.1-es fejezetét](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

## <a name="connect-toohello-server-using-hello-mysql-workbench-gui-tool"></a>Csatlakoztassa a kiszolgálót hello MySQL munkaterület grafikus felhasználói Felülettel eszközzel toohello
1.  Indítsa el a hello MySQL munkaterület alkalmazás az ügyfélszámítógépre. A MySQL Workbench-et [innen](https://dev.mysql.com/downloads/workbench/) töltheti le és telepítheti.

2.  A hello **új kapcsolat beállítása** párbeszédpanelen adja meg következő információ hello **paraméterek** lapon:

   ![új kapcsolat beállítása](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

| **Beállítás** | **Ajánlott érték** | **Leírás** |
|---|---|---|
|   Kapcsolat neve | My Connection | Adjon meg egy címkét a kapcsolathoz (tetszőlegesen kiválasztható) |
| Kapcsolati módszer | válassza a Standard (TCP/IP) lehetőséget | TCP/IP protokoll tooconnect tooAzure adatbázis használata a MySQL > |
| Gazdanév | myserver4demo.mysql.database.azure.com | A korábban feljegyzett kiszolgálónév. |
| Port | 3306 | MySQL-hello alapértelmezett portját használja. |
| Felhasználónév | myadmin@myserver4demo | hello kiszolgáló-rendszergazdai bejelentkezés korábban feljegyzett. |
| Jelszó | **** | Használja a korábban konfigurált hello rendszergazdai fiók jelszavát. |

Kattintson a **kapcsolat tesztelése** tootest, ha az összes paraméter megfelelően vannak konfigurálva.
Most, rákattinthat hello kapcsolat toosuccessfully toohello kiszolgálóhoz csatlakozzon.

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása
Ha ezekkel az erőforrásokkal egy másik gyorsindítási/oktatóanyaghoz nincs szüksége, törölheti azokat hello parancs a következő tevékenységek végrehajtásával: 

```azurecli-interactive
az group delete --name myresourcegroup
```

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [MySQL-adatbázis tervezése az Azure CLI-vel](./tutorial-design-database-using-cli.md)
