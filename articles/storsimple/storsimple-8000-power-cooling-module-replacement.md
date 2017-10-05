---
title: "Cserélje le a StorSimple 8000 series eszközén PCM |} Microsoft Docs"
description: "Ismerteti, hogyan eltávolítja és pótolja az energia- és hűtési modul (PCM) a StorSimple eszköz"
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
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: 7d181e6e434c998573dbea4b541cfacf7a28ee66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a><span data-ttu-id="df69c-103">Cserélje le a energia- és hűtési modul a StorSimple eszköz</span><span class="sxs-lookup"><span data-stu-id="df69c-103">Replace a Power and Cooling Module on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="df69c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="df69c-104">Overview</span></span>
<span data-ttu-id="df69c-105">A energia- és hűtési modul (PCM) a Microsoft Azure StorSimple eszközt a áll egy tápegység- és hűtési az elsődleges és a EBOD ház által szabályozott ventilátor.</span><span class="sxs-lookup"><span data-stu-id="df69c-105">The Power and Cooling Module (PCM) in your Microsoft Azure StorSimple device consists of a power supply and cooling fans that are controlled through the primary and EBOD enclosures.</span></span> <span data-ttu-id="df69c-106">Csak egy modellje, amely minden ház minősítéssel PCM van.</span><span class="sxs-lookup"><span data-stu-id="df69c-106">There is only one model of PCM that is certified for each enclosure.</span></span> <span data-ttu-id="df69c-107">Az elsődleges ház egy 764 W PCM minősítéssel, és a EBOD ház egy 580 W PCM minősítéssel.</span><span class="sxs-lookup"><span data-stu-id="df69c-107">The primary enclosure is certified for a 764 W PCM and the EBOD enclosure is certified for a 580 W PCM.</span></span> <span data-ttu-id="df69c-108">Annak ellenére, hogy az elsődleges ház és a EBOD ház PCMs különböző, a csere eljárás megegyezik.</span><span class="sxs-lookup"><span data-stu-id="df69c-108">Although the PCMs for the primary enclosure and the EBOD enclosure are different, the replacement procedure is identical.</span></span>

<span data-ttu-id="df69c-109">Ez az oktatóanyag azt ismerteti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="df69c-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="df69c-110">Távolítsa el a PCM</span><span class="sxs-lookup"><span data-stu-id="df69c-110">Remove a PCM</span></span>
* <span data-ttu-id="df69c-111">A helyettesítő PCM telepítése</span><span class="sxs-lookup"><span data-stu-id="df69c-111">Install a replacement PCM</span></span>

> [!IMPORTANT]
> <span data-ttu-id="df69c-112">Mielőtt eltávolítása és cseréje egy PCM, tekintse át a biztonsági információk [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="df69c-112">Before removing and replacing a PCM, review the safety information in [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>


## <a name="before-you-replace-a-pcm"></a><span data-ttu-id="df69c-113">Mielőtt lecseréli a PCM</span><span class="sxs-lookup"><span data-stu-id="df69c-113">Before you replace a PCM</span></span>
<span data-ttu-id="df69c-114">Vegye figyelembe a következő fontos problémák a PCM cseréje előtt:</span><span class="sxs-lookup"><span data-stu-id="df69c-114">Be aware of the following important issues before you replace your PCM:</span></span>

* <span data-ttu-id="df69c-115">Ha a PCM tápegység. meghibásodik, a hibás modul telepítve hagyja, de a tápkábel eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="df69c-115">If the power supply of the PCM fails, leave the faulty module installed, but remove the power cord.</span></span> <span data-ttu-id="df69c-116">A ventilátor power kapnak a ház, és továbbra is biztosítani a megfelelő hűtési továbbra is.</span><span class="sxs-lookup"><span data-stu-id="df69c-116">The fan will continue to receive power from the enclosure and continue to provide proper cooling.</span></span> <span data-ttu-id="df69c-117">Ha nem sikerül a ventilátor, a PCM kell azonnal le kell cserélni.</span><span class="sxs-lookup"><span data-stu-id="df69c-117">If the fan fails, the PCM needs to be replaced immediately.</span></span>
* <span data-ttu-id="df69c-118">Mielőtt eltávolítaná a PCM, válassza le a teljesítmény a PCM (ha van ilyen) a fő kapcsoló kikapcsolásával vagy a tápkábel fizikailag eltávolításával.</span><span class="sxs-lookup"><span data-stu-id="df69c-118">Before removing the PCM, disconnect the power from the PCM by turning off the main switch (where present) or by physically removing the power cord.</span></span> <span data-ttu-id="df69c-119">Így lehetővé teszi a rendszer figyelmeztetést, hogy egy power leállítás rövidesen.</span><span class="sxs-lookup"><span data-stu-id="df69c-119">This provides a warning to your system that a power shutdown is imminent.</span></span>
* <span data-ttu-id="df69c-120">Győződjön meg arról, hogy a többi PCM működési folyamatos rendszer működéséhez a hibás PCM cseréje előtt.</span><span class="sxs-lookup"><span data-stu-id="df69c-120">Make sure that the other PCM is functional for continued system operation before replacing the faulty PCM.</span></span> <span data-ttu-id="df69c-121">Hibás PCM kell helyettesíteni egy teljesen működőképes PCM lehető legrövidebb időn belül.</span><span class="sxs-lookup"><span data-stu-id="df69c-121">A faulty PCM must be replaced by a fully operational PCM as soon as possible.</span></span>
* <span data-ttu-id="df69c-122">PCM modul helyettesítő csak néhány percet is igénybe vehet, de túlmelegedése megelőzése érdekében sikertelen PCM eltávolításával 10 percen belül kell végezni.</span><span class="sxs-lookup"><span data-stu-id="df69c-122">PCM module replacement takes only few minutes to complete, but it must be completed within 10 minutes of removing the failed PCM to prevent overheating.</span></span>
* <span data-ttu-id="df69c-123">Vegye figyelembe, hogy a csere 764 W PCM modulok gyártól szállított tartalmaz a biztonsági mentési akkumulátor modul.</span><span class="sxs-lookup"><span data-stu-id="df69c-123">Note that the replacement 764 W PCM modules shipped from the factory do not contain the backup battery module.</span></span> <span data-ttu-id="df69c-124">Szüksége lesz az akkumulátor eltávolítása a hibás PCM, majd szúrja be a csere modulba váltja végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="df69c-124">You will need to remove the battery from your faulty PCM and then insert it into the replacement module prior to performing the replacement.</span></span> <span data-ttu-id="df69c-125">További információkért lásd: hogyan [távolítsa el, majd szúrja be a biztonsági mentési akkumulátor modul](storsimple-8000-battery-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="df69c-125">For more information, see how to [remove and insert a backup battery module](storsimple-8000-battery-replacement.md).</span></span>

## <a name="remove-a-pcm"></a><span data-ttu-id="df69c-126">Távolítsa el a PCM</span><span class="sxs-lookup"><span data-stu-id="df69c-126">Remove a PCM</span></span>
<span data-ttu-id="df69c-127">Kövesse ezeket az utasításokat, ha készen áll egy Power és hűtési modul (PCM) eltávolítása a Microsoft Azure StorSimple eszközt.</span><span class="sxs-lookup"><span data-stu-id="df69c-127">Follow these instructions when you are ready to remove a Power and Cooling Module (PCM) from your Microsoft Azure StorSimple device.</span></span>

> [!NOTE]
> <span data-ttu-id="df69c-128">A PCM eltávolítása előtt győződjön meg arról, hogy rendelkezik-e (az elsődleges ház W 764) vagy 580 W a EBOD ház a megfelelő helyettesíti.</span><span class="sxs-lookup"><span data-stu-id="df69c-128">Before you remove your PCM, verify that you have a correct replacement (764 W for the primary enclosure or 580 W for the EBOD enclosure).</span></span>

#### <a name="to-remove-a-pcm"></a><span data-ttu-id="df69c-129">Egy PCM eltávolítása</span><span class="sxs-lookup"><span data-stu-id="df69c-129">To remove a PCM</span></span>
1. <span data-ttu-id="df69c-130">A klasszikus Azure portálon kattintson **beállítások > figyelő > hardver állapotának**.</span><span class="sxs-lookup"><span data-stu-id="df69c-130">In the Azure classic portal, click **Settings > Monitor > Hardware health**.</span></span> <span data-ttu-id="df69c-131">Tekintse meg a PCM összetevők alatt **összetevők megosztott** azonosításához, amely PCM sikertelen volt:</span><span class="sxs-lookup"><span data-stu-id="df69c-131">Check the status of the PCM components under **Shared components** to identify which PCM has failed:</span></span>
   
   * <span data-ttu-id="df69c-132">Ha a tápegységet PCM 0 sikertelen volt, állapotának **tápegység PCM 0 a** pedig piros színűvé változik.</span><span class="sxs-lookup"><span data-stu-id="df69c-132">If a power supply in PCM 0 has failed, the status of **Power Supply in PCM 0** will be red.</span></span>
   * <span data-ttu-id="df69c-133">Ha egy tápegység PCM az 1. sikertelen volt, állapotának **PCM az 1. tápegység** pedig piros színűvé változik.</span><span class="sxs-lookup"><span data-stu-id="df69c-133">If a power supply in PCM 1 has failed, the status of **Power Supply in PCM 1** will be red.</span></span>
   * <span data-ttu-id="df69c-134">Ha a ventilátor PCM az 1. sikertelen volt, vagy állapotának **0 hűtési PCM 0 a** vagy **PCM 0 1 hűtési** pedig piros színűvé változik.</span><span class="sxs-lookup"><span data-stu-id="df69c-134">If the fan in PCM 1 has failed, the status of either **Cooling 0 for PCM 0** or **Cooling 1 for PCM 0** will be red.</span></span>
2. <span data-ttu-id="df69c-135">Keresse meg a sikertelen PCM az elsődleges ház hátulján olvasható.</span><span class="sxs-lookup"><span data-stu-id="df69c-135">Locate the failed PCM on the back of the primary enclosure.</span></span> <span data-ttu-id="df69c-136">Ha futtat egy 8600 modell, azonosítsa az elsődleges ház alapján a rendszer egység azonosítószámának a előlapján LED megjelenítendő látható.</span><span class="sxs-lookup"><span data-stu-id="df69c-136">If you are running an 8600 model, identify the primary enclosure by looking at the System Unit Identification Number shown on the front panel LED display.</span></span> <span data-ttu-id="df69c-137">Az alapértelmezett érték a egység azonosítója: jelenik meg a elsődleges ház **00**, mivel az alapértelmezett egység azonosítója: jelenik meg a EBOD ház **01**.</span><span class="sxs-lookup"><span data-stu-id="df69c-137">The default Unit ID displayed on the primary enclosure is **00**, whereas the default Unit ID displayed on the EBOD enclosure is **01**.</span></span> <span data-ttu-id="df69c-138">A következő ábra és táblázat magyarázza el, a LED megjelenítési előlapja.</span><span class="sxs-lookup"><span data-stu-id="df69c-138">The following diagram and table explain the front panel of the LED display.</span></span>
   
    ![Előlapján OPS azonosító](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     <span data-ttu-id="df69c-140">**1. ábra** az eszköz első panel</span><span class="sxs-lookup"><span data-stu-id="df69c-140">**Figure 1** Front panel of the device</span></span>  
   
   | <span data-ttu-id="df69c-141">Címke</span><span class="sxs-lookup"><span data-stu-id="df69c-141">Label</span></span> | <span data-ttu-id="df69c-142">Leírás</span><span class="sxs-lookup"><span data-stu-id="df69c-142">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="df69c-143">1</span><span class="sxs-lookup"><span data-stu-id="df69c-143">1</span></span> |<span data-ttu-id="df69c-144">Némító gomb</span><span class="sxs-lookup"><span data-stu-id="df69c-144">Mute button</span></span> |
   | <span data-ttu-id="df69c-145">2</span><span class="sxs-lookup"><span data-stu-id="df69c-145">2</span></span> |<span data-ttu-id="df69c-146">Rendszer energiagazdálkodási</span><span class="sxs-lookup"><span data-stu-id="df69c-146">System power</span></span> |
   | <span data-ttu-id="df69c-147">3</span><span class="sxs-lookup"><span data-stu-id="df69c-147">3</span></span> |<span data-ttu-id="df69c-148">A modul hiba</span><span class="sxs-lookup"><span data-stu-id="df69c-148">Module fault</span></span> |
   | <span data-ttu-id="df69c-149">4</span><span class="sxs-lookup"><span data-stu-id="df69c-149">4</span></span> |<span data-ttu-id="df69c-150">Logikai hiba</span><span class="sxs-lookup"><span data-stu-id="df69c-150">Logical fault</span></span> |
   | <span data-ttu-id="df69c-151">5</span><span class="sxs-lookup"><span data-stu-id="df69c-151">5</span></span> |<span data-ttu-id="df69c-152">Egység azonosító megjelenítése</span><span class="sxs-lookup"><span data-stu-id="df69c-152">Unit ID display</span></span> |
3. <span data-ttu-id="df69c-153">A figyelési kijelző LED hátulján az elsődleges ház is használható a hibás PCM azonosításához.</span><span class="sxs-lookup"><span data-stu-id="df69c-153">The monitoring indicator LEDs in the back of the primary enclosure can also be used to identify the faulty PCM.</span></span> <span data-ttu-id="df69c-154">Tekintse meg a következő ábra és táblázat segít megérteni, hogyan keresse meg a hibás PCM a LED segítségével.</span><span class="sxs-lookup"><span data-stu-id="df69c-154">See the following diagram and table to understand how to use the LEDs to locate the faulty PCM.</span></span> <span data-ttu-id="df69c-155">Például ha a LED megfelelő a **ventilátor sikertelen** van bekapcsolásával; a ventilátor sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="df69c-155">For example, if the LED corresponding to the **Fan Fail** is lit, the fan has failed.</span></span> <span data-ttu-id="df69c-156">Hasonlóképpen ha a LED megfelelő **AC sikertelen** van bekapcsolásával, az áramellátás sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="df69c-156">Likewise, if the LED corresponding to **AC Fail** is lit, the power supply has failed.</span></span> 
   
    ![Eszköz PCM figyelési kijelző LED Csatlakozópanel](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     <span data-ttu-id="df69c-158">**2. ábra** vissza a PCM a kijelző LED</span><span class="sxs-lookup"><span data-stu-id="df69c-158">**Figure 2** Back of PCM with indicator LEDs</span></span>
   
   | <span data-ttu-id="df69c-159">Címke</span><span class="sxs-lookup"><span data-stu-id="df69c-159">Label</span></span> | <span data-ttu-id="df69c-160">Leírás</span><span class="sxs-lookup"><span data-stu-id="df69c-160">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="df69c-161">1</span><span class="sxs-lookup"><span data-stu-id="df69c-161">1</span></span> |<span data-ttu-id="df69c-162">AC áramszünet esetén</span><span class="sxs-lookup"><span data-stu-id="df69c-162">AC power failure</span></span> |
   | <span data-ttu-id="df69c-163">2</span><span class="sxs-lookup"><span data-stu-id="df69c-163">2</span></span> |<span data-ttu-id="df69c-164">Hiba ventilátor</span><span class="sxs-lookup"><span data-stu-id="df69c-164">Fan failure</span></span> |
   | <span data-ttu-id="df69c-165">3</span><span class="sxs-lookup"><span data-stu-id="df69c-165">3</span></span> |<span data-ttu-id="df69c-166">Akkumulátor hiba</span><span class="sxs-lookup"><span data-stu-id="df69c-166">Battery fault</span></span> |
   | <span data-ttu-id="df69c-167">4</span><span class="sxs-lookup"><span data-stu-id="df69c-167">4</span></span> |<span data-ttu-id="df69c-168">PCM OK</span><span class="sxs-lookup"><span data-stu-id="df69c-168">PCM OK</span></span> |
   | <span data-ttu-id="df69c-169">5</span><span class="sxs-lookup"><span data-stu-id="df69c-169">5</span></span> |<span data-ttu-id="df69c-170">DC áramszünet esetén</span><span class="sxs-lookup"><span data-stu-id="df69c-170">DC power failure</span></span> |
   | <span data-ttu-id="df69c-171">6</span><span class="sxs-lookup"><span data-stu-id="df69c-171">6</span></span> |<span data-ttu-id="df69c-172">Kifogástalan akkumulátor</span><span class="sxs-lookup"><span data-stu-id="df69c-172">Battery healthy</span></span> |
4. <span data-ttu-id="df69c-173">Tekintse meg a következő ábra a háttér a StorSimple eszköz keresse meg a sikertelen PCM modul.</span><span class="sxs-lookup"><span data-stu-id="df69c-173">Refer to the following diagram of the back of the StorSimple device to locate the failed PCM module.</span></span> <span data-ttu-id="df69c-174">PCM 0 a bal oldali és a jobb oldalon pedig PCM 1.</span><span class="sxs-lookup"><span data-stu-id="df69c-174">PCM 0 is on the left and PCM 1 is on the right.</span></span> <span data-ttu-id="df69c-175">Az alábbi táblázat ismerteti a modulokat.</span><span class="sxs-lookup"><span data-stu-id="df69c-175">The table that follows explains the modules.</span></span>
   
     ![Az eszköz elsődleges ház modulok Csatlakozópanel](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     <span data-ttu-id="df69c-177">**3. ábra** oldalán a beépülő modulok eszköz</span><span class="sxs-lookup"><span data-stu-id="df69c-177">**Figure 3** Back of device with plug-in modules</span></span> 
   
   | <span data-ttu-id="df69c-178">Címke</span><span class="sxs-lookup"><span data-stu-id="df69c-178">Label</span></span> | <span data-ttu-id="df69c-179">Leírás</span><span class="sxs-lookup"><span data-stu-id="df69c-179">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="df69c-180">1</span><span class="sxs-lookup"><span data-stu-id="df69c-180">1</span></span> |<span data-ttu-id="df69c-181">PCM 0</span><span class="sxs-lookup"><span data-stu-id="df69c-181">PCM 0</span></span> |
   | <span data-ttu-id="df69c-182">2</span><span class="sxs-lookup"><span data-stu-id="df69c-182">2</span></span> |<span data-ttu-id="df69c-183">PCM 1</span><span class="sxs-lookup"><span data-stu-id="df69c-183">PCM 1</span></span> |
   | <span data-ttu-id="df69c-184">3</span><span class="sxs-lookup"><span data-stu-id="df69c-184">3</span></span> |<span data-ttu-id="df69c-185">A vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="df69c-185">Controller 0</span></span> |
   | <span data-ttu-id="df69c-186">4</span><span class="sxs-lookup"><span data-stu-id="df69c-186">4</span></span> |<span data-ttu-id="df69c-187">1. vezérlő</span><span class="sxs-lookup"><span data-stu-id="df69c-187">Controller 1</span></span> |
5. <span data-ttu-id="df69c-188">Kapcsolja ki a hibás PCM, és húzza ki a forrás.</span><span class="sxs-lookup"><span data-stu-id="df69c-188">Turn off the faulty PCM and disconnect the power supply cord.</span></span> <span data-ttu-id="df69c-189">Most már eltávolíthatja a PCM.</span><span class="sxs-lookup"><span data-stu-id="df69c-189">You can now remove the PCM.</span></span>
6. <span data-ttu-id="df69c-190">A zárolás és az USB és mutatóujj között a PCM leíró oldalán megfogható, és nyomja össze együtt, hogy a leírót megnyitni.</span><span class="sxs-lookup"><span data-stu-id="df69c-190">Grasp the latch and the side of the PCM handle between your thumb and forefinger, and squeeze them together to open the handle.</span></span>
   
    ![Nyitó PCM leíró](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    <span data-ttu-id="df69c-192">**4. ábra** a PCM leíró megnyitása</span><span class="sxs-lookup"><span data-stu-id="df69c-192">**Figure 4** Opening the PCM handle</span></span>
7. <span data-ttu-id="df69c-193">A leíró fogja túl, és távolítsa el a PCM.</span><span class="sxs-lookup"><span data-stu-id="df69c-193">Grip the handle and remove the PCM.</span></span>
   
    ![PCM-eszközök eltávolítása](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    <span data-ttu-id="df69c-195">**5. ábra** a PCM eltávolítása</span><span class="sxs-lookup"><span data-stu-id="df69c-195">**Figure 5** Removing the PCM</span></span>

## <a name="install-a-replacement-pcm"></a><span data-ttu-id="df69c-196">A helyettesítő PCM telepítése</span><span class="sxs-lookup"><span data-stu-id="df69c-196">Install a replacement PCM</span></span>
<span data-ttu-id="df69c-197">Kövesse ezeket az utasításokat egy PCM a StorSimple eszköz telepítése.</span><span class="sxs-lookup"><span data-stu-id="df69c-197">Follow these instructions to install a PCM in your StorSimple device.</span></span> <span data-ttu-id="df69c-198">Győződjön meg arról, hogy a csere PCM (764 W PCMs vonatkozik) telepítése előtt a biztonsági mentési akkumulátor modul beszúrt.</span><span class="sxs-lookup"><span data-stu-id="df69c-198">Ensure that you have inserted the backup battery module prior to installing the replacement PCM (applies to 764 W PCMs only).</span></span> <span data-ttu-id="df69c-199">További információkért lásd: hogyan [távolítsa el, majd szúrja be a biztonsági mentési akkumulátor modul](storsimple-8000-battery-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="df69c-199">For more information, see how to [remove and insert a backup battery module](storsimple-8000-battery-replacement.md).</span></span>

#### <a name="to-install-a-pcm"></a><span data-ttu-id="df69c-200">Egy PCM telepítése</span><span class="sxs-lookup"><span data-stu-id="df69c-200">To install a PCM</span></span>
1. <span data-ttu-id="df69c-201">Győződjön meg arról, hogy rendelkezik-e a ház megfelelő PCM helyettesítője.</span><span class="sxs-lookup"><span data-stu-id="df69c-201">Verify that you have the correct replacement PCM for this enclosure.</span></span> <span data-ttu-id="df69c-202">Az elsődleges ház kell egy 764 W PCM, és a EBOD ház kell egy 580 W PCM.</span><span class="sxs-lookup"><span data-stu-id="df69c-202">The primary enclosure needs a 764 W PCM and the EBOD enclosure needs a 580 W PCM.</span></span> <span data-ttu-id="df69c-203">Ne próbáljon a 580 W PCM az elsődleges szolgáltatással, illetve a 764 W PCM a EBOD szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="df69c-203">You should not attempt to use the 580 W PCM in the Primary enclosure, or the 764 W PCM in the EBOD enclosure.</span></span> <span data-ttu-id="df69c-204">A következő kép bemutatja, hol határozza meg az adatokat, amelyek a PCM dobozának a címkén.</span><span class="sxs-lookup"><span data-stu-id="df69c-204">The following image shows where to identify this information on the label that is affixed to the PCM.</span></span>
   
    ![Eszköz PCM címke](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    <span data-ttu-id="df69c-206">**6. ábra** PCM címke</span><span class="sxs-lookup"><span data-stu-id="df69c-206">**Figure 6** PCM label</span></span>
2. <span data-ttu-id="df69c-207">Ellenőrizze, hogy a ház, különös tekintettel az összekötők való megosztása kárt.</span><span class="sxs-lookup"><span data-stu-id="df69c-207">Check for damage to the enclosure, paying particular attention to the connectors.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="df69c-208">**A modul nem telepítése, ha bármely összekötő elgörbülve.**</span><span class="sxs-lookup"><span data-stu-id="df69c-208">**Do not install the module if any connector pins are bent.**</span></span>
   > 
   > 
3. <span data-ttu-id="df69c-209">A nyitott állapotban PCM leíró, és csúsztassa be a modul a ház be.</span><span class="sxs-lookup"><span data-stu-id="df69c-209">With the PCM handle in the open position, slide the module into the enclosure.</span></span>
   
    ![PCM eszköz telepítése](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    <span data-ttu-id="df69c-211">**7. ábra** a PCM telepítése</span><span class="sxs-lookup"><span data-stu-id="df69c-211">**Figure 7** Installing the PCM</span></span>
4. <span data-ttu-id="df69c-212">Kézzel zárja be a PCM leíró.</span><span class="sxs-lookup"><span data-stu-id="df69c-212">Manually close the PCM handle.</span></span> <span data-ttu-id="df69c-213">Egy kattintással kell hall, mivel a leíró zárolás kapcsolatba lép.</span><span class="sxs-lookup"><span data-stu-id="df69c-213">You should hear a click as the handle latch engages.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="df69c-214">Győződjön meg arról, hogy folytat rendelkezik-e az összekötő PIN-kód, hogy akkor is óvatosan tug a leírón a zárolás feloldása nélkül.</span><span class="sxs-lookup"><span data-stu-id="df69c-214">To ensure that the connector pins have engaged, you can gently tug on the handle without releasing the latch.</span></span> <span data-ttu-id="df69c-215">A PCM diák ki, ha azt feltételezi, hogy a zárolás bezárult, mielőtt az összekötők részt vevő.</span><span class="sxs-lookup"><span data-stu-id="df69c-215">If the PCM slides out, it implies that the latch was closed before the connectors engaged.</span></span>
   
5. <span data-ttu-id="df69c-216">A tápkábelek csatlakoztatja a áramforrásról és a PCM.</span><span class="sxs-lookup"><span data-stu-id="df69c-216">Connect the power cables to the power source and to the PCM.</span></span>
6. <span data-ttu-id="df69c-217">A törzs mentesség bálákban biztonságos.</span><span class="sxs-lookup"><span data-stu-id="df69c-217">Secure the strain relief bales.</span></span>
7. <span data-ttu-id="df69c-218">Kapcsolja be a PCM.</span><span class="sxs-lookup"><span data-stu-id="df69c-218">Turn on the PCM.</span></span>
8. <span data-ttu-id="df69c-219">Győződjön meg arról, hogy sikeres volt-e a helyettesítő: az Azure-portálon a StorSimple eszköz kezelő szolgáltatás, keresse meg az eszköz, és ezután **beállítások > figyelő > hardver állapotának**.</span><span class="sxs-lookup"><span data-stu-id="df69c-219">Verify that the replacement was successful: in the Azure portal of your StorSimple Device Manager service, navigate to your device and then to **Settings > Monitor > Hardware health**.</span></span> <span data-ttu-id="df69c-220">Az a **összetevők megosztott**, a PCM állapota zöld kell lennie.</span><span class="sxs-lookup"><span data-stu-id="df69c-220">Under the **Shared components**, the status of the PCM should be green.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="df69c-221">A csere PCM teljesen inicializálni a néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="df69c-221">It may take a few minutes for the replacement PCM to completely initialize.</span></span>

## <a name="next-steps"></a><span data-ttu-id="df69c-222">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="df69c-222">Next steps</span></span>
<span data-ttu-id="df69c-223">További információ [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="df69c-223">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

