---
title: "aaaStorSimple Snapshot Manager biztonsági mentési házirendek |} Microsoft Docs"
description: "Útmutatás a toouse StorSimple Snapshot Manager MMC beépülő modul toocreate hello és hello biztonsági mentési házirend ütemezett biztonsági mentések szabályozó kezeléséhez."
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
ms.openlocfilehash: c2ae75a8d0568090add6018da18de73eb56e6590
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toocreate-and-manage-backup-policies"></a><span data-ttu-id="e4914-103">StorSimple Snapshot Manager toocreate használatát és kezelését a biztonsági mentési házirendek</span><span class="sxs-lookup"><span data-stu-id="e4914-103">Use StorSimple Snapshot Manager toocreate and manage backup policies</span></span>
## <a name="overview"></a><span data-ttu-id="e4914-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e4914-104">Overview</span></span>
<span data-ttu-id="e4914-105">A biztonsági mentési házirend hoz létre egy ütemezést az adatok biztonsági mentése kötet helyileg vagy hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="e4914-105">A backup policy creates a schedule for backing up volume data locally or in hello cloud.</span></span> <span data-ttu-id="e4914-106">A biztonsági mentési házirend létrehozásakor adja meg egy megőrzési házirend.</span><span class="sxs-lookup"><span data-stu-id="e4914-106">When you create a backup policy, you can also specify a retention policy.</span></span> <span data-ttu-id="e4914-107">(Legfeljebb 64 pillanatképek őrizheti). További információ a biztonsági mentési házirendek: [biztonsági mentési típusok](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) a [StorSimple 8000 series: hibrid felhőalapú megoldás](storsimple-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e4914-107">(You can retain a maximum of 64 snapshots.) For more information about backup policies, see [Backup types](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) in [StorSimple 8000 series: a hybrid cloud solution](storsimple-overview.md).</span></span>

<span data-ttu-id="e4914-108">Ez az oktatóanyag azt ismerteti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="e4914-108">This tutorial explains how to:</span></span>

* <span data-ttu-id="e4914-109">A biztonsági mentési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4914-109">Create a backup policy</span></span>
* <span data-ttu-id="e4914-110">A biztonsági mentési házirend szerkesztése</span><span class="sxs-lookup"><span data-stu-id="e4914-110">Edit a backup policy</span></span>
* <span data-ttu-id="e4914-111">A biztonsági mentési házirend törlése</span><span class="sxs-lookup"><span data-stu-id="e4914-111">Delete a backup policy</span></span>

## <a name="create-a-backup-policy"></a><span data-ttu-id="e4914-112">A biztonsági mentési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="e4914-112">Create a backup policy</span></span>
<span data-ttu-id="e4914-113">A következő eljárás toocreate egy új biztonsági mentési házirend hello használata.</span><span class="sxs-lookup"><span data-stu-id="e4914-113">Use hello following procedure toocreate a new backup policy.</span></span>

#### <a name="toocreate-a-backup-policy"></a><span data-ttu-id="e4914-114">a biztonsági mentési házirend toocreate</span><span class="sxs-lookup"><span data-stu-id="e4914-114">toocreate a backup policy</span></span>
1. <span data-ttu-id="e4914-115">Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="e4914-115">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="e4914-116">A hello **hatókör** ablaktáblán kattintson a jobb gombbal **biztonsági mentési házirendek**, és kattintson a **biztonsági mentési házirend létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="e4914-116">In hello **Scope** pane, right-click **Backup Policies**, and click **Create Backup Policy**.</span></span>

    ![A biztonsági mentési házirend létrehozása](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    <span data-ttu-id="e4914-118">Hello **házirend létrehozása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e4914-118">hello **Create a Policy** dialog box appears.</span></span>

    ![Hozzon létre egy házirend – Általános lap](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. <span data-ttu-id="e4914-120">A hello **általános** lap, a következő információ teljes hello:</span><span class="sxs-lookup"><span data-stu-id="e4914-120">On hello **General** tab, complete hello following information:</span></span>

   1. <span data-ttu-id="e4914-121">A hello **neve** szövegmezőbe írja be a hello házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="e4914-121">In hello **Name** text box, type a name for hello policy.</span></span>
   2. <span data-ttu-id="e4914-122">A hello **kötet csoport** szövegmezőben, hello kötet csoport hello házirendhez társított hello nevét.</span><span class="sxs-lookup"><span data-stu-id="e4914-122">In hello **Volume group** text box, type hello name of hello volume group associated with hello policy.</span></span>
   3. <span data-ttu-id="e4914-123">Válassza ki vagy **helyi pillanatfelvétel** vagy **felhőalapú pillanatfelvétel**.</span><span class="sxs-lookup"><span data-stu-id="e4914-123">Select either **Local Snapshot** or **Cloud Snapshot**.</span></span>
   4. <span data-ttu-id="e4914-124">Válassza ki a pillanatképek tooretain hello számát.</span><span class="sxs-lookup"><span data-stu-id="e4914-124">Select hello number of snapshots tooretain.</span></span> <span data-ttu-id="e4914-125">Ha **összes**, 64 pillanatfelvételek megmaradnak (hello maximális).</span><span class="sxs-lookup"><span data-stu-id="e4914-125">If you select **All**, 64 snapshots will be retained (hello maximum).</span></span>
4. <span data-ttu-id="e4914-126">Kattintson a hello **ütemezés** fülre.</span><span class="sxs-lookup"><span data-stu-id="e4914-126">Click hello **Schedule** tab.</span></span>

    ![Hozzon létre egy házirend - ütemezés lapon](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. <span data-ttu-id="e4914-128">A hello **ütemezés** lap, a következő információ teljes hello:</span><span class="sxs-lookup"><span data-stu-id="e4914-128">On hello **Schedule** tab, complete hello following information:</span></span>

   1. <span data-ttu-id="e4914-129">Kattintson a hello **engedélyezése** jelölőnégyzetet tooschedule hello következő biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="e4914-129">Click hello **Enable** check box tooschedule hello next backup.</span></span>
   2. <span data-ttu-id="e4914-130">A **beállítások**, jelölje be **egyszer**, **napi**, **heti**, vagy **havi**.</span><span class="sxs-lookup"><span data-stu-id="e4914-130">Under **Settings**, select **One time**, **Daily**, **Weekly**, or **Monthly**.</span></span>
   3. <span data-ttu-id="e4914-131">A hello **Start** szövegmezőben kattintson hello naptár ikonra, és válassza ki a kezdési dátum.</span><span class="sxs-lookup"><span data-stu-id="e4914-131">In hello **Start** text box, click hello calendar icon and select a start date.</span></span>
   4. <span data-ttu-id="e4914-132">A **speciális beállítások**, beállíthatja a választható ismétlődő ütemezés és a befejező dátumát.</span><span class="sxs-lookup"><span data-stu-id="e4914-132">Under **Advanced Settings**, you can set optional repeat schedules and an end date.</span></span>
   5. <span data-ttu-id="e4914-133">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e4914-133">Click **OK**.</span></span>

<span data-ttu-id="e4914-134">Miután létrehozta a biztonsági mentési házirend, a következő információk hello megjelenik hello **eredmények** panelen:</span><span class="sxs-lookup"><span data-stu-id="e4914-134">After you create a backup policy, hello following information appears in hello **Results** pane:</span></span>

* <span data-ttu-id="e4914-135">**Név** – hello biztonsági mentési házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="e4914-135">**Name** – hello name of backup policy.</span></span>
* <span data-ttu-id="e4914-136">**Típus** – helyi pillanatfelvétel és felhőbeli pillanatfelvétel.</span><span class="sxs-lookup"><span data-stu-id="e4914-136">**Type** – local snapshot or cloud snapshot.</span></span>
* <span data-ttu-id="e4914-137">**Kötet csoport** – hello hello házirendhez társított kötet csoport.</span><span class="sxs-lookup"><span data-stu-id="e4914-137">**Volume Group** – hello volume group associated with hello policy.</span></span>
* <span data-ttu-id="e4914-138">**Megőrzési** – hello pillanatképek számát maradnak; hello maximális 64.</span><span class="sxs-lookup"><span data-stu-id="e4914-138">**Retention** – hello number of snapshots retained; hello maximum is 64.</span></span>
* <span data-ttu-id="e4914-139">**Létrehozott** – Ez a házirend létrehozásának hello dátuma.</span><span class="sxs-lookup"><span data-stu-id="e4914-139">**Created** – hello date that this policy was created.</span></span>
* <span data-ttu-id="e4914-140">**Engedélyezett** – hogy hello házirend van érvényben: **igaz** azt mutatja, hogy érvényes; **Hamis** azt mutatja, hogy nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="e4914-140">**Enabled** – whether hello policy is currently in effect: **True** indicates that it is in effect; **False** indicates that it is not in effect.</span></span>

## <a name="edit-a-backup-policy"></a><span data-ttu-id="e4914-141">A biztonsági mentési házirend szerkesztése</span><span class="sxs-lookup"><span data-stu-id="e4914-141">Edit a backup policy</span></span>
<span data-ttu-id="e4914-142">A következő eljárás tooedit egy meglévő biztonsági mentési házirend hello használata.</span><span class="sxs-lookup"><span data-stu-id="e4914-142">Use hello following procedure tooedit an existing backup policy.</span></span>

#### <a name="tooedit-a-backup-policy"></a><span data-ttu-id="e4914-143">a biztonsági mentési házirend tooedit</span><span class="sxs-lookup"><span data-stu-id="e4914-143">tooedit a backup policy</span></span>
1. <span data-ttu-id="e4914-144">Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="e4914-144">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="e4914-145">A hello **hatókör** ablaktáblán kattintson a hello **biztonsági mentési házirendek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="e4914-145">In hello **Scope** pane, click hello **Backup Policies** node.</span></span> <span data-ttu-id="e4914-146">Minden hello biztonsági mentési házirendek jelennek-e hello **eredmények** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="e4914-146">All hello backup policies appear in hello **Results** pane.</span></span>
3. <span data-ttu-id="e4914-147">Kattintson a jobb gombbal a kívánt tooedit, és kattintson hello házirend **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="e4914-147">Right-click hello policy that you want tooedit, and then click **Edit**.</span></span>

    ![A biztonsági mentési házirend szerkesztése](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. <span data-ttu-id="e4914-149">Ha hello **házirend létrehozása** ablak megjelenik, írja be a módosításokat, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4914-149">When hello **Create a Policy** window appears, enter your changes, and then click **OK**.</span></span>

## <a name="delete-a-backup-policy"></a><span data-ttu-id="e4914-150">A biztonsági mentési házirend törlése</span><span class="sxs-lookup"><span data-stu-id="e4914-150">Delete a backup policy</span></span>
<span data-ttu-id="e4914-151">A következő eljárás toodelete a biztonsági mentési házirend hello használata.</span><span class="sxs-lookup"><span data-stu-id="e4914-151">Use hello following procedure toodelete a backup policy.</span></span>

#### <a name="toodelete-a-backup-policy"></a><span data-ttu-id="e4914-152">a biztonsági mentési házirend toodelete</span><span class="sxs-lookup"><span data-stu-id="e4914-152">toodelete a backup policy</span></span>
1. <span data-ttu-id="e4914-153">Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="e4914-153">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="e4914-154">A hello **hatókör** ablaktáblán kattintson a hello **biztonsági mentési házirendek** csomópont.</span><span class="sxs-lookup"><span data-stu-id="e4914-154">In hello **Scope** pane, click hello **Backup Policies** node.</span></span> <span data-ttu-id="e4914-155">Minden hello biztonsági mentési házirendek jelennek-e hello **eredmények** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="e4914-155">All hello backup policies appear in hello **Results** pane.</span></span>
3. <span data-ttu-id="e4914-156">Kattintson a jobb gombbal a biztonsági mentési házirend hello toodelete szeretne, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="e4914-156">Right-click hello backup policy that you want toodelete, and then click **Delete**.</span></span>
4. <span data-ttu-id="e4914-157">Hello jóváhagyást kérő üzenet megjelenésekor kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="e4914-157">When hello confirmation message appears, click **Yes**.</span></span>

    ![A biztonsági mentési házirend megerősítés törlése](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a><span data-ttu-id="e4914-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4914-159">Next steps</span></span>
* <span data-ttu-id="e4914-160">Ismerje meg, hogyan túl[használja a StorSimple Snapshot Manager tooadminister a StorSimple megoldásban](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="e4914-160">Learn how too[use StorSimple Snapshot Manager tooadminister your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="e4914-161">Ismerje meg, hogyan túl[StorSimple Snapshot Manager tooview használatát és kezelését a biztonsági mentési feladatok](storsimple-snapshot-manager-manage-backup-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="e4914-161">Learn how too[use StorSimple Snapshot Manager tooview and manage backup jobs](storsimple-snapshot-manager-manage-backup-jobs.md).</span></span>
