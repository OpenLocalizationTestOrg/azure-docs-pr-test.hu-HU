---
title: "Aktor-alapú Azure mikroszolgáltatások-életciklus áttekintése |} Microsoft Docs"
description: "Ismerteti a Service Fabric megbízható szereplő életciklus, szemétgyűjtés és manuális törlése szereplője és azok állapota"
services: service-fabric
documentationcenter: .net
author: amanbha
manager: timlt
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/06/2017
ms.author: amanbha
ms.openlocfilehash: d49afd9e5cfe80ddc2d919c76eaa0cb168280c15
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/13/2017
---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a>Aktor életciklusát, automatikus szemétgyűjtés és manuális törlése
Egy szereplő először egy kezdeményezték bármelyik metódusa aktiválva. Egy szereplő az inaktív (szemétgyűjtési a szereplője futtatókörnyezet által összegyűjtött), ha a konfigurált időtartamon nem használható. Egy szereplő és annak állapotát is törölhetők manuálisan bármikor.

## <a name="actor-activation"></a>Aktor aktiválás
Amikor egy szereplő aktív, az alábbiak történnek:

* Ha a hívás érkezik egy szereplő számára, és egy már nem aktív, egy új szereplő jön létre.
* A aktorállapot be van töltve, ha az állapot karbantartása.
* A `OnActivateAsync` (C#) vagy `onActivateAsync` (Java) metódust (amely felülbírálhatók szereplő megvalósítása).
* Az aktor most tekinthető aktív.

## <a name="actor-deactivation"></a>Aktor inaktiválása
Amikor egy szereplő az Inaktiválás az alábbiak történnek:

* Amikor egy szereplő nem használják-e valamennyi ideje, a rendszer eltávolítja a aktív szereplője táblából.
* A `OnDeactivateAsync` (C#) vagy `onDeactivateAsync` (Java) metódust (amely felülbírálhatók szereplő megvalósítása). Ez törli a szereplő összes időzítő. Szereplő műveletek, például a metódus nem hívható módosításokat állam.

> [!TIP]
> A háló szereplője futásidejű megfelelően kibocsát egy [szereplő aktiválás és az inaktiválást kapcsolatos események](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters). A diagnosztika és teljesítményfigyelés hasznosak.
>
>

### <a name="actor-garbage-collection"></a>Aktor szemétgyűjtés
Amikor egy szereplő az Inaktiválás kiadott szereplő objektum hivatkozik, és szemétgyűjtő általában a közös nyelvi futtatókörnyezet (CLR) vagy a java virtuális gép (JVM) típusú szemétgyűjtő. A szemétgyűjtés csak a szükségtelenné vált az aktor objektumot. létezik **nem** távolítsa el a szereplő állapotkezelője tárolt állapotát. A szereplő aktiválódik, amikor legközelebb új szereplő objektum jön létre, és az állapot visszaállítása megtörtént.

Mi számít "használatban" inaktiválási és szemétgyűjtés?

* Hívás fogadása
* `IRemindable.ReceiveReminderAsync`(csak akkor, ha a szereplő használ emlékeztetők alkalmazható) meghívott metódus

> [!NOTE]
> az aktor időzítők használ, és időzítő visszahívási meghívták, ha az nem **nem** mint "használják" száma.
>
>

Előtt megnyitjuk inaktiválás részleteit, fontos, hogy adja meg a következő feltételeket:

* *Beolvasás időköze*. Ez az az időköz, amellyel a szereplője futásidejű az aktív szereplője tábla keres, amely inaktiválhatók szereplője és szemétgyűjtés. Ez az alapértelmezett értéke 1 perc.
* *Üresjárati időtúllépés*. Ez az az időtartam, amelyet egy szereplő használaton kívül hagyni (inaktív), mielőtt kikapcsolható és szemétgyűjtés. Ennek alapértelmezett értéke 60 perc.

Általában nem kell módosítania a ezeket az alapértelmezett értékeket. Azonban, ha szükséges, intervallumok módosítható keresztül `ActorServiceSettings` regisztrálásakor a [szereplő szolgáltatás](service-fabric-reliable-actors-platform.md):

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        ActorRuntime.RegisterActorAsync<MyActor>((context, actorType) =>
                new ActorService(context, actorType,
                    settings:
                        new ActorServiceSettings()
                        {
                            ActorGarbageCollectionSettings =
                                new ActorGarbageCollectionSettings(10, 2)
                        }))
            .GetAwaiter()
            .GetResult();
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
    }
}
```
Az egyes aktív szereplő szereplő futásidejű nyomon követi az az, hogy mennyi ideig lett inaktív (tehát nem használható). Az aktor futásidejű ellenőrzi, a gyakrabban minden `ScanIntervalInSeconds` megjelenítéséhez, ha azok szemétgyűjtési gyűjt, és azt gyűjti, ha üresjárati idő le lett `IdleTimeoutInSeconds`.

Bármikor egy szereplő használata esetén az üresjárati idő 0 lesz visszaállítva. Ezt követően a szereplő szemétgyűjtő, csak ha újra marad a tétlen lehet `IdleTimeoutInSeconds`. A visszaírási, hogy egy szereplő tekinthető fel lett használva, ha egy szereplő illesztőfelület-metódust vagy egy aktor emlékeztető visszahívás végrehajtása. Egy szereplő van **nem** fel lett használva, ha időzítő visszahívási tekinthető.

Az alábbi ábrán egy egyetlen szereplő ezek a fogalmak szemléltetésére életciklusát.

![Üresjárati idő – példa][1]

A példa a szereplő élettartama metódus aktorhívások, emlékeztető és időzítők hatását mutatja. A példa kapcsolatos következő szempontokat érdemes megemlíteni a következők:

* ScanInterval és IdleTimeout értéke 5 és 10 kulcsattribútumokkal. (Egység nem számít, itt, mivel az a célja, hogy csak a koncepció bemutatására.)
* A vizsgálat a szemétgyűjtésbe kerülése szereplőket T = 0, 5, 10, 15, 20, 25, a történik, a vizsgálat időköznek 5 által definiált.
* Egy rendszeres számlálót: T = 4, 8, 12, 16, 20, 24, következik be, és a visszahívás végrehajtása. Nem befolyásolja a szereplő üresjárati idő.
* Az aktor metódus hívását T = 7 alaphelyzetbe állítja az üresjárati idő 0-ra, és a szemétgyűjtés, a szereplő késlelteti.
* Az aktor emlékeztető visszahívási T = 14 mindennap, és további késlelteti a szemétgyűjtés, a szereplő.
* T = 25 a szemétgyűjtési gyűjtemény ellenőrzés során a szereplő üresjárati idő végül meghaladja a 10 üresjárati időkorlátját, és a szereplő szemétgyűjtő.

Egy szereplő soha nem lesznek szemétgyűjtő, végrehajtja a módszerekkel, függetlenül attól, mennyi idő telhet el az adott metódus végrehajtása közben. A korábban említett aktor a felület metódusai és felszólítás visszahívások végrehajtása megakadályozza, hogy a szemétgyűjtés által a gyári beállítások visszaállításával a szereplő üresjárati idő 0. Időzítő visszahívások végrehajtása nem állítja alaphelyzetbe az üresjárati idő 0. A szemétgyűjtés, a szereplő késleltetve azonban csak a időzítő visszahívási végrehajtásának befejezése után.

## <a name="deleting-actors-and-their-state"></a>Szereplője és az állapot törlése
A deaktivált szereplője szemétgyűjtés csak a szükségtelenné vált az aktor objektum, de nem távolítja el egy szereplő állapotkezelője tárolt adatokat. Ha egy szereplő újra aktivált, az adatok újra keresztül teszik elérhetővé hozzá állapotkezelő. Olyan esetekben, ahol szereplője adat tárolása állapotkezelője és inaktiválása, de soha nem újra aktiválni lehet szükség az adatok törlése.

A [szereplő szolgáltatás](service-fabric-reliable-actors-platform.md) függvény biztosít távoli hívó szereplője törlése:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

A következő hatásai vannak, attól függően, hogy-e a szereplő jelenleg aktív törlése egy szereplő rendelkezik:

* **Aktív szereplő**
  * Aktor aktív szereplője lista törlődik, és inaktív.
  * Állapotában véglegesen törlődik.
* **Inaktív szereplő**
  * Állapotában véglegesen törlődik.

Vegye figyelembe, hogy nem hívható meg egy szereplő törli a magát az aktor módszerek egyikét a szereplő, amelyben a futtatókörnyezet kapott a zárolási körül szereplő hívása egyszálas hozzáférés kényszerítésére az aktor hívás környezeten belül végrehajtása során nem törölhető, mert.

## <a name="next-steps"></a>Következő lépések
* [Aktor időzítők és az emlékeztetők](service-fabric-reliable-actors-timers-reminders.md)
* [Szereplő események](service-fabric-reliable-actors-events.md)
* [Aktor rögzítve](service-fabric-reliable-actors-reentrancy.md)
* [Aktor diagnosztika és teljesítményfigyelés](service-fabric-reliable-actors-diagnostics.md)
* [Aktor API referenciadokumentációt](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# mintakód](https://github.com/Azure/servicefabric-samples)
* [Java mintakód](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
