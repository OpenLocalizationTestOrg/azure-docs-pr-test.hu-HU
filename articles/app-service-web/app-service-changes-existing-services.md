---
title: "Az Azure App Service és a hatása a meglévő Azure-szolgáltatások"
description: "Ismerteti, milyen hatással van az új Azure App Service és a szolgáltatások meglévő szolgáltatások az Azure-ban."
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
ms.openlocfilehash: ed967fda7a216ed49532be54228ebe888cf16b6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-and-existing-azure-services"></a><span data-ttu-id="b2998-103">Azure App Service és a meglévő Azure-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b2998-103">Azure App Service and existing Azure services</span></span>
<span data-ttu-id="b2998-104">Ez a cikk ismerteti a módosításokat a meglévő Azure-szolgáltatásokhoz való összefogására olyan több Azure-szolgáltatásokhoz való módosítása részeként [Azure App Service](https://azure.microsoft.com/services/app-service/), integrált az új ajánlat.</span><span class="sxs-lookup"><span data-stu-id="b2998-104">This article outlines the changes to existing Azure services as part of the change to bring together several Azure services into [Azure App Service](https://azure.microsoft.com/services/app-service/), a new integrated offering.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a><span data-ttu-id="b2998-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b2998-105">Overview</span></span>
<span data-ttu-id="b2998-106">[Az Azure App Service](https://azure.microsoft.com/services/app-service/) egy új és egyedi felhőalapú szolgáltatás, amely lehetővé teszi a fejlesztők számára hozzon létre a web- és mobilalkalmazásokat bármilyen platformon, bármilyen eszközről.</span><span class="sxs-lookup"><span data-stu-id="b2998-106">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a new and unique cloud service that enables developers to create web and mobile apps for any platform and any device.</span></span> <span data-ttu-id="b2998-107">App Service egy integrált megoldást arra tervezték, hogy ismételt kódolási funkciók egyszerűsítésére integrálható a vállalati és a Szolgáltatottszoftver-rendszerek és üzleti folyamatok automatizálása mellett a biztonsági, megbízhatósági és méretezhetőségi funkciókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="b2998-107">App Service is an integrated solution designed to streamline repeated coding functions, integrate with enterprise and SaaS systems, and automate business processes while meeting your needs for security, reliability, and scalability.</span></span>

<span data-ttu-id="b2998-108">App Service egyesíti a meglévő Azure-szolgáltatásokat - [webhelyek](https://azure.microsoft.com/services/websites/), [Mobile Services](https://azure.microsoft.com/services/mobile-services/), és [Biztalk szolgáltatások](https://azure.microsoft.com/services/biztalk-services/) egyetlen összevont szolgáltatás, hatékony új képességek hozzáadása során.</span><span class="sxs-lookup"><span data-stu-id="b2998-108">App Service brings together the following existing Azure services - [Websites](https://azure.microsoft.com/services/websites/), [Mobile Services](https://azure.microsoft.com/services/mobile-services/), and [Biztalk Services](https://azure.microsoft.com/services/biztalk-services/) into a single combined service, while adding powerful new capabilities.</span></span>  <span data-ttu-id="b2998-109">App Service lehetővé teszi a következő alkalmazástípusok:</span><span class="sxs-lookup"><span data-stu-id="b2998-109">App Service allows you to host the following app types:</span></span>

* <span data-ttu-id="b2998-110">Web Apps</span><span class="sxs-lookup"><span data-stu-id="b2998-110">Web Apps</span></span>
* <span data-ttu-id="b2998-111">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="b2998-111">Mobile Apps</span></span>
* <span data-ttu-id="b2998-112">API Apps</span><span class="sxs-lookup"><span data-stu-id="b2998-112">API Apps</span></span>
* <span data-ttu-id="b2998-113">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="b2998-113">Logic Apps</span></span>

<span data-ttu-id="b2998-114">A következő táblázat ismerteti, hogyan meglévő Azure-szolgáltatások leképezés az App Service és a benne elérhető alkalmazástípusok.</span><span class="sxs-lookup"><span data-stu-id="b2998-114">The following table explains how existing Azure services map to App Service and the app types available within it.</span></span>

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%"><span data-ttu-id="b2998-115">Meglévő Azure szolgáltatást</span><span class="sxs-lookup"><span data-stu-id="b2998-115">Existing Azure Service</span></span></th>
<th align="left", style="width:10%"><span data-ttu-id="b2998-116">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b2998-116">Azure App Service</span></span></th>
<th align="left", style="width:80%"><span data-ttu-id="b2998-117">Változtatások a</span><span class="sxs-lookup"><span data-stu-id="b2998-117">What changed</span></span></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><span data-ttu-id="b2998-118">Azure webhelyek szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b2998-118">Azure Websites</span></span></td>
<td align="left"><span data-ttu-id="b2998-119">Web Apps</span><span class="sxs-lookup"><span data-stu-id="b2998-119">Web Apps</span></span></td>
<td align="left"><li><span data-ttu-id="b2998-120">Az Azure-webhely az App Service korlátozódik szigorúan webhelyek nevének módosítása a webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="b2998-120">For Azure Websites, App Service is strictly limited to changing the name  Websites to Web Apps.</span></span>
<p><li><span data-ttu-id="b2998-121">A webhelyek meglévő példányát is az App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="b2998-121">All your existing instances of Websites are now Web Apps in App Service.</span></span></p>
<p><li><span data-ttu-id="b2998-122">A meglévő webhelyek keresztül érheti el a <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure Portal</a>, ahol megtalálja az összes meglévő helyet <em>webalkalmazások</em>.</span><span class="sxs-lookup"><span data-stu-id="b2998-122">You can access your existing websites via the <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure Portal</a>, where you will find all your existing sites under <em>Web Apps</em>.</span></span></p>
<p><li><span data-ttu-id="b2998-123"><em>Webalkalmazás-üzemeltetési terv</em> most <em>App Service-csomag</em>.</span><span class="sxs-lookup"><span data-stu-id="b2998-123"><em>Web Hosting Plan</em> is now <em>App Service Plan</em>.</span></span> <span data-ttu-id="b2998-124">Egy <em>App Service-csomag</em> App Service-ben például a webes, mobil, logika vagy API apps bármilyen alkalmazás, rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="b2998-124">An <em>App Service Plan</em> can host any app type of App Service, such as Web, Mobile, Logic, or API apps.</span></span></p>
<p><li><span data-ttu-id="b2998-125">Az Azure App Service Web Apps általános rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="b2998-125">Azure App Service Web Apps is in General Availability.</span></span></p>
<p><li><span data-ttu-id="b2998-126"><a href="http://azure.microsoft.com/services/app-service/web/">További tudnivalók a webalkalmazások</a>.</span><span class="sxs-lookup"><span data-stu-id="b2998-126"><a href="http://azure.microsoft.com/services/app-service/web/">Learn more about Web Apps</a>.</span></span></p></td>
</tr>
<tr class="even">
<td align="left"><span data-ttu-id="b2998-127">Azure mobilszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b2998-127">Azure Mobile Services</span></span></td>
<td align="left"><span data-ttu-id="b2998-128">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="b2998-128">Mobile Apps</span></span></td>
<td align="left"><p><li><span data-ttu-id="b2998-129">A Mobile Services továbbra is önálló szolgáltatásként érhető el, és továbbra is teljes mértékben támogatott.</span><span class="sxs-lookup"><span data-stu-id="b2998-129">Mobile Services continue to be available as a standalone service and remain fully supported.</span></span></p>
<p><li><span data-ttu-id="b2998-130">Mobile Apps típus egy alkalmazás az App Service szolgáltatásban, amely egyesíti az összes a Mobile Services és egyéb funkcióit.</span><span class="sxs-lookup"><span data-stu-id="b2998-130">Mobile Apps is an app type in App Service, which integrates all of the functionality of Mobile Services and more.</span></span></p>
<p><li><span data-ttu-id="b2998-131">Könnyen <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">Mobile Apps Mobile Services át</a>.</span><span class="sxs-lookup"><span data-stu-id="b2998-131">It is easy to <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">migrate from Mobile Services to Mobile Apps</a>.</span></span></p>
<p><li><span data-ttu-id="b2998-132">Része az App Service Mobile Apps új lehetőségek elérése túl a Mobile Services-integráció a helyszíni és a Szolgáltatottszoftver-rendszereket, például átmeneti üzembe helyezési ponti, webjobs-feladatok, jobb skálázási lehetőségeket és más.</span><span class="sxs-lookup"><span data-stu-id="b2998-132">As part of App Service, Mobile Apps get new capabilities beyond Mobile Services, such as  integration with on-premises and SaaS systems, staging slots, WebJobs, better scaling options, and more.</span></span></p>
<p><li><span data-ttu-id="b2998-133"><a href="http://azure.microsoft.com/services/app-service/mobile/">Tudjon meg többet a Mobile Apps</a>.</span><span class="sxs-lookup"><span data-stu-id="b2998-133"><a href="http://azure.microsoft.com/services/app-service/mobile/">Learn more about Mobile Apps</a>.</span></span></p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left"><span data-ttu-id="b2998-134">API Apps</span><span class="sxs-lookup"><span data-stu-id="b2998-134">API Apps</span></span></td>
<td align="left">
<p><li><span data-ttu-id="b2998-135">API-alkalmazások olyan új alkalmazást, amely lehetővé teszi, hogy könnyen létrehozása és felhasználása a felhőben API App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="b2998-135">API Apps is a new app type in App Service that lets you easily build and consume APIs in the cloud.</span></span></p>
<p><li><span data-ttu-id="b2998-136"><a href="http://azure.microsoft.com/services/app-service/api/">További tudnivalók az API Apps</a>.</span><span class="sxs-lookup"><span data-stu-id="b2998-136"><a href="http://azure.microsoft.com/services/app-service/api/">Learn more about API Apps</a>.</span></span></p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left"><span data-ttu-id="b2998-137">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="b2998-137">Logic Apps</span></span></td>
<td align="left">
<p><li><span data-ttu-id="b2998-138">A Logic Apps olyan új alkalmazás az App Service szolgáltatásban, amely lehetővé teszi, hogy könnyen az üzleti folyamatok automatizálása.</span><span class="sxs-lookup"><span data-stu-id="b2998-138">Logic Apps is a new app type in App Service that lets you easily automate business processes.</span></span></p>
<p><li><span data-ttu-id="b2998-139"><a href="http://azure.microsoft.com/services/app-service/logic/">További információk a Logic Apps</a>.</span><span class="sxs-lookup"><span data-stu-id="b2998-139"><a href="http://azure.microsoft.com/services/app-service/logic/">Learn more about Logic Apps</a>.</span></span></p></td>
</tr>
<tr class="odd">
<td align="left"><span data-ttu-id="b2998-140">Azure BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="b2998-140">Azure BizTalk Services</span></span></td>
<td align="left"><span data-ttu-id="b2998-141">BizTalk API-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="b2998-141">BizTalk API Apps</span></span></td>
<td align="left">
<li><p><span data-ttu-id="b2998-142">BizTalk szolgáltatások továbbra is önálló szolgáltatásként érhető el, és továbbra is teljes mértékben támogatott.</span><span class="sxs-lookup"><span data-stu-id="b2998-142">BizTalk Services continue to be available as a standalone service and remain fully supported.</span></span></p>
<li><p><span data-ttu-id="b2998-143">BizTalk szolgáltatások minden képességet integrálva vannak az App Service vállalati alkalmazások integrálása és az alkalmazás típusok integrációs feladatok B2B végrehajtásához az App Service API Apps engedélyezése felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="b2998-143">All the capabilities of BizTalk Services are integrated into App Service as API Apps enabling users to perform enterprise application integration and B2B integration scenarios with any of the app types in App Service.</span></span></p>
<li><p><span data-ttu-id="b2998-144">A Logic Apps mostantól automatizálhatja üzleti folyamatok használata a vizuális Tervező élmény munkafolyamatok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b2998-144">With Logic Apps, you can now automate business processes using a visual design experience to create workflows.</span></span></p></td>
</tr>
</tbody>
</table>

<span data-ttu-id="b2998-145">További információkért látogasson el [App Service-dokumentáció](https://azure.microsoft.com/documentation/services/app-service/).</span><span class="sxs-lookup"><span data-stu-id="b2998-145">To learn more, please visit [App Service documentation](https://azure.microsoft.com/documentation/services/app-service/).</span></span>

