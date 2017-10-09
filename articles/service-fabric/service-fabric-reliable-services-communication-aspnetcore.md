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
# <a name="aspnet-core-in-service-fabric-reliable-services"></a>Az ASP.NET Core a Service Fabric megbízható szolgáltatások

Az ASP.NET Core egy új nyílt forráskódú és platformfüggetlen keretrendszer modern felhőalapú internetre kapcsolódó alkalmazások, például webalkalmazások, az IoT-alkalmazásokhoz és a mobil háttérkiszolgálókon. 

Ez a cikk egy részletes útmutató toohosting a Service Fabric Reliable Services hello segítségével szolgáltatásai az ASP.NET Core **Microsoft.ServiceFabric.AspNetCore.** * NuGet-csomagok készlete.

Egy bevezető oktatóanyag az ASP.NET Core a Service Fabric és a fejlesztési környezet beállítása történt utasításokat lásd: [egy webes előtér-, az ASP.NET Core segítségével alkalmazás felépítése](service-fabric-add-a-web-frontend.md).

Ez a cikk többi hello azt feltételezi, hogy ismeri az ASP.NET Core. Ha nem, azt javasoljuk egész hello [ASP.NET Core – alapok](https://docs.microsoft.com/aspnet/core/fundamentals/index).

## <a name="aspnet-core-in-hello-service-fabric-environment"></a>Az ASP.NET Core hello Service Fabric-környezetben

Miközben az ASP.NET Core alkalmazásokat is .NET Core-on vagy teljes .NET-keretrendszer, a Service Fabric szolgáltatás jelenleg csak futtathatja hello hello teljes .NET-keretrendszer. Ez azt jelenti, hogy az ASP.NET Core Service Fabric-szolgáltatás építésekor, kell továbbra is céloznia hello teljes .NET-keretrendszer.

Az ASP.NET Core a Service Fabric két különböző módon használható:
 - **És a Vendég végrehajtható fájlja üzemeltetett**. Ez a elsősorban használt toorun meglévő ASP.NET Core alkalmazások a Service Fabric code változtatás nélkül.
 - **A megbízható szolgáltatás futhat**. Ez lehetővé teszi, hogy a Service Fabric-futtatókörnyezet hello jobb integrációt, és lehetővé teszi, hogy a állapot-nyilvántartó ASP.NET Core services.

hello rest Ez a cikk azt ismerteti, hogyan belül egy megbízható szolgáltatás használja az ASP.NET Core toouse hello-ASP.NET Core integrációs összetevőket, amelyeket a Service Fabric SDK hello. 

## <a name="service-fabric-service-hosting"></a>A Service Fabric-szolgáltatás üzemeltetéséhez

A Service Fabric egy vagy több példány, illetve a replikákat a szolgáltatás futtatásához egy *gazdafolyamat szolgáltatás*, egy végrehajtható fájl, amely a szolgáltatáskód hibáit futtatja. A szolgáltatás szerző saját hello szolgáltatás gazdafolyamat és a Service Fabric aktiválódik, és figyeli a meg.

Hagyományos ASP.NET (felfelé tooMVC 5) keresztül System.Web.dll szorosan összekapcsolt tooIIS. Az ASP.NET Core hello webkiszolgáló és a webes alkalmazás közötti kizárás biztosít. Így webes alkalmazások toobe hordozható különböző kiszolgálók között, és lehetővé teszi webes kiszolgálók toobe *önállóan üzemel*, ami azt jelenti, hogy elindul-e a webkiszolgáló a saját folyamat dedikált tulajdonában lévő megakadályozását tooa folyamatként például az IIS Server szoftver. 

Rendelés toocombine a Service Fabric-szolgáltatás és az ASP.NET, a Vendég végrehajtható fájl vagy egy megbízható szolgáltatásban a képes toostart ASP.NET a szolgáltatás gazdafolyamat belül kell lennie. Az ASP.NET Core önálló üzemeltető toodo lehetővé teszi ezt.

## <a name="hosting-aspnet-core-in-a-reliable-service"></a>Az ASP.NET Core működnie a tároló
Önálló üzemeltetett ASP.NET Core alkalmazások általában egy WebHost létrehozása egy alkalmazás belépési pont, például a hello `static void Main()` metódus a `Program.cs`. Ebben az esetben a hello WebHost hello életciklusát kötött toohello életciklus hello folyamat.

![Az ASP.NET Core üzemeltető folyamatban][0]

Azonban hello alkalmazás belépési pont nincs hello megfelelő helyen toocreate a Webállomás megbízható szolgáltatásban, mert hello alkalmazás belépési pont csak akkor használatos tooregister szolgáltatás írja be a hello Service Fabric-futtatókörnyezet, az, hogy az adott szolgáltatás példányának létrehozhat Adja meg. a Webállomás hello létre kell hozni egy megbízható szolgáltatás magát. Hello szolgáltatás gazdafolyamaton belül szolgáltatáspéldány és/vagy a replikák Lépkedjen végig több életciklusának. 

A megbízható példánya a származó osztály által képviselt `StatelessService` vagy `StatefulService`. hello kommunikációs verem szolgáltatás szerepel egy `ICommunicationListener` végrehajtása a szolgáltatás osztályban. Hello `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet-csomagok tartalmaznak implementációja `ICommunicationListener` , indítsa el, és kezelheti az ASP.NET Core WebHost hello vércse vagy a megbízható szolgáltatás WebListener.

![Az ASP.NET Core működnie a tároló][1]

## <a name="aspnet-core-icommunicationlisteners"></a>Az ASP.NET Core ICommunicationListeners
Hello `ICommunicationListener` megvalósításait vércse és WebListener a hello `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet-csomagok hasonló felhasználási minták rendelkezik, de az adott tooeach webkiszolgáló némileg eltérő műveleteket végrehajtani. 

Mindkét kommunikációs figyelőket adja meg a következő argumentumok hello konstruktort:
 - **`ServiceContext serviceContext`**: hello `ServiceContext` hello szolgáltatást futtató kapcsolatos információkat tartalmazó objektum.
 - **`string endpointName`**: hello nevét egy `Endpoint` ServiceManifest.xml konfiguráció. Ez az elsősorban ahol hello két kommunikációs figyelőket eltérőek: WebListener **szükséges** egy `Endpoint` konfigurációs, vércse azonban nem.
 - **`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: egy lambda, amely hoz létre, és visszatérési megvalósító egy `IWebHost`. Ez lehetővé teszi, hogy tooconfigure `IWebHost` hello módja az ASP.NET Core alkalmazásban normális esetben tenné. hello lambda biztosít egy URL-címet, amely jön létre, attól függően, hogy a Service Fabric-integráció hello-lehetőségek, használata és hello `Endpoint` konfigurációs megadnia. URL-cím majd kell módosítani vagy vettük-toostart hello webkiszolgáló.

## <a name="service-fabric-integration-middleware"></a>A Service Fabric-integráció köztes
Hello `Microsoft.ServiceFabric.Services.AspNetCore` NuGet-csomag magában foglalja a hello `UseServiceFabricIntegration` bővítmény metódusa `IWebHostBuilder` , amely a Service Fabric-kompatibilis köztes ad. A köztes konfigurálja hello vércse vagy WebListener `ICommunicationListener` tooregister egy egyedi URL-címe a Service Fabric-szolgáltatás hello és szerint hitelesíti ügyfél kérelmek tooensure ügyfelek toohello jobb szolgáltatás csatlakozik. Erre akkor szükség a megosztott gazdagép-környezetben, például a Service Fabric, ahol több webalkalmazás hello futtathatja ugyanazon fizikai vagy virtuális gép, de ne használjon egyedi állomásnevek, tooprevent ügyfelek véletlenül csatlakozzon toohello megfelelő szolgáltatást. Ebben a forgatókönyvben hello a következő szakaszban részletesen ismertetett.

### <a name="a-case-of-mistaken-identity"></a>A gyorsítás esetében is téves identitás
Szolgáltatás replikákat protocol, függetlenül egyedi IP:port kombinációjából figyelése. A szolgáltatás replika megkezdése IP:port a végpont figyel, után a végpont címét toohello Service Fabric-szolgáltatás ahol, könnyen megtalálhatók legyenek az ügyfelek vagy egyéb szolgáltatások jelenti. Ha a szolgáltatások használata dinamikusan hozzárendelt alkalmazás-porttal rendelkezik, a szolgáltatás replika helyezi használhat, amely korábban egy másik szolgáltatás ugyanazon IP:port végpont hello hello ugyanazon fizikai vagy virtuális gép. Ez is okozhatja, hogy egy ügyfél toomistakely csatlakozás toohello megfelelő szolgáltatást. Ez akkor fordulhat elő, ha a következő eseményekre hello történik:

 1. A szolgáltatás figyeli 10.0.0.1:30000 HTTP Protokollon keresztül. 
 2. Ügyfél szolgáltatás A feloldását, és lekérdezi a cím 10.0.0.1:30000
 3. A szolgáltatás egy lépés tooa másik csomópont.
 4. B szolgáltatás el van helyezve 10.0.0.1 és helyezi a használja ugyanazt a portot 30000 hello.
 5. -Ügyfél megkísérel tooconnect tooservice A gyorsítótárazott cím 10.0.0.1:30000 együtt.
 6. Ügyfél már csatlakozik-e sikeresen csatlakoztatott tooservice nem megvalósító B toohello megfelelő szolgáltatást.

Ez azt okozhatja, hogy a hibákat, amelyek lehetnek nehéz toodiagnose véletlenszerű időpontokban. 

### <a name="using-unique-service-urls"></a>Egyedi URL-címek használata
tooprevent, szolgáltatások is egy végpont toohello Naming Service egyedi azonosítóval rendelkező közzétételére, és ezután ellenőrizze, hogy egyedi azonosítója ügyfélkérelmek során. Ez a egy együttműködési művelet megbízható környezet nem rosszindulatú-bérlői szolgáltatások között. Ez nem biztosítja a biztonságos szolgáltatás hitelesítési rosszindulatú bérlői környezetben.

A megbízható környezet esetén hello hello által hozzáadott köztes `UseServiceFabricIntegration` metódus automatikusan hozzáfűzi egy egyedi azonosítót toohello címet, amely toohello Naming Service visszaküldi, és ellenőrzi, hogy minden kérelemnél azonosítója. Ha hello azonosítója nem egyezik, akkor a hello köztes azonnal az egy HTTP 410 Megszűnt választ ad vissza.

A köztes felhasználása ellenőrizniük kell egy dinamikusan hozzárendelt portot használó szolgáltatások.

Egy rögzített egyedi portot használó szolgáltatások együttműködési környezetben nincs probléma. Egy rögzített egyedi port általában szolgál, amely egy jól ismert port szükséges az ügyfél alkalmazások tooconnect kívülről elérhető szolgáltatások. A legtöbb Internet felé néző webes alkalmazások például használja a 80-as vagy 443-as port webes böngésző kapcsolatokhoz. Ebben az esetben hello egyedi azonosítója nem engedélyezni kell.

hello alábbi ábrán látható hello áramlanak a hello köztes engedélyezve:

![Service Fabric ASP.NET Core integráció][2]

Vércse és a WebListener `ICommunicationListener` megvalósítások a mechanizmus használata pontosan hello azonos módon. Bár WebListener belső megkülönböztetni azokat a kérelmeket az alapul szolgáló hello segítségével egyedi URL-cím útvonalakon alapján *http.sys* port megosztása szolgáltatás, a funkció *nem* hello WebListener általhasznált`ICommunicationListener` végrehajtása, mert a korábban ismertetett hello forgatókönyvben HTTP 503-as és a HTTP 404-es hiba állapotkódok vezethet. Amely pedig rendkívül megnehezíti az ügyfelek toodetermine hello szándékával hello hiba, mert HTTP 503-as és a HTTP 404-es már gyakran használt tooindicate más hibák. Emiatt vércse és WebListener `ICommunicationListener` megvalósítások szabványosítására a hello által biztosított hello köztes `UseServiceFabricIntegration` kiterjesztésmetódus, hogy az ügyfelek csak kell tooperform szolgáltatásvégpont újra feloldani a HTTP 410 válaszok művelet.

## <a name="weblistener-in-reliable-services"></a>A megbízható szolgáltatások WebListener
WebListener használható egy megbízható szolgáltatás hello importálásával **Microsoft.ServiceFabric.AspNetCore.WebListener** NuGet-csomagot. Ez a csomag tartalmaz `WebListenerCommunicationListener`, megvalósítása `ICommunicationListener`, ami lehetővé teszi az ASP.NET Core WebHost belül egy megbízható szolgáltatás toocreate hello webkiszolgálóval WebListener használatával.

WebListener épül hello [Windows HTTP-kiszolgáló API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx). Ez a hello használja *http.sys* kernel-illesztőprogram használják az IIS tooprocess HTTP-kérelmek és irányíthatja őket tooprocesses, amelyeken webalkalmazások futnak. Ez lehetővé teszi több folyamatot hello ugyanazon fizikai vagy virtuális gép toohost webes alkalmazások hello ugyanazt a portot, egyedi URL-cím vagy állomásnév használatát. Ezek a funkciók hasznosak a Service Fabric hello lévő több webhely üzemeltetésére ugyanabban a fürtben.

hello következő diagram azt ábrázolja, hogyan használja a WebListener a hello *http.sys* kernel illesztőprogramját a Windows-portmegosztás:

![a HTTP.sys][3]

### <a name="weblistener-in-a-stateless-service"></a>Az állapotmentes szolgáltatások WebListener
toouse `WebListener` állapotmentes szolgáltatások, a felülbírálás hello `CreateServiceInstanceListeners` metódust, és térjen vissza a `WebListenerCommunicationListener` példány:

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

### <a name="weblistener-in-a-stateful-service"></a>Az állapotalapú service WebListener

`WebListenerCommunicationListener`jelenleg nincs használatra szolgál az állapotalapú szolgáltatások rendelkező hello alapjául szolgáló esedékes toocomplications *http.sys* port megosztása szolgáltatást. További információkért tekintse meg a következő szakasz a dinamikus port lefoglalását WebListener hello. Állapotalapú szolgáltatások esetén vércse a webkiszolgáló ajánlott hello.

### <a name="endpoint-configuration"></a>Végpont-konfiguráció

Egy `Endpoint` konfigurálni kell a Windows HTTP kiszolgáló API, beleértve a WebListener hello használó webkiszolgálók. HTTP-kiszolgáló API a Windows hello használó webkiszolgálók először kell lefoglalni az URL-cím elé *http.sys* (Ez általában érhető el a hello [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) eszköz). Ehhez a művelethez emelt szintű jogosultságokkal, amely a szolgáltatások alapértelmezés szerint nem rendelkeznek. hello "http" vagy "https" beállításait hello `Protocol` hello tulajdonságának `Endpoint` konfiguráció *ServiceManifest.xml* használt kifejezetten tooinstruct hello Service Fabric-futtatókörnyezet tooregister olyanURL-címe*http.sys* hello segítségével a nevünkben [ *erős helyettesítő* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL-előtagját.

Például tooreserve `http://+:80` egy szolgáltatás hello következő konfigurációt kell használni a ServiceManifest.xml:

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

Hello végpontnév kell átadni toohello és `WebListenerCommunicationListener` konstruktor:

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

#### <a name="use-weblistener-with-a-static-port"></a>A statikus port WebListener használata
toouse WebListener, statikus portot adja meg az hello port számát a hello `Endpoint` konfiguráció:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-weblistener-with-a-dynamic-port"></a>A dinamikus port WebListener használata
toouse WebListener, dinamikusan hozzárendelt portot, hagyja el hello `Port` hello tulajdonság `Endpoint` konfiguráció:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

Vegye figyelembe, hogy a dinamikus port lefoglalta egy `Endpoint` a konfiguráció csak egyetlen port biztosít *állomás folyamatonként*. hello üzemeltetési modell lehetővé teszi, hogy több szolgáltatás példányok és/vagy a replikák toobe hello tárolt aktuális Service Fabric ugyanezt a folyamatot, ami azt jelenti, hogy minden egyes használ-e azonos porton keresztül hello kiosztása során hello `Endpoint` konfigurációs. Több WebListener példány megoszthatja az alapul szolgáló hello segítségével *http.sys* port megosztása szolgáltatást, de, amely nem támogatja `WebListenerCommunicationListener` miatt toohello komplikációk az ügyféli kérelmek részére okozna. A dinamikus port használatról vércse webkiszolgáló ajánlott hello.

## <a name="kestrel-in-reliable-services"></a>A megbízható szolgáltatások vércse
Vércse használható egy megbízható szolgáltatás hello importálásával **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet-csomagot. Ez a csomag tartalmaz `KestrelCommunicationListener`, megvalósítása `ICommunicationListener`, ami lehetővé teszi az ASP.NET Core WebHost belül egy megbízható szolgáltatás toocreate hello webkiszolgálóval vércse használatával.

Vércse, az ASP.NET Core platformfüggetlen webkiszolgáló libuv, a platformok közötti aszinkron i/o-szalagtár egyik alapján. WebListener, eltérően vércse nem használ központi végponthoz vezető például *http.sys*. És WebListener, eltérően vércse nem támogatja a port megosztása több folyamat között. Minden példánynak vércse egy egyedi portot kell használnia.

![vércse][4]

### <a name="kestrel-in-a-stateless-service"></a>Az állapotmentes szolgáltatások vércse
toouse `Kestrel` állapotmentes szolgáltatások, a felülbírálás hello `CreateServiceInstanceListeners` metódust, és térjen vissza a `KestrelCommunicationListener` példány:

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

### <a name="kestrel-in-a-stateful-service"></a>Az állapotalapú service vércse
toouse `Kestrel` állapot-nyilvántartó szolgáltatásban, bírálja felül a hello `CreateServiceReplicaListeners` metódust, és térjen vissza a `KestrelCommunicationListener` példány:

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

Ebben a példában egy egypéldányos példánya `IReliableStateManager` toohello WebHost függőségi injektálási tároló biztosítja. Ez nem feltétlenül szükséges, de toouse lehetővé teszi `IReliableStateManager` és az MVC-vezérlő műveletmetódusokhoz megbízható gyűjteményei.

Vegye figyelembe, hogy egy `Endpoint` -konfiguráció nevét **nem** túl megadott`KestrelCommunicationListener` állapot-nyilvántartó szolgáltatásban. Ennek a magyarázatát hello a következő szakasz részletesen.

### <a name="endpoint-configuration"></a>Végpont-konfiguráció
Egy `Endpoint` konfigurációra nincs szükség toouse vércse. 

Vércse egy egyszerű önálló webkiszolgáló; WebListener (és HttpListener) eltérően nem kell egy `Endpoint` konfiguráció *ServiceManifest.xml* , mert nem szükséges URL-cím regisztrálása előzetes toostarting. 

#### <a name="use-kestrel-with-a-static-port"></a>A statikus port vércse használata
A statikus port konfigurálható hello `Endpoint` ServiceManifest.xml konfigurációját vércse való használatra. Ez azonban nem feltétlenül szükséges, akkor két lehetséges előnyöket nyújtja:
 1. Hello port nem esik hello alkalmazás porttartományát, ha az meg van nyitva hello OS tűzfalon keresztül a Service Fabric.
 2. a megadott URL-cím tooyou keresztül hello `KestrelCommunicationListener` ezt a portot fogja használni.

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

Ha egy `Endpoint` van konfigurálva, a nevét kell átadását hello `KestrelCommunicationListener` konstruktor: 

```csharp
new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) => ...
```

Ha egy `Endpoint` nem használja a konfigurációját, hagyja ki ezt a hello neve a hello `KestrelCommunicationListener` konstruktor. Ebben az esetben egy dinamikus portot fogja használni. Hello további információt a következő szakaszban talál.

#### <a name="use-kestrel-with-a-dynamic-port"></a>A dinamikus port vércse használata
Vércse hello hello automatikus port hozzárendelés nem használható `Endpoint` konfigurációját a ServiceManifest.xml, mert automatikus port hozzárendelés egy `Endpoint` konfigurációs hozzárendel egy egyedi port / *folyamaton* , és egyetlen gazdafolyamat vércse több példánya is tartalmazhat. Vércse nem támogatja a port megosztása, mert az nem működik vércse feltünteti egy egyedi portot kell megnyitni.

vércse, a dinamikus port-hozzárendelés toouse egyszerűen csak hagyja el a hello `Endpoint` ServiceManifest.xml konfiguráció teljesen, és nem továbbítja a végpont neve toohello `KestrelCommunicationListener` konstruktor:

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

Ebben a konfigurációban `KestrelCommunicationListener` automatikusan választja ki egy nem használt portot hello alkalmazás porttartományát.

## <a name="scenarios-and-configurations"></a>A forgatókönyvek és konfigurációk
Ez a szakasz ismerteti a következő forgatókönyvek hello és hello ajánlott webkiszolgáló, a port konfigurálása, a Service Fabric-integrációs beállítások és a különböző beállítások tooachieve egy megfelelően működő szolgáltatást:
 - Külsőleg kitett az ASP.NET Core állapotmentes szolgáltatások
 - Csak belső ASP.NET Core állapotmentes szolgáltatások
 - Csak belső ASP.NET Core állapotalapú szolgáltatás

Egy **külsőleg kitett** szolgáltatás egyike, amely elérhetővé teszi a hello fürtön, általában egy terhelés-kiegyenlítő kívülről elérhető végpontok.

Egy **csak belső** szolgáltatás egyik amelynek végpont csak elérhető hello fürtön belül.

> [!NOTE]
> Az állapotalapú szolgáltatás végpontok általában nem kell toohello Internet kitéve. Fürtök mögött található, amely nem tudnak a Service Fabric-szolgáltatás feloldási hello Azure terheléselosztó, például a terheléselosztók lesz nem tooexpose állapotalapú szolgáltatások, mert hello terheléselosztó nem tud toolocate lennie, és nem irányíthatja a forgalmat toohello a replika megfelelő állapotalapú szolgáltatásból. 

### <a name="externally-exposed-aspnet-core-stateless-services"></a>Külsőleg kitett az ASP.NET Core állapotmentes szolgáltatások
WebListener ajánlott az előtér-szolgáltatásokat, amelyek a Windows külső, az internetre irányuló HTTP-végpontokról láthatóvá webkiszolgálóját hello. Támadásokkal szembeni hatékonyabb védekezés és vércse viszont nem, például a Windows-hitelesítés és a port megosztása funkciókat is támogat. 

Vércse jelenleg nem támogatott (internetről hozzáférhető) edge-kiszolgálóként. Például az IIS vagy Nginx fordított proxykiszolgálón használható toohandle forgalmát hello nyilvános internethez.
 
Amikor elérhetővé toohello Internet, állapotmentes szolgáltatások egy jól ismert és állandó végponttal, amely elérhető a terheléselosztó kell használnia. Azt adja meg az alkalmazás toousers hello URL-CÍMÉT. a következő konfigurációs hello ajánlott:

|  |  | **Megjegyzések** |
| --- | --- | --- |
| Webkiszolgáló | WebListener | Ha hello szolgáltatás csak kitett tooa megbízható hálózathoz, az intraneten vércse is használható. Ellenkező esetben a WebListener hello előnyben részesített lehetőség. |
| Port konfigurálása | Statikus | A hello lehet beállítani a jól ismert statikus port `Endpoints` ServiceManifest.xml, például a 80-as HTTP vagy HTTPS-hez a 443-as konfigurációját. |
| ServiceFabricIntegrationOptions | None | Hello `ServiceFabricIntegrationOptions.None` beállítást kell használni, amikor a Service Fabric-integráció köztes konfigurálása, hogy hello szolgáltatás nem próbálja meg egy egyedi azonosítót toovalidate bejövő kérelmet. Az alkalmazás külső felhasználók nem fogja tudni hello egyedi azonosító adatokat hello köztes használják. |
| A példányok száma | -1 | A tipikus használati esetek hello beállítása példányszáma beállításaként túl-"1", hogy egy példány érhető el az összes olyan csomóponton, amelyet a forgalom fogadására a terheléselosztótól. |

Ha több kívülről elérhető szolgáltatások ugyanazt a csomópontok beállítása hello megosztásához stabil, de egyedi URL-cím használható. Ehhez a hello IWebHost konfigurálásakor megadott URL-cím módosításához. Megjegyzés: Ez csak tooWebListener vonatkozik.

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

### <a name="internal-only-stateless-aspnet-core-service"></a>Csak belső állapot nélküli ASP.NET Core szolgáltatás
Állapotmentes szolgáltatásokhoz, amelyeket a rendszer csak a hello fürtön belül egyedi URL-eket kell használnia, és dinamikusan kiosztott portok tooensure együttműködés több szolgáltatás között. a következő konfigurációs hello ajánlott:

|  |  | **Megjegyzések** |
| --- | --- | --- |
| Webkiszolgáló | vércse | Bár WebListener belső állapotmentes szolgáltatások is használhatók, a vércse hello ajánlott server tooallow több szolgáltatás példányok tooshare állomás.  |
| Port konfigurálása | dinamikusan kiosztott | Az állapotalapú szolgáltatás több replika megoszthatja a gazdafolyamat vagy a gazda operációs rendszer, és így kell egyedi portok. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Dinamikus portok hozzárendeléssel Ez a beállítás megakadályozza a korábban leírt összetéveszteni hello identitás probléma. |
| InstanceCount | bármely | hello beállítása példányszáma tooany szükséges toooperate hello szolgáltatás állítható be. |

### <a name="internal-only-stateful-aspnet-core-service"></a>Csak belső állapot-nyilvántartó ASP.NET Core szolgáltatás
Állapot-nyilvántartó szolgáltatásokat, amelyeket a rendszer csak a hello fürtön belül több szolgáltatások együttműködése dinamikusan hozzárendelt portok tooensure kell használnia. a következő konfigurációs hello ajánlott:

|  |  | **Megjegyzések** |
| --- | --- | --- |
| Webkiszolgáló | vércse | Hello `WebListenerCommunicationListener` több replikák osztozik egy állapotalapú szolgáltatások használatra nem szolgál. |
| Port konfigurálása | dinamikusan kiosztott | Az állapotalapú szolgáltatás több replika megoszthatja a gazdafolyamat vagy a gazda operációs rendszer, és így kell egyedi portok. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Dinamikus portok hozzárendeléssel Ez a beállítás megakadályozza a korábban leírt összetéveszteni hello identitás probléma. |

## <a name="next-steps"></a>Következő lépések
[A Service Fabric-alkalmazás hibakeresése a Visual Studio használatával](service-fabric-debugging-your-application.md)

<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
