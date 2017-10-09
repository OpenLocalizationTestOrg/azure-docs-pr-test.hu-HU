---
title: "aaaAdd hello Office 365 Outlook-összekötőt a Logic Apps a |} Microsoft Docs"
description: "Hozzon létre a logic apps és az Office 365 Office 365 összekötő tooenable beavatkozást igényel. Például: létrehozása, szerkesztése és a partnerek és a naptári elemek frissítése."
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
ms.openlocfilehash: 86a573c9c54701de3d3f0500d19eaf545e0710ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-office-365-outlook-connector"></a><span data-ttu-id="ed3ae-104">Hello Office 365 Outlook összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="ed3ae-104">Get started with hello Office 365 Outlook connector</span></span>
<span data-ttu-id="ed3ae-105">hello Office 365 Outlook összekötő lehetővé teszi, hogy az Office 365 Outlook való együttműködéshez.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-105">hello Office 365 Outlook connector enables interaction with Outlook in Office 365.</span></span> <span data-ttu-id="ed3ae-106">Az összekötő toocreate, a Szerkesztés, és a frissítés névjegyeket és a naptári elemek használata is beolvasása, küldése és tooemail válasz.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-106">Use this connector toocreate, edit, and update contacts and calendar items, and also get, send, and reply tooemail.</span></span>

<span data-ttu-id="ed3ae-107">Az Office 365 Outlook meg:</span><span class="sxs-lookup"><span data-stu-id="ed3ae-107">With Office 365 Outlook, you:</span></span>

* <span data-ttu-id="ed3ae-108">A munkafolyamat belül Office 365 e-mailek és a naptári funkciók hello használatához felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-108">Build your workflow using hello email and calendar features within Office 365.</span></span> 
* <span data-ttu-id="ed3ae-109">Használja toostart váltja ki, a munkafolyamatot, ha egy új e-mailt, a naptárelemek frissítésekor és még sok más.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-109">Use triggers toostart your workflow when there is a new email, when a calendar item is updated, and more.</span></span>
* <span data-ttu-id="ed3ae-110">Műveletek toosend egy e-mailt használja, hozzon létre új naptár esemény, és több.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-110">Use actions toosend an email, create a new calendar event, and more.</span></span> <span data-ttu-id="ed3ae-111">Például, ha van egy új objektumot a Salesforce (trigger), küldjön egy e-mailt tooyour Office 365 Outlook (művelet).</span><span class="sxs-lookup"><span data-stu-id="ed3ae-111">For example, when there is a new object in Salesforce (a trigger), send an email tooyour Office 365 Outlook (an action).</span></span> 

<span data-ttu-id="ed3ae-112">Ez a témakör bemutatja, hogyan toouse hello Office 365 Outlook-összekötőt a logikai alkalmazás, és is listák hello eseményindítók és műveletek.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-112">This topic shows you how toouse hello Office 365 Outlook connector in a logic app, and also lists hello triggers and actions.</span></span>

> [!NOTE]
> <span data-ttu-id="ed3ae-113">Hello cikk e verziója tooLogic alkalmazások általános elérhetőségével (GA) vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-113">This version of hello article applies tooLogic Apps general availability (GA).</span></span>
> 
> 

<span data-ttu-id="ed3ae-114">További információk a Logic Apps toolearn lásd [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="ed3ae-114">toolearn more about Logic Apps, see [What are logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="connect-toooffice-365"></a><span data-ttu-id="ed3ae-115">Csatlakozás tooOffice 365</span><span class="sxs-lookup"><span data-stu-id="ed3ae-115">Connect tooOffice 365</span></span>
<span data-ttu-id="ed3ae-116">A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először létre kell hoznia egy *kapcsolat* toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-116">Before your logic app can access any service, you first create a *connection* toohello service.</span></span> <span data-ttu-id="ed3ae-117">Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-117">A connection provides connectivity between a logic app and another service.</span></span> <span data-ttu-id="ed3ae-118">Például tooconnect tooOffice 365 Outlook, először van szüksége az Office 365 *kapcsolat*.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-118">For example, tooconnect tooOffice 365 Outlook, you first need an Office 365 *connection*.</span></span> <span data-ttu-id="ed3ae-119">toocreate a kapcsolat létrehozásakor, adja meg a hello hitelesítő adatok általában használni kívánja a tooconnect tooaccess hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-119">toocreate a connection, enter hello credentials you normally use tooaccess hello service you wish tooconnect to.</span></span> <span data-ttu-id="ed3ae-120">Ezért az Office 365 Outlook, adja meg a hello hitelesítő adatok tooyour Office 365 fiók toocreate hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-120">So with Office 365 Outlook, enter hello credentials tooyour Office 365 account toocreate hello connection.</span></span>

## <a name="create-hello-connection"></a><span data-ttu-id="ed3ae-121">Hello kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed3ae-121">Create hello connection</span></span>
> [!INCLUDE [Steps toocreate a connection tooOffice 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a><span data-ttu-id="ed3ae-122">Eseményindítók</span><span class="sxs-lookup"><span data-stu-id="ed3ae-122">Use a trigger</span></span>
<span data-ttu-id="ed3ae-123">Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-123">A trigger is an event that can be used toostart hello workflow defined in a logic app.</span></span> <span data-ttu-id="ed3ae-124">Eseményindítók "lekérdezésére" hello szolgáltatást egy intervallum és a kívánt gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-124">Triggers "poll" hello service at an interval and frequency that you want.</span></span> <span data-ttu-id="ed3ae-125">[További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="ed3ae-125">[Learn more about triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="ed3ae-126">A hello logikai alkalmazás írja be a "office 365" tooget hello eseményindítók listáját:</span><span class="sxs-lookup"><span data-stu-id="ed3ae-126">In hello logic app, type "office 365" tooget a list of hello triggers:</span></span>  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. <span data-ttu-id="ed3ae-127">Válassza ki **Office 365 Outlook - jövőbeli esemény hamarosan indításakor**.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-127">Select **Office 365 Outlook - When an upcoming event is starting soon**.</span></span> <span data-ttu-id="ed3ae-128">Ha már létezik egy kapcsolat, majd válassza ki a naptárban hello legördülő listában.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-128">If a connection already exists, then select a calendar from hello drop-down list.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    <span data-ttu-id="ed3ae-129">Ha a kért toosign, majd adja meg az hello bejelentkezési részletek toocreate hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-129">If you are prompted toosign in, then enter hello sign in details toocreate hello connection.</span></span> <span data-ttu-id="ed3ae-130">[Hello kapcsolat létrehozása](connectors-create-api-office365-outlook.md#create-the-connection) ebben a témakörben hello lépéseket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-130">[Create hello connection](connectors-create-api-office365-outlook.md#create-the-connection) in this topic lists hello steps.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="ed3ae-131">Ebben a példában hello logikai alkalmazás fut, amikor a naptár esemény frissül.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-131">In this example, hello logic app runs when a calendar event is updated.</span></span> <span data-ttu-id="ed3ae-132">Ehhez az eseményindítóhoz toosee hello eredményeit hozzáadása, amely egy szöveges üzenetet küld Önnek egy másik művelet.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-132">toosee hello results of this trigger, add another action that sends you a text message.</span></span> <span data-ttu-id="ed3ae-133">Adja hozzá például hello Twilio *üzenet küldése* 15 perc múlva indul, amikor hello naptár esemény szövegek műveletet.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-133">For example, add hello Twilio *Send message* action that texts you when hello calendar event is starting in 15 minutes.</span></span> 
   > 
   > 
3. <span data-ttu-id="ed3ae-134">Jelölje be hello **szerkesztése** gombra, majd hello **gyakoriság** és **időköz** értékeket.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-134">Select hello **Edit** button and set hello **Frequency** and **Interval** values.</span></span> <span data-ttu-id="ed3ae-135">Például, ha azt szeretné, hello eseményindító toopoll 15 percenként, majd állítsa be hello **gyakoriság** túl**perc**, és a set hello **időköz** túl**15**.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-135">For example, if you want hello trigger toopoll every 15 minutes, then set hello **Frequency** too**Minute**, and set hello **Interval** too**15**.</span></span> 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. <span data-ttu-id="ed3ae-136">**Mentés** a módosításokat (bal felső sarkában hello eszköztár).</span><span class="sxs-lookup"><span data-stu-id="ed3ae-136">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="ed3ae-137">A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-137">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="use-an-action"></a><span data-ttu-id="ed3ae-138">Egy művelettel</span><span class="sxs-lookup"><span data-stu-id="ed3ae-138">Use an action</span></span>
<span data-ttu-id="ed3ae-139">Egy művelet által definiált logikai alkalmazás hello munkafolyamat során.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-139">An action is an operation carried out by hello workflow defined in a logic app.</span></span> <span data-ttu-id="ed3ae-140">[További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span><span class="sxs-lookup"><span data-stu-id="ed3ae-140">[Learn more about actions](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).</span></span>

1. <span data-ttu-id="ed3ae-141">Válassza ki a hello plusz jel.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-141">Select hello plus sign.</span></span> <span data-ttu-id="ed3ae-142">Több beállítások megtekintéséhez: **művelet hozzáadása**, **feltétel hozzáadása**, vagy egy hello **további** beállítások.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-142">You see several choices: **Add an action**, **Add a condition**, or one of hello **More** options.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. <span data-ttu-id="ed3ae-143">Válasszon **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-143">Choose **Add an action**.</span></span>
3. <span data-ttu-id="ed3ae-144">Hello szövegmezőbe írja be a "office 365" tooget összes hello elérhető műveletek listáját.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-144">In hello text box, type “office 365” tooget a list of all hello available actions.</span></span>
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. <span data-ttu-id="ed3ae-145">Válassza ki a fenti példában **Office 365 Outlook - ügyfél létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-145">In our example, choose **Office 365 Outlook - Create contact**.</span></span> <span data-ttu-id="ed3ae-146">Ha már létezik egy kapcsolat, majd válassza a hello **mappa azonosítója**, **Utónév**, és egyéb tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="ed3ae-146">If a connection already exists, then choose hello **Folder ID**, **Given Name**, and other properties:</span></span>  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    <span data-ttu-id="ed3ae-147">Ha hello kapcsolati adatokat kéri, adja meg hello részletek toocreate hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-147">If you are prompted for hello connection information, then enter hello details toocreate hello connection.</span></span> <span data-ttu-id="ed3ae-148">[Hello kapcsolat létrehozása](connectors-create-api-office365-outlook.md#create-the-connection) a témakörben ismertetett ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-148">[Create hello connection](connectors-create-api-office365-outlook.md#create-the-connection) in this topic describes these properties.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="ed3ae-149">Ebben a példában az Office 365 Outlook új ügyfél tudjuk létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-149">In this example, we create a new contact in Office 365 Outlook.</span></span> <span data-ttu-id="ed3ae-150">Egy másik eseményindító toocreate hello forduljon kimenete is használhatja.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-150">You can use output from another trigger toocreate hello contact.</span></span> <span data-ttu-id="ed3ae-151">Adja hozzá például a hello SalesForce *egy objektumának létrejöttekor* eseményindító.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-151">For example, add hello SalesForce *When an object is created* trigger.</span></span> <span data-ttu-id="ed3ae-152">Adja meg az Office 365 Outlook hello *ügyfél létrehozása* hello SalesForce használó művelet mezők toocreate hello új új lépjen kapcsolatba Office 365-ben.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-152">Then add hello Office 365 Outlook *Create contact* action that uses hello SalesForce fields toocreate hello new new contact in Office 365.</span></span> 
   > 
   > 
5. <span data-ttu-id="ed3ae-153">**Mentés** a módosításokat (bal felső sarkában hello eszköztár).</span><span class="sxs-lookup"><span data-stu-id="ed3ae-153">**Save** your changes (top left corner of hello toolbar).</span></span> <span data-ttu-id="ed3ae-154">A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="ed3ae-154">Your logic app is saved and may be automatically enabled.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="ed3ae-155">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="ed3ae-155">Connector-specific details</span></span>

<span data-ttu-id="ed3ae-156">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/office365connector/).</span><span class="sxs-lookup"><span data-stu-id="ed3ae-156">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/office365connector/).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="ed3ae-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ed3ae-157">Next Steps</span></span>
<span data-ttu-id="ed3ae-158">[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="ed3ae-158">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="ed3ae-159">Fedezze fel más rendelkezésre álló összekötők Logic Apps: hello a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="ed3ae-159">Explore hello other available connectors in Logic Apps at our [APIs list](apis-list.md).</span></span>

