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
# <a name="actor-timers-and-reminders"></a><span data-ttu-id="c1215-103">Aktor időzítők és az emlékeztetők</span><span class="sxs-lookup"><span data-stu-id="c1215-103">Actor timers and reminders</span></span>
<span data-ttu-id="c1215-104">Szereplője időzítők vagy emlékeztetők regisztrálásával ütemezhet rendszeres munkát saját magukat.</span><span class="sxs-lookup"><span data-stu-id="c1215-104">Actors can schedule periodic work on themselves by registering either timers or reminders.</span></span> <span data-ttu-id="c1215-105">Ez a cikk bemutatja, hogyan toouse időzítők és az emlékeztetők és hello különbségeket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="c1215-105">This article shows how toouse timers and reminders and explains hello differences between them.</span></span>

## <a name="actor-timers"></a><span data-ttu-id="c1215-106">Aktor időzítők</span><span class="sxs-lookup"><span data-stu-id="c1215-106">Actor timers</span></span>
<span data-ttu-id="c1215-107">Aktor időzítők adja meg a .NET vagy Java időzítő tooensure egyszerű csomagolásának, hogy hello visszahívási módszerek tiszteletben hello kapcsolja-alapú feldolgozási biztosítja, hogy az hello szereplője futásidejű biztosít.</span><span class="sxs-lookup"><span data-stu-id="c1215-107">Actor timers provide a simple wrapper around a .NET or Java timer tooensure that hello callback methods respect hello turn-based concurrency guarantees that hello Actors runtime provides.</span></span>

<span data-ttu-id="c1215-108">Szereplője használható hello `RegisterTimer`(C#) vagy `registerTimer`(Java) és `UnregisterTimer`(C#) vagy `unregisterTimer`az Alap (Java) metódusai tooregister osztály, és azok időzítők törlése.</span><span class="sxs-lookup"><span data-stu-id="c1215-108">Actors can use hello `RegisterTimer`(C#) or `registerTimer`(Java)  and `UnregisterTimer`(C#) or `unregisterTimer`(Java) methods on their base class tooregister and unregister their timers.</span></span> <span data-ttu-id="c1215-109">hello az alábbi példában hello időzítő API-k használatát.</span><span class="sxs-lookup"><span data-stu-id="c1215-109">hello example below shows hello use of timer APIs.</span></span> <span data-ttu-id="c1215-110">API-k hello nagyon hasonló toohello .NET időzítés vagy Java időzítő.</span><span class="sxs-lookup"><span data-stu-id="c1215-110">hello APIs are very similar toohello .NET timer or Java timer.</span></span> <span data-ttu-id="c1215-111">Ebben a példában, amikor hello időzítő esedékes, hello szereplője futásidejű szolgáltatás fel fogja hívni hello `MoveObject`(C#) vagy `moveObject`(Java) metódust.</span><span class="sxs-lookup"><span data-stu-id="c1215-111">In this example, when hello timer is due, hello Actors runtime will call hello `MoveObject`(C#) or `moveObject`(Java) method.</span></span> <span data-ttu-id="c1215-112">hello metódus garantáltan toorespect hello kapcsolja-alapú feldolgozási.</span><span class="sxs-lookup"><span data-stu-id="c1215-112">hello method is guaranteed toorespect hello turn-based concurrency.</span></span> <span data-ttu-id="c1215-113">Ez azt jelenti, hogy más szereplő módszerek vagy időzítő/emlékeztető visszahívások nem lesz folyamatban van a visszahívás végrehajtása befejezéséig.</span><span class="sxs-lookup"><span data-stu-id="c1215-113">This means that no other actor methods or timer/reminder callbacks will be in progress until this callback completes execution.</span></span>

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

<span data-ttu-id="c1215-114">hello hello időzítő következő időszak hello visszahívás végrehajtása után elindul.</span><span class="sxs-lookup"><span data-stu-id="c1215-114">hello next period of hello timer starts after hello callback completes execution.</span></span> <span data-ttu-id="c1215-115">Ez azt jelenti, hogy hello időzítő leáll, miközben hello visszahívási metódus végrehajtása történik, és hello visszahívás befejezése után elindul.</span><span class="sxs-lookup"><span data-stu-id="c1215-115">This implies that hello timer is stopped while hello callback is executing and is started when hello callback finishes.</span></span>

<span data-ttu-id="c1215-116">hello szereplője futásidejű módosítások toohello szereplő állapotkezelője hello visszahívás befejezése után menti.</span><span class="sxs-lookup"><span data-stu-id="c1215-116">hello Actors runtime saves changes made toohello actor's State Manager when hello callback finishes.</span></span> <span data-ttu-id="c1215-117">Ha hiba lép fel hello állapotmentést, inaktiválja a szereplő objektum, és egy új példány aktív lesz.</span><span class="sxs-lookup"><span data-stu-id="c1215-117">If an error occurs in saving hello state, that actor object will be deactivated and a new instance will be activated.</span></span>

<span data-ttu-id="c1215-118">Összes időzítő le van állítva, ha hello szereplő inaktív szemétgyűjtés részeként.</span><span class="sxs-lookup"><span data-stu-id="c1215-118">All timers are stopped when hello actor is deactivated as part of garbage collection.</span></span> <span data-ttu-id="c1215-119">Nincs időzítő visszahívások ezt követően hívják.</span><span class="sxs-lookup"><span data-stu-id="c1215-119">No timer callbacks are invoked after that.</span></span> <span data-ttu-id="c1215-120">Hello szereplője futásidejű is, nem őriz meg hello időzítők inaktiválása előtt futó semmilyen információt.</span><span class="sxs-lookup"><span data-stu-id="c1215-120">Also, hello Actors runtime does not retain any information about hello timers that were running before deactivation.</span></span> <span data-ttu-id="c1215-121">Toohello szereplő tooregister mentése bármely időzítők a jövőbeli hello újraaktiválásakor szükséges.</span><span class="sxs-lookup"><span data-stu-id="c1215-121">It is up toohello actor tooregister any timers that it needs when it is reactivated in hello future.</span></span> <span data-ttu-id="c1215-122">További információkért lásd: hello szakasz a [szereplő szemétgyűjtés](service-fabric-reliable-actors-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="c1215-122">For more information, see hello section on [actor garbage collection](service-fabric-reliable-actors-lifecycle.md).</span></span>

## <a name="actor-reminders"></a><span data-ttu-id="c1215-123">Aktor emlékeztetők</span><span class="sxs-lookup"><span data-stu-id="c1215-123">Actor reminders</span></span>
<span data-ttu-id="c1215-124">Buborékemlékeztetők olyan mechanizmus tootrigger állandó visszahívások egy szereplő, a megadott időpontokban.</span><span class="sxs-lookup"><span data-stu-id="c1215-124">Reminders are a mechanism tootrigger persistent callbacks on an actor at specified times.</span></span> <span data-ttu-id="c1215-125">A funkció hasonló tootimers is.</span><span class="sxs-lookup"><span data-stu-id="c1215-125">Their functionality is similar tootimers.</span></span> <span data-ttu-id="c1215-126">De időzítők, eltérően emlékeztetők által kiváltott minden körülmények hello szereplő explicit módon regisztrációjának őket, vagy hello szereplő explicit módon törlődik.</span><span class="sxs-lookup"><span data-stu-id="c1215-126">But unlike timers, reminders are triggered under all circumstances until hello actor explicitly unregisters them or hello actor is explicitly deleted.</span></span> <span data-ttu-id="c1215-127">Pontosabban emlékeztetők által kiváltott szereplő deactivations és a feladatátvétel mert hello szereplője futásidejű továbbra is fennáll, hello szereplő emlékeztetők kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="c1215-127">Specifically, reminders are triggered across actor deactivations and failovers because hello Actors runtime persists information about hello actor's reminders.</span></span>

<span data-ttu-id="c1215-128">Emlékeztető tooregister, egy szereplő meghívja hello `RegisterReminderAsync` megadott hello alaposztályban, ahogy az alábbi példa hello módszer:</span><span class="sxs-lookup"><span data-stu-id="c1215-128">tooregister a reminder, an actor calls hello `RegisterReminderAsync` method provided on hello base class, as shown in hello following example:</span></span>

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

<span data-ttu-id="c1215-129">Ebben a példában `"Pay cell phone bill"` hello emlékeztető neve.</span><span class="sxs-lookup"><span data-stu-id="c1215-129">In this example, `"Pay cell phone bill"` is hello reminder name.</span></span> <span data-ttu-id="c1215-130">Ez a karakterlánc, amely szereplő használ hello toouniquely emlékeztető azonosításához.</span><span class="sxs-lookup"><span data-stu-id="c1215-130">This is a string that hello actor uses toouniquely identify a reminder.</span></span> <span data-ttu-id="c1215-131">`BitConverter.GetBytes(amountInDollars)`(C#) hello emlékeztető társított hello a környezetben.</span><span class="sxs-lookup"><span data-stu-id="c1215-131">`BitConverter.GetBytes(amountInDollars)`(C#) is hello context that is associated with hello reminder.</span></span> <span data-ttu-id="c1215-132">Azt fogja adhatók át hátsó toohello aktor egy argumentum toohello emlékeztető visszahívás, azaz `IRemindable.ReceiveReminderAsync`(C#) vagy `Remindable.receiveReminderAsync`(Java).</span><span class="sxs-lookup"><span data-stu-id="c1215-132">It will be passed back toohello actor as an argument toohello reminder callback, i.e. `IRemindable.ReceiveReminderAsync`(C#) or `Remindable.receiveReminderAsync`(Java).</span></span>

<span data-ttu-id="c1215-133">Buborékemlékeztetők használó szereplője meg kell valósítania az hello `IRemindable` csatoló, az alábbi hello példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="c1215-133">Actors that use reminders must implement hello `IRemindable` interface, as shown in hello example below.</span></span>

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

<span data-ttu-id="c1215-134">Emlékeztető kiváltásakor hello Reliable Actors futtatókörnyezet által aktivált hello `ReceiveReminderAsync`(C#) vagy `receiveReminderAsync`hello szereplő (Java) metódust.</span><span class="sxs-lookup"><span data-stu-id="c1215-134">When a reminder is triggered, hello Reliable Actors runtime will invoke hello  `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method on hello Actor.</span></span> <span data-ttu-id="c1215-135">Egy szereplő regisztrálhat több emlékeztetők, és hello `ReceiveReminderAsync`(C#) vagy `receiveReminderAsync`(Java) metódus meghívja Ha bármelyik azok megjelenését elindul.</span><span class="sxs-lookup"><span data-stu-id="c1215-135">An actor can register multiple reminders, and hello `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method is invoked when any of those reminders is triggered.</span></span> <span data-ttu-id="c1215-136">hello szereplő használata hello emlékeztető név toohello átadott `ReceiveReminderAsync`(C#) vagy `receiveReminderAsync`(Java) metódus toofigure milyen emlékeztető lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="c1215-136">hello actor can use hello reminder name that is passed in toohello `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method toofigure out which reminder was triggered.</span></span>

<span data-ttu-id="c1215-137">hello szereplője futásidejű menti hello szereplő állapota akkor hello `ReceiveReminderAsync`(C#) vagy `receiveReminderAsync`(Java) hívás befejezését.</span><span class="sxs-lookup"><span data-stu-id="c1215-137">hello Actors runtime saves hello actor's state when hello `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) call finishes.</span></span> <span data-ttu-id="c1215-138">Ha hiba lép fel hello állapotmentést, inaktiválja a szereplő objektum, és egy új példány aktív lesz.</span><span class="sxs-lookup"><span data-stu-id="c1215-138">If an error occurs in saving hello state, that actor object will be deactivated and a new instance will be activated.</span></span>

<span data-ttu-id="c1215-139">Emlékeztető toounregister, egy szereplő meghívja hello `UnregisterReminderAsync`(C#) vagy `unregisterReminderAsync`(Java) módszer, ahogy az alábbi példák hello.</span><span class="sxs-lookup"><span data-stu-id="c1215-139">toounregister a reminder, an actor calls hello `UnregisterReminderAsync`(C#) or `unregisterReminderAsync`(Java) method, as shown in hello examples below.</span></span>

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```
```Java
ActorReminder reminder = getReminder("Pay cell phone bill");
CompletableFuture reminderUnregistration = unregisterReminderAsync(reminder);
```

<span data-ttu-id="c1215-140">A fentiek hello `UnregisterReminderAsync`(C#) vagy `unregisterReminderAsync`(Java) metódus fogad el egy `IActorReminder`(C#) vagy `ActorReminder`(Java) felületet.</span><span class="sxs-lookup"><span data-stu-id="c1215-140">As shown above, hello `UnregisterReminderAsync`(C#) or `unregisterReminderAsync`(Java) method accepts an `IActorReminder`(C#) or `ActorReminder`(Java) interface.</span></span> <span data-ttu-id="c1215-141">hello szereplő alaposztály támogatja a `GetReminder`(C#) vagy `getReminder`(Java) metódus, amely használt tooretrieve hello `IActorReminder`(C#) vagy `ActorReminder`hello emlékeztető nevében történő (Java) felületet.</span><span class="sxs-lookup"><span data-stu-id="c1215-141">hello actor base class supports a `GetReminder`(C#) or `getReminder`(Java) method that can be used tooretrieve hello `IActorReminder`(C#) or `ActorReminder`(Java) interface by passing in hello reminder name.</span></span> <span data-ttu-id="c1215-142">Ez akkor hasznos, mivel hello szereplő nem kell toopersist hello `IActorReminder`(C#) vagy `ActorReminder`által eredményül adott hello (Java) felület `RegisterReminder`(C#) vagy `registerReminder`(Java) metódus hívása.</span><span class="sxs-lookup"><span data-stu-id="c1215-142">This is convenient because hello actor does not need toopersist hello `IActorReminder`(C#) or `ActorReminder`(Java) interface that was returned from hello `RegisterReminder`(C#) or `registerReminder`(Java) method call.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1215-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c1215-143">Next Steps</span></span>
<span data-ttu-id="c1215-144">További tudnivalók a megbízható szereplő események és rögzítve:</span><span class="sxs-lookup"><span data-stu-id="c1215-144">Learn about Reliable Actor events and reentrancy:</span></span>
* [<span data-ttu-id="c1215-145">Szereplő események</span><span class="sxs-lookup"><span data-stu-id="c1215-145">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="c1215-146">Aktor rögzítve</span><span class="sxs-lookup"><span data-stu-id="c1215-146">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
