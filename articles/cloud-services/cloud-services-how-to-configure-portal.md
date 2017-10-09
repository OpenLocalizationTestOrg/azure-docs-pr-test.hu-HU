---
title: "a felhőszolgáltatás (portál) aaaHow tooconfigure |} Microsoft Docs"
description: "Ismerje meg, hogy miként tooconfigure felhőalapú szolgáltatásokat az Azure-ban. Ismerje meg, tooupdate hello felhőalapú szolgáltatás konfigurációja, és konfigurálja a távelérés toorole példányok. Ezekben a példákban hello Azure-portálon."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 7308f3c0-825e-499d-bfa5-c60f86371921
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: adegeo
ms.openlocfilehash: 969a08558473e8c79153192942bfda587eb5ada5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-cloud-services"></a><span data-ttu-id="dbdce-105">TooConfigure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="dbdce-105">How tooConfigure Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dbdce-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="dbdce-106">Azure portal</span></span>](cloud-services-how-to-configure-portal.md)
> * [<span data-ttu-id="dbdce-107">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="dbdce-107">Azure classic portal</span></span>](cloud-services-how-to-configure.md)
>
>

<span data-ttu-id="dbdce-108">Konfigurálhatja a felhőszolgáltatás hello leggyakrabban használt beállításait a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="dbdce-108">You can configure hello most commonly used settings for a cloud service in hello Azure portal.</span></span> <span data-ttu-id="dbdce-109">Vagy, tetszés szerint tooupdate a konfigurációs fájlok közvetlen, töltse le a szolgáltatás konfigurációs fájl tooupdate, majd töltse fel a frissített hello fájl- és frissítési hello felhőszolgáltatás hello konfigurációs módosítások.</span><span class="sxs-lookup"><span data-stu-id="dbdce-109">Or, if you like tooupdate your configuration files directly, download a service configuration file tooupdate, and then upload hello updated file and update hello cloud service with hello configuration changes.</span></span> <span data-ttu-id="dbdce-110">Mindkét módszer esetén tooall szerepkörpéldányokat hello konfigurációfrissítések vannak leküldött.</span><span class="sxs-lookup"><span data-stu-id="dbdce-110">Either way, hello configuration updates are pushed out tooall role instances.</span></span>

<span data-ttu-id="dbdce-111">Emellett kezelheti hello példányait a felhőszolgáltatás szerepköreit, vagy a távoli asztal be őket.</span><span class="sxs-lookup"><span data-stu-id="dbdce-111">You can also manage hello instances of your cloud service roles, or remote desktop into them.</span></span>

<span data-ttu-id="dbdce-112">Azure is csak 99,95 % szolgáltatás rendelkezésre állásának biztosításához során hello konfigurációfrissítések ha legalább két szerepkör minden egyes szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="dbdce-112">Azure can only ensure 99.95 percent service availability during hello configuration updates if you have at least two role instances for every role.</span></span> <span data-ttu-id="dbdce-113">Amely lehetővé teszi, hogy egy virtuális gép tooprocess ügyfélkérelmek más hello frissítése közben.</span><span class="sxs-lookup"><span data-stu-id="dbdce-113">That enables one virtual machine tooprocess client requests while hello other is being updated.</span></span> <span data-ttu-id="dbdce-114">További információkért lásd: [szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="dbdce-114">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="change-a-cloud-service"></a><span data-ttu-id="dbdce-115">Egy felhőalapú szolgáltatás módosítása</span><span class="sxs-lookup"><span data-stu-id="dbdce-115">Change a cloud service</span></span>
<span data-ttu-id="dbdce-116">Hello megnyitása után [Azure-portálon](https://portal.azure.com/), keresse meg a tooyour felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="dbdce-116">After opening hello [Azure portal](https://portal.azure.com/), navigate tooyour cloud service.</span></span> <span data-ttu-id="dbdce-117">Itt azt sok szempontból kezelhető.</span><span class="sxs-lookup"><span data-stu-id="dbdce-117">From here, you manage many aspects of it.</span></span>

![Beállítások lap](./media/cloud-services-how-to-configure-portal/cloud-service.png)

<span data-ttu-id="dbdce-119">Hello **beállítások** vagy **összes beállítás** hivatkozásokra kattintva megnyílik hello **beállítások** panel, ahol módosíthatók a hello **tulajdonságok**, hello módosítása **Konfigurációs**, hello kezelése **tanúsítványok**, a telepítő **riasztási szabályok**, és kezelheti a hello **felhasználók** hozzáféréssel rendelkező a felhőalapú szolgáltatás toothis.</span><span class="sxs-lookup"><span data-stu-id="dbdce-119">hello **Settings** or **All settings** links will open up hello **Settings** blade where you can change hello **Properties**, change hello **Configuration**, manage hello **Certificates**, setup **Alert rules**, and manage hello **Users** who have access toothis cloud service.</span></span>

![Azure cloud service beállítások panel](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a><span data-ttu-id="dbdce-121">Kezelheti a vendég operációs rendszer verziója</span><span class="sxs-lookup"><span data-stu-id="dbdce-121">Manage Guest OS version</span></span>

<span data-ttu-id="dbdce-122">Alapértelmezés szerint Azure rendszeres időközönként frissíti a vendég operációs rendszer toohello legújabb támogatott képet hello a szolgáltatás konfigurációs (.cscfg), a megadott operációsrendszer-termékcsalád például a Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="dbdce-122">By default, Azure periodically updates your guest OS toohello latest supported image within hello OS family that you've specified in your service configuration (.cscfg), such as Windows Server 2016.</span></span>

<span data-ttu-id="dbdce-123">Ha egy adott operációs rendszer verziója tootarget van szüksége, beállíthatja azt hello **konfigurációs** panelen.</span><span class="sxs-lookup"><span data-stu-id="dbdce-123">If you need tootarget a specific OS version, you can set it in hello **Configuration** blade.</span></span>

![Az operációs rendszer verzió beállítása](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)


>[!IMPORTANT]
> <span data-ttu-id="dbdce-125">Egy adott operációs rendszer verziója letiltja az operációs rendszer automatikus kiválasztása frissíti, és lehetővé teszi a felhasználó felelőssége javítását.</span><span class="sxs-lookup"><span data-stu-id="dbdce-125">Choosing a specific OS version disables automatic OS updates and makes patching your responsibility.</span></span> <span data-ttu-id="dbdce-126">Gondoskodnia kell arról, hogy a szerepkörpéldányok frissítések kap, vagy az alkalmazás toosecurity biztonsági rések tehetik közzé.</span><span class="sxs-lookup"><span data-stu-id="dbdce-126">You must ensure that your role instances are receiving updates or you may expose your application toosecurity vulnerabilities.</span></span>

## <a name="monitoring"></a><span data-ttu-id="dbdce-127">Figyelés</span><span class="sxs-lookup"><span data-stu-id="dbdce-127">Monitoring</span></span>
<span data-ttu-id="dbdce-128">Riasztások tooyour felhőalapú szolgáltatás is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="dbdce-128">You can add alerts tooyour cloud service.</span></span> <span data-ttu-id="dbdce-129">Kattintson a **beállítások** > **riasztás szabályok** > **riasztás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="dbdce-129">Click **Settings** > **Alert Rules** > **Add alert**.</span></span>

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

<span data-ttu-id="dbdce-130">Itt beállíthatja egy riasztást.</span><span class="sxs-lookup"><span data-stu-id="dbdce-130">From here, you can setup an alert.</span></span> <span data-ttu-id="dbdce-131">A hello **metrika** legördülő listája, beállíthatja a következő típusú adatok hello egy riasztást.</span><span class="sxs-lookup"><span data-stu-id="dbdce-131">With hello **Metric** drop down box, you can setup an alert for hello following types of data.</span></span>

* <span data-ttu-id="dbdce-132">Lemez sebessége olvasott</span><span class="sxs-lookup"><span data-stu-id="dbdce-132">Disk read</span></span>
* <span data-ttu-id="dbdce-133">Lemez írási</span><span class="sxs-lookup"><span data-stu-id="dbdce-133">Disk write</span></span>
* <span data-ttu-id="dbdce-134">A hálózati</span><span class="sxs-lookup"><span data-stu-id="dbdce-134">Network in</span></span>
* <span data-ttu-id="dbdce-135">Kimenő hálózati</span><span class="sxs-lookup"><span data-stu-id="dbdce-135">Network out</span></span>
* <span data-ttu-id="dbdce-136">Processzorhasználat (%)</span><span class="sxs-lookup"><span data-stu-id="dbdce-136">CPU percentage</span></span>

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a><span data-ttu-id="dbdce-137">Egy mérték csempe a figyelés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dbdce-137">Configure monitoring from a metric tile</span></span>
<span data-ttu-id="dbdce-138">Helyett **beállítások** > **riasztási szabályok**, rákattinthat a hello metrika csempék hello az egyik **figyelés** hello szakasza **felhő szolgáltatás** panelen.</span><span class="sxs-lookup"><span data-stu-id="dbdce-138">Instead of using **Settings** > **Alert Rules**, you can click on one of hello metric tiles in hello **Monitoring** section of hello **Cloud service** blade.</span></span>

![A felhőalapú szolgáltatás figyelése](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

<span data-ttu-id="dbdce-140">Itt hello csempe használt hello diagram testreszabása, vagy vegye fel a riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="dbdce-140">From here you can customize hello chart used with hello tile, or add an alert rule.</span></span>

## <a name="reboot-reimage-or-remote-desktop"></a><span data-ttu-id="dbdce-141">Rendszer újraindítása, a lemezkép-visszaállítási vagy a távoli asztal</span><span class="sxs-lookup"><span data-stu-id="dbdce-141">Reboot, reimage, or remote desktop</span></span>
<span data-ttu-id="dbdce-142">Jelenleg nem lehet konfigurálni a távoli asztal használatával hello **Azure-portálon**.</span><span class="sxs-lookup"><span data-stu-id="dbdce-142">At this time you cannot configure remote desktop using hello **Azure portal**.</span></span> <span data-ttu-id="dbdce-143">Azonban beállíthatja azt keresztül hello [a klasszikus Azure portálon](cloud-services-role-enable-remote-desktop.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), illetve [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="dbdce-143">However, you can set it up through hello [Azure classic portal](cloud-services-role-enable-remote-desktop.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), or through [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).</span></span>

<span data-ttu-id="dbdce-144">Első lépésként kattintson a hello cloud service-példány.</span><span class="sxs-lookup"><span data-stu-id="dbdce-144">First, click on hello cloud service instance.</span></span>

![Cloud Service-példány](./media/cloud-services-how-to-configure-portal/cs-instance.png)

<span data-ttu-id="dbdce-146">A hello panel, amely megnyitja azt a távoli asztali kapcsolat kezdeményezéséhez, távolról újraindítás hello, vagy távolról lemezkép-visszaállítási (kezdő friss képének) hello példány.</span><span class="sxs-lookup"><span data-stu-id="dbdce-146">From hello blade that opens you can initiate a remote desktop connection, remotely reboot hello instance, or remotely reimage (start with a fresh image) hello instance.</span></span>

![Cloud Service példány gombok](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a><span data-ttu-id="dbdce-148">Konfigurálja újra a .cscfg</span><span class="sxs-lookup"><span data-stu-id="dbdce-148">Reconfigure your .cscfg</span></span>
<span data-ttu-id="dbdce-149">Szükség lehet tooreconfigure hello segítségével a felhőalapú szolgáltatás [szolgáltatás konfigurációs (szolgáltatáskonfigurációs séma)](cloud-services-model-and-package.md#cscfg) fájlt.</span><span class="sxs-lookup"><span data-stu-id="dbdce-149">You may need tooreconfigure your cloud service through hello [service config (cscfg)](cloud-services-model-and-package.md#cscfg) file.</span></span> <span data-ttu-id="dbdce-150">Először meg kell toodownload a .cscfg fájl, módosítsa, majd töltse fel azt.</span><span class="sxs-lookup"><span data-stu-id="dbdce-150">First you need toodownload your .cscfg file, modify it, then upload it.</span></span>

1. <span data-ttu-id="dbdce-151">Kattintson a hello **beállítások** ikon vagy hello **összes beállítás** tooopen mentése hello hivatkozás **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="dbdce-151">Click on hello **Settings** icon or hello **All settings** link tooopen up hello **Settings** blade.</span></span>

    ![Beállítások lap](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. <span data-ttu-id="dbdce-153">Kattintson a hello **konfigurációs** elemet.</span><span class="sxs-lookup"><span data-stu-id="dbdce-153">Click on hello **Configuration** item.</span></span>

    ![Konfigurációs panel](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. <span data-ttu-id="dbdce-155">Kattintson a hello **letöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="dbdce-155">Click on hello **Download** button.</span></span>

    ![Letöltés](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. <span data-ttu-id="dbdce-157">Hello szolgáltatás konfigurációs fájl frissítése után feltöltése és hello konfigurációs frissítések alkalmazásához:</span><span class="sxs-lookup"><span data-stu-id="dbdce-157">After you update hello service configuration file, upload and apply hello configuration updates:</span></span>

    ![Feltöltés](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. <span data-ttu-id="dbdce-159">Válassza ki a hello .cscfg fájlban, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="dbdce-159">Select hello .cscfg file and click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dbdce-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dbdce-160">Next steps</span></span>
* <span data-ttu-id="dbdce-161">Ismerje meg, hogyan túl[felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="dbdce-161">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="dbdce-162">Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="dbdce-162">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="dbdce-163">[A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="dbdce-163">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
* <span data-ttu-id="dbdce-164">Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="dbdce-164">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
