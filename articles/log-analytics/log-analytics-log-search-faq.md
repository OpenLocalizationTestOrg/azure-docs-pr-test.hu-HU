---
title: "Gyakori kérdések a aaaLog Analytics új naplófájl-keresési |} Microsoft Docs"
description: "Ez a cikk ismerteti a Naplóelemzési toohello új lekérdezési nyelv hello frissítés vonatkozó gyakran ismételt kérdések."
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
ms.date: 08/27/2017
ms.author: bwren
ms.openlocfilehash: b8664c8329fab0547f270793fa13e8cdd06ba637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-new-log-search-faq-and-known-issues"></a>Új Naplóelemzési naplófájl, keresés – gyakori kérdések és ismert problémák

A cikk tartalmaz a gyakori kérdéseket és az ismert problémákkal kapcsolatos hello frissítését [Naplóelemzési toohello új lekérdezési nyelv](log-analytics-log-search-upgrade.md).  Mielőtt meghozná hello döntési tooupgrade a munkaterület olvassa végig a teljes cikket.


## <a name="alerts"></a>Riasztások

### <a name="question-i-have-a-lot-of-alert-rules-do-i-need-toocreate-them-again-in-hello-new-language-after-i-upgrade"></a>Kérdés: riasztási szabályok számos van. Toocreate kell őket újra az új nyelven hello után frissítek?  
A riasztási szabályok nem, automatikusan átalakított toohello új keresés nyelvi a frissítés során.  


## <a name="computer-groups"></a>Számítógépcsoportok

### <a name="question-im-getting-errors-when-trying-toouse-computer-groups--has-their-syntax-changed"></a>Kérdés: azért kapom hibák toouse számítógépcsoportok tett kísérlet során.  Módosult a szintaxis?
Igen, a hello szintaxisa a számítógéppel módosítások csoportosítja a munkaterület frissítésekor.  Lásd: [számítógépcsoportokat a Log Analyticshez jelentkezzen keresések](log-analytics-computer-groups.md) részleteiről.

### <a name="known-issue-groups-imported-from-active-directory"></a>Ismert problémák: importált az Active Directory-csoportok
A számítógép (csoport) importálásra az Active Directoryból használó lekérdezés most nem lehet létrehozni.  Megoldás a probléma kijavításáig hozzon létre egy új számítógépcsoportot hello importálása az Active Directory csoport használata, és majd használni az új csoport a lekérdezésben.

Egy példa lekérdezés toocreate olyan új számítógép-csoport, amely tartalmazza az egy importált Active Directory-csoportot a következőképpen történik:

    ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "AD Group Name" and TimeGenerated >= ago(24h) | distinct Computer


## <a name="dashboards"></a>Irányítópultok

### <a name="question-can-i-still-use-dashboards-in-an-upgraded-workspace"></a>Kérdés: Is használhatom az irányítópultok egy frissített munkaterületen?
Folytatás toouse túl hozzáadott bármely csempék**saját irányítópult** , mielőtt a munkaterület lett frissítve, de nem szerkesztheti azokat a csempéket, vagy újakat vehet fel.  Továbbra is toocreate, és a nézetek szerkesztése [adatforrásnézet-tervezőből](log-analytics-view-designer.md) és irányítópultok is létrehozhat hello Azure-portálon.


## <a name="log-searches"></a>Napló-keresések

### <a name="question-i-have-saved-searches-outside-of-my-upgraded-workspace-can-i-convert-them-toohello-new-search-language-automatically"></a>Kérdés: a frissített munkaterület kívül rendelkezik mentett keresések. Képes vagyok alakíthatja át őket toohello új keresés nyelvi automatikusan?
Eszközzel hello nyelvi konverter hello napló keresése lap tooconvert szereplő minden egyes.  Nincs Nincs metódus tooautomatically konvertálás több keresések hello munkaterület frissítés nélkül.

### <a name="question-why-are-my-query-results-not-sorted"></a>Kérdés: Miért a lekérdezés eredményei nem rendezi a rendszer?
Eredmények nem hello új lekérdezési nyelv alapértelmezés szerint vannak rendezve.  Használjon hello [rendezési operátor](https://go.microsoft.com/fwlink/?linkid=856079) toosort a eredményeket egy vagy több tulajdonságát.

### <a name="known-issue-search-results-in-a-list-may-include-properties-with-no-data"></a>Ismert problémák: keresési eredmények listájában tartalmazhat adatot nem tartalmazó tulajdonságai
Adatot nem tartalmazó tulajdonságok napló keresési eredmények listájában jeleníthet meg.  Előzetes tooupgrade, ezek a tulajdonságok nem tudnák szerepelni.  A probléma kijavítjuk, hogy üres tulajdonságok nem jelennek meg.

### <a name="known-issue-selecting-a-value-in-a-chart-doesnt-display-detailed-results"></a>Ismert problémák: kiválasztja az értéket a diagramon nem jeleníti meg a részletes eredmények
Előzetes tooupgrade érték a diagramon, kiválasztásakor az alakítanák vissza kijelölt hello érték megfelelő rekordok részletes listáját.  Frissítés után csak hello egyetlen összesített sort adja vissza.  A probléma a rendszer jelenleg vizsgált.

## <a name="log-search-api"></a>Log Search API

### <a name="question-does-hello-log-search-api-get-updated-after-i-upgrade"></a>Kérdés: Nem hello napló keresése API követően módosul frissítek?
Hello [napló Search API](log-analytics-log-search-api.md) még nem frissített toohello új keresés nyelve.  Továbbra is toouse hello örökölt lekérdező nyelve az API-val, a munkaterület frissítése után is.  Frissített dokumentáció hello napló keresése API elérhető lesz, amikor frissül.


## <a name="portals"></a>Portálok

### <a name="question-should-i-use-hello-new-advanced-analytics-portal-or-keep-using-hello-log-search-portal"></a>Kérdés: Hello új Advanced Analytics portál vagy érdemes megtartani hello napló keresése portál használatával?
Láthatja, a két hello portálok összehasonlítása [portálok a létrehozása és módosítása az Azure Naplóelemzés napló lekérdezések](log-analytics-log-search-portals.md).  Különböző előnyeit mindegyike rendelkezik, ezért is hello ajánlott egy igényeinek.  Általános toowrite lekérdezések hello Advanced Analytics portálon, és illessze be más helyen, például az adatforrásnézet-tervezőből.  Olvasson utána [tooconsider problémák](log-analytics-log-search-portals.md#advanced-analytics-portal) , hogy mikor történt.


## <a name="power-bi"></a>Power BI

### <a name="question-does-anything-change-with-powerbi-integration"></a>Kérdés: Bármi változik a Power bi integrációja?
Igen.  Ha a munkaterületet frissítve lett majd Naplóelemzési adatok tooPower BI exportálási hello folyamat nem fog többé működni.  A frissítés előtt létrehozott meglévő ütemezések a program letiltja.  Frissítés után használja az Azure Naplóelemzés hello ugyanahhoz a platformhoz, az Application Insights, és használja ugyanezt a folyamatot tooexport Naplóelemzési lekérdezések tooPower BI mint hello [hello folyamat tooexport Application Insights lekérdezi tooPower BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).

### <a name="known-issue-power-bi-request-size-limit"></a>Ismert probléma: a Power BI kérelem méretkorlátot
Jelenleg exportált tooPower BI lehet Naplóelemzési lekérdezés, 8 MB méretkorlátot.  Ezt a határt hamarosan növekszik.


##<a name="powershell-cmdlets"></a>PowerShell-parancsmagok

### <a name="question-does-hello-log-search-powershell-cmdlet-get-updated-after-i-upgrade"></a>Kérdés: Nem hello napló keresése PowerShell-parancsmag követően módosul frissítek?
Hello [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/Get-AzureRmOperationalInsightsSearchResults) még nem frissített toohello új keresés nyelve.  Továbbra is toouse hello örökölt lekérdezési nyelv ezzel a parancsmaggal, a munkaterület frissítése után is.  Frissített dokumentáció hello parancsmag elérhető lesz, amikor frissül.


## <a name="resource-manager-templates"></a>Resource Manager-sablonok

### <a name="question-can-i-create-an-upgraded-workspace-with-a-resource-manager-template"></a>Kérdés: Hozható létre egy frissített munkaterület Resource Manager sablonnal?
Igen.  Kell egy 2017-03-15-előnézeti API verzióját használja, és tartalmazza a **szolgáltatások** a sablonban, mint például a következő hello szakasz.

    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "apiVersion": "2017-03-15-preview",
            "name": "[parameters('workspaceName')]",
            "location": "[parameters('workspaceRegion')]",
            "properties": {
                "sku": {
                    "name": "Free"
                },
                "features": {
                    "legacy": 0,
                    "searchVersion": 1
                }
            }
        }
    ],



## <a name="solutions"></a>Megoldások

### <a name="question-will-my-solutions-continue-toowork"></a>Kérdés: A megoldások továbbra is toowork?
Minden megoldás toowork folytatódik egy frissített munkaterületen, de a teljesítmény, ha az átalakított toohello új lekérdezési nyelv javítja.  Néhány meglévő megoldás ebben a szakaszban ismertetett problémákat is ismertek.

### <a name="known-issue-capacity-and-performance-solution"></a>Ismert problémák: kapacitást és teljesítményt megoldás
Néhány hello hello részt [kapacitást és teljesítményt](log-analytics-capacity.md) lehet, hogy a nézet üres.  A javítás toothis probléma hamarosan elérhető.

### <a name="known-issue-device-health-solution"></a>Ismert problémák: Eszközállapot megoldás
Hello [Eszközállapot megoldás](https://docs.microsoft.com/windows/deployment/update/device-health-monitor) nem gyűjti az adatokat egy frissített munkaterületen.  A javítás toothis probléma hamarosan elérhető.

### <a name="known-issue-application-insights-connector"></a>Ismert problémák: Application Insights-összekötő
A perspektívák [Application Insights-összekötő megoldás](log-analytics-app-insights-connector.md) egy frissített munkaterületen jelenleg nem támogatottak.  Egy javítás toothis probléma éppen elemzés alatt áll.

## <a name="upgrade-process"></a>Frissítési folyamata

### <a name="question-i-have-several-workspaces-can-i-upgrade-all-workspaces-at-hello-same-time"></a>Kérdés: több munkaterületek van. Frissíthetem: hello összes munkaterületek ugyanannyi időt vesz igénybe?  
Nem.  Frissítés alkalmazva tooa egyetlen munkaterület minden alkalommal. Jelenleg nincs frissíthető egyszerre sok munkaterületek mód. Vegye figyelembe, hogy frissíteni hello munkaterület más felhasználók is fog vonatkozni.  

### <a name="question-will-existing-log-data-collected-in-my-workspace-be-modified-if-i-upgrade"></a>Kérdés: A saját munkaterületen gyűjtött meglévő naplóadatok módosulnak Ha frissítek?  
Nem. hello napló adatok elérhető tooyour munkaterület keresések hello frissítés nem érinti. Mentett keresések, riasztások és nézetek lesznek konvertált toohello új keresés nyelvi automatikusan.  

### <a name="question-what-happens-if-i-dont-upgrade-my-workspace"></a>Kérdés: Mi történik, ha a saját munkaterület nem frissítek?  
a hónap hamarosan hello hello örökölt napló keresési elavulttá válik. Munkaterületek által adott ideje nem frissíti automatikusan frissítve lesz.

### <a name="question-i-didnt-choose-tooupgrade-but-my-workspace-has-been-upgraded-anyway-what-happened"></a>Kérdés: nem választható tooupgrade, de a saját munkaterület ennek ellenére is frissítve lett! mi történt?  
A munkaterület egy másik rendszergazda hello munkaterület sikerült frissítette. Vegye figyelembe, hogy minden munkaterületek automatikusan frissíti általánosan rendelkezésre álló új nyelvi hello elérésekor.  

### <a name="question-i-have-upgraded-by-mistake-and-now-need-toocancel-it-and-restore-everything-back-what-should-i-do"></a>Kérdés:, I tévedésből frissítette, és most kell toocancel, és minden biztonsági visszaállítási. Mit tegyek?  
Semmi akadálya.  Azt pillanatkép létrehozása a frissítés előtt a munkaterületen így is helyreállíthatja. Ne feledje, hogy keres, a riasztások vagy nézetek után hello frissítés elvesznek, ha mentette.  toorestore a munkaterület környezet, kövesse az hello eljárást [is haladhatok tovább után frissítek?](log-analytics-log-search-upgrade.md#can-i-go-back-after-i-upgrade)


## <a name="views"></a>Nézetek

### <a name="question-how-do-i-create-a-new-view-with-view-designer"></a>Kérdés: Hogyan hozható létre egy új nézetet az adatforrásnézet-tervezőből?
Előzetes tooupgrade új nézet segítségével létrehozhat adatforrásnézet-tervezőből a hello fő Irányítópulton egy csempére.  Ha a munkaterületet frissítve van, a rendszer eltávolítja a csempe.  Létrehozhat egy új nézetet az adatforrásnézet-tervezőből az OMS-portálon hello zöld hello + hello bal oldali menü gombjára kattintva.

### <a name="known-issue-see-all-option-for-line-charts-in-views-doesnt-result-in-a-line-chart"></a>Ismert problémák: lásd az összes beállítás megadása a nézetekben vonaldiagramok vonaldiagram eset sem eredményez:
Amikor rákattint az hello *láthatja az összes* beállítás alján hello nézetben sor diagram része, lehetősége lesz a tábla.  Előzetes tooupgrade, akkor jelenik meg a grafikont.  A probléma lehetséges módosításra elemezni.


## <a name="next-steps"></a>Következő lépések

- További információ [a munkaterület toohello frissítése új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md).
