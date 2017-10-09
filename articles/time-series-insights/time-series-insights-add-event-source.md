---
title: "egy esemény tooyour Azure idő adatsorozat Insights forráskörnyezettel aaaAdd |} Microsoft Docs"
description: "Ebben az oktatóanyagban csatlakoztat egy esemény tooyour idő adatsorozat Insights forráskörnyezet"
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
ms.openlocfilehash: 817df5e81cb4dc3d7376914a4651aabebadbcc32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-event-source-for-your-time-series-insights-environment-using-hello-ibiza-portal"></a><span data-ttu-id="9e634-103">Hozzon létre egy eseményforrás idő adatsorozat Insights környezetnek hello Ibiza portálon</span><span class="sxs-lookup"><span data-stu-id="9e634-103">Create an event source for your Time Series Insights environment using hello Ibiza portal</span></span>

<span data-ttu-id="9e634-104">Egy Time Series Insights-eseményforrás egy eseményközvetítőből, például az Azure Event Hubsból származik.</span><span class="sxs-lookup"><span data-stu-id="9e634-104">Time Series Insights Event Source is derived from an event broker, like Azure Event Hubs.</span></span> <span data-ttu-id="9e634-105">Idő adatsorozat Insights keresztül közvetlenül tooEvent források választásával dolgozhat fel hello adatfolyam anélkül, hogy a felhasználók toowrite egyetlen sor kódot.</span><span class="sxs-lookup"><span data-stu-id="9e634-105">Time Series Insights connects directly tooEvent Sources, ingesting hello data stream without requiring users toowrite a single line of code.</span></span> <span data-ttu-id="9e634-106">A Time Series Insights jelenleg az Azure Event Hubs és Azure IoT Hubs forrásokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="9e634-106">Currently, Time Series Insights supports Azure Event Hubs and Azure IoT Hubs.</span></span> <span data-ttu-id="9e634-107">A jövőbeli hello további eseményforrások lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="9e634-107">In hello future, more Event Sources will be added.</span></span>

## <a name="steps-tooadd-an-event-source-tooyour-environment"></a><span data-ttu-id="9e634-108">Egy esemény forráskörnyezettel tooyour lépéseket tooadd</span><span class="sxs-lookup"><span data-stu-id="9e634-108">Steps tooadd an event source tooyour environment</span></span>

1.  <span data-ttu-id="9e634-109">Jelentkezzen be toohello [Ibiza portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9e634-109">Sign in toohello [Ibiza portal](https://portal.azure.com).</span></span>
2.  <span data-ttu-id="9e634-110">Kattintson az "Összes erőforrás" hello menü hello hello Ibiza portálon a bal oldalán.</span><span class="sxs-lookup"><span data-stu-id="9e634-110">Click “All resources” in hello menu on hello left side of hello Ibiza portal.</span></span>
3.  <span data-ttu-id="9e634-111">Válassza ki az Azure Time Series Insights-környezetet.</span><span class="sxs-lookup"><span data-stu-id="9e634-111">Select your Time Series Insights environment.</span></span>

  ![Hello idő adatsorozat Insights eseményforrás létrehozása](media/add-event-source/getstarted-create-event-source-1.png)

4.  <span data-ttu-id="9e634-113">Válassza az „Eseményforrások” lehetőséget, és kattintson a „+ Hozzáadás” gombra.</span><span class="sxs-lookup"><span data-stu-id="9e634-113">Select “Event Sources”, click “+ Add.”</span></span>

  ![Hozzon létre hello idő adatsorozat Insights eseményforrás - részletek](media/add-event-source/getstarted-create-event-source-2.png)

5.  <span data-ttu-id="9e634-115">Adja meg hello hello esemény forrását.</span><span class="sxs-lookup"><span data-stu-id="9e634-115">Specify hello name of hello event source.</span></span> <span data-ttu-id="9e634-116">Ez a név az eseményforrástól származó összes eseményhez hozzá lesz rendelve, és a lekérdezéskor elérhető.</span><span class="sxs-lookup"><span data-stu-id="9e634-116">This name is associated with all events coming from this event source and is available at query time.</span></span>
6.  <span data-ttu-id="9e634-117">Válassza ki az eseményközpontok a hello Eseményközpont erőforrások hello az aktuális előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="9e634-117">Select an event hub from hello list of Event Hub resources in hello current subscription.</span></span> <span data-ttu-id="9e634-118">Máskülönben válassza az importálási beállítást "adja meg az Event Hubs-beállítások manuális" toospecify egy eseményközpontba egy másik előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="9e634-118">Otherwise choose import option "Provide Event Hub settings manually” toospecify an event hub in another subscription.</span></span> <span data-ttu-id="9e634-119">Az eseményeket JSON formátumban kell közzétenni.</span><span class="sxs-lookup"><span data-stu-id="9e634-119">Events must be published in JSON format.</span></span>
7.  <span data-ttu-id="9e634-120">Válassza ki, hogy rendelkezik-e olvasási engedéllyel a hello eseményközpont házirendet.</span><span class="sxs-lookup"><span data-stu-id="9e634-120">Select policy that has read permission in hello event hub.</span></span>
8.  <span data-ttu-id="9e634-121">Határozza meg az eseményközpont fogyasztói csoportját.</span><span class="sxs-lookup"><span data-stu-id="9e634-121">Specify event hub consumer group.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="9e634-122">Ügyeljen arra, hogy ezt a fogyasztói csoportot ne használja másik szolgáltatás (például Stream Analytics-feladat vagy másik Time Series Insights-környezet).</span><span class="sxs-lookup"><span data-stu-id="9e634-122">Make sure this consumer group is not used by any other service (such as Stream Analytics job or another Time Series Insights environment).</span></span> <span data-ttu-id="9e634-123">Fogyasztói csoportot más szolgáltatások használata esetén olvassa el a művelet negatívan befolyásolja az ebben a környezetben, és más szolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="9e634-123">If consumer group is used by other services, read operation is negatively affected for this environment and hello other services.</span></span> <span data-ttu-id="9e634-124">Ha a "$Default" fogyasztói csoportot hello használ, előfordulhat toopotential újbóli más olvasók által.</span><span class="sxs-lookup"><span data-stu-id="9e634-124">If you are using “$Default” as hello consumer group, it could lead toopotential reuse by other readers.</span></span>

9.  <span data-ttu-id="9e634-125">Kattintson a „Létrehozás” elemre.</span><span class="sxs-lookup"><span data-stu-id="9e634-125">Click “Create.”</span></span>

<span data-ttu-id="9e634-126">Hello eseményforrás létrehozása, után idő adatsorozat Insights automatikusan elindítja adatfolyam környezetébe.</span><span class="sxs-lookup"><span data-stu-id="9e634-126">After creation of hello event source, Time Series Insights will automatically start streaming data into your environment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9e634-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9e634-127">Next steps</span></span>

* <span data-ttu-id="9e634-128">[Események küldése](time-series-insights-send-events.md) toohello eseményforrás</span><span class="sxs-lookup"><span data-stu-id="9e634-128">[Send events](time-series-insights-send-events.md) toohello event source</span></span>
* <span data-ttu-id="9e634-129">A környezet megtekintése a [Time Series Insights portálon](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="9e634-129">View your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
