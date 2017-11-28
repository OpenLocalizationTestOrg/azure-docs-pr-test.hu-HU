---
title: "egy függvényt a GitHub webhook által indított Azure aaaCreate |} Microsoft Docs"
description: "Az Azure Functions toocreate, amelyet a GitHub webhook kiszolgáló nélküli függvényt használja."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 8ffcde82c9310d749159ed53eab113658e38a030
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-a-github-webhook"></a><span data-ttu-id="1d91a-103">GitHub-webhookok által meghívott függvények létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d91a-103">Create a function triggered by a GitHub webhook</span></span>

<span data-ttu-id="1d91a-104">Megtudhatja, hogyan toocreate webhook HTTP-kérelem a GitHub-specifikus hasznos adatok között az által elindított függvényt.</span><span class="sxs-lookup"><span data-stu-id="1d91a-104">Learn how toocreate a function that is triggered by an HTTP webhook request with a GitHub-specific payload.</span></span>

![Github Webhook indított függvényt hello Azure-portálon](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="1d91a-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1d91a-106">Prerequisites</span></span>

+ <span data-ttu-id="1d91a-107">Legalább egy projekttel rendelkező GitHub-fiók.</span><span class="sxs-lookup"><span data-stu-id="1d91a-107">A GitHub account with at least one project.</span></span>
+ <span data-ttu-id="1d91a-108">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="1d91a-108">An Azure subscription.</span></span> <span data-ttu-id="1d91a-109">Ha még nincs előfizetése, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), mielőtt hozzákezd.</span><span class="sxs-lookup"><span data-stu-id="1d91a-109">If you don't have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="1d91a-110">Azure-függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d91a-110">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![A függvényalkalmazás elkészült.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="1d91a-112">A függvény a következő alkalmazásban hello új függvény létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1d91a-112">Next, you create a function in hello new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-github-webhook-triggered-function"></a><span data-ttu-id="1d91a-113">GitHub-webhook által aktivált függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d91a-113">Create a GitHub webhook triggered function</span></span>

1. <span data-ttu-id="1d91a-114">Bontsa ki a függvény alkalmazást, majd kattintson a hello  **+**  gomb melletti túl**funkciók**.</span><span class="sxs-lookup"><span data-stu-id="1d91a-114">Expand your function app and click hello **+** button next too**Functions**.</span></span> <span data-ttu-id="1d91a-115">Ha ez az első függvényét hello az függvény alkalmazásban, válassza ki a **egyéni függvény**.</span><span class="sxs-lookup"><span data-stu-id="1d91a-115">If this is hello first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="1d91a-116">Ez a függvény sablonok teljes készletének hello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1d91a-116">This displays hello complete set of function templates.</span></span>

    ![Gyors üzembe helyezés lap funkciók hello Azure-portálon](./media/functions-create-github-webhook-triggered-function/add-first-function.png)

2. <span data-ttu-id="1d91a-118">Jelölje be hello **GitHub WebHook** sablont a kívánt nyelvet.</span><span class="sxs-lookup"><span data-stu-id="1d91a-118">Select hello **GitHub WebHook** template for your desired language.</span></span> <span data-ttu-id="1d91a-119">**Nevezze el a függvényt**, majd kattintson a **Létrehozás** elemre.</span><span class="sxs-lookup"><span data-stu-id="1d91a-119">**Name your function**, then select **Create**.</span></span>

     ![GitHub webhook indított függvény létrehozása a hello Azure-portálon](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

3. <span data-ttu-id="1d91a-121">Kattintson az új függvény **<> / Get függvény URL-cím**, majd másolja ki és mentse a hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="1d91a-121">In your new function, click **</> Get function URL**, then copy and save hello values.</span></span> <span data-ttu-id="1d91a-122">Ugyanaz a hello **<> / Get GitHub titkos**.</span><span class="sxs-lookup"><span data-stu-id="1d91a-122">Do hello same thing for **</> Get GitHub secret**.</span></span> <span data-ttu-id="1d91a-123">Ezen értékek tooconfigure hello webhook használhatja a Githubon.</span><span class="sxs-lookup"><span data-stu-id="1d91a-123">You use these values tooconfigure hello webhook in GitHub.</span></span>

    ![Tekintse át a hello funkciókódot](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png)

<span data-ttu-id="1d91a-125">A következő lépésben egy webhookot hoz létre a GitHub-tárban.</span><span class="sxs-lookup"><span data-stu-id="1d91a-125">Next, you create a webhook in your GitHub repository.</span></span>

## <a name="configure-hello-webhook"></a><span data-ttu-id="1d91a-126">Hello webhook konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1d91a-126">Configure hello webhook</span></span>

1. <span data-ttu-id="1d91a-127">A github webhelyen lépjen a saját tooa tárház.</span><span class="sxs-lookup"><span data-stu-id="1d91a-127">In GitHub, navigate tooa repository that you own.</span></span> <span data-ttu-id="1d91a-128">Használhat bármely elágaztatott adattárat is.</span><span class="sxs-lookup"><span data-stu-id="1d91a-128">You can also use any repository that you have forked.</span></span> <span data-ttu-id="1d91a-129">Ha a tárház toofork van szüksége, <https://github.com/Azure-Samples/functions-quickstart>.</span><span class="sxs-lookup"><span data-stu-id="1d91a-129">If you need toofork a repository, use <https://github.com/Azure-Samples/functions-quickstart>.</span></span>

1. <span data-ttu-id="1d91a-130">Kattintson a **Settings** (Beállítások), majd a **Webhooks** (Webhookok) és végül az **Add webhook** (Webhook hozzáadása) elemre.</span><span class="sxs-lookup"><span data-stu-id="1d91a-130">Click **Settings**, then click **Webhooks**, and  **Add webhook**.</span></span>

    ![GitHub-webhook hozzáadása](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

1. <span data-ttu-id="1d91a-132">Hello táblázatban megadott beállításokkal, majd kattintson az **adja hozzá a webhook**.</span><span class="sxs-lookup"><span data-stu-id="1d91a-132">Use settings as specified in hello table, then click **Add webhook**.</span></span>

    ![Hello webhook URL-CÍMÉT és a titkos kulcs beállítása](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

| <span data-ttu-id="1d91a-134">Beállítás</span><span class="sxs-lookup"><span data-stu-id="1d91a-134">Setting</span></span> | <span data-ttu-id="1d91a-135">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="1d91a-135">Suggested value</span></span> | <span data-ttu-id="1d91a-136">Leírás</span><span class="sxs-lookup"><span data-stu-id="1d91a-136">Description</span></span> |
|---|---|---|
| <span data-ttu-id="1d91a-137">**Hasznos adat URL-címe**</span><span class="sxs-lookup"><span data-stu-id="1d91a-137">**Payload URL**</span></span> | <span data-ttu-id="1d91a-138">Másolt érték</span><span class="sxs-lookup"><span data-stu-id="1d91a-138">Copied value</span></span> | <span data-ttu-id="1d91a-139">Hello által visszaadott értéket használja **<> / Get függvény URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="1d91a-139">Use hello value returned by  **</> Get function URL**.</span></span> |
| <span data-ttu-id="1d91a-140">**Titkos kód**</span><span class="sxs-lookup"><span data-stu-id="1d91a-140">**Secret**</span></span>   | <span data-ttu-id="1d91a-141">Másolt érték</span><span class="sxs-lookup"><span data-stu-id="1d91a-141">Copied value</span></span> | <span data-ttu-id="1d91a-142">Hello által visszaadott értéket használja **<> / Get GitHub titkos**.</span><span class="sxs-lookup"><span data-stu-id="1d91a-142">Use hello value returned by  **</> Get GitHub secret**.</span></span> |
| <span data-ttu-id="1d91a-143">**Tartalom típusa**</span><span class="sxs-lookup"><span data-stu-id="1d91a-143">**Content type**</span></span> | <span data-ttu-id="1d91a-144">application/json</span><span class="sxs-lookup"><span data-stu-id="1d91a-144">application/json</span></span> | <span data-ttu-id="1d91a-145">hello függvényhez a JSON hasznos adatok között.</span><span class="sxs-lookup"><span data-stu-id="1d91a-145">hello function expects a JSON payload.</span></span> |
| <span data-ttu-id="1d91a-146">Eseményindítók</span><span class="sxs-lookup"><span data-stu-id="1d91a-146">Event triggers</span></span> | <span data-ttu-id="1d91a-147">Én szeretném kiválasztani az egyes eseményeket</span><span class="sxs-lookup"><span data-stu-id="1d91a-147">Let me select individual events</span></span> | <span data-ttu-id="1d91a-148">Csak szeretnénk tootrigger probléma Megjegyzés események.</span><span class="sxs-lookup"><span data-stu-id="1d91a-148">We only want tootrigger on issue comment events.</span></span>  |
| | <span data-ttu-id="1d91a-149">Probléma megjegyzése</span><span class="sxs-lookup"><span data-stu-id="1d91a-149">Issue comment</span></span> |  |

<span data-ttu-id="1d91a-150">Most, hello webhook, akkor konfigurált tootrigger a függvény egy új probléma megjegyzés hozzáadásakor.</span><span class="sxs-lookup"><span data-stu-id="1d91a-150">Now, hello webhook is configured tootrigger your function when a new issue comment is added.</span></span>

## <a name="test-hello-function"></a><span data-ttu-id="1d91a-151">Hello függvény tesztelése</span><span class="sxs-lookup"><span data-stu-id="1d91a-151">Test hello function</span></span>

1. <span data-ttu-id="1d91a-152">Nyissa meg a GitHub-tárház hello **problémák** fülre egy új böngészőablakban.</span><span class="sxs-lookup"><span data-stu-id="1d91a-152">In your GitHub repository, open hello **Issues** tab in a new browser window.</span></span>

1. <span data-ttu-id="1d91a-153">Hello új ablakban kattintson **új probléma**, írja be a címet, és kattintson a **küldje el az új probléma**.</span><span class="sxs-lookup"><span data-stu-id="1d91a-153">In hello new window, click **New Issue**, type a title, and then click **Submit new issue**.</span></span>

1. <span data-ttu-id="1d91a-154">Hello probléma, írja be egy megjegyzést, és kattintson a **Megjegyzés**.</span><span class="sxs-lookup"><span data-stu-id="1d91a-154">In hello issue, type a comment and click **Comment**.</span></span>

    ![Adja meg a GitHub-problémára vonatkozó megjegyzést.](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png)

1. <span data-ttu-id="1d91a-156">Lépjen vissza toohello portálon, és a hello naplók megtekintése.</span><span class="sxs-lookup"><span data-stu-id="1d91a-156">Go back toohello portal and view hello logs.</span></span> <span data-ttu-id="1d91a-157">Meg kell jelennie egy nyomkövetési bejegyzés hello új megjegyzés szövege.</span><span class="sxs-lookup"><span data-stu-id="1d91a-157">You should see a trace entry with hello new comment text.</span></span>

     ![Hello naplók megtekintése hello Megjegyzés szövege.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)

## <a name="clean-up-resources"></a><span data-ttu-id="1d91a-159">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="1d91a-159">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="1d91a-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d91a-160">Next steps</span></span>

<span data-ttu-id="1d91a-161">Létrehozott egy függvényt, amely akkor fut, amikor kérelem érkezik egy GitHub-webhookból.</span><span class="sxs-lookup"><span data-stu-id="1d91a-161">You have created a function that runs when a request is received from a GitHub webhook.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="1d91a-162">További információt a webhook-eseményindítókról az [Azure Functions – HTTP- és webhookkötések](functions-bindings-http-webhook.md) című témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="1d91a-162">For more information about webhook triggers, see [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md).</span></span>
