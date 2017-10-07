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
# <a name="design-your-first-azure-database-for-postgresql-using-azure-cli"></a>Az első Azure-adatbázis kialakítása a PostgreSQL Azure parancssori felület használatával 
Ebben az oktatóanyagban használhatja az Azure CLI (parancssori felület) és egyéb segédprogramok toolearn hogyan számára:
> [!div class="checklist"]
> * Azure-adatbázis létrehozása PostgreSQL-hez
> * Hello kiszolgáló tűzfal konfigurálása
> * Használjon [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) segédprogram toocreate adatbázis
> * Mintaadatok betöltése
> * Adatok lekérdezése
> * Adatok frissítése
> * Adatok visszaállítása

Azure Cloud rendszerhéj hello hello böngészőben használhatja vagy [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli) a saját számítógép toorun hello kódblokkok ebben az oktatóanyagban.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ebben a témakörben van szükség, hogy hello Azure CLI verzióját futtatja, 2.0-s vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

Ha több előfizetéssel rendelkezik, válassza ki a megfelelő előfizetés hello hello erőforrás létezik-e vagy a lesz számlázva. Válasszon ki egy megadott előfizetés-azonosítót a fiókja alatt az [az account set](/cli/azure/account#set) parancs segítségével.
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

hello alábbi példakód létrehozza nevű kiszolgáló `mypgserver-20170401` az erőforráscsoportban `myresourcegroup` a kiszolgáló-rendszergazdai bejelentkezés `mylogin`. A kiszolgáló nevét tooDNS neve képezi le, és ezért szükséges toobe globálisan egyedi az Azure-ban. Helyettesítő hello `<server_admin_password>` saját értékkel.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401 --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
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
>

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

## <a name="connect-tooazure-database-for-postgresql-database-using-psql"></a>Kapcsolódás adatbázis tooAzure psql használatával PostgreSQL-adatbázishoz
Ha az ügyfélszámítógép PostgreSQL telepítve rendelkezik, egy helyi példányát használhatja [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), vagy hello Azure Cloud Console tooconnect tooan Azure PostgreSQL-kiszolgáló. Most már a hello psql parancssori segédprogram tooconnect toohello Azure adatbázis PostgreSQL-kiszolgáló.

1. Futtassa a következő psql parancs tooconnect tooan Azure adatbázis PostgreSQL-kiszolgáló hello
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Például a következő parancs hello csatlakozik toohello alapértelmezett adatbázis **postgres** a PostgreSQL kiszolgálón **mypgserver-20170401.postgres.database.azure.com** hozzáférési hitelesítő adatok használatával. Adja meg a hello `<server_admin_password>` úgy döntött, hogy amikor a jelszó megadását kéri.
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 ---dbname=postgres
```

2.  Ha csatlakoztatott toohello kiszolgáló, hozzon létre egy üres adatbázist hello parancssorba.
```sql
CREATE DATABASE mypgsqldb;
```

3.  Hello parancssorba hajtható végre a következő parancs tooswitch kapcsolat található, újonnan létrehozott toohello adatbázis hello **mypgsqldb**:
```sql
\c mypgsqldb
```

## <a name="create-tables-in-hello-database"></a>Táblázatok létrehozása hello adatbázis
Most, hogy tudja, hogyan tooconnect toohello PostgreSQL az Azure-adatbázis, azt is ismerteti hogyan toocomplete néhány alapvető műveleteket.

Először hogy hozzon létre egy táblát, és töltse be adatokkal. Hozzon létre egy táblát, amely nyomon követi a leltáradatokat.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

Újonnan létrehozott tábla hello listáját táblákat most beírásával hello látható:
```sql
\dt
```

## <a name="load-data-into-hello-tables"></a>Adatok betöltése az hello táblák
Most, hogy a tábla, azt beilleszthet néhány adat azt. Hello nyissa meg a parancssort időszakban futtassa a következő lekérdezés tooinsert hello néhány sornyi adat
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Lehetősége van a korábban létrehozott hello táblába mintaadatok most két sorát.

## <a name="query-and-update-hello-data-in-hello-tables"></a>Hello táblázatok lekérdezési és frissítési hello adatait
Hajtható végre a következő lekérdezés tooretrieve információ hello adatbázis táblából hello. 
```sql
SELECT * FROM inventory;
```

Hello hello-táblázatok adatait is frissítheti
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

az adatok hello sor ennek megfelelően frissül.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a>Egy adatbázis tooa korábbi időpontra időbeli visszaállítása
Tegyük fel, hogy véletlenül törölt egy tábla. Ez a valami nem egyszerűen állíthat helyre. Azure PostgreSQL-adatbázishoz lehetővé teszi (a legutóbbi too7 nap (alapszintű) és 35 napon (általános) hello) toogo hátsó tooany-időpontban, és állítsa vissza a időpontban tooa új kiszolgálóra. Az új kiszolgáló toorecover használhatja a törölt adatokat. hello lépéseket visszaállítási hello minta kiszolgáló tooa pont követően hello tábla hozzá lett adva.

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

Hello `az postgres server restore` parancsot kell a következő paraméterek hello:
| Beállítás | Ajánlott érték | Leírás  |
| --- | --- | --- |
| --Erőforráscsoport |  myResourceGroup |  Az erőforráscsoport, mely hello forráskiszolgáló található.  |
| --neve | mypgserver visszaállítása | új kiszolgáló hello hello visszaállítási parancs által létrehozott hello neve. |
| visszaállítás--időpontban | 2017-04-13T13:59:00Z | Válassza ki a időpontban toorestore számára. A dátum és idő hello forráskiszolgáló biztonsági mentés megőrzési időn belül kell lennie. Használjon ISO8601 dátum és idő formátumban. Például használhatja a saját helyi időzóna, például a `2017-04-13T05:59:00-08:00`, vagy használjon UTC Zulu formátum `2017-04-13T13:59:00Z`. |
| --forrás-kiszolgáló | mypgserver-20170401 | hello nevét vagy hello forrás server toorestore az azonosítója. |

Egy kiszolgáló tooa-időpontban visszaállítása hoz létre a megadott időben frissítésétől hello pont hello eredeti kiszolgálóként másolása az új kiszolgáló. hello helyét, és vissza hello server tartozó díjszabási hello ugyanaz, mint a forráskiszolgálón hello történik.

hello parancs szinkron, és visszatér hello kiszolgáló helyreállítása után. Miután hello visszaállítás befejezését, keresse meg a hello létrehozott új kiszolgálót. Ellenőrizze a hello adatok helyreállt a várt módon.


## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban, megtudta, hogyan toouse Azure CLI (parancssori felület) és egyéb segédprogramok:
> [!div class="checklist"]
> * Azure-adatbázis létrehozása PostgreSQL-hez
> * Hello kiszolgáló tűzfal konfigurálása
> * Használjon [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) segédprogram toocreate adatbázis
> * Mintaadatok betöltése
> * Adatok lekérdezése
> * Adatok frissítése
> * Adatok visszaállítása

A következő megtudhatja, hogyan toouse hello Azure portál toodo hasonló feladatokat, tekintse át az oktatóanyag: [kialakítást PostgreSQL használatával hello Azure-portálon az első Azure-adatbázis](tutorial-design-database-using-azure-portal.md)
