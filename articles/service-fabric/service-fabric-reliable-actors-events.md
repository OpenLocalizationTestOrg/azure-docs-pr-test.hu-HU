---
title: "Aktor-alapú Azure mikroszolgáltatások események |} Microsoft Docs"
description: "A Service Fabric Reliable Actors események bemutatása."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: aa01b0f7-8f88-403a-bfe1-5aba00312c24
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: amanbha
ms.openlocfilehash: d936670c548ff709fc2e935d3f28d94e4bde8a04
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="actor-events"></a><span data-ttu-id="f72c4-103">Szereplő események</span><span class="sxs-lookup"><span data-stu-id="f72c4-103">Actor events</span></span>
<span data-ttu-id="f72c4-104">Szereplő események teszik lehetővé a szereplő lehető legjobb értesítések küldése az ügyfelek számára.</span><span class="sxs-lookup"><span data-stu-id="f72c4-104">Actor events provide a way to send best-effort notifications from the actor to the clients.</span></span> <span data-ttu-id="f72c4-105">Szereplő események szereplő ügyfél kommunikációhoz, és a aktor szereplő kommunikáció nem használható.</span><span class="sxs-lookup"><span data-stu-id="f72c4-105">Actor events are designed for actor-to-client communication and should not be used for actor-to-actor communication.</span></span>

<span data-ttu-id="f72c4-106">Az alábbi kódtöredékek bemutatják, hogyan az alkalmazásban szereplő események használatára.</span><span class="sxs-lookup"><span data-stu-id="f72c4-106">The following code snippets show how to use actor events in your application.</span></span>

<span data-ttu-id="f72c4-107">Adja meg, amely leírja a szereplő által közzétett események illesztőfelület.</span><span class="sxs-lookup"><span data-stu-id="f72c4-107">Define an interface that describes the events published by the actor.</span></span> <span data-ttu-id="f72c4-108">Ez az interfész típusból kell származtatni a `IActorEvents` felületet.</span><span class="sxs-lookup"><span data-stu-id="f72c4-108">This interface must be derived from the `IActorEvents` interface.</span></span> <span data-ttu-id="f72c4-109">Az argumentumok a módszerek [adategyezmény-szerializálható](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="f72c4-109">The arguments of the methods must be [data contract serializable](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span></span> <span data-ttu-id="f72c4-110">A módszerek void értéket kell visszaadnia, és eseményként értesítések egy- és a lehető legkedvezőbb módon.</span><span class="sxs-lookup"><span data-stu-id="f72c4-110">The methods must return void, as event notifications are one way and best effort.</span></span>

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```
```Java
public interface GameEvents implements ActorEvents
{
    void gameScoreUpdated(UUID gameId, String currentScore);
}
```
<span data-ttu-id="f72c4-111">Az aktor felületen aktor által közzétett események deklarálható.</span><span class="sxs-lookup"><span data-stu-id="f72c4-111">Declare the events published by the actor in the actor interface.</span></span>

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```
```Java
public interface GameActor extends Actor, ActorEventPublisherE<GameEvents>
{
    CompletableFuture<?> updateGameStatus(GameStatus status);

    CompletableFuture<String> getGameScore();
}
```
<span data-ttu-id="f72c4-112">Az ügyféloldalon az eseménykezelő megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="f72c4-112">On the client side, implement the event handler.</span></span>

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

```Java
class GameEventsHandler implements GameEvents {
    public void gameScoreUpdated(UUID gameId, String currentScore)
    {
        System.out.println("Updates: Game: "+gameId+" ,Score: "+currentScore);
    }
}
```

<span data-ttu-id="f72c4-113">Az ügyfélen a közzétevő az esemény szereplő proxy létrehozására és az események előfizetni.</span><span class="sxs-lookup"><span data-stu-id="f72c4-113">On the client, create a proxy to the actor that publishes the event and subscribe to its events.</span></span>

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

<span data-ttu-id="f72c4-114">Esetén a feladatátvétel a szereplő előfordulhat, hogy átadja egy másik folyamat vagy a csomópont.</span><span class="sxs-lookup"><span data-stu-id="f72c4-114">In the event of failovers, the actor may fail over to a different process or node.</span></span> <span data-ttu-id="f72c4-115">Az aktor proxy az aktív előfizetések kezeli, és automatikusan újra előfizet rájuk.</span><span class="sxs-lookup"><span data-stu-id="f72c4-115">The actor proxy manages the active subscriptions and automatically re-subscribes them.</span></span> <span data-ttu-id="f72c4-116">Az újbóli előfizetési időköz keresztül szabályozhatja a `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API.</span><span class="sxs-lookup"><span data-stu-id="f72c4-116">You can control the re-subscription interval through the `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API.</span></span> <span data-ttu-id="f72c4-117">Mondhatja, használja a `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.</span><span class="sxs-lookup"><span data-stu-id="f72c4-117">To unsubscribe, use the `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.</span></span>

<span data-ttu-id="f72c4-118">Az aktor egyszerűen közzététele az eseményeket, akkor fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="f72c4-118">On the actor, simply publish the events as they happen.</span></span> <span data-ttu-id="f72c4-119">Az esemény-előfizetők esetén, a gyakrabban futtatókörnyezet fogja őket az értesítés elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="f72c4-119">If there are subscribers to the event, the Actors runtime will send them the notification.</span></span>

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a><span data-ttu-id="f72c4-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f72c4-120">Next steps</span></span>
* [<span data-ttu-id="f72c4-121">Aktor rögzítve</span><span class="sxs-lookup"><span data-stu-id="f72c4-121">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="f72c4-122">Aktor diagnosztika és teljesítményfigyelés</span><span class="sxs-lookup"><span data-stu-id="f72c4-122">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="f72c4-123">Aktor API referenciadokumentációt</span><span class="sxs-lookup"><span data-stu-id="f72c4-123">Actor API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="f72c4-124">C# mintakód</span><span class="sxs-lookup"><span data-stu-id="f72c4-124">C# Sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="f72c4-125">C# .NET Core mintakód</span><span class="sxs-lookup"><span data-stu-id="f72c4-125">C# .NET Core Sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [<span data-ttu-id="f72c4-126">Java mintakód</span><span class="sxs-lookup"><span data-stu-id="f72c4-126">Java Sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)
