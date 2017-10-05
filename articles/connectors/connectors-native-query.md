---
title: "A lekérdezési művelet hozzáadása a logic apps |} Microsoft Docs"
description: "A lekérdezés művelet, például a szűrő tömb műveletek végrehajtásához áttekintése."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 34e702c7-f9e5-4885-9266-fc7404adecfe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2016
ms.author: jehollan
ms.openlocfilehash: a992fa17a07d6167297c4cf5fe9fb3b58181d7df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-query-action"></a><span data-ttu-id="3b4ce-103">Ismerkedjen meg a lekérdezési művelet</span><span class="sxs-lookup"><span data-stu-id="3b4ce-103">Get started with the query action</span></span>
<span data-ttu-id="3b4ce-104">A lekérdezési művelet használatával is dolgozhat kötegek és tömbök munkafolyamatokat elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="3b4ce-104">By using the query action, you can work with batches and arrays to accomplish workflows to:</span></span>

* <span data-ttu-id="3b4ce-105">Hozzon létre egy feladatot az összes magasabb prioritású rekord adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-105">Create a task for all high-priority records from a database.</span></span>
* <span data-ttu-id="3b4ce-106">Az e-mailekhez, az Azure-blobot összes PDF melléklet mentése.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-106">Save all PDF attachments for emails into an Azure blob.</span></span>

<span data-ttu-id="3b4ce-107">Első lépések egy logikai alkalmazás lekérdezés műveletével, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="3b4ce-107">To get started using the query action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-the-query-action"></a><span data-ttu-id="3b4ce-108">A lekérdezés művelettel</span><span class="sxs-lookup"><span data-stu-id="3b4ce-108">Use the query action</span></span>
<span data-ttu-id="3b4ce-109">Egy művelet során, amely a logikai alkalmazás definiált munkafolyamat végzi.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-109">An action is an operation that is carried out by the workflow that is defined in a logic app.</span></span> <span data-ttu-id="3b4ce-110">[További információ a műveletek](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3b4ce-110">[Learn more about actions](connectors-overview.md).</span></span>  

<span data-ttu-id="3b4ce-111">A lekérdezés jelenleg műveletnek egy művelet, a szűrő tömb, a tervezőben elérhetővé neve.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-111">The query action currently has one operation, called the filter array, that is exposed in the designer.</span></span> <span data-ttu-id="3b4ce-112">Ez lehetővé teszi egy tömb lekérdezése, és a szűrt eredmények készlettel tért vissza.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-112">This allows you to query an array and return a set of filtered results.</span></span>

<span data-ttu-id="3b4ce-113">Itt látható, hogyan adhat hozzá azt a logikai alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="3b4ce-113">Here's how you can add it in a logic app:</span></span>

1. <span data-ttu-id="3b4ce-114">Válassza ki a **új lépés** gombra.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-114">Select the **New Step** button.</span></span>
2. <span data-ttu-id="3b4ce-115">Válasszon **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-115">Choose **Add an action**.</span></span>
3. <span data-ttu-id="3b4ce-116">A művelet a keresőmezőbe írja be **szűrő** listájára a **szűrő tömb** művelet.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-116">In the action search box, type **filter** to list the **Filter array** action.</span></span>
   
    ![Válassza ki a lekérdezés](./media/connectors-native-query/using-action-1.png)
4. <span data-ttu-id="3b4ce-118">Válassza ki a tömb szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-118">Select an array to filter.</span></span> <span data-ttu-id="3b4ce-119">(Az alábbi képernyőfelvételen látható Twitter keresési eredmények tömbje.)</span><span class="sxs-lookup"><span data-stu-id="3b4ce-119">(The following screenshot shows the array of results from a Twitter search.)</span></span>
5. <span data-ttu-id="3b4ce-120">Minden egyes kiértékelése feltétel létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-120">Create a condition to evaluate on each item.</span></span> <span data-ttu-id="3b4ce-121">(Az alábbi képernyőfelvételen szűrők a felhasználók, akik több mint 100 followers Twitter-üzeneteket.)</span><span class="sxs-lookup"><span data-stu-id="3b4ce-121">(The following screenshot filters tweets from users who have more than 100 followers.)</span></span>
   
    ![A lekérdezés a művelet](./media/connectors-native-query/using-action-2.png)
   
    <span data-ttu-id="3b4ce-123">A művelet egy új tömb csak, amely megfelel a szűrő eredményeit tartalmazó fog kimeneti.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-123">The action will output a new array that contains only results that met the filter requirements.</span></span>
6. <span data-ttu-id="3b4ce-124">Kattintson az eszköztáron menteni a bal felső sarkában, és a Logic Apps alkalmazást mentése és közzététele (aktiválása).</span><span class="sxs-lookup"><span data-stu-id="3b4ce-124">Click the upper-left corner of the toolbar to save, and your logic app will both save and publish (activate).</span></span>

## <a name="query-action"></a><span data-ttu-id="3b4ce-125">Lekérdezési művelet</span><span class="sxs-lookup"><span data-stu-id="3b4ce-125">Query action</span></span>
<span data-ttu-id="3b4ce-126">Az alábbiak a művelet, amely támogatja ezt az összekötőt.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-126">Here are the details for the action that this connector supports.</span></span> <span data-ttu-id="3b4ce-127">Az összekötő egy lehetséges művelettel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-127">The connector has one possible action.</span></span>

| <span data-ttu-id="3b4ce-128">Műveletek</span><span class="sxs-lookup"><span data-stu-id="3b4ce-128">Action</span></span> | <span data-ttu-id="3b4ce-129">Leírás</span><span class="sxs-lookup"><span data-stu-id="3b4ce-129">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3b4ce-130">Szűrő tömb</span><span class="sxs-lookup"><span data-stu-id="3b4ce-130">Filter array</span></span> |<span data-ttu-id="3b4ce-131">Minden elem tömbben feltételt ad meg, és az eredményt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-131">Evaluates a condition for each item in an array and returns the results</span></span> |

## <a name="action-details"></a><span data-ttu-id="3b4ce-132">A művelet részletei</span><span class="sxs-lookup"><span data-stu-id="3b4ce-132">Action details</span></span>
<span data-ttu-id="3b4ce-133">A lekérdezési művelet egy lehetséges műveletet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-133">The query action comes with one possible action.</span></span> <span data-ttu-id="3b4ce-134">A következő táblázatok ismertetik, hogy a művelet és a megfelelő kimeneti részletek művelettel társított szükséges és választható bemeneti mező.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-134">The following tables describe the required and optional input fields for the action and the corresponding output details that are associated with using the action.</span></span>

### <a name="filter-array"></a><span data-ttu-id="3b4ce-135">Szűrő tömb</span><span class="sxs-lookup"><span data-stu-id="3b4ce-135">Filter array</span></span>
<span data-ttu-id="3b4ce-136">A művelet, amely a kimenő HTTP-kérelem a beviteli mezők a következők:</span><span class="sxs-lookup"><span data-stu-id="3b4ce-136">The following are input fields for the action, which makes an HTTP outbound request.</span></span>
<span data-ttu-id="3b4ce-137">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-137">A * means that it is a required field.</span></span>

| <span data-ttu-id="3b4ce-138">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="3b4ce-138">Display name</span></span> | <span data-ttu-id="3b4ce-139">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="3b4ce-139">Property name</span></span> | <span data-ttu-id="3b4ce-140">Leírás</span><span class="sxs-lookup"><span data-stu-id="3b4ce-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b4ce-141">A *</span><span class="sxs-lookup"><span data-stu-id="3b4ce-141">From*</span></span> |<span data-ttu-id="3b4ce-142">A</span><span class="sxs-lookup"><span data-stu-id="3b4ce-142">from</span></span> |<span data-ttu-id="3b4ce-143">A tömb szűrése</span><span class="sxs-lookup"><span data-stu-id="3b4ce-143">The array to filter</span></span> |
| <span data-ttu-id="3b4ce-144">Az állapot *</span><span class="sxs-lookup"><span data-stu-id="3b4ce-144">Condition*</span></span> |<span data-ttu-id="3b4ce-145">Ha</span><span class="sxs-lookup"><span data-stu-id="3b4ce-145">where</span></span> |<span data-ttu-id="3b4ce-146">A feltétel kiértékelése minden elemhez</span><span class="sxs-lookup"><span data-stu-id="3b4ce-146">The condition to evaluate for each item</span></span> |

<br>

### <a name="output-details"></a><span data-ttu-id="3b4ce-147">Kimeneti részletei</span><span class="sxs-lookup"><span data-stu-id="3b4ce-147">Output details</span></span>
<span data-ttu-id="3b4ce-148">A HTTP-válasz kimeneti részleteit az alábbiakban.</span><span class="sxs-lookup"><span data-stu-id="3b4ce-148">The following are output details for the HTTP response.</span></span>

| <span data-ttu-id="3b4ce-149">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="3b4ce-149">Property name</span></span> | <span data-ttu-id="3b4ce-150">Adattípus</span><span class="sxs-lookup"><span data-stu-id="3b4ce-150">Data type</span></span> | <span data-ttu-id="3b4ce-151">Leírás</span><span class="sxs-lookup"><span data-stu-id="3b4ce-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3b4ce-152">Szűrt tömb</span><span class="sxs-lookup"><span data-stu-id="3b4ce-152">Filtered array</span></span> |<span data-ttu-id="3b4ce-153">A tömb</span><span class="sxs-lookup"><span data-stu-id="3b4ce-153">array</span></span> |<span data-ttu-id="3b4ce-154">Olyan tömb, amely minden szűrt eredmény objektumot tartalmaz</span><span class="sxs-lookup"><span data-stu-id="3b4ce-154">An array that contains an object for each filtered result</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3b4ce-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3b4ce-155">Next steps</span></span>
<span data-ttu-id="3b4ce-156">Most, próbálja ki a platformot és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="3b4ce-156">Now, try out the platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="3b4ce-157">Az egyéb rendelkezésre álló összekötők logic Apps alkalmazások felmérésével felfedezheti a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="3b4ce-157">You can explore the other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

