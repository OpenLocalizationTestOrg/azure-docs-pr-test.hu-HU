---
title: "ASP.NET Web API hello aaaService kommunikáció |} Microsoft Docs"
description: "Ismerje meg, hogyan tooimplement szolgáltatások közötti kommunikáció részletes használatával hello az OWIN önálló üzemeltető hello megbízható szolgáltatások API az ASP.NET Web API."
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
ms.openlocfilehash: 3fb18fcb141ada0d79a0acda3dccbc7fb044346d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a><span data-ttu-id="e8e0f-103">Első lépések: az OWIN futtató önálló Service Fabric Web API szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="e8e0f-103">Get started: Service Fabric Web API services with OWIN self-hosting</span></span>
<span data-ttu-id="e8e0f-104">Az Azure Service Fabric hello power helyezi el a beavatkozás nélküli Amikor eldönti, hogy hogyan kívánja a szolgáltatások toocommunicate felhasználóival és egymással.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-104">Azure Service Fabric puts hello power in your hands when you're deciding how you want your services toocommunicate with users and with each other.</span></span> <span data-ttu-id="e8e0f-105">Ez az oktatóanyag végzett szolgáltatások közötti kommunikáció ASP.NET Web API-t használja az Open Web Interface .NET (OWIN) önálló tárolásához a Service Fabric megbízható szolgáltatások API megvalósítására koncentrál.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-105">This tutorial focuses on implementing service communication using ASP.NET Web API with Open Web Interface for .NET (OWIN) self-hosting in Service Fabric's Reliable Services API.</span></span> <span data-ttu-id="e8e0f-106">Azt fogja elmélyedhet mélyen hello Reliable Services moduláris kommunikációs API.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-106">We'll delve deeply into hello Reliable Services pluggable communication API.</span></span> <span data-ttu-id="e8e0f-107">Egy részletes példa tooshow a Web API is használjuk, hogyan tooset be egy egyéni kommunikációs figyelő.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-107">We'll also use Web API in a step-by-step example tooshow you how tooset up a custom communication listener.</span></span>

## <a name="introduction-tooweb-api-in-service-fabric"></a><span data-ttu-id="e8e0f-108">Bevezetés tooWeb API a Service Fabric</span><span class="sxs-lookup"><span data-stu-id="e8e0f-108">Introduction tooWeb API in Service Fabric</span></span>
<span data-ttu-id="e8e0f-109">ASP.NET webes API-t egy népszerű és erőteljes környezet HTTP API-k hello .NET-keretrendszer felett.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-109">ASP.NET Web API is a popular and powerful framework for building HTTP APIs on top of hello .NET Framework.</span></span> <span data-ttu-id="e8e0f-110">Ha még nem már ismeri a hello keretrendszer, lásd: [Ismerkedés az ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) további toolearn.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-110">If you're not already familiar with hello framework, see [Getting started with ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) toolearn more.</span></span>

<span data-ttu-id="e8e0f-111">Webes API-t a Service Fabric rendszer hello azonos ASP.NET Web API megszokott és kedvelt.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-111">Web API in Service Fabric is hello same ASP.NET Web API you know and love.</span></span> <span data-ttu-id="e8e0f-112">hello különbség a hogyan meg *állomás* egy webes API-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-112">hello difference is in how you *host* a Web API application.</span></span> <span data-ttu-id="e8e0f-113">Nem fogja használni a Microsoft Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="e8e0f-113">You won't be using Microsoft Internet Information Services (IIS).</span></span> <span data-ttu-id="e8e0f-114">toobetter hello különbség ismertetése, most felosztása két részből áll:</span><span class="sxs-lookup"><span data-stu-id="e8e0f-114">toobetter understand hello difference, let's break it into two parts:</span></span>

1. <span data-ttu-id="e8e0f-115">hello webes API-alkalmazás (beleértve a tartományvezérlőket és -modellek)</span><span class="sxs-lookup"><span data-stu-id="e8e0f-115">hello Web API application (including controllers and models)</span></span>
2. <span data-ttu-id="e8e0f-116">hello állomás (hello webkiszolgáló, általában az IIS)</span><span class="sxs-lookup"><span data-stu-id="e8e0f-116">hello host (hello web server, usually IIS)</span></span>

<span data-ttu-id="e8e0f-117">Egy magát webes API-alkalmazás nem változik.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-117">A Web API application itself doesn't change.</span></span> <span data-ttu-id="e8e0f-118">Elmúlt hello előfordulhat, hogy írt webes API-alkalmazások nem más, és el tudja toosimply áthelyezés alatt az alkalmazás kódjában a legtöbb.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-118">It's no different from Web API applications you may have written in hello past, and you should be able toosimply move over most of your application code.</span></span> <span data-ttu-id="e8e0f-119">Ha Ön már lett üzemeltető IIS-kiszolgálón, ahol hello alkalmazás működteti, de miről használt kissé eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-119">But if you've been hosting on IIS, where you host hello application may be a little different from what you're used to.</span></span> <span data-ttu-id="e8e0f-120">Előtt tartalmazó rész toohello beszerzéséhez azt kezdjük valami további ismerős: hello webes API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-120">Before we get toohello hosting part, let's start with something more familiar: hello Web API application.</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="e8e0f-121">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8e0f-121">Create hello application</span></span>
<span data-ttu-id="e8e0f-122">Először hozzon létre egy új Service Fabric-alkalmazás egy állapot nélküli szolgáltatásnak a Visual Studio 2015-öt.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-122">Start by creating a new Service Fabric application with a single stateless service in Visual Studio 2015.</span></span>

<span data-ttu-id="e8e0f-123">A webes API-jával állapotmentes szolgáltatások Visual Studio sablonját elérhető tooyou.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-123">A Visual Studio template for a stateless service using Web API is available tooyou.</span></span> <span data-ttu-id="e8e0f-124">Ebben az oktatóanyagban azt, hogy milyen visszajelzést kap, ha a kiválasztott sablon eredménye teljesen új webes API a projekt fogja létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-124">In this tutorial, we'll build a Web API project from scratch that results in what you'd get if you selected this template.</span></span>

<span data-ttu-id="e8e0f-125">Válasszon egy üres állapotmentes szolgáltatások projekt toolearn hogyan toobuild teljesen, vagy a Web API-projektet hello állapotmentes szolgáltatások Web API sablon kezdődhet és egyszerűen követéséhez.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-125">Select a blank Stateless Service project toolearn how toobuild a Web API project from scratch, or you can start with hello stateless service Web API template and simply follow along.</span></span>  

<span data-ttu-id="e8e0f-126">hello első lépés az egyes NuGet-csomagok webes API toopull.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-126">hello first step is toopull in some NuGet packages for Web API.</span></span> <span data-ttu-id="e8e0f-127">azt szeretnénk, ha toouse hello csomag Microsoft.AspNet.WebApi.OwinSelfHost.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-127">hello package we want toouse is Microsoft.AspNet.WebApi.OwinSelfHost.</span></span> <span data-ttu-id="e8e0f-128">Ez a csomag tartalmazza az összes hello szükséges webes API-csomagok és hello *állomás* csomagok.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-128">This package includes all hello necessary Web API packages and hello *host* packages.</span></span> <span data-ttu-id="e8e0f-129">Ez lesz fontos később.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-129">This will be important later.</span></span>

<span data-ttu-id="e8e0f-130">Hello csomagok telepítése után megkezdheti az épület kimenő hello alapszintű webes API projektet struktúrát.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-130">After hello packages have been installed, you can begin building out hello basic Web API project structure.</span></span> <span data-ttu-id="e8e0f-131">Ha már használta a Web API, hello szerkezetének tisztában kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-131">If you've used Web API, hello project structure should look very familiar.</span></span> <span data-ttu-id="e8e0f-132">Először vegyen fel egy `Controllers` könyvtár és egy egyszerű érték tartományvezérlő:</span><span class="sxs-lookup"><span data-stu-id="e8e0f-132">Start by adding a `Controllers` directory and a simple values controller:</span></span>

<span data-ttu-id="e8e0f-133">**ValuesController.cs**</span><span class="sxs-lookup"><span data-stu-id="e8e0f-133">**ValuesController.cs**</span></span>

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

<span data-ttu-id="e8e0f-134">A következő indítási osztály hozzáadása hello projekt legfelső szintű tooregister hello útválasztási is és egyéb konfigurációs beállítása.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-134">Next, add a Startup class at hello project root tooregister hello routing, formatters, and any other configuration setup.</span></span> <span data-ttu-id="e8e0f-135">Ez egyben ahol Web API csatlakozik toohello *állomás*, amely fog kell javított változat újra később.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-135">This is also where Web API plugs in toohello *host*, which will be revisited again later.</span></span> 

<span data-ttu-id="e8e0f-136">**Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="e8e0f-136">**Startup.cs**</span></span>

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

<span data-ttu-id="e8e0f-137">Ez a hello alkalmazás rész.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-137">That's it for hello application part.</span></span> <span data-ttu-id="e8e0f-138">Ezen a ponton azt beállítása csak hello alapszintű webes API projektet elrendezés.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-138">At this point, we've set up just hello basic Web API project layout.</span></span> <span data-ttu-id="e8e0f-139">Az eddigi azt nem szabad különbség hello korábbi lehet, hogy írt webes API-projektet, illetve hello egyszerű webes API-sablon.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-139">So far, it shouldn't look much different from Web API projects you may have written in hello past or from hello basic Web API template.</span></span> <span data-ttu-id="e8e0f-140">Az üzleti logikát a megszokott módon kerül hello, tartományvezérlői és modellek.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-140">Your business logic goes in hello controllers and models as usual.</span></span>

<span data-ttu-id="e8e0f-141">Most mi a teendő-k üzemeltetésére szolgál, hogy azt ténylegesen futtatható?</span><span class="sxs-lookup"><span data-stu-id="e8e0f-141">Now what do we do about hosting so that we can actually run it?</span></span>

## <a name="service-hosting"></a><span data-ttu-id="e8e0f-142">Üzemeltető szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="e8e0f-142">Service hosting</span></span>
<span data-ttu-id="e8e0f-143">A Service Fabric, a szolgáltatás fut a *gazdafolyamat szolgáltatás*, egy végrehajtható fájl, amely a szolgáltatáskód hibáit futtatja.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-143">In Service Fabric, your service runs in a *service host process*, an executable file that runs your service code.</span></span> <span data-ttu-id="e8e0f-144">Írásakor szolgáltatás hello megbízható szolgáltatások API használatával, a projekt csak akkor tooan végrehajtható fájl, a szolgáltatás típusának regisztrálja, és futtatja a kódot.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-144">When you write a service by using hello Reliable Services API, your service project just compiles tooan executable file that registers your service type and runs your code.</span></span> <span data-ttu-id="e8e0f-145">Ez igaz a legtöbb esetben a szolgáltatás a Service Fabric a .NET írásakor.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-145">This is true in most cases when you write a service on Service Fabric in .NET.</span></span> <span data-ttu-id="e8e0f-146">Hello állapotmentes szolgáltatások projekt Program.cs megnyitásakor kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="e8e0f-146">When you open Program.cs in hello stateless service project, you should see:</span></span>

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

<span data-ttu-id="e8e0f-147">Ha, amely a következőképpen néz gyanúsan hello belépési pont tooa Konzolalkalmazás, mert van.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-147">If that looks suspiciously like hello entry point tooa console application, that's because it is.</span></span>

<span data-ttu-id="e8e0f-148">Kapcsolatos részletes adatok hello szolgáltatás gazdafolyamat és szolgáltatás regisztrációs Ez a cikk hello terjed.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-148">Further details about hello service host process and service registration are beyond hello scope of this article.</span></span> <span data-ttu-id="e8e0f-149">A fontos tooknow, de most, hogy *a szolgáltatáskód hibáit külön folyamatban fut.*.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-149">But it's important tooknow for now that *your service code is running in its own process*.</span></span>

## <a name="self-host-web-api-with-an-owin-host"></a><span data-ttu-id="e8e0f-150">Önálló gazdagép Web API egy OWIN állomás</span><span class="sxs-lookup"><span data-stu-id="e8e0f-150">Self-host Web API with an OWIN host</span></span>
<span data-ttu-id="e8e0f-151">Fényében, hogy a webes API-alkalmazás kódja egy külön folyamatban, hogyan tegye meg a számítógéphez, tooa webkiszolgáló?</span><span class="sxs-lookup"><span data-stu-id="e8e0f-151">Given that your Web API application code is hosted in its own process, how do you hook it up tooa web server?</span></span> <span data-ttu-id="e8e0f-152">Adja meg [OWIN](http://owin.org/).</span><span class="sxs-lookup"><span data-stu-id="e8e0f-152">Enter [OWIN](http://owin.org/).</span></span> <span data-ttu-id="e8e0f-153">OWIN: egyszerűen egy szerződés .NET webes alkalmazások és a webkiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-153">OWIN is simply a contract between .NET web applications and web servers.</span></span> <span data-ttu-id="e8e0f-154">Hagyományosan az ASP.NET (felfelé tooMVC 5) használatakor a hello webalkalmazás, szorosan összekapcsolt tooIIS System.Web keresztül.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-154">Traditionally when ASP.NET (up tooMVC 5) is used, hello web application is tightly coupled tooIIS through System.Web.</span></span> <span data-ttu-id="e8e0f-155">Azonban a Web API OWIN, megvalósítja, az azt üzemeltető webkiszolgáló hello írhat egy webes alkalmazás, amely le van.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-155">However, Web API implements OWIN, so you can write a web application that is decoupled from hello web server that hosts it.</span></span> <span data-ttu-id="e8e0f-156">Ebből kifolyólag használhatja egy *önállóan üzemel* OWIN webkiszolgálón, amelyek a saját folyamat megkezdése.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-156">Because of this, you can use a *self-hosted* OWIN web server that you can start in your own process.</span></span> <span data-ttu-id="e8e0f-157">Ez tökéletesen hello Service Fabric üzemeltetési modell csak leírt illeszkedik.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-157">This fits perfectly with hello Service Fabric hosting model we just described.</span></span>

<span data-ttu-id="e8e0f-158">Ebben a cikkben Katana használjuk hello OWIN gazdagépként hello webes API-alkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-158">In this article, we'll use Katana as hello OWIN host for hello Web API application.</span></span> <span data-ttu-id="e8e0f-159">Katana egy nyílt forráskódú OWIN állomás implementáció épülő [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) és a hello Windows [HTTP-kiszolgáló API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="e8e0f-159">Katana is an open-source OWIN host implementation built on [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) and hello Windows [HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="e8e0f-160">További információk Katana, nyissa meg toohello toolearn [Katana hely](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana).</span><span class="sxs-lookup"><span data-stu-id="e8e0f-160">toolearn more about Katana, go toohello [Katana site](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana).</span></span> <span data-ttu-id="e8e0f-161">A gyors áttekintést a toouse Katana tooself-állomás Web API, lásd: [használata OWIN tooSelf-állomás ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).</span><span class="sxs-lookup"><span data-stu-id="e8e0f-161">For a quick overview of how toouse Katana tooself-host Web API, see [Use OWIN tooSelf-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).</span></span>
> 
> 

## <a name="set-up-hello-web-server"></a><span data-ttu-id="e8e0f-162">Hello webkiszolgáló beállítása</span><span class="sxs-lookup"><span data-stu-id="e8e0f-162">Set up hello web server</span></span>
<span data-ttu-id="e8e0f-163">hello megbízható szolgáltatások API csatlakoztatható, ahol a kommunikációs verem, amelyek lehetővé teszik a felhasználók és az ügyfelek tooconnect toohello szolgáltatás kommunikációs belépési pontot biztosít:</span><span class="sxs-lookup"><span data-stu-id="e8e0f-163">hello Reliable Services API provides a communication entry point where you can plug in communication stacks that allow users and clients tooconnect toohello service:</span></span>

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

<span data-ttu-id="e8e0f-164">hello web server (és más kommunikációs verem használhatja a későbbi, mint például a websocket elemek hello) kell használni, hello ICommunicationListener felület toointegrate megfelelően hello.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-164">hello web server (and any other communication stack you use in hello future, such as WebSockets) should use hello ICommunicationListener interface toointegrate correctly with hello system.</span></span> <span data-ttu-id="e8e0f-165">hello okai az az alábbi lépésekkel hello több nyilvánvalóvá vált.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-165">hello reasons for this will become more apparent in hello following steps.</span></span>

<span data-ttu-id="e8e0f-166">Először hozzon létre egy osztályt, amely megvalósítja az ICommunicationListener OwinCommunicationListener:</span><span class="sxs-lookup"><span data-stu-id="e8e0f-166">First, create a class called OwinCommunicationListener that implements ICommunicationListener:</span></span>

<span data-ttu-id="e8e0f-167">**OwinCommunicationListener.cs**</span><span class="sxs-lookup"><span data-stu-id="e8e0f-167">**OwinCommunicationListener.cs**</span></span>

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

<span data-ttu-id="e8e0f-168">hello ICommunicationListener felületet biztosít a három módszer toomanage kommunikációs figyelő a szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="e8e0f-168">hello ICommunicationListener interface provides three methods toomanage a communication listener for your service:</span></span>

* <span data-ttu-id="e8e0f-169">*OpenAsync*.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-169">*OpenAsync*.</span></span> <span data-ttu-id="e8e0f-170">Indítsa el a kérések figyelését.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-170">Start listening for requests.</span></span>
* <span data-ttu-id="e8e0f-171">*CloseAsync*.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-171">*CloseAsync*.</span></span> <span data-ttu-id="e8e0f-172">Állítsa le a kérések figyelését, Befejezés bármilyen az üzenetsoroktól kérelmeket és leállítása.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-172">Stop listening for requests, finish any in-flight requests, and shut down gracefully.</span></span>
* <span data-ttu-id="e8e0f-173">*Megszakítás*.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-173">*Abort*.</span></span> <span data-ttu-id="e8e0f-174">Minden megszakítja, és azonnal leállítja.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-174">Cancel everything and stop immediately.</span></span>

<span data-ttu-id="e8e0f-175">tooget indult el, adja hozzá a titkos osztálytagok, a dolgok hello figyelő toofunction kell.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-175">tooget started, add private class members for things hello listener will need toofunction.</span></span> <span data-ttu-id="e8e0f-176">Ezek és felhasználandó hello konstruktor inicializálja, valamint később hello figyelő URL-cím beállításakor.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-176">These will be initialized through hello constructor and used later when you set up hello listening URL.</span></span>

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

## <a name="implement-openasync"></a><span data-ttu-id="e8e0f-177">OpenAsync megvalósítása</span><span class="sxs-lookup"><span data-stu-id="e8e0f-177">Implement OpenAsync</span></span>
<span data-ttu-id="e8e0f-178">mint hello webkiszolgálót tooset, kétféle információt kell:</span><span class="sxs-lookup"><span data-stu-id="e8e0f-178">tooset up hello web server, you need two pieces of information:</span></span>

* <span data-ttu-id="e8e0f-179">*Egy URL-cím elérési út előtag*.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-179">*A URL path prefix*.</span></span> <span data-ttu-id="e8e0f-180">Bár nem kötelező, továbbra is meg tooset ideális ez akár most, hogy biztonságosan tárolhatja, webes szolgáltatásokat az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-180">Although this is optional, it's good for you tooset this up now so that you can safely host multiple web services in your application.</span></span>
* <span data-ttu-id="e8e0f-181">*A port*.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-181">*A port*.</span></span>

<span data-ttu-id="e8e0f-182">Mielőtt hello web Server port kap, fontos, hogy megértette, hogy a Service Fabric, amely az alkalmazás- és hello alapul szolgáló operációs rendszer, amely az fut közötti pufferként alkalmazás réteget biztosít.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-182">Before you get a port for hello web server, it's important that you understand that Service Fabric provides an application layer that acts as a buffer between your application and hello underlying operating system that it runs on.</span></span> <span data-ttu-id="e8e0f-183">A Service Fabric tartalmaz egy módja tooconfigure *végpontok* a szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-183">As such, Service Fabric provides a way tooconfigure *endpoints* for your services.</span></span> <span data-ttu-id="e8e0f-184">A Service Fabric biztosítja, hogy végpontok száma a szolgáltatás toouse érhető el.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-184">Service Fabric ensures that endpoints are available for your service toouse.</span></span> <span data-ttu-id="e8e0f-185">Ezzel a módszerrel nem kell tooconfigure őket az alapul szolgáló operációs rendszer környezet hello.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-185">This way, you don't have tooconfigure them yourself in hello underlying OS environment.</span></span> <span data-ttu-id="e8e0f-186">A Service Fabric-alkalmazás különböző környezetekben könnyen tárolhatja, anélkül, hogy toomake módosítások tooyour alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-186">You can easily host your Service Fabric application in different environments without having toomake any changes tooyour application.</span></span> <span data-ttu-id="e8e0f-187">(Például tárolhatja, ugyanazt az alkalmazást az Azure-ban vagy a saját adatközpont hello.)</span><span class="sxs-lookup"><span data-stu-id="e8e0f-187">(For example, you can host hello same application in Azure or in your own datacenter.)</span></span>

<span data-ttu-id="e8e0f-188">HTTP-végponttal PackageRoot\ServiceManifest.xml konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="e8e0f-188">Configure an HTTP endpoint in PackageRoot\ServiceManifest.xml:</span></span>

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

<span data-ttu-id="e8e0f-189">Ez a lépés fontos, mert hello szolgáltatás gazdafolyamat futtatja korlátozott credentials (a Windows hálózati szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="e8e0f-189">This step is important because hello service host process runs under restricted credentials (Network Service on Windows).</span></span> <span data-ttu-id="e8e0f-190">Ez azt jelenti, hogy a szolgáltatás nem hozzáférés tooset be HTTP-végponttal rendelkezik önállóan.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-190">This means that your service won't have access tooset up an HTTP endpoint on its own.</span></span> <span data-ttu-id="e8e0f-191">Hello végpont-konfiguráció használatával a Service Fabric tooset hello megfelelő hozzáférés-vezérlési lista (ACL) fel tudja hello hello szolgáltatás URL-címet figyelni fogja a.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-191">By using hello endpoint configuration, Service Fabric knows tooset up hello proper access control list (ACL) for hello URL that hello service will listen on.</span></span> <span data-ttu-id="e8e0f-192">A Service Fabric is itt szabványos tooconfigure végpontok.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-192">Service Fabric also provides a standard place tooconfigure endpoints.</span></span>

<span data-ttu-id="e8e0f-193">Vissza OwinCommunicationListener.cs, akkor kezdhet OpenAsync megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-193">Back in OwinCommunicationListener.cs, you can start implementing OpenAsync.</span></span> <span data-ttu-id="e8e0f-194">Ez az az először hello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-194">This is where you start hello web server.</span></span> <span data-ttu-id="e8e0f-195">Először hello végpont információért, és hozzon létre hello szolgáltatás figyelni fogja hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-195">First, get hello endpoint information and create hello URL that hello service will listen on.</span></span> <span data-ttu-id="e8e0f-196">hello URL-címe eltérő, attól függően, hogy hello figyelő használatban van egy állapotmentes szolgáltatások vagy egy állapotalapú szolgáltatás lesz.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-196">hello URL will be different depending on whether hello listener is used in a stateless service or a stateful service.</span></span> <span data-ttu-id="e8e0f-197">Egy állapotalapú szolgáltatás hello figyelő kell egy egyedi címét minden állapotalapú szolgáltatási replika azt figyeli a toocreate.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-197">For a stateful service, hello listener needs toocreate a unique address for every stateful service replica it listens on.</span></span> <span data-ttu-id="e8e0f-198">Az állapotmentes szolgáltatások hello cím jóval egyszerűbb lehet.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-198">For stateless services, hello address can be much simpler.</span></span> 

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

<span data-ttu-id="e8e0f-199">Vegye figyelembe, hogy a "http://+" Itt nem használja.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-199">Note that "http://+" is used here.</span></span> <span data-ttu-id="e8e0f-200">Ez a toomake meg arról, hogy az összes elérhető címek, beleértve a localhost, a teljes tartománynév és a hello gép IP-hello webkiszolgálóra figyeli.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-200">This is toomake sure that hello web server is listening on all available addresses, including localhost, FQDN, and hello machine IP.</span></span>

<span data-ttu-id="e8e0f-201">hello OpenAsync a megvalósítása hello miért hello web server (vagy bármely kommunikációs verem) megvalósítása közvetlenül a nyílt egy ICommunicationListener, hanem csak az egyik legfontosabb okok valamelyike miatt `RunAsync()` hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-201">hello OpenAsync implementation is one of hello most important reasons why hello web server (or any communication stack) is implemented as an ICommunicationListener, rather than just having it open directly from `RunAsync()` in hello service.</span></span> <span data-ttu-id="e8e0f-202">hello visszatérési érték a OpenAsync a webkiszolgáló hello hello címet figyeli.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-202">hello return value from OpenAsync is hello address that hello web server is listening on.</span></span> <span data-ttu-id="e8e0f-203">Ez a cím toohello rendszer ad vissza, ha regisztrálja azokat hello cím hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-203">When this address is returned toohello system, it registers hello address with hello service.</span></span> <span data-ttu-id="e8e0f-204">A Service Fabric biztosít az API-k, amely lehetővé teszi az ügyfelek és egyéb szolgáltatások toothen feltenni ehhez a címhez szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-204">Service Fabric provides an API that allows clients and other services toothen ask for this address by service name.</span></span> <span data-ttu-id="e8e0f-205">Ez azért fontos, mert hello szolgáltatás cím nem statikus.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-205">This is important because hello service address is not static.</span></span> <span data-ttu-id="e8e0f-206">Szolgáltatások hello fürt erőforrás és a rendelkezésre állási célokra helyezi át.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-206">Services are moved around in hello cluster for resource balancing and availability purposes.</span></span> <span data-ttu-id="e8e0f-207">Ez az hello mechanizmus, amely lehetővé teszi az ügyfelek tooresolve hello cím egy szolgáltatás figyeli.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-207">This is hello mechanism that allows clients tooresolve hello listening address for a service.</span></span>

<span data-ttu-id="e8e0f-208">Vele szem előtt OpenAsync hello webkiszolgáló elindul, és adja vissza, amely figyeli a hello cím.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-208">With that in mind, OpenAsync starts hello web server and returns hello address that it's listening on.</span></span> <span data-ttu-id="e8e0f-209">Vegye figyelembe, hogy a "http://+" figyeli, de OpenAsync hello címét adja meg, mielőtt hello "+" hello IP vagy FQDN-jét hello csomópont jelenleg a rendszer lecseréli.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-209">Note that it listens on "http://+", but before OpenAsync returns hello address, hello "+" is replaced with hello IP or FQDN of hello node it is currently on.</span></span> <span data-ttu-id="e8e0f-210">Mi hello rendszer regisztrálva van-e metódus által visszaadott hello cím tartozik.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-210">hello address that is returned by this method is what's registered with hello system.</span></span> <span data-ttu-id="e8e0f-211">Akkor is ügyfelek és egyéb szolgáltatások látják, ha a szolgáltatás címét, hogy kérni.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-211">It's also what clients and other services see when they ask for a service's address.</span></span> <span data-ttu-id="e8e0f-212">Az ügyfelek toocorrectly csatlakozni a tooit, egy tényleges IP vagy FQDN Formátumban kell hello címét.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-212">For clients toocorrectly connect tooit, they need an actual IP or FQDN in hello address.</span></span>

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
        this.eventSource.Message("Web server failed tooopen endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

<span data-ttu-id="e8e0f-213">Vegye figyelembe, hogy ez hivatkozik, amely toohello OwinCommunicationListener hello konstruktorban átadott hello indítási osztály.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-213">Note that this references hello Startup class that was passed in toohello OwinCommunicationListener in hello constructor.</span></span> <span data-ttu-id="e8e0f-214">Ez a példány indítási hello web server toobootstrap hello webes API-alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-214">This startup instance is used by hello web server toobootstrap hello Web API application.</span></span>

<span data-ttu-id="e8e0f-215">Hello `ServiceEventSource.Current.Message()` sor hello diagnosztikai események ablakban jelenik később futtatásakor hello alkalmazás tooconfirm hello webkiszolgálóra sikeresen elindult.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-215">hello `ServiceEventSource.Current.Message()` line will appear in hello Diagnostic Events window later, when you run hello application tooconfirm that hello web server has started successfully.</span></span>

## <a name="implement-closeasync-and-abort"></a><span data-ttu-id="e8e0f-216">Alkalmazzon CloseAsync és Abort</span><span class="sxs-lookup"><span data-stu-id="e8e0f-216">Implement CloseAsync and Abort</span></span>
<span data-ttu-id="e8e0f-217">Végezetül megvalósítása CloseAsync és a megszakítási toostop hello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-217">Finally, implement both CloseAsync and Abort toostop hello web server.</span></span> <span data-ttu-id="e8e0f-218">hello webkiszolgáló hello server leíró OpenAsync során létrehozott ártalmatlanítása által állítható le.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-218">hello web server can be stopped by disposing hello server handle that was created during OpenAsync.</span></span>

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

<span data-ttu-id="e8e0f-219">A megvalósítás példában CloseAsync és a megszakítási egyszerűen állítsa le hello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-219">In this implementation example, both CloseAsync and Abort simply stop hello web server.</span></span> <span data-ttu-id="e8e0f-220">Úgy is dönthet, tooperform hello webkiszolgálón CloseAsync több szabályosan koordinált leállítására.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-220">You may opt tooperform a more gracefully coordinated shutdown of hello web server in CloseAsync.</span></span> <span data-ttu-id="e8e0f-221">Például hello leállítási sikerült Várjon, amíg az üzenetsoroktól kérelmek toobe előtt hello vissza.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-221">For example, hello shutdown could wait for in-flight requests toobe completed before hello return.</span></span>

## <a name="start-hello-web-server"></a><span data-ttu-id="e8e0f-222">Indítsa el a hello webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="e8e0f-222">Start hello web server</span></span>
<span data-ttu-id="e8e0f-223">Most már készen áll a toocreate, és térjen vissza a OwinCommunicationListener toostart hello web server-példányt.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-223">You're now ready toocreate and return an instance of OwinCommunicationListener toostart hello web server.</span></span> <span data-ttu-id="e8e0f-224">Vissza a hello szolgáltatás osztály (WebService.cs), bírálja felül a hello `CreateServiceInstanceListeners()` módszert:</span><span class="sxs-lookup"><span data-stu-id="e8e0f-224">Back in hello Service class (WebService.cs), override hello `CreateServiceInstanceListeners()` method:</span></span>

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

<span data-ttu-id="e8e0f-225">Amennyiben ez van hello Web API *alkalmazás* és hello OWIN *állomás* végül felel meg.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-225">This is where hello Web API *application* and hello OWIN *host* finally meet.</span></span> <span data-ttu-id="e8e0f-226">hello állomás (OwinCommunicationListener) kap hello példányának *alkalmazás* (webes API) keresztül hello indítási osztályt.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-226">hello host (OwinCommunicationListener) is given an instance of hello *application* (Web API) via hello Startup class.</span></span> <span data-ttu-id="e8e0f-227">A Service Fabric majd annak életciklusát felügyeli.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-227">Service Fabric then manages its lifecycle.</span></span> <span data-ttu-id="e8e0f-228">Ebben a mintában általában a bármely kommunikációs verem kell követni.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-228">This same pattern can typically be followed with any communication stack.</span></span>

## <a name="put-it-all-together"></a><span data-ttu-id="e8e0f-229">Az alkalmazás összeállítása</span><span class="sxs-lookup"><span data-stu-id="e8e0f-229">Put it all together</span></span>
<span data-ttu-id="e8e0f-230">Ebben a példában a semmi a hello toodo nem kell `RunAsync()` metódust, így az adott felülbírálás egyszerűen távolítható el.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-230">In this example, you don't need toodo anything in hello `RunAsync()` method, so that override can simply be removed.</span></span>

<span data-ttu-id="e8e0f-231">hello végső szolgáltatás megvalósítása nagyon egyszerű kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-231">hello final service implementation should be very simple.</span></span> <span data-ttu-id="e8e0f-232">Csak toocreate hello kommunikációs figyelő van szüksége:</span><span class="sxs-lookup"><span data-stu-id="e8e0f-232">It only needs toocreate hello communication listener:</span></span>

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

<span data-ttu-id="e8e0f-233">teljes hello `OwinCommunicationListener` osztály:</span><span class="sxs-lookup"><span data-stu-id="e8e0f-233">hello complete `OwinCommunicationListener` class:</span></span>

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
                this.eventSource.Message("Web server failed tooopen endpoint {0}. {1}", this.endpointName, ex.ToString());

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

<span data-ttu-id="e8e0f-234">Most, hogy Ön vezettek összes hello kódrészletek, a projekt egy tipikus webes API-alkalmazás megbízható szolgáltatások API belépési pontok és egy OWIN állomás kell hasonlítania.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-234">Now that you have put all hello pieces in place, your project should look like a typical Web API application with Reliable Services API entry points and an OWIN host.</span></span>

## <a name="run-and-connect-through-a-web-browser"></a><span data-ttu-id="e8e0f-235">Futtassa, és csatlakozzon egy webböngészőn keresztül</span><span class="sxs-lookup"><span data-stu-id="e8e0f-235">Run and connect through a web browser</span></span>
<span data-ttu-id="e8e0f-236">Ha még nem tette, [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e8e0f-236">If you haven't done so, [set up your development environment](service-fabric-get-started.md).</span></span>

<span data-ttu-id="e8e0f-237">Most hozza létre és telepítse a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-237">You can now build and deploy your service.</span></span> <span data-ttu-id="e8e0f-238">Nyomja le az **F5** a Visual Studio toobuild hello alkalmazás és központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-238">Press **F5** in Visual Studio toobuild and deploy hello application.</span></span> <span data-ttu-id="e8e0f-239">Hello diagnosztikai események ablakában kell megjelennie a http://localhost:8281 megnyitott hello webkiszolgálóra jelző üzenet /.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-239">In hello Diagnostic Events window, you should see a message that indicates that hello web server opened on http://localhost:8281/.</span></span>

> [!NOTE]
> <span data-ttu-id="e8e0f-240">Ha hello port a számítógépen egy másik folyamat már meg van nyitva, itt hiba jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-240">If hello port has already been opened by another process on your machine, you may see an error here.</span></span> <span data-ttu-id="e8e0f-241">Ez azt jelzi, hogy hello figyelő nem nyitható meg.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-241">This indicates that hello listener couldn't be opened.</span></span> <span data-ttu-id="e8e0f-242">Ha hello esetben próbálkozzon egy másik portot használ a ServiceManifest.xml hello végpont-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-242">If that's hello case, try using a different port for hello endpoint configuration in ServiceManifest.xml.</span></span>
> 
> 

<span data-ttu-id="e8e0f-243">Miután hello szolgáltatás fut, nyisson meg egy böngészőt, és keresse meg a túl[http://localhost:8281/api/értékek](http://localhost:8281/api/values) tootest azt.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-243">Once hello service is running, open a browser and navigate too[http://localhost:8281/api/values](http://localhost:8281/api/values) tootest it.</span></span>

## <a name="scale-it-out"></a><span data-ttu-id="e8e0f-244">Méretezhető végrehajtását</span><span class="sxs-lookup"><span data-stu-id="e8e0f-244">Scale it out</span></span>
<span data-ttu-id="e8e0f-245">Állapot nélküli webalkalmazások kiterjesztése általában azt jelenti, hogy további gépek hozzáadása és hello webalkalmazások rajtuk dolgozik.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-245">Scaling out stateless web apps typically means adding more machines and spinning up hello web apps on them.</span></span> <span data-ttu-id="e8e0f-246">A Service Fabric vezénylő alrendszer ehhez, amikor új csomópontokat ad hozzá tooa fürt.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-246">Service Fabric's orchestration engine can do this for you whenever new nodes are added tooa cluster.</span></span> <span data-ttu-id="e8e0f-247">Állapot nélküli szolgáltatás példányának létrehozásakor megadhatja a hello szám példányok toocreate szeretné.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-247">When you create instances of a stateless service, you can specify hello number of instances you want toocreate.</span></span> <span data-ttu-id="e8e0f-248">A Service Fabric helyezi a példányok száma hello fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-248">Service Fabric places that number of instances on nodes in hello cluster.</span></span> <span data-ttu-id="e8e0f-249">Biztosítja azt, és nem toocreate egynél több példány bármely egyik csomópontján.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-249">And it makes sure not toocreate more than one instance on any one node.</span></span> <span data-ttu-id="e8e0f-250">Azt is beállíthatja, hogy a Service Fabric tooalways hozzon létre egy példányát minden egyes csomópontjára megadásával **-1** hello példányok számát.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-250">You can also instruct Service Fabric tooalways create an instance on every node by specifying **-1** for hello instance count.</span></span> <span data-ttu-id="e8e0f-251">Ez biztosítja, hogy, hogy ki a fürt csomópontjai tooscale ad hozzá, amikor az állapot nélküli szolgáltatás egy példánya létrejön hello új csomópontján.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-251">This guarantees that whenever you add nodes tooscale out your cluster, an instance of your stateless service will be created on hello new nodes.</span></span> <span data-ttu-id="e8e0f-252">Ez az érték hello szolgáltatáspéldány, tulajdonsága, úgy van beállítva a példány létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="e8e0f-252">This value is a property of hello service instance, so it is set when you create a service instance.</span></span> <span data-ttu-id="e8e0f-253">Ez a Powershellen keresztül teheti meg:</span><span class="sxs-lookup"><span data-stu-id="e8e0f-253">You can do this through PowerShell:</span></span>

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

<span data-ttu-id="e8e0f-254">Is ehhez amikor állapotmentes szolgáltatások Visual Studio-projekt megadhat egy alapértelmezett szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="e8e0f-254">You can also do this when you define a default service in a Visual Studio stateless service project:</span></span>

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

<span data-ttu-id="e8e0f-255">További információt a hogyan toocreate alkalmazás és szolgáltatás példányainak: [alkalmazás üzembe helyezése](service-fabric-deploy-remove-applications.md).</span><span class="sxs-lookup"><span data-stu-id="e8e0f-255">For more information on how toocreate application and service instances, see [Deploy an application](service-fabric-deploy-remove-applications.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e8e0f-256">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e8e0f-256">Next steps</span></span>
[<span data-ttu-id="e8e0f-257">A Service Fabric-alkalmazás hibakeresése a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="e8e0f-257">Debug your Service Fabric application by using Visual Studio</span></span>](service-fabric-debugging-your-application.md)

