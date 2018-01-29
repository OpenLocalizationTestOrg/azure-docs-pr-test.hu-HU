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
ms.openlocfilehash: 8da2bc9645e432ddf0e9f627c7b5e30c44fd74b6
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/20/2017
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-operations-management-suite-oms-and-log-analytics"></a>Logikai alkalmazás figyelése és a get észrevételeket fut, az Operations Management Suite (OMS) és a Naplóelemzési

Figyelési és gazdagabb hibakeresési információ bekapcsolása Naplóelemzési logikai alkalmazás létrehozásakor egy időben. A Naplóelemzési biztosít naplózásának és figyelésének a logikai alkalmazásnak diagnosztika futtatása az Operations Management Suite (OMS) portálon keresztül. A Logic Apps-kezelési megoldás az OMS-be való hozzáadásakor a logic app futtatása és a kívánt részletes adatok, például állapot, a végrehajtási idő, a ismételt továbbítása során állapot és a korrelációs azonosító lekérése összesített állapotát.

Ez a témakör bemutatja, hogyan Naplóelemzési be-és a Logic Apps-kezelési megoldás telepítése OMS, tekintse meg a futtatókörnyezet események és az adatok a Logic Apps alkalmazást futtatni.

 > [!TIP]
 > A meglévő logic Apps alkalmazások figyeléséhez, az alábbi lépéseket követve [diagnosztikai naplózás bekapcsolásához és a logic app futásidejű adatokat küldeni a OMS](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

## <a name="requirements"></a>Követelmények

Kezdés előtt kell az OMS-munkaterület rendelkezik. Ismerje meg, [OMS-munkaterület létrehozása](../log-analytics/log-analytics-get-started.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>A logic apps létrehozásakor diagnosztikai naplózás bekapcsolása

1. A [Azure-portálon](https://portal.azure.com), logikai alkalmazás létrehozása. Válasszon **új** > **vállalati integrációs** > **logikai alkalmazás** > **létrehozása**.

   ![Logikai alkalmazás létrehozása](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

2. Az a **hozzon létre logikai alkalmazás** lapján látható ezen feladatok végrehajtásával:

   1. Adjon meg egy nevet a Logic Apps alkalmazást, és válassza ki az Azure-előfizetéshez. 
   2. Hozzon létre vagy válasszon ki egy Azure-erőforráscsoportot.
   3. Állítsa be **Analytics jelentkezzen** való **a**. 
   Válassza ki az OMS-munkaterület, ahol szeretné elküldeni a adatait a Logic Apps alkalmazást futtat. 
   4. Ha elkészült, válassza ki a **rögzítés az irányítópulton** > **létrehozása**.

      ![Logikai alkalmazás létrehozása](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      Ez a lépés befejezése után az Azure létrehoz a logikai alkalmazás, amely most már az OMS-munkaterület társított. 
      Emellett ebben a lépésben is automatikusan telepíti a Logic Apps-kezelési megoldás az OMS-munkaterület.

3. Megtekintheti a logic app futó OMS-ben, [folytassa a következő lépéseket](#view-logic-app-runs-oms).

## <a name="install-the-logic-apps-management-solution-in-oms"></a>Az OMS a Logic Apps-kezelési megoldás telepítése

Ha Ön már engedélyezve van a Naplóelemzési a logikai alkalmazás létrehozása után, kihagyhatja ezt a lépést. Már van a Logic Apps felügyeleti megoldás, OMS telepítve.

1. Az a [Azure-portálon](https://portal.azure.com), válassza a **több szolgáltatások**. Keresse meg a "naplóelemzési" szűrőként, és válassza a **Naplóelemzési** látható módon:

   ![Válassza ki a "Naplóelemzési"](media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

2. A **Naplóelemzési**, található, és válassza ki az OMS-munkaterület. 

   ![Az OMS-munkaterület kiválasztása](media/logic-apps-monitor-your-logic-apps-oms/select-logic-app.png)

3. A **felügyeleti**, válassza a **OMS-portálon**.

   ![Válassza ki a "OMS-portálon"](media/logic-apps-monitor-your-logic-apps-oms/oms-portal-page.png)

4. A kezdőlapon OMS a frissítési szalagcím akkor jelenik meg, ha válassza ki a szalagcím, hogy az OMS-munkaterület először frissítenie. Válassza a **megoldások gyűjtemény**.

   ![Válassza ki a "Megoldások gyűjtemény"](media/logic-apps-monitor-your-logic-apps-oms/solutions-gallery.png)

5. A **minden megoldás**, található, és válassza ki a csempe a **Logic Apps felügyeleti** megoldás.

   ![Válassza ki a "Logic Apps kezelése"](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-management-tile2.png)

6. Az OMS-munkaterület a megoldás telepítéséhez válassza **Hozzáadás**.

   ![Válassza a "Hozzáadás" a "Logic Apps kezelése"](media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-your-logic-app-runs-in-your-oms-workspace"></a>A Logic Apps alkalmazást futtat az OMS-munkaterület megjelenítése

1. Számát és a logic app kísérletekhez állapotának megtekintéséhez nyissa meg a az OMS-munkaterület áttekintő lapja. Tekintse át a részleteket a a **Logic Apps felügyeleti** csempére.

   ![Logic app futtatása száma és állapotát megjelenítő áttekintés csempe](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   > [!Note]
   > Ha a frissítési szalagcím akkor jelenik meg, a Logic Apps felügyeleti csempe helyett, válassza ki azt a transzparens, hogy az OMS-munkaterület először frissítenie.
  
   > ![A frissítés "OMS-munkaterület"](media/logic-apps-monitor-your-logic-apps-oms/oms-upgrade-banner.png)

2. További információt a logic app futtatása az összefoglaló megtekintéséhez válassza a **Logic Apps felügyeleti** csempére.

   A logic app fut itt, név, illetve végrehajtási állapot szerint vannak csoportosítva. A műveleteket vagy a logic app kísérletekhez Eseményindítók hibáinak adatait is megtekintheti.

   ![Állapotának összegzése a Logic Apps alkalmazást futtat](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
3. Az összes fut, egy adott logikai alkalmazást vagy az állapot megtekintéséhez jelölje ki a logikai alkalmazás vagy egy állapotát.

   Íme egy példa, amely megjeleníti az adott logikai alkalmazás a fut:

   ![A logikai alkalmazást vagy egy állapot nézet futtatások](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   Ezen a lapon két speciális lehetőség áll rendelkezésre:
   * **Nyomon követheti a tulajdonságok:** ebben az oszlopban látható a nyomon követett tulajdonságok, műveletek, a logikai alkalmazás szerint vannak csoportosítva. A nyomon követett tulajdonságainak megtekintéséhez válassza **nézet**. Az oszlop szűrő segítségével kereshet a nyomon követett tulajdonságok.
   
     ![A nyomon követett logikai alkalmazás tulajdonságainak megtekintése](media/logic-apps-monitor-your-logic-apps-oms/logic-app-tracked-properties.png)

     Minden újonnan hozzáadott nyomon követett tulajdonság csak akkor jelennek meg, hogy első alkalommal 10 – 15 percet vehet igénybe. Ismerje meg, [nyomon követett tulajdonságok hozzáadása a logikai alkalmazás](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details).

   * **Küldje el újból:** is küldje el újra egy vagy több logikai alkalmazás futtatása sikertelen, a sikeres volt, vagy futó. Jelölje be a jelölőnégyzeteket a kísérletekhez küldje el újra, és válassza a kívánt **küldje el újra**. 

     ![Küldje el újra a logic app fut.](media/logic-apps-monitor-your-logic-apps-oms/logic-app-resubmit.png)

4. Az eredmények szűréséhez végezheti el az ügyféloldali és a kiszolgálóoldali szűrés.

   * Ügyféloldali szűrő: az oszlopok, válassza ki a kívánt szűrőket. 
   Néhány példa:

     ![Példa oszlopszűrők](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * Kiszolgálóoldali szűrés: Válasszon egy olyan adott időkeretet, vagy fut, amely megjelenik a számát, a hatókör vezérlőt használja az oldal tetején. 
   Alapértelmezés szerint csak 1000 rekordok jelennek meg egyszerre. 
   
     ![Az időszak módosítása](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
5. Minden művelet és az adatait a megadott futtató megtekintéséhez válasszon ki egy logikai alkalmazást futtatni egy sort.

   Íme egy példa, amely egy adott logikai alkalmazás futtatása az összes műveletet jeleníti meg:

   ![A logikai alkalmazás nézet műveletek futtatása](media/logic-apps-monitor-your-logic-apps-oms/logic-app-action-details.png)
   
6. Bármely eredmények lapon, a lekérdezés mögött az eredmények megtekintéséhez vagy az összes eredmény megjelenítéséhez válasszon **tekintse meg az összes**, a keresési napló lapon nyílik meg, amely.
   
   ![Minden eredmények lapokon lásd:](media/logic-apps-monitor-your-logic-apps-oms/logic-app-seeall.png)
   
   A napló lapon,
   * Egy tábla a lekérdezés eredményeinek megtekintéséhez válassza **tábla**.
   * Ha módosítani szeretné a lekérdezést, szerkesztheti a lekérdezési karakterláncot a keresési sávon. 
   A jobb teljesítmény érdekében válasszon **Advanced Analytics**.

     ![Műveletek és a Futtatás logikai alkalmazás részleteinek megtekintése](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)
     
     Itt az Azure Naplóelemzés oldalon frissítheti lekérdezések és az eredmények megtekintése a táblából. 
     Ez a lekérdezés használ [Kusto lekérdezési nyelv](https://docs.loganalytics.io/docs/Language-Reference), amelyen szerkesztheti, ha meg szeretné tekinteni, eltérő eredményeket. 

     ![Az Azure Log Analytics - lekérdezési nézet](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Következő lépések

* [B2B üzenetek megfigyelése](../logic-apps/logic-apps-monitor-b2b-message.md)

