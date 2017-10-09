---
title: "aaaImport és MySQL az Azure-adatbázis exportálása |} Microsoft Docs"
description: "Ez a cikk ismerteti közös módon tooimport és exportálási adatbázisokat az Azure Database MySQL, az eszközöket, például a MySQL munkaterület használatával."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: e2c036773d975df2eea2a59d166ea10567218a41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-mysql-database-by-using-import-and-export"></a>A MySQL-adatbázis áttelepítéséhez importálása és exportálása
Ez a cikk ismerteti a két gyakori módszer tooimporting és exportáló adatok tooan Azure adatbázis MySQL kiszolgáló MySQL munkaterület használatával. 

## <a name="before-you-begin"></a>Előkészületek
Ez hogyan tooguide keresztül toostep, lesz szüksége:
- Egy Azure-adatbázis MySQL-kiszolgáló következő [hozzon létre egy Azure-portál használatával MySQL-kiszolgálóhoz tartozó Azure-adatbázis](quickstart-create-mysql-server-database-using-azure-portal.md).
- MySQL-munkaterület [letöltött](https://dev.mysql.com/downloads/workbench/), vagy egy másik MySQL eszköz tooimport és exportálása.

## <a name="use-common-tools"></a>Általános eszközök használata
Közös eszközök például a MySQL-munkaterületet, varangy vagy Navicat tooremotely csatlakozás és importálhat, illetve exportálhatja az adatokat az Azure-adatbázisba a MySQL. 

Az ilyen eszközök használatát az Internet kapcsolat tooconnect tooAzure adatbázis az ügyfélgépre MySQL. Egy SSL titkosítású kapcsolat használata ajánlott biztonsági eljárások a [MySQL az Azure-adatbázis konfigurálása az SSL-kapcsolat](concepts-ssl-connection-security.md).

Nem kell toomove az importálás és exportálás a fájlok tooany különleges felhő helye, MySQL adatbázis tooAzure áttelepítésekor. 

## <a name="create-a-database-on-hello-azure-database-for-mysql-server"></a>Adatbázis létrehozására az Azure-adatbázis hello MySQL-kiszolgáló
Hozzon létre egy üres adatbázist hello Azure adatbázis MySQL-kiszolgáló, ahová toomigrate hello adatokat. Segítségével például MySQL munkaterület, varangy vagy Navicat toocreate hello adatbázis. hello adatbázis rendelkezhet hello azonos kiírt hello adatok, vagy tartalmazó hello adatbázist hozhat létre adatbázist egy eltérő nevű nevet.

tooget csatlakoztatva, keresse meg a hello kapcsolati információk hello **tulajdonságok** MySQL az Azure-adatbázis paneljén.

![Hello kapcsolatadatok található hello Azure-portálon](./media/concepts-migrate-import-export/1_server-properties-name-login.png)

Adja hozzá a hello kapcsolati információk tooMySQL munkaterületet.

![MySQL-munkaterület kapcsolati karakterlánc](./media/concepts-migrate-import-export/2_setup-new-connection.png)

## <a name="determine-when-toouse-import-and-export-techniques-instead-of-a-dump-and-restore"></a>Toouse importálása és exportálása technikák helyett egy biztonsági másolat és helyreállítás
MySQL eszközök tooimport használja, és az Azure-beli MySQL Database adatbázisok exportálása a következő forgatókönyvek hello. Más esetekben előfordulhat, hogy kihasználhatja a hello segítségével [dump és visszaállítási](concepts-migrate-dump-restore.md) közelítik meg helyette. 

- Ha tooselectively van szüksége az Azure-beli MySQL Database néhány táblák tooimport választhat meglévő MySQL-adatbázis, az ajánlott toouse hello importálása és exportálása technika.  Ezzel a módszerrel hagyja ki ezt a felesleges táblázatra hello toosave időt és erőforrásokat. Például, használja a hello `--include-tables` vagy `--exclude-tables` kapcsoló [mysqlpump](https://dev.mysql.com/doc/refman/5.7/en/mysqlpump.html#option_mysqlpump_include-tables) és hello `--tables` kapcsoló [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html#option_mysqldump_tables).
- Adatbázis-objektumok hello táblák nem telepít át, ha explicit módon hozza létre azokat. Megkötések (elsődleges kulcs, külső kulcs, indexek), nézetek, Funkciók, eljárások, eseményindítók tartalmazza, és bármely más adatbázis-objektumhoz, amelyet az toomigrate.
- A MySQL-adatbázis nem külső adatforrások, amelybe migrálna adatokat, amikor egybesimított fájlok létrehozása, és importálja a [mysqlimport](https://dev.mysql.com/doc/refman/5.7/en/mysqlimport.html).

Győződjön meg arról, hogy minden táblájára hello adatbázis felhasználhatja a hello InnoDB tárolóprogramot tölt be adatokat az Azure-adatbázisba a MySQL. Azure MySQL-adatbázis csak hello InnoDB tárolóprogramot, támogatja, így a másodlagos tárolási motorok nem támogatja. Ha a táblák alternatív tárolási motorok van szükség, lehet, hogy tooconvert őket toouse hello InnoDB motor formátum hello áttelepítési tooAzure MySQL adatbázis előtt. 

Például ha egy WordPress vagy webes alkalmazás hello MyISAM motort használja, először alakítsa át hello táblák hello adatok áttelepítése által InnoDB táblákba. Majd állítsa vissza a MySQL adatbázis tooAzure. Használjon hello záradék `ENGINE=INNODB` tooset hello motor a tábla létrehozása, és majd hello adatátvitelt hello kompatibilis táblába hello áttelepítés előtt. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```

## <a name="performance-recommendations-for-import-and-export"></a>Teljesítmény javaslatok importálása és exportálása
-   Adatok betöltése előtt hozzon létre a fürtözött indexek és az elsődleges kulcsok. Az elsődleges kulcs sorrendben adatok betöltése. 
-   Adatok után másodlagos indexek létrehozását késleltetés be van töltve. Minden másodlagos indexek létrehozása betöltése után. 
-   Tiltsa le a külső kulcsra vonatkozó megkötések betöltés előtt. Teljesítménynövekedéshez idegen kulcs ellenőrzések letiltása biztosít. Hello megkötések engedélyezéséhez és hello adatok ellenőrzése után hello terhelés tooensure a hivatkozási integritás.
-   Az adatok párhuzamos betöltése. Ne használjon túl sok párhuzamos, amely ehhez miatt a toohit erőforrás korlátozni, és figyelje az erőforrások hello metrikák hello Azure-portál érhető el. 
-   Használja a particionált táblákat, amikor szükséges.

## <a name="import-and-export-by-using-mysql-workbench"></a>Importálás és exportálás MySQL munkaterület használatával
Két módon tooexport-importálási adatok MySQL munkaterület. Minden más célt szolgál. 

### <a name="table-data-export-and-import-wizards-from-hello-object-browsers-context-menu"></a>Tábla adatai exportálása és importálása a varázslók hello objektum böngésző helyi menüből
![MySQL-munkaterület varázslók hello objektum böngésző helyi menüben](./media/concepts-migrate-import-export/p1.png)

a tábla adatai hello varázslók támogatott importálás, és műveletek exportálása CSV és a JSON-fájlok használatával. Több konfigurációs beállítások, például az szerepel, az oszlop kiválasztása és a kódolási kiválasztása tartalmaznak. Egyes varázslók helyi vagy távoli csatlakoztatott MySQL-kiszolgálók alapján végezheti el. hello importálási művelet tartalmaz a táblázatok, oszlopok és leképezésének. 

Kattintson a jobb gombbal egy táblát a varázslók hello objektum böngésző helyi menüből érheti el. Válasszon **adatok exportálása varázsló** vagy **tábla adatok importálása varázsló**. 

#### <a name="table-data-export-wizard"></a>Adatok exportálása varázsló
a következő példa kivitel hello tábla tooa CSV-fájl hello: 
1. Kattintson a jobb gombbal az exportált hello adatbázis toobe hello tábla. 
2. Válassza ki **adatok exportálása varázsló tábla**. Válassza ki a hello oszlopok toobe exportált, soreltolást (ha van ilyen) és count (ha van ilyen). 
3. A hello **jelölje ki az exportált adatokat** kattintson **következő**. Válassza ki a hello fájl elérési útját, CSV vagy JSON fájltípust. Kiválaszthatja a hello sor elválasztó, a befoglaló karakterláncok és mezőhatároló metódust. 
4. A hello **válassza kimeneti fájl helye** kattintson **következő**. 
5. A hello **adatok exportálása** kattintson **következő**.

#### <a name="table-data-import-wizard"></a>Adatok importálása varázsló
hello alábbi példa importálja hello tábla CSV-fájlból:
1. Kattintson a jobb gombbal hello adatbázis toobe importált hello táblájában. 
2. Tallózás tooand válassza hello importálása CSV-fájl toobe, és kattintson a **következő**. 
3. Válassza ki a céltábla hello (új vagy meglévő), és válassza ki, vagy törölje a jelet hello **importálás előtt Truncate table** jelölőnégyzetet. Kattintson a **Tovább** gombra.
4. Válassza ki a kódolás és hello oszlopok toobe importálva, és kattintson a **következő**. 
5. A hello **adatimportálás** kattintson **következő**. hello varázsló ennek megfelelően hello adatok importálására.

### <a name="sql-data-export-and-import-wizards-from-hello-navigator-pane"></a>SQL adatok exportálása, majd importálja a varázslók hello Navigator panelről
A varázsló tooexport használja, vagy importáljon MySQL munkaterület előállított vagy hello mysqldump parancs által létrehozott SQL. Ezekben a varázslókban elérje hello **Navigator** ablaktáblán vagy kiválasztásával **Server** hello főmenüből. Válassza ki **adatok exportálása** vagy **adatimportálás**. 

#### <a name="data-export"></a>Adatok exportálása
![MySQL-munkaterület adatok exportálása hello Navigator panelen](./media/concepts-migrate-import-export/p2.png)

Használhatja a hello **adatok exportálása** lapon tooexport MySQL adatait. 
1. Válassza ki, hogy szeretné, hogy tooexport, opcionálisan választja adott séma objektumok/táblák minden sémájából, és hello exportálási készítése minden sémát. Konfigurációs beállítások exportálása tooa projektmappa vagy önálló SQL-fájl tartalmazza, tárolt eljárások és események dump vagy hagyja ki a tábla adatai. 
 
   Másik megoldásként használhatja **exportálása eredményhalmaz** hello SQL szerkesztő tooanother formátumban, például CSV, JSON, HTML és XML beállítása az adott eredmény tooexport. 
3. Válassza ki a hello adatbázis objektumok tooexport, és konfigurálja hello kapcsolódó beállításokat.
4. Kattintson a **frissítése** tooload hello aktuális objektumok.
5. Szükség esetén nyissa meg a hello **speciális beállítások** toorefine hello az exportálási művelet fülre. Adja hozzá például a táblázat zárolásokat, használja a név felülírandó insert utasításokban, és ajánlat azonosítók backtick karakterekkel helyett.
6. Kattintson a **Start exportálása** toobegin hello exportálás során.


#### <a name="data-import"></a>Adatok importálása
![MySQL munkaterület adatokat importál felügyeleti Navigator használatával](./media/concepts-migrate-import-export/p3.png)

Használhatja a hello **adatimportálás** tooimport lapon, vagy állítsa vissza az exportált adatokat, hello adatok az exportálási művelet vagy hello mysqldump parancs. 
1. Válassza a projektmappa hello vagy önálló SQL-fájl, válassza ki a hello séma tooimport be, vagy válassza **új** toodefine új sémát. 
2. Kattintson a **Start importálása** toobegin hello az importálási folyamat.

## <a name="next-steps"></a>Következő lépések
Egy másik áttelepítési módszert használja, mint olvasási [áttelepítése a MySQL adatbázis használatával dump és MySQL az Azure-adatbázis visszaállítása](concepts-migrate-dump-restore.md). 
