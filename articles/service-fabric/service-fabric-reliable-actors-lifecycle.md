---
title: "aktor-alapú Azure mikroszolgáltatások életciklus aaaOverview |} Microsoft Docs"
description: "Ismerteti a Service Fabric megbízható szereplő életciklus, szemétgyűjtés és manuális törlése szereplője és azok állapota"
services: service-fabric
documentationcenter: .net
author: amanbha
manager: timlt
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: amanbha
ms.openlocfilehash: a7926e372449048f0a579c2c58573754a4a82363
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a><span data-ttu-id="6ffd9-103">Aktor életciklusát, automatikus szemétgyűjtés és manuális törlése</span><span class="sxs-lookup"><span data-stu-id="6ffd9-103">Actor lifecycle, automatic garbage collection, and manual delete</span></span>
<span data-ttu-id="6ffd9-104">Egy szereplő aktív hello először kezdeményezték tooany metódusa.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-104">An actor is activated hello first time a call is made tooany of its methods.</span></span> <span data-ttu-id="6ffd9-105">Egy szereplő az inaktív (szemétgyűjtési hello szereplője futtatókörnyezet által összegyűjtött), ha a konfigurált időtartamon nem használható.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-105">An actor is deactivated (garbage collected by hello Actors runtime) if it is not used for a configurable period of time.</span></span> <span data-ttu-id="6ffd9-106">Egy szereplő és annak állapotát is törölhetők manuálisan bármikor.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-106">An actor and its state can also be deleted manually at any time.</span></span>

## <a name="actor-activation"></a><span data-ttu-id="6ffd9-107">Aktor aktiválás</span><span class="sxs-lookup"><span data-stu-id="6ffd9-107">Actor activation</span></span>
<span data-ttu-id="6ffd9-108">Egy szereplő aktiválásakor hello következő történik:</span><span class="sxs-lookup"><span data-stu-id="6ffd9-108">When an actor is activated, hello following occurs:</span></span>

* <span data-ttu-id="6ffd9-109">Ha a hívás érkezik egy szereplő számára, és egy már nem aktív, egy új szereplő jön létre.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-109">When a call comes for an actor and one is not already active, a new actor is created.</span></span>
* <span data-ttu-id="6ffd9-110">hello szereplő állapot be van töltve, ha az állapot karbantartása.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-110">hello actor's state is loaded if it's maintaining state.</span></span>
* <span data-ttu-id="6ffd9-111">Hello `OnActivateAsync` (C#) vagy `onActivateAsync` (Java) metódust (amely felülbírálhatók hello szereplő megvalósítása).</span><span class="sxs-lookup"><span data-stu-id="6ffd9-111">hello `OnActivateAsync` (C#) or `onActivateAsync` (Java) method (which can be overridden in hello actor implementation) is called.</span></span>
* <span data-ttu-id="6ffd9-112">hello szereplő most tekinthető aktív.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-112">hello actor is now considered active.</span></span>

## <a name="actor-deactivation"></a><span data-ttu-id="6ffd9-113">Aktor inaktiválása</span><span class="sxs-lookup"><span data-stu-id="6ffd9-113">Actor deactivation</span></span>
<span data-ttu-id="6ffd9-114">Ha egy szereplő az Inaktiválás hello következő történik:</span><span class="sxs-lookup"><span data-stu-id="6ffd9-114">When an actor is deactivated, hello following occurs:</span></span>

* <span data-ttu-id="6ffd9-115">Ha egy szereplő nem használják-e valamennyi ideje, az törlődik, hello aktív szereplője táblából.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-115">When an actor is not used for some period of time, it is removed from hello Active Actors table.</span></span>
* <span data-ttu-id="6ffd9-116">Hello `OnDeactivateAsync` (C#) vagy `onDeactivateAsync` (Java) metódust (amely felülbírálhatók hello szereplő megvalósítása).</span><span class="sxs-lookup"><span data-stu-id="6ffd9-116">hello `OnDeactivateAsync` (C#) or `onDeactivateAsync` (Java) method (which can be overridden in hello actor implementation) is called.</span></span> <span data-ttu-id="6ffd9-117">Ez törli a hello szereplő összes hello időmérőket.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-117">This clears all hello timers for hello actor.</span></span> <span data-ttu-id="6ffd9-118">Szereplő műveletek, például a metódus nem hívható módosításokat állam.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-118">Actor operations like state changes should not be called from this method.</span></span>

> [!TIP]
> <span data-ttu-id="6ffd9-119">hello háló szereplője futásidejű megfelelően kibocsát egy [tooactor aktiválás és az inaktiválást kapcsolatos események](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters).</span><span class="sxs-lookup"><span data-stu-id="6ffd9-119">hello Fabric Actors runtime emits some [events related tooactor activation and deactivation](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters).</span></span> <span data-ttu-id="6ffd9-120">A diagnosztika és teljesítményfigyelés hasznosak.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-120">They are useful in diagnostics and performance monitoring.</span></span>
>
>

### <a name="actor-garbage-collection"></a><span data-ttu-id="6ffd9-121">Aktor szemétgyűjtés</span><span class="sxs-lookup"><span data-stu-id="6ffd9-121">Actor garbage collection</span></span>
<span data-ttu-id="6ffd9-122">Amikor az Inaktiválás egy szereplő hivatkozások toohello szereplő objektum kiadott, és szemétgyűjtő általában hello közös nyelvi futtatókörnyezet (CLR) vagy a java virtuális gép (JVM) szemétgyűjtő.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-122">When an actor is deactivated, references toohello actor object are released and it can be garbage collected normally by hello common language runtime (CLR) or java virtual machine (JVM) garbage collector.</span></span> <span data-ttu-id="6ffd9-123">A szemétgyűjtés csak a szükségtelenné vált hello szereplő objektumot. létezik **nem** hello szereplő állapotkezelője tárolt állapot eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-123">Garbage collection only cleans up hello actor object; it does **not** remove state stored in hello actor's State Manager.</span></span> <span data-ttu-id="6ffd9-124">hello tovább idő hello szereplő aktiválva van, egy új szereplő objektum jön létre, és az állapot visszaállítása megtörtént.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-124">hello next time hello actor is activated, a new actor object is created and its state is restored.</span></span>

<span data-ttu-id="6ffd9-125">Mi számít "használatban" hello célját, valamint az inaktiválást szemétgyűjtés?</span><span class="sxs-lookup"><span data-stu-id="6ffd9-125">What counts as “being used” for hello purpose of deactivation and garbage collection?</span></span>

* <span data-ttu-id="6ffd9-126">Hívás fogadása</span><span class="sxs-lookup"><span data-stu-id="6ffd9-126">Receiving a call</span></span>
* <span data-ttu-id="6ffd9-127">`IRemindable.ReceiveReminderAsync`(csak akkor, ha a hello szereplő használ emlékeztetők alkalmazható) meghívott metódus</span><span class="sxs-lookup"><span data-stu-id="6ffd9-127">`IRemindable.ReceiveReminderAsync` method being invoked (applicable only if hello actor uses reminders)</span></span>

> [!NOTE]
> <span data-ttu-id="6ffd9-128">hello szereplő időzítők használ, és időzítő visszahívási meghívták, ha az nem **nem** mint "használják" száma.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-128">if hello actor uses timers and its timer callback is invoked, it does **not** count as "being used".</span></span>
>
>

<span data-ttu-id="6ffd9-129">Mielőtt megnyitjuk hello deaktiválása részleteit, a következő feltételek fontos toodefine hello:</span><span class="sxs-lookup"><span data-stu-id="6ffd9-129">Before we go into hello details of deactivation, it is important toodefine hello following terms:</span></span>

* <span data-ttu-id="6ffd9-130">*Beolvasás időköze*.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-130">*Scan interval*.</span></span> <span data-ttu-id="6ffd9-131">Ez hello időköz, mely hello szereplője futásidejű az aktív szereplője tábla keres, amely inaktiválhatók szereplője és szemétgyűjtés.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-131">This is hello interval at which hello Actors runtime scans its Active Actors table for actors that can be deactivated and garbage collected.</span></span> <span data-ttu-id="6ffd9-132">hello alapértelmezett ennek értéke 1 perc.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-132">hello default value for this is 1 minute.</span></span>
* <span data-ttu-id="6ffd9-133">*Üresjárati időtúllépés*.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-133">*Idle timeout*.</span></span> <span data-ttu-id="6ffd9-134">Ez az, hogy egy szereplő kell tooremain nem használt hello időt (inaktív), mielőtt kikapcsolható és szemétgyűjtés.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-134">This is hello amount of time that an actor needs tooremain unused (idle) before it can be deactivated and garbage collected.</span></span> <span data-ttu-id="6ffd9-135">hello alapértelmezett ennek értéke 60 perc.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-135">hello default value for this is 60 minutes.</span></span>

<span data-ttu-id="6ffd9-136">Általában nem szükséges toochange ezeket az alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-136">Typically, you do not need toochange these defaults.</span></span> <span data-ttu-id="6ffd9-137">Azonban, ha szükséges, intervallumok módosítható keresztül `ActorServiceSettings` regisztrálásakor a [szereplő szolgáltatás](service-fabric-reliable-actors-platform.md):</span><span class="sxs-lookup"><span data-stu-id="6ffd9-137">However, if necessary, these intervals can be changed through `ActorServiceSettings` when registering your [Actor Service](service-fabric-reliable-actors-platform.md):</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        ActorRuntime.RegisterActorAsync<MyActor>((context, actorType) =>
                new ActorService(context, actorType,
                    settings:
                        new ActorServiceSettings()
                        {
                            ActorGarbageCollectionSettings =
                                new ActorGarbageCollectionSettings(10, 2)
                        }))
            .GetAwaiter()
            .GetResult();
    }
}
```

```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
    }
}
```
<span data-ttu-id="6ffd9-138">Az egyes aktív szereplő hello szereplő futásidejű nyomon követi az hello, hogy mennyi ideig lett inaktív (tehát nem használható).</span><span class="sxs-lookup"><span data-stu-id="6ffd9-138">For each active actor, hello actor runtime keeps track of hello amount of time that it has been idle (i.e. not used).</span></span> <span data-ttu-id="6ffd9-139">hello szereplő futásidejű ellenőrzi, hello szereplője minden `ScanIntervalInSeconds` toosee, ha azok szemétgyűjtési gyűjt, és azt gyűjti, ha üresjárati idő le lett `IdleTimeoutInSeconds`.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-139">hello actor runtime checks each of hello actors every `ScanIntervalInSeconds` toosee if it can be garbage collected and collects it if it has been idle for `IdleTimeoutInSeconds`.</span></span>

<span data-ttu-id="6ffd9-140">Bármikor egy szereplő használata esetén az üresjárati idő a visszaállítási too0.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-140">Anytime an actor is used, its idle time is reset too0.</span></span> <span data-ttu-id="6ffd9-141">Ezt követően hello szereplő szemétgyűjtő, csak ha újra marad a tétlen lehet `IdleTimeoutInSeconds`.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-141">After this, hello actor can be garbage collected only if it again remains idle for `IdleTimeoutInSeconds`.</span></span> <span data-ttu-id="6ffd9-142">Visszahívása, hogy egy szereplő tekinthető toohave lett alkalmazva, ha egy szereplő illesztőfelület-metódust vagy egy aktor emlékeztető visszahívás végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-142">Recall that an actor is considered toohave been used if either an actor interface method or an actor reminder callback is executed.</span></span> <span data-ttu-id="6ffd9-143">Egy szereplő van **nem** toohave tekinthető lett alkalmazva, ha az időzítő visszahívás végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-143">An actor is **not** considered toohave been used if its timer callback is executed.</span></span>

<span data-ttu-id="6ffd9-144">hello alábbi ábrán egy egyetlen szereplő tooillustrate hello életciklusát ezekről a fogalmakról.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-144">hello following diagram shows hello lifecycle of a single actor tooillustrate these concepts.</span></span>

![Üresjárati idő – példa][1]

<span data-ttu-id="6ffd9-146">hello példa metódus aktorhívások, emlékeztető és időzítőket hello hatását a szereplő hello élettartama mutatja be.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-146">hello example shows hello impact of actor method calls, reminders, and timers on hello lifetime of this actor.</span></span> <span data-ttu-id="6ffd9-147">pontokra vonatkozó hello például a következő hello érdemes megemlíteni a következők:</span><span class="sxs-lookup"><span data-stu-id="6ffd9-147">hello following points about hello example are worth mentioning:</span></span>

* <span data-ttu-id="6ffd9-148">ScanInterval és IdleTimeout beállítása too5 és 10 kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-148">ScanInterval and IdleTimeout are set too5 and 10 respectively.</span></span> <span data-ttu-id="6ffd9-149">(Egység nem számít, itt, mert a cél csak tooillustrate hello koncepció.)</span><span class="sxs-lookup"><span data-stu-id="6ffd9-149">(Units do not matter here, since our purpose is only tooillustrate hello concept.)</span></span>
* <span data-ttu-id="6ffd9-150">a toobe szemétgyűjtés történik T = 0, 5, 10, 15, 20, 25, a hello vizsgálat időköz 5 által definiált szereplője hello vizsgálatát.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-150">hello scan for actors toobe garbage collected happens at T=0,5,10,15,20,25, as defined by hello scan interval of 5.</span></span>
* <span data-ttu-id="6ffd9-151">Egy rendszeres számlálót: T = 4, 8, 12, 16, 20, 24, következik be, és a visszahívás végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-151">A periodic timer fires at T=4,8,12,16,20,24, and its callback executes.</span></span> <span data-ttu-id="6ffd9-152">Üresjárati idő hello hello szereplő nem befolyásolja.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-152">It does not impact hello idle time of hello actor.</span></span>
* <span data-ttu-id="6ffd9-153">Az aktor metódus hívását T = 7 hello üresjárati idő too0 visszaállítja, és hello szemétgyűjtés hello szereplő késlelteti.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-153">An actor method call at T=7 resets hello idle time too0 and delays hello garbage collection of hello actor.</span></span>
* <span data-ttu-id="6ffd9-154">Az aktor emlékeztető visszahívási mindennap T = 14, és további késések hello hello szereplő szemétgyűjtés.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-154">An actor reminder callback executes at T=14 and further delays hello garbage collection of hello actor.</span></span>
* <span data-ttu-id="6ffd9-155">Hello szemétgyűjtési gyűjtemény vizsgálat T = 25 során hello szereplő üresjárati idő végül meghaladja a 10 hello üresjárati időkorlátját, és hello szereplő szemétgyűjtő.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-155">During hello garbage collection scan at T=25, hello actor's idle time finally exceeds hello idle timeout of 10, and hello actor is garbage collected.</span></span>

<span data-ttu-id="6ffd9-156">Egy szereplő soha nem lesznek szemétgyűjtő, végrehajtja a módszerekkel, függetlenül attól, mennyi idő telhet el az adott metódus végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-156">An actor will never be garbage collected while it is executing one of its methods, no matter how much time is spent in executing that method.</span></span> <span data-ttu-id="6ffd9-157">A korábban említett aktor a felület metódusai és felszólítás visszahívások hello végrehajtásának megakadályozza, hogy a szemétgyűjtés hello szereplő üresjárati idő too0 alaphelyzetbe állításával.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-157">As mentioned earlier, hello execution of actor interface methods and reminder callbacks prevents garbage collection by resetting hello actor's idle time too0.</span></span> <span data-ttu-id="6ffd9-158">időzítő visszahívások hello végrehajtása nem állítja alaphelyzetbe a hello üresjárati idő too0.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-158">hello execution of timer callbacks does not reset hello idle time too0.</span></span> <span data-ttu-id="6ffd9-159">Hello szemétgyűjtés hello szereplő késleltetve azonban amíg hello időzítő visszahívás végrehajtása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-159">However, hello garbage collection of hello actor is deferred until hello timer callback has completed execution.</span></span>

## <a name="deleting-actors-and-their-state"></a><span data-ttu-id="6ffd9-160">Szereplője és az állapot törlése</span><span class="sxs-lookup"><span data-stu-id="6ffd9-160">Deleting actors and their state</span></span>
<span data-ttu-id="6ffd9-161">A deaktivált szereplője szemétgyűjtés csak a szükségtelenné vált hello szereplő objektum, de nem távolítja el egy szereplő állapotkezelője tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-161">Garbage collection of deactivated actors only cleans up hello actor object, but it does not remove data that is stored in an actor's State Manager.</span></span> <span data-ttu-id="6ffd9-162">Újra aktivált egy szereplő esetén az adatok újra hello állapotkezelője keresztül elérhető tooit válnak.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-162">When an actor is re-activated, its data is again made available tooit through hello State Manager.</span></span> <span data-ttu-id="6ffd9-163">Azokban az esetekben, ahol szereplője adat tárolása állapotkezelője és inaktiválása, de soha nem újra aktiválni az adatok szükséges tooclean lehet.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-163">In cases where actors store data in State Manager and are deactivated but never re-activated, it may be necessary tooclean up their data.</span></span>

<span data-ttu-id="6ffd9-164">Hello [szereplő szolgáltatás](service-fabric-reliable-actors-platform.md) függvény biztosít távoli hívó szereplője törlése:</span><span class="sxs-lookup"><span data-stu-id="6ffd9-164">hello [Actor Service](service-fabric-reliable-actors-platform.md) provides a function for deleting actors from a remote caller:</span></span>

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

<span data-ttu-id="6ffd9-165">Attól függően, hogy e hello szereplő jelenleg aktív hatások következő hello törlése egy szereplő rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="6ffd9-165">Deleting an actor has hello following effects depending on whether or not hello actor is currently active:</span></span>

* <span data-ttu-id="6ffd9-166">**Aktív szereplő**</span><span class="sxs-lookup"><span data-stu-id="6ffd9-166">**Active Actor**</span></span>
  * <span data-ttu-id="6ffd9-167">Aktor aktív szereplője lista törlődik, és inaktív.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-167">Actor is removed from active actors list and is deactivated.</span></span>
  * <span data-ttu-id="6ffd9-168">Állapotában véglegesen törlődik.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-168">Its state is deleted permanently.</span></span>
* <span data-ttu-id="6ffd9-169">**Inaktív szereplő**</span><span class="sxs-lookup"><span data-stu-id="6ffd9-169">**Inactive Actor**</span></span>
  * <span data-ttu-id="6ffd9-170">Állapotában véglegesen törlődik.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-170">Its state is deleted permanently.</span></span>

<span data-ttu-id="6ffd9-171">Vegye figyelembe, hogy nem hívható meg egy szereplő törlése magát az aktor módszerek egyikét a, mert hello szereplő nem lehet törölni az aktor hívás környezetben, mely hello a futásidejű kapott körül hello szereplő hívás tooenforce egyszálas hozzáférés a zárolási végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="6ffd9-171">Note that an actor cannot call delete on itself from one of its actor methods because hello actor cannot be deleted while executing within an actor call context, in which hello runtime has obtained a lock around hello actor call tooenforce single-threaded access.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ffd9-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6ffd9-172">Next steps</span></span>
* [<span data-ttu-id="6ffd9-173">Aktor időzítők és az emlékeztetők</span><span class="sxs-lookup"><span data-stu-id="6ffd9-173">Actor timers and reminders</span></span>](service-fabric-reliable-actors-timers-reminders.md)
* [<span data-ttu-id="6ffd9-174">Szereplő események</span><span class="sxs-lookup"><span data-stu-id="6ffd9-174">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="6ffd9-175">Aktor rögzítve</span><span class="sxs-lookup"><span data-stu-id="6ffd9-175">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="6ffd9-176">Aktor diagnosztika és teljesítményfigyelés</span><span class="sxs-lookup"><span data-stu-id="6ffd9-176">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="6ffd9-177">Aktor API referenciadokumentációt</span><span class="sxs-lookup"><span data-stu-id="6ffd9-177">Actor API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="6ffd9-178">C# mintakód</span><span class="sxs-lookup"><span data-stu-id="6ffd9-178">C# Sample code</span></span>](https://github.com/Azure/servicefabric-samples)
* [<span data-ttu-id="6ffd9-179">Java mintakód</span><span class="sxs-lookup"><span data-stu-id="6ffd9-179">Java Sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
