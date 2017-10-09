---
title: "aaaManage a StorSimple biztonsági mentés katalógusa |} Microsoft Docs"
description: "Azt ismerteti, hogyan toouse hello StorSimple Device Manager szolgáltatás biztonságimásolat-katalógus lap toolist, jelölje ki, és törölje a biztonsági mentés."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: e464609e74409a06a198790719abd82ed03c03d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-your-backup-catalog"></a><span data-ttu-id="b1dd0-103">A biztonságimásolat-katalógus használatáról az hello StorSimple Device Manager szolgáltatás toomanage</span><span class="sxs-lookup"><span data-stu-id="b1dd0-103">Use hello StorSimple Device Manager service toomanage your backup catalog</span></span>
## <a name="overview"></a><span data-ttu-id="b1dd0-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b1dd0-104">Overview</span></span>
<span data-ttu-id="b1dd0-105">StorSimple Device Manager szolgáltatás hello **biztonságimásolat-katalógus** panelt jeleníti meg, amely jönnek létre, ha manuális vagy ütemezett biztonsági mentést készít minden hello biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-105">hello StorSimple Device Manager service **Backup Catalog** blade displays all hello backup sets that are created when manual or scheduled backups are taken.</span></span> <span data-ttu-id="b1dd0-106">A lap toolist összes hello biztonsági mentés használja a biztonsági mentési házirend, vagy egy kötetet, jelölje be vagy törölje a biztonsági mentések, vagy használja a biztonsági mentési toorestore vagy kötet klónozása.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-106">You can use this page toolist all hello backups for a backup policy or a volume, select or delete backups, or use a backup toorestore or clone a volume.</span></span>

<span data-ttu-id="b1dd0-107">Ez az oktatóanyag azt ismerteti, hogyan toolist, válassza ki és törölje a biztonságimásolat-készlet.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-107">This tutorial explains how toolist, select, and delete a backup set.</span></span> <span data-ttu-id="b1dd0-108">toolearn hogyan toorestore az eszköz biztonsági másolatból, nyissa meg túl[szeretné visszaállítani az eszközt egy biztonságimásolat-készlet](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="b1dd0-108">toolearn how toorestore your device from backup, go too[Restore your device from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span> <span data-ttu-id="b1dd0-109">Hogyan tooclone egy köteten, nyissa meg túl toolearn[klónozza a StorSimple-kötet](storsimple-8000-clone-volume-u2.md).</span><span class="sxs-lookup"><span data-stu-id="b1dd0-109">toolearn how tooclone a volume, go too[Clone a StorSimple volume](storsimple-8000-clone-volume-u2.md).</span></span>

![Biztonságimásolat-katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

<span data-ttu-id="b1dd0-111">Hello **biztonságimásolat-katalógus** panel biztosít egy lekérdezés toonarrow a biztonságimásolat-készlet kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-111">hello **Backup Catalog** blade provides a query toonarrow your backup set selection.</span></span> <span data-ttu-id="b1dd0-112">Hello biztonságimásolat-készletek, amelyek lekérése, a következő paraméterek hello alapján szűrheti:</span><span class="sxs-lookup"><span data-stu-id="b1dd0-112">You can filter hello backup sets that are retrieved, based on hello following parameters:</span></span>

* <span data-ttu-id="b1dd0-113">**Eszköz** – hello eszköz mely hello a biztonságimásolat-készlet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-113">**Device** – hello device on which hello backup set was created.</span></span>
* <span data-ttu-id="b1dd0-114">**A biztonsági mentési házirend, vagy a kötet** – hello biztonsági mentési házirend, vagy a kötet a biztonságimásolat-készlet társítva.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-114">**Backup policy or Volume** – hello backup policy or volume associated with this backup set.</span></span>
* <span data-ttu-id="b1dd0-115">**A kezdő és a** – dátum és idő tartomány hello hello biztonságimásolat-készlet létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-115">**From and To** – hello date and time range when hello backup set was created.</span></span>

<span data-ttu-id="b1dd0-116">hello szűrt biztonságimásolat-készletek majd megjelennének hello a következő attribútumok alapján:</span><span class="sxs-lookup"><span data-stu-id="b1dd0-116">hello filtered backup sets are then tabulated based on hello following attributes:</span></span>

* <span data-ttu-id="b1dd0-117">**Név** – hello hello biztonsági mentési házirend vagy a kötet társított hello biztonságimásolat-készlet neve.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-117">**Name** – hello name of hello backup policy or volume associated with hello backup set.</span></span>
* <span data-ttu-id="b1dd0-118">**Méret** – hello hello biztonságimásolat-készlet tényleges mérete.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-118">**Size** – hello actual size of hello backup set.</span></span>
* <span data-ttu-id="b1dd0-119">**Létrehozás** – hello dátum és idő, amikor hello létrehozásuk.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-119">**Created On** – hello date and time when hello backups were created.</span></span> 
* <span data-ttu-id="b1dd0-120">**Típus** – biztonsági másolat beállítása helyi pillanatképeket és felhőalapú pillanatfelvételek.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-120">**Type** – Backup sets can be local snapshots or cloud snapshots.</span></span> <span data-ttu-id="b1dd0-121">Egy helyi pillanatfelvétel a köteten tárolt összes adat helyileg hello eszköz, biztonsági másolatot, mivel egy felhőalapú pillanatfelvétel toohello az adatok biztonsági mentése kötet hello felhőben található.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-121">A local snapshot is a backup of all your volume data stored locally on hello device, whereas a cloud snapshot refers toohello backup of volume data residing in hello cloud.</span></span> <span data-ttu-id="b1dd0-122">Helyi pillanatképeket gyorsabb hozzáférést biztosít, mivel felhőalapú pillanatfelvételek az adatrugalmasság közül választ.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-122">Local snapshots provide faster access, whereas cloud snapshots are chosen for data resiliency.</span></span>
* <span data-ttu-id="b1dd0-123">**Elindító** – hello biztonsági mentések kezdeményezheti automatikusan egy ütemezés szerint vagy manuálisan egy felhasználó.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-123">**Initiated By** – hello backups can be initiated automatically by a schedule or manually by a user.</span></span> <span data-ttu-id="b1dd0-124">A biztonsági mentési házirend tooschedule biztonsági mentéseket is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-124">You can use a backup policy tooschedule backups.</span></span> <span data-ttu-id="b1dd0-125">Másik lehetőségként használhatja a hello **biztonsági másolatok készítéséhez** beállítás tootake manuális biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-125">Alternatively, you can use hello **Take backup** option tootake a manual backup.</span></span>

## <a name="list-backup-sets-for-a-backup-policy"></a><span data-ttu-id="b1dd0-126">Lista biztonságimásolat-készletet a biztonsági mentési házirend</span><span class="sxs-lookup"><span data-stu-id="b1dd0-126">List backup sets for a backup policy</span></span>
<span data-ttu-id="b1dd0-127">Hajtsa végre a következő lépéseket toolist hello összes hello biztonsági mentés a biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-127">Complete hello following steps toolist all hello backups for a backup policy.</span></span>

#### <a name="toolist-backup-sets"></a><span data-ttu-id="b1dd0-128">toolist biztonságimásolat-készletek</span><span class="sxs-lookup"><span data-stu-id="b1dd0-128">toolist backup sets</span></span>
1. <span data-ttu-id="b1dd0-129">Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **biztonságimásolat-katalógus**.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-129">Go tooyour StorSimple Device Manager service and click **Backup catalog**.</span></span>

2. <span data-ttu-id="b1dd0-130">Az alábbiak szerint szűrheti hello beállításokat:</span><span class="sxs-lookup"><span data-stu-id="b1dd0-130">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="b1dd0-131">Adjon meg hello időtartományt.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-131">Specify hello time range.</span></span>
   2. <span data-ttu-id="b1dd0-132">Válassza ki azt a hello megfelelő eszközt.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-132">Select hello appropriate device.</span></span>
   3. <span data-ttu-id="b1dd0-133">Szűrés **biztonsági mentési házirend** tooview hello megfelelő hello biztonsági mentéseket.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-133">Filter by **Backup policy** tooview hello corresponding hello backups.</span></span>
   3. <span data-ttu-id="b1dd0-134">Hello biztonsági mentési házirend legördülő listából, válassza ki a **összes** összes hello hello kiválasztva a biztonsági mentések tooview eszköz.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-134">From hello backup policy dropdown list, choose **All** tooview all hello backups on hello selected device.</span></span>
   4. <span data-ttu-id="b1dd0-135">Kattintson a **alkalmaz** tooexecute ezt a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-135">Click **Apply** tooexecute this query.</span></span>
      
      <span data-ttu-id="b1dd0-136">hello biztonsági mentések hello kiválasztva a biztonsági mentési házirend társított meg kell jelennie a biztonságimásolat-készletek hello listájában.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-136">hello backups associated with hello selected backup policy should appear in hello list of backup sets.</span></span>

      ![Nyissa meg toobackup katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a><span data-ttu-id="b1dd0-138">Válassza ki a biztonságimásolat-készletből</span><span class="sxs-lookup"><span data-stu-id="b1dd0-138">Select a backup set</span></span>
<span data-ttu-id="b1dd0-139">A következő lépéseket tooselect teljes hello a biztonságimásolat-készlet egy kötet vagy a biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-139">Complete hello following steps tooselect a backup set for a volume or backup policy.</span></span>

#### <a name="tooselect-a-backup-set"></a><span data-ttu-id="b1dd0-140">tooselect biztonságimásolat-készletből</span><span class="sxs-lookup"><span data-stu-id="b1dd0-140">tooselect a backup set</span></span>
1. <span data-ttu-id="b1dd0-141">Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **biztonságimásolat-katalógus**.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-141">Go tooyour StorSimple Device Manager service and click **Backup catalog**.</span></span>
2. <span data-ttu-id="b1dd0-142">Az alábbiak szerint szűrheti hello beállításokat:</span><span class="sxs-lookup"><span data-stu-id="b1dd0-142">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="b1dd0-143">Adjon meg hello időtartományt.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-143">Specify hello time range.</span></span> 
   2. <span data-ttu-id="b1dd0-144">Válassza ki azt a hello megfelelő eszközt.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-144">Select hello appropriate device.</span></span> 
   3. <span data-ttu-id="b1dd0-145">Szűrheti a kötet vagy a biztonsági mentési házirend, hogy kívánja-e tooselect hello biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-145">Filter by volume or backup policy for hello backup that you wish tooselect.</span></span>
   4. <span data-ttu-id="b1dd0-146">Kattintson a **alkalmaz** tooexecute ezt a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-146">Click **Apply** tooexecute this query.</span></span>
      
      <span data-ttu-id="b1dd0-147">hello hello kijelölt kötet vagy a biztonsági mentési házirenddel társított biztonsági másolatok meg kell jelennie hello listájában biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-147">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>

      ![Nyissa meg toobackup katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. <span data-ttu-id="b1dd0-149">Válassza ki, és bontsa ki a biztonságimásolat-készletből.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-149">Select and expand a backup set.</span></span> <span data-ttu-id="b1dd0-150">Most már megtekintheti hello biztonságimásolat-készletek bontásban hello köteteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-150">You can now see hello backup sets broken down by hello volumes that it contains.</span></span> <span data-ttu-id="b1dd0-151">Hello **visszaállítása** és **törlése** beállítások érhetők el hello helyi menüjére (kattintson a jobb gombbal) hello biztonságimásolat-készlet.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-151">hello **Restore** and **Delete** options are available via hello context menu (right-click) for hello backup set.</span></span> <span data-ttu-id="b1dd0-152">Ezek a műveletek egyikét a hello biztonságimásolat-készlet, amely a kijelölt hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-152">You can perform either of these actions on hello backup set that you selected.</span></span>

    ![Nyissa meg toobackup katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a><span data-ttu-id="b1dd0-154">A biztonságimásolat-készlet törlése</span><span class="sxs-lookup"><span data-stu-id="b1dd0-154">Delete a backup set</span></span>
<span data-ttu-id="b1dd0-155">A biztonsági mentés tooretain-hello adatok már nem kívánja törölni.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-155">Delete a backup when you no longer wish tooretain hello data associated with it.</span></span> <span data-ttu-id="b1dd0-156">Hajtsa végre a következő lépéseket toodelete biztonságimásolat-készletből hello.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-156">Perform hello following steps toodelete a backup set.</span></span>

#### <a name="toodelete-a-backup-set"></a><span data-ttu-id="b1dd0-157">toodelete biztonságimásolat-készletből</span><span class="sxs-lookup"><span data-stu-id="b1dd0-157">toodelete a backup set</span></span>
 <span data-ttu-id="b1dd0-158">Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **biztonságimásolat-katalógus**.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-158">Go tooyour StorSimple Device Manager service and click **Backup catalog**.</span></span>
2. <span data-ttu-id="b1dd0-159">Az alábbiak szerint szűrheti hello beállításokat:</span><span class="sxs-lookup"><span data-stu-id="b1dd0-159">Filter hello selections as follows:</span></span>
   
   1. <span data-ttu-id="b1dd0-160">Adjon meg hello időtartományt.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-160">Specify hello time range.</span></span> 
   2. <span data-ttu-id="b1dd0-161">Válassza ki azt a hello megfelelő eszközt.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-161">Select hello appropriate device.</span></span> 
   3. <span data-ttu-id="b1dd0-162">Szűrheti a kötet vagy a biztonsági mentési házirend, hogy kívánja-e tooselect hello biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-162">Filter by volume or backup policy for hello backup that you wish tooselect.</span></span>
   4. <span data-ttu-id="b1dd0-163">Kattintson a **alkalmaz** tooexecute ezt a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-163">Click **Apply** tooexecute this query.</span></span>
      
      <span data-ttu-id="b1dd0-164">hello hello kijelölt kötet vagy a biztonsági mentési házirenddel társított biztonsági másolatok meg kell jelennie hello listájában biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-164">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>

      ![Nyissa meg toobackup katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. <span data-ttu-id="b1dd0-166">Válassza ki, és bontsa ki a biztonságimásolat-készletből.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-166">Select and expand a backup set.</span></span> <span data-ttu-id="b1dd0-167">Most már megtekintheti hello biztonságimásolat-készletek bontásban hello köteteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-167">You can now see hello backup sets broken down by hello volumes that it contains.</span></span> <span data-ttu-id="b1dd0-168">Hello **visszaállítása** és **törlése** beállítások érhetők el hello helyi menüjére (kattintson a jobb gombbal) hello biztonságimásolat-készlet.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-168">hello **Restore** and **Delete** options are available via hello context menu (right-click) for hello backup set.</span></span> <span data-ttu-id="b1dd0-169">Kattintson a jobb gombbal a kiválasztott hello biztonságimásolat-készlet és hello helyi menüben válassza a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-169">Right-click hello selected backup set and from hello context menu, select **Delete**.</span></span>

    ![Nyissa meg toobackup katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

4. <span data-ttu-id="b1dd0-171">Amikor felszólítja a megerősítésre, tekintse át a hello megjelenő információkat, majd kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-171">When prompted for confirmation, review hello displayed information and click **Delete**.</span></span> <span data-ttu-id="b1dd0-172">hello kijelölt biztonsági véglegesen törlődik.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-172">hello selected backup is deleted permanently.</span></span>

    ![Nyissa meg toobackup katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

5. <span data-ttu-id="b1dd0-174">Értesítést fog kapni hello törlése folyamatban van, valamint hogy mikor sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-174">You will be notified when hello deletion is in progress and when it has successfully finished.</span></span> <span data-ttu-id="b1dd0-175">Hello törlése után frissítse a hello lekérdezés ezen a lapon.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-175">After hello deletion is done, refresh hello query on this page.</span></span> <span data-ttu-id="b1dd0-176">törölt hello biztonságimásolat-készlet nem fog megjelenni a biztonsági mentés hello listában.</span><span class="sxs-lookup"><span data-stu-id="b1dd0-176">hello deleted backup set will no longer appear in hello list of backup sets.</span></span>

    ![Nyissa meg toobackup katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a><span data-ttu-id="b1dd0-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b1dd0-178">Next steps</span></span>
* <span data-ttu-id="b1dd0-179">Ismerje meg, hogyan túl[használata hello biztonságimásolat-katalógus toorestore biztonságimásolat-készletből az eszköz](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="b1dd0-179">Learn how too[use hello backup catalog toorestore your device from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span>
* <span data-ttu-id="b1dd0-180">Ismerje meg, hogyan túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="b1dd0-180">Learn how too[use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

