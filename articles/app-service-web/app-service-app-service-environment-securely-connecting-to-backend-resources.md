---
title: "Biztonságos kapcsolódás háttér erőforrásokhoz az App Service-környezet"
description: "További tudnivalók a biztonságos kapcsolódás App Service-környezetből származó."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: f82eb283-a6e7-4923-a00b-4b4ccf7c4b5b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.openlocfilehash: 0b6d3a47dc429c469b37c2c74f546cfeca580358
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a><span data-ttu-id="59e93-103">Biztonságos kapcsolódás háttér erőforrásokhoz az App Service-környezet</span><span class="sxs-lookup"><span data-stu-id="59e93-103">Securely Connecting to Backend Resources from an App Service Environment</span></span>
## <a name="overview"></a><span data-ttu-id="59e93-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="59e93-104">Overview</span></span>
<span data-ttu-id="59e93-105">Mivel az App Service-környezetek mindig jön létre **vagy** az Azure Resource Manager virtuális hálózati **vagy** klasszikus telepítési modell [virtuális hálózati][virtualnetwork], más háttér erőforrások az App Service-környezetek kimenő kapcsolatok áramolhasson az kizárólag a virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="59e93-105">Since an App Service Environment is always created in **either** an Azure Resource Manager virtual network, **or** a classic deployment model [virtual network][virtualnetwork], outbound connections from an App Service Environment to other backend resources can flow exclusively over the virtual network.</span></span>  <span data-ttu-id="59e93-106">2016. június végzett legutóbbi módosítását ASEs is telepíthető a virtuális hálózatok, amelyek nyilvános címtartományt, vagy RFC1918 címterek (azaz</span><span class="sxs-lookup"><span data-stu-id="59e93-106">With a recent change made in June 2016, ASEs can also be deployed into virtual networks that use either public address ranges, or RFC1918 address spaces (i.e.</span></span> <span data-ttu-id="59e93-107">Magáncímeket).</span><span class="sxs-lookup"><span data-stu-id="59e93-107">private addresses).</span></span>  

<span data-ttu-id="59e93-108">Előfordulhat például, a virtuális gépek zárolva 1433-as port a fürtön futó SQL Server.</span><span class="sxs-lookup"><span data-stu-id="59e93-108">For example, there may be a SQL Server running on a cluster of virtual machines with port 1433 locked down.</span></span>  <span data-ttu-id="59e93-109">A végpont lehet ACLd, hogy csak más erőforrásokhoz való hozzáférést a virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="59e93-109">The endpoint may be ACLd to only allow access from other resources on the same virtual network.</span></span>  

<span data-ttu-id="59e93-110">Másik példaként, bizalmas végpontok előfordulhat, hogy működik a helyszíni és csatlakoztatni az Azure-bA vagy [pont-pont] [ SiteToSite] vagy [Azure ExpressRoute] [ ExpressRoute] kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="59e93-110">As another example, sensitive endpoints may run on-premises and be connected to Azure via either [Site-to-Site][SiteToSite] or [Azure ExpressRoute][ExpressRoute] connections.</span></span>  <span data-ttu-id="59e93-111">Ennek eredményeképpen a pont-pont vagy ExpressRoute-alagutak csatlakoztatott virtuális hálózatok csak erőforrások hozzáférhetnek a helyszíni végpont lesz.</span><span class="sxs-lookup"><span data-stu-id="59e93-111">As a result, only resources in virtual networks connected to the Site-to-Site or ExpressRoute tunnels will be able to access on-premises endpoints.</span></span>

<span data-ttu-id="59e93-112">Az összes forgatókönyvekben az App Service-környezetek futó alkalmazások tudnak biztonságosan csatlakozik a különböző kiszolgálók és az erőforrások.</span><span class="sxs-lookup"><span data-stu-id="59e93-112">For all of these scenarios, apps running on an App Service Environment will be able to securely connect to the various servers and resources.</span></span>  <span data-ttu-id="59e93-113">A saját végpontokhoz ugyanazon virtuális hálózatban egy App Service Environment-környezetben futó alkalmazások kimenő forgalom (vagy ugyanahhoz a virtuális hálózathoz csatlakozik), csak a folyamat lesz a virtuális hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="59e93-113">Outbound traffic from apps running in an App Service Environment to private endpoints in the same virtual network (or connected to the same virtual network), will only flow over the virtual network.</span></span>  <span data-ttu-id="59e93-114">Saját végpontokhoz kimenő forgalom nem halad a nyilvános interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="59e93-114">Outbound traffic to private endpoints will not flow over the public Internet.</span></span>

<span data-ttu-id="59e93-115">Egy szerint a végpontokat a virtuális hálózaton belül az App Service-környezetek vonatkozik a kimenő forgalom.</span><span class="sxs-lookup"><span data-stu-id="59e93-115">One caveat applies to outbound traffic from an App Service Environment to endpoints within a virtual network.</span></span>  <span data-ttu-id="59e93-116">App Service Environment-környezetek nem érhető el a virtuális gépek találhatók végpontok a **azonos** az App Service Environment-környezet alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="59e93-116">App Service Environments cannot reach endpoints of virtual machines located in the **same** subnet as the App Service Environment.</span></span>  <span data-ttu-id="59e93-117">Általában ne legyen hiba, amíg az App Service Environment-környezetek telepít egy alhálózatba, csak az App Service Environment-környezet által kizárólagos használatra fenntartva.</span><span class="sxs-lookup"><span data-stu-id="59e93-117">This normally should not be a problem as long as App Service Environments are deployed into a subnet reserved for exclusive use by only the App Service Environment.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a><span data-ttu-id="59e93-118">Kimenő kapcsolatok és a DNS-követelmények</span><span class="sxs-lookup"><span data-stu-id="59e93-118">Outbound Connectivity and DNS Requirements</span></span>
<span data-ttu-id="59e93-119">Az App Service Environment-környezet működését, a különböző végpontok kimenő hozzáférést igényel.</span><span class="sxs-lookup"><span data-stu-id="59e93-119">For an App Service Environment to function properly, it requires outbound access to various endpoints.</span></span> <span data-ttu-id="59e93-120">"A hálózati kapcsolat szükséges" szakaszában van a külső végpontok egy ASE által használt teljes listáját a [ExpressRoute tartozó fürthálózat-konfiguráció](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) cikk.</span><span class="sxs-lookup"><span data-stu-id="59e93-120">A full list of the external endpoints used by an ASE is in the "Required Network Connectivity" section of the [Network Configuration for ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) article.</span></span>

<span data-ttu-id="59e93-121">App Service-környezetek virtuális hálózat egy érvényes DNS-infrastruktúra szükséges.</span><span class="sxs-lookup"><span data-stu-id="59e93-121">App Service Environments require a valid DNS infrastructure configured for the virtual network.</span></span>  <span data-ttu-id="59e93-122">Ha bármilyen okból az App Service Environment-környezet létrehozása után a DNS-konfiguráció módosul, a fejlesztők kényszerítheti az App Service-környezet az új DNS-konfiguráció érvényesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="59e93-122">If for any reason the DNS configuration is changed after an App Service Environment has been created, developers can force an App Service Environment to pick up the new DNS configuration.</span></span>  <span data-ttu-id="59e93-123">A működés közbeni környezet újra kell indítani a portálon az App Service Environment-felügyelet panel tetején található "Restart" ikonnal váltanak, akkor a a környezet az új DNS-konfiguráció érvényesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="59e93-123">Triggering a rolling environment reboot using the "Restart" icon located at the top of the App Service Environment management blade in the portal will cause the environment to pick up the new DNS configuration.</span></span>

<span data-ttu-id="59e93-124">Ajánlott továbbá, hogy a virtuális hálózaton egyéni DNS-kiszolgálók kell-e a telepítő az App Service Environment-környezet létrehozása előtt időben.</span><span class="sxs-lookup"><span data-stu-id="59e93-124">It is also recommended that any custom DNS servers on the vnet be setup ahead of time prior to creating an App Service Environment.</span></span>  <span data-ttu-id="59e93-125">Ha egy virtuális hálózat DNS-konfiguráció megváltozott egy App Service Environment-környezet létrehozása közben, az App Service Environment-környezet létrehozása folyamat sikertelen vezethet.</span><span class="sxs-lookup"><span data-stu-id="59e93-125">If a virtual network's DNS configuration is changed while an App Service Environment is being created, that will result in the App Service Environment creation process failing.</span></span>  <span data-ttu-id="59e93-126">Egy hasonló tekintettel az egy egyéni DNS-kiszolgáló létezik a VPN-átjáró másik végén, és a DNS-kiszolgáló nem érhető el vagy nem érhető el, ha az App Service Environment-környezet létrehozása is sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="59e93-126">In a similar vein, if a custom DNS server exists on the other end of a VPN gateway, and the DNS server is unreachable or unavailable, the App Service Environment creation process will also fail.</span></span>

## <a name="connecting-to-a-sql-server"></a><span data-ttu-id="59e93-127">Kapcsolódás az SQL-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="59e93-127">Connecting to a SQL Server</span></span>
<span data-ttu-id="59e93-128">Egy közös SQL Server-konfigurációs az 1433-as portot figyelő végponttal rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="59e93-128">A common SQL Server configuration has an endpoint listening on port 1433:</span></span>

![SQL Server-végpont][SqlServerEndpoint]

<span data-ttu-id="59e93-130">Kétféleképpen forgalom korlátozására ehhez a végponthoz:</span><span class="sxs-lookup"><span data-stu-id="59e93-130">There are two approaches for restricting traffic to this endpoint:</span></span>

* <span data-ttu-id="59e93-131">[Hálózati hozzáférés-vezérlési listák] [ NetworkAccessControlLists] (hálózati hozzáférés-vezérlési listák)</span><span class="sxs-lookup"><span data-stu-id="59e93-131">[Network Access Control Lists][NetworkAccessControlLists] (Network ACLs)</span></span>
* <span data-ttu-id="59e93-132">[Hálózati biztonsági csoportok][NetworkSecurityGroups]</span><span class="sxs-lookup"><span data-stu-id="59e93-132">[Network Security Groups][NetworkSecurityGroups]</span></span>

## <a name="restricting-access-with-a-network-acl"></a><span data-ttu-id="59e93-133">A hálózati hozzáférés-vezérlési lista a hozzáférés korlátozása</span><span class="sxs-lookup"><span data-stu-id="59e93-133">Restricting Access With a Network ACL</span></span>
<span data-ttu-id="59e93-134">1433-as port a hálózati hozzáférés-vezérlési lista segítségével biztosítható.</span><span class="sxs-lookup"><span data-stu-id="59e93-134">Port 1433 can be secured using a network access control list.</span></span>  <span data-ttu-id="59e93-135">Whitelists ügyfél az alábbi példában egy virtuális hálózaton belül származó címek, és engedélyezi, hogy más ügyfelekkel.</span><span class="sxs-lookup"><span data-stu-id="59e93-135">The example below whitelists client addresses originating from inside of a virtual network, and blocks access to all other clients.</span></span>

![Hálózati hozzáférés vezérlő példa][NetworkAccessControlListExample]

<span data-ttu-id="59e93-137">Az SQL Server fog tudni csatlakozni az SQL Server példányát használja, az azonos virtuális hálózatban lévő App Service Environment-környezetben futó alkalmazások a **belső hálózatok** az SQL Server virtuális gép IP-címet.</span><span class="sxs-lookup"><span data-stu-id="59e93-137">Any applications running in App Service Environment in the same virtual network as the SQL Server will be able to connect to the SQL Server instance using the **VNet internal** IP address for the SQL Server virtual machine.</span></span>  

<span data-ttu-id="59e93-138">Az alábbi példa kapcsolati karakterláncot az SQL Server Privát IP-címére hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="59e93-138">The example connection string below references the SQL Server using its private IP address.</span></span>

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

<span data-ttu-id="59e93-139">Habár a virtuális gép egy nyilvános végpontot, a nyilvános IP-cím használatával történő kapcsolódási kísérletek program elutasítja a hálózati hozzáférés-vezérlési lista miatt.</span><span class="sxs-lookup"><span data-stu-id="59e93-139">Although the virtual machine has a public endpoint as well, connection attempts using the public IP address will be rejected because of the network ACL.</span></span> 

## <a name="restricting-access-with-a-network-security-group"></a><span data-ttu-id="59e93-140">Hálózati biztonsági csoport a hozzáférés korlátozása</span><span class="sxs-lookup"><span data-stu-id="59e93-140">Restricting Access With a Network Security Group</span></span>
<span data-ttu-id="59e93-141">Hálózati biztonsági csoport van egy másik módjáról hozzáférés biztosítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="59e93-141">An alternative approach for securing access is with a network security group.</span></span>  <span data-ttu-id="59e93-142">Hálózati biztonsági csoportok egyedi virtuális gépeket, vagy egy alhálózathoz, virtuális gépeket tartalmazó lehet alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="59e93-142">Network security groups can be applied to individual virtual machines, or to a subnet containing virtual machines.</span></span>

<span data-ttu-id="59e93-143">Először egy hálózati biztonsági csoportot kell létrehozni:</span><span class="sxs-lookup"><span data-stu-id="59e93-143">First a network security group needs to be created:</span></span>

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

<span data-ttu-id="59e93-144">Hozzáférés korlátozása csak a virtuális hálózat belső forgalom a hálózati biztonsági csoport nagyon egyszerű.</span><span class="sxs-lookup"><span data-stu-id="59e93-144">Restricting access to only VNet internal traffic is very simple with a network security group.</span></span>  <span data-ttu-id="59e93-145">Az alapértelmezett szabályok a hálózati biztonsági csoport csak engedélyezik a hozzáférést, az azonos virtuális hálózatban lévő egyéb hálózati ügyfelekről.</span><span class="sxs-lookup"><span data-stu-id="59e93-145">The default rules in a network security group only allow access from other network clients in the same virtual network.</span></span>

<span data-ttu-id="59e93-146">Ennek eredményeképpen zárolás SQL-kiszolgálóhoz való hozzáférés, más dolga, mint egy hálózati biztonsági csoport az alapértelmezett szabály alkalmazása vagy a virtuális gépek futnak az SQL Server, vagy az alhálózatot a virtuális gépeket tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="59e93-146">As a result locking down access to SQL Server is as simple as applying a network security group with its default rules to either the virtual machines running SQL Server, or the subnet containing the virtual machines.</span></span>

<span data-ttu-id="59e93-147">Az alábbi minta hálózati biztonsági csoport az tartalmazó alhálózat vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="59e93-147">The sample below applies a network security group to the containing subnet:</span></span>

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'

<span data-ttu-id="59e93-148">A végeredménynek olyan szabályokat, amelyek külső hozzáférés, miközben lehetővé teszi a virtuális hálózat belső hozzáférés letiltása:</span><span class="sxs-lookup"><span data-stu-id="59e93-148">The end result is a set of security rules that block external access, while allowing VNet internal access:</span></span>

![Alapértelmezett hálózati biztonsági szabályok][DefaultNetworkSecurityRules]

## <a name="getting-started"></a><span data-ttu-id="59e93-150">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="59e93-150">Getting started</span></span>
<span data-ttu-id="59e93-151">Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el a [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="59e93-151">All articles and How-To's for App Service Environments are available in the [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="59e93-152">App Service Environment-környezetek megkezdéséhez, lásd: [App Service Environment bemutatása][IntroToAppServiceEnvironment]</span><span class="sxs-lookup"><span data-stu-id="59e93-152">To get started with App Service Environments, see [Introduction to App Service Environment][IntroToAppServiceEnvironment]</span></span>

<span data-ttu-id="59e93-153">További részletek az App Service-környezet a bejövő forgalom vezérlése körül: [az App Service-környezetek bejövő forgalom szabályozása][ControlInboundASE]</span><span class="sxs-lookup"><span data-stu-id="59e93-153">For details around controlling inbound traffic to your App Service Environment, see [Controlling inbound traffic to an App Service Environment][ControlInboundASE]</span></span>

<span data-ttu-id="59e93-154">Az Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service][AzureAppService].</span><span class="sxs-lookup"><span data-stu-id="59e93-154">For more information about the Azure App Service platform, see [Azure App Service][AzureAppService].</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
