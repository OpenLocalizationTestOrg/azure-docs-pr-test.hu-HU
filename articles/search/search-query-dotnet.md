---
title: "Index lekérdezése (.NET API – Azure Search) | Microsoft Docs"
description: "Létrehozhat keresési lekérdezést az Azure Search szolgáltatásban, a keresési eredmények szűrését és rendezését pedig keresési paraméterek használatával végezheti el."
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
ms.openlocfilehash: 52bd0fd4cf70401dcf881c7f28d5cd91397bb059
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="query-your-azure-search-index-using-the-net-sdk"></a><span data-ttu-id="f8e0b-103">Az Azure Search-index lekérdezése a .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="f8e0b-103">Query your Azure Search index using the .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f8e0b-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f8e0b-104">Overview</span></span>](search-query-overview.md)
> * [<span data-ttu-id="f8e0b-105">Portál</span><span class="sxs-lookup"><span data-stu-id="f8e0b-105">Portal</span></span>](search-explorer.md)
> * [<span data-ttu-id="f8e0b-106">.NET</span><span class="sxs-lookup"><span data-stu-id="f8e0b-106">.NET</span></span>](search-query-dotnet.md)
> * [<span data-ttu-id="f8e0b-107">REST</span><span class="sxs-lookup"><span data-stu-id="f8e0b-107">REST</span></span>](search-query-rest-api.md)
> 
> 

<span data-ttu-id="f8e0b-108">Ebből a cikkből megtudhatja, hogyan történik egy index lekérdezése az [Azure Search .NET SDK](https://aka.ms/search-sdk) használatával.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-108">This article will show you how to query an index using the [Azure Search .NET SDK](https://aka.ms/search-sdk).</span></span>

<span data-ttu-id="f8e0b-109">A bemutató elindítása előtt [létre kell hoznia egy Azure Search-indexet](search-what-is-an-index.md), majd [fel kell töltenie azt adatokkal](search-what-is-data-import.md).</span><span class="sxs-lookup"><span data-stu-id="f8e0b-109">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md) and [populated it with data](search-what-is-data-import.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f8e0b-110">A cikkben szereplő összes példakód C# nyelven van megírva.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-110">All sample code in this article is written in C#.</span></span> <span data-ttu-id="f8e0b-111">A teljes forráskódot a [GitHub](http://aka.ms/search-dotnet-howto) webhelyén találja.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-111">You can find the full source code [on GitHub](http://aka.ms/search-dotnet-howto).</span></span> <span data-ttu-id="f8e0b-112">Az [Azure Search .NET SDK](search-howto-dotnet-sdk.md) leírásában részletesebb útmutatást kaphat a példakóddal kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-112">You can also read about the [Azure Search .NET SDK](search-howto-dotnet-sdk.md) for a more detailed walk through of the sample code.</span></span>

## <a name="identify-your-azure-search-services-query-api-key"></a><span data-ttu-id="f8e0b-113">Azonosítsa az Azure Search szolgáltatás lekérdezési API-kulcsát</span><span class="sxs-lookup"><span data-stu-id="f8e0b-113">Identify your Azure Search service's query api-key</span></span>
<span data-ttu-id="f8e0b-114">Az Azure Search-index létrehozását követően most már csaknem készen áll lekérdezések kiadására a .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-114">Now that you have created an Azure Search index, you are almost ready to issue queries using the .NET SDK.</span></span> <span data-ttu-id="f8e0b-115">Először is az Ön által üzembe helyezett Search szolgáltatás számára létrehozott lekérdezési API-kulcsok egyikére lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-115">First, you will need to obtain one of the query api-keys that was generated for the search service you provisioned.</span></span> <span data-ttu-id="f8e0b-116">A .NET SDK ezt az API-kulcsot minden szolgáltatáskérés alkalmával elküldi.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-116">The .NET SDK will send this api-key on every request to your service.</span></span> <span data-ttu-id="f8e0b-117">Érvényes kulcs birtokában kérelmenként létesíthető megbízhatósági kapcsolat a kérést küldő alkalmazás és az azt kezelő szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-117">Having a valid key establishes trust, on a per request basis, between the application sending the request and the service that handles it.</span></span>

1. <span data-ttu-id="f8e0b-118">A szolgáltatás API-kulcsainak megkereséséhez bejelentkezhet az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f8e0b-118">To find your service's api-keys you can sign in to the [Azure portal](https://portal.azure.com/)</span></span>
2. <span data-ttu-id="f8e0b-119">Nyissa meg az Azure Search szolgáltatáspaneljét</span><span class="sxs-lookup"><span data-stu-id="f8e0b-119">Go to your Azure Search service's blade</span></span>
3. <span data-ttu-id="f8e0b-120">Kattintson a „Kulcsok” ikonra</span><span class="sxs-lookup"><span data-stu-id="f8e0b-120">Click on the "Keys" icon</span></span>

<span data-ttu-id="f8e0b-121">A szolgáltatás *rendszergazdai kulcsokkal* és *lekérdezési kulcsokkal* fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-121">Your service will have *admin keys* and *query keys*.</span></span>

* <span data-ttu-id="f8e0b-122">Az elsődleges és másodlagos *rendszergazdai kulcsok* teljes jogosultságot biztosítanak az összes művelethez, beleértve a szolgáltatás felügyeletének, valamint az indexek, indexelők és adatforrások létrehozásának és törlésének képességét.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-122">Your primary and secondary *admin keys* grant full rights to all operations, including the ability to manage the service, create and delete indexes, indexers, and data sources.</span></span> <span data-ttu-id="f8e0b-123">Két kulcs létezi, tehát ha az elsődleges kulcs újbóli létrehozása mellett dönt, a másodlagos kulcsot továbbra is használhatja (ez fordítva is igaz).</span><span class="sxs-lookup"><span data-stu-id="f8e0b-123">There are two keys so that you can continue to use the secondary key if you decide to regenerate the primary key, and vice-versa.</span></span>
* <span data-ttu-id="f8e0b-124">A *lekérdezési kulcsok* csak olvasási hozzáférést biztosítanak az indexekhez és a dokumentumokhoz, és általában a keresési kéréseket kibocsátó ügyfélalkalmazások számára vannak kiosztva.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-124">Your *query keys* grant read-only access to indexes and documents, and are typically distributed to client applications that issue search requests.</span></span>

<span data-ttu-id="f8e0b-125">Indexlekérdezéshez a lekérdezési kulcsok egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-125">For the purposes of querying an index, you can use one of your query keys.</span></span> <span data-ttu-id="f8e0b-126">A rendszergazdai kulcsok szintén használhatók a lekérdezésekhez, az alkalmazáskódban azonban inkább lekérdezési kulcsot használjon, mivel ez a módszer jobban követi a [legalacsonyabb jogosultsági szint elvét](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span><span class="sxs-lookup"><span data-stu-id="f8e0b-126">Your admin keys can also be used for queries, but you should use a query key in your application code as this better follows the [Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege).</span></span>

## <a name="create-an-instance-of-the-searchindexclient-class"></a><span data-ttu-id="f8e0b-127">A SearchIndexClient osztály egy példányának létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8e0b-127">Create an instance of the SearchIndexClient class</span></span>
<span data-ttu-id="f8e0b-128">A lekérdezések Azure Search .NET SDK használatával történő kiadásához létre kell hoznia a `SearchIndexClient` osztály egy példányát.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-128">To issue queries with the Azure Search .NET SDK, you will need to create an instance of the `SearchIndexClient` class.</span></span> <span data-ttu-id="f8e0b-129">Ez az osztály több konstruktorral rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-129">This class has several constructors.</span></span> <span data-ttu-id="f8e0b-130">Amelyikre Önnek szüksége van, az paraméterként a Search-szolgáltatás nevét, az indexnevet és egy `SearchCredentials` objektumot használ.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-130">The one you want takes your search service name, index name, and a `SearchCredentials` object as parameters.</span></span> <span data-ttu-id="f8e0b-131">A `SearchCredentials` becsomagolja az API-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-131">`SearchCredentials` wraps your api-key.</span></span>

<span data-ttu-id="f8e0b-132">Az alábbi kód egy új `SearchIndexClient` elemet hoz létre az ([Azure Search-index létrehozása .NET SDK használatával](search-create-index-dotnet.md) című részben létrehozott) „hotels” index számára, az alkalmazás konfigurációs fájljában (a [mintaalkalmazás](http://aka.ms/search-dotnet-howto) esetében az `appsettings.json` fájlban) tárolt Search-szolgáltatásnév és API-kulcs értékeinek használatával:</span><span class="sxs-lookup"><span data-stu-id="f8e0b-132">The code below creates a new `SearchIndexClient` for the "hotels" index (created in [Create an Azure Search index using the .NET SDK](search-create-index-dotnet.md)) using values for the search service name and api-key that are stored in the application's config file (`appsettings.json` in the case of the [sample application](http://aka.ms/search-dotnet-howto)):</span></span>

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

<span data-ttu-id="f8e0b-133">A `SearchIndexClient` rendelkezik egy `Documents` tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-133">`SearchIndexClient` has a `Documents` property.</span></span> <span data-ttu-id="f8e0b-134">Ezen tulajdonság biztosítja mindazokat a módszereket, amelyek az Azure Search-indexek lekérdezéséhez szükségesek.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-134">This property provides all the methods you need to query Azure Search indexes.</span></span>

## <a name="query-your-index"></a><span data-ttu-id="f8e0b-135">Az index lekérdezése</span><span class="sxs-lookup"><span data-stu-id="f8e0b-135">Query your index</span></span>
<span data-ttu-id="f8e0b-136">A .NET SDK használatával történő keresés ugyanolyan egyszerűen végrehajtható, mint a `Documents.Search` módszer meghívása a következőn: `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-136">Searching with the .NET SDK is as simple as calling the `Documents.Search` method on your `SearchIndexClient`.</span></span> <span data-ttu-id="f8e0b-137">Ezen módszer néhány paramétert használ, ide értve a keresett szöveget, a lekérdezés további finomításához használható `SearchParameters` objektummal együtt.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-137">This method takes a few parameters, including the search text, along with a `SearchParameters` object that can be used to further refine the query.</span></span>

#### <a name="types-of-queries"></a><span data-ttu-id="f8e0b-138">A lekérdezések típusai</span><span class="sxs-lookup"><span data-stu-id="f8e0b-138">Types of Queries</span></span>
<span data-ttu-id="f8e0b-139">Az itt használt két fő [lekérdezési típus](search-query-overview.md#types-of-queries): `search` és `filter`.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-139">The two main [query types](search-query-overview.md#types-of-queries) you will use are `search` and `filter`.</span></span> <span data-ttu-id="f8e0b-140">A `search` lekérdezés egy vagy több kifejezésre keres rá az index összes *searchable* (kereshető) mezőjében.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-140">A `search` query searches for one or more terms in all *searchable* fields in your index.</span></span> <span data-ttu-id="f8e0b-141">A `filter` lekérdezés egy logikai kifejezés kiértékelését végzi el az index összes *filterable* (szűrhető) mezőjén.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-141">A `filter` query evaluates a boolean expression over all *filterable* fields in an index.</span></span>

<span data-ttu-id="f8e0b-142">A keresések és a szűrések egyaránt a `Documents.Search` módszer használatával vannak végrehajtva.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-142">Both searches and filters are performed using the `Documents.Search` method.</span></span> <span data-ttu-id="f8e0b-143">Keresési lekérdezések a `searchText` paraméterben, szűrőkifejezések pedig a `SearchParameters` osztály `Filter` tulajdonságában adhatóak át.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-143">A search query can be passed in the `searchText` parameter, while a filter expression can be passed in the `Filter` property of the `SearchParameters` class.</span></span> <span data-ttu-id="f8e0b-144">A keresés nélküli szűrés végrehajtásához a `searchText` paraméter számára a `"*"` kifejezést adja át.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-144">To filter without searching, just pass `"*"` for the `searchText` parameter.</span></span> <span data-ttu-id="f8e0b-145">A szűrés nélküli keresés végrehajtásához ne állítsa be a `Filter` tulajdonságot, vagy egyáltalán ne adja át azt egy `SearchParameters`-példányban.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-145">To search without filtering, just leave the `Filter` property unset, or do not pass in a `SearchParameters` instance at all.</span></span>

#### <a name="example-queries"></a><span data-ttu-id="f8e0b-146">Példa a lekérdezésekre</span><span class="sxs-lookup"><span data-stu-id="f8e0b-146">Example Queries</span></span>
<span data-ttu-id="f8e0b-147">Az alábbi mintakód néhány különböző módját mutatja be az [Azure Search-index létrehozása .NET SDK használatával](search-create-index-dotnet.md#DefineIndex) című részben meghatározott „hotels” index lekérdezésének.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-147">The following sample code shows a few different ways to query the "hotels" index defined in [Create an Azure Search index using the .NET SDK](search-create-index-dotnet.md#DefineIndex).</span></span> <span data-ttu-id="f8e0b-148">Vegye figyelembe, hogy a keresési eredményekkel visszaadott dokumentumok annak a `Hotel` osztálynak a példányai, amely az [Adatok importálása az Azure Search szolgáltatásban .NET SDK használatával](search-import-data-dotnet.md#HotelClass) című részben lett meghatározva.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-148">Note that the documents returned with the search results are instances of the `Hotel` class, which was defined in [Data Import in Azure Search using the .NET SDK](search-import-data-dotnet.md#HotelClass).</span></span> <span data-ttu-id="f8e0b-149">Ez a mintakód a `WriteDocuments` módszer használatával jeleníti meg a keresési eredményeket a konzolon.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-149">The sample code makes use of a `WriteDocuments` method to output the search results to the console.</span></span> <span data-ttu-id="f8e0b-150">Ezt a módszert a következő szakasz ismerteti.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-150">This method is described in the next section.</span></span>

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
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

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="handle-search-results"></a><span data-ttu-id="f8e0b-151">A keresési eredmények kezelése</span><span class="sxs-lookup"><span data-stu-id="f8e0b-151">Handle search results</span></span>
<span data-ttu-id="f8e0b-152">A `Documents.Search` módszer olyan `DocumentSearchResult` objektumot ad vissza, amely tartalmazza a lekérdezés eredményeit.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-152">The `Documents.Search` method returns a `DocumentSearchResult` object that contains the results of the query.</span></span> <span data-ttu-id="f8e0b-153">Az előző szakaszban szereplő példa a `WriteDocuments` módszer használatával jelenítette meg a keresési eredményeket a konzolon:</span><span class="sxs-lookup"><span data-stu-id="f8e0b-153">The example in the previous section used a method called `WriteDocuments` to output the search results to the console:</span></span>

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

<span data-ttu-id="f8e0b-154">Az előző szakasz lekérdezéseinek eredménye a következőképpen fog megjelenni, feltételezve, hogy a „hotels” index az [Adatok importálása az Azure Search szolgáltatásban .NET SDK használatával](search-import-data-dotnet.md) rész mintaadataival van feltöltve:</span><span class="sxs-lookup"><span data-stu-id="f8e0b-154">Here is what the results look like for the queries in the previous section, assuming the "hotels" index is populated with the sample data in [Data Import in Azure Search using the .NET SDK](search-import-data-dotnet.md):</span></span>

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
```

<span data-ttu-id="f8e0b-155">A fenti mintakód a keresési eredményeket a konzolon jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-155">The sample code above uses the console to output search results.</span></span> <span data-ttu-id="f8e0b-156">A keresési eredményeket hasonlóképpen kell megjeleníteni a saját alkalmazásában is.</span><span class="sxs-lookup"><span data-stu-id="f8e0b-156">You will likewise need to display search results in your own application.</span></span> <span data-ttu-id="f8e0b-157">Az ASP.NET MVC-alapú webalkalmazásokban történő keresésieredmény-rendelerelésre itt láthat példát: [a GitHub-on lévő minta](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample).</span><span class="sxs-lookup"><span data-stu-id="f8e0b-157">See [this sample on GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) for an example of how to render search results in an ASP.NET MVC-based web application.</span></span>

