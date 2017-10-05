---
title: "A logikai alkalmazás figyelése és a get észrevételeket fusson, OMS - Azure Logic Apps |} Microsoft Docs"
description: "A logic app fut, Naplóelemzés és az Operations Management Suite (OMS) insights és gazdagabb hibakeresési adatainak lekérése – hibaelhárítás és diagnosztika figyelése"
author: divyaswarnkar
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/9/2017
ms.author: LADocs; divswa
ms.openlocfilehash: 0e9f0ef3c87b5c0da1cc4ad16d37178c8f5c9625
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-operations-management-suite-oms-and-log-analytics"></a><span data-ttu-id="4b389-103">Logikai alkalmazás figyelése és a get észrevételeket fut, az Operations Management Suite (OMS) és a Naplóelemzési</span><span class="sxs-lookup"><span data-stu-id="4b389-103">Monitor and get insights about logic app runs with Operations Management Suite (OMS) and Log Analytics</span></span>

<span data-ttu-id="4b389-104">Figyelési és gazdagabb hibakeresési információ bekapcsolása Naplóelemzési logikai alkalmazás létrehozásakor egy időben.</span><span class="sxs-lookup"><span data-stu-id="4b389-104">For monitoring and richer debugging information, you can turn on Log Analytics at the same time when you create a logic app.</span></span> <span data-ttu-id="4b389-105">A Naplóelemzési biztosít naplózásának és figyelésének a logikai alkalmazásnak diagnosztika futtatása az Operations Management Suite (OMS) portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="4b389-105">Log Analytics provides diagnostics logging and monitoring for your logic app runs through the Operations Management Suite (OMS) portal.</span></span> <span data-ttu-id="4b389-106">A Logic Apps-kezelési megoldás az OMS-be való hozzáadásakor a logic app futtatása és a kívánt részletes adatok, például állapot, a végrehajtási idő, a ismételt továbbítása során állapot és a korrelációs azonosító lekérése összesített állapotát.</span><span class="sxs-lookup"><span data-stu-id="4b389-106">When you add the Logic Apps Management solution to OMS, you get aggregated status for your logic app runs and specific details like status, execution time, resubmission status, and correlation IDs.</span></span>

<span data-ttu-id="4b389-107">Ez a témakör bemutatja, hogyan Naplóelemzési be-és a Logic Apps-kezelési megoldás telepítése OMS, tekintse meg a futtatókörnyezet események és az adatok a Logic Apps alkalmazást futtatni.</span><span class="sxs-lookup"><span data-stu-id="4b389-107">This topic shows how to turn on Log Analytics or install the Logic Apps Management solution in OMS so you can view runtime events and data for your logic app run.</span></span>

 > [!TIP]
 > <span data-ttu-id="4b389-108">A meglévő logic Apps alkalmazások figyeléséhez, az alábbi lépéseket követve [diagnosztikai naplózás bekapcsolásához és a logic app futásidejű adatokat küldeni a OMS](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span><span class="sxs-lookup"><span data-stu-id="4b389-108">To monitor your existing logic apps, follow these steps to [turn on diagnostic logging and send logic app runtime data to OMS](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span></span>

## <a name="requirements"></a><span data-ttu-id="4b389-109">Követelmények</span><span class="sxs-lookup"><span data-stu-id="4b389-109">Requirements</span></span>

<span data-ttu-id="4b389-110">Kezdés előtt kell az OMS-munkaterület rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4b389-110">Before you start, you need to have an OMS workspace.</span></span> <span data-ttu-id="4b389-111">Ismerje meg, [OMS-munkaterület létrehozása](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4b389-111">Learn [how to create an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span> 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a><span data-ttu-id="4b389-112">A logic apps létrehozásakor diagnosztikai naplózás bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="4b389-112">Turn on diagnostics logging when creating logic apps</span></span>

1. <span data-ttu-id="4b389-113">A [Azure-portálon](https://portal.azure.com), logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4b389-113">In [Azure portal](https://portal.azure.com), create a logic app.</span></span> <span data-ttu-id="4b389-114">Válasszon **új** > **vállalati integrációs** > **logikai alkalmazás** > **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="4b389-114">Choose **New** > **Enterprise Integration** > **Logic App** > **Create**.</span></span>

   ![Logikai alkalmazás létrehozása](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

2. <span data-ttu-id="4b389-116">Az a **hozzon létre logikai alkalmazás** lapján látható ezen feladatok végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="4b389-116">In the **Create logic app** page, perform these tasks as shown:</span></span>

   1. <span data-ttu-id="4b389-117">Adjon meg egy nevet a Logic Apps alkalmazást, és válassza ki az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="4b389-117">Provide a name for your logic app and select your Azure subscription.</span></span> 
   2. <span data-ttu-id="4b389-118">Hozzon létre vagy válasszon ki egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="4b389-118">Create or select an Azure resource group.</span></span>
   3. <span data-ttu-id="4b389-119">Állítsa be **Analytics jelentkezzen** való **a**.</span><span class="sxs-lookup"><span data-stu-id="4b389-119">Set **Log Analytics** to **On**.</span></span> 
   <span data-ttu-id="4b389-120">Válassza ki az OMS-munkaterület, ahol szeretné elküldeni a adatait a Logic Apps alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="4b389-120">Select the OMS workspace where you want to send data for your logic app runs.</span></span> 
   4. <span data-ttu-id="4b389-121">Ha elkészült, válassza ki a **rögzítés az irányítópulton** > **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="4b389-121">When you're ready, choose **Pin to dashboard** > **Create**.</span></span>

      ![Logikai alkalmazás létrehozása](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      <span data-ttu-id="4b389-123">Ez a lépés befejezése után az Azure létrehoz a logikai alkalmazás, amely most már az OMS-munkaterület társított.</span><span class="sxs-lookup"><span data-stu-id="4b389-123">After you finish this step, Azure creates your logic app, which is now associated with your OMS workspace.</span></span> 
      <span data-ttu-id="4b389-124">Emellett ebben a lépésben is automatikusan telepíti a Logic Apps-kezelési megoldás az OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="4b389-124">Also, this step also automatically installs the Logic Apps Management solution in your OMS workspace.</span></span>

3. <span data-ttu-id="4b389-125">Megtekintheti a logic app futó OMS-ben, [folytassa a következő lépéseket](#view-logic-app-runs-oms).</span><span class="sxs-lookup"><span data-stu-id="4b389-125">To view your logic app runs in OMS, [continue with these steps](#view-logic-app-runs-oms).</span></span>

## <a name="install-the-logic-apps-management-solution-in-oms"></a><span data-ttu-id="4b389-126">Az OMS a Logic Apps-kezelési megoldás telepítése</span><span class="sxs-lookup"><span data-stu-id="4b389-126">Install the Logic Apps Management solution in OMS</span></span>

<span data-ttu-id="4b389-127">Ha Ön már engedélyezve van a Naplóelemzési a logikai alkalmazás létrehozása után, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="4b389-127">If you already turned on Log Analytics when you created your logic app, skip this step.</span></span> <span data-ttu-id="4b389-128">Már van a Logic Apps felügyeleti megoldás, OMS telepítve.</span><span class="sxs-lookup"><span data-stu-id="4b389-128">You already have the Logic Apps Management solution installed in OMS.</span></span>

1. <span data-ttu-id="4b389-129">Az a [Azure-portálon](https://portal.azure.com), válassza a **több szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="4b389-129">In the [Azure portal](https://portal.azure.com), choose **More Services**.</span></span> <span data-ttu-id="4b389-130">Keresse meg a "naplóelemzési" szűrőként, és válassza a **Naplóelemzési** látható módon:</span><span class="sxs-lookup"><span data-stu-id="4b389-130">Search for "log analytics" as your filter, and choose **Log Analytics** as shown:</span></span>

   ![Válassza ki a "Naplóelemzési"](media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

2. <span data-ttu-id="4b389-132">A **Naplóelemzési**, található, és válassza ki az OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="4b389-132">Under **Log Analytics**, find and select your OMS workspace.</span></span> 

   ![Az OMS-munkaterület kiválasztása](media/logic-apps-monitor-your-logic-apps-oms/select-logic-app.png)

3. <span data-ttu-id="4b389-134">A **felügyeleti**, válassza a **OMS-portálon**.</span><span class="sxs-lookup"><span data-stu-id="4b389-134">Under **Management**, choose **OMS Portal**.</span></span>

   ![Válassza ki a "OMS-portálon"](media/logic-apps-monitor-your-logic-apps-oms/oms-portal-page.png)

4. <span data-ttu-id="4b389-136">A kezdőlapon OMS a frissítési szalagcím akkor jelenik meg, ha válassza ki a szalagcím, hogy az OMS-munkaterület először frissítenie.</span><span class="sxs-lookup"><span data-stu-id="4b389-136">On your OMS homepage, if the upgrade banner appears, choose the banner so that you upgrade your OMS workspace first.</span></span> <span data-ttu-id="4b389-137">Válassza a **megoldások gyűjtemény**.</span><span class="sxs-lookup"><span data-stu-id="4b389-137">Then choose **Solutions Gallery**.</span></span>

   ![Válassza ki a "Megoldások gyűjtemény"](media/logic-apps-monitor-your-logic-apps-oms/solutions-gallery.png)

5. <span data-ttu-id="4b389-139">A **minden megoldás**, található, és válassza ki a csempe a **Logic Apps felügyeleti** megoldás.</span><span class="sxs-lookup"><span data-stu-id="4b389-139">Under **All solutions**, find and choose the tile for the **Logic Apps Management** solution.</span></span>

   ![Válassza ki a "Logic Apps kezelése"](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-management-tile2.png)

6. <span data-ttu-id="4b389-141">Az OMS-munkaterület a megoldás telepítéséhez válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4b389-141">To install the solution in your OMS workspace, choose **Add**.</span></span>

   ![Válassza a "Hozzáadás" a "Logic Apps kezelése"](media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-your-logic-app-runs-in-your-oms-workspace"></a><span data-ttu-id="4b389-143">A Logic Apps alkalmazást futtat az OMS-munkaterület megjelenítése</span><span class="sxs-lookup"><span data-stu-id="4b389-143">View your logic app runs in your OMS workspace</span></span>

1. <span data-ttu-id="4b389-144">Számát és a logic app kísérletekhez állapotának megtekintéséhez nyissa meg a az OMS-munkaterület áttekintő lapja.</span><span class="sxs-lookup"><span data-stu-id="4b389-144">To view the count and status for your logic app runs, go to the overview page for your OMS workspace.</span></span> <span data-ttu-id="4b389-145">Tekintse át a részleteket a a **Logic Apps felügyeleti** csempére.</span><span class="sxs-lookup"><span data-stu-id="4b389-145">Review the details on the **Logic Apps Management** tile.</span></span>

   ![Logic app futtatása száma és állapotát megjelenítő áttekintés csempe](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   > [!Note]
   > <span data-ttu-id="4b389-147">Ha a frissítési szalagcím akkor jelenik meg, a Logic Apps felügyeleti csempe helyett, válassza ki azt a transzparens, hogy az OMS-munkaterület először frissítenie.</span><span class="sxs-lookup"><span data-stu-id="4b389-147">If this upgrade banner appears instead of the Logic Apps Management tile, choose the banner so that you upgrade your OMS workspace first.</span></span>
  
   > ![A frissítés "OMS-munkaterület"](media/logic-apps-monitor-your-logic-apps-oms/oms-upgrade-banner.png)

2. <span data-ttu-id="4b389-149">További információt a logic app futtatása az összefoglaló megtekintéséhez válassza a **Logic Apps felügyeleti** csempére.</span><span class="sxs-lookup"><span data-stu-id="4b389-149">To view a summary with more details about your logic app runs, choose the **Logic Apps Management** tile.</span></span>

   <span data-ttu-id="4b389-150">A logic app fut itt, név, illetve végrehajtási állapot szerint vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="4b389-150">Here, your logic app runs are grouped by name or by execution status.</span></span>

   ![Állapotának összegzése a Logic Apps alkalmazást futtat](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
3. <span data-ttu-id="4b389-152">Az összes fut, egy adott logikai alkalmazást vagy az állapot megtekintéséhez jelölje ki a logikai alkalmazás vagy egy állapotát.</span><span class="sxs-lookup"><span data-stu-id="4b389-152">To view all the runs for a specific logic app or status, select the row for a logic app or a status.</span></span>

   <span data-ttu-id="4b389-153">Íme egy példa, amely megjeleníti az adott logikai alkalmazás a fut:</span><span class="sxs-lookup"><span data-stu-id="4b389-153">Here is an example that shows all the runs for a specific logic app:</span></span>

   ![A logikai alkalmazást vagy egy állapot nézet futtatások](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   > [!NOTE]
   > <span data-ttu-id="4b389-155">A **meghiúsultak** az oszlopban látható a "Yes" újraküldött futtató a kísérletekhez.</span><span class="sxs-lookup"><span data-stu-id="4b389-155">The **Resubmission** column shows "Yes" for runs that result from a resubmitted run.</span></span>

4. <span data-ttu-id="4b389-156">Az eredmények szűréséhez végezheti el az ügyféloldali és a kiszolgálóoldali szűrés.</span><span class="sxs-lookup"><span data-stu-id="4b389-156">To filter these results, you can perform both client-side and server-side filtering.</span></span>

   * <span data-ttu-id="4b389-157">Ügyféloldali szűrő: az oszlopok, válassza ki a kívánt szűrőket.</span><span class="sxs-lookup"><span data-stu-id="4b389-157">Client-side filter: For each column, choose the filters that you want.</span></span> 
   <span data-ttu-id="4b389-158">Néhány példa:</span><span class="sxs-lookup"><span data-stu-id="4b389-158">Here are some examples:</span></span>

     ![Példa oszlopszűrők](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * <span data-ttu-id="4b389-160">Kiszolgálóoldali szűrés: Válasszon egy olyan adott időkeretet, vagy fut, amely megjelenik a számát, a hatókör vezérlőt használja az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="4b389-160">Server-side filter: To choose a specific time window or to limit the number of runs that appear, use the scope control at the top of the page.</span></span> 
   <span data-ttu-id="4b389-161">Alapértelmezés szerint csak 1000 rekordok jelennek meg egyszerre.</span><span class="sxs-lookup"><span data-stu-id="4b389-161">By default, only 1,000 records appear at a time.</span></span> 
   
     ![Az időszak módosítása](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
5. <span data-ttu-id="4b389-163">A műveletek és az adatait a megadott futtató megtekintéséhez válasszon ki egy sort, amely a napló lapon nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="4b389-163">To view all the actions and their details for a specific run, select a row, which opens the Log Search page.</span></span> 

   * <span data-ttu-id="4b389-164">A táblázat ezek az információk megtekintéséhez válassza **tábla**.</span><span class="sxs-lookup"><span data-stu-id="4b389-164">To view this information in a table, choose **Table**.</span></span>
   * <span data-ttu-id="4b389-165">Ha módosítani szeretné a lekérdezést, szerkesztheti a lekérdezési karakterláncot a keresési sávon.</span><span class="sxs-lookup"><span data-stu-id="4b389-165">To change the query, you can edit the query string in the search bar.</span></span> 
   <span data-ttu-id="4b389-166">A jobb teljesítmény érdekében válasszon **Advanced Analytics**.</span><span class="sxs-lookup"><span data-stu-id="4b389-166">For a better experience, choose **Advanced Analytics**.</span></span>

     ![Műveletek és a Futtatás logikai alkalmazás részleteinek megtekintése](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)

     <span data-ttu-id="4b389-168">Itt az Azure Naplóelemzés oldalon frissítheti lekérdezések és az eredmények megtekintése a táblából.</span><span class="sxs-lookup"><span data-stu-id="4b389-168">Here on the Azure Log Analytics page, you can update queries and view the results from the table.</span></span> 
     <span data-ttu-id="4b389-169">Ez a lekérdezés használ [Kusto lekérdezési nyelv](https://docs.loganalytics.io/learn/tutorials/getting_started_with_queries.html), amelyen szerkesztheti, ha meg szeretné tekinteni, eltérő eredményeket.</span><span class="sxs-lookup"><span data-stu-id="4b389-169">This query uses [Kusto query language](https://docs.loganalytics.io/learn/tutorials/getting_started_with_queries.html), which you can edit if you want to view different results.</span></span> 

     ![Az Azure Log Analytics - lekérdezési nézet](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a><span data-ttu-id="4b389-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4b389-171">Next steps</span></span>

* [<span data-ttu-id="4b389-172">B2B üzenetek megfigyelése</span><span class="sxs-lookup"><span data-stu-id="4b389-172">Monitor B2B messages</span></span>](../logic-apps/logic-apps-monitor-b2b-message.md)
