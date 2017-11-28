---
title: "Telepítse át az importálási egy adatbázist és az Azure Database PostgreSQL exportálás |} Microsoft Docs"
description: "Ismerteti, hogyan bontsa ki a PostgreSQL-adatbázisból egy parancsfájl-fájlba, és az adatok importálása a céladatbázis, a fájl."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/14/2017
ms.openlocfilehash: 5e306d516d04789e4526bfd09bf99139b83573ba
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a><span data-ttu-id="b9bc8-103">Telepítse át az exportálási PostgreSQL-adatbázist, és importálása</span><span class="sxs-lookup"><span data-stu-id="b9bc8-103">Migrate your PostgreSQL database using export and import</span></span>
<span data-ttu-id="b9bc8-104">Használható [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) kibontásához PostgreSQL-adatbázisból egy parancsfájl fájlba és [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) az adatok importálása a céladatbázis, a fájl számára.</span><span class="sxs-lookup"><span data-stu-id="b9bc8-104">You can use [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) to extract a PostgreSQL database into a script file and [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) to import the data into the target database from that file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9bc8-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b9bc8-105">Prerequisites</span></span>
<span data-ttu-id="b9bc8-106">Ez az útmutató Útmutató lépéseit, az alábbiak szükségesek:</span><span class="sxs-lookup"><span data-stu-id="b9bc8-106">To step through this how-to guide, you need:</span></span>
- <span data-ttu-id="b9bc8-107">Egy [PostgreSQL-kiszolgáló Azure-adatbázis](quickstart-create-server-database-portal.md) a tűzfalszabályokat access és az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b9bc8-107">An [Azure Database for PostgreSQL server](quickstart-create-server-database-portal.md) with firewall rules to allow access and database under it.</span></span>
- <span data-ttu-id="b9bc8-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) telepített parancssori segédprogram</span><span class="sxs-lookup"><span data-stu-id="b9bc8-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) command-line utility installed</span></span>
- <span data-ttu-id="b9bc8-109">[psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) telepített parancssori segédprogram</span><span class="sxs-lookup"><span data-stu-id="b9bc8-109">[psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) command-line utility installed</span></span>

<span data-ttu-id="b9bc8-110">A lépések segítségével exportálja és importálja a PostgreSQL-adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="b9bc8-110">Follow these steps to export and import your PostgreSQL database.</span></span>

## <a name="create-a-script-file-using-pgdump-that-contains-the-data-to-be-loaded"></a><span data-ttu-id="b9bc8-111">Az adatok betöltését tartalmazó pg_dump használatával parancsfájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9bc8-111">Create a script file using pg_dump that contains the data to be loaded</span></span>
<span data-ttu-id="b9bc8-112">A meglévő PostgreSQL adatbázis helyszíni exportálhatók, és egy virtuális Gépet egy sql-parancsfájlt, futtassa a következő parancsot a meglévő környezetben:</span><span class="sxs-lookup"><span data-stu-id="b9bc8-112">To export your existing PostgreSQL database on-premises or in a VM to a sql script file, run the following command in your existing environment:</span></span>
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
<span data-ttu-id="b9bc8-113">Például, ha a helyi kiszolgáló és adatbázis **testdb** benne</span><span class="sxs-lookup"><span data-stu-id="b9bc8-113">For example, if you have a local server and a database called **testdb** in it</span></span>
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-the-data-on-target-azure-database-for-postrgesql"></a><span data-ttu-id="b9bc8-114">A cél Azure-adatbázis adatok importálása az PostrgeSQL</span><span class="sxs-lookup"><span data-stu-id="b9bc8-114">Import the data on target Azure Database for PostrgeSQL</span></span>
<span data-ttu-id="b9bc8-115">A psql parancssor és a -d, az adatok importálása az Azure-adatbázis számára létrehozott PostrgeSQL és az adatok betöltése az sql fájl--dbname paraméterrel is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b9bc8-115">You can use the psql command line and the -d, --dbname parameter to import the data into Azure Database for PostrgeSQL we created and load data from the sql file.</span></span>
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
<span data-ttu-id="b9bc8-116">Ez a példa psql segédprogram és nevű parancsfájl **testdb.sql** az előző lépésben, hogy adatokat importáljon belőlük az adatbázis **mypgsqldb** célkiszolgálón  **mypgserver-20170401.postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="b9bc8-116">This example uses psql utility and a script file named **testdb.sql** from previous step to import data into the database **mypgsqldb** on target server **mypgserver-20170401.postgres.database.azure.com**.</span></span>
```bash
psql --file=testdb.sql --host=mypgserver-20170401.database.windows.net --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb
```

## <a name="next-steps"></a><span data-ttu-id="b9bc8-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b9bc8-117">Next steps</span></span>
- <span data-ttu-id="b9bc8-118">Egy PostgreSQL-adatbázist biztonsági másolat és helyreállítás áttelepítéséhez lásd: [telepítse át az PostgreSQL-adatbázist használ a biztonsági másolat és helyreállítás](howto-migrate-using-dump-and-restore.md)</span><span class="sxs-lookup"><span data-stu-id="b9bc8-118">To migrate a PostgreSQL database using dump and restore, see [Migrate your PostgreSQL database using dump and restore](howto-migrate-using-dump-and-restore.md)</span></span>