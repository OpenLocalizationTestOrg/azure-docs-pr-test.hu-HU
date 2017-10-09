---
title: "aktor aaaReliable szereplője kiegészítő írja be a szerializálási |} Microsoft Docs"
description: "Ismerteti a Service Fabric Reliable Actors állapotok használt toodefine szerializálható osztályok és kapcsolatok alapvető követelményei"
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 6e50e4dc-969a-4a1c-b36c-b292d964c7e3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: d8584e7d90fe1c68af38983e71e5d0a7554689bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a><span data-ttu-id="f6243-103">Tudnivalók a Service Fabric Reliable Actors írja be a szerializálási</span><span class="sxs-lookup"><span data-stu-id="f6243-103">Notes on Service Fabric Reliable Actors type serialization</span></span>
<span data-ttu-id="f6243-104">hello argumentumok összes módszerek, egy szereplő felület minden metódusa által visszaadott eredmény típusú hello feladatokat, és egy szereplő állapotkezelője tárolt objektumok kell [adategyezmény-szerializálható](https://msdn.microsoft.com/library/ms731923.aspx).</span><span class="sxs-lookup"><span data-stu-id="f6243-104">hello arguments of all methods, result types of hello tasks returned by each method in an actor interface, and objects stored in an actor's state manager must be [data contract serializable](https://msdn.microsoft.com/library/ms731923.aspx).</span></span> <span data-ttu-id="f6243-105">Ez vonatkozik toohello argumentumok definiált hello módszerek [szereplő eseményfelület](service-fabric-reliable-actors-events.md).</span><span class="sxs-lookup"><span data-stu-id="f6243-105">This also applies toohello arguments of hello methods defined in [actor event interfaces](service-fabric-reliable-actors-events.md).</span></span> <span data-ttu-id="f6243-106">(Szereplő esemény a felület metódusai mindig visszaadniuk.)</span><span class="sxs-lookup"><span data-stu-id="f6243-106">(Actor event interface methods always return void.)</span></span>

## <a name="custom-data-types"></a><span data-ttu-id="f6243-107">Egyéni adattípusok</span><span class="sxs-lookup"><span data-stu-id="f6243-107">Custom data types</span></span>
<span data-ttu-id="f6243-108">Ebben a példában szereplő felülete a következő hello határozza meg, amely visszaadja az egy egyéni adattípus nevű metódus `VoicemailBox`:</span><span class="sxs-lookup"><span data-stu-id="f6243-108">In this example, hello following actor interface defines a method that returns a custom data type called `VoicemailBox`:</span></span>

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

```Java
public interface VoiceMailBoxActor extends Actor
{
    CompletableFuture<VoicemailBox> getMailBoxAsync();
}
```

<span data-ttu-id="f6243-109">hello felület egy szereplő hello állapotát kezelő toostore használó megvalósítja a `VoicemailBox` objektum:</span><span class="sxs-lookup"><span data-stu-id="f6243-109">hello interface is implemented by an actor that uses hello state manager toostore a `VoicemailBox` object:</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class VoiceMailBoxActorImpl extends FabricActor implements VoicemailBoxActor
{
    public VoiceMailBoxActorImpl(ActorService actorService, ActorId actorId)
    {
         super(actorService, actorId);
    }

    public CompletableFuture<VoicemailBox> getMailBoxAsync()
    {
         return this.stateManager().getStateAsync("Mailbox");
    }
}

```

<span data-ttu-id="f6243-110">Ebben a példában hello `VoicemailBox` szerializált objektum során:</span><span class="sxs-lookup"><span data-stu-id="f6243-110">In this example, hello `VoicemailBox` object is serialized when:</span></span>

* <span data-ttu-id="f6243-111">hello objektum szereplő példány és egy hívó között továbbított.</span><span class="sxs-lookup"><span data-stu-id="f6243-111">hello object is transmitted between an actor instance and a caller.</span></span>
* <span data-ttu-id="f6243-112">hello objektum hello állapotkezelője amennyiben megőrzött toodisk és tooother csomópontok replikált kerül.</span><span class="sxs-lookup"><span data-stu-id="f6243-112">hello object is saved in hello state manager where it is persisted toodisk and replicated tooother nodes.</span></span>

<span data-ttu-id="f6243-113">hello megbízható szereplő keretrendszer DataContract szerializálása használja.</span><span class="sxs-lookup"><span data-stu-id="f6243-113">hello Reliable Actor framework uses DataContract serialization.</span></span> <span data-ttu-id="f6243-114">Ezért az egyéni objektumok hello és tagjaik kell feliratozni a hello **DataContract** és **DataMember** attribútumok, illetve.</span><span class="sxs-lookup"><span data-stu-id="f6243-114">Therefore, hello custom data objects and their members must be annotated with hello **DataContract** and **DataMember** attributes, respectively.</span></span>

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```
```Java
public class Voicemail implements Serializable
{
    private static final long serialVersionUID = 42L;

    private UUID id;                    //getUUID() and setUUID()

    private String message;             //getMessage() and setMessage()

    private GregorianCalendar receivedAt; //getReceivedAt() and setReceivedAt()
}
```


```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```
```Java
public class VoicemailBox implements Serializable
{
    static final long serialVersionUID = 42L;
    
    public VoicemailBox()
    {
        this.messageList = new ArrayList<Voicemail>();
    }

    private List<Voicemail> messageList;   //getMessageList() and setMessageList()

    private String greeting;               //getGreeting() and setGreeting()
}
```


## <a name="next-steps"></a><span data-ttu-id="f6243-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6243-115">Next steps</span></span>
* [<span data-ttu-id="f6243-116">Aktor életciklusának és szemétgyűjtési gyűjtése</span><span class="sxs-lookup"><span data-stu-id="f6243-116">Actor lifecycle and garbage collection</span></span>](service-fabric-reliable-actors-lifecycle.md)
* [<span data-ttu-id="f6243-117">Aktor időzítők és az emlékeztetők</span><span class="sxs-lookup"><span data-stu-id="f6243-117">Actor timers and reminders</span></span>](service-fabric-reliable-actors-timers-reminders.md)
* [<span data-ttu-id="f6243-118">Szereplő események</span><span class="sxs-lookup"><span data-stu-id="f6243-118">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="f6243-119">Aktor rögzítve</span><span class="sxs-lookup"><span data-stu-id="f6243-119">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="f6243-120">Aktor polimorfizmus és objektumorientált tervezési minták</span><span class="sxs-lookup"><span data-stu-id="f6243-120">Actor polymorphism and object-oriented design patterns</span></span>](service-fabric-reliable-actors-polymorphism.md)
* [<span data-ttu-id="f6243-121">Aktor diagnosztika és teljesítményfigyelés</span><span class="sxs-lookup"><span data-stu-id="f6243-121">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
