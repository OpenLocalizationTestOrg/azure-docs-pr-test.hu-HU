---
title: "az Azure Event Hubs aaaWhat, és miért érdemes használni |} Microsoft Docs"
description: "Áttekintés és a bevezetés tooAzure Event Hubs - felhőméretű telemetriai adatfeldolgozást webhelyek, alkalmazások és eszközök"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm; babanisa
ms.openlocfilehash: f6199a2e5bee8506f529b6f561234d79f9c8d465
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-event-hubs"></a><span data-ttu-id="b4047-103">Mi az Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="b4047-103">What is Event Hubs?</span></span>

<span data-ttu-id="b4047-104">Az Azure Event Hubs egy kiválóan méretezhető adatstreamelési platform és eseményfeldolgozási szolgáltatás, amely másodpercenként több millió esemény fogadására és feldolgozására képes.</span><span class="sxs-lookup"><span data-stu-id="b4047-104">Azure Event Hubs is a highly scalable data streaming platform and event ingestion service capable of receiving and processing millions of events per second.</span></span> <span data-ttu-id="b4047-105">Az Event Hubs képes az elosztott szoftverek és eszközök által generált események, adatok vagy telemetria feldolgozására és tárolására.</span><span class="sxs-lookup"><span data-stu-id="b4047-105">Event Hubs can process and store events, data, or telemetry produced by distributed software and devices.</span></span> <span data-ttu-id="b4047-106">Adatok küldése az eseményközpont tooan átalakíthatók és bármilyen valós idejű elemzési szolgáltató vagy kötegelési/tárolóadapter segítségével tárolják.</span><span class="sxs-lookup"><span data-stu-id="b4047-106">Data sent tooan event hub can be transformed and stored using any real-time analytics provider or batching/storage adapters.</span></span> <span data-ttu-id="b4047-107">A hello képességét tooprovide [közzétételi-feliratkozási képességek](https://msdn.microsoft.com/library/aa560414.aspx) alacsony késéssel és nagy méretű az Event Hubs funkcionál hello "on elsajátítják" a Big Data.</span><span class="sxs-lookup"><span data-stu-id="b4047-107">With hello ability tooprovide [publish-subscribe capabilities](https://msdn.microsoft.com/library/aa560414.aspx) with low latency and at massive scale, Event Hubs serves as hello "on ramp" for Big Data.</span></span>

## <a name="why-use-event-hubs"></a><span data-ttu-id="b4047-108">Miért érdemes az Event Hubs platformot használni?</span><span class="sxs-lookup"><span data-stu-id="b4047-108">Why use Event Hubs?</span></span>

<span data-ttu-id="b4047-109">Az Event Hubs esemény- és telemetriakezelési képességei különösen az alábbiakhoz hasznosak:</span><span class="sxs-lookup"><span data-stu-id="b4047-109">Event Hubs event and telemetry handling capabilities make it especially useful for:</span></span>

* <span data-ttu-id="b4047-110">alkalmazások kialakítása,</span><span class="sxs-lookup"><span data-stu-id="b4047-110">Application instrumentation</span></span>
* <span data-ttu-id="b4047-111">felhasználói élmények vagy munkafolyamatok feldolgozása,</span><span class="sxs-lookup"><span data-stu-id="b4047-111">User experience or workflow processing</span></span>
* <span data-ttu-id="b4047-112">az eszközök internetes hálózatát (IoT) érintő forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="b4047-112">Internet of Things (IoT) scenarios</span></span>

<span data-ttu-id="b4047-113">Az Event Hubs segítségével lehetségessé válik például a viselkedéskövetés a mobilalkalmazásokban, a forgalmi információk gyűjtése a webfarmokról, a játékbeli események rögzítése a konzolos játékokban, vagy telemetriaadatok gyűjtése az ipari gépekről, csatlakoztatott járművekről vagy más eszközökről.</span><span class="sxs-lookup"><span data-stu-id="b4047-113">For example, Event Hubs enables behavior tracking in mobile apps, traffic information from web farms, in-game event capture in console games, or telemetry collected from industrial machines, connected vehicles, or other devices.</span></span>

## <a name="azure-event-hubs-overview"></a><span data-ttu-id="b4047-114">Azure Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="b4047-114">Azure Event Hubs overview</span></span>

<span data-ttu-id="b4047-115">hello mely Event Hubs megoldás architektúrák játszik szerepet hello "bejárati ajtón" megoldásarchitektúrákban, az úgynevezett egy *eseménybetöltőnek*.</span><span class="sxs-lookup"><span data-stu-id="b4047-115">hello common role that Event Hubs plays in solution architectures is hello "front door" for an event pipeline, often called an *event ingestor*.</span></span> <span data-ttu-id="b4047-116">Egy eseménybetöltőnek összetevő vagy szolgáltatás, amely az esemény-közzétevők és az esemény fogyasztók toodecouple hello éles egy esemény-adatfolyam a hello felhasználásától között.</span><span class="sxs-lookup"><span data-stu-id="b4047-116">An event ingestor is a component or service that sits between event publishers and event consumers toodecouple hello production of an event stream from hello consumption of those events.</span></span> <span data-ttu-id="b4047-117">hello alábbi ábra mutatja be ebbe az architektúrába:</span><span class="sxs-lookup"><span data-stu-id="b4047-117">hello following figure depicts this architecture:</span></span>

![Event Hubs](./media/event-hubs-what-is-event-hubs/event_hubs_full_pipeline.png)

<span data-ttu-id="b4047-119">Az Event Hubs üzenetstream-kezelési képességet is biztosít, de olyan tulajdonságokkal rendelkezik, amelyek eltérnek a hagyományos vállalati üzenetkezelés jellemzőitől.</span><span class="sxs-lookup"><span data-stu-id="b4047-119">Event Hubs provides message stream handling capability but has characteristics that are different from traditional enterprise messaging.</span></span> <span data-ttu-id="b4047-120">Az Event Hubs képességei kimondottan a nagy mennyiségre és eseményfeldolgozási forgatókönyvekre vannak optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="b4047-120">Event Hubs capabilities are built around high throughput and event processing scenarios.</span></span> <span data-ttu-id="b4047-121">Az Event Hubs így nem azonos a [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) üzenetküldési, és nem valósítja meg bizonyos hello képességeket elérhető [Service Bus üzenetkezelés](/azure/service-bus-messaging/) entitások, például a témakörök.</span><span class="sxs-lookup"><span data-stu-id="b4047-121">As such, Event Hubs is different from [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) messaging, and does not implement some of hello capabilities that are available for [Service Bus messaging](/azure/service-bus-messaging/) entities, such as topics.</span></span>

## <a name="event-hubs-features"></a><span data-ttu-id="b4047-122">Event Hubs-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b4047-122">Event Hubs features</span></span>

<span data-ttu-id="b4047-123">Az Event Hubs hello a következő fő elemekből áll:</span><span class="sxs-lookup"><span data-stu-id="b4047-123">Event Hubs contains hello following key elements:</span></span>

- <span data-ttu-id="b4047-124">[**Esemény gyártók-közzétevők**](event-hubs-features.md#event-publishers): által küldött adatok tooan eseményközpont entitás.</span><span class="sxs-lookup"><span data-stu-id="b4047-124">[**Event producers/publishers**](event-hubs-features.md#event-publishers): An entity that sends data tooan event hub.</span></span> <span data-ttu-id="b4047-125">Az esemény közzététele AMQP 1.0-n vagy HTTPS-en keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="b4047-125">An event is published via AMQP 1.0 or HTTPS.</span></span>
- <span data-ttu-id="b4047-126">[**Rögzítése**](event-hubs-features.md#capture): lehetővé teszi a streamelési adatok toocapture Event Hubs, és tárolja az Azure Blob storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="b4047-126">[**Capture**](event-hubs-features.md#capture): Enables you toocapture Event Hubs streaming data and store it in an Azure Blob storage account.</span></span>
- <span data-ttu-id="b4047-127">[**Partíciók**](event-hubs-features.md#partitions): lehetővé teszi, hogy minden fogyasztói tooonly egy adott részhalmazát, vagyis partícióját olvassa a hello eseményfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="b4047-127">[**Partitions**](event-hubs-features.md#partitions): Enables each consumer tooonly read a specific subset, or partition, of hello event stream.</span></span>
- <span data-ttu-id="b4047-128">[**SAS-tokenje**](event-hubs-features.md#sas-tokens): azonosítja, és ezzel hitelesíti a hello esemény-közzétevő.</span><span class="sxs-lookup"><span data-stu-id="b4047-128">[**SAS tokens**](event-hubs-features.md#sas-tokens): Identifies and authenticates hello event publisher.</span></span>
- <span data-ttu-id="b4047-129">[**Eseményfelhasználó**](event-hubs-features.md#event-consumers): minden entitás, amely eseményadatokat olvas egy eseményközpontból.</span><span class="sxs-lookup"><span data-stu-id="b4047-129">[**Event consumers**](event-hubs-features.md#event-consumers): An entity that reads event data from an event hub.</span></span> <span data-ttu-id="b4047-130">Az eseményfelhasználó az AMQP 1.0-n keresztül csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="b4047-130">Event consumers connect via AMQP 1.0.</span></span> 
- <span data-ttu-id="b4047-131">[**Felhasználói csoportok**](event-hubs-features.md#consumer-groups): biztosít minden több alkalmazás hello eseménystream külön láthassák fel, ezek a fogyasztók tooact egymástól függetlenül engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b4047-131">[**Consumer groups**](event-hubs-features.md#consumer-groups): Provides each multiple consuming application with a separate view of hello event stream, enabling those consumers tooact independently.</span></span>
- <span data-ttu-id="b4047-132">[**Átviteli egységek**](event-hubs-features.md#capacity): előre megvásárolt kapacitásegységek.</span><span class="sxs-lookup"><span data-stu-id="b4047-132">[**Throughput units**](event-hubs-features.md#capacity): Pre-purchased units of capacity.</span></span> <span data-ttu-id="b4047-133">Egy partíció legfeljebb egy átviteli egységgel rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="b4047-133">A single partition has a maximum scale of 1 throughput unit.</span></span>

<span data-ttu-id="b4047-134">Ezeket és más, az Event Hubs szolgáltatást technikai részleteiért lásd: hello [Event Hubs szolgáltatások – áttekintés](event-hubs-features.md).</span><span class="sxs-lookup"><span data-stu-id="b4047-134">For technical details about these and other Event Hubs features, see hello [Event Hubs features overview](event-hubs-features.md).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b4047-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4047-135">Next steps</span></span>

<span data-ttu-id="b4047-136">Az Event Hubs részletes díjszabási információi: [Event Hubs-díjszabás](https://azure.microsoft.com/pricing/details/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="b4047-136">For detailed Event Hubs pricing information, see [Event Hubs Pricing](https://azure.microsoft.com/pricing/details/event-hubs/).</span></span>

<span data-ttu-id="b4047-137">Az Event Hubs kapcsolatos további információkért látogasson el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="b4047-137">For more information about Event Hubs, visit hello following links:</span></span>

* <span data-ttu-id="b4047-138">Bevezetés az [Event Hubs használatába oktatóanyag](event-hubs-dotnet-standard-getstarted-send.md)</span><span class="sxs-lookup"><span data-stu-id="b4047-138">Get started with an [Event Hubs tutorial](event-hubs-dotnet-standard-getstarted-send.md)</span></span>
* [<span data-ttu-id="b4047-139">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="b4047-139">Event Hubs FAQ</span></span>](event-hubs-faq.md)
* [<span data-ttu-id="b4047-140">Az Event Hubsot használó mintaalkalmazások</span><span class="sxs-lookup"><span data-stu-id="b4047-140">Sample applications that use Event Hubs</span></span>](https://github.com/Azure/azure-event-hubs/tree/master/samples)
 
 

