---
title: "aaaConnecting Azure SQL Database tooAzure keresési használatával indexelők |} Microsoft Docs"
description: "Ismerje meg, hogyan toopull adatait az Azure SQL Database tooan Azure Search index indexelők használatával."
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: e9bbf352-dfff-4872-9b17-b1351aae519f
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/13/2017
ms.author: eugenesh
ms.openlocfilehash: b28a11cf18ef994de99e09af90bbfeb171ef3cde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-azure-sql-database-tooazure-search-using-indexers"></a>Kapcsolódás Azure SQL Database tooAzure keresés indexelők használatával

Mielőtt kérdezheti le egy [Azure Search-index](search-what-is-an-index.md), fel kell tölteni azt az adatait. Ha hello adatok él, az Azure SQL-adatbázis egy **Azure Search-indexelőt, az Azure SQL Database** (vagy **Azure SQL indexelő** röviden) automatizálhatja hello indexelési folyamata, ami azt jelenti, hogy a kód toowrite kisebb és kisebb infrastruktúra toocare kapcsolatban.

Ez a cikk foglalkozik hello beállítás használata esetén [indexelők](search-indexer-overview.md), de csak Azure SQL-adatbázisok (például az integrált változáskövetés) elérhető szolgáltatások is ismerteti. 

Továbbá tooAzure SQL-adatbázisok Azure Search biztosít az indexelők [Azure Cosmos DB](search-howto-index-documentdb.md), [Azure Blob Storage tárolóban](search-howto-indexing-azure-blob-storage.md), és [Azure table storage](search-howto-indexing-azure-tables.md). más adatforrások toorequest támogatása a visszajelzést adhat hello [Azure Search-visszajelzési fórumon](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="indexers-and-data-sources"></a>Indexelők és adatforrások

A **adatforrás** mely adatok tooindex, a hitelesítő adatait, adatelérési és a házirendeket, amelyek hatékonyan hello adatok (új, módosított vagy törölt sorai) változásai. Az definiálva van egy független erőforrásként, hogy több indexelők használható.

Egy **indexelő** , amely csatlakozik egy adatforráshoz a célzott keresési indexszel rendelkező erőforrás. Az indexelő használja a következő módokon hello:

* Hello adatok toopopulate index egyszeri árnyékmásolata.
* Az ütemezés szerint hello adatforrás módosításokkal index frissítése.
* Futtassa az igény szerinti tooupdate index igény szerint.

Egyetlen indexelő csak felhasználhat egy táblát vagy nézetet, de több indexelők hozhat létre, ha azt szeretné, toopopulate több keresési indexet. A fogalmakat további információkért lásd: [az indexelési műveletek: tipikus munkafolyamat](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations#typical-workflow).

Állítsa be, és konfigurálhatja az Azure SQL indexelő segítségével:

* Adatok importálása varázslót a hello [Azure-portálon](https://portal.azure.com)
* Az Azure Search [.NET SDK-val](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)
* Az Azure Search [REST API-n](https://docs.microsoft.com/en-us/rest/api/searchservice/indexer-operations)

Ebben a cikkben hello REST API toocreate fogjuk használni **indexelők** és **adatforrások**.

## <a name="when-toouse-azure-sql-indexer"></a>Amikor toouse Azure SQL indexelő
Attól függően, hogy számos tényező tooyour adatok vonatkozó hello használata az Azure SQL indexelő feltétlenül nem megfelelő. Az adatok hello-követelményeknek megfelelő, ha az Azure SQL indexelő is használhatja.

| Feltételek | Részletek |
|----------|---------|
| Egyetlen tábla vagy nézet származik adatok | Ha hello adatok több tábla van tárolja, létrehozhat hello adatokat egyetlen nézetben. Ha egy nézetben képes toouse integrált SQL Server módosítása észlelési toorefresh egy index ugyanezzel a növekményes változásokat nem lehet. További információkért lásd: [rögzítése módosított és törölt sorok](#CaptureChangedRows) alatt. |
| Adattípusok kompatibilisek. | Az Azure Search-index nem minden hello SQL típusok támogatottak. Az útmutató, [adattípusok leképezési](#TypeMapping). |
| Nincs szükség a valós idejű adatok szinkronizálása | Az indexelő indexelheti újra a tábla legfeljebb 5 perc. Ha az adatok változásait gyakran, és hello változik, megjelennek a hello index vagy egyetlen perceken belül kell toobe, azt javasoljuk, hello [REST API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) vagy [.NET SDK](search-import-data-dotnet.md) toopush frissített sorok közvetlenül. |
| Növekményes indexelő lehetőség | Ha nagy mennyiségű adathalmaz és terv toorun hello indexelő ütemezés szerint, Azure Search képesnek kell lennie a tooefficiently új, módosított vagy törölt sor azonosítása. Nem növekményes indexelő csak engedélyezett, ha az igény szerinti (nem a ütemezés) indexelő, vagy kevesebb, mint 100 000 sor indexelése. További információkért lásd: [rögzítése módosított és törölt sorok](#CaptureChangedRows) alatt. |

## <a name="create-an-azure-sql-indexer"></a>Hozzon létre egy Azure SQL indexelőt

1. Hello adatforrás létrehozása:

   ```
    POST https://myservice.search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of hello table or view that you want tooindex" }
    }
   ```

   Hello kapcsolati karakterlánc beolvasása hello [Azure-portálon](https://portal.azure.com); hello használata `ADO.NET connection string` lehetőséget.

2. Hello cél Azure Search-index létrehozása, ha Ön nem rendelkezik ilyennel. Hello használatával hozhat létre [portal](https://portal.azure.com) vagy hello [Index API létrehozása](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Győződjön meg arról, hogy hello hello forrástábla hello sémája kompatibilis a cél index séma – Lásd: [SQL és az Azure search közötti megfeleltetés adattípusok](#TypeMapping).

3. Hozzon létre hello indexelő adjon neki egy nevet, és hivatkozzon hello adatok forrása és célja index:

    ```
    POST https://myservice.search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }
    ```

Ily módon létrehozta az indexelő nem tartozhat olyan ütemezés. Ha már létrehozott automatikusan futtatja. Futtatható újra az összes idő a **indexelő futtatása** kérelem:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2016-09-01
    api-key: admin-key

Testre szabhatja a indexelő viselkedését, például a kötegméretet és hány dokumentumokat is kimarad, mielőtt egy indexelő végrehajtása meghiúsul számos tulajdonságát. További információkért lásd: [indexelő API létrehozása](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

Szükség lehet tooallow Azure-szolgáltatások tooconnect tooyour adatbázis. Lásd: [csatlakozás az Azure](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) útmutatást toodo, amely.

toomonitor hello indexelő állapotának és futtatási előzményei (indexelt elemek száma, hiba stb.), használjon egy **indexelő állapot** kérelem:

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2016-09-01
    api-key: admin-key

hello válasz alábbihoz hasonló toohello következő:

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null
            },
            ... earlier history items
        ]
    }

Futtatási előzményei too50 legutóbb befejeződött hello végrehajtások, amely (így hello legújabb végrehajtási előre első hello válaszul) hello fordított időrendi sorrendben rendezi a mentést tartalmaz.
További információt a hello válasz megtalálhatók [indexelő állapotának beolvasása](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Indexelő fussanak ütemezés szerint
Hello indexelő toorun rendszeres időközönként a ütemezés is rendezheti. toodo, vegye fel a hello **ütemezés** létrehozásakor vagy frissítésekor hello indexelő tulajdonság. hello az alábbi példában a PUT kérés tooupdate hello indexelő:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2016-09-01
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

Hello **időköz** paraméter megadása kötelező. hello időköz toohello idő hello kezdetét két egymást követő indexelő végrehajtások közötti hivatkozik. hello legkisebb megengedett időköz 5 perc; leghosszabb hello egy nap. Az XSD "daytimeduration típusú" értéknek kell formázni (korlátozott részhalmaza egy [ISO 8601 időtartama](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) érték). hello minta: `P(nD)(T(nH)(nM))`. Példák: `PT15M` a 15 percenként `PT2H` minden 2 órán át.

nem kötelező hello **startTime** azt jelzi, ha hello ütemezett végrehajtások kell megkezdéséhez. Ha ki van hagyva, hello jelenlegi UTC időt használja. Most lehet múltbeli – hello első végrehajtási ebben az esetben van ütemezve, mintha hello az indexelő futása során folyamatosan hello kezdő időpont óta hello.  

Az indexelő csak egy végrehajtásának futtatható egyszerre. Ha az indexelő futása közben a végrehajtása van ütemezve, hello végrehajtási hello következő ütemezett időpontban kerül sor.

Mérlegeljük, egy példa toomake ez konkrétabb. Tegyük fel, azt követően beállított óránkénti ütemezés hello:

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Ez történik:

1. hello első indexelő végrehajtásának indítása, vagy az akörüli 2015. március 1 12:00-kor UTC.
2. Tegyük fel, a végrehajtási időt vesz igénybe, 20 perc (vagy bármikor kisebb, mint 1 óra).
3. hello második végrehajtása kezdődik, vagy az akörüli 2015. március 1 1:00-kor
4. Most tegyük fel, hogy a végrehajtása több mint egy óra – például 70 perc – időt vesz igénybe, így ő maga befejeződne körülbelül 2:10 óra
5. Most hello harmadik végrehajtási toostart, 2:00 óra, amikor is. Azonban mivel a második végrehajtási 1 órától hello továbbra is fut, a rendszer kihagyja a hello harmadik végrehajtása. hello harmadik indítása 3 órakor

Hozzáadása, módosítása, vagy törölje a meglévő indexelőt ütemezésének használatával egy **PUT indexelő** kérelmet.

<a name="CaptureChangedRows"></a>

## <a name="capture-new-changed-and-deleted-rows"></a>Új, módosított és törölt sorok rögzítése

Használja az Azure Search **növekményes indexelő** tooavoid rendelkező toore-index teljes táblázat hello, vagy minden alkalommal, amikor az indexelő futása megtekintése. Az Azure Search biztosít két módosítása észlelési házirendek toosupport növekményes indexelő. 

### <a name="sql-integrated-change-tracking-policy"></a>Az SQL integrált változáskövetési házirend
Ha az SQL-adatbázis támogatja [a változáskövetés](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server), azt javasoljuk, **SQL integrált módosítása követési házirend**. Ez egy hello leghatékonyabb házirend. Emellett lehetővé teszi a Azure Search tooidentify törölt sorok anélkül, hogy tooadd egy explicit "helyreállítható törlésre" oszlop tooyour táblában.

#### <a name="requirements"></a>Követelmények 

+ Adatbázis-verzióra vonatkozó követelmények:
  * SQL Server 2012 SP3 és újabb verziók, használata az SQL Server Azure virtuális gépeken.
  * Az Azure SQL Database 12-es, az Azure SQL Database használata.
+ Táblák csak (Nézet). 
+ Hello adatbázis [engedélyezése változáskövetési](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-tracking-sql-server) hello tábla. 
+ Nem összetett táblázat elsődleges kulcsa (egy elsődleges kulcs tartalmazó több mint egy oszlopban) hello.  

#### <a name="usage"></a>Használat

toouse Ez a házirend létrehozása, vagy frissíti az adatforrást ehhez hasonló:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy"
      }
    }

Ha használja az SQL integrált változáskövetés házirend, ne adjon meg egy külön adatok törlési szabályzat – a ezzel a házirend-azonosító beépített támogatása törölt sorok. Azonban a hello törlések észlelt toobe "automagically", a search-index kulcsában hello dokumentum kell kell hello ugyanaz, mint a hello elsődleges kulcs a következő SQL-tábla hello. 

<a name="HighWaterMarkPolicy"></a>

### <a name="high-water-mark-change-detection-policy"></a>Magas vízjel alapján észlelésének házirend

A módosítás szabályzat rögzítése hello verzió vagy idő, amikor egy sor utolsó frissítés "magas vízjelének" oszlop támaszkodik. Egy nézet használata, a magas vízjel alapján házirendet kell használnia. hello magas vízjel alapján oszlop hello követelményeknek kell megfelelnie.

#### <a name="requirements"></a>Követelmények 

* Minden Beszúrások hello oszlop értékét adja meg.
* Minden frissítések tooan elem hello oszlop értéke hello is módosíthatja.
* hello érték az adott oszlop növekszik minden insert vagy update mutatóval.
* A lekérdezések hello HELYÉT a következő és ORDER BY záradékok hatékonyan hajtható végre:`WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`

> [!IMPORTANT] 
> Határozottan javasoljuk hello [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) hello magas vízjel alapján oszlop adattípusa esetében. Ha bármilyen más típusú adatokat használ, változáskövetési nem garantált toocapture összes hello jelenléte tranzakciók végrehajtása az indexelő lekérdezések egyidejűleg változik. Használata esetén **rowversion** írásvédett replikával konfigurációban hello elsődleges másodpéldány, hello indexelő kell mutatnia. Csak egy elsődleges másodpéldány adatok szinkronizálása forgatókönyvek esetén használható.

#### <a name="usage"></a>Használat

a magas vízjel alapján házirend toouse létrehozása, vagy frissíti az adatforrást ehhez hasonló:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a rowversion or last_updated column name]"
      }
    }

> [!WARNING]
> Hello forrástábla nem rendelkezik az index hello magas vízjel alapján oszlop alapján, ha a lekérdezések hello SQL indexelő által használt is túllépi az időkorlátot. Különösen hello `ORDER BY [High Water Mark Column]` záradékhoz szükség egy index toorun hatékonyan van, ha hello tábla hány sort tartalmaz.
>
>

Ha időtúllépési hibákat tapasztal, használhatja a hello `queryTimeout` indexelő konfigurációs beállítás tooset hello lekérdezés időkorlátja tooa értéke nagyobb, mint hello alapértelmezett 5 perces időkorlát. Például tooset hello időtúllépés too10 perc, hozzon létre vagy hello indexelő frissítése a következő konfigurációs hello:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

Le is tilthatja hello `ORDER BY [High Water Mark Column]` záradékban. Azonban ez nem ajánlott mert hello indexelő végrehajtása megszakad a hiba, ha hello indexelő toore-folyamat összes sorát, ha később - fog futni akkor is, ha hello indexelő feldolgozása már majdnem minden hello sor hello időpontjára megszakítva. toodisable hello `ORDER BY` záradék, használjon hello `disableOrderByHighWaterMarkColumn` hello indexelő definícióban beállítása:  

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Véglegesen törli oszlop törlési szabályzat
Amikor sorok hello forrástábla törlődnek, érdemes toodelete hello keresési index, valamint ezek sorát. Ha hello SQL integrált változáskövetési házirend, ez van végrehajtott megvagyunk meg. Azonban hello magas vízjel alapján változáskövetési házirend nem oldották meg a törölt soraihoz. Milyen toodo?

Hello sorok fizikailag el lesznek távolítva hello tábla, ha az Azure Search rendelkezik nincs módja tooinfer hello meglétének azt jelzi, hogy a továbbiakban nem létezik.  Azonban hello "soft-törlés" technika toologically törölje a sorokat is használhatja őket hello táblából eltávolítása nélkül. Egy oszlop tooyour tábla vagy nézet hozzáadása és sorok megjelölése törölni az oszlopnak.

Hello soft-törlés módszer használata esetén is megadhat hello helyreállítható törlésre vonatkozó házirendet az alábbiak szerint létrehozásakor vagy frissítésekor hello adatforrás:

    {
        …,
        "dataDeletionDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]",
           "softDeleteMarkerValue" : "[hello value that indicates that a row is deleted]"
        }
    }

Hello **softDeleteMarkerValue** kell egy karakterlánc – használja a tényleges érték hello karakterláncos ábrázolása. Például, ha még szerepel egészszám-oszloppal, ahol törölt soraihoz hello érték 1 jelöli, használja `"1"`. Ha rendelkezik egy BIT oszlop, ahol törölt soraihoz hello logikai true értéket jelöli, `"True"`.

<a name="TypeMapping"></a>

## <a name="mapping-between-sql-and-azure-search-data-types"></a>Az SQL és az Azure Search adattípusok közötti megfeleltetés.
| SQL-adattípus | Cél index mezőtípusok engedélyezett | Megjegyzések |
| --- | --- | --- |
| bit |Edm.Boolean, Edm.String | |
| int, smallint, tinyint |Edm.Int32, Edm.Int64, Edm.String | |
| bigint |Edm.Int64, Edm.String | |
| valódi, lebegőpontos |Edm.Double, Edm.String | |
| kis pénz típusú értékké, pénzt decimális numerikus |Edm.String |Az Azure Search nem támogatja a decimális típusok konvertálásakor Edm.Double, mert ez elveszítik pontosság |
| CHAR, nchar, varchar, nvarchar |Edm.String<br/>Collection(Edm.String) |Egy SQL-karakterlánc használt toopopulate Collection(Edm.String) mező lehet, ha a hello karakterlánc egy JSON-tömb karakterláncok jelöli:`["red", "white", "blue"]` |
| smalldatetime, dátum és idő, datetime2, dátum, datetimeoffset |Edm.DateTimeOffset, Edm.String | |
| uniqueidentifer |Edm.String | |
| földrajzi hely |Edm.GeographyPoint |Csak az srid-Azonosítónak 4326 (amely hello alapértelmezett) pontra típusú geográfiai példányban támogatottak. |
| ROWVERSION |N/A |Sor verziójú oszlopok hello search-index nem lehet tárolni, de a változások követése is használhatók |
| idő, timespan, binary, varbinary, kép, xml, geometriai, CLR-típus |N/A |Nem támogatott |

## <a name="configuration-settings"></a>Konfigurációs beállítások
SQL indexelő számos konfigurációs beállítás közzétesz:

| Beállítás | Adattípus | Cél | Alapértelmezett érték |
| --- | --- | --- | --- |
| queryTimeout |Karakterlánc |Készletek hello SQL-lekérdezés végrehajtása időtúllépés |5 perc ("00: 05:00") |
| disableOrderByHighWaterMarkColumn |logikai érték |Hatására a hello hello magas vízjel alapján házirend tooomit hello ORDER BY záradék által használt SQL-lekérdezésben. Lásd: [magas vízjel alapján házirend](#HighWaterMarkPolicy) |hamis |

Ezek a beállítások használhatók a hello `parameters.configuration` hello indexelő definíciós objektumban. Például tooset hello lekérdezés időkorlátja too10 perc, hozzon létre vagy hello indexelő frissítése a következő konfigurációs hello:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="faq"></a>GYIK

**K: használhatok Azure SQL indexelő Azure IaaS virtuális gépeken futó SQL-adatbázisok?**

Igen. Azonban tooallow a keresési szolgáltatás tooconnect tooyour adatbázis szükséges. További információkért lásd: [egy egy Azure Search-indexelőt tooSQL kiszolgáló közötti kapcsolat kezelése egy Azure virtuális gépen](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).

**K: használhatok Azure SQL indexelő helyileg futó SQL-adatbázisok?**

Közvetlenül nem. Nem javasoljuk vagy támogatják a közvetlen kapcsolat létrehozásakor, ezzel úgy igényelnének, tooopen az adatbázisok tooInternet forgalom. Az ügyfelek a jelen forgatókönyvben híd technológiák használatával például az Azure Data Factory sikeres volt. További információkért lásd: [leküldéses adatok tooan Azure Search-index Azure Data Factory használatával](https://docs.microsoft.com/azure/data-factory/data-factory-azure-search-connector).

**K: használhatok Azure SQL indexelő nem Azure IaaS futó SQL Server adatbázisok?**

Nem. Ebben az esetben nem támogatjuk, mert a nem SQL Server adatbázisok hello indexelő nem ellenőriztük.  

**K: hozható létre több indexelők ütemezés szerint fut?**

Igen. Azonban csak egy indexelő futtathat egy csomóponton egy időben. Ha egyidejűleg több indexelők van szüksége, fontolja meg a keresési szolgáltatás toomore, mint egy keresési egység vertikális felskálázásával.

**K: Indexelő hatással a lekérdezés munkaterhelések futtatásával?**

Igen. A keresőszolgáltatása hello csomópontjának egyik indexelő futása, és adott csomóponton erőforrások indexelést és a lekérdezés forgalom és az egyéb API-kérelmek kiszolgáló között vannak megosztva. Ha intenzív indexelő és a lekérdezés alkalmazásokat és szolgáltatásokat futtathatnak, és 503-as hibáinak vagy növekszik a válaszhoz szükséges idő nagy mértékű tapasztal, érdemes lehet [a keresőszolgáltatása vertikális felskálázásával](search-capacity-planning.md).

**K: Használhatom az egy másodlagos másodpéldány egy [feladatátvevő fürt](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) adatforrásként?**

Ez a konkrét licenctől függ. A teljes indexelő egy tábla vagy nézet, egy másodlagos másodpéldány is használhatja. 

Növekményes indexelés, két Azure Search támogatja észlelési szabályzatok módosítása: az SQL integrált módosítása nyomkövetési és magas vízjel alapján.

A csak olvasható replika SQL-adatbázis nem támogatja az integrált változáskövetés. Ezért a magas vízjel alapján házirendet kell használnia. 

A szabványos ajánljuk, toouse hello rowversion oszlop adattípusa esetében hello magas vízjel alapján. Azonban az SQL-adatbázis használatával rowversion támaszkodik `MIN_ACTIVE_ROWVERSION` függvénynek, amely csak olvasható replika nem támogatott. Ezért rowversion használata hello indexelő tooa elsődleges replikán kell mutatnia.

Ha egy csak olvasható replika toouse rowversion, hello a következő hiba jelenik meg: 

    "Using a rowversion column for change tracking is not supported on secondary (read-only) availability replicas. Please update hello datasource and specify a connection toohello primary availability replica.Current database 'Updateability' property is 'READ_ONLY'".

**K: használhatok egy alternatív, nem rowversion oszlop magas vízjel alapján változáskövetési?**

Nem ajánlott. Csak **rowversion** lehetővé teszi, hogy a megbízható adatok szinkronizálásához. Azonban, attól függően, hogy az alkalmazáslogikát, hogy nem ha:

+ Biztosíthatja, hogy hello indexelő futása, amikor nincsenek függőben lévő tranzakciók, amelyek indexelése hello táblán (például minden tábla frissítés történhet meg egy kötegelt ütemezés szerint, és hello Azure keresési indexelő ütemezés tooavoid hello tábla átfedésben van beállítva. frissítési ütemezés).  

+ Rendszeres időközönként tegye a teljes ismételt indexelése toopick be a hiányzó sorokat. 
