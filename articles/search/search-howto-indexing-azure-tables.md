---
title: az Azure Search Azure Table storage aaaIndexing |} Microsoft Docs
description: "Ismerje meg, hogyan tooindex adatok tárolása az Azure Search Azure Table storage-ban"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 1cc27411-d0cc-40ed-8aed-c7cb9ab402b9
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/10/2017
ms.author: eugenesh
ms.openlocfilehash: abb01a4d807cede310246b34775d8059fceb4456
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="index-azure-table-storage-with-azure-search"></a>Az Azure Search index Azure Table storage
Ez a cikk bemutatja, hogyan toouse Azure Search tooindex adatok Azure Table storage-ban tárolja.

## <a name="set-up-azure-table-storage-indexing"></a>Azure Table storage indexelése beállítása

Az Azure Table storage indexelő állíthat be ezeket az erőforrásokat használatával:

* [Azure Portal](https://ms.portal.azure.com)
* Az Azure Search [REST API-n](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Az Azure Search [.NET SDK-val](https://aka.ms/search-sdk)

Itt bemutatjuk hello folyamata hello REST API használatával. 

### <a name="step-1-create-a-datasource"></a>1. lépés: Egy adatforrás létrehozása

Egy adatforrás milyen adatok tooindex határozza meg, hello hitelesítő adatok szükség tooaccess hello adatokat, és hello házirendek, amelyek lehetővé teszik az Azure Search tooefficiently azonosítása hello adatok változásai.

Az indexelő tábla, hello adatforrás következő tulajdonságai hello kell rendelkeznie:

- **név** hello hello datasource belül a keresési szolgáltatás egyedi neve.
- **típus** kell `azuretable`.
- **hitelesítő adatok** paraméter hello tárolási fiók kapcsolati karakterláncot tartalmaz. Lásd: hello [adja meg a hitelesítő adatok](#Credentials) című szakaszban talál információt.
- **tároló** beállítása hello tábla nevét és opcionális lekérdezés.
    - Adja meg a táblanevet hello hello használatával `name` paraméter.
    - Szükség esetén adjon meg egy lekérdezést a hello `query` paraméter. 

> [!IMPORTANT] 
> Amikor csak lehetséges, használjon szűrőt PartitionKey jobb teljesítmény érdekében. Bármely más lekérdezés nincs teljesítményproblémákat nagy táblák egy teljes tábla ellenőrzése. Lásd: hello [teljesítménnyel kapcsolatos szempontok](#Performance) szakasz.


a datasource toocreate:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Hozzon létre adatforrást API hello további információkért lásd: [adatforrás létrehozása](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="ways-toospecify-credentials"></a>Többféleképpen toospecify hitelesítő adatok ####

Hello hitelesítő hello tábla az alábbi módszerek valamelyikével: 

- **Teljes hozzáférés tárolási fiók kapcsolati karakterlánc**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>` kaphat hello kapcsolati karakterláncot a hello Azure-portálon is toohello **Storage-fiók panelen** > **beállítások**   >  **Kulcsok** (a klasszikus tárfiókokba) vagy **beállítások** > **hívóbetűk** (az Azure-erőforrás Manager storage-fiókok).
- **A tárfiók megosztott hozzáférési aláírást kapcsolati karakterlánc**: `TableEndpoint=https://<your account>.table.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<hello signature>&spr=https&se=<hello validity end time>&srt=co&ss=t&sp=rl` hello közös hozzáférésű jogosultságkódot hello listában, és olvasási engedéllyel a (jelen esetben táblák) tárolók és objektumok (táblázat sorait) kell rendelkeznie.
-  **Tábla közös hozzáférésű jogosultságkódot**: `ContainerSharedAccessUri=https://<your storage account>.table.core.windows.net/<table name>?tn=<table name>&sv=2016-05-31&sig=<hello signature>&se=<hello validity end time>&sp=r` hello közös hozzáférésű jogosultságkódot hello táblán lekérdezés (olvasás) engedélyekkel kell rendelkezniük.

További információ a megosztott tárolón aláírások hozzáférni, lásd: [használata közös hozzáférésű jogosultságkód](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Ha megosztott hozzáférési aláírást hitelesítő adatokat használ, akkor a lejárati idejük tooupdate hello adatforrás hitelesítő adatokat rendszeres időközönként megújított aláírások tooprevent. Ha megosztott hozzáférési aláírást hitelesítő adatok lejárnak, hello indexelő sikertelen, és hasonló túl "hello kapcsolati karakterláncban megadott hitelesítő adatok érvénytelenek, vagy lejárt." hibaüzenet  

### <a name="step-2-create-an-index"></a>2. lépés: Az index létrehozása
hello index hello mezők egy dokumentumot, hello attribútumok, adja meg, és más hoz létre, hogy alakzat hello keresési élményt biztosít.

az index toocreate:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
          ]
    }

Indexek létrehozásával kapcsolatos további információkért lásd: [a Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index).

### <a name="step-3-create-an-indexer"></a>3. lépés:, Hozzon létre egy indexelőt
Az indexelő datasource kapcsolódik egy cél search-index és egy tooautomate hello az Adatfrissítés ütemezéséről biztosít. 

Hello index és az adatforrás létrehozása után készen áll a toocreate hello indexelő Ön:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Az indexelő futása két óránként. (hello ütemezési időköz értéke túl "PT2H".) az indexelő toorun 30 percenként túl hello időköz beállítása "PT30M". hello legrövidebb támogatott időköz értéke öt perc. hello ütemezés nem kötelező megadni. Ha nincs megadva, az indexelő futása, csak egyszer, amikor létrejön. Azonban az indexelő bármikor futtathatja.   

A létrehozás indexelő API hello további információkért lásd: [létrehozása indexelő](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

## <a name="deal-with-different-field-names"></a>Különböző mezőnevek kezelése
Egyes esetekben hello mezőnevek a meglévő index eltérnek a tulajdonságnevek hello a táblában. A keresési index hozzárendelések toomap hello tulajdonság mezőnevei hello tábla toohello mezőnevek is használhatja. toolearn mezők leképezését kapcsolatos további információkért lásd: [Azure keresési indexelő mező hozzárendelések híd adatforrások és a keresési indexek hello különbségei](search-indexer-field-mappings.md).

## <a name="handle-document-keys"></a>A dokumentum kulcsok kezelése
Az Azure Search hello dokumentum kulcs egyedileg azonosít egy dokumentumot. Minden keresési index rendelkeznie kell típusú pontosan egy kulcsmező `Edm.String`. hello kulcsmező szükség minden egyes dokumentum toohello index hozzáadott. (Valójában azok hello csak kötelező mező.)

Tábla sora egy összetett kulcs, mert az Azure Search hoz létre egy szintetikus nevű mező `Key` , amely partíciós kulcs és a sor kulcsértékei összefűzése. Például ha egy sor PartitionKey van `PK1` és RowKey `RK1`, majd hello `Key` mező értéke `PK1RK1`.

> [!NOTE]
> Hello `Key` érték dokumentum kulcsok, például kötőjelek érvénytelen karaktert tartalmaz. Hello segítségével érvénytelen karaktereket az kezelését is `base64Encode` [leképezési függvény mezőben](search-indexer-field-mappings.md#base64EncodeFunction). Ha így tesz, ne felejtse el tooalso használjon biztonságos URL-cím Base64 kódolást keresési például dokumentum kulcsok benyújtása API hívja.
>
>

## <a name="incremental-indexing-and-deletion-detection"></a>Növekményes indexelő és törlési észlelése
Egy tábla indexelő toorun ütemezés szerint beállításakor azt reindexes csak új vagy frissített sorok, egy sor alapján `Timestamp` érték. A módosítás szabályzat toospecify nem rendelkezik. Növekményes indexelő engedélyezve van, automatikusan.

tooindicate, hogy bizonyos dokumentumok el kell távolítani hello index, egy helyreállítható törlésre stratégia is használhatja. Helyett a sor törlése, adjon hozzá egy tulajdonság tooindicate, hogy törölték, és állítson be egy helyreállítható törlési szabályzat a hello adatforráson. Például a következő házirend hello úgy véli, hogy, hogy egy sor törlése, ha hello sor tulajdonsággal rendelkezik `IsDeleted` hello értékű `"true"`:

    PUT https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "<query>" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   

<a name="Performance"></a>
## <a name="performance-considerations"></a>A teljesítménnyel kapcsolatos megfontolások

Alapértelmezés szerint az Azure Search használja a következő lekérdezés szűrője hello: `Timestamp >= HighWaterMarkValue`. Azure-táblákban nem rendelkezik egy másodlagos index a hello `Timestamp` mezőjét, ez egy teljes tábla vizsgálatot igényel, és ezért nagy táblák lassú.


Az alábbiakban az indexelési teljesítmény tábla két lehetséges módszer. Mindkét megoldás támaszkodnak az táblázat partíciókat: 

- Ha az adatok természetes lehet particionálni be több partíció-címtartományokat, hozzon létre egy adatforrást és egy megfelelő indexelő minden partíció tartományra. Minden indexelő most tartománnyal tooprocess csak egy adott partícióra, lekérdezés jobb teljesítményt eredményez. Ha indexelt toobe szükséges adatokat a hello rögzített partíciók, még élvezetesebbé kis számú: minden indexelő csak egy partíció vizsgálat does. Például toocreate partíció a tartományt a datasource kulcsok a `000` túl`100`, ilyen lekérdezés használata: 
    ```
    "container" : { "name" : "my-table", "query" : "PartitionKey ge '000' and PartitionKey lt '100' " }
    ```

- Ha az adatok particionálása idő szerint (például létrehozhat egy új partíció minden nap vagy hét), vegye figyelembe a következő megközelítés hello: 
    - Hello űrlap lekérdezéssel: `(PartitionKey ge <TimeStamp>) and (other filters)`. 
    - Figyelje az indexelő folyamatban [indexelő állapot API beszerzése](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status), és rendszeres időközönként frissíti az hello `<TimeStamp>` feltétel hello lekérdezés hello legújabb sikeres magas vízjel érték alapján. 
    - Ezt a módszert Ha egy teljes újraindexelés tootrigger kell kell tooreset hello adatforrás lekérdezési hozzáadása tooresetting hello indexelőben. 


## <a name="help-us-make-azure-search-better"></a>Segítsen az Azure Search továbbfejlesztésében
Ha funkciókra vonatkozó kérések vagy ötleteket javításai, elküldeni őket a [UserVoice webhelyén](https://feedback.azure.com/forums/263029-azure-search/).
