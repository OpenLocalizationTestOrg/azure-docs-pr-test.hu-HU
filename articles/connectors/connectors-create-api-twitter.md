---
title: "A Twitter-összekötő használata a logic apps |} Microsoft Docs"
description: "A REST API paraméterekkel Twitter-összekötő áttekintése"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8bce2183-544d-4668-a2dc-9a62c152d9fa
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: be8163043535833ce45b3d50939a537406cf8152
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-twitter-connector"></a><span data-ttu-id="fb88f-103">A Twitter-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="fb88f-103">Get started with the Twitter connector</span></span>
<span data-ttu-id="fb88f-104">A Twitter-összekötő segítségével:</span><span class="sxs-lookup"><span data-stu-id="fb88f-104">With the Twitter connector you can:</span></span>

* <span data-ttu-id="fb88f-105">Tweeteket és get Twitter-üzenetek</span><span class="sxs-lookup"><span data-stu-id="fb88f-105">Post tweets and get tweets</span></span>
* <span data-ttu-id="fb88f-106">Hozzáférés ütemtervek, ismerősei és followers</span><span class="sxs-lookup"><span data-stu-id="fb88f-106">Access timelines, friends and followers</span></span>
* <span data-ttu-id="fb88f-107">Hajtsa végre az egyéb műveletek és az alábbiakban eseményindítók</span><span class="sxs-lookup"><span data-stu-id="fb88f-107">Perform any of the other actions and triggers described below</span></span>  

<span data-ttu-id="fb88f-108">Használandó [a csatlakozókat](apis-list.md), először hozzon létre egy logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fb88f-108">To use [any connector](apis-list.md), you first need to create a logic app.</span></span> <span data-ttu-id="fb88f-109">Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="fb88f-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>  

## <a name="connect-to-twitter"></a><span data-ttu-id="fb88f-110">Kapcsolódás a Twitteren</span><span class="sxs-lookup"><span data-stu-id="fb88f-110">Connect to Twitter</span></span>
<span data-ttu-id="fb88f-111">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először hozzon létre egy *kapcsolat* a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="fb88f-111">Before your logic app can access any service, you first need to create a *connection* to the service.</span></span> <span data-ttu-id="fb88f-112">A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="fb88f-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-to-twitter"></a><span data-ttu-id="fb88f-113">Kapcsolatot létesíthet a Twitteren</span><span class="sxs-lookup"><span data-stu-id="fb88f-113">Create a connection to Twitter</span></span>
> [!INCLUDE [Steps to create a connection to Twitter](../../includes/connectors-create-api-twitter.md)]
> 
> 

## <a name="use-a-twitter-trigger"></a><span data-ttu-id="fb88f-114">Twitter-eseményindítók</span><span class="sxs-lookup"><span data-stu-id="fb88f-114">Use a Twitter trigger</span></span>
<span data-ttu-id="fb88f-115">Egy eseményindító nem egy eseményt, a logikai alkalmazás definiált munkafolyamat indításához használható.</span><span class="sxs-lookup"><span data-stu-id="fb88f-115">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="fb88f-116">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="fb88f-116">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="fb88f-117">Ebben a példában I bemutatja, hogyan használható a **amikor egy új tweetet visszaküldi** eseményindító #Seattle keresését, és ha #Seattle található, frissítse a Dropbox fájlt a tweetet szöveget.</span><span class="sxs-lookup"><span data-stu-id="fb88f-117">In this example, I will show you how to use the **When a new tweet is posted**  trigger to search for #Seattle and, if #Seattle is found, update a file in Dropbox with the text from the tweet.</span></span> <span data-ttu-id="fb88f-118">Vállalati például sikerült keresse meg a vállalat nevét és a tweetet szöveget az SQL-adatbázis frissítése.</span><span class="sxs-lookup"><span data-stu-id="fb88f-118">In an enterprise example, you could search for the name of your company and update a SQL database with the text from the tweet.</span></span>

1. <span data-ttu-id="fb88f-119">Adja meg *twitter* be a keresőmezőbe a logic apps designer válassza ki a **Twitter - amikor egy új tweetet visszaküldi** eseményindító</span><span class="sxs-lookup"><span data-stu-id="fb88f-119">Enter *twitter* in the search box on the logic apps designer then select the **Twitter - When a new tweet is posted**  trigger</span></span>   
   <span data-ttu-id="fb88f-120">![Twitter eseményindító kép 1](./media/connectors-create-api-twitter/trigger-1.png)</span><span class="sxs-lookup"><span data-stu-id="fb88f-120">![Twitter trigger image 1](./media/connectors-create-api-twitter/trigger-1.png)</span></span>  
2. <span data-ttu-id="fb88f-121">Adja meg *#Seattle* a a **keresett szöveg** vezérlő</span><span class="sxs-lookup"><span data-stu-id="fb88f-121">Enter *#Seattle* in the **Search Text** control</span></span>  
   <span data-ttu-id="fb88f-122">![Twitter eseményindító kép 2](./media/connectors-create-api-twitter/trigger-2.png)</span><span class="sxs-lookup"><span data-stu-id="fb88f-122">![Twitter trigger image 2](./media/connectors-create-api-twitter/trigger-2.png)</span></span> 

<span data-ttu-id="fb88f-123">A Logic Apps alkalmazást ezen a ponton úgy van konfigurálva, az eseményindító, amely akkor kezdődik, eseményindítók és műveletek a munkafolyamat futását.</span><span class="sxs-lookup"><span data-stu-id="fb88f-123">At this point, your logic app has been configured with a trigger that will begin a run of the other triggers and actions in the workflow.</span></span> 

> [!NOTE]
> <span data-ttu-id="fb88f-124">A logikai alkalmazás működéséhez legalább egy eseményindító és egy műveletet kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="fb88f-124">For a logic app to be functional, it must contain at least one trigger and one action.</span></span> <span data-ttu-id="fb88f-125">Kövesse a következő szakaszban művelet hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="fb88f-125">Follow the steps in the next section to add an action.</span></span>  
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="fb88f-126">Feltétel hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fb88f-126">Add a condition</span></span>
<span data-ttu-id="fb88f-127">Mivel jelenleg csak a felhasználók több mint 50 felhasználóival Twitter-üzenetek iránt érdeklődik, olyan feltétel, amely megerősíti, hogy hány followers először szerepelnie kell a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fb88f-127">Since we are only interested in tweets from users with more than 50 users, a condition that confirms the number of followers must first be added to the logic app.</span></span>  

1. <span data-ttu-id="fb88f-128">Válassza ki **+ új lépés** a műveletet hajtson végre egy új tweetet található #Seattle hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fb88f-128">Select **+ New step** to add the action you would like to take when #Seattle is found in a new tweet</span></span>  
   <span data-ttu-id="fb88f-129">![Twitter művelet kép 1](../../includes/media/connectors-create-api-twitter/action-1.png)</span><span class="sxs-lookup"><span data-stu-id="fb88f-129">![Twitter action image 1](../../includes/media/connectors-create-api-twitter/action-1.png)</span></span>  
2. <span data-ttu-id="fb88f-130">Válassza ki a **feltétel hozzáadása** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="fb88f-130">Select the **Add a condition** link.</span></span>  
   <span data-ttu-id="fb88f-131">![Twitter feltétel kép 1](../../includes/media/connectors-create-api-twitter/condition-1.png) </span><span class="sxs-lookup"><span data-stu-id="fb88f-131">![Twitter condition image 1](../../includes/media/connectors-create-api-twitter/condition-1.png) </span></span>  
   <span data-ttu-id="fb88f-132">Ekkor megnyílik a **feltétel** vezérlő, ahol ellenőrizheti a feltételeket, mint *egyenlő*, *értéke kisebb, mint*, *nagyobb, mint*, *tartalmaz*stb.</span><span class="sxs-lookup"><span data-stu-id="fb88f-132">This opens the **Condition** control where you can check conditions such as *is equal to*, *is less than*, *is greater than*, *contains*, etc.</span></span>  
   <span data-ttu-id="fb88f-133">![Twitter feltétel kép 2](../../includes/media/connectors-create-api-twitter/condition-2.png)</span><span class="sxs-lookup"><span data-stu-id="fb88f-133">![Twitter condition image 2](../../includes/media/connectors-create-api-twitter/condition-2.png)</span></span>   
3. <span data-ttu-id="fb88f-134">Válassza ki a **adjon meg értéket** vezérlő.</span><span class="sxs-lookup"><span data-stu-id="fb88f-134">Select the **Choose a value** control.</span></span>  
   <span data-ttu-id="fb88f-135">A vezérlő választhat egy vagy több tulajdonságaihoz bármely korábbi műveletek vagy eseményindítók akiknek állapota kiértékelési értéke IGAZ vagy hamis.</span><span class="sxs-lookup"><span data-stu-id="fb88f-135">In this control, you can select one or more of the properties from any previous actions or triggers as the value whose condition will be evaluated to true or false.</span></span>
   <span data-ttu-id="fb88f-136">![Twitter feltétel kép 3](../../includes/media/connectors-create-api-twitter/condition-3.png)</span><span class="sxs-lookup"><span data-stu-id="fb88f-136">![Twitter condition image 3](../../includes/media/connectors-create-api-twitter/condition-3.png)</span></span>   
4. <span data-ttu-id="fb88f-137">Válassza ki a **...**  bontsa ki a tulajdonságok listájának, így minden elérhető tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="fb88f-137">Select the **...** to expand the list of properties so you can see all the properties that are available.</span></span>        
   <span data-ttu-id="fb88f-138">![Twitter feltétel kép 4](../../includes/media/connectors-create-api-twitter/condition-4.png)</span><span class="sxs-lookup"><span data-stu-id="fb88f-138">![Twitter condition image 4](../../includes/media/connectors-create-api-twitter/condition-4.png)</span></span>   
5. <span data-ttu-id="fb88f-139">Válassza ki a **Followers száma** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="fb88f-139">Select the **Followers count** property.</span></span>    
   <span data-ttu-id="fb88f-140">![Az állapot kép 5 Twitter](../../includes/media/connectors-create-api-twitter/condition-5.png)</span><span class="sxs-lookup"><span data-stu-id="fb88f-140">![Twitter condition image 5](../../includes/media/connectors-create-api-twitter/condition-5.png)</span></span>   
6. <span data-ttu-id="fb88f-141">Figyelje meg, a Followers a count tulajdonság már a érték vezérlőben.</span><span class="sxs-lookup"><span data-stu-id="fb88f-141">Notice the Followers count property is now in the value control.</span></span>    
   ![Twitter feltétel kép 6](../../includes/media/connectors-create-api-twitter/condition-6.png)   
7. <span data-ttu-id="fb88f-143">Válassza ki **nagyobb, mint** operátorok listájából.</span><span class="sxs-lookup"><span data-stu-id="fb88f-143">Select **is greater than** from the operators list.</span></span>    
   <span data-ttu-id="fb88f-144">![Twitter feltétel kép 7](../../includes/media/connectors-create-api-twitter/condition-7.png)</span><span class="sxs-lookup"><span data-stu-id="fb88f-144">![Twitter condition image 7](../../includes/media/connectors-create-api-twitter/condition-7.png)</span></span>   
8. <span data-ttu-id="fb88f-145">Adjon meg 50 az operandus a *nagyobb, mint* operátor.</span><span class="sxs-lookup"><span data-stu-id="fb88f-145">Enter 50 as the operand for the *is greater than* operator.</span></span>  
   <span data-ttu-id="fb88f-146">A feltétel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="fb88f-146">The condition is now added.</span></span> <span data-ttu-id="fb88f-147">Mentés a munkahelyi használatával a **mentése** hivatkozásra a fenti menüben.</span><span class="sxs-lookup"><span data-stu-id="fb88f-147">Save your work using the **Save** link on the menu above.</span></span>    
   <span data-ttu-id="fb88f-148">![Twitter feltétel kép 8](../../includes/media/connectors-create-api-twitter/condition-8.png)</span><span class="sxs-lookup"><span data-stu-id="fb88f-148">![Twitter condition image 8](../../includes/media/connectors-create-api-twitter/condition-8.png)</span></span>   

## <a name="use-a-twitter-action"></a><span data-ttu-id="fb88f-149">Egy Twitter művelettel</span><span class="sxs-lookup"><span data-stu-id="fb88f-149">Use a Twitter action</span></span>
<span data-ttu-id="fb88f-150">Egy művelet során a logikai alkalmazás definiált munkafolyamat által végzett.</span><span class="sxs-lookup"><span data-stu-id="fb88f-150">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="fb88f-151">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="fb88f-151">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="fb88f-152">Most, hogy hozzáadta egy eseményindítót, kövesse az alábbi lépéseket, a Twitter-üzeneteket az eseményindító által megtalált tartalmát egy új tweetet fogja elküldeni művelet hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="fb88f-152">Now that you have added a trigger, follow these steps to add an action that will post a new tweet with the contents of the tweets found by the trigger.</span></span> <span data-ttu-id="fb88f-153">Ez az útmutató alkalmazásában csak Twitter-üzeneteket a felhasználók a több mint 50 followers lesznek közzétéve.</span><span class="sxs-lookup"><span data-stu-id="fb88f-153">For the purposes of this walk-through only tweets from users with more than 50 followers will be posted.</span></span>  

<span data-ttu-id="fb88f-154">A következő lépésben adhat egy Twitter-műveletet, amely egy tweetet, néhány, az egyes tweetet, amelyek több mint 50 followers rendelkező felhasználó megtalálhatók a tulajdonságok használatával fogja elküldeni.</span><span class="sxs-lookup"><span data-stu-id="fb88f-154">In the next step, you will add a Twitter action that will post a tweet using some of the properties of each tweet that has been posted by a user who has more than 50 followers.</span></span>  

1. <span data-ttu-id="fb88f-155">Válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="fb88f-155">Select **Add an action**.</span></span> <span data-ttu-id="fb88f-156">Ekkor megnyílik a keresési vezérlőelemet kereshetnek, ahol a más műveletek és eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="fb88f-156">This opens the search control where you can search for other actions and triggers.</span></span>  
   <span data-ttu-id="fb88f-157">![Twitter feltétel kép 9](../../includes/media/connectors-create-api-twitter/condition-9.png)</span><span class="sxs-lookup"><span data-stu-id="fb88f-157">![Twitter condition image 9](../../includes/media/connectors-create-api-twitter/condition-9.png)</span></span>   
2. <span data-ttu-id="fb88f-158">Adja meg *twitter* a keresőmezőbe válassza ki a **Twitter - egy tweetet utáni** művelet.</span><span class="sxs-lookup"><span data-stu-id="fb88f-158">Enter *twitter* into the search box then select the **Twitter - Post a tweet** action.</span></span> <span data-ttu-id="fb88f-159">Ekkor megnyílik a **egy tweetet utáni** szabályozása, ahol lép az összes részletes az elküldött tweetet a.</span><span class="sxs-lookup"><span data-stu-id="fb88f-159">This opens the **Post a tweet** control where you will enter all details for the tweet being posted.</span></span>      
   <span data-ttu-id="fb88f-160">![A művelet kép 1-5 Twitter](../../includes/media/connectors-create-api-twitter/action-1-5.png)</span><span class="sxs-lookup"><span data-stu-id="fb88f-160">![Twitter action image 1-5](../../includes/media/connectors-create-api-twitter/action-1-5.png)</span></span>   
3. <span data-ttu-id="fb88f-161">Válassza ki a **Tweetet szöveg** vezérlő.</span><span class="sxs-lookup"><span data-stu-id="fb88f-161">Select the **Tweet text** control.</span></span> <span data-ttu-id="fb88f-162">A korábbi műveletek és a logikai alkalmazást az eseményindítók összes kimenetének is látható.</span><span class="sxs-lookup"><span data-stu-id="fb88f-162">All outputs from previous actions and triggers in the logic app are now visible.</span></span> <span data-ttu-id="fb88f-163">Válassza ki, ezek egyikét sem, és használhatja őket az új tweetet tweetet szövegét részeként.</span><span class="sxs-lookup"><span data-stu-id="fb88f-163">You can select any of these and use them as part of the tweet text of the new tweet.</span></span>     
   <span data-ttu-id="fb88f-164">![Twitter művelet kép 2](../../includes/media/connectors-create-api-twitter/action-2.png)</span><span class="sxs-lookup"><span data-stu-id="fb88f-164">![Twitter action image 2](../../includes/media/connectors-create-api-twitter/action-2.png)</span></span>   
4. <span data-ttu-id="fb88f-165">Válassza ki **felhasználónév**</span><span class="sxs-lookup"><span data-stu-id="fb88f-165">Select **User name**</span></span>   
5. <span data-ttu-id="fb88f-166">Adja meg *szereplő ellenőrzőösszeg:* a tweetet szöveg vezérlőben.</span><span class="sxs-lookup"><span data-stu-id="fb88f-166">Enter *says:* in the tweet text control.</span></span> <span data-ttu-id="fb88f-167">Ehhez a felhasználói név után.</span><span class="sxs-lookup"><span data-stu-id="fb88f-167">Do this just after User name.</span></span>  
6. <span data-ttu-id="fb88f-168">Válassza ki *Tweetet szöveg*.</span><span class="sxs-lookup"><span data-stu-id="fb88f-168">Select *Tweet text*.</span></span>       
   <span data-ttu-id="fb88f-169">![Twitter művelet kép 3](../../includes/media/connectors-create-api-twitter/action-3.png)</span><span class="sxs-lookup"><span data-stu-id="fb88f-169">![Twitter action image 3](../../includes/media/connectors-create-api-twitter/action-3.png)</span></span>   
7. <span data-ttu-id="fb88f-170">Mentse a munkáját, és küldjön egy tweetet, a #Seattle hashtaggel történő aktiválásához a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="fb88f-170">Save your work and send a tweet with the #Seattle hashtag to activate your workflow.</span></span>  


## <a name="connector-specific-details"></a><span data-ttu-id="fb88f-171">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="fb88f-171">Connector-specific details</span></span>

<span data-ttu-id="fb88f-172">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/twitterconnector/).</span><span class="sxs-lookup"><span data-stu-id="fb88f-172">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/twitterconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="fb88f-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fb88f-173">Next steps</span></span>
[<span data-ttu-id="fb88f-174">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fb88f-174">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

