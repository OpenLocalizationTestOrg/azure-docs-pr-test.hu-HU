---
title: "A felhőszolgáltatás (portál) konfigurálása |} Microsoft Docs"
description: "Útmutató az Azure felhőszolgáltatások konfigurálása. Ismerje meg, a felhőalapú szolgáltatás konfigurációja frissítése, és konfigurálja a távelérést a szerepkörpéldányok. Ezekben a példákban az Azure-portálon."
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
ms.openlocfilehash: a7e891d05ffe4cc2b4f68dce072a81499cc6de80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-configure-cloud-services"></a><span data-ttu-id="5d858-105">Felhőszolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5d858-105">How to Configure Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5d858-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5d858-106">Azure portal</span></span>](cloud-services-how-to-configure-portal.md)
> * [<span data-ttu-id="5d858-107">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="5d858-107">Azure classic portal</span></span>](cloud-services-how-to-configure.md)
>
>

<span data-ttu-id="5d858-108">Az Azure portálon konfigurálhatja a leggyakrabban használt beállításait egy felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5d858-108">You can configure the most commonly used settings for a cloud service in the Azure portal.</span></span> <span data-ttu-id="5d858-109">Azok az ügyfeleink, akik közvetlenül szeretnék frissíteni a konfigurációs fájlokat, letölthetik a frissítendő szolgáltatáskonfigurációs fájlt, amelyet módosítás után feltölthetnek, így frissül a felhőszolgáltatás konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="5d858-109">Or, if you like to update your configuration files directly, download a service configuration file to update, and then upload the updated file and update the cloud service with the configuration changes.</span></span> <span data-ttu-id="5d858-110">A rendszer mindkét esetben az összes szerepkörpéldányon elvégzi a konfiguráció módosítását.</span><span class="sxs-lookup"><span data-stu-id="5d858-110">Either way, the configuration updates are pushed out to all role instances.</span></span>

<span data-ttu-id="5d858-111">Emellett kezelheti a példányait a felhőszolgáltatás szerepköreit, vagy a távoli asztal be őket.</span><span class="sxs-lookup"><span data-stu-id="5d858-111">You can also manage the instances of your cloud service roles, or remote desktop into them.</span></span>

<span data-ttu-id="5d858-112">Azure is csak 99,95 % szolgáltatás rendelkezésre állásának biztosításához a konfigurációfrissítések során ha legalább két szerepkör minden egyes szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="5d858-112">Azure can only ensure 99.95 percent service availability during the configuration updates if you have at least two role instances for every role.</span></span> <span data-ttu-id="5d858-113">Amely lehetővé teszi, hogy egy virtuális gép ügyfél kérelmek feldolgozásához, a másik frissítése közben.</span><span class="sxs-lookup"><span data-stu-id="5d858-113">That enables one virtual machine to process client requests while the other is being updated.</span></span> <span data-ttu-id="5d858-114">További információkért lásd: [szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="5d858-114">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="change-a-cloud-service"></a><span data-ttu-id="5d858-115">Egy felhőalapú szolgáltatás módosítása</span><span class="sxs-lookup"><span data-stu-id="5d858-115">Change a cloud service</span></span>
<span data-ttu-id="5d858-116">Megnyitás után a [Azure-portálon](https://portal.azure.com/), keresse meg a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5d858-116">After opening the [Azure portal](https://portal.azure.com/), navigate to your cloud service.</span></span> <span data-ttu-id="5d858-117">Itt azt sok szempontból kezelhető.</span><span class="sxs-lookup"><span data-stu-id="5d858-117">From here, you manage many aspects of it.</span></span>

![Beállítások lap](./media/cloud-services-how-to-configure-portal/cloud-service.png)

<span data-ttu-id="5d858-119">A **beállítások** vagy **összes beállítás** hivatkozásokra kattintva megnyílik a **beállítások** panel, amelyen módosíthatja a **tulajdonságok**, módosítsa a **konfigurációs**, kezelése a **tanúsítványok**, telepítő **riasztási szabályok**, és kezelése a **felhasználók** ki van-e a felhőalapú szolgáltatás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="5d858-119">The **Settings** or **All settings** links will open up the **Settings** blade where you can change the **Properties**, change the **Configuration**, manage the **Certificates**, setup **Alert rules**, and manage the **Users** who have access to this cloud service.</span></span>

![Azure cloud service beállítások panel](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a><span data-ttu-id="5d858-121">Kezelheti a vendég operációs rendszer verziója</span><span class="sxs-lookup"><span data-stu-id="5d858-121">Manage Guest OS version</span></span>

<span data-ttu-id="5d858-122">Alapértelmezés szerint Azure rendszeres időközönként frissíti a vendég operációs rendszer a legújabb támogatott lemezképhez belül a szolgáltatás konfigurációs (.cscfg), a megadott operációsrendszer-termékcsalád például a Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="5d858-122">By default, Azure periodically updates your guest OS to the latest supported image within the OS family that you've specified in your service configuration (.cscfg), such as Windows Server 2016.</span></span>

<span data-ttu-id="5d858-123">Ha egy operációs rendszer adott verziójához van szüksége, állíthatja be, a **konfigurációs** panelen.</span><span class="sxs-lookup"><span data-stu-id="5d858-123">If you need to target a specific OS version, you can set it in the **Configuration** blade.</span></span>

![Az operációs rendszer verzió beállítása](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)


>[!IMPORTANT]
> <span data-ttu-id="5d858-125">Egy adott operációs rendszer verziója letiltja az operációs rendszer automatikus kiválasztása frissíti, és lehetővé teszi a felhasználó felelőssége javítását.</span><span class="sxs-lookup"><span data-stu-id="5d858-125">Choosing a specific OS version disables automatic OS updates and makes patching your responsibility.</span></span> <span data-ttu-id="5d858-126">Gondoskodnia kell arról, hogy a szerepkörpéldányok frissítések kap, vagy az alkalmazás biztonsági rések tehetik közzé.</span><span class="sxs-lookup"><span data-stu-id="5d858-126">You must ensure that your role instances are receiving updates or you may expose your application to security vulnerabilities.</span></span>

## <a name="monitoring"></a><span data-ttu-id="5d858-127">Figyelés</span><span class="sxs-lookup"><span data-stu-id="5d858-127">Monitoring</span></span>
<span data-ttu-id="5d858-128">A felhőalapú szolgáltatás riasztások adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="5d858-128">You can add alerts to your cloud service.</span></span> <span data-ttu-id="5d858-129">Kattintson a **beállítások** > **riasztás szabályok** > **riasztás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5d858-129">Click **Settings** > **Alert Rules** > **Add alert**.</span></span>

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

<span data-ttu-id="5d858-130">Itt beállíthatja egy riasztást.</span><span class="sxs-lookup"><span data-stu-id="5d858-130">From here, you can setup an alert.</span></span> <span data-ttu-id="5d858-131">Az a **metrika** legördülő listája, beállíthatja a következő típusú adatok egy riasztást.</span><span class="sxs-lookup"><span data-stu-id="5d858-131">With the **Metric** drop down box, you can setup an alert for the following types of data.</span></span>

* <span data-ttu-id="5d858-132">Lemez sebessége olvasott</span><span class="sxs-lookup"><span data-stu-id="5d858-132">Disk read</span></span>
* <span data-ttu-id="5d858-133">Lemez írási</span><span class="sxs-lookup"><span data-stu-id="5d858-133">Disk write</span></span>
* <span data-ttu-id="5d858-134">A hálózati</span><span class="sxs-lookup"><span data-stu-id="5d858-134">Network in</span></span>
* <span data-ttu-id="5d858-135">Kimenő hálózati</span><span class="sxs-lookup"><span data-stu-id="5d858-135">Network out</span></span>
* <span data-ttu-id="5d858-136">Processzorhasználat (%)</span><span class="sxs-lookup"><span data-stu-id="5d858-136">CPU percentage</span></span>

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a><span data-ttu-id="5d858-137">Egy mérték csempe a figyelés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5d858-137">Configure monitoring from a metric tile</span></span>
<span data-ttu-id="5d858-138">Helyett **beállítások** > **riasztási szabályok**, a metrika csempék egyik kattinthat a **figyelés** szakasza a **felhőalapú szolgáltatás** panelen.</span><span class="sxs-lookup"><span data-stu-id="5d858-138">Instead of using **Settings** > **Alert Rules**, you can click on one of the metric tiles in the **Monitoring** section of the **Cloud service** blade.</span></span>

![A felhőalapú szolgáltatás figyelése](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

<span data-ttu-id="5d858-140">Innen testre a diagram a csempe használt, vagy vegye fel a riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="5d858-140">From here you can customize the chart used with the tile, or add an alert rule.</span></span>

## <a name="reboot-reimage-or-remote-desktop"></a><span data-ttu-id="5d858-141">Rendszer újraindítása, a lemezkép-visszaállítási vagy a távoli asztal</span><span class="sxs-lookup"><span data-stu-id="5d858-141">Reboot, reimage, or remote desktop</span></span>
<span data-ttu-id="5d858-142">Jelenleg nem lehet konfigurálni a távoli asztal használatával a **Azure-portálon**.</span><span class="sxs-lookup"><span data-stu-id="5d858-142">At this time you cannot configure remote desktop using the **Azure portal**.</span></span> <span data-ttu-id="5d858-143">Azonban beállíthatja azt keresztül a [a klasszikus Azure portálon](cloud-services-role-enable-remote-desktop.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), illetve [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="5d858-143">However, you can set it up through the [Azure classic portal](cloud-services-role-enable-remote-desktop.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), or through [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).</span></span>

<span data-ttu-id="5d858-144">Első lépésként kattintson az a cloud service-példány.</span><span class="sxs-lookup"><span data-stu-id="5d858-144">First, click on the cloud service instance.</span></span>

![Cloud Service-példány](./media/cloud-services-how-to-configure-portal/cs-instance.png)

<span data-ttu-id="5d858-146">A panelen, megnyílik a távoli asztali kapcsolat létrehozására, távolról indítsa újra a példányt, vagy távolról újból lemezképet létrehozni (Indítás friss képének) a példányt.</span><span class="sxs-lookup"><span data-stu-id="5d858-146">From the blade that opens you can initiate a remote desktop connection, remotely reboot the instance, or remotely reimage (start with a fresh image) the instance.</span></span>

![Cloud Service példány gombok](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a><span data-ttu-id="5d858-148">Konfigurálja újra a .cscfg</span><span class="sxs-lookup"><span data-stu-id="5d858-148">Reconfigure your .cscfg</span></span>
<span data-ttu-id="5d858-149">Szükség lehet, hogy engedélyezzék a felhőalapú szolgáltatás keresztül a [szolgáltatás konfigurációs (szolgáltatáskonfigurációs séma)](cloud-services-model-and-package.md#cscfg) fájlt.</span><span class="sxs-lookup"><span data-stu-id="5d858-149">You may need to reconfigure your cloud service through the [service config (cscfg)](cloud-services-model-and-package.md#cscfg) file.</span></span> <span data-ttu-id="5d858-150">Először meg kell töltse le a .cscfg fájlban, módosítsa, majd töltse fel azt.</span><span class="sxs-lookup"><span data-stu-id="5d858-150">First you need to download your .cscfg file, modify it, then upload it.</span></span>

1. <span data-ttu-id="5d858-151">Kattintson a a **beállítások** ikon vagy a **összes beállítás** hivatkozásra kattintva nyissa meg a **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="5d858-151">Click on the **Settings** icon or the **All settings** link to open up the **Settings** blade.</span></span>

    ![Beállítások lap](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. <span data-ttu-id="5d858-153">Kattintson a **konfigurációs** elemet.</span><span class="sxs-lookup"><span data-stu-id="5d858-153">Click on the **Configuration** item.</span></span>

    ![Konfigurációs panel](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. <span data-ttu-id="5d858-155">Kattintson a **letöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5d858-155">Click on the **Download** button.</span></span>

    ![Letöltés](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. <span data-ttu-id="5d858-157">Miután frissítette a szolgáltatás konfigurációs fájlja, töltse fel, és a konfigurációs frissítések alkalmazásához:</span><span class="sxs-lookup"><span data-stu-id="5d858-157">After you update the service configuration file, upload and apply the configuration updates:</span></span>

    ![Feltöltés](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. <span data-ttu-id="5d858-159">Válassza ki a .cscfg fájlban, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d858-159">Select the .cscfg file and click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d858-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5d858-160">Next steps</span></span>
* <span data-ttu-id="5d858-161">Megtudhatja, hogyan [felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5d858-161">Learn how to [deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="5d858-162">Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5d858-162">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="5d858-163">[A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5d858-163">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
* <span data-ttu-id="5d858-164">Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5d858-164">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
