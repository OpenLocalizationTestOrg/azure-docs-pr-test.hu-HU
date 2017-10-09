---
title: "egyidejű aaaHow toomanage tooresources ír az Azure Search"
description: "Egyidejű hozzáférések optimista tooavoid közepes vezeték nélkül ütközések frissítést vagy törlést tooAzure keresési indexek, az indexelők, az adatforrások használja."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 
ms.service: search
ms.devlang: 
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/21/2017
ms.author: heidist
ms.openlocfilehash: c061d2b5c4d2dbd0fd5633405b01ab2912fbc754
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-concurrency-in-azure-search"></a><span data-ttu-id="a7ba5-103">Hogyan toomanage egyidejűségi az Azure Search</span><span class="sxs-lookup"><span data-stu-id="a7ba5-103">How toomanage concurrency in Azure Search</span></span>

<span data-ttu-id="a7ba5-104">Azure Search erőforrások, például indexekkel és adatforrások kezelése, esetén fontos tooupdate erőforrások biztonságosan, különösen akkor, ha az erőforrás hozzáférése címtárfelhasználói egyidejűleg az alkalmazás különböző összetevői.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-104">When managing Azure Search resources such as indexes and data sources, it's important tooupdate resources safely, especially if resources are accessed concurrently by different components of your application.</span></span> <span data-ttu-id="a7ba5-105">Ha két ügyfelek egyidejűleg frissítés nélkül koordinációs erőforrás, versenyhelyzetek is előfordulhatnak.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-105">When two clients concurrently update a resource without coordination, race conditions are possible.</span></span> <span data-ttu-id="a7ba5-106">tooprevent ez, az Azure Search ajánlatok egy *egyidejű hozzáférések optimista modell*.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-106">tooprevent this, Azure Search offers an *optimistic concurrency model*.</span></span> <span data-ttu-id="a7ba5-107">Nincsenek zárolások erőforráson.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-107">There are no locks on a resource.</span></span> <span data-ttu-id="a7ba5-108">Ehelyett nincs egy ETag minden erőforrás, amely azonosítja a hello erőforrás-verziója, így Ön összeállíthatnak kérelmek elkerüléséhez véletlen felülírja.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-108">Instead, there is an ETag for every resource that identifies hello resource version so that you can craft requests that avoid accidental overwrites.</span></span>

> [!Tip]
> <span data-ttu-id="a7ba5-109">A fogalmi kód egy [C# megoldás minta](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) azt ismerteti, hogyan párhuzamossági működik az Azure Search.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-109">Conceptual code in a [sample C# solution](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) explains how concurrency control works in Azure Search.</span></span> <span data-ttu-id="a7ba5-110">hello kód hívása egyidejűség-vezérlési feltételek hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-110">hello code creates conditions that invoke concurrency control.</span></span> <span data-ttu-id="a7ba5-111">Olvasási hello [az alábbi kódrészlet](#samplecode) valószínűleg elegendőek a legtöbb fejlesztő, de ha kívánja toorun azt Szerkesztés appsettings.json tooadd hello szolgáltatás nevét és egy adminisztrációs api-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-111">Reading hello [code fragment below](#samplecode) is probably sufficient for most developers, but if you want toorun it, edit appsettings.json tooadd hello service name and an admin api-key.</span></span> <span data-ttu-id="a7ba5-112">A szolgáltatás URL-t adott `http://myservice.search.windows.net`, hello szolgáltatás neve megkülönbözteti a `myservice`.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-112">Given a service URL of `http://myservice.search.windows.net`, hello service name is `myservice`.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="a7ba5-113">Működés</span><span class="sxs-lookup"><span data-stu-id="a7ba5-113">How it works</span></span>

<span data-ttu-id="a7ba5-114">Optimista feldolgozási van megvalósítva keresztül hozzáférést feltétel ellenőrzi tooindexes, indexelők, adatforrások és synonymMap erőforrások írása API-hívásokban.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-114">Optimistic concurrency is implemented through access condition checks in API calls writing tooindexes, indexers, datasources, and synonymMap resources.</span></span> 

<span data-ttu-id="a7ba5-115">Minden erőforrás rendelkezik egy [ *entitáscímke (ETag)* ](https://en.wikipedia.org/wiki/HTTP_ETag) objektum fájlverzió-információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-115">All resources have an [*entity tag (ETag)*](https://en.wikipedia.org/wiki/HTTP_ETag) that provides object version information.</span></span> <span data-ttu-id="a7ba5-116">Ellenőrzésével hello ETag először, elkerülheti a párhuzamos frissítések egy tipikus munkafolyamatban (beolvasni, helyileg módosítása és frissítése) biztosításával a hello erőforrás ETag megegyezik a helyi példány.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-116">By checking hello ETag first, you can avoid concurrent updates in a typical workflow (get, modify locally, update) by ensuring hello resource's ETag matches your local copy.</span></span> 

+ <span data-ttu-id="a7ba5-117">hello REST API-t használ egy [ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) a hello kérelemfejlécet.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-117">hello REST API uses an [ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) on hello request header.</span></span>
+ <span data-ttu-id="a7ba5-118">hello .NET SDK beállítja hello ETag keresztül accessCondition objektum, hello beállítása [If-Match |} If-Match-nincs fejléc](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) hello erőforráson.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-118">hello .NET SDK sets hello ETag through an accessCondition object, setting hello [If-Match | If-Match-None header](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) on hello resource.</span></span> <span data-ttu-id="a7ba5-119">Összes objektumtól örököl [IResourceWithETag (.NET SDK-val)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) accessCondition objektum rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-119">Any object inheriting from [IResourceWithETag (.NET SDK)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) has an accessCondition object.</span></span>

<span data-ttu-id="a7ba5-120">Minden alkalommal, amikor frissít egy erőforrást, az ETag automatikus módosítja.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-120">Every time you update a resource, its ETag changes automatically.</span></span> <span data-ttu-id="a7ba5-121">Párhuzamossági felügyeleti bevezetésekor csak azt helyezi előfeltétel kérésre hello frissítés hello távoli erőforrás igénylő toohave hello azonos ETag hello másolatként hello ügyfélen módosító hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-121">When you implement concurrency management, all you're doing is putting a precondition on hello update request that requires hello remote resource toohave hello same ETag as hello copy of hello resource that you modified on hello client.</span></span> <span data-ttu-id="a7ba5-122">Ha egy egyidejű folyamat már hello távoli erőforrás változott, hello ETag nem fog egyezni hello előfeltétel, és hello kérelem sikertelen lesz, és HTTP 412.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-122">If a concurrent process has changed hello remote resource already, hello ETag will not match hello precondition and hello request will fail with HTTP 412.</span></span> <span data-ttu-id="a7ba5-123">Hello .NET SDK használata, ez akkor jelentkezik, mint egy `CloudException` ahol hello `IsAccessConditionFailed()` kiterjesztésmetódus igaz értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-123">If you're using hello .NET SDK, this manifests as a `CloudException` where hello `IsAccessConditionFailed()` extension method returns true.</span></span>

> [!Note]
> <span data-ttu-id="a7ba5-124">Nincs egyidejűségi csak egy mechanizmus.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-124">There is only one mechanism for concurrency.</span></span> <span data-ttu-id="a7ba5-125">Mindig használja, függetlenül attól, amely API erőforrás frissítésekhez használatos.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-125">It's always used regardless of which API is used for resource updates.</span></span> 

<a name="samplecode"></a>
## <a name="use-cases-and-sample-code"></a><span data-ttu-id="a7ba5-126">Használati esetek és mintakód</span><span class="sxs-lookup"><span data-stu-id="a7ba5-126">Use cases and sample code</span></span>

<span data-ttu-id="a7ba5-127">hello a következő kód bemutatja, accessCondition ellenőrzi a kulcs frissítési műveletek:</span><span class="sxs-lookup"><span data-stu-id="a7ba5-127">hello following code demonstrates accessCondition checks for key update operations:</span></span>

+ <span data-ttu-id="a7ba5-128">Egy frissítés sikertelen, ha hello erőforrás már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-128">Fail an update if hello resource no longer exists</span></span>
+ <span data-ttu-id="a7ba5-129">Egy frissítés sikertelen, ha hello erőforrás verzió</span><span class="sxs-lookup"><span data-stu-id="a7ba5-129">Fail an update if hello resource version changes</span></span>

### <a name="sample-code-from-dotnetetagsexplainer-programhttpsgithubcomazure-samplessearch-dotnet-getting-startedtreemasterdotnetetagsexplainer"></a><span data-ttu-id="a7ba5-130">A példakód [DotNetETagsExplainer program](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)</span><span class="sxs-lookup"><span data-stu-id="a7ba5-130">Sample code from [DotNetETagsExplainer program](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)</span></span>

```
    class Program
    {
        // This sample shows how ETags work by performing conditional updates and deletes
        // on an Azure Search index.
        static void Main(string[] args)
        {
            IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
            IConfigurationRoot configuration = builder.Build();

            SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

            Console.WriteLine("Deleting index...\n");
            DeleteTestIndexIfExists(serviceClient);

            // Every top-level resource in Azure Search has an associated ETag that keeps track of which version
            // of hello resource you're working on. When you first create a resource such as an index, its ETag is
            // empty.
            Index index = DefineTestIndex();
            Console.WriteLine(
                $"Test index hasn't been created yet, so its ETag should be blank. ETag: '{index.ETag}'");

            // Once hello resource exists in Azure Search, its ETag will be populated. Make sure toouse hello object
            // returned by hello SearchServiceClient! Otherwise, you will still have hello old object with the
            // blank ETag.
            Console.WriteLine("Creating index...\n");
            index = serviceClient.Indexes.Create(index);
            
            Console.WriteLine($"Test index created; Its ETag should be populated. ETag: '{index.ETag}'");

            // ETags let you do some useful things you couldn't do otherwise. For example, by using an If-Match
            // condition, we can update an index using CreateOrUpdate and be guaranteed that hello update will only
            // succeed if hello index already exists.
            index.Fields.Add(new Field("name", AnalyzerName.EnMicrosoft));
            index =
                serviceClient.Indexes.CreateOrUpdate(
                    index,
                    accessCondition: AccessCondition.GenerateIfExistsCondition());

            Console.WriteLine(
                $"Test index updated; Its ETag should have changed since it was created. ETag: '{index.ETag}'");

            // More importantly, ETags protect you from concurrent updates toohello same resource. If another
            // client tries tooupdate hello resource, it will fail as long as all clients are using hello right
            // access conditions.
            Index indexForClient1 = index;
            Index indexForClient2 = serviceClient.Indexes.Get("test");

            Console.WriteLine("Simulating concurrent update. toostart, both clients see hello same ETag.");
            Console.WriteLine($"Client 1 ETag: '{indexForClient1.ETag}' Client 2 ETag: '{indexForClient2.ETag}'");

            // Client 1 successfully updates hello index.
            indexForClient1.Fields.Add(new Field("a", DataType.Int32));
            indexForClient1 =
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient1,
                    accessCondition: AccessCondition.IfNotChanged(indexForClient1));

            Console.WriteLine($"Test index updated by client 1; ETag: '{indexForClient1.ETag}'");

            // Client 2 tries tooupdate hello index, but fails, thanks toohello ETag check.
            try
            {
                indexForClient2.Fields.Add(new Field("b", DataType.Boolean));
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient2, 
                    accessCondition: AccessCondition.IfNotChanged(indexForClient2));

                Console.WriteLine("Whoops; This shouldn't happen");
                Environment.Exit(1);
            }
            catch (CloudException e) when (e.IsAccessConditionFailed())
            {
                Console.WriteLine("Client 2 failed tooupdate hello index, as expected.");
            }

            // You can also use access conditions with Delete operations. For example, you can implement an
            // atomic version of hello DeleteTestIndexIfExists method from this sample like this:
            Console.WriteLine("Deleting index...\n");
            serviceClient.Indexes.Delete("test", accessCondition: AccessCondition.GenerateIfExistsCondition());

            // This is slightly better than using hello Exists method since it makes only one round trip to
            // Azure Search instead of potentially two. It also avoids an extra Delete request in cases where
            // hello resource is deleted concurrently, but this doesn't matter much since resource deletion in
            // Azure Search is idempotent.

            // And we're done! Bye!
            Console.WriteLine("Complete.  Press any key tooend application...\n");
            Console.ReadKey();
        }

        private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
        {
            string searchServiceName = configuration["SearchServiceName"];
            string adminApiKey = configuration["SearchServiceAdminApiKey"];

            SearchServiceClient serviceClient =
                new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
            return serviceClient;
        }

        private static void DeleteTestIndexIfExists(SearchServiceClient serviceClient)
        {
            if (serviceClient.Indexes.Exists("test"))
            {
                serviceClient.Indexes.Delete("test");
            }
        }

        private static Index DefineTestIndex() =>
            new Index()
            {
                Name = "test",
                Fields = new[] { new Field("id", DataType.String) { IsKey = true } }
            };
    }
}
```

## <a name="design-pattern"></a><span data-ttu-id="a7ba5-131">Kialakítási mintája</span><span class="sxs-lookup"><span data-stu-id="a7ba5-131">Design pattern</span></span>

<span data-ttu-id="a7ba5-132">A kialakítási mintában egyidejű hozzáférések optimista megvalósításának tartalmaznia kell egy ciklust, amely az újrapróbálja hello hozzáférés feltételének ellenőrzése, hello hozzáférési feltételnek, a teszt során, és opcionálisan egy frissített erőforrása toore megkísérlése előtt-hello módosítások alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-132">A design pattern for implementing optimistic concurrency should include a loop that retries hello access condition check, a test for hello access condition, and optionally retrieves an updated resource before attempting toore-apply hello changes.</span></span> 

<span data-ttu-id="a7ba5-133">A kódrészlet egy már létező synonymMap tooan index hello hozzáadását mutatja be.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-133">This code snippet illustrates hello addition of a synonymMap tooan index that already exists.</span></span> <span data-ttu-id="a7ba5-134">Ez a kód származik hello [szinonimát (előzetes verzió) C# oktatóanyag az Azure Search](https://docs.microsoft.com/azure/search/search-synonyms-tutorial-sdk).</span><span class="sxs-lookup"><span data-stu-id="a7ba5-134">This code is from hello [Synonym (preview) C# tutorial for Azure Search](https://docs.microsoft.com/azure/search/search-synonyms-tutorial-sdk).</span></span> 

<span data-ttu-id="a7ba5-135">hello részlet hello "Hotels"nevű index lekérdezi, ellenőrzi a frissítési művelet hello objektum verzióját, hello feltétel hiba esetén, majd az ismételt próbálkozás hello index lekérését hello server tooget hello legújabb kezdve (felfelé toothree idő szerint), művelet kivételt okoz verzió.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-135">hello snippet gets hello "hotels" index, checks hello object version on an update operation, throws an exception if hello condition fails, and then retries hello operation (up toothree times), starting with index retrieval from hello server tooget hello latest version.</span></span>

        private static void EnableSynonymsInHotelsIndexSafely(SearchServiceClient serviceClient)
        {
            int MaxNumTries = 3;

            for (int i = 0; i < MaxNumTries; ++i)
            {
                try
                {
                    Index index = serviceClient.Indexes.Get("hotels");
                    index = AddSynonymMapsToFields(index);

                    // hello IfNotChanged condition ensures that hello index is updated only if hello ETags match.
                    serviceClient.Indexes.CreateOrUpdate(index, accessCondition: AccessCondition.IfNotChanged(index));

                    Console.WriteLine("Updated hello index successfully.\n");
                    break;
                }
                catch (CloudException e) when (e.IsAccessConditionFailed())
                {
                    Console.WriteLine($"Index update failed : {e.Message}. Attempt({i}/{MaxNumTries}).\n");
                }
            }
        }
        
        private static Index AddSynonymMapsToFields(Index index)
        {
            index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
            index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };
            return index;
        }


## <a name="next-steps"></a><span data-ttu-id="a7ba5-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a7ba5-136">Next steps</span></span>

<span data-ttu-id="a7ba5-137">Felülvizsgálati hello [szinonimák C# minta](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) hogyan toosafely frissítésekor a további környezethez.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-137">Review hello [synonyms C# sample](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) for more context on how toosafely update an existing index.</span></span>

<span data-ttu-id="a7ba5-138">Próbálja módosítani vagy hello alábbi minták tooinclude ETag-EK vagy AccessCondition objektumot.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-138">Try modifying either of hello following samples tooinclude ETags or AccessCondition objects.</span></span>

+ [<span data-ttu-id="a7ba5-139">REST API-mintát a Githubon</span><span class="sxs-lookup"><span data-stu-id="a7ba5-139">REST API sample on Github</span></span>](https://github.com/Azure-Samples/search-rest-api-getting-started) 
+ <span data-ttu-id="a7ba5-140">[.NET SDK mintát a Githubon](https://github.com/Azure-Samples/search-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="a7ba5-140">[.NET SDK sample on Github](https://github.com/Azure-Samples/search-dotnet-getting-started).</span></span> <span data-ttu-id="a7ba5-141">Ez a megoldás hello "DotNetEtagsExplainer" projektet tartalmazó cikkben bemutatott hello kódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a7ba5-141">This solution includes hello "DotNetEtagsExplainer" project containing hello code presented in this article.</span></span>

## <a name="see-also"></a><span data-ttu-id="a7ba5-142">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a7ba5-142">See also</span></span>

  <span data-ttu-id="a7ba5-143">[Közös HTTP-kérelem-válasz fejlécek](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)  </span><span class="sxs-lookup"><span data-stu-id="a7ba5-143">[Common HTTP request and response headers](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)  </span></span>  
  <span data-ttu-id="a7ba5-144">[HTTP-állapotkódok](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) [Index műveletek (REST API-t)](https://docs.microsoft.com/\rest/api/searchservice/index-operations)</span><span class="sxs-lookup"><span data-stu-id="a7ba5-144">[HTTP status codes](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) [Index operations (REST API)](https://docs.microsoft.com/\rest/api/searchservice/index-operations)</span></span>