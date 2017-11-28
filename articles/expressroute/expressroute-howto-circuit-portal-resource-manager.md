---
title: "Létrehozásához és módosításához ExpressRoute-kapcsolatcsoportot: Azure-portál |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan létrehozásához, telepítéséhez, győződjön meg arról, frissítése, törlése és kiosztásának megszüntetése ExpressRoute-kapcsolatcsoportot."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68d59d59-ed4d-482f-9cbc-534ebb090613
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/07/2017
ms.author: cherylmc;ganesr
ms.openlocfilehash: e3721cd3c031622788f553e71a6555b844f3f7dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-an-expressroute-circuit"></a><span data-ttu-id="00242-103">Létrehozásához és módosításához ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="00242-103">Create and modify an ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="00242-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="00242-104">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="00242-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="00242-105">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="00242-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="00242-106">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="00242-107">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="00242-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="00242-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="00242-108">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="00242-109">Ez a cikk ismerteti a Azure ExpressRoute-kapcsolatcsoportot létrehozni az Azure portál és az Azure Resource Manager telepítési modell használatával.</span><span class="sxs-lookup"><span data-stu-id="00242-109">This article describes how to create an Azure ExpressRoute circuit by using the Azure portal and the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="00242-110">Az alábbi lépéseket is bemutatják a kapcsolatcsoport állapotának ellenőrzése, a frissítést, vagy törölje és kiosztásának megszüntetése azt.</span><span class="sxs-lookup"><span data-stu-id="00242-110">The following steps also show you how to check the status of the circuit, update it, or delete and deprovision it.</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="00242-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="00242-111">Before you begin</span></span>
* <span data-ttu-id="00242-112">Tekintse át a [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="00242-112">Review the [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="00242-113">Győződjön meg arról, hogy rendelkezik-e a hozzáférést a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="00242-113">Ensure that you have access to the [Azure portal](https://portal.azure.com).</span></span>
* <span data-ttu-id="00242-114">Győződjön meg arról, hogy rendelkezik-e új hálózati erőforrások létrehozásához szükséges engedélyek.</span><span class="sxs-lookup"><span data-stu-id="00242-114">Ensure that you have permissions to create new networking resources.</span></span> <span data-ttu-id="00242-115">Ha nem rendelkezik a megfelelő engedélyekkel, forduljon a fiók rendszergazdájához.</span><span class="sxs-lookup"><span data-stu-id="00242-115">Contact your account administrator if you do not have the right permissions.</span></span>
* <span data-ttu-id="00242-116">Is [videó](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) ahhoz, hogy jobb megértése érdekében a lépések megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="00242-116">You can [view a video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) before beginning in order to better understand the steps.</span></span>

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="00242-117">Hozzon létre, és helyezze üzembe az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="00242-117">Create and provision an ExpressRoute circuit</span></span>
### <a name="1-sign-in-to-the-azure-portal"></a><span data-ttu-id="00242-118">1. Jelentkezzen be az Azure Portalra</span><span class="sxs-lookup"><span data-stu-id="00242-118">1. Sign in to the Azure portal</span></span>
<span data-ttu-id="00242-119">Egy böngészőből lépjen az [Azure Portalra](http://portal.azure.com), majd jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="00242-119">From a browser, navigate to the [Azure portal](http://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="2-create-a-new-expressroute-circuit"></a><span data-ttu-id="00242-120">2. Hozzon létre egy új ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="00242-120">2. Create a new ExpressRoute circuit</span></span>
> [!IMPORTANT]
> <span data-ttu-id="00242-121">Az ExpressRoute-kapcsolatcsoportot abban a pillanatban a szolgáltatási kulcs kiadott lesz terhelve.</span><span class="sxs-lookup"><span data-stu-id="00242-121">Your ExpressRoute circuit will be billed from the moment a service key is issued.</span></span> <span data-ttu-id="00242-122">Győződjön meg arról, hogy a művelet végrehajtása, ha a kapcsolat szolgáltatójánál kiépíteni a kapcsolat készen áll-e.</span><span class="sxs-lookup"><span data-stu-id="00242-122">Ensure that you perform this operation when the connectivity provider is ready to provision the circuit.</span></span>
> 
> 

1. <span data-ttu-id="00242-123">ExpressRoute-kapcsolatcsoportot hozhat létre egy új erőforrás létrehozása beállítás kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="00242-123">You can create an ExpressRoute circuit by selecting the option to create a new resource.</span></span> <span data-ttu-id="00242-124">Kattintson a **új** > **hálózati** > **ExpressRoute**, a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="00242-124">Click **New** > **Networking** > **ExpressRoute**, as shown in the following image:</span></span>
   
    ![ExpressRoute-kapcsolatcsoport létrehozása](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. <span data-ttu-id="00242-126">Miután rákattintott **ExpressRoute**, láthatja a **létrehozása ExpressRoute-kapcsolatcsoportot** panelen.</span><span class="sxs-lookup"><span data-stu-id="00242-126">After you click **ExpressRoute**, you'll see the **Create ExpressRoute circuit** blade.</span></span> <span data-ttu-id="00242-127">Ha kitöltés alatt az értékeket a panel, győződjön meg arról, hogy megadja a helyes SKU réteg és az adatforgalom-mérést.</span><span class="sxs-lookup"><span data-stu-id="00242-127">When you're filling in the values on this blade, make sure that you specify the correct SKU tier and data metering.</span></span>
   
   * <span data-ttu-id="00242-128">**Réteg** meghatározza, hogy egy ExpressRoute standard vagy ExpressRoute prémium bővítmény engedélyezve van-e.</span><span class="sxs-lookup"><span data-stu-id="00242-128">**Tier** determines whether an ExpressRoute standard or an ExpressRoute premium add-on is enabled.</span></span> <span data-ttu-id="00242-129">Megadhat **szabványos** lekérni a standard Termékváltozat vagy **prémium** a prémium szintű bővítménnyel.</span><span class="sxs-lookup"><span data-stu-id="00242-129">You can specify **Standard** to get the standard SKU or **Premium** for the premium add-on.</span></span>
   * <span data-ttu-id="00242-130">**Az adathasználat mérését** határozza meg a számlázási.</span><span class="sxs-lookup"><span data-stu-id="00242-130">**Data metering** determines the billing type.</span></span> <span data-ttu-id="00242-131">Megadhat **Metered** díjköteles adatforgalom tervhez és **korlátlan** egy adatforgalmi a.</span><span class="sxs-lookup"><span data-stu-id="00242-131">You can specify **Metered** for a metered data plan and **Unlimited** for an unlimited data plan.</span></span> <span data-ttu-id="00242-132">Vegye figyelembe, hogy a számlázási típusának módosíthatja **Metered** való **korlátlan**, de nem módosíthatja a típusát a **korlátlan** való **Metered**.</span><span class="sxs-lookup"><span data-stu-id="00242-132">Note that you can change the billing type from **Metered** to **Unlimited**, but you can't change the type from **Unlimited** to **Metered**.</span></span>
     
     ![A Termékváltozat réteg és a mérési adatok konfigurálása](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit2.png)

> [!IMPORTANT]
> <span data-ttu-id="00242-134">Felhívjuk a figyelmét arra, hogy a társviszony-létesítési helye jelzi a [helynév](expressroute-locations.md) hol vannak a Microsoft társviszony.</span><span class="sxs-lookup"><span data-stu-id="00242-134">Please be aware that the Peering Location indicates the [physical location](expressroute-locations.md) where you are peering with Microsoft.</span></span> <span data-ttu-id="00242-135">Ez az **nem** "Hely" tulajdonság, amely hivatkozik a földrajzi hely, ahol az Azure hálózati erőforrás-szolgáltató csatolva.</span><span class="sxs-lookup"><span data-stu-id="00242-135">This is **not** linked to "Location" property, which refers to the geography where the Azure Network Resource Provider is located.</span></span> <span data-ttu-id="00242-136">Nincsenek kapcsolatban, de napjainkban célszerű válassza ki a hálózati erőforrás-szolgáltató földrajzilag megközelíti a kör társviszony-létesítési helye.</span><span class="sxs-lookup"><span data-stu-id="00242-136">While they are not related, it is a good practice to choose a Network Resource Provider geographically close to the Peering Location of the circuit.</span></span> 
> 
> 

### <a name="3-view-the-circuits-and-properties"></a><span data-ttu-id="00242-137">3. A Kapcsolatcsoportok és tulajdonságok megtekintése</span><span class="sxs-lookup"><span data-stu-id="00242-137">3. View the circuits and properties</span></span>
<span data-ttu-id="00242-138">**A kapcsolatok megtekintése**</span><span class="sxs-lookup"><span data-stu-id="00242-138">**View all the circuits**</span></span>

<span data-ttu-id="00242-139">Megtekintheti a áramköröket kiválasztásával létrehozott **összes erőforrás** a bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="00242-139">You can view all the circuits that you created by selecting **All resources** on the left-side menu.</span></span>

![Kapcsolatok megtekintése](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

<span data-ttu-id="00242-141">**A Tulajdonságok megtekintése**</span><span class="sxs-lookup"><span data-stu-id="00242-141">**View the properties**</span></span>

    You can view the properties of the circuit by selecting it. On this blade, note the service key for the circuit. You must copy the circuit key for your circuit and pass it down to the service provider to complete the provisioning process. The circuit key is specific to your circuit.

![Tulajdonságok megtekintése](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

### <a name="4-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a><span data-ttu-id="00242-143">4. A szolgáltatás kulcs küldése a kapcsolat szolgáltatójánál történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="00242-143">4. Send the service key to your connectivity provider for provisioning</span></span>
<span data-ttu-id="00242-144">A panel **szolgáltató állapota** információt nyújt a jelenlegi állapotában a szolgáltatói oldalon kiépítés.</span><span class="sxs-lookup"><span data-stu-id="00242-144">On this blade, **Provider status** provides information on the current state of provisioning on the service-provider side.</span></span> <span data-ttu-id="00242-145">**Állapot áramkör** állapotát biztosít a Microsoft oldalon.</span><span class="sxs-lookup"><span data-stu-id="00242-145">**Circuit status** provides the state on the Microsoft side.</span></span> <span data-ttu-id="00242-146">Kiépítés állapotok áramkör kapcsolatos további információkért tekintse meg a [munkafolyamatok](expressroute-workflows.md#expressroute-circuit-provisioning-states) cikk.</span><span class="sxs-lookup"><span data-stu-id="00242-146">For more information about circuit provisioning states, see the [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="00242-147">Amikor létrehoz egy új ExpressRoute-kapcsolatcsoportot, a kapcsolatcsoport lesz a következő állapotot okozta:</span><span class="sxs-lookup"><span data-stu-id="00242-147">When you create a new ExpressRoute circuit, the circuit will be in the following state:</span></span>

<span data-ttu-id="00242-148">Szolgáltató állapota: nincs telepítve</span><span class="sxs-lookup"><span data-stu-id="00242-148">Provider status: Not provisioned</span></span><BR>
<span data-ttu-id="00242-149">Áramkör állapot: engedélyezve</span><span class="sxs-lookup"><span data-stu-id="00242-149">Circuit status: Enabled</span></span>

![Üzembe helyezési folyamat elindítása](./media/expressroute-howto-circuit-portal-resource-manager/viewstatus.png)

<span data-ttu-id="00242-151">A kapcsolatcsoport fogja módosítani a következő állapotot folyamatban van, lehetővé téve, a kapcsolat hitelesítésszolgáltató esetén:</span><span class="sxs-lookup"><span data-stu-id="00242-151">The circuit will change to the following state when the connectivity provider is in the process of enabling it for you:</span></span>

<span data-ttu-id="00242-152">Szolgáltató állapota: kiépítés</span><span class="sxs-lookup"><span data-stu-id="00242-152">Provider status: Provisioning</span></span><BR>
<span data-ttu-id="00242-153">Áramkör állapot: engedélyezve</span><span class="sxs-lookup"><span data-stu-id="00242-153">Circuit status: Enabled</span></span>

<span data-ttu-id="00242-154">Meg kell tudni használni az ExpressRoute-kapcsolatcsoportot a következő állapotban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="00242-154">For you to be able to use an ExpressRoute circuit, it must be in the following state:</span></span>

<span data-ttu-id="00242-155">Szolgáltató állapota: kiépítése</span><span class="sxs-lookup"><span data-stu-id="00242-155">Provider status: Provisioned</span></span><BR>
<span data-ttu-id="00242-156">Áramkör állapot: engedélyezve</span><span class="sxs-lookup"><span data-stu-id="00242-156">Circuit status: Enabled</span></span>

### <a name="5-periodically-check-the-status-and-the-state-of-the-circuit-key"></a><span data-ttu-id="00242-157">5. Ellenőrizze rendszeresen a kapcsolatcsoport kulcs állapotát és az állapot</span><span class="sxs-lookup"><span data-stu-id="00242-157">5. Periodically check the status and the state of the circuit key</span></span>
<span data-ttu-id="00242-158">A kapcsolatcsoport, hogy az lehetőséget most érdekelt tulajdonságainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="00242-158">You can view the properties of the circuit that you're interested in by selecting it.</span></span> <span data-ttu-id="00242-159">Ellenőrizze a **szolgáltató állapota** , és győződjön meg arról, hogy átkerült az **kiépítve** a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="00242-159">Check the **Provider status** and ensure that it has moved to **Provisioned** before you continue.</span></span>

![Kör és szolgáltató állapota](./media/expressroute-howto-circuit-portal-resource-manager/viewstatusprovisioned.png)

### <a name="6-create-your-routing-configuration"></a><span data-ttu-id="00242-161">6. Az útválasztó-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="00242-161">6. Create your routing configuration</span></span>
<span data-ttu-id="00242-162">Lépésenkénti útmutatásért tekintse meg a [ExpressRoute-áramkör útválasztási konfigurációja](expressroute-howto-routing-portal-resource-manager.md) cikk létrehozásához és módosításához a kapcsolatcsoport esetében.</span><span class="sxs-lookup"><span data-stu-id="00242-162">For step-by-step instructions, refer to the [ExpressRoute circuit routing configuration](expressroute-howto-routing-portal-resource-manager.md) article to create and modify circuit peerings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="00242-163">Ezek az utasítások csak a szolgáltatók által biztosított réteg 2 internetkapcsolati szolgáltatás használatával létrehozott kapcsolatok vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="00242-163">These instructions only apply to circuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="00242-164">A szolgáltató által kezelt használata réteg (általában az IP VPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.</span><span class="sxs-lookup"><span data-stu-id="00242-164">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="7-link-a-virtual-network-to-an-expressroute-circuit"></a><span data-ttu-id="00242-165">7. Virtuális hálózat összekapcsolása egy ExpressRoute-kapcsolatcsoporttal</span><span class="sxs-lookup"><span data-stu-id="00242-165">7. Link a virtual network to an ExpressRoute circuit</span></span>
<span data-ttu-id="00242-166">A következő csatolja az ExpressRoute-kapcsolatcsoportot egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="00242-166">Next, link a virtual network to your ExpressRoute circuit.</span></span> <span data-ttu-id="00242-167">Használja a [virtuális hálózatok összekapcsolása ExpressRoute-Kapcsolatcsoportok](expressroute-howto-linkvnet-arm.md) a Resource Manager üzembe helyezési modellel végzett munka során a következő cikket.</span><span class="sxs-lookup"><span data-stu-id="00242-167">Use the [Linking virtual networks to ExpressRoute circuits](expressroute-howto-linkvnet-arm.md) article when you work with the Resource Manager deployment model.</span></span>

## <a name="getting-the-status-of-an-expressroute-circuit"></a><span data-ttu-id="00242-168">Egy ExpressRoute-kapcsolatcsoport állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="00242-168">Getting the status of an ExpressRoute circuit</span></span>
<span data-ttu-id="00242-169">A kapcsolatcsoport állapotának megtekintéséhez jelölje ki.</span><span class="sxs-lookup"><span data-stu-id="00242-169">You can view the status of a circuit by selecting it.</span></span> 

![Egy ExpressRoute-kapcsolatcsoport állapotát](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

## <a name="modifying-an-expressroute-circuit"></a><span data-ttu-id="00242-171">Egy ExpressRoute-kapcsolatcsoportot módosítása</span><span class="sxs-lookup"><span data-stu-id="00242-171">Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="00242-172">ExpressRoute-kapcsolatcsoportot egyes tulajdonságait módosíthatja kapcsolat befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="00242-172">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="00242-173">Állásidő nélkül a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="00242-173">You can do the following with no downtime:</span></span>

* <span data-ttu-id="00242-174">Engedélyezi vagy letiltja az ExpressRoute-kapcsolatcsoportot prémium ExpressRoute bővítményt.</span><span class="sxs-lookup"><span data-stu-id="00242-174">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="00242-175">Növelje a ExpressRoute-kapcsolatcsoportot sávszélességét, feltéve, hogy kapacitás érhető el a port.</span><span class="sxs-lookup"><span data-stu-id="00242-175">Increase the bandwidth of your ExpressRoute circuit provided there is capacity available on the port.</span></span> <span data-ttu-id="00242-176">Vegye figyelembe, hogy a alacsonyabb verziójúra változtatása a sávszélességet a kapcsolat nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="00242-176">Note that downgrading the bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="00242-177">Díjköteles adatforgalom korlátlan adatokhoz a mérési terv módosítása</span><span class="sxs-lookup"><span data-stu-id="00242-177">Change the metering plan from Metered Data to Unlimited Data.</span></span> <span data-ttu-id="00242-178">Vegye figyelembe, hogy a mérési terv módosítása az korlátlan adatforgalom díjköteles adatok nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="00242-178">Note that changing the metering plan from Unlimited Data to Metered Data is not supported.</span></span>
* <span data-ttu-id="00242-179">Engedélyezheti és letilthatja *klasszikus műveletek engedélyezése*.</span><span class="sxs-lookup"><span data-stu-id="00242-179">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="00242-180">Korlátai és korlátozásai további információkért tekintse meg a [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="00242-180">For more information on limits and limitations, refer to the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

<span data-ttu-id="00242-181">Egy ExpressRoute-kapcsolatcsoportot módosításához kattintson a a **konfigurációs** az alábbi ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="00242-181">To modify an ExpressRoute circuit, click on the **Configuration** as shown in the figure below.</span></span>

![Kör módosítása](./media/expressroute-howto-circuit-portal-resource-manager/modifycircuit.png)

<span data-ttu-id="00242-183">A sávszélesség, SKU, számlázási modellt módosíthatja, és engedélyezi a klasszikus műveletek belül a konfigurációs panel.</span><span class="sxs-lookup"><span data-stu-id="00242-183">You can modify the bandwidth, SKU, billing model and allow classic operations within the configuration blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="00242-184">Előfordulhat, hogy újra létrehozni az ExpressRoute-kapcsolatcsoport, ha nincs elég kapacitás a meglévő porton.</span><span class="sxs-lookup"><span data-stu-id="00242-184">You may have to recreate the ExpressRoute circuit if there is inadequate capacity on the existing port.</span></span> <span data-ttu-id="00242-185">A kapcsolat nem frissíthető, ha nincsenek további kapacitást érhető el az adott helyhez.</span><span class="sxs-lookup"><span data-stu-id="00242-185">You cannot upgrade the circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="00242-186">Nem csökkenti a sávszélesség az ExpressRoute-kapcsolatcsoportot megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="00242-186">You cannot reduce the bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="00242-187">Alacsonyabb verziójúra változtatása a sávszélesség szükséges, hogy az ExpressRoute-kapcsolatcsoport kiosztásának megszüntetése és majd építenie az új ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="00242-187">Downgrading bandwidth requires you to deprovision the ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 
> <span data-ttu-id="00242-188">Prémium szintű bővítmény letiltása művelet sikertelen lesz, amely nagyobb, mint mi a szabványos kör megengedett erőforrások használata.</span><span class="sxs-lookup"><span data-stu-id="00242-188">Disable premium add-on operation can fail if you're using resources that are greater than what is permitted for the standard circuit.</span></span>
> 
> 

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="00242-189">Megszüntetés és az ExpressRoute-kapcsolatcsoport törlése</span><span class="sxs-lookup"><span data-stu-id="00242-189">Deprovisioning and deleting an ExpressRoute circuit</span></span>
<span data-ttu-id="00242-190">Az ExpressRoute-kapcsolatcsoportot kiválasztásával törölheti a **törlése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="00242-190">You can delete your ExpressRoute circuit by selecting the **delete** icon.</span></span> <span data-ttu-id="00242-191">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="00242-191">Note the following:</span></span>

* <span data-ttu-id="00242-192">Az ExpressRoute-kapcsolatcsoport az összes virtuális hálózatot kell választható.</span><span class="sxs-lookup"><span data-stu-id="00242-192">You must unlink all virtual networks from the ExpressRoute circuit.</span></span> <span data-ttu-id="00242-193">Ez a művelet sikertelen lesz, ha ellenőrizze, hogy virtuális hálózatok a kapcsolatcsoport van csatolva.</span><span class="sxs-lookup"><span data-stu-id="00242-193">If this operation fails, check whether any virtual networks are linked to the circuit.</span></span>
* <span data-ttu-id="00242-194">Ha az ExpressRoute-kapcsolatcsoport szolgáltatás szolgáltató üzembe helyezési állapota **kiépítési** vagy **kiépítve** kiosztásának megszüntetése a kapcsolatcsoport az oldalon, hogy a szolgáltató együttműködve.</span><span class="sxs-lookup"><span data-stu-id="00242-194">If the ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider to deprovision the circuit on their side.</span></span> <span data-ttu-id="00242-195">Folytatjuk erőforrásokat és kiszámlázni Önnek, amíg a szolgáltató befejeződött, a kapcsolat megszüntetés, és értesítést küld nekünk.</span><span class="sxs-lookup"><span data-stu-id="00242-195">We will continue to reserve resources and bill you until the service provider completes deprovisioning the circuit and notifies us.</span></span>
* <span data-ttu-id="00242-196">Ha a szolgáltató rendelkezik platformelőfizetés a kapcsolatcsoport (üzembe helyezési állapota szolgáltató értéke **nincs kiépítve**) a kapcsolatcsoport törölhet.</span><span class="sxs-lookup"><span data-stu-id="00242-196">If the service provider has deprovisioned the circuit (the service provider provisioning state is set to **Not provisioned**) you can then delete the circuit.</span></span> <span data-ttu-id="00242-197">A kör számlázási leállítása</span><span class="sxs-lookup"><span data-stu-id="00242-197">This will stop billing for the circuit</span></span>

## <a name="next-steps"></a><span data-ttu-id="00242-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="00242-198">Next steps</span></span>
<span data-ttu-id="00242-199">Miután létrehozta a kapcsolatcsoport, győződjön meg arról, hogy akkor tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="00242-199">After you create your circuit, make sure that you do the following:</span></span>

* [<span data-ttu-id="00242-200">Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoport esetében routing</span><span class="sxs-lookup"><span data-stu-id="00242-200">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-portal-resource-manager.md)
* [<span data-ttu-id="00242-201">A virtuális hálózat csatolása az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="00242-201">Link your virtual network to your ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)

