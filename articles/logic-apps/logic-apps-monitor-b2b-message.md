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
# <a name="monitor-and-set-up-diagnostics-logging-for-b2b-communication-in-integration-accounts"></a>Figyelheti és integrációs fiókok B2B kommunikáció diagnosztikai naplózás beállítása

Miután beállította a B2B kommunikációját két üzleti folyamatok vagy a integrációs fiókon keresztül alkalmazásokat futtató entitásokból tudjon cserélni egymással üzeneteket. tooconfirm Ez a kommunikáció megfelelően működik-e, beállíthatja az AS2, X12, figyelés és EDIFACT-üzenetek, együtt a hello integrációs fiókhoz diagnosztikai naplózás [Azure Naplóelemzés](../log-analytics/log-analytics-overview.md) szolgáltatás. Ez a szolgáltatás [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) figyeli a felhőalapú és helyszíni környezetben, így azok rendelkezésre állását és teljesítményét, karbantartása és is a futásidejű részleteit, illetve a szélesebb hibakereséshez eseményeket gyűjti. Emellett [használja a diagnosztikai adatok más szolgáltatásokkal](#extend-diagnostic-data), például az Azure Storage és az Azure Event Hubs.

## <a name="requirements"></a>Követelmények

* Egy logikai alkalmazást a diagnosztikai naplózás be van állítva. Ismerje meg, [hogyan tooset be a naplózást az adott logikai alkalmazás](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

  > [!NOTE]
  > Ez a követelmény teljesítette, kell után hello munkaterületeinek [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Használjon ugyanazon OMS-munkaterület hello integrációs fiókja naplózás beállítása során. Ha még nem rendelkezik az OMS-munkaterület, [hogyan toocreate OMS-munkaterület](../log-analytics/log-analytics-get-started.md).

* Integráció fiók, amely tooyour logikai alkalmazás van kapcsolva. Ismerje meg, [hogyan toocreate integrációs hivatkozás tooyour logikai alkalmazás fiók](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).

## <a name="turn-on-diagnostics-logging-for-your-integration-account"></a>Diagnosztika integrációs fiókja naplózásának engedélyezése

Vagy közvetlenül integrációs fiókjából naplózás bekapcsolása vagy [keresztül hello Azure figyelőszolgáltatás](#azure-monitor-service). Az Azure biztosít alapvető figyelési infrastruktúra szintű adatokkal. További információ [Azure figyelő](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md).

### <a name="turn-on-diagnostics-logging-directly-from-your-integration-account"></a>Diagnosztika közvetlen integráció fiókjából naplózás engedélyezése

1. A hello [Azure-portálon](https://portal.azure.com), található, és válassza ki a integrációs fiókját. A **figyelés**, válassza a **diagnosztikai naplók** itt látható módon:

   ![Keresés, és válassza ki a integrációs fiókját, válassza a "Diagnosztikai naplók"](media/logic-apps-monitor-b2b-message/integration-account-diagnostics.png)

2. Miután kiválasztotta a integrációs fiókját, a következő értékek hello automatikusan ki van jelölve. Ha ezek az értékek megfelelőek, válassza ki a **a diagnosztika bekapcsolásához**. Amennyiben nem válassza a kívánt hello értékeket:

   1. A **előfizetés**, válassza ki az Azure-előfizetést, amelyet a integrációs fiók hello.
   2. A **erőforráscsoport**, jelölje be hello erőforráscsoport-integráció fiókjához használt.
   3. A **erőforrástípus**, jelölje be **integrációs fiókok**. 
   4. A **erőforrás**, válassza ki a integrációs fiókját. 
   5. Válasszon **a diagnosztika bekapcsolásához**.

   ![Diagnosztika a integrációs fiók beállítása](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. A **diagnosztikai beállítások**, majd **állapot**, válassza a **a**.

   ![Kapcsolja be az Azure Diagnostics](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Immár hello OMS munkaterületet, és az adatok a naplózás toouse látható módon:

   1. Válassza ki **tooLog Analytics küldése**. 
   2. A **Naplóelemzési**, válassza a **konfigurálása**. 
   3. A **OMS-munkaterület**, jelölje be az OMS-munkaterület toouse hello a naplózás.
   4. A **napló**, jelölje be hello **IntegrationAccountTrackingEvents** kategóriát.
   5. Válassza a **Mentés** elemet.

   ![Diagnosztikai adatok tooa naplót elküldheti a Naplóelemzési beállítása](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Most [állítsa be az OMS B2B üzenetek nyomon követése](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

<a name="azure-monitor-service"></a>

### <a name="turn-on-diagnostics-logging-through-azure-monitor"></a>Diagnosztikai naplózás keresztül Azure-figyelő bekapcsolása

1. A hello [Azure-portálon](https://portal.azure.com), a hello Azure főmenü, majd a **figyelő**, **diagnosztikai naplók**. Ezután válassza ki a integrációs fiókját, ahogy az itt látható:

   ![Válassza a "Figyelése", "A diagnosztikai naplók", az integráció fiók kiválasztása](media/logic-apps-monitor-b2b-message/monitor-service-diagnostics-logs.png)

2. Miután kiválasztotta a integrációs fiókját, a következő értékek hello automatikusan ki van jelölve. Ha ezek az értékek megfelelőek, válassza ki a **a diagnosztika bekapcsolásához**. Amennyiben nem válassza a kívánt hello értékeket:

   1. A **előfizetés**, válassza ki az Azure-előfizetést, amelyet a integrációs fiók hello.
   2. A **erőforráscsoport**, jelölje be hello erőforráscsoport-integráció fiókjához használt.
   3. A **erőforrástípus**, jelölje be **integrációs fiókok**.
   4. A **erőforrás**, válassza ki a integrációs fiókját.
   5. Válasszon **a diagnosztika bekapcsolásához**.

   ![Diagnosztika a integrációs fiók beállítása](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. A **diagnosztikai beállítások**, válassza a **a**.

   ![Kapcsolja be az Azure Diagnostics](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Most válasszon hello OMS munkaterületet, és az esemény naplózási kategóriát látható módon:

   1. Válassza ki **tooLog Analytics küldése**. 
   2. A **Naplóelemzési**, válassza a **konfigurálása**. 
   3. A **OMS-munkaterület**, jelölje be az OMS-munkaterület toouse hello a naplózás.
   4. A **napló**, jelölje be hello **IntegrationAccountTrackingEvents** kategóriát.
   5. Amikor elkészült, válassza ki a **mentése**.

   ![Diagnosztikai adatok tooa naplót elküldheti a Naplóelemzési beállítása](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Most [állítsa be az OMS B2B üzenetek nyomon követése](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Hogyan és hol diagnosztikai adatok használja más szolgáltatásokkal

Azure Naplóelemzés, valamint bővítheti, hogyan használhatja a Logic Apps alkalmazást diagnosztikai adatok más Azure-szolgáltatásokkal, például: 

* [Az Azure diagnosztikai naplók az Azure Storage archív](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [Az adatfolyam Azure diagnosztikai naplók tooAzure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

Ezután a get valós idejű figyelés telemetriai adatok és más szolgáltatások analytics segítségével például [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) és [Power BI](../log-analytics/log-analytics-powerbi.md). Példa:

* [Az Event Hubs tooStream Analytics adatfolyam adatait](../stream-analytics/stream-analytics-define-inputs.md)
* [A Stream Analytics adatfolyam-továbbítási adatok elemzése és a Power bi-ban a valós idejű elemzési irányítópult létrehozása](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Alapján hello beállítása kívánt beállításokat, győződjön meg arról, hogy Ön első [az Azure storage-fiók létrehozása](../storage/common/storage-create-storage-account.md) vagy [hozzon létre egy Azure event hubs](../event-hubs/event-hubs-create.md). Ezután válasszon hello beállítások ahová toosend diagnosztikai adatokat:

![TooAzure tárolási fiók vagy esemény hub adatok küldése](./media/logic-apps-monitor-b2b-message/storage-account-event-hubs.png)

> [!NOTE]
> Megőrzési időtartamú csak válasszon toouse tárfiókot vonatkozik.

## <a name="supported-tracking-schemas"></a>Támogatott sémák nyomon követése

Azure támogatja ezeket sématípusok, amely azzal a különbséggel egyéni típus hello sémák rögzített nyomon követését.

* [AS2-követési séma](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12-követési séma](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Egyéni követési séma](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Következő lépések

* [Az OMS B2B üzenetek nyomon](../logic-apps/logic-apps-track-b2b-messages-omsportal.md "OMS követése B2B üzenetek")
* [További tudnivalók a vállalati integrációs csomag hello](../logic-apps/logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")

