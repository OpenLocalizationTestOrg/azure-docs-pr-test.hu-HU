---
title: hello Reliable Actors keretrendszer aaaPolymorphism |} Microsoft Docs
description: "Hozzon létre hierarchiákat .NET felületek és típusok a Reliable Actors keretrendszer tooreuse funkció hello és API-definíciók."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: vturecek
ms.assetid: ef0eeff6-32b7-410d-ac69-87cba8b8fd46
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: ed9ec442595bd9a5e48c9af1f6aac003439b81f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="polymorphism-in-hello-reliable-actors-framework"></a>Hello Reliable Actors keretrendszer a polimorfizmus
hello Reliable Actors keretrendszer lehetővé teszi számos használatával toobuild szereplője hello módszer megegyezik a Tervező objektumorientált használja. Ezek a módszerek egyike polimorf, amely lehetővé teszi, hogy a típusok és a több tooinherit felületeihez általános a szülők. Öröklődés hello Reliable Actors keretében általában modelljébe hello .NET néhány további korlátozásokkal. Java/Linux, esetén következik hello Java modell.

## <a name="interfaces"></a>Felületek
hello Reliable Actors keretrendszer toodefine használatához legalább egy csatoló toobe a szereplő típus. Ez az interfész használt toogenerate ügyfelek toocommunicate, a gyakrabban használt proxy osztály. Felületek örökölhet egyéb felületek, mindaddig, amíg minden egy szereplő típus által megvalósított illesztőfelület és az összes a szülők végső soron származik IActor(C#) vagy Actor(Java). IActor(C#) és Actor(Java) rendre hello platform által definiált alap-illesztőfelületben a szereplőket a .NET- és Java hello keretrendszerek. Ebből kifolyólag hello klasszikus polimorfizmus példa alakzatok használatával lehet, hogy kinéznie:

![A shape szereplőket felület hierarchia][shapes-interface-hierarchy]

## <a name="types"></a>Típusok
A hierarchiában szereplő típusú, amely hello szereplő alaposztály hello platform által biztosított alapján is létrehozhat. Hello esetében az alakzatkategóriát, lehetséges, hogy base `Shape`(C#) vagy `ShapeImpl`(Java) típusa:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```
```Java
public abstract class ShapeImpl extends FabricActor implements Shape
{
    public abstract CompletableFuture<int> getVerticeCount();

    public abstract CompletableFuture<double> getAreaAsync();
}
```

A altípushoz `Shape`(C#) vagy `ShapeImpl`(Java) szeretné felülbírálni az alapszintű hello származó metódusokat.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```
```Java
@ActorServiceAttribute(name = "Circle")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class Circle extends ShapeImpl implements Circle
{
    @Override
    public CompletableFuture<Integer> getVerticeCount()
    {
        return CompletableFuture.completedFuture(0);
    }

    @Override
    public CompletableFuture<Double> getAreaAsync()
    {
        return (this.stateManager().getStateAsync<CircleState>("circle").thenApply(state->{
          return Math.PI * state.radius * state.radius;
        }));
    }
}
```

Megjegyzés: hello `ActorService` hello szereplő attribútum. Ez az attribútum közli, hogy azt automatikusan hozzon létre egy szolgáltatás, az ilyen típusú szereplője üzemeltetési hello megbízható szereplő keretrendszer. Bizonyos esetekben előfordulhat, hogy kívánja toocreate kizárólag szánt funkció megosztása altípusainál alaptípusa, és konkrét szereplője soha nem lesznek használt tooinstantiate. Ezekben az esetekben kell használnia hello `abstract` , amely soha nem hoz létre egy adott típusú alapján szereplő kulcsszó tooindicate.

## <a name="next-steps"></a>Következő lépések
* Lásd: [hogyan hello Reliable Actors keretrendszer kihasználja a Service Fabric-platformról hello](service-fabric-reliable-actors-platform.md) tooprovide megbízhatóságát, a méretezhetőség és a konzisztens állapotú legyen.
* További tudnivalók: hello [szereplő életciklus](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
