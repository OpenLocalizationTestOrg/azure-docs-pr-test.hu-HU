---
title: "aaaUpdate a StorSimple eszköz |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello StorSimple frissítése szolgáltatás tooinstall rendszeres és karbantartási mód frissítéseket és gyorsjavításokat."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 786059f5-2a38-4105-941d-0860ce4ac515
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/18/2016
ms.author: v-sharos
ms.openlocfilehash: 05acf05c8fc89bbb4343f67ad103235bbe3dba0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-your-storsimple-8000-series-device"></a><span data-ttu-id="579c8-103">A StorSimple 8000 Series eszköz frissítése</span><span class="sxs-lookup"><span data-stu-id="579c8-103">Update your StorSimple 8000 Series device</span></span>
## <a name="overview"></a><span data-ttu-id="579c8-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="579c8-104">Overview</span></span>
<span data-ttu-id="579c8-105">hello StorSimple frissítések a szolgáltatások lehetővé teszik tooeasily a StorSimple eszköz naprakész állapotban tartása.</span><span class="sxs-lookup"><span data-stu-id="579c8-105">hello StorSimple updates features allow you tooeasily keep your StorSimple device up-to-date.</span></span> <span data-ttu-id="579c8-106">Hello frissítés típusától függően a frissítések toohello eszköz hello Windows PowerShell felületén keresztül vagy hello a klasszikus Azure portálon keresztül is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="579c8-106">Depending on hello update type, you can apply updates toohello device via hello Azure classic portal or via hello Windows PowerShell interface.</span></span> <span data-ttu-id="579c8-107">Ez az oktatóanyag leírja hello frissítési típusait, és hogyan tooinstall minden közülük.</span><span class="sxs-lookup"><span data-stu-id="579c8-107">This tutorial describes hello update types and how tooinstall each of them.</span></span>

<span data-ttu-id="579c8-108">Kétféle eszközfrissítésekhez alkalmazhatja:</span><span class="sxs-lookup"><span data-stu-id="579c8-108">You can apply two types of device updates:</span></span> 

* <span data-ttu-id="579c8-109">Rendszeres (vagy normál módú) frissítéseket</span><span class="sxs-lookup"><span data-stu-id="579c8-109">Regular (or Normal mode) updates</span></span>
* <span data-ttu-id="579c8-110">Karbantartási mód frissítések</span><span class="sxs-lookup"><span data-stu-id="579c8-110">Maintenance mode updates</span></span>

<span data-ttu-id="579c8-111">A klasszikus Azure portálon hello vagy a Windows PowerShell; keresztül rendszeres frissítések telepítése a Windows PowerShell tooinstall karbantartási mód frissítések kell használnia.</span><span class="sxs-lookup"><span data-stu-id="579c8-111">You can install regular updates via hello Azure classic portal or Windows PowerShell; however, you must use Windows PowerShell tooinstall Maintenance mode updates.</span></span> 

<span data-ttu-id="579c8-112">Minden egyes frissítés típus külön-külön alább.</span><span class="sxs-lookup"><span data-stu-id="579c8-112">Each update type is described separately, below.</span></span>

### <a name="regular-updates"></a><span data-ttu-id="579c8-113">Rendszeres frissítések</span><span class="sxs-lookup"><span data-stu-id="579c8-113">Regular updates</span></span>
<span data-ttu-id="579c8-114">Rendszeres frissítéseket tudja telepíteni, ha hello eszköz normál üzemmódban nem zavaró frissítések érhetők el.</span><span class="sxs-lookup"><span data-stu-id="579c8-114">Regular updates are non-disruptive updates that can be installed when hello device is in Normal mode.</span></span> <span data-ttu-id="579c8-115">Ezek a frissítések hello Microsoft Update webhely tooeach eszköz vezérlő keresztül lettek alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="579c8-115">These updates are applied through hello Microsoft Update website tooeach device controller.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="579c8-116">A vezérlő feladatátvétel hello frissítési folyamat alatt fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="579c8-116">A controller failover may occur during hello update process.</span></span> <span data-ttu-id="579c8-117">Azonban ez nem érinti rendszer rendelkezésre állás vagy a műveletet.</span><span class="sxs-lookup"><span data-stu-id="579c8-117">However, this will not affect system availability or operation.</span></span>
> 
> 

* <span data-ttu-id="579c8-118">További részletek a tooinstall regular frissítéséről hello segítségével a klasszikus Azure portálon: [hello a klasszikus Azure portálon keresztül rendszeres frissítések telepítése](#install-regular-updates-via-the-azure-classic-portal).</span><span class="sxs-lookup"><span data-stu-id="579c8-118">For details on how tooinstall regular updates via hello Azure classic portal, see [Install regular updates via hello Azure classic portal](#install-regular-updates-via-the-azure-classic-portal).</span></span>
* <span data-ttu-id="579c8-119">A StorSimple rendszeres frissítéseket a Windows PowerShell használatával is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="579c8-119">You can also install regular updates via Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="579c8-120">További információkért lásd: [telepítse a Windows PowerShell segítségével rendszeres frissítéseket StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="579c8-120">For details, see [Install regular updates via Windows PowerShell for StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).</span></span>

### <a name="maintenance-mode-updates"></a><span data-ttu-id="579c8-121">Karbantartási mód frissítések</span><span class="sxs-lookup"><span data-stu-id="579c8-121">Maintenance mode updates</span></span>
<span data-ttu-id="579c8-122">Karbantartási mód frissítések például a belső vezérlőprogram-frissítések lemez zavaró frissítések érhetők el.</span><span class="sxs-lookup"><span data-stu-id="579c8-122">Maintenance Mode updates are disruptive updates such as disk firmware upgrades.</span></span> <span data-ttu-id="579c8-123">Ezek a frissítések megkövetelése hello eszköz toobe állítható karbantartási üzemmódba.</span><span class="sxs-lookup"><span data-stu-id="579c8-123">These updates require hello device toobe put into Maintenance mode.</span></span> <span data-ttu-id="579c8-124">További információkért lásd: [2. lépés: Adja meg a karbantartási mód](#step2).</span><span class="sxs-lookup"><span data-stu-id="579c8-124">For details, see [Step 2: Enter Maintenance mode](#step2).</span></span> <span data-ttu-id="579c8-125">Hello Azure klasszikus portál tooinstall karbantartási mód frissítések nem használható.</span><span class="sxs-lookup"><span data-stu-id="579c8-125">You cannot use hello Azure classic portal tooinstall Maintenance mode updates.</span></span> <span data-ttu-id="579c8-126">Ehelyett a StorSimple Windows PowerShell kell használnia.</span><span class="sxs-lookup"><span data-stu-id="579c8-126">Instead, you must use Windows PowerShell for StorSimple.</span></span> 

<span data-ttu-id="579c8-127">További részletek a karbantartási mód tooinstall frissítéséről: [telepítése karbantartási mód frissítések keresztül a Windows PowerShell-lel](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="579c8-127">For details on how tooinstall Maintenance mode updates, see [Install Maintenance mode updates via Windows PowerShell for StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="579c8-128">Karbantartási módban kell lennie külön-külön alkalmazott tooeach vezérlő.</span><span class="sxs-lookup"><span data-stu-id="579c8-128">Maintenance mode updates must be applied separately tooeach controller.</span></span> 
> 
> 

## <a name="install-regular-updates-via-hello-azure-classic-portal"></a><span data-ttu-id="579c8-129">Hello a klasszikus Azure portálon keresztül rendszeres frissítések telepítése</span><span class="sxs-lookup"><span data-stu-id="579c8-129">Install regular updates via hello Azure classic portal</span></span>
<span data-ttu-id="579c8-130">Hello Azure klasszikus portál tooapply frissítések tooyour StorSimple eszközt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="579c8-130">You can use hello Azure classic portal tooapply updates tooyour StorSimple device.</span></span>

[!INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a><span data-ttu-id="579c8-131">Telepítse a Windows PowerShell segítségével rendszeres frissítéseket StorSimple</span><span class="sxs-lookup"><span data-stu-id="579c8-131">Install regular updates via Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="579c8-132">Alternatív megoldásként használható Windows PowerShell StorSimple tooapply (normál mód) rendszeres frissítések.</span><span class="sxs-lookup"><span data-stu-id="579c8-132">Alternatively, you can use Windows PowerShell for StorSimple tooapply regular (Normal mode) updates.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="579c8-133">Bár a Windows PowerShell használatával a StorSimple rendszeres frissítések telepítése erősen ajánlott hello a klasszikus Azure portálon keresztül rendszeres frissítések telepítése.</span><span class="sxs-lookup"><span data-stu-id="579c8-133">Although you can install regular updates using Windows PowerShell for StorSimple, we strongly recommend that you install regular updates through hello Azure classic portal.</span></span> <span data-ttu-id="579c8-134">Frissítés 1-gyel kezdődően előzetes ellenőrzése fogja elvégezni előzetes tooinstalling frissítések hello portálról.</span><span class="sxs-lookup"><span data-stu-id="579c8-134">Beginning with Update 1, pre-checks will be performed prior tooinstalling updates from hello portal.</span></span> <span data-ttu-id="579c8-135">Az előzetes ellenőrzések megelőzik az hibák, és a gördülékenyebb élményt nyújtsanak.</span><span class="sxs-lookup"><span data-stu-id="579c8-135">These pre-checks will preempt failures and ensure a smoother experience.</span></span> 
> 
> 

[!INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a><span data-ttu-id="579c8-136">Telepítse a StorSimple karbantartási mód frissítéseket a Windows PowerShell segítségével</span><span class="sxs-lookup"><span data-stu-id="579c8-136">Install Maintenance mode updates via Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="579c8-137">A Windows PowerShell használata tooapply karbantartási mód frissítések tooyour StorSimple eszközét.</span><span class="sxs-lookup"><span data-stu-id="579c8-137">You use Windows PowerShell for StorSimple tooapply Maintenance mode updates tooyour StorSimple device.</span></span> <span data-ttu-id="579c8-138">Az összes i/o-kérelmek fel van függesztve, ebben a módban.</span><span class="sxs-lookup"><span data-stu-id="579c8-138">All I/O requests are paused in this mode.</span></span> <span data-ttu-id="579c8-139">Például nem felejtő közvetlen elérésű memória (NVRAM) vagy a fürtszolgáltatás hello szolgáltatás is le van állítva.</span><span class="sxs-lookup"><span data-stu-id="579c8-139">Services such as non-volatile random access memory (NVRAM) or hello clustering service are also stopped.</span></span> <span data-ttu-id="579c8-140">Mindkét tartományvezérlők újraindítása van, adja meg, vagy lépjen ki az ebben a módban.</span><span class="sxs-lookup"><span data-stu-id="579c8-140">Both controllers are rebooted when you enter or exit this mode.</span></span> <span data-ttu-id="579c8-141">Ebben a módban való kilépéskor minden hello szolgáltatás folytatódik, és megfelelő kell lennie.</span><span class="sxs-lookup"><span data-stu-id="579c8-141">When you exit this mode, all hello services will resume and should be healthy.</span></span> <span data-ttu-id="579c8-142">(Ez eltarthat néhány percig.)</span><span class="sxs-lookup"><span data-stu-id="579c8-142">(This may take a few minutes.)</span></span>

<span data-ttu-id="579c8-143">Ha tooapply karbantartási mód frissítések van szüksége, kapni fog egy riasztás hello a klasszikus Azure portálon keresztül, hogy rendelkezik-e telepíteni kell.</span><span class="sxs-lookup"><span data-stu-id="579c8-143">If you need tooapply Maintenance mode updates, you will receive an alert through hello Azure classic portal that you have updates that must be installed.</span></span> <span data-ttu-id="579c8-144">Ez a riasztás a StorSimple tooinstall hello frissítéseket a Windows PowerShell használatára vonatkozó utasításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="579c8-144">This alert will include instructions for using Windows PowerShell for StorSimple tooinstall hello updates.</span></span> <span data-ttu-id="579c8-145">Miután frissítette az eszközt, használjon hello azonos eljárás toochange hello eszköz tooRegular mód.</span><span class="sxs-lookup"><span data-stu-id="579c8-145">After you update your device, use hello same procedure toochange hello device tooRegular mode.</span></span> <span data-ttu-id="579c8-146">Részletes útmutatásért lásd: [4. lépés: Kilépés a karbantartási mód](#step4).</span><span class="sxs-lookup"><span data-stu-id="579c8-146">For step-by-step instructions, see [Step 4: Exit Maintenance mode](#step4).</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="579c8-147">Mielőtt karbantartási módba, ellenőrizze, hogy mindkét eszközvezérlők kifogástalan hello ellenőrzésével **hardverállapot** a hello **karbantartási** hello klasszikus Azure portálra a lap.</span><span class="sxs-lookup"><span data-stu-id="579c8-147">Before entering Maintenance mode, verify that both device controllers are healthy by checking hello **Hardware Status** on hello **Maintenance** page in hello Azure classic portal.</span></span> <span data-ttu-id="579c8-148">Ha hello tartományvezérlő nem működik megfelelően, forduljon a Microsoft Support hello további lépéseket.</span><span class="sxs-lookup"><span data-stu-id="579c8-148">If hello controller is not healthy, contact Microsoft Support for hello next steps.</span></span> <span data-ttu-id="579c8-149">További információkért nyissa meg a Microsoft Support tooContact.</span><span class="sxs-lookup"><span data-stu-id="579c8-149">For more information, go tooContact Microsoft Support.</span></span> 
> * <span data-ttu-id="579c8-150">Ha karbantartási módban van, meg kell-e tooapply hello frissítés először egy tartományvezérlő, és majd a többi tartományvezérlő hello.</span><span class="sxs-lookup"><span data-stu-id="579c8-150">When you are in Maintenance mode, you need tooapply hello update first on one controller and then on hello other controller.</span></span>
> 
> 

### <a name="step-1-connect-toohello-serial-console-a-namestep1"></a><span data-ttu-id="579c8-151">1. lépés: Csatlakozás soros konzolon toohello<a name="step1"></span><span class="sxs-lookup"><span data-stu-id="579c8-151">Step 1: Connect toohello serial console <a name="step1"></span></span>
<span data-ttu-id="579c8-152">Például a PuTTY tooaccess hello soros konzol először egy alkalmazás használatát.</span><span class="sxs-lookup"><span data-stu-id="579c8-152">First, use an application such as PuTTY tooaccess hello serial console.</span></span> <span data-ttu-id="579c8-153">hello következő eljárás azt ismerteti, hogyan toouse PuTTY tooconnect toohello soros konzolon.</span><span class="sxs-lookup"><span data-stu-id="579c8-153">hello following procedure explains how toouse PuTTY tooconnect toohello serial console.</span></span>

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a><span data-ttu-id="579c8-154">2. lépés: Adja meg a karbantartási mód<a name="step2"></span><span class="sxs-lookup"><span data-stu-id="579c8-154">Step 2: Enter Maintenance mode <a name="step2"></span></span>
<span data-ttu-id="579c8-155">Toohello konzol csatlakozás után határozza meg, hogy vannak-e frissítések tooinstall, és adja meg a karbantartási mód tooinstall őket.</span><span class="sxs-lookup"><span data-stu-id="579c8-155">After you connect toohello console, determine whether there are updates tooinstall, and enter Maintenance mode tooinstall them.</span></span>

[!INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a><span data-ttu-id="579c8-156">3. lépés: A frissítések telepítése<a name="step3"></span><span class="sxs-lookup"><span data-stu-id="579c8-156">Step 3: Install your updates <a name="step3"></span></span>
<span data-ttu-id="579c8-157">Következő lépésként telepítse a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="579c8-157">Next, install your updates.</span></span>

[!INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]

### <a name="step-4-exit-maintenance-mode-a-namestep4"></a><span data-ttu-id="579c8-158">4. lépés: Kilépés a karbantartási mód<a name="step4"></span><span class="sxs-lookup"><span data-stu-id="579c8-158">Step 4: Exit Maintenance mode <a name="step4"></span></span>
<span data-ttu-id="579c8-159">Végezetül kilép karbantartási módból.</span><span class="sxs-lookup"><span data-stu-id="579c8-159">Finally, exit Maintenance mode.</span></span>

[!INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a><span data-ttu-id="579c8-160">A StorSimple gyorsjavítások Windows PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="579c8-160">Install hotfixes via Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="579c8-161">Frissítések a Microsoft Azure StorSimple, ellentétben a gyorsjavítás telepítve van egy megosztott mappából.</span><span class="sxs-lookup"><span data-stu-id="579c8-161">Unlike updates for Microsoft Azure StorSimple, hotfixes are installed from a shared folder.</span></span> <span data-ttu-id="579c8-162">Csakúgy, mint a frissítések két típusa van gyorsjavítások:</span><span class="sxs-lookup"><span data-stu-id="579c8-162">As with updates, there are two types of hotfixes:</span></span> 

* <span data-ttu-id="579c8-163">Rendszeres gyorsjavítások</span><span class="sxs-lookup"><span data-stu-id="579c8-163">Regular hotfixes</span></span> 
* <span data-ttu-id="579c8-164">Karbantartási mód gyorsjavítások</span><span class="sxs-lookup"><span data-stu-id="579c8-164">Maintenance mode hotfixes</span></span>  

<span data-ttu-id="579c8-165">hello alábbi eljárások azt ismertetik, hogyan toouse Windows PowerShell StorSimple tooinstall rendszeres és karbantartási mód gyorsjavítások.</span><span class="sxs-lookup"><span data-stu-id="579c8-165">hello following procedures explain how toouse Windows PowerShell for StorSimple tooinstall regular and Maintenance mode hotfixes.</span></span>

[!INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[!INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-tooupdates-if-you-perform-a-factory-reset-of-hello-device"></a><span data-ttu-id="579c8-166">Mi történik, ha visszaállítsa a gyári tooupdates hello eszköz alaphelyzetbe állítása?</span><span class="sxs-lookup"><span data-stu-id="579c8-166">What happens tooupdates if you perform a factory reset of hello device?</span></span>
<span data-ttu-id="579c8-167">Ha egy eszköz alaphelyzetbe állítása toofactory beállításokat, majd minden hello frissítések elvesznek.</span><span class="sxs-lookup"><span data-stu-id="579c8-167">If a device is reset toofactory settings, then all hello updates are lost.</span></span> <span data-ttu-id="579c8-168">Miután hello gyári alaphelyzetbe történő visszaállításhoz eszköz regisztrálva, és konfigurálva van, szüksége lesz StorSimple toomanually frissítéseinek telepítése a klasszikus Azure portálon hello és/vagy a Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="579c8-168">After hello factory-reset device is registered and configured, you will need toomanually install updates through hello Azure classic portal and/or Windows PowerShell for StorSimple.</span></span> <span data-ttu-id="579c8-169">További információ a gyári beállítások visszaállítása: [hello eszköz toofactory alapértelmezett beállításainak visszaállítása](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="579c8-169">For more information about factory reset, see [Reset hello device toofactory default settings](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>

## <a name="next-steps"></a><span data-ttu-id="579c8-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="579c8-170">Next steps</span></span>
* <span data-ttu-id="579c8-171">További információ [Windows PowerShell használatával a StorSimple tooadminister a StorSimple eszköz](storsimple-windows-powershell-administration.md).</span><span class="sxs-lookup"><span data-stu-id="579c8-171">Learn more about [using Windows PowerShell for StorSimple tooadminister your StorSimple device](storsimple-windows-powershell-administration.md).</span></span>
* <span data-ttu-id="579c8-172">További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="579c8-172">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

