---
title: "a StorSimple 8600 EBOD vezérlő aaaReplace |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooremove, és cserélje le a StorSimple 8600 eszközön legalább az egyik EBOD tartományvezérlők."
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
ms.openlocfilehash: 8343ed6f48ae97fc9204452f85e1936bfb1d6919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a><span data-ttu-id="4b55c-103">Cserélje le az EBOD vezérlőhöz a StorSimple eszköz</span><span class="sxs-lookup"><span data-stu-id="4b55c-103">Replace an EBOD controller on your StorSimple device</span></span>

## <a name="overview"></a><span data-ttu-id="4b55c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4b55c-104">Overview</span></span>
<span data-ttu-id="4b55c-105">Ez az oktatóanyag azt ismerteti, hogyan tooreplace egy hibás EBOD vezérlő modul a Microsoft Azure StorSimple eszközön.</span><span class="sxs-lookup"><span data-stu-id="4b55c-105">This tutorial explains how tooreplace a faulty EBOD controller module on your Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="4b55c-106">egy EBOD vezérlő modul tooreplace, kell:</span><span class="sxs-lookup"><span data-stu-id="4b55c-106">tooreplace an EBOD controller module, you need to:</span></span>

* <span data-ttu-id="4b55c-107">Hello hibás EBOD tartományvezérlő eltávolítása</span><span class="sxs-lookup"><span data-stu-id="4b55c-107">Remove hello faulty EBOD controller</span></span>
* <span data-ttu-id="4b55c-108">Egy új EBOD controller telepítése</span><span class="sxs-lookup"><span data-stu-id="4b55c-108">Install a new EBOD controller</span></span>

<span data-ttu-id="4b55c-109">Vegye figyelembe a következő információ megkezdése előtt hello:</span><span class="sxs-lookup"><span data-stu-id="4b55c-109">Consider hello following information before you begin:</span></span>

* <span data-ttu-id="4b55c-110">Üres EBOD modulok minden nem használt tárolóhelyek kell beszúrni.</span><span class="sxs-lookup"><span data-stu-id="4b55c-110">Blank EBOD modules must be inserted into all unused slots.</span></span> <span data-ttu-id="4b55c-111">hello ház nem cool megfelelően, ha a tárhely marad nyitva.</span><span class="sxs-lookup"><span data-stu-id="4b55c-111">hello enclosure will not cool properly if a slot is left open.</span></span>
* <span data-ttu-id="4b55c-112">hello EBOD tartományvezérlő közbeni-cserélhető és eltávolítani vagy lecserélni.</span><span class="sxs-lookup"><span data-stu-id="4b55c-112">hello EBOD controller is hot-swappable and can be removed or replaced.</span></span> <span data-ttu-id="4b55c-113">Ne távolítsa el egy hibás modul, amíg nem helyettesíti.</span><span class="sxs-lookup"><span data-stu-id="4b55c-113">Do not remove a failed module until you have a replacement.</span></span> <span data-ttu-id="4b55c-114">Hello cseréjét kezdeményezésekor 10 percen belül kell befejezése.</span><span class="sxs-lookup"><span data-stu-id="4b55c-114">When you initiate hello replacement process, you must finish it within 10 minutes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4b55c-115">Tooremove megkísérlése előtt vagy StorSimple összetevők közül bármelyik lecseréléséhez győződjön meg arról, hogy tekintse át a hello [biztonsági ikon egyezmények](storsimple-safety.md#safety-icon-conventions) és egyéb [biztonsági óvintézkedéseket](storsimple-safety.md).</span><span class="sxs-lookup"><span data-stu-id="4b55c-115">Before attempting tooremove or replace any StorSimple component, make sure that you review hello [safety icon conventions](storsimple-safety.md#safety-icon-conventions) and other [safety precautions](storsimple-safety.md).</span></span>

## <a name="remove-an-ebod-controller"></a><span data-ttu-id="4b55c-116">Távolítsa el az EBOD vezérlőhöz</span><span class="sxs-lookup"><span data-stu-id="4b55c-116">Remove an EBOD controller</span></span>
<span data-ttu-id="4b55c-117">Nem sikerült a StorSimple eszköz EBOD vezérlő moduljának hello behelyettesíteni, előtt győződjön meg arról, hogy hello más EBOD vezérlő modul az aktív és futó.</span><span class="sxs-lookup"><span data-stu-id="4b55c-117">Before replacing hello failed EBOD controller module in your StorSimple device, make sure that hello other EBOD controller module is active and running.</span></span> <span data-ttu-id="4b55c-118">hello alábbi eljárást és táblázatot azt ismertetik, hogyan tooremove hello EBOD vezérlő modul.</span><span class="sxs-lookup"><span data-stu-id="4b55c-118">hello following procedure and table explain how tooremove hello EBOD controller module.</span></span>

#### <a name="tooremove-an-ebod-module"></a><span data-ttu-id="4b55c-119">tooremove EBOD modul létrehozása</span><span class="sxs-lookup"><span data-stu-id="4b55c-119">tooremove an EBOD module</span></span>
1. <span data-ttu-id="4b55c-120">Nyissa meg hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="4b55c-120">Open hello Azure portal.</span></span>
2. <span data-ttu-id="4b55c-121">Nyissa meg tooyour eszközt, és keresse meg a túl**beállítások** > **hardver állapotának**, és ellenőrizze a hello LED hello állapotának hello aktív EBOD vezérlő modul zöld és hello LED hello nem sikerült az érték EBOD vezérlő modul az piros.</span><span class="sxs-lookup"><span data-stu-id="4b55c-121">Go tooyour device and navigate too**Settings** > **Hardware health**, and verify that hello status of hello LED for hello active EBOD controller module is green and hello LED for hello failed EBOD controller module is red.</span></span>
3. <span data-ttu-id="4b55c-122">Keresse meg a sikertelen hello EBOD vezérlő modul: hello hello eszköz oldalán.</span><span class="sxs-lookup"><span data-stu-id="4b55c-122">Locate hello failed EBOD controller module at hello back of hello device.</span></span>
4. <span data-ttu-id="4b55c-123">Távolítsa el a hello kábelek, mielőtt kilépteti a hello EBOD modul hello rendszerből hello EBOD vezérlő modul toohello vezérlő csatlakozó.</span><span class="sxs-lookup"><span data-stu-id="4b55c-123">Remove hello cables that connect hello EBOD controller module toohello controller before taking hello EBOD module out of hello system.</span></span>
5. <span data-ttu-id="4b55c-124">Jegyezze fel a pontos SAS port hello hello EBOD vezérlő modul, de a csatlakoztatott toohello vezérlő.</span><span class="sxs-lookup"><span data-stu-id="4b55c-124">Make a note of hello exact SAS port of hello EBOD controller module that was connected toohello controller.</span></span> <span data-ttu-id="4b55c-125">Szükséges toorestore hello rendszerkonfiguráció toothis hello EBOD modul cseréje után lesz.</span><span class="sxs-lookup"><span data-stu-id="4b55c-125">You will be required toorestore hello system toothis configuration after you replace hello EBOD module.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4b55c-126">Általában ez lesz a Port, amely van címkézve **működteti** a következő diagram hello.</span><span class="sxs-lookup"><span data-stu-id="4b55c-126">Typically, this will be Port A, which is labeled as **Host in** in hello following diagram.</span></span>
   
    ![Csatlakozópanel a EBOD vezérlő](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     <span data-ttu-id="4b55c-128">**1. ábra** vissza a EBOD modul</span><span class="sxs-lookup"><span data-stu-id="4b55c-128">**Figure 1** Back of EBOD module</span></span>
   
   | <span data-ttu-id="4b55c-129">Címke</span><span class="sxs-lookup"><span data-stu-id="4b55c-129">Label</span></span> | <span data-ttu-id="4b55c-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="4b55c-130">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="4b55c-131">1</span><span class="sxs-lookup"><span data-stu-id="4b55c-131">1</span></span> |<span data-ttu-id="4b55c-132">Tartalék LED</span><span class="sxs-lookup"><span data-stu-id="4b55c-132">Fault LED</span></span> |
   | <span data-ttu-id="4b55c-133">2</span><span class="sxs-lookup"><span data-stu-id="4b55c-133">2</span></span> |<span data-ttu-id="4b55c-134">Energiagazdálkodási LED</span><span class="sxs-lookup"><span data-stu-id="4b55c-134">Power LED</span></span> |
   | <span data-ttu-id="4b55c-135">3</span><span class="sxs-lookup"><span data-stu-id="4b55c-135">3</span></span> |<span data-ttu-id="4b55c-136">SAS-összekötők</span><span class="sxs-lookup"><span data-stu-id="4b55c-136">SAS connectors</span></span> |
   | <span data-ttu-id="4b55c-137">4</span><span class="sxs-lookup"><span data-stu-id="4b55c-137">4</span></span> |<span data-ttu-id="4b55c-138">SAS LED</span><span class="sxs-lookup"><span data-stu-id="4b55c-138">SAS LEDs</span></span> |
   | <span data-ttu-id="4b55c-139">5</span><span class="sxs-lookup"><span data-stu-id="4b55c-139">5</span></span> |<span data-ttu-id="4b55c-140">Csak a gyári használatra soros portokhoz</span><span class="sxs-lookup"><span data-stu-id="4b55c-140">Serial ports for factory use only</span></span> |
   | <span data-ttu-id="4b55c-141">6</span><span class="sxs-lookup"><span data-stu-id="4b55c-141">6</span></span> |<span data-ttu-id="4b55c-142">Port (a gazdagép)</span><span class="sxs-lookup"><span data-stu-id="4b55c-142">Port A (Host in)</span></span> |
   | <span data-ttu-id="4b55c-143">7</span><span class="sxs-lookup"><span data-stu-id="4b55c-143">7</span></span> |<span data-ttu-id="4b55c-144">B port (gazdagép kimenő)</span><span class="sxs-lookup"><span data-stu-id="4b55c-144">Port B (Host out)</span></span> |
   | <span data-ttu-id="4b55c-145">8</span><span class="sxs-lookup"><span data-stu-id="4b55c-145">8</span></span> |<span data-ttu-id="4b55c-146">Port C (csak a gyári általi használatra)</span><span class="sxs-lookup"><span data-stu-id="4b55c-146">Port C (Factory use only)</span></span> |

## <a name="install-a-new-ebod-controller"></a><span data-ttu-id="4b55c-147">Egy új EBOD controller telepítése</span><span class="sxs-lookup"><span data-stu-id="4b55c-147">Install a new EBOD controller</span></span>
<span data-ttu-id="4b55c-148">hello alábbi eljárást és táblázatot azt ismertetik, hogyan tooinstall egy EBOD vezérlő modulja a StorSimple eszközt.</span><span class="sxs-lookup"><span data-stu-id="4b55c-148">hello following procedure and table explain how tooinstall an EBOD controller module in your StorSimple device.</span></span>

#### <a name="tooinstall-an-ebod-controller"></a><span data-ttu-id="4b55c-149">az EBOD vezérlőhöz tooinstall</span><span class="sxs-lookup"><span data-stu-id="4b55c-149">tooinstall an EBOD controller</span></span>
1. <span data-ttu-id="4b55c-150">Ellenőrizze a hello EBOD eszköz sérülés, különösen a toohello felület összekötő.</span><span class="sxs-lookup"><span data-stu-id="4b55c-150">Check hello EBOD device for damage, especially toohello interface connector.</span></span> <span data-ttu-id="4b55c-151">Ne telepítse hello új EBOD vezérlő, ha bármely elgörbülve.</span><span class="sxs-lookup"><span data-stu-id="4b55c-151">Do not install hello new EBOD controller if any pins are bent.</span></span>
2. <span data-ttu-id="4b55c-152">A hello zárolás van életben a hello nyissa meg a pozíció, amíg hello zárolás van életben megszólítása hello ház hello modult menüpontban húzással állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="4b55c-152">With hello latches in hello open position, slide hello module into hello enclosure until hello latches engage.</span></span>
   
    ![EBOD controller telepítése](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    <span data-ttu-id="4b55c-154">**2. ábra** telepítése hello EBOD vezérlő modul</span><span class="sxs-lookup"><span data-stu-id="4b55c-154">**Figure 2**  Installing hello EBOD controller module</span></span>
3. <span data-ttu-id="4b55c-155">Bezárás hello zárolás.</span><span class="sxs-lookup"><span data-stu-id="4b55c-155">Close hello latch.</span></span> <span data-ttu-id="4b55c-156">Egy kattintással kell hall, mivel hello zárolás kapcsolatba lép.</span><span class="sxs-lookup"><span data-stu-id="4b55c-156">You should hear a click as hello latch engages.</span></span>
   
    ![EBOD zárolás feloldása](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    <span data-ttu-id="4b55c-158">**3. ábra** hello EBOD modul zárolás bezárása</span><span class="sxs-lookup"><span data-stu-id="4b55c-158">**Figure 3**  Closing hello EBOD module latch</span></span>
4. <span data-ttu-id="4b55c-159">Csatlakozzon újra hello kábelek.</span><span class="sxs-lookup"><span data-stu-id="4b55c-159">Reconnect hello cables.</span></span> <span data-ttu-id="4b55c-160">A hello pontos konfigurációja hello cseréje előtt voltak.</span><span class="sxs-lookup"><span data-stu-id="4b55c-160">Use hello exact configuration that was present before hello replacement.</span></span> <span data-ttu-id="4b55c-161">Tekintse meg a következő diagram hello és részletei tábla tooconnect hello kábelek.</span><span class="sxs-lookup"><span data-stu-id="4b55c-161">See hello following diagram and table for details about how tooconnect hello cables.</span></span>
   
    ![Az energiagazdálkodási 4U eszköz bekábelezése](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    <span data-ttu-id="4b55c-163">**4. ábra**.</span><span class="sxs-lookup"><span data-stu-id="4b55c-163">**Figure 4**.</span></span> <span data-ttu-id="4b55c-164">Újracsatlakozás a kábelek.</span><span class="sxs-lookup"><span data-stu-id="4b55c-164">Reconnecting cables</span></span>
   
   | <span data-ttu-id="4b55c-165">Címke</span><span class="sxs-lookup"><span data-stu-id="4b55c-165">Label</span></span> | <span data-ttu-id="4b55c-166">Leírás</span><span class="sxs-lookup"><span data-stu-id="4b55c-166">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="4b55c-167">1</span><span class="sxs-lookup"><span data-stu-id="4b55c-167">1</span></span> |<span data-ttu-id="4b55c-168">Elsődleges ház</span><span class="sxs-lookup"><span data-stu-id="4b55c-168">Primary enclosure</span></span> |
   | <span data-ttu-id="4b55c-169">2</span><span class="sxs-lookup"><span data-stu-id="4b55c-169">2</span></span> |<span data-ttu-id="4b55c-170">PCM 0</span><span class="sxs-lookup"><span data-stu-id="4b55c-170">PCM 0</span></span> |
   | <span data-ttu-id="4b55c-171">3</span><span class="sxs-lookup"><span data-stu-id="4b55c-171">3</span></span> |<span data-ttu-id="4b55c-172">PCM 1</span><span class="sxs-lookup"><span data-stu-id="4b55c-172">PCM 1</span></span> |
   | <span data-ttu-id="4b55c-173">4</span><span class="sxs-lookup"><span data-stu-id="4b55c-173">4</span></span> |<span data-ttu-id="4b55c-174">A vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="4b55c-174">Controller 0</span></span> |
   | <span data-ttu-id="4b55c-175">5</span><span class="sxs-lookup"><span data-stu-id="4b55c-175">5</span></span> |<span data-ttu-id="4b55c-176">1. vezérlő</span><span class="sxs-lookup"><span data-stu-id="4b55c-176">Controller 1</span></span> |
   | <span data-ttu-id="4b55c-177">6</span><span class="sxs-lookup"><span data-stu-id="4b55c-177">6</span></span> |<span data-ttu-id="4b55c-178">EBOD vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="4b55c-178">EBOD controller 0</span></span> |
   | <span data-ttu-id="4b55c-179">7</span><span class="sxs-lookup"><span data-stu-id="4b55c-179">7</span></span> |<span data-ttu-id="4b55c-180">1. EBOD vezérlő</span><span class="sxs-lookup"><span data-stu-id="4b55c-180">EBOD controller 1</span></span> |
   | <span data-ttu-id="4b55c-181">8</span><span class="sxs-lookup"><span data-stu-id="4b55c-181">8</span></span> |<span data-ttu-id="4b55c-182">EBOD ház</span><span class="sxs-lookup"><span data-stu-id="4b55c-182">EBOD enclosure</span></span> |
   | <span data-ttu-id="4b55c-183">9</span><span class="sxs-lookup"><span data-stu-id="4b55c-183">9</span></span> |<span data-ttu-id="4b55c-184">Áramelosztó egységekből</span><span class="sxs-lookup"><span data-stu-id="4b55c-184">Power Distribution Units</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4b55c-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4b55c-185">Next steps</span></span>
<span data-ttu-id="4b55c-186">További információ [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="4b55c-186">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

