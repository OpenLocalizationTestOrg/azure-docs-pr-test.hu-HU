---
title: "aaaStorSimple Snapshot Manager biztonsági mentési feladatok |} Microsoft Docs"
description: "Útmutatás a toouse StorSimple Snapshot Manager MMC beépülő modul tooview hello és ütemezett, folyamatban levő és befejezett biztonsági mentési feladatok kezelése."
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
ms.openlocfilehash: 3dba0a2aa527d17d67130f537bcdce5722b05a76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-tooview-and-manage-backup-jobs"></a><span data-ttu-id="3eece-103">StorSimple Snapshot Manager tooview használja, és a biztonsági mentési feladatok kezelése</span><span class="sxs-lookup"><span data-stu-id="3eece-103">Use StorSimple Snapshot Manager tooview and manage backup jobs</span></span>

## <a name="overview"></a><span data-ttu-id="3eece-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3eece-104">Overview</span></span>
<span data-ttu-id="3eece-105">Hello **feladatok** hello csomópontja **hatókör** ablaktábla megjeleníti azokat a hello **ütemezett**, **utolsó 24 óra**, és **futtató**biztonsági mentési feladatokat, interaktív vagy a beállított házirend kezdeményezett.</span><span class="sxs-lookup"><span data-stu-id="3eece-105">hello **Jobs** node in hello **Scope** pane shows hello **Scheduled**, **Last 24 hours**, and **Running** backup tasks that you initiated interactively or by a configured policy.</span></span> 

<span data-ttu-id="3eece-106">Ez az oktatóanyag azt ismerteti, hogyan használhatja a hello **feladatok** ütemezett, a legutóbbi és a jelenleg futó biztonsági mentési feladatok csomópont toodisplay információt.</span><span class="sxs-lookup"><span data-stu-id="3eece-106">This tutorial explains how you can use hello **Jobs** node toodisplay information about scheduled, recent, and currently running backup jobs.</span></span> <span data-ttu-id="3eece-107">(feladatok és a megfelelő információkkal hello listája jelenik meg hello **eredmények** ablaktáblán.) Továbbá kattintson a jobb gombbal a listában szereplő feladatok, és tekintse meg a helyi menü, amely felsorolja az elérhető műveletek.</span><span class="sxs-lookup"><span data-stu-id="3eece-107">(hello list of jobs and corresponding information appears in hello **Results** pane.) Additionally, you can right-click a listed job and see a context menu that lists available actions.</span></span>

## <a name="view-scheduled-jobs"></a><span data-ttu-id="3eece-108">Ütemezett feladatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="3eece-108">View scheduled jobs</span></span>
<span data-ttu-id="3eece-109">A következő eljárás tooview ütemezett biztonsági mentési feladatok hello használata.</span><span class="sxs-lookup"><span data-stu-id="3eece-109">Use hello following procedure tooview scheduled backup jobs.</span></span>

#### <a name="tooview-scheduled-jobs"></a><span data-ttu-id="3eece-110">tooview ütemezett feladatok</span><span class="sxs-lookup"><span data-stu-id="3eece-110">tooview scheduled jobs</span></span>
1. <span data-ttu-id="3eece-111">Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="3eece-111">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span> 
2. <span data-ttu-id="3eece-112">A hello **hatókör** ablaktáblában bontsa ki a hello **feladatok** csomópontot, majd kattintson **ütemezett**.</span><span class="sxs-lookup"><span data-stu-id="3eece-112">In hello **Scope** pane, expand hello **Jobs** node, and click **Scheduled**.</span></span> <span data-ttu-id="3eece-113">hello következő jelennek meg adatok a hello **eredmények** panelen:</span><span class="sxs-lookup"><span data-stu-id="3eece-113">hello following information appears in hello **Results** pane:</span></span>
   
   * <span data-ttu-id="3eece-114">**Név** – hello hello ütemezett pillanatkép neve</span><span class="sxs-lookup"><span data-stu-id="3eece-114">**Name** – hello name of hello scheduled snapshot</span></span>
   * <span data-ttu-id="3eece-115">**Ezután futtassa** – hello dátum és a következő ütemezett pillanatképre hello időpontja</span><span class="sxs-lookup"><span data-stu-id="3eece-115">**Next Run** – hello date and time of hello next scheduled snapshot</span></span>
   * <span data-ttu-id="3eece-116">**Legutóbbi futtatás** – hello dátum meghatározott időpontjakor hello legutóbbi ütemezett pillanatkép</span><span class="sxs-lookup"><span data-stu-id="3eece-116">**Last Run** – hello date and time of hello most recent scheduled snapshot</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="3eece-117">Egyszeri csak pillanatképeket hello **következő futásra** és **utolsó futtatása** hello azonos lesz.</span><span class="sxs-lookup"><span data-stu-id="3eece-117">For one-time only snapshots, hello **Next Run** and **Last Run** will be hello same.</span></span>
     
     ![Ütemezett biztonsági mentési feladatok](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. <span data-ttu-id="3eece-119">További műveletek tooperform feladatról, kattintson a jobb gombbal a hello feladat nevére a hello **eredmények** ablaktábla és hello menüpontok közül választhat.</span><span class="sxs-lookup"><span data-stu-id="3eece-119">tooperform additional actions on a specific job, right-click hello job name in hello **Results** pane and select from hello menu options.</span></span>

## <a name="view-recent-jobs"></a><span data-ttu-id="3eece-120">Legutóbbi feladatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="3eece-120">View recent jobs</span></span>
<span data-ttu-id="3eece-121">Használja a következő eljárás tooview biztonsági mentés hello és visszaállítási feladatok végrehajtott hello az elmúlt 24 órában.</span><span class="sxs-lookup"><span data-stu-id="3eece-121">Use hello following procedure tooview backup and restore jobs that were completed in hello last 24 hours.</span></span>

#### <a name="tooview-recent-jobs"></a><span data-ttu-id="3eece-122">tooview legutóbbi feladatokra</span><span class="sxs-lookup"><span data-stu-id="3eece-122">tooview recent jobs</span></span>
1. <span data-ttu-id="3eece-123">Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="3eece-123">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="3eece-124">A hello **hatókör** ablaktáblában bontsa ki a hello **feladatok** csomópontot, majd kattintson **utolsó 24 óra**.</span><span class="sxs-lookup"><span data-stu-id="3eece-124">In hello **Scope** pane, expand hello **Jobs** node, and click **Last 24 hours**.</span></span> <span data-ttu-id="3eece-125">Hello **eredmények** ablaktábla megjeleníti azokat hello tartozó biztonsági mentési feladatok utolsó 24 órában (tooa legfeljebb 64 feladatok).</span><span class="sxs-lookup"><span data-stu-id="3eece-125">hello **Results** pane shows backup jobs for hello last 24 hours (tooa maximum of 64 jobs).</span></span> <span data-ttu-id="3eece-126">hello következő jelennek meg adatok a hello **eredmények** ablaktáblán, attól függően, hogy hello **nézet** lehetőségek:</span><span class="sxs-lookup"><span data-stu-id="3eece-126">hello following information appears in hello **Results** pane, depending on hello **View** options you specify:</span></span>
   
   * <span data-ttu-id="3eece-127">**Név** – hello hello ütemezett pillanatkép neve.</span><span class="sxs-lookup"><span data-stu-id="3eece-127">**Name** – hello name of hello scheduled snapshot.</span></span>
   * <span data-ttu-id="3eece-128">**Lépések** – hello dátum és idő, amikor hello pillanatkép kezdete.</span><span class="sxs-lookup"><span data-stu-id="3eece-128">**Started** – hello date and time when hello snapshot began.</span></span>
   * <span data-ttu-id="3eece-129">**Leállítva** – hello dátum és a hello pillanatkép befejeződött és le lett állítva.</span><span class="sxs-lookup"><span data-stu-id="3eece-129">**Stopped** – hello date and time when hello snapshot finished or was terminated.</span></span>
   * <span data-ttu-id="3eece-130">**Eltelt** – hello időn közötti hello **elindítva** és **leállítva** alkalommal.</span><span class="sxs-lookup"><span data-stu-id="3eece-130">**Elapsed** – hello amount of time between hello **Started** and **Stopped** times.</span></span>
   * <span data-ttu-id="3eece-131">**Állapot** – hello nemrég befejeződött hello feladat állapota.</span><span class="sxs-lookup"><span data-stu-id="3eece-131">**Status** – hello state of hello recently completed job.</span></span> <span data-ttu-id="3eece-132">**Sikeres** azt jelzi, hogy hello biztonsági mentése sikeresen létrejött.</span><span class="sxs-lookup"><span data-stu-id="3eece-132">**Success** indicates that hello backup was created successfully.</span></span> <span data-ttu-id="3eece-133">**Nem sikerült** azt jelzi, hogy hello feladat futtatása sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="3eece-133">**Failed** indicates that hello job did not run successfully.</span></span>
   * <span data-ttu-id="3eece-134">**Információ** – hello hello sikertelenségének okát.</span><span class="sxs-lookup"><span data-stu-id="3eece-134">**Information** – hello reason for hello failure.</span></span>
   * <span data-ttu-id="3eece-135">**Memória (MB) feldolgozott** – hello adatmennyiség (MB-ban) feldolgozott hello kötet csoportból.</span><span class="sxs-lookup"><span data-stu-id="3eece-135">**Bytes processed (MB)** – hello amount of data from hello volume group that was processed (in MBs).</span></span> 
     
     ![Feladatok a hello futott az elmúlt 24 órában](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. <span data-ttu-id="3eece-137">További műveletek tooperform feladatról, kattintson a jobb gombbal a hello feladat nevére a hello **eredmények** ablaktábla és hello menüpontok közül választhat.</span><span class="sxs-lookup"><span data-stu-id="3eece-137">tooperform additional actions on a specific job, right-click hello job name in hello **Results** pane and select from hello menu options.</span></span>
   
    ![Egy feladat törlése](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a><span data-ttu-id="3eece-139">Jelenleg futó feladatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="3eece-139">View currently running jobs</span></span>
<span data-ttu-id="3eece-140">A következő eljárás tooview jelenleg futó feladatok hello használata.</span><span class="sxs-lookup"><span data-stu-id="3eece-140">Use hello following procedure tooview jobs that are currently running.</span></span>

#### <a name="tooview-currently-running-jobs"></a><span data-ttu-id="3eece-141">jelenleg futó feladatok tooview</span><span class="sxs-lookup"><span data-stu-id="3eece-141">tooview currently running jobs</span></span>
1. <span data-ttu-id="3eece-142">Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="3eece-142">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="3eece-143">A hello **hatókör** ablaktáblában bontsa ki a hello **feladatok** csomópontot, majd kattintson **futtató**.</span><span class="sxs-lookup"><span data-stu-id="3eece-143">In hello **Scope** pane, expand hello **Jobs** node, and click **Running**.</span></span> <span data-ttu-id="3eece-144">Attól függően, hogy hello **nézet** beállításokat ad meg, a következő információk hello hello megjelenik **eredmények** panelen:</span><span class="sxs-lookup"><span data-stu-id="3eece-144">Depending on hello **View** options you specify, hello following information appears in hello **Results** pane:</span></span>
   
   * <span data-ttu-id="3eece-145">**Név** – hello hello ütemezett pillanatkép neve.</span><span class="sxs-lookup"><span data-stu-id="3eece-145">**Name** – hello name of hello scheduled snapshot.</span></span>
   * <span data-ttu-id="3eece-146">**Lépések** – hello dátum és idő, amikor hello pillanatkép kezdete.</span><span class="sxs-lookup"><span data-stu-id="3eece-146">**Started** – hello date and time when hello snapshot began.</span></span>
   * <span data-ttu-id="3eece-147">**Ellenőrzőpont** – hello hello biztonsági mentés az aktuális művelet.</span><span class="sxs-lookup"><span data-stu-id="3eece-147">**Checkpoint** – hello current action of hello backup.</span></span>
   * <span data-ttu-id="3eece-148">**Állapot** – hello feldolgozottsági százalékát.</span><span class="sxs-lookup"><span data-stu-id="3eece-148">**Status** – hello percentage of completion.</span></span>
   * <span data-ttu-id="3eece-149">**Eltelt** – hello mennyisége hello biztonsági mentés óta eltelt időt.</span><span class="sxs-lookup"><span data-stu-id="3eece-149">**Elapsed** – hello amount of time that has passed since hello backup began.</span></span> 
   * <span data-ttu-id="3eece-150">**Átlagos átviteli sebessége (MB)** – a teljes ideje (MB) a feldolgozott adatok toothat mérete bájtban megadva aránya.</span><span class="sxs-lookup"><span data-stu-id="3eece-150">**Average throughput (MB)** – ratio of total bytes of data processed toothat of total time taken for processing (MBs).</span></span>
   * <span data-ttu-id="3eece-151">**Memória (MB) feldolgozott** – az adatok feldolgozása (MB) bájtok teljes száma.</span><span class="sxs-lookup"><span data-stu-id="3eece-151">**Bytes processed (MB)** – total bytes of data processed (in MBs).</span></span>
   * <span data-ttu-id="3eece-152">**(MB) írt bájtok** – az adatok írása (MB-ban) bájtok teljes száma.</span><span class="sxs-lookup"><span data-stu-id="3eece-152">**Bytes written (MB)** – total bytes of data written (in MBs).</span></span> <span data-ttu-id="3eece-153">Hello adatok, valamint a hello metaadatokat tartalmaz, és ezért a általában nagyobb, mint a bájtok feldolgozott hello.</span><span class="sxs-lookup"><span data-stu-id="3eece-153">It includes hello data as well as hello metadata and hence is typically greater than hello Bytes Processed.</span></span>
     
     ![Jelenleg futó feladatok](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. <span data-ttu-id="3eece-155">További műveletek tooperform feladatról, kattintson a jobb gombbal a hello feladat nevére a hello **eredmények** ablaktábla és hello menüpontok közül választhat.</span><span class="sxs-lookup"><span data-stu-id="3eece-155">tooperform additional actions on a specific job, right-click hello job name in hello **Results** pane and select from hello menu options.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3eece-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3eece-156">Next steps</span></span>
* <span data-ttu-id="3eece-157">Ismerje meg, hogyan túl[használja a StorSimple Snapshot Manager tooadminister a StorSimple megoldásban](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="3eece-157">Learn how too[use StorSimple Snapshot Manager tooadminister your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="3eece-158">Ismerje meg, hogyan túl[StorSimple Snapshot Manager toomanage hello biztonságimásolat-katalógus használatáról az](storsimple-snapshot-manager-manage-backup-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="3eece-158">Learn how too[use StorSimple Snapshot Manager toomanage hello backup catalog](storsimple-snapshot-manager-manage-backup-catalog.md).</span></span>

