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
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a><span data-ttu-id="a232b-103">Telepítse át az PostgreSQL-adatbázist használ a biztonsági másolat és helyreállítás</span><span class="sxs-lookup"><span data-stu-id="a232b-103">Migrate your PostgreSQL database using dump and restore</span></span>
<span data-ttu-id="a232b-104">Használhat [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract egy PostgreSQL-adatbázisból memóriaképfájl és [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) toorestore hello PostgreSQL-adatbázisból pg_dump által létrehozott archív fájlból.</span><span class="sxs-lookup"><span data-stu-id="a232b-104">You can use [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract a PostgreSQL database into a dump file and [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) toorestore hello PostgreSQL database from an archive file created by pg_dump.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a232b-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a232b-105">Prerequisites</span></span>
<span data-ttu-id="a232b-106">Ez hogyan tooguide keresztül toostep, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="a232b-106">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="a232b-107">Egy [PostgreSQL-kiszolgáló Azure-adatbázis](quickstart-create-server-database-portal.md) szabályok tooallow tűzfalhozzáférés és az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="a232b-107">An [Azure Database for PostgreSQL server](quickstart-create-server-database-portal.md) with firewall rules tooallow access and database under it.</span></span>
- <span data-ttu-id="a232b-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) és [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) telepített parancssori segédprogramok</span><span class="sxs-lookup"><span data-stu-id="a232b-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) and [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) command-line utilities installed</span></span>

<span data-ttu-id="a232b-109">Kövesse az alábbi lépéseket toodump, és állítsa vissza a PostgreSQL-adatbázisban:</span><span class="sxs-lookup"><span data-stu-id="a232b-109">Follow these steps toodump and restore your PostgreSQL database:</span></span>

## <a name="create-a-dump-file-using-pgdump-that-contains-hello-data-toobe-loaded"></a><span data-ttu-id="a232b-110">Hozzon létre egy biztonsági másolat fájlt hello adatok toobe betöltött tartalmazó pg_dump használatával</span><span class="sxs-lookup"><span data-stu-id="a232b-110">Create a dump file using pg_dump that contains hello data toobe loaded</span></span>
<span data-ttu-id="a232b-111">tooback be egy meglévő PostgreSQL a helyi adatbázis, vagy egy virtuális Gépet, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="a232b-111">tooback up an existing PostgreSQL database on-premises or in a VM, run hello following command:</span></span>
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
<span data-ttu-id="a232b-112">Például, ha a helyi kiszolgáló és adatbázis **testdb** benne</span><span class="sxs-lookup"><span data-stu-id="a232b-112">For example, if you have a local server and a database called **testdb** in it</span></span>
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-hello-data-into-hello-target-azure-database-for-postrgesql-using-pgrestore"></a><span data-ttu-id="a232b-113">Hello adatok visszaállítása hello TARGET Azure Database PostrgeSQL pg_restore használatával</span><span class="sxs-lookup"><span data-stu-id="a232b-113">Restore hello data into hello target Azure Database for PostrgeSQL using pg_restore</span></span>
<span data-ttu-id="a232b-114">Miután létrehozta a hello céladatbázis, hello pg_restore parancs és hello -d,--dbname paraméter toorestore hello adatok hello céladatbázis hello memóriakép fájlból is használhatja.</span><span class="sxs-lookup"><span data-stu-id="a232b-114">Once you have created hello target database, you can use hello pg_restore command and hello -d, --dbname parameter toorestore hello data into hello target database from hello dump file.</span></span>
```bash
pg_restore -v –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
<span data-ttu-id="a232b-115">Ebben a példában hello adatok helyreállítását a hello memóriakép **testdb.dump** hello adatbázisba **mypgsqldb** célkiszolgálón **mypgserver-20170401.postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="a232b-115">In this example, restore hello data from hello dump file **testdb.dump** into hello database **mypgsqldb** on target server **mypgserver-20170401.postgres.database.azure.com**.</span></span>
```bash
pg_restore -v --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a><span data-ttu-id="a232b-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a232b-116">Next steps</span></span>
- <span data-ttu-id="a232b-117">toomigrate egy PostgreSQL-adatbázist az exportálás és importálás, lásd: [telepítse át az exportálási PostgreSQL-adatbázist, és importálása](howto-migrate-using-export-and-import.md)</span><span class="sxs-lookup"><span data-stu-id="a232b-117">toomigrate a PostgreSQL database using export and import, see [Migrate your PostgreSQL database using export and import](howto-migrate-using-export-and-import.md)</span></span>
