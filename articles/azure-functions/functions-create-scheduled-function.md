---
title: "Ütemezés szerint futó függvények létrehozása az Azure-ban | Microsoft Docs"
description: "Megtudhatja, hogyan hozhat olyan függvényt az Azure-ban, amely az Ön által meghatározott ütemezés alapján fut."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 03cc5e71e8eb20002cf58e713fc0fc92a9129874
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a><span data-ttu-id="89662-103">Időzítő által aktivált függvény létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="89662-103">Create a function in Azure that is triggered by a timer</span></span>

<span data-ttu-id="89662-104">Tekintse át, hogyan használhatja az Azure Functions szolgáltatást olyan függvény létrehozására, amely az Ön által meghatározott ütemezés alapján fut.</span><span class="sxs-lookup"><span data-stu-id="89662-104">Learn how to use Azure Functions to create a function that runs based a schedule that you define.</span></span>

![Függvényalkalmazás létrehozása az Azure Portalon](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="89662-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="89662-106">Prerequisites</span></span>

<span data-ttu-id="89662-107">Az oktatóanyag elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="89662-107">To complete this tutorial:</span></span>

+ <span data-ttu-id="89662-108">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="89662-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="89662-109">Azure-függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="89662-109">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![A függvényalkalmazás elkészült.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="89662-111">Ezután létrehozhat egy függvényt az új függvényalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="89662-111">Next, you create a function in the new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a><span data-ttu-id="89662-112">Időzítő által aktivált függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="89662-112">Create a timer triggered function</span></span>

1. <span data-ttu-id="89662-113">Bontsa ki a függvényalkalmazást, és kattintson a **Függvények** elem melletti **+** gombra.</span><span class="sxs-lookup"><span data-stu-id="89662-113">Expand your function app and click the **+** button next to **Functions**.</span></span> <span data-ttu-id="89662-114">Ha ez az első függvény a függvényalkalmazásban, jelölje ki az **Egyéni függvény** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="89662-114">If this is the first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="89662-115">Ez megjeleníti a függvénysablonok teljes készletét.</span><span class="sxs-lookup"><span data-stu-id="89662-115">This displays the complete set of function templates.</span></span>

    ![Függvények gyors létrehozásának oldala az Azure Portalon](./media/functions-create-scheduled-function/add-first-function.png)

2. <span data-ttu-id="89662-117">Kattintson a kívánt nyelvhez tartozó **TimerTrigger** sablonra.</span><span class="sxs-lookup"><span data-stu-id="89662-117">Select the **TimerTrigger** template for your desired language.</span></span> <span data-ttu-id="89662-118">Ezt követően használja a táblázatban megadott beállításokat:</span><span class="sxs-lookup"><span data-stu-id="89662-118">Then use the settings as specified in the table:</span></span>

    ![Hozzon létre egy időzítő által aktivált függvényt az Azure Portalon.](./media/functions-create-scheduled-function/functions-create-timer-trigger.png)

    | <span data-ttu-id="89662-120">Beállítás</span><span class="sxs-lookup"><span data-stu-id="89662-120">Setting</span></span> | <span data-ttu-id="89662-121">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="89662-121">Suggested value</span></span> | <span data-ttu-id="89662-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="89662-122">Description</span></span> |
    |---|---|---|
    | <span data-ttu-id="89662-123">**A függvény neve**</span><span class="sxs-lookup"><span data-stu-id="89662-123">**Name your function**</span></span> | <span data-ttu-id="89662-124">TimerTriggerCSharp1</span><span class="sxs-lookup"><span data-stu-id="89662-124">TimerTriggerCSharp1</span></span> | <span data-ttu-id="89662-125">Az időzítő által aktivált függvény nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="89662-125">Defines the name of your timer triggered function.</span></span> |
    | <span data-ttu-id="89662-126">**[Ütemezés](http://en.wikipedia.org/wiki/Cron#CRON_expression)**</span><span class="sxs-lookup"><span data-stu-id="89662-126">**[Schedule](http://en.wikipedia.org/wiki/Cron#CRON_expression)**</span></span> | <span data-ttu-id="89662-127">0 \*/1 \* \* \* \*</span><span class="sxs-lookup"><span data-stu-id="89662-127">0 \*/1 \* \* \* \*</span></span> | <span data-ttu-id="89662-128">Hat mezőből álló [CRON-kifejezés](http://en.wikipedia.org/wiki/Cron#CRON_expression), amely úgy ütemezi a függvényt, hogy minden percben fusson.</span><span class="sxs-lookup"><span data-stu-id="89662-128">A six field [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression) that schedules your function to run every minute.</span></span> |

2. <span data-ttu-id="89662-129">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="89662-129">Click **Create**.</span></span> <span data-ttu-id="89662-130">Létrejön egy függvény a választott nyelven, amely minden percben futni fog.</span><span class="sxs-lookup"><span data-stu-id="89662-130">A function is created in your chosen language that runs every minute.</span></span>

3. <span data-ttu-id="89662-131">Ellenőrizze a végrehajtást a naplókban gyűjtött nyomkövetési adatok áttekintésével.</span><span class="sxs-lookup"><span data-stu-id="89662-131">Verify execution by viewing trace information written to the logs.</span></span>

    ![A függvények naplómegtekintője az Azure Portalon.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

<span data-ttu-id="89662-133">Most módosíthatja a függvény ütemezését, hogy ritkábban, például óránként egyszer fusson.</span><span class="sxs-lookup"><span data-stu-id="89662-133">Now, you can change the function's schedule so that it runs less often, such as once every hour.</span></span> 

## <a name="update-the-timer-schedule"></a><span data-ttu-id="89662-134">Az időzítő ütemezésének módosítása</span><span class="sxs-lookup"><span data-stu-id="89662-134">Update the timer schedule</span></span>

1. <span data-ttu-id="89662-135">Bontsa ki a függvényt, és kattintson az **Integráció** elemre.</span><span class="sxs-lookup"><span data-stu-id="89662-135">Expand your function and click **Integrate**.</span></span> <span data-ttu-id="89662-136">Itt határozhatja meg a függvény bemeneti és kimeneti kötéseit, és itt állíthatja be az ütemezést is.</span><span class="sxs-lookup"><span data-stu-id="89662-136">This is where you define input and output bindings for your function and also set the schedule.</span></span> 

2. <span data-ttu-id="89662-137">Adja meg az új `0 0 */1 * * *` értéket az **Ütemezés** beállításnál, és kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="89662-137">Enter a new **Schedule** value of `0 0 */1 * * *`, and then click **Save**.</span></span>  

![A függvények időzítési ütemezésének módosítása az Azure Portalon.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

<span data-ttu-id="89662-139">A függvény ezután óránként egyszer fog futni.</span><span class="sxs-lookup"><span data-stu-id="89662-139">You now have a function that runs once every hour.</span></span> 

## <a name="clean-up-resources"></a><span data-ttu-id="89662-140">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="89662-140">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="89662-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="89662-141">Next steps</span></span>

<span data-ttu-id="89662-142">Létrehozott egy ütemezés alapján futó függvényt.</span><span class="sxs-lookup"><span data-stu-id="89662-142">You have created a function that runs based on a schedule.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="89662-143">További információ az időzítési eseményindítóktól: [A kódvégrehajtás ütemezése az Azure Functions szolgáltatásban](functions-bindings-timer.md).</span><span class="sxs-lookup"><span data-stu-id="89662-143">For more information timer triggers, see [Schedule code execution with Azure Functions](functions-bindings-timer.md).</span></span>