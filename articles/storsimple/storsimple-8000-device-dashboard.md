---
title: "A StorSimple 8000 series eszköz használata összefoglaló |} Microsoft Docs"
description: "A StorSimple Device Manager szolgáltatás eszközének összegzése és a storage mérőszámainak és csatlakoztatott kezdeményezők megtekintése, és keresse meg a sorozatszámot és IQN ismerteti."
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
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: 784d3ce9d8f926b00ac1c6fbf48a05c0b04f900a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-device-summary-in-storsimple-device-manager-service"></a><span data-ttu-id="c570a-103">Az eszköz StorSimple Device Manager szolgáltatásban összefoglaló használata</span><span class="sxs-lookup"><span data-stu-id="c570a-103">Use the device summary in StorSimple Device Manager service</span></span>

## <a name="overview"></a><span data-ttu-id="c570a-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c570a-104">Overview</span></span>
<span data-ttu-id="c570a-105">A StorSimple eszköz összefoglaló panel lehetővé teszi az adott StorSimple eszköz, a szolgáltatás összefoglaló panel, amelyen adatokat tartalmaz minden olyan eszközre, amelyet a Microsoft Azure StorSimple megoldásban ellentétben információk áttekintését.</span><span class="sxs-lookup"><span data-stu-id="c570a-105">The StorSimple device summary blade gives you an overview of information for a specific StorSimple device, in contrast to the service summary blade, which gives you information about all the devices included in your Microsoft Azure StorSimple solution.</span></span>

<span data-ttu-id="c570a-106">Az eszköz összefoglaló panel összegzését jeleníti meg a StorSimple 8000 series eszköz regisztrálva van-e egy adott StorSimple az Eszközkezelő kiemelés egy rendszergazda figyelmet igénylő eszköz ismertetünk.</span><span class="sxs-lookup"><span data-stu-id="c570a-106">The device summary blade provides a summary view of a StorSimple 8000 series device that is registered with a given StorSimple Device Manager, highlighting those device issues that need a system administrator's attention.</span></span> <span data-ttu-id="c570a-107">Ez az oktatóanyag vezet be az eszköz összefoglaló panelen, a tartalom és funkcióját ismerteti, és azokat a feladatokat hajthat végre ezen a panelen.</span><span class="sxs-lookup"><span data-stu-id="c570a-107">This tutorial introduces the device summary blade, explains the content and function, and describes the tasks that you can perform from this blade.</span></span>

<span data-ttu-id="c570a-108">Az eszköz összefoglaló panel az alábbi információkat jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="c570a-108">The device summary blade displays the following information:</span></span>

![Eszköz összefoglaló panel](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a><span data-ttu-id="c570a-110">Felügyeleti parancssáv</span><span class="sxs-lookup"><span data-stu-id="c570a-110">Management command bar</span></span>

<span data-ttu-id="c570a-111">A StorSimple eszköz paneljén láthatja a StorSimple eszköz kezelésére szolgáló beállítások.</span><span class="sxs-lookup"><span data-stu-id="c570a-111">In the StorSimple device blade, you see the options for managing your StorSimple device.</span></span> <span data-ttu-id="c570a-112">A parancsok látja a panelt és bal oldalán látható.</span><span class="sxs-lookup"><span data-stu-id="c570a-112">You see the management commands across the top of the blade and on the left side.</span></span> <span data-ttu-id="c570a-113">Ezek a beállítások segítségével adja hozzá a megosztások vagy kötetek, vagy frissíteni, vagy az eszköz a feladatátvétel.</span><span class="sxs-lookup"><span data-stu-id="c570a-113">Use these options to add shares or volumes, or update or fail over your device.</span></span>

![Felügyeleti parancssáv](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a><span data-ttu-id="c570a-115">Alapvető erőforrások</span><span class="sxs-lookup"><span data-stu-id="c570a-115">Essentials</span></span>

<span data-ttu-id="c570a-116">Az essentials területen rögzíti, például fontos tulajdonságait, állapotát, modell, cél IQN-Nevének és a szoftververzió.</span><span class="sxs-lookup"><span data-stu-id="c570a-116">The essentials area captures some of the important properties such as, the status, model, target IQN, and the software version.</span></span> 

![Eszköz alapjai](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a><span data-ttu-id="c570a-118">Figyelés</span><span class="sxs-lookup"><span data-stu-id="c570a-118">Monitoring</span></span>

* <span data-ttu-id="c570a-119">A **riasztások** csempe pillanatképet az összes aktív riasztás biztosít az eszközt, a riasztás súlyosságát szerint csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="c570a-119">The **Alerts** tile provides a snapshot of all the active alerts for your device, grouped by alert severity.</span></span>

    ![Riasztások csempe](./media/storsimple-8000-device-dashboard/device-summary4.png)

    <span data-ttu-id="c570a-121">Kattintson a csempére kattintva nyissa meg a **riasztások** panel megnyitásához, és kattintson a riasztás, így azokat kapcsolatos további részletek megtekintéséhez egyéni riasztást javasolt műveletek.</span><span class="sxs-lookup"><span data-stu-id="c570a-121">Click the tile to open the **Alerts** blade and then click an individual alert to view additional details about that alert, including any recommended actions.</span></span> <span data-ttu-id="c570a-122">A riasztás is törölheti, ha a probléma megoldódott.</span><span class="sxs-lookup"><span data-stu-id="c570a-122">You can also clear the alert if the issue has been resolved.</span></span>

    ![Kattintson a riasztások csempe](./media/storsimple-8000-device-dashboard/device-summary10.png)

* <span data-ttu-id="c570a-124">A **állapotát és állapotfigyelő** csempe a hardver összetevő állapota betekintést biztosít egy eszközt, beleértve az eszköz állapotát.</span><span class="sxs-lookup"><span data-stu-id="c570a-124">The **Status and health** tile provides insights into the hardware component health for a device including the device status.</span></span> <span data-ttu-id="c570a-125">Az eszköz állapota lehet offline, online, inaktív vagy készen áll a beállítása.</span><span class="sxs-lookup"><span data-stu-id="c570a-125">The device status may be offline, online, deactivated, or ready to set up.</span></span>

    ![Állapot és állapota csempe](./media/storsimple-8000-device-dashboard/device-summary5.png)

* <span data-ttu-id="c570a-127">A **kötetek** csempe állapot szerint csoportosítva az eszköz a kötetek száma összegzését tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c570a-127">The **Volumes** tile provides a summary of the number of volumes in your device grouped by status.</span></span>

    ![Adatkötetek meghajtóbetűjelei](./media/storsimple-8000-device-dashboard/device-summary6.png)

    <span data-ttu-id="c570a-129">Kattintson a csempére kattintva nyissa meg a **kötetek** listára panelen, majd a tulajdonságainak megtekintéséhez vagy módosításához egy egyéni köteten található.</span><span class="sxs-lookup"><span data-stu-id="c570a-129">Click the tile to open the **Volumes** list blade, and then click on an individual volume to view or modify its properties.</span></span>
    
    ![Kattintson az adatkötetek meghajtóbetűjelei](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    <span data-ttu-id="c570a-131">További információkért lásd: hogyan [kötetek kezelése](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="c570a-131">For more information, see how to [manage volumes](storsimple-8000-manage-volumes-u2.md).</span></span>

* <span data-ttu-id="c570a-132">Az a **használati** diagram, megtekintheti az elsődleges használ, amelyek az eszközt, és a felhő tárolására az elmúlt 7 napban, az alapértelmezett időszak felhasznált.</span><span class="sxs-lookup"><span data-stu-id="c570a-132">In the **Usage** chart, you can view the primary storage used across your device, and the cloud storage consumed over the past 7 days, the default time period.</span></span>

     ![Használata csempe](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     <span data-ttu-id="c570a-134">Válasszon egy másik időskálára, használja a **szerkesztése** a diagram jobb felső sarokban lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c570a-134">To choose a different time scale, use the **Edit** option in the top-right corner of the chart.</span></span>

     ![Használati diagram szerkesztése](./media/storsimple-8000-device-dashboard/device-summary12.png)

     <span data-ttu-id="c570a-136">Ezen a diagramon az összes elsődleges (az állomások számára az eszköz által írt adatok mennyisége) és a teljes felhő tárolására egy meghatározott időtartamra vonatkozóan az eszköz által felhasznált metrikáját tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="c570a-136">In this chart, you can view metrics for the total primary storage (the amount of data written by hosts to your device) and the total cloud storage consumed by your device over a period of time.</span></span>
  
     <span data-ttu-id="c570a-137">Ebben a környezetben *elsődleges tárolási* a gazdagép által írt adatok teljes mennyisége hivatkozik, és a kötet típusa szerint sorolhatók: *elsődleges rétegzett tárolás* egyaránt helyben tárolt adatok és az adatok a rétegzett a felhőben.</span><span class="sxs-lookup"><span data-stu-id="c570a-137">In this context, *primary storage* refers to the total amount of data written by the host, and can be broken down by volume type: *primary tiered storage* includes both locally stored data and data tiered to the cloud.</span></span> <span data-ttu-id="c570a-138">*Elsődleges helyileg rögzített tárolási* csak helyben tárolja az adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c570a-138">*Primary locally pinned storage* includes just data stored locally.</span></span> <span data-ttu-id="c570a-139">*Felhőbeli tárhely*, másrészt van a felhőben tárolt adatok teljes mennyisége mérését.</span><span class="sxs-lookup"><span data-stu-id="c570a-139">*Cloud storage*, on the other hand, is a measurement of the total amount of data stored in the cloud.</span></span> <span data-ttu-id="c570a-140">Ez a tároló rétegzett adatok és a biztonsági mentések tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c570a-140">This storage includes tiered data and backups.</span></span> <span data-ttu-id="c570a-141">A felhőben tárolt adatok deduplikált, és a tömörített, mivel elsődleges tárolási előtt az adatok deduplikációja és tömörített tárolókapacitást jelzi.</span><span class="sxs-lookup"><span data-stu-id="c570a-141">The data stored in the cloud is deduplicated and compressed, whereas primary storage indicates the amount of storage used before the data is deduplicated and compressed.</span></span> <span data-ttu-id="c570a-142">(Ha meg szeretne ismerkedni a tömörítés és a két szám összehasonlíthatja). A mind az elsődleges és a felhőalapú tárolást látható összegek konfigurált követési gyakoriságnak alapulnak.</span><span class="sxs-lookup"><span data-stu-id="c570a-142">(You can compare these two numbers to get an idea of the compression rate.) For both primary and cloud storage, the amounts shown are based on the tracking frequency you configure.</span></span> <span data-ttu-id="c570a-143">Például ha úgy dönt, hogy az egy hét gyakoriságát, majd a diagram adatainak megjelenítése minden az előző hét nap.</span><span class="sxs-lookup"><span data-stu-id="c570a-143">For example, if you choose a one week frequency, then the chart shows data for each day in the previous week.</span></span>

     <span data-ttu-id="c570a-144">Felhő tárolókapacitást időbeli használni, jelölje ki a **FELHŐALAPÚ TÁROLÓT használja a** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c570a-144">To see the amount of cloud storage consumed over time, select the **CLOUD STORAGE USED** option.</span></span> <span data-ttu-id="c570a-145">A teljes tárterület a gazdagép által írt, jelölje ki a **elsődleges RÉTEGZETT TÁROLÓT használja a** és **elsődleges HELYILEG rögzített TÁROLÓT használja a** beállítások.</span><span class="sxs-lookup"><span data-stu-id="c570a-145">To see the total storage that has been written by the host, select the **PRIMARY TIERED STORAGE USED** and **PRIMARY LOCALLY PINNED STORAGE USED** options.</span></span> 
     <span data-ttu-id="c570a-146">További információkért lásd: [a StorSimple Device Manager szolgáltatás segítségével figyelheti a StorSimple eszköz](storsimple-monitor-device.md).</span><span class="sxs-lookup"><span data-stu-id="c570a-146">For more information, see [Use the StorSimple Device Manager service to monitor your StorSimple device](storsimple-monitor-device.md).</span></span>


* <span data-ttu-id="c570a-147">A **kapacitás** csempe megjeleníti az elsődleges tárolási kiépített és a fennmaradó tartozik ugyanahhoz a rendelkezésre álló tárhelyet viszonyítva az eszközt.</span><span class="sxs-lookup"><span data-stu-id="c570a-147">The **Capacity** tile displays the primary storage that is provisioned and remaining across the device relative to the total storage available for the same.</span></span> <span data-ttu-id="c570a-148">**Kiépített** hivatkozik, amely készített, és használatra, tárolókapacitást **fennmaradó** hivatkozik, amely az eszköz közötti létesíthetők kapacitása.</span><span class="sxs-lookup"><span data-stu-id="c570a-148">**Provisioned** refers to the amount of storage that is prepared and allocated for use, **Remaining** refers to the remaining capacity that can be provisioned across this device.</span></span> 

    ![Használata csempe](./media/storsimple-8000-device-dashboard/device-summary8.png)

    <span data-ttu-id="c570a-150">Kattintson a csempe megtekintéséhez, hogy ki van építve a kapacitás rétegzett és helyileg rögzített kötetek között.</span><span class="sxs-lookup"><span data-stu-id="c570a-150">Click this tile to view how the capacity is provisioned across tiered and locally pinned volumes.</span></span> <span data-ttu-id="c570a-151">A **fennmaradó rétegzett** kapacitása a rendelkezésre álló kapacitásból, amelyek többek között a felhő, létesíthetők közben a **fennmaradó helyi** erre az eszközre csatlakoztatott lemezeken maradt kapacitás.</span><span class="sxs-lookup"><span data-stu-id="c570a-151">The **Remaining Tiered** capacity is the available capacity that can be provisioned including cloud, while the **Remaining Local** is the capacity remaining on the disks attached to this device.</span></span>

    ![Kattintson a használati diagramon](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a><span data-ttu-id="c570a-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c570a-153">Next steps</span></span>
* <span data-ttu-id="c570a-154">További információ a [StorSimple szolgáltatás összefoglaló panel](storsimple-8000-service-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="c570a-154">Learn more about the [StorSimple service summary blade](storsimple-8000-service-dashboard.md).</span></span>
* <span data-ttu-id="c570a-155">További információ [felügyelete a StorSimple eszközt a StorSimple Device Manager szolgáltatással](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="c570a-155">Learn more about [using the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

