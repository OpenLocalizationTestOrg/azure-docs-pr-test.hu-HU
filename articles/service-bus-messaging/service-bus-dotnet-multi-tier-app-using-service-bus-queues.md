---
title: "Többrétegű .NET-alkalmazás az Azure Service Bus használatával | Microsoft Docs"
description: "Ezen .NET-oktatóanyag segítségével többrétegű alkalmazást fejleszthet az Azure-ban, amely Service Bus-üzenetsorokkal kommunikál a rétegek között."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1b8608ca-aa5a-4700-b400-54d65b02615c
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 04/11/2017
ms.author: sethm
ms.openlocfilehash: 8b502f5ac5d89801d390a872e7a8b06e094ecbba
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a><span data-ttu-id="51901-103">Többrétegű .NET-alkalmazás Azure Service Bus-üzenetsorok használatával</span><span class="sxs-lookup"><span data-stu-id="51901-103">.NET multi-tier application using Azure Service Bus queues</span></span>
## <a name="introduction"></a><span data-ttu-id="51901-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="51901-104">Introduction</span></span>
<span data-ttu-id="51901-105">A Visual Studio és az ingyenes Azure SDK for .NET használatával könnyen fejleszthet a Microsoft Azure platformra.</span><span class="sxs-lookup"><span data-stu-id="51901-105">Developing for Microsoft Azure is easy using Visual Studio and the free Azure SDK for .NET.</span></span> <span data-ttu-id="51901-106">Ez az oktatóanyag végigvezeti egy olyan alkalmazás létrehozásának a lépésein, amely több, a helyi környezetben futó Azure-erőforrást használ.</span><span class="sxs-lookup"><span data-stu-id="51901-106">This tutorial walks you through the steps to create an application that uses multiple Azure resources running in your local environment.</span></span>

<span data-ttu-id="51901-107">Az alábbiakat sajátítja majd el:</span><span class="sxs-lookup"><span data-stu-id="51901-107">You will learn the following:</span></span>

* <span data-ttu-id="51901-108">A számítógép felkészítése az Azure-fejlesztésre egyetlen letöltéssel és telepítéssel.</span><span class="sxs-lookup"><span data-stu-id="51901-108">How to enable your computer for Azure development with a single download and install.</span></span>
* <span data-ttu-id="51901-109">A Visual Studio használata az Azure platformra való fejlesztéshez.</span><span class="sxs-lookup"><span data-stu-id="51901-109">How to use Visual Studio to develop for Azure.</span></span>
* <span data-ttu-id="51901-110">Többrétegű alkalmazások létrehozása az Azure-ban webes és feldolgozói szerepkörök használatával.</span><span class="sxs-lookup"><span data-stu-id="51901-110">How to create a multi-tier application in Azure using web and worker roles.</span></span>
* <span data-ttu-id="51901-111">A rétegek közötti kommunikáció módja Service Bus-üzenetsorok használatával.</span><span class="sxs-lookup"><span data-stu-id="51901-111">How to communicate between tiers using Service Bus queues.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

<span data-ttu-id="51901-112">Az oktatóanyagban egy Azure-felhőszolgáltatásban hozza létre és futtatja majd a többrétegű alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="51901-112">In this tutorial you'll build and run the multi-tier application in an Azure cloud service.</span></span> <span data-ttu-id="51901-113">Az előtér egy ASP.NET MVC webes szerepkör, a háttér pedig egy Service Bus-üzenetsort használó feldolgozói szerepkör.</span><span class="sxs-lookup"><span data-stu-id="51901-113">The front end is an ASP.NET MVC web role and the back end is a worker-role that uses a Service Bus queue.</span></span> <span data-ttu-id="51901-114">Ugyanezen többrétegű alkalmazást létrehozhatja úgy is, hogy az előtér olyan webes projekt legyen, amely a felhőszolgáltatás helyett egy Azure-webhelyen helyezhető üzembe.</span><span class="sxs-lookup"><span data-stu-id="51901-114">You can create the same multi-tier application with the front end as a web project, that is deployed to an Azure website instead of a cloud service.</span></span> <span data-ttu-id="51901-115">Elvégezheti a [.NET helyszíni/felhőalapú hibridalkalmazással](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) foglalkozó oktatóanyagot is.</span><span class="sxs-lookup"><span data-stu-id="51901-115">You can also try out the [.NET on-premises/cloud hybrid application](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) tutorial.</span></span>

<span data-ttu-id="51901-116">Az alábbi képernyőfelvételen a kész alkalmazás látható.</span><span class="sxs-lookup"><span data-stu-id="51901-116">The following screen shot shows the completed application.</span></span>

![][0]

## <a name="scenario-overview-inter-role-communication"></a><span data-ttu-id="51901-117">Forgatókönyv áttekintése: szerepkörök közötti kommunikáció</span><span class="sxs-lookup"><span data-stu-id="51901-117">Scenario overview: inter-role communication</span></span>
<span data-ttu-id="51901-118">A feldolgozási kérés küldéséhez a webes szerepkörben futó előtér felhasználói felületi összetevőnek együtt kell működnie a feldolgozói szerepkörben futó középső rétegbeli logikával.</span><span class="sxs-lookup"><span data-stu-id="51901-118">To submit an order for processing, the front-end UI component, running in the web role, must interact with the middle tier logic running in the worker role.</span></span> <span data-ttu-id="51901-119">Ez a példa Service Bus-üzenetkezelést használ a rétegek közötti kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="51901-119">This example uses Service Bus messaging for the communication between the tiers.</span></span>

<span data-ttu-id="51901-120">A webes és a középső réteg között használt Service Bus-üzenetkezelés elválasztja a két összetevőt.</span><span class="sxs-lookup"><span data-stu-id="51901-120">Using Service Bus messaging between the web and middle tiers decouples the two components.</span></span> <span data-ttu-id="51901-121">A közvetlen (vagyis TCP- vagy HTTP-alapú) üzenettovábbítással szemben a webes réteg nem közvetlenül kapcsolódik a középső réteghez, hanem a munkaegységeket üzenetekként küldi le a Service Busba, amely megbízhatóan megőrzi azokat, amíg a középső réteg kész fogadni és feldolgozni azokat.</span><span class="sxs-lookup"><span data-stu-id="51901-121">In contrast to direct messaging (that is, TCP or HTTP), the web tier does not connect to the middle tier directly; instead it pushes units of work, as messages, into Service Bus, which reliably retains them until the middle tier is ready to consume and process them.</span></span>

<span data-ttu-id="51901-122">A Service Bus kétfajta entitást biztosít a közvetítőalapú üzenettovábbítás támogatásához: üzenetsorokat és témaköröket.</span><span class="sxs-lookup"><span data-stu-id="51901-122">Service Bus provides two entities to support brokered messaging: queues and topics.</span></span> <span data-ttu-id="51901-123">Az üzenetsorok esetén az egyes üzenetsorokra küldött üzeneteket egyetlen fogadó használja fel.</span><span class="sxs-lookup"><span data-stu-id="51901-123">With queues, each message sent to the queue is consumed by a single receiver.</span></span> <span data-ttu-id="51901-124">A témakörök a közzététel/előfizetés mintát támogatják, amelyben az egyes közzétett üzenetek az adott témakörre való előfizetéssel érhetők el.</span><span class="sxs-lookup"><span data-stu-id="51901-124">Topics support the publish/subscribe pattern in which each published message is made available to a subscription registered with the topic.</span></span> <span data-ttu-id="51901-125">Az egyes előfizetések logikai módon tartják fenn a saját üzenetsorukat.</span><span class="sxs-lookup"><span data-stu-id="51901-125">Each subscription logically maintains its own queue of messages.</span></span> <span data-ttu-id="51901-126">Az előfizetések konfigurálhatók szűrési szabályokkal is, amelyek az előfizetés üzenetsorába továbbított üzeneteket az adott szűrővel egyező üzenetekre korlátozzák.</span><span class="sxs-lookup"><span data-stu-id="51901-126">Subscriptions can also be configured with filter rules that restrict the set of messages passed to the subscription queue to those that match the filter.</span></span> <span data-ttu-id="51901-127">Az alábbi példa Service Bus-üzenetsorokat használ.</span><span class="sxs-lookup"><span data-stu-id="51901-127">The following example uses Service Bus queues.</span></span>

![][1]

<span data-ttu-id="51901-128">Ennek a kommunikációs mechanizmusnak több előnye is van a közvetlen üzenettovábbítással szemben:</span><span class="sxs-lookup"><span data-stu-id="51901-128">This communication mechanism has several advantages over direct messaging:</span></span>

* <span data-ttu-id="51901-129">**Időbeli elválasztás.**</span><span class="sxs-lookup"><span data-stu-id="51901-129">**Temporal decoupling.**</span></span> <span data-ttu-id="51901-130">Az aszinkron üzenettovábbítási mintának köszönhetően a létrehozóknak és a felhasználóknak nem kell egyszerre online lenniük.</span><span class="sxs-lookup"><span data-stu-id="51901-130">With the asynchronous messaging pattern, producers and consumers need not be online at the same time.</span></span> <span data-ttu-id="51901-131">A Service Bus megbízhatóan tárolja az üzeneteket, amíg a felhasználó fél készen nem áll a fogadásukra.</span><span class="sxs-lookup"><span data-stu-id="51901-131">Service Bus reliably stores messages until the consuming party is ready to receive them.</span></span> <span data-ttu-id="51901-132">Ez lehetővé teszi az elosztott alkalmazás összetevőinek leválasztását, akár önkéntesen – például karbantartási céllal –, akár egy összetevő összeomlása miatt, anélkül, hogy ez az egész rendszerre hatással lenne.</span><span class="sxs-lookup"><span data-stu-id="51901-132">This enables the components of the distributed application to be disconnected, either voluntarily, for example, for maintenance, or due to a component crash, without impacting the system as a whole.</span></span> <span data-ttu-id="51901-133">Emellett a felhasználó alkalmazásnak elég mindössze a nap bizonyos időpontjaiban online lennie.</span><span class="sxs-lookup"><span data-stu-id="51901-133">Furthermore, the consuming application might only need to come online during certain times of the day.</span></span>
* <span data-ttu-id="51901-134">**Terheléskiegyenlítés.**</span><span class="sxs-lookup"><span data-stu-id="51901-134">**Load leveling.**</span></span> <span data-ttu-id="51901-135">Számos alkalmazásban a rendszerterhelés időnként eltérő, míg az egyes munkaegységek feldolgozásához szükséges idő jellemzően állandó marad.</span><span class="sxs-lookup"><span data-stu-id="51901-135">In many applications, system load varies over time, while the processing time required for each unit of work is typically constant.</span></span> <span data-ttu-id="51901-136">Az üzenetek létrehozói és felhasználói közé üzenetsorokat helyezve a felhasználó alkalmazást (a feldolgozót) csak az átlagos terhelés, és nem a csúcsterhelés figyelembe vételével kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="51901-136">Intermediating message producers and consumers with a queue means that the consuming application (the worker) only needs to be provisioned to accommodate average load rather than peak load.</span></span> <span data-ttu-id="51901-137">A bejövő terhelés változásával az üzenetsor hossza nő vagy csökken.</span><span class="sxs-lookup"><span data-stu-id="51901-137">The depth of the queue grows and contracts as the incoming load varies.</span></span> <span data-ttu-id="51901-138">Ez közvetlen megtakarításokkal jár az alkalmazásterhelés kiszolgálásához szükséges infrastruktúraméret költségei tekintetében.</span><span class="sxs-lookup"><span data-stu-id="51901-138">This directly saves money in terms of the amount of infrastructure required to service the application load.</span></span>
* <span data-ttu-id="51901-139">**Terheléselosztás.**</span><span class="sxs-lookup"><span data-stu-id="51901-139">**Load balancing.**</span></span> <span data-ttu-id="51901-140">A terhelés növekedésével további feldolgozó folyamatok adhatók hozzá az üzenetsorból való olvasásra.</span><span class="sxs-lookup"><span data-stu-id="51901-140">As load increases, more worker processes can be added to read from the queue.</span></span> <span data-ttu-id="51901-141">Az egyes üzeneteket a feldolgozó folyamatoknak csak az egyike dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="51901-141">Each message is processed by only one of the worker processes.</span></span> <span data-ttu-id="51901-142">Ez a lekérésalapú terheléselosztás akkor is lehetővé teszi a feldolgozó gépek optimális használatát, ha azok feldolgozási teljesítménye eltérő, mivel az egyes gépek az üzeneteket a saját maximális sebességüknek megfelelően kérik le.</span><span class="sxs-lookup"><span data-stu-id="51901-142">Furthermore, this pull-based load balancing enables optimal use of the worker machines even if the worker machines differ in terms of processing power, as they will pull messages at their own maximum rate.</span></span> <span data-ttu-id="51901-143">Ezt a mintát gyakran a *versengő felhasználó* mintának hívják.</span><span class="sxs-lookup"><span data-stu-id="51901-143">This pattern is often termed the *competing consumer* pattern.</span></span>
  
  ![][2]

<span data-ttu-id="51901-144">Az alábbi szakaszok az architektúrát megvalósító kódot ismertetik.</span><span class="sxs-lookup"><span data-stu-id="51901-144">The following sections discuss the code that implements this architecture.</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="51901-145">A fejlesztési környezet kialakítása</span><span class="sxs-lookup"><span data-stu-id="51901-145">Set up the development environment</span></span>
<span data-ttu-id="51901-146">Az Azure-alkalmazások fejlesztésének megkezdése előtt szerezze be az eszközöket és állítsa be a fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="51901-146">Before you can begin developing Azure applications, get the tools and set up your development environment.</span></span>

1. <span data-ttu-id="51901-147">Telepítse az Azure SDK for .NET-et az SDK [letöltési oldaláról](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="51901-147">Install the Azure SDK for .NET from the SDK [downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="51901-148">A **.NET** oszlopban kattintson a használt [Visual Studio](http://www.visualstudio.com)-verzióra.</span><span class="sxs-lookup"><span data-stu-id="51901-148">In the **.NET** column, click the version of [Visual Studio](http://www.visualstudio.com) you are using.</span></span> <span data-ttu-id="51901-149">A jelen oktatóanyagban szereplő lépések a Visual Studio 2015-öt használják, de ezek a Visual Studio 2017-ben is működnek.</span><span class="sxs-lookup"><span data-stu-id="51901-149">The steps in this tutorial use Visual Studio 2015, but they also work with Visual Studio 2017.</span></span>
3. <span data-ttu-id="51901-150">A telepítő futtatásának vagy mentésének kérdésére válaszolva kattintson a **Futtatás** gombra.</span><span class="sxs-lookup"><span data-stu-id="51901-150">When prompted to run or save the installer, click **Run**.</span></span>
4. <span data-ttu-id="51901-151">A **Webplatform-telepítőben** kattintson a **Telepítés** gombra, és folytassa a telepítést.</span><span class="sxs-lookup"><span data-stu-id="51901-151">In the **Web Platform Installer**, click **Install** and proceed with the installation.</span></span>
5. <span data-ttu-id="51901-152">A telepítés végén az alkalmazás fejlesztésének megkezdéséhez szükséges összes eszközzel rendelkezni fog.</span><span class="sxs-lookup"><span data-stu-id="51901-152">Once the installation is complete, you will have everything necessary to start to develop the app.</span></span> <span data-ttu-id="51901-153">Az SDK olyan eszközöket tartalmaz, amelyekkel könnyedén fejleszthet Azure-alkalmazásokat a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="51901-153">The SDK includes tools that let you easily develop Azure applications in Visual Studio.</span></span>

## <a name="create-a-namespace"></a><span data-ttu-id="51901-154">Névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="51901-154">Create a namespace</span></span>
<span data-ttu-id="51901-155">A következő lépés egy szolgáltatásnévtér létrehozása, valamint egy közös hozzáférésű jogosultságkód (SAS-) kulcs beszerzése.</span><span class="sxs-lookup"><span data-stu-id="51901-155">The next step is to create a service namespace, and obtain a Shared Access Signature (SAS) key.</span></span> <span data-ttu-id="51901-156">A névtér egy alkalmazáshatárt biztosít a Service Buson keresztül közzétett minden alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="51901-156">A namespace provides an application boundary for each application exposed through Service Bus.</span></span> <span data-ttu-id="51901-157">Az SAS-kulcsot a rendszer állítja elő a névtér létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="51901-157">A SAS key is generated by the system when a namespace is created.</span></span> <span data-ttu-id="51901-158">A névtér és az SAS-kulcs együttes használata hitelesítő adatokat biztosít a Service Bus számára, amellyel hitelesíti a hozzáférést egy alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="51901-158">The combination of namespace and SAS key provides the credentials for Service Bus to authenticate access to an application.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a><span data-ttu-id="51901-159">Webes szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="51901-159">Create a web role</span></span>
<span data-ttu-id="51901-160">Ebben a szakaszban az alkalmazás előtérrendszerét hozza létre.</span><span class="sxs-lookup"><span data-stu-id="51901-160">In this section, you build the front end of your application.</span></span> <span data-ttu-id="51901-161">Először létrehozza az alkalmazás által megjelenített oldalakat.</span><span class="sxs-lookup"><span data-stu-id="51901-161">First, you create the pages that your application displays.</span></span>
<span data-ttu-id="51901-162">Ezt követően hozzáadja a kódot, amely elemeket küld el a Service Bus-üzenetsorba, és megjeleníti az üzenetsor állapotára vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="51901-162">After that, add code that submits items to a Service Bus queue and displays status information about the queue.</span></span>

### <a name="create-the-project"></a><span data-ttu-id="51901-163">A projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="51901-163">Create the project</span></span>
1. <span data-ttu-id="51901-164">Rendszergazdai jogosultságokkal indítsa el a Visual Studio alkalmazást: kattintson a jobb gombbal a **Visual Studio** programikonra, majd kattintson a **Futtatás rendszergazdaként** parancsra.</span><span class="sxs-lookup"><span data-stu-id="51901-164">Using administrator privileges, start Visual Studio: right-click the **Visual Studio** program icon, and then click **Run as administrator**.</span></span> <span data-ttu-id="51901-165">A cikkben korábban tárgyalt Azure Compute Emulatorhoz a Visual Studiót rendszergazdai jogosultságokkal kell elindítani.</span><span class="sxs-lookup"><span data-stu-id="51901-165">The Azure compute emulator, discussed later in this article, requires that Visual Studio be started with administrator privileges.</span></span>
   
   <span data-ttu-id="51901-166">A Visual Studio programban, a **File** (Fájl) menüben kattintson a **New** (Új) elemre, majd kattintson a **Project** (Projekt) elemre.</span><span class="sxs-lookup"><span data-stu-id="51901-166">In Visual Studio, on the **File** menu, click **New**, and then click **Project**.</span></span>
2. <span data-ttu-id="51901-167">Az **Installed Templates** (Telepített sablonok) lap **Visual C#** területén kattintson a **Cloud** (Felhő), majd az **Azure Cloud Service** (Azure-felhőszolgáltatás) elemre.</span><span class="sxs-lookup"><span data-stu-id="51901-167">From **Installed Templates**, under **Visual C#**, click **Cloud** and then click **Azure Cloud Service**.</span></span> <span data-ttu-id="51901-168">Adja a projektnek a **MultiTierApp** nevet.</span><span class="sxs-lookup"><span data-stu-id="51901-168">Name the project **MultiTierApp**.</span></span> <span data-ttu-id="51901-169">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="51901-169">Then click **OK**.</span></span>
   
   ![][9]
3. <span data-ttu-id="51901-170">A **.NET Framework 4.5** (.NET-keretrendszer 4.5) szerepkörök között kattintson duplán az **ASP.NET Web Role** (ASP.NET webes szerepkör) elemre.</span><span class="sxs-lookup"><span data-stu-id="51901-170">From **.NET Framework 4.5** roles, double-click **ASP.NET Web Role**.</span></span>
   
   ![][10]
4. <span data-ttu-id="51901-171">Vigye a mutatót a **WebRole1** elem fölé az **Azure Cloud Service solution** (Azure-felhőszolgáltatási megoldás) alatt, kattintson a ceruza ikonra, és írja át a webes szerepkör nevét a következőre: **FrontendWebRole**.</span><span class="sxs-lookup"><span data-stu-id="51901-171">Hover over **WebRole1** under **Azure Cloud Service solution**, click the pencil icon, and rename the web role to **FrontendWebRole**.</span></span> <span data-ttu-id="51901-172">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="51901-172">Then click **OK**.</span></span> <span data-ttu-id="51901-173">(Ügyeljen, hogy a „Frontend” nevet kis „e” betűvel írja, és ne „FrontEnd” formában.)</span><span class="sxs-lookup"><span data-stu-id="51901-173">(Make sure you enter "Frontend" with a lower-case 'e,' not "FrontEnd".)</span></span>
   
   ![][11]
5. <span data-ttu-id="51901-174">A **New ASP.NET Project** (Új ASP.NET-projekt) párbeszédpanel **Select a template** (Sablon kiválasztása) listáján kattintson az **MVC** elemre.</span><span class="sxs-lookup"><span data-stu-id="51901-174">From the **New ASP.NET Project** dialog box, in the **Select a template** list, click **MVC**.</span></span>
   
   ![][12]
6. <span data-ttu-id="51901-175">Továbbra is a **New ASP.NET Project** (Új ASP.NET-projekt) párbeszédpanelen kattintson a **Change Authentication** (Hitelesítés módosítása) gombra.</span><span class="sxs-lookup"><span data-stu-id="51901-175">Still in the **New ASP.NET Project** dialog box, click the **Change Authentication** button.</span></span> <span data-ttu-id="51901-176">A **Change Authentication** (Hitelesítés módosítása) párbeszédpanelen kattintson a **No Authentication** (Nincs hitelesítés) elemre, majd az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="51901-176">In the **Change Authentication** dialog box, click **No Authentication**, and then click **OK**.</span></span> <span data-ttu-id="51901-177">Ebben az oktatóanyaghoz egy olyan alkalmazást hoz létre, amelyhez nincs szükség felhasználói bejelentkezésre.</span><span class="sxs-lookup"><span data-stu-id="51901-177">For this tutorial, you're deploying an app that doesn't need a user login.</span></span>
   
    ![][16]
7. <span data-ttu-id="51901-178">A **New ASP.NET Project** (Új ASP.NET-projekt) párbeszédpanelen kattintson az **OK** gombra a projekt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="51901-178">Back in the **New ASP.NET Project** dialog box, click **OK** to create the project.</span></span>
8. <span data-ttu-id="51901-179">A **Megoldáskezelőben** a **FrontendWebRole** projektben kattintson a jobb gombbal a **References** (Hivatkozások) elemre, majd kattintson a **Manage NuGet Packages** (NuGet-csomagok kezelése) parancsra.</span><span class="sxs-lookup"><span data-stu-id="51901-179">In **Solution Explorer**, in the **FrontendWebRole** project, right-click **References**, then click **Manage NuGet Packages**.</span></span>
9. <span data-ttu-id="51901-180">Kattintson a **Browse** (Tallózás) lapra, és keressen a következőre: `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="51901-180">Click the **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="51901-181">Válassza ki a **WindowsAzure.ServiceBus** csomagot, kattintson a **Telepítés** elemre, és fogadja el a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="51901-181">Select the **WindowsAzure.ServiceBus** package, click **Install**, and accept the terms of use.</span></span>
   
   ![][13]
   
   <span data-ttu-id="51901-182">Vegye figyelembe, hogy a rendszer létrehozta a szükséges ügyfélszerelvényekre mutató hivatkozásokat, és hozzáadott néhány új kódfájlt.</span><span class="sxs-lookup"><span data-stu-id="51901-182">Note that the required client assemblies are now referenced and some new code files have been added.</span></span>
10. <span data-ttu-id="51901-183">A **Megoldáskezelőben** kattintson a jobb gombbal a **Models** (Modellek) elemre, kattintson az **Add** (Hozzáadás) parancsra, majd kattintson a **Class** (Osztály) elemre.</span><span class="sxs-lookup"><span data-stu-id="51901-183">In **Solution Explorer**, right-click **Models** and click **Add**, then click **Class**.</span></span> <span data-ttu-id="51901-184">A **Name** (Név) mezőbe írja be az **OnlineOrder.cs** nevet.</span><span class="sxs-lookup"><span data-stu-id="51901-184">In the **Name** box, type the name **OnlineOrder.cs**.</span></span> <span data-ttu-id="51901-185">Ezután kattintson az **Add** (Hozzáadás) gombra.</span><span class="sxs-lookup"><span data-stu-id="51901-185">Then click **Add**.</span></span>

### <a name="write-the-code-for-your-web-role"></a><span data-ttu-id="51901-186">A webes szerepkör kódjának megírása</span><span class="sxs-lookup"><span data-stu-id="51901-186">Write the code for your web role</span></span>
<span data-ttu-id="51901-187">Ebben a szakaszban az alkalmazás által megjelenített különféle oldalakat hozza létre.</span><span class="sxs-lookup"><span data-stu-id="51901-187">In this section, you create the various pages that your application displays.</span></span>

1. <span data-ttu-id="51901-188">A Visual Studióban az OnlineOrder.cs fájlban cserélje le a meglévő névtér-definíciót az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="51901-188">In the OnlineOrder.cs file in Visual Studio, replace the existing namespace definition with the following code:</span></span>
   
   ```csharp
   namespace FrontendWebRole.Models
   {
       public class OnlineOrder
       {
           public string Customer { get; set; }
           public string Product { get; set; }
       }
   }
   ```
2. <span data-ttu-id="51901-189">A **Megoldáskezelőben** kattintson duplán a **Controllers\HomeController.cs** elemre.</span><span class="sxs-lookup"><span data-stu-id="51901-189">In **Solution Explorer**, double-click **Controllers\HomeController.cs**.</span></span> <span data-ttu-id="51901-190">Adja hozzá az alábbi **using** utasításokat a fájl elejéhez a névtereknek az imént létrehozott modellbe, valamint a Service Busba való foglalásához.</span><span class="sxs-lookup"><span data-stu-id="51901-190">Add the following **using** statements at the top of the file to include the namespaces for the model you just created, as well as Service Bus.</span></span>
   
   ```csharp
   using FrontendWebRole.Models;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   ```
3. <span data-ttu-id="51901-191">Szintén a Visual Studióban a HomeController.cs fájlban cserélje le a meglévő névtér-definíciót az alábbi kódra.</span><span class="sxs-lookup"><span data-stu-id="51901-191">Also in the HomeController.cs file in Visual Studio, replace the existing namespace definition with the following code.</span></span> <span data-ttu-id="51901-192">A kód az elemeknek az üzenetsorba való küldésének kezelésére vonatkozó metódusokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="51901-192">This code contains methods for handling the submission of items to the queue.</span></span>
   
   ```csharp
   namespace FrontendWebRole.Controllers
   {
       public class HomeController : Controller
       {
           public ActionResult Index()
           {
               // Simply redirect to Submit, since Submit will serve as the
               // front page of this application.
               return RedirectToAction("Submit");
           }
   
           public ActionResult About()
           {
               return View();
           }
   
           // GET: /Home/Submit.
           // Controller method for a view you will create for the submission
           // form.
           public ActionResult Submit()
           {
               // Will put code for displaying queue message count here.
   
               return View();
           }
   
           // POST: /Home/Submit.
           // Controller method for handling submissions from the submission
           // form.
           [HttpPost]
           // Attribute to help prevent cross-site scripting attacks and
           // cross-site request forgery.  
           [ValidateAntiForgeryToken]
           public ActionResult Submit(OnlineOrder order)
           {
               if (ModelState.IsValid)
               {
                   // Will put code for submitting to queue here.
   
                   return RedirectToAction("Submit");
               }
               else
               {
                   return View(order);
               }
           }
       }
   }
   ```
4. <span data-ttu-id="51901-193">A **Build** (Létrehozás) menüben kattintson a **Build Solution** (Megoldás létrehozása) elemre az eddigi munkája pontosságának ellenőrzésére.</span><span class="sxs-lookup"><span data-stu-id="51901-193">From the **Build** menu, click **Build Solution** to test the accuracy of your work so far.</span></span>
5. <span data-ttu-id="51901-194">Most hozza létre a korábban létrehozott `Submit()` metódus nézetét.</span><span class="sxs-lookup"><span data-stu-id="51901-194">Now, create the view for the `Submit()` method you created earlier.</span></span> <span data-ttu-id="51901-195">Kattintson a jobb gombbal a `Submit()` metódusban (a paraméterekkel nem rendelkező `Submit()`-túlterhelésbe), majd válassza az **Add View** (Nézet hozzáadása) elemet.</span><span class="sxs-lookup"><span data-stu-id="51901-195">Right-click within the `Submit()` method (the overload of `Submit()` that takes no parameters), and then choose **Add View**.</span></span>
   
   ![][14]
6. <span data-ttu-id="51901-196">Megjelenik egy párbeszédpanel a nézet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="51901-196">A dialog box appears for creating the view.</span></span> <span data-ttu-id="51901-197">A **Template** (Sablon) listában válassza a **Create** (Létrehozás) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="51901-197">In the **Template** list, choose **Create**.</span></span> <span data-ttu-id="51901-198">A **Model class** (Modellosztály) listában kattintson az **OnlineOrder** osztályra.</span><span class="sxs-lookup"><span data-stu-id="51901-198">In the **Model class** list, click the **OnlineOrder** class.</span></span>
   
   ![][15]
7. <span data-ttu-id="51901-199">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="51901-199">Click **Add**.</span></span>
8. <span data-ttu-id="51901-200">Módosítsa az alkalmazás megjelenő nevét.</span><span class="sxs-lookup"><span data-stu-id="51901-200">Now, change the displayed name of your application.</span></span> <span data-ttu-id="51901-201">A **Megoldáskezelőben** kattintson duplán a **Views\Shared\\_Layout.cshtml** fájlra a Visual Studio-szerkesztőben való megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="51901-201">In **Solution Explorer**, double-click the **Views\Shared\\_Layout.cshtml** file to open it in the Visual Studio editor.</span></span>
9. <span data-ttu-id="51901-202">Cserélje le a **My ASP.NET Application** (Saját ASP.NET-alkalmazás) minden előfordulását **LITWARE's Products** (LITWARE-termékek) értékre.</span><span class="sxs-lookup"><span data-stu-id="51901-202">Replace all occurrences of **My ASP.NET Application** with **LITWARE'S Products**.</span></span>
10. <span data-ttu-id="51901-203">Távolítsa el a **Home** (Kezdőlap), **About** (Névjegy) és **Contact** (Kapcsolatfelvétel) hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="51901-203">Remove the **Home**, **About**, and **Contact** links.</span></span> <span data-ttu-id="51901-204">Törölje a kiemelt kódot:</span><span class="sxs-lookup"><span data-stu-id="51901-204">Delete the highlighted code:</span></span>
    
    ![][28]
11. <span data-ttu-id="51901-205">Végül módosítsa úgy az elküldési lapot, hogy az megjelenítse az üzenetsorral kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="51901-205">Finally, modify the submission page to include some information about the queue.</span></span> <span data-ttu-id="51901-206">A **Megoldáskezelőben** kattintson duplán a **Views\Home\Submit.cshtml** fájlra a Visual Studio-szerkesztőben való megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="51901-206">In **Solution Explorer**, double-click the **Views\Home\Submit.cshtml** file to open it in the Visual Studio editor.</span></span> <span data-ttu-id="51901-207">Adja hozzá a következő sort a `<h2>Submit</h2>` után.</span><span class="sxs-lookup"><span data-stu-id="51901-207">Add the following line after `<h2>Submit</h2>`.</span></span> <span data-ttu-id="51901-208">A `ViewBag.MessageCount` jelenleg üres.</span><span class="sxs-lookup"><span data-stu-id="51901-208">For now, the `ViewBag.MessageCount` is empty.</span></span> <span data-ttu-id="51901-209">Később fogja majd feltölteni.</span><span class="sxs-lookup"><span data-stu-id="51901-209">You will populate it later.</span></span>
    
    ```html
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```
12. <span data-ttu-id="51901-210">Megvalósította a felhasználói felületet.</span><span class="sxs-lookup"><span data-stu-id="51901-210">You now have implemented your UI.</span></span> <span data-ttu-id="51901-211">Az **F5** billentyű lenyomásával futtathatja az alkalmazást, és ellenőrizheti, hogy várakozásainak megfelelően jelenik-e meg.</span><span class="sxs-lookup"><span data-stu-id="51901-211">You can press **F5** to run your application and confirm that it looks as expected.</span></span>
    
    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a><span data-ttu-id="51901-212">Az elemeknek a Service Bus-üzenetsorba történő elküldésére szolgáló kód megírása</span><span class="sxs-lookup"><span data-stu-id="51901-212">Write the code for submitting items to a Service Bus queue</span></span>
<span data-ttu-id="51901-213">Adja hozzá az elemeknek a Service Bus-üzenetsorba történő elküldésére szolgáló kódot.</span><span class="sxs-lookup"><span data-stu-id="51901-213">Now, add code for submitting items to a queue.</span></span> <span data-ttu-id="51901-214">Először hozza létre a Service Bus-üzenetsor kapcsolati adatait tartalmazó osztályt.</span><span class="sxs-lookup"><span data-stu-id="51901-214">First, you create a class that contains your Service Bus queue connection information.</span></span> <span data-ttu-id="51901-215">Ezután inicializálja a kapcsolatot a Global.aspx.cs osztályból.</span><span class="sxs-lookup"><span data-stu-id="51901-215">Then, initialize your connection from Global.aspx.cs.</span></span> <span data-ttu-id="51901-216">Végül frissítse a korábban a HomeController.cs osztályban létrehozott elküldési kódot az elemek tényleges elküldéséhez a Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="51901-216">Finally, update the submission code you created earlier in HomeController.cs to actually submit items to a Service Bus queue.</span></span>

1. <span data-ttu-id="51901-217">A **Megoldáskezelőben** kattintson a jobb gombbal a **FrontendWebRole** projektre (a projektre, ne a szerepkörre kattintson a jobb gombbal).</span><span class="sxs-lookup"><span data-stu-id="51901-217">In **Solution Explorer**, right-click **FrontendWebRole** (right-click the project, not the role).</span></span> <span data-ttu-id="51901-218">Kattintson az **Add** (Hozzáadás), majd a **Class** (Osztály) elemre.</span><span class="sxs-lookup"><span data-stu-id="51901-218">Click **Add**, and then click **Class**.</span></span>
2. <span data-ttu-id="51901-219">Adja az osztálynak a **QueueConnector.cs** nevet.</span><span class="sxs-lookup"><span data-stu-id="51901-219">Name the class **QueueConnector.cs**.</span></span> <span data-ttu-id="51901-220">Kattintson az **Add** (Hozzáadás) gombra az osztály létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="51901-220">Click **Add** to create the class.</span></span>
3. <span data-ttu-id="51901-221">Adja hozzá a kapcsolati adatokat tartalmazó és a Service Bus-üzenetsorral létesített kapcsolatot inicializáló kódot.</span><span class="sxs-lookup"><span data-stu-id="51901-221">Now, add code that encapsulates the connection information and initializes the connection to a Service Bus queue.</span></span> <span data-ttu-id="51901-222">Cserélje le a QueueConnector.cs teljes tartalmát a következő kódra, és adjon meg értéket a `your Service Bus namespace` (névtér neve) és a `yourKey` számára, amely az az **elsődleges kulcs**, amelyet korábban az Azure Portalon szerzett be.</span><span class="sxs-lookup"><span data-stu-id="51901-222">Replace the entire contents of QueueConnector.cs with the following code, and enter values for `your Service Bus namespace` (your namespace name) and `yourKey`, which is the **primary key** you previously obtained from the Azure portal.</span></span>
   
   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Web;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   
   namespace FrontendWebRole
   {
       public static class QueueConnector
       {
           // Thread-safe. Recommended that you cache rather than recreating it
           // on every request.
           public static QueueClient OrdersQueueClient;
   
           // Obtain these values from the portal.
           public const string Namespace = "your Service Bus namespace";
   
           // The name of your queue.
           public const string QueueName = "OrdersQueue";
   
           public static NamespaceManager CreateNamespaceManager()
           {
               // Create the namespace manager which gives you access to
               // management operations.
               var uri = ServiceBusEnvironment.CreateServiceUri(
                   "sb", Namespace, String.Empty);
               var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                   "RootManageSharedAccessKey", "yourKey");
               return new NamespaceManager(uri, tP);
           }
   
           public static void Initialize()
           {
               // Using Http to be friendly with outbound firewalls.
               ServiceBusEnvironment.SystemConnectivity.Mode =
                   ConnectivityMode.Http;
   
               // Create the namespace manager which gives you access to
               // management operations.
               var namespaceManager = CreateNamespaceManager();
   
               // Create the queue if it does not exist already.
               if (!namespaceManager.QueueExists(QueueName))
               {
                   namespaceManager.CreateQueue(QueueName);
               }
   
               // Get a client to the queue.
               var messagingFactory = MessagingFactory.Create(
                   namespaceManager.Address,
                   namespaceManager.Settings.TokenProvider);
               OrdersQueueClient = messagingFactory.CreateQueueClient(
                   "OrdersQueue");
           }
       }
   }
   ```
4. <span data-ttu-id="51901-223">Ellenőrizze, hogy az **Initialize** metódus meghívása megtörténik.</span><span class="sxs-lookup"><span data-stu-id="51901-223">Now, ensure that your **Initialize** method gets called.</span></span> <span data-ttu-id="51901-224">A **Megoldáskezelőben** kattintson duplán a **Global.asax\Global.asax.cs** elemre.</span><span class="sxs-lookup"><span data-stu-id="51901-224">In **Solution Explorer**, double-click **Global.asax\Global.asax.cs**.</span></span>
5. <span data-ttu-id="51901-225">Adja hozzá az alábbi kódsort az **Application_Start** metódus végén.</span><span class="sxs-lookup"><span data-stu-id="51901-225">Add the following line of code at the end of the **Application_Start** method.</span></span>
   
   ```csharp
   FrontendWebRole.QueueConnector.Initialize();
   ```
6. <span data-ttu-id="51901-226">Végül frissítse a korábban létrehozott webkódot az elemek elküldéséhez az üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="51901-226">Finally, update the web code you created earlier, to submit items to the queue.</span></span> <span data-ttu-id="51901-227">A **Megoldáskezelőben** kattintson duplán a **Controllers\HomeController.cs** elemre.</span><span class="sxs-lookup"><span data-stu-id="51901-227">In **Solution Explorer**, double-click **Controllers\HomeController.cs**.</span></span>
7. <span data-ttu-id="51901-228">Frissítse a `Submit()` metódust (a paraméterekkel nem rendelkező túlterhelést) az alábbiak szerint, hogy megkapja az üzenetsorban lévő üzenetek számát.</span><span class="sxs-lookup"><span data-stu-id="51901-228">Update the `Submit()` method (the overload that takes no parameters) as follows to get the message count for the queue.</span></span>
   
   ```csharp
   public ActionResult Submit()
   {
       // Get a NamespaceManager which allows you to perform management and
       // diagnostic operations on your Service Bus queues.
       var namespaceManager = QueueConnector.CreateNamespaceManager();
   
       // Get the queue, and obtain the message count.
       var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
       ViewBag.MessageCount = queue.MessageCount;
   
       return View();
   }
   ```
8. <span data-ttu-id="51901-229">Frissítse a `Submit(OnlineOrder order)` metódust (az egy paraméterrel rendelkező túlterhelést) az alábbiak szerint, hogy elküldje a rendelésinformációkat az üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="51901-229">Update the `Submit(OnlineOrder order)` method (the overload that takes one parameter) as follows to submit order information to the queue.</span></span>
   
   ```csharp
   public ActionResult Submit(OnlineOrder order)
   {
       if (ModelState.IsValid)
       {
           // Create a message from the order.
           var message = new BrokeredMessage(order);
   
           // Submit the order.
           QueueConnector.OrdersQueueClient.Send(message);
           return RedirectToAction("Submit");
       }
       else
       {
           return View(order);
       }
   }
   ```
9. <span data-ttu-id="51901-230">Most ismét futtathatja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="51901-230">You can now run the application again.</span></span> <span data-ttu-id="51901-231">Minden egyes alkalommal, amikor elküld egy rendelést, az üzenetek száma nőni fog.</span><span class="sxs-lookup"><span data-stu-id="51901-231">Each time you submit an order, the message count increases.</span></span>
   
   ![][18]

## <a name="create-the-worker-role"></a><span data-ttu-id="51901-232">A feldolgozói szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="51901-232">Create the worker role</span></span>
<span data-ttu-id="51901-233">Most létrehozza a feldolgozói szerepkört, amely feldolgozza az elküldött rendeléseket.</span><span class="sxs-lookup"><span data-stu-id="51901-233">You will now create the worker role that processes the order submissions.</span></span> <span data-ttu-id="51901-234">Ez a példa a **Worker Role with Service Bus Queue** (Feldolgozói szerepkör Service Bus-üzenetsorral) Visual Studio-projektsablont használja.</span><span class="sxs-lookup"><span data-stu-id="51901-234">This example uses the **Worker Role with Service Bus Queue** Visual Studio project template.</span></span> <span data-ttu-id="51901-235">A szükséges hitelesítő adatokat már beszerezte a portálról.</span><span class="sxs-lookup"><span data-stu-id="51901-235">You already obtained the required credentials from the portal.</span></span>

1. <span data-ttu-id="51901-236">Ellenőrizze, hogy társította-e a Visual Studiót az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="51901-236">Make sure you have connected Visual Studio to your Azure account.</span></span>
2. <span data-ttu-id="51901-237">A Visual Studio **Megoldáskezelőjében** kattintson a jobb gombbal a **Roles** (Szerepkörök) mappára a **MultiTierApp** projekt alatt.</span><span class="sxs-lookup"><span data-stu-id="51901-237">In Visual Studio, in **Solution Explorer** right-click the **Roles** folder under the **MultiTierApp** project.</span></span>
3. <span data-ttu-id="51901-238">Kattintson az **Add** (Hozzáadás), majd a **New Worker Role Project** (Új feldolgozói szerepkör projekt) elemre.</span><span class="sxs-lookup"><span data-stu-id="51901-238">Click **Add**, and then click **New Worker Role Project**.</span></span> <span data-ttu-id="51901-239">Megjelenik az **Add New Role Project** (Új szerepkör projekt hozzáadása) párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="51901-239">The **Add New Role Project** dialog box appears.</span></span>
   
   ![][26]
4. <span data-ttu-id="51901-240">Az **Add New Role Project** (Új szerepkör projekt hozzáadása) párbeszédpanelen kattintson a **Worker Role with Service Bus Queue** (Feldolgozói szerepkör Service Bus-üzenetsorral) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="51901-240">In the **Add New Role Project** dialog box, click **Worker Role with Service Bus Queue**.</span></span>
   
   ![][23]
5. <span data-ttu-id="51901-241">A **Name** (Név) mezőben adja az **OrderProcessingRole** nevet a projektnek.</span><span class="sxs-lookup"><span data-stu-id="51901-241">In the **Name** box, name the project **OrderProcessingRole**.</span></span> <span data-ttu-id="51901-242">Ezután kattintson az **Add** (Hozzáadás) gombra.</span><span class="sxs-lookup"><span data-stu-id="51901-242">Then click **Add**.</span></span>
6. <span data-ttu-id="51901-243">Másolja a „Service Bus-névtér létrehozása” szakasz 9. lépésében beszerzett kapcsolati karakterláncot a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="51901-243">Copy the connection string that you obtained in step 9 of the "Create a Service Bus namespace" section to the clipboard.</span></span>
7. <span data-ttu-id="51901-244">A **Megoldáskezelőben** kattintson a jobb gombbal az 5. lépésben létrehozott **OrderProcessingRole** szerepkörre (az **OrderProcessingRole** szerepkörre kattintson a jobb gombbal a **Roles** (Szerepkörök) részen, és ne az osztályra).</span><span class="sxs-lookup"><span data-stu-id="51901-244">In **Solution Explorer**, right-click the **OrderProcessingRole** you created in step 5 (make sure that you right-click **OrderProcessingRole** under **Roles**, and not the class).</span></span> <span data-ttu-id="51901-245">Ezután kattintson a **Properties** (Tulajdonságok) elemre.</span><span class="sxs-lookup"><span data-stu-id="51901-245">Then click **Properties**.</span></span>
8. <span data-ttu-id="51901-246">A **Properties** (Tulajdonságok) párbeszédpanel **Settings** (Beállítások) lapján kattintson a **Microsoft.ServiceBus.ConnectionString** **Value** (Érték) mezőjébe, és illessze be a 6. lépésben másolt végpontértéket.</span><span class="sxs-lookup"><span data-stu-id="51901-246">On the **Settings** tab of the **Properties** dialog box, click inside the **Value** box for **Microsoft.ServiceBus.ConnectionString**, and then paste the endpoint value you copied in step 6.</span></span>
   
   ![][25]
9. <span data-ttu-id="51901-247">Hozza létre az **OnlineOrder** osztályt az üzenetsorból feldolgozott rendelések jelölésére.</span><span class="sxs-lookup"><span data-stu-id="51901-247">Create an **OnlineOrder** class to represent the orders as you process them from the queue.</span></span> <span data-ttu-id="51901-248">Használhat egy korábban létrehozott osztályt.</span><span class="sxs-lookup"><span data-stu-id="51901-248">You can reuse a class you have already created.</span></span> <span data-ttu-id="51901-249">A **Megoldáskezelőben** kattintson a jobb gombbal az **OrderProcessingRole** osztályra (az osztály ikonjára, ne a szerepkörre kattintson a jobb gombbal).</span><span class="sxs-lookup"><span data-stu-id="51901-249">In **Solution Explorer**, right-click the **OrderProcessingRole** class (right-click the class icon, not the role).</span></span> <span data-ttu-id="51901-250">Kattintson az **Add** (Hozzáadás), majd az **Existing Item** (Meglévő elem) elemre.</span><span class="sxs-lookup"><span data-stu-id="51901-250">Click **Add**, then click **Existing Item**.</span></span>
10. <span data-ttu-id="51901-251">Nyissa meg a **FrontendWebRole\Models** almappát, majd kattintson duplán az **OnlineOrder.cs** elemre a projekthez való hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="51901-251">Browse to the subfolder for **FrontendWebRole\Models**, and then double-click **OnlineOrder.cs** to add it to this project.</span></span>
11. <span data-ttu-id="51901-252">A **WorkerRole.cs** osztályban az alábbi kódban látható módon módosítsa a **QueueName** változó `"ProcessingQueue"` értékét `"OrdersQueue"` értékre.</span><span class="sxs-lookup"><span data-stu-id="51901-252">In **WorkerRole.cs**, change the value of the **QueueName** variable from `"ProcessingQueue"` to `"OrdersQueue"` as shown in the following code.</span></span>
    
    ```csharp
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```
12. <span data-ttu-id="51901-253">Adja hozzá a következő using utasítást a WorkerRole.cs fájl elejéhez.</span><span class="sxs-lookup"><span data-stu-id="51901-253">Add the following using statement at the top of the WorkerRole.cs file.</span></span>
    
    ```csharp
    using FrontendWebRole.Models;
    ```
13. <span data-ttu-id="51901-254">Az `OnMessage()` hívás `Run()` függvényében cserélje le a `try` záradék tartalmát az alábbi kódra.</span><span class="sxs-lookup"><span data-stu-id="51901-254">In the `Run()` function, inside the `OnMessage()` call, replace the contents of the `try` clause with the following code.</span></span>
    
    ```csharp
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```
14. <span data-ttu-id="51901-255">Befejezte az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="51901-255">You have completed the application.</span></span> <span data-ttu-id="51901-256">A teljes alkalmazás teszteléséhez kattintson a jobb gombbal a MultiTierApp projektre a Megoldáskezelőben, válassza a **Set as Startup Project** (Beállítás kezdőprojektként) lehetőséget, majd nyomja le az F5 billentyűt.</span><span class="sxs-lookup"><span data-stu-id="51901-256">You can test the full application by right-clicking the MultiTierApp project in Solution Explorer, selecting **Set as Startup Project**, and then pressing F5.</span></span> <span data-ttu-id="51901-257">Láthatja, hogy az üzenetek száma nem nő, mert a feldolgozói szerepkör feldolgozza az üzenetsorban lévő elemeket, és befejezettként jelöli meg azokat.</span><span class="sxs-lookup"><span data-stu-id="51901-257">Note that the message count does not increment, because the worker role processes items from the queue and marks them as complete.</span></span> <span data-ttu-id="51901-258">A feldolgozói szerepkör nyomkövetési kimenetét az Azure Compute Emulator felhasználói felületén tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="51901-258">You can see the trace output of your worker role by viewing the Azure Compute Emulator UI.</span></span> <span data-ttu-id="51901-259">Ehhez kattintson a jobb gombbal az emulátor ikonjára a tálca értesítési területén, és válassza a **Show Compute Emulator UI** (A Compute Emulator felhasználói felületének megjelenítése) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="51901-259">You can do this by right-clicking the emulator icon in the notification area of your taskbar and selecting **Show Compute Emulator UI**.</span></span>
    
    ![][19]
    
    ![][20]

## <a name="next-steps"></a><span data-ttu-id="51901-260">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51901-260">Next steps</span></span>
<span data-ttu-id="51901-261">A Service Busról a következő forrásanyagokban találhat további információkat:</span><span class="sxs-lookup"><span data-stu-id="51901-261">To learn more about Service Bus, see the following resources:</span></span>  

* <span data-ttu-id="51901-262">[Az Azure Service Bus dokumentációja][sbdocs]</span><span class="sxs-lookup"><span data-stu-id="51901-262">[Azure Service Bus documentation][sbdocs]</span></span>  
* <span data-ttu-id="51901-263">[Service Bus szolgáltatás oldala][sbacom]</span><span class="sxs-lookup"><span data-stu-id="51901-263">[Service Bus service page][sbacom]</span></span>  
* <span data-ttu-id="51901-264">[A Service Bus-üzenetsorok használata][sbacomqhowto]</span><span class="sxs-lookup"><span data-stu-id="51901-264">[How to Use Service Bus Queues][sbacomqhowto]</span></span>  

<span data-ttu-id="51901-265">További információ a többrétegű forgatókönyvekkel kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="51901-265">To learn more about multi-tier scenarios, see:</span></span>  

* <span data-ttu-id="51901-266">[Többrétegű .NET-alkalmazások tárolótáblák, üzenetsorok és blobok használatával][mutitierstorage]</span><span class="sxs-lookup"><span data-stu-id="51901-266">[.NET Multi-Tier Application Using Storage Tables, Queues, and Blobs][mutitierstorage]</span></span>  

[0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app.png
[1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
[2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
[9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
[10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
[11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
[12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
[13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
[14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
[15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
[16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
[17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app.png
[18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app2.png

[19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
[20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
[23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
[25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
[26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
[28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

[sbdocs]: /azure/service-bus-messaging/  
[sbacom]: https://azure.microsoft.com/services/service-bus/  
[sbacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
[mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
