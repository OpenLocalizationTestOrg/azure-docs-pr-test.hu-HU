---
title: "aaaMonitor B2B tranzakciók és naplózás - Azure Logic Apps beállítása |} Microsoft Docs"
description: "Figyelő AS2, X 12 és EDIFACT-üzenetek, indítsa el az integrációs fiók diagnosztikai naplózás"
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
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: a6745ebf41aab331020bfec072f5806711d125bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-set-up-diagnostics-logging-for-b2b-communication-in-integration-accounts"></a><span data-ttu-id="f65a6-103">Figyelheti és integrációs fiókok B2B kommunikáció diagnosztikai naplózás beállítása</span><span class="sxs-lookup"><span data-stu-id="f65a6-103">Monitor and set up diagnostics logging for B2B communication in integration accounts</span></span>

<span data-ttu-id="f65a6-104">Miután beállította a B2B kommunikációját két üzleti folyamatok vagy a integrációs fiókon keresztül alkalmazásokat futtató entitásokból tudjon cserélni egymással üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="f65a6-104">After you set up B2B communication between two running business processes or applications through your integration account, those entities can exchange messages with each other.</span></span> <span data-ttu-id="f65a6-105">tooconfirm Ez a kommunikáció megfelelően működik-e, beállíthatja az AS2, X12, figyelés és EDIFACT-üzenetek, együtt a hello integrációs fiókhoz diagnosztikai naplózás [Azure Naplóelemzés](../log-analytics/log-analytics-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f65a6-105">tooconfirm this communication works as expected, you can set up monitoring for AS2, X12, and EDIFACT messages, along with diagnostics logging for your integration account through hello [Azure Log Analytics](../log-analytics/log-analytics-overview.md) service.</span></span> <span data-ttu-id="f65a6-106">Ez a szolgáltatás [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) figyeli a felhőalapú és helyszíni környezetben, így azok rendelkezésre állását és teljesítményét, karbantartása és is a futásidejű részleteit, illetve a szélesebb hibakereséshez eseményeket gyűjti.</span><span class="sxs-lookup"><span data-stu-id="f65a6-106">This service in [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) monitors your cloud and on-premises environments, helping you maintain their availability and performance, and also collects runtime details and events for richer debugging.</span></span> <span data-ttu-id="f65a6-107">Emellett [használja a diagnosztikai adatok más szolgáltatásokkal](#extend-diagnostic-data), például az Azure Storage és az Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="f65a6-107">You can also [use your diagnostic data with other services](#extend-diagnostic-data), like Azure Storage and Azure Event Hubs.</span></span>

## <a name="requirements"></a><span data-ttu-id="f65a6-108">Követelmények</span><span class="sxs-lookup"><span data-stu-id="f65a6-108">Requirements</span></span>

* <span data-ttu-id="f65a6-109">Egy logikai alkalmazást a diagnosztikai naplózás be van állítva.</span><span class="sxs-lookup"><span data-stu-id="f65a6-109">A logic app that's set up with diagnostics logging.</span></span> <span data-ttu-id="f65a6-110">Ismerje meg, [hogyan tooset be a naplózást az adott logikai alkalmazás](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span><span class="sxs-lookup"><span data-stu-id="f65a6-110">Learn [how tooset up logging for that logic app](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).</span></span>

  > [!NOTE]
  > <span data-ttu-id="f65a6-111">Ez a követelmény teljesítette, kell után hello munkaterületeinek [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f65a6-111">After you've met this requirement, you should have a workspace in hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md).</span></span> <span data-ttu-id="f65a6-112">Használjon ugyanazon OMS-munkaterület hello integrációs fiókja naplózás beállítása során.</span><span class="sxs-lookup"><span data-stu-id="f65a6-112">You should use hello same OMS workspace when you set up logging for your integration account.</span></span> <span data-ttu-id="f65a6-113">Ha még nem rendelkezik az OMS-munkaterület, [hogyan toocreate OMS-munkaterület](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f65a6-113">If you don't have an OMS workspace, learn [how toocreate an OMS workspace](../log-analytics/log-analytics-get-started.md).</span></span>

* <span data-ttu-id="f65a6-114">Integráció fiók, amely tooyour logikai alkalmazás van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="f65a6-114">An integration account that's linked tooyour logic app.</span></span> <span data-ttu-id="f65a6-115">Ismerje meg, [hogyan toocreate integrációs hivatkozás tooyour logikai alkalmazás fiók](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).</span><span class="sxs-lookup"><span data-stu-id="f65a6-115">Learn [how toocreate an integration account with a link tooyour logic app](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).</span></span>

## <a name="turn-on-diagnostics-logging-for-your-integration-account"></a><span data-ttu-id="f65a6-116">Diagnosztika integrációs fiókja naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f65a6-116">Turn on diagnostics logging for your integration account</span></span>

<span data-ttu-id="f65a6-117">Vagy közvetlenül integrációs fiókjából naplózás bekapcsolása vagy [keresztül hello Azure figyelőszolgáltatás](#azure-monitor-service).</span><span class="sxs-lookup"><span data-stu-id="f65a6-117">You can turn on logging either directly from your integration account or [through hello Azure Monitor service](#azure-monitor-service).</span></span> <span data-ttu-id="f65a6-118">Az Azure biztosít alapvető figyelési infrastruktúra szintű adatokkal.</span><span class="sxs-lookup"><span data-stu-id="f65a6-118">Azure Monitor provides basic monitoring with infrastructure-level data.</span></span> <span data-ttu-id="f65a6-119">További információ [Azure figyelő](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md).</span><span class="sxs-lookup"><span data-stu-id="f65a6-119">Learn more about [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md).</span></span>

### <a name="turn-on-diagnostics-logging-directly-from-your-integration-account"></a><span data-ttu-id="f65a6-120">Diagnosztika közvetlen integráció fiókjából naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f65a6-120">Turn on diagnostics logging directly from your integration account</span></span>

1. <span data-ttu-id="f65a6-121">A hello [Azure-portálon](https://portal.azure.com), található, és válassza ki a integrációs fiókját.</span><span class="sxs-lookup"><span data-stu-id="f65a6-121">In hello [Azure portal](https://portal.azure.com), find and select your integration account.</span></span> <span data-ttu-id="f65a6-122">A **figyelés**, válassza a **diagnosztikai naplók** itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="f65a6-122">Under **Monitoring**, choose **Diagnostics logs** as shown here:</span></span>

   ![Keresés, és válassza ki a integrációs fiókját, válassza a "Diagnosztikai naplók"](media/logic-apps-monitor-b2b-message/integration-account-diagnostics.png)

2. <span data-ttu-id="f65a6-124">Miután kiválasztotta a integrációs fiókját, a következő értékek hello automatikusan ki van jelölve.</span><span class="sxs-lookup"><span data-stu-id="f65a6-124">After you select your integration account, hello following values are automatically selected.</span></span> <span data-ttu-id="f65a6-125">Ha ezek az értékek megfelelőek, válassza ki a **a diagnosztika bekapcsolásához**.</span><span class="sxs-lookup"><span data-stu-id="f65a6-125">If these values are correct, choose **Turn on diagnostics**.</span></span> <span data-ttu-id="f65a6-126">Amennyiben nem válassza a kívánt hello értékeket:</span><span class="sxs-lookup"><span data-stu-id="f65a6-126">Otherwise, select hello values that you want:</span></span>

   1. <span data-ttu-id="f65a6-127">A **előfizetés**, válassza ki az Azure-előfizetést, amelyet a integrációs fiók hello.</span><span class="sxs-lookup"><span data-stu-id="f65a6-127">Under **Subscription**, select hello Azure subscription that you use with your integration account.</span></span>
   2. <span data-ttu-id="f65a6-128">A **erőforráscsoport**, jelölje be hello erőforráscsoport-integráció fiókjához használt.</span><span class="sxs-lookup"><span data-stu-id="f65a6-128">Under **Resource group**, select hello resource group that you use with your integration account.</span></span>
   3. <span data-ttu-id="f65a6-129">A **erőforrástípus**, jelölje be **integrációs fiókok**.</span><span class="sxs-lookup"><span data-stu-id="f65a6-129">Under **Resource type**, select **Integration accounts**.</span></span> 
   4. <span data-ttu-id="f65a6-130">A **erőforrás**, válassza ki a integrációs fiókját.</span><span class="sxs-lookup"><span data-stu-id="f65a6-130">Under **Resource**, select your integration account.</span></span> 
   5. <span data-ttu-id="f65a6-131">Válasszon **a diagnosztika bekapcsolásához**.</span><span class="sxs-lookup"><span data-stu-id="f65a6-131">Choose **Turn on diagnostics**.</span></span>

   ![Diagnosztika a integrációs fiók beállítása](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. <span data-ttu-id="f65a6-133">A **diagnosztikai beállítások**, majd **állapot**, válassza a **a**.</span><span class="sxs-lookup"><span data-stu-id="f65a6-133">Under **Diagnostics settings**, and then **Status**, choose **On**.</span></span>

   ![Kapcsolja be az Azure Diagnostics](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. <span data-ttu-id="f65a6-135">Immár hello OMS munkaterületet, és az adatok a naplózás toouse látható módon:</span><span class="sxs-lookup"><span data-stu-id="f65a6-135">Now select hello OMS workspace and data toouse for logging as shown:</span></span>

   1. <span data-ttu-id="f65a6-136">Válassza ki **tooLog Analytics küldése**.</span><span class="sxs-lookup"><span data-stu-id="f65a6-136">Select **Send tooLog Analytics**.</span></span> 
   2. <span data-ttu-id="f65a6-137">A **Naplóelemzési**, válassza a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="f65a6-137">Under **Log Analytics**, choose **Configure**.</span></span> 
   3. <span data-ttu-id="f65a6-138">A **OMS-munkaterület**, jelölje be az OMS-munkaterület toouse hello a naplózás.</span><span class="sxs-lookup"><span data-stu-id="f65a6-138">Under **OMS Workspaces**, select hello OMS workspace toouse for logging.</span></span>
   4. <span data-ttu-id="f65a6-139">A **napló**, jelölje be hello **IntegrationAccountTrackingEvents** kategóriát.</span><span class="sxs-lookup"><span data-stu-id="f65a6-139">Under **Log**, select hello **IntegrationAccountTrackingEvents** category.</span></span>
   5. <span data-ttu-id="f65a6-140">Válassza a **Mentés** elemet.</span><span class="sxs-lookup"><span data-stu-id="f65a6-140">Choose **Save**.</span></span>

   ![Diagnosztikai adatok tooa naplót elküldheti a Naplóelemzési beállítása](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. <span data-ttu-id="f65a6-142">Most [állítsa be az OMS B2B üzenetek nyomon követése](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="f65a6-142">Now [set up tracking for your B2B messages in OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>

<a name="azure-monitor-service"></a>

### <a name="turn-on-diagnostics-logging-through-azure-monitor"></a><span data-ttu-id="f65a6-143">Diagnosztikai naplózás keresztül Azure-figyelő bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="f65a6-143">Turn on diagnostics logging through Azure Monitor</span></span>

1. <span data-ttu-id="f65a6-144">A hello [Azure-portálon](https://portal.azure.com), a hello Azure főmenü, majd a **figyelő**, **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="f65a6-144">In hello [Azure portal](https://portal.azure.com), on hello main Azure menu, choose **Monitor**, **Diagnostics logs**.</span></span> <span data-ttu-id="f65a6-145">Ezután válassza ki a integrációs fiókját, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="f65a6-145">Then select your integration account as shown here:</span></span>

   ![Válassza a "Figyelése", "A diagnosztikai naplók", az integráció fiók kiválasztása](media/logic-apps-monitor-b2b-message/monitor-service-diagnostics-logs.png)

2. <span data-ttu-id="f65a6-147">Miután kiválasztotta a integrációs fiókját, a következő értékek hello automatikusan ki van jelölve.</span><span class="sxs-lookup"><span data-stu-id="f65a6-147">After you select your integration account, hello following values are automatically selected.</span></span> <span data-ttu-id="f65a6-148">Ha ezek az értékek megfelelőek, válassza ki a **a diagnosztika bekapcsolásához**.</span><span class="sxs-lookup"><span data-stu-id="f65a6-148">If these values are correct, choose **Turn on diagnostics**.</span></span> <span data-ttu-id="f65a6-149">Amennyiben nem válassza a kívánt hello értékeket:</span><span class="sxs-lookup"><span data-stu-id="f65a6-149">Otherwise, select hello values that you want:</span></span>

   1. <span data-ttu-id="f65a6-150">A **előfizetés**, válassza ki az Azure-előfizetést, amelyet a integrációs fiók hello.</span><span class="sxs-lookup"><span data-stu-id="f65a6-150">Under **Subscription**, select hello Azure subscription that you use with your integration account.</span></span>
   2. <span data-ttu-id="f65a6-151">A **erőforráscsoport**, jelölje be hello erőforráscsoport-integráció fiókjához használt.</span><span class="sxs-lookup"><span data-stu-id="f65a6-151">Under **Resource group**, select hello resource group that you use with your integration account.</span></span>
   3. <span data-ttu-id="f65a6-152">A **erőforrástípus**, jelölje be **integrációs fiókok**.</span><span class="sxs-lookup"><span data-stu-id="f65a6-152">Under **Resource type**, select **Integration accounts**.</span></span>
   4. <span data-ttu-id="f65a6-153">A **erőforrás**, válassza ki a integrációs fiókját.</span><span class="sxs-lookup"><span data-stu-id="f65a6-153">Under **Resource**, select your integration account.</span></span>
   5. <span data-ttu-id="f65a6-154">Válasszon **a diagnosztika bekapcsolásához**.</span><span class="sxs-lookup"><span data-stu-id="f65a6-154">Choose **Turn on diagnostics**.</span></span>

   ![Diagnosztika a integrációs fiók beállítása](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. <span data-ttu-id="f65a6-156">A **diagnosztikai beállítások**, válassza a **a**.</span><span class="sxs-lookup"><span data-stu-id="f65a6-156">Under **Diagnostics settings**, choose **On**.</span></span>

   ![Kapcsolja be az Azure Diagnostics](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. <span data-ttu-id="f65a6-158">Most válasszon hello OMS munkaterületet, és az esemény naplózási kategóriát látható módon:</span><span class="sxs-lookup"><span data-stu-id="f65a6-158">Now select hello OMS workspace and event category for logging as shown:</span></span>

   1. <span data-ttu-id="f65a6-159">Válassza ki **tooLog Analytics küldése**.</span><span class="sxs-lookup"><span data-stu-id="f65a6-159">Select **Send tooLog Analytics**.</span></span> 
   2. <span data-ttu-id="f65a6-160">A **Naplóelemzési**, válassza a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="f65a6-160">Under **Log Analytics**, choose **Configure**.</span></span> 
   3. <span data-ttu-id="f65a6-161">A **OMS-munkaterület**, jelölje be az OMS-munkaterület toouse hello a naplózás.</span><span class="sxs-lookup"><span data-stu-id="f65a6-161">Under **OMS Workspaces**, select hello OMS workspace toouse for logging.</span></span>
   4. <span data-ttu-id="f65a6-162">A **napló**, jelölje be hello **IntegrationAccountTrackingEvents** kategóriát.</span><span class="sxs-lookup"><span data-stu-id="f65a6-162">Under **Log**, select hello **IntegrationAccountTrackingEvents** category.</span></span>
   5. <span data-ttu-id="f65a6-163">Amikor elkészült, válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="f65a6-163">When you're done, choose **Save**.</span></span>

   ![Diagnosztikai adatok tooa naplót elküldheti a Naplóelemzési beállítása](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. <span data-ttu-id="f65a6-165">Most [állítsa be az OMS B2B üzenetek nyomon követése](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span><span class="sxs-lookup"><span data-stu-id="f65a6-165">Now [set up tracking for your B2B messages in OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).</span></span>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a><span data-ttu-id="f65a6-166">Hogyan és hol diagnosztikai adatok használja más szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="f65a6-166">Extend how and where you use diagnostic data with other services</span></span>

<span data-ttu-id="f65a6-167">Azure Naplóelemzés, valamint bővítheti, hogyan használhatja a Logic Apps alkalmazást diagnosztikai adatok más Azure-szolgáltatásokkal, például:</span><span class="sxs-lookup"><span data-stu-id="f65a6-167">Along with Azure Log Analytics, you can extend how you use your logic app's diagnostic data with other Azure services, for example:</span></span> 

* [<span data-ttu-id="f65a6-168">Az Azure diagnosztikai naplók az Azure Storage archív</span><span class="sxs-lookup"><span data-stu-id="f65a6-168">Archive Azure Diagnostics Logs in Azure Storage</span></span>](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [<span data-ttu-id="f65a6-169">Az adatfolyam Azure diagnosztikai naplók tooAzure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f65a6-169">Stream Azure Diagnostics Logs tooAzure Event Hubs</span></span>](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

<span data-ttu-id="f65a6-170">Ezután a get valós idejű figyelés telemetriai adatok és más szolgáltatások analytics segítségével például [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) és [Power BI](../log-analytics/log-analytics-powerbi.md).</span><span class="sxs-lookup"><span data-stu-id="f65a6-170">You can then get real-time monitoring by using telemetry and analytics from other services, like [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) and [Power BI](../log-analytics/log-analytics-powerbi.md).</span></span> <span data-ttu-id="f65a6-171">Példa:</span><span class="sxs-lookup"><span data-stu-id="f65a6-171">For example:</span></span>

* [<span data-ttu-id="f65a6-172">Az Event Hubs tooStream Analytics adatfolyam adatait</span><span class="sxs-lookup"><span data-stu-id="f65a6-172">Stream data from Event Hubs tooStream Analytics</span></span>](../stream-analytics/stream-analytics-define-inputs.md)
* [<span data-ttu-id="f65a6-173">A Stream Analytics adatfolyam-továbbítási adatok elemzése és a Power bi-ban a valós idejű elemzési irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="f65a6-173">Analyze streaming data with Stream Analytics and create a real-time analytics dashboard in Power BI</span></span>](../stream-analytics/stream-analytics-power-bi-dashboard.md)

<span data-ttu-id="f65a6-174">Alapján hello beállítása kívánt beállításokat, győződjön meg arról, hogy Ön első [az Azure storage-fiók létrehozása](../storage/common/storage-create-storage-account.md) vagy [hozzon létre egy Azure event hubs](../event-hubs/event-hubs-create.md).</span><span class="sxs-lookup"><span data-stu-id="f65a6-174">Based on hello options that you want set up, make sure that you first [create an Azure storage account](../storage/common/storage-create-storage-account.md) or [create an Azure event hub](../event-hubs/event-hubs-create.md).</span></span> <span data-ttu-id="f65a6-175">Ezután válasszon hello beállítások ahová toosend diagnosztikai adatokat:</span><span class="sxs-lookup"><span data-stu-id="f65a6-175">Then select hello options for where you want toosend diagnostic data:</span></span>

![TooAzure tárolási fiók vagy esemény hub adatok küldése](./media/logic-apps-monitor-b2b-message/storage-account-event-hubs.png)

> [!NOTE]
> <span data-ttu-id="f65a6-177">Megőrzési időtartamú csak válasszon toouse tárfiókot vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="f65a6-177">Retention periods apply only when you choose toouse a storage account.</span></span>

## <a name="supported-tracking-schemas"></a><span data-ttu-id="f65a6-178">Támogatott sémák nyomon követése</span><span class="sxs-lookup"><span data-stu-id="f65a6-178">Supported tracking schemas</span></span>

<span data-ttu-id="f65a6-179">Azure támogatja ezeket sématípusok, amely azzal a különbséggel egyéni típus hello sémák rögzített nyomon követését.</span><span class="sxs-lookup"><span data-stu-id="f65a6-179">Azure supports these tracking schema types, which all have fixed schemas except hello Custom type.</span></span>

* [<span data-ttu-id="f65a6-180">AS2-követési séma</span><span class="sxs-lookup"><span data-stu-id="f65a6-180">AS2 tracking schema</span></span>](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [<span data-ttu-id="f65a6-181">X12-követési séma</span><span class="sxs-lookup"><span data-stu-id="f65a6-181">X12 tracking schema</span></span>](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [<span data-ttu-id="f65a6-182">Egyéni követési séma</span><span class="sxs-lookup"><span data-stu-id="f65a6-182">Custom tracking schema</span></span>](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a><span data-ttu-id="f65a6-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f65a6-183">Next steps</span></span>

* [<span data-ttu-id="f65a6-184">Az OMS B2B üzenetek nyomon</span><span class="sxs-lookup"><span data-stu-id="f65a6-184">Track B2B messages in OMS</span></span>](../logic-apps/logic-apps-track-b2b-messages-omsportal.md "OMS követése B2B üzenetek")
* [<span data-ttu-id="f65a6-185">További tudnivalók a vállalati integrációs csomag hello</span><span class="sxs-lookup"><span data-stu-id="f65a6-185">Learn more about hello Enterprise Integration Pack</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")

