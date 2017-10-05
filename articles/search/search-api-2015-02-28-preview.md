---
title: "Az Azure Search szolgáltatás REST API-verzió 2015-02-28 – előzetes verzió |} Microsoft Docs"
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
ms.openlocfilehash: e6ad5c964bfa8421be2706cb4015980e01a271b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a><span data-ttu-id="f0252-103">Az Azure Search szolgáltatás REST API-ja: Verzió 2015-02-28 – előzetes</span><span class="sxs-lookup"><span data-stu-id="f0252-103">Azure Search Service REST API: Version 2015-02-28-Preview</span></span>
<span data-ttu-id="f0252-104">Ez a cikk a referenciadokumentációt tartalmaz `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="f0252-104">This article is the reference documentation for `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="f0252-105">Ez az előnézet kibővíti az aktuális általánosan elérhető verzió [api-version = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), a következő kísérleti funkciók megadásával:</span><span class="sxs-lookup"><span data-stu-id="f0252-105">This preview extends the current generally available version, [api-version=2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), by providing the following experimental features:</span></span>

* <span data-ttu-id="f0252-106">`moreLikeThis`a lekérdezésparaméter a [dokumentumok keresése](#SearchDocs) API.</span><span class="sxs-lookup"><span data-stu-id="f0252-106">`moreLikeThis` query parameter in the [Search Documents](#SearchDocs) API.</span></span> <span data-ttu-id="f0252-107">Más-más adott dokumentumra vonatkozó dokumentumok találja meg.</span><span class="sxs-lookup"><span data-stu-id="f0252-107">It finds other documents that are relevant to another specific document.</span></span>

<span data-ttu-id="f0252-108">Néhány további részei a `2015-02-28-Preview` REST API-t a külön-külön szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="f0252-108">A few additional parts of the `2015-02-28-Preview` REST API are documented separately.</span></span> <span data-ttu-id="f0252-109">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="f0252-109">These include:</span></span>

* [<span data-ttu-id="f0252-110">A pontozási profil</span><span class="sxs-lookup"><span data-stu-id="f0252-110">Scoring Profiles</span></span>](search-api-scoring-profiles-2015-02-28-preview.md)
* [<span data-ttu-id="f0252-111">Indexelők</span><span class="sxs-lookup"><span data-stu-id="f0252-111">Indexers</span></span>](search-api-indexers-2015-02-28-preview.md)

<span data-ttu-id="f0252-112">Az Azure Search szolgáltatás több verzióban érhető el.</span><span class="sxs-lookup"><span data-stu-id="f0252-112">Azure Search service is available in multiple versions.</span></span> <span data-ttu-id="f0252-113">Tekintse meg [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="f0252-113">Please refer to [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details.</span></span>

## <a name="apis-in-this-document"></a><span data-ttu-id="f0252-114">Ez a dokumentum API-k</span><span class="sxs-lookup"><span data-stu-id="f0252-114">APIs in this document</span></span>
<span data-ttu-id="f0252-115">Az Azure Search szolgáltatás API API-műveleteket támogatja a két URL-cím szintaxissal: egyszerű és OData (lásd: [OData (Azure Search API) támogatása](http://msdn.microsoft.com/library/azure/dn798932.aspx) részletekért).</span><span class="sxs-lookup"><span data-stu-id="f0252-115">Azure Search service API supports two URL syntaxes for API operations: simple and OData (see [Support for OData (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) for details).</span></span> <span data-ttu-id="f0252-116">Az alábbi lista egyszerű szintaxisát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f0252-116">The following list shows the simple syntax.</span></span>

[<span data-ttu-id="f0252-117">Index létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0252-117">Create Index</span></span>](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[<span data-ttu-id="f0252-118">Index frissítése</span><span class="sxs-lookup"><span data-stu-id="f0252-118">Update Index</span></span>](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="f0252-119">Index beolvasása</span><span class="sxs-lookup"><span data-stu-id="f0252-119">Get Index</span></span>](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="f0252-120">Indexek listázása</span><span class="sxs-lookup"><span data-stu-id="f0252-120">Listing Indexes</span></span>](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[<span data-ttu-id="f0252-121">Megtekintheti a statisztikákat Index</span><span class="sxs-lookup"><span data-stu-id="f0252-121">Get Index Statistics</span></span>](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[<span data-ttu-id="f0252-122">Teszt elemző eszköz</span><span class="sxs-lookup"><span data-stu-id="f0252-122">Test Analyzer</span></span>](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[<span data-ttu-id="f0252-123">Index törlése</span><span class="sxs-lookup"><span data-stu-id="f0252-123">Delete an Index</span></span>](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[<span data-ttu-id="f0252-124">Hozzáadása, törlése, és az Index adatainak frissítése</span><span class="sxs-lookup"><span data-stu-id="f0252-124">Add, Delete, and Update Data within an Index</span></span>](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[<span data-ttu-id="f0252-125">Dokumentumok keresése</span><span class="sxs-lookup"><span data-stu-id="f0252-125">Search Documents</span></span>](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[<span data-ttu-id="f0252-126">Keresés a dokumentum</span><span class="sxs-lookup"><span data-stu-id="f0252-126">Lookup Document</span></span>](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[<span data-ttu-id="f0252-127">A dokumentumok száma</span><span class="sxs-lookup"><span data-stu-id="f0252-127">Count Documents</span></span>](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[<span data-ttu-id="f0252-128">Javaslatok</span><span class="sxs-lookup"><span data-stu-id="f0252-128">Suggestions</span></span>](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

- - -
<a name="IndexOps"></a>

## <a name="index-operations"></a><span data-ttu-id="f0252-129">Indexművelet</span><span class="sxs-lookup"><span data-stu-id="f0252-129">Index Operations</span></span>
<span data-ttu-id="f0252-130">Hozzon létre, és az Azure Search szolgáltatás egyszerű HTTP-kérelmek (POST, GET, PUT, DELETE) egy adott indexű erőforrás elleni keresztül Indexek kezelése.</span><span class="sxs-lookup"><span data-stu-id="f0252-130">You can create and manage indexes in Azure Search service via simple HTTP requests (POST, GET, PUT, DELETE) against a given index resource.</span></span> <span data-ttu-id="f0252-131">Index létrehozása, akkor először ÍRJON egy JSON-dokumentumában, amely leírja a sémát indexeli.</span><span class="sxs-lookup"><span data-stu-id="f0252-131">To create an index, you first POST a JSON document that describes the index schema.</span></span> <span data-ttu-id="f0252-132">A séma definiálja az index, az adattípusokat és hogyan is használhatók (például a teljes szöveges keresést, szűrők, rendezés vagy értékkorlátozás).</span><span class="sxs-lookup"><span data-stu-id="f0252-132">The schema defines the fields of the index, their data types, and how they can be used (for example, in full-text searches, filters, sorting, or faceting).</span></span> <span data-ttu-id="f0252-133">A pontozási profil, javaslattevők és egyéb attribútumai konfigurálhatja a viselkedését az index is meghatároz.</span><span class="sxs-lookup"><span data-stu-id="f0252-133">It also defines scoring profiles, suggesters and other attributes to configure the behavior of the index.</span></span>

<span data-ttu-id="f0252-134">A következő példa illusztrálja a Leírás mezőben definiált két nyelvű Szálloda adatai a keresést használt séma.</span><span class="sxs-lookup"><span data-stu-id="f0252-134">The following example provides an illustration of a schema used for searching on hotel information with the Description field defined in two languages.</span></span> <span data-ttu-id="f0252-135">Figyelje meg, hogyan attribútumok szabályozza, hogyan használja a mezőt.</span><span class="sxs-lookup"><span data-stu-id="f0252-135">Notice how attributes control how the field is used.</span></span> <span data-ttu-id="f0252-136">Például a `hotelId` a dokumentum kulcsként (`"key": true`) és a teljes szöveges keresés nem tartozik (`"searchable": false`).</span><span class="sxs-lookup"><span data-stu-id="f0252-136">For example the `hotelId` is used as the document key (`"key": true`) and is excluded from full-text searches (`"searchable": false`).</span></span>

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

<span data-ttu-id="f0252-137">Az index létrehozása után lesz feltöltött dokumentumok, amely feltölti az indexet.</span><span class="sxs-lookup"><span data-stu-id="f0252-137">After the index is created, you'll upload documents that populate the index.</span></span> <span data-ttu-id="f0252-138">Lásd: [hozzáadása vagy frissítés dokumentumok](#AddOrUpdateDocuments) a következő lépéshez.</span><span class="sxs-lookup"><span data-stu-id="f0252-138">See [Add or Update Documents](#AddOrUpdateDocuments) for this next step.</span></span>

<span data-ttu-id="f0252-139">Videó az Azure Search az indexelő, tekintse meg a [epizód Channel 9 felhő fedik le az Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).</span><span class="sxs-lookup"><span data-stu-id="f0252-139">For a video introduction to indexing in Azure Search, see the [Channel 9 Cloud Cover episode on Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).</span></span>

<a name="CreateIndex"></a>

## <a name="create-index"></a><span data-ttu-id="f0252-140">Index létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0252-140">Create Index</span></span>
<span data-ttu-id="f0252-141">Az index az rendszerezése és dokumentumok keresése az Azure Search hasonló miként tábla rendezi egy adatbázis elsődleges eszköze.</span><span class="sxs-lookup"><span data-stu-id="f0252-141">An index is the primary means of organizing and searching documents in Azure Search, similar to how a table organizes records in a database.</span></span> <span data-ttu-id="f0252-142">Minden egyes index összes felelnek meg a sémát indexeli (mezőnevek, az adattípusokat és tulajdonságait), de indexek is adja meg a további szerkezetek (javaslattevők, pontozási profilokat és a CORS-beállítások), amely más keresési tulajdonságainak megadásához dokumentumok gyűjteményével rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f0252-142">Each index has a collection of documents that all conform to the index schema (field names, data types, and properties), but indexes also specify additional constructs (suggesters, scoring profiles, and CORS options) that define other search behaviors.</span></span>

<span data-ttu-id="f0252-143">Létrehozhat egy új index használ egy HTTP POST vagy a PUT kérelmek Azure Search szolgáltatás belül.</span><span class="sxs-lookup"><span data-stu-id="f0252-143">You can create a new index within an Azure Search service using an HTTP POST or PUT request.</span></span> <span data-ttu-id="f0252-144">A kérelem törzse egy JSON-séma, amely az index és konfigurációs információt.</span><span class="sxs-lookup"><span data-stu-id="f0252-144">The body of the request is a JSON schema that specifies the index and configuration information.</span></span>

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="f0252-145">Azt is megteheti PUT használja, és adja meg az URI az index nevét.</span><span class="sxs-lookup"><span data-stu-id="f0252-145">Alternatively, you can use PUT and specify the index name on the URI.</span></span> <span data-ttu-id="f0252-146">Ha az index nem létezik, a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="f0252-146">If the index does not exist, it will be created.</span></span>

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

<span data-ttu-id="f0252-147">Az index létrehozása meghatározza, hogy a struktúra a dokumentumok és használja a keresési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="f0252-147">Creating an index determines the structure of the documents stored and used in search operations.</span></span> <span data-ttu-id="f0252-148">Az index feltöltése egy olyan különálló művelet.</span><span class="sxs-lookup"><span data-stu-id="f0252-148">Populating the index is a separate operation.</span></span> <span data-ttu-id="f0252-149">Az ebben a lépésben használhatja egy [indexelő](https://msdn.microsoft.com/library/azure/mt183328.aspx) (támogatott adatforrások érhető el) vagy egy [hozzáadása, frissítése vagy törlése dokumentumok](https://msdn.microsoft.com/library/azure/dn798930.aspx) műveletet.</span><span class="sxs-lookup"><span data-stu-id="f0252-149">For this step, you can use an [indexer](https://msdn.microsoft.com/library/azure/mt183328.aspx) (available for supported data sources) or an [Add, Update, or Delete Documents](https://msdn.microsoft.com/library/azure/dn798930.aspx) operation.</span></span> <span data-ttu-id="f0252-150">A fordított index jön létre, amikor a dokumentumok van közzétéve.</span><span class="sxs-lookup"><span data-stu-id="f0252-150">The inverted index is generated when the documents are posted.</span></span>

<span data-ttu-id="f0252-151">**Megjegyzés:**: tarifacsomag függ a indexek engedélyezett maximális száma.</span><span class="sxs-lookup"><span data-stu-id="f0252-151">**Note**: The maximum number of indexes allowed varies by pricing tier.</span></span> <span data-ttu-id="f0252-152">Az ingyenes szolgáltatás lehetővé teszi, hogy a 3 indexeket.</span><span class="sxs-lookup"><span data-stu-id="f0252-152">The free service allows up to 3 indexes.</span></span> <span data-ttu-id="f0252-153">Standard szolgáltatás lehetővé teszi, hogy 50 indexek érhető el keresési szolgáltatásonként.</span><span class="sxs-lookup"><span data-stu-id="f0252-153">Standard service allows 50 indexes per Search service.</span></span> <span data-ttu-id="f0252-154">Lásd: [korlátozások és megkötések](http://msdn.microsoft.com/library/azure/dn798934.aspx) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="f0252-154">See [Limits and constraints](http://msdn.microsoft.com/library/azure/dn798934.aspx) for details.</span></span>

<span data-ttu-id="f0252-155">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="f0252-155">**Request**</span></span>

<span data-ttu-id="f0252-156">HTTPS-kapcsolat szükséges az összes szolgáltatási kérelmek.</span><span class="sxs-lookup"><span data-stu-id="f0252-156">HTTPS is required for all service requests.</span></span> <span data-ttu-id="f0252-157">A **a Create Index** kérelem egy POST vagy a PUT metódust használó lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f0252-157">The **Create Index** request can be constructed using either a POST or PUT method.</span></span> <span data-ttu-id="f0252-158">POST használatakor meg kell adnia egy index neve a kérés törzsében séma Indexdefiníció együtt.</span><span class="sxs-lookup"><span data-stu-id="f0252-158">When using POST, you must provide an index name in the request body along with the index schema definition.</span></span> <span data-ttu-id="f0252-159">A PUT érvénytelen az URL-cím.</span><span class="sxs-lookup"><span data-stu-id="f0252-159">With PUT, the index name is part of the URL.</span></span> <span data-ttu-id="f0252-160">Ha az index nem létezik, akkor jön létre.</span><span class="sxs-lookup"><span data-stu-id="f0252-160">If the index doesn't exist, it is created.</span></span> <span data-ttu-id="f0252-161">Ha már létezik, az új definition frissül.</span><span class="sxs-lookup"><span data-stu-id="f0252-161">If it already exists, it is updated to the new definition.</span></span>

<span data-ttu-id="f0252-162">Az index neve kell kell kisbetű, betűvel vagy számmal kezdődhet, nincs perjeleket vagy pontokból és legfeljebb 128 karakter lehet.</span><span class="sxs-lookup"><span data-stu-id="f0252-162">The index name must be lower case, start with a letter or number, have no slashes or dots, and be less than 128 characters.</span></span> <span data-ttu-id="f0252-163">Után az index nevének betűvel vagy számmal verziótól kezdődően a nevét a többi tartalmazhatnak bármely betű, szám és kötőjeleket, mindaddig, amíg a kötőjelek nincsenek egymást követő.</span><span class="sxs-lookup"><span data-stu-id="f0252-163">After starting the index name with a letter or number, the rest of the name can include any letter, number and dashes, as long as the dashes are not consecutive.</span></span>

<span data-ttu-id="f0252-164">A `api-version` szükséges.</span><span class="sxs-lookup"><span data-stu-id="f0252-164">The `api-version` is required.</span></span> <span data-ttu-id="f0252-165">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) elérhető verzió listáját.</span><span class="sxs-lookup"><span data-stu-id="f0252-165">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for a list of available versions.</span></span>

<span data-ttu-id="f0252-166">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="f0252-166">**Request Headers**</span></span>

<span data-ttu-id="f0252-167">Az alábbi lista ismerteti a szükséges és választható kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="f0252-167">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="f0252-168">`Content-Type`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f0252-168">`Content-Type`: Required.</span></span> <span data-ttu-id="f0252-169">Állítsa ezt a beállítást`application/json`</span><span class="sxs-lookup"><span data-stu-id="f0252-169">Set this to `application/json`</span></span>
* <span data-ttu-id="f0252-170">`api-key`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f0252-170">`api-key`: Required.</span></span> <span data-ttu-id="f0252-171">A `api-key` szolgál</span><span class="sxs-lookup"><span data-stu-id="f0252-171">The `api-key` is used to</span></span>
* <span data-ttu-id="f0252-172">a keresőszolgáltatása kérés hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f0252-172">authenticate the request to your Search service.</span></span> <span data-ttu-id="f0252-173">Egy karakterláncértéket, a szolgáltatás egyedi.</span><span class="sxs-lookup"><span data-stu-id="f0252-173">It is a string value, unique to your service.</span></span> <span data-ttu-id="f0252-174">A **a Create Index** kérelem tartalmaznia kell egy `api-key` fejlécben az adminisztrációs kulcsot (nem egy lekérdezési kulcsot).</span><span class="sxs-lookup"><span data-stu-id="f0252-174">The **Create Index** request must include an `api-key` header set to your admin key (as opposed to a query key).</span></span>

<span data-ttu-id="f0252-175">Konfigurálnia kell a szolgáltatás nevét, a kérelem URL-címe összeállításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-175">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="f0252-176">A szolgáltatás nevet is kaphat és `api-key` a szolgáltatás irányítópultján, az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f0252-176">You can get both the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="f0252-177">Lásd: [Azure Search szolgáltatás létrehozása a portálon](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="f0252-177">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="f0252-178"><a name="RequestData"></a>
**Törzs szintaxis**</span><span class="sxs-lookup"><span data-stu-id="f0252-178"><a name="RequestData"></a>
**Request Body Syntax**</span></span>

<span data-ttu-id="f0252-179">A kérelem törzse tartalmaz sémadefiníciót, amely tartalmazza az adatmezők belül, amely ezt az indexet, adattípusok, attribútumok, valamint megfelelő dokumentumok pontozása lekérdezések során használt profilok pontozás szünnap fog kell táplált dokumentumok listáját .</span><span class="sxs-lookup"><span data-stu-id="f0252-179">The body of the request contains a schema definition, which includes the list of data fields within documents that will be fed into this index, data types, attributes, as well as an optional list of scoring profiles that are used to score matching documents at query time.</span></span>

<span data-ttu-id="f0252-180">Vegye figyelembe, hogy az egy POST kérést, meg kell adnia az index neve a kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="f0252-180">Note that for a POST request, you must specify the index name in the request body.</span></span>

<span data-ttu-id="f0252-181">Csak lehet egy kulcsmezőt az index.</span><span class="sxs-lookup"><span data-stu-id="f0252-181">There can only be one key field in the index.</span></span> <span data-ttu-id="f0252-182">Kell lennie a mezőnek van.</span><span class="sxs-lookup"><span data-stu-id="f0252-182">It has to be a string field.</span></span> <span data-ttu-id="f0252-183">Ez a mező képviseli az tárolja az index egyes dokumentumok egyedi azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="f0252-183">This field represents the unique identifier for each document stored within the index.</span></span>

<span data-ttu-id="f0252-184">Az index fő részeket közé tartoznak a következők:</span><span class="sxs-lookup"><span data-stu-id="f0252-184">The main parts of an index include the following:</span></span>

* `name`
* <span data-ttu-id="f0252-185">`fields`amely az indexbe, például név, adattípus és ezt a mezőt az engedélyezett műveletek meghatározó tulajdonságok biztosítása táplált lesz.</span><span class="sxs-lookup"><span data-stu-id="f0252-185">`fields` that will be fed into this index, including name, data type, and properties that define allowable actions on that field.</span></span>
* <span data-ttu-id="f0252-186">`suggesters`Automatikus kitöltés vagy begépelt lekérdezések használatos.</span><span class="sxs-lookup"><span data-stu-id="f0252-186">`suggesters` used for auto-complete or type-ahead queries.</span></span>
* <span data-ttu-id="f0252-187">`scoringProfiles`Egyéni keresési pontszám prioritás használatos.</span><span class="sxs-lookup"><span data-stu-id="f0252-187">`scoringProfiles` used for custom search score ranking.</span></span> <span data-ttu-id="f0252-188">Lásd: [pontozási profilok hozzáadása](https://msdn.microsoft.com/library/azure/dn798928.aspx) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="f0252-188">See [Add scoring profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for details.</span></span>
* <span data-ttu-id="f0252-189">`analyzers`, `charFilters`, `tokenizers`, `tokenFilters` meghatározásához a dokumentumok lekérdezések példánycímkének/kereshető jogkivonatokba bontottuk hogyan használják.</span><span class="sxs-lookup"><span data-stu-id="f0252-189">`analyzers`, `charFilters`, `tokenizers`, `tokenFilters` used to define how your documents/queries are broken into indexable/searchable tokens.</span></span> <span data-ttu-id="f0252-190">Lásd: [elemzése az Azure Search](https://aka.ms//azsanalysis) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="f0252-190">See [Analysis in Azure Search](https://aka.ms//azsanalysis) for details.</span></span>
* <span data-ttu-id="f0252-191">`defaultScoringProfile`használja az alapértelmezett viselkedés pontozási felülírásához.</span><span class="sxs-lookup"><span data-stu-id="f0252-191">`defaultScoringProfile` used to overwrite the default scoring behaviors.</span></span>
* <span data-ttu-id="f0252-192">`corsOptions`engedélyezi eltérő eredetű lekérdezések írásában, az index.</span><span class="sxs-lookup"><span data-stu-id="f0252-192">`corsOptions` to allow cross-origin queries against your index.</span></span>

<span data-ttu-id="f0252-193">A szerkezetének kialakítása a kérelem hasznos szintaxisa a következő.</span><span class="sxs-lookup"><span data-stu-id="f0252-193">The syntax for structuring the request payload is as follows.</span></span> <span data-ttu-id="f0252-194">Egy minta kérelem biztosítja az ebben a témakörben további.</span><span class="sxs-lookup"><span data-stu-id="f0252-194">A sample request is provided further on in this topic.</span></span>

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
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
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
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
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

<span data-ttu-id="f0252-195">**Indexattribútumok**</span><span class="sxs-lookup"><span data-stu-id="f0252-195">**Index Attributes**</span></span>

<span data-ttu-id="f0252-196">A következő attribútumok egy index létrehozásakor állítható be.</span><span class="sxs-lookup"><span data-stu-id="f0252-196">The following attributes can be set when creating an index.</span></span> <span data-ttu-id="f0252-197">További információk a pontozási és profilok pontozási: [hozzáadása a pontozási profil](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0252-197">For details about scoring and scoring profiles, see [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span></span>

<span data-ttu-id="f0252-198">`name`-Beállítja a mező nevét.</span><span class="sxs-lookup"><span data-stu-id="f0252-198">`name` - Sets the name of the field.</span></span>

<span data-ttu-id="f0252-199">`type`– Beállítja a mező adattípusát.</span><span class="sxs-lookup"><span data-stu-id="f0252-199">`type` - Sets the data type for the field.</span></span>

<span data-ttu-id="f0252-200">`searchable`-Állapotúként jelöli meg a mező teljes szöveges keresés képes.</span><span class="sxs-lookup"><span data-stu-id="f0252-200">`searchable` - Marks the field as full-text search-able.</span></span> <span data-ttu-id="f0252-201">Ez azt jelenti, hogy az elemzés során indexelő szóhatároló például is változni fog.</span><span class="sxs-lookup"><span data-stu-id="f0252-201">This means it will undergo analysis such as word-breaking during indexing.</span></span> <span data-ttu-id="f0252-202">Ha egy `searchable` mező "napfényes", belső, például értékre lesznek osztva az egyes jogkivonatokba "moziba" és "day".</span><span class="sxs-lookup"><span data-stu-id="f0252-202">If you set a `searchable` field to a value like "sunny day", internally it will be split into the individual tokens "sunny" and "day".</span></span> <span data-ttu-id="f0252-203">Ez lehetővé teszi, hogy ezek a feltételek a teljes szöveges keresés.</span><span class="sxs-lookup"><span data-stu-id="f0252-203">This enables full-text searches for these terms.</span></span> <span data-ttu-id="f0252-204">Típusú mezők `Edm.String` vagy `Collection(Edm.String)` vannak `searchable` alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="f0252-204">Fields of type `Edm.String` or `Collection(Edm.String)` are `searchable` by default.</span></span> <span data-ttu-id="f0252-205">Nem lehet a más típusú mezők `searchable`.</span><span class="sxs-lookup"><span data-stu-id="f0252-205">Fields of other types cannot be `searchable`.</span></span>

* <span data-ttu-id="f0252-206">**Megjegyzés:**: `searchable` mezők az indexben területnek felhasználását, mivel az Azure Search fogja tárolni egy további tokenekre verzióját a mező értékét a teljes szöveges keresést.</span><span class="sxs-lookup"><span data-stu-id="f0252-206">**Note**: `searchable` fields consume extra space in your index since Azure Search will store an additional tokenized version of the field value for full-text searches.</span></span> <span data-ttu-id="f0252-207">Menti a terület az indexben, és nem kell szerepeltetni keresések mező, akkor `searchable` való `false`.</span><span class="sxs-lookup"><span data-stu-id="f0252-207">If you want to save space in your index and you don't need a field to be included in searches, set `searchable` to `false`.</span></span>

<span data-ttu-id="f0252-208">`filterable`-Lehetővé teszi a mutató hivatkozás fog megjelenni a mező `$filter` lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="f0252-208">`filterable` - Allows the field to be referenced in `$filter` queries.</span></span> <span data-ttu-id="f0252-209">`filterable`eltér az `searchable` a karakterláncok kezelésének módját.</span><span class="sxs-lookup"><span data-stu-id="f0252-209">`filterable` differs from `searchable` in how strings are handled.</span></span> <span data-ttu-id="f0252-210">Típusú mezők `Edm.String` vagy `Collection(Edm.String)` , amelyek `filterable` nem kerülnek szóhatároló, így csak pontosan megegyezik az összehasonlítást.</span><span class="sxs-lookup"><span data-stu-id="f0252-210">Fields of type `Edm.String` or `Collection(Edm.String)` that are `filterable` do not undergo word-breaking, so comparisons are for exact matches only.</span></span> <span data-ttu-id="f0252-211">Például, ha a mező `f` "moziba Day", `$filter=f eq 'sunny'` megkeresi, nincs találat, de `$filter=f eq 'sunny day'` lesz.</span><span class="sxs-lookup"><span data-stu-id="f0252-211">For example, if you set such a field `f` to "sunny day", `$filter=f eq 'sunny'` will find no matches, but `$filter=f eq 'sunny day'` will.</span></span> <span data-ttu-id="f0252-212">A mezők egyike sem `filterable` alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="f0252-212">All fields are `filterable` by default.</span></span>

<span data-ttu-id="f0252-213">`sortable`-Alapértelmezés szerint a rendszer rendezése az eredmények pontszám, de sok elhatározásuk felhasználók érdemes a dokumentumok mezők szerint rendezhető.</span><span class="sxs-lookup"><span data-stu-id="f0252-213">`sortable` - By default the system sorts results by score, but in many experiences users will want to sort by fields in the documents.</span></span> <span data-ttu-id="f0252-214">Típusú mezők `Collection(Edm.String)` nem lehet `sortable`.</span><span class="sxs-lookup"><span data-stu-id="f0252-214">Fields of type `Collection(Edm.String)` cannot be `sortable`.</span></span> <span data-ttu-id="f0252-215">A többi mező kitöltése `sortable` alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="f0252-215">All other fields are `sortable` by default.</span></span>

<span data-ttu-id="f0252-216">`facetable`-A keresési eredmények találati száma (a példában, keresse meg a digitális fényképezőgépek, és tekintse meg a találatok márka, Megapixel, ár, stb.) kategória szerint tartalmazó bemutató jellemzően használt.</span><span class="sxs-lookup"><span data-stu-id="f0252-216">`facetable`- Typically used in a presentation of search results that includes hit count by category (for example, search for digital cameras and see hits by brand, by megapixels, by price, etc.).</span></span> <span data-ttu-id="f0252-217">Ez a beállítás nem használható típusú mezők `Edm.GeographyPoint`.</span><span class="sxs-lookup"><span data-stu-id="f0252-217">This option cannot be used with fields of type `Edm.GeographyPoint`.</span></span> <span data-ttu-id="f0252-218">A többi mező kitöltése `facetable` alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="f0252-218">All other fields are `facetable` by default.</span></span>

* <span data-ttu-id="f0252-219">**Megjegyzés:**: típusú mezők `Edm.String` , amelyek `filterable`, `sortable`, vagy `facetable` legfeljebb 32 KB-os hossza.</span><span class="sxs-lookup"><span data-stu-id="f0252-219">**Note**: Fields of type `Edm.String` that are `filterable`, `sortable`, or `facetable` can be at most 32KB in length.</span></span> <span data-ttu-id="f0252-220">Ennek az az oka egy egyetlen keresési kifejezés számít ilyen mezők, és az Azure Search a kifejezés hossza legfeljebb 32 KB lehet.</span><span class="sxs-lookup"><span data-stu-id="f0252-220">This is because such fields are treated as a single search term, and the maximum length of a term in Azure Search is 32KB.</span></span> <span data-ttu-id="f0252-221">Ha a szöveg hosszabb, mint a tárolása egy mezőnek van szüksége, szüksége lesz explicit módon meghatározhatja az `filterable`, `sortable`, és `facetable` való `false` az index definícióját a.</span><span class="sxs-lookup"><span data-stu-id="f0252-221">If you need to store more text than this in a single string field, you will need to explicitly set `filterable`, `sortable`, and `facetable` to `false` in your index definition.</span></span>
* <span data-ttu-id="f0252-222">**Megjegyzés:**: a mező rendelkezik-e beállítva a fenti attribútumok egyike `true` (`searchable`, `filterable`, `sortable`, vagy`facetable`) mező hatékonyan ki van zárva a fordított indexből.</span><span class="sxs-lookup"><span data-stu-id="f0252-222">**Note**: If a field has none of the above attributes set to `true` (`searchable`, `filterable`, `sortable`,  or`facetable`) the field is effectively excluded from the inverted index.</span></span> <span data-ttu-id="f0252-223">Ez akkor hasznos, amelyek nem szerepelnek a lekérdezések, amelyekre szükség van a találatok között mezők.</span><span class="sxs-lookup"><span data-stu-id="f0252-223">This option is useful for fields that are not used in queries, but are needed in search results.</span></span> <span data-ttu-id="f0252-224">Ilyen mezők kivételével az indexből javítja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="f0252-224">Excluding such fields from the index improves performance.</span></span>

<span data-ttu-id="f0252-225">`key`-A-dokumentumokat tárolhat a index egyedi azonosítót tartalmazó mezőt jelöli meg.</span><span class="sxs-lookup"><span data-stu-id="f0252-225">`key` - Marks the field as containing unique identifiers for documents within the index.</span></span> <span data-ttu-id="f0252-226">Pontosan egy mezőt ki kell választani a `key` típusúnak kell lennie, és a mező `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="f0252-226">Exactly one field must be chosen as the `key` field and it must be of type `Edm.String`.</span></span> <span data-ttu-id="f0252-227">Kulcsmezők segítségével kereshet a dokumentumok keresztül közvetlenül a [keresési API](#LookupAPI).</span><span class="sxs-lookup"><span data-stu-id="f0252-227">Key fields can be used to look up documents directly via the [Lookup API](#LookupAPI).</span></span>

<span data-ttu-id="f0252-228">`retrievable`-Állítja be, hogy a mező a keresési eredmény adhatók vissza.</span><span class="sxs-lookup"><span data-stu-id="f0252-228">`retrievable` - Sets whether the field can be returned in a search result.</span></span>  <span data-ttu-id="f0252-229">Ez akkor hasznos, ha szeretné (például margó) mező használható szűrőként, rendezés, vagy pontozási mechanizmus, de nem szeretné, hogy a végfelhasználók számára megjelenített a mező.</span><span class="sxs-lookup"><span data-stu-id="f0252-229">This is useful when you want to use a field (for example, margin) as a filter, sorting, or scoring mechanism but do not want the field to be visible to the end user.</span></span> <span data-ttu-id="f0252-230">Ennek az attribútumnak kell lennie `true` a `key` mezőket.</span><span class="sxs-lookup"><span data-stu-id="f0252-230">This attribute must be `true` for `key` fields.</span></span>

<span data-ttu-id="f0252-231">`analyzer`-Beállítja a analyzer keresési és indexelési idő használatának a mező nevét.</span><span class="sxs-lookup"><span data-stu-id="f0252-231">`analyzer` - Sets the name of the analyzer to use for the field at search time and indexing time.</span></span> <span data-ttu-id="f0252-232">Tekintse meg a megengedett értékhalmazt [elemzőkkel](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0252-232">For the allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="f0252-233">Ez a beállítás használható csak `searchable` mezőket, és nem állítható be, vagy együtt `searchAnalyzer` vagy `indexAnalyzer`.</span><span class="sxs-lookup"><span data-stu-id="f0252-233">This option can be used only with `searchable` fields and it can't be set together with either `searchAnalyzer` or `indexAnalyzer`.</span></span>  <span data-ttu-id="f0252-234">Miután a analyzer választja, azt a mező nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="f0252-234">Once the analyzer is chosen, it cannot be changed for the field.</span></span>

<span data-ttu-id="f0252-235">`searchAnalyzer`– Beállítja a mező a keresés közben használatos analyzer nevét.</span><span class="sxs-lookup"><span data-stu-id="f0252-235">`searchAnalyzer` - Sets the name of the analyzer used at search time for the field.</span></span> <span data-ttu-id="f0252-236">Tekintse meg a megengedett értékhalmazt [elemzőkkel](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0252-236">For the allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="f0252-237">Ez a beállítás használható csak `searchable` mezőket.</span><span class="sxs-lookup"><span data-stu-id="f0252-237">This option can be used only with `searchable` fields.</span></span> <span data-ttu-id="f0252-238">Együtt kell beállítani `indexAnalyzer` és együtt nem állítható be a `analyzer` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f0252-238">It must be set together with `indexAnalyzer` and it cannot be set together with the `analyzer` option.</span></span> <span data-ttu-id="f0252-239">Az elemző meglévő mezőre lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="f0252-239">This analyzer can be updated on an existing field.</span></span>

<span data-ttu-id="f0252-240">`indexAnalyzer`– Beállítja a mező az indexelő közben használatos analyzer nevét.</span><span class="sxs-lookup"><span data-stu-id="f0252-240">`indexAnalyzer` - Sets the name of the analyzer used at indexing time for the field.</span></span> <span data-ttu-id="f0252-241">Tekintse meg a megengedett értékhalmazt [elemzőkkel](https://msdn.microsoft.com/library/mt605304.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0252-241">For the allowed set of values see [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx).</span></span> <span data-ttu-id="f0252-242">Ez a beállítás használható csak `searchable` mezőket.</span><span class="sxs-lookup"><span data-stu-id="f0252-242">This option can be used only with `searchable` fields.</span></span> <span data-ttu-id="f0252-243">Együtt kell beállítani `searchAnalyzer` és együtt nem állítható be a `analyzer` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f0252-243">It must be set together with `searchAnalyzer` and it cannot be set together with the `analyzer` option.</span></span> <span data-ttu-id="f0252-244">Miután a analyzer választja, azt a mező nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="f0252-244">Once the analyzer is chosen, it cannot be changed for the field.</span></span>

<span data-ttu-id="f0252-245">`suggesters`-A keresési mód és a javaslatok a tartalom forrásának mezők beállítása.</span><span class="sxs-lookup"><span data-stu-id="f0252-245">`suggesters` - Sets the search mode and fields that are the source of the content for suggestions.</span></span> <span data-ttu-id="f0252-246">Lásd: [Javaslattevők](#Suggesters) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="f0252-246">See [Suggesters](#Suggesters) for details.</span></span>

<span data-ttu-id="f0252-247">`scoringProfiles`-Határozza meg az egyéni pontozási viselkedéseket, amelyek lehetővé teszik, hogy befolyásolják mely elemek jelennek meg a keresési eredmények között magasabb.</span><span class="sxs-lookup"><span data-stu-id="f0252-247">`scoringProfiles` - Defines custom scoring behaviors that let you influence which items appear higher in search results.</span></span> <span data-ttu-id="f0252-248">A pontozási profil mező súlyok és funkciók épülnek fel.</span><span class="sxs-lookup"><span data-stu-id="f0252-248">Scoring profiles are made up of field weights and functions.</span></span> <span data-ttu-id="f0252-249">Lásd: [hozzáadása a pontozási profil](https://msdn.microsoft.com/library/azure/dn798928.aspx) további információt a pontozási profil használt attribútumoknál.</span><span class="sxs-lookup"><span data-stu-id="f0252-249">See [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for more information about the attributes used in a scoring profile.</span></span>

<span data-ttu-id="f0252-250"><!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Nyelvi támogatás**</span><span class="sxs-lookup"><span data-stu-id="f0252-250"><!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Language support**</span></span>

<span data-ttu-id="f0252-251">Kereshető mezők változni elemzés, hogy a legtöbb gyakran magában foglalja a szóhatároló, szöveges normalizálási és szűri ki a feltételeket.</span><span class="sxs-lookup"><span data-stu-id="f0252-251">Searchable fields undergo analysis that most frequently involves word-breaking, text normalization, and filtering out terms.</span></span> <span data-ttu-id="f0252-252">Alapértelmezés szerint az Azure Search kereshető mezőt és elemzése a [Apache Lucene szabványos analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) szöveg amely bontja a következő elemek a["Unicode szöveg Szegmentálás"](http://unicode.org/reports/tr29/) szabályok.</span><span class="sxs-lookup"><span data-stu-id="f0252-252">By default, searchable fields in Azure Search are analyzed with the [Apache Lucene Standard analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) which breaks text into elements following the["Unicode Text Segmentation"](http://unicode.org/reports/tr29/) rules.</span></span> <span data-ttu-id="f0252-253">Emellett a szabványos analyzer kisbetű formájukban alakítja az összes karaktert.</span><span class="sxs-lookup"><span data-stu-id="f0252-253">Additionally, the standard analyzer converts all characters to their lower case form.</span></span> <span data-ttu-id="f0252-254">Indexelt dokumentumok és a keresési feltételek halad át az elemzés indexelő és a lekérdezés feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="f0252-254">Both indexed documents and search terms go through the analysis during indexing and query processing.</span></span>

<span data-ttu-id="f0252-255">Az Azure Search támogatja a különböző nyelveken.</span><span class="sxs-lookup"><span data-stu-id="f0252-255">Azure Search supports a variety of languages.</span></span> <span data-ttu-id="f0252-256">Minden egyes nyelvi egy nem szabványos szöveg analyzer, amelyek jellemzői egy adott nyelvhez tartozó fiókok igényel.</span><span class="sxs-lookup"><span data-stu-id="f0252-256">Each language requires a non-standard text analyzer which accounts for characteristics of a given language.</span></span> <span data-ttu-id="f0252-257">Az Azure Search kétféle típusú elemzőkkel:</span><span class="sxs-lookup"><span data-stu-id="f0252-257">Azure Search offers two types of analyzers:</span></span>

* <span data-ttu-id="f0252-258">35 elemzőkkel Lucene által támogatott.</span><span class="sxs-lookup"><span data-stu-id="f0252-258">35 analyzers backed by Lucene.</span></span>
* <span data-ttu-id="f0252-259">50 elemzőkkel által védett Microsoft természetes nyelvű Office-és Bing használt technológia feldolgozása támogatott.</span><span class="sxs-lookup"><span data-stu-id="f0252-259">50 analyzers backed by proprietary Microsoft natural language processing technology used in Office and Bing.</span></span>

<span data-ttu-id="f0252-260">Néhány fejlesztők Lucene több ismerős, egyszerű, nyílt forráskódú megoldás lehet, hogy inkább.</span><span class="sxs-lookup"><span data-stu-id="f0252-260">Some developers might prefer the more familiar, simple, open-source solution of Lucene.</span></span> <span data-ttu-id="f0252-261">Lucene elemzőkkel gyorsabb, de a Microsoft elemzőkkel képességeit, például Lemmatizálás, a word decompounding (például német, dán, holland, svéd, norvég, észt, Befejezés, magyar, Szlovák nyelven) és az entitás felismerési (URL-címek, speciális e-mailek, dátumok, számok).</span><span class="sxs-lookup"><span data-stu-id="f0252-261">Lucene analyzers are faster, but the Microsoft analyzers have advanced capabilities, such as lemmatization, word decompounding (in languages like German, Danish, Dutch, Swedish, Norwegian, Estonian, Finish, Hungarian, Slovak) and entity recognition (URLs, emails, dates, numbers).</span></span> <span data-ttu-id="f0252-262">Ha lehetséges futtassa a Microsoft és a Lucene elemzőkkel eldönteni, melyik felel meg leginkább az összehasonlítása.</span><span class="sxs-lookup"><span data-stu-id="f0252-262">If possible, you should run comparisons of both the Microsoft and Lucene analyzers to decide which one is a better fit.</span></span>

<span data-ttu-id="f0252-263">***Annak összevetése***</span><span class="sxs-lookup"><span data-stu-id="f0252-263">***How they compare***</span></span>

<span data-ttu-id="f0252-264">Az angol nyelvű tájékoztatáshoz a Lucene analyzer bővíti a szabványos analyzer.</span><span class="sxs-lookup"><span data-stu-id="f0252-264">The Lucene analyzer for English extends the standard analyzer.</span></span> <span data-ttu-id="f0252-265">Az (záró tartozó) possessives eltávolítja a szavakat, vonatkozik, amelyek megfelelően [Porter származó algoritmus](http://tartarus.org/~martin/PorterStemmer/), és eltávolítja az angol [szavak leállítása](http://en.wikipedia.org/wiki/Stop_words).</span><span class="sxs-lookup"><span data-stu-id="f0252-265">It removes possessives (trailing 's) from words, applies stemming as per [Porter Stemming algorithm](http://tartarus.org/~martin/PorterStemmer/), and removes English [stop words](http://en.wikipedia.org/wiki/Stop_words).</span></span>

<span data-ttu-id="f0252-266">Összehasonlításképpen a Microsoft analyzer helyett származó Lemmatizálás hajt végre.</span><span class="sxs-lookup"><span data-stu-id="f0252-266">In comparison, the Microsoft analyzer performs lemmatization instead of stemming.</span></span> <span data-ttu-id="f0252-267">Az azt jelenti, hogy is képes kezelni ragozott és szabálytalan word űrlapok javulás mi több megfelelő találatok eredményez (7 modulja figyelési [Azure keresési MVA bemutató](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) további részletekért).</span><span class="sxs-lookup"><span data-stu-id="f0252-267">It means it can handle inflected and irregular word forms much better what results in more relevant search results (watch module 7 of [Azure Search MVA presentation](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) for more details).</span></span>

<span data-ttu-id="f0252-268">A Microsoft elemzőkkel indexelő van átlagos Lucene megfelelő, attól függően, hogy a nyelvi lassabb a két-három alkalommal.</span><span class="sxs-lookup"><span data-stu-id="f0252-268">Indexing with Microsoft analyzers is on average two to three times slower than their Lucene equivalents, depending on the language.</span></span> <span data-ttu-id="f0252-269">Keresés teljesítménye nem átlagos mérete lekérdezések jelentős hatással lehet.</span><span class="sxs-lookup"><span data-stu-id="f0252-269">Search performance should not be significantly affected for average size queries.</span></span>

<span data-ttu-id="f0252-270">***Konfigurálás***</span><span class="sxs-lookup"><span data-stu-id="f0252-270">***Configuration***</span></span>

<span data-ttu-id="f0252-271">Az egyes mezők az index definícióját, beállíthatja a `analyzer` tulajdonság, amely meghatározza, mely nyelvi és szállítói analyzer névre.</span><span class="sxs-lookup"><span data-stu-id="f0252-271">For each field in the index definition, you can set the `analyzer` property to an analyzer name that specifies which language and vendor.</span></span> <span data-ttu-id="f0252-272">Az azonos analyzer fogja alkalmazni, amikor az indexelő, és ezt a mezőt keresésekor.</span><span class="sxs-lookup"><span data-stu-id="f0252-272">The same analyzer will be applied when indexing and searching for that field.</span></span>
<span data-ttu-id="f0252-273">Lehet például az angol, francia és spanyol-párhuzamos ugyanazt az indexet a létező Szálloda leírások külön mezők.</span><span class="sxs-lookup"><span data-stu-id="f0252-273">For example, you can have separate fields for English, French, and Spanish hotel descriptions that exist side-by-side in the same index.</span></span> <span data-ttu-id="f0252-274">Használja a ["searchFields" lekérdezésparaméter](#SearchQueryParameters) adhatja meg, melyik nyelvspecifikus mezővel rákereshet szemben a lekérdezésekben.</span><span class="sxs-lookup"><span data-stu-id="f0252-274">Use the ['searchFields' query parameter](#SearchQueryParameters) to specify which language-specific field to search against in your queries.</span></span> <span data-ttu-id="f0252-275">Áttekintheti a lekérdezés példák, amelyek tartalmazzák a `analyzer` tulajdonság [dokumentumok keresése](#SearchDocs).</span><span class="sxs-lookup"><span data-stu-id="f0252-275">You can review query examples that include the `analyzer` property in [Search Documents](#SearchDocs).</span></span> 

<span data-ttu-id="f0252-276">***Analyzer listája***</span><span class="sxs-lookup"><span data-stu-id="f0252-276">***Analyzer list***</span></span>

<span data-ttu-id="f0252-277">Az alábbiakban van együtt Lucene és a Microsoft analyzer nevek támogatott nyelvek listáját.</span><span class="sxs-lookup"><span data-stu-id="f0252-277">Below is the list of supported languages together with Lucene and Microsoft analyzer names.</span></span>

<table style="font-size:12">
    <tr>
        <th><span data-ttu-id="f0252-278">Nyelv</span><span class="sxs-lookup"><span data-stu-id="f0252-278">Language</span></span></th>
        <th><span data-ttu-id="f0252-279">Microsoft-elemző eszköz neve</span><span class="sxs-lookup"><span data-stu-id="f0252-279">Microsoft analyzer name</span></span></th>
        <th><span data-ttu-id="f0252-280">Lucene analyzer neve</span><span class="sxs-lookup"><span data-stu-id="f0252-280">Lucene analyzer name</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-281">arab</span><span class="sxs-lookup"><span data-stu-id="f0252-281">Arabic</span></span></td>
        <td><span data-ttu-id="f0252-282">ar.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-282">ar.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-283">ar.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-283">ar.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-284">örmény</span><span class="sxs-lookup"><span data-stu-id="f0252-284">Armenian</span></span></td>
        <td></td>
        <td><span data-ttu-id="f0252-285">HY.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-285">hy.lucene</span></span></td>
      </tr>
    <tr>
        <td><span data-ttu-id="f0252-286">Bangla</span><span class="sxs-lookup"><span data-stu-id="f0252-286">Bangla</span></span></td>
        <td><span data-ttu-id="f0252-287">BN.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-287">bn.microsoft</span></span></td>
        <td></td>
    </tr>
      <tr>
        <td><span data-ttu-id="f0252-288">baszk</span><span class="sxs-lookup"><span data-stu-id="f0252-288">Basque</span></span></td>
        <td></td>
        <td><span data-ttu-id="f0252-289">EU.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-289">eu.lucene</span></span></td>
    </tr>
      <tr>
         <td><span data-ttu-id="f0252-290">bolgár</span><span class="sxs-lookup"><span data-stu-id="f0252-290">Bulgarian</span></span></td>
        <td><span data-ttu-id="f0252-291">BG.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-291">bg.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-292">BG.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-292">bg.lucene</span></span></td>
      </tr>
      <tr>
        <td><span data-ttu-id="f0252-293">katalán</span><span class="sxs-lookup"><span data-stu-id="f0252-293">Catalan</span></span></td>
        <td><span data-ttu-id="f0252-294">CA.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-294">ca.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-295">CA.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-295">ca.lucene</span></span></td>          
      </tr>
    <tr>
        <td><span data-ttu-id="f0252-296">kínai (egyszerűsített)</span><span class="sxs-lookup"><span data-stu-id="f0252-296">Chinese Simplified</span></span></td>
        <td><span data-ttu-id="f0252-297">zh-Hans.microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-297">zh-Hans.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-298">zh-Hans.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-298">zh-Hans.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-299">kínai (hagyományos)</span><span class="sxs-lookup"><span data-stu-id="f0252-299">Chinese Traditional</span></span></td>
        <td><span data-ttu-id="f0252-300">zh-Hant.microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-300">zh-Hant.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-301">zh-Hant.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-301">zh-Hant.lucene</span></span></td>        
    <tr>
    <tr>
        <td><span data-ttu-id="f0252-302">horvát</span><span class="sxs-lookup"><span data-stu-id="f0252-302">Croatian</span></span></td>
        <td><span data-ttu-id="f0252-303">hr.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-303">hr.microsoft</span></span></td>
        <td/></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-304">cseh</span><span class="sxs-lookup"><span data-stu-id="f0252-304">Czech</span></span></td>
        <td><span data-ttu-id="f0252-305">cs.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-305">cs.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-306">cs.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-306">cs.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="f0252-307">dán</span><span class="sxs-lookup"><span data-stu-id="f0252-307">Danish</span></span></td>
        <td><span data-ttu-id="f0252-308">da.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-308">da.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-309">da.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-309">da.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="f0252-310">holland</span><span class="sxs-lookup"><span data-stu-id="f0252-310">Dutch</span></span></td>
        <td><span data-ttu-id="f0252-311">NL.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-311">nl.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-312">NL.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-312">nl.lucene</span></span></td>    
    </tr>    
    <tr>
        <td><span data-ttu-id="f0252-313">Angol</span><span class="sxs-lookup"><span data-stu-id="f0252-313">English</span></span></td>        
        <td><span data-ttu-id="f0252-314">en.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-314">en.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-315">en.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-315">en.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-316">észt</span><span class="sxs-lookup"><span data-stu-id="f0252-316">Estonian</span></span></td>
        <td><span data-ttu-id="f0252-317">et.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-317">et.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-318">finn</span><span class="sxs-lookup"><span data-stu-id="f0252-318">Finnish</span></span></td>
        <td><span data-ttu-id="f0252-319">Fi.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-319">fi.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-320">Fi.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-320">fi.lucene</span></span></td>        
    </tr>    
    <tr>
        <td><span data-ttu-id="f0252-321">francia</span><span class="sxs-lookup"><span data-stu-id="f0252-321">French</span></span></td>
        <td><span data-ttu-id="f0252-322">FR.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-322">fr.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-323">FR.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-323">fr.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-324">galíciai</span><span class="sxs-lookup"><span data-stu-id="f0252-324">Galician</span></span></td>
        <td></td>
        <td><span data-ttu-id="f0252-325">GL.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-325">gl.lucene</span></span></td>        
      </tr>
    <tr>
        <td><span data-ttu-id="f0252-326">német</span><span class="sxs-lookup"><span data-stu-id="f0252-326">German</span></span></td>
        <td><span data-ttu-id="f0252-327">de.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-327">de.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-328">de.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-328">de.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-329">görög</span><span class="sxs-lookup"><span data-stu-id="f0252-329">Greek</span></span></td>
        <td><span data-ttu-id="f0252-330">el.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-330">el.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-331">el.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-331">el.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-332">gudzsaráti</span><span class="sxs-lookup"><span data-stu-id="f0252-332">Gujarati</span></span></td>
        <td><span data-ttu-id="f0252-333">Gu.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-333">gu.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-334">héber</span><span class="sxs-lookup"><span data-stu-id="f0252-334">Hebrew</span></span></td>
        <td><span data-ttu-id="f0252-335">He.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-335">he.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-336">hindi</span><span class="sxs-lookup"><span data-stu-id="f0252-336">Hindi</span></span></td>
        <td><span data-ttu-id="f0252-337">Hi.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-337">hi.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-338">Hi.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-338">hi.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-339">magyar</span><span class="sxs-lookup"><span data-stu-id="f0252-339">Hungarian</span></span></td>        
        <td><span data-ttu-id="f0252-340">hu.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-340">hu.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-341">hu.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-341">hu.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-342">izlandi</span><span class="sxs-lookup"><span data-stu-id="f0252-342">Icelandic</span></span></td>
        <td><span data-ttu-id="f0252-343">is.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-343">is.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-344">Indonéziai (Bahasa)</span><span class="sxs-lookup"><span data-stu-id="f0252-344">Indonesian (Bahasa)</span></span></td>
        <td><span data-ttu-id="f0252-345">ID.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-345">id.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-346">ID.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-346">id.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-347">ír</span><span class="sxs-lookup"><span data-stu-id="f0252-347">Irish</span></span></td>
        <td></td>
          <td><span data-ttu-id="f0252-348">GA.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-348">ga.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-349">olasz</span><span class="sxs-lookup"><span data-stu-id="f0252-349">Italian</span></span></td>
        <td><span data-ttu-id="f0252-350">IT.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-350">it.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-351">IT.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-351">it.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-352">japán</span><span class="sxs-lookup"><span data-stu-id="f0252-352">Japanese</span></span></td>
        <td><span data-ttu-id="f0252-353">ja.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-353">ja.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-354">ja.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-354">ja.lucene</span></span></td>

    </tr>
    <tr>
        <td><span data-ttu-id="f0252-355">kannada</span><span class="sxs-lookup"><span data-stu-id="f0252-355">Kannada</span></span></td>
        <td><span data-ttu-id="f0252-356">ka.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-356">ka.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-357">koreai</span><span class="sxs-lookup"><span data-stu-id="f0252-357">Korean</span></span></td>
        <td><span data-ttu-id="f0252-358">ko.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-358">ko.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-359">ko.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-359">ko.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-360">lett</span><span class="sxs-lookup"><span data-stu-id="f0252-360">Latvian</span></span></td>        
        <td><span data-ttu-id="f0252-361">LV.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-361">lv.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-362">LV.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-362">lv.lucene</span></span></td>    
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-363">litván</span><span class="sxs-lookup"><span data-stu-id="f0252-363">Lithuanian</span></span></td>
        <td><span data-ttu-id="f0252-364">lt.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-364">lt.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-365">malajálam</span><span class="sxs-lookup"><span data-stu-id="f0252-365">Malayalam</span></span></td>
        <td><span data-ttu-id="f0252-366">ml.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-366">ml.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-367">Maláj (latin betűs)</span><span class="sxs-lookup"><span data-stu-id="f0252-367">Malay (Latin)</span></span></td>
        <td><span data-ttu-id="f0252-368">MS.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-368">ms.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-369">marathi</span><span class="sxs-lookup"><span data-stu-id="f0252-369">Marathi</span></span></td>
        <td><span data-ttu-id="f0252-370">MR.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-370">mr.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-371">norvég</span><span class="sxs-lookup"><span data-stu-id="f0252-371">Norwegian</span></span></td>
        <td><span data-ttu-id="f0252-372">NB.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-372">nb.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-373">No.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-373">no.lucene</span></span></td>        
    </tr>
      <tr>
        <td><span data-ttu-id="f0252-374">perzsa</span><span class="sxs-lookup"><span data-stu-id="f0252-374">Persian</span></span></td>
        <td></td>
        <td><span data-ttu-id="f0252-375">fa.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-375">fa.lucene</span></span></td>        
      </tr>
    <tr>
        <td><span data-ttu-id="f0252-376">lengyel</span><span class="sxs-lookup"><span data-stu-id="f0252-376">Polish</span></span></td>
        <td><span data-ttu-id="f0252-377">pl.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-377">pl.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-378">pl.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-378">pl.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-379">Portugál (brazíliai)</span><span class="sxs-lookup"><span data-stu-id="f0252-379">Portuguese (Brazil)</span></span></td>
        <td><span data-ttu-id="f0252-380">PT-Br.microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-380">pt-Br.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-381">PT-Br.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-381">pt-Br.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-382">Portugál (Portugália)</span><span class="sxs-lookup"><span data-stu-id="f0252-382">Portuguese (Portugal)</span></span></td>
        <td><span data-ttu-id="f0252-383">PT-Pt.microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-383">pt-Pt.microsoft</span></span></td>        
        <td><span data-ttu-id="f0252-384">PT-Pt.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-384">pt-Pt.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-385">pandzsábi</span><span class="sxs-lookup"><span data-stu-id="f0252-385">Punjabi</span></span></td>
        <td><span data-ttu-id="f0252-386">Pa.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-386">pa.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-387">román</span><span class="sxs-lookup"><span data-stu-id="f0252-387">Romanian</span></span></td>
        <td><span data-ttu-id="f0252-388">ro.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-388">ro.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-389">ro.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-389">ro.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-390">orosz</span><span class="sxs-lookup"><span data-stu-id="f0252-390">Russian</span></span></td>
        <td><span data-ttu-id="f0252-391">RU.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-391">ru.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-392">RU.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-392">ru.lucene</span></span></td>    
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-393">szerb (cirill betűs)</span><span class="sxs-lookup"><span data-stu-id="f0252-393">Serbian (Cyrillic)</span></span></td>
        <td><span data-ttu-id="f0252-394">az SR-cyrillic.microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-394">sr-cyrillic.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-395">Szerb (latin betűs)</span><span class="sxs-lookup"><span data-stu-id="f0252-395">Serbian (Latin)</span></span></td>
        <td><span data-ttu-id="f0252-396">az SR-latin.microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-396">sr-latin.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-397">szlovák</span><span class="sxs-lookup"><span data-stu-id="f0252-397">Slovak</span></span></td>
        <td><span data-ttu-id="f0252-398">SK.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-398">sk.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-399">szlovén</span><span class="sxs-lookup"><span data-stu-id="f0252-399">Slovenian</span></span></td>
        <td><span data-ttu-id="f0252-400">SL.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-400">sl.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-401">spanyol</span><span class="sxs-lookup"><span data-stu-id="f0252-401">Spanish</span></span></td>
        <td><span data-ttu-id="f0252-402">es.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-402">es.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-403">es.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-403">es.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-404">svéd</span><span class="sxs-lookup"><span data-stu-id="f0252-404">Swedish</span></span></td>
        <td><span data-ttu-id="f0252-405">SV.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-405">sv.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-406">SV.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-406">sv.lucene</span></span></td>
    </tr>

    <tr>
        <td><span data-ttu-id="f0252-407">tamil</span><span class="sxs-lookup"><span data-stu-id="f0252-407">Tamil</span></span></td>
        <td><span data-ttu-id="f0252-408">TA.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-408">ta.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-409">telugu</span><span class="sxs-lookup"><span data-stu-id="f0252-409">Telugu</span></span></td>
        <td><span data-ttu-id="f0252-410">Te.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-410">te.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-411">thai</span><span class="sxs-lookup"><span data-stu-id="f0252-411">Thai</span></span></td>
        <td><span data-ttu-id="f0252-412">TH.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-412">th.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-413">TH.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-413">th.lucene</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-414">török</span><span class="sxs-lookup"><span data-stu-id="f0252-414">Turkish</span></span></td>
        <td><span data-ttu-id="f0252-415">tr.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-415">tr.microsoft</span></span></td>
        <td><span data-ttu-id="f0252-416">tr.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-416">tr.lucene</span></span></td>        
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-417">ukrán</span><span class="sxs-lookup"><span data-stu-id="f0252-417">Ukrainian</span></span></td>
        <td><span data-ttu-id="f0252-418">UK.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-418">uk.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-419">urdu</span><span class="sxs-lookup"><span data-stu-id="f0252-419">Urdu</span></span></td>
        <td><span data-ttu-id="f0252-420">Your.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-420">ur.microsoft</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-421">vietnami</span><span class="sxs-lookup"><span data-stu-id="f0252-421">Vietnamese</span></span></td>
        <td><span data-ttu-id="f0252-422">VI.Microsoft</span><span class="sxs-lookup"><span data-stu-id="f0252-422">vi.microsoft</span></span></td>
        <td></td>
    </tr>
    <td colspan="3"><span data-ttu-id="f0252-423">Emellett az Azure Search biztosít nyelvtől független analyzer konfigurációk</span><span class="sxs-lookup"><span data-stu-id="f0252-423">Additionally Azure Search provides language-agnostic analyzer configurations</span></span></td>
    <tr>
        <td><span data-ttu-id="f0252-424">A szabványos ASCII-részekre bontás</span><span class="sxs-lookup"><span data-stu-id="f0252-424">Standard ASCII Folding</span></span></td>
        <td><span data-ttu-id="f0252-425">standardasciifolding.lucene</span><span class="sxs-lookup"><span data-stu-id="f0252-425">standardasciifolding.lucene</span></span></td>
        <td>
        <ul>
            <li><span data-ttu-id="f0252-426">Unicode szöveg Szegmentálás (szabványos jogkivonatokat létrehozó)</span><span class="sxs-lookup"><span data-stu-id="f0252-426">Unicode text segmentation (Standard Tokenizer)</span></span></li>
            <li><span data-ttu-id="f0252-427">ASCII modellrész-létrehozási szűrő - Unicode-karaktereket, amelyek nem tartoznak a először 127 ASCII-karakterekből álló az ASCII megfelelő konvertálja.</span><span class="sxs-lookup"><span data-stu-id="f0252-427">ASCII folding filter - converts Unicode characters that don't belong to the set of first 127 ASCII characters into their ASCII equivalents.</span></span> <span data-ttu-id="f0252-428">Ez akkor hasznos, a ékezetek eltávolításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-428">This is useful for removing diacritics.</span></span></li>
        </ul>
        </td>
    </tr>
</table>

<span data-ttu-id="f0252-429">Nevek attribútummal rendelkező összes elemzőkkel <i>lucene</i> szerint vannak kapcsolva [Apache Lucene nyelvi elemzőkkel](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html).</span><span class="sxs-lookup"><span data-stu-id="f0252-429">All analyzers with names annotated with <i>lucene</i> are powered by [Apache Lucene's language analyzers](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html).</span></span> <span data-ttu-id="f0252-430">További információ az ASCII-részekre bontás szűrő található [Itt](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).</span><span class="sxs-lookup"><span data-stu-id="f0252-430">More information about the ASCII folding filter can be found [here](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).</span></span>

<span data-ttu-id="f0252-431">**Javaslattevők**</span><span class="sxs-lookup"><span data-stu-id="f0252-431">**Suggesters**</span></span>

<span data-ttu-id="f0252-432">A `suggester` határozza meg, melyik index mező használható támogatja a keresést automatikusan hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="f0252-432">A `suggester` defines which fields in an index are used to support auto-complete in searches.</span></span> <span data-ttu-id="f0252-433">Részleges keresőkifejezések küldött általában a [javaslatok API](#Suggestions) közben a felhasználó éppen gépel egy keresési lekérdezés, és az API-t adja vissza, amely javasolt kifejezések.</span><span class="sxs-lookup"><span data-stu-id="f0252-433">Typically partial search strings are sent to the [Suggestions API](#Suggestions) while the user is typing a search query, and the API returns a set of suggested phrases.</span></span> <span data-ttu-id="f0252-434">A javaslattevő az indexet meghatározó meghatározza, hogy mely mezők alapján állítja össze a begépelt keresési feltételeket.</span><span class="sxs-lookup"><span data-stu-id="f0252-434">A suggester that you define in the index determines which fields are used to build the type-ahead search terms.</span></span> <span data-ttu-id="f0252-435">Lásd: [Javaslattevők](#Suggesters) konfigurációs részletek.</span><span class="sxs-lookup"><span data-stu-id="f0252-435">See [Suggesters](#Suggesters) for configuration details.</span></span>

<span data-ttu-id="f0252-436">**Pontozási profilok**</span><span class="sxs-lookup"><span data-stu-id="f0252-436">**Scoring profiles**</span></span>

<span data-ttu-id="f0252-437">A `scoringProfile` egyéni pontozási viselkedéseket, amelyek lehetővé teszik, hogy befolyásolják mely elemek jelennek meg a keresési eredmények magasabb határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f0252-437">A `scoringProfile` defines custom scoring behaviors that let you influence which items appear higher in the search results.</span></span> <span data-ttu-id="f0252-438">A pontozási profil mező súlyok és funkciók épülnek fel.</span><span class="sxs-lookup"><span data-stu-id="f0252-438">Scoring profiles are made up of field weights and functions.</span></span> <span data-ttu-id="f0252-439">Használja őket, meg kell adnia egy profilt a lekérdezési karakterlánc neve.</span><span class="sxs-lookup"><span data-stu-id="f0252-439">To use them, you specify a profile by name on the query string.</span></span>

<span data-ttu-id="f0252-440">A pontozási profil alapértelmezett működik, a háttérben minden eleme egy eredményhalmazban szerepel egy keresési pontszám kiszámításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-440">A default scoring profile operates behind the scenes to compute a search score for every item in a result set.</span></span> <span data-ttu-id="f0252-441">Használhatja a belső relevanciaprofil névtelen.</span><span class="sxs-lookup"><span data-stu-id="f0252-441">You can use the internal, unnamed scoring profile.</span></span> <span data-ttu-id="f0252-442">Másik lehetőségként állítsa `defaultScoringProfile` egy egyéni-profilt kell használnia az alapértelmezett, meghívni, amikor egyéni profil nincs megadva a lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="f0252-442">Alternatively, set `defaultScoringProfile` to use a custom profile as the default, invoked whenever a custom profile is not specified on the query string.</span></span>

<span data-ttu-id="f0252-443">Lásd: [pontozási profil hozzáadása egy keresési indexszel (Azure Search szolgáltatás REST API)](search-api-scoring-profiles-2015-02-28-preview.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="f0252-443">See [Add scoring profiles to a search index (Azure Search Service REST API)](search-api-scoring-profiles-2015-02-28-preview.md) for details.</span></span>

<span data-ttu-id="f0252-444">**A CORS-beállítások**</span><span class="sxs-lookup"><span data-stu-id="f0252-444">**CORS Options**</span></span>

<span data-ttu-id="f0252-445">Ügyféloldali Javascript nem hívható bármely API-k alapértelmezés szerint, mert a böngésző megakadályozza, hogy minden eltérő eredetű kérések.</span><span class="sxs-lookup"><span data-stu-id="f0252-445">Client-side Javascript cannot call any APIs by default since the browser will prevent all cross-origin requests.</span></span> <span data-ttu-id="f0252-446">A CORS (eltérő eredetű erőforrások megosztása) módosításával engedélyezheti a `corsOptions` engedélyezi eltérő eredetű lekérdezéseket, amelyekkel az index attribútum.</span><span class="sxs-lookup"><span data-stu-id="f0252-446">Enable CORS (Cross-Origin Resource Sharing) by setting the `corsOptions` attribute to allow cross-origin queries to your index.</span></span> <span data-ttu-id="f0252-447">Vegye figyelembe, hogy egyetlen lekérdezés biztonsági okokból a CORS API-k támogatása.</span><span class="sxs-lookup"><span data-stu-id="f0252-447">Note that only query APIs support CORS for security reasons.</span></span> <span data-ttu-id="f0252-448">A CORS állíthat be a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="f0252-448">The following options can be set for CORS:</span></span>

* <span data-ttu-id="f0252-449">`allowedOrigins`(kötelező): a lista tartalmazza, amelyek az indexe hozzáférési engedélyt.</span><span class="sxs-lookup"><span data-stu-id="f0252-449">`allowedOrigins` (required): This is a list of origins that will be granted access to your index.</span></span> <span data-ttu-id="f0252-450">Ez azt jelenti, hogy az adott források kiszolgált bármelyik Javascript-kód lekérdezheti az indexét (feltéve, hogy a megfelelő API-kulcsot biztosít) engedélyezett lesz.</span><span class="sxs-lookup"><span data-stu-id="f0252-450">This means that any Javascript code served from those origins will be allowed to query your index (assuming it provides the correct API key).</span></span> <span data-ttu-id="f0252-451">Eredetek formátuma általában az űrlap `protocol://fully-qualified-domain-name:port` bár gyakran nincs megadva a port.</span><span class="sxs-lookup"><span data-stu-id="f0252-451">Each origin is typically of the form `protocol://fully-qualified-domain-name:port` although the port is often omitted.</span></span> <span data-ttu-id="f0252-452">Lásd: [Ez a cikk](http://go.microsoft.com/fwlink/?LinkId=330822) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="f0252-452">See [this article](http://go.microsoft.com/fwlink/?LinkId=330822) for more details.</span></span>
  * <span data-ttu-id="f0252-453">Ha azt szeretné, hogy férjen hozzá minden eredet, `*` egyetlen elemként a `allowedOrigins` tömb.</span><span class="sxs-lookup"><span data-stu-id="f0252-453">If you want to allow access to all origins, include `*` as a single item in the `allowedOrigins` array.</span></span> <span data-ttu-id="f0252-454">Vegye figyelembe, hogy **ez nem ajánlott eljárás a termelési keresési szolgáltatások.**</span><span class="sxs-lookup"><span data-stu-id="f0252-454">Note that **this is not recommended practice for production search services.**</span></span> <span data-ttu-id="f0252-455">Azonban hasznos lehet a fejlesztési vagy a hibakeresési célra.</span><span class="sxs-lookup"><span data-stu-id="f0252-455">However, it may be useful for development or debugging purposes.</span></span>
* <span data-ttu-id="f0252-456">`maxAgeInSeconds`(nem kötelező): a böngészők arra használják ezt az értéket időtartama (másodpercben) meghatározásához a gyorsítótár CORS-alapú elővizsgálati válaszok.</span><span class="sxs-lookup"><span data-stu-id="f0252-456">`maxAgeInSeconds` (optional): Browsers use this value to determine the duration (in seconds) to cache CORS preflight responses.</span></span> <span data-ttu-id="f0252-457">Ez nem negatív egész számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f0252-457">This must be a non-negative integer.</span></span> <span data-ttu-id="f0252-458">Minél nagyobb ez az érték, a jobb teljesítmény, de annál tovább fog tartani, amíg a CORS-házirend változásai érvénybe léptetéséhez.</span><span class="sxs-lookup"><span data-stu-id="f0252-458">The larger this value is, the better performance will be, but the longer it will take for CORS policy changes to take effect.</span></span> <span data-ttu-id="f0252-459">Ha nincs megadva, a alapértelmezett 5 perces időtartamot használható.</span><span class="sxs-lookup"><span data-stu-id="f0252-459">If it is not set, a default duration of 5 minutes will be used.</span></span>

<span data-ttu-id="f0252-460"><a name="CreateUpdateIndexExample"></a>
**Kérelem törzse – példa**</span><span class="sxs-lookup"><span data-stu-id="f0252-460"><a name="CreateUpdateIndexExample"></a>
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

<span data-ttu-id="f0252-461">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="f0252-461">**Response**</span></span>

<span data-ttu-id="f0252-462">A kérelem sikeres: "201 Created".</span><span class="sxs-lookup"><span data-stu-id="f0252-462">For a successful request: "201 Created".</span></span>

<span data-ttu-id="f0252-463">Alapértelmezés szerint az adott válasz törzsének meg az index létrehozásának JSON fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="f0252-463">By default the response body will contain the JSON for the index definition that was created.</span></span> <span data-ttu-id="f0252-464">Ha a `Prefer` fejléc értéke `return=minimal`, az adott válasz törzse üres lesz, és a sikeres állapotkód lesz "204 Nincs tartalom" helyett "201 Created".</span><span class="sxs-lookup"><span data-stu-id="f0252-464">If the `Prefer` request header is set to `return=minimal`, the response body will be empty and the success status code will be "204 No Content" instead of "201 Created".</span></span> <span data-ttu-id="f0252-465">Ez érvényét veszti, függetlenül attól, hogy PUT vagy POST használatával hozza létre az indexet.</span><span class="sxs-lookup"><span data-stu-id="f0252-465">This is true regardless of whether PUT or POST was used to create the index.</span></span>

<span data-ttu-id="f0252-466">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="f0252-466">**Remarks**</span></span>

<span data-ttu-id="f0252-467">Jelenleg korlátozott támogatása az index sémafrissítések.</span><span class="sxs-lookup"><span data-stu-id="f0252-467">Currently, there is limited support for index schema updates.</span></span> <span data-ttu-id="f0252-468">Bármely sémafrissítések újraindexelés, például a mezőtípusok módosítása újraindexelést igénylő jelenleg nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="f0252-468">Any schema updates that would require re-indexing such as changing field types are not currently supported.</span></span> <span data-ttu-id="f0252-469">Bár a már meglévő mezők nem módosítható vagy törölhető, új mezők felveheti létező indexek bármikor.</span><span class="sxs-lookup"><span data-stu-id="f0252-469">Although existing fields cannot be changed or deleted, new fields can be added to an existing index at any time.</span></span> <span data-ttu-id="f0252-470">Amikor új mező ad hozzá, az indexben szereplő összes meglévő dokumentumok automatikusan fog rendelkezni a mező null értékű.</span><span class="sxs-lookup"><span data-stu-id="f0252-470">When a new field is added, all existing documents in the index will automatically have a null value for that field.</span></span> <span data-ttu-id="f0252-471">További tárhely az új dokumentumok az indexhez felvételének befejezéséig fognak használni.</span><span class="sxs-lookup"><span data-stu-id="f0252-471">No additional storage space will be consumed until new documents are added to the index.</span></span>

<a name="Suggesters"></a>

## <a name="suggesters"></a><span data-ttu-id="f0252-472">Javaslattevők</span><span class="sxs-lookup"><span data-stu-id="f0252-472">Suggesters</span></span>
<span data-ttu-id="f0252-473">A javaslatok az Azure Search szolgáltatása begépelt vagy automatikusan hajthat végre a lekérdezés egy olyan képességet, jegyez be a keresőmezőbe részleges karakterlánc-bemenetek válaszul lehetséges keresési feltételeinek megadása.</span><span class="sxs-lookup"><span data-stu-id="f0252-473">The suggestions feature in Azure Search is a type-ahead or auto-complete query capability, providing a list of potential search terms in response to partial string inputs entered into a search box.</span></span> <span data-ttu-id="f0252-474">Bizonyára észrevette konstrukciókat kereskedelmi keresőmotorok használatakor: írja be a ".NET" Bing előállító feltételei listájának ".NET 4.5-ös", ".NET-keretrendszer 3.5-ös", és így tovább.</span><span class="sxs-lookup"><span data-stu-id="f0252-474">You've probably noticed query suggestions when using commercial web search engines: typing ".NET" in Bing produces a list of terms for ".NET 4.5", ".NET Framework 3.5", and so forth.</span></span> <span data-ttu-id="f0252-475">A keresési szolgáltatás REST API használata esetén a következők javaslatok valósít meg egyéni Azure Search alkalmazás szükségesek:</span><span class="sxs-lookup"><span data-stu-id="f0252-475">When using the Search service REST API, implementing suggestions in a custom Azure Search application requires the following:</span></span>

* <span data-ttu-id="f0252-476">Engedélyezze a javaslatok hozzáadása egy **javaslattevő** meghívták az indexben, amely a neve, a keresési mód és a mezőket sorolja fel, amelynek begépelt konstrukció.</span><span class="sxs-lookup"><span data-stu-id="f0252-476">Enable suggestions by adding a **suggester** construction in your index, giving the name, search mode, and a list of fields for which type-ahead is invoked.</span></span> <span data-ttu-id="f0252-477">Például ha "város" mező, írja be a "Tenger" részleges keresési karakterlánc jár a "Seattle", "Tengerpart" és "Seatac" (mind a hármat olyan tényleges város nevek) lekérdezés javaslatként a felhasználó számára biztosított.</span><span class="sxs-lookup"><span data-stu-id="f0252-477">For example, if you specify "cityName" as a source field, typing a partial search string of "Sea" will result in "Seattle", "Seaside", and "Seatac" (all three are actual city names) offered up as query suggestions to the user.</span></span>
* <span data-ttu-id="f0252-478">Javaslatok meghívásához hívja a [javaslatok API](#Suggestions) az alkalmazás kódjában.</span><span class="sxs-lookup"><span data-stu-id="f0252-478">Invoke suggestions by calling the [Suggestions API](#Suggestions) in your application code.</span></span> <span data-ttu-id="f0252-479">Általában részleges keresőkifejezések vannak a szolgáltatásnak küldött, miközben a felhasználó éppen gépel egy keresési lekérdezés, és ez az API javasolt kifejezések készletét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="f0252-479">Typically partial search strings are sent to the service while the user is typing a search query, and this API returns a set of suggested phrases.</span></span>

<span data-ttu-id="f0252-480">Ez a cikk azt ismerteti, hogyan konfigurálhatja a **javaslattevő**.</span><span class="sxs-lookup"><span data-stu-id="f0252-480">This article explains how to configure a **suggester**.</span></span> <span data-ttu-id="f0252-481">Tekintse meg a [javaslatok API](#Suggestions) talál részletes információt a javaslattevő használatáról.</span><span class="sxs-lookup"><span data-stu-id="f0252-481">You should also review the [Suggestions API](#Suggestions) for details on how a suggester is used.</span></span>

<span data-ttu-id="f0252-482">**Használat**</span><span class="sxs-lookup"><span data-stu-id="f0252-482">**Usage**</span></span>

<span data-ttu-id="f0252-483">`Suggesters`jönnek létre az index és a munka legjobban, ha annak alapján hozza létre az adott dokumentumok helyett laza szót vagy kifejezést.</span><span class="sxs-lookup"><span data-stu-id="f0252-483">`Suggesters` are created in the index and work best when used to suggest specific documents rather than loose terms or phrases.</span></span> <span data-ttu-id="f0252-484">A legjobb jelölt mezők kitöltése, címek, nevek és egyéb viszonylag rövid kifejezés elem azonosításra alkalmas.</span><span class="sxs-lookup"><span data-stu-id="f0252-484">The best candidate fields are titles, names, and other relatively short phrases that can identify an item.</span></span> <span data-ttu-id="f0252-485">Kevésbé hatékony olyan ismétlődő, például a kategóriák és a címkék, és nagyon hosszú mezők például leírásokat és megjegyzéseket mezőket.</span><span class="sxs-lookup"><span data-stu-id="f0252-485">Less effective are repetitive fields, such as categories and tags, or very long fields such as descriptions or comments fields.</span></span>

<span data-ttu-id="f0252-486">Az index definícióját részeként is hozzáadhat egy egyetlen javaslattevő, hogy a `suggesters` gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="f0252-486">As part of the index definition, you can add a single suggester to the `suggesters` collection.</span></span> <span data-ttu-id="f0252-487">A javaslattevő meghatározó tulajdonságok közé tartoznak a következők:</span><span class="sxs-lookup"><span data-stu-id="f0252-487">Properties that define a suggester include the following:</span></span>

* <span data-ttu-id="f0252-488">`name`: Az a javaslattevő neve.</span><span class="sxs-lookup"><span data-stu-id="f0252-488">`name`: The name of the suggester.</span></span> <span data-ttu-id="f0252-489">A javaslattevő nevét használja, meghívásakor a `suggest` API.</span><span class="sxs-lookup"><span data-stu-id="f0252-489">You use the name of the suggester when calling the `suggest` API.</span></span>
* <span data-ttu-id="f0252-490">`searchMode`: A jelölt kifejezések keresésére használt stratégia.</span><span class="sxs-lookup"><span data-stu-id="f0252-490">`searchMode`: The strategy used to search for candidate phrases.</span></span> <span data-ttu-id="f0252-491">A jelenleg egyetlen támogatott mód van `analyzingInfixMatching`, melyik teljesít rugalmas megfelelő mondat eleji vagy mondat közepén.</span><span class="sxs-lookup"><span data-stu-id="f0252-491">The only mode currently supported is `analyzingInfixMatching`, which performs flexible matching of phrases at the beginning or in the middle of sentences.</span></span>
* <span data-ttu-id="f0252-492">`sourceFields`: Egy vagy több mezőinek listáját, amelyek a forrás javaslatok tartalmának.</span><span class="sxs-lookup"><span data-stu-id="f0252-492">`sourceFields`: A list of one or more fields that are the source of the content for suggestions.</span></span> <span data-ttu-id="f0252-493">Csak típusú mezők `Edm.String` és `Collection(Edm.String)` lehet, hogy javaslatokat adatforrásait.</span><span class="sxs-lookup"><span data-stu-id="f0252-493">Only fields of type `Edm.String` and `Collection(Edm.String)` may be sources for suggestions.</span></span> <span data-ttu-id="f0252-494">Csak olyan mezőket, amelyekre nincs beállítva egyéni nyelvi elemzőt is használható.</span><span class="sxs-lookup"><span data-stu-id="f0252-494">Only fields that don't have a custom language analyzer set can be used.</span></span>

<span data-ttu-id="f0252-495">**A javaslattevő – példa**</span><span class="sxs-lookup"><span data-stu-id="f0252-495">**Suggester Example**</span></span>

<span data-ttu-id="f0252-496">A javaslattevő az index részét képezi.</span><span class="sxs-lookup"><span data-stu-id="f0252-496">A suggester is part of the index.</span></span> <span data-ttu-id="f0252-497">Csak egy javaslattevő is szerepel a `suggesters` mellett a mezők gyűjteményében a jelenlegi verzióban gyűjtemény és `scoringProfiles`.</span><span class="sxs-lookup"><span data-stu-id="f0252-497">Only one suggester can exist in the `suggesters` collection in the current version, alongside the fields collection and `scoringProfiles`.</span></span>

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
> <span data-ttu-id="f0252-498">Ha használta az Azure Search nyilvános előzetes verziójának `suggesters` a felváltja egy régebbi logikai tulajdonság (`"suggestions": false`), amely csak a támogatott előtag javaslatok rövid karakterláncok (a 3-25 karakterből álló).</span><span class="sxs-lookup"><span data-stu-id="f0252-498">If you used the public preview version of Azure Search, `suggesters` replaces an older boolean property (`"suggestions": false`) that only supported prefix suggestions for short strings (3-25 characters).</span></span> <span data-ttu-id="f0252-499">A csere `suggesters`, támogatja a infix megfelelő megtalált jobb tűréssel tévesztések a keresési karakterláncot az egyező kifejezések elején vagy közepén mező tartalmát.</span><span class="sxs-lookup"><span data-stu-id="f0252-499">Its replacement, `suggesters`, supports infix matching that finds matching terms at the beginning or in the middle of field content, with better tolerance for mistakes in search strings.</span></span> <span data-ttu-id="f0252-500">Az általánosan elérhető kiadástól kezdve ez már a javaslatok API csak végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="f0252-500">Starting with the generally available release, this is now the only implementation of the suggestions API.</span></span> <span data-ttu-id="f0252-501">A régebbi `suggestions` bevezetett tulajdonság `api-version=2014-07-31-Preview` ebben a verzióban használható folyamatosan, de nincs operatív a `2015-02-28` vagy az Azure Search újabb verzióit.</span><span class="sxs-lookup"><span data-stu-id="f0252-501">The older `suggestions` property that was introduced in `api-version=2014-07-31-Preview` continues to work in that version, but is not operational in the `2015-02-28` or later versions of Azure Search.</span></span>
> 
> 

<a name="UpdateIndex"></a>

## <a name="update-index"></a><span data-ttu-id="f0252-502">Index frissítése</span><span class="sxs-lookup"><span data-stu-id="f0252-502">Update Index</span></span>
<span data-ttu-id="f0252-503">Frissítheti a meglévő index belül Azure Search használatával egy HTTP PUT-kérelmet.</span><span class="sxs-lookup"><span data-stu-id="f0252-503">You can update an existing index within Azure Search using an HTTP PUT request.</span></span> <span data-ttu-id="f0252-504">Frissítések lehetnek új mező hozzáadása a meglévő séma, a CORS-beállítások módosítása és a pontozási profil módosítása.</span><span class="sxs-lookup"><span data-stu-id="f0252-504">Updates can include adding new fields to the existing schema, modifying CORS options, and modifying scoring profiles.</span></span> <span data-ttu-id="f0252-505">Lásd: [hozzáadása a pontozási profil](https://msdn.microsoft.com/library/azure/dn798928.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="f0252-505">See [Add scoring Profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx) for more information.</span></span> <span data-ttu-id="f0252-506">A kérelem URI-azonosítója frissítéséhez index nevét kell megadni:</span><span class="sxs-lookup"><span data-stu-id="f0252-506">You specify the name of the index to update on the request URI:</span></span>

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="f0252-507">**Fontos:** index sémafrissítések támogatása korlátozódik, amelyek nem igényelnek, a search-index újraépítése műveletek.</span><span class="sxs-lookup"><span data-stu-id="f0252-507">**Important:** Support for index schema updates is limited to operations that don't require rebuilding the search index.</span></span> <span data-ttu-id="f0252-508">Bármely sémafrissítések műveletek, például a mezőtípusok módosítása újraindexelést igénylő jelenleg nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="f0252-508">Any schema updates that would require re-indexing, such as changing field types, are not currently supported.</span></span> <span data-ttu-id="f0252-509">Új mezők hozzáadása bármikor, noha a már meglévő mezők nem módosítható vagy törölhető.</span><span class="sxs-lookup"><span data-stu-id="f0252-509">New fields can be added at any time, although existing fields cannot be changed or deleted.</span></span> <span data-ttu-id="f0252-510">Ugyanez érvényes `suggesters`.</span><span class="sxs-lookup"><span data-stu-id="f0252-510">The same applies to `suggesters`.</span></span> <span data-ttu-id="f0252-511">Új mezők adhatók hozzá, a javaslattevő időpontjában mezőket ad hozzá, de a mezők nem távolítható el `suggesters` és a már meglévő mezők nem vehető fel `suggesters`.</span><span class="sxs-lookup"><span data-stu-id="f0252-511">New fields may be added to a suggester at the time the fields are added, but fields cannot be removed from `suggesters` and existing fields cannot be added to `suggesters`.</span></span>

<span data-ttu-id="f0252-512">Új mező hozzáadása az index, amikor az indexben szereplő összes meglévő dokumentumok automatikusan fog rendelkezni a mező null értékű.</span><span class="sxs-lookup"><span data-stu-id="f0252-512">When adding a new field to an index, all existing documents in the index will automatically have a null value for that field.</span></span> <span data-ttu-id="f0252-513">További tárhely az új dokumentumok az indexhez felvételének befejezéséig fognak használni.</span><span class="sxs-lookup"><span data-stu-id="f0252-513">No additional storage space will be consumed until new documents are added to the index.</span></span>

<span data-ttu-id="f0252-514">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="f0252-514">**Request**</span></span>

<span data-ttu-id="f0252-515">HTTPS-kapcsolat szükséges az összes szolgáltatási kérelmek.</span><span class="sxs-lookup"><span data-stu-id="f0252-515">HTTPS is required for all service requests.</span></span> <span data-ttu-id="f0252-516">A **Index frissítése** segítségével a HTTP PUT kérés jön létre.</span><span class="sxs-lookup"><span data-stu-id="f0252-516">The **Update Index** request is constructed using HTTP PUT.</span></span> <span data-ttu-id="f0252-517">A PUT érvénytelen az URL-cím.</span><span class="sxs-lookup"><span data-stu-id="f0252-517">With PUT, the index name is part of the URL.</span></span> <span data-ttu-id="f0252-518">Ha az index nem létezik, akkor jön létre.</span><span class="sxs-lookup"><span data-stu-id="f0252-518">If the index doesn't exist, it is created.</span></span> <span data-ttu-id="f0252-519">Ha az index már létezik, az új definition frissül.</span><span class="sxs-lookup"><span data-stu-id="f0252-519">If the index already exists, it is updated to the new definition.</span></span>

<span data-ttu-id="f0252-520">Az index neve kell kell kisbetű, betűvel vagy számmal kezdődhet, nincs perjeleket vagy pontokból és legfeljebb 128 karakter lehet.</span><span class="sxs-lookup"><span data-stu-id="f0252-520">The index name must be lower case, start with a letter or number, have no slashes or dots, and be less than 128 characters.</span></span> <span data-ttu-id="f0252-521">Után az index nevének betűvel vagy számmal verziótól kezdődően a nevét a többi tartalmazhatnak bármely betű, szám és kötőjeleket, mindaddig, amíg a kötőjelek nincsenek egymást követő.</span><span class="sxs-lookup"><span data-stu-id="f0252-521">After starting the index name with a letter or number, the rest of the name can include any letter, number and dashes, as long as the dashes are not consecutive.</span></span>

<span data-ttu-id="f0252-522">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="f0252-522">`api-version=[string]` (required).</span></span> <span data-ttu-id="f0252-523">Az előzetes verzió `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="f0252-523">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="f0252-524">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="f0252-524">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="f0252-525">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="f0252-525">**Request Headers**</span></span>

<span data-ttu-id="f0252-526">Az alábbi lista ismerteti a szükséges és választható kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="f0252-526">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="f0252-527">`Content-Type`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f0252-527">`Content-Type`: Required.</span></span> <span data-ttu-id="f0252-528">Állítsa ezt a beállítást`application/json`</span><span class="sxs-lookup"><span data-stu-id="f0252-528">Set this to `application/json`</span></span>
* <span data-ttu-id="f0252-529">`api-key`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f0252-529">`api-key`: Required.</span></span> <span data-ttu-id="f0252-530">A `api-key` hitelesíteni a kérelmet a keresőszolgáltatása szolgál.</span><span class="sxs-lookup"><span data-stu-id="f0252-530">The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="f0252-531">Egy karakterláncértéket, a szolgáltatás egyedi.</span><span class="sxs-lookup"><span data-stu-id="f0252-531">It is a string value, unique to your service.</span></span> <span data-ttu-id="f0252-532">A **Index frissítése** kérelem tartalmaznia kell egy `api-key` fejlécben az adminisztrációs kulcsot (nem egy lekérdezési kulcsot).</span><span class="sxs-lookup"><span data-stu-id="f0252-532">The **Update Index** request must include an `api-key` header set to your admin key (as opposed to a query key).</span></span>

<span data-ttu-id="f0252-533">Konfigurálnia kell a szolgáltatás nevét, a kérelem URL-címe összeállításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-533">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="f0252-534">Beszerezheti a szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján, az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f0252-534">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="f0252-535">Lásd: [Azure Search szolgáltatás létrehozása a portálon](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="f0252-535">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="f0252-536">**Törzs szintaxis**</span><span class="sxs-lookup"><span data-stu-id="f0252-536">**Request Body Syntax**</span></span>

<span data-ttu-id="f0252-537">Amikor frissíti a meglévő index, a szervezet tartalmaznia kell az eredeti sémadefiníciót és az új mezőket ad hozzá, valamint a módosított pontozási profilok javaslattevők és a CORS-beállítások, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="f0252-537">When updating an existing index, the body must include the original schema definition, plus the new fields you are adding, as well as the modified scoring profiles, suggesters and CORS options, if any.</span></span> <span data-ttu-id="f0252-538">Ha nem módosítja a pontozási profil és a CORS-beállítások, meg kell adnia az eredeti származó az index létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="f0252-538">If you are not modifying the scoring profiles and CORS options, you must include the originals from when the index was created.</span></span> <span data-ttu-id="f0252-539">Általában a legjobb minta frissítések kéri le az index definícióját a GET, módosítsa ezt, akkor PUT frissíti.</span><span class="sxs-lookup"><span data-stu-id="f0252-539">In general the best pattern to use for updates is to retrieve the index definition with a GET, modify it, then update it with PUT.</span></span>

<span data-ttu-id="f0252-540">Az index létrehozásához használt sémaszintaxis itt jelenik meg kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="f0252-540">The schema syntax used to create an index is reproduced here for convenience.</span></span> <span data-ttu-id="f0252-541">Lásd: [a Create Index](#CreateIndex) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="f0252-541">See [Create Index](#CreateIndex) for more details.</span></span>

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
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
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
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
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


<span data-ttu-id="f0252-542">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="f0252-542">**Response**</span></span>

<span data-ttu-id="f0252-543">A kérelem sikeres: "204 Nincs tartalom".</span><span class="sxs-lookup"><span data-stu-id="f0252-543">For a successful request: "204 No Content".</span></span>

<span data-ttu-id="f0252-544">Alapértelmezés szerint az adott válasz törzse üres lesz.</span><span class="sxs-lookup"><span data-stu-id="f0252-544">By default the response body will be empty.</span></span> <span data-ttu-id="f0252-545">Azonban ha a `Prefer` fejléc értéke `return=representation`, az adott válasz törzse fogja tartalmazni az index definícióját, hogy frissült a JSON.</span><span class="sxs-lookup"><span data-stu-id="f0252-545">However, if the `Prefer` request header is set to `return=representation`, the response body will contain the JSON for the index definition that was updated.</span></span> <span data-ttu-id="f0252-546">Ebben az esetben a sikeres állapotkód lesz "200 OK".</span><span class="sxs-lookup"><span data-stu-id="f0252-546">In this case, the success status code will be "200 OK".</span></span>

<span data-ttu-id="f0252-547">**Indexdefiníció egyéni lekérdezések frissítésekor**</span><span class="sxs-lookup"><span data-stu-id="f0252-547">**Updating index definition with custom analyzers**</span></span>

<span data-ttu-id="f0252-548">Miután egy elemző eszköz, a jogkivonatokat létrehozó, egy token szűrőt, vagy egy karakter szűrő be van állítva, nem lehet módosítani.</span><span class="sxs-lookup"><span data-stu-id="f0252-548">Once an analyzer, a tokenizer, a token filter or a char filter is defined, it cannot be modified.</span></span> <span data-ttu-id="f0252-549">Új címkék adhatók hozzá a létező indexek csak akkor, ha a `allowIndexDowntime` jelző értéke igaz, a index frissítési kérelem:</span><span class="sxs-lookup"><span data-stu-id="f0252-549">New ones can be added to an existing index only if the `allowIndexDowntime` flag is set to true in the index update request:</span></span> 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

<span data-ttu-id="f0252-550">Vegye figyelembe, hogy ez a művelet kerül az index offline legalább néhány másodpercen belül, amely az indexelést és a lekérdezés-kérelem sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="f0252-550">Note that this operation will put your index offline for at least a few seconds, causing your indexing and query requests to fail.</span></span> <span data-ttu-id="f0252-551">Az index teljesítmény- és írási rendelkezésre állását korlátozott az index frissítése után több percig vagy nagyon nagy indexek hosszabb lehet.</span><span class="sxs-lookup"><span data-stu-id="f0252-551">Performance and write availability of the index can be impaired for several minutes after the index is updated, or longer for very large indexes.</span></span>

<a name="ListIndexes"></a>

## <a name="list-indexes"></a><span data-ttu-id="f0252-552">Lista indexek</span><span class="sxs-lookup"><span data-stu-id="f0252-552">List Indexes</span></span>
<span data-ttu-id="f0252-553">A **lista indexek** művelet listáját adja vissza az indexek jelenleg az Azure Search szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="f0252-553">The **List Indexes** operation returns a list of the indexes currently in your Azure Search service.</span></span>

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="f0252-554">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="f0252-554">**Request**</span></span>

<span data-ttu-id="f0252-555">HTTPS-kapcsolat szükséges az összes szolgáltatási kérelmek.</span><span class="sxs-lookup"><span data-stu-id="f0252-555">HTTPS is required for all service requests.</span></span> <span data-ttu-id="f0252-556">A **lista indexek** kérelem a GET metódussal lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f0252-556">The **List Indexes** request can be constructed using the GET method.</span></span>

<span data-ttu-id="f0252-557">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="f0252-557">`api-version=[string]` (required).</span></span> <span data-ttu-id="f0252-558">Az előzetes verzió `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="f0252-558">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="f0252-559">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="f0252-559">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="f0252-560">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="f0252-560">**Request Headers**</span></span>

<span data-ttu-id="f0252-561">Az alábbi lista ismerteti a szükséges és választható kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="f0252-561">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="f0252-562">`api-key`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f0252-562">`api-key`: Required.</span></span> <span data-ttu-id="f0252-563">A `api-key` hitelesíteni a kérelmet a keresőszolgáltatása szolgál.</span><span class="sxs-lookup"><span data-stu-id="f0252-563">The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="f0252-564">Egy karakterláncértéket, a szolgáltatás egyedi.</span><span class="sxs-lookup"><span data-stu-id="f0252-564">It is a string value, unique to your service.</span></span> <span data-ttu-id="f0252-565">A **lista indexek** kérelem tartalmaznia kell egy `api-key` egy adminisztrációs kulcsot (szemben a lekérdezési kulcs) értékre.</span><span class="sxs-lookup"><span data-stu-id="f0252-565">The **List Indexes** request must include an `api-key` set to an admin key (as opposed to a query key).</span></span>

<span data-ttu-id="f0252-566">Konfigurálnia kell a szolgáltatás nevét, a kérelem URL-címe összeállításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-566">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="f0252-567">Beszerezheti a szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján, az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f0252-567">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="f0252-568">Lásd: [Azure Search szolgáltatás létrehozása a portálon](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="f0252-568">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="f0252-569">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="f0252-569">**Request Body**</span></span>

<span data-ttu-id="f0252-570">nincs.</span><span class="sxs-lookup"><span data-stu-id="f0252-570">None.</span></span>

<span data-ttu-id="f0252-571">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="f0252-571">**Response**</span></span>

<span data-ttu-id="f0252-572">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="f0252-572">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="f0252-573">Íme egy példa adott válasz törzse:</span><span class="sxs-lookup"><span data-stu-id="f0252-573">Here is an example response body:</span></span>

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

<span data-ttu-id="f0252-574">Vegye figyelembe, hogy a válasz le kíváncsiak vagyunk tulajdonságainak szűrheti.</span><span class="sxs-lookup"><span data-stu-id="f0252-574">Note that you can filter the response down to just the properties you're interested in.</span></span> <span data-ttu-id="f0252-575">Például, ha azt szeretné, hogy csak index neveinek listáját, használja az OData `$select` lekérdezési lehetőség:</span><span class="sxs-lookup"><span data-stu-id="f0252-575">For example, if you want only a list of index names, use the OData `$select` query option:</span></span>

    GET /indexes?api-version=2015-02-28-Preview&$select=name

<span data-ttu-id="f0252-576">Ebben az esetben a fenti példa válaszát távolságban jelenjen meg az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f0252-576">In this case, the response from the above example would appear as follows:</span></span>

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

<span data-ttu-id="f0252-577">Ez a sávszélesség menteni, ha szerepel a keresőszolgáltatása indexek számos hasznos technika.</span><span class="sxs-lookup"><span data-stu-id="f0252-577">This is a useful technique to save bandwidth if you have a lot of indexes in your Search service.</span></span>

<a name="GetIndex"></a>

## <a name="get-index"></a><span data-ttu-id="f0252-578">Index beolvasása</span><span class="sxs-lookup"><span data-stu-id="f0252-578">Get Index</span></span>
<span data-ttu-id="f0252-579">A **beolvasása Index** művelet lekérdezi az index definícióját az Azure Search.</span><span class="sxs-lookup"><span data-stu-id="f0252-579">The **Get Index** operation gets the index definition from Azure Search.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="f0252-580">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="f0252-580">**Request**</span></span>

<span data-ttu-id="f0252-581">HTTPS szolgáltatáskérések szükség.</span><span class="sxs-lookup"><span data-stu-id="f0252-581">HTTPS is required for service requests.</span></span> <span data-ttu-id="f0252-582">A **beolvasása Index** kérelem a GET metódussal lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f0252-582">The **Get Index** request can be constructed using the GET method.</span></span>

<span data-ttu-id="f0252-583">A kérelem URI azonosítója [index neve] határozza meg, hogy melyik index vissza az indexek gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="f0252-583">The [index name] in the request URI specifies which index to return from the indexes collection.</span></span>

<span data-ttu-id="f0252-584">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="f0252-584">`api-version=[string]` (required).</span></span> <span data-ttu-id="f0252-585">Az előzetes verzió `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="f0252-585">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="f0252-586">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="f0252-586">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="f0252-587">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="f0252-587">**Request Headers**</span></span>

<span data-ttu-id="f0252-588">Az alábbi lista ismerteti a szükséges és választható kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="f0252-588">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="f0252-589">`api-key`: A `api-key` hitelesíteni a kérelmet a keresőszolgáltatása szolgál.</span><span class="sxs-lookup"><span data-stu-id="f0252-589">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="f0252-590">Egy karakterláncértéket, a szolgáltatás egyedi.</span><span class="sxs-lookup"><span data-stu-id="f0252-590">It is a string value, unique to your service.</span></span> <span data-ttu-id="f0252-591">A **beolvasása Index** kérelem tartalmaznia kell egy `api-key` egy adminisztrációs kulcsot (szemben a lekérdezési kulcs) értékre.</span><span class="sxs-lookup"><span data-stu-id="f0252-591">The **Get Index** request must include an `api-key` set to an admin key (as opposed to a query key).</span></span>

<span data-ttu-id="f0252-592">Konfigurálnia kell a szolgáltatás nevét, a kérelem URL-címe összeállításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-592">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="f0252-593">Beszerezheti a szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján, az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f0252-593">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="f0252-594">Lásd: [Azure Search szolgáltatás létrehozása a portálon](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="f0252-594">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="f0252-595">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="f0252-595">**Request Body**</span></span>

<span data-ttu-id="f0252-596">nincs.</span><span class="sxs-lookup"><span data-stu-id="f0252-596">None.</span></span>

<span data-ttu-id="f0252-597">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="f0252-597">**Response**</span></span>

<span data-ttu-id="f0252-598">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="f0252-598">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="f0252-599">A következő példa JSON a [létrehozása és frissítése Index](#CreateUpdateIndexExample) a válasz forgalma példát.</span><span class="sxs-lookup"><span data-stu-id="f0252-599">See the example JSON in [Creating and Updating an Index](#CreateUpdateIndexExample) for an example of the response payload.</span></span>

<a name="DeleteIndex"></a>

## <a name="delete-index"></a><span data-ttu-id="f0252-600">Index törlése</span><span class="sxs-lookup"><span data-stu-id="f0252-600">Delete Index</span></span>
<span data-ttu-id="f0252-601">A **Index törlése** művelet eltávolítja az index és a kapcsolódó dokumentumok az Azure Search szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f0252-601">The **Delete Index** operation removes an index and associated documents from your Azure Search service.</span></span> <span data-ttu-id="f0252-602">Az index neve kaphat a szolgáltatás irányítópultján, az Azure portálon, vagy az API-t.</span><span class="sxs-lookup"><span data-stu-id="f0252-602">You can get the index name from the service dashboard in the Azure portal, or from the API.</span></span> <span data-ttu-id="f0252-603">Lásd: [lista indexek](#ListIndexes) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="f0252-603">See [List Indexes](#ListIndexes) for details.</span></span>

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

<span data-ttu-id="f0252-604">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="f0252-604">**Request**</span></span>

<span data-ttu-id="f0252-605">HTTPS szolgáltatáskérések szükség.</span><span class="sxs-lookup"><span data-stu-id="f0252-605">HTTPS is required for service requests.</span></span> <span data-ttu-id="f0252-606">A **Index törlése** kérelmet a DELETE metódussal lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f0252-606">The **Delete Index** request can be constructed using the DELETE method.</span></span>

<span data-ttu-id="f0252-607">A kérelem URI azonosítója [index neve] határozza meg, hogy melyik index a indexgyűjteményhez törölheti.</span><span class="sxs-lookup"><span data-stu-id="f0252-607">The [index name] in the request URI specifies which index to delete from the indexes collection.</span></span>

<span data-ttu-id="f0252-608">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="f0252-608">`api-version=[string]` (required).</span></span> <span data-ttu-id="f0252-609">Az előzetes verzió `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="f0252-609">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="f0252-610">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="f0252-610">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="f0252-611">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="f0252-611">**Request Headers**</span></span>

<span data-ttu-id="f0252-612">Az alábbi lista ismerteti a szükséges és választható kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="f0252-612">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="f0252-613">`api-key`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f0252-613">`api-key`: Required.</span></span> <span data-ttu-id="f0252-614">A `api-key` hitelesíteni a kérelmet a keresőszolgáltatása szolgál.</span><span class="sxs-lookup"><span data-stu-id="f0252-614">The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="f0252-615">Egy karakterláncértéket egyedi, a szolgáltatás URL-címre.</span><span class="sxs-lookup"><span data-stu-id="f0252-615">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="f0252-616">A **Index törlése** kérelem tartalmaznia kell egy `api-key` fejlécben az adminisztrációs kulcsot (nem egy lekérdezési kulcsot).</span><span class="sxs-lookup"><span data-stu-id="f0252-616">The **Delete Index** request must include an `api-key` header set to your admin key (as opposed to a query key).</span></span>

<span data-ttu-id="f0252-617">Konfigurálnia kell a szolgáltatás nevét, a kérelem URL-címe összeállításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-617">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="f0252-618">Beszerezheti a szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján, az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f0252-618">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="f0252-619">Lásd: [Azure Search szolgáltatás létrehozása a portálon](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="f0252-619">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="f0252-620">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="f0252-620">**Request Body**</span></span>

<span data-ttu-id="f0252-621">nincs.</span><span class="sxs-lookup"><span data-stu-id="f0252-621">None.</span></span>

<span data-ttu-id="f0252-622">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="f0252-622">**Response**</span></span>

<span data-ttu-id="f0252-623">Állapotkód: 204 nem tartalom visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="f0252-623">Status Code: 204 No Content is returned for a successful response.</span></span>

<a name="GetIndexStats"></a>

## <a name="get-index-statistics"></a><span data-ttu-id="f0252-624">Megtekintheti a statisztikákat Index</span><span class="sxs-lookup"><span data-stu-id="f0252-624">Get Index Statistics</span></span>
<span data-ttu-id="f0252-625">A **beolvasása Index statisztika** művelet számát adja vissza az Azure Search dokumentum az aktuális index, valamint a tárhely kihasználtsága.</span><span class="sxs-lookup"><span data-stu-id="f0252-625">The **Get Index Statistics** operation returns from Azure Search a document count for the current index, plus storage usage.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> <span data-ttu-id="f0252-626">A dokumentumok száma és a tárhely mérete statisztika nem a valós idejű néhány percenként összegyűjtése.</span><span class="sxs-lookup"><span data-stu-id="f0252-626">Statistics on document count and storage size are collected every few minutes, not in real time.</span></span> <span data-ttu-id="f0252-627">Ezért ez az API által visszaadott statisztikák nem tükrözik legutóbbi indexelési műveletek okozta módosítások.</span><span class="sxs-lookup"><span data-stu-id="f0252-627">Therefore, the statistics returned by this API may not reflect changes caused by recent indexing operations.</span></span>
> 
> 

<span data-ttu-id="f0252-628">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="f0252-628">**Request**</span></span>

<span data-ttu-id="f0252-629">HTTPS-kapcsolat szükséges szolgáltatások minden olyan kérelem esetében.</span><span class="sxs-lookup"><span data-stu-id="f0252-629">HTTPS is required for all services requests.</span></span> <span data-ttu-id="f0252-630">A **beolvasása Index statisztika** kérelem a GET metódussal lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f0252-630">The **Get Index Statistics** request can be constructed using the GET method.</span></span>

<span data-ttu-id="f0252-631">A kérelem URI azonosítója [index neve] közli az index statisztikáit. a megadott index vissza a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f0252-631">The [index name] in the request URI tells the service to return index statistics for the specified index.</span></span>

<span data-ttu-id="f0252-632">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="f0252-632">`api-version=[string]` (required).</span></span> <span data-ttu-id="f0252-633">Az előzetes verzió `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="f0252-633">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="f0252-634">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="f0252-634">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="f0252-635">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="f0252-635">**Request Headers**</span></span>

<span data-ttu-id="f0252-636">Az alábbi lista ismerteti a szükséges és választható kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="f0252-636">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="f0252-637">`api-key`: A `api-key` hitelesíteni a kérelmet a keresőszolgáltatása szolgál.</span><span class="sxs-lookup"><span data-stu-id="f0252-637">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="f0252-638">Egy karakterláncértéket, a szolgáltatás egyedi.</span><span class="sxs-lookup"><span data-stu-id="f0252-638">It is a string value, unique to your service.</span></span> <span data-ttu-id="f0252-639">A **beolvasása Index statisztika** kérelem tartalmaznia kell egy `api-key` egy adminisztrációs kulcsot (szemben a lekérdezési kulcs) értékre.</span><span class="sxs-lookup"><span data-stu-id="f0252-639">The **Get Index Statistics** request must include an `api-key` set to an admin key (as opposed to a query key).</span></span>

<span data-ttu-id="f0252-640">Konfigurálnia kell a szolgáltatás nevét, a kérelem URL-címe összeállításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-640">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="f0252-641">Beszerezheti a szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján, az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f0252-641">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="f0252-642">Lásd: [Azure Search szolgáltatás létrehozása a portálon](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="f0252-642">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="f0252-643">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="f0252-643">**Request Body**</span></span>

<span data-ttu-id="f0252-644">nincs.</span><span class="sxs-lookup"><span data-stu-id="f0252-644">None.</span></span>

<span data-ttu-id="f0252-645">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="f0252-645">**Response**</span></span>

<span data-ttu-id="f0252-646">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="f0252-646">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="f0252-647">A választörzs mérete a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="f0252-647">The response body is in the following format:</span></span>

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>

## <a name="test-analyzer"></a><span data-ttu-id="f0252-648">Teszt elemző eszköz</span><span class="sxs-lookup"><span data-stu-id="f0252-648">Test Analyzer</span></span>
<span data-ttu-id="f0252-649">A **elemzése API** bemutatja, hogyan egy analyzer bontja szöveg jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="f0252-649">The **Analyze API** shows how an analyzer breaks text into tokens.</span></span>

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="f0252-650">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="f0252-650">**Request**</span></span>

<span data-ttu-id="f0252-651">HTTPS-kapcsolat szükséges szolgáltatások minden olyan kérelem esetében.</span><span class="sxs-lookup"><span data-stu-id="f0252-651">HTTPS is required for all services requests.</span></span> <span data-ttu-id="f0252-652">A **elemzése API** kérelmek POST metódussal történő lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f0252-652">The **Analyze API** request can be constructed using the POST method.</span></span>

<span data-ttu-id="f0252-653">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="f0252-653">`api-version=[string]` (required).</span></span> <span data-ttu-id="f0252-654">Az előzetes verzió `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="f0252-654">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="f0252-655">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="f0252-655">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="f0252-656">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="f0252-656">**Request Headers**</span></span>

<span data-ttu-id="f0252-657">Az alábbi lista ismerteti a szükséges és választható kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="f0252-657">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="f0252-658">`api-key`: A `api-key` hitelesíteni a kérelmet a keresőszolgáltatása szolgál.</span><span class="sxs-lookup"><span data-stu-id="f0252-658">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="f0252-659">Egy karakterláncértéket, a szolgáltatás egyedi.</span><span class="sxs-lookup"><span data-stu-id="f0252-659">It is a string value, unique to your service.</span></span> <span data-ttu-id="f0252-660">A **elemzése API** kérelem tartalmaznia kell egy `api-key` egy adminisztrációs kulcsot (szemben a lekérdezési kulcs) értékre.</span><span class="sxs-lookup"><span data-stu-id="f0252-660">The **Analyze API** request must include an `api-key` set to an admin key (as opposed to a query key).</span></span>

<span data-ttu-id="f0252-661">Konfigurálnia kell az index neve és a szolgáltatás nevét, a kérelem URL-címe összeállításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-661">You will also need the index name and the service name to construct the request URL.</span></span> <span data-ttu-id="f0252-662">Beszerezheti a szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján, az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f0252-662">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="f0252-663">Lásd: [Azure Search szolgáltatás létrehozása a portálon](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="f0252-663">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="f0252-664">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="f0252-664">**Request Body**</span></span>

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

<span data-ttu-id="f0252-665">vagy</span><span class="sxs-lookup"><span data-stu-id="f0252-665">or</span></span>

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

<span data-ttu-id="f0252-666">A `analyzer_name`, `tokenizer_name`, `token_filter_name` és `char_filter_name` kell lenniük az előre definiált vagy egyéni lekérdezések, tokenizers, lexikális elem és az index char szűrőket érvényes nevét.</span><span class="sxs-lookup"><span data-stu-id="f0252-666">The `analyzer_name`, `tokenizer_name`, `token_filter_name` and `char_filter_name` need to be valid names of predefined or custom analyzers, tokenizers, token filters and char filters for the index.</span></span> <span data-ttu-id="f0252-667">Ismerje meg, a folyamat lexikális elemzési kapcsolatos információkért tekintse meg a [elemzése az Azure Search](https://aka.ms/azsanalysis).</span><span class="sxs-lookup"><span data-stu-id="f0252-667">To learn more about the process of lexical analysis see [Analysis in Azure Search](https://aka.ms/azsanalysis).</span></span>

<span data-ttu-id="f0252-668">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="f0252-668">**Response**</span></span>

<span data-ttu-id="f0252-669">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="f0252-669">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="f0252-670">A választörzs mérete a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="f0252-670">The response body is in the following format:</span></span>

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

<span data-ttu-id="f0252-671">**Elemezze API – példa**</span><span class="sxs-lookup"><span data-stu-id="f0252-671">**Analyze API example**</span></span>

<span data-ttu-id="f0252-672">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="f0252-672">**Request**</span></span>

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

<span data-ttu-id="f0252-673">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="f0252-673">**Response**</span></span>

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

## <a name="document-operations"></a><span data-ttu-id="f0252-674">A dokumentum műveletek</span><span class="sxs-lookup"><span data-stu-id="f0252-674">Document Operations</span></span>
<span data-ttu-id="f0252-675">Az Azure Search index a felhőben tárolt, és fel a szolgáltatás feltöltött JSON-dokumentumok segítségével.</span><span class="sxs-lookup"><span data-stu-id="f0252-675">In Azure Search, an index is stored in the cloud and populated using JSON documents that you upload to the service.</span></span> <span data-ttu-id="f0252-676">Feltöltött összes dokumentumot a corpus keresési adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f0252-676">All the documents that you upload comprise the corpus of your search data.</span></span> <span data-ttu-id="f0252-677">Dokumentumok, amelyeket a keresési feltételek vannak tokenekre, feltölti, a mezők tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="f0252-677">Documents contain fields, some of which are tokenized into search terms as they are uploaded.</span></span> <span data-ttu-id="f0252-678">A `/docs` URL-szegmenseket, az Azure Search API a dokumentumok index gyűjteményét képviseli.</span><span class="sxs-lookup"><span data-stu-id="f0252-678">The `/docs` URL segment in the Azure Search API represents the collection of documents in an index.</span></span> <span data-ttu-id="f0252-679">Összes művelet végrehajtását, például feltöltése a gyűjteményt, az egyesítés, törlését és lekérdezését dokumentumok hajtsa végre a megfelelő helyezze keretén belül egyetlen index, ezért az URL-címek az e műveletek mindig indul rendelkező `/indexes/[index name]/docs` adott index neve.</span><span class="sxs-lookup"><span data-stu-id="f0252-679">All operations performed on the collection such as uploading, merging, deleting, or querying documents take place in the context of a single index, so the URLs for these operations will always start with `/indexes/[index name]/docs` for a given index name.</span></span>

<span data-ttu-id="f0252-680">Az alkalmazás kódjában vagy létre kell hoznia JSON-dokumentumokat az Azure Search feltölteni, vagy használhat egy [indexelő](https://msdn.microsoft.com/library/dn946891.aspx) dokumentumok betölteni, ha az adatforrás vagy az Azure SQL Database, vagy az Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f0252-680">Your application code must either generate JSON documents to upload to Azure Search or you can use an [indexer](https://msdn.microsoft.com/library/dn946891.aspx) to load documents if the data source is either Azure SQL Database or Azure Cosmos DB.</span></span> <span data-ttu-id="f0252-681">Indexek általában fel van töltve, egy Ön által biztosított egyetlen adatkészletből.</span><span class="sxs-lookup"><span data-stu-id="f0252-681">Typically, indexes are populated from a single dataset that you provide.</span></span>

<span data-ttu-id="f0252-682">Meg kell terveznie, amely egy dokumentumot, amelyet meg szeretne keresni minden elemhez.</span><span class="sxs-lookup"><span data-stu-id="f0252-682">You should plan on having one document for each item that you want to search.</span></span> <span data-ttu-id="f0252-683">Egy movie bérleti alkalmazás rendelkezhet movie egy dokumentumot, egy kirakat alkalmazás rendelkezhet SKU egy dokumentumot, online anyagok alkalmazás rendelkezhet működés során egy dokumentumot, a kutatási vállalkozás előfordulhat, hogy az egyes academic papír egy dokumentumot a tárház, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="f0252-683">A movie rental application might have one document per movie, a storefront application might have one document per SKU, an online courseware application might have one document per course, a research firm might have one document for each academic paper in their repository, and so on.</span></span>

<span data-ttu-id="f0252-684">Dokumentumok egy vagy több mező áll.</span><span class="sxs-lookup"><span data-stu-id="f0252-684">Documents consist of one or more fields.</span></span> <span data-ttu-id="f0252-685">Mezők szöveg, amelyet az Azure Search által a keresési feltételek tokenekre bontott, valamint nem tokenekre vagy szűrők, vagy a pontozási profil használható nem szöveges értékeket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="f0252-685">Fields can contain text that is tokenized by Azure Search into search terms, as well as non-tokenized or non-text values that can be used in filters or scoring profiles.</span></span> <span data-ttu-id="f0252-686">A név, az adattípusokat és az egyes mezők támogatott keresési funkciók a sémát indexeli határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f0252-686">The names, data types, and search features supported for each field are determined by the index schema.</span></span> <span data-ttu-id="f0252-687">Egy azonosító kijelölt minden a sémát indexeli a mezők egyike, és minden egyes dokumentum esetében az Azonosítót tartalmazó mezőt, amely egyedileg azonosítja az indexben a dokumentum értékűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f0252-687">One of the fields in each index schema must be designated as an ID, and each document must have a value for the ID field that uniquely identifies that document in the index.</span></span> <span data-ttu-id="f0252-688">A dokumentum többi mező megadása nem kötelező, és a rendszer alapértelmezés szerint null értékű, ha nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="f0252-688">All other document fields are optional and will default to a null value if left unspecified.</span></span> <span data-ttu-id="f0252-689">Vegye figyelembe, hogy null értékek nem helyet foglalnak a keresési index.</span><span class="sxs-lookup"><span data-stu-id="f0252-689">Note that null values do not take up space in the search index.</span></span>

<span data-ttu-id="f0252-690">Feltöltheti a dokumentumok, mielőtt kell már létrehozott az index a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="f0252-690">Before you can upload documents, you must have already created the index on the service.</span></span> <span data-ttu-id="f0252-691">Lásd: [a Create Index](#CreateIndex) az első lépéshez vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="f0252-691">See [Create Index](#CreateIndex) for details about this first step.</span></span>

<a name="AddOrUpdateDocuments"></a>

## <a name="add-update-or-delete-documents"></a><span data-ttu-id="f0252-692">Adja hozzá, frissítés, vagy dokumentumok törlése</span><span class="sxs-lookup"><span data-stu-id="f0252-692">Add, Update, or Delete Documents</span></span>
<span data-ttu-id="f0252-693">Feltöltheti, egyesítési, az egyesítés vagy feltöltés és a delete dokumentumok egy megadott indextől HTTP POST használatával.</span><span class="sxs-lookup"><span data-stu-id="f0252-693">You can upload, merge, merge-or-upload or delete documents from a specified index using HTTP POST.</span></span> <span data-ttu-id="f0252-694">Nagyszámú a frissítések kötegelése (1000 dokumentumok kötegenként) vagy kötegenként körülbelül 16 MB dokumentumait ajánlott.</span><span class="sxs-lookup"><span data-stu-id="f0252-694">For large numbers of updates, batching of documents (up to 1000 documents per batch or about 16 MB per batch) is recommended.</span></span>

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

<span data-ttu-id="f0252-695">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="f0252-695">**Request**</span></span>

<span data-ttu-id="f0252-696">HTTPS-kapcsolat szükséges az összes szolgáltatási kérelmek.</span><span class="sxs-lookup"><span data-stu-id="f0252-696">HTTPS is required for all service requests.</span></span> <span data-ttu-id="f0252-697">Feltöltheti, egyesítési, az egyesítés vagy feltöltés és a delete dokumentumok egy megadott indextől HTTP POST használatával.</span><span class="sxs-lookup"><span data-stu-id="f0252-697">You can upload, merge, merge-or-upload or delete documents from a specified index using HTTP POST.</span></span>

<span data-ttu-id="f0252-698">A kérelem URI-azonosítója tartalmaz [index neve], melyik index dokumentumok küldésére szolgáló megadása.</span><span class="sxs-lookup"><span data-stu-id="f0252-698">The request URI includes [index name], specifying which index to post documents.</span></span> <span data-ttu-id="f0252-699">Egyszerre csak egy index a dokumentumokat is közzétesz.</span><span class="sxs-lookup"><span data-stu-id="f0252-699">You can only post documents to one index at a time.</span></span>

<span data-ttu-id="f0252-700">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="f0252-700">`api-version=[string]` (required).</span></span> <span data-ttu-id="f0252-701">Az előzetes verzió `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="f0252-701">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="f0252-702">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="f0252-702">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="f0252-703">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="f0252-703">**Request Headers**</span></span>

<span data-ttu-id="f0252-704">Az alábbi lista ismerteti a szükséges és választható kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="f0252-704">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="f0252-705">`Content-Type`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f0252-705">`Content-Type`: Required.</span></span> <span data-ttu-id="f0252-706">Állítsa ezt a beállítást`application/json`</span><span class="sxs-lookup"><span data-stu-id="f0252-706">Set this to `application/json`</span></span>
* <span data-ttu-id="f0252-707">`api-key`: Kötelező.</span><span class="sxs-lookup"><span data-stu-id="f0252-707">`api-key`: Required.</span></span> <span data-ttu-id="f0252-708">A `api-key` hitelesíteni a kérelmet a keresőszolgáltatása szolgál.</span><span class="sxs-lookup"><span data-stu-id="f0252-708">The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="f0252-709">Egy karakterláncértéket, a szolgáltatás egyedi.</span><span class="sxs-lookup"><span data-stu-id="f0252-709">It is a string value, unique to your service.</span></span> <span data-ttu-id="f0252-710">A **hozzáadása dokumentumok** kérelem tartalmaznia kell egy `api-key` fejlécben az adminisztrációs kulcsot (nem egy lekérdezési kulcsot).</span><span class="sxs-lookup"><span data-stu-id="f0252-710">The **Add Documents** request must include an `api-key` header set to your admin key (as opposed to a query key).</span></span>

<span data-ttu-id="f0252-711">Konfigurálnia kell a szolgáltatás nevét, a kérelem URL-címe összeállításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-711">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="f0252-712">Beszerezheti a szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján, az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f0252-712">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="f0252-713">Lásd: [Azure Search szolgáltatás létrehozása a portálon](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="f0252-713">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="f0252-714">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="f0252-714">**Request Body**</span></span>

<span data-ttu-id="f0252-715">A kérelem törzsében indexelése egy vagy több dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="f0252-715">The body of the request contains one or more documents to be indexed.</span></span> <span data-ttu-id="f0252-716">Dokumentumok egyedi kulcs azonosítják.</span><span class="sxs-lookup"><span data-stu-id="f0252-716">Documents are identified by a unique key.</span></span> <span data-ttu-id="f0252-717">Minden egyes dokumentum rendelve egy műveletet: feltöltés, egyesítési, mergeOrUpload vagy törlése.</span><span class="sxs-lookup"><span data-stu-id="f0252-717">Each document is associated with an action: upload, merge, mergeOrUpload or delete.</span></span> <span data-ttu-id="f0252-718">Feltöltési kérést kulcs/érték párok készleteként tartalmaznia kell a dokumentum adatokat.</span><span class="sxs-lookup"><span data-stu-id="f0252-718">Upload requests must include the document data as a set of key/value pairs.</span></span>

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
> <span data-ttu-id="f0252-719">A dokumentum kulcsok csak betűket, számokat, kötőjeleket tartalmazhat ("-"), aláhúzásjelet ("_"), és egyenlőségjelet ("=").</span><span class="sxs-lookup"><span data-stu-id="f0252-719">Document keys can only contain letters, numbers, dashes ("-"), underscores ("_"), and equal signs ("=").</span></span> <span data-ttu-id="f0252-720">További részletekért lásd: [elnevezési szabályait](https://msdn.microsoft.com/library/azure/dn857353.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0252-720">For more details, see [Naming rules](https://msdn.microsoft.com/library/azure/dn857353.aspx).</span></span>
> 
> 

<span data-ttu-id="f0252-721">**A dokumentum műveletek**</span><span class="sxs-lookup"><span data-stu-id="f0252-721">**Document Actions**</span></span>

* <span data-ttu-id="f0252-722">`upload`: Egy feltöltési művelet hasonlít "upsert" Ha a dokumentum program beszúrja, ha új és frissített/lecserélik akkor, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="f0252-722">`upload`: An upload action is similar to an "upsert" where the document will be inserted if it is new and updated/replaced if it exists.</span></span> <span data-ttu-id="f0252-723">Vegye figyelembe, hogy a frissítés esetben minden mező helyett.</span><span class="sxs-lookup"><span data-stu-id="f0252-723">Note that all fields are replaced in the update case.</span></span>
* <span data-ttu-id="f0252-724">`merge`: Egyesítés egy meglévő dokumentum frissíti a megadott mezőket.</span><span class="sxs-lookup"><span data-stu-id="f0252-724">`merge`: Merge updates an existing document with the specified fields.</span></span> <span data-ttu-id="f0252-725">Ha a dokumentum nem létezik, az egyesítés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="f0252-725">If the document doesn't exist, the merge will fail.</span></span> <span data-ttu-id="f0252-726">A rendszer az egyesítési művelet során megadott mezőkre cseréli a dokumentum meglévő mezőit.</span><span class="sxs-lookup"><span data-stu-id="f0252-726">Any field you specify in a merge will replace the existing field in the document.</span></span> <span data-ttu-id="f0252-727">Ez `Collection(Edm.String)` típusú mezőket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f0252-727">This includes fields of type `Collection(Edm.String)`.</span></span> <span data-ttu-id="f0252-728">Például, ha a dokumentum tartalmaz a mező "címkék" értékű `["budget"]` és értékkel egyesítésével végrehajtása `["economy", "pool"]` "címkék", a végső "címkék" mező értéke lesz `["economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="f0252-728">For example, if the document contains a field "tags" with value `["budget"]` and you execute a merge with value `["economy", "pool"]` for "tags", the final value of the "tags" field will be `["economy", "pool"]`.</span></span> <span data-ttu-id="f0252-729">A következőket hajtja végre **nem** kell `["budget", "economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="f0252-729">It will **not** be `["budget", "economy", "pool"]`.</span></span>
* <span data-ttu-id="f0252-730">`mergeOrUpload`: viselkedik `merge` Ha egy dokumentumot a megadott kulcs már létezik az index.</span><span class="sxs-lookup"><span data-stu-id="f0252-730">`mergeOrUpload`: behaves like `merge` if a document with the given key already exists in the index.</span></span> <span data-ttu-id="f0252-731">Ha a dokumentum nem létezik, hasonlóan viselkedik `upload` egy új dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="f0252-731">If the document does not exist it behaves like `upload` with a new document.</span></span>
* <span data-ttu-id="f0252-732">`delete`: Törlés a megadott dokumentum eltávolítása az index.</span><span class="sxs-lookup"><span data-stu-id="f0252-732">`delete`: Delete removes the specified document from the index.</span></span> <span data-ttu-id="f0252-733">Vegye figyelembe, hogy minden mezőt megadja egy `delete` művelet a következő kulcsmező nem lesz figyelembe véve.</span><span class="sxs-lookup"><span data-stu-id="f0252-733">Note that any fields you specify in a `delete` operation other than the key field will be ignored.</span></span> <span data-ttu-id="f0252-734">Ha szeretne egy egyéni mező eltávolítása egy dokumentumot, `merge` helyette és egyszerűen állítsa be az a mező explicit módon a `null`.</span><span class="sxs-lookup"><span data-stu-id="f0252-734">If you want to remove an individual field from a document, use `merge` instead and simply set the field explicitly to `null`.</span></span>

<span data-ttu-id="f0252-735">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="f0252-735">**Response**</span></span>

<span data-ttu-id="f0252-736">Állapotkód 200 (OK) visszaküldött a sikeres válasz, ami azt jelenti, hogy minden elem sikeresen indexelt.</span><span class="sxs-lookup"><span data-stu-id="f0252-736">Status code 200 (OK) is returned for a successful response, meaning that all items have been successfully indexed.</span></span> <span data-ttu-id="f0252-737">Ezt a `status` tulajdonság beállítása true, az összes, valamint a, a `statusCode` tulajdonság beállítása (az újonnan feltöltött dokumentumok) 201-et vagy a 200-as (az egyesített vagy törölt dokumentumok):</span><span class="sxs-lookup"><span data-stu-id="f0252-737">This is indicated by the `status` property being set to true for all items, as well as the `statusCode` property being set to either 201 (for newly uploaded documents) or 200 (for merged or deleted documents):</span></span>

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

<span data-ttu-id="f0252-738">Legalább egy tételt indexelése nem sikerült állapotkód 207 (több állapot) érték érkezett vissza.</span><span class="sxs-lookup"><span data-stu-id="f0252-738">Status code 207 (Multi-Status) is returned when at least one item was not successfully indexed.</span></span> <span data-ttu-id="f0252-739">Elemek nem indexelt rendelkezik a `status` mező értéke hamis.</span><span class="sxs-lookup"><span data-stu-id="f0252-739">Items that have not been indexed have the `status` field set to false.</span></span> <span data-ttu-id="f0252-740">A `errorMessage` és `statusCode` tulajdonságok jelzi az indexelési hiba oka:</span><span class="sxs-lookup"><span data-stu-id="f0252-740">The `errorMessage` and `statusCode` properties will indicate the reason for the indexing error:</span></span>

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
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
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

<span data-ttu-id="f0252-741">Az alábbi táblázat ismerteti a különböző dokumentumra állapotkódokat adhatók vissza a válaszban.</span><span class="sxs-lookup"><span data-stu-id="f0252-741">The following table explains the various per-document status codes that can be returned in the response.</span></span> <span data-ttu-id="f0252-742">Vegye figyelembe, hogy néhány utalnak a kérelmet, míg mások jelzi, hogy ideiglenes hibaállapotok.</span><span class="sxs-lookup"><span data-stu-id="f0252-742">Note that some indicate problems with the request itself, while others indicate temporary error conditions.</span></span> <span data-ttu-id="f0252-743">Az utóbbi várakozás után érdemes újra megpróbálnia.</span><span class="sxs-lookup"><span data-stu-id="f0252-743">The latter you should retry after a delay.</span></span>

<table style="font-size:12">
    <tr>
        <th><span data-ttu-id="f0252-744">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="f0252-744">Status code</span></span></th>
        <th><span data-ttu-id="f0252-745">Jelentése</span><span class="sxs-lookup"><span data-stu-id="f0252-745">Meaning</span></span></th>
        <th><span data-ttu-id="f0252-746">Újrapróbálkozást lehetővé tevő</span><span class="sxs-lookup"><span data-stu-id="f0252-746">Retryable</span></span></th>
        <th><span data-ttu-id="f0252-747">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f0252-747">Notes</span></span></th>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-748">200</span><span class="sxs-lookup"><span data-stu-id="f0252-748">200</span></span></td>
        <td><span data-ttu-id="f0252-749">A dokumentum sikeresen módosították vagy törölték.</span><span class="sxs-lookup"><span data-stu-id="f0252-749">Document was successfully modified or deleted.</span></span></td>
        <td><span data-ttu-id="f0252-750">n/a</span><span class="sxs-lookup"><span data-stu-id="f0252-750">n/a</span></span></td>
        <td><span data-ttu-id="f0252-751">Törlési műveletek vannak <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span><span class="sxs-lookup"><span data-stu-id="f0252-751">Delete operations are <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span></span> <span data-ttu-id="f0252-752">Ez azt jelenti, hogy akkor is, ha egy dokumentum kulcs nem létezik az indexben, ezzel a kulccsal a delete művelet végrehajtásának kísérlete eredményez 200 állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="f0252-752">That is, even if a document key does not exist in the index, attempting a delete operation with that key will result in a 200 status code.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-753">201</span><span class="sxs-lookup"><span data-stu-id="f0252-753">201</span></span></td>
        <td><span data-ttu-id="f0252-754">A dokumentum létrehozása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="f0252-754">Document was successfully created.</span></span></td>
        <td><span data-ttu-id="f0252-755">n/a</span><span class="sxs-lookup"><span data-stu-id="f0252-755">n/a</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-756">400</span><span class="sxs-lookup"><span data-stu-id="f0252-756">400</span></span></td>
        <td><span data-ttu-id="f0252-757">Hiba történt a dokumentum, amely megakadályozta a indexelt.</span><span class="sxs-lookup"><span data-stu-id="f0252-757">There was an error in the document that prevented it from being indexed.</span></span></td>
        <td><span data-ttu-id="f0252-758">Nem</span><span class="sxs-lookup"><span data-stu-id="f0252-758">No</span></span></td>
        <td><span data-ttu-id="f0252-759">A hibaüzenet a válaszban szereplő jelenik meg, miért nem megfelelő, a dokumentumon.</span><span class="sxs-lookup"><span data-stu-id="f0252-759">The error message in the response will indicate what is wrong with the document.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-760">404</span><span class="sxs-lookup"><span data-stu-id="f0252-760">404</span></span></td>
        <td><span data-ttu-id="f0252-761">A dokumentum nem egyesíthetők, mert a megadott kulcs nem létezik az index.</span><span class="sxs-lookup"><span data-stu-id="f0252-761">The document could not be merged because the given key doesn't exist in the index.</span></span></td>
        <td><span data-ttu-id="f0252-762">Nem</span><span class="sxs-lookup"><span data-stu-id="f0252-762">No</span></span></td>
        <td><span data-ttu-id="f0252-763">Ez a hiba nem történik meg a feltöltések óta hoznak létre új dokumentumokat, és ez nem történik meg a törlések ezek ugyanis <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span><span class="sxs-lookup"><span data-stu-id="f0252-763">This error does not occur for uploads since they create new documents, and it does not occur for deletes because they are <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-764">409</span><span class="sxs-lookup"><span data-stu-id="f0252-764">409</span></span></td>
        <td><span data-ttu-id="f0252-765">Verzióütközés volt észlelhető, a dokumentum index.</span><span class="sxs-lookup"><span data-stu-id="f0252-765">A version conflict was detected when attempting to index a document.</span></span></td>
        <td><span data-ttu-id="f0252-766">Igen</span><span class="sxs-lookup"><span data-stu-id="f0252-766">Yes</span></span></td>
        <td><span data-ttu-id="f0252-767">Ez akkor fordulhat elő, index egyszerre egynél többször egyazon dokumentumban való regisztráláskor.</span><span class="sxs-lookup"><span data-stu-id="f0252-767">This can happen when you're trying to index the same document more than once concurrently.</span></span></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-768">422</span><span class="sxs-lookup"><span data-stu-id="f0252-768">422</span></span></td>
        <td><span data-ttu-id="f0252-769">Az index átmenetileg nem érhető el, mert a "allowIndexDowntime" jelzőjének beállítása "igaz" a frissítés.</span><span class="sxs-lookup"><span data-stu-id="f0252-769">The index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'.</span></span></td>
        <td><span data-ttu-id="f0252-770">Igen</span><span class="sxs-lookup"><span data-stu-id="f0252-770">Yes</span></span></td>
        <td></td>
    </tr>
    <tr>
        <td><span data-ttu-id="f0252-771">503</span><span class="sxs-lookup"><span data-stu-id="f0252-771">503</span></span></td>
        <td><span data-ttu-id="f0252-772">A keresési szolgáltatás átmenetileg nem érhető el, valószínűleg a nagy terhelésnek.</span><span class="sxs-lookup"><span data-stu-id="f0252-772">Your search service is temporarily unavailable, possibly due to heavy load.</span></span></td>
        <td><span data-ttu-id="f0252-773">Igen</span><span class="sxs-lookup"><span data-stu-id="f0252-773">Yes</span></span></td>
        <td><span data-ttu-id="f0252-774">A kód várnia kell, mielőtt megpróbálná ebben az esetben, vagy azzal kockáztatja a szolgáltatás elérhetetlensége meghosszabbítva.</span><span class="sxs-lookup"><span data-stu-id="f0252-774">Your code should wait before retrying in this case or you risk prolonging the service unavailability.</span></span></td>
    </tr>
</table> 

<span data-ttu-id="f0252-775">**Megjegyzés:**: az Ügyfélkód gyakran észlel a 207 választ, ha egy lehetséges oka, hogy a rendszer terhelésnek van kitéve.</span><span class="sxs-lookup"><span data-stu-id="f0252-775">**Note**: If your client code frequently encounters a 207 response, one possible reason is that the system is under load.</span></span> <span data-ttu-id="f0252-776">Ez alapján ellenőrizheti a `statusCode` 503-as tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="f0252-776">You can confirm this by checking the `statusCode` property for 503.</span></span> <span data-ttu-id="f0252-777">Ha ez a helyzet, ajánlott ***szabályozás indexelési***.</span><span class="sxs-lookup"><span data-stu-id="f0252-777">If this is the case, we recommend ***throttling indexing requests***.</span></span> <span data-ttu-id="f0252-778">Ellenkező esetben forgalom indexelő nem subside, ha a rendszer elindítása volt elutasító összes kérelmek 503-as hiba.</span><span class="sxs-lookup"><span data-stu-id="f0252-778">Otherwise, if indexing traffic doesn't subside, the system could start rejecting all requests with 503 errors.</span></span>

<span data-ttu-id="f0252-779">429. állapotkód: azt jelzi, hogy túllépte a kvótát a dokumentumok index másodpercenkénti száma.</span><span class="sxs-lookup"><span data-stu-id="f0252-779">Status code 429 indicates that you have exceeded your quota on the number of documents per index.</span></span> <span data-ttu-id="f0252-780">Hozzon létre egy új indexet vagy magasabb kapacitáskorlátait frissítése.</span><span class="sxs-lookup"><span data-stu-id="f0252-780">You must either create a new index or upgrade for higher capacity limits.</span></span>

<span data-ttu-id="f0252-781">**Példa**</span><span class="sxs-lookup"><span data-stu-id="f0252-781">**Example:**</span></span>

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

## <a name="search-documents"></a><span data-ttu-id="f0252-782">Dokumentumok keresése</span><span class="sxs-lookup"><span data-stu-id="f0252-782">Search Documents</span></span>
<span data-ttu-id="f0252-783">A **keresési** művelet GET vagy POST-kérelmet ad ki, és meghatározza, hogy adjon a feltételeknek megfelelő dokumentumok kiválasztásának paramétereket.</span><span class="sxs-lookup"><span data-stu-id="f0252-783">A **Search** operation is issued as a GET or POST request and specifies parameters that give the criteria for selecting matching documents.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

<span data-ttu-id="f0252-784">**Mikor érdemes használni a FELADÁS egy vagy több GET helyett**</span><span class="sxs-lookup"><span data-stu-id="f0252-784">**When to use POST instead of GET**</span></span>

<span data-ttu-id="f0252-785">Ha használ HTTP GET hívja a **keresési** API-t kell vegye figyelembe, hogy a kérelem URL-címe nem lehet hosszabb 8 KB-os.</span><span class="sxs-lookup"><span data-stu-id="f0252-785">When you use HTTP GET to call the **Search** API, you need to be aware that the length of the request URL cannot exceed 8 KB.</span></span> <span data-ttu-id="f0252-786">Ez elég általában a legtöbb alkalmazás esetén.</span><span class="sxs-lookup"><span data-stu-id="f0252-786">This is usually enough for most applications.</span></span> <span data-ttu-id="f0252-787">Egyes alkalmazások azonban nagyon nagy lekérdezések vagy OData szűrőkifejezéseket eredményez.</span><span class="sxs-lookup"><span data-stu-id="f0252-787">However, some applications produce very large queries or OData filter expressions.</span></span> <span data-ttu-id="f0252-788">Ezek az alkalmazások a HTTP POST jobb választás, mivel használata esetén lehetővé teszi a nagyobb szűrőket és lekérdezéseket GET-nál.</span><span class="sxs-lookup"><span data-stu-id="f0252-788">For these applications, using HTTP POST is a better choice because it allows larger filters and queries than GET.</span></span> <span data-ttu-id="f0252-789">A FELADÁS egy vagy több a feltételek vagy a lekérdezésben záradékok a korlátozás, nem a nyers lekérdezéssel, mivel a POST kérelem méretkorlátja nagyjából 16 MB méretét.</span><span class="sxs-lookup"><span data-stu-id="f0252-789">With POST, the number of terms or clauses in a query is the limiting factor, not the size of the raw query since the request size limit for POST is approximately 16 MB.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-790">Annak ellenére, hogy a POST kérelem méretkorlátot nagyon nagy, keresési lekérdezések és szűrőkifejezéseket önkényesen összetett nem lehet.</span><span class="sxs-lookup"><span data-stu-id="f0252-790">Even though the POST request size limit is very large, search queries and filter expressions cannot be arbitrarily complex.</span></span> <span data-ttu-id="f0252-791">Lásd: [Lucene lekérdezés szintaxisát](https://msdn.microsoft.com/library/mt589323.aspx) és [OData-kifejezésszintaxist](https://msdn.microsoft.com/library/dn798921.aspx) keresési lekérdezés és a szűrő összetettsége korlátozásaival kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="f0252-791">See [Lucene query syntax](https://msdn.microsoft.com/library/mt589323.aspx) and [OData expression syntax](https://msdn.microsoft.com/library/dn798921.aspx) for more information about search query and filter complexity limitations.</span></span>
> 
> 

<span data-ttu-id="f0252-792">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="f0252-792">**Request**</span></span>

<span data-ttu-id="f0252-793">HTTPS szolgáltatáskérések szükség.</span><span class="sxs-lookup"><span data-stu-id="f0252-793">HTTPS is required for service requests.</span></span> <span data-ttu-id="f0252-794">A **keresési** kérelem a GET vagy POST metódusok használatával lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f0252-794">The **Search** request can be constructed using the GET or POST methods.</span></span>

<span data-ttu-id="f0252-795">A kérelem URI-azonosítója a lekérdezéshez, minden dokumentumhoz, amelyek megfelelnek a paraméterek melyik index határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f0252-795">The request URI specifies which index to query, for all documents that match the parameters.</span></span> <span data-ttu-id="f0252-796">Paraméterek vannak megadva, a GET-kérések esetén a lekérdezési karakterláncot, és a kérés törzsében POST esetén kéri.</span><span class="sxs-lookup"><span data-stu-id="f0252-796">Parameters are specified on the query string in the case of GET requests, and in the request body in the case of POST requests.</span></span>

<span data-ttu-id="f0252-797">Ajánlott eljárásként GET kérelmek létrehozásakor, ne felejtse el [URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) meghatározott lekérdezési paraméterek közvetlenül a REST API hívásakor.</span><span class="sxs-lookup"><span data-stu-id="f0252-797">As a best practice when creating GET requests, remember to [URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specific query parameters when calling the REST API directly.</span></span> <span data-ttu-id="f0252-798">A **keresési** műveleteket, ilyenek:</span><span class="sxs-lookup"><span data-stu-id="f0252-798">For **Search** operations, this includes:</span></span>

* `$filter`
* `facet`
* `highlightPreTag`
* `highlightPostTag`
* `search`
* `moreLikeThis`

<span data-ttu-id="f0252-799">A fenti lekérdezési paraméterek csak javasolt, URL-kódolást.</span><span class="sxs-lookup"><span data-stu-id="f0252-799">URL encoding is only recommended on the above query parameters.</span></span> <span data-ttu-id="f0252-800">Ha véletlenül URL-kódolására, a teljes lekérdezési karakterlánc (mindent után a?), kérelmek megszakad.</span><span class="sxs-lookup"><span data-stu-id="f0252-800">If you inadvertently URL-encode the entire query string (everything after the ?), requests will break.</span></span>

<span data-ttu-id="f0252-801">Emellett URL-kódolást csak akkor szükség a REST API használatával közvetlenül a GET hívásakor.</span><span class="sxs-lookup"><span data-stu-id="f0252-801">Also, URL encoding is only necessary when calling the REST API directly using GET.</span></span> <span data-ttu-id="f0252-802">Kódolás nélkül URL-cím szükség, ha hívása **keresési** használatával a FELADÁS egy vagy több, vagy használatakor a [.NET ügyféloldali kódtár](https://msdn.microsoft.com/library/dn951165.aspx), amely alkalmazás URL-címet meg.</span><span class="sxs-lookup"><span data-stu-id="f0252-802">No URL encoding is necessary when calling **Search** using POST, or when using the [.NET client library](https://msdn.microsoft.com/library/dn951165.aspx), which handles URL encoding for you.</span></span>

<span data-ttu-id="f0252-803"><a name="SearchQueryParameters"></a>
**Lekérdezés-paraméterek**</span><span class="sxs-lookup"><span data-stu-id="f0252-803"><a name="SearchQueryParameters"></a>
**Query Parameters**</span></span>

<span data-ttu-id="f0252-804">**Keresési** lekérdezés feltételeket, és megadhatja a keresés viselkedését több paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="f0252-804">**Search** accepts several parameters that provide query criteria and also specify search behavior.</span></span> <span data-ttu-id="f0252-805">Megadja, hogy ezek a paraméterek az URL-cím lekérdezési karakterlánc meghívásakor **keresési** GET keresztül, és a kérés törzsében meghívásakor JSON tulajdonságként **keresési** POST keresztül.</span><span class="sxs-lookup"><span data-stu-id="f0252-805">You provide these parameters in the URL query string when calling **Search** via GET, and as JSON properties in the request body when calling **Search** via POST.</span></span> <span data-ttu-id="f0252-806">Egyes paraméterek szintaxisa a következő némileg eltérő GET és POST között.</span><span class="sxs-lookup"><span data-stu-id="f0252-806">The syntax for some parameters is slightly different between GET and POST.</span></span> <span data-ttu-id="f0252-807">Ezek a különbségek jeleztük megfelelő alatt:</span><span class="sxs-lookup"><span data-stu-id="f0252-807">These differences are noted as applicable below:</span></span>

<span data-ttu-id="f0252-808">`search=[string]`(nem kötelező) – a keresendő szöveget.</span><span class="sxs-lookup"><span data-stu-id="f0252-808">`search=[string]` (optional) - The text to search for.</span></span> <span data-ttu-id="f0252-809">Minden `searchable` mezőben keresni alapértelmezés szerint, kivéve, ha `searchFields` van megadva.</span><span class="sxs-lookup"><span data-stu-id="f0252-809">All `searchable` fields are searched by default unless `searchFields` is specified.</span></span> <span data-ttu-id="f0252-810">Ha a Keresés `searchable` mezők, a keresési szöveget magát tokenekre, így több feltételek szóközt kell elválasztani (például: `search=hello world`).</span><span class="sxs-lookup"><span data-stu-id="f0252-810">When searching `searchable` fields, the search text itself is tokenized, so multiple terms can be separated by white space (for example: `search=hello world`).</span></span> <span data-ttu-id="f0252-811">Bármely feltételek egyeznek, használja a `*` (Ez lehet hasznos, ha logikai szűrőlekérdezések).</span><span class="sxs-lookup"><span data-stu-id="f0252-811">To match any term, use `*` (this can be useful for boolean filter queries).</span></span> <span data-ttu-id="f0252-812">A paraméter kihagyása esetében ugyanaz az eredmény értékre állítaná, `*`.</span><span class="sxs-lookup"><span data-stu-id="f0252-812">Omitting this parameter has the same effect as setting it to `*`.</span></span> <span data-ttu-id="f0252-813">Lásd: [egyszerű lekérdezés szintaxisát](https://msdn.microsoft.com/library/dn798920.aspx) a a keresési szintaxist a részletekért.</span><span class="sxs-lookup"><span data-stu-id="f0252-813">See [Simple Query Syntax](https://msdn.microsoft.com/library/dn798920.aspx) for specifics on the search syntax.</span></span>

* <span data-ttu-id="f0252-814">**Megjegyzés:**: az eredmények néha lehet a meglepő lekérdezését `searchable` mezőket.</span><span class="sxs-lookup"><span data-stu-id="f0252-814">**Note**: The results can sometimes be surprising when querying over `searchable` fields.</span></span> <span data-ttu-id="f0252-815">A jogkivonatokat létrehozó angol szöveg, például az aposztrófot, vesszővel válassza el egymástól a számok stb közös esetek kezelésére logikát tartalmaz. Például `search=123,456` fog egyezni a 123 és 456, az egyedi feltételeket, hanem egy egyetlen kifejezés 123,456 óta angolul sok ezer elválasztóként használhatók vesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="f0252-815">The tokenizer includes logic to handle cases common to English text like apostrophes, commas in numbers, etc. For example, `search=123,456` will match a single term 123,456 rather than the individual terms 123 and 456, since commas are used as thousand-separators for large numbers in English.</span></span> <span data-ttu-id="f0252-816">Ezért azt javasoljuk, központozási helyett szóköz külön feltételeit a `search` paraméter.</span><span class="sxs-lookup"><span data-stu-id="f0252-816">For this reason, we recommend using white space rather than punctuation to separate terms in the `search` parameter.</span></span>

<span data-ttu-id="f0252-817">`searchMode=any|all`(nem kötelező, az alapértelmezett `any`) – akár a keresési kifejezés részének vagy egészének egyeztetni ahhoz, hogy a dokumentumot, amely száma.</span><span class="sxs-lookup"><span data-stu-id="f0252-817">`searchMode=any|all` (optional, defaults to `any`) - whether any or all of the search terms must be matched in order to count the document as a match.</span></span>

<span data-ttu-id="f0252-818">`searchFields=[string]`(nem kötelező) – a megadott szöveg keresése vesszővel elválasztott mezők nevei a listában.</span><span class="sxs-lookup"><span data-stu-id="f0252-818">`searchFields=[string]` (optional) - The list of comma-separated field names to search for the specified text.</span></span> <span data-ttu-id="f0252-819">Célmező jelölésűnek kell lennie `searchable`.</span><span class="sxs-lookup"><span data-stu-id="f0252-819">Target fields must be marked as `searchable`.</span></span>

<span data-ttu-id="f0252-820">`queryType=simple|full`(nem kötelező, az alapértelmezett `simple`) – Ha egy egyszerű lekérdező nyelv, amely lehetővé teszi, hogy szimbólumok, mint "egyszerű" keresési szöveg beállítása értelmezi +, * és "".</span><span class="sxs-lookup"><span data-stu-id="f0252-820">`queryType=simple|full` (optional, defaults to `simple`) - when set to "simple" search text is interpreted using a simple query language that allows for symbols such as +, * and "".</span></span> <span data-ttu-id="f0252-821">Lekérdezések kiértékelése az összes kereshető mezőt között (vagy mezők jelzett `searchFields`) alapértelmezés szerint minden a dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="f0252-821">Queries are evaluated across all searchable fields (or fields indicated in `searchFields`) in each document by default.</span></span> <span data-ttu-id="f0252-822">Ha a lekérdezés típusa beállítása `full` a Lucene lekérdező nyelv, amely lehetővé teszi, hogy a mező-specifikus és súlyozott keresések keresett szöveg értelmezi.</span><span class="sxs-lookup"><span data-stu-id="f0252-822">When the query type is set to `full` search text is interpreted using the Lucene query language which allows field-specific and weighted searches.</span></span> <span data-ttu-id="f0252-823">Lásd: [egyszerű lekérdezés szintaxisát](https://msdn.microsoft.com/library/dn798920.aspx) és [Lucene lekérdezés szintaxisát](https://msdn.microsoft.com/library/mt589323.aspx) a a keresési szintaxis a részletekért.</span><span class="sxs-lookup"><span data-stu-id="f0252-823">See [Simple Query Syntax](https://msdn.microsoft.com/library/dn798920.aspx) and [Lucene Query Syntax](https://msdn.microsoft.com/library/mt589323.aspx) for specifics on the search syntaxes.</span></span> 

> [!NOTE]
> <span data-ttu-id="f0252-824">Keresés a Lucene lekérdezési nyelv nem támogatott helyett $filter hasonló funkciókat kínál, amelyek a tartományba.</span><span class="sxs-lookup"><span data-stu-id="f0252-824">Range search in the Lucene query language is not supported in favor of $filter which offers similar functionality.</span></span>
> 
> 

<span data-ttu-id="f0252-825">`moreLikeThis=[key]`(választható) **Fontos:** a szolgáltatás csak érhető el a `2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="f0252-825">`moreLikeThis=[key]` (optional) **Important:** This feature is only available in `2015-02-28-Preview`.</span></span> <span data-ttu-id="f0252-826">Ez a beállítás nem használható a szöveges keresési paramétert tartalmazó lekérdezés `search=[string]`.</span><span class="sxs-lookup"><span data-stu-id="f0252-826">This option cannot be used in a query that contains the text search parameter, `search=[string]`.</span></span> <span data-ttu-id="f0252-827">A `moreLikeThis` paraméter megkeresi a dokumentum kulcs által megadott dokumentum hasonló dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="f0252-827">The `moreLikeThis` parameter finds documents that are similar to the document specified by the document key.</span></span> <span data-ttu-id="f0252-828">Ha a keresési kérelem rendelkező `moreLikeThis`, keresési feltételeinek jön létre, a gyakoriság és a dokumentum kifejezések ritkasága alapján.</span><span class="sxs-lookup"><span data-stu-id="f0252-828">When a search request is made with `moreLikeThis`, a list of search terms is generated based on the frequency and rarity of terms in the source document.</span></span> <span data-ttu-id="f0252-829">A feltételeket szolgálnak majd a kérést.</span><span class="sxs-lookup"><span data-stu-id="f0252-829">Those terms are then used to make the request.</span></span> <span data-ttu-id="f0252-830">Alapértelmezés szerint az összes `searchable` mezők minősülnek, kivéve, ha `searchFields` korlátozása, hogy melyik mezőkre történik a keresés segítségével.</span><span class="sxs-lookup"><span data-stu-id="f0252-830">By default, the contents of all `searchable` fields are considered unless `searchFields` is used to restrict which fields are searched.</span></span>  

<span data-ttu-id="f0252-831">`$skip=#`(nem kötelező) – a keresési eredmények kihagyását; száma Nem lehet nagyobb, mint 100 000.</span><span class="sxs-lookup"><span data-stu-id="f0252-831">`$skip=#` (optional) - the number of search results to skip; Cannot be greater than 100,000.</span></span> <span data-ttu-id="f0252-832">Ha megvizsgálja a dokumentumok sorrendben kell, de nem használható `$skip` Ez a korlátozás miatt érdemes `$orderby` teljesen rendezett kulcshoz és `$filter` tartománnyal lekérdezés helyett.</span><span class="sxs-lookup"><span data-stu-id="f0252-832">If you need to scan documents in sequence but cannot use `$skip` due to this limitation, consider using `$orderby` on a totally-ordered key and `$filter` with a range query instead.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-833">Meghívásakor **keresési** POST használ, ez a paraméter neve `skip` helyett `$skip`.</span><span class="sxs-lookup"><span data-stu-id="f0252-833">When calling **Search** using POST, this parameter is named `skip` instead of `$skip`.</span></span>
> 
> 

<span data-ttu-id="f0252-834">`$top=#`(választható) - lekérdezni a keresési eredmények számát.</span><span class="sxs-lookup"><span data-stu-id="f0252-834">`$top=#` (optional) - the number of search results to retrieve.</span></span> <span data-ttu-id="f0252-835">Használható együtt `$skip` a keresési eredmények ügyféloldali lapozás megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-835">This can be used in conjunction with `$skip` to implement client-side paging of search results.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-836">Meghívásakor **keresési** POST használ, ez a paraméter neve `top` helyett `$top`.</span><span class="sxs-lookup"><span data-stu-id="f0252-836">When calling **Search** using POST, this parameter is named `top` instead of `$top`.</span></span>
> 
> 

<span data-ttu-id="f0252-837">`$count=true|false`(nem kötelező, az alapértelmezett `false`) – megadja, hogy a számuk eredményeinek beolvasása.</span><span class="sxs-lookup"><span data-stu-id="f0252-837">`$count=true|false` (optional, defaults to `false`) - Specifies whether to fetch the total count of results.</span></span> <span data-ttu-id="f0252-838">Ez az a szám, amelyek megfelelnek az összes dokumentum a `search` és `$filter` paraméterek, a rendszer figyelmen kívül hagyja `$top` és `$skip`.</span><span class="sxs-lookup"><span data-stu-id="f0252-838">This is the count of all documents that match the `search` and `$filter` parameters, ignoring `$top` and `$skip`.</span></span> <span data-ttu-id="f0252-839">Az érték `true` teljesítmény hatással lehet.</span><span class="sxs-lookup"><span data-stu-id="f0252-839">Setting this value to `true` may have a performance impact.</span></span> <span data-ttu-id="f0252-840">Vegye figyelembe, hogy visszaadott számának közelítő.</span><span class="sxs-lookup"><span data-stu-id="f0252-840">Note that the count returned is an approximation.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-841">Meghívásakor **keresési** POST használ, ez a paraméter neve `count` helyett `$count`.</span><span class="sxs-lookup"><span data-stu-id="f0252-841">When calling **Search** using POST, this parameter is named `count` instead of `$count`.</span></span>
> 
> 

<span data-ttu-id="f0252-842">`$orderby=[string]`(választható) - szűrje az eredményeket kifejezések vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="f0252-842">`$orderby=[string]` (optional) - A list of comma-separated expressions to sort the results by.</span></span> <span data-ttu-id="f0252-843">Minden egyes kifejezés lehet, vagy egy mező nevét, vagy a hívás a `geo.distance()` függvény.</span><span class="sxs-lookup"><span data-stu-id="f0252-843">Each expression can be either a field name or a call to the `geo.distance()` function.</span></span> <span data-ttu-id="f0252-844">Minden egyes kifejezés követhetnek `asc` jelezni a növekvő, és `desc` csökkenő jelzi.</span><span class="sxs-lookup"><span data-stu-id="f0252-844">Each expression can be followed by `asc` to indicated ascending, and `desc` to indicate descending.</span></span> <span data-ttu-id="f0252-845">Az alapértelmezett növekvő sorrend megadásához.</span><span class="sxs-lookup"><span data-stu-id="f0252-845">The default is ascending order.</span></span> <span data-ttu-id="f0252-846">Ki fogja bontásban a dokumentumok egyezés eredményét.</span><span class="sxs-lookup"><span data-stu-id="f0252-846">Ties will be broken by the match scores of documents.</span></span> <span data-ttu-id="f0252-847">Ha nincs `$orderby` van megadva, az alapértelmezett rendezési sorrend szerint dokumentum egyezés pontszám van csökkenő.</span><span class="sxs-lookup"><span data-stu-id="f0252-847">If no `$orderby` is specified, the default sort order is descending by document match score.</span></span> <span data-ttu-id="f0252-848">Egy legfeljebb 32 záradékok `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="f0252-848">There is a limit of 32 clauses for `$orderby`.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-849">Meghívásakor **keresési** POST használ, ez a paraméter neve `orderby` helyett `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="f0252-849">When calling **Search** using POST, this parameter is named `orderby` instead of `$orderby`.</span></span>
> 
> 

<span data-ttu-id="f0252-850">`$select=[string]`(választható) - vesszővel tagolt mezők beolvasásához listáját.</span><span class="sxs-lookup"><span data-stu-id="f0252-850">`$select=[string]` (optional) - A list of comma-separated fields to retrieve.</span></span> <span data-ttu-id="f0252-851">Ha nincs megadva, az összes mezők megjelölve a séma szerint lekérhető szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="f0252-851">If unspecified, all fields marked as retrievable in the schema are included.</span></span> <span data-ttu-id="f0252-852">Ez a paraméter értékre állításával közvetlenül is kérhet minden mező `*`.</span><span class="sxs-lookup"><span data-stu-id="f0252-852">You can also explicitly request all fields by setting this parameter to `*`.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-853">Meghívásakor **keresési** POST használ, ez a paraméter neve `select` helyett `$select`.</span><span class="sxs-lookup"><span data-stu-id="f0252-853">When calling **Search** using POST, this parameter is named `select` instead of `$select`.</span></span>
> 
> 

<span data-ttu-id="f0252-854">`facet=[string]`(nulla vagy több) - dimenzió által egy mezőt.</span><span class="sxs-lookup"><span data-stu-id="f0252-854">`facet=[string]` (zero or more) - A field to facet by.</span></span> <span data-ttu-id="f0252-855">Opcionálisan a karakterlánc tartalmazhat paramétereit, és testre szabhatja a vesszővel tagolt kifejezett értékkorlátozás `name:value` párokat.</span><span class="sxs-lookup"><span data-stu-id="f0252-855">Optionally the string may contain parameters to customize the faceting expressed as comma-separated `name:value` pairs.</span></span> <span data-ttu-id="f0252-856">Érvényes paraméterek a következők:</span><span class="sxs-lookup"><span data-stu-id="f0252-856">Valid parameters are:</span></span>

* <span data-ttu-id="f0252-857">`count`(dimenzió feltételek; száma legfeljebb alapértelmezett érték 10).</span><span class="sxs-lookup"><span data-stu-id="f0252-857">`count` (max number of facet terms; default is 10).</span></span> <span data-ttu-id="f0252-858">Nincs maximum van, de magasabb értékkel fel Önnek megfelelő teljesítményét, különösen akkor, ha a jellemzőalapú mező számos egyedi feltételeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f0252-858">There is no maximum, but higher values incur a corresponding performance penalty, especially if the faceted field contains a large number of unique terms.</span></span>
  * <span data-ttu-id="f0252-859">Például: `facet=category,count:5` a legjobb öt kategóriába lekérdezi a dimenzió eredményei között.</span><span class="sxs-lookup"><span data-stu-id="f0252-859">For example: `facet=category,count:5` gets the top five categories in facet results.</span></span>  
  * <span data-ttu-id="f0252-860">**Megjegyzés:**: Ha a `count` paraméter értéke kisebb, mint az egyedi feltételeket, az eredmények pontatlan lehet.</span><span class="sxs-lookup"><span data-stu-id="f0252-860">**Note**: If the `count` parameter is less than the number of unique terms, the results may not be accurate.</span></span> <span data-ttu-id="f0252-861">A szilánkok értékkorlátozás lekérdezések elosztott módját okozza.</span><span class="sxs-lookup"><span data-stu-id="f0252-861">This is due to the way faceting queries are distributed across shards.</span></span> <span data-ttu-id="f0252-862">Növekvő `count` általában növeli a feltételek száma, de teljesítmény költségekkel pontosságára.</span><span class="sxs-lookup"><span data-stu-id="f0252-862">Increasing `count` generally increases the accuracy of the term counts, but at a performance cost.</span></span>
* <span data-ttu-id="f0252-863">`sort`(egyik `count` rendezéséhez *csökkenő* szerinti `-count` rendezéséhez *növekvő* szerinti `value` rendezéséhez *növekvő* érték, vagy a `-value` rendezéséhez *csökkenő* érték)</span><span class="sxs-lookup"><span data-stu-id="f0252-863">`sort` (one of `count` to sort *descending* by count, `-count` to sort *ascending* by count, `value` to sort *ascending* by value, or `-value` to sort *descending* by value)</span></span>
  * <span data-ttu-id="f0252-864">Például: `facet=category,count:3,sort:count` lekérdezi a három legfontosabb kategóriák értékkorlátozás eredmények minden Város nevű dokumentumok száma szerint csökkenő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="f0252-864">For example: `facet=category,count:3,sort:count` gets the top three categories in facet results in descending order by the number of documents with each city name.</span></span> <span data-ttu-id="f0252-865">Például ha a felső három kategóriába költségvetést, Motel és engedélyezhető, és költségvetési rendelkezik 5 találatok Motel 6 van, és engedélyezhető 4 rendelkezik, akkor a gyűjtők lesz sorrendben Motel, a költségvetési, engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="f0252-865">For example, if the top three categories are Budget, Motel, and Luxury, and Budget has 5 hits, Motel has 6, and Luxury has 4, then the buckets will be in the order Motel, Budget, Luxury.</span></span>
  * <span data-ttu-id="f0252-866">Például: `facet=rating,sort:-value` hozza létre a gyűjtőbe az összes lehetséges értékelések érték szerint csökkenő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="f0252-866">For example: `facet=rating,sort:-value` produces buckets for all possible ratings, in descending order by value.</span></span> <span data-ttu-id="f0252-867">Például ha az összes minősítés 1-5, a gyűjtők lesz sorba rendezve 5, 4, 3, 2, 1, hány dokumentumok felel meg az összes minősítés függetlenül.</span><span class="sxs-lookup"><span data-stu-id="f0252-867">For example, if the ratings are from 1 to 5, the buckets will be ordered 5, 4, 3, 2, 1, irrespective of how many documents match each rating.</span></span>
* <span data-ttu-id="f0252-868">`values`(cső tagolt numerikus vagy `Edm.DateTimeOffset` dimenzióértéknek bejegyzés az dinamikus csoportja értékek)</span><span class="sxs-lookup"><span data-stu-id="f0252-868">`values` (pipe-delimited numeric or `Edm.DateTimeOffset` values specifying a dynamic set of facet entry values)</span></span>
  * <span data-ttu-id="f0252-869">Például: `facet=baseRate,values:10|20` három gyűjtők eredményez: egy alap arányának 0, de nem például legfeljebb 10, 10 egyet, de nem tartoznak a 20 a, és egy a 20 vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="f0252-869">For example: `facet=baseRate,values:10|20` produces three buckets: One for base rate 0 up to but not including 10, one for 10 up to but not including 20, and one for 20 or higher.</span></span>
  * <span data-ttu-id="f0252-870">Például: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` hoz létre két gyűjtők: egy a szállodák felújított 2010. február előtt, egy pedig a szállodákat felújított. február 1., 2010-es vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="f0252-870">For example: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` produces two buckets: One for hotels renovated before February 2010, and one for hotels renovated February 1st, 2010 or later.</span></span>
* <span data-ttu-id="f0252-871">`interval`(a számok, 0-nál nagyobb egész szám időköz vagy `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` idő értékek dátum)</span><span class="sxs-lookup"><span data-stu-id="f0252-871">`interval` (integer interval greater than 0 for numbers, or `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` for date time values)</span></span>
  * <span data-ttu-id="f0252-872">Például: `facet=baseRate,interval:100` gyűjtők mérete 100 alap-címtartományok alapján hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f0252-872">For example: `facet=baseRate,interval:100` produces buckets based on base rate ranges of size 100.</span></span> <span data-ttu-id="f0252-873">Például ha base díjszabás összes $60 és a $600 közötti, lesz a 0 – 100, 100-200-as, 200 – 300, 300-400, 400-500 és 500-600 gyűjtők.</span><span class="sxs-lookup"><span data-stu-id="f0252-873">For example, if base rates are all between $60 and $600, there will be buckets for 0-100, 100-200, 200-300, 300-400, 400-500, and 500-600.</span></span>
  * <span data-ttu-id="f0252-874">Például: `facet=lastRenovationDate,interval:year` hoz létre egy gyűjtő amikor szállodák volt felújított évente.</span><span class="sxs-lookup"><span data-stu-id="f0252-874">For example: `facet=lastRenovationDate,interval:year` produces one bucket for each year when hotels were renovated.</span></span>
* <span data-ttu-id="f0252-875">`timeoffset`([+-] óó: pp, [+-] hhmm, vagy [+-] hh) `timeoffset` nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="f0252-875">`timeoffset` ([+-]hh:mm, [+-]hhmm, or [+-]hh) `timeoffset` is optional.</span></span> <span data-ttu-id="f0252-876">Csak kombinálható az a `interval` lehetőséget, és csak akkor, ha alkalmazva típusú mező `Edm.DateTimeOffset`.</span><span class="sxs-lookup"><span data-stu-id="f0252-876">It can only be combined with the `interval` option, and only when applied to a field of type `Edm.DateTimeOffset`.</span></span> <span data-ttu-id="f0252-877">Az érték az UTC idő eltolódását fiókjára idő határok beállítása határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f0252-877">The value specifies the UTC time offset to account for in setting time boundaries.</span></span>
  * <span data-ttu-id="f0252-878">Például: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` használja, amely elindítja a nap határ 01:00:00 UTC (a cél időzónában éjfél)</span><span class="sxs-lookup"><span data-stu-id="f0252-878">For example: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` uses the day boundary that starts at 01:00:00 UTC (midnight in the target time zone)</span></span>
* <span data-ttu-id="f0252-879">**Megjegyzés:**: `count` és `sort` kombinálható is a ugyanazon dimenzió megadását, de nem használható együtt `interval` vagy `values`, és `interval` és `values` együtt nem használható együtt.</span><span class="sxs-lookup"><span data-stu-id="f0252-879">**Note**: `count` and `sort` can be combined in the same facet specification, but they cannot be combined with `interval` or `values`, and `interval` and `values` cannot be combined together.</span></span>
* <span data-ttu-id="f0252-880">**Megjegyzés:**: dátum idő időköz értékkorlátozás arra az esetre vonatkoznak UTC idő alapján, ha `timeoffset` nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="f0252-880">**Note**: Interval facets on date time are computed based on UTC time if `timeoffset` is not specified.</span></span> <span data-ttu-id="f0252-881">Például: a `facet=lastRenovationDate,interval:day`, a nap határ 00:00:00 UTC kezdődik.</span><span class="sxs-lookup"><span data-stu-id="f0252-881">For example: for `facet=lastRenovationDate,interval:day`, the day boundary starts at 00:00:00 UTC.</span></span> 

> [!NOTE]
> <span data-ttu-id="f0252-882">Meghívásakor **keresési** POST használ, ez a paraméter neve `facets` helyett `facet`.</span><span class="sxs-lookup"><span data-stu-id="f0252-882">When calling **Search** using POST, this parameter is named `facets` instead of `facet`.</span></span> <span data-ttu-id="f0252-883">Is akkor adja meg azt a JSON-tömb, ahol minden karakterlánca egy külön értékkorlátozás kifejezés karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="f0252-883">Also, you specify it as a JSON array of strings where each string is a separate facet expression.</span></span>
> 
> 

<span data-ttu-id="f0252-884">`$filter=[string]`(nem kötelező) – standard OData szintaxisban strukturált keresőkifejezést.</span><span class="sxs-lookup"><span data-stu-id="f0252-884">`$filter=[string]` (optional) - A structured search expression in standard OData syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-885">Meghívásakor **keresési** POST használ, ez a paraméter neve `filter` helyett `$filter`.</span><span class="sxs-lookup"><span data-stu-id="f0252-885">When calling **Search** using POST, this parameter is named `filter` instead of `$filter`.</span></span>
> 
> 

<span data-ttu-id="f0252-886">`highlight=[string]`(választható) - készletként használt találat vesszővel elválasztott mezők nevei mutatja be.</span><span class="sxs-lookup"><span data-stu-id="f0252-886">`highlight=[string]` (optional) - A set of comma-separated field names used for hit highlights.</span></span> <span data-ttu-id="f0252-887">Csak `searchable` mezők találatok kiemelése is használható.</span><span class="sxs-lookup"><span data-stu-id="f0252-887">Only `searchable` fields can be used for hit highlighting.</span></span>

<span data-ttu-id="f0252-888">`highlightPreTag=[string]`(nem kötelező, az alapértelmezett `<em>`) – egy karakterlánc-címke, amely lefoglalja érni emeli ki.</span><span class="sxs-lookup"><span data-stu-id="f0252-888">`highlightPreTag=[string]` (optional, defaults to `<em>`) - A string tag that prepends to hit highlights.</span></span> <span data-ttu-id="f0252-889">Meg kell a `highlightPostTag`.</span><span class="sxs-lookup"><span data-stu-id="f0252-889">Must be set with `highlightPostTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-890">Meghívásakor **keresési** GET használ, az URL-cím fenntartott karakterek százalék-kódolással kell (például % &#23; helyett).</span><span class="sxs-lookup"><span data-stu-id="f0252-890">When calling **Search** using GET, reserved characters in the URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="f0252-891">`highlightPostTag=[string]`(nem kötelező, az alapértelmezett `</em>`)-karakterlánc címke, amely hozzáfűzi a találati emeli ki.</span><span class="sxs-lookup"><span data-stu-id="f0252-891">`highlightPostTag=[string]` (optional, defaults to `</em>`) - a string tag that appends to hit highlights.</span></span> <span data-ttu-id="f0252-892">Meg kell a `highlightPreTag`.</span><span class="sxs-lookup"><span data-stu-id="f0252-892">Must be set with `highlightPreTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-893">Meghívásakor **keresési** GET használ, az URL-cím fenntartott karakterek százalék-kódolással kell (például % &#23; helyett).</span><span class="sxs-lookup"><span data-stu-id="f0252-893">When calling **Search** using GET, reserved characters in the URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="f0252-894">`scoringProfile=[string]`(választható) - kiértékelése a pontozási profil nevének egyeznie vonatkozó eredmények dokumentumok megfelelő ahhoz, hogy az eredmények rendezéséhez.</span><span class="sxs-lookup"><span data-stu-id="f0252-894">`scoringProfile=[string]` (optional) - The name of a scoring profile to evaluate match scores for matching documents in order to sort the results.</span></span>

<span data-ttu-id="f0252-895">`scoringParameter=[string]`(nulla vagy több) – azt jelzi, az értékek pontozó függvények megadott mindegyik paraméterhez (például `referencePointParameter`) a következő formátumban `name-value1,value2,...`.</span><span class="sxs-lookup"><span data-stu-id="f0252-895">`scoringParameter=[string]` (zero or more) - Indicates the values for each parameter defined in a scoring function (for example, `referencePointParameter`) using the format `name-value1,value2,...`.</span></span>

* <span data-ttu-id="f0252-896">Például, ha a pontozási profil határozza meg a függvény "mylocation" nevű paraméterrel a lekérdezési karakterláncának beállítására lenne `&scoringParameter=mylocation--122.2,44.8`.</span><span class="sxs-lookup"><span data-stu-id="f0252-896">For example, if the scoring profile defines a function with a parameter called "mylocation" the query string option would be `&scoringParameter=mylocation--122.2,44.8`.</span></span> <span data-ttu-id="f0252-897">Az első dash neve elválasztja az listában, míg a második dash része az első értéket (a földrajzi hosszúság értéke ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="f0252-897">The first dash separates the name from the value list, while the second dash is part of the first value (longitude in this example).</span></span>
* <span data-ttu-id="f0252-898">Pontozási paraméterek például címke kiemelése, vesszővel válassza el egymástól, tartalmazhat szimpla idézőjelben használatával lista bármely ilyen értéke karaktert.</span><span class="sxs-lookup"><span data-stu-id="f0252-898">For scoring parameters such as for tag boosting that can contain commas, you can escape any such values in the list using single quotes.</span></span> <span data-ttu-id="f0252-899">Az értékek maguk tartalmazhat félidézőjelet, is escape így dupla által.</span><span class="sxs-lookup"><span data-stu-id="f0252-899">If the values themselves contain single quotes, you can escape them by doubling.</span></span>
  * <span data-ttu-id="f0252-900">Például ha egy címke "mytag" nevű paramétert kiemelése rendelkezik, és a címke növelése kívánt értékek "Hello, O'Brien" és "Smith", a lekérdezési karakterláncának beállítására lenne `&scoringParameter=mytag-'Hello, O''Brien',Smith`.</span><span class="sxs-lookup"><span data-stu-id="f0252-900">For example, if you have a tag boosting parameter called "mytag" and you want to boost on the tag values "Hello, O'Brien" and "Smith", the query string option would be `&scoringParameter=mytag-'Hello, O''Brien',Smith`.</span></span> <span data-ttu-id="f0252-901">Ne feledje, hogy ajánlatok csak szükséges értékeket tartalmazó, vesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="f0252-901">Note that quotes are only required for values that contain commas.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-902">Meghívásakor **keresési** POST használ, ez a paraméter neve `scoringParameters` helyett `scoringParameter`.</span><span class="sxs-lookup"><span data-stu-id="f0252-902">When calling **Search** using POST, this parameter is named `scoringParameters` instead of `scoringParameter`.</span></span> <span data-ttu-id="f0252-903">Is, akkor adja meg azt a JSON-tömb, ahol minden karakterlánca külön karakterláncok `name-values` pár.</span><span class="sxs-lookup"><span data-stu-id="f0252-903">Also, you specify it as a JSON array of strings where each string is a separate `name-values` pair.</span></span>
> 
> 

<span data-ttu-id="f0252-904">`minimumCoverage`(nem kötelező, az alapértelmezett érték 100) – 0 és 100 százalékos aránya az indexet, át kell gondolni ahhoz, hogy a lekérdezés kell jelenteni sikeres, a keresési lekérdezés jelző közötti szám.</span><span class="sxs-lookup"><span data-stu-id="f0252-904">`minimumCoverage` (optional, defaults to 100) - a number between 0 and 100 indicating the percentage of the index that must be covered by a search query in order for the query to be reported as a success.</span></span> <span data-ttu-id="f0252-905">Alapértelmezés szerint a teljes index elérhetőnek kell lennie, vagy `Search` 503-as HTTP-állapotkód: ad vissza.</span><span class="sxs-lookup"><span data-stu-id="f0252-905">By default, the entire index must be available or `Search` will return HTTP status code 503.</span></span> <span data-ttu-id="f0252-906">Ha `minimumCoverage` és `Search` sikeres, térjen vissza a 200-as HTTP, és tartalmazzák a `@search.coverage` a válaszban az index a lekérdezésben szereplő százalékos értékét.</span><span class="sxs-lookup"><span data-stu-id="f0252-906">If you set `minimumCoverage` and `Search` succeeds, it will return HTTP 200 and include a `@search.coverage` value in the response indicating the percentage of the index that was included in the query.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-907">A paraméter értéke 100-nál kisebb search rendelkezésre állását, még a szolgáltatások csak egy replika azért hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="f0252-907">Setting this parameter to a value lower than 100 can be useful for ensuring search availability even for services with only one replica.</span></span> <span data-ttu-id="f0252-908">Azonban nem minden egyező dokumentumok garantáltan a keresési eredmények között szerepel.</span><span class="sxs-lookup"><span data-stu-id="f0252-908">However, not all matching documents are guaranteed to be present in the search results.</span></span> <span data-ttu-id="f0252-909">Ha keresési visszaírási fontosabb az alkalmazáshoz, mint a rendelkezésre állási, akkor a legcélszerűbb hagyja `minimumCoverage` az alapértelmezett érték 100.</span><span class="sxs-lookup"><span data-stu-id="f0252-909">If search recall is more important to your application than availability, then it's best to leave `minimumCoverage` at its default value of 100.</span></span>
> 
> 

<span data-ttu-id="f0252-910">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="f0252-910">`api-version=[string]` (required).</span></span> <span data-ttu-id="f0252-911">Az előzetes verzió `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="f0252-911">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="f0252-912">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="f0252-912">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="f0252-913">Megjegyzés: A művelet a `api-version` egy lekérdezési paraméter, függetlenül attól, hogy meghívja az URL-címben megadott **keresési** GET vagy POST.</span><span class="sxs-lookup"><span data-stu-id="f0252-913">Note: For this operation, the `api-version` is specified as a query parameter in the URL regardless of whether you call **Search** with GET or POST.</span></span>

<span data-ttu-id="f0252-914">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="f0252-914">**Request Headers**</span></span>

<span data-ttu-id="f0252-915">Az alábbi lista ismerteti a szükséges és választható kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="f0252-915">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="f0252-916">`api-key`: A `api-key` hitelesíteni a kérelmet a keresőszolgáltatása szolgál.</span><span class="sxs-lookup"><span data-stu-id="f0252-916">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="f0252-917">Egy karakterláncértéket egyedi, a szolgáltatás URL-címre.</span><span class="sxs-lookup"><span data-stu-id="f0252-917">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="f0252-918">A **keresési** kérelem adhat meg, egy adminisztrációs kulcsot vagy egy lekérdezés kulcs `api-key`.</span><span class="sxs-lookup"><span data-stu-id="f0252-918">The **Search** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="f0252-919">Konfigurálnia kell a szolgáltatás nevét, a kérelem URL-címe összeállításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-919">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="f0252-920">Beszerezheti a szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján, az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f0252-920">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="f0252-921">Lásd: [Azure Search szolgáltatás létrehozása a portálon](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="f0252-921">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="f0252-922">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="f0252-922">**Request Body**</span></span>

<span data-ttu-id="f0252-923">A GET: nincs.</span><span class="sxs-lookup"><span data-stu-id="f0252-923">For GET: None.</span></span>

<span data-ttu-id="f0252-924">A FELADÁS egy vagy több:</span><span class="sxs-lookup"><span data-stu-id="f0252-924">For POST:</span></span>

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
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

<span data-ttu-id="f0252-925">**A részleges kereséshez válaszok folytatása**</span><span class="sxs-lookup"><span data-stu-id="f0252-925">**Continuation of Partial Search Responses**</span></span>

<span data-ttu-id="f0252-926">Egyes esetekben Azure Search nem adhat vissza a kért eredmények egyetlen keresési válasz.</span><span class="sxs-lookup"><span data-stu-id="f0252-926">Sometimes Azure Search can't return all the requested results in a single Search response.</span></span> <span data-ttu-id="f0252-927">Ez akkor fordulhat elő, például ha a lekérdezés kísérel meg túl sok dokumentumok nem ad meg a különböző okokból `$top` vagy értéket `$top` túl nagy.</span><span class="sxs-lookup"><span data-stu-id="f0252-927">This can happen for different reasons, such as when the query requests too many documents by not specifying `$top` or specifying a value for `$top` that is too large.</span></span> <span data-ttu-id="f0252-928">Ilyen esetben az Azure Search kiterjed a `@odata.nextLink` adott válasz törzsének megjegyzése és is `@search.nextPageParameters` Ha POST kérelem volt.</span><span class="sxs-lookup"><span data-stu-id="f0252-928">In such cases, Azure Search will include the `@odata.nextLink` annotation in the response body, and also `@search.nextPageParameters` if it was a POST request.</span></span> <span data-ttu-id="f0252-929">Ezek a jegyzeteket értékek segítségével állítson össze egy másik keresési kérelem lekérni a keresési válasz a következő része.</span><span class="sxs-lookup"><span data-stu-id="f0252-929">You can use the values of these annotations to formulate another Search request to get the next part of the search response.</span></span> <span data-ttu-id="f0252-930">Ez a lehetőség egy ***folytatási*** az eredeti keresés kérelmet, és a jegyzetek általában nevezzük ***folytatási jogkivonatok***.</span><span class="sxs-lookup"><span data-stu-id="f0252-930">This is called a ***continuation*** of the original Search request, and the annotations are generally called ***continuation tokens***.</span></span> <span data-ttu-id="f0252-931">Lásd: [az alábbi példában](#SearchResponse) talál részletes információt a szintaxist, ezek a jegyzeteket és hol szerepelnek a válasz törzsében.</span><span class="sxs-lookup"><span data-stu-id="f0252-931">See [the example below](#SearchResponse) for details on the syntax of these annotations and where they appear in the response body.</span></span> 

<span data-ttu-id="f0252-932">Miért Azure Search előfordulhat, hogy vissza a folytatási jogkivonatok okai függő és bármikor megváltozhat.</span><span class="sxs-lookup"><span data-stu-id="f0252-932">The reasons why Azure Search might return continuation tokens are implementation-specific and subject to change.</span></span> <span data-ttu-id="f0252-933">Robusztus ügyfelek mindig esetekben, amikor a vártnál kevesebb dokumentumot ad vissza, és a folytatási kód megtalálható folytatni a dokumentumok kezeléséhez készen áll.</span><span class="sxs-lookup"><span data-stu-id="f0252-933">Robust clients should always be ready to handle cases where fewer documents than expected are returned and a continuation token is included to continue retrieving documents.</span></span> <span data-ttu-id="f0252-934">Ne feledje, hogy ugyanazt a HTTP-metódus karakterként használjon az eredeti kérést folytatásához.</span><span class="sxs-lookup"><span data-stu-id="f0252-934">Also note that you must use the same HTTP method as the original request in order to continue.</span></span> <span data-ttu-id="f0252-935">Például ha egy GET kérelmet küldött, minden folytatási kérést küld kell használnia az GET (és hasonlóképpen POST).</span><span class="sxs-lookup"><span data-stu-id="f0252-935">For example, if you sent a GET request, any continuation requests you send must also use GET (and likewise for POST).</span></span>

<span data-ttu-id="f0252-936"><a name="SearchResponse"></a>
**Válasz**</span><span class="sxs-lookup"><span data-stu-id="f0252-936"><a name="SearchResponse"></a>
**Response**</span></span>

<span data-ttu-id="f0252-937">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="f0252-937">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
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
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
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
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

<span data-ttu-id="f0252-938">**Példák:**</span><span class="sxs-lookup"><span data-stu-id="f0252-938">**Examples:**</span></span>

<span data-ttu-id="f0252-939">A további példákat talál a [OData kifejezési szintaxist az Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) lap.</span><span class="sxs-lookup"><span data-stu-id="f0252-939">You can find additional examples on the [OData Expression Syntax for Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) page.</span></span>

1)    <span data-ttu-id="f0252-940">Keresse meg a rendezve csökkenő dátum szerint.</span><span class="sxs-lookup"><span data-stu-id="f0252-940">Search the Index sorted descending by date.</span></span>

    <span data-ttu-id="f0252-941">GET /indexes/hotels/docs? keresési = * & $orderby = lastRenovationDate desc & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="f0252-941">GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="f0252-942">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "*", "orderby": "lastRenovationDate desc"}</span><span class="sxs-lookup"><span data-stu-id="f0252-942">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "orderby": "lastRenovationDate desc" }</span></span>

2)    <span data-ttu-id="f0252-943">Keresse meg a jellemzőalapú keresés, és értékkorlátozás a kategóriák, minősítés, címkéket, valamint az adott címtartományok baseRate elemek beolvasása:</span><span class="sxs-lookup"><span data-stu-id="f0252-943">In a faceted search, search the index and retrieve facets for categories, rating, tags, as well as items with baseRate in specific ranges:</span></span>

    <span data-ttu-id="f0252-944">GET /indexes/hotels/docs? keresési = test & értékkorlátozás = kategória & értékkorlátozás = minősítés & értékkorlátozás = címkék & értékkorlátozás baseRate, érték: 80 = |} 150 |} 220 & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="f0252-944">GET /indexes/hotels/docs?search=test&facet=category&facet=rating&facet=tags&facet=baseRate,values:80|150|220&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="f0252-945">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "teszt", "értékkorlátozással": ["kategória", "minősítés", "címkék", "baseRate, érték: 80-as |} 150 |} 220"]}</span><span class="sxs-lookup"><span data-stu-id="f0252-945">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "test", "facets": [ "category", "rating", "tags", "baseRate,values:80|150|220" ] }</span></span>

3)    <span data-ttu-id="f0252-946">Az előző jellemzőalapú lekérdezés eredményei egy szűrő használatával szűkítéséhez, miután a felhasználó a hivatkozásra kattint 3. és a "Motel" kategóriát rangsorolási:</span><span class="sxs-lookup"><span data-stu-id="f0252-946">Using a filter, narrow down the previous faceted query results after the user clicks on rating 3 and category "Motel":</span></span>

    <span data-ttu-id="f0252-947">GET /indexes/hotels/docs? keresési = test & értékkorlátozás = címkék & értékkorlátozás baseRate, érték: 80 = |} 150 |} 220 & $filter minősítés eq 3 és kategória-eq "Motel" & api-version = 2015-02-28 – előzetes verzió =</span><span class="sxs-lookup"><span data-stu-id="f0252-947">GET /indexes/hotels/docs?search=test&facet=tags&facet=baseRate,values:80|150|220&$filter=rating eq 3 and category eq 'Motel'&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="f0252-948">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "teszt", "értékkorlátozással": ["címkék", "baseRate, érték: 80-as |} 150 |} 220"], "szűrő": "minősítés eq 3 és kategória-eq"Motel""}</span><span class="sxs-lookup"><span data-stu-id="f0252-948">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "test", "facets": [ "tags", "baseRate,values:80|150|220" ], "filter": "rating eq 3 and category eq 'Motel'" }</span></span>

4) <span data-ttu-id="f0252-949">Jellemzőalapú keresés – az egyedi feltételeket a lekérdezés által visszaadott állítson be egy felső korlátot.</span><span class="sxs-lookup"><span data-stu-id="f0252-949">In a faceted search, set an upper limit on unique terms returned in a query.</span></span> <span data-ttu-id="f0252-950">Az alapértelmezett érték 10, de növelheti vagy csökkentheti a érték használata a `count` paraméter a `facet` attribútum:</span><span class="sxs-lookup"><span data-stu-id="f0252-950">The default is 10, but you can increase or decrease this value using the `count` parameter on the `facet` attribute:</span></span>

    <span data-ttu-id="f0252-951">GET /indexes/hotels/docs? keresési = test & értékkorlátozás = város, számláló: 5 & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="f0252-951">GET /indexes/hotels/docs?search=test&facet=city,count:5&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="f0252-952">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "teszt", "értékkorlátozással": ["város, számláló: 5"]}</span><span class="sxs-lookup"><span data-stu-id="f0252-952">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview  {    "search": "test",    "facets": [ "city,count:5" ]  }</span></span>

5)    <span data-ttu-id="f0252-953">Keresse meg a belül adott mezők; Ha például egy nyelvspecifikus mező:</span><span class="sxs-lookup"><span data-stu-id="f0252-953">Search the Index within specific fields; For example, a language-specific field:</span></span>

    <span data-ttu-id="f0252-954">GET /indexes/hotels/docs? keresési = hôtel & searchFields = description_fr & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="f0252-954">GET /indexes/hotels/docs?search=hôtel&searchFields=description_fr&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="f0252-955">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "hôtel", "searchFields": "description_fr"}</span><span class="sxs-lookup"><span data-stu-id="f0252-955">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "hôtel", "searchFields": "description_fr" }</span></span>

6) <span data-ttu-id="f0252-956">Keresse meg a több mező között.</span><span class="sxs-lookup"><span data-stu-id="f0252-956">Search the Index across multiple fields.</span></span> <span data-ttu-id="f0252-957">Például tárolja, és a lekérdezés több nyelven, ugyanazt az indexet belül az összes kereshető mezőt.</span><span class="sxs-lookup"><span data-stu-id="f0252-957">For example, you can store and query searchable fields in multiple languages, all within the same index.</span></span>  <span data-ttu-id="f0252-958">Ha angol és francia leírások üzemel, a dokumentumon, vagy azok a lekérdezés eredményében térhet vissza:</span><span class="sxs-lookup"><span data-stu-id="f0252-958">If English and French descriptions co-exist in the same document, you can return any or all in the query results:</span></span>

    <span data-ttu-id="f0252-959">GET /indexes/hotels/docs? keresési = Szálloda & searchFields = leírás, description_fr & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="f0252-959">GET /indexes/hotels/docs?search=hotel&searchFields=description,description_fr&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="f0252-960">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "Szálloda", "searchFields": "leírás, description_fr"}</span><span class="sxs-lookup"><span data-stu-id="f0252-960">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview  {    "search": "hotel",    "searchFields": "description, description_fr"  }</span></span>

<span data-ttu-id="f0252-961">Vegye figyelembe, hogy csak lekérdezhető egy index egyszerre.</span><span class="sxs-lookup"><span data-stu-id="f0252-961">Note that you can only query one index at a time.</span></span> <span data-ttu-id="f0252-962">Ne hozzon létre az egyes nyelvekhez több index, kivéve, ha azt tervezi, hogy egyszerre csak egy lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="f0252-962">Do not create multiple indexes for each language unless you plan to query one at a time.</span></span>

7)    <span data-ttu-id="f0252-963">Lapozófájl - Get-elemek 1. oldal (oldal mérete 10):</span><span class="sxs-lookup"><span data-stu-id="f0252-963">Paging - Get the 1st page of items (page size is 10):</span></span>

    <span data-ttu-id="f0252-964">GET /indexes/hotels/docs? keresési = * & $skip = 0 & $top = 10 & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="f0252-964">GET /indexes/hotels/docs?search=*&$skip=0&$top=10&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="f0252-965">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "*", "kihagyása": 0, a "top": 10}</span><span class="sxs-lookup"><span data-stu-id="f0252-965">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "skip": 0, "top": 10 }</span></span>

8)    <span data-ttu-id="f0252-966">Lapozófájl - Get-elemek a 2. lap (oldal mérete 10):</span><span class="sxs-lookup"><span data-stu-id="f0252-966">Paging - Get the 2nd page of items (page size is 10):</span></span>

    <span data-ttu-id="f0252-967">GET /indexes/hotels/docs? keresési = * & $skip = 10 & $top = 10 & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="f0252-967">GET /indexes/hotels/docs?search=*&$skip=10&$top=10&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="f0252-968">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "*", "kihagyása": "felső" 10: 10}</span><span class="sxs-lookup"><span data-stu-id="f0252-968">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "skip": 10, "top": 10 }</span></span>

9)    <span data-ttu-id="f0252-969">Lekéri egy adott készletét mezők:</span><span class="sxs-lookup"><span data-stu-id="f0252-969">Retrieve a specific set of fields:</span></span>

    <span data-ttu-id="f0252-970">GET /indexes/hotels/docs? keresési = * & $select = hotelName, leírása és api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="f0252-970">GET /indexes/hotels/docs?search=*&$select=hotelName,description&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="f0252-971">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "*", "select": "hotelName, leírás"}</span><span class="sxs-lookup"><span data-stu-id="f0252-971">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview { "search": "*", "select": "hotelName, description" }</span></span>

10)  <span data-ttu-id="f0252-972">A megadott szűrő kifejezésnek megfelelő dokumentumok beolvasása:</span><span class="sxs-lookup"><span data-stu-id="f0252-972">Retrieve documents matching a specific filter expression:</span></span>

    <span data-ttu-id="f0252-973">GET /indexes/hotels/docs? $filter = (baseRate ge 60 és baseRate lt 300) vagy a hotelName eq divatos maradnak, és az api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="f0252-973">GET /indexes/hotels/docs?$filter=(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="f0252-974">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"szűrő": "(baseRate ge 60 és baseRate lt 300) hotelName eq"Divatos maradás"vagy"}</span><span class="sxs-lookup"><span data-stu-id="f0252-974">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {  "filter": "(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'" }</span></span>

11) <span data-ttu-id="f0252-975">Találati emeli ki az indexet, és térjen vissza töredék keresése</span><span class="sxs-lookup"><span data-stu-id="f0252-975">Search the index and return fragments with hit highlights</span></span>

    <span data-ttu-id="f0252-976">GET /indexes/hotels/docs? keresési valami = & Jelöljön ki leírás & api-version = 2015-02-28-Preview =</span><span class="sxs-lookup"><span data-stu-id="f0252-976">GET /indexes/hotels/docs?search=something&highlight=description&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="f0252-977">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "egy", "highlight": "description"}</span><span class="sxs-lookup"><span data-stu-id="f0252-977">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "highlight": "description" }</span></span>

12) <span data-ttu-id="f0252-978">Keresse meg a, és térjen vissza a dokumentumok megtalálhatók-e el egy hivatkozási közelebb helyről rendezve</span><span class="sxs-lookup"><span data-stu-id="f0252-978">Search the index and return documents sorted from closer to farther away from a reference location</span></span>

    <span data-ttu-id="f0252-979">GET /indexes/hotels/docs? keresési valami = & $orderby=geo.distance (helyét, geography'POINT(-122.12315 47.88121) ") & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="f0252-979">GET /indexes/hotels/docs?search=something&$orderby=geo.distance(location, geography'POINT(-122.12315 47.88121)')&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="f0252-980">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "egy", "orderby": "geo.distance (helyét, geography'POINT(-122.12315 47.88121)") "}</span><span class="sxs-lookup"><span data-stu-id="f0252-980">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "orderby": "geo.distance(location, geography'POINT(-122.12315 47.88121)')" }</span></span>

13) <span data-ttu-id="f0252-981">Keresse meg a feltételezve, hogy a relevanciaprofil "földrajzi" nevű két a távolságot pontozó függvények a, egy meghatározása a paraméter neve "currentLocation", a másik egy paraméter "lastLocation" meghatározása</span><span class="sxs-lookup"><span data-stu-id="f0252-981">Search the index assuming there's a scoring profile called "geo" with two distance scoring functions, one defining a parameter called "currentLocation" and one defining a parameter called "lastLocation"</span></span>

    <span data-ttu-id="f0252-982">GET /indexes/hotels/docs? keresési valami = & scoringProfile = földrajzi & scoringParameter currentLocation--122.123,44.77233 & scoringParameter = lastLocation--121.499,44.2113 & api-version = 2015-02-28-Preview =</span><span class="sxs-lookup"><span data-stu-id="f0252-982">GET /indexes/hotels/docs?search=something&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&scoringParameter=lastLocation--121.499,44.2113&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="f0252-983">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "egy", "scoringProfile": "földrajzi", "scoringParameters": ["currentLocation--122.123,44.77233", "lastLocation--121.499,44.2113"]}</span><span class="sxs-lookup"><span data-stu-id="f0252-983">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "something",   "scoringProfile": "geo",   "scoringParameters": [ "currentLocation--122.123,44.77233", "lastLocation--121.499,44.2113" ] }</span></span>

14) <span data-ttu-id="f0252-984">Dokumentumok keresése az index használatával [egyszerű lekérdezés szintaxisát](https://msdn.microsoft.com/library/dn798920.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0252-984">Find documents in the index using [simple query syntax](https://msdn.microsoft.com/library/dn798920.aspx).</span></span> <span data-ttu-id="f0252-985">A lekérdezés által visszaadott, ha a kereshető mezők tartalmaznak a feltételek "megerősítő" és "hely", de nem a "motel" hotels:</span><span class="sxs-lookup"><span data-stu-id="f0252-985">This query returns hotels where searchable fields contain the terms "comfort" and "location" but not "motel":</span></span>

    <span data-ttu-id="f0252-986">GET /indexes/hotels/docs? keresési = megerősítő + hely-motel & searchMode = all & api-version = 2015-02-28 – előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="f0252-986">GET /indexes/hotels/docs?search=comfort +location -motel&searchMode=all&api-version=2015-02-28-Preview</span></span>

    <span data-ttu-id="f0252-987">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "megerősítő + hely-motel", "searchMode": "all"}</span><span class="sxs-lookup"><span data-stu-id="f0252-987">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "comfort +location -motel",   "searchMode": "all" }</span></span>

<span data-ttu-id="f0252-988">Vegye figyelembe a használatát `searchMode=all` felett.</span><span class="sxs-lookup"><span data-stu-id="f0252-988">Note the use of `searchMode=all` above.</span></span> <span data-ttu-id="f0252-989">Ennek a paraméternek, beleértve felülbírálja az alapértelmezett `searchMode=any`, biztosítani kell, hogy `-motel` azt jelenti, hogy ", és nem" helyett ", vagy nincs".</span><span class="sxs-lookup"><span data-stu-id="f0252-989">Including this parameter overrides the default of `searchMode=any`, ensuring that `-motel` means "AND NOT" instead of "OR NOT".</span></span> <span data-ttu-id="f0252-990">Nélkül `searchMode=all`, ", vagy nem", amely korlátozza a keresési eredmények helyett bővíti kap, és szeretne egyes felhasználóknak counter-intuitive is lehet.</span><span class="sxs-lookup"><span data-stu-id="f0252-990">Without `searchMode=all`, you get "OR NOT" which expands rather than restricts search results, and this can be counter-intuitive to some users.</span></span>

15) <span data-ttu-id="f0252-991">Dokumentumok keresése az index használatával [lucene lekérdezés szintaxisát](https://msdn.microsoft.com/library/mt589323.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0252-991">Find documents in the index using [lucene query syntax](https://msdn.microsoft.com/library/mt589323.aspx).</span></span> <span data-ttu-id="f0252-992">A lekérdezés által visszaadott szállodák, ahol a kategória mező tartalma: a kifejezés "költségvetés" és "nemrég felújított" kifejezést tartalmazó összes kereshető mezőt.</span><span class="sxs-lookup"><span data-stu-id="f0252-992">This query returns hotels where the category field contains the term "budget" and all searchable fields containing the phrase "recently renovated".</span></span> <span data-ttu-id="f0252-993">"Nemrég felújított" kifejezést tartalmazó dokumentumokat magasabb rangsora miatt a kifejezés program érték (3)</span><span class="sxs-lookup"><span data-stu-id="f0252-993">Documents containing the phrase "recently renovated" are ranked higher as a result of the term boost value (3)</span></span>

    <span data-ttu-id="f0252-994">GET /indexes/hotels/docs? keresési kategória: költségvetés = és \"nemrég felújított\"^ 3 & searchMode = all & api-version = 2015-02-28-Preview & querytype teljes =</span><span class="sxs-lookup"><span data-stu-id="f0252-994">GET /indexes/hotels/docs?search=category:budget AND \"recently renovated\"^3&searchMode=all&api-version=2015-02-28-Preview&querytype=full</span></span>

    <span data-ttu-id="f0252-995">POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "kategória: költségvetés és \"nemrég felújított\"^ 3", "queryType": "teljes", "searchMode": "all"}</span><span class="sxs-lookup"><span data-stu-id="f0252-995">POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview {   "search": "category:budget AND \"recently renovated\"^3",   "queryType": "full",   "searchMode": "all" }</span></span>

<a name="LookupAPI"></a>

## <a name="lookup-document"></a><span data-ttu-id="f0252-996">Keresés a dokumentum</span><span class="sxs-lookup"><span data-stu-id="f0252-996">Lookup Document</span></span>
<span data-ttu-id="f0252-997">A **keresési dokumentum** Azure keresési művelet lekérdezi egy dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="f0252-997">The **Lookup Document** operation retrieves a document from Azure Search.</span></span> <span data-ttu-id="f0252-998">Ez akkor hasznos, ha a felhasználó egy adott keresési eredmény kattint, és keresse meg a dokumentum részletes adatait szeretné.</span><span class="sxs-lookup"><span data-stu-id="f0252-998">This is useful when a user clicks on a specific search result, and you want to look up specific details about that document.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

<span data-ttu-id="f0252-999">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="f0252-999">**Request**</span></span>

<span data-ttu-id="f0252-1000">HTTPS szolgáltatáskérések szükség.</span><span class="sxs-lookup"><span data-stu-id="f0252-1000">HTTPS is required for service requests.</span></span> <span data-ttu-id="f0252-1001">A **keresési dokumentum** kérelmek az alábbiak szerint lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f0252-1001">The **Lookup Document** request can be constructed as follows.</span></span>

    GET /indexes/[index name]/docs/key?[query parameters]

<span data-ttu-id="f0252-1002">Másik lehetőségként használhatja a hagyományos OData-szintaxis kulcskeresési:</span><span class="sxs-lookup"><span data-stu-id="f0252-1002">Alternatively, you can use the traditional OData syntax for key lookup:</span></span>

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

<span data-ttu-id="f0252-1003">A kérelem URI-azonosítója tartalmaz egy [index neve] és [kulcs], mely dokumentum melyik index lekérése megadása.</span><span class="sxs-lookup"><span data-stu-id="f0252-1003">The request URI includes an [index name] and [key], specifying which document to retrieve from which index.</span></span> <span data-ttu-id="f0252-1004">Egyszerre csak egy dokumentum kérheti le.</span><span class="sxs-lookup"><span data-stu-id="f0252-1004">You can only get one document at a time.</span></span> <span data-ttu-id="f0252-1005">Használjon **keresési** egyetlen kérelem több dokumentum beolvasása.</span><span class="sxs-lookup"><span data-stu-id="f0252-1005">Use **Search** to get multiple documents in a single request.</span></span>

<span data-ttu-id="f0252-1006">**Lekérdezés-paraméterek**</span><span class="sxs-lookup"><span data-stu-id="f0252-1006">**Query Parameters**</span></span>

<span data-ttu-id="f0252-1007">`$select=[string]`(választható) - vesszővel tagolt mezők beolvasásához listáját.</span><span class="sxs-lookup"><span data-stu-id="f0252-1007">`$select=[string]` (optional) - a list of comma-separated fields to retrieve.</span></span> <span data-ttu-id="f0252-1008">Ha nincs megadva vagy `*`, megjelölve az lekérhető a séma összes mezők szerepelnek a leképezés.</span><span class="sxs-lookup"><span data-stu-id="f0252-1008">If unspecified or set to `*`, all fields marked as retrievable in the schema are included in the projection.</span></span>

<span data-ttu-id="f0252-1009">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="f0252-1009">`api-version=[string]` (required).</span></span> <span data-ttu-id="f0252-1010">Az előzetes verzió `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1010">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="f0252-1011">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="f0252-1011">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="f0252-1012">Megjegyzés: A művelet a `api-version` egy lekérdezési paraméter van megadva.</span><span class="sxs-lookup"><span data-stu-id="f0252-1012">Note: For this operation, the `api-version` is specified as a query parameter.</span></span>

<span data-ttu-id="f0252-1013">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="f0252-1013">**Request Headers**</span></span>

<span data-ttu-id="f0252-1014">Az alábbi lista ismerteti a szükséges és választható kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="f0252-1014">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="f0252-1015">`api-key`: A `api-key` hitelesíteni a kérelmet a keresőszolgáltatása szolgál.</span><span class="sxs-lookup"><span data-stu-id="f0252-1015">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="f0252-1016">Egy karakterláncértéket egyedi, a szolgáltatás URL-címre.</span><span class="sxs-lookup"><span data-stu-id="f0252-1016">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="f0252-1017">A **keresési dokumentum** kérelem adhat meg, egy adminisztrációs kulcsot vagy egy lekérdezés kulcs `api-key`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1017">The **Lookup Document** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="f0252-1018">Konfigurálnia kell a szolgáltatás nevét, a kérelem URL-címe összeállításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-1018">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="f0252-1019">Beszerezheti a szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján, az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f0252-1019">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="f0252-1020">Lásd: [Azure Search szolgáltatás létrehozása a portálon](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="f0252-1020">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="f0252-1021">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="f0252-1021">**Request Body**</span></span>

<span data-ttu-id="f0252-1022">nincs.</span><span class="sxs-lookup"><span data-stu-id="f0252-1022">None.</span></span>

<span data-ttu-id="f0252-1023">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="f0252-1023">**Response**</span></span>

<span data-ttu-id="f0252-1024">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="f0252-1024">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      field_name: field_value (fields matching the default or specified projection)
    }

<span data-ttu-id="f0252-1025">**Példa**</span><span class="sxs-lookup"><span data-stu-id="f0252-1025">**Example**</span></span>

<span data-ttu-id="f0252-1026">Keresés a dokumentum kulcs '2'</span><span class="sxs-lookup"><span data-stu-id="f0252-1026">Lookup the document that has key '2'</span></span>

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

<span data-ttu-id="f0252-1027">Keresés a dokumentum kulcs "3" OData-szintaxis használatával:</span><span class="sxs-lookup"><span data-stu-id="f0252-1027">Lookup the document that has key '3' using OData syntax:</span></span>

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>

## <a name="count-documents"></a><span data-ttu-id="f0252-1028">A dokumentumok száma</span><span class="sxs-lookup"><span data-stu-id="f0252-1028">Count Documents</span></span>
<span data-ttu-id="f0252-1029">A **száma dokumentumok** művelet lekérdezi egy keresési indexszel dokumentumok száma számát.</span><span class="sxs-lookup"><span data-stu-id="f0252-1029">The **Count Documents** operation retrieves a count of the number of documents in a search index.</span></span> <span data-ttu-id="f0252-1030">A `$count` szintaxisa az OData protokoll egy részét.</span><span class="sxs-lookup"><span data-stu-id="f0252-1030">The `$count` syntax is part of the OData protocol.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

<span data-ttu-id="f0252-1031">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="f0252-1031">**Request**</span></span>

<span data-ttu-id="f0252-1032">HTTPS szolgáltatáskérések szükség.</span><span class="sxs-lookup"><span data-stu-id="f0252-1032">HTTPS is required for service requests.</span></span> <span data-ttu-id="f0252-1033">A **száma dokumentumok** kérelem a GET metódussal lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f0252-1033">The **Count Documents** request can be constructed using the GET method.</span></span>

<span data-ttu-id="f0252-1034">A kérelem URI azonosítója [index neve] közli a szolgáltatást, hogy a megadott index docs gyűjteményének összes elemek számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="f0252-1034">The [index name] in the request URI tells the service to return a count of all items in the docs collection of the specified index.</span></span>

<span data-ttu-id="f0252-1035">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="f0252-1035">`api-version=[string]` (required).</span></span> <span data-ttu-id="f0252-1036">Az előzetes verzió `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1036">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="f0252-1037">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="f0252-1037">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="f0252-1038">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="f0252-1038">**Request Headers**</span></span>

<span data-ttu-id="f0252-1039">Az alábbi lista ismerteti a szükséges és választható kérelemfejlécekben.</span><span class="sxs-lookup"><span data-stu-id="f0252-1039">The following list describes the required and optional request headers.</span></span>

* <span data-ttu-id="f0252-1040">`Accept`: Ezt az értéket kell beállítani `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1040">`Accept`: This value must be set to `text/plain`.</span></span>
* <span data-ttu-id="f0252-1041">`api-key`: A `api-key` hitelesíteni a kérelmet a keresőszolgáltatása szolgál.</span><span class="sxs-lookup"><span data-stu-id="f0252-1041">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="f0252-1042">Egy karakterláncértéket egyedi, a szolgáltatás URL-címre.</span><span class="sxs-lookup"><span data-stu-id="f0252-1042">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="f0252-1043">A **száma dokumentumok** kérelem adhat meg, egy adminisztrációs kulcsot vagy egy lekérdezés kulcs `api-key`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1043">The **Count Documents** request can specify either an admin key or query key for `api-key`.</span></span>

<span data-ttu-id="f0252-1044">Konfigurálnia kell a szolgáltatás nevét, a kérelem URL-címe összeállításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-1044">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="f0252-1045">Beszerezheti a szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján, az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f0252-1045">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="f0252-1046">Lásd: [Azure Search szolgáltatás létrehozása a portálon](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="f0252-1046">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="f0252-1047">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="f0252-1047">**Request Body**</span></span>

<span data-ttu-id="f0252-1048">nincs.</span><span class="sxs-lookup"><span data-stu-id="f0252-1048">None.</span></span>

<span data-ttu-id="f0252-1049">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="f0252-1049">**Response**</span></span>

<span data-ttu-id="f0252-1050">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="f0252-1050">Status Code: 200 OK is returned for a successful response.</span></span>

<span data-ttu-id="f0252-1051">A választörzs a számérték tartalmaz egy egyszerű szöveges formátumú egész számként.</span><span class="sxs-lookup"><span data-stu-id="f0252-1051">The response body contains the count value as an integer formatted in plain text.</span></span>

<a name="Suggestions"></a>

## <a name="suggestions"></a><span data-ttu-id="f0252-1052">Javaslatok</span><span class="sxs-lookup"><span data-stu-id="f0252-1052">Suggestions</span></span>
<span data-ttu-id="f0252-1053">A **javaslatok** művelet lekérdezi a javaslatok generálása részleges kereséshez bemeneti alapján.</span><span class="sxs-lookup"><span data-stu-id="f0252-1053">The **Suggestions** operation retrieves suggestions based on partial search input.</span></span> <span data-ttu-id="f0252-1054">Általában szolgál a keresési mezőbe begépelt javaslatok adnia felhasználók írnak a keresési feltételeket.</span><span class="sxs-lookup"><span data-stu-id="f0252-1054">It's typically used in search boxes to provide type-ahead suggestions as users are entering search terms.</span></span>

<span data-ttu-id="f0252-1055">Cél dokumentumok, arra vonatkozóan, így a javasolt szöveg megismételhető, ha több jelölt dokumentumot a bemeneti azonos keresési feltételeknek megfelelő kérelmek javaslat célja.</span><span class="sxs-lookup"><span data-stu-id="f0252-1055">Suggestion requests aim at suggesting target documents, so the suggested text may be repeated if multiple candidate documents match the same search input.</span></span> <span data-ttu-id="f0252-1056">Használhat `$select` beolvasása más dokumentum mező (a dokumentum kulcsot is beleértve), így állapítható meg, melyik dokumentum minden javaslat forrását.</span><span class="sxs-lookup"><span data-stu-id="f0252-1056">You can use `$select` to retrieve other document fields (including the document key) so that you can tell which document is the source for each suggestion.</span></span>

<span data-ttu-id="f0252-1057">A **javaslatok** művelet GET vagy POST-kérelmet ad ki.</span><span class="sxs-lookup"><span data-stu-id="f0252-1057">A **Suggestions** operation is issued as a GET or POST request.</span></span>

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

<span data-ttu-id="f0252-1058">**Mikor érdemes használni a FELADÁS egy vagy több GET helyett**</span><span class="sxs-lookup"><span data-stu-id="f0252-1058">**When to use POST instead of GET**</span></span>

<span data-ttu-id="f0252-1059">Ha használ HTTP GET hívja a **javaslatok** API-t kell vegye figyelembe, hogy a kérelem URL-címe nem lehet hosszabb 8 KB-os.</span><span class="sxs-lookup"><span data-stu-id="f0252-1059">When you use HTTP GET to call the **Suggestions** API, you need to be aware that the length of the request URL cannot exceed 8 KB.</span></span> <span data-ttu-id="f0252-1060">Ez elég általában a legtöbb alkalmazás esetén.</span><span class="sxs-lookup"><span data-stu-id="f0252-1060">This is usually enough for most applications.</span></span> <span data-ttu-id="f0252-1061">Egyes alkalmazások azonban nagyon nagy lekérdezések, kifejezetten OData szűrőkifejezéseket eredményez.</span><span class="sxs-lookup"><span data-stu-id="f0252-1061">However, some applications produce very large queries, specifically OData filter expressions.</span></span> <span data-ttu-id="f0252-1062">Ezek az alkalmazások a HTTP POST jobb választás, mivel használata esetén lehetővé teszi a GET-nál nagyobb szűrők.</span><span class="sxs-lookup"><span data-stu-id="f0252-1062">For these applications, using HTTP POST is a better choice because it allows larger filters than GET.</span></span> <span data-ttu-id="f0252-1063">A FELADÁS egy vagy több a szűrő záradékok a korlátozás, nem a nyers szűrési karakterláncot, mivel a POST kérelem méretkorlátja nagyjából 16 MB méretét.</span><span class="sxs-lookup"><span data-stu-id="f0252-1063">With POST, the number of clauses in a filter is the limiting factor, not the size of the raw filter string since the request size limit for POST is approximately 16 MB.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-1064">Annak ellenére, hogy a POST kérelem méretkorlátot nagyon nagy, szűrőkifejezéseket önkényesen összetett lehet.</span><span class="sxs-lookup"><span data-stu-id="f0252-1064">Even though the POST request size limit is very large, filter expressions cannot be arbitrarily complex.</span></span> <span data-ttu-id="f0252-1065">Lásd: [OData-kifejezésszintaxist](https://msdn.microsoft.com/library/dn798921.aspx) szűrő összetettsége korlátozásaival kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="f0252-1065">See [OData expression syntax](https://msdn.microsoft.com/library/dn798921.aspx) for more information about filter complexity limitations.</span></span>
> 
> 

<span data-ttu-id="f0252-1066">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="f0252-1066">**Request**</span></span>

<span data-ttu-id="f0252-1067">HTTPS szolgáltatáskérések szükség.</span><span class="sxs-lookup"><span data-stu-id="f0252-1067">HTTPS is required for service requests.</span></span> <span data-ttu-id="f0252-1068">A **javaslatok** kérelem a GET vagy POST metódusok használatával lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f0252-1068">The **Suggestions** request can be constructed using the GET or POST methods.</span></span>

<span data-ttu-id="f0252-1069">A kérelem URI-azonosítója a lekérdezés index neve.</span><span class="sxs-lookup"><span data-stu-id="f0252-1069">The request URI specifies the name of the index to query.</span></span> <span data-ttu-id="f0252-1070">Paraméterek, például a részlegesen bemeneti keresési kifejezés van megadva a lekérdezési karakterlánc GET kérelmek esetében, és a kérés törzsében POST esetén kéri.</span><span class="sxs-lookup"><span data-stu-id="f0252-1070">Parameters, such as the partially input search term, are specified on the query string in the case of GET requests, and in the request body in the case of POST requests.</span></span>

<span data-ttu-id="f0252-1071">Ajánlott eljárásként GET kérelmek létrehozásakor, ne felejtse el [URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) meghatározott lekérdezési paraméterek közvetlenül a REST API hívásakor.</span><span class="sxs-lookup"><span data-stu-id="f0252-1071">As a best practice when creating GET requests, remember to [URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) specific query parameters when calling the REST API directly.</span></span> <span data-ttu-id="f0252-1072">A **javaslatok** műveleteket, ilyenek:</span><span class="sxs-lookup"><span data-stu-id="f0252-1072">For **Suggestions** operations, this includes:</span></span>

* `$filter`
* `highlightPreTag`
* `highlightPostTag`
* `search`

<span data-ttu-id="f0252-1073">A fenti lekérdezési paraméterek csak javasolt, URL-kódolást.</span><span class="sxs-lookup"><span data-stu-id="f0252-1073">URL encoding is only recommended on the above query parameters.</span></span> <span data-ttu-id="f0252-1074">Ha véletlenül URL-kódolására, a teljes lekérdezési karakterlánc (mindent után a?), kérelmek megszakad.</span><span class="sxs-lookup"><span data-stu-id="f0252-1074">If you inadvertently URL-encode the entire query string (everything after the ?), requests will break.</span></span>

<span data-ttu-id="f0252-1075">Emellett URL-kódolást csak akkor szükség a REST API használatával közvetlenül a GET hívásakor.</span><span class="sxs-lookup"><span data-stu-id="f0252-1075">Also, URL encoding is only necessary when calling the REST API directly using GET.</span></span> <span data-ttu-id="f0252-1076">Kódolás nélkül URL-cím szükség, ha hívása **javaslatok** használatával a FELADÁS egy vagy több, vagy használatakor a [.NET ügyféloldali kódtár](https://msdn.microsoft.com/library/dn951165.aspx), amely alkalmazás URL-címet meg.</span><span class="sxs-lookup"><span data-stu-id="f0252-1076">No URL encoding is necessary when calling **Suggestions** using POST, or when using the [.NET client library](https://msdn.microsoft.com/library/dn951165.aspx), which handles URL encoding for you.</span></span>

<span data-ttu-id="f0252-1077">**Lekérdezés-paraméterek**</span><span class="sxs-lookup"><span data-stu-id="f0252-1077">**Query Parameters**</span></span>

<span data-ttu-id="f0252-1078">**Javaslatok** lekérdezés feltételeket, és megadhatja a keresés viselkedését több paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="f0252-1078">**Suggestions** accepts several parameters that provide query criteria and also specify search behavior.</span></span> <span data-ttu-id="f0252-1079">Megadja, hogy ezek a paraméterek az URL-cím lekérdezési karakterlánc meghívásakor **javaslatok** GET keresztül, és a kérés törzsében meghívásakor JSON tulajdonságként **javaslatok** POST keresztül.</span><span class="sxs-lookup"><span data-stu-id="f0252-1079">You provide these parameters in the URL query string when calling **Suggestions** via GET, and as JSON properties in the request body when calling **Suggestions** via POST.</span></span> <span data-ttu-id="f0252-1080">Egyes paraméterek szintaxisa a következő némileg eltérő GET és POST között.</span><span class="sxs-lookup"><span data-stu-id="f0252-1080">The syntax for some parameters is slightly different between GET and POST.</span></span> <span data-ttu-id="f0252-1081">Ezek a különbségek jeleztük megfelelő alatt:</span><span class="sxs-lookup"><span data-stu-id="f0252-1081">These differences are noted as applicable below:</span></span>

<span data-ttu-id="f0252-1082">`search=[string]`– a keresett szöveg felajánlja a lekérdezések használatával.</span><span class="sxs-lookup"><span data-stu-id="f0252-1082">`search=[string]` - the search text to use to suggest queries.</span></span> <span data-ttu-id="f0252-1083">Legalább 1 karakter, és legfeljebb 100 karakter lehet.</span><span class="sxs-lookup"><span data-stu-id="f0252-1083">Must be at least 1 character, and no more than 100 characters.</span></span>

<span data-ttu-id="f0252-1084">`highlightPreTag=[string]`(választható) - karakterlánc címkékhez lefoglalja a találatok kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="f0252-1084">`highlightPreTag=[string]` (optional) - a string tag that prepends to search hits.</span></span> <span data-ttu-id="f0252-1085">Meg kell a `highlightPostTag`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1085">Must be set with `highlightPostTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-1086">Meghívásakor **javaslatok** GET használ, az URL-cím fenntartott karakterek százalék-kódolással kell (például % &#23; helyett).</span><span class="sxs-lookup"><span data-stu-id="f0252-1086">When calling **Suggestions** using GET, reserved characters in the URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="f0252-1087">`highlightPostTag=[string]`(választható) - karakterlánc címkékhez hozzáfűzi a keresési találatok.</span><span class="sxs-lookup"><span data-stu-id="f0252-1087">`highlightPostTag=[string]` (optional) - a string tag that appends to search hits.</span></span> <span data-ttu-id="f0252-1088">Meg kell a `highlightPreTag`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1088">Must be set with `highlightPreTag`.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-1089">Meghívásakor **javaslatok** GET használ, az URL-cím fenntartott karakterek százalék-kódolással kell (például % &#23; helyett).</span><span class="sxs-lookup"><span data-stu-id="f0252-1089">When calling **Suggestions** using GET, reserved characters in the URL must be percent-encoded (for example, %23 instead of #).</span></span>
> 
> 

<span data-ttu-id="f0252-1090">`suggesterName=[string]`-a javaslattevő nevét a `suggesters` gyűjteményt, amely része az index definícióját.</span><span class="sxs-lookup"><span data-stu-id="f0252-1090">`suggesterName=[string]` - the name of the suggester as specified in the `suggesters` collection that's part of the index definition.</span></span> <span data-ttu-id="f0252-1091">A `suggester` meghatározza, hogy melyik mezőkre javasolt lekérdezési kifejezések vizsgálata.</span><span class="sxs-lookup"><span data-stu-id="f0252-1091">A `suggester` determines which fields are scanned for suggested query terms.</span></span> <span data-ttu-id="f0252-1092">Lásd: [Javaslattevők](#Suggesters) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="f0252-1092">See [Suggesters](#Suggesters) for details.</span></span>

<span data-ttu-id="f0252-1093">`fuzzy=[boolean]`(nem kötelező, alapértelmezett = hamis) – Ha értéke igaz, ez az API megkeresi javaslatok még akkor is, ha a keresett szöveg helyettesített vagy hiányzó karakter szerepel.</span><span class="sxs-lookup"><span data-stu-id="f0252-1093">`fuzzy=[boolean]` (optional, default = false) - when set to true this API will find suggestions even if there's a substituted or missing character in the search text.</span></span> <span data-ttu-id="f0252-1094">Amíg ez bizonyos esetekben jobb élményt nyújt a teljesítményt, intelligens javaslat keresések lassabban futnak, és több erőforrást, származik.</span><span class="sxs-lookup"><span data-stu-id="f0252-1094">While this provides a better experience in some scenarios it comes at a performance cost as fuzzy suggestion searches are slower and consume more resources.</span></span>

<span data-ttu-id="f0252-1095">`searchFields=[string]`(nem kötelező) – a megadott keresési szöveg keresése vesszővel elválasztott mezők nevei a listában.</span><span class="sxs-lookup"><span data-stu-id="f0252-1095">`searchFields=[string]` (optional) - the list of comma-separated field names to search for the specified search text.</span></span> <span data-ttu-id="f0252-1096">Célmező javaslatok engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="f0252-1096">Target fields must be enabled for suggestions.</span></span>

<span data-ttu-id="f0252-1097">`$top=#`(nem kötelező, alapértelmezett = 5)-javaslatok beolvasása száma.</span><span class="sxs-lookup"><span data-stu-id="f0252-1097">`$top=#` (optional, default = 5) - the number of suggestions to retrieve.</span></span> <span data-ttu-id="f0252-1098">1 és 100 közötti számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f0252-1098">Must be a number between 1 and 100.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-1099">Meghívásakor **javaslatok** POST használ, ez a paraméter neve `top` helyett `$top`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1099">When calling **Suggestions** using POST, this parameter is named `top` instead of `$top`.</span></span>
> 
> 

<span data-ttu-id="f0252-1100">`$filter=[string]`(nem kötelező) – a dokumentumok szűrő kifejezés javaslatok figyelembe venni.</span><span class="sxs-lookup"><span data-stu-id="f0252-1100">`$filter=[string]` (optional) - an expression that filters the documents considered for suggestions.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-1101">Meghívásakor **javaslatok** POST használ, ez a paraméter neve `filter` helyett `$filter`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1101">When calling **Suggestions** using POST, this parameter is named `filter` instead of `$filter`.</span></span>
> 
> 

<span data-ttu-id="f0252-1102">`$orderby=[string]`(választható) - szűrje az eredményeket kifejezések vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="f0252-1102">`$orderby=[string]` (optional) - a list of comma-separated expressions to sort the results by.</span></span> <span data-ttu-id="f0252-1103">Minden egyes kifejezés lehet, vagy egy mező nevét, vagy a hívás a `geo.distance()` függvény.</span><span class="sxs-lookup"><span data-stu-id="f0252-1103">Each expression can be either a field name or a call to the `geo.distance()` function.</span></span> <span data-ttu-id="f0252-1104">Minden egyes kifejezés követhetnek `asc` jelezni a növekvő, és `desc` csökkenő jelzi.</span><span class="sxs-lookup"><span data-stu-id="f0252-1104">Each expression can be followed by `asc` to indicated ascending, and `desc` to indicate descending.</span></span> <span data-ttu-id="f0252-1105">Az alapértelmezett növekvő sorrend megadásához.</span><span class="sxs-lookup"><span data-stu-id="f0252-1105">The default is ascending order.</span></span> <span data-ttu-id="f0252-1106">Egy legfeljebb 32 záradékok `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1106">There is a limit of 32 clauses for `$orderby`.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-1107">Meghívásakor **javaslatok** POST használ, ez a paraméter neve `orderby` helyett `$orderby`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1107">When calling **Suggestions** using POST, this parameter is named `orderby` instead of `$orderby`.</span></span>
> 
> 

<span data-ttu-id="f0252-1108">`$select=[string]`(választható) - vesszővel tagolt mezők beolvasásához listáját.</span><span class="sxs-lookup"><span data-stu-id="f0252-1108">`$select=[string]` (optional) - a list of comma-separated fields to retrieve.</span></span> <span data-ttu-id="f0252-1109">Ha nincs megadva, csak a dokumentum kulcs és javaslat szöveget ad vissza.</span><span class="sxs-lookup"><span data-stu-id="f0252-1109">If unspecified, only the document key and suggestion text is returned.</span></span> <span data-ttu-id="f0252-1110">Minden mező úgy, hogy ez a paraméter explicit módon kérhet `*`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1110">You can explicitly request all fields by setting this parameter to `*`.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-1111">Meghívásakor **javaslatok** POST használ, ez a paraméter neve `select` helyett `$select`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1111">When calling **Suggestions** using POST, this parameter is named `select` instead of `$select`.</span></span>
> 
> 

<span data-ttu-id="f0252-1112">`minimumCoverage`(nem kötelező, az alapértelmezett érték a 80-as) – 0 és 100 százalékos aránya az indexet, át kell gondolni ahhoz, hogy a lekérdezés kell jelenteni sikeres, a javaslatok lekérdezés jelző közötti szám.</span><span class="sxs-lookup"><span data-stu-id="f0252-1112">`minimumCoverage` (optional, defaults to 80) - a number between 0 and 100 indicating the percentage of the index that must be covered by a suggestions query in order for the query to be reported as a success.</span></span> <span data-ttu-id="f0252-1113">Alapértelmezés szerint az index legalább 80 %-át elérhetőnek kell lennie, vagy `Suggest` 503-as HTTP-állapotkód: ad vissza.</span><span class="sxs-lookup"><span data-stu-id="f0252-1113">By default, at least 80% of the index must be available or `Suggest` will return HTTP status code 503.</span></span> <span data-ttu-id="f0252-1114">Ha `minimumCoverage` és `Suggest` sikeres, térjen vissza a 200-as HTTP, és tartalmazzák a `@search.coverage` a válaszban az index a lekérdezésben szereplő százalékos értékét.</span><span class="sxs-lookup"><span data-stu-id="f0252-1114">If you set `minimumCoverage` and `Suggest` succeeds, it will return HTTP 200 and include a `@search.coverage` value in the response indicating the percentage of the index that was included in the query.</span></span>

> [!NOTE]
> <span data-ttu-id="f0252-1115">A paraméter értéke 100-nál kisebb search rendelkezésre állását, még a szolgáltatások csak egy replika azért hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="f0252-1115">Setting this parameter to a value lower than 100 can be useful for ensuring search availability even for services with only one replica.</span></span> <span data-ttu-id="f0252-1116">Azonban nem minden egyező javaslatok garantáltan szerepel az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="f0252-1116">However, not all matching suggestions are guaranteed to be present in the results.</span></span> <span data-ttu-id="f0252-1117">Ha visszaírási fontosabb az alkalmazáshoz, mint a rendelkezésre állási, akkor a legjobb, ha nem alacsonyabb `minimumCoverage` a 80-as alapértelmezett érték alá.</span><span class="sxs-lookup"><span data-stu-id="f0252-1117">If recall is more important to your application than availability, then it's best not to lower `minimumCoverage` below its default value of 80.</span></span>
> 
> 

<span data-ttu-id="f0252-1118">`api-version=[string]`(kötelező).</span><span class="sxs-lookup"><span data-stu-id="f0252-1118">`api-version=[string]` (required).</span></span> <span data-ttu-id="f0252-1119">Az előzetes verzió `api-version=2015-02-28-Preview`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1119">The preview version is `api-version=2015-02-28-Preview`.</span></span> <span data-ttu-id="f0252-1120">Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.</span><span class="sxs-lookup"><span data-stu-id="f0252-1120">See [Search Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) for details and alternative versions.</span></span>

<span data-ttu-id="f0252-1121">Megjegyzés: A művelet a `api-version` egy lekérdezési paraméter, függetlenül attól, hogy meghívja az URL-címben megadott **javaslatok** GET vagy POST.</span><span class="sxs-lookup"><span data-stu-id="f0252-1121">Note: For this operation, the `api-version` is specified as a query parameter in the URL regardless of whether you call **Suggestions** with GET or POST.</span></span>

<span data-ttu-id="f0252-1122">**Kérelem fejlécei**</span><span class="sxs-lookup"><span data-stu-id="f0252-1122">**Request Headers**</span></span>

<span data-ttu-id="f0252-1123">Az alábbi lista ismerteti a szükséges és választható kérelemfejléc</span><span class="sxs-lookup"><span data-stu-id="f0252-1123">The following list describes the required and optional request headers</span></span>

* <span data-ttu-id="f0252-1124">`api-key`: A `api-key` hitelesíteni a kérelmet a keresőszolgáltatása szolgál.</span><span class="sxs-lookup"><span data-stu-id="f0252-1124">`api-key`: The `api-key` is used to authenticate the request to your Search service.</span></span> <span data-ttu-id="f0252-1125">Egy karakterláncértéket egyedi, a szolgáltatás URL-címre.</span><span class="sxs-lookup"><span data-stu-id="f0252-1125">It is a string value, unique to your service URL.</span></span> <span data-ttu-id="f0252-1126">A **javaslatok** kérelem adhat meg, egy adminisztrációs kulcsot vagy egy lekérdezés kulcsot a `api-key`.</span><span class="sxs-lookup"><span data-stu-id="f0252-1126">The **Suggestions** request can specify either an admin key or query key as the `api-key`.</span></span>

<span data-ttu-id="f0252-1127">Konfigurálnia kell a szolgáltatás nevét, a kérelem URL-címe összeállításához.</span><span class="sxs-lookup"><span data-stu-id="f0252-1127">You will also need the service name to construct the request URL.</span></span> <span data-ttu-id="f0252-1128">Beszerezheti a szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján, az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f0252-1128">You can get the service name and `api-key` from your service dashboard in the Azure Portal.</span></span> <span data-ttu-id="f0252-1129">Lásd: [Azure Search szolgáltatás létrehozása a portálon](search-create-service-portal.md) navigációs segítségét.</span><span class="sxs-lookup"><span data-stu-id="f0252-1129">See [Create an Azure Search service in the portal](search-create-service-portal.md) for page navigation help.</span></span>

<span data-ttu-id="f0252-1130">**Kérés törzsében**</span><span class="sxs-lookup"><span data-stu-id="f0252-1130">**Request Body**</span></span>

<span data-ttu-id="f0252-1131">A GET: nincs.</span><span class="sxs-lookup"><span data-stu-id="f0252-1131">For GET: None.</span></span>

<span data-ttu-id="f0252-1132">A FELADÁS egy vagy több:</span><span class="sxs-lookup"><span data-stu-id="f0252-1132">For POST:</span></span>

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

<span data-ttu-id="f0252-1133">**Válasz**</span><span class="sxs-lookup"><span data-stu-id="f0252-1133">**Response**</span></span>

<span data-ttu-id="f0252-1134">Állapotkód: 200 OK visszaküldött a sikeres válasz.</span><span class="sxs-lookup"><span data-stu-id="f0252-1134">Status Code: 200 OK is returned for a successful response.</span></span>

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

<span data-ttu-id="f0252-1135">Ha a leképezés beállítás segítségével kérhető le a mezők szerepelnek a tömb egyes elemei:</span><span class="sxs-lookup"><span data-stu-id="f0252-1135">If the projection option is used to retrieve fields they are included in each element of the array:</span></span>

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

<span data-ttu-id="f0252-1136">**Példa**</span><span class="sxs-lookup"><span data-stu-id="f0252-1136">**Example**</span></span>

<span data-ttu-id="f0252-1137">Ahol a részleges kereséshez bemeneti az "lux" 5 javaslatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="f0252-1137">Retrieve 5 suggestions where the partial search input is 'lux'</span></span>

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
