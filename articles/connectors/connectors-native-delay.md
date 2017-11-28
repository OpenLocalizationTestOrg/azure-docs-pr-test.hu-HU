---
title: "a logic apps késés aaaAdd |} Microsoft Docs"
description: "Hello áttekintése késleltetés és a késleltetés-műveletek, amíg és hogyan toouse őket az az Azure Logic Apps alkalmazást."
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
ms.openlocfilehash: e5bc9d639adbddc01ee0f6a4c68716f586d4344a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-delay-and-delay-until-actions"></a><span data-ttu-id="6cc75-103">Ismerkedés a hello késleltetés és a késleltetés-amíg műveletek</span><span class="sxs-lookup"><span data-stu-id="6cc75-103">Get started with hello delay and delay-until actions</span></span>
<span data-ttu-id="6cc75-104">Hello késleltetés használatával és a "késleltetés-amíg" műveletek, befejezheti a munkafolyamatokban.</span><span class="sxs-lookup"><span data-stu-id="6cc75-104">By using hello delay and "delay-until" actions, you can complete workflow scenarios.</span></span>

<span data-ttu-id="6cc75-105">Megteheti például a következőt:</span><span class="sxs-lookup"><span data-stu-id="6cc75-105">For example, you can:</span></span>

* <span data-ttu-id="6cc75-106">Várjon, amíg a hét napja toosend állapot frissítése keresztül e-mailt.</span><span class="sxs-lookup"><span data-stu-id="6cc75-106">Wait until a weekday toosend a status update over email.</span></span>
* <span data-ttu-id="6cc75-107">Késleltetés hello munkafolyamat csak egy HTTP-hívás idő toofinish rendelkezik folytatása és hello eredmény beolvasása előtt.</span><span class="sxs-lookup"><span data-stu-id="6cc75-107">Delay hello workflow until an HTTP call has time toofinish before resuming and retrieving hello result.</span></span>

<span data-ttu-id="6cc75-108">tooget lépések hello késleltetés műveletével logikai alkalmazás, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="6cc75-108">tooget started using hello delay action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-hello-delay-actions"></a><span data-ttu-id="6cc75-109">Hello késleltetés műveletek használata</span><span class="sxs-lookup"><span data-stu-id="6cc75-109">Use hello delay actions</span></span>
<span data-ttu-id="6cc75-110">Egy művelet során, amely logikai alkalmazás definiált hello munkafolyamat végzi.</span><span class="sxs-lookup"><span data-stu-id="6cc75-110">An action is an operation that is carried out by hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="6cc75-111">[További információ a műveletek](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6cc75-111">[Learn more about actions](connectors-overview.md).</span></span>

<span data-ttu-id="6cc75-112">Íme egy parancssorozat-példa hogyan toouse késleltetést logikai alkalmazás lépést:</span><span class="sxs-lookup"><span data-stu-id="6cc75-112">Here’s an example sequence of how toouse a delay step in a logic app:</span></span>

1. <span data-ttu-id="6cc75-113">A felvett egy eseményindítót, kattintson a **új lépés** tooadd műveletet.</span><span class="sxs-lookup"><span data-stu-id="6cc75-113">After adding a trigger, click **New Step** tooadd an action.</span></span>
2. <span data-ttu-id="6cc75-114">Keresse meg **késedelem** toobring hello késleltetés műveletek.</span><span class="sxs-lookup"><span data-stu-id="6cc75-114">Search for **delay** toobring up hello delay actions.</span></span> <span data-ttu-id="6cc75-115">Ebben a példában, azt fogja kiválasztani **késleltetés**.</span><span class="sxs-lookup"><span data-stu-id="6cc75-115">In this example, we will select **Delay**.</span></span>
   
    ![Késleltetés műveletek](./media/connectors-native-delay/using-action-1.png)
3. <span data-ttu-id="6cc75-117">Hajtsa végre a hello művelet tulajdonságok tooconfigure hello késedelem.</span><span class="sxs-lookup"><span data-stu-id="6cc75-117">Complete any of hello action properties tooconfigure hello delay.</span></span>
   
    ![Késleltetési konfiguráció](./media/connectors-native-delay/using-action-2.png)
4. <span data-ttu-id="6cc75-119">Kattintson a **mentése** toopublish és hello logikai alkalmazás aktiválása.</span><span class="sxs-lookup"><span data-stu-id="6cc75-119">Click **Save** toopublish and activate hello logic app.</span></span>

## <a name="action-details"></a><span data-ttu-id="6cc75-120">A művelet részletei</span><span class="sxs-lookup"><span data-stu-id="6cc75-120">Action details</span></span>
<span data-ttu-id="6cc75-121">hello ismétlődési eseményindító következő konfigurálható tulajdonságai hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6cc75-121">hello recurrence trigger has hello following properties that can be configured.</span></span>

### <a name="delay-action"></a><span data-ttu-id="6cc75-122">Késleltetés művelet</span><span class="sxs-lookup"><span data-stu-id="6cc75-122">Delay action</span></span>
<span data-ttu-id="6cc75-123">Ez a művelet késleltetése hello futtassa egy meghatározott időtartam alatt.</span><span class="sxs-lookup"><span data-stu-id="6cc75-123">This action delays hello run for a certain time interval.</span></span>
<span data-ttu-id="6cc75-124">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="6cc75-124">A * means that it is a required field.</span></span>

| <span data-ttu-id="6cc75-125">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="6cc75-125">Display name</span></span> | <span data-ttu-id="6cc75-126">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="6cc75-126">Property name</span></span> | <span data-ttu-id="6cc75-127">Leírás</span><span class="sxs-lookup"><span data-stu-id="6cc75-127">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6cc75-128">Száma *</span><span class="sxs-lookup"><span data-stu-id="6cc75-128">Count*</span></span> |<span data-ttu-id="6cc75-129">Száma</span><span class="sxs-lookup"><span data-stu-id="6cc75-129">count</span></span> |<span data-ttu-id="6cc75-130">idő egységek toodelay hello száma</span><span class="sxs-lookup"><span data-stu-id="6cc75-130">hello number of time units toodelay</span></span> |
| <span data-ttu-id="6cc75-131">Egység *</span><span class="sxs-lookup"><span data-stu-id="6cc75-131">Unit*</span></span> |<span data-ttu-id="6cc75-132">egység</span><span class="sxs-lookup"><span data-stu-id="6cc75-132">unit</span></span> |<span data-ttu-id="6cc75-133">hello időegység: `Second`, `Minute`, `Hour`, vagy`Day`</span><span class="sxs-lookup"><span data-stu-id="6cc75-133">hello unit of time: `Second`, `Minute`, `Hour`, or `Day`</span></span> |

<br>

### <a name="delay-until-action"></a><span data-ttu-id="6cc75-134">Késleltetés-amíg művelet</span><span class="sxs-lookup"><span data-stu-id="6cc75-134">Delay-until action</span></span>
<span data-ttu-id="6cc75-135">Ez a művelet a megadott dátum és idő futását hello késlelteti.</span><span class="sxs-lookup"><span data-stu-id="6cc75-135">This action delays hello run until a specified date/time.</span></span>
<span data-ttu-id="6cc75-136">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="6cc75-136">A * means that it is a required field.</span></span>

| <span data-ttu-id="6cc75-137">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="6cc75-137">Display name</span></span> | <span data-ttu-id="6cc75-138">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="6cc75-138">Property name</span></span> | <span data-ttu-id="6cc75-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="6cc75-139">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6cc75-140">Év *</span><span class="sxs-lookup"><span data-stu-id="6cc75-140">Year*</span></span> |<span data-ttu-id="6cc75-141">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="6cc75-141">timestamp</span></span> |<span data-ttu-id="6cc75-142">hello év toodelay amíg (GMT)</span><span class="sxs-lookup"><span data-stu-id="6cc75-142">hello year toodelay until (GMT)</span></span> |
| <span data-ttu-id="6cc75-143">Hónap *</span><span class="sxs-lookup"><span data-stu-id="6cc75-143">Month*</span></span> |<span data-ttu-id="6cc75-144">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="6cc75-144">timestamp</span></span> |<span data-ttu-id="6cc75-145">hello hónap toodelay amíg (GMT)</span><span class="sxs-lookup"><span data-stu-id="6cc75-145">hello month toodelay until (GMT)</span></span> |
| <span data-ttu-id="6cc75-146">Nap *</span><span class="sxs-lookup"><span data-stu-id="6cc75-146">Day*</span></span> |<span data-ttu-id="6cc75-147">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="6cc75-147">timestamp</span></span> |<span data-ttu-id="6cc75-148">hello nap toodelay amíg (GMT)</span><span class="sxs-lookup"><span data-stu-id="6cc75-148">hello day toodelay until (GMT)</span></span> |

<br>

## <a name="next-steps"></a><span data-ttu-id="6cc75-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6cc75-149">Next steps</span></span>
<span data-ttu-id="6cc75-150">Most, próbálja ki hello platform és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="6cc75-150">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="6cc75-151">Ismerje meg a hello más rendelkezésre álló összekötők logic Apps alkalmazások felmérésével a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="6cc75-151">You can explore hello other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

