---
title: "aaaView és a StorSimple virtuális tömb feladatok kezelése |} Microsoft Docs"
description: "Hello StorSimple Device Manager szolgáltatás feladatok lapján ismerteti, hogyan toouse azt tootrack legutóbbi és a jelenlegi feladataihoz hello StorSimple virtuális tömb."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 31879821-b599-4609-a7f4-d4b0f658a933
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/11/2016
ms.author: alkohli
ms.openlocfilehash: cf3f3e7bcdfff0ff2328b7354db2482286800e93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooview-jobs-for-hello-storsimple-virtual-array"></a><span data-ttu-id="f8f85-103">A StorSimple virtuális tömb hello hello StorSimple Device Manager szolgáltatás tooview feladatok használata</span><span class="sxs-lookup"><span data-stu-id="f8f85-103">Use hello StorSimple Device Manager service tooview jobs for hello StorSimple Virtual Array</span></span>
## <a name="overview"></a><span data-ttu-id="f8f85-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f8f85-104">Overview</span></span>
<span data-ttu-id="f8f85-105">Hello **feladatok** panel megtekintéséhez és kezeléséhez, amelyek csatlakoztatott tooyour StorSimple Device Manager szolgáltatás a virtuális tömbök indított feladatok egyetlen központi portált biztosít.</span><span class="sxs-lookup"><span data-stu-id="f8f85-105">hello **Jobs** blade provides a single central portal for viewing and managing jobs that are started on virtual arrays that are connected tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="f8f85-106">Több virtuális eszközön futó, befejezett, és a sikertelen feladatok tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="f8f85-106">You can view running, completed, and failed jobs for multiple virtual devices.</span></span> <span data-ttu-id="f8f85-107">Eredmények táblázatos formában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="f8f85-107">Results are presented in a tabular format.</span></span>

![Feladatok panelen](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)

<span data-ttu-id="f8f85-109">Gyorsan megtalálhatja olyan mezőkön, például a szűréssel érdeklődik hello feladatok:</span><span class="sxs-lookup"><span data-stu-id="f8f85-109">You can quickly find hello jobs you are interested in by filtering on fields such as:</span></span>

* <span data-ttu-id="f8f85-110">**Időtartománynak** – feladatok alapján szűrt hello dátum és idő tartomány lehet.</span><span class="sxs-lookup"><span data-stu-id="f8f85-110">**Time range** – Jobs can be filtered based on hello date and time range.</span></span>
* <span data-ttu-id="f8f85-111">**Eszközök** – feladatok kezdeményezett egy csatlakoztatott eszközre tooyour szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="f8f85-111">**Devices** – Jobs are initiated on a specific device connected tooyour service.</span></span> <span data-ttu-id="f8f85-112">hello szűrt feladatok majd megjelennének hello a következő attribútumok alapján:</span><span class="sxs-lookup"><span data-stu-id="f8f85-112">hello filtered jobs are then tabulated based on hello following attributes:</span></span>
  
  * <span data-ttu-id="f8f85-113">**Név** – hello lehet **összes**, **biztonsági mentés**, **Klónozás**, **feladatátvételt**, **frissítések letöltése** , vagy **frissítések telepítése**.</span><span class="sxs-lookup"><span data-stu-id="f8f85-113">**Name** – hello job name can be **All**, **Backup**, **Clone**, **Fail over**, **Download updates**, or **Install updates**.</span></span>
  * <span data-ttu-id="f8f85-114">**Állapot** – feladatok lehetnek **összes**, **folyamatban**, **sikeres**, vagy **sikertelen**, vagy **visszavonva**.</span><span class="sxs-lookup"><span data-stu-id="f8f85-114">**Status** – Jobs can be **All**, **In progress**, **Succeeded**, or **Failed**, or **Canceled**.</span></span>
  * <span data-ttu-id="f8f85-115">**Entitás** – hello feladatok társíthatók a kötet, megosztás, vagy eszköz.</span><span class="sxs-lookup"><span data-stu-id="f8f85-115">**Entity** – hello jobs can be associated with a volume, share, or device.</span></span>
  * <span data-ttu-id="f8f85-116">**Eszköz** – hello mely hello feladat el lett indítva hello eszköz nevét.</span><span class="sxs-lookup"><span data-stu-id="f8f85-116">**Device** – hello name of hello device on which hello job was started.</span></span>
  * <span data-ttu-id="f8f85-117">**Megkezdődött a** – hello idő, amikor hello feladat el lett indítva.</span><span class="sxs-lookup"><span data-stu-id="f8f85-117">**Started on** – hello time when hello job was started.</span></span>
  * <span data-ttu-id="f8f85-118">**Időtartam** – hello időtartama mely hello feladat volt futtatva.</span><span class="sxs-lookup"><span data-stu-id="f8f85-118">**Duration** – hello duration for on which hello job was run.</span></span>
* <span data-ttu-id="f8f85-119">**Állapot** – minden, futó, hajtható végre, vagy a sikertelen feladatok is kereshet.</span><span class="sxs-lookup"><span data-stu-id="f8f85-119">**Status** – You can search for all, running, completed, or failed jobs.</span></span>
* <span data-ttu-id="f8f85-120">**Típusú feladattal** – hello feladat típusa is all, biztonsági mentési, helyreállítási, feladatátvevő, töltse le a frissítéseket, vagy telepítse a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="f8f85-120">**Job type** – hello job type can be all, backup, restore, failover, download updates, or install updates.</span></span>

<span data-ttu-id="f8f85-121">feladatok hello listája 30 másodpercenként frissül.</span><span class="sxs-lookup"><span data-stu-id="f8f85-121">hello list of jobs is refreshed every 30 seconds.</span></span>

## <a name="view-job-details"></a><span data-ttu-id="f8f85-122">Feladatok részleteinek megjelenítése</span><span class="sxs-lookup"><span data-stu-id="f8f85-122">View job details</span></span>
<span data-ttu-id="f8f85-123">Hajtsa végre a következő lépéseket tooview hello adatok bármely feladat hello.</span><span class="sxs-lookup"><span data-stu-id="f8f85-123">Perform hello following steps tooview hello details of any job.</span></span>

#### <a name="tooview-job-details"></a><span data-ttu-id="f8f85-124">tooview feladat részletei</span><span class="sxs-lookup"><span data-stu-id="f8f85-124">tooview job details</span></span>
1. <span data-ttu-id="f8f85-125">A hello **feladatok** panelen, a megfelelő szűrőket és egy lekérdezés futtatásával érdeklődik megjelenítési hello feladat.</span><span class="sxs-lookup"><span data-stu-id="f8f85-125">On hello **Jobs** blade, display hello job(s) you are interested in by running a query with appropriate filters.</span></span> <span data-ttu-id="f8f85-126">Befejezett vagy nem futnak feladatok is kereshet.</span><span class="sxs-lookup"><span data-stu-id="f8f85-126">You can search for completed or running jobs.</span></span>
2. <span data-ttu-id="f8f85-127">Válassza ki a feladatot a feladatok hello táblázatos listáról.</span><span class="sxs-lookup"><span data-stu-id="f8f85-127">Select a job from hello tabular list of jobs.</span></span>
   
    ![Feladat panelen](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)
3. <span data-ttu-id="f8f85-129">Hello a hello lap alján, kattintson **részletek**.</span><span class="sxs-lookup"><span data-stu-id="f8f85-129">At hello bottom of hello page, click **Details**.</span></span>
4. <span data-ttu-id="f8f85-130">A hello **részletek** párbeszédpanelen megtekintheti állapotát, a részletek és a vonatkozó adatok.</span><span class="sxs-lookup"><span data-stu-id="f8f85-130">In hello **Details** dialog box, you can view status, details, and time statistics.</span></span> <span data-ttu-id="f8f85-131">hello alábbi ábrán egy példa hello **biztonsági mentési feladat részleteit** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="f8f85-131">hello following illustration shows an example of hello **Backup Job Details** dialog box.</span></span>
   
    ![Feladat részletei](./media/storsimple-virtual-array-manage-jobs/ova-jobs-details.png)

#### <a name="job-failures-when-hello-virtual-machine-is-paused-in-hello-hypervisor"></a><span data-ttu-id="f8f85-133">Amikor hello virtuális gép fel van függesztve, a hello hipervizor a sikertelen feladatok</span><span class="sxs-lookup"><span data-stu-id="f8f85-133">Job failures when hello virtual machine is paused in hello hypervisor</span></span>
<span data-ttu-id="f8f85-134">Ha egy feladat a folyamatban van a a StorSimple virtuális tömb és hello eszközök (a virtuális gép kiépítése a hipervizor) fel van függesztve, a nagyobb, mint 15 percig, hello feladat sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="f8f85-134">When a job is in progress on your StorSimple Virtual Array and hello device (virtual machine provisioned in hypervisor) is paused for greater than 15 minutes, hello job fails.</span></span> <span data-ttu-id="f8f85-135">Ez miatt tooyour StorSimple virtuális tömb idő alatt szinkronban hello Microsoft Azure idő.</span><span class="sxs-lookup"><span data-stu-id="f8f85-135">This is due tooyour StorSimple Virtual Array time being out of sync with hello Microsoft Azure time.</span></span> 

<span data-ttu-id="f8f85-136">Látni fogja a következő hiba hello: "az eszköz ideje szinkronban hello Microsoft Azure idő több mint 15 perces.</span><span class="sxs-lookup"><span data-stu-id="f8f85-136">You will see hello following error: "Your device time is out of sync with hello Microsoft Azure time by more than 15 minutes.</span></span> <span data-ttu-id="f8f85-137">Győződjön meg arról, hogy hello hipervizor és hello eszköz idő szinkronizálva van egy NTP ügyfélkérelmet.</span><span class="sxs-lookup"><span data-stu-id="f8f85-137">Ensure that hello hypervisor and hello device times are synchronized with an NTP servier.</span></span> <span data-ttu-id="f8f85-138">Győződjön meg arról, hogy nincsenek-e kapcsolódási problémák.</span><span class="sxs-lookup"><span data-stu-id="f8f85-138">Verify that there are no connectivity issues.</span></span> <span data-ttu-id="f8f85-139">tootroubleshoot kapcsolódási problémák, futtassa a diagnosztikai tesztek hello helyi webes felhasználói Felületét a virtuális eszköz."</span><span class="sxs-lookup"><span data-stu-id="f8f85-139">tootroubleshoot connectivity issues, run diagnostic tests from hello local web UI of your virtual device."</span></span>

<span data-ttu-id="f8f85-140">Ezek a hibák toobackup, a visszaállítás, a frissítés és a feladatátvételi feladatok alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="f8f85-140">These failures apply toobackup, restore, update, and failover jobs.</span></span> <span data-ttu-id="f8f85-141">Ha a Hyper-v-ben a virtuális gép üzembe helyezése hello gép idejét végül szinkronizálja a hipervizor.</span><span class="sxs-lookup"><span data-stu-id="f8f85-141">If your virtual machine is provisioned in Hyper-V, hello machine eventually synchronizes time with your hypervisor.</span></span> <span data-ttu-id="f8f85-142">Ebben az esetben, ha a feladat indíthatja el.</span><span class="sxs-lookup"><span data-stu-id="f8f85-142">Once that happens, you can restart your job.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8f85-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f8f85-143">Next steps</span></span>
<span data-ttu-id="f8f85-144">[Ismerje meg, hogyan toouse hello helyi webes felhasználói felület tooadminister a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="f8f85-144">[Learn how toouse hello local web UI tooadminister your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

