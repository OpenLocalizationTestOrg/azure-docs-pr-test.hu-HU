---
title: "aaaLearn hogyan toouse hello Twitter-összekötőt a logic apps |} Microsoft Docs"
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
ms.openlocfilehash: ead4e4dc95bf894fd72b908c5375b407ba27642d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-twitter-connector"></a><span data-ttu-id="c7576-103">Hello Twitter-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="c7576-103">Get started with hello Twitter connector</span></span>
<span data-ttu-id="c7576-104">Hello Twitter-összekötő segítségével:</span><span class="sxs-lookup"><span data-stu-id="c7576-104">With hello Twitter connector you can:</span></span>

* <span data-ttu-id="c7576-105">Tweeteket és get Twitter-üzenetek</span><span class="sxs-lookup"><span data-stu-id="c7576-105">Post tweets and get tweets</span></span>
* <span data-ttu-id="c7576-106">Hozzáférés ütemtervek, ismerősei és followers</span><span class="sxs-lookup"><span data-stu-id="c7576-106">Access timelines, friends and followers</span></span>
* <span data-ttu-id="c7576-107">Hajtsa végre hello más műveletek és az alább ismertetett eseményindítók</span><span class="sxs-lookup"><span data-stu-id="c7576-107">Perform any of hello other actions and triggers described below</span></span>  

<span data-ttu-id="c7576-108">toouse [a csatlakozókat](apis-list.md), először toocreate logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c7576-108">toouse [any connector](apis-list.md), you first need toocreate a logic app.</span></span> <span data-ttu-id="c7576-109">Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c7576-109">You can get started by [creating a logic app now](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>  

## <a name="connect-tootwitter"></a><span data-ttu-id="c7576-110">Csatlakozás tooTwitter</span><span class="sxs-lookup"><span data-stu-id="c7576-110">Connect tooTwitter</span></span>
<span data-ttu-id="c7576-111">A Logic Apps alkalmazást férhetnek hozzá a szolgáltatáshoz, először jóvá kell toocreate egy *kapcsolat* toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c7576-111">Before your logic app can access any service, you first need toocreate a *connection* toohello service.</span></span> <span data-ttu-id="c7576-112">A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="c7576-112">A [connection](connectors-overview.md) provides connectivity between a logic app and another service.</span></span>  

### <a name="create-a-connection-tootwitter"></a><span data-ttu-id="c7576-113">Egy kapcsolat tooTwitter létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7576-113">Create a connection tooTwitter</span></span>
> [!INCLUDE [Steps toocreate a connection tooTwitter](../../includes/connectors-create-api-twitter.md)]
> 
> 

## <a name="use-a-twitter-trigger"></a><span data-ttu-id="c7576-114">Twitter-eseményindítók</span><span class="sxs-lookup"><span data-stu-id="c7576-114">Use a Twitter trigger</span></span>
<span data-ttu-id="c7576-115">Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény.</span><span class="sxs-lookup"><span data-stu-id="c7576-115">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="c7576-116">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="c7576-116">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

<span data-ttu-id="c7576-117">Ebben a példában I bemutatja, hogyan toouse hello **amikor egy új tweetet visszaküldi** toosearch #Seattle az indítás, és ha #Seattle megtalálható, frissítse a Dropbox fájlt hello tweetet hello szöveg.</span><span class="sxs-lookup"><span data-stu-id="c7576-117">In this example, I will show you how toouse hello **When a new tweet is posted**  trigger toosearch for #Seattle and, if #Seattle is found, update a file in Dropbox with hello text from hello tweet.</span></span> <span data-ttu-id="c7576-118">Vállalati például sikerült keresse meg a vállalat nevét hello és SQL-adatbázis hello tweetet hello szöveg frissíteni.</span><span class="sxs-lookup"><span data-stu-id="c7576-118">In an enterprise example, you could search for hello name of your company and update a SQL database with hello text from hello tweet.</span></span>

1. <span data-ttu-id="c7576-119">Adja meg *twitter* hello keresőmezőbe hello logic apps designer válassza hello **Twitter - amikor egy új tweetet visszaküldi** eseményindító</span><span class="sxs-lookup"><span data-stu-id="c7576-119">Enter *twitter* in hello search box on hello logic apps designer then select hello **Twitter - When a new tweet is posted**  trigger</span></span>   
   <span data-ttu-id="c7576-120">![Twitter eseményindító kép 1](./media/connectors-create-api-twitter/trigger-1.png)</span><span class="sxs-lookup"><span data-stu-id="c7576-120">![Twitter trigger image 1](./media/connectors-create-api-twitter/trigger-1.png)</span></span>  
2. <span data-ttu-id="c7576-121">Adja meg *#Seattle* a hello **keresett szöveg** vezérlő</span><span class="sxs-lookup"><span data-stu-id="c7576-121">Enter *#Seattle* in hello **Search Text** control</span></span>  
   <span data-ttu-id="c7576-122">![Twitter eseményindító kép 2](./media/connectors-create-api-twitter/trigger-2.png)</span><span class="sxs-lookup"><span data-stu-id="c7576-122">![Twitter trigger image 2](./media/connectors-create-api-twitter/trigger-2.png)</span></span> 

<span data-ttu-id="c7576-123">Ezen a ponton a Logic Apps alkalmazást az eseményindító, megkezdődik a hello futását más eseményindítók és műveletek hello munkafolyamat konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="c7576-123">At this point, your logic app has been configured with a trigger that will begin a run of hello other triggers and actions in hello workflow.</span></span> 

> [!NOTE]
> <span data-ttu-id="c7576-124">A működési logic app toobe legalább egy eseményindító és egy műveletet kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="c7576-124">For a logic app toobe functional, it must contain at least one trigger and one action.</span></span> <span data-ttu-id="c7576-125">Kövesse hello hello következő szakasz tooadd műveletet.</span><span class="sxs-lookup"><span data-stu-id="c7576-125">Follow hello steps in hello next section tooadd an action.</span></span>  
> 
> 

## <a name="add-a-condition"></a><span data-ttu-id="c7576-126">Feltétel hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c7576-126">Add a condition</span></span>
<span data-ttu-id="c7576-127">Mivel jelenleg csak a felhasználók több mint 50 felhasználóival Twitter-üzenetek iránt érdeklődik, olyan feltétel, amely megerősíti, hogy followers hello száma hozzáadása toohello logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c7576-127">Since we are only interested in tweets from users with more than 50 users, a condition that confirms hello number of followers must first be added toohello logic app.</span></span>  

1. <span data-ttu-id="c7576-128">Válassza ki **+ új lépés** tooadd hello művelet szeretné tootake Ha #Seattle tartalmaz egy új tweetet</span><span class="sxs-lookup"><span data-stu-id="c7576-128">Select **+ New step** tooadd hello action you would like tootake when #Seattle is found in a new tweet</span></span>  
   <span data-ttu-id="c7576-129">![Twitter művelet kép 1](../../includes/media/connectors-create-api-twitter/action-1.png)</span><span class="sxs-lookup"><span data-stu-id="c7576-129">![Twitter action image 1](../../includes/media/connectors-create-api-twitter/action-1.png)</span></span>  
2. <span data-ttu-id="c7576-130">Jelölje be hello **feltétel hozzáadása** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="c7576-130">Select hello **Add a condition** link.</span></span>  
   <span data-ttu-id="c7576-131">![Twitter feltétel kép 1](../../includes/media/connectors-create-api-twitter/condition-1.png) </span><span class="sxs-lookup"><span data-stu-id="c7576-131">![Twitter condition image 1](../../includes/media/connectors-create-api-twitter/condition-1.png) </span></span>  
   <span data-ttu-id="c7576-132">Ekkor megnyílik a hello **feltétel** vezérlő, ahol ellenőrizheti a feltételek például *egyenlő*, *van kisebb, mint*, *nagyobb, mint*, *tartalmaz*stb.</span><span class="sxs-lookup"><span data-stu-id="c7576-132">This opens hello **Condition** control where you can check conditions such as *is equal to*, *is less than*, *is greater than*, *contains*, etc.</span></span>  
   <span data-ttu-id="c7576-133">![Twitter feltétel kép 2](../../includes/media/connectors-create-api-twitter/condition-2.png)</span><span class="sxs-lookup"><span data-stu-id="c7576-133">![Twitter condition image 2](../../includes/media/connectors-create-api-twitter/condition-2.png)</span></span>   
3. <span data-ttu-id="c7576-134">Jelölje be hello **adjon meg értéket** vezérlő.</span><span class="sxs-lookup"><span data-stu-id="c7576-134">Select hello **Choose a value** control.</span></span>  
   <span data-ttu-id="c7576-135">A vezérlő választhat egy vagy több hello tulajdonságok bármely korábbi műveletek vagy eseményindítók hello akiknek állapota lesz kiértékelt tootrue vagy HAMIS értékként.</span><span class="sxs-lookup"><span data-stu-id="c7576-135">In this control, you can select one or more of hello properties from any previous actions or triggers as hello value whose condition will be evaluated tootrue or false.</span></span>
   <span data-ttu-id="c7576-136">![Twitter feltétel kép 3](../../includes/media/connectors-create-api-twitter/condition-3.png)</span><span class="sxs-lookup"><span data-stu-id="c7576-136">![Twitter condition image 3](../../includes/media/connectors-create-api-twitter/condition-3.png)</span></span>   
4. <span data-ttu-id="c7576-137">Jelölje be hello **...**  tooexpand hello tulajdonságlista hívjuk fel minden hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="c7576-137">Select hello **...** tooexpand hello list of properties so you can see all hello properties that are available.</span></span>        
   <span data-ttu-id="c7576-138">![Twitter feltétel kép 4](../../includes/media/connectors-create-api-twitter/condition-4.png)</span><span class="sxs-lookup"><span data-stu-id="c7576-138">![Twitter condition image 4](../../includes/media/connectors-create-api-twitter/condition-4.png)</span></span>   
5. <span data-ttu-id="c7576-139">Jelölje be hello **Followers száma** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c7576-139">Select hello **Followers count** property.</span></span>    
   <span data-ttu-id="c7576-140">![Az állapot kép 5 Twitter](../../includes/media/connectors-create-api-twitter/condition-5.png)</span><span class="sxs-lookup"><span data-stu-id="c7576-140">![Twitter condition image 5](../../includes/media/connectors-create-api-twitter/condition-5.png)</span></span>   
6. <span data-ttu-id="c7576-141">Figyelje meg hello Followers count tulajdonság már hello érték vezérlőben.</span><span class="sxs-lookup"><span data-stu-id="c7576-141">Notice hello Followers count property is now in hello value control.</span></span>    
   ![Twitter feltétel kép 6](../../includes/media/connectors-create-api-twitter/condition-6.png)   
7. <span data-ttu-id="c7576-143">Válassza ki **nagyobb, mint** hello operátorok listából.</span><span class="sxs-lookup"><span data-stu-id="c7576-143">Select **is greater than** from hello operators list.</span></span>    
   <span data-ttu-id="c7576-144">![Twitter feltétel kép 7](../../includes/media/connectors-create-api-twitter/condition-7.png)</span><span class="sxs-lookup"><span data-stu-id="c7576-144">![Twitter condition image 7](../../includes/media/connectors-create-api-twitter/condition-7.png)</span></span>   
8. <span data-ttu-id="c7576-145">Itt adhatja meg 50 hello operandus hello *nagyobb, mint* operátor.</span><span class="sxs-lookup"><span data-stu-id="c7576-145">Enter 50 as hello operand for hello *is greater than* operator.</span></span>  
   <span data-ttu-id="c7576-146">hello állapot jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c7576-146">hello condition is now added.</span></span> <span data-ttu-id="c7576-147">Mentse a munkáját, hello segítségével **mentése** hivatkozásra a fenti hello menüben.</span><span class="sxs-lookup"><span data-stu-id="c7576-147">Save your work using hello **Save** link on hello menu above.</span></span>    
   <span data-ttu-id="c7576-148">![Twitter feltétel kép 8](../../includes/media/connectors-create-api-twitter/condition-8.png)</span><span class="sxs-lookup"><span data-stu-id="c7576-148">![Twitter condition image 8](../../includes/media/connectors-create-api-twitter/condition-8.png)</span></span>   

## <a name="use-a-twitter-action"></a><span data-ttu-id="c7576-149">Egy Twitter művelettel</span><span class="sxs-lookup"><span data-stu-id="c7576-149">Use a Twitter action</span></span>
<span data-ttu-id="c7576-150">Egy művelet által definiált logikai alkalmazás hello munkafolyamat során.</span><span class="sxs-lookup"><span data-stu-id="c7576-150">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="c7576-151">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="c7576-151">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>  

<span data-ttu-id="c7576-152">Most, hogy hozzáadta egy eseményindítót, kövesse ezeket a lépéseket tooadd egy műveletet, amely egy új tweetet hello Twitter-üzeneteket hello eseményindító által megtalált hello tartalmát fogja elküldeni.</span><span class="sxs-lookup"><span data-stu-id="c7576-152">Now that you have added a trigger, follow these steps tooadd an action that will post a new tweet with hello contents of hello tweets found by hello trigger.</span></span> <span data-ttu-id="c7576-153">Ez az útmutató hello alkalmazásában csak Twitter-üzeneteket a felhasználók a több mint 50 followers lesznek közzétéve.</span><span class="sxs-lookup"><span data-stu-id="c7576-153">For hello purposes of this walk-through only tweets from users with more than 50 followers will be posted.</span></span>  

<span data-ttu-id="c7576-154">Hello a következő lépésben adhat egy Twitter-műveletet, amely egy tweetet, az egyes tweetet, amelyek több mint 50 followers rendelkező felhasználó megtalálhatók hello tulajdonságok használatával fogja elküldeni.</span><span class="sxs-lookup"><span data-stu-id="c7576-154">In hello next step, you will add a Twitter action that will post a tweet using some of hello properties of each tweet that has been posted by a user who has more than 50 followers.</span></span>  

1. <span data-ttu-id="c7576-155">Válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c7576-155">Select **Add an action**.</span></span> <span data-ttu-id="c7576-156">Ekkor megnyílik a kereshetnek, ahol a más műveletek és eseményindítók hello keresési vezérlőelemet.</span><span class="sxs-lookup"><span data-stu-id="c7576-156">This opens hello search control where you can search for other actions and triggers.</span></span>  
   <span data-ttu-id="c7576-157">![Twitter feltétel kép 9](../../includes/media/connectors-create-api-twitter/condition-9.png)</span><span class="sxs-lookup"><span data-stu-id="c7576-157">![Twitter condition image 9](../../includes/media/connectors-create-api-twitter/condition-9.png)</span></span>   
2. <span data-ttu-id="c7576-158">Adja meg *twitter* hello keresőmezőbe válassza hello **Twitter - egy tweetet utáni** művelet.</span><span class="sxs-lookup"><span data-stu-id="c7576-158">Enter *twitter* into hello search box then select hello **Twitter - Post a tweet** action.</span></span> <span data-ttu-id="c7576-159">Ekkor megnyílik a hello **egy tweetet utáni** szabályozása, ahol lép az összes részletes az elküldött hello tweetet.</span><span class="sxs-lookup"><span data-stu-id="c7576-159">This opens hello **Post a tweet** control where you will enter all details for hello tweet being posted.</span></span>      
   <span data-ttu-id="c7576-160">![A művelet kép 1-5 Twitter](../../includes/media/connectors-create-api-twitter/action-1-5.png)</span><span class="sxs-lookup"><span data-stu-id="c7576-160">![Twitter action image 1-5](../../includes/media/connectors-create-api-twitter/action-1-5.png)</span></span>   
3. <span data-ttu-id="c7576-161">Jelölje be hello **Tweetet szöveg** vezérlő.</span><span class="sxs-lookup"><span data-stu-id="c7576-161">Select hello **Tweet text** control.</span></span> <span data-ttu-id="c7576-162">A korábbi műveletek és a hello logic app eseményindítók összes kimenetének is látható.</span><span class="sxs-lookup"><span data-stu-id="c7576-162">All outputs from previous actions and triggers in hello logic app are now visible.</span></span> <span data-ttu-id="c7576-163">Válasszon ezek egyikét sem, és használatuk hello tweetet szövege hello új tweetet részeként.</span><span class="sxs-lookup"><span data-stu-id="c7576-163">You can select any of these and use them as part of hello tweet text of hello new tweet.</span></span>     
   <span data-ttu-id="c7576-164">![Twitter művelet kép 2](../../includes/media/connectors-create-api-twitter/action-2.png)</span><span class="sxs-lookup"><span data-stu-id="c7576-164">![Twitter action image 2](../../includes/media/connectors-create-api-twitter/action-2.png)</span></span>   
4. <span data-ttu-id="c7576-165">Válassza ki **felhasználónév**</span><span class="sxs-lookup"><span data-stu-id="c7576-165">Select **User name**</span></span>   
5. <span data-ttu-id="c7576-166">Adja meg *szereplő ellenőrzőösszeg:* hello tweetet szöveg vezérlőben.</span><span class="sxs-lookup"><span data-stu-id="c7576-166">Enter *says:* in hello tweet text control.</span></span> <span data-ttu-id="c7576-167">Ehhez a felhasználói név után.</span><span class="sxs-lookup"><span data-stu-id="c7576-167">Do this just after User name.</span></span>  
6. <span data-ttu-id="c7576-168">Válassza ki *Tweetet szöveg*.</span><span class="sxs-lookup"><span data-stu-id="c7576-168">Select *Tweet text*.</span></span>       
   <span data-ttu-id="c7576-169">![Twitter művelet kép 3](../../includes/media/connectors-create-api-twitter/action-3.png)</span><span class="sxs-lookup"><span data-stu-id="c7576-169">![Twitter action image 3](../../includes/media/connectors-create-api-twitter/action-3.png)</span></span>   
7. <span data-ttu-id="c7576-170">Mentse a munkáját, és küldjön egy tweetet hello #Seattle hashtaggel történő tooactivate rendelkező, a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="c7576-170">Save your work and send a tweet with hello #Seattle hashtag tooactivate your workflow.</span></span>  


## <a name="connector-specific-details"></a><span data-ttu-id="c7576-171">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="c7576-171">Connector-specific details</span></span>

<span data-ttu-id="c7576-172">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/twitterconnector/).</span><span class="sxs-lookup"><span data-stu-id="c7576-172">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/twitterconnector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="c7576-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c7576-173">Next steps</span></span>
[<span data-ttu-id="c7576-174">Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7576-174">Create a logic app</span></span>](../logic-apps/logic-apps-create-a-logic-app.md)

