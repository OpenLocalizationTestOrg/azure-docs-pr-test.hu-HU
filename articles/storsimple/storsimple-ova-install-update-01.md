---
title: "Telepítse a frissítéseket egy StorSimple virtuális tömb |} Microsoft Docs"
description: "A StorSimple virtuális tömb webes felhasználói felület használata a portál és a gyorsjavítások metódussal frissítés alkalmazása"
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 7f1b1e7d-04d0-4ca2-9dbb-77077ff19bb9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/07/2016
ms.author: alkohli
ms.openlocfilehash: bccb0d49c1959a690d513961c32d946763385a87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="install-updates-on-your-storsimple-virtual-array"></a><span data-ttu-id="982ab-103">Telepítse a frissítéseket a StorSimple virtuális tömb</span><span class="sxs-lookup"><span data-stu-id="982ab-103">Install Updates on your StorSimple Virtual Array</span></span>
## <a name="overview"></a><span data-ttu-id="982ab-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="982ab-104">Overview</span></span>
<span data-ttu-id="982ab-105">Ez a cikk azt ismerteti, hogyan frissítések települnek arra a StorSimple virtuális tömb a klasszikus Azure portál és a helyi webes felhasználói felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="982ab-105">This article describes the steps required to install updates on your StorSimple Virtual Array via the local web UI and via the Azure classic portal.</span></span> <span data-ttu-id="982ab-106">Szeretné alkalmazni a szoftverfrissítések vagy gyorsjavítások a StorSimple virtuális tömb naprakész állapotban tartása érdekében.</span><span class="sxs-lookup"><span data-stu-id="982ab-106">You need to apply software updates or hotfixes to keep your StorSimple Virtual Array up-to-date.</span></span> 

<span data-ttu-id="982ab-107">Ne feledje, hogy egy frissítés vagy gyorsjavítás telepítése az eszköz újraindul.</span><span class="sxs-lookup"><span data-stu-id="982ab-107">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="982ab-108">Fényében, hogy a StorSimple virtuális tömb a egycsomópontos eszközről szó, a folyamatban lévő összes i/o megszakad, és az eszköz leállást észlel.</span><span class="sxs-lookup"><span data-stu-id="982ab-108">Given that the StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span> 

<span data-ttu-id="982ab-109">Egy frissítés alkalmazása előtt javasoljuk, hogy szánjon a kötetek vagy megosztások offline állapotba a gazdagépen első, majd az eszközről.</span><span class="sxs-lookup"><span data-stu-id="982ab-109">Before you apply an update, we recommend that you take the volumes or shares offline on the host first and then the device.</span></span> <span data-ttu-id="982ab-110">Ez minimalizálja az adatvesztést lehetőségét.</span><span class="sxs-lookup"><span data-stu-id="982ab-110">This minimizes any possibility of data corruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="982ab-111">Ha a frissítés 0,1 vagy GA szoftver verziója fut, a gyorsjavítás módszer a helyi webes felhasználói felületen keresztül frissítés 0,3 kell használnia.</span><span class="sxs-lookup"><span data-stu-id="982ab-111">If you are running Update 0.1 or GA software versions, you must use the hotfix method via the local web UI to install update 0.3.</span></span> <span data-ttu-id="982ab-112">Ha futtatja a frissítési 0,2, azt javasoljuk, hogy telepítse a frissítéseket, a klasszikus Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="982ab-112">If you are running Update 0.2, we recommend that you install the updates via the Azure classic portal.</span></span>
> 
> 

## <a name="use-the-local-web-ui"></a><span data-ttu-id="982ab-113">A helyi webes felhasználói felület használata</span><span class="sxs-lookup"><span data-stu-id="982ab-113">Use the local web UI</span></span>
<span data-ttu-id="982ab-114">Két lépésben történik a helyi webes felhasználói felület használata esetén:</span><span class="sxs-lookup"><span data-stu-id="982ab-114">There are two steps when using the local web UI:</span></span>

* <span data-ttu-id="982ab-115">A frissítés vagy gyorsjavítás letöltése</span><span class="sxs-lookup"><span data-stu-id="982ab-115">Download the update or the hotfix</span></span>
* <span data-ttu-id="982ab-116">A frissítés vagy gyorsjavítás telepítése</span><span class="sxs-lookup"><span data-stu-id="982ab-116">Install the update or the hotfix</span></span>

### <a name="download-the-update-or-the-hotfix"></a><span data-ttu-id="982ab-117">A frissítés vagy gyorsjavítás letöltése</span><span class="sxs-lookup"><span data-stu-id="982ab-117">Download the update or the hotfix</span></span>
<span data-ttu-id="982ab-118">Hajtsa végre a következő lépéseket a szoftverfrissítés a Microsoft Update katalógusból történő letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="982ab-118">Perform the following steps to download the software update from the Microsoft Update Catalog.</span></span>

#### <a name="to-download-the-update-or-the-hotfix"></a><span data-ttu-id="982ab-119">A frissítés vagy gyorsjavítás letöltése</span><span class="sxs-lookup"><span data-stu-id="982ab-119">To download the update or the hotfix</span></span>
1. <span data-ttu-id="982ab-120">Indítsa el az Internet Explorert, és keresse fel a [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com) címet.</span><span class="sxs-lookup"><span data-stu-id="982ab-120">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>
2. <span data-ttu-id="982ab-121">Ha most használja először a Microsoft Update katalógust ezen a számítógépen, kattintson a **Telepítés** gombra, amikor a rendszer a Microsoft Update katalógus beépülő moduljának telepítésére kéri.</span><span class="sxs-lookup"><span data-stu-id="982ab-121">If this is your first time using the Microsoft Update Catalog on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>
3. <span data-ttu-id="982ab-122">A keresési mezőbe, a Microsoft Update katalógus adja meg a Tudásbázis (KB) le szeretné tölteni a gyorsjavítást.</span><span class="sxs-lookup"><span data-stu-id="982ab-122">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download.</span></span> <span data-ttu-id="982ab-123">Adja meg **3182061** frissítési 0,3, és kattintson a **keresési**.</span><span class="sxs-lookup"><span data-stu-id="982ab-123">Enter **3182061** for Update 0.3, and then click **Search**.</span></span>
   
    <span data-ttu-id="982ab-124">A gyorsjavítás-lista megjelenik, például **StorSimple virtuális tömb frissítés 0,3**.</span><span class="sxs-lookup"><span data-stu-id="982ab-124">The hotfix listing appears, for example, **StorSimple Virtual Array Update 0.3**.</span></span>
   
    ![Keresés a katalógusban](./media/storsimple-ova-install-update-01/download1.png)
4. <span data-ttu-id="982ab-126">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="982ab-126">Click **Add**.</span></span> <span data-ttu-id="982ab-127">Ezzel a frissítést hozzáadja a kosárhoz.</span><span class="sxs-lookup"><span data-stu-id="982ab-127">The update is added to the basket.</span></span>
5. <span data-ttu-id="982ab-128">Kattintson a **Kosár megtekintése** gombra.</span><span class="sxs-lookup"><span data-stu-id="982ab-128">Click **View Basket**.</span></span>
6. <span data-ttu-id="982ab-129">Kattintson a **Letöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="982ab-129">Click **Download**.</span></span> <span data-ttu-id="982ab-130">Adja meg vagy **tallózással** válassza ki a helyet, ahová a fájlokat le szeretné tölteni.</span><span class="sxs-lookup"><span data-stu-id="982ab-130">Specify or **Browse** to a local location where you want the downloads to appear.</span></span> <span data-ttu-id="982ab-131">A frissítések a megadott helyre lesznek letöltve, azon belül az egyes frissítések nevével egyező nevű almappákba.</span><span class="sxs-lookup"><span data-stu-id="982ab-131">The updates are downloaded to the specified location and placed in a subfolder with the same name as the update.</span></span> <span data-ttu-id="982ab-132">A mappa átmásolható egy, az eszközről elérhető hálózati megosztásra is.</span><span class="sxs-lookup"><span data-stu-id="982ab-132">The folder can also be copied to a network share that is reachable from the device.</span></span>
7. <span data-ttu-id="982ab-133">Nyissa meg a másolt mappát, a Microsoft Update önálló csomagfájl kell megjelennie `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="982ab-133">Open the copied folder, you should see a Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="982ab-134">A frissítés vagy gyorsjavítás telepítése a fájllal.</span><span class="sxs-lookup"><span data-stu-id="982ab-134">This file is used to install the update or hotfix.</span></span>

### <a name="install-the-update-or-the-hotfix"></a><span data-ttu-id="982ab-135">A frissítés vagy gyorsjavítás telepítése</span><span class="sxs-lookup"><span data-stu-id="982ab-135">Install the update or the hotfix</span></span>
<span data-ttu-id="982ab-136">A frissítés vagy gyorsjavítás telepítése előtt győződjön meg arról, hogy a frissítést, vagy a gyorsjavítás letöltése vagy a gazdagépen helyileg vagy egy hálózati megosztáson keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="982ab-136">Prior to the update or hotfix installation, make sure that you have the update or the hotfix downloaded either locally on your host or accessible via a network share.</span></span> 

<span data-ttu-id="982ab-137">Ezt a módszert egy GA futtató eszközre telepítse a frissítéseket, vagy 0,1 szoftververziók frissítésére.</span><span class="sxs-lookup"><span data-stu-id="982ab-137">Use this method to install updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="982ab-138">Ez az eljárás kisebb, mint 2 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="982ab-138">This procedure takes less than 2 minutes to complete.</span></span> <span data-ttu-id="982ab-139">Hajtsa végre a következő lépésekkel telepíti a frissítést vagy gyorsjavítást.</span><span class="sxs-lookup"><span data-stu-id="982ab-139">Perform the following steps to install the update or hotfix.</span></span>

#### <a name="to-install-the-update-or-the-hotfix"></a><span data-ttu-id="982ab-140">A frissítés vagy gyorsjavítás telepítése</span><span class="sxs-lookup"><span data-stu-id="982ab-140">To install the update or the hotfix</span></span>
1. <span data-ttu-id="982ab-141">A helyi webes felhasználói felület váltson **karbantartási** > **szoftverfrissítés**.</span><span class="sxs-lookup"><span data-stu-id="982ab-141">In the local web UI, go to **Maintenance** > **Software Update**.</span></span>
   
    ![eszköz frissítése](./media/storsimple-ova-install-update-01/update1m.png)
2. <span data-ttu-id="982ab-143">A **frissítés fájl elérési útját**, írja be a fájl nevét, a frissítés vagy gyorsjavítás.</span><span class="sxs-lookup"><span data-stu-id="982ab-143">In **Update file path**, enter the file name for the update or the hotfix.</span></span> <span data-ttu-id="982ab-144">A frissítés vagy gyorsjavítás telepítési fájl is tallózással, ha egy hálózati megosztáson.</span><span class="sxs-lookup"><span data-stu-id="982ab-144">You can also browse to the update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="982ab-145">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="982ab-145">Click **Apply**.</span></span>
   
    ![eszköz frissítése](./media/storsimple-ova-install-update-01/update2m.png)
3. <span data-ttu-id="982ab-147">A figyelmeztetés akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="982ab-147">A warning is displayed.</span></span> <span data-ttu-id="982ab-148">Ez a megadott van egy egycsomópontos eszköz után a frissítés nincs alkalmazva, az eszköz újraindul, és nincs leállás.</span><span class="sxs-lookup"><span data-stu-id="982ab-148">Given this is a single node device, after the update is applied, the device restarts and there is downtime.</span></span> <span data-ttu-id="982ab-149">Kattintson a pipa ikonra.</span><span class="sxs-lookup"><span data-stu-id="982ab-149">Click the check icon.</span></span>
   
   ![eszköz frissítése](./media/storsimple-ova-install-update-01/update3m.png)
4. <span data-ttu-id="982ab-151">A frissítés elindul.</span><span class="sxs-lookup"><span data-stu-id="982ab-151">The update starts.</span></span> <span data-ttu-id="982ab-152">Az eszköz sikeres frissítése után újraindul.</span><span class="sxs-lookup"><span data-stu-id="982ab-152">After the device is successfully updated, it restarts.</span></span> <span data-ttu-id="982ab-153">Ennek az időtartamnak a helyi felhasználói felülete nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="982ab-153">The local UI is not accessible in this duration.</span></span>
   
    ![eszköz frissítése](./media/storsimple-ova-install-update-01/update5m.png)
5. <span data-ttu-id="982ab-155">Az újraindítás befejeződése után kattintva megnyílik a **bejelentkezés** lap.</span><span class="sxs-lookup"><span data-stu-id="982ab-155">After the restart is complete, you are taken to the **Sign in** page.</span></span> <span data-ttu-id="982ab-156">Győződjön meg arról, hogy az eszköz szoftvere frissítette, a helyi webes felhasználói felület, keresse fel **karbantartási** > **szoftverfrissítés**.</span><span class="sxs-lookup"><span data-stu-id="982ab-156">To verify that the device software has updated, in the local web UI, go to **Maintenance** > **Software Update**.</span></span> <span data-ttu-id="982ab-157">A megjelenített szoftverfrissítési verziót kell **10.0.0.0.0.10288.0** 0,3 frissítés.</span><span class="sxs-lookup"><span data-stu-id="982ab-157">The displayed software version should be **10.0.0.0.0.10288.0** for Update 0.3.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="982ab-158">A szoftver verziójával egy némileg eltérő módon a helyi webes felhasználói felület és a klasszikus Azure portálon jelentés azt.</span><span class="sxs-lookup"><span data-stu-id="982ab-158">We report the software versions in a slightly different way in the local web UI and the Azure classic portal.</span></span> <span data-ttu-id="982ab-159">Például a helyi webes felhasználói Felületét jelent **10.0.0.0.0.10288** és a klasszikus Azure portál jelentések **10.0.10288.0** azonos verziójához.</span><span class="sxs-lookup"><span data-stu-id="982ab-159">For example, the local web UI reports **10.0.0.0.0.10288** and the Azure classic portal reports **10.0.10288.0** for the same version.</span></span> 
   > 
   > 
   
    ![eszköz frissítése](./media/storsimple-ova-install-update-01/update6m.png)

## <a name="use-the-azure-classic-portal"></a><span data-ttu-id="982ab-161">Használja a klasszikus Azure portált</span><span class="sxs-lookup"><span data-stu-id="982ab-161">Use the Azure classic portal</span></span>
<span data-ttu-id="982ab-162">Frissítés 0,2 fut, azt javasoljuk, hogy telepítse a frissítéseket a klasszikus Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="982ab-162">If running Update 0.2, we recommend that you install updates through the Azure classic portal.</span></span> <span data-ttu-id="982ab-163">A portál eljárás a felhasználónak kell vizsgálat, töltse le és telepítse a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="982ab-163">The portal procedure requires the user to scan, download, and then install the updates.</span></span> <span data-ttu-id="982ab-164">Ez az eljárás befejezéséhez körülbelül 7 perc.</span><span class="sxs-lookup"><span data-stu-id="982ab-164">This procedure takes around 7 minutes to complete.</span></span> <span data-ttu-id="982ab-165">Hajtsa végre a következő lépésekkel telepíti a frissítést vagy gyorsjavítást.</span><span class="sxs-lookup"><span data-stu-id="982ab-165">Perform the following steps to install the update or hotfix.</span></span>

[!INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

<span data-ttu-id="982ab-166">A telepítés befejezése után (a feladatok állapota a 100 %-os), nyissa meg a **eszközök > karbantartási > szoftverfrissítések**.</span><span class="sxs-lookup"><span data-stu-id="982ab-166">After the installation is complete (as indicated by job status at 100 %), go to **Devices > Maintenance > Software Updates**.</span></span> <span data-ttu-id="982ab-167">A megjelenített szoftverfrissítési verziószámának 10.0.10288.0 kell lennie.</span><span class="sxs-lookup"><span data-stu-id="982ab-167">The displayed software version should be 10.0.10288.0.</span></span>

![eszköz frissítése](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a><span data-ttu-id="982ab-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="982ab-169">Next steps</span></span>
<span data-ttu-id="982ab-170">További információ [felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="982ab-170">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

