---
title: "az Azure üzenetsor-üzeneteket által indított függvény aaaCreate |} Microsoft Docs"
description: "Használja az Azure Functions toocreate egy kiszolgáló nélküli függvény, amelyet egy üzenetek által benyújtott tooan Azure Storage üzenetsorába."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 0b609bc0-c264-4092-8e3e-0784dcc23b5d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/17/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 44db90fa80bf77e31bf53dddabd7136de5800b11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-messages-tooan-azure-storage-queue-using-functions"></a><span data-ttu-id="30384-103">Adja hozzá az üzenetek tooan Azure Storage üzenetsorába függvények használata</span><span class="sxs-lookup"><span data-stu-id="30384-103">Add messages tooan Azure Storage queue using Functions</span></span>

<span data-ttu-id="30384-104">Az Azure Functions bemeneti és kimeneti kötések adja meg a deklaratív módon tooconnect tooexternal szolgáltatás adatait a függvény.</span><span class="sxs-lookup"><span data-stu-id="30384-104">In Azure Functions, input and output bindings provide a declarative way tooconnect tooexternal service data from your function.</span></span> <span data-ttu-id="30384-105">Ebben a témakörben megtudhatja, hogyan tooupdate egy meglévő függvény adja hozzá egy kimeneti kötése, amely üzeneteket küld tooAzure a Queue storage.</span><span class="sxs-lookup"><span data-stu-id="30384-105">In this topic, learn how tooupdate an existing function by adding an output binding that sends messages tooAzure Queue storage.</span></span>  

![Hello naplókban üzenet megtekintése.](./media/functions-integrate-storage-queue-output-binding/functions-integrate-storage-binding-in-portal.png)

## <a name="prerequisites"></a><span data-ttu-id="30384-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="30384-107">Prerequisites</span></span> 

[!INCLUDE [Previous topics](../../includes/functions-quickstart-previous-topics.md)]

* <span data-ttu-id="30384-108">Telepítse a hello [Microsoft Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="30384-108">Install hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

## <span data-ttu-id="30384-109"><a name="add-binding"></a>Kimeneti kötés hozzáadása</span><span class="sxs-lookup"><span data-stu-id="30384-109"><a name="add-binding"></a>Add an output binding</span></span>
 
1. <span data-ttu-id="30384-110">Bontsa ki a függvényalkalmazást és a függvényt.</span><span class="sxs-lookup"><span data-stu-id="30384-110">Expand both your function app and your function.</span></span>

2. <span data-ttu-id="30384-111">Válassza ki **integráció** és **+ új kimeneti**, majd válassza **Azure Queue storage** válassza **kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="30384-111">Select **Integrate** and **+ New output**, then choose **Azure Queue storage** and choose **Select**.</span></span>
    
    ![Adja hozzá a várólista tárolási kimeneti kötése tooa függvény hello Azure-portálon.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding.png)

3. <span data-ttu-id="30384-113">Hello beállításokkal hello táblázatban megadottak szerint:</span><span class="sxs-lookup"><span data-stu-id="30384-113">Use hello settings as specified in hello table:</span></span> 

    ![Adja hozzá a várólista tárolási kimeneti kötése tooa függvény hello Azure-portálon.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding-2.png)

    | <span data-ttu-id="30384-115">Beállítás</span><span class="sxs-lookup"><span data-stu-id="30384-115">Setting</span></span>      |  <span data-ttu-id="30384-116">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="30384-116">Suggested value</span></span>   | <span data-ttu-id="30384-117">Leírás</span><span class="sxs-lookup"><span data-stu-id="30384-117">Description</span></span>                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | <span data-ttu-id="30384-118">**Üzenetsor neve**</span><span class="sxs-lookup"><span data-stu-id="30384-118">**Queue name**</span></span>   | <span data-ttu-id="30384-119">myqueue-items</span><span class="sxs-lookup"><span data-stu-id="30384-119">myqueue-items</span></span>    | <span data-ttu-id="30384-120">hello hello neve várólista tooconnect tooin a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="30384-120">hello name of hello queue tooconnect tooin your Storage account.</span></span> |
    | <span data-ttu-id="30384-121">**Tárfiók kapcsolata**</span><span class="sxs-lookup"><span data-stu-id="30384-121">**Storage account connection**</span></span> | <span data-ttu-id="30384-122">AzureWebJobStorage</span><span class="sxs-lookup"><span data-stu-id="30384-122">AzureWebJobStorage</span></span> | <span data-ttu-id="30384-123">A függvény alkalmazás által már használt hello tárolási fiók kapcsolat használatát, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="30384-123">You can use hello storage account connection already being used by your function app, or create a new one.</span></span>  |
    | <span data-ttu-id="30384-124">**Üzenet-paraméter neve**</span><span class="sxs-lookup"><span data-stu-id="30384-124">**Message parameter name**</span></span> | <span data-ttu-id="30384-125">outputQueueItem</span><span class="sxs-lookup"><span data-stu-id="30384-125">outputQueueItem</span></span> | <span data-ttu-id="30384-126">hello hello kimeneti kötési paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="30384-126">hello name of hello output binding parameter.</span></span> | 

4. <span data-ttu-id="30384-127">Kattintson a **mentése** tooadd hello kötés.</span><span class="sxs-lookup"><span data-stu-id="30384-127">Click **Save** tooadd hello binding.</span></span>
 
<span data-ttu-id="30384-128">Most, hogy egy meghatározott kimeneti kötése, tooupdate hello kód toouse hello kötés tooadd üzenetek tooa várólista szüksége.</span><span class="sxs-lookup"><span data-stu-id="30384-128">Now that you have an output binding defined, you need tooupdate hello code toouse hello binding tooadd messages tooa queue.</span></span>  

## <a name="update-hello-function-code"></a><span data-ttu-id="30384-129">Hello funkciókódot frissítése</span><span class="sxs-lookup"><span data-stu-id="30384-129">Update hello function code</span></span>

1. <span data-ttu-id="30384-130">Válassza ki a függvény toodisplay hello függvény kódot hello szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="30384-130">Select your function toodisplay hello function code in hello editor.</span></span> 

2. <span data-ttu-id="30384-131">C# függvény, frissítse a függvénydefiníciót tooadd hello az alábbi módon **outputQueueItem** tárolási kötési paraméter.</span><span class="sxs-lookup"><span data-stu-id="30384-131">For a C# function, update your function definition as follows tooadd hello **outputQueueItem** storage binding parameter.</span></span> <span data-ttu-id="30384-132">JavaScript-függvény esetében hagyja ki ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="30384-132">Skip this step for a JavaScript function.</span></span>

    ```cs   
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, 
        ICollector<string> outputQueueItem, TraceWriter log)
    {
        ....
    }
    ```

3. <span data-ttu-id="30384-133">Hozzáadása előtt hello metódus visszaadja a következő kód toohello függvény hello.</span><span class="sxs-lookup"><span data-stu-id="30384-133">Add hello following code toohello function just before hello method returns.</span></span> <span data-ttu-id="30384-134">A függvény hello nyelvi hello megfelelő részlet használja.</span><span class="sxs-lookup"><span data-stu-id="30384-134">Use hello appropriate snippet for hello language of your function.</span></span>

    ```javascript
    context.bindings.outputQueueItem = "Name passed toohello function: " + 
                (req.query.name || req.body.name);
    ```

    ```cs
    outputQueueItem.Add("Name passed toohello function: " + name);     
    ```

4. <span data-ttu-id="30384-135">Válassza ki **mentése** toosave módosításokat.</span><span class="sxs-lookup"><span data-stu-id="30384-135">Select **Save** toosave changes.</span></span>

<span data-ttu-id="30384-136">egy üzenetsor hozzáadott toohello toohello HTTP-eseményindítóval átadott hello érték tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="30384-136">hello value passed toohello HTTP trigger is included in a message added toohello queue.</span></span>
 
## <a name="test-hello-function"></a><span data-ttu-id="30384-137">Hello függvény tesztelése</span><span class="sxs-lookup"><span data-stu-id="30384-137">Test hello function</span></span> 

1. <span data-ttu-id="30384-138">Hello kód módosítások mentésekor után válassza ki **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="30384-138">After hello code changes are saved, select **Run**.</span></span> 

    ![Adja hozzá a várólista tárolási kimeneti kötése tooa függvény hello Azure-portálon.](./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png)

2. <span data-ttu-id="30384-140">Ellenőrizze a hello naplók toomake meg arról, hogy sikerült-e hello függvény.</span><span class="sxs-lookup"><span data-stu-id="30384-140">Check hello logs toomake sure that hello function succeeded.</span></span> <span data-ttu-id="30384-141">Egy új sor nevű **outqueue** hozta létre a tárfiókban lévő hello funkciók runtime hello kimeneti kötése először szolgál.</span><span class="sxs-lookup"><span data-stu-id="30384-141">A new queue named **outqueue** is created in your Storage account by hello Functions runtime when hello output binding is first used.</span></span>

<span data-ttu-id="30384-142">A következő tooyour tárolási fiók tooverify hello új várólista és tooit hozzáadott üdvözlőüzenetére is elérheti.</span><span class="sxs-lookup"><span data-stu-id="30384-142">Next, you can connect tooyour storage account tooverify hello new queue and hello message you added tooit.</span></span> 

## <a name="connect-toohello-queue"></a><span data-ttu-id="30384-143">Csatlakozás toohello várólista</span><span class="sxs-lookup"><span data-stu-id="30384-143">Connect toohello queue</span></span>

<span data-ttu-id="30384-144">Kihagyás hello első három lépést, ha már telepítette a Tártallózó és tooyour tárfiók csatlakozna.</span><span class="sxs-lookup"><span data-stu-id="30384-144">Skip hello first three steps if you have already installed Storage Explorer and connected it tooyour storage account.</span></span>    

1. <span data-ttu-id="30384-145">Válassza ki a függvény **integráció** és új hello **Azure Queue storage** kimeneti kötése, majd bontsa ki a **dokumentáció**.</span><span class="sxs-lookup"><span data-stu-id="30384-145">In your function, choose **Integrate** and hello new **Azure Queue storage** output binding, then expand **Documentation**.</span></span> <span data-ttu-id="30384-146">Másolja a **Fiók neve** és a **Fiók kulcsa** értéket.</span><span class="sxs-lookup"><span data-stu-id="30384-146">Copy both **Account name** and **Account key**.</span></span> <span data-ttu-id="30384-147">Ezen hitelesítő adatok tooconnect toohello tárfiókot használni.</span><span class="sxs-lookup"><span data-stu-id="30384-147">You use these credentials tooconnect toohello storage account.</span></span>
 
    ![Hello tárfiók kapcsolat hitelesítő adatainak lekéréséhez.](./media/functions-integrate-storage-queue-output-binding/function-get-storage-account-credentials.png)

2. <span data-ttu-id="30384-149">Hello futtatása [Microsoft Azure Tártallózó](http://storageexplorer.com/) eszközt, jelölje be hello hello bal oldali ikon csatlakozni, válassza a **használja a tárfiók nevét és a kulcs**, és válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="30384-149">Run hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/) tool, select hello connect icon on hello left, choose **Use a storage account name and key**, and select **Next**.</span></span>

    ![Hello fiók Tártallózó eszköz futtatásához.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-1.png)
    
3. <span data-ttu-id="30384-151">Beillesztés hello **fióknév** és **fiókkulcs** az 1. lépés a megfelelő mezőkbe, majd válasszon **következő**, és **Connect**.</span><span class="sxs-lookup"><span data-stu-id="30384-151">Paste hello **Account name** and **Account key** from step 1 into their corresponding fields, then select **Next**, and **Connect**.</span></span> 
  
    ![Illessze be a hello tároló hitelesítő adatait, és csatlakozzon.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-2.png)

4. <span data-ttu-id="30384-153">Bontsa ki a csatolt hello tárfiókot, **várólisták** , és ellenőrizze, hogy a várólista neve **Várólista_neve-elemek** létezik-e.</span><span class="sxs-lookup"><span data-stu-id="30384-153">Expand hello attached storage account, expand **Queues** and verify that a queue named **myqueue-items** exists.</span></span> <span data-ttu-id="30384-154">Emellett meg kell jelennie egy üzenet már hello várólista.</span><span class="sxs-lookup"><span data-stu-id="30384-154">You should also see a message already in hello queue.</span></span>  
 
    ![Hozzon létre egy tárolási üzenetsort.](./media/functions-integrate-storage-queue-output-binding/function-queue-storage-output-view-queue.png)
 

## <a name="clean-up-resources"></a><span data-ttu-id="30384-156">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="30384-156">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="30384-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="30384-157">Next steps</span></span>

<span data-ttu-id="30384-158">Egy kimeneti kötése tooan meglévő függvény hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="30384-158">You have added an output binding tooan existing function.</span></span> 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="30384-159">Kötési tooQueue tárolóira vonatkozó további információkért lásd: [Azure Functions tároló várólista kötések](functions-bindings-storage-queue.md).</span><span class="sxs-lookup"><span data-stu-id="30384-159">For more information about binding tooQueue storage, see [Azure Functions Storage queue bindings](functions-bindings-storage-queue.md).</span></span> 



