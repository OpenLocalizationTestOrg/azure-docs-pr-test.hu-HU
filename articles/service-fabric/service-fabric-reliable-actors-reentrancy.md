---
title: "az aktor-alapú Azure mikroszolgáltatások aaaReentrancy |} Microsoft Docs"
description: "A Service Fabric Reliable Actors bemutatása tooreentrancy"
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: be23464a-0eea-4eca-ae5a-2e1b650d365e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 61c69bcf0f100e075d19ba155954c05789b71761
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-actors-reentrancy"></a>Megbízható szereplője rögzítve
hello Reliable Actors futásidejű, alapértelmezés szerint lehetővé teszi, hogy logikai hívás környezetfüggő rögzítve. Ez lehetővé teszi a szereplője toobe reentrant hello lévő azonos környezetben lánc hívja. Például A Aktor küld egy üzenet tooActor B, egy üzenet tooActor c küldő Hello üzenetfeldolgozást részeként szereplő C meghívja A, szereplő üdvözlőüzenetére akkor ismételten belépő, amelynek engedélyezni. Bármely más üzeneteket, amelyek egy másik hívás környezetet részei le lesz tiltva a Aktor A feldolgozás befejezéséig.

Nincsenek definiálva hello szereplő rögzítve két lehetőség közül választhat `ActorReentrancyMode` enum:

* `LogicalCallContext`(alapértelmezés)
* `Disallowed`-rögzítve letiltása

```csharp
public enum ActorReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```
```Java
public enum ActorReentrancyMode
{
    LogicalCallContext(1),
    Disallowed(2)
}
```
Rögzítve konfigurálható egy `ActorService`tartozó beállítások regisztrálás során. hello beállítás hello szereplő szolgáltatásban létrehozott tooall szereplő példányok vonatkozik.

hello alábbi példa bemutatja az aktor szolgáltatása, amely túl hello rögzítve mód beállítása`ActorReentrancyMode.Disallowed`. Ebben az esetben, ha egy szereplő küld egy ismételten belépő üzenet tooanother szereplő típusú kivételt `FabricException` fog jelezni.

```csharp
static class Program
{
    static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<Actor1>(
                (context, actorType) => new ActorService(
                    context,
                    actorType, () => new Actor1(),
                    settings: new ActorServiceSettings()
                    {
                        ActorConcurrencySettings = new ActorConcurrencySettings()
                        {
                            ReentrancyMode = ActorReentrancyMode.Disallowed
                        }
                    }))
                .GetAwaiter().GetResult();

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
```Java
static class Program
{
    static void Main()
    {
        try
        {
            ActorConcurrencySettings actorConcurrencySettings = new ActorConcurrencySettings();
            actorConcurrencySettings.setReentrancyMode(ActorReentrancyMode.Disallowed);

            ActorServiceSettings actorServiceSettings = new ActorServiceSettings();
            actorServiceSettings.setActorConcurrencySettings(actorConcurrencySettings);

            ActorRuntime.registerActorAsync(
                Actor1.getClass(),
                (context, actorType) -> new FabricActorService(
                    context,
                    actorType, () -> new Actor1(),
                    null,
                    stateProvider,
                    actorServiceSettings, timeout);

            Thread.sleep(Long.MAX_VALUE);
        }
        catch (Exception e)
        {
            throw e;
        }
    }
}
```


## <a name="next-steps"></a>Következő lépések
* További információ a hello rögzítve [szereplő API referenciadokumentációt tartalmaz](https://msdn.microsoft.com/library/azure/dn971626.aspx)
