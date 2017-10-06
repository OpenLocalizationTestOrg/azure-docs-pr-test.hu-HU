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
# <a name="query-your-azure-search-index-using-hello-rest-api"></a><span data-ttu-id="1cc69-103">Hello REST API használatával az Azure Search-index lekérdezése</span><span class="sxs-lookup"><span data-stu-id="1cc69-103">Query your Azure Search index using hello REST API</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="1cc69-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="1cc69-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="1cc69-105">Portál</span><span class="sxs-lookup"><span data-stu-id="1cc69-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="1cc69-106">.NET</span><span class="sxs-lookup"><span data-stu-id="1cc69-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="1cc69-107">REST</span><span class="sxs-lookup"><span data-stu-id="1cc69-107">REST</span></span>](search-query-rest-api.md)
>
>

<span data-ttu-id="1cc69-108">Ez a cikk bemutatja, hogyan egy index használatával tooquery hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="1cc69-108">This article shows you how tooquery an index using hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>

<span data-ttu-id="1cc69-109">A bemutató elindítása előtt [létre kell hoznia egy Azure Search-indexet](search-what-is-an-index.md), majd [fel kell töltenie azt adatokkal](search-what-is-data-import.md).</span><span class="sxs-lookup"><span data-stu-id="1cc69-109">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md) and [populated it with data](search-what-is-data-import.md).</span></span> <span data-ttu-id="1cc69-110">Háttér-információkért lásd: [A teljes szöveges keresés működése az Azure Search szolgáltatásban](search-lucene-query-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="1cc69-110">For background information, see [How full text search works in Azure Search](search-lucene-query-architecture.md).</span></span>

## <a name="identify-your-azure-search-services-query-api-key"></a><span data-ttu-id="1cc69-111">Azonosítsa az Azure Search szolgáltatás lekérdezési API-kulcsát</span><span class="sxs-lookup"><span data-stu-id="1cc69-111">Identify your Azure Search service's query api-key</span></span>
<span data-ttu-id="1cc69-112">Minden keresési művelet hello Azure Search REST API nyilvános kulcsokra épülő hello *api-kulcs* létesített hello szolgáltatás számára létrehozott.</span><span class="sxs-lookup"><span data-stu-id="1cc69-112">A key component of every search operation against hello Azure Search REST API is hello *api-key* that was generated for hello service you provisioned.</span></span> <span data-ttu-id="1cc69-113">Érvényes kulcs birtokában létesít megbízhatósági, egy kérelem alapon hello küldő hello kérelem és a kezelő hello szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="1cc69-113">Having a valid key establishes trust, on a per request basis, between hello application sending hello request and hello service that handles it.</span></span>

1. <span data-ttu-id="1cc69-114">toofind a szolgáltatás api-kulcsokat, bármikor beléphet toohello [Azure-portálon](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="1cc69-114">toofind your service's api-keys, you can sign in toohello [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="1cc69-115">Nyissa meg tooyour Azure Search szolgáltatás paneljét</span><span class="sxs-lookup"><span data-stu-id="1cc69-115">Go tooyour Azure Search service's blade</span></span>
3. <span data-ttu-id="1cc69-116">Hello "Kulcsok" ikonra</span><span class="sxs-lookup"><span data-stu-id="1cc69-116">Click hello "Keys" icon</span></span>

<span data-ttu-id="1cc69-117">A szolgáltatás *rendszergazdai kulcsokkal* és *lekérdezési kulcsokkal* rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1cc69-117">Your service has *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="1cc69-118">Az elsődleges és másodlagos *adminisztrációs kulcsok* teljes körű tooall műveleteket, köztük a hello képességét toomanage hello szolgáltatást biztosítania hozzon létre, és törölje az indexek, az indexelők és az adatforrások.</span><span class="sxs-lookup"><span data-stu-id="1cc69-118">Your primary and secondary *admin keys* grant full rights tooall operations, including hello ability toomanage hello service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="1cc69-119">Két kulcs van, hogy a Folytatás toouse hello másodlagos kulcsát. Ha úgy dönt, hogy tooregenerate hello elsődleges kulcs, és fordítva.</span><span class="sxs-lookup"><span data-stu-id="1cc69-119">There are two keys so that you can continue toouse hello secondary key if you decide tooregenerate hello primary key, and vice-versa.</span></span>
* <span data-ttu-id="1cc69-120">A *lekérdezési kulcsok* adjon olvasási hozzáférést tooindexes és a dokumentumok és keresési kérelmeket kibocsátó általában elosztott tooclient alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="1cc69-120">Your *query keys* grant read-only access tooindexes and documents, and are typically distributed tooclient applications that issue search requests.</span></span>

<span data-ttu-id="1cc69-121">Az index lekérdezése hello alkalmazásában a lekérdezési kulcsok egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="1cc69-121">For hello purposes of querying an index, you can use one of your query keys.</span></span> <span data-ttu-id="1cc69-122">Az adminisztrációs kulcsok is használható a lekérdezések, de egy lekérdezési kulcsot kell használni az alkalmazás kódjában, az alábbi módon ez jobban hello [legalacsonyabb jogosultsági szint elve](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span><span class="sxs-lookup"><span data-stu-id="1cc69-122">Your admin keys can also be used for queries, but you should use a query key in your application code as this better follows hello [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span></span>

## <a name="formulate-your-query"></a><span data-ttu-id="1cc69-123">A lekérdezés meghatározása</span><span class="sxs-lookup"><span data-stu-id="1cc69-123">Formulate your query</span></span>
<span data-ttu-id="1cc69-124">Két módon túl[keresse meg a hello REST API használatával](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span><span class="sxs-lookup"><span data-stu-id="1cc69-124">There are two ways too[search your index using hello REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span></span> <span data-ttu-id="1cc69-125">Egyik módja tooissue HTTP POST-kérelmet ahol definiálva vannak a lekérdezés-paraméterek a JSON-objektumból hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="1cc69-125">One way is tooissue an HTTP POST request where your query parameters are defined in a JSON object in hello request body.</span></span> <span data-ttu-id="1cc69-126">hello más módon tooissue HTTP GET kérelemre, ahol meg van határozva a lekérdezési paraméterek belül hello kérelem URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="1cc69-126">hello other way is tooissue an HTTP GET request where your query parameters are defined within hello request URL.</span></span> <span data-ttu-id="1cc69-127">ÁLLOMÁS rendelkezik több [korlátok enyhíteni](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) a lekérdezési paraméterei nem felelnek meg GET hello mérete.</span><span class="sxs-lookup"><span data-stu-id="1cc69-127">POST has more [relaxed limits](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) on hello size of query parameters than GET.</span></span> <span data-ttu-id="1cc69-128">Éppen ezért a POST használatát javasoljuk, hacsak nem állnak fenn olyan speciális körülmények, amelyek a GET használatát kényelmesebbé tennék.</span><span class="sxs-lookup"><span data-stu-id="1cc69-128">For this reason, we recommend using POST unless you have special circumstances where using GET would be more convenient.</span></span>

<span data-ttu-id="1cc69-129">A POST és a GET tooprovide kell a *szolgáltatásnév*, *indexnév*, és megfelelő hello *API-verzió* (hello aktuális API-verzió `2016-09-01` hello időpontban Ez a dokumentum közzétételének) hello a kérelem URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="1cc69-129">For both POST and GET, you need tooprovide your *service name*, *index name*, and hello proper *API version* (hello current API version is `2016-09-01` at hello time of publishing this document) in hello request URL.</span></span> <span data-ttu-id="1cc69-130">GET, hello *lekérdezési karakterlánc* hello: hello URL-cím végéhez, ahol meg kell adnia hello lekérdezési paramétereket.</span><span class="sxs-lookup"><span data-stu-id="1cc69-130">For GET, hello *query string* at hello end of hello URL is where you provide hello query parameters.</span></span> <span data-ttu-id="1cc69-131">Olvassa el az alábbi hello URL-formátum:</span><span class="sxs-lookup"><span data-stu-id="1cc69-131">See below for hello URL format:</span></span>

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2016-09-01

<span data-ttu-id="1cc69-132">hello formázza a FELADÁS egy vagy több rendszer hello azonos, de csak az api-version hello lekérdezési karakterlánc paraméterek.</span><span class="sxs-lookup"><span data-stu-id="1cc69-132">hello format for POST is hello same, but with only api-version in hello query string parameters.</span></span>

#### <a name="example-queries"></a><span data-ttu-id="1cc69-133">Példa a lekérdezésekre</span><span class="sxs-lookup"><span data-stu-id="1cc69-133">Example Queries</span></span>
<span data-ttu-id="1cc69-134">Alább néhány példa látható a „hotels” nevű index lekérdezéseire.</span><span class="sxs-lookup"><span data-stu-id="1cc69-134">Here are a few example queries on an index named "hotels".</span></span> <span data-ttu-id="1cc69-135">Ezek a lekérdezések GET- és POST-formátumban is megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="1cc69-135">These queries are shown in both GET and POST format.</span></span>

<span data-ttu-id="1cc69-136">Hello teljes index keressen hello kifejezés "költségvetés", és térjen vissza csak hello `hotelName` mező:</span><span class="sxs-lookup"><span data-stu-id="1cc69-136">Search hello entire index for hello term 'budget' and return only hello `hotelName` field:</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "budget",
    "select": "hotelName"
}
```

<span data-ttu-id="1cc69-137">A szűrő toohello index toofind szállodák olcsóbb, mint 150 $ éjszakánként alkalmazni, és térjen vissza a hello `hotelId` és `description`:</span><span class="sxs-lookup"><span data-stu-id="1cc69-137">Apply a filter toohello index toofind hotels cheaper than $150 per night, and return hello `hotelId` and `description`:</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

<span data-ttu-id="1cc69-138">Keresési hello teljes index, sorrendben egy adott mező (`lastRenovationDate`) csökkenő sorrendben, érvénybe hello felső két eredményeit, és csak megjelenítése `hotelName` és `lastRenovationDate`:</span><span class="sxs-lookup"><span data-stu-id="1cc69-138">Search hello entire index, order by a specific field (`lastRenovationDate`) in descending order, take hello top two results, and show only `hotelName` and `lastRenovationDate`:</span></span>

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

## <a name="submit-your-http-request"></a><span data-ttu-id="1cc69-139">A HTTP-kérés elküldése</span><span class="sxs-lookup"><span data-stu-id="1cc69-139">Submit your HTTP request</span></span>
<span data-ttu-id="1cc69-140">Azt követően, hogy meghatározta a lekérdezést a HTTP-kérés (GET esetében) URL-címének vagy (POST esetében) törzsének részeként, megadhatja a kérésfejléceket, majd elküldheti a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="1cc69-140">Now that you have formulated your query as part of your HTTP request URL (for GET) or body (for POST), you can define your request headers and submit your query.</span></span>

#### <a name="request-and-request-headers"></a><span data-ttu-id="1cc69-141">Kérés és kérésfejlécek</span><span class="sxs-lookup"><span data-stu-id="1cc69-141">Request and Request Headers</span></span>
<span data-ttu-id="1cc69-142">A GET esetében kettő, a POST esetében három kérésfejlécet kell meghatároznia:</span><span class="sxs-lookup"><span data-stu-id="1cc69-142">You must define two request headers for GET, or three for POST:</span></span>

1. <span data-ttu-id="1cc69-143">Hello `api-key` fejléc talált lépésben I fenti toohello lekérdezési kulcsot kell állítani.</span><span class="sxs-lookup"><span data-stu-id="1cc69-143">hello `api-key` header must be set toohello query key you found in step I above.</span></span> <span data-ttu-id="1cc69-144">Egy adminisztrációs kulcsot is használhatja, hello `api-key` fejléc, de az ajánlott, hogy egy lekérdezési kulcsot, kizárólag ad csak olvasási hozzáféréssel tooindexes és dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="1cc69-144">You can also use an admin key as hello `api-key` header, but it is recommended that you use a query key as it exclusively grants read-only access tooindexes and documents.</span></span>
2. <span data-ttu-id="1cc69-145">Hello `Accept` fejléc túl be kell állítani`application/json`.</span><span class="sxs-lookup"><span data-stu-id="1cc69-145">hello `Accept` header must be set too`application/json`.</span></span>
3. <span data-ttu-id="1cc69-146">Csak a POST hello `Content-Type` fejlécben is meg kell túl`application/json`.</span><span class="sxs-lookup"><span data-stu-id="1cc69-146">For POST only, hello `Content-Type` header should also be set too`application/json`.</span></span>

<span data-ttu-id="1cc69-147">Olvassa el az alábbi HTTP GET kérést toosearch hello "Hotels nevű" index használatával hello Azure Search REST API-t egy egyszerű lekérdezést, amely megkeresi a "motel" hello kifejezés használatával:</span><span class="sxs-lookup"><span data-stu-id="1cc69-147">See below for an HTTP GET request toosearch hello "hotels" index using hello Azure Search REST API, using a simple query that searches for hello term "motel":</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2016-09-01
Accept: application/json
api-key: [query key]
```

<span data-ttu-id="1cc69-148">Ez megegyezik hello példalekérdezés, ez alkalommal HTTP POST használatával:</span><span class="sxs-lookup"><span data-stu-id="1cc69-148">Here is hello same example query, this time using HTTP POST:</span></span>

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

<span data-ttu-id="1cc69-149">A sikeres kérelmek állapotkódot eredményez `200 OK` és hello keresés eredményeinek JSON-ként hello válasz törzsében.</span><span class="sxs-lookup"><span data-stu-id="1cc69-149">A successful query request will result in a Status Code of `200 OK` and hello search results are returned as JSON in hello response body.</span></span> <span data-ttu-id="1cc69-150">Íme, milyen hello hello fent lekérdezés kinézetét, feltéve, hogy hello "Hotels"nevű index példaadatok hello fel van töltve az eredmények [adatok importálása az Azure Search használatával hello REST API](search-import-data-rest-api.md) (vegye figyelembe, hogy hello JSON formátumú egyértelműség).</span><span class="sxs-lookup"><span data-stu-id="1cc69-150">Here is what hello results for hello above query look like, assuming hello "hotels" index is populated with hello sample data in [Data Import in Azure Search using hello REST API](search-import-data-rest-api.md) (note that hello JSON has been formatted for clarity).</span></span>

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

<span data-ttu-id="1cc69-151">toolearn több, látogasson el a hello "Válasz" szakaszában [dokumentumok keresése](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span><span class="sxs-lookup"><span data-stu-id="1cc69-151">toolearn more, please visit hello "Response" section of [Search Documents](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span></span> <span data-ttu-id="1cc69-152">További információk a meghiúsult műveletek esetében visszaadható HTTP-állapotkódokról: [HTTP-állapotkódok (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span><span class="sxs-lookup"><span data-stu-id="1cc69-152">For more information on other HTTP status codes that could be returned in case of failure, see [HTTP status codes (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span></span>
