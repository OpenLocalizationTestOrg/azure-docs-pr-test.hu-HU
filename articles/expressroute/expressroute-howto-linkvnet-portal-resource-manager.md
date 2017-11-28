---
title: "Virtuális hálózat csatolása ExpressRoute-kapcsolatcsoportot: Azure-portál |} Microsoft Docs"
description: "Ez a dokumentum áttekintést virtuális hálózatokról (Vnetekről) csatolása ExpressRoute-Kapcsolatcsoportok."
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
ms.openlocfilehash: 595c30ab5d9adc6061ad753d952adf894ba80b2f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit"></a><span data-ttu-id="5ed7e-103">Csatlakozás a virtuális hálózati ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="5ed7e-103">Connect a virtual network to an ExpressRoute circuit</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5ed7e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5ed7e-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="5ed7e-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5ed7e-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="5ed7e-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5ed7e-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="5ed7e-107">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5ed7e-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="5ed7e-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="5ed7e-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
> 

<span data-ttu-id="5ed7e-109">Ez a cikk segít virtuális hálózatokról (Vnetekről) hivatkozásra az Azure ExpressRoute-Kapcsolatcsoportok a Resource Manager üzembe helyezési modellel és az Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-109">This article helps you link virtual networks (VNets) to Azure ExpressRoute circuits by using the Resource Manager deployment model and the Azure portal.</span></span> <span data-ttu-id="5ed7e-110">Virtuális hálózatok lehet ugyanazt az előfizetést, vagy egy másik előfizetés részeként is lehetnek.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-110">Virtual networks can either be in the same subscription, or they can be part of another subscription.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="5ed7e-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="5ed7e-111">Before you begin</span></span>
* <span data-ttu-id="5ed7e-112">Tekintse át a [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-112">Review the [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
* <span data-ttu-id="5ed7e-113">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-113">You must have an active ExpressRoute circuit.</span></span>
  
  * <span data-ttu-id="5ed7e-114">Kövesse az utasításokat [ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-portal-resource-manager.md) és a kapcsolatcsoport szerint a kapcsolat szolgáltatójánál engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-114">Follow the instructions to [create an ExpressRoute circuit](expressroute-howto-circuit-portal-resource-manager.md) and have the circuit enabled by your connectivity provider.</span></span>
  * <span data-ttu-id="5ed7e-115">Győződjön meg arról, hogy rendelkezik az Azure magánhálózati társviszony-létesítés a kapcsolatcsoport konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-115">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="5ed7e-116">Tekintse meg a [útválasztás konfigurálása](expressroute-howto-routing-portal-resource-manager.md) útválasztási miként.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-116">See the [Configure routing](expressroute-howto-routing-portal-resource-manager.md) article for routing instructions.</span></span>
  * <span data-ttu-id="5ed7e-117">Győződjön meg arról, hogy az Azure magánhálózati társviszony-létesítés konfigurálva legyen, és a BGP társviszony-létesítés a hálózat és a Microsoft között működik-e, hogy engedélyezheti a végpontok közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-117">Ensure that Azure private peering is configured and the BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
  * <span data-ttu-id="5ed7e-118">Győződjön meg arról, hogy a virtuális hálózat és a virtuális hálózati átjáró létrehozása, és teljesen kiépítve.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-118">Ensure that you have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="5ed7e-119">Kövesse az utasításokat [hozzon létre egy virtuális hálózati átjáró ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="5ed7e-119">Follow the instructions to [create a virtual network gateway for ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="5ed7e-120">A virtuális hálózati átjáró ExpressRoute az "ExpressRoute", nem VPN GatewayType használja.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-120">A virtual network gateway for ExpressRoute uses the GatewayType 'ExpressRoute', not VPN.</span></span>

* <span data-ttu-id="5ed7e-121">Legfeljebb 10 virtuális hálózatok hozzákapcsolhatja egy szabványos ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-121">You can link up to 10 virtual networks to a standard ExpressRoute circuit.</span></span> <span data-ttu-id="5ed7e-122">Az összes virtuális hálózatot geopolitikai ugyanabban a régióban kell lennie, egy szabványos ExpressRoute-kapcsolatcsoportot használatakor.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-122">All virtual networks must be in the same geopolitical region when using a standard ExpressRoute circuit.</span></span> 
* <span data-ttu-id="5ed7e-123">Egy virtuális hálózaton kívül az ExpressRoute-kapcsolatcsoport geopolitikai régiója hivatkozásra, vagy virtuális hálózatok nagyobb számú csatlakozni az ExpressRoute-kapcsolatcsoportot, ha engedélyezte a prémium szintű ExpressRoute-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-123">You can link a virtual network outside of the geopolitical region of the ExpressRoute circuit, or connect a larger number of virtual networks to your ExpressRoute circuit if you enabled the ExpressRoute premium add-on.</span></span> <span data-ttu-id="5ed7e-124">Ellenőrizze a [gyakran ismételt kérdések](expressroute-faqs.md) a prémium szintű bővítmény olvashat.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-124">Check the [FAQ](expressroute-faqs.md) for more details on the premium add-on.</span></span>
* <span data-ttu-id="5ed7e-125">Is [videó](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) jobb megértése érdekében a lépések megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-125">You can [view a video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) before beginning to better understand the steps.</span></span>

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a><span data-ttu-id="5ed7e-126">A virtuális hálózati ugyanahhoz az előfizetéshez csatlakozni expressroute-kapcsolatcsoporthoz</span><span class="sxs-lookup"><span data-stu-id="5ed7e-126">Connect a virtual network in the same subscription to a circuit</span></span>

### <a name="to-create-a-connection"></a><span data-ttu-id="5ed7e-127">Kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ed7e-127">To create a connection</span></span>

> [!NOTE]
> <span data-ttu-id="5ed7e-128">BGP-konfigurációs információk nem jelennek meg ha a 3 rétegbeli a társviszony konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-128">BGP configuration information will not show up if the layer 3 provider configured your peerings.</span></span> <span data-ttu-id="5ed7e-129">Ha a kapcsolatcsoport kiépített állapotban van, hozhat létre kapcsolatokat kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-129">If your circuit is in a provisioned state, you should be able to create connections.</span></span>
>

1. <span data-ttu-id="5ed7e-130">Győződjön meg arról, hogy az ExpressRoute-kapcsolatcsoportot és Azure magánhálózati társviszony-létesítés rendelkezik konfigurálása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-130">Ensure that your ExpressRoute circuit and Azure private peering have been configured successfully.</span></span> <span data-ttu-id="5ed7e-131">Kövesse az utasításokat a [ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-arm.md) és [útválasztás konfigurálása](expressroute-howto-routing-arm.md).</span><span class="sxs-lookup"><span data-stu-id="5ed7e-131">Follow the instructions in [Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and [Configure routing](expressroute-howto-routing-arm.md).</span></span> <span data-ttu-id="5ed7e-132">Az ExpressRoute-kapcsolatcsoportot az alábbi képen hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="5ed7e-132">Your ExpressRoute circuit should look like the following image:</span></span>

    ![Az ExpressRoute-kapcsolatcsoport képernyőképe](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
   
2. <span data-ttu-id="5ed7e-134">Most elindíthatja a kiépítés kapcsolatot létesíthet a virtuális hálózati átjáró, és az ExpressRoute-kapcsolatcsoportot hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-134">You can now start provisioning a connection to link your virtual network gateway to your ExpressRoute circuit.</span></span> <span data-ttu-id="5ed7e-135">Kattintson **kapcsolat** > **hozzáadása** megnyitásához a **kapcsolat hozzáadása a** panelen, majd adja meg az értékeket.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-135">Click **Connection** > **Add** to open the **Add connection** blade, and then configure the values.</span></span>

    ![Vegye fel a kapcsolatot képernyőképe](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)  

3. <span data-ttu-id="5ed7e-137">Után a kapcsolat sikeresen konfigurálva, akkor a kapcsolati objektum a kapcsolat az információk jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-137">After your connection has been successfully configured, your connection object will show the information for the connection.</span></span>

     ![Kapcsolat objektum képernyőképe](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

### <a name="to-delete-a-connection"></a><span data-ttu-id="5ed7e-139">Kapcsolat törlése</span><span class="sxs-lookup"><span data-stu-id="5ed7e-139">To delete a connection</span></span>
<span data-ttu-id="5ed7e-140">A kapcsolat kiválasztásával törölheti a **törlése** ikon a kapcsolat panelen.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-140">You can delete a connection by selecting the **Delete** icon on the blade for your connection.</span></span>

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a><span data-ttu-id="5ed7e-141">Egy másik előfizetéshez tartozó virtuális hálózat bevonása egy kapcsolatcsoportba</span><span class="sxs-lookup"><span data-stu-id="5ed7e-141">Connect a virtual network in a different subscription to a circuit</span></span>
<span data-ttu-id="5ed7e-142">Több előfizetés ExpressRoute-kapcsolatcsoportot lehet megosztani.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-142">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="5ed7e-143">Az alábbi ábra egy egyszerű sematikus ExpressRoute-Kapcsolatcsoportok megosztási hogyan működik a több előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-143">The figure below shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

![Az előfizetések közötti kapcsolathoz](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- <span data-ttu-id="5ed7e-145">A nagy felhőben kisebb felhők egyes szervezet különböző részlegei tartozó előfizetések megjelenítésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-145">Each of the smaller clouds within the large cloud is used to represent subscriptions that belong to different departments within an organization.</span></span>
- <span data-ttu-id="5ed7e-146">A szervezeten belüli osztályok mindegyikének saját előfizetés használhatja a szolgáltatások telepítéséhez, de egyetlen ExpressRoute-kapcsolatcsoportot való kapcsolódáshoz a helyszíni hálózaton is megoszthatják.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-146">Each of the departments within the organization can use their own subscription for deploying their services, but they can share a single ExpressRoute circuit to connect back to your on-premises network.</span></span>
- <span data-ttu-id="5ed7e-147">Egy részleghez (ebben a példában: informatikai) az ExpressRoute-kapcsolatcsoport rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-147">A single department (in this example: IT) can own the ExpressRoute circuit.</span></span> <span data-ttu-id="5ed7e-148">A szervezeten belüli más előfizetések használhatja az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-148">Other subscriptions within the organization can use the ExpressRoute circuit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5ed7e-149">A dedikált kör kapcsolat és a sávszélesség költségek az ExpressRoute-kapcsolatcsoport tulajdonosát alkalmazandó.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-149">Connectivity and bandwidth charges for the dedicated circuit will be applied to the ExpressRoute circuit owner.</span></span> <span data-ttu-id="5ed7e-150">Az összes virtuális hálózatot megosztani a azonos sávszélességet.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-150">All virtual networks share the same bandwidth.</span></span>
    > 
    >

### <a name="administration---circuit-owners-and-circuit-users"></a><span data-ttu-id="5ed7e-151">Felügyeleti - áramkör tulajdonosa és a kapcsolatcsoport felhasználók</span><span class="sxs-lookup"><span data-stu-id="5ed7e-151">Administration - circuit owners and circuit users</span></span>

<span data-ttu-id="5ed7e-152">A kapcsolatcsoport tulajdonosát a ExpressRoute-kapcsolatcsoport erőforrás Power jogosultsága.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-152">The 'circuit owner' is an authorized Power User of the ExpressRoute circuit resource.</span></span> <span data-ttu-id="5ed7e-153">A kapcsolatcsoport tulajdonosát hozhat létre, amely a "kör felhasználók" is váltható engedélyek.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-153">The circuit owner can create authorizations that can be redeemed by 'circuit users'.</span></span> <span data-ttu-id="5ed7e-154">Kör felhasználók, amelyek nem tartoznak a ExpressRoute-kapcsolatcsoportot tárolóként ugyanazt az előfizetést virtuális hálózati átjárók tulajdonosai.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-154">Circuit users are owners of virtual network gateways that are not within the same subscription as the ExpressRoute circuit.</span></span> <span data-ttu-id="5ed7e-155">Kör felhasználók is beválthatja engedélyek (virtuális hálózatonként egy engedélyezési).</span><span class="sxs-lookup"><span data-stu-id="5ed7e-155">Circuit users can redeem authorizations (one authorization per virtual network).</span></span>

<span data-ttu-id="5ed7e-156">A kapcsolatcsoport tulajdonosát a rendelkezik módosítja, és bármikor engedélyek visszavonása.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-156">The circuit owner has the power to modify and revoke authorizations at any time.</span></span> <span data-ttu-id="5ed7e-157">Visszavonni egy engedélyezési eredményezi, az összes hivatkozás kapcsolatok törlődnek az előfizetésből, amelyek hozzáférését visszavonták.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-157">Revoking an authorization results in all link connections being deleted from the subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="5ed7e-158">Kapcsolatcsoport-tulajdonos műveletek</span><span class="sxs-lookup"><span data-stu-id="5ed7e-158">Circuit owner operations</span></span>

<span data-ttu-id="5ed7e-159">**Egy kapcsolat hitelesítési létrehozása**</span><span class="sxs-lookup"><span data-stu-id="5ed7e-159">**To create a connection authorization**</span></span>

<span data-ttu-id="5ed7e-160">A kapcsolatcsoport tulajdonosát engedélyezési hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-160">The circuit owner creates an authorization.</span></span> <span data-ttu-id="5ed7e-161">Az eredmény, használhatja a kapcsolatcsoport felhasználó való csatlakozáshoz a virtuális hálózati átjárók az ExpressRoute-kapcsolatcsoport engedélykulcs létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-161">This results in the creation of an authorization key that can be used by a circuit user to connect their virtual network gateways to the ExpressRoute circuit.</span></span> <span data-ttu-id="5ed7e-162">Az engedély csak egy kapcsolat érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-162">An authorization is valid for only one connection.</span></span>

1. <span data-ttu-id="5ed7e-163">Az ExpressRoute paneljén kattintson **engedélyek** és írja be egy **neve** a engedélyezési, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-163">In the ExpressRoute blade, Click **Authorizations** and then type a **name** for the authorization and click **Save**.</span></span>

    ![Engedélyek](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)

2. <span data-ttu-id="5ed7e-165">Ha a konfiguráció mentése másolja a **erőforrás-azonosító** és a **Engedélykulcs**.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-165">Once the configuration is saved, copy the **Resource ID** and the **Authorization Key**.</span></span>

    ![Hitelesítési kulcs](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

<span data-ttu-id="5ed7e-167">**Egy kapcsolat hitelesítési törlése**</span><span class="sxs-lookup"><span data-stu-id="5ed7e-167">**To delete a connection authorization**</span></span>

<span data-ttu-id="5ed7e-168">A kapcsolat kiválasztásával törölheti a **törlése** ikon a kapcsolat panelen.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-168">You can delete a connection by selecting the **Delete** icon on the blade for your connection.</span></span>

### <a name="circuit-user-operations"></a><span data-ttu-id="5ed7e-169">Kör felhasználói műveletek</span><span class="sxs-lookup"><span data-stu-id="5ed7e-169">Circuit user operations</span></span>

<span data-ttu-id="5ed7e-170">A kapcsolatcsoport felhasználónak van szüksége, az erőforrás-azonosító és a kapcsolatcsoport tulajdonosát a engedélykulcs.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-170">The circuit user needs the resource ID and an authorization key from the circuit owner.</span></span> 

<span data-ttu-id="5ed7e-171">**Egy kapcsolat hitelesítési beváltani**</span><span class="sxs-lookup"><span data-stu-id="5ed7e-171">**To redeem a connection authorization**</span></span>

1.  <span data-ttu-id="5ed7e-172">Kattintson a **+ új** gombra.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-172">Click the **+New** button.</span></span>

    ![Új kattintson](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)

2.  <span data-ttu-id="5ed7e-174">Keresse meg **"Kapcsolat"** a piactéren, válassza ki azt, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-174">Search for **"Connection"** in the Marketplace, select it, and click **Create**.</span></span>

    ![Keresse meg a kapcsolat](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)

3.  <span data-ttu-id="5ed7e-176">Győződjön meg arról, hogy a **kapcsolattípus** "ExpressRoute" értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-176">Make sure the **Connection type** is set to "ExpressRoute".</span></span>


4.  <span data-ttu-id="5ed7e-177">Töltse ki az adatait, majd kattintson az **OK** az alapvető beállítások panelen.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-177">Fill in the details, then click **OK** in the Basics blade.</span></span>

    ![Alapvető beállítások panel](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)

5.  <span data-ttu-id="5ed7e-179">A a **beállítások** panelen válassza ki a **virtuális hálózati átjáró** , és ellenőrizze a **engedélyezési beváltani** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-179">In the **Settings** blade, Select the **Virtual network gateway** and check the **Redeem authorization** check box.</span></span>

6.  <span data-ttu-id="5ed7e-180">Adja meg a **engedélykulcs** és a **áramkör URI partnert** és nevezze el a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-180">Enter the **Authorization key** and the **Peer circuit URI** and give the connection a name.</span></span> <span data-ttu-id="5ed7e-181">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-181">Click **OK**.</span></span>

    ![Beállítások panel](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)

7. <span data-ttu-id="5ed7e-183">Tekintse át az adatokat a **összegzés** panel megnyitásához, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-183">Review the information in the **Summary** blade and click **OK**.</span></span>


<span data-ttu-id="5ed7e-184">**Egy kapcsolat hitelesítési felszabadítása**</span><span class="sxs-lookup"><span data-stu-id="5ed7e-184">**To release a connection authorization**</span></span>

<span data-ttu-id="5ed7e-185">Törölni kell a kapcsolat az ExpressRoute-kapcsolatcsoport a virtuális hálózatra hivatkozó engedélyezési is megjelenhetnek.</span><span class="sxs-lookup"><span data-stu-id="5ed7e-185">You can release an authorization by deleting the connection that links the ExpressRoute circuit to the virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5ed7e-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ed7e-186">Next steps</span></span>
<span data-ttu-id="5ed7e-187">További információ az ExpressRoute-tal kapcsolatban: [ExpressRoute – Gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="5ed7e-187">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>
