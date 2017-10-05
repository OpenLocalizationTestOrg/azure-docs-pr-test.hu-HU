---
title: "Megtekintheti és kezelheti a StorSimple feladatok |} Microsoft Docs"
description: "A StorSimple Manager szolgáltatás feladatok lapján, és nyomon követheti az újabb, aktuális és ütemezett biztonsági mentési feladatok ismerteti."
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
ms.openlocfilehash: 6df1b27ce76de7a781ecc40af8430114d80b20d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs-update-2"></a><span data-ttu-id="e1eed-103">A StorSimple Manager szolgáltatás segítségével megtekintheti és kezelheti a StorSimple feladatok (2. frissítés)</span><span class="sxs-lookup"><span data-stu-id="e1eed-103">Use the StorSimple Manager service to view and manage StorSimple jobs (Update 2)</span></span>
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a><span data-ttu-id="e1eed-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e1eed-104">Overview</span></span>
<span data-ttu-id="e1eed-105">A **feladatok** lap egy egyetlen központi portált biztosít a megtekintése, és a StorSimple Manager szolgáltatás eszközök elindított feladatok kezelése kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="e1eed-105">The **Jobs** page provides a single central portal for viewing and managing jobs that were started on devices connected to your StorSimple Manager service.</span></span> <span data-ttu-id="e1eed-106">Több eszköz ütemezett, futó, befejezett, törölt és sikertelen feladatok tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="e1eed-106">You can view scheduled, running, completed, canceled, and failed jobs for multiple devices.</span></span> <span data-ttu-id="e1eed-107">Eredmények táblázatos formában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="e1eed-107">Results are presented in a tabular format.</span></span> 

![Feladatok lap](./media/storsimple-manage-jobs-u2/jobs.png)

<span data-ttu-id="e1eed-109">Gyorsan megtalálhatja a feladatok olyan mezőkön, például a szűréssel érdeklődik:</span><span class="sxs-lookup"><span data-stu-id="e1eed-109">You can quickly find the jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="e1eed-110">**Állapot** – futtathat feladatokat, befejeződött, megszakított, sikertelen, zároló vagy befejeződött, hibákkal.</span><span class="sxs-lookup"><span data-stu-id="e1eed-110">**Status** – Jobs can be running, completed, canceled, failed, canceling, or completed with errors.</span></span>
* <span data-ttu-id="e1eed-111">**A kezdő és a** – feladatok a dátum és idő tartomány alapján szűrhetők.</span><span class="sxs-lookup"><span data-stu-id="e1eed-111">**From and To** – Jobs can be filtered based on the date and time range.</span></span>
* <span data-ttu-id="e1eed-112">**Típus** – a feladat típusa biztonsági másolat, lehet manuális biztonsági mentés, a visszaállítási, a klónozás, a eszköz feladatátvevő helyileg rögzített kötet létrehozásához, módosításához a kötetet, frissítés, támogatja a csomagot, vagy a virtuális eszköz kiépítése.</span><span class="sxs-lookup"><span data-stu-id="e1eed-112">**Type** – The job type can be backup, manual backup, restore, clone, device failover, create locally pinned volume, modify volume, update, support package, or virtual device provisioning.</span></span>
* <span data-ttu-id="e1eed-113">**Eszközök** – feladatok kezdeményezett kapcsolódik a szolgáltatáshoz egy adott eszközön.</span><span class="sxs-lookup"><span data-stu-id="e1eed-113">**Devices** – Jobs are initiated on a certain device connected to your service.</span></span>
  <span data-ttu-id="e1eed-114">A szűrt feladatok majd megjelennének alapján a következő attribútumokat:</span><span class="sxs-lookup"><span data-stu-id="e1eed-114">The filtered jobs are then tabulated on the basis of the following attributes:</span></span>
  
  * <span data-ttu-id="e1eed-115">**Típus** – biztonsági másolat, manuális biztonsági mentés, a visszaállítási, a klónozás, a eszköz feladatátvevő helyileg rögzített kötet létrehozásához, módosításához a kötetet, frissítés, támogatja a csomagot, vagy a virtuális eszköz kiépítése.</span><span class="sxs-lookup"><span data-stu-id="e1eed-115">**Type** – backup, manual backup, restore, clone, device failover, create locally pinned volume, modify volume, update, support package, or virtual device provisioning.</span></span>
  * <span data-ttu-id="e1eed-116">**Állapot** – futó, befejeződött, megszakított, sikertelen, zároló vagy befejeződött, hibákkal.</span><span class="sxs-lookup"><span data-stu-id="e1eed-116">**Status** – running, completed, canceled, failed, canceling, or completed with errors.</span></span>
  * <span data-ttu-id="e1eed-117">**Entitás** – a feladat társíthatók a kötet, hogy a biztonsági mentési házirend, vagy eszköz.</span><span class="sxs-lookup"><span data-stu-id="e1eed-117">**Entity** – The jobs can be associated with a volume, a backup policy, or a device.</span></span> <span data-ttu-id="e1eed-118">Például a Klónozás feladat társítva egy köteten, mivel a biztonsági mentési házirend ütemezett biztonsági mentési feladat tartozik.</span><span class="sxs-lookup"><span data-stu-id="e1eed-118">For example, a clone job is associated with a volume, whereas a scheduled backup job is associated with a backup policy.</span></span> <span data-ttu-id="e1eed-119">Eszköz feladat jön létre a vészhelyreállítás (DR) vagy a visszaállítási művelet miatt.</span><span class="sxs-lookup"><span data-stu-id="e1eed-119">A device job is created as a result of a disaster recovery (DR) or a restore operation.</span></span>
  * <span data-ttu-id="e1eed-120">**Eszköz** – a név az eszköz, amelyen a feladat elindult.</span><span class="sxs-lookup"><span data-stu-id="e1eed-120">**Device** – The name of the device on which the job was started.</span></span>
  * <span data-ttu-id="e1eed-121">**Megkezdődött a** – az idő, amikor a feladat elindult.</span><span class="sxs-lookup"><span data-stu-id="e1eed-121">**Started on** – The time when the job was started.</span></span>
  * <span data-ttu-id="e1eed-122">**Folyamatban lévő** – egy futó feladat befejezésére százalékos aránya.</span><span class="sxs-lookup"><span data-stu-id="e1eed-122">**Progress** – The percentage completion of a running job.</span></span> <span data-ttu-id="e1eed-123">A projekt mindig legyen 100 %.</span><span class="sxs-lookup"><span data-stu-id="e1eed-123">For a completed job, this should always be 100%.</span></span>

<span data-ttu-id="e1eed-124">A feladatok 30 másodpercenként frissül.</span><span class="sxs-lookup"><span data-stu-id="e1eed-124">The list of jobs is refreshed every 30 seconds.</span></span>

<span data-ttu-id="e1eed-125">Ezen a lapon a következő feladathoz kapcsolódó műveleteket hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="e1eed-125">You can perform the following job-related actions on this page:</span></span>

* <span data-ttu-id="e1eed-126">Feladat részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="e1eed-126">View job details</span></span>
* <span data-ttu-id="e1eed-127">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="e1eed-127">Cancel a job</span></span>

## <a name="view-job-details"></a><span data-ttu-id="e1eed-128">Feladat részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="e1eed-128">View job details</span></span>
<span data-ttu-id="e1eed-129">A következő lépésekkel minden feladat részleteinek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e1eed-129">Perform the following steps to view the details of any job.</span></span>

#### <a name="to-view-job-details"></a><span data-ttu-id="e1eed-130">Feladat részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="e1eed-130">To view job details</span></span>
1. <span data-ttu-id="e1eed-131">Az a **feladatok** lapon, a megfelelő szűrőket és egy lekérdezés futtatásával érdeklődik feladat(ok) jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="e1eed-131">On the **Jobs** page, display the job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="e1eed-132">Kereshet befejezése után fut, és nem vonható vissza a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="e1eed-132">You can search for completed, running, or canceled jobs.</span></span>
2. <span data-ttu-id="e1eed-133">Jelölje ki a feladatot.</span><span class="sxs-lookup"><span data-stu-id="e1eed-133">Select a job.</span></span>
3. <span data-ttu-id="e1eed-134">Kattintson a lap alján **részletek**.</span><span class="sxs-lookup"><span data-stu-id="e1eed-134">At the bottom of the page, click **Details**.</span></span>
4. <span data-ttu-id="e1eed-135">Az a **biztonsági mentési feladat részleteit** párbeszédpanel, megtekintheti az állapot, a részletek, a vonatkozó adatok és a statisztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="e1eed-135">In the **Backup Job Details** dialog box, you can view the status, details, time statistics, and data statistics.</span></span>
   
    ![Feladat részletei lap](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a><span data-ttu-id="e1eed-137">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="e1eed-137">Cancel a job</span></span>
<span data-ttu-id="e1eed-138">A következő lépésekkel egy futó feladatot megszakítja.</span><span class="sxs-lookup"><span data-stu-id="e1eed-138">Perform the following steps to cancel a running job.</span></span>

> [!NOTE]
> <span data-ttu-id="e1eed-139">Nem lehet megszakítani az egyes feladatok, például a kötet módosításához kötet módosításával, vagy a kötet kiterjesztését.</span><span class="sxs-lookup"><span data-stu-id="e1eed-139">Some jobs, such as modifying a volume to change the volume type or expanding a volume, cannot be canceled.</span></span>
> 
> 

### <a name="to-cancel-a-job"></a><span data-ttu-id="e1eed-140">A feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="e1eed-140">To cancel a job</span></span>
1. <span data-ttu-id="e1eed-141">Az a **feladatok** lapon, a futó feladat, amely megfelelő szűrőket és egy lekérdezés futtatásával megszakítja jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="e1eed-141">On the **Jobs** page, display the running job(s) that you want to cancel by running a query with appropriate filters.</span></span>
2. <span data-ttu-id="e1eed-142">Jelölje ki a feladatot.</span><span class="sxs-lookup"><span data-stu-id="e1eed-142">Select the job.</span></span>
3. <span data-ttu-id="e1eed-143">Kattintson a lap alján **Mégse**.</span><span class="sxs-lookup"><span data-stu-id="e1eed-143">At the bottom of the page, click **Cancel**.</span></span>
4. <span data-ttu-id="e1eed-144">Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.</span><span class="sxs-lookup"><span data-stu-id="e1eed-144">When prompted for confirmation, click **Yes**.</span></span>

<span data-ttu-id="e1eed-145">Ez a feladat most megszakítva.</span><span class="sxs-lookup"><span data-stu-id="e1eed-145">This job is now canceled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1eed-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e1eed-146">Next steps</span></span>
* <span data-ttu-id="e1eed-147">Megtudhatja, hogyan [a StorSimple biztonsági mentési házirendek kezelése](storsimple-manage-backup-policies.md).</span><span class="sxs-lookup"><span data-stu-id="e1eed-147">Learn how to [manage your StorSimple backup policies](storsimple-manage-backup-policies.md).</span></span>
* <span data-ttu-id="e1eed-148">Megtudhatja, hogyan [felügyelete a StorSimple eszközt a StorSimple Manager szolgáltatás segítségével](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="e1eed-148">Learn how to [use the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

