---
title: "A Microsoft Azure StorSimple virtuális tömb biztonsági mentési oktatóanyag |} Microsoft Docs"
description: "Ismerteti a StorSimple virtuális tömb megosztások és a kötetek biztonsági mentése."
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
ms.openlocfilehash: c926f0c80ce56cac3106ad97ec3ec2e18a8e2cc6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a><span data-ttu-id="33812-103">A StorSimple virtuális tömb biztonsági másolatot a megosztások vagy kötetek</span><span class="sxs-lookup"><span data-stu-id="33812-103">Back up shares or volumes on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="33812-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="33812-104">Overview</span></span>

<span data-ttu-id="33812-105">A StorSimple virtuális egy hibrid felhő helyszíni virtuális tárolóeszköz, konfigurálhat egy fájl vagy iSCSI-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="33812-105">The StorSimple Virtual Array is a hybrid cloud storage on-premises virtual device that can be configured as a file server or an iSCSI server.</span></span> <span data-ttu-id="33812-106">A virtuális tömb lehetővé teszi, hogy a felhasználó az eszköz ütemezett és manuális biztonsági mentést a megosztások vagy kötetek létrehozására.</span><span class="sxs-lookup"><span data-stu-id="33812-106">The virtual array allows the user to create scheduled and manual backups of all the shares or volumes on the device.</span></span> <span data-ttu-id="33812-107">Amikor egy fájl-kiszolgálóként konfigurált lehetővé teszi elemszintű helyreállítás.</span><span class="sxs-lookup"><span data-stu-id="33812-107">When configured as a file server, it also allows item-level recovery.</span></span> <span data-ttu-id="33812-108">Ez az oktatóanyag ismerteti, hogyan ütemezett és manuális biztonsági mentés létrehozásához, és visszaállítani a virtuális tömb törölt fájlok elemszintű helyreállítás.</span><span class="sxs-lookup"><span data-stu-id="33812-108">This tutorial describes how to create scheduled and manual backups and perform item-level recovery to restore a deleted file on your virtual array.</span></span>

<span data-ttu-id="33812-109">Ez az oktatóanyag a StorSimple virtuális tömbök csak vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="33812-109">This tutorial applies to the StorSimple Virtual Arrays only.</span></span> <span data-ttu-id="33812-110">8000 Series információkért látogasson el [8000 sorozat eszköz biztonsági mentés létrehozása](storsimple-manage-backup-policies-u2.md)</span><span class="sxs-lookup"><span data-stu-id="33812-110">For information on 8000 series, go to [Create a backup for 8000 series device](storsimple-manage-backup-policies-u2.md)</span></span>

## <a name="back-up-shares-and-volumes"></a><span data-ttu-id="33812-111">Megosztások és a kötetek biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="33812-111">Back up shares and volumes</span></span>

<span data-ttu-id="33812-112">Biztonsági mentések időpontban védelmet, javítja a helyreállíthatóságot és megosztásokat és -kötetek visszaállítása idejének minimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="33812-112">Backups provide point-in-time protection, improve recoverability, and minimize restore times for shares and volumes.</span></span> <span data-ttu-id="33812-113">Biztonsági másolatot készíthet egy fájlmegosztás vagy kötet StorSimple eszközén kétféle módon: **ütemezett** vagy **manuális**.</span><span class="sxs-lookup"><span data-stu-id="33812-113">You can back up a share or volume on your StorSimple device in two ways: **Scheduled** or **Manual**.</span></span> <span data-ttu-id="33812-114">A módszer a következő szakaszban tárgyalt.</span><span class="sxs-lookup"><span data-stu-id="33812-114">Each of the methods is discussed in the following sections.</span></span>

## <a name="change-the-backup-start-time"></a><span data-ttu-id="33812-115">Módosítsa a biztonsági mentés kezdési ideje</span><span class="sxs-lookup"><span data-stu-id="33812-115">Change the backup start time</span></span>

> [!NOTE]
> <span data-ttu-id="33812-116">Ebben a kiadásban ütemezett biztonsági mentések hozza létre az alapértelmezett házirend, amely egy megadott időpontban naponta fut, és készít biztonsági másolatot a megosztások vagy kötetek az eszközön.</span><span class="sxs-lookup"><span data-stu-id="33812-116">In this release, scheduled backups are created by a default policy that runs daily at a specified time and backs up all the shares or volumes on the device.</span></span> <span data-ttu-id="33812-117">Nincs lehetőség egyéni házirendeket az ütemezett biztonsági mentések jelenleg létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="33812-117">It is not possible to create custom policies for scheduled backups at this time.</span></span>


<span data-ttu-id="33812-118">A StorSimple virtuális tömb tartozik egy alapértelmezett biztonsági mentési házirend, amely egy megadott időpontot (22:30) kezdődik, és biztonsági másolatot készít a megosztások vagy kötetek az eszközön naponta egyszer.</span><span class="sxs-lookup"><span data-stu-id="33812-118">Your StorSimple Virtual Array has a default backup policy that starts at a specified time of day (22:30) and backs up all the shares or volumes on the device once a day.</span></span> <span data-ttu-id="33812-119">Módosíthatja az idő, ahol a biztonsági mentés elindul, de a gyakoriságát és a megőrzés (amely azt adja meg a megőrizni kívánt biztonsági mentéseinek számát) nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="33812-119">You can change the time at which the backup starts, but the frequency and the retention (which specifies the number of backups to retain) cannot be changed.</span></span> <span data-ttu-id="33812-120">A biztonsági mentése során a virtuális eszköz teljes biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="33812-120">During these backups, the entire virtual device is backed up.</span></span> <span data-ttu-id="33812-121">Ez sikerült potenciálisan hatással lehet az eszköz teljesítményét, és hatással lehet az eszközre telepített munkaterhelésekhez.</span><span class="sxs-lookup"><span data-stu-id="33812-121">This could potentially impact the performance of the device and affect the workloads deployed on the device.</span></span> <span data-ttu-id="33812-122">Ezért azt javasoljuk, hogy ezek a biztonsági mentések ütemezése kevesen.</span><span class="sxs-lookup"><span data-stu-id="33812-122">Therefore, we recommend that you schedule these backups for off-peak hours.</span></span>

 <span data-ttu-id="33812-123">Ha módosítani szeretné az alapértelmezett biztonsági mentés kezdési ideje, hajtsa végre a következő lépéseket a [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="33812-123">To change the default backup start time, perform the following steps in the [Azure portal](https://portal.azure.com/).</span></span>

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a><span data-ttu-id="33812-124">A kezdési idejét az alapértelmezett biztonsági mentési házirend módosítása</span><span class="sxs-lookup"><span data-stu-id="33812-124">To change the start time for the default backup policy</span></span>

1. <span data-ttu-id="33812-125">Ugrás a **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="33812-125">Go to **Devices**.</span></span> <span data-ttu-id="33812-126">A StorSimple Device Manager szolgáltatásban regisztrált eszközök listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="33812-126">The list of devices registered with your StorSimple Device Manager service will be displayed.</span></span> 
   
    ![Keresse meg az eszközökre](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. <span data-ttu-id="33812-128">Jelölje ki, majd kattintson az eszköz.</span><span class="sxs-lookup"><span data-stu-id="33812-128">Select and click your device.</span></span> <span data-ttu-id="33812-129">A **beállítások** panel fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="33812-129">The **Settings** blade will be displayed.</span></span> <span data-ttu-id="33812-130">Ugrás a **kezelése > biztonsági mentési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="33812-130">Go to **Manage > Backup policies**.</span></span>
   
    ![az eszköz kiválasztásához](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. <span data-ttu-id="33812-132">Az a **biztonsági mentési házirendek** panelen az alapértelmezett kezdő érték 22:30.</span><span class="sxs-lookup"><span data-stu-id="33812-132">In the **Backup policies** blade, the default start time is 22:30.</span></span> <span data-ttu-id="33812-133">Eszköz időzóna is megadhat a napi ütemezés új kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="33812-133">You can specify the new start time for the daily schedule in device time zone.</span></span>
   
    ![Navigáljon a biztonsági mentési házirendek](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. <span data-ttu-id="33812-135">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="33812-135">Click **Save**.</span></span>

### <a name="take-a-manual-backup"></a><span data-ttu-id="33812-136">Manuális biztonsági mentés készítése</span><span class="sxs-lookup"><span data-stu-id="33812-136">Take a manual backup</span></span>

<span data-ttu-id="33812-137">Ütemezett biztonsági mentések mellett végre (igény) kézi biztonsági másolatot az eszközön tárolt adatok bármikor.</span><span class="sxs-lookup"><span data-stu-id="33812-137">In addition to scheduled backups, you can take a manual (on-demand) backup of device data at any time.</span></span>

#### <a name="to-create-a-manual-backup"></a><span data-ttu-id="33812-138">Manuális biztonsági mentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="33812-138">To create a manual backup</span></span>

1. <span data-ttu-id="33812-139">Ugrás a **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="33812-139">Go to **Devices**.</span></span> <span data-ttu-id="33812-140">Válassza ki azt az eszközt, és kattintson a jobb gombbal **...**  a kijelölt sor jobb szélén.</span><span class="sxs-lookup"><span data-stu-id="33812-140">Select your device and right-click **...** at the far right in the selected row.</span></span> <span data-ttu-id="33812-141">Válassza ki a helyi menüből **biztonsági másolatok készítéséhez**.</span><span class="sxs-lookup"><span data-stu-id="33812-141">From the context menu, select **Take backup**.</span></span>
   
    ![Keresse meg a biztonsági másolatok készítéséhez](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. <span data-ttu-id="33812-143">Az a **biztonsági másolatok készítéséhez** panelen kattintson a **biztonsági másolatok készítéséhez**.</span><span class="sxs-lookup"><span data-stu-id="33812-143">In the **Take backup** blade, click **Take backup**.</span></span> <span data-ttu-id="33812-144">Ez biztonsági másolatot készít a fájlkiszolgálón a megosztásokat vagy az iSCSI-kiszolgálón a köteteket.</span><span class="sxs-lookup"><span data-stu-id="33812-144">This will backup all the shares on the file server or all the volumes on your iSCSI server.</span></span> 
   
    ![biztonsági mentés indítása](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    <span data-ttu-id="33812-146">Egy igény szerinti biztonsági mentés elindul, és láthatja, hogy egy biztonsági mentési feladat elindult.</span><span class="sxs-lookup"><span data-stu-id="33812-146">An on-demand backup starts and you see that a backup job has started.</span></span>
   
    ![biztonsági mentés indítása](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    <span data-ttu-id="33812-148">Ha a feladat sikeresen befejeződött, értesítés jelenik meg újra.</span><span class="sxs-lookup"><span data-stu-id="33812-148">Once the job has successfully completed, you are notified again.</span></span> <span data-ttu-id="33812-149">Ezután elindítja a biztonsági mentési folyamat.</span><span class="sxs-lookup"><span data-stu-id="33812-149">The backup process then starts.</span></span>
   
    ![Biztonsági mentési feladat létrehozása](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. <span data-ttu-id="33812-151">A biztonsági másolatokat az előrehaladását úgy követheti nyomon, és nézze meg a feladat részleteit, kattintson az értesítés.</span><span class="sxs-lookup"><span data-stu-id="33812-151">To track the progress of the backups and look at the job details, click the notification.</span></span> <span data-ttu-id="33812-152">Ehhez szükséges, hogy **feladat részletei**.</span><span class="sxs-lookup"><span data-stu-id="33812-152">This takes you to  **Job details**.</span></span>
   
     ![biztonsági mentési feladat részleteit](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. <span data-ttu-id="33812-154">A biztonsági mentés befejezése után, nyissa meg a **felügyeleti > biztonságimásolat-katalógus**.</span><span class="sxs-lookup"><span data-stu-id="33812-154">After the backup is complete, go to **Management > Backup catalog**.</span></span> <span data-ttu-id="33812-155">Az eszközön megjelenik egy felhő-pillanatfelvételt összes megosztások (vagy kötet).</span><span class="sxs-lookup"><span data-stu-id="33812-155">You will see a cloud snapshot of all the shares (or volumes) on your device.</span></span>
   
    ![Befejezett biztonsági mentése](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a><span data-ttu-id="33812-157">Meglévő biztonsági másolatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="33812-157">View existing backups</span></span>
<span data-ttu-id="33812-158">A meglévő biztonsági másolatok megtekintéséhez a következő lépésekkel az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="33812-158">To view the existing backups, perform the following steps in the Azure portal.</span></span>

#### <a name="to-view-existing-backups"></a><span data-ttu-id="33812-159">Meglévő biztonsági másolatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="33812-159">To view existing backups</span></span>

1. <span data-ttu-id="33812-160">Ugrás a **eszközök** panelen.</span><span class="sxs-lookup"><span data-stu-id="33812-160">Go to **Devices** blade.</span></span> <span data-ttu-id="33812-161">Jelölje ki, majd kattintson az eszköz.</span><span class="sxs-lookup"><span data-stu-id="33812-161">Select and click your device.</span></span> <span data-ttu-id="33812-162">Az a **beállítások** paneljén lépjen **felügyeleti > biztonságimásolat-katalógus**.</span><span class="sxs-lookup"><span data-stu-id="33812-162">In the **Settings** blade, go to **Management > Backup Catalog**.</span></span>
   
    ![Navigáljon a biztonságimásolat-katalógus](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. <span data-ttu-id="33812-164">Adja meg a szűréshez használandó a következő feltételeknek:</span><span class="sxs-lookup"><span data-stu-id="33812-164">Specify the following criteria to be used for filtering:</span></span>
   
    - <span data-ttu-id="33812-165">**Időtartománynak** – lehet **elmúlt 1 óra**, **elmúlt 24 óra**, **elmúlt 7 napban**, **az elmúlt 30 napban**, **múltbeli évre** , és **egyéni dátum**.</span><span class="sxs-lookup"><span data-stu-id="33812-165">**Time range** – can be **Past 1 hour**, **Past 24 hours**, **Past 7 days**, **Past 30 days**, **Past year**, and **Custom date**.</span></span>
    
    - <span data-ttu-id="33812-166">**Eszközök** – válassza ki a fájlkiszolgálók vagy iSCSI-kiszolgálókhoz a StorSimple Device Manager szolgáltatásban regisztrált listájáról.</span><span class="sxs-lookup"><span data-stu-id="33812-166">**Devices** – select from the list of file servers or iSCSI servers registered with your StorSimple Device Manager service.</span></span>
   
    - <span data-ttu-id="33812-167">**Kezdeményezett** – automatikusan lehet **ütemezett** (által a biztonsági mentési házirend) vagy **manuálisan** kezdeményeznie (,).</span><span class="sxs-lookup"><span data-stu-id="33812-167">**Initiated** – can be automatically **Scheduled** (by a backup policy) or **Manually** initiated (by you).</span></span>
   
    ![Biztonsági mentések szűrése](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. <span data-ttu-id="33812-169">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="33812-169">Click **Apply**.</span></span> <span data-ttu-id="33812-170">A biztonsági mentések szűrt listája megjelenik a **biztonságimásolat-katalógus** panelen.</span><span class="sxs-lookup"><span data-stu-id="33812-170">The filtered list of backups is displayed in the **Backup catalog** blade.</span></span> <span data-ttu-id="33812-171">Megjegyzés: csak 100 biztonsági mentési elemek megjelenítése egy adott időpontban.</span><span class="sxs-lookup"><span data-stu-id="33812-171">Note only 100 backup elements can be displayed at a given time.</span></span>
   
    ![Frissített biztonságimásolat-katalógus](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a><span data-ttu-id="33812-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="33812-173">Next steps</span></span>

<span data-ttu-id="33812-174">További információ [felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="33812-174">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

