---
title: "Az első függvény létrehozása az Azure portálon | Microsoft Docs"
description: "Ismerje meg, hogyan hozhatja létre az első Azure-függvényét kiszolgáló nélküli végrehajtáshoz az Azure Portalon."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/07/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 3ec1f278f21d89782137625aff200f07f15fd9fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-function-in-the-azure-portal"></a><span data-ttu-id="c03e6-103">Az első függvény létrehozása az Azure portálon</span><span class="sxs-lookup"><span data-stu-id="c03e6-103">Create your first function in the Azure portal</span></span>

<span data-ttu-id="c03e6-104">Az Azure Functions lehetővé teszi a kód végrehajtását kiszolgáló nélküli környezetben anélkül, hogy először létre kellene hoznia egy virtuális gépet vagy közzé kellene tennie egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c03e6-104">Azure Functions lets you execute your code in a serverless environment without having to first create a VM or publish a web application.</span></span> <span data-ttu-id="c03e6-105">Ebben a témakörben megismerheti, hogyan használhatja a Functions szolgáltatást egy „hello world” függvény létrehozására az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="c03e6-105">In this topic, learn how to use Functions to create a "hello world" function in the Azure portal.</span></span>

![Függvényalkalmazás létrehozása az Azure Portalon](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a><span data-ttu-id="c03e6-107">Jelentkezzen be az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="c03e6-107">Log in to Azure</span></span>

<span data-ttu-id="c03e6-108">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c03e6-108">Log in to the [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="c03e6-109">Függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c03e6-109">Create a function app</span></span>

<span data-ttu-id="c03e6-110">Rendelkeznie kell egy függvényalkalmazással a függvények végrehajtásának biztosításához.</span><span class="sxs-lookup"><span data-stu-id="c03e6-110">You must have a function app to host the execution of your functions.</span></span> <span data-ttu-id="c03e6-111">A függvényalkalmazás lehetővé teszi, hogy logikai egységbe csoportosítsa a függvényeket az erőforrások egyszerűbb felügyelete, telepítése és megosztása érdekében.</span><span class="sxs-lookup"><span data-stu-id="c03e6-111">A function app lets you group functions as a logic unit for easier management, deployment, and sharing of resources.</span></span> 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

<span data-ttu-id="c03e6-112">Ezután létrehozhat egy függvényt az új függvényalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c03e6-112">Next, you create a function in the new function app.</span></span>

## <span data-ttu-id="c03e6-113"><a name="create-function"></a>HTTP által aktivált függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="c03e6-113"><a name="create-function"></a>Create an HTTP triggered function</span></span>

1. <span data-ttu-id="c03e6-114">Bontsa ki az új függvényalkalmazást, majd kattintson a **Függvények** elem melletti **+** gombra.</span><span class="sxs-lookup"><span data-stu-id="c03e6-114">Expand your new function app, then click the **+** button next to **Functions**.</span></span>

2.  <span data-ttu-id="c03e6-115">A **Gyors kezdés** lapon kattintson a **WebHook + API** elemre, **válasszon egy nyelvet** a függvény számára, majd kattintson a **Függvény létrehozása** elemre.</span><span class="sxs-lookup"><span data-stu-id="c03e6-115">In the **Get started quickly** page, select **WebHook + API**, **Choose a language** for your function, and click **Create this function**.</span></span> 
   
    ![A függvények gyors létrehozása az Azure Portalon.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

<span data-ttu-id="c03e6-117">A rendszer létrehoz egy függvényt a választott nyelven a HTTP által indított függvények sablonjának használatával.</span><span class="sxs-lookup"><span data-stu-id="c03e6-117">A function is created in your chosen language using the template for an HTTP triggered function.</span></span> <span data-ttu-id="c03e6-118">Az új függvényt egy HTTP-kérelem küldésével futtathatja.</span><span class="sxs-lookup"><span data-stu-id="c03e6-118">You can run the new function by sending an HTTP request.</span></span>

## <a name="test-the-function"></a><span data-ttu-id="c03e6-119">A függvény tesztelése</span><span class="sxs-lookup"><span data-stu-id="c03e6-119">Test the function</span></span>

1. <span data-ttu-id="c03e6-120">Az új függvényben kattintson a **< /> Függvény URL-címének beolvasása** elemre, válassza a **default (Function key)** (alapértelmezett (funkcióbillentyű)) lehetőséget, majd kattintson a **Másolás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c03e6-120">In your new function, click **</> Get function URL**, select **default (Function key)**, and then click **Copy**.</span></span> 

    ![A függvény URL-címének másolása az Azure portálról](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. <span data-ttu-id="c03e6-122">Illessze be a függvény URL-címét a böngésző címsorába.</span><span class="sxs-lookup"><span data-stu-id="c03e6-122">Paste the function URL into your browser's address bar.</span></span> <span data-ttu-id="c03e6-123">Az URL-címhez fűzze hozzá a `&name=<yourname>` lekérdezési karakterláncot, majd nyomja le az `Enter` billentyűt a billentyűzeten a kérés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="c03e6-123">Append the query string `&name=<yourname>` to this URL and press the `Enter` key on your keyboard to execute the request.</span></span> <span data-ttu-id="c03e6-124">A következőkben egy példa látható az Edge böngészőben a függvény által visszaadott válaszra:</span><span class="sxs-lookup"><span data-stu-id="c03e6-124">The following is an example of the response returned by the function in the Edge browser:</span></span>

    ![A függvény által visszaadott válasz a böngészőben.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    <span data-ttu-id="c03e6-126">A kérelem URL-címe alapértelmezés szerint tartalmazza a függvény HTTP protokollon keresztüli eléréséhez szükséges kulcsot.</span><span class="sxs-lookup"><span data-stu-id="c03e6-126">The request URL includes a key that is required, by default, to access your function over HTTP.</span></span>   

3. <span data-ttu-id="c03e6-127">A függvény futásakor a rendszer nyomkövetési adatok ír a naplókba.</span><span class="sxs-lookup"><span data-stu-id="c03e6-127">When your function runs, trace information is written to the logs.</span></span> <span data-ttu-id="c03e6-128">Az előző végrehajtás nyomkövetési adatainak megtekintéséhez térjen vissza a függvényhez a portálon, és a **Naplók** elem kibontásához kattintson a képernyő alján található felfelé mutató nyílra.</span><span class="sxs-lookup"><span data-stu-id="c03e6-128">To see the trace output from the previous execution, return to your function in the portal and click the up arrow at the bottom of the screen to expand **Logs**.</span></span> 

   ![A függvények naplómegtekintője az Azure Portalon.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a><span data-ttu-id="c03e6-130">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c03e6-130">Clean up resources</span></span>

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="c03e6-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c03e6-131">Next steps</span></span>

<span data-ttu-id="c03e6-132">Létrehozott egy függvényalkalmazást és egy HTTP által aktivált egyszerű függvényt.</span><span class="sxs-lookup"><span data-stu-id="c03e6-132">You have created a function app with a simple HTTP triggered function.</span></span>  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="c03e6-133">További információ: [Azure Functions – HTTP- és webhookkötések](functions-bindings-http-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="c03e6-133">For more information, see [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md).</span></span>



