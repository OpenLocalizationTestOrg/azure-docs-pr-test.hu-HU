---
title: "AAA \"(REST API - Azure Search) index létrehozása |} Microsoft dokumentumok\""
description: "Index létrehozása kódban hello Azure Search HTTP REST API használatával."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: ac6c5fba-ad59-492d-b715-d25a7a7ae051
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
ms.openlocfilehash: 117ab64a9874a443351a8a02a9b959b8f7beb7c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-index-using-hello-rest-api"></a>Hello REST API-t használó Azure Search-index létrehozása
> [!div class="op_single_selector"]
>
> * [Áttekintés](search-what-is-an-index.md)
> * [Portál](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
>
>

Ez a cikk végigvezeti az Azure Search létrehozásának folyamatán hello [index](https://docs.microsoft.com/rest/api/searchservice/Create-Index) Azure Search REST API használatával hello.

Már az útmutató követése és az index létrehozása előtt [létre kell hoznia egy Azure Search szolgáltatást](search-create-service-portal.md).

az Azure Search-index hello REST API használatával, állít ki egy egyetlen HTTP POST kérelem tooyour Azure Search szolgáltatás URL-végpontjának toocreate. Az index definícióját hello kérés törzse fogja tartalmazni megfelelően formázott JSON-tartalomként.

## <a name="identify-your-azure-search-services-admin-api-key"></a>Azonosítsa az Azure Search szolgáltatás rendszergazdai API-kulcsát
Most, hogy létrehozta az Azure Search szolgáltatást, HTTP-kérelmeket küldhet a szolgáltatás URL-végpontjának hello REST API használatával adhat ki. *Minden* API-kérelemnek tartalmaznia hello api-kulcsot hello keresőszolgáltatáshoz generált. Érvényes kulcs birtokában létesít megbízhatósági, egy kérelem alapon hello küldő hello kérelem és a kezelő hello szolgáltatás között.

1. toofind a szolgáltatás api-kulcsok be kell jelentkeznie hello [Azure-portálon](https://portal.azure.com/)
2. Nyissa meg tooyour Azure Search szolgáltatás paneljét
3. Kattintson a hello "Kulcsok" ikonra

A szolgáltatás *rendszergazdai kulcsokkal* és *lekérdezési kulcsokkal* fog rendelkezni.

* Az elsődleges és másodlagos *adminisztrációs kulcsok* teljes körű tooall műveleteket, köztük a hello képességét toomanage hello szolgáltatást biztosítania hozzon létre, és törölje az indexek, az indexelők és az adatforrások. Két kulcs van, hogy a Folytatás toouse hello másodlagos kulcsát. Ha úgy dönt, hogy tooregenerate hello elsődleges kulcs, és fordítva.
* A *lekérdezési kulcsok* adjon olvasási hozzáférést tooindexes és a dokumentumok és keresési kérelmeket kibocsátó általában elosztott tooclient alkalmazások.

Index létrehozása hello célokra is használhatja az elsődleges vagy másodlagos adminisztrátori kulcsot.

## <a name="define-your-azure-search-index-using-well-formed-json"></a>Az Azure Search-index meghatározása megfelelően formázott JSON-tartalommal
Egyetlen HTTP POST kérelem tooyour szolgáltatás létrehozza az indexet. a HTTP POST kérelem törzse hello egy egyetlen JSON-objektum, amely meghatározza az Azure Search-index fogja tartalmazni.

1. hello a JSON-objektum első tulajdonsága az index hello neve.
2. hello a JSON-objektum második tulajdonsága nem nevű JSON-tömb `fields` , amely tartalmazza az index egyes mezőihez külön JSON-objektumokat. A JSON-objektumok mindegyikének több név/érték párok tartalmaznak az egyes hello mező tulajdonságai, például a "name", "típus," stb.

Fontos, hogy a keresés felhasználói élményt és az üzleti igényeket figyelembe venni az index tervezésekor, mivel minden egyes mezőt hozzá kell rendelni hello [megfelelő attribútumokhoz](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Ezek az attribútumok határozzák melyik keresési funkciók (szűrés, értékkorlátozás, rendezés, teljes szöveges keresés stb.) alkalmazni toowhich mezőket. Meg nem adott attribútumok hello alapértelmezett hacsak kifejezetten le lesz tooenable hello megfelelő keresési funkciót.

A fenti példában az indexnek a „hotels” nevet adtuk, és a mezőket az alábbiak szerint definiáltuk:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

Hello attribútum minden mezőt, ahogyan szerintünk használni fogják őket egy alkalmazásban alapján választottuk ki. Például `hotelId` egyedi kulcs az adott személyek keresése a szállodák valószínűleg nem fognak ismerni, ezért értékre állításával letiltjuk a teljes szöveges keresés mező `searchable` túl`false`, így helyet takarítunk hello index.

Vegye figyelembe, hogy pontosan egy mezőt az indexben típusú `Edm.String` hello kijelölt hello "key" mező.

a fenti hello Indexdefiníció nyelvi elemzőt használ a hello `description_fr` mezőben, mert az adott toostore francia szöveg. Lásd: [hello nyelvi támogatás című](https://docs.microsoft.com/rest/api/searchservice/Language-support) valamint hello vonatkozó [blogbejegyzés](https://azure.microsoft.com/blog/language-support-in-azure-search/) nyelvi elemzőkkel kapcsolatos további információk.

## <a name="issue-hello-http-request"></a>A probléma hello HTTP-kérelem
1. Az index definícióját használatával hello kérelemtörzset, ki egy HTTP POST kérelem tooyour Azure Search szolgáltatás végpont URL-CÍMÉT. Hello URL-címében, lehet, hogy toouse hello állomásnevet, a szolgáltatás neve, és hogy a megfelelő hello `api-version` lekérdezési karakterlánc paraméterként (hello aktuális API-verzió `2016-09-01` dokumentum közzétételének hello időben).
2. Hello kérelemfejléc, adja meg a hello `Content-Type` , `application/json`. Konfigurálnia kell tooprovide a szolgáltatás hello i. lépésben azonosított adminisztrációs kulcsát `api-key` fejléc.

Meg kell tooprovide a saját nevét és api fő tooissue hello szolgáltatáskérés alatt:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [api-key]


Ha a kérelem sikeres, a 201-es állapotkód (Létrehozva) jelenik meg. Hello REST API-n keresztül index létrehozásával kapcsolatos további információkért látogasson el a hello [itt API-referencia](https://docs.microsoft.com/rest/api/searchservice/Create-Index). További információk a meghiúsult műveletek esetében visszaadható HTTP-állapotkódokról: [HTTP-állapotkódok (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).

Amikor végzett az index és a kívánt toodelete azt, csak küldjön egy HTTP DELETE kérelmet. Ez az például hello "Hotels nevű" index következőképpen törölhető:

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2016-09-01
    api-key: [api-key]


## <a name="next-steps"></a>Következő lépések
Miután létrehozta az Azure Search-index, lehetővé válik túl[feltöltse a tartalmát hello indexbe](search-what-is-data-import.md) , és megkezdje az adatok keresését.
