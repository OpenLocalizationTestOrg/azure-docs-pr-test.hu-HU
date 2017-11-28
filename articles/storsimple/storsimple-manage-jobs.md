---
title: "aaaView és a StorSimple-feladatok kezelése |} Microsoft Docs"
description: "Hello StorSimple Manager szolgáltatás feladatok lapján ismerteti, hogyan toouse azt tootrack újabb, aktuális és ütemezett biztonsági mentési feladatok."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 55922cd0-d490-48eb-938a-012a67c1c09e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: b7341270e37a9f2530a8da1fb7c54f6b699c699c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooview-and-manage-storsimple-jobs"></a><span data-ttu-id="bc340-103">Hello StorSimple Manager szolgáltatás tooview használja, és a StorSimple-feladatok kezelése</span><span class="sxs-lookup"><span data-stu-id="bc340-103">Use hello StorSimple Manager service tooview and manage StorSimple jobs</span></span>
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a><span data-ttu-id="bc340-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="bc340-104">Overview</span></span>
<span data-ttu-id="bc340-105">Hello **feladatok** lap egy egyetlen központi portált biztosít a megtekintése, és a feladatok az eszközökön az elindított kezelése csatlakoztatott tooyour StorSimple Manager szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="bc340-105">hello **Jobs** page provides a single central portal for viewing and managing jobs that were started on devices connected tooyour StorSimple Manager service.</span></span> <span data-ttu-id="bc340-106">Több eszköz ütemezett, futó, befejezett, és a sikertelen feladatok tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="bc340-106">You can view scheduled, running, completed, and failed jobs for multiple devices.</span></span> <span data-ttu-id="bc340-107">Eredmények táblázatos formában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="bc340-107">Results are presented in a tabular format.</span></span> 

![Feladatok lap](./media/storsimple-manage-jobs/HCS_JobsPage.png)

<span data-ttu-id="bc340-109">Gyorsan megtalálhatja olyan mezőkön, például a szűréssel érdeklődik hello feladatok:</span><span class="sxs-lookup"><span data-stu-id="bc340-109">You can quickly find hello jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="bc340-110">**Állapot** – futtathat feladatokat, ütemezett, sikertelen, befejezett, zároló vagy visszavont.</span><span class="sxs-lookup"><span data-stu-id="bc340-110">**Status** – Jobs can be running, scheduled, failed, completed, canceling, or canceled.</span></span>
* <span data-ttu-id="bc340-111">**Típus** – feladatok is létrehozható egy ütemezett vagy egy igény szerinti biztonsági mentést (**érvénybe biztonsági mentési**), a klónozás, eszköz-visszaállítási vagy frissítési művelet.</span><span class="sxs-lookup"><span data-stu-id="bc340-111">**Type** – Jobs can be created as a result of a scheduled or an on-demand backup (**Take Backup**), cloning, a device restore, or an update operation.</span></span>
* <span data-ttu-id="bc340-112">**Eszközök** – feladatok kezdeményezett egy bizonyos eszköz csatlakoztatásakor tooyour szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="bc340-112">**Devices** – Jobs are initiated on a certain device connected tooyour service.</span></span>
* <span data-ttu-id="bc340-113">**A kezdő és a** – feladatok alapján szűrt hello dátum és idő tartomány lehet.</span><span class="sxs-lookup"><span data-stu-id="bc340-113">**From and To** – Jobs can be filtered based on hello date and time range.</span></span>

<span data-ttu-id="bc340-114">hello szűrt feladatok majd megjelennének a következő attribútumok hello hello alapján:</span><span class="sxs-lookup"><span data-stu-id="bc340-114">hello filtered jobs are then tabulated on hello basis of hello following attributes:</span></span>

* <span data-ttu-id="bc340-115">**Típus** – biztonsági másolat, klónozás, visszaállítási, feladatátvétel vagy frissítés.</span><span class="sxs-lookup"><span data-stu-id="bc340-115">**Type** – Backup, clone, restore, failover, or update.</span></span>
* <span data-ttu-id="bc340-116">**Állapot** – futó, ütemezett, sikertelen, befejeződött, megszakítása vagy megszakítva.</span><span class="sxs-lookup"><span data-stu-id="bc340-116">**Status** – Running, scheduled, failed, completed, canceling, or canceled.</span></span>
* <span data-ttu-id="bc340-117">**Entitás** – hello feladatok is társítható egy köteten, hogy a biztonsági mentési házirend, vagy eszköz.</span><span class="sxs-lookup"><span data-stu-id="bc340-117">**Entity** – hello jobs can be associated with a volume, a backup policy, or a device.</span></span> <span data-ttu-id="bc340-118">A Klónozás feladat tartozik egy köteten, mivel a biztonsági mentési házirend társítva egy ütemezett biztonsági mentési feladat.</span><span class="sxs-lookup"><span data-stu-id="bc340-118">A clone job is associated with a volume, whereas a scheduled backup job is associated with a backup policy.</span></span> <span data-ttu-id="bc340-119">Eszköz feladat jön létre a vészhelyreállítás (DR) vagy a visszaállítási művelet miatt.</span><span class="sxs-lookup"><span data-stu-id="bc340-119">A device job is created as a result of a disaster recovery (DR) or a restore operation.</span></span>
* <span data-ttu-id="bc340-120">**Eszköz** – hello mely hello feladat el lett indítva hello eszköz nevét.</span><span class="sxs-lookup"><span data-stu-id="bc340-120">**Device** – hello name of hello device on which hello job was started.</span></span>
* <span data-ttu-id="bc340-121">**Megkezdődött a** – hello idő, amikor hello feladat el lett indítva.</span><span class="sxs-lookup"><span data-stu-id="bc340-121">**Started On** – hello time when hello job was started.</span></span>
* <span data-ttu-id="bc340-122">**Folyamatban lévő** – hello százalékos egy futó feladat befejezésére.</span><span class="sxs-lookup"><span data-stu-id="bc340-122">**Progress** – hello percentage completion of a running job.</span></span> <span data-ttu-id="bc340-123">A projekt mindig legyen 100 %.</span><span class="sxs-lookup"><span data-stu-id="bc340-123">For a completed job, this should always be 100%.</span></span>

<span data-ttu-id="bc340-124">feladatok hello listája 30 másodpercenként frissül.</span><span class="sxs-lookup"><span data-stu-id="bc340-124">hello list of jobs is refreshed every 30 seconds.</span></span>

<span data-ttu-id="bc340-125">Ezen a lapon a feladathoz kapcsolódó műveletek következő hello végezheti el:</span><span class="sxs-lookup"><span data-stu-id="bc340-125">You can perform hello following job-related actions on this page:</span></span>

* <span data-ttu-id="bc340-126">Feladatok részleteinek megjelenítése</span><span class="sxs-lookup"><span data-stu-id="bc340-126">View job details</span></span>
* <span data-ttu-id="bc340-127">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="bc340-127">Cancel a job</span></span>

## <a name="view-job-details"></a><span data-ttu-id="bc340-128">Feladatok részleteinek megjelenítése</span><span class="sxs-lookup"><span data-stu-id="bc340-128">View job details</span></span>
<span data-ttu-id="bc340-129">Hajtsa végre a következő lépéseket tooview hello adatok bármely feladat hello.</span><span class="sxs-lookup"><span data-stu-id="bc340-129">Perform hello following steps tooview hello details of any job.</span></span>

#### <a name="tooview-job-details"></a><span data-ttu-id="bc340-130">tooview feladat részletei</span><span class="sxs-lookup"><span data-stu-id="bc340-130">tooview job details</span></span>
1. <span data-ttu-id="bc340-131">A hello **feladatok** lapon, a megfelelő szűrőket és egy lekérdezés futtatásával érdeklődik hello feladat megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="bc340-131">On hello **Jobs** page, display hello job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="bc340-132">Kereshet befejezése után fut, és nem vonható vissza a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="bc340-132">You can search for completed, running, or canceled jobs.</span></span>
2. <span data-ttu-id="bc340-133">Jelölje ki a feladatot.</span><span class="sxs-lookup"><span data-stu-id="bc340-133">Select a job.</span></span>
3. <span data-ttu-id="bc340-134">Hello a hello lap alján, kattintson **részletek**.</span><span class="sxs-lookup"><span data-stu-id="bc340-134">At hello bottom of hello page, click **Details**.</span></span>
4. <span data-ttu-id="bc340-135">A hello **biztonsági mentési feladat részleteit** párbeszédpanelen hello állapota, részleteit, idő statisztikák és adatok statisztika megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="bc340-135">In hello **Backup Job Details** dialog box, you can view hello status, details, time statistics, and data statistics.</span></span>

## <a name="cancel-a-job"></a><span data-ttu-id="bc340-136">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="bc340-136">Cancel a job</span></span>
<span data-ttu-id="bc340-137">Hajtsa végre a következő lépéseket toocancel egy futó feladat hello.</span><span class="sxs-lookup"><span data-stu-id="bc340-137">Perform hello following steps toocancel a running job.</span></span>

### <a name="toocancel-a-job"></a><span data-ttu-id="bc340-138">egy feladat toocancel</span><span class="sxs-lookup"><span data-stu-id="bc340-138">toocancel a job</span></span>
1. <span data-ttu-id="bc340-139">A hello **feladatok** lapján hello futó feladat, amelyet az toocancel megfelelő szűrőket és egy lekérdezés futtatásával megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="bc340-139">On hello **Jobs** page, display hello running job(s) that you want toocancel by running a query with appropriate filters.</span></span>
2. <span data-ttu-id="bc340-140">Jelölje ki a hello feladatot.</span><span class="sxs-lookup"><span data-stu-id="bc340-140">Select hello job.</span></span>
3. <span data-ttu-id="bc340-141">Hello a hello lap alján, kattintson **Mégse**.</span><span class="sxs-lookup"><span data-stu-id="bc340-141">At hello bottom of hello page, click **Cancel**.</span></span>
4. <span data-ttu-id="bc340-142">Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc340-142">When prompted for confirmation, click **Yes**.</span></span>

<span data-ttu-id="bc340-143">Ez a feladat most megszakítva.</span><span class="sxs-lookup"><span data-stu-id="bc340-143">This job is now canceled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc340-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc340-144">Next steps</span></span>
* <span data-ttu-id="bc340-145">Ismerje meg, hogyan túl[a StorSimple biztonsági mentési házirendek kezelése](storsimple-manage-backup-policies.md).</span><span class="sxs-lookup"><span data-stu-id="bc340-145">Learn how too[manage your StorSimple backup policies](storsimple-manage-backup-policies.md).</span></span>
* <span data-ttu-id="bc340-146">Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="bc340-146">Learn how too[use hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

