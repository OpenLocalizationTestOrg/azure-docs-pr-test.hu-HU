---
title: "egy csoport vagy a gyűjtemény - Azure Logic Apps aaaBatch folyamat üzenetek |} Microsoft Docs"
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
ms.openlocfilehash: 2603db71ee0659d5b6bf5ce3d32f1b0d13c34194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-receive-and-batch-process-messages-in-logic-apps"></a><span data-ttu-id="aa8d2-104">Küldése, fogadására és a batch-folyamat üzenetek a logic Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="aa8d2-104">Send, receive, and batch process messages in logic apps</span></span>

<span data-ttu-id="aa8d2-105">tooprocess üzenetek csoportokban együtt, küldhet adatokat elemek vagy az üzenetek, tooa *kötegelt*, és majd dolgozni elemeket a köteg.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-105">tooprocess messages together in groups, you can send data items, or messages, tooa *batch*, and then process those items as a batch.</span></span> <span data-ttu-id="aa8d2-106">Ezt a módszert akkor hasznos, ha azt szeretné, hogy toomake meg arról, hogy az elemeket a meghatározott módon vannak csoportosítva, és dolgoznak együtt.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-106">This approach is useful when you want toomake sure data items are grouped in a specific way and are processed together.</span></span> 

<span data-ttu-id="aa8d2-107">A logic apps elemek fogadó kötegként hello segítségével hozhat létre **kötegelt** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-107">You can create logic apps that receive items as a batch by using hello **Batch** trigger.</span></span> <span data-ttu-id="aa8d2-108">A logic apps által küldött elemek tooa kötegelt hello segítségével is létrehozhat **kötegelt** művelet.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-108">You can then create logic apps that send items tooa batch by using hello **Batch** action.</span></span>

<span data-ttu-id="aa8d2-109">Ez a témakör bemutatja, hogyan hozhat létre egy kötegelési megoldás ezen feladatok végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-109">This topic shows how you can build a batching solution by performing these tasks:</span></span> 

* <span data-ttu-id="aa8d2-110">[Fogadó és elemek gyűjt kötegként logikai alkalmazás létrehozása](#batch-receiver).</span><span class="sxs-lookup"><span data-stu-id="aa8d2-110">[Create a logic app that receives and collects items as a batch](#batch-receiver).</span></span> <span data-ttu-id="aa8d2-111">A "batch fogadó" logikai alkalmazás hello kötegelt nevét és verzióját leíró feltételeket toomeet határozza meg, mielőtt hello fogadó logikai alkalmazás kiadja, és feldolgozza elemeket.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-111">This "batch receiver" logic app specifies hello batch name and release criteria toomeet before hello receiver logic app releases and processes items.</span></span> 

* <span data-ttu-id="aa8d2-112">[Hozzon létre egy logikai alkalmazás által küldött elemek tooa kötegelt](#batch-sender).</span><span class="sxs-lookup"><span data-stu-id="aa8d2-112">[Create a logic app that sends items tooa batch](#batch-sender).</span></span> <span data-ttu-id="aa8d2-113">A "batch küldő" logikai alkalmazás meghatározza, hogy hol toosend elemek, amely lehet egy meglévő kötegelt fogadó logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-113">This "batch sender" logic app specifies where toosend items, which must be an existing batch receiver logic app.</span></span> <span data-ttu-id="aa8d2-114">Is adjon meg egy egyedi kulcs, például egy ügyfél száma, túl "partíció", vagy ossza, hello cél kötegelt kulcs alapján részekre.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-114">You can also specify a unique key, like a customer number, too"partition", or divide, hello target batch into subsets based on that key.</span></span> <span data-ttu-id="aa8d2-115">Így minden elem ezzel a kulccsal összegyűjti és feldolgozza együtt.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-115">That way, all items with that key are collected and processed together.</span></span> 

## <a name="requirements"></a><span data-ttu-id="aa8d2-116">Követelmények</span><span class="sxs-lookup"><span data-stu-id="aa8d2-116">Requirements</span></span>

<span data-ttu-id="aa8d2-117">toofollow ebben a példában a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-117">toofollow this example, you need these items:</span></span>

* <span data-ttu-id="aa8d2-118">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-118">An Azure subscription.</span></span> <span data-ttu-id="aa8d2-119">Ha nem rendelkezik előfizetéssel, [kezdhet egy ingyenes Azure-fiókkal](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="aa8d2-119">If you don't have a subscription, you can [start with a free Azure account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="aa8d2-120">Egyéb esetben [regisztrálhat használatalapú fizetéses előfizetésre](https://azure.microsoft.com/pricing/purchase-options/).</span><span class="sxs-lookup"><span data-stu-id="aa8d2-120">Otherwise, you can [sign up for a Pay-As-You-Go subscription](https://azure.microsoft.com/pricing/purchase-options/).</span></span>

* <span data-ttu-id="aa8d2-121">Alapszintű ismerete [hogyan toocreate logic Apps alkalmazások](../logic-apps/logic-apps-create-a-logic-app.md)</span><span class="sxs-lookup"><span data-stu-id="aa8d2-121">Basic knowledge about [how toocreate logic apps](../logic-apps/logic-apps-create-a-logic-app.md)</span></span> 

* <span data-ttu-id="aa8d2-122">Az összes e-mail fiókot [Azure Logic Apps által támogatott e-mail szolgáltató](../connectors/apis-list.md)</span><span class="sxs-lookup"><span data-stu-id="aa8d2-122">An email account with any [email provider supported by Azure Logic Apps](../connectors/apis-list.md)</span></span>

<a name="batch-receiver"></a>

## <a name="create-logic-apps-that-receive-messages-as-a-batch"></a><span data-ttu-id="aa8d2-123">Üzenetek fogadása egy kötegelt logic Apps-alkalmazások létrehozása</span><span class="sxs-lookup"><span data-stu-id="aa8d2-123">Create logic apps that receive messages as a batch</span></span>

<span data-ttu-id="aa8d2-124">Üzenetek tooa kötegelt elküldése előtt, előbb létre kell hoznia egy "batch fogadó" logikai alkalmazás hello **kötegelt** eseményindító.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-124">Before you can send messages tooa batch, you must first create a "batch receiver" logic app with hello **Batch** trigger.</span></span> <span data-ttu-id="aa8d2-125">Ezzel a módszerrel választhat a fogadó logikai alkalmazás hello küldő logikai alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-125">That way, you can select this receiver logic app when you create hello sender logic app.</span></span> <span data-ttu-id="aa8d2-126">Hello fogadó megadhatja, hello neve, a kiadási feltételek és az egyéb beállításokat.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-126">For hello receiver, you specify hello batch name, release criteria, and other settings.</span></span> 

<span data-ttu-id="aa8d2-127">Küldő logic apps kell tudnia, ahol toosend elem, amíg a fogadó logic Apps alkalmazásokat nem kell semmi kapcsolatos hello feladók tooknow.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-127">Sender logic apps need know where toosend items, while receiver logic apps don't need tooknow anything about hello senders.</span></span>

1. <span data-ttu-id="aa8d2-128">A hello [Azure-portálon](https://portal.azure.com), logikai alkalmazás létrehozása ezen a néven: "BatchReceiver"</span><span class="sxs-lookup"><span data-stu-id="aa8d2-128">In hello [Azure portal](https://portal.azure.com), create a logic app with this name: "BatchReceiver"</span></span> 

2. <span data-ttu-id="aa8d2-129">A Logic Apps Designer, adja hozzá a hello **kötegelt** eseményindító, amely elindítja a logic app munkafolyamatot.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-129">In Logic Apps Designer, add hello **Batch** trigger, which starts your logic app workflow.</span></span> <span data-ttu-id="aa8d2-130">Hello keresési mezőbe írja be a "batch" szűrőként.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-130">In hello search box, enter "batch" as your filter.</span></span> <span data-ttu-id="aa8d2-131">Válassza ki az ehhez az eseményindítóhoz: **kötegelt – kötegelt üzenetek**</span><span class="sxs-lookup"><span data-stu-id="aa8d2-131">Select this trigger: **Batch – Batch messages**</span></span>

   ![Kötegelt eseményindító hozzáadása](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. <span data-ttu-id="aa8d2-133">Adjon meg egy nevet hello kötegelt, és a feltételek megadása hello kötegelt, például feloldása:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-133">Provide a name for hello batch, and specify criteria for releasing hello batch, for example:</span></span>

   * <span data-ttu-id="aa8d2-134">**A Batch-név**: hello használt név tooidentify hello kötegelt, amely "TestBatch" Ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-134">**Batch Name**: hello name used tooidentify hello batch, which is "TestBatch" in this example.</span></span>
   * <span data-ttu-id="aa8d2-135">**Üzenetek száma**: hello toohold üzenetek száma a kötegelt feldolgozásra, amely "5" Ebben a példában feloldása előtt.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-135">**Message Count**: hello number of messages toohold as a batch before releasing for processing, which is "5" in this example.</span></span>

   ![Kötegelt eseményindító részleteinek megadása](./media/logic-apps-batch-process-send-receive-messages/receive-batch-trigger-details.png)

4. <span data-ttu-id="aa8d2-137">Adja hozzá egy másik művelet, amely egy e-mailt küld, amikor hello kötegelt eseményindító következik be.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-137">Add another action that sends an email when hello batch trigger fires.</span></span> <span data-ttu-id="aa8d2-138">Minden alkalommal hello kötegelt öt elemek, a hello logikai alkalmazás elküld egy e-mailt.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-138">Each time hello batch has five items, hello logic app sends an email.</span></span>

   1. <span data-ttu-id="aa8d2-139">Hello kötegelt eseményindító, válassza ki **+ új lépés** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-139">Under hello batch trigger, choose **+ New Step** > **Add an action**.</span></span>

   2. <span data-ttu-id="aa8d2-140">Hello keresési mezőbe írja be az "e-mail" szűrőként.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-140">In hello search box, enter "email" as your filter.</span></span>
   <span data-ttu-id="aa8d2-141">Az e-mailt provider alapján, válassza ki az e-mailek csatlakozó.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-141">Based on your email provider, select an email connector.</span></span>
   
      <span data-ttu-id="aa8d2-142">Például ha egy munkahelyi vagy iskolai fiókjával rendelkezik, válassza ki az hello Office 365 Outlook összekötő.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-142">For example, if you have a work or school account, select hello Office 365 Outlook connector.</span></span> 
      <span data-ttu-id="aa8d2-143">Ha Gmail fiókkal rendelkezik, válassza ki a hello Gmail összekötőt.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-143">If you have a Gmail account, select hello Gmail connector.</span></span>

   3. <span data-ttu-id="aa8d2-144">Válassza ki a művelet az összekötőhöz:  **{*e-mailt provider*}-küldjön egy e-mailek **</span><span class="sxs-lookup"><span data-stu-id="aa8d2-144">Select this action for your connector: **{*email provider*} - Send an email**</span></span>

      <span data-ttu-id="aa8d2-145">Példa:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-145">For example:</span></span>

      ![Válassza ki az e-mailt Provider "E-mail küldési" műveletet](./media/logic-apps-batch-process-send-receive-messages/add-send-email-action.png)

5. <span data-ttu-id="aa8d2-147">Ha a kapcsolat korábban az e-mailek szolgáltató nem hozott létre, e-mailek hitelesítő adatok megadása a hitelesítéshez, ha a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-147">If you didn't previously create a connection for your email provider, provide your email credentials for authentication when prompted.</span></span> <span data-ttu-id="aa8d2-148">További információ [az e-mailek hitelesítő adatok hitelesítése](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="aa8d2-148">Learn more about [authenticating your email credentials](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

6. <span data-ttu-id="aa8d2-149">Az előzőekben adott hozzá hello művelet hello tulajdonságainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-149">Set hello properties for hello action you just added.</span></span>

   * <span data-ttu-id="aa8d2-150">A hello **való** hello címzett e-mail címét adja meg.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-150">In hello **To** box, enter hello recipient's email address.</span></span> 
   <span data-ttu-id="aa8d2-151">Tesztelési célokra használhatja az e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-151">For testing purposes, you can use your own email address.</span></span>

   * <span data-ttu-id="aa8d2-152">A hello **tulajdonos** mezőben, ha hello **dinamikus tartalom** megjelenik a listán, válassza ki a hello **partíciónév** mező.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-152">In hello **Subject** box, when hello **Dynamic content** list appears, select hello **Partition Name** field.</span></span>

     ![Hello "Dinamikus tartalom" listából válassza a "Partíció neve"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     <span data-ttu-id="aa8d2-154">Egy későbbi szakasz ismerteti megadhatja, hogy felosztása a logikai cél kötegelt hello egyedi partíciókulcsot beállítja toowhere küldhet üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-154">In a later section, you can specify a unique partition key that divides hello target batch into logical sets toowhere you can send messages.</span></span> 
     <span data-ttu-id="aa8d2-155">Minden hello küldő logikai alkalmazás által létrehozott egyedi szám van.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-155">Each set has a unique number that's generated by hello sender logic app.</span></span> 
     <span data-ttu-id="aa8d2-156">A funkció lehetővé teszi a kötegek használata több részhalmazának, és adja meg a hello névvel, Ön által biztosított minden részhalmaza.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-156">This capability lets you use a single batch with multiple subsets and define each subset with hello name that you provide.</span></span>

   * <span data-ttu-id="aa8d2-157">A hello **törzs** mezőben, ha hello **dinamikus tartalom** megjelenik a listán, válassza ki a hello **üzenet azonosítója** mező.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-157">In hello **Body** box, when hello **Dynamic content** list appears, select hello **Message Id** field.</span></span>

     ![A "Törzs" válassza a "Üzenet azonosítója"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

     <span data-ttu-id="aa8d2-159">Mivel hello hello küldési e-mail művelet a megadott tömb, hello designer automatikusan hozzáadja a **minden** körül hello hurok **egy e-mailek küldése** művelet.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-159">Because hello input for hello send email action is an array, hello designer automatically adds a **For each** loop around hello **Send an email** action.</span></span> 
     <span data-ttu-id="aa8d2-160">Ez a ciklus egyes elemek hello kötegben hello belső művelet végez.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-160">This loop performs hello inner action on each item in hello batch.</span></span> 
     <span data-ttu-id="aa8d2-161">Igen hello kötegelt eseményindító set toofive elemekhez, kapott öt e-mailek, minden alkalommal hello eseményindító következik be.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-161">So, with hello batch trigger set toofive items, you get five emails each time hello trigger fires.</span></span>

7.  <span data-ttu-id="aa8d2-162">Most, hogy a létrehozott logikai alkalmazás kötegelt fogadó, mentse a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-162">Now that you created a batch receiver logic app, save your logic app.</span></span>

    ![A logikai alkalmazás mentése](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

<a name="batch-sender"></a>

## <a name="create-logic-apps-that-send-messages-tooa-batch"></a><span data-ttu-id="aa8d2-164">A logic apps által küldött üzenetek tooa kötegelt létrehozása</span><span class="sxs-lookup"><span data-stu-id="aa8d2-164">Create logic apps that send messages tooa batch</span></span>

<span data-ttu-id="aa8d2-165">Most hozzon létre egy vagy több logikai alkalmazások által küldött hello fogadó logikai alkalmazás által meghatározott elemek toohello kötegelt.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-165">Now create one or more logic apps that send items toohello batch defined by hello receiver logic app.</span></span> <span data-ttu-id="aa8d2-166">Hello küldő megadhatja a hello fogadó logikai alkalmazás és a neve, üzenet tartalma és egyéb beállításokat.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-166">For hello sender, you specify hello receiver logic app and batch name, message content, and any other settings.</span></span> <span data-ttu-id="aa8d2-167">Is megadhat egy egyedi partíciós kulcs toodivide hello kötegelt részhalmazának toocollect elemek az ezzel a kulccsal.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-167">You can optionally provide a unique partition key toodivide hello batch into subsets toocollect items with that key.</span></span>

<span data-ttu-id="aa8d2-168">Küldő logic apps kell tudnia, ahol toosend elem, amíg a fogadó logic Apps alkalmazásokat nem kell semmi kapcsolatos hello feladók tooknow.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-168">Sender logic apps need know where toosend items, while receiver logic apps don't need tooknow anything about hello senders.</span></span>

1. <span data-ttu-id="aa8d2-169">Egy másik logikai alkalmazás létrehozása ezen a néven: "BatchSender"</span><span class="sxs-lookup"><span data-stu-id="aa8d2-169">Create another logic app with this name: "BatchSender"</span></span>

   1. <span data-ttu-id="aa8d2-170">Hello keresési mezőbe írja be a "ismétlődési" szűrőként.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-170">In hello search box, enter "recurrence" as your filter.</span></span> 
   <span data-ttu-id="aa8d2-171">Válassza ki az ehhez az eseményindítóhoz: **ütemezés - ismétlődési**</span><span class="sxs-lookup"><span data-stu-id="aa8d2-171">Select this trigger: **Schedule - Recurrence**</span></span>

      ![Hello "Ismétlődés ütemezése" eseményindító hozzáadása](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-receiver.png)

   2. <span data-ttu-id="aa8d2-173">Állítsa be hello gyakoriságát és időköz toorun hello küldő logikai alkalmazás percenként.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-173">Set hello frequency and interval toorun hello sender logic app every minute.</span></span>

      ![Gyakoriság és eseményindító ismétlődési időköz beállítása](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-receiver-details.png)

2. <span data-ttu-id="aa8d2-175">Adjon hozzá egy új lépést tooa kötegelt üzenetek küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-175">Add a new step for sending messages tooa batch.</span></span>

   1. <span data-ttu-id="aa8d2-176">Hello ismétlődési eseményindító, válassza ki **+ új lépés** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-176">Under hello recurrence trigger, choose **+ New Step** > **Add an action**.</span></span>

   2. <span data-ttu-id="aa8d2-177">Hello keresési mezőbe írja be a "batch" szűrőként.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-177">In hello search box, enter "batch" as your filter.</span></span> 

   3. <span data-ttu-id="aa8d2-178">Ez a művelet kiválasztása: **toobatch üzenetek küldése – a Logic Apps-munkafolyamat kötegelt eseményindító kiválasztása**</span><span class="sxs-lookup"><span data-stu-id="aa8d2-178">Select this action: **Send messages toobatch – Choose a Logic Apps workflow with batch trigger**</span></span>

      ![Válassza a "Küldési üzenetek toobatch"](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   4. <span data-ttu-id="aa8d2-180">Most válassza a "BatchReceiver" logikai alkalmazás, amely korábban hozott létre, amely csomópontként jelenik meg a műveletet.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-180">Now select your "BatchReceiver" logic app that you previously created, which now appears as an action.</span></span>

      ![Válassza ki a "batch fogadó" logikai alkalmazás](./media/logic-apps-batch-process-send-receive-messages/send-batch-select-batch-receiver.png)

      > [!NOTE]
      > <span data-ttu-id="aa8d2-182">hello lista is mutatja az más logikai alkalmazások kötegelt eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-182">hello list also shows any other logic apps that have batch triggers.</span></span>

3. <span data-ttu-id="aa8d2-183">Hello kötegelt tulajdonságainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-183">Set hello batch properties.</span></span>

   * <span data-ttu-id="aa8d2-184">**A Batch-név**: hello hello fogadó logikai alkalmazás, amely "TestBatch" Ebben a példában, és futásidőben érvényesíti által definiált neve.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-184">**Batch Name**: hello batch name defined by hello receiver logic app, which is "TestBatch" in this example and is validated at runtime.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="aa8d2-185">Győződjön meg arról, hogy meg kell egyeznie a hello neve hello fogadó logikai alkalmazás által meghatározott hello kötegelt nevét ne módosítsa.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-185">Make sure that you don't change hello batch name, which must match hello batch name that's specified by hello receiver logic app.</span></span>
     > <span data-ttu-id="aa8d2-186">Hello változó neve a logic app toofail hello küldő okoz.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-186">Changing hello batch name causes hello sender logic app toofail.</span></span>

   * <span data-ttu-id="aa8d2-187">**Üzenet-tartalom**: hello üzenet tartalmát, amelyet az toosend.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-187">**Message Content**: hello message content that you want toosend.</span></span> 
   <span data-ttu-id="aa8d2-188">Ennél a példánál adja hozzá a kifejezést, hogy Beszúrások aktuális dátum és idő hello tartalom, hogy küldjön toohello kötegelt hello üzenetbe:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-188">For this example, add this expression that inserts hello current date and time into hello message content that you send toohello batch:</span></span>

     1. <span data-ttu-id="aa8d2-189">Ha hello **dinamikus tartalom** megjelenik a listán, válassza a **kifejezés**.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-189">When hello **Dynamic content** list appears, choose **Expression**.</span></span> 
     2. <span data-ttu-id="aa8d2-190">Adja meg a hello kifejezés **utcnow()**, és válassza a **OK**.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-190">Enter hello expression **utcnow()**, and choose **OK**.</span></span> 

        ![A "Üzenet tartalom" válassza a "Kifejezése".](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details.png)

4. <span data-ttu-id="aa8d2-193">Most már készen hello köteg partíció.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-193">Now set up a partition for hello batch.</span></span> <span data-ttu-id="aa8d2-194">Hello "BatchReceiver" művelet, válassza a **speciális beállítások megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-194">In hello "BatchReceiver" action, choose **Show advanced options**.</span></span>

   * <span data-ttu-id="aa8d2-195">**A partíció neve**: egy nem kötelező egyedi partíciós kulcs toouse hello cél kötegelt megosztásának.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-195">**Partition Name**: An optional unique partition key toouse for dividing hello target batch.</span></span> <span data-ttu-id="aa8d2-196">Vegye fel az ebben a példában egy kifejezés, amely egy és öt közötti véletlenszerű számot állít elő.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-196">For this example, add an expression that generates a random number between one and five.</span></span>
   
     1. <span data-ttu-id="aa8d2-197">Ha hello **dinamikus tartalom** megjelenik a listán, válassza a **kifejezés**.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-197">When hello **Dynamic content** list appears, choose **Expression**.</span></span>
     2. <span data-ttu-id="aa8d2-198">Adja meg a kifejezés: **rand(1,6)**</span><span class="sxs-lookup"><span data-stu-id="aa8d2-198">Enter this expression: **rand(1,6)**</span></span>

        ![A célként megadott köteg partíció beállítása](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-partition-advanced-options.png)

        <span data-ttu-id="aa8d2-200">Ez **VÉL** függvény hoz létre egy és 5 közötti számot.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-200">This **rand** function generates a number between one and five.</span></span> 
        <span data-ttu-id="aa8d2-201">Ezért ez a köteg vannak felosztásával öt számozott partíciókra, amely ebben a kifejezésben dinamikusan.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-201">So you are dividing this batch into five numbered partitions, which this expression dynamically sets.</span></span>

   * <span data-ttu-id="aa8d2-202">**Üzenet azonosítója**: nem kötelező azonosítót és egy előállított GUID Azonosítóhoz, ha üres.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-202">**Message Id**: An optional message identifier and is a generated GUID when empty.</span></span> 
   <span data-ttu-id="aa8d2-203">Ehhez a példához hagyja üresen a mezőt.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-203">For this example, leave this box blank.</span></span>

5. <span data-ttu-id="aa8d2-204">Mentse a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-204">Save your logic app.</span></span> <span data-ttu-id="aa8d2-205">A küldő logikai alkalmazás most következőhöz hasonló toothis példa:</span><span class="sxs-lookup"><span data-stu-id="aa8d2-205">Your sender logic app now looks similar toothis example:</span></span>

   ![Mentse a küldő Logic Apps alkalmazást](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details-finished.png)

## <a name="test-your-logic-apps"></a><span data-ttu-id="aa8d2-207">A logic Apps alkalmazások tesztelése</span><span class="sxs-lookup"><span data-stu-id="aa8d2-207">Test your logic apps</span></span>

<span data-ttu-id="aa8d2-208">tootest a kötegelés megoldás, hagyja a logic Apps alkalmazásokat futtató néhány percig.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-208">tootest your batching solution, leave your logic apps running for a few minutes.</span></span> <span data-ttu-id="aa8d2-209">Hamarosan e-mailek első öt csoportokban indítása, mindezt hello azonos partícióazonosító kulcs.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-209">Soon, you start getting emails in groups of five, all with hello same partition key.</span></span>

<span data-ttu-id="aa8d2-210">A BatchSender Logic Apps alkalmazást percenként fut, egy és öt közötti véletlenszerű számot állít elő, és a létrehozott szám használ kulcsaként hello partíció hello cél kötegelt, ahol az üzenetek küldése történik.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-210">Your BatchSender logic app runs every minute, generates a random number between one and five, and uses this generated number as hello partition key for hello target batch where messages are sent.</span></span> <span data-ttu-id="aa8d2-211">Minden alkalommal, amikor hello kötegelt hello öt elemek rendelkezik ugyanazzal a partíciókulccsal, a BatchReceiver Logic Apps alkalmazást következik be, és elküldi mail minden egyes üzenet esetében.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-211">Each time hello batch has five items with hello same partition key, your BatchReceiver logic app fires and sends mail for each message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aa8d2-212">Amikor végzett tesztelése, győződjön meg arról, tiltsa le a hello BatchSender logic app toostop üzenetküldésre, és el lehessen kerülni a beérkezett túlterhelését.</span><span class="sxs-lookup"><span data-stu-id="aa8d2-212">When you're done testing, make sure that you disable hello BatchSender logic app toostop sending messages and avoid overloading your inbox.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa8d2-213">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aa8d2-213">Next steps</span></span>

* [<span data-ttu-id="aa8d2-214">A JSON logikai alkalmazás definícióiról létrehozása</span><span class="sxs-lookup"><span data-stu-id="aa8d2-214">Build on logic app definitions by using JSON</span></span>](../logic-apps/logic-apps-author-definitions.md)
* [<span data-ttu-id="aa8d2-215">Egy kiszolgáló nélküli alkalmazást a Visual Studio és az Azure Logic Apps és függvények létrehozása</span><span class="sxs-lookup"><span data-stu-id="aa8d2-215">Build a serverless app in Visual Studio with Azure Logic Apps and Functions</span></span>](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [<span data-ttu-id="aa8d2-216">Kivétel kezelése és a hibanaplózás a logic Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="aa8d2-216">Exception handling and error logging for logic apps</span></span>](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
