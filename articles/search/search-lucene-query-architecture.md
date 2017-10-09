---
title: "az Azure Search aaaFull szöveges keresési motor (Lucene) architektúra |} Microsoft Docs"
description: "MAGYARÁZAT Lucene lekérdezés feldolgozása és dokumentum beolvasása fogalmak a teljes szöveges keresés, mint a kapcsolódó tooAzure keresési."
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
ms.openlocfilehash: c6d1bea8d40154fd9237b9e44584cdfcd193cbd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-full-text-search-works-in-azure-search"></a><span data-ttu-id="05fbe-103">Hogyan teljes szöveges keresés az Azure Search működik</span><span class="sxs-lookup"><span data-stu-id="05fbe-103">How full text search works in Azure Search</span></span>

<span data-ttu-id="05fbe-104">Ez a cikk a fejlesztők számára, akik bemutatják, hogyan Lucene teljes szöveges keresés működik-e az Azure Search van.</span><span class="sxs-lookup"><span data-stu-id="05fbe-104">This article is for developers who need a deeper understanding of how Lucene full text search works in Azure Search.</span></span> <span data-ttu-id="05fbe-105">A szöveges lekérdezések Azure Search problémamentesen fog továbbítani a kívánt eredmény elérése érdekében a legtöbb esetben azonban úgy tűnik, "off" valamilyen módon eredményt időnként előfordulhat, hogy.</span><span class="sxs-lookup"><span data-stu-id="05fbe-105">For text queries, Azure Search will seamlessly deliver expected results in most scenarios, but occasionally you might get a result that seems "off" somehow.</span></span> <span data-ttu-id="05fbe-106">Ezekben a helyzetekben háttér rendelkező hello Lucene lekérdezés-végrehajtás négy szakaszainak (elemzési, lexikális elemzés lekérdezéséhez dokumentálja a megfelelő pontozási) segítségével azonosíthatja az adott módosítások tooquery paraméterek vagy, hogy a rendszer hello konfigurációs index kívánt eredmény.</span><span class="sxs-lookup"><span data-stu-id="05fbe-106">In these situations, having a background in hello four stages of Lucene query execution (query parsing, lexical analysis, document matching, scoring) can help you identify specific changes tooquery parameters or index configuration that will deliver hello desired outcome.</span></span> 

> [!Note] 
> <span data-ttu-id="05fbe-107">Az Azure Search Lucene használ a teljes szöveges keresés, de Lucene integrációs nem teljes.</span><span class="sxs-lookup"><span data-stu-id="05fbe-107">Azure Search uses Lucene for full text search, but Lucene integration is not exhaustive.</span></span> <span data-ttu-id="05fbe-108">A Microsoft szelektív módon teszi közzé, és a Lucene funkció tooenable hello forgatókönyvek fontos tooAzure keresés kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="05fbe-108">We selectively expose and extend Lucene functionality tooenable hello scenarios important tooAzure Search.</span></span> 

## <a name="architecture-overview-and-diagram"></a><span data-ttu-id="05fbe-109">Architektúra áttekintése és diagramja</span><span class="sxs-lookup"><span data-stu-id="05fbe-109">Architecture overview and diagram</span></span>

<span data-ttu-id="05fbe-110">Teljes szöveges keresési lekérdezés feldolgozása kezdődik-e elemzés hello lekérdezés szöveges tooextract keresési feltételeket.</span><span class="sxs-lookup"><span data-stu-id="05fbe-110">Processing a full text search query starts with parsing hello query text tooextract search terms.</span></span> <span data-ttu-id="05fbe-111">hello keresőmotor egy index tooretrieve dokumentumok használja a megfelelő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="05fbe-111">hello search engine uses an index tooretrieve documents with matching terms.</span></span> <span data-ttu-id="05fbe-112">Egyes lekérdezések néha bontásban használati elkészített az új űrlapok toocast szélesebb körű nettó keresztül mi tekinthető, amely lehetséges.</span><span class="sxs-lookup"><span data-stu-id="05fbe-112">Individual query terms are sometimes broken down and reconstituted into new forms toocast a broader net over what could be considered as a potential match.</span></span> <span data-ttu-id="05fbe-113">Egy eredménykészlet majd relevanciájának pontszám hozzárendelt tooeach egyedi egyező dokumentum szerint van rendezve.</span><span class="sxs-lookup"><span data-stu-id="05fbe-113">A result set is then sorted by a relevance score assigned tooeach individual matching document.</span></span> <span data-ttu-id="05fbe-114">A sorrend hello hello tetején adja vissza toohello hívó alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="05fbe-114">Those at hello top of hello ranked list are returned toohello calling application.</span></span>

<span data-ttu-id="05fbe-115">Lekérdezés-végrehajtás rendelkezik állapítani, négy fázisból áll:</span><span class="sxs-lookup"><span data-stu-id="05fbe-115">Restated, query execution has four stages:</span></span> 

1. <span data-ttu-id="05fbe-116">A lekérdezés elemzése</span><span class="sxs-lookup"><span data-stu-id="05fbe-116">Query parsing</span></span> 
2. <span data-ttu-id="05fbe-117">Lexikális elemzés</span><span class="sxs-lookup"><span data-stu-id="05fbe-117">Lexical analysis</span></span> 
3. <span data-ttu-id="05fbe-118">A dokumentum beolvasása</span><span class="sxs-lookup"><span data-stu-id="05fbe-118">Document retrieval</span></span> 
4. <span data-ttu-id="05fbe-119">Pontozó</span><span class="sxs-lookup"><span data-stu-id="05fbe-119">Scoring</span></span> 

<span data-ttu-id="05fbe-120">az alábbi ábrán hello hello használt összetevőknek tooprocess keresési kérelem mutatja be.</span><span class="sxs-lookup"><span data-stu-id="05fbe-120">hello diagram below illustrates hello components used tooprocess a search request.</span></span> 

 ![Az Azure Search Lucene lekérdezés-architektúra ábrája][1]


| <span data-ttu-id="05fbe-122">A legfontosabb összetevők</span><span class="sxs-lookup"><span data-stu-id="05fbe-122">Key components</span></span> | <span data-ttu-id="05fbe-123">Funkcionális leírása</span><span class="sxs-lookup"><span data-stu-id="05fbe-123">Functional description</span></span> | 
|----------------|------------------------|
|<span data-ttu-id="05fbe-124">**Lekérdezés elemzők**</span><span class="sxs-lookup"><span data-stu-id="05fbe-124">**Query parsers**</span></span> | <span data-ttu-id="05fbe-125">A lekérdezési operátorok lekérdezési kifejezések elválasztása, és hozzon létre hello lekérdezés struktúra (a lekérdezés-fa) küldött toobe toohello keresőmotort.</span><span class="sxs-lookup"><span data-stu-id="05fbe-125">Separate query terms from query operators and create hello query structure (a query tree) toobe sent toohello search engine.</span></span> |
|<span data-ttu-id="05fbe-126">**Lekérdezések**</span><span class="sxs-lookup"><span data-stu-id="05fbe-126">**Analyzers**</span></span> | <span data-ttu-id="05fbe-127">Lexikális elemzést lekérdezés igényei szerint.</span><span class="sxs-lookup"><span data-stu-id="05fbe-127">Perform lexical analysis on query terms.</span></span> <span data-ttu-id="05fbe-128">Ez a folyamat magába foglaló átalakítása, eltávolítása vagy lekérdezési kifejezések bővíteni.</span><span class="sxs-lookup"><span data-stu-id="05fbe-128">This process can involve transforming, removing, or expanding of query terms.</span></span> |
|<span data-ttu-id="05fbe-129">**Index**</span><span class="sxs-lookup"><span data-stu-id="05fbe-129">**Index**</span></span> | <span data-ttu-id="05fbe-130">Egy hatékony adatszerkezet toostore használt, és rendezze az indexelt dokumentumok kinyert kereshető feltételeket.</span><span class="sxs-lookup"><span data-stu-id="05fbe-130">An efficient data structure used toostore and organize searchable terms extracted from indexed documents.</span></span> |
|<span data-ttu-id="05fbe-131">**Keresőmotor**</span><span class="sxs-lookup"><span data-stu-id="05fbe-131">**Search engine**</span></span> | <span data-ttu-id="05fbe-132">Beolvassa és dokumentumok hello hello tartalma alapján megfelelő pontszámok fordított index.</span><span class="sxs-lookup"><span data-stu-id="05fbe-132">Retrieves and scores matching documents based on hello contents of hello inverted index.</span></span> |

## <a name="anatomy-of-a-search-request"></a><span data-ttu-id="05fbe-133">Keresési kérelem szerkezete</span><span class="sxs-lookup"><span data-stu-id="05fbe-133">Anatomy of a search request</span></span>

<span data-ttu-id="05fbe-134">A keresési kérelme, mert egy teljes megadását mi vissza kell adni egy eredményhalmazban szerepel.</span><span class="sxs-lookup"><span data-stu-id="05fbe-134">A search request is a complete specification of what should be returned in a result set.</span></span> <span data-ttu-id="05fbe-135">A legegyszerűbb esetben bármilyen feltételt nem tartalmazó egy üres lekérdezést esetében.</span><span class="sxs-lookup"><span data-stu-id="05fbe-135">In simplest form, it is an empty query with no criteria of any kind.</span></span> <span data-ttu-id="05fbe-136">A modell példa paramétereket tartalmaz, több lekérdezés feltételeket, lehet, hogy a hatókörbe tartozó toocertain mezők, valószínűleg egy kifejezést, és a rendezés szabályok.</span><span class="sxs-lookup"><span data-stu-id="05fbe-136">A more realistic example includes parameters, several query terms, perhaps scoped toocertain fields, with possibly a filter expression and ordering rules.</span></span>  

<span data-ttu-id="05fbe-137">hello következő példája el tudja küldeni tooAzure keresési keresési kérelem hello segítségével [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span><span class="sxs-lookup"><span data-stu-id="05fbe-137">hello following example is a search request you might send tooAzure Search using hello [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span></span>  

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

<span data-ttu-id="05fbe-138">Ehhez a kérelemhez hello keresőmotor hello a következő:</span><span class="sxs-lookup"><span data-stu-id="05fbe-138">For this request, hello search engine does hello following:</span></span>

1. <span data-ttu-id="05fbe-139">Kiszűri egyrészt a dokumentumok, ahol hello ár legalább $60 és kisebb, mint 300 $-e.</span><span class="sxs-lookup"><span data-stu-id="05fbe-139">Filters out documents where hello price is at least $60 and less than $300.</span></span>
2. <span data-ttu-id="05fbe-140">Hello lekérdezés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="05fbe-140">Executes hello query.</span></span> <span data-ttu-id="05fbe-141">Ebben a példában hello keresési lekérdezés áll kifejezések és a feltételek: `"Spacious, air-condition* +\"Ocean view\""` (felhasználók általában nem absztrakt, de ha beleértve a hello példában kiválaszthatjuk tooexplain hogyan elemzőkkel kezelnie).</span><span class="sxs-lookup"><span data-stu-id="05fbe-141">In this example, hello search query consists of phrases and terms: `"Spacious, air-condition* +\"Ocean view\""` (users typically don't enter punctuation, but including it in hello example allows us tooexplain how analyzers handle it).</span></span> <span data-ttu-id="05fbe-142">Ehhez a lekérdezéshez hello keresőmotor megvizsgálja hello leírása, és a megadott cím mezők `searchFields` "Óceáni nézet", tartalmazó dokumentumok és továbbá hello kifejezés "ahhoz," vagy a feltételeket, amelyek hello előtaggal kezdődik. "air-condition".</span><span class="sxs-lookup"><span data-stu-id="05fbe-142">For this query, hello search engine scans hello description and title fields specified in `searchFields` for documents that contain "Ocean view", and additionally on hello term "spacious", or on terms that start with hello prefix "air-condition".</span></span> <span data-ttu-id="05fbe-143">Hello `searchMode` paraméter használt toomatch bármely kifejezés (alapértelmezett), vagy azokat, olyan esetekben, ahol a kifejezés nincs explicit módon szükséges (`+`).</span><span class="sxs-lookup"><span data-stu-id="05fbe-143">hello `searchMode` parameter is used toomatch on any term (default) or all of them, for cases where a term is not explicitly required (`+`).</span></span>
3. <span data-ttu-id="05fbe-144">Rendelések szállodák eredő hello által megadott földrajzi hely, és ezután visszaérkezik a hívó alkalmazás toohello közelségi kapcsolat tooa.</span><span class="sxs-lookup"><span data-stu-id="05fbe-144">Orders hello resulting set of hotels by proximity tooa given geography location, and then returned toohello calling application.</span></span> 

<span data-ttu-id="05fbe-145">hello Ez a cikk többsége tárgya hello feldolgozása *keresési lekérdezés*: `"Spacious, air-condition* +\"Ocean view\""`.</span><span class="sxs-lookup"><span data-stu-id="05fbe-145">hello majority of this article is about processing of hello *search query*: `"Spacious, air-condition* +\"Ocean view\""`.</span></span> <span data-ttu-id="05fbe-146">Szűrési és rendezési kívül esnek a hatókörön.</span><span class="sxs-lookup"><span data-stu-id="05fbe-146">Filtering and ordering are out of scope.</span></span> <span data-ttu-id="05fbe-147">További információkért lásd: hello [keresési API-referenciadokumentáció](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span><span class="sxs-lookup"><span data-stu-id="05fbe-147">For more information, see hello [Search API reference documentation](https://docs.microsoft.com/rest/api/searchservice/search-documents).</span></span>

<a name="stage1"></a>
## <a name="stage-1-query-parsing"></a><span data-ttu-id="05fbe-148">1. fázis: Lekérdezés elemzése</span><span class="sxs-lookup"><span data-stu-id="05fbe-148">Stage 1: Query parsing</span></span> 

<span data-ttu-id="05fbe-149">Amint azt a hello lekérdezési karakterlánc hello hello kérelem első sorának:</span><span class="sxs-lookup"><span data-stu-id="05fbe-149">As noted, hello query string is hello first line of hello request:</span></span> 

~~~~
 "search": "Spacious, air-condition* +\"Ocean view\"", 
~~~~

<span data-ttu-id="05fbe-150">hello lekérdezés elemző elválasztja az operátorok (például `*` és `+` hello példában) a keresési feltételeket, és deconstructs hello keresési lekérdezést a *segédlekérdezések* támogatott típusú:</span><span class="sxs-lookup"><span data-stu-id="05fbe-150">hello query parser separates operators (such as `*` and `+` in hello example) from search terms, and deconstructs hello search query into *subqueries* of a supported type:</span></span> 

+ <span data-ttu-id="05fbe-151">*kifejezés lekérdezés* önálló feltételek (például ahhoz)</span><span class="sxs-lookup"><span data-stu-id="05fbe-151">*term query* for standalone terms (like spacious)</span></span>
+ <span data-ttu-id="05fbe-152">*kifejezés lekérdezés* idézőjelek közé zárt feltételek (például óceáni megtekintése)</span><span class="sxs-lookup"><span data-stu-id="05fbe-152">*phrase query* for quoted terms (like ocean view)</span></span>
+ <span data-ttu-id="05fbe-153">*előtag-lekérdezés* követ egy előtag operátor feltételek `*` (például air-condition)</span><span class="sxs-lookup"><span data-stu-id="05fbe-153">*prefix query* for terms followed by a prefix operator `*` (like air-condition)</span></span>

<span data-ttu-id="05fbe-154">A támogatott lekérdezéstípusok teljes listáját lásd: [Lucene lekérdezés sytnax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)</span><span class="sxs-lookup"><span data-stu-id="05fbe-154">For a full list of supported query types see [Lucene query sytnax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)</span></span>

<span data-ttu-id="05fbe-155">Operátor segédlekérdezésben társított határozza meg, hogy hello lekérdezés "" vagy "kell lennie" ahhoz, hogy a dokumentum meg toobe tekinthető egyezés.</span><span class="sxs-lookup"><span data-stu-id="05fbe-155">Operators associated with a subquery determine whether hello query "must be" or "should be" satisfied in order for a document toobe considered a match.</span></span> <span data-ttu-id="05fbe-156">Például `+"Ocean view"` "kell" van esedékes toohello `+` operátor.</span><span class="sxs-lookup"><span data-stu-id="05fbe-156">For example, `+"Ocean view"` is "must" due toohello `+` operator.</span></span> 

<span data-ttu-id="05fbe-157">hello lekérdezéselemzőben átszervezése hello segédlekérdezések be egy *lekérdezés fa* (egyik belső struktúra hello lekérdezés képviselő) továbbítja a toohello keresőmotort.</span><span class="sxs-lookup"><span data-stu-id="05fbe-157">hello query parser restructures hello subqueries into a *query tree* (an internal structure representing hello query) it passes on toohello search engine.</span></span> <span data-ttu-id="05fbe-158">Hello első lépésként elemzése lekérdezés hello lekérdezés fa néz ki.</span><span class="sxs-lookup"><span data-stu-id="05fbe-158">In hello first stage of query parsing, hello query tree looks like this.</span></span>  

 ![Logikai érték searchmode minden lekérdezése][2]

### <a name="supported-parsers-simple-and-full-lucene"></a><span data-ttu-id="05fbe-160">Támogatott elemzők: egyszerű és a teljes Lucene</span><span class="sxs-lookup"><span data-stu-id="05fbe-160">Supported parsers: Simple and Full Lucene</span></span> 

 <span data-ttu-id="05fbe-161">Az Azure Search mutatja meg két különböző lekérdezési nyelv `simple` (alapértelmezett) és `full`.</span><span class="sxs-lookup"><span data-stu-id="05fbe-161">Azure Search exposes two different query languages, `simple` (default) and `full`.</span></span> <span data-ttu-id="05fbe-162">Hello beállítása által `queryType` paraméter, a search kérelemmel, közli hello lekérdezéselemzőben mely lekérdezési nyelv úgy dönt, hogy tudja, hogyan toointerpret hello operátorok és szintaxisát.</span><span class="sxs-lookup"><span data-stu-id="05fbe-162">By setting hello `queryType` parameter with your search request, you tell hello query parser which query language you choose so that it knows how toointerpret hello operators and syntax.</span></span> <span data-ttu-id="05fbe-163">Hello [egyszerű lekérdezési nyelv](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) intuitív és robusztus, gyakran megfelelő toointerpret felhasználói bevitelt,-ügyféloldali feldolgozás nélkül van.</span><span class="sxs-lookup"><span data-stu-id="05fbe-163">hello [Simple query langauge](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) is intuitive and robust, often suitable toointerpret user input as-is without client-side processing.</span></span> <span data-ttu-id="05fbe-164">Támogatja a lekérdezési operátorok ismerős webes keresőmotorokból.</span><span class="sxs-lookup"><span data-stu-id="05fbe-164">It supports query operators familiar from web search engines.</span></span> <span data-ttu-id="05fbe-165">Hello [teljes Lucene lekérdezési nyelv](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), amely úgy, hogy elérhetővé `queryType=full`, hello alapértelmezett egyszerű lekérdezési nyelv bővíti a további operátorok és a helyettesítő karakter, az intelligens egyeztetésű például lekérdezéstípusok, a reguláris kifejezéssel és a mező hatókörű lekérdezések támogatásának hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="05fbe-165">hello [Full Lucene query language](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), which you get by setting `queryType=full`, extends hello default Simple query language by adding support for more operators and query types like wildcard, fuzzy, regex, and field-scoped queries.</span></span> <span data-ttu-id="05fbe-166">Például egy egyszerű lekérdezés szintaxisát küldött reguláris kifejezés értelmezését egy lekérdezési karakterlánc és a kifejezés nem.</span><span class="sxs-lookup"><span data-stu-id="05fbe-166">For example, a regular expression sent in Simple query syntax would be interpreted as a query string and not an expression.</span></span> <span data-ttu-id="05fbe-167">Példa egy kérelem hello ebben a cikkben hello teljes Lucene lekérdezés nyelvét használja.</span><span class="sxs-lookup"><span data-stu-id="05fbe-167">hello example request in this article uses hello Full Lucene query language.</span></span>

### <a name="impact-of-searchmode-on-hello-parser"></a><span data-ttu-id="05fbe-168">A hello elemző searchMode hatása</span><span class="sxs-lookup"><span data-stu-id="05fbe-168">Impact of searchMode on hello parser</span></span> 

<span data-ttu-id="05fbe-169">Egy másik keresési kérelmet, amely befolyásolja az elemzés paraméter hello `searchMode` paraméter.</span><span class="sxs-lookup"><span data-stu-id="05fbe-169">Another search request parameter that affects parsing is hello `searchMode` parameter.</span></span> <span data-ttu-id="05fbe-170">Azt szabályozza, hogy hello alapértelmezett operátor logikai lekérdezések: vagy az összes bármely (alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="05fbe-170">It controls hello default operator for Boolean queries: any (default) or all.</span></span>  

<span data-ttu-id="05fbe-171">Amikor `searchMode=any`, amely hello alapértelmezett, hello helyet elválasztó között ahhoz, és a air-condition vagy (`||`), így hello minta lekérdezésszöveg egyenértékű:</span><span class="sxs-lookup"><span data-stu-id="05fbe-171">When `searchMode=any`, which is hello default, hello space delimiter between spacious and air-condition is OR (`||`), making hello sample query text equivalent to:</span></span> 

~~~~
Spacious,||air-condition*+"Ocean view" 
~~~~

<span data-ttu-id="05fbe-172">Explicit operátorok, például a `+` a `+"Ocean view"`, amelyek egyértelműen logikai lekérdezés kialakításában (hello kifejezés *kell* felel meg).</span><span class="sxs-lookup"><span data-stu-id="05fbe-172">Explicit operators, such as `+` in `+"Ocean view"`, are unambiguous in boolean query construction (hello term *must* match).</span></span> <span data-ttu-id="05fbe-173">Kevésbé nyilvánvaló van hogyan fennmaradó toointerpret hello kapcsolatos kifejezések: ahhoz, és air-condition.</span><span class="sxs-lookup"><span data-stu-id="05fbe-173">Less obvious is how toointerpret hello remaining terms: spacious and air-condition.</span></span> <span data-ttu-id="05fbe-174">Kell hello keresőmotor találatokat óceáni nézeten *és* ahhoz, *és* air-condition?</span><span class="sxs-lookup"><span data-stu-id="05fbe-174">Should hello search engine find matches on ocean view *and* spacious *and* air-condition?</span></span> <span data-ttu-id="05fbe-175">Vagy kell keresés óceáni nézet plus *egy* a fennmaradó feltételek hello?</span><span class="sxs-lookup"><span data-stu-id="05fbe-175">Or should it find ocean view plus *either one* of hello remaining terms?</span></span> 

<span data-ttu-id="05fbe-176">Alapértelmezés szerint (`searchMode=any`), hello keresőmotor azt feltételezi, hogy a hello szélesebb körű értelmezése.</span><span class="sxs-lookup"><span data-stu-id="05fbe-176">By default (`searchMode=any`), hello search engine assumes hello broader interpretation.</span></span> <span data-ttu-id="05fbe-177">Vagy mező *kell* egyeztetni, amely tükrözi "vagy" szemantikáját.</span><span class="sxs-lookup"><span data-stu-id="05fbe-177">Either field *should* be matched, reflecting "or" semantics.</span></span> <span data-ttu-id="05fbe-178">hello kezdeti lekérdezést fa mutatja korábban, a hello két "kell" művelet, hello alapértelmezett jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="05fbe-178">hello initial query tree illustrated previously, with hello two "should" operations, shows hello default.</span></span>  

<span data-ttu-id="05fbe-179">Tegyük fel, hogy most hivatott `searchMode=all`.</span><span class="sxs-lookup"><span data-stu-id="05fbe-179">Suppose that we now set `searchMode=all`.</span></span> <span data-ttu-id="05fbe-180">Ebben az esetben hello terület kerül értelmezésre "és" művelet.</span><span class="sxs-lookup"><span data-stu-id="05fbe-180">In this case, hello space is interpreted as an "and" operation.</span></span> <span data-ttu-id="05fbe-181">Hello fennmaradó feltételek mindegyikének lehet hello dokumentum tooqualify szerepel, amely.</span><span class="sxs-lookup"><span data-stu-id="05fbe-181">Each of hello remaining terms must both be present in hello document tooqualify as a match.</span></span> <span data-ttu-id="05fbe-182">hello eredményül kapott mintalekérdezés értelmezését az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="05fbe-182">hello resulting sample query would be interpreted as follows:</span></span> 

~~~~
+Spacious,+air-condition*+"Ocean view"  
~~~~

<span data-ttu-id="05fbe-183">Ehhez a lekérdezéshez módosított lekérdezés fát következőképpen nézne ki, ahol a megfelelő dokumentumokra az összes három segédlekérdezések hello metszetét:</span><span class="sxs-lookup"><span data-stu-id="05fbe-183">A modified query tree for this query would be as follows, where a matching document is hello intersection of all three subqueries:</span></span> 

 ![Az összes logikai lekérdezés searchmode][3]

> [!Note] 
> <span data-ttu-id="05fbe-185">Kiválasztása `searchMode=any` keresztül `searchMode=all` legjobb döntést jutott reprezentatív lekérdezések futtatásával.</span><span class="sxs-lookup"><span data-stu-id="05fbe-185">Choosing `searchMode=any` over `searchMode=all` is a decision best arrived at by running representative queries.</span></span> <span data-ttu-id="05fbe-186">Valószínűleg tooinclude operátorok (Ha a Keresés a dokumentum tárolja közös) felhasználók előfordulhat eredmények intuitívabb Ha `searchMode=all` arról tájékoztatja a logikai lekérdezési szerkezeteket.</span><span class="sxs-lookup"><span data-stu-id="05fbe-186">Users who are likely tooinclude operators (common when searching document stores) might find results more intuitive if `searchMode=all` informs boolean query constructs.</span></span> <span data-ttu-id="05fbe-187">További információk az hello együttműködés közötti `searchMode` és operátorok, lásd: [egyszerű lekérdezés szintaxisát](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).</span><span class="sxs-lookup"><span data-stu-id="05fbe-187">For more about hello interplay between `searchMode` and operators, see [Simple query syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).</span></span>

<a name="stage2"></a>
## <a name="stage-2-lexical-analysis"></a><span data-ttu-id="05fbe-188">2. fázis: Lexikális elemzés</span><span class="sxs-lookup"><span data-stu-id="05fbe-188">Stage 2: Lexical analysis</span></span> 

<span data-ttu-id="05fbe-189">Lexikális elemzőkkel folyamat *lekérdezések távon* és *lekérdezések kifejezés* hello lekérdezés fa felépítése után.</span><span class="sxs-lookup"><span data-stu-id="05fbe-189">Lexical analyzers process *term queries* and *phrase queries* after hello query tree is structured.</span></span> <span data-ttu-id="05fbe-190">Egy elemző szöveg megadott bemeneti adatok tooit hello hello elemző által folyamatok hello szöveg és majd toobe építeni küld vissza a tokenekre feltételek hello lekérdezés fa fogad el.</span><span class="sxs-lookup"><span data-stu-id="05fbe-190">An analyzer accepts hello text inputs given tooit by hello parser, processes hello text, and then sends back tokenized terms toobe incorporated into hello query tree.</span></span> 

<span data-ttu-id="05fbe-191">hello lexikális elemzés a leggyakoribb formája *nyelvi elemzés* amely átalakítja az alapján a szabályok adott tooa nyelvi megadott lekérdezési kifejezések:</span><span class="sxs-lookup"><span data-stu-id="05fbe-191">hello most common form of lexical analysis is *linguistic analysis* which transforms query terms based on rules specific tooa given language:</span></span> 

* <span data-ttu-id="05fbe-192">Lekérdezési kifejezés toohello legfelső szintű űrlap szó csökkentése</span><span class="sxs-lookup"><span data-stu-id="05fbe-192">Reducing a query term toohello root form of a word</span></span> 
* <span data-ttu-id="05fbe-193">Nélkülözhető szavak eltávolítása (szavak, például az "a" vagy "és" angol nyelven)</span><span class="sxs-lookup"><span data-stu-id="05fbe-193">Removing non-essential words (stopwords, such as "the" or "and" in English)</span></span> 
* <span data-ttu-id="05fbe-194">Egy összetett a word ossza összetevői</span><span class="sxs-lookup"><span data-stu-id="05fbe-194">Breaking a composite word into component parts</span></span> 
* <span data-ttu-id="05fbe-195">Alsó körül, egy nagybetű word</span><span class="sxs-lookup"><span data-stu-id="05fbe-195">Lower casing an upper case word</span></span> 

<span data-ttu-id="05fbe-196">Ezeket a műveleteket az egyes tooerase különbségei hello szöveges bevitel hello hello indexben tárolt felhasználói és hello feltételek által biztosított.</span><span class="sxs-lookup"><span data-stu-id="05fbe-196">All of these operations tend tooerase differences between hello text input provided by hello user and hello terms stored in hello index.</span></span> <span data-ttu-id="05fbe-197">Az ilyen műveletek szöveg feldolgozási túlmutató, és maga hello nyelvi alapos ismerete szükséges.</span><span class="sxs-lookup"><span data-stu-id="05fbe-197">Such operations go beyond text processing and require in-depth knowledge of hello language itself.</span></span> <span data-ttu-id="05fbe-198">tooadd nyelvi tájékoztatáshoz Azure Search réteg támogatja listája túl hosszú [nyelvi elemzőkkel](https://docs.microsoft.com/rest/api/searchservice/language-support) Lucene és a Microsoft.</span><span class="sxs-lookup"><span data-stu-id="05fbe-198">tooadd this layer of linguistic awareness, Azure Search supports a long list of [language analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support) from both Lucene and Microsoft.</span></span>

> [!Note]
> <span data-ttu-id="05fbe-199">A forgatókönyvtől függően a minimális tooelaborate elemzés követelmények között lehet.</span><span class="sxs-lookup"><span data-stu-id="05fbe-199">Analysis requirements can range from minimal tooelaborate depending on your scenario.</span></span> <span data-ttu-id="05fbe-200">Az előre megadott hello elemzőkkel egyikének kiválasztásával hello, vagy hozzon létre egy saját lexikális elemzés összetettsége szabályozhatja [egyéni analyzer](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="05fbe-200">You can control complexity of lexical analysis by hello selecting one of hello predefined analyzers or by creating your own [custom analyzer](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search).</span></span> <span data-ttu-id="05fbe-201">Elemzőkkel hatókörön belüli toosearchable és megadott mező definíciójának részeként.</span><span class="sxs-lookup"><span data-stu-id="05fbe-201">Analyzers are scoped toosearchable fields and are specified as part of a field definition.</span></span> <span data-ttu-id="05fbe-202">Ez lehetővé teszi toovary lexikális elemzési mező alapon.</span><span class="sxs-lookup"><span data-stu-id="05fbe-202">This allows you toovary lexical analysis on a per-field basis.</span></span> <span data-ttu-id="05fbe-203">Nincs megadva, hello *szabványos* Lucene analyzer szolgál.</span><span class="sxs-lookup"><span data-stu-id="05fbe-203">Unspecified, hello *standard* Lucene analyzer is used.</span></span>

<span data-ttu-id="05fbe-204">A példánkban előzetes tooanalysis hello kezdeti lekérdezést fa rendelkezik hello kifejezés "Spacious," nagybetűs "s", és vesszővel válassza el, amely lekérdezéselemzőben hello értelmezi hello lekérdezési kifejezésre (vesszővel nem tekinthető a lekérdezési nyelv operátor) részeként.</span><span class="sxs-lookup"><span data-stu-id="05fbe-204">In our example, prior tooanalysis, hello initial query tree has hello term "Spacious," with an uppercase "S" and a comma that hello query parser interprets as a part of hello query term (a comma is not considered a query language operator).</span></span>  

<span data-ttu-id="05fbe-205">Hello alapértelmezett analyzer hello kifejezés dolgozza fel, emellett kisbetűs "óceáni nézet" és "ahhoz,", és hello vesszővel karakter eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="05fbe-205">When hello default analyzer processes hello term, it will lowercase "ocean view" and "spacious", and remove hello comma character.</span></span> <span data-ttu-id="05fbe-206">hello módosított lekérdezés fa a következőképpen fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="05fbe-206">hello modified query tree will look as follows:</span></span> 

 ![Logikai lekérdezés elemzett adatokkal][4]

### <a name="testing-analyzer-behaviors"></a><span data-ttu-id="05fbe-208">Tesztelési analyzer viselkedések</span><span class="sxs-lookup"><span data-stu-id="05fbe-208">Testing analyzer behaviors</span></span> 

<span data-ttu-id="05fbe-209">egy elemző hello viselkedését hello is meg kell vizsgálni [elemzése API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer).</span><span class="sxs-lookup"><span data-stu-id="05fbe-209">hello behavior of an analyzer can be tested using hello [Analyze API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer).</span></span> <span data-ttu-id="05fbe-210">Adja meg a hello szöveg tooanalyze toosee milyen analyzer adott kifejezések hoz létre.</span><span class="sxs-lookup"><span data-stu-id="05fbe-210">Provide hello text you want tooanalyze toosee what terms given analyzer will generate.</span></span> <span data-ttu-id="05fbe-211">Például hogyan kellene feldolgozni hello szabványos analyzer toosee hello "air-condition" szöveget, kiadhatja hello kérelem a következő:</span><span class="sxs-lookup"><span data-stu-id="05fbe-211">For example, toosee how hello standard analyzer would process hello text "air-condition", you can issue hello following request:</span></span>

~~~~
{ 
    "text": "air-condition",
    "analyzer": "standard"
}
~~~~

<span data-ttu-id="05fbe-212">hello szabványos analyzer hello bemeneti szöveg bontja a következő két jogkivonatok ellátása megjegyzésekkel őket a tulajdonságai, például a kezdő és záró eltolások (találati konzolban használt), valamint (használt kifejezés a megfelelő) pozíciójuk hello:</span><span class="sxs-lookup"><span data-stu-id="05fbe-212">hello standard analyzer breaks hello input text into hello following two tokens, annotating them with attributes like start and end offsets (used for hit highlighting) as well as their position (used for phrase matching):</span></span>

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

### <a name="exceptions-toolexical-analysis"></a><span data-ttu-id="05fbe-213">Kivételek toolexical elemzés</span><span class="sxs-lookup"><span data-stu-id="05fbe-213">Exceptions toolexical analysis</span></span> 

<span data-ttu-id="05fbe-214">Lexikális elemző csak a teljes feltételek – kifejezés lekérdezés vagy egy kifejezés lekérdezés igénylő tooquery típusok vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="05fbe-214">Lexical analysis applies only tooquery types that require complete terms – either a term query or a phrase query.</span></span> <span data-ttu-id="05fbe-215">Hiányos adatokkal – előtag lekérdezés, a helyettesítő karaktereknek, a reguláris kifejezéssel lekérdezés – vagy a tooa intelligens lekérdezés tooquery típusok nem vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="05fbe-215">It doesn’t apply tooquery types with incomplete terms – prefix query, wildcard query, regex query – or tooa fuzzy query.</span></span> <span data-ttu-id="05fbe-216">Azok lekérdezése típusok, beleértve a hello előtag lekérdezés kifejezés *air-condition\**  a fenti példában kerülnek közvetlenül toohello lekérdezés konzolfáján hello elemzési fázis kihagyásával.</span><span class="sxs-lookup"><span data-stu-id="05fbe-216">Those query types, including hello prefix query with term *air-condition\** in our example, are added directly toohello query tree, bypassing hello analysis stage.</span></span> <span data-ttu-id="05fbe-217">hello csak az adott típusú lekérdezési kifejezések végzett átalakításában van lowercasing.</span><span class="sxs-lookup"><span data-stu-id="05fbe-217">hello only transformation performed on query terms of those types is lowercasing.</span></span>

<a name="stage3"></a>
## <a name="stage-3-document-retrieval"></a><span data-ttu-id="05fbe-218">3. fázis: A dokumentum beolvasása</span><span class="sxs-lookup"><span data-stu-id="05fbe-218">Stage 3: Document retrieval</span></span> 

<span data-ttu-id="05fbe-219">A dokumentum beolvasása toofinding dokumentumok hello index folyamatmegfeleltetési feltételek hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="05fbe-219">Document retrieval refers toofinding documents with matching terms in hello index.</span></span> <span data-ttu-id="05fbe-220">Ez a szakasz egy példán keresztül legjobb értendő.</span><span class="sxs-lookup"><span data-stu-id="05fbe-220">This stage is understood best through an example.</span></span> <span data-ttu-id="05fbe-221">Kezdjük egy szállodák indexszel rendelkező hello egyszerű séma a következő:</span><span class="sxs-lookup"><span data-stu-id="05fbe-221">Let's start with a hotels index having hello following simple schema:</span></span> 

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

<span data-ttu-id="05fbe-222">További azt feltételezik, hogy ezt az indexet tartalmaz a következő négy dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="05fbe-222">Further assume that this index contains hello following four documents:</span></span> 

~~~~
{ 
    "value": [
        {         
            "id": "1",         
            "title": "Hotel Atman",         
            "description": "Spacious rooms, ocean view, walking distance toohello beach."   
        },       
        {         
            "id": "2",         
            "title": "Beach Resort",        
            "description": "Located on hello north shore of hello island of Kauaʻi. Ocean view."     
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

<span data-ttu-id="05fbe-223">**Hogyan feltételek indexelt**</span><span class="sxs-lookup"><span data-stu-id="05fbe-223">**How terms are indexed**</span></span>

<span data-ttu-id="05fbe-224">toounderstand lekérését, ennek segítségével tooknow néhány alapvető indexelése.</span><span class="sxs-lookup"><span data-stu-id="05fbe-224">toounderstand retrieval, it helps tooknow a few basics about indexing.</span></span> <span data-ttu-id="05fbe-225">hello tárolási mértékegysége fordított index, egy az összes kereshető mezőt.</span><span class="sxs-lookup"><span data-stu-id="05fbe-225">hello unit of storage is an inverted index, one for each searchable field.</span></span> <span data-ttu-id="05fbe-226">Fordított index belül az összes dokumentumot az összes kifejezést a rendezett listáját.</span><span class="sxs-lookup"><span data-stu-id="05fbe-226">Within an inverted index is a sorted list of all terms from all documents.</span></span> <span data-ttu-id="05fbe-227">Minden kifejezéshez maps, amelyben előfordul, a nyilvánvaló hello az alábbi példa a dokumentumok toohello listáját.</span><span class="sxs-lookup"><span data-stu-id="05fbe-227">Each term maps toohello list of documents in which it occurs, as evident in hello example below.</span></span>

<span data-ttu-id="05fbe-228">tooproduce hello feltételek fordított indexben hello keresőmotor hajt végre lexikális elemzés hello tartalomban dokumentumok, hasonló toowhat történik a lekérdezés feldolgozása során.</span><span class="sxs-lookup"><span data-stu-id="05fbe-228">tooproduce hello terms in an inverted index, hello search engine performs lexical analysis over hello content of documents, similar toowhat happens during query processing.</span></span> <span data-ttu-id="05fbe-229">Szöveg bemenetek átadott tooan analyzer alsó cased tisztító absztrakt, és így tovább hello analyzer konfigurációjától függően.</span><span class="sxs-lookup"><span data-stu-id="05fbe-229">Text inputs are passed tooan analyzer, lower-cased, stripped of punctuation, and so forth, depending on hello analyzer configuration.</span></span> <span data-ttu-id="05fbe-230">Általános, de nem szükséges, az toouse hello keresési és indexelő műveletek, így a lekérdezési kifejezések keresse meg a további feltételek belül hello index hasonlóan azonos elemzőkkel.</span><span class="sxs-lookup"><span data-stu-id="05fbe-230">It's common, but not required, toouse hello same analyzers for search and indexing operations so that query terms look more like terms inside hello index.</span></span>

> [!Note]
> <span data-ttu-id="05fbe-231">Az Azure Search lehetővé teszi a különböző elemzőkkel indexeléshez adja meg, és keressen további keresztül `indexAnalyzer` és `searchAnalyzer` paraméterek mezőben.</span><span class="sxs-lookup"><span data-stu-id="05fbe-231">Azure Search lets you specify different analyzers for indexing and search via additional `indexAnalyzer` and `searchAnalyzer` field parameters.</span></span> <span data-ttu-id="05fbe-232">Ha nincs megadva, a hello beállítása analyzer hello `analyzer` a tulajdonságot használja az indexelés és a keresést.</span><span class="sxs-lookup"><span data-stu-id="05fbe-232">If unspecified, hello analyzer set with hello `analyzer` property is used for both indexing and searching.</span></span>  

<span data-ttu-id="05fbe-233">**Példa dokumentumok fordított indexe**</span><span class="sxs-lookup"><span data-stu-id="05fbe-233">**Inverted index for example documents**</span></span>

<span data-ttu-id="05fbe-234">Adatszolgáltató tooour példa, hello **cím** mezőben fordított hello index néz ki:</span><span class="sxs-lookup"><span data-stu-id="05fbe-234">Returning tooour example, for hello **title** field, hello inverted index looks like this:</span></span>

| <span data-ttu-id="05fbe-235">Időtartam</span><span class="sxs-lookup"><span data-stu-id="05fbe-235">Term</span></span> | <span data-ttu-id="05fbe-236">Dokumentumok listájához</span><span class="sxs-lookup"><span data-stu-id="05fbe-236">Document list</span></span> |
|------|---------------|
| <span data-ttu-id="05fbe-237">atman</span><span class="sxs-lookup"><span data-stu-id="05fbe-237">atman</span></span> | <span data-ttu-id="05fbe-238">1</span><span class="sxs-lookup"><span data-stu-id="05fbe-238">1</span></span> |
| <span data-ttu-id="05fbe-239">kínál</span><span class="sxs-lookup"><span data-stu-id="05fbe-239">beach</span></span> | <span data-ttu-id="05fbe-240">2</span><span class="sxs-lookup"><span data-stu-id="05fbe-240">2</span></span> |
| <span data-ttu-id="05fbe-241">Szálloda</span><span class="sxs-lookup"><span data-stu-id="05fbe-241">hotel</span></span> | <span data-ttu-id="05fbe-242">1, 3</span><span class="sxs-lookup"><span data-stu-id="05fbe-242">1, 3</span></span> |
| <span data-ttu-id="05fbe-243">óceáni</span><span class="sxs-lookup"><span data-stu-id="05fbe-243">ocean</span></span> | <span data-ttu-id="05fbe-244">4</span><span class="sxs-lookup"><span data-stu-id="05fbe-244">4</span></span>  |
| <span data-ttu-id="05fbe-245">PlayA</span><span class="sxs-lookup"><span data-stu-id="05fbe-245">playa</span></span> | <span data-ttu-id="05fbe-246">3</span><span class="sxs-lookup"><span data-stu-id="05fbe-246">3</span></span> |
| <span data-ttu-id="05fbe-247">végső esetben</span><span class="sxs-lookup"><span data-stu-id="05fbe-247">resort</span></span> | <span data-ttu-id="05fbe-248">3</span><span class="sxs-lookup"><span data-stu-id="05fbe-248">3</span></span> |
| <span data-ttu-id="05fbe-249">Retreat</span><span class="sxs-lookup"><span data-stu-id="05fbe-249">retreat</span></span> | <span data-ttu-id="05fbe-250">4</span><span class="sxs-lookup"><span data-stu-id="05fbe-250">4</span></span> |

<span data-ttu-id="05fbe-251">A hello cím mezőben csak *Szálloda* mutatja két dokumentumot: 1, 3.</span><span class="sxs-lookup"><span data-stu-id="05fbe-251">In hello title field, only *hotel* shows up in two documents: 1, 3.</span></span>

<span data-ttu-id="05fbe-252">A hello **leírás** mezőben hello index a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="05fbe-252">For hello **description** field, hello index is as follows:</span></span>

| <span data-ttu-id="05fbe-253">Időtartam</span><span class="sxs-lookup"><span data-stu-id="05fbe-253">Term</span></span> | <span data-ttu-id="05fbe-254">Dokumentumok listájához</span><span class="sxs-lookup"><span data-stu-id="05fbe-254">Document list</span></span> |
|------|---------------|
| <span data-ttu-id="05fbe-255">vezeték nélkül</span><span class="sxs-lookup"><span data-stu-id="05fbe-255">air</span></span> | <span data-ttu-id="05fbe-256">3</span><span class="sxs-lookup"><span data-stu-id="05fbe-256">3</span></span>
| <span data-ttu-id="05fbe-257">és</span><span class="sxs-lookup"><span data-stu-id="05fbe-257">and</span></span> | <span data-ttu-id="05fbe-258">4</span><span class="sxs-lookup"><span data-stu-id="05fbe-258">4</span></span>
| <span data-ttu-id="05fbe-259">kínál</span><span class="sxs-lookup"><span data-stu-id="05fbe-259">beach</span></span> | <span data-ttu-id="05fbe-260">1</span><span class="sxs-lookup"><span data-stu-id="05fbe-260">1</span></span>
| <span data-ttu-id="05fbe-261">annak</span><span class="sxs-lookup"><span data-stu-id="05fbe-261">conditioned</span></span> | <span data-ttu-id="05fbe-262">3</span><span class="sxs-lookup"><span data-stu-id="05fbe-262">3</span></span>
| <span data-ttu-id="05fbe-263">Feladatkezelő</span><span class="sxs-lookup"><span data-stu-id="05fbe-263">comfortable</span></span> | <span data-ttu-id="05fbe-264">3</span><span class="sxs-lookup"><span data-stu-id="05fbe-264">3</span></span>
| <span data-ttu-id="05fbe-265">távolság</span><span class="sxs-lookup"><span data-stu-id="05fbe-265">distance</span></span> | <span data-ttu-id="05fbe-266">1</span><span class="sxs-lookup"><span data-stu-id="05fbe-266">1</span></span>
| <span data-ttu-id="05fbe-267">sziget</span><span class="sxs-lookup"><span data-stu-id="05fbe-267">island</span></span> | <span data-ttu-id="05fbe-268">2</span><span class="sxs-lookup"><span data-stu-id="05fbe-268">2</span></span>
| <span data-ttu-id="05fbe-269">kauaʻi</span><span class="sxs-lookup"><span data-stu-id="05fbe-269">kauaʻi</span></span> | <span data-ttu-id="05fbe-270">2</span><span class="sxs-lookup"><span data-stu-id="05fbe-270">2</span></span>
| <span data-ttu-id="05fbe-271">található</span><span class="sxs-lookup"><span data-stu-id="05fbe-271">located</span></span> | <span data-ttu-id="05fbe-272">2</span><span class="sxs-lookup"><span data-stu-id="05fbe-272">2</span></span>
| <span data-ttu-id="05fbe-273">északi régiója</span><span class="sxs-lookup"><span data-stu-id="05fbe-273">north</span></span> | <span data-ttu-id="05fbe-274">2</span><span class="sxs-lookup"><span data-stu-id="05fbe-274">2</span></span>
| <span data-ttu-id="05fbe-275">óceáni</span><span class="sxs-lookup"><span data-stu-id="05fbe-275">ocean</span></span> | <span data-ttu-id="05fbe-276">1, 2, 3</span><span class="sxs-lookup"><span data-stu-id="05fbe-276">1, 2, 3</span></span>
| <span data-ttu-id="05fbe-277">/</span><span class="sxs-lookup"><span data-stu-id="05fbe-277">of</span></span> | <span data-ttu-id="05fbe-278">2</span><span class="sxs-lookup"><span data-stu-id="05fbe-278">2</span></span>
| <span data-ttu-id="05fbe-279">a</span><span class="sxs-lookup"><span data-stu-id="05fbe-279">on</span></span> |<span data-ttu-id="05fbe-280">2</span><span class="sxs-lookup"><span data-stu-id="05fbe-280">2</span></span>
| <span data-ttu-id="05fbe-281">Csendes</span><span class="sxs-lookup"><span data-stu-id="05fbe-281">quiet</span></span> | <span data-ttu-id="05fbe-282">4</span><span class="sxs-lookup"><span data-stu-id="05fbe-282">4</span></span>
| <span data-ttu-id="05fbe-283">helyiségekben</span><span class="sxs-lookup"><span data-stu-id="05fbe-283">rooms</span></span>  | <span data-ttu-id="05fbe-284">1, 3</span><span class="sxs-lookup"><span data-stu-id="05fbe-284">1, 3</span></span>
| <span data-ttu-id="05fbe-285">secluded</span><span class="sxs-lookup"><span data-stu-id="05fbe-285">secluded</span></span> | <span data-ttu-id="05fbe-286">4</span><span class="sxs-lookup"><span data-stu-id="05fbe-286">4</span></span>
| <span data-ttu-id="05fbe-287">parti</span><span class="sxs-lookup"><span data-stu-id="05fbe-287">shore</span></span> | <span data-ttu-id="05fbe-288">2</span><span class="sxs-lookup"><span data-stu-id="05fbe-288">2</span></span>
| <span data-ttu-id="05fbe-289">Ahhoz</span><span class="sxs-lookup"><span data-stu-id="05fbe-289">spacious</span></span> | <span data-ttu-id="05fbe-290">1</span><span class="sxs-lookup"><span data-stu-id="05fbe-290">1</span></span>
| <span data-ttu-id="05fbe-291">helló</span><span class="sxs-lookup"><span data-stu-id="05fbe-291">hello</span></span> | <span data-ttu-id="05fbe-292">1, 2</span><span class="sxs-lookup"><span data-stu-id="05fbe-292">1, 2</span></span>
| <span data-ttu-id="05fbe-293">túl</span><span class="sxs-lookup"><span data-stu-id="05fbe-293">too</span></span>| <span data-ttu-id="05fbe-294">1</span><span class="sxs-lookup"><span data-stu-id="05fbe-294">1</span></span>
| <span data-ttu-id="05fbe-295">megtekintés</span><span class="sxs-lookup"><span data-stu-id="05fbe-295">view</span></span> | <span data-ttu-id="05fbe-296">1, 2, 3</span><span class="sxs-lookup"><span data-stu-id="05fbe-296">1, 2, 3</span></span>
| <span data-ttu-id="05fbe-297">érdekében</span><span class="sxs-lookup"><span data-stu-id="05fbe-297">walking</span></span> | <span data-ttu-id="05fbe-298">1</span><span class="sxs-lookup"><span data-stu-id="05fbe-298">1</span></span>
| <span data-ttu-id="05fbe-299">a</span><span class="sxs-lookup"><span data-stu-id="05fbe-299">with</span></span> | <span data-ttu-id="05fbe-300">3</span><span class="sxs-lookup"><span data-stu-id="05fbe-300">3</span></span>


<span data-ttu-id="05fbe-301">**Egyező lekérdezési kifejezések indexelt feltételek ellen**</span><span class="sxs-lookup"><span data-stu-id="05fbe-301">**Matching query terms against indexed terms**</span></span>

<span data-ttu-id="05fbe-302">Fordított hello indexek fenti, most toohello mintalekérdezés vissza, és hogyan egyező dokumentumok lásd a példalekérdezés talált.</span><span class="sxs-lookup"><span data-stu-id="05fbe-302">Given hello inverted indices above, let’s return toohello sample query and see how matching documents are found for our example query.</span></span> <span data-ttu-id="05fbe-303">Visszahívása hello utolsó lekérdezési tree néz ki:</span><span class="sxs-lookup"><span data-stu-id="05fbe-303">Recall that hello final query tree looks like this:</span></span> 

 ![Logikai lekérdezés elemzett adatokkal][4]

<span data-ttu-id="05fbe-305">Lekérdezés-végrehajtás során egyes lekérdezések végrehajtása elleni hello kereshető mezők egymástól függetlenül.</span><span class="sxs-lookup"><span data-stu-id="05fbe-305">During query execution, individual queries are executed against hello searchable fields independently.</span></span> 

+ <span data-ttu-id="05fbe-306">hello TermQuery "ahhoz,", megegyezik a dokumentum 1 (szálloda Atman).</span><span class="sxs-lookup"><span data-stu-id="05fbe-306">hello TermQuery, "spacious", matches document 1 (Hotel Atman).</span></span> 

+ <span data-ttu-id="05fbe-307">hello PrefixQuery, "air-condition *", nem felel meg a dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="05fbe-307">hello PrefixQuery, "air-condition*", doesn't match any documents.</span></span> 

  <span data-ttu-id="05fbe-308">Ez az, hogy a fejlesztők néha confuses.</span><span class="sxs-lookup"><span data-stu-id="05fbe-308">This is a behavior that sometimes confuses developers.</span></span> <span data-ttu-id="05fbe-309">Bár hello kifejezés klimatizált hello dokumentum szerepel, az oszlik két feltételek hello alapértelmezett analyzer által.</span><span class="sxs-lookup"><span data-stu-id="05fbe-309">Although hello term air-conditioned exists in hello document, it is split into two terms by hello default analyzer.</span></span> <span data-ttu-id="05fbe-310">Visszahívása előtag a lekérdezések részleges feltételeket tartalmaz, amelyek nem került sor.</span><span class="sxs-lookup"><span data-stu-id="05fbe-310">Recall that prefix queries, which contain partial terms, are not analyzed.</span></span> <span data-ttu-id="05fbe-311">Ezért a feltételek "air-condition" vannak kereshető hello előtaggal rendelkező fordított index, és nem található.</span><span class="sxs-lookup"><span data-stu-id="05fbe-311">Therefore terms with prefix "air-condition" are looked up in hello inverted index and not found.</span></span>

+ <span data-ttu-id="05fbe-312">hello PhraseQuery a "óceáni nézet", hello feltételek "óceáni" és "nézet" keres, és hello közelében hello eredeti dokumentumban feltételeket ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="05fbe-312">hello PhraseQuery, "ocean view", looks up hello terms "ocean" and "view" and checks hello proximity of terms in hello original document.</span></span> <span data-ttu-id="05fbe-313">Dokumentumok, 1, 2, 3 felel meg a lekérdezés hello Leírás mezőben.</span><span class="sxs-lookup"><span data-stu-id="05fbe-313">Documents 1, 2 and 3 match this query in hello description field.</span></span> <span data-ttu-id="05fbe-314">Figyelje meg a dokumentum 4 hello kifejezés óceáni hello címben rendelkezik, de nem tekinthető egyezés, azt keres egyes szavak helyett hello "óceáni nézet" kifejezés helyett szerepel.</span><span class="sxs-lookup"><span data-stu-id="05fbe-314">Notice document 4 has hello term ocean in hello title but isn’t considered a match, as we're looking for hello "ocean view" phrase rather than individual words.</span></span> 

> [!Note]
> <span data-ttu-id="05fbe-315">Keresési lekérdezés különállóan végre elleni hello Azure Search index, kivéve, ha a beállított hello hello mezők korlátozhatja az összes kereshető mezőt `searchFields` paraméter, hello keresési kérelem (példa) ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="05fbe-315">A search query is executed independently against all searchable fields in hello Azure Search index unless you limit hello fields set with hello `searchFields` parameter, as illustrated in hello example search request.</span></span> <span data-ttu-id="05fbe-316">Minden kiválasztott hello mezők megfelelő dokumentumok adja vissza.</span><span class="sxs-lookup"><span data-stu-id="05fbe-316">Documents that match in any of hello selected fields are returned.</span></span> 

<span data-ttu-id="05fbe-317">A szóban forgó hello lekérdezés teljes hello megfelelő hello dokumentumok, 1, 2, 3.</span><span class="sxs-lookup"><span data-stu-id="05fbe-317">On hello whole, for hello query in question, hello documents that match are 1, 2, 3.</span></span> 

## <a name="stage-4-scoring"></a><span data-ttu-id="05fbe-318">4. szakasz: pontozási</span><span class="sxs-lookup"><span data-stu-id="05fbe-318">Stage 4: Scoring</span></span>  

<span data-ttu-id="05fbe-319">Minden egyes dokumentum egy keresési eredménykészletben hozzá van rendelve egy relevanciájának pontszám.</span><span class="sxs-lookup"><span data-stu-id="05fbe-319">Every document in a search result set is assigned a relevance score.</span></span> <span data-ttu-id="05fbe-320">hello hello relevanciájának pontszám funkciója toorank magasabb ezeket a dokumentumokat, hogy a legjobb válaszoljon a felhasználói kérdés kifejezett hello keresési lekérdezés alapján.</span><span class="sxs-lookup"><span data-stu-id="05fbe-320">hello function of hello relevance score is toorank higher those documents that best answer a user question as expressed by hello search query.</span></span> <span data-ttu-id="05fbe-321">hello pontszám egyező kifejezések statisztikai tulajdonságok alapján számítja ki.</span><span class="sxs-lookup"><span data-stu-id="05fbe-321">hello score is computed based on statistical properties of terms that matched.</span></span> <span data-ttu-id="05fbe-322">Hello részében képlet pontozási hello van [TF/IDF (kifejezés gyakoriság-inverz dokumentum gyakoriság)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).</span><span class="sxs-lookup"><span data-stu-id="05fbe-322">At hello core of hello scoring formula is [TF/IDF (term frequency-inverse document frequency)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).</span></span> <span data-ttu-id="05fbe-323">Ritka és gyakori feltételeket tartalmazó lekérdezésekben TF/IDF hello ritka kifejezés, amely elősegíti.</span><span class="sxs-lookup"><span data-stu-id="05fbe-323">In queries containing rare and common terms, TF/IDF promotes results containing hello rare term.</span></span> <span data-ttu-id="05fbe-324">Például egy elméleti index összes Wikipedia cikkekkel, a dokumentumok egyező hello lekérdezés *hello elnök*, a megfelelő dokumentumok *elnök* relevánsnak több mint a megfelelő dokumentumok *a*.</span><span class="sxs-lookup"><span data-stu-id="05fbe-324">For example, in a hypothetical index with all Wikipedia articles, from documents that matched hello query *hello president*, documents matching on *president* are considered more relevant than documents matching on *the*.</span></span>


### <a name="scoring-example"></a><span data-ttu-id="05fbe-325">A pontozási – példa</span><span class="sxs-lookup"><span data-stu-id="05fbe-325">Scoring example</span></span>

<span data-ttu-id="05fbe-326">A példa lekérdezés egyező hello három dokumentumok visszahívása:</span><span class="sxs-lookup"><span data-stu-id="05fbe-326">Recall hello three documents that matched our example query:</span></span>
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
      "description": "Spacious rooms, ocean view, walking distance toohello beach."
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
      "description": "Located on a cliff on hello north shore of hello island of Kauai. Ocean view."
    }
  ]
}
~~~~

<span data-ttu-id="05fbe-327">Legjobb dokumentum 1 egyező hello lekérdezést, mert mindkét hello kifejezés *ahhoz,* és hello szükséges kódot *óceáni nézet* hello leírási mezője fordul elő.</span><span class="sxs-lookup"><span data-stu-id="05fbe-327">Document 1 matched hello query best because both hello term *spacious* and hello required phrase *ocean view* occur in hello description field.</span></span> <span data-ttu-id="05fbe-328">hello következő két dokumentumok megfelelő csak hello kifejezés *óceáni nézet*.</span><span class="sxs-lookup"><span data-stu-id="05fbe-328">hello next two documents match only hello phrase *ocean view*.</span></span> <span data-ttu-id="05fbe-329">Akkor lehet, hogy kell meglepetést adott hello relevanciájának pontszám dokumentum 2 és 3 különbözik, annak ellenére, hogy azok egyező hello hello lekérdezést azonos módon.</span><span class="sxs-lookup"><span data-stu-id="05fbe-329">It might be surprising that hello relevance score for document 2 and 3 is different even though they matched hello query in hello same way.</span></span> <span data-ttu-id="05fbe-330">Ennek az oka képlet pontozási hello csak TF/IDF-nál több részből áll.</span><span class="sxs-lookup"><span data-stu-id="05fbe-330">It's because hello scoring formula has more components than just TF/IDF.</span></span> <span data-ttu-id="05fbe-331">Ebben az esetben dokumentum 3 van hozzárendelve valamivel nagyobb pontszám, mert annak leírását rövidebb.</span><span class="sxs-lookup"><span data-stu-id="05fbe-331">In this case, document 3 was assigned a slightly higher score because its description is shorter.</span></span> <span data-ttu-id="05fbe-332">További tudnivalók [Lucene tartozó gyakorlati pontozási képlet](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) hogyan mező maximális hossza és más tényezők befolyásolhatják toounderstand hello relevanciájának pontszámot.</span><span class="sxs-lookup"><span data-stu-id="05fbe-332">Learn about [Lucene's Practical Scoring Formula](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) toounderstand how field length and other factors can influence hello relevance score.</span></span>

<span data-ttu-id="05fbe-333">Néhány lekérdezés (helyettesítő, előtag, regex) típusú mindig hozzájárul az állandó pontszám toohello összesített pontszám dokumentum.</span><span class="sxs-lookup"><span data-stu-id="05fbe-333">Some query types (wildcard, prefix, regex) always contribute a constant score toohello overall document score.</span></span> <span data-ttu-id="05fbe-334">Ez lehetővé teszi, hogy a lekérdezés bővítése toobe hello eredmények között, de az nem befolyásolja a hello rangsorolási szereplő keresztül találat.</span><span class="sxs-lookup"><span data-stu-id="05fbe-334">This allows matches found through query expansion toobe included in hello results, but without affecting hello ranking.</span></span> 

<span data-ttu-id="05fbe-335">Egy példa azt mutatja be, ezért ez számít.</span><span class="sxs-lookup"><span data-stu-id="05fbe-335">An example illustrates why this matters.</span></span> <span data-ttu-id="05fbe-336">Helyettesítő karakteres kereséssel előtag keresések, beleértve nincsenek definíció nem egyértelmű, mert hello bemeneti érték egy részleges karakterlánc lehetséges megegyezik a különböző feltételek nagyon nagy számú ("bemutató *", a bemeneti-vel találat, a "bemutatók", "tourettes" fontolja meg, és " tourmaline").</span><span class="sxs-lookup"><span data-stu-id="05fbe-336">Wildcard searches, including prefix searches, are ambiguous by definition because hello input is a partial string with potential matches on a very large number of disparate terms (consider an input of "tour*", with matches found on “tours”, “tourettes”, and “tourmaline”).</span></span> <span data-ttu-id="05fbe-337">Hello jellegéből ezekkel az eredményekkel, nincs mód tooreasonably következtethető ki, mely feltételek értékesebb, mint a többire.</span><span class="sxs-lookup"><span data-stu-id="05fbe-337">Given hello nature of these results, there is no way tooreasonably infer which terms are more valuable than others.</span></span> <span data-ttu-id="05fbe-338">Ezért azt figyelmen kívül hagyása kifejezés gyakoriságot amikor pontozási eredményei típusok helyettesítő, előtag és regex lekérdezésekben.</span><span class="sxs-lookup"><span data-stu-id="05fbe-338">For this reason, we ignore term frequencies when scoring results in queries of types wildcard, prefix and regex.</span></span> <span data-ttu-id="05fbe-339">Egy konstans beépített hello részleges bemeneti eredmények a többrészes keresési kérelmet, amely részleges és teljes feltételeket tartalmaz, a pontszám tooavoid eltérés felé potenciálisan váratlan megegyezik.</span><span class="sxs-lookup"><span data-stu-id="05fbe-339">In a multi-part search request that includes partial and complete terms, results from hello partial input are incorporated with a constant score tooavoid bias towards potentially unexpected matches.</span></span>

### <a name="score-tuning"></a><span data-ttu-id="05fbe-340">Pontszám hangolása</span><span class="sxs-lookup"><span data-stu-id="05fbe-340">Score tuning</span></span>

<span data-ttu-id="05fbe-341">Két módon tootune relevanciájának pontszámok az Azure Search:</span><span class="sxs-lookup"><span data-stu-id="05fbe-341">There are two ways tootune relevance scores in Azure Search:</span></span>

1. <span data-ttu-id="05fbe-342">**A pontozási profil** lépteti elő a dokumentumok az eredmények alapján a szabályok szerint rangsorolunk hello listája.</span><span class="sxs-lookup"><span data-stu-id="05fbe-342">**Scoring profiles** promote documents in hello ranked list of results based on a set of rules.</span></span> <span data-ttu-id="05fbe-343">A jelen példában hello cím mezőben több megfelelő hello Leírás mezőben egyező dokumentumoknál újabbak egyező dokumentumok volt javasolt.</span><span class="sxs-lookup"><span data-stu-id="05fbe-343">In our example, we could consider documents that matched in hello title field more relevant than documents that matched in hello description field.</span></span> <span data-ttu-id="05fbe-344">Emellett az indexnek minden Szálloda ár mező volna, ha azt sikerült előléptetni alsó ár dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="05fbe-344">Additionally, if our index had a price field for each hotel, we could promote documents with lower price.</span></span> <span data-ttu-id="05fbe-345">Hogyan túl további[adja hozzá a pontozási profil tooa search-index.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)</span><span class="sxs-lookup"><span data-stu-id="05fbe-345">Learn more how too[add Scoring Profiles tooa search index.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)</span></span>
2. <span data-ttu-id="05fbe-346">**Távon kiemelési** (csak a hello teljes Lucene lekérdezés szintaxisa érhető el) biztosít a kiemelési operátor `^` , amely lehet alkalmazott hello lekérdezés fa tooany részét.</span><span class="sxs-lookup"><span data-stu-id="05fbe-346">**Term boosting** (available only in hello Full Lucene query syntax) provides a boosting operator `^` that can be applied tooany part of hello query tree.</span></span> <span data-ttu-id="05fbe-347">A jelen példában helyett hello előtag alapján keres *air-condition*\*, vagy hello pontos időszakra keressen egy *air-condition* vagy hello előtag, de a pontos hello megfelelő dokumentumok kifejezés magasabb rangsora program toohello kifejezés lekérdezés alkalmazásával: *vezeték nélkül-feltétel ^ 2. Air-condition**.</span><span class="sxs-lookup"><span data-stu-id="05fbe-347">In our example, instead of searching on hello prefix *air-condition*\*, one could search for either hello exact term *air-condition* or hello prefix, but documents that match on hello exact term are ranked higher by applying boost toohello term query: *air-condition^2||air-condition**.</span></span> <span data-ttu-id="05fbe-348">További információ [távon kiemelési](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).</span><span class="sxs-lookup"><span data-stu-id="05fbe-348">Learn more about [term boosting](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).</span></span>


### <a name="scoring-in-a-distributed-index"></a><span data-ttu-id="05fbe-349">Egy elosztott index pontozási</span><span class="sxs-lookup"><span data-stu-id="05fbe-349">Scoring in a distributed index</span></span>

<span data-ttu-id="05fbe-350">Az indexek, az Azure Search a program automatikusan felosztása több szegmensben osztják, így nekünk tooquickly terjesztése hello index között több csomópont alatt szolgáltatás méretezési regisztrálnia vagy csökkentheti.</span><span class="sxs-lookup"><span data-stu-id="05fbe-350">All indexes in Azure Search are automatically split into multiple shards, allowing us tooquickly distribute hello index among multiple nodes during service scale up or scale down.</span></span> <span data-ttu-id="05fbe-351">Keresési kérelem elküldésekor megjelenítés elleni minden shard egymástól függetlenül.</span><span class="sxs-lookup"><span data-stu-id="05fbe-351">When a search request is issued, it’s issued against each shard independently.</span></span> <span data-ttu-id="05fbe-352">hello eredmények az egyes shard majd egyesített és (Ha nincs más rendezés van meghatározva) pontszám szerint rendezve.</span><span class="sxs-lookup"><span data-stu-id="05fbe-352">hello results from each shard are then merged and ordered by score (if no other ordering is defined).</span></span> <span data-ttu-id="05fbe-353">Fontos, hogy minden szilánkok között nem pontozó függvény súlyok lekérdezési kifejezés gyakoriság szemben az összes dokumentumban belül hello shard, más néven inverz dokumentum gyakoriságát hello tooknow!</span><span class="sxs-lookup"><span data-stu-id="05fbe-353">It is important tooknow that hello scoring function weights query term frequency against its inverse document frequency in all documents within hello shard, not across all shards!</span></span>

<span data-ttu-id="05fbe-354">Ez azt jelenti, hogy egy relevanciájának pontszám *sikerült* térhet azonos dokumentumokhoz, ha ezek különböző szilánkok legyen elhelyezve.</span><span class="sxs-lookup"><span data-stu-id="05fbe-354">This means a relevance score *could* be different for identical documents if they reside on different shards.</span></span> <span data-ttu-id="05fbe-355">Ezek az eltérések Szerencsére toodisappear általában a dokumentumok hello index hello száma növekedésével toomore még akkor is, kifejezés terjesztési miatt.</span><span class="sxs-lookup"><span data-stu-id="05fbe-355">Fortunately, such differences tend toodisappear as hello number of documents in hello index grows due toomore even term distribution.</span></span> <span data-ttu-id="05fbe-356">A mely shard bármely adott dokumentum kerülnek lehetséges tooassume nincs.</span><span class="sxs-lookup"><span data-stu-id="05fbe-356">It’s not possible tooassume on which shard any given document will be placed.</span></span> <span data-ttu-id="05fbe-357">Azonban ha egy dokumentum kulcs nem változik, a rendszer mindig hozzárendel toohello ugyanazt a shard.</span><span class="sxs-lookup"><span data-stu-id="05fbe-357">However, assuming a document key doesn't change, it will always be assigned toohello same shard.</span></span>

<span data-ttu-id="05fbe-358">A dokumentum pontszám általában nem ajánlott attribútum hello dokumentumok rendezéshez, ha fontos a sorrend stabilitását.</span><span class="sxs-lookup"><span data-stu-id="05fbe-358">In general, document score is not hello best attribute for ordering documents if order stability is important.</span></span> <span data-ttu-id="05fbe-359">Például egy azonos pontszám két dokumentum megadott, nincs garancia melyik jelenik meg először a későbbi frissítési kísérletei során hello ugyanabban a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="05fbe-359">For example, given two document with an identical score, there is no guarantee which one appears first in subsequent runs of hello same query.</span></span> <span data-ttu-id="05fbe-360">A dokumentum pontszám csak adjon dokumentum fontos általános értelemben relatív tooother dokumentumok hello eredmények készletében.</span><span class="sxs-lookup"><span data-stu-id="05fbe-360">Document score should only give a general sense of document relevance relative tooother documents in hello results set.</span></span>

## <a name="conclusion"></a><span data-ttu-id="05fbe-361">Összegzés</span><span class="sxs-lookup"><span data-stu-id="05fbe-361">Conclusion</span></span>

<span data-ttu-id="05fbe-362">internetes keresők hello sikerességének titkos adatok teljes szöveges keresés az elvárások idézett elő.</span><span class="sxs-lookup"><span data-stu-id="05fbe-362">hello success of internet search engines has raised expectations for full text search over private data.</span></span> <span data-ttu-id="05fbe-363">Szinte bármilyen keresési élményt biztosít most várhatóan hello motor toounderstand a leképezést, még akkor is, ha feltételek hibásan van megadva vagy hiányos.</span><span class="sxs-lookup"><span data-stu-id="05fbe-363">For almost any kind of search experience, we now expect hello engine toounderstand our intent, even when terms are misspelled or incomplete.</span></span> <span data-ttu-id="05fbe-364">Akkor is előfordulhat, hogy várhatóan közelében kifejezések vagy soha nem ténylegesen megadott szinonimák alapján.</span><span class="sxs-lookup"><span data-stu-id="05fbe-364">We might even expect matches based on near equivalent terms or synonyms that we never actually specified.</span></span>

<span data-ttu-id="05fbe-365">Műszaki szempontból a teljes szöveges keresés nagyon összetett, nyelvi kifinomult elemzést és egy rendszeres megközelítés tooprocessing, hogy az eszközöket, bontsa ki és lekérdezési kifejezések toodeliver vonatkozó eredményt átalakító.</span><span class="sxs-lookup"><span data-stu-id="05fbe-365">From a technical standpoint, full text search is highly complex, requiring sophisticated linguistic analysis and a systematic approach tooprocessing in ways that distill, expand, and transform query terms toodeliver a relevant result.</span></span> <span data-ttu-id="05fbe-366">A megadott hello rejlő összetett szolgáltatásokkal, tényezőket, amelyek hatással lehetnek a lekérdezés eredményeit hello számos vannak.</span><span class="sxs-lookup"><span data-stu-id="05fbe-366">Given hello inherent complexities, there are a lot of factors that can affect hello outcome of a query.</span></span> <span data-ttu-id="05fbe-367">Emiatt hello idő toounderstand hello beállítás esetén a teljes szöveges keresés befektetés előnyökkel anyagi toowork keresztül váratlan eredményekhez tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="05fbe-367">For this reason, investing hello time toounderstand hello mechanics of full text search offers tangible benefits when trying toowork through unexpected results.</span></span>  

<span data-ttu-id="05fbe-368">Ez a cikk felfedezte hello környezetében Azure Search teljes szöveges keresés.</span><span class="sxs-lookup"><span data-stu-id="05fbe-368">This article explored full text search in hello context of Azure Search.</span></span> <span data-ttu-id="05fbe-369">Reméljük, biztosít elegendő háttér toorecognize lehetséges okokért és megoldásokért lekérdezés kapcsolatos gyakori problémák címzéshez.</span><span class="sxs-lookup"><span data-stu-id="05fbe-369">We hope it gives you sufficient background toorecognize potential causes and resolutions for addressing common query problems.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="05fbe-370">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="05fbe-370">Next steps</span></span>

+ <span data-ttu-id="05fbe-371">Hozza létre a hello minta index, próbálja ki a különböző lekérdezéseket, és tekintse át az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="05fbe-371">Build hello sample index, try out different queries and review results.</span></span> <span data-ttu-id="05fbe-372">Útmutatásért lásd: [hozza létre és hello portálon index lekérdezése](search-get-started-portal.md#query-index).</span><span class="sxs-lookup"><span data-stu-id="05fbe-372">For instructions, see [Build and query an index in hello portal](search-get-started-portal.md#query-index).</span></span>

+ <span data-ttu-id="05fbe-373">További lekérdezési szintaxist hello próbálja [dokumentumok keresése](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) példa szakasz vagy [egyszerű lekérdezés szintaxisát](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) a keresési ablak hello portálon.</span><span class="sxs-lookup"><span data-stu-id="05fbe-373">Try additional query syntax from hello [Search Documents](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) example section or from [Simple query syntax](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) in Search explorer in hello portal.</span></span>

+ <span data-ttu-id="05fbe-374">Felülvizsgálati [profilok pontozási](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) Ha azt szeretné, hogy az alkalmazás prioritás tootune.</span><span class="sxs-lookup"><span data-stu-id="05fbe-374">Review [scoring profiles](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) if you want tootune ranking in your search application.</span></span>

+ <span data-ttu-id="05fbe-375">Megtudhatja, hogyan tooapply [nyelvspecifikus lexikális elemzőkkel](https://docs.microsoft.com/rest/api/searchservice/language-support).</span><span class="sxs-lookup"><span data-stu-id="05fbe-375">Learn how tooapply [language-specific lexical analyzers](https://docs.microsoft.com/rest/api/searchservice/language-support).</span></span>

+ <span data-ttu-id="05fbe-376">[Konfigurálja az egyéni lekérdezések](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) minimális feldolgozási vagy adott mezők speciális terhelése.</span><span class="sxs-lookup"><span data-stu-id="05fbe-376">[Configure custom analyzers](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) for either minimal processing or specialized processing on specific fields.</span></span>

+ <span data-ttu-id="05fbe-377">[Hasonlítsa össze a szabványos és az angol nyelvű elemzőkkel](http://alice.unearth.ai/))-mellé a bemutató webhelyen.</span><span class="sxs-lookup"><span data-stu-id="05fbe-377">[Compare standard and English analyzers](http://alice.unearth.ai/)) side-by-side on this demo web site.</span></span> 

## <a name="see-also"></a><span data-ttu-id="05fbe-378">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="05fbe-378">See also</span></span>

[<span data-ttu-id="05fbe-379">REST API-t dokumentumok keresése</span><span class="sxs-lookup"><span data-stu-id="05fbe-379">Search Documents REST API</span></span>](https://docs.microsoft.com/rest/api/searchservice/search-documents)

[<span data-ttu-id="05fbe-380">Egyszerű lekérdezési szintaxis</span><span class="sxs-lookup"><span data-stu-id="05fbe-380">Simple query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)

[<span data-ttu-id="05fbe-381">Teljes Lucene lekérdezés szintaxisa</span><span class="sxs-lookup"><span data-stu-id="05fbe-381">Full Lucene query syntax</span></span>](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

[<span data-ttu-id="05fbe-382">A keresési eredmények kezelése</span><span class="sxs-lookup"><span data-stu-id="05fbe-382">Handle search results</span></span>](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
[2]: ./media/search-lucene-query-architecture/azSearch-queryparsing-should2.png
[3]: ./media/search-lucene-query-architecture/azSearch-queryparsing-must2.png
[4]: ./media/search-lucene-query-architecture/azSearch-queryparsing-spacious2.png
