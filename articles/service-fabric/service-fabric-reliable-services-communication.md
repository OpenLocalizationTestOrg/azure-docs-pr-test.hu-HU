---
title: "aaaReliable kommunikációs áttekintése |} Microsoft Docs"
description: "Hello Reliable Services kommunikációs modellt, beleértve a szolgáltatások figyelőinek megnyitásakor, végpontok megoldása és -szolgáltatások közötti kommunikáció áttekintése."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: 36217988-420e-409d-b0a4-e0e875b6eac8
ms.service: service-fabric
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/07/2017
ms.author: vturecek
ms.openlocfilehash: 93a7017b50df0822969daa5ad78302c73e8ba641
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-reliable-services-communication-apis"></a>Hogyan toouse hello Reliable Services kommunikációs API-k
Az Azure Service Fabric platformként rendszer teljesen független kapcsolatos szolgáltatások közötti kommunikáció. Protokollok és a verem elfogadhatók, az UDP tooHTTP. Már fel toohello service fejlesztői toochoose hogyan lépjen kapcsolatba a szolgáltatások. hello Reliable Services alkalmazás-keretrendszer biztosít, továbbá az API-k használható toobuild vermek kommunikáció az egyéni kommunikációs összetevők.

## <a name="set-up-service-communication"></a>Szolgáltatások közötti kommunikáció beállítása
hello megbízható szolgáltatások API szolgáltatások közötti kommunikáció egy egyszerű felületen használ. a szolgáltatási végpont tooopen egyszerűen valósítja meg ez az interfész:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

```java
public interface CommunicationListener {
    CompletableFuture<String> openAsync(CancellationToken cancellationToken);

    CompletableFuture<?> closeAsync(CancellationToken cancellationToken);

    void abort();
}
```

A szolgáltatás-alapú osztály metódus-felülbírálása visszaküldésével majd a kommunikációs figyelő megvalósítás is hozzáadhat.

Az állapotmentes szolgáltatások:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```
```java
public class MyStatelessService extends StatelessService {

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ...
    }
    ...
}
```

Az állapotalapú szolgáltatások:

> [!NOTE]
> Állapot-nyilvántartó megbízható szolgáltatások nem támogatottak a Java még.
>
>

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

Mindkét esetben meg kell visszaadnia figyelők. Ez lehetővé teszi, hogy a szolgáltatás toolisten potenciálisan protokollal különböző, több figyelői használatával több végpontokon. Például előfordulhat, hogy egy HTTP-figyelő és egy külön WebSocket-figyelő. Minden egyes figyelő lekérdezi a nevét, és eredményül kapott gyűjtemény hello *name: cím* párok jelentésekként jelennek meg a JSON-objektumból, amikor egy ügyfél lekérdezi egy szolgáltatáspéldány vagy partíció hello figyelő címeit.

Az állapotmentes szolgáltatások hello felülbírálás ServiceInstanceListeners gyűjteményének beolvasása. A `ServiceInstanceListener` egy függvény toocreate tartalmaz egy `ICommunicationListener(C#) / CommunicationListener(Java)` és elnevezi azt. Állapotalapú szolgáltatások esetén hello felülbírálás ServiceReplicaListeners gyűjteményének beolvasása. Ez egy némileg eltérő a állapotmentes párjukhoz, mert egy `ServiceReplicaListener` egy beállítás tooopen rendelkezik egy `ICommunicationListener` a másodlagos replikákon. Nem csak használhatók több kommunikációs figyelőket a szolgáltatásban, de azt is megadhatja mely figyelők a másodlagos replikákon kérelmek fogadásához, és melyeket figyelni csak az elsődleges replikára változott.

Például rendelkezhet egy ServiceRemotingListener, amely RPC-hívások csak az elsődleges replikára változott, és egy második, egyéni figyelő, amely olvasási kérések a másodlagos replikákon HTTP Protokollon keresztül:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [!NOTE]
> Egy szolgáltatás minden egyes figyelő több figyelői létrehozásakor **kell** egy egyedi nevet kell adni.
>
>

Végezetül ismertetik a szükséges hello hello szolgáltatást hello végpontok [szolgáltatás jegyzékfájl](service-fabric-application-model.md) hello szakaszban végpontokon.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

hello kommunikációs figyelő érhetik el hello végpont erőforrásokat lefoglalni tooit hello `CodePackageActivationContext` a hello `ServiceContext`. hello figyelő is indíthatja a kérések figyelését, ha meg van nyitva.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```
```java
CodePackageActivationContext codePackageActivationContext = serviceContext.getCodePackageActivationContext();
int port = codePackageActivationContext.getEndpoint("ServiceEndpoint").getPort();

```

> [!NOTE]
> Végpont erőforrások közös toohello teljes szolgáltatáscsomagot, és azok osztja a Service Fabric hello szolgáltatáscsomag aktiválásakor. Több, azonos ServiceHost megoszthatja hello üzemeltetett szolgáltatás replika hello azonos port. Ez azt jelenti, hogy hello kommunikációs figyelő támogatnia kell a port megosztása. hello ajánlott módja ezzel hello kommunikációs figyelő toouse hello partíció azonosítója és a replika/példány azonosítója hello figyelési cím létrehozása során.
>
>

### <a name="service-address-registration"></a>Service cím regisztrálása
Rendszerszolgáltatás nevű hello *szolgáltatás* Service Fabric-fürtök futtatja. hello szolgáltatás a szolgáltatások és az-címét, amely minden példányt, vagy a replika hello szolgáltatást figyel-regisztrálójába. Ha hello `OpenAsync(C#) / openAsync(Java)` metódusában egy `ICommunicationListener(C#) / CommunicationListener(Java)` befejeződött, a visszatérési érték a Naming Service hello lekérdezi regisztrálva. A visszatérési érték, amely lekérdezi a közzétett hello szolgáltatás van egy karakterláncot, amelynek az értéke bármi lehet minden. A karakterlánc értéke ügyfelek látják, ha egy címet a Naming Service hello hello szolgáltatást, hogy kérni.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

    // hello string returned here will be published in hello Naming Service.
    return Task.FromResult(this.publishAddress);
}
```
```java
public CompletableFuture<String> openAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.getCodePackageActivationContext.getEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.getPort();

    this.publishAddress = String.format("http://%s:%d/", FabricRuntime.getNodeContext().getIpAddressOrFQDN(), port);

    this.webApp = new WebApp(port);
    this.webApp.start();

    /* hello string returned here will be published in hello Naming Service.
     */
    return CompletableFuture.completedFuture(this.publishAddress);
}
```

A Service Fabric biztosít az API-k, amelyek lehetővé teszik az ügyfelek és egyéb szolgáltatások toothen feltenni ehhez a címhez szolgáltatás neve. Ez azért fontos, mert hello szolgáltatás cím nem statikus. Szolgáltatások hello fürt erőforrás és a rendelkezésre állási célokra helyezi át. Ez az hello mechanizmus, amelyek lehetővé teszik az ügyfelek tooresolve hello cím egy szolgáltatás figyeli.

> [!NOTE]
> A teljes útmutató, hogyan toowrite egy kommunikációs figyelő: a [Service Fabric Web API szolgáltatások OWIN önálló üzemeltető](service-fabric-reliable-services-communication-webapi.md) C#, Java írhat a saját HTTP megvalósított NFS-kiszolgáló, mivel lásd EchoServer alkalmazás Példa: https://github.com/Azure-Samples/service-fabric-java-getting-started.
>
>

## <a name="communicating-with-a-service"></a>A szolgáltatással folytatott kommunikáció
hello megbízható szolgáltatások API a következő szolgáltatásokkal kommunikálni szalagtárak toowrite ügyfelek hello biztosít.

### <a name="service-endpoint-resolution"></a>Szolgáltatási végpont felbontás
első lépés toocommunication hello szolgáltatást tooresolve végpontcím hello partíciók és azt szeretné, hogy tootalk hello service-példány. Hello `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` segédprogram osztály egy alapszintű primitív, amellyel az ügyfelek határozza meg a szolgáltatások futási időben hello végpontja. A Service Fabric terminológia hello folyamat meghatározása során a szolgáltatások végpontja hello a hivatkozott tooas hello *végpont névfeloldási szolgáltatás*.

a fürtön belüli tooservices tooconnect, ServicePartitionResolver hozhatók létre az alapértelmezett beállításokkal. Ez az ajánlott használati esetek többségében hello:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();
```

tooconnect tooservices különböző fürtben, a ServicePartitionResolver is létrehozható a fürt átjáró végpontjai vannak beállítva. Ne feledje, hogy átjáró végpontok toohello kapcsolódás csak másik végpontok ugyanabban a fürtben. Példa:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Másik lehetőségként `ServicePartitionResolver` adható meg a függvény létrehozásához egy `FabricClient` toouse belső:

```csharp
public delegate FabricClient CreateFabricClientDelegate();
```
```java
public FabricServicePartitionResolver(CreateFabricClient createFabricClient) {
...
}

public interface CreateFabricClient {
    public FabricClient getFabricClient();
}
```

`FabricClient`hello objektum, amely a Service Fabric-fürt hello hello fürt a különböző felügyeleti műveleteihez használt toocommunicate van. Ez akkor hasznos, ha azt szeretné, hogy egy szolgáltatás partíció feloldó hogyan működjön együtt a fürt teljesebb körű vezérlése. `FabricClient`elvégzi a belső gyorsítótár, és általában drága toocreate, ezért fontos tooreuse `FabricClient` példányok szerint lehetséges.

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver(() -> new CreateFabricClientImpl());
```

A resolve metódus nem használható tooretrieve hello cím egy szolgáltatás vagy szolgáltatás partíciója particionált szolgáltatások számára.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();

CompletableFuture<ResolvedServicePartition> partition =
    resolver.resolveAsync(new URI("fabric:/MyApp/MyService"), new ServicePartitionKey());
```

A szolgáltatás címe egyszerűen a egy ServicePartitionResolver segítségével megoldható, de további feladata, tooensure hello feloldva megfelelően cím is használható. Az ügyfél toodetect kell, hogy hello kapcsolódási kísérlet egy átmeneti hiba miatt sikertelen volt, és követően újra megkísérelhető (pl. szolgáltatás áthelyezése vagy átmenetileg nem érhető el), vagy egy állandó hiba (pl. szolgáltatást törölték vagy hello kért erőforrás már nem létezik). Egy szolgáltatáspéldány vagy replikák mozgathatók a csomópont toonode több okokból bármikor. hello szolgáltatás címe ServicePartitionResolver révén az Ügyfélkód kísérletek tooconnect hello időpontjára elavult lehet. Ebben az esetben újra hello ügyfélnek kell toore feloldása hello cím. Előző hello biztosító `ResolvedServicePartition` azt jelzi, hogy feloldó igények tootry újra hello helyett egyszerűen a gyorsítótárazott cím beolvasása.

Általában hello Ügyfélkód kell együttműködni hello ServicePartitionResolver közvetlenül. Létrehozott, és átadja toocommunication ügyfél előállítók a hello megbízható Services API. hello előállítók használja hello feloldó belső toogenerate egy objektumot, amely szolgáltatásokkal használt toocommunicate.

### <a name="communication-clients-and-factories"></a>Kommunikáció az ügyfelek és előállítók
hello kommunikációs gyári függvénytár egy tipikus hiba-kezelési újrapróbálkozási mintát, amely megkönnyíti a újrapróbálása kapcsolatok tooresolved Szolgáltatásvégpontok valósítja meg. hello gyári könyvtár biztosít hello újrapróbálkozási mechanizmussal, míg a hello hibakezelők megadnia.

`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`Meghatározza a kommunikációs ügyfélgyára, amely létrehozza az ügyfelek is kommunikálhatnak tooa Service Fabric-szolgáltatás által megvalósított hello alapszintű felületet. hello végrehajtásának hello CommunicationClientFactory hello kommunikációs verem hello Service Fabric-szolgáltatás által használt, ahol hello ügyfél szeretne toocommunicate függ. hello megbízható szolgáltatások API biztosít egy `CommunicationClientFactoryBase<TCommunicationClient>`. Ez egy alapszintű hello CommunicationClientFactory illesztőfelület megvalósítása biztosít, és feladatokat, amelyek közös tooall hello kommunikációs verem hajtja végre. (Ezek a feladatok között a ServicePartitionResolver toodetermine hello szolgáltatásvégpont használatával). Ügyfelek általában valósítania hello absztrakt CommunicationClientFactoryBase osztály toohandle logikát, amely adott toohello kommunikációs verem.

hello kommunikációs ügyfél csak egy címet kap, és tooconnect tooa szolgáltatást használja. hello ügyfél bármilyen szeretnének protokoll használható.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```
```java
public class MyCommunicationClient implements CommunicationClient {

    private ResolvedServicePartition resolvedServicePartition;
    private String listenerName;
    private ResolvedServiceEndpoint endPoint;

    /*
     * Getters and Setters
     */
}
```

hello ügyfélgyára elsődlegesen létrehozásáért felelős az ügyfelek kommunikációt. Az ügyfelek, amelyek nem tartanak fenn állandó kapcsolatot, például egy HTTP-ügyfél hello gyári csak toocreate és kell visszatérési hello ügyfél. Állandó kapcsolatot, például az egyes bináris protokollok más protokollokat kell is lehet érvényesíteni a hello gyári toodetermine e hello kapcsolat kell toobe újból létrehozza.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```
```java
public class MyCommunicationClientFactory extends CommunicationClientFactoryBase<MyCommunicationClient> {

    @Override
    protected boolean validateClient(MyCommunicationClient clientChannel) {
    }

    @Override
    protected boolean validateClient(String endpoint, MyCommunicationClient client) {
    }

    @Override
    protected CompletableFuture<MyCommunicationClient> createClientAsync(String endpoint) {
    }

    @Override
    protected void abortClient(MyCommunicationClient client) {
    }
}
```

Végül egy kivételkezelőbe felelős milyen művelet tootake meghatározása során kivétel következik be. Kivételek kategóriákba vannak **Újrapróbálkozást lehetővé tevő** és **Újrapróbálkozást lehetővé nem tevő**.

* **Újrapróbálkozást lehetővé nem tevő** kivételek egyszerűen get újra kiváltja hátsó toohello hívó.
* **Újrapróbálkozást lehetővé tevő** kivételek további kategóriákba **átmeneti** és **nem átmeneti**.
  * **Átmeneti** kivételek, azokat, egyszerűen újra feloldani az hello szolgáltatás végpontcím nem ismételhető. Ez magában foglalja az átmeneti hálózati probléma, vagy nem létezik a szolgáltatás hibaválaszok eltérő jelző hello szolgáltatási végpont címe.
  * **Nem tranziens** kivételek az alábbiakhoz szükséges hello szolgáltatás végpont címe toobe újra feloldani. Ezek közé tartozik a kivételeket, amelyek jelzik a hello szolgáltatásvégponthoz nem érhető el, jelző hello szolgáltatás áthelyezte tooa másik csomópont.

Hello `TryHandleException` lehetővé teszi egy adott kivétel kapcsolatos döntést hoznak. Ha azt **nem tudja** milyen döntéseket toomake vonatkozó kivételt, az kell visszaadnia **hamis**. Ha azt **tudja** milyen döntési toomake kell megfelelően állítsa hello eredményt, és vissza **igaz**.

```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let hello next IExceptionHandler attempt toohandle it)
        result = null;
        return false;
    }
}
```
```java
public class MyExceptionHandler implements ExceptionHandler {

    @Override
    public ExceptionHandlingResult handleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings) {        

        /* if exceptionInformation.getException() is known and is transient (can be retried without re-resolving)
         */
        result = new ExceptionHandlingRetryResult(exceptionInformation.getException(), true, retrySettings, retrySettings.getDefaultMaxRetryCount());
        return true;


        /* if exceptionInformation.getException() is known and is not transient (indicates a new service endpoint address must be resolved)
         */
        result = new ExceptionHandlingRetryResult(exceptionInformation.getException(), false, retrySettings, retrySettings.getDefaultMaxRetryCount());
        return true;

        /* if exceptionInformation.getException() is unknown (let hello next ExceptionHandler attempt toohandle it)
         */
        result = null;
        return false;

    }
}
```
### <a name="putting-it-all-together"></a>A teljes kép
Az egy `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, és `IExceptionHandler(C#) / ExceptionHandler(Java)` olyan kommunikációs protokollt épül egy `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` burkolt menüelem együtt, és hello tartalék kezelése és a szolgáltatás partíció cím feloldási hurok körül ezeket az összetevőket tartalmaz.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with hello service using hello client.
   },
   CancellationToken.None);

```
```java
private MyCommunicationClientFactory myCommunicationClientFactory;
private URI myServiceUri;

FabricServicePartitionClient myServicePartitionClient = new FabricServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

CompletableFuture<?> result = myServicePartitionClient.invokeWithRetryAsync(client -> {
      /* Communicate with hello service using hello client.
       */
   });

```

## <a name="next-steps"></a>Következő lépések
* Példa a szolgáltatások közötti HTTP-kommunikációt egy [C#-projektet mintát a Githubon](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) vagy [Java mintaprojektet a Githubon](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).
* [Távoli eljáráshívások a Reliable Services távoli eljáráshívás](service-fabric-reliable-services-communication-remoting.md)
* [Webes API-t használó OWIN Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [WCF-kommunikáció Reliable Services használatával](service-fabric-reliable-services-communication-wcf.md)
