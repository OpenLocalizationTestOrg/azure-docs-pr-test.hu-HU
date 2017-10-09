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
# <a name="actor-events"></a>Szereplő események
Szereplő események olyan módon toosend legjobb értesítések hello szereplő toohello ügyfelek számára. Szereplő események szereplő ügyfél kommunikációhoz, és a aktor szereplő kommunikáció nem használható.

hello következő hogyan kódot részletek megjelenítése toouse szereplő események az alkalmazásban.

Adja meg, amely leírja a hello aktor által közzétett hello események illesztőfelület. Ez az interfész típusból kell származtatni hello `IActorEvents` felületet. hello argumentumok hello módszerek [adategyezmény-szerializálható](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). hello módszerek void értéket kell visszaadnia, és eseményként értesítések egy- és a lehető legkedvezőbb módon.

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
Hello szereplő hello szereplő felületen által közzétett hello események deklarálható.

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
Hello ügyféloldalon hello eseménykezelő megvalósítása.

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

Hello ügyfélen hozzon létre egy proxy toohello szereplő hello esemény közzétevő és tooits események előfizetés.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

Feladatátvétel hello esetben hello szereplő előfordulhat, hogy feladatátvételt tooa másik folyamat vagy csomópont. hello szereplő proxy hello aktív előfizetések kezeli, és automatikusan újra előfizet rájuk. Hello újbóli előfizetési időköz hello segítségével szabályozhatja `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API. toounsubscribe, használjon hello `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.

A hello szereplő egyszerűen tegye közzé hello események, akkor fordulhat elő. Ha vannak előfizetők toohello esemény, hello szereplője futásidejű fogja őket hello értesítés küldése.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a>Következő lépések
* [Aktor rögzítve](service-fabric-reliable-actors-reentrancy.md)
* [Aktor diagnosztika és teljesítményfigyelés](service-fabric-reliable-actors-diagnostics.md)
* [Aktor API referenciadokumentációt](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# mintakód](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [C# .NET Core mintakód](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [Java mintakód](http://github.com/Azure-Samples/service-fabric-java-getting-started)
