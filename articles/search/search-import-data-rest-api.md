---
title: "Adatok feltöltése (REST API – Azure Search) | Microsoft Docs"
description: "Megismerkedhet az adatfeltöltéssel az Azure Search szolgáltatás indexébe, a REST API használatával."
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
ms.openlocfilehash: f22a33ed86fbfc46dfa732239263a49f34c4afee
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="upload-data-to-azure-search-using-the-rest-api"></a><span data-ttu-id="03818-103">Adatfeltöltés az Azure Search szolgáltatásba a REST API használatával</span><span class="sxs-lookup"><span data-stu-id="03818-103">Upload data to Azure Search using the REST API</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="03818-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="03818-104">Overview</span></span>](search-what-is-data-import.md)
> * [<span data-ttu-id="03818-105">.NET</span><span class="sxs-lookup"><span data-stu-id="03818-105">.NET</span></span>](search-import-data-dotnet.md)
> * [<span data-ttu-id="03818-106">REST</span><span class="sxs-lookup"><span data-stu-id="03818-106">REST</span></span>](search-import-data-rest-api.md)
>
>

<span data-ttu-id="03818-107">Jelen cikk az Azure Search-indexbe történő adatimportálást mutatja be az [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/) használatával.</span><span class="sxs-lookup"><span data-stu-id="03818-107">This article will show you how to use the [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/) to import data into an Azure Search index.</span></span>

<span data-ttu-id="03818-108">A bemutató elindítása előtt [létre kell hoznia egy Azure Search-indexet](search-what-is-an-index.md).</span><span class="sxs-lookup"><span data-stu-id="03818-108">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md).</span></span>

<span data-ttu-id="03818-109">A dokumentumok REST API használatával az indexbe történő küldéséhez egy HTTP POST kérést fog kiadni az index URL-címének végpontján.</span><span class="sxs-lookup"><span data-stu-id="03818-109">In order to push documents into your index using the REST API, you will issue an HTTP POST request to your index's URL endpoint.</span></span> <span data-ttu-id="03818-110">A HTTP-kérés törzse egy olyan JSON-objektum, amely tartalmazza a hozzáadni, módosítani vagy törölni kívánt dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="03818-110">The body of the HTTP request body is a JSON object containing the documents to be added, modified, or deleted.</span></span>

## <a name="identify-your-azure-search-services-admin-api-key"></a><span data-ttu-id="03818-111">Azonosítsa az Azure Search szolgáltatás rendszergazdai API-kulcsát</span><span class="sxs-lookup"><span data-stu-id="03818-111">Identify your Azure Search service's admin api-key</span></span>
<span data-ttu-id="03818-112">Ha a REST API használatával HTTP-kérések kiadását végzi a szolgáltatáson, *mindegyik* API-kérésnek tartalmaznia kell az Ön által üzembe helyezett Search szolgáltatáshoz létrehozott API-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="03818-112">When issuing HTTP requests against your service using the REST API, *each* API request must include the api-key that was generated for the Search service you provisioned.</span></span> <span data-ttu-id="03818-113">Érvényes kulcs birtokában kérelmenként létesíthető megbízhatósági kapcsolat a kérést küldő alkalmazás és az azt kezelő szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="03818-113">Having a valid key establishes trust, on a per request basis, between the application sending the request and the service that handles it.</span></span>

1. <span data-ttu-id="03818-114">A szolgáltatás API-kulcsainak megkereséséhez bejelentkezhet az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="03818-114">To find your service's api-keys, you can sign in to the [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="03818-115">Nyissa meg az Azure Search szolgáltatáspaneljét</span><span class="sxs-lookup"><span data-stu-id="03818-115">Go to your Azure Search service's blade</span></span>
3. <span data-ttu-id="03818-116">Kattintson a „Kulcsok” ikonra</span><span class="sxs-lookup"><span data-stu-id="03818-116">Click on the "Keys" icon</span></span>

<span data-ttu-id="03818-117">A szolgáltatás *rendszergazdai kulcsokkal* és *lekérdezési kulcsokkal* fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="03818-117">Your service will have *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="03818-118">Az elsődleges és másodlagos *rendszergazdai kulcsok* teljes jogosultságot biztosítanak az összes művelethez, beleértve a szolgáltatás felügyeletének, valamint az indexek, indexelők és adatforrások létrehozásának és törlésének képességét.</span><span class="sxs-lookup"><span data-stu-id="03818-118">Your primary and secondary *admin keys* grant full rights to all operations, including the ability to manage the service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="03818-119">Két kulcs létezi, tehát ha az elsődleges kulcs újbóli létrehozása mellett dönt, a másodlagos kulcsot továbbra is használhatja (ez fordítva is igaz).</span><span class="sxs-lookup"><span data-stu-id="03818-119">There are two keys so that you can continue to use the secondary key if you decide to regenerate the primary key, and vice-versa.</span></span>
* <span data-ttu-id="03818-120">A *lekérdezési kulcsok* csak olvasási hozzáférést biztosítanak az indexekhez és a dokumentumokhoz, és általában a keresési kéréseket kibocsátó ügyfélalkalmazások számára vannak kiosztva.</span><span class="sxs-lookup"><span data-stu-id="03818-120">Your *query keys* grant read-only access to indexes and documents, and are typically distributed to client applications that issue search requests.</span></span>

<span data-ttu-id="03818-121">Egy indexbe történő adatimportáláshoz az elsődleges vagy a másodlagos rendszergazdai kulcsok bármelyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="03818-121">For the purposes of importing data into an index, you can use either your primary or secondary admin key.</span></span>

## <a name="decide-which-indexing-action-to-use"></a><span data-ttu-id="03818-122">A használni kívánt indexelési művelet megadása</span><span class="sxs-lookup"><span data-stu-id="03818-122">Decide which indexing action to use</span></span>
<span data-ttu-id="03818-123">A REST API használatakor JSON-kéréstörzsekkel ellátott HTTP POST kéréseket fog kiadni az Azure Search-index végponti URL-címére.</span><span class="sxs-lookup"><span data-stu-id="03818-123">When using the REST API, you will issue HTTP POST requests with JSON request bodies to your Azure Search index's endpoint URL.</span></span> <span data-ttu-id="03818-124">A HTTP-kéréstörzsben lévő JSON-objektum egyetlen „value” nevű egyedi JSON-tömböt fog tartalmazni. A tömbben lévő JSON-objektumok az indexhez hozzáadni, abban frissíteni vagy abból törölni kívánt dokumentumokat képviselik.</span><span class="sxs-lookup"><span data-stu-id="03818-124">The JSON object in your HTTP request body will contain a single JSON array named "value" containing JSON objects representing documents you would like to add to your index, update, or delete.</span></span>

<span data-ttu-id="03818-125">A „value” tömbben található minden JSON-objektum egy-egy indexelendő dokumentumot képvisel.</span><span class="sxs-lookup"><span data-stu-id="03818-125">Each JSON object in the "value" array represents a document to be indexed.</span></span> <span data-ttu-id="03818-126">Ezen objektumok mindegyike tartalmazza a dokumentum kulcsát, valamint megadja a végrehajtani kívánt indexelési műveletet (például feltöltés, egyesítés vagy törlés).</span><span class="sxs-lookup"><span data-stu-id="03818-126">Each of these objects contains the document's key and specifies the desired indexing action (upload, merge, delete, etc).</span></span> <span data-ttu-id="03818-127">Attól függően, hogy az alábbi műveletek közül melyiket választja ki, az egyes dokumentumok esetében csak bizonyos mezők lesznek kötelezően megjelenítendők:</span><span class="sxs-lookup"><span data-stu-id="03818-127">Depending on which of the below actions you choose, only certain fields must be included for each document:</span></span>

| @search.action | <span data-ttu-id="03818-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="03818-128">Description</span></span> | <span data-ttu-id="03818-129">Az egyes dokumentumok kötelező mezői</span><span class="sxs-lookup"><span data-stu-id="03818-129">Necessary fields for each document</span></span> | <span data-ttu-id="03818-130">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="03818-130">Notes</span></span> |
| --- | --- | --- | --- |
| `upload` |<span data-ttu-id="03818-131">Az `upload` művelet működése hasonló az „upsert” (frissítés/beszúrás) műveletéhez, ahol a rendszer az új dokumentumot beilleszti, ha pedig már létező dokumentumról van szó, akkor frissíti/kicseréli azt.</span><span class="sxs-lookup"><span data-stu-id="03818-131">An `upload` action is similar to an "upsert" where the document will be inserted if it is new and updated/replaced if it exists.</span></span> |<span data-ttu-id="03818-132">billentyű, továbbá a meghatározni kívánt egyéb mezők</span><span class="sxs-lookup"><span data-stu-id="03818-132">key, plus any other fields you wish to define</span></span> |<span data-ttu-id="03818-133">Létező dokumentum frissítése/cseréje esetén a kérésben nem megadott mezők beállítása a következő lesz: `null`.</span><span class="sxs-lookup"><span data-stu-id="03818-133">When updating/replacing an existing document, any field that is not specified in the request will have its field set to `null`.</span></span> <span data-ttu-id="03818-134">Ez történik abban az esetben is, ha a mező korábban nem null értékre lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="03818-134">This occurs even when the field was previously set to a non-null value.</span></span> |
| `merge` |<span data-ttu-id="03818-135">Egy meglévő dokumentumot frissít a megadott mezőkkel.</span><span class="sxs-lookup"><span data-stu-id="03818-135">Updates an existing document with the specified fields.</span></span> <span data-ttu-id="03818-136">Ha a dokumentum nem található az indexben, az egyesítés meg fog hiúsulni.</span><span class="sxs-lookup"><span data-stu-id="03818-136">If the document does not exist in the index, the merge will fail.</span></span> |<span data-ttu-id="03818-137">billentyű, továbbá a meghatározni kívánt egyéb mezők</span><span class="sxs-lookup"><span data-stu-id="03818-137">key, plus any other fields you wish to define</span></span> |<span data-ttu-id="03818-138">A rendszer az egyesítési művelet során megadott mezőkre cseréli a dokumentum meglévő mezőit.</span><span class="sxs-lookup"><span data-stu-id="03818-138">Any field you specify in a merge will replace the existing field in the document.</span></span> <span data-ttu-id="03818-139">Ez `Collection(Edm.String)` típusú mezőket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="03818-139">This includes fields of type `Collection(Edm.String)`.</span></span> <span data-ttu-id="03818-140">Ha például a dokumentum egy `["budget"]` értékű `tags` mezőt tartalmaz, és egyesítést hajt végre a `tags` mező `["economy", "pool"]` értékével, a `tags` mező végső értéke `["economy", "pool"]` lesz.</span><span class="sxs-lookup"><span data-stu-id="03818-140">For example, if the document contains a field `tags` with value `["budget"]` and you execute a merge with value `["economy", "pool"]` for `tags`, the final value of the `tags` field will be `["economy", "pool"]`.</span></span> <span data-ttu-id="03818-141">Nem pedig a következő lesz: `["budget", "economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="03818-141">It will not be `["budget", "economy", "pool"]`.</span></span> |
| `mergeOrUpload` |<span data-ttu-id="03818-142">Ha az indexben már létezik az adott kulccsal ellátott dokumentum, ezen művelet viselkedése hasonló lesz a `merge` műveletéhez.</span><span class="sxs-lookup"><span data-stu-id="03818-142">This action behaves like `merge` if a document with the given key already exists in the index.</span></span> <span data-ttu-id="03818-143">Ha nem létezik ilyen dokumentum, a művelet viselkedése az `upload` új dokumentum esetében mutatott viselkedésének fog megfelelni.</span><span class="sxs-lookup"><span data-stu-id="03818-143">If the document does not exist, it behaves like `upload` with a new document.</span></span> |<span data-ttu-id="03818-144">billentyű, továbbá a meghatározni kívánt egyéb mezők</span><span class="sxs-lookup"><span data-stu-id="03818-144">key, plus any other fields you wish to define</span></span> |- |
| `delete` |<span data-ttu-id="03818-145">Eltávolítja a megadott dokumentumot az indexből.</span><span class="sxs-lookup"><span data-stu-id="03818-145">Removes the specified document from the index.</span></span> |<span data-ttu-id="03818-146">csak billentyű</span><span class="sxs-lookup"><span data-stu-id="03818-146">key only</span></span> |<span data-ttu-id="03818-147">A rendszer figyelmen kívül hagyja a kulcsmezőn kívül megadott mezőket.</span><span class="sxs-lookup"><span data-stu-id="03818-147">Any fields you specify other than the key field will be ignored.</span></span> <span data-ttu-id="03818-148">Ha egyetlen mezőt kíván eltávolítani a dokumentumból, e helyett használja a `merge` műveletet, és a mező számára explicit módon adja meg a null értéket.</span><span class="sxs-lookup"><span data-stu-id="03818-148">If you want to remove an individual field from a document, use `merge` instead and simply set the field explicitly to null.</span></span> |

## <a name="construct-your-http-request-and-request-body"></a><span data-ttu-id="03818-149">A HTTP-kérés és a kérés törzsének létrehozása</span><span class="sxs-lookup"><span data-stu-id="03818-149">Construct your HTTP request and request body</span></span>
<span data-ttu-id="03818-150">Most, hogy összegyűjtötte az indexelési műveletekhez szükséges mezők értékeit, készen áll a tulajdonképpeni HTTP-kérés és a JSON-kérés törzsének létrehozására az adatok importálásához.</span><span class="sxs-lookup"><span data-stu-id="03818-150">Now that you have gathered the necessary field values for your index actions, you are ready to construct the actual HTTP request and JSON request body to import your data.</span></span>

#### <a name="request-and-request-headers"></a><span data-ttu-id="03818-151">Kérés és kérésfejlécek</span><span class="sxs-lookup"><span data-stu-id="03818-151">Request and Request Headers</span></span>
<span data-ttu-id="03818-152">Az URL-címben meg kell majd adnia a szolgáltatás nevét, az index nevét (ami ebben az esetben „hotels”), valamint a megfelelő API-verziót (a jelen dokumentum kiadásakor érvényes API-verzió: `2016-09-01`).</span><span class="sxs-lookup"><span data-stu-id="03818-152">In the URL, you will need to provide your service name, index name ("hotels" in this case), as well as the proper API version (the current API version is `2016-09-01` at the time of publishing this document).</span></span> <span data-ttu-id="03818-153">Meg kell határoznia a `Content-Type` és `api-key` kérésfejléceket is.</span><span class="sxs-lookup"><span data-stu-id="03818-153">You will need to define the `Content-Type` and `api-key` request headers.</span></span> <span data-ttu-id="03818-154">Az utóbbi esetében használja a szolgáltatás rendszergazdai kulcsainak egyikét.</span><span class="sxs-lookup"><span data-stu-id="03818-154">For the latter, use one of your service's admin keys.</span></span>

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a><span data-ttu-id="03818-155">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="03818-155">Request Body</span></span>
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
            "description": "Close to town hall and the river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

<span data-ttu-id="03818-156">Ebben az esetben a következő keresési műveleteket használjuk: `upload`, `mergeOrUpload` és `delete`.</span><span class="sxs-lookup"><span data-stu-id="03818-156">In this case, we are using `upload`, `mergeOrUpload`, and `delete` as our search actions.</span></span>

<span data-ttu-id="03818-157">Jelen példában feltételezzük, hogy a „hotels” index már fel van töltve dokumentumokkal.</span><span class="sxs-lookup"><span data-stu-id="03818-157">Assume that this example "hotels" index is already populated with a number of documents.</span></span> <span data-ttu-id="03818-158">Figyelje meg, hogy az `mergeOrUpload` használatakor nem volt szükséges megadni a dokumentumban szereplő összes mezőt, illetve hogy a `delete` használatakor kizárólag a dokumentumkulcsot (`hotelId`) adtuk meg.</span><span class="sxs-lookup"><span data-stu-id="03818-158">Note how we did not have to specify all the possible document fields when using `mergeOrUpload` and how we only specified the document key (`hotelId`) when using `delete`.</span></span>

<span data-ttu-id="03818-159">Vegye figyelembe azt is, hogy egyetlen indexelési kérésbe legfeljebb 1000 dokumentumot (vagy 16 MB adatmennyiséget) foglalhat.</span><span class="sxs-lookup"><span data-stu-id="03818-159">Also, note that you can only include up to 1000 documents (or 16 MB) in a single indexing request.</span></span>

## <a name="understand-your-http-response-code"></a><span data-ttu-id="03818-160">A HTTP válaszkód ismertetése</span><span class="sxs-lookup"><span data-stu-id="03818-160">Understand your HTTP response code</span></span>
#### <a name="200"></a><span data-ttu-id="03818-161">200</span><span class="sxs-lookup"><span data-stu-id="03818-161">200</span></span>
<span data-ttu-id="03818-162">Az indexelési kérés sikeres elküldését követően HTTP-választ fog kapni a következő állapotkóddal: `200 OK`.</span><span class="sxs-lookup"><span data-stu-id="03818-162">After submitting a successful indexing request you will receive an HTTP response with status code of `200 OK`.</span></span> <span data-ttu-id="03818-163">A HTTP-válasz JSON-törzse a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="03818-163">The JSON body of the HTTP response will be as follows:</span></span>

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

#### <a name="207"></a><span data-ttu-id="03818-164">207</span><span class="sxs-lookup"><span data-stu-id="03818-164">207</span></span>
<span data-ttu-id="03818-165">Ha az indexelés legalább egy elem esetében meghiúsult, a rendszer a következő állapotkódot fogja visszaadni: `207`.</span><span class="sxs-lookup"><span data-stu-id="03818-165">A status code of `207` will be returned when at least one item was not successfully indexed.</span></span> <span data-ttu-id="03818-166">A HTTP-válasz JSON-törzse fogja tartalmazni a sikertelen indexelésű dokumentum(ok) adatait.</span><span class="sxs-lookup"><span data-stu-id="03818-166">The JSON body of the HTTP response will contain information about the unsuccessful document(s).</span></span>

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "The search service is too busy to process this document. Please try again later."
        },
        ...
    ]
}
```

> [!NOTE]
> <span data-ttu-id="03818-167">Ez leggyakrabban azt jelenti, hogy a Search szolgáltatás terhelése elérte azt a pontot, amelytől kezdve az indexelési kérések `503` válaszokat adnak vissza.</span><span class="sxs-lookup"><span data-stu-id="03818-167">This often means that the load on your search service is reaching a point where indexing requests will begin to return `503` responses.</span></span> <span data-ttu-id="03818-168">Ebben az esetben határozottan javasoljuk az ügyfélkód visszahívását és az újrapróbálkozás előtt egy kis várakozást.</span><span class="sxs-lookup"><span data-stu-id="03818-168">In this case, we highly recommend that your client code back off and wait before retrying.</span></span> <span data-ttu-id="03818-169">Ezzel a rendszer számára időt ad a helyreállításra, így a jövőbeni kérések nagyobb eséllyel lesznek sikeresek.</span><span class="sxs-lookup"><span data-stu-id="03818-169">This will give the system some time to recover, increasing the chances that future requests will succeed.</span></span> <span data-ttu-id="03818-170">A gyors újrapróbálkozásokkal csupán az adott szituációt állandósítja.</span><span class="sxs-lookup"><span data-stu-id="03818-170">Rapidly retrying your requests will only prolong the situation.</span></span>
>
>

#### <a name="429"></a><span data-ttu-id="03818-171">429</span><span class="sxs-lookup"><span data-stu-id="03818-171">429</span></span>
<span data-ttu-id="03818-172">Az indexenkénti dokumentumszám-kvóta túllépésekor a rendszer a következő állapotkódot fogja visszaadni: `429`.</span><span class="sxs-lookup"><span data-stu-id="03818-172">A status code of `429` will be returned when you have exceeded your quota on the number of documents per index.</span></span>

#### <a name="503"></a><span data-ttu-id="03818-173">503</span><span class="sxs-lookup"><span data-stu-id="03818-173">503</span></span>
<span data-ttu-id="03818-174">Ha az indexelés a kérésben szereplő összes elem esetében meghiúsult, a rendszer a következő állapotkódot fogja visszaadni: `503`.</span><span class="sxs-lookup"><span data-stu-id="03818-174">A status code of `503` will be returned if none of the items in the request were successfully indexed.</span></span> <span data-ttu-id="03818-175">Ez a hibaüzenet azt jelzi, hogy a rendszer terhelése nagy, és a kérés jelenleg nem dolgozható fel.</span><span class="sxs-lookup"><span data-stu-id="03818-175">This error means that the system is under heavy load and your request can't be processed at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="03818-176">Ebben az esetben határozottan javasoljuk az ügyfélkód visszahívását és az újrapróbálkozás előtt egy kis várakozást.</span><span class="sxs-lookup"><span data-stu-id="03818-176">In this case, we highly recommend that your client code back off and wait before retrying.</span></span> <span data-ttu-id="03818-177">Ezzel a rendszer számára időt ad a helyreállításra, így a jövőbeni kérések nagyobb eséllyel lesznek sikeresek.</span><span class="sxs-lookup"><span data-stu-id="03818-177">This will give the system some time to recover, increasing the chances that future requests will succeed.</span></span> <span data-ttu-id="03818-178">A gyors újrapróbálkozásokkal csupán az adott szituációt állandósítja.</span><span class="sxs-lookup"><span data-stu-id="03818-178">Rapidly retrying your requests will only prolong the situation.</span></span>
>
>

<span data-ttu-id="03818-179">További információk a dokumentumokkal végzett műveletekről, illetve a sikeres/meghiúsult műveletekre adott rendszerválaszokról: [Dokumentumok hozzáadása, frissítése vagy törlése](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents).</span><span class="sxs-lookup"><span data-stu-id="03818-179">For more information on document actions and success/error responses, please see [Add, Update, or Delete Documents](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents).</span></span> <span data-ttu-id="03818-180">További információk a meghiúsult műveletek esetében visszaadható HTTP-állapotkódokról: [HTTP-állapotkódok (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span><span class="sxs-lookup"><span data-stu-id="03818-180">For more information on other HTTP status codes that could be returned in case of failure, see [HTTP status codes (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span></span>

## <a name="next-steps"></a><span data-ttu-id="03818-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="03818-181">Next steps</span></span>
<span data-ttu-id="03818-182">Az Azure Search-index feltöltését követően készen áll a dokumentumkeresési lekérdezések kiadásának elindítására.</span><span class="sxs-lookup"><span data-stu-id="03818-182">After populating your Azure Search index, you will be ready to start issuing queries to search for documents.</span></span> <span data-ttu-id="03818-183">Részletes információk: [Az Azure Search-index lekérdezése](search-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="03818-183">See [Query Your Azure Search Index](search-query-overview.md) for details.</span></span>
