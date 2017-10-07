---
title: "aaaStorSimple Snapshot Manager kötet csoportok |} Microsoft Docs"
description: "Útmutatás a toouse StorSimple Snapshot Manager MMC beépülő modul toocreate hello és kötet csoportok kezelése."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 7a232414-6a28-4b81-bd7b-cf61e28b33d7
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: f7830b62761db7aa5139456b555b6168fb6a85de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toocreate-and-manage-volume-groups"></a><span data-ttu-id="ba37f-103">StorSimple Snapshot Manager toocreate használja, és a kötet csoportok kezelése</span><span class="sxs-lookup"><span data-stu-id="ba37f-103">Use StorSimple Snapshot Manager toocreate and manage volume groups</span></span>
## <a name="overview"></a><span data-ttu-id="ba37f-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ba37f-104">Overview</span></span>
<span data-ttu-id="ba37f-105">Használhatja a hello **kötet csoportok** hello csomópont **hatókör** mentéseket ablaktábla tooassign kötetek toovolume csoportok, egy kötet csoport adatainak megtekintése és a kötet csoportok szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="ba37f-105">You can use hello **Volume Groups** node on hello **Scope** pane tooassign volumes toovolume groups, view information about a volume group, schedule backups, and edit volume groups.</span></span>

<span data-ttu-id="ba37f-106">Kötet csoportok azonban a kapcsolódó kötetek használt tooensure, hogy a biztonsági mentések alkalmazáskonzisztens.</span><span class="sxs-lookup"><span data-stu-id="ba37f-106">Volume groups are pools of related volumes used tooensure that backups are application-consistent.</span></span> <span data-ttu-id="ba37f-107">További információkért lásd: [kötetek és a kötet csoportok](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) és [integráció a Windows a kötet árnyékmásolata szolgáltatás](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).</span><span class="sxs-lookup"><span data-stu-id="ba37f-107">For more information, see [Volumes and volume groups](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) and [Integration with Windows Volume Shadow Copy Service](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="ba37f-108">Egy kötet csoportban található összes kötetet egy felhőszolgáltató kell származnia.</span><span class="sxs-lookup"><span data-stu-id="ba37f-108">All volumes in a volume group must come from a single cloud service provider.</span></span>
> * <span data-ttu-id="ba37f-109">Kötet csoportok konfigurálásakor nem keverhetők a fürt megosztott kötetei (CSV-k) és a CSV a hello kötet ugyanabban a csoportban.</span><span class="sxs-lookup"><span data-stu-id="ba37f-109">When you configure volume groups, do not mix cluster-shared volumes (CSVs) and non-CSVs in hello same volume group.</span></span> <span data-ttu-id="ba37f-110">StorSimple Snapshot Manager nem támogatja a CSV-k kombinációját, és a nem megosztott hello azonos pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="ba37f-110">StorSimple Snapshot Manager does not support a mix of CSVs and non-CSVs in hello same snapshot.</span></span>

![Kötet csoportok csomópont](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

<span data-ttu-id="ba37f-112">**1. ábra: A StorSimple Snapshot Manager kötet csoportok csomópont**</span><span class="sxs-lookup"><span data-stu-id="ba37f-112">**Figure 1: StorSimple Snapshot Manager Volume Groups node**</span></span> 

<span data-ttu-id="ba37f-113">Ez az oktatóanyag azt ismerteti, hogyan használhatja a StorSimple Snapshot Manager:</span><span class="sxs-lookup"><span data-stu-id="ba37f-113">This tutorial explains how you can use StorSimple Snapshot Manager to:</span></span>

* <span data-ttu-id="ba37f-114">A kötet csoportok adatainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="ba37f-114">View information about your volume groups</span></span>
* <span data-ttu-id="ba37f-115">Kötet csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ba37f-115">Create a volume group</span></span>
* <span data-ttu-id="ba37f-116">Készítsen biztonsági másolatot egy kötet csoport</span><span class="sxs-lookup"><span data-stu-id="ba37f-116">Back up a volume group</span></span>
* <span data-ttu-id="ba37f-117">Kötet csoport szerkesztése</span><span class="sxs-lookup"><span data-stu-id="ba37f-117">Edit a volume group</span></span>
* <span data-ttu-id="ba37f-118">Kötet csoport törlése</span><span class="sxs-lookup"><span data-stu-id="ba37f-118">Delete a volume group</span></span>

<span data-ttu-id="ba37f-119">Ezek a műveletek mindegyikét is elérhetők a hello **műveletek** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="ba37f-119">All of these actions are also available on hello **Actions** pane.</span></span>

## <a name="view-volume-groups"></a><span data-ttu-id="ba37f-120">Kötet csoportok megtekintése</span><span class="sxs-lookup"><span data-stu-id="ba37f-120">View volume groups</span></span>
<span data-ttu-id="ba37f-121">Ha hello **kötet csoportok** csomópont, hello **eredmények** ablaktábla megjeleníti azokat a következő információk minden kötet csoportról hello, attól függően, hogy hello oszlopkiválasztásokat elvégezte.</span><span class="sxs-lookup"><span data-stu-id="ba37f-121">If you click hello **Volume Groups** node, hello **Results** pane shows hello following information about each volume group, depending on hello column selections you make.</span></span> <span data-ttu-id="ba37f-122">(hello oszlopai hello **eredmények** ablaktáblán is konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="ba37f-122">(hello columns in hello **Results** pane are configurable.</span></span> <span data-ttu-id="ba37f-123">Kattintson a jobb gombbal hello **kötetek** csomópontban jelölje ki **nézet**, majd válassza ki **oszlopok hozzáadása/eltávolítása**.)</span><span class="sxs-lookup"><span data-stu-id="ba37f-123">Right-click hello **Volumes** node, select **View**, and then select **Add/Remove Columns**.)</span></span>

| <span data-ttu-id="ba37f-124">Eredmények oszlop</span><span class="sxs-lookup"><span data-stu-id="ba37f-124">Results column</span></span> | <span data-ttu-id="ba37f-125">Leírás</span><span class="sxs-lookup"><span data-stu-id="ba37f-125">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="ba37f-126">Név</span><span class="sxs-lookup"><span data-stu-id="ba37f-126">Name</span></span> |<span data-ttu-id="ba37f-127">Hello **neve** oszlop hello kötet csoport hello nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ba37f-127">hello **Name** column contains hello name of hello volume group.</span></span> |
| <span data-ttu-id="ba37f-128">Alkalmazás</span><span class="sxs-lookup"><span data-stu-id="ba37f-128">Application</span></span> |<span data-ttu-id="ba37f-129">Hello **alkalmazások** az oszlopban látható a jelenleg telepített VSS-író hello száma és a Windows-gazdagépen futó hello.</span><span class="sxs-lookup"><span data-stu-id="ba37f-129">hello **Applications** column shows hello number of VSS writers currently installed and running on hello Windows host.</span></span> |
| <span data-ttu-id="ba37f-130">Kiválasztva</span><span class="sxs-lookup"><span data-stu-id="ba37f-130">Selected</span></span> |<span data-ttu-id="ba37f-131">Hello **kijelölt** az oszlopban látható található kötetek számát hello hello kötet csoportban.</span><span class="sxs-lookup"><span data-stu-id="ba37f-131">hello **Selected** column shows hello number of volumes that are contained in hello volume group.</span></span> <span data-ttu-id="ba37f-132">A nulla (0) azt jelzi, hogy egyetlen alkalmazás hello kötetek hello kötet csoport társítva.</span><span class="sxs-lookup"><span data-stu-id="ba37f-132">A zero (0) indicates that no application is associated with hello volumes in hello volume group.</span></span> |
| <span data-ttu-id="ba37f-133">Importált</span><span class="sxs-lookup"><span data-stu-id="ba37f-133">Imported</span></span> |<span data-ttu-id="ba37f-134">Hello **importált** oszlop importált kötetek hello számát mutatja.</span><span class="sxs-lookup"><span data-stu-id="ba37f-134">hello **Imported** column shows hello number of imported volumes.</span></span> <span data-ttu-id="ba37f-135">Ha értéke túl**igaz**, ebben az oszlopban azt jelzi, hogy a kötet csoport importált hello Azure-portálon, és nem jött létre a StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="ba37f-135">When set too**True**, this column indicates that a volume group was imported from hello Azure portal and was not created in StorSimple Snapshot Manager.</span></span> |

> [!NOTE]
> <span data-ttu-id="ba37f-136">StorSimple Snapshot Manager kötet csoport is megjelenik a hello **biztonsági mentési házirendek** hello Azure-portálon lapján.</span><span class="sxs-lookup"><span data-stu-id="ba37f-136">StorSimple Snapshot Manager volume groups are also displayed on hello **Backup Policies** tab in hello Azure portal.</span></span>
> 
> 

## <a name="create-a-volume-group"></a><span data-ttu-id="ba37f-137">Kötet csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ba37f-137">Create a volume group</span></span>
<span data-ttu-id="ba37f-138">A következő eljárás toocreate egy kötet csoport hello használata.</span><span class="sxs-lookup"><span data-stu-id="ba37f-138">Use hello following procedure toocreate a volume group.</span></span>

#### <a name="toocreate-a-volume-group"></a><span data-ttu-id="ba37f-139">a kötet csoport toocreate</span><span class="sxs-lookup"><span data-stu-id="ba37f-139">toocreate a volume group</span></span>
1. <span data-ttu-id="ba37f-140">Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="ba37f-140">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="ba37f-141">A hello **hatókör** ablaktáblán kattintson a jobb gombbal **kötet csoportok**, és kattintson a **kötet csoport létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ba37f-141">In hello **Scope** pane, right-click **Volume Groups**, and then click **Create Volume Group**.</span></span>
   
    ![Kötet csoport létrehozása](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
   
    <span data-ttu-id="ba37f-143">Hello **hozzon létre egy kötet csoportot** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ba37f-143">hello **Create a volume group** dialog box appears.</span></span>
   
    ![Hozzon létre egy kötet csoport párbeszédpanel](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png)
3. <span data-ttu-id="ba37f-145">Adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="ba37f-145">Enter hello following information:</span></span>
   
   1. <span data-ttu-id="ba37f-146">A hello **neve** mezőbe írjon be egy egyedi nevet hello új kötet csoport.</span><span class="sxs-lookup"><span data-stu-id="ba37f-146">In hello **Name** box, type a unique name for hello new volume group.</span></span>
   2. <span data-ttu-id="ba37f-147">A hello **alkalmazások** mezőben, válassza ki, hogy fel szeretné venni toohello kötet csoport hello kötetek társított alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="ba37f-147">In hello **Applications** box, select applications associated with hello volumes that you will be adding toohello volume group.</span></span>
      
       <span data-ttu-id="ba37f-148">Hello **alkalmazások** mezőben listák csak ezeket az alkalmazásokat, amelyek a StorSimple-köteteket használ, és rendelkezik a VSS-írók engedélyezve van, a számukra.</span><span class="sxs-lookup"><span data-stu-id="ba37f-148">hello **Applications** box lists only those applications that use StorSimple volumes and have VSS writers enabled for them.</span></span> <span data-ttu-id="ba37f-149">VSS-író szolgáltatás engedélyezve van, csak ha összes hello kötetek tisztában-e, hogy hello író vannak a StorSimple-köteteket.</span><span class="sxs-lookup"><span data-stu-id="ba37f-149">A VSS writer is enabled only if all hello volumes that hello writer is aware of are StorSimple volumes.</span></span> <span data-ttu-id="ba37f-150">Ha hello alkalmazások mező üres, amely Azure StorSimple-köteteket használ, és támogatott van-e VSS-író alkalmazás sem lesz telepítve.</span><span class="sxs-lookup"><span data-stu-id="ba37f-150">If hello Applications box is empty, then no applications that use Azure StorSimple volumes and have supported VSS writers are installed.</span></span> <span data-ttu-id="ba37f-151">(Jelenleg Azure StorSimple támogatja a Microsoft Exchange, SQL Server.) További információ a VSS-író: [integráció a Windows a kötet árnyékmásolata szolgáltatás](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).</span><span class="sxs-lookup"><span data-stu-id="ba37f-151">(Currently, Azure StorSimple supports Microsoft Exchange and SQL Server.) For more information about VSS writers, see [Integration with Windows Volume Shadow Copy Service](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).</span></span>
      
       <span data-ttu-id="ba37f-152">Ha kiválaszt egy alkalmazást, akkor hozzá társított összes kötet automatikusan ki van jelölve.</span><span class="sxs-lookup"><span data-stu-id="ba37f-152">If you select an application, all volumes associated with it are automatically selected.</span></span> <span data-ttu-id="ba37f-153">Ezzel szemben, ha egy adott alkalmazáshoz társított kötetek, hello alkalmazás automatikusan ki van jelölve a hello **alkalmazások** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="ba37f-153">Conversely, if you select volumes associated with a specific application, hello application is automatically selected in hello **Applications** box.</span></span> 
   3. <span data-ttu-id="ba37f-154">A hello **kötetek** jelölje ki a StorSimple kötetek tooadd toohello kötet csoport.</span><span class="sxs-lookup"><span data-stu-id="ba37f-154">In hello **Volumes** box, select StorSimple volumes tooadd toohello volume group.</span></span> 
      
      * <span data-ttu-id="ba37f-155">Megadhat egy vagy több partíciókkal rendelkező köteteket.</span><span class="sxs-lookup"><span data-stu-id="ba37f-155">You can include volumes with single or multiple partitions.</span></span> <span data-ttu-id="ba37f-156">(Több partíció kötetek lehetnek dinamikus lemezek vagy alaplemezek több partícióval rendelkező.) A kötet, amely több partíciót tartalmaznak a rendszer egyetlen egységként kezeli.</span><span class="sxs-lookup"><span data-stu-id="ba37f-156">(Multiple partition volumes can be dynamic disks or basic disks with multiple partitions.) A volume that contains multiple partitions is treated as a single unit.</span></span> <span data-ttu-id="ba37f-157">Ezért ha többi partíció hozzáadása csak az egyik hello partíciók tooa kötet csoport összes hello vannak automatikusan hozzáadott toothat kötet csoportjának hello, azonos idő.</span><span class="sxs-lookup"><span data-stu-id="ba37f-157">Consequently, if you add only one of hello partitions tooa volume group, all hello other partitions are automatically added toothat volume group at hello same time.</span></span> <span data-ttu-id="ba37f-158">Egy több partíció kötet tooa kötet csoport hozzáadása után hello több partíció kötet továbbra is egyetlen egységként kezelt toobe.</span><span class="sxs-lookup"><span data-stu-id="ba37f-158">After you add a multiple partition volume tooa volume group, hello multiple partition volume continues toobe treated as a single unit.</span></span>
      * <span data-ttu-id="ba37f-159">Bármely kötetek toothem nem hozzárendelésével üres kötet csoportokat is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="ba37f-159">You can create empty volume groups by not assigning any volumes toothem.</span></span> 
      * <span data-ttu-id="ba37f-160">Ne keverje a fürt megosztott kötetei (CSV-k) és a CSV a hello kötet ugyanabban a csoportban.</span><span class="sxs-lookup"><span data-stu-id="ba37f-160">Do not mix cluster-shared volumes (CSVs) and non-CSVs in hello same volume group.</span></span> <span data-ttu-id="ba37f-161">StorSimple Snapshot Manager nem támogatja a megosztott fürtkötetek kombinációját, és nem CSV-köteteket a hello azonos pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="ba37f-161">StorSimple Snapshot Manager does not support a mix of CSV volumes and non-CSV volumes in hello same snapshot.</span></span>
4. <span data-ttu-id="ba37f-162">Kattintson a **OK** toosave hello kötet csoport.</span><span class="sxs-lookup"><span data-stu-id="ba37f-162">Click **OK** toosave hello volume group.</span></span>

## <a name="back-up-a-volume-group"></a><span data-ttu-id="ba37f-163">Készítsen biztonsági másolatot egy kötet csoport</span><span class="sxs-lookup"><span data-stu-id="ba37f-163">Back up a volume group</span></span>
<span data-ttu-id="ba37f-164">A következő eljárás tooback kötet csoport hello használata.</span><span class="sxs-lookup"><span data-stu-id="ba37f-164">Use hello following procedure tooback up a volume group.</span></span>

#### <a name="tooback-up-a-volume-group"></a><span data-ttu-id="ba37f-165">kötet csoport tooback</span><span class="sxs-lookup"><span data-stu-id="ba37f-165">tooback up a volume group</span></span>
1. <span data-ttu-id="ba37f-166">Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="ba37f-166">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="ba37f-167">A hello **hatókör** ablaktáblában bontsa ki a hello **kötet csoportok** csomópontot, kattintson a jobb gombbal egy kötet felügyeleticsoport-nevet, és kattintson **érvénybe biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="ba37f-167">In hello **Scope** pane, expand hello **Volume Groups** node, right-click a volume group name, and then click **Take Backup**.</span></span>
   
    ![Készítsen biztonsági másolatot kötet csoport azonnal](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)
3. <span data-ttu-id="ba37f-169">A hello **érvénybe biztonsági mentés** párbeszédpanelen jelölje ki **helyi pillanatfelvétel** vagy **felhőalapú pillanatfelvétel**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ba37f-169">In hello **Take Backup** dialog box, select **Local Snapshot** or **Cloud Snapshot**, and then click **Create**.</span></span>
   
    ![Igénybe vehet a biztonsági mentési párbeszédpanel](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png)
4. <span data-ttu-id="ba37f-171">amely a biztonsági mentési hello tooconfirm fut, bontsa ki a hello **feladatok** csomópontra, majd **futtató**.</span><span class="sxs-lookup"><span data-stu-id="ba37f-171">tooconfirm that hello backup is running, expand hello **Jobs** node, and then click **Running**.</span></span> <span data-ttu-id="ba37f-172">hello biztonsági mentést kell szerepelnie.</span><span class="sxs-lookup"><span data-stu-id="ba37f-172">hello backup should be listed.</span></span>
5. <span data-ttu-id="ba37f-173">befejeződött tooview hello pillanatkép, bontsa ki a hello **biztonságimásolat-katalógus** csomópontot, bontsa ki hello kötet csoport nevét, majd **helyi pillanatfelvétel** vagy **felhőalapú pillanatfelvétel**.</span><span class="sxs-lookup"><span data-stu-id="ba37f-173">tooview hello completed snapshot, expand hello **Backup Catalog** node, expand hello volume group name, and then click **Local Snapshot** or **Cloud Snapshot**.</span></span> <span data-ttu-id="ba37f-174">hello biztonsági mentés megjelenik, ha sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="ba37f-174">hello backup will be listed if it finished successfully.</span></span>

## <a name="edit-a-volume-group"></a><span data-ttu-id="ba37f-175">Kötet csoport szerkesztése</span><span class="sxs-lookup"><span data-stu-id="ba37f-175">Edit a volume group</span></span>
<span data-ttu-id="ba37f-176">A következő eljárás tooedit egy kötet csoport hello használata.</span><span class="sxs-lookup"><span data-stu-id="ba37f-176">Use hello following procedure tooedit a volume group.</span></span>

#### <a name="tooedit-a-volume-group"></a><span data-ttu-id="ba37f-177">a kötet csoport tooedit</span><span class="sxs-lookup"><span data-stu-id="ba37f-177">tooedit a volume group</span></span>
1. <span data-ttu-id="ba37f-178">Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="ba37f-178">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="ba37f-179">A hello **hatókör** ablaktáblában bontsa ki a hello **kötet csoportok** csomópontot, kattintson a jobb gombbal egy kötet felügyeleticsoport-nevet, és kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="ba37f-179">In hello **Scope** pane, expand hello **Volume Groups** node, right-click a volume group name, and then click **Edit**.</span></span>
3. <span data-ttu-id="ba37f-180">hello ** hozzon létre egy kötet csoportot ** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ba37f-180">hello **Create a volume group **dialog box appears.</span></span> <span data-ttu-id="ba37f-181">Módosíthatja a hello **neve**, **alkalmazások**, és **kötetek** bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="ba37f-181">You can change hello **Name**, **Applications**, and **Volumes** entries.</span></span>
4. <span data-ttu-id="ba37f-182">Kattintson a **OK** toosave a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="ba37f-182">Click **OK** toosave your changes.</span></span>

## <a name="delete-a-volume-group"></a><span data-ttu-id="ba37f-183">Kötet csoport törlése</span><span class="sxs-lookup"><span data-stu-id="ba37f-183">Delete a volume group</span></span>
<span data-ttu-id="ba37f-184">A következő eljárás toodelete egy kötet csoport hello használata.</span><span class="sxs-lookup"><span data-stu-id="ba37f-184">Use hello following procedure toodelete a volume group.</span></span> 

> [!WARNING]
> <span data-ttu-id="ba37f-185">Ez a hello kötet csoporthoz társított összes hello biztonsági mentés is törli.</span><span class="sxs-lookup"><span data-stu-id="ba37f-185">This also deletes all hello backups associated with hello volume group.</span></span>
> 
> 

#### <a name="toodelete-a-volume-group"></a><span data-ttu-id="ba37f-186">a kötet csoport toodelete</span><span class="sxs-lookup"><span data-stu-id="ba37f-186">toodelete a volume group</span></span>
1. <span data-ttu-id="ba37f-187">Kattintson a hello asztali ikon toostart StorSimple Snapshot Manager.</span><span class="sxs-lookup"><span data-stu-id="ba37f-187">Click hello desktop icon toostart StorSimple Snapshot Manager.</span></span>
2. <span data-ttu-id="ba37f-188">A hello **hatókör** ablaktáblában bontsa ki a hello **kötet csoportok** csomópontot, kattintson a jobb gombbal egy kötet felügyeleticsoport-nevet, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="ba37f-188">In hello **Scope** pane, expand hello **Volume Groups** node, right-click a volume group name, and then click **Delete**.</span></span>
3. <span data-ttu-id="ba37f-189">Hello **kötet csoport törlése** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ba37f-189">hello **Delete Volume Group** dialog box appears.</span></span> <span data-ttu-id="ba37f-190">Típus **megerősítése** hello szövegmezőbe, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="ba37f-190">Type **Confirm** in hello text box, and then click **OK**.</span></span>
   
    <span data-ttu-id="ba37f-191">törölt hello kötet csoport eltűnik hello hello listájából **eredmények** ablaktábla és az adott kötet csoporthoz rendelt összes biztonsági mentés hello biztonságimásolat-katalógus törlődnek.</span><span class="sxs-lookup"><span data-stu-id="ba37f-191">hello deleted volume group vanishes from hello list in hello **Results** pane and all backups that are associated with that volume group are deleted from hello backup catalog.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba37f-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ba37f-192">Next steps</span></span>
* <span data-ttu-id="ba37f-193">Ismerje meg, hogyan túl[használja a StorSimple Snapshot Manager tooadminister a StorSimple megoldásban](storsimple-snapshot-manager-admin.md).</span><span class="sxs-lookup"><span data-stu-id="ba37f-193">Learn how too[use StorSimple Snapshot Manager tooadminister your StorSimple solution](storsimple-snapshot-manager-admin.md).</span></span>
* <span data-ttu-id="ba37f-194">Ismerje meg, hogyan túl[StorSimple Snapshot Manager toocreate használatát és kezelését a biztonsági mentési házirendek](storsimple-snapshot-manager-manage-backup-policies.md).</span><span class="sxs-lookup"><span data-stu-id="ba37f-194">Learn how too[use StorSimple Snapshot Manager toocreate and manage backup policies](storsimple-snapshot-manager-manage-backup-policies.md).</span></span>

