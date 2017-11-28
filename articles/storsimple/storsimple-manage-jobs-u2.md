---
title: "aaaView és a StorSimple-feladatok kezelése |} Microsoft Docs"
description: "Hello StorSimple Manager szolgáltatás feladatok lapján ismerteti, hogyan toouse azt tootrack újabb, aktuális és ütemezett biztonsági mentési feladatok."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: cdd3e9e8-7ccd-40a3-8c07-0ab1338c0e59
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: d6ecdcbc3d8a4757c2328303f268e51c8ce26b65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooview-and-manage-storsimple-jobs-update-2"></a><span data-ttu-id="4bd64-103">Hello StorSimple Manager szolgáltatás tooview használatát és kezelését a StorSimple feladatok (2. frissítés)</span><span class="sxs-lookup"><span data-stu-id="4bd64-103">Use hello StorSimple Manager service tooview and manage StorSimple jobs (Update 2)</span></span>
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a><span data-ttu-id="4bd64-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4bd64-104">Overview</span></span>
<span data-ttu-id="4bd64-105">Hello **feladatok** lap egy egyetlen központi portált biztosít a megtekintése, és a feladatok az eszközökön az elindított kezelése csatlakoztatott tooyour StorSimple Manager szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4bd64-105">hello **Jobs** page provides a single central portal for viewing and managing jobs that were started on devices connected tooyour StorSimple Manager service.</span></span> <span data-ttu-id="4bd64-106">Több eszköz ütemezett, futó, befejezett, törölt és sikertelen feladatok tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="4bd64-106">You can view scheduled, running, completed, canceled, and failed jobs for multiple devices.</span></span> <span data-ttu-id="4bd64-107">Eredmények táblázatos formában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="4bd64-107">Results are presented in a tabular format.</span></span> 

![Feladatok lap](./media/storsimple-manage-jobs-u2/jobs.png)

<span data-ttu-id="4bd64-109">Gyorsan megtalálhatja olyan mezőkön, például a szűréssel érdeklődik hello feladatok:</span><span class="sxs-lookup"><span data-stu-id="4bd64-109">You can quickly find hello jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="4bd64-110">**Állapot** – futtathat feladatokat, befejeződött, megszakított, sikertelen, zároló vagy befejeződött, hibákkal.</span><span class="sxs-lookup"><span data-stu-id="4bd64-110">**Status** – Jobs can be running, completed, canceled, failed, canceling, or completed with errors.</span></span>
* <span data-ttu-id="4bd64-111">**A kezdő és a** – feladatok alapján szűrt hello dátum és idő tartomány lehet.</span><span class="sxs-lookup"><span data-stu-id="4bd64-111">**From and To** – Jobs can be filtered based on hello date and time range.</span></span>
* <span data-ttu-id="4bd64-112">**Típus** – hello feladattípus lehet biztonsági másolat, manuális biztonsági mentés, a visszaállítási, a klónozás, a eszköz feladatátvevő helyileg rögzített kötet létrehozásához, módosításához a kötetet, frissítés, támogatja a csomagot, vagy a virtuális eszköz kiépítése.</span><span class="sxs-lookup"><span data-stu-id="4bd64-112">**Type** – hello job type can be backup, manual backup, restore, clone, device failover, create locally pinned volume, modify volume, update, support package, or virtual device provisioning.</span></span>
* <span data-ttu-id="4bd64-113">**Eszközök** – feladatok kezdeményezett egy bizonyos eszköz csatlakoztatásakor tooyour szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="4bd64-113">**Devices** – Jobs are initiated on a certain device connected tooyour service.</span></span>
  <span data-ttu-id="4bd64-114">hello szűrt feladatok majd megjelennének a következő attribútumok hello hello alapján:</span><span class="sxs-lookup"><span data-stu-id="4bd64-114">hello filtered jobs are then tabulated on hello basis of hello following attributes:</span></span>
  
  * <span data-ttu-id="4bd64-115">**Típus** – biztonsági másolat, manuális biztonsági mentés, a visszaállítási, a klónozás, a eszköz feladatátvevő helyileg rögzített kötet létrehozásához, módosításához a kötetet, frissítés, támogatja a csomagot, vagy a virtuális eszköz kiépítése.</span><span class="sxs-lookup"><span data-stu-id="4bd64-115">**Type** – backup, manual backup, restore, clone, device failover, create locally pinned volume, modify volume, update, support package, or virtual device provisioning.</span></span>
  * <span data-ttu-id="4bd64-116">**Állapot** – futó, befejeződött, megszakított, sikertelen, zároló vagy befejeződött, hibákkal.</span><span class="sxs-lookup"><span data-stu-id="4bd64-116">**Status** – running, completed, canceled, failed, canceling, or completed with errors.</span></span>
  * <span data-ttu-id="4bd64-117">**Entitás** – hello feladatok is társítható egy köteten, hogy a biztonsági mentési házirend, vagy eszköz.</span><span class="sxs-lookup"><span data-stu-id="4bd64-117">**Entity** – hello jobs can be associated with a volume, a backup policy, or a device.</span></span> <span data-ttu-id="4bd64-118">Például a Klónozás feladat társítva egy köteten, mivel a biztonsági mentési házirend ütemezett biztonsági mentési feladat tartozik.</span><span class="sxs-lookup"><span data-stu-id="4bd64-118">For example, a clone job is associated with a volume, whereas a scheduled backup job is associated with a backup policy.</span></span> <span data-ttu-id="4bd64-119">Eszköz feladat jön létre a vészhelyreállítás (DR) vagy a visszaállítási művelet miatt.</span><span class="sxs-lookup"><span data-stu-id="4bd64-119">A device job is created as a result of a disaster recovery (DR) or a restore operation.</span></span>
  * <span data-ttu-id="4bd64-120">**Eszköz** – hello mely hello feladat el lett indítva hello eszköz nevét.</span><span class="sxs-lookup"><span data-stu-id="4bd64-120">**Device** – hello name of hello device on which hello job was started.</span></span>
  * <span data-ttu-id="4bd64-121">**Megkezdődött a** – hello idő, amikor hello feladat el lett indítva.</span><span class="sxs-lookup"><span data-stu-id="4bd64-121">**Started on** – hello time when hello job was started.</span></span>
  * <span data-ttu-id="4bd64-122">**Folyamatban lévő** – hello százalékos egy futó feladat befejezésére.</span><span class="sxs-lookup"><span data-stu-id="4bd64-122">**Progress** – hello percentage completion of a running job.</span></span> <span data-ttu-id="4bd64-123">A projekt mindig legyen 100 %.</span><span class="sxs-lookup"><span data-stu-id="4bd64-123">For a completed job, this should always be 100%.</span></span>

<span data-ttu-id="4bd64-124">feladatok hello listája 30 másodpercenként frissül.</span><span class="sxs-lookup"><span data-stu-id="4bd64-124">hello list of jobs is refreshed every 30 seconds.</span></span>

<span data-ttu-id="4bd64-125">Ezen a lapon a feladathoz kapcsolódó műveletek következő hello végezheti el:</span><span class="sxs-lookup"><span data-stu-id="4bd64-125">You can perform hello following job-related actions on this page:</span></span>

* <span data-ttu-id="4bd64-126">Feladatok részleteinek megjelenítése</span><span class="sxs-lookup"><span data-stu-id="4bd64-126">View job details</span></span>
* <span data-ttu-id="4bd64-127">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="4bd64-127">Cancel a job</span></span>

## <a name="view-job-details"></a><span data-ttu-id="4bd64-128">Feladatok részleteinek megjelenítése</span><span class="sxs-lookup"><span data-stu-id="4bd64-128">View job details</span></span>
<span data-ttu-id="4bd64-129">Hajtsa végre a következő lépéseket tooview hello adatok bármely feladat hello.</span><span class="sxs-lookup"><span data-stu-id="4bd64-129">Perform hello following steps tooview hello details of any job.</span></span>

#### <a name="tooview-job-details"></a><span data-ttu-id="4bd64-130">tooview feladat részletei</span><span class="sxs-lookup"><span data-stu-id="4bd64-130">tooview job details</span></span>
1. <span data-ttu-id="4bd64-131">A hello **feladatok** lapon, a megfelelő szűrőket és egy lekérdezés futtatásával érdeklődik hello feladat megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="4bd64-131">On hello **Jobs** page, display hello job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="4bd64-132">Kereshet befejezése után fut, és nem vonható vissza a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="4bd64-132">You can search for completed, running, or canceled jobs.</span></span>
2. <span data-ttu-id="4bd64-133">Jelölje ki a feladatot.</span><span class="sxs-lookup"><span data-stu-id="4bd64-133">Select a job.</span></span>
3. <span data-ttu-id="4bd64-134">Hello a hello lap alján, kattintson **részletek**.</span><span class="sxs-lookup"><span data-stu-id="4bd64-134">At hello bottom of hello page, click **Details**.</span></span>
4. <span data-ttu-id="4bd64-135">A hello **biztonsági mentési feladat részleteit** párbeszédpanelen hello állapota, részleteit, idő statisztikák és adatok statisztika megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="4bd64-135">In hello **Backup Job Details** dialog box, you can view hello status, details, time statistics, and data statistics.</span></span>
   
    ![Feladat részletei lap](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a><span data-ttu-id="4bd64-137">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="4bd64-137">Cancel a job</span></span>
<span data-ttu-id="4bd64-138">Hajtsa végre a következő lépéseket toocancel egy futó feladat hello.</span><span class="sxs-lookup"><span data-stu-id="4bd64-138">Perform hello following steps toocancel a running job.</span></span>

> [!NOTE]
> <span data-ttu-id="4bd64-139">Nem lehet megszakítani a más feladatok, például egy kötet toochange hello kötettípus módosítása vagy a kötet kiterjesztését.</span><span class="sxs-lookup"><span data-stu-id="4bd64-139">Some jobs, such as modifying a volume toochange hello volume type or expanding a volume, cannot be canceled.</span></span>
> 
> 

### <a name="toocancel-a-job"></a><span data-ttu-id="4bd64-140">egy feladat toocancel</span><span class="sxs-lookup"><span data-stu-id="4bd64-140">toocancel a job</span></span>
1. <span data-ttu-id="4bd64-141">A hello **feladatok** lapján hello futó feladat, amelyet az toocancel megfelelő szűrőket és egy lekérdezés futtatásával megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4bd64-141">On hello **Jobs** page, display hello running job(s) that you want toocancel by running a query with appropriate filters.</span></span>
2. <span data-ttu-id="4bd64-142">Jelölje ki a hello feladatot.</span><span class="sxs-lookup"><span data-stu-id="4bd64-142">Select hello job.</span></span>
3. <span data-ttu-id="4bd64-143">Hello a hello lap alján, kattintson **Mégse**.</span><span class="sxs-lookup"><span data-stu-id="4bd64-143">At hello bottom of hello page, click **Cancel**.</span></span>
4. <span data-ttu-id="4bd64-144">Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.</span><span class="sxs-lookup"><span data-stu-id="4bd64-144">When prompted for confirmation, click **Yes**.</span></span>

<span data-ttu-id="4bd64-145">Ez a feladat most megszakítva.</span><span class="sxs-lookup"><span data-stu-id="4bd64-145">This job is now canceled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4bd64-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4bd64-146">Next steps</span></span>
* <span data-ttu-id="4bd64-147">Ismerje meg, hogyan túl[a StorSimple biztonsági mentési házirendek kezelése](storsimple-manage-backup-policies.md).</span><span class="sxs-lookup"><span data-stu-id="4bd64-147">Learn how too[manage your StorSimple backup policies](storsimple-manage-backup-policies.md).</span></span>
* <span data-ttu-id="4bd64-148">Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="4bd64-148">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

