---
title: "Az App Service-környezetek többrétegű biztonsági architektúrája"
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
ms.openlocfilehash: 0fb02c13f99a8f4a46e0142c20da3b152c809b6b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a><span data-ttu-id="be9c4-103">Az App Service-környezetek egy többrétegű biztonsági architektúra megvalósítása</span><span class="sxs-lookup"><span data-stu-id="be9c4-103">Implementing a Layered Security Architecture with App Service Environments</span></span>
## <a name="overview"></a><span data-ttu-id="be9c4-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="be9c4-104">Overview</span></span>
<span data-ttu-id="be9c4-105">App Service Environment-környezetek adjon meg egy virtuális hálózatot helyezett izolált futtatókörnyezetben, mivel a fejlesztők egy többrétegű biztonsági architektúra eltérő szintű hálózati hozzáférést biztosít az egyes fizikai alkalmazás rétegek hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="be9c4-105">Since App Service Environments provide an isolated runtime environment deployed into a virtual network, developers can create a layered security architecture providing differing levels of network access for each physical application tier.</span></span>

<span data-ttu-id="be9c4-106">A közös desire API vissza-végpontok általános Internet-hozzáférés elrejtéséhez, és csak a felsőbb rétegbeli webes alkalmazások által használt hívása API-k engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="be9c4-106">A common desire is to hide API back-ends from general Internet access, and only allow APIs to be called by upstream web apps.</span></span>  <span data-ttu-id="be9c4-107">[Hálózati biztonsági csoportokkal (NSG-k)] [ NetworkSecurityGroups] nyilvános hozzáférés korlátozása API-alkalmazások App Service Environment-környezetek tartalmazó alhálózatok használható.</span><span class="sxs-lookup"><span data-stu-id="be9c4-107">[Network security groups (NSGs)][NetworkSecurityGroups] can be used on subnets containing App Service Environments to restrict public access to API applications.</span></span>

<span data-ttu-id="be9c4-108">Az alábbi ábrán egy példa architektúra látható, az App Service-környezetek telepített WebAPI-alapú alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="be9c4-108">The diagram below shows an example architecture with a WebAPI based app deployed on an App Service Environment.</span></span>  <span data-ttu-id="be9c4-109">Három különálló webes alkalmazás-példányt, három különálló App Service Environment-környezetek, a háttér-hívások indítása WebAPI ugyanahhoz az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="be9c4-109">Three separate web app instances, deployed on three separate App Service Environments, make back-end calls to the same WebAPI app.</span></span>

![Fogalmi architektúra][ConceptualArchitecture] 

<span data-ttu-id="be9c4-111">A zöld plusz jel jelzi, hogy engedélyezi-e a hálózati biztonsági csoportot az "apiase" tartalmazó alhálózat bejövő hívások a felsőbb rétegbeli webes alkalmazásokból, jól hívások magából.</span><span class="sxs-lookup"><span data-stu-id="be9c4-111">The green plus signs indicate that the network security group on the subnet containing "apiase" allows inbound calls from the upstream web apps, as well calls from itself.</span></span>  <span data-ttu-id="be9c4-112">Azonban a hálózati biztonsági csoporton kifejezetten megtagadja általános bejövő forgalom az internethez való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="be9c4-112">However the same network security group explicitly denies access to general inbound traffic from the Internet.</span></span> 

<span data-ttu-id="be9c4-113">Ez a témakör további része végigvezeti a lépéseken, amelyekkel a hálózati biztonsági csoport konfigurálása "apiase" tartalmazó alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="be9c4-113">The remainder of this topic walks through the steps needed to configure the network security group on the subnet containing "apiase".</span></span>

## <a name="determining-the-network-behavior"></a><span data-ttu-id="be9c4-114">A hálózati probléma meghatározása</span><span class="sxs-lookup"><span data-stu-id="be9c4-114">Determining the Network Behavior</span></span>
<span data-ttu-id="be9c4-115">Ahhoz, hogy tudja, milyen hálózati biztonsági szabályok szükségesek, meg kell határoznia, melyik hálózati ügyfelek részére engedélyezve lesz az App Service-környezet, az API-alkalmazást tartalmazó elérni, és mely ügyfelek le lesz tiltva.</span><span class="sxs-lookup"><span data-stu-id="be9c4-115">In order to know what network security rules are needed, you need to determine which network clients will be allowed to reach the App Service Environment containing the API app, and which clients will be blocked.</span></span>

<span data-ttu-id="be9c4-116">Mivel [hálózati biztonsági csoportokkal (NSG-k)] [ NetworkSecurityGroups] alkalmazott alhálózatok, és az App Service Environment-környezetek alhálózatokra vannak telepítve, egy NSG foglalt szabályok vonatkoznak **összes** alkalmazások az App Service-környezetek futnak.</span><span class="sxs-lookup"><span data-stu-id="be9c4-116">Since [network security groups (NSGs)][NetworkSecurityGroups] are applied to subnets, and App Service Environments are deployed into subnets, the rules contained in an NSG apply to **all** apps running on an App Service Environment.</span></span>  <span data-ttu-id="be9c4-117">Használja a mintaarchitektúrája ebben a cikkben után a hálózati biztonsági csoport van alkalmazva tartalmazó "apiase", "apiase" App Service Environment-környezet futó minden alkalmazás fogja védeni az ugyanazokat a biztonsági szabályok.</span><span class="sxs-lookup"><span data-stu-id="be9c4-117">Using the sample architecture for this article, once a network security group is applied to the subnet containing "apiase", all apps running on the "apiase" App Service Environment will be protected by the same set of security rules.</span></span> 

* <span data-ttu-id="be9c4-118">**Határozza meg a felsőbb rétegbeli hívók kimenő IP-cím:** Mi az az IP-címeket a felsőbb rétegbeli hívók?</span><span class="sxs-lookup"><span data-stu-id="be9c4-118">**Determine the outbound IP address of upstream callers:**  What is the IP address or addresses of the upstream callers?</span></span>  <span data-ttu-id="be9c4-119">Ezeket a címeket kell explicit módon hozzáférhessen az NSG.</span><span class="sxs-lookup"><span data-stu-id="be9c4-119">These addresses will need to be explicitly allowed access in the NSG.</span></span>  <span data-ttu-id="be9c4-120">App Service Environment-környezetek közötti hívások "Internet" hívások minősülnek, mivel ez azt jelenti, a kimenő IP-címet hozzárendelni a három felsőbb szintű App Service-környezetek mindegyikének kell az NSG az "apiase" alhálózat hozzáférése engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="be9c4-120">Since calls between App Service Environments are considered "Internet" calls, this means the outbound IP address assigned to each of the three upstream App Service Environments needs to be allowed access in the NSG for the "apiase" subnet.</span></span>   <span data-ttu-id="be9c4-121">A kimenő IP-cím meghatározása egy App Service Environment-környezetben futó alkalmazásokhoz című vonatkozó részletes információért a [hálózati architektúra] [ NetworkArchitecture] a cikk áttekintése.</span><span class="sxs-lookup"><span data-stu-id="be9c4-121">For more details on determining the outbound IP address for apps running in an App Service Environment see the [Network Architecture][NetworkArchitecture] Overview article.</span></span>
* <span data-ttu-id="be9c4-122">**A háttér-API-alkalmazásba kell hívnia magának?**</span><span class="sxs-lookup"><span data-stu-id="be9c4-122">**Will the back-end API app need to call itself?**</span></span>  <span data-ttu-id="be9c4-123">Néha kihagyott és finom pont a forgatókönyvet, ahol a háttér-alkalmazást kell hívhatja saját magát.</span><span class="sxs-lookup"><span data-stu-id="be9c4-123">A sometimes overlooked and subtle point is the scenario where the back-end application needs to call itself.</span></span>  <span data-ttu-id="be9c4-124">Ha egy háttér-API-alkalmazás az App Service-környezetek hívható meg magának kell, ez is kezeli az "Internet" hívásként.</span><span class="sxs-lookup"><span data-stu-id="be9c4-124">If a back-end API application on an App Service Environment needs to call itself, this is also treated as an "Internet" call.</span></span>  <span data-ttu-id="be9c4-125">A minta-architektúrában ehhez a kimenő IP-címét a "apiase" App Service Environment-környezet, valamint a hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="be9c4-125">In the sample architecture this requires allowing access from the outbound IP address of the "apiase" App Service Environment as well.</span></span>

## <a name="setting-up-the-network-security-group"></a><span data-ttu-id="be9c4-126">A hálózati biztonsági csoport</span><span class="sxs-lookup"><span data-stu-id="be9c4-126">Setting up the Network Security Group</span></span>
<span data-ttu-id="be9c4-127">Amennyiben az ismert kimenő IP-címek készlete, a következő lépésre hálózati biztonsági csoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="be9c4-127">Once the set of outbound IP addresses are known, the next step is to construct a network security group.</span></span>  <span data-ttu-id="be9c4-128">Hálózati biztonsági csoportok mindkét Resource Manager-alapú virtuális hálózatokon, valamint a klasszikus virtuális hálózatok hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="be9c4-128">Network security groups can be created for both Resource Manager based virtual networks, as well as classic virtual networks.</span></span>  <span data-ttu-id="be9c4-129">Az alábbi példák bemutatják, létrehozása és konfigurálása egy NSG-t egy Powershell-lel klasszikus virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="be9c4-129">The examples below show creating and configuring an NSG on a classic virtual network using Powershell.</span></span>

<span data-ttu-id="be9c4-130">Minta architektúrájának a környezetben található déli középső Régiójában, egy üres NSG az adott régióban jön létre:</span><span class="sxs-lookup"><span data-stu-id="be9c4-130">For the sample architecture, the environments are located in South Central US, so an empty NSG is created in that region:</span></span>

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

<span data-ttu-id="be9c4-131">Először egy explicit engedélyezése a szabály szerepel-e az Azure felügyeleti infrastruktúrát a a cikkben leírtaknak megfelelően [bejövő forgalom] [ InboundTraffic] App Service Environment-környezetek számára.</span><span class="sxs-lookup"><span data-stu-id="be9c4-131">First an explicit allow rule is added for the Azure management infrastructure as noted in the article on [inbound traffic][InboundTraffic] for App Service Environments.</span></span>

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

<span data-ttu-id="be9c4-132">A következő két szabály hozzáadódnak engedélyezze a HTTP és HTTPS hívásokat a az első felsőbb szintű App Service Environment-környezet ("fe1ase").</span><span class="sxs-lookup"><span data-stu-id="be9c4-132">Next, two rules are added to allow HTTP and HTTPS calls from the first upstream App Service Environment ("fe1ase").</span></span>

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="be9c4-133">Le, és ismételje meg a második és harmadik felsőbb szintű App Service Environment-környezetek ("fe2ase" és "fe3ase").</span><span class="sxs-lookup"><span data-stu-id="be9c4-133">Rinse and repeat for the second and third upstream App Service Environments ("fe2ase"and "fe3ase").</span></span>

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="be9c4-134">Végül hozzáférést a háttér-API App Service Environment-környezet kimenő IP-címét, hogy vissza tudja hívja az magát.</span><span class="sxs-lookup"><span data-stu-id="be9c4-134">Lastly, grant access to the outbound IP address of the back-end API's App Service Environment so that it can call back into itself.</span></span>

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="be9c4-135">Nincs más hálózati biztonsági szabály be kell állítani, mert minden NSG tartozik egy alapértelmezett szabályokkal, amelyeket az internetről bejövő hozzáférésének letiltására alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="be9c4-135">No other network security rules need to be configured because every NSG has a set of default rules that block inbound access from the Internet by default.</span></span>

<span data-ttu-id="be9c4-136">A hálózati biztonsági csoport szabályainak teljes listája az alábbiakban tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="be9c4-136">The full list of rules in the network security group are shown below.</span></span>  <span data-ttu-id="be9c4-137">Vegye figyelembe, hogyan letiltja az az utolsó szabályt, amely ki van jelölve, a bejövő elérését az összes hívó eltérő azokat, amelyek kifejezetten hozzáférést kapott.</span><span class="sxs-lookup"><span data-stu-id="be9c4-137">Note how the last rule, which is highlighted, blocks inbound access from all callers other than those which have been explicitly granted access.</span></span>

![NSG-konfiguráció][NSGConfiguration] 

<span data-ttu-id="be9c4-139">Az utolsó lépés, hogy az NSG alkalmazása az alhálózatot, amely tartalmazza a "apiase" App Service Environment-környezet.</span><span class="sxs-lookup"><span data-stu-id="be9c4-139">The final step is to apply the NSG to the subnet that contains the "apiase" App Service Environment.</span></span>  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

<span data-ttu-id="be9c4-140">Az NSG alkalmazva, az csak a három felsőbb szintű App Service Environment-környezetek, és az App Service-környezet, az API-háttér tartalmazó engedélyezettek hívni a "apiase" környezetbe.</span><span class="sxs-lookup"><span data-stu-id="be9c4-140">With the NSG applied to the subnet, only the three upstream App Service Environments, and the App Service Environment containing the API back-end, are allowed to call into the "apiase" environment.</span></span>

## <a name="additional-links-and-information"></a><span data-ttu-id="be9c4-141">További hivatkozások és információk</span><span class="sxs-lookup"><span data-stu-id="be9c4-141">Additional Links and Information</span></span>
<span data-ttu-id="be9c4-142">Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el a [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="be9c4-142">All articles and How-To's for App Service Environments are available in the [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="be9c4-143">Információ a [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="be9c4-143">Information about [network security groups](../virtual-network/virtual-networks-nsg.md).</span></span> 

<span data-ttu-id="be9c4-144">Understanding [kimenő IP-címek] [ NetworkArchitecture] és az App Service Environment-környezetek.</span><span class="sxs-lookup"><span data-stu-id="be9c4-144">Understanding [outbound IP addresses][NetworkArchitecture] and App Service Environments.</span></span>

<span data-ttu-id="be9c4-145">[Hálózati portok] [ InboundTraffic] App Service Environment-környezetek használják.</span><span class="sxs-lookup"><span data-stu-id="be9c4-145">[Network ports][InboundTraffic] used by App Service Environments.</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
