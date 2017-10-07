---
title: "Azure Service Bus használatával aaa.NET többrétegű alkalmazást |} Microsoft Docs"
description: "A .NET-oktatóanyaga, amely segít az Azure Service Bus-üzenetsorok toocommunicate rétegek közötti használó többrétegű alkalmazást fejleszthet."
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
ms.openlocfilehash: 485910ff1d3b8b0a709ee14ede32e57cf873829a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a><span data-ttu-id="a65c6-103">Többrétegű .NET-alkalmazás Azure Service Bus-üzenetsorok használatával</span><span class="sxs-lookup"><span data-stu-id="a65c6-103">.NET multi-tier application using Azure Service Bus queues</span></span>
## <a name="introduction"></a><span data-ttu-id="a65c6-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="a65c6-104">Introduction</span></span>
<span data-ttu-id="a65c6-105">Microsoft Azure a Visual Studio használatával könnyen és hello ingyenes Azure SDK for .NET fejlesztés.</span><span class="sxs-lookup"><span data-stu-id="a65c6-105">Developing for Microsoft Azure is easy using Visual Studio and hello free Azure SDK for .NET.</span></span> <span data-ttu-id="a65c6-106">Ez az oktatóanyag bemutatja, hogyan hello lépéseket toocreate használó több Azure-erőforrások a helyi környezetben futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="a65c6-106">This tutorial walks you through hello steps toocreate an application that uses multiple Azure resources running in your local environment.</span></span>

<span data-ttu-id="a65c6-107">Hello következő témák köre:</span><span class="sxs-lookup"><span data-stu-id="a65c6-107">You will learn hello following:</span></span>

* <span data-ttu-id="a65c6-108">Hogyan tooenable a számítógép egyetlen Azure fejlesztési töltse le és telepítse.</span><span class="sxs-lookup"><span data-stu-id="a65c6-108">How tooenable your computer for Azure development with a single download and install.</span></span>
* <span data-ttu-id="a65c6-109">Hogyan toouse Visual Studio toodevelop az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="a65c6-109">How toouse Visual Studio toodevelop for Azure.</span></span>
* <span data-ttu-id="a65c6-110">Hogyan toocreate egy többrétegű alkalmazást az Azure-ban webes és feldolgozói szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="a65c6-110">How toocreate a multi-tier application in Azure using web and worker roles.</span></span>
* <span data-ttu-id="a65c6-111">Hogyan toocommunicate közötti tiers Service Bus-üzenetsorok használatával.</span><span class="sxs-lookup"><span data-stu-id="a65c6-111">How toocommunicate between tiers using Service Bus queues.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

<span data-ttu-id="a65c6-112">Ez az oktatóanyag lesz hozza létre és hello többrétegű alkalmazást futtat egy Azure felhőszolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a65c6-112">In this tutorial you'll build and run hello multi-tier application in an Azure cloud service.</span></span> <span data-ttu-id="a65c6-113">hello előtér egy ASP.NET MVC webes szerepkör, és hello háttér feldolgozói szerepkör Service Bus-üzenetsort használó.</span><span class="sxs-lookup"><span data-stu-id="a65c6-113">hello front end is an ASP.NET MVC web role and hello back end is a worker-role that uses a Service Bus queue.</span></span> <span data-ttu-id="a65c6-114">Létrehozhat olyan webes projekt, telepített tooan a felhőszolgáltatás helyett az Azure webhelyén, a hello előtér ugyanezen többrétegű alkalmazást hello.</span><span class="sxs-lookup"><span data-stu-id="a65c6-114">You can create hello same multi-tier application with hello front end as a web project, that is deployed tooan Azure website instead of a cloud service.</span></span> <span data-ttu-id="a65c6-115">Hello is kipróbálhatja [.NET helyszíni/felhőbeli hibridalkalmazást](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="a65c6-115">You can also try out hello [.NET on-premises/cloud hybrid application](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) tutorial.</span></span>

<span data-ttu-id="a65c6-116">hello következő képernyőfelvétel a hello befejeződött alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a65c6-116">hello following screen shot shows hello completed application.</span></span>

![][0]

## <a name="scenario-overview-inter-role-communication"></a><span data-ttu-id="a65c6-117">Forgatókönyv áttekintése: szerepkörök közötti kommunikáció</span><span class="sxs-lookup"><span data-stu-id="a65c6-117">Scenario overview: inter-role communication</span></span>
<span data-ttu-id="a65c6-118">a feldolgozási, hello előtér felhasználói felületi összetevőnek hello webes szerepkörben futó kérés toosubmit együtt kell működnie hello feldolgozói szerepkörben futó hello középső rétegbeli logikával.</span><span class="sxs-lookup"><span data-stu-id="a65c6-118">toosubmit an order for processing, hello front-end UI component, running in hello web role, must interact with hello middle tier logic running in hello worker role.</span></span> <span data-ttu-id="a65c6-119">A példa Service Bus üzenetkezelés hello-hello rétegek közötti kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="a65c6-119">This example uses Service Bus messaging for hello communication between hello tiers.</span></span>

<span data-ttu-id="a65c6-120">A Service Bus használatával hello webes és a középső réteg között üzenetküldési elválasztja a két összetevőt.</span><span class="sxs-lookup"><span data-stu-id="a65c6-120">Using Service Bus messaging between hello web and middle tiers decouples the two components.</span></span> <span data-ttu-id="a65c6-121">Ezzel szemben toodirect messaging (vagyis TCP- vagy HTTP), hello webes réteg nem csatlakozik közvetlenül; toohello középső réteg hanem a munkaegységeket munka, üzenetekként le a Service Busba, amely megbízhatóan megőrzi azokat, amíg a hello középső réteg kész tooconsume és dolgozza fel őket.</span><span class="sxs-lookup"><span data-stu-id="a65c6-121">In contrast toodirect messaging (that is, TCP or HTTP), hello web tier does not connect toohello middle tier directly; instead it pushes units of work, as messages, into Service Bus, which reliably retains them until hello middle tier is ready tooconsume and process them.</span></span>

<span data-ttu-id="a65c6-122">Service Bus biztosít két entitások toosupport brokered messaging: üzenetsorok és témakörök.</span><span class="sxs-lookup"><span data-stu-id="a65c6-122">Service Bus provides two entities toosupport brokered messaging: queues and topics.</span></span> <span data-ttu-id="a65c6-123">Az üzenetsorok esetében minden üzenetet küldött toohello várólista egyetlen fogadó használja fel.</span><span class="sxs-lookup"><span data-stu-id="a65c6-123">With queues, each message sent toohello queue is consumed by a single receiver.</span></span> <span data-ttu-id="a65c6-124">Témakörök, amelyben minden egyes közzétett üzenetek legyen elérhető tooa előfizetéssel hello témakör hello közzététel/előfizetés mintát támogatják.</span><span class="sxs-lookup"><span data-stu-id="a65c6-124">Topics support hello publish/subscribe pattern in which each published message is made available tooa subscription registered with hello topic.</span></span> <span data-ttu-id="a65c6-125">Az egyes előfizetések logikai módon tartják fenn a saját üzenetsorukat.</span><span class="sxs-lookup"><span data-stu-id="a65c6-125">Each subscription logically maintains its own queue of messages.</span></span> <span data-ttu-id="a65c6-126">Előfizetések, amelyek korlátozzák a hello átadott üzenetek készletét a hello előfizetés várólista toothose hello szűrőnek szűrési szabályokkal is konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="a65c6-126">Subscriptions can also be configured with filter rules that restrict hello set of messages passed to hello subscription queue toothose that match hello filter.</span></span> <span data-ttu-id="a65c6-127">hello alábbi példa Service Bus-üzenetsorok.</span><span class="sxs-lookup"><span data-stu-id="a65c6-127">hello following example uses Service Bus queues.</span></span>

![][1]

<span data-ttu-id="a65c6-128">Ennek a kommunikációs mechanizmusnak több előnye is van a közvetlen üzenettovábbítással szemben:</span><span class="sxs-lookup"><span data-stu-id="a65c6-128">This communication mechanism has several advantages over direct messaging:</span></span>

* <span data-ttu-id="a65c6-129">**Időbeli elválasztás.**</span><span class="sxs-lookup"><span data-stu-id="a65c6-129">**Temporal decoupling.**</span></span> <span data-ttu-id="a65c6-130">Hello aszinkron üzenettovábbítási mintának köszönhetően a létrehozóknak és a felhasználóknak nem kell online hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="a65c6-130">With hello asynchronous messaging pattern, producers and consumers need not be online at hello same time.</span></span> <span data-ttu-id="a65c6-131">A Service Bus megbízhatóan tárolja az üzeneteket, amíg hello fogyasztó fél készen áll a fogadásukra.</span><span class="sxs-lookup"><span data-stu-id="a65c6-131">Service Bus reliably stores messages until hello consuming party is ready to receive them.</span></span> <span data-ttu-id="a65c6-132">Ez lehetővé teszi a hello összetevői hello elosztott alkalmazás toobe, akár önkéntesen, például karbantartási, vagy leválasztása miatt tooa összetevő összeomlása, a rendszer egészének befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="a65c6-132">This enables hello components of hello distributed application toobe disconnected, either voluntarily, for example, for maintenance, or due tooa component crash, without impacting the system as a whole.</span></span> <span data-ttu-id="a65c6-133">Ezenkívül hello fel az alkalmazás csak módosítania kell toocome online hello nap bizonyos időpontjaiban.</span><span class="sxs-lookup"><span data-stu-id="a65c6-133">Furthermore, hello consuming application might only need toocome online during certain times of hello day.</span></span>
* <span data-ttu-id="a65c6-134">**Terheléskiegyenlítés.**</span><span class="sxs-lookup"><span data-stu-id="a65c6-134">**Load leveling.**</span></span> <span data-ttu-id="a65c6-135">Számos alkalmazás rendszerterhelés időnként eltérő, míg egyes Munkaegységek szükséges hello feldolgozási idő jellemzően állandó marad.</span><span class="sxs-lookup"><span data-stu-id="a65c6-135">In many applications, system load varies over time, while hello processing time required for each unit of work is typically constant.</span></span> <span data-ttu-id="a65c6-136">Közé üzenetek létrehozói és felhasználói üzenetsorokat azt jelenti, hogy fel az alkalmazás (hello munkavégző) csak igények toobe hello kiépített csúcsterhelés helyett tooaccommodate átlagos terhelés.</span><span class="sxs-lookup"><span data-stu-id="a65c6-136">Intermediating message producers and consumers with a queue means that hello consuming application (hello worker) only needs toobe provisioned tooaccommodate average load rather than peak load.</span></span> <span data-ttu-id="a65c6-137">hello várólista hello mélysége növekszik és csökken, mivel változik hello bejövő terhelés.</span><span class="sxs-lookup"><span data-stu-id="a65c6-137">hello depth of hello queue grows and contracts as hello incoming load varies.</span></span> <span data-ttu-id="a65c6-138">Ez közvetlen megtakarításokkal pénz infrastruktúra szükséges tooservice hello alkalmazásterhelés hello mennyisége tekintetében.</span><span class="sxs-lookup"><span data-stu-id="a65c6-138">This directly saves money in terms of hello amount of infrastructure required tooservice hello application load.</span></span>
* <span data-ttu-id="a65c6-139">**Terheléselosztás.**</span><span class="sxs-lookup"><span data-stu-id="a65c6-139">**Load balancing.**</span></span> <span data-ttu-id="a65c6-140">A terhelés növekedésével további feldolgozó folyamatok adhatók hozzá tooread hello üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="a65c6-140">As load increases, more worker processes can be added tooread from hello queue.</span></span> <span data-ttu-id="a65c6-141">Minden üzenetet csak az egyik hello munkavégző folyamatok dolgoz fel.</span><span class="sxs-lookup"><span data-stu-id="a65c6-141">Each message is processed by only one of hello worker processes.</span></span> <span data-ttu-id="a65c6-142">Ezenkívül a lekérésalapú terheléselosztás lehetővé teszi, hogy hello feldolgozó gépek optimális használatát akkor is, ha azok feldolgozási teljesítménye eltérő módon kérik le a saját maximális díj üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="a65c6-142">Furthermore, this pull-based load balancing enables optimal use of hello worker machines even if the worker machines differ in terms of processing power, as they will pull messages at their own maximum rate.</span></span> <span data-ttu-id="a65c6-143">Ezt a mintát gyakran nevezik hello *versengő felhasználó* mintát.</span><span class="sxs-lookup"><span data-stu-id="a65c6-143">This pattern is often termed hello *competing consumer* pattern.</span></span>
  
  ![][2]

<span data-ttu-id="a65c6-144">hello a következő szakaszok az architektúrát megvalósító kódot hello tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="a65c6-144">hello following sections discuss hello code that implements this architecture.</span></span>

## <a name="set-up-hello-development-environment"></a><span data-ttu-id="a65c6-145">Hello fejlesztési környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="a65c6-145">Set up hello development environment</span></span>
<span data-ttu-id="a65c6-146">Csak akkor használhatja az Azure-alkalmazások fejlesztésével, hello eszközök lekérni, és állítsa be a fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="a65c6-146">Before you can begin developing Azure applications, get hello tools and set up your development environment.</span></span>

1. <span data-ttu-id="a65c6-147">Hello Azure SDK telepítse a .NET hello SDK [letöltési oldalon](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="a65c6-147">Install hello Azure SDK for .NET from hello SDK [downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="a65c6-148">A hello **.NET** oszlopban kattintson hello verziója [Visual Studio](http://www.visualstudio.com) használ.</span><span class="sxs-lookup"><span data-stu-id="a65c6-148">In hello **.NET** column, click hello version of [Visual Studio](http://www.visualstudio.com) you are using.</span></span> <span data-ttu-id="a65c6-149">hello lépéseit az oktatóanyag Visual Studio 2015-öt használja, de ezek a Visual Studio 2017 is működnek.</span><span class="sxs-lookup"><span data-stu-id="a65c6-149">hello steps in this tutorial use Visual Studio 2015, but they also work with Visual Studio 2017.</span></span>
3. <span data-ttu-id="a65c6-150">Ha toorun kéri, vagy hello telepítő mentéséhez, kattintson **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-150">When prompted toorun or save hello installer, click **Run**.</span></span>
4. <span data-ttu-id="a65c6-151">A hello **Webplatform-telepítő**, kattintson a **telepítése** és hello a telepítés folytatásához.</span><span class="sxs-lookup"><span data-stu-id="a65c6-151">In hello **Web Platform Installer**, click **Install** and proceed with hello installation.</span></span>
5. <span data-ttu-id="a65c6-152">Hello telepítés befejezése után, hogy minden szükséges toostart toodevelop hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a65c6-152">Once hello installation is complete, you will have everything necessary toostart toodevelop hello app.</span></span> <span data-ttu-id="a65c6-153">hello SDK olyan eszközöket tartalmaz, amelyekkel könnyedén fejleszthet Azure-alkalmazásokat a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a65c6-153">hello SDK includes tools that let you easily develop Azure applications in Visual Studio.</span></span>

## <a name="create-a-namespace"></a><span data-ttu-id="a65c6-154">Névtér létrehozása</span><span class="sxs-lookup"><span data-stu-id="a65c6-154">Create a namespace</span></span>
<span data-ttu-id="a65c6-155">következő lépés hello toocreate egy névtér, és egy közös hozzáférésű Jogosultságkód (SAS) kulcs beszerzése.</span><span class="sxs-lookup"><span data-stu-id="a65c6-155">hello next step is toocreate a service namespace, and obtain a Shared Access Signature (SAS) key.</span></span> <span data-ttu-id="a65c6-156">A névtér egy alkalmazáshatárt biztosít a Service Buson keresztül közzétett minden alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="a65c6-156">A namespace provides an application boundary for each application exposed through Service Bus.</span></span> <span data-ttu-id="a65c6-157">Az SAS-kulcsot a hello rendszer jön létre, amikor egy névtér jön létre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-157">A SAS key is generated by hello system when a namespace is created.</span></span> <span data-ttu-id="a65c6-158">névtér és SAS-kulcs hello kombinációja a Service Bus tooauthenticate hozzáférés tooan alkalmazás hello hitelesítő adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a65c6-158">hello combination of namespace and SAS key provides hello credentials for Service Bus tooauthenticate access tooan application.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a><span data-ttu-id="a65c6-159">Webes szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="a65c6-159">Create a web role</span></span>
<span data-ttu-id="a65c6-160">Ebben a szakaszban az alkalmazás előtérrendszerét hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-160">In this section, you build hello front end of your application.</span></span> <span data-ttu-id="a65c6-161">Először hozza létre az alkalmazás által megjelenített hello oldalakat.</span><span class="sxs-lookup"><span data-stu-id="a65c6-161">First, you create hello pages that your application displays.</span></span>
<span data-ttu-id="a65c6-162">Ezt követően adja hozzá a kódot, amely elküldi a cikkek tooa Service Bus-üzenetsorba, és hello várólista állapotadatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a65c6-162">After that, add code that submits items tooa Service Bus queue and displays status information about hello queue.</span></span>

### <a name="create-hello-project"></a><span data-ttu-id="a65c6-163">Hello projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="a65c6-163">Create hello project</span></span>
1. <span data-ttu-id="a65c6-164">Rendszergazdai jogosultságokkal indítsa el a Visual Studio: kattintson a jobb gombbal hello **Visual Studio** programikonra, és kattintson a **Futtatás rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-164">Using administrator privileges, start Visual Studio: right-click hello **Visual Studio** program icon, and then click **Run as administrator**.</span></span> <span data-ttu-id="a65c6-165">hello Azure compute emulator, ebben a cikkben korábban tárgyalt van szükség, hogy a Visual Studio rendszergazdai jogosultságokkal indítja el.</span><span class="sxs-lookup"><span data-stu-id="a65c6-165">hello Azure compute emulator, discussed later in this article, requires that Visual Studio be started with administrator privileges.</span></span>
   
   <span data-ttu-id="a65c6-166">A Visual Studio, a hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-166">In Visual Studio, on hello **File** menu, click **New**, and then click **Project**.</span></span>
2. <span data-ttu-id="a65c6-167">Az **Installed Templates** (Telepített sablonok) lap **Visual C#** területén kattintson a **Cloud** (Felhő), majd az **Azure Cloud Service** (Azure-felhőszolgáltatás) elemre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-167">From **Installed Templates**, under **Visual C#**, click **Cloud** and then click **Azure Cloud Service**.</span></span> <span data-ttu-id="a65c6-168">Név hello projekt **MultiTierApp**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-168">Name hello project **MultiTierApp**.</span></span> <span data-ttu-id="a65c6-169">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a65c6-169">Then click **OK**.</span></span>
   
   ![][9]
3. <span data-ttu-id="a65c6-170">A **.NET Framework 4.5** (.NET-keretrendszer 4.5) szerepkörök között kattintson duplán az **ASP.NET Web Role** (ASP.NET webes szerepkör) elemre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-170">From **.NET Framework 4.5** roles, double-click **ASP.NET Web Role**.</span></span>
   
   ![][10]
4. <span data-ttu-id="a65c6-171">Vigye **WebRole1** alatt **Azure Cloud Service megoldás**, hello ceruza ikonra, és nevezze át a hello webes szerepkör túl**FrontendWebRole**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-171">Hover over **WebRole1** under **Azure Cloud Service solution**, click hello pencil icon, and rename hello web role too**FrontendWebRole**.</span></span> <span data-ttu-id="a65c6-172">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a65c6-172">Then click **OK**.</span></span> <span data-ttu-id="a65c6-173">(Ügyeljen, hogy a „Frontend” nevet kis „e” betűvel írja, és ne „FrontEnd” formában.)</span><span class="sxs-lookup"><span data-stu-id="a65c6-173">(Make sure you enter "Frontend" with a lower-case 'e,' not "FrontEnd".)</span></span>
   
   ![][11]
5. <span data-ttu-id="a65c6-174">A hello **új ASP.NET projekt** párbeszédpanel hello **válasszon olyan sablont,** listában, kattintson **MVC**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-174">From hello **New ASP.NET Project** dialog box, in hello **Select a template** list, click **MVC**.</span></span>
   
   ![][12]
6. <span data-ttu-id="a65c6-175">Továbbra is a hello **új ASP.NET-projekt** párbeszédpanelen kattintson az hello **hitelesítés módosítása** gomb.</span><span class="sxs-lookup"><span data-stu-id="a65c6-175">Still in hello **New ASP.NET Project** dialog box, click hello **Change Authentication** button.</span></span> <span data-ttu-id="a65c6-176">A hello **hitelesítés módosítása** párbeszédpanel, kattintson a **nem hitelesítési**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-176">In hello **Change Authentication** dialog box, click **No Authentication**, and then click **OK**.</span></span> <span data-ttu-id="a65c6-177">Ebben az oktatóanyaghoz egy olyan alkalmazást hoz létre, amelyhez nincs szükség felhasználói bejelentkezésre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-177">For this tutorial, you're deploying an app that doesn't need a user login.</span></span>
   
    ![][16]
7. <span data-ttu-id="a65c6-178">Vissza a hello **új ASP.NET projekt** párbeszédpanel, kattintson a **OK** toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="a65c6-178">Back in hello **New ASP.NET Project** dialog box, click **OK** toocreate hello project.</span></span>
8. <span data-ttu-id="a65c6-179">A **Solution Explorer**, a hello **FrontendWebRole** projektre, kattintson a jobb gombbal **hivatkozások**, majd kattintson a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-179">In **Solution Explorer**, in hello **FrontendWebRole** project, right-click **References**, then click **Manage NuGet Packages**.</span></span>
9. <span data-ttu-id="a65c6-180">Kattintson a hello **Tallózás** lapra, és keresse meg `Microsoft Azure Service Bus`.</span><span class="sxs-lookup"><span data-stu-id="a65c6-180">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus`.</span></span> <span data-ttu-id="a65c6-181">Jelölje be hello **WindowsAzure.ServiceBus** csomagot, kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.</span><span class="sxs-lookup"><span data-stu-id="a65c6-181">Select hello **WindowsAzure.ServiceBus** package, click **Install**, and accept hello terms of use.</span></span>
   
   ![][13]
   
   <span data-ttu-id="a65c6-182">Vegye figyelembe, hogy hello szükséges ügyfélszerelvények most már hivatkozottak és hozzáadott néhány új kódot fájlt.</span><span class="sxs-lookup"><span data-stu-id="a65c6-182">Note that hello required client assemblies are now referenced and some new code files have been added.</span></span>
10. <span data-ttu-id="a65c6-183">A **Megoldáskezelőben** kattintson a jobb gombbal a **Models** (Modellek) elemre, kattintson az **Add** (Hozzáadás) parancsra, majd kattintson a **Class** (Osztály) elemre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-183">In **Solution Explorer**, right-click **Models** and click **Add**, then click **Class**.</span></span> <span data-ttu-id="a65c6-184">A hello **neve** mezőbe, írja be a hello nevet **OnlineOrder.cs**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-184">In hello **Name** box, type hello name **OnlineOrder.cs**.</span></span> <span data-ttu-id="a65c6-185">Ezután kattintson az **Add** (Hozzáadás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a65c6-185">Then click **Add**.</span></span>

### <a name="write-hello-code-for-your-web-role"></a><span data-ttu-id="a65c6-186">A webes szerepkör hello kód írása</span><span class="sxs-lookup"><span data-stu-id="a65c6-186">Write hello code for your web role</span></span>
<span data-ttu-id="a65c6-187">Ebben a szakaszban az alkalmazás által megjelenített különféle oldalakat hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-187">In this section, you create hello various pages that your application displays.</span></span>

1. <span data-ttu-id="a65c6-188">Visual Studio hello OnlineOrder.cs fájlban cserélje le a meglévő névtér-definíciót hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="a65c6-188">In hello OnlineOrder.cs file in Visual Studio, replace the existing namespace definition with hello following code:</span></span>
   
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
2. <span data-ttu-id="a65c6-189">A **Megoldáskezelőben** kattintson duplán a **Controllers\HomeController.cs** elemre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-189">In **Solution Explorer**, double-click **Controllers\HomeController.cs**.</span></span> <span data-ttu-id="a65c6-190">Adja hozzá a következő hello **használatával** hello hello tetején utasítások tooinclude hello névtereknek az imént létrehozott modellbe, valamint a Service Bus az fájlt.</span><span class="sxs-lookup"><span data-stu-id="a65c6-190">Add hello following **using** statements at hello top of hello file tooinclude hello namespaces for the model you just created, as well as Service Bus.</span></span>
   
   ```csharp
   using FrontendWebRole.Models;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   ```
3. <span data-ttu-id="a65c6-191">Is Visual Studio hello HomeController.cs fájlban cserélje le a meglévő névtér-definíciót a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="a65c6-191">Also in hello HomeController.cs file in Visual Studio, replace the existing namespace definition with hello following code.</span></span> <span data-ttu-id="a65c6-192">Ez a kód elemek toohello várólista hello küldésének kezelésére vonatkozó metódusokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a65c6-192">This code contains methods for handling hello submission of items toohello queue.</span></span>
   
   ```csharp
   namespace FrontendWebRole.Controllers
   {
       public class HomeController : Controller
       {
           public ActionResult Index()
           {
               // Simply redirect tooSubmit, since Submit will serve as the
               // front page of this application.
               return RedirectToAction("Submit");
           }
   
           public ActionResult About()
           {
               return View();
           }
   
           // GET: /Home/Submit.
           // Controller method for a view you will create for hello submission
           // form.
           public ActionResult Submit()
           {
               // Will put code for displaying queue message count here.
   
               return View();
           }
   
           // POST: /Home/Submit.
           // Controller method for handling submissions from hello submission
           // form.
           [HttpPost]
           // Attribute toohelp prevent cross-site scripting attacks and
           // cross-site request forgery.  
           [ValidateAntiForgeryToken]
           public ActionResult Submit(OnlineOrder order)
           {
               if (ModelState.IsValid)
               {
                   // Will put code for submitting tooqueue here.
   
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
4. <span data-ttu-id="a65c6-193">A hello **Build** menüben kattintson a **megoldás fordítása** eddigi munkája pontosságát tootest hello.</span><span class="sxs-lookup"><span data-stu-id="a65c6-193">From hello **Build** menu, click **Build Solution** tootest hello accuracy of your work so far.</span></span>
5. <span data-ttu-id="a65c6-194">Most, a hello hello nézetet létrehoznia `Submit()` módszer a korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="a65c6-194">Now, create hello view for hello `Submit()` method you created earlier.</span></span> <span data-ttu-id="a65c6-195">Kattintson a jobb gombbal hello `Submit()` metódus (hello túlterhelése `Submit()` -túlterhelésbe), és válassza a **nézet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-195">Right-click within hello `Submit()` method (hello overload of `Submit()` that takes no parameters), and then choose **Add View**.</span></span>
   
   ![][14]
6. <span data-ttu-id="a65c6-196">Megjelenik egy párbeszédpanel hello nézet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a65c6-196">A dialog box appears for creating hello view.</span></span> <span data-ttu-id="a65c6-197">A hello **sablon** menüben válassza ki **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-197">In hello **Template** list, choose **Create**.</span></span> <span data-ttu-id="a65c6-198">A hello **Model class** listában, kattintson a hello **OnlineOrder** osztály.</span><span class="sxs-lookup"><span data-stu-id="a65c6-198">In hello **Model class** list, click hello **OnlineOrder** class.</span></span>
   
   ![][15]
7. <span data-ttu-id="a65c6-199">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="a65c6-199">Click **Add**.</span></span>
8. <span data-ttu-id="a65c6-200">Módosítsa az alkalmazás hello megjelenített nevét.</span><span class="sxs-lookup"><span data-stu-id="a65c6-200">Now, change hello displayed name of your application.</span></span> <span data-ttu-id="a65c6-201">A **Megoldáskezelőben**, kattintson duplán a **Views\Shared\\_Layout.cshtml** tooopen fájlt azt hello Visual Studio szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="a65c6-201">In **Solution Explorer**, double-click the **Views\Shared\\_Layout.cshtml** file tooopen it in hello Visual Studio editor.</span></span>
9. <span data-ttu-id="a65c6-202">Cserélje le a **My ASP.NET Application** (Saját ASP.NET-alkalmazás) minden előfordulását **LITWARE's Products** (LITWARE-termékek) értékre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-202">Replace all occurrences of **My ASP.NET Application** with **LITWARE'S Products**.</span></span>
10. <span data-ttu-id="a65c6-203">Távolítsa el a hello **Home**, **kapcsolatos**, és **forduljon** hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="a65c6-203">Remove hello **Home**, **About**, and **Contact** links.</span></span> <span data-ttu-id="a65c6-204">Törölje a kiemelt hello kódot:</span><span class="sxs-lookup"><span data-stu-id="a65c6-204">Delete hello highlighted code:</span></span>
    
    ![][28]
11. <span data-ttu-id="a65c6-205">Végül módosítsa hello küldésének lap tooinclude hello várólista kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="a65c6-205">Finally, modify hello submission page tooinclude some information about hello queue.</span></span> <span data-ttu-id="a65c6-206">A **Megoldáskezelőben**, kattintson duplán a **Views\Home\Submit.cshtml** tooopen fájlt azt hello Visual Studio szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="a65c6-206">In **Solution Explorer**, double-click the **Views\Home\Submit.cshtml** file tooopen it in hello Visual Studio editor.</span></span> <span data-ttu-id="a65c6-207">Adja hozzá a következő sor után hello `<h2>Submit</h2>`.</span><span class="sxs-lookup"><span data-stu-id="a65c6-207">Add hello following line after `<h2>Submit</h2>`.</span></span> <span data-ttu-id="a65c6-208">Most hello `ViewBag.MessageCount` üres.</span><span class="sxs-lookup"><span data-stu-id="a65c6-208">For now, hello `ViewBag.MessageCount` is empty.</span></span> <span data-ttu-id="a65c6-209">Később fogja majd feltölteni.</span><span class="sxs-lookup"><span data-stu-id="a65c6-209">You will populate it later.</span></span>
    
    ```html
    <p>Current number of orders in queue waiting toobe processed: @ViewBag.MessageCount</p>
    ```
12. <span data-ttu-id="a65c6-210">Megvalósította a felhasználói felületet.</span><span class="sxs-lookup"><span data-stu-id="a65c6-210">You now have implemented your UI.</span></span> <span data-ttu-id="a65c6-211">Lenyomhatja **F5** toorun az alkalmazást, és győződjön meg arról, hogy várakozásainak megfelelően várt.</span><span class="sxs-lookup"><span data-stu-id="a65c6-211">You can press **F5** toorun your application and confirm that it looks as expected.</span></span>
    
    ![][17]

### <a name="write-hello-code-for-submitting-items-tooa-service-bus-queue"></a><span data-ttu-id="a65c6-212">Elemek tooa Service Bus-üzenetsorba küldéséhez hello kód írása</span><span class="sxs-lookup"><span data-stu-id="a65c6-212">Write hello code for submitting items tooa Service Bus queue</span></span>
<span data-ttu-id="a65c6-213">Ezután adja hozzá a kód elküldésekor elemek tooa várólista.</span><span class="sxs-lookup"><span data-stu-id="a65c6-213">Now, add code for submitting items tooa queue.</span></span> <span data-ttu-id="a65c6-214">Először hozza létre a Service Bus-üzenetsor kapcsolati adatait tartalmazó osztályt.</span><span class="sxs-lookup"><span data-stu-id="a65c6-214">First, you create a class that contains your Service Bus queue connection information.</span></span> <span data-ttu-id="a65c6-215">Ezután inicializálja a kapcsolatot a Global.aspx.cs osztályból.</span><span class="sxs-lookup"><span data-stu-id="a65c6-215">Then, initialize your connection from Global.aspx.cs.</span></span> <span data-ttu-id="a65c6-216">Végül frissítse a korábban a HomeController.cs tooactually submit elemek tooa Service Bus-üzenetsorba létrehozott hello elküldési kódot.</span><span class="sxs-lookup"><span data-stu-id="a65c6-216">Finally, update hello submission code you created earlier in HomeController.cs tooactually submit items tooa Service Bus queue.</span></span>

1. <span data-ttu-id="a65c6-217">A **Megoldáskezelőben**, kattintson a jobb gombbal **FrontendWebRole** (kattintson a jobb gombbal hello projekt, nem hello szerepkör).</span><span class="sxs-lookup"><span data-stu-id="a65c6-217">In **Solution Explorer**, right-click **FrontendWebRole** (right-click hello project, not hello role).</span></span> <span data-ttu-id="a65c6-218">Kattintson az **Add** (Hozzáadás), majd a **Class** (Osztály) elemre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-218">Click **Add**, and then click **Class**.</span></span>
2. <span data-ttu-id="a65c6-219">Hello osztály neve **QueueConnector.cs**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-219">Name hello class **QueueConnector.cs**.</span></span> <span data-ttu-id="a65c6-220">Kattintson a **Hozzáadás** toocreate hello osztály.</span><span class="sxs-lookup"><span data-stu-id="a65c6-220">Click **Add** toocreate hello class.</span></span>
3. <span data-ttu-id="a65c6-221">Ezután adja hozzá a kódot, amely magában foglalja a hello kapcsolati adatokat, és inicializálja az hello kapcsolat tooa Service Bus-üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="a65c6-221">Now, add code that encapsulates hello connection information and initializes hello connection tooa Service Bus queue.</span></span> <span data-ttu-id="a65c6-222">Cserélje le a QueueConnector.cs teljes tartalmát hello hello a következő kódot, és adja meg az értékeket `your Service Bus namespace` (névtér neve) és `yourKey`, vagyis hello **elsődleges kulcs** korábban szerzett hello Azure portál.</span><span class="sxs-lookup"><span data-stu-id="a65c6-222">Replace hello entire contents of QueueConnector.cs with hello following code, and enter values for `your Service Bus namespace` (your namespace name) and `yourKey`, which is hello **primary key** you previously obtained from hello Azure portal.</span></span>
   
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
   
           // Obtain these values from hello portal.
           public const string Namespace = "your Service Bus namespace";
   
           // hello name of your queue.
           public const string QueueName = "OrdersQueue";
   
           public static NamespaceManager CreateNamespaceManager()
           {
               // Create hello namespace manager which gives you access to
               // management operations.
               var uri = ServiceBusEnvironment.CreateServiceUri(
                   "sb", Namespace, String.Empty);
               var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                   "RootManageSharedAccessKey", "yourKey");
               return new NamespaceManager(uri, tP);
           }
   
           public static void Initialize()
           {
               // Using Http toobe friendly with outbound firewalls.
               ServiceBusEnvironment.SystemConnectivity.Mode =
                   ConnectivityMode.Http;
   
               // Create hello namespace manager which gives you access to
               // management operations.
               var namespaceManager = CreateNamespaceManager();
   
               // Create hello queue if it does not exist already.
               if (!namespaceManager.QueueExists(QueueName))
               {
                   namespaceManager.CreateQueue(QueueName);
               }
   
               // Get a client toohello queue.
               var messagingFactory = MessagingFactory.Create(
                   namespaceManager.Address,
                   namespaceManager.Settings.TokenProvider);
               OrdersQueueClient = messagingFactory.CreateQueueClient(
                   "OrdersQueue");
           }
       }
   }
   ```
4. <span data-ttu-id="a65c6-223">Ellenőrizze, hogy az **Initialize** metódus meghívása megtörténik.</span><span class="sxs-lookup"><span data-stu-id="a65c6-223">Now, ensure that your **Initialize** method gets called.</span></span> <span data-ttu-id="a65c6-224">A **Megoldáskezelőben** kattintson duplán a **Global.asax\Global.asax.cs** elemre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-224">In **Solution Explorer**, double-click **Global.asax\Global.asax.cs**.</span></span>
5. <span data-ttu-id="a65c6-225">Adja hozzá a következő kódsort hello hello végén hello **Application_Start** metódust.</span><span class="sxs-lookup"><span data-stu-id="a65c6-225">Add hello following line of code at hello end of hello **Application_Start** method.</span></span>
   
   ```csharp
   FrontendWebRole.QueueConnector.Initialize();
   ```
6. <span data-ttu-id="a65c6-226">Végül frissítse a korábban létrehozott elemek toohello várólista elküldeni hello webes kódját.</span><span class="sxs-lookup"><span data-stu-id="a65c6-226">Finally, update hello web code you created earlier, to submit items toohello queue.</span></span> <span data-ttu-id="a65c6-227">A **Megoldáskezelőben** kattintson duplán a **Controllers\HomeController.cs** elemre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-227">In **Solution Explorer**, double-click **Controllers\HomeController.cs**.</span></span>
7. <span data-ttu-id="a65c6-228">Frissítés hello `Submit()` metódust (hello rendelkező túlterhelést paraméter nélküli) az alábbiak szerint tooget üdvözlőüzenetére száma hello várólista.</span><span class="sxs-lookup"><span data-stu-id="a65c6-228">Update hello `Submit()` method (hello overload that takes no parameters) as follows tooget hello message count for hello queue.</span></span>
   
   ```csharp
   public ActionResult Submit()
   {
       // Get a NamespaceManager which allows you tooperform management and
       // diagnostic operations on your Service Bus queues.
       var namespaceManager = QueueConnector.CreateNamespaceManager();
   
       // Get hello queue, and obtain hello message count.
       var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
       ViewBag.MessageCount = queue.MessageCount;
   
       return View();
   }
   ```
8. <span data-ttu-id="a65c6-229">Frissítés hello `Submit(OnlineOrder order)` metódust (hello rendelkező túlterhelést egy paraméter) az alábbiak szerint toosubmit sorrendben információk toohello várólista.</span><span class="sxs-lookup"><span data-stu-id="a65c6-229">Update hello `Submit(OnlineOrder order)` method (hello overload that takes one parameter) as follows toosubmit order information toohello queue.</span></span>
   
   ```csharp
   public ActionResult Submit(OnlineOrder order)
   {
       if (ModelState.IsValid)
       {
           // Create a message from hello order.
           var message = new BrokeredMessage(order);
   
           // Submit hello order.
           QueueConnector.OrdersQueueClient.Send(message);
           return RedirectToAction("Submit");
       }
       else
       {
           return View(order);
       }
   }
   ```
9. <span data-ttu-id="a65c6-230">Most futtathatja hello az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a65c6-230">You can now run hello application again.</span></span> <span data-ttu-id="a65c6-231">Minden alkalommal, amikor elküld egy rendelést hello üzenetek száma nőni fog.</span><span class="sxs-lookup"><span data-stu-id="a65c6-231">Each time you submit an order, hello message count increases.</span></span>
   
   ![][18]

## <a name="create-hello-worker-role"></a><span data-ttu-id="a65c6-232">Hello feldolgozói szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="a65c6-232">Create hello worker role</span></span>
<span data-ttu-id="a65c6-233">Mostantól létrehozhat hello feldolgozói szerepkör, amely feldolgozza a hello elküldött rendeléseket.</span><span class="sxs-lookup"><span data-stu-id="a65c6-233">You will now create hello worker role that processes hello order submissions.</span></span> <span data-ttu-id="a65c6-234">Ez a példa hello **feldolgozói szerepkör Service Bus-üzenetsorba való** Visual Studio-projektsablont.</span><span class="sxs-lookup"><span data-stu-id="a65c6-234">This example uses hello **Worker Role with Service Bus Queue** Visual Studio project template.</span></span> <span data-ttu-id="a65c6-235">Hello portálról beszerzett hello szükséges hitelesítő adatok már.</span><span class="sxs-lookup"><span data-stu-id="a65c6-235">You already obtained hello required credentials from hello portal.</span></span>

1. <span data-ttu-id="a65c6-236">Győződjön meg arról, hogy csatlakozott a Visual Studio tooyour Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="a65c6-236">Make sure you have connected Visual Studio tooyour Azure account.</span></span>
2. <span data-ttu-id="a65c6-237">A Visual Studio a **Megoldáskezelőben** kattintson a jobb gombbal a **szerepkörök** hello mappája **MultiTierApp** projekt.</span><span class="sxs-lookup"><span data-stu-id="a65c6-237">In Visual Studio, in **Solution Explorer** right-click the **Roles** folder under hello **MultiTierApp** project.</span></span>
3. <span data-ttu-id="a65c6-238">Kattintson az **Add** (Hozzáadás), majd a **New Worker Role Project** (Új feldolgozói szerepkör projekt) elemre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-238">Click **Add**, and then click **New Worker Role Project**.</span></span> <span data-ttu-id="a65c6-239">Hello **új szerepkör projekt hozzáadása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a65c6-239">hello **Add New Role Project** dialog box appears.</span></span>
   
   ![][26]
4. <span data-ttu-id="a65c6-240">A hello **új szerepkör projekt hozzáadása** párbeszédpanel, kattintson a **feldolgozói szerepkör Service Bus-üzenetsorba való**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-240">In hello **Add New Role Project** dialog box, click **Worker Role with Service Bus Queue**.</span></span>
   
   ![][23]
5. <span data-ttu-id="a65c6-241">A hello **neve** mezőbe, a név hello projekt **OrderProcessingRole**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-241">In hello **Name** box, name hello project **OrderProcessingRole**.</span></span> <span data-ttu-id="a65c6-242">Ezután kattintson az **Add** (Hozzáadás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a65c6-242">Then click **Add**.</span></span>
6. <span data-ttu-id="a65c6-243">Másolás hello kapcsolati karakterláncot, amely hello "Service Bus-névtér létrehozása" szakasz toohello vágólapra 9. lépésben beolvasott.</span><span class="sxs-lookup"><span data-stu-id="a65c6-243">Copy hello connection string that you obtained in step 9 of hello "Create a Service Bus namespace" section toohello clipboard.</span></span>
7. <span data-ttu-id="a65c6-244">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **OrderProcessingRole** 5. lépésben létrehozott (Győződjön meg arról, hogy a jobb gombbal **OrderProcessingRole** alatt **Szerepkörök**, és nem hello osztály).</span><span class="sxs-lookup"><span data-stu-id="a65c6-244">In **Solution Explorer**, right-click hello **OrderProcessingRole** you created in step 5 (make sure that you right-click **OrderProcessingRole** under **Roles**, and not hello class).</span></span> <span data-ttu-id="a65c6-245">Ezután kattintson a **Properties** (Tulajdonságok) elemre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-245">Then click **Properties**.</span></span>
8. <span data-ttu-id="a65c6-246">A hello **beállítások** hello lapján **tulajdonságok** párbeszédpanelen kattintson belül hello **érték** mezőjének **value**, majd illessze be a 6. lépésben másolt végpontértéket hello.</span><span class="sxs-lookup"><span data-stu-id="a65c6-246">On hello **Settings** tab of hello **Properties** dialog box, click inside hello **Value** box for **Microsoft.ServiceBus.ConnectionString**, and then paste hello endpoint value you copied in step 6.</span></span>
   
   ![][25]
9. <span data-ttu-id="a65c6-247">Hozzon létre egy **OnlineOrder** toorepresent hello rendelések osztályt hello várólistából feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="a65c6-247">Create an **OnlineOrder** class toorepresent hello orders as you process them from hello queue.</span></span> <span data-ttu-id="a65c6-248">Használhat egy korábban létrehozott osztályt.</span><span class="sxs-lookup"><span data-stu-id="a65c6-248">You can reuse a class you have already created.</span></span> <span data-ttu-id="a65c6-249">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **OrderProcessingRole** osztály (kattintson a jobb gombbal hello osztály ikonjára, ne hello szerepkörre).</span><span class="sxs-lookup"><span data-stu-id="a65c6-249">In **Solution Explorer**, right-click hello **OrderProcessingRole** class (right-click hello class icon, not hello role).</span></span> <span data-ttu-id="a65c6-250">Kattintson az **Add** (Hozzáadás), majd az **Existing Item** (Meglévő elem) elemre.</span><span class="sxs-lookup"><span data-stu-id="a65c6-250">Click **Add**, then click **Existing Item**.</span></span>
10. <span data-ttu-id="a65c6-251">Keresse meg az almappát toohello **FrontendWebRole\Models**, majd kattintson duplán **OnlineOrder.cs** tooadd azt toothis projekt.</span><span class="sxs-lookup"><span data-stu-id="a65c6-251">Browse toohello subfolder for **FrontendWebRole\Models**, and then double-click **OnlineOrder.cs** tooadd it toothis project.</span></span>
11. <span data-ttu-id="a65c6-252">A **WorkerRole.cs**, hello hello értékének módosítása **QueueName** változót `"ProcessingQueue"` túl`"OrdersQueue"` hello kód a következő ábrán.</span><span class="sxs-lookup"><span data-stu-id="a65c6-252">In **WorkerRole.cs**, change hello value of hello **QueueName** variable from `"ProcessingQueue"` too`"OrdersQueue"` as shown in hello following code.</span></span>
    
    ```csharp
    // hello name of your queue.
    const string QueueName = "OrdersQueue";
    ```
12. <span data-ttu-id="a65c6-253">Adja hozzá hello következő using utasítást hello hello WorkerRole.cs fájl elejéhez.</span><span class="sxs-lookup"><span data-stu-id="a65c6-253">Add hello following using statement at hello top of hello WorkerRole.cs file.</span></span>
    
    ```csharp
    using FrontendWebRole.Models;
    ```
13. <span data-ttu-id="a65c6-254">A hello `Run()` függvény belül hello `OnMessage()` hívható meg, hogy lecseréli a hello hello tartalmát `try` a következő kód hello záradékot.</span><span class="sxs-lookup"><span data-stu-id="a65c6-254">In hello `Run()` function, inside hello `OnMessage()` call, replace hello contents of hello `try` clause with hello following code.</span></span>
    
    ```csharp
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View hello message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```
14. <span data-ttu-id="a65c6-255">Hello alkalmazás befejeződött.</span><span class="sxs-lookup"><span data-stu-id="a65c6-255">You have completed hello application.</span></span> <span data-ttu-id="a65c6-256">Kattintson a jobb gombbal a hello MultiTierApp projektre a Megoldáskezelőben, tesztelheti hello teljes alkalmazás kiválasztása **beállítás kezdőprojektként**, majd nyomja le az F5 billentyűt.</span><span class="sxs-lookup"><span data-stu-id="a65c6-256">You can test hello full application by right-clicking hello MultiTierApp project in Solution Explorer, selecting **Set as Startup Project**, and then pressing F5.</span></span> <span data-ttu-id="a65c6-257">Vegye figyelembe, hogy az üzenetek száma nem nő, mert hello feldolgozói szerepkör feldolgozza hello várólistában lévő elemeket, és befejezettként jelöli meg azokat.</span><span class="sxs-lookup"><span data-stu-id="a65c6-257">Note that the message count does not increment, because hello worker role processes items from hello queue and marks them as complete.</span></span> <span data-ttu-id="a65c6-258">A feldolgozói szerepkör nyomkövetési kimenetét hello megtekintésével hello Azure Compute Emulator felhasználói felületén tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="a65c6-258">You can see hello trace output of your worker role by viewing hello Azure Compute Emulator UI.</span></span> <span data-ttu-id="a65c6-259">Ehhez kattintson a jobb gombbal a hello emulátor ikonjára a tálca értesítési területén hello és kiválasztásával **Show Compute Emulator felhasználói felületén**.</span><span class="sxs-lookup"><span data-stu-id="a65c6-259">You can do this by right-clicking hello emulator icon in hello notification area of your taskbar and selecting **Show Compute Emulator UI**.</span></span>
    
    ![][19]
    
    ![][20]

## <a name="next-steps"></a><span data-ttu-id="a65c6-260">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a65c6-260">Next steps</span></span>
<span data-ttu-id="a65c6-261">toolearn Service Bus kapcsolatos további információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="a65c6-261">toolearn more about Service Bus, see hello following resources:</span></span>  

* <span data-ttu-id="a65c6-262">[Az Azure Service Bus dokumentációja][sbdocs]</span><span class="sxs-lookup"><span data-stu-id="a65c6-262">[Azure Service Bus documentation][sbdocs]</span></span>  
* <span data-ttu-id="a65c6-263">[Service Bus szolgáltatás oldala][sbacom]</span><span class="sxs-lookup"><span data-stu-id="a65c6-263">[Service Bus service page][sbacom]</span></span>  
* <span data-ttu-id="a65c6-264">[Hogyan tooUse Service Bus-üzenetsorok][sbacomqhowto]</span><span class="sxs-lookup"><span data-stu-id="a65c6-264">[How tooUse Service Bus Queues][sbacomqhowto]</span></span>  

<span data-ttu-id="a65c6-265">További információ a többrétegű konfigurációkat, toolearn lásd:</span><span class="sxs-lookup"><span data-stu-id="a65c6-265">toolearn more about multi-tier scenarios, see:</span></span>  

* <span data-ttu-id="a65c6-266">[Többrétegű .NET-alkalmazások tárolótáblák, üzenetsorok és blobok használatával][mutitierstorage]</span><span class="sxs-lookup"><span data-stu-id="a65c6-266">[.NET Multi-Tier Application Using Storage Tables, Queues, and Blobs][mutitierstorage]</span></span>  

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
