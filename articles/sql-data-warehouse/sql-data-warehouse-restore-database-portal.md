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
# <a name="restore-azure-sql-data-warehouse-portal"></a>Állítsa vissza az Azure SQL Data Warehouse (portál)
> [!div class="op_single_selector"]
> * [– Áttekintés][Overview]
> * [Portál][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
>
>
Ebből a cikkből megtudhatja, hogyan toorestore Azure SQL Data Warehouse használatával hello Azure-portálon.

## <a name="before-you-begin"></a>Előkészületek
**A DTU-kapacitásának ellenőrzése.** Az SQL Data Warehouse-példányokhoz egy SQL server (például myserver.database.windows.net), amely alapértelmezett adatok átviteli egységek (DTU) tartozó kvóta üzemelteti. Az SQL Data Warehouse próbál visszaállítani, győződjön meg arról, hogy az SQL server rendelkezik-e elég fennmaradó DTU-kvótáról hello adatbázis, amely éppen visszaállítása. toolearn hogyan toocalculate DTU-kvótáról vagy toorequest több Dtu: [DTU-kvótát Módosítás kérése][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Az aktív vagy szüneteltetett adatbázis visszaállítása
egy adatbázis toorestore:

1. Jelentkezzen be toohello [Azure-portálon][Azure portal].
2. Hello bal oldali ablaktáblában jelöljön ki **Tallózás**, majd válassza ki **SQL Server-kiszolgálók**.

    ![Válassza ki a Tallózás > SQL Server-kiszolgálók](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. A kiszolgáló található, és állítsa be azt.

    ![Válassza ki a kiszolgálót](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)
4. Szeretné, hogy a toorestore, és állítsa be azt a hello példány az SQL Data Warehouse megkeresése

    ![Válassza ki az SQL Data Warehouse toorestore hello példánya](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Hello felső hello Data Warehouse panelre, válassza ki a **visszaállítása**.

    ![Válassza ki a visszaállítási](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)
6. Adjon meg egy új **adatbázisnév**.
7. Jelölje be hello legújabb **visszaállítási pont**.

   Ellenőrizze, hogy hello legutóbbi helyreállítási pontot választja. Visszaállítási pontok jelennek meg az egyezményes világidő (UTC), mert hello alapértelmezett beállítás nem feltétlenül hello legutóbbi visszaállítási pontot.

      ![Válasszon ki egy helyreállítási pontot](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)
8. Kattintson az **OK** gombra.
9. hello adatbázis visszaállítási folyamat megkezdődik, és használhatja **értesítések** toomonitor hello folyamat.

> [!NOTE]
> Hello visszaállítás befejeződését követően a helyreállított adatbázis követve konfigurálhatja [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].
>
>

## <a name="restore-a-deleted-database"></a>Törölt adatbázis visszaállítása
a törölt adatbázisok toorestore:

1. Jelentkezzen be toohello [Azure-portálon][Azure portal].
2. Hello bal oldali ablaktáblában jelöljön ki **Tallózás**, majd válassza ki **SQL Server-kiszolgálók**.

    ![Válassza ki a Tallózás > SQL Server-kiszolgálók](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. A kiszolgáló található, és állítsa be azt.

    ![Válassza ki a kiszolgálót](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)
4. Görgessen lefelé toohello **műveletek** szakaszt, a kiszolgálójuk paneljéről.
5. Jelölje be hello **adatbázisait törölte a** csempére.

    ![Válassza ki a hello törölt adatbázisok csempére](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)
6. Válassza ki a megjeleníteni kívánt toorestore hello törölt adatbázis.

    ![Egy adatbázis toorestore kiválasztása](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)
7. Adjon meg egy új **adatbázisnév**.

    ![Adja hozzá a hello adatbázis nevét](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
8. Kattintson az **OK** gombra.
9. hello adatbázis visszaállítási folyamat megkezdődik, és használhatja **értesítések** toomonitor hello folyamat.

> [!NOTE]
> tooconfigure hello visszaállítás befejeződését követően az adatbázis lásd [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].
>
>

## <a name="next-steps"></a>Következő lépések
hello üzleti folytonosságot biztosító szolgáltatásokat az Azure SQL Database kiadásának, olvassa el a hello kapcsolatos toolearn [Azure SQL Database üzleti folytonosság – áttekintés][Azure SQL Database business continuity overview].

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
