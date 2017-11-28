---
title: "App Service Environment-környezet |} Microsoft Docs"
description: "Mi az az Azure App Service-környezetek? App Service Environment bemutatása."
keywords: "az Azure app service Environment-környezet, virtuális hálózati biztonságos hálózatkezelés"
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 1db5c057-3c56-4537-b580-cdd21fe3f3a7
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: stefsch
ms.openlocfilehash: 1de3f2c513f4f5ce6fd2ab2b57514122955a9a98
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="app-service-environment-documentation"></a><span data-ttu-id="f1313-105">Az App Service-környezet dokumentáció</span><span class="sxs-lookup"><span data-stu-id="f1313-105">App Service Environment Documentation</span></span>
<span data-ttu-id="f1313-106">Az App Service Environment-környezet egy [prémium] [ PremiumTier] terv lehetőségét, amely egy teljesen elkülönített és dedikált környezetben történő biztonságos futtatására, nagy méretekben Azure App Service-alkalmazásokhoz Azure App Service szolgáltatás beleértve [Web Apps][WebApps], [Mobile Apps][MobileApps], és [API-alkalmazások][APIApps].</span><span class="sxs-lookup"><span data-stu-id="f1313-106">An App Service Environment is a [Premium][PremiumTier] service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps at high scale, including [Web Apps][WebApps], [Mobile Apps][MobileApps], and [API Apps][APIApps].</span></span>  

<span data-ttu-id="f1313-107">App Service-környezetek ideálisak igénylő alkalmazások és szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="f1313-107">App Service Environments are ideal for application workloads requiring:</span></span>

* <span data-ttu-id="f1313-108">Nagyon nagy méretű</span><span class="sxs-lookup"><span data-stu-id="f1313-108">Very high scale</span></span>
* <span data-ttu-id="f1313-109">Elkülönítés és a biztonságos hálózati hozzáférést</span><span class="sxs-lookup"><span data-stu-id="f1313-109">Isolation and secure network access</span></span>

<span data-ttu-id="f1313-110">Az ügyfelek több App Service Environment-környezetek belül egyetlen Azure-régió, valamint több Azure-régiók közötti hozhatnak létre.</span><span class="sxs-lookup"><span data-stu-id="f1313-110">Customers can create multiple App Service Environments within a single Azure region, as well as across multiple Azure regions.</span></span>  <span data-ttu-id="f1313-111">Így az App Service Environment-környezetek magas RPS munkaterhelések támogatásához állapot nélküli alkalmazások szintek vízszintesen méretezéshez ideális.</span><span class="sxs-lookup"><span data-stu-id="f1313-111">This makes App Service Environments ideal for horizontally scaling state-less application tiers in support of high RPS workloads.</span></span>

<span data-ttu-id="f1313-112">App Service-környezetek csak az egyetlen ügyfél-alkalmazások futtatása érdekében elkülönített, és mindig telepített virtuális hálózatba.</span><span class="sxs-lookup"><span data-stu-id="f1313-112">App Service Environments are isolated to running only a single customer's applications, and are always deployed into a virtual network.</span></span>  <span data-ttu-id="f1313-113">Az ügyfelek szabályozhassák részletes mindkét bejövő és kimenő hálózati forgalom használó [hálózati biztonsági csoportok][NetworkSecurityGroups].</span><span class="sxs-lookup"><span data-stu-id="f1313-113">Customers have fine-grained control over both inbound and outbound application network traffic using [network security groups][NetworkSecurityGroups].</span></span>  <span data-ttu-id="f1313-114">Alkalmazások is is nagy sebességű biztonságos kapcsolat létrehozása a virtuális hálózatokon keresztül kapcsolódjanak a helyszíni vállalati erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="f1313-114">Applications can also establish high-speed secure connections over virtual networks to on-premises corporate resources.</span></span>

<span data-ttu-id="f1313-115">Alkalmazások gyakran kell hozzáférhet a vállalati erőforrásokhoz, például a belső adatbázisok és webszolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="f1313-115">Apps frequently need to access corporate resources such as internal databases and web services.</span></span>  <span data-ttu-id="f1313-116">App Service Environment-környezetek futó alkalmazások hozzáférhetnek a keresztül elérhető erőforrásokhoz [pont-pont] [ SiteToSite] VPN és [Azure ExpressRoute] [ ExpressRoute] kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="f1313-116">Apps running on App Service Environments can access resources reachable via [Site-to-Site][SiteToSite] VPN and [Azure ExpressRoute][ExpressRoute] connections.</span></span>

* [<span data-ttu-id="f1313-117">Mi az az App Service Environment?</span><span class="sxs-lookup"><span data-stu-id="f1313-117">What is an App Service Environment?</span></span>](../app-service-web/app-service-app-service-environment-intro.md)
* [<span data-ttu-id="f1313-118">App Service Environment környezetek létrehozása</span><span class="sxs-lookup"><span data-stu-id="f1313-118">Creating an App Service Environment</span></span>](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="f1313-119">Alkalmazások létrehozása App Service Environment környezetben</span><span class="sxs-lookup"><span data-stu-id="f1313-119">Creating Apps in an App Service Environment</span></span>](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [<span data-ttu-id="f1313-120">Létrehozásával és a belső Terheléselosztók használatával az App Service-környezetek</span><span class="sxs-lookup"><span data-stu-id="f1313-120">Creating and Using an Internal Load Balancer with App Service Environments</span></span>](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [<span data-ttu-id="f1313-121">Az App Service Environment konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1313-121">Configuring an App Service Environment</span></span>](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [<span data-ttu-id="f1313-122">Alkalmazások skálázása App Service Environmentben</span><span class="sxs-lookup"><span data-stu-id="f1313-122">Scaling Apps in an App Service Environment</span></span>](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [<span data-ttu-id="f1313-123">Hálózatbiztonság és -architektúra</span><span class="sxs-lookup"><span data-stu-id="f1313-123">Network Security and Architecture</span></span>](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a><span data-ttu-id="f1313-124">Hogyan meg</span><span class="sxs-lookup"><span data-stu-id="f1313-124">How To's</span></span>
[!INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]

## <a name="videos"></a><span data-ttu-id="f1313-125">Videók</span><span class="sxs-lookup"><span data-stu-id="f1313-125">Videos</span></span>
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2016/BRK3205/player]

>[!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON325/player]

>[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3715/player]



<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
