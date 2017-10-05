---
title: "Cserélje le a Microsoft Azure StorSimple eszközön akkumulátor |} Microsoft Docs"
description: "Távolítsa el, cserélje le, és a biztonsági mentési akkumulátor modul a StorSimple eszköz karbantartása ismerteti."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3c8a6654-4826-4883-aad8-75f332347c53
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: f8b89b3f6851ec9ee0570f551b5407419fdba2d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a><span data-ttu-id="13fd8-103">Cserélje le a biztonsági mentési akkumulátor modul a StorSimple eszköz</span><span class="sxs-lookup"><span data-stu-id="13fd8-103">Replace the backup battery module on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="13fd8-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="13fd8-104">Overview</span></span>
<span data-ttu-id="13fd8-105">Az elsődleges ház Power- és hűtési modul (PCM) a Microsoft Azure StorSimple eszköz rendelkezik egy további akkumulátor csomagot.</span><span class="sxs-lookup"><span data-stu-id="13fd8-105">The primary enclosure Power and Cooling Module (PCM) on your Microsoft Azure StorSimple device has an additional battery pack.</span></span> <span data-ttu-id="13fd8-106">Ez a csomag biztosítja a power, így a StorSimple eszköz adatok mentése az elsődleges házhoz AC áramkimaradás esetén.</span><span class="sxs-lookup"><span data-stu-id="13fd8-106">This pack provides power so that the StorSimple device can save data if there is loss of AC power to the primary enclosure.</span></span> <span data-ttu-id="13fd8-107">Ez akkumulátor csomag nevezzük a *biztonsági mentési akkumulátor modul*.</span><span class="sxs-lookup"><span data-stu-id="13fd8-107">This battery pack is referred to as the *backup battery module*.</span></span> <span data-ttu-id="13fd8-108">A biztonsági mentési akkumulátor modul csak a StorSimple eszköz (a EBOD ház nem tartalmaz a biztonsági mentési akkumulátor modul) az elsődleges ház létezik.</span><span class="sxs-lookup"><span data-stu-id="13fd8-108">The backup battery module exists only for the primary enclosure in your StorSimple device (the EBOD enclosure does not contain a backup battery module).</span></span> 

<span data-ttu-id="13fd8-109">Ez az oktatóanyag azt ismerteti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="13fd8-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="13fd8-110">Távolítsa el a biztonsági mentési akkumulátor modul</span><span class="sxs-lookup"><span data-stu-id="13fd8-110">Remove the backup battery module</span></span> 
* <span data-ttu-id="13fd8-111">Egy új biztonsági mentési akkumulátor modul telepítése</span><span class="sxs-lookup"><span data-stu-id="13fd8-111">Install a new backup battery module</span></span>
* <span data-ttu-id="13fd8-112">A biztonsági mentési akkumulátor modul karbantartása</span><span class="sxs-lookup"><span data-stu-id="13fd8-112">Maintain the backup battery module</span></span>

> [!IMPORTANT]
> <span data-ttu-id="13fd8-113">Mielőtt eltávolítása és cseréje egy biztonsági mentési akkumulátor modult, tekintse át a biztonsági információk a [StorSimple hardver összetevő cseréje bemutatása](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="13fd8-113">Before removing and replacing a backup battery module, review the safety information in the [Introduction to StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>
> 
> 

## <a name="remove-the-backup-battery-module"></a><span data-ttu-id="13fd8-114">Távolítsa el a biztonsági mentési akkumulátor modul</span><span class="sxs-lookup"><span data-stu-id="13fd8-114">Remove the backup battery module</span></span>
<span data-ttu-id="13fd8-115">A biztonsági mentési akkumulátor modul, a StorSimple eszközt a rendszer a terepen cserélhető Cisco egységet.</span><span class="sxs-lookup"><span data-stu-id="13fd8-115">The backup battery module for your StorSimple device is a field-replaceable unit.</span></span> <span data-ttu-id="13fd8-116">Mielőtt a PCM van telepítve, az akkumulátor modul az eredeti csomagban kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="13fd8-116">Before it is installed in the PCM, the battery module should be stored in its original packaging.</span></span> <span data-ttu-id="13fd8-117">A következő lépésekkel távolítsa el a biztonsági mentési telep.</span><span class="sxs-lookup"><span data-stu-id="13fd8-117">Perform the following steps to remove the backup battery.</span></span>

#### <a name="to-remove-the-backup-battery-module"></a><span data-ttu-id="13fd8-118">A biztonsági mentési akkumulátor modul eltávolítása</span><span class="sxs-lookup"><span data-stu-id="13fd8-118">To remove the backup battery module</span></span>
1. <span data-ttu-id="13fd8-119">A klasszikus Azure portálon lépjen **eszközök** > **karbantartási** > **hardverállapot**.</span><span class="sxs-lookup"><span data-stu-id="13fd8-119">In the Azure classic portal, go to **Devices** > **Maintenance** > **Hardware Status**.</span></span> <span data-ttu-id="13fd8-120">A **megosztott összetevők**, nézze meg az akkumulátor állapotát.</span><span class="sxs-lookup"><span data-stu-id="13fd8-120">Under **Shared Components**, look at the status of the battery.</span></span>
2. <span data-ttu-id="13fd8-121">Azonosítsa a PCM, amelyben az akkumulátor sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="13fd8-121">Identify the PCM in which the battery has failed.</span></span> <span data-ttu-id="13fd8-122">1. ábra mutatja a StorSimple eszköz hátulján olvasható.</span><span class="sxs-lookup"><span data-stu-id="13fd8-122">Figure 1 shows the back of the StorSimple device.</span></span>
   
    ![Az eszköz elsődleges ház modulok Csatlakozópanel](./media/storsimple-battery-replacement/IC740994.png)
   
    <span data-ttu-id="13fd8-124">**1. ábra** hátsó PCM és a tartományvezérlő modulok megjelenítő elsődleges eszköz</span><span class="sxs-lookup"><span data-stu-id="13fd8-124">**Figure 1** Back of primary device showing PCM and controller modules</span></span>
   
   | <span data-ttu-id="13fd8-125">Címke</span><span class="sxs-lookup"><span data-stu-id="13fd8-125">Label</span></span> | <span data-ttu-id="13fd8-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="13fd8-126">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="13fd8-127">1</span><span class="sxs-lookup"><span data-stu-id="13fd8-127">1</span></span> |<span data-ttu-id="13fd8-128">PCM 0</span><span class="sxs-lookup"><span data-stu-id="13fd8-128">PCM 0</span></span> |
   | <span data-ttu-id="13fd8-129">2</span><span class="sxs-lookup"><span data-stu-id="13fd8-129">2</span></span> |<span data-ttu-id="13fd8-130">PCM 1</span><span class="sxs-lookup"><span data-stu-id="13fd8-130">PCM 1</span></span> |
   | <span data-ttu-id="13fd8-131">3</span><span class="sxs-lookup"><span data-stu-id="13fd8-131">3</span></span> |<span data-ttu-id="13fd8-132">A vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="13fd8-132">Controller 0</span></span> |
   | <span data-ttu-id="13fd8-133">4</span><span class="sxs-lookup"><span data-stu-id="13fd8-133">4</span></span> |<span data-ttu-id="13fd8-134">1. vezérlő</span><span class="sxs-lookup"><span data-stu-id="13fd8-134">Controller 1</span></span> |
   
    <span data-ttu-id="13fd8-135">3 szám a 2. ábrán látható, a figyelési kijelző PCM 0, amely megfelel a kereslet az olyan **akkumulátor tartalék** kell kell kapcsolni.</span><span class="sxs-lookup"><span data-stu-id="13fd8-135">As shown by number 3 in the Figure 2, the monitoring indicator LED on PCM 0 that corresponds to **Battery Fault** should be lit.</span></span>
   
    ![Az eszköz PCM figyelési kijelző LED Csatlakozópanel](./media/storsimple-battery-replacement/IC740992.png)
   
    <span data-ttu-id="13fd8-137">**2. ábra** vissza a PCM a figyelési kijelző LED megjelenítése</span><span class="sxs-lookup"><span data-stu-id="13fd8-137">**Figure 2** Back of PCM showing the monitoring indicator LEDs</span></span>
   
   | <span data-ttu-id="13fd8-138">Címke</span><span class="sxs-lookup"><span data-stu-id="13fd8-138">Label</span></span> | <span data-ttu-id="13fd8-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="13fd8-139">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="13fd8-140">1</span><span class="sxs-lookup"><span data-stu-id="13fd8-140">1</span></span> |<span data-ttu-id="13fd8-141">AC áramszünet esetén</span><span class="sxs-lookup"><span data-stu-id="13fd8-141">AC power failure</span></span> |
   | <span data-ttu-id="13fd8-142">2</span><span class="sxs-lookup"><span data-stu-id="13fd8-142">2</span></span> |<span data-ttu-id="13fd8-143">Hiba ventilátor</span><span class="sxs-lookup"><span data-stu-id="13fd8-143">Fan failure</span></span> |
   | <span data-ttu-id="13fd8-144">3</span><span class="sxs-lookup"><span data-stu-id="13fd8-144">3</span></span> |<span data-ttu-id="13fd8-145">Akkumulátor hiba</span><span class="sxs-lookup"><span data-stu-id="13fd8-145">Battery fault</span></span> |
   | <span data-ttu-id="13fd8-146">4</span><span class="sxs-lookup"><span data-stu-id="13fd8-146">4</span></span> |<span data-ttu-id="13fd8-147">PCM OK</span><span class="sxs-lookup"><span data-stu-id="13fd8-147">PCM OK</span></span> |
   | <span data-ttu-id="13fd8-148">5</span><span class="sxs-lookup"><span data-stu-id="13fd8-148">5</span></span> |<span data-ttu-id="13fd8-149">DC áramszünet esetén</span><span class="sxs-lookup"><span data-stu-id="13fd8-149">DC power failure</span></span> |
   | <span data-ttu-id="13fd8-150">6</span><span class="sxs-lookup"><span data-stu-id="13fd8-150">6</span></span> |<span data-ttu-id="13fd8-151">Kifogástalan akkumulátor</span><span class="sxs-lookup"><span data-stu-id="13fd8-151">Battery healthy</span></span> |
3. <span data-ttu-id="13fd8-152">A sikertelen akkumulátor PCM eltávolításához kövesse [távolítsa el a PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span><span class="sxs-lookup"><span data-stu-id="13fd8-152">To remove the PCM with a failed battery, follow the steps in [Remove a PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).</span></span>
4. <span data-ttu-id="13fd8-153">A eltávolított PCM növekedési felfelé elforgatása az akkumulátor modul leíróját a következő, az alábbi ábrán jelzett és lekéréses, akár az akkumulátor eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="13fd8-153">With the PCM removed, lift and rotate the battery module handle upward as indicated in the following figure, and pull it up to remove the battery.</span></span>
   
    ![PCM akkumulátor eltávolítása](./media/storsimple-battery-replacement/IC741019.png)
   
    <span data-ttu-id="13fd8-155">**3. ábra** az akkumulátor eltávolítása a PCM</span><span class="sxs-lookup"><span data-stu-id="13fd8-155">**Figure 3** Removing the battery from the PCM</span></span>
5. <span data-ttu-id="13fd8-156">A modul a terepen cserélhető Cisco egységet csomagolására helyezze el.</span><span class="sxs-lookup"><span data-stu-id="13fd8-156">Place the module in the field-replaceable unit packaging.</span></span>
6. <span data-ttu-id="13fd8-157">Térjen vissza a hibás egység Microsoft megfelelő szervizelési és kezelése.</span><span class="sxs-lookup"><span data-stu-id="13fd8-157">Return the defective unit to Microsoft for proper servicing and handling.</span></span>

## <a name="install-a-new-backup-battery-module"></a><span data-ttu-id="13fd8-158">Egy új biztonsági mentési akkumulátor modul telepítése</span><span class="sxs-lookup"><span data-stu-id="13fd8-158">Install a new backup battery module</span></span>
<span data-ttu-id="13fd8-159">A következő lépésekkel telepíti a helyettesítő akkumulátor modulját az elsődleges szolgáltatással a StorSimple eszköz PCM.</span><span class="sxs-lookup"><span data-stu-id="13fd8-159">Perform the following steps to install the replacement battery module in the PCM in the primary enclosure of your StorSimple device.</span></span>

#### <a name="to-install-the-battery-module"></a><span data-ttu-id="13fd8-160">A akkumulátor modul telepítése</span><span class="sxs-lookup"><span data-stu-id="13fd8-160">To install the battery module</span></span>
1. <span data-ttu-id="13fd8-161">A biztonsági mentési akkumulátor modul elhelyezni a PCM a megfelelő helyzetben.</span><span class="sxs-lookup"><span data-stu-id="13fd8-161">Place the backup battery module in the proper orientation in the PCM.</span></span>
2. <span data-ttu-id="13fd8-162">Nyomja le az akkumulátor modul leíróját a következő egészen az, hogy az összekötő ülés.</span><span class="sxs-lookup"><span data-stu-id="13fd8-162">Press down the battery module handle all the way to seat the connector.</span></span>
3. <span data-ttu-id="13fd8-163">Cserélje le az elsődleges szolgáltatással PCM a következő [energia- és hűtési modul lecseréli a StorSimple eszköz](storsimple-power-cooling-module-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="13fd8-163">Replace the PCM in the primary enclosure by following the guidelines in [Replace a Power and Cooling Module on your StorSimple device](storsimple-power-cooling-module-replacement.md).</span></span>
4. <span data-ttu-id="13fd8-164">A Csere befejezése után lépjen **eszközök** > **karbantartási** > **hardverállapot** a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="13fd8-164">After the replacement is complete, go to **Devices** > **Maintenance** > **Hardware Status** in the Azure classic portal.</span></span> <span data-ttu-id="13fd8-165">Győződjön meg arról, hogy a telepítés sikeres volt-e az akkumulátor állapotának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="13fd8-165">Verify the status of the battery to make sure that the installation was successful.</span></span> <span data-ttu-id="13fd8-166">Zöld állapot azt jelzi, hogy az akkumulátor állapota kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="13fd8-166">A green status indicates that the battery is healthy.</span></span>

## <a name="maintain-the-backup-battery-module"></a><span data-ttu-id="13fd8-167">A biztonsági mentési akkumulátor modul karbantartása</span><span class="sxs-lookup"><span data-stu-id="13fd8-167">Maintain the backup battery module</span></span>
<span data-ttu-id="13fd8-168">A StorSimple eszközt a a biztonsági mentési akkumulátor a modul adja meg a tartományvezérlő teljesítményét power adatvesztési esemény során.</span><span class="sxs-lookup"><span data-stu-id="13fd8-168">In your StorSimple device, the backup battery module provides power to the controller during a power loss event.</span></span> <span data-ttu-id="13fd8-169">Lehetővé teszi a StorSimple eszköz szabályozott módon leállítása előtt kritikus adatok mentése.</span><span class="sxs-lookup"><span data-stu-id="13fd8-169">It allows the StorSimple device to save critical data prior to shutting down in a controlled manner.</span></span> <span data-ttu-id="13fd8-170">A két akkumulátor feltöltött a PCMs a rendszer két egymást követő adatvesztési eseményeket is kezelni.</span><span class="sxs-lookup"><span data-stu-id="13fd8-170">With two fully charged batteries in the PCMs, the system can handle two consecutive loss events.</span></span>

<span data-ttu-id="13fd8-171">A klasszikus Azure portálon a **hardverállapot** a a **karbantartási** lap jelzi, hogy a az akkumulátor hibásan működik, vagy a záró életciklusa közeledik.</span><span class="sxs-lookup"><span data-stu-id="13fd8-171">In the Azure classic portal, the **Hardware Status** on the **Maintenance** page indicates whether the battery is malfunctioning or the end-of-life is approaching.</span></span> <span data-ttu-id="13fd8-172">Akkumulátor állapotát jelzi **PCM 0 akkumulátor** vagy **PCM 1 akkumulátor** alatt **megosztott összetevők**.</span><span class="sxs-lookup"><span data-stu-id="13fd8-172">The battery status is indicated by **Battery in PCM 0** or **Battery in PCM 1** under **Shared Components**.</span></span> <span data-ttu-id="13fd8-173">Ezen a lapon megjelenik egy **csökkentett teljesítményű** állapotban a(z) élettartam végi közeledik, és **sikertelen** end életciklusa elérte a.</span><span class="sxs-lookup"><span data-stu-id="13fd8-173">This page will show a **DEGRADED** state for end-of-life approaching, and **FAILED** for end-of-life reached.</span></span> 

> [!NOTE]
> <span data-ttu-id="13fd8-174">Az akkumulátor is tud jelentéseket **sikertelen** ha egyszerűen kell számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="13fd8-174">The battery can report **FAILED** when it simply needs to be charged.</span></span>
> 
> 

<span data-ttu-id="13fd8-175">Ha a **csökkentett teljesítményű** állapot jelenik meg, az alábbi intézkedéseket javasoljuk:</span><span class="sxs-lookup"><span data-stu-id="13fd8-175">If the **DEGRADED** state appears, we recommend the following course of action:</span></span>

* <span data-ttu-id="13fd8-176">Előfordulhat, hogy a rendszer észlelt egy friss az áramellátás megszakadása miatt, vagy előfordulhat, hogy a az akkumulátorok rendszeres karbantartás zajlik.</span><span class="sxs-lookup"><span data-stu-id="13fd8-176">The system may have experienced a recent power loss or the batteries may be undergoing periodic maintenance.</span></span> <span data-ttu-id="13fd8-177">Vegye figyelembe a rendszer a folytatás előtt 12 óra.</span><span class="sxs-lookup"><span data-stu-id="13fd8-177">Observe the system for 12 hours before proceeding.</span></span>
  
  * <span data-ttu-id="13fd8-178">Ha az állapot továbbra is **csökkentett teljesítményű** után a létrehozni kívánt AC folyamatos kapcsolat 12 óra kapcsolja a tartományvezérlők és PCMs fut, majd az akkumulátor kell lecserélni.</span><span class="sxs-lookup"><span data-stu-id="13fd8-178">If the state is still **DEGRADED** after 12 hours of continuous connection to AC power with the controllers and PCMs running, then the battery needs to be replaced.</span></span> <span data-ttu-id="13fd8-179">Adjon [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) helyettesítő biztonsági mentési akkumulátor modulként.</span><span class="sxs-lookup"><span data-stu-id="13fd8-179">Please [contact Microsoft Support](storsimple-contact-microsoft-support.md) for a replacement backup battery module.</span></span>
  * <span data-ttu-id="13fd8-180">A állapotba kerül, 12 óra elteltével az OK gombra, ha az akkumulátor működik, és azt csak akkor szükséges, a karbantartási kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="13fd8-180">If the state becomes OK after 12 hours, the battery is operational, and it only needed a maintenance charge.</span></span>
* <span data-ttu-id="13fd8-181">Ha nem lett az AC Power társított veszteséget és a PCM-e kapcsolva és csatlakoztatva AC power, az akkumulátor kell lecserélni.</span><span class="sxs-lookup"><span data-stu-id="13fd8-181">If there has not been an associated loss of AC power and the PCM is turned on and connected to AC power, the battery needs to be replaced.</span></span> <span data-ttu-id="13fd8-182">[Forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) helyettesítő biztonsági mentési akkumulátor modul rendezéséhez.</span><span class="sxs-lookup"><span data-stu-id="13fd8-182">[Contact Microsoft Support](storsimple-contact-microsoft-support.md) to order a replacement backup battery module.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="13fd8-183">Számolja fel a sikertelen akkumulátor nemzeti és regionális szabályok szerint.</span><span class="sxs-lookup"><span data-stu-id="13fd8-183">Dispose of the failed battery according to national and regional regulations.</span></span> 
> 
> 

## <a name="next-steps"></a><span data-ttu-id="13fd8-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13fd8-184">Next steps</span></span>
<span data-ttu-id="13fd8-185">További információ [StorSimple hardver összetevő cseréje](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="13fd8-185">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

