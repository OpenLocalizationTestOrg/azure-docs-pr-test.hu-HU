---
title: "Az első Azure-adatbázis kialakítása a PostgreSQL Azure-portál használatával |} Microsoft Docs"
description: "Ez az oktatóanyag bemutatja hogyan tooDesign az első Azure adatbázis a PostgreSQL hello Azure-portál használatával."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: tutorial, mvc
ms.topic: tutorial
ms.date: 05/10/2017
ms.openlocfilehash: fde7e9d1ae2bad4291d18bebd3356f4f8a2ac86a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-database-for-postgresql-using-hello-azure-portal"></a>Az első Azure-adatbázis kialakítása a PostgreSQL hello Azure-portál használatával

Azure PostgreSQL-adatbázishoz egy felügyelt szolgáltatás, amely lehetővé teszi toorun, kezeléséhez, és magas rendelkezésre állású PostgreSQL-adatbázisok hello felhőben méretezése. Hello Azure-portált használja, egyszerűen kezelheti a kiszolgálót, és egy adatbázis tervezése.

Ebben az oktatóanyagban használhatja az Azure portál toolearn hello hogyan számára:
> [!div class="checklist"]
> * Azure-adatbázis létrehozása PostgreSQL-hez
> * Hello kiszolgáló tűzfal konfigurálása
> * Használjon [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) segédprogram toocreate adatbázis
> * Mintaadatok betöltése
> * Adatok lekérdezése
> * Adatok frissítése
> * Adatok visszaállítása

## <a name="prerequisites"></a>Előfeltételek
Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.

## <a name="log-in-toohello-azure-portal"></a>Jelentkezzen be toohello Azure-portálon
Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

## <a name="create-an-azure-database-for-postgresql"></a>Azure-adatbázis létrehozása PostgreSQL-hez

Az Azure-adatbázis PostgreSQL-kiszolgálóhoz [számítási és tárolási erőforrások](./concepts-compute-unit-and-storage.md) egy meghatározott készletével együtt jön létre. hello server belül létrejön egy [Azure erőforráscsoport](../azure-resource-manager/resource-group-overview.md).

Kövesse ezeket a lépéseket toocreate egy PostgreSQL-kiszolgálóhoz tartozó Azure-adatbázis:
1.  Kattintson a hello **+ új** hello bal felső sarkában hello Azure-portálon található gombra.
2.  Válassza ki **adatbázisok** a hello **új** lapon, és válassza ki **PostgreSQL az Azure-adatbázis** a hello **adatbázisok** lap.
 ![Azure-adatbázishoz tartozó PostgreSQL - hello adatbázis létrehozása](./media/tutorial-design-database-using-azure-portal/1-create-database.png)

3.  Hello új kiszolgáló részletei űrlap kitöltése a következő információ, hello kép megelőző hello szerint:
    - Kiszolgáló neve: **mypgserver-20170401** (a kiszolgáló nevét leképezhető tooDNS nevét és így szükséges toobe globálisan egyedi) 
    - Előfizetés: Ha több előfizetéssel rendelkezik, válassza ki a megfelelő előfizetés hello hello erőforrás létezik-e vagy a lesz számlázva.
    - Erőforráscsoport: **myresourcegroup**
    - Az Ön által választott kiszolgálói rendszergazdai bejelentkezési név és jelszó
    - Hely
    - PostgreSQL-verzió

  > [!IMPORTANT]
  > hello kiszolgáló-rendszergazdai bejelentkezés és a jelszót, amely az itt megadott a szükséges toolog toohello Server és az adatbázisok a gyors üzembe helyezési későbbi részében. Jegyezze meg vagy jegyezze fel ezt az információt későbbi használatra.

4.  Kattintson a **tarifacsomag** toospecify hello és teljesítményszintet szolgáltatásszint az új adatbázishoz. Ehhez a rövid útmutatóhoz válassza az **Alapszintű** szolgáltatásszintet, **50 Számítási egységet**, és a szolgáltatási keretbe foglalt **50 GB** tárterületet.
 ![Azure-adatbázis PostgreSQL - kivételezési hello szolgáltatási rétegben](./media/tutorial-design-database-using-azure-portal/2-service-tier.png)
5.  Kattintson az **OK** gombra.
6.  Kattintson a **létrehozása** tooprovision hello kiszolgáló. Az üzembe helyezés eltarthat néhány percig.

  > [!TIP]
  > Ellenőrizze a hello **PIN-kód toodashboard** beállítás tooallow könnyű nyomon a központi telepítés.

7.  Hello eszköztáron kattintson **értesítések** toomonitor hello telepítési folyamat.
 ![Azure-adatbázis PostgreSQL-hez - Értesítések megtekintése](./media/tutorial-design-database-using-azure-portal/3-notifications.png)
   
  Alapértelmezés szerint a **postgres** adatbázis a kiszolgáló alatt jön létre. Hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) egy alapértelmezett adatbázis való használatra szolgál, a felhasználók, segédprogramok és harmadik féltől származó alkalmazások. 

## <a name="configure-a-server-level-firewall-rule"></a>Kiszolgálószintű tűzfalszabály konfigurálása

hello Azure adatbázis PostgreSQL szolgáltatás tűzfal hello kiszolgáló-szintjén hoz létre. Ez a tűzfal megakadályozza, hogy a külső alkalmazások és eszközök toohello kiszolgáló és a hello kiszolgálón lévő összes adatbázis csatlakozzon, kivéve, ha egy tűzfalszabály tooopen hello tűzfal adott IP-címekhez. 

1.  Hello központi telepítés befejezése után kattintson **összes erőforrás** hello bal oldali menüből és hello nevet írja be a **mypgserver-20170401** toosearch az újonnan létrehozott kiszolgáló. Kattintson a felsorolt hello keresési eredmény hello kiszolgálónevet. Hello **áttekintése** lapon, a kiszolgáló megnyílik, és további konfigurációs lehetőségeket.
 
 ![Azure-adatbázis PostgreSQL-hez - Kiszolgáló keresés ](./media/tutorial-design-database-using-azure-portal/4-locate.png)

2.  Hello server panelen válassza ki a **kapcsolatbiztonsági**. 
3.  Hello szövegmező alatt kattintson **szabály neve,** , és adja hozzá egy új tűzfal szabály toowhitelist hello IP-címtartománya kapcsolat. Ebben az oktatóanyagban most engedélyezése minden IP-címet írja be a **szabály neve = AllowAllIps**, **kezdő IP-= 0.0.0.0** és **záró IP-Címnél = 255.255.255.255** , majd **mentése** . Beállíthat egy tűzfalszabályt, amely hozzá van rendelve egy IP tartomány toobe képes tooconnect a hálózatról.
 
 ![Azure-adatbázis PostgreSQL-hez - Tűzfalszabály létrehozása](./media/tutorial-design-database-using-azure-portal/5-firewall-2.png)

4.  Kattintson a **mentése** majd hello **X** tooclose hello **kapcsolatok biztonsági** lap.

  > [!NOTE]
  > Azure PostgreSQL-kiszolgáló az 5432-es porton keresztül kommunikál. Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a port 5432 kimenő forgalmát. Ha igen, csak akkor tudja tooconnect tooyour Azure SQL adatbázis-kiszolgáló kivéve, ha az IT-részleg 5432 portot nyit meg.
  >


## <a name="get-hello-connection-information"></a>Hello kapcsolatadatok beolvasása

A PostgreSQL-kiszolgálóhoz tartozó Azure-adatbázis létrehozásakor hello alapértelmezett **postgres** adatbázis is végrehajtásakor létrejön. tooconnect tooyour adatbázis-kiszolgáló tooprovide állomás információkat és a hozzáférési hitelesítő adatok szükségesek.

1. A hello Azure-portálon a bal oldali menüből, kattintson az **összes erőforrás** , és keresse meg az imént létrehozott hello kiszolgáló **mypgserver-20170401**.

  ![Azure-adatbázis PostgreSQL-hez - Kiszolgáló keresés ](./media/tutorial-design-database-using-azure-portal/4-locate.png)

3. Hello kiszolgáló nevére kattint **mypgserver-20170401**.
4. Jelölje be hello server **áttekintése** lap. Jegyezze fel a hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név**.

 ![Azure-adatbázis PostgreSQL-hez - Kiszolgáló-rendszergazdai bejelentkezés](./media/tutorial-design-database-using-azure-portal/6-server-name.png)


## <a name="connect-toopostgresql-database-using-psql-in-cloud-shell"></a>Csatlakozás tooPostgreSQL adatbázis psql felhő rendszerhéj használatával

Most már a hello psql parancssori segédprogram tooconnect toohello Azure adatbázis PostgreSQL-kiszolgáló. 
1. Indítsa el a hello Azure Cloud rendszerhéj hello terminál ikonra a felső navigációs panelen hello keresztül.

   ![Azure-adatbázis PostgreSQL-hez - Azure Cloud Shell terminálikon](./media/tutorial-design-database-using-azure-portal/7-cloud-shell.png)

2. hello Azure Cloud rendszerhéj megnyitása a böngészőben, amely lehetővé teszi, tootype bash parancsok.

   ![Azure-adatbázis PostgreSQL-hez - Azure Shell Bash parancssor](./media/tutorial-design-database-using-azure-portal/8-bash.png)

3. A parancssorból hello felhő rendszerhéj tooyour Azure adatbázis hello psql parancsokkal PostgreSQL-kiszolgáló csatlakozzon. hello következő formátuma használt tooconnect tooan Azure adatbázis hello PostgreSQL kiszolgáló [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) segédprogram:
   ```bash
   psql --host=<myserver> --port=<port> --username=<server admin login> --dbname=<database name>
   ```

   Például a következő parancs hello csatlakozik toohello alapértelmezett adatbázis **postgres** a PostgreSQL kiszolgálón **mypgserver-20170401.postgres.database.azure.com** hozzáférési hitelesítő adatok használatával. Adja meg a kiszolgáló-rendszergazdai jelszavát, amikor a rendszer kéri.

   ```bash
   psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
   ```

## <a name="create-a-new-database"></a>Új adatbázis létrehozása
Ha már csatlakoztatott toohello kiszolgáló, egy üres adatbázis létrehozásához hello a parancssorból.
```bash
CREATE DATABASE mypgsqldb;
```

Hello parancssorba hajtható végre a következő parancs tooswitch kapcsolat található, újonnan létrehozott toohello adatbázis hello **mypgsqldb**.
```bash
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

Újonnan létrehozott tábla tabvles hello listája most írja be a hello látható:
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

## <a name="restore-data-tooa-previous-point-in-time"></a>Adatpont tooa előző időpont visszaállítása
Képzelje el ezt a táblázatot véletlenül törölt. Ebben a helyzetben valami nem egyszerűen állíthat helyre. Azure PostgreSQL-adatbázishoz lehetővé teszi (a legutóbbi too7 nap (alapszintű) és 35 napon (általános) hello) toogo hátsó tooany-időpontban, és állítsa vissza a időpontban tooa új kiszolgálóra. Az új kiszolgáló toorecover használhatja a törölt adatokat. hello lépéseket visszaállítási hello minta kiszolgáló tooa pont követően hello tábla hozzá lett adva.

1.  A hello Azure-adatbázis a kiszolgáló PostgreSQL-lap, kattintson **visszaállítása** hello eszköztáron. Hello **visszaállítása** lap megnyitásakor.
  ![Azure portál – visszaállítási űrlap beállítások](./media/tutorial-design-database-using-azure-portal/9-azure-portal-restore.png)
2.  Töltse ki a hello **visszaállítása** hello szükséges információt a képernyőn:

  ![Azure portál – visszaállítási űrlap beállítások](./media/tutorial-design-database-using-azure-portal/10-azure-portal-restore.png)
  - **Visszaállítási pont**: egy pont időponthoz kötött, amely hello server módosítása előtt válasszon
  - **Célkiszolgáló**: Adjon meg egy új kiszolgálónevet azt szeretné, hogy toorestore
  - **Hely**: hello régió nem választhat, alapértelmezés szerint ugyanaz, mint a forráskiszolgálón hello
  - **A tarifacsomag**: Ez az érték nem módosítható, ha egy kiszolgáló visszaállítása. Ugyanaz, mint a forráskiszolgálón hello. 
3.  Kattintson a **OK** toorestore hello server túl[tooa pont időponthoz kötött visszaállítás](./howto-restore-server-portal.md) előtt hello táblák törölve lett. A kiszolgáló tooa különböző pont visszaállítása időben ismétlődő új kiszolgáló létrehozása hello eredeti kiszolgálóként hello pont időben ad meg, feltéve, hogy a hello megőrzési időtartamon belül a [szolgáltatásréteg](./concepts-service-tiers.md).

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban megtanulta, hogyan toouse hello Azure-portál és az egyéb segédprogramokat, ha:
> [!div class="checklist"]
> * Azure-adatbázis létrehozása PostgreSQL-hez
> * Hello kiszolgáló tűzfal konfigurálása
> * Használjon [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) segédprogram toocreate adatbázis
> * Mintaadatok betöltése
> * Adatok lekérdezése
> * Adatok frissítése
> * Adatok visszaállítása

A következő megtudhatja, hogyan toouse Azure CLI toodo hasonló feladatokat, tekintse át az oktatóanyag: [az első Azure-adatbázis kialakítása a PostgreSQL Azure parancssori felület használatával](tutorial-design-database-using-azure-cli.md)
