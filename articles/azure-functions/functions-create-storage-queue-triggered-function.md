---
title: "az Azure üzenetsor-üzeneteket által indított függvény aaaCreate |} Microsoft Docs"
description: "Használja az Azure Functions toocreate egy kiszolgáló nélküli függvény, amelyet egy üzenetek által benyújtott tooan Azure Storage üzenetsorába."
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
ms.openlocfilehash: e9501ed336b502eaeee3fa62ec4ae085c76de0ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a><span data-ttu-id="77468-103">Azure Storage-üzenetsor által aktivált függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="77468-103">Create a function triggered by Azure Queue storage</span></span>

<span data-ttu-id="77468-104">Ismerje meg, hogyan toocreate üzenetek által elindított függvény benyújtott tooan Azure Storage üzenetsorába.</span><span class="sxs-lookup"><span data-stu-id="77468-104">Learn how toocreate a function triggered when messages are submitted tooan Azure Storage queue.</span></span>

![Hello naplókban üzenet megtekintése.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="77468-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="77468-106">Prerequisites</span></span>

- <span data-ttu-id="77468-107">Töltse le és telepítse a hello [Microsoft Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="77468-107">Download and install hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

- <span data-ttu-id="77468-108">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="77468-108">An Azure subscription.</span></span> <span data-ttu-id="77468-109">Ha még nincs előfizetése, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), mielőtt hozzákezd.</span><span class="sxs-lookup"><span data-stu-id="77468-109">If you don't have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="77468-110">Azure-függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="77468-110">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![A függvényalkalmazás elkészült.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="77468-112">A függvény a következő alkalmazásban hello új függvény létrehozása.</span><span class="sxs-lookup"><span data-stu-id="77468-112">Next, you create a function in hello new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a><span data-ttu-id="77468-113">Üzenetsor által aktivált függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="77468-113">Create a Queue triggered function</span></span>

1. <span data-ttu-id="77468-114">Bontsa ki a függvény alkalmazást, majd kattintson a hello  **+**  gomb melletti túl**funkciók**.</span><span class="sxs-lookup"><span data-stu-id="77468-114">Expand your function app and click hello **+** button next too**Functions**.</span></span> <span data-ttu-id="77468-115">Ha ez az első függvényét hello az függvény alkalmazásban, válassza ki a **egyéni függvény**.</span><span class="sxs-lookup"><span data-stu-id="77468-115">If this is hello first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="77468-116">Ez a függvény sablonok teljes készletének hello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="77468-116">This displays hello complete set of function templates.</span></span>

    ![Gyors üzembe helyezés lap funkciók hello Azure-portálon](./media/functions-create-storage-queue-triggered-function/add-first-function.png)

2. <span data-ttu-id="77468-118">Jelölje be hello **QueueTrigger** sablont a kívánt nyelvet, és a hello tábla hello-beállítások használata.</span><span class="sxs-lookup"><span data-stu-id="77468-118">Select hello **QueueTrigger** template for your desired language, and  use hello settings as specified in hello table.</span></span>

    ![Hello tárolási indított várólista-függvény létrehozása.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)
    
    | <span data-ttu-id="77468-120">Beállítás</span><span class="sxs-lookup"><span data-stu-id="77468-120">Setting</span></span> | <span data-ttu-id="77468-121">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="77468-121">Suggested value</span></span> | <span data-ttu-id="77468-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="77468-122">Description</span></span> |
    |---|---|---|
    | <span data-ttu-id="77468-123">**Üzenetsor neve**</span><span class="sxs-lookup"><span data-stu-id="77468-123">**Queue name**</span></span>   | <span data-ttu-id="77468-124">myqueue-items</span><span class="sxs-lookup"><span data-stu-id="77468-124">myqueue-items</span></span>    | <span data-ttu-id="77468-125">Hello neve várólista tooconnect tooin a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="77468-125">Name of hello queue tooconnect tooin your Storage account.</span></span> |
    | <span data-ttu-id="77468-126">**Tárfiók kapcsolata**</span><span class="sxs-lookup"><span data-stu-id="77468-126">**Storage account connection**</span></span> | <span data-ttu-id="77468-127">AzureWebJobStorage</span><span class="sxs-lookup"><span data-stu-id="77468-127">AzureWebJobStorage</span></span> | <span data-ttu-id="77468-128">A függvény alkalmazás által már használt hello tárolási fiók kapcsolat használatát, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="77468-128">You can use hello storage account connection already being used by your function app, or create a new one.</span></span>  |
    | <span data-ttu-id="77468-129">**A függvény neve**</span><span class="sxs-lookup"><span data-stu-id="77468-129">**Name your function**</span></span> | <span data-ttu-id="77468-130">Egyedi a függvényalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="77468-130">Unique in your function app</span></span> | <span data-ttu-id="77468-131">Az üzenetsor által aktivált függvény neve.</span><span class="sxs-lookup"><span data-stu-id="77468-131">Name of this queue triggered function.</span></span> |

3. <span data-ttu-id="77468-132">Kattintson a **létrehozása** toocreate a függvény.</span><span class="sxs-lookup"><span data-stu-id="77468-132">Click **Create** toocreate your function.</span></span>

<span data-ttu-id="77468-133">A következő tooyour Azure Storage-fiók csatlakozzon, és hozzon létre hello **Várólista_neve-elemek** tároló várólista.</span><span class="sxs-lookup"><span data-stu-id="77468-133">Next, you connect tooyour Azure Storage account and create hello **myqueue-items** storage queue.</span></span>

## <a name="create-hello-queue"></a><span data-ttu-id="77468-134">Hello várólista létrehozása</span><span class="sxs-lookup"><span data-stu-id="77468-134">Create hello queue</span></span>

1. <span data-ttu-id="77468-135">A függvényben kattintson az **Integráció** elemre, bontsa ki a **Dokumentáció** elemet, és másolja a **Fiók neve** és a **Fiók kulcsa** értéket.</span><span class="sxs-lookup"><span data-stu-id="77468-135">In your function, click **Integrate**, expand **Documentation**, and copy both **Account name** and **Account key**.</span></span> <span data-ttu-id="77468-136">Ezen hitelesítő adatok tooconnect toohello tárfiókot használni.</span><span class="sxs-lookup"><span data-stu-id="77468-136">You use these credentials tooconnect toohello storage account.</span></span> <span data-ttu-id="77468-137">Ha a tárfiók már csatlakozott, hagyja ki a toostep 4.</span><span class="sxs-lookup"><span data-stu-id="77468-137">If you have already connected your storage account, skip toostep 4.</span></span>

    ![Hello tárfiók kapcsolat hitelesítő adatainak lekéréséhez.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)<span data-ttu-id="77468-139">v</span><span class="sxs-lookup"><span data-stu-id="77468-139">v</span></span>

1. <span data-ttu-id="77468-140">Hello futtatása [Microsoft Azure Tártallózó](http://storageexplorer.com/) eszköz, kattintson a hello hello bal oldali ikon csatlakozni, válassza a **használja a tárfiók nevét és a kulcs**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="77468-140">Run hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/) tool, click hello connect icon on hello left, choose **Use a storage account name and key**, and click **Next**.</span></span>

    ![Hello fiók Tártallózó eszköz futtatásához.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. <span data-ttu-id="77468-142">Adja meg a hello **fióknév** és **fiókkulcs** 1. lépésben kattintson **következő** , majd **Connect**.</span><span class="sxs-lookup"><span data-stu-id="77468-142">Enter hello **Account name** and **Account key** from step 1, click **Next** and then **Connect**.</span></span>

    ![Adja meg hello tároló hitelesítő adatait, és csatlakozzon.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. <span data-ttu-id="77468-144">Bontsa ki a csatolt hello storage-fiókot, kattintson a jobb gombbal **várólisták**, kattintson a **létrehozás várólista**, típus `myqueue-items`, és nyomja meg az enter.</span><span class="sxs-lookup"><span data-stu-id="77468-144">Expand hello attached storage account, right-click **Queues**, click **Create queue**, type `myqueue-items`, and then press enter.</span></span>

    ![Hozzon létre egy tárolási üzenetsort.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

<span data-ttu-id="77468-146">Most, hogy a tároló várólista, tesztelheti hello függvény toohello üzenet-várólista hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="77468-146">Now that you have a storage queue, you can test hello function by adding a message toohello queue.</span></span>

## <a name="test-hello-function"></a><span data-ttu-id="77468-147">Hello függvény tesztelése</span><span class="sxs-lookup"><span data-stu-id="77468-147">Test hello function</span></span>

1. <span data-ttu-id="77468-148">Vissza a hello Azure-portálon, a Tallózás tooyour függvény bontsa ki a hello **naplók** hello alján lévő hello lap, és győződjön meg arról, hogy a napló streaming nem szünetel.</span><span class="sxs-lookup"><span data-stu-id="77468-148">Back in hello Azure portal, browse tooyour function expand hello **Logs** at hello bottom of hello page and make sure that log streaming isn't paused.</span></span>

1. <span data-ttu-id="77468-149">A Storage Explorerben bontsa ki a tárfiókot, az **Üzenetsorok**, majd az **üzenetsorelemek** elemet, és kattintson az **Üzenet hozzáadása** elemre.</span><span class="sxs-lookup"><span data-stu-id="77468-149">In Storage Explorer, expand your storage account, **Queues**, and **myqueue-items**, then click **Add message**.</span></span>

    ![Adjon hozzá egy üzenetsor toohello.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. <span data-ttu-id="77468-151">Írja be a „Hello World!”</span><span class="sxs-lookup"><span data-stu-id="77468-151">Type your "Hello World!"</span></span> <span data-ttu-id="77468-152">üzenetet az **Üzenet szövege** mezőbe, és kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="77468-152">message in **Message text** and click **OK**.</span></span>

1. <span data-ttu-id="77468-153">Várjon néhány másodpercet, majd lépjen vissza tooyour függvény naplókat, és győződjön meg arról, hogy új üdvözlőüzenetére kiolvasott hello várólista.</span><span class="sxs-lookup"><span data-stu-id="77468-153">Wait for a few seconds, then go back tooyour function logs and verify that hello new message has been read from hello queue.</span></span>

    ![Hello naplókban üzenet megtekintése.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. <span data-ttu-id="77468-155">Tártallózó, kattintson **frissítése** , és győződjön meg arról, hogy üdvözlőüzenetére feldolgozása megtörtént, és már nem hello várólista.</span><span class="sxs-lookup"><span data-stu-id="77468-155">Back in Storage Explorer, click **Refresh** and verify that hello message has been processed and is no longer in hello queue.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="77468-156">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="77468-156">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="77468-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="77468-157">Next steps</span></span>

<span data-ttu-id="77468-158">Létrehozott egy függvényt, amely egy üzenetet tooa storage üzenetsorába felvételekor futnak.</span><span class="sxs-lookup"><span data-stu-id="77468-158">You have created a function that runs when a message is added tooa storage queue.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="77468-159">További információ a tárolási üzenetsor eseményindítóiról: [Azure Functions – a tárolási üzenetsor kötései](functions-bindings-storage-queue.md).</span><span class="sxs-lookup"><span data-stu-id="77468-159">For more information about Queue storage triggers, see [Azure Functions Storage queue bindings](functions-bindings-storage-queue.md).</span></span>