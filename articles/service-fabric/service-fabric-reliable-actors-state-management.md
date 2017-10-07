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
# <a name="reliable-actors-state-management"></a>Megbízható szereplője állapotkezelés
Reliable Actors egyszálas objektumok, amellyel skálája ágyazható be mind a programot, és a állapotban. Szereplője Reliable Services futtatni, mert azok is-állapot karbantartásához megbízhatóan használatával azonos adatmegőrzési és replikációs mechanizmusok Reliable Services használó hello. Ezzel a módszerrel szereplője ne veszítse el állapotukra hiba esetén a szemétgyűjtés után, vagy tooresource terheléselosztás vagy frissítés miatt fürt csomópontjai között áthelyezett Újraaktiválási után.

## <a name="state-persistence-and-replication"></a>Állapot megőrzését és a replikáció
Minden Reliable Actors minősülnek *állapotalapú alkalmazások és szolgáltatások* mivel egyes szereplő példányok leképezik tooa egyedi azonosítója. Ez azt jelenti, hogy ugyanazon szereplő azonosító ismételt hívása toohello irányított toohello szereplő ugyanezen példányában. Állapot nélküli rendszerben ellentétben ügyfél hívások nem garantált irányított toobe toohello minden esetben ugyanarra a kiszolgálóra. Emiatt aktorszolgáltatások a mindig állapotalapú szolgáltatások.

Annak ellenére, hogy gyakrabban minősülnek állapotfüggő, hogy nem jelenti azt, azok állapot megbízhatóan kell tárolnia. Szereplője használhatja hello szintű állapot megőrzését, és a replikáció a tárhellyel kapcsolatos követelmények alapján az adatokat:

* **A megőrzött állapot**: állapot megőrzött toodisk, és a replikált too3 vagy további replikákat. Ez a lehetőség hello a tartós állapot tárolási, ahol állapota is megőrizni a teljes fürt szolgáltatáskimaradás keresztül.
* **"Volatile" állapot**: állapot replikált too3 vagy további replikákat, és csak a memóriában tárolja. Ez lehetővé teszi a rugalmasság Csomóponthiba és szereplő hibák ellen, és a frissítések és az erőforrás terheléselosztás során. Azonban állapota nem megőrzött toodisk. Így minden replikák egyszerre is elvesznek, ha hello állapotát, valamint vész el.
* **Megőrzött állapot nélküli**: állapot nem replikálható, illetve toodisk írása. Ez a szint, egyszerűen nincs szükségük toomaintain állapot megbízhatóan szereplője van.

Minden egyes adatmegőrzési szintje egyszerűen egy másik *állapotszolgáltató* és *replikációs* a szolgáltatás konfigurációját. Függetlenül attól, állapot írása toodisk hello állapotszolgáltató – hello összetevő állapota tárolt megbízható szolgáltatásban függ. Replikáció működése függ a szolgáltatása hány replikák üzembe helyezéséhez. Csakúgy, mint a Reliable Services mindkét hello állapotszolgáltató és replikaszám manuálisan egyszerűen megadhatók. hello szereplő keretrendszer által biztosított, hogy a csomag használatakor a szereplő, automatikusan kiválasztja az alapértelmezett állapot szolgáltatót, és automatikusan létrehozza a replika száma tooachieve egy három adatmegőrzési beállítások beállításainak attribútum. hello StatePersistence attribútum nem örökli osztályból származtatott, szereplő típusonkénti biztosítania kell a StatePersistence szintje.

### <a name="persisted-state"></a>Megőrzött állapot
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
A beállítás a lemezen tárolja az adatokat, és automatikusan beállítja a hello szolgáltatás replika száma too3 állapota szolgáltató használ.

### <a name="volatile-state"></a>"Volatile" állapota
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
Ezt a beállítást használja, a memória-csak állapotszolgáltató és készletek hello replika száma too3.

### <a name="no-persisted-state"></a>Nincs olyan megőrzött állapot
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
Ezt a beállítást használja, a memória-csak állapotszolgáltató és készletek hello replika száma too1.

### <a name="defaults-and-generated-settings"></a>Alapértelmezett és a létrehozott beállítások
Ha a hello `StatePersistence` attribútum, egy állapotszolgáltató automatikusan ki van jelölve, futási időben hello szereplő szolgáltatás elindulásakor. hello replikaszám, azonban állítja be fordítás során hello Visual Studio szereplő összeállítási eszközök. hello összeállítási eszközök automatikus generálása a *alapértelmezett szolgáltatás* ApplicationManifest.xml hello szereplő szolgáltatása esetében. Paraméterek létrejönnek az **a replikakészlet minimális mérete** és **cél replika méretének beállítása**.

Ezek a paraméterek manuálisan módosíthatja. Azonban minden alkalommal hello `StatePersistence` attribútum módosul, hello paraméterei vannak beállítva toohello alapértelmezett replika méretét értékeit kijelölt hello `StatePersistence` attribútum, felülírja az előző értéket. Más szóval hello ServiceManifest.xml beállított értékei *csak* hello megváltoztatásakor build időpontban felül `StatePersistence` attribútum értéke.

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

## <a name="state-manager"></a>Állapot manager
Minden szereplő példány rendelkezik saját állapotkezelője: a szótár-szerű adatszerkezet, amely megbízhatóan tárolja az kulcs/érték párok. hello állapotkezelője egy állapotszolgáltató csomagolásának. Használhatja toostore adatok függetlenül attól, milyen adatmegőrzési beállítást használja. Nem biztosít semmilyen garantálja, hogy a futó szereplő szolgáltatást is módosítható egy "volatile" (a memória-csak) állapot beállítás tooa megőrzött állapota beállítást, miközben megőrzi az adat a működés közbeni frissítés keresztül. Azonban egy futó szolgáltatással kapcsolatos lehetséges toochange replika számát is.

Állapot manager kulcsok karakterláncoknak kell lenniük. Értékek általános és állhat bármilyen, beleértve az egyéni típusok. Hello állapotkezelője tárolt értékekkel kell lennie adategyezmény szerializálható, mert előfordulhat, hogy áthaladni hello hálózati tooother csomópontok replikáció során, és előfordulhat, hogy írható toodisk, attól függően, hogy egy szereplő állapota adatmegőrzési beállítást.

hello állapotkezelője közös szótár módszerek állapot, a megbízható szótárban található hasonló toothose kezeléséről mutatja.

### <a name="accessing-state"></a>Hozzáférés állapota
Állapot gombot hello állapotkezelője keresztül elérhető. Állapot kezelő metódusok nincsenek minden aszinkron, mert a lemez i/o azok lehet szükség, amikor szereplője teszi lehetővé a megőrzött állapot. Első hozzáférést, akkor objektumokat gyorsítótárba kerüljenek-e a memóriában. Ismételje meg a hozzáférési műveleteket access objektumok közvetlenül a memória, és lemezes i/o- vagy aszinkron környezetben-váltás terhet nélkül szinkron módon adja vissza. Egy állapotú objektumot eltávolítják a következő esetekben hello hello gyorsítótárból:

* Egy aktormetódus nem kezelt kivételt jelez, miután objektum kikeresi hello állapotkezelője.
* Egy szereplő javítása után aktiválása folyamatban vagy meghibásodás után.
* hello állapota szolgáltató lapok toodisk állapotban. Ez a viselkedés attól függ, hogy hello állapota szolgáltató általi megvalósítása. hello alapértelmezett állapotszolgáltató hello `Persisted` beállítás van ez a viselkedés.

Állapot szabványos használatával kérheti le *beolvasása* művelet, amelynek jelez `KeyNotFoundException`(C#) vagy `NoSuchElementException`(Java), ha a bejegyzés nem létezik hello kulcs:

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

Állapotának beolvasására használatával egy *TryGet* módszerrel, amely nem lépett, ha a bejegyzés nem létezik a kulcs:

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

### <a name="saving-state"></a>Állapot mentése
hello állapotát kezelő lekérdezési módszerek tooan referenciaobjektum helyi memória adja vissza. Módosítja az objektumot a helyi memória önmagában nem okoz azt tartósan mentett toobe. Ha egy objektum hello állapotkezelője lekért és módosítani, azt kell lehet ismételten beszúrni objektumba hello állapotát kezelő toobe tartósan mentve.

Állapot is beszúrhat egy feltétel nélküli használatával *beállítása*, amely egyenértékű az hello van hello `dictionary["key"] = value` Szintaxis:

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

Állapot használatával adhat hozzá egy *Hozzáadás* metódust. Ez a módszer jelez `InvalidOperationException`(C#) vagy `IllegalStateException`(Java), amikor tooadd egy kulcs, amely már létezik.

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

Állapot segítségével is hozzáadhat egy *TryAdd* metódust. Ez a módszer nem lépett, amikor tooadd egy kulcs, amely már létezik.

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

Aktor metódus hello végén hello állapotkezelője automatikusan menti a hozzáadott vagy egy insert vagy az update művelet által módosított értékeket. A "Mentés" tárolásakor toodisk és a replikációs, attól függően, hogy a használt hello beállításokat is tartalmazhat. Nem módosult az értékeket nem megőrzött és replikálja. Ha nincs érték módosítva lett, a mentési művelet hello nincs hatása. Ha sikertelen a Mentés hello módosított állapota a rendszer elveti és hello eredeti állapot van töltve.

Mentheti is állapot manuálisan által hívó hello `SaveStateAsync` alapszintű hello szereplő metódust:

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

### <a name="removing-state"></a>Állapot eltávolítása
Megszüntetheti a állapot véglegesen egy szereplő állapotkezelője a hívó hello *eltávolítása* metódust. Ez a módszer jelez `KeyNotFoundException`(C#) vagy `NoSuchElementException`(Java), amikor tooremove egy kulcsot, amely nem létezik.

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

Akkor is eltávolíthatja állapot véglegesen hello *TryRemove* metódust. Ez a módszer nem lépett, amikor tooremove egy kulcs, amely nem létezik.

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

## <a name="next-steps"></a>Következő lépések

A Reliable Actors tárolt állapotban kell előtt az írásbeli toodisk szerializált és replikálja a magas rendelkezésre állás. További információ [szereplő típus szerializálási](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

A következő további információ [szereplő diagnosztika és teljesítményfigyelés](service-fabric-reliable-actors-diagnostics.md).
