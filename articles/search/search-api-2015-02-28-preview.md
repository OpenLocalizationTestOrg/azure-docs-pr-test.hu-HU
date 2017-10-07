---
title: "aaaAzure keresési szolgáltatás REST API-verzió 2015-02-28 – előzetes verzió |} Microsoft Docs"
description: "Az Azure Search szolgáltatás REST API-verzió 2015-02-28-előzetes verzió például a természetes nyelvi elemzőkkel és moreLikeThis keresések kísérleti funkciót tartalmaz."
services: search
documentationcenter: na
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 3dba3bf8-9c83-42f6-82bc-04727bd11037
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/01/2017
ms.author: brjohnst
ms.openlocfilehash: 63cb30b7f43a42552c6744ea087afea947857224
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a><span data-ttu-id="9286d-103">Az Azure Search szolgáltatás REST API-ja: Verzió 2015-02-28 – előzetes</span><span class="sxs-lookup"><span data-stu-id="9286d-103">Azure Search Service REST API: Version 2015-02-28-Preview</span></span>
<span data-ttu-id="9286d-104">Ez a cikk hello referenciadokumentációt tartalmaz `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9286d-104">This article is hello reference documentation for `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9286d-105">Ez az előnézet bővíti hello általánosan elérhető verziószámának, [api-version = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), a következő kísérleti funkciók hello megadásával:</span><span class="sxs-lookup"><span data-stu-id="9286d-105">This preview extends hello current generally available version, [api-version=2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), by providing hello following experimental features:</span></span>

* <span data-ttu-id="9286d-106">`moreLikeThis`a hello lekérdezésparaméter [dokumentumok keresése](#SearchDocs) API.</span><span class="sxs-lookup"><span data-stu-id="9286d-106">`moreLikeThis` query parameter in hello [Search Documents](#SearchDocs) API.</span></span> <span data-ttu-id="9286d-107">Egyéb releváns tooanother adott dokumentum-dokumentumok találja meg.</span><span class="sxs-lookup"><span data-stu-id="9286d-107">It finds other documents that are relevant tooanother specific document.</span></span>

<span data-ttu-id="9286d-108">Néhány további részeinek hello `2015-02-28-Preview` REST API-t a külön-külön szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="9286d-108">A few additional parts of hello `2015-02-28-Preview` REST API are documented separately.</span></span> <span data-ttu-id="9286d-109">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="9286d-109">These include:</span></span>

* [<span data-ttu-id="9286d-110">A pontozási profil</span><span class="sxs-lookup"><span data-stu-id="9286d-110">Scoring Profiles</span></span>](search-api-scoring-profiles-2015-02-28-preview.md)
* [<span data-ttu-id="9286d-111">Indexelők</span><span class="sxs-lookup"><span data-stu-id="9286d-111">Indexers</span></span>](search-api-indexers-2015-02-28-preview.md)

<span data-ttu-id="9286d-112">Az Azure Search szolgáltatás több verzióban érhető el.</span><span class="sxs-lookup"><span data-stu-id="9286d-112">Azure Search service is available in multiple versions.</span></span> <span data-ttu-id="9286d-113">Tekintse meg a túl[keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="9286d-113">Please refer too[Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details.</span></span>

## <a name="apis-in-this-document"></a><span data-ttu-id="9286d-114">Ez a dokumentum API-k</span><span class="sxs-lookup"><span data-stu-id="9286d-114">APIs in this document</span></span>
<span data-ttu-id="9286d-115">Az Azure Search szolgáltatás API API-műveleteket támogatja a két URL-cím szintaxissal: egyszerű és OData (lásd: [OData (Azure Search API) támogatása](http://msdn.microsoft.com/library/azure/dn798932.aspx) részletekért).</span><span class="sxs-lookup"><span data-stu-id="9286d-115">Azure Search service API supports two URL syntaxes for API operations: simple and OData (see [Support for OData (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) for details).</span></span> <span data-ttu-id="9286d-116">hello alábbi hello egyszerű szintaxisa látható.</span><span class="sxs-lookup"><span data-stu-id="9286d-116">hello following list shows hello simple syntax.</span></span>

[<span data-ttu-id="9286d-117">Index létrehozása</span><span class="sxs-lookup"><span data-stu-id="9286d-117">Create Index</span></span>](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[<span data-ttu-id="9286d-118">Index frissítése</span><span class="sxs-lookup"><span data-stu-id="9286d-118">Update Index</span></span>](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="9286d-119">Index beolvasása</span><span class="sxs-lookup"><span data-stu-id="9286d-119">Get Index</span></span>](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="9286d-120">Indexek listázása</span><span class="sxs-lookup"><span data-stu-id="9286d-120">Listing Indexes</span></span>](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[<span data-ttu-id="9286d-121">Megtekintheti a statisztikákat Index</span><span class="sxs-lookup"><span data-stu-id="9286d-121">Get Index Statistics</span></span>](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[<span data-ttu-id="9286d-122">Teszt elemző eszköz</span><span class="sxs-lookup"><span data-stu-id="9286d-122">Test Analyzer</span></span>](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[<span data-ttu-id="9286d-123">Index törlése</span><span class="sxs-lookup"><span data-stu-id="9286d-123">Delete an Index</span></span>](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="9286d-124">Hozzáadása, törlése, és az Index adatainak frissítése</span><span class="sxs-lookup"><span data-stu-id="9286d-124">Add, Delete, and Update Data within an Index</span></span>](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[<span data-ttu-id="9286d-125">Dokumentumok keresése</span><span class="sxs-lookup"><span data-stu-id="9286d-125">Search Documents</span></span>](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[<span data-ttu-id="9286d-126">Keresés a dokumentum</span><span class="sxs-lookup"><span data-stu-id="9286d-126">Lookup Document</span></span>](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[<span data-ttu-id="9286d-127">A dokumentumok száma</span><span class="sxs-lookup"><span data-stu-id="9286d-127">Count Documents</span></span>](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[<span data-ttu-id="9286d-128">Javaslatok</span><span class="sxs-lookup"><span data-stu-id="9286d-128">Suggestions</span></span>](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

- - -
<a name="IndexOps"></a>

## <a name="index-operations"></a><span data-ttu-id="9286d-129">Indexművelet</span><span class="sxs-lookup"><span data-stu-id="9286d-129">Index Operations</span></span>
<span data-ttu-id="9286d-130">Hozzon létre, és az Azure Search szolgáltatás egyszerű HTTP-kérelmek (POST, GET, PUT, DELETE) egy adott indexű erőforrás elleni keresztül Indexek kezelése.</span><span class="sxs-lookup"><span data-stu-id="9286d-130">You can create and manage indexes in Azure Search service via simple HTTP requests (POST, GET, PUT, DELETE) against a given index resource.</span></span> <span data-ttu-id="9286d-131">az index toocreate, először utáni hello a sémát indexeli leíró JSON-dokumentumból.</span><span class="sxs-lookup"><span data-stu-id="9286d-131">toocreate an index, you first POST a JSON document that describes hello index schema.</span></span> <span data-ttu-id="9286d-132">hello séma definiálja az hello mezők hello index, az adattípusokat és hogyan lehet használni őket (például a teljes szöveges keresés, szűrők, rendezés vagy értékkorlátozás).</span><span class="sxs-lookup"><span data-stu-id="9286d-132">hello schema defines hello fields of hello index, their data types, and how they can be used (for example, in full-text searches, filters, sorting, or faceting).</span></span> <span data-ttu-id="9286d-133">A pontozási profilok, javaslattevők és egyéb attribútumai tooconfigure hello viselkedésének hello index is meghatározza.</span><span class="sxs-lookup"><span data-stu-id="9286d-133">It also defines scoring profiles, suggesters and other attributes tooconfigure hello behavior of hello index.</span></span>

<span data-ttu-id="9286d-134">hello következő példa illusztrálja hello Leírás mezője két nyelvű Szálloda adatai a keresést használt séma.</span><span class="sxs-lookup"><span data-stu-id="9286d-134">hello following example provides an illustration of a schema used for searching on hotel information with hello Description field defined in two languages.</span></span> <span data-ttu-id="9286d-135">Figyelje meg, hogyan attribútumok szabályozza, hogyan használja fel hello mező.</span><span class="sxs-lookup"><span data-stu-id="9286d-135">Notice how attributes control how hello field is used.</span></span> <span data-ttu-id="9286d-136">Például hello `hotelId` hello dokumentum kulcsként használt (`"key": true`) és a teljes szöveges keresés nem tartozik (`"searchable": false`).</span><span class="sxs-lookup"><span data-stu-id="9286d-136">For example hello `hotelId` is used as hello document key (`"key": true`) and is excluded from full-text searches (`"searchable": false`).</span></span>

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

<span data-ttu-id="9286d-137">Hello index létrehozása után be hello index feltöltése dokumentumok fogja feltölteni.</span><span class="sxs-lookup"><span data-stu-id="9286d-137">After hello index is created, you'll upload documents that populate hello index.</span></span> <span data-ttu-id="9286d-138">Lásd: [hozzáadása vagy frissítés dokumentumok](#AddOrUpdateDocuments) a következő lépéshez.</span><span class="sxs-lookup"><span data-stu-id="9286d-138">See [Add or Update Documents](#AddOrUpdateDocuments) for this next step.</span></span>

<span data-ttu-id="9286d-139">A videó tooindexing az Azure Search, lásd: hello [epizód Channel 9 felhő fedik le az Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).</span><span class="sxs-lookup"><span data-stu-id="9286d-139">For a video introduction tooindexing in Azure Search, see hello [Channel 9 Cloud Cover episode on Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).</span></span>

<a name="CreateIndex"></a>

## <a name="create-index"></a><span data-ttu-id="9286d-140">Index létrehozása</span><span class="sxs-lookup"><span data-stu-id="9286d-140">Create Index</span></span>
<span data-ttu-id="9286d-141">Az index hello elsődleges eszközök rendszerezése és az Azure Search dokumentumok keresése, hasonló toohow egy tábla egy adatbázis rendezi.</span><span class="sxs-lookup"><span data-stu-id="9286d-141">An index is hello primary means of organizing and searching documents in Azure Search, similar toohow a table organizes records in a database.</span></span> <span data-ttu-id="9286d-142">Minden egyes index összes toohello a sémát indexeli (mezőnevek, az adattípusokat és tulajdonságait) felel meg, de indexek is adja meg a további szerkezetek (javaslattevők, pontozási profilokat és a CORS-beállítások), amely más keresési tulajdonságainak megadásához dokumentumok gyűjteményével rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="9286d-142">Each index has a collection of documents that all conform toohello index schema (field names, data types, and properties), but indexes also specify additional constructs (suggesters, scoring profiles, and CORS options) that define other search behaviors.</span></span>

<span data-ttu-id="9286d-143">Létrehozhat egy új index használ egy HTTP POST vagy a PUT kérelmek Azure Search szolgáltatás belül.</span><span class="sxs-lookup"><span data-stu-id="9286d-143">You can create a new index within an Azure Search service using an HTTP POST or PUT request.</span></span> <span data-ttu-id="9286d-144">hello hello kérelem törzse egy JSON-séma, amely hello index és konfigurációs információt.</span><span class="sxs-lookup"><span data-stu-id="9286d-144">hello body of hello request is a JSON schema that specifies hello index and configuration information.</span></span>

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="9286d-145">Azt is megteheti PUT használja, és adja meg a hello URI hello index nevét.</span><span class="sxs-lookup"><span data-stu-id="9286d-145">Alternatively, you can use PUT and specify hello index name on hello URI.</span></span> <span data-ttu-id="9286d-146">Ha hello index nem létezik, a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="9286d-146">If hello index does not exist, it will be created.</span></span>

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

<span data-ttu-id="9286d-147">Az index létrehozása határozza meg, és használja a keresési műveleteket hello dokumentumok hello szerkezete.</span><span class="sxs-lookup"><span data-stu-id="9286d-147">Creating an index determines hello structure of hello documents stored and used in search operations.</span></span> <span data-ttu-id="9286d-148">Feltöltése hello indexe egy külön művelet.</span><span class="sxs-lookup"><span data-stu-id="9286d-148">Populating hello index is a separate operation.</span></span> <span data-ttu-id="9286d-149">Az ebben a lépésben használhatja egy [indexelő](https://msdn.microsoft.com/library/azure/mt183328.aspx) (támogatott adatforrások érhető el) vagy egy [hozzáadása, frissítése vagy törlése dokumentumok](https://msdn.microsoft.com/library/azure/dn798930.aspx) műveletet.</span><span class="sxs-lookup"><span data-stu-id="9286d-149">For this step, you can use an [indexer](https://msdn.microsoft.com/library/azure/mt183328.aspx) (available for supported data sources) or an [Add, Update, or Delete Documents](https://msdn.microsoft.com/library/azure/dn798930.aspx) operation.</span></span> <span data-ttu-id="9286d-150">fordított hello index jön létre, amikor hello dokumentumok van közzétéve.</span><span class="sxs-lookup"><span data-stu-id="9286d-150">hello inverted index is generated when hello documents are posted.</span></span>

<span data-ttu-id="9286d-151">**Megjegyzés:**: indexek engedélyezett maximális száma hello függ a tarifacsomagra vált.</span><span class="sxs-lookup"><span data-stu-id="9286d-151">**Note**: hello maximum number of indexes allowed varies by pricing tier.</span></span> <span data-ttu-id="9286d-152">hello szabad szolgáltatás lehetővé teszi, hogy be too3 indexeket.</span><span class="sxs-lookup"><span data-stu-id="9286d-152">hello free service allows up too3 indexes.</span></span> <span data-ttu-id="9286d-153">Standard szolgáltatás lehetővé teszi, hogy 50 indexek érhető el keresési szolgáltatásonként.</span><span class="sxs-lookup"><span data-stu-id="9286d-153">Standard service allows 50 indexes per Search service.</span></span> <span data-ttu-id="9286d-154">Lásd: [korlátozások és megkötések](http://msdn.microsoft.com/library/azure/dn798934.aspx) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="9286d-154">See [Limits and constraints](http://msdn.microsoft.com/library/azure/dn798934.aspx) for details.</span></span>

<span data-ttu-id="9286d-155">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="9286d-155">**Request**</span></span>

<span data-ttu-id="9286d-156">HTTPS-kapcsolat szükséges az összes szolgáltatási kérelmek.</span><span class="sxs-lookup"><span data-stu-id="9286d-156">HTTPS is required for all service requests.</span></span> <span data-ttu-id="9286d-157">Hello **a Create Index** kérelem egy POST vagy a PUT metódust használó lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9286d-157">hello **Create Index** request can be constructed using either a POST or PUT method.</span></span> <span data-ttu-id="9286d-158">POST használatakor meg kell adnia egy index neve mellett hello séma Indexdefiníció hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="9286d-158">When using POST, you must provide an index name in hello request body along with hello index schema definition.</span></span> <span data-ttu-id="9286d-159">A PUT hello indexnév része hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="9286d-159">With PUT, hello index name is part of hello URL.</span></span> <span data-ttu-id="9286d-160">Ha hello index nem létezik, akkor jön létre.</span><span class="sxs-lookup"><span data-stu-id="9286d-160">If hello index doesn't exist, it is created.</span></span> <span data-ttu-id="9286d-161">Ha már létezik, akkor frissített toohello új definíciója.</span><span class="sxs-lookup"><span data-stu-id="9286d-161">If it already exists, it is updated toohello new definition.</span></span>

<span data-ttu-id="9286d-162">hello indexnév kell kell kisbetű, betűvel vagy számmal kezdődhet, nincs perjeleket vagy pontokból és 128 karakternél rövidebb lehet.</span><span class="sxs-lookup"><span data-stu-id="9286d-162">hello index name must be lower case, start with a letter or number, have no slashes or dots, and be less than 128 characters.</span></span> <span data-ttu-id="9286d-163">Hello az index nevének betűvel vagy számmal verziótól kezdődően után hello részeinek hello neve tartalmazhatnak bármely betű, szám és kötőjeleket, mindaddig, amíg nincsenek egymást követő kötőjeleket hello.</span><span class="sxs-lookup"><span data-stu-id="9286d-163">After starting hello index name with a letter or number, hello rest of hello name can include any letter, number and dashes, as long as hello dashes are not consecutive.</span></span>

<span data-ttu-id="9286d-164">Hello `api-version` szükséges.</span><span class="sxs-lookup"><span data-stu-id="9286d-164">hello `api-version` is required.</span></span> <span data-ttu-id="9286d-165">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) elérhető verzió listáját.</span><span class="sxs-lookup"><span data-stu-id="9286d-165">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for a list of available versions.</span></span>

<span data-ttu-id="9286d-166">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="9286d-166">**Request Headers**</span></span>

<span data-ttu-id="9286d-167">a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9286d-167">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9286d-168">`Content-Type`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="9286d-168">`Content-Type`: Required.</span></span> <span data-ttu-id="9286d-169">Állítsa ezt túl`application/json`</span><span class="sxs-lookup"><span data-stu-id="9286d-169">Set this too`application/json`</span></span>
* <span data-ttu-id="9286d-170">`api-key`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="9286d-170">`api-key`: Required.</span></span> <span data-ttu-id="9286d-171">Hello `api-key` szolgál</span><span class="sxs-lookup"><span data-stu-id="9286d-171">hello `api-key` is used to</span></span>
* <span data-ttu-id="9286d-172">hello kérelem tooyour keresési szolgáltatás hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9286d-172">authenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9286d-173">Egy karakterláncértéket, egyedi tooyour szolgáltatást is.</span><span class="sxs-lookup"><span data-stu-id="9286d-173">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="9286d-174">Hello **a Create Index** kérelem tartalmaznia kell egy `api-key` fejlécet beállítani tooyour adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).</span><span class="sxs-lookup"><span data-stu-id="9286d-174">hello **Create Index** request must include an `api-key` header set tooyour admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9286d-175">Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="9286d-175">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9286d-176">Hello szolgáltatás nevet is kaphat és `api-key` a szolgáltatás irányítópultján a hello Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9286d-176">You can get both hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9286d-177">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="9286d-177">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9286d-178"><a name="RequestData"></a>
**Törzs szintaxis**</span><span class="sxs-lookup"><span data-stu-id="9286d-178"><a name="RequestData"></a>
**Request Body Syntax**</span></span>

<span data-ttu-id="9286d-179">hello kérelem törzse hello tartalmaz sémadefiníciót, beleértve hello a mezőket sorolja fel adatokat fog az indexbe kell táplált dokumentumok belül, adattípusok, attribútumok, valamint a pontozási profilok, amelyek egy választható listájához használt tooscore megfelelő dokumentumok lekérdezés idejét.</span><span class="sxs-lookup"><span data-stu-id="9286d-179">hello body of hello request contains a schema definition, which includes hello list of data fields within documents that will be fed into this index, data types, attributes, as well as an optional list of scoring profiles that are used tooscore matching documents at query time.</span></span>

<span data-ttu-id="9286d-180">Vegye figyelembe, hogy egy POST kérést, meg kell adnia hello indexnév hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="9286d-180">Note that for a POST request, you must specify hello index name in hello request body.</span></span>

<span data-ttu-id="9286d-181">Csak lehet egy kulcsmezőt hello index.</span><span class="sxs-lookup"><span data-stu-id="9286d-181">There can only be one key field in hello index.</span></span> <span data-ttu-id="9286d-182">Toobe mezőnek van.</span><span class="sxs-lookup"><span data-stu-id="9286d-182">It has toobe a string field.</span></span> <span data-ttu-id="9286d-183">Ez a mező képviseli hello hello index tárolt egyes dokumentumok egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9286d-183">This field represents hello unique identifier for each document stored within hello index.</span></span>

<span data-ttu-id="9286d-184">hello fő részeket az index hello alábbiakat foglalja magába:</span><span class="sxs-lookup"><span data-stu-id="9286d-184">hello main parts of an index include hello following:</span></span>

* `name`
* <span data-ttu-id="9286d-185">`fields`amely az indexbe, például név, adattípus és ezt a mezőt az engedélyezett műveletek meghatározó tulajdonságok biztosítása táplált lesz.</span><span class="sxs-lookup"><span data-stu-id="9286d-185">`fields` that will be fed into this index, including name, data type, and properties that define allowable actions on that field.</span></span>
* <span data-ttu-id="9286d-186">`suggesters`Automatikus kitöltés vagy begépelt lekérdezések használatos.</span><span class="sxs-lookup"><span data-stu-id="9286d-186">`suggesters` used for auto-complete or type-ahead queries.</span></span>
* <span data-ttu-id="9286d-187">`scoringProfiles`Egyéni keresési pontszám prioritás használatos.</span><span class="sxs-lookup"><span data-stu-id="9286d-187">`scoringProfiles` used for custom search score ranking.</span></span> <span data-ttu-id="9286d-188">Lásd: [pontozási profilok hozzáadása](https://msdn.microsoft.com/library/azure/dn798928.aspx) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="9286d-188">See [Add scoring profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for details.</span></span>
* <span data-ttu-id="9286d-189">`analyzers`, `charFilters`, `tokenizers`, `tokenFilters` hogyan a dokumentumok lekérdezések bontottuk példánycímkének/kereshető jogkivonatokba toodefine használt.</span><span class="sxs-lookup"><span data-stu-id="9286d-189">`analyzers`, `charFilters`, `tokenizers`, `tokenFilters` used toodefine how your documents/queries are broken into indexable/searchable tokens.</span></span> <span data-ttu-id="9286d-190">Lásd: [elemzése az Azure Search](https://aka.ms//azsanalysis) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="9286d-190">See [Analysis in Azure Search](https://aka.ms//azsanalysis) for details.</span></span>
* <span data-ttu-id="9286d-191">`defaultScoringProfile`toooverwrite hello alapértelmezett viselkedés pontozási használt.</span><span class="sxs-lookup"><span data-stu-id="9286d-191">`defaultScoringProfile` used toooverwrite hello default scoring behaviors.</span></span>
* <span data-ttu-id="9286d-192">`corsOptions`az index tooallow eltérő eredetű lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="9286d-192">`corsOptions` tooallow cross-origin queries against your index.</span></span>

<span data-ttu-id="9286d-193">hello hello-kérések forgalma rendszerezésére szolgál szintaxisa a következő.</span><span class="sxs-lookup"><span data-stu-id="9286d-193">hello syntax for structuring hello request payload is as follows.</span></span> <span data-ttu-id="9286d-194">Egy minta kérelem biztosítja az ebben a témakörben további.</span><span class="sxs-lookup"><span data-stu-id="9286d-194">A sample request is provided further on in this topic.</span></span>

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of hello analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of hello search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of hello indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in hello future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies toosearchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading toonow over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

<span data-ttu-id="9286d-195">**Indexattribútumok**</span><span class="sxs-lookup"><span data-stu-id="9286d-195">**Index Attributes**</span></span>

<span data-ttu-id="9286d-196">hello következő attribútumok állíthat be az index létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="9286d-196">hello following attributes can be set when creating an index.</span></span> <span data-ttu-id="9286d-197">További információk a pontozási és profilok pontozási: [hozzáadása a pontozási profil](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span><span class="sxs-lookup"><span data-stu-id="9286d-197">For details about scoring and scoring profiles, see [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span></span>

<span data-ttu-id="9286d-198">`name`-Készletek hello hello mező nevét.</span><span class="sxs-lookup"><span data-stu-id="9286d-198">`name` - Sets hello name of hello field.</span></span>

<span data-ttu-id="9286d-199">`type`-Készletek hello hello mező adattípusának.</span><span class="sxs-lookup"><span data-stu-id="9286d-199">`type` - Sets hello data type for hello field.</span></span>

<span data-ttu-id="9286d-200">`searchable`-Állapotúként jelöli meg hello mező teljes szöveges keresés képes.</span><span class="sxs-lookup"><span data-stu-id="9286d-200">`searchable` - Marks hello field as full-text search-able.</span></span> <span data-ttu-id="9286d-201">Ez azt jelenti, hogy az elemzés során indexelő szóhatároló például is változni fog.</span><span class="sxs-lookup"><span data-stu-id="9286d-201">This means it will undergo analysis such as word-breaking during indexing.</span></span> <span data-ttu-id="9286d-202">Ha egy `searchable` tooa mezőérték "napfényes" hasonlóan, belső, akkor "moziba" és "day" hello egyedi jogkivonatok felosztása.</span><span class="sxs-lookup"><span data-stu-id="9286d-202">If you set a `searchable` field tooa value like "sunny day", internally it will be split into hello individual tokens "sunny" and "day".</span></span> <span data-ttu-id="9286d-203">Ez lehetővé teszi, hogy ezek a feltételek a teljes szöveges keresés.</span><span class="sxs-lookup"><span data-stu-id="9286d-203">This enables full-text searches for these terms.</span></span> <span data-ttu-id="9286d-204">Típusú mezők `Edm.String` vagy `Collection(Edm.String)` vannak `searchable` alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="9286d-204">Fields of type `Edm.String` or `Collection(Edm.String)` are `searchable` by default.</span></span> <span data-ttu-id="9286d-205">Nem lehet a más típusú mezők `searchable`.</span><span class="sxs-lookup"><span data-stu-id="9286d-205">Fields of other types cannot be `searchable`.</span></span>

* <span data-ttu-id="9286d-206">**Megjegyzés:**: `searchable` mezők az indexben területnek felhasználását, mivel az Azure Search fogja tárolni egy hello mező értéke a teljes szöveges keresés további tokenekre verzióját.</span><span class="sxs-lookup"><span data-stu-id="9286d-206">**Note**: `searchable` fields consume extra space in your index since Azure Search will store an additional tokenized version of hello field value for full-text searches.</span></span> <span data-ttu-id="9286d-207">Ha a kívánt toosave terület az indexet, és nem kell egy mező toobe keresések szerepel, és állítsa `searchable` túl`false`.</span><span class="sxs-lookup"><span data-stu-id="9286d-207">If you want toosave space in your index and you don't need a field toobe included in searches, set `searchable` too`false`.</span></span>

<span data-ttu-id="9286d-208">`filterable`-Lehetővé teszi a hivatkozott hello mező toobe `$filter` lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="9286d-208">`filterable` - Allows hello field toobe referenced in `$filter` queries.</span></span> <span data-ttu-id="9286d-209">`filterable`eltér az `searchable` a karakterláncok kezelésének módját.</span><span class="sxs-lookup"><span data-stu-id="9286d-209">`filterable` differs from `searchable` in how strings are handled.</span></span> <span data-ttu-id="9286d-210">Típusú mezők `Edm.String` vagy `Collection(Edm.String)` , amelyek `filterable` nem kerülnek szóhatároló, így csak pontosan megegyezik az összehasonlítást.</span><span class="sxs-lookup"><span data-stu-id="9286d-210">Fields of type `Edm.String` or `Collection(Edm.String)` that are `filterable` do not undergo word-breaking, so comparisons are for exact matches only.</span></span> <span data-ttu-id="9286d-211">Például, ha a mező `f` túl "napfényes" `$filter=f eq 'sunny'` megkeresi, nincs találat, de `$filter=f eq 'sunny day'` fog.</span><span class="sxs-lookup"><span data-stu-id="9286d-211">For example, if you set such a field `f` too"sunny day", `$filter=f eq 'sunny'` will find no matches, but `$filter=f eq 'sunny day'` will.</span></span> <span data-ttu-id="9286d-212">A mezők egyike sem `filterable` alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="9286d-212">All fields are `filterable` by default.</span></span>

<span data-ttu-id="9286d-213">`sortable`-Alapértelmezés szerint hello rendszer rendezése az eredmények pontszám, de sok elhatározásuk felhasználók érdemes toosort hello dokumentumok mezőket.</span><span class="sxs-lookup"><span data-stu-id="9286d-213">`sortable` - By default hello system sorts results by score, but in many experiences users will want toosort by fields in hello documents.</span></span> <span data-ttu-id="9286d-214">Típusú mezők `Collection(Edm.String)` nem lehet `sortable`.</span><span class="sxs-lookup"><span data-stu-id="9286d-214">Fields of type `Collection(Edm.String)` cannot be `sortable`.</span></span> <span data-ttu-id="9286d-215">A többi mező kitöltése `sortable` alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="9286d-215">All other fields are `sortable` by default.</span></span>

<span data-ttu-id="9286d-216">`facetable`-A keresési eredmények találati száma (a példában, keresse meg a digitális fényképezőgépek, és tekintse meg a találatok márka, Megapixel, ár, stb.) kategória szerint tartalmazó bemutató jellemzően használt.</span><span class="sxs-lookup"><span data-stu-id="9286d-216">`facetable`- Typically used in a presentation of search results that includes hit count by category (for example, search for digital cameras and see hits by brand, by megapixels, by price, etc.).</span></span> <span data-ttu-id="9286d-217">Ez a beállítás nem használható típusú mezők `Edm.GeographyPoint`.</span><span class="sxs-lookup"><span data-stu-id="9286d-217">This option cannot be used with fields of type `Edm.GeographyPoint`.</span></span> <span data-ttu-id="9286d-218">A többi mező kitöltése `facetable` alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="9286d-218">All other fields are `facetable` by default.</span></span>

* <span data-ttu-id="9286d-219">**Megjegyzés:**: típusú mezők `Edm.String` , amelyek `filterable`, `sortable`, vagy `facetable` legfeljebb 32 KB-os hossza.</span><span class="sxs-lookup"><span data-stu-id="9286d-219">**Note**: Fields of type `Edm.String` that are `filterable`, `sortable`, or `facetable` can be at most 32KB in length.</span></span> <span data-ttu-id="9286d-220">Ennek az az oka ilyen mezők egyetlen keresőkifejezéssel számít, és hello az Azure Search kifejezés hossza legfeljebb 32 KB lehet.</span><span class="sxs-lookup"><span data-stu-id="9286d-220">This is because such fields are treated as a single search term, and hello maximum length of a term in Azure Search is 32KB.</span></span> <span data-ttu-id="9286d-221">Ha ez az egyetlen mezőnek több szöveget kell toostore, szüksége lesz a tooexplicitly beállítása `filterable`, `sortable`, és `facetable` túl`false` az index definícióját a.</span><span class="sxs-lookup"><span data-stu-id="9286d-221">If you need toostore more text than this in a single string field, you will need tooexplicitly set `filterable`, `sortable`, and `facetable` too`false` in your index definition.</span></span>
* <span data-ttu-id="9286d-222">**Megjegyzés:**: Ha a mező nincs a fenti hello attribútumok beállítása túl`true` (`searchable`, `filterable`, `sortable`, vagy`facetable`) hello mező hatékonyan fordított hello index ki van zárva.</span><span class="sxs-lookup"><span data-stu-id="9286d-222">**Note**: If a field has none of hello above attributes set too`true` (`searchable`, `filterable`, `sortable`,  or`facetable`) hello field is effectively excluded from hello inverted index.</span></span> <span data-ttu-id="9286d-223">Ez akkor hasznos, amelyek nem szerepelnek a lekérdezések, amelyekre szükség van a találatok között mezők.</span><span class="sxs-lookup"><span data-stu-id="9286d-223">This option is useful for fields that are not used in queries, but are needed in search results.</span></span> <span data-ttu-id="9286d-224">Ilyen mezők kizárását hello index javítja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="9286d-224">Excluding such fields from hello index improves performance.</span></span>

<span data-ttu-id="9286d-225">`key`-Jelek hello dokumentumok hello index belül egyedi azonosítót tartalmazó mezőt.</span><span class="sxs-lookup"><span data-stu-id="9286d-225">`key` - Marks hello field as containing unique identifiers for documents within hello index.</span></span> <span data-ttu-id="9286d-226">Pontosan egy mezőt ki kell választani, hello `key` típusúnak kell lennie, és a mező `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="9286d-226">Exactly one field must be chosen as hello `key` field and it must be of type `Edm.String`.</span></span> <span data-ttu-id="9286d-227">Kulcsmezők lehet fel dokumentumokat közvetlenül hello keresztül használt toolook [keresési API](#LookupAPI).</span><span class="sxs-lookup"><span data-stu-id="9286d-227">Key fields can be used toolook up documents directly via hello [Lookup API](#LookupAPI).</span></span>

<span data-ttu-id="9286d-228">`retrievable`-Állítja be, hogy hello mező a keresési eredmény adhatók vissza.</span><span class="sxs-lookup"><span data-stu-id="9286d-228">`retrievable` - Sets whether hello field can be returned in a search result.</span></span>  <span data-ttu-id="9286d-229">Ez akkor hasznos, ha kívánja szűrőként, toouse (például margó) mezőt, rendezés, vagy pontozási mechanizmus, de nem szeretné hello mező toobe látható toohello végfelhasználói.</span><span class="sxs-lookup"><span data-stu-id="9286d-229">This is useful when you want toouse a field (for example, margin) as a filter, sorting, or scoring mechanism but do not want hello field toobe visible toohello end user.</span></span> <span data-ttu-id="9286d-230">Ennek az attribútumnak kell lennie `true` a `key` mezőket.</span><span class="sxs-lookup"><span data-stu-id="9286d-230">This attribute must be `true` for `key` fields.</span></span>

<span data-ttu-id="9286d-231">`analyzer`-Készletek hello hello analyzer toouse hello mező nevét, a Keresés és indexelési idő.</span><span class="sxs-lookup"><span data-stu-id="9286d-231">`analyzer` - Sets hello name of hello analyzer toouse for hello field at search time and indexing time.</span></span> <span data-ttu-id="9286d-232">Megengedett értékek hello lásd: [elemzőkkel](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="9286d-232">For hello allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="9286d-233">Ez a beállítás használható csak `searchable` mezőket, és nem állítható be, vagy együtt `searchAnalyzer` vagy `indexAnalyzer`.</span><span class="sxs-lookup"><span data-stu-id="9286d-233">This option can be used only with `searchable` fields and it can't be set together with either `searchAnalyzer` or `indexAnalyzer`.</span></span>  <span data-ttu-id="9286d-234">Miután hello analyzer választja, azt hello mező nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="9286d-234">Once hello analyzer is chosen, it cannot be changed for hello field.</span></span>

<span data-ttu-id="9286d-235">`searchAnalyzer`-Készletek hello hello mező keresés közben használatos hello elemző nevét.</span><span class="sxs-lookup"><span data-stu-id="9286d-235">`searchAnalyzer` - Sets hello name of hello analyzer used at search time for hello field.</span></span> <span data-ttu-id="9286d-236">Megengedett értékek hello lásd: [elemzőkkel](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="9286d-236">For hello allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="9286d-237">Ez a beállítás használható csak `searchable` mezőket.</span><span class="sxs-lookup"><span data-stu-id="9286d-237">This option can be used only with `searchable` fields.</span></span> <span data-ttu-id="9286d-238">Együtt kell beállítani `indexAnalyzer` és nem állítható be együtt hello `analyzer` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9286d-238">It must be set together with `indexAnalyzer` and it cannot be set together with hello `analyzer` option.</span></span> <span data-ttu-id="9286d-239">Az elemző meglévő mezőre lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="9286d-239">This analyzer can be updated on an existing field.</span></span>

<span data-ttu-id="9286d-240">`indexAnalyzer`-Készletek hello hello mező indexelési közben használatos hello elemző nevét.</span><span class="sxs-lookup"><span data-stu-id="9286d-240">`indexAnalyzer` - Sets hello name of hello analyzer used at indexing time for hello field.</span></span> <span data-ttu-id="9286d-241">Megengedett értékek hello lásd: [elemzőkkel](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="9286d-241">For hello allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="9286d-242">Ez a beállítás használható csak `searchable` mezőket.</span><span class="sxs-lookup"><span data-stu-id="9286d-242">This option can be used only with `searchable` fields.</span></span> <span data-ttu-id="9286d-243">Együtt kell beállítani `searchAnalyzer` és nem állítható be együtt hello `analyzer` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9286d-243">It must be set together with `searchAnalyzer` and it cannot be set together with hello `analyzer` option.</span></span> <span data-ttu-id="9286d-244">Miután hello analyzer választja, azt hello mező nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="9286d-244">Once hello analyzer is chosen, it cannot be changed for hello field.</span></span>

<span data-ttu-id="9286d-245">`suggesters`-Készletek hello keresési mód és a javaslatok hello tartalma hello forrás mezőt.</span><span class="sxs-lookup"><span data-stu-id="9286d-245">`suggesters` - Sets hello search mode and fields that are hello source of hello content for suggestions.</span></span> <span data-ttu-id="9286d-246">Lásd: [Javaslattevők](#Suggesters) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="9286d-246">See [Suggesters](#Suggesters) for details.</span></span>

<span data-ttu-id="9286d-247">`scoringProfiles`-Határozza meg az egyéni pontozási viselkedéseket, amelyek lehetővé teszik, hogy befolyásolják mely elemek jelennek meg a keresési eredmények között magasabb.</span><span class="sxs-lookup"><span data-stu-id="9286d-247">`scoringProfiles` - Defines custom scoring behaviors that let you influence which items appear higher in search results.</span></span> <span data-ttu-id="9286d-248">A pontozási profil mező súlyok és funkciók épülnek fel.</span><span class="sxs-lookup"><span data-stu-id="9286d-248">Scoring profiles are made up of field weights and functions.</span></span> <span data-ttu-id="9286d-249">Lásd: [hozzáadása a pontozási profil](https://msdn.microsoft.com/library/azure/dn798928.aspx) további információt a relevanciaprofil használt hello attribútum.</span><span class="sxs-lookup"><span data-stu-id="9286d-249">See [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for more information about hello attributes used in a scoring profile.</span></span>

<span data-ttu-id="9286d-250"><!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Nyelvi támogatás**</span><span class="sxs-lookup"><span data-stu-id="9286d-250"><!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Language support**</span></span>

<span data-ttu-id="9286d-251">Kereshető mezők változni elemzés, hogy a legtöbb gyakran magában foglalja a szóhatároló, szöveges normalizálási és szűri ki a feltételeket.</span><span class="sxs-lookup"><span data-stu-id="9286d-251">Searchable fields undergo analysis that most frequently involves word-breaking, text normalization, and filtering out terms.</span></span> <span data-ttu-id="9286d-252">Alapértelmezés szerint az Azure Search kereshető mezők hello az elemzett [Apache Lucene szabványos analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) szöveg amely bontja a következő elemek a["Unicode szöveg Szegmentálás"](http://unicode.org/reports/tr29/) szabályok.</span><span class="sxs-lookup"><span data-stu-id="9286d-252">By default, searchable fields in Azure Search are analyzed with hello [Apache Lucene Standard analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) which breaks text into elements following the["Unicode Text Segmentation"](http://unicode.org/reports/tr29/) rules.</span></span> <span data-ttu-id="9286d-253">Emellett hello szabványos analyzer alakítja minden karakterek tootheir nagybetűs formában.</span><span class="sxs-lookup"><span data-stu-id="9286d-253">Additionally, hello standard analyzer converts all characters tootheir lower case form.</span></span> <span data-ttu-id="9286d-254">Indexelt dokumentumok és a keresési feltételek halad át hello elemzés indexelő és a lekérdezés feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="9286d-254">Both indexed documents and search terms go through hello analysis during indexing and query processing.</span></span>

<span data-ttu-id="9286d-255">Az Azure Search támogatja a különböző nyelveken.</span><span class="sxs-lookup"><span data-stu-id="9286d-255">Azure Search supports a variety of languages.</span></span> <span data-ttu-id="9286d-256">Minden egyes nyelvi egy nem szabványos szöveg analyzer, amelyek jellemzői egy adott nyelvhez tartozó fiókok igényel.</span><span class="sxs-lookup"><span data-stu-id="9286d-256">Each language requires a non-standard text analyzer which accounts for characteristics of a given language.</span></span> <span data-ttu-id="9286d-257">Az Azure Search kétféle típusú elemzőkkel:</span><span class="sxs-lookup"><span data-stu-id="9286d-257">Azure Search offers two types of analyzers:</span></span>

* <span data-ttu-id="9286d-258">35 elemzőkkel Lucene által támogatott.</span><span class="sxs-lookup"><span data-stu-id="9286d-258">35 analyzers backed by Lucene.</span></span>
* <span data-ttu-id="9286d-259">50 elemzőkkel által védett Microsoft természetes nyelvű Office-és Bing használt technológia feldolgozása támogatott.</span><span class="sxs-lookup"><span data-stu-id="9286d-259">50 analyzers backed by proprietary Microsoft natural language processing technology used in Office and Bing.</span></span>

<span data-ttu-id="9286d-260">Néhány fejlesztők inkább hello Lucene több ismerős, egyszerű, nyílt forráskódú megoldás.</span><span class="sxs-lookup"><span data-stu-id="9286d-260">Some developers might prefer hello more familiar, simple, open-source solution of Lucene.</span></span> <span data-ttu-id="9286d-261">Lucene elemzőkkel gyorsabb, de hello Microsoft elemzőkkel képességeit, például a Lemmatizálás speciális, word (például német, dán, holland, svéd, norvég, észt, Befejezés, magyar, Szlovák nyelven) decompounding és entitás felismerési (URL-címek e-mailek, dátumok, számok).</span><span class="sxs-lookup"><span data-stu-id="9286d-261">Lucene analyzers are faster, but hello Microsoft analyzers have advanced capabilities, such as lemmatization, word decompounding (in languages like German, Danish, Dutch, Swedish, Norwegian, Estonian, Finish, Hungarian, Slovak) and entity recognition (URLs, emails, dates, numbers).</span></span> <span data-ttu-id="9286d-262">Ha lehetséges futtassa a mindkét hello Microsoft és Lucene elemzőkkel toodecide melyik felel meg leginkább az összehasonlítást.</span><span class="sxs-lookup"><span data-stu-id="9286d-262">If possible, you should run comparisons of both hello Microsoft and Lucene analyzers toodecide which one is a better fit.</span></span>

<span data-ttu-id="9286d-263">***Annak összevetése***</span><span class="sxs-lookup"><span data-stu-id="9286d-263">***How they compare***</span></span>

<span data-ttu-id="9286d-264">az angol nyelvű tájékoztatáshoz hello Lucene analyzer hello szabványos analyzer bővíti.</span><span class="sxs-lookup"><span data-stu-id="9286d-264">hello Lucene analyzer for English extends hello standard analyzer.</span></span> <span data-ttu-id="9286d-265">Az (záró tartozó) possessives eltávolítja a szavakat, vonatkozik, amelyek megfelelően [Porter származó algoritmus](http://tartarus.org/~martin/PorterStemmer/), és eltávolítja az angol [szavak leállítása](http://en.wikipedia.org/wiki/Stop_words).</span><span class="sxs-lookup"><span data-stu-id="9286d-265">It removes possessives (trailing 's) from words, applies stemming as per [Porter Stemming algorithm](http://tartarus.org/~martin/PorterStemmer/), and removes English [stop words](http://en.wikipedia.org/wiki/Stop_words).</span></span>

<span data-ttu-id="9286d-266">Összehasonlításképpen hello Microsoft analyzer helyett származó Lemmatizálás hajt végre.</span><span class="sxs-lookup"><span data-stu-id="9286d-266">In comparison, hello Microsoft analyzer performs lemmatization instead of stemming.</span></span> <span data-ttu-id="9286d-267">Az azt jelenti, hogy is képes kezelni ragozott és szabálytalan word űrlapok javulás mi több megfelelő találatok eredményez (7 modulja figyelési [Azure keresési MVA bemutató](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) további részletekért).</span><span class="sxs-lookup"><span data-stu-id="9286d-267">It means it can handle inflected and irregular word forms much better what results in more relevant search results (watch module 7 of [Azure Search MVA presentation](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) for more details).</span></span>

<span data-ttu-id="9286d-268">A Microsoft elemzőkkel indexelő van átlagosan toothree kétszer lassabb, mint a Lucene megfelelőjükre, attól függően, hogy hello nyelv.</span><span class="sxs-lookup"><span data-stu-id="9286d-268">Indexing with Microsoft analyzers is on average two toothree times slower than their Lucene equivalents, depending on hello language.</span></span> <span data-ttu-id="9286d-269">Keresés teljesítménye nem átlagos mérete lekérdezések jelentős hatással lehet.</span><span class="sxs-lookup"><span data-stu-id="9286d-269">Search performance should not be significantly affected for average size queries.</span></span>

<span data-ttu-id="9286d-270">***Konfigurálás***</span><span class="sxs-lookup"><span data-stu-id="9286d-270">***Configuration***</span></span>

<span data-ttu-id="9286d-271">Az egyes mezők hello Indexdefiníció, beállíthatja a hello `analyzer` tulajdonság tooan analyzer a neve, mely nyelvi és szállítói.</span><span class="sxs-lookup"><span data-stu-id="9286d-271">For each field in hello index definition, you can set hello `analyzer` property tooan analyzer name that specifies which language and vendor.</span></span> <span data-ttu-id="9286d-272">hello azonos analyzer fogja alkalmazni, amikor az indexelő, és ezt a mezőt keresésekor.</span><span class="sxs-lookup"><span data-stu-id="9286d-272">hello same analyzer will be applied when indexing and searching for that field.</span></span>
<span data-ttu-id="9286d-273">Például rendelkezhet az angol, francia és spanyol-párhuzamos hello a létező Szálloda leírások külön mezők ugyanazt az indexet.</span><span class="sxs-lookup"><span data-stu-id="9286d-273">For example, you can have separate fields for English, French, and Spanish hotel descriptions that exist side-by-side in hello same index.</span></span> <span data-ttu-id="9286d-274">Használjon hello ["searchFields" lekérdezésparaméter](#SearchQueryParameters) toospecify mely nyelvspecifikus mező toosearch szemben a lekérdezésekben.</span><span class="sxs-lookup"><span data-stu-id="9286d-274">Use hello ['searchFields' query parameter](#SearchQueryParameters) toospecify which language-specific field toosearch against in your queries.</span></span> <span data-ttu-id="9286d-275">Tekintse át, amely tartalmazza az hello lekérdezés példák `analyzer` tulajdonság [dokumentumok keresése](#SearchDocs).</span><span class="sxs-lookup"><span data-stu-id="9286d-275">You can review query examples that include hello `analyzer` property in [Search Documents](#SearchDocs).</span></span> 

<span data-ttu-id="9286d-276">***Analyzer listája***</span><span class="sxs-lookup"><span data-stu-id="9286d-276">***Analyzer list***</span></span>

<span data-ttu-id="9286d-277">Az alábbiakban rendszer hello támogatott nyelvek együtt Lucene és a Microsoft elemző nevét.</span><span class="sxs-lookup"><span data-stu-id="9286d-277">Below is hello list of supported languages together with Lucene and Microsoft analyzer names.</span></span>

<table style="font-size:12">
    <tr>
        <th><span data-ttu-id="9286d-278">Nyelv</span><span class="sxs-lookup"><span data-stu-id="9286d-278">Language</span></span></th>
        <th><span data-ttu-id="9286d-279">Microsoft-elemző eszköz neve</span><span class="sxs-lookup"><span data-stu-id="9286d-279">Microsoft analyzer name</span></span></th>
        <th><span data-ttu-id="9286d-280">Lucene analyzer neve</span><span class="sxs-lookup"><span data-stu-id="9286d-280">Lucene analyzer name</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-281">arab</span><span class="sxs-lookup"><span data-stu-id="9286d-281">Arabic</span></span></td>
        <td><span data-ttu-id="9286d-282">ar.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-282">ar.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-283">ar.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-283">ar.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-284">örmény</span><span class="sxs-lookup"><span data-stu-id="9286d-284">Armenian</span></span></td>
        <td></td>
        <td><span data-ttu-id="9286d-285">HY.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-285">hy.lucene</span></span></td>
      </tr>
    <tr>
        <td><span data-ttu-id="9286d-286">Bangla</span><span class="sxs-lookup"><span data-stu-id="9286d-286">Bangla</span></span></td>
        <td><span data-ttu-id="9286d-287">BN.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-287">bn.microsoft</span></span></td>
        <td></td>
    </tr>
      <tr>
        <td><span data-ttu-id="9286d-288">baszk</span><span class="sxs-lookup"><span data-stu-id="9286d-288">Basque</span></span></td>
        <td></td>
        <td><span data-ttu-id="9286d-289">EU.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-289">eu.lucene</span></span></td>
    </tr>
      <tr>
         <td><span data-ttu-id="9286d-290">bolgár</span><span class="sxs-lookup"><span data-stu-id="9286d-290">Bulgarian</span></span></td>
        <td><span data-ttu-id="9286d-291">BG.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-291">bg.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-292">BG.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-292">bg.lucene</span></span></td>
      </tr>
      <tr>
        <td><span data-ttu-id="9286d-293">katalán</span><span class="sxs-lookup"><span data-stu-id="9286d-293">Catalan</span></span></td>
        <td><span data-ttu-id="9286d-294">CA.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-294">ca.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-295">CA.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-295">ca.lucene</span></span></td>          
      </tr>
    <tr>
        <td><span data-ttu-id="9286d-296">kínai (egyszerűsített)</span><span class="sxs-lookup"><span data-stu-id="9286d-296">Chinese Simplified</span></span></td>
        <td><span data-ttu-id="9286d-297">zh-Hans.microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-297">zh-Hans.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-298">zh-Hans.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-298">zh-Hans.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-299">kínai (hagyományos)</span><span class="sxs-lookup"><span data-stu-id="9286d-299">Chinese Traditional</span></span></td>
        <td><span data-ttu-id="9286d-300">zh-Hant.microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-300">zh-Hant.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-301">zh-Hant.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-301">zh-Hant.lucene</span></span></td>        
    <tr>
    <tr>
        <td><span data-ttu-id="9286d-302">horvát</span><span class="sxs-lookup"><span data-stu-id="9286d-302">Croatian</span></span></td>
        <td><span data-ttu-id="9286d-303">hr.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-303">hr.microsoft</span></span></td>
        <td/></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-304">cseh</span><span class="sxs-lookup"><span data-stu-id="9286d-304">Czech</span></span></td>
        <td><span data-ttu-id="9286d-305">cs.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-305">cs.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-306">cs.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-306">cs.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="9286d-307">dán</span><span class="sxs-lookup"><span data-stu-id="9286d-307">Danish</span></span></td>
        <td><span data-ttu-id="9286d-308">da.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-308">da.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-309">da.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-309">da.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="9286d-310">holland</span><span class="sxs-lookup"><span data-stu-id="9286d-310">Dutch</span></span></td>
        <td><span data-ttu-id="9286d-311">NL.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-311">nl.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-312">NL.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-312">nl.lucene</span></span></td>    
    </tr>    
    <tr>
        <td><span data-ttu-id="9286d-313">Angol</span><span class="sxs-lookup"><span data-stu-id="9286d-313">English</span></span></td>        
        <td><span data-ttu-id="9286d-314">en.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-314">en.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-315">en.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-315">en.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-316">észt</span><span class="sxs-lookup"><span data-stu-id="9286d-316">Estonian</span></span></td>
        <td><span data-ttu-id="9286d-317">et.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-317">et.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-318">finn</span><span class="sxs-lookup"><span data-stu-id="9286d-318">Finnish</span></span></td>
        <td><span data-ttu-id="9286d-319">Fi.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-319">fi.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-320">Fi.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-320">fi.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="9286d-321">francia</span><span class="sxs-lookup"><span data-stu-id="9286d-321">French</span></span></td>
        <td><span data-ttu-id="9286d-322">FR.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-322">fr.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-323">FR.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-323">fr.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-324">galíciai</span><span class="sxs-lookup"><span data-stu-id="9286d-324">Galician</span></span></td>
        <td></td>
        <td><span data-ttu-id="9286d-325">GL.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-325">gl.lucene</span></span></td>        
      </tr>
    <tr>
        <td><span data-ttu-id="9286d-326">német</span><span class="sxs-lookup"><span data-stu-id="9286d-326">German</span></span></td>
        <td><span data-ttu-id="9286d-327">de.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-327">de.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-328">de.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-328">de.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-329">görög</span><span class="sxs-lookup"><span data-stu-id="9286d-329">Greek</span></span></td>
        <td><span data-ttu-id="9286d-330">el.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-330">el.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-331">el.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-331">el.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-332">gudzsaráti</span><span class="sxs-lookup"><span data-stu-id="9286d-332">Gujarati</span></span></td>
        <td><span data-ttu-id="9286d-333">Gu.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-333">gu.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-334">héber</span><span class="sxs-lookup"><span data-stu-id="9286d-334">Hebrew</span></span></td>
        <td><span data-ttu-id="9286d-335">He.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-335">he.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-336">hindi</span><span class="sxs-lookup"><span data-stu-id="9286d-336">Hindi</span></span></td>
        <td><span data-ttu-id="9286d-337">Hi.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-337">hi.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-338">Hi.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-338">hi.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-339">magyar</span><span class="sxs-lookup"><span data-stu-id="9286d-339">Hungarian</span></span></td>        
        <td><span data-ttu-id="9286d-340">hu.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-340">hu.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-341">hu.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-341">hu.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-342">izlandi</span><span class="sxs-lookup"><span data-stu-id="9286d-342">Icelandic</span></span></td>
        <td><span data-ttu-id="9286d-343">is.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-343">is.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-344">Indonéziai (Bahasa)</span><span class="sxs-lookup"><span data-stu-id="9286d-344">Indonesian (Bahasa)</span></span></td>
        <td><span data-ttu-id="9286d-345">ID.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-345">id.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-346">ID.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-346">id.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-347">ír</span><span class="sxs-lookup"><span data-stu-id="9286d-347">Irish</span></span></td>
        <td></td>
          <td><span data-ttu-id="9286d-348">GA.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-348">ga.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-349">olasz</span><span class="sxs-lookup"><span data-stu-id="9286d-349">Italian</span></span></td>
        <td><span data-ttu-id="9286d-350">IT.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-350">it.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-351">IT.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-351">it.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-352">japán</span><span class="sxs-lookup"><span data-stu-id="9286d-352">Japanese</span></span></td>
        <td><span data-ttu-id="9286d-353">ja.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-353">ja.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-354">ja.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-354">ja.lucene</span></span></td>

    </tr>
    <tr>
        <td><span data-ttu-id="9286d-355">kannada</span><span class="sxs-lookup"><span data-stu-id="9286d-355">Kannada</span></span></td>
        <td><span data-ttu-id="9286d-356">ka.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-356">ka.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-357">koreai</span><span class="sxs-lookup"><span data-stu-id="9286d-357">Korean</span></span></td>
        <td><span data-ttu-id="9286d-358">ko.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-358">ko.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-359">ko.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-359">ko.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-360">lett</span><span class="sxs-lookup"><span data-stu-id="9286d-360">Latvian</span></span></td>        
        <td><span data-ttu-id="9286d-361">LV.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-361">lv.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-362">LV.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-362">lv.lucene</span></span></td>    
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-363">litván</span><span class="sxs-lookup"><span data-stu-id="9286d-363">Lithuanian</span></span></td>
        <td><span data-ttu-id="9286d-364">lt.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-364">lt.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-365">malajálam</span><span class="sxs-lookup"><span data-stu-id="9286d-365">Malayalam</span></span></td>
        <td><span data-ttu-id="9286d-366">ml.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-366">ml.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-367">Maláj (latin betűs)</span><span class="sxs-lookup"><span data-stu-id="9286d-367">Malay (Latin)</span></span></td>
        <td><span data-ttu-id="9286d-368">MS.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-368">ms.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-369">marathi</span><span class="sxs-lookup"><span data-stu-id="9286d-369">Marathi</span></span></td>
        <td><span data-ttu-id="9286d-370">MR.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-370">mr.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-371">norvég</span><span class="sxs-lookup"><span data-stu-id="9286d-371">Norwegian</span></span></td>
        <td><span data-ttu-id="9286d-372">NB.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-372">nb.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-373">No.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-373">no.lucene</span></span></td>        
    </tr>
      <tr>
        <td><span data-ttu-id="9286d-374">perzsa</span><span class="sxs-lookup"><span data-stu-id="9286d-374">Persian</span></span></td>
        <td></td>
        <td><span data-ttu-id="9286d-375">fa.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-375">fa.lucene</span></span></td>        
      </tr>
    <tr>
        <td><span data-ttu-id="9286d-376">lengyel</span><span class="sxs-lookup"><span data-stu-id="9286d-376">Polish</span></span></td>
        <td><span data-ttu-id="9286d-377">pl.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-377">pl.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-378">pl.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-378">pl.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-379">Portugál (brazíliai)</span><span class="sxs-lookup"><span data-stu-id="9286d-379">Portuguese (Brazil)</span></span></td>
        <td><span data-ttu-id="9286d-380">PT-Br.microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-380">pt-Br.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-381">PT-Br.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-381">pt-Br.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-382">Portugál (Portugália)</span><span class="sxs-lookup"><span data-stu-id="9286d-382">Portuguese (Portugal)</span></span></td>
        <td><span data-ttu-id="9286d-383">PT-Pt.microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-383">pt-Pt.microsoft</span></span></td>        
        <td><span data-ttu-id="9286d-384">PT-Pt.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-384">pt-Pt.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-385">pandzsábi</span><span class="sxs-lookup"><span data-stu-id="9286d-385">Punjabi</span></span></td>
        <td><span data-ttu-id="9286d-386">Pa.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-386">pa.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-387">román</span><span class="sxs-lookup"><span data-stu-id="9286d-387">Romanian</span></span></td>
        <td><span data-ttu-id="9286d-388">ro.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-388">ro.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-389">ro.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-389">ro.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-390">orosz</span><span class="sxs-lookup"><span data-stu-id="9286d-390">Russian</span></span></td>
        <td><span data-ttu-id="9286d-391">RU.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-391">ru.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-392">RU.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-392">ru.lucene</span></span></td>    
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-393">szerb (cirill betűs)</span><span class="sxs-lookup"><span data-stu-id="9286d-393">Serbian (Cyrillic)</span></span></td>
        <td><span data-ttu-id="9286d-394">az SR-cyrillic.microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-394">sr-cyrillic.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-395">Szerb (latin betűs)</span><span class="sxs-lookup"><span data-stu-id="9286d-395">Serbian (Latin)</span></span></td>
        <td><span data-ttu-id="9286d-396">az SR-latin.microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-396">sr-latin.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-397">szlovák</span><span class="sxs-lookup"><span data-stu-id="9286d-397">Slovak</span></span></td>
        <td><span data-ttu-id="9286d-398">SK.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-398">sk.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-399">szlovén</span><span class="sxs-lookup"><span data-stu-id="9286d-399">Slovenian</span></span></td>
        <td><span data-ttu-id="9286d-400">SL.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-400">sl.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-401">spanyol</span><span class="sxs-lookup"><span data-stu-id="9286d-401">Spanish</span></span></td>
        <td><span data-ttu-id="9286d-402">es.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-402">es.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-403">es.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-403">es.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-404">svéd</span><span class="sxs-lookup"><span data-stu-id="9286d-404">Swedish</span></span></td>
        <td><span data-ttu-id="9286d-405">SV.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-405">sv.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-406">SV.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-406">sv.lucene</span></span></td>
    </tr>

    <tr>
        <td><span data-ttu-id="9286d-407">tamil</span><span class="sxs-lookup"><span data-stu-id="9286d-407">Tamil</span></span></td>
        <td><span data-ttu-id="9286d-408">TA.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-408">ta.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-409">telugu</span><span class="sxs-lookup"><span data-stu-id="9286d-409">Telugu</span></span></td>
        <td><span data-ttu-id="9286d-410">Te.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-410">te.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-411">thai</span><span class="sxs-lookup"><span data-stu-id="9286d-411">Thai</span></span></td>
        <td><span data-ttu-id="9286d-412">TH.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-412">th.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-413">TH.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-413">th.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-414">török</span><span class="sxs-lookup"><span data-stu-id="9286d-414">Turkish</span></span></td>
        <td><span data-ttu-id="9286d-415">tr.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-415">tr.microsoft</span></span></td>
        <td><span data-ttu-id="9286d-416">tr.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-416">tr.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-417">ukrán</span><span class="sxs-lookup"><span data-stu-id="9286d-417">Ukrainian</span></span></td>
        <td><span data-ttu-id="9286d-418">UK.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-418">uk.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-419">urdu</span><span class="sxs-lookup"><span data-stu-id="9286d-419">Urdu</span></span></td>
        <td><span data-ttu-id="9286d-420">Your.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-420">ur.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-421">vietnami</span><span class="sxs-lookup"><span data-stu-id="9286d-421">Vietnamese</span></span></td>
        <td><span data-ttu-id="9286d-422">VI.Microsoft</span><span class="sxs-lookup"><span data-stu-id="9286d-422">vi.microsoft</span></span></td>
        <td></td>
    </tr>
    <td colspan="3"><span data-ttu-id="9286d-423">Emellett az Azure Search biztosít nyelvtől független analyzer konfigurációk</span><span class="sxs-lookup"><span data-stu-id="9286d-423">Additionally Azure Search provides language-agnostic analyzer configurations</span></span></td>
    <tr>
        <td><span data-ttu-id="9286d-424">A szabványos ASCII-részekre bontás</span><span class="sxs-lookup"><span data-stu-id="9286d-424">Standard ASCII Folding</span></span></td>
        <td><span data-ttu-id="9286d-425">standardasciifolding.lucene</span><span class="sxs-lookup"><span data-stu-id="9286d-425">standardasciifolding.lucene</span></span></td>
        <td>
        <ul>
            <li><span data-ttu-id="9286d-426">Unicode szöveg Szegmentálás (szabványos jogkivonatokat létrehozó)</span><span class="sxs-lookup"><span data-stu-id="9286d-426">Unicode text segmentation (Standard Tokenizer)</span></span></li>
            <li><span data-ttu-id="9286d-427">ASCII modellrész-létrehozási szűrő - konvertálja, amely nem tartozik a toohello készlete először 127 ASCII-karaktereket az ASCII megfelelő Unicode-karaktereket.</span><span class="sxs-lookup"><span data-stu-id="9286d-427">ASCII folding filter - converts Unicode characters that don't belong toohello set of first 127 ASCII characters into their ASCII equivalents.</span></span> <span data-ttu-id="9286d-428">Ez akkor hasznos, a ékezetek eltávolításához.</span><span class="sxs-lookup"><span data-stu-id="9286d-428">This is useful for removing diacritics.</span></span></li>
        </ul>
        </td>
    </tr>
</table>

<span data-ttu-id="9286d-429">Nevek attribútummal rendelkező összes elemzőkkel <i>lucene</i> szerint vannak kapcsolva [Apache Lucene nyelvi elemzőkkel](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html).</span><span class="sxs-lookup"><span data-stu-id="9286d-429">All analyzers with names annotated with <i>lucene</i> are powered by [Apache Lucene's language analyzers](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html).</span></span> <span data-ttu-id="9286d-430">További információ a szűrő részekre hello ASCII található [Itt](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).</span><span class="sxs-lookup"><span data-stu-id="9286d-430">More information about hello ASCII folding filter can be found [here](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).</span></span>

<span data-ttu-id="9286d-431">**Javaslattevők**</span><span class="sxs-lookup"><span data-stu-id="9286d-431">**Suggesters**</span></span>

<span data-ttu-id="9286d-432">A `suggester` határozza meg, melyik mezőkre index használt toosupport automatikusan hajthat végre keresést az.</span><span class="sxs-lookup"><span data-stu-id="9286d-432">A `suggester` defines which fields in an index are used toosupport auto-complete in searches.</span></span> <span data-ttu-id="9286d-433">Általában részleges keresőkifejezések toohello küldött [javaslatok API](#Suggestions) hello felhasználó éppen gépel egy keresési lekérdezés, és hello API javasolt kifejezések készletét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="9286d-433">Typically partial search strings are sent toohello [Suggestions API](#Suggestions) while hello user is typing a search query, and hello API returns a set of suggested phrases.</span></span> <span data-ttu-id="9286d-434">A javaslattevő hello indexet meghatározó meghatározza, melyik mezőkre használt toobuild hello begépelt keresési feltételeket.</span><span class="sxs-lookup"><span data-stu-id="9286d-434">A suggester that you define in hello index determines which fields are used toobuild hello type-ahead search terms.</span></span> <span data-ttu-id="9286d-435">Lásd: [Javaslattevők](#Suggesters) konfigurációs részletek.</span><span class="sxs-lookup"><span data-stu-id="9286d-435">See [Suggesters](#Suggesters) for configuration details.</span></span>

<span data-ttu-id="9286d-436">**Pontozási profilok**</span><span class="sxs-lookup"><span data-stu-id="9286d-436">**Scoring profiles**</span></span>

<span data-ttu-id="9286d-437">A `scoringProfile` egyéni pontozási viselkedéseket, amelyek lehetővé teszik, hogy befolyásolják mely elemek jelennek meg a találatok között hello magasabb határozza meg.</span><span class="sxs-lookup"><span data-stu-id="9286d-437">A `scoringProfile` defines custom scoring behaviors that let you influence which items appear higher in hello search results.</span></span> <span data-ttu-id="9286d-438">A pontozási profil mező súlyok és funkciók épülnek fel.</span><span class="sxs-lookup"><span data-stu-id="9286d-438">Scoring profiles are made up of field weights and functions.</span></span> <span data-ttu-id="9286d-439">toouse őket, a lekérdezési karakterlánc hello név szerint adjon meg egy profilt.</span><span class="sxs-lookup"><span data-stu-id="9286d-439">toouse them, you specify a profile by name on hello query string.</span></span>

<span data-ttu-id="9286d-440">A pontozási profil alapértelmezett hello színfalak toocompute mögött működik, minden eleme egy eredményhalmaz keresési pontszámot.</span><span class="sxs-lookup"><span data-stu-id="9286d-440">A default scoring profile operates behind hello scenes toocompute a search score for every item in a result set.</span></span> <span data-ttu-id="9286d-441">Hello belső, névtelen pontozási profil is használhat.</span><span class="sxs-lookup"><span data-stu-id="9286d-441">You can use hello internal, unnamed scoring profile.</span></span> <span data-ttu-id="9286d-442">Másik lehetőségként állítsa `defaultScoringProfile` toouse hello alapértelmezett, amikor egyéni profil nincs megadva hello lekérdezési karakterláncot a meghívott egyéni profil.</span><span class="sxs-lookup"><span data-stu-id="9286d-442">Alternatively, set `defaultScoringProfile` toouse a custom profile as hello default, invoked whenever a custom profile is not specified on hello query string.</span></span>

<span data-ttu-id="9286d-443">Lásd: [Hozzáadás pontozási profilok tooa search-index (Azure Search szolgáltatás REST API)](search-api-scoring-profiles-2015-02-28-preview.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="9286d-443">See [Add scoring profiles tooa search index (Azure Search Service REST API)](search-api-scoring-profiles-2015-02-28-preview.md) for details.</span></span>

<span data-ttu-id="9286d-444">**A CORS-beállítások**</span><span class="sxs-lookup"><span data-stu-id="9286d-444">**CORS Options**</span></span>

<span data-ttu-id="9286d-445">Ügyféloldali Javascript nem hívható bármely API-k alapértelmezés szerint, mert hello böngésző megakadályozza, hogy minden eltérő eredetű kérések.</span><span class="sxs-lookup"><span data-stu-id="9286d-445">Client-side Javascript cannot call any APIs by default since hello browser will prevent all cross-origin requests.</span></span> <span data-ttu-id="9286d-446">Engedélyezze a CORS (eltérő eredetű erőforrások megosztása) beállítás hello `corsOptions` attribútum tooallow eltérő eredetű lekérdezések tooyour index.</span><span class="sxs-lookup"><span data-stu-id="9286d-446">Enable CORS (Cross-Origin Resource Sharing) by setting hello `corsOptions` attribute tooallow cross-origin queries tooyour index.</span></span> <span data-ttu-id="9286d-447">Vegye figyelembe, hogy egyetlen lekérdezés biztonsági okokból a CORS API-k támogatása.</span><span class="sxs-lookup"><span data-stu-id="9286d-447">Note that only query APIs support CORS for security reasons.</span></span> <span data-ttu-id="9286d-448">a következő beállítások hello CORS állítható be:</span><span class="sxs-lookup"><span data-stu-id="9286d-448">hello following options can be set for CORS:</span></span>

* <span data-ttu-id="9286d-449">`allowedOrigins`(kötelező): a lista tartalmazza, amelyek kapnak hozzáférést tooyour index.</span><span class="sxs-lookup"><span data-stu-id="9286d-449">`allowedOrigins` (required): This is a list of origins that will be granted access tooyour index.</span></span> <span data-ttu-id="9286d-450">Ez azt jelenti, hogy bármelyik Javascript-kód azokat az eredeteket helyről lesz engedélyezett tooquery az indexét (feltéve, hogy biztosít hello megfelelő API-kulcsot).</span><span class="sxs-lookup"><span data-stu-id="9286d-450">This means that any Javascript code served from those origins will be allowed tooquery your index (assuming it provides hello correct API key).</span></span> <span data-ttu-id="9286d-451">Eredetek formátuma általában hello űrlap `protocol://fully-qualified-domain-name:port` bár hello port gyakran nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="9286d-451">Each origin is typically of hello form `protocol://fully-qualified-domain-name:port` although hello port is often omitted.</span></span> <span data-ttu-id="9286d-452">Lásd: [Ez a cikk](http://go.microsoft.com/fwlink/?LinkId=330822) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="9286d-452">See [this article](http://go.microsoft.com/fwlink/?LinkId=330822) for more details.</span></span>
  * <span data-ttu-id="9286d-453">Ha azt szeretné, hogy tooallow hozzáférés tooall források, `*` hello egyetlen elemként `allowedOrigins` tömb.</span><span class="sxs-lookup"><span data-stu-id="9286d-453">If you want tooallow access tooall origins, include `*` as a single item in hello `allowedOrigins` array.</span></span> <span data-ttu-id="9286d-454">Vegye figyelembe, hogy **ez nem ajánlott eljárás a termelési keresési szolgáltatások.**</span><span class="sxs-lookup"><span data-stu-id="9286d-454">Note that **this is not recommended practice for production search services.**</span></span> <span data-ttu-id="9286d-455">Azonban hasznos lehet a fejlesztési vagy a hibakeresési célra.</span><span class="sxs-lookup"><span data-stu-id="9286d-455">However, it may be useful for development or debugging purposes.</span></span>
* <span data-ttu-id="9286d-456">`maxAgeInSeconds`(nem kötelező): a böngészők arra használják ezt érték toodetermine hello időtartama (másodpercben) toocache CORS elővizsgálati válaszok.</span><span class="sxs-lookup"><span data-stu-id="9286d-456">`maxAgeInSeconds` (optional): Browsers use this value toodetermine hello duration (in seconds) toocache CORS preflight responses.</span></span> <span data-ttu-id="9286d-457">Ez nem negatív egész számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9286d-457">This must be a non-negative integer.</span></span> <span data-ttu-id="9286d-458">hello nagyobb az értéke, hello jobb teljesítmény, de hello hosszabb ideig fog tartani, amíg a CORS házirend módosításait tootake hatást.</span><span class="sxs-lookup"><span data-stu-id="9286d-458">hello larger this value is, hello better performance will be, but hello longer it will take for CORS policy changes tootake effect.</span></span> <span data-ttu-id="9286d-459">Ha nincs megadva, a alapértelmezett 5 perces időtartamot használható.</span><span class="sxs-lookup"><span data-stu-id="9286d-459">If it is not set, a default duration of 5 minutes will be used.</span></span>

<span data-ttu-id="9286d-460"><a name="CreateUpdateIndexExample"></a>
**Kérelem törzse – példa**</span><span class="sxs-lookup"><span data-stu-id="9286d-460"><a name="CreateUpdateIndexExample"></a>
**Request Body Example**</span></span>

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

<span data-ttu-id="9286d-461">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="9286d-461">**Response**</span></span>

<span data-ttu-id="9286d-462">A kérelem sikeres: "201 Created".</span><span class="sxs-lookup"><span data-stu-id="9286d-462">For a successful request: "201 Created".</span></span>

<span data-ttu-id="9286d-463">Alapértelmezés szerint hello adott válasz törzsének hello JSON a létrehozott hello Indexdefiníció fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="9286d-463">By default hello response body will contain hello JSON for hello index definition that was created.</span></span> <span data-ttu-id="9286d-464">Ha hello `Prefer` kérelem fejléce túl van-e állítva`return=minimal`, hello adott válasz törzse üres lesz, és hello sikeres állapotkód lesz "204 Nincs tartalom" helyett "201 Created".</span><span class="sxs-lookup"><span data-stu-id="9286d-464">If hello `Prefer` request header is set too`return=minimal`, hello response body will be empty and hello success status code will be "204 No Content" instead of "201 Created".</span></span> <span data-ttu-id="9286d-465">Ez érvényét veszti, függetlenül attól, hogy PUT vagy POST lett használt toocreate hello index.</span><span class="sxs-lookup"><span data-stu-id="9286d-465">This is true regardless of whether PUT or POST was used toocreate hello index.</span></span>

<span data-ttu-id="9286d-466">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="9286d-466">**Remarks**</span></span>

<span data-ttu-id="9286d-467">Jelenleg korlátozott támogatása az index sémafrissítések.</span><span class="sxs-lookup"><span data-stu-id="9286d-467">Currently, there is limited support for index schema updates.</span></span> <span data-ttu-id="9286d-468">Bármely sémafrissítések újraindexelés, például a mezőtípusok módosítása újraindexelést igénylő jelenleg nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="9286d-468">Any schema updates that would require re-indexing such as changing field types are not currently supported.</span></span> <span data-ttu-id="9286d-469">Bár a már meglévő mezők nem módosítható vagy törölhető, új mezők hozzáadása tooan létező indexek bármikor.</span><span class="sxs-lookup"><span data-stu-id="9286d-469">Although existing fields cannot be changed or deleted, new fields can be added tooan existing index at any time.</span></span> <span data-ttu-id="9286d-470">Új mező felvételekor hello index összes meglévő dokumentumok automatikusan fog rendelkezni a mező null értéket.</span><span class="sxs-lookup"><span data-stu-id="9286d-470">When a new field is added, all existing documents in hello index will automatically have a null value for that field.</span></span> <span data-ttu-id="9286d-471">További tárhely az új dokumentumok toohello index felvételének befejezéséig fognak használni.</span><span class="sxs-lookup"><span data-stu-id="9286d-471">No additional storage space will be consumed until new documents are added toohello index.</span></span>

<a name="Suggesters"></a>

## <a name="suggesters"></a><span data-ttu-id="9286d-472">Javaslattevők</span><span class="sxs-lookup"><span data-stu-id="9286d-472">Suggesters</span></span>
<span data-ttu-id="9286d-473">hello javaslatok az Azure Search szolgáltatása begépelt vagy automatikusan hajthat végre a lekérdezés egy olyan képességet, válasz toopartial karakterlánc bemenetek jegyez be a keresőmezőbe a potenciális keresési feltételeinek megadása.</span><span class="sxs-lookup"><span data-stu-id="9286d-473">hello suggestions feature in Azure Search is a type-ahead or auto-complete query capability, providing a list of potential search terms in response toopartial string inputs entered into a search box.</span></span> <span data-ttu-id="9286d-474">Bizonyára észrevette konstrukciókat kereskedelmi keresőmotorok használatakor: írja be a ".NET" Bing előállító feltételei listájának ".NET 4.5-ös", ".NET-keretrendszer 3.5-ös", és így tovább.</span><span class="sxs-lookup"><span data-stu-id="9286d-474">You've probably noticed query suggestions when using commercial web search engines: typing ".NET" in Bing produces a list of terms for ".NET 4.5", ".NET Framework 3.5", and so forth.</span></span> <span data-ttu-id="9286d-475">Hello keresési szolgáltatás REST API használata esetén hello következő javaslatokat valósít meg egyéni Azure Search alkalmazás szükséges:</span><span class="sxs-lookup"><span data-stu-id="9286d-475">When using hello Search service REST API, implementing suggestions in a custom Azure Search application requires hello following:</span></span>

* <span data-ttu-id="9286d-476">Engedélyezze a javaslatok hozzáadása egy **javaslattevő** meghívták az indexben, amely hello nevét, a keresési mód és a mezőket sorolja fel, amelynek begépelt konstrukció.</span><span class="sxs-lookup"><span data-stu-id="9286d-476">Enable suggestions by adding a **suggester** construction in your index, giving hello name, search mode, and a list of fields for which type-ahead is invoked.</span></span> <span data-ttu-id="9286d-477">Például ha megadja a "város" mező, írja be a "Tenger" részleges keresési karakterlánc a "Seattle" eredményez, "Tengerpart" és "Seatac" (mind a hármat tényleges városnév) kínált lekérdezés javaslatok toohello felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="9286d-477">For example, if you specify "cityName" as a source field, typing a partial search string of "Sea" will result in "Seattle", "Seaside", and "Seatac" (all three are actual city names) offered up as query suggestions toohello user.</span></span>
* <span data-ttu-id="9286d-478">Javaslatok meghívásához hívó hello [javaslatok API](#Suggestions) az alkalmazás kódjában.</span><span class="sxs-lookup"><span data-stu-id="9286d-478">Invoke suggestions by calling hello [Suggestions API](#Suggestions) in your application code.</span></span> <span data-ttu-id="9286d-479">Általában részleges keresőkifejezések küldött toohello szolgáltatás hello felhasználó éppen gépel egy keresési lekérdezés, és ez az API javasolt kifejezések készletét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="9286d-479">Typically partial search strings are sent toohello service while hello user is typing a search query, and this API returns a set of suggested phrases.</span></span>

<span data-ttu-id="9286d-480">Ez a cikk azt ismerteti, hogyan tooconfigure egy **javaslattevő**.</span><span class="sxs-lookup"><span data-stu-id="9286d-480">This article explains how tooconfigure a **suggester**.</span></span> <span data-ttu-id="9286d-481">Olvassa el a hello [javaslatok API](#Suggestions) talál részletes információt a javaslattevő használatáról.</span><span class="sxs-lookup"><span data-stu-id="9286d-481">You should also review hello [Suggestions API](#Suggestions) for details on how a suggester is used.</span></span>

<span data-ttu-id="9286d-482">**Használat**</span><span class="sxs-lookup"><span data-stu-id="9286d-482">**Usage**</span></span>

<span data-ttu-id="9286d-483">`Suggesters`hello index létrehozása és a legjobban, ha a használt toosuggest specifikus dokumentumok ahelyett, hogy laza szót vagy kifejezést.</span><span class="sxs-lookup"><span data-stu-id="9286d-483">`Suggesters` are created in hello index and work best when used toosuggest specific documents rather than loose terms or phrases.</span></span> <span data-ttu-id="9286d-484">hello legjobb jelölt mezők kitöltése, címek, nevek és egyéb viszonylag rövid kifejezés elem azonosításra alkalmas.</span><span class="sxs-lookup"><span data-stu-id="9286d-484">hello best candidate fields are titles, names, and other relatively short phrases that can identify an item.</span></span> <span data-ttu-id="9286d-485">Kevésbé hatékony olyan ismétlődő, például a kategóriák és a címkék, és nagyon hosszú mezők például leírásokat és megjegyzéseket mezőket.</span><span class="sxs-lookup"><span data-stu-id="9286d-485">Less effective are repetitive fields, such as categories and tags, or very long fields such as descriptions or comments fields.</span></span>

<span data-ttu-id="9286d-486">Hello Indexdefiníció részeként is hozzáadhat egy egyetlen javaslattevő toohello `suggesters` gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="9286d-486">As part of hello index definition, you can add a single suggester toohello `suggesters` collection.</span></span> <span data-ttu-id="9286d-487">A javaslattevő meghatározó tulajdonságok hello alábbiakat foglalja magába:</span><span class="sxs-lookup"><span data-stu-id="9286d-487">Properties that define a suggester include hello following:</span></span>

* <span data-ttu-id="9286d-488">`name`: hello hello javaslattevő nevét.</span><span class="sxs-lookup"><span data-stu-id="9286d-488">`name`: hello name of hello suggester.</span></span> <span data-ttu-id="9286d-489">Hello hello javaslattevő nevét használja hello meghívásakor `suggest` API.</span><span class="sxs-lookup"><span data-stu-id="9286d-489">You use hello name of hello suggester when calling hello `suggest` API.</span></span>
* <span data-ttu-id="9286d-490">`searchMode`: a jelölt kifejezések használt stratégia toosearch hello.</span><span class="sxs-lookup"><span data-stu-id="9286d-490">`searchMode`: hello strategy used toosearch for candidate phrases.</span></span> <span data-ttu-id="9286d-491">hello jelenleg egyetlen támogatott mód van `analyzingInfixMatching`, melyik teljesít kifejezések hello elején vagy mondat közepén hello rugalmas megfelelő.</span><span class="sxs-lookup"><span data-stu-id="9286d-491">hello only mode currently supported is `analyzingInfixMatching`, which performs flexible matching of phrases at hello beginning or in hello middle of sentences.</span></span>
* <span data-ttu-id="9286d-492">`sourceFields`: Egy vagy több mezőinek listáját, amelyek javaslatok hello tartalmának hello forrását.</span><span class="sxs-lookup"><span data-stu-id="9286d-492">`sourceFields`: A list of one or more fields that are hello source of hello content for suggestions.</span></span> <span data-ttu-id="9286d-493">Csak típusú mezők `Edm.String` és `Collection(Edm.String)` lehet, hogy javaslatokat adatforrásait.</span><span class="sxs-lookup"><span data-stu-id="9286d-493">Only fields of type `Edm.String` and `Collection(Edm.String)` may be sources for suggestions.</span></span> <span data-ttu-id="9286d-494">Csak olyan mezőket, amelyekre nincs beállítva egyéni nyelvi elemzőt is használható.</span><span class="sxs-lookup"><span data-stu-id="9286d-494">Only fields that don't have a custom language analyzer set can be used.</span></span>

<span data-ttu-id="9286d-495">**A javaslattevő – példa**</span><span class="sxs-lookup"><span data-stu-id="9286d-495">**Suggester Example**</span></span>

<span data-ttu-id="9286d-496">A javaslattevő hello index részét képezi.</span><span class="sxs-lookup"><span data-stu-id="9286d-496">A suggester is part of hello index.</span></span> <span data-ttu-id="9286d-497">Csak egy javaslattevő létezhet hello `suggesters` hello jelenlegi verziójával együtt hello gyűjtemény mezők gyűjtemény és `scoringProfiles`.</span><span class="sxs-lookup"><span data-stu-id="9286d-497">Only one suggester can exist in hello `suggesters` collection in hello current version, alongside hello fields collection and `scoringProfiles`.</span></span>

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [!NOTE]
> <span data-ttu-id="9286d-498">Ha használta az Azure Search hello nyilvános előzetes verziójának `suggesters` a felváltja egy régebbi logikai tulajdonság (`"suggestions": false`), amely csak a támogatott előtag javaslatok rövid karakterláncok (a 3-25 karakterből álló).</span><span class="sxs-lookup"><span data-stu-id="9286d-498">If you used hello public preview version of Azure Search, `suggesters` replaces an older boolean property (`"suggestions": false`) that only supported prefix suggestions for short strings (3-25 characters).</span></span> <span data-ttu-id="9286d-499">A csere `suggesters`, támogatja a infix megfelelő megtalált jobb tűréssel tévesztések a keresési karakterláncot az egyező kifejezések hello elején vagy közepén hello mező tartalmát.</span><span class="sxs-lookup"><span data-stu-id="9286d-499">Its replacement, `suggesters`, supports infix matching that finds matching terms at hello beginning or in hello middle of field content, with better tolerance for mistakes in search strings.</span></span> <span data-ttu-id="9286d-500">Hello általánosan elérhető kiadástól kezdve, ez már hello hello javaslatok API csak végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="9286d-500">Starting with hello generally available release, this is now hello only implementation of hello suggestions API.</span></span> <span data-ttu-id="9286d-501">hello régebbi `suggestions` bevezetett tulajdonság `api-version=2014-07-31-Preview` toowork ebben a verzióban továbbra is fennáll, de nincs hello operatív `2015-02-28` vagy az Azure Search újabb verzióit.</span><span class="sxs-lookup"><span data-stu-id="9286d-501">hello older `suggestions` property that was introduced in `api-version=2014-07-31-Preview` continues toowork in that version, but is not operational in hello `2015-02-28` or later versions of Azure Search.</span></span>
> 
> 

<a name="UpdateIndex"></a>

## <a name="update-index"></a><span data-ttu-id="9286d-502">Index frissítése</span><span class="sxs-lookup"><span data-stu-id="9286d-502">Update Index</span></span>
<span data-ttu-id="9286d-503">Frissítheti a meglévő index belül Azure Search használatával egy HTTP PUT-kérelmet.</span><span class="sxs-lookup"><span data-stu-id="9286d-503">You can update an existing index within Azure Search using an HTTP PUT request.</span></span> <span data-ttu-id="9286d-504">Frissítések lehetnek, hozzáadása új mezők toohello meglévő séma, a CORS-beállítások módosítását, és a pontozási profil módosítása.</span><span class="sxs-lookup"><span data-stu-id="9286d-504">Updates can include adding new fields toohello existing schema, modifying CORS options, and modifying scoring profiles.</span></span> <span data-ttu-id="9286d-505">Lásd: [hozzáadása a pontozási profil](https://msdn.microsoft.com/library/azure/dn798928.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="9286d-505">See [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for more information.</span></span> <span data-ttu-id="9286d-506">Hello index tooupdate hello neve meg hello kérelem URI-azonosítója:</span><span class="sxs-lookup"><span data-stu-id="9286d-506">You specify hello name of hello index tooupdate on hello request URI:</span></span>

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="9286d-507">**Fontos:** index sémafrissítések támogatása, amelyek nem igényelnek hello search-index újraépítése korlátozott toooperations.</span><span class="sxs-lookup"><span data-stu-id="9286d-507">**Important:** Support for index schema updates is limited toooperations that don't require rebuilding hello search index.</span></span> <span data-ttu-id="9286d-508">Bármely sémafrissítések műveletek, például a mezőtípusok módosítása újraindexelést igénylő jelenleg nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="9286d-508">Any schema updates that would require re-indexing, such as changing field types, are not currently supported.</span></span> <span data-ttu-id="9286d-509">Új mezők hozzáadása bármikor, noha a már meglévő mezők nem módosítható vagy törölhető.</span><span class="sxs-lookup"><span data-stu-id="9286d-509">New fields can be added at any time, although existing fields cannot be changed or deleted.</span></span> <span data-ttu-id="9286d-510">hello Ugyanez vonatkozik túl`suggesters`.</span><span class="sxs-lookup"><span data-stu-id="9286d-510">hello same applies too`suggesters`.</span></span> <span data-ttu-id="9286d-511">Új mezők adhatók hozzá tooa javaslattevő: hello idő hello mezőket ad hozzá, de a mezők nem távolítható el `suggesters` és a már meglévő mezők nem adhatók meg túl`suggesters`.</span><span class="sxs-lookup"><span data-stu-id="9286d-511">New fields may be added tooa suggester at hello time hello fields are added, but fields cannot be removed from `suggesters` and existing fields cannot be added too`suggesters`.</span></span>

<span data-ttu-id="9286d-512">Egy új tooan mezőindex felvételekor hello index összes meglévő dokumentumok automatikusan lesz mező null értékű.</span><span class="sxs-lookup"><span data-stu-id="9286d-512">When adding a new field tooan index, all existing documents in hello index will automatically have a null value for that field.</span></span> <span data-ttu-id="9286d-513">További tárhely az új dokumentumok toohello index felvételének befejezéséig fognak használni.</span><span class="sxs-lookup"><span data-stu-id="9286d-513">No additional storage space will be consumed until new documents are added toohello index.</span></span>

<span data-ttu-id="9286d-514">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="9286d-514">**Request**</span></span>

<span data-ttu-id="9286d-515">HTTPS-kapcsolat szükséges az összes szolgáltatási kérelmek.</span><span class="sxs-lookup"><span data-stu-id="9286d-515">HTTPS is required for all service requests.</span></span> <span data-ttu-id="9286d-516">Hello **Index frissítése** segítségével a HTTP PUT kérés jön létre.</span><span class="sxs-lookup"><span data-stu-id="9286d-516">hello **Update Index** request is constructed using HTTP PUT.</span></span> <span data-ttu-id="9286d-517">A PUT hello indexnév része hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="9286d-517">With PUT, hello index name is part of hello URL.</span></span> <span data-ttu-id="9286d-518">Ha hello index nem létezik, akkor jön létre.</span><span class="sxs-lookup"><span data-stu-id="9286d-518">If hello index doesn't exist, it is created.</span></span> <span data-ttu-id="9286d-519">Hello index már létezik, akkor új frissített toohello-definíció.</span><span class="sxs-lookup"><span data-stu-id="9286d-519">If hello index already exists, it is updated toohello new definition.</span></span>

<span data-ttu-id="9286d-520">hello indexnév kell kell kisbetű, betűvel vagy számmal kezdődhet, nincs perjeleket vagy pontokból és 128 karakternél rövidebb lehet.</span><span class="sxs-lookup"><span data-stu-id="9286d-520">hello index name must be lower case, start with a letter or number, have no slashes or dots, and be less than 128 characters.</span></span> <span data-ttu-id="9286d-521">Hello az index nevének betűvel vagy számmal verziótól kezdődően után hello részeinek hello neve tartalmazhatnak bármely betű, szám és kötőjeleket, mindaddig, amíg nincsenek egymást követő kötőjeleket hello.</span><span class="sxs-lookup"><span data-stu-id="9286d-521">After starting hello index name with a letter or number, hello rest of hello name can include any letter, number and dashes, as long as hello dashes are not consecutive.</span></span>

<span data-ttu-id="9286d-522">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="9286d-522">`api-version=[string]` (required).</span></span> <span data-ttu-id="9286d-523">hello előzetes verziója `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9286d-523">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9286d-524">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="9286d-524">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9286d-525">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="9286d-525">**Request Headers**</span></span>

<span data-ttu-id="9286d-526">a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9286d-526">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9286d-527">`Content-Type`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="9286d-527">`Content-Type`: Required.</span></span> <span data-ttu-id="9286d-528">Állítsa ezt túl`application/json`</span><span class="sxs-lookup"><span data-stu-id="9286d-528">Set this too`application/json`</span></span>
* <span data-ttu-id="9286d-529">`api-key`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="9286d-529">`api-key`: Required.</span></span> <span data-ttu-id="9286d-530">Hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9286d-530">hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9286d-531">Egy karakterláncértéket, egyedi tooyour szolgáltatást is.</span><span class="sxs-lookup"><span data-stu-id="9286d-531">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="9286d-532">Hello **Index frissítése** kérelem tartalmaznia kell egy `api-key` fejlécet beállítani tooyour adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).</span><span class="sxs-lookup"><span data-stu-id="9286d-532">hello **Update Index** request must include an `api-key` header set tooyour admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9286d-533">Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="9286d-533">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9286d-534">Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9286d-534">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9286d-535">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="9286d-535">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9286d-536">**Törzs szintaxis**</span><span class="sxs-lookup"><span data-stu-id="9286d-536">**Request Body Syntax**</span></span>

<span data-ttu-id="9286d-537">Amikor frissíti a létező indexek, hello törzs tartalmaznia kell hello eredeti sémadefiníciót, valamint hello új mezőket ad hozzá, valamint hello módosította a pontozási profil, javaslattevők és a CORS-beállítások, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="9286d-537">When updating an existing index, hello body must include hello original schema definition, plus hello new fields you are adding, as well as hello modified scoring profiles, suggesters and CORS options, if any.</span></span> <span data-ttu-id="9286d-538">Ha nem módosítja a pontozási profil hello és a CORS-beállítások, meg kell adnia a hello index létrehozásának hello eredeti.</span><span class="sxs-lookup"><span data-stu-id="9286d-538">If you are not modifying hello scoring profiles and CORS options, you must include hello originals from when hello index was created.</span></span> <span data-ttu-id="9286d-539">Hello legjobb mintát toouse frissítések általában tooretrieve hello index definícióját a GET, módosítsa, majd frissíti a PUT.</span><span class="sxs-lookup"><span data-stu-id="9286d-539">In general hello best pattern toouse for updates is tooretrieve hello index definition with a GET, modify it, then update it with PUT.</span></span>

<span data-ttu-id="9286d-540">hello sémaszintaxis itt jelenik meg az index toocreate használt kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="9286d-540">hello schema syntax used toocreate an index is reproduced here for convenience.</span></span> <span data-ttu-id="9286d-541">Lásd: [a Create Index](#CreateIndex) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="9286d-541">See [Create Index](#CreateIndex) for more details.</span></span>

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of hello analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of hello search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of hello indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in hello future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies toosearchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading toonow over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


<span data-ttu-id="9286d-542">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="9286d-542">**Response**</span></span>

<span data-ttu-id="9286d-543">A kérelem sikeres: "204 Nincs tartalom".</span><span class="sxs-lookup"><span data-stu-id="9286d-543">For a successful request: "204 No Content".</span></span>

<span data-ttu-id="9286d-544">Alapértelmezés szerint hello adott válasz törzse üres lesz.</span><span class="sxs-lookup"><span data-stu-id="9286d-544">By default hello response body will be empty.</span></span> <span data-ttu-id="9286d-545">Azonban, ha hello `Prefer` kérelem fejléce túl van-e állítva`return=representation`, hello adott válasz törzsének hello JSON a frissített hello Indexdefiníció fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="9286d-545">However, if hello `Prefer` request header is set too`return=representation`, hello response body will contain hello JSON for hello index definition that was updated.</span></span> <span data-ttu-id="9286d-546">Ebben az esetben lesz hello sikeres állapotkód: "200 OK".</span><span class="sxs-lookup"><span data-stu-id="9286d-546">In this case, hello success status code will be "200 OK".</span></span>

<span data-ttu-id="9286d-547">**Indexdefiníció egyéni lekérdezések frissítésekor**</span><span class="sxs-lookup"><span data-stu-id="9286d-547">**Updating index definition with custom analyzers**</span></span>

<span data-ttu-id="9286d-548">Miután egy elemző eszköz, a jogkivonatokat létrehozó, egy token szűrőt, vagy egy karakter szűrő be van állítva, nem lehet módosítani.</span><span class="sxs-lookup"><span data-stu-id="9286d-548">Once an analyzer, a tokenizer, a token filter or a char filter is defined, it cannot be modified.</span></span> <span data-ttu-id="9286d-549">Újakat tooan meglévő-index vehetők, csak ha hello `allowIndexDowntime` jelző be van állítva tootrue hello index frissítési kérelem:</span><span class="sxs-lookup"><span data-stu-id="9286d-549">New ones can be added tooan existing index only if hello `allowIndexDowntime` flag is set tootrue in hello index update request:</span></span> 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

<span data-ttu-id="9286d-550">Vegye figyelembe, hogy ez a művelet kerül-e az index offline legalább néhány másodperc, ami az indexelési és lekérdezés toofail kéri.</span><span class="sxs-lookup"><span data-stu-id="9286d-550">Note that this operation will put your index offline for at least a few seconds, causing your indexing and query requests toofail.</span></span> <span data-ttu-id="9286d-551">Teljesítmény- és írási rendelkezésre állási hello index korlátozott hello index frissítése után több percig vagy nagyon nagy indexek hosszabb lehet.</span><span class="sxs-lookup"><span data-stu-id="9286d-551">Performance and write availability of hello index can be impaired for several minutes after hello index is updated, or longer for very large indexes.</span></span>

<a name="ListIndexes"></a>

## <a name="list-indexes"></a><span data-ttu-id="9286d-552">Lista indexek</span><span class="sxs-lookup"><span data-stu-id="9286d-552">List Indexes</span></span>
<span data-ttu-id="9286d-553">Hello **lista indexek** művelet listáját adja vissza hello indexei jelenleg az Azure Search szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="9286d-553">hello **List Indexes** operation returns a list of hello indexes currently in your Azure Search service.</span></span>

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="9286d-554">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="9286d-554">**Request**</span></span>

<span data-ttu-id="9286d-555">HTTPS-kapcsolat szükséges az összes szolgáltatási kérelmek.</span><span class="sxs-lookup"><span data-stu-id="9286d-555">HTTPS is required for all service requests.</span></span> <span data-ttu-id="9286d-556">Hello **lista indexek** kérelmek GET metódussal hello lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9286d-556">hello **List Indexes** request can be constructed using hello GET method.</span></span>

<span data-ttu-id="9286d-557">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="9286d-557">`api-version=[string]` (required).</span></span> <span data-ttu-id="9286d-558">hello előzetes verziója `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9286d-558">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9286d-559">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="9286d-559">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9286d-560">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="9286d-560">**Request Headers**</span></span>

<span data-ttu-id="9286d-561">a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9286d-561">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9286d-562">`api-key`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="9286d-562">`api-key`: Required.</span></span> <span data-ttu-id="9286d-563">Hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9286d-563">hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9286d-564">Egy karakterláncértéket, egyedi tooyour szolgáltatást is.</span><span class="sxs-lookup"><span data-stu-id="9286d-564">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="9286d-565">Hello **lista indexek** kérelem tartalmaznia kell egy `api-key` set tooan adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).</span><span class="sxs-lookup"><span data-stu-id="9286d-565">hello **List Indexes** request must include an `api-key` set tooan admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9286d-566">Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="9286d-566">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9286d-567">Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9286d-567">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9286d-568">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="9286d-568">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9286d-569">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="9286d-569">**Request Body**</span></span>

<span data-ttu-id="9286d-570">nincs.</span><span class="sxs-lookup"><span data-stu-id="9286d-570">None.</span></span>

<span data-ttu-id="9286d-571">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="9286d-571">**Response**</span></span>

<span data-ttu-id="9286d-572">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="9286d-572">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="9286d-573">Íme egy példa adott válasz törzse:</span><span class="sxs-lookup"><span data-stu-id="9286d-573">Here is an example response body:</span></span>

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

<span data-ttu-id="9286d-574">Vegye figyelembe, hogy le toojust hello tulajdonságok kíváncsiak vagyunk hello válasz szűrheti.</span><span class="sxs-lookup"><span data-stu-id="9286d-574">Note that you can filter hello response down toojust hello properties you're interested in.</span></span> <span data-ttu-id="9286d-575">Például, ha azt szeretné, hogy csak index neveinek listáját, használja a hello OData `$select` lekérdezési lehetőség:</span><span class="sxs-lookup"><span data-stu-id="9286d-575">For example, if you want only a list of index names, use hello OData `$select` query option:</span></span>

    GET /indexes?api-version=2015-02-28-Preview&$select=name

<span data-ttu-id="9286d-576">Ebben az esetben a fenti példa hello hello válasz távolságban jelenjen meg az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9286d-576">In this case, hello response from hello above example would appear as follows:</span></span>

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

<span data-ttu-id="9286d-577">Ez az egy hasznos módszer toosave sávszélesség, ha nagy mennyiségű indexek szerepel a keresési szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="9286d-577">This is a useful technique toosave bandwidth if you have a lot of indexes in your Search service.</span></span>

<a name="GetIndex"></a>

## <a name="get-index"></a><span data-ttu-id="9286d-578">Index beolvasása</span><span class="sxs-lookup"><span data-stu-id="9286d-578">Get Index</span></span>
<span data-ttu-id="9286d-579">Hello **beolvasása Index** művelet hello index definíciójának beolvasása az Azure Search.</span><span class="sxs-lookup"><span data-stu-id="9286d-579">hello **Get Index** operation gets hello index definition from Azure Search.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="9286d-580">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="9286d-580">**Request**</span></span>

<span data-ttu-id="9286d-581">HTTPS szolgáltatáskérések szükség.</span><span class="sxs-lookup"><span data-stu-id="9286d-581">HTTPS is required for service requests.</span></span> <span data-ttu-id="9286d-582">Hello **beolvasása Index** kérelmek GET metódussal hello lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9286d-582">hello **Get Index** request can be constructed using hello GET method.</span></span>

<span data-ttu-id="9286d-583">[index neve] hello hello kérelem URI-azonosítója a határozza meg, hogy melyik index tooreturn hello indexek gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="9286d-583">hello [index name] in hello request URI specifies which index tooreturn from hello indexes collection.</span></span>

<span data-ttu-id="9286d-584">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="9286d-584">`api-version=[string]` (required).</span></span> <span data-ttu-id="9286d-585">hello előzetes verziója `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9286d-585">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9286d-586">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="9286d-586">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9286d-587">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="9286d-587">**Request Headers**</span></span>

<span data-ttu-id="9286d-588">a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9286d-588">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9286d-589">`api-key`: hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9286d-589">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9286d-590">Egy karakterláncértéket, egyedi tooyour szolgáltatást is.</span><span class="sxs-lookup"><span data-stu-id="9286d-590">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="9286d-591">Hello **beolvasása Index** kérelem tartalmaznia kell egy `api-key` set tooan adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).</span><span class="sxs-lookup"><span data-stu-id="9286d-591">hello **Get Index** request must include an `api-key` set tooan admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9286d-592">Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="9286d-592">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9286d-593">Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9286d-593">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9286d-594">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="9286d-594">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9286d-595">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="9286d-595">**Request Body**</span></span>

<span data-ttu-id="9286d-596">nincs.</span><span class="sxs-lookup"><span data-stu-id="9286d-596">None.</span></span>

<span data-ttu-id="9286d-597">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="9286d-597">**Response**</span></span>

<span data-ttu-id="9286d-598">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="9286d-598">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="9286d-599">Hello példa JSON a [létrehozása és frissítése Index](#CreateUpdateIndexExample) hello válasz hasznos példát.</span><span class="sxs-lookup"><span data-stu-id="9286d-599">See hello example JSON in [Creating and Updating an Index](#CreateUpdateIndexExample) for an example of hello response payload.</span></span>

<a name="DeleteIndex"></a>

## <a name="delete-index"></a><span data-ttu-id="9286d-600">Index törlése</span><span class="sxs-lookup"><span data-stu-id="9286d-600">Delete Index</span></span>
<span data-ttu-id="9286d-601">Hello **Index törlése** művelet eltávolítja az index és a kapcsolódó dokumentumok az Azure Search szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9286d-601">hello **Delete Index** operation removes an index and associated documents from your Azure Search service.</span></span> <span data-ttu-id="9286d-602">Hello indexnév hello szolgáltatás irányítópultján hello Azure-portálon, vagy a hello API kaphat.</span><span class="sxs-lookup"><span data-stu-id="9286d-602">You can get hello index name from hello service dashboard in hello Azure portal, or from hello API.</span></span> <span data-ttu-id="9286d-603">Lásd: [lista indexek](#ListIndexes) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="9286d-603">See [List Indexes](#ListIndexes) for details.</span></span>

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="9286d-604">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="9286d-604">**Request**</span></span>

<span data-ttu-id="9286d-605">HTTPS szolgáltatáskérések szükség.</span><span class="sxs-lookup"><span data-stu-id="9286d-605">HTTPS is required for service requests.</span></span> <span data-ttu-id="9286d-606">Hello **Index törlése** kérelem hello DELETE metódust használó lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9286d-606">hello **Delete Index** request can be constructed using hello DELETE method.</span></span>

<span data-ttu-id="9286d-607">[index neve] hello hello kérelem URI-azonosítója a határozza meg, hogy melyik index toodelete hello indexek gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="9286d-607">hello [index name] in hello request URI specifies which index toodelete from hello indexes collection.</span></span>

<span data-ttu-id="9286d-608">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="9286d-608">`api-version=[string]` (required).</span></span> <span data-ttu-id="9286d-609">hello előzetes verziója `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9286d-609">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9286d-610">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="9286d-610">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9286d-611">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="9286d-611">**Request Headers**</span></span>

<span data-ttu-id="9286d-612">a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9286d-612">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9286d-613">`api-key`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="9286d-613">`api-key`: Required.</span></span> <span data-ttu-id="9286d-614">Hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9286d-614">hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9286d-615">Egy karakterláncértéket, egyedi tooyour szolgáltatás URL-címe.</span><span class="sxs-lookup"><span data-stu-id="9286d-615">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="9286d-616">Hello **Index törlése** kérelem tartalmaznia kell egy `api-key` fejlécet beállítani tooyour adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).</span><span class="sxs-lookup"><span data-stu-id="9286d-616">hello **Delete Index** request must include an `api-key` header set tooyour admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9286d-617">Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="9286d-617">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9286d-618">Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9286d-618">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9286d-619">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="9286d-619">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9286d-620">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="9286d-620">**Request Body**</span></span>

<span data-ttu-id="9286d-621">nincs.</span><span class="sxs-lookup"><span data-stu-id="9286d-621">None.</span></span>

<span data-ttu-id="9286d-622">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="9286d-622">**Response**</span></span>

<span data-ttu-id="9286d-623">Állapotkód: 204 nem tartalom visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="9286d-623">Status Code: 204 No Content is returned for a successful response.</span></span>

<a name="GetIndexStats"></a>

## <a name="get-index-statistics"></a><span data-ttu-id="9286d-624">Megtekintheti a statisztikákat Index</span><span class="sxs-lookup"><span data-stu-id="9286d-624">Get Index Statistics</span></span>
<span data-ttu-id="9286d-625">Hello **beolvasása Index statisztika** művelet számát adja vissza az Azure Search dokumentum hello aktuális index és a tárhely kihasználtsága.</span><span class="sxs-lookup"><span data-stu-id="9286d-625">hello **Get Index Statistics** operation returns from Azure Search a document count for hello current index, plus storage usage.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> <span data-ttu-id="9286d-626">A dokumentumok száma és a tárhely mérete statisztika nem a valós idejű néhány percenként összegyűjtése.</span><span class="sxs-lookup"><span data-stu-id="9286d-626">Statistics on document count and storage size are collected every few minutes, not in real time.</span></span> <span data-ttu-id="9286d-627">Ezért ez az API által visszaadott hello statisztika nem tükrözik legutóbbi indexelési műveletek okozta módosítások.</span><span class="sxs-lookup"><span data-stu-id="9286d-627">Therefore, hello statistics returned by this API may not reflect changes caused by recent indexing operations.</span></span>
> 
> 

<span data-ttu-id="9286d-628">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="9286d-628">**Request**</span></span>

<span data-ttu-id="9286d-629">HTTPS-kapcsolat szükséges szolgáltatások minden olyan kérelem esetében.</span><span class="sxs-lookup"><span data-stu-id="9286d-629">HTTPS is required for all services requests.</span></span> <span data-ttu-id="9286d-630">Hello **beolvasása Index statisztika** kérelmek GET metódussal hello lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9286d-630">hello **Get Index Statistics** request can be constructed using hello GET method.</span></span>

<span data-ttu-id="9286d-631">[index neve] hello hello kérelem URI-azonosítója a hello szolgáltatás tooreturn index statisztika hello megadott index alapján.</span><span class="sxs-lookup"><span data-stu-id="9286d-631">hello [index name] in hello request URI tells hello service tooreturn index statistics for hello specified index.</span></span>

<span data-ttu-id="9286d-632">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="9286d-632">`api-version=[string]` (required).</span></span> <span data-ttu-id="9286d-633">hello előzetes verziója `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9286d-633">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9286d-634">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="9286d-634">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9286d-635">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="9286d-635">**Request Headers**</span></span>

<span data-ttu-id="9286d-636">a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9286d-636">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9286d-637">`api-key`: hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9286d-637">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9286d-638">Egy karakterláncértéket, egyedi tooyour szolgáltatást is.</span><span class="sxs-lookup"><span data-stu-id="9286d-638">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="9286d-639">Hello **beolvasása Index statisztika** kérelem tartalmaznia kell egy `api-key` set tooan adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).</span><span class="sxs-lookup"><span data-stu-id="9286d-639">hello **Get Index Statistics** request must include an `api-key` set tooan admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9286d-640">Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="9286d-640">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9286d-641">Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9286d-641">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9286d-642">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="9286d-642">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9286d-643">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="9286d-643">**Request Body**</span></span>

<span data-ttu-id="9286d-644">nincs.</span><span class="sxs-lookup"><span data-stu-id="9286d-644">None.</span></span>

<span data-ttu-id="9286d-645">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="9286d-645">**Response**</span></span>

<span data-ttu-id="9286d-646">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="9286d-646">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="9286d-647">hello adott válasz törzsének hello a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="9286d-647">hello response body is in hello following format:</span></span>

    {
      "documentCount": number,
      "storageSize": number (size of hello index in bytes)
    }

<a name="TestAnalyzer"></a>

## <a name="test-analyzer"></a><span data-ttu-id="9286d-648">Teszt elemző eszköz</span><span class="sxs-lookup"><span data-stu-id="9286d-648">Test Analyzer</span></span>
<span data-ttu-id="9286d-649">Hello **elemzése API** bemutatja, hogyan egy analyzer bontja szöveg jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="9286d-649">hello **Analyze API** shows how an analyzer breaks text into tokens.</span></span>

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="9286d-650">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="9286d-650">**Request**</span></span>

<span data-ttu-id="9286d-651">HTTPS-kapcsolat szükséges szolgáltatások minden olyan kérelem esetében.</span><span class="sxs-lookup"><span data-stu-id="9286d-651">HTTPS is required for all services requests.</span></span> <span data-ttu-id="9286d-652">Hello **elemzése API** kérelmek POST metódussal hello lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9286d-652">hello **Analyze API** request can be constructed using hello POST method.</span></span>

<span data-ttu-id="9286d-653">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="9286d-653">`api-version=[string]` (required).</span></span> <span data-ttu-id="9286d-654">hello előzetes verziója `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9286d-654">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9286d-655">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="9286d-655">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9286d-656">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="9286d-656">**Request Headers**</span></span>

<span data-ttu-id="9286d-657">a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9286d-657">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9286d-658">`api-key`: hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9286d-658">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9286d-659">Egy karakterláncértéket, egyedi tooyour szolgáltatást is.</span><span class="sxs-lookup"><span data-stu-id="9286d-659">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="9286d-660">Hello **elemzése API** kérelem tartalmaznia kell egy `api-key` set tooan adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).</span><span class="sxs-lookup"><span data-stu-id="9286d-660">hello **Analyze API** request must include an `api-key` set tooan admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9286d-661">Konfigurálnia kell hello index neve és hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="9286d-661">You will also need hello index name and hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9286d-662">Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9286d-662">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9286d-663">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="9286d-663">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9286d-664">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="9286d-664">**Request Body**</span></span>

    {
      "text": "Text tooanalyze",
      "analyzer": "analyzer_name"
    }

<span data-ttu-id="9286d-665">vagy</span><span class="sxs-lookup"><span data-stu-id="9286d-665">or</span></span>

    {
      "text": "Text tooanalyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

<span data-ttu-id="9286d-666">Hello `analyzer_name`, `tokenizer_name`, `token_filter_name` és `char_filter_name` toobe érvényes nevet az előre definiált vagy egyéni lekérdezések, tokenizers, lexikális elem és char szűrőket kell hello index.</span><span class="sxs-lookup"><span data-stu-id="9286d-666">hello `analyzer_name`, `tokenizer_name`, `token_filter_name` and `char_filter_name` need toobe valid names of predefined or custom analyzers, tokenizers, token filters and char filters for hello index.</span></span> <span data-ttu-id="9286d-667">hello folyamat lexikális elemzési kapcsolatos információkért tekintse meg toolearn [elemzése az Azure Search](https://aka.ms/azsanalysis).</span><span class="sxs-lookup"><span data-stu-id="9286d-667">toolearn more about hello process of lexical analysis see [Analysis in Azure Search](https://aka.ms/azsanalysis).</span></span>

<span data-ttu-id="9286d-668">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="9286d-668">**Response**</span></span>

<span data-ttu-id="9286d-669">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="9286d-669">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="9286d-670">hello adott válasz törzsének hello a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="9286d-670">hello response body is in hello following format:</span></span>

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of hello first character of hello token),
          "endOffset": number (index of hello last character of hello token),
          "position": number (position of hello token in hello input text)
        },
        ...
      ]
    }

<span data-ttu-id="9286d-671">**Elemezze API – példa**</span><span class="sxs-lookup"><span data-stu-id="9286d-671">**Analyze API example**</span></span>

<span data-ttu-id="9286d-672">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="9286d-672">**Request**</span></span>

    {
      "text": "Text tooanalyze",
      "analyzer": "standard"
    }

<span data-ttu-id="9286d-673">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="9286d-673">**Response**</span></span>

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

- - -
<a name="DocOps"></a>

## <a name="document-operations"></a><span data-ttu-id="9286d-674">A dokumentum műveletek</span><span class="sxs-lookup"><span data-stu-id="9286d-674">Document Operations</span></span>
<span data-ttu-id="9286d-675">Az Azure Search index hello felhő tárolja, és fel JSON-dokumentumokat, hogy töltsön toohello szolgáltatás segítségével.</span><span class="sxs-lookup"><span data-stu-id="9286d-675">In Azure Search, an index is stored in hello cloud and populated using JSON documents that you upload toohello service.</span></span> <span data-ttu-id="9286d-676">Minden hello dokumentumok feltöltött hello corpus keresési adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9286d-676">All hello documents that you upload comprise hello corpus of your search data.</span></span> <span data-ttu-id="9286d-677">Dokumentumok, amelyeket a keresési feltételek vannak tokenekre, feltölti, a mezők tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="9286d-677">Documents contain fields, some of which are tokenized into search terms as they are uploaded.</span></span> <span data-ttu-id="9286d-678">Hello `/docs` URL-szegmenseket, az Azure Search API hello index dokumentumok hello gyűjteményét képviseli.</span><span class="sxs-lookup"><span data-stu-id="9286d-678">hello `/docs` URL segment in hello Azure Search API represents hello collection of documents in an index.</span></span> <span data-ttu-id="9286d-679">Összes művelet végrehajtását, például feltöltése hello gyűjtemény, az egyesítés, törlése vagy dokumentumok lekérdezését kerül sor egy index, így hello URL-címek hello környezetében az e műveletek mindig indul rendelkező `/indexes/[index name]/docs` adott index neve.</span><span class="sxs-lookup"><span data-stu-id="9286d-679">All operations performed on hello collection such as uploading, merging, deleting, or querying documents take place in hello context of a single index, so hello URLs for these operations will always start with `/indexes/[index name]/docs` for a given index name.</span></span>

<span data-ttu-id="9286d-680">Az alkalmazás kódjában JSON-dokumentumok tooupload tooAzure keresési vagy létre kell hoznia, vagy használhat egy [indexelő](https://msdn.microsoft.com/library/dn946891.aspx) tooload Ha hello adatforrás vagy az Azure SQL Database vagy az Azure Cosmos DB dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="9286d-680">Your application code must either generate JSON documents tooupload tooAzure Search or you can use an [indexer](https://msdn.microsoft.com/library/dn946891.aspx) tooload documents if hello data source is either Azure SQL Database or Azure Cosmos DB.</span></span> <span data-ttu-id="9286d-681">Indexek általában fel van töltve, egy Ön által biztosított egyetlen adatkészletből.</span><span class="sxs-lookup"><span data-stu-id="9286d-681">Typically, indexes are populated from a single dataset that you provide.</span></span>

<span data-ttu-id="9286d-682">Meg kell terveznie, amely egy dokumentumot, amelyet az toosearch minden elemhez.</span><span class="sxs-lookup"><span data-stu-id="9286d-682">You should plan on having one document for each item that you want toosearch.</span></span> <span data-ttu-id="9286d-683">Egy movie bérleti alkalmazás rendelkezhet movie egy dokumentumot, egy kirakat alkalmazás rendelkezhet SKU egy dokumentumot, online anyagok alkalmazás rendelkezhet működés során egy dokumentumot, a kutatási vállalkozás előfordulhat, hogy az egyes academic papír egy dokumentumot a tárház, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="9286d-683">A movie rental application might have one document per movie, a storefront application might have one document per SKU, an online courseware application might have one document per course, a research firm might have one document for each academic paper in their repository, and so on.</span></span>

<span data-ttu-id="9286d-684">Dokumentumok egy vagy több mező áll.</span><span class="sxs-lookup"><span data-stu-id="9286d-684">Documents consist of one or more fields.</span></span> <span data-ttu-id="9286d-685">Mezők szöveg, amelyet az Azure Search által a keresési feltételek tokenekre bontott, valamint nem tokenekre vagy szűrők, vagy a pontozási profil használható nem szöveges értékeket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="9286d-685">Fields can contain text that is tokenized by Azure Search into search terms, as well as non-tokenized or non-text values that can be used in filters or scoring profiles.</span></span> <span data-ttu-id="9286d-686">hello név, az adattípusokat és az egyes mezők támogatott keresési funkciók határozza meg a hello a sémát indexeli.</span><span class="sxs-lookup"><span data-stu-id="9286d-686">hello names, data types, and search features supported for each field are determined by hello index schema.</span></span> <span data-ttu-id="9286d-687">Egy azonosító kijelölt egyik hello minden a sémát indexeli, és minden egyes dokumentum hello azonosító mezőjéhez, amely egyedileg azonosítja az adott dokumentum hello index értékűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9286d-687">One of hello fields in each index schema must be designated as an ID, and each document must have a value for hello ID field that uniquely identifies that document in hello index.</span></span> <span data-ttu-id="9286d-688">A dokumentum többi mező megadása nem kötelező, és tooa null értéke alapértelmezés szerint, ha nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="9286d-688">All other document fields are optional and will default tooa null value if left unspecified.</span></span> <span data-ttu-id="9286d-689">Figyelje meg, hogy a null értékek nem lépnek fel területet a hello search-index.</span><span class="sxs-lookup"><span data-stu-id="9286d-689">Note that null values do not take up space in hello search index.</span></span>

<span data-ttu-id="9286d-690">Feltöltheti a dokumentumok, mielőtt kell már létrehozott hello index hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="9286d-690">Before you can upload documents, you must have already created hello index on hello service.</span></span> <span data-ttu-id="9286d-691">Lásd: [a Create Index](#CreateIndex) az első lépéshez vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="9286d-691">See [Create Index](#CreateIndex) for details about this first step.</span></span>

<a name="AddOrUpdateDocuments"></a>

## <a name="add-update-or-delete-documents"></a><span data-ttu-id="9286d-692">Adja hozzá, frissítés, vagy dokumentumok törlése</span><span class="sxs-lookup"><span data-stu-id="9286d-692">Add, Update, or Delete Documents</span></span>
<span data-ttu-id="9286d-693">Feltöltheti, egyesítési, az egyesítés vagy feltöltés és a delete dokumentumok egy megadott indextől HTTP POST használatával.</span><span class="sxs-lookup"><span data-stu-id="9286d-693">You can upload, merge, merge-or-upload or delete documents from a specified index using HTTP POST.</span></span> <span data-ttu-id="9286d-694">Nagyszámú a frissítések kötegelése dokumentumok (too1000 dokumentumok kötegenként) vagy kötegenként körülbelül 16 MB ajánlott.</span><span class="sxs-lookup"><span data-stu-id="9286d-694">For large numbers of updates, batching of documents (up too1000 documents per batch or about 16 MB per batch) is recommended.</span></span>

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="9286d-695">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="9286d-695">**Request**</span></span>

<span data-ttu-id="9286d-696">HTTPS-kapcsolat szükséges az összes szolgáltatási kérelmek.</span><span class="sxs-lookup"><span data-stu-id="9286d-696">HTTPS is required for all service requests.</span></span> <span data-ttu-id="9286d-697">Feltöltheti, egyesítési, az egyesítés vagy feltöltés és a delete dokumentumok egy megadott indextől HTTP POST használatával.</span><span class="sxs-lookup"><span data-stu-id="9286d-697">You can upload, merge, merge-or-upload or delete documents from a specified index using HTTP POST.</span></span>

<span data-ttu-id="9286d-698">hello kérelem URI tartalmaz [index neve], melyik index toopost dokumentumok megadása.</span><span class="sxs-lookup"><span data-stu-id="9286d-698">hello request URI includes [index name], specifying which index toopost documents.</span></span> <span data-ttu-id="9286d-699">Dokumentumok tooone index egyszerre csak beküldheti.</span><span class="sxs-lookup"><span data-stu-id="9286d-699">You can only post documents tooone index at a time.</span></span>

<span data-ttu-id="9286d-700">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="9286d-700">`api-version=[string]` (required).</span></span> <span data-ttu-id="9286d-701">hello előzetes verziója `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9286d-701">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9286d-702">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="9286d-702">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9286d-703">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="9286d-703">**Request Headers**</span></span>

<span data-ttu-id="9286d-704">a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9286d-704">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9286d-705">`Content-Type`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="9286d-705">`Content-Type`: Required.</span></span> <span data-ttu-id="9286d-706">Állítsa ezt túl`application/json`</span><span class="sxs-lookup"><span data-stu-id="9286d-706">Set this too`application/json`</span></span>
* <span data-ttu-id="9286d-707">`api-key`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="9286d-707">`api-key`: Required.</span></span> <span data-ttu-id="9286d-708">Hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9286d-708">hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9286d-709">Egy karakterláncértéket, egyedi tooyour szolgáltatást is.</span><span class="sxs-lookup"><span data-stu-id="9286d-709">It is a string value, unique tooyour service.</span></span> <span data-ttu-id="9286d-710">Hello **hozzáadása dokumentumok** kérelem tartalmaznia kell egy `api-key` fejlécet beállítani tooyour adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).</span><span class="sxs-lookup"><span data-stu-id="9286d-710">hello **Add Documents** request must include an `api-key` header set tooyour admin key (as opposed tooa query key).</span></span>

<span data-ttu-id="9286d-711">Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="9286d-711">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9286d-712">Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9286d-712">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9286d-713">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="9286d-713">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9286d-714">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="9286d-714">**Request Body**</span></span>

<span data-ttu-id="9286d-715">hello kérelem törzse hello tartalmaz egy vagy több dokumentumok toobe indexelt.</span><span class="sxs-lookup"><span data-stu-id="9286d-715">hello body of hello request contains one or more documents toobe indexed.</span></span> <span data-ttu-id="9286d-716">Dokumentumok egyedi kulcs azonosítják.</span><span class="sxs-lookup"><span data-stu-id="9286d-716">Documents are identified by a unique key.</span></span> <span data-ttu-id="9286d-717">Minden egyes dokumentum rendelve egy műveletet: feltöltés, egyesítési, mergeOrUpload vagy törlése.</span><span class="sxs-lookup"><span data-stu-id="9286d-717">Each document is associated with an action: upload, merge, mergeOrUpload or delete.</span></span> <span data-ttu-id="9286d-718">Feltöltés kérelmek kulcs/érték párok készleteként hello dokumentum adatot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="9286d-718">Upload requests must include hello document data as a set of key/value pairs.</span></span>

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [!NOTE]
> <span data-ttu-id="9286d-719">A dokumentum kulcsok csak betűket, számokat, kötőjeleket tartalmazhat ("-"), aláhúzásjelet ("_"), és egyenlőségjelet ("=").</span><span class="sxs-lookup"><span data-stu-id="9286d-719">Document keys can only contain letters, numbers, dashes ("-"), underscores ("_"), and equal signs ("=").</span></span> <span data-ttu-id="9286d-720">További részletekért lásd: [elnevezési szabályait](https://msdn.microsoft.com/library/azure/dn857353.aspx).</span><span class="sxs-lookup"><span data-stu-id="9286d-720">For more details, see [Naming rules](https://msdn.microsoft.com/library/azure/dn857353.aspx).</span></span>
> 
> 

<span data-ttu-id="9286d-721">**A dokumentum műveletek**</span><span class="sxs-lookup"><span data-stu-id="9286d-721">**Document Actions**</span></span>

* <span data-ttu-id="9286d-722">`upload`: Egy feltöltési művelete hasonló tooan "upsert", ahol hello dokumentum lesz beszúrni, ha új, majd frissíteni vagy lecserélni rá.</span><span class="sxs-lookup"><span data-stu-id="9286d-722">`upload`: An upload action is similar tooan "upsert" where hello document will be inserted if it is new and updated/replaced if it exists.</span></span> <span data-ttu-id="9286d-723">Vegye figyelembe, hogy minden mező hello frissítés esetben helyett.</span><span class="sxs-lookup"><span data-stu-id="9286d-723">Note that all fields are replaced in hello update case.</span></span>
* <span data-ttu-id="9286d-724">`merge`: Egyesítés frissítések egy meglévő dokumentum hello található megadott mezőket.</span><span class="sxs-lookup"><span data-stu-id="9286d-724">`merge`: Merge updates an existing document with hello specified fields.</span></span> <span data-ttu-id="9286d-725">Ha hello dokumentum nem létezik, hello egyesítés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="9286d-725">If hello document doesn't exist, hello merge will fail.</span></span> <span data-ttu-id="9286d-726">Bármely egyesítésével megadott mező felül fogja írni a létező mező hello hello dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="9286d-726">Any field you specify in a merge will replace hello existing field in hello document.</span></span> <span data-ttu-id="9286d-727">Ez `Collection(Edm.String)` típusú mezőket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9286d-727">This includes fields of type `Collection(Edm.String)`.</span></span> <span data-ttu-id="9286d-728">Például ha hello dokumentum mezőt tartalmaz "címkék" értékű `["budget"]` és értékkel egyesítésével végrehajtása `["economy", "pool"]` "címkék" hello "címkék" mező hello végső értéke lesz `["economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="9286d-728">For example, if hello document contains a field "tags" with value `["budget"]` and you execute a merge with value `["economy", "pool"]` for "tags", hello final value of hello "tags" field will be `["economy", "pool"]`.</span></span> <span data-ttu-id="9286d-729">A következőket hajtja végre **nem** kell `["budget", "economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="9286d-729">It will **not** be `["budget", "economy", "pool"]`.</span></span>
* <span data-ttu-id="9286d-730">`mergeOrUpload`: viselkedik `merge` Ha hello kulcs már megadott dokumentum hello index már létezik.</span><span class="sxs-lookup"><span data-stu-id="9286d-730">`mergeOrUpload`: behaves like `merge` if a document with hello given key already exists in hello index.</span></span> <span data-ttu-id="9286d-731">Ha hello dokumentum nem létezik, hasonlóan viselkedik `upload` egy új dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="9286d-731">If hello document does not exist it behaves like `upload` with a new document.</span></span>
* <span data-ttu-id="9286d-732">`delete`: Törlés hello megadott dokumentum eltávolítása hello index.</span><span class="sxs-lookup"><span data-stu-id="9286d-732">`delete`: Delete removes hello specified document from hello index.</span></span> <span data-ttu-id="9286d-733">Vegye figyelembe, hogy minden mezőt megadja egy `delete` hello kulcsmező nem lesz figyelembe véve.</span><span class="sxs-lookup"><span data-stu-id="9286d-733">Note that any fields you specify in a `delete` operation other than hello key field will be ignored.</span></span> <span data-ttu-id="9286d-734">Ha azt szeretné, hogy a dokumentum egy egyéni mező tooremove, `merge` helyette és egyszerűen állítja be hello mező túl`null`.</span><span class="sxs-lookup"><span data-stu-id="9286d-734">If you want tooremove an individual field from a document, use `merge` instead and simply set hello field explicitly too`null`.</span></span>

<span data-ttu-id="9286d-735">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="9286d-735">**Response**</span></span>

<span data-ttu-id="9286d-736">Állapotkód 200 (OK) visszaküldött a sikeres válasz, ami azt jelenti, hogy minden elem sikeresen indexelt.</span><span class="sxs-lookup"><span data-stu-id="9286d-736">Status code 200 (OK) is returned for a successful response, meaning that all items have been successfully indexed.</span></span> <span data-ttu-id="9286d-737">Ez jelzi hello `status` tulajdonság beállítása az összes elemek, valamint hello tootrue `statusCode` tulajdonság tooeither 201-es (az újonnan feltöltött dokumentumok) vagy a 200-as (az egyesített vagy törölt dokumentumok):</span><span class="sxs-lookup"><span data-stu-id="9286d-737">This is indicated by hello `status` property being set tootrue for all items, as well as hello `statusCode` property being set tooeither 201 (for newly uploaded documents) or 200 (for merged or deleted documents):</span></span>

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

<span data-ttu-id="9286d-738">Legalább egy tételt indexelése nem sikerült állapotkód 207 (több állapot) érték érkezett vissza.</span><span class="sxs-lookup"><span data-stu-id="9286d-738">Status code 207 (Multi-Status) is returned when at least one item was not successfully indexed.</span></span> <span data-ttu-id="9286d-739">Nem indexelt útjuk hello `status` mezőkészlethez toofalse.</span><span class="sxs-lookup"><span data-stu-id="9286d-739">Items that have not been indexed have hello `status` field set toofalse.</span></span> <span data-ttu-id="9286d-740">Hello `errorMessage` és `statusCode` tulajdonságok hello indexelési hiba okát hello jelzi:</span><span class="sxs-lookup"><span data-stu-id="9286d-740">hello `errorMessage` and `statusCode` properties will indicate hello reason for hello indexing error:</span></span>

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "hello search service is too busy tooprocess this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with hello 'allowIndexDowntime' flag set too'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

<span data-ttu-id="9286d-741">a következő táblázat hello hello különböző dokumentumra hello válaszként visszaküldött állapotkódok ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9286d-741">hello following table explains hello various per-document status codes that can be returned in hello response.</span></span> <span data-ttu-id="9286d-742">Vegye figyelembe, hogy néhány utalnak hello kérelem, míg mások jelzi, hogy ideiglenes hibaállapotok.</span><span class="sxs-lookup"><span data-stu-id="9286d-742">Note that some indicate problems with hello request itself, while others indicate temporary error conditions.</span></span> <span data-ttu-id="9286d-743">hello utóbbi várakozás után érdemes újra megpróbálnia.</span><span class="sxs-lookup"><span data-stu-id="9286d-743">hello latter you should retry after a delay.</span></span>

<table style="font-size:12">
    <tr>
        <th><span data-ttu-id="9286d-744">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="9286d-744">Status code</span></span></th>
        <th><span data-ttu-id="9286d-745">Jelentése</span><span class="sxs-lookup"><span data-stu-id="9286d-745">Meaning</span></span></th>
        <th><span data-ttu-id="9286d-746">Újrapróbálkozást lehetővé tevő</span><span class="sxs-lookup"><span data-stu-id="9286d-746">Retryable</span></span></th>
        <th><span data-ttu-id="9286d-747">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="9286d-747">Notes</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-748">200</span><span class="sxs-lookup"><span data-stu-id="9286d-748">200</span></span></td>
        <td><span data-ttu-id="9286d-749">A dokumentum sikeresen módosították vagy törölték.</span><span class="sxs-lookup"><span data-stu-id="9286d-749">Document was successfully modified or deleted.</span></span></td>
        <td><span data-ttu-id="9286d-750">n/a</span><span class="sxs-lookup"><span data-stu-id="9286d-750">n/a</span></span></td>
        <td><span data-ttu-id="9286d-751">Törlési műveletek vannak <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span><span class="sxs-lookup"><span data-stu-id="9286d-751">Delete operations are <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span></span> <span data-ttu-id="9286d-752">Ez azt jelenti, hogy akkor is, ha egy dokumentum kulcs hello index nem létezik, a delete művelet végrehajtásának kísérlete ezzel a kulccsal eredményez 200 állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="9286d-752">That is, even if a document key does not exist in hello index, attempting a delete operation with that key will result in a 200 status code.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-753">201</span><span class="sxs-lookup"><span data-stu-id="9286d-753">201</span></span></td>
        <td><span data-ttu-id="9286d-754">A dokumentum létrehozása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="9286d-754">Document was successfully created.</span></span></td>
        <td><span data-ttu-id="9286d-755">n/a</span><span class="sxs-lookup"><span data-stu-id="9286d-755">n/a</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-756">400</span><span class="sxs-lookup"><span data-stu-id="9286d-756">400</span></span></td>
        <td><span data-ttu-id="9286d-757">Hiba történt, amely megakadályozta a indexelt hello dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="9286d-757">There was an error in hello document that prevented it from being indexed.</span></span></td>
        <td><span data-ttu-id="9286d-758">Nem</span><span class="sxs-lookup"><span data-stu-id="9286d-758">No</span></span></td>
        <td><span data-ttu-id="9286d-759">hello hibaüzenet hello válaszul jelenik meg, miért nem megfelelő az hello dokumentum.</span><span class="sxs-lookup"><span data-stu-id="9286d-759">hello error message in hello response will indicate what is wrong with hello document.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-760">404</span><span class="sxs-lookup"><span data-stu-id="9286d-760">404</span></span></td>
        <td><span data-ttu-id="9286d-761">hello dokumentum nem egyesíthetők, mert a megadott kulcs hello hello index nem létezik.</span><span class="sxs-lookup"><span data-stu-id="9286d-761">hello document could not be merged because hello given key doesn't exist in hello index.</span></span></td>
        <td><span data-ttu-id="9286d-762">Nem</span><span class="sxs-lookup"><span data-stu-id="9286d-762">No</span></span></td>
        <td><span data-ttu-id="9286d-763">Ez a hiba nem történik meg a feltöltések óta hoznak létre új dokumentumokat, és ez nem történik meg a törlések ezek ugyanis <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span><span class="sxs-lookup"><span data-stu-id="9286d-763">This error does not occur for uploads since they create new documents, and it does not occur for deletes because they are <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-764">409</span><span class="sxs-lookup"><span data-stu-id="9286d-764">409</span></span></td>
        <td><span data-ttu-id="9286d-765">Verzióütközés észlelt tooindex dokumentum megkísérlése során.</span><span class="sxs-lookup"><span data-stu-id="9286d-765">A version conflict was detected when attempting tooindex a document.</span></span></td>
        <td><span data-ttu-id="9286d-766">Igen</span><span class="sxs-lookup"><span data-stu-id="9286d-766">Yes</span></span></td>
        <td><span data-ttu-id="9286d-767">Ez akkor fordulhat elő, azonos egyszerre egynél többször dokumentum tooindex hello regisztráláskor.</span><span class="sxs-lookup"><span data-stu-id="9286d-767">This can happen when you're trying tooindex hello same document more than once concurrently.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-768">422</span><span class="sxs-lookup"><span data-stu-id="9286d-768">422</span></span></td>
        <td><span data-ttu-id="9286d-769">hello index átmenetileg nem érhető el, mert hello "allowIndexDowntime" jelző beállítása too'true a frissítés ".</span><span class="sxs-lookup"><span data-stu-id="9286d-769">hello index is temporarily unavailable because it was updated with hello 'allowIndexDowntime' flag set too'true'.</span></span></td>
        <td><span data-ttu-id="9286d-770">Igen</span><span class="sxs-lookup"><span data-stu-id="9286d-770">Yes</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="9286d-771">503</span><span class="sxs-lookup"><span data-stu-id="9286d-771">503</span></span></td>
        <td><span data-ttu-id="9286d-772">A keresési szolgáltatás átmenetileg nem érhető el, valószínűleg tooheavy terhelés miatt.</span><span class="sxs-lookup"><span data-stu-id="9286d-772">Your search service is temporarily unavailable, possibly due tooheavy load.</span></span></td>
        <td><span data-ttu-id="9286d-773">Igen</span><span class="sxs-lookup"><span data-stu-id="9286d-773">Yes</span></span></td>
        <td><span data-ttu-id="9286d-774">A kód várnia kell, mielőtt megpróbálná ebben az esetben, vagy azzal kockáztatja hello szolgáltatás elérhetetlensége meghosszabbítva.</span><span class="sxs-lookup"><span data-stu-id="9286d-774">Your code should wait before retrying in this case or you risk prolonging hello service unavailability.</span></span></td>
    </tr>
</table> 

<span data-ttu-id="9286d-775">**Megjegyzés:**: Ha az Ügyfélkód gyakran tapasztal 207 választ, egy lehetséges oka, hogy hello rendszer terhelésnek van kitéve.</span><span class="sxs-lookup"><span data-stu-id="9286d-775">**Note**: If your client code frequently encounters a 207 response, one possible reason is that hello system is under load.</span></span> <span data-ttu-id="9286d-776">Hello ellenőrzésével győződhet `statusCode` 503-as tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="9286d-776">You can confirm this by checking hello `statusCode` property for 503.</span></span> <span data-ttu-id="9286d-777">Ha ez helyzet hello, ajánlott ***szabályozás indexelési***.</span><span class="sxs-lookup"><span data-stu-id="9286d-777">If this is hello case, we recommend ***throttling indexing requests***.</span></span> <span data-ttu-id="9286d-778">Ellenkező esetben az indexelő forgalom nem subside, ha hello rendszer elindítása volt elutasító összes kérelmek 503-as hiba.</span><span class="sxs-lookup"><span data-stu-id="9286d-778">Otherwise, if indexing traffic doesn't subside, hello system could start rejecting all requests with 503 errors.</span></span>

<span data-ttu-id="9286d-779">Állapotkód 429 azt jelzi, hogy a dokumentumok / index hello száma kvóta túllépve.</span><span class="sxs-lookup"><span data-stu-id="9286d-779">Status code 429 indicates that you have exceeded your quota on hello number of documents per index.</span></span> <span data-ttu-id="9286d-780">Hozzon létre egy új indexet vagy magasabb kapacitáskorlátait frissítése.</span><span class="sxs-lookup"><span data-stu-id="9286d-780">You must either create a new index or upgrade for higher capacity limits.</span></span>

<span data-ttu-id="9286d-781">**Példa**</span><span class="sxs-lookup"><span data-stu-id="9286d-781">**Example:**</span></span>

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
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
- - -
<a name="SearchDocs"></a>

## <a name="search-documents"></a><span data-ttu-id="9286d-782">Dokumentumok keresése</span><span class="sxs-lookup"><span data-stu-id="9286d-782">Search Documents</span></span>
<span data-ttu-id="9286d-783">A **keresési** művelet GET vagy POST-kérelmet ad ki, és adja meg a paramétereket, amelyek segítségével a hello feltételeknek megfelelő dokumentumok kijelöléséhez.</span><span class="sxs-lookup"><span data-stu-id="9286d-783">A **Search** operation is issued as a GET or POST request and specifies parameters that give hello criteria for selecting matching documents.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

<span data-ttu-id="9286d-784">**Amikor toouse utáni helyett**</span><span class="sxs-lookup"><span data-stu-id="9286d-784">**When toouse POST instead of GET**</span></span>

<span data-ttu-id="9286d-785">Ha használja a HTTP GET toocall hello **keresési** toobe vegye figyelembe, hogy a hello kérelem URL-címe hello hossza nem haladhatja meg a 8 KB-os API-t kell.</span><span class="sxs-lookup"><span data-stu-id="9286d-785">When you use HTTP GET toocall hello **Search** API, you need toobe aware that hello length of hello request URL cannot exceed 8 KB.</span></span> <span data-ttu-id="9286d-786">Ez elég általában a legtöbb alkalmazás esetén.</span><span class="sxs-lookup"><span data-stu-id="9286d-786">This is usually enough for most applications.</span></span> <span data-ttu-id="9286d-787">Egyes alkalmazások azonban nagyon nagy lekérdezések vagy OData szűrőkifejezéseket eredményez.</span><span class="sxs-lookup"><span data-stu-id="9286d-787">However, some applications produce very large queries or OData filter expressions.</span></span> <span data-ttu-id="9286d-788">Ezek az alkalmazások a HTTP POST jobb választás, mivel használata esetén lehetővé teszi a nagyobb szűrőket és lekérdezéseket GET-nál.</span><span class="sxs-lookup"><span data-stu-id="9286d-788">For these applications, using HTTP POST is a better choice because it allows larger filters and queries than GET.</span></span> <span data-ttu-id="9286d-789">A FELADÁS egy vagy több, a feltételek vagy a lekérdezésben záradékok hello száma van hello korlátozása tényező, hello nyers lekérdezéssel, mivel a hello POST kérelem méretkorlátot nagyjából 16 MB nem hello méretét.</span><span class="sxs-lookup"><span data-stu-id="9286d-789">With POST, hello number of terms or clauses in a query is hello limiting factor, not hello size of hello raw query since hello request size limit for POST is approximately 16 MB.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-790">Annak ellenére, hogy hello POST kérelem méretkorlátot nagyon nagy, keresési lekérdezések és szűrőkifejezéseket önkényesen összetett nem lehet.</span><span class="sxs-lookup"><span data-stu-id="9286d-790">Even though hello POST request size limit is very large, search queries and filter expressions cannot be arbitrarily complex.</span></span> <span data-ttu-id="9286d-791">Lásd: [Lucene lekérdezés szintaxisát](https://msdn.microsoft.com/library/mt589323.aspx) és [OData-kifejezésszintaxist](https://msdn.microsoft.com/library/dn798921.aspx) keresési lekérdezés és a szűrő összetettsége korlátozásaival kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="9286d-791">See [Lucene query syntax](https://msdn.microsoft.com/library/mt589323.aspx) and [OData expression syntax](https://msdn.microsoft.com/library/dn798921.aspx) for more information about search query and filter complexity limitations.</span></span>
> 
> 

<span data-ttu-id="9286d-792">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="9286d-792">**Request**</span></span>

<span data-ttu-id="9286d-793">HTTPS szolgáltatáskérések szükség.</span><span class="sxs-lookup"><span data-stu-id="9286d-793">HTTPS is required for service requests.</span></span> <span data-ttu-id="9286d-794">Hello **keresési** kérelem hello GET vagy POST metódus használatával lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9286d-794">hello **Search** request can be constructed using hello GET or POST methods.</span></span>

<span data-ttu-id="9286d-795">hello kérelem URI-azonosítója határozza meg, hogy melyik index tooquery hello paraméterek megfelelő minden dokumentumhoz.</span><span class="sxs-lookup"><span data-stu-id="9286d-795">hello request URI specifies which index tooquery, for all documents that match hello parameters.</span></span> <span data-ttu-id="9286d-796">A GET kérelmek hello esetben hello lekérdezési karakterlánc paraméter meg van adva, és hello kérelem törzse hello esetben POST kérelmek.</span><span class="sxs-lookup"><span data-stu-id="9286d-796">Parameters are specified on hello query string in hello case of GET requests, and in hello request body in hello case of POST requests.</span></span>

<span data-ttu-id="9286d-797">Ajánlott eljárásként GET kérelmek létrehozásakor, ne feledje túl[URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) meghatározott lekérdezési paraméterek meghívásakor közvetlenül hello REST API-t.</span><span class="sxs-lookup"><span data-stu-id="9286d-797">As a best practice when creating GET requests, remember too[URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specific query parameters when calling hello REST API directly.</span></span> <span data-ttu-id="9286d-798">A **keresési** műveleteket, ilyenek:</span><span class="sxs-lookup"><span data-stu-id="9286d-798">For **Search** operations, this includes:</span></span>

* `$filter`
* `facet`
* `highlightPreTag`
* `highlightPostTag`
* `search`
* `moreLikeThis`

<span data-ttu-id="9286d-799">URL-kódolást csak ajánlott hello fent lekérdezési paramétereket.</span><span class="sxs-lookup"><span data-stu-id="9286d-799">URL encoding is only recommended on hello above query parameters.</span></span> <span data-ttu-id="9286d-800">Ha akkor véletlenül URL-encode hello teljes lekérdezési karakterlánc (mindent után hello?), a kérelmek megszakad.</span><span class="sxs-lookup"><span data-stu-id="9286d-800">If you inadvertently URL-encode hello entire query string (everything after hello ?), requests will break.</span></span>

<span data-ttu-id="9286d-801">URL-kódolást is csak szükséges REST API használatával közvetlenül LEKÉRNI hello hívásakor.</span><span class="sxs-lookup"><span data-stu-id="9286d-801">Also, URL encoding is only necessary when calling hello REST API directly using GET.</span></span> <span data-ttu-id="9286d-802">Kódolás nélkül URL-cím szükség, ha hívása **keresési** használatával a FELADÁS egy vagy több, vagy hello használatakor [.NET ügyféloldali kódtár](https://msdn.microsoft.com/library/dn951165.aspx), amely alkalmazás URL-címet meg.</span><span class="sxs-lookup"><span data-stu-id="9286d-802">No URL encoding is necessary when calling **Search** using POST, or when using hello [.NET client library](https://msdn.microsoft.com/library/dn951165.aspx), which handles URL encoding for you.</span></span>

<span data-ttu-id="9286d-803"><a name="SearchQueryParameters"></a>
**Lekérdezés-paraméterek**</span><span class="sxs-lookup"><span data-stu-id="9286d-803"><a name="SearchQueryParameters"></a>
**Query Parameters**</span></span>

<span data-ttu-id="9286d-804">**Keresési** lekérdezés feltételeket, és megadhatja a keresés viselkedését több paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="9286d-804">**Search** accepts several parameters that provide query criteria and also specify search behavior.</span></span> <span data-ttu-id="9286d-805">Megadja, hogy ezek a paraméterek hello URL-címet a lekérdezési karakterlánc meghívásakor **keresési** GET keresztül, és JSON tulajdonságként hello kérés törzsében meghívásakor **keresési** POST keresztül.</span><span class="sxs-lookup"><span data-stu-id="9286d-805">You provide these parameters in hello URL query string when calling **Search** via GET, and as JSON properties in hello request body when calling **Search** via POST.</span></span> <span data-ttu-id="9286d-806">egyes paraméterek hello szintaxisa némileg eltérő GET és POST között.</span><span class="sxs-lookup"><span data-stu-id="9286d-806">hello syntax for some parameters is slightly different between GET and POST.</span></span> <span data-ttu-id="9286d-807">Ezek a különbségek jeleztük megfelelő alatt:</span><span class="sxs-lookup"><span data-stu-id="9286d-807">These differences are noted as applicable below:</span></span>

<span data-ttu-id="9286d-808">`search=[string]`(nem kötelező) – a szöveg toosearch hello.</span><span class="sxs-lookup"><span data-stu-id="9286d-808">`search=[string]` (optional) - hello text toosearch for.</span></span> <span data-ttu-id="9286d-809">Minden `searchable` mezőben keresni alapértelmezés szerint, kivéve, ha `searchFields` van megadva.</span><span class="sxs-lookup"><span data-stu-id="9286d-809">All `searchable` fields are searched by default unless `searchFields` is specified.</span></span> <span data-ttu-id="9286d-810">Ha a Keresés `searchable` hello keresett szöveg magát, a mezők tokenekre, így több feltételek szóközt kell elválasztani (például: `search=hello world`).</span><span class="sxs-lookup"><span data-stu-id="9286d-810">When searching `searchable` fields, hello search text itself is tokenized, so multiple terms can be separated by white space (for example: `search=hello world`).</span></span> <span data-ttu-id="9286d-811">toomatch bármely kifejezés használata `*` (Ez lehet hasznos, ha logikai szűrőlekérdezések).</span><span class="sxs-lookup"><span data-stu-id="9286d-811">toomatch any term, use `*` (this can be useful for boolean filter queries).</span></span> <span data-ttu-id="9286d-812">A paraméter kihagyása rendelkezik azonos érvényesíti, amely túl hello`*`.</span><span class="sxs-lookup"><span data-stu-id="9286d-812">Omitting this parameter has hello same effect as setting it too`*`.</span></span> <span data-ttu-id="9286d-813">Lásd: [egyszerű lekérdezés szintaxisát](https://msdn.microsoft.com/library/dn798920.aspx) a hello keresési szintaxist a részletekért.</span><span class="sxs-lookup"><span data-stu-id="9286d-813">See [Simple Query Syntax](https://msdn.microsoft.com/library/dn798920.aspx) for specifics on hello search syntax.</span></span>

* <span data-ttu-id="9286d-814">**Megjegyzés:**: hello eredmények néha lehet a meglepő lekérdezését `searchable` mezőket.</span><span class="sxs-lookup"><span data-stu-id="9286d-814">**Note**: hello results can sometimes be surprising when querying over `searchable` fields.</span></span> <span data-ttu-id="9286d-815">hello jogkivonatokat létrehozó logika toohandle esetekben közös tooEnglish szöveg például az aposztrófot, vesszővel válassza el egymástól tartalmaz számokat, stb. Például `search=123,456` fog egyezni a hello adott kifejezések 123 és 456, helyett egy egyetlen kifejezés 123,456 óta angolul sok ezer elválasztóként használhatók vesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="9286d-815">hello tokenizer includes logic toohandle cases common tooEnglish text like apostrophes, commas in numbers, etc. For example, `search=123,456` will match a single term 123,456 rather than hello individual terms 123 and 456, since commas are used as thousand-separators for large numbers in English.</span></span> <span data-ttu-id="9286d-816">Ezért azt javasoljuk, központozási tooseparate feltételek helyett szóköz a hello `search` paraméter.</span><span class="sxs-lookup"><span data-stu-id="9286d-816">For this reason, we recommend using white space rather than punctuation tooseparate terms in hello `search` parameter.</span></span>

<span data-ttu-id="9286d-817">`searchMode=any|all`(nem kötelező, alapértelmezés szerint használt érték túl`any`) – akár hello keresőkifejezéseket részének vagy egészének rendelés toocount hello dokumentumban, amely kell egymáshoz.</span><span class="sxs-lookup"><span data-stu-id="9286d-817">`searchMode=any|all` (optional, defaults too`any`) - whether any or all of hello search terms must be matched in order toocount hello document as a match.</span></span>

<span data-ttu-id="9286d-818">`searchFields=[string]`(nem kötelező) – hello listájának vesszővel tagolt mező nevének toosearch hello a megadott szöveg.</span><span class="sxs-lookup"><span data-stu-id="9286d-818">`searchFields=[string]` (optional) - hello list of comma-separated field names toosearch for hello specified text.</span></span> <span data-ttu-id="9286d-819">Célmező jelölésűnek kell lennie `searchable`.</span><span class="sxs-lookup"><span data-stu-id="9286d-819">Target fields must be marked as `searchable`.</span></span>

<span data-ttu-id="9286d-820">`queryType=simple|full`(nem kötelező, alapértelmezés szerint használt érték túl`simple`) – Ha egy egyszerű lekérdező nyelv, amely lehetővé teszi, hogy szimbólumok, mint készlet túl "egyszerű" keresési szöveg értelmezi +, * és "".</span><span class="sxs-lookup"><span data-stu-id="9286d-820">`queryType=simple|full` (optional, defaults too`simple`) - when set too"simple" search text is interpreted using a simple query language that allows for symbols such as +, * and "".</span></span> <span data-ttu-id="9286d-821">Lekérdezések kiértékelése az összes kereshető mezőt között (vagy mezők jelzett `searchFields`) alapértelmezés szerint minden a dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="9286d-821">Queries are evaluated across all searchable fields (or fields indicated in `searchFields`) in each document by default.</span></span> <span data-ttu-id="9286d-822">Ha hello lekérdezés típusa túl beállítása`full` hello Lucene lekérdező nyelv, amely lehetővé teszi, hogy a mező-specifikus és súlyozott keresések keresett szöveg értelmezi.</span><span class="sxs-lookup"><span data-stu-id="9286d-822">When hello query type is set too`full` search text is interpreted using hello Lucene query language which allows field-specific and weighted searches.</span></span> <span data-ttu-id="9286d-823">Lásd: [egyszerű lekérdezés szintaxisát](https://msdn.microsoft.com/library/dn798920.aspx) és [Lucene lekérdezés szintaxisát](https://msdn.microsoft.com/library/mt589323.aspx) a hello keresési szintaxis a részletekért.</span><span class="sxs-lookup"><span data-stu-id="9286d-823">See [Simple Query Syntax](https://msdn.microsoft.com/library/dn798920.aspx) and [Lucene Query Syntax](https://msdn.microsoft.com/library/mt589323.aspx) for specifics on hello search syntaxes.</span></span> 

> [!NOTE]
> <span data-ttu-id="9286d-824">Keresés a hello Lucene lekérdezési nyelv nem támogatott helyett $filter hasonló funkciókat kínál, amelyek között.</span><span class="sxs-lookup"><span data-stu-id="9286d-824">Range search in hello Lucene query language is not supported in favor of $filter which offers similar functionality.</span></span>
> 
> 

<span data-ttu-id="9286d-825">`moreLikeThis=[key]`(választható) **Fontos:** a szolgáltatás csak érhető el a `2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9286d-825">`moreLikeThis=[key]` (optional) **Important:** This feature is only available in `2015-02-28-Preview`.</span></span> <span data-ttu-id="9286d-826">Ez a beállítás nem használható a hello szöveges keresési paramétert tartalmazó lekérdezésben `search=[string]`.</span><span class="sxs-lookup"><span data-stu-id="9286d-826">This option cannot be used in a query that contains hello text search parameter, `search=[string]`.</span></span> <span data-ttu-id="9286d-827">Hello `moreLikeThis` paraméter hello dokumentum kulcs által megadott hasonló toohello dokumentum-dokumentumok talál.</span><span class="sxs-lookup"><span data-stu-id="9286d-827">hello `moreLikeThis` parameter finds documents that are similar toohello document specified by hello document key.</span></span> <span data-ttu-id="9286d-828">Ha a keresési kérelem rendelkező `moreLikeThis`, keresési feltételeinek hello gyakoriságát és ritkasága hello forrásdokumentum kifejezések alapján jön létre.</span><span class="sxs-lookup"><span data-stu-id="9286d-828">When a search request is made with `moreLikeThis`, a list of search terms is generated based on hello frequency and rarity of terms in hello source document.</span></span> <span data-ttu-id="9286d-829">A feltételeket nem, akkor a használt toomake hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="9286d-829">Those terms are then used toomake hello request.</span></span> <span data-ttu-id="9286d-830">Alapértelmezés szerint az összes hello `searchable` mezők minősülnek, kivéve, ha `searchFields` mezőket figyelembe veszi a keresésnél használt toorestrict van.</span><span class="sxs-lookup"><span data-stu-id="9286d-830">By default, hello contents of all `searchable` fields are considered unless `searchFields` is used toorestrict which fields are searched.</span></span>  

<span data-ttu-id="9286d-831">`$skip=#`(nem kötelező) – hello száma keresési eredmények tooskip; Nem lehet nagyobb, mint 100 000.</span><span class="sxs-lookup"><span data-stu-id="9286d-831">`$skip=#` (optional) - hello number of search results tooskip; Cannot be greater than 100,000.</span></span> <span data-ttu-id="9286d-832">Ha tooscan dokumentumok sorrendben kell, de nem használható `$skip` toothis korlátozás miatt érdemes `$orderby` teljesen rendezett kulcshoz és `$filter` tartománnyal lekérdezés helyett.</span><span class="sxs-lookup"><span data-stu-id="9286d-832">If you need tooscan documents in sequence but cannot use `$skip` due toothis limitation, consider using `$orderby` on a totally-ordered key and `$filter` with a range query instead.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-833">Meghívásakor **keresési** POST használ, ez a paraméter neve `skip` helyett `$skip`.</span><span class="sxs-lookup"><span data-stu-id="9286d-833">When calling **Search** using POST, this parameter is named `skip` instead of `$skip`.</span></span>
> 
> 

<span data-ttu-id="9286d-834">`$top=#`(nem kötelező) – hello száma keresési eredmények tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="9286d-834">`$top=#` (optional) - hello number of search results tooretrieve.</span></span> <span data-ttu-id="9286d-835">Használható együtt `$skip` tooimplement ügyféloldali lapozás a keresési eredmények.</span><span class="sxs-lookup"><span data-stu-id="9286d-835">This can be used in conjunction with `$skip` tooimplement client-side paging of search results.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-836">Meghívásakor **keresési** POST használ, ez a paraméter neve `top` helyett `$top`.</span><span class="sxs-lookup"><span data-stu-id="9286d-836">When calling **Search** using POST, this parameter is named `top` instead of `$top`.</span></span>
> 
> 

<span data-ttu-id="9286d-837">`$count=true|false`(nem kötelező, alapértelmezés szerint használt érték túl`false`) – Itt adhatja meg, hogy toofetch hello eredmények teljes száma.</span><span class="sxs-lookup"><span data-stu-id="9286d-837">`$count=true|false` (optional, defaults too`false`) - Specifies whether toofetch hello total count of results.</span></span> <span data-ttu-id="9286d-838">Ez az összes dokumentumot, amelyek megfelelnek a hello hello száma `search` és `$filter` paraméterek, a rendszer figyelmen kívül hagyja `$top` és `$skip`.</span><span class="sxs-lookup"><span data-stu-id="9286d-838">This is hello count of all documents that match hello `search` and `$filter` parameters, ignoring `$top` and `$skip`.</span></span> <span data-ttu-id="9286d-839">Ha az érték túl`true` teljesítmény hatással lehet.</span><span class="sxs-lookup"><span data-stu-id="9286d-839">Setting this value too`true` may have a performance impact.</span></span> <span data-ttu-id="9286d-840">Vegye figyelembe, hogy visszaadott hello számának közelítő.</span><span class="sxs-lookup"><span data-stu-id="9286d-840">Note that hello count returned is an approximation.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-841">Meghívásakor **keresési** POST használ, ez a paraméter neve `count` helyett `$count`.</span><span class="sxs-lookup"><span data-stu-id="9286d-841">When calling **Search** using POST, this parameter is named `count` instead of `$count`.</span></span>
> 
> 

<span data-ttu-id="9286d-842">`$orderby=[string]`(választható) - kifejezések vesszővel tagolt toosort hello eredményeket listáját.</span><span class="sxs-lookup"><span data-stu-id="9286d-842">`$orderby=[string]` (optional) - A list of comma-separated expressions toosort hello results by.</span></span> <span data-ttu-id="9286d-843">Minden egyes kifejezés lehet, vagy egy mező nevét, vagy egy hívás toohello `geo.distance()` függvény.</span><span class="sxs-lookup"><span data-stu-id="9286d-843">Each expression can be either a field name or a call toohello `geo.distance()` function.</span></span> <span data-ttu-id="9286d-844">Minden egyes kifejezés követhetnek `asc` tooindicated növekvő, és `desc` csökkenő tooindicate.</span><span class="sxs-lookup"><span data-stu-id="9286d-844">Each expression can be followed by `asc` tooindicated ascending, and `desc` tooindicate descending.</span></span> <span data-ttu-id="9286d-845">hello alapértelmezett növekvő sorrend megadásához.</span><span class="sxs-lookup"><span data-stu-id="9286d-845">hello default is ascending order.</span></span> <span data-ttu-id="9286d-846">TIES által hello egyezés pontszámok dokumentumok oszlanak meg.</span><span class="sxs-lookup"><span data-stu-id="9286d-846">Ties will be broken by hello match scores of documents.</span></span> <span data-ttu-id="9286d-847">Ha nincs `$orderby` van megadva, hello alapértelmezett rendezési sorrend szerint dokumentum egyezés pontszám van csökkenő.</span><span class="sxs-lookup"><span data-stu-id="9286d-847">If no `$orderby` is specified, hello default sort order is descending by document match score.</span></span> <span data-ttu-id="9286d-848">Egy legfeljebb 32 záradékok `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="9286d-848">There is a limit of 32 clauses for `$orderby`.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-849">Meghívásakor **keresési** POST használ, ez a paraméter neve `orderby` helyett `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="9286d-849">When calling **Search** using POST, this parameter is named `orderby` instead of `$orderby`.</span></span>
> 
> 

<span data-ttu-id="9286d-850">`$select=[string]`(választható) - tooretrieve mezők vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="9286d-850">`$select=[string]` (optional) - A list of comma-separated fields tooretrieve.</span></span> <span data-ttu-id="9286d-851">Ha nincs megadva, az összes mezők lekérhető hello sémában jelölésű szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="9286d-851">If unspecified, all fields marked as retrievable in hello schema are included.</span></span> <span data-ttu-id="9286d-852">Ez a paraméter túl beállításával közvetlenül is kérhet minden mező`*`.</span><span class="sxs-lookup"><span data-stu-id="9286d-852">You can also explicitly request all fields by setting this parameter too`*`.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-853">Meghívásakor **keresési** POST használ, ez a paraméter neve `select` helyett `$select`.</span><span class="sxs-lookup"><span data-stu-id="9286d-853">When calling **Search** using POST, this parameter is named `select` instead of `$select`.</span></span>
> 
> 

<span data-ttu-id="9286d-854">`facet=[string]`(nulla vagy több) - egy mező toofacet által.</span><span class="sxs-lookup"><span data-stu-id="9286d-854">`facet=[string]` (zero or more) - A field toofacet by.</span></span> <span data-ttu-id="9286d-855">Opcionálisan hello karakterlánc tartalmazhat paraméterek toocustomize hello értékkorlátozás vesszővel tagolt kifejezett `name:value` párokat.</span><span class="sxs-lookup"><span data-stu-id="9286d-855">Optionally hello string may contain parameters toocustomize hello faceting expressed as comma-separated `name:value` pairs.</span></span> <span data-ttu-id="9286d-856">Érvényes paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="9286d-856">Valid parameters are:</span></span>

* <span data-ttu-id="9286d-857">`count`(dimenzió feltételek; száma legfeljebb alapértelmezett érték 10).</span><span class="sxs-lookup"><span data-stu-id="9286d-857">`count` (max number of facet terms; default is 10).</span></span> <span data-ttu-id="9286d-858">Nincs maximum van, de magasabb értékkel fel Önnek megfelelő teljesítményét, különösen akkor, ha hello jellemzőalapú mező számos egyedi feltételeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9286d-858">There is no maximum, but higher values incur a corresponding performance penalty, especially if hello faceted field contains a large number of unique terms.</span></span>
  * <span data-ttu-id="9286d-859">Például: `facet=category,count:5` lekérdezi hello felső öt kategóriába értékkorlátozás eredményekben.</span><span class="sxs-lookup"><span data-stu-id="9286d-859">For example: `facet=category,count:5` gets hello top five categories in facet results.</span></span>  
  * <span data-ttu-id="9286d-860">**Megjegyzés:**: Ha hello `count` paraméter értéke kisebb, mint hello az egyedi feltételeket, hello eredmények pontatlan lehet.</span><span class="sxs-lookup"><span data-stu-id="9286d-860">**Note**: If hello `count` parameter is less than hello number of unique terms, hello results may not be accurate.</span></span> <span data-ttu-id="9286d-861">Ez az értékkorlátozás lekérdezések elosztott szilánkok toohello módon miatt.</span><span class="sxs-lookup"><span data-stu-id="9286d-861">This is due toohello way faceting queries are distributed across shards.</span></span> <span data-ttu-id="9286d-862">Növekvő `count` általában növeli hello hello kifejezés számát, de teljesítmény költségekkel pontosságát.</span><span class="sxs-lookup"><span data-stu-id="9286d-862">Increasing `count` generally increases hello accuracy of hello term counts, but at a performance cost.</span></span>
* <span data-ttu-id="9286d-863">`sort`(egyik `count` toosort *csökkenő* szerinti `-count` toosort *növekvő* szerinti `value` toosort *növekvő* értékkel, vagy `-value` toosort *csökkenő* érték)</span><span class="sxs-lookup"><span data-stu-id="9286d-863">`sort` (one of `count` toosort *descending* by count, `-count` toosort *ascending* by count, `value` toosort *ascending* by value, or `-value` toosort *descending* by value)</span></span>
  * <span data-ttu-id="9286d-864">Például: `facet=category,count:3,sort:count` lekérdezi hello felső három kategóriába értékkorlátozás eredmények hello dokumentumok száma, az egyes városnév szerint csökkenő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="9286d-864">For example: `facet=category,count:3,sort:count` gets hello top three categories in facet results in descending order by hello number of documents with each city name.</span></span> <span data-ttu-id="9286d-865">Például ha hello felső három kategóriába költségvetést, Motel és engedélyezhető, és költségvetési rendelkezik 5 találatok Motel 6 van, és engedélyezhető 4 rendelkezik, akkor hello gyűjtők lesz hello sorrendben Motel, a költségvetési, engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="9286d-865">For example, if hello top three categories are Budget, Motel, and Luxury, and Budget has 5 hits, Motel has 6, and Luxury has 4, then hello buckets will be in hello order Motel, Budget, Luxury.</span></span>
  * <span data-ttu-id="9286d-866">Például: `facet=rating,sort:-value` hozza létre a gyűjtőbe az összes lehetséges értékelések érték szerint csökkenő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="9286d-866">For example: `facet=rating,sort:-value` produces buckets for all possible ratings, in descending order by value.</span></span> <span data-ttu-id="9286d-867">Például ha hello minősítések 1 too5 származnak, hello gyűjtők úgy kell rendezni, 5, 4, 3, 2, 1, hány dokumentumok felel meg az összes minősítés függetlenül.</span><span class="sxs-lookup"><span data-stu-id="9286d-867">For example, if hello ratings are from 1 too5, hello buckets will be ordered 5, 4, 3, 2, 1, irrespective of how many documents match each rating.</span></span>
* <span data-ttu-id="9286d-868">`values`(cső tagolt numerikus vagy `Edm.DateTimeOffset` dimenzióértéknek bejegyzés az dinamikus csoportja értékek)</span><span class="sxs-lookup"><span data-stu-id="9286d-868">`values` (pipe-delimited numeric or `Edm.DateTimeOffset` values specifying a dynamic set of facet entry values)</span></span>
  * <span data-ttu-id="9286d-869">Például: `facet=baseRate,values:10|20` három gyűjtők eredményez: base arányának 0 toobut nem például 10, 10 toobut nem például 20, és egy 20 vagy magasabb be egyet be egyet.</span><span class="sxs-lookup"><span data-stu-id="9286d-869">For example: `facet=baseRate,values:10|20` produces three buckets: One for base rate 0 up toobut not including 10, one for 10 up toobut not including 20, and one for 20 or higher.</span></span>
  * <span data-ttu-id="9286d-870">Például: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` hoz létre két gyűjtők: egy a szállodák felújított 2010. február előtt, egy pedig a szállodákat felújított. február 1., 2010-es vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="9286d-870">For example: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` produces two buckets: One for hotels renovated before February 2010, and one for hotels renovated February 1st, 2010 or later.</span></span>
* <span data-ttu-id="9286d-871">`interval`(a számok, 0-nál nagyobb egész szám időköz vagy `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` idő értékek dátum)</span><span class="sxs-lookup"><span data-stu-id="9286d-871">`interval` (integer interval greater than 0 for numbers, or `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` for date time values)</span></span>
  * <span data-ttu-id="9286d-872">Például: `facet=baseRate,interval:100` gyűjtők mérete 100 alap-címtartományok alapján hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9286d-872">For example: `facet=baseRate,interval:100` produces buckets based on base rate ranges of size 100.</span></span> <span data-ttu-id="9286d-873">Például ha base díjszabás összes $60 és a $600 közötti, lesz a 0 – 100, 100-200-as, 200 – 300, 300-400, 400-500 és 500-600 gyűjtők.</span><span class="sxs-lookup"><span data-stu-id="9286d-873">For example, if base rates are all between $60 and $600, there will be buckets for 0-100, 100-200, 200-300, 300-400, 400-500, and 500-600.</span></span>
  * <span data-ttu-id="9286d-874">Például: `facet=lastRenovationDate,interval:year` hoz létre egy gyűjtő amikor szállodák volt felújított évente.</span><span class="sxs-lookup"><span data-stu-id="9286d-874">For example: `facet=lastRenovationDate,interval:year` produces one bucket for each year when hotels were renovated.</span></span>
* <span data-ttu-id="9286d-875">`timeoffset`([+-] óó: pp, [+-] hhmm, vagy [+-] hh) `timeoffset` nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="9286d-875">`timeoffset` ([+-]hh:mm, [+-]hhmm, or [+-]hh) `timeoffset` is optional.</span></span> <span data-ttu-id="9286d-876">Csak kombinálható az hello `interval` lehetőséget, és csak akkor, ha alkalmazott tooa típusú mező `Edm.DateTimeOffset`.</span><span class="sxs-lookup"><span data-stu-id="9286d-876">It can only be combined with hello `interval` option, and only when applied tooa field of type `Edm.DateTimeOffset`.</span></span> <span data-ttu-id="9286d-877">hello érték hello UTC idő eltolási tooaccount az idő határok beállítása határozza meg.</span><span class="sxs-lookup"><span data-stu-id="9286d-877">hello value specifies hello UTC time offset tooaccount for in setting time boundaries.</span></span>
  * <span data-ttu-id="9286d-878">Például: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` használ hello 01:00:00 UTC (hello cél időzónában éjfél) kezdődő nap határ</span><span class="sxs-lookup"><span data-stu-id="9286d-878">For example: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` uses hello day boundary that starts at 01:00:00 UTC (midnight in hello target time zone)</span></span>
* <span data-ttu-id="9286d-879">**Megjegyzés:**: `count` és `sort` kombinálható hello azonos értékkorlátozás megadását, de nem használható együtt sem `interval` vagy `values`, és `interval` és `values` együtt nem használható együtt.</span><span class="sxs-lookup"><span data-stu-id="9286d-879">**Note**: `count` and `sort` can be combined in hello same facet specification, but they cannot be combined with `interval` or `values`, and `interval` and `values` cannot be combined together.</span></span>
* <span data-ttu-id="9286d-880">**Megjegyzés:**: dátum idő időköz értékkorlátozás arra az esetre vonatkoznak UTC idő alapján, ha `timeoffset` nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="9286d-880">**Note**: Interval facets on date time are computed based on UTC time if `timeoffset` is not specified.</span></span> <span data-ttu-id="9286d-881">Például: a `facet=lastRenovationDate,interval:day`, hello nap határ 00:00:00 UTC kezdődik.</span><span class="sxs-lookup"><span data-stu-id="9286d-881">For example: for `facet=lastRenovationDate,interval:day`, hello day boundary starts at 00:00:00 UTC.</span></span> 

> [!NOTE]
> <span data-ttu-id="9286d-882">Meghívásakor **keresési** POST használ, ez a paraméter neve `facets` helyett `facet`.</span><span class="sxs-lookup"><span data-stu-id="9286d-882">When calling **Search** using POST, this parameter is named `facets` instead of `facet`.</span></span> <span data-ttu-id="9286d-883">Is akkor adja meg azt a JSON-tömb, ahol minden karakterlánca egy külön értékkorlátozás kifejezés karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="9286d-883">Also, you specify it as a JSON array of strings where each string is a separate facet expression.</span></span>
> 
> 

<span data-ttu-id="9286d-884">`$filter=[string]`(nem kötelező) – standard OData szintaxisban strukturált keresőkifejezést.</span><span class="sxs-lookup"><span data-stu-id="9286d-884">`$filter=[string]` (optional) - A structured search expression in standard OData syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-885">Meghívásakor **keresési** POST használ, ez a paraméter neve `filter` helyett `$filter`.</span><span class="sxs-lookup"><span data-stu-id="9286d-885">When calling **Search** using POST, this parameter is named `filter` instead of `$filter`.</span></span>
> 
> 

<span data-ttu-id="9286d-886">`highlight=[string]`(választható) - készletként használt találat vesszővel elválasztott mezők nevei mutatja be.</span><span class="sxs-lookup"><span data-stu-id="9286d-886">`highlight=[string]` (optional) - A set of comma-separated field names used for hit highlights.</span></span> <span data-ttu-id="9286d-887">Csak `searchable` mezők találatok kiemelése is használható.</span><span class="sxs-lookup"><span data-stu-id="9286d-887">Only `searchable` fields can be used for hit highlighting.</span></span>

<span data-ttu-id="9286d-888">`highlightPreTag=[string]`(nem kötelező, alapértelmezés szerint használt érték túl`<em>`) – egy karakterlánc-címke, amely lefoglalja toohit emeli ki.</span><span class="sxs-lookup"><span data-stu-id="9286d-888">`highlightPreTag=[string]` (optional, defaults too`<em>`) - A string tag that prepends toohit highlights.</span></span> <span data-ttu-id="9286d-889">Meg kell a `highlightPostTag`.</span><span class="sxs-lookup"><span data-stu-id="9286d-889">Must be set with `highlightPostTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-890">Meghívásakor **keresési** GET használ, foglalt karaktereket: hello URL-címben százalék-kódolással kell (például % &#23; helyett).</span><span class="sxs-lookup"><span data-stu-id="9286d-890">When calling **Search** using GET, reserved characters in hello URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="9286d-891">`highlightPostTag=[string]`(nem kötelező, alapértelmezés szerint használt érték túl`</em>`)-karakterlánc címke, amely hozzáfűzi toohit emeli ki.</span><span class="sxs-lookup"><span data-stu-id="9286d-891">`highlightPostTag=[string]` (optional, defaults too`</em>`) - a string tag that appends toohit highlights.</span></span> <span data-ttu-id="9286d-892">Meg kell a `highlightPreTag`.</span><span class="sxs-lookup"><span data-stu-id="9286d-892">Must be set with `highlightPreTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-893">Meghívásakor **keresési** GET használ, foglalt karaktereket: hello URL-címben százalék-kódolással kell (például % &#23; helyett).</span><span class="sxs-lookup"><span data-stu-id="9286d-893">When calling **Search** using GET, reserved characters in hello URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="9286d-894">`scoringProfile=[string]`(nem kötelező) – a pontozási profil tooevaluate hello neve egyezik pontszámokat rendelés toosort hello eredmények megfelelő dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="9286d-894">`scoringProfile=[string]` (optional) - hello name of a scoring profile tooevaluate match scores for matching documents in order toosort hello results.</span></span>

<span data-ttu-id="9286d-895">`scoringParameter=[string]`(nulla vagy több) – azt jelzi, hello értékek pontozó függvények megadott mindegyik paraméterhez (például `referencePointParameter`) hello formátumban `name-value1,value2,...`.</span><span class="sxs-lookup"><span data-stu-id="9286d-895">`scoringParameter=[string]` (zero or more) - Indicates hello values for each parameter defined in a scoring function (for example, `referencePointParameter`) using hello format `name-value1,value2,...`.</span></span>

* <span data-ttu-id="9286d-896">Például, ha a pontozási profil hello meghatározása függvény és egy paraméter "mylocation" hello lekérdezési karakterláncának beállítására lenne `&scoringParameter=mylocation--122.2,44.8`.</span><span class="sxs-lookup"><span data-stu-id="9286d-896">For example, if hello scoring profile defines a function with a parameter called "mylocation" hello query string option would be `&scoringParameter=mylocation--122.2,44.8`.</span></span> <span data-ttu-id="9286d-897">hello első dash hello második dash pedig hello első érték (ebben a példában hosszúság) része, amely elválasztja hello értékek listájából, hello nevét.</span><span class="sxs-lookup"><span data-stu-id="9286d-897">hello first dash separates hello name from hello value list, while hello second dash is part of hello first value (longitude in this example).</span></span>
* <span data-ttu-id="9286d-898">Pontozó paraméterek címke, amely kiemelése tartalmazhat vesszővel válassza el egymástól, mint például ilyen értékeket hello lista használatával szimpla idézőjelben karaktert.</span><span class="sxs-lookup"><span data-stu-id="9286d-898">For scoring parameters such as for tag boosting that can contain commas, you can escape any such values in hello list using single quotes.</span></span> <span data-ttu-id="9286d-899">Maguk hello értékeket tartalmazhat félidézőjelet, is escape így dupla által.</span><span class="sxs-lookup"><span data-stu-id="9286d-899">If hello values themselves contain single quotes, you can escape them by doubling.</span></span>
  * <span data-ttu-id="9286d-900">Például ha egy címke "mytag" nevű paramétert kiemelése és azt szeretné, hogy hello címke tooboost értékek "Hello, O'Brien" és "Smith" hello lekérdezési karakterláncának beállítására lenne `&scoringParameter=mytag-'Hello, O''Brien',Smith`.</span><span class="sxs-lookup"><span data-stu-id="9286d-900">For example, if you have a tag boosting parameter called "mytag" and you want tooboost on hello tag values "Hello, O'Brien" and "Smith", hello query string option would be `&scoringParameter=mytag-'Hello, O''Brien',Smith`.</span></span> <span data-ttu-id="9286d-901">Ne feledje, hogy ajánlatok csak szükséges értékeket tartalmazó, vesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="9286d-901">Note that quotes are only required for values that contain commas.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-902">Meghívásakor **keresési** POST használ, ez a paraméter neve `scoringParameters` helyett `scoringParameter`.</span><span class="sxs-lookup"><span data-stu-id="9286d-902">When calling **Search** using POST, this parameter is named `scoringParameters` instead of `scoringParameter`.</span></span> <span data-ttu-id="9286d-903">Is, akkor adja meg azt a JSON-tömb, ahol minden karakterlánca külön karakterláncok `name-values` pár.</span><span class="sxs-lookup"><span data-stu-id="9286d-903">Also, you specify it as a JSON array of strings where each string is a separate `name-values` pair.</span></span>
> 
> 

<span data-ttu-id="9286d-904">`minimumCoverage`(nem kötelező, alapértelmezés szerint használt érték too100) – sikeres jelentett 0 és 100 jelző hello százalékos aránya, amelyek ahhoz, hogy hello lekérdezés toobe keresési lekérdezés hatálya hello index közötti szám.</span><span class="sxs-lookup"><span data-stu-id="9286d-904">`minimumCoverage` (optional, defaults too100) - a number between 0 and 100 indicating hello percentage of hello index that must be covered by a search query in order for hello query toobe reported as a success.</span></span> <span data-ttu-id="9286d-905">Alapértelmezés szerint hello teljes index elérhetőnek kell lennie, vagy `Search` 503-as HTTP-állapotkód: ad vissza.</span><span class="sxs-lookup"><span data-stu-id="9286d-905">By default, hello entire index must be available or `Search` will return HTTP status code 503.</span></span> <span data-ttu-id="9286d-906">Ha `minimumCoverage` és `Search` sikeres, térjen vissza a 200-as HTTP, és tartalmazzák a `@search.coverage` hello válaszban hello index hello lekérdezésben szereplő hello százalékos értékét.</span><span class="sxs-lookup"><span data-stu-id="9286d-906">If you set `minimumCoverage` and `Search` succeeds, it will return HTTP 200 and include a `@search.coverage` value in hello response indicating hello percentage of hello index that was included in hello query.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-907">Ha az érték paraméter tooa kisebb, mint 100 search rendelkezésre állását, még a szolgáltatások csak egy replika azért hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="9286d-907">Setting this parameter tooa value lower than 100 can be useful for ensuring search availability even for services with only one replica.</span></span> <span data-ttu-id="9286d-908">Azonban nem minden egyező dokumentumok garantáltan toobe hello keresési eredmények között szerepel.</span><span class="sxs-lookup"><span data-stu-id="9286d-908">However, not all matching documents are guaranteed toobe present in hello search results.</span></span> <span data-ttu-id="9286d-909">Ha keresés visszaírási fontosabb tooyour alkalmazás, mint a rendelkezésre állási, akkor ajánlott tooleave `minimumCoverage` az alapértelmezett érték 100.</span><span class="sxs-lookup"><span data-stu-id="9286d-909">If search recall is more important tooyour application than availability, then it's best tooleave `minimumCoverage` at its default value of 100.</span></span>
> 
> 

<span data-ttu-id="9286d-910">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="9286d-910">`api-version=[string]` (required).</span></span> <span data-ttu-id="9286d-911">hello előzetes verziója `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9286d-911">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9286d-912">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="9286d-912">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9286d-913">Megjegyzés: Ehhez a művelethez hello `api-version` hello URL-címében, függetlenül attól, hogy meghívja a lekérdezési paraméter van megadva **keresési** GET vagy POST.</span><span class="sxs-lookup"><span data-stu-id="9286d-913">Note: For this operation, hello `api-version` is specified as a query parameter in hello URL regardless of whether you call **Search** with GET or POST.</span></span>

<span data-ttu-id="9286d-914">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="9286d-914">**Request Headers**</span></span>

<span data-ttu-id="9286d-915">a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9286d-915">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9286d-916">`api-key`: hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9286d-916">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9286d-917">Egy karakterláncértéket, egyedi tooyour szolgáltatás URL-címe.</span><span class="sxs-lookup"><span data-stu-id="9286d-917">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="9286d-918">Hello **keresési** kérelem adhat meg, egy adminisztrációs kulcsot vagy egy lekérdezés kulcs `api-key`.</span><span class="sxs-lookup"><span data-stu-id="9286d-918">hello **Search** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="9286d-919">Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="9286d-919">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9286d-920">Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9286d-920">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9286d-921">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="9286d-921">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9286d-922">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="9286d-922">**Request Body**</span></span>

<span data-ttu-id="9286d-923">A GET: nincs.</span><span class="sxs-lookup"><span data-stu-id="9286d-923">For GET: None.</span></span>

<span data-ttu-id="9286d-924">A FELADÁS egy vagy több:</span><span class="sxs-lookup"><span data-stu-id="9286d-924">For POST:</span></span>

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered toodeclare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

<span data-ttu-id="9286d-925">**A részleges kereséshez válaszok folytatása**</span><span class="sxs-lookup"><span data-stu-id="9286d-925">**Continuation of Partial Search Responses**</span></span>

<span data-ttu-id="9286d-926">Néha Azure Search nem térhet vissza az összes hello egy keresési választ kért eredményez.</span><span class="sxs-lookup"><span data-stu-id="9286d-926">Sometimes Azure Search can't return all hello requested results in a single Search response.</span></span> <span data-ttu-id="9286d-927">Ez akkor fordulhat elő, amikor hello lekérdezési kérelmek nem ad meg a túl sok dokumentumok például különböző okokból `$top` vagy értéket `$top` túl nagy.</span><span class="sxs-lookup"><span data-stu-id="9286d-927">This can happen for different reasons, such as when hello query requests too many documents by not specifying `$top` or specifying a value for `$top` that is too large.</span></span> <span data-ttu-id="9286d-928">Ilyen esetben a Azure Search olyan hello `@odata.nextLink` hello válasz törzsében, Megjegyzés, továbbá `@search.nextPageParameters` Ha POST kérelem volt.</span><span class="sxs-lookup"><span data-stu-id="9286d-928">In such cases, Azure Search will include hello `@odata.nextLink` annotation in hello response body, and also `@search.nextPageParameters` if it was a POST request.</span></span> <span data-ttu-id="9286d-929">Használhatja a jegyzetek tooformulate hello értékének egy másik keresési kérelem tooget hello következő része hello keresési válasz.</span><span class="sxs-lookup"><span data-stu-id="9286d-929">You can use hello values of these annotations tooformulate another Search request tooget hello next part of hello search response.</span></span> <span data-ttu-id="9286d-930">Ez a lehetőség egy ***folytatási*** hello eredeti keresési kérelmet, és hello jegyzetek általában nevezzük ***folytatási jogkivonatok***.</span><span class="sxs-lookup"><span data-stu-id="9286d-930">This is called a ***continuation*** of hello original Search request, and hello annotations are generally called ***continuation tokens***.</span></span> <span data-ttu-id="9286d-931">Lásd: [hello az alábbi példa](#SearchResponse) hello szintaxis ezeket a jegyzeteket és ahol hello adott válasz törzsének jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="9286d-931">See [hello example below](#SearchResponse) for details on hello syntax of these annotations and where they appear in hello response body.</span></span> 

<span data-ttu-id="9286d-932">Miért Azure Search előfordulhat, hogy vissza a folytatási jogkivonatok hello ok: a függő és a tárgy toochange.</span><span class="sxs-lookup"><span data-stu-id="9286d-932">hello reasons why Azure Search might return continuation tokens are implementation-specific and subject toochange.</span></span> <span data-ttu-id="9286d-933">Robusztus ügyfelek mindig kell készen toohandle esetekben, ahol vártnál kevesebb dokumentumot ad vissza és a folytatási kód belefoglalt toocontinue dokumentumok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="9286d-933">Robust clients should always be ready toohandle cases where fewer documents than expected are returned and a continuation token is included toocontinue retrieving documents.</span></span> <span data-ttu-id="9286d-934">Is vegye figyelembe, hogy használjon hello azonos HTTP módszert az eredeti kérést hello rendelés toocontinue.</span><span class="sxs-lookup"><span data-stu-id="9286d-934">Also note that you must use hello same HTTP method as hello original request in order toocontinue.</span></span> <span data-ttu-id="9286d-935">Például ha egy GET kérelmet küldött, minden folytatási kérést küld kell használnia az GET (és hasonlóképpen POST).</span><span class="sxs-lookup"><span data-stu-id="9286d-935">For example, if you sent a GET request, any continuation requests you send must also use GET (and likewise for POST).</span></span>

<span data-ttu-id="9286d-936"><a name="SearchResponse"></a>
**Válasz**</span><span class="sxs-lookup"><span data-stu-id="9286d-936"><a name="SearchResponse"></a>
**Response**</span></span>

<span data-ttu-id="9286d-937">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="9286d-937">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      "@odata.count": # (if $count=true was provided in hello query),
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "@search.facets": { (if faceting was specified in hello query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body toofetch hello next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL toofetch hello next page of results if not all results could be returned in this response; Applies tooboth GET and POST)
    }

<span data-ttu-id="9286d-938">**Példák:**</span><span class="sxs-lookup"><span data-stu-id="9286d-938">**Examples:**</span></span>

<span data-ttu-id="9286d-939">A hello további példákat talál [OData kifejezési szintaxist az Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) lap.</span><span class="sxs-lookup"><span data-stu-id="9286d-939">You can find additional examples on hello [OData Expression Syntax for Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) page.</span></span>

1)    <span data-ttu-id="9286d-940">Keresési hello Index csökkenő dátum szerint.</span><span class="sxs-lookup"><span data-stu-id="9286d-940">Search hello Index sorted descending by date.</span></span>

    <span data-ttu-id="9286d-941">GET /indexes/hotels/docs? keresési = * & $orderby = lastRenovationDate desc & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="9286d-941">GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9286d-942">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "*", "orderby": "lastRenovationDate desc"}</span><span class="sxs-lookup"><span data-stu-id="9286d-942">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "orderby": "lastRenovationDate desc" }</span></span>

2)    <span data-ttu-id="9286d-943">A jellemzőalapú keresés – hello index keresse, és értékkorlátozás a kategóriák, minősítés, címkéket, valamint az adott címtartományok baseRate elemek beolvasása:</span><span class="sxs-lookup"><span data-stu-id="9286d-943">In a faceted search, search hello index and retrieve facets for categories, rating, tags, as well as items with baseRate in specific ranges:</span></span>

    <span data-ttu-id="9286d-944">GET /indexes/hotels/docs? keresési = test & értékkorlátozás = kategória & értékkorlátozás = minősítés & értékkorlátozás = címkék & értékkorlátozás baseRate, érték: 80 = |} 150 |} 220 & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="9286d-944">GET /indexes/hotels/docs?search=test&facet=category&facet=rating&facet=tags&facet=baseRate,values:80|150|220&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9286d-945">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "teszt", "értékkorlátozással": ["kategória", "minősítés", "címkék", "baseRate, érték: 80-as |} 150 |} 220"]}</span><span class="sxs-lookup"><span data-stu-id="9286d-945">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "test", "facets": [ "category", "rating", "tags", "baseRate,values:80|150|220" ] }</span></span>

3)    <span data-ttu-id="9286d-946">Egy szűrő használatával szűkítéséhez hello előző jellemzőalapú lekérdezés eredményeit, miután hello felhasználó a hivatkozásra kattint 3. és a "Motel" kategóriát rangsorolási:</span><span class="sxs-lookup"><span data-stu-id="9286d-946">Using a filter, narrow down hello previous faceted query results after hello user clicks on rating 3 and category "Motel":</span></span>

    <span data-ttu-id="9286d-947">GET /indexes/hotels/docs? keresési = test & értékkorlátozás = címkék & értékkorlátozás baseRate, érték: 80 = |} 150 |} 220 & $filter minősítés eq 3 és kategória-eq "Motel" & api-version = 2015-02-28 – előzetes verzió =</span><span class="sxs-lookup"><span data-stu-id="9286d-947">GET /indexes/hotels/docs?search=test&facet=tags&facet=baseRate,values:80|150|220&$filter=rating eq 3 and category eq 'Motel'&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9286d-948">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "teszt", "értékkorlátozással": ["címkék", "baseRate, érték: 80-as |} 150 |} 220"], "szűrő": "minősítés eq 3 és kategória-eq"Motel""}</span><span class="sxs-lookup"><span data-stu-id="9286d-948">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "test", "facets": [ "tags", "baseRate,values:80|150|220" ], "filter": "rating eq 3 and category eq 'Motel'" }</span></span>

4) <span data-ttu-id="9286d-949">Jellemzőalapú keresés – az egyedi feltételeket a lekérdezés által visszaadott állítson be egy felső korlátot.</span><span class="sxs-lookup"><span data-stu-id="9286d-949">In a faceted search, set an upper limit on unique terms returned in a query.</span></span> <span data-ttu-id="9286d-950">hello alapértelmezés szerint 10, de növelheti vagy csökkentheti a hello segítségével értéket `count` hello paraméter `facet` attribútum:</span><span class="sxs-lookup"><span data-stu-id="9286d-950">hello default is 10, but you can increase or decrease this value using hello `count` parameter on hello `facet` attribute:</span></span>

    <span data-ttu-id="9286d-951">GET /indexes/hotels/docs? keresési = test & értékkorlátozás = város, számláló: 5 & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="9286d-951">GET /indexes/hotels/docs?search=test&facet=city,count:5&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9286d-952">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "teszt", "értékkorlátozással": ["város, számláló: 5"]}</span><span class="sxs-lookup"><span data-stu-id="9286d-952">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview  {    "search": "test",    "facets": [ "city,count:5" ]  }</span></span>

5)    <span data-ttu-id="9286d-953">Keresési hello Index belül adott mezők; Ha például egy nyelvspecifikus mező:</span><span class="sxs-lookup"><span data-stu-id="9286d-953">Search hello Index within specific fields; For example, a language-specific field:</span></span>

    <span data-ttu-id="9286d-954">GET /indexes/hotels/docs? keresési = hôtel & searchFields = description_fr & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="9286d-954">GET /indexes/hotels/docs?search=hôtel&searchFields=description_fr&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9286d-955">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "hôtel", "searchFields": "description_fr"}</span><span class="sxs-lookup"><span data-stu-id="9286d-955">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "hôtel", "searchFields": "description_fr" }</span></span>

6) <span data-ttu-id="9286d-956">Keresési hello Index több mező között.</span><span class="sxs-lookup"><span data-stu-id="9286d-956">Search hello Index across multiple fields.</span></span> <span data-ttu-id="9286d-957">Például is tárolhatja, és a lekérdezés kereshető mezők belül különböző nyelveken hello ugyanazt az indexet.</span><span class="sxs-lookup"><span data-stu-id="9286d-957">For example, you can store and query searchable fields in multiple languages, all within hello same index.</span></span>  <span data-ttu-id="9286d-958">Ha angol és francia leírások működjenek együtt hello azonos dokumentum, lépjen vissza a hello bármely vagy valamennyi lekérdezési eredmények:</span><span class="sxs-lookup"><span data-stu-id="9286d-958">If English and French descriptions co-exist in hello same document, you can return any or all in hello query results:</span></span>

    <span data-ttu-id="9286d-959">GET /indexes/hotels/docs? keresési = Szálloda & searchFields = leírás, description_fr & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="9286d-959">GET /indexes/hotels/docs?search=hotel&searchFields=description,description_fr&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9286d-960">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "Szálloda", "searchFields": "leírás, description_fr"}</span><span class="sxs-lookup"><span data-stu-id="9286d-960">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview  {    "search": "hotel",    "searchFields": "description, description_fr"  }</span></span>

<span data-ttu-id="9286d-961">Vegye figyelembe, hogy csak lekérdezhető egy index egyszerre.</span><span class="sxs-lookup"><span data-stu-id="9286d-961">Note that you can only query one index at a time.</span></span> <span data-ttu-id="9286d-962">Ne hozzon létre az egyes nyelvekhez több indexek, ha azok egy tooquery egyszerre.</span><span class="sxs-lookup"><span data-stu-id="9286d-962">Do not create multiple indexes for each language unless you plan tooquery one at a time.</span></span>

7)    <span data-ttu-id="9286d-963">Lapozófájl - Get hello 1. az elemek oldalára (oldal mérete 10):</span><span class="sxs-lookup"><span data-stu-id="9286d-963">Paging - Get hello 1st page of items (page size is 10):</span></span>

    <span data-ttu-id="9286d-964">GET /indexes/hotels/docs? keresési = * & $skip = 0 & $top = 10 & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="9286d-964">GET /indexes/hotels/docs?search=*&$skip=0&$top=10&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9286d-965">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "*", "kihagyása": 0, a "top": 10}</span><span class="sxs-lookup"><span data-stu-id="9286d-965">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "skip": 0, "top": 10 }</span></span>

8)    <span data-ttu-id="9286d-966">Lapozófájl - Get hello 2. az elemek oldalára (oldal mérete 10):</span><span class="sxs-lookup"><span data-stu-id="9286d-966">Paging - Get hello 2nd page of items (page size is 10):</span></span>

    <span data-ttu-id="9286d-967">GET /indexes/hotels/docs? keresési = * & $skip = 10 & $top = 10 & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="9286d-967">GET /indexes/hotels/docs?search=*&$skip=10&$top=10&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9286d-968">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "*", "kihagyása": "felső" 10: 10}</span><span class="sxs-lookup"><span data-stu-id="9286d-968">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "skip": 10, "top": 10 }</span></span>

9)    <span data-ttu-id="9286d-969">Lekéri egy adott készletét mezők:</span><span class="sxs-lookup"><span data-stu-id="9286d-969">Retrieve a specific set of fields:</span></span>

    <span data-ttu-id="9286d-970">GET /indexes/hotels/docs? keresési = * & $select = hotelName, leírása és api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="9286d-970">GET /indexes/hotels/docs?search=*&$select=hotelName,description&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9286d-971">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "*", "select": "hotelName, leírás"}</span><span class="sxs-lookup"><span data-stu-id="9286d-971">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "select": "hotelName, description" }</span></span>

10)  <span data-ttu-id="9286d-972">A megadott szűrő kifejezésnek megfelelő dokumentumok beolvasása:</span><span class="sxs-lookup"><span data-stu-id="9286d-972">Retrieve documents matching a specific filter expression:</span></span>

    <span data-ttu-id="9286d-973">GET /indexes/hotels/docs? $filter = (baseRate ge 60 és baseRate lt 300) vagy a hotelName eq divatos maradnak, és az api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="9286d-973">GET /indexes/hotels/docs?$filter=(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9286d-974">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"szűrő": "(baseRate ge 60 és baseRate lt 300) hotelName eq"Divatos maradás"vagy"}</span><span class="sxs-lookup"><span data-stu-id="9286d-974">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {  "filter": "(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'" }</span></span>

11) <span data-ttu-id="9286d-975">Keresési hello index és visszatérési töredék találati emeli ki a</span><span class="sxs-lookup"><span data-stu-id="9286d-975">Search hello index and return fragments with hit highlights</span></span>

    <span data-ttu-id="9286d-976">GET /indexes/hotels/docs? keresési valami = & Jelöljön ki leírás & api-version = 2015-02-28-Preview =</span><span class="sxs-lookup"><span data-stu-id="9286d-976">GET /indexes/hotels/docs?search=something&highlight=description&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9286d-977">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "egy", "highlight": "description"}</span><span class="sxs-lookup"><span data-stu-id="9286d-977">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "highlight": "description" }</span></span>

12) <span data-ttu-id="9286d-978">Hello témakörét, és térjen vissza a rendezési sorrend szorosabb toofarther el egy hivatkozási helyre dokumentumok</span><span class="sxs-lookup"><span data-stu-id="9286d-978">Search hello index and return documents sorted from closer toofarther away from a reference location</span></span>

    <span data-ttu-id="9286d-979">GET /indexes/hotels/docs? keresési valami = & $orderby=geo.distance (helyét, geography'POINT(-122.12315 47.88121) ") & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="9286d-979">GET /indexes/hotels/docs?search=something&$orderby=geo.distance(location, geography'POINT(-122.12315 47.88121)')&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9286d-980">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "egy", "orderby": "geo.distance (helyét, geography'POINT(-122.12315 47.88121)") "}</span><span class="sxs-lookup"><span data-stu-id="9286d-980">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "orderby": "geo.distance(location, geography'POINT(-122.12315 47.88121)')" }</span></span>

13) <span data-ttu-id="9286d-981">A pontozási profil "földrajzi" nevű keresési hello index feltéve, hogy két a távolságot pontozó függvények, egy meghatározása a paraméter neve "currentLocation", a másik egy paraméter "lastLocation" meghatározása</span><span class="sxs-lookup"><span data-stu-id="9286d-981">Search hello index assuming there's a scoring profile called "geo" with two distance scoring functions, one defining a parameter called "currentLocation" and one defining a parameter called "lastLocation"</span></span>

    <span data-ttu-id="9286d-982">GET /indexes/hotels/docs? keresési valami = & scoringProfile = földrajzi & scoringParameter currentLocation--122.123,44.77233 & scoringParameter = lastLocation--121.499,44.2113 & api-version = 2015-02-28-Preview =</span><span class="sxs-lookup"><span data-stu-id="9286d-982">GET /indexes/hotels/docs?search=something&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&scoringParameter=lastLocation--121.499,44.2113&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9286d-983">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "egy", "scoringProfile": "földrajzi", "scoringParameters": ["currentLocation--122.123,44.77233", "lastLocation--121.499,44.2113"]}</span><span class="sxs-lookup"><span data-stu-id="9286d-983">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "scoringProfile": "geo",   "scoringParameters": [ "currentLocation--122.123,44.77233", "lastLocation--121.499,44.2113" ] }</span></span>

14) <span data-ttu-id="9286d-984">Dokumentumok keresése a hello index használatával [egyszerű lekérdezés szintaxisát](https://msdn.microsoft.com/library/dn798920.aspx).</span><span class="sxs-lookup"><span data-stu-id="9286d-984">Find documents in hello index using [simple query syntax](https://msdn.microsoft.com/library/dn798920.aspx).</span></span> <span data-ttu-id="9286d-985">A lekérdezés által visszaadott, ha a kereshető mezők tartalmaznak hello feltételek "megerősítő" és "hely", de nem a "motel" hotels:</span><span class="sxs-lookup"><span data-stu-id="9286d-985">This query returns hotels where searchable fields contain hello terms "comfort" and "location" but not "motel":</span></span>

    <span data-ttu-id="9286d-986">GET /indexes/hotels/docs? keresési = megerősítő + hely-motel & searchMode = all & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="9286d-986">GET /indexes/hotels/docs?search=comfort +location -motel&searchMode=all&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="9286d-987">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "megerősítő + hely-motel", "searchMode": "all"}</span><span class="sxs-lookup"><span data-stu-id="9286d-987">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "comfort +location -motel",   "searchMode": "all" }</span></span>

<span data-ttu-id="9286d-988">Vegye figyelembe a hello használata `searchMode=all` felett.</span><span class="sxs-lookup"><span data-stu-id="9286d-988">Note hello use of `searchMode=all` above.</span></span> <span data-ttu-id="9286d-989">Ennek a paraméternek, beleértve a felülbírálások hello alapértelmezett `searchMode=any`, biztosítani kell, hogy `-motel` azt jelenti, hogy ", és nem" helyett ", vagy nincs".</span><span class="sxs-lookup"><span data-stu-id="9286d-989">Including this parameter overrides hello default of `searchMode=any`, ensuring that `-motel` means "AND NOT" instead of "OR NOT".</span></span> <span data-ttu-id="9286d-990">Nélkül `searchMode=all`, ", vagy nem", amely korlátozza a keresési eredmények helyett bővíti kap, és ez lehet counter-intuitive toosome felhasználók.</span><span class="sxs-lookup"><span data-stu-id="9286d-990">Without `searchMode=all`, you get "OR NOT" which expands rather than restricts search results, and this can be counter-intuitive toosome users.</span></span>

15) <span data-ttu-id="9286d-991">Dokumentumok keresése a hello index használatával [lucene lekérdezés szintaxisát](https://msdn.microsoft.com/library/mt589323.aspx).</span><span class="sxs-lookup"><span data-stu-id="9286d-991">Find documents in hello index using [lucene query syntax](https://msdn.microsoft.com/library/mt589323.aspx).</span></span> <span data-ttu-id="9286d-992">A lekérdezés által visszaadott szállodák, ahol hello kategória mező tartalma: hello kifejezés "költségvetés" és "nemrég felújított" hello kifejezést tartalmazó összes kereshető mezőt.</span><span class="sxs-lookup"><span data-stu-id="9286d-992">This query returns hotels where hello category field contains hello term "budget" and all searchable fields containing hello phrase "recently renovated".</span></span> <span data-ttu-id="9286d-993">"Nemrég felújított" hello kifejezést tartalmazó dokumentumokat magasabb rangsora miatt hello kifejezés program érték (3)</span><span class="sxs-lookup"><span data-stu-id="9286d-993">Documents containing hello phrase "recently renovated" are ranked higher as a result of hello term boost value (3)</span></span>

    <span data-ttu-id="9286d-994">GET /indexes/hotels/docs? keresési kategória: költségvetés = és \"nemrég felújított\"^ 3 & searchMode = all & api-version = 2015-02-28-Preview & querytype teljes =</span><span class="sxs-lookup"><span data-stu-id="9286d-994">GET /indexes/hotels/docs?search=category:budget AND \"recently renovated\"^3&searchMode=all&api-version=2015-02-28-Preview&querytype=full</span></span>

    <span data-ttu-id="9286d-995">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "kategória: költségvetés és \"nemrég felújított\"^ 3", "queryType": "teljes", "searchMode": "all"}</span><span class="sxs-lookup"><span data-stu-id="9286d-995">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "category:budget AND \"recently renovated\"^3",   "queryType": "full",   "searchMode": "all" }</span></span>

<a name="LookupAPI"></a>

## <a name="lookup-document"></a><span data-ttu-id="9286d-996">Keresés a dokumentum</span><span class="sxs-lookup"><span data-stu-id="9286d-996">Lookup Document</span></span>
<span data-ttu-id="9286d-997">Hello **keresési dokumentum** Azure keresési művelet lekérdezi egy dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="9286d-997">hello **Lookup Document** operation retrieves a document from Azure Search.</span></span> <span data-ttu-id="9286d-998">Ez akkor hasznos, ha a felhasználó egy adott keresési eredmény kattint, és azt szeretné, hogy toolook fel a dokumentum részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="9286d-998">This is useful when a user clicks on a specific search result, and you want toolook up specific details about that document.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

<span data-ttu-id="9286d-999">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="9286d-999">**Request**</span></span>

<span data-ttu-id="9286d-1000">HTTPS szolgáltatáskérések szükség.</span><span class="sxs-lookup"><span data-stu-id="9286d-1000">HTTPS is required for service requests.</span></span> <span data-ttu-id="9286d-1001">Hello **keresési dokumentum** kérelmek az alábbiak szerint lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9286d-1001">hello **Lookup Document** request can be constructed as follows.</span></span>

    GET /indexes/[index name]/docs/key?[query parameters]

<span data-ttu-id="9286d-1002">Alternatív megoldásként használható hello hagyományos OData szintaxis kulcskeresési:</span><span class="sxs-lookup"><span data-stu-id="9286d-1002">Alternatively, you can use hello traditional OData syntax for key lookup:</span></span>

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

<span data-ttu-id="9286d-1003">hello kérelem URI tartalmaz egy [index neve] és [kulcs], mely dokumentum tooretrieve mely indexből megadása.</span><span class="sxs-lookup"><span data-stu-id="9286d-1003">hello request URI includes an [index name] and [key], specifying which document tooretrieve from which index.</span></span> <span data-ttu-id="9286d-1004">Egyszerre csak egy dokumentum kérheti le.</span><span class="sxs-lookup"><span data-stu-id="9286d-1004">You can only get one document at a time.</span></span> <span data-ttu-id="9286d-1005">Használjon **keresési** tooget egyetlen kérelemben több dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="9286d-1005">Use **Search** tooget multiple documents in a single request.</span></span>

<span data-ttu-id="9286d-1006">**Lekérdezés-paraméterek**</span><span class="sxs-lookup"><span data-stu-id="9286d-1006">**Query Parameters**</span></span>

<span data-ttu-id="9286d-1007">`$select=[string]`(választható) - tooretrieve mezők vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="9286d-1007">`$select=[string]` (optional) - a list of comma-separated fields tooretrieve.</span></span> <span data-ttu-id="9286d-1008">Ha nincs megadva, vagy túl`*`, hello séma szerint lekérhető megjelölt összes mezők szerepelnek hello leképezés.</span><span class="sxs-lookup"><span data-stu-id="9286d-1008">If unspecified or set too`*`, all fields marked as retrievable in hello schema are included in hello projection.</span></span>

<span data-ttu-id="9286d-1009">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="9286d-1009">`api-version=[string]` (required).</span></span> <span data-ttu-id="9286d-1010">hello előzetes verziója `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1010">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9286d-1011">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="9286d-1011">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9286d-1012">Megjegyzés: Ehhez a művelethez hello `api-version` egy lekérdezési paraméter van megadva.</span><span class="sxs-lookup"><span data-stu-id="9286d-1012">Note: For this operation, hello `api-version` is specified as a query parameter.</span></span>

<span data-ttu-id="9286d-1013">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="9286d-1013">**Request Headers**</span></span>

<span data-ttu-id="9286d-1014">a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9286d-1014">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9286d-1015">`api-key`: hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9286d-1015">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9286d-1016">Egy karakterláncértéket, egyedi tooyour szolgáltatás URL-címe.</span><span class="sxs-lookup"><span data-stu-id="9286d-1016">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="9286d-1017">Hello **keresési dokumentum** kérelem adhat meg, egy adminisztrációs kulcsot vagy egy lekérdezés kulcs `api-key`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1017">hello **Lookup Document** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="9286d-1018">Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="9286d-1018">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9286d-1019">Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9286d-1019">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9286d-1020">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="9286d-1020">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9286d-1021">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="9286d-1021">**Request Body**</span></span>

<span data-ttu-id="9286d-1022">nincs.</span><span class="sxs-lookup"><span data-stu-id="9286d-1022">None.</span></span>

<span data-ttu-id="9286d-1023">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="9286d-1023">**Response**</span></span>

<span data-ttu-id="9286d-1024">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="9286d-1024">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      field_name: field_value (fields matching hello default or specified projection)
    }

<span data-ttu-id="9286d-1025">**Példa**</span><span class="sxs-lookup"><span data-stu-id="9286d-1025">**Example**</span></span>

<span data-ttu-id="9286d-1026">Keresési hello dokumentum kulcs '2'</span><span class="sxs-lookup"><span data-stu-id="9286d-1026">Lookup hello document that has key '2'</span></span>

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

<span data-ttu-id="9286d-1027">Keresési hello dokumentum kulcs "3" OData-szintaxis használatával:</span><span class="sxs-lookup"><span data-stu-id="9286d-1027">Lookup hello document that has key '3' using OData syntax:</span></span>

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>

## <a name="count-documents"></a><span data-ttu-id="9286d-1028">A dokumentumok száma</span><span class="sxs-lookup"><span data-stu-id="9286d-1028">Count Documents</span></span>
<span data-ttu-id="9286d-1029">Hello **száma dokumentumok** művelet lekérdezi egy keresési indexszel dokumentumok száma hello számát.</span><span class="sxs-lookup"><span data-stu-id="9286d-1029">hello **Count Documents** operation retrieves a count of hello number of documents in a search index.</span></span> <span data-ttu-id="9286d-1030">Hello `$count` szintaxis hello OData protokoll részét képezi.</span><span class="sxs-lookup"><span data-stu-id="9286d-1030">hello `$count` syntax is part of hello OData protocol.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

<span data-ttu-id="9286d-1031">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="9286d-1031">**Request**</span></span>

<span data-ttu-id="9286d-1032">HTTPS szolgáltatáskérések szükség.</span><span class="sxs-lookup"><span data-stu-id="9286d-1032">HTTPS is required for service requests.</span></span> <span data-ttu-id="9286d-1033">Hello **száma dokumentumok** kérelmek GET metódussal hello lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9286d-1033">hello **Count Documents** request can be constructed using hello GET method.</span></span>

<span data-ttu-id="9286d-1034">hello [index neve] hello kérelem URI-azonosítója az összes elemek számát közli hello szolgáltatás tooreturn hello docs gyűjteményében hello megadott index.</span><span class="sxs-lookup"><span data-stu-id="9286d-1034">hello [index name] in hello request URI tells hello service tooreturn a count of all items in hello docs collection of hello specified index.</span></span>

<span data-ttu-id="9286d-1035">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="9286d-1035">`api-version=[string]` (required).</span></span> <span data-ttu-id="9286d-1036">hello előzetes verziója `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1036">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9286d-1037">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="9286d-1037">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9286d-1038">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="9286d-1038">**Request Headers**</span></span>

<span data-ttu-id="9286d-1039">a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9286d-1039">hello following list describes hello required and optional request headers.</span></span>

* <span data-ttu-id="9286d-1040">`Accept`: Ez az érték túl be kell állítani`text/plain`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1040">`Accept`: This value must be set too`text/plain`.</span></span>
* <span data-ttu-id="9286d-1041">`api-key`: hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9286d-1041">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9286d-1042">Egy karakterláncértéket, egyedi tooyour szolgáltatás URL-címe.</span><span class="sxs-lookup"><span data-stu-id="9286d-1042">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="9286d-1043">Hello **száma dokumentumok** kérelem adhat meg, egy adminisztrációs kulcsot vagy egy lekérdezés kulcs `api-key`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1043">hello **Count Documents** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="9286d-1044">Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="9286d-1044">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9286d-1045">Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9286d-1045">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9286d-1046">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="9286d-1046">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9286d-1047">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="9286d-1047">**Request Body**</span></span>

<span data-ttu-id="9286d-1048">nincs.</span><span class="sxs-lookup"><span data-stu-id="9286d-1048">None.</span></span>

<span data-ttu-id="9286d-1049">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="9286d-1049">**Response**</span></span>

<span data-ttu-id="9286d-1050">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="9286d-1050">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="9286d-1051">hello adott válasz törzsének hello számérték tartalmaz egy egyszerű szöveges formátumú egész számként.</span><span class="sxs-lookup"><span data-stu-id="9286d-1051">hello response body contains hello count value as an integer formatted in plain text.</span></span>

<a name="Suggestions"></a>

## <a name="suggestions"></a><span data-ttu-id="9286d-1052">Javaslatok</span><span class="sxs-lookup"><span data-stu-id="9286d-1052">Suggestions</span></span>
<span data-ttu-id="9286d-1053">Hello **javaslatok** művelet lekérdezi a javaslatok generálása részleges kereséshez bemeneti alapján.</span><span class="sxs-lookup"><span data-stu-id="9286d-1053">hello **Suggestions** operation retrieves suggestions based on partial search input.</span></span> <span data-ttu-id="9286d-1054">Általában használatba a keresési mezőbe tooprovide begépelt javaslatok, a keresési feltételeket ad felhasználók.</span><span class="sxs-lookup"><span data-stu-id="9286d-1054">It's typically used in search boxes tooprovide type-ahead suggestions as users are entering search terms.</span></span>

<span data-ttu-id="9286d-1055">Javaslat kérelmek célja a cél dokumentumok arra vonatkozóan, így hello javasolt szöveg megismételhető, ha több jelölt dokumentumok egyeznek hello azonos keressen-e bemeneti.</span><span class="sxs-lookup"><span data-stu-id="9286d-1055">Suggestion requests aim at suggesting target documents, so hello suggested text may be repeated if multiple candidate documents match hello same search input.</span></span> <span data-ttu-id="9286d-1056">Használhat `$select` más tooretrieve dokumentálja a mezők (hello dokumentum kulcsot is beleértve), így állapítható meg, melyik dokumentum minden javaslat hello forrása.</span><span class="sxs-lookup"><span data-stu-id="9286d-1056">You can use `$select` tooretrieve other document fields (including hello document key) so that you can tell which document is hello source for each suggestion.</span></span>

<span data-ttu-id="9286d-1057">A **javaslatok** művelet GET vagy POST-kérelmet ad ki.</span><span class="sxs-lookup"><span data-stu-id="9286d-1057">A **Suggestions** operation is issued as a GET or POST request.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

<span data-ttu-id="9286d-1058">**Amikor toouse utáni helyett**</span><span class="sxs-lookup"><span data-stu-id="9286d-1058">**When toouse POST instead of GET**</span></span>

<span data-ttu-id="9286d-1059">Ha használja a HTTP GET toocall hello **javaslatok** toobe vegye figyelembe, hogy a hello kérelem URL-címe hello hossza nem haladhatja meg a 8 KB-os API-t kell.</span><span class="sxs-lookup"><span data-stu-id="9286d-1059">When you use HTTP GET toocall hello **Suggestions** API, you need toobe aware that hello length of hello request URL cannot exceed 8 KB.</span></span> <span data-ttu-id="9286d-1060">Ez elég általában a legtöbb alkalmazás esetén.</span><span class="sxs-lookup"><span data-stu-id="9286d-1060">This is usually enough for most applications.</span></span> <span data-ttu-id="9286d-1061">Egyes alkalmazások azonban nagyon nagy lekérdezések, kifejezetten OData szűrőkifejezéseket eredményez.</span><span class="sxs-lookup"><span data-stu-id="9286d-1061">However, some applications produce very large queries, specifically OData filter expressions.</span></span> <span data-ttu-id="9286d-1062">Ezek az alkalmazások a HTTP POST jobb választás, mivel használata esetén lehetővé teszi a GET-nál nagyobb szűrők.</span><span class="sxs-lookup"><span data-stu-id="9286d-1062">For these applications, using HTTP POST is a better choice because it allows larger filters than GET.</span></span> <span data-ttu-id="9286d-1063">A FELADÁS egy vagy több egy szűrő záradékok hello száma hello korlátozó tényezővé, nem hello hello nyers szűrési karakterláncot mérete, mert hello POST kérelem méretkorlátot nagyjából 16 MB.</span><span class="sxs-lookup"><span data-stu-id="9286d-1063">With POST, hello number of clauses in a filter is hello limiting factor, not hello size of hello raw filter string since hello request size limit for POST is approximately 16 MB.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-1064">Annak ellenére, hogy hello POST kérelem méretkorlátot nagyon nagy, szűrőkifejezéseket önkényesen összetett nem lehet.</span><span class="sxs-lookup"><span data-stu-id="9286d-1064">Even though hello POST request size limit is very large, filter expressions cannot be arbitrarily complex.</span></span> <span data-ttu-id="9286d-1065">Lásd: [OData-kifejezésszintaxist](https://msdn.microsoft.com/library/dn798921.aspx) szűrő összetettsége korlátozásaival kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="9286d-1065">See [OData expression syntax](https://msdn.microsoft.com/library/dn798921.aspx) for more information about filter complexity limitations.</span></span>
> 
> 

<span data-ttu-id="9286d-1066">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="9286d-1066">**Request**</span></span>

<span data-ttu-id="9286d-1067">HTTPS szolgáltatáskérések szükség.</span><span class="sxs-lookup"><span data-stu-id="9286d-1067">HTTPS is required for service requests.</span></span> <span data-ttu-id="9286d-1068">Hello **javaslatok** kérelem hello GET vagy POST metódus használatával lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9286d-1068">hello **Suggestions** request can be constructed using hello GET or POST methods.</span></span>

<span data-ttu-id="9286d-1069">hello kérelem URI-azonosítója hello index tooquery hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="9286d-1069">hello request URI specifies hello name of hello index tooquery.</span></span> <span data-ttu-id="9286d-1070">Paraméterek, például hello részben bemeneti keresőkifejezéssel, hello lekérdezési karakterlánc hello esetben a GET kérelmek nincsenek megadva, és hello kérelem törzse hello esetben POST kérelmek.</span><span class="sxs-lookup"><span data-stu-id="9286d-1070">Parameters, such as hello partially input search term, are specified on hello query string in hello case of GET requests, and in hello request body in hello case of POST requests.</span></span>

<span data-ttu-id="9286d-1071">Ajánlott eljárásként GET kérelmek létrehozásakor, ne feledje túl[URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) meghatározott lekérdezési paraméterek meghívásakor közvetlenül hello REST API-t.</span><span class="sxs-lookup"><span data-stu-id="9286d-1071">As a best practice when creating GET requests, remember too[URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specific query parameters when calling hello REST API directly.</span></span> <span data-ttu-id="9286d-1072">A **javaslatok** műveleteket, ilyenek:</span><span class="sxs-lookup"><span data-stu-id="9286d-1072">For **Suggestions** operations, this includes:</span></span>

* `$filter`
* `highlightPreTag`
* `highlightPostTag`
* `search`

<span data-ttu-id="9286d-1073">URL-kódolást csak ajánlott hello fent lekérdezési paramétereket.</span><span class="sxs-lookup"><span data-stu-id="9286d-1073">URL encoding is only recommended on hello above query parameters.</span></span> <span data-ttu-id="9286d-1074">Ha akkor véletlenül URL-encode hello teljes lekérdezési karakterlánc (mindent után hello?), a kérelmek megszakad.</span><span class="sxs-lookup"><span data-stu-id="9286d-1074">If you inadvertently URL-encode hello entire query string (everything after hello ?), requests will break.</span></span>

<span data-ttu-id="9286d-1075">URL-kódolást is csak szükséges REST API használatával közvetlenül LEKÉRNI hello hívásakor.</span><span class="sxs-lookup"><span data-stu-id="9286d-1075">Also, URL encoding is only necessary when calling hello REST API directly using GET.</span></span> <span data-ttu-id="9286d-1076">Kódolás nélkül URL-cím szükség, ha hívása **javaslatok** használatával a FELADÁS egy vagy több, vagy hello használatakor [.NET ügyféloldali kódtár](https://msdn.microsoft.com/library/dn951165.aspx), amely alkalmazás URL-címet meg.</span><span class="sxs-lookup"><span data-stu-id="9286d-1076">No URL encoding is necessary when calling **Suggestions** using POST, or when using hello [.NET client library](https://msdn.microsoft.com/library/dn951165.aspx), which handles URL encoding for you.</span></span>

<span data-ttu-id="9286d-1077">**Lekérdezés-paraméterek**</span><span class="sxs-lookup"><span data-stu-id="9286d-1077">**Query Parameters**</span></span>

<span data-ttu-id="9286d-1078">**Javaslatok** lekérdezés feltételeket, és megadhatja a keresés viselkedését több paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="9286d-1078">**Suggestions** accepts several parameters that provide query criteria and also specify search behavior.</span></span> <span data-ttu-id="9286d-1079">Megadja, hogy ezek a paraméterek hello URL-címet a lekérdezési karakterlánc meghívásakor **javaslatok** GET keresztül, és JSON tulajdonságként hello kérés törzsében meghívásakor **javaslatok** POST keresztül.</span><span class="sxs-lookup"><span data-stu-id="9286d-1079">You provide these parameters in hello URL query string when calling **Suggestions** via GET, and as JSON properties in hello request body when calling **Suggestions** via POST.</span></span> <span data-ttu-id="9286d-1080">egyes paraméterek hello szintaxisa némileg eltérő GET és POST között.</span><span class="sxs-lookup"><span data-stu-id="9286d-1080">hello syntax for some parameters is slightly different between GET and POST.</span></span> <span data-ttu-id="9286d-1081">Ezek a különbségek jeleztük megfelelő alatt:</span><span class="sxs-lookup"><span data-stu-id="9286d-1081">These differences are noted as applicable below:</span></span>

<span data-ttu-id="9286d-1082">`search=[string]`-hello keresési szöveg toouse toosuggest lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="9286d-1082">`search=[string]` - hello search text toouse toosuggest queries.</span></span> <span data-ttu-id="9286d-1083">Legalább 1 karakter, és legfeljebb 100 karakter lehet.</span><span class="sxs-lookup"><span data-stu-id="9286d-1083">Must be at least 1 character, and no more than 100 characters.</span></span>

<span data-ttu-id="9286d-1084">`highlightPreTag=[string]`(választható) - toosearch találatok lefoglalja egy karakterlánc címkét.</span><span class="sxs-lookup"><span data-stu-id="9286d-1084">`highlightPreTag=[string]` (optional) - a string tag that prepends toosearch hits.</span></span> <span data-ttu-id="9286d-1085">Meg kell a `highlightPostTag`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1085">Must be set with `highlightPostTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-1086">Meghívásakor **javaslatok** GET használ, foglalt karaktereket: hello URL-címben százalék-kódolással kell (például % &#23; helyett).</span><span class="sxs-lookup"><span data-stu-id="9286d-1086">When calling **Suggestions** using GET, reserved characters in hello URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="9286d-1087">`highlightPostTag=[string]`(választható) - toosearch találatok hozzáfűz egy karakterlánc címkét.</span><span class="sxs-lookup"><span data-stu-id="9286d-1087">`highlightPostTag=[string]` (optional) - a string tag that appends toosearch hits.</span></span> <span data-ttu-id="9286d-1088">Meg kell a `highlightPreTag`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1088">Must be set with `highlightPreTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-1089">Meghívásakor **javaslatok** GET használ, foglalt karaktereket: hello URL-címben százalék-kódolással kell (például % &#23; helyett).</span><span class="sxs-lookup"><span data-stu-id="9286d-1089">When calling **Suggestions** using GET, reserved characters in hello URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="9286d-1090">`suggesterName=[string]`-hello nevét, a megadott hello hello javaslattevő `suggesters` gyűjteményt, amely hello Indexdefiníció része.</span><span class="sxs-lookup"><span data-stu-id="9286d-1090">`suggesterName=[string]` - hello name of hello suggester as specified in hello `suggesters` collection that's part of hello index definition.</span></span> <span data-ttu-id="9286d-1091">A `suggester` meghatározza, hogy melyik mezőkre javasolt lekérdezési kifejezések vizsgálata.</span><span class="sxs-lookup"><span data-stu-id="9286d-1091">A `suggester` determines which fields are scanned for suggested query terms.</span></span> <span data-ttu-id="9286d-1092">Lásd: [Javaslattevők](#Suggesters) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="9286d-1092">See [Suggesters](#Suggesters) for details.</span></span>

<span data-ttu-id="9286d-1093">`fuzzy=[boolean]`(nem kötelező, alapértelmezett = false) – Ha állítsa tootrue API megkeresi a javaslatok, még akkor is, ha a helyettesített vagy hiányzó karakter szerepel hello keresett szöveg.</span><span class="sxs-lookup"><span data-stu-id="9286d-1093">`fuzzy=[boolean]` (optional, default = false) - when set tootrue this API will find suggestions even if there's a substituted or missing character in hello search text.</span></span> <span data-ttu-id="9286d-1094">Amíg ez bizonyos esetekben jobb élményt nyújt a teljesítményt, intelligens javaslat keresések lassabban futnak, és több erőforrást, származik.</span><span class="sxs-lookup"><span data-stu-id="9286d-1094">While this provides a better experience in some scenarios it comes at a performance cost as fuzzy suggestion searches are slower and consume more resources.</span></span>

<span data-ttu-id="9286d-1095">`searchFields=[string]`(nem kötelező) – hello listájának vesszővel tagolt mező nevének toosearch hello a megadott keresési szöveg.</span><span class="sxs-lookup"><span data-stu-id="9286d-1095">`searchFields=[string]` (optional) - hello list of comma-separated field names toosearch for hello specified search text.</span></span> <span data-ttu-id="9286d-1096">Célmező javaslatok engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="9286d-1096">Target fields must be enabled for suggestions.</span></span>

<span data-ttu-id="9286d-1097">`$top=#`(nem kötelező, alapértelmezett = 5) – hello javaslatok tooretrieve száma.</span><span class="sxs-lookup"><span data-stu-id="9286d-1097">`$top=#` (optional, default = 5) - hello number of suggestions tooretrieve.</span></span> <span data-ttu-id="9286d-1098">1 és 100 közötti számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9286d-1098">Must be a number between 1 and 100.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-1099">Meghívásakor **javaslatok** POST használ, ez a paraméter neve `top` helyett `$top`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1099">When calling **Suggestions** using POST, this parameter is named `top` instead of `$top`.</span></span>
> 
> 

<span data-ttu-id="9286d-1100">`$filter=[string]`(nem kötelező) – hello dokumentumok szűrésére szolgáló kifejezés javaslatok figyelembe venni.</span><span class="sxs-lookup"><span data-stu-id="9286d-1100">`$filter=[string]` (optional) - an expression that filters hello documents considered for suggestions.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-1101">Meghívásakor **javaslatok** POST használ, ez a paraméter neve `filter` helyett `$filter`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1101">When calling **Suggestions** using POST, this parameter is named `filter` instead of `$filter`.</span></span>
> 
> 

<span data-ttu-id="9286d-1102">`$orderby=[string]`(választható) - kifejezések vesszővel tagolt toosort hello eredményeket listáját.</span><span class="sxs-lookup"><span data-stu-id="9286d-1102">`$orderby=[string]` (optional) - a list of comma-separated expressions toosort hello results by.</span></span> <span data-ttu-id="9286d-1103">Minden egyes kifejezés lehet, vagy egy mező nevét, vagy egy hívás toohello `geo.distance()` függvény.</span><span class="sxs-lookup"><span data-stu-id="9286d-1103">Each expression can be either a field name or a call toohello `geo.distance()` function.</span></span> <span data-ttu-id="9286d-1104">Minden egyes kifejezés követhetnek `asc` tooindicated növekvő, és `desc` csökkenő tooindicate.</span><span class="sxs-lookup"><span data-stu-id="9286d-1104">Each expression can be followed by `asc` tooindicated ascending, and `desc` tooindicate descending.</span></span> <span data-ttu-id="9286d-1105">hello alapértelmezett növekvő sorrend megadásához.</span><span class="sxs-lookup"><span data-stu-id="9286d-1105">hello default is ascending order.</span></span> <span data-ttu-id="9286d-1106">Egy legfeljebb 32 záradékok `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1106">There is a limit of 32 clauses for `$orderby`.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-1107">Meghívásakor **javaslatok** POST használ, ez a paraméter neve `orderby` helyett `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1107">When calling **Suggestions** using POST, this parameter is named `orderby` instead of `$orderby`.</span></span>
> 
> 

<span data-ttu-id="9286d-1108">`$select=[string]`(választható) - tooretrieve mezők vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="9286d-1108">`$select=[string]` (optional) - a list of comma-separated fields tooretrieve.</span></span> <span data-ttu-id="9286d-1109">Ha nincs megadva, csak a dokumentum kulcs hello és javaslat szöveget ad vissza.</span><span class="sxs-lookup"><span data-stu-id="9286d-1109">If unspecified, only hello document key and suggestion text is returned.</span></span> <span data-ttu-id="9286d-1110">Ez a paraméter túl beállításával explicit módon kérhet minden mező`*`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1110">You can explicitly request all fields by setting this parameter too`*`.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-1111">Meghívásakor **javaslatok** POST használ, ez a paraméter neve `select` helyett `$select`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1111">When calling **Suggestions** using POST, this parameter is named `select` instead of `$select`.</span></span>
> 
> 

<span data-ttu-id="9286d-1112">`minimumCoverage`(nem kötelező, alapértelmezés szerint használt érték too80) – sikeres jelentett 0 és 100 hello százalékos hello index, át kell gondolni a javaslatok lekérdezés ahhoz, hogy hello lekérdezés toobe jelző közötti szám.</span><span class="sxs-lookup"><span data-stu-id="9286d-1112">`minimumCoverage` (optional, defaults too80) - a number between 0 and 100 indicating hello percentage of hello index that must be covered by a suggestions query in order for hello query toobe reported as a success.</span></span> <span data-ttu-id="9286d-1113">Alapértelmezés szerint legalább 80 %-át hello index elérhetőnek kell lennie, vagy `Suggest` 503-as HTTP-állapotkód: ad vissza.</span><span class="sxs-lookup"><span data-stu-id="9286d-1113">By default, at least 80% of hello index must be available or `Suggest` will return HTTP status code 503.</span></span> <span data-ttu-id="9286d-1114">Ha `minimumCoverage` és `Suggest` sikeres, térjen vissza a 200-as HTTP, és tartalmazzák a `@search.coverage` hello válaszban hello index hello lekérdezésben szereplő hello százalékos értékét.</span><span class="sxs-lookup"><span data-stu-id="9286d-1114">If you set `minimumCoverage` and `Suggest` succeeds, it will return HTTP 200 and include a `@search.coverage` value in hello response indicating hello percentage of hello index that was included in hello query.</span></span>

> [!NOTE]
> <span data-ttu-id="9286d-1115">Ha az érték paraméter tooa kisebb, mint 100 search rendelkezésre állását, még a szolgáltatások csak egy replika azért hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="9286d-1115">Setting this parameter tooa value lower than 100 can be useful for ensuring search availability even for services with only one replica.</span></span> <span data-ttu-id="9286d-1116">Azonban nem minden egyező javaslatok garantáltan toobe hello eredmények szerepel.</span><span class="sxs-lookup"><span data-stu-id="9286d-1116">However, not all matching suggestions are guaranteed toobe present in hello results.</span></span> <span data-ttu-id="9286d-1117">Ha visszaírási fontosabb tooyour alkalmazás, mint a rendelkezésre állási, akkor az ajánlott nem toolower `minimumCoverage` a 80-as alapértelmezett érték alá.</span><span class="sxs-lookup"><span data-stu-id="9286d-1117">If recall is more important tooyour application than availability, then it's best not toolower `minimumCoverage` below its default value of 80.</span></span>
> 
> 

<span data-ttu-id="9286d-1118">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="9286d-1118">`api-version=[string]` (required).</span></span> <span data-ttu-id="9286d-1119">hello előzetes verziója `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1119">hello preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="9286d-1120">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="9286d-1120">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="9286d-1121">Megjegyzés: Ehhez a művelethez hello `api-version` hello URL-címében, függetlenül attól, hogy meghívja a lekérdezési paraméter van megadva **javaslatok** GET vagy POST.</span><span class="sxs-lookup"><span data-stu-id="9286d-1121">Note: For this operation, hello `api-version` is specified as a query parameter in hello URL regardless of whether you call **Suggestions** with GET or POST.</span></span>

<span data-ttu-id="9286d-1122">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="9286d-1122">**Request Headers**</span></span>

<span data-ttu-id="9286d-1123">a következő lista hello hello szükséges és választható kérelemfejléc ismerteti</span><span class="sxs-lookup"><span data-stu-id="9286d-1123">hello following list describes hello required and optional request headers</span></span>

* <span data-ttu-id="9286d-1124">`api-key`: hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9286d-1124">`api-key`: hello `api-key` is used tooauthenticate hello request tooyour Search service.</span></span> <span data-ttu-id="9286d-1125">Egy karakterláncértéket, egyedi tooyour szolgáltatás URL-címe.</span><span class="sxs-lookup"><span data-stu-id="9286d-1125">It is a string value, unique tooyour service URL.</span></span> <span data-ttu-id="9286d-1126">Hello **javaslatok** kérelem megadható egy adminisztrációs kulcsot vagy egy lekérdezési kulcsot hello `api-key`.</span><span class="sxs-lookup"><span data-stu-id="9286d-1126">hello **Suggestions** request can specify either an admin key or query key as hello `api-key`.</span></span>

<span data-ttu-id="9286d-1127">Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="9286d-1127">You will also need hello service name tooconstruct hello request URL.</span></span> <span data-ttu-id="9286d-1128">Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9286d-1128">You can get hello service name and `api-key` from your service dashboard in hello Azure Portal.</span></span> <span data-ttu-id="9286d-1129">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="9286d-1129">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="9286d-1130">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="9286d-1130">**Request Body**</span></span>

<span data-ttu-id="9286d-1131">A GET: nincs.</span><span class="sxs-lookup"><span data-stu-id="9286d-1131">For GET: None.</span></span>

<span data-ttu-id="9286d-1132">A FELADÁS egy vagy több:</span><span class="sxs-lookup"><span data-stu-id="9286d-1132">For POST:</span></span>

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered toodeclare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

<span data-ttu-id="9286d-1133">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="9286d-1133">**Response**</span></span>

<span data-ttu-id="9286d-1134">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="9286d-1134">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

<span data-ttu-id="9286d-1135">Ha hello leképezése beállítás használatban tooretrieve mezők szerepelnek a hello tömb egyes elemei:</span><span class="sxs-lookup"><span data-stu-id="9286d-1135">If hello projection option is used tooretrieve fields they are included in each element of hello array:</span></span>

    {
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

<span data-ttu-id="9286d-1136">**Példa**</span><span class="sxs-lookup"><span data-stu-id="9286d-1136">**Example**</span></span>

<span data-ttu-id="9286d-1137">Ahol a hello részleges kereséshez bemeneti érték a "lux" 5 javaslatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="9286d-1137">Retrieve 5 suggestions where hello partial search input is 'lux'</span></span>

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
