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
# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Első lépések: az OWIN futtató önálló Service Fabric Web API szolgáltatások
Az Azure Service Fabric helyezi a power a beavatkozás nélküli, amikor eldönti, hogyan szeretné a szolgáltatásokat, hogy és egymással kommunikáló felhasználók. Ez az oktatóanyag végzett szolgáltatások közötti kommunikáció ASP.NET Web API-t használja az Open Web Interface .NET (OWIN) önálló tárolásához a Service Fabric megbízható szolgáltatások API megvalósítására koncentrál. Azt fogja elmélyedhet mélyen a Reliable Services moduláris kommunikációs API. Használjuk Web API is részletes példa mutatjuk be, hogyan állíthat be egy egyéni kommunikációs figyelő.

## <a name="introduction-to-web-api-in-service-fabric"></a>Bevezetés a Service Fabric a webes API-hoz
ASP.NET webes API-t egy népszerű és erőteljes környezet HTTP API-k létrehozásához a .NET-keretrendszer. Ha még nem már ismeri a keretében, lásd: [Ismerkedés az ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) további.

Webes API-t a Service Fabric az azonos ASP.NET Web API megszokott és kedvelt. A különbség a hogyan meg *állomás* egy webes API-alkalmazást. Nem fogja használni a Microsoft Internet Information Services (IIS). Jobb megértése érdekében a különbség, most felosztása, két részből áll:

1. A webes API-alkalmazás (beleértve a tartományvezérlőket és -modellek)
2. A gazdagép (a webkiszolgáló, általában az IIS)

Egy magát webes API-alkalmazás nem változik. Helyzetet teremt, mintha a webes API-alkalmazások írt kap a múltban, és meg kell az alkalmazás kódjában többsége egyszerűen átvitele. Azonban ha azt korábban futtató IIS-kiszolgálón, ahol az alkalmazás állomás kissé eltér az éppen használt lehet. Után azt beolvasni a birtokosi részére, kezdjük valami további ismerős: a webes API-alkalmazás.

## <a name="create-the-application"></a>Az alkalmazás létrehozása
Először hozzon létre egy új Service Fabric-alkalmazás egy állapot nélküli szolgáltatásnak a Visual Studio 2015-öt.

A webes API-jával állapotmentes szolgáltatások Visual Studio sablonját is elérhetők. Ebben az oktatóanyagban azt, hogy milyen visszajelzést kap, ha a kiválasztott sablon eredménye teljesen új webes API a projekt fogja létrehozása.

Válasszon ki egy üres állapotmentes szolgáltatások projektet megtudhatja, hogyan hozhat létre egy teljesen új webes API-projektet, vagy indítsa el az állapotmentes szolgáltatások webes API-sablonhoz, és egyszerűen követéséhez.  

Az első lépés néhány NuGet-csomagok a webes API le tudja. A csomag használandó szeretnénk Microsoft.AspNet.WebApi.OwinSelfHost. Ez a csomag tartalmazza a szükséges webes API-csomagok és a *állomás* csomagok. Ez lesz fontos később.

A csomagok telepítése után megkezdheti a webes API-projekt alapszintű struktúrát létrehozására. Ha már használta a webes API-t, a projekt struktúra tisztában kell kinéznie. Először vegyen fel egy `Controllers` könyvtár és egy egyszerű érték tartományvezérlő:

**ValuesController.cs**

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

A következő indítási osztály hozzáadása a projekt legfelső szintű regisztrálni az útvonal is és bármely egyéb konfigurációs beállítása. Ez egyben ahol Web API csatlakoztatja az való a *állomás*, amely fog kell javított változat újra később. 

**Startup.cs**

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

Ez az alkalmazás részére. Ezen a ponton létrehoztunk hogy csak az alapszintű webes API projektet elrendezés. Eddig akkor nem jelenik meg sokkal írt kap a múltban Web API-projektet, illetve az egyszerű webes API-sablon Az üzleti logikát a megszokott módon végrehajtja a tartományvezérlők és modellek.

Most mi a teendő-k üzemeltetésére szolgál, hogy azt ténylegesen futtatható?

## <a name="service-hosting"></a>Üzemeltető szolgáltatáshoz
A Service Fabric, a szolgáltatás fut a *gazdafolyamat szolgáltatás*, egy végrehajtható fájl, amely a szolgáltatáskód hibáit futtatja. A megbízható szolgáltatások API-jával írásakor szolgáltatás szolgáltatásprojektre csak egy végrehajtható fájl, amely a szolgáltatás típusának regisztrálja, és a kód futtatása lefordítja. Ez igaz a legtöbb esetben a szolgáltatás a Service Fabric a .NET írásakor. Az állapotmentes szolgáltatások projekt Program.cs megnyitásakor kell megjelennie:

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

Ha, amely a következőképpen néz gyanúsan a belépési pont számára egy konzolalkalmazást, mert van.

A szolgáltatás gazdafolyamat és a szolgáltatás regisztrációs kapcsolatos részletes adatok nem térnek Ez a cikk. De fontos tudni, hogy a most, hogy *a szolgáltatáskód hibáit külön folyamatban fut.*.

## <a name="self-host-web-api-with-an-owin-host"></a>Önálló gazdagép Web API egy OWIN állomás
Figyelembe véve, hogy a webes API-alkalmazás kódja egy külön folyamatban, hogyan tegye meg kapcsolja legfeljebb egy webkiszolgáló? Adja meg [OWIN](http://owin.org/). OWIN: egyszerűen egy szerződés .NET webes alkalmazások és a webkiszolgálók között. Hagyományosan az ASP.NET (max. MVC 5) használata esetén a webes alkalmazás szorosan csatlakoztatott IIS System.Web keresztül. Azonban a Web API OWIN, megvalósítja, az azt üzemeltető webkiszolgáló írhat egy webes alkalmazás, amely le van. Ebből kifolyólag használhatja egy *önállóan üzemel* OWIN webkiszolgálón, amelyek a saját folyamat megkezdése. Ez tökéletesen csak ismerteti a Service Fabric üzemeltetési modell illeszkedik.

Ebben a cikkben Katana használjuk az OWIN gazdagépként a webes API-alkalmazáshoz. Katana egy nyílt forráskódú OWIN állomás implementáció épülő [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) és a Windows [HTTP-kiszolgáló API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

> [!NOTE]
> Katana kapcsolatos további tudnivalókért keresse fel a [Katana hely](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). Önálló gazdagép Web API Katana használatával történő gyors áttekintést, lásd: [használja OWIN Self-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).
> 
> 

## <a name="set-up-the-web-server"></a>A webalkalmazás-kiszolgáló beállítása
A megbízható szolgáltatások API biztosít egy kommunikációs belépési pontot, amelyen csatlakoztathatja a kommunikációs verem, amelyek lehetővé teszik a felhasználók és az ügyfelek kapcsolódni a szolgáltatáshoz:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

A webalkalmazás-kiszolgáló (és más kommunikációs verem például websocket elemek használata a jövőben,) kell ICommunicationListener felületet használ megfelelően integrálása a rendszer. Ennek az oka a következő lépésekben több nyilvánvaló lesz.

Először hozzon létre egy osztályt, amely megvalósítja az ICommunicationListener OwinCommunicationListener:

**OwinCommunicationListener.cs**

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

A ICommunicationListener felület kezelheti a szolgáltatás kommunikációs figyelő három módszert kínál:

* *OpenAsync*. Indítsa el a kérések figyelését.
* *CloseAsync*. Állítsa le a kérések figyelését, Befejezés bármilyen az üzenetsoroktól kérelmeket és leállítása.
* *Megszakítás*. Minden megszakítja, és azonnal leállítja.

Első lépésként adja hozzá a titkos osztálytagjaihoz dolgot a figyelő működéséhez kell. Ezek és felhasználandó inicializálja a konstruktor, valamint később a figyelő URL-cím beállításakor.

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

## <a name="implement-openasync"></a>OpenAsync megvalósítása
A webkiszolgáló beállításához két adatra van szükség:

* *Egy URL-cím elérési út előtag*. Bár ez nem kötelező, ahhoz, hogy beállítaniuk ezt most, hogy biztonságosan jó gazdagép több webszolgáltatásból az alkalmazásban.
* *A port*.

Mielőtt port lekérése a webkiszolgálón, fontos, hogy megértette, hogy a Service Fabric, amely az alkalmazás és az általa futtatott operációs rendszer közötti pufferként alkalmazás réteget biztosít. Konfigurálása módszert kínál, a Service Fabric *végpontok* a szolgáltatásokhoz. A Service Fabric biztosítja, hogy végpontok érhető el a szolgáltatást. Ezzel a módszerrel nem kell konfigurálásuk saját magának az alapul szolgáló operációs rendszer környezetében. A Service Fabric-alkalmazás különböző környezetekben könnyen tárolhatja, anélkül, hogy ne módosítsa az alkalmazás számára. (Például tárolhatja, ugyanazt az alkalmazást az Azure-ban vagy a saját adatközpont.)

HTTP-végponttal PackageRoot\ServiceManifest.xml konfigurálása:

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Ez a lépés fontos, mert a gazdagép-folyamat futtatja korlátozott credentials (a Windows hálózati szolgáltatás). Ez azt jelenti, hogy a szolgáltatás nem állíthat be egy HTTP-végpont a saját hozzáférést. A végpont-konfiguráció használatával a Service Fabric tudni fogja, hogy a megfelelő hozzáférés-vezérlési lista (ACL) beállítása az URL-címhez, amely a szolgáltatás figyelni fogja. A Service Fabric is itt szabványos végpontok beállítása.

Vissza OwinCommunicationListener.cs, akkor kezdhet OpenAsync megvalósítása. Ez az az először a webkiszolgálón. Először a végpont-információkat lekérni, és az URL-címet, a szolgáltatás figyelni fogja létrehozni. Az URL-cím attól függően, hogy a figyelő használatban van egy állapotmentes szolgáltatások vagy egy állapotalapú szolgáltatás eltérőek lesznek. A figyelő egy állapotalapú szolgáltatás kell minden állapotalapú szolgáltatási replika elkezdi figyelni a egyedi cím létrehozása. Az állapotmentes szolgáltatások a cím jóval egyszerűbb lehet. 

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

Vegye figyelembe, hogy a "http://+" Itt nem használja. Ez a, győződjön meg arról, hogy a webkiszolgáló összes elérhető címek, beleértve a localhost, teljes tartománynév és a gép IP-figyeli.

OpenAsync végrehajtására a legfontosabb okai miért a webalkalmazás-kiszolgáló (vagy bármely kommunikációs verem) megvalósítása egy ICommunicationListener, hanem csak azt közvetlenül a nyílt `RunAsync()` a szolgáltatásban. A visszatérési érték a OpenAsync a címet, hogy a webkiszolgáló nem. Ezt a címet a rendszer küld vissza, ha regisztrálja azokat a cím a szolgáltatással. A Service Fabric biztosít az API-k, amely lehetővé teszi az ügyfelek és egyéb szolgáltatások majd feltenni ehhez a címhez szolgáltatás neve. Ez azért fontos, mert a szolgáltatás-cím nem statikus. Szolgáltatások helyezi át a fürt erőforrás és a rendelkezésre állás céljából. Azt a mechanizmust, amely lehetővé teszi az ügyfelek fel tudják oldani a figyelő cím egy szolgáltatás számára.

Vele szem előtt a OpenAsync a webkiszolgáló elindul, és visszahelyezi a címet, amely a figyeléshez. Vegye figyelembe, hogy elkezdi figyelni a "http://+", de még mielőtt OpenAsync visszahelyezi a címet, a "+" cseréli az IP- vagy a jelenleg a csomópont teljesen minősített Tartománynevét. Ez a metódus által visszaadott cím esetén milyen regisztrálva van a rendszer. Akkor is ügyfelek és egyéb szolgáltatások látják, ha a szolgáltatás címét, hogy kérni. Az ügyfelek számára, hogy megfelelően csatlakozik azok kell egy tényleges IP vagy FQDN Formátumban a címben.

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

Vegye figyelembe, hogy ez a konstruktor OwinCommunicationListener továbbított indítási osztályra hivatkozik. Ez a példány indítási szolgál a webkiszolgáló bootstrap a webes API-alkalmazás.

A `ServiceEventSource.Current.Message()` sor megjelennek a diagnosztikai események ablak később, győződjön meg arról, hogy a webalkalmazás-kiszolgáló sikeresen elindult-e az alkalmazás futtatásakor.

## <a name="implement-closeasync-and-abort"></a>Alkalmazzon CloseAsync és Abort
Végezetül megvalósítása CloseAsync és a megszakítási leállítja a webkiszolgálón. A webkiszolgáló a kiszolgáló leíró OpenAsync során létrehozott ártalmatlanítása által állítható le.

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

A megvalósítás példában CloseAsync és a megszakítási egyszerűen állítsa le a webkiszolgálón. Úgy is dönthet, hajtsa végre a webkiszolgáló több szabályosan koordinált leállítási CloseAsync. Például a leállítási sikerült Várjon, amíg az üzenetsoroktól kérelem végrehajtása előtt a visszatérési.

## <a name="start-the-web-server"></a>Indítsa el a webkiszolgálón
Most már készen áll létrehozásához, és térjen vissza a webkiszolgáló elindításához OwinCommunicationListener példánya. Vissza a szolgáltatásosztály (WebService.cs), bírálja felül a `CreateServiceInstanceListeners()` módszert:

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

Ez akkor, ha a webes API *alkalmazás* és az OWIN *állomás* végül felel meg. A gazdagép (OwinCommunicationListener) kap egy példányát a *alkalmazás* (webes API) keresztül az indítási osztályt. A Service Fabric majd annak életciklusát felügyeli. Ebben a mintában általában a bármely kommunikációs verem kell követni.

## <a name="put-it-all-together"></a>Az alkalmazás összeállítása
Ebben a példában, nem kell semmit a `RunAsync()` metódust, így az adott felülbírálás egyszerűen távolítható el.

Kell, hogy a végső szolgáltatás megvalósítása nagyon egyszerű. Csak a kommunikációs figyelő létrehozásához szükséges:

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

A teljes `OwinCommunicationListener` osztály:

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

Most, hogy Ön vezettek minden a helyére, a projekt egy tipikus webes API-alkalmazás megbízható szolgáltatások API belépési pontok és egy OWIN állomás kell hasonlítania.

## <a name="run-and-connect-through-a-web-browser"></a>Futtassa, és csatlakozzon egy webböngészőn keresztül
Ha még nem tette, [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md).

Most hozza létre és telepítse a szolgáltatást. Nyomja le az **F5** felépítéséhez és az alkalmazás központi telepítése a Visual Studio. A diagnosztikai ablakban kell megjelennie, amely jelzi, hogy a webkiszolgáló megnyitott http://localhost:8281 üzenet /.

> [!NOTE]
> Ha a port már nyitva van a számítógépen egy másik folyamat, itt hiba jelenhet meg. Ez azt jelzi, hogy a figyelő nem nyitható meg. Ha ez a helyzet, próbálkozzon egy másik portot használ a végpont-konfiguráció a ServiceManifest.xml.
> 
> 

Ha a szolgáltatás fut, nyisson meg egy böngészőt, és keresse meg [http://localhost:8281/api/értékek](http://localhost:8281/api/values) tesztek.

## <a name="scale-it-out"></a>Méretezhető végrehajtását
Állapot nélküli webalkalmazások kiterjesztése általában azt jelenti, hogy további gépek hozzáadása és a webalkalmazások rajtuk dolgozik. A Service Fabric vezénylő alrendszer ehhez, amikor új csomópontokat ad hozzá egy fürt. Állapot nélküli szolgáltatás példányának létrehozásakor megadhatja a létrehozandó példányok száma. A Service Fabric a példányok száma a fürtben lévő csomópontok helyezi. És biztosítja, hogy nem hozható létre egynél több példány bármely csomópontjára. Azt is beállíthatja, hogy a Service Fabric mindig hozzon létre egy példányát minden egyes csomópontjára megadásával **-1** a példányok számát. Ez biztosítja, hogy, hogy terjessze ki a fürt a csomópontok hozzáadásakor az állapot nélküli szolgáltatás egy példánya létrejön az új csomópontján. Ez az érték a szolgáltatáspéldány tulajdonsága, úgy van beállítva a példány létrehozásakor. Ez a Powershellen keresztül teheti meg:

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

Is ehhez amikor állapotmentes szolgáltatások Visual Studio-projekt megadhat egy alapértelmezett szolgáltatás:

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

Alkalmazás és szolgáltatás példányainak létrehozásával kapcsolatos további információkért lásd: [alkalmazás üzembe helyezése](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Következő lépések
[A Service Fabric-alkalmazás hibakeresése a Visual Studio használatával](service-fabric-debugging-your-application.md)

