---
title: "az első Azure adatbázis a MySQL-adatbázis - Azure Portal aaaDesign |} Microsoft Docs"
description: "Ez az oktatóanyag azt ismerteti, hogyan toocreate és kezelése az Azure-adatbázis MySQL-kiszolgáló és az adatbázis-Azure portál használatával."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/06/2017
ms.custom: mvc
ms.openlocfilehash: 06dd952acc5356b8cccaf36917df1ff8db4f7139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Az első Azure-adatbázis MySQL-adatbázis megtervezése
Azure MySQL-adatbázis egy felügyelt szolgáltatás, amely lehetővé teszi toorun, kezelése, és magas rendelkezésre állású MySQL-adatbázisok hello felhőben méretezni. Hello Azure-portált használja, egyszerűen kezelheti a kiszolgálót, és egy adatbázis tervezése.

Ebben az oktatóanyagban használhatja az Azure portál toolearn hello hogyan számára:

> [!div class="checklist"]
> * A MySQL az Azure-adatbázis létrehozása
> * Hello kiszolgáló tűzfal konfigurálása
> * Mysql parancssori eszköz toocreate adatbázis használata
> * Mintaadatok betöltése
> * Adatok lekérdezése
> * Adatok frissítése
> * Adatok visszaállítása

## <a name="sign-in-toohello-azure-portal"></a>Jelentkezzen be toohello Azure-portálon
Nyissa meg a kedvenc webböngészőt, és látogasson el a hello [Microsoft Azure-portálon](https://portal.azure.com/). Adja meg a hitelesítő adatok toosign toohello portálon. az alapértelmezett nézet hello a szolgáltatás irányítópultján.

## <a name="create-an-azure-database-for-mysql-server"></a>Azure-adatbázis létrehozása MySQL-kiszolgálóhoz
A MySQL-kiszolgálóhoz készült Azure-adatbázis [számítási és tárolási erőforrások](./concepts-compute-unit-and-storage.md) egy meghatározott készletével együtt jön létre. hello server belül létrejön egy [Azure erőforráscsoport](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).

1. Keresse meg a túl**adatbázisok** > **MySQL az Azure-adatbázis**. Ha nem találja a MySQL-kiszolgáló **adatbázisok** kategória, kattintson a **láthatja az összes** tooshow összes elérhető szolgáltatások adatbázisában. Is beírhat **MySQL az Azure-adatbázis** hello Keresés mezőbe tooquickly található hello szolgáltatást.
![2-1 Navigálás tooMySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)

2. Kattintson a **MySQL az Azure-adatbázis** csempére, majd **létrehozása**.

A példa kedvéért töltse ki a hello Azure adatbázis MySQL űrlap a következő információ hello:

| **Beállítás** | **Ajánlott érték** | **Mező leírása** |
|---|---|---|
| *Kiszolgálónév* | myserver4demo  | Kiszolgálónév a globálisan egyedi toobe tartalmaz. |
| *Előfizetés* | mysubscription | Jelölje ki az előfizetését a hello legördülő. |
| *Erőforráscsoport* | myResourceGroup | Hozzon létre erőforráscsoportot, vagy használjon egy meglévőt. |
| *Kiszolgálói rendszergazdai bejelentkezés* | myadmin | A telepítő a rendszergazdai fiók nevét. |
| *Jelszó* |  | Állítson be erős jelszót a rendszergazdafiókhoz. |
| *Jelszó megerősítése* |  | Erősítse meg a hello rendszergazdai fiók jelszavát. |
| *Hely* |  | Válasszon a rendelkezésre álló régiók közül. |
| *Verzió* | 5.7 | Válassza ki a hello legújabb verziót. |
| *Teljesítmény konfigurálása* | Alapszintű, 50 számítási egység, 50 GB  | Válassza ki a **Tarifacsomagot**, a **Számítási egységek** számát, a GB-ban mért **Tárterületet** , majd kattintson az **OK** gombra. |
| *PIN-kód tooDashboard* | Jelölőnégyzet | Ajánlott a ebben a mezőben toocheck így könnyen később a előfordulhat hello kiszolgáló |
Ezt követően kattintson a **Create** (Létrehozás) gombra. Egy-két percen egy új Azure-adatbázis MySQL-kiszolgáló fut. hello felhőben. Kattinthat **értesítések** hello eszköztár toomonitor hello telepítési folyamatának gombjára.

## <a name="configure-firewall"></a>Tűzfal konfigurálása
Az Azure-adatbázis MySQL adatbázis tűzfal védi meg. Alapértelmezés szerint minden kapcsolatok toohello server és hello adatbázisok hello-kiszolgálón belül utasítja el. Csatlakozás előtt tooAzure adatbázis a MySQL hello először, konfigurálja a hello tűzfal tooadd hello ügyfél gép nyilvános IP-címe (vagy IP-címtartomány).

1. Kattintson az újonnan létrehozott kiszolgáló, majd a **kapcsolatbiztonsági**.
   ![3-1 kapcsolatbiztonsági](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)
2. Is **hozzáadása a saját IP**, vagy itt a tűzfalszabályok konfigurálása. Ne feledje tooclick **mentése** hello szabályok létrehozása után.
Most toohello server mysql parancssori eszköz vagy MySQL munkaterület grafikus felhasználói felület használatával is elérheti.

> [!TIP]
> MySQL-kiszolgálóhoz tartozó Azure-adatbázis 3306 porton keresztül kommunikál. Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a port 3306 kimenő forgalmát. Ha igen, kivéve, ha az IT-részleg 3306 portot nyit meg tooAzure MySQL-kiszolgáló nem lehet csatlakoztatni.

## <a name="get-connection-information"></a>Kapcsolatadatok lekérése
Teljesen minősített Get hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név** hello Azure-portálon a MySQL-kiszolgáló az Azure-adatbázis. Hello teljesen minősített neve tooconnect tooyour kiszolgálók mysql parancssori eszközzel használja. 

1. A [Azure-portálon](https://portal.azure.com/), kattintson a **összes erőforrás** hello bal oldali menüből hello típusnév és keresse meg a MySQL-kiszolgálóhoz tartozó Azure-adatbázis. Válassza ki a hello kiszolgáló neve tooview hello adatait.

2. A hello beállítások elemcsoportban kattintson **tulajdonságok**. Jegyezze fel **kiszolgálónév** és **KISZOLGÁLÓI rendszergazda bejelentkezési név**. Hello Másolás gombra következő tooeach mező toocopy toohello vágólapra kattintva.
   ![4-2 kiszolgáló tulajdonságai](./media/tutorial-design-database-using-portal/4_2-server-properties.png)

Ebben a példában hello kiszolgáló neve, *myserver4demo.mysql.database.azure.com*, és a kiszolgáló-rendszergazdai bejelentkezés hello  *myadmin@myserver4demo* .

## <a name="connect-toohello-server-using-mysql"></a>Csatlakoztassa a kiszolgálót a mysql használata toohello
Használjon [mysql parancssori eszköz](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) tooestablish MySQL-kiszolgáló kapcsolati tooyour Azure-adatbázis. Hello mysql parancssori eszköz hello Azure Cloud rendszerhéj hello böngészőben, illetve a saját gép mysql-eszközök telepítve helyileg futtathatja. toolaunch hello Azure Cloud rendszerhéj, kattintson a hello `Try It` ebben a cikkben egy kódblokk gombra, vagy keresse fel a hello Azure-portálon, és kattintson a hello `>_` hello felső, jobb eszköztárban látható ikonra. 

Írja be a hello parancs tooconnect:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a>Hozzon létre egy üres adatbázist
Ha már csatlakoztatott toohello kiszolgáló, hozzon létre egy üres adatbázist toowork rendelkező.
```sql
CREATE DATABASE mysampledb;
```

Hello a parancssorból futtassa a következő parancs tooswitch kapcsolat található, újonnan létrehozott toothis adatbázis hello:
```sql
USE mysampledb;
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
Tegyük fel, egy fontos adatbázistábla véletlenül törölt, és az adatok nem állíthatók vissza hello könnyen. Azure MySQL-adatbázis lehetővé teszi toorestore hello server tooa pont időben hello adatbázis másolatának létrehozása az új kiszolgálóra. Az új kiszolgáló toorecover használhatja a törölt adatokat. hello lépéseket visszaállítási hello minta kiszolgáló tooa pont követően hello tábla hozzá lett adva.

1. Hello Azure-portálon keresse meg a MySQL az Azure-adatbázis. A hello **áttekintése** kattintson **visszaállítása** hello eszköztáron. hello visszaállítási lap nyílik meg.

   ![10-1 visszaállítási adatbázis](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. Töltse ki a hello **visszaállítása** hello szükséges információt a képernyőn.
   
   ![10-2 visszaállítási képernyő](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - **Visszaállítási pont**: válassza ki a-időpontban, amelyet az toorestore, felsorolt hello időkereten belül. Győződjön meg arról, hogy tooconvert a helyi időzónára tooUTC.
   - **Toonew-kiszolgáló helyreállítása**: Adjon meg egy új azt szeretné, hogy toorestore kiszolgálónevet.
   - **Hely**: hello a régióban legyen, mint a forráskiszolgálón hello, és nem módosítható.
   - **IP-címek**: tarifacsomag hello hello hello azonos a forrás kiszolgálón, és nem módosítható.
   
3. Kattintson a **OK** toorestore hello server túl[visszaállítási tooa pont időben](./howto-restore-server-portal.md) előtt hello tábla törölve lett. A kiszolgáló visszaállítása másolatot hoz létre új hello kiszolgáló frissítésétől hello pont időpontban. 

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban használhatja az Azure portál toolearned hello hogyan számára:

> [!div class="checklist"]
> * A MySQL az Azure-adatbázis létrehozása
> * Hello kiszolgáló tűzfal konfigurálása
> * Mysql parancssori eszköz toocreate adatbázis használata
> * Mintaadatok betöltése
> * Adatok lekérdezése
> * Adatok frissítése
> * Adatok visszaállítása

> [!div class="nextstepaction"]
> [Hogyan tooconnect alkalmazások tooAzure MySQL adatbázist](./howto-connection-string.md)
