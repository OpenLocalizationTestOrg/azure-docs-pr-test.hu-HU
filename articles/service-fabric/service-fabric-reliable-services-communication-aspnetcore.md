---
title: "Az ASP.NET Core kommunikáció szolgáltatás |} Microsoft Docs"
description: "Ismerje meg, hogyan használható az ASP.NET Core állapotmentes és állapotalapú Reliable Services."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 8aa4668d-cbb6-4225-bd2d-ab5925a868f2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 05/02/2017
ms.author: vturecek
ms.openlocfilehash: 8ac4d409f7363e8b4ae98be659a627ac8db8d787
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="aspnet-core-in-service-fabric-reliable-services"></a><span data-ttu-id="f1bc8-103">Az ASP.NET Core a Service Fabric megbízható szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="f1bc8-103">ASP.NET Core in Service Fabric Reliable Services</span></span>

<span data-ttu-id="f1bc8-104">Az ASP.NET Core egy új nyílt forráskódú és platformfüggetlen keretrendszer modern felhőalapú internetre kapcsolódó alkalmazások, például webalkalmazások, az IoT-alkalmazásokhoz és a mobil háttérkiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-104">ASP.NET Core is a new open-source and cross-platform framework for building modern cloud-based Internet-connected applications, such as web apps, IoT apps, and mobile backends.</span></span> 

<span data-ttu-id="f1bc8-105">Ez a cikk egy részletes útmutató a Service Fabric Reliable Services használatával az ASP.NET Core szolgáltatásait üzemeltető a **Microsoft.ServiceFabric.AspNetCore.** * NuGet-csomagok készlete.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-105">This article is an in-depth guide to hosting ASP.NET Core services in Service Fabric Reliable Services using the **Microsoft.ServiceFabric.AspNetCore.*** set of NuGet packages.</span></span>

<span data-ttu-id="f1bc8-106">Egy bevezető oktatóanyag az ASP.NET Core a Service Fabric és a fejlesztési környezet beállítása történt utasításokat lásd: [egy webes előtér-, az ASP.NET Core segítségével alkalmazás felépítése](service-fabric-add-a-web-frontend.md).</span><span class="sxs-lookup"><span data-stu-id="f1bc8-106">For an introductory tutorial on ASP.NET Core in Service Fabric and instructions on getting your development environment set up, see [Building a web front-end for your application using ASP.NET Core](service-fabric-add-a-web-frontend.md).</span></span>

<span data-ttu-id="f1bc8-107">Ez a cikk többi azt feltételezi, hogy ismeri az ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-107">The rest of this article assumes you are already familiar with ASP.NET Core.</span></span> <span data-ttu-id="f1bc8-108">Ha nem, azt javasoljuk olvasása a [ASP.NET Core – alapok](https://docs.microsoft.com/aspnet/core/fundamentals/index).</span><span class="sxs-lookup"><span data-stu-id="f1bc8-108">If not, we recommend reading through the [ASP.NET Core fundamentals](https://docs.microsoft.com/aspnet/core/fundamentals/index).</span></span>

## <a name="aspnet-core-in-the-service-fabric-environment"></a><span data-ttu-id="f1bc8-109">Az ASP.NET Core a Service Fabric-környezetben</span><span class="sxs-lookup"><span data-stu-id="f1bc8-109">ASP.NET Core in the Service Fabric environment</span></span>

<span data-ttu-id="f1bc8-110">Az ASP.NET Core alkalmazásokat futtathat a .NET Core vagy a teljes .NET-keretrendszer, amíg a Service Fabric-szolgáltatás jelenleg csak futtathatja a teljes .NET-keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-110">While ASP.NET Core apps can run on .NET Core or on the full .NET Framework, Service Fabric services currently can only run on the full .NET Framework.</span></span> <span data-ttu-id="f1bc8-111">Ez azt jelenti, hogy az ASP.NET Core Service Fabric-szolgáltatás építésekor, kell továbbra is célozhat meg a teljes .NET-keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-111">This means when you build an ASP.NET  Core Service Fabric service, you must still target the full .NET Framework.</span></span>

<span data-ttu-id="f1bc8-112">Az ASP.NET Core a Service Fabric két különböző módon használható:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-112">ASP.NET Core can be used in two different ways in Service Fabric:</span></span>
 - <span data-ttu-id="f1bc8-113">**És a Vendég végrehajtható fájlja üzemeltetett**.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-113">**Hosted as a guest executable**.</span></span> <span data-ttu-id="f1bc8-114">Ez elsősorban az ASP.NET Core meglévő alkalmazások futtatásához a Service Fabric módosítások nélküli kód.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-114">This is primarily used to run existing ASP.NET Core applications on Service Fabric with no code changes.</span></span>
 - <span data-ttu-id="f1bc8-115">**A megbízható szolgáltatás futhat**.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-115">**Run inside a Reliable Service**.</span></span> <span data-ttu-id="f1bc8-116">Ez lehetővé teszi, hogy a Service Fabric-futtatókörnyezet jobb integrációt, és lehetővé teszi az állapotalapú ASP.NET Core szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-116">This allows better integration with the Service Fabric runtime and allows stateful ASP.NET Core services.</span></span>

<span data-ttu-id="f1bc8-117">A többi Ez a cikk ismerteti, hogyan ASP.NET Core belül megbízható szolgáltatás az ASP.NET Core integrációs összetevők, küldje el a Service Fabric SDK-val.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-117">The rest of this article explains how to use ASP.NET Core inside a Reliable Service using the ASP.NET Core integration components that ship with the Service Fabric SDK.</span></span> 

## <a name="service-fabric-service-hosting"></a><span data-ttu-id="f1bc8-118">A Service Fabric-szolgáltatás üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="f1bc8-118">Service Fabric service hosting</span></span>

<span data-ttu-id="f1bc8-119">A Service Fabric egy vagy több példány, illetve a replikákat a szolgáltatás futtatásához egy *gazdafolyamat szolgáltatás*, egy végrehajtható fájl, amely a szolgáltatáskód hibáit futtatja.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-119">In Service Fabric, one or more instances and/or replicas of your service run in a *service host process*, an executable file that runs your service code.</span></span> <span data-ttu-id="f1bc8-120">A szolgáltatás neve, a gazdagép-folyamat saját és a Service Fabric aktiválódik, és figyeli a meg.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-120">You, as a service author, own the service host process and Service Fabric activates and monitors it for you.</span></span>

<span data-ttu-id="f1bc8-121">IIS System.Web.dll keresztül szorosan csatlakoztatott hagyományos ASP.NET (max. MVC 5).</span><span class="sxs-lookup"><span data-stu-id="f1bc8-121">Traditional ASP.NET (up to MVC 5) is tightly coupled to IIS through System.Web.dll.</span></span> <span data-ttu-id="f1bc8-122">Az ASP.NET Core biztosít a webkiszolgáló és a webes alkalmazás közötti kizárás.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-122">ASP.NET Core provides a separation between the web server and your web application.</span></span> <span data-ttu-id="f1bc8-123">Ez lehetővé teszi, hogy a webes alkalmazások nem lesznek hordozható különböző kiszolgálók között, és is lehetővé teszi a webkiszolgálók *önállóan üzemel*, ami azt jelenti, hogy a webkiszolgáló elindíthatja a saját folyamatának figyelésekor dedikált tulajdonában lévő folyamat például az IIS Server szoftver.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-123">This allows web applications to be portable between different web servers and also allows web servers to be *self-hosted*, which means you can start a web server in your own process, as opposed to a process that is owned by dedicated web server software such as IIS.</span></span> 

<span data-ttu-id="f1bc8-124">A Service Fabric-szolgáltatás és az ASP.NET, a Vendég végrehajtható fájl vagy egy megbízható szolgáltatásban egyesítéséhez ASP.NET indítását a szolgáltatás gazdafolyamat belül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-124">In order to combine a Service Fabric service and ASP.NET, either as a guest executable or in a Reliable Service, you must be able to start ASP.NET inside your service host process.</span></span> <span data-ttu-id="f1bc8-125">Az ASP.NET Core önálló üzemeltető ezt teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-125">ASP.NET Core self-hosting allows you to do this.</span></span>

## <a name="hosting-aspnet-core-in-a-reliable-service"></a><span data-ttu-id="f1bc8-126">Az ASP.NET Core működnie a tároló</span><span class="sxs-lookup"><span data-stu-id="f1bc8-126">Hosting ASP.NET Core in a Reliable Service</span></span>
<span data-ttu-id="f1bc8-127">Általában önálló üzemeltetett ASP.NET Core alkalmazások létrehozása a Webállomás belépési pont egy alkalmazás, például a `static void Main()` metódus a `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-127">Typically, self-hosted ASP.NET Core applications create a WebHost in an application's entry point, such as the `static void Main()` method in `Program.cs`.</span></span> <span data-ttu-id="f1bc8-128">Ebben az esetben a Webállomás életciklusát van kötve az életciklus a folyamat.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-128">In this case, the lifecycle of the WebHost is bound to the lifecycle of the process.</span></span>

![Az ASP.NET Core üzemeltető folyamatban][0]

<span data-ttu-id="f1bc8-130">Azonban az alkalmazás belépési pont nincs a megfelelő helyen a Webállomás létrehozásához a megbízható szolgáltatás, az alkalmazás belépési pont csak használatával a Service Fabric-futtatókörnyezet, a szolgáltatás típusának regisztrálni, hogy az adott szolgáltatás típusú példányok létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-130">However, the application entry point is not the right place to create a WebHost in a Reliable Service, because the application entry point is only used to register a service type with the Service Fabric runtime, so that it may create instances of that service type.</span></span> <span data-ttu-id="f1bc8-131">A Webállomás létre kell hozni egy megbízható szolgáltatás magát.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-131">The WebHost should be created in a Reliable Service itself.</span></span> <span data-ttu-id="f1bc8-132">A szolgáltatás gazdafolyamaton belül szolgáltatáspéldány és/vagy a replikák Lépkedjen végig több életciklusának.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-132">Within the service host process, service instances and/or replicas can go through multiple lifecycles.</span></span> 

<span data-ttu-id="f1bc8-133">A megbízható példánya a származó osztály által képviselt `StatelessService` vagy `StatefulService`.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-133">A Reliable Service instance is represented by your service class deriving from `StatelessService` or `StatefulService`.</span></span> <span data-ttu-id="f1bc8-134">A kommunikációs verem szolgáltatás szerepel egy `ICommunicationListener` végrehajtása a szolgáltatás osztályban.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-134">The communication stack for a service is contained in an `ICommunicationListener` implementation in your service class.</span></span> <span data-ttu-id="f1bc8-135">A `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet-csomagok tartalmaznak implementációja `ICommunicationListener` , indítsa el és kezelheti az ASP.NET Core WebHost vércse vagy WebListener megbízható szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-135">The `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet packages contain implementations of `ICommunicationListener` that start and manage the ASP.NET Core WebHost for either Kestrel or WebListener in a Reliable Service.</span></span>

![Az ASP.NET Core működnie a tároló][1]

## <a name="aspnet-core-icommunicationlisteners"></a><span data-ttu-id="f1bc8-137">Az ASP.NET Core ICommunicationListeners</span><span class="sxs-lookup"><span data-stu-id="f1bc8-137">ASP.NET Core ICommunicationListeners</span></span>
<span data-ttu-id="f1bc8-138">A `ICommunicationListener` vércse és a WebListener megvalósításait a `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet-csomagok hasonló felhasználási minták rendelkezik, de az adott minden webkiszolgálóhoz némileg eltérő műveletet végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-138">The `ICommunicationListener` implementations for Kestrel and WebListener in the  `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet packages have similar use patterns but perform slightly different actions specific to each web server.</span></span> 

<span data-ttu-id="f1bc8-139">Mindkét kommunikációs figyelőket adja meg a következő argumentumok konstruktort:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-139">Both communication listeners provide a constructor that takes the following arguments:</span></span>
 - <span data-ttu-id="f1bc8-140">**`ServiceContext serviceContext`**: A `ServiceContext` objektum, amely a futó szolgáltatás adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-140">**`ServiceContext serviceContext`**: The `ServiceContext` object that contains information about the running service.</span></span>
 - <span data-ttu-id="f1bc8-141">**`string endpointName`**: a neve egy `Endpoint` ServiceManifest.xml konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-141">**`string endpointName`**: the name of an `Endpoint` configuration in ServiceManifest.xml.</span></span> <span data-ttu-id="f1bc8-142">Ez a főként ha a két kommunikációs figyelőket eltérőek: WebListener **szükséges** egy `Endpoint` konfigurációs, vércse azonban nem.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-142">This is primarily where the two communication listeners differ: WebListener **requires** an `Endpoint` configuration, while Kestrel does not.</span></span>
 - <span data-ttu-id="f1bc8-143">**`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: egy lambda, amely hoz létre, és visszatérési megvalósító egy `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-143">**`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: a lambda that you implement in which you create and return an `IWebHost`.</span></span> <span data-ttu-id="f1bc8-144">Ez lehetővé teszi, hogy konfigurálja `IWebHost` módja az ASP.NET Core alkalmazásban normális esetben tenné.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-144">This allows you to configure `IWebHost` the way you normally would in an ASP.NET Core application.</span></span> <span data-ttu-id="f1bc8-145">A lambda biztosít egy URL-címnek, amely, attól függően, hogy a Service Fabric-integráció lehetőségek akkor jön létre, és a `Endpoint` konfigurációs megadnia.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-145">The lambda provides a URL which is generated for you depending on the Service Fabric integration options you use and the `Endpoint` configuration you provide.</span></span> <span data-ttu-id="f1bc8-146">URL-cím majd kell módosítani vagy vettük-elindítása a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-146">That URL can then be modified or used as-is to start the web server.</span></span>

## <a name="service-fabric-integration-middleware"></a><span data-ttu-id="f1bc8-147">A Service Fabric-integráció köztes</span><span class="sxs-lookup"><span data-stu-id="f1bc8-147">Service Fabric integration middleware</span></span>
<span data-ttu-id="f1bc8-148">A `Microsoft.ServiceFabric.Services.AspNetCore` NuGet-csomag magában foglalja a `UseServiceFabricIntegration` bővítmény metódusa `IWebHostBuilder` , amely a Service Fabric-kompatibilis köztes ad.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-148">The `Microsoft.ServiceFabric.Services.AspNetCore` NuGet package includes the `UseServiceFabricIntegration` extension method on `IWebHostBuilder` that adds Service Fabric-aware middleware.</span></span> <span data-ttu-id="f1bc8-149">A köztes konfigurálja a vércse vagy WebListener `ICommunicationListener` regisztrálni egy egyedi URL-címe a Service Fabric-szolgáltatás majd összeveti az ügyfelek kéréseit az ügyfelek csatlakoznak a jobb szolgáltatás biztosítására.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-149">This middleware configures the Kestrel or WebListener `ICommunicationListener` to register a unique service URL with the Service Fabric Naming Service and then validates client requests to ensure clients are connecting to the right service.</span></span> <span data-ttu-id="f1bc8-150">Erre akkor szükség a megosztott gazdagép-környezetben, például a Service Fabric, ahol több webalkalmazás az azonos fizikai vagy virtuális gép is futtathatók, de ne használja egyedi állomásnevek, hogy megakadályozza a felhasználókat a megfelelő szolgáltatást véletlenül csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-150">This is necessary in a shared-host environment such as Service Fabric, where multiple web applications can run on the same physical or virtual machine but do not use unique host names, to prevent clients from mistakenly connecting to the wrong service.</span></span> <span data-ttu-id="f1bc8-151">Ebben a forgatókönyvben a következő szakaszban részletesen ismertetett.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-151">This scenario is described in more detail in the next section.</span></span>

### <a name="a-case-of-mistaken-identity"></a><span data-ttu-id="f1bc8-152">A gyorsítás esetében is téves identitás</span><span class="sxs-lookup"><span data-stu-id="f1bc8-152">A case of mistaken identity</span></span>
<span data-ttu-id="f1bc8-153">Szolgáltatás replikákat protocol, függetlenül egyedi IP:port kombinációjából figyelése.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-153">Service replicas, regardless of protocol, listen on a unique IP:port combination.</span></span> <span data-ttu-id="f1bc8-154">Szolgáltatás replika megkezdése IP:port a végpont figyel, után, hogy végpontcím és jelentéseket küldeni a Service Fabric-szolgáltatás ahol, könnyen megtalálhatók legyenek az ügyfelek vagy egyéb szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-154">Once a service replica has started listening on an IP:port endpoint, it reports that endpoint address to the Service Fabric Naming Service where it can be discovered by clients or other services.</span></span> <span data-ttu-id="f1bc8-155">Szolgáltatások alkalmazás dinamikusan hozzárendelt portok használatára, ha egy szolgáltatás replika helyezi használhat egy másik szolgáltatás, amely korábban a azonos fizikai vagy virtuális gép ugyanazon IP:port végpontja.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-155">If services use dynamically-assigned application ports, a service replica may coincidentally use the same IP:port endpoint of another service that was previously on the same physical or virtual machine.</span></span> <span data-ttu-id="f1bc8-156">Ennek hatására mistakely ügyfél csatlakozzon a megfelelő szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-156">This can cause a client to mistakely connect to the wrong service.</span></span> <span data-ttu-id="f1bc8-157">Ez akkor fordulhat elő, ha az események az alábbi sorrendben történik:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-157">This can happen if the following sequence of events occur:</span></span>

 1. <span data-ttu-id="f1bc8-158">A szolgáltatás figyeli 10.0.0.1:30000 HTTP Protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-158">Service A listens on 10.0.0.1:30000 over HTTP.</span></span> 
 2. <span data-ttu-id="f1bc8-159">Ügyfél szolgáltatás A feloldását, és lekérdezi a cím 10.0.0.1:30000</span><span class="sxs-lookup"><span data-stu-id="f1bc8-159">Client resolves Service A and gets address 10.0.0.1:30000</span></span>
 3. <span data-ttu-id="f1bc8-160">A szolgáltatás áthelyezése egy másik csomópont.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-160">Service A moves to a different node.</span></span>
 4. <span data-ttu-id="f1bc8-161">B szolgáltatás 10.0.0.1 helyezkedik el, és helyezi a 30000 ugyanazt a portot használja.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-161">Service B is placed on 10.0.0.1 and coincidentally uses the same port 30000.</span></span>
 5. <span data-ttu-id="f1bc8-162">Kapcsolódás a szolgáltatás A gyorsítótárazott cím 10.0.0.1:30000 az ügyfél megpróbálja.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-162">Client attempts to connect to service A with cached address 10.0.0.1:30000.</span></span>
 6. <span data-ttu-id="f1bc8-163">Ügyfél most már sikeresen csatlakozik-e a szolgáltatás nem megvalósító B csatlakozik-e a megfelelő szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-163">Client is now successfully connected to service B not realizing it is connected to the wrong service.</span></span>

<span data-ttu-id="f1bc8-164">Ez azt okozhatja, hogy a hibák diagnosztizálása nehezen véletlenszerű időpontokban.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-164">This can cause bugs at random times that can be difficult to diagnose.</span></span> 

### <a name="using-unique-service-urls"></a><span data-ttu-id="f1bc8-165">Egyedi URL-címek használata</span><span class="sxs-lookup"><span data-stu-id="f1bc8-165">Using unique service URLs</span></span>
<span data-ttu-id="f1bc8-166">Ennek megelőzése érdekében szolgáltatások is végpont küldje el a szolgáltatás az egyedi azonosítóját, és ezután ellenőrizze, hogy egyedi azonosítója ügyfélkérelmek során.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-166">To prevent this, services can post an endpoint to the Naming Service with a unique identifier, and then validate that unique identifier during client requests.</span></span> <span data-ttu-id="f1bc8-167">Ez a egy együttműködési művelet megbízható környezet nem rosszindulatú-bérlői szolgáltatások között.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-167">This is a cooperative action between services in a non-hostile-tenant trusted environment.</span></span> <span data-ttu-id="f1bc8-168">Ez nem biztosítja a biztonságos szolgáltatás hitelesítési rosszindulatú bérlői környezetben.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-168">This does not provide secure service authentication in a hostile-tenant environment.</span></span>

<span data-ttu-id="f1bc8-169">A köztes által hozzáadott megbízható környezetben a `UseServiceFabricIntegration` metódus automatikusan hozzáfűzi a cím, a szolgáltatás be, és ellenőrzi, hogy minden kérelemnél meg azonosító egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-169">In a trusted environment, the middleware that's added by the `UseServiceFabricIntegration` method automatically appends a unique identifier to the address that is posted to the Naming Service and validates that identifier on each request.</span></span> <span data-ttu-id="f1bc8-170">Ha az azonosító nem egyezik, akkor a köztes azonnal az egy HTTP 410 Megszűnt választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-170">If the identifier does not match, the middleware immediately returns an HTTP 410 Gone response.</span></span>

<span data-ttu-id="f1bc8-171">A köztes felhasználása ellenőrizniük kell egy dinamikusan hozzárendelt portot használó szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-171">Services that use a dynamically-assigned port should make use of this middleware.</span></span>

<span data-ttu-id="f1bc8-172">Egy rögzített egyedi portot használó szolgáltatások együttműködési környezetben nincs probléma.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-172">Services that use a fixed unique port do not have this problem in a cooperative environment.</span></span> <span data-ttu-id="f1bc8-173">A rögzített egyedi port általában használatos külsőleg szolgáltatásaival, amelyeket egy jól ismert port ügyfél alkalmazásokhoz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-173">A fixed unique port is typically used for externally-facing services that need a well-known port for client applications to connect to.</span></span> <span data-ttu-id="f1bc8-174">A legtöbb Internet felé néző webes alkalmazások például használja a 80-as vagy 443-as port webes böngésző kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-174">For example, most Internet-facing web applications will use port 80 or 443 for web browser connections.</span></span> <span data-ttu-id="f1bc8-175">Ebben az esetben az egyedi azonosító is engedélyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-175">In this case, the unique identifier should not be enabled.</span></span>

<span data-ttu-id="f1bc8-176">Az alábbi ábrán a áramlanak a engedélyezve köztes:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-176">The following diagram shows the request flow with the middleware enabled:</span></span>

![Service Fabric ASP.NET Core integráció][2]

<span data-ttu-id="f1bc8-178">Vércse és a WebListener `ICommunicationListener` megvalósítások használja a mechanizmus pontosan azonos módon.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-178">Both Kestrel and WebListener `ICommunicationListener` implementations use this mechanism in exactly the same way.</span></span> <span data-ttu-id="f1bc8-179">Bár WebListener belső megkülönböztetni azokat a kérelmeket a használatával az alapul szolgáló egyedi URL-cím elérési utak alapján *http.sys* port megosztása szolgáltatás, a funkció *nem* a WebListener általhasznált`ICommunicationListener` végrehajtása, mert a korábban ismertetett forgatókönyvben HTTP 503-as és a HTTP 404-es hiba állapotkódok vezethet.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-179">Although WebListener can internally differentiate requests based on unique URL paths using the underlying *http.sys* port sharing feature, that functionality is *not* used by the WebListener `ICommunicationListener` implementation because that will result in HTTP 503 and HTTP 404 error status codes in the scenario described earlier.</span></span> <span data-ttu-id="f1bc8-180">Amely pedig rendkívül megnehezíti az ügyfelek számára az határozza meg a hiba, a célt a HTTP 503-as és a HTTP 404-es már gyakran használt egyéb hibákat.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-180">That in turn makes it very difficult for clients to determine the intent of the error, as HTTP 503 and HTTP 404 are already commonly used to indicate other errors.</span></span> <span data-ttu-id="f1bc8-181">Emiatt vércse és WebListener `ICommunicationListener` megvalósítások szabványosítására a a köztes által biztosított a `UseServiceFabricIntegration` kiterjesztésmetódus, hogy az ügyfelek csak kell elvégezni a szolgáltatásvégpont újra oldja meg a HTTP 410 válaszok művelet.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-181">Thus, both Kestrel and WebListener `ICommunicationListener` implementations standardize on the middleware provided by the `UseServiceFabricIntegration` extension method so that clients only need to perform a service endpoint re-resolve action on HTTP 410 responses.</span></span>

## <a name="weblistener-in-reliable-services"></a><span data-ttu-id="f1bc8-182">A megbízható szolgáltatások WebListener</span><span class="sxs-lookup"><span data-stu-id="f1bc8-182">WebListener in Reliable Services</span></span>
<span data-ttu-id="f1bc8-183">WebListener használható egy megbízható szolgáltatás importálásával a **Microsoft.ServiceFabric.AspNetCore.WebListener** NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-183">WebListener can be used in a Reliable Service by importing the **Microsoft.ServiceFabric.AspNetCore.WebListener** NuGet package.</span></span> <span data-ttu-id="f1bc8-184">Ez a csomag tartalmaz `WebListenerCommunicationListener`, megvalósítása `ICommunicationListener`, ami lehetővé teszi az ASP.NET Core WebHost belül egy megbízható szolgáltatás létrehozásához WebListener használja, mint a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-184">This package contains `WebListenerCommunicationListener`, an implementation of `ICommunicationListener`, that allows you to create an ASP.NET Core WebHost inside a Reliable Service using WebListener as the web server.</span></span>

<span data-ttu-id="f1bc8-185">WebListener épül, a [Windows HTTP-kiszolgáló API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="f1bc8-185">WebListener is built on the [Windows HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx).</span></span> <span data-ttu-id="f1bc8-186">Ez a *http.sys* a HTTP-kérelmek feldolgozásához, és a webalkalmazások futó folyamatok továbbítani az IIS által használt kernel-illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-186">This uses the *http.sys* kernel driver used by IIS to process HTTP requests and route them to processes running web applications.</span></span> <span data-ttu-id="f1bc8-187">Ez lehetővé teszi több folyamatot, az azonos fizikai vagy virtuális gépet a webalkalmazások ugyanazt a portot, az egyedi URL-cím vagy állomásnév használatát.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-187">This allows multiple processes on the same physical or virtual machine to host web applications on the same port, disambiguated by either a unique URL path or hostname.</span></span> <span data-ttu-id="f1bc8-188">Ezek a funkciók hasznosak a Service Fabric ugyanazon fürt több webhely üzemeltetésére.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-188">These features are useful in Service Fabric for hosting multiple websites in the same cluster.</span></span>

<span data-ttu-id="f1bc8-189">A következő diagram azt ábrázolja, hogyan használja a WebListener a *http.sys* kernel illesztőprogramját a Windows-portmegosztás:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-189">The following diagram illustrates how WebListener uses the *http.sys* kernel driver on Windows for port sharing:</span></span>

![a HTTP.sys][3]

### <a name="weblistener-in-a-stateless-service"></a><span data-ttu-id="f1bc8-191">Az állapotmentes szolgáltatások WebListener</span><span class="sxs-lookup"><span data-stu-id="f1bc8-191">WebListener in a stateless service</span></span>
<span data-ttu-id="f1bc8-192">Használandó `WebListener` az állapotmentes szolgáltatások, bírálja felül a `CreateServiceInstanceListeners` metódust, és térjen vissza a `WebListenerCommunicationListener` példány:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-192">To use `WebListener` in a stateless service, override the `CreateServiceInstanceListeners` method and return a `WebListenerCommunicationListener` instance:</span></span>

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseWebListener()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build()))
    };
}
```

### <a name="weblistener-in-a-stateful-service"></a><span data-ttu-id="f1bc8-193">Az állapotalapú service WebListener</span><span class="sxs-lookup"><span data-stu-id="f1bc8-193">WebListener in a stateful service</span></span>

<span data-ttu-id="f1bc8-194">`WebListenerCommunicationListener`jelenleg nincs használatra szolgál az állapotalapú szolgáltatások miatt az alapul szolgáló, komplikációk *http.sys* port megosztása szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-194">`WebListenerCommunicationListener` is currently not designed for use in stateful services due to complications with the underlying *http.sys* port sharing feature.</span></span> <span data-ttu-id="f1bc8-195">További információkért lásd a következő WebListener a dinamikus port kiosztását.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-195">For more information, see the following section on dynamic port allocation with WebListener.</span></span> <span data-ttu-id="f1bc8-196">Állapotalapú szolgáltatások esetén vércse az az ajánlott webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-196">For stateful services, Kestrel is the recommended web server.</span></span>

### <a name="endpoint-configuration"></a><span data-ttu-id="f1bc8-197">Végpont-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="f1bc8-197">Endpoint configuration</span></span>

<span data-ttu-id="f1bc8-198">Egy `Endpoint` konfigurálni kell a webkiszolgálók, a Windows HTTP kiszolgáló API-t használó, beleértve a WebListener.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-198">An `Endpoint` configuration is required for web servers that use the Windows HTTP Server API, including WebListener.</span></span> <span data-ttu-id="f1bc8-199">A Windows HTTP-kiszolgáló API használó webkiszolgálók először kell lefoglalni az URL-cím elé *http.sys* (Ez általában érhető el, és a [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) eszköz).</span><span class="sxs-lookup"><span data-stu-id="f1bc8-199">Web servers that use the Windows HTTP Server API must first reserve their URL with *http.sys* (this is normally accomplished with the [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) tool).</span></span> <span data-ttu-id="f1bc8-200">Ehhez a művelethez emelt szintű jogosultságokkal, amely a szolgáltatások alapértelmezés szerint nem rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-200">This action requires elevated privileges that your services by default do not have.</span></span> <span data-ttu-id="f1bc8-201">A "http" vagy "https" beállításait a `Protocol` tulajdonsága a `Endpoint` konfiguráció *ServiceManifest.xml* használt kifejezetten kérje meg a Service Fabric-futtatókörnyezet, az egy URL-cím regisztrálása  *a HTTP.sys* a nevében használatával a [ *erős helyettesítő* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL-előtagját.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-201">The "http" or "https" options for the `Protocol` property of the `Endpoint` configuration in *ServiceManifest.xml* are used specifically to instruct the Service Fabric runtime to register a URL with *http.sys* on your behalf using the [*strong wildcard*](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL prefix.</span></span>

<span data-ttu-id="f1bc8-202">Ahhoz például, hogy foglalási `http://+:80` egy szolgáltatás, a következő konfigurációs kell használni a ServiceManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-202">For example, to reserve `http://+:80` for a service, the following configuration should be used in ServiceManifest.xml:</span></span>

```xml
<ServiceManifest ... >
    ...
    <Resources>
        <Endpoints>
            <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>

</ServiceManifest>
```

<span data-ttu-id="f1bc8-203">A végpont nevének kell átadni és a `WebListenerCommunicationListener` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-203">And the endpoint name must be passed to the `WebListenerCommunicationListener` constructor:</span></span>

```csharp
 new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     return new WebHostBuilder()
         .UseWebListener()
         .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
         .UseUrls(url)
         .Build();
 })
```

#### <a name="use-weblistener-with-a-static-port"></a><span data-ttu-id="f1bc8-204">A statikus port WebListener használata</span><span class="sxs-lookup"><span data-stu-id="f1bc8-204">Use WebListener with a static port</span></span>
<span data-ttu-id="f1bc8-205">A WebListener statikus port használatára, adja meg a portszámot a `Endpoint` konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-205">To use a static port with WebListener, provide the port number in the `Endpoint` configuration:</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-weblistener-with-a-dynamic-port"></a><span data-ttu-id="f1bc8-206">A dinamikus port WebListener használata</span><span class="sxs-lookup"><span data-stu-id="f1bc8-206">Use WebListener with a dynamic port</span></span>
<span data-ttu-id="f1bc8-207">Egy dinamikusan hozzárendelt port WebListener használni, hagyja ki ezt a `Port` tulajdonságot a `Endpoint` konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-207">To use a dynamically assigned port with WebListener, omit the `Port` property in the `Endpoint` configuration:</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

<span data-ttu-id="f1bc8-208">Vegye figyelembe, hogy a dinamikus port lefoglalta egy `Endpoint` a konfiguráció csak egyetlen port biztosít *állomás folyamatonként*.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-208">Note that a dynamic port allocated by an `Endpoint` configuration only provides one port *per host process*.</span></span> <span data-ttu-id="f1bc8-209">Az aktuális Service Fabric üzemeltetési modell lehetővé teszi, hogy több szolgáltatáspéldány és/vagy ugyanabban a folyamatban, ami azt jelenti, hogy mindegyik keresztül kiosztása során ugyanazt a portot is meg fogja osztani üzemeltetését replikák a `Endpoint` konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-209">The current Service Fabric hosting model allows multiple service instances and/or replicas to be hosted in the same process, meaning that each one will share the same port when allocated through the `Endpoint` configuration.</span></span> <span data-ttu-id="f1bc8-210">Több WebListener példány megoszthat egy portot használ, az alapul szolgáló *http.sys* port megosztása szolgáltatást, de, amely nem támogatja `WebListenerCommunicationListener` miatt a komplikációk az ügyféli kérelmek részére okozna.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-210">Multiple WebListener instances can share a port using the underlying *http.sys* port sharing feature, but that is not supported by `WebListenerCommunicationListener` due to the complications it introduces for client requests.</span></span> <span data-ttu-id="f1bc8-211">Dinamikus portok használatát vércse esetén ajánlott a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-211">For dynamic port usage, Kestrel is the recommended web server.</span></span>

## <a name="kestrel-in-reliable-services"></a><span data-ttu-id="f1bc8-212">A megbízható szolgáltatások vércse</span><span class="sxs-lookup"><span data-stu-id="f1bc8-212">Kestrel in Reliable Services</span></span>
<span data-ttu-id="f1bc8-213">Vércse használható egy megbízható szolgáltatás importálásával a **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-213">Kestrel can be used in a Reliable Service by importing the **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet package.</span></span> <span data-ttu-id="f1bc8-214">Ez a csomag tartalmaz `KestrelCommunicationListener`, megvalósítása `ICommunicationListener`, ami lehetővé teszi az ASP.NET Core WebHost belül egy megbízható szolgáltatás létrehozásához vércse használja, mint a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-214">This package contains `KestrelCommunicationListener`, an implementation of `ICommunicationListener`, that allows you to create an ASP.NET Core WebHost inside a Reliable Service using Kestrel as the web server.</span></span>

<span data-ttu-id="f1bc8-215">Vércse, az ASP.NET Core platformfüggetlen webkiszolgáló libuv, a platformok közötti aszinkron i/o-szalagtár egyik alapján.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-215">Kestrel is a cross-platform web server for ASP.NET Core based on libuv, a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="f1bc8-216">WebListener, eltérően vércse nem használ központi végponthoz vezető például *http.sys*.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-216">Unlike WebListener, Kestrel does not use a centralized endpoint manager such as *http.sys*.</span></span> <span data-ttu-id="f1bc8-217">És WebListener, eltérően vércse nem támogatja a port megosztása több folyamat között.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-217">And unlike WebListener, Kestrel does not support port sharing between multiple processes.</span></span> <span data-ttu-id="f1bc8-218">Minden példánynak vércse egy egyedi portot kell használnia.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-218">Each instance of Kestrel must use a unique port.</span></span>

![vércse][4]

### <a name="kestrel-in-a-stateless-service"></a><span data-ttu-id="f1bc8-220">Az állapotmentes szolgáltatások vércse</span><span class="sxs-lookup"><span data-stu-id="f1bc8-220">Kestrel in a stateless service</span></span>
<span data-ttu-id="f1bc8-221">Használandó `Kestrel` az állapotmentes szolgáltatások, bírálja felül a `CreateServiceInstanceListeners` metódust, és térjen vissza a `KestrelCommunicationListener` példány:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-221">To use `Kestrel` in a stateless service, override the `CreateServiceInstanceListeners` method and return a `KestrelCommunicationListener` instance:</span></span>

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

### <a name="kestrel-in-a-stateful-service"></a><span data-ttu-id="f1bc8-222">Az állapotalapú service vércse</span><span class="sxs-lookup"><span data-stu-id="f1bc8-222">Kestrel in a stateful service</span></span>
<span data-ttu-id="f1bc8-223">Használandó `Kestrel` állapot-nyilvántartó szolgáltatásban, bírálja felül a `CreateServiceReplicaListeners` metódust, és térjen vissza a `KestrelCommunicationListener` példány:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-223">To use `Kestrel` in a stateful service, override the `CreateServiceReplicaListeners` method and return a `KestrelCommunicationListener` instance:</span></span>

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new ServiceReplicaListener[]
    {
        new ServiceReplicaListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                         services => services
                             .AddSingleton<StatefulServiceContext>(serviceContext)
                             .AddSingleton<IReliableStateManager>(this.StateManager))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

<span data-ttu-id="f1bc8-224">Ebben a példában egy egypéldányos példánya `IReliableStateManager` valósul meg a Webállomás függőségi beszúrására szolgáló tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-224">In this example, a singleton instance of `IReliableStateManager` is provided to the WebHost dependency injection container.</span></span> <span data-ttu-id="f1bc8-225">Ez nem feltétlenül szükséges, de lehetővé teszi használatát `IReliableStateManager` és az MVC-vezérlő műveletmetódusokhoz megbízható gyűjteményei.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-225">This is not strictly necessary, but it allows you to use `IReliableStateManager` and Reliable Collections in your MVC controller action methods.</span></span>

<span data-ttu-id="f1bc8-226">Vegye figyelembe, hogy egy `Endpoint` -konfiguráció nevét **nem** megadott `KestrelCommunicationListener` állapot-nyilvántartó szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-226">Note that an `Endpoint` configuration name is **not** provided to `KestrelCommunicationListener` in a stateful service.</span></span> <span data-ttu-id="f1bc8-227">Ennek a magyarázatát részletesebben az alábbi részben található.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-227">This is explained in more detail in the following section.</span></span>

### <a name="endpoint-configuration"></a><span data-ttu-id="f1bc8-228">Végpont-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="f1bc8-228">Endpoint configuration</span></span>
<span data-ttu-id="f1bc8-229">Egy `Endpoint` konfigurációs vércse használatához nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-229">An `Endpoint` configuration is not required to use Kestrel.</span></span> 

<span data-ttu-id="f1bc8-230">Vércse egy egyszerű önálló webkiszolgáló; WebListener (és HttpListener) eltérően nem kell egy `Endpoint` konfiguráció *ServiceManifest.xml* , mert nem szükséges URL-cím regisztrálása elkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-230">Kestrel is a simple stand-alone web server; unlike WebListener (or HttpListener), it does not need an `Endpoint` configuration in *ServiceManifest.xml* because it does not require URL registration prior to starting.</span></span> 

#### <a name="use-kestrel-with-a-static-port"></a><span data-ttu-id="f1bc8-231">A statikus port vércse használata</span><span class="sxs-lookup"><span data-stu-id="f1bc8-231">Use Kestrel with a static port</span></span>
<span data-ttu-id="f1bc8-232">A statikus port konfigurálható a `Endpoint` ServiceManifest.xml konfigurációját vércse való használatra.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-232">A static port can be configured in the `Endpoint` configuration of ServiceManifest.xml for use with Kestrel.</span></span> <span data-ttu-id="f1bc8-233">Ez azonban nem feltétlenül szükséges, akkor két lehetséges előnyöket nyújtja:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-233">Although this is not strictly necessary, it provides two potential benefits:</span></span>
 1. <span data-ttu-id="f1bc8-234">Ha a port nem esik az alkalmazás porttartományát, nyitja meg, az operációs rendszer tűzfalon keresztül a Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-234">If the port does not fall in the application port range, it is opened through the OS firewall by Service Fabric.</span></span>
 2. <span data-ttu-id="f1bc8-235">Az URL-cím le a `KestrelCommunicationListener` ezt a portot fogja használni.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-235">The URL provided to you through `KestrelCommunicationListener` will use this port.</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

<span data-ttu-id="f1bc8-236">Ha egy `Endpoint` van konfigurálva, a nevét kell átadását a `KestrelCommunicationListener` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-236">If an `Endpoint` is configured, its name must be passed into the `KestrelCommunicationListener` constructor:</span></span> 

```csharp
new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) => ...
```

<span data-ttu-id="f1bc8-237">Ha egy `Endpoint` nem használja a konfigurációját, hagyja ki ezt a nevet a `KestrelCommunicationListener` konstruktor.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-237">If an `Endpoint` configuration is not used, omit the name in the `KestrelCommunicationListener` constructor.</span></span> <span data-ttu-id="f1bc8-238">Ebben az esetben egy dinamikus portot fogja használni.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-238">In this case, a dynamic port will be used.</span></span> <span data-ttu-id="f1bc8-239">További információ a következő szakaszban talál.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-239">See the next section for more information.</span></span>

#### <a name="use-kestrel-with-a-dynamic-port"></a><span data-ttu-id="f1bc8-240">A dinamikus port vércse használata</span><span class="sxs-lookup"><span data-stu-id="f1bc8-240">Use Kestrel with a dynamic port</span></span>
<span data-ttu-id="f1bc8-241">Vércse nem használhatja az automatikus port hozzárendelés a `Endpoint` konfigurációját a ServiceManifest.xml, mert automatikus port hozzárendelés egy `Endpoint` konfigurációs hozzárendel egy egyedi port / *folyamaton*, és egyetlen gazdafolyamat vércse több példánya is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-241">Kestrel cannot use the automatic port assignment from the `Endpoint` configuration in ServiceManifest.xml, because automatic port assignment from an `Endpoint` configuration assigns a unique port per *host process*, and a single host process can contain multiple Kestrel instances.</span></span> <span data-ttu-id="f1bc8-242">Vércse nem támogatja a port megosztása, mert az nem működik vércse feltünteti egy egyedi portot kell megnyitni.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-242">Since Kestrel does not support port sharing, this does not work as each Kestrel instance must be opened on a unique port.</span></span>

<span data-ttu-id="f1bc8-243">Dinamikus portok hozzárendelés vércse használni, egyszerűen csak hagyja ki ezt a `Endpoint` ServiceManifest.xml konfiguráció teljesen, és nem továbbítja az végpont nevét, a `KestrelCommunicationListener` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-243">To use dynamic port assignment with Kestrel, simply omit the `Endpoint` configuration in ServiceManifest.xml entirely, and do not pass an endpoint name to the `KestrelCommunicationListener` constructor:</span></span>

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

<span data-ttu-id="f1bc8-244">Ebben a konfigurációban `KestrelCommunicationListener` automatikusan választja ki egy nem használt portot az alkalmazás porttartományát.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-244">In this configuration, `KestrelCommunicationListener` will automatically select an unused port from the application port range.</span></span>

## <a name="scenarios-and-configurations"></a><span data-ttu-id="f1bc8-245">A forgatókönyvek és konfigurációk</span><span class="sxs-lookup"><span data-stu-id="f1bc8-245">Scenarios and configurations</span></span>
<span data-ttu-id="f1bc8-246">Ez a szakasz ismerteti a következő esetekben és a webkiszolgáló, port konfigurálása, a Service Fabric-integrációs beállítások és egyéb beállítások egy megfelelően működő szolgáltatás eléréséhez kombinációja javasolt:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-246">This section describes the following scenarios and provides the recommended combination of web server, port configuration, Service Fabric integration options, and miscellaneous settings to achieve a properly functioning service:</span></span>
 - <span data-ttu-id="f1bc8-247">Külsőleg kitett az ASP.NET Core állapotmentes szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="f1bc8-247">Externally exposed ASP.NET Core stateless service</span></span>
 - <span data-ttu-id="f1bc8-248">Csak belső ASP.NET Core állapotmentes szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="f1bc8-248">Internal-only ASP.NET Core stateless service</span></span>
 - <span data-ttu-id="f1bc8-249">Csak belső ASP.NET Core állapotalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f1bc8-249">Internal-only ASP.NET Core stateful service</span></span>

<span data-ttu-id="f1bc8-250">Egy **külsőleg kitett** szolgáltatás egyike, amely elérhetővé teszi a végpont, általában egy terhelés-kiegyenlítő keresztül a fürtön kívüli elérhető.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-250">An **externally exposed** service is one that exposes an endpoint reachable from outside the cluster, usually through a load balancer.</span></span>

<span data-ttu-id="f1bc8-251">Egy **csak belső** szolgáltatás az egyik amelynek-végpont esetében csak a fürtön belül elérhető.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-251">An **internal-only** service is one whose endpoint is only reachable from within the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="f1bc8-252">Állapot-nyilvántartó Szolgáltatásvégpontok általában nem szabad felfedni az internethez.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-252">Stateful service endpoints generally should not be exposed to the Internet.</span></span> <span data-ttu-id="f1bc8-253">Állapotalapú szolgáltatások teszi közzé, mert a terheléselosztó nem lesz képes forgalom megfelelő irányításához, és keresse meg lehet mögötti terheléselosztókat, amelyekben nem tudnak a Service Fabric szolgáltatás megoldás, az Azure terheléselosztó, például a fürtök az állapotalapú szolgáltatás replika.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-253">Clusters that are behind load balancers that are unaware of Service Fabric service resolution, such as the Azure Load Balancer, will be unable to expose stateful services because the load balancer will not be able to locate and route traffic to the appropriate stateful service replica.</span></span> 

### <a name="externally-exposed-aspnet-core-stateless-services"></a><span data-ttu-id="f1bc8-254">Külsőleg kitett az ASP.NET Core állapotmentes szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="f1bc8-254">Externally exposed ASP.NET Core stateless services</span></span>
<span data-ttu-id="f1bc8-255">WebListener az előtér-szolgáltatásokat, amelyek a Windows külső, az internetre irányuló HTTP-végpontokról láthatóvá ajánlott webkiszolgálóját.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-255">WebListener is the recommended web server for front-end services that expose external, Internet-facing HTTP endpoints on Windows.</span></span> <span data-ttu-id="f1bc8-256">Támadásokkal szembeni hatékonyabb védekezés és vércse viszont nem, például a Windows-hitelesítés és a port megosztása funkciókat is támogat.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-256">It provides better protection against attacks and supports features that Kestrel does not, such as Windows Authentication and port sharing.</span></span> 

<span data-ttu-id="f1bc8-257">Vércse jelenleg nem támogatott (internetről hozzáférhető) edge-kiszolgálóként.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-257">Kestrel is not supported as an edge (Internet-facing) server at this time.</span></span> <span data-ttu-id="f1bc8-258">Például az IIS vagy Nginx fordított proxykiszolgálón a nyilvános internetről érkező forgalom kezelésére használható.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-258">A reverse proxy server such as IIS or Nginx must be used to handle traffic from the public Internet.</span></span>
 
<span data-ttu-id="f1bc8-259">Ha kapcsolódik az internethez, állapotmentes szolgáltatások egy jól ismert és állandó végponttal, amely elérhető a terheléselosztót használja.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-259">When exposed to the Internet, a stateless service should use a well-known and stable endpoint that is reachable through a load balancer.</span></span> <span data-ttu-id="f1bc8-260">Ez az az URL-címet, a felhasználók számára az alkalmazás biztosítja.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-260">This is the URL you will provide to users of your application.</span></span> <span data-ttu-id="f1bc8-261">A következő konfiguráció ajánlott:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-261">The following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="f1bc8-262">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="f1bc8-262">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f1bc8-263">Webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="f1bc8-263">Web server</span></span> | <span data-ttu-id="f1bc8-264">WebListener</span><span class="sxs-lookup"><span data-stu-id="f1bc8-264">WebListener</span></span> | <span data-ttu-id="f1bc8-265">Ha a szolgáltatás csak fel van fedve egy megbízható hálózathoz, az intraneten vércse is használható.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-265">If the service is only exposed to a trusted network, such an intranet, Kestrel may be used.</span></span> <span data-ttu-id="f1bc8-266">Ellenkező esetben WebListener a kívánt beállítást.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-266">Otherwise, WebListener is the preferred option.</span></span> |
| <span data-ttu-id="f1bc8-267">Port konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1bc8-267">Port configuration</span></span> | <span data-ttu-id="f1bc8-268">Statikus</span><span class="sxs-lookup"><span data-stu-id="f1bc8-268">static</span></span> | <span data-ttu-id="f1bc8-269">A jól ismert statikus port lehet beállítani a `Endpoints` ServiceManifest.xml, például a 80-as HTTP vagy HTTPS-hez a 443-as konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-269">A well-known static port should be configured in the `Endpoints` configuration of ServiceManifest.xml, such as 80 for HTTP or 443 for HTTPS.</span></span> |
| <span data-ttu-id="f1bc8-270">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="f1bc8-270">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="f1bc8-271">None</span><span class="sxs-lookup"><span data-stu-id="f1bc8-271">None</span></span> | <span data-ttu-id="f1bc8-272">A `ServiceFabricIntegrationOptions.None` beállítást kell használni, amikor a Service Fabric-integráció köztes konfigurálása, hogy a szolgáltatás nem próbálja meg egy egyedi azonosítót a bejövő kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-272">The `ServiceFabricIntegrationOptions.None` option should be used when configuring Service Fabric integration middleware so that the service does not attempt to validate incoming requests for a unique identifier.</span></span> <span data-ttu-id="f1bc8-273">Az alkalmazás külső felhasználók nem fogja tudni az egyedi azonosító adatokat, a köztes használják.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-273">External users of your application will not know the unique identifying information used by the middleware.</span></span> |
| <span data-ttu-id="f1bc8-274">A példányok száma</span><span class="sxs-lookup"><span data-stu-id="f1bc8-274">Instance Count</span></span> | <span data-ttu-id="f1bc8-275">-1</span><span class="sxs-lookup"><span data-stu-id="f1bc8-275">-1</span></span> | <span data-ttu-id="f1bc8-276">Tipikus használati esetekben a példányok száma beállítást meg kell-"1", hogy egy példány érhető el az összes olyan csomóponton, amelyet a forgalom fogadására a terheléselosztótól.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-276">In typical use cases, the instance count setting should be set to "-1" so that an instance is available on all nodes that receive traffic from a load balancer.</span></span> |

<span data-ttu-id="f1bc8-277">Ha több külsőleg kitett szolgáltatás ugyanazokat a csomópontok megosztásához stabil, de egyedi URL-cím használható.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-277">If multiple externally exposed services share the same set of nodes, a unique but stable URL path should be used.</span></span> <span data-ttu-id="f1bc8-278">Ehhez a IWebHost konfigurálásakor megadott URL-cím módosításával.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-278">This can be accomplished by modifying the URL provided when configuring IWebHost.</span></span> <span data-ttu-id="f1bc8-279">Megjegyzés: Ez kizárólag a WebListener.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-279">Note this applies to WebListener only.</span></span>

 ```csharp
 new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     url += "/MyUniqueServicePath";
 
     return new WebHostBuilder()
         .UseWebListener()
         ...
         .UseUrls(url)
         .Build();
 })
 ```

### <a name="internal-only-stateless-aspnet-core-service"></a><span data-ttu-id="f1bc8-280">Csak belső állapot nélküli ASP.NET Core szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f1bc8-280">Internal-only stateless ASP.NET Core service</span></span>
<span data-ttu-id="f1bc8-281">Csak a fürtön belül hívását állapotmentes szolgáltatások egyedi URL-eket kell használnia, és dinamikusan kiosztott portok több szolgáltatás együttműködésének biztosításához.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-281">Stateless services that are only called from within the cluster should use unique URLs and dynamically assigned ports to ensure cooperation between multiple services.</span></span> <span data-ttu-id="f1bc8-282">A következő konfiguráció ajánlott:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-282">The following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="f1bc8-283">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="f1bc8-283">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f1bc8-284">Webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="f1bc8-284">Web server</span></span> | <span data-ttu-id="f1bc8-285">vércse</span><span class="sxs-lookup"><span data-stu-id="f1bc8-285">Kestrel</span></span> | <span data-ttu-id="f1bc8-286">Bár WebListener belső állapotmentes szolgáltatások is használhatók, vércse a javasolt kiszolgálót úgy, hogy több-gazdagépet megosztásához szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-286">Although WebListener may be used for internal stateless services, Kestrel is the recommended server to allow multiple service instances to share a host.</span></span>  |
| <span data-ttu-id="f1bc8-287">Port konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1bc8-287">Port configuration</span></span> | <span data-ttu-id="f1bc8-288">dinamikusan kiosztott</span><span class="sxs-lookup"><span data-stu-id="f1bc8-288">dynamically assigned</span></span> | <span data-ttu-id="f1bc8-289">Az állapotalapú szolgáltatás több replika megoszthatja a gazdafolyamat vagy a gazda operációs rendszer, és így kell egyedi portok.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-289">Multiple replicas of a stateful service may share a host process or host operating system and thus will need unique ports.</span></span> |
| <span data-ttu-id="f1bc8-290">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="f1bc8-290">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="f1bc8-291">UseUniqueServiceUrl</span><span class="sxs-lookup"><span data-stu-id="f1bc8-291">UseUniqueServiceUrl</span></span> | <span data-ttu-id="f1bc8-292">A dinamikus port-hozzárendelés Ez a beállítás megakadályozza a korábban leírt téves identitás probléma.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-292">With dynamic port assignment, this setting prevents the mistaken identity issue described earlier.</span></span> |
| <span data-ttu-id="f1bc8-293">InstanceCount</span><span class="sxs-lookup"><span data-stu-id="f1bc8-293">InstanceCount</span></span> | <span data-ttu-id="f1bc8-294">bármely</span><span class="sxs-lookup"><span data-stu-id="f1bc8-294">any</span></span> | <span data-ttu-id="f1bc8-295">A példányok száma beállítás állítható be értéket a szolgáltatás működtetéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-295">The instance count setting can be set to any value necessary to operate the service.</span></span> |

### <a name="internal-only-stateful-aspnet-core-service"></a><span data-ttu-id="f1bc8-296">Csak belső állapot-nyilvántartó ASP.NET Core szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f1bc8-296">Internal-only stateful ASP.NET Core service</span></span>
<span data-ttu-id="f1bc8-297">Állapotalapú szolgáltatások, amelyek csak a fürtön belül hívását dinamikusan hozzárendelt portok használjon több szolgáltatás együttműködésének biztosításához.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-297">Stateful services that are only called from within the cluster should use dynamically assigned ports to ensure cooperation between multiple services.</span></span> <span data-ttu-id="f1bc8-298">A következő konfiguráció ajánlott:</span><span class="sxs-lookup"><span data-stu-id="f1bc8-298">The following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="f1bc8-299">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="f1bc8-299">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f1bc8-300">Webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="f1bc8-300">Web server</span></span> | <span data-ttu-id="f1bc8-301">vércse</span><span class="sxs-lookup"><span data-stu-id="f1bc8-301">Kestrel</span></span> | <span data-ttu-id="f1bc8-302">A `WebListenerCommunicationListener` több replikák osztozik egy állapotalapú szolgáltatások használatra nem szolgál.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-302">The `WebListenerCommunicationListener` is not designed for use by stateful services in which replicas share a host process.</span></span> |
| <span data-ttu-id="f1bc8-303">Port konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1bc8-303">Port configuration</span></span> | <span data-ttu-id="f1bc8-304">dinamikusan kiosztott</span><span class="sxs-lookup"><span data-stu-id="f1bc8-304">dynamically assigned</span></span> | <span data-ttu-id="f1bc8-305">Az állapotalapú szolgáltatás több replika megoszthatja a gazdafolyamat vagy a gazda operációs rendszer, és így kell egyedi portok.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-305">Multiple replicas of a stateful service may share a host process or host operating system and thus will need unique ports.</span></span> |
| <span data-ttu-id="f1bc8-306">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="f1bc8-306">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="f1bc8-307">UseUniqueServiceUrl</span><span class="sxs-lookup"><span data-stu-id="f1bc8-307">UseUniqueServiceUrl</span></span> | <span data-ttu-id="f1bc8-308">A dinamikus port-hozzárendelés Ez a beállítás megakadályozza a korábban leírt téves identitás probléma.</span><span class="sxs-lookup"><span data-stu-id="f1bc8-308">With dynamic port assignment, this setting prevents the mistaken identity issue described earlier.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f1bc8-309">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1bc8-309">Next steps</span></span>
[<span data-ttu-id="f1bc8-310">A Service Fabric-alkalmazás hibakeresése a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="f1bc8-310">Debug your Service Fabric application by using Visual Studio</span></span>](service-fabric-debugging-your-application.md)

<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
