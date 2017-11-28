---
title: "A szinonimák előzetes verziójának oktatóanyaga az Azure Search szolgáltatásban | Microsoft Docs"
description: "A szinonimák előzetes verziójú szolgáltatás hozzáadása az Azure Search szolgáltatás egyik indexéhez."
services: search
manager: jhubbard
documentationcenter: 
author: HeidiSteen
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 03/31/2017
ms.author: heidist
ms.openlocfilehash: 014959ed471f796d2184f0f8ff10d15cdc8a2ec6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="synonym-preview-c-tutorial-for-azure-search"></a><span data-ttu-id="ccd83-103">Szinonima (előzetes verzió) C# oktatóanyag az Azure Search szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="ccd83-103">Synonym (preview) C# tutorial for Azure Search</span></span>

<span data-ttu-id="ccd83-104">A szinonimák bővítik a lekérdezéseket azáltal, hogy találatként kezelik a bemeneti kifejezéssel szemantikailag egyenértékűnek tekintett kifejezéseket.</span><span class="sxs-lookup"><span data-stu-id="ccd83-104">Synonyms expand a query by matching on terms considered semantically equivalent to the input term.</span></span> <span data-ttu-id="ccd83-105">Előfordulhat például, hogy azt szeretné, hogy a „kocsi” kifejezésre olyan dokumentumokat kapjon eredményül, amelyek az „autó” vagy a „jármű” kifejezéseket is tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="ccd83-105">For example, you might want "car" to match documents containing the terms "automobile" or "vehicle".</span></span>

<span data-ttu-id="ccd83-106">Az Azure Search szolgáltatásban a szinonimák meghatározása egy *szinonimatérképpel* történik, az egyenértékű kifejezéseket társító *leképezési szabályok* segítségével.</span><span class="sxs-lookup"><span data-stu-id="ccd83-106">In Azure Search, synonyms are defined in a *synonym map*, through *mapping rules* that associate equivalent terms.</span></span> <span data-ttu-id="ccd83-107">Több szinonimatérképet is létrehozhat, közzéteheti őket bármely index számára elérhető szolgáltatásszintű erőforrásként, majd hivatkozhat arra, amelyiket a mezőszinten használni kívánja.</span><span class="sxs-lookup"><span data-stu-id="ccd83-107">You can create multiple synonym maps, post them as a service-wide resource available to any index, and then reference which one to use at the field level.</span></span> <span data-ttu-id="ccd83-108">Az indexben való keresés mellett az Azure Search szolgáltatás lekérdezéskor a szinonimatérképben is keres, ha meg van határozva egy a lekérdezésben használt mezőkhöz.</span><span class="sxs-lookup"><span data-stu-id="ccd83-108">At query time, in addition to searching an index, Azure Search does a lookup in a synonym map, if one is specified on fields used in the query.</span></span>

> [!NOTE]
> <span data-ttu-id="ccd83-109">A szinonimák szolgáltatás jelenleg előzetes verzióban van, és csak a legújabb előzetes verziójú API- és SDK-verziók támogatják (api-version=2016-09-01-Preview, SDK version 4.x-preview).</span><span class="sxs-lookup"><span data-stu-id="ccd83-109">The synonyms feature is currently in preview and only supported in the latest preview API and SDK versions (api-version=2016-09-01-Preview, SDK version 4.x-preview).</span></span> <span data-ttu-id="ccd83-110">Jelenleg nincs Azure Portal-támogatás.</span><span class="sxs-lookup"><span data-stu-id="ccd83-110">There is no Azure portal support at this time.</span></span> <span data-ttu-id="ccd83-111">Mivel az előzetes verziójú API-kra nem vonatkozik SLA, és az előzetes verziójú szolgáltatások változhatnak, nem javasolt éles környezetben használni őket.</span><span class="sxs-lookup"><span data-stu-id="ccd83-111">Preview APIs are not under SLA and preview features may change, so we do not recommend using them in production applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ccd83-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ccd83-112">Prerequisites</span></span>

<span data-ttu-id="ccd83-113">Az oktatóanyag az alábbi követelményekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="ccd83-113">Tutorial requirements include the following:</span></span>

* [<span data-ttu-id="ccd83-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ccd83-114">Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="ccd83-115">Azure Search szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ccd83-115">Azure Search service</span></span>](search-create-service-portal.md)
* [<span data-ttu-id="ccd83-116">A Microsoft.Azure.Search .NET-kódtár előzetes verziója</span><span class="sxs-lookup"><span data-stu-id="ccd83-116">Preview version of Microsoft.Azure.Search .NET library</span></span>](https://aka.ms/search-sdk-preview)
* [<span data-ttu-id="ccd83-117">Az Azure Search szolgáltatás használata .NET-alkalmazásból</span><span class="sxs-lookup"><span data-stu-id="ccd83-117">How to use Azure Search from a .NET Application</span></span>](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

## <a name="overview"></a><span data-ttu-id="ccd83-118">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ccd83-118">Overview</span></span>

<span data-ttu-id="ccd83-119">Az „előtte és utána” lekérdezések a szinonimák előnyeit mutatják be.</span><span class="sxs-lookup"><span data-stu-id="ccd83-119">Before-and-after queries demonstrate the value of synonyms.</span></span> <span data-ttu-id="ccd83-120">A jelen oktatóanyagban egy mintaalkalmazást használunk, amely lekérdezéseket hajt végre és eredményeket ad vissza egy mintaindexen.</span><span class="sxs-lookup"><span data-stu-id="ccd83-120">In this tutorial, we use a sample application that executes queries and returns results on a sample index.</span></span> <span data-ttu-id="ccd83-121">A mintaalkalmazás egy „hotels” nevű kisméretű indexet hoz létre, amely két dokumentummal van feltöltve.</span><span class="sxs-lookup"><span data-stu-id="ccd83-121">The sample application creates a small index named "hotels" populated with two documents.</span></span> <span data-ttu-id="ccd83-122">Az alkalmazás keresési lekérdezéseket futtat olyan kifejezésekkel, amelyek nem szerepelnek az indexben, engedélyezi a szinonimák szolgáltatást, majd ismét kiadja ugyanazokat a kereséseket.</span><span class="sxs-lookup"><span data-stu-id="ccd83-122">The application executes search queries using terms and phrases that do not appear in the index, enables the synonyms feature, then issues the same searches again.</span></span> <span data-ttu-id="ccd83-123">Az alábbi kód az általános folyamatot mutatja be.</span><span class="sxs-lookup"><span data-stu-id="ccd83-123">The code below demonstrates the overall flow.</span></span>

```csharp
  static void Main(string[] args)
  {
      SearchServiceClient serviceClient = CreateSearchServiceClient();

      Console.WriteLine("{0}", "Cleaning up resources...\n");
      CleanupResources(serviceClient);

      Console.WriteLine("{0}", "Creating index...\n");
      CreateHotelsIndex(serviceClient);

      ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

      Console.WriteLine("{0}", "Uploading documents...\n");
      UploadDocuments(indexClient);

      ISearchIndexClient indexClientForQueries = CreateSearchIndexClient();

      RunQueriesWithNonExistentTermsInIndex(indexClientForQueries);

      Console.WriteLine("{0}", "Adding synonyms...\n");
      UploadSynonyms(serviceClient);
      EnableSynonymsInHotelsIndex(serviceClient);
      Thread.Sleep(10000); // Wait for the changes to propagate

      RunQueriesWithNonExistentTermsInIndex(indexClientForQueries);

      Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");

      Console.ReadKey();
  }
```
<span data-ttu-id="ccd83-124">A mintaindex létrehozásának és a dokumentumok feltöltésének lépései [Az Azure Search szolgáltatás használata .NET-alkalmazásból](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk) című cikkben találhatók.</span><span class="sxs-lookup"><span data-stu-id="ccd83-124">The steps to create and populate the sample index are explained in [How to use Azure Search from a .NET Application](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).</span></span>

## <a name="before-queries"></a><span data-ttu-id="ccd83-125">„Előtte” lekérdezések</span><span class="sxs-lookup"><span data-stu-id="ccd83-125">"Before" queries</span></span>

<span data-ttu-id="ccd83-126">A `RunQueriesWithNonExistentTermsInIndex` parancsban olyan keresési lekérdezéseket adunk ki, mint „five star” (ötcsillagos), „internet”, valamint „economy AND hotel” (gazdaság ÉS szálloda).</span><span class="sxs-lookup"><span data-stu-id="ccd83-126">In `RunQueriesWithNonExistentTermsInIndex`, we issue search queries with "five star", "internet", and "economy AND hotel".</span></span>
```csharp
Console.WriteLine("Search the entire index for the phrase \"five star\":\n");
results = indexClient.Documents.Search<Hotel>("\"five star\"", parameters);
WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'internet':\n");
results = indexClient.Documents.Search<Hotel>("internet", parameters);
WriteDocuments(results);

Console.WriteLine("Search the entire index for the terms 'economy' AND 'hotel':\n");
results = indexClient.Documents.Search<Hotel>("economy AND hotel", parameters);
WriteDocuments(results);
```
<span data-ttu-id="ccd83-127">A két indexelt dokumentum közül egyik sem tartalmazza a kifejezéseket, ezért az első `RunQueriesWithNonExistentTermsInIndex` parancs a következő kimenetet eredményezi.</span><span class="sxs-lookup"><span data-stu-id="ccd83-127">Neither of the two indexed documents contain the terms, so we get the following output from the first `RunQueriesWithNonExistentTermsInIndex`.</span></span>
~~~
Search the entire index for the phrase "five star":

no document matched

Search the entire index for the term 'internet':

no document matched

Search the entire index for the terms 'economy' AND 'hotel':

no document matched
~~~

## <a name="enable-synonyms"></a><span data-ttu-id="ccd83-128">Szinonimák engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ccd83-128">Enable synonyms</span></span>

<span data-ttu-id="ccd83-129">A szinonimák engedélyezése egy kétlépéses folyamat.</span><span class="sxs-lookup"><span data-stu-id="ccd83-129">Enabling synonyms is a two-step process.</span></span> <span data-ttu-id="ccd83-130">Először meghatározzuk és feltöltjük a szinonimaszabályokat, majd mezőket konfigurálunk az alkalmazásukhoz.</span><span class="sxs-lookup"><span data-stu-id="ccd83-130">We first define and upload synonym rules and then configure fields to use them.</span></span> <span data-ttu-id="ccd83-131">A folyamatot az `UploadSynonyms` és az `EnableSynonymsInHotelsIndex` vázolja.</span><span class="sxs-lookup"><span data-stu-id="ccd83-131">The process is outlined in `UploadSynonyms` and `EnableSynonymsInHotelsIndex`.</span></span>

1. <span data-ttu-id="ccd83-132">Adjon hozzá egy szinonimatérképet a keresőszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="ccd83-132">Add a synonym map to your search service.</span></span> <span data-ttu-id="ccd83-133">Az `UploadSynonyms` parancsban négy szabályt határozunk meg a 'desc-synonymmap' szinonimatérképben, és feltöltjük őket a szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="ccd83-133">In `UploadSynonyms`, we define four rules in our synonym map 'desc-synonymmap' and upload to the service.</span></span>
```csharp
    var synonymMap = new SynonymMap()
    {
        Name = "desc-synonymmap",
        Format = "solr",
        Synonyms = "hotel, motel\n
                    internet,wifi\n
                    five star=>luxury\n
                    economy,inexpensive=>budget"
    };

    serviceClient.SynonymMaps.CreateOrUpdate(synonymMap);
```
<span data-ttu-id="ccd83-134">A szinonimatérképnek meg kell felelnie a nyílt forráskódú szabványos `solr` formátumnak.</span><span class="sxs-lookup"><span data-stu-id="ccd83-134">A synonym map must conform to the open source standard `solr` format.</span></span> <span data-ttu-id="ccd83-135">A formátum ismertetését a [Szinonimák az Azure Search szolgáltatásban](search-synonyms.md) című cikk `Apache Solr synonym format` szakasza tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ccd83-135">The format is explained in [Synonyms in Azure Search](search-synonyms.md) under the section `Apache Solr synonym format`.</span></span>

2. <span data-ttu-id="ccd83-136">Konfiguráljon kereshető mezőket a szinonimatérkép indexdefinícióban történő használatához.</span><span class="sxs-lookup"><span data-stu-id="ccd83-136">Configure searchable fields to use the synonym map in the index definition.</span></span> <span data-ttu-id="ccd83-137">Az `EnableSynonymsInHotelsIndex` parancsban engedélyezzük a szinonimákat a `category` és a `tags` mezőkben úgy, hogy a `synonymMaps` tulajdonságot az újonnan feltöltött szinonimatérkép nevére állítjuk.</span><span class="sxs-lookup"><span data-stu-id="ccd83-137">In `EnableSynonymsInHotelsIndex`, we enable synonyms on two fields `category` and `tags` by setting the `synonymMaps` property to the name of the newly uploaded synonym map.</span></span>
```csharp
  Index index = serviceClient.Indexes.Get("hotels");
  index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
  index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };

  serviceClient.Indexes.CreateOrUpdate(index);
```
<span data-ttu-id="ccd83-138">Szinonimatérkép hozzáadásakor nem kell újraépíteni az indexet.</span><span class="sxs-lookup"><span data-stu-id="ccd83-138">When you add a synonym map, index rebuilds are not required.</span></span> <span data-ttu-id="ccd83-139">A szolgáltatáshoz hozzáadhat egy szinonimatérképet, majd bármely indexben módosíthatja a meglévő mezőmeghatározásokat úgy, hogy azok az új szinonimatérképet használják.</span><span class="sxs-lookup"><span data-stu-id="ccd83-139">You can add a synonym map to your service, and then amend existing field definitions in any index to use the new synonym map.</span></span> <span data-ttu-id="ccd83-140">Az új attribútumok hozzáadása nincs hatással az index elérhetőségére.</span><span class="sxs-lookup"><span data-stu-id="ccd83-140">The addition of new attributes has no impact on index availability.</span></span> <span data-ttu-id="ccd83-141">Ugyanez vonatkozik a szinonimák egy adott mező számára történő letiltására.</span><span class="sxs-lookup"><span data-stu-id="ccd83-141">The same applies in disabling synonyms for a field.</span></span> <span data-ttu-id="ccd83-142">A `synonymMaps` tulajdonságot egyszerűen átállíthatja egy üres listára.</span><span class="sxs-lookup"><span data-stu-id="ccd83-142">You can simply set the `synonymMaps` property to an empty list.</span></span>
```csharp
  index.Fields.First(f => f.Name == "category").SynonymMaps = new List<string>();
```

## <a name="after-queries"></a><span data-ttu-id="ccd83-143">„Utána” lekérdezések</span><span class="sxs-lookup"><span data-stu-id="ccd83-143">"After" queries</span></span>

<span data-ttu-id="ccd83-144">A szinonimatérkép feltöltése és az index ennek használatára való frissítése után a második `RunQueriesWithNonExistentTermsInIndex` hívás a következő kimenetet eredményezi:</span><span class="sxs-lookup"><span data-stu-id="ccd83-144">After the synonym map is uploaded and the index is updated to use the synonym map, the second `RunQueriesWithNonExistentTermsInIndex` call outputs the following:</span></span>

~~~
Search the entire index for the phrase "five star":

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the term 'internet':

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the terms 'economy' AND 'hotel':

Name: Roach Motel       Category: Budget        Tags: [motel, budget]
~~~
<span data-ttu-id="ccd83-145">Az első lekérdezés a `five star=>luxury` szabály alapján megtalálja a dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="ccd83-145">The first query finds the document from the rule `five star=>luxury`.</span></span> <span data-ttu-id="ccd83-146">A második lekérdezés kibővíti a keresést az `internet,wifi` használatával, a harmadik pedig a `hotel, motel` és az `economy,inexpensive=>budget` kifejezést is használja a dokumentumok megtalálásához.</span><span class="sxs-lookup"><span data-stu-id="ccd83-146">The second query expands the search using `internet,wifi` and the third using both `hotel, motel` and `economy,inexpensive=>budget` in finding the documents they matched.</span></span>

<span data-ttu-id="ccd83-147">A szinonimák hozzáadásával teljesen megváltozik a keresési élmény.</span><span class="sxs-lookup"><span data-stu-id="ccd83-147">Adding synonyms completely changes the search experience.</span></span> <span data-ttu-id="ccd83-148">Ebben az oktatóanyagban az eredeti lekérdezések nem hoztak használható eredményeket, bár az indexben található dokumentumok a kérdéses témakörre vonatkoztak.</span><span class="sxs-lookup"><span data-stu-id="ccd83-148">In this tutorial, the original queries failed to return meaningful results even though the documents in our index were relevant.</span></span> <span data-ttu-id="ccd83-149">A szinonimák engedélyezésével úgy bővíthetjük ki az indexet, hogy a gyakran használt kifejezéseket is tartalmazza a benne található mögöttes adatok megváltoztatása nélkül.</span><span class="sxs-lookup"><span data-stu-id="ccd83-149">By enabling synonyms, we can expand an index to include terms in common use, with no changes to underlying data in the index.</span></span>

## <a name="sample-application-source-code"></a><span data-ttu-id="ccd83-150">Mintaalkalmazás forráskódja</span><span class="sxs-lookup"><span data-stu-id="ccd83-150">Sample application source code</span></span>
<span data-ttu-id="ccd83-151">A jelen útmutatóban használt mintaalkalmazás teljes forráskódját a [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) webhelyén találja.</span><span class="sxs-lookup"><span data-stu-id="ccd83-151">You can find the full source code of the sample application used in this walk through on [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ccd83-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ccd83-152">Next steps</span></span>

* <span data-ttu-id="ccd83-153">Tekintse át [a szinonimák Azure Searchben történő használatával](search-synonyms.md) foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="ccd83-153">Review [How to use synonyms in Azure Search](search-synonyms.md)</span></span>
* <span data-ttu-id="ccd83-154">Tekintse át a [szinonimák REST API dokumentációját](https://aka.ms/rgm6rq).</span><span class="sxs-lookup"><span data-stu-id="ccd83-154">Review [Synonyms REST API documentation](https://aka.ms/rgm6rq)</span></span>
* <span data-ttu-id="ccd83-155">Nézze át a [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) és a [REST API](https://docs.microsoft.com/rest/api/searchservice/) referenciáit.</span><span class="sxs-lookup"><span data-stu-id="ccd83-155">Browse the references for the [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) and [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>
