---
title: "aaaReliable szereplője felügyeleti állapot |} Microsoft Docs"
description: "Útmutatás a Reliable Actors állapot felügyelt, megőrzött és a magas rendelkezésre állás érdekében replikálva."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 346d92426b1890617d108a9504afb179e463bded
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-actors-state-management"></a><span data-ttu-id="67bb8-103">Megbízható szereplője állapotkezelés</span><span class="sxs-lookup"><span data-stu-id="67bb8-103">Reliable Actors state management</span></span>
<span data-ttu-id="67bb8-104">Reliable Actors egyszálas objektumok, amellyel skálája ágyazható be mind a programot, és a állapotban.</span><span class="sxs-lookup"><span data-stu-id="67bb8-104">Reliable Actors are single-threaded objects that can encapsulate both logic and state.</span></span> <span data-ttu-id="67bb8-105">Szereplője Reliable Services futtatni, mert azok is-állapot karbantartásához megbízhatóan használatával azonos adatmegőrzési és replikációs mechanizmusok Reliable Services használó hello.</span><span class="sxs-lookup"><span data-stu-id="67bb8-105">Because actors run on Reliable Services, they can maintain state reliably by using hello same persistence and replication mechanisms that Reliable Services uses.</span></span> <span data-ttu-id="67bb8-106">Ezzel a módszerrel szereplője ne veszítse el állapotukra hiba esetén a szemétgyűjtés után, vagy tooresource terheléselosztás vagy frissítés miatt fürt csomópontjai között áthelyezett Újraaktiválási után.</span><span class="sxs-lookup"><span data-stu-id="67bb8-106">This way, actors don't lose their state after failures, upon reactivation after garbage collection, or when they are moved around between nodes in a cluster due tooresource balancing or upgrades.</span></span>

## <a name="state-persistence-and-replication"></a><span data-ttu-id="67bb8-107">Állapot megőrzését és a replikáció</span><span class="sxs-lookup"><span data-stu-id="67bb8-107">State persistence and replication</span></span>
<span data-ttu-id="67bb8-108">Minden Reliable Actors minősülnek *állapotalapú alkalmazások és szolgáltatások* mivel egyes szereplő példányok leképezik tooa egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="67bb8-108">All Reliable Actors are considered *stateful* because each actor instance maps tooa unique ID.</span></span> <span data-ttu-id="67bb8-109">Ez azt jelenti, hogy ugyanazon szereplő azonosító ismételt hívása toohello irányított toohello szereplő ugyanezen példányában.</span><span class="sxs-lookup"><span data-stu-id="67bb8-109">This means that repeated calls toohello same actor ID are routed toohello same actor instance.</span></span> <span data-ttu-id="67bb8-110">Állapot nélküli rendszerben ellentétben ügyfél hívások nem garantált irányított toobe toohello minden esetben ugyanarra a kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="67bb8-110">In a stateless system, by contrast, client calls are not guaranteed toobe routed toohello same server every time.</span></span> <span data-ttu-id="67bb8-111">Emiatt aktorszolgáltatások a mindig állapotalapú szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="67bb8-111">For this reason, actor services are always stateful services.</span></span>

<span data-ttu-id="67bb8-112">Annak ellenére, hogy gyakrabban minősülnek állapotfüggő, hogy nem jelenti azt, azok állapot megbízhatóan kell tárolnia.</span><span class="sxs-lookup"><span data-stu-id="67bb8-112">Even though actors are considered stateful, that does not mean they must store state reliably.</span></span> <span data-ttu-id="67bb8-113">Szereplője használhatja hello szintű állapot megőrzését, és a replikáció a tárhellyel kapcsolatos követelmények alapján az adatokat:</span><span class="sxs-lookup"><span data-stu-id="67bb8-113">Actors can choose hello level of state persistence and replication based on their data storage requirements:</span></span>

* <span data-ttu-id="67bb8-114">**A megőrzött állapot**: állapot megőrzött toodisk, és a replikált too3 vagy további replikákat.</span><span class="sxs-lookup"><span data-stu-id="67bb8-114">**Persisted state**: State is persisted toodisk and is replicated too3 or more replicas.</span></span> <span data-ttu-id="67bb8-115">Ez a lehetőség hello a tartós állapot tárolási, ahol állapota is megőrizni a teljes fürt szolgáltatáskimaradás keresztül.</span><span class="sxs-lookup"><span data-stu-id="67bb8-115">This is hello most durable state storage option, where state can persist through complete cluster outage.</span></span>
* <span data-ttu-id="67bb8-116">**"Volatile" állapot**: állapot replikált too3 vagy további replikákat, és csak a memóriában tárolja.</span><span class="sxs-lookup"><span data-stu-id="67bb8-116">**Volatile state**: State is replicated too3 or more replicas and only kept in memory.</span></span> <span data-ttu-id="67bb8-117">Ez lehetővé teszi a rugalmasság Csomóponthiba és szereplő hibák ellen, és a frissítések és az erőforrás terheléselosztás során.</span><span class="sxs-lookup"><span data-stu-id="67bb8-117">This provides resilience against node failure and actor failure, and during upgrades and resource balancing.</span></span> <span data-ttu-id="67bb8-118">Azonban állapota nem megőrzött toodisk.</span><span class="sxs-lookup"><span data-stu-id="67bb8-118">However, state is not persisted toodisk.</span></span> <span data-ttu-id="67bb8-119">Így minden replikák egyszerre is elvesznek, ha hello állapotát, valamint vész el.</span><span class="sxs-lookup"><span data-stu-id="67bb8-119">So if all replicas are lost at once, hello state is lost as well.</span></span>
* <span data-ttu-id="67bb8-120">**Megőrzött állapot nélküli**: állapot nem replikálható, illetve toodisk írása.</span><span class="sxs-lookup"><span data-stu-id="67bb8-120">**No persisted state**: State is not replicated or written toodisk.</span></span> <span data-ttu-id="67bb8-121">Ez a szint, egyszerűen nincs szükségük toomaintain állapot megbízhatóan szereplője van.</span><span class="sxs-lookup"><span data-stu-id="67bb8-121">This level is for actors that simply don't need toomaintain state reliably.</span></span>

<span data-ttu-id="67bb8-122">Minden egyes adatmegőrzési szintje egyszerűen egy másik *állapotszolgáltató* és *replikációs* a szolgáltatás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="67bb8-122">Each level of persistence is simply a different *state provider* and *replication* configuration of your service.</span></span> <span data-ttu-id="67bb8-123">Függetlenül attól, állapot írása toodisk hello állapotszolgáltató – hello összetevő állapota tárolt megbízható szolgáltatásban függ.</span><span class="sxs-lookup"><span data-stu-id="67bb8-123">Whether or not state is written toodisk depends on hello state provider--hello component in a reliable service that stores state.</span></span> <span data-ttu-id="67bb8-124">Replikáció működése függ a szolgáltatása hány replikák üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="67bb8-124">Replication depends on how many replicas a service is deployed with.</span></span> <span data-ttu-id="67bb8-125">Csakúgy, mint a Reliable Services mindkét hello állapotszolgáltató és replikaszám manuálisan egyszerűen megadhatók.</span><span class="sxs-lookup"><span data-stu-id="67bb8-125">As with Reliable Services, both hello state provider and replica count can easily be set manually.</span></span> <span data-ttu-id="67bb8-126">hello szereplő keretrendszer által biztosított, hogy a csomag használatakor a szereplő, automatikusan kiválasztja az alapértelmezett állapot szolgáltatót, és automatikusan létrehozza a replika száma tooachieve egy három adatmegőrzési beállítások beállításainak attribútum.</span><span class="sxs-lookup"><span data-stu-id="67bb8-126">hello actor framework provides an attribute that, when used on an actor, automatically selects a default state provider and automatically generates settings for replica count tooachieve one of these three persistence settings.</span></span> <span data-ttu-id="67bb8-127">hello StatePersistence attribútum nem örökli osztályból származtatott, szereplő típusonkénti biztosítania kell a StatePersistence szintje.</span><span class="sxs-lookup"><span data-stu-id="67bb8-127">hello StatePersistence attribute is not inherited by derived class, each Actor type must provide its StatePersistence level.</span></span>

### <a name="persisted-state"></a><span data-ttu-id="67bb8-128">Megőrzött állapot</span><span class="sxs-lookup"><span data-stu-id="67bb8-128">Persisted state</span></span>
```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl  extends FabricActor implements MyActor
{
}
```  
<span data-ttu-id="67bb8-129">A beállítás a lemezen tárolja az adatokat, és automatikusan beállítja a hello szolgáltatás replika száma too3 állapota szolgáltató használ.</span><span class="sxs-lookup"><span data-stu-id="67bb8-129">This setting uses a state provider that stores data on disk and automatically sets hello service replica count too3.</span></span>

### <a name="volatile-state"></a><span data-ttu-id="67bb8-130">"Volatile" állapota</span><span class="sxs-lookup"><span data-stu-id="67bb8-130">Volatile state</span></span>
```csharp
[StatePersistence(StatePersistence.Volatile)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Volatile)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
<span data-ttu-id="67bb8-131">Ezt a beállítást használja, a memória-csak állapotszolgáltató és készletek hello replika száma too3.</span><span class="sxs-lookup"><span data-stu-id="67bb8-131">This setting uses an in-memory-only state provider and sets hello replica count too3.</span></span>

### <a name="no-persisted-state"></a><span data-ttu-id="67bb8-132">Nincs olyan megőrzött állapot</span><span class="sxs-lookup"><span data-stu-id="67bb8-132">No persisted state</span></span>
```csharp
[StatePersistence(StatePersistence.None)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.None)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
<span data-ttu-id="67bb8-133">Ezt a beállítást használja, a memória-csak állapotszolgáltató és készletek hello replika száma too1.</span><span class="sxs-lookup"><span data-stu-id="67bb8-133">This setting uses an in-memory-only state provider and sets hello replica count too1.</span></span>

### <a name="defaults-and-generated-settings"></a><span data-ttu-id="67bb8-134">Alapértelmezett és a létrehozott beállítások</span><span class="sxs-lookup"><span data-stu-id="67bb8-134">Defaults and generated settings</span></span>
<span data-ttu-id="67bb8-135">Ha a hello `StatePersistence` attribútum, egy állapotszolgáltató automatikusan ki van jelölve, futási időben hello szereplő szolgáltatás elindulásakor.</span><span class="sxs-lookup"><span data-stu-id="67bb8-135">When you're using hello `StatePersistence` attribute, a state provider is automatically selected for you at runtime when hello actor service starts.</span></span> <span data-ttu-id="67bb8-136">hello replikaszám, azonban állítja be fordítás során hello Visual Studio szereplő összeállítási eszközök.</span><span class="sxs-lookup"><span data-stu-id="67bb8-136">hello replica count, however, is set at compile time by hello Visual Studio actor build tools.</span></span> <span data-ttu-id="67bb8-137">hello összeállítási eszközök automatikus generálása a *alapértelmezett szolgáltatás* ApplicationManifest.xml hello szereplő szolgáltatása esetében.</span><span class="sxs-lookup"><span data-stu-id="67bb8-137">hello build tools automatically generate a *default service* for hello actor service in ApplicationManifest.xml.</span></span> <span data-ttu-id="67bb8-138">Paraméterek létrejönnek az **a replikakészlet minimális mérete** és **cél replika méretének beállítása**.</span><span class="sxs-lookup"><span data-stu-id="67bb8-138">Parameters are created for **min replica set size** and **target replica set size**.</span></span>

<span data-ttu-id="67bb8-139">Ezek a paraméterek manuálisan módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="67bb8-139">You can change these parameters manually.</span></span> <span data-ttu-id="67bb8-140">Azonban minden alkalommal hello `StatePersistence` attribútum módosul, hello paraméterei vannak beállítva toohello alapértelmezett replika méretét értékeit kijelölt hello `StatePersistence` attribútum, felülírja az előző értéket.</span><span class="sxs-lookup"><span data-stu-id="67bb8-140">But each time hello `StatePersistence` attribute is changed, hello parameters are set toohello default replica set size values for hello selected `StatePersistence` attribute, overriding any previous values.</span></span> <span data-ttu-id="67bb8-141">Más szóval hello ServiceManifest.xml beállított értékei *csak* hello megváltoztatásakor build időpontban felül `StatePersistence` attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="67bb8-141">In other words, hello values that you set in ServiceManifest.xml are *only* overridden at build time when you change hello `StatePersistence` attribute value.</span></span>

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application12Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="MyActorService_PartitionCount" DefaultValue="10" />
      <Parameter Name="MyActorService_MinReplicaSetSize" DefaultValue="3" />
      <Parameter Name="MyActorService_TargetReplicaSetSize" DefaultValue="3" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyActorPkg" ServiceManifestVersion="1.0.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MyActorService" GeneratedIdRef="77d965dc-85fb-488c-bd06-c6c1fe29d593|Persisted">
         <StatefulService ServiceTypeName="MyActorServiceType" TargetReplicaSetSize="[MyActorService_TargetReplicaSetSize]" MinReplicaSetSize="[MyActorService_MinReplicaSetSize]">
            <UniformInt64Partition PartitionCount="[MyActorService_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
         </StatefulService>
      </Service>
   </DefaultServices>
</ApplicationManifest>
```

## <a name="state-manager"></a><span data-ttu-id="67bb8-142">Állapot manager</span><span class="sxs-lookup"><span data-stu-id="67bb8-142">State manager</span></span>
<span data-ttu-id="67bb8-143">Minden szereplő példány rendelkezik saját állapotkezelője: a szótár-szerű adatszerkezet, amely megbízhatóan tárolja az kulcs/érték párok.</span><span class="sxs-lookup"><span data-stu-id="67bb8-143">Every actor instance has its own state manager: a dictionary-like data structure that reliably stores key/value pairs.</span></span> <span data-ttu-id="67bb8-144">hello állapotkezelője egy állapotszolgáltató csomagolásának.</span><span class="sxs-lookup"><span data-stu-id="67bb8-144">hello state manager is a wrapper around a state provider.</span></span> <span data-ttu-id="67bb8-145">Használhatja toostore adatok függetlenül attól, milyen adatmegőrzési beállítást használja.</span><span class="sxs-lookup"><span data-stu-id="67bb8-145">You can use it toostore data regardless of which persistence setting is used.</span></span> <span data-ttu-id="67bb8-146">Nem biztosít semmilyen garantálja, hogy a futó szereplő szolgáltatást is módosítható egy "volatile" (a memória-csak) állapot beállítás tooa megőrzött állapota beállítást, miközben megőrzi az adat a működés közbeni frissítés keresztül.</span><span class="sxs-lookup"><span data-stu-id="67bb8-146">It does not provide any guarantees that a running actor service can be changed from a volatile (in-memory-only) state setting tooa persisted state setting through a rolling upgrade while preserving data.</span></span> <span data-ttu-id="67bb8-147">Azonban egy futó szolgáltatással kapcsolatos lehetséges toochange replika számát is.</span><span class="sxs-lookup"><span data-stu-id="67bb8-147">However, it is possible toochange replica count for a running service.</span></span>

<span data-ttu-id="67bb8-148">Állapot manager kulcsok karakterláncoknak kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="67bb8-148">State manager keys must be strings.</span></span> <span data-ttu-id="67bb8-149">Értékek általános és állhat bármilyen, beleértve az egyéni típusok.</span><span class="sxs-lookup"><span data-stu-id="67bb8-149">Values are generic and can be any type, including custom types.</span></span> <span data-ttu-id="67bb8-150">Hello állapotkezelője tárolt értékekkel kell lennie adategyezmény szerializálható, mert előfordulhat, hogy áthaladni hello hálózati tooother csomópontok replikáció során, és előfordulhat, hogy írható toodisk, attól függően, hogy egy szereplő állapota adatmegőrzési beállítást.</span><span class="sxs-lookup"><span data-stu-id="67bb8-150">Values stored in hello state manager must be data contract serializable because they might be transmitted over hello network tooother nodes during replication and might be written toodisk, depending on an actor's state persistence setting.</span></span>

<span data-ttu-id="67bb8-151">hello állapotkezelője közös szótár módszerek állapot, a megbízható szótárban található hasonló toothose kezeléséről mutatja.</span><span class="sxs-lookup"><span data-stu-id="67bb8-151">hello state manager exposes common dictionary methods for managing state, similar toothose found in Reliable Dictionary.</span></span>

### <a name="accessing-state"></a><span data-ttu-id="67bb8-152">Hozzáférés állapota</span><span class="sxs-lookup"><span data-stu-id="67bb8-152">Accessing state</span></span>
<span data-ttu-id="67bb8-153">Állapot gombot hello állapotkezelője keresztül elérhető.</span><span class="sxs-lookup"><span data-stu-id="67bb8-153">State can be accessed through hello state manager by key.</span></span> <span data-ttu-id="67bb8-154">Állapot kezelő metódusok nincsenek minden aszinkron, mert a lemez i/o azok lehet szükség, amikor szereplője teszi lehetővé a megőrzött állapot.</span><span class="sxs-lookup"><span data-stu-id="67bb8-154">State manager methods are all asynchronous because they might require disk I/O when actors have persisted state.</span></span> <span data-ttu-id="67bb8-155">Első hozzáférést, akkor objektumokat gyorsítótárba kerüljenek-e a memóriában.</span><span class="sxs-lookup"><span data-stu-id="67bb8-155">Upon first access, state objects are cached in memory.</span></span> <span data-ttu-id="67bb8-156">Ismételje meg a hozzáférési műveleteket access objektumok közvetlenül a memória, és lemezes i/o- vagy aszinkron környezetben-váltás terhet nélkül szinkron módon adja vissza.</span><span class="sxs-lookup"><span data-stu-id="67bb8-156">Repeat access operations access objects directly from memory and return synchronously without incurring disk I/O or asynchronous context-switching overhead.</span></span> <span data-ttu-id="67bb8-157">Egy állapotú objektumot eltávolítják a következő esetekben hello hello gyorsítótárból:</span><span class="sxs-lookup"><span data-stu-id="67bb8-157">A state object is removed from hello cache in hello following cases:</span></span>

* <span data-ttu-id="67bb8-158">Egy aktormetódus nem kezelt kivételt jelez, miután objektum kikeresi hello állapotkezelője.</span><span class="sxs-lookup"><span data-stu-id="67bb8-158">An actor method throws an unhandled exception after it retrieves an object from hello state manager.</span></span>
* <span data-ttu-id="67bb8-159">Egy szereplő javítása után aktiválása folyamatban vagy meghibásodás után.</span><span class="sxs-lookup"><span data-stu-id="67bb8-159">An actor is reactivated, either after being deactivated or after failure.</span></span>
* <span data-ttu-id="67bb8-160">hello állapota szolgáltató lapok toodisk állapotban.</span><span class="sxs-lookup"><span data-stu-id="67bb8-160">hello state provider pages state toodisk.</span></span> <span data-ttu-id="67bb8-161">Ez a viselkedés attól függ, hogy hello állapota szolgáltató általi megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="67bb8-161">This behavior depends on hello state provider implementation.</span></span> <span data-ttu-id="67bb8-162">hello alapértelmezett állapotszolgáltató hello `Persisted` beállítás van ez a viselkedés.</span><span class="sxs-lookup"><span data-stu-id="67bb8-162">hello default state provider for hello `Persisted` setting has this behavior.</span></span>

<span data-ttu-id="67bb8-163">Állapot szabványos használatával kérheti le *beolvasása* művelet, amelynek jelez `KeyNotFoundException`(C#) vagy `NoSuchElementException`(Java), ha a bejegyzés nem létezik hello kulcs:</span><span class="sxs-lookup"><span data-stu-id="67bb8-163">You can retrieve state by using a standard *Get* operation that throws `KeyNotFoundException`(C#) or `NoSuchElementException`(Java) if an entry does not exist for hello key:</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<int> GetCountAsync()
    {
        return this.StateManager.GetStateAsync<int>("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().getStateAsync("MyState");
    }
}
```

<span data-ttu-id="67bb8-164">Állapotának beolvasására használatával egy *TryGet* módszerrel, amely nem lépett, ha a bejegyzés nem létezik a kulcs:</span><span class="sxs-lookup"><span data-stu-id="67bb8-164">You can also retrieve state by using a *TryGet* method that does not throw if an entry does not exist for a key:</span></span>

```csharp
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task<int> GetCountAsync()
    {
        ConditionalValue<int> result = await this.StateManager.TryGetStateAsync<int>("MyState");
        if (result.HasValue)
        {
            return result.Value;
        }

        return 0;
    }
}
```
```Java
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().<Integer>tryGetStateAsync("MyState").thenApply(result -> {
            if (result.hasValue()) {
                return result.getValue();
            } else {
                return 0;
            });
    }
}
```

### <a name="saving-state"></a><span data-ttu-id="67bb8-165">Állapot mentése</span><span class="sxs-lookup"><span data-stu-id="67bb8-165">Saving state</span></span>
<span data-ttu-id="67bb8-166">hello állapotát kezelő lekérdezési módszerek tooan referenciaobjektum helyi memória adja vissza.</span><span class="sxs-lookup"><span data-stu-id="67bb8-166">hello state manager retrieval methods return a reference tooan object in local memory.</span></span> <span data-ttu-id="67bb8-167">Módosítja az objektumot a helyi memória önmagában nem okoz azt tartósan mentett toobe.</span><span class="sxs-lookup"><span data-stu-id="67bb8-167">Modifying this object in local memory alone does not cause it toobe saved durably.</span></span> <span data-ttu-id="67bb8-168">Ha egy objektum hello állapotkezelője lekért és módosítani, azt kell lehet ismételten beszúrni objektumba hello állapotát kezelő toobe tartósan mentve.</span><span class="sxs-lookup"><span data-stu-id="67bb8-168">When an object is retrieved from hello state manager and modified, it must be reinserted into hello state manager toobe saved durably.</span></span>

<span data-ttu-id="67bb8-169">Állapot is beszúrhat egy feltétel nélküli használatával *beállítása*, amely egyenértékű az hello van hello `dictionary["key"] = value` Szintaxis:</span><span class="sxs-lookup"><span data-stu-id="67bb8-169">You can insert state by using an unconditional *Set*, which is hello equivalent of hello `dictionary["key"] = value` syntax:</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task SetCountAsync(int value)
    {
        return this.StateManager.SetStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture setCountAsync(int value)
    {
        return this.stateManager().setStateAsync("MyState", value);
    }
}
```

<span data-ttu-id="67bb8-170">Állapot használatával adhat hozzá egy *Hozzáadás* metódust.</span><span class="sxs-lookup"><span data-stu-id="67bb8-170">You can add state by using an *Add* method.</span></span> <span data-ttu-id="67bb8-171">Ez a módszer jelez `InvalidOperationException`(C#) vagy `IllegalStateException`(Java), amikor tooadd egy kulcs, amely már létezik.</span><span class="sxs-lookup"><span data-stu-id="67bb8-171">This method throws `InvalidOperationException`(C#) or `IllegalStateException`(Java) when it tries tooadd a key that already exists.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task AddCountAsync(int value)
    {
        return this.StateManager.AddStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().addOrUpdateStateAsync("MyState", value, (key, old_value) -> old_value + value);
    }
}
```

<span data-ttu-id="67bb8-172">Állapot segítségével is hozzáadhat egy *TryAdd* metódust.</span><span class="sxs-lookup"><span data-stu-id="67bb8-172">You can also add state by using a *TryAdd* method.</span></span> <span data-ttu-id="67bb8-173">Ez a módszer nem lépett, amikor tooadd egy kulcs, amely már létezik.</span><span class="sxs-lookup"><span data-stu-id="67bb8-173">This method does not throw when it tries tooadd a key that already exists.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task AddCountAsync(int value)
    {
        bool result = await this.StateManager.TryAddStateAsync<int>("MyState", value);

        if (result)
        {
            // Added successfully!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().tryAddStateAsync("MyState", value).thenApply((result)->{
            if(result)
            {
                // Added successfully!
            }
        });
    }
}
```

<span data-ttu-id="67bb8-174">Aktor metódus hello végén hello állapotkezelője automatikusan menti a hozzáadott vagy egy insert vagy az update művelet által módosított értékeket.</span><span class="sxs-lookup"><span data-stu-id="67bb8-174">At hello end of an actor method, hello state manager automatically saves any values that have been added or modified by an insert or update operation.</span></span> <span data-ttu-id="67bb8-175">A "Mentés" tárolásakor toodisk és a replikációs, attól függően, hogy a használt hello beállításokat is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="67bb8-175">A "save" can include persisting toodisk and replication, depending on hello settings used.</span></span> <span data-ttu-id="67bb8-176">Nem módosult az értékeket nem megőrzött és replikálja.</span><span class="sxs-lookup"><span data-stu-id="67bb8-176">Values that have not been modified are not persisted or replicated.</span></span> <span data-ttu-id="67bb8-177">Ha nincs érték módosítva lett, a mentési művelet hello nincs hatása.</span><span class="sxs-lookup"><span data-stu-id="67bb8-177">If no values have been modified, hello save operation does nothing.</span></span> <span data-ttu-id="67bb8-178">Ha sikertelen a Mentés hello módosított állapota a rendszer elveti és hello eredeti állapot van töltve.</span><span class="sxs-lookup"><span data-stu-id="67bb8-178">If saving fails, hello modified state is discarded and hello original state is reloaded.</span></span>

<span data-ttu-id="67bb8-179">Mentheti is állapot manuálisan által hívó hello `SaveStateAsync` alapszintű hello szereplő metódust:</span><span class="sxs-lookup"><span data-stu-id="67bb8-179">You can also save state manually by calling hello `SaveStateAsync` method on hello actor base:</span></span>

```csharp
async Task IMyActor.SetCountAsync(int count)
{
    await this.StateManager.AddOrUpdateStateAsync("count", count, (key, value) => count > value ? count : value);

    await this.SaveStateAsync();
}
```
```Java
interface MyActor {
    CompletableFuture setCountAsync(int count)
    {
        this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value).thenApply();

        this.stateManager().saveStateAsync().thenApply();
    }
}
```

### <a name="removing-state"></a><span data-ttu-id="67bb8-180">Állapot eltávolítása</span><span class="sxs-lookup"><span data-stu-id="67bb8-180">Removing state</span></span>
<span data-ttu-id="67bb8-181">Megszüntetheti a állapot véglegesen egy szereplő állapotkezelője a hívó hello *eltávolítása* metódust.</span><span class="sxs-lookup"><span data-stu-id="67bb8-181">You can remove state permanently from an actor's state manager by calling hello *Remove* method.</span></span> <span data-ttu-id="67bb8-182">Ez a módszer jelez `KeyNotFoundException`(C#) vagy `NoSuchElementException`(Java), amikor tooremove egy kulcsot, amely nem létezik.</span><span class="sxs-lookup"><span data-stu-id="67bb8-182">This method throws `KeyNotFoundException`(C#) or `NoSuchElementException`(Java) when it tries tooremove a key that doesn't exist.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task RemoveCountAsync()
    {
        return this.StateManager.RemoveStateAsync("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().removeStateAsync("MyState");
    }
}
```

<span data-ttu-id="67bb8-183">Akkor is eltávolíthatja állapot véglegesen hello *TryRemove* metódust.</span><span class="sxs-lookup"><span data-stu-id="67bb8-183">You can also remove state permanently by using hello *TryRemove* method.</span></span> <span data-ttu-id="67bb8-184">Ez a módszer nem lépett, amikor tooremove egy kulcs, amely nem létezik.</span><span class="sxs-lookup"><span data-stu-id="67bb8-184">This method does not throw when it tries tooremove a key that does not exist.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task RemoveCountAsync()
    {
        bool result = await this.StateManager.TryRemoveStateAsync("MyState");

        if (result)
        {
            // State removed!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().tryRemoveStateAsync("MyState").thenApply((result)->{
            if(result)
            {
                // State removed!
            }
        });
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="67bb8-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="67bb8-185">Next steps</span></span>

<span data-ttu-id="67bb8-186">A Reliable Actors tárolt állapotban kell előtt az írásbeli toodisk szerializált és replikálja a magas rendelkezésre állás.</span><span class="sxs-lookup"><span data-stu-id="67bb8-186">State that's stored in Reliable Actors must be serialized before its written toodisk and replicated for high availability.</span></span> <span data-ttu-id="67bb8-187">További információ [szereplő típus szerializálási](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="67bb8-187">Learn more about [Actor type serialization](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span></span>

<span data-ttu-id="67bb8-188">A következő további információ [szereplő diagnosztika és teljesítményfigyelés](service-fabric-reliable-actors-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="67bb8-188">Next, learn more about [Actor diagnostics and performance monitoring](service-fabric-reliable-actors-diagnostics.md).</span></span>
