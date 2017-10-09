---
title: "aaaGet Azure figyelőjének |} Microsoft Docs"
description: "Azure-figyelő toogain betekintést hello művelet az erőforrások használatának megkezdése és kérdéskörének ki adatokat."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ce2930aa-fc41-4b81-b0cb-e7ea922467e1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: johnkem
ms.openlocfilehash: 49e7105621a9e60c9fa14c4911c4fe45e0d197a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-monitor"></a>Ismerkedés az Azure Monitor szolgáltatással
Az Azure figyelőjének értéke a hello platform szolgáltatás, amely egyetlen helyről az Azure-erőforrások figyelése. Azure megfigyelővel ábrázolhatja, lekérdezése, irányíthatja, archiválására, és művelet végrehajtása a hello metrikák és a naplók az Azure-erőforrások származik. Ezek az adatok hello figyelő portálpanelén dolgozhat [figyelő PowerShell-parancsmagok](insights-powershell-samples.md), [platformfüggetlen parancssori felület](insights-cli-samples.md), vagy [Azure figyelő REST API-k](https://msdn.microsoft.com/library/dn931943.aspx). Ez a cikk azt végezze el néhány a legfontosabb összetevők hello Azure figyelő bemutatásához hello portál használatával.

1. Hello portálon lépjen túl**további szolgáltatások** és hello található **figyelő** lehetőséget. Kattintson a hello csillagra tooadd ezt a beállítást tooyour kedvencei úgy, hogy az mindig hello bal oldali navigációs sáv könnyen elérhető.
   
    ![Hello szolgáltatások listában figyelése](./media/monitoring-get-started/monitor-more-services.png)
2. Hello kattintson **figyelő** beállítás tooopen mentése hello **figyelő** panelen. Ez a panel egyetlen, összevont nézetben jeleníti meg az összes figyelési beállítást és adatot. Először megnyitja a toohello **tevékenységnapló** szakasz.
   
    ![Navigálás a Monitor panelen](./media/monitoring-get-started/monitor-blade-nav.png)
   
    Az Azure figyelőknek a figyelési adatok három fő kategóriába: hello **tevékenységnapló**, **metrikák**, és **diagnosztikai naplók**.
3. Kattintson a **tevékenységnapló** , amelyek a tevékenység napló szakasz hello tooensure jelenik meg.
   
    ![Tevékenységnapló panel](./media/monitoring-get-started/monitor-act-log-blade.png)
   
    Hello [ **tevékenységnapló** ](monitoring-overview-activity-logs.md) erőforrást az előfizetésében végrehajtani minden műveletnél ismerteti. Az hello tevékenységnapló is meghatározható hello "mi, ki, és mikor" létrehozott, módosítja vagy törli az erőforrást az előfizetésében műveleteket. Például hello tevékenységnapló megtudhatja, ha a webalkalmazás le lett állítva, és akik megszakította. Tevékenység naplóeseményeket hello platform és tárolja elérhető tooquery 90 napig.
   
    Hozhat létre és mentsen lekérdezéseket, közös szűrők, majd a PIN-kód hello legfontosabb lekérdezések tooa portál Irányítópultjára, hogy minden esetben ha a megadott feltételeket teljesítő eseményekről.
4. Hello nézet tooa adott erőforráscsoport szűrése hello múlt héten keresztül, majd kattintson az hello **mentése** gombra.
   
    ![Tevékenységnapló lekérdezésének mentése](./media/monitoring-get-started/monitor-act-log-save.png)
5. Most kattintson hello **PIN-kód** gombra.
   
    ![Kattintás a Rögzítés gombra a tevékenységnaplóban](./media/monitoring-get-started/monitor-act-log-pin.png)
   
    Hello nézetek a forgatókönyv a legtöbb rögzített tooa irányítópult lehet. Ezt kihasználva létrehozhat egy olyan információs felületet, amelyet egyetlen forrásként használhat a szolgáltatásai működési adataihoz. 
6. Tooyour irányítópult visszaadása. Most láthatja, hogy hello lekérdezés (és az eredmények számát) megjelenik az irányítópulton. Ez akkor hasznos, ha azt szeretné, tooquickly lásd történt mostanában az előfizetésében, pl. magas-profil műveleteket. egy új szerepkör lett hozzárendelve, vagy egy virtuális gépet töröltek.
   
    ![Tevékenység naplóban rögzített toodashboard](./media/monitoring-get-started/monitor-act-log-db.png)
7. Térjen vissza a toohello **figyelő** csempére, majd kattintson a hello **metrikák** szakasz. Először tooselect erőforrás szűrés, majd válassza a beállítások hello legördülő használatával hello panel hello tetején.
   
    ![Erőforrás szűrése metrikákhoz](./media/monitoring-get-started/monitor-met-filter.png)
   
    Minden Azure-erőforrás szolgáltat [**metrikákat**](monitoring-overview-metrics.md). Ez a nézet egyetlen panelen gyűjti össze az összes metrikát, így Ön könnyen elemezheti az erőforrások működését.
8. A kijelölt erőforrás hello bal oldalán található hello panel az összes elérhető mérőszámok jelennek meg. Több metrikák egyszerre diagram metrikák kiválasztásával, és módosítsa a hello graph típus és a kívánt időtartományt. Az adott erőforráshoz beállított összes metrikariasztást is megtekintheti.
   
    ![Metrika panel](./media/monitoring-get-started/monitor-metric-blade.png)
   
   > [!NOTE]
   > Bizonyos metrikák csak akkor érhetők el, ha engedélyezi az [Application Insights](../application-insights/app-insights-overview.md) és/vagy a Windows- vagy Linux-alapú Azure Diagnostics szolgáltatást az erőforráson.
   > 
   > 
9. Ha elégedett a diagramon, használhatja a hello **PIN-kód** gomb toopin azt tooyour irányítópult.
10. Térjen vissza a toohello **figyelő** panel megnyitásához, és kattintson **diagnosztikai naplók**.
    
    ![Diagnosztikai naplók panel](./media/monitoring-get-started/monitor-diaglogs-blade.png)
    
    [**Diagnosztikai naplók** ](monitoring-overview-of-diagnostic-logs.md) kibocsátott naplók *által* egy erőforrást, amely az adott erőforráshoz hello műveletekre vonatkozó adatokat. Például a hálózati biztonsági csoportok szabályainak számlálói, illetve a Logic Apps-munkafolyamatok naplói is a diagnosztikai naplók egy-egy típusát képezik. Ezek a naplók egy tárfiókot, adatfolyamként továbbított tooan Eseményközpont, tárolja, illetve túl küldött[Naplóelemzési](../log-analytics/log-analytics-overview.md). A Log Analytics a Microsoft speciális keresésekhez és riasztásokhoz használható operatív információs terméke.
    
    Hello portálon tekintheti meg és szűrheti az előfizetés tooidentify az összes erőforrás listáját, ha a diagnosztikai naplók engedélyezve van.
11. Kattintson egy erőforrás hello diagnosztikai naplók panelen. Ha a diagnosztikai naplókat egy tárfiókban tárolja, látni fogja az óránként létrehozott naplók listáját, és közvetlenül innen le is töltheti őket.
    
    ![Diagnosztikai naplók egy erőforráshoz](./media/monitoring-get-started/monitor-diaglogs-detail.png)
    
    Is **diagnosztikai beállítások**, amely lehetővé teszi fel tooset beállításokat, vagy módosítsa a archiválási tooa tárfiók, streaming tooEvent hubok, vagy tooa Naplóelemzési munkaterület küldésekor.
    
    ![Diagnosztikai naplók engedélyezése](./media/monitoring-get-started/monitor-diaglogs-enable.png)
    
    Ha a diagnosztikai naplók tooLog Analytics beállítását követően is kereshet őket a hello **naplófájl-keresési** hello portál szakaszban.
12. Keresse meg a toohello **riasztások** hello figyelő panel szakasza.
    
    ![Nyilvános riasztások panel](./media/monitoring-get-started/monitor-alerts-nopp.png)
    
    Itt kezelheti az Azure-erőforrások összes [**riasztását**](monitoring-overview-alerts.md). Ez magában foglalja a riasztások a metrikákat, naplózási eseményeket, az Application Insights webalkalmazás-tesztek (hely) és az Application Insights proaktív diagnosztika. Riasztások is elindíthatja az elküldött e-mailek toobe vagy egy HTTP POST tooa webhook URL-CÍMÉT.
13. Kattintson a **metrika riasztás hozzáadása** toocreate riasztást.
    
    ![Metrikariasztás hozzáadása](./media/monitoring-get-started/monitor-alerts-add.png)
    
    Ezután a PIN-kód egy riasztási tooyour irányítópult tooeasily bármikor lásd: állapotában.
14. a figyelő szakasz hello hivatkozások is túl[Application Insights](../application-insights/app-insights-overview.md) alkalmazások és [Naplóelemzési](../log-analytics/log-analytics-overview.md) megoldásokat. Ezek az egyéb Microsoft-termékek szervesen az Azure Monitor szolgáltatásba vannak integrálva.
15. Ha nem használja az Application Insights vagy a Log Analytics szolgáltatást, az Azure Monitor valószínűleg jelenlegi figyelési, naplózási és riasztási termékeivel működik együtt. Tekintse meg a [alkalmazáspartnerek oldalán](monitoring-partners.md) teljes listáját, valamint útmutatást a toointegrate.

Kövesse a következő lépést, és az összes releváns csempék tooa irányítópulton rögzítési, az alkalmazás és az infrastruktúra ehhez hasonló átfogó nézeteket hozhat létre:

![Az Azure Monitor irányítópultja](./media/monitoring-get-started/monitor-final-dash.png)

## <a name="next-steps"></a>Következő lépések
* Olvasási hello [Azure figyelője – áttekintés](monitoring-overview.md)

