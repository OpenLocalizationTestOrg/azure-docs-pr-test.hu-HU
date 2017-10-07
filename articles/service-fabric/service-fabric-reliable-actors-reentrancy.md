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
# <a name="reliable-actors-reentrancy"></a><span data-ttu-id="eb557-103">Megbízható szereplője rögzítve</span><span class="sxs-lookup"><span data-stu-id="eb557-103">Reliable Actors reentrancy</span></span>
<span data-ttu-id="eb557-104">hello Reliable Actors futásidejű, alapértelmezés szerint lehetővé teszi, hogy logikai hívás környezetfüggő rögzítve.</span><span class="sxs-lookup"><span data-stu-id="eb557-104">hello Reliable Actors runtime, by default, allows logical call context-based reentrancy.</span></span> <span data-ttu-id="eb557-105">Ez lehetővé teszi a szereplője toobe reentrant hello lévő azonos környezetben lánc hívja.</span><span class="sxs-lookup"><span data-stu-id="eb557-105">This allows for actors toobe reentrant if they are in hello same call context chain.</span></span> <span data-ttu-id="eb557-106">Például A Aktor küld egy üzenet tooActor B, egy üzenet tooActor c küldő Hello üzenetfeldolgozást részeként szereplő C meghívja A, szereplő üdvözlőüzenetére akkor ismételten belépő, amelynek engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="eb557-106">For example, Actor A sends a message tooActor B, who sends a message tooActor C. As part of hello message processing, if Actor C calls Actor A, hello message is reentrant, so it will be allowed.</span></span> <span data-ttu-id="eb557-107">Bármely más üzeneteket, amelyek egy másik hívás környezetet részei le lesz tiltva a Aktor A feldolgozás befejezéséig.</span><span class="sxs-lookup"><span data-stu-id="eb557-107">Any other messages that are part of a different call context will be blocked on Actor A until it finishes processing.</span></span>

<span data-ttu-id="eb557-108">Nincsenek definiálva hello szereplő rögzítve két lehetőség közül választhat `ActorReentrancyMode` enum:</span><span class="sxs-lookup"><span data-stu-id="eb557-108">There are two options available for actor reentrancy defined in hello `ActorReentrancyMode` enum:</span></span>

* <span data-ttu-id="eb557-109">`LogicalCallContext`(alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="eb557-109">`LogicalCallContext` (default behavior)</span></span>
* <span data-ttu-id="eb557-110">`Disallowed`-rögzítve letiltása</span><span class="sxs-lookup"><span data-stu-id="eb557-110">`Disallowed` - disables reentrancy</span></span>

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
<span data-ttu-id="eb557-111">Rögzítve konfigurálható egy `ActorService`tartozó beállítások regisztrálás során.</span><span class="sxs-lookup"><span data-stu-id="eb557-111">Reentrancy can be configured in an `ActorService`'s settings during registration.</span></span> <span data-ttu-id="eb557-112">hello beállítás hello szereplő szolgáltatásban létrehozott tooall szereplő példányok vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="eb557-112">hello setting applies tooall actor instances created in hello actor service.</span></span>

<span data-ttu-id="eb557-113">hello alábbi példa bemutatja az aktor szolgáltatása, amely túl hello rögzítve mód beállítása`ActorReentrancyMode.Disallowed`.</span><span class="sxs-lookup"><span data-stu-id="eb557-113">hello following example shows an actor service that sets hello reentrancy mode too`ActorReentrancyMode.Disallowed`.</span></span> <span data-ttu-id="eb557-114">Ebben az esetben, ha egy szereplő küld egy ismételten belépő üzenet tooanother szereplő típusú kivételt `FabricException` fog jelezni.</span><span class="sxs-lookup"><span data-stu-id="eb557-114">In this case, if an actor sends a reentrant message tooanother actor, an exception of type `FabricException` will be thrown.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="eb557-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eb557-115">Next steps</span></span>
* <span data-ttu-id="eb557-116">További információ a hello rögzítve [szereplő API referenciadokumentációt tartalmaz](https://msdn.microsoft.com/library/azure/dn971626.aspx)</span><span class="sxs-lookup"><span data-stu-id="eb557-116">Learn more about reentrancy in hello [Actor API reference documentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)</span></span>
