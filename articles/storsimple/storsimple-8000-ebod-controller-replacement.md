---
title: "Cserélje le a StorSimple 8600 EBOD vezérlő |} Microsoft Docs"
description: "Távolítsa el, és cserélje le a StorSimple 8600 eszközön legalább az egyik EBOD tartományvezérlők ismerteti."
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
ms.openlocfilehash: 45699c267d1009c4884dd164fd3f2950d6d5f555
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a><span data-ttu-id="06cf9-103">Cserélje le az EBOD vezérlőhöz a StorSimple eszköz</span><span class="sxs-lookup"><span data-stu-id="06cf9-103">Replace an EBOD controller on your StorSimple device</span></span>

## <a name="overview"></a><span data-ttu-id="06cf9-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="06cf9-104">Overview</span></span>
<span data-ttu-id="06cf9-105">Ez az oktatóanyag ismerteti, hogyan cserélje le a hibás EBOD vezérlő modul a Microsoft Azure StorSimple eszközön.</span><span class="sxs-lookup"><span data-stu-id="06cf9-105">This tutorial explains how to replace a faulty EBOD controller module on your Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="06cf9-106">Egy EBOD vezérlő modul cseréjéhez kell:</span><span class="sxs-lookup"><span data-stu-id="06cf9-106">To replace an EBOD controller module, you need to:</span></span>

* <span data-ttu-id="06cf9-107">Távolítsa el a hibás EBOD vezérlő</span><span class="sxs-lookup"><span data-stu-id="06cf9-107">Remove the faulty EBOD controller</span></span>
* <span data-ttu-id="06cf9-108">Egy új EBOD controller telepítése</span><span class="sxs-lookup"><span data-stu-id="06cf9-108">Install a new EBOD controller</span></span>

<span data-ttu-id="06cf9-109">Mielőtt elkezdené, vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="06cf9-109">Consider the following information before you begin:</span></span>

* <span data-ttu-id="06cf9-110">Üres EBOD modulok minden nem használt tárolóhelyek kell beszúrni.</span><span class="sxs-lookup"><span data-stu-id="06cf9-110">Blank EBOD modules must be inserted into all unused slots.</span></span> <span data-ttu-id="06cf9-111">A ház nem cool megfelelően, ha a tárhely marad nyitva.</span><span class="sxs-lookup"><span data-stu-id="06cf9-111">The enclosure will not cool properly if a slot is left open.</span></span>
* <span data-ttu-id="06cf9-112">A EBOD tartományvezérlő közbeni-cserélhető és eltávolítani vagy lecserélni.</span><span class="sxs-lookup"><span data-stu-id="06cf9-112">The EBOD controller is hot-swappable and can be removed or replaced.</span></span> <span data-ttu-id="06cf9-113">Ne távolítsa el egy hibás modul, amíg nem helyettesíti.</span><span class="sxs-lookup"><span data-stu-id="06cf9-113">Do not remove a failed module until you have a replacement.</span></span> <span data-ttu-id="06cf9-114">A cseréjét kezdeményezésekor 10 percen belül kell befejezése.</span><span class="sxs-lookup"><span data-stu-id="06cf9-114">When you initiate the replacement process, you must finish it within 10 minutes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="06cf9-115">Távolítsa el vagy cserélje le a StorSimple összetevők közül bármelyik megkísérlése előtt győződjön meg arról, hogy tekintse át a [biztonsági ikon egyezmények](storsimple-safety.md#safety-icon-conventions) és egyéb [biztonsági óvintézkedéseket](storsimple-safety.md).</span><span class="sxs-lookup"><span data-stu-id="06cf9-115">Before attempting to remove or replace any StorSimple component, make sure that you review the [safety icon conventions](storsimple-safety.md#safety-icon-conventions) and other [safety precautions](storsimple-safety.md).</span></span>

## <a name="remove-an-ebod-controller"></a><span data-ttu-id="06cf9-116">Távolítsa el az EBOD vezérlőhöz</span><span class="sxs-lookup"><span data-stu-id="06cf9-116">Remove an EBOD controller</span></span>
<span data-ttu-id="06cf9-117">A StorSimple eszköz sikertelen EBOD vezérlő moduljának cseréje, előtt ellenőrizze, hogy a többi EBOD vezérlő modul aktív, és fut..</span><span class="sxs-lookup"><span data-stu-id="06cf9-117">Before replacing the failed EBOD controller module in your StorSimple device, make sure that the other EBOD controller module is active and running.</span></span> <span data-ttu-id="06cf9-118">Az alábbi eljárást és táblázatot azt ismertetik, hogyan EBOD vezérlő modul eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="06cf9-118">The following procedure and table explain how to remove the EBOD controller module.</span></span>

#### <a name="to-remove-an-ebod-module"></a><span data-ttu-id="06cf9-119">Egy EBOD modul eltávolítása</span><span class="sxs-lookup"><span data-stu-id="06cf9-119">To remove an EBOD module</span></span>
1. <span data-ttu-id="06cf9-120">Nyissa meg az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="06cf9-120">Open the Azure portal.</span></span>
2. <span data-ttu-id="06cf9-121">Nyissa meg az eszközre, és navigáljon a **beállítások** > **hardver állapotának**, és győződjön meg arról, hogy az aktív EBOD vezérlő modul LED állapota zöld és a sikertelen EBOD vezérlő LED modul az piros.</span><span class="sxs-lookup"><span data-stu-id="06cf9-121">Go to your device and navigate to **Settings** > **Hardware health**, and verify that the status of the LED for the active EBOD controller module is green and the LED for the failed EBOD controller module is red.</span></span>
3. <span data-ttu-id="06cf9-122">Keresse meg a sikertelen EBOD vezérlő modul hátulján az eszközön található.</span><span class="sxs-lookup"><span data-stu-id="06cf9-122">Locate the failed EBOD controller module at the back of the device.</span></span>
4. <span data-ttu-id="06cf9-123">Távolítsa el a kábelek, mielőtt kilépteti a EBOD modul kívül a rendszer a tartományvezérlő-hez a EBOD vezérlő modul.</span><span class="sxs-lookup"><span data-stu-id="06cf9-123">Remove the cables that connect the EBOD controller module to the controller before taking the EBOD module out of the system.</span></span>
5. <span data-ttu-id="06cf9-124">Jegyezze fel a pontos SAS-port-vezérlőhöz csatlakoztatott EBOD vezérlő modul.</span><span class="sxs-lookup"><span data-stu-id="06cf9-124">Make a note of the exact SAS port of the EBOD controller module that was connected to the controller.</span></span> <span data-ttu-id="06cf9-125">Akkor le kell állítani a rendszer ezt a konfigurációt a EBOD modul cseréje után.</span><span class="sxs-lookup"><span data-stu-id="06cf9-125">You will be required to restore the system to this configuration after you replace the EBOD module.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="06cf9-126">Általában ez lesz a Port, amely van címkézve **működteti** az alábbi diagram szemlélteti.</span><span class="sxs-lookup"><span data-stu-id="06cf9-126">Typically, this will be Port A, which is labeled as **Host in** in the following diagram.</span></span>
   
    ![Csatlakozópanel a EBOD vezérlő](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     <span data-ttu-id="06cf9-128">**1. ábra** vissza a EBOD modul</span><span class="sxs-lookup"><span data-stu-id="06cf9-128">**Figure 1** Back of EBOD module</span></span>
   
   | <span data-ttu-id="06cf9-129">Címke</span><span class="sxs-lookup"><span data-stu-id="06cf9-129">Label</span></span> | <span data-ttu-id="06cf9-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="06cf9-130">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="06cf9-131">1</span><span class="sxs-lookup"><span data-stu-id="06cf9-131">1</span></span> |<span data-ttu-id="06cf9-132">Tartalék LED</span><span class="sxs-lookup"><span data-stu-id="06cf9-132">Fault LED</span></span> |
   | <span data-ttu-id="06cf9-133">2</span><span class="sxs-lookup"><span data-stu-id="06cf9-133">2</span></span> |<span data-ttu-id="06cf9-134">Energiagazdálkodási LED</span><span class="sxs-lookup"><span data-stu-id="06cf9-134">Power LED</span></span> |
   | <span data-ttu-id="06cf9-135">3</span><span class="sxs-lookup"><span data-stu-id="06cf9-135">3</span></span> |<span data-ttu-id="06cf9-136">SAS-összekötők</span><span class="sxs-lookup"><span data-stu-id="06cf9-136">SAS connectors</span></span> |
   | <span data-ttu-id="06cf9-137">4</span><span class="sxs-lookup"><span data-stu-id="06cf9-137">4</span></span> |<span data-ttu-id="06cf9-138">SAS LED</span><span class="sxs-lookup"><span data-stu-id="06cf9-138">SAS LEDs</span></span> |
   | <span data-ttu-id="06cf9-139">5</span><span class="sxs-lookup"><span data-stu-id="06cf9-139">5</span></span> |<span data-ttu-id="06cf9-140">Csak a gyári használatra soros portokhoz</span><span class="sxs-lookup"><span data-stu-id="06cf9-140">Serial ports for factory use only</span></span> |
   | <span data-ttu-id="06cf9-141">6</span><span class="sxs-lookup"><span data-stu-id="06cf9-141">6</span></span> |<span data-ttu-id="06cf9-142">Port (a gazdagép)</span><span class="sxs-lookup"><span data-stu-id="06cf9-142">Port A (Host in)</span></span> |
   | <span data-ttu-id="06cf9-143">7</span><span class="sxs-lookup"><span data-stu-id="06cf9-143">7</span></span> |<span data-ttu-id="06cf9-144">B port (gazdagép kimenő)</span><span class="sxs-lookup"><span data-stu-id="06cf9-144">Port B (Host out)</span></span> |
   | <span data-ttu-id="06cf9-145">8</span><span class="sxs-lookup"><span data-stu-id="06cf9-145">8</span></span> |<span data-ttu-id="06cf9-146">Port C (csak a gyári általi használatra)</span><span class="sxs-lookup"><span data-stu-id="06cf9-146">Port C (Factory use only)</span></span> |

## <a name="install-a-new-ebod-controller"></a><span data-ttu-id="06cf9-147">Egy új EBOD controller telepítése</span><span class="sxs-lookup"><span data-stu-id="06cf9-147">Install a new EBOD controller</span></span>
<span data-ttu-id="06cf9-148">Az alábbi eljárást és táblázatot ismertetik a StorSimple eszköz egy EBOD vezérlő modul telepítése.</span><span class="sxs-lookup"><span data-stu-id="06cf9-148">The following procedure and table explain how to install an EBOD controller module in your StorSimple device.</span></span>

#### <a name="to-install-an-ebod-controller"></a><span data-ttu-id="06cf9-149">Egy EBOD tartományvezérlő telepítése</span><span class="sxs-lookup"><span data-stu-id="06cf9-149">To install an EBOD controller</span></span>
1. <span data-ttu-id="06cf9-150">Ellenőrizze a EBOD eszközön való megosztása kárt, különösen az illesztőfelület-összekötő.</span><span class="sxs-lookup"><span data-stu-id="06cf9-150">Check the EBOD device for damage, especially to the interface connector.</span></span> <span data-ttu-id="06cf9-151">Ha bármely elgörbülve ne telepítse az új EBOD tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="06cf9-151">Do not install the new EBOD controller if any pins are bent.</span></span>
2. <span data-ttu-id="06cf9-152">A zárolás van életben nyitva csúsztassa be a modul be a ház mindaddig, amíg a zárolás van életben kódolása.</span><span class="sxs-lookup"><span data-stu-id="06cf9-152">With the latches in the open position, slide the module into the enclosure until the latches engage.</span></span>
   
    ![EBOD controller telepítése](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    <span data-ttu-id="06cf9-154">**2. ábra** a EBOD tartományvezérlő-modul telepítése</span><span class="sxs-lookup"><span data-stu-id="06cf9-154">**Figure 2**  Installing the EBOD controller module</span></span>
3. <span data-ttu-id="06cf9-155">Zárja be a zárolás.</span><span class="sxs-lookup"><span data-stu-id="06cf9-155">Close the latch.</span></span> <span data-ttu-id="06cf9-156">A kattintson kell hall, mivel a zárolás kapcsolatba lép.</span><span class="sxs-lookup"><span data-stu-id="06cf9-156">You should hear a click as the latch engages.</span></span>
   
    ![EBOD zárolás feloldása](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    <span data-ttu-id="06cf9-158">**3. ábra** a EBOD modul zárolás bezárása</span><span class="sxs-lookup"><span data-stu-id="06cf9-158">**Figure 3**  Closing the EBOD module latch</span></span>
4. <span data-ttu-id="06cf9-159">Csatlakozzon újból a kábelek.</span><span class="sxs-lookup"><span data-stu-id="06cf9-159">Reconnect the cables.</span></span> <span data-ttu-id="06cf9-160">Használja a pontos konfigurációját, hogy a csere előtt voltak.</span><span class="sxs-lookup"><span data-stu-id="06cf9-160">Use the exact configuration that was present before the replacement.</span></span> <span data-ttu-id="06cf9-161">A következő ábra és táblázat kábel összekapcsolása vonatkozó további információért tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="06cf9-161">See the following diagram and table for details about how to connect the cables.</span></span>
   
    ![Az energiagazdálkodási 4U eszköz bekábelezése](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    <span data-ttu-id="06cf9-163">**4. ábra**.</span><span class="sxs-lookup"><span data-stu-id="06cf9-163">**Figure 4**.</span></span> <span data-ttu-id="06cf9-164">Újracsatlakozás a kábelek.</span><span class="sxs-lookup"><span data-stu-id="06cf9-164">Reconnecting cables</span></span>
   
   | <span data-ttu-id="06cf9-165">Címke</span><span class="sxs-lookup"><span data-stu-id="06cf9-165">Label</span></span> | <span data-ttu-id="06cf9-166">Leírás</span><span class="sxs-lookup"><span data-stu-id="06cf9-166">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="06cf9-167">1</span><span class="sxs-lookup"><span data-stu-id="06cf9-167">1</span></span> |<span data-ttu-id="06cf9-168">Elsődleges ház</span><span class="sxs-lookup"><span data-stu-id="06cf9-168">Primary enclosure</span></span> |
   | <span data-ttu-id="06cf9-169">2</span><span class="sxs-lookup"><span data-stu-id="06cf9-169">2</span></span> |<span data-ttu-id="06cf9-170">PCM 0</span><span class="sxs-lookup"><span data-stu-id="06cf9-170">PCM 0</span></span> |
   | <span data-ttu-id="06cf9-171">3</span><span class="sxs-lookup"><span data-stu-id="06cf9-171">3</span></span> |<span data-ttu-id="06cf9-172">PCM 1</span><span class="sxs-lookup"><span data-stu-id="06cf9-172">PCM 1</span></span> |
   | <span data-ttu-id="06cf9-173">4</span><span class="sxs-lookup"><span data-stu-id="06cf9-173">4</span></span> |<span data-ttu-id="06cf9-174">A vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="06cf9-174">Controller 0</span></span> |
   | <span data-ttu-id="06cf9-175">5</span><span class="sxs-lookup"><span data-stu-id="06cf9-175">5</span></span> |<span data-ttu-id="06cf9-176">1. vezérlő</span><span class="sxs-lookup"><span data-stu-id="06cf9-176">Controller 1</span></span> |
   | <span data-ttu-id="06cf9-177">6</span><span class="sxs-lookup"><span data-stu-id="06cf9-177">6</span></span> |<span data-ttu-id="06cf9-178">EBOD vezérlő 0</span><span class="sxs-lookup"><span data-stu-id="06cf9-178">EBOD controller 0</span></span> |
   | <span data-ttu-id="06cf9-179">7</span><span class="sxs-lookup"><span data-stu-id="06cf9-179">7</span></span> |<span data-ttu-id="06cf9-180">1. EBOD vezérlő</span><span class="sxs-lookup"><span data-stu-id="06cf9-180">EBOD controller 1</span></span> |
   | <span data-ttu-id="06cf9-181">8</span><span class="sxs-lookup"><span data-stu-id="06cf9-181">8</span></span> |<span data-ttu-id="06cf9-182">EBOD ház</span><span class="sxs-lookup"><span data-stu-id="06cf9-182">EBOD enclosure</span></span> |
   | <span data-ttu-id="06cf9-183">9</span><span class="sxs-lookup"><span data-stu-id="06cf9-183">9</span></span> |<span data-ttu-id="06cf9-184">Áramelosztó egységekből</span><span class="sxs-lookup"><span data-stu-id="06cf9-184">Power Distribution Units</span></span> |

## <a name="next-steps"></a><span data-ttu-id="06cf9-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06cf9-185">Next steps</span></span>
<span data-ttu-id="06cf9-186">További információ [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="06cf9-186">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

