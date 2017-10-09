---
title: "aaaIntroduction tooApp Service-környezet 1-es verzió"
description: "Tudnivalók az App Service Environment-környezet v1 hello szolgáltatás, hogy a biztonságos, virtuális hálózatot csatlakoztatott, dedikált méretezési egységek az összes az alkalmazások futtatásához."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 78e6d4f5-da46-4eb5-a632-b5fdc17d2394
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: 6e3cd1909b241887b5ec19412b9f7884d870cc3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooapp-service-environment-v1"></a><span data-ttu-id="4c503-103">Bevezetés tooApp Service-környezet 1-es verzió</span><span class="sxs-lookup"><span data-stu-id="4c503-103">Introduction tooApp Service Environment v1</span></span>

> [!NOTE]
> <span data-ttu-id="4c503-104">Ez a cikk hello App Service Environment-környezet v1 tárgya.</span><span class="sxs-lookup"><span data-stu-id="4c503-104">This article is about hello App Service Environment v1.</span></span>  <span data-ttu-id="4c503-105">App Service-környezet, amely könnyebben toouse, és nagyobb teljesítményű infrastruktúra futó hello újabb verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="4c503-105">There is a newer version of hello App Service Environment that is easier  toouse and runs on more powerful infrastructure.</span></span> <span data-ttu-id="4c503-106">több hello új verzióval kapcsolatos kezdődnie hello toolearn [bemutatása toohello App Service Environment-környezet](../app-service/app-service-environment/intro.md).</span><span class="sxs-lookup"><span data-stu-id="4c503-106">toolearn more about hello new version start with hello [Introduction toohello App  Service Environment](../app-service/app-service-environment/intro.md).</span></span>
> 

## <a name="overview"></a><span data-ttu-id="4c503-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4c503-107">Overview</span></span>
<span data-ttu-id="4c503-108">Az App Service Environment-környezet egy [prémium] [ PremiumTier] terv lehetőségét, amely egy teljesen elkülönített és dedikált környezetben történő biztonságos futtatására, nagy méretekben Azure App Service-alkalmazásokhoz Azure App Service szolgáltatás beleértve [Web Apps][WebApps], [Mobile Apps][MobileApps], és [API-alkalmazások][APIApps].</span><span class="sxs-lookup"><span data-stu-id="4c503-108">An App Service Environment is a [Premium][PremiumTier] service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps at high scale, including [Web Apps][WebApps], [Mobile Apps][MobileApps], and [API Apps][APIApps].</span></span>  

<span data-ttu-id="4c503-109">App Service-környezetek ideálisak igénylő alkalmazások és szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="4c503-109">App Service Environments are ideal for application workloads requiring:</span></span>

* <span data-ttu-id="4c503-110">Nagyon nagy méretű</span><span class="sxs-lookup"><span data-stu-id="4c503-110">Very high scale</span></span>
* <span data-ttu-id="4c503-111">Elkülönítés és a biztonságos hálózati hozzáférést</span><span class="sxs-lookup"><span data-stu-id="4c503-111">Isolation and secure network access</span></span>

<span data-ttu-id="4c503-112">Az ügyfelek több App Service Environment-környezetek belül egyetlen Azure-régió, valamint több Azure-régiók közötti hozhatnak létre.</span><span class="sxs-lookup"><span data-stu-id="4c503-112">Customers can create multiple App Service Environments within a single Azure region, as well as across multiple Azure regions.</span></span>  <span data-ttu-id="4c503-113">Így az App Service Environment-környezetek magas RPS munkaterhelések támogatásához állapot nélküli alkalmazások szintek vízszintesen méretezéshez ideális.</span><span class="sxs-lookup"><span data-stu-id="4c503-113">This makes App Service Environments ideal for horizontally scaling state-less application tiers in support of high RPS workloads.</span></span>

<span data-ttu-id="4c503-114">App Service-környezetek elkülönített toorunning csak egyetlen vevő alkalmazások, és mindig telepített virtuális hálózatba.</span><span class="sxs-lookup"><span data-stu-id="4c503-114">App Service Environments are isolated toorunning only a single customer's applications, and are always deployed into a virtual network.</span></span>  <span data-ttu-id="4c503-115">Ügyfelek mindkét bejövő és kimenő hálózati forgalom részletes szabályozhatják, és alkalmazások képes kapcsolatot létrehozni a nagy sebességű biztonságos virtuális hálózatok tooon helyszíni vállalati erőforrások keresztül.</span><span class="sxs-lookup"><span data-stu-id="4c503-115">Customers have fine-grained control over both inbound and outbound application network traffic, and applications can establish high-speed secure connections over virtual networks tooon-premises corporate resources.</span></span>

<span data-ttu-id="4c503-116">Összes cikket, és hogyan-a következőre vonatkozó App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="4c503-116">All articles and How-To's about App Service Environments are available in hello [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="4c503-117">Megtudhatja, hogyan App Service Environment-környezetek engedélyezése a nagy méretű és a biztonságos hálózati hozzáférés, lásd: hello [AzureCon mélyreható] [ AzureConDeepDive] az App Service Environment-környezetek!</span><span class="sxs-lookup"><span data-stu-id="4c503-117">For an overview of how App Service Environments enable high scale and secure network access, see hello [AzureCon Deep Dive][AzureConDeepDive] on App Service Environments!</span></span>

<span data-ttu-id="4c503-118">Egy részletesen a vízszintes méretezést több App Service Environment-környezetek cikke hello hogyan toosetup egy [földrajzilag elosztott alkalmazás erőforrásigényét][GeodistributedAppFootprint].</span><span class="sxs-lookup"><span data-stu-id="4c503-118">For a deep-dive on horizontally scaling using multiple App Service Environments see hello article on how toosetup a [geo-distributed app footprint][GeodistributedAppFootprint].</span></span>

<span data-ttu-id="4c503-119">Hogyan hello biztonsági architektúrája látható hello AzureCon mélyreható lett konfigurálva, toosee cikke hello megvalósításáról a [biztonsági architektúrája réteges](app-service-app-service-environment-layered-security.md) az App Service Environment-környezetek.</span><span class="sxs-lookup"><span data-stu-id="4c503-119">toosee how hello security architecture shown in hello AzureCon Deep Dive was configured, see hello article on implementing a [layered security architecture](app-service-app-service-environment-layered-security.md) with App Service Environments.</span></span>

<span data-ttu-id="4c503-120">App Service Environment-környezetek futó alkalmazásokra is hozzáférhet a saját felsőbb rétegbeli eszközök, például a webalkalmazási tűzfalak (WAF) által kezdeményezett.</span><span class="sxs-lookup"><span data-stu-id="4c503-120">Apps running on App Service Environments can have their access gated by upstream devices such as web application firewalls (WAF).</span></span>  <span data-ttu-id="4c503-121">hello foglalkozó [egy WAF konfigurálása az App Service Environment-környezetek](app-service-app-service-environment-web-application-firewall.md) ebben a forgatókönyvben ismerteti.</span><span class="sxs-lookup"><span data-stu-id="4c503-121">hello article on [configuring a WAF for App Service Environments](app-service-app-service-environment-web-application-firewall.md) covers this scenario.</span></span> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a><span data-ttu-id="4c503-122">Dedikált számítási erőforrásokat</span><span class="sxs-lookup"><span data-stu-id="4c503-122">Dedicated Compute Resources</span></span>
<span data-ttu-id="4c503-123">Kizárólag tooa egyetlen előfizetés dedikált összes hello számítási erőforrások egy App Service Environment-környezetben, és az App Service-környezetek konfigurálható toofifty (50) számítási erőforrások kizárólagos használatra egy bizonyos alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4c503-123">All of hello compute resources in an App Service Environment are dedicated exclusively tooa single subscription, and an App Service Environment can be configured with up toofifty (50) compute resources for exclusive use by a single application.</span></span>

<span data-ttu-id="4c503-124">Az App Service-környezetek előtér-számítási erőforráskészlet, valamint egy toothree feldolgozókészletek számítási erőforrás áll.</span><span class="sxs-lookup"><span data-stu-id="4c503-124">An App Service Environment is composed of a front-end compute resource pool, as well as one toothree worker compute resource pools.</span></span> 

<span data-ttu-id="4c503-125">hello előtér-készlet számítási erőforrásokat is automatikus terheléselosztást app kérelmek belül az App Service-környezetek, SSL-lezárást felelős tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4c503-125">hello front-end pool contains compute resources responsible for SSL termination as well automatic load balancing of app requests within an App Service Environment.</span></span> 

<span data-ttu-id="4c503-126">Minden egyes munkavégző készletét tartalmazza túl lefoglalt számítási erőforrásokat[App Service-csomagok][AppServicePlan], viszont tartalmazó egy vagy több Azure App Service-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="4c503-126">Each worker pool contains compute resources allocated too[App Service Plans][AppServicePlan], which in turn contain one or more Azure App Service apps.</span></span>  <span data-ttu-id="4c503-127">Lehet toothree különböző feldolgozókészletek fel egy App Service Environment-környezetben, mert rendelkezik hello rugalmasságot toochoose különböző számítási erőforrásokat az egyes munkavégző készletét.</span><span class="sxs-lookup"><span data-stu-id="4c503-127">Since there can be up toothree different worker pools in an App Service Environment, you have hello flexibility toochoose different compute resources for each worker pool.</span></span>  

<span data-ttu-id="4c503-128">Például így toocreate egy feldolgozókészletek kevésbé hatékony számítási erőforrással az App Service-csomagok fejlesztési vagy tesztelési alkalmazásokhoz készült.</span><span class="sxs-lookup"><span data-stu-id="4c503-128">For example, this allows you toocreate one worker pool with less powerful compute resources for App Service Plans intended for development or test apps.</span></span>  <span data-ttu-id="4c503-129">A második (vagy akár harmadik) feldolgozókészletek használhatja az App Service-csomagok futó termelési alkalmazások szánt nagyobb teljesítményű számítási erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="4c503-129">A second (or even third) worker pool could use more powerful compute resources intended for App Service Plans running production apps.</span></span>

<span data-ttu-id="4c503-130">A számítási erőforrások elérhető toohello előtér- és feldolgozókészletek hello mennyisége további részletekért lásd: [hogyan tooConfigure az App Service-környezetek][HowToConfigureanAppServiceEnvironment].</span><span class="sxs-lookup"><span data-stu-id="4c503-130">For more details on hello quantity of compute resources available toohello front-end and worker pools, see [How tooConfigure an App Service Environment][HowToConfigureanAppServiceEnvironment].</span></span>  

<span data-ttu-id="4c503-131">A rendelkezésre álló hello részleteinek számítási erőforrás mérete egy App Service Environment-környezet támogatja, olvassa el a hello [App Service szolgáltatás díjszabása] [ AppServicePricing] lapon, és tekintse át az App Service Environment-környezetek hello lehetőségek a hello prémium tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="4c503-131">For details on hello available compute resource sizes supported in an App Service Environment, consult hello [App Service Pricing][AppServicePricing] page and review hello available options for App Service Environments in hello Premium pricing tier.</span></span>

## <a name="virtual-network-support"></a><span data-ttu-id="4c503-132">Virtuális hálózati támogatása</span><span class="sxs-lookup"><span data-stu-id="4c503-132">Virtual Network Support</span></span>
<span data-ttu-id="4c503-133">Az App Service-környezetek hozhatók létre **vagy** az Azure Resource Manager virtuális hálózati **vagy** klasszikus telepítési modell virtuális hálózat ([további információ a virtuális hálózatok][MoreInfoOnVirtualNetworks]).</span><span class="sxs-lookup"><span data-stu-id="4c503-133">An App Service Environment can be created in **either** an Azure Resource Manager virtual network, **or** a classic deployment model virtual network ([more info on virtual networks][MoreInfoOnVirtualNetworks]).</span></span>  <span data-ttu-id="4c503-134">Mivel az App Service-környezetek mindig szerepel egy virtuális hálózatot, és pontosan egy virtuális hálózati alhálózat, belül kihasználható hello biztonsági a virtuális hálózatok toocontrol mindkét bejövő és kimenő hálózati kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="4c503-134">Since an App Service Environment always exists in a virtual network, and more precisely within a subnet of a virtual network, you can leverage hello security features of virtual networks toocontrol both inbound and outbound network communications.</span></span>  

<span data-ttu-id="4c503-135">Az App Service Environment-környezet vagy nyilvános IP-címmel, vagy a belső csak Azure belső Load Balancer (ILB) címmel rendelkező irányuló internetes lehet.</span><span class="sxs-lookup"><span data-stu-id="4c503-135">An App Service Environment can be either Internet facing with a public IP address, or internal facing with only an Azure Internal Load Balancer (ILB) address.</span></span>

<span data-ttu-id="4c503-136">Használhat [hálózati biztonsági csoportok] [ NetworkSecurityGroups] toorestrict bejövő hálózati kommunikációs toohello alhálózat egy App Service Environment-környezet tároló.</span><span class="sxs-lookup"><span data-stu-id="4c503-136">You can use [network security groups][NetworkSecurityGroups] toorestrict inbound network communications toohello subnet where an App Service Environment resides.</span></span>  <span data-ttu-id="4c503-137">Ez lehetővé teszi toorun alkalmazások mögött fölérendelt eszközöket és szolgáltatásokat, például a webalkalmazási tűzfalak és a hálózati Szolgáltatottszoftver-szolgáltatók.</span><span class="sxs-lookup"><span data-stu-id="4c503-137">This allows you toorun apps behind upstream devices and services such as web application firewalls, and network SaaS providers.</span></span>

<span data-ttu-id="4c503-138">Alkalmazások is gyakran kell tooaccess vállalati erőforrások, például belső adatbázisok és a web services.</span><span class="sxs-lookup"><span data-stu-id="4c503-138">Apps also frequently need tooaccess corporate resources such as internal databases and web services.</span></span>  <span data-ttu-id="4c503-139">Általános gyakorlatként javasolt toomake ezek végpontok elérhető csak toointernal hálózati forgalom továbbítására egy Azure virtuális hálózaton belül van.</span><span class="sxs-lookup"><span data-stu-id="4c503-139">A common approach is toomake these endpoints available only toointernal network traffic flowing within an Azure virtual network.</span></span>  <span data-ttu-id="4c503-140">Amikor egy App Service Environment-környezet illesztett toohello hello belső szolgáltatások, hello környezetben futó alkalmazások megegyező virtuális hálózatban érhetik el azokat, beleértve a végpontok segítségével elérhető [pont-pont] [ SiteToSite]és [Azure ExpressRoute] [ ExpressRoute] kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="4c503-140">Once an App Service Environment is joined toohello same virtual network as hello internal services, apps running in hello environment can access them, including endpoints reachable via [Site-to-Site][SiteToSite] and [Azure ExpressRoute][ExpressRoute] connections.</span></span>

<span data-ttu-id="4c503-141">Az App Service Environment-környezetek virtuális hálózatok és a helyszíni hálózatokban működéséről további részleteket tekintse meg a következő cikkeket hello [hálózati architektúra][NetworkArchitectureOverview], [bejövő vezérlése Forgalom][ControllingInboundTraffic], és [biztonságos kapcsolódás tooBackends][SecurelyConnectingToBackends].</span><span class="sxs-lookup"><span data-stu-id="4c503-141">For more details on how App Service Environments work with virtual networks and on-premises networks consult hello following articles on [Network Architecture][NetworkArchitectureOverview], [Controlling Inbound Traffic][ControllingInboundTraffic], and [Securely Connecting tooBackends][SecurelyConnectingToBackends].</span></span> 

## <a name="getting-started"></a><span data-ttu-id="4c503-142">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="4c503-142">Getting started</span></span>
<span data-ttu-id="4c503-143">Lásd az App Service-környezetek lépései tooget [hogyan tooCreate egy App Service Environment-környezet][HowToCreateAnAppServiceEnvironment]</span><span class="sxs-lookup"><span data-stu-id="4c503-143">tooget started with App Service Environments, see [How tooCreate An App Service Environment][HowToCreateAnAppServiceEnvironment]</span></span>

<span data-ttu-id="4c503-144">Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el hello [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="4c503-144">All articles and How-To's for App Service Environments are available in hello [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="4c503-145">Hello Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service][AzureAppService].</span><span class="sxs-lookup"><span data-stu-id="4c503-145">For more information about hello Azure App Service platform, see [Azure App Service][AzureAppService].</span></span>

<span data-ttu-id="4c503-146">App Service Environment-környezet hálózati architektúra hello áttekintését lásd: hello [hálózati architektúra áttekintése] [ NetworkArchitectureOverview] cikk.</span><span class="sxs-lookup"><span data-stu-id="4c503-146">For an overview of hello App Service Environment network architecture, see hello [Network Architecture Overview][NetworkArchitectureOverview] article.</span></span>

<span data-ttu-id="4c503-147">További információ az App Service-környezetek használatáról az expressroute-nál: következő cikket hello [Expressroute és az App Service Environment-környezetek][NetworkConfigDetailsForExpressRoute].</span><span class="sxs-lookup"><span data-stu-id="4c503-147">For details on using an App Service Environment with ExpressRoute, see hello following article on [Express Route and App Service Environments][NetworkConfigDetailsForExpressRoute].</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->


