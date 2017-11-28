---
title: "Megtekintheti és kezelheti a StorSimple 8000 series feladataihoz |} Microsoft Docs"
description: "Útmutatást nyújt a StorSimple Device Manager szolgáltatás feladatok panelen és nyomon követheti az újabb, aktuális és ütemezett biztonsági mentési feladatok."
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
ms.openlocfilehash: bf8038b7171053b75eeb9aed88bff4246e65a8a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-view-and-manage-jobs-update-3-and-later"></a><span data-ttu-id="afdbe-103">A StorSimple Device Manager szolgáltatás segítségével megtekintheti és kezelheti a feladatok (Update 3 és újabb verziók)</span><span class="sxs-lookup"><span data-stu-id="afdbe-103">Use the StorSimple Device Manager service to view and manage jobs (Update 3 and later)</span></span>

## <a name="overview"></a><span data-ttu-id="afdbe-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="afdbe-104">Overview</span></span>
<span data-ttu-id="afdbe-105">A **feladatok** panel egy egyetlen központi portált biztosít a megtekintése, és a StorSimple Device Manager szolgáltatáshoz való kapcsolódás az eszközökön az elindított feladatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="afdbe-105">The **Jobs** blade provides a single central portal for viewing and managing jobs that were started on devices connected to your StorSimple Device Manager service.</span></span> <span data-ttu-id="afdbe-106">Több eszköz ütemezett, futó, befejezett, törölt és sikertelen feladatok tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="afdbe-106">You can view scheduled, running, completed, canceled, and failed jobs for multiple devices.</span></span> <span data-ttu-id="afdbe-107">Eredmények táblázatos formában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="afdbe-107">Results are presented in a tabular format.</span></span>

![Feladatok panelen](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

<span data-ttu-id="afdbe-109">Gyorsan megtalálhatja a feladatok olyan mezőkön, például a szűréssel érdeklődik:</span><span class="sxs-lookup"><span data-stu-id="afdbe-109">You can quickly find the jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="afdbe-110">**Állapot** – feladatok lehet folyamatban, sikeresen befejeződött, megszakított, sikertelen, zároló vagy sikeres, a hibák.</span><span class="sxs-lookup"><span data-stu-id="afdbe-110">**Status** – Jobs can be in progress, succeeded, canceled, failed, canceling, or succeeded with errors.</span></span>
* <span data-ttu-id="afdbe-111">**Időtartománynak** – feladatok a dátum és idő tartomány alapján szűrhetők.</span><span class="sxs-lookup"><span data-stu-id="afdbe-111">**Time range** – Jobs can be filtered based on the date and time range.</span></span> <span data-ttu-id="afdbe-112">A tartomány: 1 óra, az elmúlt 24 óra múlva elmúlt 7 nap elmúlt 30 napban, elmúlt év vagy egyéni dátuma múltbeli.</span><span class="sxs-lookup"><span data-stu-id="afdbe-112">The ranges are past 1 hour, past 24 hours, past 7 days, past 30 days, past year, or custom date.</span></span>
* <span data-ttu-id="afdbe-113">**Típus** – a feladattípus lehet ütemezett biztonsági mentést, a manuális biztonsági mentés, a biztonsági mentés visszaállítási, kötet klónozása, kötettárolók feladatátvételt, helyileg rögzített kötet létrehozása, módosítása kötet, telepítse a frissítéseket, támogatási gyűjtését és felhő készülék létrehozása.</span><span class="sxs-lookup"><span data-stu-id="afdbe-113">**Type** – The job type can be scheduled backup, manual backup, restore backup, clone volume, fail over volume containers, create locally-pinned volume, modify volume, install updates, collect support logs and create cloud appliance.</span></span>
* <span data-ttu-id="afdbe-114">**Eszközök** – feladatok kezdeményezett kapcsolódik a szolgáltatáshoz egy adott eszközön.</span><span class="sxs-lookup"><span data-stu-id="afdbe-114">**Devices** – Jobs are initiated on a certain device connected to your service.</span></span>
  
<span data-ttu-id="afdbe-115">A szűrt feladatok majd megjelennének alapján a következő attribútumokat:</span><span class="sxs-lookup"><span data-stu-id="afdbe-115">The filtered jobs are then tabulated on the basis of the following attributes:</span></span>
  
* <span data-ttu-id="afdbe-116">**Név** – ütemezett biztonsági mentést, manuális biztonsági mentés, visszaállítás biztonsági mentés, klónozás köteten, a feladatátvétel kötettárolók, helyileg rögzített kötet létrehozása, módosítása a kötet, telepítse a frissítéseket, támogatási gyűjtését, vagy hozzon létre felhő készülék.</span><span class="sxs-lookup"><span data-stu-id="afdbe-116">**Name** – scheduled backup, manual backup, restore backup, clone volume, fail over volume containers, create locally pinned volume, modify volume, install updates, collect support logs, or create cloud appliance.</span></span>
* <span data-ttu-id="afdbe-117">**Állapot** – futó, befejeződött, megszakított, sikertelen, zároló vagy befejeződött, hibákkal.</span><span class="sxs-lookup"><span data-stu-id="afdbe-117">**Status** – running, completed, canceled, failed, canceling, or completed with errors.</span></span>
* <span data-ttu-id="afdbe-118">**Entitás** – a feladat társíthatók a kötet, hogy a biztonsági mentési házirend, vagy eszköz.</span><span class="sxs-lookup"><span data-stu-id="afdbe-118">**Entity** – The jobs can be associated with a volume, a backup policy, or a device.</span></span> <span data-ttu-id="afdbe-119">Például a Klónozás feladat társítva egy köteten, mivel a biztonsági mentési házirend ütemezett biztonsági mentési feladat tartozik.</span><span class="sxs-lookup"><span data-stu-id="afdbe-119">For example, a clone job is associated with a volume, whereas a scheduled backup job is associated with a backup policy.</span></span> <span data-ttu-id="afdbe-120">Eszköz feladat jön létre a vészhelyreállítás (DR) vagy a visszaállítási művelet miatt.</span><span class="sxs-lookup"><span data-stu-id="afdbe-120">A device job is created as a result of a disaster recovery (DR) or a restore operation.</span></span>
* <span data-ttu-id="afdbe-121">**Eszköz** – a név az eszköz, amelyen a feladat elindult.</span><span class="sxs-lookup"><span data-stu-id="afdbe-121">**Device** – The name of the device on which the job was started.</span></span>
* <span data-ttu-id="afdbe-122">**Megkezdődött a** – az idő, amikor a feladat elindult.</span><span class="sxs-lookup"><span data-stu-id="afdbe-122">**Started on** – The time when the job was started.</span></span>
* <span data-ttu-id="afdbe-123">**Időtartam** – a feladat végrehajtásához szükséges időt.</span><span class="sxs-lookup"><span data-stu-id="afdbe-123">**Duration** – The time required to complete the job.</span></span>

<span data-ttu-id="afdbe-124">A feladatok 30 másodpercenként frissül.</span><span class="sxs-lookup"><span data-stu-id="afdbe-124">The list of jobs is refreshed every 30 seconds.</span></span>

<span data-ttu-id="afdbe-125">Ezen a lapon a következő feladathoz kapcsolódó műveleteket hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="afdbe-125">You can perform the following job-related actions on this page:</span></span>

* <span data-ttu-id="afdbe-126">Feladat részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="afdbe-126">View job details</span></span>
* <span data-ttu-id="afdbe-127">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="afdbe-127">Cancel a job</span></span>

## <a name="view-job-details"></a><span data-ttu-id="afdbe-128">Feladat részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="afdbe-128">View job details</span></span>
<span data-ttu-id="afdbe-129">A következő lépésekkel minden feladat részleteinek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="afdbe-129">Perform the following steps to view the details of any job.</span></span>

#### <a name="to-view-job-details"></a><span data-ttu-id="afdbe-130">Feladat részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="afdbe-130">To view job details</span></span>
1. <span data-ttu-id="afdbe-131">Nyissa meg a StorSimple eszköz Manager szolgáltatáshoz, és kattintson a **feladatok**.</span><span class="sxs-lookup"><span data-stu-id="afdbe-131">Go to your StorSimple Device Manager service and then click **Jobs**.</span></span>

2. <span data-ttu-id="afdbe-132">Az a **feladatok** panelen, a megfelelő szűrőket és egy lekérdezés futtatásával érdeklődik feladat(ok) megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="afdbe-132">In the **Jobs** blade, display the job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="afdbe-133">Kereshet befejezése után fut, és nem vonható vissza a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="afdbe-133">You can search for completed, running, or canceled jobs.</span></span>

    ![Feladat panelen](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

2. <span data-ttu-id="afdbe-135">Válassza ki, majd kattintson egy feladatra.</span><span class="sxs-lookup"><span data-stu-id="afdbe-135">Select and click a job.</span></span>

    ![Feladat panelen](./media/storsimple-8000-manage-jobs-u2/jobs3.png)

3. <span data-ttu-id="afdbe-137">A feladat részleteit megjelenítő panelen megtekintheti a állapota, részleteit, idő statisztikák és adatok statisztika.</span><span class="sxs-lookup"><span data-stu-id="afdbe-137">In the job details blade, you can view the status, details, time statistics, and data statistics.</span></span>
   
    ![Feladat részletei](./media/storsimple-8000-manage-jobs-u2/jobs4.png)

## <a name="cancel-a-job"></a><span data-ttu-id="afdbe-139">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="afdbe-139">Cancel a job</span></span>
<span data-ttu-id="afdbe-140">A következő lépésekkel egy futó feladatot megszakítja.</span><span class="sxs-lookup"><span data-stu-id="afdbe-140">Perform the following steps to cancel a running job.</span></span>

> [!NOTE]
> <span data-ttu-id="afdbe-141">Nem lehet megszakítani az egyes feladatok, például a kötet módosításához kötet módosításával, vagy a kötet kiterjesztését.</span><span class="sxs-lookup"><span data-stu-id="afdbe-141">Some jobs, such as modifying a volume to change the volume type or expanding a volume, cannot be canceled.</span></span>


### <a name="to-cancel-a-job"></a><span data-ttu-id="afdbe-142">A feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="afdbe-142">To cancel a job</span></span>
1. <span data-ttu-id="afdbe-143">Az a **feladatok** lapon, a futó feladat, amely megfelelő szűrőket és egy lekérdezés futtatásával megszakítja jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="afdbe-143">On the **Jobs** page, display the running job(s) that you want to cancel by running a query with appropriate filters.</span></span> <span data-ttu-id="afdbe-144">Jelölje ki a feladatot.</span><span class="sxs-lookup"><span data-stu-id="afdbe-144">Select the job.</span></span>

2. <span data-ttu-id="afdbe-145">Kattintson a jobb gombbal a kiválasztott feladat meghívása a helyi menüt, és kattintson a **Mégse**.</span><span class="sxs-lookup"><span data-stu-id="afdbe-145">Right-click on the selected job to invoke the context menu and click **Cancel**.</span></span>

    ![Feladat részletei](./media/storsimple-8000-manage-jobs-u2/jobs2.png)

3. <span data-ttu-id="afdbe-147">Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.</span><span class="sxs-lookup"><span data-stu-id="afdbe-147">When prompted for confirmation, click **Yes**.</span></span> <span data-ttu-id="afdbe-148">Ez a feladat most megszakítva.</span><span class="sxs-lookup"><span data-stu-id="afdbe-148">This job is now canceled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="afdbe-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="afdbe-149">Next steps</span></span>
* <span data-ttu-id="afdbe-150">Megtudhatja, hogyan [a StorSimple biztonsági mentési házirendek kezelése](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="afdbe-150">Learn how to [manage your StorSimple backup policies](storsimple-8000-manage-backup-policies-u2.md).</span></span>
* <span data-ttu-id="afdbe-151">Megtudhatja, hogyan [felügyelete a StorSimple eszközt a StorSimple Device Manager szolgáltatással](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="afdbe-151">Learn how to [use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

