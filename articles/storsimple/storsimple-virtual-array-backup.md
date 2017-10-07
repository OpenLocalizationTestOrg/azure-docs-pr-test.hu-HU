---
title: "aaaMicrosoft Azure StorSimple virtuális tömb biztonsági mentési oktatóanyag |} Microsoft Docs"
description: "Ismerteti, hogyan osztja meg a StorSimple virtuális tömb mentése tooback és köteteket."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: e3cdcd9e-33b1-424d-82aa-b369d934067e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7a015fd594f8f56c48fab149a2736be9dec2c24b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a><span data-ttu-id="a3014-103">A StorSimple virtuális tömb biztonsági másolatot a megosztások vagy kötetek</span><span class="sxs-lookup"><span data-stu-id="a3014-103">Back up shares or volumes on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="a3014-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a3014-104">Overview</span></span>

<span data-ttu-id="a3014-105">a StorSimple virtuális tömb hello egy hibrid felhő helyszíni virtuális tárolóeszköz, konfigurálhat egy fájl vagy iSCSI-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="a3014-105">hello StorSimple Virtual Array is a hybrid cloud storage on-premises virtual device that can be configured as a file server or an iSCSI server.</span></span> <span data-ttu-id="a3014-106">hello virtuális tömb lehetővé teszi, hogy hello felhasználói toocreate ütemezett és manuális biztonsági mentés minden hello megosztások vagy kötetek hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="a3014-106">hello virtual array allows hello user toocreate scheduled and manual backups of all hello shares or volumes on hello device.</span></span> <span data-ttu-id="a3014-107">Amikor egy fájl-kiszolgálóként konfigurált lehetővé teszi elemszintű helyreállítás.</span><span class="sxs-lookup"><span data-stu-id="a3014-107">When configured as a file server, it also allows item-level recovery.</span></span> <span data-ttu-id="a3014-108">Ez az oktatóanyag ismerteti, hogyan toocreate ütemezett és manuális biztonsági mentések, és végezze el a virtuális tömb törölt fájlok elemszintű helyreállítás toorestore.</span><span class="sxs-lookup"><span data-stu-id="a3014-108">This tutorial describes how toocreate scheduled and manual backups and perform item-level recovery toorestore a deleted file on your virtual array.</span></span>

<span data-ttu-id="a3014-109">Ez az oktatóanyag toohello StorSimple virtuális tömbök csak vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a3014-109">This tutorial applies toohello StorSimple Virtual Arrays only.</span></span> <span data-ttu-id="a3014-110">8000 Series információkért nyissa meg túl[8000 sorozat eszköz biztonsági mentés létrehozása](storsimple-manage-backup-policies-u2.md)</span><span class="sxs-lookup"><span data-stu-id="a3014-110">For information on 8000 series, go too[Create a backup for 8000 series device](storsimple-manage-backup-policies-u2.md)</span></span>

## <a name="back-up-shares-and-volumes"></a><span data-ttu-id="a3014-111">Megosztások és a kötetek biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="a3014-111">Back up shares and volumes</span></span>

<span data-ttu-id="a3014-112">Biztonsági mentések időpontban védelmet, javítja a helyreállíthatóságot és megosztásokat és -kötetek visszaállítása idejének minimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="a3014-112">Backups provide point-in-time protection, improve recoverability, and minimize restore times for shares and volumes.</span></span> <span data-ttu-id="a3014-113">Biztonsági másolatot készíthet egy fájlmegosztás vagy kötet StorSimple eszközén kétféle módon: **ütemezett** vagy **manuális**.</span><span class="sxs-lookup"><span data-stu-id="a3014-113">You can back up a share or volume on your StorSimple device in two ways: **Scheduled** or **Manual**.</span></span> <span data-ttu-id="a3014-114">A következő szakaszok hello hello módszerek ismertet.</span><span class="sxs-lookup"><span data-stu-id="a3014-114">Each of hello methods is discussed in hello following sections.</span></span>

## <a name="change-hello-backup-start-time"></a><span data-ttu-id="a3014-115">Hello biztonsági mentés kezdési ideje</span><span class="sxs-lookup"><span data-stu-id="a3014-115">Change hello backup start time</span></span>

> [!NOTE]
> <span data-ttu-id="a3014-116">Ebben a kiadásban ütemezett biztonsági mentések hozza létre, amely egy megadott időpontban naponta fut, és készít biztonsági másolatot minden hello megosztások vagy kötetek; hello eszközön alapértelmezett házirend.</span><span class="sxs-lookup"><span data-stu-id="a3014-116">In this release, scheduled backups are created by a default policy that runs daily at a specified time and backs up all hello shares or volumes on hello device.</span></span> <span data-ttu-id="a3014-117">Ez jelenleg nem lehetséges toocreate egyéni házirendeket az ütemezett biztonsági mentések.</span><span class="sxs-lookup"><span data-stu-id="a3014-117">It is not possible toocreate custom policies for scheduled backups at this time.</span></span>


<span data-ttu-id="a3014-118">A StorSimple virtuális tömb tartozik egy alapértelmezett biztonsági mentési házirend, amely egy megadott időpontot (22:30) kezdődik, és biztonsági mentést készít minden hello megosztások vagy kötetek; hello eszközön naponta egyszer.</span><span class="sxs-lookup"><span data-stu-id="a3014-118">Your StorSimple Virtual Array has a default backup policy that starts at a specified time of day (22:30) and backs up all hello shares or volumes on hello device once a day.</span></span> <span data-ttu-id="a3014-119">Módosíthatja a hello idő mely hello biztonsági mentés elindul, de hello gyakoriságát és hello megőrzési (amely azt adja meg a biztonsági mentések tooretain hello száma) nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="a3014-119">You can change hello time at which hello backup starts, but hello frequency and hello retention (which specifies hello number of backups tooretain) cannot be changed.</span></span> <span data-ttu-id="a3014-120">Ezek a biztonsági mentések során hello teljes virtuális eszköz biztonsági mentése.</span><span class="sxs-lookup"><span data-stu-id="a3014-120">During these backups, hello entire virtual device is backed up.</span></span> <span data-ttu-id="a3014-121">Ez sikerült potenciálisan hello eszköz hello teljesítményt, és hatással lehet a hello eszközön telepített hello munkaterhelésekhez.</span><span class="sxs-lookup"><span data-stu-id="a3014-121">This could potentially impact hello performance of hello device and affect hello workloads deployed on hello device.</span></span> <span data-ttu-id="a3014-122">Ezért azt javasoljuk, hogy ezek a biztonsági mentések ütemezése kevesen.</span><span class="sxs-lookup"><span data-stu-id="a3014-122">Therefore, we recommend that you schedule these backups for off-peak hours.</span></span>

 <span data-ttu-id="a3014-123">toochange hello alapértelmezett biztonsági mentés kezdési időpontja, hajtsa végre a következő lépéseket a hello hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a3014-123">toochange hello default backup start time, perform hello following steps in hello [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="toochange-hello-start-time-for-hello-default-backup-policy"></a><span data-ttu-id="a3014-124">toochange hello kezdő időpont hello alapértelmezett biztonsági mentési házirend</span><span class="sxs-lookup"><span data-stu-id="a3014-124">toochange hello start time for hello default backup policy</span></span>

1. <span data-ttu-id="a3014-125">Nyissa meg túl**eszközök**.</span><span class="sxs-lookup"><span data-stu-id="a3014-125">Go too**Devices**.</span></span> <span data-ttu-id="a3014-126">a StorSimple Device Manager szolgáltatásban regisztrált eszközök hello listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a3014-126">hello list of devices registered with your StorSimple Device Manager service will be displayed.</span></span> 
   
    ![Keresse meg a toodevices](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. <span data-ttu-id="a3014-128">Jelölje ki, majd kattintson az eszköz.</span><span class="sxs-lookup"><span data-stu-id="a3014-128">Select and click your device.</span></span> <span data-ttu-id="a3014-129">Hello **beállítások** panel fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="a3014-129">hello **Settings** blade will be displayed.</span></span> <span data-ttu-id="a3014-130">Nyissa meg túl**kezelése > biztonsági mentési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="a3014-130">Go too**Manage > Backup policies**.</span></span>
   
    ![az eszköz kiválasztásához](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. <span data-ttu-id="a3014-132">A hello **biztonsági mentési házirendek** panelen hello alapértelmezett kezdési ideje 22:30.</span><span class="sxs-lookup"><span data-stu-id="a3014-132">In hello **Backup policies** blade, hello default start time is 22:30.</span></span> <span data-ttu-id="a3014-133">Eszköz időzóna hello napi ütemezés hello új kezdési időpontot adhat meg.</span><span class="sxs-lookup"><span data-stu-id="a3014-133">You can specify hello new start time for hello daily schedule in device time zone.</span></span>
   
    ![Keresse meg a toobackup házirendek](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. <span data-ttu-id="a3014-135">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="a3014-135">Click **Save**.</span></span>

### <a name="take-a-manual-backup"></a><span data-ttu-id="a3014-136">Manuális biztonsági mentés készítése</span><span class="sxs-lookup"><span data-stu-id="a3014-136">Take a manual backup</span></span>

<span data-ttu-id="a3014-137">Ezenkívül tooscheduled biztonsági mentések, végre eszközadatok manuális (igény) biztonsági mentés bármikor.</span><span class="sxs-lookup"><span data-stu-id="a3014-137">In addition tooscheduled backups, you can take a manual (on-demand) backup of device data at any time.</span></span>

#### <a name="toocreate-a-manual-backup"></a><span data-ttu-id="a3014-138">Manuális biztonsági mentés toocreate</span><span class="sxs-lookup"><span data-stu-id="a3014-138">toocreate a manual backup</span></span>

1. <span data-ttu-id="a3014-139">Nyissa meg túl**eszközök**.</span><span class="sxs-lookup"><span data-stu-id="a3014-139">Go too**Devices**.</span></span> <span data-ttu-id="a3014-140">Válassza ki azt az eszközt, és kattintson a jobb gombbal **...**  jobb szélén hello: hello kijelölt sor.</span><span class="sxs-lookup"><span data-stu-id="a3014-140">Select your device and right-click **...** at hello far right in hello selected row.</span></span> <span data-ttu-id="a3014-141">Hello helyi menüből válassza ki a **biztonsági másolatok készítéséhez**.</span><span class="sxs-lookup"><span data-stu-id="a3014-141">From hello context menu, select **Take backup**.</span></span>
   
    ![Keresse meg a tootake biztonsági mentése](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. <span data-ttu-id="a3014-143">A hello **biztonsági másolatok készítéséhez** panelen kattintson a **biztonsági másolatok készítéséhez**.</span><span class="sxs-lookup"><span data-stu-id="a3014-143">In hello **Take backup** blade, click **Take backup**.</span></span> <span data-ttu-id="a3014-144">Ez biztonsági másolatot készít minden hello megosztások fájlkiszolgáló hello vagy az iSCSI-kiszolgálón az összes hello kötetek.</span><span class="sxs-lookup"><span data-stu-id="a3014-144">This will backup all hello shares on hello file server or all hello volumes on your iSCSI server.</span></span> 
   
    ![biztonsági mentés indítása](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    <span data-ttu-id="a3014-146">Egy igény szerinti biztonsági mentés elindul, és láthatja, hogy egy biztonsági mentési feladat elindult.</span><span class="sxs-lookup"><span data-stu-id="a3014-146">An on-demand backup starts and you see that a backup job has started.</span></span>
   
    ![biztonsági mentés indítása](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    <span data-ttu-id="a3014-148">Miután hello feladat sikeresen befejeződött, értesítés jelenik meg újra.</span><span class="sxs-lookup"><span data-stu-id="a3014-148">Once hello job has successfully completed, you are notified again.</span></span> <span data-ttu-id="a3014-149">Ezután elindítja hello biztonsági mentési folyamat.</span><span class="sxs-lookup"><span data-stu-id="a3014-149">hello backup process then starts.</span></span>
   
    ![Biztonsági mentési feladat létrehozása](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. <span data-ttu-id="a3014-151">tootrack hello előrehaladását hello biztonsági mentéseit és nézze meg hello feladat részleteit, kattintson a hello értesítés.</span><span class="sxs-lookup"><span data-stu-id="a3014-151">tootrack hello progress of hello backups and look at hello job details, click hello notification.</span></span> <span data-ttu-id="a3014-152">Ezzel megnyitná túl **feladat részletei**.</span><span class="sxs-lookup"><span data-stu-id="a3014-152">This takes you too **Job details**.</span></span>
   
     ![biztonsági mentési feladat részleteit](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. <span data-ttu-id="a3014-154">Hello biztonsági mentés befejezése után, nyissa meg túl**felügyeleti > biztonságimásolat-katalógus**.</span><span class="sxs-lookup"><span data-stu-id="a3014-154">After hello backup is complete, go too**Management > Backup catalog**.</span></span> <span data-ttu-id="a3014-155">Látni fogja a felhő-pillanatfelvételt a összes hello megosztások (vagy kötetek;) az eszközön.</span><span class="sxs-lookup"><span data-stu-id="a3014-155">You will see a cloud snapshot of all hello shares (or volumes) on your device.</span></span>
   
    ![Befejezett biztonsági mentése](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a><span data-ttu-id="a3014-157">Meglévő biztonsági másolatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="a3014-157">View existing backups</span></span>
<span data-ttu-id="a3014-158">tooview hello létező biztonsági másolatai, hajtsa végre a következő lépéseket az Azure-portálon hello hello.</span><span class="sxs-lookup"><span data-stu-id="a3014-158">tooview hello existing backups, perform hello following steps in hello Azure portal.</span></span>

#### <a name="tooview-existing-backups"></a><span data-ttu-id="a3014-159">tooview meglévő biztonsági másolatok</span><span class="sxs-lookup"><span data-stu-id="a3014-159">tooview existing backups</span></span>

1. <span data-ttu-id="a3014-160">Nyissa meg túl**eszközök** panelen.</span><span class="sxs-lookup"><span data-stu-id="a3014-160">Go too**Devices** blade.</span></span> <span data-ttu-id="a3014-161">Jelölje ki, majd kattintson az eszköz.</span><span class="sxs-lookup"><span data-stu-id="a3014-161">Select and click your device.</span></span> <span data-ttu-id="a3014-162">A hello **beállítások** paneljén lépjen túl**felügyeleti > biztonságimásolat-katalógus**.</span><span class="sxs-lookup"><span data-stu-id="a3014-162">In hello **Settings** blade, go too**Management > Backup Catalog**.</span></span>
   
    ![Keresse meg a toobackup katalógus](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. <span data-ttu-id="a3014-164">Adja meg a következő feltételek toobe a szűréshez használt hello:</span><span class="sxs-lookup"><span data-stu-id="a3014-164">Specify hello following criteria toobe used for filtering:</span></span>
   
    - <span data-ttu-id="a3014-165">**Időtartománynak** – lehet **elmúlt 1 óra**, **elmúlt 24 óra**, **elmúlt 7 napban**, **az elmúlt 30 napban**, **múltbeli évre** , és **egyéni dátum**.</span><span class="sxs-lookup"><span data-stu-id="a3014-165">**Time range** – can be **Past 1 hour**, **Past 24 hours**, **Past 7 days**, **Past 30 days**, **Past year**, and **Custom date**.</span></span>
    
    - <span data-ttu-id="a3014-166">**Eszközök** – válassza ki a fájlkiszolgálók és a StorSimple Device Manager szolgáltatásban regisztrált iSCSI-kiszolgálókhoz hello listából.</span><span class="sxs-lookup"><span data-stu-id="a3014-166">**Devices** – select from hello list of file servers or iSCSI servers registered with your StorSimple Device Manager service.</span></span>
   
    - <span data-ttu-id="a3014-167">**Kezdeményezett** – automatikusan lehet **ütemezett** (által a biztonsági mentési házirend) vagy **manuálisan** kezdeményeznie (,).</span><span class="sxs-lookup"><span data-stu-id="a3014-167">**Initiated** – can be automatically **Scheduled** (by a backup policy) or **Manually** initiated (by you).</span></span>
   
    ![Biztonsági mentések szűrése](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. <span data-ttu-id="a3014-169">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="a3014-169">Click **Apply**.</span></span> <span data-ttu-id="a3014-170">hello biztonsági mentések szűrt listája megjelenik hello **biztonságimásolat-katalógus** panelen.</span><span class="sxs-lookup"><span data-stu-id="a3014-170">hello filtered list of backups is displayed in hello **Backup catalog** blade.</span></span> <span data-ttu-id="a3014-171">Megjegyzés: csak 100 biztonsági mentési elemek megjelenítése egy adott időpontban.</span><span class="sxs-lookup"><span data-stu-id="a3014-171">Note only 100 backup elements can be displayed at a given time.</span></span>
   
    ![Frissített biztonságimásolat-katalógus](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a><span data-ttu-id="a3014-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3014-173">Next steps</span></span>

<span data-ttu-id="a3014-174">További információ [felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="a3014-174">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

