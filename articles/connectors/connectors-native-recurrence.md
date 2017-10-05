---
title: "Adja hozzá az ismétlődési eseményindító a logic apps |} Microsoft Docs"
description: "Az ismétlődési eseményindító, és azt egy Azure logikai alkalmazás használata áttekintése."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 51dd4f22-7dc5-41af-a0a9-e7148378cd50
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: fe558958c316c8dba42163e277ae01451f712e5a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-recurrence-trigger"></a><span data-ttu-id="d53c0-103">Ismerkedjen meg az ismétlődési eseményindító</span><span class="sxs-lookup"><span data-stu-id="d53c0-103">Get started with the recurrence trigger</span></span>
<span data-ttu-id="d53c0-104">Az ismétlődési eseményindító segítségével hatékony munkafolyamatokat hozhat létre a felhőben.</span><span class="sxs-lookup"><span data-stu-id="d53c0-104">By using the recurrence trigger, you can create powerful workflows in the cloud.</span></span>

<span data-ttu-id="d53c0-105">Megteheti például a következőt:</span><span class="sxs-lookup"><span data-stu-id="d53c0-105">For example, you can:</span></span>

* <span data-ttu-id="d53c0-106">Egy SQL tárolt eljárás futtatására minden nap munkafolyamat ütemezéséhez.</span><span class="sxs-lookup"><span data-stu-id="d53c0-106">Schedule a workflow to run a SQL stored procedure every day.</span></span>
* <span data-ttu-id="d53c0-107">A múlt héten kapcsolatos egy bizonyos hashtaggel történő e-mail-összes Twitter-üzeneteket összegzését.</span><span class="sxs-lookup"><span data-stu-id="d53c0-107">Email a summary of all tweets within the last week about a certain hashtag.</span></span>

<span data-ttu-id="d53c0-108">Ismerkedés az ismétlődési eseményindító logikai alkalmazás használatával, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="d53c0-108">To get started using the recurrence trigger in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-a-recurrence-trigger"></a><span data-ttu-id="d53c0-109">Ismétlődés eseményindítók</span><span class="sxs-lookup"><span data-stu-id="d53c0-109">Use a recurrence trigger</span></span>
<span data-ttu-id="d53c0-110">Egy eseményindító egy eseményt, amely segítségével indítsa el a munkafolyamatot, amely a logikai alkalmazás van definiálva.</span><span class="sxs-lookup"><span data-stu-id="d53c0-110">A trigger is an event that can be used to start the workflow that is defined in a logic app.</span></span> <span data-ttu-id="d53c0-111">[További tudnivalók az eseményindítók](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d53c0-111">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="d53c0-112">Íme egy parancssorozat-példa bemutatja, hogyan állíthatja be a logikai alkalmazás ismétlődési eseményindító:</span><span class="sxs-lookup"><span data-stu-id="d53c0-112">Here’s an example sequence of how to set up a recurrence trigger in a logic app:</span></span>

1. <span data-ttu-id="d53c0-113">Adja hozzá a **ismétlődési** logikai alkalmazás első lépéseként eseményindító.</span><span class="sxs-lookup"><span data-stu-id="d53c0-113">Add the **Recurrence** trigger as the first step in a logic app.</span></span>
2. <span data-ttu-id="d53c0-114">Töltse ki a paraméterek az ismétlődési.</span><span class="sxs-lookup"><span data-stu-id="d53c0-114">Fill in the parameters for the recurrence interval.</span></span>

<span data-ttu-id="d53c0-115">A logikai alkalmazást most futtató minden időköz után elindul.</span><span class="sxs-lookup"><span data-stu-id="d53c0-115">The logic app now starts a run after each interval of time.</span></span>

![HTTP eseményindító](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a><span data-ttu-id="d53c0-117">Eseményindító részletei</span><span class="sxs-lookup"><span data-stu-id="d53c0-117">Trigger details</span></span>
<span data-ttu-id="d53c0-118">Az ismétlődési eseményindító tulajdonságai a következők konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="d53c0-118">The recurrence trigger has the following properties that you can configure.</span></span>

<span data-ttu-id="d53c0-119">A logikai alkalmazást a megadott időtartam után következik be.</span><span class="sxs-lookup"><span data-stu-id="d53c0-119">It fires a logic app after a specified time interval.</span></span>
<span data-ttu-id="d53c0-120">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="d53c0-120">A * means that it is a required field.</span></span>

| <span data-ttu-id="d53c0-121">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="d53c0-121">Display name</span></span> | <span data-ttu-id="d53c0-122">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="d53c0-122">Property name</span></span> | <span data-ttu-id="d53c0-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="d53c0-123">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d53c0-124">Gyakoriság *</span><span class="sxs-lookup"><span data-stu-id="d53c0-124">Frequency*</span></span> |<span data-ttu-id="d53c0-125">gyakoriság</span><span class="sxs-lookup"><span data-stu-id="d53c0-125">frequency</span></span> |<span data-ttu-id="d53c0-126">Időegység: `Second`, `Minute`, `Hour`, `Day`, vagy `Year`.</span><span class="sxs-lookup"><span data-stu-id="d53c0-126">The unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.</span></span> |
| <span data-ttu-id="d53c0-127">Időköz *</span><span class="sxs-lookup"><span data-stu-id="d53c0-127">Interval*</span></span> |<span data-ttu-id="d53c0-128">időköz</span><span class="sxs-lookup"><span data-stu-id="d53c0-128">interval</span></span> |<span data-ttu-id="d53c0-129">Az adott gyakorisága az ismétlődés időköze.</span><span class="sxs-lookup"><span data-stu-id="d53c0-129">The interval of the given frequency for the recurrence.</span></span> |
| <span data-ttu-id="d53c0-130">Időzóna</span><span class="sxs-lookup"><span data-stu-id="d53c0-130">Time Zone</span></span> |<span data-ttu-id="d53c0-131">Időzóna</span><span class="sxs-lookup"><span data-stu-id="d53c0-131">timeZone</span></span> |<span data-ttu-id="d53c0-132">Ha a kezdési idő áll rendelkezésre a UTC eltolás nélkül, ezt az időzónát használható.</span><span class="sxs-lookup"><span data-stu-id="d53c0-132">If a start time is provided without a UTC offset, this time zone will be used.</span></span> |
| <span data-ttu-id="d53c0-133">Kezdési idő</span><span class="sxs-lookup"><span data-stu-id="d53c0-133">Start time</span></span> |<span data-ttu-id="d53c0-134">startTime</span><span class="sxs-lookup"><span data-stu-id="d53c0-134">startTime</span></span> |<span data-ttu-id="d53c0-135">A kezdési idő [ISO 8601 formátum](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).</span><span class="sxs-lookup"><span data-stu-id="d53c0-135">The start time in [ISO 8601 format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).</span></span> |

<br>

## <a name="next-steps"></a><span data-ttu-id="d53c0-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d53c0-136">Next steps</span></span>
<span data-ttu-id="d53c0-137">Most, próbálja ki a platformot és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="d53c0-137">Now, try out the platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="d53c0-138">Az egyéb rendelkezésre álló összekötők logic Apps alkalmazások felmérésével felfedezheti a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="d53c0-138">You can explore the other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

