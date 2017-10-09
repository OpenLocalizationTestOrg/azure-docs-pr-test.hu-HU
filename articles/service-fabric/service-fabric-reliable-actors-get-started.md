---
title: "aaaCreate az első szereplő-alapú Azure mikroszolgáltatási C# |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello létrehozása, a Hibakeresés és a Service Fabric Reliable Actors használatával egyszerű szereplő-alapú szolgáltatás telepítése."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d4aebe72-1551-4062-b1eb-54d83297f139
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: ab4f75bef0adb6e70f0ead587475b3fb51e6e6a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-reliable-actors"></a>Bevezetés a Reliable Actors
> [!div class="op_single_selector"]
> * [C# Windowson](service-fabric-reliable-actors-get-started.md)
> * [Java Linuxon](service-fabric-reliable-actors-get-started-java.md)
> 
> 

Ez a cikk ismerteti az Azure Service Fabric Reliable Actors hello alapjait, és végigvezeti a létrehozása, hibakeresés, és egy egyszerű megbízható szereplő alkalmazást a Visual Studio telepítése.

## <a name="installation-and-setup"></a>Telepítés és beállítás
Mielőtt elkezdené, győződjön meg arról, hogy rendelkezik-e a Service Fabric környezet hello állítsa be a számítógépre.
Ha tooset van szüksége, hogy részletes útmutatást lásd: a [hogyan hello fejlesztési környezet létrehozása tooset](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Alapfogalmak
lépések a Reliable Actors, akkor csak tooget toounderstand néhány főbb fogalmait és kifejezéseit van szükség:

* **Aktor szolgáltatás**. Reliable Actors Reliable Services hello Service Fabric infrastruktúra telepített vannak csomagolva. Szereplő példányok a megnevezett példánya. a rendszer aktiválja.
* **Aktor regisztrációs**. Mint a Reliable Services egy megbízható szereplő szolgáltatásnak kell toobe hello Service Fabric-futtatókörnyezet regisztrálva. Emellett hello szereplő típus kell toobe hello szereplő futásidejű regisztrálva.
* **Aktor felület**. hello szereplő objektumfelület használt toodefine egy szereplő egy szigorú típusmegadású nyilvános felületén. Hello szereplő felület hello megbízható szereplő modell terminológia, határozza meg a hello szereplő hello üzenetek típusát megértéséhez, és feldolgozni. más szereplője hello szereplő felületet használja, és ügyfélalkalmazások túl "" (aszinkron küldés) üzenetek toohello szereplő. Reliable Actors több adapter is létrehozható.
* **ActorProxy osztály**. hello ActorProxy osztály ügyfél alkalmazások tooinvoke által használt módszerek hello szereplő felületen keresztül elérhetővé tett hello. hello ActorProxy osztály két fontos funkciók biztosítja:
  
  * Nevek feloldása: képes toolocate hello szereplő hello fürt (hello csomópont keresése hello fürt, ahol tárolása).
  * Kezelési hiba: metódus meghívásához újra és újra oldja fel a hello szereplő helyet után, például hello szereplő toobe igénylő hiba áthelyezését tooanother hello fürt csomópontja.

a következő szabályok, amelyek a projektjükhöz tooactor felületek hello érdemes megemlíteni a következők:

* Aktor a felület metódusai nem túlterhelt.
* Aktor felület metódusok nem lehetnek kimenő, ref vagy választható paraméterek:.
* Általános illesztőfelületeinek nem támogatottak.

## <a name="create-a-new-project-in-visual-studio"></a>A Visual Studio új projekt létrehozása
Indítsa el a Visual Studio 2015-öt vagy a Visual Studio 2017 rendszergazdaként, és új Service Fabric-alkalmazás projekt létrehozása:

![Service Fabric tools for Visual Studio – új projekt][1]

A hello tovább párbeszédpanelen kiválaszthatja hello típusát projekt, amelyet az toocreate.

![A Service Fabric projektsablonjai][5]

Hello HelloWorld projekt most hello Service Fabric Reliable Actors szolgáltatás használata.

A létrehozást követően hello megoldás, a következő struktúra hello kell megjelennie:

![A Service Fabric-projekt struktúra][2]

## <a name="reliable-actors-basic-building-blocks"></a>Megbízható szereplője alapvető építőelemeket, amelyekből
Egy tipikus Reliable Actors megoldás három projektet áll:

* **hello projektet (MyActorApplication)**. Ez az összes hello szolgáltatást együtt a központi telepítési csomagok hello projektet. Hello tartalmaz *ApplicationManifest.xml* és a PowerShell-parancsfájlok hello alkalmazás kezeléséhez.
* **hello felület project (MyActor.Interfaces)**. Ez a hello projekt, amely hello szereplő hello felület definícióját tartalmazza. Hello MyActor.Interfaces projektben hello szereplője hello megoldás által használt csatolók hello adhat meg. Az aktor-adapterek bármilyen tetszőleges nevet, a projekt adható meg azonban hello felület hello szereplő szerződés hello szereplő végrehajtása és a hívó hello szereplő, ezért általában teszi logika toodefine hello ügyfelek által közösen használt határozza meg azt a szerelvényt, amely a hello szereplő megvalósítási külön, és több más projektek megoszthatók.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **hello szereplő projekt (MyActor)**. Ez a hello használt projekt toodefine hello Service Fabric-szolgáltatás, amelyet folyamatos toohost hello szereplő. Hello szereplő hello végrehajtásának tartalmaz. Az aktor megvalósítása az abból származó hello alaptípus osztály `Actor` és megvalósít hello hello MyActor.Interfaces projektben definiált adaptert. Az aktor osztály is meg kell valósítania elfogadó konstruktorral egy `ActorService` példány és egy `ActorId` , és továbbadja őket toohello alap `Actor` osztály. Ez lehetővé teszi a platform függőségek konstruktor függőségi beszúrást.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

hello szereplő szolgáltatás regisztrálni kell a Service Fabric-futtatókörnyezet hello szolgáltatás típussal. A sorrendjének hello szereplő szolgáltatás toorun a szereplő példányok, az aktor típusát is regisztrálni kell a hello szereplő szolgáltatás. Hello `ActorRuntime` regisztrációs módszer a szereplőket végrehajtja ezt a műveletet.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Ha a Visual Studio új projekt indítja, és csak szereplő definícióval rendelkezik, hello regisztrációs alapértelmezés szerint a Visual Studio generáló hello kódot tartalmazza. Más szereplője hello szolgáltatásban határozza meg, ha szüksége tooadd hello szereplő regisztrációs használatával:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [!TIP]
> hello Service Fabric szereplője futásidejű megfelelően kibocsát egy [események és teljesítményszámlálók kapcsolódó tooactor módszerek](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). A diagnosztika és teljesítményfigyelés hasznosak.
> 
> 

## <a name="debugging"></a>Hibakeresés
hello Service Fabric tools for Visual Studio támogatja a helyi gépen hibakeresést. A hibakeresési munkamenetben elindíthatja elérte-e hello F5 billentyűt. A Visual Studio létrehozza (ha szükséges) csomagokat. Is hello helyi Service Fabric-fürt hello alkalmazást telepít, és csatolja a hello hibakereső.

Hello központi telepítése során lásd: hello a folyamat állapotát a hello **kimeneti** ablak.

![A Service Fabric hibakeresési kimeneti ablak][3]

## <a name="next-steps"></a>Következő lépések
További információ [hello Service Fabric-platformról használatát a Reliable Actors](service-fabric-reliable-actors-platform.md).

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
