---
title: "Azure Time Series Insights-környezet létrehozása | Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan hozhat létre Time Series-környezetet, hogyan csatlakoztathatja egy eseményforráshoz és kezdheti elemezni az eseményadatokat percek alatt."
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
ms.openlocfilehash: eb710795916a2d7beea75a6408a0982fb4dc8750
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a><span data-ttu-id="1cb93-103">Új Time Series Insights-környezet létrehozása az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="1cb93-103">Create a new Time Series Insights environment in the Azure portal</span></span>

<span data-ttu-id="1cb93-104">A Time Series Insights-környezet egy Azure-erőforrás bejövő adatforgalmi és tárolási kapacitással.</span><span class="sxs-lookup"><span data-stu-id="1cb93-104">Time Series Insights environment is an Azure resource with ingress and storage capacity.</span></span> <span data-ttu-id="1cb93-105">Az ügyfelek az Azure Portalon hozhatnak létre környezeteket a szükséges kapacitással.</span><span class="sxs-lookup"><span data-stu-id="1cb93-105">Customers provision environments via the Azure portal with the required capacity.</span></span>

## <a name="steps-to-create-the-environment"></a><span data-ttu-id="1cb93-106">A környezet létrehozásának lépései</span><span class="sxs-lookup"><span data-stu-id="1cb93-106">Steps to create the environment</span></span>

<span data-ttu-id="1cb93-107">A környezet létrehozásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1cb93-107">Follow these steps to create your environment:</span></span>

1.  <span data-ttu-id="1cb93-108">Jelentkezzen be az [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1cb93-108">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2.  <span data-ttu-id="1cb93-109">Kattintson a plusz jelre („+”) a bal felső sarokban.</span><span class="sxs-lookup"><span data-stu-id="1cb93-109">Click the plus sign (“+”) in the top left corner.</span></span>
3.  <span data-ttu-id="1cb93-110">A keresőmezőben keressen a „Time Series Insights” kifejezésre.</span><span class="sxs-lookup"><span data-stu-id="1cb93-110">Search for “Time Series Insights” in the search box.</span></span>

  ![A Time Series Insights-környezet létrehozása](media/get-started/getstarted-create-environment1.png)

4.  <span data-ttu-id="1cb93-112">Válassza a „Time Series Insights” elemet, és kattintson a „Létrehozás” lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="1cb93-112">Select “Time Series Insights”, click “Create”.</span></span>

  ![A Time Series Insights-erőforráscsoport létrehozása](media/get-started/getstarted-create-environment2.png)

5.  <span data-ttu-id="1cb93-114">Adja meg a környezet nevét.</span><span class="sxs-lookup"><span data-stu-id="1cb93-114">Specify environment name.</span></span> <span data-ttu-id="1cb93-115">Ez a név jelöli majd a környezetet a [Time Series Explorerben](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1cb93-115">This name will represent the environment in [time series explorer](https://insights.timeseries.azure.com).</span></span>
6.  <span data-ttu-id="1cb93-116">Válasszon egy előfizetést.</span><span class="sxs-lookup"><span data-stu-id="1cb93-116">Select a subscription.</span></span> <span data-ttu-id="1cb93-117">Azt az előfizetést válassza, amely tartalmazza az eseményforrást.</span><span class="sxs-lookup"><span data-stu-id="1cb93-117">Choose one that contains your event source.</span></span> <span data-ttu-id="1cb93-118">A Time Series Insights képes automatikusan észlelni az azonos előfizetésben lévő Azure IoT Hub- és Event Hub-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="1cb93-118">Time Series Insights can auto-detect Azure IoT Hub and Event Hub resources existing in the same subscription.</span></span>
7.  <span data-ttu-id="1cb93-119">Válasszon ki vagy hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="1cb93-119">Select or create a resource group.</span></span> <span data-ttu-id="1cb93-120">Az erőforráscsoport olyan Azure-erőforrások gyűjteménye, amelyeket együtt használnak.</span><span class="sxs-lookup"><span data-stu-id="1cb93-120">A resource group is a collection of Azure resources used together.</span></span>
8.  <span data-ttu-id="1cb93-121">Válasszon egy üzemeltetési helyet.</span><span class="sxs-lookup"><span data-stu-id="1cb93-121">Select a hosting location.</span></span> <span data-ttu-id="1cb93-122">Annak érdekében, hogy az adatokat ne kelljen egyik adatközpontból a másikba mozgatni, olyan helyet válasszon, amely tartalmazza az eseményforrást.</span><span class="sxs-lookup"><span data-stu-id="1cb93-122">To avoid moving data across data centers, choose location that contains your event source.</span></span>
9.  <span data-ttu-id="1cb93-123">Válasszon tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="1cb93-123">Select a pricing tier.</span></span>
10. <span data-ttu-id="1cb93-124">Válassza ki a kapacitást.</span><span class="sxs-lookup"><span data-stu-id="1cb93-124">Select capacity.</span></span> <span data-ttu-id="1cb93-125">A környezet kapacitása a létrehozást követően módosítható.</span><span class="sxs-lookup"><span data-stu-id="1cb93-125">You can change capacity of an environment after creation.</span></span>
11. <span data-ttu-id="1cb93-126">Hozza létre a környezetet.</span><span class="sxs-lookup"><span data-stu-id="1cb93-126">Create your environment.</span></span> <span data-ttu-id="1cb93-127">A környezetet rögzítheti az irányítópulton, így a bejelentkezés után könnyen elérheti.</span><span class="sxs-lookup"><span data-stu-id="1cb93-127">You can also pin your environment to the dashboard for easy access whenever you sign in.</span></span>

  ![A Time Series Insights rögzítése az irányítópulton](media/get-started/getstarted-create-environment3.png)

## <a name="next-steps"></a><span data-ttu-id="1cb93-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1cb93-129">Next steps</span></span>

* <span data-ttu-id="1cb93-130">[Adathozzáférési házirendek meghatározása](time-series-insights-data-access.md) a környezet eléréséhez a [Time Series Insights portálon](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="1cb93-130">[Define data access policies](time-series-insights-data-access.md) to access your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
* [<span data-ttu-id="1cb93-131">Eseményforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cb93-131">Create an event source</span></span>](time-series-insights-add-event-source.md)
* <span data-ttu-id="1cb93-132">[Események küldése](time-series-insights-send-events.md) az eseményforrásnak</span><span class="sxs-lookup"><span data-stu-id="1cb93-132">[Send events](time-series-insights-send-events.md) to the event source</span></span>
