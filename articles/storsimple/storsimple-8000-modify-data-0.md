---
title: "DATA 0 módosítása a StorSimple 8000 series eszközön található beállítások |} Microsoft Docs"
description: "Ismerje meg, hogyan használható Windows PowerShell-lel konfigurálja újra a StorSimple eszköz DATA 0 hálózati adapterén."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: 90df43e22f17fd32fe642514df098b72700e77af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="modify-the-data-0-network-interface-settings-on-your-storsimple-8000-series-device"></a><span data-ttu-id="ea6f9-103">A DATA 0 hálózati kapcsolati beállítások a StorSimple 8000 series eszközön módosítása</span><span class="sxs-lookup"><span data-stu-id="ea6f9-103">Modify the DATA 0 network interface settings on your StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="ea6f9-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ea6f9-104">Overview</span></span>

<span data-ttu-id="ea6f9-105">A Microsoft Azure StorSimple eszköz hat hálózati adapterrel, a DATA 0 DATA 5 rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-105">Your Microsoft Azure StorSimple device has six network interfaces, from DATA 0 to DATA 5.</span></span> <span data-ttu-id="ea6f9-106">A DATA 0 felület mindig konfigurálva van a Windows PowerShell felületéről, sem a soros konzolon keresztül, és automatikusan felhő-kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-106">The DATA 0 interface is always configured through the Windows PowerShell interface or the serial console, and is automatically cloud-enabled.</span></span> <span data-ttu-id="ea6f9-107">Vegye figyelembe, hogy nem konfigurálhat az Azure portálon keresztül DATA 0 hálózati adapterén.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-107">Note that you cannot configure DATA 0 network interface through the Azure portal.</span></span>

<span data-ttu-id="ea6f9-108">A DATA 0, a telepítővarázslót során először konfigurálja a felület telepítési a StorSimple eszköz kezdeti.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-108">The DATA 0 interface is first configured through the setup wizard during initial deployment of the StorSimple device.</span></span> <span data-ttu-id="ea6f9-109">Ha az eszköz egy üzemeltetési módban van, szükség lehet, hogy engedélyezzék a DATA 0 beállításait.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-109">When the device is in an operational mode, you may need to reconfigure DATA 0 settings.</span></span> <span data-ttu-id="ea6f9-110">Ez az oktatóanyag biztosít két módszer módosításához a DATA 0 hálózati beállításai, mind a Windows PowerShell-lel.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-110">This tutorial provides two methods to modify DATA 0 network settings, both through Windows PowerShell for StorSimple.</span></span>

<span data-ttu-id="ea6f9-111">Ez az oktatóanyag elolvasása, után fogja tudni:</span><span class="sxs-lookup"><span data-stu-id="ea6f9-111">After reading this tutorial, you will be able to:</span></span>

* <span data-ttu-id="ea6f9-112">Módosítsa a DATA 0 hálózati beállítás a telepítővarázslót</span><span class="sxs-lookup"><span data-stu-id="ea6f9-112">Modify DATA 0 network setting through the setup wizard</span></span>
* <span data-ttu-id="ea6f9-113">DATA 0 hálózati beállítások módosítása a `Set-HcsNetInterface` parancsmag</span><span class="sxs-lookup"><span data-stu-id="ea6f9-113">Modify DATA 0 network settings through the `Set-HcsNetInterface` cmdlet</span></span>

## <a name="modify-data-0-network-settings-through-setup-wizard"></a><span data-ttu-id="ea6f9-114">A telepítő varázsló lépéseinek DATA 0 hálózati beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="ea6f9-114">Modify DATA 0 network settings through setup wizard</span></span>
<span data-ttu-id="ea6f9-115">Olyan módon konfigurálhatja újra DATA 0 hálózati beállítások a Windows PowerShell-felületet a StorSimple eszköz csatlakozik, és a telepítő varázsló munkamenet elindításához.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-115">You can reconfigure DATA 0 network settings by connecting to the Windows PowerShell interface of your StorSimple device and launching a setup wizard session.</span></span> <span data-ttu-id="ea6f9-116">A következő lépésekkel módosíthatja a DATA 0 beállítások:</span><span class="sxs-lookup"><span data-stu-id="ea6f9-116">Perform the following steps to modify DATA 0 settings:</span></span>

#### <a name="to-modify-data-0-network-settings-through-setup-wizard"></a><span data-ttu-id="ea6f9-117">A telepítő varázsló lépéseinek DATA 0 hálózati beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="ea6f9-117">To modify DATA 0 network settings through setup wizard</span></span>
1. <span data-ttu-id="ea6f9-118">A soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-118">In the serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="ea6f9-119">Amikor a rendszer kéri adja meg a **eszköz rendszergazdai jelszava**.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-119">When prompted provide the **device administrator password**.</span></span> <span data-ttu-id="ea6f9-120">Az alapértelmezett jelszó `Password1`.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-120">The default password is `Password1`.</span></span>
2. <span data-ttu-id="ea6f9-121">A parancssorba írja be:</span><span class="sxs-lookup"><span data-stu-id="ea6f9-121">At the command prompt, type:</span></span>
   
    `Invoke-HcsSetupWizard`
3. <span data-ttu-id="ea6f9-122">Konfigurálhatja a DATA 0 megjelenik egy telepítővarázsló az eszköz felületén.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-122">A setup wizard appears to help configure the DATA 0 interface of your device.</span></span> <span data-ttu-id="ea6f9-123">Adjon meg új értékeket az IP cím, az átjáró és a hálózati maszk.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-123">Provide new values for the IP address, gateway, and netmask.</span></span>

> [!NOTE]
> <span data-ttu-id="ea6f9-124">A rögzített vezérlők IP-címek használatával újra kell konfigurálni kell a **hálózati beállításai** panel a StorSimple eszköz az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-124">The fixed controllers IPs will need to be reconfigured through the **Network settings** blade of the StorSimple device in the Azure portal.</span></span> <span data-ttu-id="ea6f9-125">További információkért látogasson el [módosítása a hálózati adapterek](storsimple-8000-modify-device-config.md#modify-network-interfaces).</span><span class="sxs-lookup"><span data-stu-id="ea6f9-125">For more information, go to [Modify network interfaces](storsimple-8000-modify-device-config.md#modify-network-interfaces).</span></span>

## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a><span data-ttu-id="ea6f9-126">Set-HcsNetInterface parancsmaggal DATA 0 hálózati beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="ea6f9-126">Modify DATA 0 network settings through Set-HcsNetInterface cmdlet</span></span>
<span data-ttu-id="ea6f9-127">Konfigurálja újra a DATA 0 hálózati adapter használatával megadva is a `Set-HcsNetInterface` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-127">An alternate way to reconfigure DATA 0 network interface is through the use of the `Set-HcsNetInterface` cmdlet.</span></span> <span data-ttu-id="ea6f9-128">A parancsmag végrehajtásának a Windows PowerShell-felületet a StorSimple eszköz.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-128">The cmdlet is executed from the Windows PowerShell interface of your StorSimple device.</span></span> <span data-ttu-id="ea6f9-129">Ez az eljárás használata esetén a vezérlő fix IP-címek is konfigurálható itt.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-129">When using this procedure, the controller fixed IPs can also be configured here.</span></span> <span data-ttu-id="ea6f9-130">A következő lépésekkel módosíthatja az adatokat 0 beállítások:</span><span class="sxs-lookup"><span data-stu-id="ea6f9-130">Perform the following steps to modify the DATA 0 settings:</span></span> 

#### <a name="to-modify-data-0-network-settings-through-the-set-hcsnetinterface-cmdlet"></a><span data-ttu-id="ea6f9-131">A Set-HcsNetInterface parancsmaggal DATA 0 hálózati beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="ea6f9-131">To modify DATA 0 network settings through the Set-HcsNetInterface cmdlet</span></span>
1. <span data-ttu-id="ea6f9-132">A soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-132">In the serial console menu, choose option 1, **Log in with full access**.</span></span> <span data-ttu-id="ea6f9-133">Amikor a rendszer kéri adja meg az eszköz rendszergazdai jelszava.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-133">When prompted provide the device administrator password.</span></span> <span data-ttu-id="ea6f9-134">Az alapértelmezett jelszó `Password1`.</span><span class="sxs-lookup"><span data-stu-id="ea6f9-134">The default password is `Password1`.</span></span>
2. <span data-ttu-id="ea6f9-135">A parancssorba írja be:</span><span class="sxs-lookup"><span data-stu-id="ea6f9-135">At the command prompt, type:</span></span>
   
    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
   
    <span data-ttu-id="ea6f9-136">A csúcsos zárójelek közé írja be a következő értékeket a DATA 0:</span><span class="sxs-lookup"><span data-stu-id="ea6f9-136">In the angled brackets, type the following values for DATA 0:</span></span>
   
   * <span data-ttu-id="ea6f9-137">IPv4-cím</span><span class="sxs-lookup"><span data-stu-id="ea6f9-137">IPv4 address</span></span>
   * <span data-ttu-id="ea6f9-138">IPv4-átjáró</span><span class="sxs-lookup"><span data-stu-id="ea6f9-138">IPv4 gateway</span></span>
   * <span data-ttu-id="ea6f9-139">IPv4 alhálózati maszk</span><span class="sxs-lookup"><span data-stu-id="ea6f9-139">IPv4 subnet mask</span></span>
   * <span data-ttu-id="ea6f9-140">0. vezérlő rögzített IPv4-cím</span><span class="sxs-lookup"><span data-stu-id="ea6f9-140">Fixed IPv4 address for Controller 0</span></span>
   * <span data-ttu-id="ea6f9-141">A vezérlő 1 fix IPv4-cím</span><span class="sxs-lookup"><span data-stu-id="ea6f9-141">Fixed IPv4 address for Controller 1</span></span>
     
     <span data-ttu-id="ea6f9-142">További információt ennek a parancsmagnak [StorSimple parancsmag-referencia a Windows PowerShell](https://technet.microsoft.com/library/dn688161.aspx).</span><span class="sxs-lookup"><span data-stu-id="ea6f9-142">For more information on the use of this cmdlet, go to [Windows PowerShell for StorSimple cmdlet reference](https://technet.microsoft.com/library/dn688161.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ea6f9-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ea6f9-143">Next steps</span></span>
* <span data-ttu-id="ea6f9-144">DATA 0 kivételével a hálózati adapterek konfigurálásához használja a [hálózati beállítások konfigurálása az Azure-portálon a](storsimple-8000-modify-device-config.md).</span><span class="sxs-lookup"><span data-stu-id="ea6f9-144">To configure network interfaces other than DATA 0, you can use the [Configure network settings in the Azure portal](storsimple-8000-modify-device-config.md).</span></span> 
* <span data-ttu-id="ea6f9-145">Ha a hálózati adapterek konfigurálásakor esetleges problémákat tapasztal, tekintse meg a [telepítési problémák elhárításához](storsimple-troubleshoot-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="ea6f9-145">If you experience any issues when configuring your network interfaces, refer to [Troubleshoot deployment issues](storsimple-troubleshoot-deployment.md).</span></span>

