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
# <a name="query-for-as2-x12-and-edifact-messages-in-hello-microsoft-operations-management-suite-oms"></a>Az AS2, X 12 és EDIFACT üzenetek a Microsoft Operations Management Suite (OMS) hello lekérdezés

toofind hello AS2, X 12 és EDIFACT üzenetek nyomon követett webhelyekről, [Azure Naplóelemzés](../log-analytics/log-analytics-overview.md) a hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), a specifikus alapuló intézkedések kezdeményezésére szűrő lekérdezéseket hozhat létre feltételek. Például egy adott interchange ellenőrző szám alapján is megtalálhatja.

## <a name="requirements"></a>Követelmények

* Egy logikai alkalmazást a diagnosztikai naplózás be van állítva. Ismerje meg, [hogyan toocreate logikai alkalmazás](../logic-apps/logic-apps-create-a-logic-app.md) és [hogyan tooset be a naplózást az adott logikai alkalmazás](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Integráció fiók be van állítva a figyelés és naplózás. Ismerje meg, [hogyan toocreate integrációs fiók](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) és [hogyan figyelés és naplózás fiók tooset](../logic-apps/logic-apps-monitor-b2b-message.md).

* Ha még nem tette, [diagnosztikai adatok tooLog Analytics közzététele](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) és [állítsa be az OMS nyomkövetési üzenet](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> Miután teljesítette hello előző követelmények, hello munkaterületeinek rendelkeznie kell [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Használjon hello azonos OMS-munkaterület nyomon követése a B2B kommunikáció az OMS Szolgáltatáshoz. 
>  
> Ha még nem rendelkezik az OMS-munkaterület, [hogyan toocreate OMS-munkaterület](../log-analytics/log-analytics-get-started.md).

## <a name="create-message-queries-with-filters-in-hello-operations-management-suite-portal"></a>Állapotüzenet-lekérdezések létrehozása szűrőkkel hello Operations Management Suite-portálon

Ez a példa bemutatja, hogyan található üzenetek az adatcsere ellenőrző szám alapján.

> [!TIP] 
> Ha ismeri az OMS-munkaterület neve, nyissa meg tooyour munkaterület kezdőlap (`https://{your-workspace-name}.portal.mms.microsoft.com`), 4. lépés: Indítsa el. Ellenkező esetben kezdjék 1. lépés.

1. A hello [Azure-portálon](https://portal.azure.com), válassza a **több szolgáltatások**. Keresse meg a "naplóelemzési", és válassza a **Naplóelemzési** itt látható módon:

   ![A Naplóelemzési keresése](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. A **Naplóelemzési**, található, és válassza ki az OMS-munkaterület.

   ![Az OMS-munkaterület kiválasztása](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. A **felügyeleti**, válassza a **OMS-portálon**.

   ![Válassza ki az OMS-portálon](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/omsportalpage.png)

4. Az OMS kezdőlapján válassza **naplófájl-keresési**.

   ![Az OMS kezdőlapján válassza a "Naplófájl-keresési"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   – vagy –

   ![A hello OMS menüben válassza a "Naplófájl-keresési"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

5. Hello keresési mezőbe, írja be egy mező, amelyet az toofind, és nyomja le az ENTER **Enter**. Amikor elkezdi beírni, OMS megjeleníti a lehetséges találatok és műveletek közül választhat. További információ [hogyan Naplóelemzési toofind adatok](../log-analytics/log-analytics-log-searches.md).

   Ez a példa eseményeket keres **típus = AzureDiagnostics**.

   ![Kezdje beírni a lekérdezési karakterlánc](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

6. Hello bal oldali sávon kattintson hello időkeretet, amelyet az tooview. tooadd szűrőlekérdezésnek tooyour válasszon **+ Hozzáadás**.

   ![Szűrő tooquery hozzáadása](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

7. A **szűrők hozzáadása**, adja meg a hello szűrő nevét, így kívánt hello szűrő található. Válassza ki a hello szűrőt, és válassza a **+ Hozzáadás**.

   toofind hello interchange ellenőrző szám, ebben a példában hello word "csomópont" keres, és kiválasztja **event_record_messageProperties_interchangeControlNumber_s** hello szűrőként.

   ![Válassza ki a szűrő](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

9. Hello bal oldali sávon, adja meg a hello szűrő értéket, hogy szeretné, hogy toouse, és válassza **alkalmaz**.

   Ebben a példában hello interchange ellenőrző szám köszönőüzenetei azt szeretnénk, ha kiválasztja.

   ![Adja meg a szűrő értéket](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

10. Térjen vissza van felépítése toohello lekérdezést. A lekérdezés a kijelölt szűrő esemény és értékű frissítve lett. Az előző eredmények most túl szűrve.

    ![Térjen vissza a szűrt eredményekkel tooyour lekérdezés](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a>Jövőbeli használatra a lekérdezés mentése

1. A lekérdezés a hello **naplófájl-keresési** lapon, válassza ki **mentése**. Nevezze el a lekérdezést, válasszon egy kategóriát, és válassza a **mentése**.

   ![A lekérdezés adjon egy nevet és a kategória](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. tooview a lekérdezésbe, válassza a **Kedvencek**.

   ![Válassza ki a "Kedvencek"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. A **mentett keresések**, válassza ki a lekérdezést, hogy hello eredmények megtekinthetők. tooupdate hello lekérdezés, eltérő eredményeket található hello lekérdezés szerkesztése.

   ![Válassza ki a lekérdezés](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-hello-operations-management-suite-portal"></a>Keresse meg és futtassa a lekérdezések hello Operations Management Suite-portálon

1. Nyissa meg az OMS-munkaterület kezdőlapjának (`https://{your-workspace-name}.portal.mms.microsoft.com`), és válassza a **naplófájl-keresési**.

   ![Az OMS kezdőlapján válassza a "Naplófájl-keresési"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   – vagy –

   ![A hello OMS menüben válassza a "Naplófájl-keresési"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. A hello **naplófájl-keresési** kezdőlapját, válassza a **Kedvencek**.

   ![Válassza ki a "Kedvencek"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. A **mentett keresések**, válassza ki a lekérdezést, hogy hello eredmények megtekinthetők. tooupdate hello lekérdezés, eltérő eredményeket található hello lekérdezés szerkesztése.

   ![Válassza ki a lekérdezés](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a>Következő lépések

* [AS2-követési sémák](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12-követési sémák](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Egyéni követési sémák](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)