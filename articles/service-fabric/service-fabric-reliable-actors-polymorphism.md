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
# <a name="polymorphism-in-hello-reliable-actors-framework"></a><span data-ttu-id="afd1a-103">Hello Reliable Actors keretrendszer a polimorfizmus</span><span class="sxs-lookup"><span data-stu-id="afd1a-103">Polymorphism in hello Reliable Actors framework</span></span>
<span data-ttu-id="afd1a-104">hello Reliable Actors keretrendszer lehetővé teszi számos használatával toobuild szereplője hello módszer megegyezik a Tervező objektumorientált használja.</span><span class="sxs-lookup"><span data-stu-id="afd1a-104">hello Reliable Actors framework allows you toobuild actors using many of hello same techniques that you would use in object-oriented design.</span></span> <span data-ttu-id="afd1a-105">Ezek a módszerek egyike polimorf, amely lehetővé teszi, hogy a típusok és a több tooinherit felületeihez általános a szülők.</span><span class="sxs-lookup"><span data-stu-id="afd1a-105">One of those techniques is polymorphism, which allows types and interfaces tooinherit from more generalized parents.</span></span> <span data-ttu-id="afd1a-106">Öröklődés hello Reliable Actors keretében általában modelljébe hello .NET néhány további korlátozásokkal.</span><span class="sxs-lookup"><span data-stu-id="afd1a-106">Inheritance in hello Reliable Actors framework generally follows hello .NET model with a few additional constraints.</span></span> <span data-ttu-id="afd1a-107">Java/Linux, esetén következik hello Java modell.</span><span class="sxs-lookup"><span data-stu-id="afd1a-107">In case of Java/Linux, it follows hello Java model.</span></span>

## <a name="interfaces"></a><span data-ttu-id="afd1a-108">Felületek</span><span class="sxs-lookup"><span data-stu-id="afd1a-108">Interfaces</span></span>
<span data-ttu-id="afd1a-109">hello Reliable Actors keretrendszer toodefine használatához legalább egy csatoló toobe a szereplő típus.</span><span class="sxs-lookup"><span data-stu-id="afd1a-109">hello Reliable Actors framework requires you toodefine at least one interface toobe implemented by your actor type.</span></span> <span data-ttu-id="afd1a-110">Ez az interfész használt toogenerate ügyfelek toocommunicate, a gyakrabban használt proxy osztály.</span><span class="sxs-lookup"><span data-stu-id="afd1a-110">This interface is used toogenerate a proxy class that can be used by clients toocommunicate with your actors.</span></span> <span data-ttu-id="afd1a-111">Felületek örökölhet egyéb felületek, mindaddig, amíg minden egy szereplő típus által megvalósított illesztőfelület és az összes a szülők végső soron származik IActor(C#) vagy Actor(Java).</span><span class="sxs-lookup"><span data-stu-id="afd1a-111">Interfaces can inherit from other interfaces as long as every interface that is implemented by an actor type and all of its parents ultimately derive from IActor(C#) or Actor(Java) .</span></span> <span data-ttu-id="afd1a-112">IActor(C#) és Actor(Java) rendre hello platform által definiált alap-illesztőfelületben a szereplőket a .NET- és Java hello keretrendszerek.</span><span class="sxs-lookup"><span data-stu-id="afd1a-112">IActor(C#) and Actor(Java) are hello platform-defined base interfaces for actors in hello frameworks .NET and Java respectively.</span></span> <span data-ttu-id="afd1a-113">Ebből kifolyólag hello klasszikus polimorfizmus példa alakzatok használatával lehet, hogy kinéznie:</span><span class="sxs-lookup"><span data-stu-id="afd1a-113">Thus, hello classic polymorphism example using shapes might look something like this:</span></span>

![A shape szereplőket felület hierarchia][shapes-interface-hierarchy]

## <a name="types"></a><span data-ttu-id="afd1a-115">Típusok</span><span class="sxs-lookup"><span data-stu-id="afd1a-115">Types</span></span>
<span data-ttu-id="afd1a-116">A hierarchiában szereplő típusú, amely hello szereplő alaposztály hello platform által biztosított alapján is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="afd1a-116">You can also create a hierarchy of actor types, which are derived from hello base Actor class that is provided by hello platform.</span></span> <span data-ttu-id="afd1a-117">Hello esetében az alakzatkategóriát, lehetséges, hogy base `Shape`(C#) vagy `ShapeImpl`(Java) típusa:</span><span class="sxs-lookup"><span data-stu-id="afd1a-117">In hello case of shapes, you might have a base `Shape`(C#) or `ShapeImpl`(Java) type:</span></span>

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

<span data-ttu-id="afd1a-118">A altípushoz `Shape`(C#) vagy `ShapeImpl`(Java) szeretné felülbírálni az alapszintű hello származó metódusokat.</span><span class="sxs-lookup"><span data-stu-id="afd1a-118">Subtypes of `Shape`(C#) or `ShapeImpl`(Java) can override methods from hello base.</span></span>

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

<span data-ttu-id="afd1a-119">Megjegyzés: hello `ActorService` hello szereplő attribútum.</span><span class="sxs-lookup"><span data-stu-id="afd1a-119">Note hello `ActorService` attribute on hello actor type.</span></span> <span data-ttu-id="afd1a-120">Ez az attribútum közli, hogy azt automatikusan hozzon létre egy szolgáltatás, az ilyen típusú szereplője üzemeltetési hello megbízható szereplő keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="afd1a-120">This attribute tells hello Reliable Actor framework that it should automatically create a service for hosting actors of this type.</span></span> <span data-ttu-id="afd1a-121">Bizonyos esetekben előfordulhat, hogy kívánja toocreate kizárólag szánt funkció megosztása altípusainál alaptípusa, és konkrét szereplője soha nem lesznek használt tooinstantiate.</span><span class="sxs-lookup"><span data-stu-id="afd1a-121">In some cases, you may wish toocreate a base type that is solely intended for sharing functionality with subtypes and will never be used tooinstantiate concrete actors.</span></span> <span data-ttu-id="afd1a-122">Ezekben az esetekben kell használnia hello `abstract` , amely soha nem hoz létre egy adott típusú alapján szereplő kulcsszó tooindicate.</span><span class="sxs-lookup"><span data-stu-id="afd1a-122">In those cases, you should use hello `abstract` keyword tooindicate that you will never create an actor based on that type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="afd1a-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="afd1a-123">Next steps</span></span>
* <span data-ttu-id="afd1a-124">Lásd: [hogyan hello Reliable Actors keretrendszer kihasználja a Service Fabric-platformról hello](service-fabric-reliable-actors-platform.md) tooprovide megbízhatóságát, a méretezhetőség és a konzisztens állapotú legyen.</span><span class="sxs-lookup"><span data-stu-id="afd1a-124">See [how hello Reliable Actors framework leverages hello Service Fabric platform](service-fabric-reliable-actors-platform.md) tooprovide reliability, scalability, and consistent state.</span></span>
* <span data-ttu-id="afd1a-125">További tudnivalók: hello [szereplő életciklus](service-fabric-reliable-actors-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="afd1a-125">Learn about hello [actor lifecycle](service-fabric-reliable-actors-lifecycle.md).</span></span>

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
