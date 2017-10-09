---
title: "az aktor-alapú Azure mikroszolgáltatások aaaEvents |} Microsoft Docs"
description: "A Service Fabric Reliable Actors tooevents bemutatása."
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
ms.openlocfilehash: a51e41c35441a5fea508138968b36a35f0ba6699
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="actor-events"></a><span data-ttu-id="ff04c-103">Szereplő események</span><span class="sxs-lookup"><span data-stu-id="ff04c-103">Actor events</span></span>
<span data-ttu-id="ff04c-104">Szereplő események olyan módon toosend legjobb értesítések hello szereplő toohello ügyfelek számára.</span><span class="sxs-lookup"><span data-stu-id="ff04c-104">Actor events provide a way toosend best-effort notifications from hello actor toohello clients.</span></span> <span data-ttu-id="ff04c-105">Szereplő események szereplő ügyfél kommunikációhoz, és a aktor szereplő kommunikáció nem használható.</span><span class="sxs-lookup"><span data-stu-id="ff04c-105">Actor events are designed for actor-to-client communication and should not be used for actor-to-actor communication.</span></span>

<span data-ttu-id="ff04c-106">hello következő hogyan kódot részletek megjelenítése toouse szereplő események az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ff04c-106">hello following code snippets show how toouse actor events in your application.</span></span>

<span data-ttu-id="ff04c-107">Adja meg, amely leírja a hello aktor által közzétett hello események illesztőfelület.</span><span class="sxs-lookup"><span data-stu-id="ff04c-107">Define an interface that describes hello events published by hello actor.</span></span> <span data-ttu-id="ff04c-108">Ez az interfész típusból kell származtatni hello `IActorEvents` felületet.</span><span class="sxs-lookup"><span data-stu-id="ff04c-108">This interface must be derived from hello `IActorEvents` interface.</span></span> <span data-ttu-id="ff04c-109">hello argumentumok hello módszerek [adategyezmény-szerializálható](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="ff04c-109">hello arguments of hello methods must be [data contract serializable](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span></span> <span data-ttu-id="ff04c-110">hello módszerek void értéket kell visszaadnia, és eseményként értesítések egy- és a lehető legkedvezőbb módon.</span><span class="sxs-lookup"><span data-stu-id="ff04c-110">hello methods must return void, as event notifications are one way and best effort.</span></span>

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
<span data-ttu-id="ff04c-111">Hello szereplő hello szereplő felületen által közzétett hello események deklarálható.</span><span class="sxs-lookup"><span data-stu-id="ff04c-111">Declare hello events published by hello actor in hello actor interface.</span></span>

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
<span data-ttu-id="ff04c-112">Hello ügyféloldalon hello eseménykezelő megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="ff04c-112">On hello client side, implement hello event handler.</span></span>

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

<span data-ttu-id="ff04c-113">Hello ügyfélen hozzon létre egy proxy toohello szereplő hello esemény közzétevő és tooits események előfizetés.</span><span class="sxs-lookup"><span data-stu-id="ff04c-113">On hello client, create a proxy toohello actor that publishes hello event and subscribe tooits events.</span></span>

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

<span data-ttu-id="ff04c-114">Feladatátvétel hello esetben hello szereplő előfordulhat, hogy feladatátvételt tooa másik folyamat vagy csomópont.</span><span class="sxs-lookup"><span data-stu-id="ff04c-114">In hello event of failovers, hello actor may fail over tooa different process or node.</span></span> <span data-ttu-id="ff04c-115">hello szereplő proxy hello aktív előfizetések kezeli, és automatikusan újra előfizet rájuk.</span><span class="sxs-lookup"><span data-stu-id="ff04c-115">hello actor proxy manages hello active subscriptions and automatically re-subscribes them.</span></span> <span data-ttu-id="ff04c-116">Hello újbóli előfizetési időköz hello segítségével szabályozhatja `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API.</span><span class="sxs-lookup"><span data-stu-id="ff04c-116">You can control hello re-subscription interval through hello `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API.</span></span> <span data-ttu-id="ff04c-117">toounsubscribe, használjon hello `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.</span><span class="sxs-lookup"><span data-stu-id="ff04c-117">toounsubscribe, use hello `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.</span></span>

<span data-ttu-id="ff04c-118">A hello szereplő egyszerűen tegye közzé hello események, akkor fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="ff04c-118">On hello actor, simply publish hello events as they happen.</span></span> <span data-ttu-id="ff04c-119">Ha vannak előfizetők toohello esemény, hello szereplője futásidejű fogja őket hello értesítés küldése.</span><span class="sxs-lookup"><span data-stu-id="ff04c-119">If there are subscribers toohello event, hello Actors runtime will send them hello notification.</span></span>

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a><span data-ttu-id="ff04c-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ff04c-120">Next steps</span></span>
* [<span data-ttu-id="ff04c-121">Aktor rögzítve</span><span class="sxs-lookup"><span data-stu-id="ff04c-121">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="ff04c-122">Aktor diagnosztika és teljesítményfigyelés</span><span class="sxs-lookup"><span data-stu-id="ff04c-122">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="ff04c-123">Aktor API referenciadokumentációt</span><span class="sxs-lookup"><span data-stu-id="ff04c-123">Actor API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="ff04c-124">C# mintakód</span><span class="sxs-lookup"><span data-stu-id="ff04c-124">C# Sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="ff04c-125">C# .NET Core mintakód</span><span class="sxs-lookup"><span data-stu-id="ff04c-125">C# .NET Core Sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [<span data-ttu-id="ff04c-126">Java mintakód</span><span class="sxs-lookup"><span data-stu-id="ff04c-126">Java Sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)
