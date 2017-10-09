---
title: "Létrehozásához és módosításához ExpressRoute-kapcsolatcsoportot: Azure-portál |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate, kiépítéséhez, győződjön meg arról, frissítése, törlése és kiosztásának megszüntetése ExpressRoute-kapcsolatcsoportot."
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
ms.openlocfilehash: 200418aed6bdebace43a2f57cf532d1c8d13cb83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit"></a><span data-ttu-id="ad74d-103">Létrehozásához és módosításához ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="ad74d-103">Create and modify an ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ad74d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ad74d-104">Azure portal</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
> * [<span data-ttu-id="ad74d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ad74d-105">PowerShell</span></span>](expressroute-howto-circuit-arm.md)
> * [<span data-ttu-id="ad74d-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ad74d-106">Azure CLI</span></span>](howto-circuit-cli.md)
> * [<span data-ttu-id="ad74d-107">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ad74d-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [<span data-ttu-id="ad74d-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="ad74d-108">PowerShell (classic)</span></span>](expressroute-howto-circuit-classic.md)
>

<span data-ttu-id="ad74d-109">Ez a cikk ismerteti, hogyan toocreate használatával Azure ExpressRoute-kapcsolatcsoportot hello Azure portál és hello Azure Resource Manager telepítési modell.</span><span class="sxs-lookup"><span data-stu-id="ad74d-109">This article describes how toocreate an Azure ExpressRoute circuit by using hello Azure portal and hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="ad74d-110">az alábbi lépések is hello megjelenítése hogyan hello kapcsolatcsoport állapotának toocheck hello a frissítést, vagy törölje és kiosztásának megszüntetése azt.</span><span class="sxs-lookup"><span data-stu-id="ad74d-110">hello following steps also show you how toocheck hello status of hello circuit, update it, or delete and deprovision it.</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="ad74d-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="ad74d-111">Before you begin</span></span>
* <span data-ttu-id="ad74d-112">Felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md) és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="ad74d-112">Review hello [prerequisites](expressroute-prerequisites.md) and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="ad74d-113">Győződjön meg arról, hogy rendelkezik-e hozzáférési toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ad74d-113">Ensure that you have access toohello [Azure portal](https://portal.azure.com).</span></span>
* <span data-ttu-id="ad74d-114">Győződjön meg arról, hogy rendelkezik-e engedélyekkel toocreate új hálózati erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="ad74d-114">Ensure that you have permissions toocreate new networking resources.</span></span> <span data-ttu-id="ad74d-115">Ha nem rendelkezik hello megfelelő engedélyekkel, forduljon a fiók rendszergazdájához.</span><span class="sxs-lookup"><span data-stu-id="ad74d-115">Contact your account administrator if you do not have hello right permissions.</span></span>
* <span data-ttu-id="ad74d-116">Is [videó](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) sorrendben kezdete elé toobetter megismerheti hello lépéseit.</span><span class="sxs-lookup"><span data-stu-id="ad74d-116">You can [view a video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) before beginning in order toobetter understand hello steps.</span></span>

## <a name="create-and-provision-an-expressroute-circuit"></a><span data-ttu-id="ad74d-117">Hozzon létre, és helyezze üzembe az ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="ad74d-117">Create and provision an ExpressRoute circuit</span></span>
### <a name="1-sign-in-toohello-azure-portal"></a><span data-ttu-id="ad74d-118">1. Jelentkezzen be toohello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ad74d-118">1. Sign in toohello Azure portal</span></span>
<span data-ttu-id="ad74d-119">Egy böngészőből keresse meg a toohello [Azure-portálon](http://portal.azure.com) és jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="ad74d-119">From a browser, navigate toohello [Azure portal](http://portal.azure.com) and sign in with your Azure account.</span></span>

### <a name="2-create-a-new-expressroute-circuit"></a><span data-ttu-id="ad74d-120">2. Hozzon létre egy új ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="ad74d-120">2. Create a new ExpressRoute circuit</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ad74d-121">Az ExpressRoute-kapcsolatcsoportot jelenik meg a szolgáltatási kulcs hello pillanattól lesz terhelve.</span><span class="sxs-lookup"><span data-stu-id="ad74d-121">Your ExpressRoute circuit will be billed from hello moment a service key is issued.</span></span> <span data-ttu-id="ad74d-122">Győződjön meg arról, hogy készen áll a tooprovision hello áramkör hello kapcsolat szolgáltatójánál esetén elvégzi ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="ad74d-122">Ensure that you perform this operation when hello connectivity provider is ready tooprovision hello circuit.</span></span>
> 
> 

1. <span data-ttu-id="ad74d-123">Létrehozhat egy ExpressRoute-kapcsolatcsoportot hello beállítás toocreate kiválasztásával egy új erőforrást.</span><span class="sxs-lookup"><span data-stu-id="ad74d-123">You can create an ExpressRoute circuit by selecting hello option toocreate a new resource.</span></span> <span data-ttu-id="ad74d-124">Kattintson a **új** > **hálózati** > **ExpressRoute**, ahogy az a következő kép hello:</span><span class="sxs-lookup"><span data-stu-id="ad74d-124">Click **New** > **Networking** > **ExpressRoute**, as shown in hello following image:</span></span>
   
    ![ExpressRoute-kapcsolatcsoport létrehozása](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. <span data-ttu-id="ad74d-126">Miután rákattintott **ExpressRoute**, látni fogja, hello **létrehozása ExpressRoute-kapcsolatcsoportot** panelen.</span><span class="sxs-lookup"><span data-stu-id="ad74d-126">After you click **ExpressRoute**, you'll see hello **Create ExpressRoute circuit** blade.</span></span> <span data-ttu-id="ad74d-127">A panel hello értékek kitöltés alatt ellenőrizze, meg kell adnia hello megfelelő SKU réteg és az adatforgalom-mérést.</span><span class="sxs-lookup"><span data-stu-id="ad74d-127">When you're filling in hello values on this blade, make sure that you specify hello correct SKU tier and data metering.</span></span>
   
   * <span data-ttu-id="ad74d-128">**Réteg** meghatározza, hogy egy ExpressRoute standard vagy ExpressRoute prémium bővítmény engedélyezve van-e.</span><span class="sxs-lookup"><span data-stu-id="ad74d-128">**Tier** determines whether an ExpressRoute standard or an ExpressRoute premium add-on is enabled.</span></span> <span data-ttu-id="ad74d-129">Megadhat **szabványos** tooget hello standard Termékváltozat vagy **prémium** hello prémium bővítménnyel.</span><span class="sxs-lookup"><span data-stu-id="ad74d-129">You can specify **Standard** tooget hello standard SKU or **Premium** for hello premium add-on.</span></span>
   * <span data-ttu-id="ad74d-130">**Az adathasználat mérését** hello számlázási típusa határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ad74d-130">**Data metering** determines hello billing type.</span></span> <span data-ttu-id="ad74d-131">Megadhat **Metered** díjköteles adatforgalom tervhez és **korlátlan** egy adatforgalmi a.</span><span class="sxs-lookup"><span data-stu-id="ad74d-131">You can specify **Metered** for a metered data plan and **Unlimited** for an unlimited data plan.</span></span> <span data-ttu-id="ad74d-132">Vegye figyelembe, hogy számlázási típusának hello módosíthatja **Metered** túl**korlátlan**, de nem módosíthatja a hello típusát **korlátlan** túl**Metered**.</span><span class="sxs-lookup"><span data-stu-id="ad74d-132">Note that you can change hello billing type from **Metered** too**Unlimited**, but you can't change hello type from **Unlimited** too**Metered**.</span></span>
     
     ![Hello SKU réteg és a mérési adatok konfigurálása](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit2.png)

> [!IMPORTANT]
> <span data-ttu-id="ad74d-134">Felhívjuk a figyelmét arra, hogy hello társviszony-létesítés hely jelzi hello [helynév](expressroute-locations.md) hol vannak a Microsoft társviszony.</span><span class="sxs-lookup"><span data-stu-id="ad74d-134">Please be aware that hello Peering Location indicates hello [physical location](expressroute-locations.md) where you are peering with Microsoft.</span></span> <span data-ttu-id="ad74d-135">Ez az **nem** kapcsolódó túl "Hely" tulajdonság, amely a toohello földrajzi hely, ahol hello Azure hálózati erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="ad74d-135">This is **not** linked too"Location" property, which refers toohello geography where hello Azure Network Resource Provider is located.</span></span> <span data-ttu-id="ad74d-136">Bár nem áll(nak) kapcsolatban, ez a beállítás egy célszerű toochoose a hálózati erőforrás-szolgáltató földrajzilag Bezárás toohello hello kör társviszony-létesítés helyét.</span><span class="sxs-lookup"><span data-stu-id="ad74d-136">While they are not related, it is a good practice toochoose a Network Resource Provider geographically close toohello Peering Location of hello circuit.</span></span> 
> 
> 

### <a name="3-view-hello-circuits-and-properties"></a><span data-ttu-id="ad74d-137">3. Nézet hello Kapcsolatcsoportok és tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ad74d-137">3. View hello circuits and properties</span></span>
<span data-ttu-id="ad74d-138">**Minden hello kapcsolatok megtekintése**</span><span class="sxs-lookup"><span data-stu-id="ad74d-138">**View all hello circuits**</span></span>

<span data-ttu-id="ad74d-139">Megtekintheti az összes hello áramkör kiválasztásával létrehozott **összes erőforrás** hello bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="ad74d-139">You can view all hello circuits that you created by selecting **All resources** on hello left-side menu.</span></span>

![Kapcsolatok megtekintése](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

<span data-ttu-id="ad74d-141">**Hello tulajdonságainak megtekintése**</span><span class="sxs-lookup"><span data-stu-id="ad74d-141">**View hello properties**</span></span>

    You can view hello properties of hello circuit by selecting it. On this blade, note hello service key for hello circuit. You must copy hello circuit key for your circuit and pass it down toohello service provider toocomplete hello provisioning process. hello circuit key is specific tooyour circuit.

![Tulajdonságok megtekintése](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

### <a name="4-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a><span data-ttu-id="ad74d-143">4. Kapcsolat kulcs tooyour hello szolgáltató küldése történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="ad74d-143">4. Send hello service key tooyour connectivity provider for provisioning</span></span>
<span data-ttu-id="ad74d-144">A panel **szolgáltató állapota** információt nyújt a hello aktuális állapotának kiépítés hello szolgáltatói oldalán.</span><span class="sxs-lookup"><span data-stu-id="ad74d-144">On this blade, **Provider status** provides information on hello current state of provisioning on hello service-provider side.</span></span> <span data-ttu-id="ad74d-145">**Állapot áramkör** hello Microsoft ügyféloldali hello állapot biztosít.</span><span class="sxs-lookup"><span data-stu-id="ad74d-145">**Circuit status** provides hello state on hello Microsoft side.</span></span> <span data-ttu-id="ad74d-146">Kiépítés állapotok áramkör kapcsolatos további információkért lásd: hello [munkafolyamatok](expressroute-workflows.md#expressroute-circuit-provisioning-states) cikk.</span><span class="sxs-lookup"><span data-stu-id="ad74d-146">For more information about circuit provisioning states, see hello [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) article.</span></span>

<span data-ttu-id="ad74d-147">Amikor létrehoz egy új ExpressRoute-kapcsolatcsoportot, hello áramkör hello állapota a következő lehet:</span><span class="sxs-lookup"><span data-stu-id="ad74d-147">When you create a new ExpressRoute circuit, hello circuit will be in hello following state:</span></span>

<span data-ttu-id="ad74d-148">Szolgáltató állapota: nincs telepítve</span><span class="sxs-lookup"><span data-stu-id="ad74d-148">Provider status: Not provisioned</span></span><BR>
<span data-ttu-id="ad74d-149">Áramkör állapot: engedélyezve</span><span class="sxs-lookup"><span data-stu-id="ad74d-149">Circuit status: Enabled</span></span>

![Üzembe helyezési folyamat elindítása](./media/expressroute-howto-circuit-portal-resource-manager/viewstatus.png)

<span data-ttu-id="ad74d-151">hello áramkör toohello hello folyamatán, amely lehetővé teszi a hello kapcsolat hitelesítésszolgáltató esetén a következő állapota változik:</span><span class="sxs-lookup"><span data-stu-id="ad74d-151">hello circuit will change toohello following state when hello connectivity provider is in hello process of enabling it for you:</span></span>

<span data-ttu-id="ad74d-152">Szolgáltató állapota: kiépítés</span><span class="sxs-lookup"><span data-stu-id="ad74d-152">Provider status: Provisioning</span></span><BR>
<span data-ttu-id="ad74d-153">Áramkör állapot: engedélyezve</span><span class="sxs-lookup"><span data-stu-id="ad74d-153">Circuit status: Enabled</span></span>

<span data-ttu-id="ad74d-154">Az Ön toobe képes toouse ExpressRoute-kapcsolatcsoportot az állapot a következő hello kell lennie:</span><span class="sxs-lookup"><span data-stu-id="ad74d-154">For you toobe able toouse an ExpressRoute circuit, it must be in hello following state:</span></span>

<span data-ttu-id="ad74d-155">Szolgáltató állapota: kiépítése</span><span class="sxs-lookup"><span data-stu-id="ad74d-155">Provider status: Provisioned</span></span><BR>
<span data-ttu-id="ad74d-156">Áramkör állapot: engedélyezve</span><span class="sxs-lookup"><span data-stu-id="ad74d-156">Circuit status: Enabled</span></span>

### <a name="5-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a><span data-ttu-id="ad74d-157">5. Rendszeres időközönként ellenőrizze a hello állapotát és hello áramkör kulcs hello állapota</span><span class="sxs-lookup"><span data-stu-id="ad74d-157">5. Periodically check hello status and hello state of hello circuit key</span></span>
<span data-ttu-id="ad74d-158">Megtekintheti, hogy az lehetőséget most érdekelt hello áramkör hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="ad74d-158">You can view hello properties of hello circuit that you're interested in by selecting it.</span></span> <span data-ttu-id="ad74d-159">Ellenőrizze a hello **szolgáltató állapota** , és győződjön meg arról, hogy túl van-e áthelyezése**kiépítve** a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="ad74d-159">Check hello **Provider status** and ensure that it has moved too**Provisioned** before you continue.</span></span>

![Kör és szolgáltató állapota](./media/expressroute-howto-circuit-portal-resource-manager/viewstatusprovisioned.png)

### <a name="6-create-your-routing-configuration"></a><span data-ttu-id="ad74d-161">6. Az útválasztó-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="ad74d-161">6. Create your routing configuration</span></span>
<span data-ttu-id="ad74d-162">Lépésenkénti útmutatásért tekintse meg a toohello [ExpressRoute-áramkör útválasztási konfigurációja](expressroute-howto-routing-portal-resource-manager.md) toocreate a következő cikket, és módosítsa a kapcsolatcsoport esetében.</span><span class="sxs-lookup"><span data-stu-id="ad74d-162">For step-by-step instructions, refer toohello [ExpressRoute circuit routing configuration](expressroute-howto-routing-portal-resource-manager.md) article toocreate and modify circuit peerings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ad74d-163">Ezek az utasítások csak a szolgáltatók által biztosított réteg 2 internetkapcsolati szolgáltatás használatával létrehozott toocircuits vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="ad74d-163">These instructions only apply toocircuits that are created with service providers that offer layer 2 connectivity services.</span></span> <span data-ttu-id="ad74d-164">A szolgáltató által kezelt használata réteg (általában az IP VPN, például az MPLS) 3 szolgáltatások, a kapcsolat szolgáltatójánál konfigurálása és kezelése az Ön útválasztást.</span><span class="sxs-lookup"><span data-stu-id="ad74d-164">If you're using a service provider that offers managed layer 3 services (typically an IP VPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

### <a name="7-link-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="ad74d-165">7. Hivatkozásra egy virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="ad74d-165">7. Link a virtual network tooan ExpressRoute circuit</span></span>
<span data-ttu-id="ad74d-166">A következő hivatkozás egy virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="ad74d-166">Next, link a virtual network tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="ad74d-167">Használjon hello [csatolása a virtuális hálózatok tooExpressRoute Kapcsolatcsoportok](expressroute-howto-linkvnet-arm.md) hello Resource Manager üzembe helyezési modellben való munka során a következő cikket.</span><span class="sxs-lookup"><span data-stu-id="ad74d-167">Use hello [Linking virtual networks tooExpressRoute circuits](expressroute-howto-linkvnet-arm.md) article when you work with hello Resource Manager deployment model.</span></span>

## <a name="getting-hello-status-of-an-expressroute-circuit"></a><span data-ttu-id="ad74d-168">Egy ExpressRoute-kapcsolatcsoportot hello állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="ad74d-168">Getting hello status of an ExpressRoute circuit</span></span>
<span data-ttu-id="ad74d-169">Expressroute-kapcsolatcsoporthoz hello állapotának megtekintéséhez jelölje ki.</span><span class="sxs-lookup"><span data-stu-id="ad74d-169">You can view hello status of a circuit by selecting it.</span></span> 

![Egy ExpressRoute-kapcsolatcsoport állapotát](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

## <a name="modifying-an-expressroute-circuit"></a><span data-ttu-id="ad74d-171">Egy ExpressRoute-kapcsolatcsoportot módosítása</span><span class="sxs-lookup"><span data-stu-id="ad74d-171">Modifying an ExpressRoute circuit</span></span>
<span data-ttu-id="ad74d-172">ExpressRoute-kapcsolatcsoportot egyes tulajdonságait módosíthatja kapcsolat befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="ad74d-172">You can modify certain properties of an ExpressRoute circuit without impacting connectivity.</span></span>

<span data-ttu-id="ad74d-173">Mindent hello állásidő nélkül a következő:</span><span class="sxs-lookup"><span data-stu-id="ad74d-173">You can do hello following with no downtime:</span></span>

* <span data-ttu-id="ad74d-174">Engedélyezi vagy letiltja az ExpressRoute-kapcsolatcsoportot prémium ExpressRoute bővítményt.</span><span class="sxs-lookup"><span data-stu-id="ad74d-174">Enable or disable an ExpressRoute premium add-on for your ExpressRoute circuit.</span></span>
* <span data-ttu-id="ad74d-175">Az ExpressRoute-kapcsolatcsoportot növekedése hello sávszélesség megadott érhető el kapacitás hello porton.</span><span class="sxs-lookup"><span data-stu-id="ad74d-175">Increase hello bandwidth of your ExpressRoute circuit provided there is capacity available on hello port.</span></span> <span data-ttu-id="ad74d-176">Vegye figyelembe, hogy a alacsonyabb verziójúra változtatása hello sávszélesség, a kapcsolat nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="ad74d-176">Note that downgrading hello bandwidth of a circuit is not supported.</span></span> 
* <span data-ttu-id="ad74d-177">Hello mérési adatok díjköteles tooUnlimited adatokat a terv módosítása</span><span class="sxs-lookup"><span data-stu-id="ad74d-177">Change hello metering plan from Metered Data tooUnlimited Data.</span></span> <span data-ttu-id="ad74d-178">Vegye figyelembe az adatok nem támogatott adatforgalmi tooMetered változó hello mérési terv.</span><span class="sxs-lookup"><span data-stu-id="ad74d-178">Note that changing hello metering plan from Unlimited Data tooMetered Data is not supported.</span></span>
* <span data-ttu-id="ad74d-179">Engedélyezheti és letilthatja *klasszikus műveletek engedélyezése*.</span><span class="sxs-lookup"><span data-stu-id="ad74d-179">You can enable and disable *Allow Classic Operations*.</span></span>

<span data-ttu-id="ad74d-180">További információ a korlátai és korlátozásai, tekintse meg a toohello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="ad74d-180">For more information on limits and limitations, refer toohello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

<span data-ttu-id="ad74d-181">toomodify ExpressRoute-kapcsolatcsoportot, kattintson a hello **konfigurációs** a hello az alábbi ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="ad74d-181">toomodify an ExpressRoute circuit, click on hello **Configuration** as shown in hello figure below.</span></span>

![Kör módosítása](./media/expressroute-howto-circuit-portal-resource-manager/modifycircuit.png)

<span data-ttu-id="ad74d-183">Hello sávszélesség, SKU, számlázási modellt módosíthatja, és engedélyezi a klasszikus műveletek hello konfigurációs panel belül.</span><span class="sxs-lookup"><span data-stu-id="ad74d-183">You can modify hello bandwidth, SKU, billing model and allow classic operations within hello configuration blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ad74d-184">Előfordulhat, hogy toorecreate hello ExpressRoute-kapcsolatcsoportot esetén nincs elég kapacitás hello meglévő porton.</span><span class="sxs-lookup"><span data-stu-id="ad74d-184">You may have toorecreate hello ExpressRoute circuit if there is inadequate capacity on hello existing port.</span></span> <span data-ttu-id="ad74d-185">Hello kapcsolatcsoport nem frissíthető, ha nincsenek további kapacitást érhető el az adott helyhez.</span><span class="sxs-lookup"><span data-stu-id="ad74d-185">You cannot upgrade hello circuit if there is no additional capacity available at that location.</span></span>
>
> <span data-ttu-id="ad74d-186">Nem csökken a hello sávszélesség az ExpressRoute-kapcsolatcsoportot megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="ad74d-186">You cannot reduce hello bandwidth of an ExpressRoute circuit without disruption.</span></span> <span data-ttu-id="ad74d-187">Alacsonyabb verziójúra változtatása sávszélesség toodeprovision hello ExpressRoute-kapcsolatcsoportot meg, és majd építenie az új ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="ad74d-187">Downgrading bandwidth requires you toodeprovision hello ExpressRoute circuit and then reprovision a new ExpressRoute circuit.</span></span>
> 
> <span data-ttu-id="ad74d-188">Prémium szintű bővítmény letiltása művelet sikertelen lesz, amely nagyobb, mint a megengedett hello szabványos áramkör erőforrások használata.</span><span class="sxs-lookup"><span data-stu-id="ad74d-188">Disable premium add-on operation can fail if you're using resources that are greater than what is permitted for hello standard circuit.</span></span>
> 
> 

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><span data-ttu-id="ad74d-189">Megszüntetés és az ExpressRoute-kapcsolatcsoport törlése</span><span class="sxs-lookup"><span data-stu-id="ad74d-189">Deprovisioning and deleting an ExpressRoute circuit</span></span>
<span data-ttu-id="ad74d-190">Az ExpressRoute-kapcsolatcsoport törlése hello kiválasztásával **törlése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ad74d-190">You can delete your ExpressRoute circuit by selecting hello **delete** icon.</span></span> <span data-ttu-id="ad74d-191">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="ad74d-191">Note hello following:</span></span>

* <span data-ttu-id="ad74d-192">Az ExpressRoute-kapcsolatcsoportot hello összes virtuális hálózatot kell választható.</span><span class="sxs-lookup"><span data-stu-id="ad74d-192">You must unlink all virtual networks from hello ExpressRoute circuit.</span></span> <span data-ttu-id="ad74d-193">Ha ez a művelet sikertelen, ellenőrizze, hogy a virtuális hálózatok vannak-e kapcsolva toohello körön.</span><span class="sxs-lookup"><span data-stu-id="ad74d-193">If this operation fails, check whether any virtual networks are linked toohello circuit.</span></span>
* <span data-ttu-id="ad74d-194">Ha hello ExpressRoute körön szolgáltató üzembe helyezési állapota **kiépítési** vagy **kiépítve** együttműködve kell a service provider toodeprovision hello kapcsolatcsoport az oldalon.</span><span class="sxs-lookup"><span data-stu-id="ad74d-194">If hello ExpressRoute circuit service provider provisioning state is **Provisioning** or **Provisioned** you must work with your service provider toodeprovision hello circuit on their side.</span></span> <span data-ttu-id="ad74d-195">Rendszer továbbra is tooreserve erőforrások és kiszámlázni Önnek, amíg hello szolgáltató megszüntetési hello áramkör befejeződik, és értesíti a számunkra.</span><span class="sxs-lookup"><span data-stu-id="ad74d-195">We will continue tooreserve resources and bill you until hello service provider completes deprovisioning hello circuit and notifies us.</span></span>
* <span data-ttu-id="ad74d-196">Ha hello szolgáltató rendelkezik platformelőfizetés hello áramkör (üzembe helyezési állapota hello szolgáltató értéke túl**nincs kiépítve**) hello áramkör törölhet.</span><span class="sxs-lookup"><span data-stu-id="ad74d-196">If hello service provider has deprovisioned hello circuit (hello service provider provisioning state is set too**Not provisioned**) you can then delete hello circuit.</span></span> <span data-ttu-id="ad74d-197">Ezzel leállítja a számlázási hello kör</span><span class="sxs-lookup"><span data-stu-id="ad74d-197">This will stop billing for hello circuit</span></span>

## <a name="next-steps"></a><span data-ttu-id="ad74d-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ad74d-198">Next steps</span></span>
<span data-ttu-id="ad74d-199">Miután létrehozta a kapcsolatcsoport, győződjön meg arról, hogy Ön hello a következő:</span><span class="sxs-lookup"><span data-stu-id="ad74d-199">After you create your circuit, make sure that you do hello following:</span></span>

* [<span data-ttu-id="ad74d-200">Létrehozásához és módosításához az ExpressRoute-kapcsolatcsoport esetében routing</span><span class="sxs-lookup"><span data-stu-id="ad74d-200">Create and modify routing for your ExpressRoute circuit</span></span>](expressroute-howto-routing-portal-resource-manager.md)
* [<span data-ttu-id="ad74d-201">A virtuális hálózati tooyour ExpressRoute-kapcsolatcsoportot hivatkozás</span><span class="sxs-lookup"><span data-stu-id="ad74d-201">Link your virtual network tooyour ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)

