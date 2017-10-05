---
title: "Cserélje le a Microsoft Azure StorSimple 8000 series eszköz akkumulátorról |} Microsoft Docs"
description: "Távolítsa el, cserélje le, és a biztonsági mentési akkumulátor modul a StorSimple eszköz karbantartása ismerteti."
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
ms.openlocfilehash: 174a3163082594ea6a49b7f5a78857848f8f0566
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a><span data-ttu-id="ee8e5-103">Cserélje le a biztonsági mentési akkumulátor modul a StorSimple eszköz</span><span class="sxs-lookup"><span data-stu-id="ee8e5-103">Replace the backup battery module on your StorSimple device</span></span>

## <a name="overview"></a><span data-ttu-id="ee8e5-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ee8e5-104">Overview</span></span>
<span data-ttu-id="ee8e5-105">Az elsődleges ház Power- és hűtési modul (PCM) a Microsoft Azure StorSimple eszköz rendelkezik egy további akkumulátor csomagot.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-105">The primary enclosure Power and Cooling Module (PCM) on your Microsoft Azure StorSimple device has an additional battery pack.</span></span> <span data-ttu-id="ee8e5-106">Ez a csomag biztosítja a power, így a StorSimple eszköz adatok mentése az elsődleges házhoz AC áramkimaradás esetén.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-106">This pack provides power so that the StorSimple device can save data if there is loss of AC power to the primary enclosure.</span></span> <span data-ttu-id="ee8e5-107">Ez akkumulátor csomag nevezzük a *biztonsági mentési akkumulátor modul*.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-107">This battery pack is referred to as the *backup battery module*.</span></span> <span data-ttu-id="ee8e5-108">A biztonsági mentési akkumulátor modul csak a StorSimple eszköz (a EBOD ház nem tartalmaz a biztonsági mentési akkumulátor modul) az elsődleges ház létezik.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-108">The backup battery module exists only for the primary enclosure in your StorSimple device (the EBOD enclosure does not contain a backup battery module).</span></span>

<span data-ttu-id="ee8e5-109">Ez az oktatóanyag azt ismerteti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="ee8e5-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="ee8e5-110">Távolítsa el a biztonsági mentési akkumulátor modul</span><span class="sxs-lookup"><span data-stu-id="ee8e5-110">Remove the backup battery module</span></span>
* <span data-ttu-id="ee8e5-111">Egy új biztonsági mentési akkumulátor modul telepítése</span><span class="sxs-lookup"><span data-stu-id="ee8e5-111">Install a new backup battery module</span></span>
* <span data-ttu-id="ee8e5-112">A biztonsági mentési akkumulátor modul karbantartása</span><span class="sxs-lookup"><span data-stu-id="ee8e5-112">Maintain the backup battery module</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ee8e5-113">Mielőtt eltávolítása és cseréje egy biztonsági mentési akkumulátor modult, tekintse át a biztonsági információk a [StorSimple hardver összetevő cseréje bemutatása](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="ee8e5-113">Before removing and replacing a backup battery module, review the safety information in the [Introduction to StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>


## <a name="remove-the-backup-battery-module"></a><span data-ttu-id="ee8e5-114">Távolítsa el a biztonsági mentési akkumulátor modul</span><span class="sxs-lookup"><span data-stu-id="ee8e5-114">Remove the backup battery module</span></span>
<span data-ttu-id="ee8e5-115">A biztonsági mentési akkumulátor modul, a StorSimple eszközt a rendszer a terepen cserélhető Cisco egységet.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-115">The backup battery module for your StorSimple device is a field-replaceable unit.</span></span> <span data-ttu-id="ee8e5-116">Mielőtt a PCM van telepítve, az akkumulátor modul az eredeti csomagban kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-116">Before it is installed in the PCM, the battery module should be stored in its original packaging.</span></span> <span data-ttu-id="ee8e5-117">A következő lépésekkel távolítsa el a biztonsági mentési telep.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-117">Perform the following steps to remove the backup battery.</span></span>

#### <a name="to-remove-the-backup-battery-module"></a><span data-ttu-id="ee8e5-118">A biztonsági mentési akkumulátor modul eltávolítása</span><span class="sxs-lookup"><span data-stu-id="ee8e5-118">To remove the backup battery module</span></span>
1. <span data-ttu-id="ee8e5-119">Az Azure portálon nyissa meg a StorSimple Device Manager szolgáltatás paneljét.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-119">In the Azure portal, go to your StorSimple Device Manager service blade.</span></span> <span data-ttu-id="ee8e5-120">Ugrás a **eszközök** majd az eszközök listájában válassza ki az eszköz.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-120">Go to **Devices** and then select your device from the list of devices.</span></span> <span data-ttu-id="ee8e5-121">Navigáljon a **figyelő** > **hardver állapotának**.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-121">Navigate to **Monitor** > **Hardware health**.</span></span> <span data-ttu-id="ee8e5-122">A **megosztott összetevők**, nézze meg az akkumulátor állapotát.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-122">Under **Shared Components**, look at the status of the battery.</span></span>
2. <span data-ttu-id="ee8e5-123">Azonosítsa a PCM, amelyben az akkumulátor sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-123">Identify the PCM in which the battery has failed.</span></span> <span data-ttu-id="ee8e5-124">1. ábra mutatja a StorSimple eszköz hátulján olvasható.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-124">Figure 1 shows the back of the StorSimple device.</span></span>
   
    ![Az eszköz elsődleges ház modulok Csatlakozópanel](./media/storsimple-battery-replacement/IC740994.png)
   
    <span data-ttu-id="ee8e5-126">**1. ábra** hátsó PCM és a tartományvezérlő modulok megjelenítő elsődleges eszköz</span><span class="sxs-lookup"><span data-stu-id="ee8e5-126">**Figure 1** Back of primary device showing PCM and controller modules</span></span>
   
   | <span data-ttu-id="ee8e5-127">Címke</span><span class="sxs-lookup"><span data-stu-id="ee8e5-127">Label</span></span> | <span data-ttu-id="ee8e5-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="ee8e5-128">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="ee8e5-129">1</span><span class="sxs-lookup"><span data-stu-id="ee8e5-129">1</span></span> |<span data-ttu-id="ee8e5-130">PCM 0</span><span class="sxs-lookup"><span data-stu-id="ee8e5-130">PCM 0</span></span> |
   | <span data-ttu-id="ee8e5-131">2</span><span class="sxs-lookup"><span data-stu-id="ee8e5-131">2</span></span> |<span data-ttu-id="ee8e5-132">PCM 1</span><span class="sxs-lookup"><span data-stu-id="ee8e5-132">PCM 1</span></span> |
   | <span data-ttu-id="ee8e5-133">3</span><span class="sxs-lookup"><span data-stu-id="ee8e5-133">3</span></span> |<span data-ttu-id="ee8e5-134">A vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="ee8e5-134">Controller 0</span></span> |
   | <span data-ttu-id="ee8e5-135">4</span><span class="sxs-lookup"><span data-stu-id="ee8e5-135">4</span></span> |<span data-ttu-id="ee8e5-136">1. vezérlő</span><span class="sxs-lookup"><span data-stu-id="ee8e5-136">Controller 1</span></span> |
   
    <span data-ttu-id="ee8e5-137">3 szám a 2. ábrán látható, a figyelési kijelző PCM 0, amely megfelel a kereslet az olyan **akkumulátor tartalék** kell kell kapcsolni.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-137">As shown by number 3 in the Figure 2, the monitoring indicator LED on PCM 0 that corresponds to **Battery Fault** should be lit.</span></span>
   
    ![Az eszköz PCM figyelési kijelző LED Csatlakozópanel](./media/storsimple-battery-replacement/IC740992.png)
   
    <span data-ttu-id="ee8e5-139">**2. ábra** vissza a PCM a figyelési kijelző LED megjelenítése</span><span class="sxs-lookup"><span data-stu-id="ee8e5-139">**Figure 2** Back of PCM showing the monitoring indicator LEDs</span></span>
   
   | <span data-ttu-id="ee8e5-140">Címke</span><span class="sxs-lookup"><span data-stu-id="ee8e5-140">Label</span></span> | <span data-ttu-id="ee8e5-141">Leírás</span><span class="sxs-lookup"><span data-stu-id="ee8e5-141">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="ee8e5-142">1</span><span class="sxs-lookup"><span data-stu-id="ee8e5-142">1</span></span> |<span data-ttu-id="ee8e5-143">AC áramszünet esetén</span><span class="sxs-lookup"><span data-stu-id="ee8e5-143">AC power failure</span></span> |
   | <span data-ttu-id="ee8e5-144">2</span><span class="sxs-lookup"><span data-stu-id="ee8e5-144">2</span></span> |<span data-ttu-id="ee8e5-145">Hiba ventilátor</span><span class="sxs-lookup"><span data-stu-id="ee8e5-145">Fan failure</span></span> |
   | <span data-ttu-id="ee8e5-146">3</span><span class="sxs-lookup"><span data-stu-id="ee8e5-146">3</span></span> |<span data-ttu-id="ee8e5-147">Akkumulátor hiba</span><span class="sxs-lookup"><span data-stu-id="ee8e5-147">Battery fault</span></span> |
   | <span data-ttu-id="ee8e5-148">4</span><span class="sxs-lookup"><span data-stu-id="ee8e5-148">4</span></span> |<span data-ttu-id="ee8e5-149">PCM OK</span><span class="sxs-lookup"><span data-stu-id="ee8e5-149">PCM OK</span></span> |
   | <span data-ttu-id="ee8e5-150">5</span><span class="sxs-lookup"><span data-stu-id="ee8e5-150">5</span></span> |<span data-ttu-id="ee8e5-151">DC áramszünet esetén</span><span class="sxs-lookup"><span data-stu-id="ee8e5-151">DC power failure</span></span> |
   | <span data-ttu-id="ee8e5-152">6</span><span class="sxs-lookup"><span data-stu-id="ee8e5-152">6</span></span> |<span data-ttu-id="ee8e5-153">Kifogástalan akkumulátor</span><span class="sxs-lookup"><span data-stu-id="ee8e5-153">Battery healthy</span></span> |
3. <span data-ttu-id="ee8e5-154">A sikertelen akkumulátor PCM eltávolításához kövesse [távolítsa el a PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span><span class="sxs-lookup"><span data-stu-id="ee8e5-154">To remove the PCM with a failed battery, follow the steps in [Remove a PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span></span>
4. <span data-ttu-id="ee8e5-155">A eltávolított PCM növekedési felfelé elforgatása az akkumulátor modul leíróját a következő, az alábbi ábrán jelzett és lekéréses, akár az akkumulátor eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-155">With the PCM removed, lift and rotate the battery module handle upward as indicated in the following figure, and pull it up to remove the battery.</span></span>
   
    ![PCM akkumulátor eltávolítása](./media/storsimple-battery-replacement/IC741019.png)
   
    <span data-ttu-id="ee8e5-157">**3. ábra** az akkumulátor eltávolítása a PCM</span><span class="sxs-lookup"><span data-stu-id="ee8e5-157">**Figure 3** Removing the battery from the PCM</span></span>
5. <span data-ttu-id="ee8e5-158">A modul a terepen cserélhető Cisco egységet csomagolására helyezze el.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-158">Place the module in the field-replaceable unit packaging.</span></span>
6. <span data-ttu-id="ee8e5-159">Térjen vissza a hibás egység Microsoft megfelelő szervizelési és kezelése.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-159">Return the defective unit to Microsoft for proper servicing and handling.</span></span>

## <a name="install-a-new-backup-battery-module"></a><span data-ttu-id="ee8e5-160">Egy új biztonsági mentési akkumulátor modul telepítése</span><span class="sxs-lookup"><span data-stu-id="ee8e5-160">Install a new backup battery module</span></span>
<span data-ttu-id="ee8e5-161">A következő lépésekkel telepíti a helyettesítő akkumulátor modulját az elsődleges szolgáltatással a StorSimple eszköz PCM.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-161">Perform the following steps to install the replacement battery module in the PCM in the primary enclosure of your StorSimple device.</span></span>

#### <a name="to-install-the-battery-module"></a><span data-ttu-id="ee8e5-162">A akkumulátor modul telepítése</span><span class="sxs-lookup"><span data-stu-id="ee8e5-162">To install the battery module</span></span>
1. <span data-ttu-id="ee8e5-163">A biztonsági mentési akkumulátor modul elhelyezni a PCM a megfelelő helyzetben.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-163">Place the backup battery module in the proper orientation in the PCM.</span></span>
2. <span data-ttu-id="ee8e5-164">Nyomja le az akkumulátor modul leíróját a következő egészen az, hogy az összekötő ülés.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-164">Press down the battery module handle all the way to seat the connector.</span></span>
3. <span data-ttu-id="ee8e5-165">Cserélje le az elsődleges szolgáltatással PCM a következő [energia- és hűtési modul lecseréli a StorSimple eszköz](storsimple-power-cooling-module-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="ee8e5-165">Replace the PCM in the primary enclosure by following the guidelines in [Replace a Power and Cooling Module on your StorSimple device](storsimple-power-cooling-module-replacement.md).</span></span>
4. <span data-ttu-id="ee8e5-166">A Csere befejezése után nyissa meg az eszközre, és folytassa a **figyelő** > **hardver állapotának** az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-166">After the replacement is complete, go to your device and then go to **Monitor** > **Hardware health** in the Azure portal.</span></span> <span data-ttu-id="ee8e5-167">Győződjön meg arról, hogy a telepítés sikeres volt-e az akkumulátor állapotának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-167">Verify the status of the battery to make sure that the installation was successful.</span></span> <span data-ttu-id="ee8e5-168">Zöld állapot azt jelzi, hogy az akkumulátor állapota kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-168">A green status indicates that the battery is healthy.</span></span>

## <a name="maintain-the-backup-battery-module"></a><span data-ttu-id="ee8e5-169">A biztonsági mentési akkumulátor modul karbantartása</span><span class="sxs-lookup"><span data-stu-id="ee8e5-169">Maintain the backup battery module</span></span>
<span data-ttu-id="ee8e5-170">A StorSimple eszközt a a biztonsági mentési akkumulátor a modul adja meg a tartományvezérlő teljesítményét power adatvesztési esemény során.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-170">In your StorSimple device, the backup battery module provides power to the controller during a power loss event.</span></span> <span data-ttu-id="ee8e5-171">Lehetővé teszi a StorSimple eszköz szabályozott módon leállítása előtt kritikus adatok mentése.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-171">It allows the StorSimple device to save critical data prior to shutting down in a controlled manner.</span></span> <span data-ttu-id="ee8e5-172">A két akkumulátor feltöltött a PCMs a rendszer két egymást követő adatvesztési eseményeket is kezelni.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-172">With two fully charged batteries in the PCMs, the system can handle two consecutive loss events.</span></span>

<span data-ttu-id="ee8e5-173">Az Azure portálon a **hardver állapotának** alatt a **figyelő** panel jelzi, hogy a az akkumulátor hibásan működik, vagy a záró életciklusa közeledik.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-173">In the Azure portal, the **Hardware health** under the **Monitor** blade indicates whether the battery is malfunctioning or the end-of-life is approaching.</span></span> <span data-ttu-id="ee8e5-174">Akkumulátor állapotát jelzi **PCM 0 akkumulátor** vagy **PCM 1 akkumulátor** alatt **megosztott összetevők**.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-174">The battery status is indicated by **Battery in PCM 0** or **Battery in PCM 1** under **Shared Components**.</span></span> <span data-ttu-id="ee8e5-175">Ezen a panelen megjelenik egy **csökkentett teljesítményű** állapotban a(z) élettartam végi közeledik, és **sikertelen** end életciklusa elérte a.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-175">This blade will show a **DEGRADED** state for end-of-life approaching, and **FAILED** for end-of-life reached.</span></span>

> [!NOTE]
> <span data-ttu-id="ee8e5-176">Az akkumulátor is tud jelentéseket **sikertelen** ha egyszerűen kell számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-176">The battery can report **FAILED** when it simply needs to be charged.</span></span>


<span data-ttu-id="ee8e5-177">Ha a **csökkentett teljesítményű** állapot jelenik meg, az alábbi intézkedéseket javasoljuk:</span><span class="sxs-lookup"><span data-stu-id="ee8e5-177">If the **DEGRADED** state appears, we recommend the following course of action:</span></span>

* <span data-ttu-id="ee8e5-178">Előfordulhat, hogy a rendszer észlelt egy friss az áramellátás megszakadása miatt, vagy előfordulhat, hogy a az akkumulátorok rendszeres karbantartás zajlik.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-178">The system may have experienced a recent power loss or the batteries may be undergoing periodic maintenance.</span></span> <span data-ttu-id="ee8e5-179">Vegye figyelembe a rendszer a folytatás előtt 12 óra.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-179">Observe the system for 12 hours before proceeding.</span></span>
  
  * <span data-ttu-id="ee8e5-180">Ha az állapot továbbra is **csökkentett teljesítményű** után a létrehozni kívánt AC folyamatos kapcsolat 12 óra kapcsolja a tartományvezérlők és PCMs fut, majd az akkumulátor kell lecserélni.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-180">If the state is still **DEGRADED** after 12 hours of continuous connection to AC power with the controllers and PCMs running, then the battery needs to be replaced.</span></span> <span data-ttu-id="ee8e5-181">Adjon [forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md) helyettesítő biztonsági mentési akkumulátor modulként.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-181">Please [contact Microsoft Support](storsimple-8000-contact-microsoft-support.md) for a replacement backup battery module.</span></span>
  * <span data-ttu-id="ee8e5-182">A állapotba kerül, 12 óra elteltével az OK gombra, ha az akkumulátor működik, és azt csak akkor szükséges, a karbantartási kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-182">If the state becomes OK after 12 hours, the battery is operational, and it only needed a maintenance charge.</span></span>
* <span data-ttu-id="ee8e5-183">Ha nem lett az AC Power társított veszteséget és a PCM-e kapcsolva és csatlakoztatva AC power, az akkumulátor kell lecserélni.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-183">If there has not been an associated loss of AC power and the PCM is turned on and connected to AC power, the battery needs to be replaced.</span></span> <span data-ttu-id="ee8e5-184">[Forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md) helyettesítő biztonsági mentési akkumulátor modul rendezéséhez.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-184">[Contact Microsoft Support](storsimple-8000-contact-microsoft-support.md) to order a replacement backup battery module.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ee8e5-185">Számolja fel a sikertelen akkumulátor nemzeti és regionális szabályok szerint.</span><span class="sxs-lookup"><span data-stu-id="ee8e5-185">Dispose of the failed battery according to national and regional regulations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee8e5-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ee8e5-186">Next steps</span></span>
<span data-ttu-id="ee8e5-187">További információ [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="ee8e5-187">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

