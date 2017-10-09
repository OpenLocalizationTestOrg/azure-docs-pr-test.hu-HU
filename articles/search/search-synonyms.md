---
pageTitle: Synonyms in Azure Search (preview) | Microsoft Docs
description: "Hello szinonimák (előzetes verzió) szolgáltatás, hello Azure Search REST API felfedett előzetes dokumentációjában talál."
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
ms.openlocfilehash: 2695139d2b298fa2e7c1814715fdf96729f594ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="synonyms-in-azure-search-preview"></a><span data-ttu-id="3dccf-102">Szinonimák az Azure Search (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="3dccf-102">Synonyms in Azure Search (preview)</span></span>

<span data-ttu-id="3dccf-103">A keresőmotorok szinonimák egyenértékű feltételeket, amely implicit módon bontsa ki a lekérdezés hello hatókör nélkül adja meg a hello kifejezés tooactually rendelkező hello felhasználói társítani.</span><span class="sxs-lookup"><span data-stu-id="3dccf-103">Synonyms in search engines associate equivalent terms that implicitly expand hello scope of a query, without hello user having tooactually provide hello term.</span></span> <span data-ttu-id="3dccf-104">Például adott hello kifejezés "kutya" és "canine" és "ételadagot", "kutya", "canine" vagy "ételadagot" tartalmazó dokumentumokat szinonimát társítását hello hello-lekérdezés hatókörén belül is tartozik.</span><span class="sxs-lookup"><span data-stu-id="3dccf-104">For example, given hello term "dog" and synonym associations of "canine" and "puppy", any documents containing "dog", "canine" or "puppy" will fall within hello scope of hello query.</span></span>

<span data-ttu-id="3dccf-105">Az Azure Search szinonimát bővítése időben történik.</span><span class="sxs-lookup"><span data-stu-id="3dccf-105">In Azure Search, synonym expansion is done at query time.</span></span> <span data-ttu-id="3dccf-106">Szinonimát maps tooa szolgáltatás nem működőképes tooexisting műveletekkel adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="3dccf-106">You can add synonym maps tooa service with no disruption tooexisting operations.</span></span> <span data-ttu-id="3dccf-107">Hozzáadhat egy **synonymMaps** tulajdonság tooa mező definition toorebuild hello index nélkül.</span><span class="sxs-lookup"><span data-stu-id="3dccf-107">You can add a  **synonymMaps** property tooa field definition without having toorebuild hello index.</span></span> <span data-ttu-id="3dccf-108">További információkért lásd: [Index frissítése](https://docs.microsoft.com/rest/api/searchservice/update-index).</span><span class="sxs-lookup"><span data-stu-id="3dccf-108">For more information, see [Update Index](https://docs.microsoft.com/rest/api/searchservice/update-index).</span></span>

## <a name="feature-availability"></a><span data-ttu-id="3dccf-109">Szolgáltatások rendelkezésre állása</span><span class="sxs-lookup"><span data-stu-id="3dccf-109">Feature availability</span></span>

<span data-ttu-id="3dccf-110">a szolgáltatás jelenleg a hello szinonimák előzetes megtekintéséhez, és csak a támogatott hello legújabb előzetes api-verzió (api-version = 2016 09-01. dátumú előnézeti).</span><span class="sxs-lookup"><span data-stu-id="3dccf-110">hello synonyms feature is currently in preview and only supported in hello latest preview api-version (api-version=2016-09-01-Preview).</span></span> <span data-ttu-id="3dccf-111">Jelenleg nincs Azure Portal-támogatás.</span><span class="sxs-lookup"><span data-stu-id="3dccf-111">There is no Azure portal support at this time.</span></span> <span data-ttu-id="3dccf-112">Hello API-verzió van megadva hello kérés, mert már lehetséges toocombine általánosan elérhető (GA) és a hello API-k előzetes ugyanahhoz az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="3dccf-112">Because hello API version is specified on hello request, it's possible toocombine generally available (GA) and preview APIs in hello same app.</span></span> <span data-ttu-id="3dccf-113">Azonban előnézeti API-k nincsenek az SLA-t és a szolgáltatások módosíthatja, ezért nem javasoljuk azok az éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="3dccf-113">However, preview APIs are not under SLA and features may change, so we do not recommend using them in production applications.</span></span>

## <a name="how-toouse-synonyms-in-azure-search"></a><span data-ttu-id="3dccf-114">Az Azure-ban toouse szinonimák a keresés</span><span class="sxs-lookup"><span data-stu-id="3dccf-114">How toouse synonyms in Azure search</span></span>

<span data-ttu-id="3dccf-115">Az Azure Search szinonimát támogatási alapján határozza meg, és töltse fel a tooyour szolgáltatás szinonimát leképezések.</span><span class="sxs-lookup"><span data-stu-id="3dccf-115">In Azure Search, synonym support is based on synonym maps that you define and upload tooyour service.</span></span> <span data-ttu-id="3dccf-116">A maps képeznek egy független erőforrás (például az adatforrásokat, illetve indexek), és használhatja a keresési szolgáltatás index bármely kereshető mező.</span><span class="sxs-lookup"><span data-stu-id="3dccf-116">These maps constitute an independent resource (like indexes or data sources), and can be used by any searchable field in any index in your search service.</span></span>

<span data-ttu-id="3dccf-117">Szinonimát képezi le, és indexek egymástól függetlenül megmaradjanak.</span><span class="sxs-lookup"><span data-stu-id="3dccf-117">Synonym maps and indexes are maintained independently.</span></span> <span data-ttu-id="3dccf-118">Miután szinonimát térképre határozza meg, és töltse fel tooyour szolgáltatás, engedélyezheti nevű új tulajdonsággal hozzáadásával hello szinonimát szolgáltatás mező **synonymMaps** hello mező definícióban.</span><span class="sxs-lookup"><span data-stu-id="3dccf-118">Once you define a synonym map and upload it tooyour service, you can enable hello synonym feature on a field by adding a new property called **synonymMaps** in hello field definition.</span></span> <span data-ttu-id="3dccf-119">Létrehozása, frissítése és törlése egy szinonima térkép azonban mindig a teljes dokumentum művelet, ami azt jelenti, hogy nem hozható létre, frissítése vagy törlése Növekményesen hello szinonimát térkép részeit.</span><span class="sxs-lookup"><span data-stu-id="3dccf-119">Creating, updating, and deleting a synonym map is always a whole-document operation, meaning that you cannot create, update or delete parts of hello synonym map incrementally.</span></span> <span data-ttu-id="3dccf-120">Egy Újrabetöltés még egy-egy bejegyzésnek frissítése szükséges.</span><span class="sxs-lookup"><span data-stu-id="3dccf-120">Updating even a single entry requires a reload.</span></span>

<span data-ttu-id="3dccf-121">Szinonimák beépítése a keresőalkalmazás két lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="3dccf-121">Incorporating synonyms into your search application is a two-step process:</span></span>

1.  <span data-ttu-id="3dccf-122">Szinonimát térkép tooyour keresési szolgáltatás keresztül hello az alábbi API-k hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="3dccf-122">Add a synonym map tooyour search service through hello APIs below.</span></span>  

2.  <span data-ttu-id="3dccf-123">Adja meg a kereshető mező toouse hello szinonimát térkép hello index definícióját.</span><span class="sxs-lookup"><span data-stu-id="3dccf-123">Configure a searchable field toouse hello synonym map in hello index definition.</span></span>

### <a name="synonymmaps-resource-apis"></a><span data-ttu-id="3dccf-124">SynonymMaps erőforrás API-k</span><span class="sxs-lookup"><span data-stu-id="3dccf-124">SynonymMaps Resource APIs</span></span>

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a><span data-ttu-id="3dccf-125">Adja hozzá, vagy frissítse a szolgáltatás alatt szinonimát térkép, POST használatával, vagy HELYEZZE.</span><span class="sxs-lookup"><span data-stu-id="3dccf-125">Add or update a synonym map under your service, using POST or PUT.</span></span>

<span data-ttu-id="3dccf-126">Szinonimát maps feltöltése toohello szolgáltatás POST vagy PUT keresztül.</span><span class="sxs-lookup"><span data-stu-id="3dccf-126">Synonym maps are uploaded toohello service via POST or PUT.</span></span> <span data-ttu-id="3dccf-127">Minden egyes szabály hello új sor karaktert ("\n") kell elválasztani.</span><span class="sxs-lookup"><span data-stu-id="3dccf-127">Each rule must be delimited by hello new line character ('\n').</span></span> <span data-ttu-id="3dccf-128">Too5, egy ingyenes szolgáltatás szinonimát térképre 000 szabálynál és más termékváltozatok 10 000 szabályokat adhat meg.</span><span class="sxs-lookup"><span data-stu-id="3dccf-128">You can define up too5,000 rules per synonym map in a free service and 10,000 rules in all other SKUs.</span></span> <span data-ttu-id="3dccf-129">Minden egyes szabály legfeljebb too20 bővítések tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="3dccf-129">Each rule can have up too20 expansions.</span></span>

<span data-ttu-id="3dccf-130">Ebben az előzetesben szinonimát leképezéseket formátumúnak kell lennie hello Apache Solr alábbiakban ismertetett.</span><span class="sxs-lookup"><span data-stu-id="3dccf-130">In this preview, synonym maps must be in hello Apache Solr format which is explained below.</span></span> <span data-ttu-id="3dccf-131">Ha egy meglévő szinonimát szótár más formátumú, és szeretné toouse azt közvetlenül, tudassa velünk, a [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span><span class="sxs-lookup"><span data-stu-id="3dccf-131">If you have an existing synonym dictionary in a different format and want toouse it directly, please let us know on [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span></span>

<span data-ttu-id="3dccf-132">Létrehozhat új szinonimát leképezés HTTP POST, mint például a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="3dccf-132">You can create a new synonym map using HTTP POST, as in hello following example:</span></span>

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

<span data-ttu-id="3dccf-133">Azt is megteheti használja a PUT és hello szinonimát leképezésnév meg hello URI.</span><span class="sxs-lookup"><span data-stu-id="3dccf-133">Alternatively, you can use PUT and specify hello synonym map name on hello URI.</span></span> <span data-ttu-id="3dccf-134">Ha hello szinonimát leképezés nem létezik, a rendszer létrehozza.</span><span class="sxs-lookup"><span data-stu-id="3dccf-134">If hello synonym map does not exist, it will be created.</span></span>

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a><span data-ttu-id="3dccf-135">Apache Solr szinonimát formátumban</span><span class="sxs-lookup"><span data-stu-id="3dccf-135">Apache Solr synonym format</span></span>

<span data-ttu-id="3dccf-136">hello Solr formátumhoz egyenértékű és explicit szinonimát leképezéseket.</span><span class="sxs-lookup"><span data-stu-id="3dccf-136">hello Solr format supports equivalent and explicit synonym mappings.</span></span> <span data-ttu-id="3dccf-137">Leképezési szabályokat követi toohello nyílt forráskódú szinonimát szűrő megadását Apache Solr, a jelen dokumentumban ismertetett: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter).</span><span class="sxs-lookup"><span data-stu-id="3dccf-137">Mapping rules adhere toohello open source synonym filter specification of Apache Solr, described in this document: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter).</span></span> <span data-ttu-id="3dccf-138">Az alábbiakban az egyenértékű szinonimák minta szabályt.</span><span class="sxs-lookup"><span data-stu-id="3dccf-138">Below is a sample rule for equivalent synonyms.</span></span>
```
              USA, United States, United States of America
```

<span data-ttu-id="3dccf-139">A fenti hello szabálynak, a keresési lekérdezés "USA" túl bont ki "USA" vagy "Az Amerikai Egyesült Államok" vagy "Az Amerikai Egyesült Államok".</span><span class="sxs-lookup"><span data-stu-id="3dccf-139">With hello rule above, a search query "USA" will expand too"USA" OR "United States" OR "United States of America".</span></span>

<span data-ttu-id="3dccf-140">Explicit leképezési nyíl megjelölt "= >".</span><span class="sxs-lookup"><span data-stu-id="3dccf-140">Explicit mapping is denoted by an arrow "=>".</span></span> <span data-ttu-id="3dccf-141">Megadása esetén a kifejezés, amely megfelel a hello bal oldalán keresési lekérdezés sorozatát "= >" hello alternatívák hello jobb oldalán a helyébe.</span><span class="sxs-lookup"><span data-stu-id="3dccf-141">When specified, a term sequence of a search query that matches hello left hand side of "=>" will be replaced with hello alternatives on hello right hand side.</span></span> <span data-ttu-id="3dccf-142">Az alábbi hello szabály megadott, keresőkifejezések "Washington", "Wash."</span><span class="sxs-lookup"><span data-stu-id="3dccf-142">Given hello rule below, search queries "Washington", "Wash."</span></span> <span data-ttu-id="3dccf-143">vagy a "WA" összes felülíródik túl "WA".</span><span class="sxs-lookup"><span data-stu-id="3dccf-143">or "WA" will all be rewritten too"WA".</span></span> <span data-ttu-id="3dccf-144">Explicit leképezése csak megadott hello irányban vonatkozik, és nem újraírási hello lekérdezés "WA" túl ebben az esetben a "Washington".</span><span class="sxs-lookup"><span data-stu-id="3dccf-144">Explicit mapping only applies in hello direction specified and does not rewrite hello query "WA" too"Washington" in this case.</span></span>
```
              Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a><span data-ttu-id="3dccf-145">Lista szinonimát leképezi a szolgáltatás alatt.</span><span class="sxs-lookup"><span data-stu-id="3dccf-145">List synonym maps under your service.</span></span>

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a><span data-ttu-id="3dccf-146">A szolgáltatás alatt szinonimát térkép beolvasása.</span><span class="sxs-lookup"><span data-stu-id="3dccf-146">Get a synonym map under your service.</span></span>

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a><span data-ttu-id="3dccf-147">A szolgáltatás alatt szinonimák térképre törlése.</span><span class="sxs-lookup"><span data-stu-id="3dccf-147">Delete a synonyms map under your service.</span></span>

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

### <a name="configure-a-searchable-field-toouse-hello-synonym-map-in-hello-index-definition"></a><span data-ttu-id="3dccf-148">Adja meg a kereshető mező toouse hello szinonimát térkép hello index definícióját.</span><span class="sxs-lookup"><span data-stu-id="3dccf-148">Configure a searchable field toouse hello synonym map in hello index definition.</span></span>

<span data-ttu-id="3dccf-149">Új mező tulajdonság **synonymMaps** lehet használt toospecify egy szinonima térkép toouse kereshető mező.</span><span class="sxs-lookup"><span data-stu-id="3dccf-149">A new field property **synonymMaps** can be used toospecify a synonym map toouse for a searchable field.</span></span> <span data-ttu-id="3dccf-150">Szinonimát maps szolgáltatás szintű erőforrás, és az index hello szolgáltatásban minden mező szerint lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="3dccf-150">Synonym maps are service level resources and can be referenced by any field of an index under hello service.</span></span>

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

<span data-ttu-id="3dccf-151">**synonymMaps** kereshető mezők hello típusa "Edm.String" vagy "Collection(Edm.String)" adható meg.</span><span class="sxs-lookup"><span data-stu-id="3dccf-151">**synonymMaps** can be specified for searchable fields of hello type 'Edm.String' or 'Collection(Edm.String)'.</span></span>

> [!NOTE]
> <span data-ttu-id="3dccf-152">Ebben az előzetesben mezőben leképezése egy szinonima csak tartozhat.</span><span class="sxs-lookup"><span data-stu-id="3dccf-152">In this preview, you can only have one synonym map per field.</span></span> <span data-ttu-id="3dccf-153">Ha szeretné toouse több szinonimát maps, tudassa velünk, a [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span><span class="sxs-lookup"><span data-stu-id="3dccf-153">If you want toouse multiple synonym maps, please let us know on [UserVoice](https://feedback.azure.com/forums/263029-azure-search).</span></span>

## <a name="impact-of-synonyms-on-other-search-features"></a><span data-ttu-id="3dccf-154">Egyéb keresési funkciókat a szinonimák hatása</span><span class="sxs-lookup"><span data-stu-id="3dccf-154">Impact of synonyms on other search features</span></span>

<span data-ttu-id="3dccf-155">hello szinonimák szolgáltatás újraírja hello eredeti lekérdezés szinonimák a hello vagy operátor.</span><span class="sxs-lookup"><span data-stu-id="3dccf-155">hello synonyms feature rewrites hello original query with synonyms with hello OR operator.</span></span> <span data-ttu-id="3dccf-156">Emiatt találati kiemelése és profilok pontozási kezeli a hello eredeti kifejezés és szinonimák egyenértékűként.</span><span class="sxs-lookup"><span data-stu-id="3dccf-156">For this reason, hit highlighting and scoring profiles treat hello original term and synonyms as equivalent.</span></span>

<span data-ttu-id="3dccf-157">Szinonimát szolgáltatás toosearch lekérdezések vonatkozik, és nem vonatkozik a toofilters értékkorlátozást.</span><span class="sxs-lookup"><span data-stu-id="3dccf-157">Synonym feature applies toosearch queries and does not apply toofilters or facets.</span></span> <span data-ttu-id="3dccf-158">Ehhez hasonlóan javaslatok csak alapulnak hello eredeti kifejezés; találat szinonimát hello válaszban nem jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="3dccf-158">Similarly, suggestions are based only on hello original term; synonym matches do not appear in hello response.</span></span>

<span data-ttu-id="3dccf-159">Szinonimát bővítések csak akkor érvényesíthetők toowildcard keresőkifejezéseket; előtag, az intelligens egyeztetésű, és a regex feltételek nem bontja ki.</span><span class="sxs-lookup"><span data-stu-id="3dccf-159">Synonym expansions do not apply toowildcard search terms; prefix, fuzzy, and regex terms aren't expanded.</span></span>

## <a name="tips-for-building-a-synonym-map"></a><span data-ttu-id="3dccf-160">Tippek az szinonimát térképre felépítése</span><span class="sxs-lookup"><span data-stu-id="3dccf-160">Tips for building a synonym map</span></span>

- <span data-ttu-id="3dccf-161">Egy rövid, tetszetős szinonimát leképezés hatékonyabb, mint a lehetséges megfeleltetésekben teljesnek.</span><span class="sxs-lookup"><span data-stu-id="3dccf-161">A concise, well-designed synonym map is more efficient than an exhaustive list of possible matches.</span></span> <span data-ttu-id="3dccf-162">Túl nagy vagy összetett szótárak tooparse hosszabb ideig eltarthat, és hatással hello lekérdezés-késleltetés, hello lekérdezés toomany szinonimák tárolásához.</span><span class="sxs-lookup"><span data-stu-id="3dccf-162">Excessively large or complex dictionaries take longer tooparse and affect hello query latency if hello query expands toomany synonyms.</span></span> <span data-ttu-id="3dccf-163">Ahelyett, hogy a becslés, amellyel feltételek használhatók, kaphat a tényleges feltételek hello keresztül egy [keresési forgalom elemző jelentés](search-traffic-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="3dccf-163">Rather than guess at which terms might be used, you can get hello actual terms via a [search traffic analysis report](search-traffic-analytics.md).</span></span>

- <span data-ttu-id="3dccf-164">Egy előzetes és az érvényesítés a gyakorlatban, engedélyezése, és majd használja, ez a jelentés tooprecisely határozza meg, mely feltételek fog hasznot húzhatnak a szinonima egyezés, és folytassa toouse azt, hogy a szinonima térképre van előállító jobb eredményének ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="3dccf-164">As both a preliminary and validation exercise, enable and then use this report tooprecisely determine which terms will benefit from a synonym match, and then continue toouse it as validation that your synonym map is producing a better outcome.</span></span> <span data-ttu-id="3dccf-165">Hello előre megadott jelentésben, hello tartalmazó csempék éppen "leggyakoribb keresési lekérdezések" és "nulla-eredmény keresési lekérdezések" ad hello szükséges információkat.</span><span class="sxs-lookup"><span data-stu-id="3dccf-165">In hello predefined report, hello tiles "Most common search queries" and "Zero-result search queries" will give you hello necessary information.</span></span>

- <span data-ttu-id="3dccf-166">Létrehozhat több szinonimát maps keresési alkalmazás (például úgy, hogy ha az alkalmazás támogatja a többnyelvű vásárlói bázisunk nyelv).</span><span class="sxs-lookup"><span data-stu-id="3dccf-166">You can create multiple synonym maps for your search application (for example, by language if your application supports a multi-lingual customer base).</span></span> <span data-ttu-id="3dccf-167">Jelenleg a mező csak egyikét használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="3dccf-167">Currently, a field can only use one of them.</span></span> <span data-ttu-id="3dccf-168">A mező synonymMaps tulajdonság bármikor frissítheti.</span><span class="sxs-lookup"><span data-stu-id="3dccf-168">You can update a field's synonymMaps property at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3dccf-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3dccf-169">Next Steps</span></span>

- <span data-ttu-id="3dccf-170">Ha egy meglévő index (nem éles) fejlesztői környezetben, kísérletezhet egy kis szótár toosee hogyan szinonimák hello hozzáadása változik hello keresési élményt biztosít, beleértve a profilok pontozási találati kiemelése és javaslatok hatással.</span><span class="sxs-lookup"><span data-stu-id="3dccf-170">If you have an existing index in a development (non-production) environment, experiment with a small dictionary toosee how hello addition of synonyms changes hello search experience, including impact on scoring profiles, hit highlighting, and suggestions.</span></span>

- <span data-ttu-id="3dccf-171">[Engedélyezze a keresési forgalom analytics](search-traffic-analytics.md) használata hello előre Power BI-jelentés mely kifejezések toolearn hello legtöbb, és mely azokat, visszatérési dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="3dccf-171">[Enable search traffic analytics](search-traffic-analytics.md) and use hello predefined Power BI report toolearn which terms are used hello most, and which ones return zero documents.</span></span> <span data-ttu-id="3dccf-172">Hello szótár tooinclude szinonimák rendszer lekérdezések, amelyek az indexben toodocuments feloldása kell volt képes ezek insights felvértezve, vizsgálja felül.</span><span class="sxs-lookup"><span data-stu-id="3dccf-172">Armed with these insights, revise hello dictionary tooinclude synonyms for unproductive queries that should be resolving toodocuments in your index.</span></span>
