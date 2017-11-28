---
title: "aaaHow Azure App Service szolgáltatásban működik"
description: "Ismerje meg az App Service működését"
keywords: "app service, azure app service, méret, méretezhető, app service-csomag, app service ára"
services: app-service
documentationcenter: 
author: yochay
manager: erikre
editor: 
ms.assetid: ae74fc32-969e-4580-8d61-02c922f1f184
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/23/2017
ms.author: yochayk
ms.openlocfilehash: b20733ec8844773d063e2b6918605c4a48db1f5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-app-service-works"></a><span data-ttu-id="f0bff-104">Az App Service működése</span><span class="sxs-lookup"><span data-stu-id="f0bff-104">How App Service works</span></span>
<span data-ttu-id="f0bff-105">Az Azure App Service egy felhőalapú szolgáltatás, amely tervezett toosolve hello gyakorlati problémákat, mérnökök szembesülhetnek ma.</span><span class="sxs-lookup"><span data-stu-id="f0bff-105">Azure App Service is a cloud service that's designed toosolve hello practical problems that engineers face today.</span></span>
<span data-ttu-id="f0bff-106">App Service fókusz szolgáltatásában hatékony fejlesztést tesz lehetővé a hello veszélyeztetése nélkül toodeliver alkalmazások felhőhöz méretezett kell.</span><span class="sxs-lookup"><span data-stu-id="f0bff-106">App Service focuses on providing superior developer productivity without compromising on hello need toodeliver applications at cloud scale.</span></span> 

<span data-ttu-id="f0bff-107">App Service a hello szolgáltatásokat és vállalati üzleti alkalmazások létrehozásához szükséges keretrendszerek is biztosít.</span><span class="sxs-lookup"><span data-stu-id="f0bff-107">App Service also provides hello features and frameworks that are necessary for creating enterprise line-of-business applications.</span></span> <span data-ttu-id="f0bff-108">App Service lehetővé teszi a legnépszerűbb fejlesztési nyelveket, többek között a Java, PHP, Node.js, Python és a Microsoft .NET-nyelveket hello alkalmazások fejlesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="f0bff-108">App Service lets you develop apps in most popular development languages, including Java, PHP, Node.js, Python, and hello Microsoft .NET languages.</span></span> <span data-ttu-id="f0bff-109">Az App Service-szel a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="f0bff-109">With App Service, you can:</span></span>

* <span data-ttu-id="f0bff-110">Kiválóan méretezhető webalkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f0bff-110">Build highly scalable web apps.</span></span>
* <span data-ttu-id="f0bff-111">A Mobile Apps könnyen kezelhető mobileszközös lehetőségek (adatkezelési háttéralkalmazások, felhasználói hitelesítés és leküldéses értesítések) egész tárházát biztosítja.</span><span class="sxs-lookup"><span data-stu-id="f0bff-111">Quickly build Mobile Apps back ends with a set of easy-to-use mobile capabilities such as data back ends, user authentication, and push notifications.</span></span>
* <span data-ttu-id="f0bff-112">API-k végrehajtása, üzembe helyezése és közzététele az API Apps szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="f0bff-112">Implement, deploy, and publish APIs with API Apps.</span></span>
* <span data-ttu-id="f0bff-113">Üzleti alkalmazások összekapcsolása munkafolyamatokban és az adatok átalakítása a Logic Apps segítségével.</span><span class="sxs-lookup"><span data-stu-id="f0bff-113">Tie business applications together into workflows and transform data with Logic Apps.</span></span>

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

<span data-ttu-id="f0bff-114">Mindegyik alkalmazástípus alapja hello méretezhető és rugalmas webalkalmazások platformon, amely lehetővé teszi a fejlesztők toohave alkalmazás tervezési tooapp karbantartási tapasztalata az optimalizált teljes életciklusát.</span><span class="sxs-lookup"><span data-stu-id="f0bff-114">All app types rely on hello scalable and flexible Web Apps platform, which enables developers toohave an optimized full lifecycle experience from app design tooapp maintenance.</span></span> <span data-ttu-id="f0bff-115">hello életciklus-kezelési lehetőségek hello következő engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="f0bff-115">hello lifecycle capabilities enable hello following:</span></span>

* <span data-ttu-id="f0bff-116">**Alkalmazások gyors létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f0bff-116">**Quick app creation**.</span></span> <span data-ttu-id="f0bff-117">Új, vagy válasszon egy hello Azure piactér működési támogatási rendszer (OSS) csomagot.</span><span class="sxs-lookup"><span data-stu-id="f0bff-117">Start from scratch or pick an operational support system (OSS) package from hello Azure Marketplace.</span></span>
* <span data-ttu-id="f0bff-118">**Folyamatos üzembe helyezés**.</span><span class="sxs-lookup"><span data-stu-id="f0bff-118">**Continuous deployment**.</span></span> <span data-ttu-id="f0bff-119">Népszerű verziókövetési megoldásokból (például a TFS-ből, a GitHubról és a BitBucketből) származó új kód automatikus üzembe helyezése és online társzolgáltatásokban (például a OneDrive-ban vagy a DropBoxban) tárolt tartalom szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="f0bff-119">Automatically deploy new code from popular source control solutions such as TFS, GitHub, and Bitbucket, and sync content from online storage services such as OneDrive and Dropbox.</span></span>
* <span data-ttu-id="f0bff-120">**Tesztelés élesben**.</span><span class="sxs-lookup"><span data-stu-id="f0bff-120">**Test in production**.</span></span> <span data-ttu-id="f0bff-121">Éles üzem előtti környezetek létrehozása és problémamentesen hello mennyisége toothem forgalom kezelése.</span><span class="sxs-lookup"><span data-stu-id="f0bff-121">Smoothly create pre-production environments and manage hello amount of traffic that's going toothem.</span></span> <span data-ttu-id="f0bff-122">Szükség esetén hello felhőben hibakeresési, és állítsa vissza, ha problémák észlelése.</span><span class="sxs-lookup"><span data-stu-id="f0bff-122">Debug in hello cloud when needed, and roll back when issues are found.</span></span>
* <span data-ttu-id="f0bff-123">**Aszinkron és kötegelt feladatok futtatása**.</span><span class="sxs-lookup"><span data-stu-id="f0bff-123">**Running asynchronous tasks and batch jobs**.</span></span> <span data-ttu-id="f0bff-124">Kód futtatása háttérfolyamatként, vagy a kód aktiválása meghatározott események bekövetkezésekor (például az Azure Storage-üzenetsorokra érkező üzenetek esetén) és ütemezett időpontokban (CRON).</span><span class="sxs-lookup"><span data-stu-id="f0bff-124">Run code in a background process or activate your code based on events (such as messages landing in an Azure Storage queue) and scheduled times (CRON).</span></span>
* <span data-ttu-id="f0bff-125">**Méretezési hello app**.</span><span class="sxs-lookup"><span data-stu-id="f0bff-125">**Scaling hello app**.</span></span> <span data-ttu-id="f0bff-126">A vízszintes és függőleges adatforgalom és erőforrás-használat alapján szolgáltatás használata sok beállítások tooautomatically méretezési egyikét.</span><span class="sxs-lookup"><span data-stu-id="f0bff-126">Use one of many options tooautomatically scale your service horizontally and vertically based on traffic and resource utilization.</span></span> <span data-ttu-id="f0bff-127">Igazított privát környezet, dedikált tooyour alkalmazások közül.</span><span class="sxs-lookup"><span data-stu-id="f0bff-127">Configure private environments that are dedicated tooyour apps.</span></span>   
* <span data-ttu-id="f0bff-128">**Karbantartása hello app**.</span><span class="sxs-lookup"><span data-stu-id="f0bff-128">**Maintaining hello app**.</span></span> <span data-ttu-id="f0bff-129">Számos hello hibakeresés használja, és diagnosztikai funkciók toostay problémák és tooefficiently előre feloldása vagy a valós időben (például az automatikus javítás és éles hibakeresési), vagy után hello tény elemzésével naplók és memóriaképek.</span><span class="sxs-lookup"><span data-stu-id="f0bff-129">Use many of hello debugging and diagnostics features toostay ahead of problems and tooefficiently resolve them either in real time (with features such as auto-healing and live debugging) or after hello fact by analyzing logs and memory dumps.</span></span>

<span data-ttu-id="f0bff-130">Egy teljes App Service tehát a kódra a fejlesztők toofocus engedélyezése, és gyorsan állapotot elérni a stabil és kiválóan méretezhető éles.</span><span class="sxs-lookup"><span data-stu-id="f0bff-130">As a whole, App Service capabilities enable developers toofocus on their code and quickly reach a stable and highly scalable production state.</span></span> <span data-ttu-id="f0bff-131">Hello API-alkalmazások és a Logic Apps funkcióinak a fejlesztők létrehozhatják a valós vállalati alkalmazások, amelyek az üzleti megoldások és a helyszíni toocloud integrációs közötti akadályok hídjaként.</span><span class="sxs-lookup"><span data-stu-id="f0bff-131">With hello API Apps and Logic Apps features, developers can build real-world enterprise applications that bridge barriers between business solutions and on-premises toocloud integration.</span></span> 

## <a name="videos"></a><span data-ttu-id="f0bff-132">Videók</span><span class="sxs-lookup"><span data-stu-id="f0bff-132">Videos</span></span>
* [<span data-ttu-id="f0bff-133">Azure App Service-architektúra</span><span class="sxs-lookup"><span data-stu-id="f0bff-133">Azure App Service Architecture</span></span>](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)

## <a name="next-steps"></a><span data-ttu-id="f0bff-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f0bff-134">Next steps</span></span>

<span data-ttu-id="f0bff-135">További tudnivalók az App Service a következő témakörök hello egyikében:</span><span class="sxs-lookup"><span data-stu-id="f0bff-135">Learn more about App Service in one of hello following topics:</span></span>

* [<span data-ttu-id="f0bff-136">Az Azure App Service-ről</span><span class="sxs-lookup"><span data-stu-id="f0bff-136">What is Azure App Service?</span></span>](app-service-value-prop-what-is.md)
  * [<span data-ttu-id="f0bff-137">Webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="f0bff-137">Web App</span></span>](../app-service-web/app-service-web-overview.md)
  * [<span data-ttu-id="f0bff-138">Mobilalkalmazás</span><span class="sxs-lookup"><span data-stu-id="f0bff-138">Mobile App</span></span>](../app-service-mobile/app-service-mobile-value-prop.md)
  * [<span data-ttu-id="f0bff-139">API-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="f0bff-139">API App</span></span>](../app-service-api/app-service-api-apps-why-best-platform.md)
* [<span data-ttu-id="f0bff-140">Azure App Service-architektúra (bemutató)</span><span class="sxs-lookup"><span data-stu-id="f0bff-140">Azure App Service Architecture (presentation)</span></span>](http://www.slideshare.net/maartenba/windows-azure-web-sites-things-they-dont-teach-kids-in-school-comunity-day-2013)
* [<span data-ttu-id="f0bff-141">Az Azure App Service, a Cloud Services és a Virtual Machines összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="f0bff-141">Azure App Service, Cloud Services, and Virtual Machines comparison</span></span>](../app-service-web/choose-web-site-cloud-service-vm.md)
* [<span data-ttu-id="f0bff-142">Az App Service-csomagok ismertetése</span><span class="sxs-lookup"><span data-stu-id="f0bff-142">Understanding App Service Plans</span></span>](azure-web-sites-web-hosting-plans-in-depth-overview.md)
* [<span data-ttu-id="f0bff-143">Bevezetés tooApp Service-környezet</span><span class="sxs-lookup"><span data-stu-id="f0bff-143">Introduction tooApp Service Environment</span></span>](../app-service-web/app-service-app-service-environment-intro.md)
  * [<span data-ttu-id="f0bff-144">Gyakorlat: App Service Environment létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0bff-144">Exercise: Create an App Service Environment</span></span>](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="f0bff-145">Azure App Service fejlesztői vermek támogatása</span><span class="sxs-lookup"><span data-stu-id="f0bff-145">Azure App Service Development Stacks Support</span></span>](https://azure.microsoft.com/blog/windows-azure-websites-development-stacks-support/)



