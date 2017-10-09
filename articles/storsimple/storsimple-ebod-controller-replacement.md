---
title: "a StorSimple EBOD vezérlő aaaReplace |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooremove, és cserélje le a StorSimple 8600 eszközön legalább az egyik EBOD tartományvezérlők."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 8cbfa507-1a56-4e24-99dd-7db9abd3b850
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 5d29de2ee30bfdd70910050eee5cfa1d293d444f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a><span data-ttu-id="f6158-103">Cserélje le az EBOD vezérlőhöz a StorSimple eszköz</span><span class="sxs-lookup"><span data-stu-id="f6158-103">Replace an EBOD controller on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="f6158-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f6158-104">Overview</span></span>
<span data-ttu-id="f6158-105">Ez az oktatóanyag azt ismerteti, hogyan tooreplace egy hibás EBOD vezérlő modul a Microsoft Azure StorSimple eszközön.</span><span class="sxs-lookup"><span data-stu-id="f6158-105">This tutorial explains how tooreplace a faulty EBOD controller module on your Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="f6158-106">egy EBOD vezérlő modul tooreplace, kell:</span><span class="sxs-lookup"><span data-stu-id="f6158-106">tooreplace an EBOD controller module, you need to:</span></span>

* <span data-ttu-id="f6158-107">Hello hibás EBOD tartományvezérlő eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f6158-107">Remove hello faulty EBOD controller</span></span>
* <span data-ttu-id="f6158-108">Egy új EBOD controller telepítése</span><span class="sxs-lookup"><span data-stu-id="f6158-108">Install a new EBOD controller</span></span>

<span data-ttu-id="f6158-109">Vegye figyelembe a következő információ megkezdése előtt hello:</span><span class="sxs-lookup"><span data-stu-id="f6158-109">Consider hello following information before you begin:</span></span>

* <span data-ttu-id="f6158-110">Üres EBOD modulok minden nem használt tárolóhelyek kell beszúrni.</span><span class="sxs-lookup"><span data-stu-id="f6158-110">Blank EBOD modules must be inserted into all unused slots.</span></span> <span data-ttu-id="f6158-111">hello ház nem cool megfelelően, ha a tárhely marad nyitva.</span><span class="sxs-lookup"><span data-stu-id="f6158-111">hello enclosure will not cool properly if a slot is left open.</span></span>
* <span data-ttu-id="f6158-112">hello EBOD tartományvezérlő közbeni-cserélhető és eltávolítani vagy lecserélni.</span><span class="sxs-lookup"><span data-stu-id="f6158-112">hello EBOD controller is hot-swappable and can be removed or replaced.</span></span> <span data-ttu-id="f6158-113">Ne távolítsa el egy hibás modul, amíg nem helyettesíti.</span><span class="sxs-lookup"><span data-stu-id="f6158-113">Do not remove a failed module until you have a replacement.</span></span> <span data-ttu-id="f6158-114">Hello cseréjét kezdeményezésekor 10 percen belül kell befejezése.</span><span class="sxs-lookup"><span data-stu-id="f6158-114">When you initiate hello replacement process, you must finish it within 10 minutes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f6158-115">Tooremove megkísérlése előtt vagy StorSimple összetevők közül bármelyik lecseréléséhez győződjön meg arról, hogy tekintse át a hello [biztonsági ikon egyezmények](storsimple-safety.md#safety-icon-conventions) és egyéb [biztonsági óvintézkedéseket](storsimple-safety.md).</span><span class="sxs-lookup"><span data-stu-id="f6158-115">Before attempting tooremove or replace any StorSimple component, make sure that you review hello [safety icon conventions](storsimple-safety.md#safety-icon-conventions) and other [safety precautions](storsimple-safety.md).</span></span>
> 
> 

## <a name="remove-an-ebod-controller"></a><span data-ttu-id="f6158-116">Távolítsa el az EBOD vezérlőhöz</span><span class="sxs-lookup"><span data-stu-id="f6158-116">Remove an EBOD controller</span></span>
<span data-ttu-id="f6158-117">Nem sikerült a StorSimple eszköz EBOD vezérlő moduljának hello behelyettesíteni, előtt győződjön meg arról, hogy hello más EBOD vezérlő modul az aktív és futó.</span><span class="sxs-lookup"><span data-stu-id="f6158-117">Before replacing hello failed EBOD controller module in your StorSimple device, make sure that hello other EBOD controller module is active and running.</span></span> <span data-ttu-id="f6158-118">hello alábbi eljárást és táblázatot azt ismertetik, hogyan tooremove hello EBOD vezérlő modul.</span><span class="sxs-lookup"><span data-stu-id="f6158-118">hello following procedure and table explain how tooremove hello EBOD controller module.</span></span>

#### <a name="tooremove-an-ebod-module"></a><span data-ttu-id="f6158-119">tooremove EBOD modul létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6158-119">tooremove an EBOD module</span></span>
1. <span data-ttu-id="f6158-120">Nyissa meg hello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f6158-120">Open hello Azure classic portal.</span></span>
2. <span data-ttu-id="f6158-121">Keresse meg a túl**eszközök** > **karbantartási** > **hardverállapot**, és győződjön meg arról, hogy hello hello állapotának a kereslet az olyan aktív EBOD hello a tartományvezérlő modul zöld pedig hello LED sikertelen hello EBOD vezérlő modul piros.</span><span class="sxs-lookup"><span data-stu-id="f6158-121">Navigate too**Devices** > **Maintenance** > **Hardware Status**, and verify that hello status of hello LED for hello active EBOD controller module is green and hello LED for hello failed EBOD controller module is red.</span></span>
3. <span data-ttu-id="f6158-122">Keresse meg a sikertelen hello EBOD vezérlő modul: hello hello eszköz oldalán.</span><span class="sxs-lookup"><span data-stu-id="f6158-122">Locate hello failed EBOD controller module at hello back of hello device.</span></span>
4. <span data-ttu-id="f6158-123">Távolítsa el a hello kábelek, mielőtt kilépteti a hello EBOD modul hello rendszerből hello EBOD vezérlő modul toohello vezérlő csatlakozó.</span><span class="sxs-lookup"><span data-stu-id="f6158-123">Remove hello cables that connect hello EBOD controller module toohello controller before taking hello EBOD module out of hello system.</span></span>
5. <span data-ttu-id="f6158-124">Jegyezze fel a pontos SAS port hello hello EBOD vezérlő modul, de a csatlakoztatott toohello vezérlő.</span><span class="sxs-lookup"><span data-stu-id="f6158-124">Make a note of hello exact SAS port of hello EBOD controller module that was connected toohello controller.</span></span> <span data-ttu-id="f6158-125">Szükséges toorestore hello rendszerkonfiguráció toothis hello EBOD modul cseréje után lesz.</span><span class="sxs-lookup"><span data-stu-id="f6158-125">You will be required toorestore hello system toothis configuration after you replace hello EBOD module.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="f6158-126">Általában ez lesz a Port, amely van címkézve **működteti** a következő diagram hello.</span><span class="sxs-lookup"><span data-stu-id="f6158-126">Typically, this will be Port A, which is labeled as **Host in** in hello following diagram.</span></span>
   > 
   > 
   
    ![Csatlakozópanel a EBOD vezérlő](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     <span data-ttu-id="f6158-128">**1. ábra** vissza a EBOD modul</span><span class="sxs-lookup"><span data-stu-id="f6158-128">**Figure 1** Back of EBOD module</span></span>
   
   | <span data-ttu-id="f6158-129">Címke</span><span class="sxs-lookup"><span data-stu-id="f6158-129">Label</span></span> | <span data-ttu-id="f6158-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="f6158-130">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="f6158-131">1</span><span class="sxs-lookup"><span data-stu-id="f6158-131">1</span></span> |<span data-ttu-id="f6158-132">Tartalék LED</span><span class="sxs-lookup"><span data-stu-id="f6158-132">Fault LED</span></span> |
   | <span data-ttu-id="f6158-133">2</span><span class="sxs-lookup"><span data-stu-id="f6158-133">2</span></span> |<span data-ttu-id="f6158-134">Energiagazdálkodási LED</span><span class="sxs-lookup"><span data-stu-id="f6158-134">Power LED</span></span> |
   | <span data-ttu-id="f6158-135">3</span><span class="sxs-lookup"><span data-stu-id="f6158-135">3</span></span> |<span data-ttu-id="f6158-136">SAS-összekötők</span><span class="sxs-lookup"><span data-stu-id="f6158-136">SAS connectors</span></span> |
   | <span data-ttu-id="f6158-137">4</span><span class="sxs-lookup"><span data-stu-id="f6158-137">4</span></span> |<span data-ttu-id="f6158-138">SAS LED</span><span class="sxs-lookup"><span data-stu-id="f6158-138">SAS LEDs</span></span> |
   | <span data-ttu-id="f6158-139">5</span><span class="sxs-lookup"><span data-stu-id="f6158-139">5</span></span> |<span data-ttu-id="f6158-140">Csak a gyári használatra soros portokhoz</span><span class="sxs-lookup"><span data-stu-id="f6158-140">Serial ports for factory use only</span></span> |
   | <span data-ttu-id="f6158-141">6</span><span class="sxs-lookup"><span data-stu-id="f6158-141">6</span></span> |<span data-ttu-id="f6158-142">Port (a gazdagép)</span><span class="sxs-lookup"><span data-stu-id="f6158-142">Port A (Host in)</span></span> |
   | <span data-ttu-id="f6158-143">7</span><span class="sxs-lookup"><span data-stu-id="f6158-143">7</span></span> |<span data-ttu-id="f6158-144">B port (gazdagép kimenő)</span><span class="sxs-lookup"><span data-stu-id="f6158-144">Port B (Host out)</span></span> |
   | <span data-ttu-id="f6158-145">8</span><span class="sxs-lookup"><span data-stu-id="f6158-145">8</span></span> |<span data-ttu-id="f6158-146">Port C (csak a gyári általi használatra)</span><span class="sxs-lookup"><span data-stu-id="f6158-146">Port C (Factory use only)</span></span> |

## <a name="install-a-new-ebod-controller"></a><span data-ttu-id="f6158-147">Egy új EBOD controller telepítése</span><span class="sxs-lookup"><span data-stu-id="f6158-147">Install a new EBOD controller</span></span>
<span data-ttu-id="f6158-148">hello alábbi eljárást és táblázatot azt ismertetik, hogyan tooinstall egy EBOD vezérlő modulja a StorSimple eszközt.</span><span class="sxs-lookup"><span data-stu-id="f6158-148">hello following procedure and table explain how tooinstall an EBOD controller module in your StorSimple device.</span></span>

#### <a name="tooinstall-an-ebod-controller"></a><span data-ttu-id="f6158-149">az EBOD vezérlőhöz tooinstall</span><span class="sxs-lookup"><span data-stu-id="f6158-149">tooinstall an EBOD controller</span></span>
1. <span data-ttu-id="f6158-150">Ellenőrizze a hello EBOD eszköz sérülés, különösen a toohello felület összekötő.</span><span class="sxs-lookup"><span data-stu-id="f6158-150">Check hello EBOD device for damage, especially toohello interface connector.</span></span> <span data-ttu-id="f6158-151">Ne telepítse hello új EBOD vezérlő, ha bármely elgörbülve.</span><span class="sxs-lookup"><span data-stu-id="f6158-151">Do not install hello new EBOD controller if any pins are bent.</span></span>
2. <span data-ttu-id="f6158-152">A hello zárolás van életben a hello nyissa meg a pozíció, amíg hello zárolás van életben megszólítása hello ház hello modult menüpontban húzással állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="f6158-152">With hello latches in hello open position, slide hello module into hello enclosure until hello latches engage.</span></span>
   
    ![EBOD controller telepítése](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    <span data-ttu-id="f6158-154">**2. ábra** telepítése hello EBOD vezérlő modul</span><span class="sxs-lookup"><span data-stu-id="f6158-154">**Figure 2**  Installing hello EBOD controller module</span></span>
3. <span data-ttu-id="f6158-155">Bezárás hello zárolás.</span><span class="sxs-lookup"><span data-stu-id="f6158-155">Close hello latch.</span></span> <span data-ttu-id="f6158-156">Egy kattintással kell hall, mivel hello zárolás kapcsolatba lép.</span><span class="sxs-lookup"><span data-stu-id="f6158-156">You should hear a click as hello latch engages.</span></span>
   
    ![EBOD zárolás feloldása](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    <span data-ttu-id="f6158-158">**3. ábra** hello EBOD modul zárolás bezárása</span><span class="sxs-lookup"><span data-stu-id="f6158-158">**Figure 3**  Closing hello EBOD module latch</span></span>
4. <span data-ttu-id="f6158-159">Csatlakozzon újra hello kábelek.</span><span class="sxs-lookup"><span data-stu-id="f6158-159">Reconnect hello cables.</span></span> <span data-ttu-id="f6158-160">A hello pontos konfigurációja hello cseréje előtt voltak.</span><span class="sxs-lookup"><span data-stu-id="f6158-160">Use hello exact configuration that was present before hello replacement.</span></span> <span data-ttu-id="f6158-161">Tekintse meg a következő diagram hello és részletei tábla tooconnect hello kábelek.</span><span class="sxs-lookup"><span data-stu-id="f6158-161">See hello following diagram and table for details about how tooconnect hello cables.</span></span>
   
    ![Az energiagazdálkodási 4U eszköz bekábelezése](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    <span data-ttu-id="f6158-163">**4. ábra**.</span><span class="sxs-lookup"><span data-stu-id="f6158-163">**Figure 4**.</span></span> <span data-ttu-id="f6158-164">Újracsatlakozás a kábelek.</span><span class="sxs-lookup"><span data-stu-id="f6158-164">Reconnecting cables</span></span>
   
   | <span data-ttu-id="f6158-165">Címke</span><span class="sxs-lookup"><span data-stu-id="f6158-165">Label</span></span> | <span data-ttu-id="f6158-166">Leírás</span><span class="sxs-lookup"><span data-stu-id="f6158-166">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="f6158-167">1</span><span class="sxs-lookup"><span data-stu-id="f6158-167">1</span></span> |<span data-ttu-id="f6158-168">Elsődleges ház</span><span class="sxs-lookup"><span data-stu-id="f6158-168">Primary enclosure</span></span> |
   | <span data-ttu-id="f6158-169">2</span><span class="sxs-lookup"><span data-stu-id="f6158-169">2</span></span> |<span data-ttu-id="f6158-170">PCM 0</span><span class="sxs-lookup"><span data-stu-id="f6158-170">PCM 0</span></span> |
   | <span data-ttu-id="f6158-171">3</span><span class="sxs-lookup"><span data-stu-id="f6158-171">3</span></span> |<span data-ttu-id="f6158-172">PCM 1</span><span class="sxs-lookup"><span data-stu-id="f6158-172">PCM 1</span></span> |
   | <span data-ttu-id="f6158-173">4</span><span class="sxs-lookup"><span data-stu-id="f6158-173">4</span></span> |<span data-ttu-id="f6158-174">A vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="f6158-174">Controller 0</span></span> |
   | <span data-ttu-id="f6158-175">5</span><span class="sxs-lookup"><span data-stu-id="f6158-175">5</span></span> |<span data-ttu-id="f6158-176">1. vezérlő</span><span class="sxs-lookup"><span data-stu-id="f6158-176">Controller 1</span></span> |
   | <span data-ttu-id="f6158-177">6</span><span class="sxs-lookup"><span data-stu-id="f6158-177">6</span></span> |<span data-ttu-id="f6158-178">EBOD vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="f6158-178">EBOD controller 0</span></span> |
   | <span data-ttu-id="f6158-179">7</span><span class="sxs-lookup"><span data-stu-id="f6158-179">7</span></span> |<span data-ttu-id="f6158-180">1. EBOD vezérlő</span><span class="sxs-lookup"><span data-stu-id="f6158-180">EBOD controller 1</span></span> |
   | <span data-ttu-id="f6158-181">8</span><span class="sxs-lookup"><span data-stu-id="f6158-181">8</span></span> |<span data-ttu-id="f6158-182">EBOD ház</span><span class="sxs-lookup"><span data-stu-id="f6158-182">EBOD enclosure</span></span> |
   | <span data-ttu-id="f6158-183">9</span><span class="sxs-lookup"><span data-stu-id="f6158-183">9</span></span> |<span data-ttu-id="f6158-184">Áramelosztó egységekből</span><span class="sxs-lookup"><span data-stu-id="f6158-184">Power Distribution Units</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f6158-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6158-185">Next steps</span></span>
<span data-ttu-id="f6158-186">További információ [StorSimple hardver összetevő cseréje](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="f6158-186">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

