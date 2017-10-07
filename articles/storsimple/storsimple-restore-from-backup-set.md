---
title: "a StorSimple-kötet biztonsági másolatból aaaRestore |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello StorSimple Manager szolgáltatás biztonságimásolat-katalógus lap toorestore a StorSimple-kötet a biztonságimásolat-készletből."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b979782e-3184-4465-ad5f-e4516a5885d2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: e0efa74b14603be41af0cfc5400de3c39ab8824e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a><span data-ttu-id="dc373-103">A StorSimple-kötet visszaállítása egy biztonságimásolat-készlet</span><span class="sxs-lookup"><span data-stu-id="dc373-103">Restore a StorSimple volume from a backup set</span></span>
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a><span data-ttu-id="dc373-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="dc373-104">Overview</span></span>
<span data-ttu-id="dc373-105">Hello **biztonságimásolat-katalógus** oldal megjelenik, amely jönnek létre, ha manuális vagy automatikus biztonsági mentés készül minden hello biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="dc373-105">hello **Backup Catalog** page displays all hello backup sets that are created when manual or automated backups are taken.</span></span> <span data-ttu-id="dc373-106">A lap toolist összes hello biztonsági mentés használja a biztonsági mentési házirend, vagy egy kötetet, jelölje be vagy törölje a biztonsági mentések, vagy használja a biztonsági mentési toorestore vagy kötet klónozása.</span><span class="sxs-lookup"><span data-stu-id="dc373-106">You can use this page toolist all hello backups for a backup policy or a volume, select or delete backups, or use a backup toorestore or clone a volume.</span></span>

 ![Biztonságimásolat-katalógus lap](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

<span data-ttu-id="dc373-108">Ez az oktatóanyag azt ismerteti, hogyan toouse hello **biztonságimásolat-katalógus** lap toorestore egy kötet a biztonságimásolat-készletből az eszközön.</span><span class="sxs-lookup"><span data-stu-id="dc373-108">This tutorial explains how toouse hello **Backup Catalog** page toorestore a volume on your device from a backup set.</span></span>

## <a name="how-toouse-hello-backup-catalog"></a><span data-ttu-id="dc373-109">Hogyan toouse hello biztonságimásolat-katalógus</span><span class="sxs-lookup"><span data-stu-id="dc373-109">How toouse hello backup catalog</span></span>
<span data-ttu-id="dc373-110">Hello **biztonságimásolat-katalógus** lap biztosít egy lekérdezést, amely segít toonarrow a biztonságimásolat-készlet kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="dc373-110">hello **Backup Catalog** page provides a query that helps you toonarrow your backup set selection.</span></span> <span data-ttu-id="dc373-111">Hello biztonságimásolat-készletek, amelyek a rendszer beolvassa a következő paraméterek hello alapján szűrheti:</span><span class="sxs-lookup"><span data-stu-id="dc373-111">You can filter hello backup sets that are retrieved based on hello following parameters:</span></span>

* <span data-ttu-id="dc373-112">**Eszköz** – hello eszköz mely hello a biztonságimásolat-készlet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="dc373-112">**Device** – hello device on which hello backup set was created.</span></span>
* <span data-ttu-id="dc373-113">**Biztonsági mentési házirend** vagy **kötet** – hello biztonsági mentési házirend, vagy a kötet a biztonságimásolat-készlet társítva.</span><span class="sxs-lookup"><span data-stu-id="dc373-113">**Backup policy** or **volume** – hello backup policy or volume associated with this backup set.</span></span>
* <span data-ttu-id="dc373-114">**A** és **való** – dátum és idő tartomány hello hello biztonságimásolat-készlet létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="dc373-114">**From** and **To** – hello date and time range when hello backup set was created.</span></span>

<span data-ttu-id="dc373-115">hello szűrt biztonságimásolat-készletek majd megjelennének hello a következő attribútumok alapján:</span><span class="sxs-lookup"><span data-stu-id="dc373-115">hello filtered backup sets are then tabulated based on hello following attributes:</span></span>

* <span data-ttu-id="dc373-116">**Név** – hello hello biztonsági mentési házirend vagy a kötet társított hello biztonságimásolat-készlet neve.</span><span class="sxs-lookup"><span data-stu-id="dc373-116">**Name** – hello name of hello backup policy or volume associated with hello backup set.</span></span>
* <span data-ttu-id="dc373-117">**Méret** – hello hello biztonságimásolat-készlet tényleges mérete.</span><span class="sxs-lookup"><span data-stu-id="dc373-117">**Size** – hello actual size of hello backup set.</span></span>
* <span data-ttu-id="dc373-118">**A létrehozott** – hello dátum és idő, amikor hello létrehozásuk.</span><span class="sxs-lookup"><span data-stu-id="dc373-118">**Created on** – hello date and time when hello backups were created.</span></span> 
* <span data-ttu-id="dc373-119">**Típus** – biztonsági másolat beállítása helyi pillanatképeket és felhőalapú pillanatfelvételek.</span><span class="sxs-lookup"><span data-stu-id="dc373-119">**Type** – Backup sets can be local snapshots or cloud snapshots.</span></span> <span data-ttu-id="dc373-120">Egy helyi pillanatfelvétel a köteten tárolt összes adat helyileg hello eszköz, biztonsági másolatot, mivel egy felhőalapú pillanatfelvétel toohello az adatok biztonsági mentése kötet hello felhőben található.</span><span class="sxs-lookup"><span data-stu-id="dc373-120">A local snapshot is a backup of all your volume data stored locally on hello device, whereas a cloud snapshot refers toohello backup of volume data residing in hello cloud.</span></span> <span data-ttu-id="dc373-121">Helyi pillanatképeket gyorsabb hozzáférést biztosít, mivel felhőalapú pillanatfelvételek az adatrugalmasság közül választ.</span><span class="sxs-lookup"><span data-stu-id="dc373-121">Local snapshots provide faster access, whereas cloud snapshots are chosen for data resiliency.</span></span>
* <span data-ttu-id="dc373-122">**Által kezdeményezett** – hello biztonsági mentések kezdeményezhetők, automatikusan a tooa ütemezés szerint vagy manuálisan felhasználója.</span><span class="sxs-lookup"><span data-stu-id="dc373-122">**Initiated by** – hello backups can be initiated automatically according tooa schedule or manually by a user.</span></span> <span data-ttu-id="dc373-123">(A biztonsági mentési házirend tooschedule biztonsági mentéseket is használható.</span><span class="sxs-lookup"><span data-stu-id="dc373-123">(You can use a backup policy tooschedule backups.</span></span> <span data-ttu-id="dc373-124">Másik lehetőségként használhatja a hello **biztonsági másolatok készítéséhez** beállítás tootake egy interaktív biztonsági mentést.)</span><span class="sxs-lookup"><span data-stu-id="dc373-124">Alternatively, you can use hello **Take backup** option tootake an interactive backup.)</span></span>

## <a name="how-toorestore-your-storsimple-volume-from-a-backup"></a><span data-ttu-id="dc373-125">Hogyan toorestore a StorSimple-kötet egy biztonsági másolatból</span><span class="sxs-lookup"><span data-stu-id="dc373-125">How toorestore your StorSimple volume from a backup</span></span>
<span data-ttu-id="dc373-126">Használhatja a hello **biztonságimásolat-katalógus** lapon toorestore a StorSimple-kötet egy adott biztonsági másolatból.</span><span class="sxs-lookup"><span data-stu-id="dc373-126">You can use hello **Backup Catalog** page toorestore your StorSimple volume from a specific backup.</span></span> 

> [!WARNING]
> <span data-ttu-id="dc373-127">Egy biztonsági másolatból történő visszaállítását lecseréli a meglévő kötetek hello hello biztonsági másolatból.</span><span class="sxs-lookup"><span data-stu-id="dc373-127">Restoring from a backup will replace hello existing volumes from hello backup.</span></span> <span data-ttu-id="dc373-128">Ezt követően hello a biztonsági mentés készült írt adatok hello adatvesztést okozhat.</span><span class="sxs-lookup"><span data-stu-id="dc373-128">This may cause hello loss of any data that was written after hello backup was taken.</span></span>
> 
> 

<span data-ttu-id="dc373-129">Egy olyan köteten a visszaállítási elindítása előtt győződjön meg arról, hogy hello kötet offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="dc373-129">Before you initiate a restore on a volume, ensure that hello volume is offline.</span></span> <span data-ttu-id="dc373-130">Először kell először tootake hello kötet offline hello állomáson, majd az eszköz hello.</span><span class="sxs-lookup"><span data-stu-id="dc373-130">You will need tootake hello volume offline on hello host first and then hello device.</span></span> <span data-ttu-id="dc373-131">Hello kövesse [kötet offline állapotba](storsimple-manage-volumes.md#take-a-volume-offline).</span><span class="sxs-lookup"><span data-stu-id="dc373-131">Follow hello steps in [Take a volume offline](storsimple-manage-volumes.md#take-a-volume-offline).</span></span> <span data-ttu-id="dc373-132">Hajtsa végre a következő lépéseket toorestore egy kötet a biztonságimásolat-készletből hello.</span><span class="sxs-lookup"><span data-stu-id="dc373-132">Perform hello following steps toorestore a volume from a backup set.</span></span>

### <a name="toorestore-from-a-backup-set"></a><span data-ttu-id="dc373-133">a biztonságimásolat-készletből toorestore</span><span class="sxs-lookup"><span data-stu-id="dc373-133">toorestore from a backup set</span></span>
1. <span data-ttu-id="dc373-134">A hello StorSimple Manager szolgáltatás lapján, kattintson a hello **biztonságimásolat-katalógus** fülre.</span><span class="sxs-lookup"><span data-stu-id="dc373-134">On hello StorSimple Manager service page, click hello **Backup catalog** tab.</span></span>
   
    ![Biztonságimásolat-katalógus](./media/storsimple-restore-from-backup-set/HCS_Restore.png)
2. <span data-ttu-id="dc373-136">Válassza ki a biztonságimásolat-készletet a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="dc373-136">Select a backup set as follows:</span></span>
   
   1. <span data-ttu-id="dc373-137">Válassza ki azt a hello megfelelő eszközt.</span><span class="sxs-lookup"><span data-stu-id="dc373-137">Select hello appropriate device.</span></span>
   2. <span data-ttu-id="dc373-138">Hello legördülő listában válassza ki hello kötet vagy a biztonsági mentési házirendet hello biztonsági mentés, hogy kívánja-e tooselect.</span><span class="sxs-lookup"><span data-stu-id="dc373-138">In hello drop-down list, choose hello volume or backup policy for hello backup that you wish tooselect.</span></span>
   3. <span data-ttu-id="dc373-139">Adjon meg hello időtartományt.</span><span class="sxs-lookup"><span data-stu-id="dc373-139">Specify hello time range.</span></span>
   4. <span data-ttu-id="dc373-140">Kattintson a pipa ikonra hello</span><span class="sxs-lookup"><span data-stu-id="dc373-140">Click hello check icon</span></span> ![pipa ikon](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) <span data-ttu-id="dc373-142">tooexecute ezt a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="dc373-142">tooexecute this query.</span></span>
      
      <span data-ttu-id="dc373-143">hello hello kijelölt kötet vagy a biztonsági mentési házirenddel társított biztonsági másolatok meg kell jelennie hello listájában biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="dc373-143">hello backups associated with hello selected volume or backup policy should appear in hello list of backup sets.</span></span>
3. <span data-ttu-id="dc373-144">Bontsa ki a hello biztonságimásolat-készlet tooview hello társított kötetek.</span><span class="sxs-lookup"><span data-stu-id="dc373-144">Expand hello backup set tooview hello associated volumes.</span></span> <span data-ttu-id="dc373-145">Ezek a kötetek offline állapotba kell helyezni a hello állomás és az eszköz visszaállítani őket.</span><span class="sxs-lookup"><span data-stu-id="dc373-145">These volumes must be taken offline on hello host and device before you can restore them.</span></span> <span data-ttu-id="dc373-146">Hello kövesse [kötet offline állapotba](storsimple-manage-volumes.md#take-a-volume-offline).</span><span class="sxs-lookup"><span data-stu-id="dc373-146">Follow hello steps in [Take a volume offline](storsimple-manage-volumes.md#take-a-volume-offline).</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="dc373-147">Győződjön meg arról, hogy elvégezte hello kötetek offline hello gazdagépen először hello eszközön hello kötetek offline végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="dc373-147">Make sure that you have taken hello volumes offline on hello host first, before you take hello volumes offline on hello device.</span></span> <span data-ttu-id="dc373-148">Ha nem hajtja végre hello kötetek offline hello állomáson, veremtúlcsordulást okozhat toodata sérülése.</span><span class="sxs-lookup"><span data-stu-id="dc373-148">If you do not take hello volumes offline on hello host, it could potentially lead toodata corruption.</span></span>
   > 
   > 
4. <span data-ttu-id="dc373-149">Válassza ki a biztonságimásolat-készletből.</span><span class="sxs-lookup"><span data-stu-id="dc373-149">Select a backup set.</span></span> <span data-ttu-id="dc373-150">Kattintson a **visszaállítása** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="dc373-150">Click **Restore** at hello bottom of hello page.</span></span>
5. <span data-ttu-id="dc373-151">Megerősítést kér bekéri.</span><span class="sxs-lookup"><span data-stu-id="dc373-151">You will be prompted for confirmation.</span></span> 
   
    ![Jóváhagyás lap](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)
6. <span data-ttu-id="dc373-153">Hello visszaállítási információk áttekintéséhez, és kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png).</span><span class="sxs-lookup"><span data-stu-id="dc373-153">Review hello restore information and click hello check icon ![check icon](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png).</span></span> <span data-ttu-id="dc373-154">A szolgáltatás kezdeményez egy visszaállítási feladat, amelyet látni lehet hello elérésével **feladatok** lap.</span><span class="sxs-lookup"><span data-stu-id="dc373-154">This will initiate a restore job that you can view by accessing hello **Jobs** page.</span></span> 
7. <span data-ttu-id="dc373-155">Hello visszaállítás befejezése után ellenőrizheti, hogy a kötetek hello tartalmát helyébe kötetek hello biztonsági másolatból.</span><span class="sxs-lookup"><span data-stu-id="dc373-155">After hello restore is complete, you can verify that hello contents of your volumes are replaced by volumes from hello backup.</span></span>

<span data-ttu-id="dc373-156">![Videó elérhető](./media/storsimple-restore-from-backup-set/Video_icon.png)**Videó elérhető**</span><span class="sxs-lookup"><span data-stu-id="dc373-156">![Video available](./media/storsimple-restore-from-backup-set/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="dc373-157">a videó bemutatja, hogyan használja a hello klónozást, és állítsa vissza a StorSimple toorecover törölt fájlok, a szolgáltatások toowatch kattintson [Itt](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span><span class="sxs-lookup"><span data-stu-id="dc373-157">toowatch a video that demonstrates how you can use hello clone and restore features in StorSimple toorecover deleted files, click [here](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc373-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dc373-158">Next steps</span></span>
* <span data-ttu-id="dc373-159">Ismerje meg, hogyan túl[kezelése StorSimple-köteteket](storsimple-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="dc373-159">Learn how too[Manage StorSimple volumes](storsimple-manage-volumes.md).</span></span>
* <span data-ttu-id="dc373-160">Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="dc373-160">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

