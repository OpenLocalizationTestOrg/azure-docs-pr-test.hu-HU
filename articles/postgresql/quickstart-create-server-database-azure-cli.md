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
# <a name="create-an-azure-database-for-postgresql-using-hello-azure-cli"></a>Hozzon létre egy Azure-adatbázis PostgreSQL hello Azure parancssori felület használatával
Azure PostgreSQL-adatbázishoz egy felügyelt szolgáltatás, amely lehetővé teszi toorun, kezeléséhez, és magas rendelkezésre állású PostgreSQL-adatbázisok hello felhőben méretezése. hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez. A gyors üzembe helyezés bemutatja, hogyan toocreate egy Azure-adatbázis a PostgreSQL-kiszolgáló egy [Azure erőforráscsoport](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) Azure parancssori felület használatával hello.

Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

Ha több előfizetéssel rendelkezik, válassza ki azt a hello megfelelő előfizetés, amelyben hello erőforrás lesz terhelve. Válasszon ki egy megadott előfizetés-azonosítót a fiókja alatt az [az account set](/cli/azure/account#set) parancs segítségével.
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hello segítségével [az csoport létrehozása](/cli/azure/group#create) parancsot. Az erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat. hello alábbi példa létrehoz egy erőforráscsoportot `myresourcegroup` a hello `westus` helyét.
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a>Azure-adatbázis létrehozása PostgreSQL-kiszolgálóhoz

Hozzon létre egy [PostgreSQL-kiszolgáló Azure-adatbázis](overview.md) hello segítségével [az postgres kiszolgáló létrehozni](/cli/azure/postgres/server#create) parancsot. A kiszolgáló adatbázisok egy csoportját tartalmazza, amelyeket a rendszer egy csoportként kezel. 

hello alábbi példakód létrehozza a kiszolgáló nevű `mypgserver-20170401` az erőforráscsoportban `myresourcegroup` a kiszolgáló-rendszergazdai bejelentkezés `mylogin`. hello kiszolgáló nevét, amelyhez tooDNS neve képezi le, és ezért szükséges toobe globálisan egyedi az Azure-ban. Helyettesítő hello `<server_admin_password>` saját értékkel.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401  --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> hello kiszolgáló-rendszergazdai bejelentkezés és a jelszót, amely az itt megadott a szükséges toolog toohello Server és az adatbázisok a gyors üzembe helyezési későbbi részében. Jegyezze meg vagy jegyezze fel ezt az információt későbbi használatra.

Alapértelmezés szerint a **postgres** adatbázis a kiszolgáló alatt jön létre. Hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) egy alapértelmezett adatbázis való használatra szolgál, a felhasználók, segédprogramok és harmadik féltől származó alkalmazások. 


## <a name="configure-a-server-level-firewall-rule"></a>Kiszolgálószintű tűzfalszabály konfigurálása

Hozzon létre egy Azure PostgreSQL kiszolgálószintű tűzfalszabály hello [az postgres-tűzfalszabály létrehozása](/cli/azure/postgres/server/firewall-rule#create) parancsot. Egy kiszolgálószintű tűzfalszabályt lehetővé teszi a külső alkalmazás, például a [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) vagy [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server hello Azure PostgreSQL szolgáltatás tűzfalon keresztül. 

Beállíthat egy tűzfalszabályt, amely hozzá van rendelve egy IP tartomány toobe képes tooconnect a hálózatról. hello alábbi példában [az postgres-tűzfalszabály létrehozása](/cli/azure/postgres/server/firewall-rule#create) toocreate tűzfalszabály `AllowAllIps` egy IP-címtartományt. tooopen összes IP-cím, IP-cím és 255.255.255.255 indítása zárócíme hello hello 0.0.0.0 használják.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure PostgreSQL-kiszolgáló az 5432-es porton keresztül kommunikál. Ha vállalati hálózaton belülről próbál csatlakozni, elképzelhető, hogy a hálózati tűzfal nem engedélyezi a kimenő forgalmat az 5432-es porton keresztül. Rendelkezik az IT-részleg, nyissa meg a port 5432 tooconnect tooyour Azure SQL Database-kiszolgálóhoz.

## <a name="get-hello-connection-information"></a>Hello kapcsolatadatok beolvasása

tooconnect tooyour kiszolgáló, tooprovide állomás információkat és a hozzáférési hitelesítő adatok szükségesek.
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

hello eredménye JSON formátumban. Jegyezze fel a hello **administratorLogin** és **fullyQualifiedDomainName**.
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

## <a name="connect-toopostgresql-database-using-psql"></a>Csatlakozás tooPostgreSQL adatbázist psql használatával

Ha az ügyfélszámítógép PostgreSQL telepítve rendelkezik, egy helyi példányát használhatja [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooconnect tooan Azure PostgreSQL-kiszolgáló. Most már a hello psql parancssori segédprogram tooconnect toohello Azure PostgreSQL kiszolgáló.

1. Futtassa a következő psql parancs tooconnect tooan Azure adatbázis PostgreSQL-kiszolgáló hello
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Például a következő parancs hello csatlakozik toohello alapértelmezett adatbázis **postgres** a PostgreSQL kiszolgálón **mypgserver-20170401.postgres.database.azure.com** hozzáférési hitelesítő adatok használatával. Adja meg a hello `<server_admin_password>` úgy döntött, hogy amikor a jelszó megadását kéri.
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
```

2.  Ha csatlakoztatott toohello kiszolgáló, hozzon létre egy üres adatbázist hello parancssorba.
```sql
CREATE DATABASE mypgsqldb;
```

3.  Hello parancssorba hajtható végre a következő parancs tooswitch kapcsolat található, újonnan létrehozott toohello adatbázis hello **mypgsqldb**:
```sql
\c mypgsqldb
```

## <a name="connect-toopostgresql-database-using-pgadmin"></a>Csatlakozás tooPostgreSQL adatbázist pgAdmin használatával

tooconnect tooAzure PostgreSQL kiszolgáló grafikus felhasználói Felülettel hello eszközzel _pgAdmin_
1.  Indítsa el a hello _pgAdmin_ alkalmazás az ügyfélszámítógépre. A _pgAdmin-t_ http://www.pgadmin.org/ oldalról telepítheti.
2.  Válasszon **új kiszolgáló hozzáadása** a hello **Gyorshivatkozások** menü.
3.  A hello **- kiszolgáló létrehozása** párbeszédpanel **általános** lapra, adja meg a hello kiszolgáló egyedi rövid nevét. Például **Azure PostgreSQL Server**.
 ![pgAdmin tool - create - server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)
4.  A hello **- kiszolgáló létrehozása** párbeszédpanelen **kapcsolat** lapon:
    - Adja meg a hello teljes kiszolgálónév (például **mypgserver-20170401.postgres.database.azure.com**) hello a **állomásnév / cím** mezőbe. 
    - Meg kell adnia port 5432 hello **Port** mezőbe. 
    - Adja meg a hello **kiszolgáló-rendszergazdai bejelentkezés (user@mypgserver)** korábban beszerzett a gyors üzembe helyezés és hello server hello történő létrehozásakor a megadott jelszó **felhasználónév** és **jelszó** dobozban, illetve.
    - Az **SSL-mód** elemnél válassza a **Kötelező** lehetőséget. Alapértelmezés szerint a rendszer minden Azure PostgreSQL-kiszolgálót az SSL-kényszerítéssel bekapcsolva hoz létre. tooturn ki SSL kényszerítése, a részletek megtekintéséhez [kényszerítése SSL](./concepts-ssl-connection-security.md).

    ![pgAdmin - Create - Server](./media/quickstart-create-server-database-azure-cli/2-pgadmin-create-server.png)
5.  Kattintson a **Save** (Mentés) gombra.
6.  Hello böngésző bal oldali ablaktáblán bontsa ki a hello **kiszolgálócsoportok**. Válassza ki az **Azure PostgreSQL Server** kiszolgálót.
7.  Válassza ki a hello **Server** csatlakozik, és válassza a **adatbázisok** rajta. 
8.  Kattintson a jobb gombbal a **adatbázisok** tooCreate adatbázis.
9.  Válassza ki az adatbázis nevét **mypgsqldb** és hello tulajdonosa hozzá, a kiszolgáló-rendszergazdai bejelentkezés **mylogin**.
10. Kattintson a **mentése** toocreate egy üres adatbázist.
11. A hello **böngésző**, bontsa ki a hello **kiszolgálók** csoport. Bontsa ki a létrehozott hello kiszolgáló, és tekintse meg a hello adatbázis **mypgsqldb** rajta.
 ![pgAdmin - Create - Database](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)


## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Hello törlésével hello gyors üzembe helyezés létrehozott összes erőforrás karbantartása [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md).

> [!TIP]
> Az ebben a gyűjteményben lévő többi rövid útmutató erre a rövid útmutatóra épül. Ha toocontinue, a következő toowork quickstarts, így nem karbantartáshoz hello a gyors üzembe helyezés a létrejött erőforrásokat. Ha nem tervezi toocontinue, a következő lépéseket toodelete hello az összes erőforrást felhasználják hozta létre a gyors üzembe helyezés az Azure CLI hello.

```azurecli-interactive
az group delete --name myresourcegroup
```

Ha most szeretné toodelete hello egy újonnan létrehozott kiszolgálón, futtassa [az postgres kiszolgáló törlése](/cli/azure/postgres/server#delete) parancsot.
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mypgserver-20170401
```

## <a name="next-steps"></a>Következő lépések
> [!div class="nextstepaction"]
> [Adatbázis migrálása exportálással és importálással](./howto-migrate-using-export-and-import.md)
