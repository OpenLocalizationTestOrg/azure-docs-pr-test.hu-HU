---
title: "az Operations Management Suite - Azure Logic Apps B2B üzenetek aaaQuery |} Microsoft Docs"
description: "Hozzon létre lekérdezések tootrack AS2, X 12 és EDIFACT üzenetek hello Operations Management Suite szolgáltatásban"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: aee6644ff19add8f074ed5f1725db87b1d3b74b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="query-for-as2-x12-and-edifact-messages-in-hello-microsoft-operations-management-suite-oms"></a><span data-ttu-id="3038c-103">Az AS2, X 12 és EDIFACT üzenetek a Microsoft Operations Management Suite (OMS) hello lekérdezés</span><span class="sxs-lookup"><span data-stu-id="3038c-103">Query for AS2, X12, and EDIFACT messages in hello Microsoft Operations Management Suite (OMS)</span></span>

<span data-ttu-id="3038c-104">toofind hello AS2, X 12 és EDIFACT üzenetek nyomon követett webhelyekről, [Azure Naplóelemzés](../log-analytics/log-analytics-overview.md) a hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), a specifikus alapuló intézkedések kezdeményezésére szűrő lekérdezéseket hozhat létre feltételek.</span><span class="sxs-lookup"><span data-stu-id="3038c-104">toofind hello AS2, X12, or EDIFACT messages that you're tracking with [Azure Log Analytics](../log-analytics/log-analytics-overview.md) in hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), you can create queries that filter actions based on specific criteria.</span></span> <span data-ttu-id="3038c-105">Például egy adott interchange ellenőrző szám alapján is megtalálhatja.</span><span class="sxs-lookup"><span data-stu-id="3038c-105">For example, you can find messages based on a specific interchange control number.</span></span>

## <a name="requirements"></a><span data-ttu-id="3038c-106">Követelmények</span><span class="sxs-lookup"><span data-stu-id="3038c-106">Requirements</span></span>

* <span data-ttu-id="3038c-107">Egy logikai alkalmazást a diagnosztikai naplózás be van állítva.</span><span class="sxs-lookup"><span data-stu-id="3038c-107">A logic app that's set up with diagnostics logging.</span></span> <span data-ttu-id="3038c-108">Ismerje meg, [hogyan toocreate logikai alkalmazás](../logic-apps/logic-apps-create-a-logic-app.md) és [hogyan tooset be a naplózást az adott logikai alkalmazás](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span><span class="sxs-lookup"><span data-stu-id="3038c-108">Learn [how toocreate a logic app](../logic-apps/logic-apps-create-a-logic-app.md) and [how tooset up logging for that logic app](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span></span>

* <span data-ttu-id="3038c-109">Integráció fiók be van állítva a figyelés és naplózás.</span><span class="sxs-lookup"><span data-stu-id="3038c-109">An integration account that's set up with monitoring and logging.</span></span> <span data-ttu-id="3038c-110">Ismerje meg, [hogyan toocreate integrációs fiók](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) és [hogyan figyelés és naplózás fiók tooset](../logic-apps/logic-apps-monitor-b2b-message.md).</span><span class="sxs-lookup"><span data-stu-id="3038c-110">Learn [how toocreate an integration account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) and [how tooset up monitoring and logging for that account](../logic-apps/logic-apps-monitor-b2b-message.md).</span></span>

* <span data-ttu-id="3038c-111">Ha még nem tette, [diagnosztikai adatok tooLog Analytics közzététele](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) és [állítsa be az OMS nyomkövetési üzenet](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="3038c-111">If you haven't already, [publish diagnostic data tooLog Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) and [set up message tracking in OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>

> [!NOTE]
> <span data-ttu-id="3038c-112">Miután teljesítette hello előző követelmények, hello munkaterületeinek rendelkeznie kell [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3038c-112">After you've met hello previous requirements, you should have a workspace in hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).</span></span> <span data-ttu-id="3038c-113">Használjon hello azonos OMS-munkaterület nyomon követése a B2B kommunikáció az OMS Szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="3038c-113">You should use hello same OMS workspace for tracking your B2B communication in OMS.</span></span> 
>  
> <span data-ttu-id="3038c-114">Ha még nem rendelkezik az OMS-munkaterület, [hogyan toocreate OMS-munkaterület](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3038c-114">If you don't have an OMS workspace, learn [how toocreate an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span>

## <a name="create-message-queries-with-filters-in-hello-operations-management-suite-portal"></a><span data-ttu-id="3038c-115">Állapotüzenet-lekérdezések létrehozása szűrőkkel hello Operations Management Suite-portálon</span><span class="sxs-lookup"><span data-stu-id="3038c-115">Create message queries with filters in hello Operations Management Suite portal</span></span>

<span data-ttu-id="3038c-116">Ez a példa bemutatja, hogyan található üzenetek az adatcsere ellenőrző szám alapján.</span><span class="sxs-lookup"><span data-stu-id="3038c-116">This example shows how you can find messages based on their interchange control number.</span></span>

> [!TIP] 
> <span data-ttu-id="3038c-117">Ha ismeri az OMS-munkaterület neve, nyissa meg tooyour munkaterület kezdőlap (`https://{your-workspace-name}.portal.mms.microsoft.com`), 4. lépés: Indítsa el.</span><span class="sxs-lookup"><span data-stu-id="3038c-117">If you know your OMS workspace name, go tooyour workspace home page (`https://{your-workspace-name}.portal.mms.microsoft.com`), and start at Step 4.</span></span> <span data-ttu-id="3038c-118">Ellenkező esetben kezdjék 1. lépés.</span><span class="sxs-lookup"><span data-stu-id="3038c-118">Otherwise, start at Step 1.</span></span>

1. <span data-ttu-id="3038c-119">A hello [Azure-portálon](https://portal.azure.com), válassza a **több szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="3038c-119">In hello [Azure portal](https://portal.azure.com), choose **More Services**.</span></span> <span data-ttu-id="3038c-120">Keresse meg a "naplóelemzési", és válassza a **Naplóelemzési** itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="3038c-120">Search for "log analytics", and then choose **Log Analytics** as shown here:</span></span>

   ![A Naplóelemzési keresése](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. <span data-ttu-id="3038c-122">A **Naplóelemzési**, található, és válassza ki az OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="3038c-122">Under **Log Analytics**, find and select your OMS workspace.</span></span>

   ![Az OMS-munkaterület kiválasztása](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. <span data-ttu-id="3038c-124">A **felügyeleti**, válassza a **OMS-portálon**.</span><span class="sxs-lookup"><span data-stu-id="3038c-124">Under **Management**, choose **OMS Portal**.</span></span>

   ![Válassza ki az OMS-portálon](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/omsportalpage.png)

4. <span data-ttu-id="3038c-126">Az OMS kezdőlapján válassza **naplófájl-keresési**.</span><span class="sxs-lookup"><span data-stu-id="3038c-126">On your OMS home page, choose **Log Search**.</span></span>

   ![Az OMS kezdőlapján válassza a "Naplófájl-keresési"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   <span data-ttu-id="3038c-128">– vagy –</span><span class="sxs-lookup"><span data-stu-id="3038c-128">-or-</span></span>

   ![A hello OMS menüben válassza a "Naplófájl-keresési"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

5. <span data-ttu-id="3038c-130">Hello keresési mezőbe, írja be egy mező, amelyet az toofind, és nyomja le az ENTER **Enter**.</span><span class="sxs-lookup"><span data-stu-id="3038c-130">In hello search box, enter a field that you want toofind, and press **Enter**.</span></span> <span data-ttu-id="3038c-131">Amikor elkezdi beírni, OMS megjeleníti a lehetséges találatok és műveletek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="3038c-131">When you start typing, OMS shows you possible matches and operations that you can use.</span></span> <span data-ttu-id="3038c-132">További információ [hogyan Naplóelemzési toofind adatok](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="3038c-132">Learn more about [how toofind data in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

   <span data-ttu-id="3038c-133">Ez a példa eseményeket keres **típus = AzureDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="3038c-133">This example searches for events with **Type=AzureDiagnostics**.</span></span>

   ![Kezdje beírni a lekérdezési karakterlánc](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

6. <span data-ttu-id="3038c-135">Hello bal oldali sávon kattintson hello időkeretet, amelyet az tooview.</span><span class="sxs-lookup"><span data-stu-id="3038c-135">In hello left bar, choose hello timeframe that you want tooview.</span></span> <span data-ttu-id="3038c-136">tooadd szűrőlekérdezésnek tooyour válasszon **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3038c-136">tooadd a filter tooyour query, choose **+Add**.</span></span>

   ![Szűrő tooquery hozzáadása](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

7. <span data-ttu-id="3038c-138">A **szűrők hozzáadása**, adja meg a hello szűrő nevét, így kívánt hello szűrő található.</span><span class="sxs-lookup"><span data-stu-id="3038c-138">Under **Add Filters**, enter hello filter name so you can find hello filter you want.</span></span> <span data-ttu-id="3038c-139">Válassza ki a hello szűrőt, és válassza a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3038c-139">Select hello filter, and choose **+Add**.</span></span>

   <span data-ttu-id="3038c-140">toofind hello interchange ellenőrző szám, ebben a példában hello word "csomópont" keres, és kiválasztja **event_record_messageProperties_interchangeControlNumber_s** hello szűrőként.</span><span class="sxs-lookup"><span data-stu-id="3038c-140">toofind hello interchange control number, this example searches for hello word "interchange", and selects **event_record_messageProperties_interchangeControlNumber_s** as hello filter.</span></span>

   ![Válassza ki a szűrő](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

9. <span data-ttu-id="3038c-142">Hello bal oldali sávon, adja meg a hello szűrő értéket, hogy szeretné, hogy toouse, és válassza **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="3038c-142">In hello left bar, select hello filter value that you want toouse, and choose **Apply**.</span></span>

   <span data-ttu-id="3038c-143">Ebben a példában hello interchange ellenőrző szám köszönőüzenetei azt szeretnénk, ha kiválasztja.</span><span class="sxs-lookup"><span data-stu-id="3038c-143">This example selects hello interchange control number for hello messages we want.</span></span>

   ![Adja meg a szűrő értéket](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

10. <span data-ttu-id="3038c-145">Térjen vissza van felépítése toohello lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="3038c-145">Now return toohello query that you're building.</span></span> <span data-ttu-id="3038c-146">A lekérdezés a kijelölt szűrő esemény és értékű frissítve lett.</span><span class="sxs-lookup"><span data-stu-id="3038c-146">Your query has been updated with your selected filter event and value.</span></span> <span data-ttu-id="3038c-147">Az előző eredmények most túl szűrve.</span><span class="sxs-lookup"><span data-stu-id="3038c-147">Your previous results are now filtered too.</span></span>

    ![Térjen vissza a szűrt eredményekkel tooyour lekérdezés](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a><span data-ttu-id="3038c-149">Jövőbeli használatra a lekérdezés mentése</span><span class="sxs-lookup"><span data-stu-id="3038c-149">Save your query for future use</span></span>

1. <span data-ttu-id="3038c-150">A lekérdezés a hello **naplófájl-keresési** lapon, válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="3038c-150">From your query on hello **Log Search** page, choose **Save**.</span></span> <span data-ttu-id="3038c-151">Nevezze el a lekérdezést, válasszon egy kategóriát, és válassza a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="3038c-151">Give your query a name, select a category, and choose **Save**.</span></span>

   ![A lekérdezés adjon egy nevet és a kategória](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. <span data-ttu-id="3038c-153">tooview a lekérdezésbe, válassza a **Kedvencek**.</span><span class="sxs-lookup"><span data-stu-id="3038c-153">tooview your query, choose **Favorites**.</span></span>

   ![Válassza ki a "Kedvencek"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. <span data-ttu-id="3038c-155">A **mentett keresések**, válassza ki a lekérdezést, hogy hello eredmények megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="3038c-155">Under **Saved Searches**, select your query so that you can view hello results.</span></span> <span data-ttu-id="3038c-156">tooupdate hello lekérdezés, eltérő eredményeket található hello lekérdezés szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="3038c-156">tooupdate hello query so you can find different results, edit hello query.</span></span>

   ![Válassza ki a lekérdezés](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-hello-operations-management-suite-portal"></a><span data-ttu-id="3038c-158">Keresse meg és futtassa a lekérdezések hello Operations Management Suite-portálon</span><span class="sxs-lookup"><span data-stu-id="3038c-158">Find and run saved queries in hello Operations Management Suite portal</span></span>

1. <span data-ttu-id="3038c-159">Nyissa meg az OMS-munkaterület kezdőlapjának (`https://{your-workspace-name}.portal.mms.microsoft.com`), és válassza a **naplófájl-keresési**.</span><span class="sxs-lookup"><span data-stu-id="3038c-159">Open your OMS workspace home page (`https://{your-workspace-name}.portal.mms.microsoft.com`), and choose **Log Search**.</span></span>

   ![Az OMS kezdőlapján válassza a "Naplófájl-keresési"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   <span data-ttu-id="3038c-161">– vagy –</span><span class="sxs-lookup"><span data-stu-id="3038c-161">-or-</span></span>

   ![A hello OMS menüben válassza a "Naplófájl-keresési"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. <span data-ttu-id="3038c-163">A hello **naplófájl-keresési** kezdőlapját, válassza a **Kedvencek**.</span><span class="sxs-lookup"><span data-stu-id="3038c-163">On hello **Log Search** home page, choose **Favorites**.</span></span>

   ![Válassza ki a "Kedvencek"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. <span data-ttu-id="3038c-165">A **mentett keresések**, válassza ki a lekérdezést, hogy hello eredmények megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="3038c-165">Under **Saved Searches**, select your query so that you can view hello results.</span></span> <span data-ttu-id="3038c-166">tooupdate hello lekérdezés, eltérő eredményeket található hello lekérdezés szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="3038c-166">tooupdate hello query so you can find different results, edit hello query.</span></span>

   ![Válassza ki a lekérdezés](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a><span data-ttu-id="3038c-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3038c-168">Next steps</span></span>

* [<span data-ttu-id="3038c-169">AS2-követési sémák</span><span class="sxs-lookup"><span data-stu-id="3038c-169">AS2 tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [<span data-ttu-id="3038c-170">X12-követési sémák</span><span class="sxs-lookup"><span data-stu-id="3038c-170">X12 tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [<span data-ttu-id="3038c-171">Egyéni követési sémák</span><span class="sxs-lookup"><span data-stu-id="3038c-171">Custom tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)