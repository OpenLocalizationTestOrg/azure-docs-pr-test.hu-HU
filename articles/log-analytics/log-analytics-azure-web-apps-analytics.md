---
title: Azure Web Apps analitikus adatok aaaView |} Microsoft Docs
description: "Hello Azure Web Apps Analytics megoldás toogain insights használhatja az Azure Web Apps kapcsolatos különböző metrikák gyűjtése az Azure-webalkalmazás-erőforrások között."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 20ff337f-b1a3-4696-9b5a-d39727a94220
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: banders
ms.openlocfilehash: 7e9725f95c9faf01da89184975ad5444dd19ff95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-analytic-data-for-metrics-across-all-your-azure-web-app-resources"></a>Az Azure-webalkalmazás-erőforrások között metrikáihoz analitikai adatok megtekintése

![Web Apps szimbólum](./media/log-analytics-azure-web-apps-analytics/azure-web-apps-analytics-symbol.png)  
hello Azure Web Apps Analytics (előzetes verzió) megoldást nyújt betekintést az [Azure Web Apps](../app-service-web/app-service-web-overview.md) különböző metrikák begyűjtenie az Azure-webalkalmazás-erőforrások között. Hello megoldással elemzése, és keresse meg a webes alkalmazás erőforrás metrika értékét.

Hello megoldással, megtekintheti a:

- Hello legmagasabb válaszidő felső webalkalmazások
- A webes alkalmazások között, beleértve a sikeres és sikertelen kérelmek kérelmek száma
- A legmagasabb bejövő és kimenő forgalmat felső webalkalmazások
- A magas Processzor- és memóriafelhasználását felső szolgáltatáscsomagok
- Az Azure Web Apps tevékenység napló üzemeltetése

## <a name="connected-sources"></a>Összekapcsolt források

Legtöbb egyéb Naplóelemzési megoldásoktól eltérően adatokat nem gyűjtötte be a Azure Web Apps-ügynökök által. Hello megoldás által használt összes adatok származnak, közvetlenül az Azure-ból.

| Összekapcsolt forrás | Támogatott | Leírás |
| --- | --- | --- |
| [Windows-ügynökök](log-analytics-windows-agents.md) | Nem | hello megoldás a Windows-ügynökök nem gyűjt adatokat. |
| [Linux-ügynökök](log-analytics-linux-agents.md) | Nem | hello megoldás a Linux-ügynökök nem gyűjt adatokat. |
| [SCOM felügyeleti csoport](log-analytics-om-agents.md) | Nem | hello megoldás az ügynökök a csatlakoztatott SCOM felügyeleti csoport nem gyűjt adatokat. |
| [Azure Storage-fiók](log-analytics-azure-storage.md) | Nem | hello megoldás nem az Azure storage nem gyűjtemény adatait. |

## <a name="prerequisites"></a>Előfeltételek

- Azure Web Apps metrika erőforrásadatok tooaccess, rendelkeznie kell Azure-előfizetéssel.

## <a name="configuration"></a>Konfiguráció

Hajtsa végre a következő lépéseket tooconfigure hello Azure Web Apps elemzési megoldások a munkaterületek hello.

1. Hello Azure Web Apps elemzési megoldások a engedélyezése [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureWebAppsAnalyticsOMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).
2. [Engedélyezze a PowerShell használatával Azure erőforráscsoport metrikák naplózási tooOMS](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell).

hello Azure Web Apps elemzési megoldások metrikák két készletét gyűjti az Azure-ból:

- Az Azure Web Apps metrikák
  - Munkakészlet átlagos memória
  - Átlagos válaszidő
  - Küldött vagy fogadott bájtok
  - CPU-idő
  - Kérelmek
  - Munkakészlet memória
  - Httpxxx
- App Service-csomag metrikák
  - Küldött vagy fogadott bájtok
  - Processzorhasználat (%)
  - Lemezvárólista hossza
  - HTTP-várólista hossza
  - Memóriahasználat (%)

App Service-csomag metrikái összegyűjtése csak egy dedikált service-csomag használata. Ez nem vonatkozik a toofree vagy közös App Service-csomagokról.

Hello megoldás használatával hello OMS-portálon való hozzáadásakor láthatja hello következő csempére. Túl kell[engedélyezése a PowerShell használatával Azure erőforráscsoport metrikák naplózási tooOMS](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell).

![Értékelés értesítési végrehajtása](./media/log-analytics-azure-web-apps-analytics/performing-assessment.png)

Miután konfigurálta a hello megoldás, adatokat kell kezdődnie, áramló tooyour munkaterület 15 percen belül.

## <a name="using-hello-solution"></a>Hello megoldással

Hello Azure Web Apps Analytics megoldás tooyour munkaterület felvételekor hello **Azure Web Apps Analytics** csempe kerül tooyour áttekintő irányítópulthoz. Ez a csempe az Azure Web Apps, hogy hello megoldás rendelkezik hozzáférési tooin az Azure-előfizetéshez hello száma számát jeleníti meg.

![Az Azure Web Apps Analytics csempe](./media/log-analytics-azure-web-apps-analytics/azure-web-apps-analytics-tile.png)

### <a name="view-azure-web-apps-analytics-information"></a>Azure Web Apps Analytics adatainak megtekintése

Kattintson a hello **Azure Web Apps Analytics** csempe tooopen hello **Azure Web Apps Analytics** irányítópult. hello irányítópult hello paneleken hello a következő táblázat tartalmazza. Minden egyes panel be, hogy a panel a feltételek hello megadva hatókör és a kívánt időtartományt megfelelő tooten elemeket sorolja fel. A napló keresési, amely visszaadja az összes rekord kattintva futtathatja **láthatja az összes** hello panel vagy hello panel fejléc kattintva hello alján.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

| Oszlop | Leírás |
| --- | --- |
| Az Azure-webalkalmazás |   |
| Web Apps kérelem trendek | Webalkalmazások kérelem trendje kijelölt dátumtartomány hello hello sor látható, és egy hello felső tíz webes kérelmek listáját jeleníti meg. Kattintson a hello sor diagram toorun napló keresése<code>Type=AzureMetrics ResourceId=*"/MICROSOFT.WEB/SITES/"* (MetricName=Requests OR MetricName=Http*) &#124; measure avg(Average) by MetricName interval 1HOUR</code> <br>Kattintson egy webes kérelem elem toorun hello webes kérelem metrika trend kérő napló keresése. |
| Web Apps válaszidő | Hello webalkalmazások válaszidő hello dátumtartományon belül a kiválasztott vonaldiagram jeleníti meg. Is hello listája listájának megjelenítése felső tíz webes alkalmazások response alkalommal fordult elő. Kattintson a diagram toorun hello napló keresése<code>Type:AzureMetrics ResourceId=*"/MICROSOFT.WEB/SITES/"* MetricName="AverageResponseTime" &#124; measure avg(Average) by Resource interval 1HOUR</code><br> Kattintson az egy webalkalmazás toorun hello webalkalmazás válaszidejét adatszolgáltató napló keresést. |
| Webes alkalmazások forgalom | A Web Apps-forgalmat, vonaldiagram mutatja MB-ban, és hello felső webalkalmazások forgalom sorolja fel. Kattintson a diagram toorun hello napló keresése<code>Type:AzureMetrics ResourceId=*"/MICROSOFT.WEB/SITES/"*  MetricName=BytesSent OR BytesReceived &#124; measure sum(Average) by Resource interval 1HOUR</code><br> Webalkalmazások összes forgalom az elmúlt percben hello jeleníti meg. Kattintson a webalkalmazás toorun bájt megjelenítő napló keresés fogadott és küldött hello Web App. |
| Az Azure App Service-csomagok |   |
| App Service-csomagok a CPU-felhasználás &gt; 80 %-át | Hello összesített számát mutatja App Service-csomag, amely nagyobb, mint a 80 %-os és listák hello első 10 App Service-csomagok CPU-kihasználtság szerinti CPU-kihasználtsága. Kattintson a teljes terület toorun hello napló keresése<code>Type=AzureMetrics ResourceId=*"/MICROSOFT.WEB/SERVERFARMS/"* MetricName=CpuPercentage &#124; measure Avg(Average) by Resource</code><br> Ez az App Service-csomagok és az átlagos processzorhasználat listáját tartalmazza. Kattintson egy App Service-csomag toorun az átlagos processzorhasználat megjelenítő napló keresést. |
| App Service-csomagok memóriahasználata a &gt; 80 %-át | Hello összesített számát mutatja App Service-csomagok, amelyek memória kihasználtsága meghaladja a 80 %-os és listák hello első 10 App Service-csomagok memóriahasználata által. Kattintson a teljes terület toorun hello napló keresése<code>Type=AzureMetrics ResourceId=*"/MICROSOFT.WEB/SERVERFARMS/"* MetricName=MemoryPercentage &#124; measure Avg(Average) by Resource</code><br> Ez az App Service-csomagok és az átlagos memóriafelhasználás a listáját jeleníti meg. Kattintson egy App Service-csomag toorun megjelenítő, az átlagos memóriafelhasználás a napló keresést. |
| Az Azure Web Apps tevékenységi naplóit |   |
| Az Azure Web Apps tevékenység naplózása | Azt mutatja be hello webalkalmazások száma [tevékenységi naplóit](log-analytics-activity.md) és listák hello első 10 tevékenység napló műveletek. Kattintson a teljes terület toorun hello napló keresése<code>Type=AzureActivity ResourceProvider= "Azure Web Sites" &#124; measure count() by OperationName</code><br> Az tevékenység napló műveleteket mutat hello listáját. Egy tevékenység napló művelet toorun hello rekordok hello a művelethez felsoroló napló Keresés gombra. |



### <a name="azure-web-apps"></a>Azure Web Apps

Hello irányítópulton részletezve tooget a Web Apps metrikákat további betekintést. A paneleken első készlete hello trend hello webalkalmazások kérelmek száma a hibák (például HTTP404), a forgalom és az átlagos válaszidő időbeli megjelenítése. Különböző Web Apps e metrikák részletes információkat is látható.

![Az Azure webalkalmazás panelen](./media/log-analytics-azure-web-apps-analytics/web-apps-dash01.png)

Egy elsődleges adatok jelennek meg, hogy adott oka, hogy a webes alkalmazás magas válaszidő segítségével azonosíthatók, és vizsgálja ki toofind hello alapvető okát. Egy küszöbértéket egyben alkalmazott toohelp hello több könnyen azonosíthatja azokat a problémákat.

- Webalkalmazások vörösen láthatók rendelkezik válaszidő magasabb, mint 1 másodperc.
- Web Apps narancssárga látható válaszidejének 0,7 második és kisebb, mint 1 másodperc nagyobbnak.
- Webalkalmazások zölden láthatók második rendelkezik 0,7-nál kisebb a válaszidőt.

Hello napló keresése Példa kép a következő, láthatja, hogy hello *anugup3* webalkalmazás kellett egy sokkal nagyobb válaszideje, mint más webes alkalmazásokat hello.

![naplófájl-keresési példa](./media/log-analytics-azure-web-apps-analytics/web-app-search-example.png)

### <a name="app-service-plans"></a>App Service-csomagok

Dedikált Service-csomagok használ, ha az App Service-csomagok esetében is begyűjtheti a metrikák. Ebben a nézetben látható az App Service-csomagok és magas CPU és memória kihasználtsága (&gt; 80 %-ot). Bemutatja azt is, a magas memóriát és CPU-kihasználtság felső alkalmazásszolgáltatások hello. Egy küszöbértéket hasonlóan alkalmazott toohelp hello több könnyen azonosíthatja azokat a problémákat is.

- App Service-csomagok vörösen láthatók a Processzor/memória kihasználtsága 80 %-nál nagyobb lehet.
- App Service-csomagokról narancssárga látható memóriakihasználtsága a CPU/magasabb, mint 60 % és 80 %-nál kisebb.
- App Service-csomagok zölden láthatók memóriakihasználtsága a CPU/alacsonyabb, mint 60 %-át.

![Az Azure App Service-csomagok paneleken](./media/log-analytics-azure-web-apps-analytics/web-apps-dash02.png)

## <a name="azure-web-apps-log-searches"></a>Az Azure Web Apps napló keresések

Hello **lista a népszerű Azure Web Apps keresési lekérdezések** elsajátíthatja, hogy minden hello kapcsolatos tevékenység naplókat Web Apps, amely a webalkalmazások erőforrásokon végrehajtott műveletek hello betekintést biztosít. Azt is tartalmazza az összes hello kapcsolatos műveletek és hello ennyiszer azok történt.

Bármely hello napló keresési lekérdezések használatával kiindulási pontként, egyszerűen létrehozhat egy riasztást. Érdemes például toocreate riasztást, ha egy metrika az átlagos válaszidő érték nagyobb, mint 1 másodpercenként.

## <a name="next-steps"></a>Következő lépések

- Hozzon létre egy [riasztás](log-analytics-alerts-creating.md) az adott metrika.
- Használjon [naplófájl-keresési](log-analytics-log-searches.md) tooview részletes adatait a tevékenységi naplóit.
