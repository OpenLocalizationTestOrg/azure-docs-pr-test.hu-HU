---
title: "aaaMigrate PostgreSQL az Azure-adatbázis exportálására és importálásra egy adatbázis segítségével |} Microsoft Docs"
description: "Ismerteti, hogyan bontsa ki a PostgreSQL-adatbázisból egy parancsfájl-fájlba, és hello adatok importálása hello céladatbázis a fájl."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/14/2017
ms.openlocfilehash: 5ca4650782b02e1067c5a8a3710f2dfbc8c76063
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>Telepítse át az exportálási PostgreSQL-adatbázist, és importálása
Használhat [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract egy PostgreSQL-adatbázisból egy parancsfájlt és [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooimport hello adatok hello céladatbázis a fájl.

## <a name="prerequisites"></a>Előfeltételek
Ez hogyan tooguide keresztül toostep, lesz szüksége:
- Egy [PostgreSQL-kiszolgáló Azure-adatbázis](quickstart-create-server-database-portal.md) szabályok tooallow tűzfalhozzáférés és az adatbázist.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) telepített parancssori segédprogram
- [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) telepített parancssori segédprogram

Kövesse az alábbi lépéseket tooexport, és importálja a PostgreSQL-adatbázisból.

## <a name="create-a-script-file-using-pgdump-that-contains-hello-data-toobe-loaded"></a>Hello adatok toobe betöltött tartalmazó pg_dump használata parancsfájl létrehozása
tooexport a meglévő PostgreSQL-adatbázis a helyszíni vagy a virtuális gép tooa sql-parancsfájlt, futtassa a következő parancsot a meglévő környezetben hello:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Például, ha a helyi kiszolgáló és adatbázis **testdb** benne
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-hello-data-on-target-azure-database-for-postrgesql"></a>A cél Azure-adatbázis a PostrgeSQL hello adatok importálása
Hello psql parancssori és hello -d,--dbname paraméter tooimport hello adatok PostrgeSQL azt létrehozott és az adatok betöltése az sql-fájl hello Azure-adatbázisba is használhatja.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
Ez a példa psql segédprogram és nevű parancsfájl **testdb.sql** az előző lépés tooimport adatok hello adatbázisba **mypgsqldb** célkiszolgálón  **mypgserver-20170401.postgres.database.azure.com**.
```bash
psql --file=testdb.sql --host=mypgserver-20170401.database.windows.net --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb
```

## <a name="next-steps"></a>Következő lépések
- toomigrate egy PostgreSQL-adatbázist biztonsági másolat és helyreállítás, lásd: [telepítse át az PostgreSQL-adatbázist használ a biztonsági másolat és helyreállítás](howto-migrate-using-dump-and-restore.md)
