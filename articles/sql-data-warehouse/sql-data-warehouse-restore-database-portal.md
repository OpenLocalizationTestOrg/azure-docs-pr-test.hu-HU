---
title: "Azure SQL Data Warehouse (Azure-portál) aaaRestore |} Microsoft Docs"
description: "Az Azure portál feladatokat az Azure SQL-adatraktár a visszaállításakor."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: barbkess
editor: 
ms.assetid: b0aef539-7657-4b0e-9899-74098f5c21bc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 09/21/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: cb225d2a21b61acab70a51b69c266f8d3ffacc9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-azure-sql-data-warehouse-portal"></a><span data-ttu-id="c2f0f-103">Állítsa vissza az Azure SQL Data Warehouse (portál)</span><span class="sxs-lookup"><span data-stu-id="c2f0f-103">Restore Azure SQL Data Warehouse (portal)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="c2f0f-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="c2f0f-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="c2f0f-105">[Portál][Portal]</span><span class="sxs-lookup"><span data-stu-id="c2f0f-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="c2f0f-106">[PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="c2f0f-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="c2f0f-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="c2f0f-107">[REST][REST]</span></span>
>
>
<span data-ttu-id="c2f0f-108">Ebből a cikkből megtudhatja, hogyan toorestore Azure SQL Data Warehouse használatával hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-108">In this article, you will learn how toorestore Azure SQL Data Warehouse by using hello Azure portal.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c2f0f-109">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="c2f0f-109">Before you begin</span></span>
<span data-ttu-id="c2f0f-110">**A DTU-kapacitásának ellenőrzése.**</span><span class="sxs-lookup"><span data-stu-id="c2f0f-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="c2f0f-111">Az SQL Data Warehouse-példányokhoz egy SQL server (például myserver.database.windows.net), amely alapértelmezett adatok átviteli egységek (DTU) tartozó kvóta üzemelteti.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-111">Each instance of SQL Data Warehouse is hosted by a SQL server (for example, myserver.database.windows.net) which has a default data throughput unit (DTU) quota.</span></span> <span data-ttu-id="c2f0f-112">Az SQL Data Warehouse próbál visszaállítani, győződjön meg arról, hogy az SQL server rendelkezik-e elég fennmaradó DTU-kvótáról hello adatbázis, amely éppen visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-112">Before you can restore SQL Data Warehouse, verify that your SQL server has enough remaining DTU quota for hello database that you're restoring.</span></span> <span data-ttu-id="c2f0f-113">toolearn hogyan toocalculate DTU-kvótáról vagy toorequest több Dtu: [DTU-kvótát Módosítás kérése][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="c2f0f-113">toolearn how toocalculate DTU quota or toorequest more DTUs, see [Request a DTU quota change][Request a DTU quota change].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="c2f0f-114">Az aktív vagy szüneteltetett adatbázis visszaállítása</span><span class="sxs-lookup"><span data-stu-id="c2f0f-114">Restore an active or paused database</span></span>
<span data-ttu-id="c2f0f-115">egy adatbázis toorestore:</span><span class="sxs-lookup"><span data-stu-id="c2f0f-115">toorestore a database:</span></span>

1. <span data-ttu-id="c2f0f-116">Jelentkezzen be toohello [Azure-portálon][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="c2f0f-116">Sign in toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="c2f0f-117">Hello bal oldali ablaktáblában jelöljön ki **Tallózás**, majd válassza ki **SQL Server-kiszolgálók**.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-117">In hello left pane, select **Browse**, and then select **SQL servers**.</span></span>

    ![Válassza ki a Tallózás > SQL Server-kiszolgálók](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. <span data-ttu-id="c2f0f-119">A kiszolgáló található, és állítsa be azt.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-119">Find your server, and then select it.</span></span>

    ![Válassza ki a kiszolgálót](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)
4. <span data-ttu-id="c2f0f-121">Szeretné, hogy a toorestore, és állítsa be azt a hello példány az SQL Data Warehouse megkeresése</span><span class="sxs-lookup"><span data-stu-id="c2f0f-121">Find hello instance of SQL Data Warehouse that you want toorestore from, and then select it.</span></span>

    ![Válassza ki az SQL Data Warehouse toorestore hello példánya](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. <span data-ttu-id="c2f0f-123">Hello felső hello Data Warehouse panelre, válassza ki a **visszaállítása**.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-123">At hello top of hello Data Warehouse blade, select **Restore**.</span></span>

    ![Válassza ki a visszaállítási](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)
6. <span data-ttu-id="c2f0f-125">Adjon meg egy új **adatbázisnév**.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-125">Specify a new **Database name**.</span></span>
7. <span data-ttu-id="c2f0f-126">Jelölje be hello legújabb **visszaállítási pont**.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-126">Select hello latest **Restore point**.</span></span>

   <span data-ttu-id="c2f0f-127">Ellenőrizze, hogy hello legutóbbi helyreállítási pontot választja.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-127">Make sure you choose hello latest restore point.</span></span> <span data-ttu-id="c2f0f-128">Visszaállítási pontok jelennek meg az egyezményes világidő (UTC), mert hello alapértelmezett beállítás nem feltétlenül hello legutóbbi visszaállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-128">Because restore points are shown in Coordinated Universal Time (UTC), hello default option might not be hello latest restore point.</span></span>

      ![Válasszon ki egy helyreállítási pontot](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)
8. <span data-ttu-id="c2f0f-130">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-130">Select **OK**.</span></span>
9. <span data-ttu-id="c2f0f-131">hello adatbázis visszaállítási folyamat megkezdődik, és használhatja **értesítések** toomonitor hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-131">hello database restore process will begin, and you can use **NOTIFICATIONS** toomonitor hello process.</span></span>

> [!NOTE]
> <span data-ttu-id="c2f0f-132">Hello visszaállítás befejeződését követően a helyreállított adatbázis követve konfigurálhatja [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="c2f0f-132">After hello restore has finished, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
>
>

## <a name="restore-a-deleted-database"></a><span data-ttu-id="c2f0f-133">Törölt adatbázis visszaállítása</span><span class="sxs-lookup"><span data-stu-id="c2f0f-133">Restore a deleted database</span></span>
<span data-ttu-id="c2f0f-134">a törölt adatbázisok toorestore:</span><span class="sxs-lookup"><span data-stu-id="c2f0f-134">toorestore a deleted database:</span></span>

1. <span data-ttu-id="c2f0f-135">Jelentkezzen be toohello [Azure-portálon][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="c2f0f-135">Sign in toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="c2f0f-136">Hello bal oldali ablaktáblában jelöljön ki **Tallózás**, majd válassza ki **SQL Server-kiszolgálók**.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-136">In hello left pane, select **Browse**, and then select **SQL servers**.</span></span>

    ![Válassza ki a Tallózás > SQL Server-kiszolgálók](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. <span data-ttu-id="c2f0f-138">A kiszolgáló található, és állítsa be azt.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-138">Find your server, and then select it.</span></span>

    ![Válassza ki a kiszolgálót](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)
4. <span data-ttu-id="c2f0f-140">Görgessen lefelé toohello **műveletek** szakaszt, a kiszolgálójuk paneljéről.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-140">Scroll down toohello **Operations** section on your server's blade.</span></span>
5. <span data-ttu-id="c2f0f-141">Jelölje be hello **adatbázisait törölte a** csempére.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-141">Select hello **Deleted databases** tile.</span></span>

    ![Válassza ki a hello törölt adatbázisok csempére](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)
6. <span data-ttu-id="c2f0f-143">Válassza ki a megjeleníteni kívánt toorestore hello törölt adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-143">Select hello deleted database that you want toorestore.</span></span>

    ![Egy adatbázis toorestore kiválasztása](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)
7. <span data-ttu-id="c2f0f-145">Adjon meg egy új **adatbázisnév**.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-145">Specify a new **Database name**.</span></span>

    ![Adja hozzá a hello adatbázis nevét](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
8. <span data-ttu-id="c2f0f-147">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-147">Select **OK**.</span></span>
9. <span data-ttu-id="c2f0f-148">hello adatbázis visszaállítási folyamat megkezdődik, és használhatja **értesítések** toomonitor hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="c2f0f-148">hello database restore process will begin, and you can use **NOTIFICATIONS** toomonitor hello process.</span></span>

> [!NOTE]
> <span data-ttu-id="c2f0f-149">tooconfigure hello visszaállítás befejeződését követően az adatbázis lásd [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="c2f0f-149">tooconfigure your database after hello restore has finished, see [Configure your database after recovery][Configure your database after recovery].</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="c2f0f-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2f0f-150">Next steps</span></span>
<span data-ttu-id="c2f0f-151">hello üzleti folytonosságot biztosító szolgáltatásokat az Azure SQL Database kiadásának, olvassa el a hello kapcsolatos toolearn [Azure SQL Database üzleti folytonosság – áttekintés][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="c2f0f-151">toolearn about hello business continuity features of Azure SQL Database editions, read hello [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portal]: https://portal.azure.com/
