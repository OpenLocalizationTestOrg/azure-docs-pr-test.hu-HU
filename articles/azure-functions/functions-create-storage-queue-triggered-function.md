---
title: "Üzenetsorban lévő üzenetek által aktivált függvények létrehozása az Azure-ban | Microsoft Docs"
description: "Használja az Azure Functions szolgáltatást olyan kiszolgáló nélküli függvények létrehozására, amelyeket az Azure Storage üzenetsorába elküldött üzenetek hívnak meg."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 92a03154bf5a8945e2de9606afd138803c76fafe
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a><span data-ttu-id="6d9fe-103">Azure Storage-üzenetsor által aktivált függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d9fe-103">Create a function triggered by Azure Queue storage</span></span>

<span data-ttu-id="6d9fe-104">Ismerje meg, hogyan hozhat létre az Azure Storage üzenetsorába küldött üzenetek által aktivált függvényt.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-104">Learn how to create a function triggered when messages are submitted to an Azure Storage queue.</span></span>

![Tekintse meg a naplókban található üzeneteket.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="6d9fe-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6d9fe-106">Prerequisites</span></span>

- <span data-ttu-id="6d9fe-107">A [Microsoft Azure Storage Explorer](http://storageexplorer.com/) letöltése és telepítése.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-107">Download and install the [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

- <span data-ttu-id="6d9fe-108">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-108">An Azure subscription.</span></span> <span data-ttu-id="6d9fe-109">Ha még nincs előfizetése, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), mielőtt hozzákezd.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-109">If you don't have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="6d9fe-110">Azure-függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d9fe-110">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![A függvényalkalmazás elkészült.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="6d9fe-112">Ezután létrehozhat egy függvényt az új függvényalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-112">Next, you create a function in the new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a><span data-ttu-id="6d9fe-113">Üzenetsor által aktivált függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d9fe-113">Create a Queue triggered function</span></span>

1. <span data-ttu-id="6d9fe-114">Bontsa ki a függvényalkalmazást, és kattintson a **Függvények** elem melletti **+** gombra.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-114">Expand your function app and click the **+** button next to **Functions**.</span></span> <span data-ttu-id="6d9fe-115">Ha ez az első függvény a függvényalkalmazásban, jelölje ki az **Egyéni függvény** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-115">If this is the first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="6d9fe-116">Ez megjeleníti a függvénysablonok teljes készletét.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-116">This displays the complete set of function templates.</span></span>

    ![Függvények gyors létrehozásának oldala az Azure Portalon](./media/functions-create-storage-queue-triggered-function/add-first-function.png)

2. <span data-ttu-id="6d9fe-118">Kattintson a választott nyelvhez tartozó **QueueTrigger** sablonra, és használja a táblázatban megadott beállításokat.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-118">Select the **QueueTrigger** template for your desired language, and  use the settings as specified in the table.</span></span>

    ![Hozza létre a tároló üzenetsora által aktivált függvényt.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)
    
    | <span data-ttu-id="6d9fe-120">Beállítás</span><span class="sxs-lookup"><span data-stu-id="6d9fe-120">Setting</span></span> | <span data-ttu-id="6d9fe-121">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="6d9fe-121">Suggested value</span></span> | <span data-ttu-id="6d9fe-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="6d9fe-122">Description</span></span> |
    |---|---|---|
    | <span data-ttu-id="6d9fe-123">**Üzenetsor neve**</span><span class="sxs-lookup"><span data-stu-id="6d9fe-123">**Queue name**</span></span>   | <span data-ttu-id="6d9fe-124">myqueue-items</span><span class="sxs-lookup"><span data-stu-id="6d9fe-124">myqueue-items</span></span>    | <span data-ttu-id="6d9fe-125">A tárfiókhoz csatlakoztatni kívánt üzenetsor neve.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-125">Name of the queue to connect to in your Storage account.</span></span> |
    | <span data-ttu-id="6d9fe-126">**Tárfiók kapcsolata**</span><span class="sxs-lookup"><span data-stu-id="6d9fe-126">**Storage account connection**</span></span> | <span data-ttu-id="6d9fe-127">AzureWebJobStorage</span><span class="sxs-lookup"><span data-stu-id="6d9fe-127">AzureWebJobStorage</span></span> | <span data-ttu-id="6d9fe-128">Választhatja a függvényalkalmazás által már használt tárfiókkapcsolatot, vagy létrehozhat egy újat.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-128">You can use the storage account connection already being used by your function app, or create a new one.</span></span>  |
    | <span data-ttu-id="6d9fe-129">**A függvény neve**</span><span class="sxs-lookup"><span data-stu-id="6d9fe-129">**Name your function**</span></span> | <span data-ttu-id="6d9fe-130">Egyedi a függvényalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="6d9fe-130">Unique in your function app</span></span> | <span data-ttu-id="6d9fe-131">Az üzenetsor által aktivált függvény neve.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-131">Name of this queue triggered function.</span></span> |

3. <span data-ttu-id="6d9fe-132">Kattintson a **Létrehozás** elemre a függvény létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-132">Click **Create** to create your function.</span></span>

<span data-ttu-id="6d9fe-133">Ezután csatlakozzon az Azure Storage-fiókjához, és hozza létre a **myqueue-items** tárolót.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-133">Next, you connect to your Azure Storage account and create the **myqueue-items** storage queue.</span></span>

## <a name="create-the-queue"></a><span data-ttu-id="6d9fe-134">Az üzenetsor létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d9fe-134">Create the queue</span></span>

1. <span data-ttu-id="6d9fe-135">A függvényben kattintson az **Integráció** elemre, bontsa ki a **Dokumentáció** elemet, és másolja a **Fiók neve** és a **Fiók kulcsa** értéket.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-135">In your function, click **Integrate**, expand **Documentation**, and copy both **Account name** and **Account key**.</span></span> <span data-ttu-id="6d9fe-136">Ezekkel a hitelesítő adatokkal csatlakozhat a tárfiókhoz.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-136">You use these credentials to connect to the storage account.</span></span> <span data-ttu-id="6d9fe-137">Ha már csatlakozott a tárfiókjához, folytassa a 4. lépéssel.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-137">If you have already connected your storage account, skip to step 4.</span></span>

    ![Kérje le a tárfiókhoz való csatlakozáshoz szükséges hitelesítő adatokat.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)<span data-ttu-id="6d9fe-139">v</span><span class="sxs-lookup"><span data-stu-id="6d9fe-139">v</span></span>

1. <span data-ttu-id="6d9fe-140">Futtassa a [Microsoft Azure Storage Explorer](http://storageexplorer.com/) eszközt, kattintson a bal oldalon található csatlakozási ikonra, válassza a **Tárfiók nevének és kulcsának használata** lehetőséget, és kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-140">Run the [Microsoft Azure Storage Explorer](http://storageexplorer.com/) tool, click the connect icon on the left, choose **Use a storage account name and key**, and click **Next**.</span></span>

    ![Futtassa a Storage Account Explorer eszközt.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. <span data-ttu-id="6d9fe-142">Adja meg az 1. lépésben megállapított **Fiók neve** és **Fiók kulcsa** értéket, és kattintson a **Tovább**, majd a **Csatlakozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-142">Enter the **Account name** and **Account key** from step 1, click **Next** and then **Connect**.</span></span>

    ![Adja meg a tároló hitelesítő adatait, és csatlakozzon.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. <span data-ttu-id="6d9fe-144">Bontsa ki a csatolt tárfiókot, kattintson a jobb gombbal az **Üzenetsorok** elemre, kattintson az **Üzenetsor létrehozása** elemre, írja be a `myqueue-items` értéket, és nyomja meg az Enter billentyűt.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-144">Expand the attached storage account, right-click **Queues**, click **Create queue**, type `myqueue-items`, and then press enter.</span></span>

    ![Hozzon létre egy tárolási üzenetsort.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

<span data-ttu-id="6d9fe-146">Az üzenetsor létrehozása után tesztelheti a függvényt úgy, hogy felvesz egy üzenetet az üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-146">Now that you have a storage queue, you can test the function by adding a message to the queue.</span></span>

## <a name="test-the-function"></a><span data-ttu-id="6d9fe-147">A függvény tesztelése</span><span class="sxs-lookup"><span data-stu-id="6d9fe-147">Test the function</span></span>

1. <span data-ttu-id="6d9fe-148">Térjen vissza az Azure Portalra, keresse meg a függvényt, bontsa ki a **Naplók** elemet a lap alján, és győződjön meg arról, hogy a naplózási adatfolyam nincs leállítva.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-148">Back in the Azure portal, browse to your function expand the **Logs** at the bottom of the page and make sure that log streaming isn't paused.</span></span>

1. <span data-ttu-id="6d9fe-149">A Storage Explorerben bontsa ki a tárfiókot, az **Üzenetsorok**, majd az **üzenetsorelemek** elemet, és kattintson az **Üzenet hozzáadása** elemre.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-149">In Storage Explorer, expand your storage account, **Queues**, and **myqueue-items**, then click **Add message**.</span></span>

    ![Vegyen fel egy üzenetet az üzenetsorba.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. <span data-ttu-id="6d9fe-151">Írja be a „Hello World!”</span><span class="sxs-lookup"><span data-stu-id="6d9fe-151">Type your "Hello World!"</span></span> <span data-ttu-id="6d9fe-152">üzenetet az **Üzenet szövege** mezőbe, és kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-152">message in **Message text** and click **OK**.</span></span>

1. <span data-ttu-id="6d9fe-153">Várjon néhány másodpercet, majd térjen vissza a függvény naplóihoz, és győződjön meg arról, hogy megtörtént az új üzenet olvasása az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-153">Wait for a few seconds, then go back to your function logs and verify that the new message has been read from the queue.</span></span>

    ![Tekintse meg a naplókban található üzeneteket.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. <span data-ttu-id="6d9fe-155">Térjen vissza a Storage Explorerbe, kattintson a **Frissítés** elemre, és ellenőrizze, hogy megtörtént-e az üzenet feldolgozása, és hogy el lett-e távolítva az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-155">Back in Storage Explorer, click **Refresh** and verify that the message has been processed and is no longer in the queue.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="6d9fe-156">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6d9fe-156">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="6d9fe-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6d9fe-157">Next steps</span></span>

<span data-ttu-id="6d9fe-158">Létrehozott egy függvényt, amely akkor fut, amikor üzenet felvétele történik a tárolási üzenetsorba.</span><span class="sxs-lookup"><span data-stu-id="6d9fe-158">You have created a function that runs when a message is added to a storage queue.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="6d9fe-159">További információ a tárolási üzenetsor eseményindítóiról: [Azure Functions – a tárolási üzenetsor kötései](functions-bindings-storage-queue.md).</span><span class="sxs-lookup"><span data-stu-id="6d9fe-159">For more information about Queue storage triggers, see [Azure Functions Storage queue bindings](functions-bindings-storage-queue.md).</span></span>