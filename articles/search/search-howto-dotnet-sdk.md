---
title: "aaaHow toouse Azure Search .NET-alkalmazásból |} Microsoft Docs"
description: "Hogyan toouse Azure keresési .NET-alkalmazás"
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
ms.openlocfilehash: 8e13fbe5549547d65941b856ce5a90611261388f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-search-from-a-net-application"></a><span data-ttu-id="819e2-103">Hogyan toouse Azure keresési .NET-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="819e2-103">How toouse Azure Search from a .NET Application</span></span>
<span data-ttu-id="819e2-104">Ez a cikk egy forgatókönyv tooget, a hello lépéseivel [Azure Search .NET SDK](https://aka.ms/search-sdk).</span><span class="sxs-lookup"><span data-stu-id="819e2-104">This article is a walkthrough tooget you up and running with hello [Azure Search .NET SDK](https://aka.ms/search-sdk).</span></span> <span data-ttu-id="819e2-105">Hello .NET SDK tooimplement egy gazdag is használhatja az Azure Search segítségével az alkalmazás nyújtotta adatkeresési élménybe.</span><span class="sxs-lookup"><span data-stu-id="819e2-105">You can use hello .NET SDK tooimplement a rich search experience in your application using Azure Search.</span></span>

## <a name="whats-in-hello-azure-search-sdk"></a><span data-ttu-id="819e2-106">Mi az a hello az Azure Search SDK</span><span class="sxs-lookup"><span data-stu-id="819e2-106">What's in hello Azure Search SDK</span></span>
<span data-ttu-id="819e2-107">hello SDK áll egy ügyféloldali kódtár `Microsoft.Azure.Search`.</span><span class="sxs-lookup"><span data-stu-id="819e2-107">hello SDK consists of a client library, `Microsoft.Azure.Search`.</span></span> <span data-ttu-id="819e2-108">Azt, toomanage lehetővé teszi, hogy az indexek, az adatforrások és az indexelők, valamint feltöltése és dokumentumok kezeléséhez, és hajtsa végre a lekérdezéseket, anélkül, hogy a HTTP és a JSON hello adatokkal toodeal.</span><span class="sxs-lookup"><span data-stu-id="819e2-108">It enables you toomanage your indexes, data sources, and indexers, as well as upload and manage documents, and execute queries, all without having toodeal with hello details of HTTP and JSON.</span></span>

<span data-ttu-id="819e2-109">hello ügyféloldali kódtár határozza meg, például osztályok `Index`, `Field`, és `Document`, illetve műveletek, például `Indexes.Create` és `Documents.Search` a hello `SearchServiceClient` és `SearchIndexClient` osztályok.</span><span class="sxs-lookup"><span data-stu-id="819e2-109">hello client library defines classes like `Index`, `Field`, and `Document`, as well as operations like `Indexes.Create` and `Documents.Search` on hello `SearchServiceClient` and `SearchIndexClient` classes.</span></span> <span data-ttu-id="819e2-110">Ezeket az osztályokat a következő névterek hello szerint vannak csoportosítva:</span><span class="sxs-lookup"><span data-stu-id="819e2-110">These classes are organized into hello following namespaces:</span></span>

* [<span data-ttu-id="819e2-111">Microsoft.Azure.Search</span><span class="sxs-lookup"><span data-stu-id="819e2-111">Microsoft.Azure.Search</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [<span data-ttu-id="819e2-112">Microsoft.Azure.Search.Models</span><span class="sxs-lookup"><span data-stu-id="819e2-112">Microsoft.Azure.Search.Models</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

<span data-ttu-id="819e2-113">hello aktuális hello Azure Search .NET SDK verziója most általánosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="819e2-113">hello current version of hello Azure Search .NET SDK is now generally available.</span></span> <span data-ttu-id="819e2-114">Ha azt szeretné, hogy tooprovide visszajelzést az USA tooincorporate hello következő verziójában, látogasson el a [visszajelzés](https://feedback.azure.com/forums/263029-azure-search/).</span><span class="sxs-lookup"><span data-stu-id="819e2-114">If you would like tooprovide feedback for us tooincorporate in hello next version, please visit our [feedback page](https://feedback.azure.com/forums/263029-azure-search/).</span></span>

<span data-ttu-id="819e2-115">hello .NET SDK verziót támogatja `2016-09-01` a hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="819e2-115">hello .NET SDK supports version `2016-09-01` of hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span> <span data-ttu-id="819e2-116">Ebben a verzióban mostantól tartalmazza az egyéni lekérdezések és Azure-Blob és az Azure tábla indexelő támogatást.</span><span class="sxs-lookup"><span data-stu-id="819e2-116">This version now includes support for custom analyzers and Azure Blob and Azure Table indexer support.</span></span> <span data-ttu-id="819e2-117">Az előzetes funkciók, amelyek *nem* verzióhoz, például az indexelés JSON és a CSV-fájlok támogatását szerepelnek [előzetes](search-api-2015-02-28-preview.md) és régebbi hello keresztül elérhető [hello .NET SDK 2.0-előzetes verzióját ](https://aka.ms/search-sdk-preview).</span><span class="sxs-lookup"><span data-stu-id="819e2-117">Preview features that are *not* part of this version, such as support for indexing JSON and CSV files, are in [preview](search-api-2015-02-28-preview.md) and available via hello older [2.0-preview version of hello .NET SDK](https://aka.ms/search-sdk-preview).</span></span>

<span data-ttu-id="819e2-118">Ez az SDK nem támogatja a [felügyeleti műveletek](https://docs.microsoft.com/rest/api/searchmanagement/) például létrehozása és a keresési szolgáltatás méretezése és API-kulcsokat kezelése.</span><span class="sxs-lookup"><span data-stu-id="819e2-118">This SDK does not support [Management Operations](https://docs.microsoft.com/rest/api/searchmanagement/) such as creating and scaling Search services and managing API keys.</span></span> <span data-ttu-id="819e2-119">Ha toomanage kell a .NET-alkalmazás erőforrások keresése, használhatja a hello [Azure Search .NET SDK-t felügyeleti](https://aka.ms/search-mgmt-sdk).</span><span class="sxs-lookup"><span data-stu-id="819e2-119">If you need toomanage your Search resources from a .NET application, you can use hello [Azure Search .NET Management SDK](https://aka.ms/search-mgmt-sdk).</span></span>

## <a name="upgrading-toohello-latest-version-of-hello-sdk"></a><span data-ttu-id="819e2-120">Toohello hello SDK legújabb verziójának frissítése</span><span class="sxs-lookup"><span data-stu-id="819e2-120">Upgrading toohello latest version of hello SDK</span></span>
<span data-ttu-id="819e2-121">Ha már használja egy régebbi verzióját hello Azure Search .NET SDK-t, és azt szeretné, hogy tooupgrade toohello általánosan elérhető verziójának, [Ez a cikk](search-dotnet-sdk-migration.md) ismerteti, hogyan.</span><span class="sxs-lookup"><span data-stu-id="819e2-121">If you're already using an older version of hello Azure Search .NET SDK and you'd like tooupgrade toohello new generally available version, [this article](search-dotnet-sdk-migration.md) explains how.</span></span>

## <a name="requirements-for-hello-sdk"></a><span data-ttu-id="819e2-122">Hello SDK követelményei</span><span class="sxs-lookup"><span data-stu-id="819e2-122">Requirements for hello SDK</span></span>
1. <span data-ttu-id="819e2-123">A Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="819e2-123">Visual Studio 2017.</span></span>
2. <span data-ttu-id="819e2-124">A saját Azure Search szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="819e2-124">Your own Azure Search service.</span></span> <span data-ttu-id="819e2-125">A sorrend toouse hello SDK szüksége lesz a hello a szolgáltatás nevét és egy vagy több API-kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="819e2-125">In order toouse hello SDK, you will need hello name of your service and one or more API keys.</span></span> <span data-ttu-id="819e2-126">[Szolgáltatás létrehozása a portál hello](search-create-service-portal.md) segít a fenti lépéseket.</span><span class="sxs-lookup"><span data-stu-id="819e2-126">[Create a service in hello portal](search-create-service-portal.md) will help you through these steps.</span></span>
3. <span data-ttu-id="819e2-127">Töltse le az Azure Search .NET SDK hello [NuGet-csomag](http://www.nuget.org/packages/Microsoft.Azure.Search) a Visual Studio "Manage NuGet Packages" használatával.</span><span class="sxs-lookup"><span data-stu-id="819e2-127">Download hello Azure Search .NET SDK [NuGet package](http://www.nuget.org/packages/Microsoft.Azure.Search) by using "Manage NuGet Packages" in Visual Studio.</span></span> <span data-ttu-id="819e2-128">Hello csomag neve csak keressen `Microsoft.Azure.Search` NuGet.org meg.</span><span class="sxs-lookup"><span data-stu-id="819e2-128">Just search for hello package name `Microsoft.Azure.Search` on NuGet.org.</span></span>

<span data-ttu-id="819e2-129">hello Azure Search .NET SDK támogatja a .NET-keretrendszer 4.6 hello és a .NET Core célzó alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="819e2-129">hello Azure Search .NET SDK supports applications targeting hello .NET Framework 4.6 and .NET Core.</span></span>

## <a name="core-scenarios"></a><span data-ttu-id="819e2-130">Legfontosabb forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="819e2-130">Core scenarios</span></span>
<span data-ttu-id="819e2-131">Számos szempontot toodo szüksége lesz az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="819e2-131">There are several things you'll need toodo in your search application.</span></span> <span data-ttu-id="819e2-132">Ez az oktatóanyag azt érintünk core forgatókönyvekben:</span><span class="sxs-lookup"><span data-stu-id="819e2-132">In this tutorial, we'll cover these core scenarios:</span></span>

* <span data-ttu-id="819e2-133">Az index létrehozása</span><span class="sxs-lookup"><span data-stu-id="819e2-133">Creating an index</span></span>
* <span data-ttu-id="819e2-134">A dokumentumok hello index feltöltése</span><span class="sxs-lookup"><span data-stu-id="819e2-134">Populating hello index with documents</span></span>
* <span data-ttu-id="819e2-135">A teljes szöveges keresés és a szűrők használatával dokumentumok keresése</span><span class="sxs-lookup"><span data-stu-id="819e2-135">Searching for documents using full-text search and filters</span></span>

<span data-ttu-id="819e2-136">a következő hello mintakód ezen mutatja be.</span><span class="sxs-lookup"><span data-stu-id="819e2-136">hello sample code that follows illustrates each of these.</span></span> <span data-ttu-id="819e2-137">Érzi, hogy szabad toouse hello kódtöredékek a saját alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="819e2-137">Feel free toouse hello code snippets in your own application.</span></span>

### <a name="overview"></a><span data-ttu-id="819e2-138">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="819e2-138">Overview</span></span>
<span data-ttu-id="819e2-139">hello azt fogja felfedezése mintaalkalmazás létrehoz egy új "hotels" nevű index tölti fel az néhány dokumentumok, akkor néhány keresési lekérdezést hajt végre.</span><span class="sxs-lookup"><span data-stu-id="819e2-139">hello sample application we'll be exploring creates a new index named "hotels", populates it with a few documents, then executes some search queries.</span></span> <span data-ttu-id="819e2-140">Hello fő program, amely itt található általános folyamata hello:</span><span class="sxs-lookup"><span data-stu-id="819e2-140">Here is hello main program, showing hello overall flow:</span></span>

```csharp
// This sample shows how toodelete, create, upload documents and query an index
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

    Console.WriteLine("{0}", "Complete.  Press any key tooend application...\n");
    Console.ReadKey();
}
```

> [!NOTE]
> <span data-ttu-id="819e2-141">Hello teljes forráskód a lépésein végighaladva a használt hello mintaalkalmazásnak található [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span><span class="sxs-lookup"><span data-stu-id="819e2-141">You can find hello full source code of hello sample application used in this walk through on [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>
> 
>

<span data-ttu-id="819e2-142">Végigvezetjük a lépésről lépésre.</span><span class="sxs-lookup"><span data-stu-id="819e2-142">We'll walk through this step by step.</span></span> <span data-ttu-id="819e2-143">Először igazolnia kell, hogy új toocreate `SearchServiceClient`.</span><span class="sxs-lookup"><span data-stu-id="819e2-143">First we need toocreate a new `SearchServiceClient`.</span></span> <span data-ttu-id="819e2-144">Ez az objektum teszi lehetővé toomanage indexeket.</span><span class="sxs-lookup"><span data-stu-id="819e2-144">This object allows you toomanage indexes.</span></span> <span data-ttu-id="819e2-145">Az egyik rendelés tooconstruct kell tooprovide az Azure Search szolgáltatás neve, valamint az adminisztrációs API-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="819e2-145">In order tooconstruct one, you need tooprovide your Azure Search service name as well as an admin API key.</span></span> <span data-ttu-id="819e2-146">Ezt az információt is beírhatja hello `appsettings.json` hello fájljának [mintaalkalmazás](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span><span class="sxs-lookup"><span data-stu-id="819e2-146">You can enter this information in hello `appsettings.json` file of hello [sample application](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>

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
> <span data-ttu-id="819e2-147">Ha megad egy helytelen kulcsot (például egy lekérdezési kulcsot ahol egy adminisztrációs kulcsot volt szükség), hello `SearchServiceClient` kivételhibát egy `CloudException` hello hiba üzenet "Tiltott" hello, először egy művelet metódust hívja meg, például a `Indexes.Create`.</span><span class="sxs-lookup"><span data-stu-id="819e2-147">If you provide an incorrect key (for example, a query key where an admin key was required), hello `SearchServiceClient` will throw a `CloudException` with hello error message "Forbidden" hello first time you call an operation method on it, such as `Indexes.Create`.</span></span> <span data-ttu-id="819e2-148">Ilyen esetben tooyou gondosan ellenőrizze az API-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="819e2-148">If this happens tooyou, double-check our API key.</span></span>
> 
> 

<span data-ttu-id="819e2-149">hello tovább néhány sor hívás módszerek toocreate index "Hotels" nevet adtuk, először törlése, ha már létezik.</span><span class="sxs-lookup"><span data-stu-id="819e2-149">hello next few lines call methods toocreate an index named "hotels", deleting it first if it already exists.</span></span> <span data-ttu-id="819e2-150">Végigvezetjük ezen módszerek egy kicsit később.</span><span class="sxs-lookup"><span data-stu-id="819e2-150">We will walk through these methods a little later.</span></span>

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

<span data-ttu-id="819e2-151">A következő hello kell toobe feltöltve.</span><span class="sxs-lookup"><span data-stu-id="819e2-151">Next, hello index needs toobe populated.</span></span> <span data-ttu-id="819e2-152">toodo, fel kell egy `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="819e2-152">toodo this, we will need a `SearchIndexClient`.</span></span> <span data-ttu-id="819e2-153">Nincsenek a két módon tooobtain egyik: hozhat létre, azt, vagy meghívásával `Indexes.GetClient` a hello `SearchServiceClient`.</span><span class="sxs-lookup"><span data-stu-id="819e2-153">There are two ways tooobtain one: by constructing it, or by calling `Indexes.GetClient` on hello `SearchServiceClient`.</span></span> <span data-ttu-id="819e2-154">Ez utóbbi hello használjuk kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="819e2-154">We use hello latter for convenience.</span></span>

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> <span data-ttu-id="819e2-155">A keresőalkalmazásokban az indexkezelést és -feltöltést általában a keresési lekérdezésektől eltérő elem végzi.</span><span class="sxs-lookup"><span data-stu-id="819e2-155">In a typical search application, index management and population is handled by a separate component from search queries.</span></span> <span data-ttu-id="819e2-156">`Indexes.GetClient`előnyei index feltöltése, mert menti azt, hogy egy másik probléma hello `SearchCredentials`.</span><span class="sxs-lookup"><span data-stu-id="819e2-156">`Indexes.GetClient` is convenient for populating an index because it saves you hello trouble of providing another `SearchCredentials`.</span></span> <span data-ttu-id="819e2-157">Ennek érdekében átadásakor hello adminisztrációs kulcsot adott meg használt toocreate hello `SearchServiceClient` új toohello `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="819e2-157">It does this by passing hello admin key that you used toocreate hello `SearchServiceClient` toohello new `SearchIndexClient`.</span></span> <span data-ttu-id="819e2-158">Azonban hello része az alkalmazás, amely végrehajtja a lekérdezéseket, hogy a rendszer jobb toocreate hello `SearchIndexClient` közvetlenül, hogy egy lekérdezési kulcsot helyett egy adminisztrációs kulcsot is át.</span><span class="sxs-lookup"><span data-stu-id="819e2-158">However, in hello part of your application that executes queries, it is better toocreate hello `SearchIndexClient` directly so that you can pass in a query key instead of an admin key.</span></span> <span data-ttu-id="819e2-159">Ez a legalacsonyabb jogosultsági szint elve hello konzisztens, és segít toomake biztonságosabb az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="819e2-159">This is consistent with hello principle of least privilege and will help toomake your application more secure.</span></span> <span data-ttu-id="819e2-160">További információk az adminisztrációs kulcsok és a lekérdezési kulcsok található [Itt](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).</span><span class="sxs-lookup"><span data-stu-id="819e2-160">You can find out more about admin keys and query keys [here](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).</span></span>
> 
> 

<span data-ttu-id="819e2-161">Most, hogy egy `SearchIndexClient`, hello index feltöltünk is.</span><span class="sxs-lookup"><span data-stu-id="819e2-161">Now that we have a `SearchIndexClient`, we can populate hello index.</span></span> <span data-ttu-id="819e2-162">Ez végigvezetjük később más módon történik.</span><span class="sxs-lookup"><span data-stu-id="819e2-162">This is done by another method that we will walk through later.</span></span>

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

<span data-ttu-id="819e2-163">Végül azt néhány keresési-lekérdezéseket hajt végre, és hello eredmények megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="819e2-163">Finally, we execute a few search queries and display hello results.</span></span> <span data-ttu-id="819e2-164">Most használjuk a különböző `SearchIndexClient`:</span><span class="sxs-lookup"><span data-stu-id="819e2-164">This time we use a different `SearchIndexClient`:</span></span>

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

<span data-ttu-id="819e2-165">Most elindítjuk hello közelebbről `RunQueries` később metódust.</span><span class="sxs-lookup"><span data-stu-id="819e2-165">We will take a closer look at hello `RunQueries` method later.</span></span> <span data-ttu-id="819e2-166">Itt egy hello kód toocreate hello új `SearchIndexClient`:</span><span class="sxs-lookup"><span data-stu-id="819e2-166">Here is hello code toocreate hello new `SearchIndexClient`:</span></span>

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

<span data-ttu-id="819e2-167">Most használjuk a lekérdezési kulcs óta nem kell írási hozzáférést toohello index.</span><span class="sxs-lookup"><span data-stu-id="819e2-167">This time we use a query key since we do not need write access toohello index.</span></span> <span data-ttu-id="819e2-168">Ezt az információt is beírhatja hello `appsettings.json` hello fájljának [mintaalkalmazás](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span><span class="sxs-lookup"><span data-stu-id="819e2-168">You can enter this information in hello `appsettings.json` file of hello [sample application](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).</span></span>

<span data-ttu-id="819e2-169">Ha az alkalmazás futtatásához a egy érvényes szolgáltatásnevet és API-kulcsokat, hello kimeneti kell néznek ki:</span><span class="sxs-lookup"><span data-stu-id="819e2-169">If you run this application with a valid service name and API keys, hello output should look like this:</span></span>

    Deleting index...
    
    Creating index...
    
    Uploading documents...
    
    Waiting for documents toobe indexed...
    
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
    
    Complete.  Press any key tooend application...

<span data-ttu-id="819e2-170">Ez a cikk végén hello hello teljes forráskód hello alkalmazás valósul meg.</span><span class="sxs-lookup"><span data-stu-id="819e2-170">hello full source code of hello application is provided at hello end of this article.</span></span>

<span data-ttu-id="819e2-171">A következő most elindítjuk által meghívott hello módszerek részletes bemutatása `Main`.</span><span class="sxs-lookup"><span data-stu-id="819e2-171">Next, we will take a closer look at each of hello methods called by `Main`.</span></span>

### <a name="creating-an-index"></a><span data-ttu-id="819e2-172">Az index létrehozása</span><span class="sxs-lookup"><span data-stu-id="819e2-172">Creating an index</span></span>
<span data-ttu-id="819e2-173">Létrehozása után egy `SearchServiceClient`, következő lépésként hello `Main` does van a delete hello "Hotels"nevű index, ha már létezik.</span><span class="sxs-lookup"><span data-stu-id="819e2-173">After creating a `SearchServiceClient`, hello next thing `Main` does is delete hello "hotels" index if it already exists.</span></span> <span data-ttu-id="819e2-174">Amely a következő metódus hello végzi el:</span><span class="sxs-lookup"><span data-stu-id="819e2-174">That is done by hello following method:</span></span>

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
    }
}
```

<span data-ttu-id="819e2-175">Ezt a módszert használja a megadott hello `SearchServiceClient` toocheck hello index létezik, és ha igen, törölje azt.</span><span class="sxs-lookup"><span data-stu-id="819e2-175">This method uses hello given `SearchServiceClient` toocheck if hello index exists, and if so, delete it.</span></span>

> [!NOTE]
> <span data-ttu-id="819e2-176">hello példakódot ebben a cikkben az egyszerűség kedvéért hello hello Azure Search .NET SDK szinkron módszereit használja.</span><span class="sxs-lookup"><span data-stu-id="819e2-176">hello example code in this article uses hello synchronous methods of hello Azure Search .NET SDK for simplicity.</span></span> <span data-ttu-id="819e2-177">Javasoljuk, hogy a saját alkalmazások tookeep hello aszinkron metódusok használja őket a méretezhető és rugalmas.</span><span class="sxs-lookup"><span data-stu-id="819e2-177">We recommend that you use hello asynchronous methods in your own applications tookeep them scalable and responsive.</span></span> <span data-ttu-id="819e2-178">Például a hello metódus fent, rendszerrel lehetett `ExistsAsync` és `DeleteAsync` helyett `Exists` és `Delete`.</span><span class="sxs-lookup"><span data-stu-id="819e2-178">For example, in hello method above you could use `ExistsAsync` and `DeleteAsync` instead of `Exists` and `Delete`.</span></span>
> 
> 

<span data-ttu-id="819e2-179">Ezt követően `Main` hoz létre egy új "Hotels nevű" index által a metódus hívása:</span><span class="sxs-lookup"><span data-stu-id="819e2-179">Next, `Main` creates a new "hotels" index by calling this method:</span></span>

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

<span data-ttu-id="819e2-180">Ezzel a módszerrel hoz létre egy új `Index` listáját tartalmazó objektum `Field` hello séma hello új index definiáló objektumok.</span><span class="sxs-lookup"><span data-stu-id="819e2-180">This method creates a new `Index` object with a list of `Field` objects that defines hello schema of hello new index.</span></span> <span data-ttu-id="819e2-181">Egyes mezők nevét, adattípus és a keresés viselkedését meghatározó számos attribútum rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="819e2-181">Each field has a name, data type, and several attributes that define its search behavior.</span></span> <span data-ttu-id="819e2-182">Hello `FieldBuilder` osztály reflexiós toocreate listáját használja `Field` hello index megvizsgálásával objektumok hello nyilvános tulajdonságokat és a megadott hello attribútumait `Hotel` osztály modell.</span><span class="sxs-lookup"><span data-stu-id="819e2-182">hello `FieldBuilder` class uses reflection toocreate a list of `Field` objects for hello index by examining hello public properties and attributes of hello given `Hotel` model class.</span></span> <span data-ttu-id="819e2-183">Hello közelebbről lesz vesszük `Hotel` osztály később.</span><span class="sxs-lookup"><span data-stu-id="819e2-183">We'll take a closer look at hello `Hotel` class later on.</span></span>

> [!NOTE]
> <span data-ttu-id="819e2-184">Bármikor létrehozhat hello listája `Field` objektumok közvetlenül helyett `FieldBuilder` szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="819e2-184">You can always create hello list of `Field` objects directly instead of using `FieldBuilder` if needed.</span></span> <span data-ttu-id="819e2-185">Például kívánt toouse egy modellosztállyal, vagy egy meglévő modellosztállyal, hogy nem szeretné, hogy toomodify attribútumok hozzáadásával toouse szükség lehet.</span><span class="sxs-lookup"><span data-stu-id="819e2-185">For example, you may not want toouse a model class or you may need toouse an existing model class that you don't want toomodify by adding attributes.</span></span>
>
> 

<span data-ttu-id="819e2-186">Ezenkívül toofields, azt is megteheti a pontozási profil, a javaslattevők vagy a CORS-beállítások toohello Index (ezek hiányoznak kivonatosan mutatja hello mintából).</span><span class="sxs-lookup"><span data-stu-id="819e2-186">In addition toofields, you can also add scoring profiles, suggesters, or CORS options toohello Index (these are omitted from hello sample for brevity).</span></span> <span data-ttu-id="819e2-187">További információ a hello Index objektum és azok részlegei található hello [SDK referenciáit](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index), valamint az hello [Azure Search REST API-referencia](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="819e2-187">You can find more information about hello Index object and its constituent parts in hello [SDK reference](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index), as well as in hello [Azure Search REST API reference](https://docs.microsoft.com/rest/api/searchservice/).</span></span>

### <a name="populating-hello-index"></a><span data-ttu-id="819e2-188">Hello index feltöltése</span><span class="sxs-lookup"><span data-stu-id="819e2-188">Populating hello index</span></span>
<span data-ttu-id="819e2-189">következő lépése hello `Main` toopopulate hello újonnan létrehozott index.</span><span class="sxs-lookup"><span data-stu-id="819e2-189">hello next step in `Main` is toopopulate hello newly-created index.</span></span> <span data-ttu-id="819e2-190">Ez a módszer a következő hello történik:</span><span class="sxs-lookup"><span data-stu-id="819e2-190">This is done in hello following method:</span></span>

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
            Description = "Close tootown hall and hello river"
        }
    };

    var batch = IndexBatch.Upload(hotels);

    try
    {
        indexClient.Documents.Index(batch);
    }
    catch (IndexBatchException e)
    {
        // Sometimes when your Search service is under load, indexing will fail for some of hello documents in
        // hello batch. Depending on your application, you can take compensating actions like delaying and
        // retrying. For this simple demo, we just log hello failed document keys and continue.
        Console.WriteLine(
            "Failed tooindex some of hello documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

    Console.WriteLine("Waiting for documents toobe indexed...\n");
    Thread.Sleep(2000);
}
```

<span data-ttu-id="819e2-191">Ez a metódus négy részből áll.</span><span class="sxs-lookup"><span data-stu-id="819e2-191">This method has four parts.</span></span> <span data-ttu-id="819e2-192">hello először létrehoz egy tömbjét `Hotel` objektumok bemeneti adatok tooupload toohello indexben erre a célra.</span><span class="sxs-lookup"><span data-stu-id="819e2-192">hello first creates an array of `Hotel` objects that will serve as our input data tooupload toohello index.</span></span> <span data-ttu-id="819e2-193">Ezek az adatok nem változtatható az egyszerűség érdekében.</span><span class="sxs-lookup"><span data-stu-id="819e2-193">This data is hard-coded for simplicity.</span></span> <span data-ttu-id="819e2-194">A saját alkalmazásban az adatok valószínűleg határozza meg egy külső adatforrásból, például az SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="819e2-194">In your own application, your data will likely come from an external data source such as a SQL database.</span></span>

<span data-ttu-id="819e2-195">hello második rész létrehoz egy `IndexBatch` hello dokumentumokat tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="819e2-195">hello second part creates an `IndexBatch` containing hello documents.</span></span> <span data-ttu-id="819e2-196">Megadhatja a kívánt tooapply toohello kötegelt hoz létre, ebben az esetben meghívásával hello időpontban hello művelet `IndexBatch.Upload`.</span><span class="sxs-lookup"><span data-stu-id="819e2-196">You specify hello operation you want tooapply toohello batch at hello time you create it, in this case by calling `IndexBatch.Upload`.</span></span> <span data-ttu-id="819e2-197">hello kötegelt nem feltöltött toohello Azure Search-index által hello `Documents.Index` metódust.</span><span class="sxs-lookup"><span data-stu-id="819e2-197">hello batch is then uploaded toohello Azure Search index by hello `Documents.Index` method.</span></span>

> [!NOTE]
> <span data-ttu-id="819e2-198">Ebben a példában azt csak feltölteni dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="819e2-198">In this example, we are just uploading documents.</span></span> <span data-ttu-id="819e2-199">Ha meglévő vagy delete dokumentumokat toomerge változik, létrehozhat meghívásával kötegek `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, vagy `IndexBatch.Delete` helyette.</span><span class="sxs-lookup"><span data-stu-id="819e2-199">If you wanted toomerge changes into existing documents or delete documents, you could create batches by calling `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, or `IndexBatch.Delete` instead.</span></span> <span data-ttu-id="819e2-200">A kötegek különböző műveletei adjunk meghívásával `IndexBatch.New`, gyűjteménye, amely veszi `IndexAction` objektumok, amelyek mindegyike közli az Azure Search tooperform a dokumentum egy adott művelethez.</span><span class="sxs-lookup"><span data-stu-id="819e2-200">You can also mix different operations in a single batch by calling `IndexBatch.New`, which takes a collection of `IndexAction` objects, each of which tells Azure Search tooperform a particular operation on a document.</span></span> <span data-ttu-id="819e2-201">Minden egyes hozhat létre `IndexAction` saját műveletet hello megfelelő metódus meghívásával `IndexAction.Merge`, `IndexAction.Upload`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="819e2-201">You can create each `IndexAction` with its own operation by calling hello corresponding method such as `IndexAction.Merge`, `IndexAction.Upload`, and so on.</span></span>
> 
> 

<span data-ttu-id="819e2-202">hello harmadik ezt a módszert része egy catch blokkba, amely kezeli az egyik fontos hiba esetében az indexeléshez.</span><span class="sxs-lookup"><span data-stu-id="819e2-202">hello third part of this method is a catch block that handles an important error case for indexing.</span></span> <span data-ttu-id="819e2-203">Ha az Azure Search szolgáltatás sikertelen tooindex néhány hello hello kötegben, dokumentumok egy `IndexBatchException` által okozott `Documents.Index`.</span><span class="sxs-lookup"><span data-stu-id="819e2-203">If your Azure Search service fails tooindex some of hello documents in hello batch, an `IndexBatchException` is thrown by `Documents.Index`.</span></span> <span data-ttu-id="819e2-204">Ez akkor történhet meg, ha olyankor végzi a dokumentumok indexelését, amikor a szolgáltatás nagy terhelés alatt áll.</span><span class="sxs-lookup"><span data-stu-id="819e2-204">This can happen if you are indexing documents while your service is under heavy load.</span></span> <span data-ttu-id="819e2-205">**Javasoljuk, hogy a kódban explicit módon kezelje ezt az esetet.**</span><span class="sxs-lookup"><span data-stu-id="819e2-205">**We strongly recommend explicitly handling this case in your code.**</span></span> <span data-ttu-id="819e2-206">Késleltetés, és próbálkozzon újra a sikertelen indexelési hello dokumentumok, vagy jelentkezzen és például hello minta nem, vagy valami mást, attól függően, hogy az alkalmazás konzisztenciájának követelményeinek elvégezhető folytatásához.</span><span class="sxs-lookup"><span data-stu-id="819e2-206">You can delay and then retry indexing hello documents that failed, or you can log and continue like hello sample does, or you can do something else depending on your application's data consistency requirements.</span></span>

> [!NOTE]
> <span data-ttu-id="819e2-207">Használhatja a hello `FindFailedActionsToRetry` tartalmazó új kötegelt metódus tooconstruct csak hello túl egy korábbi hívás sikertelen műveletek`Index`.</span><span class="sxs-lookup"><span data-stu-id="819e2-207">You can use hello `FindFailedActionsToRetry` method tooconstruct a new batch containing only hello actions that failed in a previous call too`Index`.</span></span> <span data-ttu-id="819e2-208">hello metódus dokumentált [Itt](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) , és egy tooproperly használatának értékelése [a StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).</span><span class="sxs-lookup"><span data-stu-id="819e2-208">hello method is documented [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) and there is a discussion of how tooproperly use it [on StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).</span></span>
>
>

<span data-ttu-id="819e2-209">Végezetül hello `UploadDocuments` két másodpercen metódus késést.</span><span class="sxs-lookup"><span data-stu-id="819e2-209">Finally, hello `UploadDocuments` method delays for two seconds.</span></span> <span data-ttu-id="819e2-210">Indexelő történik aszinkron módon történik az Azure Search szolgáltatásban, így hello mintaalkalmazás kell toowait egy rövid időn belül tooensure, hogy a keresés hello dokumentumok is.</span><span class="sxs-lookup"><span data-stu-id="819e2-210">Indexing happens asynchronously in your Azure Search service, so hello sample application needs toowait a short time tooensure that hello documents are available for searching.</span></span> <span data-ttu-id="819e2-211">Ilyen mértékű késleltetésre kizárólag demók, tesztek és mintaalkalmazások esetében van szükség.</span><span class="sxs-lookup"><span data-stu-id="819e2-211">Delays like this are typically only necessary in demos, tests, and sample applications.</span></span>

#### <a name="how-hello-net-sdk-handles-documents"></a><span data-ttu-id="819e2-212">Hogyan kezeli a .NET SDK hello a dokumentumok</span><span class="sxs-lookup"><span data-stu-id="819e2-212">How hello .NET SDK handles documents</span></span>
<span data-ttu-id="819e2-213">Előfordulhat, hogy lehet szeretné megtudni, hogy hogyan hello Azure Search .NET SDK például a felhasználói osztály példányainak képes tooupload `Hotel` toohello index.</span><span class="sxs-lookup"><span data-stu-id="819e2-213">You may be wondering how hello Azure Search .NET SDK is able tooupload instances of a user-defined class like `Hotel` toohello index.</span></span> <span data-ttu-id="819e2-214">toohelp válaszolja meg, hogy a kérdést, nézzük hello `Hotel` osztály:</span><span class="sxs-lookup"><span data-stu-id="819e2-214">toohelp answer that question, let's look at hello `Hotel` class:</span></span>

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// hello SerializePropertyNamesAsCamelCase attribute is defined in hello Azure Search .NET SDK.
// It ensures that Pascal-case property names in hello model class are mapped toocamel-case
// field names in hello index.
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

<span data-ttu-id="819e2-215">hello először thing toonotice, hogy minden egyes nyilvános tulajdonsága `Hotel` felel meg a hello Indexdefiníció, de egy fontos különbséggel tooa mező: hello minden mező kezdetű névvel rendelkező visszaalakítja ("teve eset"), közben minden nyilvános hello neve a tulajdonság `Hotel` egy nagybetűt ("Pascal eset") kezdődik.</span><span class="sxs-lookup"><span data-stu-id="819e2-215">hello first thing toonotice is that each public property of `Hotel` corresponds tooa field in hello index definition, but with one crucial difference: hello name of each field starts with a lower-case letter ("camel case"), while hello name of each public property of `Hotel` starts with an upper-case letter ("Pascal case").</span></span> <span data-ttu-id="819e2-216">Ez az a közös hajtható végre adatkötés, ahol hello cél séma az alkalmazásfejlesztő hello külső hello irányítását .NET-alkalmazások esetén.</span><span class="sxs-lookup"><span data-stu-id="819e2-216">This is a common scenario in .NET applications that perform data-binding where hello target schema is outside hello control of hello application developer.</span></span> <span data-ttu-id="819e2-217">Ahelyett, hogy a tooviolate hello .NET irányelvek elnevezési azáltal, hogy a tulajdonság nevét teve eset, megadható, hogy a hello SDK toomap hello tulajdonság nevének toocamel-kódaláírásokra automatikusan hello `[SerializePropertyNamesAsCamelCase]` attribútum.</span><span class="sxs-lookup"><span data-stu-id="819e2-217">Rather than having tooviolate hello .NET naming guidelines by making property names camel-case, you can tell hello SDK toomap hello property names toocamel-case automatically with hello `[SerializePropertyNamesAsCamelCase]` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="819e2-218">hello Azure Search .NET SDK-t használ a hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) könyvtár tooserialize és deszerializálható JSON formátumból az egyéni modell objektumok tooand.</span><span class="sxs-lookup"><span data-stu-id="819e2-218">hello Azure Search .NET SDK uses hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) library tooserialize and deserialize your custom model objects tooand from JSON.</span></span> <span data-ttu-id="819e2-219">A szerializálás szükség szerint testre szabható.</span><span class="sxs-lookup"><span data-stu-id="819e2-219">You can customize this serialization if needed.</span></span> <span data-ttu-id="819e2-220">További részletekért lásd: [JSON.NET az egyéni szerializálási](#JsonDotNet).</span><span class="sxs-lookup"><span data-stu-id="819e2-220">For more details, see [Custom Serialization with JSON.NET](#JsonDotNet).</span></span>
> 
> 

<span data-ttu-id="819e2-221">hello második lépésként toonotice hello attribútumok például `IsFilterable`, `IsSearchable`, `Key`, és `Analyzer` , amely minden egyes nyilvános tulajdonsága adja.</span><span class="sxs-lookup"><span data-stu-id="819e2-221">hello second thing toonotice are hello attributes such as `IsFilterable`, `IsSearchable`, `Key`, and `Analyzer` that decorate each public property.</span></span> <span data-ttu-id="819e2-222">Ezek az attribútumok hozzárendelését közvetlenül toohello [hello Azure Search-index megfelelő attribútumait](https://docs.microsoft.com/rest/api/searchservice/create-index#request).</span><span class="sxs-lookup"><span data-stu-id="819e2-222">These attributes map directly toohello [corresponding attributes of hello Azure Search index](https://docs.microsoft.com/rest/api/searchservice/create-index#request).</span></span> <span data-ttu-id="819e2-223">Hello `FieldBuilder` osztály hello index használja e tooconstruct mező definíciókat.</span><span class="sxs-lookup"><span data-stu-id="819e2-223">hello `FieldBuilder` class uses these tooconstruct field definitions for hello index.</span></span>

<span data-ttu-id="819e2-224">hello kapcsolatos fontos dolog harmadik hello `Hotel` osztály hello adattípusok hello nyilvános tulajdonságainak.</span><span class="sxs-lookup"><span data-stu-id="819e2-224">hello third important thing about hello `Hotel` class are hello data types of hello public properties.</span></span> <span data-ttu-id="819e2-225">hello .NET ezeket a tulajdonságokat képezi le hello Indexdefiníció tootheir egyenértékű mező típusa.</span><span class="sxs-lookup"><span data-stu-id="819e2-225">hello .NET types of  these properties map tootheir equivalent field types in hello index definition.</span></span> <span data-ttu-id="819e2-226">Például hello `Category` karakterlánc típusú tulajdonság leképezhető toohello `category` mező, amelynek a típusa `Edm.String`.</span><span class="sxs-lookup"><span data-stu-id="819e2-226">For example, hello `Category` string property maps toohello `category` field, which is of type `Edm.String`.</span></span> <span data-ttu-id="819e2-227">Hasonló típusú hozzárendelés közötti `bool?` és `Edm.Boolean`, `DateTimeOffset?` és `Edm.DateTimeOffset`, stb. hello speciális szabályok a következő típusleképezéshez hello szerepelnek a hello `Documents.Get` metódus a hello [Azure Search .NET SDK hivatkozás](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="819e2-227">There are similar type mappings between `bool?` and `Edm.Boolean`, `DateTimeOffset?` and `Edm.DateTimeOffset`, etc. hello specific rules for hello type mapping are documented with hello `Documents.Get` method in hello [Azure Search .NET SDK reference](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span></span> <span data-ttu-id="819e2-228">Hello `FieldBuilder` osztály gondoskodik meg ezt a hozzárendelést, de továbbra is lehet hasznos toounderstand arra az esetre kell tootroubleshoot szerializálási probléma merül fel.</span><span class="sxs-lookup"><span data-stu-id="819e2-228">hello `FieldBuilder` class takes care of this mapping for you, but it can still be helpful toounderstand in case you need tootroubleshoot any serialization issues.</span></span>

<span data-ttu-id="819e2-229">Ez lehetőséget toouse saját osztályok-dokumentumokként működik mindkét irányban; Keresési eredmények beolvasásához és is rendelkezik hello SDK automatikusan deszerializálni őket tooa típus az Ön által választott, a következő szakaszban hello lesz látható.</span><span class="sxs-lookup"><span data-stu-id="819e2-229">This ability toouse your own classes as documents works in both directions; You can also retrieve search results and have hello SDK automatically deserialize them tooa type of your choice, as we will see in hello next section.</span></span>

> [!NOTE]
> <span data-ttu-id="819e2-230">hello Azure Search .NET SDK-t is támogatja a hello segítségével dinamikusan gépelt dokumentumok `Document` osztályt, amely nevek toofield mezőértékek kulcs/érték hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="819e2-230">hello Azure Search .NET SDK also supports dynamically-typed documents using hello `Document` class, which is a key/value mapping of field names toofield values.</span></span> <span data-ttu-id="819e2-231">Ez olyan esetekben hasznos, ha hello a sémát indexeli tervezéskor nem tudja, vagy ha kényelmetlen toobind toospecific modell osztályok lenne.</span><span class="sxs-lookup"><span data-stu-id="819e2-231">This is useful in scenarios where you don't know hello index schema at design-time, or where it would be inconvenient toobind toospecific model classes.</span></span> <span data-ttu-id="819e2-232">Minden hello módszerek hello SDK a dokumentumok kezelése rendelkezik, amelyek használhatók a hello túlterhelések `Document` osztály, valamint az erős típusmegadású túlterhelések, amelyek általános típusú paramétert.</span><span class="sxs-lookup"><span data-stu-id="819e2-232">All hello methods in hello SDK that deal with documents have overloads that work with hello `Document` class, as well as strongly-typed overloads that take a generic type parameter.</span></span> <span data-ttu-id="819e2-233">Ez utóbbi csak hello hello mintakód ebben az oktatóanyagban használt.</span><span class="sxs-lookup"><span data-stu-id="819e2-233">Only hello latter are used in hello sample code in this tutorial.</span></span> <span data-ttu-id="819e2-234">Hello `Document` osztály örökli `Dictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="819e2-234">hello `Document` class inherits from `Dictionary<string, object>`.</span></span> <span data-ttu-id="819e2-235">Más részletei [Itt](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).</span><span class="sxs-lookup"><span data-stu-id="819e2-235">You can find other details [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).</span></span>
> 
> 

<span data-ttu-id="819e2-236">**Miért használjon nullázható adattípusokat?**</span><span class="sxs-lookup"><span data-stu-id="819e2-236">**Why you should use nullable data types**</span></span>

<span data-ttu-id="819e2-237">A saját modell osztályok toomap tooan Azure Search-index tervezésekor ajánlott érték típusú tulajdonságok például deklaráló `bool` és `int` toobe nullázható (például `bool?` helyett `bool`).</span><span class="sxs-lookup"><span data-stu-id="819e2-237">When designing your own model classes toomap tooan Azure Search index, we recommend declaring properties of value types such as `bool` and `int` toobe nullable (for example, `bool?` instead of `bool`).</span></span> <span data-ttu-id="819e2-238">Ha egy nem nullázható tulajdonságot használja, akkor túl**garantálni** , hogy az indexben nem dokumentumok hello megfelelő mező null értéket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="819e2-238">If you use a non-nullable property, you have too**guarantee** that no documents in your index contain a null value for hello corresponding field.</span></span> <span data-ttu-id="819e2-239">Hello SDK sem hello Azure Search szolgáltatás segítségével Ön tooenforce ez.</span><span class="sxs-lookup"><span data-stu-id="819e2-239">Neither hello SDK nor hello Azure Search service will help you tooenforce this.</span></span>

<span data-ttu-id="819e2-240">Ez nem csak egy elméleti szempont: képzelhető el, egy olyan forgatókönyvet, ahol hozzáadhat egy új tooan meglévő mezőindex, amely típusú `Edm.Int32`.</span><span class="sxs-lookup"><span data-stu-id="819e2-240">This is not just a hypothetical concern: Imagine a scenario where you add a new field tooan existing index that is of type `Edm.Int32`.</span></span> <span data-ttu-id="819e2-241">Hello index definíciójának frissítése után összes dokumentumot lesz az új mező null értéket (mivel az Azure Search nullázható minden esetében).</span><span class="sxs-lookup"><span data-stu-id="819e2-241">After updating hello index definition, all documents will have a null value for that new field (since all types are nullable in Azure Search).</span></span> <span data-ttu-id="819e2-242">Ha az egy nem nullázható majd használja egy modell osztály `int` tulajdonság, mező, elérhetővé válik egy `JsonSerializationException` ez, például tooretrieve dokumentumok tett kísérlet során:</span><span class="sxs-lookup"><span data-stu-id="819e2-242">If you then use a model class with a non-nullable `int` property for that field, you will get a `JsonSerializationException` like this when trying tooretrieve documents:</span></span>

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

<span data-ttu-id="819e2-243">Ezért javasoljuk, hogy a modellosztályokban nullázható értéktípusokat használjon.</span><span class="sxs-lookup"><span data-stu-id="819e2-243">For this reason, we recommend that you use nullable types in your model classes as a best practice.</span></span>

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a><span data-ttu-id="819e2-244">A JSON.NET egyedi szerializálás</span><span class="sxs-lookup"><span data-stu-id="819e2-244">Custom Serialization with JSON.NET</span></span>
<span data-ttu-id="819e2-245">hello SDK JSON.NET szerializálása és deszerializálása dokumentumok használ.</span><span class="sxs-lookup"><span data-stu-id="819e2-245">hello SDK uses JSON.NET for serializing and deserializing documents.</span></span> <span data-ttu-id="819e2-246">Testre szabhatja a szerializálás és szükség esetén meghatározhat egy saját deszerializálási `JsonConverter` vagy `IContractResolver` (lásd: hello [JSON.NET dokumentáció](http://www.newtonsoft.com/json/help/html/Introduction.htm) további részletekért).</span><span class="sxs-lookup"><span data-stu-id="819e2-246">You can customize serialization and deserialization if needed by defining your own `JsonConverter` or `IContractResolver` (see hello [JSON.NET documentation](http://www.newtonsoft.com/json/help/html/Introduction.htm) for more details).</span></span> <span data-ttu-id="819e2-247">Ez akkor lehet hasznos, ha azt szeretné, egy meglévő modellosztállyal tooadapt az alkalmazásból az Azure Search és egyéb speciális forgatókönyvekhez használható.</span><span class="sxs-lookup"><span data-stu-id="819e2-247">This can be useful when you want tooadapt an existing model class from your application for use with Azure Search, and other more advanced scenarios.</span></span> <span data-ttu-id="819e2-248">Például az egyedi szerializálás lehetővé teszi:</span><span class="sxs-lookup"><span data-stu-id="819e2-248">For example, with custom serialization you can:</span></span>

* <span data-ttu-id="819e2-249">Tartalmaznak, vagy zárja ki a modell osztály egyes tulajdonságai dokumentum mezői tárolt.</span><span class="sxs-lookup"><span data-stu-id="819e2-249">Include or exclude certain properties of your model class from being stored as document fields.</span></span>
* <span data-ttu-id="819e2-250">Képezze le a kódban nevei és az indexben mezőnevek között.</span><span class="sxs-lookup"><span data-stu-id="819e2-250">Map between property names in your code and field names in your index.</span></span>
* <span data-ttu-id="819e2-251">Hozzon létre egyéni attribútumok csak tulajdonságok toodocument mezők leképezése is használható.</span><span class="sxs-lookup"><span data-stu-id="819e2-251">Create custom attributes that can be used for mapping properties toodocument fields.</span></span>

<span data-ttu-id="819e2-252">Egyedi szerializálás implementációi hello egység vizsgálatokra hello Azure Search .NET SDK a Githubon található.</span><span class="sxs-lookup"><span data-stu-id="819e2-252">You can find examples of implementing custom serialization in hello unit tests for hello Azure Search .NET SDK on GitHub.</span></span> <span data-ttu-id="819e2-253">A jó kiindulási pont [Ez a mappa](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models).</span><span class="sxs-lookup"><span data-stu-id="819e2-253">A good starting point is [this folder](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models).</span></span> <span data-ttu-id="819e2-254">Hello egyedi szerializálás tesztek által használt osztályok tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="819e2-254">It contains classes that are used by hello custom serialization tests.</span></span>

### <a name="searching-for-documents-in-hello-index"></a><span data-ttu-id="819e2-255">Hello index dokumentumok keresése</span><span class="sxs-lookup"><span data-stu-id="819e2-255">Searching for documents in hello index</span></span>
<span data-ttu-id="819e2-256">hello mintaalkalmazás hello utolsó lépése a bizonyos dokumentumok hello index toosearch.</span><span class="sxs-lookup"><span data-stu-id="819e2-256">hello last step in hello sample application is toosearch for some documents in hello index.</span></span> <span data-ttu-id="819e2-257">a következő metódus hello hajtja végre ezt:</span><span class="sxs-lookup"><span data-stu-id="819e2-257">hello following method does this:</span></span>

```csharp
private static void RunQueries(ISearchIndexClient indexClient)
{
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
}
```

<span data-ttu-id="819e2-258">Minden alkalommal, amikor végrehajtja a lekérdezést, ez a módszer először létrehoz egy új `SearchParameters` objektum.</span><span class="sxs-lookup"><span data-stu-id="819e2-258">Each time it executes a query, this method first creates a new `SearchParameters` object.</span></span> <span data-ttu-id="819e2-259">Ez a használt toospecify további beállításokat, például a rendezési, szűrési, lapozás és értékkorlátozás hello lekérdezéshez.</span><span class="sxs-lookup"><span data-stu-id="819e2-259">This is used toospecify additional options for hello query such as sorting, filtering, paging, and faceting.</span></span> <span data-ttu-id="819e2-260">Ezzel a módszerrel hello beállítás van `Filter`, `Select`, `OrderBy`, és `Top` különböző lekérdezések tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="819e2-260">In this method, we're setting hello `Filter`, `Select`, `OrderBy`, and `Top` property for different queries.</span></span> <span data-ttu-id="819e2-261">Minden hello `SearchParameters` tulajdonságok szerepelnek [Itt](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).</span><span class="sxs-lookup"><span data-stu-id="819e2-261">All hello `SearchParameters` properties are documented [here](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).</span></span>

<span data-ttu-id="819e2-262">következő lépés hello tooactually hello keresési lekérdezés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="819e2-262">hello next step is tooactually execute hello search query.</span></span> <span data-ttu-id="819e2-263">Ebben az esetben hello segítségével `Documents.Search` metódust.</span><span class="sxs-lookup"><span data-stu-id="819e2-263">This is done using hello `Documents.Search` method.</span></span> <span data-ttu-id="819e2-264">Minden egyes lekérdezés esetén, azt át hello keresési szöveg toouse karakterláncként (vagy `"*"` nincs a keresési szöveget esetén), és keresse meg a korábban létrehozott paraméterek hello.</span><span class="sxs-lookup"><span data-stu-id="819e2-264">For each query, we pass hello search text toouse as a string (or `"*"` if there is no search text), plus hello search parameters created earlier.</span></span> <span data-ttu-id="819e2-265">Azt is megadhatja, `Hotel` a hello típust paraméterként `Documents.Search`, amely közli hello SDK toodeserialize dokumentumok hello keresési eredmények között a következő típusú objektumokat a `Hotel`.</span><span class="sxs-lookup"><span data-stu-id="819e2-265">We also specify `Hotel` as hello type parameter for `Documents.Search`, which tells hello SDK toodeserialize documents in hello search results into objects of type `Hotel`.</span></span>

> [!NOTE]
> <span data-ttu-id="819e2-266">Hello keresési lekérdezés kifejezés szintaxissal kapcsolatos további információ található [Itt](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="819e2-266">You can find more information about hello search query expression syntax [here](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).</span></span>
> 
> 

<span data-ttu-id="819e2-267">Végül minden egyes lekérdezés után ez a módszer telepítéseket összes hello találat hello keresési eredmények között, minden egyes dokumentum toohello konzol nyomtatás:</span><span class="sxs-lookup"><span data-stu-id="819e2-267">Finally, after each query this method iterates through all hello matches in hello search results, printing each document toohello console:</span></span>

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

<span data-ttu-id="819e2-268">Vegyük pedig egyes hello lekérdezések részletes bemutatása.</span><span class="sxs-lookup"><span data-stu-id="819e2-268">Let's take a closer look at each of hello queries in turn.</span></span> <span data-ttu-id="819e2-269">Hello kód tooexecute hello első lekérdezés a következő:</span><span class="sxs-lookup"><span data-stu-id="819e2-269">Here is hello code tooexecute hello first query:</span></span>

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

<span data-ttu-id="819e2-270">Ebben az esetben azt keresünk, amelyek megfelelnek a hello word "költségvetés" hotels, és szeretnénk tooget vissza csak hello Szálloda nevek, hello által megadott `Select` paraméter.</span><span class="sxs-lookup"><span data-stu-id="819e2-270">In this case, we're searching for hotels that match hello word "budget", and we want tooget back only hello hotel names, as specified by hello `Select` parameter.</span></span> <span data-ttu-id="819e2-271">Az alábbiakban hello eredmények:</span><span class="sxs-lookup"><span data-stu-id="819e2-271">Here are hello results:</span></span>

    Name: Roach Motel

<span data-ttu-id="819e2-272">Ezt követően azt szeretné, hogy egy éjszakánként sebessége kisebb, mint 150 $ toofind hello szállodák, és térjen vissza a csak a hello Szálloda azonosító és a Leírás:</span><span class="sxs-lookup"><span data-stu-id="819e2-272">Next, we want toofind hello hotels with a nightly rate of less than $150, and return only hello hotel ID and description:</span></span>

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

<span data-ttu-id="819e2-273">Ez a lekérdezés használ egy OData `$filter` kifejezését `baseRate lt 150`, toofilter hello dokumentumok hello index.</span><span class="sxs-lookup"><span data-stu-id="819e2-273">This query uses an OData `$filter` expression, `baseRate lt 150`, toofilter hello documents in hello index.</span></span> <span data-ttu-id="819e2-274">További információk a hello, amely támogatja az Azure Search OData-szintaxis található [Itt](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).</span><span class="sxs-lookup"><span data-stu-id="819e2-274">You can find out more about hello OData syntax that Azure Search supports [here](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).</span></span>

<span data-ttu-id="819e2-275">Az alábbiakban hello lekérdezési eredmények hello:</span><span class="sxs-lookup"><span data-stu-id="819e2-275">Here are hello results of hello query:</span></span>

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close tootown hall and hello river

<span data-ttu-id="819e2-276">A következő szeretnénk toofind hello felső két szállodák, amely rendelkezik lett utoljára felújított, hello Szálloda neve és az utolsó felújítás dátumának megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="819e2-276">Next, we want toofind hello top two hotels that have been most recently renovated, and show hello hotel name and last renovation date.</span></span> <span data-ttu-id="819e2-277">Íme hello kódot:</span><span class="sxs-lookup"><span data-stu-id="819e2-277">Here is hello code:</span></span> 

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

<span data-ttu-id="819e2-278">Ebben az esetben újra használjuk OData szintaxis toospecify hello `OrderBy` paraméterként `lastRenovationDate desc`.</span><span class="sxs-lookup"><span data-stu-id="819e2-278">In this case, we again use OData syntax toospecify hello `OrderBy` parameter as `lastRenovationDate desc`.</span></span> <span data-ttu-id="819e2-279">Azt is beállíthatja `Top` too2 tooensure jelenleg csak a get hello felső két dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="819e2-279">We also set `Top` too2 tooensure we only get hello top two documents.</span></span> <span data-ttu-id="819e2-280">Előtt, hogy állítsa `Select` toospecify mezőket vissza kell adni.</span><span class="sxs-lookup"><span data-stu-id="819e2-280">As before, we set `Select` toospecify which fields should be returned.</span></span>

<span data-ttu-id="819e2-281">Az alábbiakban hello eredmények:</span><span class="sxs-lookup"><span data-stu-id="819e2-281">Here are hello results:</span></span>

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

<span data-ttu-id="819e2-282">Végül azt szeretnénk toofind hello word "motel" megfelelő összes szállodák:</span><span class="sxs-lookup"><span data-stu-id="819e2-282">Finally, we want toofind all hotels that match hello word "motel":</span></span>

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

<span data-ttu-id="819e2-283">Az alábbiakban hello eredményeket, amely az összes mezők szerepelhetnek, mivel jelenleg nem adta meg a hello és `Select` tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="819e2-283">And here are hello results, which include all fields since we did not specify hello `Select` property:</span></span>

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

<span data-ttu-id="819e2-284">Ez a lépés befejezése hello oktatóanyag, de itt nem állnak le.</span><span class="sxs-lookup"><span data-stu-id="819e2-284">This step completes hello tutorial, but don't stop here.</span></span> <span data-ttu-id="819e2-285">**További lépések** Azure Search többet további forrásokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="819e2-285">**Next steps** provides additional resources for learning more about Azure Search.</span></span>

## <a name="next-steps"></a><span data-ttu-id="819e2-286">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="819e2-286">Next steps</span></span>
* <span data-ttu-id="819e2-287">Keresse meg a hivatkozásokat hello hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) és [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="819e2-287">Browse hello references for hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) and [REST API](https://docs.microsoft.com/rest/api/searchservice/).</span></span>
* <span data-ttu-id="819e2-288">A Tudásbázis keresztül elmélyítsék [videókat és más mintákat és oktatóanyagok](search-video-demo-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="819e2-288">Deepen your knowledge through [videos and other samples and tutorials](search-video-demo-tutorial-list.md).</span></span>
* <span data-ttu-id="819e2-289">Felülvizsgálati [elnevezési konvenciói](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) különböző objektumok elnevezési toolearn hello szabályai.</span><span class="sxs-lookup"><span data-stu-id="819e2-289">Review [naming conventions](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) toolearn hello rules for naming various objects.</span></span>
* <span data-ttu-id="819e2-290">Felülvizsgálati [a támogatott adattípusokat](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) az Azure Search.</span><span class="sxs-lookup"><span data-stu-id="819e2-290">Review [supported data types](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) in Azure Search.</span></span>
