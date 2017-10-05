---
title: "Munkafolyamatok konfigurálásához ExpressRoute-kapcsolatcsoportot |} Microsoft Docs"
description: "Ezen a lapon végigvezeti a munkafolyamatokat a ExpressRoute-kapcsolatcsoportot és társviszony konfigurálása"
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
ms.openlocfilehash: cba1b2cfee379e7d2b079bcb3089981ef1044d66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a><span data-ttu-id="6a0d5-103">Az ExpressRoute kapcsolatcsoport-kiépítési munkafolyamatai és a kapcsolatcsoportok állapotai</span><span class="sxs-lookup"><span data-stu-id="6a0d5-103">ExpressRoute workflows for circuit provisioning and circuit states</span></span>
<span data-ttu-id="6a0d5-104">Ezen a lapon végigvezeti a szolgáltatás üzembe helyezési és konfigurációs munkafolyamatok útválasztási magas szinten.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-104">This page walks you through the service provisioning and routing configuration workflows at a high level.</span></span>

![](./media/expressroute-workflows/expressroute-circuit-workflow.png)

<span data-ttu-id="6a0d5-105">Az alábbi ábra és a megfelelő lépéseket kell követnie ahhoz, hogy rendelkezik kiépített ExpressRoute-kapcsolatcsoportot feladatokat jeleníti végpontok közötti.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-105">The following figure and corresponding steps show the tasks you must follow in order to have an ExpressRoute circuit provisioned end-to-end.</span></span> 

1. <span data-ttu-id="6a0d5-106">PowerShell segítségével konfigurálhatja az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-106">Use PowerShell to configure an ExpressRoute circuit.</span></span> <span data-ttu-id="6a0d5-107">Kövesse az utasításokat a [létrehozása ExpressRoute-Kapcsolatcsoportok](expressroute-howto-circuit-classic.md) cikkben olvashat.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-107">Follow the instructions in the [Create ExpressRoute circuits](expressroute-howto-circuit-classic.md) article for more details.</span></span>
2. <span data-ttu-id="6a0d5-108">A szolgáltató közötti kapcsolatot sorrendje.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-108">Order connectivity from the service provider.</span></span> <span data-ttu-id="6a0d5-109">Ez a folyamat függően változik.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-109">This process varies.</span></span> <span data-ttu-id="6a0d5-110">Kapcsolat rendezése kapcsolatos további részletekért forduljon a kapcsolat szolgáltatóját.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-110">Contact your connectivity provider for more details about how to order connectivity.</span></span>
3. <span data-ttu-id="6a0d5-111">Győződjön meg arról, hogy a kapcsolatcsoport van kiépítve sikeresen üzembe helyezési állapota a Powershellen keresztül ExpressRoute-kapcsolatcsoportot ellenőrzésével.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-111">Ensure that the circuit has been provisioned successfully by verifying the ExpressRoute circuit provisioning state through PowerShell.</span></span> 
4. <span data-ttu-id="6a0d5-112">Útválasztási tartományok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-112">Configure routing domains.</span></span> <span data-ttu-id="6a0d5-113">Ha a kapcsolat szolgáltatójánál, 3. rétegbeli kezeli, a vállalat konfigurálja a kapcsolatcsoport útválasztást.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-113">If your connectivity provider manages Layer 3 for you, they will configure routing for your circuit.</span></span> <span data-ttu-id="6a0d5-114">Ha a kapcsolat szolgáltatójánál csak a 2. rétegbeli szolgáltatásokat biztosít, konfigurálnia kell egy leírt irányelveket útválasztási a [útválasztási követelmények](expressroute-routing.md) és [útválasztási konfigurációja](expressroute-howto-routing-classic.md) lapokat.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-114">If your connectivity provider only offers Layer 2 services, you must configure routing per guidelines described in the [routing requirements](expressroute-routing.md) and [routing configuration](expressroute-howto-routing-classic.md) pages.</span></span>
   
   * <span data-ttu-id="6a0d5-115">Azure magánhálózati társviszony-létesítés engedélyezése – engedélyeznie kell a társviszony csatlakozni a virtuális gépek / felhőszolgáltatások telepített virtuális hálózatokon belül.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-115">Enable Azure private peering - You must enable this peering to connect to VMs / cloud services deployed within virtual networks.</span></span>
   * <span data-ttu-id="6a0d5-116">Engedélyezze az Azure nyilvános társviszony - engedélyeznie kell az Azure nyilvános társviszony-létesítés Ha nyilvános IP-címek futó Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-116">Enable Azure public peering - You must enable Azure public peering if you wish to connect to Azure services hosted on public IP addresses.</span></span> <span data-ttu-id="6a0d5-117">Ez a követelmény az Azure-erőforrások eléréséhez, ha úgy döntött, az Azure magánhálózati társviszony-létesítés útválasztás alapértelmezett engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-117">This is a requirement to access Azure resources if you have chosen to enable default routing for Azure private peering.</span></span>
   * <span data-ttu-id="6a0d5-118">Engedélyezze a Microsoft társviszony - hozzáférés Office 365 és Dynamics 365 engedélyeznie kell ezt.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-118">Enable Microsoft peering - You must enable this to access Office 365 and Dynamics 365.</span></span> 
     
     > [!IMPORTANT]
     > <span data-ttu-id="6a0d5-119">Győződjön meg arról, hogy egy külön proxyt használ, / Edge böngésző számára csatlakoztatni a Microsoft a akkor használata az interneten.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-119">You must ensure that you use a separate proxy / edge to connect to Microsoft than the one you use for the Internet.</span></span> <span data-ttu-id="6a0d5-120">Az azonos él használ ExpressRoute és az internetről is aszimmetrikus útválasztási okozhat, és következtében a hálózati kapcsolat kimaradások.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-120">Using the same edge for both ExpressRoute and the Internet will cause asymmetric routing and cause connectivity outages for your network.</span></span>
     > 
     > 
     
     ![](./media/expressroute-workflows/routing-workflow.png)
5. <span data-ttu-id="6a0d5-121">Virtuális hálózatok csatolása az ExpressRoute-Kapcsolatcsoportok - hozzákapcsolhatja virtuális hálózatok az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-121">Linking virtual networks to ExpressRoute circuits - You can link virtual networks to your ExpressRoute circuit.</span></span> <span data-ttu-id="6a0d5-122">Kövesse az utasításokat [Vnetek csatolásához](expressroute-howto-linkvnet-arm.md) a kapcsolatcsoport számára.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-122">Follow instructions [to link VNets](expressroute-howto-linkvnet-arm.md) to your circuit.</span></span> <span data-ttu-id="6a0d5-123">A Vnetek, ExpressRoute-kapcsolatcsoport Azure ugyanahhoz az előfizetéshez lehet, vagy lehet egy másik előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-123">These VNets can either be in the same Azure subscription as the ExpressRoute circuit, or can be in a different subscription.</span></span>

## <a name="expressroute-circuit-provisioning-states"></a><span data-ttu-id="6a0d5-124">Kiépítés állapotok ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="6a0d5-124">ExpressRoute circuit provisioning states</span></span>
<span data-ttu-id="6a0d5-125">Minden egyes ExpressRoute-kapcsolatcsoportot két állapota van:</span><span class="sxs-lookup"><span data-stu-id="6a0d5-125">Each ExpressRoute circuit has two states:</span></span>

* <span data-ttu-id="6a0d5-126">Szolgáltatás szolgáltató üzembe helyezési állapota</span><span class="sxs-lookup"><span data-stu-id="6a0d5-126">Service provider provisioning state</span></span>
* <span data-ttu-id="6a0d5-127">status</span><span class="sxs-lookup"><span data-stu-id="6a0d5-127">Status</span></span>

<span data-ttu-id="6a0d5-128">Állapotát a Microsoft a kiépítési állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-128">Status represents Microsoft's provisioning state.</span></span> <span data-ttu-id="6a0d5-129">Ez a tulajdonság engedélyezve van beállítva, ha Expressroute-kapcsolatcsoportot létrehozni</span><span class="sxs-lookup"><span data-stu-id="6a0d5-129">This property is set to Enabled when you create an Expressroute circuit</span></span>

<span data-ttu-id="6a0d5-130">A kapcsolati szolgáltató üzembe helyezési állapota a kapcsolat szolgáltatójánál oldalon állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-130">The connectivity provider provisioning state represents the state on the connectivity provider's side.</span></span> <span data-ttu-id="6a0d5-131">Ez lehet *NotProvisioned*, *kiépítési*, vagy *kiépítve*.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-131">It can either be *NotProvisioned*, *Provisioning*, or *Provisioned*.</span></span> <span data-ttu-id="6a0d5-132">Az ExpressRoute-kapcsolatcsoport is használni tudja kiépítve állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-132">The ExpressRoute circuit must be in Provisioned state for you to be able to use it.</span></span>

### <a name="possible-states-of-an-expressroute-circuit"></a><span data-ttu-id="6a0d5-133">Az ExpressRoute-kapcsolatcsoportot lehetséges állapota</span><span class="sxs-lookup"><span data-stu-id="6a0d5-133">Possible states of an ExpressRoute circuit</span></span>
<span data-ttu-id="6a0d5-134">Ez a szakasz ki a lehetséges állapotok az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-134">This section lists out the possible states for an ExpressRoute circuit.</span></span>

<span data-ttu-id="6a0d5-135">**A létrehozás időpontjában**</span><span class="sxs-lookup"><span data-stu-id="6a0d5-135">**At creation time**</span></span>

<span data-ttu-id="6a0d5-136">A következő állapotot okozta ExpressRoute-kapcsolatcsoportot megjelenik, amint az ExpressRoute-kapcsolatcsoportot létrehozni a PowerShell-parancsmag futtatása.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-136">You will see the ExpressRoute circuit in the following state as soon as you run the PowerShell cmdlet to create the ExpressRoute circuit.</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="6a0d5-137">**Amikor kapcsolat szolgáltatójánál éppen a kapcsolatcsoport kiépítése**</span><span class="sxs-lookup"><span data-stu-id="6a0d5-137">**When connectivity provider is in the process of provisioning the circuit**</span></span>

<span data-ttu-id="6a0d5-138">Az ExpressRoute-kapcsolatcsoport a következő állapotban megjelenik, amint a szolgáltatás kulcs átadása a kapcsolat szolgáltatóját, és azok elindította a telepítési folyamatot.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-138">You will see the ExpressRoute circuit in the following state as soon as you pass the service key to the connectivity provider and they have started the provisioning process.</span></span>

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


<span data-ttu-id="6a0d5-139">**Ha a kapcsolat szolgáltatóját az üzembe helyezési folyamat befejeződött**</span><span class="sxs-lookup"><span data-stu-id="6a0d5-139">**When connectivity provider has completed the provisioning process**</span></span>

<span data-ttu-id="6a0d5-140">A következő állapotot okozta ExpressRoute-kapcsolatcsoportot jelenik meg, amint a kapcsolat szolgáltatójánál a kiépítési folyamat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-140">You will see the ExpressRoute circuit in the following state as soon as the connectivity provider has completed the provisioning process.</span></span>

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled

<span data-ttu-id="6a0d5-141">Üzembe helyezve, és engedélyezve a kapcsolatcsoport egyetlen állapotát is használni tudja lehet.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-141">Provisioned and Enabled is the only state the circuit can be in for you to be able to use it.</span></span> <span data-ttu-id="6a0d5-142">Ha egy 2. rétegbeli szolgáltatót használja, beállíthatja a kapcsolatcsoport útválasztást csak akkor, ha az ebben az állapotban van.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-142">If you are using a Layer 2 provider, you can configure routing for your circuit only when it is in this state.</span></span>

<span data-ttu-id="6a0d5-143">**Ha a kapcsolat szolgáltatójánál megszüntetés a kapcsolatcsoport van**</span><span class="sxs-lookup"><span data-stu-id="6a0d5-143">**When connectivity provider is deprovisioning the circuit**</span></span>

<span data-ttu-id="6a0d5-144">A szolgáltató az ExpressRoute-kapcsolatcsoport kiosztásának megszüntetése a kért a következő állapotának beállítása után a szolgáltató a megszüntetési folyamatot a kapcsolatcsoport látják.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-144">If you requested the service provider to deprovision the ExpressRoute circuit, you will see the circuit set to the following state after the service provider has completed the deprovisioning process.</span></span>

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


<span data-ttu-id="6a0d5-145">Ha szeretné, engedélyezheti, ha szükséges, vagy a PowerShell-parancsmagok a kapcsolatcsoport törléséhez.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-145">You can choose to re-enable it if needed, or run PowerShell cmdlets to delete the circuit.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="6a0d5-146">Ha törli a kapcsolatcsoport a ServiceProviderProvisioningState kiépítésekor vagy kiépítve a művelet sikertelen lesz a PowerShell-parancsmagot kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-146">If you run the PowerShell cmdlet to delete the circuit when the ServiceProviderProvisioningState is Provisioning or Provisioned the operation will fail.</span></span> <span data-ttu-id="6a0d5-147">A kapcsolat szolgáltatójánál kiosztásának megszüntetése először az ExpressRoute-kapcsolatcsoport adjon működnek, és törölje a kapcsolatcsoport.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-147">Please work with your connectivity provider to deprovision the ExpressRoute circuit first and then delete the circuit.</span></span> <span data-ttu-id="6a0d5-148">A Microsoft továbbra is a kapcsolatcsoport számlázási, amíg nem futtat a kapcsolatcsoport törlése a PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-148">Microsoft will continue to bill the circuit until you run the PowerShell cmdlet to delete the circuit.</span></span>
> 
> 

## <a name="routing-session-configuration-state"></a><span data-ttu-id="6a0d5-149">Munkamenet-konfiguráció útválasztási állapota</span><span class="sxs-lookup"><span data-stu-id="6a0d5-149">Routing session configuration state</span></span>
<span data-ttu-id="6a0d5-150">A BGP üzembe helyezési állapota értesíti Önt arról, ha a BGP-munkamenetet a Microsoft edge engedélyezve lett.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-150">The BGP provisioning state lets you know if the BGP session has been enabled on the Microsoft edge.</span></span> <span data-ttu-id="6a0d5-151">Az állapot meg fogja tudni használni a társviszony-létesítést engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-151">The state must be enabled for you to be able to use the peering.</span></span>

<span data-ttu-id="6a0d5-152">Fontos, különösen a Microsoft társviszony-létesítés BGP munkamenet-állapot ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-152">It is important to check the BGP session state especially for Microsoft peering.</span></span> <span data-ttu-id="6a0d5-153">Mellett a BGP üzembe helyezési állapota, van egy másik állapothoz nevű *meghirdetett nyilvános előtag állapot*.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-153">In addition to the BGP provisioning state, there is another state called *advertised public prefixes state*.</span></span> <span data-ttu-id="6a0d5-154">A meghirdetett nyilvános előtag állapotban kell lennie a *konfigurált* állapot, mind a BGP-munkamenethez be kell, és a végpontok közötti működéséhez irányításához.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-154">The advertised public prefixes state must be in *configured* state, both for the BGP session to be up and for your routing to work end-to-end.</span></span> 

<span data-ttu-id="6a0d5-155">Ha a meghirdetett nyilvános előtag állapot beállítása a *szükséges érvényesítési* állapotba kerül, a BGP-munkamenet nincs engedélyezve, mint a hirdetett előtagok nem egyezik a AS számot sem az útválasztási nyilvántartó.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-155">If the advertised public prefix state is set to a *validation needed* state, the BGP session is not enabled, as the advertised prefixes did not match the AS number in any of the routing registries.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="6a0d5-156">Ha a meghirdetett nyilvános előtag állapota *manuális érvényesítési* állapotba kerül, meg kell nyitnia a támogatási jegy [Microsoft támogatási szolgálatához](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , és adja meg a megbizonyosodhat róla, hogy Ön a tulajdonosa az IP-címek meghirdetett mentén társított autonóm rendszer számát.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-156">If the advertised public prefixes state is in *manual validation* state, you must open a support ticket with [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) and provide evidence that you own the IP addresses advertised along with the associated Autonomous System number.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="6a0d5-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6a0d5-157">Next steps</span></span>
* <span data-ttu-id="6a0d5-158">Az ExpressRoute-kapcsolat konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6a0d5-158">Configure your ExpressRoute connection.</span></span>
  
  * [<span data-ttu-id="6a0d5-159">ExpressRoute-kapcsolatcsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="6a0d5-159">Create an ExpressRoute circuit</span></span>](expressroute-howto-circuit-arm.md)
  * [<span data-ttu-id="6a0d5-160">Útválasztás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6a0d5-160">Configure routing</span></span>](expressroute-howto-routing-arm.md)
  * [<span data-ttu-id="6a0d5-161">VNet csatlakoztatása egy ExpressRoute-kapcsolatcsoporthoz</span><span class="sxs-lookup"><span data-stu-id="6a0d5-161">Link a VNet to an ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-arm.md)

