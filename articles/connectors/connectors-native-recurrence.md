---
title: "a logic apps aaaAdd hello ismétlődési eseményindítót |} Microsoft Docs"
description: "Áttekintés hello ismétlődési eseményindító, és hogyan toouse azt egy Azure logikai alkalmazást."
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
ms.openlocfilehash: e7c625c382a88a1e7cdfff4ddc0caf55727232bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-recurrence-trigger"></a><span data-ttu-id="d96e5-103">Ismerkedés a hello ismétlődési eseményindító</span><span class="sxs-lookup"><span data-stu-id="d96e5-103">Get started with hello recurrence trigger</span></span>
<span data-ttu-id="d96e5-104">Hello ismétlődési eseményindító segítségével hatékony munkafolyamatokat hozhat létre hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="d96e5-104">By using hello recurrence trigger, you can create powerful workflows in hello cloud.</span></span>

<span data-ttu-id="d96e5-105">Megteheti például a következőt:</span><span class="sxs-lookup"><span data-stu-id="d96e5-105">For example, you can:</span></span>

* <span data-ttu-id="d96e5-106">Ütemezhet egy munkafolyamat toorun SQL tárolt eljárás minden nap.</span><span class="sxs-lookup"><span data-stu-id="d96e5-106">Schedule a workflow toorun a SQL stored procedure every day.</span></span>
* <span data-ttu-id="d96e5-107">Az e-mail hello kapcsolatos egy bizonyos hashtaggel történő múlt héten belül minden Twitter-üzeneteket összegzését.</span><span class="sxs-lookup"><span data-stu-id="d96e5-107">Email a summary of all tweets within hello last week about a certain hashtag.</span></span>

<span data-ttu-id="d96e5-108">tooget használatának hello ismétlődési eseményindító a logikai alkalmazás, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="d96e5-108">tooget started using hello recurrence trigger in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-a-recurrence-trigger"></a><span data-ttu-id="d96e5-109">Ismétlődés eseményindítók</span><span class="sxs-lookup"><span data-stu-id="d96e5-109">Use a recurrence trigger</span></span>
<span data-ttu-id="d96e5-110">Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény.</span><span class="sxs-lookup"><span data-stu-id="d96e5-110">A trigger is an event that can be used toostart hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="d96e5-111">[További tudnivalók az eseményindítók](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d96e5-111">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="d96e5-112">Íme egy parancssorozat-példa hogyan tooset ismétlődése be trigger logikai alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="d96e5-112">Here’s an example sequence of how tooset up a recurrence trigger in a logic app:</span></span>

1. <span data-ttu-id="d96e5-113">Adja hozzá a hello **ismétlődési** hello első lépéseként logikai alkalmazás eseményindító.</span><span class="sxs-lookup"><span data-stu-id="d96e5-113">Add hello **Recurrence** trigger as hello first step in a logic app.</span></span>
2. <span data-ttu-id="d96e5-114">Töltse ki hello paraméterek hello ismétlési időköz.</span><span class="sxs-lookup"><span data-stu-id="d96e5-114">Fill in hello parameters for hello recurrence interval.</span></span>

<span data-ttu-id="d96e5-115">hello logikai alkalmazás most futtató minden időköz után elindul.</span><span class="sxs-lookup"><span data-stu-id="d96e5-115">hello logic app now starts a run after each interval of time.</span></span>

![HTTP eseményindító](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a><span data-ttu-id="d96e5-117">Eseményindító részletei</span><span class="sxs-lookup"><span data-stu-id="d96e5-117">Trigger details</span></span>
<span data-ttu-id="d96e5-118">hello ismétlődési eseményindító következő konfigurálható tulajdonságai hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d96e5-118">hello recurrence trigger has hello following properties that you can configure.</span></span>

<span data-ttu-id="d96e5-119">A logikai alkalmazást a megadott időtartam után következik be.</span><span class="sxs-lookup"><span data-stu-id="d96e5-119">It fires a logic app after a specified time interval.</span></span>
<span data-ttu-id="d96e5-120">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="d96e5-120">A * means that it is a required field.</span></span>

| <span data-ttu-id="d96e5-121">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="d96e5-121">Display name</span></span> | <span data-ttu-id="d96e5-122">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="d96e5-122">Property name</span></span> | <span data-ttu-id="d96e5-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="d96e5-123">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d96e5-124">Gyakoriság *</span><span class="sxs-lookup"><span data-stu-id="d96e5-124">Frequency*</span></span> |<span data-ttu-id="d96e5-125">frequency</span><span class="sxs-lookup"><span data-stu-id="d96e5-125">frequency</span></span> |<span data-ttu-id="d96e5-126">hello időegység: `Second`, `Minute`, `Hour`, `Day`, vagy `Year`.</span><span class="sxs-lookup"><span data-stu-id="d96e5-126">hello unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.</span></span> |
| <span data-ttu-id="d96e5-127">Időköz *</span><span class="sxs-lookup"><span data-stu-id="d96e5-127">Interval*</span></span> |<span data-ttu-id="d96e5-128">interval</span><span class="sxs-lookup"><span data-stu-id="d96e5-128">interval</span></span> |<span data-ttu-id="d96e5-129">hello hello tartozó hello Ismétlődési gyakoriság megadott időintervallum.</span><span class="sxs-lookup"><span data-stu-id="d96e5-129">hello interval of hello given frequency for hello recurrence.</span></span> |
| <span data-ttu-id="d96e5-130">Időzóna</span><span class="sxs-lookup"><span data-stu-id="d96e5-130">Time Zone</span></span> |<span data-ttu-id="d96e5-131">Időzóna</span><span class="sxs-lookup"><span data-stu-id="d96e5-131">timeZone</span></span> |<span data-ttu-id="d96e5-132">Ha a kezdési idő áll rendelkezésre a UTC eltolás nélkül, ezt az időzónát használható.</span><span class="sxs-lookup"><span data-stu-id="d96e5-132">If a start time is provided without a UTC offset, this time zone will be used.</span></span> |
| <span data-ttu-id="d96e5-133">Kezdési idő</span><span class="sxs-lookup"><span data-stu-id="d96e5-133">Start time</span></span> |<span data-ttu-id="d96e5-134">startTime</span><span class="sxs-lookup"><span data-stu-id="d96e5-134">startTime</span></span> |<span data-ttu-id="d96e5-135">hello kezdési időpontja [ISO 8601 formátum](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).</span><span class="sxs-lookup"><span data-stu-id="d96e5-135">hello start time in [ISO 8601 format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).</span></span> |

<br>

## <a name="next-steps"></a><span data-ttu-id="d96e5-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d96e5-136">Next steps</span></span>
<span data-ttu-id="d96e5-137">Most, próbálja ki hello platform és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="d96e5-137">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="d96e5-138">Ismerje meg a hello más rendelkezésre álló összekötők logic Apps alkalmazások felmérésével a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="d96e5-138">You can explore hello other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

