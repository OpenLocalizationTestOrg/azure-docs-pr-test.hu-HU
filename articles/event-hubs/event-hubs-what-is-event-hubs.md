---
title: "Mi az az Azure Event Hubs, és mire használható? | Microsoft Docs"
description: "Az Azure Event Hubs áttekintése és bemutatása – Felhőméretű telemetriaadat-feldolgozás webhelyekről, alkalmazásokból és eszközökről származó adatok esetén"
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
ms.openlocfilehash: 1a6bf0a0352e6d9e3a22586ac825558d12e1307a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-event-hubs"></a><span data-ttu-id="21e31-103">Mi az Event Hubs?</span><span class="sxs-lookup"><span data-stu-id="21e31-103">What is Event Hubs?</span></span>

<span data-ttu-id="21e31-104">Az Azure Event Hubs egy kiválóan méretezhető adatstreamelési platform és eseményfeldolgozási szolgáltatás, amely másodpercenként több millió esemény fogadására és feldolgozására képes.</span><span class="sxs-lookup"><span data-stu-id="21e31-104">Azure Event Hubs is a highly scalable data streaming platform and event ingestion service capable of receiving and processing millions of events per second.</span></span> <span data-ttu-id="21e31-105">Az Event Hubs képes az elosztott szoftverek és eszközök által generált események, adatok vagy telemetria feldolgozására és tárolására.</span><span class="sxs-lookup"><span data-stu-id="21e31-105">Event Hubs can process and store events, data, or telemetry produced by distributed software and devices.</span></span> <span data-ttu-id="21e31-106">Az eseményközpontokba elküldött adatok bármilyen valós idejű elemzési szolgáltató vagy kötegelési/tárolóadapter segítségével átalakíthatók és tárolhatók.</span><span class="sxs-lookup"><span data-stu-id="21e31-106">Data sent to an event hub can be transformed and stored using any real-time analytics provider or batching/storage adapters.</span></span> <span data-ttu-id="21e31-107">Az alacsony késésű és nagy méretű [közzétételi-feliratkozási képességeket](https://msdn.microsoft.com/library/aa560414.aspx) biztosító Event Hubs az „első lépcsőfok” a big data jellegű adatmennyiségek kezelése irányában.</span><span class="sxs-lookup"><span data-stu-id="21e31-107">With the ability to provide [publish-subscribe capabilities](https://msdn.microsoft.com/library/aa560414.aspx) with low latency and at massive scale, Event Hubs serves as the "on ramp" for Big Data.</span></span>

## <a name="why-use-event-hubs"></a><span data-ttu-id="21e31-108">Miért érdemes az Event Hubs platformot használni?</span><span class="sxs-lookup"><span data-stu-id="21e31-108">Why use Event Hubs?</span></span>

<span data-ttu-id="21e31-109">Az Event Hubs esemény- és telemetriakezelési képességei különösen az alábbiakhoz hasznosak:</span><span class="sxs-lookup"><span data-stu-id="21e31-109">Event Hubs event and telemetry handling capabilities make it especially useful for:</span></span>

* <span data-ttu-id="21e31-110">alkalmazások kialakítása,</span><span class="sxs-lookup"><span data-stu-id="21e31-110">Application instrumentation</span></span>
* <span data-ttu-id="21e31-111">felhasználói élmények vagy munkafolyamatok feldolgozása,</span><span class="sxs-lookup"><span data-stu-id="21e31-111">User experience or workflow processing</span></span>
* <span data-ttu-id="21e31-112">az eszközök internetes hálózatát (IoT) érintő forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="21e31-112">Internet of Things (IoT) scenarios</span></span>

<span data-ttu-id="21e31-113">Az Event Hubs segítségével lehetségessé válik például a viselkedéskövetés a mobilalkalmazásokban, a forgalmi információk gyűjtése a webfarmokról, a játékbeli események rögzítése a konzolos játékokban, vagy telemetriaadatok gyűjtése az ipari gépekről, csatlakoztatott járművekről vagy más eszközökről.</span><span class="sxs-lookup"><span data-stu-id="21e31-113">For example, Event Hubs enables behavior tracking in mobile apps, traffic information from web farms, in-game event capture in console games, or telemetry collected from industrial machines, connected vehicles, or other devices.</span></span>

## <a name="azure-event-hubs-overview"></a><span data-ttu-id="21e31-114">Azure Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="21e31-114">Azure Event Hubs overview</span></span>

<span data-ttu-id="21e31-115">Az Event Hubs gyakran tölti be az eseményfolyamatok „bejárati ajtajának” a szerepét a megoldásarchitektúrákban, mely szerepet gyakran nevezik *eseménybetöltőnek*.</span><span class="sxs-lookup"><span data-stu-id="21e31-115">The common role that Event Hubs plays in solution architectures is the "front door" for an event pipeline, often called an *event ingestor*.</span></span> <span data-ttu-id="21e31-116">Az eseménybetöltő egy olyan összetevő vagy szolgáltatás, amely az esemény-közzétevők és az eseményfelhasználók közé ékelődve elkülöníti az eseménystream létrehozását az események felhasználásától.</span><span class="sxs-lookup"><span data-stu-id="21e31-116">An event ingestor is a component or service that sits between event publishers and event consumers to decouple the production of an event stream from the consumption of those events.</span></span> <span data-ttu-id="21e31-117">A következő ábra ezt az architektúrát ábrázolja:</span><span class="sxs-lookup"><span data-stu-id="21e31-117">The following figure depicts this architecture:</span></span>

![Event Hubs](./media/event-hubs-what-is-event-hubs/event_hubs_full_pipeline.png)

<span data-ttu-id="21e31-119">Az Event Hubs üzenetstream-kezelési képességet is biztosít, de olyan tulajdonságokkal rendelkezik, amelyek eltérnek a hagyományos vállalati üzenetkezelés jellemzőitől.</span><span class="sxs-lookup"><span data-stu-id="21e31-119">Event Hubs provides message stream handling capability but has characteristics that are different from traditional enterprise messaging.</span></span> <span data-ttu-id="21e31-120">Az Event Hubs képességei kimondottan a nagy mennyiségre és eseményfeldolgozási forgatókönyvekre vannak optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="21e31-120">Event Hubs capabilities are built around high throughput and event processing scenarios.</span></span> <span data-ttu-id="21e31-121">Az Event Hubs különbözik az [Azure Service Bus](https://azure.microsoft.com/services/service-bus/)-üzenetkezeléstől, és nem valósít meg bizonyos képességeket, amelyek a különböző [Service Bus-üzenetküldési](/azure/service-bus-messaging/) entitások (például a témakörök) esetén elérhetőek.</span><span class="sxs-lookup"><span data-stu-id="21e31-121">As such, Event Hubs is different from [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) messaging, and does not implement some of the capabilities that are available for [Service Bus messaging](/azure/service-bus-messaging/) entities, such as topics.</span></span>

## <a name="event-hubs-features"></a><span data-ttu-id="21e31-122">Event Hubs-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="21e31-122">Event Hubs features</span></span>

<span data-ttu-id="21e31-123">Az Event Hubs az alábbi fő elemeket foglalja magába:</span><span class="sxs-lookup"><span data-stu-id="21e31-123">Event Hubs contains the following key elements:</span></span>

- <span data-ttu-id="21e31-124">[**Esemény-közzétevő**](event-hubs-features.md#event-publishers): minden entitás, amely adatokat küld egy eseményközpontnak.</span><span class="sxs-lookup"><span data-stu-id="21e31-124">[**Event producers/publishers**](event-hubs-features.md#event-publishers): An entity that sends data to an event hub.</span></span> <span data-ttu-id="21e31-125">Az esemény közzététele AMQP 1.0-n vagy HTTPS-en keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="21e31-125">An event is published via AMQP 1.0 or HTTPS.</span></span>
- <span data-ttu-id="21e31-126">[**Rögzítés**](event-hubs-features.md#capture): Event Hubs-alapú streamelt adatokat rögzíthet, amelyeket egy Azure Blob Storage-fiókban tárolhat.</span><span class="sxs-lookup"><span data-stu-id="21e31-126">[**Capture**](event-hubs-features.md#capture): Enables you to capture Event Hubs streaming data and store it in an Azure Blob storage account.</span></span>
- <span data-ttu-id="21e31-127">[**Partíciók**](event-hubs-features.md#partitions): beállítható, hogy a felhasználó csak az eseménystream egy meghatározott részhalmazát, azaz partícióját olvashassa.</span><span class="sxs-lookup"><span data-stu-id="21e31-127">[**Partitions**](event-hubs-features.md#partitions): Enables each consumer to only read a specific subset, or partition, of the event stream.</span></span>
- <span data-ttu-id="21e31-128">[**SAS-tokenek**](event-hubs-features.md#sas-tokens): meghatározzák és hitelesítik az esemény-közzétevőt.</span><span class="sxs-lookup"><span data-stu-id="21e31-128">[**SAS tokens**](event-hubs-features.md#sas-tokens): Identifies and authenticates the event publisher.</span></span>
- <span data-ttu-id="21e31-129">[**Eseményfelhasználó**](event-hubs-features.md#event-consumers): minden entitás, amely eseményadatokat olvas egy eseményközpontból.</span><span class="sxs-lookup"><span data-stu-id="21e31-129">[**Event consumers**](event-hubs-features.md#event-consumers): An entity that reads event data from an event hub.</span></span> <span data-ttu-id="21e31-130">Az eseményfelhasználó az AMQP 1.0-n keresztül csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="21e31-130">Event consumers connect via AMQP 1.0.</span></span> 
- <span data-ttu-id="21e31-131">[**Felhasználói csoportok**](event-hubs-features.md#consumer-groups): minden egyes több forrást felhasználó alkalmazás az eseménystream külön nézetével rendelkezik, így ezek a felhasználók a többitől függetlenül tevékenykedhetnek.</span><span class="sxs-lookup"><span data-stu-id="21e31-131">[**Consumer groups**](event-hubs-features.md#consumer-groups): Provides each multiple consuming application with a separate view of the event stream, enabling those consumers to act independently.</span></span>
- <span data-ttu-id="21e31-132">[**Átviteli egységek**](event-hubs-features.md#capacity): előre megvásárolt kapacitásegységek.</span><span class="sxs-lookup"><span data-stu-id="21e31-132">[**Throughput units**](event-hubs-features.md#capacity): Pre-purchased units of capacity.</span></span> <span data-ttu-id="21e31-133">Egy partíció legfeljebb egy átviteli egységgel rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="21e31-133">A single partition has a maximum scale of 1 throughput unit.</span></span>

<span data-ttu-id="21e31-134">További technikai részletek ezekről és más Event Hubs-szolgáltatásokról: [Event Hubs features overview](event-hubs-features.md) (Event Hubs-szolgáltatások – Áttekintés).</span><span class="sxs-lookup"><span data-stu-id="21e31-134">For technical details about these and other Event Hubs features, see the [Event Hubs features overview](event-hubs-features.md).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="21e31-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="21e31-135">Next steps</span></span>

<span data-ttu-id="21e31-136">Az Event Hubs részletes díjszabási információi: [Event Hubs-díjszabás](https://azure.microsoft.com/pricing/details/event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="21e31-136">For detailed Event Hubs pricing information, see [Event Hubs Pricing](https://azure.microsoft.com/pricing/details/event-hubs/).</span></span>

<span data-ttu-id="21e31-137">Ha további információkat szeretne az Event Hubsról, tekintse meg az alábbi hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="21e31-137">For more information about Event Hubs, visit the following links:</span></span>

* <span data-ttu-id="21e31-138">Bevezetés az [Event Hubs használatába oktatóanyag](event-hubs-dotnet-standard-getstarted-send.md)</span><span class="sxs-lookup"><span data-stu-id="21e31-138">Get started with an [Event Hubs tutorial](event-hubs-dotnet-standard-getstarted-send.md)</span></span>
* [<span data-ttu-id="21e31-139">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="21e31-139">Event Hubs FAQ</span></span>](event-hubs-faq.md)
* [<span data-ttu-id="21e31-140">Az Event Hubsot használó mintaalkalmazások</span><span class="sxs-lookup"><span data-stu-id="21e31-140">Sample applications that use Event Hubs</span></span>](https://github.com/Azure/azure-event-hubs/tree/master/samples)
 
 

