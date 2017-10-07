---
title: "a Microsoft Azure StorSimple virtuális tömb aaaInstall frissítések |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello StorSimple virtuális tömb webes felhasználói felület tooapply frissíti hello portál és a gyorsjavítások a módszerrel"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 9997a97b-9382-43ed-b56e-61369335c987
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7424abc7e46d4f08b4eae1194642b263f32c4318
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-updates-on-your-storsimple-virtual-array---azure-portal"></a><span data-ttu-id="17f3e-103">Telepítse a frissítéseket a StorSimple virtuális tömb – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="17f3e-103">Install Updates on your StorSimple Virtual Array - Azure portal</span></span>

## <a name="overview"></a><span data-ttu-id="17f3e-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="17f3e-104">Overview</span></span>

<span data-ttu-id="17f3e-105">Ez a cikk ismerteti a hello lépéseket szükséges tooinstall frissítések a StorSimple virtuális tömb hello Azure-portálon és hello helyi webes felhasználói felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="17f3e-105">This article describes hello steps required tooinstall updates on your StorSimple Virtual Array via hello local web UI and via hello Azure portal.</span></span> <span data-ttu-id="17f3e-106">Meg kell tooapply szoftver frissítéseket vagy gyorsjavításokat tookeep a StorSimple virtuális tömb naprakész.</span><span class="sxs-lookup"><span data-stu-id="17f3e-106">You need tooapply software updates or hotfixes tookeep your StorSimple Virtual Array up-to-date.</span></span> 

<span data-ttu-id="17f3e-107">Ne feledje, hogy egy frissítés vagy gyorsjavítás telepítése az eszköz újraindul.</span><span class="sxs-lookup"><span data-stu-id="17f3e-107">Keep in mind that installing an update or hotfix restarts your device.</span></span> <span data-ttu-id="17f3e-108">Adott meg, hogy a StorSimple virtuális tömb hello egycsomópontos eszköz, folyamatban lévő összes i/o megszakad, és az eszköz leállást észlel.</span><span class="sxs-lookup"><span data-stu-id="17f3e-108">Given that hello StorSimple Virtual Array is a single node device, any I/O in progress is disrupted and your device experiences downtime.</span></span> 

<span data-ttu-id="17f3e-109">Egy frissítés alkalmazása előtt ajánlott végrehajtása hello kötetek vagy megosztások offline hello először meg, és ezután hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="17f3e-109">Before you apply an update, we recommend that you take hello volumes or shares offline on hello host first and then hello device.</span></span> <span data-ttu-id="17f3e-110">Ez minimalizálja az adatvesztést lehetőségét.</span><span class="sxs-lookup"><span data-stu-id="17f3e-110">This minimizes any possibility of data corruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="17f3e-111">Ha futtatja a frissítési 0,1 vagy GA szoftververziók, hello helyi webes felhasználói felület tooinstall Update 0,3 hello gyorsjavítás metódust kell használnia.</span><span class="sxs-lookup"><span data-stu-id="17f3e-111">If you are running Update 0.1 or GA software versions, you must use hello hotfix method via hello local web UI tooinstall update 0.3.</span></span> <span data-ttu-id="17f3e-112">Ha futtatja a frissítési 0,2, ajánlott hello a klasszikus Azure portálon keresztül hello frissítések telepítése.</span><span class="sxs-lookup"><span data-stu-id="17f3e-112">If you are running Update 0.2, we recommend that you install hello updates via hello Azure classic portal.</span></span>
 

## <a name="use-hello-local-web-ui"></a><span data-ttu-id="17f3e-113">Hello helyi webes felhasználói felület használata</span><span class="sxs-lookup"><span data-stu-id="17f3e-113">Use hello local web UI</span></span>

<span data-ttu-id="17f3e-114">Két lépésből áll hello helyi webes felhasználói felület használata esetén:</span><span class="sxs-lookup"><span data-stu-id="17f3e-114">There are two steps when using hello local web UI:</span></span>

* <span data-ttu-id="17f3e-115">Hello frissítést vagy gyorsjavítást hello letöltése</span><span class="sxs-lookup"><span data-stu-id="17f3e-115">Download hello update or hello hotfix</span></span>
* <span data-ttu-id="17f3e-116">Hello frissítés vagy gyorsjavítás hello telepítése</span><span class="sxs-lookup"><span data-stu-id="17f3e-116">Install hello update or hello hotfix</span></span>

### <a name="download-hello-update-or-hello-hotfix"></a><span data-ttu-id="17f3e-117">Hello frissítést vagy gyorsjavítást hello letöltése</span><span class="sxs-lookup"><span data-stu-id="17f3e-117">Download hello update or hello hotfix</span></span>

<span data-ttu-id="17f3e-118">Hajtsa végre a következő lépéseket toodownload hello szoftverfrissítést a Microsoft Update katalógus hello hello.</span><span class="sxs-lookup"><span data-stu-id="17f3e-118">Perform hello following steps toodownload hello software update from hello Microsoft Update Catalog.</span></span>

#### <a name="toodownload-hello-update-or-hello-hotfix"></a><span data-ttu-id="17f3e-119">toodownload hello frissítés vagy hello gyorsjavítás.</span><span class="sxs-lookup"><span data-stu-id="17f3e-119">toodownload hello update or hello hotfix</span></span>

1. <span data-ttu-id="17f3e-120">Indítsa el az Internet Explorert, és keresse meg a túl[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="17f3e-120">Start Internet Explorer and navigate too[http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).</span></span>

2. <span data-ttu-id="17f3e-121">Ha az első alkalommal használja a Microsoft Update katalógus hello ezen a számítógépen, kattintson a **telepítése** amikor felszólító tooinstall hello bővítmény a Microsoft Update katalógusból.</span><span class="sxs-lookup"><span data-stu-id="17f3e-121">If this is your first time using hello Microsoft Update Catalog on this computer, click **Install** when prompted tooinstall hello Microsoft Update Catalog add-on.</span></span>

3. <span data-ttu-id="17f3e-122">A Microsoft Update katalógus hello hello a keresési mezőbe, írja be a hello Tudásbázis (KB) száma hello gyorsjavítás toodownload szeretné.</span><span class="sxs-lookup"><span data-stu-id="17f3e-122">In hello search box of hello Microsoft Update Catalog, enter hello Knowledge Base (KB) number of hello hotfix you want toodownload.</span></span> <span data-ttu-id="17f3e-123">Adja meg **3182061** frissítési 0,3, és kattintson a **keresési**.</span><span class="sxs-lookup"><span data-stu-id="17f3e-123">Enter **3182061** for Update 0.3, and then click **Search**.</span></span>
   
    <span data-ttu-id="17f3e-124">hello gyorsjavítás lista megjelenik, például **StorSimple virtuális tömb frissítés 0,3**.</span><span class="sxs-lookup"><span data-stu-id="17f3e-124">hello hotfix listing appears, for example, **StorSimple Virtual Array Update 0.3**.</span></span>
   
    ![Keresés a katalógusban](./media/storsimple-virtual-array-install-update/download1.png)

4. <span data-ttu-id="17f3e-126">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="17f3e-126">Click **Add**.</span></span> <span data-ttu-id="17f3e-127">hello frissítés hozzá legyen adva toohello kosár.</span><span class="sxs-lookup"><span data-stu-id="17f3e-127">hello update is added toohello basket.</span></span>

5. <span data-ttu-id="17f3e-128">Kattintson a **Kosár megtekintése** gombra.</span><span class="sxs-lookup"><span data-stu-id="17f3e-128">Click **View Basket**.</span></span>

6. <span data-ttu-id="17f3e-129">Kattintson a **Letöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="17f3e-129">Click **Download**.</span></span> <span data-ttu-id="17f3e-130">Adja meg vagy **Tallózás** tooa helyi helyének hello tooappear tölti le.</span><span class="sxs-lookup"><span data-stu-id="17f3e-130">Specify or **Browse** tooa local location where you want hello downloads tooappear.</span></span> <span data-ttu-id="17f3e-131">hello frissítések letöltése toohello megadott helyre, majd egy almappát azonos nevet hello frissítésként hello helyezi.</span><span class="sxs-lookup"><span data-stu-id="17f3e-131">hello updates are downloaded toohello specified location and placed in a subfolder with hello same name as hello update.</span></span> <span data-ttu-id="17f3e-132">hello mappába másolt tooa hálózati megosztásra, amely elérhető hello eszköz is lehet.</span><span class="sxs-lookup"><span data-stu-id="17f3e-132">hello folder can also be copied tooa network share that is reachable from hello device.</span></span>

7. <span data-ttu-id="17f3e-133">Nyitott hello mappába másolta, a Microsoft Update önálló csomagfájl kell megjelennie `WindowsTH-KB3011067-x64`.</span><span class="sxs-lookup"><span data-stu-id="17f3e-133">Open hello copied folder, you should see a Microsoft Update Standalone Package file `WindowsTH-KB3011067-x64`.</span></span> <span data-ttu-id="17f3e-134">Ez a fájl használt tooinstall hello frissítést vagy gyorsjavítást.</span><span class="sxs-lookup"><span data-stu-id="17f3e-134">This file is used tooinstall hello update or hotfix.</span></span>

### <a name="install-hello-update-or-hello-hotfix"></a><span data-ttu-id="17f3e-135">Hello frissítés vagy gyorsjavítás hello telepítése</span><span class="sxs-lookup"><span data-stu-id="17f3e-135">Install hello update or hello hotfix</span></span>

<span data-ttu-id="17f3e-136">Előzetes toohello frissítés vagy gyorsjavítás telepítési, győződjön meg arról, hogy hello frissítést vagy hello gyorsjavítás letöltése vagy a gazdagépen helyileg vagy egy hálózati megosztáson keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="17f3e-136">Prior toohello update or hotfix installation, make sure that you have hello update or hello hotfix downloaded either locally on your host or accessible via a network share.</span></span> 

<span data-ttu-id="17f3e-137">A metódus tooinstall frissítések használatához GA futtató eszközök, vagy 0,1 szoftververziók frissítése.</span><span class="sxs-lookup"><span data-stu-id="17f3e-137">Use this method tooinstall updates on a device running GA or Update 0.1 software versions.</span></span> <span data-ttu-id="17f3e-138">Ez az eljárás kisebb, mint 2 percet toocomplete vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="17f3e-138">This procedure takes less than 2 minutes toocomplete.</span></span> <span data-ttu-id="17f3e-139">Hajtsa végre a következő hello lépések tooinstall hello frissítést vagy gyorsjavítást.</span><span class="sxs-lookup"><span data-stu-id="17f3e-139">Perform hello following steps tooinstall hello update or hotfix.</span></span>

#### <a name="tooinstall-hello-update-or-hello-hotfix"></a><span data-ttu-id="17f3e-140">tooinstall hello frissítés vagy hello gyorsjavítás.</span><span class="sxs-lookup"><span data-stu-id="17f3e-140">tooinstall hello update or hello hotfix</span></span>

1. <span data-ttu-id="17f3e-141">Hello helyi webes felhasználói felületén, nyissa meg túl**karbantartási** > **szoftverfrissítés**.</span><span class="sxs-lookup"><span data-stu-id="17f3e-141">In hello local web UI, go too**Maintenance** > **Software Update**.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update/update1m.png)

2. <span data-ttu-id="17f3e-143">A **frissítés fájl elérési útját**adja meg a fájlnevet hello hello frissítés vagy gyorsjavítás hello.</span><span class="sxs-lookup"><span data-stu-id="17f3e-143">In **Update file path**, enter hello file name for hello update or hello hotfix.</span></span> <span data-ttu-id="17f3e-144">Toohello frissítés vagy gyorsjavítás telepítési fájl is megkeresheti, ha egy hálózati megosztáson.</span><span class="sxs-lookup"><span data-stu-id="17f3e-144">You can also browse toohello update or hotfix installation file if placed on a network share.</span></span> <span data-ttu-id="17f3e-145">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="17f3e-145">Click **Apply**.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update/update2m.png)

3. <span data-ttu-id="17f3e-147">A figyelmeztetés akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="17f3e-147">A warning is displayed.</span></span> <span data-ttu-id="17f3e-148">Ez a megadott van egy egycsomópontos eszköz után hello frissítés, hello eszköz újraindul, és nincs leállás.</span><span class="sxs-lookup"><span data-stu-id="17f3e-148">Given this is a single node device, after hello update is applied, hello device restarts and there is downtime.</span></span> <span data-ttu-id="17f3e-149">Kattintson a pipa ikonra hello.</span><span class="sxs-lookup"><span data-stu-id="17f3e-149">Click hello check icon.</span></span>
   
   ![eszköz frissítése](./media/storsimple-virtual-array-install-update/update3m.png)

4. <span data-ttu-id="17f3e-151">hello frissítés elindul.</span><span class="sxs-lookup"><span data-stu-id="17f3e-151">hello update starts.</span></span> <span data-ttu-id="17f3e-152">Hello eszköz sikeres frissítése után újraindul.</span><span class="sxs-lookup"><span data-stu-id="17f3e-152">After hello device is successfully updated, it restarts.</span></span> <span data-ttu-id="17f3e-153">hello helyi felhasználói felület mpelement nem érhető el ennek az időtartamnak.</span><span class="sxs-lookup"><span data-stu-id="17f3e-153">hello local UI is not accessible in this duration.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update/update5m.png)

5. <span data-ttu-id="17f3e-155">Hello újraindítás befejezése után a következő lépés az toohello **bejelentkezés** lap.</span><span class="sxs-lookup"><span data-stu-id="17f3e-155">After hello restart is complete, you are taken toohello **Sign in** page.</span></span> <span data-ttu-id="17f3e-156">tooverify hello eszközhöz frissített, hello helyi webes felhasználói felületén, nyissa meg túl**karbantartási** > **szoftverfrissítés**.</span><span class="sxs-lookup"><span data-stu-id="17f3e-156">tooverify that hello device software has updated, in hello local web UI, go too**Maintenance** > **Software Update**.</span></span> <span data-ttu-id="17f3e-157">megjelenített hello szoftverének verziójával kell **10.0.0.0.0.10288.0** 0,3 frissítés.</span><span class="sxs-lookup"><span data-stu-id="17f3e-157">hello displayed software version should be **10.0.0.0.0.10288.0** for Update 0.3.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="17f3e-158">A Microsoft hello helyi webes felhasználói felület némileg eltérő módon hello szoftververziók jelentést, és hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="17f3e-158">We report hello software versions in a slightly different way in hello local web UI and hello Azure portal.</span></span> <span data-ttu-id="17f3e-159">Például a helyi webes felhasználói felület jelentések hello **10.0.0.0.0.10288** és az Azure portál jelentések hello **10.0.10288.0** hello az azonos verzióját.</span><span class="sxs-lookup"><span data-stu-id="17f3e-159">For example, hello local web UI reports **10.0.0.0.0.10288** and hello Azure portal reports **10.0.10288.0** for hello same version.</span></span>
   
    ![eszköz frissítése](./media/storsimple-virtual-array-install-update/update6m.png)

## <a name="use-hello-azure-portal"></a><span data-ttu-id="17f3e-161">Hello Azure portál használata</span><span class="sxs-lookup"><span data-stu-id="17f3e-161">Use hello Azure portal</span></span>

<span data-ttu-id="17f3e-162">Frissítés 0,2 fut, azt javasoljuk, hogy telepítse a frissítéseket hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="17f3e-162">If running Update 0.2, we recommend that you install updates through hello Azure portal.</span></span> <span data-ttu-id="17f3e-163">hello portál eljáráshoz szükséges hello felhasználói tooscan, töltse le és telepítse a hello frissítések.</span><span class="sxs-lookup"><span data-stu-id="17f3e-163">hello portal procedure requires hello user tooscan, download, and then install hello updates.</span></span> <span data-ttu-id="17f3e-164">Ezzel az eljárással toocomplete körülbelül 7 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="17f3e-164">This procedure takes around 7 minutes toocomplete.</span></span> <span data-ttu-id="17f3e-165">Hajtsa végre a következő hello lépések tooinstall hello frissítést vagy gyorsjavítást.</span><span class="sxs-lookup"><span data-stu-id="17f3e-165">Perform hello following steps tooinstall hello update or hotfix.</span></span>

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal.md)]

<span data-ttu-id="17f3e-166">Hello után egy telepítés befejeződött (a feladatok állapota a 100 %-os), nyissa meg tooyour StorSimple Device Manager szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="17f3e-166">After hello installation is complete (as indicated by job status at 100 %), go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="17f3e-167">Válassza ki **eszközök** majd válassza ki, és kattintson a kívánt eszközök csatlakoztatott toothis szolgáltatás hello listája tooupdate hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="17f3e-167">Select **Devices** and then select and click hello device you want tooupdate from hello list of devices connected toothis service.</span></span> <span data-ttu-id="17f3e-168">A hello **beállítások** panelen, lépjen túl**kezelése** válassza ki azt **eszközfrissítésekhez**.</span><span class="sxs-lookup"><span data-stu-id="17f3e-168">In hello **Settings** blade, go too**Manage** section and select **Device updates**.</span></span> <span data-ttu-id="17f3e-169">megjelenített hello szoftverének verziójával kell **10.0.10288.0**.</span><span class="sxs-lookup"><span data-stu-id="17f3e-169">hello displayed software version should be **10.0.10288.0**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="17f3e-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="17f3e-170">Next steps</span></span>

<span data-ttu-id="17f3e-171">További információ [felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="17f3e-171">Learn more about [administering your StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).</span></span>

