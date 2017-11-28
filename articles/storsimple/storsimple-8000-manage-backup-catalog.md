---
title: "A StorSimple biztonságimásolat-katalógus kezelése |} Microsoft Docs"
description: "Ismerteti a StorSimple Device Manager szolgáltatás biztonságimásolat-katalógus lapnak a használatával listában válassza ki, és törölje a biztonsági mentés."
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
ms.openlocfilehash: ac2577c6cd350d6d437d55e61ec73d954cb24893
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-your-backup-catalog"></a><span data-ttu-id="b3bba-103">A StorSimple Device Manager szolgáltatással, a biztonságimásolat-katalógus kezelése</span><span class="sxs-lookup"><span data-stu-id="b3bba-103">Use the StorSimple Device Manager service to manage your backup catalog</span></span>
## <a name="overview"></a><span data-ttu-id="b3bba-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b3bba-104">Overview</span></span>
<span data-ttu-id="b3bba-105">A StorSimple Device Manager szolgáltatás **biztonságimásolat-katalógus** csempe megjeleníti a biztonsági mentés során manuális vagy ütemezett biztonsági mentést készít a létrehozott.</span><span class="sxs-lookup"><span data-stu-id="b3bba-105">The StorSimple Device Manager service **Backup Catalog** blade displays all the backup sets that are created when manual or scheduled backups are taken.</span></span> <span data-ttu-id="b3bba-106">Ezen a lapon a biztonsági mentési házirend, vagy egy kötetet, jelölje be a biztonsági másolatok felsorolása, vagy törölje a biztonsági mentések, vagy a biztonsági másolat használatával állítsa vissza vagy egy kötet klónozása.</span><span class="sxs-lookup"><span data-stu-id="b3bba-106">You can use this page to list all the backups for a backup policy or a volume, select or delete backups, or use a backup to restore or clone a volume.</span></span>

<span data-ttu-id="b3bba-107">Ez az oktatóanyag ismerteti, hogyan listában válassza ki, és egy biztonságimásolat-készlet törlése.</span><span class="sxs-lookup"><span data-stu-id="b3bba-107">This tutorial explains how to list, select, and delete a backup set.</span></span> <span data-ttu-id="b3bba-108">További információk az eszköz visszaállítása biztonsági másolatból, [szeretné visszaállítani az eszközt egy biztonságimásolat-készlet](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="b3bba-108">To learn how to restore your device from backup, go to [Restore your device from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span> <span data-ttu-id="b3bba-109">Kötet klónozása útmutató, keresse fel [klónozza a StorSimple-kötet](storsimple-8000-clone-volume-u2.md).</span><span class="sxs-lookup"><span data-stu-id="b3bba-109">To learn how to clone a volume, go to [Clone a StorSimple volume](storsimple-8000-clone-volume-u2.md).</span></span>

![Biztonságimásolat-katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

<span data-ttu-id="b3bba-111">A **biztonságimásolat-katalógus** panel biztosít egy lekérdezést szűkítheti a biztonsági másolat beállítása.</span><span class="sxs-lookup"><span data-stu-id="b3bba-111">The **Backup Catalog** blade provides a query to narrow your backup set selection.</span></span> <span data-ttu-id="b3bba-112">A biztonságimásolat-készletek, amelyek lekérése, a következő paraméterek alapján szűrheti:</span><span class="sxs-lookup"><span data-stu-id="b3bba-112">You can filter the backup sets that are retrieved, based on the following parameters:</span></span>

* <span data-ttu-id="b3bba-113">**Eszköz** – az eszköz, amelyre a biztonságimásolat-készlet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b3bba-113">**Device** – The device on which the backup set was created.</span></span>
* <span data-ttu-id="b3bba-114">**A biztonsági mentési házirend, vagy a kötet** – a biztonsági mentési házirend, vagy a kötet a biztonságimásolat-készlet társítva.</span><span class="sxs-lookup"><span data-stu-id="b3bba-114">**Backup policy or Volume** – The backup policy or volume associated with this backup set.</span></span>
* <span data-ttu-id="b3bba-115">**A kezdő és a** – a dátum és idő tartomány, a biztonságimásolat-készlet létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b3bba-115">**From and To** – The date and time range when the backup set was created.</span></span>

<span data-ttu-id="b3bba-116">A szűrt biztonsági mentés majd megjelennének a következő attribútumok alapján:</span><span class="sxs-lookup"><span data-stu-id="b3bba-116">The filtered backup sets are then tabulated based on the following attributes:</span></span>

* <span data-ttu-id="b3bba-117">**Név** – a biztonsági mentési házirend, vagy a kötet a biztonságimásolat-készlet társított nevét.</span><span class="sxs-lookup"><span data-stu-id="b3bba-117">**Name** – The name of the backup policy or volume associated with the backup set.</span></span>
* <span data-ttu-id="b3bba-118">**Méret** – a biztonságimásolat-készlet tényleges méretének.</span><span class="sxs-lookup"><span data-stu-id="b3bba-118">**Size** – The actual size of the backup set.</span></span>
* <span data-ttu-id="b3bba-119">**Létrehozás** – a dátum és idő, amikor a biztonsági mentés készült.</span><span class="sxs-lookup"><span data-stu-id="b3bba-119">**Created On** – The date and time when the backups were created.</span></span> 
* <span data-ttu-id="b3bba-120">**Típus** – biztonsági másolat beállítása helyi pillanatképeket és felhőalapú pillanatfelvételek.</span><span class="sxs-lookup"><span data-stu-id="b3bba-120">**Type** – Backup sets can be local snapshots or cloud snapshots.</span></span> <span data-ttu-id="b3bba-121">Egy helyi pillanatfelvétel minden kötet adatait az eszközön, helyileg tárolt biztonsági másolatot, mivel egy felhőalapú pillanatfelvétel hivatkozik a felhőben tárolt kötet adatok biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="b3bba-121">A local snapshot is a backup of all your volume data stored locally on the device, whereas a cloud snapshot refers to the backup of volume data residing in the cloud.</span></span> <span data-ttu-id="b3bba-122">Helyi pillanatképeket gyorsabb hozzáférést biztosít, mivel felhőalapú pillanatfelvételek az adatrugalmasság közül választ.</span><span class="sxs-lookup"><span data-stu-id="b3bba-122">Local snapshots provide faster access, whereas cloud snapshots are chosen for data resiliency.</span></span>
* <span data-ttu-id="b3bba-123">**Elindító** – a biztonsági mentések kezdeményezheti automatikusan egy ütemezés szerint vagy manuálisan egy felhasználó által.</span><span class="sxs-lookup"><span data-stu-id="b3bba-123">**Initiated By** – The backups can be initiated automatically by a schedule or manually by a user.</span></span> <span data-ttu-id="b3bba-124">A biztonsági mentési házirend segítségével biztonsági mentés ütemezése.</span><span class="sxs-lookup"><span data-stu-id="b3bba-124">You can use a backup policy to schedule backups.</span></span> <span data-ttu-id="b3bba-125">Másik lehetőségként használhatja a **biztonsági másolatok készítéséhez** beállítás manuális biztonsági mentés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="b3bba-125">Alternatively, you can use the **Take backup** option to take a manual backup.</span></span>

## <a name="list-backup-sets-for-a-backup-policy"></a><span data-ttu-id="b3bba-126">Lista biztonságimásolat-készletet a biztonsági mentési házirend</span><span class="sxs-lookup"><span data-stu-id="b3bba-126">List backup sets for a backup policy</span></span>
<span data-ttu-id="b3bba-127">Az alábbi lépésekkel egy biztonsági mentési házirend biztonsági másolatok felsorolása.</span><span class="sxs-lookup"><span data-stu-id="b3bba-127">Complete the following steps to list all the backups for a backup policy.</span></span>

#### <a name="to-list-backup-sets"></a><span data-ttu-id="b3bba-128">A lista biztonságimásolat-készletek</span><span class="sxs-lookup"><span data-stu-id="b3bba-128">To list backup sets</span></span>
1. <span data-ttu-id="b3bba-129">Nyissa meg a StorSimple Device Manager szolgáltatást, és kattintson a **biztonságimásolat-katalógus**.</span><span class="sxs-lookup"><span data-stu-id="b3bba-129">Go to your StorSimple Device Manager service and click **Backup catalog**.</span></span>

2. <span data-ttu-id="b3bba-130">Az alábbiak szerint szűrheti a lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="b3bba-130">Filter the selections as follows:</span></span>
   
   1. <span data-ttu-id="b3bba-131">Adja meg azt az időtartományt.</span><span class="sxs-lookup"><span data-stu-id="b3bba-131">Specify the time range.</span></span>
   2. <span data-ttu-id="b3bba-132">Válassza ki a megfelelő eszközt.</span><span class="sxs-lookup"><span data-stu-id="b3bba-132">Select the appropriate device.</span></span>
   3. <span data-ttu-id="b3bba-133">Szűrés **biztonsági mentési házirend** megtekintéséhez a megfelelő biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="b3bba-133">Filter by **Backup policy** to view the corresponding the backups.</span></span>
   3. <span data-ttu-id="b3bba-134">A biztonsági mentési házirend legördülő listából válassza ki a **összes** kattintva megtekintheti az összes biztonsági mentés a kiválasztott eszköz.</span><span class="sxs-lookup"><span data-stu-id="b3bba-134">From the backup policy dropdown list, choose **All** to view all the backups on the selected device.</span></span>
   4. <span data-ttu-id="b3bba-135">Kattintson a **alkalmaz** a lekérdezés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="b3bba-135">Click **Apply** to execute this query.</span></span>
      
      <span data-ttu-id="b3bba-136">A kijelölt biztonsági mentési házirend társított biztonsági másolatok meg kell jelennie a biztonságimásolat-készletek listájában.</span><span class="sxs-lookup"><span data-stu-id="b3bba-136">The backups associated with the selected backup policy should appear in the list of backup sets.</span></span>

      ![Ugrás a biztonságimásolat-katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a><span data-ttu-id="b3bba-138">Válassza ki a biztonságimásolat-készletből</span><span class="sxs-lookup"><span data-stu-id="b3bba-138">Select a backup set</span></span>
<span data-ttu-id="b3bba-139">Az alábbi lépésekkel válassza ki a biztonsági másolatot egy kötet vagy a biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="b3bba-139">Complete the following steps to select a backup set for a volume or backup policy.</span></span>

#### <a name="to-select-a-backup-set"></a><span data-ttu-id="b3bba-140">Jelölje be a biztonságimásolat-készletből</span><span class="sxs-lookup"><span data-stu-id="b3bba-140">To select a backup set</span></span>
1. <span data-ttu-id="b3bba-141">Nyissa meg a StorSimple Device Manager szolgáltatást, és kattintson a **biztonságimásolat-katalógus**.</span><span class="sxs-lookup"><span data-stu-id="b3bba-141">Go to your StorSimple Device Manager service and click **Backup catalog**.</span></span>
2. <span data-ttu-id="b3bba-142">Az alábbiak szerint szűrheti a lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="b3bba-142">Filter the selections as follows:</span></span>
   
   1. <span data-ttu-id="b3bba-143">Adja meg azt az időtartományt.</span><span class="sxs-lookup"><span data-stu-id="b3bba-143">Specify the time range.</span></span> 
   2. <span data-ttu-id="b3bba-144">Válassza ki a megfelelő eszközt.</span><span class="sxs-lookup"><span data-stu-id="b3bba-144">Select the appropriate device.</span></span> 
   3. <span data-ttu-id="b3bba-145">Szűrés a biztonsági mentés, válassza ki szeretne kötet vagy a biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="b3bba-145">Filter by volume or backup policy for the backup that you wish to select.</span></span>
   4. <span data-ttu-id="b3bba-146">Kattintson a **alkalmaz** a lekérdezés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="b3bba-146">Click **Apply** to execute this query.</span></span>
      
      <span data-ttu-id="b3bba-147">A biztonsági mentések társított a kijelölt kötetről, vagy a biztonsági mentési házirend megjelenjen-e a biztonságimásolat-készletek listájában.</span><span class="sxs-lookup"><span data-stu-id="b3bba-147">The backups associated with the selected volume or backup policy should appear in the list of backup sets.</span></span>

      ![Ugrás a biztonságimásolat-katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. <span data-ttu-id="b3bba-149">Válassza ki, és bontsa ki a biztonságimásolat-készletből.</span><span class="sxs-lookup"><span data-stu-id="b3bba-149">Select and expand a backup set.</span></span> <span data-ttu-id="b3bba-150">Most már megtekintheti a biztonsági mentés, a benne található kötetek szerinti bontásban.</span><span class="sxs-lookup"><span data-stu-id="b3bba-150">You can now see the backup sets broken down by the volumes that it contains.</span></span> <span data-ttu-id="b3bba-151">A **visszaállítása** és **törlése** beállítások érhetők el a helyi menüt (kattintson a jobb gombbal) a biztonsági másolat készletéhez.</span><span class="sxs-lookup"><span data-stu-id="b3bba-151">The **Restore** and **Delete** options are available via the context menu (right-click) for the backup set.</span></span> <span data-ttu-id="b3bba-152">Ezek a műveletek egyikét a biztonságimásolat-készlet, verziószámával a hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="b3bba-152">You can perform either of these actions on the backup set that you selected.</span></span>

    ![Ugrás a biztonságimásolat-katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a><span data-ttu-id="b3bba-154">A biztonságimásolat-készlet törlése</span><span class="sxs-lookup"><span data-stu-id="b3bba-154">Delete a backup set</span></span>
<span data-ttu-id="b3bba-155">Törli az egy biztonsági mentés, ha már nem szeretne a vele társított adatok megőrzése mellett.</span><span class="sxs-lookup"><span data-stu-id="b3bba-155">Delete a backup when you no longer wish to retain the data associated with it.</span></span> <span data-ttu-id="b3bba-156">A következő lépésekkel egy biztonságimásolat-készlet törlése.</span><span class="sxs-lookup"><span data-stu-id="b3bba-156">Perform the following steps to delete a backup set.</span></span>

#### <a name="to-delete-a-backup-set"></a><span data-ttu-id="b3bba-157">A biztonságimásolat-készlet törlése</span><span class="sxs-lookup"><span data-stu-id="b3bba-157">To delete a backup set</span></span>
 <span data-ttu-id="b3bba-158">Nyissa meg a StorSimple Device Manager szolgáltatást, és kattintson a **biztonságimásolat-katalógus**.</span><span class="sxs-lookup"><span data-stu-id="b3bba-158">Go to your StorSimple Device Manager service and click **Backup catalog**.</span></span>
2. <span data-ttu-id="b3bba-159">Az alábbiak szerint szűrheti a lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="b3bba-159">Filter the selections as follows:</span></span>
   
   1. <span data-ttu-id="b3bba-160">Adja meg azt az időtartományt.</span><span class="sxs-lookup"><span data-stu-id="b3bba-160">Specify the time range.</span></span> 
   2. <span data-ttu-id="b3bba-161">Válassza ki a megfelelő eszközt.</span><span class="sxs-lookup"><span data-stu-id="b3bba-161">Select the appropriate device.</span></span> 
   3. <span data-ttu-id="b3bba-162">Szűrés a biztonsági mentés, válassza ki szeretne kötet vagy a biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="b3bba-162">Filter by volume or backup policy for the backup that you wish to select.</span></span>
   4. <span data-ttu-id="b3bba-163">Kattintson a **alkalmaz** a lekérdezés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="b3bba-163">Click **Apply** to execute this query.</span></span>
      
      <span data-ttu-id="b3bba-164">A biztonsági mentések társított a kijelölt kötetről, vagy a biztonsági mentési házirend megjelenjen-e a biztonságimásolat-készletek listájában.</span><span class="sxs-lookup"><span data-stu-id="b3bba-164">The backups associated with the selected volume or backup policy should appear in the list of backup sets.</span></span>

      ![Ugrás a biztonságimásolat-katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. <span data-ttu-id="b3bba-166">Válassza ki, és bontsa ki a biztonságimásolat-készletből.</span><span class="sxs-lookup"><span data-stu-id="b3bba-166">Select and expand a backup set.</span></span> <span data-ttu-id="b3bba-167">Most már megtekintheti a biztonsági mentés, a benne található kötetek szerinti bontásban.</span><span class="sxs-lookup"><span data-stu-id="b3bba-167">You can now see the backup sets broken down by the volumes that it contains.</span></span> <span data-ttu-id="b3bba-168">A **visszaállítása** és **törlése** beállítások érhetők el a helyi menüt (kattintson a jobb gombbal) a biztonsági másolat készletéhez.</span><span class="sxs-lookup"><span data-stu-id="b3bba-168">The **Restore** and **Delete** options are available via the context menu (right-click) for the backup set.</span></span> <span data-ttu-id="b3bba-169">Kattintson a jobb gombbal a kijelölt biztonságimásolat-készlet és a helyi menüben válassza a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="b3bba-169">Right-click the selected backup set and from the context menu, select **Delete**.</span></span>

    ![Ugrás a biztonságimásolat-katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

4. <span data-ttu-id="b3bba-171">Amikor felszólítja a megerősítésre, tekintse át a megjelenített információkat, majd kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="b3bba-171">When prompted for confirmation, review the displayed information and click **Delete**.</span></span> <span data-ttu-id="b3bba-172">A kijelölt biztonsági véglegesen törlődik.</span><span class="sxs-lookup"><span data-stu-id="b3bba-172">The selected backup is deleted permanently.</span></span>

    ![Ugrás a biztonságimásolat-katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

5. <span data-ttu-id="b3bba-174">Meg fog értesítést kapni a Törlés folyamatban van, és ha sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b3bba-174">You will be notified when the deletion is in progress and when it has successfully finished.</span></span> <span data-ttu-id="b3bba-175">A törlés után frissítse a lekérdezést. ezen a lapon.</span><span class="sxs-lookup"><span data-stu-id="b3bba-175">After the deletion is done, refresh the query on this page.</span></span> <span data-ttu-id="b3bba-176">A törölt biztonságimásolat-készlet nem fog megjelenni a biztonságimásolat-készletek listájában.</span><span class="sxs-lookup"><span data-stu-id="b3bba-176">The deleted backup set will no longer appear in the list of backup sets.</span></span>

    ![Ugrás a biztonságimásolat-katalógus](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a><span data-ttu-id="b3bba-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b3bba-178">Next steps</span></span>
* <span data-ttu-id="b3bba-179">Megtudhatja, hogyan [a biztonságimásolat-katalógus használatával állítsa vissza az eszköz biztonságimásolat-készletből](storsimple-8000-restore-from-backup-set-u2.md).</span><span class="sxs-lookup"><span data-stu-id="b3bba-179">Learn how to [use the backup catalog to restore your device from a backup set](storsimple-8000-restore-from-backup-set-u2.md).</span></span>
* <span data-ttu-id="b3bba-180">Megtudhatja, hogyan [felügyelete a StorSimple eszközt a StorSimple Device Manager szolgáltatással](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="b3bba-180">Learn how to [use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

