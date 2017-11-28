---
title: "A Batch-folyamat üzenetek egy csoport vagy a gyűjtemény - Azure Logic Apps |} Microsoft Docs"
description: "A kötegelt feldolgozáson a logic apps üzeneteket küldjön és fogadjon"
keywords: "kötegelt, kötegelt folyamat"
author: jonfancey
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/7/2017
ms.author: LADocs; estfan; jonfan
ms.openlocfilehash: 480ffce5dbe7c25181bb0ba5639de884e98ff4e6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="send-receive-and-batch-process-messages-in-logic-apps"></a><span data-ttu-id="21d0d-104">Küldése, fogadására és a batch-folyamat üzenetek a logic Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="21d0d-104">Send, receive, and batch process messages in logic apps</span></span>

<span data-ttu-id="21d0d-105">Együtt a csoportokban lévő üzenetek feldolgozásához, elküldheti az elemeket, vagy üzenetek, a egy *kötegelt*, és majd dolgozni elemeket a köteg.</span><span class="sxs-lookup"><span data-stu-id="21d0d-105">To process messages together in groups, you can send data items, or messages, to a *batch*, and then process those items as a batch.</span></span> <span data-ttu-id="21d0d-106">Ez a módszer akkor hasznos, ha azt szeretné, hogy az elemeket a meghatározott módon vannak csoportosítva, és dolgoznak együtt.</span><span class="sxs-lookup"><span data-stu-id="21d0d-106">This approach is useful when you want to make sure data items are grouped in a specific way and are processed together.</span></span> 

<span data-ttu-id="21d0d-107">A logic apps elemek fogadó kötegként használatával hozhat létre a **kötegelt** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="21d0d-107">You can create logic apps that receive items as a batch by using the **Batch** trigger.</span></span> <span data-ttu-id="21d0d-108">A logic apps által küldött elemeket a köteg használatával is létrehozhat a **kötegelt** művelet.</span><span class="sxs-lookup"><span data-stu-id="21d0d-108">You can then create logic apps that send items to a batch by using the **Batch** action.</span></span>

<span data-ttu-id="21d0d-109">Ez a témakör bemutatja, hogyan hozhat létre egy kötegelési megoldás ezen feladatok végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="21d0d-109">This topic shows how you can build a batching solution by performing these tasks:</span></span> 

* <span data-ttu-id="21d0d-110">[Fogadó és elemek gyűjt kötegként logikai alkalmazás létrehozása](#batch-receiver).</span><span class="sxs-lookup"><span data-stu-id="21d0d-110">[Create a logic app that receives and collects items as a batch](#batch-receiver).</span></span> <span data-ttu-id="21d0d-111">A "batch fogadó" logikai alkalmazás határozza meg a kötegelt nevét és verzióját leíró feltételek teljesítéséhez, mielőtt a fogadó logikai alkalmazás kiadja, és feldolgozza elemeket.</span><span class="sxs-lookup"><span data-stu-id="21d0d-111">This "batch receiver" logic app specifies the batch name and release criteria to meet before the receiver logic app releases and processes items.</span></span> 

* <span data-ttu-id="21d0d-112">[Hozzon létre egy logikai alkalmazás, amely elemeket küld egy kötegelt](#batch-sender).</span><span class="sxs-lookup"><span data-stu-id="21d0d-112">[Create a logic app that sends items to a batch](#batch-sender).</span></span> <span data-ttu-id="21d0d-113">A "batch küldő" logikai alkalmazás határozza meg, hova küldje a cikkek, amelyek egy meglévő kötegelt fogadó logic app kell lennie.</span><span class="sxs-lookup"><span data-stu-id="21d0d-113">This "batch sender" logic app specifies where to send items, which must be an existing batch receiver logic app.</span></span> <span data-ttu-id="21d0d-114">Egyedi kulcs, például egy ügyfél száma "partíció", vagy ossza, a cél kötegelt kulcs alapján részekre is megadható.</span><span class="sxs-lookup"><span data-stu-id="21d0d-114">You can also specify a unique key, like a customer number, to "partition", or divide, the target batch into subsets based on that key.</span></span> <span data-ttu-id="21d0d-115">Így minden elem ezzel a kulccsal összegyűjti és feldolgozza együtt.</span><span class="sxs-lookup"><span data-stu-id="21d0d-115">That way, all items with that key are collected and processed together.</span></span> 

## <a name="requirements"></a><span data-ttu-id="21d0d-116">Követelmények</span><span class="sxs-lookup"><span data-stu-id="21d0d-116">Requirements</span></span>

<span data-ttu-id="21d0d-117">Kövesse az alábbi példát, ezek az elemek szükségesek:</span><span class="sxs-lookup"><span data-stu-id="21d0d-117">To follow this example, you need these items:</span></span>

* <span data-ttu-id="21d0d-118">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="21d0d-118">An Azure subscription.</span></span> <span data-ttu-id="21d0d-119">Ha nem rendelkezik előfizetéssel, [kezdhet egy ingyenes Azure-fiókkal](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="21d0d-119">If you don't have a subscription, you can [start with a free Azure account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="21d0d-120">Egyéb esetben [regisztrálhat használatalapú fizetéses előfizetésre](https://azure.microsoft.com/pricing/purchase-options/).</span><span class="sxs-lookup"><span data-stu-id="21d0d-120">Otherwise, you can [sign up for a Pay-As-You-Go subscription](https://azure.microsoft.com/pricing/purchase-options/).</span></span>

* <span data-ttu-id="21d0d-121">Alapszintű ismerete [logic Apps alkalmazások létrehozása](../logic-apps/logic-apps-create-a-logic-app.md)</span><span class="sxs-lookup"><span data-stu-id="21d0d-121">Basic knowledge about [how to create logic apps](../logic-apps/logic-apps-create-a-logic-app.md)</span></span> 

* <span data-ttu-id="21d0d-122">Az összes e-mail fiókot [Azure Logic Apps által támogatott e-mail szolgáltató](../connectors/apis-list.md)</span><span class="sxs-lookup"><span data-stu-id="21d0d-122">An email account with any [email provider supported by Azure Logic Apps](../connectors/apis-list.md)</span></span>

<a name="batch-receiver"></a>

## <a name="create-logic-apps-that-receive-messages-as-a-batch"></a><span data-ttu-id="21d0d-123">Üzenetek fogadása egy kötegelt logic Apps-alkalmazások létrehozása</span><span class="sxs-lookup"><span data-stu-id="21d0d-123">Create logic apps that receive messages as a batch</span></span>

<span data-ttu-id="21d0d-124">Mielőtt üzeneteket küldhetnek egy tranzakcióköteghez, létre kell hoznia egy "batch fogadó" logikai alkalmazást a a **kötegelt** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="21d0d-124">Before you can send messages to a batch, you must first create a "batch receiver" logic app with the **Batch** trigger.</span></span> <span data-ttu-id="21d0d-125">Ezzel a módszerrel választhat a fogadó logikai alkalmazás a küldő logikai alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="21d0d-125">That way, you can select this receiver logic app when you create the sender logic app.</span></span> <span data-ttu-id="21d0d-126">A címzett adja meg a kötegelt, kiadási feltételek, és egyéb beállításokat.</span><span class="sxs-lookup"><span data-stu-id="21d0d-126">For the receiver, you specify the batch name, release criteria, and other settings.</span></span> 

<span data-ttu-id="21d0d-127">Küldő logic Apps alkalmazásokat kell tudja, hova küldje a elemek, amíg a fogadó logic Apps alkalmazásokat nem kell ismernie a küldők.</span><span class="sxs-lookup"><span data-stu-id="21d0d-127">Sender logic apps need know where to send items, while receiver logic apps don't need to know anything about the senders.</span></span>

1. <span data-ttu-id="21d0d-128">Az a [Azure-portálon](https://portal.azure.com), logikai alkalmazás létrehozása ezen a néven: "BatchReceiver"</span><span class="sxs-lookup"><span data-stu-id="21d0d-128">In the [Azure portal](https://portal.azure.com), create a logic app with this name: "BatchReceiver"</span></span> 

2. <span data-ttu-id="21d0d-129">A Logic Apps Designer, adja hozzá a **kötegelt** eseményindító, amely elindítja a logic app munkafolyamatot.</span><span class="sxs-lookup"><span data-stu-id="21d0d-129">In Logic Apps Designer, add the **Batch** trigger, which starts your logic app workflow.</span></span> <span data-ttu-id="21d0d-130">A keresési mezőbe írja be a "batch" szűrőként.</span><span class="sxs-lookup"><span data-stu-id="21d0d-130">In the search box, enter "batch" as your filter.</span></span> <span data-ttu-id="21d0d-131">Válassza ki az ehhez az eseményindítóhoz: **kötegelt – kötegelt üzenetek**</span><span class="sxs-lookup"><span data-stu-id="21d0d-131">Select this trigger: **Batch – Batch messages**</span></span>

   ![Kötegelt eseményindító hozzáadása](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. <span data-ttu-id="21d0d-133">Adjon meg egy nevet a kötegelt, és a feltételek megadása a kötegelt például feloldása:</span><span class="sxs-lookup"><span data-stu-id="21d0d-133">Provide a name for the batch, and specify criteria for releasing the batch, for example:</span></span>

   * <span data-ttu-id="21d0d-134">**A Batch-név**: A a kötegben, amely "TestBatch" Ebben a példában azonosítására használt nevet.</span><span class="sxs-lookup"><span data-stu-id="21d0d-134">**Batch Name**: The name used to identify the batch, which is "TestBatch" in this example.</span></span>
   * <span data-ttu-id="21d0d-135">**Üzenetek száma**: ahhoz, hogy a kötegelt feldolgozásra, amely "5" Ebben a példában kibocsátása előtt üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="21d0d-135">**Message Count**: The number of messages to hold as a batch before releasing for processing, which is "5" in this example.</span></span>

   ![Kötegelt eseményindító részleteinek megadása](./media/logic-apps-batch-process-send-receive-messages/receive-batch-trigger-details.png)

4. <span data-ttu-id="21d0d-137">Adja hozzá egy másik művelet, amely egy e-mailt küld, amikor a kötegelt eseményindító következik be.</span><span class="sxs-lookup"><span data-stu-id="21d0d-137">Add another action that sends an email when the batch trigger fires.</span></span> <span data-ttu-id="21d0d-138">Minden alkalommal, amikor a kötegelt rendelkezik öt elemek, a logikai alkalmazást egy e-mailt küld.</span><span class="sxs-lookup"><span data-stu-id="21d0d-138">Each time the batch has five items, the logic app sends an email.</span></span>

   1. <span data-ttu-id="21d0d-139">Válassza ki a kötegelt eseményindító **+ új lépés** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-139">Under the batch trigger, choose **+ New Step** > **Add an action**.</span></span>

   2. <span data-ttu-id="21d0d-140">A keresési mezőbe írja be az "e-mail" szűrőként.</span><span class="sxs-lookup"><span data-stu-id="21d0d-140">In the search box, enter "email" as your filter.</span></span>
   <span data-ttu-id="21d0d-141">Az e-mailt provider alapján, válassza ki az e-mailek csatlakozó.</span><span class="sxs-lookup"><span data-stu-id="21d0d-141">Based on your email provider, select an email connector.</span></span>
   
      <span data-ttu-id="21d0d-142">Például, ha munkahelyi vagy iskolai fiókkal rendelkezik, jelölje be az Office 365 Outlook-összekötő.</span><span class="sxs-lookup"><span data-stu-id="21d0d-142">For example, if you have a work or school account, select the Office 365 Outlook connector.</span></span> 
      <span data-ttu-id="21d0d-143">Ha Gmail fiókkal rendelkezik, válassza ki a Gmail összekötőt.</span><span class="sxs-lookup"><span data-stu-id="21d0d-143">If you have a Gmail account, select the Gmail connector.</span></span>

   3. <span data-ttu-id="21d0d-144">Válassza ki a művelet az összekötőhöz:  **{*e-mailt provider*}-küldjön egy e-mailek **</span><span class="sxs-lookup"><span data-stu-id="21d0d-144">Select this action for your connector: **{*email provider*} - Send an email**</span></span>

      <span data-ttu-id="21d0d-145">Példa:</span><span class="sxs-lookup"><span data-stu-id="21d0d-145">For example:</span></span>

      ![Válassza ki az e-mailt Provider "E-mail küldési" műveletet](./media/logic-apps-batch-process-send-receive-messages/add-send-email-action.png)

5. <span data-ttu-id="21d0d-147">Ha a kapcsolat korábban az e-mailek szolgáltató nem hozott létre, e-mailek hitelesítő adatok megadása a hitelesítéshez, ha a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="21d0d-147">If you didn't previously create a connection for your email provider, provide your email credentials for authentication when prompted.</span></span> <span data-ttu-id="21d0d-148">További információ [az e-mailek hitelesítő adatok hitelesítése](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="21d0d-148">Learn more about [authenticating your email credentials](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

6. <span data-ttu-id="21d0d-149">A művelet az előzőekben adott hozzá tulajdonságainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="21d0d-149">Set the properties for the action you just added.</span></span>

   * <span data-ttu-id="21d0d-150">Az a **való** mezőbe írja be a címzett e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="21d0d-150">In the **To** box, enter the recipient's email address.</span></span> 
   <span data-ttu-id="21d0d-151">Tesztelési célokra használhatja az e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="21d0d-151">For testing purposes, you can use your own email address.</span></span>

   * <span data-ttu-id="21d0d-152">A a **tulajdonos** be, amikor a **dinamikus tartalom** megjelenik a listán, válassza ki a **partíciónév** mező.</span><span class="sxs-lookup"><span data-stu-id="21d0d-152">In the **Subject** box, when the **Dynamic content** list appears, select the **Partition Name** field.</span></span>

     ![A "Dinamikus tartalom" listából válassza a "Partíció neve"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     <span data-ttu-id="21d0d-154">Egy későbbi szakasz ismerteti adjon meg egy egyedi partíciós kulcs, amely felosztja a célként megadott köteg logikai készletek ahol küldhet üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="21d0d-154">In a later section, you can specify a unique partition key that divides the target batch into logical sets to where you can send messages.</span></span> 
     <span data-ttu-id="21d0d-155">Minden a küldő logikai alkalmazás által létrehozott egyedi szám van.</span><span class="sxs-lookup"><span data-stu-id="21d0d-155">Each set has a unique number that's generated by the sender logic app.</span></span> 
     <span data-ttu-id="21d0d-156">A funkció lehetővé teszi a kötegek használjon több részhalmazának, és adja meg a névvel, Ön által biztosított minden részhalmaza.</span><span class="sxs-lookup"><span data-stu-id="21d0d-156">This capability lets you use a single batch with multiple subsets and define each subset with the name that you provide.</span></span>

   * <span data-ttu-id="21d0d-157">A a **törzs** mezőben, ha a **dinamikus tartalom** megjelenik a listán, válassza ki a **üzenetazonosítója** mező.</span><span class="sxs-lookup"><span data-stu-id="21d0d-157">In the **Body** box, when the **Dynamic content** list appears, select the **Message Id** field.</span></span>

     ![A "Törzs" válassza a "Üzenet azonosítója"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

     <span data-ttu-id="21d0d-159">Mivel az e-mail küldési művelet a megadott tömb, automatikusan hozzáadja a Tervező egy **az egyes** körül hurok a **egy e-mailek küldése** művelet.</span><span class="sxs-lookup"><span data-stu-id="21d0d-159">Because the input for the send email action is an array, the designer automatically adds a **For each** loop around the **Send an email** action.</span></span> 
     <span data-ttu-id="21d0d-160">Ez a ciklus a kötegben lévő egyes elemek belső műveletet végez.</span><span class="sxs-lookup"><span data-stu-id="21d0d-160">This loop performs the inner action on each item in the batch.</span></span> 
     <span data-ttu-id="21d0d-161">Így a kötegelt eseményindító öt elemet beállítva, az beszerzése öt e-mailek minden idő az eseményindító következik be.</span><span class="sxs-lookup"><span data-stu-id="21d0d-161">So, with the batch trigger set to five items, you get five emails each time the trigger fires.</span></span>

7.  <span data-ttu-id="21d0d-162">Most, hogy a létrehozott logikai alkalmazás kötegelt fogadó, mentse a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="21d0d-162">Now that you created a batch receiver logic app, save your logic app.</span></span>

    ![A logikai alkalmazás mentése](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

<a name="batch-sender"></a>

## <a name="create-logic-apps-that-send-messages-to-a-batch"></a><span data-ttu-id="21d0d-164">Üzenetek küldése egy kötegelt logic Apps-alkalmazások létrehozása</span><span class="sxs-lookup"><span data-stu-id="21d0d-164">Create logic apps that send messages to a batch</span></span>

<span data-ttu-id="21d0d-165">Most hozzon létre egy vagy több logikai alkalmazások által küldött elemeket a köteg határozzák meg a fogadó logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="21d0d-165">Now create one or more logic apps that send items to the batch defined by the receiver logic app.</span></span> <span data-ttu-id="21d0d-166">A küldő megadhatja a fogadó logikai alkalmazás és a neve, üzenet tartalma és egyéb beállításokat.</span><span class="sxs-lookup"><span data-stu-id="21d0d-166">For the sender, you specify the receiver logic app and batch name, message content, and any other settings.</span></span> <span data-ttu-id="21d0d-167">Megadhat egyedi partíciókulcsot részekre a kötegelt elemek gyűjtéséhez ezzel a kulccsal.</span><span class="sxs-lookup"><span data-stu-id="21d0d-167">You can optionally provide a unique partition key to divide the batch into subsets to collect items with that key.</span></span>

<span data-ttu-id="21d0d-168">Küldő logic Apps alkalmazásokat kell tudja, hova küldje a elemek, amíg a fogadó logic Apps alkalmazásokat nem kell ismernie a küldők.</span><span class="sxs-lookup"><span data-stu-id="21d0d-168">Sender logic apps need know where to send items, while receiver logic apps don't need to know anything about the senders.</span></span>

1. <span data-ttu-id="21d0d-169">Egy másik logikai alkalmazás létrehozása ezen a néven: "BatchSender"</span><span class="sxs-lookup"><span data-stu-id="21d0d-169">Create another logic app with this name: "BatchSender"</span></span>

   1. <span data-ttu-id="21d0d-170">A keresési mezőbe írja be a "ismétlődési" szűrőként.</span><span class="sxs-lookup"><span data-stu-id="21d0d-170">In the search box, enter "recurrence" as your filter.</span></span> 
   <span data-ttu-id="21d0d-171">Válassza ki az ehhez az eseményindítóhoz: **ütemezés - ismétlődési**</span><span class="sxs-lookup"><span data-stu-id="21d0d-171">Select this trigger: **Schedule - Recurrence**</span></span>

      ![A "Ismétlődés ütemezése" eseményindító hozzáadása](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-receiver.png)

   2. <span data-ttu-id="21d0d-173">Adja meg a gyakoriság és a feladó futtatásának időköze logikai alkalmazás percenként.</span><span class="sxs-lookup"><span data-stu-id="21d0d-173">Set the frequency and interval to run the sender logic app every minute.</span></span>

      ![Gyakoriság és eseményindító ismétlődési időköz beállítása](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-receiver-details.png)

2. <span data-ttu-id="21d0d-175">Adjon hozzá egy új lépést egy kötegelt üzenetküldésre.</span><span class="sxs-lookup"><span data-stu-id="21d0d-175">Add a new step for sending messages to a batch.</span></span>

   1. <span data-ttu-id="21d0d-176">Válassza ki az ismétlődési eseményindító **+ új lépés** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-176">Under the recurrence trigger, choose **+ New Step** > **Add an action**.</span></span>

   2. <span data-ttu-id="21d0d-177">A keresési mezőbe írja be a "batch" szűrőként.</span><span class="sxs-lookup"><span data-stu-id="21d0d-177">In the search box, enter "batch" as your filter.</span></span> 

   3. <span data-ttu-id="21d0d-178">Ez a művelet kiválasztása: **üzenetek küldését a batch- – válassza ki a Logic Apps-munkafolyamat kötegelt eseményindító**</span><span class="sxs-lookup"><span data-stu-id="21d0d-178">Select this action: **Send messages to batch – Choose a Logic Apps workflow with batch trigger**</span></span>

      ![Jelölje be a "Batch-üzenetek küldése"](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   4. <span data-ttu-id="21d0d-180">Most válassza a "BatchReceiver" logikai alkalmazás, amely korábban hozott létre, amely csomópontként jelenik meg a műveletet.</span><span class="sxs-lookup"><span data-stu-id="21d0d-180">Now select your "BatchReceiver" logic app that you previously created, which now appears as an action.</span></span>

      ![Válassza ki a "batch fogadó" logikai alkalmazás](./media/logic-apps-batch-process-send-receive-messages/send-batch-select-batch-receiver.png)

      > [!NOTE]
      > <span data-ttu-id="21d0d-182">A listán az más logikai alkalmazások kötegelt eseményindítók is látható.</span><span class="sxs-lookup"><span data-stu-id="21d0d-182">The list also shows any other logic apps that have batch triggers.</span></span>

3. <span data-ttu-id="21d0d-183">A kötegelt beállításainak megadása.</span><span class="sxs-lookup"><span data-stu-id="21d0d-183">Set the batch properties.</span></span>

   * <span data-ttu-id="21d0d-184">**A Batch-név**: határozzák meg a fogadó logikai alkalmazás, amely "TestBatch" Ebben a példában, és futásidőben érvényesíti a batch-neve.</span><span class="sxs-lookup"><span data-stu-id="21d0d-184">**Batch Name**: The batch name defined by the receiver logic app, which is "TestBatch" in this example and is validated at runtime.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="21d0d-185">Győződjön meg arról, hogy nem módosíthatja a kötegelt nevét, amely meg kell egyeznie a kötegelt a fogadó logikai alkalmazás által meghatározott.</span><span class="sxs-lookup"><span data-stu-id="21d0d-185">Make sure that you don't change the batch name, which must match the batch name that's specified by the receiver logic app.</span></span>
     > <span data-ttu-id="21d0d-186">A kötegelt nevének módosítása hatására a küldő logikai alkalmazás sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="21d0d-186">Changing the batch name causes the sender logic app to fail.</span></span>

   * <span data-ttu-id="21d0d-187">**Üzenet-tartalom**: az elküldeni kívánt üzenet tartalma.</span><span class="sxs-lookup"><span data-stu-id="21d0d-187">**Message Content**: The message content that you want to send.</span></span> 
   <span data-ttu-id="21d0d-188">Ebben a példában adja hozzá a kifejezés, amely az aktuális dátum és idő szúr be a a kötegelt küldendő üzenet tartalma:</span><span class="sxs-lookup"><span data-stu-id="21d0d-188">For this example, add this expression that inserts the current date and time into the message content that you send to the batch:</span></span>

     1. <span data-ttu-id="21d0d-189">Ha a **dinamikus tartalom** megjelenik a listán, válassza a **kifejezés**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-189">When the **Dynamic content** list appears, choose **Expression**.</span></span> 
     2. <span data-ttu-id="21d0d-190">Adja meg a kifejezés **utcnow()**, és válassza a **OK**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-190">Enter the expression **utcnow()**, and choose **OK**.</span></span> 

        ![A "Üzenet tartalom" válassza a "Kifejezése".](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details.png)

4. <span data-ttu-id="21d0d-193">Most már készen a köteg partíció.</span><span class="sxs-lookup"><span data-stu-id="21d0d-193">Now set up a partition for the batch.</span></span> <span data-ttu-id="21d0d-194">Válassza ki a "BatchReceiver" művelet **speciális beállítások megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-194">In the "BatchReceiver" action, choose **Show advanced options**.</span></span>

   * <span data-ttu-id="21d0d-195">**Partícióazonosító neve**: egy nem kötelező egyedi partíciókulcs a célként megadott köteg megosztásának használatára.</span><span class="sxs-lookup"><span data-stu-id="21d0d-195">**Partition Name**: An optional unique partition key to use for dividing the target batch.</span></span> <span data-ttu-id="21d0d-196">Vegye fel az ebben a példában egy kifejezés, amely egy és öt közötti véletlenszerű számot állít elő.</span><span class="sxs-lookup"><span data-stu-id="21d0d-196">For this example, add an expression that generates a random number between one and five.</span></span>
   
     1. <span data-ttu-id="21d0d-197">Ha a **dinamikus tartalom** megjelenik a listán, válassza a **kifejezés**.</span><span class="sxs-lookup"><span data-stu-id="21d0d-197">When the **Dynamic content** list appears, choose **Expression**.</span></span>
     2. <span data-ttu-id="21d0d-198">Adja meg a kifejezés: **rand(1,6)**</span><span class="sxs-lookup"><span data-stu-id="21d0d-198">Enter this expression: **rand(1,6)**</span></span>

        ![A célként megadott köteg partíció beállítása](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-partition-advanced-options.png)

        <span data-ttu-id="21d0d-200">Ez **VÉL** függvény hoz létre egy és 5 közötti számot.</span><span class="sxs-lookup"><span data-stu-id="21d0d-200">This **rand** function generates a number between one and five.</span></span> 
        <span data-ttu-id="21d0d-201">Ezért ez a köteg vannak felosztásával öt számozott partíciókra, amely ebben a kifejezésben dinamikusan.</span><span class="sxs-lookup"><span data-stu-id="21d0d-201">So you are dividing this batch into five numbered partitions, which this expression dynamically sets.</span></span>

   * <span data-ttu-id="21d0d-202">**Üzenet azonosítója**: nem kötelező azonosítót és egy előállított GUID Azonosítóhoz, ha üres.</span><span class="sxs-lookup"><span data-stu-id="21d0d-202">**Message Id**: An optional message identifier and is a generated GUID when empty.</span></span> 
   <span data-ttu-id="21d0d-203">Ehhez a példához hagyja üresen a mezőt.</span><span class="sxs-lookup"><span data-stu-id="21d0d-203">For this example, leave this box blank.</span></span>

5. <span data-ttu-id="21d0d-204">Mentse a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="21d0d-204">Save your logic app.</span></span> <span data-ttu-id="21d0d-205">A küldő logikai alkalmazás most hasonlít-e ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="21d0d-205">Your sender logic app now looks similar to this example:</span></span>

   ![Mentse a küldő Logic Apps alkalmazást](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details-finished.png)

## <a name="test-your-logic-apps"></a><span data-ttu-id="21d0d-207">A logic Apps alkalmazások tesztelése</span><span class="sxs-lookup"><span data-stu-id="21d0d-207">Test your logic apps</span></span>

<span data-ttu-id="21d0d-208">A kötegelési megoldás teszteléséhez hagyja meg a logic Apps alkalmazásokat futtató néhány percig.</span><span class="sxs-lookup"><span data-stu-id="21d0d-208">To test your batching solution, leave your logic apps running for a few minutes.</span></span> <span data-ttu-id="21d0d-209">Hamarosan először e-mailek első öt, mind az azonos partíciókulcsú csoportokban.</span><span class="sxs-lookup"><span data-stu-id="21d0d-209">Soon, you start getting emails in groups of five, all with the same partition key.</span></span>

<span data-ttu-id="21d0d-210">A BatchSender Logic Apps alkalmazást percenként fut, egy és öt közötti véletlenszerű számot állít elő, és a létrehozott számot használja a célként megadott köteg partíciókulcsnak, ahol az üzenetek küldése történik.</span><span class="sxs-lookup"><span data-stu-id="21d0d-210">Your BatchSender logic app runs every minute, generates a random number between one and five, and uses this generated number as the partition key for the target batch where messages are sent.</span></span> <span data-ttu-id="21d0d-211">Minden alkalommal, amikor a kötegelt rendelkezik ugyanazzal a partíciós kulccsal, öt elemet a BatchReceiver Logic Apps alkalmazást következik be, és minden üzenetet az e-mail küldése.</span><span class="sxs-lookup"><span data-stu-id="21d0d-211">Each time the batch has five items with the same partition key, your BatchReceiver logic app fires and sends mail for each message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21d0d-212">Ha elkészült teszteléshez, győződjön meg arról, hogy tiltsa le az üzenetek küldése és kerülje a túl van terhelve beérkezett BatchSender logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="21d0d-212">When you're done testing, make sure that you disable the BatchSender logic app to stop sending messages and avoid overloading your inbox.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21d0d-213">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="21d0d-213">Next steps</span></span>

* [<span data-ttu-id="21d0d-214">A JSON logikai alkalmazás definícióiról létrehozása</span><span class="sxs-lookup"><span data-stu-id="21d0d-214">Build on logic app definitions by using JSON</span></span>](../logic-apps/logic-apps-author-definitions.md)
* [<span data-ttu-id="21d0d-215">Egy kiszolgáló nélküli alkalmazást a Visual Studio és az Azure Logic Apps és függvények létrehozása</span><span class="sxs-lookup"><span data-stu-id="21d0d-215">Build a serverless app in Visual Studio with Azure Logic Apps and Functions</span></span>](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [<span data-ttu-id="21d0d-216">Kivétel kezelése és a hibanaplózás a logic Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="21d0d-216">Exception handling and error logging for logic apps</span></span>](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
