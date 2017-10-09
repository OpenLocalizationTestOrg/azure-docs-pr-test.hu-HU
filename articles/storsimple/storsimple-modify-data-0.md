---
title: "DATA 0 aaaModify hello a StorSimple eszközön található beállítások |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Windows PowerShell a StorSimple tooreconfigure hello a StorSimple eszköz DATA 0 hálózati adapterén."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 58e3d509-f425-4a7f-b650-d496a7c85193
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: caec51c3344d953299253301c2a0d7577d553c6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="modify-hello-data-0-network-interface-settings-on-your-storsimple-device"></a><span data-ttu-id="980cd-103">A StorSimple eszköz hello DATA 0 hálózati kapcsolati beállítások módosítása</span><span class="sxs-lookup"><span data-stu-id="980cd-103">Modify hello DATA 0 network interface settings on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="980cd-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="980cd-104">Overview</span></span>
<span data-ttu-id="980cd-105">A Microsoft Azure StorSimple eszköz rendelkezik hat hálózati adapterrel, a DATA 0 tooDATA 5.</span><span class="sxs-lookup"><span data-stu-id="980cd-105">Your Microsoft Azure StorSimple device has six network interfaces, from DATA 0 tooDATA 5.</span></span> <span data-ttu-id="980cd-106">hello DATA 0 felület mindig hello Windows PowerShell felületén vagy a soros konzol hello konfigurálva, és automatikusan felhő engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="980cd-106">hello DATA 0 interface is always configured through hello Windows PowerShell interface or hello serial console, and is automatically cloud-enabled.</span></span> <span data-ttu-id="980cd-107">Vegye figyelembe, hogy nem konfigurálhat DATA 0 hálózati adapterén hello a klasszikus Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="980cd-107">Note that you cannot configure DATA 0 network interface through hello Azure classic portal.</span></span> 

<span data-ttu-id="980cd-108">hello DATA 0 felület először konfigurálva hello beállítása varázsló hello StorSimple eszköz kezdeti telepítése során.</span><span class="sxs-lookup"><span data-stu-id="980cd-108">hello DATA 0 interface is first configured through hello setup wizard during initial deployment of hello StorSimple device.</span></span> <span data-ttu-id="980cd-109">Olyan működési mód hello eszköz esetén szükség lehet a DATA 0 tooreconfigure beállításait.</span><span class="sxs-lookup"><span data-stu-id="980cd-109">When hello device is in an operational mode, you may need tooreconfigure DATA 0 settings.</span></span> <span data-ttu-id="980cd-110">Ez az oktatóanyag toomodify DATA 0 hálózati beállítások, mindkettő a Windows PowerShell-lel két módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="980cd-110">This tutorial provides two methods toomodify DATA 0 network settings, both through Windows PowerShell for StorSimple.</span></span>

<span data-ttu-id="980cd-111">Ez az oktatóanyag elolvasása, után fogja tudni:</span><span class="sxs-lookup"><span data-stu-id="980cd-111">After reading this tutorial, you will be able to:</span></span>

* <span data-ttu-id="980cd-112">Módosítsa a DATA 0 hálózati beállítás keresztül hello beállítása varázsló</span><span class="sxs-lookup"><span data-stu-id="980cd-112">Modify DATA 0 network setting through hello setup wizard</span></span>
* <span data-ttu-id="980cd-113">Módosítsa a DATA 0 hálózati beállítások hello `Set-HcsNetInterface` parancsmag</span><span class="sxs-lookup"><span data-stu-id="980cd-113">Modify DATA 0 network settings through hello `Set-HcsNetInterface` cmdlet</span></span>

## <a name="modify-data-0-network-settings-through-setup-wizard"></a><span data-ttu-id="980cd-114">A telepítő varázsló lépéseinek DATA 0 hálózati beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="980cd-114">Modify DATA 0 network settings through setup wizard</span></span>
<span data-ttu-id="980cd-115">DATA 0 hálózati beállítások újrakonfigurálhatja toohello Windows PowerShell felületet a StorSimple eszköz csatlakoztatása és a telepítő varázsló munkamenet elindításához.</span><span class="sxs-lookup"><span data-stu-id="980cd-115">You can reconfigure DATA 0 network settings by connecting toohello Windows PowerShell interface of your StorSimple device and launching a setup wizard session.</span></span> <span data-ttu-id="980cd-116">Hajtsa végre a következő lépéseket toomodify DATA 0 hello beállítások:</span><span class="sxs-lookup"><span data-stu-id="980cd-116">Perform hello following steps toomodify DATA 0 settings:</span></span>

#### <a name="toomodify-data-0-network-settings-through-setup-wizard"></a><span data-ttu-id="980cd-117">toomodify DATA 0 hálózati beállítások beállítása varázsló</span><span class="sxs-lookup"><span data-stu-id="980cd-117">toomodify DATA 0 network settings through setup wizard</span></span>
1. <span data-ttu-id="980cd-118">Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="980cd-118">In hello serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="980cd-119">Amikor a rendszer kéri adja meg a hello **eszköz rendszergazdai jelszava**.</span><span class="sxs-lookup"><span data-stu-id="980cd-119">When prompted provide hello **device administrator password**.</span></span> <span data-ttu-id="980cd-120">hello alapértelmezett jelszó `Password1`.</span><span class="sxs-lookup"><span data-stu-id="980cd-120">hello default password is `Password1`.</span></span>
2. <span data-ttu-id="980cd-121">Hello parancsot, írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="980cd-121">At hello command prompt, type:</span></span>
   
    `Invoke-HcsSetupWizard`
3. <span data-ttu-id="980cd-122">A telepítő varázsló jelenik meg toohelp DATA 0 hello konfigurálása az eszköz felületén.</span><span class="sxs-lookup"><span data-stu-id="980cd-122">A setup wizard will appear toohelp you configure hello DATA 0 interface of your device.</span></span> <span data-ttu-id="980cd-123">Adjon meg új értékeket hello IP-cím, az átjáró és a hálózati maszk.</span><span class="sxs-lookup"><span data-stu-id="980cd-123">Provide new values for hello IP address, gateway, and netmask.</span></span>

> [!NOTE]
> <span data-ttu-id="980cd-124">Hello, tartományvezérlői IP-címet kell újrakonfigurálni keresztül hello toobe rögzített **konfigurálása** hello StorSimple eszközt a klasszikus Azure portálon hello oldalán.</span><span class="sxs-lookup"><span data-stu-id="980cd-124">hello fixed controllers IPs will need toobe reconfigured through hello **Configure** page of hello StorSimple device in hello Azure classic portal.</span></span> <span data-ttu-id="980cd-125">További információ: túl[módosítása a hálózati adapterek](storsimple-modify-device-config.md#modify-network-interfaces).</span><span class="sxs-lookup"><span data-stu-id="980cd-125">For more information, go too[Modify network interfaces](storsimple-modify-device-config.md#modify-network-interfaces).</span></span>
> 
> 

## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a><span data-ttu-id="980cd-126">Set-HcsNetInterface parancsmaggal DATA 0 hálózati beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="980cd-126">Modify DATA 0 network settings through Set-HcsNetInterface cmdlet</span></span>
<span data-ttu-id="980cd-127">Egy másik módja tooreconfigure DATA 0 hálózati adapter van hello hello használata `Set-HcsNetInterface` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="980cd-127">An alternate way tooreconfigure DATA 0 network interface is through hello use of  hello `Set-HcsNetInterface` cmdlet.</span></span> <span data-ttu-id="980cd-128">a StorSimple eszköz hello Windows PowerShell felhasználói felületen keresztüli hello parancsmag végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="980cd-128">hello cmdlet is executed from hello Windows PowerShell interface of your StorSimple device.</span></span> <span data-ttu-id="980cd-129">Ezzel az eljárással hello vezérlő rögzített IP-címei is konfigurálható itt.</span><span class="sxs-lookup"><span data-stu-id="980cd-129">When using this procedure, hello controller fixed IPs can also be configured here.</span></span> <span data-ttu-id="980cd-130">Hajtsa végre a következő lépéseket toomodify hello DATA 0 hello beállítások:</span><span class="sxs-lookup"><span data-stu-id="980cd-130">Perform hello following steps toomodify hello DATA 0 settings:</span></span> 

#### <a name="toomodify-data-0-network-settings-through-hello-set-hcsnetinterface-cmdlet"></a><span data-ttu-id="980cd-131">hello Set-HcsNetInterface parancsmag toomodify DATA 0 hálózati beállítások</span><span class="sxs-lookup"><span data-stu-id="980cd-131">toomodify DATA 0 network settings through hello Set-HcsNetInterface cmdlet</span></span>
1. <span data-ttu-id="980cd-132">Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="980cd-132">In hello serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="980cd-133">Amikor a rendszer kéri hello eszköz rendszergazdai jelszava adja meg.</span><span class="sxs-lookup"><span data-stu-id="980cd-133">When prompted provide hello device administrator password.</span></span> <span data-ttu-id="980cd-134">hello alapértelmezett jelszó `Password1`.</span><span class="sxs-lookup"><span data-stu-id="980cd-134">hello default password is `Password1`.</span></span>
2. <span data-ttu-id="980cd-135">Hello parancsot, írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="980cd-135">At hello command prompt, type:</span></span>
   
    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
   
    <span data-ttu-id="980cd-136">Hello dőlt zárójelek közé írja be a következő értékeket a DATA 0 hello:</span><span class="sxs-lookup"><span data-stu-id="980cd-136">In hello angled brackets, type hello following values for DATA 0:</span></span>
   
   * <span data-ttu-id="980cd-137">IPv4-cím</span><span class="sxs-lookup"><span data-stu-id="980cd-137">IPv4 address</span></span>
   * <span data-ttu-id="980cd-138">IPv4-átjáró</span><span class="sxs-lookup"><span data-stu-id="980cd-138">IPv4 gateway</span></span>
   * <span data-ttu-id="980cd-139">IPv4 alhálózati maszk</span><span class="sxs-lookup"><span data-stu-id="980cd-139">IPv4 subnet mask</span></span>
   * <span data-ttu-id="980cd-140">0. vezérlő rögzített IPv4-cím</span><span class="sxs-lookup"><span data-stu-id="980cd-140">Fixed IPv4 address for Controller 0</span></span>
   * <span data-ttu-id="980cd-141">A vezérlő 1 fix IPv4-cím</span><span class="sxs-lookup"><span data-stu-id="980cd-141">Fixed IPv4 address for Controller 1</span></span>
     
     <span data-ttu-id="980cd-142">További információ a hello használata parancsmag: túl[StorSimple parancsmag-referencia a Windows PowerShell](https://technet.microsoft.com/library/dn688161.aspx).</span><span class="sxs-lookup"><span data-stu-id="980cd-142">For more information on hello use of this cmdlet, go too[Windows PowerShell for StorSimple cmdlet reference](https://technet.microsoft.com/library/dn688161.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="980cd-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="980cd-143">Next steps</span></span>
* <span data-ttu-id="980cd-144">DATA 0 kivételével tooconfigure hálózati – hello használható [konfigurációs lapján, a klasszikus Azure portálon hello](storsimple-modify-device-config.md).</span><span class="sxs-lookup"><span data-stu-id="980cd-144">tooconfigure network interfaces other than DATA 0, you can use hello [Configure page in hello Azure classic portal](storsimple-modify-device-config.md).</span></span> 
* <span data-ttu-id="980cd-145">Ha probléma merül fel a hálózati adapterek konfigurálásakor, tekintse meg a túl[telepítési problémák elhárításához](storsimple-troubleshoot-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="980cd-145">If you experience any issues when configuring your network interfaces, refer too[Troubleshoot deployment issues](storsimple-troubleshoot-deployment.md).</span></span>

