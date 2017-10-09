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
# <a name="connect-a-virtual-network-tooan-expressroute-circuit-using-powershell-classic"></a>Csatlakozás a virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot (klasszikus) PowerShell használatával
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure-portálon](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasszikus)](expressroute-howto-linkvnet-classic.md)
>

Ez a cikk segítséget nyújt a virtuális hálózatokon (Vnetek) tooAzure ExpressRoute-Kapcsolatcsoportok hivatkozás hello klasszikus telepítési modell és a PowerShell használatával. Virtuális hálózatok lehet az ugyanahhoz az előfizetéshez hello vagy egy másik előfizetés része lehet.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Tudnivalók az Azure üzembe helyezési modelljeiről**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Konfigurációs előfeltételek
1. Hello Azure PowerShell-modulok hello legújabb verziójára van szüksége. Hello legújabb PowerShell-modulok letölthető hello hello PowerShell szakasza [Azure letöltőoldala](https://azure.microsoft.com/downloads/). Hello utasításait követve [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) kapcsolatos részletes útmutatás tooconfigure a számítógép toouse hello Azure PowerShell-modulok.
2. Tooreview hello kell [Előfeltételek](expressroute-prerequisites.md), [útválasztási követelmények](expressroute-routing.md), és [munkafolyamatok](expressroute-workflows.md) konfigurálás elkezdése előtt.
3. Egy aktív ExpressRoute-kapcsolatcsoportra lesz szüksége.
   * Útmutatás alapján hello túl[ExpressRoute-kapcsolatcsoportot létrehozni](expressroute-howto-circuit-classic.md) , és a kapcsolat szolgáltatójánál hello áramkör engedélyezése.
   * Győződjön meg arról, hogy rendelkezik az Azure magánhálózati társviszony-létesítés a kapcsolatcsoport konfigurálva. Lásd: hello [útválasztás konfigurálása](expressroute-howto-routing-classic.md) útválasztási miként.
   * Győződjön meg arról, hogy az Azure magánhálózati társviszony-létesítés van konfigurálva, és hello BGP társviszony-létesítés a hálózat és a Microsoft között működik-e, hogy engedélyezheti a végpontok közötti kapcsolat.
   * Rendelkeznie kell egy virtuális hálózat és a virtuális hálózati átjáró létrehozása, és teljesen kiépítve. Útmutatás alapján hello túl[virtuális hálózat konfigurálása ExpressRoute](expressroute-howto-vnet-portal-classic.md).

Too10 virtuális hálózatok tooan ExpressRoute-kapcsolatcsoportot is csatolhatja. Az összes virtuális hálózatot kell hello geopolitikai ugyanabban a régióban. Virtuális hálózatok tooyour ExpressRoute-kapcsolatcsoportot nagyobb számú hivatkozásra, vagy virtuális hálózatok, amelyek más geopolitikai régiókban, ha engedélyezte a hello ExpressRoute prémium szintű bővítmény hivatkozásra. Ellenőrizze a hello [– gyakori kérdések](expressroute-faqs.md) hello prémium szintű bővítmény olvashat.

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a>A virtuális hálózat a hello azonos előfizetés tooa áramkör
A virtuális hálózati tooan ExpressRoute-kapcsolatcsoportot hozzákapcsolhatja hello a következő parancsmag használatával. Győződjön meg arról, hogy hello virtuális hálózati átjáró jön létre, és készen áll a linking hello parancsmag futtatása előtt.

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a>Egy másik előfizetésben található tooa kapcsolat a virtuális hálózatot
Több előfizetés ExpressRoute-kapcsolatcsoportot lehet megosztani. a következő ábra hello mutatja egy egyszerű sematikus ExpressRoute-Kapcsolatcsoportok megosztási hogyan működik a több előfizetésekhez.

Hello kisebb felhők hello nagy felhőben egy szervezeten belül toodifferent részlegek tartozó használt toorepresent előfizetések. A szolgáltatások – de hello részlegek telepítése megoszthat egy ExpressRoute körön tooconnect hátsó tooyour a helyszíni hálózaton hello szervezeten belül hello osztályok mindegyikének alkalmazhatja a saját előfizetés. Egy részleghez (ebben a példában: informatikai) is saját hello ExpressRoute-kapcsolatcsoportot. Hello szervezeten belüli más előfizetésekkel használható hello ExpressRoute-kapcsolatcsoportot.

> [!NOTE]
> Kapcsolat és a sávszélesség díjak dedikált hello kör lesz alkalmazott toohello ExpressRoute-kapcsolatcsoport tulajdonosát. Az összes virtuális hálózatot megosztása hello azonos sávszélesség.
> 
> 

![Az előfizetések közötti kapcsolathoz](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Adminisztráció
Hello *kapcsolatcsoport tulajdonosát* hello rendszergazda/coadministrator, mely hello ExpressRoute körön létrejött hello előfizetés van. hello kapcsolatcsoport tulajdonosát engedélyezhető a rendszergazdák/coadministrators más előfizetések hivatkozott tooas *felhasználók áramkör*, toouse hello dedikált saját körön. Kör felhasználók számára engedélyezett toouse hello szervezet ExpressRoute körön is hello virtuális hálózatot az előfizetés toohello ExpressRoute-kapcsolatcsoportot a hivatkozásra, után jogosultak.

hello kapcsolatcsoport tulajdonosát hello power toomodify és visszavonni az engedélyeket bármikor rendelkezik. Az engedélyt visszavonni törlődnek, amelyek hozzáférését visszavonták hello előfizetésből összes hivatkozást eredményez.

### <a name="circuit-owner-operations"></a>Kapcsolatcsoport-tulajdonos műveletek

**Az engedély létrehozása**

hello kapcsolatcsoport tulajdonosát engedélyezi hello rendszergazdák más előfizetések toouse hello megadva kapcsolat. Hello rendszergazda hello áramkör (a Contoso informatikai) a következő példa hello, lehetővé teszi a tootwo virtuális hálózatok toohello áramkör be egy másik előfizetés (fejlesztői, tesztelési) toolink hello rendszergazdája. hello Contoso rendszergazda lehetővé teszi, hogy ez megadásával hello fejlesztői-teszt Microsoft azonosítóját. hello parancsmag nem küld e-mailek toohello megadott Microsoft-azonosítót. hello kapcsolatcsoport tulajdonosát kell tooexplicitly értesítés hello más előfizetés tulajdonosa, amely engedélyezési hello befejeződött.

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0

**Engedélyek ellenőrzése**

hello kapcsolatcsoport tulajdonosát hello a következő parancsmag futtatásával egy adott körön kiállított összes engedélyek tekinthetők át:

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


**Engedélyek frissítése**

hello kapcsolatcsoport tulajdonosát hello a következő parancsmag használatával módosíthatja az engedélyeket:

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


**Engedélyek törlése**

hello kapcsolatcsoport tulajdonosát képes visszavonni vagy törlése engedélyek toohello felhasználói hello a következő parancsmag futtatásával:

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### <a name="circuit-user-operations"></a>Kör felhasználói műveletek

**Engedélyek ellenőrzése**

hello áramkör felhasználói engedélyek hello a következő parancsmag használatával tekintse át:

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

**Hivatkozás engedélyek váltja be**

hello áramkör felhasználó futtathatja a következő parancsmag tooredeem hello egy hivatkozás hitelesítési:

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

Futtassa ezt a parancsot az újonnan csatolt hello előfizetésben hello virtuális hálózathoz:

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"

## <a name="next-steps"></a>Következő lépések
ExpressRoute kapcsolatos további információkért lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md).

