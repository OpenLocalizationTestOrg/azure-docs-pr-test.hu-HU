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
# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a><span data-ttu-id="79e5e-103">Az App Service-környezetek egy többrétegű biztonsági architektúra megvalósítása</span><span class="sxs-lookup"><span data-stu-id="79e5e-103">Implementing a Layered Security Architecture with App Service Environments</span></span>
## <a name="overview"></a><span data-ttu-id="79e5e-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="79e5e-104">Overview</span></span>
<span data-ttu-id="79e5e-105">App Service Environment-környezetek adjon meg egy virtuális hálózatot helyezett izolált futtatókörnyezetben, mivel a fejlesztők egy többrétegű biztonsági architektúra eltérő szintű hálózati hozzáférést biztosít az egyes fizikai alkalmazás rétegek hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="79e5e-105">Since App Service Environments provide an isolated runtime environment deployed into a virtual network, developers can create a layered security architecture providing differing levels of network access for each physical application tier.</span></span>

<span data-ttu-id="79e5e-106">Egy közös desire toohide API vissza-végpontok általános Internet-hozzáférés, és csak a felsőbb rétegbeli webes alkalmazások által meghívott API-k toobe engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="79e5e-106">A common desire is toohide API back-ends from general Internet access, and only allow APIs toobe called by upstream web apps.</span></span>  <span data-ttu-id="79e5e-107">[Hálózati biztonsági csoportokkal (NSG-k)] [ NetworkSecurityGroups] App Service Environment-környezetek toorestrict nyilvános tooAPI alkalmazásokat tartalmazó alhálózatok is használhatók.</span><span class="sxs-lookup"><span data-stu-id="79e5e-107">[Network security groups (NSGs)][NetworkSecurityGroups] can be used on subnets containing App Service Environments toorestrict public access tooAPI applications.</span></span>

<span data-ttu-id="79e5e-108">az alábbi ábrán hello egy példa architektúra látható, az App Service-környezetek telepített WebAPI-alapú alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="79e5e-108">hello diagram below shows an example architecture with a WebAPI based app deployed on an App Service Environment.</span></span>  <span data-ttu-id="79e5e-109">Három külön web app-példányt, a három különálló App Service Environment-környezetek, ellenőrizze a háttér-hívások toohello WebAPI ugyanahhoz az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="79e5e-109">Three separate web app instances, deployed on three separate App Service Environments, make back-end calls toohello same WebAPI app.</span></span>

![Fogalmi architektúra][ConceptualArchitecture] 

<span data-ttu-id="79e5e-111">hello zöld plusz jel jelzi, hogy hello hálózati biztonsági csoportot tartalmazó "apiase" hello alhálózaton lehetővé teszi, hogy hello érkező bejövő hívások felsőbb rétegbeli webes alkalmazásokhoz, mint magából jól hívások.</span><span class="sxs-lookup"><span data-stu-id="79e5e-111">hello green plus signs indicate that hello network security group on hello subnet containing "apiase" allows inbound calls from hello upstream web apps, as well calls from itself.</span></span>  <span data-ttu-id="79e5e-112">Azonban az azonos hálózati biztonsági csoport kifejezetten megtagadja hello toogeneral elérni bejövő hello internetes forgalmát.</span><span class="sxs-lookup"><span data-stu-id="79e5e-112">However hello same network security group explicitly denies access toogeneral inbound traffic from hello Internet.</span></span> 

<span data-ttu-id="79e5e-113">Ez a témakör további része hello tartalmazó "apiase" hello alhálózaton végigvezeti a hello lépéseket szükséges tooconfigure hello hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="79e5e-113">hello remainder of this topic walks through hello steps needed tooconfigure hello network security group on hello subnet containing "apiase".</span></span>

## <a name="determining-hello-network-behavior"></a><span data-ttu-id="79e5e-114">Hello hálózati viselkedésének meghatározása</span><span class="sxs-lookup"><span data-stu-id="79e5e-114">Determining hello Network Behavior</span></span>
<span data-ttu-id="79e5e-115">Rendelés tooknow milyen hálózati biztonsági szabályok van szükség, akkor kell toodetermine mely hálózati ügyfelek részére engedélyezett tooreach hello App Service Environment-környezet tartalmazó hello API-alkalmazást, és mely ügyfelek le lesz tiltva.</span><span class="sxs-lookup"><span data-stu-id="79e5e-115">In order tooknow what network security rules are needed, you need toodetermine which network clients will be allowed tooreach hello App Service Environment containing hello API app, and which clients will be blocked.</span></span>

<span data-ttu-id="79e5e-116">Mivel a [hálózati biztonsági csoportokkal (NSG-k)] [ NetworkSecurityGroups] alkalmazott toosubnets, és telepítve vannak az App Service Environment-környezetek alhálózatokra, egy NSG hello szabályaival alkalmazása közé tartoznak túl**összes** az App Service-környezetek futó alkalmazásokra.</span><span class="sxs-lookup"><span data-stu-id="79e5e-116">Since [network security groups (NSGs)][NetworkSecurityGroups] are applied toosubnets, and App Service Environments are deployed into subnets, hello rules contained in an NSG apply too**all** apps running on an App Service Environment.</span></span>  <span data-ttu-id="79e5e-117">Hello mintaarchitektúrája ebben a cikkben után a hálózati biztonsági csoport "apiase" hello "apiase" App Service Environment-környezet hello védelemmel látja el futó összes alkalmazást tartalmazó alkalmazott toohello alhálózati azonos számítógépen állítsa be a biztonsági szabályok.</span><span class="sxs-lookup"><span data-stu-id="79e5e-117">Using hello sample architecture for this article, once a network security group is applied toohello subnet containing "apiase", all apps running on hello "apiase" App Service Environment will be protected by hello same set of security rules.</span></span> 

* <span data-ttu-id="79e5e-118">**Határozza meg a felsőbb rétegbeli hívóknak hello kimenő IP-címe:** hello IP-címeket hello fölérendelt hívók újdonságai?</span><span class="sxs-lookup"><span data-stu-id="79e5e-118">**Determine hello outbound IP address of upstream callers:**  What is hello IP address or addresses of hello upstream callers?</span></span>  <span data-ttu-id="79e5e-119">Ezeknél a címeknél kifejezetten engedélyezett hozzáférési hello NSG toobe lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="79e5e-119">These addresses will need toobe explicitly allowed access in hello NSG.</span></span>  <span data-ttu-id="79e5e-120">App Service Environment-környezetek közötti hívások "Internet" hívások minősülnek, mivel ez azt jelenti, hello kimenő IP cím tooeach a hello három felsőbb szintű App Service Environment-környezetek igényeinek toobe hozzáférhetnek a hello NSG hello "apiase" alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="79e5e-120">Since calls between App Service Environments are considered "Internet" calls, this means hello outbound IP address assigned tooeach of hello three upstream App Service Environments needs toobe allowed access in hello NSG for hello "apiase" subnet.</span></span>   <span data-ttu-id="79e5e-121">Kapcsolatban további részleteket a hello kimenő IP-cím meghatározása egy App Service Environment-környezetben futó alkalmazásokhoz című hello [hálózati architektúra] [ NetworkArchitecture] a cikk áttekintése.</span><span class="sxs-lookup"><span data-stu-id="79e5e-121">For more details on determining hello outbound IP address for apps running in an App Service Environment see hello [Network Architecture][NetworkArchitecture] Overview article.</span></span>
* <span data-ttu-id="79e5e-122">**Háttér-API-alkalmazás hello kell magát toocall?**</span><span class="sxs-lookup"><span data-stu-id="79e5e-122">**Will hello back-end API app need toocall itself?**</span></span>  <span data-ttu-id="79e5e-123">Egyes esetekben kihagyott és finom pont a amikor hello háttéralkalmazás kell magát toocall hello lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="79e5e-123">A sometimes overlooked and subtle point is hello scenario where hello back-end application needs toocall itself.</span></span>  <span data-ttu-id="79e5e-124">Ha egy háttér-API-alkalmazás az App Service-környezetek toocall magának kell, ez is kezeli az "Internet" hívásként.</span><span class="sxs-lookup"><span data-stu-id="79e5e-124">If a back-end API application on an App Service Environment needs toocall itself, this is also treated as an "Internet" call.</span></span>  <span data-ttu-id="79e5e-125">Hello mintaarchitektúrája ehhez hello kimenő IP-címről hello "apiase" App Service Environment-környezet, valamint a hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="79e5e-125">In hello sample architecture this requires allowing access from hello outbound IP address of hello "apiase" App Service Environment as well.</span></span>

## <a name="setting-up-hello-network-security-group"></a><span data-ttu-id="79e5e-126">Hálózati biztonsági csoport hello beállítása</span><span class="sxs-lookup"><span data-stu-id="79e5e-126">Setting up hello Network Security Group</span></span>
<span data-ttu-id="79e5e-127">Ha hello a kimenő IP-címek ismert, hello tovább lépés tooconstruct hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="79e5e-127">Once hello set of outbound IP addresses are known, hello next step is tooconstruct a network security group.</span></span>  <span data-ttu-id="79e5e-128">Hálózati biztonsági csoportok mindkét Resource Manager-alapú virtuális hálózatokon, valamint a klasszikus virtuális hálózatok hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="79e5e-128">Network security groups can be created for both Resource Manager based virtual networks, as well as classic virtual networks.</span></span>  <span data-ttu-id="79e5e-129">hello az alábbi példák létrehozásához és konfigurálásához egy NSG-t egy Powershell-lel klasszikus virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="79e5e-129">hello examples below show creating and configuring an NSG on a classic virtual network using Powershell.</span></span>

<span data-ttu-id="79e5e-130">Hello minta architektúra hello környezetben található déli középső Régiójában, egy üres NSG az adott régióban jön létre:</span><span class="sxs-lookup"><span data-stu-id="79e5e-130">For hello sample architecture, hello environments are located in South Central US, so an empty NSG is created in that region:</span></span>

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

<span data-ttu-id="79e5e-131">Először egy explicit módon engedélyezze szabály hello Azure felügyeleti infrastruktúra a hello cikkben leírtaknak megfelelően a rendszer hozzá [bejövő forgalom] [ InboundTraffic] App Service Environment-környezetek számára.</span><span class="sxs-lookup"><span data-stu-id="79e5e-131">First an explicit allow rule is added for hello Azure management infrastructure as noted in hello article on [inbound traffic][InboundTraffic] for App Service Environments.</span></span>

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

<span data-ttu-id="79e5e-132">A következő két szabályok hozzáadása történik meg tooallow HTTP és HTTPS hívásait hello első felsőbb szintű App Service Environment-környezet ("fe1ase").</span><span class="sxs-lookup"><span data-stu-id="79e5e-132">Next, two rules are added tooallow HTTP and HTTPS calls from hello first upstream App Service Environment ("fe1ase").</span></span>

    #Grant access toorequests from hello first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="79e5e-133">Rinse, és ismételje meg a hello második és harmadik felsőbb szintű App Service Environment-környezetek ("fe2ase" és "fe3ase").</span><span class="sxs-lookup"><span data-stu-id="79e5e-133">Rinse and repeat for hello second and third upstream App Service Environments ("fe2ase"and "fe3ase").</span></span>

    #Grant access toorequests from hello second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

    #Grant access toorequests from hello third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="79e5e-134">Hozzáférés toohello kimenő IP-cím hello háttér-API App Service Environment-környezet végül adja meg, hogy vissza tudja hívja az magát.</span><span class="sxs-lookup"><span data-stu-id="79e5e-134">Lastly, grant access toohello outbound IP address of hello back-end API's App Service Environment so that it can call back into itself.</span></span>

    #Allow apps on hello apiase environment toocall back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="79e5e-135">Nincs más hálózati biztonsági szabály kell konfigurálni, mert minden NSG tartozik egy alapértelmezett szabályokat, amelyek blokkolják a bejövő hozzáférést a hello Internet alapértelmezés szerint toobe.</span><span class="sxs-lookup"><span data-stu-id="79e5e-135">No other network security rules need toobe configured because every NSG has a set of default rules that block inbound access from hello Internet by default.</span></span>

<span data-ttu-id="79e5e-136">hello hello hálózati biztonsági csoport szabályainak teljes listája az alábbiakban tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="79e5e-136">hello full list of rules in hello network security group are shown below.</span></span>  <span data-ttu-id="79e5e-137">Vegye figyelembe, hogyan letiltja az hello utolsó szabályt, amely ki van jelölve, a bejövő elérését az összes hívó eltérő azokat, amelyek kifejezetten hozzáférést kapott.</span><span class="sxs-lookup"><span data-stu-id="79e5e-137">Note how hello last rule, which is highlighted, blocks inbound access from all callers other than those which have been explicitly granted access.</span></span>

![NSG-konfiguráció][NSGConfiguration] 

<span data-ttu-id="79e5e-139">utolsó lépésként hello tooapply hello NSG toohello alhálózatot, amely tartalmazza a hello "apiase" App Service Environment-környezet.</span><span class="sxs-lookup"><span data-stu-id="79e5e-139">hello final step is tooapply hello NSG toohello subnet that contains hello "apiase" App Service Environment.</span></span>  

     #Apply hello NSG toohello backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

<span data-ttu-id="79e5e-140">Hello alkalmazott NSG toohello alhálózattal csak hello három felsőbb szintű App Service Environment-környezetek és hello App Service Environment-környezet tartalmazó hello API-háttér engedélyezettek toocall hello "apiase" környezetbe.</span><span class="sxs-lookup"><span data-stu-id="79e5e-140">With hello NSG applied toohello subnet, only hello three upstream App Service Environments, and hello App Service Environment containing hello API back-end, are allowed toocall into hello "apiase" environment.</span></span>

## <a name="additional-links-and-information"></a><span data-ttu-id="79e5e-141">További hivatkozások és információk</span><span class="sxs-lookup"><span data-stu-id="79e5e-141">Additional Links and Information</span></span>
<span data-ttu-id="79e5e-142">Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="79e5e-142">All articles and How-To's for App Service Environments are available in hello [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="79e5e-143">Információ a [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="79e5e-143">Information about [network security groups](../virtual-network/virtual-networks-nsg.md).</span></span> 

<span data-ttu-id="79e5e-144">Understanding [kimenő IP-címek] [ NetworkArchitecture] és az App Service Environment-környezetek.</span><span class="sxs-lookup"><span data-stu-id="79e5e-144">Understanding [outbound IP addresses][NetworkArchitecture] and App Service Environments.</span></span>

<span data-ttu-id="79e5e-145">[Hálózati portok] [ InboundTraffic] App Service Environment-környezetek használják.</span><span class="sxs-lookup"><span data-stu-id="79e5e-145">[Network ports][InboundTraffic] used by App Service Environments.</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
