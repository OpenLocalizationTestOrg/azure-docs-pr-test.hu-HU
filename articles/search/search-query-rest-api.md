---
title: "Index lekérdezése (REST API – Azure Search) | Microsoft Docs"
description: "Létrehozhat keresési lekérdezést az Azure Search szolgáltatásban, a keresési eredmények szűrését és rendezését pedig keresési paraméterek használatával végezheti el."
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
ms.openlocfilehash: 49062bec233ad35cd457f9665fa94c1855343582
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="query-your-azure-search-index-using-the-rest-api"></a><span data-ttu-id="c922e-103">Az Azure Search-index lekérdezése a REST API használatával</span><span class="sxs-lookup"><span data-stu-id="c922e-103">Query your Azure Search index using the REST API</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="c922e-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c922e-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="c922e-105">Portál</span><span class="sxs-lookup"><span data-stu-id="c922e-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="c922e-106">.NET</span><span class="sxs-lookup"><span data-stu-id="c922e-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="c922e-107">REST</span><span class="sxs-lookup"><span data-stu-id="c922e-107">REST</span></span>](search-query-rest-api.md)
>
>

<span data-ttu-id="c922e-108">Ebből a cikkből megtudhatja, hogyan történik egy index lekérdezése az [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/) használatával.</span><span class="sxs-lookup"><span data-stu-id="c922e-108">This article shows you how to query an index using the [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>

<span data-ttu-id="c922e-109">A bemutató elindítása előtt [létre kell hoznia egy Azure Search-indexet](search-what-is-an-index.md), majd [fel kell töltenie azt adatokkal](search-what-is-data-import.md).</span><span class="sxs-lookup"><span data-stu-id="c922e-109">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md) and [populated it with data](search-what-is-data-import.md).</span></span> <span data-ttu-id="c922e-110">Háttér-információkért lásd: [A teljes szöveges keresés működése az Azure Search szolgáltatásban](search-lucene-query-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="c922e-110">For background information, see [How full text search works in Azure Search](search-lucene-query-architecture.md).</span></span>

## <a name="identify-your-azure-search-services-query-api-key"></a><span data-ttu-id="c922e-111">Azonosítsa az Azure Search szolgáltatás lekérdezési API-kulcsát</span><span class="sxs-lookup"><span data-stu-id="c922e-111">Identify your Azure Search service's query api-key</span></span>
<span data-ttu-id="c922e-112">Az Azure Search REST API-ján végrehajtott keresési műveletek egyik fontos eleme az Ön által üzembe helyezett szolgáltatás számára létrehozott *API-kulcs*.</span><span class="sxs-lookup"><span data-stu-id="c922e-112">A key component of every search operation against the Azure Search REST API is the *api-key* that was generated for the service you provisioned.</span></span> <span data-ttu-id="c922e-113">Érvényes kulcs birtokában kérelmenként létesíthető megbízhatósági kapcsolat a kérést küldő alkalmazás és az azt kezelő szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="c922e-113">Having a valid key establishes trust, on a per request basis, between the application sending the request and the service that handles it.</span></span>

1. <span data-ttu-id="c922e-114">A szolgáltatás API-kulcsainak megkereséséhez bejelentkezhet az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c922e-114">To find your service's api-keys, you can sign in to the [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="c922e-115">Nyissa meg az Azure Search szolgáltatáspaneljét</span><span class="sxs-lookup"><span data-stu-id="c922e-115">Go to your Azure Search service's blade</span></span>
3. <span data-ttu-id="c922e-116">Kattintson a „Kulcsok” ikonra</span><span class="sxs-lookup"><span data-stu-id="c922e-116">Click the "Keys" icon</span></span>

<span data-ttu-id="c922e-117">A szolgáltatás *rendszergazdai kulcsokkal* és *lekérdezési kulcsokkal* rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c922e-117">Your service has *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="c922e-118">Az elsődleges és másodlagos *rendszergazdai kulcsok* teljes jogosultságot biztosítanak az összes művelethez, beleértve a szolgáltatás felügyeletének, valamint az indexek, indexelők és adatforrások létrehozásának és törlésének képességét.</span><span class="sxs-lookup"><span data-stu-id="c922e-118">Your primary and secondary *admin keys* grant full rights to all operations, including the ability to manage the service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="c922e-119">Két kulcs létezi, tehát ha az elsődleges kulcs újbóli létrehozása mellett dönt, a másodlagos kulcsot továbbra is használhatja (ez fordítva is igaz).</span><span class="sxs-lookup"><span data-stu-id="c922e-119">There are two keys so that you can continue to use the secondary key if you decide to regenerate the primary key, and vice-versa.</span></span>
* <span data-ttu-id="c922e-120">A *lekérdezési kulcsok* csak olvasási hozzáférést biztosítanak az indexekhez és a dokumentumokhoz, és általában a keresési kéréseket kibocsátó ügyfélalkalmazások számára vannak kiosztva.</span><span class="sxs-lookup"><span data-stu-id="c922e-120">Your *query keys* grant read-only access to indexes and documents, and are typically distributed to client applications that issue search requests.</span></span>

<span data-ttu-id="c922e-121">Indexlekérdezéshez a lekérdezési kulcsok egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="c922e-121">For the purposes of querying an index, you can use one of your query keys.</span></span> <span data-ttu-id="c922e-122">A rendszergazdai kulcsok szintén használhatók a lekérdezésekhez, az alkalmazáskódban azonban inkább lekérdezési kulcsot használjon, mivel ez a módszer jobban követi a [legalacsonyabb jogosultsági szint elvét](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span><span class="sxs-lookup"><span data-stu-id="c922e-122">Your admin keys can also be used for queries, but you should use a query key in your application code as this better follows the [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span></span>

## <a name="formulate-your-query"></a><span data-ttu-id="c922e-123">A lekérdezés meghatározása</span><span class="sxs-lookup"><span data-stu-id="c922e-123">Formulate your query</span></span>
<span data-ttu-id="c922e-124">Kétféleképpen [keresheti meg az indexet a REST API használatával](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span><span class="sxs-lookup"><span data-stu-id="c922e-124">There are two ways to [search your index using the REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span></span> <span data-ttu-id="c922e-125">Az egyik lehetőség egy HTTP POST kérés kiadása azon a helyen, ahol a lekérdezési paraméterek vannak meghatározva a kéréstörzs JSON-objektumában.</span><span class="sxs-lookup"><span data-stu-id="c922e-125">One way is to issue an HTTP POST request where your query parameters are defined in a JSON object in the request body.</span></span> <span data-ttu-id="c922e-126">A másik lehetőség egy HTTP GET kérés kiadása azon a helyen, ahol a lekérdezési paraméterek vannak meghatározva a kérés URL-címén belül.</span><span class="sxs-lookup"><span data-stu-id="c922e-126">The other way is to issue an HTTP GET request where your query parameters are defined within the request URL.</span></span> <span data-ttu-id="c922e-127">A lekérdezési paraméterek méretének tekintetében a POST több [enyhe korlátozással](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) rendelkezik, mint a GET.</span><span class="sxs-lookup"><span data-stu-id="c922e-127">POST has more [relaxed limits](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) on the size of query parameters than GET.</span></span> <span data-ttu-id="c922e-128">Éppen ezért a POST használatát javasoljuk, hacsak nem állnak fenn olyan speciális körülmények, amelyek a GET használatát kényelmesebbé tennék.</span><span class="sxs-lookup"><span data-stu-id="c922e-128">For this reason, we recommend using POST unless you have special circumstances where using GET would be more convenient.</span></span>

<span data-ttu-id="c922e-129">A POST és a GET esetében egyaránt meg kell majd adnia a *szolgáltatás nevét*, az *index nevét*, valamint a megfelelő *API-verziót* (a jelen dokumentum kiadásakor érvényes API-verzió: `2016-09-01`) a kérés URL-címében.</span><span class="sxs-lookup"><span data-stu-id="c922e-129">For both POST and GET, you need to provide your *service name*, *index name*, and the proper *API version* (the current API version is `2016-09-01` at the time of publishing this document) in the request URL.</span></span> <span data-ttu-id="c922e-130">A GET esetében a lekérdezési paramétereket az URL-cím végén található *lekérdezési karakterláncban* kell megadni.</span><span class="sxs-lookup"><span data-stu-id="c922e-130">For GET, the *query string* at the end of the URL is where you provide the query parameters.</span></span> <span data-ttu-id="c922e-131">Az URL-cím formátuma alább látható:</span><span class="sxs-lookup"><span data-stu-id="c922e-131">See below for the URL format:</span></span>

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2016-09-01

<span data-ttu-id="c922e-132">A POST esetében a formátum ugyanez, azzal a kiegészítéssel, hogy a lekérdezési karakterlánc paraméterei között csak az API-verzió szerepel.</span><span class="sxs-lookup"><span data-stu-id="c922e-132">The format for POST is the same, but with only api-version in the query string parameters.</span></span>

#### <a name="example-queries"></a><span data-ttu-id="c922e-133">Példa a lekérdezésekre</span><span class="sxs-lookup"><span data-stu-id="c922e-133">Example Queries</span></span>
<span data-ttu-id="c922e-134">Alább néhány példa látható a „hotels” nevű index lekérdezéseire.</span><span class="sxs-lookup"><span data-stu-id="c922e-134">Here are a few example queries on an index named "hotels".</span></span> <span data-ttu-id="c922e-135">Ezek a lekérdezések GET- és POST-formátumban is megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="c922e-135">These queries are shown in both GET and POST format.</span></span>

<span data-ttu-id="c922e-136">A teljes indexben keres a „budget” kifejezésre, és csak a `hotelName` mezőt adja vissza:</span><span class="sxs-lookup"><span data-stu-id="c922e-136">Search the entire index for the term 'budget' and return only the `hotelName` field:</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "budget",
    "select": "hotelName"
}
```

<span data-ttu-id="c922e-137">Egy olyan szűrőt alkalmaz az indexen, amely az éjszakánkénti 150 dollárnál olcsóbb szállodákra keres rá, majd visszaadja a `hotelId` és `description` mezőket:</span><span class="sxs-lookup"><span data-stu-id="c922e-137">Apply a filter to the index to find hotels cheaper than $150 per night, and return the `hotelId` and `description`:</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2016-09-01

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

<span data-ttu-id="c922e-138">A teljes indexen végrehajtja a keresést, azt egy adott mező (`lastRenovationDate`) szerint csökkenő sorrendbe rendezi, veszi az első két találatot, majd kizárólag a `hotelName` és `lastRenovationDate` mezőket jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="c922e-138">Search the entire index, order by a specific field (`lastRenovationDate`) in descending order, take the top two results, and show only `hotelName` and `lastRenovationDate`:</span></span>

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

## <a name="submit-your-http-request"></a><span data-ttu-id="c922e-139">A HTTP-kérés elküldése</span><span class="sxs-lookup"><span data-stu-id="c922e-139">Submit your HTTP request</span></span>
<span data-ttu-id="c922e-140">Azt követően, hogy meghatározta a lekérdezést a HTTP-kérés (GET esetében) URL-címének vagy (POST esetében) törzsének részeként, megadhatja a kérésfejléceket, majd elküldheti a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="c922e-140">Now that you have formulated your query as part of your HTTP request URL (for GET) or body (for POST), you can define your request headers and submit your query.</span></span>

#### <a name="request-and-request-headers"></a><span data-ttu-id="c922e-141">Kérés és kérésfejlécek</span><span class="sxs-lookup"><span data-stu-id="c922e-141">Request and Request Headers</span></span>
<span data-ttu-id="c922e-142">A GET esetében kettő, a POST esetében három kérésfejlécet kell meghatároznia:</span><span class="sxs-lookup"><span data-stu-id="c922e-142">You must define two request headers for GET, or three for POST:</span></span>

1. <span data-ttu-id="c922e-143">Az `api-key` fejléc beállításának a fenti I. lépésben található lekérdezési kulcsnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c922e-143">The `api-key` header must be set to the query key you found in step I above.</span></span> <span data-ttu-id="c922e-144">Az `api-key` fejléceként rendszergazdai kulcsot is használhat, javasolt azonban a lekérdezési kulcs használata, mivel az kizárólag csak olvasási hozzáférést biztosít az indexekhez és a dokumentumokhoz.</span><span class="sxs-lookup"><span data-stu-id="c922e-144">You can also use an admin key as the `api-key` header, but it is recommended that you use a query key as it exclusively grants read-only access to indexes and documents.</span></span>
2. <span data-ttu-id="c922e-145">Az `Accept` fejléc beállítása a következő legyen: `application/json`.</span><span class="sxs-lookup"><span data-stu-id="c922e-145">The `Accept` header must be set to `application/json`.</span></span>
3. <span data-ttu-id="c922e-146">Kizárólag a POST kérelem esetében, a `Content-Type` fejléc beállítása szintén a következő legyen: `application/json`.</span><span class="sxs-lookup"><span data-stu-id="c922e-146">For POST only, the `Content-Type` header should also be set to `application/json`.</span></span>

<span data-ttu-id="c922e-147">Az alábbiakban megtekintheti a „hotels” index Azure Search REST API használatával történő kereséséhez tartozó HTTP GET kérést, amely a „motel” kifejezésre egy egyszerű lekérdezéssel keres rá:</span><span class="sxs-lookup"><span data-stu-id="c922e-147">See below for an HTTP GET request to search the "hotels" index using the Azure Search REST API, using a simple query that searches for the term "motel":</span></span>

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2016-09-01
Accept: application/json
api-key: [query key]
```

<span data-ttu-id="c922e-148">Az alábbiakban ugyanazt a példalekérdezést láthatja a HTTP POST használatával:</span><span class="sxs-lookup"><span data-stu-id="c922e-148">Here is the same example query, this time using HTTP POST:</span></span>

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2016-09-01
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

<span data-ttu-id="c922e-149">A sikeres lekérdezési kérés `200 OK` állapotkódot eredményez, a keresési eredmények pedig a kéréstörzsben, JSON-objektum formájában lesznek visszaadva.</span><span class="sxs-lookup"><span data-stu-id="c922e-149">A successful query request will result in a Status Code of `200 OK` and the search results are returned as JSON in the response body.</span></span> <span data-ttu-id="c922e-150">A fenti lekérdezés eredménye a következőképpen fog megjelenni, feltételezve, hogy a „hotels” index az [Adatok importálása az Azure Search szolgáltatásban REST API használatával](search-import-data-rest-api.md) rész mintaadataival van feltöltve (vegye figyelembe, hogy a JSON a jobb áttekinthetőség érdekében formázva van).</span><span class="sxs-lookup"><span data-stu-id="c922e-150">Here is what the results for the above query look like, assuming the "hotels" index is populated with the sample data in [Data Import in Azure Search using the REST API](search-import-data-rest-api.md) (note that the JSON has been formatted for clarity).</span></span>

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

<span data-ttu-id="c922e-151">További segítségért tekintse meg a [Dokumentumok keresése](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) rész „Válasz” szakaszát.</span><span class="sxs-lookup"><span data-stu-id="c922e-151">To learn more, please visit the "Response" section of [Search Documents](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).</span></span> <span data-ttu-id="c922e-152">További információk a meghiúsult műveletek esetében visszaadható HTTP-állapotkódokról: [HTTP-állapotkódok (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span><span class="sxs-lookup"><span data-stu-id="c922e-152">For more information on other HTTP status codes that could be returned in case of failure, see [HTTP status codes (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span></span>
