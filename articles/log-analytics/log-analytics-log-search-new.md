---
title: "az OMS szolgáltatáshoz aaaLog keresések |} Microsoft Docs"
description: "Egy naplófájl-keresési tooretrieve igényel Naplóelemzési származó adatokat.  Ez a cikk ismerteti a keresések Naplóelemzési használt új naplófájl és fogalmak toounderstand szüksége előtt hozzon létre egyet."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: 08fda1d9eb9e6ab824ffb9e12af09832c3e3fad2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-log-searches-in-log-analytics"></a>A Naplóelemzési keres ismertetése napló

> [!NOTE]
> Ez a cikk ismerteti a napló keresések az Azure Naplóelemzés hello új lekérdezési nyelv használatával.  További tudnivalók hello új nyelv, és hello eljárás tooupgrade lekérése a munkaterületen a [az Azure Naplóelemzés munkaterület toonew napló keresés frissítése](log-analytics-log-search-upgrade.md).  
>
> Ha a munkaterületet még nem frissített toohello új lekérdező nyelv, tekintse át túl[található adatokat, és napló kereséseket a Naplóelemzési](log-analytics-log-searches.md).

Egy naplófájl-keresési tooretrieve igényel Naplóelemzési származó adatokat.  Adatok hello portálon még elemzése, hogy egy riasztási szabály toobe konfigurálása értesítést kap egy adott feltétel, vagy hello napló Analytics API-t használó lekérése során adatokat egy keresési toospecify hello naplóadatokat kívánt fogja használni.  Ez a cikk ismerteti a napló keresés használata a Naplóelemzési és fogalmat tisztában kell lennie azzal előtt hozzon létre egyet. Lásd: hello [további lépések](#next-steps) szakasz létrehozása és szerkesztése a napló keresések kapcsolatos részletekért és hello lekérdezési nyelv a hivatkozásokat.

## <a name="where-log-searches-are-used"></a>Ha a napló keresések használnak

hello különböző módon, hogy szüksége lesz napló keresések Naplóelemzési hello alábbiakat foglalja magába:

- **Portálok.** Adatok elemzése interaktív hello tárházban a hello hajthat végre [naplófájl-keresési portál](log-analytics-log-search-log-search-portal.md) vagy hello [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587).  Ez lehetővé teszi, hogy tooedit a lekérdezéséhez és elemzéséhez hello eredmények különböző formátumokban és képi megjelenítéseket.  Az Ön által létrehozott irányuló legtöbb lekérdezésnek hello portálok egyikében elindul, és majd másolása után ellenőrizte, hogy az elvárt módon működik.
- **A riasztási szabályok.** [Riasztási szabályok](log-analytics-alerts.md) proaktív módon az adatokat a munkaterületen a problémák azonosításához.  Minden riasztási szabály, amely rendszeres időközönként automatikusan fut egy napló keresési alapul.  hello eredményei ellenőrzött toodetermine ha riasztást kell létrehozni.
- **Nézetek.**  A felhasználó irányítópultok szereplő adatok toobe vizuális hozhat létre [adatforrásnézet-tervezőből](log-analytics-view-designer.md).  Napló keresések adatokkal hello által használt [csempék](log-analytics-view-designer-tiles.md) és [képi megjelenítés részek](log-analytics-view-designer-parts.md) az egyes nézetek.  Részletezhető le a képi megjelenítés részeiből hello napló keresése portál tooperform további elemzés hello adatokon.
- **Exportálás.**  Ha adatainak exportálásához hello Naplóelemzési munkaterület tooExcel vagy [Power BI](log-analytics-powerbi.md), létrehozhat egy naplófájl-keresési toodefine hello adatok tooexport.
- **PowerShell.** Egy PowerShell-parancsfájlt futtathatja a parancssorban vagy egy Azure Automation-runbook által használt [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults?view=azurermps-4.0.0) Naplóelemzési tooretrieve adatait.  Ez a parancsmag egy lekérdezés toodetermine hello adatok tooretrieve igényel.
- **Napló Analytics API.**  Hello [Naplóelemzési jelentkezzen keresési API-JÁNAK](log-analytics-log-search-api.md) lehetővé teszi, hogy a REST API-ügyfél hello munkaterület tooretrieve adatait.  hello API-kérelem Naplóelemzési toodetermine hello adatok tooretrieve futtatott lekérdezés tartalmazza.

![Napló-keresések](media/log-analytics-log-search-new/log-search-overview.png)

## <a name="how-log-analytics-data-is-organized"></a>Log Analytics-adatok
-Lekérdezés összeállításához indítása meghatározása a táblák keresett hello adatokkal rendelkeznek. Minden egyes [adatforrás](log-analytics-data-sources.md) és [megoldás](../operations-management-suite/operations-management-suite-solutions.md) hello Naplóelemzési munkaterület dedikált táblákban tárolja az adatokat.  Minden adatforrás és a megoldás dokumentációja tartalmazza a hello adattípus, amely létrehozza hello nevét és leírását minden tulajdonságát.     Sok lekérdezések csak egyetlen táblázatok adatait, de különböző beállítások tooinclude adatokat több táblából mások is használhatják.

![Táblák](media/log-analytics-log-search-new/queries-tables.png)


## <a name="writing-a-query"></a>A lekérdezés írása
A napló hello fő megkeresi a Naplóelemzési van [egy részletes lekérdező nyelv](https://docs.loganalytics.io/) , amely lehetővé teszi beolvasása és elemezhetik az sokféleképpen hello tárházból.  Ugyanezen a nyelven lekérdezés használt [Application Insights](../application-insights/app-insights-analytics.md).  Tanulási toowrite lekérdezés Mitől kritikus toocreating napló megkeresi a Naplóelemzési.  Az alapvető lekérdezések általában kell kezdődnie, és majd folyamatban toouse fejlettebb, Funkciók, a követelmények még bonyolultabbá válnak.

hello alapszintű struktúrát a lekérdezés az operátorok a függőleges vonás elválasztott sorozatát követ forrástábla `|`.  Tanúsítványlánc egyszerre több operátorok toorefine hello adatokat, és hajtsa végre a speciális funkciók.

Tegyük fel, hogy keresett toofind hello felső tíz rendelkező számítógépek hello legtöbb hibaesemények keresztül hello elmúlt nap.

    Event
    | where (EventLevelName == "Error")
    | where (TimeGenerated > ago(1days))
    | summarize ErrorCount = count() by Computer
    | top 10 by ErrorCount desc

Vagy például toofind számítógépek, amelyek még nem volt szívverés hello utolsó nap.

    Heartbeat
    | where TimeGenerated > ago(7d)
    | summarize max(TimeGenerated) by Computer
    | where max_TimeGenerated < ago(1d)  

Mi a helyzet a sor diagramot hello processzorhasználata az elmúlt hét számítógépenként?

    Perf
    | where ObjectName == "Processor" and CounterName == "% Processor Time"
    | where TimeGenerated  between (startofweek(ago(7d)) .. endofweek(ago(7d)) )
    | summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)
    | render timechart    

A következő gyors mintákat, függetlenül attól, milyen hello dolgozik, adatok hello látható hello lekérdezés szerkezete hasonlít.  Oszthatja azt különböző lépéseket, ahol hello eredményül kapott adatokat egy parancs hello adatcsatorna toohello a következő parancs használatával küldi el azokat.

Az hello Azure Log Analytics lekérdezési nyelv oktatóanyagok és nyelvi referencia többek között a teljes dokumentációjáért lásd: hello [Azure Log Analytics lekérdezési nyelv dokumentáció](https://docs.loganalytics.io/).

## <a name="next-steps"></a>Következő lépések

- További tudnivalók: hello [portálok toocreate használni, és szerkesztésére napló](log-analytics-log-search-portals.md).
- Tekintse meg a [útmutató a lekérdezések írásáról](https://go.microsoft.com/fwlink/?linkid=856078) hello új lekérdezési nyelv használatával.
