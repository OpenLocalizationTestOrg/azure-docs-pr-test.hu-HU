---
title: "Bevezetés az alkalmazás szolgáltatási környezet 1-es verzió"
description: "További tudnivalók az App Service Environment-környezet v1 szolgáltatás, hogy a biztonságos, virtuális hálózatot csatlakoztatott, dedikált méretezési egységek összes, az alkalmazások futtatásához."
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
ms.openlocfilehash: 38cb79eb32bd61cdbfb6da91d50e6713d71a2b0d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="introduction-to-app-service-environment-v1"></a><span data-ttu-id="2105c-103">Bevezetés az alkalmazás szolgáltatási környezet 1-es verzió</span><span class="sxs-lookup"><span data-stu-id="2105c-103">Introduction to App Service Environment v1</span></span>

> [!NOTE]
> <span data-ttu-id="2105c-104">Ez a cikk az App Service Environment-környezet v1 tárgya.</span><span class="sxs-lookup"><span data-stu-id="2105c-104">This article is about the App Service Environment v1.</span></span>  <span data-ttu-id="2105c-105">Az App Service-környezet, amely könnyebben használható, és nagyobb teljesítményű infrastruktúra fut egy újabb verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="2105c-105">There is a newer version of the App Service Environment that is easier  to use and runs on more powerful infrastructure.</span></span> <span data-ttu-id="2105c-106">További információt az új verzió útmutató a [az App Service Environment bemutatása](../app-service/app-service-environment/intro.md).</span><span class="sxs-lookup"><span data-stu-id="2105c-106">To learn more about the new version start with the [Introduction to the App  Service Environment](../app-service/app-service-environment/intro.md).</span></span>
> 

## <a name="overview"></a><span data-ttu-id="2105c-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2105c-107">Overview</span></span>
<span data-ttu-id="2105c-108">Az App Service Environment-környezet egy [prémium] [ PremiumTier] terv lehetőségét, amely egy teljesen elkülönített és dedikált környezetben történő biztonságos futtatására, nagy méretekben Azure App Service-alkalmazásokhoz Azure App Service szolgáltatás beleértve [Web Apps][WebApps], [Mobile Apps][MobileApps], és [API-alkalmazások][APIApps].</span><span class="sxs-lookup"><span data-stu-id="2105c-108">An App Service Environment is a [Premium][PremiumTier] service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps at high scale, including [Web Apps][WebApps], [Mobile Apps][MobileApps], and [API Apps][APIApps].</span></span>  

<span data-ttu-id="2105c-109">App Service-környezetek ideálisak igénylő alkalmazások és szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="2105c-109">App Service Environments are ideal for application workloads requiring:</span></span>

* <span data-ttu-id="2105c-110">Nagyon nagy méretű</span><span class="sxs-lookup"><span data-stu-id="2105c-110">Very high scale</span></span>
* <span data-ttu-id="2105c-111">Elkülönítés és a biztonságos hálózati hozzáférést</span><span class="sxs-lookup"><span data-stu-id="2105c-111">Isolation and secure network access</span></span>

<span data-ttu-id="2105c-112">Az ügyfelek több App Service Environment-környezetek belül egyetlen Azure-régió, valamint több Azure-régiók közötti hozhatnak létre.</span><span class="sxs-lookup"><span data-stu-id="2105c-112">Customers can create multiple App Service Environments within a single Azure region, as well as across multiple Azure regions.</span></span>  <span data-ttu-id="2105c-113">Így az App Service Environment-környezetek magas RPS munkaterhelések támogatásához állapot nélküli alkalmazások szintek vízszintesen méretezéshez ideális.</span><span class="sxs-lookup"><span data-stu-id="2105c-113">This makes App Service Environments ideal for horizontally scaling state-less application tiers in support of high RPS workloads.</span></span>

<span data-ttu-id="2105c-114">App Service-környezetek csak az egyetlen ügyfél-alkalmazások futtatása érdekében elkülönített, és mindig telepített virtuális hálózatba.</span><span class="sxs-lookup"><span data-stu-id="2105c-114">App Service Environments are isolated to running only a single customer's applications, and are always deployed into a virtual network.</span></span>  <span data-ttu-id="2105c-115">Ügyfelek mindkét bejövő és kimenő hálózati forgalom részletes szabályozhatják, és alkalmazások képes kapcsolatot létrehozni a nagy sebességű biztonságos helyszíni vállalati erőforrások virtuális hálózatokon keresztül.</span><span class="sxs-lookup"><span data-stu-id="2105c-115">Customers have fine-grained control over both inbound and outbound application network traffic, and applications can establish high-speed secure connections over virtual networks to on-premises corporate resources.</span></span>

<span data-ttu-id="2105c-116">Az összes cikket, és hogyan-a következőre vonatkozó App Service Environment-környezetek érhetők el a [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="2105c-116">All articles and How-To's about App Service Environments are available in the [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="2105c-117">Megtudhatja, hogyan App Service Environment-környezetek engedélyezése a nagy méretű és a biztonságos hálózati hozzáférés, tekintse meg a [AzureCon mélyreható] [ AzureConDeepDive] az App Service Environment-környezetek!</span><span class="sxs-lookup"><span data-stu-id="2105c-117">For an overview of how App Service Environments enable high scale and secure network access, see the [AzureCon Deep Dive][AzureConDeepDive] on App Service Environments!</span></span>

<span data-ttu-id="2105c-118">Több App Service Environment-környezetek részletesen a vízszintes méretezést a cikk tekintse meg a telepítő hogyan egy [földrajzilag elosztott alkalmazás erőforrásigényét][GeodistributedAppFootprint].</span><span class="sxs-lookup"><span data-stu-id="2105c-118">For a deep-dive on horizontally scaling using multiple App Service Environments see the article on how to setup a [geo-distributed app footprint][GeodistributedAppFootprint].</span></span>

<span data-ttu-id="2105c-119">Hogyan lett konfigurálva a biztonsági architektúrája a AzureCon mélyreható látható, olvassa el a cikk végrehajtási egy [biztonsági architektúrája réteges](app-service-app-service-environment-layered-security.md) az App Service Environment-környezetek.</span><span class="sxs-lookup"><span data-stu-id="2105c-119">To see how the security architecture shown in the AzureCon Deep Dive was configured, see the article on implementing a [layered security architecture](app-service-app-service-environment-layered-security.md) with App Service Environments.</span></span>

<span data-ttu-id="2105c-120">App Service Environment-környezetek futó alkalmazásokra is hozzáférhet a saját felsőbb rétegbeli eszközök, például a webalkalmazási tűzfalak (WAF) által kezdeményezett.</span><span class="sxs-lookup"><span data-stu-id="2105c-120">Apps running on App Service Environments can have their access gated by upstream devices such as web application firewalls (WAF).</span></span>  <span data-ttu-id="2105c-121">A cikk a [egy WAF konfigurálása az App Service Environment-környezetek](app-service-app-service-environment-web-application-firewall.md) ebben a forgatókönyvben ismerteti.</span><span class="sxs-lookup"><span data-stu-id="2105c-121">The article on [configuring a WAF for App Service Environments](app-service-app-service-environment-web-application-firewall.md) covers this scenario.</span></span> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a><span data-ttu-id="2105c-122">Dedikált számítási erőforrásokat</span><span class="sxs-lookup"><span data-stu-id="2105c-122">Dedicated Compute Resources</span></span>
<span data-ttu-id="2105c-123">A számítási erőforrásokat egy App Service Environment-környezetben a kizárólag egyetlen előfizetés számára vannak fenntartva, és az egyetlen alkalmazást az App Service-környezetek konfigurálható legfeljebb ötven (50) számítási erőforrással kizárólagos használatra.</span><span class="sxs-lookup"><span data-stu-id="2105c-123">All of the compute resources in an App Service Environment are dedicated exclusively to a single subscription, and an App Service Environment can be configured with up to fifty (50) compute resources for exclusive use by a single application.</span></span>

<span data-ttu-id="2105c-124">Az App Service-környezetek előtér-számítási erőforráskészlet, valamint egy-három feldolgozókészletek számítási erőforrás áll.</span><span class="sxs-lookup"><span data-stu-id="2105c-124">An App Service Environment is composed of a front-end compute resource pool, as well as one to three worker compute resource pools.</span></span> 

<span data-ttu-id="2105c-125">Az előtér-készlet számítási erőforrásokat is automatikus terheléselosztást app kérelmek belül az App Service-környezetek, SSL-lezárást felelős tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2105c-125">The front-end pool contains compute resources responsible for SSL termination as well automatic load balancing of app requests within an App Service Environment.</span></span> 

<span data-ttu-id="2105c-126">Minden egyes munkavégző készletét tartalmazza a számítási erőforrások vannak rendelve [App Service-csomagok][AppServicePlan], viszont tartalmazó egy vagy több Azure App Service-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="2105c-126">Each worker pool contains compute resources allocated to [App Service Plans][AppServicePlan], which in turn contain one or more Azure App Service apps.</span></span>  <span data-ttu-id="2105c-127">Az App Service-környezetek legfeljebb három különböző feldolgozókészletek lehet, mivel így rugalmasan kiválaszthatja a különböző számítási erőforrásokat az egyes munkavégző készletét.</span><span class="sxs-lookup"><span data-stu-id="2105c-127">Since there can be up to three different worker pools in an App Service Environment, you have the flexibility to choose different compute resources for each worker pool.</span></span>  

<span data-ttu-id="2105c-128">Például így kevésbé hatékony számítási erőforrással egy munkavégző-készlet létrehozása az App Service-csomagok fejlesztési vagy tesztelési alkalmazásokhoz készült.</span><span class="sxs-lookup"><span data-stu-id="2105c-128">For example, this allows you to create one worker pool with less powerful compute resources for App Service Plans intended for development or test apps.</span></span>  <span data-ttu-id="2105c-129">A második (vagy akár harmadik) feldolgozókészletek használhatja az App Service-csomagok futó termelési alkalmazások szánt nagyobb teljesítményű számítási erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="2105c-129">A second (or even third) worker pool could use more powerful compute resources intended for App Service Plans running production apps.</span></span>

<span data-ttu-id="2105c-130">Az előtér- és a feldolgozói készletek elérhető számítási erőforrások mennyisége további részletekért lásd: [az App Service-környezetek konfigurálása][HowToConfigureanAppServiceEnvironment].</span><span class="sxs-lookup"><span data-stu-id="2105c-130">For more details on the quantity of compute resources available to the front-end and worker pools, see [How To Configure an App Service Environment][HowToConfigureanAppServiceEnvironment].</span></span>  

<span data-ttu-id="2105c-131">A rendelkezésre álló számítási erőforrás mérete egy App Service Environment-környezet támogatja a részletekért tekintse meg a [App Service szolgáltatás díjszabása] [ AppServicePricing] lapon, és tekintse át az elérhető lehetőségek a prémium tarifacsomag az App Service-környezetek számára.</span><span class="sxs-lookup"><span data-stu-id="2105c-131">For details on the available compute resource sizes supported in an App Service Environment, consult the [App Service Pricing][AppServicePricing] page and review the available options for App Service Environments in the Premium pricing tier.</span></span>

## <a name="virtual-network-support"></a><span data-ttu-id="2105c-132">Virtuális hálózati támogatása</span><span class="sxs-lookup"><span data-stu-id="2105c-132">Virtual Network Support</span></span>
<span data-ttu-id="2105c-133">Az App Service-környezetek hozhatók létre **vagy** az Azure Resource Manager virtuális hálózati **vagy** klasszikus telepítési modell virtuális hálózat ([további információ a virtuális hálózatok][MoreInfoOnVirtualNetworks]).</span><span class="sxs-lookup"><span data-stu-id="2105c-133">An App Service Environment can be created in **either** an Azure Resource Manager virtual network, **or** a classic deployment model virtual network ([more info on virtual networks][MoreInfoOnVirtualNetworks]).</span></span>  <span data-ttu-id="2105c-134">Mivel az App Service-környezetek mindig szerepel egy virtuális hálózatot, és pontosan egy virtuális hálózati alhálózat, belül kihasználható a biztonsági mindkét bejövő és kimenő hálózati kommunikáció vezérlésére virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="2105c-134">Since an App Service Environment always exists in a virtual network, and more precisely within a subnet of a virtual network, you can leverage the security features of virtual networks to control both inbound and outbound network communications.</span></span>  

<span data-ttu-id="2105c-135">Az App Service Environment-környezet vagy nyilvános IP-címmel, vagy a belső csak Azure belső Load Balancer (ILB) címmel rendelkező irányuló internetes lehet.</span><span class="sxs-lookup"><span data-stu-id="2105c-135">An App Service Environment can be either Internet facing with a public IP address, or internal facing with only an Azure Internal Load Balancer (ILB) address.</span></span>

<span data-ttu-id="2105c-136">Használhat [hálózati biztonsági csoportok] [ NetworkSecurityGroups] korlátozni a bejövő hálózati kommunikáció az alhálózathoz, ahol az App Service-környezetek található.</span><span class="sxs-lookup"><span data-stu-id="2105c-136">You can use [network security groups][NetworkSecurityGroups] to restrict inbound network communications to the subnet where an App Service Environment resides.</span></span>  <span data-ttu-id="2105c-137">Ez lehetővé teszi a felsőbb rétegbeli eszközöket és szolgáltatásokat, például a webalkalmazási tűzfalak és a hálózati Szolgáltatottszoftver-szolgáltatók mögött alkalmazásainak futtatásához.</span><span class="sxs-lookup"><span data-stu-id="2105c-137">This allows you to run apps behind upstream devices and services such as web application firewalls, and network SaaS providers.</span></span>

<span data-ttu-id="2105c-138">Alkalmazások is gyakran kell hozzáférhet a vállalati erőforrásokhoz, például a belső adatbázisok és webszolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="2105c-138">Apps also frequently need to access corporate resources such as internal databases and web services.</span></span>  <span data-ttu-id="2105c-139">Egy gyakori megoldás, hogy elérhetővé tegye ezeket a végpontokat csak a belső hálózati forgalom továbbítására egy Azure virtuális hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="2105c-139">A common approach is to make these endpoints available only to internal network traffic flowing within an Azure virtual network.</span></span>  <span data-ttu-id="2105c-140">A belső szolgáltatásként az azonos virtuális hálózatban egy App Service Environment-környezet része, ha a környezetben futó alkalmazások érhetik el azokat, többek között a végpontok segítségével elérhető [pont-pont] [ SiteToSite] és [Azure ExpressRoute] [ ExpressRoute] kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="2105c-140">Once an App Service Environment is joined to the same virtual network as the internal services, apps running in the environment can access them, including endpoints reachable via [Site-to-Site][SiteToSite] and [Azure ExpressRoute][ExpressRoute] connections.</span></span>

<span data-ttu-id="2105c-141">Az App Service Environment-környezetek virtuális hálózatok és a helyszíni hálózatokban működése vonatkozó részletes információért tekintse meg a következő cikkeket a [hálózati architektúra][NetworkArchitectureOverview], [bejövő forgalom szabályozásának][ControllingInboundTraffic], és [háttérkiszolgálókon való biztonságos kapcsolódás][SecurelyConnectingToBackends].</span><span class="sxs-lookup"><span data-stu-id="2105c-141">For more details on how App Service Environments work with virtual networks and on-premises networks consult the following articles on [Network Architecture][NetworkArchitectureOverview], [Controlling Inbound Traffic][ControllingInboundTraffic], and [Securely Connecting to Backends][SecurelyConnectingToBackends].</span></span> 

## <a name="getting-started"></a><span data-ttu-id="2105c-142">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="2105c-142">Getting started</span></span>
<span data-ttu-id="2105c-143">App Service Environment-környezetek megkezdéséhez, lásd: [hogyan számára hozzon létre egy App Service-környezet][HowToCreateAnAppServiceEnvironment]</span><span class="sxs-lookup"><span data-stu-id="2105c-143">To get started with App Service Environments, see [How To Create An App Service Environment][HowToCreateAnAppServiceEnvironment]</span></span>

<span data-ttu-id="2105c-144">Összes cikket, és hogyan-a következőre az App Service Environment-környezetek érhetők el a [alkalmazásszolgáltatási környezetek – fontos fájl](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="2105c-144">All articles and How-To's for App Service Environments are available in the [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="2105c-145">Az Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service][AzureAppService].</span><span class="sxs-lookup"><span data-stu-id="2105c-145">For more information about the Azure App Service platform, see [Azure App Service][AzureAppService].</span></span>

<span data-ttu-id="2105c-146">Az App Service Environment-környezet hálózati architektúra áttekintése, tekintse meg a [hálózati architektúra áttekintése] [ NetworkArchitectureOverview] cikk.</span><span class="sxs-lookup"><span data-stu-id="2105c-146">For an overview of the App Service Environment network architecture, see the [Network Architecture Overview][NetworkArchitectureOverview] article.</span></span>

<span data-ttu-id="2105c-147">További információ az App Service-környezetek használatáról az ExpressRoute, a következő cikkben: a [Expressroute és az App Service Environment-környezetek][NetworkConfigDetailsForExpressRoute].</span><span class="sxs-lookup"><span data-stu-id="2105c-147">For details on using an App Service Environment with ExpressRoute, see the following article on [Express Route and App Service Environments][NetworkConfigDetailsForExpressRoute].</span></span>

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


