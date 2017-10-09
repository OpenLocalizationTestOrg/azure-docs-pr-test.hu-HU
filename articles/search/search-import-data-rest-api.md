---
title: "AAA \"feltölteni az adatokat (REST API - Azure Search) |} Microsoft dokumentumok\""
description: "Ismerje meg, hogyan tooupload az tooan indexe az Azure Search használatával hello REST API-t."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: 8d0749fb-6e08-4a17-8cd3-1a215138abc6
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
ms.openlocfilehash: 6ba1336012d1f0f6d6d6c933e16aa879afb9b824
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-tooazure-search-using-hello-rest-api"></a>Feltöltés tooAzure keresési használatával végzett hello REST API-n
> [!div class="op_single_selector"]
>
> * [Áttekintés](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
>
>

Ez a cikk bemutatja, hogyan toouse hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/) tooimport adatokat az Azure Search-index.

A bemutató elindítása előtt [létre kell hoznia egy Azure Search-indexet](search-what-is-an-index.md).

Az rendelések toopush hello REST API használatával az indexbe egy HTTP POST kérelem tooyour index URL-végpontjának állít ki. HTTP-kérelem törzse hello dokumentumokat toobe hozzáadott, módosított és törölt tartalmazó JSON-objektum hello hello törzsét.

## <a name="identify-your-azure-search-services-admin-api-key"></a>Azonosítsa az Azure Search szolgáltatás rendszergazdai API-kulcsát
HTTP-kérelmeket küldhet a REST API-t hello szolgáltatást kiállításához *minden* API-kérelem hello api-kulcsot hello keresőszolgáltatáshoz generált tartalmaznia kell. Érvényes kulcs birtokában létesít megbízhatósági, egy kérelem alapon hello küldő hello kérelem és a kezelő hello szolgáltatás között.

1. toofind a szolgáltatás api-kulcsokat, bármikor beléphet toohello [Azure-portálon](https://portal.azure.com/)
2. Nyissa meg tooyour Azure Search szolgáltatás paneljét
3. Kattintson a hello "Kulcsok" ikonra

A szolgáltatás *rendszergazdai kulcsokkal* és *lekérdezési kulcsokkal* fog rendelkezni.

* Az elsődleges és másodlagos *adminisztrációs kulcsok* teljes körű tooall műveleteket, köztük a hello képességét toomanage hello szolgáltatást biztosítania hozzon létre, és törölje az indexek, az indexelők és az adatforrások. Két kulcs van, hogy a Folytatás toouse hello másodlagos kulcsát. Ha úgy dönt, hogy tooregenerate hello elsődleges kulcs, és fordítva.
* A *lekérdezési kulcsok* adjon olvasási hozzáférést tooindexes és a dokumentumok és keresési kérelmeket kibocsátó általában elosztott tooclient alkalmazások.

Az adatok importálása egy index hello célokra is használhatja az elsődleges vagy másodlagos adminisztrátori kulcsot.

## <a name="decide-which-indexing-action-toouse"></a>Döntse el, melyik indexelési művelet toouse
Hello REST API használata esetén a HTTP POST-kérésnél állít ki a JSON kérelem szervek tooyour Azure Search-index a végpont URL-címmel. a HTTP-kérés törzsében hello JSON-objektumot fog tartalmazni egyetlen JSON-tömb nevű, "érték" tartalmazó dokumentumok tooadd tooyour index milyen képviselő JSON-objektumok, frissítése vagy törlése.

Minden egyes hello "érték" tömb JSON-objektum egy dokumentum toobe indexelt jelöli. Az objektumok hello dokumentum kulcsot tartalmaz, és adja meg a szükséges hello indexelési művelet (feltöltési, egyesítési, törlés, stb.). Attól függően, amely alatt úgy dönt, műveletek hello csak bizonyos mezők szerepelnie kell függvénykötésnek nyilvántartott egyes dokumentumok:

| @search.action | Leírás | Az egyes dokumentumok kötelező mezői | Megjegyzések |
| --- | --- | --- | --- |
| `upload` |Egy `upload` művelete hasonló tooan "upsert", ahol hello dokumentum lesz beszúrni, ha új, majd frissíteni vagy lecserélni rá. |billentyűt, és bármely más mezők toodefine kívánja |Ha egy meglévő dokumentum frissítése vagy cseréje, bármely hello kérelemben nincs megadva mező rendelkezik-e a mező értéke túl`null`. Ez akkor fordul elő, akkor is, ha hello mező korábban megadott tooa értéke nem lehet null. |
| `merge` |Egy meglévő dokumentum hello található frissítések megadott mezőket. Ha hello dokumentum hello index nem létezik, hello egyesítés sikertelen lesz. |billentyűt, és bármely más mezők toodefine kívánja |Bármely egyesítésével megadott mező felül fogja írni a létező mező hello hello dokumentumban. Ez `Collection(Edm.String)` típusú mezőket tartalmaz. Például akkor, ha hello dokumentum tartalmaz egy mező `tags` értékű `["budget"]` és értékkel egyesítésével végrehajtása `["economy", "pool"]` a `tags`, végső értéke hello hello `tags` mező kitöltése `["economy", "pool"]`. Nem pedig a következő lesz: `["budget", "economy", "pool"]`. |
| `mergeOrUpload` |Ez a művelet viselkedik `merge` Ha hello kulcs már megadott dokumentum hello index már létezik. Ha hello dokumentum nem létezik, hasonlóan viselkedik `upload` egy új dokumentumot. |billentyűt, és bármely más mezők toodefine kívánja |- |
| `delete` |Hello megadott dokumentum eltávolítása hello index. |csak billentyű |A mezőket, akkor adjon meg eltérő hello kulcsmező figyelmen kívül hagyja. Ha azt szeretné, hogy a dokumentum egy egyéni mező tooremove, `merge` helyette és egyszerűen állítsa be az hello mezőt explicit módon toonull. |

## <a name="construct-your-http-request-and-request-body"></a>A HTTP-kérés és a kérés törzsének létrehozása
Most, hogy az index műveletekhez szükséges mezőértékek hello összegyűjtötte, készen áll a tooconstruct hello tényleges HTTP-kérelem és a JSON kérelem törzse tooimport adatait.

#### <a name="request-and-request-headers"></a>Kérés és kérésfejlécek
Hello URL-címében, szüksége lesz tooprovide a szolgáltatás nevét, index neve ("Hotels" nevű ebben az esetben), valamint hello megfelelő API-verzió (hello aktuális API-verzió `2016-09-01` dokumentum közzétételének hello időben). Szüksége lesz a toodefine hello `Content-Type` és `api-key` kérelem fejlécei. Az utóbbi hello használja a szolgáltatás adminisztrációs kulcsok egyikét.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>A kérelem törzse
```JSON
{
    "value": [
        {
            "@search.action": "upload",
            "hotelId": "1",
            "baseRate": 199.0,
            "description": "Best hotel in town",
            "description_fr": "Meilleur hôtel en ville",
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
            "description_fr": "Hôtel le moins cher en ville",
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
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close tootown hall and hello river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

Ebben az esetben a következő keresési műveleteket használjuk: `upload`, `mergeOrUpload` és `delete`.

Jelen példában feltételezzük, hogy a „hotels” index már fel van töltve dokumentumokkal. Vegye figyelembe, hogyan jelenleg nem rendelkezett toospecify minden hello lehetséges dokumentum mező használata esetén `mergeOrUpload` , és hogyan azt csak meghatározott hello dokumentum kulcs (`hotelId`) használatakor `delete`.

Emellett vegye figyelembe, hogy csak akkor szerepelhet too1000 dokumentumok (vagy 16 MB) egyetlen indexelési kérelemben.

## <a name="understand-your-http-response-code"></a>A HTTP válaszkód ismertetése
#### <a name="200"></a>200
Az indexelési kérés sikeres elküldését követően HTTP-választ fog kapni a következő állapotkóddal: `200 OK`. hello hello HTTP-válasz törzsében JSON a következők:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
Ha az indexelés legalább egy elem esetében meghiúsult, a rendszer a következő állapotkódot fogja visszaadni: `207`. hello hello HTTP-válasz törzsében JSON hello sikertelen dokumentum információkat tartalmaznak.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "hello search service is too busy tooprocess this document. Please try again later."
        },
        ...
    ]
}
```

> [!NOTE]
> Ez gyakran azt jelenti, hogy hello betöltése a Search szolgáltatás közel jár a pont, ahol kérelmek indexelő megkezdődik tooreturn `503` válaszokat. Ebben az esetben határozottan javasoljuk az ügyfélkód visszahívását és az újrapróbálkozás előtt egy kis várakozást. Ekkor kap hello rendszer bizonyos idő toorecover növelése hello esélyét, hogy a későbbi kérelmek sikeres lesz. A kérelmek gyorsan újrapróbálkozás csak meghosszabbítása hello helyzet.
>
>

#### <a name="429"></a>429
Az állapotkódot `429` során túllépte a kvótát a dokumentumok / index hello száma adja vissza.

#### <a name="503"></a>503
Az állapotkódot `503` Ha hello kérelem hello elemek egyike sikeresen indexelt eredmény. Ez a hiba azt jelenti, hogy, hogy hello rendszer nagy terhelésnek van kitéve, és jelenleg nem lehet feldolgozni a kérését.

> [!NOTE]
> Ebben az esetben határozottan javasoljuk az ügyfélkód visszahívását és az újrapróbálkozás előtt egy kis várakozást. Ekkor kap hello rendszer bizonyos idő toorecover növelése hello esélyét, hogy a későbbi kérelmek sikeres lesz. A kérelmek gyorsan újrapróbálkozás csak meghosszabbítása hello helyzet.
>
>

További információk a dokumentumokkal végzett műveletekről, illetve a sikeres/meghiúsult műveletekre adott rendszerválaszokról: [Dokumentumok hozzáadása, frissítése vagy törlése](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents). További információk a meghiúsult műveletek esetében visszaadható HTTP-állapotkódokról: [HTTP-állapotkódok (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).

## <a name="next-steps"></a>Következő lépések
Után feltöltése az Azure Search-index, a lekérdezések toosearch dokumentumok kiállító készen toostart lesz. Részletes információk: [Az Azure Search-index lekérdezése](search-query-overview.md).
