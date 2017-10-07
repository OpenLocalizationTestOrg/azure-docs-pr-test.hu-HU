---
title: "App Service aaaAzure és a hatása a meglévő Azure-szolgáltatások"
description: "Azt ismerteti, hogyan hello az új Azure App Service és a szolgáltatások befolyásolja a meglévő szolgáltatások az Azure-ban."
services: app-service
documentationcenter: 
author: yochay
manager: nirma
editor: 
ms.assetid: 86c6a292-3c33-49f4-890c-89cc0321b397
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2016
ms.author: yochaykk
ms.openlocfilehash: a831a88fee38465e5b0b7c2c2340cf8a0d64c864
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-and-existing-azure-services"></a><span data-ttu-id="dae0b-103">Azure App Service és a meglévő Azure-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="dae0b-103">Azure App Service and existing Azure services</span></span>
<span data-ttu-id="dae0b-104">Ez a cikk ismerteti a hello módosítások tooexisting Azure services hello módosítása toobring együtt részeként több Azure-szolgáltatásokhoz való [Azure App Service](https://azure.microsoft.com/services/app-service/), integrált az új ajánlat.</span><span class="sxs-lookup"><span data-stu-id="dae0b-104">This article outlines hello changes tooexisting Azure services as part of hello change toobring together several Azure services into [Azure App Service](https://azure.microsoft.com/services/app-service/), a new integrated offering.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a><span data-ttu-id="dae0b-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="dae0b-105">Overview</span></span>
<span data-ttu-id="dae0b-106">[Az Azure App Service](https://azure.microsoft.com/services/app-service/) egy új és egyedi felhőalapú szolgáltatás, amely lehetővé teszi a fejlesztők toocreate web- és mobilalkalmazásokat bármilyen platformon, bármilyen eszközről.</span><span class="sxs-lookup"><span data-stu-id="dae0b-106">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a new and unique cloud service that enables developers toocreate web and mobile apps for any platform and any device.</span></span> <span data-ttu-id="dae0b-107">App Service egy integrált megoldás részeként tervezett toostreamline ismétlődő kódolási funkciók, integrálható a vállalati és a Szolgáltatottszoftver-rendszereket, és üzleti folyamatok automatizálása a biztonsági, megbízhatósági és méretezhetőségi igényeinek betartása mellett.</span><span class="sxs-lookup"><span data-stu-id="dae0b-107">App Service is an integrated solution designed toostreamline repeated coding functions, integrate with enterprise and SaaS systems, and automate business processes while meeting your needs for security, reliability, and scalability.</span></span>

<span data-ttu-id="dae0b-108">App Service egyesíti hello követően a meglévő Azure - szolgáltatások [webhelyek](https://azure.microsoft.com/services/websites/), [Mobile Services](https://azure.microsoft.com/services/mobile-services/), és [Biztalk szolgáltatások](https://azure.microsoft.com/services/biztalk-services/) egyetlen összevont szolgáltatás, közben hatékony új képességek hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="dae0b-108">App Service brings together hello following existing Azure services - [Websites](https://azure.microsoft.com/services/websites/), [Mobile Services](https://azure.microsoft.com/services/mobile-services/), and [Biztalk Services](https://azure.microsoft.com/services/biztalk-services/) into a single combined service, while adding powerful new capabilities.</span></span>  <span data-ttu-id="dae0b-109">App Service lehetővé teszi a következő alkalmazástípusok toohost hello:</span><span class="sxs-lookup"><span data-stu-id="dae0b-109">App Service allows you toohost hello following app types:</span></span>

* <span data-ttu-id="dae0b-110">Web Apps</span><span class="sxs-lookup"><span data-stu-id="dae0b-110">Web Apps</span></span>
* <span data-ttu-id="dae0b-111">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="dae0b-111">Mobile Apps</span></span>
* <span data-ttu-id="dae0b-112">API Apps</span><span class="sxs-lookup"><span data-stu-id="dae0b-112">API Apps</span></span>
* <span data-ttu-id="dae0b-113">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="dae0b-113">Logic Apps</span></span>

<span data-ttu-id="dae0b-114">hello következő táblázat ismerteti, hogyan meglévő Azure szolgáltatások leképezése tooApp szolgáltatás és hello alkalmazástípusok belül érhető el.</span><span class="sxs-lookup"><span data-stu-id="dae0b-114">hello following table explains how existing Azure services map tooApp Service and hello app types available within it.</span></span>

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%"><span data-ttu-id="dae0b-115">Meglévő Azure szolgáltatást</span><span class="sxs-lookup"><span data-stu-id="dae0b-115">Existing Azure Service</span></span></th>
<th align="left", style="width:10%"><span data-ttu-id="dae0b-116">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="dae0b-116">Azure App Service</span></span></th>
<th align="left", style="width:80%"><span data-ttu-id="dae0b-117">Változtatások a</span><span class="sxs-lookup"><span data-stu-id="dae0b-117">What changed</span></span></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><span data-ttu-id="dae0b-118">Azure webhelyek szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="dae0b-118">Azure Websites</span></span></td>
<td align="left"><span data-ttu-id="dae0b-119">Web Apps</span><span class="sxs-lookup"><span data-stu-id="dae0b-119">Web Apps</span></span></td>
<td align="left"><li><span data-ttu-id="dae0b-120">Azure-webhelyekre az App Service kizárólag toochanging hello neve webhelyek tooWeb alkalmazások esetén.</span><span class="sxs-lookup"><span data-stu-id="dae0b-120">For Azure Websites, App Service is strictly limited toochanging hello name  Websites tooWeb Apps.</span></span>
<p><li><span data-ttu-id="dae0b-121">A webhelyek meglévő példányát is az App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="dae0b-121">All your existing instances of Websites are now Web Apps in App Service.</span></span></p>
<p><li><span data-ttu-id="dae0b-122">A meglévő webhelyek hello keresztül érheti el <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure Portal</a>, ahol megtalálja az összes meglévő helyet <em>webalkalmazások</em>.</span><span class="sxs-lookup"><span data-stu-id="dae0b-122">You can access your existing websites via hello <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure Portal</a>, where you will find all your existing sites under <em>Web Apps</em>.</span></span></p>
<p><li><span data-ttu-id="dae0b-123"><em>Webalkalmazás-üzemeltetési terv</em> most <em>App Service-csomag</em>.</span><span class="sxs-lookup"><span data-stu-id="dae0b-123"><em>Web Hosting Plan</em> is now <em>App Service Plan</em>.</span></span> <span data-ttu-id="dae0b-124">Egy <em>App Service-csomag</em> App Service-ben például a webes, mobil, logika vagy API apps bármilyen alkalmazás, rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="dae0b-124">An <em>App Service Plan</em> can host any app type of App Service, such as Web, Mobile, Logic, or API apps.</span></span></p>
<p><li><span data-ttu-id="dae0b-125">Az Azure App Service Web Apps általános rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="dae0b-125">Azure App Service Web Apps is in General Availability.</span></span></p>
<p><li><span data-ttu-id="dae0b-126"><a href="http://azure.microsoft.com/services/app-service/web/">További tudnivalók a webalkalmazások</a>.</span><span class="sxs-lookup"><span data-stu-id="dae0b-126"><a href="http://azure.microsoft.com/services/app-service/web/">Learn more about Web Apps</a>.</span></span></p></td>
</tr>
<tr class="even">
<td align="left"><span data-ttu-id="dae0b-127">Azure mobilszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="dae0b-127">Azure Mobile Services</span></span></td>
<td align="left"><span data-ttu-id="dae0b-128">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="dae0b-128">Mobile Apps</span></span></td>
<td align="left"><p><li><span data-ttu-id="dae0b-129">A Mobile Services továbbra is toobe önálló szolgáltatásként érhető el, és továbbra is teljes mértékben támogatott.</span><span class="sxs-lookup"><span data-stu-id="dae0b-129">Mobile Services continue toobe available as a standalone service and remain fully supported.</span></span></p>
<p><li><span data-ttu-id="dae0b-130">Mobile Apps típus egy alkalmazás az App Service szolgáltatásban, amely egyesíti az összes hello Mobile Services és egyéb funkcióit.</span><span class="sxs-lookup"><span data-stu-id="dae0b-130">Mobile Apps is an app type in App Service, which integrates all of hello functionality of Mobile Services and more.</span></span></p>
<p><li><span data-ttu-id="dae0b-131">Egyszerű túl<a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">át a Mobile Services tooMobile alkalmazások</a>.</span><span class="sxs-lookup"><span data-stu-id="dae0b-131">It is easy too<a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">migrate from Mobile Services tooMobile Apps</a>.</span></span></p>
<p><li><span data-ttu-id="dae0b-132">Része az App Service Mobile Apps új lehetőségek elérése túl a Mobile Services-integráció a helyszíni és a Szolgáltatottszoftver-rendszereket, például átmeneti üzembe helyezési ponti, webjobs-feladatok, jobb skálázási lehetőségeket és más.</span><span class="sxs-lookup"><span data-stu-id="dae0b-132">As part of App Service, Mobile Apps get new capabilities beyond Mobile Services, such as  integration with on-premises and SaaS systems, staging slots, WebJobs, better scaling options, and more.</span></span></p>
<p><li><span data-ttu-id="dae0b-133"><a href="http://azure.microsoft.com/services/app-service/mobile/">Tudjon meg többet a Mobile Apps</a>.</span><span class="sxs-lookup"><span data-stu-id="dae0b-133"><a href="http://azure.microsoft.com/services/app-service/mobile/">Learn more about Mobile Apps</a>.</span></span></p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left"><span data-ttu-id="dae0b-134">API Apps</span><span class="sxs-lookup"><span data-stu-id="dae0b-134">API Apps</span></span></td>
<td align="left">
<p><li><span data-ttu-id="dae0b-135">API-alkalmazások olyan új alkalmazás az App Service segítségével könnyen létrehozása és felhasználása hello felhő API-k.</span><span class="sxs-lookup"><span data-stu-id="dae0b-135">API Apps is a new app type in App Service that lets you easily build and consume APIs in hello cloud.</span></span></p>
<p><li><span data-ttu-id="dae0b-136"><a href="http://azure.microsoft.com/services/app-service/api/">További tudnivalók az API Apps</a>.</span><span class="sxs-lookup"><span data-stu-id="dae0b-136"><a href="http://azure.microsoft.com/services/app-service/api/">Learn more about API Apps</a>.</span></span></p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left"><span data-ttu-id="dae0b-137">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="dae0b-137">Logic Apps</span></span></td>
<td align="left">
<p><li><span data-ttu-id="dae0b-138">A Logic Apps olyan új alkalmazás az App Service szolgáltatásban, amely lehetővé teszi, hogy könnyen az üzleti folyamatok automatizálása.</span><span class="sxs-lookup"><span data-stu-id="dae0b-138">Logic Apps is a new app type in App Service that lets you easily automate business processes.</span></span></p>
<p><li><span data-ttu-id="dae0b-139"><a href="http://azure.microsoft.com/services/app-service/logic/">További információk a Logic Apps</a>.</span><span class="sxs-lookup"><span data-stu-id="dae0b-139"><a href="http://azure.microsoft.com/services/app-service/logic/">Learn more about Logic Apps</a>.</span></span></p></td>
</tr>
<tr class="odd">
<td align="left"><span data-ttu-id="dae0b-140">Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="dae0b-140">Azure BizTalk Services</span></span></td>
<td align="left"><span data-ttu-id="dae0b-141">BizTalk API-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="dae0b-141">BizTalk API Apps</span></span></td>
<td align="left">
<li><p><span data-ttu-id="dae0b-142">BizTalk szolgáltatások továbbra is toobe önálló szolgáltatásként érhető el, és továbbra is teljes mértékben támogatott.</span><span class="sxs-lookup"><span data-stu-id="dae0b-142">BizTalk Services continue toobe available as a standalone service and remain fully supported.</span></span></p>
<li><p><span data-ttu-id="dae0b-143">BizTalk szolgáltatások összes hello képességek integrálva vannak az App Service API-alkalmazások engedélyezése felhasználók tooperform vállalati alkalmazások integrálása és B2B integrációs feladatokhoz egyetlen hello alkalmazástípusok az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="dae0b-143">All hello capabilities of BizTalk Services are integrated into App Service as API Apps enabling users tooperform enterprise application integration and B2B integration scenarios with any of hello app types in App Service.</span></span></p>
<li><p><span data-ttu-id="dae0b-144">A Logic Apps mostantól automatizálhatja a vizuális Tervező élmény toocreate munkafolyamatokkal üzleti folyamatokat.</span><span class="sxs-lookup"><span data-stu-id="dae0b-144">With Logic Apps, you can now automate business processes using a visual design experience toocreate workflows.</span></span></p></td>
</tr>
</tbody>
</table>

<span data-ttu-id="dae0b-145">toolearn több, látogasson el a [App Service-dokumentáció](https://azure.microsoft.com/documentation/services/app-service/).</span><span class="sxs-lookup"><span data-stu-id="dae0b-145">toolearn more, please visit [App Service documentation](https://azure.microsoft.com/documentation/services/app-service/).</span></span>

