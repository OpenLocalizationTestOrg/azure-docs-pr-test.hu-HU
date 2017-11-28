---
title: "aaaChange hello StorSimple eszköz mód |} Microsoft Docs"
description: "Hello StorSimple eszköz módokat ismerteti és bemutatja hogyan toouse Windows PowerShell a StorSimple toochange hello eszköz mód."
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
ms.openlocfilehash: 299fd380a83bcd06780c97937f4064f0791b440d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-device-mode-on-your-storsimple-device"></a><span data-ttu-id="d95ff-103">A StorSimple eszköz hello eszköz üzemmód módosítása</span><span class="sxs-lookup"><span data-stu-id="d95ff-103">Change hello device mode on your StorSimple device</span></span>
<span data-ttu-id="d95ff-104">Ez a cikk ismerteti, rövid hello különböző módokat, amelyben a StorSimple eszköz működhet.</span><span class="sxs-lookup"><span data-stu-id="d95ff-104">This article provides a brief description of hello various modes in which your StorSimple device can operate.</span></span> <span data-ttu-id="d95ff-105">A StorSimple eszköz működhet három üzemmódban: normál, karbantartási és helyreállítás.</span><span class="sxs-lookup"><span data-stu-id="d95ff-105">Your StorSimple device can function in three modes: normal, maintenance, and recovery.</span></span> 

<span data-ttu-id="d95ff-106">A cikk elolvasása után tudni fogja:</span><span class="sxs-lookup"><span data-stu-id="d95ff-106">After reading this article, you will know:</span></span>

* <span data-ttu-id="d95ff-107">Milyen hello StorSimple eszköz módok a következők:</span><span class="sxs-lookup"><span data-stu-id="d95ff-107">What hello StorSimple device modes are</span></span>
* <span data-ttu-id="d95ff-108">Hogyan toofigure kimenő üzemmódjának hello StorSimple-eszköz van</span><span class="sxs-lookup"><span data-stu-id="d95ff-108">How toofigure out which mode hello StorSimple device is in</span></span>
* <span data-ttu-id="d95ff-109">Hogyan normál toomaintenance üzemmódról toochange és *ez fordítva is igaz*</span><span class="sxs-lookup"><span data-stu-id="d95ff-109">How toochange from normal toomaintenance mode and *vice versa*</span></span>

<span data-ttu-id="d95ff-110">felügyeleti feladatok felett hello csak a StorSimple eszköz hello Windows PowerShell felületén keresztül végezhető el.</span><span class="sxs-lookup"><span data-stu-id="d95ff-110">hello above management tasks can only be performed via hello Windows PowerShell interface of your StorSimple device.</span></span>

## <a name="about-storsimple-device-modes"></a><span data-ttu-id="d95ff-111">A StorSimple eszköz módokkal kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="d95ff-111">About StorSimple device modes</span></span>
<span data-ttu-id="d95ff-112">A StorSimple eszköz normál, karbantartási vagy helyreállítási módban is működik.</span><span class="sxs-lookup"><span data-stu-id="d95ff-112">Your StorSimple device can operate in normal, maintenance, or recovery mode.</span></span> <span data-ttu-id="d95ff-113">Módokban az alábbiakban röviden bemutatjuk.</span><span class="sxs-lookup"><span data-stu-id="d95ff-113">Each of these modes is briefly described below.</span></span>

### <a name="normal-mode"></a><span data-ttu-id="d95ff-114">Normál mód</span><span class="sxs-lookup"><span data-stu-id="d95ff-114">Normal mode</span></span>
<span data-ttu-id="d95ff-115">Ez normál működési mód hello teljesen konfigurált a StorSimple eszköz típusúként van definiálva.</span><span class="sxs-lookup"><span data-stu-id="d95ff-115">This is defined as hello normal operational mode for a fully configured StorSimple device.</span></span> <span data-ttu-id="d95ff-116">Alapértelmezés szerint az eszköz normál üzemmódban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d95ff-116">By default, your device should be in normal mode.</span></span>

### <a name="maintenance-mode"></a><span data-ttu-id="d95ff-117">A karbantartási mód</span><span class="sxs-lookup"><span data-stu-id="d95ff-117">Maintenance mode</span></span>
<span data-ttu-id="d95ff-118">Néha hello StorSimple-eszköz szükség toobe karbantartási módba helyezve.</span><span class="sxs-lookup"><span data-stu-id="d95ff-118">Sometimes hello StorSimple device may need toobe placed into maintenance mode.</span></span> <span data-ttu-id="d95ff-119">Ez a mód lehetővé teszi hello eszközön tooperform karbantartási és zavaró frissítések telepítése, például azok kapcsolatos toodisk belső vezérlőprogram.</span><span class="sxs-lookup"><span data-stu-id="d95ff-119">This mode allows you tooperform maintenance on hello device and install disruptive updates, such as those related toodisk firmware.</span></span>

<span data-ttu-id="d95ff-120">A StorSimple helyezhetik hello rendszer csak a Windows PowerShell hello keresztül karbantartási módba.</span><span class="sxs-lookup"><span data-stu-id="d95ff-120">You can put hello system into maintenance mode only via hello Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="d95ff-121">Az összes i/o-kérelmek fel van függesztve, ebben a módban.</span><span class="sxs-lookup"><span data-stu-id="d95ff-121">All I/O requests are paused in this mode.</span></span> <span data-ttu-id="d95ff-122">Például nem felejtő közvetlen elérésű memória (NVRAM) vagy a fürtszolgáltatás hello szolgáltatás is le van állítva.</span><span class="sxs-lookup"><span data-stu-id="d95ff-122">Services such as non-volatile random access memory (NVRAM) or hello clustering service are also stopped.</span></span> <span data-ttu-id="d95ff-123">Mindkét hello tartományvezérlők adja meg, vagy lépjen ki az ebben a módban újraindul.</span><span class="sxs-lookup"><span data-stu-id="d95ff-123">Both hello controllers are restarted when you enter or exit this mode.</span></span> <span data-ttu-id="d95ff-124">Hello karbantartási módból való kilépéskor minden hello szolgáltatás folytatódik, és megfelelő kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d95ff-124">When you exit hello maintenance mode, all hello services will resume and should be healthy.</span></span> <span data-ttu-id="d95ff-125">Ez eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="d95ff-125">This may take a few minutes.</span></span>

> [!NOTE]
> <span data-ttu-id="d95ff-126">**A karbantartási mód csak egy megfelelően működő eszközön támogatott. Nem támogatott egy eszközön, amely az egyik vagy mindkét hello, tartományvezérlői nem megfelelően működnek.**
> </span><span class="sxs-lookup"><span data-stu-id="d95ff-126">**Maintenance mode is only supported on a properly functioning device. It is not supported on a device in which one or both of hello controllers are not functioning.**
</span></span></br>
> 
> 

### <a name="recovery-mode"></a><span data-ttu-id="d95ff-127">Helyreállítási módban</span><span class="sxs-lookup"><span data-stu-id="d95ff-127">Recovery mode</span></span>
<span data-ttu-id="d95ff-128">Helyreállítási módban, "A Windows csökkentett módban hálózati támogatás" jelentheti.</span><span class="sxs-lookup"><span data-stu-id="d95ff-128">Recovery mode can be described as "Windows Safe Mode with network support".</span></span> <span data-ttu-id="d95ff-129">Helyreállítási módban hello Microsoft Support csoport végez, és lehetővé teszi, hogy azok tooperform diagnosztika hello rendszeren.</span><span class="sxs-lookup"><span data-stu-id="d95ff-129">Recovery mode engages hello Microsoft Support team and allows them tooperform diagnostics on hello system.</span></span> <span data-ttu-id="d95ff-130">hello elsődleges helyreállítási módban célja tooretrieve hello rendszer naplóit.</span><span class="sxs-lookup"><span data-stu-id="d95ff-130">hello primary goal of recovery mode is tooretrieve hello system logs.</span></span>

<span data-ttu-id="d95ff-131">Ha a rendszer helyreállítási módba kerül, a következő lépéshez lépjen kapcsolatba a Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="d95ff-131">If your system goes into recovery mode, you should contact Microsoft Support for next steps.</span></span> <span data-ttu-id="d95ff-132">További információ: túl[forduljon a Microsoft ügyfélszolgálatához](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="d95ff-132">For more information, go too[Contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

> [!NOTE]
> <span data-ttu-id="d95ff-133">**Hello eszköz helyreállítási módban nem helyezhető el. Hello eszköz rossz állapotban van, ha a helyreállítási módban tooget hello eszköz megpróbál olyan állapotba, amelyben Microsoft Support szakembereinek ellenőrizheti azt.**</span><span class="sxs-lookup"><span data-stu-id="d95ff-133">**You cannot place hello device in recovery mode. If hello device is in a bad state, recovery mode tries tooget hello device into a state in which Microsoft Support personnel can examine it.**</span></span>
> 
> 

## <a name="determine-storsimple-device-mode"></a><span data-ttu-id="d95ff-134">Határozza meg a StorSimple eszköz mód</span><span class="sxs-lookup"><span data-stu-id="d95ff-134">Determine StorSimple device mode</span></span>
#### <a name="toodetermine-hello-current-device-mode"></a><span data-ttu-id="d95ff-135">toodetermine hello aktuális eszköz mód</span><span class="sxs-lookup"><span data-stu-id="d95ff-135">toodetermine hello current device mode</span></span>
1. <span data-ttu-id="d95ff-136">Jelentkezzen be toohello eszköz soros konzoljához hello utasításait követve [PuTTY használata tooconnect toohello eszköz soros konzoljához](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="d95ff-136">Log on toohello device serial console by following hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="d95ff-137">Nézze meg a szalagcím üdvözlőüzenetére hello hello eszköz soros konzoljához menüjében.</span><span class="sxs-lookup"><span data-stu-id="d95ff-137">Look at hello banner message in hello serial console menu of hello device.</span></span> <span data-ttu-id="d95ff-138">Ez az üzenet explicit módon azt jelzi, hogy hello eszköz karbantartás vagy helyreállítási módban.</span><span class="sxs-lookup"><span data-stu-id="d95ff-138">This message explicitly indicates whether hello device is in maintenance or recovery mode.</span></span> <span data-ttu-id="d95ff-139">Hello üzenet nem tartalmaz semmilyen konkrét információkat toohello rendszer mód, ha hello eszköz normál módban van.</span><span class="sxs-lookup"><span data-stu-id="d95ff-139">If hello message does not contain any specific information pertaining toohello system mode, hello device is in normal mode.</span></span>

## <a name="change-hello-storsimple-device-mode"></a><span data-ttu-id="d95ff-140">Hello StorSimple eszköz módjának módosítása</span><span class="sxs-lookup"><span data-stu-id="d95ff-140">Change hello StorSimple device mode</span></span>
<span data-ttu-id="d95ff-141">Hello StorSimple eszköz helyezzen karbantartási üzemmódról (normál) tooperform karbantartási, vagy telepítse a karbantartási mód frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="d95ff-141">You can place hello StorSimple device into maintenance mode (from normal mode) tooperform maintenance or install maintenance mode updates.</span></span> <span data-ttu-id="d95ff-142">Hajtsa végre a következő eljárások tooenter vagy kilépés a karbantartási mód hello.</span><span class="sxs-lookup"><span data-stu-id="d95ff-142">Perform hello following procedures tooenter or exit maintenance mode.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d95ff-143">Mielőtt karbantartási módba, ellenőrizze, hogy mindkét eszközvezérlők kifogástalan hello elérésével **hardverállapot** a hello **karbantartási** hello klasszikus Azure portálra a lap.</span><span class="sxs-lookup"><span data-stu-id="d95ff-143">Before entering maintenance mode, verify that both device controllers are healthy by accessing hello **Hardware Status** on hello **Maintenance** page in hello Azure classic portal.</span></span> <span data-ttu-id="d95ff-144">Ha az egyik vagy mindkét hello tartományvezérlők nem működik megfelelően, forduljon a Microsoft Support hello további lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d95ff-144">If one or both hello controllers are not healthy, contact Microsoft Support for hello next steps.</span></span> <span data-ttu-id="d95ff-145">További információ: túl[forduljon a Microsoft ügyfélszolgálatához](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="d95ff-145">For more information, go too[Contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>
> 
> 

#### <a name="tooenter-maintenance-mode"></a><span data-ttu-id="d95ff-146">tooenter a karbantartási mód</span><span class="sxs-lookup"><span data-stu-id="d95ff-146">tooenter maintenance mode</span></span>
1. <span data-ttu-id="d95ff-147">Jelentkezzen be toohello eszköz soros konzoljához hello utasításait követve [PuTTY használata tooconnect toohello eszköz soros konzoljához](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="d95ff-147">Log on toohello device serial console by following hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="d95ff-148">Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="d95ff-148">In hello serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="d95ff-149">Amikor a rendszer kéri, adja meg a hello **eszköz rendszergazdai jelszava**.</span><span class="sxs-lookup"><span data-stu-id="d95ff-149">When prompted, provide hello **device administrator password**.</span></span> <span data-ttu-id="d95ff-150">hello alapértelmezett jelszava: `Password1`.</span><span class="sxs-lookup"><span data-stu-id="d95ff-150">hello default password is: `Password1`.</span></span>
3. <span data-ttu-id="d95ff-151">Írja be a parancssorba hello</span><span class="sxs-lookup"><span data-stu-id="d95ff-151">At hello command prompt, type</span></span> 
   
    `Enter-HcsMaintenanceMode`
4. <span data-ttu-id="d95ff-152">Látni fogja a tájékoztatással, hogy a karbantartási mód figyelmeztető üzenetet fog megzavarhatják összes i/o-kérelmek és sever hello kapcsolat toohello a klasszikus Azure portálon, és megerősítés kéri.</span><span class="sxs-lookup"><span data-stu-id="d95ff-152">You will see a warning message telling you that maintenance mode will disrupt all I/O requests and sever hello connection toohello Azure classic portal, and you will be prompted for confirmation.</span></span> <span data-ttu-id="d95ff-153">Típus **Y** tooenter karbantartási módban.</span><span class="sxs-lookup"><span data-stu-id="d95ff-153">Type **Y** tooenter maintenance mode.</span></span>
5. <span data-ttu-id="d95ff-154">Mindkét tartományvezérlők újraindul.</span><span class="sxs-lookup"><span data-stu-id="d95ff-154">Both controllers will restart.</span></span> <span data-ttu-id="d95ff-155">Ha hello újraindítás befejeződött, egy másik üzenet jelenik meg arról, hogy hello eszköz karbantartási módban.</span><span class="sxs-lookup"><span data-stu-id="d95ff-155">When hello restart is complete, another message will appear indicating that hello device is in maintenance mode.</span></span>

#### <a name="tooexit-maintenance-mode"></a><span data-ttu-id="d95ff-156">tooexit a karbantartási mód</span><span class="sxs-lookup"><span data-stu-id="d95ff-156">tooexit maintenance mode</span></span>
1. <span data-ttu-id="d95ff-157">Jelentkezzen be toohello eszköz soros konzoljához.</span><span class="sxs-lookup"><span data-stu-id="d95ff-157">Log on toohello device serial console.</span></span> <span data-ttu-id="d95ff-158">Ellenőrizze az üzenetből hello szalagcím, amely az eszköz karbantartási módban van.</span><span class="sxs-lookup"><span data-stu-id="d95ff-158">Verify from hello banner message that your device is in maintenance mode.</span></span>
2. <span data-ttu-id="d95ff-159">Hello parancsot, írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="d95ff-159">At hello command prompt, type:</span></span>
   
    `Exit-HcsMaintenanceMode`
3. <span data-ttu-id="d95ff-160">Ekkor megjelenik egy figyelmeztető üzenet, és egy megerősítő üzenetet.</span><span class="sxs-lookup"><span data-stu-id="d95ff-160">A warning message and a confirmation message will appear.</span></span> <span data-ttu-id="d95ff-161">Típus **Y** tooexit karbantartási módban.</span><span class="sxs-lookup"><span data-stu-id="d95ff-161">Type **Y** tooexit maintenance mode.</span></span>
4. <span data-ttu-id="d95ff-162">Mindkét tartományvezérlők újraindul.</span><span class="sxs-lookup"><span data-stu-id="d95ff-162">Both controllers will restart.</span></span> <span data-ttu-id="d95ff-163">Ha hello újraindítás befejeződött, egy másik üzenet jelenik meg arról, hogy hello eszköz normál üzemmódban.</span><span class="sxs-lookup"><span data-stu-id="d95ff-163">When hello restart is complete, another message will appear indicating that hello device is in normal mode.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d95ff-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d95ff-164">Next steps</span></span>
<span data-ttu-id="d95ff-165">Ismerje meg, hogyan túl[normál és a karbantartási mód frissítések alkalmazása](storsimple-update-device.md) a StorSimple eszköz.</span><span class="sxs-lookup"><span data-stu-id="d95ff-165">Learn how too[apply normal and maintenance mode updates](storsimple-update-device.md) on your StorSimple device.</span></span>

