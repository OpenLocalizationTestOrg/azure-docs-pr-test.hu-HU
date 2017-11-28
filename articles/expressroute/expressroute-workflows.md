---
title: "az ExpressRoute-kapcsolatcsoportot konfigurálása aaaWorkflows |} Microsoft Docs"
description: "Ezen a lapon bemutatja, hogyan hello munkafolyamatainak ExpressRoute-kapcsolatcsoportot és társviszony konfigurálása"
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 55e0418c-e0bf-44a7-9aa1-720076df9297
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: cherylmc
ms.openlocfilehash: 8e1dfc137401e0d6d53608ae6c8de0085e182eba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a><span data-ttu-id="6d9f4-103">Az ExpressRoute kapcsolatcsoport-kiépítési munkafolyamatai és a kapcsolatcsoportok állapotai</span><span class="sxs-lookup"><span data-stu-id="6d9f4-103">ExpressRoute workflows for circuit provisioning and circuit states</span></span>
<span data-ttu-id="6d9f4-104">Ezen a lapon bemutatja, hogyan hello szolgáltatás üzembe helyezési és útválasztási konfigurációs munkafolyamatok magas szinten.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-104">This page walks you through hello service provisioning and routing configuration workflows at a high level.</span></span>

![](./media/expressroute-workflows/expressroute-circuit-workflow.png)

<span data-ttu-id="6d9f4-105">hello alábbi ábra és a hozzá tartozó lépések hello feladatok megjelenítése követni kell rendelés toohave egy ExpressRoute körön kiosztott-végpontok.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-105">hello following figure and corresponding steps show hello tasks you must follow in order toohave an ExpressRoute circuit provisioned end-to-end.</span></span> 

1. <span data-ttu-id="6d9f4-106">Használjon PowerShell tooconfigure ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-106">Use PowerShell tooconfigure an ExpressRoute circuit.</span></span> <span data-ttu-id="6d9f4-107">Hello hello utasításait követve [létrehozása ExpressRoute-Kapcsolatcsoportok](expressroute-howto-circuit-classic.md) cikkben olvashat.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-107">Follow hello instructions in hello [Create ExpressRoute circuits](expressroute-howto-circuit-classic.md) article for more details.</span></span>
2. <span data-ttu-id="6d9f4-108">Rendelés kapcsolat hello-szolgáltatótól.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-108">Order connectivity from hello service provider.</span></span> <span data-ttu-id="6d9f4-109">Ez a folyamat függően változik.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-109">This process varies.</span></span> <span data-ttu-id="6d9f4-110">A kapcsolat szolgáltatójánál kapcsolatos további részletekért forduljon tooorder kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-110">Contact your connectivity provider for more details about how tooorder connectivity.</span></span>
3. <span data-ttu-id="6d9f4-111">Győződjön meg arról, hogy hello áramkör van kiépítve sikeresen üzembe helyezési állapota a Powershellen keresztül hello ExpressRoute-kapcsolatcsoportot ellenőrzésével.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-111">Ensure that hello circuit has been provisioned successfully by verifying hello ExpressRoute circuit provisioning state through PowerShell.</span></span> 
4. <span data-ttu-id="6d9f4-112">Útválasztási tartományok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-112">Configure routing domains.</span></span> <span data-ttu-id="6d9f4-113">Ha a kapcsolat szolgáltatójánál, 3. rétegbeli kezeli, a vállalat konfigurálja a kapcsolatcsoport útválasztást.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-113">If your connectivity provider manages Layer 3 for you, they will configure routing for your circuit.</span></span> <span data-ttu-id="6d9f4-114">Ha a kapcsolat szolgáltatójánál csak a 2. rétegbeli szolgáltatásokat biztosít, konfigurálnia kell egy hello leírt irányelveket útválasztási [útválasztási követelmények](expressroute-routing.md) és [útválasztási konfigurációja](expressroute-howto-routing-classic.md) lapokat.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-114">If your connectivity provider only offers Layer 2 services, you must configure routing per guidelines described in hello [routing requirements](expressroute-routing.md) and [routing configuration](expressroute-howto-routing-classic.md) pages.</span></span>
   
   * <span data-ttu-id="6d9f4-115">Azure magánhálózati társviszony-létesítés engedélyezése – kell engedélyezni a társviszony-létesítési tooconnect tooVMs / felhőszolgáltatások telepített virtuális hálózatokon belül.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-115">Enable Azure private peering - You must enable this peering tooconnect tooVMs / cloud services deployed within virtual networks.</span></span>
   * <span data-ttu-id="6d9f4-116">Engedélyezze az Azure nyilvános társviszony - engedélyeznie kell az Azure nyilvános társviszony Ha tooconnect tooAzure alkalmazáskönyvtár nyilvános IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-116">Enable Azure public peering - You must enable Azure public peering if you wish tooconnect tooAzure services hosted on public IP addresses.</span></span> <span data-ttu-id="6d9f4-117">Ez az a követelmény tooaccess Azure-erőforrások, ha a kiválasztott tooenable alapértelmezett útválasztást Azure magánhálózati társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-117">This is a requirement tooaccess Azure resources if you have chosen tooenable default routing for Azure private peering.</span></span>
   * <span data-ttu-id="6d9f4-118">Engedélyezze a Microsoft társviszony - engedélyeznie kell az Office 365 és Dynamics 365 tooaccess.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-118">Enable Microsoft peering - You must enable this tooaccess Office 365 and Dynamics 365.</span></span> 
     
     > [!IMPORTANT]
     > <span data-ttu-id="6d9f4-119">Győződjön meg arról, hogy egy külön proxy használata / biztonsági tooconnect tooMicrosoft hello egyikét használja, mint hello Internet.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-119">You must ensure that you use a separate proxy / edge tooconnect tooMicrosoft than hello one you use for hello Internet.</span></span> <span data-ttu-id="6d9f4-120">Az ExpressRoute- és hello Internet ugyanazt biztonsági fog okozhat, aszimmetrikus Útválasztás és a kapcsolat üzemszünet korlátozza a hálózat hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-120">Using hello same edge for both ExpressRoute and hello Internet will cause asymmetric routing and cause connectivity outages for your network.</span></span>
     > 
     > 
     
     ![](./media/expressroute-workflows/routing-workflow.png)
5. <span data-ttu-id="6d9f4-121">TooExpressRoute Kapcsolatcsoportok csatolása a virtuális hálózatok, mert a virtuális hálózatok tooyour ExpressRoute-kapcsolatcsoportot társíthatja.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-121">Linking virtual networks tooExpressRoute circuits - You can link virtual networks tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="6d9f4-122">Kövesse az utasításokat [toolink Vnetek](expressroute-howto-linkvnet-arm.md) tooyour körön.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-122">Follow instructions [toolink VNets](expressroute-howto-linkvnet-arm.md) tooyour circuit.</span></span> <span data-ttu-id="6d9f4-123">A Vnetek lehet a azonos Azure-előfizetéssel, hello ExpressRoute-kapcsolatcsoportot hello, vagy egy másik előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-123">These VNets can either be in hello same Azure subscription as hello ExpressRoute circuit, or can be in a different subscription.</span></span>

## <a name="expressroute-circuit-provisioning-states"></a><span data-ttu-id="6d9f4-124">Kiépítés állapotok ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="6d9f4-124">ExpressRoute circuit provisioning states</span></span>
<span data-ttu-id="6d9f4-125">Minden egyes ExpressRoute-kapcsolatcsoportot két állapota van:</span><span class="sxs-lookup"><span data-stu-id="6d9f4-125">Each ExpressRoute circuit has two states:</span></span>

* <span data-ttu-id="6d9f4-126">Szolgáltatás szolgáltató üzembe helyezési állapota</span><span class="sxs-lookup"><span data-stu-id="6d9f4-126">Service provider provisioning state</span></span>
* <span data-ttu-id="6d9f4-127">status</span><span class="sxs-lookup"><span data-stu-id="6d9f4-127">Status</span></span>

<span data-ttu-id="6d9f4-128">Állapotát a Microsoft a kiépítési állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-128">Status represents Microsoft's provisioning state.</span></span> <span data-ttu-id="6d9f4-129">A tulajdonság értéke tooEnabled, amikor az Expressroute-kapcsolatcsoportot létrehozni</span><span class="sxs-lookup"><span data-stu-id="6d9f4-129">This property is set tooEnabled when you create an Expressroute circuit</span></span>

<span data-ttu-id="6d9f4-130">hello kapcsolati szolgáltató üzembe helyezési állapota hello állapotát hello kapcsolat szolgáltatójánál oldalán jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-130">hello connectivity provider provisioning state represents hello state on hello connectivity provider's side.</span></span> <span data-ttu-id="6d9f4-131">Ez lehet *NotProvisioned*, *kiépítési*, vagy *kiépítve*.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-131">It can either be *NotProvisioned*, *Provisioning*, or *Provisioned*.</span></span> <span data-ttu-id="6d9f4-132">hello ExpressRoute-kapcsolatcsoportot állapotban kell lennie kiépítve az Ön toobe képes toouse azt.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-132">hello ExpressRoute circuit must be in Provisioned state for you toobe able toouse it.</span></span>

### <a name="possible-states-of-an-expressroute-circuit"></a><span data-ttu-id="6d9f4-133">Az ExpressRoute-kapcsolatcsoportot lehetséges állapota</span><span class="sxs-lookup"><span data-stu-id="6d9f4-133">Possible states of an ExpressRoute circuit</span></span>
<span data-ttu-id="6d9f4-134">Ez a rész felsorolja a kimenő hello lehetséges állapotok az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-134">This section lists out hello possible states for an ExpressRoute circuit.</span></span>

<span data-ttu-id="6d9f4-135">**A létrehozás időpontjában**</span><span class="sxs-lookup"><span data-stu-id="6d9f4-135">**At creation time**</span></span>

<span data-ttu-id="6d9f4-136">Hello ExpressRoute-kapcsolatcsoport állapotát, amint azt követő hello a hello PowerShell parancsmag toocreate hello ExpressRoute-kapcsolatcsoportot futtatása jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-136">You will see hello ExpressRoute circuit in hello following state as soon as you run hello PowerShell cmdlet toocreate hello ExpressRoute circuit.</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="6d9f4-137">**Amikor kapcsolat szolgáltatójánál hello áramkör kiépítés hello folyamatban van**</span><span class="sxs-lookup"><span data-stu-id="6d9f4-137">**When connectivity provider is in hello process of provisioning hello circuit**</span></span>

<span data-ttu-id="6d9f4-138">Látni fogja a hello ExpressRoute-kapcsolatcsoport állapotát, amint azt követő hello a hello szolgáltató kulcs toohello kapcsolatot adjon át, és azok elindította hello létesítésének folyamatát kell használnia.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-138">You will see hello ExpressRoute circuit in hello following state as soon as you pass hello service key toohello connectivity provider and they have started hello provisioning process.</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


<span data-ttu-id="6d9f4-139">**Ha a kapcsolat szolgáltatójánál befejeződött hello létesítésének folyamatát kell használnia**</span><span class="sxs-lookup"><span data-stu-id="6d9f4-139">**When connectivity provider has completed hello provisioning process**</span></span>

<span data-ttu-id="6d9f4-140">Látni fogja, amint hello kapcsolat szolgáltatójánál hello kiépítési folyamat befejeződött a következő állapotot hello az ExpressRoute-kapcsolatcsoportot hello.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-140">You will see hello ExpressRoute circuit in hello following state as soon as hello connectivity provider has completed hello provisioning process.</span></span>

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled

<span data-ttu-id="6d9f4-141">Üzembe helyezve, és engedélyezve csak hello állapot hello áramkör lehet az Ön toobe képes toouse azt.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-141">Provisioned and Enabled is hello only state hello circuit can be in for you toobe able toouse it.</span></span> <span data-ttu-id="6d9f4-142">Ha egy 2. rétegbeli szolgáltatót használja, beállíthatja a kapcsolatcsoport útválasztást csak akkor, ha az ebben az állapotban van.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-142">If you are using a Layer 2 provider, you can configure routing for your circuit only when it is in this state.</span></span>

<span data-ttu-id="6d9f4-143">**Ha a kapcsolat szolgáltatójánál megszüntetés hello áramkör van**</span><span class="sxs-lookup"><span data-stu-id="6d9f4-143">**When connectivity provider is deprovisioning hello circuit**</span></span>

<span data-ttu-id="6d9f4-144">A kért hello szolgáltatás szolgáltató toodeprovision hello ExpressRoute-kapcsolatcsoportot látják hello áramkör toohello hello szolgáltató hello megszüntetés folyamat befejeződését követően a következő állapot beállítása.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-144">If you requested hello service provider toodeprovision hello ExpressRoute circuit, you will see hello circuit set toohello following state after hello service provider has completed hello deprovisioning process.</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="6d9f4-145">Kiválaszthatja a toore engedélyezése, ha szükséges, vagy a PowerShell-parancsmagok futtatásához toodelete hello körön.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-145">You can choose toore-enable it if needed, or run PowerShell cmdlets toodelete hello circuit.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="6d9f4-146">Ha futtatja hello PowerShell parancsmag toodelete hello áramkör hello ServiceProviderProvisioningState kiépítési vagy kiépítve hello művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-146">If you run hello PowerShell cmdlet toodelete hello circuit when hello ServiceProviderProvisioningState is Provisioning or Provisioned hello operation will fail.</span></span> <span data-ttu-id="6d9f4-147">Adjon használata a kapcsolat szolgáltató toodeprovision hello ExpressRoute-kapcsolatcsoportot először, és törölje a hello körön.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-147">Please work with your connectivity provider toodeprovision hello ExpressRoute circuit first and then delete hello circuit.</span></span> <span data-ttu-id="6d9f4-148">Microsoft toobill hello áramkör folytatódik, amíg nem futtat PowerShell parancsmag toodelete hello áramkör hello.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-148">Microsoft will continue toobill hello circuit until you run hello PowerShell cmdlet toodelete hello circuit.</span></span>
> 
> 

## <a name="routing-session-configuration-state"></a><span data-ttu-id="6d9f4-149">Munkamenet-konfiguráció útválasztási állapota</span><span class="sxs-lookup"><span data-stu-id="6d9f4-149">Routing session configuration state</span></span>
<span data-ttu-id="6d9f4-150">hello BGP üzembe helyezési állapota értesíti Önt arról, ha hello BGP munkamenet engedélyezve van a Microsoft edge hello.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-150">hello BGP provisioning state lets you know if hello BGP session has been enabled on hello Microsoft edge.</span></span> <span data-ttu-id="6d9f4-151">hello állapot engedélyezni kell az Ön toobe képes toouse hello társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-151">hello state must be enabled for you toobe able toouse hello peering.</span></span>

<span data-ttu-id="6d9f4-152">Fontos toocheck hello BGP munkamenet-állapot kifejezetten a Microsoft társviszony-létesítést is.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-152">It is important toocheck hello BGP session state especially for Microsoft peering.</span></span> <span data-ttu-id="6d9f4-153">Továbbá toohello BGP üzembe helyezési állapota, van egy másik állapothoz nevű *meghirdetett nyilvános előtag állapot*.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-153">In addition toohello BGP provisioning state, there is another state called *advertised public prefixes state*.</span></span> <span data-ttu-id="6d9f4-154">hello meghirdetett nyilvános előtag állapotban kell lennie a *konfigurált* állapot, mind a hello BGP munkamenet toobe mentése és az útválasztási toowork-végpontok.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-154">hello advertised public prefixes state must be in *configured* state, both for hello BGP session toobe up and for your routing toowork end-to-end.</span></span> 

<span data-ttu-id="6d9f4-155">Ha hello meghirdetett nyilvános előtag állapot értéke tooa *szükséges érvényesítési* állapotba kerül, a BGP-munkamenetet hello nincs engedélyezve, hello hirdetett előtagok nem felelt meg a hello hello útválasztási nyilvántartó valamelyikében SZÁMOT.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-155">If hello advertised public prefix state is set tooa *validation needed* state, hello BGP session is not enabled, as hello advertised prefixes did not match hello AS number in any of hello routing registries.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="6d9f4-156">Hogy hello meghirdetett nyilvános előtag állapotban van-e *manuális érvényesítési* állapotba kerül, meg kell nyitnia a támogatási jegy [Microsoft támogatási szolgálatához](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , és adja meg a megbizonyosodhat róla, hogy Ön a tulajdonosa mentén meghirdetett hello IP-címek társított hello autonóm rendszer számát.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-156">If hello advertised public prefixes state is in *manual validation* state, you must open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) and provide evidence that you own hello IP addresses advertised along with hello associated Autonomous System number.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="6d9f4-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6d9f4-157">Next steps</span></span>
* <span data-ttu-id="6d9f4-158">Az ExpressRoute-kapcsolat konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6d9f4-158">Configure your ExpressRoute connection.</span></span>
  
  * [<span data-ttu-id="6d9f4-159">ExpressRoute-kapcsolatcsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d9f4-159">Create an ExpressRoute circuit</span></span>](expressroute-howto-circuit-arm.md)
  * [<span data-ttu-id="6d9f4-160">Útválasztás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6d9f4-160">Configure routing</span></span>](expressroute-howto-routing-arm.md)
  * [<span data-ttu-id="6d9f4-161">Hivatkozásra egy VNet tooan ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="6d9f4-161">Link a VNet tooan ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)

