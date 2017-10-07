---
title: "aktor-alapú Azure mikroszolgáltatások életciklus aaaOverview |} Microsoft Docs"
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
ms.date: 06/13/2017
ms.author: amanbha
ms.openlocfilehash: a7926e372449048f0a579c2c58573754a4a82363
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a>Aktor életciklusát, automatikus szemétgyűjtés és manuális törlése
Egy szereplő aktív hello először kezdeményezték tooany metódusa. Egy szereplő az inaktív (szemétgyűjtési hello szereplője futtatókörnyezet által összegyűjtött), ha a konfigurált időtartamon nem használható. Egy szereplő és annak állapotát is törölhetők manuálisan bármikor.

## <a name="actor-activation"></a>Aktor aktiválás
Egy szereplő aktiválásakor hello következő történik:

* Ha a hívás érkezik egy szereplő számára, és egy már nem aktív, egy új szereplő jön létre.
* hello szereplő állapot be van töltve, ha az állapot karbantartása.
* Hello `OnActivateAsync` (C#) vagy `onActivateAsync` (Java) metódust (amely felülbírálhatók hello szereplő megvalósítása).
* hello szereplő most tekinthető aktív.

## <a name="actor-deactivation"></a>Aktor inaktiválása
Ha egy szereplő az Inaktiválás hello következő történik:

* Ha egy szereplő nem használják-e valamennyi ideje, az törlődik, hello aktív szereplője táblából.
* Hello `OnDeactivateAsync` (C#) vagy `onDeactivateAsync` (Java) metódust (amely felülbírálhatók hello szereplő megvalósítása). Ez törli a hello szereplő összes hello időmérőket. Szereplő műveletek, például a metódus nem hívható módosításokat állam.

> [!TIP]
> hello háló szereplője futásidejű megfelelően kibocsát egy [tooactor aktiválás és az inaktiválást kapcsolatos események](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters). A diagnosztika és teljesítményfigyelés hasznosak.
>
>

### <a name="actor-garbage-collection"></a>Aktor szemétgyűjtés
Amikor az Inaktiválás egy szereplő hivatkozások toohello szereplő objektum kiadott, és szemétgyűjtő általában hello közös nyelvi futtatókörnyezet (CLR) vagy a java virtuális gép (JVM) szemétgyűjtő. A szemétgyűjtés csak a szükségtelenné vált hello szereplő objektumot. létezik **nem** hello szereplő állapotkezelője tárolt állapot eltávolítása. hello tovább idő hello szereplő aktiválva van, egy új szereplő objektum jön létre, és az állapot visszaállítása megtörtént.

Mi számít "használatban" hello célját, valamint az inaktiválást szemétgyűjtés?

* Hívás fogadása
* `IRemindable.ReceiveReminderAsync`(csak akkor, ha a hello szereplő használ emlékeztetők alkalmazható) meghívott metódus

> [!NOTE]
> hello szereplő időzítők használ, és időzítő visszahívási meghívták, ha az nem **nem** mint "használják" száma.
>
>

Mielőtt megnyitjuk hello deaktiválása részleteit, a következő feltételek fontos toodefine hello:

* *Beolvasás időköze*. Ez hello időköz, mely hello szereplője futásidejű az aktív szereplője tábla keres, amely inaktiválhatók szereplője és szemétgyűjtés. hello alapértelmezett ennek értéke 1 perc.
* *Üresjárati időtúllépés*. Ez az, hogy egy szereplő kell tooremain nem használt hello időt (inaktív), mielőtt kikapcsolható és szemétgyűjtés. hello alapértelmezett ennek értéke 60 perc.

Általában nem szükséges toochange ezeket az alapértelmezett értékeket. Azonban, ha szükséges, intervallumok módosítható keresztül `ActorServiceSettings` regisztrálásakor a [szereplő szolgáltatás](service-fabric-reliable-actors-platform.md):

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
Az egyes aktív szereplő hello szereplő futásidejű nyomon követi az hello, hogy mennyi ideig lett inaktív (tehát nem használható). hello szereplő futásidejű ellenőrzi, hello szereplője minden `ScanIntervalInSeconds` toosee, ha azok szemétgyűjtési gyűjt, és azt gyűjti, ha üresjárati idő le lett `IdleTimeoutInSeconds`.

Bármikor egy szereplő használata esetén az üresjárati idő a visszaállítási too0. Ezt követően hello szereplő szemétgyűjtő, csak ha újra marad a tétlen lehet `IdleTimeoutInSeconds`. Visszahívása, hogy egy szereplő tekinthető toohave lett alkalmazva, ha egy szereplő illesztőfelület-metódust vagy egy aktor emlékeztető visszahívás végrehajtása. Egy szereplő van **nem** toohave tekinthető lett alkalmazva, ha az időzítő visszahívás végrehajtása.

hello alábbi ábrán egy egyetlen szereplő tooillustrate hello életciklusát ezekről a fogalmakról.

![Üresjárati idő – példa][1]

hello példa metódus aktorhívások, emlékeztető és időzítőket hello hatását a szereplő hello élettartama mutatja be. pontokra vonatkozó hello például a következő hello érdemes megemlíteni a következők:

* ScanInterval és IdleTimeout beállítása too5 és 10 kulcsattribútumokkal. (Egység nem számít, itt, mert a cél csak tooillustrate hello koncepció.)
* a toobe szemétgyűjtés történik T = 0, 5, 10, 15, 20, 25, a hello vizsgálat időköz 5 által definiált szereplője hello vizsgálatát.
* Egy rendszeres számlálót: T = 4, 8, 12, 16, 20, 24, következik be, és a visszahívás végrehajtása. Üresjárati idő hello hello szereplő nem befolyásolja.
* Az aktor metódus hívását T = 7 hello üresjárati idő too0 visszaállítja, és hello szemétgyűjtés hello szereplő késlelteti.
* Az aktor emlékeztető visszahívási mindennap T = 14, és további késések hello hello szereplő szemétgyűjtés.
* Hello szemétgyűjtési gyűjtemény vizsgálat T = 25 során hello szereplő üresjárati idő végül meghaladja a 10 hello üresjárati időkorlátját, és hello szereplő szemétgyűjtő.

Egy szereplő soha nem lesznek szemétgyűjtő, végrehajtja a módszerekkel, függetlenül attól, mennyi idő telhet el az adott metódus végrehajtása közben. A korábban említett aktor a felület metódusai és felszólítás visszahívások hello végrehajtásának megakadályozza, hogy a szemétgyűjtés hello szereplő üresjárati idő too0 alaphelyzetbe állításával. időzítő visszahívások hello végrehajtása nem állítja alaphelyzetbe a hello üresjárati idő too0. Hello szemétgyűjtés hello szereplő késleltetve azonban amíg hello időzítő visszahívás végrehajtása befejeződött.

## <a name="deleting-actors-and-their-state"></a>Szereplője és az állapot törlése
A deaktivált szereplője szemétgyűjtés csak a szükségtelenné vált hello szereplő objektum, de nem távolítja el egy szereplő állapotkezelője tárolt adatokat. Újra aktivált egy szereplő esetén az adatok újra hello állapotkezelője keresztül elérhető tooit válnak. Azokban az esetekben, ahol szereplője adat tárolása állapotkezelője és inaktiválása, de soha nem újra aktiválni az adatok szükséges tooclean lehet.

Hello [szereplő szolgáltatás](service-fabric-reliable-actors-platform.md) függvény biztosít távoli hívó szereplője törlése:

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

Attól függően, hogy e hello szereplő jelenleg aktív hatások következő hello törlése egy szereplő rendelkezik:

* **Aktív szereplő**
  * Aktor aktív szereplője lista törlődik, és inaktív.
  * Állapotában véglegesen törlődik.
* **Inaktív szereplő**
  * Állapotában véglegesen törlődik.

Vegye figyelembe, hogy nem hívható meg egy szereplő törlése magát az aktor módszerek egyikét a, mert hello szereplő nem lehet törölni az aktor hívás környezetben, mely hello a futásidejű kapott körül hello szereplő hívás tooenforce egyszálas hozzáférés a zárolási végrehajtása közben.

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
