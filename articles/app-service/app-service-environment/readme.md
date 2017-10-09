---
title: "App Service Environment-környezet információs aaaAzure"
description: "Listák hello dokumentáció, és ismerteti az Azure App Service Environment-környezet"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 77452413-5193-4762-8b3d-5fa8e4edf1ca
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 6edc74804ded7497e70c31c9e08252257add4415
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-environment-documentation"></a><span data-ttu-id="626a8-103">App Service-környezet dokumentáció</span><span class="sxs-lookup"><span data-stu-id="626a8-103">App Service environment documentation</span></span>
 <span data-ttu-id="626a8-104">Az Azure App Service Environment-környezet az Azure App Service szolgáltatása, amely egy teljesen elkülönített és dedikált környezetben történő biztonságos futtatására, nagy méretekben App Service-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="626a8-104">Azure App Service Environment is an Azure App Service feature that provides a fully isolated and dedicated environment for securely running App Service apps at high scale.</span></span> <span data-ttu-id="626a8-105">Ezzel a funkcióval rendelkezhet az [webalkalmazások][webapps], [mobilalkalmazások][mobileapps], [API-alkalmazások][APIApps], és [funkciók][Functions].</span><span class="sxs-lookup"><span data-stu-id="626a8-105">This capability can host your [web apps][webapps], [mobile apps][mobileapps], [API apps][APIApps], and [functions][Functions].</span></span>

<span data-ttu-id="626a8-106">App Service-környezetek (ASEs) ideálisak igénylő alkalmazások és szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="626a8-106">App Service environments (ASEs) are ideal for application workloads that require:</span></span>

* <span data-ttu-id="626a8-107">Nagyon nagy méretű.</span><span class="sxs-lookup"><span data-stu-id="626a8-107">Very high scale.</span></span>
* <span data-ttu-id="626a8-108">Elkülönítés és a biztonságos hálózati hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="626a8-108">Isolation and secure network access.</span></span>

<span data-ttu-id="626a8-109">Az ügyfelek hozhatnak létre több ASEs egyetlen Azure régión belül és több Azure-régiók között.</span><span class="sxs-lookup"><span data-stu-id="626a8-109">Customers can create multiple ASEs within a single Azure region and across multiple Azure regions.</span></span> <span data-ttu-id="626a8-110">Ez sokoldalúsága teszi ASEs ideális vízszintesen skálázás állapot nélküli alkalmazások rétegek magas RPS munkaterhelések támogatásához.</span><span class="sxs-lookup"><span data-stu-id="626a8-110">This versatility makes ASEs ideal for horizontally scaling stateless application tiers in support of high RPS workloads.</span></span>

<span data-ttu-id="626a8-111">ASEs elkülönített toorunning csak egyetlen vevő alkalmazások, és mindig egy Azure virtuális hálózatra telepített.</span><span class="sxs-lookup"><span data-stu-id="626a8-111">ASEs are isolated toorunning only a single customer's applications and are always deployed into an Azure virtual network.</span></span> <span data-ttu-id="626a8-112">Az ügyfelek szabályozhassák részletes bejövő és kimenő hálózati forgalmat a [hálózati biztonsági csoportok][NSGs].</span><span class="sxs-lookup"><span data-stu-id="626a8-112">Customers have fine-grained control over inbound and outbound application network traffic by using [Network Security Groups][NSGs].</span></span> <span data-ttu-id="626a8-113">Alkalmazások is is nagy sebességű biztonságos kapcsolat létrehozása a virtuális hálózatok tooon helyszíni vállalati erőforrások keresztül.</span><span class="sxs-lookup"><span data-stu-id="626a8-113">Applications can also establish high-speed secure connections over virtual networks tooon-premises corporate resources.</span></span>

<span data-ttu-id="626a8-114">Alkalmazások gyakran kell tooaccess vállalati erőforrások, például belső adatbázisok és webszolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="626a8-114">Apps frequently need tooaccess corporate resources, such as internal databases and web services.</span></span> <span data-ttu-id="626a8-115">ASEs a futó alkalmazások férjenek hozzá erőforrásokhoz keresztül [pont-pont] [ SiteToSite] VPN és [Azure ExpressRoute] [ ExpressRoute] kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="626a8-115">Apps that run on ASEs can access resources via [site-to-site][SiteToSite] VPN and [Azure ExpressRoute][ExpressRoute] connections.</span></span>

* <span data-ttu-id="626a8-116">[Mi az az App Service-környezet?][Intro]</span><span class="sxs-lookup"><span data-stu-id="626a8-116">[What is an App Service environment?][Intro]</span></span>
* <span data-ttu-id="626a8-117">[App Service-környezet létrehozása][MakeExternalASE]</span><span class="sxs-lookup"><span data-stu-id="626a8-117">[Create an App Service environment][MakeExternalASE]</span></span>
* <span data-ttu-id="626a8-118">[A belső terheléselosztók App Service Environment-környezet létrehozása][MakeILBASE]</span><span class="sxs-lookup"><span data-stu-id="626a8-118">[Create an internal load balancer App Service environment][MakeILBASE]</span></span>
* <span data-ttu-id="626a8-119">[App Service-környezet használata][UsingASE]</span><span class="sxs-lookup"><span data-stu-id="626a8-119">[Use an App Service environment][UsingASE]</span></span>
* <span data-ttu-id="626a8-120">[Hálózati szempontok és a hello App Service Environment-környezet][ASENetwork]</span><span class="sxs-lookup"><span data-stu-id="626a8-120">[Networking considerations and hello App Service environment][ASENetwork]</span></span>
* <span data-ttu-id="626a8-121">[App Service-környezet létrehozása sablonból][MakeASEfromTemplate]</span><span class="sxs-lookup"><span data-stu-id="626a8-121">[Create an App Service environment from a template][MakeASEfromTemplate]</span></span>


## <a name="videos"></a><span data-ttu-id="626a8-122">Videók</span><span class="sxs-lookup"><span data-stu-id="626a8-122">Videos</span></span>
<span data-ttu-id="626a8-123">Fő Modern PaaS a hello vállalati az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="626a8-123">Master Modern PaaS for hello Enterprise with Azure App Service</span></span>
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2016/BRK3205/player]

<span data-ttu-id="626a8-124">Hatékonyan skálázható és biztonságos alkalmazások üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="626a8-124">Deploying Highly Scalable and Secure Apps</span></span>
>[!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON325/player]

<span data-ttu-id="626a8-125">Nagyvállalati web- és mobilalkalmazások futtatása az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="626a8-125">Running Enterprise Web and Mobile Apps on Azure App Service</span></span>
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3715/player]

## <a name="app-service-environment-v1"></a><span data-ttu-id="626a8-126">App Service-környezet v1</span><span class="sxs-lookup"><span data-stu-id="626a8-126">App Service Environment v1</span></span> ##
<span data-ttu-id="626a8-127">App Service Environment-környezet két verziója van: ASEv1 és ASEv2.</span><span class="sxs-lookup"><span data-stu-id="626a8-127">There are two versions of App Service Environment: ASEv1 and ASEv2.</span></span> <span data-ttu-id="626a8-128">A ASEv1 információkért lásd: [App Service Environment-környezet v1 dokumentáció][ASEv1README].</span><span class="sxs-lookup"><span data-stu-id="626a8-128">For information on ASEv1, see [App Service Environment v1 documentation][ASEv1README].</span></span>


<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[ASEv1README]: ../app-service-app-service-environments-readme.md
[SiteToSite]: ../../vpn-gateway/vpn-gateway-site-to-site-create.md
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
