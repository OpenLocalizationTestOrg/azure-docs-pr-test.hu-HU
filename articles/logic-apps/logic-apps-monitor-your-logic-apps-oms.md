---
title: "a Logic Apps alkalmazást aaaMonitor és get észrevételeket fusson, OMS - Azure Logic Apps |} Microsoft Docs"
description: "A logic app fut, Naplóelemzés és az Operations Management Suite (OMS) tooget insights és – hibaelhárítás és diagnosztika gazdagabb hibakeresési részletei figyelése"
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
ms.openlocfilehash: a76fd6d1ff5c0010550be0f991514ce95f659fd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-operations-management-suite-oms-and-log-analytics"></a><span data-ttu-id="40313-103">Logikai alkalmazás figyelése és a get észrevételeket fut, az Operations Management Suite (OMS) és a Naplóelemzési</span><span class="sxs-lookup"><span data-stu-id="40313-103">Monitor and get insights about logic app runs with Operations Management Suite (OMS) and Log Analytics</span></span>

<span data-ttu-id="40313-104">Figyelési és gazdagabb hibakeresési információ bekapcsolása Naplóelemzési: hello ugyanannyi időt vesz igénybe, ha logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="40313-104">For monitoring and richer debugging information, you can turn on Log Analytics at hello same time when you create a logic app.</span></span> <span data-ttu-id="40313-105">A Naplóelemzési biztosít a diagnosztika naplózásának és figyelésének a logikai alkalmazásnak hello Operations Management Suite (OMS) portálon keresztül futtatja.</span><span class="sxs-lookup"><span data-stu-id="40313-105">Log Analytics provides diagnostics logging and monitoring for your logic app runs through hello Operations Management Suite (OMS) portal.</span></span> <span data-ttu-id="40313-106">Hello Logic Apps felügyeleti megoldás tooOMS hozzáadásakor összesített állapotának beolvasása a logic app futtatása és a kívánt részletes adatok, például állapot, a végrehajtási idő, a ismételt továbbítása során állapot és a korrelációs azonosító.</span><span class="sxs-lookup"><span data-stu-id="40313-106">When you add hello Logic Apps Management solution tooOMS, you get aggregated status for your logic app runs and specific details like status, execution time, resubmission status, and correlation IDs.</span></span>

<span data-ttu-id="40313-107">Ez a témakör bemutatja, hogyan tooturn Naplóelemzési vagy a telepítés hello-e az OMS Logic Apps felügyeleti megoldás, futásidejű események és a logikai alkalmazásnak adatok futtathatók.</span><span class="sxs-lookup"><span data-stu-id="40313-107">This topic shows how tooturn on Log Analytics or install hello Logic Apps Management solution in OMS so you can view runtime events and data for your logic app run.</span></span>

 > [!TIP]
 > <span data-ttu-id="40313-108">toomonitor a meglévő logic apps, kövesse az alábbi lépéseket túl [diagnosztikai naplózás bekapcsolásához és a logic app futásidejű adatok tooOMS küldése](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span><span class="sxs-lookup"><span data-stu-id="40313-108">toomonitor your existing logic apps, follow these steps too [turn on diagnostic logging and send logic app runtime data tooOMS](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span></span>

## <a name="requirements"></a><span data-ttu-id="40313-109">Követelmények</span><span class="sxs-lookup"><span data-stu-id="40313-109">Requirements</span></span>

<span data-ttu-id="40313-110">Mielőtt elkezdené, toohave OMS-munkaterület szüksége.</span><span class="sxs-lookup"><span data-stu-id="40313-110">Before you start, you need toohave an OMS workspace.</span></span> <span data-ttu-id="40313-111">Ismerje meg, [hogyan toocreate OMS-munkaterület](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="40313-111">Learn [how toocreate an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span> 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a><span data-ttu-id="40313-112">A logic apps létrehozásakor diagnosztikai naplózás bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="40313-112">Turn on diagnostics logging when creating logic apps</span></span>

1. <span data-ttu-id="40313-113">A [Azure-portálon](https://portal.azure.com), logikai alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="40313-113">In [Azure portal](https://portal.azure.com), create a logic app.</span></span> <span data-ttu-id="40313-114">Válasszon **új** > **vállalati integrációs** > **logikai alkalmazás** > **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="40313-114">Choose **New** > **Enterprise Integration** > **Logic App** > **Create**.</span></span>

   ![Logikai alkalmazás létrehozása](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

2. <span data-ttu-id="40313-116">A hello **hozzon létre logikai alkalmazás** lapján látható ezen feladatok végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="40313-116">In hello **Create logic app** page, perform these tasks as shown:</span></span>

   1. <span data-ttu-id="40313-117">Adjon meg egy nevet a Logic Apps alkalmazást, és válassza ki az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="40313-117">Provide a name for your logic app and select your Azure subscription.</span></span> 
   2. <span data-ttu-id="40313-118">Hozzon létre vagy válasszon ki egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="40313-118">Create or select an Azure resource group.</span></span>
   3. <span data-ttu-id="40313-119">Állítsa be **Naplóelemzési** túl**a**.</span><span class="sxs-lookup"><span data-stu-id="40313-119">Set **Log Analytics** too**On**.</span></span> 
   <span data-ttu-id="40313-120">Jelölje be hello OMS-munkaterület, ahová a logikai alkalmazásnak túl adatküldés fut.</span><span class="sxs-lookup"><span data-stu-id="40313-120">Select hello OMS workspace where you want too send data for your logic app runs.</span></span> 
   4. <span data-ttu-id="40313-121">Ha elkészült, válassza ki a **PIN-kód toodashboard** > **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="40313-121">When you're ready, choose **Pin toodashboard** > **Create**.</span></span>

      ![Logikai alkalmazás létrehozása](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      <span data-ttu-id="40313-123">Ez a lépés befejezése után az Azure létrehoz a logikai alkalmazás, amely most már az OMS-munkaterület társított.</span><span class="sxs-lookup"><span data-stu-id="40313-123">After you finish this step, Azure creates your logic app, which is now associated with your OMS workspace.</span></span> 
      <span data-ttu-id="40313-124">Emellett ebben a lépésben is automatikusan telepíti hello Logic Apps-kezelési megoldás az OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="40313-124">Also, this step also automatically installs hello Logic Apps Management solution in your OMS workspace.</span></span>

3. <span data-ttu-id="40313-125">a Logic Apps alkalmazást futtat OMS-ben, tooview [folytassa a következő lépéseket](#view-logic-app-runs-oms).</span><span class="sxs-lookup"><span data-stu-id="40313-125">tooview your logic app runs in OMS, [continue with these steps](#view-logic-app-runs-oms).</span></span>

## <a name="install-hello-logic-apps-management-solution-in-oms"></a><span data-ttu-id="40313-126">Az OMS hello Logic Apps-kezelési megoldás telepítése</span><span class="sxs-lookup"><span data-stu-id="40313-126">Install hello Logic Apps Management solution in OMS</span></span>

<span data-ttu-id="40313-127">Ha Ön már engedélyezve van a Naplóelemzési a logikai alkalmazás létrehozása után, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="40313-127">If you already turned on Log Analytics when you created your logic app, skip this step.</span></span> <span data-ttu-id="40313-128">Már van telepítve az OMS hello Logic Apps felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="40313-128">You already have hello Logic Apps Management solution installed in OMS.</span></span>

1. <span data-ttu-id="40313-129">A hello [Azure-portálon](https://portal.azure.com), válassza a **több szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="40313-129">In hello [Azure portal](https://portal.azure.com), choose **More Services**.</span></span> <span data-ttu-id="40313-130">Keresse meg a "naplóelemzési" szűrőként, és válassza a **Naplóelemzési** látható módon:</span><span class="sxs-lookup"><span data-stu-id="40313-130">Search for "log analytics" as your filter, and choose **Log Analytics** as shown:</span></span>

   ![Válassza ki a "Naplóelemzési"](media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

2. <span data-ttu-id="40313-132">A **Naplóelemzési**, található, és válassza ki az OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="40313-132">Under **Log Analytics**, find and select your OMS workspace.</span></span> 

   ![Az OMS-munkaterület kiválasztása](media/logic-apps-monitor-your-logic-apps-oms/select-logic-app.png)

3. <span data-ttu-id="40313-134">A **felügyeleti**, válassza a **OMS-portálon**.</span><span class="sxs-lookup"><span data-stu-id="40313-134">Under **Management**, choose **OMS Portal**.</span></span>

   ![Válassza ki a "OMS-portálon"](media/logic-apps-monitor-your-logic-apps-oms/oms-portal-page.png)

4. <span data-ttu-id="40313-136">A OMS kezdőlap hello frissítési szalagcím akkor jelenik meg, ha válasszon hello szalagcím, hogy az OMS-munkaterület először frissítenie.</span><span class="sxs-lookup"><span data-stu-id="40313-136">On your OMS homepage, if hello upgrade banner appears, choose hello banner so that you upgrade your OMS workspace first.</span></span> <span data-ttu-id="40313-137">Válassza a **megoldások gyűjtemény**.</span><span class="sxs-lookup"><span data-stu-id="40313-137">Then choose **Solutions Gallery**.</span></span>

   ![Válassza ki a "Megoldások gyűjtemény"](media/logic-apps-monitor-your-logic-apps-oms/solutions-gallery.png)

5. <span data-ttu-id="40313-139">A **minden megoldás**, található, és válassza ki a hello csempéjére a hozzá tartozó hello **Logic Apps felügyeleti** megoldás.</span><span class="sxs-lookup"><span data-stu-id="40313-139">Under **All solutions**, find and choose hello tile for hello **Logic Apps Management** solution.</span></span>

   ![Válassza ki a "Logic Apps kezelése"](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-management-tile2.png)

6. <span data-ttu-id="40313-141">az OMS-munkaterület tooinstall hello megoldás kiválasztása **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="40313-141">tooinstall hello solution in your OMS workspace, choose **Add**.</span></span>

   ![Válassza a "Hozzáadás" a "Logic Apps kezelése"](media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-your-logic-app-runs-in-your-oms-workspace"></a><span data-ttu-id="40313-143">A Logic Apps alkalmazást futtat az OMS-munkaterület megjelenítése</span><span class="sxs-lookup"><span data-stu-id="40313-143">View your logic app runs in your OMS workspace</span></span>

1. <span data-ttu-id="40313-144">tooview hello számát és a logikai alkalmazás állapotának fut, nyissa meg toohello az OMS-munkaterület áttekintő lapja.</span><span class="sxs-lookup"><span data-stu-id="40313-144">tooview hello count and status for your logic app runs, go toohello overview page for your OMS workspace.</span></span> <span data-ttu-id="40313-145">Tekintse át a hello hello részleteket **Logic Apps felügyeleti** csempére.</span><span class="sxs-lookup"><span data-stu-id="40313-145">Review hello details on hello **Logic Apps Management** tile.</span></span>

   ![Logic app futtatása száma és állapotát megjelenítő áttekintés csempe](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   > [!Note]
   > <span data-ttu-id="40313-147">Ha a frissítési szalagcím hello Logic Apps felügyeleti csempe nem jelenik meg, válassza ki a hello transzparens, hogy az OMS-munkaterület először frissítenie.</span><span class="sxs-lookup"><span data-stu-id="40313-147">If this upgrade banner appears instead of hello Logic Apps Management tile, choose hello banner so that you upgrade your OMS workspace first.</span></span>
  
   > ![A frissítés "OMS-munkaterület"](media/logic-apps-monitor-your-logic-apps-oms/oms-upgrade-banner.png)

2. <span data-ttu-id="40313-149">tooview összegzését, amelyen további információkat talál a logic app futtatja, válasszon hello **Logic Apps felügyeleti** csempére.</span><span class="sxs-lookup"><span data-stu-id="40313-149">tooview a summary with more details about your logic app runs, choose hello **Logic Apps Management** tile.</span></span>

   <span data-ttu-id="40313-150">A logic app fut itt, név, illetve végrehajtási állapot szerint vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="40313-150">Here, your logic app runs are grouped by name or by execution status.</span></span>

   ![Állapotának összegzése a Logic Apps alkalmazást futtat](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
3. <span data-ttu-id="40313-152">az összes hello tooview futtatása egy adott logikai alkalmazás vagy az állapot, a logikai alkalmazás vagy egy állapot válassza hello sort.</span><span class="sxs-lookup"><span data-stu-id="40313-152">tooview all hello runs for a specific logic app or status, select hello row for a logic app or a status.</span></span>

   <span data-ttu-id="40313-153">Itt a következő példa bemutatja, az adott logikai alkalmazás összes hello fut:</span><span class="sxs-lookup"><span data-stu-id="40313-153">Here is an example that shows all hello runs for a specific logic app:</span></span>

   ![A logikai alkalmazást vagy egy állapot nézet futtatások](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   > [!NOTE]
   > <span data-ttu-id="40313-155">Hello **meghiúsultak** az oszlopban látható a "Yes" újraküldött futtató a kísérletekhez.</span><span class="sxs-lookup"><span data-stu-id="40313-155">hello **Resubmission** column shows "Yes" for runs that result from a resubmitted run.</span></span>

4. <span data-ttu-id="40313-156">toofilter ezek annak az eredménye, hajthat végre az ügyféloldali és a kiszolgálóoldali szűrés.</span><span class="sxs-lookup"><span data-stu-id="40313-156">toofilter these results, you can perform both client-side and server-side filtering.</span></span>

   * <span data-ttu-id="40313-157">Ügyféloldali szűrő: az oszlopok, válassza ki a kívánt hello szűrőket.</span><span class="sxs-lookup"><span data-stu-id="40313-157">Client-side filter: For each column, choose hello filters that you want.</span></span> 
   <span data-ttu-id="40313-158">Néhány példa:</span><span class="sxs-lookup"><span data-stu-id="40313-158">Here are some examples:</span></span>

     ![Példa oszlopszűrők](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * <span data-ttu-id="40313-160">Kiszolgálóoldali szűrés: toochoose adott időpont ablak vagy toolimit hello számos fut, amely megjelenik, használjon hello hatókör vezérlő hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="40313-160">Server-side filter: toochoose a specific time window or toolimit hello number of runs that appear, use hello scope control at hello top of hello page.</span></span> 
   <span data-ttu-id="40313-161">Alapértelmezés szerint csak 1000 rekordok jelennek meg egyszerre.</span><span class="sxs-lookup"><span data-stu-id="40313-161">By default, only 1,000 records appear at a time.</span></span> 
   
     ![Változás hello időkerete](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
5. <span data-ttu-id="40313-163">minden tooview hello műveletek és egy adott futtatási, válassza ki az adataikat egymás után, amely hello napló keresése oldal megnyitása.</span><span class="sxs-lookup"><span data-stu-id="40313-163">tooview all hello actions and their details for a specific run, select a row, which opens hello Log Search page.</span></span> 

   * <span data-ttu-id="40313-164">tooview ezt az információt a tábla válasszon **tábla**.</span><span class="sxs-lookup"><span data-stu-id="40313-164">tooview this information in a table, choose **Table**.</span></span>
   * <span data-ttu-id="40313-165">toochange hello lekérdezés, szerkesztheti hello keresősávban hello lekérdezési karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="40313-165">toochange hello query, you can edit hello query string in hello search bar.</span></span> 
   <span data-ttu-id="40313-166">A jobb teljesítmény érdekében válasszon **Advanced Analytics**.</span><span class="sxs-lookup"><span data-stu-id="40313-166">For a better experience, choose **Advanced Analytics**.</span></span>

     ![Műveletek és a Futtatás logikai alkalmazás részleteinek megtekintése](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)

     <span data-ttu-id="40313-168">Itt hello Azure Naplóelemzés lapján frissítheti lekérdezések és nézet hello eredmények hello táblából.</span><span class="sxs-lookup"><span data-stu-id="40313-168">Here on hello Azure Log Analytics page, you can update queries and view hello results from hello table.</span></span> 
     <span data-ttu-id="40313-169">Ez a lekérdezés használ [Kusto lekérdezési nyelv](https://docs.loganalytics.io/learn/tutorials/getting_started_with_queries.html), amelyen szerkesztheti, ha azt szeretné, hogy tooview eltérő eredményt.</span><span class="sxs-lookup"><span data-stu-id="40313-169">This query uses [Kusto query language](https://docs.loganalytics.io/learn/tutorials/getting_started_with_queries.html), which you can edit if you want tooview different results.</span></span> 

     ![Az Azure Log Analytics - lekérdezési nézet](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a><span data-ttu-id="40313-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="40313-171">Next steps</span></span>

* [<span data-ttu-id="40313-172">B2B üzenetek megfigyelése</span><span class="sxs-lookup"><span data-stu-id="40313-172">Monitor B2B messages</span></span>](../logic-apps/logic-apps-monitor-b2b-message.md)
