---
title: "a StorSimple virtuális tömb 0,6 módosításának aaaInstall |} Microsoft Docs"
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
ms.date: 05/18/2017
ms.author: alkohli
ms.openlocfilehash: 2ccd1b5fc1957c35ebec35aa947d331b3ff05464
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-06-on-your-storsimple-virtual-array"></a><span data-ttu-id="0e115-103">A StorSimple virtuális tömb 0,6 frissítés telepítése</span><span class="sxs-lookup"><span data-stu-id="0e115-103">Install Update 0.6 on your StorSimple Virtual Array</span></span>

## <a name="overview"></a><span data-ttu-id="0e115-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0e115-104">Overview</span></span>

<span data-ttu-id="0e115-105">Ez a cikk ismerteti a hello lépéseket szükséges tooinstall frissítés 0,6 a StorSimple virtuális tömb hello Azure-portálon és hello helyi webes felhasználói felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="0e115-105">This article describes hello steps required tooinstall Update 0.6 on your StorSimple Virtual Array via hello local web UI and via hello Azure portal.</span></span> <span data-ttu-id="0e115-106">Hello szoftver frissítéseket vagy gyorsjavításokat tookeep alkalmazni a StorSimple virtuális tömb naprakész.</span><span class="sxs-lookup"><span data-stu-id="0e115-106">You apply hello software updates or hotfixes tookeep your StorSimple Virtual Array up-to-date.</span></span>

<span data-ttu-id="0e115-107">Egy frissítés alkalmazása előtt ajánlott végrehajtása hello kötetek vagy megosztások offline hello először meg, és ezután hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="0e115-107">Before you apply an update, we recommend that you take hello volumes or shares offline on hello host first and then hello device.</span></span> <span data-ttu-id="0e115-108">Ez minimalizálja az adatvesztést lehetőségét.</span><span class="sxs-lookup"><span data-stu-id="0e115-108">This minimizes any possibility of data corruption.</span></span> <span data-ttu-id="0e115-109">Miután hello kötetek vagy megosztások offline üzemmódban, is gondolja kézi hello eszköz biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="0e115-109">After hello volumes or shares are offline, you should also take a manual backup of hello device.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="0e115-110">Frissítés 0,6 megfelel-e túl**10.0.10293.0** szoftverének verziójával, az eszközön.</span><span class="sxs-lookup"><span data-stu-id="0e115-110">Update 0.6 corresponds too**10.0.10293.0** software version on your device.</span></span> <span data-ttu-id="0e115-111">Információ a what's new in a frissítés: túl[kibocsátási megjegyzései frissítés 0,6](storsimple-virtual-array-update-06-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="0e115-111">For information on what is new in this update, go too[Release notes for Update 0.6](storsimple-virtual-array-update-06-release-notes.md).</span></span>
>
> - <span data-ttu-id="0e115-112">Ha a frissítés 0,2 futtatja, vagy később, akkor javasoljuk, hogy telepítenie hello frissítések hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="0e115-112">If you are running Update 0.2 or later, we recommend that you install hello updates via hello Azure portal.</span></span> <span data-ttu-id="0e115-113">Ha a frissítés 0,1 vagy GA szoftver verziója fut, hello gyorsjavítás metódus hello helyi webes felhasználói felület tooinstall frissítés 0,6 keresztül kell használnia.</span><span class="sxs-lookup"><span data-stu-id="0e115-113">If you are running Update 0.1 or GA software versions, you must use hello hotfix method via hello local web UI tooinstall Update 0.6.</span></span>
>
> - <span data-ttu-id="0e115-114">Ne feledje, hogy egy frissítés vagy gyorsjavítás telepítése az eszköz újraindul.</span><span class="sxs-lookup"><span data-stu-id="0e115-114">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="0e115-115">Adott meg, hogy a StorSimple virtuális tömb hello egycsomópontos eszköz, folyamatban lévő összes i/o megszakad, és az eszköz leállást észlel.</span><span class="sxs-lookup"><span data-stu-id="0e115-115">Given that hello StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span>

## <a name="use-hello-azure-portal"></a><span data-ttu-id="0e115-116">Hello Azure portál használata</span><span class="sxs-lookup"><span data-stu-id="0e115-116">Use hello Azure portal</span></span>

<span data-ttu-id="0e115-117">Update futtatása 0,2 és újabb verziók, azt javasoljuk, hogy telepítse a frissítéseket hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="0e115-117">If running Update 0.2 and later, we recommend that you install updates through hello Azure portal.</span></span> <span data-ttu-id="0e115-118">hello portál eljáráshoz szükséges hello felhasználói tooscan, töltse le és telepítse a hello frissítések.</span><span class="sxs-lookup"><span data-stu-id="0e115-118">hello portal procedure requires hello user tooscan, download, and then install hello updates.</span></span> <span data-ttu-id="0e115-119">Ezzel az eljárással toocomplete körülbelül 7 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="0e115-119">This procedure takes around 7 minutes toocomplete.</span></span> <span data-ttu-id="0e115-120">Hajtsa végre a következő hello lépések tooinstall hello frissítést vagy gyorsjavítást.</span><span class="sxs-lookup"><span data-stu-id="0e115-120">Perform hello following steps tooinstall hello update or hotfix.</span></span>

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

<span data-ttu-id="0e115-121">Hello után egy telepítés befejeződött, nyissa meg tooyour StorSimple Device Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0e115-121">After hello installation is complete, go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="0e115-122">Válassza ki **eszközök** majd válassza ki, és kattintson a frissített hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="0e115-122">Select **Devices** and then select and click hello device you just updated.</span></span> <span data-ttu-id="0e115-123">Nyissa meg túl**beállítások > kezelés > Eszközfrissítésekhez**.</span><span class="sxs-lookup"><span data-stu-id="0e115-123">Go too**Settings > Manage > Device Updates**.</span></span> <span data-ttu-id="0e115-124">megjelenített hello szoftverének verziójával kell **10.0.10293.0**.</span><span class="sxs-lookup"><span data-stu-id="0e115-124">hello displayed software version should be **10.0.10293.0**.</span></span>

## <a name="use-hello-local-web-ui"></a><span data-ttu-id="0e115-125">Hello helyi webes felhasználói felület használata</span><span class="sxs-lookup"><span data-stu-id="0e115-125">Use hello local web UI</span></span>

<span data-ttu-id="0e115-126">Két lépésből áll hello helyi webes felhasználói felület használata esetén:</span><span class="sxs-lookup"><span data-stu-id="0e115-126">There are two steps when using hello local web UI:</span></span>

* <span data-ttu-id="0e115-127">Hello frissítést vagy gyorsjavítást hello letöltése</span><span class="sxs-lookup"><span data-stu-id="0e115-127">Download hello update or hello hotfix</span></span>
* <span data-ttu-id="0e115-128">Hello frissítés vagy gyorsjavítás hello telepítése</span><span class="sxs-lookup"><span data-stu-id="0e115-128">Install hello update or hello hotfix</span></span>

### <a name="download-hello-update-or-hello-hotfix"></a><span data-ttu-id="0e115-129">Hello frissítést vagy gyorsjavítást hello letöltése</span><span class="sxs-lookup"><span data-stu-id="0e115-129">Download hello update or hello hotfix</span></span>

<span data-ttu-id="0e115-130">Hajtsa végre a következő lépéseket toodownload hello szoftverfrissítést a Microsoft Update katalógus hello hello.</span><span class="sxs-lookup"><span data-stu-id="0e115-130">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

#### <a name="toodownload-hello-update-or-hello-hotfix"></a><span data-ttu-id="0e115-131">toodownload hello frissítés vagy hello gyorsjavítás.</span><span class="sxs-lookup"><span data-stu-id="0e115-131">toodownload hello update or hello hotfix</span></span>

1. <span data-ttu-id="0e115-132">Indítsa el az Internet Explorert, és keresse meg a túl[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="0e115-132">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>

2. <span data-ttu-id="0e115-133">Ha használ a Microsoft Update katalógus hello hello az első alkalommal ezen a számítógépen, kattintson a **telepítése** amikor felszólító tooinstall hello Microsoft Update katalógus bővítmény.</span><span class="sxs-lookup"><span data-stu-id="0e115-133">If you are using hello Microsoft Update Catalog for hello first time on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>

3. <span data-ttu-id="0e115-134">A Microsoft Update katalógus hello hello a keresési mezőbe, írja be a hello Tudásbázis (KB) száma hello gyorsjavítás toodownload szeretné.</span><span class="sxs-lookup"><span data-stu-id="0e115-134">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload.</span></span> <span data-ttu-id="0e115-135">Adja meg **4023268** frissítési 0,6, és kattintson a **keresési**.</span><span class="sxs-lookup"><span data-stu-id="0e115-135">Enter **4023268** for Update 0.6, and then click **Search**.</span></span>
   
    <span data-ttu-id="0e115-136">hello gyorsjavítás lista megjelenik, például **StorSimple virtuális tömb frissítés 0,6**.</span><span class="sxs-lookup"><span data-stu-id="0e115-136">hello hotfix listing appears, for example, **StorSimple Virtual Array Update 0.6**.</span></span>
   
    ![Keresés a katalógusban](./media/storsimple-virtual-array-install-update-06/download1.png)

4. <span data-ttu-id="0e115-138">Kattintson a **Letöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="0e115-138">Click **Download**.</span></span>

5. <span data-ttu-id="0e115-139">Öt fájlok toodownload kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="0e115-139">You should see five files toodownload.</span></span> <span data-ttu-id="0e115-140">Töltse le, minden egyes ezen fájlok tooa mappa.</span><span class="sxs-lookup"><span data-stu-id="0e115-140">Download each of those files tooa folder.</span></span> <span data-ttu-id="0e115-141">hello mappába másolt tooa hálózati megosztásra, amely elérhető hello eszköz is lehet.</span><span class="sxs-lookup"><span data-stu-id="0e115-141">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>

6. <span data-ttu-id="0e115-142">Nyissa meg a hello mappát, ahol hello fájlok találhatók.</span><span class="sxs-lookup"><span data-stu-id="0e115-142">Open hello folder where hello files are located.</span></span>
    <span data-ttu-id="0e115-143">![Fájlok hello csomag](./media/storsimple-virtual-array-install-update-06/update06folder.png)</span><span class="sxs-lookup"><span data-stu-id="0e115-143">![Files in hello package](./media/storsimple-virtual-array-install-update-06/update06folder.png)</span></span>

    <span data-ttu-id="0e115-144">Lásd:</span><span class="sxs-lookup"><span data-stu-id="0e115-144">You see:</span></span>
    -  <span data-ttu-id="0e115-145">A Microsoft Update önálló csomagfájl `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="0e115-145">A Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="0e115-146">Ez a fájl egy használt tooupdate hello eszköz szoftver.</span><span class="sxs-lookup"><span data-stu-id="0e115-146">This file is used tooupdate hello device software.</span></span>
    - <span data-ttu-id="0e115-147">Geneva figyelési ügynök csomagfájl `GenevaMonitoringAgentPackageInstaller`.</span><span class="sxs-lookup"><span data-stu-id="0e115-147">A Geneva Monitoring Agent Package file `GenevaMonitoringAgentPackageInstaller`.</span></span> <span data-ttu-id="0e115-148">A fájl használt tooupdate hello megfigyelési és diagnosztikai szolgáltatás (MDS) ügynök.</span><span class="sxs-lookup"><span data-stu-id="0e115-148">This file is used tooupdate hello Monitoring and Diagnostics service (MDS) agent.</span></span> <span data-ttu-id="0e115-149">Kattintson duplán a cab-fájl hello.</span><span class="sxs-lookup"><span data-stu-id="0e115-149">Double-click hello cab file.</span></span> <span data-ttu-id="0e115-150">A _.msi_ jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0e115-150">A _.msi_ is displayed.</span></span> <span data-ttu-id="0e115-151">Válassza hello fájlt, kattintson a jobb gombbal, majd **kibontása** hello fájl.</span><span class="sxs-lookup"><span data-stu-id="0e115-151">Select hello file, right-click, and then **Extract** hello file.</span></span> <span data-ttu-id="0e115-152">Hello használata _.msi_ fájl tooupdate hello ügynök.</span><span class="sxs-lookup"><span data-stu-id="0e115-152">You use hello _.msi_ file tooupdate hello agent.</span></span>

        ![Az MDS-ügynök frissítése fájl kibontása](./media/storsimple-virtual-array-install-update-06/extract-geneva-monitoring-agent-installer.png)

        > [!IMPORTANT]
        > <span data-ttu-id="0e115-154">Ha futtatja a StorSimple frissítés 0,5 (0.0.10293.0) nem kell tooupdate hello MDS ügynök.</span><span class="sxs-lookup"><span data-stu-id="0e115-154">You do not need tooupdate hello MDS agent if you are running StorSimple Update 0.5 (0.0.10293.0).</span></span>

    - <span data-ttu-id="0e115-155">Három fontos Windows biztonsági frissítéseket tartalmazó fájlokat `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, és `windows8.1-kb4019213-x64`.</span><span class="sxs-lookup"><span data-stu-id="0e115-155">Three files that contain critical Windows security updates, `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, and `windows8.1-kb4019213-x64`.</span></span>


### <a name="install-hello-update-or-hello-hotfix"></a><span data-ttu-id="0e115-156">Hello frissítés vagy gyorsjavítás hello telepítése</span><span class="sxs-lookup"><span data-stu-id="0e115-156">Install hello update or hello hotfix</span></span>

<span data-ttu-id="0e115-157">Előzetes toohello frissítés vagy gyorsjavítás telepítési, győződjön meg arról, hogy hello frissítést vagy hello gyorsjavítás letöltése vagy a gazdagépen helyileg vagy egy hálózati megosztáson keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="0e115-157">Prior toohello update or hotfix installation, make sure that you have hello update or hello hotfix downloaded either locally on your host or accessible via a network share.</span></span>

<span data-ttu-id="0e115-158">A metódus tooinstall frissítések használatához GA futtató eszközök, vagy 0,1 szoftververziók frissítése.</span><span class="sxs-lookup"><span data-stu-id="0e115-158">Use this method tooinstall updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="0e115-159">Ez az eljárás körülbelül 12 perc toocomplete vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="0e115-159">This procedure takes approximately 12 minutes toocomplete.</span></span> <span data-ttu-id="0e115-160">Hajtsa végre a következő hello lépések tooinstall hello frissítést vagy gyorsjavítást.</span><span class="sxs-lookup"><span data-stu-id="0e115-160">Perform hello following steps tooinstall hello update or hotfix.</span></span>

#### <a name="tooinstall-hello-update-or-hello-hotfix"></a><span data-ttu-id="0e115-161">tooinstall hello frissítés vagy hello gyorsjavítás.</span><span class="sxs-lookup"><span data-stu-id="0e115-161">tooinstall hello update or hello hotfix</span></span>

1. <span data-ttu-id="0e115-162">Hello helyi webes felhasználói felületén, nyissa meg túl**karbantartási** > **szoftverfrissítés**.</span><span class="sxs-lookup"><span data-stu-id="0e115-162">In hello local web UI, go too**Maintenance** > **Software Update**.</span></span> <span data-ttu-id="0e115-163">Jegyezze fel az Ön által futtatott hello szoftverének verziójával.</span><span class="sxs-lookup"><span data-stu-id="0e115-163">Make a note of hello software version that you are running.</span></span> <span data-ttu-id="0e115-164">Ha futtatja **10.0.10290.0**, nem kell tooupdate hello MDS ügynök 6. lépésben.</span><span class="sxs-lookup"><span data-stu-id="0e115-164">If you are running **10.0.10290.0**, you do not need tooupdate hello MDS agent in step 6.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. <span data-ttu-id="0e115-166">A **frissítés fájl elérési útját**adja meg a fájlnevet hello hello frissítés vagy gyorsjavítás hello.</span><span class="sxs-lookup"><span data-stu-id="0e115-166">In **Update file path**, enter hello file name for hello update or hello hotfix.</span></span> <span data-ttu-id="0e115-167">Toohello frissítés vagy gyorsjavítás telepítési fájl is megkeresheti, ha egy hálózati megosztáson.</span><span class="sxs-lookup"><span data-stu-id="0e115-167">You can also browse toohello update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="0e115-168">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="0e115-168">Click **Apply**.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. <span data-ttu-id="0e115-170">A figyelmeztetés akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0e115-170">A warning is displayed.</span></span> <span data-ttu-id="0e115-171">A megadott hello virtuális egy egycsomópontos eszköz hello frissítés, hello eszköz újraindul, és nincs leállás után.</span><span class="sxs-lookup"><span data-stu-id="0e115-171">Given hello virtual array is a single node device, after hello update is applied, hello device restarts and there is downtime.</span></span> <span data-ttu-id="0e115-172">Kattintson a pipa ikonra hello.</span><span class="sxs-lookup"><span data-stu-id="0e115-172">Click hello check icon.</span></span>
   
   ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. <span data-ttu-id="0e115-174">hello frissítés elindul.</span><span class="sxs-lookup"><span data-stu-id="0e115-174">hello update starts.</span></span> <span data-ttu-id="0e115-175">Hello eszköz sikeres frissítése után újraindul.</span><span class="sxs-lookup"><span data-stu-id="0e115-175">After hello device is successfully updated, it restarts.</span></span> <span data-ttu-id="0e115-176">hello helyi felhasználói felület mpelement nem érhető el ennek az időtartamnak.</span><span class="sxs-lookup"><span data-stu-id="0e115-176">hello local UI is not accessible in this duration.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. <span data-ttu-id="0e115-178">Hello újraindítás befejezése után a következő lépés az toohello **bejelentkezés** lap.</span><span class="sxs-lookup"><span data-stu-id="0e115-178">After hello restart is complete, you are taken toohello **Sign in** page.</span></span> <span data-ttu-id="0e115-179">tooverify hello eszközhöz frissített, hello helyi webes felhasználói felületén, nyissa meg túl**karbantartási** > **szoftverfrissítés**.</span><span class="sxs-lookup"><span data-stu-id="0e115-179">tooverify that hello device software has updated, in hello local web UI, go too**Maintenance** > **Software Update**.</span></span> <span data-ttu-id="0e115-180">megjelenített hello szoftverének verziójával kell **10.0.0.0.0.10293** 0,6 frissítés.</span><span class="sxs-lookup"><span data-stu-id="0e115-180">hello displayed software version should be **10.0.0.0.0.10293** for Update 0.6.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0e115-181">A Microsoft hello helyi webes felhasználói felület némileg eltérő módon hello szoftververziók jelentést, és hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="0e115-181">We report hello software versions in a slightly different way in hello local web UI and hello Azure portal.</span></span> <span data-ttu-id="0e115-182">Például a helyi webes felhasználói felület jelentések hello **10.0.0.0.0.10293** és az Azure portál jelentések hello **10.0.10293.0** hello az azonos verzióját.</span><span class="sxs-lookup"><span data-stu-id="0e115-182">For example, hello local web UI reports **10.0.0.0.0.10293** and hello Azure portal reports **10.0.10293.0** for hello same version.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update-06/update6m.png)

6. <span data-ttu-id="0e115-184">Kihagyhatja ezt a lépést, ha a StorSimple virtuális tömb frissítés 0,5 futtatása során (**10.0.10290.0**) a frissítés alkalmazása előtt.</span><span class="sxs-lookup"><span data-stu-id="0e115-184">Skip this step if you were running StorSimple Virtual Array Update 0.5 (**10.0.10290.0**) before you applied this update.</span></span> <span data-ttu-id="0e115-185">Feljegyezte hello szoftverének verziójával, az 1. lépésben tooupdate megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="0e115-185">You made a note of hello software version in step 1 before you began tooupdate.</span></span> <span data-ttu-id="0e115-186">Ha 0,5 frissítés futtatásakor, az MDS-ügynök már naprakész.</span><span class="sxs-lookup"><span data-stu-id="0e115-186">If you were running Update 0.5, your MDS agent is already up-to-date .</span></span>

    <span data-ttu-id="0e115-187">Ha egy szoftver verziója előzetes tooUpdate 0,5 futtat, a hello következő lépés az Ön tooupdate hello MDS ügynök.</span><span class="sxs-lookup"><span data-stu-id="0e115-187">If you are running a software version prior tooUpdate 0.5, hello next step for you is tooupdate hello MDS agent.</span></span> <span data-ttu-id="0e115-188">A hello **szoftverfrissítés** lapon, nyissa meg toohello **frissítés fájl elérési útját** , és keresse meg a toohello `GenevaMonitoringAgentPackageInstaller.msi` fájlt.</span><span class="sxs-lookup"><span data-stu-id="0e115-188">In hello **Software Update** page, go toohello **Update file path** and browse toohello `GenevaMonitoringAgentPackageInstaller.msi` file.</span></span> <span data-ttu-id="0e115-189">Ismételje meg a 2 – 4.</span><span class="sxs-lookup"><span data-stu-id="0e115-189">Repeat steps 2-4.</span></span> <span data-ttu-id="0e115-190">Hello virtuális tömb újraindítása után jelentkezzen be hello helyi webes felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="0e115-190">After hello virtual array restarts, sign into hello local web UI.</span></span>

7. <span data-ttu-id="0e115-191">2 – 4 tooinstall hello Windows biztonsági javítások fájlokkal megismétli `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, és `windows8.1-kb4019213-x64`.</span><span class="sxs-lookup"><span data-stu-id="0e115-191">Repeat step 2-4 tooinstall hello Windows security fixes using files `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, and `windows8.1-kb4019213-x64`.</span></span> <span data-ttu-id="0e115-192">hello virtuális tömb minden telepítése után újraindul, és a hello helyi webes felhasználói felület toosign kell.</span><span class="sxs-lookup"><span data-stu-id="0e115-192">hello virtual array restarts after each install and you need toosign into hello local web UI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0e115-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0e115-193">Next steps</span></span>

<span data-ttu-id="0e115-194">További információ [felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="0e115-194">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

