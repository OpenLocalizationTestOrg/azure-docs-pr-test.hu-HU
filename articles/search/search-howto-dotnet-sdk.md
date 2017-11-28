---
title: "Hogyan használható az Azure Search .NET-alkalmazásból |} Microsoft Docs"
description: "Azure Search .NET-alkalmazás használata"
services: search
documentationcenter: 
author: brjohnstmsft
manager: jlembicz
editor: 
ms.assetid: 93653341-c05f-4cfd-be45-bb877f964fcb
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/22/2017
ms.author: brjohnst
ms.openlocfilehash: 552a7ab193e12d2e72da494166d774e974c85d47
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-search-from-a-net-application"></a><span data-ttu-id="4cb6d-103">Azure Search .NET-alkalmazás használata</span><span class="sxs-lookup"><span data-stu-id="4cb6d-103">How to use Azure Search from a .NET Application</span></span>
<span data-ttu-id="4cb6d-104">Ez a cikk a forgatókönyv az első működik, és a rendszer a [Azure Search .NET SDK](https://aka.ms/search-sdk).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-104">This article is a walkthrough to get you up and running with the [Azure Search .NET SDK](https://aka.ms/search-sdk).</span></span> <span data-ttu-id="4cb6d-105">A .NET SDK használatával valósít meg egy hatékony keresési élményt biztosít az alkalmazás Azure Search használatával.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-105">You can use the .NET SDK to implement a rich search experience in your application using Azure Search.</span></span>

## <a name="whats-in-the-azure-search-sdk"></a><span data-ttu-id="4cb6d-106">Mi az az Azure SDK keresése</span><span class="sxs-lookup"><span data-stu-id="4cb6d-106">What's in the Azure Search SDK</span></span>
<span data-ttu-id="4cb6d-107">Az SDK-t tartalmaz egy ügyféloldali kódtár `Microsoft.Azure.Search`.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-107">The SDK consists of a client library, `Microsoft.Azure.Search`.</span></span> <span data-ttu-id="4cb6d-108">Lehetővé teszi kezeli az indexek, az adatforrások és az indexelők, valamint feltöltése és dokumentumok kezeléséhez, és hajtsa végre a lekérdezéseket, anélkül, HTTP és a JSON részleteit kezelésére.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-108">It enables you to manage your indexes, data sources, and indexers, as well as upload and manage documents, and execute queries, all without having to deal with the details of HTTP and JSON.</span></span>

<span data-ttu-id="4cb6d-109">Az ügyféloldali kódtár határozza meg, például osztályok `Index`, `Field`, és `Document`, illetve műveletek, például `Indexes.Create` és `Documents.Search` a a `SearchServiceClient` és `SearchIndexClient` osztályok.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-109">The client library defines classes like `Index`, `Field`, and `Document`, as well as operations like `Indexes.Create` and `Documents.Search` on the `SearchServiceClient` and `SearchIndexClient` classes.</span></span> <span data-ttu-id="4cb6d-110">Ezeket az osztályokat a következő névterek szerint vannak csoportosítva:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-110">These classes are organized into the following namespaces:</span></span>

* [<span data-ttu-id="4cb6d-111">Microsoft.Azure.Search</span><span class="sxs-lookup"><span data-stu-id="4cb6d-111">Microsoft.Azure.Search</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [<span data-ttu-id="4cb6d-112">Microsoft.Azure.Search.Models</span><span class="sxs-lookup"><span data-stu-id="4cb6d-112">Microsoft.Azure.Search.Models</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

<span data-ttu-id="4cb6d-113">Az aktuális az Azure Search .NET SDK verziója most általánosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-113">The current version of the Azure Search .NET SDK is now generally available.</span></span> <span data-ttu-id="4cb6d-114">Ha azt szeretné, vegye figyelembe a következő verziójára való visszajelzést, kérjük, látogasson el a [visszajelzés](https://feedback.azure.com/forums/263029-azure-search/).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-114">If you would like to provide feedback for us to incorporate in the next version, please visit our [feedback page](https://feedback.azure.com/forums/263029-azure-search/).</span></span>

<span data-ttu-id="4cb6d-115">Támogatja a .NET SDK `2016-09-01` , a [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-115">The .NET SDK supports version `2016-09-01` of the [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span> <span data-ttu-id="4cb6d-116">Ebben a verzióban mostantól tartalmazza az egyéni lekérdezések és Azure-Blob és az Azure tábla indexelő támogatást.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-116">This version now includes support for custom analyzers and Azure Blob and Azure Table indexer support.</span></span> <span data-ttu-id="4cb6d-117">Az előzetes funkciók, amelyek *nem* verzióhoz, például az indexelés JSON és a CSV-fájlok támogatását szerepelnek [előzetes](search-api-2015-02-28-preview.md) és a régebbi elérhető [2.0-előzetes verzióját, a .NET SDK](https://aka.ms/search-sdk-preview).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-117">Preview features that are *not* part of this version, such as support for indexing JSON and CSV files, are in [preview](search-api-2015-02-28-preview.md) and available via the older [2.0-preview version of the .NET SDK](https://aka.ms/search-sdk-preview).</span></span>

<span data-ttu-id="4cb6d-118">Ez az SDK nem támogatja a [felügyeleti műveletek](https://docs.microsoft.com/rest/api/searchmanagement/) például létrehozása és a keresési szolgáltatás méretezése és API-kulcsokat kezelése.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-118">This SDK does not support [Management Operations](https://docs.microsoft.com/rest/api/searchmanagement/) such as creating and scaling Search services and managing API keys.</span></span> <span data-ttu-id="4cb6d-119">Ha a keresési erőforrások kezelése a .NET-alkalmazás van szüksége, használhatja a [Azure Search .NET SDK-t felügyeleti](https://aka.ms/search-mgmt-sdk).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-119">If you need to manage your Search resources from a .NET application, you can use the [Azure Search .NET Management SDK](https://aka.ms/search-mgmt-sdk).</span></span>

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a><span data-ttu-id="4cb6d-120">Az SDK legújabb verziójára</span><span class="sxs-lookup"><span data-stu-id="4cb6d-120">Upgrading to the latest version of the SDK</span></span>
<span data-ttu-id="4cb6d-121">Ha már használja az Azure Search .NET SDK régebbi verziója, és frissítse az általánosan elérhető új verzióra szeretné [Ez a cikk](search-dotnet-sdk-migration.md) ismerteti, hogyan.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-121">If you're already using an older version of the Azure Search .NET SDK and you'd like to upgrade to the new generally available version, [this article](search-dotnet-sdk-migration.md) explains how.</span></span>

## <a name="requirements-for-the-sdk"></a><span data-ttu-id="4cb6d-122">Az SDK követelményei</span><span class="sxs-lookup"><span data-stu-id="4cb6d-122">Requirements for the SDK</span></span>
1. <span data-ttu-id="4cb6d-123">A Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-123">Visual Studio 2017.</span></span>
2. <span data-ttu-id="4cb6d-124">A saját Azure Search szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-124">Your own Azure Search service.</span></span> <span data-ttu-id="4cb6d-125">Az SDK használatához szüksége lesz a nevét, valamint a szolgáltatás egy vagy több API-kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-125">In order to use the SDK, you will need the name of your service and one or more API keys.</span></span> <span data-ttu-id="4cb6d-126">[A szolgáltatás létrehozása a portálon](search-create-service-portal.md) segít a fenti lépéseket.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-126">[Create a service in the portal](search-create-service-portal.md) will help you through these steps.</span></span>
3. <span data-ttu-id="4cb6d-127">Töltse le az Azure Search .NET SDK [NuGet-csomag](http://www.nuget.org/packages/Microsoft.Azure.Search) a Visual Studio "Manage NuGet Packages" használatával.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-127">Download the Azure Search .NET SDK [NuGet package](http://www.nuget.org/packages/Microsoft.Azure.Search) by using "Manage NuGet Packages" in Visual Studio.</span></span> <span data-ttu-id="4cb6d-128">A csomag neve csak keressen `Microsoft.Azure.Search` NuGet.org meg.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-128">Just search for the package name `Microsoft.Azure.Search` on NuGet.org.</span></span>

<span data-ttu-id="4cb6d-129">Az Azure Search .NET SDK támogatja a .NET-keretrendszer 4.6 és a .NET Core célzó alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-129">The Azure Search .NET SDK supports applications targeting the .NET Framework 4.6 and .NET Core.</span></span>

## <a name="core-scenarios"></a><span data-ttu-id="4cb6d-130">Legfontosabb forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="4cb6d-130">Core scenarios</span></span>
<span data-ttu-id="4cb6d-131">Számos szempontot következőket kell tennie az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-131">There are several things you'll need to do in your search application.</span></span> <span data-ttu-id="4cb6d-132">Ez az oktatóanyag azt érintünk core forgatókönyvekben:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-132">In this tutorial, we'll cover these core scenarios:</span></span>

* <span data-ttu-id="4cb6d-133">Az index létrehozása</span><span class="sxs-lookup"><span data-stu-id="4cb6d-133">Creating an index</span></span>
* <span data-ttu-id="4cb6d-134">A dokumentumok index feltöltése</span><span class="sxs-lookup"><span data-stu-id="4cb6d-134">Populating the index with documents</span></span>
* <span data-ttu-id="4cb6d-135">A teljes szöveges keresés és a szűrők használatával dokumentumok keresése</span><span class="sxs-lookup"><span data-stu-id="4cb6d-135">Searching for documents using full-text search and filters</span></span>

<span data-ttu-id="4cb6d-136">Az alábbi mintakód bemutatja ezen.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-136">The sample code that follows illustrates each of these.</span></span> <span data-ttu-id="4cb6d-137">Szabadon használhatja a kódrészleteket a saját alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-137">Feel free to use the code snippets in your own application.</span></span>

### <a name="overview"></a><span data-ttu-id="4cb6d-138">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4cb6d-138">Overview</span></span>
<span data-ttu-id="4cb6d-139">A mintaalkalmazás, azt fogja felfedezése létrehoz egy új "hotels" nevű index tölti fel az néhány dokumentumok, akkor néhány keresési lekérdezést hajt végre.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-139">The sample application we'll be exploring creates a new index named "hotels", populates it with a few documents, then executes some search queries.</span></span> <span data-ttu-id="4cb6d-140">A fő program, megjeleníti az általános folyamat a következő:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-140">Here is the main program, showing the overall flow:</span></span>

```csharp
// This sample shows how to delete, create, upload documents and query an index
static void Main(string[] args)
{
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();

    SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

    Console.WriteLine("{0}", "Deleting index...\n");
    DeleteHotelsIndexIfExists(serviceClient);

    Console.WriteLine("{0}", "Creating index...\n");
    CreateHotelsIndex(serviceClient);

    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(indexClient);

    ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

    RunQueries(indexClientForQueries);

    Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
    Console.ReadKey();
}
```

> [!NOTE]
> <span data-ttu-id="4cb6d-141">A jelen útmutatóban használt mintaalkalmazás teljes forráskódját a [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) webhelyén találja.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-141">You can find the full source code of the sample application used in this walk through on [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>
> 
>

<span data-ttu-id="4cb6d-142">Végigvezetjük a lépésről lépésre.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-142">We'll walk through this step by step.</span></span> <span data-ttu-id="4cb6d-143">Először létre kell hozni egy új `SearchServiceClient`.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-143">First we need to create a new `SearchServiceClient`.</span></span> <span data-ttu-id="4cb6d-144">Ez az objektum indexek kezelését teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-144">This object allows you to manage indexes.</span></span> <span data-ttu-id="4cb6d-145">Egy kiszámításához meg kell adnia az Azure Search szolgáltatás neve, valamint az adminisztrációs API-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-145">In order to construct one, you need to provide your Azure Search service name as well as an admin API key.</span></span> <span data-ttu-id="4cb6d-146">Ezt az információt is megadhatja a `appsettings.json` fájlt a [mintaalkalmazás](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-146">You can enter this information in the `appsettings.json` file of the [sample application](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

> [!NOTE]
> <span data-ttu-id="4cb6d-147">Ha megad egy helytelen kulcsot (például egy lekérdezési kulcsot ahol egy adminisztrációs kulcsot volt szükség), a `SearchServiceClient` kivételhibát egy `CloudException` , a hiba üzenet "Tiltott" egy művelet metódust hívja meg, mint például az első alkalommal `Indexes.Create`.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-147">If you provide an incorrect key (for example, a query key where an admin key was required), the `SearchServiceClient` will throw a `CloudException` with the error message "Forbidden" the first time you call an operation method on it, such as `Indexes.Create`.</span></span> <span data-ttu-id="4cb6d-148">Ha ez történik, ellenőrizze az API-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-148">If this happens to you, double-check our API key.</span></span>
> 
> 

<span data-ttu-id="4cb6d-149">A következő néhány sor metódushívások "Hotels" nevet adtuk, először törlése, ha már létezik egy index létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-149">The next few lines call methods to create an index named "hotels", deleting it first if it already exists.</span></span> <span data-ttu-id="4cb6d-150">Végigvezetjük ezen módszerek egy kicsit később.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-150">We will walk through these methods a little later.</span></span>

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

<span data-ttu-id="4cb6d-151">Ezt követően kell kell feltöltenie.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-151">Next, the index needs to be populated.</span></span> <span data-ttu-id="4cb6d-152">Ehhez fel kell egy `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-152">To do this, we will need a `SearchIndexClient`.</span></span> <span data-ttu-id="4cb6d-153">Az beszerzése utólag két módja van: hozhat létre, azt, vagy meghívásával `Indexes.GetClient` a a `SearchServiceClient`.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-153">There are two ways to obtain one: by constructing it, or by calling `Indexes.GetClient` on the `SearchServiceClient`.</span></span> <span data-ttu-id="4cb6d-154">Használjuk az utóbbi kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-154">We use the latter for convenience.</span></span>

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> <span data-ttu-id="4cb6d-155">A keresőalkalmazásokban az indexkezelést és -feltöltést általában a keresési lekérdezésektől eltérő elem végzi.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-155">In a typical search application, index management and population is handled by a separate component from search queries.</span></span> <span data-ttu-id="4cb6d-156">A `Indexes.GetClient` kényelmes megoldás az index feltöltésére, mivel így nem szükséges újabb `SearchCredentials` objektumot biztosítania.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-156">`Indexes.GetClient` is convenient for populating an index because it saves you the trouble of providing another `SearchCredentials`.</span></span> <span data-ttu-id="4cb6d-157">Ezt azon rendszergazdai kulcs átadásával hajtja végre, amelyet a `SearchServiceClient` elemnek az új `SearchIndexClient` objektumban történő létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-157">It does this by passing the admin key that you used to create the `SearchServiceClient` to the new `SearchIndexClient`.</span></span> <span data-ttu-id="4cb6d-158">A lekérdezéseket végrehajtó alkalmazás részeként azonban jobb megoldás a `SearchIndexClient` közvetlen létrehozása, így az egy rendszergazdai kulcs helyett lekérdezési kulcs formájában adható át.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-158">However, in the part of your application that executes queries, it is better to create the `SearchIndexClient` directly so that you can pass in a query key instead of an admin key.</span></span> <span data-ttu-id="4cb6d-159">Ez a legalacsonyabb jogosultsági szint elvét konzisztens, és segít biztonságosabbá teszi az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-159">This is consistent with the principle of least privilege and will help to make your application more secure.</span></span> <span data-ttu-id="4cb6d-160">További információk az adminisztrációs kulcsok és a lekérdezési kulcsok található [Itt](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-160">You can find out more about admin keys and query keys [here](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).</span></span>
> 
> 

<span data-ttu-id="4cb6d-161">Most, hogy egy `SearchIndexClient`, azt töltheti fel az index.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-161">Now that we have a `SearchIndexClient`, we can populate the index.</span></span> <span data-ttu-id="4cb6d-162">Ez végigvezetjük később más módon történik.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-162">This is done by another method that we will walk through later.</span></span>

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

<span data-ttu-id="4cb6d-163">Végül azt néhány keresési-lekérdezéseket hajt végre, és az eredmények megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-163">Finally, we execute a few search queries and display the results.</span></span> <span data-ttu-id="4cb6d-164">Most használjuk a különböző `SearchIndexClient`:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-164">This time we use a different `SearchIndexClient`:</span></span>

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

<span data-ttu-id="4cb6d-165">Most elindítjuk részletes bemutatása a `RunQueries` metódus később.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-165">We will take a closer look at the `RunQueries` method later.</span></span> <span data-ttu-id="4cb6d-166">Új kód `SearchIndexClient`:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-166">Here is the code to create the new `SearchIndexClient`:</span></span>

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

<span data-ttu-id="4cb6d-167">Most használjuk a lekérdezési kulcs mivel jelenleg nem írási hozzáférésre van szüksége az index.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-167">This time we use a query key since we do not need write access to the index.</span></span> <span data-ttu-id="4cb6d-168">Ezt az információt is megadhatja a `appsettings.json` fájlt a [mintaalkalmazás](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-168">You can enter this information in the `appsettings.json` file of the [sample application](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>

<span data-ttu-id="4cb6d-169">Ha az alkalmazás futtatásához a egy érvényes szolgáltatásnevet és API-kulcsokat, a kimenet ehhez hasonló kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-169">If you run this application with a valid service name and API keys, the output should look like this:</span></span>

    Deleting index...
    
    Creating index...
    
    Uploading documents...
    
    Waiting for documents to be indexed...
    
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
    
    Complete.  Press any key to end application...

<span data-ttu-id="4cb6d-170">Ez a cikk végén az alkalmazás a teljes forráskód valósul meg.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-170">The full source code of the application is provided at the end of this article.</span></span>

<span data-ttu-id="4cb6d-171">A következő most elindítjuk szorosabb meg az egyes módszerek által meghívott `Main`.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-171">Next, we will take a closer look at each of the methods called by `Main`.</span></span>

### <a name="creating-an-index"></a><span data-ttu-id="4cb6d-172">Az index létrehozása</span><span class="sxs-lookup"><span data-stu-id="4cb6d-172">Creating an index</span></span>
<span data-ttu-id="4cb6d-173">Miután létrehozta a `SearchServiceClient`, a következő lépésként `Main` does esetén törlése a "Hotels nevű" index már létezik.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-173">After creating a `SearchServiceClient`, the next thing `Main` does is delete the "hotels" index if it already exists.</span></span> <span data-ttu-id="4cb6d-174">Ezt a következő módszerrel:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-174">That is done by the following method:</span></span>

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
    }
}
```

<span data-ttu-id="4cb6d-175">Ezt a módszert használja az adott `SearchServiceClient` ellenőrizze, hogy létezik-e az indexet, és ha igen, törölje azt.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-175">This method uses the given `SearchServiceClient` to check if the index exists, and if so, delete it.</span></span>

> [!NOTE]
> <span data-ttu-id="4cb6d-176">Ebben a cikkben a példakód az egyszerűség érdekében az Azure Search .NET SDK szinkron módszereit használja.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-176">The example code in this article uses the synchronous methods of the Azure Search .NET SDK for simplicity.</span></span> <span data-ttu-id="4cb6d-177">Azt javasoljuk, hogy a méretezhetőség és a gyors válaszadás érdekében használja saját alkalmazásaiban az aszinkron módszereket.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-177">We recommend that you use the asynchronous methods in your own applications to keep them scalable and responsive.</span></span> <span data-ttu-id="4cb6d-178">Például a metódusban használhat `ExistsAsync` és `DeleteAsync` helyett `Exists` és `Delete`.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-178">For example, in the method above you could use `ExistsAsync` and `DeleteAsync` instead of `Exists` and `Delete`.</span></span>
> 
> 

<span data-ttu-id="4cb6d-179">Ezt követően `Main` hoz létre egy új "Hotels nevű" index által a metódus hívása:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-179">Next, `Main` creates a new "hotels" index by calling this method:</span></span>

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels",
        Fields = FieldBuilder.BuildForType<Hotel>()
    };

    serviceClient.Indexes.Create(definition);
}
```

<span data-ttu-id="4cb6d-180">Ezzel a módszerrel hoz létre egy új `Index` listáját tartalmazó objektum `Field` objektumok sémájának definiálásához az új index.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-180">This method creates a new `Index` object with a list of `Field` objects that defines the schema of the new index.</span></span> <span data-ttu-id="4cb6d-181">Egyes mezők nevét, adattípus és a keresés viselkedését meghatározó számos attribútum rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-181">Each field has a name, data type, and several attributes that define its search behavior.</span></span> <span data-ttu-id="4cb6d-182">A `FieldBuilder` osztály reflexió használatával létrehozhat egy `Field` objektumok nyilvános tulajdonságokat és az attribútumok megvizsgálásával az index az adott `Hotel` osztály modell.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-182">The `FieldBuilder` class uses reflection to create a list of `Field` objects for the index by examining the public properties and attributes of the given `Hotel` model class.</span></span> <span data-ttu-id="4cb6d-183">Részletes bemutatása lesz vesszük a `Hotel` osztály később.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-183">We'll take a closer look at the `Hotel` class later on.</span></span>

> [!NOTE]
> <span data-ttu-id="4cb6d-184">Bármikor létrehozhat listájának `Field` objektumok közvetlenül helyett `FieldBuilder` szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-184">You can always create the list of `Field` objects directly instead of using `FieldBuilder` if needed.</span></span> <span data-ttu-id="4cb6d-185">Például előfordulhat, hogy nem használni kívánt egy modellosztállyal, vagy használjon egy meglévő modell osztály nem kívánt attribútumok hozzáadásával módosítani szeretne.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-185">For example, you may not want to use a model class or you may need to use an existing model class that you don't want to modify by adding attributes.</span></span>
>
> 

<span data-ttu-id="4cb6d-186">Mezők mellett azt is megteheti pontozási profil, a javaslattevők vagy a CORS-beállítások az indexhez (ezek hiányoznak kivonatosan mutatja a mintából).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-186">In addition to fields, you can also add scoring profiles, suggesters, or CORS options to the Index (these are omitted from the sample for brevity).</span></span> <span data-ttu-id="4cb6d-187">Az Index objektum és a bennük foglalt részeit további információ található a [SDK referenciáit](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index), valamint ennek a a [Azure Search REST API-referencia](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-187">You can find more information about the Index object and its constituent parts in the [SDK reference](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index), as well as in the [Azure Search REST API reference](https://docs.microsoft.com/rest/api/searchservice/).</span></span>

### <a name="populating-the-index"></a><span data-ttu-id="4cb6d-188">Az index feltöltése</span><span class="sxs-lookup"><span data-stu-id="4cb6d-188">Populating the index</span></span>
<span data-ttu-id="4cb6d-189">A következő lépéssel `Main` az újonnan létrehozott index feltöltése.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-189">The next step in `Main` is to populate the newly-created index.</span></span> <span data-ttu-id="4cb6d-190">Ezt a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-190">This is done in the following method:</span></span>

```csharp
private static void UploadDocuments(ISearchIndexClient indexClient)
{
    var hotels = new Hotel[]
    {
        new Hotel()
        { 
            HotelId = "1", 
            BaseRate = 199.0, 
            Description = "Best hotel in town",
            DescriptionFr = "Meilleur hôtel en ville",
            HotelName = "Fancy Stay",
            Category = "Luxury", 
            Tags = new[] { "pool", "view", "wifi", "concierge" },
            ParkingIncluded = false, 
            SmokingAllowed = false,
            LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero), 
            Rating = 5, 
            Location = GeographyPoint.Create(47.678581, -122.131577)
        },
        new Hotel()
        { 
            HotelId = "2", 
            BaseRate = 79.99,
            Description = "Cheapest hotel in town",
            DescriptionFr = "Hôtel le moins cher en ville",
            HotelName = "Roach Motel",
            Category = "Budget",
            Tags = new[] { "motel", "budget" },
            ParkingIncluded = true,
            SmokingAllowed = true,
            LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
            Rating = 1,
            Location = GeographyPoint.Create(49.678581, -122.131577)
        },
        new Hotel() 
        { 
            HotelId = "3", 
            BaseRate = 129.99,
            Description = "Close to town hall and the river"
        }
    };

    var batch = IndexBatch.Upload(hotels);

    try
    {
        indexClient.Documents.Index(batch);
    }
    catch (IndexBatchException e)
    {
        // Sometimes when your Search service is under load, indexing will fail for some of the documents in
        // the batch. Depending on your application, you can take compensating actions like delaying and
        // retrying. For this simple demo, we just log the failed document keys and continue.
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

    Console.WriteLine("Waiting for documents to be indexed...\n");
    Thread.Sleep(2000);
}
```

<span data-ttu-id="4cb6d-191">Ez a metódus négy részből áll.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-191">This method has four parts.</span></span> <span data-ttu-id="4cb6d-192">Az első létrehoz egy tömbjét `Hotel` objektumok feltölteni az index a bemeneti adatok erre a célra.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-192">The first creates an array of `Hotel` objects that will serve as our input data to upload to the index.</span></span> <span data-ttu-id="4cb6d-193">Ezek az adatok nem változtatható az egyszerűség érdekében.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-193">This data is hard-coded for simplicity.</span></span> <span data-ttu-id="4cb6d-194">A saját alkalmazásban az adatok valószínűleg határozza meg egy külső adatforrásból, például az SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-194">In your own application, your data will likely come from an external data source such as a SQL database.</span></span>

<span data-ttu-id="4cb6d-195">A második rész létrehoz egy `IndexBatch` tartalmazó a dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-195">The second part creates an `IndexBatch` containing the documents.</span></span> <span data-ttu-id="4cb6d-196">Megadja a kötegelt hoz létre, ebben az esetben meghívásával időpontjában alkalmazni kívánt műveletet `IndexBatch.Upload`.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-196">You specify the operation you want to apply to the batch at the time you create it, in this case by calling `IndexBatch.Upload`.</span></span> <span data-ttu-id="4cb6d-197">A kötegelt majd tölt fel az Azure Search-index által az a `Documents.Index` metódust.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-197">The batch is then uploaded to the Azure Search index by the `Documents.Index` method.</span></span>

> [!NOTE]
> <span data-ttu-id="4cb6d-198">Ebben a példában azt csak feltölteni dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-198">In this example, we are just uploading documents.</span></span> <span data-ttu-id="4cb6d-199">Ha szeretne változtatások egyesítése a létező dokumentumok vagy törölhetnek dokumentumokat, létrehozhat meghívásával kötegek `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, vagy `IndexBatch.Delete` helyette.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-199">If you wanted to merge changes into existing documents or delete documents, you could create batches by calling `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, or `IndexBatch.Delete` instead.</span></span> <span data-ttu-id="4cb6d-200">A kötegek különböző műveletei adjunk meghívásával `IndexBatch.New`, gyűjteménye, amely veszi `IndexAction` objektumok, amelyek arra utasítja a Azure Search a dokumentum az adott művelet végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-200">You can also mix different operations in a single batch by calling `IndexBatch.New`, which takes a collection of `IndexAction` objects, each of which tells Azure Search to perform a particular operation on a document.</span></span> <span data-ttu-id="4cb6d-201">Minden egyes hozhat létre `IndexAction` saját műveletet a megfelelő metódus meghívásával `IndexAction.Merge`, `IndexAction.Upload`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-201">You can create each `IndexAction` with its own operation by calling the corresponding method such as `IndexAction.Merge`, `IndexAction.Upload`, and so on.</span></span>
> 
> 

<span data-ttu-id="4cb6d-202">Ez a módszer harmadik része egy catch blokkba, amely kezeli az egyik fontos hiba esetében az indexeléshez.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-202">The third part of this method is a catch block that handles an important error case for indexing.</span></span> <span data-ttu-id="4cb6d-203">Ha az Azure Search-szolgáltatásnak nem sikerül indexelnie a kötegben szereplő fájlok valamelyikét, a `Documents.Index` rendszer `IndexBatchException` választ ad.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-203">If your Azure Search service fails to index some of the documents in the batch, an `IndexBatchException` is thrown by `Documents.Index`.</span></span> <span data-ttu-id="4cb6d-204">Ez akkor történhet meg, ha olyankor végzi a dokumentumok indexelését, amikor a szolgáltatás nagy terhelés alatt áll.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-204">This can happen if you are indexing documents while your service is under heavy load.</span></span> <span data-ttu-id="4cb6d-205">**Javasoljuk, hogy a kódban explicit módon kezelje ezt az esetet.**</span><span class="sxs-lookup"><span data-stu-id="4cb6d-205">**We strongly recommend explicitly handling this case in your code.**</span></span> <span data-ttu-id="4cb6d-206">Azon dokumentumok esetében, ahol az indexelés meghiúsult, elhalaszthatja azt, majd később újra megpróbálkozhat az indexeléssel, vagy a mintának megfelelően naplózhatja azt, és folytathatja a munkáját, esetleg – az alkalmazás adatkonzisztencia-követelményeitől függően – más műveletbe kezdhet.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-206">You can delay and then retry indexing the documents that failed, or you can log and continue like the sample does, or you can do something else depending on your application's data consistency requirements.</span></span>

> [!NOTE]
> <span data-ttu-id="4cb6d-207">Használhatja a `FindFailedActionsToRetry` metódus csak egy korábbi hívás a sikertelen műveleteket tartalmazó új köteg összeállításához `Index`.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-207">You can use the `FindFailedActionsToRetry` method to construct a new batch containing only the actions that failed in a previous call to `Index`.</span></span> <span data-ttu-id="4cb6d-208">A metódus dokumentált [Itt](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) , és annak megfelelően használatát tárgyalja [a StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-208">The method is documented [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) and there is a discussion of how to properly use it [on StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).</span></span>
>
>

<span data-ttu-id="4cb6d-209">Végezetül a `UploadDocuments` metódus késések két másodpercig.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-209">Finally, the `UploadDocuments` method delays for two seconds.</span></span> <span data-ttu-id="4cb6d-210">Az Azure Search-szolgáltatásban az indexelés aszinkron módon történik, így a mintaalkalmazásnak egy rövid ideig várnia kell, amíg a rendszer meggyőződik arról, hogy a dokumentum kereshető.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-210">Indexing happens asynchronously in your Azure Search service, so the sample application needs to wait a short time to ensure that the documents are available for searching.</span></span> <span data-ttu-id="4cb6d-211">Ilyen mértékű késleltetésre kizárólag demók, tesztek és mintaalkalmazások esetében van szükség.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-211">Delays like this are typically only necessary in demos, tests, and sample applications.</span></span>

#### <a name="how-the-net-sdk-handles-documents"></a><span data-ttu-id="4cb6d-212">A .NET SDK dokumentumkezelési módszere</span><span class="sxs-lookup"><span data-stu-id="4cb6d-212">How the .NET SDK handles documents</span></span>
<span data-ttu-id="4cb6d-213">Megfordulhat a fejében, hogy miként képes az Azure Search .NET SDK felhasználó által meghatározott `Hotel` osztályhoz hasonló példányok feltöltésére az indexbe.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-213">You may be wondering how the Azure Search .NET SDK is able to upload instances of a user-defined class like `Hotel` to the index.</span></span> <span data-ttu-id="4cb6d-214">A kérdés megválaszolásához, vizsgáljuk meg a `Hotel` osztály:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-214">To help answer that question, let's look at the `Hotel` class:</span></span>

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// The SerializePropertyNamesAsCamelCase attribute is defined in the Azure Search .NET SDK.
// It ensures that Pascal-case property names in the model class are mapped to camel-case
// field names in the index.
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }

    [IsSearchable]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    [IsFilterable, IsFacetable]
    public bool? SmokingAllowed { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public int? Rating { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }
}
```

<span data-ttu-id="4cb6d-215">Az első szembetűnő dolog, hogy a `Hotel` minden egyes nyilvános tulajdonsága az indexdefiníció egy-egy mezőjének felel meg, egy lényeges különbséggel: a mezők neve minden esetben kisbetűvel, míg a `Hotel` nyilvános tulajdonságainak neve nagybetűvel kezdődik.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-215">The first thing to notice is that each public property of `Hotel` corresponds to a field in the index definition, but with one crucial difference: The name of each field starts with a lower-case letter ("camel case"), while the name of each public property of `Hotel` starts with an upper-case letter ("Pascal case").</span></span> <span data-ttu-id="4cb6d-216">Gyakran kerül sor erre olyan adatkötést végző .NET-alkalmazások esetében, ahol a célséma vezérlése az alkalmazás fejlesztőjének hatáskörén kívül esik.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-216">This is a common scenario in .NET applications that perform data-binding where the target schema is outside the control of the application developer.</span></span> <span data-ttu-id="4cb6d-217">A .NET elnevezési irányelveinek megsértése helyett (a tulajdonságnevek kisbetűs megadásával), utasíthatja az SDK-t a tulajdonságnevek automatikus kisbetűs leképezésére a `[SerializePropertyNamesAsCamelCase]` attribútummal.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-217">Rather than having to violate the .NET naming guidelines by making property names camel-case, you can tell the SDK to map the property names to camel-case automatically with the `[SerializePropertyNamesAsCamelCase]` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="4cb6d-218">Az Azure Search .NET SDK a [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) könyvtárat használja az egyéni modellek JSON-ból és JSON-ba történő szerializálására és deszerializálására.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-218">The Azure Search .NET SDK uses the [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) library to serialize and deserialize your custom model objects to and from JSON.</span></span> <span data-ttu-id="4cb6d-219">A szerializálás szükség szerint testre szabható.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-219">You can customize this serialization if needed.</span></span> <span data-ttu-id="4cb6d-220">További részletekért lásd: [JSON.NET az egyéni szerializálási](#JsonDotNet).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-220">For more details, see [Custom Serialization with JSON.NET](#JsonDotNet).</span></span>
> 
> 

<span data-ttu-id="4cb6d-221">Figyelje meg, a második lépés olyan attribútumok, amelyek például `IsFilterable`, `IsSearchable`, `Key`, és `Analyzer` , amely minden egyes nyilvános tulajdonsága adja.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-221">The second thing to notice are the attributes such as `IsFilterable`, `IsSearchable`, `Key`, and `Analyzer` that decorate each public property.</span></span> <span data-ttu-id="4cb6d-222">Ezek az attribútumok hozzárendelését közvetlenül a [megfelelő attribútumok az Azure Search-index](https://docs.microsoft.com/rest/api/searchservice/create-index#request).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-222">These attributes map directly to the [corresponding attributes of the Azure Search index](https://docs.microsoft.com/rest/api/searchservice/create-index#request).</span></span> <span data-ttu-id="4cb6d-223">A `FieldBuilder` osztály használja ezeket az index definícióját mező összeállításához.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-223">The `FieldBuilder` class uses these to construct field definitions for the index.</span></span>

<span data-ttu-id="4cb6d-224">A harmadik fontos dolog kapcsolatos a `Hotel` osztály az adattípusok a nyilvános tulajdonságainak.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-224">The third important thing about the `Hotel` class are the data types of the public properties.</span></span> <span data-ttu-id="4cb6d-225">A .NET típusú ezeket a tulajdonságokat a megfelelő típusú az index definícióját a hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-225">The .NET types of  these properties map to their equivalent field types in the index definition.</span></span> <span data-ttu-id="4cb6d-226">Például a rendszer a `Edm.String` típusú `Category` szöveges tulajdonságot a `category` mezőbe képezi le.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-226">For example, the `Category` string property maps to the `category` field, which is of type `Edm.String`.</span></span> <span data-ttu-id="4cb6d-227">Hasonló típusleképezés történik a `bool?` és `Edm.Boolean`, illetve a `DateTimeOffset?` és `Edm.DateTimeOffset` között is. A típusleképezés vonatkozó szabályainak dokumentációja az [Azure Search .NET SDK-referenciában](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_), a `Documents.Get` metódusnál található.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-227">There are similar type mappings between `bool?` and `Edm.Boolean`, `DateTimeOffset?` and `Edm.DateTimeOffset`, etc. The specific rules for the type mapping are documented with the `Documents.Get` method in the [Azure Search .NET SDK reference](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span></span> <span data-ttu-id="4cb6d-228">A `FieldBuilder` osztály gondoskodik meg ezt a hozzárendelést, de továbbra is hasznos lehet megérteni, abban az esetben kell elhárítania a szerializálási probléma merül fel.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-228">The `FieldBuilder` class takes care of this mapping for you, but it can still be helpful to understand in case you need to troubleshoot any serialization issues.</span></span>

<span data-ttu-id="4cb6d-229">Ez nem tudják használni a saját osztályok-dokumentumokként működik mindkét irányban; Keresési eredmények beolvasásához is, és látható lesz a következő szakaszban az SDK-val automatikusan deszerializálni őket a kiválasztott típusú lehet.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-229">This ability to use your own classes as documents works in both directions; You can also retrieve search results and have the SDK automatically deserialize them to a type of your choice, as we will see in the next section.</span></span>

> [!NOTE]
> <span data-ttu-id="4cb6d-230">Az Azure Search .NET SDK támogatja a `Document` osztályt használó, dinamikus dokumentumtípusokat is, amely alatt a mezők neveinek értékekre történő kulcs/érték-leképezését értjük.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-230">The Azure Search .NET SDK also supports dynamically-typed documents using the `Document` class, which is a key/value mapping of field names to field values.</span></span> <span data-ttu-id="4cb6d-231">Ez olyan helyzetekben hasznos, ha például a tervezés időpontjában az indexséma még nem ismert, illetve ha az adott modellosztályokhoz történő kötés nehézkes volna.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-231">This is useful in scenarios where you don't know the index schema at design-time, or where it would be inconvenient to bind to specific model classes.</span></span> <span data-ttu-id="4cb6d-232">Az SDK-ban lévő összes, dokumentumokkal foglalkozó módszer a `Document` osztállyal kompatibilis túlterhelésekkel rendelkezik, valamint olyan szigorú típusmegadású túlterhelésekkel, amelyek általános típusú paramétert vesznek fel.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-232">All the methods in the SDK that deal with documents have overloads that work with the `Document` class, as well as strongly-typed overloads that take a generic type parameter.</span></span> <span data-ttu-id="4cb6d-233">Csak az utóbbi használnak a mintakódot az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-233">Only the latter are used in the sample code in this tutorial.</span></span> <span data-ttu-id="4cb6d-234">A `Document` osztály örökli `Dictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-234">The `Document` class inherits from `Dictionary<string, object>`.</span></span> <span data-ttu-id="4cb6d-235">Más részletei [Itt](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-235">You can find other details [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).</span></span>
> 
> 

<span data-ttu-id="4cb6d-236">**Miért használjon nullázható adattípusokat?**</span><span class="sxs-lookup"><span data-stu-id="4cb6d-236">**Why you should use nullable data types**</span></span>

<span data-ttu-id="4cb6d-237">Az Azure Search-indexre leképezést végző, saját modellosztályok létrehozásakor javasoljuk, hogy például a `bool` és `int` értéktípusok tulajdonságainak megadása nullázhatóként történjen (például `bool` helyett `bool?`).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-237">When designing your own model classes to map to an Azure Search index, we recommend declaring properties of value types such as `bool` and `int` to be nullable (for example, `bool?` instead of `bool`).</span></span> <span data-ttu-id="4cb6d-238">Nem nullázható tulajdonság használatakor **garantálnia** kell, hogy az index egyetlen dokumentuma sem tartalmaz az adott mezőben null értéket.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-238">If you use a non-nullable property, you have to **guarantee** that no documents in your index contain a null value for the corresponding field.</span></span> <span data-ttu-id="4cb6d-239">Ennek kényszerítéséhez sem az SDK, sem az Azure Search szolgáltatás nem nyújt segítséget.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-239">Neither the SDK nor the Azure Search service will help you to enforce this.</span></span>

<span data-ttu-id="4cb6d-240">Ennek nem csupán elméleti jelentősége van: képzeljünk el például egy olyan alkalmazási helyzetet, ahol egy `Edm.Int32` típusú, meglévő indexhez új mezőt kell hozzáadnunk.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-240">This is not just a hypothetical concern: Imagine a scenario where you add a new field to an existing index that is of type `Edm.Int32`.</span></span> <span data-ttu-id="4cb6d-241">Az indexdefiníció frissítését követően ehhez a mezőhöz minden dokumentumban null érték tartozik (mivel az Azure Search szolgáltatásban az összes értéktípus nullázható).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-241">After updating the index definition, all documents will have a null value for that new field (since all types are nullable in Azure Search).</span></span> <span data-ttu-id="4cb6d-242">Ha ezt követően egy modellosztályt úgy alkalmaz, hogy ehhez a mezőhöz nem nullázható `int` tulajdonságot ad meg, a dokumentumok lekérdezésének megkísérlésekor egy ehhez hasonló `JsonSerializationException` választ kap:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-242">If you then use a model class with a non-nullable `int` property for that field, you will get a `JsonSerializationException` like this when trying to retrieve documents:</span></span>

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

<span data-ttu-id="4cb6d-243">Ezért javasoljuk, hogy a modellosztályokban nullázható értéktípusokat használjon.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-243">For this reason, we recommend that you use nullable types in your model classes as a best practice.</span></span>

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a><span data-ttu-id="4cb6d-244">A JSON.NET egyedi szerializálás</span><span class="sxs-lookup"><span data-stu-id="4cb6d-244">Custom Serialization with JSON.NET</span></span>
<span data-ttu-id="4cb6d-245">Az SDK JSON.NET szerializálása és deszerializálása dokumentumok használ.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-245">The SDK uses JSON.NET for serializing and deserializing documents.</span></span> <span data-ttu-id="4cb6d-246">Testre szabhatja a szerializálás és szükség esetén meghatározhat egy saját deszerializálási `JsonConverter` vagy `IContractResolver` (lásd a [JSON.NET dokumentáció](http://www.newtonsoft.com/json/help/html/Introduction.htm) további részletekért).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-246">You can customize serialization and deserialization if needed by defining your own `JsonConverter` or `IContractResolver` (see the [JSON.NET documentation](http://www.newtonsoft.com/json/help/html/Introduction.htm) for more details).</span></span> <span data-ttu-id="4cb6d-247">Ez akkor lehet hasznos, ha meg szeretné alkalmazkodnak az alkalmazás Azure Search és egyéb speciális forgatókönyvekhez használható modellt egy meglévő osztály.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-247">This can be useful when you want to adapt an existing model class from your application for use with Azure Search, and other more advanced scenarios.</span></span> <span data-ttu-id="4cb6d-248">Például az egyedi szerializálás lehetővé teszi:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-248">For example, with custom serialization you can:</span></span>

* <span data-ttu-id="4cb6d-249">Tartalmaznak, vagy zárja ki a modell osztály egyes tulajdonságai dokumentum mezői tárolt.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-249">Include or exclude certain properties of your model class from being stored as document fields.</span></span>
* <span data-ttu-id="4cb6d-250">Képezze le a kódban nevei és az indexben mezőnevek között.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-250">Map between property names in your code and field names in your index.</span></span>
* <span data-ttu-id="4cb6d-251">Hozzon létre egyéni attribútumok tulajdonságok hozzárendelése dokumentum mező használható.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-251">Create custom attributes that can be used for mapping properties to document fields.</span></span>

<span data-ttu-id="4cb6d-252">Példák egyedi szerializálás egység tesztek végrehajtása az Azure Search .NET SDK a Githubon található.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-252">You can find examples of implementing custom serialization in the unit tests for the Azure Search .NET SDK on GitHub.</span></span> <span data-ttu-id="4cb6d-253">A jó kiindulási pont [Ez a mappa](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-253">A good starting point is [this folder](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models).</span></span> <span data-ttu-id="4cb6d-254">Az egyéni szerializálási tesztek által használt osztályok tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-254">It contains classes that are used by the custom serialization tests.</span></span>

### <a name="searching-for-documents-in-the-index"></a><span data-ttu-id="4cb6d-255">Az indexelt dokumentumok keresése</span><span class="sxs-lookup"><span data-stu-id="4cb6d-255">Searching for documents in the index</span></span>
<span data-ttu-id="4cb6d-256">A mintaalkalmazás utolsó lépése, hogy az index egyes dokumentumok keresése.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-256">The last step in the sample application is to search for some documents in the index.</span></span> <span data-ttu-id="4cb6d-257">A következő metódus végzi ezt:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-257">The following method does this:</span></span>

```csharp
private static void RunQueries(ISearchIndexClient indexClient)
{
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
}
```

<span data-ttu-id="4cb6d-258">Minden alkalommal, amikor végrehajtja a lekérdezést, ez a módszer először létrehoz egy új `SearchParameters` objektum.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-258">Each time it executes a query, this method first creates a new `SearchParameters` object.</span></span> <span data-ttu-id="4cb6d-259">Ez például rendezési, szűrési, lapozás és értékkorlátozás a lekérdezés további beállítások megadására szolgál.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-259">This is used to specify additional options for the query such as sorting, filtering, paging, and faceting.</span></span> <span data-ttu-id="4cb6d-260">Ezzel a módszerrel beállítás van a `Filter`, `Select`, `OrderBy`, és `Top` különböző lekérdezések tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-260">In this method, we're setting the `Filter`, `Select`, `OrderBy`, and `Top` property for different queries.</span></span> <span data-ttu-id="4cb6d-261">Minden a `SearchParameters` tulajdonságok szerepelnek [Itt](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-261">All the `SearchParameters` properties are documented [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).</span></span>

<span data-ttu-id="4cb6d-262">A következő lépésre ténylegesen a keresési lekérdezés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-262">The next step is to actually execute the search query.</span></span> <span data-ttu-id="4cb6d-263">Ebben az esetben használja a `Documents.Search` metódust.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-263">This is done using the `Documents.Search` method.</span></span> <span data-ttu-id="4cb6d-264">Minden egyes lekérdezés esetén, azt át kívánja használni, mint a karakterlánc a keresési szöveget (vagy `"*"` nincs a keresési szöveget esetén), és a keresési paramétereket, korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-264">For each query, we pass the search text to use as a string (or `"*"` if there is no search text), plus the search parameters created earlier.</span></span> <span data-ttu-id="4cb6d-265">Azt is megadhatja, `Hotel` a típust paraméterként `Documents.Search`, amely közli a dokumentumok a keresési eredmények deszerializálása típusú objektumot az SDK `Hotel`.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-265">We also specify `Hotel` as the type parameter for `Documents.Search`, which tells the SDK to deserialize documents in the search results into objects of type `Hotel`.</span></span>

> [!NOTE]
> <span data-ttu-id="4cb6d-266">További információ a keresési lekérdezés kifejezésszintaxist található [Itt](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-266">You can find more information about the search query expression syntax [here](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).</span></span>
> 
> 

<span data-ttu-id="4cb6d-267">Végül minden egyes lekérdezés után ez a módszer telepítéseket összes a találat a keresési eredmények között, a konzol minden egyes dokumentum nyomtatása:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-267">Finally, after each query this method iterates through all the matches in the search results, printing each document to the console:</span></span>

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

<span data-ttu-id="4cb6d-268">Vegyük pedig egyes lekérdezések részletes bemutatása.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-268">Let's take a closer look at each of the queries in turn.</span></span> <span data-ttu-id="4cb6d-269">Az első lekérdezés végrehajtása a kód itt látható:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-269">Here is the code to execute the first query:</span></span>

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

<span data-ttu-id="4cb6d-270">Ebben az esetben azt keresünk, amelyek megfelelnek a word "költségvetés" hotels, és azt szeretné, hogy vissza csak a szállodák neveket, leírtak szerint a `Select` paraméter.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-270">In this case, we're searching for hotels that match the word "budget", and we want to get back only the hotel names, as specified by the `Select` parameter.</span></span> <span data-ttu-id="4cb6d-271">Az alábbi történik:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-271">Here are the results:</span></span>

    Name: Roach Motel

<span data-ttu-id="4cb6d-272">A következő azt szeretnénk, a szállodák egy éjszakánként sebessége kisebb, mint 150 $ található, és csak a szállodák Azonosítóját és a leírását adja vissza:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-272">Next, we want to find the hotels with a nightly rate of less than $150, and return only the hotel ID and description:</span></span>

```csharp
parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

<span data-ttu-id="4cb6d-273">Ez a lekérdezés használ egy OData `$filter` kifejezését `baseRate lt 150`, a dokumentumok az indexben szűrése.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-273">This query uses an OData `$filter` expression, `baseRate lt 150`, to filter the documents in the index.</span></span> <span data-ttu-id="4cb6d-274">További információk az OData-szintaxis, amely támogatja az Azure Search található [Itt](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-274">You can find out more about the OData syntax that Azure Search supports [here](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).</span></span>

<span data-ttu-id="4cb6d-275">Az alábbiakban a lekérdezés eredményét:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-275">Here are the results of the query:</span></span>

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river

<span data-ttu-id="4cb6d-276">A következő szeretnénk keresse meg a felső két szállodák, amely rendelkezik lett utoljára felújított, és a szállodák nevét és az utolsó felújítás dátumának megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-276">Next, we want to find the top two hotels that have been most recently renovated, and show the hotel name and last renovation date.</span></span> <span data-ttu-id="4cb6d-277">A kód itt látható:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-277">Here is the code:</span></span> 

```csharp
parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

<span data-ttu-id="4cb6d-278">Ebben az esetben újra használjuk OData szintaxis adja meg a `OrderBy` paraméterként `lastRenovationDate desc`.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-278">In this case, we again use OData syntax to specify the `OrderBy` parameter as `lastRenovationDate desc`.</span></span> <span data-ttu-id="4cb6d-279">Azt is beállíthatja `Top` 2. Győződjön meg arról, azt csak a felső két dokumentumok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-279">We also set `Top` to 2 to ensure we only get the top two documents.</span></span> <span data-ttu-id="4cb6d-280">Előtt, hogy állítsa `Select` adhatja meg, melyik mezőkre vissza kell adni.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-280">As before, we set `Select` to specify which fields should be returned.</span></span>

<span data-ttu-id="4cb6d-281">Az alábbi történik:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-281">Here are the results:</span></span>

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

<span data-ttu-id="4cb6d-282">Végül azt szeretnénk található összes szállodák, amelyek megfelelnek a "motel" szót:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-282">Finally, we want to find all hotels that match the word "motel":</span></span>

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

<span data-ttu-id="4cb6d-283">Az alábbiakban az eredményeket, amely az összes mezők szerepelhetnek, mivel jelenleg nem adta meg, és a `Select` tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="4cb6d-283">And here are the results, which include all fields since we did not specify the `Select` property:</span></span>

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

<span data-ttu-id="4cb6d-284">Ez a lépés az oktatóanyag befejezése, de itt nem állnak le.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-284">This step completes the tutorial, but don't stop here.</span></span> <span data-ttu-id="4cb6d-285">**További lépések** Azure Search többet további forrásokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-285">**Next steps** provides additional resources for learning more about Azure Search.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4cb6d-286">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4cb6d-286">Next steps</span></span>
* <span data-ttu-id="4cb6d-287">Nézze át a [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) és a [REST API](https://docs.microsoft.com/rest/api/searchservice/) referenciáit.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-287">Browse the references for the [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) and [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>
* <span data-ttu-id="4cb6d-288">A Tudásbázis keresztül elmélyítsék [videókat és más mintákat és oktatóanyagok](search-video-demo-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="4cb6d-288">Deepen your knowledge through [videos and other samples and tutorials](search-video-demo-tutorial-list.md).</span></span>
* <span data-ttu-id="4cb6d-289">Felülvizsgálati [elnevezési konvenciói](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) további a különféle objektumok elnevezési szabályai.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-289">Review [naming conventions](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) to learn the rules for naming various objects.</span></span>
* <span data-ttu-id="4cb6d-290">Felülvizsgálati [a támogatott adattípusokat](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) az Azure Search.</span><span class="sxs-lookup"><span data-stu-id="4cb6d-290">Review [supported data types](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) in Azure Search.</span></span>
