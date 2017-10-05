---
title: "Az Azure SQL Data Warehouse (Azure-portál) számítási teljesítményt kezelése |} Microsoft Docs"
description: "Az Azure portál feladatok kezelésére számítási teljesítményt. Skála dwu-k beállításával számítási erőforrásokat. Vagy, és sablonok felfüggesztése és folytatása a számítási erőforrásokat költségek csökkentése érdekében."
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
ms.openlocfilehash: 63888d5dd103b585cf18e4787d3e779810163e3d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a><span data-ttu-id="dd508-105">Kezelheti a számítási teljesítményt az Azure SQL Data Warehouse (Azure-portál)</span><span class="sxs-lookup"><span data-stu-id="dd508-105">Manage compute power in Azure SQL Data Warehouse (Azure portal)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dd508-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="dd508-106">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="dd508-107">Portal</span><span class="sxs-lookup"><span data-stu-id="dd508-107">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="dd508-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="dd508-108">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="dd508-109">REST</span><span class="sxs-lookup"><span data-stu-id="dd508-109">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="dd508-110">TSQL</span><span class="sxs-lookup"><span data-stu-id="dd508-110">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>


## <a name="scale-compute-power"></a><span data-ttu-id="dd508-111">Skála számítási teljesítmény</span><span class="sxs-lookup"><span data-stu-id="dd508-111">Scale compute power</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="dd508-112">Számítási erőforrásokat módosítása:</span><span class="sxs-lookup"><span data-stu-id="dd508-112">To change compute resources:</span></span>

1. <span data-ttu-id="dd508-113">Nyissa meg a [Azure-portálon][Azure portal], nyissa meg az adatbázishoz, és kattintson a **méretezési**.</span><span class="sxs-lookup"><span data-stu-id="dd508-113">Open the [Azure portal][Azure portal], open your database, and click **Scale**.</span></span>

    ![Kattintson a Scale][1]
2. <span data-ttu-id="dd508-115">A méretezése a panelen a csúszka jobbra vagy balra módosítása a DWU-beállítására állnak.</span><span class="sxs-lookup"><span data-stu-id="dd508-115">In the Scale blade, move the slider left or right to change the DWU setting.</span></span>

    ![Csúszka][2]
3. <span data-ttu-id="dd508-117">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="dd508-117">Click **Save**.</span></span> <span data-ttu-id="dd508-118">Egy megerősítő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="dd508-118">A confirmation message appears.</span></span> <span data-ttu-id="dd508-119">Kattintson a **Igen** megerősítéséhez vagy **nem** megszakítja a műveletet.</span><span class="sxs-lookup"><span data-stu-id="dd508-119">Click **yes** to confirm or **no** to cancel.</span></span>

    ![Kattintson a Save (Mentés) gombra.][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="dd508-121">Felfüggesztés számítási</span><span class="sxs-lookup"><span data-stu-id="dd508-121">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="dd508-122">Egy adatbázis felfüggesztése:</span><span class="sxs-lookup"><span data-stu-id="dd508-122">To pause a database:</span></span>

1. <span data-ttu-id="dd508-123">Nyissa meg a [Azure-portálon] [ Azure portal] , és nyissa meg az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="dd508-123">Open the [Azure portal][Azure portal] and open your database.</span></span> <span data-ttu-id="dd508-124">Figyelje meg, hogy az állapot **Online**.</span><span class="sxs-lookup"><span data-stu-id="dd508-124">Notice that the Status is **Online**.</span></span>

    ![Online állapota][6]
2. <span data-ttu-id="dd508-126">Számítási és memória-erőforrások felfüggesztéséhez kattintson **szüneteltetése**, majd egy megerősítő üzenet megjelenik -e.</span><span class="sxs-lookup"><span data-stu-id="dd508-126">To suspend compute and memory resources, click **Pause**, and then a confirmation message appears.</span></span> <span data-ttu-id="dd508-127">Kattintson a **Igen** megerősítéséhez vagy **nem** megszakítja a műveletet.</span><span class="sxs-lookup"><span data-stu-id="dd508-127">Click **yes** to confirm or **no** to cancel.</span></span>

    ![Erősítse meg a felfüggesztés][7]
3. <span data-ttu-id="dd508-129">Az SQL Data Warehouse megkezdi az adatbázisban, amíg az állapot értéke **felfüggesztése**.</span><span class="sxs-lookup"><span data-stu-id="dd508-129">While SQL Data Warehouse is starting the database, the status is **Pausing**.</span></span>
4. <span data-ttu-id="dd508-130">Ha az állapot értéke **felfüggesztve**, a szüneteltetési művelete történik, és van már nem kívánt szó, a dwu-k.</span><span class="sxs-lookup"><span data-stu-id="dd508-130">When the status is **Paused**, the pause operation is done and you are no longer being charged for DWUs.</span></span>

    ![Felfüggesztés állapota][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="dd508-132">Folytatás számítási</span><span class="sxs-lookup"><span data-stu-id="dd508-132">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="dd508-133">Egy adatbázis folytatása:</span><span class="sxs-lookup"><span data-stu-id="dd508-133">To resume a database:</span></span>

1. <span data-ttu-id="dd508-134">Nyissa meg a [Azure-portálon] [ Azure portal] , és nyissa meg az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="dd508-134">Open the [Azure portal][Azure portal] and open your database.</span></span> <span data-ttu-id="dd508-135">Figyelje meg, hogy az állapot **felfüggesztve**.</span><span class="sxs-lookup"><span data-stu-id="dd508-135">Notice that the Status is **Paused**.</span></span>

    ![Felfüggesztés adatbázis][4]
2. <span data-ttu-id="dd508-137">Kattintson a adatbázis folytatása **Start**, majd egy megerősítő üzenet megjelenik -e.</span><span class="sxs-lookup"><span data-stu-id="dd508-137">To resume the database click **Start**, and then a confirmation message appears.</span></span> <span data-ttu-id="dd508-138">Kattintson a **Igen** megerősítéséhez vagy **nem** megszakítja a műveletet.</span><span class="sxs-lookup"><span data-stu-id="dd508-138">Click **yes** to confirm or **no** to cancel.</span></span>

    ![Erősítse meg a folytatási][5]
3. <span data-ttu-id="dd508-140">Az SQL Data Warehouse megkezdi az adatbázisban, amíg az állapot értéke "Folytatása".</span><span class="sxs-lookup"><span data-stu-id="dd508-140">While SQL Data Warehouse is starting the database, the status is "Resuming".</span></span>
4. <span data-ttu-id="dd508-141">Ha az állapot értéke **online**, az adatbázis készen áll.</span><span class="sxs-lookup"><span data-stu-id="dd508-141">When the status is **online**, the database is ready.</span></span>

    ![Online állapota][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="dd508-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dd508-143">Next steps</span></span>
<span data-ttu-id="dd508-144">További információkért lásd: [kezelése-áttekintés][Management overview].</span><span class="sxs-lookup"><span data-stu-id="dd508-144">For more information, see [Management overview][Management overview].</span></span>

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
