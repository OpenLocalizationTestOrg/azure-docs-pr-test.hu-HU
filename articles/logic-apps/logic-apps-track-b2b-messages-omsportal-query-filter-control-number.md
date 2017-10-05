---
title: "Az Operations Management Suite - Azure Logic Apps B2B üzenetek lekérdezés |} Microsoft Docs"
description: "Az Operations Management Suite a nyomon követendő AS2, X 12 és EDIFACT üzeneteinek lekérdezések létrehozása"
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
ms.openlocfilehash: 2748d3d3daf7c13dca05f663a4a088598e1b3605
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="query-for-as2-x12-and-edifact-messages-in-the-microsoft-operations-management-suite-oms"></a><span data-ttu-id="fd948-103">Az AS2, X 12 és EDIFACT üzenetek a Microsoft Operations Management Suite (OMS) lekérdezés</span><span class="sxs-lookup"><span data-stu-id="fd948-103">Query for AS2, X12, and EDIFACT messages in the Microsoft Operations Management Suite (OMS)</span></span>

<span data-ttu-id="fd948-104">Az AS2 megkereséséhez X12 vagy EDIFACT-üzenetek, hogy követi nyomon a [Azure Naplóelemzés](../log-analytics/log-analytics-overview.md) a a [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), műveletek a megadott feltételek alapján szűrő lekérdezéseket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="fd948-104">To find the AS2, X12, or EDIFACT messages that you're tracking with [Azure Log Analytics](../log-analytics/log-analytics-overview.md) in the [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), you can create queries that filter actions based on specific criteria.</span></span> <span data-ttu-id="fd948-105">Például egy adott interchange ellenőrző szám alapján is megtalálhatja.</span><span class="sxs-lookup"><span data-stu-id="fd948-105">For example, you can find messages based on a specific interchange control number.</span></span>

## <a name="requirements"></a><span data-ttu-id="fd948-106">Követelmények</span><span class="sxs-lookup"><span data-stu-id="fd948-106">Requirements</span></span>

* <span data-ttu-id="fd948-107">Egy logikai alkalmazást a diagnosztikai naplózás be van állítva.</span><span class="sxs-lookup"><span data-stu-id="fd948-107">A logic app that's set up with diagnostics logging.</span></span> <span data-ttu-id="fd948-108">Ismerje meg, [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md) és [adott logikai alkalmazás naplózásának beállítása](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span><span class="sxs-lookup"><span data-stu-id="fd948-108">Learn [how to create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) and [how to set up logging for that logic app](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span></span>

* <span data-ttu-id="fd948-109">Integráció fiók be van állítva a figyelés és naplózás.</span><span class="sxs-lookup"><span data-stu-id="fd948-109">An integration account that's set up with monitoring and logging.</span></span> <span data-ttu-id="fd948-110">Ismerje meg, [integrációs fiók létrehozása](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) és [figyelés és naplózás fiók beállításával](../logic-apps/logic-apps-monitor-b2b-message.md).</span><span class="sxs-lookup"><span data-stu-id="fd948-110">Learn [how to create an integration account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) and [how to set up monitoring and logging for that account](../logic-apps/logic-apps-monitor-b2b-message.md).</span></span>

* <span data-ttu-id="fd948-111">Ha még nem tette, [diagnosztikai adatok közzétételére Naplóelemzési](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) és [állítsa be az OMS nyomkövetési üzenet](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="fd948-111">If you haven't already, [publish diagnostic data to Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) and [set up message tracking in OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>

> [!NOTE]
> <span data-ttu-id="fd948-112">Miután teljesítette az előző követelményeknek, rendelkeznie kell egy munkaterület a [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fd948-112">After you've met the previous requirements, you should have a workspace in the [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).</span></span> <span data-ttu-id="fd948-113">Az azonos OMS-munkaterület nyomon követése a B2B kommunikáció OMS kell használnia.</span><span class="sxs-lookup"><span data-stu-id="fd948-113">You should use the same OMS workspace for tracking your B2B communication in OMS.</span></span> 
>  
> <span data-ttu-id="fd948-114">Ha még nem rendelkezik az OMS-munkaterület, [OMS-munkaterület létrehozása](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="fd948-114">If you don't have an OMS workspace, learn [how to create an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span>

## <a name="create-message-queries-with-filters-in-the-operations-management-suite-portal"></a><span data-ttu-id="fd948-115">Állapotüzenet-lekérdezések létrehozása szűrőkkel az Operations Management Suite-portálon</span><span class="sxs-lookup"><span data-stu-id="fd948-115">Create message queries with filters in the Operations Management Suite portal</span></span>

<span data-ttu-id="fd948-116">Ez a példa bemutatja, hogyan található üzenetek az adatcsere ellenőrző szám alapján.</span><span class="sxs-lookup"><span data-stu-id="fd948-116">This example shows how you can find messages based on their interchange control number.</span></span>

> [!TIP] 
> <span data-ttu-id="fd948-117">Ha ismeri az OMS-munkaterület neve, nyissa meg a munkaterület kezdőlapra (`https://{your-workspace-name}.portal.mms.microsoft.com`), 4. lépés: Indítsa el.</span><span class="sxs-lookup"><span data-stu-id="fd948-117">If you know your OMS workspace name, go to your workspace home page (`https://{your-workspace-name}.portal.mms.microsoft.com`), and start at Step 4.</span></span> <span data-ttu-id="fd948-118">Ellenkező esetben kezdjék 1. lépés.</span><span class="sxs-lookup"><span data-stu-id="fd948-118">Otherwise, start at Step 1.</span></span>

1. <span data-ttu-id="fd948-119">Az a [Azure-portálon](https://portal.azure.com), válassza a **több szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="fd948-119">In the [Azure portal](https://portal.azure.com), choose **More Services**.</span></span> <span data-ttu-id="fd948-120">Keresse meg a "naplóelemzési", és válassza a **Naplóelemzési** itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="fd948-120">Search for "log analytics", and then choose **Log Analytics** as shown here:</span></span>

   ![A Naplóelemzési keresése](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. <span data-ttu-id="fd948-122">A **Naplóelemzési**, található, és válassza ki az OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="fd948-122">Under **Log Analytics**, find and select your OMS workspace.</span></span>

   ![Az OMS-munkaterület kiválasztása](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. <span data-ttu-id="fd948-124">A **felügyeleti**, válassza a **OMS-portálon**.</span><span class="sxs-lookup"><span data-stu-id="fd948-124">Under **Management**, choose **OMS Portal**.</span></span>

   ![Válassza ki az OMS-portálon](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/omsportalpage.png)

4. <span data-ttu-id="fd948-126">Az OMS kezdőlapján válassza **naplófájl-keresési**.</span><span class="sxs-lookup"><span data-stu-id="fd948-126">On your OMS home page, choose **Log Search**.</span></span>

   ![Az OMS kezdőlapján válassza a "Naplófájl-keresési"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   <span data-ttu-id="fd948-128">– vagy –</span><span class="sxs-lookup"><span data-stu-id="fd948-128">-or-</span></span>

   ![A OMS menüben válassza a "Naplófájl-keresési"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

5. <span data-ttu-id="fd948-130">A keresési mezőbe, írja be egy mező található, és nyomja le az ENTER kívánt **Enter**.</span><span class="sxs-lookup"><span data-stu-id="fd948-130">In the search box, enter a field that you want to find, and press **Enter**.</span></span> <span data-ttu-id="fd948-131">Amikor elkezdi beírni, OMS megjeleníti a lehetséges találatok és műveletek közül választhat.</span><span class="sxs-lookup"><span data-stu-id="fd948-131">When you start typing, OMS shows you possible matches and operations that you can use.</span></span> <span data-ttu-id="fd948-132">További információ [adatok megkeresése a Naplóelemzési](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="fd948-132">Learn more about [how to find data in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

   <span data-ttu-id="fd948-133">Ez a példa eseményeket keres **típus = AzureDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="fd948-133">This example searches for events with **Type=AzureDiagnostics**.</span></span>

   ![Kezdje beírni a lekérdezési karakterlánc](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

6. <span data-ttu-id="fd948-135">A bal oldali sávon válassza ki a megtekinteni kívánt időkeretet.</span><span class="sxs-lookup"><span data-stu-id="fd948-135">In the left bar, choose the timeframe that you want to view.</span></span> <span data-ttu-id="fd948-136">Adjon hozzá egy szűrőt a lekérdezést, válassza a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="fd948-136">To add a filter to your query, choose **+Add**.</span></span>

   ![Szűrő felvétele lekérdezés](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

7. <span data-ttu-id="fd948-138">A **szűrők hozzáadása**, így megtalálja a kívánt szűrőt, adja meg a szűrő nevét.</span><span class="sxs-lookup"><span data-stu-id="fd948-138">Under **Add Filters**, enter the filter name so you can find the filter you want.</span></span> <span data-ttu-id="fd948-139">Válassza ki a szűrőt, és válassza a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="fd948-139">Select the filter, and choose **+Add**.</span></span>

   <span data-ttu-id="fd948-140">Interchange ellenőrző szám megkereséséhez ebben a példában keres rá a "csomópont" szót, majd kiválasztja **event_record_messageProperties_interchangeControlNumber_s** a szűrőként.</span><span class="sxs-lookup"><span data-stu-id="fd948-140">To find the interchange control number, this example searches for the word "interchange", and selects **event_record_messageProperties_interchangeControlNumber_s** as the filter.</span></span>

   ![Válassza ki a szűrő](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

9. <span data-ttu-id="fd948-142">A bal oldali sávon, válassza ki a szűrő értéket használja, és válassza a kívánt **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="fd948-142">In the left bar, select the filter value that you want to use, and choose **Apply**.</span></span>

   <span data-ttu-id="fd948-143">Ez a példa azt szeretnénk, ha üzenetekhez interchange ellenőrző szám választja ki.</span><span class="sxs-lookup"><span data-stu-id="fd948-143">This example selects the interchange control number for the messages we want.</span></span>

   ![Adja meg a szűrő értéket](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

10. <span data-ttu-id="fd948-145">Térjen vissza a lekérdezést, amely éppen összeállításakor.</span><span class="sxs-lookup"><span data-stu-id="fd948-145">Now return to the query that you're building.</span></span> <span data-ttu-id="fd948-146">A lekérdezés a kijelölt szűrő esemény és értékű frissítve lett.</span><span class="sxs-lookup"><span data-stu-id="fd948-146">Your query has been updated with your selected filter event and value.</span></span> <span data-ttu-id="fd948-147">Az előző eredmények most túl szűrve.</span><span class="sxs-lookup"><span data-stu-id="fd948-147">Your previous results are now filtered too.</span></span>

    ![Térjen vissza a lekérdezés szűrt eredményekkel](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a><span data-ttu-id="fd948-149">Jövőbeli használatra a lekérdezés mentése</span><span class="sxs-lookup"><span data-stu-id="fd948-149">Save your query for future use</span></span>

1. <span data-ttu-id="fd948-150">A lekérdezés a a **naplófájl-keresési** lapon, válassza ki **mentése**.</span><span class="sxs-lookup"><span data-stu-id="fd948-150">From your query on the **Log Search** page, choose **Save**.</span></span> <span data-ttu-id="fd948-151">Nevezze el a lekérdezést, válasszon egy kategóriát, és válassza a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="fd948-151">Give your query a name, select a category, and choose **Save**.</span></span>

   ![A lekérdezés adjon egy nevet és a kategória](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. <span data-ttu-id="fd948-153">A lekérdezés megtekintéséhez válassza **Kedvencek**.</span><span class="sxs-lookup"><span data-stu-id="fd948-153">To view your query, choose **Favorites**.</span></span>

   ![Válassza ki a "Kedvencek"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. <span data-ttu-id="fd948-155">A **mentett keresések**, válassza ki a lekérdezést, hogy az eredmények megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="fd948-155">Under **Saved Searches**, select your query so that you can view the results.</span></span> <span data-ttu-id="fd948-156">A lekérdezés, eltérő eredményeket található frissítéséhez szerkessze a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="fd948-156">To update the query so you can find different results, edit the query.</span></span>

   ![Válassza ki a lekérdezés](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-the-operations-management-suite-portal"></a><span data-ttu-id="fd948-158">Keresse meg és lekérdezések futtatása az Operations Management Suite-portálon</span><span class="sxs-lookup"><span data-stu-id="fd948-158">Find and run saved queries in the Operations Management Suite portal</span></span>

1. <span data-ttu-id="fd948-159">Nyissa meg az OMS-munkaterület kezdőlapjának (`https://{your-workspace-name}.portal.mms.microsoft.com`), és válassza a **naplófájl-keresési**.</span><span class="sxs-lookup"><span data-stu-id="fd948-159">Open your OMS workspace home page (`https://{your-workspace-name}.portal.mms.microsoft.com`), and choose **Log Search**.</span></span>

   ![Az OMS kezdőlapján válassza a "Naplófájl-keresési"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   <span data-ttu-id="fd948-161">– vagy –</span><span class="sxs-lookup"><span data-stu-id="fd948-161">-or-</span></span>

   ![A OMS menüben válassza a "Naplófájl-keresési"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. <span data-ttu-id="fd948-163">Az a **naplófájl-keresési** kezdőlapját, válassza a **Kedvencek**.</span><span class="sxs-lookup"><span data-stu-id="fd948-163">On the **Log Search** home page, choose **Favorites**.</span></span>

   ![Válassza ki a "Kedvencek"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. <span data-ttu-id="fd948-165">A **mentett keresések**, válassza ki a lekérdezést, hogy az eredmények megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="fd948-165">Under **Saved Searches**, select your query so that you can view the results.</span></span> <span data-ttu-id="fd948-166">A lekérdezés, eltérő eredményeket található frissítéséhez szerkessze a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="fd948-166">To update the query so you can find different results, edit the query.</span></span>

   ![Válassza ki a lekérdezés](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a><span data-ttu-id="fd948-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fd948-168">Next steps</span></span>

* [<span data-ttu-id="fd948-169">AS2-követési sémák</span><span class="sxs-lookup"><span data-stu-id="fd948-169">AS2 tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [<span data-ttu-id="fd948-170">X12-követési sémák</span><span class="sxs-lookup"><span data-stu-id="fd948-170">X12 tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [<span data-ttu-id="fd948-171">Egyéni követési sémák</span><span class="sxs-lookup"><span data-stu-id="fd948-171">Custom tracking schemas</span></span>](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)