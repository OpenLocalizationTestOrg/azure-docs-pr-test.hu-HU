---
title: "Keresés több nyelv aaaAzure |} Microsoft Docs"
description: "Az Azure Search 56 nyelvet támogat, Lucene és természetes nyelvű feldolgozása technológia a Microsoft a nyelvi elemzőkkel kihasználva."
services: search
documentationcenter: 
author: yahnoosh
manager: pablocas
editor: 
ms.assetid: 55a00b44-804d-41bb-9c96-e6ea498616f5
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/23/2017
ms.author: jlembicz
ms.openlocfilehash: 9a2e567a82ee563521c12ea320f6c484a8e73f04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a><span data-ttu-id="32482-103">Több nyelven is az Azure Search-dokumentumok index létrehozása</span><span class="sxs-lookup"><span data-stu-id="32482-103">Create an index for documents in multiple languages in Azure Search</span></span>
> [!div class="op_single_selector"]
>
> * [<span data-ttu-id="32482-104">Portál</span><span class="sxs-lookup"><span data-stu-id="32482-104">Portal</span></span>](search-language-support.md)
> * [<span data-ttu-id="32482-105">REST</span><span class="sxs-lookup"><span data-stu-id="32482-105">REST</span></span>](https://msdn.microsoft.com/library/azure/dn879793.aspx)
> * [<span data-ttu-id="32482-106">.NET</span><span class="sxs-lookup"><span data-stu-id="32482-106">.NET</span></span>](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)
>
>

<span data-ttu-id="32482-107">A nyelvi elemzőkkel unleashing hello power lehető legkönnyebben hello Indexdefiníció kereshető mező egy tulajdonságának beállításakor.</span><span class="sxs-lookup"><span data-stu-id="32482-107">Unleashing hello power of language analyzers is as easy as setting one property on a searchable field in hello index definition.</span></span> <span data-ttu-id="32482-108">Most ezt a lépést hello portálon teheti meg.</span><span class="sxs-lookup"><span data-stu-id="32482-108">Now you can do this step in hello portal.</span></span>

<span data-ttu-id="32482-109">Az alábbiakban példaként bemutató képernyőképeket láthat hello Azure portál panel az Azure Search, amelyek lehetővé teszik a felhasználók toodefine a sémát indexeli.</span><span class="sxs-lookup"><span data-stu-id="32482-109">Below are screenshots of hello Azure Portal blades for Azure Search that allow users toodefine an index schema.</span></span> <span data-ttu-id="32482-110">Ezen a panelen a felhasználók az összes hello mezők létrehozása és azok hello analyzer tulajdonság beállítása.</span><span class="sxs-lookup"><span data-stu-id="32482-110">From this blade, users can create all of hello fields and set hello analyzer property for each of them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32482-111">Csak beállíthat egy nyelvi elemzőt során mező definícióját, és a egy új index létrehozását a hello szabad vagy egy új tooan meglévő mezőindex hozzáadásakor.</span><span class="sxs-lookup"><span data-stu-id="32482-111">You can only set a language analyzer during field definition, as in when creating a new index from hello ground up, or when adding a new field tooan existing index.</span></span> <span data-ttu-id="32482-112">Ellenőrizze, hogy teljesen meg minden attribútumokat, hello analyzer hello mező létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="32482-112">Make sure you fully specify all attributes, including hello analyzer, while creating hello field.</span></span> <span data-ttu-id="32482-113">Nem lehet képes tooedit hello attribútumok, vagy hello analyzer típusának módosítása után a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="32482-113">You won't be able tooedit hello attributes or change hello analyzer type once you save your changes.</span></span>
>
>

## <a name="define-a-new-field-definition"></a><span data-ttu-id="32482-114">Új mező definíció megadása</span><span class="sxs-lookup"><span data-stu-id="32482-114">Define a new field definition</span></span>
1. <span data-ttu-id="32482-115">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) és a keresési szolgáltatás nyitott hello szolgáltatás panelre.</span><span class="sxs-lookup"><span data-stu-id="32482-115">Sign in toohello [Azure portal](https://portal.azure.com) and open hello service blade of your search service.</span></span>
2. <span data-ttu-id="32482-116">Kattintson a **index hozzáadása** hello parancsban hello szolgáltatás irányítópult toostart hello tetején sávot új index, vagy nyisson meg egy meglévő index tooset új mező hozzáadása egy analyzer tooan meglévő-index.</span><span class="sxs-lookup"><span data-stu-id="32482-116">Click **Add index** in hello command bar at hello top of hello service dashboard toostart a new index, or open an existing index tooset an analyzer on new fields you're adding tooan existing index.</span></span>
3. <span data-ttu-id="32482-117">hello mezők panel jelenik meg, hello séma hello index meghatározása lehetőségeit is ide hello Analyzer használt, egy nyelvi elemzőt kiválasztásakor.</span><span class="sxs-lookup"><span data-stu-id="32482-117">hello Fields blade appears, giving you options for defining hello schema of hello index, including hello Analyzer tab used for choosing a language analyzer.</span></span>
4. <span data-ttu-id="32482-118">A mezőket indítsa el mező definíciójának megadása hello adattípus kiválasztása és attribútumok toomark hello mezőben teljes szöveges keresési eredmények között, használható értékkorlátozás navigációs szerkezetben lekérhető, kereshető rendezhető, és a beállítása stb.</span><span class="sxs-lookup"><span data-stu-id="32482-118">In Fields, start a field definition by providing a name, choosing hello data type, and setting attributes toomark hello field as full text searchable, retrievable in search results, usable in facet navigation structures, sortable, and so forth.</span></span>
5. <span data-ttu-id="32482-119">Mielőtt továbblép a következő mező toohello, nyissa meg a hello **Analyzer** fülre.</span><span class="sxs-lookup"><span data-stu-id="32482-119">Before moving on toohello next field, open hello **Analyzer** tab.</span></span>

<span data-ttu-id="32482-120">![][1]
*egy elemző tooselect kattintson hello Analyzer lapon hello mezők panelen*</span><span class="sxs-lookup"><span data-stu-id="32482-120">![][1]
*tooselect an analyzer, click hello Analyzer tab on hello Fields blade*</span></span>

## <a name="choose-an-analyzer"></a><span data-ttu-id="32482-121">Válasszon egy elemző eszköz</span><span class="sxs-lookup"><span data-stu-id="32482-121">Choose an analyzer</span></span>
1. <span data-ttu-id="32482-122">Görgessen toofind hello mező meghatározásakor.</span><span class="sxs-lookup"><span data-stu-id="32482-122">Scroll toofind hello field you are defining.</span></span>
2. <span data-ttu-id="32482-123">Ha még nem kereshető, hello mezőt, hello jelölőnégyzet most toomark kattintson rá **kereshető**.</span><span class="sxs-lookup"><span data-stu-id="32482-123">If you haven't marked hello field as searchable, click hello checkbox now toomark it as **Searchable**.</span></span>
3. <span data-ttu-id="32482-124">Kattintson a hello Analyzer terület toodisplay hello listája elérhető elemzőkkel.</span><span class="sxs-lookup"><span data-stu-id="32482-124">Click hello Analyzer area toodisplay hello list of available analyzers.</span></span>
4. <span data-ttu-id="32482-125">Válassza ki a hello analyzer toouse szeretné.</span><span class="sxs-lookup"><span data-stu-id="32482-125">Choose hello analyzer you want toouse.</span></span>

<span data-ttu-id="32482-126">![][2]
*Válasszon ki egy támogatott hello elemzőkkel az egyes mezők*</span><span class="sxs-lookup"><span data-stu-id="32482-126">![][2]
*Select one of hello supported analyzers for each field*</span></span>

<span data-ttu-id="32482-127">Alapértelmezés szerint az összes kereshető mezőt használja hello [szabványos Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) Ez az nyelvi független.</span><span class="sxs-lookup"><span data-stu-id="32482-127">By default, all searchable fields use hello [Standard Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) which is language agnostic.</span></span> <span data-ttu-id="32482-128">tooview hello teljes listáját támogatott elemzőkkel [nyelvi támogatás az Azure Search](https://msdn.microsoft.com/library/azure/dn879793.aspx).</span><span class="sxs-lookup"><span data-stu-id="32482-128">tooview hello full list of supported analyzers, see [Language Support in Azure Search](https://msdn.microsoft.com/library/azure/dn879793.aspx).</span></span>

<span data-ttu-id="32482-129">Ha egy mező hello nyelvi elemzőt van jelölve, azt fogja használni minden egyes indexelési és keresési kérelemmel mező.</span><span class="sxs-lookup"><span data-stu-id="32482-129">Once hello language analyzer is selected for a field, it will be used with each indexing and search request for that field.</span></span> <span data-ttu-id="32482-130">Amikor a lekérdezés különböző lekérdezések használatával több mezőkkel, hello lekérdezés egymástól függetlenül általi feldolgozásának hello jobb elemzőkkel minden mező.</span><span class="sxs-lookup"><span data-stu-id="32482-130">When a query is issued against multiple fields using different analyzers, hello query will be processed independently by hello right analyzers for each field.</span></span>

<span data-ttu-id="32482-131">Sok webes és mobilalkalmazásokhoz kiszolgálására körül hello földgömb különböző nyelveken használó felhasználók.</span><span class="sxs-lookup"><span data-stu-id="32482-131">Many web and mobile applications serve users around hello globe using different languages.</span></span> <span data-ttu-id="32482-132">Már lehetséges toodefine index forgatókönyv esetén például ez hozzon létre egy mezőt, minden támogatott nyelven.</span><span class="sxs-lookup"><span data-stu-id="32482-132">It’s possible toodefine an index for a scenario like this by creating a field for each language supported.</span></span>

<span data-ttu-id="32482-133">![][3]
*Minden támogatott nyelven Leírás mezőt Indexdefiníció*</span><span class="sxs-lookup"><span data-stu-id="32482-133">![][3]
*Index definition with a description field for each language supported*</span></span>

<span data-ttu-id="32482-134">Ha hello nyelvi hello ügynök lekérdezés kiadása ismert, a keresési kérelem lehet-e a hatókörön belüli tooa adott mező hello segítségével **searchFields** lekérdezési paraméter.</span><span class="sxs-lookup"><span data-stu-id="32482-134">If hello language of hello agent issuing a query is known, a search request can be scoped tooa specific field using hello **searchFields** query parameter.</span></span> <span data-ttu-id="32482-135">hello következő lekérdezést kap, csak a Lengyel hello leírást szemben:</span><span class="sxs-lookup"><span data-stu-id="32482-135">hello following query will be issued only against hello description in Polish:</span></span>

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2016-09-01`

<span data-ttu-id="32482-136">Akkor is lekérdezheti az indexét hello portálról használatával **keresési ablak** toopaste az egy lekérdezésben hasonló toohello, amelyik fentebb is látható.</span><span class="sxs-lookup"><span data-stu-id="32482-136">You can query your index from hello portal, using **Search explorer** toopaste in a query similar toohello one shown above.</span></span> <span data-ttu-id="32482-137">Keresési ablak hello parancssáv hello szolgáltatás panelen a érhető el.</span><span class="sxs-lookup"><span data-stu-id="32482-137">Search explorer is available from hello command bar in hello service blade.</span></span> <span data-ttu-id="32482-138">Lásd: [hello portálon az Azure Search-index lekérdezése](search-explorer.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="32482-138">See [Query your Azure Search index in hello portal](search-explorer.md) for details.</span></span>

<span data-ttu-id="32482-139">Néha hello nyelvi hello ügynök lekérdezés kiadása nem ismert, mely eset hello a lekérdezés adhatók ki minden mezőkkel egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="32482-139">Sometimes hello language of hello agent issuing a query is not known, in which case hello query can be issued against all fields simultaneously.</span></span> <span data-ttu-id="32482-140">Ha szükséges, egy bizonyos nyelvi eredményez preferencia definiálható [profilok pontozási](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span><span class="sxs-lookup"><span data-stu-id="32482-140">If needed, preference for results in a certain language can be defined using [scoring profiles](https://msdn.microsoft.com/library/azure/dn798928.aspx).</span></span> <span data-ttu-id="32482-141">Hello az alábbi példában a találat hello leírásában angolul lesz pontozni magasabb relatív toomatches, lengyel és francia:</span><span class="sxs-lookup"><span data-stu-id="32482-141">In hello example below, matches found in hello description in English will be scored higher relative toomatches in Polish and French:</span></span>

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2016-09-01`

<span data-ttu-id="32482-142">Ha a .NET-fejlesztők, vegye figyelembe, hogy a nyelvi elemzőkkel hello segítségével konfigurálhatja [Azure Search .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search).</span><span class="sxs-lookup"><span data-stu-id="32482-142">If you're a .NET developer, note that you can configure language analyzers using hello [Azure Search .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search).</span></span> <span data-ttu-id="32482-143">hello legújabb kiadásának hello Microsoft nyelvi elemzőkkel is támogatását is magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="32482-143">hello latest release includes support for hello Microsoft language analyzers as well.</span></span>

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
