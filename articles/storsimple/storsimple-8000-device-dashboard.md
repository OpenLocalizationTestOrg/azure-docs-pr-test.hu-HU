---
title: "aaaUse a StorSimple 8000 series eszköz összefoglaló |} Microsoft Docs"
description: "Hello StorSimple Device Manager szolgáltatás eszközök összefoglaló ismerteti, hogyan toouse azt tooview tárolási metrikák és a csatlakoztatott kezdeményezők és a keresés hello sorozatszámát és IQN."
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
ms.openlocfilehash: b45ffc6ec52ebb6549c25a00c68c62460b208b7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-device-summary-in-storsimple-device-manager-service"></a><span data-ttu-id="c90e3-103">Hello az eszköz StorSimple Device Manager szolgáltatás összegzése</span><span class="sxs-lookup"><span data-stu-id="c90e3-103">Use hello device summary in StorSimple Device Manager service</span></span>

## <a name="overview"></a><span data-ttu-id="c90e3-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c90e3-104">Overview</span></span>
<span data-ttu-id="c90e3-105">hello StorSimple eszköz összefoglaló panel lehetővé teszi az adott StorSimple eszköz információk áttekintését, ezzel szemben a toohello szolgáltatás összefoglaló panel, amelyen a Microsoft Azure StorSimple megoldásban szereplő összes hello eszközök adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c90e3-105">hello StorSimple device summary blade gives you an overview of information for a specific StorSimple device, in contrast toohello service summary blade, which gives you information about all hello devices included in your Microsoft Azure StorSimple solution.</span></span>

<span data-ttu-id="c90e3-106">hello eszköz összefoglaló panel összegzését jeleníti meg a StorSimple 8000 series eszköz regisztrálva van-e egy adott StorSimple az Eszközkezelő kiemelés egy rendszergazda figyelmet igénylő eszköz ismertetünk.</span><span class="sxs-lookup"><span data-stu-id="c90e3-106">hello device summary blade provides a summary view of a StorSimple 8000 series device that is registered with a given StorSimple Device Manager, highlighting those device issues that need a system administrator's attention.</span></span> <span data-ttu-id="c90e3-107">Ez az oktatóanyag hello eszköz összefoglaló panel vezet be, hello tartalom és funkcióját ismerteti, és azokat hello feladatokat hajthat végre ezen a panelen.</span><span class="sxs-lookup"><span data-stu-id="c90e3-107">This tutorial introduces hello device summary blade, explains hello content and function, and describes hello tasks that you can perform from this blade.</span></span>

<span data-ttu-id="c90e3-108">hello eszköz összefoglaló csempe megjeleníti a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="c90e3-108">hello device summary blade displays hello following information:</span></span>

![Eszköz összefoglaló panel](./media/storsimple-8000-device-dashboard/device-summary1.png)

## <a name="management-command-bar"></a><span data-ttu-id="c90e3-110">Felügyeleti parancssáv</span><span class="sxs-lookup"><span data-stu-id="c90e3-110">Management command bar</span></span>

<span data-ttu-id="c90e3-111">Hello StorSimple eszköz panelen látható hello-beállítások a StorSimple eszköz kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="c90e3-111">In hello StorSimple device blade, you see hello options for managing your StorSimple device.</span></span> <span data-ttu-id="c90e3-112">Hello parancsok hello panelről és hello bal oldalán hello tetején megjelenik.</span><span class="sxs-lookup"><span data-stu-id="c90e3-112">You see hello management commands across hello top of hello blade and on hello left side.</span></span> <span data-ttu-id="c90e3-113">Használja a beállítások tooadd megosztások vagy kötetek, vagy frissíteni, vagy az eszköz a feladatátvétel.</span><span class="sxs-lookup"><span data-stu-id="c90e3-113">Use these options tooadd shares or volumes, or update or fail over your device.</span></span>

![Felügyeleti parancssáv](./media/storsimple-8000-device-dashboard/device-summary2.png)

## <a name="essentials"></a><span data-ttu-id="c90e3-115">Alapvető erőforrások</span><span class="sxs-lookup"><span data-stu-id="c90e3-115">Essentials</span></span>

<span data-ttu-id="c90e3-116">hello essentials terület rögzíti, hello fontos tulajdonságok többek között, hello állapota, modell, cél IQN és hello szoftverének verziójával.</span><span class="sxs-lookup"><span data-stu-id="c90e3-116">hello essentials area captures some of hello important properties such as, hello status, model, target IQN, and hello software version.</span></span> 

![Eszköz alapjai](./media/storsimple-8000-device-dashboard/device-summary3.png)

## <a name="monitoring"></a><span data-ttu-id="c90e3-118">Figyelés</span><span class="sxs-lookup"><span data-stu-id="c90e3-118">Monitoring</span></span>

* <span data-ttu-id="c90e3-119">Hello **riasztások** csempe hello összes aktív riasztás pillanatképe biztosít az eszközt, a riasztás súlyosságát szerint csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="c90e3-119">hello **Alerts** tile provides a snapshot of all hello active alerts for your device, grouped by alert severity.</span></span>

    ![Riasztások csempe](./media/storsimple-8000-device-dashboard/device-summary4.png)

    <span data-ttu-id="c90e3-121">Kattintson a hello csempe tooopen hello **riasztások** panel megnyitásához, és kattintson egy adott riasztás tooview további részleteit a riasztást, többek között a javasolt műveletek.</span><span class="sxs-lookup"><span data-stu-id="c90e3-121">Click hello tile tooopen hello **Alerts** blade and then click an individual alert tooview additional details about that alert, including any recommended actions.</span></span> <span data-ttu-id="c90e3-122">Hello riasztás is törölheti, ha hello problémát sikerült megoldani.</span><span class="sxs-lookup"><span data-stu-id="c90e3-122">You can also clear hello alert if hello issue has been resolved.</span></span>

    ![Kattintson a riasztások csempe](./media/storsimple-8000-device-dashboard/device-summary10.png)

* <span data-ttu-id="c90e3-124">Hello **állapotát és állapotfigyelő** csempe hello hardver összetevő állapota betekintést biztosít egy eszközt, beleértve a hello Eszközállapot.</span><span class="sxs-lookup"><span data-stu-id="c90e3-124">hello **Status and health** tile provides insights into hello hardware component health for a device including hello device status.</span></span> <span data-ttu-id="c90e3-125">hello Eszközállapot lehet offline, online, inaktív vagy készen tooset fel.</span><span class="sxs-lookup"><span data-stu-id="c90e3-125">hello device status may be offline, online, deactivated, or ready tooset up.</span></span>

    ![Állapot és állapota csempe](./media/storsimple-8000-device-dashboard/device-summary5.png)

* <span data-ttu-id="c90e3-127">Hello **kötetek** csempe állapot szerint csoportosítva az eszköz a kötetek száma hello összegzését tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c90e3-127">hello **Volumes** tile provides a summary of hello number of volumes in your device grouped by status.</span></span>

    ![Adatkötetek meghajtóbetűjelei](./media/storsimple-8000-device-dashboard/device-summary6.png)

    <span data-ttu-id="c90e3-129">Kattintson a hello csempe tooopen hello **kötetek** panelt, és majd kattintson az egy egyéni kötet tooview vagy annak tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="c90e3-129">Click hello tile tooopen hello **Volumes** list blade, and then click on an individual volume tooview or modify its properties.</span></span>
    
    ![Kattintson az adatkötetek meghajtóbetűjelei](./media/storsimple-8000-device-dashboard/device-summary9.png)
    
    <span data-ttu-id="c90e3-131">További információkért lásd: hogyan túl[kötetek kezelése](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="c90e3-131">For more information, see how too[manage volumes](storsimple-8000-manage-volumes-u2.md).</span></span>

* <span data-ttu-id="c90e3-132">A hello **használati** diagram, megtekintheti a eszköz, mind a hello felhőalapú tárolás alatt hello felhasználásra elmúlt 7 nap, hello alapértelmezett időszakban használt elsődleges tároló hello.</span><span class="sxs-lookup"><span data-stu-id="c90e3-132">In hello **Usage** chart, you can view hello primary storage used across your device, and hello cloud storage consumed over hello past 7 days, hello default time period.</span></span>

     ![Használata csempe](./media/storsimple-8000-device-dashboard/device-summary7.png)
    
     <span data-ttu-id="c90e3-134">egy másik idő méretezési toochoose hello használata **szerkesztése** hello diagram hello jobb felső sarkában található beállítás.</span><span class="sxs-lookup"><span data-stu-id="c90e3-134">toochoose a different time scale, use hello **Edit** option in hello top-right corner of hello chart.</span></span>

     ![Használati diagram szerkesztése](./media/storsimple-8000-device-dashboard/device-summary12.png)

     <span data-ttu-id="c90e3-136">Ezen a diagramon hello teljes elsődleges tárterület (állomások tooyour eszköz által írt adatok mennyiségét hello) metrikáit megtekintheti és hello teljes felhőbeli tárhelyén, az eszköz által felhasznált, egy meghatározott időtartamra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="c90e3-136">In this chart, you can view metrics for hello total primary storage (hello amount of data written by hosts tooyour device) and hello total cloud storage consumed by your device over a period of time.</span></span>
  
     <span data-ttu-id="c90e3-137">Ebben a környezetben *elsődleges tárolási* hello állomás által írt adatok teljes mennyiségének toohello hivatkozik, és a kötet típusa szerint sorolhatók: *elsődleges rétegzett tárolás* egyaránt helyben tárolt adatok és az adatok rétegzett toohello felhő.</span><span class="sxs-lookup"><span data-stu-id="c90e3-137">In this context, *primary storage* refers toohello total amount of data written by hello host, and can be broken down by volume type: *primary tiered storage* includes both locally stored data and data tiered toohello cloud.</span></span> <span data-ttu-id="c90e3-138">*Elsődleges helyileg rögzített tárolási* csak helyben tárolja az adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c90e3-138">*Primary locally pinned storage* includes just data stored locally.</span></span> <span data-ttu-id="c90e3-139">*Felhőbeli tárhely*, a hello ugyanakkor, a hello hello felhőben tárolt adatok teljes mennyiségének mérését.</span><span class="sxs-lookup"><span data-stu-id="c90e3-139">*Cloud storage*, on hello other hand, is a measurement of hello total amount of data stored in hello cloud.</span></span> <span data-ttu-id="c90e3-140">Ez a tároló rétegzett adatok és a biztonsági mentések tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c90e3-140">This storage includes tiered data and backups.</span></span> <span data-ttu-id="c90e3-141">hello felhőben tárolt hello adatok deduplikált, és a tömörített, mivel elsődleges tárolási tárhelyet előtt hello adatok deduplikációja és tömörített hello összegét mutatja.</span><span class="sxs-lookup"><span data-stu-id="c90e3-141">hello data stored in hello cloud is deduplicated and compressed, whereas primary storage indicates hello amount of storage used before hello data is deduplicated and compressed.</span></span> <span data-ttu-id="c90e3-142">(Ezek két szám tooget hello tömörítési arány képet is összehasonlíthatja.) A mind az elsődleges és a felhőbeli tárhelyén, hello összegek konfigurált gyakoriságnak követési hello alapulnak.</span><span class="sxs-lookup"><span data-stu-id="c90e3-142">(You can compare these two numbers tooget an idea of hello compression rate.) For both primary and cloud storage, hello amounts shown are based on hello tracking frequency you configure.</span></span> <span data-ttu-id="c90e3-143">Például ha úgy dönt, hogy az egy hét gyakoriságát, majd hello diagram adatainak megjelenítése minden nap az előző hét a hello.</span><span class="sxs-lookup"><span data-stu-id="c90e3-143">For example, if you choose a one week frequency, then hello chart shows data for each day in hello previous week.</span></span>

     <span data-ttu-id="c90e3-144">felhőalapú tárolás felhasznált idő, jelölje be hello keresztül toosee hello mennyisége **FELHŐALAPÚ TÁROLÓT használja a** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c90e3-144">toosee hello amount of cloud storage consumed over time, select hello **CLOUD STORAGE USED** option.</span></span> <span data-ttu-id="c90e3-145">toosee hello tárhelyet hello állomás, jelölje be hello írt **elsődleges RÉTEGZETT TÁROLÓT használja a** és **elsődleges HELYILEG rögzített TÁROLÓT használja a** beállítások.</span><span class="sxs-lookup"><span data-stu-id="c90e3-145">toosee hello total storage that has been written by hello host, select hello **PRIMARY TIERED STORAGE USED** and **PRIMARY LOCALLY PINNED STORAGE USED** options.</span></span> 
     <span data-ttu-id="c90e3-146">További információkért lásd: [használata hello StorSimple Device Manager szolgáltatás toomonitor a StorSimple eszköz](storsimple-monitor-device.md).</span><span class="sxs-lookup"><span data-stu-id="c90e3-146">For more information, see [Use hello StorSimple Device Manager service toomonitor your StorSimple device](storsimple-monitor-device.md).</span></span>


* <span data-ttu-id="c90e3-147">Hello **kapacitás** csempe megjeleníti hello elsődleges tárolási ellátott és keresztül hello eszköz relatív toohello teljes tárterület érhető el a fennmaradó hello azonos.</span><span class="sxs-lookup"><span data-stu-id="c90e3-147">hello **Capacity** tile displays hello primary storage that is provisioned and remaining across hello device relative toohello total storage available for hello same.</span></span> <span data-ttu-id="c90e3-148">**Kiépített** toohello összeg elő és használatra, tároló hivatkozik **fennmaradó** maradt kapacitás, amelyek között az eszköz létesíthetők toohello hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="c90e3-148">**Provisioned** refers toohello amount of storage that is prepared and allocated for use, **Remaining** refers toohello remaining capacity that can be provisioned across this device.</span></span> 

    ![Használata csempe](./media/storsimple-8000-device-dashboard/device-summary8.png)

    <span data-ttu-id="c90e3-150">Kattintson a csempére tooview hogyan ki van építve hello kapacitás rétegzett és helyileg rögzített kötetek között.</span><span class="sxs-lookup"><span data-stu-id="c90e3-150">Click this tile tooview how hello capacity is provisioned across tiered and locally pinned volumes.</span></span> <span data-ttu-id="c90e3-151">Hello **fennmaradó rétegzett** kapacitása hello elérhető kapacitás, amelyek többek között a felhőbe, miközben hello létesíthetők **fennmaradó helyi** hello lemezek fennmaradó hello kapacitás toothis eszköz csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="c90e3-151">hello **Remaining Tiered** capacity is hello available capacity that can be provisioned including cloud, while hello **Remaining Local** is hello capacity remaining on hello disks attached toothis device.</span></span>

    ![Kattintson a használati diagramon](./media/storsimple-8000-device-dashboard/device-summary13.png)


## <a name="next-steps"></a><span data-ttu-id="c90e3-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c90e3-153">Next steps</span></span>
* <span data-ttu-id="c90e3-154">További tudnivalók hello [StorSimple szolgáltatás összefoglaló panel](storsimple-8000-service-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="c90e3-154">Learn more about hello [StorSimple service summary blade](storsimple-8000-service-dashboard.md).</span></span>
* <span data-ttu-id="c90e3-155">További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="c90e3-155">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

