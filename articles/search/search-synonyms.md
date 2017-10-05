---
pageTitle: Synonyms in Azure Search (preview) | Microsoft Docs
description: "A szinonimák (előzetes verzió) szolgáltatás, az Azure Search REST API felfedett előzetes dokumentációjában talál."
services: search
documentationCenter: 
authors: mhko
manager: pablocas
editor: 
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/07/2016
ms.author: nateko
ms.openlocfilehash: 739a0ad77c68ea74ec25bc80c7539ac8b3f18201
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="synonyms-in-azure-search-preview"></a><span data-ttu-id="ff47a-102">Szinonimák az Azure Search (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="ff47a-102">Synonyms in Azure Search (preview)</span></span>

<span data-ttu-id="ff47a-103">Szinonimák a keresőprogramok, amely implicit módon bontsa ki a lekérdezés hatókörét a felhasználónak nem kell ténylegesen adja meg a kifejezés nem megfelelő használati társítani.</span><span class="sxs-lookup"><span data-stu-id="ff47a-103">Synonyms in search engines associate equivalent terms that implicitly expand the scope of a query, without the user having to actually provide the term.</span></span> <span data-ttu-id="ff47a-104">Például a kifejezés "kutya" és a szinonima rendelését "canine" és "ételadagot", "kutya" tartalmazó dokumentumokat kap, "canine" vagy "ételadagot" is tartozik a lekérdezés hatókörén belül.</span><span class="sxs-lookup"><span data-stu-id="ff47a-104">For example, given the term "dog" and synonym associations of "canine" and "puppy", any documents containing "dog", "canine" or "puppy" will fall within the scope of the query.</span></span>

<span data-ttu-id="ff47a-105">Az Azure Search szinonimát bővítése időben történik.</span><span class="sxs-lookup"><span data-stu-id="ff47a-105">In Azure Search, synonym expansion is done at query time.</span></span> <span data-ttu-id="ff47a-106">Egy szolgáltatás együtt nincs meglévő műveletek megszakadását szinonimát leképezéseket adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="ff47a-106">You can add synonym maps to a service with no disruption to existing operations.</span></span> <span data-ttu-id="ff47a-107">Hozzáadhat egy **synonymMaps** anélkül, hogy az index újraépítése mező definíció tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="ff47a-107">You can add a  **synonymMaps** property to a field definition without having to rebuild the index.</span></span> <span data-ttu-id="ff47a-108">További információkért lásd: [Index frissítése](https://docs.microsoft.com/rest/api/searchservice/update-index).</span><span class="sxs-lookup"><span data-stu-id="ff47a-108">For more information, see [Update Index](https://docs.microsoft.com/rest/api/searchservice/update-index).</span></span>

## <a name="feature-availability"></a><span data-ttu-id="ff47a-109">Szolgáltatások rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="ff47a-109">Feature availability</span></span>

<span data-ttu-id="ff47a-110">A szinonimák funkció jelenleg előzetes verzióban érhetők, és csak akkor támogatott a legújabb előzetes api-verzió (api-version = 2016 09-01. dátumú előnézeti).</span><span class="sxs-lookup"><span data-stu-id="ff47a-110">The synonyms feature is currently in preview and only supported in the latest preview api-version (api-version=2016-09-01-Preview).</span></span> <span data-ttu-id="ff47a-111">Jelenleg nincs Azure Portal-támogatás.</span><span class="sxs-lookup"><span data-stu-id="ff47a-111">There is no Azure portal support at this time.</span></span> <span data-ttu-id="ff47a-112">Az API-verzió van megadva a kérés, mert is lehet kombinálni általánosan elérhető (GA), és ugyanahhoz az alkalmazáshoz az API-k előzetes.</span><span class="sxs-lookup"><span data-stu-id="ff47a-112">Because the API version is specified on the request, it's possible to combine generally available (GA) and preview APIs in the same app.</span></span> <span data-ttu-id="ff47a-113">Azonban előnézeti API-k nincsenek az SLA-t és a szolgáltatások módosíthatja, ezért nem javasoljuk azok az éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="ff47a-113">However, preview APIs are not under SLA and features may change, so we do not recommend using them in production applications.</span></span>

## <a name="how-to-use-synonyms-in-azure-search"></a><span data-ttu-id="ff47a-114">Az Azure search szinonimák használata</span><span class="sxs-lookup"><span data-stu-id="ff47a-114">How to use synonyms in Azure search</span></span>

<span data-ttu-id="ff47a-115">Az Azure Search szinonimát támogatása, amely meghatározza, és töltse fel a szolgáltatás a szinonima maps alapul.</span><span class="sxs-lookup"><span data-stu-id="ff47a-115">In Azure Search, synonym support is based on synonym maps that you define and upload to your service.</span></span> <span data-ttu-id="ff47a-116">A maps képeznek egy független erőforrás (például az adatforrásokat, illetve indexek), és használhatja a keresési szolgáltatás index bármely kereshető mező.</span><span class="sxs-lookup"><span data-stu-id="ff47a-116">These maps constitute an independent resource (like indexes or data sources), and can be used by any searchable field in any index in your search service.</span></span>

<span data-ttu-id="ff47a-117">Szinonimát képezi le, és indexek egymástól függetlenül megmaradjanak.</span><span class="sxs-lookup"><span data-stu-id="ff47a-117">Synonym maps and indexes are maintained independently.</span></span> <span data-ttu-id="ff47a-118">Miután szinonimát térképre határozza meg, és töltse fel azt a szolgáltatást, a mező szinonimát funkciót nevű új tulajdonsággal hozzáadásával engedélyezheti **synonymMaps** mező definíciójában.</span><span class="sxs-lookup"><span data-stu-id="ff47a-118">Once you define a synonym map and upload it to your service, you can enable the synonym feature on a field by adding a new property called **synonymMaps** in the field definition.</span></span> <span data-ttu-id="ff47a-119">Mindig egy teljes-dokumentum művelet, ami azt jelenti, hogy Ön nem létrehozása, frissítése vagy törlése a szinonima térkép részei Növekményesen létrehozása, frissítése és törlése szinonimát térképre.</span><span class="sxs-lookup"><span data-stu-id="ff47a-119">Creating, updating, and deleting a synonym map is always a whole-document operation, meaning that you cannot create, update or delete parts of the synonym map incrementally.</span></span> <span data-ttu-id="ff47a-120">Egy Újrabetöltés még egy-egy bejegyzésnek frissítése szükséges.</span><span class="sxs-lookup"><span data-stu-id="ff47a-120">Updating even a single entry requires a reload.</span></span>

<span data-ttu-id="ff47a-121">Szinonimák beépítése a keresőalkalmazás két lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="ff47a-121">Incorporating synonyms into your search application is a two-step process:</span></span>

1.  <span data-ttu-id="ff47a-122">Vegyen fel egy szinonima társítást az alábbi API-k segítségével a keresési szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="ff47a-122">Add a synonym map to your search service through the APIs below.</span></span>  

2.  <span data-ttu-id="ff47a-123">Az index definícióját a szinonima térkép használandó kereshető mezők konfigurálására.</span><span class="sxs-lookup"><span data-stu-id="ff47a-123">Configure a searchable field to use the synonym map in the index definition.</span></span>

### <a name="synonymmaps-resource-apis"></a><span data-ttu-id="ff47a-124">SynonymMaps erőforrás API-k</span><span class="sxs-lookup"><span data-stu-id="ff47a-124">SynonymMaps Resource APIs</span></span>

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a><span data-ttu-id="ff47a-125">Adja hozzá, vagy frissítse a szolgáltatás alatt szinonimát térkép, POST használatával, vagy HELYEZZE.</span><span class="sxs-lookup"><span data-stu-id="ff47a-125">Add or update a synonym map under your service, using POST or PUT.</span></span>

<span data-ttu-id="ff47a-126">Szinonimát maps feltöltötte-e a szolgáltatás POST vagy PUT keresztül.</span><span class="sxs-lookup"><span data-stu-id="ff47a-126">Synonym maps are uploaded to the service via POST or PUT.</span></span> <span data-ttu-id="ff47a-127">Minden egyes szabály az új sor karaktert ("\n") kell elválasztani.</span><span class="sxs-lookup"><span data-stu-id="ff47a-127">Each rule must be delimited by the new line character ('\n').</span></span> <span data-ttu-id="ff47a-128">Legfeljebb 5000 szabálynál szinonimát térkép szabad szolgáltatásban és más termékváltozatok 10 000 szabályokat adhat meg.</span><span class="sxs-lookup"><span data-stu-id="ff47a-128">You can define up to 5,000 rules per synonym map in a free service and 10,000 rules in all other SKUs.</span></span> <span data-ttu-id="ff47a-129">Minden egyes szabály legfeljebb 20 bővítések rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="ff47a-129">Each rule can have up to 20 expansions.</span></span>

<span data-ttu-id="ff47a-130">Ebben az előzetesben szinonimát maps az alábbiakban ismertetett Apache Solr formátumúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ff47a-130">In this preview, synonym maps must be in the Apache Solr format which is explained below.</span></span> <span data-ttu-id="ff47a-131">Ha egy meglévő szinonimát szótár más formátumú, és közvetlenül használja, tudassa velünk, a [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span><span class="sxs-lookup"><span data-stu-id="ff47a-131">If you have an existing synonym dictionary in a different format and want to use it directly, please let us know on [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span></span>

<span data-ttu-id="ff47a-132">Létrehozhat egy új szinonimát leképezés HTTP POST használatával az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="ff47a-132">You can create a new synonym map using HTTP POST, as in the following example:</span></span>

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

<span data-ttu-id="ff47a-133">Azt is megteheti PUT használja, és adja meg az URI a szinonima térkép nevét.</span><span class="sxs-lookup"><span data-stu-id="ff47a-133">Alternatively, you can use PUT and specify the synonym map name on the URI.</span></span> <span data-ttu-id="ff47a-134">Ha a szinonima leképezés nem létezik, a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="ff47a-134">If the synonym map does not exist, it will be created.</span></span>

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a><span data-ttu-id="ff47a-135">Apache Solr szinonimát formátumban</span><span class="sxs-lookup"><span data-stu-id="ff47a-135">Apache Solr synonym format</span></span>

<span data-ttu-id="ff47a-136">A Solr formátumhoz egyenértékű és explicit szinonimát leképezéseket.</span><span class="sxs-lookup"><span data-stu-id="ff47a-136">The Solr format supports equivalent and explicit synonym mappings.</span></span> <span data-ttu-id="ff47a-137">A nyílt forráskódú szinonimát szűrő megadását Apache Solr, a jelen dokumentumban ismertetett igazodik leképezési szabályokat: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter).</span><span class="sxs-lookup"><span data-stu-id="ff47a-137">Mapping rules adhere to the open source synonym filter specification of Apache Solr, described in this document: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter).</span></span> <span data-ttu-id="ff47a-138">Az alábbiakban az egyenértékű szinonimák minta szabályt.</span><span class="sxs-lookup"><span data-stu-id="ff47a-138">Below is a sample rule for equivalent synonyms.</span></span>
```
              USA, United States, United States of America
```

<span data-ttu-id="ff47a-139">Keresési lekérdezés a fenti a szabály az "USA" bővített "USA" vagy "Az Amerikai Egyesült Államok" vagy "Az Amerikai Egyesült Államok".</span><span class="sxs-lookup"><span data-stu-id="ff47a-139">With the rule above, a search query "USA" will expand to "USA" OR "United States" OR "United States of America".</span></span>

<span data-ttu-id="ff47a-140">Explicit leképezési nyíl megjelölt "= >".</span><span class="sxs-lookup"><span data-stu-id="ff47a-140">Explicit mapping is denoted by an arrow "=>".</span></span> <span data-ttu-id="ff47a-141">Megadása esetén a kifejezés, amely megfelel a bal oldali keresési lekérdezés sorozatát "= >" váltja fel a alternatívák jobb oldalán.</span><span class="sxs-lookup"><span data-stu-id="ff47a-141">When specified, a term sequence of a search query that matches the left hand side of "=>" will be replaced with the alternatives on the right hand side.</span></span> <span data-ttu-id="ff47a-142">Az alábbi szabály tekintve keresőkifejezések "Washington", "Wash."</span><span class="sxs-lookup"><span data-stu-id="ff47a-142">Given the rule below, search queries "Washington", "Wash."</span></span> <span data-ttu-id="ff47a-143">vagy a "WA" összes felülíródik a "WA".</span><span class="sxs-lookup"><span data-stu-id="ff47a-143">or "WA" will all be rewritten to "WA".</span></span> <span data-ttu-id="ff47a-144">Explicit leképezési csak a megadott irányban vonatkozik, és ebben az esetben nem írja a lekérdezés "Washington" a "WA".</span><span class="sxs-lookup"><span data-stu-id="ff47a-144">Explicit mapping only applies in the direction specified and does not rewrite the query "WA" to "Washington" in this case.</span></span>
```
              Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a><span data-ttu-id="ff47a-145">Lista szinonimát leképezi a szolgáltatás alatt.</span><span class="sxs-lookup"><span data-stu-id="ff47a-145">List synonym maps under your service.</span></span>

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a><span data-ttu-id="ff47a-146">A szolgáltatás alatt szinonimát térkép beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ff47a-146">Get a synonym map under your service.</span></span>

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a><span data-ttu-id="ff47a-147">A szolgáltatás alatt szinonimák térképre törlése.</span><span class="sxs-lookup"><span data-stu-id="ff47a-147">Delete a synonyms map under your service.</span></span>

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

### <a name="configure-a-searchable-field-to-use-the-synonym-map-in-the-index-definition"></a><span data-ttu-id="ff47a-148">Az index definícióját a szinonima térkép használandó kereshető mezők konfigurálására.</span><span class="sxs-lookup"><span data-stu-id="ff47a-148">Configure a searchable field to use the synonym map in the index definition.</span></span>

<span data-ttu-id="ff47a-149">Új mező tulajdonság **synonymMaps** segítségével adja meg a szinonima térképre használandó kereshető mező.</span><span class="sxs-lookup"><span data-stu-id="ff47a-149">A new field property **synonymMaps** can be used to specify a synonym map to use for a searchable field.</span></span> <span data-ttu-id="ff47a-150">Szinonimát maps szolgáltatás szintű erőforrás, és az index a szolgáltatás alatt futó minden mező szerint lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="ff47a-150">Synonym maps are service level resources and can be referenced by any field of an index under the service.</span></span>

    POST https://[servicename].search.windows.net/indexes?api-version=2016-09-01-Preview
    api-key: [admin key]

    {
       "name":"myindex",
       "fields":[
          {
             "name":"id",
             "type":"Edm.String",
             "key":true
          },
          {
             "name":"name",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"en.lucene",
             "synonymMaps":[
                "mysynonymmap"
             ]
          },
          {
             "name":"name_jp",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"ja.microsoft",
             "synonymMaps":[
                "japanesesynonymmap"
             ]
          }
       ]
    }

<span data-ttu-id="ff47a-151">**synonymMaps** adható meg "Edm.String" vagy "Collection(Edm.String)" típusú kereshető mezőt.</span><span class="sxs-lookup"><span data-stu-id="ff47a-151">**synonymMaps** can be specified for searchable fields of the type 'Edm.String' or 'Collection(Edm.String)'.</span></span>

> [!NOTE]
> <span data-ttu-id="ff47a-152">Ebben az előzetesben mezőben leképezése egy szinonima csak tartozhat.</span><span class="sxs-lookup"><span data-stu-id="ff47a-152">In this preview, you can only have one synonym map per field.</span></span> <span data-ttu-id="ff47a-153">Ha több szinonimát maps használni kívánt, tudassa velünk, a [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span><span class="sxs-lookup"><span data-stu-id="ff47a-153">If you want to use multiple synonym maps, please let us know on [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span></span>

## <a name="impact-of-synonyms-on-other-search-features"></a><span data-ttu-id="ff47a-154">Egyéb keresési funkciókat a szinonimák hatása</span><span class="sxs-lookup"><span data-stu-id="ff47a-154">Impact of synonyms on other search features</span></span>

<span data-ttu-id="ff47a-155">A szinonimák szolgáltatás felülírja az eredeti lekérdezés szinonimák a vagy operátorral.</span><span class="sxs-lookup"><span data-stu-id="ff47a-155">The synonyms feature rewrites the original query with synonyms with the OR operator.</span></span> <span data-ttu-id="ff47a-156">Emiatt találati kiemelése és profilok pontozási kezeli az eredeti és a szinonimák egyenértékűként.</span><span class="sxs-lookup"><span data-stu-id="ff47a-156">For this reason, hit highlighting and scoring profiles treat the original term and synonyms as equivalent.</span></span>

<span data-ttu-id="ff47a-157">Szinonimát szolgáltatás keresési lekérdezések, és nem vonatkozik a szűrők vagy értékkorlátozást.</span><span class="sxs-lookup"><span data-stu-id="ff47a-157">Synonym feature applies to search queries and does not apply to filters or facets.</span></span> <span data-ttu-id="ff47a-158">Ehhez hasonlóan javaslatok alapuló csak az eredeti kifejezés; találat szinonimát a válaszban nem jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="ff47a-158">Similarly, suggestions are based only on the original term; synonym matches do not appear in the response.</span></span>

<span data-ttu-id="ff47a-159">Szinonimát bővítések nem vonatkoznak a helyettesítő karakteres kifejezést; előtag, az intelligens egyeztetésű, és a regex feltételek nem bontja ki.</span><span class="sxs-lookup"><span data-stu-id="ff47a-159">Synonym expansions do not apply to wildcard search terms; prefix, fuzzy, and regex terms aren't expanded.</span></span>

## <a name="tips-for-building-a-synonym-map"></a><span data-ttu-id="ff47a-160">Tippek az szinonimát térképre felépítése</span><span class="sxs-lookup"><span data-stu-id="ff47a-160">Tips for building a synonym map</span></span>

- <span data-ttu-id="ff47a-161">Egy rövid, tetszetős szinonimát leképezés hatékonyabb, mint a lehetséges megfeleltetésekben teljesnek.</span><span class="sxs-lookup"><span data-stu-id="ff47a-161">A concise, well-designed synonym map is more efficient than an exhaustive list of possible matches.</span></span> <span data-ttu-id="ff47a-162">A túl nagy vagy összetett szótárak elemzése, és ezek befolyásolják a lekérdezés a lekérdezés sok szinonimák való tárolásához hosszabb ideig.</span><span class="sxs-lookup"><span data-stu-id="ff47a-162">Excessively large or complex dictionaries take longer to parse and affect the query latency if the query expands to many synonyms.</span></span> <span data-ttu-id="ff47a-163">Ahelyett, hogy a becslés, amellyel feltételek használhatók, a tényleges feltételeket keresztül kérhet egy [keresési forgalom elemző jelentés](search-traffic-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="ff47a-163">Rather than guess at which terms might be used, you can get the actual terms via a [search traffic analysis report](search-traffic-analytics.md).</span></span>

- <span data-ttu-id="ff47a-164">Egy előzetes és az érvényesítés megadásával, mert engedélyezése, majd használja ez a jelentés pontosan határozza meg, hogy mely feltételeit fog hasznot húzhatnak a szinonima egyezés, és folytassa használni, hogy a szinonima térképre van előállító jobb eredményének ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="ff47a-164">As both a preliminary and validation exercise, enable and then use this report to precisely determine which terms will benefit from a synonym match, and then continue to use it as validation that your synonym map is producing a better outcome.</span></span> <span data-ttu-id="ff47a-165">Az előre meghatározott jelentésben a csempék "leggyakoribb keresési lekérdezések" és "nulla-eredmény keresési lekérdezések" Erre azért van szükség a szükséges információkat.</span><span class="sxs-lookup"><span data-stu-id="ff47a-165">In the predefined report, the tiles "Most common search queries" and "Zero-result search queries" will give you the necessary information.</span></span>

- <span data-ttu-id="ff47a-166">Létrehozhat több szinonimát maps keresési alkalmazás (például úgy, hogy ha az alkalmazás támogatja a többnyelvű vásárlói bázisunk nyelv).</span><span class="sxs-lookup"><span data-stu-id="ff47a-166">You can create multiple synonym maps for your search application (for example, by language if your application supports a multi-lingual customer base).</span></span> <span data-ttu-id="ff47a-167">Jelenleg a mező csak egyikét használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="ff47a-167">Currently, a field can only use one of them.</span></span> <span data-ttu-id="ff47a-168">A mező synonymMaps tulajdonság bármikor frissítheti.</span><span class="sxs-lookup"><span data-stu-id="ff47a-168">You can update a field's synonymMaps property at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff47a-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ff47a-169">Next Steps</span></span>

- <span data-ttu-id="ff47a-170">Ha egy meglévő index (nem éles) fejlesztői környezetben, hogyan szinonimák hozzáadása módosítja keresési élményt biztosít, beleértve a profilok pontozási gyakorolt megtekintéséhez kattintson a kijelölés kis dictionary és javaslatok kísérletezhet.</span><span class="sxs-lookup"><span data-stu-id="ff47a-170">If you have an existing index in a development (non-production) environment, experiment with a small dictionary to see how the addition of synonyms changes the search experience, including impact on scoring profiles, hit highlighting, and suggestions.</span></span>

- <span data-ttu-id="ff47a-171">[Engedélyezze a keresési forgalom analytics](search-traffic-analytics.md) és az előre definiált Power BI-jelentés segítségével megtudhatja, mely kifejezések a legtöbb, és melyeket térjen vissza a dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="ff47a-171">[Enable search traffic analytics](search-traffic-analytics.md) and use the predefined Power BI report to learn which terms are used the most, and which ones return zero documents.</span></span> <span data-ttu-id="ff47a-172">A szótár kell lennie során az indexben dokumentumok rendszer lekérdezésekhez szinonimák felvenni volt képes ezek insights felvértezve, vizsgálja felül.</span><span class="sxs-lookup"><span data-stu-id="ff47a-172">Armed with these insights, revise the dictionary to include synonyms for unproductive queries that should be resolving to documents in your index.</span></span>
