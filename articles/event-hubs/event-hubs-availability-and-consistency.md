---
title: "aaaAvailability és az Azure Event Hubs következetes |} Microsoft Docs"
description: "Hogyan tooprovide hello maximális mennyisége rendelkezésre állását és az Azure Event Hubs használatával egységesítése partíciót."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 8f3637a1-bbd7-481e-be49-b3adf9510ba1
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: a8ededaae1589830da21cb8910ca694d2d628bd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-consistency-in-event-hubs"></a><span data-ttu-id="538ba-103">Rendelkezésre állás és a konzisztencia az Event Hubs</span><span class="sxs-lookup"><span data-stu-id="538ba-103">Availability and consistency in Event Hubs</span></span>

## <a name="overview"></a><span data-ttu-id="538ba-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="538ba-104">Overview</span></span>
<span data-ttu-id="538ba-105">Az Azure Event Hubs használ egy [modell particionálás](event-hubs-features.md#partitions) tooimprove rendelkezésre állását és párhuzamos folyamatkezelést biztosítja egyetlen eseményközpont belül.</span><span class="sxs-lookup"><span data-stu-id="538ba-105">Azure Event Hubs uses a [partitioning model](event-hubs-features.md#partitions) tooimprove availability and parallelization within a single event hub.</span></span> <span data-ttu-id="538ba-106">Például ha egy eseményközpontba négy partíciót, és ezek a partíciók egyik áthelyeznek egy kiszolgáló tooanother terheléselosztási műveletet, akkor is továbbra is küldhet és fogadhat három más partíciókból.</span><span class="sxs-lookup"><span data-stu-id="538ba-106">For example, if an event hub has four partitions, and one of those partitions is moved from one server tooanother in a load balancing operation, you can still send and receive from three other partitions.</span></span> <span data-ttu-id="538ba-107">Emellett több partíciót rendelkező lehetővé teszi toohave a teljes átviteli sebesség növelése több egyidejű olvasók feldolgozni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="538ba-107">Additionally, having more partitions enables you toohave more concurrent readers processing your data, improving your aggregate throughput.</span></span> <span data-ttu-id="538ba-108">Particionálás és az elosztott rendszer rendelés hello következményei megértése, egyik fontos szempontja, hogy a megoldás kialakításának.</span><span class="sxs-lookup"><span data-stu-id="538ba-108">Understanding hello implications of partitioning and ordering in a distributed system is a critical aspect of solution design.</span></span>

<span data-ttu-id="538ba-109">toohelp hello kompromisszum közötti rendezés és a rendelkezésre állási ismertetik, lásd: hello [CAP tétel](https://en.wikipedia.org/wiki/CAP_theorem), más néven sörgyár tartozó tétel.</span><span class="sxs-lookup"><span data-stu-id="538ba-109">toohelp explain hello trade-off between ordering and availability, see hello [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem), also known as Brewer's theorem.</span></span> <span data-ttu-id="538ba-110">A tétel hello választott közötti konzisztencia, a rendelkezésre állás és a partíció képesség ismerteti.</span><span class="sxs-lookup"><span data-stu-id="538ba-110">This theorem discusses hello choice between consistency, availability, and partition tolerance.</span></span>

<span data-ttu-id="538ba-111">Sörgyár tartozó tétel meghatározza, hogy konzisztencia és rendelkezésre állás az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="538ba-111">Brewer's theorem defines consistency and availability as follows:</span></span>
* <span data-ttu-id="538ba-112">Partícióazonosító tolerancia: hello azon képessége, egy adatfeldolgozási rendszer toocontinue adatok feldolgozása még akkor is, ha a partíció hiba történik.</span><span class="sxs-lookup"><span data-stu-id="538ba-112">Partition tolerance: hello ability of a data processing system toocontinue processing data even if a partition failure occurs.</span></span>
* <span data-ttu-id="538ba-113">Rendelkezésre állás: nem hibás csomópont választ ad vissza egy ésszerű (a nem hibák vagy időtúllépés) elfogadható időn belül.</span><span class="sxs-lookup"><span data-stu-id="538ba-113">Availability: a non-failing node returns a reasonable response within a reasonable amount of time (with no errors or timeouts).</span></span>
* <span data-ttu-id="538ba-114">Konzisztencia: olvasási garantáltan tooreturn hello legutóbbi írása az adott ügyfél.</span><span class="sxs-lookup"><span data-stu-id="538ba-114">Consistency: a read is guaranteed tooreturn hello most recent write for a given client.</span></span>

## <a name="partition-tolerance"></a><span data-ttu-id="538ba-115">Partíció tolerancia</span><span class="sxs-lookup"><span data-stu-id="538ba-115">Partition tolerance</span></span>
<span data-ttu-id="538ba-116">Az Event Hubs egy particionált adatmodell épül.</span><span class="sxs-lookup"><span data-stu-id="538ba-116">Event Hubs is built on top of a partitioned data model.</span></span> <span data-ttu-id="538ba-117">Konfigurálható hello partíciók száma az eseményközpont a telepítés során, de nem ez az érték később módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="538ba-117">You can configure hello number of partitions in your event hub during setup, but you cannot change this value later.</span></span> <span data-ttu-id="538ba-118">Az Event Hubs partíciók kell használnia, mert van toomake rendelkezésre állás és a konzisztencia kapcsolatos döntést az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="538ba-118">Since you must use partitions with Event Hubs, you have toomake a decision about availability and consistency for your application.</span></span>

## <a name="availability"></a><span data-ttu-id="538ba-119">Rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="538ba-119">Availability</span></span>
<span data-ttu-id="538ba-120">hello legegyszerűbb módja tooget használatába Event Hubs az toouse hello alapértelmezés.</span><span class="sxs-lookup"><span data-stu-id="538ba-120">hello simplest way tooget started with Event Hubs is toouse hello default behavior.</span></span> <span data-ttu-id="538ba-121">Ha létrehoz egy új `EventHubClient` objektumra, és használja a hello `Send` metódust, az eseményeket a rendszer automatikusan terjeszt a partíciók az eseményközpont között.</span><span class="sxs-lookup"><span data-stu-id="538ba-121">If you create a new `EventHubClient` object and use hello `Send` method, your events are automatically distributed between partitions in your event hub.</span></span> <span data-ttu-id="538ba-122">Ez a viselkedés lehetővé teszi a hello legnagyobb mennyisége idő.</span><span class="sxs-lookup"><span data-stu-id="538ba-122">This behavior allows for hello greatest amount of up time.</span></span>

<span data-ttu-id="538ba-123">Használati esetek hello maximális idő szükséges ez a modell használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="538ba-123">For use cases that require hello maximum up time, this model is preferred.</span></span>

## <a name="consistency"></a><span data-ttu-id="538ba-124">Konzisztencia</span><span class="sxs-lookup"><span data-stu-id="538ba-124">Consistency</span></span>
<span data-ttu-id="538ba-125">Bizonyos esetekben hello események sorrendje fontos lehet.</span><span class="sxs-lookup"><span data-stu-id="538ba-125">In some scenarios, hello ordering of events can be important.</span></span> <span data-ttu-id="538ba-126">Például érdemes lehet a háttér-rendszer tooprocess egy frissítési parancs előtt a Törlés parancsot.</span><span class="sxs-lookup"><span data-stu-id="538ba-126">For example, you may want your back-end system tooprocess an update command before a delete command.</span></span> <span data-ttu-id="538ba-127">Ebben a példában, vagy hello partíciós kulcs egy olyan eseményre, vagy használhatja a `PartitionSender` objektum tooonly küldése események tooa bizonyos partíció.</span><span class="sxs-lookup"><span data-stu-id="538ba-127">In this instance, you can either set hello partition key on an event, or use a `PartitionSender` object tooonly send events tooa certain partition.</span></span> <span data-ttu-id="538ba-128">Ezzel biztosítja, hogy ezek az események olvasása hello partícióról, olvasott sorrendben.</span><span class="sxs-lookup"><span data-stu-id="538ba-128">Doing so ensures that when these events are read from hello partition, they are read in order.</span></span>

<span data-ttu-id="538ba-129">Ezzel a konfigurációval vegye figyelembe, hogy ha küldi hello adott partíció toowhich nem érhető el, kapni fog egy hibaüzenetet.</span><span class="sxs-lookup"><span data-stu-id="538ba-129">With this configuration, keep in mind that if hello particular partition toowhich you are sending is unavailable, you will receive an error response.</span></span> <span data-ttu-id="538ba-130">Összehasonlítási pontként Ha nem rendelkezik egy kapcsolat tooa egypartíciós, hello Event Hubs szolgáltatás küldi el a esemény toohello következő elérhető partíció.</span><span class="sxs-lookup"><span data-stu-id="538ba-130">As a point of comparison, if you do not have an affinity tooa single partition, hello Event Hubs service sends your event toohello next available partition.</span></span>

<span data-ttu-id="538ba-131">Egy lehetséges megoldás tooensure idő, is Mindeközben rendelési lenne tooaggregate események alkalmazás feldolgozása az esemény részeként.</span><span class="sxs-lookup"><span data-stu-id="538ba-131">One possible solution tooensure ordering, while also maximizing up time, would be tooaggregate events as part of your event processing application.</span></span> <span data-ttu-id="538ba-132">Ennek legegyszerűbb módja tooaccomplish hello az toostamp az esemény egyéni feladatütemezési számú tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="538ba-132">hello easiest way tooaccomplish this is toostamp your event with a custom sequence number property.</span></span> <span data-ttu-id="538ba-133">a következő kód hello példáját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="538ba-133">hello following code shows an example:</span></span>

```csharp
// Get hello latest sequence number from your application
var sequenceNumber = GetNextSequenceNumber();
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set a custom sequence number property
data.Properties.Add("SequenceNumber", sequenceNumber);
// Send single message async
await eventHubClient.SendAsync(data);
```

<span data-ttu-id="538ba-134">Ebben a példában elküldi az esemény tooone hello rendelkezésre álló partíciók az eseményközpont, és beállítja a megfelelő sorszám hello az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="538ba-134">This example sends your event tooone of hello available partitions in your event hub, and sets hello corresponding sequence number from your application.</span></span> <span data-ttu-id="538ba-135">Ez a megoldás által a feldolgozás alkalmazás állapota toobe igényel, de lehetővé a feladók nagy valószínűséggel toobe rendelkezésre álló végpontok.</span><span class="sxs-lookup"><span data-stu-id="538ba-135">This solution requires state toobe kept by your processing application, but gives your senders an endpoint that is more likely toobe available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="538ba-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="538ba-136">Next steps</span></span>
<span data-ttu-id="538ba-137">További információ az Event Hubs érhetők el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="538ba-137">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="538ba-138">Event Hubs szolgáltatás – áttekintés</span><span class="sxs-lookup"><span data-stu-id="538ba-138">Event Hubs service overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="538ba-139">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="538ba-139">Create an event hub</span></span>](event-hubs-create.md)
