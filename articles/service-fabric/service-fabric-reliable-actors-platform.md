---
title: "a Service Fabric szereplője aaaReliable |} Microsoft Docs"
description: "Útmutatás a Reliable Actors a Reliable Services rendszer réteges és hello Service Fabric-platformról hello szolgáltatásának használatához."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: vturecek
ms.openlocfilehash: ecffb54139f1171c7839b77fed0be60950881198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-reliable-actors-use-hello-service-fabric-platform"></a>Service Fabric-platformról hello használatát a Reliable Actors
Ez a cikk ismerteti a Reliable Actors működése hello Azure Service Fabric-platformról. A keretrendszer, amely egy hello nevű állapot-nyilvántartó megbízható szolgáltatás implementációjában futtatása Reliable Actors *szereplő szolgáltatás*. hello szereplő szolgáltatás összes hello összetevők szükséges toomanage hello életciklus- és a a szereplőket terjesztéséhez tartalmazza:

* hello szereplő futásidejű életciklusát, szemétgyűjtés, felügyeli, és egyszálas hozzáférést.
* Az aktor szolgáltatás távoli eljáráshívási figyelő távelérési hívások tooactors fogad, és tooa kézbesítő tooroute toohello megfelelő szereplő példány küldése.
* hello szereplő Állapotszolgáltató becsomagolja állapotszolgáltatója (például hello megbízható gyűjtemények állapotszolgáltató), és egy adapter biztosít szereplő állapotkezelés.

Ezen összetevők együttesen űrlap hello megbízható szereplő keretrendszer.

## <a name="service-layering"></a>A réteges szolgáltatás
Hello szereplő szolgáltatás egy olyan megbízható szolgáltatás, mert az összes hello [alkalmazásmodell](service-fabric-application-model.md), életciklusát, [csomagolási](service-fabric-package-apps.md), [telepítési](service-fabric-deploy-remove-applications.md), frissítése és méretezése elveit Megbízható szolgáltatások alkalmazása hello azonos módon tooactor szolgáltatások. 

![Aktor szolgáltatás réteges][1]

hello előző ábrán látható hello Service Fabric alkalmazás-keretrendszerbeli és a felhasználói kód hello kapcsolatát. Kék elemek képviselő hello Reliable Services alkalmazás-keretrendszer, narancssárga hello megbízható szereplő keretrendszer, és zöld jelenti felhasználói kód.

A Reliable Services, a szolgáltatási örökli hello `StatefulService` osztály. Ez az osztály maga a származó `StatefulServiceBase` (vagy `StatelessService` állapotmentes szolgáltatások). A Reliable Actors hello szereplő szolgáltatást használja. hello szereplő szolgáltatás egy másik megvalósításában hello `StatefulServiceBase` osztály megvalósít hello szereplő minta a szereplője futtatják. Mert maga hello szereplő szolgáltatás megvalósítása csak `StatefulServiceBase`, saját szolgáltatás abból származó írhat `ActorService` és megvalósítása szolgáltatásszintű szolgáltatások hello azonos módon történő örökléskor `StatefulService`, például:

* Szolgáltatás biztonsági mentése és visszaállítása.
* Összes szereplő, például egy áramköri megszakító megosztás funkciót.
* Távoli eljáráshívások hello szereplő maga és a minden egyes szereplő.

> [!NOTE]
> Állapotalapú szolgáltatások jelenleg nem támogatottak a Java/Linux.

### <a name="using-hello-actor-service"></a>Hello szereplő szolgáltatással
Aktor célpéldánynál toohello szereplő szolgáltatás, amelyben futnak. Hello szereplő szolgáltatással szereplő példányok programokon keresztül szerezhet be hello szolgáltatást a környezetben. hello szolgáltatás környezet magában hordozza hello Partícióazonosító, szolgáltatás neve, az alkalmazásnév és más Service Fabric platform-specifikus adatait.:

```csharp
Task MyActorMethod()
{
    Guid partitionId = this.ActorService.Context.PartitionId;
    string serviceTypeName = this.ActorService.Context.ServiceTypeName;
    Uri serviceInstanceName = this.ActorService.Context.ServiceName;
    string applicationInstanceName = this.ActorService.Context.CodePackageActivationContext.ApplicationName;
}
```
```Java
CompletableFuture<?> MyActorMethod()
{
    UUID partitionId = this.getActorService().getServiceContext().getPartitionId();
    String serviceTypeName = this.getActorService().getServiceContext().getServiceTypeName();
    URI serviceInstanceName = this.getActorService().getServiceContext().getServiceName();
    String applicationInstanceName = this.getActorService().getServiceContext().getCodePackageActivationContext().getApplicationName();
}
```


Az összes megbízható szolgáltatások, például hello szereplő szolgáltatás a Service Fabric-futtatókörnyezet hello szolgáltatás típusú szerepelnie kell. Hello szereplő Service toorun a szereplő példányok, az aktor típusát is regisztrálni kell hello szereplő szolgáltatással. Hello `ActorRuntime` regisztrációs módszer a szereplőket végrehajtja ezt a műveletet. Hello legegyszerűbb esetben csak regisztrálhatja az aktor típusát, és hello szereplő szolgáltatás az alapértelmezett beállításokkal implicit módon használható:

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>().GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```

Másik megoldásként egy lambda által biztosított hello regisztrációs metódus tooconstruct hello szereplő szolgáltatás saját kezűleg is használhatja. Beállíthatja majd hello szereplő szolgáltatás, explicit módon összeállítani a szereplő példányokat, ahol megváltoztathatják a függőségek tooyour szereplő saját konstruktoraikban keresztül:

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new ActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
static class Program
{
    private static void Main()
    {
      ActorRuntime.registerActorAsync(
              MyActor.class,
              (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
              timeout);

        Thread.sleep(Long.MAX_VALUE);
    }
}
```

### <a name="actor-service-methods"></a>Aktor metódusok
Aktor szolgáltatás megvalósítja hello `IActorService` (C#) vagy `ActorService` (Java), amely viszont megvalósítja `IService` (C#) vagy `Service` (Java). Ez a Reliable Services távoli eljáráshívási, amely lehetővé teszi a távoli eljáráshívások metódusok által használt hello felület. Amely a szolgáltatás távoli eljáráshívás keresztül távolról hívható szolgáltatásiszint-metódusokat tartalmaz.

#### <a name="enumerating-actors"></a>Szereplője számbavétele
hello szereplő szolgáltatás lehetővé teszi egy ügyfél hello szereplője hello szolgáltatást üzemeltető tooenumerate metaadatait. Mivel hello szereplő szolgáltatás egy particionált állapotalapú szolgáltatásból, a számbavételi partíciónként történik. Mindegyik partíció sok szereplője tartalmazhat, mert hello számbavételi adja vissza a rendszer lapozható eredmények készlete. hello lapok visszacsatolódnak keresztül, amíg az összes olvasható. a következő példa azt mutatja meg hogyan hello toocreate szereplő szolgáltatási egy partíció összes aktív szereplő listáját:

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);

    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

```Java
ActorService actorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new ArrayList<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = actorServiceProxy.getActorsAsync(continuationToken);

    while(ActorInformation x: page.getItems())
    {
         if(x.isActive()){
              activeActors.add(x);
         }
    }

    continuationToken = page.getContinuationToken();
}
while (continuationToken != null);
```

#### <a name="deleting-actors"></a>Szereplője törlése
hello szereplő szolgáltatás is biztosít egy olyan függvényt szereplője törlése:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

Szereplője és állapotukra törléséről további információkért lásd: hello [szereplő életciklus dokumentáció](service-fabric-reliable-actors-lifecycle.md).

### <a name="custom-actor-service"></a>Egyéni szereplő szolgáltatás
Hello szereplő regisztrációs lambda használatával regisztrálhatja a saját egyéni szereplő szolgáltatás abból származó `ActorService` (C#) és `FabricActorService` (Java). A egyéni szereplő szolgáltatásban valósíthatja meg a saját szolgáltatásiszint-funkció szolgáltatás osztály írása `ActorService` (C#) vagy `FabricActorService` (Java). Egy egyéni szereplő szolgáltatási örökli összes hello szereplő futásidejű funkciói `ActorService` (C#) vagy `FabricActorService` (Java) és a saját metódusok lehetnek használt tooimplement.

```csharp
class MyActorService : ActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }
}
```
```Java
class MyActorService extends FabricActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, BiFunction<FabricActorService, ActorId, ActorBase> newActor)
    {
         super(context, typeInfo, newActor);
    }
}
```

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new MyActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
        Thread.sleep(Long.MAX_VALUE);
    }
}
```

#### <a name="implementing-actor-backup-and-restore"></a>Végrehajtási szereplő biztonsági mentése és visszaállítása
 A következő példa hello, hello egyéni szereplő szolgáltatás elérhetővé teszi a metódus tooback szereplő adatok kihasználásával hello távoli eljáráshívási figyelő már szerepel a `ActorService`:

```csharp
public interface IMyActorService : IService
{
    Task BackupActorsAsync();
}

class MyActorService : ActorService, IMyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }

    public Task BackupActorsAsync()
    {
        return this.BackupAsync(new BackupDescription(PerformBackupAsync));
    }

    private async Task<bool> PerformBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store hello contents of backupInfo.Directory
           return true;
        }
        finally
        {
           Directory.Delete(backupInfo.Directory, recursive: true);
        }
    }
}
```
```Java
public interface MyActorService extends Service
{
    CompletableFuture<?> backupActorsAsync();
}

class MyActorServiceImpl extends ActorService implements MyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<FabricActorService, ActorId, ActorBase> newActor)
    {
       super(context, typeInfo, newActor);
    }

    public CompletableFuture backupActorsAsync()
    {
        return this.backupAsync(new BackupDescription((backupInfo, cancellationToken) -> performBackupAsync(backupInfo, cancellationToken)));
    }

    private CompletableFuture<Boolean> performBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store hello contents of backupInfo.Directory
           return true;
        }
        finally
        {
           deleteDirectory(backupInfo.Directory)
        }
    }

    void deleteDirectory(File file) {
        File[] contents = file.listFiles();
        if (contents != null) {
            for (File f : contents) {
               deleteDirectory(f);
             }
        }
        file.delete();
    }
}
```


Ebben a példában `IMyActorService` , amely távoli eljáráshívási szerződés `IService` (C#) és `Service` (Java), majd valósítható `MyActorService`. A távoli eljáráshívás szerződés metódusok hozzáadásával `IMyActorService` jelenleg is elérhető tooa ügyfél keresztül egy távoli eljáráshívás proxy létrehozásával `ActorServiceProxy`:

```csharp
IMyActorService myActorServiceProxy = ActorServiceProxy.Create<IMyActorService>(
    new Uri("fabric:/MyApp/MyService"), ActorId.CreateRandom());

await myActorServiceProxy.BackupActorsAsync();
```
```Java
MyActorService myActorServiceProxy = ActorServiceProxy.create(MyActorService.class,
    new URI("fabric:/MyApp/MyService"), actorId);

myActorServiceProxy.backupActorsAsync();
```

## <a name="application-model"></a>Alkalmazásmodellt.
Aktorszolgáltatások Reliable Services, így hello alkalmazásmodell van hello azonos. Azonban hello szereplő keretrendszer build tools készítése hello alkalmazás modell fájlok meg.

### <a name="service-manifest"></a>Szolgáltatás jegyzék
hello szereplő framework összeállítási eszközök automatikus előállítására hello az aktor szolgáltatás ServiceManifest.xml fájl tartalmát. Ez a fájl tartalmazza:

* Aktor szolgáltatás típusa. hello típusnév jön létre, a aktor projekt neve alapján. Az aktor hello adatmegőrzési attribútum alapján, hello HasPersistedState jelző értéke is ennek megfelelően.
* A kódcsomag.
* A konfigurációs csomag.
* Erőforrások és a végpontok.

### <a name="application-manifest"></a>Az alkalmazásjegyzék
hello szereplő framework összeállítási eszközök automatikusan létrehozni az aktor szolgáltatás alapértelmezett szolgáltatásdefiníció. hello build tools feltöltése hello alapértelmezett szolgáltatás tulajdonságai:

* Set replikaszám a szereplő hello adatmegőrzési attribútum határozza meg. Minden alkalommal hello adatmegőrzési attribútum a aktor módosul, set replikaszám hello hello alapértelmezett szolgáltatás definícióban ennek megfelelően alaphelyzetbe áll.
* Partícióséma és a tartomány tooUniform Int64 hello teljes Int64 kulcs tartománnyal van beállítva.

## <a name="service-fabric-partition-concepts-for-actors"></a>A Service Fabric partícióazonosító fogalmak actors
Aktorszolgáltatások a particionált állapotalapú szolgáltatások. Mindegyik partíció szereplő szolgáltatási szereplője tartalmaz. Partíciók a rendszer automatikusan terjeszt a Service Fabric több csomópont feladatait. Ennek eredményeképpen szereplő példányok terjesztése.

![Aktor particionálás és terjesztési][5]

Megbízható szolgáltatások különböző partíciós séma és a partíció kulcstartományokkal hozhatja létre. hello szereplő szolgáltatás hello teljes Int64 kulcs tartomány toomap szereplője toopartitions hello Int64 particionálási sémát használ.

### <a name="actor-id"></a>Aktor azonosítója
Minden egyes szereplő hello szolgáltatásban létrehozott van társítva, hello által képviselt egyedi azonosítója `ActorId` osztály. `ActorId`érték nem átlátszó azonosító használható szereplője egységes eloszlásának hello szolgáltatáspartíciók keresztül véletlenszerű azonosítók létrehozásával:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


Minden `ActorId` kivonatolt tooan Int64 van. Ezért hello szereplő szolgáltatás hello Int64 kulcs számos egy Int64 particionálási sémát kell használnia. Azonban használható egyéni értéket egy `ActorID`, beleértve a GUID/UUID-k, karakterláncok és Int64s.

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```
```Java
ActorProxyBase.create(MyActor.class, new ActorId(UUID.randomUUID()));
ActorProxyBase.create(MyActor.class, new ActorId("myActorId"));
ActorProxyBase.create(MyActor.class, new ActorId(1234));
```

GUID azonosítók/UUID-k és karakterláncok használatakor hello értékei kivonatolt tooan Int64. Azonban ha feltétlenül van explicit módon adja meg egy Int64 tooan `ActorId`, hello Int64 felelteti meg közvetlenül tooa partíció további kivonatoláshoz nélkül. Ez a módszer toocontrol partíció hello szereplője elhelyezkedő is használhatja.

## <a name="next-steps"></a>Következő lépések
* [Aktor állapotkezelés](service-fabric-reliable-actors-state-management.md)
* [Aktor életciklusának és szemétgyűjtési gyűjtése](service-fabric-reliable-actors-lifecycle.md)
* [Actors API referenciadokumentációt tartalmaz](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [.NET mintakód](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java-példakód](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
