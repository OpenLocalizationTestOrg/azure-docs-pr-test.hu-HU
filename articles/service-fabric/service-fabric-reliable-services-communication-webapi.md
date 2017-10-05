---
title: "Az ASP.NET Web API kommunikáció szolgáltatás |} Microsoft Docs"
description: "Az ASP.NET Web API OWIN önálló üzemeltető a megbízható szolgáltatások API használatával valósítja meg a szolgáltatások közötti kommunikáció részletes tudnivalók."
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
ms.date: 02/10/2017
ms.author: vturecek
redirect_url: /azure/service-fabric/service-fabric-reliable-services-communication-aspnetcore
ms.openlocfilehash: 73b7e1c0cb93ae7c54780a3aab837b0e5bcdb0a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a><span data-ttu-id="ce025-103">Első lépések: az OWIN futtató önálló Service Fabric Web API szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="ce025-103">Get started: Service Fabric Web API services with OWIN self-hosting</span></span>
<span data-ttu-id="ce025-104">Az Azure Service Fabric helyezi a power a beavatkozás nélküli, amikor eldönti, hogyan szeretné a szolgáltatásokat, hogy és egymással kommunikáló felhasználók.</span><span class="sxs-lookup"><span data-stu-id="ce025-104">Azure Service Fabric puts the power in your hands when you're deciding how you want your services to communicate with users and with each other.</span></span> <span data-ttu-id="ce025-105">Ez az oktatóanyag végzett szolgáltatások közötti kommunikáció ASP.NET Web API-t használja az Open Web Interface .NET (OWIN) önálló tárolásához a Service Fabric megbízható szolgáltatások API megvalósítására koncentrál.</span><span class="sxs-lookup"><span data-stu-id="ce025-105">This tutorial focuses on implementing service communication using ASP.NET Web API with Open Web Interface for .NET (OWIN) self-hosting in Service Fabric's Reliable Services API.</span></span> <span data-ttu-id="ce025-106">Azt fogja elmélyedhet mélyen a Reliable Services moduláris kommunikációs API.</span><span class="sxs-lookup"><span data-stu-id="ce025-106">We'll delve deeply into the Reliable Services pluggable communication API.</span></span> <span data-ttu-id="ce025-107">Használjuk Web API is részletes példa mutatjuk be, hogyan állíthat be egy egyéni kommunikációs figyelő.</span><span class="sxs-lookup"><span data-stu-id="ce025-107">We'll also use Web API in a step-by-step example to show you how to set up a custom communication listener.</span></span>

## <a name="introduction-to-web-api-in-service-fabric"></a><span data-ttu-id="ce025-108">Bevezetés a Service Fabric a webes API-hoz</span><span class="sxs-lookup"><span data-stu-id="ce025-108">Introduction to Web API in Service Fabric</span></span>
<span data-ttu-id="ce025-109">ASP.NET webes API-t egy népszerű és erőteljes környezet HTTP API-k létrehozásához a .NET-keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="ce025-109">ASP.NET Web API is a popular and powerful framework for building HTTP APIs on top of the .NET Framework.</span></span> <span data-ttu-id="ce025-110">Ha még nem már ismeri a keretében, lásd: [Ismerkedés az ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) további.</span><span class="sxs-lookup"><span data-stu-id="ce025-110">If you're not already familiar with the framework, see [Getting started with ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) to learn more.</span></span>

<span data-ttu-id="ce025-111">Webes API-t a Service Fabric az azonos ASP.NET Web API megszokott és kedvelt.</span><span class="sxs-lookup"><span data-stu-id="ce025-111">Web API in Service Fabric is the same ASP.NET Web API you know and love.</span></span> <span data-ttu-id="ce025-112">A különbség a hogyan meg *állomás* egy webes API-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ce025-112">The difference is in how you *host* a Web API application.</span></span> <span data-ttu-id="ce025-113">Nem fogja használni a Microsoft Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="ce025-113">You won't be using Microsoft Internet Information Services (IIS).</span></span> <span data-ttu-id="ce025-114">Jobb megértése érdekében a különbség, most felosztása, két részből áll:</span><span class="sxs-lookup"><span data-stu-id="ce025-114">To better understand the difference, let's break it into two parts:</span></span>

1. <span data-ttu-id="ce025-115">A webes API-alkalmazás (beleértve a tartományvezérlőket és -modellek)</span><span class="sxs-lookup"><span data-stu-id="ce025-115">The Web API application (including controllers and models)</span></span>
2. <span data-ttu-id="ce025-116">A gazdagép (a webkiszolgáló, általában az IIS)</span><span class="sxs-lookup"><span data-stu-id="ce025-116">The host (the web server, usually IIS)</span></span>

<span data-ttu-id="ce025-117">Egy magát webes API-alkalmazás nem változik.</span><span class="sxs-lookup"><span data-stu-id="ce025-117">A Web API application itself doesn't change.</span></span> <span data-ttu-id="ce025-118">Helyzetet teremt, mintha a webes API-alkalmazások írt kap a múltban, és meg kell az alkalmazás kódjában többsége egyszerűen átvitele.</span><span class="sxs-lookup"><span data-stu-id="ce025-118">It's no different from Web API applications you may have written in the past, and you should be able to simply move over most of your application code.</span></span> <span data-ttu-id="ce025-119">Azonban ha azt korábban futtató IIS-kiszolgálón, ahol az alkalmazás állomás kissé eltér az éppen használt lehet.</span><span class="sxs-lookup"><span data-stu-id="ce025-119">But if you've been hosting on IIS, where you host the application may be a little different from what you're used to.</span></span> <span data-ttu-id="ce025-120">Után azt beolvasni a birtokosi részére, kezdjük valami további ismerős: a webes API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ce025-120">Before we get to the hosting part, let's start with something more familiar: the Web API application.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="ce025-121">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce025-121">Create the application</span></span>
<span data-ttu-id="ce025-122">Először hozzon létre egy új Service Fabric-alkalmazás egy állapot nélküli szolgáltatásnak a Visual Studio 2015-öt.</span><span class="sxs-lookup"><span data-stu-id="ce025-122">Start by creating a new Service Fabric application with a single stateless service in Visual Studio 2015.</span></span>

<span data-ttu-id="ce025-123">A webes API-jával állapotmentes szolgáltatások Visual Studio sablonját is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="ce025-123">A Visual Studio template for a stateless service using Web API is available to you.</span></span> <span data-ttu-id="ce025-124">Ebben az oktatóanyagban azt, hogy milyen visszajelzést kap, ha a kiválasztott sablon eredménye teljesen új webes API a projekt fogja létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ce025-124">In this tutorial, we'll build a Web API project from scratch that results in what you'd get if you selected this template.</span></span>

<span data-ttu-id="ce025-125">Válasszon ki egy üres állapotmentes szolgáltatások projektet megtudhatja, hogyan hozhat létre egy teljesen új webes API-projektet, vagy indítsa el az állapotmentes szolgáltatások webes API-sablonhoz, és egyszerűen követéséhez.</span><span class="sxs-lookup"><span data-stu-id="ce025-125">Select a blank Stateless Service project to learn how to build a Web API project from scratch, or you can start with the stateless service Web API template and simply follow along.</span></span>  

<span data-ttu-id="ce025-126">Az első lépés néhány NuGet-csomagok a webes API le tudja.</span><span class="sxs-lookup"><span data-stu-id="ce025-126">The first step is to pull in some NuGet packages for Web API.</span></span> <span data-ttu-id="ce025-127">A csomag használandó szeretnénk Microsoft.AspNet.WebApi.OwinSelfHost.</span><span class="sxs-lookup"><span data-stu-id="ce025-127">The package we want to use is Microsoft.AspNet.WebApi.OwinSelfHost.</span></span> <span data-ttu-id="ce025-128">Ez a csomag tartalmazza a szükséges webes API-csomagok és a *állomás* csomagok.</span><span class="sxs-lookup"><span data-stu-id="ce025-128">This package includes all the necessary Web API packages and the *host* packages.</span></span> <span data-ttu-id="ce025-129">Ez lesz fontos később.</span><span class="sxs-lookup"><span data-stu-id="ce025-129">This will be important later.</span></span>

<span data-ttu-id="ce025-130">A csomagok telepítése után megkezdheti a webes API-projekt alapszintű struktúrát létrehozására.</span><span class="sxs-lookup"><span data-stu-id="ce025-130">After the packages have been installed, you can begin building out the basic Web API project structure.</span></span> <span data-ttu-id="ce025-131">Ha már használta a webes API-t, a projekt struktúra tisztában kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="ce025-131">If you've used Web API, the project structure should look very familiar.</span></span> <span data-ttu-id="ce025-132">Először vegyen fel egy `Controllers` könyvtár és egy egyszerű érték tartományvezérlő:</span><span class="sxs-lookup"><span data-stu-id="ce025-132">Start by adding a `Controllers` directory and a simple values controller:</span></span>

<span data-ttu-id="ce025-133">**ValuesController.cs**</span><span class="sxs-lookup"><span data-stu-id="ce025-133">**ValuesController.cs**</span></span>

```csharp
using System.Collections.Generic;
using System.Web.Http;

namespace WebService.Controllers
{
    public class ValuesController : ApiController
    {
        // GET api/values 
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5 
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values 
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5 
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5 
        public void Delete(int id)
        {
        }
    }
}

```

<span data-ttu-id="ce025-134">A következő indítási osztály hozzáadása a projekt legfelső szintű regisztrálni az útvonal is és bármely egyéb konfigurációs beállítása.</span><span class="sxs-lookup"><span data-stu-id="ce025-134">Next, add a Startup class at the project root to register the routing, formatters, and any other configuration setup.</span></span> <span data-ttu-id="ce025-135">Ez egyben ahol Web API csatlakoztatja az való a *állomás*, amely fog kell javított változat újra később.</span><span class="sxs-lookup"><span data-stu-id="ce025-135">This is also where Web API plugs in to the *host*, which will be revisited again later.</span></span> 

<span data-ttu-id="ce025-136">**Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="ce025-136">**Startup.cs**</span></span>

```csharp
using System.Web.Http;
using Owin;

namespace WebService
{
    public static class Startup
    {
        public static void ConfigureApp(IAppBuilder appBuilder)
        {
            // Configure Web API for self-host. 
            HttpConfiguration config = new HttpConfiguration();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            appBuilder.UseWebApi(config);
        }
    }
}
```

<span data-ttu-id="ce025-137">Ez az alkalmazás részére.</span><span class="sxs-lookup"><span data-stu-id="ce025-137">That's it for the application part.</span></span> <span data-ttu-id="ce025-138">Ezen a ponton létrehoztunk hogy csak az alapszintű webes API projektet elrendezés.</span><span class="sxs-lookup"><span data-stu-id="ce025-138">At this point, we've set up just the basic Web API project layout.</span></span> <span data-ttu-id="ce025-139">Eddig akkor nem jelenik meg sokkal írt kap a múltban Web API-projektet, illetve az egyszerű webes API-sablon</span><span class="sxs-lookup"><span data-stu-id="ce025-139">So far, it shouldn't look much different from Web API projects you may have written in the past or from the basic Web API template.</span></span> <span data-ttu-id="ce025-140">Az üzleti logikát a megszokott módon végrehajtja a tartományvezérlők és modellek.</span><span class="sxs-lookup"><span data-stu-id="ce025-140">Your business logic goes in the controllers and models as usual.</span></span>

<span data-ttu-id="ce025-141">Most mi a teendő-k üzemeltetésére szolgál, hogy azt ténylegesen futtatható?</span><span class="sxs-lookup"><span data-stu-id="ce025-141">Now what do we do about hosting so that we can actually run it?</span></span>

## <a name="service-hosting"></a><span data-ttu-id="ce025-142">Üzemeltető szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="ce025-142">Service hosting</span></span>
<span data-ttu-id="ce025-143">A Service Fabric, a szolgáltatás fut a *gazdafolyamat szolgáltatás*, egy végrehajtható fájl, amely a szolgáltatáskód hibáit futtatja.</span><span class="sxs-lookup"><span data-stu-id="ce025-143">In Service Fabric, your service runs in a *service host process*, an executable file that runs your service code.</span></span> <span data-ttu-id="ce025-144">A megbízható szolgáltatások API-jával írásakor szolgáltatás szolgáltatásprojektre csak egy végrehajtható fájl, amely a szolgáltatás típusának regisztrálja, és a kód futtatása lefordítja.</span><span class="sxs-lookup"><span data-stu-id="ce025-144">When you write a service by using the Reliable Services API, your service project just compiles to an executable file that registers your service type and runs your code.</span></span> <span data-ttu-id="ce025-145">Ez igaz a legtöbb esetben a szolgáltatás a Service Fabric a .NET írásakor.</span><span class="sxs-lookup"><span data-stu-id="ce025-145">This is true in most cases when you write a service on Service Fabric in .NET.</span></span> <span data-ttu-id="ce025-146">Az állapotmentes szolgáltatások projekt Program.cs megnyitásakor kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="ce025-146">When you open Program.cs in the stateless service project, you should see:</span></span>

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric.Services.Runtime;

internal static class Program
{
    private static void Main()
    {
        try
        {
            ServiceRuntime.RegisterServiceAsync("WebServiceType",
                context => new WebService(context)).GetAwaiter().GetResult();

            ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(WebService).Name);

            // Prevents this host process from terminating so services keeps running. 
            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

<span data-ttu-id="ce025-147">Ha, amely a következőképpen néz gyanúsan a belépési pont számára egy konzolalkalmazást, mert van.</span><span class="sxs-lookup"><span data-stu-id="ce025-147">If that looks suspiciously like the entry point to a console application, that's because it is.</span></span>

<span data-ttu-id="ce025-148">A szolgáltatás gazdafolyamat és a szolgáltatás regisztrációs kapcsolatos részletes adatok nem térnek Ez a cikk.</span><span class="sxs-lookup"><span data-stu-id="ce025-148">Further details about the service host process and service registration are beyond the scope of this article.</span></span> <span data-ttu-id="ce025-149">De fontos tudni, hogy a most, hogy *a szolgáltatáskód hibáit külön folyamatban fut.*.</span><span class="sxs-lookup"><span data-stu-id="ce025-149">But it's important to know for now that *your service code is running in its own process*.</span></span>

## <a name="self-host-web-api-with-an-owin-host"></a><span data-ttu-id="ce025-150">Önálló gazdagép Web API egy OWIN állomás</span><span class="sxs-lookup"><span data-stu-id="ce025-150">Self-host Web API with an OWIN host</span></span>
<span data-ttu-id="ce025-151">Figyelembe véve, hogy a webes API-alkalmazás kódja egy külön folyamatban, hogyan tegye meg kapcsolja legfeljebb egy webkiszolgáló?</span><span class="sxs-lookup"><span data-stu-id="ce025-151">Given that your Web API application code is hosted in its own process, how do you hook it up to a web server?</span></span> <span data-ttu-id="ce025-152">Adja meg [OWIN](http://owin.org/).</span><span class="sxs-lookup"><span data-stu-id="ce025-152">Enter [OWIN](http://owin.org/).</span></span> <span data-ttu-id="ce025-153">OWIN: egyszerűen egy szerződés .NET webes alkalmazások és a webkiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="ce025-153">OWIN is simply a contract between .NET web applications and web servers.</span></span> <span data-ttu-id="ce025-154">Hagyományosan az ASP.NET (max. MVC 5) használata esetén a webes alkalmazás szorosan csatlakoztatott IIS System.Web keresztül.</span><span class="sxs-lookup"><span data-stu-id="ce025-154">Traditionally when ASP.NET (up to MVC 5) is used, the web application is tightly coupled to IIS through System.Web.</span></span> <span data-ttu-id="ce025-155">Azonban a Web API OWIN, megvalósítja, az azt üzemeltető webkiszolgáló írhat egy webes alkalmazás, amely le van.</span><span class="sxs-lookup"><span data-stu-id="ce025-155">However, Web API implements OWIN, so you can write a web application that is decoupled from the web server that hosts it.</span></span> <span data-ttu-id="ce025-156">Ebből kifolyólag használhatja egy *önállóan üzemel* OWIN webkiszolgálón, amelyek a saját folyamat megkezdése.</span><span class="sxs-lookup"><span data-stu-id="ce025-156">Because of this, you can use a *self-hosted* OWIN web server that you can start in your own process.</span></span> <span data-ttu-id="ce025-157">Ez tökéletesen csak ismerteti a Service Fabric üzemeltetési modell illeszkedik.</span><span class="sxs-lookup"><span data-stu-id="ce025-157">This fits perfectly with the Service Fabric hosting model we just described.</span></span>

<span data-ttu-id="ce025-158">Ebben a cikkben Katana használjuk az OWIN gazdagépként a webes API-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="ce025-158">In this article, we'll use Katana as the OWIN host for the Web API application.</span></span> <span data-ttu-id="ce025-159">Katana egy nyílt forráskódú OWIN állomás implementáció épülő [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) és a Windows [HTTP-kiszolgáló API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="ce025-159">Katana is an open-source OWIN host implementation built on [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) and the Windows [HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="ce025-160">Katana kapcsolatos további tudnivalókért keresse fel a [Katana hely](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana).</span><span class="sxs-lookup"><span data-stu-id="ce025-160">To learn more about Katana, go to the [Katana site](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana).</span></span> <span data-ttu-id="ce025-161">Önálló gazdagép Web API Katana használatával történő gyors áttekintést, lásd: [használja OWIN Self-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).</span><span class="sxs-lookup"><span data-stu-id="ce025-161">For a quick overview of how to use Katana to self-host Web API, see [Use OWIN to Self-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).</span></span>
> 
> 

## <a name="set-up-the-web-server"></a><span data-ttu-id="ce025-162">A webalkalmazás-kiszolgáló beállítása</span><span class="sxs-lookup"><span data-stu-id="ce025-162">Set up the web server</span></span>
<span data-ttu-id="ce025-163">A megbízható szolgáltatások API biztosít egy kommunikációs belépési pontot, amelyen csatlakoztathatja a kommunikációs verem, amelyek lehetővé teszik a felhasználók és az ügyfelek kapcsolódni a szolgáltatáshoz:</span><span class="sxs-lookup"><span data-stu-id="ce025-163">The Reliable Services API provides a communication entry point where you can plug in communication stacks that allow users and clients to connect to the service:</span></span>

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

<span data-ttu-id="ce025-164">A webalkalmazás-kiszolgáló (és más kommunikációs verem például websocket elemek használata a jövőben,) kell ICommunicationListener felületet használ megfelelően integrálása a rendszer.</span><span class="sxs-lookup"><span data-stu-id="ce025-164">The web server (and any other communication stack you use in the future, such as WebSockets) should use the ICommunicationListener interface to integrate correctly with the system.</span></span> <span data-ttu-id="ce025-165">Ennek az oka a következő lépésekben több nyilvánvaló lesz.</span><span class="sxs-lookup"><span data-stu-id="ce025-165">The reasons for this will become more apparent in the following steps.</span></span>

<span data-ttu-id="ce025-166">Először hozzon létre egy osztályt, amely megvalósítja az ICommunicationListener OwinCommunicationListener:</span><span class="sxs-lookup"><span data-stu-id="ce025-166">First, create a class called OwinCommunicationListener that implements ICommunicationListener:</span></span>

<span data-ttu-id="ce025-167">**OwinCommunicationListener.cs**</span><span class="sxs-lookup"><span data-stu-id="ce025-167">**OwinCommunicationListener.cs**</span></span>

```csharp
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;
using System;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}
```

<span data-ttu-id="ce025-168">A ICommunicationListener felület kezelheti a szolgáltatás kommunikációs figyelő három módszert kínál:</span><span class="sxs-lookup"><span data-stu-id="ce025-168">The ICommunicationListener interface provides three methods to manage a communication listener for your service:</span></span>

* <span data-ttu-id="ce025-169">*OpenAsync*.</span><span class="sxs-lookup"><span data-stu-id="ce025-169">*OpenAsync*.</span></span> <span data-ttu-id="ce025-170">Indítsa el a kérések figyelését.</span><span class="sxs-lookup"><span data-stu-id="ce025-170">Start listening for requests.</span></span>
* <span data-ttu-id="ce025-171">*CloseAsync*.</span><span class="sxs-lookup"><span data-stu-id="ce025-171">*CloseAsync*.</span></span> <span data-ttu-id="ce025-172">Állítsa le a kérések figyelését, Befejezés bármilyen az üzenetsoroktól kérelmeket és leállítása.</span><span class="sxs-lookup"><span data-stu-id="ce025-172">Stop listening for requests, finish any in-flight requests, and shut down gracefully.</span></span>
* <span data-ttu-id="ce025-173">*Megszakítás*.</span><span class="sxs-lookup"><span data-stu-id="ce025-173">*Abort*.</span></span> <span data-ttu-id="ce025-174">Minden megszakítja, és azonnal leállítja.</span><span class="sxs-lookup"><span data-stu-id="ce025-174">Cancel everything and stop immediately.</span></span>

<span data-ttu-id="ce025-175">Első lépésként adja hozzá a titkos osztálytagjaihoz dolgot a figyelő működéséhez kell.</span><span class="sxs-lookup"><span data-stu-id="ce025-175">To get started, add private class members for things the listener will need to function.</span></span> <span data-ttu-id="ce025-176">Ezek és felhasználandó inicializálja a konstruktor, valamint később a figyelő URL-cím beállításakor.</span><span class="sxs-lookup"><span data-stu-id="ce025-176">These will be initialized through the constructor and used later when you set up the listening URL.</span></span>

```csharp
internal class OwinCommunicationListener : ICommunicationListener
{
    private readonly ServiceEventSource eventSource;
    private readonly Action<IAppBuilder> startup;
    private readonly ServiceContext serviceContext;
    private readonly string endpointName;
    private readonly string appRoot;

    private IDisposable webApp;
    private string publishAddress;
    private string listeningAddress;

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
        : this(startup, serviceContext, eventSource, endpointName, null)
    {
    }

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
    {
        if (startup == null)
        {
            throw new ArgumentNullException(nameof(startup));
        }

        if (serviceContext == null)
        {
            throw new ArgumentNullException(nameof(serviceContext));
        }

        if (endpointName == null)
        {
            throw new ArgumentNullException(nameof(endpointName));
        }

        if (eventSource == null)
        {
            throw new ArgumentNullException(nameof(eventSource));
        }

        this.startup = startup;
        this.serviceContext = serviceContext;
        this.endpointName = endpointName;
        this.eventSource = eventSource;
        this.appRoot = appRoot;
    }


    ...

```

## <a name="implement-openasync"></a><span data-ttu-id="ce025-177">OpenAsync megvalósítása</span><span class="sxs-lookup"><span data-stu-id="ce025-177">Implement OpenAsync</span></span>
<span data-ttu-id="ce025-178">A webkiszolgáló beállításához két adatra van szükség:</span><span class="sxs-lookup"><span data-stu-id="ce025-178">To set up the web server, you need two pieces of information:</span></span>

* <span data-ttu-id="ce025-179">*Egy URL-cím elérési út előtag*.</span><span class="sxs-lookup"><span data-stu-id="ce025-179">*A URL path prefix*.</span></span> <span data-ttu-id="ce025-180">Bár ez nem kötelező, ahhoz, hogy beállítaniuk ezt most, hogy biztonságosan jó gazdagép több webszolgáltatásból az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ce025-180">Although this is optional, it's good for you to set this up now so that you can safely host multiple web services in your application.</span></span>
* <span data-ttu-id="ce025-181">*A port*.</span><span class="sxs-lookup"><span data-stu-id="ce025-181">*A port*.</span></span>

<span data-ttu-id="ce025-182">Mielőtt port lekérése a webkiszolgálón, fontos, hogy megértette, hogy a Service Fabric, amely az alkalmazás és az általa futtatott operációs rendszer közötti pufferként alkalmazás réteget biztosít.</span><span class="sxs-lookup"><span data-stu-id="ce025-182">Before you get a port for the web server, it's important that you understand that Service Fabric provides an application layer that acts as a buffer between your application and the underlying operating system that it runs on.</span></span> <span data-ttu-id="ce025-183">Konfigurálása módszert kínál, a Service Fabric *végpontok* a szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="ce025-183">As such, Service Fabric provides a way to configure *endpoints* for your services.</span></span> <span data-ttu-id="ce025-184">A Service Fabric biztosítja, hogy végpontok érhető el a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ce025-184">Service Fabric ensures that endpoints are available for your service to use.</span></span> <span data-ttu-id="ce025-185">Ezzel a módszerrel nem kell konfigurálásuk saját magának az alapul szolgáló operációs rendszer környezetében.</span><span class="sxs-lookup"><span data-stu-id="ce025-185">This way, you don't have to configure them yourself in the underlying OS environment.</span></span> <span data-ttu-id="ce025-186">A Service Fabric-alkalmazás különböző környezetekben könnyen tárolhatja, anélkül, hogy ne módosítsa az alkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="ce025-186">You can easily host your Service Fabric application in different environments without having to make any changes to your application.</span></span> <span data-ttu-id="ce025-187">(Például tárolhatja, ugyanazt az alkalmazást az Azure-ban vagy a saját adatközpont.)</span><span class="sxs-lookup"><span data-stu-id="ce025-187">(For example, you can host the same application in Azure or in your own datacenter.)</span></span>

<span data-ttu-id="ce025-188">HTTP-végponttal PackageRoot\ServiceManifest.xml konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="ce025-188">Configure an HTTP endpoint in PackageRoot\ServiceManifest.xml:</span></span>

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

<span data-ttu-id="ce025-189">Ez a lépés fontos, mert a gazdagép-folyamat futtatja korlátozott credentials (a Windows hálózati szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="ce025-189">This step is important because the service host process runs under restricted credentials (Network Service on Windows).</span></span> <span data-ttu-id="ce025-190">Ez azt jelenti, hogy a szolgáltatás nem állíthat be egy HTTP-végpont a saját hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="ce025-190">This means that your service won't have access to set up an HTTP endpoint on its own.</span></span> <span data-ttu-id="ce025-191">A végpont-konfiguráció használatával a Service Fabric tudni fogja, hogy a megfelelő hozzáférés-vezérlési lista (ACL) beállítása az URL-címhez, amely a szolgáltatás figyelni fogja.</span><span class="sxs-lookup"><span data-stu-id="ce025-191">By using the endpoint configuration, Service Fabric knows to set up the proper access control list (ACL) for the URL that the service will listen on.</span></span> <span data-ttu-id="ce025-192">A Service Fabric is itt szabványos végpontok beállítása.</span><span class="sxs-lookup"><span data-stu-id="ce025-192">Service Fabric also provides a standard place to configure endpoints.</span></span>

<span data-ttu-id="ce025-193">Vissza OwinCommunicationListener.cs, akkor kezdhet OpenAsync megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="ce025-193">Back in OwinCommunicationListener.cs, you can start implementing OpenAsync.</span></span> <span data-ttu-id="ce025-194">Ez az az először a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="ce025-194">This is where you start the web server.</span></span> <span data-ttu-id="ce025-195">Először a végpont-információkat lekérni, és az URL-címet, a szolgáltatás figyelni fogja létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ce025-195">First, get the endpoint information and create the URL that the service will listen on.</span></span> <span data-ttu-id="ce025-196">Az URL-cím attól függően, hogy a figyelő használatban van egy állapotmentes szolgáltatások vagy egy állapotalapú szolgáltatás eltérőek lesznek.</span><span class="sxs-lookup"><span data-stu-id="ce025-196">The URL will be different depending on whether the listener is used in a stateless service or a stateful service.</span></span> <span data-ttu-id="ce025-197">A figyelő egy állapotalapú szolgáltatás kell minden állapotalapú szolgáltatási replika elkezdi figyelni a egyedi cím létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ce025-197">For a stateful service, the listener needs to create a unique address for every stateful service replica it listens on.</span></span> <span data-ttu-id="ce025-198">Az állapotmentes szolgáltatások a cím jóval egyszerűbb lehet.</span><span class="sxs-lookup"><span data-stu-id="ce025-198">For stateless services, the address can be much simpler.</span></span> 

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
    var protocol = serviceEndpoint.Protocol;
    int port = serviceEndpoint.Port;

    if (this.serviceContext is StatefulServiceContext)
    {
        StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}{3}/{4}/{5}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/',
            statefulServiceContext.PartitionId,
            statefulServiceContext.ReplicaId,
            Guid.NewGuid());
    }
    else if (this.serviceContext is StatelessServiceContext)
    {
        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/');
    }
    else
    {
        throw new InvalidOperationException();
    }

    ...

```

<span data-ttu-id="ce025-199">Vegye figyelembe, hogy a "http://+" Itt nem használja.</span><span class="sxs-lookup"><span data-stu-id="ce025-199">Note that "http://+" is used here.</span></span> <span data-ttu-id="ce025-200">Ez a, győződjön meg arról, hogy a webkiszolgáló összes elérhető címek, beleértve a localhost, teljes tartománynév és a gép IP-figyeli.</span><span class="sxs-lookup"><span data-stu-id="ce025-200">This is to make sure that the web server is listening on all available addresses, including localhost, FQDN, and the machine IP.</span></span>

<span data-ttu-id="ce025-201">OpenAsync végrehajtására a legfontosabb okai miért a webalkalmazás-kiszolgáló (vagy bármely kommunikációs verem) megvalósítása egy ICommunicationListener, hanem csak azt közvetlenül a nyílt `RunAsync()` a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="ce025-201">The OpenAsync implementation is one of the most important reasons why the web server (or any communication stack) is implemented as an ICommunicationListener, rather than just having it open directly from `RunAsync()` in the service.</span></span> <span data-ttu-id="ce025-202">A visszatérési érték a OpenAsync a címet, hogy a webkiszolgáló nem.</span><span class="sxs-lookup"><span data-stu-id="ce025-202">The return value from OpenAsync is the address that the web server is listening on.</span></span> <span data-ttu-id="ce025-203">Ezt a címet a rendszer küld vissza, ha regisztrálja azokat a cím a szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="ce025-203">When this address is returned to the system, it registers the address with the service.</span></span> <span data-ttu-id="ce025-204">A Service Fabric biztosít az API-k, amely lehetővé teszi az ügyfelek és egyéb szolgáltatások majd feltenni ehhez a címhez szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="ce025-204">Service Fabric provides an API that allows clients and other services to then ask for this address by service name.</span></span> <span data-ttu-id="ce025-205">Ez azért fontos, mert a szolgáltatás-cím nem statikus.</span><span class="sxs-lookup"><span data-stu-id="ce025-205">This is important because the service address is not static.</span></span> <span data-ttu-id="ce025-206">Szolgáltatások helyezi át a fürt erőforrás és a rendelkezésre állás céljából.</span><span class="sxs-lookup"><span data-stu-id="ce025-206">Services are moved around in the cluster for resource balancing and availability purposes.</span></span> <span data-ttu-id="ce025-207">Azt a mechanizmust, amely lehetővé teszi az ügyfelek fel tudják oldani a figyelő cím egy szolgáltatás számára.</span><span class="sxs-lookup"><span data-stu-id="ce025-207">This is the mechanism that allows clients to resolve the listening address for a service.</span></span>

<span data-ttu-id="ce025-208">Vele szem előtt a OpenAsync a webkiszolgáló elindul, és visszahelyezi a címet, amely a figyeléshez.</span><span class="sxs-lookup"><span data-stu-id="ce025-208">With that in mind, OpenAsync starts the web server and returns the address that it's listening on.</span></span> <span data-ttu-id="ce025-209">Vegye figyelembe, hogy elkezdi figyelni a "http://+", de még mielőtt OpenAsync visszahelyezi a címet, a "+" cseréli az IP- vagy a jelenleg a csomópont teljesen minősített Tartománynevét.</span><span class="sxs-lookup"><span data-stu-id="ce025-209">Note that it listens on "http://+", but before OpenAsync returns the address, the "+" is replaced with the IP or FQDN of the node it is currently on.</span></span> <span data-ttu-id="ce025-210">Ez a metódus által visszaadott cím esetén milyen regisztrálva van a rendszer.</span><span class="sxs-lookup"><span data-stu-id="ce025-210">The address that is returned by this method is what's registered with the system.</span></span> <span data-ttu-id="ce025-211">Akkor is ügyfelek és egyéb szolgáltatások látják, ha a szolgáltatás címét, hogy kérni.</span><span class="sxs-lookup"><span data-stu-id="ce025-211">It's also what clients and other services see when they ask for a service's address.</span></span> <span data-ttu-id="ce025-212">Az ügyfelek számára, hogy megfelelően csatlakozik azok kell egy tényleges IP vagy FQDN Formátumban a címben.</span><span class="sxs-lookup"><span data-stu-id="ce025-212">For clients to correctly connect to it, they need an actual IP or FQDN in the address.</span></span>

```csharp
    ...

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    try
    {
        this.eventSource.Message("Starting web server on " + this.listeningAddress);

        this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

        this.eventSource.Message("Listening on " + this.publishAddress);

        return Task.FromResult(this.publishAddress);
    }
    catch (Exception ex)
    {
        this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

<span data-ttu-id="ce025-213">Vegye figyelembe, hogy ez a konstruktor OwinCommunicationListener továbbított indítási osztályra hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="ce025-213">Note that this references the Startup class that was passed in to the OwinCommunicationListener in the constructor.</span></span> <span data-ttu-id="ce025-214">Ez a példány indítási szolgál a webkiszolgáló bootstrap a webes API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ce025-214">This startup instance is used by the web server to bootstrap the Web API application.</span></span>

<span data-ttu-id="ce025-215">A `ServiceEventSource.Current.Message()` sor megjelennek a diagnosztikai események ablak később, győződjön meg arról, hogy a webalkalmazás-kiszolgáló sikeresen elindult-e az alkalmazás futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="ce025-215">The `ServiceEventSource.Current.Message()` line will appear in the Diagnostic Events window later, when you run the application to confirm that the web server has started successfully.</span></span>

## <a name="implement-closeasync-and-abort"></a><span data-ttu-id="ce025-216">Alkalmazzon CloseAsync és Abort</span><span class="sxs-lookup"><span data-stu-id="ce025-216">Implement CloseAsync and Abort</span></span>
<span data-ttu-id="ce025-217">Végezetül megvalósítása CloseAsync és a megszakítási leállítja a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="ce025-217">Finally, implement both CloseAsync and Abort to stop the web server.</span></span> <span data-ttu-id="ce025-218">A webkiszolgáló a kiszolgáló leíró OpenAsync során létrehozott ártalmatlanítása által állítható le.</span><span class="sxs-lookup"><span data-stu-id="ce025-218">The web server can be stopped by disposing the server handle that was created during OpenAsync.</span></span>

```csharp
public Task CloseAsync(CancellationToken cancellationToken)
{
    this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.webApp != null)
    {
        try
        {
            this.webApp.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}
```

<span data-ttu-id="ce025-219">A megvalósítás példában CloseAsync és a megszakítási egyszerűen állítsa le a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="ce025-219">In this implementation example, both CloseAsync and Abort simply stop the web server.</span></span> <span data-ttu-id="ce025-220">Úgy is dönthet, hajtsa végre a webkiszolgáló több szabályosan koordinált leállítási CloseAsync.</span><span class="sxs-lookup"><span data-stu-id="ce025-220">You may opt to perform a more gracefully coordinated shutdown of the web server in CloseAsync.</span></span> <span data-ttu-id="ce025-221">Például a leállítási sikerült Várjon, amíg az üzenetsoroktól kérelem végrehajtása előtt a visszatérési.</span><span class="sxs-lookup"><span data-stu-id="ce025-221">For example, the shutdown could wait for in-flight requests to be completed before the return.</span></span>

## <a name="start-the-web-server"></a><span data-ttu-id="ce025-222">Indítsa el a webkiszolgálón</span><span class="sxs-lookup"><span data-stu-id="ce025-222">Start the web server</span></span>
<span data-ttu-id="ce025-223">Most már készen áll létrehozásához, és térjen vissza a webkiszolgáló elindításához OwinCommunicationListener példánya.</span><span class="sxs-lookup"><span data-stu-id="ce025-223">You're now ready to create and return an instance of OwinCommunicationListener to start the web server.</span></span> <span data-ttu-id="ce025-224">Vissza a szolgáltatásosztály (WebService.cs), bírálja felül a `CreateServiceInstanceListeners()` módszert:</span><span class="sxs-lookup"><span data-stu-id="ce025-224">Back in the Service class (WebService.cs), override the `CreateServiceInstanceListeners()` method:</span></span>

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                           .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                           .Select(endpoint => endpoint.Name);

    return endpoints.Select(endpoint => new ServiceInstanceListener(
        serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
}
```

<span data-ttu-id="ce025-225">Ez akkor, ha a webes API *alkalmazás* és az OWIN *állomás* végül felel meg.</span><span class="sxs-lookup"><span data-stu-id="ce025-225">This is where the Web API *application* and the OWIN *host* finally meet.</span></span> <span data-ttu-id="ce025-226">A gazdagép (OwinCommunicationListener) kap egy példányát a *alkalmazás* (webes API) keresztül az indítási osztályt.</span><span class="sxs-lookup"><span data-stu-id="ce025-226">The host (OwinCommunicationListener) is given an instance of the *application* (Web API) via the Startup class.</span></span> <span data-ttu-id="ce025-227">A Service Fabric majd annak életciklusát felügyeli.</span><span class="sxs-lookup"><span data-stu-id="ce025-227">Service Fabric then manages its lifecycle.</span></span> <span data-ttu-id="ce025-228">Ebben a mintában általában a bármely kommunikációs verem kell követni.</span><span class="sxs-lookup"><span data-stu-id="ce025-228">This same pattern can typically be followed with any communication stack.</span></span>

## <a name="put-it-all-together"></a><span data-ttu-id="ce025-229">Az alkalmazás összeállítása</span><span class="sxs-lookup"><span data-stu-id="ce025-229">Put it all together</span></span>
<span data-ttu-id="ce025-230">Ebben a példában, nem kell semmit a `RunAsync()` metódust, így az adott felülbírálás egyszerűen távolítható el.</span><span class="sxs-lookup"><span data-stu-id="ce025-230">In this example, you don't need to do anything in the `RunAsync()` method, so that override can simply be removed.</span></span>

<span data-ttu-id="ce025-231">Kell, hogy a végső szolgáltatás megvalósítása nagyon egyszerű.</span><span class="sxs-lookup"><span data-stu-id="ce025-231">The final service implementation should be very simple.</span></span> <span data-ttu-id="ce025-232">Csak a kommunikációs figyelő létrehozásához szükséges:</span><span class="sxs-lookup"><span data-stu-id="ce025-232">It only needs to create the communication listener:</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Linq;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace WebService
{
    internal sealed class WebService : StatelessService
    {
        public WebService(StatelessServiceContext context)
            : base(context)
        { }

        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                                   .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                                   .Select(endpoint => endpoint.Name);

            return endpoints.Select(endpoint => new ServiceInstanceListener(
                serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
        }
    }
}
```

<span data-ttu-id="ce025-233">A teljes `OwinCommunicationListener` osztály:</span><span class="sxs-lookup"><span data-stu-id="ce025-233">The complete `OwinCommunicationListener` class:</span></span>

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        private readonly ServiceEventSource eventSource;
        private readonly Action<IAppBuilder> startup;
        private readonly ServiceContext serviceContext;
        private readonly string endpointName;
        private readonly string appRoot;

        private IDisposable webApp;
        private string publishAddress;
        private string listeningAddress;

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
            : this(startup, serviceContext, eventSource, endpointName, null)
        {
        }

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
        {
            if (startup == null)
            {
                throw new ArgumentNullException(nameof(startup));
            }

            if (serviceContext == null)
            {
                throw new ArgumentNullException(nameof(serviceContext));
            }

            if (endpointName == null)
            {
                throw new ArgumentNullException(nameof(endpointName));
            }

            if (eventSource == null)
            {
                throw new ArgumentNullException(nameof(eventSource));
            }

            this.startup = startup;
            this.serviceContext = serviceContext;
            this.endpointName = endpointName;
            this.eventSource = eventSource;
            this.appRoot = appRoot;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
            var protocol = serviceEndpoint.Protocol;
            int port = serviceEndpoint.Port;

            if (this.serviceContext is StatefulServiceContext)
            {
                StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}{3}/{4}/{5}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/',
                    statefulServiceContext.PartitionId,
                    statefulServiceContext.ReplicaId,
                    Guid.NewGuid());
            }
            else if (this.serviceContext is StatelessServiceContext)
            {
                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/');
            }
            else
            {
                throw new InvalidOperationException();
            }

            this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            try
            {
                this.eventSource.Message("Starting web server on " + this.listeningAddress);

                this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

                this.eventSource.Message("Listening on " + this.publishAddress);

                return Task.FromResult(this.publishAddress);
            }
            catch (Exception ex)
            {
                this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

                this.StopWebServer();

                throw;
            }
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.webApp != null)
            {
                try
                {
                    this.webApp.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}
```

<span data-ttu-id="ce025-234">Most, hogy Ön vezettek minden a helyére, a projekt egy tipikus webes API-alkalmazás megbízható szolgáltatások API belépési pontok és egy OWIN állomás kell hasonlítania.</span><span class="sxs-lookup"><span data-stu-id="ce025-234">Now that you have put all the pieces in place, your project should look like a typical Web API application with Reliable Services API entry points and an OWIN host.</span></span>

## <a name="run-and-connect-through-a-web-browser"></a><span data-ttu-id="ce025-235">Futtassa, és csatlakozzon egy webböngészőn keresztül</span><span class="sxs-lookup"><span data-stu-id="ce025-235">Run and connect through a web browser</span></span>
<span data-ttu-id="ce025-236">Ha még nem tette, [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ce025-236">If you haven't done so, [set up your development environment](service-fabric-get-started.md).</span></span>

<span data-ttu-id="ce025-237">Most hozza létre és telepítse a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ce025-237">You can now build and deploy your service.</span></span> <span data-ttu-id="ce025-238">Nyomja le az **F5** felépítéséhez és az alkalmazás központi telepítése a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ce025-238">Press **F5** in Visual Studio to build and deploy the application.</span></span> <span data-ttu-id="ce025-239">A diagnosztikai ablakban kell megjelennie, amely jelzi, hogy a webkiszolgáló megnyitott http://localhost:8281 üzenet /.</span><span class="sxs-lookup"><span data-stu-id="ce025-239">In the Diagnostic Events window, you should see a message that indicates that the web server opened on http://localhost:8281/.</span></span>

> [!NOTE]
> <span data-ttu-id="ce025-240">Ha a port már nyitva van a számítógépen egy másik folyamat, itt hiba jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="ce025-240">If the port has already been opened by another process on your machine, you may see an error here.</span></span> <span data-ttu-id="ce025-241">Ez azt jelzi, hogy a figyelő nem nyitható meg.</span><span class="sxs-lookup"><span data-stu-id="ce025-241">This indicates that the listener couldn't be opened.</span></span> <span data-ttu-id="ce025-242">Ha ez a helyzet, próbálkozzon egy másik portot használ a végpont-konfiguráció a ServiceManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="ce025-242">If that's the case, try using a different port for the endpoint configuration in ServiceManifest.xml.</span></span>
> 
> 

<span data-ttu-id="ce025-243">Ha a szolgáltatás fut, nyisson meg egy böngészőt, és keresse meg [http://localhost:8281/api/értékek](http://localhost:8281/api/values) tesztek.</span><span class="sxs-lookup"><span data-stu-id="ce025-243">Once the service is running, open a browser and navigate to [http://localhost:8281/api/values](http://localhost:8281/api/values) to test it.</span></span>

## <a name="scale-it-out"></a><span data-ttu-id="ce025-244">Méretezhető végrehajtását</span><span class="sxs-lookup"><span data-stu-id="ce025-244">Scale it out</span></span>
<span data-ttu-id="ce025-245">Állapot nélküli webalkalmazások kiterjesztése általában azt jelenti, hogy további gépek hozzáadása és a webalkalmazások rajtuk dolgozik.</span><span class="sxs-lookup"><span data-stu-id="ce025-245">Scaling out stateless web apps typically means adding more machines and spinning up the web apps on them.</span></span> <span data-ttu-id="ce025-246">A Service Fabric vezénylő alrendszer ehhez, amikor új csomópontokat ad hozzá egy fürt.</span><span class="sxs-lookup"><span data-stu-id="ce025-246">Service Fabric's orchestration engine can do this for you whenever new nodes are added to a cluster.</span></span> <span data-ttu-id="ce025-247">Állapot nélküli szolgáltatás példányának létrehozásakor megadhatja a létrehozandó példányok száma.</span><span class="sxs-lookup"><span data-stu-id="ce025-247">When you create instances of a stateless service, you can specify the number of instances you want to create.</span></span> <span data-ttu-id="ce025-248">A Service Fabric a példányok száma a fürtben lévő csomópontok helyezi.</span><span class="sxs-lookup"><span data-stu-id="ce025-248">Service Fabric places that number of instances on nodes in the cluster.</span></span> <span data-ttu-id="ce025-249">És biztosítja, hogy nem hozható létre egynél több példány bármely csomópontjára.</span><span class="sxs-lookup"><span data-stu-id="ce025-249">And it makes sure not to create more than one instance on any one node.</span></span> <span data-ttu-id="ce025-250">Azt is beállíthatja, hogy a Service Fabric mindig hozzon létre egy példányát minden egyes csomópontjára megadásával **-1** a példányok számát.</span><span class="sxs-lookup"><span data-stu-id="ce025-250">You can also instruct Service Fabric to always create an instance on every node by specifying **-1** for the instance count.</span></span> <span data-ttu-id="ce025-251">Ez biztosítja, hogy, hogy terjessze ki a fürt a csomópontok hozzáadásakor az állapot nélküli szolgáltatás egy példánya létrejön az új csomópontján.</span><span class="sxs-lookup"><span data-stu-id="ce025-251">This guarantees that whenever you add nodes to scale out your cluster, an instance of your stateless service will be created on the new nodes.</span></span> <span data-ttu-id="ce025-252">Ez az érték a szolgáltatáspéldány tulajdonsága, úgy van beállítva a példány létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="ce025-252">This value is a property of the service instance, so it is set when you create a service instance.</span></span> <span data-ttu-id="ce025-253">Ez a Powershellen keresztül teheti meg:</span><span class="sxs-lookup"><span data-stu-id="ce025-253">You can do this through PowerShell:</span></span>

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

<span data-ttu-id="ce025-254">Is ehhez amikor állapotmentes szolgáltatások Visual Studio-projekt megadhat egy alapértelmezett szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="ce025-254">You can also do this when you define a default service in a Visual Studio stateless service project:</span></span>

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

<span data-ttu-id="ce025-255">Alkalmazás és szolgáltatás példányainak létrehozásával kapcsolatos további információkért lásd: [alkalmazás üzembe helyezése](service-fabric-deploy-remove-applications.md).</span><span class="sxs-lookup"><span data-stu-id="ce025-255">For more information on how to create application and service instances, see [Deploy an application](service-fabric-deploy-remove-applications.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ce025-256">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ce025-256">Next steps</span></span>
[<span data-ttu-id="ce025-257">A Service Fabric-alkalmazás hibakeresése a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="ce025-257">Debug your Service Fabric application by using Visual Studio</span></span>](service-fabric-debugging-your-application.md)

