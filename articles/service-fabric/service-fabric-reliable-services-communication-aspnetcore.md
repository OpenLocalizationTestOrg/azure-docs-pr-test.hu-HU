---
title: "az ASP.NET Core hello aaaService kommunikáció |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse ASP.NET alapvető az állapotmentes és állapotalapú Reliable Services."
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
ms.openlocfilehash: 6e6a83ab04390150292f63de5d9b51d290284e50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-core-in-service-fabric-reliable-services"></a><span data-ttu-id="d4fba-103">Az ASP.NET Core a Service Fabric megbízható szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="d4fba-103">ASP.NET Core in Service Fabric Reliable Services</span></span>

<span data-ttu-id="d4fba-104">Az ASP.NET Core egy új nyílt forráskódú és platformfüggetlen keretrendszer modern felhőalapú internetre kapcsolódó alkalmazások, például webalkalmazások, az IoT-alkalmazásokhoz és a mobil háttérkiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="d4fba-104">ASP.NET Core is a new open-source and cross-platform framework for building modern cloud-based Internet-connected applications, such as web apps, IoT apps, and mobile backends.</span></span> 

<span data-ttu-id="d4fba-105">Ez a cikk egy részletes útmutató toohosting a Service Fabric Reliable Services hello segítségével szolgáltatásai az ASP.NET Core **Microsoft.ServiceFabric.AspNetCore.** * NuGet-csomagok készlete.</span><span class="sxs-lookup"><span data-stu-id="d4fba-105">This article is an in-depth guide toohosting ASP.NET Core services in Service Fabric Reliable Services using hello **Microsoft.ServiceFabric.AspNetCore.*** set of NuGet packages.</span></span>

<span data-ttu-id="d4fba-106">Egy bevezető oktatóanyag az ASP.NET Core a Service Fabric és a fejlesztési környezet beállítása történt utasításokat lásd: [egy webes előtér-, az ASP.NET Core segítségével alkalmazás felépítése](service-fabric-add-a-web-frontend.md).</span><span class="sxs-lookup"><span data-stu-id="d4fba-106">For an introductory tutorial on ASP.NET Core in Service Fabric and instructions on getting your development environment set up, see [Building a web front-end for your application using ASP.NET Core](service-fabric-add-a-web-frontend.md).</span></span>

<span data-ttu-id="d4fba-107">Ez a cikk többi hello azt feltételezi, hogy ismeri az ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4fba-107">hello rest of this article assumes you are already familiar with ASP.NET Core.</span></span> <span data-ttu-id="d4fba-108">Ha nem, azt javasoljuk egész hello [ASP.NET Core – alapok](https://docs.microsoft.com/aspnet/core/fundamentals/index).</span><span class="sxs-lookup"><span data-stu-id="d4fba-108">If not, we recommend reading through hello [ASP.NET Core fundamentals](https://docs.microsoft.com/aspnet/core/fundamentals/index).</span></span>

## <a name="aspnet-core-in-hello-service-fabric-environment"></a><span data-ttu-id="d4fba-109">Az ASP.NET Core hello Service Fabric-környezetben</span><span class="sxs-lookup"><span data-stu-id="d4fba-109">ASP.NET Core in hello Service Fabric environment</span></span>

<span data-ttu-id="d4fba-110">Miközben az ASP.NET Core alkalmazásokat is .NET Core-on vagy teljes .NET-keretrendszer, a Service Fabric szolgáltatás jelenleg csak futtathatja hello hello teljes .NET-keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="d4fba-110">While ASP.NET Core apps can run on .NET Core or on hello full .NET Framework, Service Fabric services currently can only run on hello full .NET Framework.</span></span> <span data-ttu-id="d4fba-111">Ez azt jelenti, hogy az ASP.NET Core Service Fabric-szolgáltatás építésekor, kell továbbra is céloznia hello teljes .NET-keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="d4fba-111">This means when you build an ASP.NET  Core Service Fabric service, you must still target hello full .NET Framework.</span></span>

<span data-ttu-id="d4fba-112">Az ASP.NET Core a Service Fabric két különböző módon használható:</span><span class="sxs-lookup"><span data-stu-id="d4fba-112">ASP.NET Core can be used in two different ways in Service Fabric:</span></span>
 - <span data-ttu-id="d4fba-113">**És a Vendég végrehajtható fájlja üzemeltetett**.</span><span class="sxs-lookup"><span data-stu-id="d4fba-113">**Hosted as a guest executable**.</span></span> <span data-ttu-id="d4fba-114">Ez a elsősorban használt toorun meglévő ASP.NET Core alkalmazások a Service Fabric code változtatás nélkül.</span><span class="sxs-lookup"><span data-stu-id="d4fba-114">This is primarily used toorun existing ASP.NET Core applications on Service Fabric with no code changes.</span></span>
 - <span data-ttu-id="d4fba-115">**A megbízható szolgáltatás futhat**.</span><span class="sxs-lookup"><span data-stu-id="d4fba-115">**Run inside a Reliable Service**.</span></span> <span data-ttu-id="d4fba-116">Ez lehetővé teszi, hogy a Service Fabric-futtatókörnyezet hello jobb integrációt, és lehetővé teszi, hogy a állapot-nyilvántartó ASP.NET Core services.</span><span class="sxs-lookup"><span data-stu-id="d4fba-116">This allows better integration with hello Service Fabric runtime and allows stateful ASP.NET Core services.</span></span>

<span data-ttu-id="d4fba-117">hello rest Ez a cikk azt ismerteti, hogyan belül egy megbízható szolgáltatás használja az ASP.NET Core toouse hello-ASP.NET Core integrációs összetevőket, amelyeket a Service Fabric SDK hello.</span><span class="sxs-lookup"><span data-stu-id="d4fba-117">hello rest of this article explains how toouse ASP.NET Core inside a Reliable Service using hello ASP.NET Core integration components that ship with hello Service Fabric SDK.</span></span> 

## <a name="service-fabric-service-hosting"></a><span data-ttu-id="d4fba-118">A Service Fabric-szolgáltatás üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="d4fba-118">Service Fabric service hosting</span></span>

<span data-ttu-id="d4fba-119">A Service Fabric egy vagy több példány, illetve a replikákat a szolgáltatás futtatásához egy *gazdafolyamat szolgáltatás*, egy végrehajtható fájl, amely a szolgáltatáskód hibáit futtatja.</span><span class="sxs-lookup"><span data-stu-id="d4fba-119">In Service Fabric, one or more instances and/or replicas of your service run in a *service host process*, an executable file that runs your service code.</span></span> <span data-ttu-id="d4fba-120">A szolgáltatás szerző saját hello szolgáltatás gazdafolyamat és a Service Fabric aktiválódik, és figyeli a meg.</span><span class="sxs-lookup"><span data-stu-id="d4fba-120">You, as a service author, own hello service host process and Service Fabric activates and monitors it for you.</span></span>

<span data-ttu-id="d4fba-121">Hagyományos ASP.NET (felfelé tooMVC 5) keresztül System.Web.dll szorosan összekapcsolt tooIIS.</span><span class="sxs-lookup"><span data-stu-id="d4fba-121">Traditional ASP.NET (up tooMVC 5) is tightly coupled tooIIS through System.Web.dll.</span></span> <span data-ttu-id="d4fba-122">Az ASP.NET Core hello webkiszolgáló és a webes alkalmazás közötti kizárás biztosít.</span><span class="sxs-lookup"><span data-stu-id="d4fba-122">ASP.NET Core provides a separation between hello web server and your web application.</span></span> <span data-ttu-id="d4fba-123">Így webes alkalmazások toobe hordozható különböző kiszolgálók között, és lehetővé teszi webes kiszolgálók toobe *önállóan üzemel*, ami azt jelenti, hogy elindul-e a webkiszolgáló a saját folyamat dedikált tulajdonában lévő megakadályozását tooa folyamatként például az IIS Server szoftver.</span><span class="sxs-lookup"><span data-stu-id="d4fba-123">This allows web applications toobe portable between different web servers and also allows web servers toobe *self-hosted*, which means you can start a web server in your own process, as opposed tooa process that is owned by dedicated web server software such as IIS.</span></span> 

<span data-ttu-id="d4fba-124">Rendelés toocombine a Service Fabric-szolgáltatás és az ASP.NET, a Vendég végrehajtható fájl vagy egy megbízható szolgáltatásban a képes toostart ASP.NET a szolgáltatás gazdafolyamat belül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d4fba-124">In order toocombine a Service Fabric service and ASP.NET, either as a guest executable or in a Reliable Service, you must be able toostart ASP.NET inside your service host process.</span></span> <span data-ttu-id="d4fba-125">Az ASP.NET Core önálló üzemeltető toodo lehetővé teszi ezt.</span><span class="sxs-lookup"><span data-stu-id="d4fba-125">ASP.NET Core self-hosting allows you toodo this.</span></span>

## <a name="hosting-aspnet-core-in-a-reliable-service"></a><span data-ttu-id="d4fba-126">Az ASP.NET Core működnie a tároló</span><span class="sxs-lookup"><span data-stu-id="d4fba-126">Hosting ASP.NET Core in a Reliable Service</span></span>
<span data-ttu-id="d4fba-127">Önálló üzemeltetett ASP.NET Core alkalmazások általában egy WebHost létrehozása egy alkalmazás belépési pont, például a hello `static void Main()` metódus a `Program.cs`.</span><span class="sxs-lookup"><span data-stu-id="d4fba-127">Typically, self-hosted ASP.NET Core applications create a WebHost in an application's entry point, such as hello `static void Main()` method in `Program.cs`.</span></span> <span data-ttu-id="d4fba-128">Ebben az esetben a hello WebHost hello életciklusát kötött toohello életciklus hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="d4fba-128">In this case, hello lifecycle of hello WebHost is bound toohello lifecycle of hello process.</span></span>

![Az ASP.NET Core üzemeltető folyamatban][0]

<span data-ttu-id="d4fba-130">Azonban hello alkalmazás belépési pont nincs hello megfelelő helyen toocreate a Webállomás megbízható szolgáltatásban, mert hello alkalmazás belépési pont csak akkor használatos tooregister szolgáltatás írja be a hello Service Fabric-futtatókörnyezet, az, hogy az adott szolgáltatás példányának létrehozhat Adja meg.</span><span class="sxs-lookup"><span data-stu-id="d4fba-130">However, hello application entry point is not hello right place toocreate a WebHost in a Reliable Service, because hello application entry point is only used tooregister a service type with hello Service Fabric runtime, so that it may create instances of that service type.</span></span> <span data-ttu-id="d4fba-131">a Webállomás hello létre kell hozni egy megbízható szolgáltatás magát.</span><span class="sxs-lookup"><span data-stu-id="d4fba-131">hello WebHost should be created in a Reliable Service itself.</span></span> <span data-ttu-id="d4fba-132">Hello szolgáltatás gazdafolyamaton belül szolgáltatáspéldány és/vagy a replikák Lépkedjen végig több életciklusának.</span><span class="sxs-lookup"><span data-stu-id="d4fba-132">Within hello service host process, service instances and/or replicas can go through multiple lifecycles.</span></span> 

<span data-ttu-id="d4fba-133">A megbízható példánya a származó osztály által képviselt `StatelessService` vagy `StatefulService`.</span><span class="sxs-lookup"><span data-stu-id="d4fba-133">A Reliable Service instance is represented by your service class deriving from `StatelessService` or `StatefulService`.</span></span> <span data-ttu-id="d4fba-134">hello kommunikációs verem szolgáltatás szerepel egy `ICommunicationListener` végrehajtása a szolgáltatás osztályban.</span><span class="sxs-lookup"><span data-stu-id="d4fba-134">hello communication stack for a service is contained in an `ICommunicationListener` implementation in your service class.</span></span> <span data-ttu-id="d4fba-135">Hello `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet-csomagok tartalmaznak implementációja `ICommunicationListener` , indítsa el, és kezelheti az ASP.NET Core WebHost hello vércse vagy a megbízható szolgáltatás WebListener.</span><span class="sxs-lookup"><span data-stu-id="d4fba-135">hello `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet packages contain implementations of `ICommunicationListener` that start and manage hello ASP.NET Core WebHost for either Kestrel or WebListener in a Reliable Service.</span></span>

![Az ASP.NET Core működnie a tároló][1]

## <a name="aspnet-core-icommunicationlisteners"></a><span data-ttu-id="d4fba-137">Az ASP.NET Core ICommunicationListeners</span><span class="sxs-lookup"><span data-stu-id="d4fba-137">ASP.NET Core ICommunicationListeners</span></span>
<span data-ttu-id="d4fba-138">Hello `ICommunicationListener` megvalósításait vércse és WebListener a hello `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet-csomagok hasonló felhasználási minták rendelkezik, de az adott tooeach webkiszolgáló némileg eltérő műveleteket végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="d4fba-138">hello `ICommunicationListener` implementations for Kestrel and WebListener in hello  `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet packages have similar use patterns but perform slightly different actions specific tooeach web server.</span></span> 

<span data-ttu-id="d4fba-139">Mindkét kommunikációs figyelőket adja meg a következő argumentumok hello konstruktort:</span><span class="sxs-lookup"><span data-stu-id="d4fba-139">Both communication listeners provide a constructor that takes hello following arguments:</span></span>
 - <span data-ttu-id="d4fba-140">**`ServiceContext serviceContext`**: hello `ServiceContext` hello szolgáltatást futtató kapcsolatos információkat tartalmazó objektum.</span><span class="sxs-lookup"><span data-stu-id="d4fba-140">**`ServiceContext serviceContext`**: hello `ServiceContext` object that contains information about hello running service.</span></span>
 - <span data-ttu-id="d4fba-141">**`string endpointName`**: hello nevét egy `Endpoint` ServiceManifest.xml konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d4fba-141">**`string endpointName`**: hello name of an `Endpoint` configuration in ServiceManifest.xml.</span></span> <span data-ttu-id="d4fba-142">Ez az elsősorban ahol hello két kommunikációs figyelőket eltérőek: WebListener **szükséges** egy `Endpoint` konfigurációs, vércse azonban nem.</span><span class="sxs-lookup"><span data-stu-id="d4fba-142">This is primarily where hello two communication listeners differ: WebListener **requires** an `Endpoint` configuration, while Kestrel does not.</span></span>
 - <span data-ttu-id="d4fba-143">**`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: egy lambda, amely hoz létre, és visszatérési megvalósító egy `IWebHost`.</span><span class="sxs-lookup"><span data-stu-id="d4fba-143">**`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: a lambda that you implement in which you create and return an `IWebHost`.</span></span> <span data-ttu-id="d4fba-144">Ez lehetővé teszi, hogy tooconfigure `IWebHost` hello módja az ASP.NET Core alkalmazásban normális esetben tenné.</span><span class="sxs-lookup"><span data-stu-id="d4fba-144">This allows you tooconfigure `IWebHost` hello way you normally would in an ASP.NET Core application.</span></span> <span data-ttu-id="d4fba-145">hello lambda biztosít egy URL-címet, amely jön létre, attól függően, hogy a Service Fabric-integráció hello-lehetőségek, használata és hello `Endpoint` konfigurációs megadnia.</span><span class="sxs-lookup"><span data-stu-id="d4fba-145">hello lambda provides a URL which is generated for you depending on hello Service Fabric integration options you use and hello `Endpoint` configuration you provide.</span></span> <span data-ttu-id="d4fba-146">URL-cím majd kell módosítani vagy vettük-toostart hello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="d4fba-146">That URL can then be modified or used as-is toostart hello web server.</span></span>

## <a name="service-fabric-integration-middleware"></a><span data-ttu-id="d4fba-147">A Service Fabric-integráció köztes</span><span class="sxs-lookup"><span data-stu-id="d4fba-147">Service Fabric integration middleware</span></span>
<span data-ttu-id="d4fba-148">Hello `Microsoft.ServiceFabric.Services.AspNetCore` NuGet-csomag magában foglalja a hello `UseServiceFabricIntegration` bővítmény metódusa `IWebHostBuilder` , amely a Service Fabric-kompatibilis köztes ad.</span><span class="sxs-lookup"><span data-stu-id="d4fba-148">hello `Microsoft.ServiceFabric.Services.AspNetCore` NuGet package includes hello `UseServiceFabricIntegration` extension method on `IWebHostBuilder` that adds Service Fabric-aware middleware.</span></span> <span data-ttu-id="d4fba-149">A köztes konfigurálja hello vércse vagy WebListener `ICommunicationListener` tooregister egy egyedi URL-címe a Service Fabric-szolgáltatás hello és szerint hitelesíti ügyfél kérelmek tooensure ügyfelek toohello jobb szolgáltatás csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="d4fba-149">This middleware configures hello Kestrel or WebListener `ICommunicationListener` tooregister a unique service URL with hello Service Fabric Naming Service and then validates client requests tooensure clients are connecting toohello right service.</span></span> <span data-ttu-id="d4fba-150">Erre akkor szükség a megosztott gazdagép-környezetben, például a Service Fabric, ahol több webalkalmazás hello futtathatja ugyanazon fizikai vagy virtuális gép, de ne használjon egyedi állomásnevek, tooprevent ügyfelek véletlenül csatlakozzon toohello megfelelő szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d4fba-150">This is necessary in a shared-host environment such as Service Fabric, where multiple web applications can run on hello same physical or virtual machine but do not use unique host names, tooprevent clients from mistakenly connecting toohello wrong service.</span></span> <span data-ttu-id="d4fba-151">Ebben a forgatókönyvben hello a következő szakaszban részletesen ismertetett.</span><span class="sxs-lookup"><span data-stu-id="d4fba-151">This scenario is described in more detail in hello next section.</span></span>

### <a name="a-case-of-mistaken-identity"></a><span data-ttu-id="d4fba-152">A gyorsítás esetében is téves identitás</span><span class="sxs-lookup"><span data-stu-id="d4fba-152">A case of mistaken identity</span></span>
<span data-ttu-id="d4fba-153">Szolgáltatás replikákat protocol, függetlenül egyedi IP:port kombinációjából figyelése.</span><span class="sxs-lookup"><span data-stu-id="d4fba-153">Service replicas, regardless of protocol, listen on a unique IP:port combination.</span></span> <span data-ttu-id="d4fba-154">A szolgáltatás replika megkezdése IP:port a végpont figyel, után a végpont címét toohello Service Fabric-szolgáltatás ahol, könnyen megtalálhatók legyenek az ügyfelek vagy egyéb szolgáltatások jelenti.</span><span class="sxs-lookup"><span data-stu-id="d4fba-154">Once a service replica has started listening on an IP:port endpoint, it reports that endpoint address toohello Service Fabric Naming Service where it can be discovered by clients or other services.</span></span> <span data-ttu-id="d4fba-155">Ha a szolgáltatások használata dinamikusan hozzárendelt alkalmazás-porttal rendelkezik, a szolgáltatás replika helyezi használhat, amely korábban egy másik szolgáltatás ugyanazon IP:port végpont hello hello ugyanazon fizikai vagy virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="d4fba-155">If services use dynamically-assigned application ports, a service replica may coincidentally use hello same IP:port endpoint of another service that was previously on hello same physical or virtual machine.</span></span> <span data-ttu-id="d4fba-156">Ez is okozhatja, hogy egy ügyfél toomistakely csatlakozás toohello megfelelő szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d4fba-156">This can cause a client toomistakely connect toohello wrong service.</span></span> <span data-ttu-id="d4fba-157">Ez akkor fordulhat elő, ha a következő eseményekre hello történik:</span><span class="sxs-lookup"><span data-stu-id="d4fba-157">This can happen if hello following sequence of events occur:</span></span>

 1. <span data-ttu-id="d4fba-158">A szolgáltatás figyeli 10.0.0.1:30000 HTTP Protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="d4fba-158">Service A listens on 10.0.0.1:30000 over HTTP.</span></span> 
 2. <span data-ttu-id="d4fba-159">Ügyfél szolgáltatás A feloldását, és lekérdezi a cím 10.0.0.1:30000</span><span class="sxs-lookup"><span data-stu-id="d4fba-159">Client resolves Service A and gets address 10.0.0.1:30000</span></span>
 3. <span data-ttu-id="d4fba-160">A szolgáltatás egy lépés tooa másik csomópont.</span><span class="sxs-lookup"><span data-stu-id="d4fba-160">Service A moves tooa different node.</span></span>
 4. <span data-ttu-id="d4fba-161">B szolgáltatás el van helyezve 10.0.0.1 és helyezi a használja ugyanazt a portot 30000 hello.</span><span class="sxs-lookup"><span data-stu-id="d4fba-161">Service B is placed on 10.0.0.1 and coincidentally uses hello same port 30000.</span></span>
 5. <span data-ttu-id="d4fba-162">-Ügyfél megkísérel tooconnect tooservice A gyorsítótárazott cím 10.0.0.1:30000 együtt.</span><span class="sxs-lookup"><span data-stu-id="d4fba-162">Client attempts tooconnect tooservice A with cached address 10.0.0.1:30000.</span></span>
 6. <span data-ttu-id="d4fba-163">Ügyfél már csatlakozik-e sikeresen csatlakoztatott tooservice nem megvalósító B toohello megfelelő szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d4fba-163">Client is now successfully connected tooservice B not realizing it is connected toohello wrong service.</span></span>

<span data-ttu-id="d4fba-164">Ez azt okozhatja, hogy a hibákat, amelyek lehetnek nehéz toodiagnose véletlenszerű időpontokban.</span><span class="sxs-lookup"><span data-stu-id="d4fba-164">This can cause bugs at random times that can be difficult toodiagnose.</span></span> 

### <a name="using-unique-service-urls"></a><span data-ttu-id="d4fba-165">Egyedi URL-címek használata</span><span class="sxs-lookup"><span data-stu-id="d4fba-165">Using unique service URLs</span></span>
<span data-ttu-id="d4fba-166">tooprevent, szolgáltatások is egy végpont toohello Naming Service egyedi azonosítóval rendelkező közzétételére, és ezután ellenőrizze, hogy egyedi azonosítója ügyfélkérelmek során.</span><span class="sxs-lookup"><span data-stu-id="d4fba-166">tooprevent this, services can post an endpoint toohello Naming Service with a unique identifier, and then validate that unique identifier during client requests.</span></span> <span data-ttu-id="d4fba-167">Ez a egy együttműködési művelet megbízható környezet nem rosszindulatú-bérlői szolgáltatások között.</span><span class="sxs-lookup"><span data-stu-id="d4fba-167">This is a cooperative action between services in a non-hostile-tenant trusted environment.</span></span> <span data-ttu-id="d4fba-168">Ez nem biztosítja a biztonságos szolgáltatás hitelesítési rosszindulatú bérlői környezetben.</span><span class="sxs-lookup"><span data-stu-id="d4fba-168">This does not provide secure service authentication in a hostile-tenant environment.</span></span>

<span data-ttu-id="d4fba-169">A megbízható környezet esetén hello hello által hozzáadott köztes `UseServiceFabricIntegration` metódus automatikusan hozzáfűzi egy egyedi azonosítót toohello címet, amely toohello Naming Service visszaküldi, és ellenőrzi, hogy minden kérelemnél azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d4fba-169">In a trusted environment, hello middleware that's added by hello `UseServiceFabricIntegration` method automatically appends a unique identifier toohello address that is posted toohello Naming Service and validates that identifier on each request.</span></span> <span data-ttu-id="d4fba-170">Ha hello azonosítója nem egyezik, akkor a hello köztes azonnal az egy HTTP 410 Megszűnt választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="d4fba-170">If hello identifier does not match, hello middleware immediately returns an HTTP 410 Gone response.</span></span>

<span data-ttu-id="d4fba-171">A köztes felhasználása ellenőrizniük kell egy dinamikusan hozzárendelt portot használó szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="d4fba-171">Services that use a dynamically-assigned port should make use of this middleware.</span></span>

<span data-ttu-id="d4fba-172">Egy rögzített egyedi portot használó szolgáltatások együttműködési környezetben nincs probléma.</span><span class="sxs-lookup"><span data-stu-id="d4fba-172">Services that use a fixed unique port do not have this problem in a cooperative environment.</span></span> <span data-ttu-id="d4fba-173">Egy rögzített egyedi port általában szolgál, amely egy jól ismert port szükséges az ügyfél alkalmazások tooconnect kívülről elérhető szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="d4fba-173">A fixed unique port is typically used for externally-facing services that need a well-known port for client applications tooconnect to.</span></span> <span data-ttu-id="d4fba-174">A legtöbb Internet felé néző webes alkalmazások például használja a 80-as vagy 443-as port webes böngésző kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="d4fba-174">For example, most Internet-facing web applications will use port 80 or 443 for web browser connections.</span></span> <span data-ttu-id="d4fba-175">Ebben az esetben hello egyedi azonosítója nem engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="d4fba-175">In this case, hello unique identifier should not be enabled.</span></span>

<span data-ttu-id="d4fba-176">hello alábbi ábrán látható hello áramlanak a hello köztes engedélyezve:</span><span class="sxs-lookup"><span data-stu-id="d4fba-176">hello following diagram shows hello request flow with hello middleware enabled:</span></span>

![Service Fabric ASP.NET Core integráció][2]

<span data-ttu-id="d4fba-178">Vércse és a WebListener `ICommunicationListener` megvalósítások a mechanizmus használata pontosan hello azonos módon.</span><span class="sxs-lookup"><span data-stu-id="d4fba-178">Both Kestrel and WebListener `ICommunicationListener` implementations use this mechanism in exactly hello same way.</span></span> <span data-ttu-id="d4fba-179">Bár WebListener belső megkülönböztetni azokat a kérelmeket az alapul szolgáló hello segítségével egyedi URL-cím útvonalakon alapján *http.sys* port megosztása szolgáltatás, a funkció *nem* hello WebListener általhasznált`ICommunicationListener` végrehajtása, mert a korábban ismertetett hello forgatókönyvben HTTP 503-as és a HTTP 404-es hiba állapotkódok vezethet.</span><span class="sxs-lookup"><span data-stu-id="d4fba-179">Although WebListener can internally differentiate requests based on unique URL paths using hello underlying *http.sys* port sharing feature, that functionality is *not* used by hello WebListener `ICommunicationListener` implementation because that will result in HTTP 503 and HTTP 404 error status codes in hello scenario described earlier.</span></span> <span data-ttu-id="d4fba-180">Amely pedig rendkívül megnehezíti az ügyfelek toodetermine hello szándékával hello hiba, mert HTTP 503-as és a HTTP 404-es már gyakran használt tooindicate más hibák.</span><span class="sxs-lookup"><span data-stu-id="d4fba-180">That in turn makes it very difficult for clients toodetermine hello intent of hello error, as HTTP 503 and HTTP 404 are already commonly used tooindicate other errors.</span></span> <span data-ttu-id="d4fba-181">Emiatt vércse és WebListener `ICommunicationListener` megvalósítások szabványosítására a hello által biztosított hello köztes `UseServiceFabricIntegration` kiterjesztésmetódus, hogy az ügyfelek csak kell tooperform szolgáltatásvégpont újra feloldani a HTTP 410 válaszok művelet.</span><span class="sxs-lookup"><span data-stu-id="d4fba-181">Thus, both Kestrel and WebListener `ICommunicationListener` implementations standardize on hello middleware provided by hello `UseServiceFabricIntegration` extension method so that clients only need tooperform a service endpoint re-resolve action on HTTP 410 responses.</span></span>

## <a name="weblistener-in-reliable-services"></a><span data-ttu-id="d4fba-182">A megbízható szolgáltatások WebListener</span><span class="sxs-lookup"><span data-stu-id="d4fba-182">WebListener in Reliable Services</span></span>
<span data-ttu-id="d4fba-183">WebListener használható egy megbízható szolgáltatás hello importálásával **Microsoft.ServiceFabric.AspNetCore.WebListener** NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="d4fba-183">WebListener can be used in a Reliable Service by importing hello **Microsoft.ServiceFabric.AspNetCore.WebListener** NuGet package.</span></span> <span data-ttu-id="d4fba-184">Ez a csomag tartalmaz `WebListenerCommunicationListener`, megvalósítása `ICommunicationListener`, ami lehetővé teszi az ASP.NET Core WebHost belül egy megbízható szolgáltatás toocreate hello webkiszolgálóval WebListener használatával.</span><span class="sxs-lookup"><span data-stu-id="d4fba-184">This package contains `WebListenerCommunicationListener`, an implementation of `ICommunicationListener`, that allows you toocreate an ASP.NET Core WebHost inside a Reliable Service using WebListener as hello web server.</span></span>

<span data-ttu-id="d4fba-185">WebListener épül hello [Windows HTTP-kiszolgáló API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="d4fba-185">WebListener is built on hello [Windows HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx).</span></span> <span data-ttu-id="d4fba-186">Ez a hello használja *http.sys* kernel-illesztőprogram használják az IIS tooprocess HTTP-kérelmek és irányíthatja őket tooprocesses, amelyeken webalkalmazások futnak.</span><span class="sxs-lookup"><span data-stu-id="d4fba-186">This uses hello *http.sys* kernel driver used by IIS tooprocess HTTP requests and route them tooprocesses running web applications.</span></span> <span data-ttu-id="d4fba-187">Ez lehetővé teszi több folyamatot hello ugyanazon fizikai vagy virtuális gép toohost webes alkalmazások hello ugyanazt a portot, egyedi URL-cím vagy állomásnév használatát.</span><span class="sxs-lookup"><span data-stu-id="d4fba-187">This allows multiple processes on hello same physical or virtual machine toohost web applications on hello same port, disambiguated by either a unique URL path or hostname.</span></span> <span data-ttu-id="d4fba-188">Ezek a funkciók hasznosak a Service Fabric hello lévő több webhely üzemeltetésére ugyanabban a fürtben.</span><span class="sxs-lookup"><span data-stu-id="d4fba-188">These features are useful in Service Fabric for hosting multiple websites in hello same cluster.</span></span>

<span data-ttu-id="d4fba-189">hello következő diagram azt ábrázolja, hogyan használja a WebListener a hello *http.sys* kernel illesztőprogramját a Windows-portmegosztás:</span><span class="sxs-lookup"><span data-stu-id="d4fba-189">hello following diagram illustrates how WebListener uses hello *http.sys* kernel driver on Windows for port sharing:</span></span>

![a HTTP.sys][3]

### <a name="weblistener-in-a-stateless-service"></a><span data-ttu-id="d4fba-191">Az állapotmentes szolgáltatások WebListener</span><span class="sxs-lookup"><span data-stu-id="d4fba-191">WebListener in a stateless service</span></span>
<span data-ttu-id="d4fba-192">toouse `WebListener` állapotmentes szolgáltatások, a felülbírálás hello `CreateServiceInstanceListeners` metódust, és térjen vissza a `WebListenerCommunicationListener` példány:</span><span class="sxs-lookup"><span data-stu-id="d4fba-192">toouse `WebListener` in a stateless service, override hello `CreateServiceInstanceListeners` method and return a `WebListenerCommunicationListener` instance:</span></span>

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

### <a name="weblistener-in-a-stateful-service"></a><span data-ttu-id="d4fba-193">Az állapotalapú service WebListener</span><span class="sxs-lookup"><span data-stu-id="d4fba-193">WebListener in a stateful service</span></span>

<span data-ttu-id="d4fba-194">`WebListenerCommunicationListener`jelenleg nincs használatra szolgál az állapotalapú szolgáltatások rendelkező hello alapjául szolgáló esedékes toocomplications *http.sys* port megosztása szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d4fba-194">`WebListenerCommunicationListener` is currently not designed for use in stateful services due toocomplications with hello underlying *http.sys* port sharing feature.</span></span> <span data-ttu-id="d4fba-195">További információkért tekintse meg a következő szakasz a dinamikus port lefoglalását WebListener hello.</span><span class="sxs-lookup"><span data-stu-id="d4fba-195">For more information, see hello following section on dynamic port allocation with WebListener.</span></span> <span data-ttu-id="d4fba-196">Állapotalapú szolgáltatások esetén vércse a webkiszolgáló ajánlott hello.</span><span class="sxs-lookup"><span data-stu-id="d4fba-196">For stateful services, Kestrel is hello recommended web server.</span></span>

### <a name="endpoint-configuration"></a><span data-ttu-id="d4fba-197">Végpont-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="d4fba-197">Endpoint configuration</span></span>

<span data-ttu-id="d4fba-198">Egy `Endpoint` konfigurálni kell a Windows HTTP kiszolgáló API, beleértve a WebListener hello használó webkiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="d4fba-198">An `Endpoint` configuration is required for web servers that use hello Windows HTTP Server API, including WebListener.</span></span> <span data-ttu-id="d4fba-199">HTTP-kiszolgáló API a Windows hello használó webkiszolgálók először kell lefoglalni az URL-cím elé *http.sys* (Ez általában érhető el a hello [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) eszköz).</span><span class="sxs-lookup"><span data-stu-id="d4fba-199">Web servers that use hello Windows HTTP Server API must first reserve their URL with *http.sys* (this is normally accomplished with hello [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) tool).</span></span> <span data-ttu-id="d4fba-200">Ehhez a művelethez emelt szintű jogosultságokkal, amely a szolgáltatások alapértelmezés szerint nem rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="d4fba-200">This action requires elevated privileges that your services by default do not have.</span></span> <span data-ttu-id="d4fba-201">hello "http" vagy "https" beállításait hello `Protocol` hello tulajdonságának `Endpoint` konfiguráció *ServiceManifest.xml* használt kifejezetten tooinstruct hello Service Fabric-futtatókörnyezet tooregister olyanURL-címe*http.sys* hello segítségével a nevünkben [ *erős helyettesítő* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL-előtagját.</span><span class="sxs-lookup"><span data-stu-id="d4fba-201">hello "http" or "https" options for hello `Protocol` property of hello `Endpoint` configuration in *ServiceManifest.xml* are used specifically tooinstruct hello Service Fabric runtime tooregister a URL with *http.sys* on your behalf using hello [*strong wildcard*](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL prefix.</span></span>

<span data-ttu-id="d4fba-202">Például tooreserve `http://+:80` egy szolgáltatás hello következő konfigurációt kell használni a ServiceManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="d4fba-202">For example, tooreserve `http://+:80` for a service, hello following configuration should be used in ServiceManifest.xml:</span></span>

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

<span data-ttu-id="d4fba-203">Hello végpontnév kell átadni toohello és `WebListenerCommunicationListener` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="d4fba-203">And hello endpoint name must be passed toohello `WebListenerCommunicationListener` constructor:</span></span>

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

#### <a name="use-weblistener-with-a-static-port"></a><span data-ttu-id="d4fba-204">A statikus port WebListener használata</span><span class="sxs-lookup"><span data-stu-id="d4fba-204">Use WebListener with a static port</span></span>
<span data-ttu-id="d4fba-205">toouse WebListener, statikus portot adja meg az hello port számát a hello `Endpoint` konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="d4fba-205">toouse a static port with WebListener, provide hello port number in hello `Endpoint` configuration:</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-weblistener-with-a-dynamic-port"></a><span data-ttu-id="d4fba-206">A dinamikus port WebListener használata</span><span class="sxs-lookup"><span data-stu-id="d4fba-206">Use WebListener with a dynamic port</span></span>
<span data-ttu-id="d4fba-207">toouse WebListener, dinamikusan hozzárendelt portot, hagyja el hello `Port` hello tulajdonság `Endpoint` konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="d4fba-207">toouse a dynamically assigned port with WebListener, omit hello `Port` property in hello `Endpoint` configuration:</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

<span data-ttu-id="d4fba-208">Vegye figyelembe, hogy a dinamikus port lefoglalta egy `Endpoint` a konfiguráció csak egyetlen port biztosít *állomás folyamatonként*.</span><span class="sxs-lookup"><span data-stu-id="d4fba-208">Note that a dynamic port allocated by an `Endpoint` configuration only provides one port *per host process*.</span></span> <span data-ttu-id="d4fba-209">hello üzemeltetési modell lehetővé teszi, hogy több szolgáltatás példányok és/vagy a replikák toobe hello tárolt aktuális Service Fabric ugyanezt a folyamatot, ami azt jelenti, hogy minden egyes használ-e azonos porton keresztül hello kiosztása során hello `Endpoint` konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="d4fba-209">hello current Service Fabric hosting model allows multiple service instances and/or replicas toobe hosted in hello same process, meaning that each one will share hello same port when allocated through hello `Endpoint` configuration.</span></span> <span data-ttu-id="d4fba-210">Több WebListener példány megoszthatja az alapul szolgáló hello segítségével *http.sys* port megosztása szolgáltatást, de, amely nem támogatja `WebListenerCommunicationListener` miatt toohello komplikációk az ügyféli kérelmek részére okozna.</span><span class="sxs-lookup"><span data-stu-id="d4fba-210">Multiple WebListener instances can share a port using hello underlying *http.sys* port sharing feature, but that is not supported by `WebListenerCommunicationListener` due toohello complications it introduces for client requests.</span></span> <span data-ttu-id="d4fba-211">A dinamikus port használatról vércse webkiszolgáló ajánlott hello.</span><span class="sxs-lookup"><span data-stu-id="d4fba-211">For dynamic port usage, Kestrel is hello recommended web server.</span></span>

## <a name="kestrel-in-reliable-services"></a><span data-ttu-id="d4fba-212">A megbízható szolgáltatások vércse</span><span class="sxs-lookup"><span data-stu-id="d4fba-212">Kestrel in Reliable Services</span></span>
<span data-ttu-id="d4fba-213">Vércse használható egy megbízható szolgáltatás hello importálásával **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="d4fba-213">Kestrel can be used in a Reliable Service by importing hello **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet package.</span></span> <span data-ttu-id="d4fba-214">Ez a csomag tartalmaz `KestrelCommunicationListener`, megvalósítása `ICommunicationListener`, ami lehetővé teszi az ASP.NET Core WebHost belül egy megbízható szolgáltatás toocreate hello webkiszolgálóval vércse használatával.</span><span class="sxs-lookup"><span data-stu-id="d4fba-214">This package contains `KestrelCommunicationListener`, an implementation of `ICommunicationListener`, that allows you toocreate an ASP.NET Core WebHost inside a Reliable Service using Kestrel as hello web server.</span></span>

<span data-ttu-id="d4fba-215">Vércse, az ASP.NET Core platformfüggetlen webkiszolgáló libuv, a platformok közötti aszinkron i/o-szalagtár egyik alapján.</span><span class="sxs-lookup"><span data-stu-id="d4fba-215">Kestrel is a cross-platform web server for ASP.NET Core based on libuv, a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="d4fba-216">WebListener, eltérően vércse nem használ központi végponthoz vezető például *http.sys*.</span><span class="sxs-lookup"><span data-stu-id="d4fba-216">Unlike WebListener, Kestrel does not use a centralized endpoint manager such as *http.sys*.</span></span> <span data-ttu-id="d4fba-217">És WebListener, eltérően vércse nem támogatja a port megosztása több folyamat között.</span><span class="sxs-lookup"><span data-stu-id="d4fba-217">And unlike WebListener, Kestrel does not support port sharing between multiple processes.</span></span> <span data-ttu-id="d4fba-218">Minden példánynak vércse egy egyedi portot kell használnia.</span><span class="sxs-lookup"><span data-stu-id="d4fba-218">Each instance of Kestrel must use a unique port.</span></span>

![vércse][4]

### <a name="kestrel-in-a-stateless-service"></a><span data-ttu-id="d4fba-220">Az állapotmentes szolgáltatások vércse</span><span class="sxs-lookup"><span data-stu-id="d4fba-220">Kestrel in a stateless service</span></span>
<span data-ttu-id="d4fba-221">toouse `Kestrel` állapotmentes szolgáltatások, a felülbírálás hello `CreateServiceInstanceListeners` metódust, és térjen vissza a `KestrelCommunicationListener` példány:</span><span class="sxs-lookup"><span data-stu-id="d4fba-221">toouse `Kestrel` in a stateless service, override hello `CreateServiceInstanceListeners` method and return a `KestrelCommunicationListener` instance:</span></span>

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

### <a name="kestrel-in-a-stateful-service"></a><span data-ttu-id="d4fba-222">Az állapotalapú service vércse</span><span class="sxs-lookup"><span data-stu-id="d4fba-222">Kestrel in a stateful service</span></span>
<span data-ttu-id="d4fba-223">toouse `Kestrel` állapot-nyilvántartó szolgáltatásban, bírálja felül a hello `CreateServiceReplicaListeners` metódust, és térjen vissza a `KestrelCommunicationListener` példány:</span><span class="sxs-lookup"><span data-stu-id="d4fba-223">toouse `Kestrel` in a stateful service, override hello `CreateServiceReplicaListeners` method and return a `KestrelCommunicationListener` instance:</span></span>

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

<span data-ttu-id="d4fba-224">Ebben a példában egy egypéldányos példánya `IReliableStateManager` toohello WebHost függőségi injektálási tároló biztosítja.</span><span class="sxs-lookup"><span data-stu-id="d4fba-224">In this example, a singleton instance of `IReliableStateManager` is provided toohello WebHost dependency injection container.</span></span> <span data-ttu-id="d4fba-225">Ez nem feltétlenül szükséges, de toouse lehetővé teszi `IReliableStateManager` és az MVC-vezérlő műveletmetódusokhoz megbízható gyűjteményei.</span><span class="sxs-lookup"><span data-stu-id="d4fba-225">This is not strictly necessary, but it allows you toouse `IReliableStateManager` and Reliable Collections in your MVC controller action methods.</span></span>

<span data-ttu-id="d4fba-226">Vegye figyelembe, hogy egy `Endpoint` -konfiguráció nevét **nem** túl megadott`KestrelCommunicationListener` állapot-nyilvántartó szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="d4fba-226">Note that an `Endpoint` configuration name is **not** provided too`KestrelCommunicationListener` in a stateful service.</span></span> <span data-ttu-id="d4fba-227">Ennek a magyarázatát hello a következő szakasz részletesen.</span><span class="sxs-lookup"><span data-stu-id="d4fba-227">This is explained in more detail in hello following section.</span></span>

### <a name="endpoint-configuration"></a><span data-ttu-id="d4fba-228">Végpont-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="d4fba-228">Endpoint configuration</span></span>
<span data-ttu-id="d4fba-229">Egy `Endpoint` konfigurációra nincs szükség toouse vércse.</span><span class="sxs-lookup"><span data-stu-id="d4fba-229">An `Endpoint` configuration is not required toouse Kestrel.</span></span> 

<span data-ttu-id="d4fba-230">Vércse egy egyszerű önálló webkiszolgáló; WebListener (és HttpListener) eltérően nem kell egy `Endpoint` konfiguráció *ServiceManifest.xml* , mert nem szükséges URL-cím regisztrálása előzetes toostarting.</span><span class="sxs-lookup"><span data-stu-id="d4fba-230">Kestrel is a simple stand-alone web server; unlike WebListener (or HttpListener), it does not need an `Endpoint` configuration in *ServiceManifest.xml* because it does not require URL registration prior toostarting.</span></span> 

#### <a name="use-kestrel-with-a-static-port"></a><span data-ttu-id="d4fba-231">A statikus port vércse használata</span><span class="sxs-lookup"><span data-stu-id="d4fba-231">Use Kestrel with a static port</span></span>
<span data-ttu-id="d4fba-232">A statikus port konfigurálható hello `Endpoint` ServiceManifest.xml konfigurációját vércse való használatra.</span><span class="sxs-lookup"><span data-stu-id="d4fba-232">A static port can be configured in hello `Endpoint` configuration of ServiceManifest.xml for use with Kestrel.</span></span> <span data-ttu-id="d4fba-233">Ez azonban nem feltétlenül szükséges, akkor két lehetséges előnyöket nyújtja:</span><span class="sxs-lookup"><span data-stu-id="d4fba-233">Although this is not strictly necessary, it provides two potential benefits:</span></span>
 1. <span data-ttu-id="d4fba-234">Hello port nem esik hello alkalmazás porttartományát, ha az meg van nyitva hello OS tűzfalon keresztül a Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d4fba-234">If hello port does not fall in hello application port range, it is opened through hello OS firewall by Service Fabric.</span></span>
 2. <span data-ttu-id="d4fba-235">a megadott URL-cím tooyou keresztül hello `KestrelCommunicationListener` ezt a portot fogja használni.</span><span class="sxs-lookup"><span data-stu-id="d4fba-235">hello URL provided tooyou through `KestrelCommunicationListener` will use this port.</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

<span data-ttu-id="d4fba-236">Ha egy `Endpoint` van konfigurálva, a nevét kell átadását hello `KestrelCommunicationListener` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="d4fba-236">If an `Endpoint` is configured, its name must be passed into hello `KestrelCommunicationListener` constructor:</span></span> 

```csharp
new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) => ...
```

<span data-ttu-id="d4fba-237">Ha egy `Endpoint` nem használja a konfigurációját, hagyja ki ezt a hello neve a hello `KestrelCommunicationListener` konstruktor.</span><span class="sxs-lookup"><span data-stu-id="d4fba-237">If an `Endpoint` configuration is not used, omit hello name in hello `KestrelCommunicationListener` constructor.</span></span> <span data-ttu-id="d4fba-238">Ebben az esetben egy dinamikus portot fogja használni.</span><span class="sxs-lookup"><span data-stu-id="d4fba-238">In this case, a dynamic port will be used.</span></span> <span data-ttu-id="d4fba-239">Hello további információt a következő szakaszban talál.</span><span class="sxs-lookup"><span data-stu-id="d4fba-239">See hello next section for more information.</span></span>

#### <a name="use-kestrel-with-a-dynamic-port"></a><span data-ttu-id="d4fba-240">A dinamikus port vércse használata</span><span class="sxs-lookup"><span data-stu-id="d4fba-240">Use Kestrel with a dynamic port</span></span>
<span data-ttu-id="d4fba-241">Vércse hello hello automatikus port hozzárendelés nem használható `Endpoint` konfigurációját a ServiceManifest.xml, mert automatikus port hozzárendelés egy `Endpoint` konfigurációs hozzárendel egy egyedi port / *folyamaton* , és egyetlen gazdafolyamat vércse több példánya is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="d4fba-241">Kestrel cannot use hello automatic port assignment from hello `Endpoint` configuration in ServiceManifest.xml, because automatic port assignment from an `Endpoint` configuration assigns a unique port per *host process*, and a single host process can contain multiple Kestrel instances.</span></span> <span data-ttu-id="d4fba-242">Vércse nem támogatja a port megosztása, mert az nem működik vércse feltünteti egy egyedi portot kell megnyitni.</span><span class="sxs-lookup"><span data-stu-id="d4fba-242">Since Kestrel does not support port sharing, this does not work as each Kestrel instance must be opened on a unique port.</span></span>

<span data-ttu-id="d4fba-243">vércse, a dinamikus port-hozzárendelés toouse egyszerűen csak hagyja el a hello `Endpoint` ServiceManifest.xml konfiguráció teljesen, és nem továbbítja a végpont neve toohello `KestrelCommunicationListener` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="d4fba-243">toouse dynamic port assignment with Kestrel, simply omit hello `Endpoint` configuration in ServiceManifest.xml entirely, and do not pass an endpoint name toohello `KestrelCommunicationListener` constructor:</span></span>

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

<span data-ttu-id="d4fba-244">Ebben a konfigurációban `KestrelCommunicationListener` automatikusan választja ki egy nem használt portot hello alkalmazás porttartományát.</span><span class="sxs-lookup"><span data-stu-id="d4fba-244">In this configuration, `KestrelCommunicationListener` will automatically select an unused port from hello application port range.</span></span>

## <a name="scenarios-and-configurations"></a><span data-ttu-id="d4fba-245">A forgatókönyvek és konfigurációk</span><span class="sxs-lookup"><span data-stu-id="d4fba-245">Scenarios and configurations</span></span>
<span data-ttu-id="d4fba-246">Ez a szakasz ismerteti a következő forgatókönyvek hello és hello ajánlott webkiszolgáló, a port konfigurálása, a Service Fabric-integrációs beállítások és a különböző beállítások tooachieve egy megfelelően működő szolgáltatást:</span><span class="sxs-lookup"><span data-stu-id="d4fba-246">This section describes hello following scenarios and provides hello recommended combination of web server, port configuration, Service Fabric integration options, and miscellaneous settings tooachieve a properly functioning service:</span></span>
 - <span data-ttu-id="d4fba-247">Külsőleg kitett az ASP.NET Core állapotmentes szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="d4fba-247">Externally exposed ASP.NET Core stateless service</span></span>
 - <span data-ttu-id="d4fba-248">Csak belső ASP.NET Core állapotmentes szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="d4fba-248">Internal-only ASP.NET Core stateless service</span></span>
 - <span data-ttu-id="d4fba-249">Csak belső ASP.NET Core állapotalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="d4fba-249">Internal-only ASP.NET Core stateful service</span></span>

<span data-ttu-id="d4fba-250">Egy **külsőleg kitett** szolgáltatás egyike, amely elérhetővé teszi a hello fürtön, általában egy terhelés-kiegyenlítő kívülről elérhető végpontok.</span><span class="sxs-lookup"><span data-stu-id="d4fba-250">An **externally exposed** service is one that exposes an endpoint reachable from outside hello cluster, usually through a load balancer.</span></span>

<span data-ttu-id="d4fba-251">Egy **csak belső** szolgáltatás egyik amelynek végpont csak elérhető hello fürtön belül.</span><span class="sxs-lookup"><span data-stu-id="d4fba-251">An **internal-only** service is one whose endpoint is only reachable from within hello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="d4fba-252">Az állapotalapú szolgáltatás végpontok általában nem kell toohello Internet kitéve.</span><span class="sxs-lookup"><span data-stu-id="d4fba-252">Stateful service endpoints generally should not be exposed toohello Internet.</span></span> <span data-ttu-id="d4fba-253">Fürtök mögött található, amely nem tudnak a Service Fabric-szolgáltatás feloldási hello Azure terheléselosztó, például a terheléselosztók lesz nem tooexpose állapotalapú szolgáltatások, mert hello terheléselosztó nem tud toolocate lennie, és nem irányíthatja a forgalmat toohello a replika megfelelő állapotalapú szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="d4fba-253">Clusters that are behind load balancers that are unaware of Service Fabric service resolution, such as hello Azure Load Balancer, will be unable tooexpose stateful services because hello load balancer will not be able toolocate and route traffic toohello appropriate stateful service replica.</span></span> 

### <a name="externally-exposed-aspnet-core-stateless-services"></a><span data-ttu-id="d4fba-254">Külsőleg kitett az ASP.NET Core állapotmentes szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="d4fba-254">Externally exposed ASP.NET Core stateless services</span></span>
<span data-ttu-id="d4fba-255">WebListener ajánlott az előtér-szolgáltatásokat, amelyek a Windows külső, az internetre irányuló HTTP-végpontokról láthatóvá webkiszolgálóját hello.</span><span class="sxs-lookup"><span data-stu-id="d4fba-255">WebListener is hello recommended web server for front-end services that expose external, Internet-facing HTTP endpoints on Windows.</span></span> <span data-ttu-id="d4fba-256">Támadásokkal szembeni hatékonyabb védekezés és vércse viszont nem, például a Windows-hitelesítés és a port megosztása funkciókat is támogat.</span><span class="sxs-lookup"><span data-stu-id="d4fba-256">It provides better protection against attacks and supports features that Kestrel does not, such as Windows Authentication and port sharing.</span></span> 

<span data-ttu-id="d4fba-257">Vércse jelenleg nem támogatott (internetről hozzáférhető) edge-kiszolgálóként.</span><span class="sxs-lookup"><span data-stu-id="d4fba-257">Kestrel is not supported as an edge (Internet-facing) server at this time.</span></span> <span data-ttu-id="d4fba-258">Például az IIS vagy Nginx fordított proxykiszolgálón használható toohandle forgalmát hello nyilvános internethez.</span><span class="sxs-lookup"><span data-stu-id="d4fba-258">A reverse proxy server such as IIS or Nginx must be used toohandle traffic from hello public Internet.</span></span>
 
<span data-ttu-id="d4fba-259">Amikor elérhetővé toohello Internet, állapotmentes szolgáltatások egy jól ismert és állandó végponttal, amely elérhető a terheléselosztó kell használnia.</span><span class="sxs-lookup"><span data-stu-id="d4fba-259">When exposed toohello Internet, a stateless service should use a well-known and stable endpoint that is reachable through a load balancer.</span></span> <span data-ttu-id="d4fba-260">Azt adja meg az alkalmazás toousers hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="d4fba-260">This is hello URL you will provide toousers of your application.</span></span> <span data-ttu-id="d4fba-261">a következő konfigurációs hello ajánlott:</span><span class="sxs-lookup"><span data-stu-id="d4fba-261">hello following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="d4fba-262">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="d4fba-262">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d4fba-263">Webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="d4fba-263">Web server</span></span> | <span data-ttu-id="d4fba-264">WebListener</span><span class="sxs-lookup"><span data-stu-id="d4fba-264">WebListener</span></span> | <span data-ttu-id="d4fba-265">Ha hello szolgáltatás csak kitett tooa megbízható hálózathoz, az intraneten vércse is használható.</span><span class="sxs-lookup"><span data-stu-id="d4fba-265">If hello service is only exposed tooa trusted network, such an intranet, Kestrel may be used.</span></span> <span data-ttu-id="d4fba-266">Ellenkező esetben a WebListener hello előnyben részesített lehetőség.</span><span class="sxs-lookup"><span data-stu-id="d4fba-266">Otherwise, WebListener is hello preferred option.</span></span> |
| <span data-ttu-id="d4fba-267">Port konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d4fba-267">Port configuration</span></span> | <span data-ttu-id="d4fba-268">Statikus</span><span class="sxs-lookup"><span data-stu-id="d4fba-268">static</span></span> | <span data-ttu-id="d4fba-269">A hello lehet beállítani a jól ismert statikus port `Endpoints` ServiceManifest.xml, például a 80-as HTTP vagy HTTPS-hez a 443-as konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="d4fba-269">A well-known static port should be configured in hello `Endpoints` configuration of ServiceManifest.xml, such as 80 for HTTP or 443 for HTTPS.</span></span> |
| <span data-ttu-id="d4fba-270">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="d4fba-270">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="d4fba-271">None</span><span class="sxs-lookup"><span data-stu-id="d4fba-271">None</span></span> | <span data-ttu-id="d4fba-272">Hello `ServiceFabricIntegrationOptions.None` beállítást kell használni, amikor a Service Fabric-integráció köztes konfigurálása, hogy hello szolgáltatás nem próbálja meg egy egyedi azonosítót toovalidate bejövő kérelmet.</span><span class="sxs-lookup"><span data-stu-id="d4fba-272">hello `ServiceFabricIntegrationOptions.None` option should be used when configuring Service Fabric integration middleware so that hello service does not attempt toovalidate incoming requests for a unique identifier.</span></span> <span data-ttu-id="d4fba-273">Az alkalmazás külső felhasználók nem fogja tudni hello egyedi azonosító adatokat hello köztes használják.</span><span class="sxs-lookup"><span data-stu-id="d4fba-273">External users of your application will not know hello unique identifying information used by hello middleware.</span></span> |
| <span data-ttu-id="d4fba-274">A példányok száma</span><span class="sxs-lookup"><span data-stu-id="d4fba-274">Instance Count</span></span> | <span data-ttu-id="d4fba-275">-1</span><span class="sxs-lookup"><span data-stu-id="d4fba-275">-1</span></span> | <span data-ttu-id="d4fba-276">A tipikus használati esetek hello beállítása példányszáma beállításaként túl-"1", hogy egy példány érhető el az összes olyan csomóponton, amelyet a forgalom fogadására a terheléselosztótól.</span><span class="sxs-lookup"><span data-stu-id="d4fba-276">In typical use cases, hello instance count setting should be set too"-1" so that an instance is available on all nodes that receive traffic from a load balancer.</span></span> |

<span data-ttu-id="d4fba-277">Ha több kívülről elérhető szolgáltatások ugyanazt a csomópontok beállítása hello megosztásához stabil, de egyedi URL-cím használható.</span><span class="sxs-lookup"><span data-stu-id="d4fba-277">If multiple externally exposed services share hello same set of nodes, a unique but stable URL path should be used.</span></span> <span data-ttu-id="d4fba-278">Ehhez a hello IWebHost konfigurálásakor megadott URL-cím módosításához.</span><span class="sxs-lookup"><span data-stu-id="d4fba-278">This can be accomplished by modifying hello URL provided when configuring IWebHost.</span></span> <span data-ttu-id="d4fba-279">Megjegyzés: Ez csak tooWebListener vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="d4fba-279">Note this applies tooWebListener only.</span></span>

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

### <a name="internal-only-stateless-aspnet-core-service"></a><span data-ttu-id="d4fba-280">Csak belső állapot nélküli ASP.NET Core szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="d4fba-280">Internal-only stateless ASP.NET Core service</span></span>
<span data-ttu-id="d4fba-281">Állapotmentes szolgáltatásokhoz, amelyeket a rendszer csak a hello fürtön belül egyedi URL-eket kell használnia, és dinamikusan kiosztott portok tooensure együttműködés több szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="d4fba-281">Stateless services that are only called from within hello cluster should use unique URLs and dynamically assigned ports tooensure cooperation between multiple services.</span></span> <span data-ttu-id="d4fba-282">a következő konfigurációs hello ajánlott:</span><span class="sxs-lookup"><span data-stu-id="d4fba-282">hello following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="d4fba-283">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="d4fba-283">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d4fba-284">Webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="d4fba-284">Web server</span></span> | <span data-ttu-id="d4fba-285">vércse</span><span class="sxs-lookup"><span data-stu-id="d4fba-285">Kestrel</span></span> | <span data-ttu-id="d4fba-286">Bár WebListener belső állapotmentes szolgáltatások is használhatók, a vércse hello ajánlott server tooallow több szolgáltatás példányok tooshare állomás.</span><span class="sxs-lookup"><span data-stu-id="d4fba-286">Although WebListener may be used for internal stateless services, Kestrel is hello recommended server tooallow multiple service instances tooshare a host.</span></span>  |
| <span data-ttu-id="d4fba-287">Port konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d4fba-287">Port configuration</span></span> | <span data-ttu-id="d4fba-288">dinamikusan kiosztott</span><span class="sxs-lookup"><span data-stu-id="d4fba-288">dynamically assigned</span></span> | <span data-ttu-id="d4fba-289">Az állapotalapú szolgáltatás több replika megoszthatja a gazdafolyamat vagy a gazda operációs rendszer, és így kell egyedi portok.</span><span class="sxs-lookup"><span data-stu-id="d4fba-289">Multiple replicas of a stateful service may share a host process or host operating system and thus will need unique ports.</span></span> |
| <span data-ttu-id="d4fba-290">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="d4fba-290">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="d4fba-291">UseUniqueServiceUrl</span><span class="sxs-lookup"><span data-stu-id="d4fba-291">UseUniqueServiceUrl</span></span> | <span data-ttu-id="d4fba-292">Dinamikus portok hozzárendeléssel Ez a beállítás megakadályozza a korábban leírt összetéveszteni hello identitás probléma.</span><span class="sxs-lookup"><span data-stu-id="d4fba-292">With dynamic port assignment, this setting prevents hello mistaken identity issue described earlier.</span></span> |
| <span data-ttu-id="d4fba-293">InstanceCount</span><span class="sxs-lookup"><span data-stu-id="d4fba-293">InstanceCount</span></span> | <span data-ttu-id="d4fba-294">bármely</span><span class="sxs-lookup"><span data-stu-id="d4fba-294">any</span></span> | <span data-ttu-id="d4fba-295">hello beállítása példányszáma tooany szükséges toooperate hello szolgáltatás állítható be.</span><span class="sxs-lookup"><span data-stu-id="d4fba-295">hello instance count setting can be set tooany value necessary toooperate hello service.</span></span> |

### <a name="internal-only-stateful-aspnet-core-service"></a><span data-ttu-id="d4fba-296">Csak belső állapot-nyilvántartó ASP.NET Core szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="d4fba-296">Internal-only stateful ASP.NET Core service</span></span>
<span data-ttu-id="d4fba-297">Állapot-nyilvántartó szolgáltatásokat, amelyeket a rendszer csak a hello fürtön belül több szolgáltatások együttműködése dinamikusan hozzárendelt portok tooensure kell használnia.</span><span class="sxs-lookup"><span data-stu-id="d4fba-297">Stateful services that are only called from within hello cluster should use dynamically assigned ports tooensure cooperation between multiple services.</span></span> <span data-ttu-id="d4fba-298">a következő konfigurációs hello ajánlott:</span><span class="sxs-lookup"><span data-stu-id="d4fba-298">hello following configuration is recommended:</span></span>

|  |  | <span data-ttu-id="d4fba-299">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="d4fba-299">**Notes**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d4fba-300">Webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="d4fba-300">Web server</span></span> | <span data-ttu-id="d4fba-301">vércse</span><span class="sxs-lookup"><span data-stu-id="d4fba-301">Kestrel</span></span> | <span data-ttu-id="d4fba-302">Hello `WebListenerCommunicationListener` több replikák osztozik egy állapotalapú szolgáltatások használatra nem szolgál.</span><span class="sxs-lookup"><span data-stu-id="d4fba-302">hello `WebListenerCommunicationListener` is not designed for use by stateful services in which replicas share a host process.</span></span> |
| <span data-ttu-id="d4fba-303">Port konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d4fba-303">Port configuration</span></span> | <span data-ttu-id="d4fba-304">dinamikusan kiosztott</span><span class="sxs-lookup"><span data-stu-id="d4fba-304">dynamically assigned</span></span> | <span data-ttu-id="d4fba-305">Az állapotalapú szolgáltatás több replika megoszthatja a gazdafolyamat vagy a gazda operációs rendszer, és így kell egyedi portok.</span><span class="sxs-lookup"><span data-stu-id="d4fba-305">Multiple replicas of a stateful service may share a host process or host operating system and thus will need unique ports.</span></span> |
| <span data-ttu-id="d4fba-306">ServiceFabricIntegrationOptions</span><span class="sxs-lookup"><span data-stu-id="d4fba-306">ServiceFabricIntegrationOptions</span></span> | <span data-ttu-id="d4fba-307">UseUniqueServiceUrl</span><span class="sxs-lookup"><span data-stu-id="d4fba-307">UseUniqueServiceUrl</span></span> | <span data-ttu-id="d4fba-308">Dinamikus portok hozzárendeléssel Ez a beállítás megakadályozza a korábban leírt összetéveszteni hello identitás probléma.</span><span class="sxs-lookup"><span data-stu-id="d4fba-308">With dynamic port assignment, this setting prevents hello mistaken identity issue described earlier.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d4fba-309">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d4fba-309">Next steps</span></span>
[<span data-ttu-id="d4fba-310">A Service Fabric-alkalmazás hibakeresése a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="d4fba-310">Debug your Service Fabric application by using Visual Studio</span></span>](service-fabric-debugging-your-application.md)

<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
