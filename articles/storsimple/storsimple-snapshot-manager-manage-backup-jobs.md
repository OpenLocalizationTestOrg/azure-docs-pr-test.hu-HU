---
title: "StorSimple Snapshot Manager biztonsági mentési feladatok |} Microsoft Docs"
description: "A StorSimple Snapshot Manager beépülő MMC-modulban történő megtekintését és kezelését ütemezett, folyamatban levő és befejezett biztonsági mentési feladatok használatát ismerteti."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: bf4dcff6-c819-4766-b9d9-9922831cb200
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 03e306b62250f2bb033cc14e856a59760b5406c3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a><span data-ttu-id="df67c-103">StorSimple Snapshot Manager segítségével megtekintheti és kezelheti a biztonsági mentési feladatok</span><span class="sxs-lookup"><span data-stu-id="df67c-103">Use StorSimple Snapshot Manager to view and manage backup jobs</span></span>

## <a name="overview"></a><span data-ttu-id="df67c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="df67c-104">Overview</span></span>
<span data-ttu-id="df67c-105">A **feladatok** csomópontja a **hatókör** látható a **ütemezett**, **utolsó 24 óra**, és **futtató** interaktív módon vagy a beállított házirend kezdeményezett biztonsági mentési feladatokat.</span><span class="sxs-lookup"><span data-stu-id="df67c-105">The **Jobs** node in the **Scope** pane shows the **Scheduled**, **Last 24 hours**, and **Running** backup tasks that you initiated interactively or by a configured policy.</span></span> 

<span data-ttu-id="df67c-106">Ez az oktatóanyag azt ismerteti, hogyan használhatja a **feladatok** csomópont ütemezett, a legutóbbi és a jelenleg futó biztonsági mentési feladatok adatainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="df67c-106">This tutorial explains how you can use the **Jobs** node to display information about scheduled, recent, and currently running backup jobs.</span></span> <span data-ttu-id="df67c-107">(A feladatok és a megfelelő információkkal listája jelenik meg a **eredmények** ablaktáblán.) Továbbá kattintson a jobb gombbal a listában szereplő feladatok, és tekintse meg a helyi menü, amely felsorolja az elérhető műveletek.</span><span class="sxs-lookup"><span data-stu-id="df67c-107">(The list of jobs and corresponding information appears in the **Results** pane.) Additionally, you can right-click a listed job and see a context menu that lists available actions.</span></span>

## <a name="view-scheduled-jobs"></a><span data-ttu-id="df67c-108">Ütemezett feladatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="df67c-108">View scheduled jobs</span></span>
<span data-ttu-id="df67c-109">Az alábbi eljárás segítségével megtekintheti az ütemezett biztonsági mentési feladatot.</span><span class="sxs-lookup"><span data-stu-id="df67c-109">Use the following procedure to view scheduled backup jobs.</span></span>

#### <a name="to-view-scheduled-jobs"></a><span data-ttu-id="df67c-110">Ütemezett feladatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="df67c-110">To view scheduled jobs</span></span>
1. <span data-ttu-id="df67c-111">Kattintson az asztal ikonra kattintva indítsa el a StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="df67c-111">Click the desktop icon to start StorSimple Snapshot Manager.</span></span> 
2. <span data-ttu-id="df67c-112">Az a **hatókör** ablaktáblában bontsa ki a **feladatok** csomópontot, majd kattintson **ütemezett**.</span><span class="sxs-lookup"><span data-stu-id="df67c-112">In the **Scope** pane, expand the **Jobs** node, and click **Scheduled**.</span></span> <span data-ttu-id="df67c-113">A következő információ szerepel a **eredmények** panelen:</span><span class="sxs-lookup"><span data-stu-id="df67c-113">The following information appears in the **Results** pane:</span></span>
   
   * <span data-ttu-id="df67c-114">**Név** – az ütemezett pillanatkép neve</span><span class="sxs-lookup"><span data-stu-id="df67c-114">**Name** – the name of the scheduled snapshot</span></span>
   * <span data-ttu-id="df67c-115">**Ezután futtassa** – a dátum és idő, a következő ütemezett pillanatképre</span><span class="sxs-lookup"><span data-stu-id="df67c-115">**Next Run** – the date and time of the next scheduled snapshot</span></span>
   * <span data-ttu-id="df67c-116">**Legutóbbi futtatás** – a dátum és idő, a legutóbbi ütemezett pillanatkép</span><span class="sxs-lookup"><span data-stu-id="df67c-116">**Last Run** – the date and time of the most recent scheduled snapshot</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="df67c-117">Egyszeri csak pillanatképekhez a **következő futásra** és **utolsó futtatása** ugyanaz lesz.</span><span class="sxs-lookup"><span data-stu-id="df67c-117">For one-time only snapshots, the **Next Run** and **Last Run** will be the same.</span></span>
     
     ![Ütemezett biztonsági mentési feladatok](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. <span data-ttu-id="df67c-119">További műveleteket hajt végre egy adott feladat, kattintson a jobb gombbal a projekt nevére a **eredmények** ablaktábla és a menüpontok közül választhat.</span><span class="sxs-lookup"><span data-stu-id="df67c-119">To perform additional actions on a specific job, right-click the job name in the **Results** pane and select from the menu options.</span></span>

## <a name="view-recent-jobs"></a><span data-ttu-id="df67c-120">Legutóbbi feladatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="df67c-120">View recent jobs</span></span>
<span data-ttu-id="df67c-121">A következő eljárás segítségével megtekintheti a biztonsági mentési és visszaállítási feladatok az elmúlt 24 órában elvégzett.</span><span class="sxs-lookup"><span data-stu-id="df67c-121">Use the following procedure to view backup and restore jobs that were completed in the last 24 hours.</span></span>

#### <a name="to-view-recent-jobs"></a><span data-ttu-id="df67c-122">Legutóbbi feladatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="df67c-122">To view recent jobs</span></span>
1. <span data-ttu-id="df67c-123">Kattintson az asztal ikonra kattintva indítsa el a StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="df67c-123">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="df67c-124">Az a **hatókör** ablaktáblában bontsa ki a **feladatok** csomópontot, majd kattintson **utolsó 24 óra**.</span><span class="sxs-lookup"><span data-stu-id="df67c-124">In the **Scope** pane, expand the **Jobs** node, and click **Last 24 hours**.</span></span> <span data-ttu-id="df67c-125">A **eredmények** ablaktábla megjeleníti azokat a biztonsági mentési feladatok, a legutóbbi 24 órában (a maximális 64 feladatok).</span><span class="sxs-lookup"><span data-stu-id="df67c-125">The **Results** pane shows backup jobs for the last 24 hours (to a maximum of 64 jobs).</span></span> <span data-ttu-id="df67c-126">A következő információ szerepel a **eredmények** ablaktáblán, attól függően, a **nézet** lehetőségek:</span><span class="sxs-lookup"><span data-stu-id="df67c-126">The following information appears in the **Results** pane, depending on the **View** options you specify:</span></span>
   
   * <span data-ttu-id="df67c-127">**Név** – az ütemezett pillanatképre nevét.</span><span class="sxs-lookup"><span data-stu-id="df67c-127">**Name** – the name of the scheduled snapshot.</span></span>
   * <span data-ttu-id="df67c-128">**Lépések** – dátum és idő, amikor a pillanatképet kezdete.</span><span class="sxs-lookup"><span data-stu-id="df67c-128">**Started** – the date and time when the snapshot began.</span></span>
   * <span data-ttu-id="df67c-129">**Leállítva** – dátum és idő, amikor a pillanatképet befejeződött, vagy le lett állítva.</span><span class="sxs-lookup"><span data-stu-id="df67c-129">**Stopped** – the date and time when the snapshot finished or was terminated.</span></span>
   * <span data-ttu-id="df67c-130">**Eltelt** – közötti idő a **elindítva** és **leállítva** alkalommal.</span><span class="sxs-lookup"><span data-stu-id="df67c-130">**Elapsed** – the amount of time between the **Started** and **Stopped** times.</span></span>
   * <span data-ttu-id="df67c-131">**Állapot** – a legutóbb befejeződött feladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="df67c-131">**Status** – the state of the recently completed job.</span></span> <span data-ttu-id="df67c-132">**Sikeres** azt jelzi, hogy a biztonsági másolat sikeresen létrejött-e.</span><span class="sxs-lookup"><span data-stu-id="df67c-132">**Success** indicates that the backup was created successfully.</span></span> <span data-ttu-id="df67c-133">**Nem sikerült** azt jelzi, hogy a feladat futtatása sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="df67c-133">**Failed** indicates that the job did not run successfully.</span></span>
   * <span data-ttu-id="df67c-134">**Információ** – a hiba okát.</span><span class="sxs-lookup"><span data-stu-id="df67c-134">**Information** – the reason for the failure.</span></span>
   * <span data-ttu-id="df67c-135">**Memória (MB) feldolgozott** – a kötet csoportból (MB) a feldolgozott adatok mennyisége.</span><span class="sxs-lookup"><span data-stu-id="df67c-135">**Bytes processed (MB)** – the amount of data from the volume group that was processed (in MBs).</span></span> 
     
     ![Feladat futott az elmúlt 24 órában](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. <span data-ttu-id="df67c-137">További műveleteket hajt végre egy adott feladat, kattintson a jobb gombbal a projekt nevére a **eredmények** ablaktábla és a menüpontok közül választhat.</span><span class="sxs-lookup"><span data-stu-id="df67c-137">To perform additional actions on a specific job, right-click the job name in the **Results** pane and select from the menu options.</span></span>
   
    ![Egy feladat törlése](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a><span data-ttu-id="df67c-139">Jelenleg futó feladatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="df67c-139">View currently running jobs</span></span>
<span data-ttu-id="df67c-140">Az alábbi eljárással az éppen futó feladatok megtekintése.</span><span class="sxs-lookup"><span data-stu-id="df67c-140">Use the following procedure to view jobs that are currently running.</span></span>

#### <a name="to-view-currently-running-jobs"></a><span data-ttu-id="df67c-141">Az éppen futó feladatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="df67c-141">To view currently running jobs</span></span>
1. <span data-ttu-id="df67c-142">Kattintson az asztal ikonra kattintva indítsa el a StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="df67c-142">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="df67c-143">Az a **hatókör** ablaktáblában bontsa ki a **feladatok** csomópontot, majd kattintson **futtató**.</span><span class="sxs-lookup"><span data-stu-id="df67c-143">In the **Scope** pane, expand the **Jobs** node, and click **Running**.</span></span> <span data-ttu-id="df67c-144">Attól függően a **nézet** beállításokat ad meg, a következő információ szerepel a **eredmények** panelen:</span><span class="sxs-lookup"><span data-stu-id="df67c-144">Depending on the **View** options you specify, the following information appears in the **Results** pane:</span></span>
   
   * <span data-ttu-id="df67c-145">**Név** – az ütemezett pillanatképre nevét.</span><span class="sxs-lookup"><span data-stu-id="df67c-145">**Name** – the name of the scheduled snapshot.</span></span>
   * <span data-ttu-id="df67c-146">**Lépések** – dátum és idő, amikor a pillanatképet kezdete.</span><span class="sxs-lookup"><span data-stu-id="df67c-146">**Started** – the date and time when the snapshot began.</span></span>
   * <span data-ttu-id="df67c-147">**Ellenőrzőpont** – a biztonsági mentés az aktuális művelet.</span><span class="sxs-lookup"><span data-stu-id="df67c-147">**Checkpoint** – the current action of the backup.</span></span>
   * <span data-ttu-id="df67c-148">**Állapot** – készenléti százalék.</span><span class="sxs-lookup"><span data-stu-id="df67c-148">**Status** – the percentage of completion.</span></span>
   * <span data-ttu-id="df67c-149">**Eltelt** – a biztonsági mentés óta eltelt idő mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="df67c-149">**Elapsed** – the amount of time that has passed since the backup began.</span></span> 
   * <span data-ttu-id="df67c-150">**Átlagos átviteli sebessége (MB)** – viszonyított aránya, valamint a teljes ideje (MB) feldolgozásának feldolgozott adatmennyiség bájtban.</span><span class="sxs-lookup"><span data-stu-id="df67c-150">**Average throughput (MB)** – ratio of total bytes of data processed to that of total time taken for processing (MBs).</span></span>
   * <span data-ttu-id="df67c-151">**Memória (MB) feldolgozott** – az adatok feldolgozása (MB) bájtok teljes száma.</span><span class="sxs-lookup"><span data-stu-id="df67c-151">**Bytes processed (MB)** – total bytes of data processed (in MBs).</span></span>
   * <span data-ttu-id="df67c-152">**(MB) írt bájtok** – az adatok írása (MB-ban) bájtok teljes száma.</span><span class="sxs-lookup"><span data-stu-id="df67c-152">**Bytes written (MB)** – total bytes of data written (in MBs).</span></span> <span data-ttu-id="df67c-153">Tartalmazza az adatokat, valamint a metaadatok, és ezért a általában nagyobb, mint a bájtok feldolgozott.</span><span class="sxs-lookup"><span data-stu-id="df67c-153">It includes the data as well as the metadata and hence is typically greater than the Bytes Processed.</span></span>
     
     ![Jelenleg futó feladatok](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. <span data-ttu-id="df67c-155">További műveleteket hajt végre egy adott feladat, kattintson a jobb gombbal a projekt nevére a **eredmények** ablaktábla és a menüpontok közül választhat.</span><span class="sxs-lookup"><span data-stu-id="df67c-155">To perform additional actions on a specific job, right-click the job name in the **Results** pane and select from the menu options.</span></span>

## <a name="next-steps"></a><span data-ttu-id="df67c-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="df67c-156">Next steps</span></span>
* <span data-ttu-id="df67c-157">Megtudhatja, hogyan [StorSimple Snapshot Manager segítségével felügyelheti a StorSimple megoldásban](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="df67c-157">Learn how to [use StorSimple Snapshot Manager to administer your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="df67c-158">Megtudhatja, hogyan [StorSimple Snapshot Manager segítségével kezelheti a biztonságimásolat-katalógus](storsimple-snapshot-manager-manage-backup-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="df67c-158">Learn how to [use StorSimple Snapshot Manager to manage the backup catalog](storsimple-snapshot-manager-manage-backup-catalog.md).</span></span>

