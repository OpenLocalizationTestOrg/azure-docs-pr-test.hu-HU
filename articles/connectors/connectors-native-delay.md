---
title: "Adja hozzá a logic apps késleltetést |} Microsoft Docs"
description: "A késleltetés és a késleltetés áttekintése-műveletek, és hogyan érdemes azokat használni az Azure Logic Apps alkalmazást."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 915f48bf-3bd8-4656-be73-91a941d0afcd
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 5f4f7052d48b4ca4ed91212d970551141e78e852
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-delay-and-delay-until-actions"></a><span data-ttu-id="3f256-103">Bevezetés az a késleltetés és a késleltetés-ig műveletek</span><span class="sxs-lookup"><span data-stu-id="3f256-103">Get started with the delay and delay-until actions</span></span>
<span data-ttu-id="3f256-104">A késleltetés használatával és a "késleltetés-amíg" műveletek, befejezheti a munkafolyamatokban.</span><span class="sxs-lookup"><span data-stu-id="3f256-104">By using the delay and "delay-until" actions, you can complete workflow scenarios.</span></span>

<span data-ttu-id="3f256-105">Megteheti például a következőt:</span><span class="sxs-lookup"><span data-stu-id="3f256-105">For example, you can:</span></span>

* <span data-ttu-id="3f256-106">Várjon, amíg a hét napja állapotfrissítés keresztül e-mailek küldése.</span><span class="sxs-lookup"><span data-stu-id="3f256-106">Wait until a weekday to send a status update over email.</span></span>
* <span data-ttu-id="3f256-107">A munkafolyamat elhalasztani, amíg egy HTTP-hívás befejezését, mielőtt folytatja a futtatását, és az eredmény beolvasása idő tartozik.</span><span class="sxs-lookup"><span data-stu-id="3f256-107">Delay the workflow until an HTTP call has time to finish before resuming and retrieving the result.</span></span>

<span data-ttu-id="3f256-108">Első lépések egy logikai alkalmazás késleltetés műveletével, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="3f256-108">To get started using the delay action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-the-delay-actions"></a><span data-ttu-id="3f256-109">A késleltetés műveletek használata</span><span class="sxs-lookup"><span data-stu-id="3f256-109">Use the delay actions</span></span>
<span data-ttu-id="3f256-110">Egy művelet során, amely a logikai alkalmazás definiált munkafolyamat végzi.</span><span class="sxs-lookup"><span data-stu-id="3f256-110">An action is an operation that is carried out by the workflow that is defined in a logic app.</span></span> <span data-ttu-id="3f256-111">[További információ a műveletek](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3f256-111">[Learn more about actions](connectors-overview.md).</span></span>

<span data-ttu-id="3f256-112">Íme egy parancssorozat-példa a logikai alkalmazás egy késleltetés lépés használata:</span><span class="sxs-lookup"><span data-stu-id="3f256-112">Here’s an example sequence of how to use a delay step in a logic app:</span></span>

1. <span data-ttu-id="3f256-113">A felvett egy eseményindítót, kattintson a **új lépés** művelet hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="3f256-113">After adding a trigger, click **New Step** to add an action.</span></span>
2. <span data-ttu-id="3f256-114">Keresse meg **késleltetés** a késleltetés műveletek elindítani.</span><span class="sxs-lookup"><span data-stu-id="3f256-114">Search for **delay** to bring up the delay actions.</span></span> <span data-ttu-id="3f256-115">Ebben a példában, azt fogja kiválasztani **késleltetés**.</span><span class="sxs-lookup"><span data-stu-id="3f256-115">In this example, we will select **Delay**.</span></span>
   
    ![Késleltetés műveletek](./media/connectors-native-delay/using-action-1.png)
3. <span data-ttu-id="3f256-117">A művelet tulajdonságait, a késés konfigurálásához hajtsa végre.</span><span class="sxs-lookup"><span data-stu-id="3f256-117">Complete any of the action properties to configure the delay.</span></span>
   
    ![Késleltetési konfiguráció](./media/connectors-native-delay/using-action-2.png)
4. <span data-ttu-id="3f256-119">Kattintson a **mentése** közzététele, és aktiválja a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3f256-119">Click **Save** to publish and activate the logic app.</span></span>

## <a name="action-details"></a><span data-ttu-id="3f256-120">A művelet részletei</span><span class="sxs-lookup"><span data-stu-id="3f256-120">Action details</span></span>
<span data-ttu-id="3f256-121">Az ismétlődési eseményindító tulajdonságai a következők konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="3f256-121">The recurrence trigger has the following properties that can be configured.</span></span>

### <a name="delay-action"></a><span data-ttu-id="3f256-122">Késleltetés művelet</span><span class="sxs-lookup"><span data-stu-id="3f256-122">Delay action</span></span>
<span data-ttu-id="3f256-123">Ez a művelet késlelteti a Futtatás egy meghatározott időtartam alatt.</span><span class="sxs-lookup"><span data-stu-id="3f256-123">This action delays the run for a certain time interval.</span></span>
<span data-ttu-id="3f256-124">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="3f256-124">A * means that it is a required field.</span></span>

| <span data-ttu-id="3f256-125">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="3f256-125">Display name</span></span> | <span data-ttu-id="3f256-126">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="3f256-126">Property name</span></span> | <span data-ttu-id="3f256-127">Leírás</span><span class="sxs-lookup"><span data-stu-id="3f256-127">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3f256-128">Száma *</span><span class="sxs-lookup"><span data-stu-id="3f256-128">Count*</span></span> |<span data-ttu-id="3f256-129">Száma</span><span class="sxs-lookup"><span data-stu-id="3f256-129">count</span></span> |<span data-ttu-id="3f256-130">A késleltetési idő egységek száma</span><span class="sxs-lookup"><span data-stu-id="3f256-130">The number of time units to delay</span></span> |
| <span data-ttu-id="3f256-131">Egység *</span><span class="sxs-lookup"><span data-stu-id="3f256-131">Unit*</span></span> |<span data-ttu-id="3f256-132">egység</span><span class="sxs-lookup"><span data-stu-id="3f256-132">unit</span></span> |<span data-ttu-id="3f256-133">Időegység: `Second`, `Minute`, `Hour`, vagy`Day`</span><span class="sxs-lookup"><span data-stu-id="3f256-133">The unit of time: `Second`, `Minute`, `Hour`, or `Day`</span></span> |

<br>

### <a name="delay-until-action"></a><span data-ttu-id="3f256-134">Késleltetés-amíg művelet</span><span class="sxs-lookup"><span data-stu-id="3f256-134">Delay-until action</span></span>
<span data-ttu-id="3f256-135">Ez a művelet késlelteti a Futtatás csak a megadott dátum és idő.</span><span class="sxs-lookup"><span data-stu-id="3f256-135">This action delays the run until a specified date/time.</span></span>
<span data-ttu-id="3f256-136">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="3f256-136">A * means that it is a required field.</span></span>

| <span data-ttu-id="3f256-137">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="3f256-137">Display name</span></span> | <span data-ttu-id="3f256-138">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="3f256-138">Property name</span></span> | <span data-ttu-id="3f256-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="3f256-139">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3f256-140">Év *</span><span class="sxs-lookup"><span data-stu-id="3f256-140">Year*</span></span> |<span data-ttu-id="3f256-141">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="3f256-141">timestamp</span></span> |<span data-ttu-id="3f256-142">Az év (GMT) amíg késleltetése</span><span class="sxs-lookup"><span data-stu-id="3f256-142">The year to delay until (GMT)</span></span> |
| <span data-ttu-id="3f256-143">Hónap *</span><span class="sxs-lookup"><span data-stu-id="3f256-143">Month*</span></span> |<span data-ttu-id="3f256-144">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="3f256-144">timestamp</span></span> |<span data-ttu-id="3f256-145">A hónap (GMT) amíg késleltetése</span><span class="sxs-lookup"><span data-stu-id="3f256-145">The month to delay until (GMT)</span></span> |
| <span data-ttu-id="3f256-146">Nap *</span><span class="sxs-lookup"><span data-stu-id="3f256-146">Day*</span></span> |<span data-ttu-id="3f256-147">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="3f256-147">timestamp</span></span> |<span data-ttu-id="3f256-148">A nap (GMT) amíg késleltetése</span><span class="sxs-lookup"><span data-stu-id="3f256-148">The day to delay until (GMT)</span></span> |

<br>

## <a name="next-steps"></a><span data-ttu-id="3f256-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3f256-149">Next steps</span></span>
<span data-ttu-id="3f256-150">Most, próbálja ki a platformot és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="3f256-150">Now, try out the platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="3f256-151">Az egyéb rendelkezésre álló összekötők logic Apps alkalmazások felmérésével felfedezheti a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="3f256-151">You can explore the other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

