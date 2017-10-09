---
title: "aaaSynonyms tekintse meg az Azure Search oktatóanyag |} Microsoft Docs"
description: "Hello szinonimák előzetes funkció tooan index hozzáadása az Azure Search."
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
ms.openlocfilehash: 055c1cbafb945823a3dc4da0c522db236b1d192c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="synonym-preview-c-tutorial-for-azure-search"></a><span data-ttu-id="8dc52-103">Szinonima (előzetes verzió) C# oktatóanyag az Azure Search szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="8dc52-103">Synonym (preview) C# tutorial for Azure Search</span></span>

<span data-ttu-id="8dc52-104">Szinonimák igényei szerint figyelembe veendő szemantikailag toohello bemeneti kifejezés egyeztetésével bontsa ki a lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="8dc52-104">Synonyms expand a query by matching on terms considered semantically equivalent toohello input term.</span></span> <span data-ttu-id="8dc52-105">Például érdemes "autó" toomatch dokumentumok tartalmazó hello feltételek "autó" vagy "vehicle".</span><span class="sxs-lookup"><span data-stu-id="8dc52-105">For example, you might want "car" toomatch documents containing hello terms "automobile" or "vehicle".</span></span>

<span data-ttu-id="8dc52-106">Az Azure Search szolgáltatásban a szinonimák meghatározása egy *szinonimatérképpel* történik, az egyenértékű kifejezéseket társító *leképezési szabályok* segítségével.</span><span class="sxs-lookup"><span data-stu-id="8dc52-106">In Azure Search, synonyms are defined in a *synonym map*, through *mapping rules* that associate equivalent terms.</span></span> <span data-ttu-id="8dc52-107">Hozzon létre több szinonimát maps, elküldeni őket egy szolgáltatás szintű erőforrás elérhető tooany index, és melyik egy toouse hello mező szinten majd hivatkoznia.</span><span class="sxs-lookup"><span data-stu-id="8dc52-107">You can create multiple synonym maps, post them as a service-wide resource available tooany index, and then reference which one toouse at hello field level.</span></span> <span data-ttu-id="8dc52-108">Időben továbbá toosearching indexet, az Azure Search végzi egy keresési szinonimát térképen, ha hello lekérdezésben használt mezőket meg van adva.</span><span class="sxs-lookup"><span data-stu-id="8dc52-108">At query time, in addition toosearching an index, Azure Search does a lookup in a synonym map, if one is specified on fields used in hello query.</span></span>

> [!NOTE]
> <span data-ttu-id="8dc52-109">a szolgáltatás jelenleg a hello szinonimák villámnézetét, és csak a támogatott hello legújabb előzetes API és SDK-verzió (api-version = 2016 09-01. dátumú előnézeti, SDK verzió 4.x előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="8dc52-109">hello synonyms feature is currently in preview and only supported in hello latest preview API and SDK versions (api-version=2016-09-01-Preview, SDK version 4.x-preview).</span></span> <span data-ttu-id="8dc52-110">Jelenleg nincs Azure Portal-támogatás.</span><span class="sxs-lookup"><span data-stu-id="8dc52-110">There is no Azure portal support at this time.</span></span> <span data-ttu-id="8dc52-111">Mivel az előzetes verziójú API-kra nem vonatkozik SLA, és az előzetes verziójú szolgáltatások változhatnak, nem javasolt éles környezetben használni őket.</span><span class="sxs-lookup"><span data-stu-id="8dc52-111">Preview APIs are not under SLA and preview features may change, so we do not recommend using them in production applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8dc52-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8dc52-112">Prerequisites</span></span>

<span data-ttu-id="8dc52-113">Útmutató követelmények hello alábbiakat foglalja magába:</span><span class="sxs-lookup"><span data-stu-id="8dc52-113">Tutorial requirements include hello following:</span></span>

* [<span data-ttu-id="8dc52-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8dc52-114">Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="8dc52-115">Azure Search szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="8dc52-115">Azure Search service</span></span>](search-create-service-portal.md)
* [<span data-ttu-id="8dc52-116">A Microsoft.Azure.Search .NET-kódtár előzetes verziója</span><span class="sxs-lookup"><span data-stu-id="8dc52-116">Preview version of Microsoft.Azure.Search .NET library</span></span>](https://aka.ms/search-sdk-preview)
* [<span data-ttu-id="8dc52-117">Hogyan toouse Azure keresési .NET-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="8dc52-117">How toouse Azure Search from a .NET Application</span></span>](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

## <a name="overview"></a><span data-ttu-id="8dc52-118">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8dc52-118">Overview</span></span>

<span data-ttu-id="8dc52-119">Előtt és után lekérdezések szinonimák hello értékének bemutatása.</span><span class="sxs-lookup"><span data-stu-id="8dc52-119">Before-and-after queries demonstrate hello value of synonyms.</span></span> <span data-ttu-id="8dc52-120">A jelen oktatóanyagban egy mintaalkalmazást használunk, amely lekérdezéseket hajt végre és eredményeket ad vissza egy mintaindexen.</span><span class="sxs-lookup"><span data-stu-id="8dc52-120">In this tutorial, we use a sample application that executes queries and returns results on a sample index.</span></span> <span data-ttu-id="8dc52-121">hello mintaalkalmazás két dokumentumok feltöltve a "hotels" nevű kis index létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8dc52-121">hello sample application creates a small index named "hotels" populated with two documents.</span></span> <span data-ttu-id="8dc52-122">hello alkalmazás végrehajtja a keresési lekérdezések használati kifejezések, amelyek nem szerepelnek a hello index, lehetővé teszi, hogy hello szinonimák szolgáltatást, majd problémák hello azonos keresések újra.</span><span class="sxs-lookup"><span data-stu-id="8dc52-122">hello application executes search queries using terms and phrases that do not appear in hello index, enables hello synonyms feature, then issues hello same searches again.</span></span> <span data-ttu-id="8dc52-123">az alábbi hello kód bemutatja, hello általános folyamata.</span><span class="sxs-lookup"><span data-stu-id="8dc52-123">hello code below demonstrates hello overall flow.</span></span>

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
      Thread.Sleep(10000); // Wait for hello changes toopropagate

      RunQueriesWithNonExistentTermsInIndex(indexClientForQueries);

      Console.WriteLine("{0}", "Complete.  Press any key tooend application...\n");

      Console.ReadKey();
  }
```
<span data-ttu-id="8dc52-124">hello lépéseket toocreate és hello minta index feltöltése magyarázata [hogyan toouse Azure keresési .NET-alkalmazás](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).</span><span class="sxs-lookup"><span data-stu-id="8dc52-124">hello steps toocreate and populate hello sample index are explained in [How toouse Azure Search from a .NET Application](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).</span></span>

## <a name="before-queries"></a><span data-ttu-id="8dc52-125">„Előtte” lekérdezések</span><span class="sxs-lookup"><span data-stu-id="8dc52-125">"Before" queries</span></span>

<span data-ttu-id="8dc52-126">A `RunQueriesWithNonExistentTermsInIndex` parancsban olyan keresési lekérdezéseket adunk ki, mint „five star” (ötcsillagos), „internet”, valamint „economy AND hotel” (gazdaság ÉS szálloda).</span><span class="sxs-lookup"><span data-stu-id="8dc52-126">In `RunQueriesWithNonExistentTermsInIndex`, we issue search queries with "five star", "internet", and "economy AND hotel".</span></span>
```csharp
Console.WriteLine("Search hello entire index for hello phrase \"five star\":\n");
results = indexClient.Documents.Search<Hotel>("\"five star\"", parameters);
WriteDocuments(results);

Console.WriteLine("Search hello entire index for hello term 'internet':\n");
results = indexClient.Documents.Search<Hotel>("internet", parameters);
WriteDocuments(results);

Console.WriteLine("Search hello entire index for hello terms 'economy' AND 'hotel':\n");
results = indexClient.Documents.Search<Hotel>("economy AND hotel", parameters);
WriteDocuments(results);
```
<span data-ttu-id="8dc52-127">Hello feltételeket, hello két indexelt dokumentumok egyike sem tartalmaz, így azt először kimenetét hello hello következő `RunQueriesWithNonExistentTermsInIndex`.</span><span class="sxs-lookup"><span data-stu-id="8dc52-127">Neither of hello two indexed documents contain hello terms, so we get hello following output from hello first `RunQueriesWithNonExistentTermsInIndex`.</span></span>
~~~
Search hello entire index for hello phrase "five star":

no document matched

Search hello entire index for hello term 'internet':

no document matched

Search hello entire index for hello terms 'economy' AND 'hotel':

no document matched
~~~

## <a name="enable-synonyms"></a><span data-ttu-id="8dc52-128">Szinonimák engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8dc52-128">Enable synonyms</span></span>

<span data-ttu-id="8dc52-129">A szinonimák engedélyezése egy kétlépéses folyamat.</span><span class="sxs-lookup"><span data-stu-id="8dc52-129">Enabling synonyms is a two-step process.</span></span> <span data-ttu-id="8dc52-130">Azt először határozza meg és szinonimát szabályok feltölteni, majd konfigurálja a mezők toouse őket.</span><span class="sxs-lookup"><span data-stu-id="8dc52-130">We first define and upload synonym rules and then configure fields toouse them.</span></span> <span data-ttu-id="8dc52-131">hello folyamatot szemlélteti az `UploadSynonyms` és `EnableSynonymsInHotelsIndex`.</span><span class="sxs-lookup"><span data-stu-id="8dc52-131">hello process is outlined in `UploadSynonyms` and `EnableSynonymsInHotelsIndex`.</span></span>

1. <span data-ttu-id="8dc52-132">Adjon hozzá egy szinonima térkép tooyour keresési szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8dc52-132">Add a synonym map tooyour search service.</span></span> <span data-ttu-id="8dc52-133">A `UploadSynonyms`, azt a szinonima leképezés "desc-synonymmap" négy szabályokat definiálhat, és töltse fel a toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8dc52-133">In `UploadSynonyms`, we define four rules in our synonym map 'desc-synonymmap' and upload toohello service.</span></span>
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
<span data-ttu-id="8dc52-134">Egy szinonima térkép meg kell felelnie toohello nyílt forráskódú standard `solr` formátumban.</span><span class="sxs-lookup"><span data-stu-id="8dc52-134">A synonym map must conform toohello open source standard `solr` format.</span></span> <span data-ttu-id="8dc52-135">hello formátum esetén, tekintse meg a [az Azure Search szinonimák](search-synonyms.md) hello szakaszban `Apache Solr synonym format`.</span><span class="sxs-lookup"><span data-stu-id="8dc52-135">hello format is explained in [Synonyms in Azure Search](search-synonyms.md) under hello section `Apache Solr synonym format`.</span></span>

2. <span data-ttu-id="8dc52-136">Konfigurálja a kereshető mezők toouse hello szinonimát térkép hello index definícióját.</span><span class="sxs-lookup"><span data-stu-id="8dc52-136">Configure searchable fields toouse hello synonym map in hello index definition.</span></span> <span data-ttu-id="8dc52-137">A `EnableSynonymsInHotelsIndex`, engedélyezzük a két mező szinonimák `category` és `tags` által hello beállítása `synonymMaps` toohello tulajdonságnevének hello újonnan feltöltött szinonimát leképezés.</span><span class="sxs-lookup"><span data-stu-id="8dc52-137">In `EnableSynonymsInHotelsIndex`, we enable synonyms on two fields `category` and `tags` by setting hello `synonymMaps` property toohello name of hello newly uploaded synonym map.</span></span>
```csharp
  Index index = serviceClient.Indexes.Get("hotels");
  index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
  index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };

  serviceClient.Indexes.CreateOrUpdate(index);
```
<span data-ttu-id="8dc52-138">Szinonimatérkép hozzáadásakor nem kell újraépíteni az indexet.</span><span class="sxs-lookup"><span data-stu-id="8dc52-138">When you add a synonym map, index rebuilds are not required.</span></span> <span data-ttu-id="8dc52-139">Térkép szinonimát tooyour szolgáltatás hozzáadása, és majd módosíthatja bármely index toouse hello új szinonimát leképezés létező mező meghatározásokat.</span><span class="sxs-lookup"><span data-stu-id="8dc52-139">You can add a synonym map tooyour service, and then amend existing field definitions in any index toouse hello new synonym map.</span></span> <span data-ttu-id="8dc52-140">hello hozzáadása új attribútumok nem hatást gyakorol index rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="8dc52-140">hello addition of new attributes has no impact on index availability.</span></span> <span data-ttu-id="8dc52-141">Ugyanez vonatkozik a szinonimák mező letiltása hello.</span><span class="sxs-lookup"><span data-stu-id="8dc52-141">hello same applies in disabling synonyms for a field.</span></span> <span data-ttu-id="8dc52-142">Egyszerűen beállítható hello `synonymMaps` tulajdonság tooan listája üres.</span><span class="sxs-lookup"><span data-stu-id="8dc52-142">You can simply set hello `synonymMaps` property tooan empty list.</span></span>
```csharp
  index.Fields.First(f => f.Name == "category").SynonymMaps = new List<string>();
```

## <a name="after-queries"></a><span data-ttu-id="8dc52-143">„Utána” lekérdezések</span><span class="sxs-lookup"><span data-stu-id="8dc52-143">"After" queries</span></span>

<span data-ttu-id="8dc52-144">Után hello szinonimát térkép tölt fel, és hello index frissített toouse hello szinonimát térkép, második hello `RunQueriesWithNonExistentTermsInIndex` hívás hello következő kimenete:</span><span class="sxs-lookup"><span data-stu-id="8dc52-144">After hello synonym map is uploaded and hello index is updated toouse hello synonym map, hello second `RunQueriesWithNonExistentTermsInIndex` call outputs hello following:</span></span>

~~~
Search hello entire index for hello phrase "five star":

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search hello entire index for hello term 'internet':

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search hello entire index for hello terms 'economy' AND 'hotel':

Name: Roach Motel       Category: Budget        Tags: [motel, budget]
~~~
<span data-ttu-id="8dc52-145">a lekérdezés első hello talál hello hello szabály dokumentum `five star=>luxury`.</span><span class="sxs-lookup"><span data-stu-id="8dc52-145">hello first query finds hello document from hello rule `five star=>luxury`.</span></span> <span data-ttu-id="8dc52-146">hello második lekérdezés kibontása hello keresési használatával `internet,wifi` és harmadik használatával is hello `hotel, motel` és `economy,inexpensive=>budget` hello keresését az egyező.</span><span class="sxs-lookup"><span data-stu-id="8dc52-146">hello second query expands hello search using `internet,wifi` and hello third using both `hotel, motel` and `economy,inexpensive=>budget` in finding hello documents they matched.</span></span>

<span data-ttu-id="8dc52-147">Szinonimák teljesen hozzáadása megváltoztatja a hello keresési élményt biztosít.</span><span class="sxs-lookup"><span data-stu-id="8dc52-147">Adding synonyms completely changes hello search experience.</span></span> <span data-ttu-id="8dc52-148">Ebben az oktatóanyagban hello eredeti lekérdezés tooreturn jelentéssel bíró eredmények nem, annak ellenére, hogy az indexben hello dokumentumok volt megfelelő.</span><span class="sxs-lookup"><span data-stu-id="8dc52-148">In this tutorial, hello original queries failed tooreturn meaningful results even though hello documents in our index were relevant.</span></span> <span data-ttu-id="8dc52-149">Azáltal, hogy a szinonimák, azt egy index tooinclude feltételek közös használatához hello index módosítások toounderlying adatot nem tartalmazó bővítheti.</span><span class="sxs-lookup"><span data-stu-id="8dc52-149">By enabling synonyms, we can expand an index tooinclude terms in common use, with no changes toounderlying data in hello index.</span></span>

## <a name="sample-application-source-code"></a><span data-ttu-id="8dc52-150">Mintaalkalmazás forráskódja</span><span class="sxs-lookup"><span data-stu-id="8dc52-150">Sample application source code</span></span>
<span data-ttu-id="8dc52-151">Hello teljes forráskód a lépésein végighaladva a használt hello mintaalkalmazásnak található [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).</span><span class="sxs-lookup"><span data-stu-id="8dc52-151">You can find hello full source code of hello sample application used in this walk through on [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8dc52-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8dc52-152">Next steps</span></span>

* <span data-ttu-id="8dc52-153">Felülvizsgálati [hogyan toouse szinonimák az Azure Search](search-synonyms.md)</span><span class="sxs-lookup"><span data-stu-id="8dc52-153">Review [How toouse synonyms in Azure Search](search-synonyms.md)</span></span>
* <span data-ttu-id="8dc52-154">Tekintse át a [szinonimák REST API dokumentációját](https://aka.ms/rgm6rq).</span><span class="sxs-lookup"><span data-stu-id="8dc52-154">Review [Synonyms REST API documentation](https://aka.ms/rgm6rq)</span></span>
* <span data-ttu-id="8dc52-155">Keresse meg a hivatkozásokat hello hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) és [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="8dc52-155">Browse hello references for hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) and [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>
