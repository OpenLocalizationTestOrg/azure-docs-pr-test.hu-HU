---
title: "aaaLayered az App Service Environment-környezetek biztonsági architektúrája"
description: "Az App Service Environment-környezetek egy többrétegű biztonsági architektúra megvalósítása."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 73ce0213-bd3e-4876-b1ed-5ecad4ad5601
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: stefsch
ms.openlocfilehash: 0627ba6fa849908506fe62c451c888c147cabc03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Az App Service-környezetek egy többrétegű biztonsági architektúra megvalósítása
## <a name="overview"></a>Áttekintés
App Service Environment-környezetek adjon meg egy virtuális hálózatot helyezett izolált futtatókörnyezetben, mivel a fejlesztők egy többrétegű biztonsági architektúra eltérő szintű hálózati hozzáférést biztosít az egyes fizikai alkalmazás rétegek hozhat létre.

Egy közös desire toohide API vissza-végpontok általános Internet-hozzáférés, és csak a felsőbb rétegbeli webes alkalmazások által meghívott API-k toobe engedélyezése.  [Hálózati biztonsági csoportokkal (NSG-k)] [ NetworkSecurityGroups] App Service Environment-környezetek toorestrict nyilvános tooAPI alkalmazásokat tartalmazó alhálózatok is használhatók.

az alábbi ábrán hello egy példa architektúra látható, az App Service-környezetek telepített WebAPI-alapú alkalmazáshoz.  Három külön web app-példányt, a három különálló App Service Environment-környezetek, ellenőrizze a háttér-hívások toohello WebAPI ugyanahhoz az alkalmazáshoz.

![Fogalmi architektúra][ConceptualArchitecture] 

hello zöld plusz jel jelzi, hogy hello hálózati biztonsági csoportot tartalmazó "apiase" hello alhálózaton lehetővé teszi, hogy hello érkező bejövő hívások felsőbb rétegbeli webes alkalmazásokhoz, mint magából jól hívások.  Azonban az azonos hálózati biztonsági csoport kifejezetten megtagadja hello toogeneral elérni bejövő hello internetes forgalmát. 

Ez a témakör további része hello tartalmazó "apiase" hello alhálózaton végigvezeti a hello lépéseket szükséges tooconfigure hello hálózati biztonsági csoport.

## <a name="determining-hello-network-behavior"></a>Hello hálózati viselkedésének meghatározása
Rendelés tooknow milyen hálózati biztonsági szabályok van szükség, akkor kell toodetermine mely hálózati ügyfelek részére engedélyezett tooreach hello App Service Environment-környezet tartalmazó hello API-alkalmazást, és mely ügyfelek le lesz tiltva.

Mivel a [hálózati biztonsági csoportokkal (NSG-k)] [ NetworkSecurityGroups] alkalmazott toosubnets, és telepítve vannak az App Service Environment-környezetek alhálózatokra, egy NSG hello szabályaival alkalmazása közé tartoznak túl**összes** az App Service-környezetek futó alkalmazásokra.  Hello mintaarchitektúrája ebben a cikkben után a hálózati biztonsági csoport "apiase" hello "apiase" App Service Environment-környezet hello védelemmel látja el futó összes alkalmazást tartalmazó alkalmazott toohello alhálózati azonos számítógépen állítsa be a biztonsági szabályok. 

* **Határozza meg a felsőbb rétegbeli hívóknak hello kimenő IP-címe:** hello IP-címeket hello fölérendelt hívók újdonságai?  Ezeknél a címeknél kifejezetten engedélyezett hozzáférési hello NSG toobe lesz szüksége.  App Service Environment-környezetek közötti hívások "Internet" hívások minősülnek, mivel ez azt jelenti, hello kimenő IP cím tooeach a hello három felsőbb szintű App Service Environment-környezetek igényeinek toobe hozzáférhetnek a hello NSG hello "apiase" alhálózathoz.   Kapcsolatban további részleteket a hello kimenő IP-cím meghatározása egy App Service Environment-környezetben futó alkalmazásokhoz című hello [hálózati architektúra] [ NetworkArchitecture] a cikk áttekintése.
* **Háttér-API-alkalmazás hello kell magát toocall?**  Egyes esetekben kihagyott és finom pont a amikor hello háttéralkalmazás kell magát toocall hello lehetőséget.  Ha egy háttér-API-alkalmazás az App Service-környezetek toocall magának kell, ez is kezeli az "Internet" hívásként.  Hello mintaarchitektúrája ehhez hello kimenő IP-címről hello "apiase" App Service Environment-környezet, valamint a hozzáférés.

## <a name="setting-up-hello-network-security-group"></a>Hálózati biztonsági csoport hello beállítása
Ha hello a kimenő IP-címek ismert, hello tovább lépés tooconstruct hálózati biztonsági csoport.  Hálózati biztonsági csoportok mindkét Resource Manager-alapú virtuális hálózatokon, valamint a klasszikus virtuális hálózatok hozhatók létre.  hello az alábbi példák létrehozásához és konfigurálásához egy NSG-t egy Powershell-lel klasszikus virtuális hálózaton.

Hello minta architektúra hello környezetben található déli középső Régiójában, egy üres NSG az adott régióban jön létre:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Először egy explicit módon engedélyezze szabály hello Azure felügyeleti infrastruktúra a hello cikkben leírtaknak megfelelően a rendszer hozzá [bejövő forgalom] [ InboundTraffic] App Service Environment-környezetek számára.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

A következő két szabályok hozzáadása történik meg tooallow HTTP és HTTPS hívásait hello első felsőbb szintű App Service Environment-környezet ("fe1ase").

    #Grant access toorequests from hello first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Rinse, és ismételje meg a hello második és harmadik felsőbb szintű App Service Environment-környezetek ("fe2ase" és "fe3ase").

    #Grant access toorequests from hello second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

    #Grant access toorequests from hello third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Hozzáférés toohello kimenő IP-cím hello háttér-API App Service Environment-környezet végül adja meg, hogy vissza tudja hívja az magát.

    #Allow apps on hello apiase environment toocall back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Nincs más hálózati biztonsági szabály kell konfigurálni, mert minden NSG tartozik egy alapértelmezett szabályokat, amelyek blokkolják a bejövő hozzáférést a hello Internet alapértelmezés szerint toobe.

hello hello hálózati biztonsági csoport szabályainak teljes listája az alábbiakban tekintheti meg.  Vegye figyelembe, hogyan letiltja az hello utolsó szabályt, amely ki van jelölve, a bejövő elérését az összes hívó eltérő azokat, amelyek kifejezetten hozzáférést kapott.

![NSG-konfiguráció][NSGConfiguration] 

utolsó lépésként hello tooapply hello NSG toohello alhálózatot, amely tartalmazza a hello "apiase" App Service Environment-környezet.  

     #Apply hello NSG toohello backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Hello alkalmazott NSG toohello alhálózattal csak hello három felsőbb szintű App Service Environment-környezetek és hello App Service Environment-környezet tartalmazó hello API-háttér engedélyezettek toocall hello "apiase" környezetbe.

## <a name="additional-links-and-information"></a>További hivatkozások és információk
Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Információ a [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md). 

Understanding [kimenő IP-címek] [ NetworkArchitecture] és az App Service Environment-környezetek.

[Hálózati portok] [ InboundTraffic] App Service Environment-környezetek használják.

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
