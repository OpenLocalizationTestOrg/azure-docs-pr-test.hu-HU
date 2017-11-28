---
title: "a logic apps aaaAdd hello lekérdezési művelet |} Microsoft Docs"
description: "A műveleteket, például a szűrő tömb hello lekérdezés műveletei áttekintése."
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
ms.openlocfilehash: 3d4be901e7e6bf1b644057648930667ab34f2124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-query-action"></a><span data-ttu-id="b30b9-103">Ismerkedés a hello lekérdezési művelet</span><span class="sxs-lookup"><span data-stu-id="b30b9-103">Get started with hello query action</span></span>
<span data-ttu-id="b30b9-104">Hello lekérdezési művelet használatával kötegek és tömbök tooaccomplish munkafolyamatokat dolgozhat:</span><span class="sxs-lookup"><span data-stu-id="b30b9-104">By using hello query action, you can work with batches and arrays tooaccomplish workflows to:</span></span>

* <span data-ttu-id="b30b9-105">Hozzon létre egy feladatot az összes magasabb prioritású rekord adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="b30b9-105">Create a task for all high-priority records from a database.</span></span>
* <span data-ttu-id="b30b9-106">Az e-mailekhez, az Azure-blobot összes PDF melléklet mentése.</span><span class="sxs-lookup"><span data-stu-id="b30b9-106">Save all PDF attachments for emails into an Azure blob.</span></span>

<span data-ttu-id="b30b9-107">tooget használatának hello lekérdezési művelet egy logic App, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="b30b9-107">tooget started using hello query action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-hello-query-action"></a><span data-ttu-id="b30b9-108">Hello lekérdezés művelettel</span><span class="sxs-lookup"><span data-stu-id="b30b9-108">Use hello query action</span></span>
<span data-ttu-id="b30b9-109">Egy művelet során, amely logikai alkalmazás definiált hello munkafolyamat végzi.</span><span class="sxs-lookup"><span data-stu-id="b30b9-109">An action is an operation that is carried out by hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="b30b9-110">[További információ a műveletek](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b30b9-110">[Learn more about actions](connectors-overview.md).</span></span>  

<span data-ttu-id="b30b9-111">hello lekérdezés jelenleg műveletnek egy művelet hívása hello szűrő tömb, hello Designer elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="b30b9-111">hello query action currently has one operation, called hello filter array, that is exposed in hello designer.</span></span> <span data-ttu-id="b30b9-112">Ez lehetővé teszi egy tömb tooquery, és vissza szűrt eredmények.</span><span class="sxs-lookup"><span data-stu-id="b30b9-112">This allows you tooquery an array and return a set of filtered results.</span></span>

<span data-ttu-id="b30b9-113">Itt látható, hogyan adhat hozzá azt a logikai alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="b30b9-113">Here's how you can add it in a logic app:</span></span>

1. <span data-ttu-id="b30b9-114">Jelölje be hello **új lépés** gombra.</span><span class="sxs-lookup"><span data-stu-id="b30b9-114">Select hello **New Step** button.</span></span>
2. <span data-ttu-id="b30b9-115">Válasszon **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="b30b9-115">Choose **Add an action**.</span></span>
3. <span data-ttu-id="b30b9-116">Hello művelet keresési mezőbe, írja be a **szűrő** toolist hello **szűrő tömb** művelet.</span><span class="sxs-lookup"><span data-stu-id="b30b9-116">In hello action search box, type **filter** toolist hello **Filter array** action.</span></span>
   
    ![Válassza ki a hello lekérdezési művelet](./media/connectors-native-query/using-action-1.png)
4. <span data-ttu-id="b30b9-118">Válasszon egy tömb toofilter.</span><span class="sxs-lookup"><span data-stu-id="b30b9-118">Select an array toofilter.</span></span> <span data-ttu-id="b30b9-119">(hello következő képernyőfelvétel egy Twitter-keresés eredményei hello tömbje.)</span><span class="sxs-lookup"><span data-stu-id="b30b9-119">(hello following screenshot shows hello array of results from a Twitter search.)</span></span>
5. <span data-ttu-id="b30b9-120">Hozzon létre egy feltétel tooevaluate minden elemhez.</span><span class="sxs-lookup"><span data-stu-id="b30b9-120">Create a condition tooevaluate on each item.</span></span> <span data-ttu-id="b30b9-121">(a következő képernyőkép hello szűrők a felhasználók, akik több mint 100 followers Twitter-üzeneteket.)</span><span class="sxs-lookup"><span data-stu-id="b30b9-121">(hello following screenshot filters tweets from users who have more than 100 followers.)</span></span>
   
    ![Teljes hello lekérdezési művelet](./media/connectors-native-query/using-action-2.png)
   
    <span data-ttu-id="b30b9-123">hello művelet kimeneti fog egy új, csak a hello szűrő megfelel eredményeket tartalmazó tömb.</span><span class="sxs-lookup"><span data-stu-id="b30b9-123">hello action will output a new array that contains only results that met hello filter requirements.</span></span>
6. <span data-ttu-id="b30b9-124">Kattintson a bal felső sarkának hello hello eszköztár toosave, és a logic app fog egyaránt mentés, és tegye közzé (aktiválása).</span><span class="sxs-lookup"><span data-stu-id="b30b9-124">Click hello upper-left corner of hello toolbar toosave, and your logic app will both save and publish (activate).</span></span>

## <a name="query-action"></a><span data-ttu-id="b30b9-125">Lekérdezési művelet</span><span class="sxs-lookup"><span data-stu-id="b30b9-125">Query action</span></span>
<span data-ttu-id="b30b9-126">Az alábbiakban hello részleteit, amely támogatja ezt az összekötőt hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="b30b9-126">Here are hello details for hello action that this connector supports.</span></span> <span data-ttu-id="b30b9-127">hello összekötő lehetséges egy-egy művelettel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b30b9-127">hello connector has one possible action.</span></span>

| <span data-ttu-id="b30b9-128">Műveletek</span><span class="sxs-lookup"><span data-stu-id="b30b9-128">Action</span></span> | <span data-ttu-id="b30b9-129">Leírás</span><span class="sxs-lookup"><span data-stu-id="b30b9-129">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b30b9-130">Szűrő tömb</span><span class="sxs-lookup"><span data-stu-id="b30b9-130">Filter array</span></span> |<span data-ttu-id="b30b9-131">Minden elem tömbben feltételt ad meg, és hello eredményt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="b30b9-131">Evaluates a condition for each item in an array and returns hello results</span></span> |

## <a name="action-details"></a><span data-ttu-id="b30b9-132">A művelet részletei</span><span class="sxs-lookup"><span data-stu-id="b30b9-132">Action details</span></span>
<span data-ttu-id="b30b9-133">hello lekérdezési művelet egy lehetséges műveletet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b30b9-133">hello query action comes with one possible action.</span></span> <span data-ttu-id="b30b9-134">hello alábbi táblázatok bemutatják az hello szükséges és választható beviteli mezők hello művelet és hello megfelelő kimeneti részletekért társított hello műveletével.</span><span class="sxs-lookup"><span data-stu-id="b30b9-134">hello following tables describe hello required and optional input fields for hello action and hello corresponding output details that are associated with using hello action.</span></span>

### <a name="filter-array"></a><span data-ttu-id="b30b9-135">Szűrő tömb</span><span class="sxs-lookup"><span data-stu-id="b30b9-135">Filter array</span></span>
<span data-ttu-id="b30b9-136">Az alábbiakban hello hello művelet, amely a kimenő HTTP-kérelem a beviteli mezők.</span><span class="sxs-lookup"><span data-stu-id="b30b9-136">hello following are input fields for hello action, which makes an HTTP outbound request.</span></span>
<span data-ttu-id="b30b9-137">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="b30b9-137">A * means that it is a required field.</span></span>

| <span data-ttu-id="b30b9-138">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="b30b9-138">Display name</span></span> | <span data-ttu-id="b30b9-139">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="b30b9-139">Property name</span></span> | <span data-ttu-id="b30b9-140">Leírás</span><span class="sxs-lookup"><span data-stu-id="b30b9-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b30b9-141">A *</span><span class="sxs-lookup"><span data-stu-id="b30b9-141">From*</span></span> |<span data-ttu-id="b30b9-142">A</span><span class="sxs-lookup"><span data-stu-id="b30b9-142">from</span></span> |<span data-ttu-id="b30b9-143">hello tömb toofilter</span><span class="sxs-lookup"><span data-stu-id="b30b9-143">hello array toofilter</span></span> |
| <span data-ttu-id="b30b9-144">Az állapot *</span><span class="sxs-lookup"><span data-stu-id="b30b9-144">Condition*</span></span> |<span data-ttu-id="b30b9-145">Ha</span><span class="sxs-lookup"><span data-stu-id="b30b9-145">where</span></span> |<span data-ttu-id="b30b9-146">hello feltétel tooevaluate minden elemhez</span><span class="sxs-lookup"><span data-stu-id="b30b9-146">hello condition tooevaluate for each item</span></span> |

<br>

### <a name="output-details"></a><span data-ttu-id="b30b9-147">Kimeneti részletei</span><span class="sxs-lookup"><span data-stu-id="b30b9-147">Output details</span></span>
<span data-ttu-id="b30b9-148">Az alábbiakban hello kimeneti részletes hello HTTP-válasz.</span><span class="sxs-lookup"><span data-stu-id="b30b9-148">hello following are output details for hello HTTP response.</span></span>

| <span data-ttu-id="b30b9-149">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="b30b9-149">Property name</span></span> | <span data-ttu-id="b30b9-150">Adattípus</span><span class="sxs-lookup"><span data-stu-id="b30b9-150">Data type</span></span> | <span data-ttu-id="b30b9-151">Leírás</span><span class="sxs-lookup"><span data-stu-id="b30b9-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b30b9-152">Szűrt tömb</span><span class="sxs-lookup"><span data-stu-id="b30b9-152">Filtered array</span></span> |<span data-ttu-id="b30b9-153">A tömb</span><span class="sxs-lookup"><span data-stu-id="b30b9-153">array</span></span> |<span data-ttu-id="b30b9-154">Olyan tömb, amely minden szűrt eredmény objektumot tartalmaz</span><span class="sxs-lookup"><span data-stu-id="b30b9-154">An array that contains an object for each filtered result</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b30b9-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b30b9-155">Next steps</span></span>
<span data-ttu-id="b30b9-156">Most, próbálja ki hello platform és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="b30b9-156">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="b30b9-157">Ismerje meg a hello más rendelkezésre álló összekötők logic Apps alkalmazások felmérésével a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="b30b9-157">You can explore hello other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

