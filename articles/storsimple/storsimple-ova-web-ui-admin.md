---
title: "A StorSimple virtuális tömb webes felhasználói felület felügyeleti |} Microsoft Docs"
description: "Ismerteti, hogyan hajthat végre alapszintű eszköz felügyeleti feladatokat a StorSimple virtuális tömb webes felhasználói felületen."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: ea65b4c7-a478-43e6-83df-1d9ea62916a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: 989e7b697f9b527df549fb32be18edd1d3c8d224
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a><span data-ttu-id="7abea-103">A webes felhasználói felület segítségével felügyelheti a StorSimple virtuális tömb</span><span class="sxs-lookup"><span data-stu-id="7abea-103">Use the Web UI to administer your StorSimple Virtual Array</span></span>
![a telepítő folyamatábra](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a><span data-ttu-id="7abea-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="7abea-105">Overview</span></span>
<span data-ttu-id="7abea-106">Ebben a cikkben az oktatóanyagok vonatkoznak a Microsoft Azure StorSimple virtuális tömb (más néven a helyszíni virtuális eszközét) 2016. március általános elérhetőségével (GA) verzióját futtatja.</span><span class="sxs-lookup"><span data-stu-id="7abea-106">The tutorials in this article apply to the Microsoft Azure StorSimple Virtual Array (also known as the StorSimple on-premises virtual device) running March 2016 general availability (GA) release.</span></span> <span data-ttu-id="7abea-107">Ez a cikk ismerteti az egyes a bonyolult munkafolyamatok és a feladat, amely a StorSimple virtuális tömb hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="7abea-107">This article describes some of the complex workflows and management tasks that can be performed on the StorSimple Virtual Array.</span></span> <span data-ttu-id="7abea-108">Kezelheti a StorSimple virtuális tömb, használja a StorSimple Manager szolgáltatás (néven a portál felhasználói felületének) felhasználói felület és az eszköz helyi webes felhasználói Felületét.</span><span class="sxs-lookup"><span data-stu-id="7abea-108">You can manage the StorSimple Virtual Array using the StorSimple Manager service UI (referred to as the portal UI) and the local web UI for the device.</span></span> <span data-ttu-id="7abea-109">Ez a cikk összpontosít, hogy a webes felhasználói felületen végrehajtható műveletek.</span><span class="sxs-lookup"><span data-stu-id="7abea-109">This article focuses on the tasks that you can perform using the web UI.</span></span>

<span data-ttu-id="7abea-110">Ez a cikk tartalmazza az alábbi oktatóanyagok:</span><span class="sxs-lookup"><span data-stu-id="7abea-110">This article includes the following tutorials:</span></span>

* <span data-ttu-id="7abea-111">A szolgáltatásadat-titkosítási kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="7abea-111">Get the service data encryption key</span></span>
* <span data-ttu-id="7abea-112">Webes felhasználói felület telepítő hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="7abea-112">Troubleshoot web UI setup errors</span></span>
* <span data-ttu-id="7abea-113">A napló-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="7abea-113">Generate a log package</span></span>
* <span data-ttu-id="7abea-114">Állítsa le és indítsa újra az eszközt</span><span class="sxs-lookup"><span data-stu-id="7abea-114">Shut down or restart your device</span></span>

## <a name="get-the-service-data-encryption-key"></a><span data-ttu-id="7abea-115">A szolgáltatásadat-titkosítási kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="7abea-115">Get the service data encryption key</span></span>
<span data-ttu-id="7abea-116">A szolgáltatásadat-titkosítási kulcs jön létre, amikor az első eszköz regisztrálása a StorSimple Manager szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="7abea-116">A service data encryption key is generated when you register your first device with the StorSimple Manager service.</span></span> <span data-ttu-id="7abea-117">Ezt a kulcsot meg kell majd további eszközök regisztrálása a StorSimple Manager szolgáltatás a szolgáltatás regisztrációs kulcsával szükséges.</span><span class="sxs-lookup"><span data-stu-id="7abea-117">This key is then required with the service registration key to register additional devices with the StorSimple Manager service.</span></span>

<span data-ttu-id="7abea-118">Ha a szolgáltatásadat-titkosítási kulcs, és ennek lekéréséhez szükséges feljegyezni hajtsa végre a következő lépéseket a helyi webes felhasználói felület az eszköz regisztrálva a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="7abea-118">If you have misplaced your service data encryption key and need to retrieve it, perform the following steps in the local web UI of the device registered with your service.</span></span>

#### <a name="to-get-the-service-data-encryption-key"></a><span data-ttu-id="7abea-119">A szolgáltatásadat-titkosítási kulcs segítségével</span><span class="sxs-lookup"><span data-stu-id="7abea-119">To get the service data encryption key</span></span>
1. <span data-ttu-id="7abea-120">Csatlakozás a helyi webes felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="7abea-120">Connect to the local web UI.</span></span> <span data-ttu-id="7abea-121">Ugrás a **konfigurációs** > **Felhőbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="7abea-121">Go to **Configuration** > **Cloud Settings**.</span></span>
2. <span data-ttu-id="7abea-122">Kattintson a lap alján **Get szolgáltatásadat-titkosítási kulcs**.</span><span class="sxs-lookup"><span data-stu-id="7abea-122">At the bottom of the page, click **Get service data encryption key**.</span></span> <span data-ttu-id="7abea-123">A kulcs jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7abea-123">A key will appear.</span></span> <span data-ttu-id="7abea-124">Másolja ki és mentse ezt a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="7abea-124">Copy and save this key.</span></span>
   
    ![szolgáltatásadat-titkosítási kulcs 1 beolvasása](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a><span data-ttu-id="7abea-126">Webes felhasználói felület telepítő hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="7abea-126">Troubleshoot web UI setup errors</span></span>
<span data-ttu-id="7abea-127">Bizonyos esetekben amikor konfigurálja az eszközt a helyi webes felhasználói felületén keresztül mutatjuk be hibák.</span><span class="sxs-lookup"><span data-stu-id="7abea-127">In some instances when you configure the device through the local web UI, you might run into errors.</span></span> <span data-ttu-id="7abea-128">A diagnosztikai tesztek diagnosztizálhatja és ilyen hibák elhárítása, futtassa.</span><span class="sxs-lookup"><span data-stu-id="7abea-128">To diagnose and troubleshoot such errors, you can run the diagnostics tests.</span></span>

#### <a name="to-run-the-diagnostic-tests"></a><span data-ttu-id="7abea-129">A diagnosztikai tesztek futtatásához</span><span class="sxs-lookup"><span data-stu-id="7abea-129">To run the diagnostic tests</span></span>
1. <span data-ttu-id="7abea-130">A helyi webes felhasználói felület váltson **hibaelhárítás** > **diagnosztikai tesztek**.</span><span class="sxs-lookup"><span data-stu-id="7abea-130">In the local web UI, go to **Troubleshooting** > **Diagnostic tests**.</span></span>
   
    ![1 diagnosztika futtatása](./media/storsimple-ova-web-ui-admin/image29.png)
2. <span data-ttu-id="7abea-132">Kattintson a lap alján **diagnosztikai tesztek futtatása**.</span><span class="sxs-lookup"><span data-stu-id="7abea-132">At the bottom of the page, click **Run Diagnostic Tests**.</span></span> <span data-ttu-id="7abea-133">A szolgáltatás kezdeményez a hálózati, az eszköz, a webalkalmazás-proxy, minden lehetséges problémák elemzéséhez tesztek során, és a felhő beállításait.</span><span class="sxs-lookup"><span data-stu-id="7abea-133">This will initiate tests to diagnose any possible issues with your network, device, web proxy, time, or cloud settings.</span></span> <span data-ttu-id="7abea-134">Értesítést fog kapni, hogy az eszköz teszteket futtat.</span><span class="sxs-lookup"><span data-stu-id="7abea-134">You will be notified that the device is running tests.</span></span>
3. <span data-ttu-id="7abea-135">Miután a teszt befejeződött, az eredmények jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7abea-135">After the tests have completed, the results will be displayed.</span></span> <span data-ttu-id="7abea-136">A következő példa bemutatja a diagnosztikai tesztek eredményét.</span><span class="sxs-lookup"><span data-stu-id="7abea-136">The following example shows the results of diagnostic tests.</span></span> <span data-ttu-id="7abea-137">Vegye figyelembe, hogy a webproxy beállításai nem lettek konfigurálva az eszközön, és ezért a webalkalmazás-proxy teszt nem futott.</span><span class="sxs-lookup"><span data-stu-id="7abea-137">Note that the web proxy settings were not configured on this device, and therefore, the web proxy test was not run.</span></span> <span data-ttu-id="7abea-138">Hálózati beállítások, a DNS-kiszolgáló és az időbeállítást minden más vizsgálatok sikeresek voltak.</span><span class="sxs-lookup"><span data-stu-id="7abea-138">All the other tests for network settings, DNS server, and time settings were successful.</span></span>
   
    ![2 diagnosztika futtatása](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a><span data-ttu-id="7abea-140">A napló-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="7abea-140">Generate a log package</span></span>
<span data-ttu-id="7abea-141">Napló csomag magában foglalja a vonatkozó naplókat, hogy bármely eszköz problémák elhárítása a Microsoft Support segít.</span><span class="sxs-lookup"><span data-stu-id="7abea-141">A log package is comprised of all the relevant logs that can assist Microsoft Support with troubleshooting any device issues.</span></span> <span data-ttu-id="7abea-142">Ebben a kiadásban a napló csomag hozhatók létre a helyi webes felhasználói felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="7abea-142">In this release, a log package can be generated via the local web UI.</span></span>

#### <a name="to-generate-the-log-package"></a><span data-ttu-id="7abea-143">A napló-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="7abea-143">To generate the log package</span></span>
1. <span data-ttu-id="7abea-144">A helyi webes felhasználói felület váltson **hibaelhárítás** > **rendszernaplókat**.</span><span class="sxs-lookup"><span data-stu-id="7abea-144">In the local web UI, go to **Troubleshooting** > **System logs**.</span></span>
   
    ![napló csomagot 1](./media/storsimple-ova-web-ui-admin/image31.png)
2. <span data-ttu-id="7abea-146">Kattintson a lap alján **napló-csomag létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7abea-146">At the bottom of the page, click **Create log package**.</span></span> <span data-ttu-id="7abea-147">A rendszer a naplók csomag jön létre.</span><span class="sxs-lookup"><span data-stu-id="7abea-147">A package of the system logs will be created.</span></span> <span data-ttu-id="7abea-148">Ez eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="7abea-148">This will take a couple of minutes.</span></span>
   
    ![napló csomagot 2](./media/storsimple-ova-web-ui-admin/image32.png)
   
    <span data-ttu-id="7abea-150">Értesítést fog kapni a csomag sikeresen létre, és az oldal frissül a csomag létrehozásának dátumát és időpontját jelzi.</span><span class="sxs-lookup"><span data-stu-id="7abea-150">You will be notified after the package is successfully created, and the page will be updated to indicate the time and date when the package was created.</span></span>
   
    ![napló csomagot 3](./media/storsimple-ova-web-ui-admin/image33.png)
3. <span data-ttu-id="7abea-152">Kattintson a **napló letöltőcsomag**.</span><span class="sxs-lookup"><span data-stu-id="7abea-152">Click **Download log package**.</span></span> <span data-ttu-id="7abea-153">A tömörített csomag a rendszeren, tölti le a program.</span><span class="sxs-lookup"><span data-stu-id="7abea-153">A zipped package will be downloaded on your system.</span></span>
   
    ![napló csomagot 4](./media/storsimple-ova-web-ui-admin/image34.png)
4. <span data-ttu-id="7abea-155">Bontsa ki a letöltött napló csomagot, és megtekintheti a rendszernapló fájljaiban.</span><span class="sxs-lookup"><span data-stu-id="7abea-155">You can unzip the downloaded log package and view the system log files.</span></span>

## <a name="shut-down-and-restart-your-device"></a><span data-ttu-id="7abea-156">Állítsa le és indítsa újra az eszközt</span><span class="sxs-lookup"><span data-stu-id="7abea-156">Shut down and restart your device</span></span>
<span data-ttu-id="7abea-157">Állítsa le, illetve újra is indíthatja a virtuális eszközt a helyi webes felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="7abea-157">You can shut down or restart your virtual device using the local web UI.</span></span> <span data-ttu-id="7abea-158">Azt javasoljuk, hogy újraindítás előtt a kötetek vagy megosztások offline állapotba a gazdagép és az eszköz.</span><span class="sxs-lookup"><span data-stu-id="7abea-158">We recommend that before you restart, take the volumes or shares offline on the host and then the device.</span></span> <span data-ttu-id="7abea-159">Ezzel minimálisra csökkentheti adatsérülés lehetőségét.</span><span class="sxs-lookup"><span data-stu-id="7abea-159">This will minimize any possibility of data corruption.</span></span> 

#### <a name="to-shut-down-your-virtual-device"></a><span data-ttu-id="7abea-160">A virtuális eszköz leállítása</span><span class="sxs-lookup"><span data-stu-id="7abea-160">To shut down your virtual device</span></span>
1. <span data-ttu-id="7abea-161">A helyi webes felhasználói felület váltson **karbantartási** > **energiaellátási beállítások**.</span><span class="sxs-lookup"><span data-stu-id="7abea-161">In the local web UI, go to **Maintenance** > **Power settings**.</span></span>
2. <span data-ttu-id="7abea-162">Kattintson a lap alján **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="7abea-162">At the bottom of the page, click **Shutdown**.</span></span>
   
    ![1 eszköz Leállítás](./media/storsimple-ova-web-ui-admin/image36.png)
3. <span data-ttu-id="7abea-164">Megjelenik egy figyelmeztetés, amely meghatározza, hogy, hogy a leállás, az eszköz meg fogja szakítani a bármely IO, amelyek folyamatban volt egy állásidővel.</span><span class="sxs-lookup"><span data-stu-id="7abea-164">A warning will appear stating that a shutdown of the device will interrupt any IO that were in progress, resulting in a downtime.</span></span> <span data-ttu-id="7abea-165">Kattintson a pipa ikonra</span><span class="sxs-lookup"><span data-stu-id="7abea-165">Click the check icon</span></span> ![pipa ikon](./media/storsimple-ova-web-ui-admin/image3.png)<span data-ttu-id="7abea-167">.</span><span class="sxs-lookup"><span data-stu-id="7abea-167">.</span></span>
   
    ![eszköz leállítás figyelmeztetés](./media/storsimple-ova-web-ui-admin/image37.png)
   
    <span data-ttu-id="7abea-169">Értesítést fog kapni, hogy a leállítási inicializálva lett.</span><span class="sxs-lookup"><span data-stu-id="7abea-169">You will be notified that the shutdown has been initiated.</span></span>
   
    ![eszköz leállítás indítása](./media/storsimple-ova-web-ui-admin/image38.png)
   
    <span data-ttu-id="7abea-171">Az eszköz most leáll.</span><span class="sxs-lookup"><span data-stu-id="7abea-171">The device will now shut down.</span></span> <span data-ttu-id="7abea-172">Ha el szeretné indítani az eszközt, akkor ehhez a Hyper-V kezelője segítségével.</span><span class="sxs-lookup"><span data-stu-id="7abea-172">If you want to start your device, you will need to do that through the Hyper-V Manager.</span></span>

#### <a name="to-restart-your-virtual-device"></a><span data-ttu-id="7abea-173">A virtuális eszköz újraindítása</span><span class="sxs-lookup"><span data-stu-id="7abea-173">To restart your virtual device</span></span>
1. <span data-ttu-id="7abea-174">A helyi webes felhasználói felület váltson **karbantartási** > **energiaellátási beállítások**.</span><span class="sxs-lookup"><span data-stu-id="7abea-174">In the local web UI, go to **Maintenance** > **Power settings**.</span></span>
2. <span data-ttu-id="7abea-175">Kattintson a lap alján **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="7abea-175">At the bottom of the page, click **Restart**.</span></span>
   
    ![eszköz újraindítása](./media/storsimple-ova-web-ui-admin/image36.png)
3. <span data-ttu-id="7abea-177">Megjelenik egy figyelmeztetés, amely meghatározza, hogy, hogy az eszköz újraindítása meg fogja szakítani a bármely IOs-, amelyek folyamatban volt egy állásidővel.</span><span class="sxs-lookup"><span data-stu-id="7abea-177">A warning will appear stating that restarting the device will interrupt any IOs that were in progress, resulting in a downtime.</span></span> <span data-ttu-id="7abea-178">Kattintson a pipa ikonra</span><span class="sxs-lookup"><span data-stu-id="7abea-178">Click the check icon</span></span> ![pipa ikon](./media/storsimple-ova-web-ui-admin/image3.png)<span data-ttu-id="7abea-180">.</span><span class="sxs-lookup"><span data-stu-id="7abea-180">.</span></span>
   
    ![Indítsa újra a figyelmeztetés](./media/storsimple-ova-web-ui-admin/image37.png)
   
    <span data-ttu-id="7abea-182">Értesítést fog kapni az újraindítást kezdeményezett.</span><span class="sxs-lookup"><span data-stu-id="7abea-182">You will be notified that the restart has been initiated.</span></span>
   
    ![újraindítást kezdeményezett](./media/storsimple-ova-web-ui-admin/image39.png)
   
    <span data-ttu-id="7abea-184">Az újraindítás folyamatban van, amíg a kapcsolat a felhasználói felület megszűnik.</span><span class="sxs-lookup"><span data-stu-id="7abea-184">While the restart is in progress, you will lose the connection to the UI.</span></span> <span data-ttu-id="7abea-185">Az újraindítás végezhet figyelést, ha a felhasználói felület rendszeresen frissíteni.</span><span class="sxs-lookup"><span data-stu-id="7abea-185">You can monitor the restart by refreshing the UI periodically.</span></span> <span data-ttu-id="7abea-186">Másik lehetőségként az eszköz újraindítása állapotát a Hyper-V kezelője segítségével figyelheti.</span><span class="sxs-lookup"><span data-stu-id="7abea-186">Alternatively, you can monitor the device restart status through the Hyper-V Manager.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7abea-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7abea-187">Next steps</span></span>
<span data-ttu-id="7abea-188">Megtudhatja, hogyan [a StorSimple Manager szolgáltatás használni az eszköz kezelését](storsimple-virtual-array-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="7abea-188">Learn how to [use the StorSimple Manager service to manage your device](storsimple-virtual-array-manager-service-administration.md).</span></span>

