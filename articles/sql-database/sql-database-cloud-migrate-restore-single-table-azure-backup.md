---
title: "egy táblát az Azure SQL Database biztonsági másolatból aaaRestore |} Microsoft Docs"
description: "Ismerje meg, hogyan toorestore egyetlen táblát az Azure SQL Database biztonsági másolatból."
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: 340b41bd-9df8-47fb-adfc-03216de38a5e
ms.service: sql-database
ms.custom: business continuity
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: daleche
ms.openlocfilehash: 696d2ac87a70bccdf063bfecb8255723404aa5bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorestore-a-single-table-from-an-azure-sql-database-backup"></a><span data-ttu-id="300b2-103">Hogyan toorestore egyetlen tábla egy Azure SQL-adatbázis másolatból</span><span class="sxs-lookup"><span data-stu-id="300b2-103">How toorestore a single table from an Azure SQL Database backup</span></span>
<span data-ttu-id="300b2-104">Felmerülhet a helyzetet, amelyben véletlenül módosította az SQL-adatbázisban található egyes adatokat, és most szeretné toorecover hello egyetlen érintett tábla.</span><span class="sxs-lookup"><span data-stu-id="300b2-104">You may encounter a situation in which you accidentally modified some data in a SQL database and now you want toorecover hello single affected table.</span></span> <span data-ttu-id="300b2-105">Ez a cikk ismerteti, hogyan toorestore egyetlen tábla az SQL-adatbázis hello egyik adatbázisban [automatikus biztonsági mentésekhez](sql-database-automated-backups.md).</span><span class="sxs-lookup"><span data-stu-id="300b2-105">This article describes how toorestore a single table in a database from one of hello SQL Database [automatic backups](sql-database-automated-backups.md).</span></span>

## <a name="preparation-steps-rename-hello-table-and-restore-a-copy-of-hello-database"></a><span data-ttu-id="300b2-106">Előkészítő lépések: hello táblázat átnevezése és hello adatbázis másolatának visszaállítása</span><span class="sxs-lookup"><span data-stu-id="300b2-106">Preparation steps: Rename hello table and restore a copy of hello database</span></span>
1. <span data-ttu-id="300b2-107">Azonosítsa az Azure SQL-adatbázis, amelyet vissza hello példányra tooreplace hello táblájában.</span><span class="sxs-lookup"><span data-stu-id="300b2-107">Identify hello table in your Azure SQL database that you want tooreplace with hello restored copy.</span></span> <span data-ttu-id="300b2-108">Microsoft SQL Management Studio toorename hello táblázattal.</span><span class="sxs-lookup"><span data-stu-id="300b2-108">Use Microsoft SQL Management Studio toorename hello table.</span></span> <span data-ttu-id="300b2-109">Például nevezze át a táblázatban hello &lt;táblanév&gt;_old.</span><span class="sxs-lookup"><span data-stu-id="300b2-109">For example, rename hello table as &lt;table name&gt;_old.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="300b2-110">tooavoid blokkolja, győződjön meg arról, hogy nem készül átnevezni hello tábla futó tevékenység.</span><span class="sxs-lookup"><span data-stu-id="300b2-110">tooavoid being blocked, make sure that there's no activity running on hello table that you are renaming.</span></span> <span data-ttu-id="300b2-111">Ha hibát tapasztal, győződjön meg arról, hogy ezzel az eljárással a karbantartási időszak alatt.</span><span class="sxs-lookup"><span data-stu-id="300b2-111">If you encounter issues, make sure that perform this procedure during a maintenance window.</span></span>
   >

2. <span data-ttu-id="300b2-112">A biztonsági másolat visszaállításával a adatbázis tooa pont idő, amelyet az toorecover toousing hello [pontot-In_Time visszaállítása](sql-database-recovery-using-backups.md#point-in-time-restore) lépéseket.</span><span class="sxs-lookup"><span data-stu-id="300b2-112">Restore a backup of your database tooa point in time that you want toorecover toousing hello [Point-In_Time Restore](sql-database-recovery-using-backups.md#point-in-time-restore) steps.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="300b2-113">hello visszaállított adatbázis hello neve lesz hello DBName + időbélyeg-formátumnak; például **Adventureworks2012_2016-01-01T22-12Z**.</span><span class="sxs-lookup"><span data-stu-id="300b2-113">hello name of hello restored database will be in hello DBName+TimeStamp format; for example, **Adventureworks2012_2016-01-01T22-12Z**.</span></span> <span data-ttu-id="300b2-114">Ez a lépés nem írja felül a meglévő adatbázis nevének hello hello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="300b2-114">This step doesn't overwrite hello existing database name on hello server.</span></span> <span data-ttu-id="300b2-115">Ez egy biztonsági intézkedés, és célja tooallow tooverify hello visszaállított adatbázis ahhoz, hogy az aktuális adatbázis törlése és átnevezése hello visszaállított adatbázis üzemi használatra.</span><span class="sxs-lookup"><span data-stu-id="300b2-115">This is a safety measure, and it's intended tooallow you tooverify hello restored database before they drop their current database and rename hello restored database for production use.</span></span>
   
## <a name="copying-hello-table-from-hello-restored-database-by-using-hello-sql-database-migration-tool"></a><span data-ttu-id="300b2-116">Hello másolása hello tábla adatbázis visszaállítása hello SQL-adatbázis áttelepítéséhez eszközzel</span><span class="sxs-lookup"><span data-stu-id="300b2-116">Copying hello table from hello restored database by using hello SQL Database Migration tool</span></span>

1. <span data-ttu-id="300b2-117">Töltse le és telepítse a hello [SQL-adatbázis áttelepítése varázsló](https://sqlazuremw.codeplex.com).</span><span class="sxs-lookup"><span data-stu-id="300b2-117">Download and install hello [SQL Database Migration Wizard](https://sqlazuremw.codeplex.com).</span></span>
2. <span data-ttu-id="300b2-118">Nyitott hello SQL-adatbázis áttelepítése varázsló, a hello **válasszon folyamat** lapon, válassza ki **elemzési vagy áttelepítése alatt adatbázis**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="300b2-118">Open hello SQL Database Migration Wizard, on hello **Select Process** page, select **Database under Analyze/Migrate**, and then click **Next**.</span></span>

   ![SQL-adatbázis áttelepítése varázsló – folyamat kiválasztása](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)

3. <span data-ttu-id="300b2-120">A hello **tooServer csatlakozás** párbeszédpanel mezőbe hello a következő beállítások érvényesek:</span><span class="sxs-lookup"><span data-stu-id="300b2-120">In hello **Connect tooServer** dialog box, apply hello following settings:</span></span>

   * <span data-ttu-id="300b2-121">Kiszolgáló neve: **az SQL server**</span><span class="sxs-lookup"><span data-stu-id="300b2-121">Server name: **Your SQL server**</span></span>
   * <span data-ttu-id="300b2-122">Hitelesítési: **SQL Server-hitelesítés**</span><span class="sxs-lookup"><span data-stu-id="300b2-122">Authentication: **SQL Server Authentication**</span></span>
   * <span data-ttu-id="300b2-123">Bejelentkezés: **bejelentkezési adatait**</span><span class="sxs-lookup"><span data-stu-id="300b2-123">Login: **Your login**</span></span>
   * <span data-ttu-id="300b2-124">Jelszó: **jelszavát**</span><span class="sxs-lookup"><span data-stu-id="300b2-124">Password: **Your password**</span></span>
   * <span data-ttu-id="300b2-125">Adatbázis: **Master DB (listában az összes adatbázis)**</span><span class="sxs-lookup"><span data-stu-id="300b2-125">Database: **Master DB (List all databases)**</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="300b2-126">Alapértelmezés szerint a hello varázslója menti a bejelentkezési adatokat.</span><span class="sxs-lookup"><span data-stu-id="300b2-126">By default hello wizard saves your login information.</span></span> <span data-ttu-id="300b2-127">Ha úgy, hogy nem szeretné, válassza ki a **elfelejti bejelentkezési adatok**.</span><span class="sxs-lookup"><span data-stu-id="300b2-127">If you don't want it to, select **Forget Login Information**.</span></span>
   >
   
     ![SQL-adatbázis áttelepítése varázsló - forrás kiválasztása - 1.lépés](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. <span data-ttu-id="300b2-129">A hello **forrásának kijelölése** párbeszédpanel megnyitásához, jelölje be hello vissza adatbázis nevét a hello **előkészítő lépések** szakaszt, mint a forrás-, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="300b2-129">In hello **Select Source** dialog box, select hello restored database name from hello **Preparation steps** section as your source, and then click **Next**.</span></span>
   
    ![SQL-adatbázis áttelepítése varázsló - forrás kiválasztása - lépés 2. régiója](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)
5. <span data-ttu-id="300b2-131">A hello **válasszon objektumok** párbeszédpanel megnyitásához, jelölje be hello **válassza ki az adott adatbázis-objektumok** lehetőséget, majd válassza ki a megjeleníteni kívánt toomigrate toohello célkiszolgáló hello table(or tables).</span><span class="sxs-lookup"><span data-stu-id="300b2-131">In hello **Choose Objects** dialog box, select hello **Select specific database objects** option, and then select hello table(or tables) that you want toomigrate toohello target server.</span></span>
   <span data-ttu-id="300b2-132">![SQL-adatbázis áttelepítése varázsló – objektumok kiválasztása](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)</span><span class="sxs-lookup"><span data-stu-id="300b2-132">![SQL Database Migration wizard - Choose Objects](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)</span></span>
6. <span data-ttu-id="300b2-133">A hello **parancsfájl varázsló összefoglalás** kattintson **Igen** amikor felszólítást kap arról, hogy a számítógép készen áll a toogenerate egy SQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="300b2-133">On hello **Script Wizard Summary** page, click **Yes** when you’re prompted about whether you’re ready toogenerate a SQL script.</span></span> <span data-ttu-id="300b2-134">Akkor is hello beállítás toosave hello TSQL parancsfájl későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="300b2-134">You also have hello option toosave hello TSQL Script for later use.</span></span>
   <span data-ttu-id="300b2-135">![SQL-adatbázis áttelepítése varázsló - parancsfájl varázsló összefoglalás](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)</span><span class="sxs-lookup"><span data-stu-id="300b2-135">![SQL Database Migration wizard - Script Wizard Summary](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)</span></span>
7. <span data-ttu-id="300b2-136">A hello **a teszteredményeket összegző** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="300b2-136">On hello **Results Summary** page, click **Next**.</span></span>
   <span data-ttu-id="300b2-137">![SQL-adatbázis áttelepítése varázsló - eredmények összefoglalása](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)</span><span class="sxs-lookup"><span data-stu-id="300b2-137">![SQL Database Migration wizard - Results Summary](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)</span></span>
8. <span data-ttu-id="300b2-138">A hello **cél kiszolgáló kapcsolat beállítása** lapján kattintson **tooServer csatlakozás**, majd adja meg az alábbiak szerint hello részletek:</span><span class="sxs-lookup"><span data-stu-id="300b2-138">On hello **Setup Target Server Connection** page, click **Connect tooServer**, and then enter hello details as follows:</span></span>
   
   * <span data-ttu-id="300b2-139">**Kiszolgálónév**: cél server-példány</span><span class="sxs-lookup"><span data-stu-id="300b2-139">**Server Name**: Target server instance</span></span>
   * <span data-ttu-id="300b2-140">**Hitelesítési**: **SQL Server-hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="300b2-140">**Authentication**: **SQL Server authentication**.</span></span> <span data-ttu-id="300b2-141">Adja meg a bejelentkezési hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="300b2-141">Enter your login credentials.</span></span>
   * <span data-ttu-id="300b2-142">**Adatbázis**: **Master DB (listában az összes adatbázis)**.</span><span class="sxs-lookup"><span data-stu-id="300b2-142">**Database**: **Master DB (List all databases)**.</span></span> <span data-ttu-id="300b2-143">Ez a beállítás minden hello adatbázisok hello célkiszolgálón sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="300b2-143">This option lists all hello databases on hello target server.</span></span>
     
     ![SQL-adatbázis áttelepítése varázsló – cél kiszolgáló kapcsolat beállítása](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)
9. <span data-ttu-id="300b2-145">Kattintson a **Connect**, jelölje be hello céladatbázis szeretné toomove hello táblázatot, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="300b2-145">Click **Connect**, select hello target database that you want toomove hello table to, and then click **Next**.</span></span> <span data-ttu-id="300b2-146">Ez be kell fejeződnie korábban létrehozott hello parancsprogram futtatása, és megtekintheti az hello újonnan áthelyezése a táblázat másolt toohello céladatbázis.</span><span class="sxs-lookup"><span data-stu-id="300b2-146">This should finish running hello previously generated script, and you should see hello newly moved table copied toohello target database.</span></span>

## <a name="verification-step"></a><span data-ttu-id="300b2-147">Ellenőrzési lépés</span><span class="sxs-lookup"><span data-stu-id="300b2-147">Verification step</span></span>

- <span data-ttu-id="300b2-148">Lekérdezés és a vizsgálati hello újonnan másolt tábla toomake arról, hogy hello adatok érintetlenül maradnak.</span><span class="sxs-lookup"><span data-stu-id="300b2-148">Query and test hello newly copied table toomake sure that hello data is intact.</span></span> <span data-ttu-id="300b2-149">Amint megerősíti, dobja el átnevezett hello táblázatos formában **előkészítő lépések** szakasz.</span><span class="sxs-lookup"><span data-stu-id="300b2-149">Upon confirmation, you can drop hello renamed table form **Preparation steps** section.</span></span> <span data-ttu-id="300b2-150">(például &lt;táblanév&gt;_old).</span><span class="sxs-lookup"><span data-stu-id="300b2-150">(for example, &lt;table name&gt;_old).</span></span>

## <a name="next-steps"></a><span data-ttu-id="300b2-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="300b2-151">Next steps</span></span>
[<span data-ttu-id="300b2-152">SQL-adatbázis automatikus biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="300b2-152">SQL Database automatic backups</span></span>](sql-database-automated-backups.md)

