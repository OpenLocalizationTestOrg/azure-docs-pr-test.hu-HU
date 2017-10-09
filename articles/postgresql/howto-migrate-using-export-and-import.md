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
# <a name="migrate-your-postgresql-database-using-export-and-import"></a><span data-ttu-id="77f83-103">Telepítse át az exportálási PostgreSQL-adatbázist, és importálása</span><span class="sxs-lookup"><span data-stu-id="77f83-103">Migrate your PostgreSQL database using export and import</span></span>
<span data-ttu-id="77f83-104">Használhat [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract egy PostgreSQL-adatbázisból egy parancsfájlt és [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooimport hello adatok hello céladatbázis a fájl.</span><span class="sxs-lookup"><span data-stu-id="77f83-104">You can use [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract a PostgreSQL database into a script file and [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooimport hello data into hello target database from that file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="77f83-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="77f83-105">Prerequisites</span></span>
<span data-ttu-id="77f83-106">Ez hogyan tooguide keresztül toostep, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="77f83-106">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="77f83-107">Egy [PostgreSQL-kiszolgáló Azure-adatbázis](quickstart-create-server-database-portal.md) szabályok tooallow tűzfalhozzáférés és az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="77f83-107">An [Azure Database for PostgreSQL server](quickstart-create-server-database-portal.md) with firewall rules tooallow access and database under it.</span></span>
- <span data-ttu-id="77f83-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) telepített parancssori segédprogram</span><span class="sxs-lookup"><span data-stu-id="77f83-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) command-line utility installed</span></span>
- <span data-ttu-id="77f83-109">[psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) telepített parancssori segédprogram</span><span class="sxs-lookup"><span data-stu-id="77f83-109">[psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) command-line utility installed</span></span>

<span data-ttu-id="77f83-110">Kövesse az alábbi lépéseket tooexport, és importálja a PostgreSQL-adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="77f83-110">Follow these steps tooexport and import your PostgreSQL database.</span></span>

## <a name="create-a-script-file-using-pgdump-that-contains-hello-data-toobe-loaded"></a><span data-ttu-id="77f83-111">Hello adatok toobe betöltött tartalmazó pg_dump használata parancsfájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="77f83-111">Create a script file using pg_dump that contains hello data toobe loaded</span></span>
<span data-ttu-id="77f83-112">tooexport a meglévő PostgreSQL-adatbázis a helyszíni vagy a virtuális gép tooa sql-parancsfájlt, futtassa a következő parancsot a meglévő környezetben hello:</span><span class="sxs-lookup"><span data-stu-id="77f83-112">tooexport your existing PostgreSQL database on-premises or in a VM tooa sql script file, run hello following command in your existing environment:</span></span>
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
<span data-ttu-id="77f83-113">Például, ha a helyi kiszolgáló és adatbázis **testdb** benne</span><span class="sxs-lookup"><span data-stu-id="77f83-113">For example, if you have a local server and a database called **testdb** in it</span></span>
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-hello-data-on-target-azure-database-for-postrgesql"></a><span data-ttu-id="77f83-114">A cél Azure-adatbázis a PostrgeSQL hello adatok importálása</span><span class="sxs-lookup"><span data-stu-id="77f83-114">Import hello data on target Azure Database for PostrgeSQL</span></span>
<span data-ttu-id="77f83-115">Hello psql parancssori és hello -d,--dbname paraméter tooimport hello adatok PostrgeSQL azt létrehozott és az adatok betöltése az sql-fájl hello Azure-adatbázisba is használhatja.</span><span class="sxs-lookup"><span data-stu-id="77f83-115">You can use hello psql command line and hello -d, --dbname parameter tooimport hello data into Azure Database for PostrgeSQL we created and load data from hello sql file.</span></span>
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
<span data-ttu-id="77f83-116">Ez a példa psql segédprogram és nevű parancsfájl **testdb.sql** az előző lépés tooimport adatok hello adatbázisba **mypgsqldb** célkiszolgálón  **mypgserver-20170401.postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="77f83-116">This example uses psql utility and a script file named **testdb.sql** from previous step tooimport data into hello database **mypgsqldb** on target server **mypgserver-20170401.postgres.database.azure.com**.</span></span>
```bash
psql --file=testdb.sql --host=mypgserver-20170401.database.windows.net --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb
```

## <a name="next-steps"></a><span data-ttu-id="77f83-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="77f83-117">Next steps</span></span>
- <span data-ttu-id="77f83-118">toomigrate egy PostgreSQL-adatbázist biztonsági másolat és helyreállítás, lásd: [telepítse át az PostgreSQL-adatbázist használ a biztonsági másolat és helyreállítás](howto-migrate-using-dump-and-restore.md)</span><span class="sxs-lookup"><span data-stu-id="77f83-118">toomigrate a PostgreSQL database using dump and restore, see [Migrate your PostgreSQL database using dump and restore](howto-migrate-using-dump-and-restore.md)</span></span>
