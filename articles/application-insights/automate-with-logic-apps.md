---
title: "aaaAutomate Azure Application Insights Logic Apps segítségével dolgozza fel."
description: "Ismerje meg, milyen gyorsan automatizálhatja ismételhető folyamatok hello Application Insights összekötő tooyour logikai alkalmazás hozzáadásával."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: bwren
ms.openlocfilehash: 8565aadf0644ffb10da8a0964821901907db2e27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a><span data-ttu-id="b6e0f-103">Az Application Insights-folyamatok automatizálása a Logic Apps segítségével</span><span class="sxs-lookup"><span data-stu-id="b6e0f-103">Automate Application Insights processes by using Logic Apps</span></span>

<span data-ttu-id="b6e0f-104">Tegye veszi észre magát ismételten futó hello azonos lekérdezések a telemetriai adatok toocheck a szolgáltatás megfelelően működnek-e?</span><span class="sxs-lookup"><span data-stu-id="b6e0f-104">Do you find yourself repeatedly running hello same queries on your telemetry data toocheck whether your service is functioning properly?</span></span> <span data-ttu-id="b6e0f-105">Kinézetű tooautomate ezeket a lekérdezéseket, a trendek és rendellenességeket kereséséhez és majd a felhasználókat ezekbe a csoportokba a saját munkafolyamatok építhetők fel? hello Azure Application Insights-összekötő (előzetes verzió) Logic Apps hello ideális eszközt erre a célra.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-105">Are you looking tooautomate these queries for finding trends and anomalies and then build your own workflows around them? hello Azure Application Insights connector (preview) for Logic Apps is hello right tool for this purpose.</span></span>

<span data-ttu-id="b6e0f-106">Ezt az integrációt a automatizálhat egysoros kód írása nélkül számos folyamatokat.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-106">With this integration, you can automate numerous processes without writing a single line of code.</span></span> <span data-ttu-id="b6e0f-107">Létrehozhat egy logikai alkalmazást az Application Insights hello összekötő tooquickly bármely Application Insights folyamat automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-107">You can create a logic app with hello Application Insights connector tooquickly automate any Application Insights process.</span></span> 

<span data-ttu-id="b6e0f-108">Hozzáadhat további műveleteket is.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-108">You can add additional actions as well.</span></span> <span data-ttu-id="b6e0f-109">hello Logic Apps szolgáltatás az Azure App Service elérhetővé több száz műveletek.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-109">hello Logic Apps feature of Azure App Service makes hundreds of actions available.</span></span> <span data-ttu-id="b6e0f-110">Például a logikai alkalmazás, automatikusan küldjön e-mailben értesítést vagy hozzon létre egy hibajelentést a Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-110">For example, by using a logic app, you can automatically send an email notification or create a bug in Visual Studio Team Services.</span></span> <span data-ttu-id="b6e0f-111">Is használhatja a hello számos elérhető [sablonok](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) toohelp felgyorsítása hello a logikai alkalmazás létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-111">You can also use one of hello many available [templates](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) toohelp speed up hello process of creating your logic app.</span></span> 

## <a name="create-a-logic-app-for-application-insights"></a><span data-ttu-id="b6e0f-112">Az Application Insights logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6e0f-112">Create a logic app for Application Insights</span></span>

<span data-ttu-id="b6e0f-113">Ebben az oktatóanyagban elsajátíthatja, hogyan toocreate hello Analytics autocluster algoritmus toogroup használó logikai alkalmazás attribútumok webalkalmazás hello adataiban.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-113">In this tutorial, you learn how toocreate a logic app that uses hello Analytics autocluster algorithm toogroup attributes in hello data for a web application.</span></span> <span data-ttu-id="b6e0f-114">hello folyamata hello eredmények automatikusan elküldi e-mailben, hogyan használhatja az alkalmazásokhoz kapcsolódó elemzések elemzés és a Logic Apps együtt egy példát.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-114">hello flow automatically sends hello results by email, just one example of how you can use Application Insights Analytics and Logic Apps together.</span></span> 

### <a name="step-1-create-a-logic-app"></a><span data-ttu-id="b6e0f-115">1. lépés: Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6e0f-115">Step 1: Create a logic app</span></span>
1. <span data-ttu-id="b6e0f-116">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b6e0f-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="b6e0f-117">A hello **új** ablaktáblán válassza előbb **Web + mobil**, majd válassza ki **logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-117">In hello **New** pane, select **Web + Mobile**, and then select **Logic App**.</span></span>

    ![Új logikai alkalmazás ablak](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a><span data-ttu-id="b6e0f-119">2. lépés: A Logic Apps alkalmazást eseményindító létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6e0f-119">Step 2: Create a trigger for your logic app</span></span>
1. <span data-ttu-id="b6e0f-120">A hello **Logic App Designer** ablakban, a **indítsa el az általános eseményindító**, jelölje be **ismétlődési**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-120">In hello **Logic App Designer** window, under **Start with a common trigger**, select **Recurrence**.</span></span>

    ![Logic App-Tervező ablak](./media/automate-with-logic-apps/logicapp2.png)

2. <span data-ttu-id="b6e0f-122">A hello **gyakoriság** mezőben válassza **nap** , majd a hello **időköz** mezőbe írja be **1**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-122">In hello **Frequency** box, select **Day** and then, in hello **Interval** box, type **1**.</span></span>

    ![Logic App Designer "Ismétlődési" ablak](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-application-insights-action"></a><span data-ttu-id="b6e0f-124">3. lépés: Új Application Insights művelet</span><span class="sxs-lookup"><span data-stu-id="b6e0f-124">Step 3: Add an Application Insights action</span></span>
1. <span data-ttu-id="b6e0f-125">Kattintson a **új lépés**, és kattintson a **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-125">Click **New step**, and then click **Add an action**.</span></span>

2. <span data-ttu-id="b6e0f-126">A hello **művelet kiválasztását** keresési mezőbe, írja be **Azure Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-126">In hello **Choose an action** search box, type **Azure Application Insights**.</span></span>

3. <span data-ttu-id="b6e0f-127">A **műveletek**, kattintson a **Azure Application Insights – megjelenítése Analytics lekérdezés előnézeti**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-127">Under **Actions**, click **Azure Application Insights – Visualize Analytics query Preview**.</span></span>

    ![Logic App Designer "Művelet kiválasztása" ablak](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-tooan-application-insights-resource"></a><span data-ttu-id="b6e0f-129">4. lépés: Csatlakozás tooan Application Insights-erőforrás</span><span class="sxs-lookup"><span data-stu-id="b6e0f-129">Step 4: Connect tooan Application Insights resource</span></span>

<span data-ttu-id="b6e0f-130">toocomplete ezt a lépést, az erőforrás Azonosítóját és API-kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-130">toocomplete this step, you need an application ID and an API key for your resource.</span></span> <span data-ttu-id="b6e0f-131">Helyreállíthatók a hello Azure-portálon a hello a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="b6e0f-131">You can retrieve them from hello Azure portal, as shown in hello following diagram:</span></span>

![Alkalmazás Azonosítójának hello Azure-portálon](./media/automate-with-logic-apps/appid.png) 

<span data-ttu-id="b6e0f-133">Adja meg a kapcsolat hello Alkalmazásazonosítót és hello API-kulcs nevét.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-133">Provide a name for your connection, hello application ID, and hello API key.</span></span>

![Logic App Tervező folyamata kapcsolat ablak](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-hello-analytics-query-and-chart-type"></a><span data-ttu-id="b6e0f-135">5. lépés: Adja meg a hello Analytics lekérdezési és diagramtípus</span><span class="sxs-lookup"><span data-stu-id="b6e0f-135">Step 5: Specify hello Analytics query and chart type</span></span>
<span data-ttu-id="b6e0f-136">A következő példa hello a hello lekérdezés kiválasztása hello sikertelen kérelmek hello utolsó napon belül, és korrelálja őket a kivételeket, amelyek hello művelet részeként történt.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-136">In hello following example, hello query selects hello failed requests within hello last day and correlates them with exceptions that occurred as part of hello operation.</span></span> <span data-ttu-id="b6e0f-137">Elemzés hello sikertelen kérelmek hello operation_Id azonosítója alapján ad eredményül.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-137">Analytics correlates hello failed requests, based on hello operation_Id identifier.</span></span> <span data-ttu-id="b6e0f-138">hello lekérdezés hello eredmények majd szegmensek hello autocluster algoritmus használatával.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-138">hello query then segments hello results by using hello autocluster algorithm.</span></span> 

<span data-ttu-id="b6e0f-139">A saját lekérdezések létrehozása, ellenőrizze, hogy megfelelően működnek az Analytics mielőtt hozzáadja tooyour folyamata.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-139">When you create your own queries, verify that they are working properly in Analytics before you add it tooyour flow.</span></span>

1. <span data-ttu-id="b6e0f-140">A hello **lekérdezés** mezőben adja meg a következő Analytics lekérdezési hello:</span><span class="sxs-lookup"><span data-stu-id="b6e0f-140">In hello **Query** box, add hello following Analytics query:</span></span> 

    ```
    requests
    | where timestamp > ago(1d)
    | where success == "False"
    | project name, operation_Id
    | join ( exceptions
        | project problemId, outerMessage, operation_Id
    ) on operation_Id
    | evaluate autocluster()
    ```

2. <span data-ttu-id="b6e0f-141">A hello **diagramtípus** mezőben válassza **Html tábla**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-141">In hello **Chart Type** box, select **Html Table**.</span></span>

    ![Elemzés lekérdezés konfigurációs ablak](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-hello-logic-app-toosend-email"></a><span data-ttu-id="b6e0f-143">6. lépés: Hello logic app toosend e-mail konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b6e0f-143">Step 6: Configure hello logic app toosend email</span></span>

1. <span data-ttu-id="b6e0f-144">Kattintson a **új lépés**, majd válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-144">Click **New step**, and then select **Add an action**.</span></span>

2. <span data-ttu-id="b6e0f-145">Hello keresési mezőbe, írja be a **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-145">In hello search box, type **Office 365 Outlook**.</span></span>

3. <span data-ttu-id="b6e0f-146">Kattintson a **Office 365 Outlook – az e-mailek küldése**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-146">Click **Office 365 Outlook – Send an email**.</span></span>

    ![Az Office 365 Outlook kiválasztása](./media/automate-with-logic-apps/flow2b.png)

4. <span data-ttu-id="b6e0f-148">A hello **egy e-mailek küldése** ablakban, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="b6e0f-148">In hello **Send an email** window, do hello following:</span></span>

   <span data-ttu-id="b6e0f-149">a.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-149">a.</span></span> <span data-ttu-id="b6e0f-150">Írja be a hello hello címzett e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-150">Type hello email address of hello recipient.</span></span>

   <span data-ttu-id="b6e0f-151">b.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-151">b.</span></span> <span data-ttu-id="b6e0f-152">Írja be az üdvözlő e-mail tárgyát.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-152">Type a subject for hello email.</span></span>

   <span data-ttu-id="b6e0f-153">c.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-153">c.</span></span> <span data-ttu-id="b6e0f-154">Kattintson bárhova hello **törzs** mezőbe, majd, hello dinamikus tartalom megnyíló menüben jobb hello: válasszon **törzs**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-154">Click anywhere in hello **Body** box and then, on hello dynamic content menu that opens at hello right, select **Body**.</span></span>

   <span data-ttu-id="b6e0f-155">d.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-155">d.</span></span> <span data-ttu-id="b6e0f-156">Kattintson a **speciális beállítások megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-156">Click **Show advanced options**.</span></span>

      ![Az Office 365 Outlook konfiguráció](./media/automate-with-logic-apps/flow5.png)

5. <span data-ttu-id="b6e0f-158">Az hello hello dinamikus tartalom menüben, a következő:</span><span class="sxs-lookup"><span data-stu-id="b6e0f-158">On hello dynamic content menu, do hello following:</span></span>

    <span data-ttu-id="b6e0f-159">a.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-159">a.</span></span> <span data-ttu-id="b6e0f-160">Válassza ki **melléklet neve**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-160">Select **Attachment Name**.</span></span>

    <span data-ttu-id="b6e0f-161">b.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-161">b.</span></span> <span data-ttu-id="b6e0f-162">Válassza ki **melléklet tartalom**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-162">Select **Attachment Content**.</span></span>
    
    <span data-ttu-id="b6e0f-163">c.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-163">c.</span></span> <span data-ttu-id="b6e0f-164">A hello **HTML** mezőben válassza **Igen**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-164">In hello **Is HTML** box, select **Yes**.</span></span>

      ![Az Office 365 e-mailek konfigurálására szolgáló képernyőn](./media/automate-with-logic-apps/flow7.png)

### <a name="step-7-save-and-test-your-logic-app"></a><span data-ttu-id="b6e0f-166">7. lépés: Mentéséhez, és tesztelje a Logic Apps alkalmazást</span><span class="sxs-lookup"><span data-stu-id="b6e0f-166">Step 7: Save and test your logic app</span></span>
* <span data-ttu-id="b6e0f-167">Kattintson a **mentése** toosave a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-167">Click **Save** toosave your changes.</span></span>

<span data-ttu-id="b6e0f-168">Hello eseményindító toorun hello logikai alkalmazás megvárhatja, vagy futtassa hello logikai alkalmazás azonnal kiválasztásával **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="b6e0f-168">You can wait for hello trigger toorun hello logic app, or you can run hello logic app immediately by selecting **Run**.</span></span>

![Logikai alkalmazás létrehozása képernyő](./media/automate-with-logic-apps/step7.png)

<span data-ttu-id="b6e0f-170">A logikai alkalmazás futtatásakor hello e-mail listában megadott hello címzettek megkapják az egy e-mailt, amely a következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="b6e0f-170">When your logic app runs, hello recipients you specified in hello email list will receive an email that looks like hello following:</span></span>

![Logic app e-mailt](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a><span data-ttu-id="b6e0f-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b6e0f-172">Next steps</span></span>

- <span data-ttu-id="b6e0f-173">További tudnivalók: létrehozása [elemzési lekérdezések](app-insights-analytics-using.md).</span><span class="sxs-lookup"><span data-stu-id="b6e0f-173">Learn more about creating [Analytics queries](app-insights-analytics-using.md).</span></span>
- <span data-ttu-id="b6e0f-174">További információ [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).</span><span class="sxs-lookup"><span data-stu-id="b6e0f-174">Learn more about [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).</span></span>



<!--Link references-->





