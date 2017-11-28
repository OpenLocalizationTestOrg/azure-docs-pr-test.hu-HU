---
title: "Teljes szöveges keresés (Lucene) motor architektúra az Azure Search |} Microsoft Docs"
description: "Teljes szöveges keresés, mint a kapcsolódó Azure Search Lucene lekérdezés feldolgozása és dokumentum beolvasása fogalmakat ismerteti."
services: search
manager: jhubbard
author: yahnoosh
documentationcenter: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/06/2017
ms.author: jlembicz
ms.openlocfilehash: 9b7adf78271407963ed1d4b34a7760d707b5fc3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-full-text-search-works-in-azure-search"></a><span data-ttu-id="2677d-103">Hogyan teljes szöveges keresés az Azure Search működik</span><span class="sxs-lookup"><span data-stu-id="2677d-103">How full text search works in Azure Search</span></span>

<span data-ttu-id="2677d-104">Ez a cikk a fejlesztők számára, akik bemutatják, hogyan Lucene teljes szöveges keresés működik-e az Azure Search van.</span><span class="sxs-lookup"><span data-stu-id="2677d-104">This article is for developers who need a deeper understanding of how Lucene full text search works in Azure Search.</span></span> <span data-ttu-id="2677d-105">A szöveges lekérdezések Azure Search problémamentesen fog továbbítani a kívánt eredmény elérése érdekében a legtöbb esetben azonban úgy tűnik, "off" valamilyen módon eredményt időnként előfordulhat, hogy.</span><span class="sxs-lookup"><span data-stu-id="2677d-105">For text queries, Azure Search will seamlessly deliver expected results in most scenarios, but occasionally you might get a result that seems "off" somehow.</span></span> <span data-ttu-id="2677d-106">Ezekben a helyzetekben, hogy a háttérben Lucene lekérdezés-végrehajtás négy szakaszaiban (elemzési, lexikális elemzés lekérdezéséhez dokumentálja a megfelelő pontozási) segítenek azonosítani azokat a lekérdezés-paraméterek vagy az index konfigurációja, hogy a rendszer a kívánt eredmény adott módosításait.</span><span class="sxs-lookup"><span data-stu-id="2677d-106">In these situations, having a background in the four stages of Lucene query execution (query parsing, lexical analysis, document matching, scoring) can help you identify specific changes to query parameters or index configuration that will deliver the desired outcome.</span></span> 

> [!Note] 
> <span data-ttu-id="2677d-107">Az Azure Search Lucene használ a teljes szöveges keresés, de Lucene integrációs nem teljes.</span><span class="sxs-lookup"><span data-stu-id="2677d-107">Azure Search uses Lucene for full text search, but Lucene integration is not exhaustive.</span></span> <span data-ttu-id="2677d-108">A Microsoft szelektív módon teszi közzé, és ahhoz, hogy az Azure Search fontos forgatókönyvek Lucene bővíthetők.</span><span class="sxs-lookup"><span data-stu-id="2677d-108">We selectively expose and extend Lucene functionality to enable the scenarios important to Azure Search.</span></span> 

## <a name="architecture-overview-and-diagram"></a><span data-ttu-id="2677d-109">Architektúra áttekintése és diagramja</span><span class="sxs-lookup"><span data-stu-id="2677d-109">Architecture overview and diagram</span></span>

<span data-ttu-id="2677d-110">Teljes szöveges keresési lekérdezés feldolgozása kezdődik-e a lekérdezés szövegének keresőkifejezéseket kibontásához elemzése.</span><span class="sxs-lookup"><span data-stu-id="2677d-110">Processing a full text search query starts with parsing the query text to extract search terms.</span></span> <span data-ttu-id="2677d-111">A keresőmotor index használja a megfelelő adatokkal dokumentumok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2677d-111">The search engine uses an index to retrieve documents with matching terms.</span></span> <span data-ttu-id="2677d-112">Egyéni lekérdezési kifejezések néha lebontva, és az új űrlapok szélesebb körű nettó konvertálni keresztül mi tekinthető, amely lehetséges elkészített.</span><span class="sxs-lookup"><span data-stu-id="2677d-112">Individual query terms are sometimes broken down and reconstituted into new forms to cast a broader net over what could be considered as a potential match.</span></span> <span data-ttu-id="2677d-113">Egy eredménykészlet majd minden egyes megfelelő dokumentum rendelt relevanciájának pontszámot szerint van rendezve.</span><span class="sxs-lookup"><span data-stu-id="2677d-113">A result set is then sorted by a relevance score assigned to each individual matching document.</span></span> <span data-ttu-id="2677d-114">Azok a rangsorolt lista tetején a rendszer visszairányítja a hívó alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2677d-114">Those at the top of the ranked list are returned to the calling application.</span></span>

<span data-ttu-id="2677d-115">Lekérdezés-végrehajtás rendelkezik állapítani, négy fázisból áll:</span><span class="sxs-lookup"><span data-stu-id="2677d-115">Restated, query execution has four stages:</span></span> 

1. <span data-ttu-id="2677d-116">A lekérdezés elemzése</span><span class="sxs-lookup"><span data-stu-id="2677d-116">Query parsing</span></span> 
2. <span data-ttu-id="2677d-117">Lexikális elemzés</span><span class="sxs-lookup"><span data-stu-id="2677d-117">Lexical analysis</span></span> 
3. <span data-ttu-id="2677d-118">A dokumentum beolvasása</span><span class="sxs-lookup"><span data-stu-id="2677d-118">Document retrieval</span></span> 
4. <span data-ttu-id="2677d-119">Pontozó</span><span class="sxs-lookup"><span data-stu-id="2677d-119">Scoring</span></span> 

<span data-ttu-id="2677d-120">Az alábbi ábra szemlélteti a search kérelmek feldolgozásához használt összetevőket.</span><span class="sxs-lookup"><span data-stu-id="2677d-120">The diagram below illustrates the components used to process a search request.</span></span> 

 ![Az Azure Search Lucene lekérdezés-architektúra ábrája][1]


| <span data-ttu-id="2677d-122">A legfontosabb összetevők</span><span class="sxs-lookup"><span data-stu-id="2677d-122">Key components</span></span> | <span data-ttu-id="2677d-123">Funkcionális leírása</span><span class="sxs-lookup"><span data-stu-id="2677d-123">Functional description</span></span> | 
|----------------|------------------------|
|<span data-ttu-id="2677d-124">**Lekérdezés elemzők**</span><span class="sxs-lookup"><span data-stu-id="2677d-124">**Query parsers**</span></span> | <span data-ttu-id="2677d-125">A lekérdezési operátorok lekérdezési kifejezések külön, és a keresőmotor küldendő lekérdezési struktúra (a lekérdezés-fa) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2677d-125">Separate query terms from query operators and create the query structure (a query tree) to be sent to the search engine.</span></span> |
|<span data-ttu-id="2677d-126">**Lekérdezések**</span><span class="sxs-lookup"><span data-stu-id="2677d-126">**Analyzers**</span></span> | <span data-ttu-id="2677d-127">Lexikális elemzést lekérdezés igényei szerint.</span><span class="sxs-lookup"><span data-stu-id="2677d-127">Perform lexical analysis on query terms.</span></span> <span data-ttu-id="2677d-128">Ez a folyamat magába foglaló átalakítása, eltávolítása vagy lekérdezési kifejezések bővíteni.</span><span class="sxs-lookup"><span data-stu-id="2677d-128">This process can involve transforming, removing, or expanding of query terms.</span></span> |
|<span data-ttu-id="2677d-129">**Index**</span><span class="sxs-lookup"><span data-stu-id="2677d-129">**Index**</span></span> | <span data-ttu-id="2677d-130">Egy hatékony tárolására és kinyert kereshető feltételek szervezésére szolgáló adatstruktúra indexelt dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="2677d-130">An efficient data structure used to store and organize searchable terms extracted from indexed documents.</span></span> |
|<span data-ttu-id="2677d-131">**Keresőmotor**</span><span class="sxs-lookup"><span data-stu-id="2677d-131">**Search engine**</span></span> | <span data-ttu-id="2677d-132">Olvassa be, és a fordított index tartalma alapján egyező dokumentumok pontszámaihoz.</span><span class="sxs-lookup"><span data-stu-id="2677d-132">Retrieves and scores matching documents based on the contents of the inverted index.</span></span> |

## <a name="anatomy-of-a-search-request"></a><span data-ttu-id="2677d-133">Keresési kérelem szerkezete</span><span class="sxs-lookup"><span data-stu-id="2677d-133">Anatomy of a search request</span></span>

<span data-ttu-id="2677d-134">A keresési kérelme, mert egy teljes megadását mi vissza kell adni egy eredményhalmazban szerepel.</span><span class="sxs-lookup"><span data-stu-id="2677d-134">A search request is a complete specification of what should be returned in a result set.</span></span> <span data-ttu-id="2677d-135">A legegyszerűbb esetben bármilyen feltételt nem tartalmazó egy üres lekérdezést esetében.</span><span class="sxs-lookup"><span data-stu-id="2677d-135">In simplest form, it is an empty query with no criteria of any kind.</span></span> <span data-ttu-id="2677d-136">A modell példa paramétereket tartalmaz, több lekérdezés feltételeket, lehet, hogy bizonyos mezők, valószínűleg egy kifejezést, és a rendezés szabályok hatókörébe.</span><span class="sxs-lookup"><span data-stu-id="2677d-136">A more realistic example includes parameters, several query terms, perhaps scoped to certain fields, with possibly a filter expression and ordering rules.</span></span>  

<span data-ttu-id="2677d-137">A következő példa az Azure Search használatával el tudja küldeni keresési kérelem a [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span><span class="sxs-lookup"><span data-stu-id="2677d-137">The following example is a search request you might send to Azure Search using the [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span></span>  

~~~~
POST /indexes/hotels/docs/search?api-version=2016-09-01 
{  
    "search": "Spacious, air-condition* +\"Ocean view\"",  
    "searchFields": "description, title",  
    "searchMode": "any",
    "filter": "price ge 60 and price lt 300",  
    "orderby": "geo.distance(location, geography'POINT(-159.476235 22.227659)')", 
    "queryType": "full" 
 } 
~~~~

<span data-ttu-id="2677d-138">A kérelem a keresőmotor a következőket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="2677d-138">For this request, the search engine does the following:</span></span>

1. <span data-ttu-id="2677d-139">Az ár esetén legalább $60 és kisebb, mint 300 $ dokumentumok szűrők.</span><span class="sxs-lookup"><span data-stu-id="2677d-139">Filters out documents where the price is at least $60 and less than $300.</span></span>
2. <span data-ttu-id="2677d-140">A lekérdezés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="2677d-140">Executes the query.</span></span> <span data-ttu-id="2677d-141">Ebben a példában a keresési lekérdezés áll kifejezések és a feltételek: `"Spacious, air-condition* +\"Ocean view\""` (felhasználók általában nem adhatja meg absztrakt azonban, beleértve a következő példában lehetővé teszi azt ismertetik, hogyan elemzőkkel kezelnie).</span><span class="sxs-lookup"><span data-stu-id="2677d-141">In this example, the search query consists of phrases and terms: `"Spacious, air-condition* +\"Ocean view\""` (users typically don't enter punctuation, but including it in the example allows us to explain how analyzers handle it).</span></span> <span data-ttu-id="2677d-142">Ehhez a lekérdezéshez a keresőmotor megvizsgálja a leírást, és címmezők megadott `searchFields` "Óceáni nézet", tartalmazó dokumentumok és továbbá "ahhoz," kifejezést vagy a feltételeket, amelyek a előtaggal kezdődik "air-condition".</span><span class="sxs-lookup"><span data-stu-id="2677d-142">For this query, the search engine scans the description and title fields specified in `searchFields` for documents that contain "Ocean view", and additionally on the term "spacious", or on terms that start with the prefix "air-condition".</span></span> <span data-ttu-id="2677d-143">A `searchMode` paraméterrel felel meg a kifejezés (alapértelmezett), vagy azokat, olyan esetekben, ahol a kifejezés nincs explicit módon szükséges (`+`).</span><span class="sxs-lookup"><span data-stu-id="2677d-143">The `searchMode` parameter is used to match on any term (default) or all of them, for cases where a term is not explicitly required (`+`).</span></span>
3. <span data-ttu-id="2677d-144">Rendelések a létrejövő által egy adott földrajzi hely közelében a szállodák készletét, és ezután visszaérkezik a hívó alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2677d-144">Orders the resulting set of hotels by proximity to a given geography location, and then returned to the calling application.</span></span> 

<span data-ttu-id="2677d-145">Ez a cikk a legtöbb tárgya feldolgozását a *keresési lekérdezés*: `"Spacious, air-condition* +\"Ocean view\""`.</span><span class="sxs-lookup"><span data-stu-id="2677d-145">The majority of this article is about processing of the *search query*: `"Spacious, air-condition* +\"Ocean view\""`.</span></span> <span data-ttu-id="2677d-146">Szűrési és rendezési kívül esnek a hatókörön.</span><span class="sxs-lookup"><span data-stu-id="2677d-146">Filtering and ordering are out of scope.</span></span> <span data-ttu-id="2677d-147">További információkért lásd: a [keresési API-referenciadokumentáció](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span><span class="sxs-lookup"><span data-stu-id="2677d-147">For more information, see the [Search API reference documentation](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span></span>

<a name="stage1"></a>
## <a name="stage-1-query-parsing"></a><span data-ttu-id="2677d-148">1. fázis: Lekérdezés elemzése</span><span class="sxs-lookup"><span data-stu-id="2677d-148">Stage 1: Query parsing</span></span> 

<span data-ttu-id="2677d-149">Amint azt a lekérdezési karakterláncban a kérelem első sorának:</span><span class="sxs-lookup"><span data-stu-id="2677d-149">As noted, the query string is the first line of the request:</span></span> 

~~~~
 "search": "Spacious, air-condition* +\"Ocean view\"", 
~~~~

<span data-ttu-id="2677d-150">A lekérdezéselemzőben elválasztja az operátorok (például `*` és `+` a példában) keresési feltételeket, és a keresési lekérdezést deconstructs *segédlekérdezések* támogatott típusú:</span><span class="sxs-lookup"><span data-stu-id="2677d-150">The query parser separates operators (such as `*` and `+` in the example) from search terms, and deconstructs the search query into *subqueries* of a supported type:</span></span> 

+ <span data-ttu-id="2677d-151">*kifejezés lekérdezés* önálló feltételek (például ahhoz)</span><span class="sxs-lookup"><span data-stu-id="2677d-151">*term query* for standalone terms (like spacious)</span></span>
+ <span data-ttu-id="2677d-152">*kifejezés lekérdezés* idézőjelek közé zárt feltételek (például óceáni megtekintése)</span><span class="sxs-lookup"><span data-stu-id="2677d-152">*phrase query* for quoted terms (like ocean view)</span></span>
+ <span data-ttu-id="2677d-153">*előtag-lekérdezés* követ egy előtag operátor feltételek `*` (például air-condition)</span><span class="sxs-lookup"><span data-stu-id="2677d-153">*prefix query* for terms followed by a prefix operator `*` (like air-condition)</span></span>

<span data-ttu-id="2677d-154">A támogatott lekérdezéstípusok teljes listáját lásd: [Lucene lekérdezés sytnax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)</span><span class="sxs-lookup"><span data-stu-id="2677d-154">For a full list of supported query types see [Lucene query sytnax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)</span></span>

<span data-ttu-id="2677d-155">Operátor segédlekérdezésben társított határozza meg, hogy a lekérdezés "" vagy "kell lennie" ahhoz, hogy a dokumentum akkor, ha egyezés teljesül.</span><span class="sxs-lookup"><span data-stu-id="2677d-155">Operators associated with a subquery determine whether the query "must be" or "should be" satisfied in order for a document to be considered a match.</span></span> <span data-ttu-id="2677d-156">Például `+"Ocean view"` "kell" van miatt a `+` operátor.</span><span class="sxs-lookup"><span data-stu-id="2677d-156">For example, `+"Ocean view"` is "must" due to the `+` operator.</span></span> 

<span data-ttu-id="2677d-157">Lekérdezéselemzőben átszervezése a segédlekérdezések egy *lekérdezés fa* (egyik belső struktúra, a lekérdezés képviselő) továbbítja a keresőmotort.</span><span class="sxs-lookup"><span data-stu-id="2677d-157">The query parser restructures the subqueries into a *query tree* (an internal structure representing the query) it passes on to the search engine.</span></span> <span data-ttu-id="2677d-158">A lekérdezés elemzése első szakaszában a lekérdezés fa néz ki.</span><span class="sxs-lookup"><span data-stu-id="2677d-158">In the first stage of query parsing, the query tree looks like this.</span></span>  

 ![Logikai érték searchmode minden lekérdezése][2]

### <a name="supported-parsers-simple-and-full-lucene"></a><span data-ttu-id="2677d-160">Támogatott elemzők: egyszerű és a teljes Lucene</span><span class="sxs-lookup"><span data-stu-id="2677d-160">Supported parsers: Simple and Full Lucene</span></span> 

 <span data-ttu-id="2677d-161">Az Azure Search mutatja meg két különböző lekérdezési nyelv `simple` (alapértelmezett) és `full`.</span><span class="sxs-lookup"><span data-stu-id="2677d-161">Azure Search exposes two different query languages, `simple` (default) and `full`.</span></span> <span data-ttu-id="2677d-162">Úgy, hogy a `queryType` paraméter, a search kérelemmel, közli a lekérdezéselemzőben mely lekérdezési nyelv úgy dönt, így az tudni fogja, a kezelők és értelmezése.</span><span class="sxs-lookup"><span data-stu-id="2677d-162">By setting the `queryType` parameter with your search request, you tell the query parser which query language you choose so that it knows how to interpret the operators and syntax.</span></span> <span data-ttu-id="2677d-163">A [egyszerű lekérdezési nyelv](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) intuitív és robusztus, akkor gyakran megfelelő szerint felhasználói adatbevitelt értelmezése-ügyféloldali feldolgozás nélkül van.</span><span class="sxs-lookup"><span data-stu-id="2677d-163">The [Simple query langauge](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) is intuitive and robust, often suitable to interpret user input as-is without client-side processing.</span></span> <span data-ttu-id="2677d-164">Támogatja a lekérdezési operátorok ismerős webes keresőmotorokból.</span><span class="sxs-lookup"><span data-stu-id="2677d-164">It supports query operators familiar from web search engines.</span></span> <span data-ttu-id="2677d-165">A [teljes Lucene lekérdezési nyelv](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), amely úgy, hogy elérhetővé `queryType=full`, az alapértelmezett egyszerű lekérdezési nyelv bővíti a további operátorok és a helyettesítő karakter, az intelligens egyeztetésű például lekérdezéstípusok, a reguláris kifejezéssel és a mező hatókörű lekérdezések támogatásának hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="2677d-165">The [Full Lucene query language](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), which you get by setting `queryType=full`, extends the default Simple query language by adding support for more operators and query types like wildcard, fuzzy, regex, and field-scoped queries.</span></span> <span data-ttu-id="2677d-166">Például egy egyszerű lekérdezés szintaxisát küldött reguláris kifejezés értelmezését egy lekérdezési karakterlánc és a kifejezés nem.</span><span class="sxs-lookup"><span data-stu-id="2677d-166">For example, a regular expression sent in Simple query syntax would be interpreted as a query string and not an expression.</span></span> <span data-ttu-id="2677d-167">A példa egy kérelem a cikkben a teljes Lucene lekérdezés nyelvét használja.</span><span class="sxs-lookup"><span data-stu-id="2677d-167">The example request in this article uses the Full Lucene query language.</span></span>

### <a name="impact-of-searchmode-on-the-parser"></a><span data-ttu-id="2677d-168">Az elemző a searchMode hatása</span><span class="sxs-lookup"><span data-stu-id="2677d-168">Impact of searchMode on the parser</span></span> 

<span data-ttu-id="2677d-169">Keresési kérelmet egy másik paraméter, amely befolyásolja a elemzése a `searchMode` paraméter.</span><span class="sxs-lookup"><span data-stu-id="2677d-169">Another search request parameter that affects parsing is the `searchMode` parameter.</span></span> <span data-ttu-id="2677d-170">Azt szabályozza, hogy az alapértelmezett operátor logikai lekérdezések: vagy az összes bármely (alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="2677d-170">It controls the default operator for Boolean queries: any (default) or all.</span></span>  

<span data-ttu-id="2677d-171">Ha `searchMode=any`, amely az alapértelmezett, a hely elválasztó között ahhoz, és a air-condition vagy (`||`), így a minta lekérdezésszöveg egyenértékű:</span><span class="sxs-lookup"><span data-stu-id="2677d-171">When `searchMode=any`, which is the default, the space delimiter between spacious and air-condition is OR (`||`), making the sample query text equivalent to:</span></span> 

~~~~
Spacious,||air-condition*+"Ocean view" 
~~~~

<span data-ttu-id="2677d-172">Explicit operátorok, például a `+` a `+"Ocean view"`, amelyek egyértelműen logikai lekérdezés kialakításában (kifejezés *kell* felel meg).</span><span class="sxs-lookup"><span data-stu-id="2677d-172">Explicit operators, such as `+` in `+"Ocean view"`, are unambiguous in boolean query construction (the term *must* match).</span></span> <span data-ttu-id="2677d-173">Kevésbé nyilvánvaló van a fennmaradó feltételek értelmezése: ahhoz, és air-condition.</span><span class="sxs-lookup"><span data-stu-id="2677d-173">Less obvious is how to interpret the remaining terms: spacious and air-condition.</span></span> <span data-ttu-id="2677d-174">Érdemes a keresőmotor találatokat óceáni nézeten *és* ahhoz, *és* air-condition?</span><span class="sxs-lookup"><span data-stu-id="2677d-174">Should the search engine find matches on ocean view *and* spacious *and* air-condition?</span></span> <span data-ttu-id="2677d-175">Vagy kell keresés óceáni nézet plus *egy* fennmaradó feltételét?</span><span class="sxs-lookup"><span data-stu-id="2677d-175">Or should it find ocean view plus *either one* of the remaining terms?</span></span> 

<span data-ttu-id="2677d-176">Alapértelmezés szerint (`searchMode=any`), a keresőmotor azt feltételezi, hogy a szélesebb körű értelmezése.</span><span class="sxs-lookup"><span data-stu-id="2677d-176">By default (`searchMode=any`), the search engine assumes the broader interpretation.</span></span> <span data-ttu-id="2677d-177">Vagy mező *kell* egyeztetni, amely tükrözi "vagy" szemantikáját.</span><span class="sxs-lookup"><span data-stu-id="2677d-177">Either field *should* be matched, reflecting "or" semantics.</span></span> <span data-ttu-id="2677d-178">A kezdeti lekérdezést fa korábban bemutatott, a két, "kell" műveletek, az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="2677d-178">The initial query tree illustrated previously, with the two "should" operations, shows the default.</span></span>  

<span data-ttu-id="2677d-179">Tegyük fel, hogy most hivatott `searchMode=all`.</span><span class="sxs-lookup"><span data-stu-id="2677d-179">Suppose that we now set `searchMode=all`.</span></span> <span data-ttu-id="2677d-180">Ebben az esetben a hely a "és" művelet kerül értelmezésre.</span><span class="sxs-lookup"><span data-stu-id="2677d-180">In this case, the space is interpreted as an "and" operation.</span></span> <span data-ttu-id="2677d-181">A fennmaradó feltételek mindegyikének is jelen kell lennie ahhoz, hogy egyezés a dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="2677d-181">Each of the remaining terms must both be present in the document to qualify as a match.</span></span> <span data-ttu-id="2677d-182">Az eredményül kapott mintalekérdezés értelmezését az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="2677d-182">The resulting sample query would be interpreted as follows:</span></span> 

~~~~
+Spacious,+air-condition*+"Ocean view"  
~~~~

<span data-ttu-id="2677d-183">Ehhez a lekérdezéshez módosított lekérdezés fát következőképpen nézne ki, ahol a megfelelő dokumentumokra az összes három segédlekérdezések metszetét:</span><span class="sxs-lookup"><span data-stu-id="2677d-183">A modified query tree for this query would be as follows, where a matching document is the intersection of all three subqueries:</span></span> 

 ![Az összes logikai lekérdezés searchmode][3]

> [!Note] 
> <span data-ttu-id="2677d-185">Kiválasztása `searchMode=any` keresztül `searchMode=all` legjobb döntést jutott reprezentatív lekérdezések futtatásával.</span><span class="sxs-lookup"><span data-stu-id="2677d-185">Choosing `searchMode=any` over `searchMode=all` is a decision best arrived at by running representative queries.</span></span> <span data-ttu-id="2677d-186">Felhasználók, akik valószínűleg tartalmaz az operátorok (Ha a Keresés a dokumentum tárolja közös) található eredmények intuitívabb Ha `searchMode=all` arról tájékoztatja a logikai lekérdezési szerkezeteket.</span><span class="sxs-lookup"><span data-stu-id="2677d-186">Users who are likely to include operators (common when searching document stores) might find results more intuitive if `searchMode=all` informs boolean query constructs.</span></span> <span data-ttu-id="2677d-187">További információk az közötti együttműködés `searchMode` és operátorok, lásd: [egyszerű lekérdezés szintaxisát](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).</span><span class="sxs-lookup"><span data-stu-id="2677d-187">For more about the interplay between `searchMode` and operators, see [Simple query syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).</span></span>

<a name="stage2"></a>
## <a name="stage-2-lexical-analysis"></a><span data-ttu-id="2677d-188">2. fázis: Lexikális elemzés</span><span class="sxs-lookup"><span data-stu-id="2677d-188">Stage 2: Lexical analysis</span></span> 

<span data-ttu-id="2677d-189">Lexikális elemzőkkel folyamat *lekérdezések távon* és *lekérdezések kifejezés* a lekérdezés fa felépítése után.</span><span class="sxs-lookup"><span data-stu-id="2677d-189">Lexical analyzers process *term queries* and *phrase queries* after the query tree is structured.</span></span> <span data-ttu-id="2677d-190">Egy elemző eszköz az az elemző által megadott szöveges adatokat fogad, dolgozza fel a szöveg, és majd küld vissza a tokenekre feltételeket a lekérdezés fa szóló.</span><span class="sxs-lookup"><span data-stu-id="2677d-190">An analyzer accepts the text inputs given to it by the parser, processes the text, and then sends back tokenized terms to be incorporated into the query tree.</span></span> 

<span data-ttu-id="2677d-191">Az általános űrlap lexikális elemzési *nyelvi elemzés* átalakítások lekérdezése alapján a szabályok egy adott nyelvre vonatkozó feltételeket, amelyek:</span><span class="sxs-lookup"><span data-stu-id="2677d-191">The most common form of lexical analysis is *linguistic analysis* which transforms query terms based on rules specific to a given language:</span></span> 

* <span data-ttu-id="2677d-192">A lekérdezés kifejezés csökkentése a legfelső szintű űrlap szó</span><span class="sxs-lookup"><span data-stu-id="2677d-192">Reducing a query term to the root form of a word</span></span> 
* <span data-ttu-id="2677d-193">Nélkülözhető szavak eltávolítása (szavak, például az "a" vagy "és" angol nyelven)</span><span class="sxs-lookup"><span data-stu-id="2677d-193">Removing non-essential words (stopwords, such as "the" or "and" in English)</span></span> 
* <span data-ttu-id="2677d-194">Egy összetett a word ossza összetevői</span><span class="sxs-lookup"><span data-stu-id="2677d-194">Breaking a composite word into component parts</span></span> 
* <span data-ttu-id="2677d-195">Alsó körül, egy nagybetű word</span><span class="sxs-lookup"><span data-stu-id="2677d-195">Lower casing an upper case word</span></span> 

<span data-ttu-id="2677d-196">Ezeket a műveleteket az egyes törlése a felhasználó által megadott szöveg bemenetet és a feltételeket az indexben tárolt közötti különbséget.</span><span class="sxs-lookup"><span data-stu-id="2677d-196">All of these operations tend to erase differences between the text input provided by the user and the terms stored in the index.</span></span> <span data-ttu-id="2677d-197">Az ilyen műveletek szöveg feldolgozási túlmutató, és magát a nyelv alapos ismerete szükséges.</span><span class="sxs-lookup"><span data-stu-id="2677d-197">Such operations go beyond text processing and require in-depth knowledge of the language itself.</span></span> <span data-ttu-id="2677d-198">Nyelvi tájékoztatási réteg hozzáadásához az Azure Search támogatja listája túl hosszú [nyelvi elemzőkkel](https://docs.microsoft.com/rest/api/searchservice/language-support) Lucene és a Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2677d-198">To add this layer of linguistic awareness, Azure Search supports a long list of [language analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support) from both Lucene and Microsoft.</span></span>

> [!Note]
> <span data-ttu-id="2677d-199">Elemzés követelmények között lehet minimális fejlesztett ki attól függően, a forgatókönyv számára.</span><span class="sxs-lookup"><span data-stu-id="2677d-199">Analysis requirements can range from minimal to elaborate depending on your scenario.</span></span> <span data-ttu-id="2677d-200">Az előre definiált elemzőkkel kiválasztásával közül, vagy hozzon létre egy saját lexikális elemzés összetettsége szabályozhatja [egyéni analyzer](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="2677d-200">You can control complexity of lexical analysis by the selecting one of the predefined analyzers or by creating your own [custom analyzer](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search).</span></span> <span data-ttu-id="2677d-201">Lekérdezések hatóköre kereshető mezőt, és egy mező definíciójának részeként vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="2677d-201">Analyzers are scoped to searchable fields and are specified as part of a field definition.</span></span> <span data-ttu-id="2677d-202">Ez lehetővé teszi, hogy eltérő lexikális elemzés, mező alapon.</span><span class="sxs-lookup"><span data-stu-id="2677d-202">This allows you to vary lexical analysis on a per-field basis.</span></span> <span data-ttu-id="2677d-203">Nincs megadva, a *szabványos* Lucene analyzer szolgál.</span><span class="sxs-lookup"><span data-stu-id="2677d-203">Unspecified, the *standard* Lucene analyzer is used.</span></span>

<span data-ttu-id="2677d-204">A példa kedvéért elemzés, mielőtt a kezdeti lekérdezést fa van kifejezés "Spacious," nagybetűs "S" és a lekérdezési kifejezésre (vesszővel nem tekinthető a lekérdezési nyelv operátor) részeként értelmezi lekérdezéselemzőben vesszővel válassza el.</span><span class="sxs-lookup"><span data-stu-id="2677d-204">In our example, prior to analysis, the initial query tree has the term "Spacious," with an uppercase "S" and a comma that the query parser interprets as a part of the query term (a comma is not considered a query language operator).</span></span>  

<span data-ttu-id="2677d-205">Az alapértelmezett analyzer dolgozza fel a kifejezés, emellett kisbetűs "óceáni nézet" és "ahhoz,", és a vessző karakter eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="2677d-205">When the default analyzer processes the term, it will lowercase "ocean view" and "spacious", and remove the comma character.</span></span> <span data-ttu-id="2677d-206">A módosított lekérdezés fa a következőképpen fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="2677d-206">The modified query tree will look as follows:</span></span> 

 ![Logikai lekérdezés elemzett adatokkal][4]

### <a name="testing-analyzer-behaviors"></a><span data-ttu-id="2677d-208">Tesztelési analyzer viselkedések</span><span class="sxs-lookup"><span data-stu-id="2677d-208">Testing analyzer behaviors</span></span> 

<span data-ttu-id="2677d-209">Egy elemző viselkedését használatával kell tesztelni a [elemzése API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer).</span><span class="sxs-lookup"><span data-stu-id="2677d-209">The behavior of an analyzer can be tested using the [Analyze API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer).</span></span> <span data-ttu-id="2677d-210">Adja meg a analyzer adott kifejezések létrehoz megjelenítéséhez elemezni kívánt szöveg.</span><span class="sxs-lookup"><span data-stu-id="2677d-210">Provide the text you want to analyze to see what terms given analyzer will generate.</span></span> <span data-ttu-id="2677d-211">Például hogy módját a szabványos analyzer szeretné feldolgozni a szöveges "air-condition", kiadhatja a következő kérelmet:</span><span class="sxs-lookup"><span data-stu-id="2677d-211">For example, to see how the standard analyzer would process the text "air-condition", you can issue the following request:</span></span>

~~~~
{ 
    "text": "air-condition",
    "analyzer": "standard"
}
~~~~

<span data-ttu-id="2677d-212">A szabványos analyzer bontja a következő két jogkivonatok ellátása megjegyzésekkel őket a tulajdonságai, például a kezdő és záró eltolások (találati konzolban használt), valamint pozíciójuk (használt kifejezés a megfelelő) a bemeneti szöveg:</span><span class="sxs-lookup"><span data-stu-id="2677d-212">The standard analyzer breaks the input text into the following two tokens, annotating them with attributes like start and end offsets (used for hit highlighting) as well as their position (used for phrase matching):</span></span>

~~~~
{  
  "tokens": [
    {
      "token": "air",
      "startOffset": 0,
      "endOffset": 3,
      "position": 0
    },
    {
      "token": "condition",
      "startOffset": 4,
      "endOffset": 13,
      "position": 1
    }
  ]
}
~~~~

### <a name="exceptions-to-lexical-analysis"></a><span data-ttu-id="2677d-213">Kivételeket lexikális elemzés</span><span class="sxs-lookup"><span data-stu-id="2677d-213">Exceptions to lexical analysis</span></span> 

<span data-ttu-id="2677d-214">Lexikális elemző csak a teljes feltételek – kifejezés lekérdezés vagy egy kifejezés lekérdezés igénylő lekérdezéstípusok vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="2677d-214">Lexical analysis applies only to query types that require complete terms – either a term query or a phrase query.</span></span> <span data-ttu-id="2677d-215">Lekérdezéstípusok – előtag lekérdezés, helyettesítő karaktereknek, regex lekérdezés – hiányos adatokkal, vagy egy intelligens lekérdezés nem vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="2677d-215">It doesn’t apply to query types with incomplete terms – prefix query, wildcard query, regex query – or to a fuzzy query.</span></span> <span data-ttu-id="2677d-216">Azok lekérdezése típusok, beleértve a kifejezés előtag lekérdezés *air-condition\**  a fenti példában kerülnek közvetlenül a lekérdezés fa, az elemzési fázis kihagyásával.</span><span class="sxs-lookup"><span data-stu-id="2677d-216">Those query types, including the prefix query with term *air-condition\** in our example, are added directly to the query tree, bypassing the analysis stage.</span></span> <span data-ttu-id="2677d-217">Az adott típusú lekérdezési kifejezések végre csak átalakítás lowercasing van.</span><span class="sxs-lookup"><span data-stu-id="2677d-217">The only transformation performed on query terms of those types is lowercasing.</span></span>

<a name="stage3"></a>
## <a name="stage-3-document-retrieval"></a><span data-ttu-id="2677d-218">3. fázis: A dokumentum beolvasása</span><span class="sxs-lookup"><span data-stu-id="2677d-218">Stage 3: Document retrieval</span></span> 

<span data-ttu-id="2677d-219">Az index feltételeit egyeztetésével dokumentumok keresése dokumentum beolvasása hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="2677d-219">Document retrieval refers to finding documents with matching terms in the index.</span></span> <span data-ttu-id="2677d-220">Ez a szakasz egy példán keresztül legjobb értendő.</span><span class="sxs-lookup"><span data-stu-id="2677d-220">This stage is understood best through an example.</span></span> <span data-ttu-id="2677d-221">Kezdjük a szállodák index a következő egyszerű séma rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="2677d-221">Let's start with a hotels index having the following simple schema:</span></span> 

~~~~
{   
    "name": "hotels",     
    "fields": [     
        { "name": "id", "type": "Edm.String", "key": true, "searchable": false },     
        { "name": "title", "type": "Edm.String", "searchable": true },     
        { "name": "description", "type": "Edm.String", "searchable": true }
    ] 
} 
~~~~

<span data-ttu-id="2677d-222">További azt feltételezik, hogy ezt az indexet tartalmaz a következő négy dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="2677d-222">Further assume that this index contains the following four documents:</span></span> 

~~~~
{ 
    "value": [
        {         
            "id": "1",         
            "title": "Hotel Atman",         
            "description": "Spacious rooms, ocean view, walking distance to the beach."   
        },       
        {         
            "id": "2",         
            "title": "Beach Resort",        
            "description": "Located on the north shore of the island of Kauaʻi. Ocean view."     
        },       
        {         
            "id": "3",         
            "title": "Playa Hotel",         
            "description": "Comfortable, air-conditioned rooms with ocean view."
        },       
        {         
            "id": "4",         
            "title": "Ocean Retreat",         
            "description": "Quiet and secluded"
        }    
    ]
}
~~~~

<span data-ttu-id="2677d-223">**Hogyan feltételek indexelt**</span><span class="sxs-lookup"><span data-stu-id="2677d-223">**How terms are indexed**</span></span>

<span data-ttu-id="2677d-224">Szeretné megtudni, lekérés, áttekinteni tudni, hogy néhány alapvető indexelése.</span><span class="sxs-lookup"><span data-stu-id="2677d-224">To understand retrieval, it helps to know a few basics about indexing.</span></span> <span data-ttu-id="2677d-225">Tárolási egységének értéke egy fordított index, egy az összes kereshető mezőt.</span><span class="sxs-lookup"><span data-stu-id="2677d-225">The unit of storage is an inverted index, one for each searchable field.</span></span> <span data-ttu-id="2677d-226">Fordított index belül az összes dokumentumot az összes kifejezést a rendezett listáját.</span><span class="sxs-lookup"><span data-stu-id="2677d-226">Within an inverted index is a sorted list of all terms from all documents.</span></span> <span data-ttu-id="2677d-227">Minden kifejezéshez van leképezve, amelyben előfordul, az alábbi példa szerint egyértelmű dokumentumok listáját.</span><span class="sxs-lookup"><span data-stu-id="2677d-227">Each term maps to the list of documents in which it occurs, as evident in the example below.</span></span>

<span data-ttu-id="2677d-228">A feltételek fordított index létrehozásához a keresőmotor lexikális elemzés a dokumentumok tartalmát, hasonló mi történik, a lekérdezés feldolgozása közben keresztüli hajt végre.</span><span class="sxs-lookup"><span data-stu-id="2677d-228">To produce the terms in an inverted index, the search engine performs lexical analysis over the content of documents, similar to what happens during query processing.</span></span> <span data-ttu-id="2677d-229">Szöveg bemenetek átadni egy analyzer alsó cased absztrakt, és így tovább, a analyzer konfigurációjától függően az újrafoglalások.</span><span class="sxs-lookup"><span data-stu-id="2677d-229">Text inputs are passed to an analyzer, lower-cased, stripped of punctuation, and so forth, depending on the analyzer configuration.</span></span> <span data-ttu-id="2677d-230">Általános, de nem szükséges, az azonos elemzőkkel használandó keresési és indexelési műveletek, hogy lekérdezési kifejezések mint belül az index feltételeket is.</span><span class="sxs-lookup"><span data-stu-id="2677d-230">It's common, but not required, to use the same analyzers for search and indexing operations so that query terms look more like terms inside the index.</span></span>

> [!Note]
> <span data-ttu-id="2677d-231">Az Azure Search lehetővé teszi a különböző elemzőkkel indexeléshez adja meg, és keressen további keresztül `indexAnalyzer` és `searchAnalyzer` paraméterek mezőben.</span><span class="sxs-lookup"><span data-stu-id="2677d-231">Azure Search lets you specify different analyzers for indexing and search via additional `indexAnalyzer` and `searchAnalyzer` field parameters.</span></span> <span data-ttu-id="2677d-232">Ha nincs megadva, a analyzer beállított a `analyzer` a tulajdonságot használja az indexelés és a keresést.</span><span class="sxs-lookup"><span data-stu-id="2677d-232">If unspecified, the analyzer set with the `analyzer` property is used for both indexing and searching.</span></span>  

<span data-ttu-id="2677d-233">**Példa dokumentumok fordított indexe**</span><span class="sxs-lookup"><span data-stu-id="2677d-233">**Inverted index for example documents**</span></span>

<span data-ttu-id="2677d-234">A jelen példában visszatér a **cím** mezőt, a fordított index néz ki:</span><span class="sxs-lookup"><span data-stu-id="2677d-234">Returning to our example, for the **title** field, the inverted index looks like this:</span></span>

| <span data-ttu-id="2677d-235">Időtartam</span><span class="sxs-lookup"><span data-stu-id="2677d-235">Term</span></span> | <span data-ttu-id="2677d-236">Dokumentumok listájához</span><span class="sxs-lookup"><span data-stu-id="2677d-236">Document list</span></span> |
|------|---------------|
| <span data-ttu-id="2677d-237">atman</span><span class="sxs-lookup"><span data-stu-id="2677d-237">atman</span></span> | <span data-ttu-id="2677d-238">1</span><span class="sxs-lookup"><span data-stu-id="2677d-238">1</span></span> |
| <span data-ttu-id="2677d-239">kínál</span><span class="sxs-lookup"><span data-stu-id="2677d-239">beach</span></span> | <span data-ttu-id="2677d-240">2</span><span class="sxs-lookup"><span data-stu-id="2677d-240">2</span></span> |
| <span data-ttu-id="2677d-241">Szálloda</span><span class="sxs-lookup"><span data-stu-id="2677d-241">hotel</span></span> | <span data-ttu-id="2677d-242">1, 3</span><span class="sxs-lookup"><span data-stu-id="2677d-242">1, 3</span></span> |
| <span data-ttu-id="2677d-243">óceáni</span><span class="sxs-lookup"><span data-stu-id="2677d-243">ocean</span></span> | <span data-ttu-id="2677d-244">4</span><span class="sxs-lookup"><span data-stu-id="2677d-244">4</span></span>  |
| <span data-ttu-id="2677d-245">PlayA</span><span class="sxs-lookup"><span data-stu-id="2677d-245">playa</span></span> | <span data-ttu-id="2677d-246">3</span><span class="sxs-lookup"><span data-stu-id="2677d-246">3</span></span> |
| <span data-ttu-id="2677d-247">végső esetben</span><span class="sxs-lookup"><span data-stu-id="2677d-247">resort</span></span> | <span data-ttu-id="2677d-248">3</span><span class="sxs-lookup"><span data-stu-id="2677d-248">3</span></span> |
| <span data-ttu-id="2677d-249">Retreat</span><span class="sxs-lookup"><span data-stu-id="2677d-249">retreat</span></span> | <span data-ttu-id="2677d-250">4</span><span class="sxs-lookup"><span data-stu-id="2677d-250">4</span></span> |

<span data-ttu-id="2677d-251">A cím mezőben csak *Szálloda* mutatja két dokumentumot: 1, 3.</span><span class="sxs-lookup"><span data-stu-id="2677d-251">In the title field, only *hotel* shows up in two documents: 1, 3.</span></span>

<span data-ttu-id="2677d-252">Az a **leírás** mezőben az index a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="2677d-252">For the **description** field, the index is as follows:</span></span>

| <span data-ttu-id="2677d-253">Időtartam</span><span class="sxs-lookup"><span data-stu-id="2677d-253">Term</span></span> | <span data-ttu-id="2677d-254">Dokumentumok listájához</span><span class="sxs-lookup"><span data-stu-id="2677d-254">Document list</span></span> |
|------|---------------|
| <span data-ttu-id="2677d-255">vezeték nélkül</span><span class="sxs-lookup"><span data-stu-id="2677d-255">air</span></span> | <span data-ttu-id="2677d-256">3</span><span class="sxs-lookup"><span data-stu-id="2677d-256">3</span></span>
| <span data-ttu-id="2677d-257">és</span><span class="sxs-lookup"><span data-stu-id="2677d-257">and</span></span> | <span data-ttu-id="2677d-258">4</span><span class="sxs-lookup"><span data-stu-id="2677d-258">4</span></span>
| <span data-ttu-id="2677d-259">kínál</span><span class="sxs-lookup"><span data-stu-id="2677d-259">beach</span></span> | <span data-ttu-id="2677d-260">1</span><span class="sxs-lookup"><span data-stu-id="2677d-260">1</span></span>
| <span data-ttu-id="2677d-261">annak</span><span class="sxs-lookup"><span data-stu-id="2677d-261">conditioned</span></span> | <span data-ttu-id="2677d-262">3</span><span class="sxs-lookup"><span data-stu-id="2677d-262">3</span></span>
| <span data-ttu-id="2677d-263">Feladatkezelő</span><span class="sxs-lookup"><span data-stu-id="2677d-263">comfortable</span></span> | <span data-ttu-id="2677d-264">3</span><span class="sxs-lookup"><span data-stu-id="2677d-264">3</span></span>
| <span data-ttu-id="2677d-265">távolság</span><span class="sxs-lookup"><span data-stu-id="2677d-265">distance</span></span> | <span data-ttu-id="2677d-266">1</span><span class="sxs-lookup"><span data-stu-id="2677d-266">1</span></span>
| <span data-ttu-id="2677d-267">sziget</span><span class="sxs-lookup"><span data-stu-id="2677d-267">island</span></span> | <span data-ttu-id="2677d-268">2</span><span class="sxs-lookup"><span data-stu-id="2677d-268">2</span></span>
| <span data-ttu-id="2677d-269">kauaʻi</span><span class="sxs-lookup"><span data-stu-id="2677d-269">kauaʻi</span></span> | <span data-ttu-id="2677d-270">2</span><span class="sxs-lookup"><span data-stu-id="2677d-270">2</span></span>
| <span data-ttu-id="2677d-271">található</span><span class="sxs-lookup"><span data-stu-id="2677d-271">located</span></span> | <span data-ttu-id="2677d-272">2</span><span class="sxs-lookup"><span data-stu-id="2677d-272">2</span></span>
| <span data-ttu-id="2677d-273">északi régiója</span><span class="sxs-lookup"><span data-stu-id="2677d-273">north</span></span> | <span data-ttu-id="2677d-274">2</span><span class="sxs-lookup"><span data-stu-id="2677d-274">2</span></span>
| <span data-ttu-id="2677d-275">óceáni</span><span class="sxs-lookup"><span data-stu-id="2677d-275">ocean</span></span> | <span data-ttu-id="2677d-276">1, 2, 3</span><span class="sxs-lookup"><span data-stu-id="2677d-276">1, 2, 3</span></span>
| <span data-ttu-id="2677d-277">/</span><span class="sxs-lookup"><span data-stu-id="2677d-277">of</span></span> | <span data-ttu-id="2677d-278">2</span><span class="sxs-lookup"><span data-stu-id="2677d-278">2</span></span>
| <span data-ttu-id="2677d-279">a</span><span class="sxs-lookup"><span data-stu-id="2677d-279">on</span></span> |<span data-ttu-id="2677d-280">2</span><span class="sxs-lookup"><span data-stu-id="2677d-280">2</span></span>
| <span data-ttu-id="2677d-281">Csendes</span><span class="sxs-lookup"><span data-stu-id="2677d-281">quiet</span></span> | <span data-ttu-id="2677d-282">4</span><span class="sxs-lookup"><span data-stu-id="2677d-282">4</span></span>
| <span data-ttu-id="2677d-283">helyiségekben</span><span class="sxs-lookup"><span data-stu-id="2677d-283">rooms</span></span>  | <span data-ttu-id="2677d-284">1, 3</span><span class="sxs-lookup"><span data-stu-id="2677d-284">1, 3</span></span>
| <span data-ttu-id="2677d-285">secluded</span><span class="sxs-lookup"><span data-stu-id="2677d-285">secluded</span></span> | <span data-ttu-id="2677d-286">4</span><span class="sxs-lookup"><span data-stu-id="2677d-286">4</span></span>
| <span data-ttu-id="2677d-287">parti</span><span class="sxs-lookup"><span data-stu-id="2677d-287">shore</span></span> | <span data-ttu-id="2677d-288">2</span><span class="sxs-lookup"><span data-stu-id="2677d-288">2</span></span>
| <span data-ttu-id="2677d-289">Ahhoz</span><span class="sxs-lookup"><span data-stu-id="2677d-289">spacious</span></span> | <span data-ttu-id="2677d-290">1</span><span class="sxs-lookup"><span data-stu-id="2677d-290">1</span></span>
| <span data-ttu-id="2677d-291">a</span><span class="sxs-lookup"><span data-stu-id="2677d-291">the</span></span> | <span data-ttu-id="2677d-292">1, 2</span><span class="sxs-lookup"><span data-stu-id="2677d-292">1, 2</span></span>
| <span data-ttu-id="2677d-293">erre:</span><span class="sxs-lookup"><span data-stu-id="2677d-293">to</span></span> | <span data-ttu-id="2677d-294">1</span><span class="sxs-lookup"><span data-stu-id="2677d-294">1</span></span>
| <span data-ttu-id="2677d-295">megtekintés</span><span class="sxs-lookup"><span data-stu-id="2677d-295">view</span></span> | <span data-ttu-id="2677d-296">1, 2, 3</span><span class="sxs-lookup"><span data-stu-id="2677d-296">1, 2, 3</span></span>
| <span data-ttu-id="2677d-297">érdekében</span><span class="sxs-lookup"><span data-stu-id="2677d-297">walking</span></span> | <span data-ttu-id="2677d-298">1</span><span class="sxs-lookup"><span data-stu-id="2677d-298">1</span></span>
| <span data-ttu-id="2677d-299">a</span><span class="sxs-lookup"><span data-stu-id="2677d-299">with</span></span> | <span data-ttu-id="2677d-300">3</span><span class="sxs-lookup"><span data-stu-id="2677d-300">3</span></span>


<span data-ttu-id="2677d-301">**Egyező lekérdezési kifejezések indexelt feltételek ellen**</span><span class="sxs-lookup"><span data-stu-id="2677d-301">**Matching query terms against indexed terms**</span></span>

<span data-ttu-id="2677d-302">A fenti fordított indexek, most térjen vissza a mintalekérdezés, és hogyan egyező dokumentumok tekintse meg a példalekérdezés talált.</span><span class="sxs-lookup"><span data-stu-id="2677d-302">Given the inverted indices above, let’s return to the sample query and see how matching documents are found for our example query.</span></span> <span data-ttu-id="2677d-303">Visszahívása, hogy az utolsó lekérdezési fa néz ki:</span><span class="sxs-lookup"><span data-stu-id="2677d-303">Recall that the final query tree looks like this:</span></span> 

 ![Logikai lekérdezés elemzett adatokkal][4]

<span data-ttu-id="2677d-305">Lekérdezés-végrehajtás során egyes lekérdezések végrehajtása a kereshető mezők elleni egymástól függetlenül.</span><span class="sxs-lookup"><span data-stu-id="2677d-305">During query execution, individual queries are executed against the searchable fields independently.</span></span> 

+ <span data-ttu-id="2677d-306">A TermQuery, "ahhoz,", megegyezik a dokumentum 1 (szálloda Atman).</span><span class="sxs-lookup"><span data-stu-id="2677d-306">The TermQuery, "spacious", matches document 1 (Hotel Atman).</span></span> 

+ <span data-ttu-id="2677d-307">A PrefixQuery "air-condition *", nem felel meg a dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="2677d-307">The PrefixQuery, "air-condition*", doesn't match any documents.</span></span> 

  <span data-ttu-id="2677d-308">Ez az, hogy a fejlesztők néha confuses.</span><span class="sxs-lookup"><span data-stu-id="2677d-308">This is a behavior that sometimes confuses developers.</span></span> <span data-ttu-id="2677d-309">Bár klimatizált kifejezés szerepel a dokumentum, azt oszlik két feltételek által az alapértelmezett elemző eszközt.</span><span class="sxs-lookup"><span data-stu-id="2677d-309">Although the term air-conditioned exists in the document, it is split into two terms by the default analyzer.</span></span> <span data-ttu-id="2677d-310">Visszahívása előtag a lekérdezések részleges feltételeket tartalmaz, amelyek nem került sor.</span><span class="sxs-lookup"><span data-stu-id="2677d-310">Recall that prefix queries, which contain partial terms, are not analyzed.</span></span> <span data-ttu-id="2677d-311">Ezért a kifejezéseket "air-condition" előtag a fordított index keresni, és nem található.</span><span class="sxs-lookup"><span data-stu-id="2677d-311">Therefore terms with prefix "air-condition" are looked up in the inverted index and not found.</span></span>

+ <span data-ttu-id="2677d-312">A PhraseQuery "óceáni nézet", a feltételek "óceáni" és "view" keresése, és ellenőrzi a közelében, az eredeti dokumentumhoz kifejezések.</span><span class="sxs-lookup"><span data-stu-id="2677d-312">The PhraseQuery, "ocean view", looks up the terms "ocean" and "view" and checks the proximity of terms in the original document.</span></span> <span data-ttu-id="2677d-313">Dokumentumok, 1, 2, 3 felel meg a lekérdezés a Leírás mezőben.</span><span class="sxs-lookup"><span data-stu-id="2677d-313">Documents 1, 2 and 3 match this query in the description field.</span></span> <span data-ttu-id="2677d-314">Figyelje meg a dokumentum 4 a kifejezés óceáni a címben rendelkezik, de nem tekinthető egyezés, azt keres egyes szavak helyett a "óceáni nézet" kifejezés helyett szerepel.</span><span class="sxs-lookup"><span data-stu-id="2677d-314">Notice document 4 has the term ocean in the title but isn’t considered a match, as we're looking for the "ocean view" phrase rather than individual words.</span></span> 

> [!Note]
> <span data-ttu-id="2677d-315">Keresési lekérdezés végrehajtása egymástól függetlenül szemben az összes kereshető mezőt az Azure Search-index kivéve korlátozhatja a mezőket, állítsa a `searchFields` paraméter, a keresési kérelem (példa) ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="2677d-315">A search query is executed independently against all searchable fields in the Azure Search index unless you limit the fields set with the `searchFields` parameter, as illustrated in the example search request.</span></span> <span data-ttu-id="2677d-316">A kijelölt mezőket egyik megfelelő dokumentumok adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2677d-316">Documents that match in any of the selected fields are returned.</span></span> 

<span data-ttu-id="2677d-317">A teljes az adott lekérdezéshez megfelelő dokumentumok 1, 2, 3.</span><span class="sxs-lookup"><span data-stu-id="2677d-317">On the whole, for the query in question, the documents that match are 1, 2, 3.</span></span> 

## <a name="stage-4-scoring"></a><span data-ttu-id="2677d-318">4. szakasz: pontozási</span><span class="sxs-lookup"><span data-stu-id="2677d-318">Stage 4: Scoring</span></span>  

<span data-ttu-id="2677d-319">Minden egyes dokumentum egy keresési eredménykészletben hozzá van rendelve egy relevanciájának pontszám.</span><span class="sxs-lookup"><span data-stu-id="2677d-319">Every document in a search result set is assigned a relevance score.</span></span> <span data-ttu-id="2677d-320">A relevanciájának pontszám feladata a nagyobb a dimenziószáma ezeket a dokumentumokat, amely a legjobb felhasználói kérdések megválaszolása, ahogy a keresési lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="2677d-320">The function of the relevance score is to rank higher those documents that best answer a user question as expressed by the search query.</span></span> <span data-ttu-id="2677d-321">A pontszám egyező kifejezések statisztikai tulajdonságok alapján számítja ki.</span><span class="sxs-lookup"><span data-stu-id="2677d-321">The score is computed based on statistical properties of terms that matched.</span></span> <span data-ttu-id="2677d-322">A pontozási képlet középpontjában van [TF/IDF (kifejezés gyakoriság-inverz dokumentum gyakoriság)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).</span><span class="sxs-lookup"><span data-stu-id="2677d-322">At the core of the scoring formula is [TF/IDF (term frequency-inverse document frequency)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).</span></span> <span data-ttu-id="2677d-323">Ritka és gyakori feltételeket tartalmazó lekérdezésekben TF/IDF elősegíti a ritka kifejezés, amely a.</span><span class="sxs-lookup"><span data-stu-id="2677d-323">In queries containing rare and common terms, TF/IDF promotes results containing the rare term.</span></span> <span data-ttu-id="2677d-324">Például egy elméleti index összes Wikipedia cikkekkel, a dokumentumok, amely egyezik a lekérdezés *elnökét*, a megfelelő dokumentumok *elnök* relevánsnak több mint a megfelelő dokumentumok *a*.</span><span class="sxs-lookup"><span data-stu-id="2677d-324">For example, in a hypothetical index with all Wikipedia articles, from documents that matched the query *the president*, documents matching on *president* are considered more relevant than documents matching on *the*.</span></span>


### <a name="scoring-example"></a><span data-ttu-id="2677d-325">A pontozási – példa</span><span class="sxs-lookup"><span data-stu-id="2677d-325">Scoring example</span></span>

<span data-ttu-id="2677d-326">A három dokumentumok a példalekérdezés egyező visszahívása:</span><span class="sxs-lookup"><span data-stu-id="2677d-326">Recall the three documents that matched our example query:</span></span>
~~~~
search=Spacious, air-condition* +"Ocean view"  
~~~~
~~~~
{  
  "value": [
    {
      "@search.score": 0.25610128,
      "id": "1",
      "title": "Hotel Atman",
      "description": "Spacious rooms, ocean view, walking distance to the beach."
    },
    {
      "@search.score": 0.08951007,
      "id": "3",
      "title": "Playa Hotel",
      "description": "Comfortable, air-conditioned rooms with ocean view."
    },
    {
      "@search.score": 0.05967338,
      "id": "2",
      "title": "Ocean Resort",
      "description": "Located on a cliff on the north shore of the island of Kauai. Ocean view."
    }
  ]
}
~~~~

<span data-ttu-id="2677d-327">1 felelt meg a lekérdezés legjobb mert mindkét kifejezés *ahhoz,* és a szükséges kódot *óceáni nézet* fordulhat elő, a Leírás mezőben.</span><span class="sxs-lookup"><span data-stu-id="2677d-327">Document 1 matched the query best because both the term *spacious* and the required phrase *ocean view* occur in the description field.</span></span> <span data-ttu-id="2677d-328">A következő két dokumentumok felel meg a kifejezés csak *óceáni nézet*.</span><span class="sxs-lookup"><span data-stu-id="2677d-328">The next two documents match only the phrase *ocean view*.</span></span> <span data-ttu-id="2677d-329">Előfordulhat, hogy meglepő, hogy a dokumentum 2 és 3 relevanciájának pontszám eltér, annak ellenére, hogy azok a lekérdezés egyező azonos módon.</span><span class="sxs-lookup"><span data-stu-id="2677d-329">It might be surprising that the relevance score for document 2 and 3 is different even though they matched the query in the same way.</span></span> <span data-ttu-id="2677d-330">Ennek az oka a pontozási képlet csak TF/IDF-nál több részből áll.</span><span class="sxs-lookup"><span data-stu-id="2677d-330">It's because the scoring formula has more components than just TF/IDF.</span></span> <span data-ttu-id="2677d-331">Ebben az esetben dokumentum 3 van hozzárendelve valamivel nagyobb pontszám, mert annak leírását rövidebb.</span><span class="sxs-lookup"><span data-stu-id="2677d-331">In this case, document 3 was assigned a slightly higher score because its description is shorter.</span></span> <span data-ttu-id="2677d-332">További tudnivalók [Lucene tartozó gyakorlati pontozási képlet](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) megértése, hogyan mező maximális hossza és más tényezők befolyásolhatják a a relevanciájának pontszámot.</span><span class="sxs-lookup"><span data-stu-id="2677d-332">Learn about [Lucene's Practical Scoring Formula](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) to understand how field length and other factors can influence the relevance score.</span></span>

<span data-ttu-id="2677d-333">Néhány lekérdezés (helyettesítő, előtag, regex) típusú mindig hozzájárulhatnak a a teljes dokumentum pontszám állandó pontszámot.</span><span class="sxs-lookup"><span data-stu-id="2677d-333">Some query types (wildcard, prefix, regex) always contribute a constant score to the overall document score.</span></span> <span data-ttu-id="2677d-334">Ez lehetővé teszi, hogy a találat keresztül lekérdezés bővítése foglalandó az eredményeket, de az nem befolyásolja a rangsorolási.</span><span class="sxs-lookup"><span data-stu-id="2677d-334">This allows matches found through query expansion to be included in the results, but without affecting the ranking.</span></span> 

<span data-ttu-id="2677d-335">Egy példa azt mutatja be, ezért ez számít.</span><span class="sxs-lookup"><span data-stu-id="2677d-335">An example illustrates why this matters.</span></span> <span data-ttu-id="2677d-336">Helyettesítő karakteres kereséssel előtag keresések, beleértve egyértelműek definition, mert a bemeneti érték egy részleges karakterlánc lehetséges megegyezik (vel, fontolja meg egy bemeneti "bemutató *", "bemutatók", "tourettes" és "tourmaline" találat) különböző feltételeket nagyon nagy számú.</span><span class="sxs-lookup"><span data-stu-id="2677d-336">Wildcard searches, including prefix searches, are ambiguous by definition because the input is a partial string with potential matches on a very large number of disparate terms (consider an input of "tour*", with matches found on “tours”, “tourettes”, and “tourmaline”).</span></span> <span data-ttu-id="2677d-337">Ezekkel az eredményekkel jellegéből, nincs mód ésszerűen következtetési forrásaként, mely feltételek értékesebb, mint a többire.</span><span class="sxs-lookup"><span data-stu-id="2677d-337">Given the nature of these results, there is no way to reasonably infer which terms are more valuable than others.</span></span> <span data-ttu-id="2677d-338">Ezért azt figyelmen kívül hagyása kifejezés gyakoriságot amikor pontozási eredményei típusok helyettesítő, előtag és regex lekérdezésekben.</span><span class="sxs-lookup"><span data-stu-id="2677d-338">For this reason, we ignore term frequencies when scoring results in queries of types wildcard, prefix and regex.</span></span> <span data-ttu-id="2677d-339">A többrészes keresési kérelmet, amely részleges és teljes feltételeket tartalmaz, a részleges bemeneti eredmények elkerülése érdekében a potenciálisan váratlan egyezések felé időeltérés állandó pontszám feldolgozásába beépített.</span><span class="sxs-lookup"><span data-stu-id="2677d-339">In a multi-part search request that includes partial and complete terms, results from the partial input are incorporated with a constant score to avoid bias towards potentially unexpected matches.</span></span>

### <a name="score-tuning"></a><span data-ttu-id="2677d-340">Pontszám hangolása</span><span class="sxs-lookup"><span data-stu-id="2677d-340">Score tuning</span></span>

<span data-ttu-id="2677d-341">Az Azure Search relevanciájának pontszámok hangolására két módja van:</span><span class="sxs-lookup"><span data-stu-id="2677d-341">There are two ways to tune relevance scores in Azure Search:</span></span>

1. <span data-ttu-id="2677d-342">**A pontozási profil** lépteti elő a dokumentumokat a meghatározott szabályok alapján eredmények rangsorolt listáját.</span><span class="sxs-lookup"><span data-stu-id="2677d-342">**Scoring profiles** promote documents in the ranked list of results based on a set of rules.</span></span> <span data-ttu-id="2677d-343">A fenti példában a dokumentumok, a Leírás mezőben egyező-nál több megfelelő cím mezőben egyező dokumentumok volt javasolt.</span><span class="sxs-lookup"><span data-stu-id="2677d-343">In our example, we could consider documents that matched in the title field more relevant than documents that matched in the description field.</span></span> <span data-ttu-id="2677d-344">Emellett az indexnek minden Szálloda ár mező volna, ha azt sikerült előléptetni alsó ár dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="2677d-344">Additionally, if our index had a price field for each hotel, we could promote documents with lower price.</span></span> <span data-ttu-id="2677d-345">További tudnivalók a [pontozási profil hozzáadása egy keresési indexszel.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)</span><span class="sxs-lookup"><span data-stu-id="2677d-345">Learn more how to [add Scoring Profiles to a search index.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)</span></span>
2. <span data-ttu-id="2677d-346">**Távon kiemelési** (csak a teljes Lucene lekérdezés szintaxisát elérhető) biztosít a kiemelési operátor `^` , amelyek alkalmazhatók a lekérdezés fa bármely részét.</span><span class="sxs-lookup"><span data-stu-id="2677d-346">**Term boosting** (available only in the Full Lucene query syntax) provides a boosting operator `^` that can be applied to any part of the query tree.</span></span> <span data-ttu-id="2677d-347">Ebben a példában az előtag alapján keres helyett *air-condition*\*, egy megkeresése, amelyek vagy a pontos kifejezés *air-condition* vagy az előtag, de a pontos kifejezés a megfelelő dokumentumok rendszer előrébb program alkalmaz a kifejezés lekérdezés: *vezeték nélkül-feltétel ^ 2. Air-condition**.</span><span class="sxs-lookup"><span data-stu-id="2677d-347">In our example, instead of searching on the prefix *air-condition*\*, one could search for either the exact term *air-condition* or the prefix, but documents that match on the exact term are ranked higher by applying boost to the term query: *air-condition^2||air-condition**.</span></span> <span data-ttu-id="2677d-348">További információ [távon kiemelési](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).</span><span class="sxs-lookup"><span data-stu-id="2677d-348">Learn more about [term boosting](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).</span></span>


### <a name="scoring-in-a-distributed-index"></a><span data-ttu-id="2677d-349">Egy elosztott index pontozási</span><span class="sxs-lookup"><span data-stu-id="2677d-349">Scoring in a distributed index</span></span>

<span data-ttu-id="2677d-350">Az indexek, az Azure Search automatikusan felosztása több szilánkok gyors terjesztése során szolgáltatás méretezési az index több csomópont között, így akár vagy csökkentheti.</span><span class="sxs-lookup"><span data-stu-id="2677d-350">All indexes in Azure Search are automatically split into multiple shards, allowing us to quickly distribute the index among multiple nodes during service scale up or scale down.</span></span> <span data-ttu-id="2677d-351">Keresési kérelem elküldésekor megjelenítés elleni minden shard egymástól függetlenül.</span><span class="sxs-lookup"><span data-stu-id="2677d-351">When a search request is issued, it’s issued against each shard independently.</span></span> <span data-ttu-id="2677d-352">Minden shard eredményeinek majd egyesített és (Ha nincs más rendezés van meghatározva) pontszám szerint rendezve.</span><span class="sxs-lookup"><span data-stu-id="2677d-352">The results from each shard are then merged and ordered by score (if no other ordering is defined).</span></span> <span data-ttu-id="2677d-353">Fontos tudni, hogy a pontozó függvény súlyok összes szilánkok között nem kifejezés gyakoriság szemben a más néven inverz dokumentum gyakoriságot adja meg a shard minden dokumentumot lekérdezése!</span><span class="sxs-lookup"><span data-stu-id="2677d-353">It is important to know that the scoring function weights query term frequency against its inverse document frequency in all documents within the shard, not across all shards!</span></span>

<span data-ttu-id="2677d-354">Ez azt jelenti, hogy egy relevanciájának pontszám *sikerült* térhet azonos dokumentumokhoz, ha ezek különböző szilánkok legyen elhelyezve.</span><span class="sxs-lookup"><span data-stu-id="2677d-354">This means a relevance score *could* be different for identical documents if they reside on different shards.</span></span> <span data-ttu-id="2677d-355">Szerencsére ezek az eltérések általában az indexelt dokumentumok száma miatt több még kifejezés terjesztési növekedésével eltűnik.</span><span class="sxs-lookup"><span data-stu-id="2677d-355">Fortunately, such differences tend to disappear as the number of documents in the index grows due to more even term distribution.</span></span> <span data-ttu-id="2677d-356">Nincs lehetőség számára, hogy mely shard az adott dokumentumtípus kerülnek.</span><span class="sxs-lookup"><span data-stu-id="2677d-356">It’s not possible to assume on which shard any given document will be placed.</span></span> <span data-ttu-id="2677d-357">Azonban ha egy dokumentum kulcs nem változik, az mindig rendeli hozzá ugyanazt a shard.</span><span class="sxs-lookup"><span data-stu-id="2677d-357">However, assuming a document key doesn't change, it will always be assigned to the same shard.</span></span>

<span data-ttu-id="2677d-358">A dokumentum pontszám általában nem a legjobb attribútum dokumentumok rendezéshez, ha fontos a sorrend stabilitását.</span><span class="sxs-lookup"><span data-stu-id="2677d-358">In general, document score is not the best attribute for ordering documents if order stability is important.</span></span> <span data-ttu-id="2677d-359">Például egy azonos pontszám két dokumentum megadott, nincs garancia melyik jelenik meg először a következő frissítési kísérletei során ugyanabban a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="2677d-359">For example, given two document with an identical score, there is no guarantee which one appears first in subsequent runs of the same query.</span></span> <span data-ttu-id="2677d-360">A dokumentum pontszám csak adjon más dokumentumok viszonyítva dokumentum fontos általános értelemben eredmények készletében.</span><span class="sxs-lookup"><span data-stu-id="2677d-360">Document score should only give a general sense of document relevance relative to other documents in the results set.</span></span>

## <a name="conclusion"></a><span data-ttu-id="2677d-361">Összegzés</span><span class="sxs-lookup"><span data-stu-id="2677d-361">Conclusion</span></span>

<span data-ttu-id="2677d-362">Internetes keresők sikeres titkos adatok a teljes szöveges keresés elvárásainak idézett elő.</span><span class="sxs-lookup"><span data-stu-id="2677d-362">The success of internet search engines has raised expectations for full text search over private data.</span></span> <span data-ttu-id="2677d-363">Szinte bármilyen keresési élményt biztosít terveink most már a megérteni a leképezést, akkor is, ha feltételek hibás vagy hiányos a motor.</span><span class="sxs-lookup"><span data-stu-id="2677d-363">For almost any kind of search experience, we now expect the engine to understand our intent, even when terms are misspelled or incomplete.</span></span> <span data-ttu-id="2677d-364">Akkor is előfordulhat, hogy várhatóan közelében kifejezések vagy soha nem ténylegesen megadott szinonimák alapján.</span><span class="sxs-lookup"><span data-stu-id="2677d-364">We might even expect matches based on near equivalent terms or synonyms that we never actually specified.</span></span>

<span data-ttu-id="2677d-365">Technikai szempontból teljes szöveges keresés nagyon összetett, nyelvi kifinomult elemzést és, hogy az eszközöket, bontsa ki és lekérdezési kifejezések képes biztosítani a megfelelő eredmény átalakítási feldolgozási rendszeres megközelítése.</span><span class="sxs-lookup"><span data-stu-id="2677d-365">From a technical standpoint, full text search is highly complex, requiring sophisticated linguistic analysis and a systematic approach to processing in ways that distill, expand, and transform query terms to deliver a relevant result.</span></span> <span data-ttu-id="2677d-366">Adott a rejlő összetett szolgáltatásokkal, nagy mennyiségű tényezőket, amelyek hatással lehetnek a lekérdezés eredményeit vannak.</span><span class="sxs-lookup"><span data-stu-id="2677d-366">Given the inherent complexities, there are a lot of factors that can affect the outcome of a query.</span></span> <span data-ttu-id="2677d-367">Emiatt beállítás esetén a teljes szöveges keresés megismerése időt befektetés előnyökkel anyagi közben váratlan eredményekhez keresztül működnek.</span><span class="sxs-lookup"><span data-stu-id="2677d-367">For this reason, investing the time to understand the mechanics of full text search offers tangible benefits when trying to work through unexpected results.</span></span>  

<span data-ttu-id="2677d-368">Ez a cikk felfedezte az Azure Search kontextusában a teljes szöveges keresés.</span><span class="sxs-lookup"><span data-stu-id="2677d-368">This article explored full text search in the context of Azure Search.</span></span> <span data-ttu-id="2677d-369">Reméljük, biztosít elegendő háttér ismeri fel a lehetséges okokért és megoldásokért lekérdezés kapcsolatos gyakori problémák címzéshez.</span><span class="sxs-lookup"><span data-stu-id="2677d-369">We hope it gives you sufficient background to recognize potential causes and resolutions for addressing common query problems.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="2677d-370">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2677d-370">Next steps</span></span>

+ <span data-ttu-id="2677d-371">Az minta index létrehozása, próbálja ki a különböző lekérdezéseket, és tekintse át az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="2677d-371">Build the sample index, try out different queries and review results.</span></span> <span data-ttu-id="2677d-372">Útmutatásért lásd: [hozza létre, és a portál egy index lekérdezése](search-get-started-portal.md#query-index).</span><span class="sxs-lookup"><span data-stu-id="2677d-372">For instructions, see [Build and query an index in the portal](search-get-started-portal.md#query-index).</span></span>

+ <span data-ttu-id="2677d-373">Próbálja meg a további lekérdezés szintaxisa a [dokumentumok keresése](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) példa szakasz vagy [egyszerű lekérdezés szintaxisát](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) a keresési ablak a portálon.</span><span class="sxs-lookup"><span data-stu-id="2677d-373">Try additional query syntax from the [Search Documents](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) example section or from [Simple query syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) in Search explorer in the portal.</span></span>

+ <span data-ttu-id="2677d-374">Felülvizsgálati [profilok pontozási](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) Ha azt szeretné, hogy az alkalmazás rangsorolási finomhangolásához.</span><span class="sxs-lookup"><span data-stu-id="2677d-374">Review [scoring profiles](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) if you want to tune ranking in your search application.</span></span>

+ <span data-ttu-id="2677d-375">Megtudhatja, hogyan alkalmazza [nyelvspecifikus lexikális elemzőkkel](https://docs.microsoft.com/rest/api/searchservice/language-support).</span><span class="sxs-lookup"><span data-stu-id="2677d-375">Learn how to apply [language-specific lexical analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support).</span></span>

+ <span data-ttu-id="2677d-376">[Konfigurálja az egyéni lekérdezések](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) minimális feldolgozási vagy adott mezők speciális terhelése.</span><span class="sxs-lookup"><span data-stu-id="2677d-376">[Configure custom analyzers](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) for either minimal processing or specialized processing on specific fields.</span></span>

+ <span data-ttu-id="2677d-377">[Hasonlítsa össze a szabványos és az angol nyelvű elemzőkkel](http://alice.unearth.ai/))-mellé a bemutató webhelyen.</span><span class="sxs-lookup"><span data-stu-id="2677d-377">[Compare standard and English analyzers](http://alice.unearth.ai/)) side-by-side on this demo web site.</span></span> 

## <a name="see-also"></a><span data-ttu-id="2677d-378">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="2677d-378">See also</span></span>

[<span data-ttu-id="2677d-379">REST API-t dokumentumok keresése</span><span class="sxs-lookup"><span data-stu-id="2677d-379">Search Documents REST API</span></span>](https://docs.microsoft.com/rest/api/searchservice/search-documents)

[<span data-ttu-id="2677d-380">Egyszerű lekérdezési szintaxis</span><span class="sxs-lookup"><span data-stu-id="2677d-380">Simple query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)

[<span data-ttu-id="2677d-381">Teljes Lucene lekérdezés szintaxisa</span><span class="sxs-lookup"><span data-stu-id="2677d-381">Full Lucene query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

[<span data-ttu-id="2677d-382">A keresési eredmények kezelése</span><span class="sxs-lookup"><span data-stu-id="2677d-382">Handle search results</span></span>](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
[2]: ./media/search-lucene-query-architecture/azSearch-queryparsing-should2.png
[3]: ./media/search-lucene-query-architecture/azSearch-queryparsing-must2.png
[4]: ./media/search-lucene-query-architecture/azSearch-queryparsing-spacious2.png
