---
title: "Eseményforrás hozzáadása Azure Time Series Insight-környezethez | Microsoft Docs"
description: "Ebben az oktatóanyagban csatlakoztatni fog egy eseményforrást az Azure Time Series Insights-környezetéhez"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.openlocfilehash: ffa2eaf3680e68ac14aabf49b6308caeb173fd43
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-event-source-for-your-time-series-insights-environment-using-the-ibiza-portal"></a><span data-ttu-id="7c244-103">Eseményforrás létrehozása Azure Time Series Insights-környezethez az Ibiza Portal használatával</span><span class="sxs-lookup"><span data-stu-id="7c244-103">Create an event source for your Time Series Insights environment using the Ibiza portal</span></span>

<span data-ttu-id="7c244-104">Egy Time Series Insights-eseményforrás egy eseményközvetítőből, például az Azure Event Hubsból származik.</span><span class="sxs-lookup"><span data-stu-id="7c244-104">Time Series Insights Event Source is derived from an event broker, like Azure Event Hubs.</span></span> <span data-ttu-id="7c244-105">A Time Series Insights közvetlenül csatlakozik az eseményforrásokhoz, és anélkül dolgozza fel az adatfolyamot, hogy a felhasználóknak akár egy sor kódot is kéne írniuk.</span><span class="sxs-lookup"><span data-stu-id="7c244-105">Time Series Insights connects directly to Event Sources, ingesting the data stream without requiring users to write a single line of code.</span></span> <span data-ttu-id="7c244-106">A Time Series Insights jelenleg az Azure Event Hubs és Azure IoT Hubs forrásokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="7c244-106">Currently, Time Series Insights supports Azure Event Hubs and Azure IoT Hubs.</span></span> <span data-ttu-id="7c244-107">A jövőben további eseményforrásokkal is bővülni fog.</span><span class="sxs-lookup"><span data-stu-id="7c244-107">In the future, more Event Sources will be added.</span></span>

## <a name="steps-to-add-an-event-source-to-your-environment"></a><span data-ttu-id="7c244-108">Eseményforrás környezethez való hozzáadásának lépései</span><span class="sxs-lookup"><span data-stu-id="7c244-108">Steps to add an event source to your environment</span></span>

1.  <span data-ttu-id="7c244-109">Jelentkezzen be az [Ibiza Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7c244-109">Sign in to the [Ibiza portal](https://portal.azure.com).</span></span>
2.  <span data-ttu-id="7c244-110">Az Ibiza Portal bal oldali menüjében kattintson a „Minden erőforrás” lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="7c244-110">Click “All resources” in the menu on the left side of the Ibiza portal.</span></span>
3.  <span data-ttu-id="7c244-111">Válassza ki az Azure Time Series Insights-környezetet.</span><span class="sxs-lookup"><span data-stu-id="7c244-111">Select your Time Series Insights environment.</span></span>

  ![A Time Series Insights-eseményforrás létrehozása](media/add-event-source/getstarted-create-event-source-1.png)

4.  <span data-ttu-id="7c244-113">Válassza az „Eseményforrások” lehetőséget, és kattintson a „+ Hozzáadás” gombra.</span><span class="sxs-lookup"><span data-stu-id="7c244-113">Select “Event Sources”, click “+ Add.”</span></span>

  ![A Time Series Insights-eseményforrás létrehozása – részletek](media/add-event-source/getstarted-create-event-source-2.png)

5.  <span data-ttu-id="7c244-115">Adja meg az eseményforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="7c244-115">Specify the name of the event source.</span></span> <span data-ttu-id="7c244-116">Ez a név az eseményforrástól származó összes eseményhez hozzá lesz rendelve, és a lekérdezéskor elérhető.</span><span class="sxs-lookup"><span data-stu-id="7c244-116">This name is associated with all events coming from this event source and is available at query time.</span></span>
6.  <span data-ttu-id="7c244-117">Válasszon eseményközpontot az adott előfizetés eseményközpont-erőforrásainak listájából.</span><span class="sxs-lookup"><span data-stu-id="7c244-117">Select an event hub from the list of Event Hub resources in the current subscription.</span></span> <span data-ttu-id="7c244-118">Egyébként válassza az „Eseményközpont beállításainak kézi megadása” import-beállítást, ha más előfizetéshez tartozó eseményközpontot kíván megadni.</span><span class="sxs-lookup"><span data-stu-id="7c244-118">Otherwise choose import option "Provide Event Hub settings manually” to specify an event hub in another subscription.</span></span> <span data-ttu-id="7c244-119">Az eseményeket JSON formátumban kell közzétenni.</span><span class="sxs-lookup"><span data-stu-id="7c244-119">Events must be published in JSON format.</span></span>
7.  <span data-ttu-id="7c244-120">Olyan szabályzatot válasszon, amelynek olvasási engedélye van az eseményközpontban.</span><span class="sxs-lookup"><span data-stu-id="7c244-120">Select policy that has read permission in the event hub.</span></span>
8.  <span data-ttu-id="7c244-121">Határozza meg az eseményközpont fogyasztói csoportját.</span><span class="sxs-lookup"><span data-stu-id="7c244-121">Specify event hub consumer group.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="7c244-122">Ügyeljen arra, hogy ezt a fogyasztói csoportot ne használja másik szolgáltatás (például Stream Analytics-feladat vagy másik Time Series Insights-környezet).</span><span class="sxs-lookup"><span data-stu-id="7c244-122">Make sure this consumer group is not used by any other service (such as Stream Analytics job or another Time Series Insights environment).</span></span> <span data-ttu-id="7c244-123">Ha a fogyasztói csoportot más szolgáltatások is használják, az zavarhatja az olvasási műveleteket ebben a környezetben és a többi szolgáltatásban is.</span><span class="sxs-lookup"><span data-stu-id="7c244-123">If consumer group is used by other services, read operation is negatively affected for this environment and the other services.</span></span> <span data-ttu-id="7c244-124">Ha a „$Default” elemet használja a fogyasztói csoportként, előfordulhat, hogy más olvasók újra fel fogják használni a csoportot.</span><span class="sxs-lookup"><span data-stu-id="7c244-124">If you are using “$Default” as the consumer group, it could lead to potential reuse by other readers.</span></span>

9.  <span data-ttu-id="7c244-125">Kattintson a „Létrehozás” elemre.</span><span class="sxs-lookup"><span data-stu-id="7c244-125">Click “Create.”</span></span>

<span data-ttu-id="7c244-126">Az eseményforrás létrehozása után a Time Series Insights automatikusan megkezdi az adatok streamelését a környezetbe.</span><span class="sxs-lookup"><span data-stu-id="7c244-126">After creation of the event source, Time Series Insights will automatically start streaming data into your environment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7c244-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7c244-127">Next steps</span></span>

* <span data-ttu-id="7c244-128">[Események küldése](time-series-insights-send-events.md) az eseményforrásnak</span><span class="sxs-lookup"><span data-stu-id="7c244-128">[Send events](time-series-insights-send-events.md) to the event source</span></span>
* <span data-ttu-id="7c244-129">A környezet megtekintése a [Time Series Insights portálon](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="7c244-129">View your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
