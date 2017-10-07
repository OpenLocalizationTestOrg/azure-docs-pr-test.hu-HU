---
title: "aaaStorSimple virtuális tömb webes felhasználói felület felügyeleti |} Microsoft Docs"
description: "Ismerteti, hogyan tooperform alapvető eszköz felügyeleti feladatok hello StorSimple virtuális tömb webes felhasználói felületen keresztül."
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
ms.openlocfilehash: 31a20a587c4302231f027fcf772a50df33b23407
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-web-ui-tooadminister-your-storsimple-virtual-array"></a><span data-ttu-id="0b634-103">Használja a következő hello webes felhasználói felületén tooadminister a StorSimple virtuális tömböt</span><span class="sxs-lookup"><span data-stu-id="0b634-103">Use hello Web UI tooadminister your StorSimple Virtual Array</span></span>
![a telepítő folyamatábra](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a><span data-ttu-id="0b634-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0b634-105">Overview</span></span>
<span data-ttu-id="0b634-106">hello oktatóanyagok ebben a cikkben toohello Microsoft Azure StorSimple virtuális tömb (más néven hello StorSimple helyszíni virtuális eszköz) futó 2016. március általános elérhetőségével (GA) kiadás vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="0b634-106">hello tutorials in this article apply toohello Microsoft Azure StorSimple Virtual Array (also known as hello StorSimple on-premises virtual device) running March 2016 general availability (GA) release.</span></span> <span data-ttu-id="0b634-107">Ez a cikk ismerteti az egyes hello bonyolult munkafolyamatok és a StorSimple virtuális tömb hello végrehajtható felügyeleti feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="0b634-107">This article describes some of hello complex workflows and management tasks that can be performed on hello StorSimple Virtual Array.</span></span> <span data-ttu-id="0b634-108">Kezelheti a StorSimple virtuális tömb hello StorSimple Manager szolgáltatás (hivatkozott tooas hello portál felhasználói felületének) felhasználói felület használatával hello és helyi webes felhasználói felület hello eszköz hello.</span><span class="sxs-lookup"><span data-stu-id="0b634-108">You can manage hello StorSimple Virtual Array using hello StorSimple Manager service UI (referred tooas hello portal UI) and hello local web UI for hello device.</span></span> <span data-ttu-id="0b634-109">Ez a cikk foglalkozik hello feladatok végrehajtható hello webes felhasználói felület használatával.</span><span class="sxs-lookup"><span data-stu-id="0b634-109">This article focuses on hello tasks that you can perform using hello web UI.</span></span>

<span data-ttu-id="0b634-110">Ez a cikk a következő oktatóanyagok hello tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="0b634-110">This article includes hello following tutorials:</span></span>

* <span data-ttu-id="0b634-111">Hello szolgáltatásadat-titkosítási kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="0b634-111">Get hello service data encryption key</span></span>
* <span data-ttu-id="0b634-112">Webes felhasználói felület telepítő hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="0b634-112">Troubleshoot web UI setup errors</span></span>
* <span data-ttu-id="0b634-113">A napló-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b634-113">Generate a log package</span></span>
* <span data-ttu-id="0b634-114">Állítsa le és indítsa újra az eszközt</span><span class="sxs-lookup"><span data-stu-id="0b634-114">Shut down or restart your device</span></span>

## <a name="get-hello-service-data-encryption-key"></a><span data-ttu-id="0b634-115">Hello szolgáltatásadat-titkosítási kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="0b634-115">Get hello service data encryption key</span></span>
<span data-ttu-id="0b634-116">A szolgáltatásadat-titkosítási kulcs jön létre, amikor az első eszköz regisztrálása a StorSimple Manager szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="0b634-116">A service data encryption key is generated when you register your first device with hello StorSimple Manager service.</span></span> <span data-ttu-id="0b634-117">Ezt a kulcsot meg kell majd hello szolgáltatás regisztrációs kulcs tooregister további eszközök hello StorSimple Manager szolgáltatás szükséges.</span><span class="sxs-lookup"><span data-stu-id="0b634-117">This key is then required with hello service registration key tooregister additional devices with hello StorSimple Manager service.</span></span>

<span data-ttu-id="0b634-118">Ha rendelkezik a szolgáltatásadat-titkosítási kulcs helytelen, és tooretrieve kell azt, tegye a következőket hello lépéseket hello helyi webes felhasználói felület hello eszköz regisztrálva a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="0b634-118">If you have misplaced your service data encryption key and need tooretrieve it, perform hello following steps in hello local web UI of hello device registered with your service.</span></span>

#### <a name="tooget-hello-service-data-encryption-key"></a><span data-ttu-id="0b634-119">tooget hello szolgáltatásadat-titkosítási kulcs</span><span class="sxs-lookup"><span data-stu-id="0b634-119">tooget hello service data encryption key</span></span>
1. <span data-ttu-id="0b634-120">Csatlakozás toohello helyi webes felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="0b634-120">Connect toohello local web UI.</span></span> <span data-ttu-id="0b634-121">Nyissa meg túl**konfigurációs** > **Felhőbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="0b634-121">Go too**Configuration** > **Cloud Settings**.</span></span>
2. <span data-ttu-id="0b634-122">Hello a hello lap alján, kattintson **Get szolgáltatásadat-titkosítási kulcs**.</span><span class="sxs-lookup"><span data-stu-id="0b634-122">At hello bottom of hello page, click **Get service data encryption key**.</span></span> <span data-ttu-id="0b634-123">A kulcs jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0b634-123">A key will appear.</span></span> <span data-ttu-id="0b634-124">Másolja ki és mentse ezt a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="0b634-124">Copy and save this key.</span></span>
   
    ![szolgáltatásadat-titkosítási kulcs 1 beolvasása](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a><span data-ttu-id="0b634-126">Webes felhasználói felület telepítő hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="0b634-126">Troubleshoot web UI setup errors</span></span>
<span data-ttu-id="0b634-127">Bizonyos esetekben hello helyi webes felhasználói felületén keresztül hello eszköz konfigurálásakor mutatjuk be hibák.</span><span class="sxs-lookup"><span data-stu-id="0b634-127">In some instances when you configure hello device through hello local web UI, you might run into errors.</span></span> <span data-ttu-id="0b634-128">toodiagnose és ilyen hibák elhárításával kapcsolatos tudnivalókat hello diagnosztikai tesztek futtatása.</span><span class="sxs-lookup"><span data-stu-id="0b634-128">toodiagnose and troubleshoot such errors, you can run hello diagnostics tests.</span></span>

#### <a name="toorun-hello-diagnostic-tests"></a><span data-ttu-id="0b634-129">toorun hello diagnosztikai tesztek</span><span class="sxs-lookup"><span data-stu-id="0b634-129">toorun hello diagnostic tests</span></span>
1. <span data-ttu-id="0b634-130">Hello helyi webes felhasználói felületén, nyissa meg túl**hibaelhárítás** > **diagnosztikai tesztek**.</span><span class="sxs-lookup"><span data-stu-id="0b634-130">In hello local web UI, go too**Troubleshooting** > **Diagnostic tests**.</span></span>
   
    ![1 diagnosztika futtatása](./media/storsimple-ova-web-ui-admin/image29.png)
2. <span data-ttu-id="0b634-132">Hello a hello lap alján, kattintson **diagnosztikai tesztek futtatása**.</span><span class="sxs-lookup"><span data-stu-id="0b634-132">At hello bottom of hello page, click **Run Diagnostic Tests**.</span></span> <span data-ttu-id="0b634-133">A szolgáltatás kezdeményez tesztek toodiagnose a hálózati, eszköz, webalkalmazás-proxy, idő vagy felhőbeállítások minden lehetséges problémákat.</span><span class="sxs-lookup"><span data-stu-id="0b634-133">This will initiate tests toodiagnose any possible issues with your network, device, web proxy, time, or cloud settings.</span></span> <span data-ttu-id="0b634-134">Értesítést fog kapni, hogy hello eszköz teszteket futtat.</span><span class="sxs-lookup"><span data-stu-id="0b634-134">You will be notified that hello device is running tests.</span></span>
3. <span data-ttu-id="0b634-135">Miután hello teszt befejeződött, hello eredmény jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0b634-135">After hello tests have completed, hello results will be displayed.</span></span> <span data-ttu-id="0b634-136">hello következő példa bemutatja hello diagnosztikai tesztek eredményét.</span><span class="sxs-lookup"><span data-stu-id="0b634-136">hello following example shows hello results of diagnostic tests.</span></span> <span data-ttu-id="0b634-137">Vegye figyelembe, hogy hello webproxy beállításai nem lettek konfigurálva az eszközön, és ezért hello webalkalmazás-proxy teszt nem futott.</span><span class="sxs-lookup"><span data-stu-id="0b634-137">Note that hello web proxy settings were not configured on this device, and therefore, hello web proxy test was not run.</span></span> <span data-ttu-id="0b634-138">Minden egyéb hálózati beállítások, a DNS-kiszolgáló tesztek hello és időbeállítást sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="0b634-138">All hello other tests for network settings, DNS server, and time settings were successful.</span></span>
   
    ![2 diagnosztika futtatása](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a><span data-ttu-id="0b634-140">A napló-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b634-140">Generate a log package</span></span>
<span data-ttu-id="0b634-141">A napló csomag, amely segíthet a Microsoft Support bármely eszköz problémák elhárítása az összes hello megfelelő napló magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="0b634-141">A log package is comprised of all hello relevant logs that can assist Microsoft Support with troubleshooting any device issues.</span></span> <span data-ttu-id="0b634-142">Ebben a kiadásban a napló csomag is generálható hello helyi webes felhasználói felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="0b634-142">In this release, a log package can be generated via hello local web UI.</span></span>

#### <a name="toogenerate-hello-log-package"></a><span data-ttu-id="0b634-143">toogenerate hello napló csomag</span><span class="sxs-lookup"><span data-stu-id="0b634-143">toogenerate hello log package</span></span>
1. <span data-ttu-id="0b634-144">Hello helyi webes felhasználói felületén, nyissa meg túl**hibaelhárítás** > **rendszernaplókat**.</span><span class="sxs-lookup"><span data-stu-id="0b634-144">In hello local web UI, go too**Troubleshooting** > **System logs**.</span></span>
   
    ![napló csomagot 1](./media/storsimple-ova-web-ui-admin/image31.png)
2. <span data-ttu-id="0b634-146">Hello a hello lap alján, kattintson **napló-csomag létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0b634-146">At hello bottom of hello page, click **Create log package**.</span></span> <span data-ttu-id="0b634-147">A csomag hello rendszer naplók jön létre.</span><span class="sxs-lookup"><span data-stu-id="0b634-147">A package of hello system logs will be created.</span></span> <span data-ttu-id="0b634-148">Ez eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="0b634-148">This will take a couple of minutes.</span></span>
   
    ![napló csomagot 2](./media/storsimple-ova-web-ui-admin/image32.png)
   
    <span data-ttu-id="0b634-150">Értesítést fog kapni után hello csomag sikeresen jön létre, és hello oldal frissül tooindicate hello és hello csomag létrehozásának időpontja.</span><span class="sxs-lookup"><span data-stu-id="0b634-150">You will be notified after hello package is successfully created, and hello page will be updated tooindicate hello time and date when hello package was created.</span></span>
   
    ![napló csomagot 3](./media/storsimple-ova-web-ui-admin/image33.png)
3. <span data-ttu-id="0b634-152">Kattintson a **napló letöltőcsomag**.</span><span class="sxs-lookup"><span data-stu-id="0b634-152">Click **Download log package**.</span></span> <span data-ttu-id="0b634-153">A tömörített csomag a rendszeren, tölti le a program.</span><span class="sxs-lookup"><span data-stu-id="0b634-153">A zipped package will be downloaded on your system.</span></span>
   
    ![napló csomagot 4](./media/storsimple-ova-web-ui-admin/image34.png)
4. <span data-ttu-id="0b634-155">Bontsa ki a letöltött napló csomag hello tekinthetők meg és hello rendszernapló fájljaiban.</span><span class="sxs-lookup"><span data-stu-id="0b634-155">You can unzip hello downloaded log package and view hello system log files.</span></span>

## <a name="shut-down-and-restart-your-device"></a><span data-ttu-id="0b634-156">Állítsa le és indítsa újra az eszközt</span><span class="sxs-lookup"><span data-stu-id="0b634-156">Shut down and restart your device</span></span>
<span data-ttu-id="0b634-157">Állítsa le, vagy indítsa újra a virtuális eszköz hello helyi webes felhasználói felület használatával.</span><span class="sxs-lookup"><span data-stu-id="0b634-157">You can shut down or restart your virtual device using hello local web UI.</span></span> <span data-ttu-id="0b634-158">Javasoljuk, hogy indítsa újra, hello kötetek vagy megosztások offline végrehajtása a hello állomás és az eszköz majd hello előtt.</span><span class="sxs-lookup"><span data-stu-id="0b634-158">We recommend that before you restart, take hello volumes or shares offline on hello host and then hello device.</span></span> <span data-ttu-id="0b634-159">Ezzel minimálisra csökkentheti adatsérülés lehetőségét.</span><span class="sxs-lookup"><span data-stu-id="0b634-159">This will minimize any possibility of data corruption.</span></span> 

#### <a name="tooshut-down-your-virtual-device"></a><span data-ttu-id="0b634-160">tooshut le a virtuális eszköz</span><span class="sxs-lookup"><span data-stu-id="0b634-160">tooshut down your virtual device</span></span>
1. <span data-ttu-id="0b634-161">Hello helyi webes felhasználói felületén, nyissa meg túl**karbantartási** > **energiaellátási beállítások**.</span><span class="sxs-lookup"><span data-stu-id="0b634-161">In hello local web UI, go too**Maintenance** > **Power settings**.</span></span>
2. <span data-ttu-id="0b634-162">Hello a hello lap alján, kattintson **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="0b634-162">At hello bottom of hello page, click **Shutdown**.</span></span>
   
    ![1 eszköz Leállítás](./media/storsimple-ova-web-ui-admin/image36.png)
3. <span data-ttu-id="0b634-164">Megjelenik egy figyelmeztetés, amely meghatározza, hogy, hogy a leállás hello eszköz meg fogja szakítani a bármely IO, amelyek folyamatban volt egy állásidővel.</span><span class="sxs-lookup"><span data-stu-id="0b634-164">A warning will appear stating that a shutdown of hello device will interrupt any IO that were in progress, resulting in a downtime.</span></span> <span data-ttu-id="0b634-165">Kattintson a pipa ikonra hello</span><span class="sxs-lookup"><span data-stu-id="0b634-165">Click hello check icon</span></span> ![pipa ikon](./media/storsimple-ova-web-ui-admin/image3.png)<span data-ttu-id="0b634-167">.</span><span class="sxs-lookup"><span data-stu-id="0b634-167">.</span></span>
   
    ![eszköz leállítás figyelmeztetés](./media/storsimple-ova-web-ui-admin/image37.png)
   
    <span data-ttu-id="0b634-169">Értesítést fog kapni hello leállítás inicializálva lett.</span><span class="sxs-lookup"><span data-stu-id="0b634-169">You will be notified that hello shutdown has been initiated.</span></span>
   
    ![eszköz leállítás indítása](./media/storsimple-ova-web-ui-admin/image38.png)
   
    <span data-ttu-id="0b634-171">hello eszköz most leáll.</span><span class="sxs-lookup"><span data-stu-id="0b634-171">hello device will now shut down.</span></span> <span data-ttu-id="0b634-172">Ha azt szeretné, toostart, szüksége lesz, amely a Hyper-V kezelője hello toodo.</span><span class="sxs-lookup"><span data-stu-id="0b634-172">If you want toostart your device, you will need toodo that through hello Hyper-V Manager.</span></span>

#### <a name="toorestart-your-virtual-device"></a><span data-ttu-id="0b634-173">toorestart a virtuális eszköz</span><span class="sxs-lookup"><span data-stu-id="0b634-173">toorestart your virtual device</span></span>
1. <span data-ttu-id="0b634-174">Hello helyi webes felhasználói felületén, nyissa meg túl**karbantartási** > **energiaellátási beállítások**.</span><span class="sxs-lookup"><span data-stu-id="0b634-174">In hello local web UI, go too**Maintenance** > **Power settings**.</span></span>
2. <span data-ttu-id="0b634-175">Hello a hello lap alján, kattintson **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="0b634-175">At hello bottom of hello page, click **Restart**.</span></span>
   
    ![eszköz újraindítása](./media/storsimple-ova-web-ui-admin/image36.png)
3. <span data-ttu-id="0b634-177">Megjelenik egy figyelmeztetés, amely meghatározza, hogy újraindítás hello eszköz meg fogja szakítani a bármely IOs-, amelyek folyamatban volt egy állásidővel.</span><span class="sxs-lookup"><span data-stu-id="0b634-177">A warning will appear stating that restarting hello device will interrupt any IOs that were in progress, resulting in a downtime.</span></span> <span data-ttu-id="0b634-178">Kattintson a pipa ikonra hello</span><span class="sxs-lookup"><span data-stu-id="0b634-178">Click hello check icon</span></span> ![pipa ikon](./media/storsimple-ova-web-ui-admin/image3.png)<span data-ttu-id="0b634-180">.</span><span class="sxs-lookup"><span data-stu-id="0b634-180">.</span></span>
   
    ![Indítsa újra a figyelmeztetés](./media/storsimple-ova-web-ui-admin/image37.png)
   
    <span data-ttu-id="0b634-182">Értesítést fog kapni, hogy hello újraindítást kezdeményezett.</span><span class="sxs-lookup"><span data-stu-id="0b634-182">You will be notified that hello restart has been initiated.</span></span>
   
    ![újraindítást kezdeményezett](./media/storsimple-ova-web-ui-admin/image39.png)
   
    <span data-ttu-id="0b634-184">Hello újraindítása folyamatban van, amíg hello kapcsolat toohello felhasználói felület elvész.</span><span class="sxs-lookup"><span data-stu-id="0b634-184">While hello restart is in progress, you will lose hello connection toohello UI.</span></span> <span data-ttu-id="0b634-185">Hello újraindítás végezhet figyelést, ha felhasználói felület hello rendszeresen frissíteni.</span><span class="sxs-lookup"><span data-stu-id="0b634-185">You can monitor hello restart by refreshing hello UI periodically.</span></span> <span data-ttu-id="0b634-186">Másik lehetőségként hello eszköz újraindítása állapotát a Hyper-V kezelője hello figyelheti.</span><span class="sxs-lookup"><span data-stu-id="0b634-186">Alternatively, you can monitor hello device restart status through hello Hyper-V Manager.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b634-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0b634-187">Next steps</span></span>
<span data-ttu-id="0b634-188">Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás toomanage az eszköz](storsimple-virtual-array-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="0b634-188">Learn how too[use hello StorSimple Manager service toomanage your device](storsimple-virtual-array-manager-service-administration.md).</span></span>

