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
# <a name="design-your-first-azure-database-for-mysql-database"></a>Az első Azure-adatbázis MySQL-adatbázis megtervezése

A MySQL az Azure adatbázisa a hello MySQL Community Edition adatbázis-kezelő a Microsoft felhőalapú relációs adatbázis-szolgáltatás. Ebben az oktatóanyagban használhatja az Azure CLI (parancssori felület) és egyéb segédprogramok toolearn hogyan számára:

> [!div class="checklist"]
> * A MySQL az Azure-adatbázis létrehozása
> * Hello kiszolgáló tűzfal konfigurálása
> * Használjon [mysql parancssori eszköz](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate adatbázis
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
Hozzon létre egy [Azure erőforráscsoport](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) rendelkező [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) parancsot. Az erőforráscsoport olyan logikai tároló, amelyben a rendszer üzembe helyezi és csoportként kezeli az Azure-erőforrásokat.

hello alábbi példa létrehoz egy erőforráscsoportot `mycliresource` a hello `westus` helyét.

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Azure-adatbázis létrehozása MySQL-kiszolgálóhoz
Azure-adatbázis létrehozása hello az mysql kiszolgálóval MySQL kiszolgálóra létre parancsot. Egy kiszolgáló több adatbázist is tud kezelni. Általában külön adatbázissal rendelkezik minden projekt vagy felhasználó.

hello alábbi példa adatbázist hoz létre Azure található MySQL kiszolgáló `westus` hello erőforráscsoportban `mycliresource` nevű `mycliserver`. hello kiszolgáló rendelkezik egy rendszergazdanaplójában rögzíti a nevű `myadmin` és a jelszó `Password01!`. hello kiszolgáló jön létre a **alapvető** teljesítményszinttel és **50** hello kiszolgáló összes hello adatbázisának között megosztott egységek számítási. Méretezheti számítási és tárolási felfelé vagy lefelé hello alkalmazás igényeinek.

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver
--location westus --user myadmin --password Password01!
--performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Tűzfalszabály konfigurálása
Azure-adatbázis létrehozása a MySQL kiszolgálószintű tűzfalszabályt a hello az mysql-tűzfalszabály létrehozása parancs. Egy kiszolgálószintű tűzfalszabályt lehetővé teszi a külső alkalmazás, például a **mysql** parancssori eszköz vagy a MySQL munkaterület tooconnect tooyour server hello Azure MySQL szolgáltatás tűzfalon keresztül. 

hello alábbi példa létrehoz egy előre meghatározott címtartományba egy tűzfalszabályt. Ez a példa bemutatja a hello teljes lehetséges az IP-címeket.

```azurecli-interactive
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="get-hello-connection-information"></a>Hello kapcsolatadatok beolvasása

tooconnect tooyour kiszolgáló, tooprovide állomás információkat és a hozzáférési hitelesítő adatok szükségesek.
```azurecli-interactive
az mysql server show --resource-group mycliresource --name mycliserver
```

hello eredménye JSON formátumban. Jegyezze fel a hello **fullyQualifiedDomainName** és **administratorLogin**.
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

## <a name="connect-toohello-server-using-mysql"></a>Csatlakoztassa a kiszolgálót a mysql használata toohello
Használjon [mysql parancssori eszköz](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) tooestablish MySQL-kiszolgáló kapcsolati tooyour Azure-adatbázis. Ebben a példában a rendszer hello paranccsal:
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a>Hozzon létre egy üres adatbázist
Ha már csatlakoztatott toohello kiszolgáló, egy üres adatbázis létrehozásához.
```sql
mysql> CREATE DATABASE mysampledb;
```

Hello a parancssorból futtassa a következő parancs tooswitch hello kapcsolat található, újonnan létrehozott toothis adatbázis hello:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-hello-database"></a>Táblázatok létrehozása hello adatbázis
Most, hogy tudja, hogyan tooconnect toohello Azure adatbázis MySQL-adatbázis, azt is ismerteti hogyan toocomplete néhány alapvető műveleteket.

Először hogy hozzon létre egy táblát, és töltse be adatokkal. Hozzon létre egy tábla tárolja a leltáradatokat.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-hello-tables"></a>Adatok betöltése az hello táblák
Most, hogy a tábla, azt beilleszthet néhány adat azt. Hello nyissa meg a parancssort időszakban futtassa a következő lekérdezés tooinsert hello néhány sornyi adatot.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Most már rendelkezik a korábban létrehozott hello táblába mintaadatok két sorát.

## <a name="query-and-update-hello-data-in-hello-tables"></a>Hello táblázatok lekérdezési és frissítési hello adatait
Hajtható végre a következő lekérdezés tooretrieve információ hello adatbázis táblából hello.
```sql
SELECT * FROM inventory;
```

Hello hello-táblázatok adatait is frissítheti.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

az adatok hello sor ennek megfelelően frissül.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a>Egy adatbázis tooa korábbi időpontra időbeli visszaállítása
Képzelje el ezt a táblázatot véletlenül törölt. Ez a valami nem egyszerűen állíthat helyre. Azure MySQL-adatbázis lehetővé teszi toogo hátsó tooany pont hello időpont utolsó too35 nap mentése és visszaállítása ezen a pontján tooa új kiszolgálót. Az új kiszolgáló toorecover használhatja a törölt adatokat. hello lépéseket visszaállítási hello minta kiszolgáló tooa pont követően hello tábla hozzá lett adva.

Hello visszaállítási szükséges információkat a következő hello:

- Visszaállítási pont: Válasszon egy pont időponthoz kötött, amely hello server módosítása előtt következik be. Nagyobb, mint vagy egyenlő toohello source adatbázis legrégebbi biztonsági mentési értékével.
- Célkiszolgáló: Adjon meg egy új kiszolgálónevet azt szeretné, hogy toorestore
- Forráskiszolgáló: hello nevezze azt szeretné, hogy a toorestore hello kiszolgáló
- Hely: Nem választhat ki hello régió, alapértelmezés szerint ugyanaz, mint a forráskiszolgálón hello

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

toorestore hello kiszolgáló és [tooa pont időponthoz kötött visszaállítás](./howto-restore-server-portal.md) előtt hello tábla törölve lett. A kiszolgáló tooa különböző pont visszaállítása időben ismétlődő új kiszolgáló létrehozása hello eredeti kiszolgálóként hello pont időben ad meg, feltéve, hogy a hello megőrzési időtartamon belül a [szolgáltatásréteg](./concepts-service-tiers.md).

## <a name="next-steps"></a>Következő lépések
Ez az oktatóanyag megtanulta, hogy:
> [!div class="checklist"]
> * A MySQL az Azure-adatbázis létrehozása
> * Hello kiszolgáló tűzfal konfigurálása
> * Használjon [mysql parancssori eszköz](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate adatbázis
> * Mintaadatok betöltése
> * Adatok lekérdezése
> * Adatok frissítése
> * Adatok visszaállítása

> [!div class="nextstepaction"]
> [Azure-adatbázis a MySQL - Azure CLI-minták](./sample-scripts-azure-cli.md)
