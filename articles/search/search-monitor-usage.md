---
title: "aaaMonitor használatát és az Azure Search szolgáltatást statisztikákat |} Microsoft Docs"
description: "Erőforrás-felhasználás és index mérete nyomon az Azure Search, egy üzemeltetett felhőalapú keresőszolgáltatás, a Microsoft Azure."
services: search
documentationcenter: 
author: bernitorres
manager: jlembicz
editor: 
tags: azure-portal
ms.assetid: 122948de-d29a-426e-88b4-58cbcee4bc23
ms.service: search
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 05/01/2017
ms.author: betorres
ms.openlocfilehash: f38eabb5d04a410e11eaaff22157da8aba9e4845
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-an-azure-search-service"></a>Egy Azure Search szolgáltatás figyelése

Az Azure Search használati és teljesítményadatokat keresési szolgáltatások nyomon követése különböző erőforrások kínál. Ez lehetővé teszi az toometrics, a naplókat, a index statisztikák és a kiterjesztett figyelési képességekkel a Power BI eléréséhez. Ez a cikk ismerteti, hogyan tooenable hello különböző figyelési stratégiák, és hogyan toointerpret hello eredményül kapott adatokat.

## <a name="azure-search-metrics"></a>Az Azure Search metrikák
Metrikák közel valós idejű információkat a keresőszolgáltatása biztosítanak, és elérhető összes szolgáltatáshoz, további beállítás nélkül. Ezek segítségével nyomon követhető a szolgáltatás a too30 nap hello teljesítményét.

Az Azure Search számára gyűjti az adatokat három különböző metrikák:

* Keresési késés: idő hello keresési szolgáltatás szükséges tooprocess keresőkifejezések, egy perc alatt összesített értéket.
* / Másodperc (QPS) keresőkifejezések: keresési száma percenként összesítve másodpercenként fogadott lekérdezések.
* Szabályozottan halmozott keresési lekérdezések százalékos aránya: percenként összesítve volt szabályozva, keresési lekérdezések aránya.

![Képernyőfelvétel a QPS tevékenység][1]

### <a name="set-up-alerts"></a>Riasztások beállítása
Hello metrika Részletek lapján konfigurálhatja riasztások tootrigger e-mailben értesítést vagy automatikus műveletek metrika ebbe a megadott küszöbértéket.

További információ a metrikák dokumentációjában hello teljes Azure a figyelőhöz.  

## <a name="how-tootrack-resource-usage"></a>Hogyan tootrack Erőforrás kihasználtsága
Hello növekedése indexek és dokumentum mérete követési segítségével proaktív módon állítsa be a kapacitás szerezze meg a hello felső korlátja a szolgáltatás létrehozása előtt. Ehhez a hello portálon vagy programozottan a hello REST API használatával.

### <a name="using-hello-portal"></a>Hello portál használatával

toomonitor Erőforrás kihasználtsága, tekintse meg hello számát, valamint a szolgáltatás statisztikája hello [portal](https://portal.azure.com).

1. Jelentkezzen be toohello [portal](https://portal.azure.com).
2. Nyissa meg az Azure Search szolgáltatás szolgáltatási irányítópultját hello. Csempék hello szolgáltatás hello kezdőlapján található, vagy tallózással szolgáltatás toohello hello Ugrósávon tallózással.

hello használata című szakaszában tartalmaz egy mérési jelzi, hogy milyen részére rendelkezésre álló erőforrások jelenleg használatban van. Szolgáltatás korlátozások az indexet, dokumentumokat és a tárolási információkért lásd: [szolgáltatási korlátait](search-limits-quotas-capacity.md).

  ![Használata csempe][2]

> [!NOTE]
> hello fenti képernyőfelvételen hello szabad szolgáltatást, amely legfeljebb egy másodpéldány és particionálása, minden egyes, és csak 3 állomás indexek, 10 000 dokumentumokat vagy adatokat 50 MB lehet az, amelyik előbb következik be. Létrehozás dátuma: Basic vagy Standard szint szolgáltatások sokkal nagyobb szolgáltatásra vonatkozó korlátozások rendelkeznek. A réteg kiválasztásáról további információkért lásd: [válasszon egy réteghez vagy SKU](search-sku-tier.md).
>
>

### <a name="using-hello-rest-api"></a>Hello REST API használatával
Hello Azure Search REST API és a .NET SDK hello adja meg a programozott hozzáférést tooservice metrikákat.  Ha használ [indexelők](https://msdn.microsoft.com/library/azure/dn946891.aspx) tooload egy Azure SQL Database vagy az Azure Cosmos DB index, egy további API elérhető tooget hello számok van szüksége.

* [Megtekintheti a statisztikákat Index](/rest/api/searchservice/get-index-statistics)
* [A dokumentumok száma](/rest/api/searchservice/count-documents)
* [Az indexelő állapotának beolvasása](/rest/api/searchservice/get-indexer-status)

## <a name="how-tooexport-logs-and-metrics"></a>Hogyan tooexport naplózza, és a metrikák

A szolgáltatás és hello nyers adatok hello metrikáihoz hello előző szakaszban leírt hello műveletnaplók exportálhatja. A műveletnaplók lehetővé teszik, hogy tudja, hogyan hello szolgáltatást használják, és képes használni a Power BI szolgáltatásból, amikor adatok másolása tooa tárfiók. Az Azure search biztosítja a figyelési Power BI-tartalomcsomag erre a célra.


### <a name="enabling-monitoring"></a>Figyelés engedélyezése
Nyissa meg az Azure Search szolgáltatás hello [Azure-portálon](http://portal.azure.com) hello figyelés engedélyezése beállítás alapján.

Hello adatok kiválasztásához tooexport kívánt: naplók, metrikák vagy mindkettőt. Tooa tárfiók másolja, elküldi a tooan eseményközpont vagy tooLog Analytics exportálni onnan.

![Hogyan tooenable hello portálon figyelése][3]

a PowerShell vagy Azure CLI hello tooenable hello dokumentációjában [Itt](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

### <a name="logs-and-metrics-schemas"></a>Naplók és a metrikák sémák
Hello adatok esetén másolt tooa tárolási fiókot, hello adatok formátuma két tárolókban lévő JSON és annak helye:

* insights-logs-operationlogs: a keresési forgalmi naplók
* insights-metrikák-pt1m: metrikáihoz

Egy blob tároló száma óránként van.

Példa elérési útja:`resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2015/m=12/d=25/h=01/m=00/name=PT1H.json`

#### <a name="log-schema"></a>Séma
hello naplókat blobok a keresési szolgáltatás forgalmi naplók tartalmazzák.
Minden egyes blob tartozik egy legfelső szintű objektum nevű **rekordok** , amely tartalmazza a napló objektumokból álló tömb.
Minden egyes blob rekordok rendelkezzen minden hello művelet hello során bekövetkezett azonos órához.

| Név | Típus | Példa | Megjegyzések |
| --- | --- | --- | --- |
| time |Dátum és idő |"2015-12-07T00:00:43.6872559Z" |Hello művelet időbélyegzője |
| resourceId |Karakterlánc |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>RESOURCEGROUPS/ALAPÉRTELMEZETT/SZOLGÁLTATÓK /<br/> MICROSOFT. KERESÉSI/SEARCHSERVICES/SEARCHSERVICE" |Az erőforrás-azonosítója |
| operationName |Karakterlánc |"Query.Search" |hello hello művelet neve |
| operationVersion |Karakterlánc |"2015-02-28" |hello api-version használt |
| category |Karakterlánc |"OperationLogs" |állandó |
| resultType |Karakterlánc |"Sikeres" |A lehetséges értékek: sikeres vagy sikertelen volt |
| resultSignature |int |200 |HTTP eredménykódja |
| durationMS |int |50 |Ezredmásodpercben hello művelet időtartama |
| properties |Objektum |Lásd a következő táblázat hello: |A művelet vonatkozó adatokat tartalmazó objektum |

**Tulajdonságok séma**
| Név | Típus | Példa | Megjegyzések |
| --- | --- | --- | --- |
| Leírás |Karakterlánc |"/Indexes('content')/docs beolvasása" |hello művelet végpont |
| Lekérdezés |Karakterlánc |"? keresési = AzureSearch & $count = true & api-version = 2015-02-28" |hello lekérdezési paraméterekről |
| Dokumentumok |int |42 |Feldolgozott dokumentumok száma |
| indexName |Karakterlánc |"testindex" |Hello művelethez társított hello index nevét |

#### <a name="metrics-schema"></a>Metrikák séma
| Név | Típus | Példa | Megjegyzések |
| --- | --- | --- | --- |
| resourceId |Karakterlánc |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>RESOURCEGROUPS/ALAPÉRTELMEZETT/SZOLGÁLTATÓK /<br/>MICROSOFT. KERESÉSI/SEARCHSERVICES/SEARCHSERVICE" |az erőforrás-azonosító |
| metricName |Karakterlánc |"Várakozási" |hello hello metrika neve |
| time |Dátum és idő |"2015-12-07T00:00:43.6872559Z" |hello művelet időbélyeg |
| Átlagos |int |64 |átlagos érték hello hello nyers minták hello metrika időintervallumban |
| minimális |int |37 |minimális érték hello hello nyers minták hello metrika időintervallumban |
| Maximális |int |78 |hello nyers minták hello metrika időközben hello maximális értéke |
| összesen |int |258 |hello összértéke hello nyers minták hello metrika időintervallumban |
| Száma |int |4 |nyers minták száma hello használt toogenerate hello metrika |
| időkeretben vannak |Karakterlánc |"PT1M" |az ISO 8601 hello mérőszám hello idő felbontása |

Minden metrikák egy perces időközönként jelenti. Minden mérőszám minimális, maximális és átlagos száma percenként értékeket.

Hello SearchQueriesPerSecond mérőszám minimális érték hello legalacsonyabb, hogy percben regisztrált másodpercenként keresési lekérdezések. hello Ugyanez vonatkozik toohello maximális értéket. Átlagos, az összesítő hello hello teljes perc között.
Gondoljon egy perc alatt erről a forgatókönyvről: hello maximális SearchQueriesPerSecond, a magas terhelést egy másodperces követ 58 másodperc átlagos terhelés, és végül egy második csak egy lekérdezéssel, amely minimális hello.

A ThrottledSearchQueriesPercentage, minimum, maximum, átlagos és összesen, mindegyik rendelkezik hello ugyanarra az értékre: hello volt szabályozva, a keresési lekérdezések egy perc alatt hello száma keresési lekérdezések aránya.

## <a name="analyzing-your-data-with-power-bi"></a>Az adatokat a Power BI elemzése

Azt javasoljuk, [Power BI](https://powerbi.microsoft.com) tooexplore és az adatok megjelenítése. Egyszerűen csatlakoztassa tooyour Azure Storage-fiókot, és gyorsan indítsa el az adatok elemzése.

Az Azure Search biztosít egy [Power BI-tartalomcsomag](https://app.powerbi.com/getdata/services/azure-search) , amely lehetővé teszi toomonitor, és a keresési forgalom előre definiált diagramok és táblák ismertetése. A Power BI-jelentések automatikusan csatlakozó tooyour adatokat, és adja meg a keresési szolgáltatás visual észrevételeket tartalmaz. További információkért lásd: hello [tartalomcsomag súgólap](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-search/).

![Az Azure Search Power BI-irányítópulton][4]

## <a name="next-steps"></a>Következő lépések
Felülvizsgálati [méretezni a replikák és a partíciók](search-limits-quotas-capacity.md) hogyan toobalance hello partíciókat és a meglévő szolgáltatás-replikáit az útmutatót.

Látogasson el [kezelése a Microsoft Azure – keresés szolgáltatást](search-manage.md) további információt a felügyeleti szolgáltatás, vagy [teljesítmény- és optimalizálási](search-performance-optimization.md) a finomhangoláshoz útmutatást.

További tudnivalók elképesztő jelentések létrehozásához. Lásd: [első lépések a Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/) részletek

<!--Image references-->
[1]: ./media/search-monitor-usage/AzSearch-Monitor-BarChart.PNG
[2]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG
[3]: ./media/search-monitor-usage/AzureSearch-Enable-Monitoring.PNG
[4]: ./media/search-monitor-usage/AzureSearch-PowerBI-Dashboard.png
