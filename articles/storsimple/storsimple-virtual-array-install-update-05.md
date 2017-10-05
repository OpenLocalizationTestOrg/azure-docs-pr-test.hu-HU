---
title: "A frissítés 0,5 a StorSimple virtuális tömb |} Microsoft Docs"
description: "A StorSimple virtuális tömb webes felhasználói felület használata az Azure portál és a gyorsjavítások metódussal frissítés alkalmazása"
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
ms.date: 05/10/2017
ms.author: alkohli
ms.openlocfilehash: c47da5b90c16e2d5b5709e2a6affc026238b9468
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="install-update-05-on-your-storsimple-virtual-array"></a><span data-ttu-id="71e92-103">A StorSimple virtuális tömb 0,5 frissítés telepítése</span><span class="sxs-lookup"><span data-stu-id="71e92-103">Install Update 0.5 on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="71e92-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="71e92-104">Overview</span></span>

<span data-ttu-id="71e92-105">Ez a cikk ismerteti a StorSimple virtuális tömb 0,5 frissítés telepítéséhez, és az Azure-portál a helyi webes felhasználói felületen keresztül szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="71e92-105">This article describes the steps required to install Update 0.5 on your StorSimple Virtual Array via the local web UI and via the Azure portal.</span></span> <span data-ttu-id="71e92-106">Szeretné alkalmazni a szoftverfrissítések vagy gyorsjavítások a StorSimple virtuális tömb naprakész állapotban tartása érdekében.</span><span class="sxs-lookup"><span data-stu-id="71e92-106">You need to apply software updates or hotfixes to keep your StorSimple Virtual Array up-to-date.</span></span>

<span data-ttu-id="71e92-107">Egy frissítés alkalmazása előtt javasoljuk, hogy szánjon a kötetek vagy megosztások offline állapotba a gazdagépen első, majd az eszközről.</span><span class="sxs-lookup"><span data-stu-id="71e92-107">Before you apply an update, we recommend that you take the volumes or shares offline on the host first and then the device.</span></span> <span data-ttu-id="71e92-108">Ez minimalizálja az adatvesztést lehetőségét.</span><span class="sxs-lookup"><span data-stu-id="71e92-108">This minimizes any possibility of data corruption.</span></span> <span data-ttu-id="71e92-109">Után a kötetek vagy megosztások offline üzemmódban, akkor kell vennie kézi biztonsági mentési eszköz.</span><span class="sxs-lookup"><span data-stu-id="71e92-109">After the volumes or shares are offline, you should also take a manual backup of the device.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="71e92-110">Frissítés 0,5 megfelel-e **10.0.10290.0** szoftverének verziójával, az eszközön.</span><span class="sxs-lookup"><span data-stu-id="71e92-110">Update 0.5 corresponds to **10.0.10290.0** software version on your device.</span></span> <span data-ttu-id="71e92-111">What's new in a frissítés információkért látogasson el [kibocsátási megjegyzései frissítés 0,5](storsimple-virtual-array-update-05-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="71e92-111">For information on what is new in this update, go to [Release notes for Update 0.5](storsimple-virtual-array-update-05-release-notes.md).</span></span>
>
> - <span data-ttu-id="71e92-112">Ha a frissítés 0,2 futtatja, vagy később, akkor javasoljuk, hogy az Azure-portálon a frissítések telepítése.</span><span class="sxs-lookup"><span data-stu-id="71e92-112">If you are running Update 0.2 or later, we recommend that you install the updates via the Azure portal.</span></span> <span data-ttu-id="71e92-113">Ha a frissítés 0,1 vagy GA szoftver verziója fut, a gyorsjavítás módszer a helyi webes felhasználói felületen keresztül frissítés 0,5 kell használnia.</span><span class="sxs-lookup"><span data-stu-id="71e92-113">If you are running Update 0.1 or GA software versions, you must use the hotfix method via the local web UI to install Update 0.5.</span></span>
>
> - <span data-ttu-id="71e92-114">Ne feledje, hogy egy frissítés vagy gyorsjavítás telepítése az eszköz újraindul.</span><span class="sxs-lookup"><span data-stu-id="71e92-114">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="71e92-115">Fényében, hogy a StorSimple virtuális tömb a egycsomópontos eszközről szó, a folyamatban lévő összes i/o megszakad, és az eszköz leállást észlel.</span><span class="sxs-lookup"><span data-stu-id="71e92-115">Given that the StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span>

## <a name="use-the-azure-portal"></a><span data-ttu-id="71e92-116">Az Azure Portal használata</span><span class="sxs-lookup"><span data-stu-id="71e92-116">Use the Azure portal</span></span>

<span data-ttu-id="71e92-117">Ha Update futtatása 0,2 és újabb verziók, azt javasoljuk, hogy telepítse a frissítéseket az Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="71e92-117">If running Update 0.2 and later, we recommend that you install updates through the Azure portal.</span></span> <span data-ttu-id="71e92-118">A portál eljárás a felhasználónak kell vizsgálat, töltse le és telepítse a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="71e92-118">The portal procedure requires the user to scan, download, and then install the updates.</span></span> <span data-ttu-id="71e92-119">Ez az eljárás befejezéséhez körülbelül 7 perc.</span><span class="sxs-lookup"><span data-stu-id="71e92-119">This procedure takes around 7 minutes to complete.</span></span> <span data-ttu-id="71e92-120">Hajtsa végre a következő lépésekkel telepíti a frissítést vagy gyorsjavítást.</span><span class="sxs-lookup"><span data-stu-id="71e92-120">Perform the following steps to install the update or hotfix.</span></span>

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

<span data-ttu-id="71e92-121">A telepítés befejezése után, nyissa meg a StorSimple eszköz Manager szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="71e92-121">After the installation is complete, go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="71e92-122">Válassza ki **eszközök** majd válassza ki, és kattintson az eszköz nemrég frissített.</span><span class="sxs-lookup"><span data-stu-id="71e92-122">Select **Devices** and then select and click the device you just updated.</span></span> <span data-ttu-id="71e92-123">Ugrás a **beállítások > kezelése > Eszközfrissítésekhez**.</span><span class="sxs-lookup"><span data-stu-id="71e92-123">Go to **Settings > Manage > Device Updates**.</span></span> <span data-ttu-id="71e92-124">A megjelenített szoftverfrissítési verziót kell **10.0.10290.0**.</span><span class="sxs-lookup"><span data-stu-id="71e92-124">The displayed software version should be **10.0.10290.0**.</span></span>

## <a name="use-the-local-web-ui"></a><span data-ttu-id="71e92-125">A helyi webes felhasználói felület használata</span><span class="sxs-lookup"><span data-stu-id="71e92-125">Use the local web UI</span></span>

<span data-ttu-id="71e92-126">Két lépésben történik a helyi webes felhasználói felület használata esetén:</span><span class="sxs-lookup"><span data-stu-id="71e92-126">There are two steps when using the local web UI:</span></span>

* <span data-ttu-id="71e92-127">A frissítés vagy gyorsjavítás letöltése</span><span class="sxs-lookup"><span data-stu-id="71e92-127">Download the update or the hotfix</span></span>
* <span data-ttu-id="71e92-128">A frissítés vagy gyorsjavítás telepítése</span><span class="sxs-lookup"><span data-stu-id="71e92-128">Install the update or the hotfix</span></span>

### <a name="download-the-update-or-the-hotfix"></a><span data-ttu-id="71e92-129">A frissítés vagy gyorsjavítás letöltése</span><span class="sxs-lookup"><span data-stu-id="71e92-129">Download the update or the hotfix</span></span>

<span data-ttu-id="71e92-130">Hajtsa végre a következő lépéseket a szoftverfrissítés a Microsoft Update katalógusból történő letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="71e92-130">Perform the following steps to download the software update from the Microsoft Update Catalog.</span></span>

#### <a name="to-download-the-update-or-the-hotfix"></a><span data-ttu-id="71e92-131">A frissítés vagy gyorsjavítás letöltése</span><span class="sxs-lookup"><span data-stu-id="71e92-131">To download the update or the hotfix</span></span>

1. <span data-ttu-id="71e92-132">Indítsa el az Internet Explorert, és keresse fel a [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com) címet.</span><span class="sxs-lookup"><span data-stu-id="71e92-132">Start Internet Explorer and navigate to [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>

2. <span data-ttu-id="71e92-133">Ha most használja először a Microsoft Update katalógust ezen a számítógépen, kattintson a **Telepítés** gombra, amikor a rendszer a Microsoft Update katalógus beépülő moduljának telepítésére kéri.</span><span class="sxs-lookup"><span data-stu-id="71e92-133">If this is your first time using the Microsoft Update Catalog on this computer, click **Install** when prompted to install the Microsoft Update Catalog add-on.</span></span>

3. <span data-ttu-id="71e92-134">A keresési mezőbe, a Microsoft Update katalógus adja meg a Tudásbázis (KB) le szeretné tölteni a gyorsjavítást.</span><span class="sxs-lookup"><span data-stu-id="71e92-134">In the search box of the Microsoft Update Catalog, enter the Knowledge Base (KB) number of the hotfix you want to download.</span></span> <span data-ttu-id="71e92-135">Adja meg **4021576** frissítési 0,5, és kattintson a **keresési**.</span><span class="sxs-lookup"><span data-stu-id="71e92-135">Enter **4021576** for Update 0.5, and then click **Search**.</span></span>
   
    <span data-ttu-id="71e92-136">A gyorsjavítás-lista megjelenik, például **StorSimple virtuális tömb frissítés 0,5**.</span><span class="sxs-lookup"><span data-stu-id="71e92-136">The hotfix listing appears, for example, **StorSimple Virtual Array Update 0.5**.</span></span>
   
    ![Keresés a katalógusban](./media/storsimple-virtual-array-install-update-05/download1.png)

4. <span data-ttu-id="71e92-138">Kattintson a **Letöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="71e92-138">Click **Download**.</span></span> 

5. <span data-ttu-id="71e92-139">Két fájlok letöltéséhez, megjelenik egy *.msu* és egy *.cab* fájlt.</span><span class="sxs-lookup"><span data-stu-id="71e92-139">You should see two files to download, a *.msu* and a *.cab* file.</span></span> <span data-ttu-id="71e92-140">Töltse le ezeket a fájlokat egy mappát.</span><span class="sxs-lookup"><span data-stu-id="71e92-140">Download each of those files to a folder.</span></span> <span data-ttu-id="71e92-141">A mappa átmásolható egy, az eszközről elérhető hálózati megosztásra is.</span><span class="sxs-lookup"><span data-stu-id="71e92-141">The folder can also be copied to a network share that is reachable from the device.</span></span>

6. <span data-ttu-id="71e92-142">Nyissa meg a fájlokat tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="71e92-142">Open the folder where the files are located.</span></span>
    <span data-ttu-id="71e92-143">![A csomagban lévő fájlok](./media/storsimple-virtual-array-install-update-05/update05folder.png)</span><span class="sxs-lookup"><span data-stu-id="71e92-143">![Files in the package](./media/storsimple-virtual-array-install-update-05/update05folder.png)</span></span>

    <span data-ttu-id="71e92-144">Lásd:</span><span class="sxs-lookup"><span data-stu-id="71e92-144">You see:</span></span>
    -  <span data-ttu-id="71e92-145">A Microsoft Update önálló csomagfájl `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="71e92-145">A Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="71e92-146">Ezt a fájlt az eszköz frissítéséhez használt.</span><span class="sxs-lookup"><span data-stu-id="71e92-146">This file is used to update the device software.</span></span>
    - <span data-ttu-id="71e92-147">Geneva figyelési ügynök csomagfájl `GenevaMonitoringAgentPackageInstaller`.</span><span class="sxs-lookup"><span data-stu-id="71e92-147">A Geneva Monitoring Agent Package file `GenevaMonitoringAgentPackageInstaller`.</span></span> <span data-ttu-id="71e92-148">A megfigyelési és diagnosztikai szolgáltatás (MDS) ügynök frissítése a fájllal.</span><span class="sxs-lookup"><span data-stu-id="71e92-148">This file is used to update the Monitoring and Diagnostics service (MDS) agent.</span></span> <span data-ttu-id="71e92-149">Kattintson duplán a cab-fájl.</span><span class="sxs-lookup"><span data-stu-id="71e92-149">Double-click the cab file.</span></span> <span data-ttu-id="71e92-150">Egy .msi jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="71e92-150">A .msi is displayed.</span></span> <span data-ttu-id="71e92-151">Válassza ki a fájlt, kattintson a jobb gombbal, majd **kibontása** a fájlt.</span><span class="sxs-lookup"><span data-stu-id="71e92-151">Select the file, right-click, and then **Extract** the file.</span></span> <span data-ttu-id="71e92-152">Használhatja a _.msi_ fájlt az ügynök frissítése.</span><span class="sxs-lookup"><span data-stu-id="71e92-152">You will use the _.msi_ file to update the agent.</span></span>

        ![Az MDS-ügynök frissítése fájl kibontása](./media/storsimple-virtual-array-install-update-05/extract-geneva-monitoring-agent-installer.png)
        
    

### <a name="install-the-update-or-the-hotfix"></a><span data-ttu-id="71e92-154">A frissítés vagy gyorsjavítás telepítése</span><span class="sxs-lookup"><span data-stu-id="71e92-154">Install the update or the hotfix</span></span>

<span data-ttu-id="71e92-155">A frissítés vagy gyorsjavítás telepítése előtt győződjön meg arról, hogy a frissítést, vagy a gyorsjavítás letöltése vagy a gazdagépen helyileg vagy egy hálózati megosztáson keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="71e92-155">Prior to the update or hotfix installation, make sure that you have the update or the hotfix downloaded either locally on your host or accessible via a network share.</span></span>

<span data-ttu-id="71e92-156">Ezt a módszert egy GA futtató eszközre telepítse a frissítéseket, vagy 0,1 szoftververziók frissítésére.</span><span class="sxs-lookup"><span data-stu-id="71e92-156">Use this method to install updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="71e92-157">Ez az eljárás kisebb, mint 2 percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="71e92-157">This procedure takes less than 2 minutes to complete.</span></span> <span data-ttu-id="71e92-158">Hajtsa végre a következő lépésekkel telepíti a frissítést vagy gyorsjavítást.</span><span class="sxs-lookup"><span data-stu-id="71e92-158">Perform the following steps to install the update or hotfix.</span></span>

#### <a name="to-install-the-update-or-the-hotfix"></a><span data-ttu-id="71e92-159">A frissítés vagy gyorsjavítás telepítése</span><span class="sxs-lookup"><span data-stu-id="71e92-159">To install the update or the hotfix</span></span>

1. <span data-ttu-id="71e92-160">A helyi webes felhasználói felület váltson **karbantartási** > **szoftverfrissítés**.</span><span class="sxs-lookup"><span data-stu-id="71e92-160">In the local web UI, go to **Maintenance** > **Software Update**.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. <span data-ttu-id="71e92-162">A **frissítés fájl elérési útját**, írja be a fájl nevét, a frissítés vagy gyorsjavítás.</span><span class="sxs-lookup"><span data-stu-id="71e92-162">In **Update file path**, enter the file name for the update or the hotfix.</span></span> <span data-ttu-id="71e92-163">A frissítés vagy gyorsjavítás telepítési fájl is tallózással, ha egy hálózati megosztáson.</span><span class="sxs-lookup"><span data-stu-id="71e92-163">You can also browse to the update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="71e92-164">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="71e92-164">Click **Apply**.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. <span data-ttu-id="71e92-166">A figyelmeztetés akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="71e92-166">A warning is displayed.</span></span> <span data-ttu-id="71e92-167">Ez a megadott van egy egycsomópontos eszköz után a frissítés nincs alkalmazva, az eszköz újraindul, és nincs leállás.</span><span class="sxs-lookup"><span data-stu-id="71e92-167">Given this is a single node device, after the update is applied, the device restarts and there is downtime.</span></span> <span data-ttu-id="71e92-168">Kattintson a pipa ikonra.</span><span class="sxs-lookup"><span data-stu-id="71e92-168">Click the check icon.</span></span>
   
   ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. <span data-ttu-id="71e92-170">A frissítés elindul.</span><span class="sxs-lookup"><span data-stu-id="71e92-170">The update starts.</span></span> <span data-ttu-id="71e92-171">Az eszköz sikeres frissítése után újraindul.</span><span class="sxs-lookup"><span data-stu-id="71e92-171">After the device is successfully updated, it restarts.</span></span> <span data-ttu-id="71e92-172">Ennek az időtartamnak a helyi felhasználói felülete nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="71e92-172">The local UI is not accessible in this duration.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. <span data-ttu-id="71e92-174">Az újraindítás befejeződése után kattintva megnyílik a **bejelentkezés** lap.</span><span class="sxs-lookup"><span data-stu-id="71e92-174">After the restart is complete, you are taken to the **Sign in** page.</span></span> <span data-ttu-id="71e92-175">Győződjön meg arról, hogy az eszköz szoftvere frissítette, a helyi webes felhasználói felület, keresse fel **karbantartási** > **szoftverfrissítés**.</span><span class="sxs-lookup"><span data-stu-id="71e92-175">To verify that the device software has updated, in the local web UI, go to **Maintenance** > **Software Update**.</span></span> <span data-ttu-id="71e92-176">A megjelenített szoftverfrissítési verziót kell **10.0.0.0.0.10290.0** 0,5 frissítés.</span><span class="sxs-lookup"><span data-stu-id="71e92-176">The displayed software version should be **10.0.0.0.0.10290.0** for Update 0.5.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="71e92-177">A szoftver verziójával egy némileg eltérő módon a helyi webes felhasználói felület és az Azure-portálon jelentés azt.</span><span class="sxs-lookup"><span data-stu-id="71e92-177">We report the software versions in a slightly different way in the local web UI and the Azure portal.</span></span> <span data-ttu-id="71e92-178">Például a helyi webes felhasználói Felületét jelent **10.0.0.0.0.10290** és az Azure portál jelentések **10.0.10290.0** azonos verziójához.</span><span class="sxs-lookup"><span data-stu-id="71e92-178">For example, the local web UI reports **10.0.0.0.0.10290** and the Azure portal reports **10.0.10290.0** for the same version.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update6m.png)

6. <span data-ttu-id="71e92-180">A következő lépés, hogy frissítse az MDS-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="71e92-180">The next step is to update the MDS agent.</span></span> <span data-ttu-id="71e92-181">A a **szoftverfrissítés** lap megnyitásához válassza a **frissítés fájl elérési útját** , és keresse meg a `GenevaMonitoringAgentPackageInstaller.msi` fájl.</span><span class="sxs-lookup"><span data-stu-id="71e92-181">In the **Software Update** page, go to the **Update file path** and browse to the `GenevaMonitoringAgentPackageInstaller.msi` file.</span></span> <span data-ttu-id="71e92-182">Ismételje meg a 2 – 4.</span><span class="sxs-lookup"><span data-stu-id="71e92-182">Repeat steps 2-4.</span></span> <span data-ttu-id="71e92-183">A virtuális tömb újraindítása után jelentkezzen be a helyi webes felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="71e92-183">After the virtual array restarts, sign into the local web UI.</span></span>

<span data-ttu-id="71e92-184">A frissítés most már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="71e92-184">The update is now complete.</span></span>

## <a name="next-steps"></a><span data-ttu-id="71e92-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="71e92-185">Next steps</span></span>

<span data-ttu-id="71e92-186">További információ [felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="71e92-186">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

