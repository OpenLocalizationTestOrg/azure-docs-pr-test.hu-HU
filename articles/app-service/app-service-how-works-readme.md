---
title: "Az Azure App Service működése"
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
ms.openlocfilehash: 2d830963d3d2adba71a6ca99f79eac0fc8cbfb12
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-app-service-works"></a><span data-ttu-id="7a7b2-104">Az App Service működése</span><span class="sxs-lookup"><span data-stu-id="7a7b2-104">How App Service works</span></span>
<span data-ttu-id="7a7b2-105">Az Azure App Service felhőszolgáltatás, amely a mérnökök számára napi kihívást jelentő gyakorlati problémákat hivatott megoldani.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-105">Azure App Service is a cloud service that's designed to solve the practical problems that engineers face today.</span></span>
<span data-ttu-id="7a7b2-106">Az App Service rendkívül hatékony fejlesztést tesz lehetővé anélkül, hogy ehhez kompromisszumokat kellene kötni az alkalmazások felhőhöz méretezett szolgáltatása terén.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-106">App Service focuses on providing superior developer productivity without compromising on the need to deliver applications at cloud scale.</span></span> 

<span data-ttu-id="7a7b2-107">Az App Service emellett biztosítja a vállalati szintű üzletági alkalmazások létrehozásához szükséges szolgáltatásokat és kereteket.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-107">App Service also provides the features and frameworks that are necessary for creating enterprise line-of-business applications.</span></span> <span data-ttu-id="7a7b2-108">Az App Service-szel a legnépszerűbb fejlesztési nyelvekkel fejleszthet alkalmazásokat, beleértve a Java, PHP, Node.js, Python, és a Microsoft .NET nyelveket.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-108">App Service lets you develop apps in most popular development languages, including Java, PHP, Node.js, Python, and the Microsoft .NET languages.</span></span> <span data-ttu-id="7a7b2-109">Az App Service-szel a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="7a7b2-109">With App Service, you can:</span></span>

* <span data-ttu-id="7a7b2-110">Kiválóan méretezhető webalkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-110">Build highly scalable web apps.</span></span>
* <span data-ttu-id="7a7b2-111">A Mobile Apps könnyen kezelhető mobileszközös lehetőségek (adatkezelési háttéralkalmazások, felhasználói hitelesítés és leküldéses értesítések) egész tárházát biztosítja.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-111">Quickly build Mobile Apps back ends with a set of easy-to-use mobile capabilities such as data back ends, user authentication, and push notifications.</span></span>
* <span data-ttu-id="7a7b2-112">API-k végrehajtása, üzembe helyezése és közzététele az API Apps szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-112">Implement, deploy, and publish APIs with API Apps.</span></span>
* <span data-ttu-id="7a7b2-113">Üzleti alkalmazások összekapcsolása munkafolyamatokban és az adatok átalakítása a Logic Apps segítségével.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-113">Tie business applications together into workflows and transform data with Logic Apps.</span></span>

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

<span data-ttu-id="7a7b2-114">Mindegyik alkalmazástípus alapja a méretezhető és rugalmas Web Apps-platform, amely a fejlesztők számára a tervezésétől kezdve a karbantartásáig optimizált, teljes körű életciklus-kezelést biztosít.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-114">All app types rely on the scalable and flexible Web Apps platform, which enables developers to have an optimized full lifecycle experience from app design to app maintenance.</span></span> <span data-ttu-id="7a7b2-115">Az életciklus-kezelési funkciók a következőket teszik lehetővé:</span><span class="sxs-lookup"><span data-stu-id="7a7b2-115">The lifecycle capabilities enable the following:</span></span>

* <span data-ttu-id="7a7b2-116">**Alkalmazások gyors létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-116">**Quick app creation**.</span></span> <span data-ttu-id="7a7b2-117">Kezdje nulláról, vagy válasszon egy operatív támogatási rendszer (OSS) csomagot az Azure Marketplace-ről.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-117">Start from scratch or pick an operational support system (OSS) package from the Azure Marketplace.</span></span>
* <span data-ttu-id="7a7b2-118">**Folyamatos üzembe helyezés**.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-118">**Continuous deployment**.</span></span> <span data-ttu-id="7a7b2-119">Népszerű verziókövetési megoldásokból (például a TFS-ből, a GitHubról és a BitBucketből) származó új kód automatikus üzembe helyezése és online társzolgáltatásokban (például a OneDrive-ban vagy a DropBoxban) tárolt tartalom szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-119">Automatically deploy new code from popular source control solutions such as TFS, GitHub, and Bitbucket, and sync content from online storage services such as OneDrive and Dropbox.</span></span>
* <span data-ttu-id="7a7b2-120">**Tesztelés élesben**.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-120">**Test in production**.</span></span> <span data-ttu-id="7a7b2-121">Zökkenőmentes éles üzem előtti környezetek létrehozása és a feléjük irányuló forgalom mennyiségének kezelése.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-121">Smoothly create pre-production environments and manage the amount of traffic that's going to them.</span></span> <span data-ttu-id="7a7b2-122">Szükség esetén felhőbeli hibakeresést indíthat, és problémák észlelése esetén visszaállítást végezhet.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-122">Debug in the cloud when needed, and roll back when issues are found.</span></span>
* <span data-ttu-id="7a7b2-123">**Aszinkron és kötegelt feladatok futtatása**.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-123">**Running asynchronous tasks and batch jobs**.</span></span> <span data-ttu-id="7a7b2-124">Kód futtatása háttérfolyamatként, vagy a kód aktiválása meghatározott események bekövetkezésekor (például az Azure Storage-üzenetsorokra érkező üzenetek esetén) és ütemezett időpontokban (CRON).</span><span class="sxs-lookup"><span data-stu-id="7a7b2-124">Run code in a background process or activate your code based on events (such as messages landing in an Azure Storage queue) and scheduled times (CRON).</span></span>
* <span data-ttu-id="7a7b2-125">**Az alkalmazás méretezése**.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-125">**Scaling the app**.</span></span> <span data-ttu-id="7a7b2-126">Számos lehetőség áll rendelkezésre a szolgáltatások adatforgalom és erőforrás-használat függvényében való, automatikus horizontális és vertikális méretezésére.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-126">Use one of many options to automatically scale your service horizontally and vertically based on traffic and resource utilization.</span></span> <span data-ttu-id="7a7b2-127">Az alkalmazásokhoz igazított privát környezet kialakítása.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-127">Configure private environments that are dedicated to your apps.</span></span>   
* <span data-ttu-id="7a7b2-128">**Az alkalmazás karbantartása**.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-128">**Maintaining the app**.</span></span> <span data-ttu-id="7a7b2-129">Aknázza ki a hibakeresési és diagnosztikai szolgáltatásokban rejlő számos lehetőséget a problémák megelőzésére és hatékony kezelésére – akár valós időben (például az automatikus javítási és éles hibakeresési funkcióval), akár utólag, a naplók és a memóriaképek elemzésével.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-129">Use many of the debugging and diagnostics features to stay ahead of problems and to efficiently resolve them either in real time (with features such as auto-healing and live debugging) or after the fact by analyzing logs and memory dumps.</span></span>

<span data-ttu-id="7a7b2-130">Összességében az App Service tehát lehetővé teszi a fejlesztőknek, hogy a kódra összpontosítsanak, és gyorsan eljussanak a stabil és rugalmasan skálázható fejlesztési állapotig.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-130">As a whole, App Service capabilities enable developers to focus on their code and quickly reach a stable and highly scalable production state.</span></span> <span data-ttu-id="7a7b2-131">Az API Apps- és Logic Apps-szolgáltatások lehetővé teszik, hogy a fejlesztők tényleges vállalati alkalmazásokat tervezzenek, amelyekkel áthidalhatók az üzleti megoldások közötti akadályok, valamint a helyszíni környezet és a felhő integrálásának nehézségei.</span><span class="sxs-lookup"><span data-stu-id="7a7b2-131">With the API Apps and Logic Apps features, developers can build real-world enterprise applications that bridge barriers between business solutions and on-premises to cloud integration.</span></span> 

## <a name="videos"></a><span data-ttu-id="7a7b2-132">Videók</span><span class="sxs-lookup"><span data-stu-id="7a7b2-132">Videos</span></span>
* [<span data-ttu-id="7a7b2-133">Azure App Service-architektúra</span><span class="sxs-lookup"><span data-stu-id="7a7b2-133">Azure App Service Architecture</span></span>](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)

## <a name="next-steps"></a><span data-ttu-id="7a7b2-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7a7b2-134">Next steps</span></span>

<span data-ttu-id="7a7b2-135">Az App Service-ről a következő témakörökben talál további információt:</span><span class="sxs-lookup"><span data-stu-id="7a7b2-135">Learn more about App Service in one of the following topics:</span></span>

* [<span data-ttu-id="7a7b2-136">Az Azure App Service-ről</span><span class="sxs-lookup"><span data-stu-id="7a7b2-136">What is Azure App Service?</span></span>](app-service-value-prop-what-is.md)
  * [<span data-ttu-id="7a7b2-137">Webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="7a7b2-137">Web App</span></span>](../app-service-web/app-service-web-overview.md)
  * [<span data-ttu-id="7a7b2-138">Mobilalkalmazás</span><span class="sxs-lookup"><span data-stu-id="7a7b2-138">Mobile App</span></span>](../app-service-mobile/app-service-mobile-value-prop.md)
  * [<span data-ttu-id="7a7b2-139">API-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="7a7b2-139">API App</span></span>](../app-service-api/app-service-api-apps-why-best-platform.md)
* [<span data-ttu-id="7a7b2-140">Azure App Service-architektúra (bemutató)</span><span class="sxs-lookup"><span data-stu-id="7a7b2-140">Azure App Service Architecture (presentation)</span></span>](http://www.slideshare.net/maartenba/windows-azure-web-sites-things-they-dont-teach-kids-in-school-comunity-day-2013)
* [<span data-ttu-id="7a7b2-141">Az Azure App Service, a Cloud Services és a Virtual Machines összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="7a7b2-141">Azure App Service, Cloud Services, and Virtual Machines comparison</span></span>](../app-service-web/choose-web-site-cloud-service-vm.md)
* [<span data-ttu-id="7a7b2-142">Az App Service-csomagok ismertetése</span><span class="sxs-lookup"><span data-stu-id="7a7b2-142">Understanding App Service Plans</span></span>](azure-web-sites-web-hosting-plans-in-depth-overview.md)
* [<span data-ttu-id="7a7b2-143">Az App Service Environment bemutatása</span><span class="sxs-lookup"><span data-stu-id="7a7b2-143">Introduction to App Service Environment</span></span>](../app-service-web/app-service-app-service-environment-intro.md)
  * [<span data-ttu-id="7a7b2-144">Gyakorlat: App Service Environment létrehozása</span><span class="sxs-lookup"><span data-stu-id="7a7b2-144">Exercise: Create an App Service Environment</span></span>](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="7a7b2-145">Azure App Service fejlesztői vermek támogatása</span><span class="sxs-lookup"><span data-stu-id="7a7b2-145">Azure App Service Development Stacks Support</span></span>](https://azure.microsoft.com/blog/windows-azure-websites-development-stacks-support/)



