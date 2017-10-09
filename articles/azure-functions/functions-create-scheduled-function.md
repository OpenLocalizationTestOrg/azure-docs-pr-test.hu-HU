---
title: "egy függvényt, amely az Azure-ban ütemezés szerint futtatott aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate futó Azure-ban függvény alapján meghatározott ütemezés szerint."
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
ms.openlocfilehash: 793b06a65a154466dfd4c121bcc88082227cd597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a><span data-ttu-id="6e7ee-103">Időzítő által aktivált függvény létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="6e7ee-103">Create a function in Azure that is triggered by a timer</span></span>

<span data-ttu-id="6e7ee-104">Ismerje meg, hogyan toouse Azure Functions toocreate lefutó függvény alapján meghatározott ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-104">Learn how toouse Azure Functions toocreate a function that runs based a schedule that you define.</span></span>

![Függvény-alkalmazás létrehozása az Azure-portálon hello](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="6e7ee-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6e7ee-106">Prerequisites</span></span>

<span data-ttu-id="6e7ee-107">toocomplete Ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="6e7ee-107">toocomplete this tutorial:</span></span>

+ <span data-ttu-id="6e7ee-108">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="6e7ee-109">Azure-függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e7ee-109">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![A függvényalkalmazás elkészült.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="6e7ee-111">A függvény a következő alkalmazásban hello új függvény létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-111">Next, you create a function in hello new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a><span data-ttu-id="6e7ee-112">Időzítő által aktivált függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e7ee-112">Create a timer triggered function</span></span>

1. <span data-ttu-id="6e7ee-113">Bontsa ki a függvény alkalmazást, majd kattintson a hello  **+**  gomb melletti túl**funkciók**.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-113">Expand your function app and click hello **+** button next too**Functions**.</span></span> <span data-ttu-id="6e7ee-114">Ha ez az első függvényét hello az függvény alkalmazásban, válassza ki a **egyéni függvény**.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-114">If this is hello first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="6e7ee-115">Ez a függvény sablonok teljes készletének hello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-115">This displays hello complete set of function templates.</span></span>

    ![Gyors üzembe helyezés lap funkciók hello Azure-portálon](./media/functions-create-scheduled-function/add-first-function.png)

2. <span data-ttu-id="6e7ee-117">Jelölje be hello **TimerTrigger** sablont a kívánt nyelvet.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-117">Select hello **TimerTrigger** template for your desired language.</span></span> <span data-ttu-id="6e7ee-118">Majd beállításokkal hello hello táblázatban megadottak szerint:</span><span class="sxs-lookup"><span data-stu-id="6e7ee-118">Then use hello settings as specified in hello table:</span></span>

    ![Hozzon létre egy indított időzítő hello Azure-portálon.](./media/functions-create-scheduled-function/functions-create-timer-trigger.png)

    | <span data-ttu-id="6e7ee-120">Beállítás</span><span class="sxs-lookup"><span data-stu-id="6e7ee-120">Setting</span></span> | <span data-ttu-id="6e7ee-121">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="6e7ee-121">Suggested value</span></span> | <span data-ttu-id="6e7ee-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="6e7ee-122">Description</span></span> |
    |---|---|---|
    | <span data-ttu-id="6e7ee-123">**A függvény neve**</span><span class="sxs-lookup"><span data-stu-id="6e7ee-123">**Name your function**</span></span> | <span data-ttu-id="6e7ee-124">TimerTriggerCSharp1</span><span class="sxs-lookup"><span data-stu-id="6e7ee-124">TimerTriggerCSharp1</span></span> | <span data-ttu-id="6e7ee-125">Az időzítő indított függvény hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-125">Defines hello name of your timer triggered function.</span></span> |
    | <span data-ttu-id="6e7ee-126">**[Ütemezés](http://en.wikipedia.org/wiki/Cron#CRON_expression)**</span><span class="sxs-lookup"><span data-stu-id="6e7ee-126">**[Schedule](http://en.wikipedia.org/wiki/Cron#CRON_expression)**</span></span> | <span data-ttu-id="6e7ee-127">0 \*/1 \* \* \* \*</span><span class="sxs-lookup"><span data-stu-id="6e7ee-127">0 \*/1 \* \* \* \*</span></span> | <span data-ttu-id="6e7ee-128">A hat mező [CRON-kifejezés](http://en.wikipedia.org/wiki/Cron#CRON_expression) , amely ütemezi a függvény toorun percenként.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-128">A six field [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression) that schedules your function toorun every minute.</span></span> |

2. <span data-ttu-id="6e7ee-129">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-129">Click **Create**.</span></span> <span data-ttu-id="6e7ee-130">Létrejön egy függvény a választott nyelven, amely minden percben futni fog.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-130">A function is created in your chosen language that runs every minute.</span></span>

3. <span data-ttu-id="6e7ee-131">Ellenőrizze a végrehajtási toohello naplók írt nyomkövetési adatok megtekintéséhez használatos időkategóriát.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-131">Verify execution by viewing trace information written toohello logs.</span></span>

    ![Funkciók viewer bejelentkezés hello Azure-portálon.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

<span data-ttu-id="6e7ee-133">Most módosíthatja hello függvény ütemezését, hogy kevesebb gyakran, például óránként egyszer fusson.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-133">Now, you can change hello function's schedule so that it runs less often, such as once every hour.</span></span> 

## <a name="update-hello-timer-schedule"></a><span data-ttu-id="6e7ee-134">Frissítés hello időzítő ütemezése</span><span class="sxs-lookup"><span data-stu-id="6e7ee-134">Update hello timer schedule</span></span>

1. <span data-ttu-id="6e7ee-135">Bontsa ki a függvényt, és kattintson az **Integráció** elemre.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-135">Expand your function and click **Integrate**.</span></span> <span data-ttu-id="6e7ee-136">Ez az ahol határozza meg a bemeneti és kimeneti kötések a függvény és hello ütemezést is beállíthat.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-136">This is where you define input and output bindings for your function and also set hello schedule.</span></span> 

2. <span data-ttu-id="6e7ee-137">Adja meg az új `0 0 */1 * * *` értéket az **Ütemezés** beállításnál, és kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-137">Enter a new **Schedule** value of `0 0 */1 * * *`, and then click **Save**.</span></span>  

![Funkciók hello Azure-portálon időzítő ütemezés frissítése.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

<span data-ttu-id="6e7ee-139">A függvény ezután óránként egyszer fog futni.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-139">You now have a function that runs once every hour.</span></span> 

## <a name="clean-up-resources"></a><span data-ttu-id="6e7ee-140">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6e7ee-140">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="6e7ee-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6e7ee-141">Next steps</span></span>

<span data-ttu-id="6e7ee-142">Létrehozott egy ütemezés alapján futó függvényt.</span><span class="sxs-lookup"><span data-stu-id="6e7ee-142">You have created a function that runs based on a schedule.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="6e7ee-143">További információ az időzítési eseményindítóktól: [A kódvégrehajtás ütemezése az Azure Functions szolgáltatásban](functions-bindings-timer.md).</span><span class="sxs-lookup"><span data-stu-id="6e7ee-143">For more information timer triggers, see [Schedule code execution with Azure Functions](functions-bindings-timer.md).</span></span>