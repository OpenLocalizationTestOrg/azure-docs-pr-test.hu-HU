---
title: "Útmutató az Azure Search keresési eredmények lapon |} Microsoft Docs"
description: "Az Azure Search, egy üzemeltetett felhőalapú keresőszolgáltatás, a Microsoft Azure tördelési."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: a0a1d315-8624-4cdf-b38e-ba12569c6fcc
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: heidist
ms.openlocfilehash: 1054e15a2751c53aad5dbc8054c4cec41102dee9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-page-search-results-in-azure-search"></a><span data-ttu-id="287cc-103">A keresési eredmények oldalakra tördelése az Azure Search-ben</span><span class="sxs-lookup"><span data-stu-id="287cc-103">How to page search results in Azure Search</span></span>
<span data-ttu-id="287cc-104">Ez a cikk az Azure Search szolgáltatás REST API használatával valósítja meg a keresési eredmények oldalának, például az érintett teljes, a dokumentum beolvasása, a rendezési sorrend és a navigációs szokásos megoldások szabványos elemeit nyújt útmutatást.</span><span class="sxs-lookup"><span data-stu-id="287cc-104">This article provides guidance on how to use the Azure Search Service REST API to implement standard elements of a search results page, such as total counts, document retrieval, sort orders, and navigation.</span></span>

<span data-ttu-id="287cc-105">Az alábbiakban leírt minden esetben keresztül megadott lapokkal kapcsolatos beállításokat, amelyek adatokat vagy a keresési eredmények oldalának információkat a [keresés a dokumentum](http://msdn.microsoft.com/library/azure/dn798927.aspx) az Azure Search szolgáltatás küldött kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="287cc-105">In every case mentioned below, page-related options that contribute data or information to your search results page are specified through the [Search Document](http://msdn.microsoft.com/library/azure/dn798927.aspx) requests sent to your Azure Search Service.</span></span> <span data-ttu-id="287cc-106">A kérelemnek tartalmaznia egy GET parancs, a elérési utat, és a lekérdezési paraméterek, amely tájékoztatja a szolgáltatás milyen vonatkozó kérelem és a hogyan állítson össze a választ.</span><span class="sxs-lookup"><span data-stu-id="287cc-106">Requests include a GET command, path, and query parameters that inform the service what is being requested, and how to formulate the response.</span></span>

> [!NOTE]
> <span data-ttu-id="287cc-107">Kérés számos olyan elemeket, például egy URL-címe és az elérési út, HTTP-műveletet, `api-version`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="287cc-107">A valid request includes a number of elements, such as a service URL and path, HTTP verb, `api-version`, and so on.</span></span> <span data-ttu-id="287cc-108">Kivonatosan mutatja azt levágja a példák, jelölje ki, amely fontos tördelési szintaxisát.</span><span class="sxs-lookup"><span data-stu-id="287cc-108">For brevity, we trimmed the examples to highlight just the syntax that is relevant to pagination.</span></span> <span data-ttu-id="287cc-109">Tekintse meg a [Azure Search szolgáltatás REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) dokumentációjában szintaxis található.</span><span class="sxs-lookup"><span data-stu-id="287cc-109">Please see the [Azure Search Service REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) documentation for details about request syntax.</span></span>
> 
> 

## <a name="total-hits-and-page-counts"></a><span data-ttu-id="287cc-110">Találatok és a lap száma</span><span class="sxs-lookup"><span data-stu-id="287cc-110">Total hits and Page Counts</span></span>
<span data-ttu-id="287cc-111">Egy lekérdezés által visszaadott eredmények száma megjelenítő, és majd visszaadni az eredmények kisebb csoportjai, az alapvető gyakorlatilag az összes keresési lapra.</span><span class="sxs-lookup"><span data-stu-id="287cc-111">Showing the total number of results returned from a query, and then returning those results in smaller chunks, is fundamental to virtually all search pages.</span></span>

![][1]

<span data-ttu-id="287cc-112">Az Azure Search, használja a `$count`, `$top`, és `$skip` paramétereit, és ezeket az értékeket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="287cc-112">In Azure Search, you use the `$count`, `$top`, and `$skip` parameters to return these values.</span></span> <span data-ttu-id="287cc-113">A következő példa bemutatja, adja vissza a találatok összesen mintát kér `@OData.count`:</span><span class="sxs-lookup"><span data-stu-id="287cc-113">The following example shows a sample request for total hits, returned as `@OData.count`:</span></span>

        GET /indexes/onlineCatalog/docs?$count=true

<span data-ttu-id="287cc-114">15 csoportokban dokumentumok beolvasása, és a találatok, az első lap kezdődő is megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="287cc-114">Retrieve documents in groups of 15, and also show the total hits, starting at the first page:</span></span>

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

<span data-ttu-id="287cc-115">Eredmények paginating szükséges az `$top` és `$skip`, ahol `$top` határozza meg, hány elemek egy kötegelt válasz és `$skip` határozza meg, hány elemek kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="287cc-115">Paginating results requires both `$top` and `$skip`, where `$top` specifies how many items to return in a batch, and `$skip` specifies how many items to skip.</span></span> <span data-ttu-id="287cc-116">A következő példában a növekményes ugráskor minden lapon látható a következő 15 elemek jelzi a `$skip` paraméter.</span><span class="sxs-lookup"><span data-stu-id="287cc-116">In the following example, each page shows the next 15 items, indicated by the incremental jumps in the `$skip` parameter.</span></span>

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a><span data-ttu-id="287cc-117">Elrendezés</span><span class="sxs-lookup"><span data-stu-id="287cc-117">Layout</span></span>
<span data-ttu-id="287cc-118">A keresési eredmények oldalát előfordulhat, hogy kívánja megjeleníteni a miniatűr képére, mezők és a teljes termék lapra mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="287cc-118">On a search results page, you might want to show a thumbnail image, a subset of fields, and a link to a full product page.</span></span>

 ![][2]

<span data-ttu-id="287cc-119">Használja az Azure Search `$select` és a keresési parancs, ez a felület megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="287cc-119">In Azure Search, you would use `$select` and a lookup command to implement this experience.</span></span>

<span data-ttu-id="287cc-120">Egy mozaik elrendezés mezők vissza:</span><span class="sxs-lookup"><span data-stu-id="287cc-120">To return a subset of fields for a tiled layout:</span></span>

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

<span data-ttu-id="287cc-121">Képek és médiafájlok nem közvetlenül kereshető, és egy másik tárolási platform, például az Azure Blob Storage tárolóban, költségek csökkentése érdekében kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="287cc-121">Images and media files are not directly searchable and should be stored in another storage platform, such as Azure Blob storage, to reduce costs.</span></span> <span data-ttu-id="287cc-122">Az index és a dokumentumok határozza meg az URL-címet a külső tartalom tárol.</span><span class="sxs-lookup"><span data-stu-id="287cc-122">In the index and documents, define a field that stores the URL address of the external content.</span></span> <span data-ttu-id="287cc-123">A mezőben egy Képhivatkozás módon használhatja.</span><span class="sxs-lookup"><span data-stu-id="287cc-123">You can then use the field as an image reference.</span></span> <span data-ttu-id="287cc-124">A kép URL-című dokumentumban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="287cc-124">The URL to the image should be in the document.</span></span>

<span data-ttu-id="287cc-125">A termék leírását tartalmazó lapon a beolvasandó egy **onClick** esemény, használja [keresési dokumentum](http://msdn.microsoft.com/library/azure/dn798929.aspx) felelt meg a kulcs a dokumentum beolvasása.</span><span class="sxs-lookup"><span data-stu-id="287cc-125">To retrieve a product description page for an **onClick** event, use [Lookup Document](http://msdn.microsoft.com/library/azure/dn798929.aspx) to pass in the key of the document to retrieve.</span></span> <span data-ttu-id="287cc-126">A kulcs adattípusa `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="287cc-126">The data type of the key is `Edm.String`.</span></span> <span data-ttu-id="287cc-127">Az ebben a példában is *246810*.</span><span class="sxs-lookup"><span data-stu-id="287cc-127">In this example, it is *246810*.</span></span> 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a><span data-ttu-id="287cc-128">Rendezési szempont relevanciájának, minősítés vagy ár</span><span class="sxs-lookup"><span data-stu-id="287cc-128">Sort by relevance, rating, or price</span></span>
<span data-ttu-id="287cc-129">Sorrendjét relevanciájának gyakran alapértelmezett, de a gyakori, hogy alternatív rendezési rendelések azonnal elérhetők, hogy az ügyfelek is gyorsan átütemezésével a meglévő eredményeit azokat egy másik sorrend.</span><span class="sxs-lookup"><span data-stu-id="287cc-129">Sort orders often default to relevance, but it's common to make alternative sort orders readily available so that customers can quickly reshuffle existing results into a different rank order.</span></span>

 ![][3]

<span data-ttu-id="287cc-130">Az Azure Search rendezés alapul a `$orderby` kifejezés indexeli, mezők`"Sortable": true.`</span><span class="sxs-lookup"><span data-stu-id="287cc-130">In Azure Search, sorting is based on the `$orderby` expression, for all fields that are indexed as `"Sortable": true.`</span></span>

<span data-ttu-id="287cc-131">Relevanciájának erősen kapcsolódik a pontozási profil.</span><span class="sxs-lookup"><span data-stu-id="287cc-131">Relevance is strongly associated with scoring profiles.</span></span> <span data-ttu-id="287cc-132">Segítségével az alapértelmezett pontozási, amely szöveg elemzése és a statisztika támaszkodik a dokumentumok további vagy erősebb egyezés Ugrás a kívánt keresőkifejezést a magasabb pontszámok rangsorolni levő összes eredményt.</span><span class="sxs-lookup"><span data-stu-id="287cc-132">You can use the default scoring, which relies on text analysis and statistics to rank order all results, with higher scores going to documents with more or stronger matches on a search term.</span></span>

<span data-ttu-id="287cc-133">Alternatív a rendezési sorrend társított általában **onClick** eseményeket, amelyek a számon olyan módszer, amelyik a rendezési sorrend alkot.</span><span class="sxs-lookup"><span data-stu-id="287cc-133">Alternative sort orders are typically associated with **onClick** events that call back to a method that builds the sort order.</span></span> <span data-ttu-id="287cc-134">Például adja meg a page eleme:</span><span class="sxs-lookup"><span data-stu-id="287cc-134">For example, given this page element:</span></span>

 ![][4]

<span data-ttu-id="287cc-135">Fogad el bemenetként a kijelölt rendezési beállítást, és ez a lehetőség társított feltétel rendezett listák metódus hozna létre.</span><span class="sxs-lookup"><span data-stu-id="287cc-135">You would create a method that accepts the selected sort option as input, and returns an ordered list for the criteria associated with that option.</span></span>

 ![][5]

> [!NOTE]
> <span data-ttu-id="287cc-136">Az alapértelmezett pontozási nem elegendő-e több forgatókönyv, de javasolt relevanciájának alapozva egyéni pontozási profil helyette.</span><span class="sxs-lookup"><span data-stu-id="287cc-136">While the default scoring is sufficient for many scenarios, we recommend basing relevance on a custom scoring profile instead.</span></span> <span data-ttu-id="287cc-137">Egyéni pontozási profil akadályozható meg, amelyek az üzleti több hasznos program elemekre.</span><span class="sxs-lookup"><span data-stu-id="287cc-137">A custom scoring profile gives you a way to boost items that are more beneficial to your business.</span></span> <span data-ttu-id="287cc-138">Lásd: [a relevanciaprofil felvétele](http://msdn.microsoft.com/library/azure/dn798928.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="287cc-138">See [Add a scoring profile](http://msdn.microsoft.com/library/azure/dn798928.aspx) for more information.</span></span> 
> 
> 

## <a name="faceted-navigation"></a><span data-ttu-id="287cc-139">Jellemzőalapú navigáció</span><span class="sxs-lookup"><span data-stu-id="287cc-139">Faceted navigation</span></span>
<span data-ttu-id="287cc-140">Keresési navigációs esetében gyakori, a találatokat tartalmazó lap, gyakran oldalán vagy a lap tetején található meg.</span><span class="sxs-lookup"><span data-stu-id="287cc-140">Search navigation is common on a results page, often located at the side or top of a page.</span></span> <span data-ttu-id="287cc-141">Az Azure Search jellemzőalapú navigációs biztosít irányuló keresési előre definiált szűrők alapján.</span><span class="sxs-lookup"><span data-stu-id="287cc-141">In Azure Search, faceted navigation provides self-directed search based on predefined filters.</span></span> <span data-ttu-id="287cc-142">Lásd: [az Azure Search Jellemzőalapú navigációs](search-faceted-navigation.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="287cc-142">See [Faceted navigation in Azure Search](search-faceted-navigation.md) for details.</span></span>

## <a name="filters-at-the-page-level"></a><span data-ttu-id="287cc-143">A lap szintjén szűrők</span><span class="sxs-lookup"><span data-stu-id="287cc-143">Filters at the page level</span></span>
<span data-ttu-id="287cc-144">Ha a megoldás kialakításának részét dedikált keresési lapok az adott típusú tartalmakat (például egy online kereskedelmi alkalmazás, amely rendelkezik a lap tetején felsorolt osztályok), egy kifejezést mellett is beszúrhat egy **onClick**esemény előszűrt állapotban oldal megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="287cc-144">If your solution design included dedicated search pages for specific types of content (for example, an online retail application that has departments listed at the top of the page), you can insert a filter expression alongside an **onClick** event to open a page in a prefiltered state.</span></span> 

<span data-ttu-id="287cc-145">Egy szűrőt, vagy egy keresési kifejezés nélkül küldhet.</span><span class="sxs-lookup"><span data-stu-id="287cc-145">You can send a filter with or without a search expression.</span></span> <span data-ttu-id="287cc-146">Például a következő kérés márka neve csak a megfelelő azt dokumentumok visszaadó szűrést.</span><span class="sxs-lookup"><span data-stu-id="287cc-146">For example, the following request will filter on brand name, returning only those documents that match it.</span></span>

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

<span data-ttu-id="287cc-147">Lásd: [dokumentumok keresése (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) kapcsolatos további információk `$filter` kifejezések.</span><span class="sxs-lookup"><span data-stu-id="287cc-147">See [Search Documents (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) for more information about `$filter` expressions.</span></span>

## <a name="see-also"></a><span data-ttu-id="287cc-148">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="287cc-148">See Also</span></span>
* [<span data-ttu-id="287cc-149">Az Azure Search szolgáltatás REST API</span><span class="sxs-lookup"><span data-stu-id="287cc-149">Azure Search Service REST API</span></span>](http://msdn.microsoft.com/library/azure/dn798935.aspx)
* [<span data-ttu-id="287cc-150">Indexművelet</span><span class="sxs-lookup"><span data-stu-id="287cc-150">Index Operations</span></span>](http://msdn.microsoft.com/library/azure/dn798918.aspx)
* [<span data-ttu-id="287cc-151">A dokumentum műveletek</span><span class="sxs-lookup"><span data-stu-id="287cc-151">Document Operations</span></span>](http://msdn.microsoft.com/library/azure/dn800962.aspx)
* [<span data-ttu-id="287cc-152">Videók és oktatóanyagok Azure Search kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="287cc-152">Video and tutorials about Azure Search</span></span>](search-video-demo-tutorial-list.md)
* [<span data-ttu-id="287cc-153">Az Azure Search jellemzőalapú navigáció</span><span class="sxs-lookup"><span data-stu-id="287cc-153">Faceted Navigation in Azure Search</span></span>](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
