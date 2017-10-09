---
title: "aaaReliable szereplője időzítők és az emlékeztetők |} Microsoft Docs"
description: "Bevezetés tootimers és a Service Fabric Reliable Actors emlékeztetők."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 00c48716-569e-4a64-bd6c-25234c85ff4f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: c5116ec1923014e131130b9f4e86dd1e133bbf7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="actor-timers-and-reminders"></a>Aktor időzítők és az emlékeztetők
Szereplője időzítők vagy emlékeztetők regisztrálásával ütemezhet rendszeres munkát saját magukat. Ez a cikk bemutatja, hogyan toouse időzítők és az emlékeztetők és hello különbségeket ismerteti.

## <a name="actor-timers"></a>Aktor időzítők
Aktor időzítők adja meg a .NET vagy Java időzítő tooensure egyszerű csomagolásának, hogy hello visszahívási módszerek tiszteletben hello kapcsolja-alapú feldolgozási biztosítja, hogy az hello szereplője futásidejű biztosít.

Szereplője használható hello `RegisterTimer`(C#) vagy `registerTimer`(Java) és `UnregisterTimer`(C#) vagy `unregisterTimer`az Alap (Java) metódusai tooregister osztály, és azok időzítők törlése. hello az alábbi példában hello időzítő API-k használatát. API-k hello nagyon hasonló toohello .NET időzítés vagy Java időzítő. Ebben a példában, amikor hello időzítő esedékes, hello szereplője futásidejű szolgáltatás fel fogja hívni hello `MoveObject`(C#) vagy `moveObject`(Java) metódust. hello metódus garantáltan toorespect hello kapcsolja-alapú feldolgozási. Ez azt jelenti, hogy más szereplő módszerek vagy időzítő/emlékeztető visszahívások nem lesz folyamatban van a visszahívás végrehajtása befejezéséig.

```csharp
class VisualObjectActor : Actor, IVisualObject
{
    private IActorTimer _updateTimer;

    public VisualObjectActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    protected override Task OnActivateAsync()
    {
        ...

        _updateTimer = RegisterTimer(
            MoveObject,                     // Callback method
            null,                           // Parameter toopass toohello callback method
            TimeSpan.FromMilliseconds(15),  // Amount of time toodelay before hello callback is invoked
            TimeSpan.FromMilliseconds(15)); // Time interval between invocations of hello callback method

        return base.OnActivateAsync();
    }

    protected override Task OnDeactivateAsync()
    {
        if (_updateTimer != null)
        {
            UnregisterTimer(_updateTimer);
        }

        return base.OnDeactivateAsync();
    }

    private Task MoveObject(object state)
    {
        ...
        return Task.FromResult(true);
    }
}
```
```Java
public class VisualObjectActorImpl extends FabricActor implements VisualObjectActor
{
    private ActorTimer updateTimer;

    public VisualObjectActorImpl(FabricActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    @Override
    protected CompletableFuture onActivateAsync()
    {
        ...

        return this.stateManager()
                .getOrAddStateAsync(
                        stateName,
                        VisualObject.createRandom(
                                this.getId().toString(),
                                new Random(this.getId().toString().hashCode())))
                .thenApply((r) -> {
                    this.registerTimer(
                            (o) -> this.moveObject(o),                        // Callback method
                            "moveObject",
                            null,                                             // Parameter toopass toohello callback method
                            Duration.ofMillis(10),                            // Amount of time toodelay before hello callback is invoked
                            Duration.ofMillis(timerIntervalInMilliSeconds));  // Time interval between invocations of hello callback method
                    return null;
                });
    }

    @Override
    protected CompletableFuture onDeactivateAsync()
    {
        if (updateTimer != null)
        {
            unregisterTimer(updateTimer);
        }

        return super.onDeactivateAsync();
    }

    private CompletableFuture moveObject(Object state)
    {
        ...
        return this.stateManager().getStateAsync(this.stateName).thenCompose(v -> {
            VisualObject v1 = (VisualObject)v;
            v1.move();
            return (CompletableFuture<?>)this.stateManager().setStateAsync(stateName, v1).
                    thenApply(r -> {
                      ...
                      return null;});
        });
    }
}
```

hello hello időzítő következő időszak hello visszahívás végrehajtása után elindul. Ez azt jelenti, hogy hello időzítő leáll, miközben hello visszahívási metódus végrehajtása történik, és hello visszahívás befejezése után elindul.

hello szereplője futásidejű módosítások toohello szereplő állapotkezelője hello visszahívás befejezése után menti. Ha hiba lép fel hello állapotmentést, inaktiválja a szereplő objektum, és egy új példány aktív lesz.

Összes időzítő le van állítva, ha hello szereplő inaktív szemétgyűjtés részeként. Nincs időzítő visszahívások ezt követően hívják. Hello szereplője futásidejű is, nem őriz meg hello időzítők inaktiválása előtt futó semmilyen információt. Toohello szereplő tooregister mentése bármely időzítők a jövőbeli hello újraaktiválásakor szükséges. További információkért lásd: hello szakasz a [szereplő szemétgyűjtés](service-fabric-reliable-actors-lifecycle.md).

## <a name="actor-reminders"></a>Aktor emlékeztetők
Buborékemlékeztetők olyan mechanizmus tootrigger állandó visszahívások egy szereplő, a megadott időpontokban. A funkció hasonló tootimers is. De időzítők, eltérően emlékeztetők által kiváltott minden körülmények hello szereplő explicit módon regisztrációjának őket, vagy hello szereplő explicit módon törlődik. Pontosabban emlékeztetők által kiváltott szereplő deactivations és a feladatátvétel mert hello szereplője futásidejű továbbra is fennáll, hello szereplő emlékeztetők kapcsolatos információkat.

Emlékeztető tooregister, egy szereplő meghívja hello `RegisterReminderAsync` megadott hello alaposztályban, ahogy az alábbi példa hello módszer:

```csharp
protected override async Task OnActivateAsync()
{
    string reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    IActorReminder reminderRegistration = await this.RegisterReminderAsync(
        reminderName,
        BitConverter.GetBytes(amountInDollars),
        TimeSpan.FromDays(3),
        TimeSpan.FromDays(1));
}
```

```Java
@Override
protected CompletableFuture onActivateAsync()
{
    String reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    ActorReminder reminderRegistration = this.registerReminderAsync(
            reminderName,
            state,
            dueTime,    //hello amount of time toodelay before firing hello reminder
            period);    //hello time interval between firing of reminders
}
```

Ebben a példában `"Pay cell phone bill"` hello emlékeztető neve. Ez a karakterlánc, amely szereplő használ hello toouniquely emlékeztető azonosításához. `BitConverter.GetBytes(amountInDollars)`(C#) hello emlékeztető társított hello a környezetben. Azt fogja adhatók át hátsó toohello aktor egy argumentum toohello emlékeztető visszahívás, azaz `IRemindable.ReceiveReminderAsync`(C#) vagy `Remindable.receiveReminderAsync`(Java).

Buborékemlékeztetők használó szereplője meg kell valósítania az hello `IRemindable` csatoló, az alábbi hello példában látható módon.

```csharp
public class ToDoListActor : Actor, IToDoListActor, IRemindable
{
    public ToDoListActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task ReceiveReminderAsync(string reminderName, byte[] context, TimeSpan dueTime, TimeSpan period)
    {
        if (reminderName.Equals("Pay cell phone bill"))
        {
            int amountToPay = BitConverter.ToInt32(context, 0);
            System.Console.WriteLine("Please pay your cell phone bill of ${0}!", amountToPay);
        }
        return Task.FromResult(true);
    }
}
```
```Java
public class ToDoListActorImpl extends FabricActor implements ToDoListActor, Remindable
{
    public ToDoListActor(FabricActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture receiveReminderAsync(String reminderName, byte[] context, Duration dueTime, Duration period)
    {
        if (reminderName.equals("Pay cell phone bill"))
        {
            int amountToPay = ByteBuffer.wrap(context).getInt();
            System.out.println("Please pay your cell phone bill of " + amountToPay);
        }
        return CompletableFuture.completedFuture(true);
    }

```

Emlékeztető kiváltásakor hello Reliable Actors futtatókörnyezet által aktivált hello `ReceiveReminderAsync`(C#) vagy `receiveReminderAsync`hello szereplő (Java) metódust. Egy szereplő regisztrálhat több emlékeztetők, és hello `ReceiveReminderAsync`(C#) vagy `receiveReminderAsync`(Java) metódus meghívja Ha bármelyik azok megjelenését elindul. hello szereplő használata hello emlékeztető név toohello átadott `ReceiveReminderAsync`(C#) vagy `receiveReminderAsync`(Java) metódus toofigure milyen emlékeztető lett elindítva.

hello szereplője futásidejű menti hello szereplő állapota akkor hello `ReceiveReminderAsync`(C#) vagy `receiveReminderAsync`(Java) hívás befejezését. Ha hiba lép fel hello állapotmentést, inaktiválja a szereplő objektum, és egy új példány aktív lesz.

Emlékeztető toounregister, egy szereplő meghívja hello `UnregisterReminderAsync`(C#) vagy `unregisterReminderAsync`(Java) módszer, ahogy az alábbi példák hello.

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```
```Java
ActorReminder reminder = getReminder("Pay cell phone bill");
CompletableFuture reminderUnregistration = unregisterReminderAsync(reminder);
```

A fentiek hello `UnregisterReminderAsync`(C#) vagy `unregisterReminderAsync`(Java) metódus fogad el egy `IActorReminder`(C#) vagy `ActorReminder`(Java) felületet. hello szereplő alaposztály támogatja a `GetReminder`(C#) vagy `getReminder`(Java) metódus, amely használt tooretrieve hello `IActorReminder`(C#) vagy `ActorReminder`hello emlékeztető nevében történő (Java) felületet. Ez akkor hasznos, mivel hello szereplő nem kell toopersist hello `IActorReminder`(C#) vagy `ActorReminder`által eredményül adott hello (Java) felület `RegisterReminder`(C#) vagy `registerReminder`(Java) metódus hívása.

## <a name="next-steps"></a>Következő lépések
További tudnivalók a megbízható szereplő események és rögzítve:
* [Szereplő események](service-fabric-reliable-actors-events.md)
* [Aktor rögzítve](service-fabric-reliable-actors-reentrancy.md)
