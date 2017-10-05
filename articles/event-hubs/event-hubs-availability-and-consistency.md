---
title: "Rendelkezésre állás és az Azure Event Hubs következetes |} Microsoft Docs"
description: "A legnagyobb rendelkezésre állását és konzisztencia biztosításához az Azure Event Hubs partíciók használatával hogyan."
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
ms.openlocfilehash: 681a9d1636d547492f6f827461c6b2494b918778
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="availability-and-consistency-in-event-hubs"></a><span data-ttu-id="1399f-103">Rendelkezésre állás és a konzisztencia az Event Hubs</span><span class="sxs-lookup"><span data-stu-id="1399f-103">Availability and consistency in Event Hubs</span></span>

## <a name="overview"></a><span data-ttu-id="1399f-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="1399f-104">Overview</span></span>
<span data-ttu-id="1399f-105">Az Azure Event Hubs használ egy [modell particionálás](event-hubs-features.md#partitions) rendelkezésre állását és párhuzamos folyamatkezelést biztosítja belül egyetlen eseményközpont javítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="1399f-105">Azure Event Hubs uses a [partitioning model](event-hubs-features.md#partitions) to improve availability and parallelization within a single event hub.</span></span> <span data-ttu-id="1399f-106">Például ha egy eseményközpontba négy partíciókkal rendelkezik, és ezek a partíciók egyik áthelyezése egyik kiszolgálóról a másikra terheléselosztási művelet, akkor is továbbra is küldhet és fogadhat három más partíciókból.</span><span class="sxs-lookup"><span data-stu-id="1399f-106">For example, if an event hub has four partitions, and one of those partitions is moved from one server to another in a load balancing operation, you can still send and receive from three other partitions.</span></span> <span data-ttu-id="1399f-107">Emellett több partíciót rendelkező lehetővé teszi a teljes átviteli sebesség növelése több egyidejű olvasók feldolgozni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="1399f-107">Additionally, having more partitions enables you to have more concurrent readers processing your data, improving your aggregate throughput.</span></span> <span data-ttu-id="1399f-108">Particionálás és az elosztott rendszer rendelés következményeit megértése, egyik fontos szempontja, hogy a megoldás kialakításának.</span><span class="sxs-lookup"><span data-stu-id="1399f-108">Understanding the implications of partitioning and ordering in a distributed system is a critical aspect of solution design.</span></span>

<span data-ttu-id="1399f-109">Rendezés és a rendelkezésre állási közötti kompromisszum magyarázatának elősegítésére, tekintse meg a [CAP tétel](https://en.wikipedia.org/wiki/CAP_theorem), más néven sörgyár tartozó tétel.</span><span class="sxs-lookup"><span data-stu-id="1399f-109">To help explain the trade-off between ordering and availability, see the [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem), also known as Brewer's theorem.</span></span> <span data-ttu-id="1399f-110">A tétel konzisztencia, a rendelkezésre állás és a partíció képesség közötti választás ismerteti.</span><span class="sxs-lookup"><span data-stu-id="1399f-110">This theorem discusses the choice between consistency, availability, and partition tolerance.</span></span>

<span data-ttu-id="1399f-111">Sörgyár tartozó tétel meghatározza, hogy konzisztencia és rendelkezésre állás az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="1399f-111">Brewer's theorem defines consistency and availability as follows:</span></span>
* <span data-ttu-id="1399f-112">Partícióazonosító tolerancia: a lehetősége az adatok feldolgozása rendszert adatainak feldolgozása még akkor is, ha a partíció hiba történik.</span><span class="sxs-lookup"><span data-stu-id="1399f-112">Partition tolerance: the ability of a data processing system to continue processing data even if a partition failure occurs.</span></span>
* <span data-ttu-id="1399f-113">Rendelkezésre állás: nem hibás csomópont választ ad vissza egy ésszerű (a nem hibák vagy időtúllépés) elfogadható időn belül.</span><span class="sxs-lookup"><span data-stu-id="1399f-113">Availability: a non-failing node returns a reasonable response within a reasonable amount of time (with no errors or timeouts).</span></span>
* <span data-ttu-id="1399f-114">Konzisztencia: olvasási garantáltan vissza a legutóbbi írása az adott ügyfél.</span><span class="sxs-lookup"><span data-stu-id="1399f-114">Consistency: a read is guaranteed to return the most recent write for a given client.</span></span>

## <a name="partition-tolerance"></a><span data-ttu-id="1399f-115">Partíció tolerancia</span><span class="sxs-lookup"><span data-stu-id="1399f-115">Partition tolerance</span></span>
<span data-ttu-id="1399f-116">Az Event Hubs egy particionált adatmodell épül.</span><span class="sxs-lookup"><span data-stu-id="1399f-116">Event Hubs is built on top of a partitioned data model.</span></span> <span data-ttu-id="1399f-117">Konfigurálhatja a partíciók száma az eseményközpont a telepítés során, de nem ez az érték később módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="1399f-117">You can configure the number of partitions in your event hub during setup, but you cannot change this value later.</span></span> <span data-ttu-id="1399f-118">Az Event Hubs partíciók kell használnia, mert akkor rendelkezésre állási és az alkalmazás konzisztenciájának kapcsolatos döntést.</span><span class="sxs-lookup"><span data-stu-id="1399f-118">Since you must use partitions with Event Hubs, you have to make a decision about availability and consistency for your application.</span></span>

## <a name="availability"></a><span data-ttu-id="1399f-119">Rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="1399f-119">Availability</span></span>
<span data-ttu-id="1399f-120">Az Event Hubs használatába legegyszerűbb módja, hogy az alapértelmezett viselkedés használja.</span><span class="sxs-lookup"><span data-stu-id="1399f-120">The simplest way to get started with Event Hubs is to use the default behavior.</span></span> <span data-ttu-id="1399f-121">Ha létrehoz egy új `EventHubClient` objektumra, és használja a `Send` metódust, az eseményeket a rendszer automatikusan terjeszt a partíciók az eseményközpont között.</span><span class="sxs-lookup"><span data-stu-id="1399f-121">If you create a new `EventHubClient` object and use the `Send` method, your events are automatically distributed between partitions in your event hub.</span></span> <span data-ttu-id="1399f-122">Ez a viselkedés lehetővé teszi, hogy a legnagyobb mennyisége idő.</span><span class="sxs-lookup"><span data-stu-id="1399f-122">This behavior allows for the greatest amount of up time.</span></span>

<span data-ttu-id="1399f-123">A maximális idő igénylő használat esetén ez a modell használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="1399f-123">For use cases that require the maximum up time, this model is preferred.</span></span>

## <a name="consistency"></a><span data-ttu-id="1399f-124">Konzisztencia</span><span class="sxs-lookup"><span data-stu-id="1399f-124">Consistency</span></span>
<span data-ttu-id="1399f-125">Bizonyos esetekben az események sorrendje fontos lehet.</span><span class="sxs-lookup"><span data-stu-id="1399f-125">In some scenarios, the ordering of events can be important.</span></span> <span data-ttu-id="1399f-126">Például érdemes lehet a háttér-rendszer egy frissítési parancs előtt a Törlés parancsot.</span><span class="sxs-lookup"><span data-stu-id="1399f-126">For example, you may want your back-end system to process an update command before a delete command.</span></span> <span data-ttu-id="1399f-127">Ebben a példában, vagy a partíciós kulcs egy olyan eseményre, vagy használhatja a `PartitionSender` objektum csak egy bizonyos partíció elküldje az eseményeket.</span><span class="sxs-lookup"><span data-stu-id="1399f-127">In this instance, you can either set the partition key on an event, or use a `PartitionSender` object to only send events to a certain partition.</span></span> <span data-ttu-id="1399f-128">Ezzel biztosítja, hogy ezek az események olvasása a partícióból, olvasott sorrendben.</span><span class="sxs-lookup"><span data-stu-id="1399f-128">Doing so ensures that when these events are read from the partition, they are read in order.</span></span>

<span data-ttu-id="1399f-129">Ezzel a konfigurációval vegye figyelembe, hogy az adott partíció, amelyhez küldendő nem érhető el, ha kapni fog egy hibaüzenetet.</span><span class="sxs-lookup"><span data-stu-id="1399f-129">With this configuration, keep in mind that if the particular partition to which you are sending is unavailable, you will receive an error response.</span></span> <span data-ttu-id="1399f-130">Összehasonlítási pontként Ha nem rendelkezik egyetlen partícióra kapcsolatot az Event Hubs szolgáltatás elküldi az esemény a következő elérhető partíció.</span><span class="sxs-lookup"><span data-stu-id="1399f-130">As a point of comparison, if you do not have an affinity to a single partition, the Event Hubs service sends your event to the next available partition.</span></span>

<span data-ttu-id="1399f-131">Győződjön meg arról, rendezés, idő, is Mindeközben az egyik lehetséges megoldás az alkalmazás feldolgozása az esemény részeként lenne az összegyűjtött eseményeket.</span><span class="sxs-lookup"><span data-stu-id="1399f-131">One possible solution to ensure ordering, while also maximizing up time, would be to aggregate events as part of your event processing application.</span></span> <span data-ttu-id="1399f-132">A legegyszerűbb ehhez módja az esemény stamp egyéni feladatütemezési számú tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="1399f-132">The easiest way to accomplish this is to stamp your event with a custom sequence number property.</span></span> <span data-ttu-id="1399f-133">A következő kód egy példát mutat be:</span><span class="sxs-lookup"><span data-stu-id="1399f-133">The following code shows an example:</span></span>

```csharp
// Get the latest sequence number from your application
var sequenceNumber = GetNextSequenceNumber();
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set a custom sequence number property
data.Properties.Add("SequenceNumber", sequenceNumber);
// Send single message async
await eventHubClient.SendAsync(data);
```

<span data-ttu-id="1399f-134">Ebben a példában az esemény küld egy, a rendelkezésre álló partíciók az eseményközpont a, és beállítja a megfelelő sorszámmal az alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="1399f-134">This example sends your event to one of the available partitions in your event hub, and sets the corresponding sequence number from your application.</span></span> <span data-ttu-id="1399f-135">Ez a megoldás állapotot a feldolgozás alkalmazáshoz őrzi igényel, de lehetővé a feladók olyan végponttal, amely valószínűbb, hogy elérhető legyen.</span><span class="sxs-lookup"><span data-stu-id="1399f-135">This solution requires state to be kept by your processing application, but gives your senders an endpoint that is more likely to be available.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1399f-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1399f-136">Next steps</span></span>
<span data-ttu-id="1399f-137">Az alábbi webhelyeken további információt talál az Event Hubsról:</span><span class="sxs-lookup"><span data-stu-id="1399f-137">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="1399f-138">Event Hubs szolgáltatás – áttekintés</span><span class="sxs-lookup"><span data-stu-id="1399f-138">Event Hubs service overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="1399f-139">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="1399f-139">Create an event hub</span></span>](event-hubs-create.md)
