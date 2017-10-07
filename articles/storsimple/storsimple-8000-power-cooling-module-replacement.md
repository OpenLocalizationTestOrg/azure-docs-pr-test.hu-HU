---
title: "a StorSimple 8000 series eszközén PCM aaaReplace |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooremove és csere hello energia- és hűtési modul (PCM) a StorSimple eszköz"
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
ms.openlocfilehash: 474fd09787c5361a81efda4de74356027ac60f47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a><span data-ttu-id="4a399-103">Cserélje le a energia- és hűtési modul a StorSimple eszköz</span><span class="sxs-lookup"><span data-stu-id="4a399-103">Replace a Power and Cooling Module on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="4a399-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4a399-104">Overview</span></span>
<span data-ttu-id="4a399-105">hello Power és hűtési modul (PCM) a Microsoft Azure StorSimple eszközt a áll egy tápegység- és hűtési elsődleges hello és EBOD csatolmányt által szabályozott ventilátor.</span><span class="sxs-lookup"><span data-stu-id="4a399-105">hello Power and Cooling Module (PCM) in your Microsoft Azure StorSimple device consists of a power supply and cooling fans that are controlled through hello primary and EBOD enclosures.</span></span> <span data-ttu-id="4a399-106">Csak egy modellje, amely minden ház minősítéssel PCM van.</span><span class="sxs-lookup"><span data-stu-id="4a399-106">There is only one model of PCM that is certified for each enclosure.</span></span> <span data-ttu-id="4a399-107">hello elsődleges ház egy 764 W PCM minősítéssel, és egy 580 W PCM hello EBOD ház minősítéssel.</span><span class="sxs-lookup"><span data-stu-id="4a399-107">hello primary enclosure is certified for a 764 W PCM and hello EBOD enclosure is certified for a 580 W PCM.</span></span> <span data-ttu-id="4a399-108">Annak ellenére, hogy hello PCMs hello elsődleges ház és hello EBOD ház különböző, hello helyettesítő eljárás megegyezik.</span><span class="sxs-lookup"><span data-stu-id="4a399-108">Although hello PCMs for hello primary enclosure and hello EBOD enclosure are different, hello replacement procedure is identical.</span></span>

<span data-ttu-id="4a399-109">Ez az oktatóanyag azt ismerteti, hogyan:</span><span class="sxs-lookup"><span data-stu-id="4a399-109">This tutorial explains how to:</span></span>

* <span data-ttu-id="4a399-110">Távolítsa el a PCM</span><span class="sxs-lookup"><span data-stu-id="4a399-110">Remove a PCM</span></span>
* <span data-ttu-id="4a399-111">A helyettesítő PCM telepítése</span><span class="sxs-lookup"><span data-stu-id="4a399-111">Install a replacement PCM</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4a399-112">Mielőtt eltávolítása és cseréje egy PCM, tekintse át a hello biztonsági adatokat a [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="4a399-112">Before removing and replacing a PCM, review hello safety information in [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>


## <a name="before-you-replace-a-pcm"></a><span data-ttu-id="4a399-113">Mielőtt lecseréli a PCM</span><span class="sxs-lookup"><span data-stu-id="4a399-113">Before you replace a PCM</span></span>
<span data-ttu-id="4a399-114">Vegye figyelembe a fontos problémákat követően lecseréli a PCM hello:</span><span class="sxs-lookup"><span data-stu-id="4a399-114">Be aware of hello following important issues before you replace your PCM:</span></span>

* <span data-ttu-id="4a399-115">Ha hello tápegység a meghiúsul PCM, hello hello hibás modul telepítve hagyja, de hello tápkábelét eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="4a399-115">If hello power supply of hello PCM fails, leave hello faulty module installed, but remove hello power cord.</span></span> <span data-ttu-id="4a399-116">hello ventilátor hello ház tooreceive tápellátás folytatódik, és folytatni a tooprovide megfelelő hűtési.</span><span class="sxs-lookup"><span data-stu-id="4a399-116">hello fan will continue tooreceive power from hello enclosure and continue tooprovide proper cooling.</span></span> <span data-ttu-id="4a399-117">Hello ventilátor meghibásodásakor hello PCM kell cserélni azonnal toobe.</span><span class="sxs-lookup"><span data-stu-id="4a399-117">If hello fan fails, hello PCM needs toobe replaced immediately.</span></span>
* <span data-ttu-id="4a399-118">Mielőtt eltávolítaná a hello PCM, válassza le hello power hello PCM (ha van ilyen) hello fő kapcsoló kikapcsolásával vagy hello tápkábelét fizikailag eltávolításával.</span><span class="sxs-lookup"><span data-stu-id="4a399-118">Before removing hello PCM, disconnect hello power from hello PCM by turning off hello main switch (where present) or by physically removing hello power cord.</span></span> <span data-ttu-id="4a399-119">Ez biztosítja, hogy egy power leállítás rövidesen figyelmeztető tooyour rendszer.</span><span class="sxs-lookup"><span data-stu-id="4a399-119">This provides a warning tooyour system that a power shutdown is imminent.</span></span>
* <span data-ttu-id="4a399-120">Győződjön meg arról, hogy más PCM működőképességét a hello folytatható rendszer cseréje hello hibás PCM előtt.</span><span class="sxs-lookup"><span data-stu-id="4a399-120">Make sure that hello other PCM is functional for continued system operation before replacing hello faulty PCM.</span></span> <span data-ttu-id="4a399-121">Hibás PCM kell helyettesíteni egy teljesen működőképes PCM lehető legrövidebb időn belül.</span><span class="sxs-lookup"><span data-stu-id="4a399-121">A faulty PCM must be replaced by a fully operational PCM as soon as possible.</span></span>
* <span data-ttu-id="4a399-122">PCM modul helyettesítő csak néhány perc toocomplete vesz igénybe, de eltávolítása nem sikerült hello PCM tooprevent túlmelegedése követő 10 percen belül kell végezni.</span><span class="sxs-lookup"><span data-stu-id="4a399-122">PCM module replacement takes only few minutes toocomplete, but it must be completed within 10 minutes of removing hello failed PCM tooprevent overheating.</span></span>
* <span data-ttu-id="4a399-123">Vegye figyelembe, hogy hello helyettesítő 764 W PCM modulok hello gyárból származó szállított tartalmaz hello biztonsági mentési akkumulátor modul.</span><span class="sxs-lookup"><span data-stu-id="4a399-123">Note that hello replacement 764 W PCM modules shipped from hello factory do not contain hello backup battery module.</span></span> <span data-ttu-id="4a399-124">Először a hibás PCM tooremove hello akkumulátor kell, majd beilleszteni hello helyettesítő modul előzetes tooperforming hello helyettesítő.</span><span class="sxs-lookup"><span data-stu-id="4a399-124">You will need tooremove hello battery from your faulty PCM and then insert it into hello replacement module prior tooperforming hello replacement.</span></span> <span data-ttu-id="4a399-125">További információkért lásd: hogyan túl[távolítsa el, majd szúrja be a biztonsági mentési akkumulátor modul](storsimple-8000-battery-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="4a399-125">For more information, see how too[remove and insert a backup battery module](storsimple-8000-battery-replacement.md).</span></span>

## <a name="remove-a-pcm"></a><span data-ttu-id="4a399-126">Távolítsa el a PCM</span><span class="sxs-lookup"><span data-stu-id="4a399-126">Remove a PCM</span></span>
<span data-ttu-id="4a399-127">Kövesse ezeket az utasításokat, ha készen áll a tooremove egy Power és hűtési modul (PCM) a Microsoft Azure StorSimple eszközön.</span><span class="sxs-lookup"><span data-stu-id="4a399-127">Follow these instructions when you are ready tooremove a Power and Cooling Module (PCM) from your Microsoft Azure StorSimple device.</span></span>

> [!NOTE]
> <span data-ttu-id="4a399-128">A PCM eltávolítása előtt győződjön meg arról, hogy rendelkezik-e a megfelelő helyettesítő (764 hello elsődleges ház W) vagy a hello EBOD ház W 580.</span><span class="sxs-lookup"><span data-stu-id="4a399-128">Before you remove your PCM, verify that you have a correct replacement (764 W for hello primary enclosure or 580 W for hello EBOD enclosure).</span></span>

#### <a name="tooremove-a-pcm"></a><span data-ttu-id="4a399-129">egy PCM tooremove</span><span class="sxs-lookup"><span data-stu-id="4a399-129">tooremove a PCM</span></span>
1. <span data-ttu-id="4a399-130">Hello a klasszikus Azure portálon, kattintson **beállítások > figyelő > hardver állapotának**.</span><span class="sxs-lookup"><span data-stu-id="4a399-130">In hello Azure classic portal, click **Settings > Monitor > Hardware health**.</span></span> <span data-ttu-id="4a399-131">Hello PCM összetevők alatt hello állapotának ellenőrzése **összetevők megosztott** tooidentify, amely PCM sikertelen volt:</span><span class="sxs-lookup"><span data-stu-id="4a399-131">Check hello status of hello PCM components under **Shared components** tooidentify which PCM has failed:</span></span>
   
   * <span data-ttu-id="4a399-132">Nem sikerült a tápegységet PCM 0, ha hello állapotának **tápegység PCM 0 a** pedig piros színűvé változik.</span><span class="sxs-lookup"><span data-stu-id="4a399-132">If a power supply in PCM 0 has failed, hello status of **Power Supply in PCM 0** will be red.</span></span>
   * <span data-ttu-id="4a399-133">Ha egy tápegység PCM az 1. sikertelen volt, hello állapotának **PCM az 1. tápegység** pedig piros színűvé változik.</span><span class="sxs-lookup"><span data-stu-id="4a399-133">If a power supply in PCM 1 has failed, hello status of **Power Supply in PCM 1** will be red.</span></span>
   * <span data-ttu-id="4a399-134">Ha hello ventilátor PCM az 1. sikertelen volt, vagy állapotának hello **0 hűtési PCM 0 a** vagy **PCM 0 1 hűtési** pedig piros színűvé változik.</span><span class="sxs-lookup"><span data-stu-id="4a399-134">If hello fan in PCM 1 has failed, hello status of either **Cooling 0 for PCM 0** or **Cooling 1 for PCM 0** will be red.</span></span>
2. <span data-ttu-id="4a399-135">Keresse meg a hello sikertelen PCM biztonsági a hello elsődleges ház hello.</span><span class="sxs-lookup"><span data-stu-id="4a399-135">Locate hello failed PCM on hello back of hello primary enclosure.</span></span> <span data-ttu-id="4a399-136">Ha futtat egy 8600 modell, azonosítsa a hello elsődleges ház hello hello előlapja LED képernyőjén látható rendszer azonosító száma alapján.</span><span class="sxs-lookup"><span data-stu-id="4a399-136">If you are running an 8600 model, identify hello primary enclosure by looking at hello System Unit Identification Number shown on hello front panel LED display.</span></span> <span data-ttu-id="4a399-137">az alapértelmezett egység azonosítója: hello elsődleges ház megjelenő hello **00**, mivel egység azonosítója: hello EBOD ház megjelenő hello alapértelmezett **01**.</span><span class="sxs-lookup"><span data-stu-id="4a399-137">hello default Unit ID displayed on hello primary enclosure is **00**, whereas hello default Unit ID displayed on hello EBOD enclosure is **01**.</span></span> <span data-ttu-id="4a399-138">hello következő ábra és táblázat ismertetik hello előlapja hello LED megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="4a399-138">hello following diagram and table explain hello front panel of hello LED display.</span></span>
   
    ![Előlapján OPS azonosító](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     <span data-ttu-id="4a399-140">**1. ábra** első panel hello eszköz</span><span class="sxs-lookup"><span data-stu-id="4a399-140">**Figure 1** Front panel of hello device</span></span>  
   
   | <span data-ttu-id="4a399-141">Címke</span><span class="sxs-lookup"><span data-stu-id="4a399-141">Label</span></span> | <span data-ttu-id="4a399-142">Leírás</span><span class="sxs-lookup"><span data-stu-id="4a399-142">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="4a399-143">1</span><span class="sxs-lookup"><span data-stu-id="4a399-143">1</span></span> |<span data-ttu-id="4a399-144">Némító gomb</span><span class="sxs-lookup"><span data-stu-id="4a399-144">Mute button</span></span> |
   | <span data-ttu-id="4a399-145">2</span><span class="sxs-lookup"><span data-stu-id="4a399-145">2</span></span> |<span data-ttu-id="4a399-146">Rendszer energiagazdálkodási</span><span class="sxs-lookup"><span data-stu-id="4a399-146">System power</span></span> |
   | <span data-ttu-id="4a399-147">3</span><span class="sxs-lookup"><span data-stu-id="4a399-147">3</span></span> |<span data-ttu-id="4a399-148">A modul hiba</span><span class="sxs-lookup"><span data-stu-id="4a399-148">Module fault</span></span> |
   | <span data-ttu-id="4a399-149">4</span><span class="sxs-lookup"><span data-stu-id="4a399-149">4</span></span> |<span data-ttu-id="4a399-150">Logikai hiba</span><span class="sxs-lookup"><span data-stu-id="4a399-150">Logical fault</span></span> |
   | <span data-ttu-id="4a399-151">5</span><span class="sxs-lookup"><span data-stu-id="4a399-151">5</span></span> |<span data-ttu-id="4a399-152">Egység azonosító megjelenítése</span><span class="sxs-lookup"><span data-stu-id="4a399-152">Unit ID display</span></span> |
3. <span data-ttu-id="4a399-153">kijelző LED hello hello elsődleges ház oldalán a figyelési hello is tooidentify hello hibás PCM.</span><span class="sxs-lookup"><span data-stu-id="4a399-153">hello monitoring indicator LEDs in hello back of hello primary enclosure can also be used tooidentify hello faulty PCM.</span></span> <span data-ttu-id="4a399-154">Hello következő ábra és táblázat hogyan toounderstand toouse hello LED toolocate hello hibás PCM.</span><span class="sxs-lookup"><span data-stu-id="4a399-154">See hello following diagram and table toounderstand how toouse hello LEDs toolocate hello faulty PCM.</span></span> <span data-ttu-id="4a399-155">Például ha hello vezetett megfelelő toohello **ventilátor sikertelen** van bekapcsolásával; hello ventilátor sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="4a399-155">For example, if hello LED corresponding toohello **Fan Fail** is lit, hello fan has failed.</span></span> <span data-ttu-id="4a399-156">Hasonlóképpen, ha hello vezetett túl megfelelő**AC sikertelen** van bekapcsolásával; hello tápegység sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="4a399-156">Likewise, if hello LED corresponding too**AC Fail** is lit, hello power supply has failed.</span></span> 
   
    ![Eszköz PCM figyelési kijelző LED Csatlakozópanel](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     <span data-ttu-id="4a399-158">**2. ábra** vissza a PCM a kijelző LED</span><span class="sxs-lookup"><span data-stu-id="4a399-158">**Figure 2** Back of PCM with indicator LEDs</span></span>
   
   | <span data-ttu-id="4a399-159">Címke</span><span class="sxs-lookup"><span data-stu-id="4a399-159">Label</span></span> | <span data-ttu-id="4a399-160">Leírás</span><span class="sxs-lookup"><span data-stu-id="4a399-160">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="4a399-161">1</span><span class="sxs-lookup"><span data-stu-id="4a399-161">1</span></span> |<span data-ttu-id="4a399-162">AC áramszünet esetén</span><span class="sxs-lookup"><span data-stu-id="4a399-162">AC power failure</span></span> |
   | <span data-ttu-id="4a399-163">2</span><span class="sxs-lookup"><span data-stu-id="4a399-163">2</span></span> |<span data-ttu-id="4a399-164">Hiba ventilátor</span><span class="sxs-lookup"><span data-stu-id="4a399-164">Fan failure</span></span> |
   | <span data-ttu-id="4a399-165">3</span><span class="sxs-lookup"><span data-stu-id="4a399-165">3</span></span> |<span data-ttu-id="4a399-166">Akkumulátor hiba</span><span class="sxs-lookup"><span data-stu-id="4a399-166">Battery fault</span></span> |
   | <span data-ttu-id="4a399-167">4</span><span class="sxs-lookup"><span data-stu-id="4a399-167">4</span></span> |<span data-ttu-id="4a399-168">PCM OK</span><span class="sxs-lookup"><span data-stu-id="4a399-168">PCM OK</span></span> |
   | <span data-ttu-id="4a399-169">5</span><span class="sxs-lookup"><span data-stu-id="4a399-169">5</span></span> |<span data-ttu-id="4a399-170">DC áramszünet esetén</span><span class="sxs-lookup"><span data-stu-id="4a399-170">DC power failure</span></span> |
   | <span data-ttu-id="4a399-171">6</span><span class="sxs-lookup"><span data-stu-id="4a399-171">6</span></span> |<span data-ttu-id="4a399-172">Kifogástalan akkumulátor</span><span class="sxs-lookup"><span data-stu-id="4a399-172">Battery healthy</span></span> |
4. <span data-ttu-id="4a399-173">Tekintse meg a következő hátsó hello StorSimple eszköz toolocate sikertelen hello PCM modul hello ábrája toohello.</span><span class="sxs-lookup"><span data-stu-id="4a399-173">Refer toohello following diagram of hello back of hello StorSimple device toolocate hello failed PCM module.</span></span> <span data-ttu-id="4a399-174">Hello bal oldali van PCM 0 és PCM 1 hello jobb.</span><span class="sxs-lookup"><span data-stu-id="4a399-174">PCM 0 is on hello left and PCM 1 is on hello right.</span></span> <span data-ttu-id="4a399-175">hello táblázatot hello modulok ismerteti.</span><span class="sxs-lookup"><span data-stu-id="4a399-175">hello table that follows explains hello modules.</span></span>
   
     ![Az eszköz elsődleges ház modulok Csatlakozópanel](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     <span data-ttu-id="4a399-177">**3. ábra** oldalán a beépülő modulok eszköz</span><span class="sxs-lookup"><span data-stu-id="4a399-177">**Figure 3** Back of device with plug-in modules</span></span> 
   
   | <span data-ttu-id="4a399-178">Címke</span><span class="sxs-lookup"><span data-stu-id="4a399-178">Label</span></span> | <span data-ttu-id="4a399-179">Leírás</span><span class="sxs-lookup"><span data-stu-id="4a399-179">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="4a399-180">1</span><span class="sxs-lookup"><span data-stu-id="4a399-180">1</span></span> |<span data-ttu-id="4a399-181">PCM 0</span><span class="sxs-lookup"><span data-stu-id="4a399-181">PCM 0</span></span> |
   | <span data-ttu-id="4a399-182">2</span><span class="sxs-lookup"><span data-stu-id="4a399-182">2</span></span> |<span data-ttu-id="4a399-183">PCM 1</span><span class="sxs-lookup"><span data-stu-id="4a399-183">PCM 1</span></span> |
   | <span data-ttu-id="4a399-184">3</span><span class="sxs-lookup"><span data-stu-id="4a399-184">3</span></span> |<span data-ttu-id="4a399-185">A vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="4a399-185">Controller 0</span></span> |
   | <span data-ttu-id="4a399-186">4</span><span class="sxs-lookup"><span data-stu-id="4a399-186">4</span></span> |<span data-ttu-id="4a399-187">1. vezérlő</span><span class="sxs-lookup"><span data-stu-id="4a399-187">Controller 1</span></span> |
5. <span data-ttu-id="4a399-188">Kapcsolja ki hibás PCM hello, vagy válassza le hello tápegység csatlakozója.</span><span class="sxs-lookup"><span data-stu-id="4a399-188">Turn off hello faulty PCM and disconnect hello power supply cord.</span></span> <span data-ttu-id="4a399-189">Most már eltávolíthatja hello PCM.</span><span class="sxs-lookup"><span data-stu-id="4a399-189">You can now remove hello PCM.</span></span>
6. <span data-ttu-id="4a399-190">Hello zárolás és PCM kezelni a mutatóujj között görgetőgomb hello hello oldalán megfogható, és együtt tooopen hello leíró nyomja össze.</span><span class="sxs-lookup"><span data-stu-id="4a399-190">Grasp hello latch and hello side of hello PCM handle between your thumb and forefinger, and squeeze them together tooopen hello handle.</span></span>
   
    ![Nyitó PCM leíró](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    <span data-ttu-id="4a399-192">**4. ábra** nyitó hello PCM kezelése</span><span class="sxs-lookup"><span data-stu-id="4a399-192">**Figure 4** Opening hello PCM handle</span></span>
7. <span data-ttu-id="4a399-193">Fogantyú hello hello PCM eltávolítása és kezelésére.</span><span class="sxs-lookup"><span data-stu-id="4a399-193">Grip hello handle and remove hello PCM.</span></span>
   
    ![PCM-eszközök eltávolítása](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    <span data-ttu-id="4a399-195">**5. ábra** eltávolításával hello PCM</span><span class="sxs-lookup"><span data-stu-id="4a399-195">**Figure 5** Removing hello PCM</span></span>

## <a name="install-a-replacement-pcm"></a><span data-ttu-id="4a399-196">A helyettesítő PCM telepítése</span><span class="sxs-lookup"><span data-stu-id="4a399-196">Install a replacement PCM</span></span>
<span data-ttu-id="4a399-197">Hajtsa végre az ezen utasításokat tooinstall egy PCM a StorSimple eszköz.</span><span class="sxs-lookup"><span data-stu-id="4a399-197">Follow these instructions tooinstall a PCM in your StorSimple device.</span></span> <span data-ttu-id="4a399-198">Győződjön meg arról, hogy beszúrt hello biztonsági mentési akkumulátor modul előzetes tooinstalling hello helyettesítő PCM (too764 W PCMs vonatkozik).</span><span class="sxs-lookup"><span data-stu-id="4a399-198">Ensure that you have inserted hello backup battery module prior tooinstalling hello replacement PCM (applies too764 W PCMs only).</span></span> <span data-ttu-id="4a399-199">További információkért lásd: hogyan túl[távolítsa el, majd szúrja be a biztonsági mentési akkumulátor modul](storsimple-8000-battery-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="4a399-199">For more information, see how too[remove and insert a backup battery module](storsimple-8000-battery-replacement.md).</span></span>

#### <a name="tooinstall-a-pcm"></a><span data-ttu-id="4a399-200">egy PCM tooinstall</span><span class="sxs-lookup"><span data-stu-id="4a399-200">tooinstall a PCM</span></span>
1. <span data-ttu-id="4a399-201">Győződjön meg arról, hogy rendelkezik-e hello megfelelő váltja fel PCM a ház.</span><span class="sxs-lookup"><span data-stu-id="4a399-201">Verify that you have hello correct replacement PCM for this enclosure.</span></span> <span data-ttu-id="4a399-202">hello elsődleges ház kell egy 764 W PCM és hello EBOD ház kell egy 580 W PCM.</span><span class="sxs-lookup"><span data-stu-id="4a399-202">hello primary enclosure needs a 764 W PCM and hello EBOD enclosure needs a 580 W PCM.</span></span> <span data-ttu-id="4a399-203">Ne próbáljon toouse 580 W PCM hello elsődleges szolgáltatással hello, vagy a hello EBOD ház 764 W PCM hello.</span><span class="sxs-lookup"><span data-stu-id="4a399-203">You should not attempt toouse hello 580 W PCM in hello Primary enclosure, or hello 764 W PCM in hello EBOD enclosure.</span></span> <span data-ttu-id="4a399-204">a következő kép hello jeleníti meg, ahol ezt az információt a hello címke, amely tooidentify elhelyezni toohello PCM.</span><span class="sxs-lookup"><span data-stu-id="4a399-204">hello following image shows where tooidentify this information on hello label that is affixed toohello PCM.</span></span>
   
    ![Eszköz PCM címke](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    <span data-ttu-id="4a399-206">**6. ábra** PCM címke</span><span class="sxs-lookup"><span data-stu-id="4a399-206">**Figure 6** PCM label</span></span>
2. <span data-ttu-id="4a399-207">Ellenőrizze, hogy a sérülés toohello ház, különös tekintettel toohello összekötők.</span><span class="sxs-lookup"><span data-stu-id="4a399-207">Check for damage toohello enclosure, paying particular attention toohello connectors.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="4a399-208">**Ha bármely összekötő elgörbülve ne telepítse hello modul.**</span><span class="sxs-lookup"><span data-stu-id="4a399-208">**Do not install hello module if any connector pins are bent.**</span></span>
   > 
   > 
3. <span data-ttu-id="4a399-209">Hello PCM hello kezelni a nyissa meg a pozíció, hello ház hello modult menüpontban húzással állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="4a399-209">With hello PCM handle in hello open position, slide hello module into hello enclosure.</span></span>
   
    ![PCM eszköz telepítése](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    <span data-ttu-id="4a399-211">**7. ábra** telepítése hello PCM</span><span class="sxs-lookup"><span data-stu-id="4a399-211">**Figure 7** Installing hello PCM</span></span>
4. <span data-ttu-id="4a399-212">Zárja be kézzel a hello PCM leíró.</span><span class="sxs-lookup"><span data-stu-id="4a399-212">Manually close hello PCM handle.</span></span> <span data-ttu-id="4a399-213">Egy kattintással kell hall, mivel hello leíró zárolás kapcsolatba lép.</span><span class="sxs-lookup"><span data-stu-id="4a399-213">You should hear a click as hello handle latch engages.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4a399-214">amely összekötő PIN-kódok hello tooensure végző rendelkezik, akkor is óvatosan tug hello leírón hello zárolás feloldása nélkül.</span><span class="sxs-lookup"><span data-stu-id="4a399-214">tooensure that hello connector pins have engaged, you can gently tug on hello handle without releasing hello latch.</span></span> <span data-ttu-id="4a399-215">Hello PCM diák ki, ha ez arra utalhat, hogy hello zárolás előtt hello összekötők részt vevő be lett zárva.</span><span class="sxs-lookup"><span data-stu-id="4a399-215">If hello PCM slides out, it implies that hello latch was closed before hello connectors engaged.</span></span>
   
5. <span data-ttu-id="4a399-216">Csatlakozás hello power kábelek toohello áramforrásról és közös toohello PCM.</span><span class="sxs-lookup"><span data-stu-id="4a399-216">Connect hello power cables toohello power source and toohello PCM.</span></span>
6. <span data-ttu-id="4a399-217">Biztonságos hello törzs mentesség bálákban.</span><span class="sxs-lookup"><span data-stu-id="4a399-217">Secure hello strain relief bales.</span></span>
7. <span data-ttu-id="4a399-218">Hello PCM bekapcsolása.</span><span class="sxs-lookup"><span data-stu-id="4a399-218">Turn on hello PCM.</span></span>
8. <span data-ttu-id="4a399-219">Győződjön meg arról, hogy sikeres volt-e helyettesítő hello: hello Azure-portálon a StorSimple Device Manager szolgáltatást, lépjen a tooyour eszközt, majd túl**beállítások > figyelő > hardver állapotának**.</span><span class="sxs-lookup"><span data-stu-id="4a399-219">Verify that hello replacement was successful: in hello Azure portal of your StorSimple Device Manager service, navigate tooyour device and then too**Settings > Monitor > Hardware health**.</span></span> <span data-ttu-id="4a399-220">A hello **összetevők megosztott**, hello PCM hello állapota zöld kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4a399-220">Under hello **Shared components**, hello status of hello PCM should be green.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4a399-221">Hello helyettesítő PCM toocompletely inicializálása néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="4a399-221">It may take a few minutes for hello replacement PCM toocompletely initialize.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a399-222">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4a399-222">Next steps</span></span>
<span data-ttu-id="4a399-223">További információ [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="4a399-223">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

