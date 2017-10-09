---
title: "A virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot hivatkozás: Azure-portál |} Microsoft Docs"
description: "Ez a dokumentum áttekintést hogyan toolink virtuális hálózatok (Vnetek) tooExpressRoute kapcsolatok."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f5cb5441-2fba-46d9-99a5-d1d586e7bda4
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: cherylmc
ms.openlocfilehash: 8bedcb11df7e30281fd439afdfb76cc67626a8f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit"></a><span data-ttu-id="bf0f1-103">Csatlakozás a virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="bf0f1-103">Connect a virtual network tooan ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bf0f1-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bf0f1-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="bf0f1-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bf0f1-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="bf0f1-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="bf0f1-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="bf0f1-107">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="bf0f1-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="bf0f1-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="bf0f1-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
> 

<span data-ttu-id="bf0f1-109">Ez a cikk segítséget nyújt a virtuális hálózatokon (Vnetek) tooAzure ExpressRoute-Kapcsolatcsoportok hivatkozás hello Resource Manager telepítési modell segítségével, és hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-109">This article helps you link virtual networks (VNets) tooAzure ExpressRoute circuits by using hello Resource Manager deployment model and hello Azure portal.</span></span> <span data-ttu-id="bf0f1-110">Virtuális hálózatok lehet a hello ugyanazt az előfizetést, vagy egy másik előfizetés része lehet.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-110">Virtual networks can either be in hello same subscription, or they can be part of another subscription.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="bf0f1-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="bf0f1-111">Before you begin</span></span>
* <span data-ttu-id="bf0f1-112">Felülvizsgálati hello [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-112">Review hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="bf0f1-113">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-113">You must have an active ExpressRoute circuit.</span></span>
  
  * <span data-ttu-id="bf0f1-114">Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-portal-resource-manager.md) és hello áramkör szerint a kapcsolat szolgáltatójánál engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-114">Follow hello instructions too[create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have hello circuit enabled by your connectivity provider.</span></span>
  * <span data-ttu-id="bf0f1-115">Győződjön meg arról, hogy rendelkezik az Azure magánhálózati társviszony-létesítés a kapcsolatcsoport konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-115">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="bf0f1-116">Lásd: hello [útválasztás konfigurálása](expressroute-howto-routing-portal-resource-manager.md) útválasztási miként.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-116">See hello [Configure routing](expressroute-howto-routing-portal-resource-manager.md) article for routing instructions.</span></span>
  * <span data-ttu-id="bf0f1-117">Győződjön meg arról, hogy az Azure magánhálózati társviszony-létesítés van konfigurálva, és hello BGP társviszony-létesítés a hálózat és a Microsoft között működik-e, hogy engedélyezheti a végpontok közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-117">Ensure that Azure private peering is configured and hello BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="bf0f1-118">Győződjön meg arról, hogy a virtuális hálózat és a virtuális hálózati átjáró létrehozása, és teljesen kiépítve.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-118">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="bf0f1-119">Útmutatás alapján hello túl[hozzon létre egy virtuális hálózati átjáró ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="bf0f1-119">Follow hello instructions too[create a virtual network gateway for ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="bf0f1-120">Az ExpressRoute a virtuális hálózati átjáró nem VPN GatewayType "ExpressRoute" hello használja.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-120">A virtual network gateway for ExpressRoute uses hello GatewayType 'ExpressRoute', not VPN.</span></span>

* <span data-ttu-id="bf0f1-121">Too10 virtuális hálózatok tooa szabványos ExpressRoute-kapcsolatcsoportot is csatolhatja.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-121">You can link up too10 virtual networks tooa standard ExpressRoute circuit.</span></span> <span data-ttu-id="bf0f1-122">Az összes virtuális hálózatot kell hello geopolitikai régión szabványos ExpressRoute-kapcsolatcsoportot használatakor.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-122">All virtual networks must be in hello same geopolitical region when using a standard ExpressRoute circuit.</span></span> 
* <span data-ttu-id="bf0f1-123">Egy virtuális hálózaton kívül hello geopolitikai területet hello ExpressRoute-kapcsolatcsoportot hivatkozásra, vagy csatlakoztassa a virtuális hálózatok tooyour ExpressRoute-kapcsolatcsoportot nagyobb számú, ha engedélyezte a hello ExpressRoute prémium szintű bővítmény.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-123">You can link a virtual network outside of hello geopolitical region of hello ExpressRoute circuit, or connect a larger number of virtual networks tooyour ExpressRoute circuit if you enabled hello ExpressRoute premium add-on.</span></span> <span data-ttu-id="bf0f1-124">Ellenőrizze a hello [– gyakori kérdések](expressroute-faqs.md) hello prémium szintű bővítmény olvashat.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-124">Check hello [FAQ](expressroute-faqs.md) for more details on hello premium add-on.</span></span>
* <span data-ttu-id="bf0f1-125">Is [videó](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) előtt kezdete toobetter megismerheti hello lépéseit.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-125">You can [view a video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) before beginning toobetter understand hello steps.</span></span>

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a><span data-ttu-id="bf0f1-126">A virtuális hálózat a hello azonos előfizetés tooa áramkör</span><span class="sxs-lookup"><span data-stu-id="bf0f1-126">Connect a virtual network in hello same subscription tooa circuit</span></span>

### <a name="toocreate-a-connection"></a><span data-ttu-id="bf0f1-127">a kapcsolat toocreate</span><span class="sxs-lookup"><span data-stu-id="bf0f1-127">toocreate a connection</span></span>

> [!NOTE]
> <span data-ttu-id="bf0f1-128">BGP-konfigurációs információk nem jelennek meg ha hello 3 rétegbeli konfigurálva, a esetében.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-128">BGP configuration information will not show up if hello layer 3 provider configured your peerings.</span></span> <span data-ttu-id="bf0f1-129">Ha a kapcsolatcsoport kiépített állapotban van, meg kell tudni toocreate kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-129">If your circuit is in a provisioned state, you should be able toocreate connections.</span></span>
>

1. <span data-ttu-id="bf0f1-130">Győződjön meg arról, hogy az ExpressRoute-kapcsolatcsoportot és Azure magánhálózati társviszony-létesítés rendelkezik konfigurálása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-130">Ensure that your ExpressRoute circuit and Azure private peering have been configured successfully.</span></span> <span data-ttu-id="bf0f1-131">Hello utasításait követve [ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-arm.md) és [útválasztás konfigurálása](expressroute-howto-routing-arm.md).</span><span class="sxs-lookup"><span data-stu-id="bf0f1-131">Follow hello instructions in [Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and [Configure routing](expressroute-howto-routing-arm.md).</span></span> <span data-ttu-id="bf0f1-132">Az ExpressRoute-kapcsolatcsoportot kép a következő hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="bf0f1-132">Your ExpressRoute circuit should look like hello following image:</span></span>

    ![Az ExpressRoute-kapcsolatcsoport képernyőképe](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
   
2. <span data-ttu-id="bf0f1-134">Most elindíthatja a kapcsolat toolink kiépítése a virtuális hálózati átjáró tooyour ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-134">You can now start provisioning a connection toolink your virtual network gateway tooyour ExpressRoute circuit.</span></span> <span data-ttu-id="bf0f1-135">Kattintson a **kapcsolat** > **Hozzáadás** tooopen hello **kapcsolat hozzáadása a** panelen, majd konfigurálja a hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-135">Click **Connection** > **Add** tooopen hello **Add connection** blade, and then configure hello values.</span></span>

    ![Vegye fel a kapcsolatot képernyőképe](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)  

3. <span data-ttu-id="bf0f1-137">Után a kapcsolat sikeresen konfigurálva, akkor a kapcsolati objektum hello kapcsolat hello információk jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-137">After your connection has been successfully configured, your connection object will show hello information for hello connection.</span></span>

     ![Kapcsolat objektum képernyőképe](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

### <a name="toodelete-a-connection"></a><span data-ttu-id="bf0f1-139">a kapcsolat toodelete</span><span class="sxs-lookup"><span data-stu-id="bf0f1-139">toodelete a connection</span></span>
<span data-ttu-id="bf0f1-140">A kapcsolat hello kiválasztásával törölheti **törlése** hello panelen a kapcsolat ikon.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-140">You can delete a connection by selecting hello **Delete** icon on hello blade for your connection.</span></span>

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a><span data-ttu-id="bf0f1-141">Egy másik előfizetésben található tooa kapcsolat a virtuális hálózatot</span><span class="sxs-lookup"><span data-stu-id="bf0f1-141">Connect a virtual network in a different subscription tooa circuit</span></span>
<span data-ttu-id="bf0f1-142">Több előfizetés ExpressRoute-kapcsolatcsoportot lehet megosztani.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-142">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="bf0f1-143">hello az alábbi ábra egy egyszerű sematikus ExpressRoute-Kapcsolatcsoportok megosztási hogyan működik a több előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-143">hello figure below shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

![Az előfizetések közötti kapcsolathoz](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- <span data-ttu-id="bf0f1-145">Hello kisebb felhők hello nagy felhőben egy szervezeten belül toodifferent részlegek tartozó használt toorepresent előfizetések.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-145">Each of hello smaller clouds within hello large cloud is used toorepresent subscriptions that belong toodifferent departments within an organization.</span></span>
- <span data-ttu-id="bf0f1-146">Hello szervezeten belül hello osztályok mindegyikének saját előfizetés használhatja a szolgáltatások telepítéséhez, de egyetlen ExpressRoute körön tooconnect hátsó tooyour a helyszíni hálózathoz is megoszthatják.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-146">Each of hello departments within hello organization can use their own subscription for deploying their services, but they can share a single ExpressRoute circuit tooconnect back tooyour on-premises network.</span></span>
- <span data-ttu-id="bf0f1-147">Egy részleghez (ebben a példában: informatikai) is saját hello ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-147">A single department (in this example: IT) can own hello ExpressRoute circuit.</span></span> <span data-ttu-id="bf0f1-148">Hello szervezeten belüli más előfizetésekkel használható hello ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-148">Other subscriptions within hello organization can use hello ExpressRoute circuit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bf0f1-149">Kapcsolat és a sávszélesség díjak dedikált hello kör lesz alkalmazott toohello ExpressRoute-kapcsolatcsoport tulajdonosát.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-149">Connectivity and bandwidth charges for hello dedicated circuit will be applied toohello ExpressRoute circuit owner.</span></span> <span data-ttu-id="bf0f1-150">Az összes virtuális hálózatot megosztása hello azonos sávszélesség.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-150">All virtual networks share hello same bandwidth.</span></span>
    > 
    >

### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="bf0f1-151">Felügyeleti - áramkör tulajdonosa és a kapcsolatcsoport felhasználók</span><span class="sxs-lookup"><span data-stu-id="bf0f1-151">Administration - circuit owners and circuit users</span></span>

<span data-ttu-id="bf0f1-152">hello "kapcsolatcsoport tulajdonosát" hello ExpressRoute-kapcsolatcsoport erőforrás Power jogosultsága.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-152">hello 'circuit owner' is an authorized Power User of hello ExpressRoute circuit resource.</span></span> <span data-ttu-id="bf0f1-153">hello kapcsolatcsoport tulajdonosát hozhat létre, amely a "kör felhasználók" is váltható engedélyek.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-153">hello circuit owner can create authorizations that can be redeemed by 'circuit users'.</span></span> <span data-ttu-id="bf0f1-154">Kör felhasználók tulajdonosai virtuális hálózati átjárók, amelyek nincsenek belül hello ugyanahhoz az előfizetéshez, ExpressRoute-kapcsolatcsoportot hello.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-154">Circuit users are owners of virtual network gateways that are not within hello same subscription as hello ExpressRoute circuit.</span></span> <span data-ttu-id="bf0f1-155">Kör felhasználók is beválthatja engedélyek (virtuális hálózatonként egy engedélyezési).</span><span class="sxs-lookup"><span data-stu-id="bf0f1-155">Circuit users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="bf0f1-156">hello kapcsolatcsoport tulajdonosát hello power toomodify és visszavonni az engedélyeket bármikor rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-156">hello circuit owner has hello power toomodify and revoke authorizations at any time.</span></span> <span data-ttu-id="bf0f1-157">Az összes hivatkozás kapcsolatok törlődnek, amelyek hozzáférését visszavonták hello előfizetésből engedélyezési eredményez visszavonása.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-157">Revoking an authorization results in all link connections being deleted from hello subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="bf0f1-158">Kapcsolatcsoport-tulajdonos műveletek</span><span class="sxs-lookup"><span data-stu-id="bf0f1-158">Circuit owner operations</span></span>

<span data-ttu-id="bf0f1-159">**egy kapcsolat hitelesítési toocreate**</span><span class="sxs-lookup"><span data-stu-id="bf0f1-159">**toocreate a connection authorization**</span></span>

<span data-ttu-id="bf0f1-160">hello kapcsolatcsoport tulajdonosát engedélyezési hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-160">hello circuit owner creates an authorization.</span></span> <span data-ttu-id="bf0f1-161">Az eredmény lehet engedélykulcs hello létrehozását a virtuális hálózati átjárók toohello ExpressRoute-kapcsolatcsoportot egy kör felhasználói tooconnect használják.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-161">This results in hello creation of an authorization key that can be used by a circuit user tooconnect their virtual network gateways toohello ExpressRoute circuit.</span></span> <span data-ttu-id="bf0f1-162">Az engedély csak egy kapcsolat érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-162">An authorization is valid for only one connection.</span></span>

1. <span data-ttu-id="bf0f1-163">Hello ExpressRoute paneljén kattintson **engedélyek** és írja be a **neve** hello engedélyezési, majd kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-163">In hello ExpressRoute blade, Click **Authorizations** and then type a **name** for hello authorization and click **Save**.</span></span>

    ![Engedélyek](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)

2. <span data-ttu-id="bf0f1-165">Ha mentett hello konfigurációs, másolja a hello **erőforrás-azonosító** és hello **Engedélykulcs**.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-165">Once hello configuration is saved, copy hello **Resource ID** and hello **Authorization Key**.</span></span>

    ![Hitelesítési kulcs](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

<span data-ttu-id="bf0f1-167">**egy kapcsolat hitelesítési toodelete**</span><span class="sxs-lookup"><span data-stu-id="bf0f1-167">**toodelete a connection authorization**</span></span>

<span data-ttu-id="bf0f1-168">A kapcsolat hello kiválasztásával törölheti **törlése** hello panelen a kapcsolat ikon.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-168">You can delete a connection by selecting hello **Delete** icon on hello blade for your connection.</span></span>

### <a name="circuit-user-operations"></a><span data-ttu-id="bf0f1-169">Kör felhasználói műveletek</span><span class="sxs-lookup"><span data-stu-id="bf0f1-169">Circuit user operations</span></span>

<span data-ttu-id="bf0f1-170">hello áramkör felhasználói hello erőforrás-azonosító és engedélykulcs hello kapcsolatcsoport tulajdonosát a kell.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-170">hello circuit user needs hello resource ID and an authorization key from hello circuit owner.</span></span> 

<span data-ttu-id="bf0f1-171">**egy kapcsolat hitelesítési tooredeem**</span><span class="sxs-lookup"><span data-stu-id="bf0f1-171">**tooredeem a connection authorization**</span></span>

1.  <span data-ttu-id="bf0f1-172">Kattintson a hello **+ új** gombra.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-172">Click hello **+New** button.</span></span>

    ![Új kattintson](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)

2.  <span data-ttu-id="bf0f1-174">Keresse meg **"Kapcsolat"** hello piactér, válassza ki azt, majd kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-174">Search for **"Connection"** in hello Marketplace, select it, and click **Create**.</span></span>

    ![Keresse meg a kapcsolat](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)

3.  <span data-ttu-id="bf0f1-176">Győződjön meg arról, hogy hello **kapcsolattípus** túl értéke "ExpressRoute".</span><span class="sxs-lookup"><span data-stu-id="bf0f1-176">Make sure hello **Connection type** is set too"ExpressRoute".</span></span>


4.  <span data-ttu-id="bf0f1-177">Töltse ki hello részleteit, majd kattintson az **OK** hello alapvető beállítások panelen.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-177">Fill in hello details, then click **OK** in hello Basics blade.</span></span>

    ![Alapvető beállítások panel](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)

5.  <span data-ttu-id="bf0f1-179">A hello **beállítások** panelen, jelölje be hello **virtuális hálózati átjáró** , és ellenőrizze a hello **engedélyezési beváltani** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-179">In hello **Settings** blade, Select hello **Virtual network gateway** and check hello **Redeem authorization** check box.</span></span>

6.  <span data-ttu-id="bf0f1-180">Adja meg a hello **engedélykulcs** és hello **áramkör URI partnert** és nevezze el hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-180">Enter hello **Authorization key** and hello **Peer circuit URI** and give hello connection a name.</span></span> <span data-ttu-id="bf0f1-181">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-181">Click **OK**.</span></span>

    ![Beállítások panel](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)

7. <span data-ttu-id="bf0f1-183">Tekintse át a hello hello információkat **összegzés** panel megnyitásához, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-183">Review hello information in hello **Summary** blade and click **OK**.</span></span>


<span data-ttu-id="bf0f1-184">**egy kapcsolat hitelesítési toorelease**</span><span class="sxs-lookup"><span data-stu-id="bf0f1-184">**toorelease a connection authorization**</span></span>

<span data-ttu-id="bf0f1-185">Az engedély törölni kell, amely a hello ExpressRoute körön toohello virtuális hálózati kapcsolat hello is megjelenhetnek.</span><span class="sxs-lookup"><span data-stu-id="bf0f1-185">You can release an authorization by deleting hello connection that links hello ExpressRoute circuit toohello virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf0f1-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bf0f1-186">Next steps</span></span>
<span data-ttu-id="bf0f1-187">ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="bf0f1-187">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
