---
title: "egy Azure idő adatsorozat Insights környezet aaaCreate |} Microsoft Docs"
description: "Ebből az oktatóanyagból megtudhatja, hogyan toocreate adatsorozat környezet, csatlakoztassa tooan eseményforrás, és a eseményadatok tooanalyze kész percben."
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
ms.openlocfilehash: 7120fc9a6e4d4a4972f8cb37e4d9945cfb746fd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-new-time-series-insights-environment-in-hello-azure-portal"></a><span data-ttu-id="9ee5e-103">Hozzon létre egy új idő adatsorozat Insights környezetben hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9ee5e-103">Create a new Time Series Insights environment in hello Azure portal</span></span>

<span data-ttu-id="9ee5e-104">A Time Series Insights-környezet egy Azure-erőforrás bejövő adatforgalmi és tárolási kapacitással.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-104">Time Series Insights environment is an Azure resource with ingress and storage capacity.</span></span> <span data-ttu-id="9ee5e-105">Az ügyfelek hello szükséges kapacitással rendelkező környezetek hello Azure-portálon keresztül kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-105">Customers provision environments via hello Azure portal with hello required capacity.</span></span>

## <a name="steps-toocreate-hello-environment"></a><span data-ttu-id="9ee5e-106">Lépéseket toocreate hello környezet</span><span class="sxs-lookup"><span data-stu-id="9ee5e-106">Steps toocreate hello environment</span></span>

<span data-ttu-id="9ee5e-107">Kövesse ezeket a lépéseket toocreate a környezetben:</span><span class="sxs-lookup"><span data-stu-id="9ee5e-107">Follow these steps toocreate your environment:</span></span>

1.  <span data-ttu-id="9ee5e-108">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9ee5e-108">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2.  <span data-ttu-id="9ee5e-109">Kattintson a bal sarok hello felső hello plusz jel ("+").</span><span class="sxs-lookup"><span data-stu-id="9ee5e-109">Click hello plus sign (“+”) in hello top left corner.</span></span>
3.  <span data-ttu-id="9ee5e-110">Keresse meg a "Idő adatsorozat Insights" hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-110">Search for “Time Series Insights” in hello search box.</span></span>

  ![Hello idő adatsorozat Insights környezet létrehozása](media/get-started/getstarted-create-environment1.png)

4.  <span data-ttu-id="9ee5e-112">Válassza a „Time Series Insights” elemet, és kattintson a „Létrehozás” lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-112">Select “Time Series Insights”, click “Create”.</span></span>

  ![Hello idő adatsorozat Insights erőforráscsoport létrehozása](media/get-started/getstarted-create-environment2.png)

5.  <span data-ttu-id="9ee5e-114">Adja meg a környezet nevét.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-114">Specify environment name.</span></span> <span data-ttu-id="9ee5e-115">Ez a név jelöli hello környezet [idő adatsorozat explorer](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9ee5e-115">This name will represent hello environment in [time series explorer](https://insights.timeseries.azure.com).</span></span>
6.  <span data-ttu-id="9ee5e-116">Válasszon egy előfizetést.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-116">Select a subscription.</span></span> <span data-ttu-id="9ee5e-117">Azt az előfizetést válassza, amely tartalmazza az eseményforrást.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-117">Choose one that contains your event source.</span></span> <span data-ttu-id="9ee5e-118">Idő adatsorozat Insights is automatikus észlelésű Azure IoT-központot, és a meglévő Eseményközpont erőforrások hello ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-118">Time Series Insights can auto-detect Azure IoT Hub and Event Hub resources existing in hello same subscription.</span></span>
7.  <span data-ttu-id="9ee5e-119">Válasszon ki vagy hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-119">Select or create a resource group.</span></span> <span data-ttu-id="9ee5e-120">Az erőforráscsoport olyan Azure-erőforrások gyűjteménye, amelyeket együtt használnak.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-120">A resource group is a collection of Azure resources used together.</span></span>
8.  <span data-ttu-id="9ee5e-121">Válasszon egy üzemeltetési helyet.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-121">Select a hosting location.</span></span> <span data-ttu-id="9ee5e-122">tooavoid áthelyezése az adatok között adatközpontok, válassza ki az esemény forrását tartalmazó helyet.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-122">tooavoid moving data across data centers, choose location that contains your event source.</span></span>
9.  <span data-ttu-id="9ee5e-123">Válasszon tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-123">Select a pricing tier.</span></span>
10. <span data-ttu-id="9ee5e-124">Válassza ki a kapacitást.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-124">Select capacity.</span></span> <span data-ttu-id="9ee5e-125">A környezet kapacitása a létrehozást követően módosítható.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-125">You can change capacity of an environment after creation.</span></span>
11. <span data-ttu-id="9ee5e-126">Hozza létre a környezetet.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-126">Create your environment.</span></span> <span data-ttu-id="9ee5e-127">Egyszerűen hozzáférhetnek a környezeti toohello irányítópult is rögzítheti, amikor bejelentkezik.</span><span class="sxs-lookup"><span data-stu-id="9ee5e-127">You can also pin your environment toohello dashboard for easy access whenever you sign in.</span></span>

  ![Hello idő adatsorozat Insights PIN-kód toodashboard létrehozása](media/get-started/getstarted-create-environment3.png)

## <a name="next-steps"></a><span data-ttu-id="9ee5e-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ee5e-129">Next steps</span></span>

* <span data-ttu-id="9ee5e-130">[Adja meg az adat-hozzáférési házirendjeit](time-series-insights-data-access.md) tooaccess a környezet [idő adatsorozat Insights portálon találja meg](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="9ee5e-130">[Define data access policies](time-series-insights-data-access.md) tooaccess your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>
* [<span data-ttu-id="9ee5e-131">Eseményforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ee5e-131">Create an event source</span></span>](time-series-insights-add-event-source.md)
* <span data-ttu-id="9ee5e-132">[Események küldése](time-series-insights-send-events.md) toohello eseményforrás</span><span class="sxs-lookup"><span data-stu-id="9ee5e-132">[Send events](time-series-insights-send-events.md) toohello event source</span></span>
