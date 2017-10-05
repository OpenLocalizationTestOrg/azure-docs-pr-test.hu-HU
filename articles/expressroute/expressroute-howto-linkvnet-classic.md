---
title: "Virtuális hálózat csatolása ExpressRoute-kapcsolatcsoportot: PowerShell: klasszikus: Azure |} Microsoft Docs"
description: "Ez a dokumentum áttekintést csatolása a virtuális hálózatokon (Vnetek) az ExpressRoute-Kapcsolatcsoportok a klasszikus telepítési modell és a PowerShell használatával."
services: expressroute
documentationcenter: na
author: ganesr
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 9b53fd72-9b6b-4844-80b9-4e1d54fd0c17
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2017
ms.author: ganesr
ms.openlocfilehash: 8df8a4c6ff0897c821e13248e0494b17e1a4992d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-powershell-classic"></a><span data-ttu-id="5682b-103">Egy virtuális hálózathoz csatlakozni powershellel (klasszikus) ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="5682b-103">Connect a virtual network to an ExpressRoute circuit using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5682b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5682b-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="5682b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5682b-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="5682b-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5682b-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="5682b-107">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5682b-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="5682b-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="5682b-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
>

<span data-ttu-id="5682b-109">Ez a cikk segítséget nyújt a virtuális hálózatokon (Vnetek) csatolása az Azure ExpressRoute-Kapcsolatcsoportok a klasszikus telepítési modell és a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="5682b-109">This article will help you link virtual networks (VNets) to Azure ExpressRoute circuits by using the classic deployment model and PowerShell.</span></span> <span data-ttu-id="5682b-110">Virtuális hálózatok ugyanahhoz az előfizetéshez lehet, vagy egy másik előfizetés része lehet.</span><span class="sxs-lookup"><span data-stu-id="5682b-110">Virtual networks can either be in the same subscription or can be part of another subscription.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="5682b-111">**Tudnivalók az Azure üzembehelyezési modellekről**</span><span class="sxs-lookup"><span data-stu-id="5682b-111">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a><span data-ttu-id="5682b-112">Konfigurációs előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5682b-112">Configuration prerequisites</span></span>
1. <span data-ttu-id="5682b-113">Az Azure PowerShell-modulok legújabb verziójára van szüksége.</span><span class="sxs-lookup"><span data-stu-id="5682b-113">You need the latest version of the Azure PowerShell modules.</span></span> <span data-ttu-id="5682b-114">Letöltheti a legújabb PowerShell-modulok PowerShell szakaszában a [Azure letöltőoldala](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="5682b-114">You can download the latest PowerShell modules from the PowerShell section of the [Azure Downloads page](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="5682b-115">Kövesse az utasításokat a [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview) kapcsolatos részletes útmutatás az Azure PowerShell-modulok használata a számítógép konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="5682b-115">Follow the instructions in [How to install and configure Azure PowerShell](/powershell/azure/overview) for step-by-step guidance on how to configure your computer to use the Azure PowerShell modules.</span></span>
2. <span data-ttu-id="5682b-116">Át kell tekintenie a [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="5682b-116">You need to review the [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
3. <span data-ttu-id="5682b-117">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="5682b-117">You must have an active ExpressRoute circuit.</span></span>
   * <span data-ttu-id="5682b-118">Kövesse az utasításokat [ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-classic.md) és a kapcsolat szolgáltatójánál engedélyezése a kapcsolatcsoport rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="5682b-118">Follow the instructions to [create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have your connectivity provider enable the circuit.</span></span>
   * <span data-ttu-id="5682b-119">Győződjön meg arról, hogy rendelkezik az Azure magánhálózati társviszony-létesítés a kapcsolatcsoport konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="5682b-119">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="5682b-120">Tekintse meg a [útválasztás konfigurálása](expressroute-howto-routing-classic.md) útválasztási miként.</span><span class="sxs-lookup"><span data-stu-id="5682b-120">See the [Configure routing](expressroute-howto-routing-classic.md) article for routing instructions.</span></span>
   * <span data-ttu-id="5682b-121">Győződjön meg arról, hogy az Azure magánhálózati társviszony-létesítés konfigurálva legyen, és a BGP társviszony-létesítés a hálózat és a Microsoft között működik-e, hogy engedélyezheti a végpontok közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="5682b-121">Ensure that Azure private peering is configured and the BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
   * <span data-ttu-id="5682b-122">Rendelkeznie kell egy virtuális hálózat és a virtuális hálózati átjáró létrehozása, és teljesen kiépítve.</span><span class="sxs-lookup"><span data-stu-id="5682b-122">You must have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="5682b-123">Kövesse az utasításokat [virtuális hálózat konfigurálása ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span><span class="sxs-lookup"><span data-stu-id="5682b-123">Follow the instructions to [configure a virtual network for ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span></span>

<span data-ttu-id="5682b-124">Legfeljebb 10 virtuális hálózatok ExpressRoute-kapcsolatcsoportot társíthatja.</span><span class="sxs-lookup"><span data-stu-id="5682b-124">You can link up to 10 virtual networks to an ExpressRoute circuit.</span></span> <span data-ttu-id="5682b-125">Az összes virtuális hálózatot geopolitikai ugyanabban a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5682b-125">All virtual networks must be in the same geopolitical region.</span></span> <span data-ttu-id="5682b-126">Az ExpressRoute-kapcsolatcsoportot virtuális hálózatok, vagy hivatkozás virtuális hálózatok, amelyek más geopolitikai régiókban, ha engedélyezte a prémium szintű ExpressRoute-bővítmény nagyobb számú társíthatja.</span><span class="sxs-lookup"><span data-stu-id="5682b-126">You can link a larger number of virtual networks to your ExpressRoute circuit, or link virtual networks that are in other geopolitical regions if you enabled the ExpressRoute premium add-on.</span></span> <span data-ttu-id="5682b-127">Ellenőrizze a [gyakran ismételt kérdések](expressroute-faqs.md) a prémium szintű bővítmény olvashat.</span><span class="sxs-lookup"><span data-stu-id="5682b-127">Check the [FAQ](expressroute-faqs.md) for more details on the premium add-on.</span></span>

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a><span data-ttu-id="5682b-128">A virtuális hálózati ugyanahhoz az előfizetéshez csatlakozni expressroute-kapcsolatcsoporthoz</span><span class="sxs-lookup"><span data-stu-id="5682b-128">Connect a virtual network in the same subscription to a circuit</span></span>
<span data-ttu-id="5682b-129">A következő parancsmag használatával egy virtuális hálózatot is kapcsolhat ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="5682b-129">You can link a virtual network to an ExpressRoute circuit by using the following cmdlet.</span></span> <span data-ttu-id="5682b-130">Győződjön meg arról, hogy a virtuális hálózati átjáró jön létre, és készen áll a csatolás a parancsmag futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="5682b-130">Make sure that the virtual network gateway is created and is ready for linking before you run the cmdlet.</span></span>

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a><span data-ttu-id="5682b-131">Egy másik előfizetéshez tartozó virtuális hálózat bevonása egy kapcsolatcsoportba</span><span class="sxs-lookup"><span data-stu-id="5682b-131">Connect a virtual network in a different subscription to a circuit</span></span>
<span data-ttu-id="5682b-132">Több előfizetés ExpressRoute-kapcsolatcsoportot lehet megosztani.</span><span class="sxs-lookup"><span data-stu-id="5682b-132">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="5682b-133">Az alábbi ábrán egy egyszerű sematikus ExpressRoute-Kapcsolatcsoportok megosztási hogyan működik a több előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="5682b-133">The following figure shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="5682b-134">A nagy felhőben kisebb felhők egyes szervezet különböző részlegei tartozó előfizetések megjelenítésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="5682b-134">Each of the smaller clouds within the large cloud is used to represent subscriptions that belong to different departments within an organization.</span></span> <span data-ttu-id="5682b-135">Egyetlen ExpressRoute-kapcsolatcsoportot való kapcsolódáshoz a helyszíni hálózat üzembe helyezése a szolgáltatásból, de a részlegek megoszthatja a szervezeten belüli osztályok mindegyikének alkalmazhatja a saját előfizetés.</span><span class="sxs-lookup"><span data-stu-id="5682b-135">Each of the departments within the organization can use their own subscription for deploying their services--but the departments can share a single ExpressRoute circuit to connect back to your on-premises network.</span></span> <span data-ttu-id="5682b-136">Egy részleghez (ebben a példában: informatikai) az ExpressRoute-kapcsolatcsoport rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="5682b-136">A single department (in this example: IT) can own the ExpressRoute circuit.</span></span> <span data-ttu-id="5682b-137">A szervezeten belüli más előfizetések használhatja az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="5682b-137">Other subscriptions within the organization can use the ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="5682b-138">A dedikált kör kapcsolat és a sávszélesség költségek az ExpressRoute-kapcsolatcsoport tulajdonosát alkalmazandó.</span><span class="sxs-lookup"><span data-stu-id="5682b-138">Connectivity and bandwidth charges for the dedicated circuit will be applied to the ExpressRoute circuit owner.</span></span> <span data-ttu-id="5682b-139">Az összes virtuális hálózatot megosztani a azonos sávszélességet.</span><span class="sxs-lookup"><span data-stu-id="5682b-139">All virtual networks share the same bandwidth.</span></span>
> 
> 

![Az előfizetések közötti kapcsolathoz](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a><span data-ttu-id="5682b-141">Adminisztráció</span><span class="sxs-lookup"><span data-stu-id="5682b-141">Administration</span></span>
<span data-ttu-id="5682b-142">A *kapcsolatcsoport tulajdonosát* az előfizetés, amelyben az ExpressRoute-kapcsolatcsoport jön létre, rendszergazdai/coadministrator van.</span><span class="sxs-lookup"><span data-stu-id="5682b-142">The *circuit owner* is the administrator/coadministrator of the subscription in which the ExpressRoute circuit is created.</span></span> <span data-ttu-id="5682b-143">A kapcsolatcsoport tulajdonosát adhatják meg a rendszergazdák/coadministrators más előfizetések néven *felhasználók áramkör*, akkor használja a dedikált áramkör saját.</span><span class="sxs-lookup"><span data-stu-id="5682b-143">The circuit owner can authorize administrators/coadministrators of other subscriptions, referred to as *circuit users*, to use the dedicated circuit that they own.</span></span> <span data-ttu-id="5682b-144">Kör felhasználók, akik jogosultak a szervezet ExpressRoute-kapcsolatcsoportot használatára után jogosultak a virtuális hálózatot az előfizetését a társíthatja az ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="5682b-144">Circuit users who are authorized to use the organization's ExpressRoute circuit can link the virtual network in their subscription to the ExpressRoute circuit after they are authorized.</span></span>

<span data-ttu-id="5682b-145">A kapcsolatcsoport tulajdonosát a rendelkezik módosítja, és bármikor engedélyek visszavonása.</span><span class="sxs-lookup"><span data-stu-id="5682b-145">The circuit owner has the power to modify and revoke authorizations at any time.</span></span> <span data-ttu-id="5682b-146">Az engedélyt visszavonni azt eredményezi, hogy törli az előfizetést, amelyek hozzáférését visszavonták az összes hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="5682b-146">Revoking an authorization will result in all links being deleted from the subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="5682b-147">Kapcsolatcsoport-tulajdonos műveletek</span><span class="sxs-lookup"><span data-stu-id="5682b-147">Circuit owner operations</span></span>

<span data-ttu-id="5682b-148">**Az engedély létrehozása**</span><span class="sxs-lookup"><span data-stu-id="5682b-148">**Creating an authorization**</span></span>

<span data-ttu-id="5682b-149">A kapcsolatcsoport tulajdonosát a megadott expressroute-kapcsolatcsoport használandó másik előfizetés rendszergazdája engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="5682b-149">The circuit owner authorizes the administrators of other subscriptions to use the specified circuit.</span></span> <span data-ttu-id="5682b-150">A következő példában a rendszergazda a kapcsolat (a Contoso informatikai) lehetővé teszi, hogy a rendszergazda egy másik előfizetés (fejlesztői-teszt) legfeljebb két virtuális hálózatok összekapcsolása a kapcsolatcsoport.</span><span class="sxs-lookup"><span data-stu-id="5682b-150">In the following example, the administrator of the circuit (Contoso IT) enables the administrator of another subscription (Dev-Test) to link up to two virtual networks to the circuit.</span></span> <span data-ttu-id="5682b-151">A Contoso informatikai rendszergazda lehetővé teszi, hogy ez a fejlesztés-tesztelés Microsoft azonosító megadásával</span><span class="sxs-lookup"><span data-stu-id="5682b-151">The Contoso IT administrator enables this by specifying the Dev-Test Microsoft ID.</span></span> <span data-ttu-id="5682b-152">A parancsmag nem e-mailt küld a megadott Microsoft.</span><span class="sxs-lookup"><span data-stu-id="5682b-152">The cmdlet doesn't send email to the specified Microsoft ID.</span></span> <span data-ttu-id="5682b-153">A kapcsolatcsoport tulajdonosát kell, hogy helyesek-e az engedélyezési explicit módon értesíteni az előfizetés tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="5682b-153">The circuit owner needs to explicitly notify the other subscription owner that the authorization is complete.</span></span>

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0

<span data-ttu-id="5682b-154">**Engedélyek ellenőrzése**</span><span class="sxs-lookup"><span data-stu-id="5682b-154">**Reviewing authorizations**</span></span>

<span data-ttu-id="5682b-155">A kapcsolatcsoport tulajdonosát tekintse át a következő parancsmag futtatásával egy adott körön kiállított összes engedélyek:</span><span class="sxs-lookup"><span data-stu-id="5682b-155">The circuit owner can review all authorizations that are issued on a particular circuit by running the following cmdlet:</span></span>

    Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"

    Description         : EngineeringTeam
    Limit               : 3
    LinkAuthorizationId : ####################################
    MicrosoftIds        : engadmin@contoso.com
    Used                : 1

    Description         : MarketingTeam
    Limit               : 1
    LinkAuthorizationId : @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    MicrosoftIds        : marketingadmin@contoso.com
    Used                : 0

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : salesadmin@contoso.com
    Used                : 2


<span data-ttu-id="5682b-156">**Engedélyek frissítése**</span><span class="sxs-lookup"><span data-stu-id="5682b-156">**Updating authorizations**</span></span>

<span data-ttu-id="5682b-157">A kapcsolatcsoport tulajdonosát a következő parancsmag használatával módosíthatja az engedélyeket:</span><span class="sxs-lookup"><span data-stu-id="5682b-157">The circuit owner can modify authorizations by using the following cmdlet:</span></span>

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


<span data-ttu-id="5682b-158">**Engedélyek törlése**</span><span class="sxs-lookup"><span data-stu-id="5682b-158">**Deleting authorizations**</span></span>

<span data-ttu-id="5682b-159">A kapcsolatcsoport tulajdonosát is revoke vagy törlése a felhasználó engedélyek a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="5682b-159">The circuit owner can revoke/delete authorizations to the user by running the following cmdlet:</span></span>

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### <a name="circuit-user-operations"></a><span data-ttu-id="5682b-160">Kör felhasználói műveletek</span><span class="sxs-lookup"><span data-stu-id="5682b-160">Circuit user operations</span></span>

<span data-ttu-id="5682b-161">**Engedélyek ellenőrzése**</span><span class="sxs-lookup"><span data-stu-id="5682b-161">**Reviewing authorizations**</span></span>

<span data-ttu-id="5682b-162">A kapcsolatcsoport felhasználói engedélyek tekintse át a következő parancsmag használatával:</span><span class="sxs-lookup"><span data-stu-id="5682b-162">The circuit user can review authorizations by using the following cmdlet:</span></span>

    Get-AzureAuthorizedDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : ContosoIT
    Location                         : Washington DC
    MaximumAllowedLinks              : 2
    ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled
    UsedLinks                        : 0

<span data-ttu-id="5682b-163">**Hivatkozás engedélyek váltja be**</span><span class="sxs-lookup"><span data-stu-id="5682b-163">**Redeeming link authorizations**</span></span>

<span data-ttu-id="5682b-164">A kapcsolatcsoport felhasználó futtathatja egy hivatkozás hitelesítési beváltani a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="5682b-164">The circuit user can run the following cmdlet to redeem a link authorization:</span></span>

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

<span data-ttu-id="5682b-165">Futtassa ezt a parancsot az újonnan csatolt előfizetésben található virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="5682b-165">Run this command in the newly linked subscription for the virtual network:</span></span>

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"

## <a name="next-steps"></a><span data-ttu-id="5682b-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5682b-166">Next steps</span></span>
<span data-ttu-id="5682b-167">További információ az ExpressRoute-tal kapcsolatban: [ExpressRoute – Gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="5682b-167">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

