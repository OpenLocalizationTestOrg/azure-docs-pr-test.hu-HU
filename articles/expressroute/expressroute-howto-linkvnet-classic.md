---
title: "A virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot hivatkozás: PowerShell: klasszikus: Azure |} Microsoft Docs"
description: "Ez a dokumentum áttekintést hogyan toolink virtuális hálózatok hello klasszikus telepítési modell és a PowerShell használatával (Vnetek) tooExpressRoute kapcsolatok."
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
ms.openlocfilehash: 6b8a01dcd4bbb9618ec3dd438cf0107538fb2a7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit-using-powershell-classic"></a><span data-ttu-id="c5e70-103">Csatlakozás a virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot (klasszikus) PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="c5e70-103">Connect a virtual network tooan ExpressRoute circuit using PowerShell (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c5e70-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c5e70-104">Azure portal</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [<span data-ttu-id="c5e70-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c5e70-105">PowerShell</span></span>](expressroute-howto-linkvnet-arm.md)
> * [<span data-ttu-id="c5e70-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c5e70-106">Azure CLI</span></span>](howto-linkvnet-cli.md)
> * [<span data-ttu-id="c5e70-107">Video - Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c5e70-107">Video - Azure portal</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [<span data-ttu-id="c5e70-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="c5e70-108">PowerShell (classic)</span></span>](expressroute-howto-linkvnet-classic.md)
>

<span data-ttu-id="c5e70-109">Ez a cikk segítséget nyújt a virtuális hálózatokon (Vnetek) tooAzure ExpressRoute-Kapcsolatcsoportok hivatkozás hello klasszikus telepítési modell és a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="c5e70-109">This article will help you link virtual networks (VNets) tooAzure ExpressRoute circuits by using hello classic deployment model and PowerShell.</span></span> <span data-ttu-id="c5e70-110">Virtuális hálózatok lehet az ugyanahhoz az előfizetéshez hello vagy egy másik előfizetés része lehet.</span><span class="sxs-lookup"><span data-stu-id="c5e70-110">Virtual networks can either be in hello same subscription or can be part of another subscription.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="c5e70-111">**Tudnivalók az Azure üzembe helyezési modelljeiről**</span><span class="sxs-lookup"><span data-stu-id="c5e70-111">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a><span data-ttu-id="c5e70-112">Konfigurációs előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c5e70-112">Configuration prerequisites</span></span>
1. <span data-ttu-id="c5e70-113">Hello Azure PowerShell-modulok hello legújabb verziójára van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c5e70-113">You need hello latest version of hello Azure PowerShell modules.</span></span> <span data-ttu-id="c5e70-114">Hello legújabb PowerShell-modulok letölthető hello hello PowerShell szakasza [Azure letöltőoldala](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c5e70-114">You can download hello latest PowerShell modules from hello PowerShell section of hello [Azure Downloads page](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="c5e70-115">Hello utasításait követve [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) kapcsolatos részletes útmutatás tooconfigure a számítógép toouse hello Azure PowerShell-modulok.</span><span class="sxs-lookup"><span data-stu-id="c5e70-115">Follow hello instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for step-by-step guidance on how tooconfigure your computer toouse hello Azure PowerShell modules.</span></span>
2. <span data-ttu-id="c5e70-116">Tooreview hello kell [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="c5e70-116">You need tooreview hello [prerequisites](expressroute-prerequisites.md), [routing requirements](expressroute-routing.md), and [workflows](expressroute-workflows.md) before you begin configuration.</span></span>
3. <span data-ttu-id="c5e70-117">Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="c5e70-117">You must have an active ExpressRoute circuit.</span></span>
   * <span data-ttu-id="c5e70-118">Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-classic.md) , és a kapcsolat szolgáltatójánál hello áramkör engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c5e70-118">Follow hello instructions too[create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have your connectivity provider enable hello circuit.</span></span>
   * <span data-ttu-id="c5e70-119">Győződjön meg arról, hogy rendelkezik az Azure magánhálózati társviszony-létesítés a kapcsolatcsoport konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="c5e70-119">Ensure that you have Azure private peering configured for your circuit.</span></span> <span data-ttu-id="c5e70-120">Lásd: hello [útválasztás konfigurálása](expressroute-howto-routing-classic.md) útválasztási miként.</span><span class="sxs-lookup"><span data-stu-id="c5e70-120">See hello [Configure routing](expressroute-howto-routing-classic.md) article for routing instructions.</span></span>
   * <span data-ttu-id="c5e70-121">Győződjön meg arról, hogy az Azure magánhálózati társviszony-létesítés van konfigurálva, és hello BGP társviszony-létesítés a hálózat és a Microsoft között működik-e, hogy engedélyezheti a végpontok közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="c5e70-121">Ensure that Azure private peering is configured and hello BGP peering between your network and Microsoft is up so that you can enable end-to-end connectivity.</span></span>
   * <span data-ttu-id="c5e70-122">Rendelkeznie kell egy virtuális hálózat és a virtuális hálózati átjáró létrehozása, és teljesen kiépítve.</span><span class="sxs-lookup"><span data-stu-id="c5e70-122">You must have a virtual network and a virtual network gateway created and fully provisioned.</span></span> <span data-ttu-id="c5e70-123">Útmutatás alapján hello túl[virtuális hálózat konfigurálása ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span><span class="sxs-lookup"><span data-stu-id="c5e70-123">Follow hello instructions too[configure a virtual network for ExpressRoute](expressroute-howto-vnet-portal-classic.md).</span></span>

<span data-ttu-id="c5e70-124">Too10 virtuális hálózatok tooan ExpressRoute-kapcsolatcsoportot is csatolhatja.</span><span class="sxs-lookup"><span data-stu-id="c5e70-124">You can link up too10 virtual networks tooan ExpressRoute circuit.</span></span> <span data-ttu-id="c5e70-125">Az összes virtuális hálózatot kell hello geopolitikai ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="c5e70-125">All virtual networks must be in hello same geopolitical region.</span></span> <span data-ttu-id="c5e70-126">Virtuális hálózatok tooyour ExpressRoute-kapcsolatcsoportot nagyobb számú hivatkozásra, vagy virtuális hálózatok, amelyek más geopolitikai régiókban, ha engedélyezte a hello ExpressRoute prémium szintű bővítmény hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="c5e70-126">You can link a larger number of virtual networks tooyour ExpressRoute circuit, or link virtual networks that are in other geopolitical regions if you enabled hello ExpressRoute premium add-on.</span></span> <span data-ttu-id="c5e70-127">Ellenőrizze a hello [– gyakori kérdések](expressroute-faqs.md) hello prémium szintű bővítmény olvashat.</span><span class="sxs-lookup"><span data-stu-id="c5e70-127">Check hello [FAQ](expressroute-faqs.md) for more details on hello premium add-on.</span></span>

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a><span data-ttu-id="c5e70-128">A virtuális hálózat a hello azonos előfizetés tooa áramkör</span><span class="sxs-lookup"><span data-stu-id="c5e70-128">Connect a virtual network in hello same subscription tooa circuit</span></span>
<span data-ttu-id="c5e70-129">A virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot hozzákapcsolhatja hello a következő parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="c5e70-129">You can link a virtual network tooan ExpressRoute circuit by using hello following cmdlet.</span></span> <span data-ttu-id="c5e70-130">Győződjön meg arról, hogy hello virtuális hálózati átjáró jön létre, és készen áll a linking hello parancsmag futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="c5e70-130">Make sure that hello virtual network gateway is created and is ready for linking before you run hello cmdlet.</span></span>

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a><span data-ttu-id="c5e70-131">Egy másik előfizetésben található tooa kapcsolat a virtuális hálózatot</span><span class="sxs-lookup"><span data-stu-id="c5e70-131">Connect a virtual network in a different subscription tooa circuit</span></span>
<span data-ttu-id="c5e70-132">Több előfizetés ExpressRoute-kapcsolatcsoportot lehet megosztani.</span><span class="sxs-lookup"><span data-stu-id="c5e70-132">You can share an ExpressRoute circuit across multiple subscriptions.</span></span> <span data-ttu-id="c5e70-133">a következő ábra hello mutatja egy egyszerű sematikus ExpressRoute-Kapcsolatcsoportok megosztási hogyan működik a több előfizetésekhez.</span><span class="sxs-lookup"><span data-stu-id="c5e70-133">hello following figure shows a simple schematic of how sharing works for ExpressRoute circuits across multiple subscriptions.</span></span>

<span data-ttu-id="c5e70-134">Hello kisebb felhők hello nagy felhőben egy szervezeten belül toodifferent részlegek tartozó használt toorepresent előfizetések.</span><span class="sxs-lookup"><span data-stu-id="c5e70-134">Each of hello smaller clouds within hello large cloud is used toorepresent subscriptions that belong toodifferent departments within an organization.</span></span> <span data-ttu-id="c5e70-135">A szolgáltatások – de hello részlegek telepítése megoszthat egy ExpressRoute körön tooconnect hátsó tooyour a helyszíni hálózaton hello szervezeten belül hello osztályok mindegyikének alkalmazhatja a saját előfizetés.</span><span class="sxs-lookup"><span data-stu-id="c5e70-135">Each of hello departments within hello organization can use their own subscription for deploying their services--but hello departments can share a single ExpressRoute circuit tooconnect back tooyour on-premises network.</span></span> <span data-ttu-id="c5e70-136">Egy részleghez (ebben a példában: informatikai) is saját hello ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="c5e70-136">A single department (in this example: IT) can own hello ExpressRoute circuit.</span></span> <span data-ttu-id="c5e70-137">Hello szervezeten belüli más előfizetésekkel használható hello ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="c5e70-137">Other subscriptions within hello organization can use hello ExpressRoute circuit.</span></span>

> [!NOTE]
> <span data-ttu-id="c5e70-138">Kapcsolat és a sávszélesség díjak dedikált hello kör lesz alkalmazott toohello ExpressRoute-kapcsolatcsoport tulajdonosát.</span><span class="sxs-lookup"><span data-stu-id="c5e70-138">Connectivity and bandwidth charges for hello dedicated circuit will be applied toohello ExpressRoute circuit owner.</span></span> <span data-ttu-id="c5e70-139">Az összes virtuális hálózatot megosztása hello azonos sávszélesség.</span><span class="sxs-lookup"><span data-stu-id="c5e70-139">All virtual networks share hello same bandwidth.</span></span>
> 
> 

![Az előfizetések közötti kapcsolathoz](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a><span data-ttu-id="c5e70-141">Adminisztráció</span><span class="sxs-lookup"><span data-stu-id="c5e70-141">Administration</span></span>
<span data-ttu-id="c5e70-142">Hello *kapcsolatcsoport tulajdonosát* hello rendszergazda/coadministrator, mely hello ExpressRoute körön létrejött hello előfizetés van.</span><span class="sxs-lookup"><span data-stu-id="c5e70-142">hello *circuit owner* is hello administrator/coadministrator of hello subscription in which hello ExpressRoute circuit is created.</span></span> <span data-ttu-id="c5e70-143">hello kapcsolatcsoport tulajdonosát engedélyezhető a rendszergazdák/coadministrators más előfizetések hivatkozott tooas *felhasználók áramkör*, toouse hello dedikált saját körön.</span><span class="sxs-lookup"><span data-stu-id="c5e70-143">hello circuit owner can authorize administrators/coadministrators of other subscriptions, referred tooas *circuit users*, toouse hello dedicated circuit that they own.</span></span> <span data-ttu-id="c5e70-144">Kör felhasználók számára engedélyezett toouse hello szervezet ExpressRoute körön is hello virtuális hálózatot az előfizetés toohello ExpressRoute-kapcsolatcsoportot a hivatkozásra, után jogosultak.</span><span class="sxs-lookup"><span data-stu-id="c5e70-144">Circuit users who are authorized toouse hello organization's ExpressRoute circuit can link hello virtual network in their subscription toohello ExpressRoute circuit after they are authorized.</span></span>

<span data-ttu-id="c5e70-145">hello kapcsolatcsoport tulajdonosát hello power toomodify és visszavonni az engedélyeket bármikor rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c5e70-145">hello circuit owner has hello power toomodify and revoke authorizations at any time.</span></span> <span data-ttu-id="c5e70-146">Az engedélyt visszavonni törlődnek, amelyek hozzáférését visszavonták hello előfizetésből összes hivatkozást eredményez.</span><span class="sxs-lookup"><span data-stu-id="c5e70-146">Revoking an authorization will result in all links being deleted from hello subscription whose access was revoked.</span></span>

### <a name="circuit-owner-operations"></a><span data-ttu-id="c5e70-147">Kapcsolatcsoport-tulajdonos műveletek</span><span class="sxs-lookup"><span data-stu-id="c5e70-147">Circuit owner operations</span></span>

<span data-ttu-id="c5e70-148">**Az engedély létrehozása**</span><span class="sxs-lookup"><span data-stu-id="c5e70-148">**Creating an authorization**</span></span>

<span data-ttu-id="c5e70-149">hello kapcsolatcsoport tulajdonosát engedélyezi hello rendszergazdák más előfizetések toouse hello megadva kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="c5e70-149">hello circuit owner authorizes hello administrators of other subscriptions toouse hello specified circuit.</span></span> <span data-ttu-id="c5e70-150">Hello rendszergazda hello áramkör (a Contoso informatikai) a következő példa hello, lehetővé teszi a tootwo virtuális hálózatok toohello áramkör be egy másik előfizetés (fejlesztői, tesztelési) toolink hello rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="c5e70-150">In hello following example, hello administrator of hello circuit (Contoso IT) enables hello administrator of another subscription (Dev-Test) toolink up tootwo virtual networks toohello circuit.</span></span> <span data-ttu-id="c5e70-151">hello Contoso rendszergazda lehetővé teszi, hogy ez megadásával hello fejlesztői-teszt Microsoft azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="c5e70-151">hello Contoso IT administrator enables this by specifying hello Dev-Test Microsoft ID.</span></span> <span data-ttu-id="c5e70-152">hello parancsmag nem küld e-mailek toohello megadott Microsoft-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="c5e70-152">hello cmdlet doesn't send email toohello specified Microsoft ID.</span></span> <span data-ttu-id="c5e70-153">hello kapcsolatcsoport tulajdonosát kell tooexplicitly értesítés hello más előfizetés tulajdonosa, amely engedélyezési hello befejeződött.</span><span class="sxs-lookup"><span data-stu-id="c5e70-153">hello circuit owner needs tooexplicitly notify hello other subscription owner that hello authorization is complete.</span></span>

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0

<span data-ttu-id="c5e70-154">**Engedélyek ellenőrzése**</span><span class="sxs-lookup"><span data-stu-id="c5e70-154">**Reviewing authorizations**</span></span>

<span data-ttu-id="c5e70-155">hello kapcsolatcsoport tulajdonosát hello a következő parancsmag futtatásával egy adott körön kiállított összes engedélyek tekinthetők át:</span><span class="sxs-lookup"><span data-stu-id="c5e70-155">hello circuit owner can review all authorizations that are issued on a particular circuit by running hello following cmdlet:</span></span>

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


<span data-ttu-id="c5e70-156">**Engedélyek frissítése**</span><span class="sxs-lookup"><span data-stu-id="c5e70-156">**Updating authorizations**</span></span>

<span data-ttu-id="c5e70-157">hello kapcsolatcsoport tulajdonosát hello a következő parancsmag használatával módosíthatja az engedélyeket:</span><span class="sxs-lookup"><span data-stu-id="c5e70-157">hello circuit owner can modify authorizations by using hello following cmdlet:</span></span>

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


<span data-ttu-id="c5e70-158">**Engedélyek törlése**</span><span class="sxs-lookup"><span data-stu-id="c5e70-158">**Deleting authorizations**</span></span>

<span data-ttu-id="c5e70-159">hello kapcsolatcsoport tulajdonosát képes visszavonni vagy törlése engedélyek toohello felhasználói hello a következő parancsmag futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c5e70-159">hello circuit owner can revoke/delete authorizations toohello user by running hello following cmdlet:</span></span>

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### <a name="circuit-user-operations"></a><span data-ttu-id="c5e70-160">Kör felhasználói műveletek</span><span class="sxs-lookup"><span data-stu-id="c5e70-160">Circuit user operations</span></span>

<span data-ttu-id="c5e70-161">**Engedélyek ellenőrzése**</span><span class="sxs-lookup"><span data-stu-id="c5e70-161">**Reviewing authorizations**</span></span>

<span data-ttu-id="c5e70-162">hello áramkör felhasználói engedélyek hello a következő parancsmag használatával tekintse át:</span><span class="sxs-lookup"><span data-stu-id="c5e70-162">hello circuit user can review authorizations by using hello following cmdlet:</span></span>

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

<span data-ttu-id="c5e70-163">**Hivatkozás engedélyek váltja be**</span><span class="sxs-lookup"><span data-stu-id="c5e70-163">**Redeeming link authorizations**</span></span>

<span data-ttu-id="c5e70-164">hello áramkör felhasználó futtathatja a következő parancsmag tooredeem hello egy hivatkozás hitelesítési:</span><span class="sxs-lookup"><span data-stu-id="c5e70-164">hello circuit user can run hello following cmdlet tooredeem a link authorization:</span></span>

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

<span data-ttu-id="c5e70-165">Futtassa ezt a parancsot az újonnan csatolt hello előfizetésben hello virtuális hálózathoz:</span><span class="sxs-lookup"><span data-stu-id="c5e70-165">Run this command in hello newly linked subscription for hello virtual network:</span></span>

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"

## <a name="next-steps"></a><span data-ttu-id="c5e70-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c5e70-166">Next steps</span></span>
<span data-ttu-id="c5e70-167">ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="c5e70-167">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

