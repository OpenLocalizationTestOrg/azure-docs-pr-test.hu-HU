---
title: "A StorSimple eszköz üzemmód módosítása |} Microsoft Docs"
description: "A StorSimple eszköz módok és használatához a Windows PowerShell-lel módosíthatja eszköz módját ismerteti."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: e9d7d277-8a2f-45eb-9fef-355486e14cbc
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2016
ms.author: alkohli
ms.openlocfilehash: 33c65bf2eecff3914f3227c76f7d638a4507e1f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="change-the-device-mode-on-your-storsimple-device"></a><span data-ttu-id="60564-103">A StorSimple eszköz az eszköz üzemmód módosítása</span><span class="sxs-lookup"><span data-stu-id="60564-103">Change the device mode on your StorSimple device</span></span>
<span data-ttu-id="60564-104">A cikkben a különböző módokat, amelyben a StorSimple eszköz működhet rövid leírása.</span><span class="sxs-lookup"><span data-stu-id="60564-104">This article provides a brief description of the various modes in which your StorSimple device can operate.</span></span> <span data-ttu-id="60564-105">A StorSimple eszköz működhet három üzemmódban: normál, karbantartási és helyreállítás.</span><span class="sxs-lookup"><span data-stu-id="60564-105">Your StorSimple device can function in three modes: normal, maintenance, and recovery.</span></span> 

<span data-ttu-id="60564-106">A cikk elolvasása után tudni fogja:</span><span class="sxs-lookup"><span data-stu-id="60564-106">After reading this article, you will know:</span></span>

* <span data-ttu-id="60564-107">Milyen a StorSimple eszköz módok a következők:</span><span class="sxs-lookup"><span data-stu-id="60564-107">What the StorSimple device modes are</span></span>
* <span data-ttu-id="60564-108">Mérje fel, melyik módban a StorSimple eszköz hogyan van</span><span class="sxs-lookup"><span data-stu-id="60564-108">How to figure out which mode the StorSimple device is in</span></span>
* <span data-ttu-id="60564-109">A normál és a karbantartási mód módosítása és *ez fordítva is igaz*</span><span class="sxs-lookup"><span data-stu-id="60564-109">How to change from normal to maintenance mode and *vice versa*</span></span>

<span data-ttu-id="60564-110">A fenti kezelési feladatok csak a Windows PowerShell-felületet a StorSimple eszköz keresztül végezhető el.</span><span class="sxs-lookup"><span data-stu-id="60564-110">The above management tasks can only be performed via the Windows PowerShell interface of your StorSimple device.</span></span>

## <a name="about-storsimple-device-modes"></a><span data-ttu-id="60564-111">A StorSimple eszköz módokkal kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="60564-111">About StorSimple device modes</span></span>
<span data-ttu-id="60564-112">A StorSimple eszköz normál, karbantartási vagy helyreállítási módban is működik.</span><span class="sxs-lookup"><span data-stu-id="60564-112">Your StorSimple device can operate in normal, maintenance, or recovery mode.</span></span> <span data-ttu-id="60564-113">Módokban az alábbiakban röviden bemutatjuk.</span><span class="sxs-lookup"><span data-stu-id="60564-113">Each of these modes is briefly described below.</span></span>

### <a name="normal-mode"></a><span data-ttu-id="60564-114">Normál mód</span><span class="sxs-lookup"><span data-stu-id="60564-114">Normal mode</span></span>
<span data-ttu-id="60564-115">Ez a teljesen konfigurált a StorSimple eszköz normál működési mód típusúként van definiálva.</span><span class="sxs-lookup"><span data-stu-id="60564-115">This is defined as the normal operational mode for a fully configured StorSimple device.</span></span> <span data-ttu-id="60564-116">Alapértelmezés szerint az eszköz normál üzemmódban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="60564-116">By default, your device should be in normal mode.</span></span>

### <a name="maintenance-mode"></a><span data-ttu-id="60564-117">A karbantartási mód</span><span class="sxs-lookup"><span data-stu-id="60564-117">Maintenance mode</span></span>
<span data-ttu-id="60564-118">A StorSimple eszköz néha szükség lehet karbantartási módba kell helyezni.</span><span class="sxs-lookup"><span data-stu-id="60564-118">Sometimes the StorSimple device may need to be placed into maintenance mode.</span></span> <span data-ttu-id="60564-119">Ebben a módban lehetővé teszi az eszköz karbantartásához és zavaró frissítések, például a lemez belső vezérlőprogram kapcsolódó telepítése.</span><span class="sxs-lookup"><span data-stu-id="60564-119">This mode allows you to perform maintenance on the device and install disruptive updates, such as those related to disk firmware.</span></span>

<span data-ttu-id="60564-120">A StorSimple helyezhetik a rendszer csak a Windows PowerShell segítségével karbantartási módba.</span><span class="sxs-lookup"><span data-stu-id="60564-120">You can put the system into maintenance mode only via the Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="60564-121">Az összes i/o-kérelmek fel van függesztve, ebben a módban.</span><span class="sxs-lookup"><span data-stu-id="60564-121">All I/O requests are paused in this mode.</span></span> <span data-ttu-id="60564-122">Például nem felejtő közvetlen elérésű memória (NVRAM) vagy a fürtözött szolgáltatás is le van állítva.</span><span class="sxs-lookup"><span data-stu-id="60564-122">Services such as non-volatile random access memory (NVRAM) or the clustering service are also stopped.</span></span> <span data-ttu-id="60564-123">Mindkét vezérlőhöz adja meg, vagy lépjen ki az ebben a módban újraindul.</span><span class="sxs-lookup"><span data-stu-id="60564-123">Both the controllers are restarted when you enter or exit this mode.</span></span> <span data-ttu-id="60564-124">A karbantartási mód kilépéskor a szolgáltatások folytatódik, és kifogástalan kell lennie.</span><span class="sxs-lookup"><span data-stu-id="60564-124">When you exit the maintenance mode, all the services will resume and should be healthy.</span></span> <span data-ttu-id="60564-125">Ez eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="60564-125">This may take a few minutes.</span></span>

> [!NOTE]
> <span data-ttu-id="60564-126">**A karbantartási mód csak egy megfelelően működő eszközön támogatott. Nem támogatott egy eszközön, amelyben a tartományvezérlők valamelyike nem megfelelően működnek.**
> </span><span class="sxs-lookup"><span data-stu-id="60564-126">**Maintenance mode is only supported on a properly functioning device. It is not supported on a device in which one or both of the controllers are not functioning.**
</span></span></br>
> 
> 

### <a name="recovery-mode"></a><span data-ttu-id="60564-127">Helyreállítási módban</span><span class="sxs-lookup"><span data-stu-id="60564-127">Recovery mode</span></span>
<span data-ttu-id="60564-128">Helyreállítási módban, "A Windows csökkentett módban hálózati támogatás" jelentheti.</span><span class="sxs-lookup"><span data-stu-id="60564-128">Recovery mode can be described as "Windows Safe Mode with network support".</span></span> <span data-ttu-id="60564-129">Helyreállítási módban kapcsolatba lép a Microsoft Support csoport, és lehetővé teszi a diagnosztikát végezzen a rendszeren.</span><span class="sxs-lookup"><span data-stu-id="60564-129">Recovery mode engages the Microsoft Support team and allows them to perform diagnostics on the system.</span></span> <span data-ttu-id="60564-130">Helyreállítási módban elsődleges célja a rendszernaplókat beolvasása.</span><span class="sxs-lookup"><span data-stu-id="60564-130">The primary goal of recovery mode is to retrieve the system logs.</span></span>

<span data-ttu-id="60564-131">Ha a rendszer helyreállítási módba kerül, a következő lépéshez lépjen kapcsolatba a Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="60564-131">If your system goes into recovery mode, you should contact Microsoft Support for next steps.</span></span> <span data-ttu-id="60564-132">További információkért látogasson el [forduljon a Microsoft ügyfélszolgálatához](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="60564-132">For more information, go to [Contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

> [!NOTE]
> <span data-ttu-id="60564-133">**Az eszköz nem helyezhető el a következő helyreállítási módban. Ha az eszköz rossz állapotban van, helyreállítási módban próbálja meg olyan állapotba, amelyben Microsoft Support szakembereinek ellenőrizheti, hogy az eszköz eléréséhez.**</span><span class="sxs-lookup"><span data-stu-id="60564-133">**You cannot place the device in recovery mode. If the device is in a bad state, recovery mode tries to get the device into a state in which Microsoft Support personnel can examine it.**</span></span>
> 
> 

## <a name="determine-storsimple-device-mode"></a><span data-ttu-id="60564-134">Határozza meg a StorSimple eszköz mód</span><span class="sxs-lookup"><span data-stu-id="60564-134">Determine StorSimple device mode</span></span>
#### <a name="to-determine-the-current-device-mode"></a><span data-ttu-id="60564-135">Az aktuális eszköz mód meghatározásához</span><span class="sxs-lookup"><span data-stu-id="60564-135">To determine the current device mode</span></span>
1. <span data-ttu-id="60564-136">Jelentkezzen be az eszköz soros konzoljához lépéseit követve [a PuTTY használata az eszköz soros konzoljához való csatlakozáshoz](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="60564-136">Log on to the device serial console by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="60564-137">Tekintse meg az eszköz soros konzoljához menüjében a szalagcím üzenet.</span><span class="sxs-lookup"><span data-stu-id="60564-137">Look at the banner message in the serial console menu of the device.</span></span> <span data-ttu-id="60564-138">Ez az üzenet explicit módon azt jelzi, hogy az eszköz karbantartás vagy helyreállítási módban.</span><span class="sxs-lookup"><span data-stu-id="60564-138">This message explicitly indicates whether the device is in maintenance or recovery mode.</span></span> <span data-ttu-id="60564-139">Ha az üzenet nem tartalmaz a rendszer módra vonatkozó információk, az eszköz normál üzemmódban van.</span><span class="sxs-lookup"><span data-stu-id="60564-139">If the message does not contain any specific information pertaining to the system mode, the device is in normal mode.</span></span>

## <a name="change-the-storsimple-device-mode"></a><span data-ttu-id="60564-140">A StorSimple eszköz üzemmód módosítása</span><span class="sxs-lookup"><span data-stu-id="60564-140">Change the StorSimple device mode</span></span>
<span data-ttu-id="60564-141">Elhelyezhet a StorSimple eszköz karbantartási módba (a normál módba) karbantartás vagy a karbantartási mód frissítések telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="60564-141">You can place the StorSimple device into maintenance mode (from normal mode) to perform maintenance or install maintenance mode updates.</span></span> <span data-ttu-id="60564-142">Hajtsa végre a következő adja meg, vagy kilép karbantartási módból.</span><span class="sxs-lookup"><span data-stu-id="60564-142">Perform the following procedures to enter or exit maintenance mode.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="60564-143">Mielőtt karbantartási módba, ellenőrizze, hogy mindkét eszközvezérlők kifogástalan elérésével a **hardverállapot** a a **karbantartási** a klasszikus Azure portálra a lap.</span><span class="sxs-lookup"><span data-stu-id="60564-143">Before entering maintenance mode, verify that both device controllers are healthy by accessing the **Hardware Status** on the **Maintenance** page in the Azure classic portal.</span></span> <span data-ttu-id="60564-144">Ha egyik vagy mindkét a tartományvezérlők nem működik megfelelően, a következő lépéseket illetően forduljon a Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="60564-144">If one or both the controllers are not healthy, contact Microsoft Support for the next steps.</span></span> <span data-ttu-id="60564-145">További információkért látogasson el [forduljon a Microsoft ügyfélszolgálatához](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="60564-145">For more information, go to [Contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>
> 
> 

#### <a name="to-enter-maintenance-mode"></a><span data-ttu-id="60564-146">Adja meg a karbantartási módból való</span><span class="sxs-lookup"><span data-stu-id="60564-146">To enter maintenance mode</span></span>
1. <span data-ttu-id="60564-147">Jelentkezzen be az eszköz soros konzoljához lépéseit követve [a PuTTY használata az eszköz soros konzoljához való csatlakozáshoz](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="60564-147">Log on to the device serial console by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="60564-148">A soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="60564-148">In the serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="60564-149">Amikor a rendszer kéri, adja meg a **eszköz rendszergazdai jelszava**.</span><span class="sxs-lookup"><span data-stu-id="60564-149">When prompted, provide the **device administrator password**.</span></span> <span data-ttu-id="60564-150">Az alapértelmezett jelszava: `Password1`.</span><span class="sxs-lookup"><span data-stu-id="60564-150">The default password is: `Password1`.</span></span>
3. <span data-ttu-id="60564-151">Írja be a parancssorba</span><span class="sxs-lookup"><span data-stu-id="60564-151">At the command prompt, type</span></span> 
   
    `Enter-HcsMaintenanceMode`
4. <span data-ttu-id="60564-152">Látni fogja, hogy a karbantartási mód lesz megzavarhatják összes i/o-kérelmek és -kiszolgálón a kapcsolat a klasszikus Azure portálra, és megerősítés bekéri felszólító figyelmeztető üzenetet.</span><span class="sxs-lookup"><span data-stu-id="60564-152">You will see a warning message telling you that maintenance mode will disrupt all I/O requests and sever the connection to the Azure classic portal, and you will be prompted for confirmation.</span></span> <span data-ttu-id="60564-153">Típus **Y** a karbantartási üzemmódba.</span><span class="sxs-lookup"><span data-stu-id="60564-153">Type **Y** to enter maintenance mode.</span></span>
5. <span data-ttu-id="60564-154">Mindkét tartományvezérlők újraindul.</span><span class="sxs-lookup"><span data-stu-id="60564-154">Both controllers will restart.</span></span> <span data-ttu-id="60564-155">Ha az újraindítás befejeződött, egy másik üzenet jelenik meg arról, hogy az eszköz karbantartási módban.</span><span class="sxs-lookup"><span data-stu-id="60564-155">When the restart is complete, another message will appear indicating that the device is in maintenance mode.</span></span>

#### <a name="to-exit-maintenance-mode"></a><span data-ttu-id="60564-156">A kilépéshez a karbantartási mód</span><span class="sxs-lookup"><span data-stu-id="60564-156">To exit maintenance mode</span></span>
1. <span data-ttu-id="60564-157">Jelentkezzen be az eszköz soros konzoljához.</span><span class="sxs-lookup"><span data-stu-id="60564-157">Log on to the device serial console.</span></span> <span data-ttu-id="60564-158">Ellenőrizze a szalagcím üzenet, amely az eszköz karbantartási módban van.</span><span class="sxs-lookup"><span data-stu-id="60564-158">Verify from the banner message that your device is in maintenance mode.</span></span>
2. <span data-ttu-id="60564-159">A parancssorba írja be:</span><span class="sxs-lookup"><span data-stu-id="60564-159">At the command prompt, type:</span></span>
   
    `Exit-HcsMaintenanceMode`
3. <span data-ttu-id="60564-160">Ekkor megjelenik egy figyelmeztető üzenet, és egy megerősítő üzenetet.</span><span class="sxs-lookup"><span data-stu-id="60564-160">A warning message and a confirmation message will appear.</span></span> <span data-ttu-id="60564-161">Típus **Y** kilép karbantartási módból.</span><span class="sxs-lookup"><span data-stu-id="60564-161">Type **Y** to exit maintenance mode.</span></span>
4. <span data-ttu-id="60564-162">Mindkét tartományvezérlők újraindul.</span><span class="sxs-lookup"><span data-stu-id="60564-162">Both controllers will restart.</span></span> <span data-ttu-id="60564-163">Ha az újraindítás befejeződött, egy másik üzenet jelenik meg arról, hogy az eszköz normál üzemmódban.</span><span class="sxs-lookup"><span data-stu-id="60564-163">When the restart is complete, another message will appear indicating that the device is in normal mode.</span></span>

## <a name="next-steps"></a><span data-ttu-id="60564-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60564-164">Next steps</span></span>
<span data-ttu-id="60564-165">Megtudhatja, hogyan [normál és a karbantartási mód frissítések alkalmazása](storsimple-update-device.md) a StorSimple eszköz.</span><span class="sxs-lookup"><span data-stu-id="60564-165">Learn how to [apply normal and maintenance mode updates](storsimple-update-device.md) on your StorSimple device.</span></span>

