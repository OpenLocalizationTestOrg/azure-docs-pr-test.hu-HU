---
title: "Adja hozzá az Office 365 Outlook-összekötőt a Logic Apps |} Microsoft Docs"
description: "Hozzon létre a logic apps Office 365-összekötő és az Office 365 közötti interakció. Például: létrehozása, szerkesztése és a partnerek és a naptári elemek frissítése."
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2f6cc2c-bba2-493a-b0ba-841785462a80
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 5335dae62e61659b68e8befb4ed0d404dffb800c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-office-365-outlook-connector"></a><span data-ttu-id="0a480-104">Az Office 365 Outlook-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="0a480-104">Get started with the Office 365 Outlook connector</span></span>
<span data-ttu-id="0a480-105">Az Office 365 Outlook-összekötő lehetővé teszi, hogy az Office 365 Outlook való együttműködéshez.</span><span class="sxs-lookup"><span data-stu-id="0a480-105">The Office 365 Outlook connector enables interaction with Outlook in Office 365.</span></span> <span data-ttu-id="0a480-106">Ez az összekötő segítségével létrehozása, szerkesztése, és frissítse a névjegyeket és a naptári elemek, és is, küldése és e-mail válaszolni.</span><span class="sxs-lookup"><span data-stu-id="0a480-106">Use this connector to create, edit, and update contacts and calendar items, and also get, send, and reply to email.</span></span>

<span data-ttu-id="0a480-107">Az Office 365 Outlook meg:</span><span class="sxs-lookup"><span data-stu-id="0a480-107">With Office 365 Outlook, you:</span></span>

* <span data-ttu-id="0a480-108">Office 365 belül e-mailek és a naptári funkciókat használ, a munkafolyamat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0a480-108">Build your workflow using the email and calendar features within Office 365.</span></span> 
* <span data-ttu-id="0a480-109">Eseményindítók segítségével a munkafolyamat indítható el, amikor egy új e-mailt, a naptárelemek frissítésekor és még sok más.</span><span class="sxs-lookup"><span data-stu-id="0a480-109">Use triggers to start your workflow when there is a new email, when a calendar item is updated, and more.</span></span>
* <span data-ttu-id="0a480-110">Műveletek segítségével egy e-mailt küldeni, hozzon létre egy új naptár esemény, és több.</span><span class="sxs-lookup"><span data-stu-id="0a480-110">Use actions to send an email, create a new calendar event, and more.</span></span> <span data-ttu-id="0a480-111">Például ha van egy új objektumot a Salesforce (trigger), küldjön egy e-mailt a az Office 365 Outlook (művelet).</span><span class="sxs-lookup"><span data-stu-id="0a480-111">For example, when there is a new object in Salesforce (a trigger), send an email to your Office 365 Outlook (an action).</span></span> 

<span data-ttu-id="0a480-112">Ez a témakör bemutatja, hogyan használható az Office 365 Outlook összekötő logikai alkalmazás, és eseményindítók és műveletek is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0a480-112">This topic shows you how to use the Office 365 Outlook connector in a logic app, and also lists the triggers and actions.</span></span>

> [!NOTE]
> <span data-ttu-id="0a480-113">A cikk e verziója a Logic Apps általános elérhetőségével (GA) vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="0a480-113">This version of the article applies to Logic Apps general availability (GA).</span></span>
> 
> 

<span data-ttu-id="0a480-114">A Logic Apps kapcsolatos további információkért lásd: [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="0a480-114">To learn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-to-office-365"></a><span data-ttu-id="0a480-115">Csatlakozás az Office 365-höz</span><span class="sxs-lookup"><span data-stu-id="0a480-115">Connect to Office 365</span></span>
<span data-ttu-id="0a480-116">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először létre kell hoznia egy *kapcsolat* a szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="0a480-116">Before your logic app can access any service, you first create a *connection* to the service.</span></span> <span data-ttu-id="0a480-117">Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít.</span><span class="sxs-lookup"><span data-stu-id="0a480-117">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="0a480-118">Például Office 365 Outlook csatlakozhat, először az Office 365 *kapcsolat*.</span><span class="sxs-lookup"><span data-stu-id="0a480-118">For example, to connect to Office 365 Outlook, you first need an Office 365 *connection*.</span></span> <span data-ttu-id="0a480-119">VPN-kapcsolat létrehozásához adja meg a hitelesítő adatok általában segítségével éri el a szolgáltatást, amelyhez csatlakozni szeretne.</span><span class="sxs-lookup"><span data-stu-id="0a480-119">To create a connection, enter the credentials you normally use to access the service you wish to connect to.</span></span> <span data-ttu-id="0a480-120">Ezért az Office 365 Outlook, adja meg a hitelesítő adatokat az Office 365-fiókra, a VPN-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0a480-120">So with Office 365 Outlook, enter the credentials to your Office 365 account to create the connection.</span></span>

## <a name="create-the-connection"></a><span data-ttu-id="0a480-121">A kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a480-121">Create the connection</span></span>
> [!INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="0a480-122">Eseményindítók</span><span class="sxs-lookup"><span data-stu-id="0a480-122">Use a trigger</span></span>
<span data-ttu-id="0a480-123">Egy eseményindító nem egy eseményt, a logikai alkalmazás definiált munkafolyamat indításához használható.</span><span class="sxs-lookup"><span data-stu-id="0a480-123">A trigger is an event that can be used to start the workflow defined in a logic app.</span></span> <span data-ttu-id="0a480-124">Eseményindítók "lekérdezésére" a szolgáltatás egy intervallum és a kívánt gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="0a480-124">Triggers "poll" the service at an interval and frequency that you want.</span></span> <span data-ttu-id="0a480-125">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="0a480-125">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="0a480-126">A logikai alkalmazásban írjon be egy "office 365" listájának az eseményindítók:</span><span class="sxs-lookup"><span data-stu-id="0a480-126">In the logic app, type "office 365" to get a list of the triggers:</span></span>  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. <span data-ttu-id="0a480-127">Válassza ki **Office 365 Outlook - jövőbeli esemény hamarosan indításakor**.</span><span class="sxs-lookup"><span data-stu-id="0a480-127">Select **Office 365 Outlook - When an upcoming event is starting soon**.</span></span> <span data-ttu-id="0a480-128">Már létezik egy kapcsolat, a naptárban ezután válassza ki a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="0a480-128">If a connection already exists, then select a calendar from the drop-down list.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    <span data-ttu-id="0a480-129">Ha a bejelentkezéshez kéri, akkor írja be a bejelentkezési a kapcsolat létrehozásához szükséges adatok.</span><span class="sxs-lookup"><span data-stu-id="0a480-129">If you are prompted to sign in, then enter the sign in details to create the connection.</span></span> <span data-ttu-id="0a480-130">[A kapcsolat létrehozása](connectors-create-api-office365-outlook.md#create-the-connection) a témakör felsorolja azokat a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="0a480-130">[Create the connection](connectors-create-api-office365-outlook.md#create-the-connection) in this topic lists the steps.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="0a480-131">Ebben a példában a logikai alkalmazás fut, amikor a naptár esemény frissül.</span><span class="sxs-lookup"><span data-stu-id="0a480-131">In this example, the logic app runs when a calendar event is updated.</span></span> <span data-ttu-id="0a480-132">Ehhez az eseményindítóhoz eredményeit, vegye fel egy újabb műveletet, amely egy szöveges üzenetet küld Önnek.</span><span class="sxs-lookup"><span data-stu-id="0a480-132">To see the results of this trigger, add another action that sends you a text message.</span></span> <span data-ttu-id="0a480-133">Adja hozzá például a Twilio *üzenet küldése* művelet adott szövegek, a naptár esemény indításakor 15 percen belül.</span><span class="sxs-lookup"><span data-stu-id="0a480-133">For example, add the Twilio *Send message* action that texts you when the calendar event is starting in 15 minutes.</span></span> 
   > 
   > 
3. <span data-ttu-id="0a480-134">Válassza ki a **szerkesztése** gombra, majd a **gyakoriság** és **időköz** értékeket.</span><span class="sxs-lookup"><span data-stu-id="0a480-134">Select the **Edit** button and set the **Frequency** and **Interval** values.</span></span> <span data-ttu-id="0a480-135">Például, ha azt szeretné, hogy az eseményindító, és kérdezze le a 15 percenként, majd állítsa be a **gyakoriság** való **perc**, és állítsa be a **időköz** való **15**.</span><span class="sxs-lookup"><span data-stu-id="0a480-135">For example, if you want the trigger to poll every 15 minutes, then set the **Frequency** to **Minute**, and set the **Interval** to **15**.</span></span> 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. <span data-ttu-id="0a480-136">**Mentés** a módosításokat (bal felső sarkában az eszköztár).</span><span class="sxs-lookup"><span data-stu-id="0a480-136">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="0a480-137">A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="0a480-137">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="0a480-138">Egy művelettel</span><span class="sxs-lookup"><span data-stu-id="0a480-138">Use an action</span></span>
<span data-ttu-id="0a480-139">Egy művelet során a logikai alkalmazás definiált munkafolyamat által végzett.</span><span class="sxs-lookup"><span data-stu-id="0a480-139">An action is an operation carried out by the workflow defined in a logic app.</span></span> <span data-ttu-id="0a480-140">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="0a480-140">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="0a480-141">Kattintson a plusz ikonra.</span><span class="sxs-lookup"><span data-stu-id="0a480-141">Select the plus sign.</span></span> <span data-ttu-id="0a480-142">Több beállítások megtekintéséhez: **művelet hozzáadása**, **feltétel hozzáadása**, vagy az egyik a **további** beállítások.</span><span class="sxs-lookup"><span data-stu-id="0a480-142">You see several choices: **Add an action**, **Add a condition**, or one of the **More** options.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. <span data-ttu-id="0a480-143">Válasszon **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="0a480-143">Choose **Add an action**.</span></span>
3. <span data-ttu-id="0a480-144">A szövegmezőbe írja be a "office 365" az összes elérhető műveletek listájának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="0a480-144">In the text box, type “office 365” to get a list of all the available actions.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. <span data-ttu-id="0a480-145">Válassza ki a fenti példában **Office 365 Outlook - ügyfél létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0a480-145">In our example, choose **Office 365 Outlook - Create contact**.</span></span> <span data-ttu-id="0a480-146">Ha már létezik egy kapcsolat, majd válassza ki a **mappa azonosítója**, **Utónév**, és egyéb tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="0a480-146">If a connection already exists, then choose the **Folder ID**, **Given Name**, and other properties:</span></span>  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    <span data-ttu-id="0a480-147">Ha a kapcsolati adatokat kéri, adja meg a részleteket a VPN-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0a480-147">If you are prompted for the connection information, then enter the details to create the connection.</span></span> <span data-ttu-id="0a480-148">[A kapcsolat létrehozása](connectors-create-api-office365-outlook.md#create-the-connection) a témakörben ismertetett ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="0a480-148">[Create the connection](connectors-create-api-office365-outlook.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="0a480-149">Ebben a példában az Office 365 Outlook új ügyfél tudjuk létrehozni.</span><span class="sxs-lookup"><span data-stu-id="0a480-149">In this example, we create a new contact in Office 365 Outlook.</span></span> <span data-ttu-id="0a480-150">Egy másik eseményindító kimenete hozhat létre az ügyfélhez.</span><span class="sxs-lookup"><span data-stu-id="0a480-150">You can use output from another trigger to create the contact.</span></span> <span data-ttu-id="0a480-151">Adja hozzá például a SalesForce *egy objektumának létrejöttekor* eseményindító.</span><span class="sxs-lookup"><span data-stu-id="0a480-151">For example, add the SalesForce *When an object is created* trigger.</span></span> <span data-ttu-id="0a480-152">Adja meg az Office 365 Outlook *ügyfél létrehozása* művelet, amely a SalesForce mezők használja az Office 365-ben az új új ügyfél létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0a480-152">Then add the Office 365 Outlook *Create contact* action that uses the SalesForce fields to create the new new contact in Office 365.</span></span> 
   > 
   > 
5. <span data-ttu-id="0a480-153">**Mentés** a módosításokat (bal felső sarkában az eszköztár).</span><span class="sxs-lookup"><span data-stu-id="0a480-153">**Save** your changes (top left corner of the toolbar).</span></span> <span data-ttu-id="0a480-154">A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="0a480-154">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="0a480-155">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="0a480-155">Connector-specific details</span></span>

<span data-ttu-id="0a480-156">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/office365connector/).</span><span class="sxs-lookup"><span data-stu-id="0a480-156">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/office365connector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0a480-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0a480-157">Next Steps</span></span>
<span data-ttu-id="0a480-158">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="0a480-158">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="0a480-159">Az egyéb rendelkezésre álló összekötők Logic Apps, megismerkedhet a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="0a480-159">Explore the other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

