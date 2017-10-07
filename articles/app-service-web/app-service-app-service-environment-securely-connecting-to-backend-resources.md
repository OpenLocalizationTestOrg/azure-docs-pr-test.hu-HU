---
title: "aaaSecurely csatlakozás tooBackEnd erőforrásokhoz bárhonnan, az App Service-környezetek"
description: "További információk a hogyan toosecurely kapcsolódó toobackend erőforrások az App Service Environment-környezet."
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
ms.openlocfilehash: 6311d3fc301512ea3c4ed8f14f268f75755aa415
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securely-connecting-toobackend-resources-from-an-app-service-environment"></a><span data-ttu-id="77b5a-103">Biztonságosan csatlakozás tooBackend erőforrásokhoz bárhonnan, az App Service-környezetek</span><span class="sxs-lookup"><span data-stu-id="77b5a-103">Securely Connecting tooBackend Resources from an App Service Environment</span></span>
## <a name="overview"></a><span data-ttu-id="77b5a-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="77b5a-104">Overview</span></span>
<span data-ttu-id="77b5a-105">Mivel az App Service-környezetek mindig jön létre **vagy** az Azure Resource Manager virtuális hálózati **vagy** klasszikus telepítési modell [virtuális hálózati] [ virtualnetwork], az App Service Environment-környezet tooother háttér erőforrásokból kimenő kapcsolatok áramolhasson kizárólag hello virtuális hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="77b5a-105">Since an App Service Environment is always created in **either** an Azure Resource Manager virtual network, **or** a classic deployment model [virtual network][virtualnetwork], outbound connections from an App Service Environment tooother backend resources can flow exclusively over hello virtual network.</span></span>  <span data-ttu-id="77b5a-106">2016. június végzett legutóbbi módosítását ASEs is is telepíthető az nyilvános címtartományt, vagy RFC1918 címterek (azaz Magáncímeket) használó virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="77b5a-106">With a recent change made in June 2016, ASEs can also be deployed into virtual networks that use either public address ranges, or RFC1918 address spaces (i.e. private addresses).</span></span>  

<span data-ttu-id="77b5a-107">Előfordulhat például, a virtuális gépek zárolva 1433-as port a fürtön futó SQL Server.</span><span class="sxs-lookup"><span data-stu-id="77b5a-107">For example, there may be a SQL Server running on a cluster of virtual machines with port 1433 locked down.</span></span>  <span data-ttu-id="77b5a-108">lehet, hogy hello végpont tooonly egyéb erőforrásokhoz való hozzáférés engedélyezése a ACLd hello ugyanazt a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="77b5a-108">hello endpoint may be ACLd tooonly allow access from other resources on hello same virtual network.</span></span>  

<span data-ttu-id="77b5a-109">Másik példaként, bizalmas végpontok a helyi, és akár keresztül csatlakoztatott tooAzure kell [pont-pont] [ SiteToSite] vagy [Azure ExpressRoute] [ ExpressRoute] kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="77b5a-109">As another example, sensitive endpoints may run on-premises and be connected tooAzure via either [Site-to-Site][SiteToSite] or [Azure ExpressRoute][ExpressRoute] connections.</span></span>  <span data-ttu-id="77b5a-110">Ennek eredményeképpen csak azokat az erőforrásokat a virtuális hálózatok csatlakoztatva toohello pont-pont, vagy ExpressRoute-alagutak lesz képes tooaccess helyszíni végpontok.</span><span class="sxs-lookup"><span data-stu-id="77b5a-110">As a result, only resources in virtual networks connected toohello Site-to-Site or ExpressRoute tunnels will be able tooaccess on-premises endpoints.</span></span>

<span data-ttu-id="77b5a-111">Az összes forgatókönyvekben az App Service-környezetek futó alkalmazások kell képes toosecurely csatlakoztassa toohello különböző kiszolgálókon és erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="77b5a-111">For all of these scenarios, apps running on an App Service Environment will be able toosecurely connect toohello various servers and resources.</span></span>  <span data-ttu-id="77b5a-112">Egy App Service Environment-környezet tooprivate végpontok hello a futó alkalmazásokból kimenő forgalom ugyanazt a virtuális hálózatot (vagy toohello csatlakoztatva azonos virtuális hálózatban), csak a folyamat fog hello virtuális hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="77b5a-112">Outbound traffic from apps running in an App Service Environment tooprivate endpoints in hello same virtual network (or connected toohello same virtual network), will only flow over hello virtual network.</span></span>  <span data-ttu-id="77b5a-113">Kimenő forgalom tooprivate végpontok nem halad keresztül hello a nyilvános internethez.</span><span class="sxs-lookup"><span data-stu-id="77b5a-113">Outbound traffic tooprivate endpoints will not flow over hello public Internet.</span></span>

<span data-ttu-id="77b5a-114">Egy szerint toooutbound forgalmat egy App Service Environment-környezet tooendpoints a virtuális hálózaton belül érvényes.</span><span class="sxs-lookup"><span data-stu-id="77b5a-114">One caveat applies toooutbound traffic from an App Service Environment tooendpoints within a virtual network.</span></span>  <span data-ttu-id="77b5a-115">App Service Environment-környezetek nem érhető el a végpontok hello található virtuális gépek **azonos** alhálózat szerint hello App Service Environment-környezet.</span><span class="sxs-lookup"><span data-stu-id="77b5a-115">App Service Environments cannot reach endpoints of virtual machines located in hello **same** subnet as hello App Service Environment.</span></span>  <span data-ttu-id="77b5a-116">Általában ne legyen hiba, amíg az App Service Environment-környezetek telepít egy alhálózatba csak hello App Service Environment-környezet által kizárólagos használatra fenntartva.</span><span class="sxs-lookup"><span data-stu-id="77b5a-116">This normally should not be a problem as long as App Service Environments are deployed into a subnet reserved for exclusive use by only hello App Service Environment.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a><span data-ttu-id="77b5a-117">Kimenő kapcsolatok és a DNS-követelmények</span><span class="sxs-lookup"><span data-stu-id="77b5a-117">Outbound Connectivity and DNS Requirements</span></span>
<span data-ttu-id="77b5a-118">Az egy App Service Environment-környezet toofunction megfelelően, akkor csak kimenő hozzáférést toovarious végpontok.</span><span class="sxs-lookup"><span data-stu-id="77b5a-118">For an App Service Environment toofunction properly, it requires outbound access toovarious endpoints.</span></span> <span data-ttu-id="77b5a-119">Hello külső végpontok egy ASE által használt teljes listája megtalálható-e hello hello "A hálózati kapcsolat szükséges" szakasza [ExpressRoute tartozó fürthálózat-konfiguráció](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) cikk.</span><span class="sxs-lookup"><span data-stu-id="77b5a-119">A full list of hello external endpoints used by an ASE is in hello "Required Network Connectivity" section of hello [Network Configuration for ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) article.</span></span>

<span data-ttu-id="77b5a-120">App Service-környezetek hello virtuális hálózat egy érvényes DNS-infrastruktúra szükséges.</span><span class="sxs-lookup"><span data-stu-id="77b5a-120">App Service Environments require a valid DNS infrastructure configured for hello virtual network.</span></span>  <span data-ttu-id="77b5a-121">Ha bármilyen okból hello DNS-konfiguráció esetén megváltozik egy App Service Environment-környezet létrehozása után, a fejlesztők kényszerítheti az App Service Environment-környezet toopick hello új DNS-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="77b5a-121">If for any reason hello DNS configuration is changed after an App Service Environment has been created, developers can force an App Service Environment toopick up hello new DNS configuration.</span></span>  <span data-ttu-id="77b5a-122">Hello "Restart" ikonnal hello App Service Environment-környezet hello tetején található működés közbeni környezet újraindítás kiváltó felügyelet panel hello portálon hatására hello környezet toopick hello új DNS-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="77b5a-122">Triggering a rolling environment reboot using hello "Restart" icon located at hello top of hello App Service Environment management blade in hello portal will cause hello environment toopick up hello new DNS configuration.</span></span>

<span data-ttu-id="77b5a-123">Ajánlott továbbá, hogy egyéni DNS-kiszolgálók hello a virtuális hálózaton kell előre idő előzetes toocreating az App Service-környezetek beállítása.</span><span class="sxs-lookup"><span data-stu-id="77b5a-123">It is also recommended that any custom DNS servers on hello vnet be setup ahead of time prior toocreating an App Service Environment.</span></span>  <span data-ttu-id="77b5a-124">Ha egy virtuális hálózat DNS-konfiguráció megváltozott egy App Service Environment-környezet létrehozása közben, hello App Service Environment-környezet létrehozása folyamat sikertelen vezethet.</span><span class="sxs-lookup"><span data-stu-id="77b5a-124">If a virtual network's DNS configuration is changed while an App Service Environment is being created, that will result in hello App Service Environment creation process failing.</span></span>  <span data-ttu-id="77b5a-125">Egy hasonló tekintettel az egyéni DNS-kiszolgáló létezik a hello másik végén egy VPN gateway és hello DNS-kiszolgáló esetén nem érhető el vagy nem érhető el, hello App Service Environment-környezet létrehozása is sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="77b5a-125">In a similar vein, if a custom DNS server exists on hello other end of a VPN gateway, and hello DNS server is unreachable or unavailable, hello App Service Environment creation process will also fail.</span></span>

## <a name="connecting-tooa-sql-server"></a><span data-ttu-id="77b5a-126">Kapcsolódás az SQL Server tooa</span><span class="sxs-lookup"><span data-stu-id="77b5a-126">Connecting tooa SQL Server</span></span>
<span data-ttu-id="77b5a-127">Egy közös SQL Server-konfigurációs az 1433-as portot figyelő végponttal rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="77b5a-127">A common SQL Server configuration has an endpoint listening on port 1433:</span></span>

![SQL Server-végpont][SqlServerEndpoint]

<span data-ttu-id="77b5a-129">Kétféleképpen a forgalom toothis végpont korlátozása:</span><span class="sxs-lookup"><span data-stu-id="77b5a-129">There are two approaches for restricting traffic toothis endpoint:</span></span>

* <span data-ttu-id="77b5a-130">[Hálózati hozzáférés-vezérlési listák] [ NetworkAccessControlLists] (hálózati hozzáférés-vezérlési listák)</span><span class="sxs-lookup"><span data-stu-id="77b5a-130">[Network Access Control Lists][NetworkAccessControlLists] (Network ACLs)</span></span>
* <span data-ttu-id="77b5a-131">[Hálózati biztonsági csoportok][NetworkSecurityGroups]</span><span class="sxs-lookup"><span data-stu-id="77b5a-131">[Network Security Groups][NetworkSecurityGroups]</span></span>

## <a name="restricting-access-with-a-network-acl"></a><span data-ttu-id="77b5a-132">A hálózati hozzáférés-vezérlési lista a hozzáférés korlátozása</span><span class="sxs-lookup"><span data-stu-id="77b5a-132">Restricting Access With a Network ACL</span></span>
<span data-ttu-id="77b5a-133">1433-as port a hálózati hozzáférés-vezérlési lista segítségével biztosítható.</span><span class="sxs-lookup"><span data-stu-id="77b5a-133">Port 1433 can be secured using a network access control list.</span></span>  <span data-ttu-id="77b5a-134">whitelists ügyfél az alábbi hello példában címek egy virtuális hálózaton belül származó, és blokkolja hozzáférést tooall más ügyfelekkel.</span><span class="sxs-lookup"><span data-stu-id="77b5a-134">hello example below whitelists client addresses originating from inside of a virtual network, and blocks access tooall other clients.</span></span>

![Hálózati hozzáférés vezérlő példa][NetworkAccessControlListExample]

<span data-ttu-id="77b5a-136">A App Service Environment-környezetben futó alkalmazások hello ugyanazt a virtuális hálózatot, mert hello SQL Server lesz képes tooconnect toohello SQL Server-példány használatával hello **belső hálózatok** hello SQL Server virtuális gép IP-címet.</span><span class="sxs-lookup"><span data-stu-id="77b5a-136">Any applications running in App Service Environment in hello same virtual network as hello SQL Server will be able tooconnect toohello SQL Server instance using hello **VNet internal** IP address for hello SQL Server virtual machine.</span></span>  

<span data-ttu-id="77b5a-137">hello példa kapcsolati karakterlánc hivatkozás alatti hello SQL Server Privát IP-címére.</span><span class="sxs-lookup"><span data-stu-id="77b5a-137">hello example connection string below references hello SQL Server using its private IP address.</span></span>

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

<span data-ttu-id="77b5a-138">Bár a hello virtuális gépen van egy nyilvános végpontot, hello nyilvános IP-cím használatával történő kapcsolódási kísérletek elutasításra hello hálózati hozzáférés-vezérlési lista miatt.</span><span class="sxs-lookup"><span data-stu-id="77b5a-138">Although hello virtual machine has a public endpoint as well, connection attempts using hello public IP address will be rejected because of hello network ACL.</span></span> 

## <a name="restricting-access-with-a-network-security-group"></a><span data-ttu-id="77b5a-139">Hálózati biztonsági csoport a hozzáférés korlátozása</span><span class="sxs-lookup"><span data-stu-id="77b5a-139">Restricting Access With a Network Security Group</span></span>
<span data-ttu-id="77b5a-140">Hálózati biztonsági csoport van egy másik módjáról hozzáférés biztosítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="77b5a-140">An alternative approach for securing access is with a network security group.</span></span>  <span data-ttu-id="77b5a-141">Hálózati biztonsági csoportok alkalmazott tooindividual virtuális gépek vagy virtuális gépeket tartalmazó tooa alhálózati lehet.</span><span class="sxs-lookup"><span data-stu-id="77b5a-141">Network security groups can be applied tooindividual virtual machines, or tooa subnet containing virtual machines.</span></span>

<span data-ttu-id="77b5a-142">Hálózati biztonsági csoport először létre toobe van szüksége:</span><span class="sxs-lookup"><span data-stu-id="77b5a-142">First a network security group needs toobe created:</span></span>

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

<span data-ttu-id="77b5a-143">Hozzáférés tooonly virtuális hálózat belső forgalom korlátozása a hálózati biztonsági csoport nagyon egyszerű.</span><span class="sxs-lookup"><span data-stu-id="77b5a-143">Restricting access tooonly VNet internal traffic is very simple with a network security group.</span></span>  <span data-ttu-id="77b5a-144">hello alapértelmezett szabályok a hálózati biztonsági csoport csak engedélyezik a hozzáférést a többi hálózati ügyfelet hello ugyanazt a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="77b5a-144">hello default rules in a network security group only allow access from other network clients in hello same virtual network.</span></span>

<span data-ttu-id="77b5a-145">Emiatt a hozzáférés tooSQL zárolása-kiszolgáló egy más dolga, mint az alapértelmezett szabályok tooeither hello futó virtuális gépek az SQL Server vagy hello virtuális gépeket tartalmazó hello alhálózatot a hálózati biztonsági csoport alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="77b5a-145">As a result locking down access tooSQL Server is as simple as applying a network security group with its default rules tooeither hello virtual machines running SQL Server, or hello subnet containing hello virtual machines.</span></span>

<span data-ttu-id="77b5a-146">az alábbi hello minta hálózati biztonsági csoport toohello tartalmazó alhálózat vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="77b5a-146">hello sample below applies a network security group toohello containing subnet:</span></span>

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'

<span data-ttu-id="77b5a-147">hello végeredménynek olyan szabályokat, amelyek külső hozzáférés, miközben lehetővé teszi a virtuális hálózat belső hozzáférés letiltása:</span><span class="sxs-lookup"><span data-stu-id="77b5a-147">hello end result is a set of security rules that block external access, while allowing VNet internal access:</span></span>

![Alapértelmezett hálózati biztonsági szabályok][DefaultNetworkSecurityRules]

## <a name="getting-started"></a><span data-ttu-id="77b5a-149">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="77b5a-149">Getting started</span></span>
<span data-ttu-id="77b5a-150">Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="77b5a-150">All articles and How-To's for App Service Environments are available in hello [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="77b5a-151">Lásd az App Service-környezetek lépései tooget [bemutatása tooApp Service-környezet][IntroToAppServiceEnvironment]</span><span class="sxs-lookup"><span data-stu-id="77b5a-151">tooget started with App Service Environments, see [Introduction tooApp Service Environment][IntroToAppServiceEnvironment]</span></span>

<span data-ttu-id="77b5a-152">Ellenőrző bejövő forgalom tooyour App Service Environment-környezet körül, lásd: [szabályozása a bejövő forgalom tooan App Service Environment-környezet][ControlInboundASE]</span><span class="sxs-lookup"><span data-stu-id="77b5a-152">For details around controlling inbound traffic tooyour App Service Environment, see [Controlling inbound traffic tooan App Service Environment][ControlInboundASE]</span></span>

<span data-ttu-id="77b5a-153">Hello Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service][AzureAppService].</span><span class="sxs-lookup"><span data-stu-id="77b5a-153">For more information about hello Azure App Service platform, see [Azure App Service][AzureAppService].</span></span>

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
