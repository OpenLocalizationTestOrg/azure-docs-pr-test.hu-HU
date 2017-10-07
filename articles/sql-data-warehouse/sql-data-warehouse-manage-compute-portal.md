---
title: "aaaManage számítási teljesítményt az Azure SQL Data Warehouse (Azure-portál) |} Microsoft Docs"
description: "Az Azure portál feladatok toomanage számítási teljesítményt. Skála dwu-k beállításával számítási erőforrásokat. Vagy, és sablonok felfüggesztése és folytatása a számítási erőforrások toosave költségeket."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 233b0da5-4abd-4d1d-9586-4ccc5f50b071
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: b2e84b3763e97ce88c190eecfb64b2d06f727229
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Kezelheti a számítási teljesítményt az Azure SQL Data Warehouse (Azure-portál)
> [!div class="op_single_selector"]
> * [Áttekintés](sql-data-warehouse-manage-compute-overview.md)
> * [Portál](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>


## <a name="scale-compute-power"></a>Skála számítási teljesítmény
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

toochange számítási erőforrások:

1. Nyissa meg hello [Azure-portálon][Azure portal], nyissa meg az adatbázishoz, és kattintson a **méretezési**.

    ![Kattintson a Scale][1]
2. Hello méretezése a panelen bal oldali hello csúszkát, vagy a jobb gombbal a toochange hello DWU-beállítására állnak.

    ![Csúszka][2]
3. Kattintson a **Save** (Mentés) gombra. Egy megerősítő üzenet jelenik meg. Kattintson a **Igen** tooconfirm vagy **nem** toocancel.

    ![Kattintson a Save (Mentés) gombra.][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Felfüggesztés számítási
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

egy adatbázis toopause:

1. Nyissa meg hello [Azure-portálon] [ Azure portal] , és nyissa meg az adatbázis. Figyelje meg, hogy állapota hello **Online**.

    ![Online állapota][6]
2. toosuspend számítási és memória-erőforrásokat, kattintson a **szüneteltetése**, majd egy megerősítő üzenet megjelenik -e. Kattintson a **Igen** tooconfirm vagy **nem** toocancel.

    ![Erősítse meg a felfüggesztés][7]
3. Az SQL Data Warehouse hello adatbázis indul, míg hello állapota **felfüggesztése**.
4. Hello állapota mikor **felfüggesztve**, hello szüneteltetési művelete történik, és van már nem kívánt szó, a dwu-k.

    ![Felfüggesztés állapota][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Folytatás számítási
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

egy adatbázis tooresume:

1. Nyissa meg hello [Azure-portálon] [ Azure portal] , és nyissa meg az adatbázis. Figyelje meg, hogy állapota hello **felfüggesztve**.

    ![Felfüggesztés adatbázis][4]
2. tooresume hello adatbázis kattintson **Start**, majd egy megerősítő üzenet megjelenik -e. Kattintson a **Igen** tooconfirm vagy **nem** toocancel.

    ![Erősítse meg a folytatási][5]
3. Az SQL Data Warehouse hello adatbázis indul, míg hello állapota "Folytatása".
4. Hello állapota mikor **online**, készen áll a hello adatbázis.

    ![Online állapota][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Következő lépések
További információkért lásd: [kezelése-áttekintés][Management overview].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
