---
title: "A StorSimple 8000 series eszköz használata összefoglaló |} Microsoft Docs"
description: "Ismerteti a StorSimple szolgáltatás összefoglaló panelre, és ismerteti a StorSimple megoldásban állapotának figyelése céljából."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: d987a4ae170f21532a886552cbe1eb5a0d25fc3f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-service-summary-blade-for-storsimple-8000-series-device"></a><span data-ttu-id="19505-103">A szolgáltatás összefoglaló panelre a StorSimple 8000 series eszköz használata</span><span class="sxs-lookup"><span data-stu-id="19505-103">Use the service summary blade for StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="19505-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="19505-104">Overview</span></span>

<span data-ttu-id="19505-105">A StorSimple Device Manager szolgáltatás összefoglaló panel összegzését jeleníti meg az eszközök csatlakoznak a StorSimple Device Manager szolgáltatás, a rendszergazda figyelmet igénylő eszközök kiemelve.</span><span class="sxs-lookup"><span data-stu-id="19505-105">The StorSimple Device Manager service summary blade provides a summary view of all the devices that are connected to the StorSimple Device Manager service, highlighting those devices that need a system administrator's attention.</span></span> <span data-ttu-id="19505-106">Ez az oktatóanyag vezet be a szolgáltatás összefoglaló panelre, ismerteti az irányítópult-tartalom és a függvény, és azokat a feladatokat hajthat végre ezen a lapon.</span><span class="sxs-lookup"><span data-stu-id="19505-106">This tutorial introduces the service summary blade, explains the dashboard content and function, and describes the tasks that you can perform from this page.</span></span>

![Szolgáltatás összegzése](./media/storsimple-8000-service-dashboard/service-summary1.png)


## <a name="management-commands"></a><span data-ttu-id="19505-108">Felügyeleti parancsok</span><span class="sxs-lookup"><span data-stu-id="19505-108">Management commands</span></span>

<span data-ttu-id="19505-109">A StorSimple szolgáltatás összefoglaló panelen látható a StorSimple eszköz Manager szolgáltatás és a StorSimple 8000 sorozat eszközeire, a szolgáltatásra regisztrált kezelésére szolgáló beállítások.</span><span class="sxs-lookup"><span data-stu-id="19505-109">In the StorSimple service summary blade, you see the options for managing your StorSimple Device Manager service and the StorSimple 8000 series devices registered to this service.</span></span> <span data-ttu-id="19505-110">A parancsok látja a panelt és bal oldalán látható.</span><span class="sxs-lookup"><span data-stu-id="19505-110">You see the management commands across the top of the blade and on the left side.</span></span>

![Parancssáv](./media/storsimple-8000-service-dashboard/service-summary2.png)

<span data-ttu-id="19505-112">Adja hozzá ezeket a beállításokat, például a különböző műveletek végrehajtásához, megosztások vagy kötetek vagy a figyelő a StorSimple eszközön futó feladatok a különböző használja.</span><span class="sxs-lookup"><span data-stu-id="19505-112">Use these options to perform various operations such as add shares or volumes, or monitor the various jobs running on the StorSimple devices.</span></span>


## <a name="essentials"></a><span data-ttu-id="19505-113">Alapvető erőforrások</span><span class="sxs-lookup"><span data-stu-id="19505-113">Essentials</span></span>

<span data-ttu-id="19505-114">Az essentials területen rögzíti a fontos tulajdonságok, mint például az erőforráscsoportot, helyét és előfizetés, amelyben a StorSimple Device Manager készült.</span><span class="sxs-lookup"><span data-stu-id="19505-114">The essentials area captures some of the important properties such as, the resource group, location, and subscription in which your StorSimple Device Manager was created.</span></span>

![Alapvető erőforrások](./media/storsimple-8000-service-dashboard/service-summary3.png)

## <a name="storsimple-device-manager-service-summary"></a><span data-ttu-id="19505-116">StorSimple Device Manager szolgáltatás összegzése</span><span class="sxs-lookup"><span data-stu-id="19505-116">StorSimple Device Manager service summary</span></span>

* <span data-ttu-id="19505-117">A **riasztások** csempe biztosít az összes aktív riasztás pillanatképet minden eszközön, a riasztás súlyosságát szerint csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="19505-117">The **Alerts** tile provides a snapshot of all the active alerts across all devices, grouped by alert severity.</span></span>

    ![Riasztások csempe](./media/storsimple-8000-service-dashboard/service-summary4.png)

    <span data-ttu-id="19505-119">A csempére kattintva megnyithatja a **riasztások** panel, ahol egyéni riasztást, hogy a riasztással kapcsolatos további részletek megtekintéséhez kattintson többek között a javasolt műveletek.</span><span class="sxs-lookup"><span data-stu-id="19505-119">Clicking the tile opens the **Alerts** blade, where you can click an individual alert to view additional details about that alert, including any recommended actions.</span></span> <span data-ttu-id="19505-120">A riasztás is törölheti, ha a probléma megoldódott.</span><span class="sxs-lookup"><span data-stu-id="19505-120">You can also clear the alert if the issue has been resolved.</span></span>

    ![Kattintson a riasztások csempe](./media/storsimple-8000-service-dashboard/service-summary8.png)

* <span data-ttu-id="19505-122">A **kapacitás** csempe megjeleníti a kiépített és a fennmaradó relatív a teljes tárterület érhető el az összes eszközön van minden eszközön elsődleges tárterület megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="19505-122">The **Capacity** tile displays shows the primary storage that is provisioned and remaining across all devices relative to the total storage available across all devices.</span></span> <span data-ttu-id="19505-123">**Kiépített** hivatkozik, amely készített, és használatra, tárolókapacitást **fennmaradó** hivatkozik, amely az összes eszközön létesíthetők kapacitása.</span><span class="sxs-lookup"><span data-stu-id="19505-123">**Provisioned** refers to the amount of storage that is prepared and allocated for use, **Remaining** refers to the remaining capacity that can be provisioned across all devices.</span></span>

    ![A kapacitás csempe](./media/storsimple-8000-service-dashboard/service-summary6.png)

    <span data-ttu-id="19505-125">A **fennmaradó rétegzett** kapacitása a rendelkezésre álló kapacitásból, amelyek többek között a felhő, létesíthetők közben a **fennmaradó helyi** a kapacitás, a StorSimple 8000 csatlakoztatott lemezek fennmaradó sorozat eszközeire.</span><span class="sxs-lookup"><span data-stu-id="19505-125">The **Remaining Tiered** capacity is the available capacity that can be provisioned including cloud, while the **Remaining Local** is the capacity remaining on the disks attached to the StorSimple 8000 series devices.</span></span>


* <span data-ttu-id="19505-126">Az a **használati** diagram, a kapcsolódó metrikák láthatja az eszközökhöz.</span><span class="sxs-lookup"><span data-stu-id="19505-126">In the **Usage** chart, you can see the relevant metrics for your devices.</span></span> <span data-ttu-id="19505-127">Megtekintheti az összes eszközön használt elsődleges tárhely és az elmúlt 7 napban, az alapértelmezett időszak eszközök által használt felhőbeli tárhelyhez.</span><span class="sxs-lookup"><span data-stu-id="19505-127">You can view the primary storage used across all devices, and the cloud storage consumed by devices over the past 7 days, the default time period.</span></span> 

    ![Használata csempe](./media/storsimple-8000-service-dashboard/service-summary7.png) 

    <span data-ttu-id="19505-129">Válasszon egy másik időskálára, használja a **szerkesztése** a diagram jobb felső sarokban lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="19505-129">To choose a different time scale, use the **Edit** option in the top-right corner of the chart.</span></span>

     ![Kattintson a használata csempe](./media/storsimple-8000-service-dashboard/service-summary10.png)

     ![A diagram adatok exportálása](./media/storsimple-8000-service-dashboard/service-summary11.png)

* <span data-ttu-id="19505-132">A **eszközök** csempe a StorSimple 8000 series eszközök a StorSimple eszköz állapot szerint csoportosítva száma összegzését tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="19505-132">The **Devices** tile provides a summary of the number of StorSimple 8000 series devices in your StorSimple Device Manager grouped by device status.</span></span> 

    ![Eszközök csempe](./media/storsimple-8000-service-dashboard/service-summary5.png)

    <span data-ttu-id="19505-134">Kattintson a csempére kattintva nyissa meg a **eszközök** panel listára, majd elemezze az eszközhöz tartozó eszközök összegzése az egyes eszköz.</span><span class="sxs-lookup"><span data-stu-id="19505-134">Click this tile to open the **Devices** list blade and then click an individual device to drill into the device summary specific to the device.</span></span> <span data-ttu-id="19505-135">Egy adott eszköz összefoglaló paneljéről eszközspecifikus műveletek is elvégezheti.</span><span class="sxs-lookup"><span data-stu-id="19505-135">You can also perform device-specific actions from a given device summary blade.</span></span> <span data-ttu-id="19505-136">Az eszköz összefoglaló panel kapcsolatos további információkért látogasson el [eszköz összefoglaló panel](storsimple-8000-device-dashboard.md).</span><span class="sxs-lookup"><span data-stu-id="19505-136">For more information about the device summary blade, go to [Device summary blade](storsimple-8000-device-dashboard.md).</span></span>

    ![Kattintson az eszközök csempe](./media/storsimple-8000-service-dashboard/service-summary9.png)

## <a name="view-the-activity-logs"></a><span data-ttu-id="19505-138">A tevékenység naplók megtekintése</span><span class="sxs-lookup"><span data-stu-id="19505-138">View the activity logs</span></span>

<span data-ttu-id="19505-139">A különböző műveletek a StorSimple Device Manager belül elvégzett megtekintéséhez kattintson a **tevékenységi naplóit** hivatkozás a StorSimple szolgáltatás összefoglaló panel bal oldalán.</span><span class="sxs-lookup"><span data-stu-id="19505-139">To view the various operations carried out within your StorSimple Device Manager, click the **Activity logs** link on the left side of your StorSimple service summary blade.</span></span> <span data-ttu-id="19505-140">Ehhez szükséges, hogy a **tevékenységi naplóit** panel, ahol láthatja a legutóbbi műveletek összegzését.</span><span class="sxs-lookup"><span data-stu-id="19505-140">This takes you to the **Activity logs** blade, where you can see a summary of the recent operations carried out.</span></span>

![Tevékenységnaplók](./media/storsimple-8000-service-dashboard/activity-logs1.png)
## <a name="next-steps"></a><span data-ttu-id="19505-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19505-142">Next steps</span></span>

* <span data-ttu-id="19505-143">További tudnivalók a [felügyelete a StorSimple eszközt a StorSimple Device Manager szolgáltatással](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="19505-143">Learn more about how to [use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

