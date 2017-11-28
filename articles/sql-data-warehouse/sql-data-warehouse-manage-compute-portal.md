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
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a><span data-ttu-id="1d1f3-105">Kezelheti a számítási teljesítményt az Azure SQL Data Warehouse (Azure-portál)</span><span class="sxs-lookup"><span data-stu-id="1d1f3-105">Manage compute power in Azure SQL Data Warehouse (Azure portal)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1d1f3-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="1d1f3-106">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="1d1f3-107">Portál</span><span class="sxs-lookup"><span data-stu-id="1d1f3-107">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="1d1f3-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1d1f3-108">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="1d1f3-109">REST</span><span class="sxs-lookup"><span data-stu-id="1d1f3-109">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="1d1f3-110">TSQL</span><span class="sxs-lookup"><span data-stu-id="1d1f3-110">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>


## <a name="scale-compute-power"></a><span data-ttu-id="1d1f3-111">Skála számítási teljesítmény</span><span class="sxs-lookup"><span data-stu-id="1d1f3-111">Scale compute power</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="1d1f3-112">toochange számítási erőforrások:</span><span class="sxs-lookup"><span data-stu-id="1d1f3-112">toochange compute resources:</span></span>

1. <span data-ttu-id="1d1f3-113">Nyissa meg hello [Azure-portálon][Azure portal], nyissa meg az adatbázishoz, és kattintson a **méretezési**.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-113">Open hello [Azure portal][Azure portal], open your database, and click **Scale**.</span></span>

    ![Kattintson a Scale][1]
2. <span data-ttu-id="1d1f3-115">Hello méretezése a panelen bal oldali hello csúszkát, vagy a jobb gombbal a toochange hello DWU-beállítására állnak.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-115">In hello Scale blade, move hello slider left or right toochange hello DWU setting.</span></span>

    ![Csúszka][2]
3. <span data-ttu-id="1d1f3-117">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-117">Click **Save**.</span></span> <span data-ttu-id="1d1f3-118">Egy megerősítő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-118">A confirmation message appears.</span></span> <span data-ttu-id="1d1f3-119">Kattintson a **Igen** tooconfirm vagy **nem** toocancel.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-119">Click **yes** tooconfirm or **no** toocancel.</span></span>

    ![Kattintson a Save (Mentés) gombra.][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a><span data-ttu-id="1d1f3-121">Felfüggesztés számítási</span><span class="sxs-lookup"><span data-stu-id="1d1f3-121">Pause compute</span></span>
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

<span data-ttu-id="1d1f3-122">egy adatbázis toopause:</span><span class="sxs-lookup"><span data-stu-id="1d1f3-122">toopause a database:</span></span>

1. <span data-ttu-id="1d1f3-123">Nyissa meg hello [Azure-portálon] [ Azure portal] , és nyissa meg az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-123">Open hello [Azure portal][Azure portal] and open your database.</span></span> <span data-ttu-id="1d1f3-124">Figyelje meg, hogy állapota hello **Online**.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-124">Notice that hello Status is **Online**.</span></span>

    ![Online állapota][6]
2. <span data-ttu-id="1d1f3-126">toosuspend számítási és memória-erőforrásokat, kattintson a **szüneteltetése**, majd egy megerősítő üzenet megjelenik -e.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-126">toosuspend compute and memory resources, click **Pause**, and then a confirmation message appears.</span></span> <span data-ttu-id="1d1f3-127">Kattintson a **Igen** tooconfirm vagy **nem** toocancel.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-127">Click **yes** tooconfirm or **no** toocancel.</span></span>

    ![Erősítse meg a felfüggesztés][7]
3. <span data-ttu-id="1d1f3-129">Az SQL Data Warehouse hello adatbázis indul, míg hello állapota **felfüggesztése**.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-129">While SQL Data Warehouse is starting hello database, hello status is **Pausing**.</span></span>
4. <span data-ttu-id="1d1f3-130">Hello állapota mikor **felfüggesztve**, hello szüneteltetési művelete történik, és van már nem kívánt szó, a dwu-k.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-130">When hello status is **Paused**, hello pause operation is done and you are no longer being charged for DWUs.</span></span>

    ![Felfüggesztés állapota][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a><span data-ttu-id="1d1f3-132">Folytatás számítási</span><span class="sxs-lookup"><span data-stu-id="1d1f3-132">Resume compute</span></span>
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

<span data-ttu-id="1d1f3-133">egy adatbázis tooresume:</span><span class="sxs-lookup"><span data-stu-id="1d1f3-133">tooresume a database:</span></span>

1. <span data-ttu-id="1d1f3-134">Nyissa meg hello [Azure-portálon] [ Azure portal] , és nyissa meg az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-134">Open hello [Azure portal][Azure portal] and open your database.</span></span> <span data-ttu-id="1d1f3-135">Figyelje meg, hogy állapota hello **felfüggesztve**.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-135">Notice that hello Status is **Paused**.</span></span>

    ![Felfüggesztés adatbázis][4]
2. <span data-ttu-id="1d1f3-137">tooresume hello adatbázis kattintson **Start**, majd egy megerősítő üzenet megjelenik -e.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-137">tooresume hello database click **Start**, and then a confirmation message appears.</span></span> <span data-ttu-id="1d1f3-138">Kattintson a **Igen** tooconfirm vagy **nem** toocancel.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-138">Click **yes** tooconfirm or **no** toocancel.</span></span>

    ![Erősítse meg a folytatási][5]
3. <span data-ttu-id="1d1f3-140">Az SQL Data Warehouse hello adatbázis indul, míg hello állapota "Folytatása".</span><span class="sxs-lookup"><span data-stu-id="1d1f3-140">While SQL Data Warehouse is starting hello database, hello status is "Resuming".</span></span>
4. <span data-ttu-id="1d1f3-141">Hello állapota mikor **online**, készen áll a hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="1d1f3-141">When hello status is **online**, hello database is ready.</span></span>

    ![Online állapota][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="1d1f3-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d1f3-143">Next steps</span></span>
<span data-ttu-id="1d1f3-144">További információkért lásd: [kezelése-áttekintés][Management overview].</span><span class="sxs-lookup"><span data-stu-id="1d1f3-144">For more information, see [Management overview][Management overview].</span></span>

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
