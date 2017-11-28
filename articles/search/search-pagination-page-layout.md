---
title: "aaaHow toopage keresési találatok az Azure Search |} Microsoft Docs"
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
ms.openlocfilehash: e3abc1ca4d5994b0a77955379081a4fcfa5a7fa7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toopage-search-results-in-azure-search"></a><span data-ttu-id="f3aa8-103">Hogyan toopage keresési eredmények az Azure Search</span><span class="sxs-lookup"><span data-stu-id="f3aa8-103">How toopage search results in Azure Search</span></span>
<span data-ttu-id="f3aa8-104">Ez a cikk hogyan toouse hello Azure Search szolgáltatás REST API tooimplement szokásos megoldások szabványos elemeit a keresési eredmények, például az érintett teljes, a dokumentum beolvasása, a rendezési sorrend és a navigációs nyújt útmutatást.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-104">This article provides guidance on how toouse hello Azure Search Service REST API tooimplement standard elements of a search results page, such as total counts, document retrieval, sort orders, and navigation.</span></span>

<span data-ttu-id="f3aa8-105">Az alábbiakban leírt minden esetben hello keresztül megadott lapokkal kapcsolatos beállításokat Ez hozzájárul az adatok és információk tooyour keresési eredmények oldalának [keresés a dokumentum](http://msdn.microsoft.com/library/azure/dn798927.aspx) küldött kérelmek tooyour Azure Search szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-105">In every case mentioned below, page-related options that contribute data or information tooyour search results page are specified through hello [Search Document](http://msdn.microsoft.com/library/azure/dn798927.aspx) requests sent tooyour Azure Search Service.</span></span> <span data-ttu-id="f3aa8-106">A kérelemnek tartalmaznia egy GET parancs, a elérési utat, és a lekérdezési paraméterek, amely tájékoztatja a mi vonatkozó kérelem hello szolgáltatást és a hogyan tooformulate hello válasz.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-106">Requests include a GET command, path, and query parameters that inform hello service what is being requested, and how tooformulate hello response.</span></span>

> [!NOTE]
> <span data-ttu-id="f3aa8-107">Kérés számos olyan elemeket, például egy URL-címe és az elérési út, HTTP-műveletet, `api-version`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-107">A valid request includes a number of elements, such as a service URL and path, HTTP verb, `api-version`, and so on.</span></span> <span data-ttu-id="f3aa8-108">Kivonatosan mutatja hello példák toohighlight csak hello szintaxis, amely megfelelő toopagination rövidített azt.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-108">For brevity, we trimmed hello examples toohighlight just hello syntax that is relevant toopagination.</span></span> <span data-ttu-id="f3aa8-109">Tekintse meg a hello [Azure Search szolgáltatás REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) dokumentációjában szintaxis található.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-109">Please see hello [Azure Search Service REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) documentation for details about request syntax.</span></span>
> 
> 

## <a name="total-hits-and-page-counts"></a><span data-ttu-id="f3aa8-110">Találatok és a lap száma</span><span class="sxs-lookup"><span data-stu-id="f3aa8-110">Total hits and Page Counts</span></span>
<span data-ttu-id="f3aa8-111">Alapvető toovirtually megjelenítő hello összes, egy lekérdezés által visszaadott eredmények száma, és azokat, majd vissza kisebb csoportjai eredményezi, az összes keresési lapok.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-111">Showing hello total number of results returned from a query, and then returning those results in smaller chunks, is fundamental toovirtually all search pages.</span></span>

![][1]

<span data-ttu-id="f3aa8-112">Az Azure Search hello használata `$count`, `$top`, és `$skip` paraméterek tooreturn ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-112">In Azure Search, you use hello `$count`, `$top`, and `$skip` parameters tooreturn these values.</span></span> <span data-ttu-id="f3aa8-113">hello alábbi példa azt mutatja, adja vissza a találatok összesen mintát kér `@OData.count`:</span><span class="sxs-lookup"><span data-stu-id="f3aa8-113">hello following example shows a sample request for total hits, returned as `@OData.count`:</span></span>

        GET /indexes/onlineCatalog/docs?$count=true

<span data-ttu-id="f3aa8-114">15 csoportokban dokumentumok beolvasása, és hello találatok, hello első lapon kezdődő is megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="f3aa8-114">Retrieve documents in groups of 15, and also show hello total hits, starting at hello first page:</span></span>

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

<span data-ttu-id="f3aa8-115">Eredmények paginating szükséges az `$top` és `$skip`, ahol `$top` Megadja, hány elemek tooreturn kötegekben, és `$skip` Megadja, hogy hány elemek tooskip.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-115">Paginating results requires both `$top` and `$skip`, where `$top` specifies how many items tooreturn in a batch, and `$skip` specifies how many items tooskip.</span></span> <span data-ttu-id="f3aa8-116">A hello a következő példában minden egyes lapon látható hello mellett 15 elemeket jelzi hello növekményes ezzel a művelettel a hello `$skip` paraméter.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-116">In hello following example, each page shows hello next 15 items, indicated by hello incremental jumps in hello `$skip` parameter.</span></span>

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=0&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=15&$count=true

        GET /indexes/onlineCatalog/docs?search=*$top=15&$skip=30&$count=true

## <a name="layout"></a><span data-ttu-id="f3aa8-117">Elrendezés</span><span class="sxs-lookup"><span data-stu-id="f3aa8-117">Layout</span></span>
<span data-ttu-id="f3aa8-118">A keresési eredmények oldalát érdemes lehet tooshow miniatűr képére, mezők és a hivatkozás tooa teljes termék oldalát.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-118">On a search results page, you might want tooshow a thumbnail image, a subset of fields, and a link tooa full product page.</span></span>

 ![][2]

<span data-ttu-id="f3aa8-119">Használja az Azure Search `$select` és a keresési parancs tooimplement a felhasználói élmény.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-119">In Azure Search, you would use `$select` and a lookup command tooimplement this experience.</span></span>

<span data-ttu-id="f3aa8-120">egy mozaik elrendezés mezők tooreturn:</span><span class="sxs-lookup"><span data-stu-id="f3aa8-120">tooreturn a subset of fields for a tiled layout:</span></span>

        GET /indexes/ onlineCatalog/docs?search=*&$select=productName,imageFile,description,price,rating 

<span data-ttu-id="f3aa8-121">Képek és médiafájlok tárolására nincsenek közvetlenül kereshető, és egy másik tárolási platform, például az Azure Blob Storage tárolóban, tooreduce költségek kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-121">Images and media files are not directly searchable and should be stored in another storage platform, such as Azure Blob storage, tooreduce costs.</span></span> <span data-ttu-id="f3aa8-122">A hello index és a dokumentumok határozza meg a külső tartalom hello hello URL-címet tárol.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-122">In hello index and documents, define a field that stores hello URL address of hello external content.</span></span> <span data-ttu-id="f3aa8-123">Hello mező egy Képhivatkozás módon használhatja.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-123">You can then use hello field as an image reference.</span></span> <span data-ttu-id="f3aa8-124">hello URL-cím toohello lemezkép hello dokumentumban lehet.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-124">hello URL toohello image should be in hello document.</span></span>

<span data-ttu-id="f3aa8-125">Leírás lap tooretrieve egy **onClick** esemény, használjon [keresési dokumentum](http://msdn.microsoft.com/library/azure/dn798929.aspx) toopass hello dokumentum tooretrieve hello kulcsában.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-125">tooretrieve a product description page for an **onClick** event, use [Lookup Document](http://msdn.microsoft.com/library/azure/dn798929.aspx) toopass in hello key of hello document tooretrieve.</span></span> <span data-ttu-id="f3aa8-126">hello hello kulcs adattípusa `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-126">hello data type of hello key is `Edm.String`.</span></span> <span data-ttu-id="f3aa8-127">Az ebben a példában is *246810*.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-127">In this example, it is *246810*.</span></span> 

        GET /indexes/onlineCatalog/docs/246810

## <a name="sort-by-relevance-rating-or-price"></a><span data-ttu-id="f3aa8-128">Rendezési szempont relevanciájának, minősítés vagy ár</span><span class="sxs-lookup"><span data-stu-id="f3aa8-128">Sort by relevance, rating, or price</span></span>
<span data-ttu-id="f3aa8-129">Sorrendjét gyakran toorelevance alapértelmezett, de közös toomake alternatív a rendezési sorrend azonnal elérhetők, hogy az ügyfelek is gyorsan átütemezésével a meglévő eredményeit azokat egy másik sorrend.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-129">Sort orders often default toorelevance, but it's common toomake alternative sort orders readily available so that customers can quickly reshuffle existing results into a different rank order.</span></span>

 ![][3]

<span data-ttu-id="f3aa8-130">Az Azure Search rendezés alapuló hello `$orderby` kifejezés indexeli, mezők`"Sortable": true.`</span><span class="sxs-lookup"><span data-stu-id="f3aa8-130">In Azure Search, sorting is based on hello `$orderby` expression, for all fields that are indexed as `"Sortable": true.`</span></span>

<span data-ttu-id="f3aa8-131">Relevanciájának erősen kapcsolódik a pontozási profil.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-131">Relevance is strongly associated with scoring profiles.</span></span> <span data-ttu-id="f3aa8-132">Használhat hello alapértelmezett pontozási, amely támaszkodik szöveg elemzése és statisztikái toorank sorrendje minden eredmény elérése érdekében a további vagy erősebb megegyezik a toodocuments továbblép a kívánt keresőkifejezést a magasabb pontszámok.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-132">You can use hello default scoring, which relies on text analysis and statistics toorank order all results, with higher scores going toodocuments with more or stronger matches on a search term.</span></span>

<span data-ttu-id="f3aa8-133">Alternatív a rendezési sorrend társított általában **onClick** eseményeket, amelyek visszahívási tooa metódus, amely buildek hello rendezési sorrend.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-133">Alternative sort orders are typically associated with **onClick** events that call back tooa method that builds hello sort order.</span></span> <span data-ttu-id="f3aa8-134">Például adja meg a page eleme:</span><span class="sxs-lookup"><span data-stu-id="f3aa8-134">For example, given this page element:</span></span>

 ![][4]

<span data-ttu-id="f3aa8-135">Fogad el bemenetként kijelölt hello rendezési beállítás, és ez a lehetőség társított hello feltétel rendezett listák metódus hozna létre.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-135">You would create a method that accepts hello selected sort option as input, and returns an ordered list for hello criteria associated with that option.</span></span>

 ![][5]

> [!NOTE]
> <span data-ttu-id="f3aa8-136">Hello alapértelmezett pontozási nem elegendő a számos forgatókönyv, de javasolt relevanciájának alapozva egyéni pontozási profil helyette.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-136">While hello default scoring is sufficient for many scenarios, we recommend basing relevance on a custom scoring profile instead.</span></span> <span data-ttu-id="f3aa8-137">Egy egyéni relevanciaprofil lehetővé teszi egy tooboost elemek, amelyek több előnyös tooyour üzleti.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-137">A custom scoring profile gives you a way tooboost items that are more beneficial tooyour business.</span></span> <span data-ttu-id="f3aa8-138">Lásd: [a relevanciaprofil felvétele](http://msdn.microsoft.com/library/azure/dn798928.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-138">See [Add a scoring profile](http://msdn.microsoft.com/library/azure/dn798928.aspx) for more information.</span></span> 
> 
> 

## <a name="faceted-navigation"></a><span data-ttu-id="f3aa8-139">Jellemzőalapú navigáció</span><span class="sxs-lookup"><span data-stu-id="f3aa8-139">Faceted navigation</span></span>
<span data-ttu-id="f3aa8-140">Keresési navigációs esetében gyakori, a találatokat tartalmazó lap, gyakran hello oldalán vagy a lap tetején található meg.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-140">Search navigation is common on a results page, often located at hello side or top of a page.</span></span> <span data-ttu-id="f3aa8-141">Az Azure Search jellemzőalapú navigációs biztosít irányuló keresési előre definiált szűrők alapján.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-141">In Azure Search, faceted navigation provides self-directed search based on predefined filters.</span></span> <span data-ttu-id="f3aa8-142">Lásd: [az Azure Search Jellemzőalapú navigációs](search-faceted-navigation.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-142">See [Faceted navigation in Azure Search](search-faceted-navigation.md) for details.</span></span>

## <a name="filters-at-hello-page-level"></a><span data-ttu-id="f3aa8-143">Szűrők hello lap szinten</span><span class="sxs-lookup"><span data-stu-id="f3aa8-143">Filters at hello page level</span></span>
<span data-ttu-id="f3aa8-144">Ha a megoldás kialakításának részét dedikált keresési lapok az adott típusú tartalmakat (például egy online kereskedelmi alkalmazás részlegek hello tetején hello lapján felsorolt), egy kifejezést mellett is beszúrhat egy **onClick** esemény tooopen egy lap előszűrt állapotban.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-144">If your solution design included dedicated search pages for specific types of content (for example, an online retail application that has departments listed at hello top of hello page), you can insert a filter expression alongside an **onClick** event tooopen a page in a prefiltered state.</span></span> 

<span data-ttu-id="f3aa8-145">Egy szűrőt, vagy egy keresési kifejezés nélkül küldhet.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-145">You can send a filter with or without a search expression.</span></span> <span data-ttu-id="f3aa8-146">Például hello következő kérelem szűrést márka neve csak a megfelelő azt dokumentumok ad vissza.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-146">For example, hello following request will filter on brand name, returning only those documents that match it.</span></span>

        GET /indexes/onlineCatalog/docs?$filter=brandname eq ‘Microsoft’ and category eq ‘Games’

<span data-ttu-id="f3aa8-147">Lásd: [dokumentumok keresése (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) kapcsolatos további információk `$filter` kifejezések.</span><span class="sxs-lookup"><span data-stu-id="f3aa8-147">See [Search Documents (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) for more information about `$filter` expressions.</span></span>

## <a name="see-also"></a><span data-ttu-id="f3aa8-148">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f3aa8-148">See Also</span></span>
* [<span data-ttu-id="f3aa8-149">Az Azure Search szolgáltatás REST API</span><span class="sxs-lookup"><span data-stu-id="f3aa8-149">Azure Search Service REST API</span></span>](http://msdn.microsoft.com/library/azure/dn798935.aspx)
* [<span data-ttu-id="f3aa8-150">Indexművelet</span><span class="sxs-lookup"><span data-stu-id="f3aa8-150">Index Operations</span></span>](http://msdn.microsoft.com/library/azure/dn798918.aspx)
* [<span data-ttu-id="f3aa8-151">A dokumentum műveletek</span><span class="sxs-lookup"><span data-stu-id="f3aa8-151">Document Operations</span></span>](http://msdn.microsoft.com/library/azure/dn800962.aspx)
* [<span data-ttu-id="f3aa8-152">Videók és oktatóanyagok Azure Search kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="f3aa8-152">Video and tutorials about Azure Search</span></span>](search-video-demo-tutorial-list.md)
* [<span data-ttu-id="f3aa8-153">Az Azure Search jellemzőalapú navigáció</span><span class="sxs-lookup"><span data-stu-id="f3aa8-153">Faceted Navigation in Azure Search</span></span>](search-faceted-navigation.md)

<!--Image references-->
[1]: ./media/search-pagination-page-layout/Pages-1-Viewing1ofNResults.PNG
[2]: ./media/search-pagination-page-layout/Pages-2-Tiled.PNG
[3]: ./media/search-pagination-page-layout/Pages-3-SortBy.png
[4]: ./media/search-pagination-page-layout/Pages-4-SortbyRelevance.png
[5]: ./media/search-pagination-page-layout/Pages-5-BuildSort.png 
