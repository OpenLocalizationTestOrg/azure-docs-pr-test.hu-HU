---
title: "a Microsoft Azure StorSimple 8000 series eszközön aaaReplace akkumulátor |} Microsoft Docs"
description: "Leírja, hogyan tooremove, cserélje le, és a StorSimple eszköz hello biztonsági mentési akkumulátor modul karbantartása."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 5ac767807e6c3fd817d8d522629db2aceaac9bdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replace-hello-backup-battery-module-on-your-storsimple-device"></a><span data-ttu-id="e96ea-103">Cserélje le a StorSimple eszköz hello biztonsági mentési akkumulátor modul</span><span class="sxs-lookup"><span data-stu-id="e96ea-103">Replace hello backup battery module on your StorSimple device</span></span>

## <a name="overview"></a><span data-ttu-id="e96ea-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e96ea-104">Overview</span></span>
<span data-ttu-id="e96ea-105">hello elsődleges ház teljesítmény- és hűtési modul (PCM) a Microsoft Azure StorSimple eszköz rendelkezik egy további akkumulátor csomagot.</span><span class="sxs-lookup"><span data-stu-id="e96ea-105">hello primary enclosure Power and Cooling Module (PCM) on your Microsoft Azure StorSimple device has an additional battery pack.</span></span> <span data-ttu-id="e96ea-106">Ez a csomag biztosítja a teljesítmény, így hello StorSimple eszköz adat menthető AC power toohello elsődleges ház elvesztése esetén.</span><span class="sxs-lookup"><span data-stu-id="e96ea-106">This pack provides power so that hello StorSimple device can save data if there is loss of AC power toohello primary enclosure.</span></span> <span data-ttu-id="e96ea-107">Ez akkumulátor csomag hivatkozott tooas hello *biztonsági mentési akkumulátor modul*.</span><span class="sxs-lookup"><span data-stu-id="e96ea-107">This battery pack is referred tooas hello *backup battery module*.</span></span> <span data-ttu-id="e96ea-108">csak az elsődleges ház hello a StorSimple eszköz létezik hello biztonsági mentési akkumulátor modul (hello EBOD ház nem tartalmaz a biztonsági mentési akkumulátor modul).</span><span class="sxs-lookup"><span data-stu-id="e96ea-108">hello backup battery module exists only for hello primary enclosure in your StorSimple device (hello EBOD enclosure does not contain a backup battery module).</span></span>

<span data-ttu-id="e96ea-109">Ez az oktatóanyag azt ismerteti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="e96ea-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="e96ea-110">Hello biztonsági mentési akkumulátor modul eltávolítása</span><span class="sxs-lookup"><span data-stu-id="e96ea-110">Remove hello backup battery module</span></span>
* <span data-ttu-id="e96ea-111">Egy új biztonsági mentési akkumulátor modul telepítése</span><span class="sxs-lookup"><span data-stu-id="e96ea-111">Install a new backup battery module</span></span>
* <span data-ttu-id="e96ea-112">Hello biztonsági mentési akkumulátor modul karbantartása</span><span class="sxs-lookup"><span data-stu-id="e96ea-112">Maintain hello backup battery module</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e96ea-113">Mielőtt eltávolítása és cseréje egy biztonsági mentési akkumulátor modult, tekintse át a biztonsági információk hello hello [bemutatása tooStorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="e96ea-113">Before removing and replacing a backup battery module, review hello safety information in hello [Introduction tooStorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>


## <a name="remove-hello-backup-battery-module"></a><span data-ttu-id="e96ea-114">Hello biztonsági mentési akkumulátor modul eltávolítása</span><span class="sxs-lookup"><span data-stu-id="e96ea-114">Remove hello backup battery module</span></span>
<span data-ttu-id="e96ea-115">hello biztonsági mentési akkumulátor modul a StorSimple eszközt a egy terepen cserélhető Cisco egységet.</span><span class="sxs-lookup"><span data-stu-id="e96ea-115">hello backup battery module for your StorSimple device is a field-replaceable unit.</span></span> <span data-ttu-id="e96ea-116">Mielőtt hello PCM van telepítve, az eredeti csomagban hello akkumulátor modul kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="e96ea-116">Before it is installed in hello PCM, hello battery module should be stored in its original packaging.</span></span> <span data-ttu-id="e96ea-117">Hajtsa végre a következő lépéseket tooremove hello biztonsági mentési akkumulátor hello.</span><span class="sxs-lookup"><span data-stu-id="e96ea-117">Perform hello following steps tooremove hello backup battery.</span></span>

#### <a name="tooremove-hello-backup-battery-module"></a><span data-ttu-id="e96ea-118">tooremove hello biztonsági mentési akkumulátor modul</span><span class="sxs-lookup"><span data-stu-id="e96ea-118">tooremove hello backup battery module</span></span>
1. <span data-ttu-id="e96ea-119">A hello Azure-portálon válassza a tooyour StorSimple Device Manager szolgáltatás panelre.</span><span class="sxs-lookup"><span data-stu-id="e96ea-119">In hello Azure portal, go tooyour StorSimple Device Manager service blade.</span></span> <span data-ttu-id="e96ea-120">Nyissa meg túl**eszközök** majd válassza ki az eszköz hello eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="e96ea-120">Go too**Devices** and then select your device from hello list of devices.</span></span> <span data-ttu-id="e96ea-121">Keresse meg a túl**figyelő** > **hardver állapotának**.</span><span class="sxs-lookup"><span data-stu-id="e96ea-121">Navigate too**Monitor** > **Hardware health**.</span></span> <span data-ttu-id="e96ea-122">A **megosztott összetevők**, nézze meg hello akkumulátor hello állapotának.</span><span class="sxs-lookup"><span data-stu-id="e96ea-122">Under **Shared Components**, look at hello status of hello battery.</span></span>
2. <span data-ttu-id="e96ea-123">Hello PCM mely hello az akkumulátor nem sikerült azonosítani.</span><span class="sxs-lookup"><span data-stu-id="e96ea-123">Identify hello PCM in which hello battery has failed.</span></span> <span data-ttu-id="e96ea-124">1. ábra mutatja hello hátsó hello StorSimple eszközt.</span><span class="sxs-lookup"><span data-stu-id="e96ea-124">Figure 1 shows hello back of hello StorSimple device.</span></span>
   
    ![Az eszköz elsődleges ház modulok Csatlakozópanel](./media/storsimple-battery-replacement/IC740994.png)
   
    <span data-ttu-id="e96ea-126">**1. ábra** hátsó PCM és a tartományvezérlő modulok megjelenítő elsődleges eszköz</span><span class="sxs-lookup"><span data-stu-id="e96ea-126">**Figure 1** Back of primary device showing PCM and controller modules</span></span>
   
   | <span data-ttu-id="e96ea-127">Címke</span><span class="sxs-lookup"><span data-stu-id="e96ea-127">Label</span></span> | <span data-ttu-id="e96ea-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="e96ea-128">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="e96ea-129">1</span><span class="sxs-lookup"><span data-stu-id="e96ea-129">1</span></span> |<span data-ttu-id="e96ea-130">PCM 0</span><span class="sxs-lookup"><span data-stu-id="e96ea-130">PCM 0</span></span> |
   | <span data-ttu-id="e96ea-131">2</span><span class="sxs-lookup"><span data-stu-id="e96ea-131">2</span></span> |<span data-ttu-id="e96ea-132">PCM 1</span><span class="sxs-lookup"><span data-stu-id="e96ea-132">PCM 1</span></span> |
   | <span data-ttu-id="e96ea-133">3</span><span class="sxs-lookup"><span data-stu-id="e96ea-133">3</span></span> |<span data-ttu-id="e96ea-134">A vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="e96ea-134">Controller 0</span></span> |
   | <span data-ttu-id="e96ea-135">4</span><span class="sxs-lookup"><span data-stu-id="e96ea-135">4</span></span> |<span data-ttu-id="e96ea-136">1. vezérlő</span><span class="sxs-lookup"><span data-stu-id="e96ea-136">Controller 1</span></span> |
   
    <span data-ttu-id="e96ea-137">Hello 2. ábra a 3-as számú eredményobjektumokról mutató figyelési hello PCM 0 megfelelő túl a kereslet az olyan**akkumulátor tartalék** kell kell kapcsolni.</span><span class="sxs-lookup"><span data-stu-id="e96ea-137">As shown by number 3 in hello Figure 2, hello monitoring indicator LED on PCM 0 that corresponds too**Battery Fault** should be lit.</span></span>
   
    ![Az eszköz PCM figyelési kijelző LED Csatlakozópanel](./media/storsimple-battery-replacement/IC740992.png)
   
    <span data-ttu-id="e96ea-139">**2. ábra** vissza a PCM ábrázoló hello kijelző LED figyelése</span><span class="sxs-lookup"><span data-stu-id="e96ea-139">**Figure 2** Back of PCM showing hello monitoring indicator LEDs</span></span>
   
   | <span data-ttu-id="e96ea-140">Címke</span><span class="sxs-lookup"><span data-stu-id="e96ea-140">Label</span></span> | <span data-ttu-id="e96ea-141">Leírás</span><span class="sxs-lookup"><span data-stu-id="e96ea-141">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="e96ea-142">1</span><span class="sxs-lookup"><span data-stu-id="e96ea-142">1</span></span> |<span data-ttu-id="e96ea-143">AC áramszünet esetén</span><span class="sxs-lookup"><span data-stu-id="e96ea-143">AC power failure</span></span> |
   | <span data-ttu-id="e96ea-144">2</span><span class="sxs-lookup"><span data-stu-id="e96ea-144">2</span></span> |<span data-ttu-id="e96ea-145">Hiba ventilátor</span><span class="sxs-lookup"><span data-stu-id="e96ea-145">Fan failure</span></span> |
   | <span data-ttu-id="e96ea-146">3</span><span class="sxs-lookup"><span data-stu-id="e96ea-146">3</span></span> |<span data-ttu-id="e96ea-147">Akkumulátor hiba</span><span class="sxs-lookup"><span data-stu-id="e96ea-147">Battery fault</span></span> |
   | <span data-ttu-id="e96ea-148">4</span><span class="sxs-lookup"><span data-stu-id="e96ea-148">4</span></span> |<span data-ttu-id="e96ea-149">PCM OK</span><span class="sxs-lookup"><span data-stu-id="e96ea-149">PCM OK</span></span> |
   | <span data-ttu-id="e96ea-150">5</span><span class="sxs-lookup"><span data-stu-id="e96ea-150">5</span></span> |<span data-ttu-id="e96ea-151">DC áramszünet esetén</span><span class="sxs-lookup"><span data-stu-id="e96ea-151">DC power failure</span></span> |
   | <span data-ttu-id="e96ea-152">6</span><span class="sxs-lookup"><span data-stu-id="e96ea-152">6</span></span> |<span data-ttu-id="e96ea-153">Kifogástalan akkumulátor</span><span class="sxs-lookup"><span data-stu-id="e96ea-153">Battery healthy</span></span> |
3. <span data-ttu-id="e96ea-154">tooremove hello PCM sikertelen töltöttségű telepre vonatkozó, a kövesse hello [távolítsa el a PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span><span class="sxs-lookup"><span data-stu-id="e96ea-154">tooremove hello PCM with a failed battery, follow hello steps in [Remove a PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span></span>
4. <span data-ttu-id="e96ea-155">Hello PCM eltávolítva, a növekedési és a Elforgatás hello töltöttségű telepre vonatkozó modul felfelé kezelni, a következő ábra hello jelöltük, és húzza azt ki tooremove hello töltöttségű telepre vonatkozó.</span><span class="sxs-lookup"><span data-stu-id="e96ea-155">With hello PCM removed, lift and rotate hello battery module handle upward as indicated in hello following figure, and pull it up tooremove hello battery.</span></span>
   
    ![PCM akkumulátor eltávolítása](./media/storsimple-battery-replacement/IC741019.png)
   
    <span data-ttu-id="e96ea-157">**3. ábra** hello PCM hello akkumulátor eltávolítása</span><span class="sxs-lookup"><span data-stu-id="e96ea-157">**Figure 3** Removing hello battery from hello PCM</span></span>
5. <span data-ttu-id="e96ea-158">Tegyen hello modul hello terepen cserélhető Cisco egységet-csomagban.</span><span class="sxs-lookup"><span data-stu-id="e96ea-158">Place hello module in hello field-replaceable unit packaging.</span></span>
6. <span data-ttu-id="e96ea-159">Térjen vissza a hello hibás egység tooMicrosoft megfelelő szervizelési és kezelése.</span><span class="sxs-lookup"><span data-stu-id="e96ea-159">Return hello defective unit tooMicrosoft for proper servicing and handling.</span></span>

## <a name="install-a-new-backup-battery-module"></a><span data-ttu-id="e96ea-160">Egy új biztonsági mentési akkumulátor modul telepítése</span><span class="sxs-lookup"><span data-stu-id="e96ea-160">Install a new backup battery module</span></span>
<span data-ttu-id="e96ea-161">Hajtsa végre a következő lépéseket akkumulátor tooinstall hello helyettesítő modul hello PCM hello elsődleges szolgáltatással a StorSimple eszköz hello.</span><span class="sxs-lookup"><span data-stu-id="e96ea-161">Perform hello following steps tooinstall hello replacement battery module in hello PCM in hello primary enclosure of your StorSimple device.</span></span>

#### <a name="tooinstall-hello-battery-module"></a><span data-ttu-id="e96ea-162">tooinstall hello akkumulátor modul</span><span class="sxs-lookup"><span data-stu-id="e96ea-162">tooinstall hello battery module</span></span>
1. <span data-ttu-id="e96ea-163">Hely hello biztonsági mentési akkumulátor modul a hello PCM hello megfelelő helyzetben.</span><span class="sxs-lookup"><span data-stu-id="e96ea-163">Place hello backup battery module in hello proper orientation in hello PCM.</span></span>
2. <span data-ttu-id="e96ea-164">Lefelé hello akkumulátor modul nyomja le az összes hello módon tooseat hello összekötő kezelni.</span><span class="sxs-lookup"><span data-stu-id="e96ea-164">Press down hello battery module handle all hello way tooseat hello connector.</span></span>
3. <span data-ttu-id="e96ea-165">Cserélje le PCM hello hello elsődleges szolgáltatással hello útmutatását követve [energia- és hűtési modul lecseréli a StorSimple eszköz](storsimple-power-cooling-module-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="e96ea-165">Replace hello PCM in hello primary enclosure by following hello guidelines in [Replace a Power and Cooling Module on your StorSimple device](storsimple-power-cooling-module-replacement.md).</span></span>
4. <span data-ttu-id="e96ea-166">Hello helyettesítő befejezése után nyissa meg tooyour eszközt, és keresse meg a túl**figyelő** > **hardver állapotának** a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="e96ea-166">After hello replacement is complete, go tooyour device and then go too**Monitor** > **Hardware health** in hello Azure portal.</span></span> <span data-ttu-id="e96ea-167">Hello hello akkumulátor toomake meg arról, hogy sikeres volt-e hello telepítési állapotának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="e96ea-167">Verify hello status of hello battery toomake sure that hello installation was successful.</span></span> <span data-ttu-id="e96ea-168">Zöld állapot azt jelzi, hogy hello akkumulátorról működik megfelelően.</span><span class="sxs-lookup"><span data-stu-id="e96ea-168">A green status indicates that hello battery is healthy.</span></span>

## <a name="maintain-hello-backup-battery-module"></a><span data-ttu-id="e96ea-169">Hello biztonsági mentési akkumulátor modul karbantartása</span><span class="sxs-lookup"><span data-stu-id="e96ea-169">Maintain hello backup battery module</span></span>
<span data-ttu-id="e96ea-170">A StorSimple eszköz hello biztonsági mentési akkumulátor modul biztosít power toohello vezérlő power adatvesztési esemény során.</span><span class="sxs-lookup"><span data-stu-id="e96ea-170">In your StorSimple device, hello backup battery module provides power toohello controller during a power loss event.</span></span> <span data-ttu-id="e96ea-171">Ez lehetővé teszi, hogy hello StorSimple eszköz toosave kritikus fontosságú adatok előzetes tooshutting le szabályozott módon.</span><span class="sxs-lookup"><span data-stu-id="e96ea-171">It allows hello StorSimple device toosave critical data prior tooshutting down in a controlled manner.</span></span> <span data-ttu-id="e96ea-172">A két akkumulátor feltöltött hello PCMs hello rendszer kezelni tud a két egymást követő adatvesztési eseményeket.</span><span class="sxs-lookup"><span data-stu-id="e96ea-172">With two fully charged batteries in hello PCMs, hello system can handle two consecutive loss events.</span></span>

<span data-ttu-id="e96ea-173">Hello Azure-portálon, a hello **hardver állapotának** alatt hello **figyelő** panel jelzi, hogy a hello akkumulátor hibásan működik, vagy hello end életciklusa közeledik.</span><span class="sxs-lookup"><span data-stu-id="e96ea-173">In hello Azure portal, hello **Hardware health** under hello **Monitor** blade indicates whether hello battery is malfunctioning or hello end-of-life is approaching.</span></span> <span data-ttu-id="e96ea-174">hello akkumulátor állapotát jelzi **PCM 0 akkumulátor** vagy **PCM 1 akkumulátor** alatt **megosztott összetevők**.</span><span class="sxs-lookup"><span data-stu-id="e96ea-174">hello battery status is indicated by **Battery in PCM 0** or **Battery in PCM 1** under **Shared Components**.</span></span> <span data-ttu-id="e96ea-175">Ezen a panelen megjelenik egy **csökkentett teljesítményű** állapotban a(z) élettartam végi közeledik, és **sikertelen** end életciklusa elérte a.</span><span class="sxs-lookup"><span data-stu-id="e96ea-175">This blade will show a **DEGRADED** state for end-of-life approaching, and **FAILED** for end-of-life reached.</span></span>

> [!NOTE]
> <span data-ttu-id="e96ea-176">hello akkumulátor is tud jelentéseket **sikertelen** ha egyszerűen kell toobe számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="e96ea-176">hello battery can report **FAILED** when it simply needs toobe charged.</span></span>


<span data-ttu-id="e96ea-177">Ha hello **csökkentett teljesítményű** állapot jelenik meg, azt javasoljuk, hogy a következő lépések hello:</span><span class="sxs-lookup"><span data-stu-id="e96ea-177">If hello **DEGRADED** state appears, we recommend hello following course of action:</span></span>

* <span data-ttu-id="e96ea-178">Előfordulhat, hogy hello rendszer észlelt egy friss az áramellátás megszakadása miatt, vagy előfordulhat, hogy a hello akkumulátorok rendszeres karbantartás zajlik.</span><span class="sxs-lookup"><span data-stu-id="e96ea-178">hello system may have experienced a recent power loss or hello batteries may be undergoing periodic maintenance.</span></span> <span data-ttu-id="e96ea-179">Figyelje meg hello rendszer a folytatás előtt 12 óra.</span><span class="sxs-lookup"><span data-stu-id="e96ea-179">Observe hello system for 12 hours before proceeding.</span></span>
  
  * <span data-ttu-id="e96ea-180">Ha hello állapot továbbra is **csökkentett teljesítményű** után 12 óra folyamatos kapcsolat tooAC power hello, tartományvezérlői és PCMs fut, majd hello akkumulátor toobe cserélni kell-e.</span><span class="sxs-lookup"><span data-stu-id="e96ea-180">If hello state is still **DEGRADED** after 12 hours of continuous connection tooAC power with hello controllers and PCMs running, then hello battery needs toobe replaced.</span></span> <span data-ttu-id="e96ea-181">Adjon [forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md) helyettesítő biztonsági mentési akkumulátor modulként.</span><span class="sxs-lookup"><span data-stu-id="e96ea-181">Please [contact Microsoft Support](storsimple-8000-contact-microsoft-support.md) for a replacement backup battery module.</span></span>
  * <span data-ttu-id="e96ea-182">Hello állapotba kerül, 12 óra elteltével az OK gombra, ha hello akkumulátorról működik, és csak szükséges a karbantartási járnak.</span><span class="sxs-lookup"><span data-stu-id="e96ea-182">If hello state becomes OK after 12 hours, hello battery is operational, and it only needed a maintenance charge.</span></span>
* <span data-ttu-id="e96ea-183">Ha nem történt AC power és hello PCM társított megszűnését be van kapcsolva, és tooAC power csatlakoztatva, hello akkumulátor kell cserélni toobe.</span><span class="sxs-lookup"><span data-stu-id="e96ea-183">If there has not been an associated loss of AC power and hello PCM is turned on and connected tooAC power, hello battery needs toobe replaced.</span></span> <span data-ttu-id="e96ea-184">[Forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md) tooorder helyettesítő biztonsági mentési akkumulátor modul.</span><span class="sxs-lookup"><span data-stu-id="e96ea-184">[Contact Microsoft Support](storsimple-8000-contact-microsoft-support.md) tooorder a replacement backup battery module.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e96ea-185">Számolja fel hello toonational és regionális előírások szerint akkumulátor nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="e96ea-185">Dispose of hello failed battery according toonational and regional regulations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e96ea-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e96ea-186">Next steps</span></span>
<span data-ttu-id="e96ea-187">További információ [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="e96ea-187">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

