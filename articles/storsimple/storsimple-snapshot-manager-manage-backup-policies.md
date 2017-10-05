---
title: "StorSimple Snapshot Manager biztonsági mentési házirendek |} Microsoft Docs"
description: "Ismerteti a StorSimple Snapshot Manager MMC beépülő modul használatával hozzon létre, és a biztonsági mentési házirend ütemezett biztonsági mentést vezérlő kezeléséhez."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 04415d0b-42f0-4737-8afa-257fb2dbe5d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 218c89e403673c16c72da95aa2c1d685bbed5a86
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a><span data-ttu-id="8a103-103">StorSimple Snapshot Manager segítségével biztonsági szabályzatok létrehozása és kezelése</span><span class="sxs-lookup"><span data-stu-id="8a103-103">Use StorSimple Snapshot Manager to create and manage backup policies</span></span>
## <a name="overview"></a><span data-ttu-id="8a103-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8a103-104">Overview</span></span>
<span data-ttu-id="8a103-105">A biztonsági mentési házirend hoz létre egy ütemezést az adatok biztonsági mentése kötet helyileg vagy a felhőben.</span><span class="sxs-lookup"><span data-stu-id="8a103-105">A backup policy creates a schedule for backing up volume data locally or in the cloud.</span></span> <span data-ttu-id="8a103-106">A biztonsági mentési házirend létrehozásakor adja meg egy megőrzési házirend.</span><span class="sxs-lookup"><span data-stu-id="8a103-106">When you create a backup policy, you can also specify a retention policy.</span></span> <span data-ttu-id="8a103-107">(Legfeljebb 64 pillanatképek őrizheti). További információ a biztonsági mentési házirendek: [biztonsági mentési típusok](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) a [StorSimple 8000 series: hibrid felhőalapú megoldás](storsimple-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8a103-107">(You can retain a maximum of 64 snapshots.) For more information about backup policies, see [Backup types](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) in [StorSimple 8000 series: a hybrid cloud solution](storsimple-overview.md).</span></span>

<span data-ttu-id="8a103-108">Ez az oktatóanyag azt ismerteti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="8a103-108">This tutorial explains how to:</span></span>

* <span data-ttu-id="8a103-109">A biztonsági mentési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="8a103-109">Create a backup policy</span></span>
* <span data-ttu-id="8a103-110">A biztonsági mentési házirend szerkesztése</span><span class="sxs-lookup"><span data-stu-id="8a103-110">Edit a backup policy</span></span>
* <span data-ttu-id="8a103-111">A biztonsági mentési házirend törlése</span><span class="sxs-lookup"><span data-stu-id="8a103-111">Delete a backup policy</span></span>

## <a name="create-a-backup-policy"></a><span data-ttu-id="8a103-112">A biztonsági mentési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="8a103-112">Create a backup policy</span></span>
<span data-ttu-id="8a103-113">A következő eljárással hozhat létre egy új biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="8a103-113">Use the following procedure to create a new backup policy.</span></span>

#### <a name="to-create-a-backup-policy"></a><span data-ttu-id="8a103-114">A biztonsági mentési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="8a103-114">To create a backup policy</span></span>
1. <span data-ttu-id="8a103-115">Kattintson az asztal ikonra kattintva indítsa el a StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="8a103-115">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="8a103-116">Az a **hatókör** ablaktáblán kattintson a jobb gombbal **biztonsági mentési házirendek**, és kattintson a **biztonsági mentési házirend létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8a103-116">In the **Scope** pane, right-click **Backup Policies**, and click **Create Backup Policy**.</span></span>

    ![A biztonsági mentési házirend létrehozása](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    <span data-ttu-id="8a103-118">A **házirend létrehozása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8a103-118">The **Create a Policy** dialog box appears.</span></span>

    ![Hozzon létre egy házirend – Általános lap](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. <span data-ttu-id="8a103-120">Az a **általános** lapra, adja meg a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="8a103-120">On the **General** tab, complete the following information:</span></span>

   1. <span data-ttu-id="8a103-121">Az a **neve** szövegmezőbe írja be a házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="8a103-121">In the **Name** text box, type a name for the policy.</span></span>
   2. <span data-ttu-id="8a103-122">Az a **kötet csoport** mezőbe, írja be a kötet csoport nevét a házirendhez társított.</span><span class="sxs-lookup"><span data-stu-id="8a103-122">In the **Volume group** text box, type the name of the volume group associated with the policy.</span></span>
   3. <span data-ttu-id="8a103-123">Válassza ki vagy **helyi pillanatfelvétel** vagy **felhőalapú pillanatfelvétel**.</span><span class="sxs-lookup"><span data-stu-id="8a103-123">Select either **Local Snapshot** or **Cloud Snapshot**.</span></span>
   4. <span data-ttu-id="8a103-124">Válassza ki a megőrizni kívánt pillanatképek számát.</span><span class="sxs-lookup"><span data-stu-id="8a103-124">Select the number of snapshots to retain.</span></span> <span data-ttu-id="8a103-125">Ha **minden**, 64 pillanatképek lesz (a maximális) maradnak.</span><span class="sxs-lookup"><span data-stu-id="8a103-125">If you select **All**, 64 snapshots will be retained (the maximum).</span></span>
4. <span data-ttu-id="8a103-126">Kattintson a **ütemezés** fülre.</span><span class="sxs-lookup"><span data-stu-id="8a103-126">Click the **Schedule** tab.</span></span>

    ![Hozzon létre egy házirend - ütemezés lapon](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. <span data-ttu-id="8a103-128">Az a **ütemezés** lapra, adja meg a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="8a103-128">On the **Schedule** tab, complete the following information:</span></span>

   1. <span data-ttu-id="8a103-129">Kattintson a **engedélyezése** melletti jelölőnégyzetet, hogy a következő biztonsági mentés ütemezése.</span><span class="sxs-lookup"><span data-stu-id="8a103-129">Click the **Enable** check box to schedule the next backup.</span></span>
   2. <span data-ttu-id="8a103-130">A **beállítások**, jelölje be **egyszer**, **napi**, **heti**, vagy **havi**.</span><span class="sxs-lookup"><span data-stu-id="8a103-130">Under **Settings**, select **One time**, **Daily**, **Weekly**, or **Monthly**.</span></span>
   3. <span data-ttu-id="8a103-131">Az a **Start** szövegmezőben kattintson a naptár ikonra, és válassza ki a kezdési dátum.</span><span class="sxs-lookup"><span data-stu-id="8a103-131">In the **Start** text box, click the calendar icon and select a start date.</span></span>
   4. <span data-ttu-id="8a103-132">A **speciális beállítások**, beállíthatja a választható ismétlődő ütemezés és a befejező dátumát.</span><span class="sxs-lookup"><span data-stu-id="8a103-132">Under **Advanced Settings**, you can set optional repeat schedules and an end date.</span></span>
   5. <span data-ttu-id="8a103-133">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8a103-133">Click **OK**.</span></span>

<span data-ttu-id="8a103-134">Miután létrehozta a biztonsági mentési házirend, az alábbi információk megjelennek a **eredmények** panelen:</span><span class="sxs-lookup"><span data-stu-id="8a103-134">After you create a backup policy, the following information appears in the **Results** pane:</span></span>

* <span data-ttu-id="8a103-135">**Név** – a biztonsági mentési házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="8a103-135">**Name** – the name of backup policy.</span></span>
* <span data-ttu-id="8a103-136">**Típus** – helyi pillanatfelvétel és felhőbeli pillanatfelvétel.</span><span class="sxs-lookup"><span data-stu-id="8a103-136">**Type** – local snapshot or cloud snapshot.</span></span>
* <span data-ttu-id="8a103-137">**Kötet csoport** – a kötet csoport, a házirendhez társított.</span><span class="sxs-lookup"><span data-stu-id="8a103-137">**Volume Group** – the volume group associated with the policy.</span></span>
* <span data-ttu-id="8a103-138">**Megőrzési** – számú pillanatkép maradnak; a maximális 64.</span><span class="sxs-lookup"><span data-stu-id="8a103-138">**Retention** – the number of snapshots retained; the maximum is 64.</span></span>
* <span data-ttu-id="8a103-139">**Létrehozott** – Ez a házirend létrehozásának dátumát.</span><span class="sxs-lookup"><span data-stu-id="8a103-139">**Created** – the date that this policy was created.</span></span>
* <span data-ttu-id="8a103-140">**Engedélyezett** – hogy a házirend van érvényben: **igaz** azt mutatja, hogy érvényes; **Hamis** azt mutatja, hogy nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="8a103-140">**Enabled** – whether the policy is currently in effect: **True** indicates that it is in effect; **False** indicates that it is not in effect.</span></span>

## <a name="edit-a-backup-policy"></a><span data-ttu-id="8a103-141">A biztonsági mentési házirend szerkesztése</span><span class="sxs-lookup"><span data-stu-id="8a103-141">Edit a backup policy</span></span>
<span data-ttu-id="8a103-142">A következő eljárással szerkesztheti a meglévő biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="8a103-142">Use the following procedure to edit an existing backup policy.</span></span>

#### <a name="to-edit-a-backup-policy"></a><span data-ttu-id="8a103-143">A biztonsági mentési házirend szerkesztése</span><span class="sxs-lookup"><span data-stu-id="8a103-143">To edit a backup policy</span></span>
1. <span data-ttu-id="8a103-144">Kattintson az asztal ikonra kattintva indítsa el a StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="8a103-144">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="8a103-145">Az a **hatókör** ablaktáblán kattintson a **biztonsági mentési házirendek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="8a103-145">In the **Scope** pane, click the **Backup Policies** node.</span></span> <span data-ttu-id="8a103-146">A biztonsági mentési házirendek megjelennek a **eredmények** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="8a103-146">All the backup policies appear in the **Results** pane.</span></span>
3. <span data-ttu-id="8a103-147">Kattintson a jobb gombbal a házirend szerkesztése, és kattintson a kívánt **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="8a103-147">Right-click the policy that you want to edit, and then click **Edit**.</span></span>

    ![A biztonsági mentési házirend szerkesztése](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. <span data-ttu-id="8a103-149">Ha a **házirend létrehozása** ablak megjelenik, írja be a módosításokat, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="8a103-149">When the **Create a Policy** window appears, enter your changes, and then click **OK**.</span></span>

## <a name="delete-a-backup-policy"></a><span data-ttu-id="8a103-150">A biztonsági mentési házirend törlése</span><span class="sxs-lookup"><span data-stu-id="8a103-150">Delete a backup policy</span></span>
<span data-ttu-id="8a103-151">Az alábbi eljárás segítségével törölheti a biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="8a103-151">Use the following procedure to delete a backup policy.</span></span>

#### <a name="to-delete-a-backup-policy"></a><span data-ttu-id="8a103-152">A biztonsági mentési házirend törlése</span><span class="sxs-lookup"><span data-stu-id="8a103-152">To delete a backup policy</span></span>
1. <span data-ttu-id="8a103-153">Kattintson az asztal ikonra kattintva indítsa el a StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="8a103-153">Click the desktop icon to start StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="8a103-154">Az a **hatókör** ablaktáblán kattintson a **biztonsági mentési házirendek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="8a103-154">In the **Scope** pane, click the **Backup Policies** node.</span></span> <span data-ttu-id="8a103-155">A biztonsági mentési házirendek megjelennek a **eredmények** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="8a103-155">All the backup policies appear in the **Results** pane.</span></span>
3. <span data-ttu-id="8a103-156">Kattintson a jobb gombbal a biztonsági mentési házirend, törlése, és kattintson a kívánt **törlése**.</span><span class="sxs-lookup"><span data-stu-id="8a103-156">Right-click the backup policy that you want to delete, and then click **Delete**.</span></span>
4. <span data-ttu-id="8a103-157">A jóváhagyást kérő üzenet megjelenésekor kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="8a103-157">When the confirmation message appears, click **Yes**.</span></span>

    ![A biztonsági mentési házirend megerősítés törlése](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a><span data-ttu-id="8a103-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8a103-159">Next steps</span></span>
* <span data-ttu-id="8a103-160">Megtudhatja, hogyan [StorSimple Snapshot Manager segítségével felügyelheti a StorSimple megoldásban](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="8a103-160">Learn how to [use StorSimple Snapshot Manager to administer your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="8a103-161">Megtudhatja, hogyan [StorSimple Snapshot Manager segítségével megtekintheti és kezelheti a biztonsági mentési feladatok](storsimple-snapshot-manager-manage-backup-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="8a103-161">Learn how to [use StorSimple Snapshot Manager to view and manage backup jobs](storsimple-snapshot-manager-manage-backup-jobs.md).</span></span>
