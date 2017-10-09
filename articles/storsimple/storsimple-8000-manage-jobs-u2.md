---
title: "aaaView és a StorSimple 8000 Series feladatok kezelése |} Microsoft Docs"
description: "Hello StorSimple Device Manager szolgáltatás feladatok panelen ismerteti, hogyan toouse azt tootrack újabb, aktuális és ütemezett biztonsági mentési feladatok."
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
ms.openlocfilehash: a76acd67e2568478dd43d0fb16c1ae2dfb72a23d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooview-and-manage-jobs-update-3-and-later"></a><span data-ttu-id="94ec2-103">Hello StorSimple Device Manager szolgáltatás tooview használatát és kezelését a feladatok (Update 3 és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="94ec2-103">Use hello StorSimple Device Manager service tooview and manage jobs (Update 3 and later)</span></span>

## <a name="overview"></a><span data-ttu-id="94ec2-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="94ec2-104">Overview</span></span>
<span data-ttu-id="94ec2-105">Hello **feladatok** panel egy egyetlen központi portált biztosít a megtekintése, és a feladatok az eszközökön az elindított kezelése csatlakoztatott tooyour StorSimple Device Manager szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="94ec2-105">hello **Jobs** blade provides a single central portal for viewing and managing jobs that were started on devices connected tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="94ec2-106">Több eszköz ütemezett, futó, befejezett, törölt és sikertelen feladatok tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="94ec2-106">You can view scheduled, running, completed, canceled, and failed jobs for multiple devices.</span></span> <span data-ttu-id="94ec2-107">Eredmények táblázatos formában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="94ec2-107">Results are presented in a tabular format.</span></span>

![Feladatok panelen](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

<span data-ttu-id="94ec2-109">Gyorsan megtalálhatja olyan mezőkön, például a szűréssel érdeklődik hello feladatok:</span><span class="sxs-lookup"><span data-stu-id="94ec2-109">You can quickly find hello jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="94ec2-110">**Állapot** – feladatok lehet folyamatban, sikeresen befejeződött, megszakított, sikertelen, zároló vagy sikeres, a hibák.</span><span class="sxs-lookup"><span data-stu-id="94ec2-110">**Status** – Jobs can be in progress, succeeded, canceled, failed, canceling, or succeeded with errors.</span></span>
* <span data-ttu-id="94ec2-111">**Időtartománynak** – feladatok alapján szűrt hello dátum és idő tartomány lehet.</span><span class="sxs-lookup"><span data-stu-id="94ec2-111">**Time range** – Jobs can be filtered based on hello date and time range.</span></span> <span data-ttu-id="94ec2-112">hello tartomány: 1 óra, az elmúlt 24 óra múlva elmúlt 7 nap elmúlt 30 nap, az elmúlt év, vagy egyéni dátuma múltbeli.</span><span class="sxs-lookup"><span data-stu-id="94ec2-112">hello ranges are past 1 hour, past 24 hours, past 7 days, past 30 days, past year, or custom date.</span></span>
* <span data-ttu-id="94ec2-113">**Típus** – hello feladattípus lehet ütemezett biztonsági mentést, a manuális biztonsági mentés, a biztonsági mentés visszaállítási, kötet klónozása, kötettárolók feladatátvételt, helyileg rögzített kötet létrehozása, módosítása kötet, telepítse a frissítéseket, támogatási gyűjtését és felhő készülék létrehozása.</span><span class="sxs-lookup"><span data-stu-id="94ec2-113">**Type** – hello job type can be scheduled backup, manual backup, restore backup, clone volume, fail over volume containers, create locally-pinned volume, modify volume, install updates, collect support logs and create cloud appliance.</span></span>
* <span data-ttu-id="94ec2-114">**Eszközök** – feladatok kezdeményezett egy bizonyos eszköz csatlakoztatásakor tooyour szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="94ec2-114">**Devices** – Jobs are initiated on a certain device connected tooyour service.</span></span>
  
<span data-ttu-id="94ec2-115">hello szűrt feladatok majd megjelennének a következő attribútumok hello hello alapján:</span><span class="sxs-lookup"><span data-stu-id="94ec2-115">hello filtered jobs are then tabulated on hello basis of hello following attributes:</span></span>
  
* <span data-ttu-id="94ec2-116">**Név** – ütemezett biztonsági mentést, manuális biztonsági mentés, visszaállítás biztonsági mentés, klónozás köteten, a feladatátvétel kötettárolók, helyileg rögzített kötet létrehozása, módosítása a kötet, telepítse a frissítéseket, támogatási gyűjtését, vagy hozzon létre felhő készülék.</span><span class="sxs-lookup"><span data-stu-id="94ec2-116">**Name** – scheduled backup, manual backup, restore backup, clone volume, fail over volume containers, create locally pinned volume, modify volume, install updates, collect support logs, or create cloud appliance.</span></span>
* <span data-ttu-id="94ec2-117">**Állapot** – futó, befejeződött, megszakított, sikertelen, zároló vagy befejeződött, hibákkal.</span><span class="sxs-lookup"><span data-stu-id="94ec2-117">**Status** – running, completed, canceled, failed, canceling, or completed with errors.</span></span>
* <span data-ttu-id="94ec2-118">**Entitás** – hello feladatok is társítható egy köteten, hogy a biztonsági mentési házirend, vagy eszköz.</span><span class="sxs-lookup"><span data-stu-id="94ec2-118">**Entity** – hello jobs can be associated with a volume, a backup policy, or a device.</span></span> <span data-ttu-id="94ec2-119">Például a Klónozás feladat társítva egy köteten, mivel a biztonsági mentési házirend ütemezett biztonsági mentési feladat tartozik.</span><span class="sxs-lookup"><span data-stu-id="94ec2-119">For example, a clone job is associated with a volume, whereas a scheduled backup job is associated with a backup policy.</span></span> <span data-ttu-id="94ec2-120">Eszköz feladat jön létre a vészhelyreállítás (DR) vagy a visszaállítási művelet miatt.</span><span class="sxs-lookup"><span data-stu-id="94ec2-120">A device job is created as a result of a disaster recovery (DR) or a restore operation.</span></span>
* <span data-ttu-id="94ec2-121">**Eszköz** – hello mely hello feladat el lett indítva hello eszköz nevét.</span><span class="sxs-lookup"><span data-stu-id="94ec2-121">**Device** – hello name of hello device on which hello job was started.</span></span>
* <span data-ttu-id="94ec2-122">**Megkezdődött a** – hello idő, amikor hello feladat el lett indítva.</span><span class="sxs-lookup"><span data-stu-id="94ec2-122">**Started on** – hello time when hello job was started.</span></span>
* <span data-ttu-id="94ec2-123">**Időtartam** – hello idő szükséges toocomplete hello feladat.</span><span class="sxs-lookup"><span data-stu-id="94ec2-123">**Duration** – hello time required toocomplete hello job.</span></span>

<span data-ttu-id="94ec2-124">feladatok hello listája 30 másodpercenként frissül.</span><span class="sxs-lookup"><span data-stu-id="94ec2-124">hello list of jobs is refreshed every 30 seconds.</span></span>

<span data-ttu-id="94ec2-125">Ezen a lapon a feladathoz kapcsolódó műveletek következő hello végezheti el:</span><span class="sxs-lookup"><span data-stu-id="94ec2-125">You can perform hello following job-related actions on this page:</span></span>

* <span data-ttu-id="94ec2-126">Feladatok részleteinek megjelenítése</span><span class="sxs-lookup"><span data-stu-id="94ec2-126">View job details</span></span>
* <span data-ttu-id="94ec2-127">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="94ec2-127">Cancel a job</span></span>

## <a name="view-job-details"></a><span data-ttu-id="94ec2-128">Feladatok részleteinek megjelenítése</span><span class="sxs-lookup"><span data-stu-id="94ec2-128">View job details</span></span>
<span data-ttu-id="94ec2-129">Hajtsa végre a következő lépéseket tooview hello adatok bármely feladat hello.</span><span class="sxs-lookup"><span data-stu-id="94ec2-129">Perform hello following steps tooview hello details of any job.</span></span>

#### <a name="tooview-job-details"></a><span data-ttu-id="94ec2-130">tooview feladat részletei</span><span class="sxs-lookup"><span data-stu-id="94ec2-130">tooview job details</span></span>
1. <span data-ttu-id="94ec2-131">Nyissa meg tooyour StorSimple Device Manager szolgáltatást, és kattintson a **feladatok**.</span><span class="sxs-lookup"><span data-stu-id="94ec2-131">Go tooyour StorSimple Device Manager service and then click **Jobs**.</span></span>

2. <span data-ttu-id="94ec2-132">A hello **feladatok** panelen, a megfelelő szűrőket és egy lekérdezés futtatásával érdeklődik megjelenítési hello feladat.</span><span class="sxs-lookup"><span data-stu-id="94ec2-132">In hello **Jobs** blade, display hello job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="94ec2-133">Kereshet befejezése után fut, és nem vonható vissza a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="94ec2-133">You can search for completed, running, or canceled jobs.</span></span>

    ![Feladat panelen](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

2. <span data-ttu-id="94ec2-135">Válassza ki, majd kattintson egy feladatra.</span><span class="sxs-lookup"><span data-stu-id="94ec2-135">Select and click a job.</span></span>

    ![Feladat panelen](./media/storsimple-8000-manage-jobs-u2/jobs3.png)

3. <span data-ttu-id="94ec2-137">Hello feladat részleteit megjelenítő panelen megtekintheti hello állapota, részleteit, idő statisztikák és adatok statisztika.</span><span class="sxs-lookup"><span data-stu-id="94ec2-137">In hello job details blade, you can view hello status, details, time statistics, and data statistics.</span></span>
   
    ![Feladat részletei](./media/storsimple-8000-manage-jobs-u2/jobs4.png)

## <a name="cancel-a-job"></a><span data-ttu-id="94ec2-139">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="94ec2-139">Cancel a job</span></span>
<span data-ttu-id="94ec2-140">Hajtsa végre a következő lépéseket toocancel egy futó feladat hello.</span><span class="sxs-lookup"><span data-stu-id="94ec2-140">Perform hello following steps toocancel a running job.</span></span>

> [!NOTE]
> <span data-ttu-id="94ec2-141">Nem lehet megszakítani a más feladatok, például egy kötet toochange hello kötettípus módosítása vagy a kötet kiterjesztését.</span><span class="sxs-lookup"><span data-stu-id="94ec2-141">Some jobs, such as modifying a volume toochange hello volume type or expanding a volume, cannot be canceled.</span></span>


### <a name="toocancel-a-job"></a><span data-ttu-id="94ec2-142">egy feladat toocancel</span><span class="sxs-lookup"><span data-stu-id="94ec2-142">toocancel a job</span></span>
1. <span data-ttu-id="94ec2-143">A hello **feladatok** lapján hello futó feladat, amelyet az toocancel megfelelő szűrőket és egy lekérdezés futtatásával megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="94ec2-143">On hello **Jobs** page, display hello running job(s) that you want toocancel by running a query with appropriate filters.</span></span> <span data-ttu-id="94ec2-144">Jelölje ki a hello feladatot.</span><span class="sxs-lookup"><span data-stu-id="94ec2-144">Select hello job.</span></span>

2. <span data-ttu-id="94ec2-145">Hello kijelölt feladat tooinvoke hello helyi menüben kattintson a jobb gombbal, és kattintson a **Mégse**.</span><span class="sxs-lookup"><span data-stu-id="94ec2-145">Right-click on hello selected job tooinvoke hello context menu and click **Cancel**.</span></span>

    ![Feladat részletei](./media/storsimple-8000-manage-jobs-u2/jobs2.png)

3. <span data-ttu-id="94ec2-147">Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.</span><span class="sxs-lookup"><span data-stu-id="94ec2-147">When prompted for confirmation, click **Yes**.</span></span> <span data-ttu-id="94ec2-148">Ez a feladat most megszakítva.</span><span class="sxs-lookup"><span data-stu-id="94ec2-148">This job is now canceled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="94ec2-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="94ec2-149">Next steps</span></span>
* <span data-ttu-id="94ec2-150">Ismerje meg, hogyan túl[a StorSimple biztonsági mentési házirendek kezelése](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="94ec2-150">Learn how too[manage your StorSimple backup policies](storsimple-8000-manage-backup-policies-u2.md).</span></span>
* <span data-ttu-id="94ec2-151">Ismerje meg, hogyan túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="94ec2-151">Learn how too[use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

