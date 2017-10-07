---
title: "aaaHow toouse Fiddler tooevaluate és az Azure Search REST API-k tesztelése |} Microsoft Docs"
description: "Fiddler használata a kódolás nélkül tooverifying Azure Search rendelkezésre állását, illetve próbálhatja ki hello REST API-k esetében."
services: search
documentationcenter: 
author: HeidiSteen
manager: mblythe
editor: 
ms.assetid: 790e5779-c6a3-4a07-9d1e-d6739e6b87d2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: heidist
ms.openlocfilehash: 2912e1180717d7b40a1e4f7f7f00daf2cc254f0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-fiddler-tooevaluate-and-test-azure-search-rest-apis"></a>Használja a Fiddler tooevaluate és tesztelése az Azure Search REST API-k
> [!div class="op_single_selector"]
>
> * [Áttekintés](search-query-overview.md)
> * [Keresési ablak](search-explorer.md)
> * [Fiddler](search-fiddler.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
>
>

Ez a cikk azt ismerteti, hogyan toouse Fiddler, valamint egy [Telerik ingyenesen letölthető](http://www.telerik.com/fiddler), tooissue HTTP kérelmeket tooand válaszok megtekintése hello Azure Search REST API használatával anélkül, hogy toowrite összes kódot. Az Azure Search teljes körűen felügyelt, üzemeltetett felhőalapú keresőszolgáltatás, amely egyszerűen programozható .NET és REST API-kon keresztül. hello Azure Search szolgáltatás REST API-jainak dokumentációja a [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).

A lépéseket követve hello lesz index létrehozása, dokumentumok, lekérdezés hello index, majd szolgáltatási adatokat lekérdezés hello rendszer feltöltése.

Ezek a lépések toocomplete, szüksége lesz egy Azure Search szolgáltatást és `api-key`. Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) tooget indításának kapcsolatos utasításokat.

## <a name="create-an-index"></a>Index létrehozása
1. Indítsa el a Fiddlert. A hello **fájl** menüben kapcsolja ki a **forgalom rögzítése** toohide HTTP-tevékenység, amely független toohello aktuális feladatot.
2. A hello **szerkesztő** lapon állítson össze a kérelmeket, amelyek a következő képernyőfelvétel hello tűnik.

      ![][1]
3. Válassza a **PUT** lehetőséget.
4. Adjon meg egy URL-címet, amely meghatározza a hello szolgáltatás URL-címe, attribútumainak és hello api-verzió. Néhány mutatók tookeep figyelembe vételével:

   * HTTPS használata hello előtag.
   * A kérelemattribútum az „/indexes/hotels”. Ez egy "hotels" nevű index alapján keresési toocreate.
   * Az api-version kisbetűvel van írva, és a következő formában van megadva: „?api-version=2016-09-01”. Az API-verziók azért fontosak, mert az Azure Search rendszeresen telepíti a frissítéseket. Ritka esetekben a szolgáltatás frissítése a legfrissebb módosítása toohello API vezethetnek. Emiatt az Azure Search számára minden kérelemnél meg kell adni az API-verziót, hogy Ön mindig teljes mértékben kézben tudja tartani, hogy melyik verzió van használatban.

     hello teljes URL-címet a következő példa hasonló toohello kell kinéznie.

             https://my-app.search.windows.net/indexes/hotels?api-version=2016-09-01
5. Adja meg a hello kérelemfejléc hello állomás és api-kulcs cseréje, amelyek az Ön szolgáltatásában érvényes értékekkel.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
6. A kérelem törzse területre illessze be a hello index definícióját alkotó mezőket hello.

          {
         "name": "hotels",  
         "fields": [
           {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
           {"name": "baseRate", "type": "Edm.Double"},
           {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
           {"name": "hotelName", "type": "Edm.String"},
           {"name": "category", "type": "Edm.String"},
           {"name": "tags", "type": "Collection(Edm.String)"},
           {"name": "parkingIncluded", "type": "Edm.Boolean"},
           {"name": "smokingAllowed", "type": "Edm.Boolean"},
           {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
           {"name": "rating", "type": "Edm.Int32"},
           {"name": "location", "type": "Edm.GeographyPoint"}
          ]
         }
7. Kattintson az **Execute** (Végrehajtás) parancsra.

Néhány másodpercen belül egy 201-es HTTP-válasz hello munkamenetlistában megjelenik, ami hello index sikeresen létrejött.

Ha HTTP 504, ellenőrizze a hello URL-címben a HTTPS PROTOKOLLT. Ha megjelenik a HTTP 400-as vagy 404-es, a szervezet tooverify nem volt-e beillesztési hibák hello kérés ellenőrzéséhez. Egy HTTP 403 általában hello api-kulccsal (Érvénytelen a kulcs vagy szintaktikai hiba van hogyan hello api-kulcs van megadva) kapcsolatos problémát jelez.

## <a name="load-documents"></a>Dokumentumok betöltése
A hello **szerkesztő** lapon a kérelem toopost dokumentumok hello következőképpen néznek. hello kérelem törzse hello 4 szállodák hello keresési adatait tartalmazza.

   ![][2]

1. Válassza a **POST** lehetőséget.
2. Adjon meg egy olyan URL-címet, amely a HTTPS előtaggal kezdődik, amelyet a szolgáltatás URL-címe, majd az „/indexes/<'indexnév'>/docs/index?api-version=2016-09-01” karakterlánc követ. hello teljes URL-címet a következő példa hasonló toohello kell kinéznie.

         https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
3. Kérelem fejléce kell lennie hello azonos mint korábban. Ne feledje, hogy Ön helyett hello állomás és api-kulcs értékeket, amelyek az Ön szolgáltatásában érvényes.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. hello kérelem törzse négy dokumentumok toobe hozzáadott toohello szállodák indexet tartalmaz.

         {
         "value": [
         {
             "@search.action": "upload",
             "hotelId": "1",
             "baseRate": 199.0,
             "description": "Best hotel in town",
             "hotelName": "Fancy Stay",
             "category": "Luxury",
             "tags": ["pool", "view", "wifi", "concierge"],
             "parkingIncluded": false,
             "smokingAllowed": false,
             "lastRenovationDate": "2010-06-27T00:00:00Z",
             "rating": 5,
             "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "2",
             "baseRate": 79.99,
             "description": "Cheapest hotel in town",
             "hotelName": "Roach Motel",
             "category": "Budget",
             "tags": ["motel", "budget"],
             "parkingIncluded": true,
             "smokingAllowed": true,
             "lastRenovationDate": "1982-04-28T00:00:00Z",
             "rating": 1,
             "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "3",
             "baseRate": 279.99,
             "description": "Surprisingly expensive",
             "hotelName": "Dew Drop Inn",
             "category": "Bed and Breakfast",
             "tags": ["charming", "quaint"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
           },
           {
             "@search.action": "upload",
             "hotelId": "4",
             "baseRate": 220.00,
             "description": "This could be hello one",
             "hotelName": "A Hotel for Everyone",
             "category": "Basic hotel",
             "tags": ["pool", "wifi"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
           }
          ]
         }
5. Kattintson az **Execute** (Végrehajtás) parancsra.

Néhány másodpercen belül meg kell jelennie egy 200-as HTTP-válasz hello munkamenetlistában. Ez azt jelzi, hogy hello dokumentumok sikeresen létrejöttek. Ha a 207-es, legalább egy dokumentum tooupload nem sikerült. Ha a 404-es, hello fejléc vagy a hello kérés törzsében szintaktikai hiba van.

## <a name="query-hello-index"></a>Lekérdezés hello indexe
Most, hogy az index és a dokumentumok is betöltődtek, lekérdezheti őket.  A hello **szerkesztő** lapon egy **beolvasása** a szolgáltatást lekérdező parancs a következő képernyőfelvétel hasonló toohello fog kinézni.

   ![][3]

1. Válassza a **GET** lehetőséget.
2. Adjon meg egy olyan URL-címet, amely a HTTPS előtaggal kezdődik, amelyet a szolgáltatási URL, majd az „/indexes/<'indexname'>/docs?” karakterlánc, végül a lekérdezési paraméterek követnek. Példaképpen használja a következő URL-cím, egy, az Ön szolgáltatásában érvényes hello minta állomásnév cseréje hello.

         https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2016-09-01

   Ez a lekérdezés a "motel" hello kifejezés keres, és értékkorlátozó kategóriákat értékelések.
3. Kérelem fejléce kell lennie hello azonos mint korábban. Ne feledje, hogy Ön helyett hello állomás és api-kulcs értékeket, amelyek az Ön szolgáltatásában érvényes.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444

hello válaszkód a 200-as kell lennie, és hello válasz kimenete a következő képernyőfelvétel hasonló toohello kell kinéznie.

   ![][4]

hello következő példalekérdezés származik hello [Search-Index művelet (Azure Search API)](http://msdn.microsoft.com/library/dn798927.aspx) az MSDN Webhelyén. Ebben a témakörben hello mintalekérdezéseket számos közé tartozik a szóközt tartalmaz, amely a Fiddler nem engedélyezettek. Minden szóközt cseréljen le a + karakterre, mielőtt beillesztené a hello előtt hello lekérdezés Fiddler használatával történő lekérdezés-karakterlánc hossza.

**A szóközök cseréje előtt:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01

**A szóközök + jelre cserélése után:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2016-09-01

## <a name="query-hello-system"></a>Lekérdezés hello rendszer
Hello rendszer tooget számát, valamint a tárolási dokumentumfelhasználás is lekérheti. A hello **szerkesztő** lapon a kérelem hasonló toohello következő jelenik meg, és hello válasz szerepleni fog hello számát a dokumentumok és a felhasznált lemezterület mérete.

 ![][5]

1. Válassza a **GET** lehetőséget.
2. Adjon meg egy olyan URL-címet, amely tartalmazza a szolgáltatás URL-címét, majd az „/indexes/hotels/stats?api-version=2016-09-01” karakterláncot:

         https://my-app.search.windows.net/indexes/hotels/stats?api-version=2016-09-01
3. Adja meg a hello kérelemfejléc hello állomás és api-kulcs cseréje, amelyek az Ön szolgáltatásában érvényes értékekkel.

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. Hagyja üresen hello kérés törzsében.
5. Kattintson az **Execute** (Végrehajtás) parancsra. Meg kell jelennie egy hello munkamenetlistában a 200-as HTTP-állapotkód:. Válassza ki a parancshoz közzétett hello bejegyzést.
6. Kattintson a hello **ellenőrök** lapra, majd hello **fejlécek** fülre, majd jelölje ki hello JSON formátumban. Meg kell jelennie hello dokumentumok száma és a tárhely méretét (kilobájtban).

## <a name="next-steps"></a>Következő lépések
Lásd: [Azure a Search szolgáltatás kezelése](search-manage.md) egy kódot nem megközelítés toomanaging és az Azure Search használatával.

<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
