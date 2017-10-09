---
title: aaaAutomate Azure Application Insights dolgozza fel a Microsoft Flow
description: "Ismerje meg, hogyan használhatja a Microsoft Flow tooquickly ismételhető folyamatok automatizálása hello Application Insights-összekötő használatával."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: bwren
ms.openlocfilehash: b34488a6b8b8b0a6add960a67f1426cbbbc13552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-azure-application-insights-processes-with-hello-connector-for-microsoft-flow"></a><span data-ttu-id="299b0-103">Hello Connector Azure Application Insights-folyamatok automatizálása a Microsoft Flow</span><span class="sxs-lookup"><span data-stu-id="299b0-103">Automate Azure Application Insights processes with hello connector for Microsoft Flow</span></span>

<span data-ttu-id="299b0-104">Tegye veszi észre magát ismételten futó hello azonos lekérdezések a telemetriai adatok toocheck, hogy a szolgáltatás megfelelően működik?</span><span class="sxs-lookup"><span data-stu-id="299b0-104">Do you find yourself repeatedly running hello same queries on your telemetry data toocheck that your service is functioning properly?</span></span> <span data-ttu-id="299b0-105">Kinézetű tooautomate ezeket a lekérdezéseket, a trendek és rendellenességeket kereséséhez és majd a felhasználókat ezekbe a csoportokba a saját munkafolyamatok építhetők fel? hello Azure Application Insights-összekötő (előzetes verzió) Microsoft Flow egy hello megfelelő eszköz ezekből a célokból.</span><span class="sxs-lookup"><span data-stu-id="299b0-105">Are you looking tooautomate these queries for finding trends and anomalies and then build your own workflows around them? hello Azure Application Insights connector (preview) for Microsoft Flow is hello right tool for these purposes.</span></span>

<span data-ttu-id="299b0-106">Ez az integráció mostantól automatizálhatja számos folyamatok egysoros kód írása nélkül.</span><span class="sxs-lookup"><span data-stu-id="299b0-106">With this integration, you can now automate numerous processes without writing a single line of code.</span></span> <span data-ttu-id="299b0-107">Az Application Insights műveletet hoz létre egy folyamatot, miután hello folyamat automatikusan futtatja az Application Insights Analytics lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="299b0-107">After you create a flow by using an Application Insights action, hello flow automatically runs your Application Insights Analytics query.</span></span> 

<span data-ttu-id="299b0-108">Hozzáadhat további műveleteket is.</span><span class="sxs-lookup"><span data-stu-id="299b0-108">You can add additional actions as well.</span></span> <span data-ttu-id="299b0-109">Microsoft Flow elérhetővé több száz műveletek.</span><span class="sxs-lookup"><span data-stu-id="299b0-109">Microsoft Flow makes hundreds of actions available.</span></span> <span data-ttu-id="299b0-110">Például Microsoft Flow tooautomatically küldése e-mailben értesítést használja, vagy hozzon létre egy hibajelentést a Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="299b0-110">For example, you can use Microsoft Flow tooautomatically send an email notification or create a bug in Visual Studio Team Services.</span></span> <span data-ttu-id="299b0-111">Is használhatja a hello számos [sablonok](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) hello Microsoft Flow-összekötő elérhető.</span><span class="sxs-lookup"><span data-stu-id="299b0-111">You can also use one of hello many [templates](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) that are available for hello connector for Microsoft Flow.</span></span> <span data-ttu-id="299b0-112">Ezeket a sablonokat a folyamat létrehozásának folyamatán hello felgyorsításában.</span><span class="sxs-lookup"><span data-stu-id="299b0-112">These templates speed up hello process of creating a flow.</span></span> 

<!--hello Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="create-a-flow-for-application-insights"></a><span data-ttu-id="299b0-113">Hozzon létre egy folyamatot az Application Insights</span><span class="sxs-lookup"><span data-stu-id="299b0-113">Create a flow for Application Insights</span></span>

<span data-ttu-id="299b0-114">Ebből az oktatóanyagból megtudhatja, hogyan toocreate a folyamat által használt hello Analytics automatikus-fürt algoritmus toogroup attribútumok webalkalmazás hello adataiban.</span><span class="sxs-lookup"><span data-stu-id="299b0-114">In this tutorial, you will learn how toocreate a flow that uses hello Analytics auto-cluster algorithm toogroup attributes in hello data for a web application.</span></span> <span data-ttu-id="299b0-115">hello folyamata hello eredmények automatikusan elküldi e-mailben, hogyan használhatja a Microsoft Flow és az Application Insights Analytics együtt egy példát.</span><span class="sxs-lookup"><span data-stu-id="299b0-115">hello flow automatically sends hello results by email, just one example of how you can use Microsoft Flow and Application Insights Analytics together.</span></span> 

### <a name="step-1-create-a-flow"></a><span data-ttu-id="299b0-116">1. lépés: A folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="299b0-116">Step 1: Create a flow</span></span>
1. <span data-ttu-id="299b0-117">Jelentkezzen be a túl[Microsoft Flow](http://flow.microsoft.com), majd válassza ki **saját Forgalomáramlás**.</span><span class="sxs-lookup"><span data-stu-id="299b0-117">Sign in too[Microsoft Flow](http://flow.microsoft.com), and then select **My Flows**.</span></span>
2. <span data-ttu-id="299b0-118">Kattintson a **a folyamat létrehozása üres**.</span><span class="sxs-lookup"><span data-stu-id="299b0-118">Click **Create a flow from blank**.</span></span>

### <a name="step-2-create-a-trigger-for-your-flow"></a><span data-ttu-id="299b0-119">2. lépés: A folyamat eseményindító létrehozása</span><span class="sxs-lookup"><span data-stu-id="299b0-119">Step 2: Create a trigger for your flow</span></span>
1. <span data-ttu-id="299b0-120">Válassza ki **ütemezés**, majd válassza ki **ütemezés - ismétlődési**.</span><span class="sxs-lookup"><span data-stu-id="299b0-120">Select **Schedule**, and then select **Schedule - Recurrence**.</span></span>
2. <span data-ttu-id="299b0-121">A hello **gyakoriság** mezőben válassza **nap**, és a hello **időköz** adja meg a **1**.</span><span class="sxs-lookup"><span data-stu-id="299b0-121">In hello **Frequency** box, select **Day**, and in hello **Interval** box, enter **1**.</span></span>

    ![Microsoft Flow eseményindító párbeszédpanel](./media/app-insights-automate-with-flow/flow1.png)


### <a name="step-3-add-an-application-insights-action"></a><span data-ttu-id="299b0-123">3. lépés: Új Application Insights művelet</span><span class="sxs-lookup"><span data-stu-id="299b0-123">Step 3: Add an Application Insights action</span></span>
1. <span data-ttu-id="299b0-124">Kattintson a **új lépés**, és kattintson a **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="299b0-124">Click **New step**, and then click **Add an action**.</span></span>
2. <span data-ttu-id="299b0-125">Keresse meg **Azure Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="299b0-125">Search for **Azure Application Insights**.</span></span>
3. <span data-ttu-id="299b0-126">Kattintson a **Azure Application Insights – megjelenítése Analytics lekérdezés előnézeti**.</span><span class="sxs-lookup"><span data-stu-id="299b0-126">Click **Azure Application Insights – Visualize Analytics query Preview**.</span></span>

    ![Analytics lekérdezési ablakban futtassa](./media/app-insights-automate-with-flow/flow2.png)

### <a name="step-4-connect-tooan-application-insights-resource"></a><span data-ttu-id="299b0-128">4. lépés: Csatlakozás tooan Application Insights-erőforrás</span><span class="sxs-lookup"><span data-stu-id="299b0-128">Step 4: Connect tooan Application Insights resource</span></span>

<span data-ttu-id="299b0-129">toocomplete ezt a lépést, az erőforrás Azonosítóját és API-kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="299b0-129">toocomplete this step, you need an application ID and an API key for your resource.</span></span> <span data-ttu-id="299b0-130">Helyreállíthatók a hello Azure-portálon a hello a következő ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="299b0-130">You can retrieve them from hello Azure portal, as shown in hello following diagram:</span></span>

![Alkalmazás Azonosítójának hello Azure-portálon](./media/app-insights-automate-with-flow/appid.png) 

- <span data-ttu-id="299b0-132">Adja meg a hálózati kapcsolatot, valamint hello alkalmazás azonosítója és API-kulcs nevét.</span><span class="sxs-lookup"><span data-stu-id="299b0-132">Provide a name for your connection, along with hello application ID and API key.</span></span>

    ![Microsoft Flow kapcsolat ablak](./media/app-insights-automate-with-flow/flow3.png)

### <a name="step-5-specify-hello-analytics-query-and-chart-type"></a><span data-ttu-id="299b0-134">5. lépés: Adja meg a hello Analytics lekérdezési és diagramtípus</span><span class="sxs-lookup"><span data-stu-id="299b0-134">Step 5: Specify hello Analytics query and chart type</span></span>
<span data-ttu-id="299b0-135">Ez a példa lekérdezés kiválasztása hello sikertelen kérelmek hello utolsó napon belül, és korrelálja őket a kivételeket, amelyek hello művelet részeként történt.</span><span class="sxs-lookup"><span data-stu-id="299b0-135">This example query selects hello failed requests within hello last day and correlates them with exceptions that occurred as part of hello operation.</span></span> <span data-ttu-id="299b0-136">Elemzés korrelálja őket hello operation_Id azonosítója alapján.</span><span class="sxs-lookup"><span data-stu-id="299b0-136">Analytics correlates them based on hello operation_Id identifier.</span></span> <span data-ttu-id="299b0-137">hello lekérdezés hello eredmények majd szegmensek hello autocluster algoritmus használatával.</span><span class="sxs-lookup"><span data-stu-id="299b0-137">hello query then segments hello results by using hello autocluster algorithm.</span></span> 

<span data-ttu-id="299b0-138">A saját lekérdezések létrehozása, ellenőrizze, hogy megfelelően működnek az Analytics mielőtt hozzáadja tooyour folyamata.</span><span class="sxs-lookup"><span data-stu-id="299b0-138">When you create your own queries, verify that they are working properly in Analytics before you add it tooyour flow.</span></span>

- <span data-ttu-id="299b0-139">Adja hozzá a következő Analytics lekérdezési hello, és válassza a hello HTML tábla diagram típusát.</span><span class="sxs-lookup"><span data-stu-id="299b0-139">Add hello following Analytics query, and then select hello HTML table chart type.</span></span> 

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
    
    ![Elemzés lekérdezés konfigurációs ablak](./media/app-insights-automate-with-flow/flow4.png)

### <a name="step-6-configure-hello-flow-toosend-email"></a><span data-ttu-id="299b0-141">6. lépés: Hello folyamat toosend e-mail konfigurálása</span><span class="sxs-lookup"><span data-stu-id="299b0-141">Step 6: Configure hello flow toosend email</span></span>

1. <span data-ttu-id="299b0-142">Kattintson a **új lépés**, és kattintson a **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="299b0-142">Click **New step**, and then click **Add an action**.</span></span>
2. <span data-ttu-id="299b0-143">Keresse meg **Office 365 Outlook**.</span><span class="sxs-lookup"><span data-stu-id="299b0-143">Search for **Office 365 Outlook**.</span></span>
3. <span data-ttu-id="299b0-144">Kattintson a **Office 365 Outlook – az e-mailek küldése**.</span><span class="sxs-lookup"><span data-stu-id="299b0-144">Click **Office 365 Outlook – Send an email**.</span></span>

    ![Az Office 365 Outlook kiválasztási ablaka](./media/app-insights-automate-with-flow/flow2b.png)

4. <span data-ttu-id="299b0-146">A hello **egy e-mailek küldése** ablakban, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="299b0-146">In hello **Send an email** window, do hello following:</span></span>

   <span data-ttu-id="299b0-147">a.</span><span class="sxs-lookup"><span data-stu-id="299b0-147">a.</span></span> <span data-ttu-id="299b0-148">Írja be a hello hello címzett e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="299b0-148">Type hello email address of hello recipient.</span></span>

   <span data-ttu-id="299b0-149">b.</span><span class="sxs-lookup"><span data-stu-id="299b0-149">b.</span></span> <span data-ttu-id="299b0-150">Írja be az üdvözlő e-mail tárgyát.</span><span class="sxs-lookup"><span data-stu-id="299b0-150">Type a subject for hello email.</span></span>

   <span data-ttu-id="299b0-151">c.</span><span class="sxs-lookup"><span data-stu-id="299b0-151">c.</span></span> <span data-ttu-id="299b0-152">Kattintson bárhova hello **törzs** mezőbe, majd, hello dinamikus tartalom megnyíló menüben jobb hello: válasszon **törzs**.</span><span class="sxs-lookup"><span data-stu-id="299b0-152">Click anywhere in hello **Body** box and then, on hello dynamic content menu that opens at hello right, select **Body**.</span></span>

   <span data-ttu-id="299b0-153">d.</span><span class="sxs-lookup"><span data-stu-id="299b0-153">d.</span></span> <span data-ttu-id="299b0-154">Kattintson a **speciális beállítások megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="299b0-154">Click **Show advanced options**.</span></span>

    ![Az Office 365 Outlook konfiguráció](./media/app-insights-automate-with-flow/flow5.png)

5. <span data-ttu-id="299b0-156">Az hello hello dinamikus tartalom menüben, a következő:</span><span class="sxs-lookup"><span data-stu-id="299b0-156">On hello dynamic content menu, do hello following:</span></span>

    <span data-ttu-id="299b0-157">a.</span><span class="sxs-lookup"><span data-stu-id="299b0-157">a.</span></span> <span data-ttu-id="299b0-158">Válassza ki **melléklet neve**.</span><span class="sxs-lookup"><span data-stu-id="299b0-158">Select **Attachment Name**.</span></span>

    <span data-ttu-id="299b0-159">b.</span><span class="sxs-lookup"><span data-stu-id="299b0-159">b.</span></span> <span data-ttu-id="299b0-160">Válassza ki **melléklet tartalom**.</span><span class="sxs-lookup"><span data-stu-id="299b0-160">Select **Attachment Content**.</span></span>
    
    <span data-ttu-id="299b0-161">c.</span><span class="sxs-lookup"><span data-stu-id="299b0-161">c.</span></span> <span data-ttu-id="299b0-162">A hello **HTML** mezőben válassza **Igen**.</span><span class="sxs-lookup"><span data-stu-id="299b0-162">In hello **Is HTML** box, select **Yes**.</span></span>

    ![Az Office 365 e-mailek konfigurációs ablak](./media/app-insights-automate-with-flow/flow7.png)

### <a name="step-7-save-and-test-your-flow"></a><span data-ttu-id="299b0-164">7. lépés: Mentéséhez és a folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="299b0-164">Step 7: Save and test your flow</span></span>
- <span data-ttu-id="299b0-165">A hello **folyamat nevének** mezőbe, vegye fel a folyamat nevét, majd kattintson **folyamat létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="299b0-165">In hello **Flow name** box, add a name for your flow, and then click **Create flow**.</span></span>

    ![Adatfolyam-létrehozási ablak](./media/app-insights-automate-with-flow/flow8.png)

<span data-ttu-id="299b0-167">Megvárhatja hello eseményindító toorun ezt a műveletet, vagy hello folyamata, azonnal futtathatja [futó hello eseményindító az igény szerinti](https://flow.microsoft.com/blog/run-now-and-six-more-services/).</span><span class="sxs-lookup"><span data-stu-id="299b0-167">You can wait for hello trigger toorun this action, or you can run hello flow immediately by [running hello trigger on demand](https://flow.microsoft.com/blog/run-now-and-six-more-services/).</span></span>

<span data-ttu-id="299b0-168">Amikor hello folyamat fut, a hello e-mail listában megadott hello címzettek kapnak e-mailt, amely a következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="299b0-168">When hello flow runs, hello recipients you have specified in hello email list receive an email message that looks like hello following:</span></span>

![Példa e-mailre](./media/app-insights-automate-with-flow/flow9.png)


## <a name="next-steps"></a><span data-ttu-id="299b0-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="299b0-170">Next steps</span></span>

- <span data-ttu-id="299b0-171">További tudnivalók: létrehozása [elemzési lekérdezések](app-insights-analytics-using.md).</span><span class="sxs-lookup"><span data-stu-id="299b0-171">Learn more about creating [Analytics queries](app-insights-analytics-using.md).</span></span>
- <span data-ttu-id="299b0-172">További információ [Microsoft Flow](https://ms.flow.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="299b0-172">Learn more about [Microsoft Flow](https://ms.flow.microsoft.com).</span></span>



<!--Link references-->





