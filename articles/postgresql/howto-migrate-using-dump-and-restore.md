---
title: "aaaHow tooDump és PostgreSQL az Azure-adatbázis visszaállítása |} Microsoft Docs"
description: "Ismerteti, hogyan adatbázis tooextract egy PostgreSQL-memóriakép fájlba és visszaállítási hello PostgreSQL-adatbázisból az Azure Database pg_dump hozta létre a PostgreSQL archív fájlból."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/14/2017
ms.openlocfilehash: 9ad28e9dec3927b0f80b5e6bab6423cc944f5156
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Telepítse át az PostgreSQL-adatbázist használ a biztonsági másolat és helyreállítás
Használhat [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract egy PostgreSQL-adatbázisból memóriaképfájl és [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) toorestore hello PostgreSQL-adatbázisból pg_dump által létrehozott archív fájlból.

## <a name="prerequisites"></a>Előfeltételek
Ez hogyan tooguide keresztül toostep, lesz szüksége:
- Egy [PostgreSQL-kiszolgáló Azure-adatbázis](quickstart-create-server-database-portal.md) szabályok tooallow tűzfalhozzáférés és az adatbázist.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) és [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) telepített parancssori segédprogramok

Kövesse az alábbi lépéseket toodump, és állítsa vissza a PostgreSQL-adatbázisban:

## <a name="create-a-dump-file-using-pgdump-that-contains-hello-data-toobe-loaded"></a>Hozzon létre egy biztonsági másolat fájlt hello adatok toobe betöltött tartalmazó pg_dump használatával
tooback be egy meglévő PostgreSQL a helyi adatbázis, vagy egy virtuális Gépet, futtassa a következő parancs hello:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Például, ha a helyi kiszolgáló és adatbázis **testdb** benne
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-hello-data-into-hello-target-azure-database-for-postrgesql-using-pgrestore"></a>Hello adatok visszaállítása hello TARGET Azure Database PostrgeSQL pg_restore használatával
Miután létrehozta a hello céladatbázis, hello pg_restore parancs és hello -d,--dbname paraméter toorestore hello adatok hello céladatbázis hello memóriakép fájlból is használhatja.
```bash
pg_restore -v –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Ebben a példában hello adatok helyreállítását a hello memóriakép **testdb.dump** hello adatbázisba **mypgsqldb** célkiszolgálón **mypgserver-20170401.postgres.database.azure.com**.
```bash
pg_restore -v --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Következő lépések
- toomigrate egy PostgreSQL-adatbázist az exportálás és importálás, lásd: [telepítse át az exportálási PostgreSQL-adatbázist, és importálása](howto-migrate-using-export-and-import.md)
