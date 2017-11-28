---
title: "az első függvény eltávolítása hello Azure Portal aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan kiszolgáló nélküli végrehajtási a függvény az első Azure toocreate hello Azure-portálon."
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
ms.openlocfilehash: 84283d7d4bc6015061946af4589f9a70ae61f36b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-function-in-hello-azure-portal"></a><span data-ttu-id="02bb0-103">A hello Azure-portálon az első függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="02bb0-103">Create your first function in hello Azure portal</span></span>

<span data-ttu-id="02bb0-104">Az Azure Functions lehetővé teszi, hogy a kód egy kiszolgáló nélküli környezetben anélkül, hogy hozzon létre egy virtuális Gépet, vagy tegye közzé a webalkalmazást toofirst hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="02bb0-104">Azure Functions lets you execute your code in a serverless environment without having toofirst create a VM or publish a web application.</span></span> <span data-ttu-id="02bb0-105">Ebben a témakörben elsajátíthatja toouse működését toocreate hello Azure-portálon a "hello world" függvényt.</span><span class="sxs-lookup"><span data-stu-id="02bb0-105">In this topic, learn how toouse Functions toocreate a "hello world" function in hello Azure portal.</span></span>

![Függvény-alkalmazás létrehozása az Azure-portálon hello](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-tooazure"></a><span data-ttu-id="02bb0-107">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="02bb0-107">Log in tooAzure</span></span>

<span data-ttu-id="02bb0-108">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="02bb0-108">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="02bb0-109">Függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="02bb0-109">Create a function app</span></span>

<span data-ttu-id="02bb0-110">Rendelkeznie kell egy függvény app toohost hello a függvények végrehajtásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="02bb0-110">You must have a function app toohost hello execution of your functions.</span></span> <span data-ttu-id="02bb0-111">A függvényalkalmazás lehetővé teszi, hogy logikai egységbe csoportosítsa a függvényeket az erőforrások egyszerűbb felügyelete, telepítése és megosztása érdekében.</span><span class="sxs-lookup"><span data-stu-id="02bb0-111">A function app lets you group functions as a logic unit for easier management, deployment, and sharing of resources.</span></span> 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

<span data-ttu-id="02bb0-112">A függvény a következő alkalmazásban hello új függvény létrehozása.</span><span class="sxs-lookup"><span data-stu-id="02bb0-112">Next, you create a function in hello new function app.</span></span>

## <span data-ttu-id="02bb0-113"><a name="create-function"></a>HTTP által aktivált függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="02bb0-113"><a name="create-function"></a>Create an HTTP triggered function</span></span>

1. <span data-ttu-id="02bb0-114">Bontsa ki az új függvény alkalmazást, majd kattintson az hello  **+**  gomb melletti túl**funkciók**.</span><span class="sxs-lookup"><span data-stu-id="02bb0-114">Expand your new function app, then click hello **+** button next too**Functions**.</span></span>

2.  <span data-ttu-id="02bb0-115">A hello **gyorsan** lapon jelölje be **WebHook + API**, **válassza ki a nyelvet** a függvény, és kattintson a **Ez a függvény létrehozása** .</span><span class="sxs-lookup"><span data-stu-id="02bb0-115">In hello **Get started quickly** page, select **WebHook + API**, **Choose a language** for your function, and click **Create this function**.</span></span> 
   
    ![Az Azure-portálon hello Functions gyorsindítójával.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

<span data-ttu-id="02bb0-117">A következő függvényt egy indított HTTP függvény hello sablonnal a választott nyelven jön létre.</span><span class="sxs-lookup"><span data-stu-id="02bb0-117">A function is created in your chosen language using hello template for an HTTP triggered function.</span></span> <span data-ttu-id="02bb0-118">HTTP-kérelem küldésével hello új funkció is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="02bb0-118">You can run hello new function by sending an HTTP request.</span></span>

## <a name="test-hello-function"></a><span data-ttu-id="02bb0-119">Hello függvény tesztelése</span><span class="sxs-lookup"><span data-stu-id="02bb0-119">Test hello function</span></span>

1. <span data-ttu-id="02bb0-120">Az új függvényben kattintson a **< /> Függvény URL-címének beolvasása** elemre, válassza a **default (Function key)** (alapértelmezett (funkcióbillentyű)) lehetőséget, majd kattintson a **Másolás** gombra.</span><span class="sxs-lookup"><span data-stu-id="02bb0-120">In your new function, click **</> Get function URL**, select **default (Function key)**, and then click **Copy**.</span></span> 

    ![Hello függvény URL-Címének másolása az Azure-portálon hello](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. <span data-ttu-id="02bb0-122">Hello függvény URL-cím illessze be a böngésző címsorába.</span><span class="sxs-lookup"><span data-stu-id="02bb0-122">Paste hello function URL into your browser's address bar.</span></span> <span data-ttu-id="02bb0-123">Hello lekérdezési karakterlánc hozzáfűzése `&name=<yourname>` toothis URL-címet, és nyomja le az hello `Enter` kulcs a billentyűzet tooexecute hello kérésre.</span><span class="sxs-lookup"><span data-stu-id="02bb0-123">Append hello query string `&name=<yourname>` toothis URL and press hello `Enter` key on your keyboard tooexecute hello request.</span></span> <span data-ttu-id="02bb0-124">hello az alábbiakban látható egy példa az Edge böngészőben hello hello függvény által visszaadott hello választ:</span><span class="sxs-lookup"><span data-stu-id="02bb0-124">hello following is an example of hello response returned by hello function in hello Edge browser:</span></span>

    ![Függvény válasz hello böngészőben.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    <span data-ttu-id="02bb0-126">hello kérelem URL-cím szükséges, alapértelmezés szerint tooaccess kulcsot tartalmaz, a függvény HTTP Protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="02bb0-126">hello request URL includes a key that is required, by default, tooaccess your function over HTTP.</span></span>   

3. <span data-ttu-id="02bb0-127">A függvény futásakor nyomkövetési adatok írása toohello naplókat.</span><span class="sxs-lookup"><span data-stu-id="02bb0-127">When your function runs, trace information is written toohello logs.</span></span> <span data-ttu-id="02bb0-128">toosee hello nyomkövetési kimenetét hello előző végrehajtási, térjen vissza a tooyour függvény hello portálon, és kattintson a felfelé mutató nyíl hello képernyő tooexpand hello alján hello **naplók**.</span><span class="sxs-lookup"><span data-stu-id="02bb0-128">toosee hello trace output from hello previous execution, return tooyour function in hello portal and click hello up arrow at hello bottom of hello screen tooexpand **Logs**.</span></span> 

   ![Funkciók viewer bejelentkezés hello Azure-portálon.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a><span data-ttu-id="02bb0-130">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="02bb0-130">Clean up resources</span></span>

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="02bb0-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="02bb0-131">Next steps</span></span>

<span data-ttu-id="02bb0-132">Létrehozott egy függvényalkalmazást és egy HTTP által aktivált egyszerű függvényt.</span><span class="sxs-lookup"><span data-stu-id="02bb0-132">You have created a function app with a simple HTTP triggered function.</span></span>  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="02bb0-133">További információ: [Azure Functions – HTTP- és webhookkötések](functions-bindings-http-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="02bb0-133">For more information, see [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md).</span></span>



