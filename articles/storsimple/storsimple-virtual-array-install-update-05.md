---
title: "a StorSimple virtuális tömb 0,5 módosításának aaaInstall |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello StorSimple virtuális tömb webes felhasználói felület tooapply frissítések segítségével hello Azure portál és a gyorsjavítások módszer"
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
ms.openlocfilehash: c38daa85daa0086e67cf0206d76cb19d9c8b21b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-05-on-your-storsimple-virtual-array"></a><span data-ttu-id="06b7a-103">A StorSimple virtuális tömb 0,5 frissítés telepítése</span><span class="sxs-lookup"><span data-stu-id="06b7a-103">Install Update 0.5 on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="06b7a-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="06b7a-104">Overview</span></span>

<span data-ttu-id="06b7a-105">Ez a cikk ismerteti a hello lépéseket szükséges tooinstall frissítés 0,5 a StorSimple virtuális tömb hello Azure-portálon és hello helyi webes felhasználói felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="06b7a-105">This article describes hello steps required tooinstall Update 0.5 on your StorSimple Virtual Array via hello local web UI and via hello Azure portal.</span></span> <span data-ttu-id="06b7a-106">Meg kell tooapply szoftver frissítéseket vagy gyorsjavításokat tookeep a StorSimple virtuális tömb naprakész.</span><span class="sxs-lookup"><span data-stu-id="06b7a-106">You need tooapply software updates or hotfixes tookeep your StorSimple Virtual Array up-to-date.</span></span>

<span data-ttu-id="06b7a-107">Egy frissítés alkalmazása előtt ajánlott végrehajtása hello kötetek vagy megosztások offline hello először meg, és ezután hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="06b7a-107">Before you apply an update, we recommend that you take hello volumes or shares offline on hello host first and then hello device.</span></span> <span data-ttu-id="06b7a-108">Ez minimalizálja az adatvesztést lehetőségét.</span><span class="sxs-lookup"><span data-stu-id="06b7a-108">This minimizes any possibility of data corruption.</span></span> <span data-ttu-id="06b7a-109">Miután hello kötetek vagy megosztások offline üzemmódban, is gondolja kézi hello eszköz biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="06b7a-109">After hello volumes or shares are offline, you should also take a manual backup of hello device.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="06b7a-110">Frissítés 0,5 megfelel-e túl**10.0.10290.0** szoftverének verziójával, az eszközön.</span><span class="sxs-lookup"><span data-stu-id="06b7a-110">Update 0.5 corresponds too**10.0.10290.0** software version on your device.</span></span> <span data-ttu-id="06b7a-111">Információ a what's new in a frissítés: túl[kibocsátási megjegyzései frissítés 0,5](storsimple-virtual-array-update-05-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="06b7a-111">For information on what is new in this update, go too[Release notes for Update 0.5](storsimple-virtual-array-update-05-release-notes.md).</span></span>
>
> - <span data-ttu-id="06b7a-112">Ha a frissítés 0,2 futtatja, vagy később, akkor javasoljuk, hogy telepítenie hello frissítések hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="06b7a-112">If you are running Update 0.2 or later, we recommend that you install hello updates via hello Azure portal.</span></span> <span data-ttu-id="06b7a-113">Ha a frissítés 0,1 vagy GA szoftver verziója fut, hello gyorsjavítás metódus hello helyi webes felhasználói felület tooinstall frissítés 0,5 keresztül kell használnia.</span><span class="sxs-lookup"><span data-stu-id="06b7a-113">If you are running Update 0.1 or GA software versions, you must use hello hotfix method via hello local web UI tooinstall Update 0.5.</span></span>
>
> - <span data-ttu-id="06b7a-114">Ne feledje, hogy egy frissítés vagy gyorsjavítás telepítése az eszköz újraindul.</span><span class="sxs-lookup"><span data-stu-id="06b7a-114">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="06b7a-115">Adott meg, hogy a StorSimple virtuális tömb hello egycsomópontos eszköz, folyamatban lévő összes i/o megszakad, és az eszköz leállást észlel.</span><span class="sxs-lookup"><span data-stu-id="06b7a-115">Given that hello StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span>

## <a name="use-hello-azure-portal"></a><span data-ttu-id="06b7a-116">Hello Azure portál használata</span><span class="sxs-lookup"><span data-stu-id="06b7a-116">Use hello Azure portal</span></span>

<span data-ttu-id="06b7a-117">Update futtatása 0,2 és újabb verziók, azt javasoljuk, hogy telepítse a frissítéseket hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="06b7a-117">If running Update 0.2 and later, we recommend that you install updates through hello Azure portal.</span></span> <span data-ttu-id="06b7a-118">hello portál eljáráshoz szükséges hello felhasználói tooscan, töltse le és telepítse a hello frissítések.</span><span class="sxs-lookup"><span data-stu-id="06b7a-118">hello portal procedure requires hello user tooscan, download, and then install hello updates.</span></span> <span data-ttu-id="06b7a-119">Ezzel az eljárással toocomplete körülbelül 7 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="06b7a-119">This procedure takes around 7 minutes toocomplete.</span></span> <span data-ttu-id="06b7a-120">Hajtsa végre a következő hello lépések tooinstall hello frissítést vagy gyorsjavítást.</span><span class="sxs-lookup"><span data-stu-id="06b7a-120">Perform hello following steps tooinstall hello update or hotfix.</span></span>

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

<span data-ttu-id="06b7a-121">Hello után egy telepítés befejeződött, nyissa meg tooyour StorSimple Device Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="06b7a-121">After hello installation is complete, go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="06b7a-122">Válassza ki **eszközök** majd válassza ki, és kattintson a frissített hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="06b7a-122">Select **Devices** and then select and click hello device you just updated.</span></span> <span data-ttu-id="06b7a-123">Nyissa meg túl**beállítások > kezelés > Eszközfrissítésekhez**.</span><span class="sxs-lookup"><span data-stu-id="06b7a-123">Go too**Settings > Manage > Device Updates**.</span></span> <span data-ttu-id="06b7a-124">megjelenített hello szoftverének verziójával kell **10.0.10290.0**.</span><span class="sxs-lookup"><span data-stu-id="06b7a-124">hello displayed software version should be **10.0.10290.0**.</span></span>

## <a name="use-hello-local-web-ui"></a><span data-ttu-id="06b7a-125">Hello helyi webes felhasználói felület használata</span><span class="sxs-lookup"><span data-stu-id="06b7a-125">Use hello local web UI</span></span>

<span data-ttu-id="06b7a-126">Két lépésből áll hello helyi webes felhasználói felület használata esetén:</span><span class="sxs-lookup"><span data-stu-id="06b7a-126">There are two steps when using hello local web UI:</span></span>

* <span data-ttu-id="06b7a-127">Hello frissítést vagy gyorsjavítást hello letöltése</span><span class="sxs-lookup"><span data-stu-id="06b7a-127">Download hello update or hello hotfix</span></span>
* <span data-ttu-id="06b7a-128">Hello frissítés vagy gyorsjavítás hello telepítése</span><span class="sxs-lookup"><span data-stu-id="06b7a-128">Install hello update or hello hotfix</span></span>

### <a name="download-hello-update-or-hello-hotfix"></a><span data-ttu-id="06b7a-129">Hello frissítést vagy gyorsjavítást hello letöltése</span><span class="sxs-lookup"><span data-stu-id="06b7a-129">Download hello update or hello hotfix</span></span>

<span data-ttu-id="06b7a-130">Hajtsa végre a következő lépéseket toodownload hello szoftverfrissítést a Microsoft Update katalógus hello hello.</span><span class="sxs-lookup"><span data-stu-id="06b7a-130">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

#### <a name="toodownload-hello-update-or-hello-hotfix"></a><span data-ttu-id="06b7a-131">toodownload hello frissítés vagy hello gyorsjavítás.</span><span class="sxs-lookup"><span data-stu-id="06b7a-131">toodownload hello update or hello hotfix</span></span>

1. <span data-ttu-id="06b7a-132">Indítsa el az Internet Explorert, és keresse meg a túl[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="06b7a-132">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>

2. <span data-ttu-id="06b7a-133">Ha az első alkalommal használja a Microsoft Update katalógus hello ezen a számítógépen, kattintson a **telepítése** amikor felszólító tooinstall hello bővítmény a Microsoft Update katalógusból.</span><span class="sxs-lookup"><span data-stu-id="06b7a-133">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>

3. <span data-ttu-id="06b7a-134">A Microsoft Update katalógus hello hello a keresési mezőbe, írja be a hello Tudásbázis (KB) száma hello gyorsjavítás toodownload szeretné.</span><span class="sxs-lookup"><span data-stu-id="06b7a-134">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload.</span></span> <span data-ttu-id="06b7a-135">Adja meg **4021576** frissítési 0,5, és kattintson a **keresési**.</span><span class="sxs-lookup"><span data-stu-id="06b7a-135">Enter **4021576** for Update 0.5, and then click **Search**.</span></span>
   
    <span data-ttu-id="06b7a-136">hello gyorsjavítás lista megjelenik, például **StorSimple virtuális tömb frissítés 0,5**.</span><span class="sxs-lookup"><span data-stu-id="06b7a-136">hello hotfix listing appears, for example, **StorSimple Virtual Array Update 0.5**.</span></span>
   
    ![Keresés a katalógusban](./media/storsimple-virtual-array-install-update-05/download1.png)

4. <span data-ttu-id="06b7a-138">Kattintson a **Letöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="06b7a-138">Click **Download**.</span></span> 

5. <span data-ttu-id="06b7a-139">Két fájlok toodownload, megjelenik egy *.msu* és egy *.cab* fájlt.</span><span class="sxs-lookup"><span data-stu-id="06b7a-139">You should see two files toodownload, a *.msu* and a *.cab* file.</span></span> <span data-ttu-id="06b7a-140">Töltse le, minden egyes ezen fájlok tooa mappa.</span><span class="sxs-lookup"><span data-stu-id="06b7a-140">Download each of those files tooa folder.</span></span> <span data-ttu-id="06b7a-141">hello mappába másolt tooa hálózati megosztásra, amely elérhető hello eszköz is lehet.</span><span class="sxs-lookup"><span data-stu-id="06b7a-141">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>

6. <span data-ttu-id="06b7a-142">Nyissa meg a hello mappát, ahol hello fájlok találhatók.</span><span class="sxs-lookup"><span data-stu-id="06b7a-142">Open hello folder where hello files are located.</span></span>
    <span data-ttu-id="06b7a-143">![Fájlok hello csomag](./media/storsimple-virtual-array-install-update-05/update05folder.png)</span><span class="sxs-lookup"><span data-stu-id="06b7a-143">![Files in hello package](./media/storsimple-virtual-array-install-update-05/update05folder.png)</span></span>

    <span data-ttu-id="06b7a-144">Lásd:</span><span class="sxs-lookup"><span data-stu-id="06b7a-144">You see:</span></span>
    -  <span data-ttu-id="06b7a-145">A Microsoft Update önálló csomagfájl `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="06b7a-145">A Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="06b7a-146">Ez a fájl egy használt tooupdate hello eszköz szoftver.</span><span class="sxs-lookup"><span data-stu-id="06b7a-146">This file is used tooupdate hello device software.</span></span>
    - <span data-ttu-id="06b7a-147">Geneva figyelési ügynök csomagfájl `GenevaMonitoringAgentPackageInstaller`.</span><span class="sxs-lookup"><span data-stu-id="06b7a-147">A Geneva Monitoring Agent Package file `GenevaMonitoringAgentPackageInstaller`.</span></span> <span data-ttu-id="06b7a-148">A fájl használt tooupdate hello megfigyelési és diagnosztikai szolgáltatás (MDS) ügynök.</span><span class="sxs-lookup"><span data-stu-id="06b7a-148">This file is used tooupdate hello Monitoring and Diagnostics service (MDS) agent.</span></span> <span data-ttu-id="06b7a-149">Kattintson duplán a cab-fájl hello.</span><span class="sxs-lookup"><span data-stu-id="06b7a-149">Double-click hello cab file.</span></span> <span data-ttu-id="06b7a-150">Egy .msi jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="06b7a-150">A .msi is displayed.</span></span> <span data-ttu-id="06b7a-151">Válassza hello fájlt, kattintson a jobb gombbal, majd **kibontása** hello fájl.</span><span class="sxs-lookup"><span data-stu-id="06b7a-151">Select hello file, right-click, and then **Extract** hello file.</span></span> <span data-ttu-id="06b7a-152">Hello használandó _.msi_ fájl tooupdate hello ügynök.</span><span class="sxs-lookup"><span data-stu-id="06b7a-152">You will use hello _.msi_ file tooupdate hello agent.</span></span>

        ![Az MDS-ügynök frissítése fájl kibontása](./media/storsimple-virtual-array-install-update-05/extract-geneva-monitoring-agent-installer.png)
        
    

### <a name="install-hello-update-or-hello-hotfix"></a><span data-ttu-id="06b7a-154">Hello frissítés vagy gyorsjavítás hello telepítése</span><span class="sxs-lookup"><span data-stu-id="06b7a-154">Install hello update or hello hotfix</span></span>

<span data-ttu-id="06b7a-155">Előzetes toohello frissítés vagy gyorsjavítás telepítési, győződjön meg arról, hogy hello frissítést vagy hello gyorsjavítás letöltése vagy a gazdagépen helyileg vagy egy hálózati megosztáson keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="06b7a-155">Prior toohello update or hotfix installation, make sure that you have hello update or hello hotfix downloaded either locally on your host or accessible via a network share.</span></span>

<span data-ttu-id="06b7a-156">A metódus tooinstall frissítések használatához GA futtató eszközök, vagy 0,1 szoftververziók frissítése.</span><span class="sxs-lookup"><span data-stu-id="06b7a-156">Use this method tooinstall updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="06b7a-157">Ez az eljárás kisebb, mint 2 percet toocomplete vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="06b7a-157">This procedure takes less than 2 minutes toocomplete.</span></span> <span data-ttu-id="06b7a-158">Hajtsa végre a következő hello lépések tooinstall hello frissítést vagy gyorsjavítást.</span><span class="sxs-lookup"><span data-stu-id="06b7a-158">Perform hello following steps tooinstall hello update or hotfix.</span></span>

#### <a name="tooinstall-hello-update-or-hello-hotfix"></a><span data-ttu-id="06b7a-159">tooinstall hello frissítés vagy hello gyorsjavítás.</span><span class="sxs-lookup"><span data-stu-id="06b7a-159">tooinstall hello update or hello hotfix</span></span>

1. <span data-ttu-id="06b7a-160">Hello helyi webes felhasználói felületén, nyissa meg túl**karbantartási** > **szoftverfrissítés**.</span><span class="sxs-lookup"><span data-stu-id="06b7a-160">In hello local web UI, go too**Maintenance** > **Software Update**.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. <span data-ttu-id="06b7a-162">A **frissítés fájl elérési útját**adja meg a fájlnevet hello hello frissítés vagy gyorsjavítás hello.</span><span class="sxs-lookup"><span data-stu-id="06b7a-162">In **Update file path**, enter hello file name for hello update or hello hotfix.</span></span> <span data-ttu-id="06b7a-163">Toohello frissítés vagy gyorsjavítás telepítési fájl is megkeresheti, ha egy hálózati megosztáson.</span><span class="sxs-lookup"><span data-stu-id="06b7a-163">You can also browse toohello update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="06b7a-164">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="06b7a-164">Click **Apply**.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. <span data-ttu-id="06b7a-166">A figyelmeztetés akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="06b7a-166">A warning is displayed.</span></span> <span data-ttu-id="06b7a-167">Ez a megadott van egy egycsomópontos eszköz után hello frissítés, hello eszköz újraindul, és nincs leállás.</span><span class="sxs-lookup"><span data-stu-id="06b7a-167">Given this is a single node device, after hello update is applied, hello device restarts and there is downtime.</span></span> <span data-ttu-id="06b7a-168">Kattintson a pipa ikonra hello.</span><span class="sxs-lookup"><span data-stu-id="06b7a-168">Click hello check icon.</span></span>
   
   ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. <span data-ttu-id="06b7a-170">hello frissítés elindul.</span><span class="sxs-lookup"><span data-stu-id="06b7a-170">hello update starts.</span></span> <span data-ttu-id="06b7a-171">Hello eszköz sikeres frissítése után újraindul.</span><span class="sxs-lookup"><span data-stu-id="06b7a-171">After hello device is successfully updated, it restarts.</span></span> <span data-ttu-id="06b7a-172">hello helyi felhasználói felület mpelement nem érhető el ennek az időtartamnak.</span><span class="sxs-lookup"><span data-stu-id="06b7a-172">hello local UI is not accessible in this duration.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. <span data-ttu-id="06b7a-174">Hello újraindítás befejezése után a következő lépés az toohello **bejelentkezés** lap.</span><span class="sxs-lookup"><span data-stu-id="06b7a-174">After hello restart is complete, you are taken toohello **Sign in** page.</span></span> <span data-ttu-id="06b7a-175">tooverify hello eszközhöz frissített, hello helyi webes felhasználói felületén, nyissa meg túl**karbantartási** > **szoftverfrissítés**.</span><span class="sxs-lookup"><span data-stu-id="06b7a-175">tooverify that hello device software has updated, in hello local web UI, go too**Maintenance** > **Software Update**.</span></span> <span data-ttu-id="06b7a-176">megjelenített hello szoftverének verziójával kell **10.0.0.0.0.10290.0** 0,5 frissítés.</span><span class="sxs-lookup"><span data-stu-id="06b7a-176">hello displayed software version should be **10.0.0.0.0.10290.0** for Update 0.5.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="06b7a-177">A Microsoft hello helyi webes felhasználói felület némileg eltérő módon hello szoftververziók jelentést, és hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="06b7a-177">We report hello software versions in a slightly different way in hello local web UI and hello Azure portal.</span></span> <span data-ttu-id="06b7a-178">Például a helyi webes felhasználói felület jelentések hello **10.0.0.0.0.10290** és az Azure portál jelentések hello **10.0.10290.0** hello az azonos verzióját.</span><span class="sxs-lookup"><span data-stu-id="06b7a-178">For example, hello local web UI reports **10.0.0.0.0.10290** and hello Azure portal reports **10.0.10290.0** for hello same version.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update6m.png)

6. <span data-ttu-id="06b7a-180">következő lépés hello tooupdate hello MDS-ügynök.</span><span class="sxs-lookup"><span data-stu-id="06b7a-180">hello next step is tooupdate hello MDS agent.</span></span> <span data-ttu-id="06b7a-181">A hello **szoftverfrissítés** lapon, nyissa meg toohello **frissítés fájl elérési útját** , és keresse meg a toohello `GenevaMonitoringAgentPackageInstaller.msi` fájlt.</span><span class="sxs-lookup"><span data-stu-id="06b7a-181">In hello **Software Update** page, go toohello **Update file path** and browse toohello `GenevaMonitoringAgentPackageInstaller.msi` file.</span></span> <span data-ttu-id="06b7a-182">Ismételje meg a 2 – 4.</span><span class="sxs-lookup"><span data-stu-id="06b7a-182">Repeat steps 2-4.</span></span> <span data-ttu-id="06b7a-183">Hello virtuális tömb újraindítása után jelentkezzen be hello helyi webes felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="06b7a-183">After hello virtual array restarts, sign into hello local web UI.</span></span>

<span data-ttu-id="06b7a-184">hello frissítés most már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="06b7a-184">hello update is now complete.</span></span>

## <a name="next-steps"></a><span data-ttu-id="06b7a-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06b7a-185">Next steps</span></span>

<span data-ttu-id="06b7a-186">További információ [felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="06b7a-186">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

