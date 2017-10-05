---
title: "Üzenetsorban lévő üzenetek által aktivált függvények létrehozása az Azure-ban | Microsoft Docs"
description: "Használja az Azure Functions szolgáltatást olyan kiszolgáló nélküli függvények létrehozására, amelyeket az Azure Storage üzenetsorába elküldött üzenetek hívnak meg."
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
ms.openlocfilehash: 57c59273a9da55f3e357764c522b444ae2d73cb5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="add-messages-to-an-azure-storage-queue-using-functions"></a><span data-ttu-id="72757-103">Üzenetek hozzáadása az Azure Storage üzenetsorába a Functions szolgáltatás használatával</span><span class="sxs-lookup"><span data-stu-id="72757-103">Add messages to an Azure Storage queue using Functions</span></span>

<span data-ttu-id="72757-104">Az Azure Functions bemeneti és kimeneti kötései deklaratív módszert biztosítanak a külső szolgáltatások adataihoz a függvényből történő csatlakozásra.</span><span class="sxs-lookup"><span data-stu-id="72757-104">In Azure Functions, input and output bindings provide a declarative way to connect to external service data from your function.</span></span> <span data-ttu-id="72757-105">Ebben a témakörben megtudhatja, hogyan módosíthat egy meglévő függvényt egy kimeneti kötés hozzáadásával, amely üzeneteket küld az Azure üzenetsor-tárolójába.</span><span class="sxs-lookup"><span data-stu-id="72757-105">In this topic, learn how to update an existing function by adding an output binding that sends messages to Azure Queue storage.</span></span>  

![Tekintse meg a naplókban található üzeneteket.](./media/functions-integrate-storage-queue-output-binding/functions-integrate-storage-binding-in-portal.png)

## <a name="prerequisites"></a><span data-ttu-id="72757-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="72757-107">Prerequisites</span></span> 

[!INCLUDE [Previous topics](../../includes/functions-quickstart-previous-topics.md)]

* <span data-ttu-id="72757-108">Telepítse a [Microsoft Azure Storage Explorert](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="72757-108">Install the [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

## <span data-ttu-id="72757-109"><a name="add-binding"></a>Kimeneti kötés hozzáadása</span><span class="sxs-lookup"><span data-stu-id="72757-109"><a name="add-binding"></a>Add an output binding</span></span>
 
1. <span data-ttu-id="72757-110">Bontsa ki a függvényalkalmazást és a függvényt.</span><span class="sxs-lookup"><span data-stu-id="72757-110">Expand both your function app and your function.</span></span>

2. <span data-ttu-id="72757-111">Válassza ki **integráció** és **+ új kimeneti**, majd válassza **Azure Queue storage** válassza **kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="72757-111">Select **Integrate** and **+ New output**, then choose **Azure Queue storage** and choose **Select**.</span></span>
    
    ![Vegye fel egy üzenetsor-tároló kimeneti kötését egy függvénybe az Azure Portalon.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding.png)

3. <span data-ttu-id="72757-113">Használja a táblázatban megadott beállításokat:</span><span class="sxs-lookup"><span data-stu-id="72757-113">Use the settings as specified in the table:</span></span> 

    ![Vegye fel egy üzenetsor-tároló kimeneti kötését egy függvénybe az Azure Portalon.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding-2.png)

    | <span data-ttu-id="72757-115">Beállítás</span><span class="sxs-lookup"><span data-stu-id="72757-115">Setting</span></span>      |  <span data-ttu-id="72757-116">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="72757-116">Suggested value</span></span>   | <span data-ttu-id="72757-117">Leírás</span><span class="sxs-lookup"><span data-stu-id="72757-117">Description</span></span>                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | <span data-ttu-id="72757-118">**Üzenetsor neve**</span><span class="sxs-lookup"><span data-stu-id="72757-118">**Queue name**</span></span>   | <span data-ttu-id="72757-119">myqueue-items</span><span class="sxs-lookup"><span data-stu-id="72757-119">myqueue-items</span></span>    | <span data-ttu-id="72757-120">A tárfiókhoz csatlakoztatni kívánt üzenetsor neve.</span><span class="sxs-lookup"><span data-stu-id="72757-120">The name of the queue to connect to in your Storage account.</span></span> |
    | <span data-ttu-id="72757-121">**Tárfiók kapcsolata**</span><span class="sxs-lookup"><span data-stu-id="72757-121">**Storage account connection**</span></span> | <span data-ttu-id="72757-122">AzureWebJobStorage</span><span class="sxs-lookup"><span data-stu-id="72757-122">AzureWebJobStorage</span></span> | <span data-ttu-id="72757-123">Választhatja a függvényalkalmazás által már használt tárfiókkapcsolatot, vagy létrehozhat egy újat.</span><span class="sxs-lookup"><span data-stu-id="72757-123">You can use the storage account connection already being used by your function app, or create a new one.</span></span>  |
    | <span data-ttu-id="72757-124">**Üzenet-paraméter neve**</span><span class="sxs-lookup"><span data-stu-id="72757-124">**Message parameter name**</span></span> | <span data-ttu-id="72757-125">outputQueueItem</span><span class="sxs-lookup"><span data-stu-id="72757-125">outputQueueItem</span></span> | <span data-ttu-id="72757-126">A kimeneti kötés paraméterének neve.</span><span class="sxs-lookup"><span data-stu-id="72757-126">The name of the output binding parameter.</span></span> | 

4. <span data-ttu-id="72757-127">Kattintson a **Mentés** gombra a kötés felvételéhez.</span><span class="sxs-lookup"><span data-stu-id="72757-127">Click **Save** to add the binding.</span></span>
 
<span data-ttu-id="72757-128">Miután meghatározta a kimeneti kötést, módosítania kell a kódot, hogy az a kötés használatával üzeneteket adjon hozzá az üzenetsorhoz.</span><span class="sxs-lookup"><span data-stu-id="72757-128">Now that you have an output binding defined, you need to update the code to use the binding to add messages to a queue.</span></span>  

## <a name="update-the-function-code"></a><span data-ttu-id="72757-129">A függvénykód módosítása</span><span class="sxs-lookup"><span data-stu-id="72757-129">Update the function code</span></span>

1. <span data-ttu-id="72757-130">A függvényre kattintva jelenítse meg a szerkesztőben a függvénykódot.</span><span class="sxs-lookup"><span data-stu-id="72757-130">Select your function to display the function code in the editor.</span></span> 

2. <span data-ttu-id="72757-131">C# függvény, frissítse a függvény definícióját a következőképpen vegye fel a **outputQueueItem** tárolási kötési paraméter.</span><span class="sxs-lookup"><span data-stu-id="72757-131">For a C# function, update your function definition as follows to add the **outputQueueItem** storage binding parameter.</span></span> <span data-ttu-id="72757-132">JavaScript-függvény esetében hagyja ki ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="72757-132">Skip this step for a JavaScript function.</span></span>

    ```cs   
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, 
        ICollector<string> outputQueueItem, TraceWriter log)
    {
        ....
    }
    ```

3. <span data-ttu-id="72757-133">Adja hozzá a következő kódot a függvényhez a metódus visszatérése előtt.</span><span class="sxs-lookup"><span data-stu-id="72757-133">Add the following code to the function just before the method returns.</span></span> <span data-ttu-id="72757-134">A függvény nyelvének megfelelő kódrészletet használjon.</span><span class="sxs-lookup"><span data-stu-id="72757-134">Use the appropriate snippet for the language of your function.</span></span>

    ```javascript
    context.bindings.outputQueueItem = "Name passed to the function: " + 
                (req.query.name || req.body.name);
    ```

    ```cs
    outputQueueItem.Add("Name passed to the function: " + name);     
    ```

4. <span data-ttu-id="72757-135">A módosítások mentéséhez kattintson a **Mentés** elemre.</span><span class="sxs-lookup"><span data-stu-id="72757-135">Select **Save** to save changes.</span></span>

<span data-ttu-id="72757-136">A HTTP-eseményindítónak üzenetben átadott érték bekerül az üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="72757-136">The value passed to the HTTP trigger is included in a message added to the queue.</span></span>
 
## <a name="test-the-function"></a><span data-ttu-id="72757-137">A függvény tesztelése</span><span class="sxs-lookup"><span data-stu-id="72757-137">Test the function</span></span> 

1. <span data-ttu-id="72757-138">A kód módosításainak mentése után kattintson a **Futtatás** elemre.</span><span class="sxs-lookup"><span data-stu-id="72757-138">After the code changes are saved, select **Run**.</span></span> 

    ![Vegye fel egy üzenetsor-tároló kimeneti kötését egy függvénybe az Azure Portalon.](./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png)

2. <span data-ttu-id="72757-140">A naplók ellenőrzésével győződjön meg arról, hogy sikeres volt a függvény futtatása.</span><span class="sxs-lookup"><span data-stu-id="72757-140">Check the logs to make sure that the function succeeded.</span></span> <span data-ttu-id="72757-141">A Functions futtatókörnyezete egy **outqueue** nevű új üzenetsort hoz létre a tárfiókjában a kimeneti kötés első használatakor.</span><span class="sxs-lookup"><span data-stu-id="72757-141">A new queue named **outqueue** is created in your Storage account by the Functions runtime when the output binding is first used.</span></span>

<span data-ttu-id="72757-142">Ezután csatlakozhat a tárfiókhoz, hogy ellenőrizze az új üzenetsort és az abba felvett üzenetet.</span><span class="sxs-lookup"><span data-stu-id="72757-142">Next, you can connect to your storage account to verify the new queue and the message you added to it.</span></span> 

## <a name="connect-to-the-queue"></a><span data-ttu-id="72757-143">Csatlakozás az üzenetsorhoz</span><span class="sxs-lookup"><span data-stu-id="72757-143">Connect to the queue</span></span>

<span data-ttu-id="72757-144">Hagyja ki az első három lépést, ha már telepítette a Storage Explorert, és már csatlakoztatta azt a tárfiókjához.</span><span class="sxs-lookup"><span data-stu-id="72757-144">Skip the first three steps if you have already installed Storage Explorer and connected it to your storage account.</span></span>    

1. <span data-ttu-id="72757-145">Válassza ki a függvény **integráció** és az új **Azure Queue storage** kimeneti kötése, majd bontsa ki a **dokumentáció**.</span><span class="sxs-lookup"><span data-stu-id="72757-145">In your function, choose **Integrate** and the new **Azure Queue storage** output binding, then expand **Documentation**.</span></span> <span data-ttu-id="72757-146">Másolja a **Fiók neve** és a **Fiók kulcsa** értéket.</span><span class="sxs-lookup"><span data-stu-id="72757-146">Copy both **Account name** and **Account key**.</span></span> <span data-ttu-id="72757-147">Ezekkel a hitelesítő adatokkal csatlakozhat a tárfiókhoz.</span><span class="sxs-lookup"><span data-stu-id="72757-147">You use these credentials to connect to the storage account.</span></span>
 
    ![Kérje le a tárfiókhoz való csatlakozáshoz szükséges hitelesítő adatokat.](./media/functions-integrate-storage-queue-output-binding/function-get-storage-account-credentials.png)

2. <span data-ttu-id="72757-149">Futtassa a [Microsoft Azure Storage Explorer](http://storageexplorer.com/) eszközt, kattintson a bal oldalon található csatlakozási ikonra, válassza ki a **Tárfiók nevének és kulcsának használata** lehetőséget, és kattintson a **Tovább** elemre.</span><span class="sxs-lookup"><span data-stu-id="72757-149">Run the [Microsoft Azure Storage Explorer](http://storageexplorer.com/) tool, select the connect icon on the left, choose **Use a storage account name and key**, and select **Next**.</span></span>

    ![Futtassa a Storage Account Explorer eszközt.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-1.png)
    
3. <span data-ttu-id="72757-151">Illessze be az első lépésből a **Fiók neve** és a **Fiók kulcsa** értéket a megfelelő mezőkbe, majd kattintson a **Tovább**, és a **Csatlakozás** elemre.</span><span class="sxs-lookup"><span data-stu-id="72757-151">Paste the **Account name** and **Account key** from step 1 into their corresponding fields, then select **Next**, and **Connect**.</span></span> 
  
    ![Illessze be a tároló hitelesítő adatait, és csatlakozzon.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-2.png)

4. <span data-ttu-id="72757-153">Bontsa ki a csatolt tárfiókot, **várólisták** , és ellenőrizze, hogy a várólista neve **Várólista_neve-elemek** létezik-e.</span><span class="sxs-lookup"><span data-stu-id="72757-153">Expand the attached storage account, expand **Queues** and verify that a queue named **myqueue-items** exists.</span></span> <span data-ttu-id="72757-154">Ezenkívül egy üzenetnek is kell szerepelnie már az üzenetsorban.</span><span class="sxs-lookup"><span data-stu-id="72757-154">You should also see a message already in the queue.</span></span>  
 
    ![Hozzon létre egy tárolási üzenetsort.](./media/functions-integrate-storage-queue-output-binding/function-queue-storage-output-view-queue.png)
 

## <a name="clean-up-resources"></a><span data-ttu-id="72757-156">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="72757-156">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="72757-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="72757-157">Next steps</span></span>

<span data-ttu-id="72757-158">Felvett egy kimeneti kötést egy meglévő függvénybe.</span><span class="sxs-lookup"><span data-stu-id="72757-158">You have added an output binding to an existing function.</span></span> 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="72757-159">További információ a tárolási üzenetsor kötéséről: [Azure Functions – a tárolási üzenetsor kötései](functions-bindings-storage-queue.md).</span><span class="sxs-lookup"><span data-stu-id="72757-159">For more information about binding to Queue storage, see [Azure Functions Storage queue bindings](functions-bindings-storage-queue.md).</span></span> 



