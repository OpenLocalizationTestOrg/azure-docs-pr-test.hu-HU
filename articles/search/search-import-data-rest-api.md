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
# <a name="upload-data-tooazure-search-using-hello-rest-api"></a><span data-ttu-id="d516f-103">Feltöltés tooAzure keresési használatával végzett hello REST API-n</span><span class="sxs-lookup"><span data-stu-id="d516f-103">Upload data tooAzure Search using hello REST API</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="d516f-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="d516f-104">Overview</span></span>](search-what-is-data-import.md)
> * [<span data-ttu-id="d516f-105">.NET</span><span class="sxs-lookup"><span data-stu-id="d516f-105">.NET</span></span>](search-import-data-dotnet.md)
> * [<span data-ttu-id="d516f-106">REST</span><span class="sxs-lookup"><span data-stu-id="d516f-106">REST</span></span>](search-import-data-rest-api.md)
>
>

<span data-ttu-id="d516f-107">Ez a cikk bemutatja, hogyan toouse hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/) tooimport adatokat az Azure Search-index.</span><span class="sxs-lookup"><span data-stu-id="d516f-107">This article will show you how toouse hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/) tooimport data into an Azure Search index.</span></span>

<span data-ttu-id="d516f-108">A bemutató elindítása előtt [létre kell hoznia egy Azure Search-indexet](search-what-is-an-index.md).</span><span class="sxs-lookup"><span data-stu-id="d516f-108">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md).</span></span>

<span data-ttu-id="d516f-109">Az rendelések toopush hello REST API használatával az indexbe egy HTTP POST kérelem tooyour index URL-végpontjának állít ki.</span><span class="sxs-lookup"><span data-stu-id="d516f-109">In order toopush documents into your index using hello REST API, you will issue an HTTP POST request tooyour index's URL endpoint.</span></span> <span data-ttu-id="d516f-110">HTTP-kérelem törzse hello dokumentumokat toobe hozzáadott, módosított és törölt tartalmazó JSON-objektum hello hello törzsét.</span><span class="sxs-lookup"><span data-stu-id="d516f-110">hello body of hello HTTP request body is a JSON object containing hello documents toobe added, modified, or deleted.</span></span>

## <a name="identify-your-azure-search-services-admin-api-key"></a><span data-ttu-id="d516f-111">Azonosítsa az Azure Search szolgáltatás rendszergazdai API-kulcsát</span><span class="sxs-lookup"><span data-stu-id="d516f-111">Identify your Azure Search service's admin api-key</span></span>
<span data-ttu-id="d516f-112">HTTP-kérelmeket küldhet a REST API-t hello szolgáltatást kiállításához *minden* API-kérelem hello api-kulcsot hello keresőszolgáltatáshoz generált tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="d516f-112">When issuing HTTP requests against your service using hello REST API, *each* API request must include hello api-key that was generated for hello Search service you provisioned.</span></span> <span data-ttu-id="d516f-113">Érvényes kulcs birtokában létesít megbízhatósági, egy kérelem alapon hello küldő hello kérelem és a kezelő hello szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="d516f-113">Having a valid key establishes trust, on a per request basis, between hello application sending hello request and hello service that handles it.</span></span>

1. <span data-ttu-id="d516f-114">toofind a szolgáltatás api-kulcsokat, bármikor beléphet toohello [Azure-portálon](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="d516f-114">toofind your service's api-keys, you can sign in toohello [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="d516f-115">Nyissa meg tooyour Azure Search szolgáltatás paneljét</span><span class="sxs-lookup"><span data-stu-id="d516f-115">Go tooyour Azure Search service's blade</span></span>
3. <span data-ttu-id="d516f-116">Kattintson a hello "Kulcsok" ikonra</span><span class="sxs-lookup"><span data-stu-id="d516f-116">Click on hello "Keys" icon</span></span>

<span data-ttu-id="d516f-117">A szolgáltatás *rendszergazdai kulcsokkal* és *lekérdezési kulcsokkal* fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="d516f-117">Your service will have *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="d516f-118">Az elsődleges és másodlagos *adminisztrációs kulcsok* teljes körű tooall műveleteket, köztük a hello képességét toomanage hello szolgáltatást biztosítania hozzon létre, és törölje az indexek, az indexelők és az adatforrások.</span><span class="sxs-lookup"><span data-stu-id="d516f-118">Your primary and secondary *admin keys* grant full rights tooall operations, including hello ability toomanage hello service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="d516f-119">Két kulcs van, hogy a Folytatás toouse hello másodlagos kulcsát. Ha úgy dönt, hogy tooregenerate hello elsődleges kulcs, és fordítva.</span><span class="sxs-lookup"><span data-stu-id="d516f-119">There are two keys so that you can continue toouse hello secondary key if you decide tooregenerate hello primary key, and vice-versa.</span></span>
* <span data-ttu-id="d516f-120">A *lekérdezési kulcsok* adjon olvasási hozzáférést tooindexes és a dokumentumok és keresési kérelmeket kibocsátó általában elosztott tooclient alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="d516f-120">Your *query keys* grant read-only access tooindexes and documents, and are typically distributed tooclient applications that issue search requests.</span></span>

<span data-ttu-id="d516f-121">Az adatok importálása egy index hello célokra is használhatja az elsődleges vagy másodlagos adminisztrátori kulcsot.</span><span class="sxs-lookup"><span data-stu-id="d516f-121">For hello purposes of importing data into an index, you can use either your primary or secondary admin key.</span></span>

## <a name="decide-which-indexing-action-toouse"></a><span data-ttu-id="d516f-122">Döntse el, melyik indexelési művelet toouse</span><span class="sxs-lookup"><span data-stu-id="d516f-122">Decide which indexing action toouse</span></span>
<span data-ttu-id="d516f-123">Hello REST API használata esetén a HTTP POST-kérésnél állít ki a JSON kérelem szervek tooyour Azure Search-index a végpont URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="d516f-123">When using hello REST API, you will issue HTTP POST requests with JSON request bodies tooyour Azure Search index's endpoint URL.</span></span> <span data-ttu-id="d516f-124">a HTTP-kérés törzsében hello JSON-objektumot fog tartalmazni egyetlen JSON-tömb nevű, "érték" tartalmazó dokumentumok tooadd tooyour index milyen képviselő JSON-objektumok, frissítése vagy törlése.</span><span class="sxs-lookup"><span data-stu-id="d516f-124">hello JSON object in your HTTP request body will contain a single JSON array named "value" containing JSON objects representing documents you would like tooadd tooyour index, update, or delete.</span></span>

<span data-ttu-id="d516f-125">Minden egyes hello "érték" tömb JSON-objektum egy dokumentum toobe indexelt jelöli.</span><span class="sxs-lookup"><span data-stu-id="d516f-125">Each JSON object in hello "value" array represents a document toobe indexed.</span></span> <span data-ttu-id="d516f-126">Az objektumok hello dokumentum kulcsot tartalmaz, és adja meg a szükséges hello indexelési művelet (feltöltési, egyesítési, törlés, stb.).</span><span class="sxs-lookup"><span data-stu-id="d516f-126">Each of these objects contains hello document's key and specifies hello desired indexing action (upload, merge, delete, etc).</span></span> <span data-ttu-id="d516f-127">Attól függően, amely alatt úgy dönt, műveletek hello csak bizonyos mezők szerepelnie kell függvénykötésnek nyilvántartott egyes dokumentumok:</span><span class="sxs-lookup"><span data-stu-id="d516f-127">Depending on which of hello below actions you choose, only certain fields must be included for each document:</span></span>

| @search.action | <span data-ttu-id="d516f-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="d516f-128">Description</span></span> | <span data-ttu-id="d516f-129">Az egyes dokumentumok kötelező mezői</span><span class="sxs-lookup"><span data-stu-id="d516f-129">Necessary fields for each document</span></span> | <span data-ttu-id="d516f-130">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d516f-130">Notes</span></span> |
| --- | --- | --- | --- |
| `upload` |<span data-ttu-id="d516f-131">Egy `upload` művelete hasonló tooan "upsert", ahol hello dokumentum lesz beszúrni, ha új, majd frissíteni vagy lecserélni rá.</span><span class="sxs-lookup"><span data-stu-id="d516f-131">An `upload` action is similar tooan "upsert" where hello document will be inserted if it is new and updated/replaced if it exists.</span></span> |<span data-ttu-id="d516f-132">billentyűt, és bármely más mezők toodefine kívánja</span><span class="sxs-lookup"><span data-stu-id="d516f-132">key, plus any other fields you wish toodefine</span></span> |<span data-ttu-id="d516f-133">Ha egy meglévő dokumentum frissítése vagy cseréje, bármely hello kérelemben nincs megadva mező rendelkezik-e a mező értéke túl`null`.</span><span class="sxs-lookup"><span data-stu-id="d516f-133">When updating/replacing an existing document, any field that is not specified in hello request will have its field set too`null`.</span></span> <span data-ttu-id="d516f-134">Ez akkor fordul elő, akkor is, ha hello mező korábban megadott tooa értéke nem lehet null.</span><span class="sxs-lookup"><span data-stu-id="d516f-134">This occurs even when hello field was previously set tooa non-null value.</span></span> |
| `merge` |<span data-ttu-id="d516f-135">Egy meglévő dokumentum hello található frissítések megadott mezőket.</span><span class="sxs-lookup"><span data-stu-id="d516f-135">Updates an existing document with hello specified fields.</span></span> <span data-ttu-id="d516f-136">Ha hello dokumentum hello index nem létezik, hello egyesítés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="d516f-136">If hello document does not exist in hello index, hello merge will fail.</span></span> |<span data-ttu-id="d516f-137">billentyűt, és bármely más mezők toodefine kívánja</span><span class="sxs-lookup"><span data-stu-id="d516f-137">key, plus any other fields you wish toodefine</span></span> |<span data-ttu-id="d516f-138">Bármely egyesítésével megadott mező felül fogja írni a létező mező hello hello dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="d516f-138">Any field you specify in a merge will replace hello existing field in hello document.</span></span> <span data-ttu-id="d516f-139">Ez `Collection(Edm.String)` típusú mezőket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d516f-139">This includes fields of type `Collection(Edm.String)`.</span></span> <span data-ttu-id="d516f-140">Például akkor, ha hello dokumentum tartalmaz egy mező `tags` értékű `["budget"]` és értékkel egyesítésével végrehajtása `["economy", "pool"]` a `tags`, végső értéke hello hello `tags` mező kitöltése `["economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="d516f-140">For example, if hello document contains a field `tags` with value `["budget"]` and you execute a merge with value `["economy", "pool"]` for `tags`, hello final value of hello `tags` field will be `["economy", "pool"]`.</span></span> <span data-ttu-id="d516f-141">Nem pedig a következő lesz: `["budget", "economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="d516f-141">It will not be `["budget", "economy", "pool"]`.</span></span> |
| `mergeOrUpload` |<span data-ttu-id="d516f-142">Ez a művelet viselkedik `merge` Ha hello kulcs már megadott dokumentum hello index már létezik.</span><span class="sxs-lookup"><span data-stu-id="d516f-142">This action behaves like `merge` if a document with hello given key already exists in hello index.</span></span> <span data-ttu-id="d516f-143">Ha hello dokumentum nem létezik, hasonlóan viselkedik `upload` egy új dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="d516f-143">If hello document does not exist, it behaves like `upload` with a new document.</span></span> |<span data-ttu-id="d516f-144">billentyűt, és bármely más mezők toodefine kívánja</span><span class="sxs-lookup"><span data-stu-id="d516f-144">key, plus any other fields you wish toodefine</span></span> |- |
| `delete` |<span data-ttu-id="d516f-145">Hello megadott dokumentum eltávolítása hello index.</span><span class="sxs-lookup"><span data-stu-id="d516f-145">Removes hello specified document from hello index.</span></span> |<span data-ttu-id="d516f-146">csak billentyű</span><span class="sxs-lookup"><span data-stu-id="d516f-146">key only</span></span> |<span data-ttu-id="d516f-147">A mezőket, akkor adjon meg eltérő hello kulcsmező figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="d516f-147">Any fields you specify other than hello key field will be ignored.</span></span> <span data-ttu-id="d516f-148">Ha azt szeretné, hogy a dokumentum egy egyéni mező tooremove, `merge` helyette és egyszerűen állítsa be az hello mezőt explicit módon toonull.</span><span class="sxs-lookup"><span data-stu-id="d516f-148">If you want tooremove an individual field from a document, use `merge` instead and simply set hello field explicitly toonull.</span></span> |

## <a name="construct-your-http-request-and-request-body"></a><span data-ttu-id="d516f-149">A HTTP-kérés és a kérés törzsének létrehozása</span><span class="sxs-lookup"><span data-stu-id="d516f-149">Construct your HTTP request and request body</span></span>
<span data-ttu-id="d516f-150">Most, hogy az index műveletekhez szükséges mezőértékek hello összegyűjtötte, készen áll a tooconstruct hello tényleges HTTP-kérelem és a JSON kérelem törzse tooimport adatait.</span><span class="sxs-lookup"><span data-stu-id="d516f-150">Now that you have gathered hello necessary field values for your index actions, you are ready tooconstruct hello actual HTTP request and JSON request body tooimport your data.</span></span>

#### <a name="request-and-request-headers"></a><span data-ttu-id="d516f-151">Kérés és kérésfejlécek</span><span class="sxs-lookup"><span data-stu-id="d516f-151">Request and Request Headers</span></span>
<span data-ttu-id="d516f-152">Hello URL-címében, szüksége lesz tooprovide a szolgáltatás nevét, index neve ("Hotels" nevű ebben az esetben), valamint hello megfelelő API-verzió (hello aktuális API-verzió `2016-09-01` dokumentum közzétételének hello időben).</span><span class="sxs-lookup"><span data-stu-id="d516f-152">In hello URL, you will need tooprovide your service name, index name ("hotels" in this case), as well as hello proper API version (hello current API version is `2016-09-01` at hello time of publishing this document).</span></span> <span data-ttu-id="d516f-153">Szüksége lesz a toodefine hello `Content-Type` és `api-key` kérelem fejlécei.</span><span class="sxs-lookup"><span data-stu-id="d516f-153">You will need toodefine hello `Content-Type` and `api-key` request headers.</span></span> <span data-ttu-id="d516f-154">Az utóbbi hello használja a szolgáltatás adminisztrációs kulcsok egyikét.</span><span class="sxs-lookup"><span data-stu-id="d516f-154">For hello latter, use one of your service's admin keys.</span></span>

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a><span data-ttu-id="d516f-155">A kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="d516f-155">Request Body</span></span>
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

<span data-ttu-id="d516f-156">Ebben az esetben a következő keresési műveleteket használjuk: `upload`, `mergeOrUpload` és `delete`.</span><span class="sxs-lookup"><span data-stu-id="d516f-156">In this case, we are using `upload`, `mergeOrUpload`, and `delete` as our search actions.</span></span>

<span data-ttu-id="d516f-157">Jelen példában feltételezzük, hogy a „hotels” index már fel van töltve dokumentumokkal.</span><span class="sxs-lookup"><span data-stu-id="d516f-157">Assume that this example "hotels" index is already populated with a number of documents.</span></span> <span data-ttu-id="d516f-158">Vegye figyelembe, hogyan jelenleg nem rendelkezett toospecify minden hello lehetséges dokumentum mező használata esetén `mergeOrUpload` , és hogyan azt csak meghatározott hello dokumentum kulcs (`hotelId`) használatakor `delete`.</span><span class="sxs-lookup"><span data-stu-id="d516f-158">Note how we did not have toospecify all hello possible document fields when using `mergeOrUpload` and how we only specified hello document key (`hotelId`) when using `delete`.</span></span>

<span data-ttu-id="d516f-159">Emellett vegye figyelembe, hogy csak akkor szerepelhet too1000 dokumentumok (vagy 16 MB) egyetlen indexelési kérelemben.</span><span class="sxs-lookup"><span data-stu-id="d516f-159">Also, note that you can only include up too1000 documents (or 16 MB) in a single indexing request.</span></span>

## <a name="understand-your-http-response-code"></a><span data-ttu-id="d516f-160">A HTTP válaszkód ismertetése</span><span class="sxs-lookup"><span data-stu-id="d516f-160">Understand your HTTP response code</span></span>
#### <a name="200"></a><span data-ttu-id="d516f-161">200</span><span class="sxs-lookup"><span data-stu-id="d516f-161">200</span></span>
<span data-ttu-id="d516f-162">Az indexelési kérés sikeres elküldését követően HTTP-választ fog kapni a következő állapotkóddal: `200 OK`.</span><span class="sxs-lookup"><span data-stu-id="d516f-162">After submitting a successful indexing request you will receive an HTTP response with status code of `200 OK`.</span></span> <span data-ttu-id="d516f-163">hello hello HTTP-válasz törzsében JSON a következők:</span><span class="sxs-lookup"><span data-stu-id="d516f-163">hello JSON body of hello HTTP response will be as follows:</span></span>

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

#### <a name="207"></a><span data-ttu-id="d516f-164">207</span><span class="sxs-lookup"><span data-stu-id="d516f-164">207</span></span>
<span data-ttu-id="d516f-165">Ha az indexelés legalább egy elem esetében meghiúsult, a rendszer a következő állapotkódot fogja visszaadni: `207`.</span><span class="sxs-lookup"><span data-stu-id="d516f-165">A status code of `207` will be returned when at least one item was not successfully indexed.</span></span> <span data-ttu-id="d516f-166">hello hello HTTP-válasz törzsében JSON hello sikertelen dokumentum információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="d516f-166">hello JSON body of hello HTTP response will contain information about hello unsuccessful document(s).</span></span>

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
> <span data-ttu-id="d516f-167">Ez gyakran azt jelenti, hogy hello betöltése a Search szolgáltatás közel jár a pont, ahol kérelmek indexelő megkezdődik tooreturn `503` válaszokat.</span><span class="sxs-lookup"><span data-stu-id="d516f-167">This often means that hello load on your search service is reaching a point where indexing requests will begin tooreturn `503` responses.</span></span> <span data-ttu-id="d516f-168">Ebben az esetben határozottan javasoljuk az ügyfélkód visszahívását és az újrapróbálkozás előtt egy kis várakozást.</span><span class="sxs-lookup"><span data-stu-id="d516f-168">In this case, we highly recommend that your client code back off and wait before retrying.</span></span> <span data-ttu-id="d516f-169">Ekkor kap hello rendszer bizonyos idő toorecover növelése hello esélyét, hogy a későbbi kérelmek sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="d516f-169">This will give hello system some time toorecover, increasing hello chances that future requests will succeed.</span></span> <span data-ttu-id="d516f-170">A kérelmek gyorsan újrapróbálkozás csak meghosszabbítása hello helyzet.</span><span class="sxs-lookup"><span data-stu-id="d516f-170">Rapidly retrying your requests will only prolong hello situation.</span></span>
>
>

#### <a name="429"></a><span data-ttu-id="d516f-171">429</span><span class="sxs-lookup"><span data-stu-id="d516f-171">429</span></span>
<span data-ttu-id="d516f-172">Az állapotkódot `429` során túllépte a kvótát a dokumentumok / index hello száma adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d516f-172">A status code of `429` will be returned when you have exceeded your quota on hello number of documents per index.</span></span>

#### <a name="503"></a><span data-ttu-id="d516f-173">503</span><span class="sxs-lookup"><span data-stu-id="d516f-173">503</span></span>
<span data-ttu-id="d516f-174">Az állapotkódot `503` Ha hello kérelem hello elemek egyike sikeresen indexelt eredmény.</span><span class="sxs-lookup"><span data-stu-id="d516f-174">A status code of `503` will be returned if none of hello items in hello request were successfully indexed.</span></span> <span data-ttu-id="d516f-175">Ez a hiba azt jelenti, hogy, hogy hello rendszer nagy terhelésnek van kitéve, és jelenleg nem lehet feldolgozni a kérését.</span><span class="sxs-lookup"><span data-stu-id="d516f-175">This error means that hello system is under heavy load and your request can't be processed at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="d516f-176">Ebben az esetben határozottan javasoljuk az ügyfélkód visszahívását és az újrapróbálkozás előtt egy kis várakozást.</span><span class="sxs-lookup"><span data-stu-id="d516f-176">In this case, we highly recommend that your client code back off and wait before retrying.</span></span> <span data-ttu-id="d516f-177">Ekkor kap hello rendszer bizonyos idő toorecover növelése hello esélyét, hogy a későbbi kérelmek sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="d516f-177">This will give hello system some time toorecover, increasing hello chances that future requests will succeed.</span></span> <span data-ttu-id="d516f-178">A kérelmek gyorsan újrapróbálkozás csak meghosszabbítása hello helyzet.</span><span class="sxs-lookup"><span data-stu-id="d516f-178">Rapidly retrying your requests will only prolong hello situation.</span></span>
>
>

<span data-ttu-id="d516f-179">További információk a dokumentumokkal végzett műveletekről, illetve a sikeres/meghiúsult műveletekre adott rendszerválaszokról: [Dokumentumok hozzáadása, frissítése vagy törlése](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents).</span><span class="sxs-lookup"><span data-stu-id="d516f-179">For more information on document actions and success/error responses, please see [Add, Update, or Delete Documents](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents).</span></span> <span data-ttu-id="d516f-180">További információk a meghiúsult műveletek esetében visszaadható HTTP-állapotkódokról: [HTTP-állapotkódok (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span><span class="sxs-lookup"><span data-stu-id="d516f-180">For more information on other HTTP status codes that could be returned in case of failure, see [HTTP status codes (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d516f-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d516f-181">Next steps</span></span>
<span data-ttu-id="d516f-182">Után feltöltése az Azure Search-index, a lekérdezések toosearch dokumentumok kiállító készen toostart lesz.</span><span class="sxs-lookup"><span data-stu-id="d516f-182">After populating your Azure Search index, you will be ready toostart issuing queries toosearch for documents.</span></span> <span data-ttu-id="d516f-183">Részletes információk: [Az Azure Search-index lekérdezése](search-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d516f-183">See [Query Your Azure Search Index](search-query-overview.md) for details.</span></span>
