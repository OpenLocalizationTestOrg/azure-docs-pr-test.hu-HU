---
title: "AAA \"lekérdezheti az indexét (.NET API - Azure Search) |} Microsoft dokumentumok\""
description: "Hozza létre az Azure search keresési lekérdezés, és használja a keresési paraméterek toofilter és rendezési keresési eredmények."
services: search
manager: jhubbard
documentationcenter: 
author: brjohnstmsft
ms.assetid: 12c3efba-ea99-4187-9d2d-f63b5ec7040d
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/19/2017
ms.author: brjohnst
ms.openlocfilehash: 8b3ba1cd1270aad038fb48d9053fcff35d243e13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="query-your-azure-search-index-using-hello-net-sdk"></a><span data-ttu-id="855d0-103">Hello .NET SDK használatával az Azure Search-index lekérdezése</span><span class="sxs-lookup"><span data-stu-id="855d0-103">Query your Azure Search index using hello .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="855d0-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="855d0-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="855d0-105">Portál</span><span class="sxs-lookup"><span data-stu-id="855d0-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="855d0-106">.NET</span><span class="sxs-lookup"><span data-stu-id="855d0-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="855d0-107">REST</span><span class="sxs-lookup"><span data-stu-id="855d0-107">REST</span></span>](search-query-rest-api.md)
> 
> 

<span data-ttu-id="855d0-108">Ez a cikk bemutatja, hogyan egy index használatával tooquery hello [Azure Search .NET SDK](https://aka.ms/search-sdk).</span><span class="sxs-lookup"><span data-stu-id="855d0-108">This article will show you how tooquery an index using hello [Azure Search .NET SDK](https://aka.ms/search-sdk).</span></span>

<span data-ttu-id="855d0-109">A bemutató elindítása előtt [létre kell hoznia egy Azure Search-indexet](search-what-is-an-index.md), majd [fel kell töltenie azt adatokkal](search-what-is-data-import.md).</span><span class="sxs-lookup"><span data-stu-id="855d0-109">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md) and [populated it with data](search-what-is-data-import.md).</span></span>

> [!NOTE]
> <span data-ttu-id="855d0-110">A cikkben szereplő összes példakód C# nyelven van megírva.</span><span class="sxs-lookup"><span data-stu-id="855d0-110">All sample code in this article is written in C#.</span></span> <span data-ttu-id="855d0-111">Hello teljes forráskód található [a Githubon](http://aka.ms/search-dotnet-howto).</span><span class="sxs-lookup"><span data-stu-id="855d0-111">You can find hello full source code [on GitHub](http://aka.ms/search-dotnet-howto).</span></span> <span data-ttu-id="855d0-112">Hello is olvashat [Azure Search .NET SDK](search-howto-dotnet-sdk.md) egy részletesebb lépésein végighaladva hello mintakódot az.</span><span class="sxs-lookup"><span data-stu-id="855d0-112">You can also read about hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) for a more detailed walk through of hello sample code.</span></span>

## <a name="identify-your-azure-search-services-query-api-key"></a><span data-ttu-id="855d0-113">Azonosítsa az Azure Search szolgáltatás lekérdezési API-kulcsát</span><span class="sxs-lookup"><span data-stu-id="855d0-113">Identify your Azure Search service's query api-key</span></span>
<span data-ttu-id="855d0-114">Most, hogy létrehozta az Azure Search-index, áll majdnem kész tooissue lekérdezések hello .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="855d0-114">Now that you have created an Azure Search index, you are almost ready tooissue queries using hello .NET SDK.</span></span> <span data-ttu-id="855d0-115">Először szüksége lesz egy hello lekérdezés api-kulcs lett létrehozva, tooobtain hello keresőszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="855d0-115">First, you will need tooobtain one of hello query api-keys that was generated for hello search service you provisioned.</span></span> <span data-ttu-id="855d0-116">hello .NET SDK minden kérelem tooyour szolgáltatás elküld az api-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="855d0-116">hello .NET SDK will send this api-key on every request tooyour service.</span></span> <span data-ttu-id="855d0-117">Érvényes kulcs birtokában létesít megbízhatósági, egy kérelem alapon hello küldő hello kérelem és a kezelő hello szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="855d0-117">Having a valid key establishes trust, on a per request basis, between hello application sending hello request and hello service that handles it.</span></span>

1. <span data-ttu-id="855d0-118">toofind a szolgáltatás api-kulcsokat bejelentkezhet toohello [Azure-portálon](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="855d0-118">toofind your service's api-keys you can sign in toohello [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="855d0-119">Nyissa meg tooyour Azure Search szolgáltatás paneljét</span><span class="sxs-lookup"><span data-stu-id="855d0-119">Go tooyour Azure Search service's blade</span></span>
3. <span data-ttu-id="855d0-120">Kattintson a hello "Kulcsok" ikonra</span><span class="sxs-lookup"><span data-stu-id="855d0-120">Click on hello "Keys" icon</span></span>

<span data-ttu-id="855d0-121">A szolgáltatás *rendszergazdai kulcsokkal* és *lekérdezési kulcsokkal* fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="855d0-121">Your service will have *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="855d0-122">Az elsődleges és másodlagos *adminisztrációs kulcsok* teljes körű tooall műveleteket, köztük a hello képességét toomanage hello szolgáltatást biztosítania hozzon létre, és törölje az indexek, az indexelők és az adatforrások.</span><span class="sxs-lookup"><span data-stu-id="855d0-122">Your primary and secondary *admin keys* grant full rights tooall operations, including hello ability toomanage hello service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="855d0-123">Két kulcs van, hogy a Folytatás toouse hello másodlagos kulcsát. Ha úgy dönt, hogy tooregenerate hello elsődleges kulcs, és fordítva.</span><span class="sxs-lookup"><span data-stu-id="855d0-123">There are two keys so that you can continue toouse hello secondary key if you decide tooregenerate hello primary key, and vice-versa.</span></span>
* <span data-ttu-id="855d0-124">A *lekérdezési kulcsok* adjon olvasási hozzáférést tooindexes és a dokumentumok és keresési kérelmeket kibocsátó általában elosztott tooclient alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="855d0-124">Your *query keys* grant read-only access tooindexes and documents, and are typically distributed tooclient applications that issue search requests.</span></span>

<span data-ttu-id="855d0-125">Az index lekérdezése hello alkalmazásában a lekérdezési kulcsok egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="855d0-125">For hello purposes of querying an index, you can use one of your query keys.</span></span> <span data-ttu-id="855d0-126">Az adminisztrációs kulcsok is használható a lekérdezések, de egy lekérdezési kulcsot kell használni az alkalmazás kódjában, az alábbi módon ez jobban hello [legalacsonyabb jogosultsági szint elve](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span><span class="sxs-lookup"><span data-stu-id="855d0-126">Your admin keys can also be used for queries, but you should use a query key in your application code as this better follows hello [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span></span>

## <a name="create-an-instance-of-hello-searchindexclient-class"></a><span data-ttu-id="855d0-127">Hello SearchIndexClient osztály példányának létrehozása</span><span class="sxs-lookup"><span data-stu-id="855d0-127">Create an instance of hello SearchIndexClient class</span></span>
<span data-ttu-id="855d0-128">az Azure Search .NET SDK hello lekérdezések tooissue, szüksége lesz toocreate hello példányának `SearchIndexClient` osztály.</span><span class="sxs-lookup"><span data-stu-id="855d0-128">tooissue queries with hello Azure Search .NET SDK, you will need toocreate an instance of hello `SearchIndexClient` class.</span></span> <span data-ttu-id="855d0-129">Ez az osztály több konstruktorral rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="855d0-129">This class has several constructors.</span></span> <span data-ttu-id="855d0-130">hello kívánt időt vesz igénybe, a keresőszolgáltatása nevét, az index neve, és egy `SearchCredentials` objektumot használ paraméterként.</span><span class="sxs-lookup"><span data-stu-id="855d0-130">hello one you want takes your search service name, index name, and a `SearchCredentials` object as parameters.</span></span> <span data-ttu-id="855d0-131">A `SearchCredentials` becsomagolja az API-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="855d0-131">`SearchCredentials` wraps your api-key.</span></span>

<span data-ttu-id="855d0-132">az alábbi hello kód létrehoz egy új `SearchIndexClient` hello "Hotels nevű" index (létrehozott [hello .NET SDK használatával Azure Search-index létrehozása](search-create-index-dotnet.md)) hello keresőszolgáltatása nevét és api-kulcs hello alkalmazás config tárolt értékekkel fájl (`appsettings.json` hello hello esetében [mintaalkalmazás](http://aka.ms/search-dotnet-howto)):</span><span class="sxs-lookup"><span data-stu-id="855d0-132">hello code below creates a new `SearchIndexClient` for hello "hotels" index (created in [Create an Azure Search index using hello .NET SDK](search-create-index-dotnet.md)) using values for hello search service name and api-key that are stored in hello application's config file (`appsettings.json` in hello case of hello [sample application](http://aka.ms/search-dotnet-howto)):</span></span>

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

<span data-ttu-id="855d0-133">A `SearchIndexClient` rendelkezik egy `Documents` tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="855d0-133">`SearchIndexClient` has a `Documents` property.</span></span> <span data-ttu-id="855d0-134">Ez a tulajdonság biztosít minden hello tooquery Azure Search-indexek szükséges módszereket.</span><span class="sxs-lookup"><span data-stu-id="855d0-134">This property provides all hello methods you need tooquery Azure Search indexes.</span></span>

## <a name="query-your-index"></a><span data-ttu-id="855d0-135">Az index lekérdezése</span><span class="sxs-lookup"><span data-stu-id="855d0-135">Query your index</span></span>
<span data-ttu-id="855d0-136">A helyettesítő hello .NET SDK legegyszerűbb hívó hello `Documents.Search` metódust a `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="855d0-136">Searching with hello .NET SDK is as simple as calling hello `Documents.Search` method on your `SearchIndexClient`.</span></span> <span data-ttu-id="855d0-137">Ez a módszer veszi néhány paraméterekkel, többek között a keresési szöveget: hello, valamint egy `SearchParameters` objektum, amely használt toofurther finomítása hello lekérdezés lehet.</span><span class="sxs-lookup"><span data-stu-id="855d0-137">This method takes a few parameters, including hello search text, along with a `SearchParameters` object that can be used toofurther refine hello query.</span></span>

#### <a name="types-of-queries"></a><span data-ttu-id="855d0-138">A lekérdezések típusai</span><span class="sxs-lookup"><span data-stu-id="855d0-138">Types of Queries</span></span>
<span data-ttu-id="855d0-139">hello két fő [típusok lekérdezése](search-query-overview.md#types-of-queries) használandó vannak `search` és `filter`.</span><span class="sxs-lookup"><span data-stu-id="855d0-139">hello two main [query types](search-query-overview.md#types-of-queries) you will use are `search` and `filter`.</span></span> <span data-ttu-id="855d0-140">A `search` lekérdezés egy vagy több kifejezésre keres rá az index összes *searchable* (kereshető) mezőjében.</span><span class="sxs-lookup"><span data-stu-id="855d0-140">A `search` query searches for one or more terms in all *searchable* fields in your index.</span></span> <span data-ttu-id="855d0-141">A `filter` lekérdezés egy logikai kifejezés kiértékelését végzi el az index összes *filterable* (szűrhető) mezőjén.</span><span class="sxs-lookup"><span data-stu-id="855d0-141">A `filter` query evaluates a boolean expression over all *filterable* fields in an index.</span></span>

<span data-ttu-id="855d0-142">Keresés és a szűrők készül hello segítségével `Documents.Search` metódust.</span><span class="sxs-lookup"><span data-stu-id="855d0-142">Both searches and filters are performed using hello `Documents.Search` method.</span></span> <span data-ttu-id="855d0-143">Keresési lekérdezés átadhatók a hello `searchText` paraméter, miközben egy kifejezést a hello adhatók át `Filter` hello tulajdonságának `SearchParameters` osztály.</span><span class="sxs-lookup"><span data-stu-id="855d0-143">A search query can be passed in hello `searchText` parameter, while a filter expression can be passed in hello `Filter` property of hello `SearchParameters` class.</span></span> <span data-ttu-id="855d0-144">toofilter nélkül keres, csak adja át `"*"` a hello `searchText` paraméter.</span><span class="sxs-lookup"><span data-stu-id="855d0-144">toofilter without searching, just pass `"*"` for hello `searchText` parameter.</span></span> <span data-ttu-id="855d0-145">toosearch szűrés nélkül ne változtassa meg hello `Filter` tulajdonság beállított, vagy nem adjon át egy `SearchParameters` minden példány.</span><span class="sxs-lookup"><span data-stu-id="855d0-145">toosearch without filtering, just leave hello `Filter` property unset, or do not pass in a `SearchParameters` instance at all.</span></span>

#### <a name="example-queries"></a><span data-ttu-id="855d0-146">Példa a lekérdezésekre</span><span class="sxs-lookup"><span data-stu-id="855d0-146">Example Queries</span></span>
<span data-ttu-id="855d0-147">a következő példakód hello látható többféleképpen tooquery hello "Hotels"nevű index definiált [hello .NET SDK használatával Azure Search-index létrehozása](search-create-index-dotnet.md#DefineIndex).</span><span class="sxs-lookup"><span data-stu-id="855d0-147">hello following sample code shows a few different ways tooquery hello "hotels" index defined in [Create an Azure Search index using hello .NET SDK](search-create-index-dotnet.md#DefineIndex).</span></span> <span data-ttu-id="855d0-148">Ne feledje, hogy ad vissza, amelyben hello keresési eredmények hello dokumentumok hello példányai `Hotel` osztályt, amely lett definiálva [adatok importálása az Azure Search használatával hello .NET SDK](search-import-data-dotnet.md#HotelClass).</span><span class="sxs-lookup"><span data-stu-id="855d0-148">Note that hello documents returned with hello search results are instances of hello `Hotel` class, which was defined in [Data Import in Azure Search using hello .NET SDK](search-import-data-dotnet.md#HotelClass).</span></span> <span data-ttu-id="855d0-149">mintakód hello használ egy `WriteDocuments` metódus toooutput hello keresési eredmények toohello konzol.</span><span class="sxs-lookup"><span data-stu-id="855d0-149">hello sample code makes use of a `WriteDocuments` method toooutput hello search results toohello console.</span></span> <span data-ttu-id="855d0-150">Ez a módszer hello a következő szakaszban ismertetett.</span><span class="sxs-lookup"><span data-stu-id="855d0-150">This method is described in hello next section.</span></span>

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search hello entire index for hello term 'budget' and return only hello hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter toohello index toofind hotels cheaper than $150 per night, ");
Console.WriteLine("and return hello hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search hello entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take hello top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search hello entire index for hello term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="handle-search-results"></a><span data-ttu-id="855d0-151">A keresési eredmények kezelése</span><span class="sxs-lookup"><span data-stu-id="855d0-151">Handle search results</span></span>
<span data-ttu-id="855d0-152">Hello `Documents.Search` metódus értéket ad vissza egy `DocumentSearchResult` hello hello lekérdezés eredményeit tartalmazó objektum.</span><span class="sxs-lookup"><span data-stu-id="855d0-152">hello `Documents.Search` method returns a `DocumentSearchResult` object that contains hello results of hello query.</span></span> <span data-ttu-id="855d0-153">hello példa az előző szakaszban hello használt a hívott metódus `WriteDocuments` toooutput hello keresési eredmények toohello konzol:</span><span class="sxs-lookup"><span data-stu-id="855d0-153">hello example in hello previous section used a method called `WriteDocuments` toooutput hello search results toohello console:</span></span>

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

<span data-ttu-id="855d0-154">Ez mit hello eredmények néz ki, az előző szakaszban hello hello lekérdezésekhez feltételezve hello "Hotels"nevű index példaadatok hello van feltöltve [adatok importálása az Azure Search használatával hello .NET SDK](search-import-data-dotnet.md):</span><span class="sxs-lookup"><span data-stu-id="855d0-154">Here is what hello results look like for hello queries in hello previous section, assuming hello "hotels" index is populated with hello sample data in [Data Import in Azure Search using hello .NET SDK](search-import-data-dotnet.md):</span></span>

```
Search hello entire index for hello term 'budget' and return only hello hotelName field:

Name: Roach Motel

Apply a filter toohello index toofind hotels cheaper than $150 per night, and return hello hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close tootown hall and hello river

Search hello entire index, order by a specific field (lastRenovationDate) in descending order, take hello top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search hello entire index for hello term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
```

<span data-ttu-id="855d0-155">a fenti hello mintakód hello konzol toooutput keresési eredményeket használja.</span><span class="sxs-lookup"><span data-stu-id="855d0-155">hello sample code above uses hello console toooutput search results.</span></span> <span data-ttu-id="855d0-156">A saját alkalmazásban toodisplay keresési eredmények hasonlóképpen kell.</span><span class="sxs-lookup"><span data-stu-id="855d0-156">You will likewise need toodisplay search results in your own application.</span></span> <span data-ttu-id="855d0-157">Lásd: [ezt a mintát a Githubon](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) hogyan toorender keresés eredménye egy ASP.NET MVC-alapú webalkalmazás példát.</span><span class="sxs-lookup"><span data-stu-id="855d0-157">See [this sample on GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) for an example of how toorender search results in an ASP.NET MVC-based web application.</span></span>

