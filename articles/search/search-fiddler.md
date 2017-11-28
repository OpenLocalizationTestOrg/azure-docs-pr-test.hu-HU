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
# <a name="use-fiddler-tooevaluate-and-test-azure-search-rest-apis"></a><span data-ttu-id="61816-103">Használja a Fiddler tooevaluate és tesztelése az Azure Search REST API-k</span><span class="sxs-lookup"><span data-stu-id="61816-103">Use Fiddler tooevaluate and test Azure Search REST APIs</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="61816-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="61816-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="61816-105">Keresési ablak</span><span class="sxs-lookup"><span data-stu-id="61816-105">Search Explorer</span></span>](search-explorer.md)
> * [<span data-ttu-id="61816-106">Fiddler</span><span class="sxs-lookup"><span data-stu-id="61816-106">Fiddler</span></span>](search-fiddler.md)
> * [<span data-ttu-id="61816-107">.NET</span><span class="sxs-lookup"><span data-stu-id="61816-107">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="61816-108">REST</span><span class="sxs-lookup"><span data-stu-id="61816-108">REST</span></span>](search-query-rest-api.md)
>
>

<span data-ttu-id="61816-109">Ez a cikk azt ismerteti, hogyan toouse Fiddler, valamint egy [Telerik ingyenesen letölthető](http://www.telerik.com/fiddler), tooissue HTTP kérelmeket tooand válaszok megtekintése hello Azure Search REST API használatával anélkül, hogy toowrite összes kódot.</span><span class="sxs-lookup"><span data-stu-id="61816-109">This article explains how toouse Fiddler, available as a [free download from Telerik](http://www.telerik.com/fiddler), tooissue HTTP requests tooand view responses using hello Azure Search REST API, without having toowrite any code.</span></span> <span data-ttu-id="61816-110">Az Azure Search teljes körűen felügyelt, üzemeltetett felhőalapú keresőszolgáltatás, amely egyszerűen programozható .NET és REST API-kon keresztül.</span><span class="sxs-lookup"><span data-stu-id="61816-110">Azure Search is fully-managed, hosted cloud search service on Microsoft Azure, easily programmable through .NET and REST APIs.</span></span> <span data-ttu-id="61816-111">hello Azure Search szolgáltatás REST API-jainak dokumentációja a [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).</span><span class="sxs-lookup"><span data-stu-id="61816-111">hello Azure Search service REST APIs are documented on [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).</span></span>

<span data-ttu-id="61816-112">A lépéseket követve hello lesz index létrehozása, dokumentumok, lekérdezés hello index, majd szolgáltatási adatokat lekérdezés hello rendszer feltöltése.</span><span class="sxs-lookup"><span data-stu-id="61816-112">In hello following steps, you'll create an index, upload documents, query hello index, and then query hello system for service information.</span></span>

<span data-ttu-id="61816-113">Ezek a lépések toocomplete, szüksége lesz egy Azure Search szolgáltatást és `api-key`.</span><span class="sxs-lookup"><span data-stu-id="61816-113">toocomplete these steps, you will need an Azure Search service and `api-key`.</span></span> <span data-ttu-id="61816-114">Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) tooget indításának kapcsolatos utasításokat.</span><span class="sxs-lookup"><span data-stu-id="61816-114">See [Create an Azure Search service in hello portal](search-create-service-portal.md) for instructions on how tooget started.</span></span>

## <a name="create-an-index"></a><span data-ttu-id="61816-115">Index létrehozása</span><span class="sxs-lookup"><span data-stu-id="61816-115">Create an index</span></span>
1. <span data-ttu-id="61816-116">Indítsa el a Fiddlert.</span><span class="sxs-lookup"><span data-stu-id="61816-116">Start Fiddler.</span></span> <span data-ttu-id="61816-117">A hello **fájl** menüben kapcsolja ki a **forgalom rögzítése** toohide HTTP-tevékenység, amely független toohello aktuális feladatot.</span><span class="sxs-lookup"><span data-stu-id="61816-117">On hello **File** menu, turn off **Capture Traffic** toohide extraneous HTTP activity that is unrelated toohello current task.</span></span>
2. <span data-ttu-id="61816-118">A hello **szerkesztő** lapon állítson össze a kérelmeket, amelyek a következő képernyőfelvétel hello tűnik.</span><span class="sxs-lookup"><span data-stu-id="61816-118">On hello **Composer** tab, you'll formulate a request that looks like hello following screen shot.</span></span>

      ![][1]
3. <span data-ttu-id="61816-119">Válassza a **PUT** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="61816-119">Select **PUT**.</span></span>
4. <span data-ttu-id="61816-120">Adjon meg egy URL-címet, amely meghatározza a hello szolgáltatás URL-címe, attribútumainak és hello api-verzió.</span><span class="sxs-lookup"><span data-stu-id="61816-120">Enter a URL that specifies hello service URL, request attributes, and hello api-version.</span></span> <span data-ttu-id="61816-121">Néhány mutatók tookeep figyelembe vételével:</span><span class="sxs-lookup"><span data-stu-id="61816-121">A few pointers tookeep in mind:</span></span>

   * <span data-ttu-id="61816-122">HTTPS használata hello előtag.</span><span class="sxs-lookup"><span data-stu-id="61816-122">Use HTTPS as hello prefix.</span></span>
   * <span data-ttu-id="61816-123">A kérelemattribútum az „/indexes/hotels”.</span><span class="sxs-lookup"><span data-stu-id="61816-123">Request attribute is "/indexes/hotels".</span></span> <span data-ttu-id="61816-124">Ez egy "hotels" nevű index alapján keresési toocreate.</span><span class="sxs-lookup"><span data-stu-id="61816-124">This tells Search toocreate an index named 'hotels'.</span></span>
   * <span data-ttu-id="61816-125">Az api-version kisbetűvel van írva, és a következő formában van megadva: „?api-version=2016-09-01”.</span><span class="sxs-lookup"><span data-stu-id="61816-125">Api-version is lowercase, specified as "?api-version=2016-09-01".</span></span> <span data-ttu-id="61816-126">Az API-verziók azért fontosak, mert az Azure Search rendszeresen telepíti a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="61816-126">API versions are important because Azure Search deploys updates regularly.</span></span> <span data-ttu-id="61816-127">Ritka esetekben a szolgáltatás frissítése a legfrissebb módosítása toohello API vezethetnek.</span><span class="sxs-lookup"><span data-stu-id="61816-127">On rare occasions, a service update may introduce a breaking change toohello API.</span></span> <span data-ttu-id="61816-128">Emiatt az Azure Search számára minden kérelemnél meg kell adni az API-verziót, hogy Ön mindig teljes mértékben kézben tudja tartani, hogy melyik verzió van használatban.</span><span class="sxs-lookup"><span data-stu-id="61816-128">For this reason, Azure Search requires an api-version on each request so that you are in full control over which one is used.</span></span>

     <span data-ttu-id="61816-129">hello teljes URL-címet a következő példa hasonló toohello kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="61816-129">hello full URL should look similar toohello following example.</span></span>

             https://my-app.search.windows.net/indexes/hotels?api-version=2016-09-01
5. <span data-ttu-id="61816-130">Adja meg a hello kérelemfejléc hello állomás és api-kulcs cseréje, amelyek az Ön szolgáltatásában érvényes értékekkel.</span><span class="sxs-lookup"><span data-stu-id="61816-130">Specify hello request header, replacing hello host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
6. <span data-ttu-id="61816-131">A kérelem törzse területre illessze be a hello index definícióját alkotó mezőket hello.</span><span class="sxs-lookup"><span data-stu-id="61816-131">In Request Body, paste in hello fields that make up hello index definition.</span></span>

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
7. <span data-ttu-id="61816-132">Kattintson az **Execute** (Végrehajtás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="61816-132">Click **Execute**.</span></span>

<span data-ttu-id="61816-133">Néhány másodpercen belül egy 201-es HTTP-válasz hello munkamenetlistában megjelenik, ami hello index sikeresen létrejött.</span><span class="sxs-lookup"><span data-stu-id="61816-133">In a few seconds, you should see an HTTP 201 response in hello session list, indicating hello index was created successfully.</span></span>

<span data-ttu-id="61816-134">Ha HTTP 504, ellenőrizze a hello URL-címben a HTTPS PROTOKOLLT.</span><span class="sxs-lookup"><span data-stu-id="61816-134">If you get HTTP 504, verify hello URL specifies HTTPS.</span></span> <span data-ttu-id="61816-135">Ha megjelenik a HTTP 400-as vagy 404-es, a szervezet tooverify nem volt-e beillesztési hibák hello kérés ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="61816-135">If you see HTTP 400 or 404, check hello request body tooverify there were no copy-paste errors.</span></span> <span data-ttu-id="61816-136">Egy HTTP 403 általában hello api-kulccsal (Érvénytelen a kulcs vagy szintaktikai hiba van hogyan hello api-kulcs van megadva) kapcsolatos problémát jelez.</span><span class="sxs-lookup"><span data-stu-id="61816-136">An HTTP 403 typically indicates a problem with hello api-key (either an invalid key or a syntax problem with how hello api-key is specified).</span></span>

## <a name="load-documents"></a><span data-ttu-id="61816-137">Dokumentumok betöltése</span><span class="sxs-lookup"><span data-stu-id="61816-137">Load documents</span></span>
<span data-ttu-id="61816-138">A hello **szerkesztő** lapon a kérelem toopost dokumentumok hello következőképpen néznek.</span><span class="sxs-lookup"><span data-stu-id="61816-138">On hello **Composer** tab, your request toopost documents will look like hello following.</span></span> <span data-ttu-id="61816-139">hello kérelem törzse hello 4 szállodák hello keresési adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="61816-139">hello body of hello request contains hello search data for 4 hotels.</span></span>

   ![][2]

1. <span data-ttu-id="61816-140">Válassza a **POST** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="61816-140">Select **POST**.</span></span>
2. <span data-ttu-id="61816-141">Adjon meg egy olyan URL-címet, amely a HTTPS előtaggal kezdődik, amelyet a szolgáltatás URL-címe, majd az „/indexes/<'indexnév'>/docs/index?api-version=2016-09-01” karakterlánc követ.</span><span class="sxs-lookup"><span data-stu-id="61816-141">Enter a URL that starts with HTTPS, followed by your service URL, followed by "/indexes/<'indexname'>/docs/index?api-version=2016-09-01".</span></span> <span data-ttu-id="61816-142">hello teljes URL-címet a következő példa hasonló toohello kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="61816-142">hello full URL should look similar toohello following example.</span></span>

         https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
3. <span data-ttu-id="61816-143">Kérelem fejléce kell lennie hello azonos mint korábban.</span><span class="sxs-lookup"><span data-stu-id="61816-143">Request Header should be hello same as before.</span></span> <span data-ttu-id="61816-144">Ne feledje, hogy Ön helyett hello állomás és api-kulcs értékeket, amelyek az Ön szolgáltatásában érvényes.</span><span class="sxs-lookup"><span data-stu-id="61816-144">Remember that you replaced hello host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. <span data-ttu-id="61816-145">hello kérelem törzse négy dokumentumok toobe hozzáadott toohello szállodák indexet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="61816-145">hello Request Body contains four documents toobe added toohello hotels index.</span></span>

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
5. <span data-ttu-id="61816-146">Kattintson az **Execute** (Végrehajtás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="61816-146">Click **Execute**.</span></span>

<span data-ttu-id="61816-147">Néhány másodpercen belül meg kell jelennie egy 200-as HTTP-válasz hello munkamenetlistában.</span><span class="sxs-lookup"><span data-stu-id="61816-147">In a few seconds, you should see an HTTP 200 response in hello session list.</span></span> <span data-ttu-id="61816-148">Ez azt jelzi, hogy hello dokumentumok sikeresen létrejöttek.</span><span class="sxs-lookup"><span data-stu-id="61816-148">This indicates hello documents were created successfully.</span></span> <span data-ttu-id="61816-149">Ha a 207-es, legalább egy dokumentum tooupload nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="61816-149">If you get a 207, at least one document failed tooupload.</span></span> <span data-ttu-id="61816-150">Ha a 404-es, hello fejléc vagy a hello kérés törzsében szintaktikai hiba van.</span><span class="sxs-lookup"><span data-stu-id="61816-150">If you get a 404, you have a syntax error in either hello header or body of hello request.</span></span>

## <a name="query-hello-index"></a><span data-ttu-id="61816-151">Lekérdezés hello indexe</span><span class="sxs-lookup"><span data-stu-id="61816-151">Query hello index</span></span>
<span data-ttu-id="61816-152">Most, hogy az index és a dokumentumok is betöltődtek, lekérdezheti őket.</span><span class="sxs-lookup"><span data-stu-id="61816-152">Now that an index and documents are loaded, you can issue queries against them.</span></span>  <span data-ttu-id="61816-153">A hello **szerkesztő** lapon egy **beolvasása** a szolgáltatást lekérdező parancs a következő képernyőfelvétel hasonló toohello fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="61816-153">On hello **Composer** tab, a **GET** command that queries your service will look similar toohello following screen shot.</span></span>

   ![][3]

1. <span data-ttu-id="61816-154">Válassza a **GET** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="61816-154">Select **GET**.</span></span>
2. <span data-ttu-id="61816-155">Adjon meg egy olyan URL-címet, amely a HTTPS előtaggal kezdődik, amelyet a szolgáltatási URL, majd az „/indexes/<'indexname'>/docs?” karakterlánc, végül a lekérdezési paraméterek követnek.</span><span class="sxs-lookup"><span data-stu-id="61816-155">Enter a URL that starts with HTTPS, followed by your service URL, followed by "/indexes/<'indexname'>/docs?", followed by query parameters.</span></span> <span data-ttu-id="61816-156">Példaképpen használja a következő URL-cím, egy, az Ön szolgáltatásában érvényes hello minta állomásnév cseréje hello.</span><span class="sxs-lookup"><span data-stu-id="61816-156">By way of example, use hello following URL, replacing hello sample host name with one that is valid for your service.</span></span>

         https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2016-09-01

   <span data-ttu-id="61816-157">Ez a lekérdezés a "motel" hello kifejezés keres, és értékkorlátozó kategóriákat értékelések.</span><span class="sxs-lookup"><span data-stu-id="61816-157">This query searches on hello term “motel” and retrieves facet categories for ratings.</span></span>
3. <span data-ttu-id="61816-158">Kérelem fejléce kell lennie hello azonos mint korábban.</span><span class="sxs-lookup"><span data-stu-id="61816-158">Request Header should be hello same as before.</span></span> <span data-ttu-id="61816-159">Ne feledje, hogy Ön helyett hello állomás és api-kulcs értékeket, amelyek az Ön szolgáltatásában érvényes.</span><span class="sxs-lookup"><span data-stu-id="61816-159">Remember that you replaced hello host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444

<span data-ttu-id="61816-160">hello válaszkód a 200-as kell lennie, és hello válasz kimenete a következő képernyőfelvétel hasonló toohello kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="61816-160">hello response code should be 200, and hello response output should look similar toohello following screen shot.</span></span>

   ![][4]

<span data-ttu-id="61816-161">hello következő példalekérdezés származik hello [Search-Index művelet (Azure Search API)](http://msdn.microsoft.com/library/dn798927.aspx) az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="61816-161">hello following example query is from hello [Search Index operation (Azure Search API)](http://msdn.microsoft.com/library/dn798927.aspx) on MSDN.</span></span> <span data-ttu-id="61816-162">Ebben a témakörben hello mintalekérdezéseket számos közé tartozik a szóközt tartalmaz, amely a Fiddler nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="61816-162">Many of hello example queries in this topic include spaces, which are not allowed in Fiddler.</span></span> <span data-ttu-id="61816-163">Minden szóközt cseréljen le a + karakterre, mielőtt beillesztené a hello előtt hello lekérdezés Fiddler használatával történő lekérdezés-karakterlánc hossza.</span><span class="sxs-lookup"><span data-stu-id="61816-163">Replace each space with a + character before pasting in hello query string before attempting hello query in Fiddler.</span></span>

<span data-ttu-id="61816-164">**A szóközök cseréje előtt:**</span><span class="sxs-lookup"><span data-stu-id="61816-164">**Before spaces are replaced:**</span></span>

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01

<span data-ttu-id="61816-165">**A szóközök + jelre cserélése után:**</span><span class="sxs-lookup"><span data-stu-id="61816-165">**After spaces are replaced with +:**</span></span>

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2016-09-01

## <a name="query-hello-system"></a><span data-ttu-id="61816-166">Lekérdezés hello rendszer</span><span class="sxs-lookup"><span data-stu-id="61816-166">Query hello system</span></span>
<span data-ttu-id="61816-167">Hello rendszer tooget számát, valamint a tárolási dokumentumfelhasználás is lekérheti.</span><span class="sxs-lookup"><span data-stu-id="61816-167">You can also query hello system tooget document counts and storage consumption.</span></span> <span data-ttu-id="61816-168">A hello **szerkesztő** lapon a kérelem hasonló toohello következő jelenik meg, és hello válasz szerepleni fog hello számát a dokumentumok és a felhasznált lemezterület mérete.</span><span class="sxs-lookup"><span data-stu-id="61816-168">On hello **Composer** tab, your request will look similar toohello following, and hello response will return a count for hello number of documents and space used.</span></span>

 ![][5]

1. <span data-ttu-id="61816-169">Válassza a **GET** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="61816-169">Select **GET**.</span></span>
2. <span data-ttu-id="61816-170">Adjon meg egy olyan URL-címet, amely tartalmazza a szolgáltatás URL-címét, majd az „/indexes/hotels/stats?api-version=2016-09-01” karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="61816-170">Enter a URL that includes your service URL, followed by "/indexes/hotels/stats?api-version=2016-09-01":</span></span>

         https://my-app.search.windows.net/indexes/hotels/stats?api-version=2016-09-01
3. <span data-ttu-id="61816-171">Adja meg a hello kérelemfejléc hello állomás és api-kulcs cseréje, amelyek az Ön szolgáltatásában érvényes értékekkel.</span><span class="sxs-lookup"><span data-stu-id="61816-171">Specify hello request header, replacing hello host and api-key with values that are valid for your service.</span></span>

         User-Agent: Fiddler
         host: my-app.search.windows.net
         content-type: application/json
         api-key: 1111222233334444
4. <span data-ttu-id="61816-172">Hagyja üresen hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="61816-172">Leave hello request body empty.</span></span>
5. <span data-ttu-id="61816-173">Kattintson az **Execute** (Végrehajtás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="61816-173">Click **Execute**.</span></span> <span data-ttu-id="61816-174">Meg kell jelennie egy hello munkamenetlistában a 200-as HTTP-állapotkód:.</span><span class="sxs-lookup"><span data-stu-id="61816-174">You should see an HTTP 200 status code in hello session list.</span></span> <span data-ttu-id="61816-175">Válassza ki a parancshoz közzétett hello bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="61816-175">Select hello entry posted for your command.</span></span>
6. <span data-ttu-id="61816-176">Kattintson a hello **ellenőrök** lapra, majd hello **fejlécek** fülre, majd jelölje ki hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="61816-176">Click hello **Inspectors** tab, click hello **Headers** tab, and then select hello JSON format.</span></span> <span data-ttu-id="61816-177">Meg kell jelennie hello dokumentumok száma és a tárhely méretét (kilobájtban).</span><span class="sxs-lookup"><span data-stu-id="61816-177">You should see hello document count and storage size (in KB).</span></span>

## <a name="next-steps"></a><span data-ttu-id="61816-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61816-178">Next steps</span></span>
<span data-ttu-id="61816-179">Lásd: [Azure a Search szolgáltatás kezelése](search-manage.md) egy kódot nem megközelítés toomanaging és az Azure Search használatával.</span><span class="sxs-lookup"><span data-stu-id="61816-179">See [Manage your Search service on Azure](search-manage.md) for a no-code approach toomanaging and using Azure Search.</span></span>

<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
