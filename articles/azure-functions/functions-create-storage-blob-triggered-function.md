---
title: "egy Azure Blob storage által indított függvényt aaaCreate |} Microsoft Docs"
description: "Használja az Azure Functions toocreate egy kiszolgáló nélküli függvény, amelyet elemének hozzáadott tooAzure Blob Storage tárolóban."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: d6bff41c-a624-40c1-bbc7-80590df29ded
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: acb7d29abb07a22da11d0e65d2ed54591f8e3f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-azure-blob-storage"></a><span data-ttu-id="73b90-103">Azure Blob-tároló által aktivált függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="73b90-103">Create a function triggered by Azure Blob storage</span></span>

<span data-ttu-id="73b90-104">Ismerje meg, hogyan toocreate váltódik ki, amelyek fájljai egy olyan függvényt feltöltött tooor frissítése az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="73b90-104">Learn how toocreate a function triggered when files are uploaded tooor updated in Azure Blob storage.</span></span>

![Hello naplókban üzenet megtekintése.](./media/functions-create-storage-blob-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="73b90-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="73b90-106">Prerequisites</span></span>

+ <span data-ttu-id="73b90-107">Töltse le és telepítse a hello [Microsoft Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="73b90-107">Download and install hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>
+ <span data-ttu-id="73b90-108">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="73b90-108">An Azure subscription.</span></span> <span data-ttu-id="73b90-109">Ha még nincs előfizetése, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), mielőtt hozzákezd.</span><span class="sxs-lookup"><span data-stu-id="73b90-109">If you don't have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="73b90-110">Azure-függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="73b90-110">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![A függvényalkalmazás elkészült.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="73b90-112">A függvény a következő alkalmazásban hello új függvény létrehozása.</span><span class="sxs-lookup"><span data-stu-id="73b90-112">Next, you create a function in hello new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-blob-storage-triggered-function"></a><span data-ttu-id="73b90-113">A blobtároló által aktivált függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="73b90-113">Create a Blob storage triggered function</span></span>

1. <span data-ttu-id="73b90-114">Bontsa ki a függvény alkalmazást, majd kattintson a hello  **+**  gomb melletti túl**funkciók**.</span><span class="sxs-lookup"><span data-stu-id="73b90-114">Expand your function app and click hello **+** button next too**Functions**.</span></span> <span data-ttu-id="73b90-115">Ha ez az első függvényét hello az függvény alkalmazásban, válassza ki a **egyéni függvény**.</span><span class="sxs-lookup"><span data-stu-id="73b90-115">If this is hello first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="73b90-116">Ez a függvény sablonok teljes készletének hello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="73b90-116">This displays hello complete set of function templates.</span></span>

    ![Gyors üzembe helyezés lap funkciók hello Azure-portálon](./media/functions-create-storage-blob-triggered-function/add-first-function.png)

2. <span data-ttu-id="73b90-118">Jelölje be hello **BlobTrigger** sablont a kívánt nyelvet, és a hello tábla hello-beállítások használata.</span><span class="sxs-lookup"><span data-stu-id="73b90-118">Select hello **BlobTrigger** template for your desired language, and use hello settings as specified in hello table.</span></span>

    ![Hello Blob storage indított függvény létrehozása.](./media/functions-create-storage-blob-triggered-function/functions-create-blob-storage-trigger-portal.png)

    | <span data-ttu-id="73b90-120">Beállítás</span><span class="sxs-lookup"><span data-stu-id="73b90-120">Setting</span></span> | <span data-ttu-id="73b90-121">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="73b90-121">Suggested value</span></span> | <span data-ttu-id="73b90-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="73b90-122">Description</span></span> |
    |---|---|---|
    | <span data-ttu-id="73b90-123">**Elérési út**</span><span class="sxs-lookup"><span data-stu-id="73b90-123">**Path**</span></span>   | <span data-ttu-id="73b90-124">mycontainer/{name}</span><span class="sxs-lookup"><span data-stu-id="73b90-124">mycontainer/{name}</span></span>    | <span data-ttu-id="73b90-125">A figyelt blobtárolóban található hely.</span><span class="sxs-lookup"><span data-stu-id="73b90-125">Location in Blob storage being monitored.</span></span> <span data-ttu-id="73b90-126">hello blob fájlnevét hello átadása hello kötésben szerint hello _neve_ paraméter.</span><span class="sxs-lookup"><span data-stu-id="73b90-126">hello file name of hello blob is passed in hello binding as hello _name_ parameter.</span></span>  |
    | <span data-ttu-id="73b90-127">**Tárfiók kapcsolata**</span><span class="sxs-lookup"><span data-stu-id="73b90-127">**Storage account connection**</span></span> | <span data-ttu-id="73b90-128">AzureWebJobStorage</span><span class="sxs-lookup"><span data-stu-id="73b90-128">AzureWebJobStorage</span></span> | <span data-ttu-id="73b90-129">A függvény alkalmazás által már használt hello tárolási fiók kapcsolat használatát, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="73b90-129">You can use hello storage account connection already being used by your function app, or create a new one.</span></span>  |
    | <span data-ttu-id="73b90-130">**A függvény neve**</span><span class="sxs-lookup"><span data-stu-id="73b90-130">**Name your function**</span></span> | <span data-ttu-id="73b90-131">Egyedi a függvényalkalmazásban</span><span class="sxs-lookup"><span data-stu-id="73b90-131">Unique in your function app</span></span> | <span data-ttu-id="73b90-132">A blob által aktivált függvény neve.</span><span class="sxs-lookup"><span data-stu-id="73b90-132">Name of this blob triggered function.</span></span> |

3. <span data-ttu-id="73b90-133">Kattintson a **létrehozása** toocreate a függvény.</span><span class="sxs-lookup"><span data-stu-id="73b90-133">Click **Create** toocreate your function.</span></span>

<span data-ttu-id="73b90-134">A következő tooyour Azure Storage-fiók csatlakozzon, és hozzon létre hello **mycontainer** tároló.</span><span class="sxs-lookup"><span data-stu-id="73b90-134">Next, you connect tooyour Azure Storage account and create hello **mycontainer** container.</span></span>

## <a name="create-hello-container"></a><span data-ttu-id="73b90-135">Hello tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="73b90-135">Create hello container</span></span>

1. <span data-ttu-id="73b90-136">A függvényben kattintson az **Integráció** elemre, bontsa ki a **Dokumentáció** elemet, és másolja a **Fiók neve** és a **Fiók kulcsa** értéket.</span><span class="sxs-lookup"><span data-stu-id="73b90-136">In your function, click **Integrate**, expand **Documentation**, and copy both **Account name** and **Account key**.</span></span> <span data-ttu-id="73b90-137">Ezen hitelesítő adatok tooconnect toohello tárfiókot használni.</span><span class="sxs-lookup"><span data-stu-id="73b90-137">You use these credentials tooconnect toohello storage account.</span></span> <span data-ttu-id="73b90-138">Ha a tárfiók már csatlakozott, hagyja ki a toostep 4.</span><span class="sxs-lookup"><span data-stu-id="73b90-138">If you have already connected your storage account, skip toostep 4.</span></span>

    ![Hello tárfiók kapcsolat hitelesítő adatainak lekéréséhez.](./media/functions-create-storage-blob-triggered-function/functions-storage-account-connection.png)

1. <span data-ttu-id="73b90-140">Hello futtatása [Microsoft Azure Tártallózó](http://storageexplorer.com/) eszköz, kattintson a hello hello bal oldali ikon csatlakozni, válassza a **használja a tárfiók nevét és a kulcs**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="73b90-140">Run hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/) tool, click hello connect icon on hello left, choose **Use a storage account name and key**, and click **Next**.</span></span>

    ![Hello fiók Tártallózó eszköz futtatásához.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-1.png)

1. <span data-ttu-id="73b90-142">Adja meg a hello **fióknév** és **fiókkulcs** 1. lépésben kattintson **következő** , majd **Connect**.</span><span class="sxs-lookup"><span data-stu-id="73b90-142">Enter hello **Account name** and **Account key** from step 1, click **Next** and then **Connect**.</span></span> 

    ![Adja meg hello tároló hitelesítő adatait, és csatlakozzon.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-connect-2.png)

1. <span data-ttu-id="73b90-144">Bontsa ki a csatolt hello storage-fiókot, kattintson a jobb gombbal **Blob-tárolók**, kattintson a **létrehozás blob tároló**, típus `mycontainer`, és nyomja meg az enter.</span><span class="sxs-lookup"><span data-stu-id="73b90-144">Expand hello attached storage account, right-click **Blob containers**, click **Create blob container**, type `mycontainer`, and then press enter.</span></span>

    ![Hozzon létre egy tárolási üzenetsort.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-create-blob-container.png)

<span data-ttu-id="73b90-146">Most, hogy a blob-tároló, tesztelheti hello függvény egy fájltároló toohello feltöltésével.</span><span class="sxs-lookup"><span data-stu-id="73b90-146">Now that you have a blob container, you can test hello function by uploading a file toohello container.</span></span>

## <a name="test-hello-function"></a><span data-ttu-id="73b90-147">Hello függvény tesztelése</span><span class="sxs-lookup"><span data-stu-id="73b90-147">Test hello function</span></span>

1. <span data-ttu-id="73b90-148">Vissza a hello Azure-portálon, a Tallózás tooyour függvény bontsa ki a hello **naplók** hello alján lévő hello lap, és győződjön meg arról, hogy a napló streaming nem szünetel.</span><span class="sxs-lookup"><span data-stu-id="73b90-148">Back in hello Azure portal, browse tooyour function expand hello **Logs** at hello bottom of hello page and make sure that log streaming isn't paused.</span></span>

1. <span data-ttu-id="73b90-149">A Storage Explorerben bontsa ki a tárfiókját, a **Blobtárolók** és a **saját tároló** elemet.</span><span class="sxs-lookup"><span data-stu-id="73b90-149">In Storage Explorer, expand your storage account, **Blob containers**, and **mycontainer**.</span></span> <span data-ttu-id="73b90-150">Kattintson a **Feltöltés**, majd a **Fájlok feltöltése...** elemre.</span><span class="sxs-lookup"><span data-stu-id="73b90-150">Click **Upload** and then **Upload files...**.</span></span>

    ![A fájl toohello blobtárolóban feltöltése.](./media/functions-create-storage-blob-triggered-function/functions-storage-manager-upload-file-blob.png)

1. <span data-ttu-id="73b90-152">A hello **fájlok feltöltése** párbeszédpanelen kattintson az hello **fájlok** mező.</span><span class="sxs-lookup"><span data-stu-id="73b90-152">In hello **Upload files** dialog box, click hello **Files** field.</span></span> <span data-ttu-id="73b90-153">Keresse meg a helyi számítógépen, például a képfájl tooa fájl, válassza ki azt, és kattintson a **nyitott** , majd **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="73b90-153">Browse tooa file on your local computer, such as an image file, select it and click **Open** and then **Upload**.</span></span>

1. <span data-ttu-id="73b90-154">Lépjen vissza tooyour függvény naplókat, és győződjön meg arról, hogy hello a blob beolvasása megtörtént.</span><span class="sxs-lookup"><span data-stu-id="73b90-154">Go back tooyour function logs and verify that hello blob has been read.</span></span>

   ![Hello naplókban üzenet megtekintése.](./media/functions-create-storage-blob-triggered-function/functions-blob-storage-trigger-view-logs.png)

    >[!NOTE]
    > <span data-ttu-id="73b90-156">A függvény alkalmazás futtatásakor hello alapértelmezett felhasználási tervben lehet tooseveral perc közötti blob folyamatban, vagy frissített hello és hello be késleltetést működéséhez indított alatt.</span><span class="sxs-lookup"><span data-stu-id="73b90-156">When your function app runs in hello default Consumption plan, there may be a delay of up tooseveral minutes between hello blob being added or updated and hello function being triggered.</span></span> <span data-ttu-id="73b90-157">Ha kis késleltetésre van szüksége a blob által aktivált függvényekhez, célszerű App Service-csomagban futtatnia a függvényalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="73b90-157">If you need low latency in your blob triggered functions, consider running your function app in an App Service plan.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="73b90-158">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="73b90-158">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="73b90-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="73b90-159">Next steps</span></span>

<span data-ttu-id="73b90-160">Létrehozott egy függvényt, amely felvételekor futnak blob frissítése tooor a Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="73b90-160">You have created a function that runs when a blob is added tooor updated in Blob storage.</span></span> 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="73b90-161">További információ a blobtároló eseményindítóiról: [Azure Functions – a blobtároló kötései](functions-bindings-storage-blob.md).</span><span class="sxs-lookup"><span data-stu-id="73b90-161">For more information about Blob storage triggers, see [Azure Functions Blob storage bindings](functions-bindings-storage-blob.md).</span></span>
