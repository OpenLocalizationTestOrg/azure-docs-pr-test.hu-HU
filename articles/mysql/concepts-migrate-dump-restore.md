---
title: "a MySQL adatbázis használatával dump és MySQL az Azure-adatbázis visszaállítása aaaMigrate |} Microsoft Docs"
description: "Ez a cikk ismerteti a két közös módon tooback mentése és visszaállítása adatbázisokat az Azure-adatbázis a MySQL, például mysqldump MySQL munkaterület vagy PHPMyAdmin eszközökkel."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: d05d483ff53483df8e005eae2d9a4f8190e8f663
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-mysql-database-tooazure-database-for-mysql-using-dump-and-restore"></a>A MySQL-adatbázis tooAzure adatbázis áttelepítése a MySQL használata a biztonsági másolat és helyreállítás
Ez a cikk azt ismerteti, két gyakori módszer tooback be, és a MySQL-adatbázisokat az Azure-adatbázis visszaállítás
- Biztonsági másolat és a visszaállítási hello parancssori (mysqldump használatával) 
- Memóriakép és visszaállítás PHPMyAdmin segítségével 

## <a name="before-you-begin"></a>Előkészületek
Ez hogyan tooguide keresztül toostep, toohave kell:
- [Azure-adatbázis létrehozása a MySQL-kiszolgáló - Azure-portálon](quickstart-create-mysql-server-database-using-azure-portal.md)
- [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) parancssori segédprogram a gépre telepítve.
- MySQL-munkaterület [MySQL munkaterület letöltése](https://dev.mysql.com/downloads/workbench/), varangy, Navicat vagy más külső MySQL eszköz toodo, dump és visszaállításra vonatkozó parancsot kap.

## <a name="use-common-tools"></a>Általános eszközök használata
Közös segédprogramok és az eszközöket, például a MySQL-munkaterületet, mysqldump, varangy vagy Navicat tooremotely csatlakozzon, és állítsa vissza adatokat a MySQL az Azure-adatbázisba. Az ilyen eszközök használatát az internet kapcsolat tooconnect toohello Azure-adatbázis az ügyfélgépre MySQL. Ajánlott biztonsági eljárások az SSL-titkosítású kapcsolat használata, lásd a [MySQL az Azure-adatbázis konfigurálása az SSL-kapcsolat](concepts-ssl-connection-security.md). A MySQL adatbázis tooAzure áttelepítésekor nem kell toomove hello kiírt fájlok tooany különleges felhő helyét. 

## <a name="common-uses-for-dump-and-restore"></a>Gyakori használati mód a biztonsági másolat és helyreállítás
Néhány gyakori forgatókönyvek például a mysqldump és mysqlpump toodump és a betöltés adatbázisok segédprogramot MySQL az Azure-beli MySQL Database lehet használni. Más esetekben használhat hello [importálására és exportálására](concepts-migrate-import-export.md) közelítik meg helyette.

- Adatbázis használata a hello teljes adatbázis áttelepítésekor listázása. Ez a javaslat MySQL adatok nagy mennyiségű áthelyezésekor, vagy ha azt szeretné, toominimize szolgáltatáskiesést élő webhelyek vagy alkalmazások rendelkezik. 
-  Ellenőrizze, hogy minden táblájára hello adatbázis hello InnoDB tárolási összetevőt használja, ha adatok betöltése az Azure-adatbázisba a MySQL. Azure MySQL-adatbázis csak InnoDB tárolóprogramot támogatja, és ezért nem támogatja a másodlagos tárolási motorok. Ha a táblák konfigurált többi tárolási motor, formátumba alakítsa át azokat hello InnoDB motor áttelepítési tooAzure adatbázis előtt a MySQL.
   Például rendelkezik egy WordPress vagy webalkalmazás hello MyISAM táblák használata, ha először alakítsa át azokat a táblákat a MySQL tooAzure adatbázis visszaállítása előtt InnoDB formátumra alakítja át. Használjon hello záradék `ENGINE=InnoDB` tooset hello új tábla létrehozásakor használt motor, majd hello adatátvitelt hello kompatibilis táblába hello visszaállítása előtt. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- tooavoid bármely kompatibilitási problémák, MySQL ugyanazon verzióját hello forrás és cél rendszereken használatos, ha az adatbázisok való kiírása hello biztosítása. Például ha a meglévő MySQL-kiszolgáló verziója 5.7-es verzióját, majd át kell telepítenie tooAzure adatbázis konfigurált MySQL 5.7 toorun verziójához. Hello `mysql_upgrade` parancs egy Azure-adatbázis MySQL-kiszolgáló nem működik, és nem támogatott. Ha tooupgrade MySQL-verziók között kell, először dump, vagy a saját környezetében az MySQL egy újabb verziójával a alacsonyabb verziójú adatbázis exportálása. Ezután futtassa `mysql_upgrade`, MySQL az Azure-adatbázisba történő áttelepítésének megkezdése előtt.

## <a name="performance-considerations"></a>A teljesítménnyel kapcsolatos megfontolások
toooptimize teljesítmény érdekében ezeket a szempontokat, amikor nagy adatbázisok vonatkozó figyelmeztetés hajtsa végre a megfelelő:
-   Használjon hello `exclude-triggers` mysqldump, ha az adatbázisok vonatkozó beállítást. Eseményindítók kizárása kiírt fájlok tooavoid hello eseményindító parancsok kiváltó hello adatok visszaállítása során. 
-   Hello elkerülése `single-transaction` mysqldump, ha nagyon nagy adatbázisok vonatkozó beállítást. Sok tábla egy tranzakción belül vonatkozó hatására a megnövelt tárhely és memória-erőforrások toobe visszaállítás során felhasznált és a teljesítmény működés lelassulhat, vagy az erőforrás-korlátozások.
-   Használjon többértékű Beszúrások, amikor való kiírása adatbázisok, SQL toominimize utasítás végrehajtása terhelés mellett betöltése közben. Mysqldump segédprogram által létrehozott biztonsági másolat fájlok használatakor többértékű Beszúrások alapértelmezés szerint engedélyezve vannak. 
-  Használjon hello `order-by-primary` mysqldump való kiírása adatbázisok, így hello adatok parancsprogrammal létrehozva, elsődleges kulcs ahhoz, ha a beállítást.
-   Használjon hello `disable-keys` mysqldump, amikor adatokat, toodisable külső kulcsra vonatkozó megkötések betöltés előtt vonatkozó beállítást. Idegen kulcs ellenőrzések letiltása biztosít teljesítménynövekedést érhet el. Hello megkötések engedélyezéséhez és hello adatok ellenőrzése után hello terhelés tooensure a hivatkozási integritás.
-   Használja a particionált táblákat, amikor szükséges.
-   Az adatok párhuzamos betöltése. Ne használjon túl sok párhuzamos volna miatt a toohit erőforrás korlátozni, és figyelését hello mérőszámainkat hello Azure-portálon elérhető erőforrások. 
-   Használjon hello `defer-table-indexes` mysqlpump való kiírása adatbázisok, úgy, hogy az index létrehozása táblák adatok betöltése után történik, ha a beállítást.

## <a name="create-a-backup-file-from-hello-command-line-using-mysqldump"></a>A biztonsági másolat létrehozása, ha a hello parancssori mysqldump használatával
meglévő MySQL-adatbázisok hello helyi helyszíni kiszolgálón vagy egy virtuális gép, futtassa a következő parancs hello tooback: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

hello paraméterek tooprovide a következők:
- [uname] A felhasználónevet 
- az adatbázis [fázis] hello jelszó (Megjegyzés: a -p és hello jelszó között nincs hely) 
- az adatbázis [dbname] hello neve 
- az adatbázis biztonsági mentése [backupfile.sql] hello fájlnevét 
- [--opt] hello mysqldump beállítás 

Például egy adatbázis tooback nevű, "testdb" a MySQL kiszolgálón tesztfelhasználó "néven" hello felhasználónév és a nem jelszó tooa fájl testdb_backup.sql, használja a következő parancs hello. hello parancs készít biztonsági másolatot hello `testdb` nevű fájlba adatbázis `testdb_backup.sql`, tartalmazó összes hello SQL-utasítások szükséges toore-hello adatbázis létrehozása. 

```bash
$ mysqldump -u root -p testdb > testdb_backup.sql
```
tooselect tábláit fel az adatbázis tooback, lista hello táblanevek választják el egymástól. Például tooback tábláit csak table1 és table2 a hello "testdb", kövesse az ebben a példában: 
```bash
$ mysqldump -u root -p testdb table1 table2 > testdb_tables_backup.sql
```

egynél több adatbázis számára egyszerre, használjon hello--tooback adatbázis váltson, és szóközökkel elválasztott hello adatbázis nevét. 
```bash
$ mysqldump -u root -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql 
```
minden hello adatbázisok egy adott időpontban hello Server tooback, hello – minden-adatbázisok beállítást kell használnia.
```
$ mysqldump -u root -p --all-databases > alldb_backup.sql 
```

## <a name="create-a-database-on-hello-target-azure-database-for-mysql-server"></a>Hello cél Azure-adatbázis kiszolgáló MySQL-adatbázis létrehozása
Hozzon létre egy üres adatbázist hello cél Azure-adatbázis MySQL-kiszolgáló, ahová toomigrate hello adatokat. Segítségével például MySQL munkaterület, varangy vagy Navicat toocreate hello adatbázis. hello adatbázis rendelkezhet azonos nevet, amely kiírt tartalmazott hello adatok hello adatbázisként hello, vagy létrehozhat egy adatbázist egy másik nevet.

tooget csatlakoztatva, hello kapcsolatadatok hello tulajdonságai lapon keresse meg az Azure-adatbázis a MySQL.
![Hello kapcsolatadatok található hello Azure-portálon](./media/concepts-migrate-dump-restore/1_server-properties-name-login.png)

Adja hozzá a MySQL munkaterület hello kapcsolat adatait.
![MySQL-munkaterület kapcsolati karakterlánc](./media/concepts-migrate-dump-restore/2_setup-new-connection.png)


## <a name="restore-your-mysql-database-using-command-line-or-mysql-workbench"></a>Állítsa vissza a MySQL-adatbázis használatával parancssori vagy MySQL munkaterület
Miután létrehozta a hello céladatbázis, hello mysql parancs vagy MySQL munkaterület toorestore hello adatok adott hello újonnan létrehozott adatbázis hello memóriakép is használhatja.
```bash
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
Ebben a példában az újonnan létrehozott adatbázist hello cél Azure-adatbázis MySQL kiszolgáló hello hello adatok helyreállítását.
```bash
$ mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p testdb < testdb_backup.sql
```

## <a name="export-using-phpmyadmin"></a>Exportálás PHPMyAdmin használatával
tooexport, hello közös eszköz phpMyAdmin, amelyek esetleg már telepített helyileg a környezetben is használhatja. tooexport a MySQL-adatbázis PHPMyAdmin használatával:
- Nyissa meg a phpMyAdmin.
- Válassza ki az adatbázist. Kattintson a hello bal oldali listában hello hello adatbázis nevére. 
- Kattintson a hello **exportálása** hivatkozásra. Új oldal tooview hello memóriakép adatbázis jelenik meg.
- A hello exportálási területen, kattintson a hello **kijelölése az összes** hivatkozás toochoose hello táblák az adatbázisban. 
- A hello SQL beállítások területen kattintson a hello megfelelő beállításokkal. 
- Hello kattintson **mentése fájlként** beállítás és hello megfelelő tömörítési lehetőséget, és kattintson a hello **Ugrás** gombra. Figyelmeztetés, toosave hello fájlt helyben kell megjelenik egy párbeszédpanel.

## <a name="import-using-phpmyadmin"></a>Importálás PHPMyAdmin
Az adatbázis importálása hasonló tooexporting. Hello alábbi műveleteket:
- Nyissa meg a phpMyAdmin. 
- Hello phpMyAdmin telepítő oldalon kattintson **Hozzáadás** tooadd az Azure-adatbázis MySQL-kiszolgáló. Adja meg a hello kapcsolati adatokat és a bejelentkezési adatok.
- Hozzon létre egy megfelelő nevű, és válassza ki azt az üdvözlő képernyőt hello bal oldalon. toorewrite hello meglévő adatbázis, kattintson a hello adatbázis nevét, jelölje be az összes hello hello táblanevek mellett, majd válassza **Drop** toodelete hello meglévő táblákat. 
- Kattintson a hello **SQL** hivatkozás tooshow hello lap, ahol adja meg az SQL-parancsokat, vagy az SQL-fájl feltöltése. 
- Használjon hello **Tallózás** gomb toofind hello adatbázisfájlt. 
- Kattintson a hello **Ugrás** tooexport hello biztonsági mentés gombra, hello SQL-parancsok, és hozza létre újból az adatbázist.

## <a name="next-steps"></a>Következő lépések
[MySQL-adatbázis alkalmazások tooAzure kapcsolati](./howto-connection-string.md)
