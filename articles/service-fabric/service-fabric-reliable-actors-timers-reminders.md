---
title: "Megbízható szereplője időzítők és az emlékeztetők |} Microsoft Docs"
description: "Bevezetés a Service Fabric Reliable Actors időzítők és az emlékeztetők."
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
ms.openlocfilehash: 06b026ce06e0f16a77ac238de0af2263f272933c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="actor-timers-and-reminders"></a><span data-ttu-id="9ca1d-103">Aktor időzítők és az emlékeztetők</span><span class="sxs-lookup"><span data-stu-id="9ca1d-103">Actor timers and reminders</span></span>
<span data-ttu-id="9ca1d-104">Szereplője időzítők vagy emlékeztetők regisztrálásával ütemezhet rendszeres munkát saját magukat.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-104">Actors can schedule periodic work on themselves by registering either timers or reminders.</span></span> <span data-ttu-id="9ca1d-105">Ez a cikk bemutatja, hogyan használja a időzítők és az emlékeztetők, és a különbségeket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-105">This article shows how to use timers and reminders and explains the differences between them.</span></span>

## <a name="actor-timers"></a><span data-ttu-id="9ca1d-106">Aktor időzítők</span><span class="sxs-lookup"><span data-stu-id="9ca1d-106">Actor timers</span></span>
<span data-ttu-id="9ca1d-107">Aktor időzítők adja meg annak érdekében, hogy a visszahívási módszerek tiszteletben tartják a kapcsolja-alapú feldolgozási .NET vagy Java időzítő egyszerű csomagolásának garantálja, hogy a szereplője futtatókörnyezetet biztosít.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-107">Actor timers provide a simple wrapper around a .NET or Java timer to ensure that the callback methods respect the turn-based concurrency guarantees that the Actors runtime provides.</span></span>

<span data-ttu-id="9ca1d-108">Szereplője használhatja a `RegisterTimer`(C#) vagy `registerTimer`(Java) és `UnregisterTimer`(C#) vagy `unregisterTimer`(Java) módszerek regisztrálásához és azok időzítők az alaposztályban.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-108">Actors can use the `RegisterTimer`(C#) or `registerTimer`(Java)  and `UnregisterTimer`(C#) or `unregisterTimer`(Java) methods on their base class to register and unregister their timers.</span></span> <span data-ttu-id="9ca1d-109">Az alábbi példában látható időzítő API-k használatát.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-109">The example below shows the use of timer APIs.</span></span> <span data-ttu-id="9ca1d-110">Az API-k nagyon hasonlóak a .NET időzítés vagy Java időzítőt.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-110">The APIs are very similar to the .NET timer or Java timer.</span></span> <span data-ttu-id="9ca1d-111">Ebben a példában, amikor az időzítő esedékes, a gyakrabban futásidejű fel fogja hívni a `MoveObject`(C#) vagy `moveObject`(Java) metódust.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-111">In this example, when the timer is due, the Actors runtime will call the `MoveObject`(C#) or `moveObject`(Java) method.</span></span> <span data-ttu-id="9ca1d-112">A metódus garantáltan tiszteletben tartják a kapcsolja-alapú feldolgozási.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-112">The method is guaranteed to respect the turn-based concurrency.</span></span> <span data-ttu-id="9ca1d-113">Ez azt jelenti, hogy más szereplő módszerek vagy időzítő/emlékeztető visszahívások nem lesz folyamatban van a visszahívás végrehajtása befejezéséig.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-113">This means that no other actor methods or timer/reminder callbacks will be in progress until this callback completes execution.</span></span>

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
            null,                           // Parameter to pass to the callback method
            TimeSpan.FromMilliseconds(15),  // Amount of time to delay before the callback is invoked
            TimeSpan.FromMilliseconds(15)); // Time interval between invocations of the callback method

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
                            null,                                             // Parameter to pass to the callback method
                            Duration.ofMillis(10),                            // Amount of time to delay before the callback is invoked
                            Duration.ofMillis(timerIntervalInMilliSeconds));  // Time interval between invocations of the callback method
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

<span data-ttu-id="9ca1d-114">A következő időszak az időzítő elindítja a visszahívás végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-114">The next period of the timer starts after the callback completes execution.</span></span> <span data-ttu-id="9ca1d-115">Ez azt jelenti, hogy az időzítő leáll, miközben a visszahívás végrehajtása történik, és a visszahívás befejezése után elindult.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-115">This implies that the timer is stopped while the callback is executing and is started when the callback finishes.</span></span>

<span data-ttu-id="9ca1d-116">A szereplője futásidejű a visszahívás befejezése után a szereplő állapotkezelője végrehajtott módosítások mentése.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-116">The Actors runtime saves changes made to the actor's State Manager when the callback finishes.</span></span> <span data-ttu-id="9ca1d-117">Ha hiba lép fel az állapotmentést, inaktiválja a szereplő objektum, és egy új példány aktív lesz.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-117">If an error occurs in saving the state, that actor object will be deactivated and a new instance will be activated.</span></span>

<span data-ttu-id="9ca1d-118">Összes időzítő le van állítva, ha a szereplő inaktív szemétgyűjtés részeként.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-118">All timers are stopped when the actor is deactivated as part of garbage collection.</span></span> <span data-ttu-id="9ca1d-119">Nincs időzítő visszahívások ezt követően hívják.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-119">No timer callbacks are invoked after that.</span></span> <span data-ttu-id="9ca1d-120">A szereplője futásidejű is, nem őriz meg az időzítők inaktiválása előtt futó semmilyen információt.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-120">Also, the Actors runtime does not retain any information about the timers that were running before deactivation.</span></span> <span data-ttu-id="9ca1d-121">Esetén a szereplő bármely időzítők újraaktiválásakor azt a jövőben szükséges regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-121">It is up to the actor to register any timers that it needs when it is reactivated in the future.</span></span> <span data-ttu-id="9ca1d-122">További információkért lásd [szereplő szemétgyűjtés](service-fabric-reliable-actors-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="9ca1d-122">For more information, see the section on [actor garbage collection](service-fabric-reliable-actors-lifecycle.md).</span></span>

## <a name="actor-reminders"></a><span data-ttu-id="9ca1d-123">Aktor emlékeztetők</span><span class="sxs-lookup"><span data-stu-id="9ca1d-123">Actor reminders</span></span>
<span data-ttu-id="9ca1d-124">Emlékeztetők egy olyan mechanizmus való állandó visszahívások egy szereplő a következő alkalommal megadva.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-124">Reminders are a mechanism to trigger persistent callbacks on an actor at specified times.</span></span> <span data-ttu-id="9ca1d-125">Működés időzítők hasonlít.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-125">Their functionality is similar to timers.</span></span> <span data-ttu-id="9ca1d-126">De időzítők, eltérően emlékeztetők által kiváltott minden körülmények addig, amíg a szereplő explicit módon regisztrációjának őket, vagy a szereplő explicit módon törlődik.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-126">But unlike timers, reminders are triggered under all circumstances until the actor explicitly unregisters them or the actor is explicitly deleted.</span></span> <span data-ttu-id="9ca1d-127">Pontosabban emlékeztetők által kiváltott szereplő deactivations és a feladatátvétel, mert a szereplője futásidejű továbbra is fennáll, a szereplő emlékeztetők kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-127">Specifically, reminders are triggered across actor deactivations and failovers because the Actors runtime persists information about the actor's reminders.</span></span>

<span data-ttu-id="9ca1d-128">Emlékeztető regisztrálásához egy szereplő meghívja a `RegisterReminderAsync` metódus az alaposztályban, megadva a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="9ca1d-128">To register a reminder, an actor calls the `RegisterReminderAsync` method provided on the base class, as shown in the following example:</span></span>

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
            dueTime,    //The amount of time to delay before firing the reminder
            period);    //The time interval between firing of reminders
}
```

<span data-ttu-id="9ca1d-129">Ebben a példában `"Pay cell phone bill"` emlékeztető neve.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-129">In this example, `"Pay cell phone bill"` is the reminder name.</span></span> <span data-ttu-id="9ca1d-130">Ez a karakterlánc, amely a szereplő használatával egyedi módon azonosítja az egy emlékeztető az.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-130">This is a string that the actor uses to uniquely identify a reminder.</span></span> <span data-ttu-id="9ca1d-131">`BitConverter.GetBytes(amountInDollars)`(C#) a környezetben a felszólítás társított.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-131">`BitConverter.GetBytes(amountInDollars)`(C#) is the context that is associated with the reminder.</span></span> <span data-ttu-id="9ca1d-132">Az átkerülnek vissza a szereplő emlékeztető visszahívási argumentumaként azaz `IRemindable.ReceiveReminderAsync`(C#) vagy `Remindable.receiveReminderAsync`(Java).</span><span class="sxs-lookup"><span data-stu-id="9ca1d-132">It will be passed back to the actor as an argument to the reminder callback, i.e. `IRemindable.ReceiveReminderAsync`(C#) or `Remindable.receiveReminderAsync`(Java).</span></span>

<span data-ttu-id="9ca1d-133">Buborékemlékeztetők használó szereplője meg kell valósítania a `IRemindable` csatoló, az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-133">Actors that use reminders must implement the `IRemindable` interface, as shown in the example below.</span></span>

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

<span data-ttu-id="9ca1d-134">Emlékeztető kiváltásakor a Reliable Actors futtatókörnyezet által aktivált a `ReceiveReminderAsync`(C#) vagy `receiveReminderAsync`a szereplő (Java) metódust.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-134">When a reminder is triggered, the Reliable Actors runtime will invoke the  `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method on the Actor.</span></span> <span data-ttu-id="9ca1d-135">Egy szereplő regisztrálhat több emlékeztetők, és a `ReceiveReminderAsync`(C#) vagy `receiveReminderAsync`(Java) metódus meghívja Ha bármelyik azok megjelenését elindul.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-135">An actor can register multiple reminders, and the `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method is invoked when any of those reminders is triggered.</span></span> <span data-ttu-id="9ca1d-136">Az aktor is használja a a felszólítás kerül átadásra a `ReceiveReminderAsync`(C#) vagy `receiveReminderAsync`(Java) módszer mérje fel, melyik emlékeztető lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-136">The actor can use the reminder name that is passed in to the `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) method to figure out which reminder was triggered.</span></span>

<span data-ttu-id="9ca1d-137">A futásidejű menti a szereplő szereplője állapotba, ha a `ReceiveReminderAsync`(C#) vagy `receiveReminderAsync`(Java) hívás befejezését.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-137">The Actors runtime saves the actor's state when the `ReceiveReminderAsync`(C#) or `receiveReminderAsync`(Java) call finishes.</span></span> <span data-ttu-id="9ca1d-138">Ha hiba lép fel az állapotmentést, inaktiválja a szereplő objektum, és egy új példány aktív lesz.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-138">If an error occurs in saving the state, that actor object will be deactivated and a new instance will be activated.</span></span>

<span data-ttu-id="9ca1d-139">Emlékeztető regisztrációját, egy szereplő meghívja a `UnregisterReminderAsync`(C#) vagy `unregisterReminderAsync`(Java) metódust, az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-139">To unregister a reminder, an actor calls the `UnregisterReminderAsync`(C#) or `unregisterReminderAsync`(Java) method, as shown in the examples below.</span></span>

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```
```Java
ActorReminder reminder = getReminder("Pay cell phone bill");
CompletableFuture reminderUnregistration = unregisterReminderAsync(reminder);
```

<span data-ttu-id="9ca1d-140">A fent látható módon a `UnregisterReminderAsync`(C#) vagy `unregisterReminderAsync`(Java) metódus fogad el egy `IActorReminder`(C#) vagy `ActorReminder`(Java) felületet.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-140">As shown above, the `UnregisterReminderAsync`(C#) or `unregisterReminderAsync`(Java) method accepts an `IActorReminder`(C#) or `ActorReminder`(Java) interface.</span></span> <span data-ttu-id="9ca1d-141">Az aktor alaposztály támogatja a `GetReminder`(C#) vagy `getReminder`beolvasásához használható metódus (Java) a `IActorReminder`(C#) vagy `ActorReminder`emlékeztető nevében történő (Java) felületet.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-141">The actor base class supports a `GetReminder`(C#) or `getReminder`(Java) method that can be used to retrieve the `IActorReminder`(C#) or `ActorReminder`(Java) interface by passing in the reminder name.</span></span> <span data-ttu-id="9ca1d-142">Ez akkor hasznos, mivel a szereplő nem kell megőrizni a `IActorReminder`(C#) vagy `ActorReminder`által eredményül adott (Java) felületet a `RegisterReminder`(C#) vagy `registerReminder`(Java) metódus hívása.</span><span class="sxs-lookup"><span data-stu-id="9ca1d-142">This is convenient because the actor does not need to persist the `IActorReminder`(C#) or `ActorReminder`(Java) interface that was returned from the `RegisterReminder`(C#) or `registerReminder`(Java) method call.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ca1d-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ca1d-143">Next Steps</span></span>
<span data-ttu-id="9ca1d-144">További tudnivalók a megbízható szereplő események és rögzítve:</span><span class="sxs-lookup"><span data-stu-id="9ca1d-144">Learn about Reliable Actor events and reentrancy:</span></span>
* [<span data-ttu-id="9ca1d-145">Szereplő események</span><span class="sxs-lookup"><span data-stu-id="9ca1d-145">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="9ca1d-146">Aktor rögzítve</span><span class="sxs-lookup"><span data-stu-id="9ca1d-146">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
