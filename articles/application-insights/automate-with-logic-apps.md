---
title: "Azure Application Insights-folyamatok automatizálása a Logic Apps segítségével."
description: "Ismerje meg, hogyan gyorsan automatizálhatja ismételhető folyamatokat a Logic Apps alkalmazást az Application Insights-összekötő hozzáadásával."
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
ms.openlocfilehash: 36df5bc0a019f4197d40fd6fa5a2a9957820c8b4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a><span data-ttu-id="6eb57-103">Az Application Insights-folyamatok automatizálása a Logic Apps segítségével</span><span class="sxs-lookup"><span data-stu-id="6eb57-103">Automate Application Insights processes by using Logic Apps</span></span>

<span data-ttu-id="6eb57-104">Tegye veszi észre magát a azonos lekérdezések ismételten futó ellenőrizze, hogy a szolgáltatás megfelelően működik-e a telemetriai adatok?</span><span class="sxs-lookup"><span data-stu-id="6eb57-104">Do you find yourself repeatedly running the same queries on your telemetry data to check whether your service is functioning properly?</span></span> <span data-ttu-id="6eb57-105">Van szüksége a lekérdezéseket, a trendek és rendellenességeket kereséshez automatizálását, és majd a felhasználókat ezekbe a csoportokba a saját munkafolyamatok építhetők fel?</span><span class="sxs-lookup"><span data-stu-id="6eb57-105">Are you looking to automate these queries for finding trends and anomalies and then build your own workflows around them?</span></span> <span data-ttu-id="6eb57-106">Az Azure Application Insights-összekötő (előzetes verzió) Logic Apps az ideális eszközt erre a célra.</span><span class="sxs-lookup"><span data-stu-id="6eb57-106">The Azure Application Insights connector (preview) for Logic Apps is the right tool for this purpose.</span></span>

<span data-ttu-id="6eb57-107">Ezt az integrációt a automatizálhat egysoros kód írása nélkül számos folyamatokat.</span><span class="sxs-lookup"><span data-stu-id="6eb57-107">With this integration, you can automate numerous processes without writing a single line of code.</span></span> <span data-ttu-id="6eb57-108">Egy logikai alkalmazást az Application Insights-összekötővel gyorsan a bármely Application Insights folyamat automatizálásához hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="6eb57-108">You can create a logic app with the Application Insights connector to quickly automate any Application Insights process.</span></span> 

<span data-ttu-id="6eb57-109">Hozzáadhat további műveleteket is.</span><span class="sxs-lookup"><span data-stu-id="6eb57-109">You can add additional actions as well.</span></span> <span data-ttu-id="6eb57-110">A Logic Apps szolgáltatás az Azure App Service elérhetővé több száz műveletek.</span><span class="sxs-lookup"><span data-stu-id="6eb57-110">The Logic Apps feature of Azure App Service makes hundreds of actions available.</span></span> <span data-ttu-id="6eb57-111">Például a logikai alkalmazás, automatikusan küldjön e-mailben értesítést vagy hozzon létre egy hibajelentést a Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="6eb57-111">For example, by using a logic app, you can automatically send an email notification or create a bug in Visual Studio Team Services.</span></span> <span data-ttu-id="6eb57-112">Is használhatja az egyik rendelkezésre álló több [sablonok](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) Ha szeretné felgyorsítani a logikai alkalmazás létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="6eb57-112">You can also use one of the many available [templates](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) to help speed up the process of creating your logic app.</span></span> 

## <a name="create-a-logic-app-for-application-insights"></a><span data-ttu-id="6eb57-113">Az Application Insights logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6eb57-113">Create a logic app for Application Insights</span></span>

<span data-ttu-id="6eb57-114">Ebben az oktatóanyagban elsajátíthatja, hogy a csoport attribútumok Analytics autocluster algoritmust használ az adatok webalkalmazás logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6eb57-114">In this tutorial, you learn how to create a logic app that uses the Analytics autocluster algorithm to group attributes in the data for a web application.</span></span> <span data-ttu-id="6eb57-115">A folyamat automatikusan elküldi az eredményeket e-mailben, hogyan használhatja az alkalmazásokhoz kapcsolódó elemzések elemzés és a Logic Apps együtt egy példát.</span><span class="sxs-lookup"><span data-stu-id="6eb57-115">The flow automatically sends the results by email, just one example of how you can use Application Insights Analytics and Logic Apps together.</span></span> 

### <a name="step-1-create-a-logic-app"></a><span data-ttu-id="6eb57-116">1. lépés: Logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6eb57-116">Step 1: Create a logic app</span></span>
1. <span data-ttu-id="6eb57-117">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6eb57-117">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6eb57-118">Az a **új** ablaktáblán válassza előbb **Web + mobil**, majd válassza ki **logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-118">In the **New** pane, select **Web + Mobile**, and then select **Logic App**.</span></span>

    ![Új logikai alkalmazás ablak](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a><span data-ttu-id="6eb57-120">2. lépés: A Logic Apps alkalmazást eseményindító létrehozása</span><span class="sxs-lookup"><span data-stu-id="6eb57-120">Step 2: Create a trigger for your logic app</span></span>
1. <span data-ttu-id="6eb57-121">Az a **Logic App Designer** ablakban, a **indítsa el az általános eseményindító**, jelölje be **ismétlődési**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-121">In the **Logic App Designer** window, under **Start with a common trigger**, select **Recurrence**.</span></span>

    ![Logic App-Tervező ablak](./media/automate-with-logic-apps/logicapp2.png)

2. <span data-ttu-id="6eb57-123">Az a **gyakoriság** mezőben válassza **nap** , majd a a **időköz** mezőbe írja be **1**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-123">In the **Frequency** box, select **Day** and then, in the **Interval** box, type **1**.</span></span>

    ![Logic App Designer "Ismétlődési" ablak](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-application-insights-action"></a><span data-ttu-id="6eb57-125">3. lépés: Új Application Insights művelet</span><span class="sxs-lookup"><span data-stu-id="6eb57-125">Step 3: Add an Application Insights action</span></span>
1. <span data-ttu-id="6eb57-126">Kattintson a **új lépés**, és kattintson a **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-126">Click **New step**, and then click **Add an action**.</span></span>

2. <span data-ttu-id="6eb57-127">Az a **művelet kiválasztását** keresési mezőbe, írja be **Azure Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-127">In the **Choose an action** search box, type **Azure Application Insights**.</span></span>

3. <span data-ttu-id="6eb57-128">A **műveletek**, kattintson a **Azure Application Insights – megjelenítése Analytics lekérdezés előnézeti**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-128">Under **Actions**, click **Azure Application Insights – Visualize Analytics query Preview**.</span></span>

    ![Logic App Designer "Művelet kiválasztása" ablak](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-to-an-application-insights-resource"></a><span data-ttu-id="6eb57-130">4. lépés: Csatlakozás az Application Insights-erőforrás</span><span class="sxs-lookup"><span data-stu-id="6eb57-130">Step 4: Connect to an Application Insights resource</span></span>

<span data-ttu-id="6eb57-131">E lépés elvégzése után az alkalmazás Azonosítóját és API-kulcs az erőforrás kell.</span><span class="sxs-lookup"><span data-stu-id="6eb57-131">To complete this step, you need an application ID and an API key for your resource.</span></span> <span data-ttu-id="6eb57-132">Helyreállíthatók Azure-portálról, az alábbi ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="6eb57-132">You can retrieve them from the Azure portal, as shown in the following diagram:</span></span>

![Alkalmazás azonosítója az Azure-portálon](./media/automate-with-logic-apps/appid.png) 

<span data-ttu-id="6eb57-134">Adjon meg egy nevet a kapcsolatot, az alkalmazás Azonosítóját és az API-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="6eb57-134">Provide a name for your connection, the application ID, and the API key.</span></span>

![Logic App Tervező folyamata kapcsolat ablak](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a><span data-ttu-id="6eb57-136">5. lépés: Adja meg a Analytics lekérdezés és a diagram típusát</span><span class="sxs-lookup"><span data-stu-id="6eb57-136">Step 5: Specify the Analytics query and chart type</span></span>
<span data-ttu-id="6eb57-137">A következő példában a lekérdezés a sikertelen kérelmek kiválasztja a legutóbbi napon belül, és korrelálja őket a kivételeket, amelyek a művelet részeként történt.</span><span class="sxs-lookup"><span data-stu-id="6eb57-137">In the following example, the query selects the failed requests within the last day and correlates them with exceptions that occurred as part of the operation.</span></span> <span data-ttu-id="6eb57-138">Elemzés felel meg a sikertelen kérelmek, operation_Id azonosítója alapján.</span><span class="sxs-lookup"><span data-stu-id="6eb57-138">Analytics correlates the failed requests, based on the operation_Id identifier.</span></span> <span data-ttu-id="6eb57-139">A lekérdezés eredménye majd felosztja a autocluster algoritmus használatával.</span><span class="sxs-lookup"><span data-stu-id="6eb57-139">The query then segments the results by using the autocluster algorithm.</span></span> 

<span data-ttu-id="6eb57-140">A saját lekérdezések létrehozása, ellenőrizze, hogy megfelelően működnek az Analytics csak a folyamat hozzáadni azt.</span><span class="sxs-lookup"><span data-stu-id="6eb57-140">When you create your own queries, verify that they are working properly in Analytics before you add it to your flow.</span></span>

1. <span data-ttu-id="6eb57-141">Az a **lekérdezés** mezőben adja meg a következő Analytics-lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="6eb57-141">In the **Query** box, add the following Analytics query:</span></span> 

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

2. <span data-ttu-id="6eb57-142">Az a **diagramtípus** mezőben válassza **Html tábla**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-142">In the **Chart Type** box, select **Html Table**.</span></span>

    ![Elemzés lekérdezés konfigurációs ablak](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-the-logic-app-to-send-email"></a><span data-ttu-id="6eb57-144">6. lépés: az e-mail üzenetek küldéséhez a logikai alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6eb57-144">Step 6: Configure the logic app to send email</span></span>

1. <span data-ttu-id="6eb57-145">Kattintson a **új lépés**, majd válassza ki **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-145">Click **New step**, and then select **Add an action**.</span></span>

2. <span data-ttu-id="6eb57-146">Írja be a keresőmezőbe, **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-146">In the search box, type **Office 365 Outlook**.</span></span>

3. <span data-ttu-id="6eb57-147">Kattintson a **Office 365 Outlook – az e-mailek küldése**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-147">Click **Office 365 Outlook – Send an email**.</span></span>

    ![Az Office 365 Outlook kiválasztása](./media/automate-with-logic-apps/flow2b.png)

4. <span data-ttu-id="6eb57-149">Az a **egy e-mailek küldése** ablakban tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="6eb57-149">In the **Send an email** window, do the following:</span></span>

   <span data-ttu-id="6eb57-150">a.</span><span class="sxs-lookup"><span data-stu-id="6eb57-150">a.</span></span> <span data-ttu-id="6eb57-151">Írja be a címzett e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="6eb57-151">Type the email address of the recipient.</span></span>

   <span data-ttu-id="6eb57-152">b.</span><span class="sxs-lookup"><span data-stu-id="6eb57-152">b.</span></span> <span data-ttu-id="6eb57-153">Írja be az e-mail tárgyát.</span><span class="sxs-lookup"><span data-stu-id="6eb57-153">Type a subject for the email.</span></span>

   <span data-ttu-id="6eb57-154">c.</span><span class="sxs-lookup"><span data-stu-id="6eb57-154">c.</span></span> <span data-ttu-id="6eb57-155">Kattintson bárhova a **törzs** mezőbe, majd válassza a dinamikus tartalom megnyíló menüben jobb **törzs**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-155">Click anywhere in the **Body** box and then, on the dynamic content menu that opens at the right, select **Body**.</span></span>

   <span data-ttu-id="6eb57-156">d.</span><span class="sxs-lookup"><span data-stu-id="6eb57-156">d.</span></span> <span data-ttu-id="6eb57-157">Kattintson a **speciális beállítások megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-157">Click **Show advanced options**.</span></span>

      ![Az Office 365 Outlook konfiguráció](./media/automate-with-logic-apps/flow5.png)

5. <span data-ttu-id="6eb57-159">A dinamikus tartalom menü tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="6eb57-159">On the dynamic content menu, do the following:</span></span>

    <span data-ttu-id="6eb57-160">a.</span><span class="sxs-lookup"><span data-stu-id="6eb57-160">a.</span></span> <span data-ttu-id="6eb57-161">Válassza ki **melléklet neve**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-161">Select **Attachment Name**.</span></span>

    <span data-ttu-id="6eb57-162">b.</span><span class="sxs-lookup"><span data-stu-id="6eb57-162">b.</span></span> <span data-ttu-id="6eb57-163">Válassza ki **melléklet tartalom**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-163">Select **Attachment Content**.</span></span>
    
    <span data-ttu-id="6eb57-164">c.</span><span class="sxs-lookup"><span data-stu-id="6eb57-164">c.</span></span> <span data-ttu-id="6eb57-165">Az a **HTML** mezőben válassza **Igen**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-165">In the **Is HTML** box, select **Yes**.</span></span>

      ![Az Office 365 e-mailek konfigurálására szolgáló képernyőn](./media/automate-with-logic-apps/flow7.png)

### <a name="step-7-save-and-test-your-logic-app"></a><span data-ttu-id="6eb57-167">7. lépés: Mentéséhez, és tesztelje a Logic Apps alkalmazást</span><span class="sxs-lookup"><span data-stu-id="6eb57-167">Step 7: Save and test your logic app</span></span>
* <span data-ttu-id="6eb57-168">Kattintson a **mentése** menti a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="6eb57-168">Click **Save** to save your changes.</span></span>

<span data-ttu-id="6eb57-169">A logikai alkalmazás működéséhez megvárhatja, vagy futtassa a logikai alkalmazás azonnal kiválasztásával **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="6eb57-169">You can wait for the trigger to run the logic app, or you can run the logic app immediately by selecting **Run**.</span></span>

![Logikai alkalmazás létrehozása képernyő](./media/automate-with-logic-apps/step7.png)

<span data-ttu-id="6eb57-171">Amikor a Logic Apps alkalmazást futtat, a címzettek, az e-mailek listában megadott kap egy e-mailt, amely a következőképpen néznek:</span><span class="sxs-lookup"><span data-stu-id="6eb57-171">When your logic app runs, the recipients you specified in the email list will receive an email that looks like the following:</span></span>

![Logic app e-mailt](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a><span data-ttu-id="6eb57-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6eb57-173">Next steps</span></span>

- <span data-ttu-id="6eb57-174">További tudnivalók: létrehozása [elemzési lekérdezések](app-insights-analytics-using.md).</span><span class="sxs-lookup"><span data-stu-id="6eb57-174">Learn more about creating [Analytics queries](app-insights-analytics-using.md).</span></span>
- <span data-ttu-id="6eb57-175">További információ [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).</span><span class="sxs-lookup"><span data-stu-id="6eb57-175">Learn more about [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).</span></span>



<!--Link references-->





