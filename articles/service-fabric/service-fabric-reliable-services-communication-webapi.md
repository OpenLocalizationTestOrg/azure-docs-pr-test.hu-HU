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
# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Első lépések: az OWIN futtató önálló Service Fabric Web API szolgáltatások
Az Azure Service Fabric hello power helyezi el a beavatkozás nélküli Amikor eldönti, hogy hogyan kívánja a szolgáltatások toocommunicate felhasználóival és egymással. Ez az oktatóanyag végzett szolgáltatások közötti kommunikáció ASP.NET Web API-t használja az Open Web Interface .NET (OWIN) önálló tárolásához a Service Fabric megbízható szolgáltatások API megvalósítására koncentrál. Azt fogja elmélyedhet mélyen hello Reliable Services moduláris kommunikációs API. Egy részletes példa tooshow a Web API is használjuk, hogyan tooset be egy egyéni kommunikációs figyelő.

## <a name="introduction-tooweb-api-in-service-fabric"></a>Bevezetés tooWeb API a Service Fabric
ASP.NET webes API-t egy népszerű és erőteljes környezet HTTP API-k hello .NET-keretrendszer felett. Ha még nem már ismeri a hello keretrendszer, lásd: [Ismerkedés az ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) további toolearn.

Webes API-t a Service Fabric rendszer hello azonos ASP.NET Web API megszokott és kedvelt. hello különbség a hogyan meg *állomás* egy webes API-alkalmazást. Nem fogja használni a Microsoft Internet Information Services (IIS). toobetter hello különbség ismertetése, most felosztása két részből áll:

1. hello webes API-alkalmazás (beleértve a tartományvezérlőket és -modellek)
2. hello állomás (hello webkiszolgáló, általában az IIS)

Egy magát webes API-alkalmazás nem változik. Elmúlt hello előfordulhat, hogy írt webes API-alkalmazások nem más, és el tudja toosimply áthelyezés alatt az alkalmazás kódjában a legtöbb. Ha Ön már lett üzemeltető IIS-kiszolgálón, ahol hello alkalmazás működteti, de miről használt kissé eltérő lehet. Előtt tartalmazó rész toohello beszerzéséhez azt kezdjük valami további ismerős: hello webes API-alkalmazás.

## <a name="create-hello-application"></a>Hello alkalmazás létrehozása
Először hozzon létre egy új Service Fabric-alkalmazás egy állapot nélküli szolgáltatásnak a Visual Studio 2015-öt.

A webes API-jával állapotmentes szolgáltatások Visual Studio sablonját elérhető tooyou. Ebben az oktatóanyagban azt, hogy milyen visszajelzést kap, ha a kiválasztott sablon eredménye teljesen új webes API a projekt fogja létrehozása.

Válasszon egy üres állapotmentes szolgáltatások projekt toolearn hogyan toobuild teljesen, vagy a Web API-projektet hello állapotmentes szolgáltatások Web API sablon kezdődhet és egyszerűen követéséhez.  

hello első lépés az egyes NuGet-csomagok webes API toopull. azt szeretnénk, ha toouse hello csomag Microsoft.AspNet.WebApi.OwinSelfHost. Ez a csomag tartalmazza az összes hello szükséges webes API-csomagok és hello *állomás* csomagok. Ez lesz fontos később.

Hello csomagok telepítése után megkezdheti az épület kimenő hello alapszintű webes API projektet struktúrát. Ha már használta a Web API, hello szerkezetének tisztában kell kinéznie. Először vegyen fel egy `Controllers` könyvtár és egy egyszerű érték tartományvezérlő:

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

A következő indítási osztály hozzáadása hello projekt legfelső szintű tooregister hello útválasztási is és egyéb konfigurációs beállítása. Ez egyben ahol Web API csatlakozik toohello *állomás*, amely fog kell javított változat újra később. 

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

Ez a hello alkalmazás rész. Ezen a ponton azt beállítása csak hello alapszintű webes API projektet elrendezés. Az eddigi azt nem szabad különbség hello korábbi lehet, hogy írt webes API-projektet, illetve hello egyszerű webes API-sablon. Az üzleti logikát a megszokott módon kerül hello, tartományvezérlői és modellek.

Most mi a teendő-k üzemeltetésére szolgál, hogy azt ténylegesen futtatható?

## <a name="service-hosting"></a>Üzemeltető szolgáltatáshoz
A Service Fabric, a szolgáltatás fut a *gazdafolyamat szolgáltatás*, egy végrehajtható fájl, amely a szolgáltatáskód hibáit futtatja. Írásakor szolgáltatás hello megbízható szolgáltatások API használatával, a projekt csak akkor tooan végrehajtható fájl, a szolgáltatás típusának regisztrálja, és futtatja a kódot. Ez igaz a legtöbb esetben a szolgáltatás a Service Fabric a .NET írásakor. Hello állapotmentes szolgáltatások projekt Program.cs megnyitásakor kell megjelennie:

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

Ha, amely a következőképpen néz gyanúsan hello belépési pont tooa Konzolalkalmazás, mert van.

Kapcsolatos részletes adatok hello szolgáltatás gazdafolyamat és szolgáltatás regisztrációs Ez a cikk hello terjed. A fontos tooknow, de most, hogy *a szolgáltatáskód hibáit külön folyamatban fut.*.

## <a name="self-host-web-api-with-an-owin-host"></a>Önálló gazdagép Web API egy OWIN állomás
Fényében, hogy a webes API-alkalmazás kódja egy külön folyamatban, hogyan tegye meg a számítógéphez, tooa webkiszolgáló? Adja meg [OWIN](http://owin.org/). OWIN: egyszerűen egy szerződés .NET webes alkalmazások és a webkiszolgálók között. Hagyományosan az ASP.NET (felfelé tooMVC 5) használatakor a hello webalkalmazás, szorosan összekapcsolt tooIIS System.Web keresztül. Azonban a Web API OWIN, megvalósítja, az azt üzemeltető webkiszolgáló hello írhat egy webes alkalmazás, amely le van. Ebből kifolyólag használhatja egy *önállóan üzemel* OWIN webkiszolgálón, amelyek a saját folyamat megkezdése. Ez tökéletesen hello Service Fabric üzemeltetési modell csak leírt illeszkedik.

Ebben a cikkben Katana használjuk hello OWIN gazdagépként hello webes API-alkalmazás számára. Katana egy nyílt forráskódú OWIN állomás implementáció épülő [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) és a hello Windows [HTTP-kiszolgáló API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

> [!NOTE]
> További információk Katana, nyissa meg toohello toolearn [Katana hely](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). A gyors áttekintést a toouse Katana tooself-állomás Web API, lásd: [használata OWIN tooSelf-állomás ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).
> 
> 

## <a name="set-up-hello-web-server"></a>Hello webkiszolgáló beállítása
hello megbízható szolgáltatások API csatlakoztatható, ahol a kommunikációs verem, amelyek lehetővé teszik a felhasználók és az ügyfelek tooconnect toohello szolgáltatás kommunikációs belépési pontot biztosít:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

hello web server (és más kommunikációs verem használhatja a későbbi, mint például a websocket elemek hello) kell használni, hello ICommunicationListener felület toointegrate megfelelően hello. hello okai az az alábbi lépésekkel hello több nyilvánvalóvá vált.

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

hello ICommunicationListener felületet biztosít a három módszer toomanage kommunikációs figyelő a szolgáltatás:

* *OpenAsync*. Indítsa el a kérések figyelését.
* *CloseAsync*. Állítsa le a kérések figyelését, Befejezés bármilyen az üzenetsoroktól kérelmeket és leállítása.
* *Megszakítás*. Minden megszakítja, és azonnal leállítja.

tooget indult el, adja hozzá a titkos osztálytagok, a dolgok hello figyelő toofunction kell. Ezek és felhasználandó hello konstruktor inicializálja, valamint később hello figyelő URL-cím beállításakor.

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
mint hello webkiszolgálót tooset, kétféle információt kell:

* *Egy URL-cím elérési út előtag*. Bár nem kötelező, továbbra is meg tooset ideális ez akár most, hogy biztonságosan tárolhatja, webes szolgáltatásokat az alkalmazásban.
* *A port*.

Mielőtt hello web Server port kap, fontos, hogy megértette, hogy a Service Fabric, amely az alkalmazás- és hello alapul szolgáló operációs rendszer, amely az fut közötti pufferként alkalmazás réteget biztosít. A Service Fabric tartalmaz egy módja tooconfigure *végpontok* a szolgáltatásokhoz. A Service Fabric biztosítja, hogy végpontok száma a szolgáltatás toouse érhető el. Ezzel a módszerrel nem kell tooconfigure őket az alapul szolgáló operációs rendszer környezet hello. A Service Fabric-alkalmazás különböző környezetekben könnyen tárolhatja, anélkül, hogy toomake módosítások tooyour alkalmazásokat. (Például tárolhatja, ugyanazt az alkalmazást az Azure-ban vagy a saját adatközpont hello.)

HTTP-végponttal PackageRoot\ServiceManifest.xml konfigurálása:

```xml
<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Ez a lépés fontos, mert hello szolgáltatás gazdafolyamat futtatja korlátozott credentials (a Windows hálózati szolgáltatás). Ez azt jelenti, hogy a szolgáltatás nem hozzáférés tooset be HTTP-végponttal rendelkezik önállóan. Hello végpont-konfiguráció használatával a Service Fabric tooset hello megfelelő hozzáférés-vezérlési lista (ACL) fel tudja hello hello szolgáltatás URL-címet figyelni fogja a. A Service Fabric is itt szabványos tooconfigure végpontok.

Vissza OwinCommunicationListener.cs, akkor kezdhet OpenAsync megvalósítása. Ez az az először hello webkiszolgáló. Először hello végpont információért, és hozzon létre hello szolgáltatás figyelni fogja hello URL-CÍMÉT. hello URL-címe eltérő, attól függően, hogy hello figyelő használatban van egy állapotmentes szolgáltatások vagy egy állapotalapú szolgáltatás lesz. Egy állapotalapú szolgáltatás hello figyelő kell egy egyedi címét minden állapotalapú szolgáltatási replika azt figyeli a toocreate. Az állapotmentes szolgáltatások hello cím jóval egyszerűbb lehet. 

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

Vegye figyelembe, hogy a "http://+" Itt nem használja. Ez a toomake meg arról, hogy az összes elérhető címek, beleértve a localhost, a teljes tartománynév és a hello gép IP-hello webkiszolgálóra figyeli.

hello OpenAsync a megvalósítása hello miért hello web server (vagy bármely kommunikációs verem) megvalósítása közvetlenül a nyílt egy ICommunicationListener, hanem csak az egyik legfontosabb okok valamelyike miatt `RunAsync()` hello szolgáltatásban. hello visszatérési érték a OpenAsync a webkiszolgáló hello hello címet figyeli. Ez a cím toohello rendszer ad vissza, ha regisztrálja azokat hello cím hello szolgáltatásban. A Service Fabric biztosít az API-k, amely lehetővé teszi az ügyfelek és egyéb szolgáltatások toothen feltenni ehhez a címhez szolgáltatás neve. Ez azért fontos, mert hello szolgáltatás cím nem statikus. Szolgáltatások hello fürt erőforrás és a rendelkezésre állási célokra helyezi át. Ez az hello mechanizmus, amely lehetővé teszi az ügyfelek tooresolve hello cím egy szolgáltatás figyeli.

Vele szem előtt OpenAsync hello webkiszolgáló elindul, és adja vissza, amely figyeli a hello cím. Vegye figyelembe, hogy a "http://+" figyeli, de OpenAsync hello címét adja meg, mielőtt hello "+" hello IP vagy FQDN-jét hello csomópont jelenleg a rendszer lecseréli. Mi hello rendszer regisztrálva van-e metódus által visszaadott hello cím tartozik. Akkor is ügyfelek és egyéb szolgáltatások látják, ha a szolgáltatás címét, hogy kérni. Az ügyfelek toocorrectly csatlakozni a tooit, egy tényleges IP vagy FQDN Formátumban kell hello címét.

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

Vegye figyelembe, hogy ez hivatkozik, amely toohello OwinCommunicationListener hello konstruktorban átadott hello indítási osztály. Ez a példány indítási hello web server toobootstrap hello webes API-alkalmazás használja.

Hello `ServiceEventSource.Current.Message()` sor hello diagnosztikai események ablakban jelenik később futtatásakor hello alkalmazás tooconfirm hello webkiszolgálóra sikeresen elindult.

## <a name="implement-closeasync-and-abort"></a>Alkalmazzon CloseAsync és Abort
Végezetül megvalósítása CloseAsync és a megszakítási toostop hello webkiszolgáló. hello webkiszolgáló hello server leíró OpenAsync során létrehozott ártalmatlanítása által állítható le.

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

A megvalósítás példában CloseAsync és a megszakítási egyszerűen állítsa le hello webkiszolgáló. Úgy is dönthet, tooperform hello webkiszolgálón CloseAsync több szabályosan koordinált leállítására. Például hello leállítási sikerült Várjon, amíg az üzenetsoroktól kérelmek toobe előtt hello vissza.

## <a name="start-hello-web-server"></a>Indítsa el a hello webkiszolgáló
Most már készen áll a toocreate, és térjen vissza a OwinCommunicationListener toostart hello web server-példányt. Vissza a hello szolgáltatás osztály (WebService.cs), bírálja felül a hello `CreateServiceInstanceListeners()` módszert:

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

Amennyiben ez van hello Web API *alkalmazás* és hello OWIN *állomás* végül felel meg. hello állomás (OwinCommunicationListener) kap hello példányának *alkalmazás* (webes API) keresztül hello indítási osztályt. A Service Fabric majd annak életciklusát felügyeli. Ebben a mintában általában a bármely kommunikációs verem kell követni.

## <a name="put-it-all-together"></a>Az alkalmazás összeállítása
Ebben a példában a semmi a hello toodo nem kell `RunAsync()` metódust, így az adott felülbírálás egyszerűen távolítható el.

hello végső szolgáltatás megvalósítása nagyon egyszerű kell lennie. Csak toocreate hello kommunikációs figyelő van szüksége:

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

teljes hello `OwinCommunicationListener` osztály:

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

Most, hogy Ön vezettek összes hello kódrészletek, a projekt egy tipikus webes API-alkalmazás megbízható szolgáltatások API belépési pontok és egy OWIN állomás kell hasonlítania.

## <a name="run-and-connect-through-a-web-browser"></a>Futtassa, és csatlakozzon egy webböngészőn keresztül
Ha még nem tette, [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md).

Most hozza létre és telepítse a szolgáltatást. Nyomja le az **F5** a Visual Studio toobuild hello alkalmazás és központi telepítését. Hello diagnosztikai események ablakában kell megjelennie a http://localhost:8281 megnyitott hello webkiszolgálóra jelző üzenet /.

> [!NOTE]
> Ha hello port a számítógépen egy másik folyamat már meg van nyitva, itt hiba jelenhet meg. Ez azt jelzi, hogy hello figyelő nem nyitható meg. Ha hello esetben próbálkozzon egy másik portot használ a ServiceManifest.xml hello végpont-konfiguráció.
> 
> 

Miután hello szolgáltatás fut, nyisson meg egy böngészőt, és keresse meg a túl[http://localhost:8281/api/értékek](http://localhost:8281/api/values) tootest azt.

## <a name="scale-it-out"></a>Méretezhető végrehajtását
Állapot nélküli webalkalmazások kiterjesztése általában azt jelenti, hogy további gépek hozzáadása és hello webalkalmazások rajtuk dolgozik. A Service Fabric vezénylő alrendszer ehhez, amikor új csomópontokat ad hozzá tooa fürt. Állapot nélküli szolgáltatás példányának létrehozásakor megadhatja a hello szám példányok toocreate szeretné. A Service Fabric helyezi a példányok száma hello fürt csomópontja. Biztosítja azt, és nem toocreate egynél több példány bármely egyik csomópontján. Azt is beállíthatja, hogy a Service Fabric tooalways hozzon létre egy példányát minden egyes csomópontjára megadásával **-1** hello példányok számát. Ez biztosítja, hogy, hogy ki a fürt csomópontjai tooscale ad hozzá, amikor az állapot nélküli szolgáltatás egy példánya létrejön hello új csomópontján. Ez az érték hello szolgáltatáspéldány, tulajdonsága, úgy van beállítva a példány létrehozásakor. Ez a Powershellen keresztül teheti meg:

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

További információt a hogyan toocreate alkalmazás és szolgáltatás példányainak: [alkalmazás üzembe helyezése](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Következő lépések
[A Service Fabric-alkalmazás hibakeresése a Visual Studio használatával](service-fabric-debugging-your-application.md)

