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
# <a name="monitor-and-get-insights-about-logic-app-runs-with-operations-management-suite-oms-and-log-analytics"></a>Logikai alkalmazás figyelése és a get észrevételeket fut, az Operations Management Suite (OMS) és a Naplóelemzési

Figyelési és gazdagabb hibakeresési információ bekapcsolása Naplóelemzési: hello ugyanannyi időt vesz igénybe, ha logikai alkalmazás létrehozása. A Naplóelemzési biztosít a diagnosztika naplózásának és figyelésének a logikai alkalmazásnak hello Operations Management Suite (OMS) portálon keresztül futtatja. Hello Logic Apps felügyeleti megoldás tooOMS hozzáadásakor összesített állapotának beolvasása a logic app futtatása és a kívánt részletes adatok, például állapot, a végrehajtási idő, a ismételt továbbítása során állapot és a korrelációs azonosító.

Ez a témakör bemutatja, hogyan tooturn Naplóelemzési vagy a telepítés hello-e az OMS Logic Apps felügyeleti megoldás, futásidejű események és a logikai alkalmazásnak adatok futtathatók.

 > [!TIP]
 > toomonitor a meglévő logic apps, kövesse az alábbi lépéseket túl [diagnosztikai naplózás bekapcsolásához és a logic app futásidejű adatok tooOMS küldése](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

## <a name="requirements"></a>Követelmények

Mielőtt elkezdené, toohave OMS-munkaterület szüksége. Ismerje meg, [hogyan toocreate OMS-munkaterület](../log-analytics/log-analytics-get-started.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>A logic apps létrehozásakor diagnosztikai naplózás bekapcsolása

1. A [Azure-portálon](https://portal.azure.com), logikai alkalmazás létrehozása. Válasszon **új** > **vállalati integrációs** > **logikai alkalmazás** > **létrehozása**.

   ![Logikai alkalmazás létrehozása](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

2. A hello **hozzon létre logikai alkalmazás** lapján látható ezen feladatok végrehajtásával:

   1. Adjon meg egy nevet a Logic Apps alkalmazást, és válassza ki az Azure-előfizetéshez. 
   2. Hozzon létre vagy válasszon ki egy Azure-erőforráscsoportot.
   3. Állítsa be **Naplóelemzési** túl**a**. 
   Jelölje be hello OMS-munkaterület, ahová a logikai alkalmazásnak túl adatküldés fut. 
   4. Ha elkészült, válassza ki a **PIN-kód toodashboard** > **létrehozása**.

      ![Logikai alkalmazás létrehozása](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      Ez a lépés befejezése után az Azure létrehoz a logikai alkalmazás, amely most már az OMS-munkaterület társított. 
      Emellett ebben a lépésben is automatikusan telepíti hello Logic Apps-kezelési megoldás az OMS-munkaterület.

3. a Logic Apps alkalmazást futtat OMS-ben, tooview [folytassa a következő lépéseket](#view-logic-app-runs-oms).

## <a name="install-hello-logic-apps-management-solution-in-oms"></a>Az OMS hello Logic Apps-kezelési megoldás telepítése

Ha Ön már engedélyezve van a Naplóelemzési a logikai alkalmazás létrehozása után, kihagyhatja ezt a lépést. Már van telepítve az OMS hello Logic Apps felügyeleti megoldás.

1. A hello [Azure-portálon](https://portal.azure.com), válassza a **több szolgáltatások**. Keresse meg a "naplóelemzési" szűrőként, és válassza a **Naplóelemzési** látható módon:

   ![Válassza ki a "Naplóelemzési"](media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

2. A **Naplóelemzési**, található, és válassza ki az OMS-munkaterület. 

   ![Az OMS-munkaterület kiválasztása](media/logic-apps-monitor-your-logic-apps-oms/select-logic-app.png)

3. A **felügyeleti**, válassza a **OMS-portálon**.

   ![Válassza ki a "OMS-portálon"](media/logic-apps-monitor-your-logic-apps-oms/oms-portal-page.png)

4. A OMS kezdőlap hello frissítési szalagcím akkor jelenik meg, ha válasszon hello szalagcím, hogy az OMS-munkaterület először frissítenie. Válassza a **megoldások gyűjtemény**.

   ![Válassza ki a "Megoldások gyűjtemény"](media/logic-apps-monitor-your-logic-apps-oms/solutions-gallery.png)

5. A **minden megoldás**, található, és válassza ki a hello csempéjére a hozzá tartozó hello **Logic Apps felügyeleti** megoldás.

   ![Válassza ki a "Logic Apps kezelése"](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-management-tile2.png)

6. az OMS-munkaterület tooinstall hello megoldás kiválasztása **Hozzáadás**.

   ![Válassza a "Hozzáadás" a "Logic Apps kezelése"](media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-your-logic-app-runs-in-your-oms-workspace"></a>A Logic Apps alkalmazást futtat az OMS-munkaterület megjelenítése

1. tooview hello számát és a logikai alkalmazás állapotának fut, nyissa meg toohello az OMS-munkaterület áttekintő lapja. Tekintse át a hello hello részleteket **Logic Apps felügyeleti** csempére.

   ![Logic app futtatása száma és állapotát megjelenítő áttekintés csempe](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   > [!Note]
   > Ha a frissítési szalagcím hello Logic Apps felügyeleti csempe nem jelenik meg, válassza ki a hello transzparens, hogy az OMS-munkaterület először frissítenie.
  
   > ![A frissítés "OMS-munkaterület"](media/logic-apps-monitor-your-logic-apps-oms/oms-upgrade-banner.png)

2. tooview összegzését, amelyen további információkat talál a logic app futtatja, válasszon hello **Logic Apps felügyeleti** csempére.

   A logic app fut itt, név, illetve végrehajtási állapot szerint vannak csoportosítva.

   ![Állapotának összegzése a Logic Apps alkalmazást futtat](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
3. az összes hello tooview futtatása egy adott logikai alkalmazás vagy az állapot, a logikai alkalmazás vagy egy állapot válassza hello sort.

   Itt a következő példa bemutatja, az adott logikai alkalmazás összes hello fut:

   ![A logikai alkalmazást vagy egy állapot nézet futtatások](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   > [!NOTE]
   > Hello **meghiúsultak** az oszlopban látható a "Yes" újraküldött futtató a kísérletekhez.

4. toofilter ezek annak az eredménye, hajthat végre az ügyféloldali és a kiszolgálóoldali szűrés.

   * Ügyféloldali szűrő: az oszlopok, válassza ki a kívánt hello szűrőket. 
   Néhány példa:

     ![Példa oszlopszűrők](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * Kiszolgálóoldali szűrés: toochoose adott időpont ablak vagy toolimit hello számos fut, amely megjelenik, használjon hello hatókör vezérlő hello oldal hello tetején. 
   Alapértelmezés szerint csak 1000 rekordok jelennek meg egyszerre. 
   
     ![Változás hello időkerete](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
5. minden tooview hello műveletek és egy adott futtatási, válassza ki az adataikat egymás után, amely hello napló keresése oldal megnyitása. 

   * tooview ezt az információt a tábla válasszon **tábla**.
   * toochange hello lekérdezés, szerkesztheti hello keresősávban hello lekérdezési karakterláncot. 
   A jobb teljesítmény érdekében válasszon **Advanced Analytics**.

     ![Műveletek és a Futtatás logikai alkalmazás részleteinek megtekintése](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)

     Itt hello Azure Naplóelemzés lapján frissítheti lekérdezések és nézet hello eredmények hello táblából. 
     Ez a lekérdezés használ [Kusto lekérdezési nyelv](https://docs.loganalytics.io/learn/tutorials/getting_started_with_queries.html), amelyen szerkesztheti, ha azt szeretné, hogy tooview eltérő eredményt. 

     ![Az Azure Log Analytics - lekérdezési nézet](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Következő lépések

* [B2B üzenetek megfigyelése](../logic-apps/logic-apps-monitor-b2b-message.md)
