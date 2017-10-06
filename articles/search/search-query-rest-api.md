---
title: "AAA \"lekérdezheti az indexét (REST API - Azure Search) |} Microsoft dokumentumok\""
description: "Hozza létre az Azure search keresési lekérdezés, és használja a keresési paraméterek toofilter és rendezési keresési eredmények."
services: search
documentationcenter: 
manager: jhubbard
author: ashmaka
ms.assetid: 8b3ca890-2f5f-44b6-a140-6cb676fc2c9c
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 01/12/2017
ms.author: ashmaka
ms.openlocfilehash: 2f12238b8f4b045f536489cfc8766fb68307bbe2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="query-your-azure-search-index-using-hello-rest-api"></a>Hello REST API használatával az Azure Search-index lekérdezése
> [!div class="op_single_selector"]
>
> * [Áttekintés](search-query-overview.md)
> * [Portál](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
>
>

Ez a cikk bemutatja, hogyan egy index használatával tooquery hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).

A bemutató elindítása előtt [létre kell hoznia egy Azure Search-indexet](search-what-is-an-index.md), majd [fel kell töltenie azt adatokkal](search-what-is-data-import.md). Háttér-információkért lásd: [A teljes szöveges keresés működése az Azure Search szolgáltatásban](search-lucene-query-architecture.md).

## <a name="identify-your-azure-search-services-query-api-key"></a>Azonosítsa az Azure Search szolgáltatás lekérdezési API-kulcsát
Minden keresési művelet hello Azure Search REST API nyilvános kulcsokra épülő hello *api-kulcs* létesített hello szolgáltatás számára létrehozott. Érvényes kulcs birtokában létesít megbízhatósági, egy kérelem alapon hello küldő hello kérelem és a kezelő hello szolgáltatás között.

1. toofind a szolgáltatás api-kulcsokat, bármikor beléphet toohello [Azure-portálon](https://portal.azure.com/)
2. Nyissa meg tooyour Azure Search szolgáltatás paneljét
3. Hello "Kulcsok" ikonra

A szolgáltatás *rendszergazdai kulcsokkal* és *lekérdezési kulcsokkal* rendelkezik.

* Az elsődleges és másodlagos *adminisztrációs kulcsok* teljes körű tooall műveleteket, köztük a hello képességét toomanage hello szolgáltatást biztosítania hozzon létre, és törölje az indexek, az indexelők és az adatforrások. Két kulcs van, hogy a Folytatás toouse hello másodlagos kulcsát. Ha úgy dönt, hogy tooregenerate hello elsődleges kulcs, és fordítva.
* A *lekérdezési kulcsok* adjon olvasási hozzáférést tooindexes és a dokumentumok és keresési kérelmeket kibocsátó általában elosztott tooclient alkalmazások.

Az index lekérdezése hello alkalmazásában a lekérdezési kulcsok egyikét használhatja. Az adminisztrációs kulcsok is használható a lekérdezések, de egy lekérdezési kulcsot kell használni az alkalmazás kódjában, az alábbi módon ez jobban hello [legalacsonyabb jogosultsági szint elve](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="formulate-your-query"></a>A lekérdezés meghatározása
Két módon túl[keresse meg a hello REST API használatával](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). Egyik módja tooissue HTTP POST-kérelmet ahol definiálva vannak a lekérdezés-paraméterek a JSON-objektumból hello kérés törzsében. hello más módon tooissue HTTP GET kérelemre, ahol meg van határozva a lekérdezési paraméterek belül hello kérelem URL-CÍMÉT. ÁLLOMÁS rendelkezik több [korlátok enyhíteni](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) a lekérdezési paraméterei nem felelnek meg GET hello mérete. Éppen ezért a POST használatát javasoljuk, hacsak nem állnak fenn olyan speciális körülmények, amelyek a GET használatát kényelmesebbé tennék.

A POST és a GET tooprovide kell a *szolgáltatásnév*, *indexnév*, és megfelelő hello *API-verzió* (hello aktuális API-verzió `2016-09-01` hello időpontban Ez a dokumentum közzétételének) hello a kérelem URL-CÍMÉT. GET, hello *lekérdezési karakterlánc* hello: hello URL-cím végéhez, ahol meg kell adnia hello lekérdezési paramétereket. Olvassa el az alábbi hello URL-formátum:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2016-09-01

hello formázza a FELADÁS egy vagy több rendszer hello azonos, de csak az api-version hello lekérdezési karakterlánc paraméterek.

#### <a name="example-queries"></a>Példa a lekérdezésekre
Alább néhány példa látható a „hotels” nevű index lekérdezéseire. Ezek a lekérdezések GET- és POST-formátumban is megtekinthetők.

Hello teljes index keressen hello kifejezés "költségvetés", és térjen vissza csak hello `hotelName` mező:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "budget",
    "select": "hotelName"
}
```

A szűrő toohello index toofind szállodák olcsóbb, mint 150 $ éjszakánként alkalmazni, és térjen vissza a hello `hotelId` és `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Keresési hello teljes index, sorrendben egy adott mező (`lastRenovationDate`) csökkenő sorrendben, érvénybe hello felső két eredményeit, és csak megjelenítése `hotelName` és `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="submit-your-http-request"></a>A HTTP-kérés elküldése
Azt követően, hogy meghatározta a lekérdezést a HTTP-kérés (GET esetében) URL-címének vagy (POST esetében) törzsének részeként, megadhatja a kérésfejléceket, majd elküldheti a lekérdezést.

#### <a name="request-and-request-headers"></a>Kérés és kérésfejlécek
A GET esetében kettő, a POST esetében három kérésfejlécet kell meghatároznia:

1. Hello `api-key` fejléc talált lépésben I fenti toohello lekérdezési kulcsot kell állítani. Egy adminisztrációs kulcsot is használhatja, hello `api-key` fejléc, de az ajánlott, hogy egy lekérdezési kulcsot, kizárólag ad csak olvasási hozzáféréssel tooindexes és dokumentumok.
2. Hello `Accept` fejléc túl be kell állítani`application/json`.
3. Csak a POST hello `Content-Type` fejlécben is meg kell túl`application/json`.

Olvassa el az alábbi HTTP GET kérést toosearch hello "Hotels nevű" index használatával hello Azure Search REST API-t egy egyszerű lekérdezést, amely megkeresi a "motel" hello kifejezés használatával:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2016-09-01
Accept: application/json
api-key: [query key]
```

Ez megegyezik hello példalekérdezés, ez alkalommal HTTP POST használatával:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

A sikeres kérelmek állapotkódot eredményez `200 OK` és hello keresés eredményeinek JSON-ként hello válasz törzsében. Íme, milyen hello hello fent lekérdezés kinézetét, feltéve, hogy hello "Hotels"nevű index példaadatok hello fel van töltve az eredmények [adatok importálása az Azure Search használatával hello REST API](search-import-data-rest-api.md) (vegye figyelembe, hogy hello JSON formátumú egyértelműség).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

toolearn több, látogasson el a hello "Válasz" szakaszában [dokumentumok keresése](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). További információk a meghiúsult műveletek esetében visszaadható HTTP-állapotkódokról: [HTTP-állapotkódok (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).
