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
# <a name="how-toomanage-concurrency-in-azure-search"></a>Hogyan toomanage egyidejűségi az Azure Search

Azure Search erőforrások, például indexekkel és adatforrások kezelése, esetén fontos tooupdate erőforrások biztonságosan, különösen akkor, ha az erőforrás hozzáférése címtárfelhasználói egyidejűleg az alkalmazás különböző összetevői. Ha két ügyfelek egyidejűleg frissítés nélkül koordinációs erőforrás, versenyhelyzetek is előfordulhatnak. tooprevent ez, az Azure Search ajánlatok egy *egyidejű hozzáférések optimista modell*. Nincsenek zárolások erőforráson. Ehelyett nincs egy ETag minden erőforrás, amely azonosítja a hello erőforrás-verziója, így Ön összeállíthatnak kérelmek elkerüléséhez véletlen felülírja.

> [!Tip]
> A fogalmi kód egy [C# megoldás minta](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) azt ismerteti, hogyan párhuzamossági működik az Azure Search. hello kód hívása egyidejűség-vezérlési feltételek hoz létre. Olvasási hello [az alábbi kódrészlet](#samplecode) valószínűleg elegendőek a legtöbb fejlesztő, de ha kívánja toorun azt Szerkesztés appsettings.json tooadd hello szolgáltatás nevét és egy adminisztrációs api-kulcsot. A szolgáltatás URL-t adott `http://myservice.search.windows.net`, hello szolgáltatás neve megkülönbözteti a `myservice`.

## <a name="how-it-works"></a>Működés

Optimista feldolgozási van megvalósítva keresztül hozzáférést feltétel ellenőrzi tooindexes, indexelők, adatforrások és synonymMap erőforrások írása API-hívásokban. 

Minden erőforrás rendelkezik egy [ *entitáscímke (ETag)* ](https://en.wikipedia.org/wiki/HTTP_ETag) objektum fájlverzió-információkat biztosít. Ellenőrzésével hello ETag először, elkerülheti a párhuzamos frissítések egy tipikus munkafolyamatban (beolvasni, helyileg módosítása és frissítése) biztosításával a hello erőforrás ETag megegyezik a helyi példány. 

+ hello REST API-t használ egy [ETag](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) a hello kérelemfejlécet.
+ hello .NET SDK beállítja hello ETag keresztül accessCondition objektum, hello beállítása [If-Match |} If-Match-nincs fejléc](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) hello erőforráson. Összes objektumtól örököl [IResourceWithETag (.NET SDK-val)](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.iresourcewithetag) accessCondition objektum rendelkezik.

Minden alkalommal, amikor frissít egy erőforrást, az ETag automatikus módosítja. Párhuzamossági felügyeleti bevezetésekor csak azt helyezi előfeltétel kérésre hello frissítés hello távoli erőforrás igénylő toohave hello azonos ETag hello másolatként hello ügyfélen módosító hello erőforrás. Ha egy egyidejű folyamat már hello távoli erőforrás változott, hello ETag nem fog egyezni hello előfeltétel, és hello kérelem sikertelen lesz, és HTTP 412. Hello .NET SDK használata, ez akkor jelentkezik, mint egy `CloudException` ahol hello `IsAccessConditionFailed()` kiterjesztésmetódus igaz értéket ad vissza.

> [!Note]
> Nincs egyidejűségi csak egy mechanizmus. Mindig használja, függetlenül attól, amely API erőforrás frissítésekhez használatos. 

<a name="samplecode"></a>
## <a name="use-cases-and-sample-code"></a>Használati esetek és mintakód

hello a következő kód bemutatja, accessCondition ellenőrzi a kulcs frissítési műveletek:

+ Egy frissítés sikertelen, ha hello erőforrás már nem létezik.
+ Egy frissítés sikertelen, ha hello erőforrás verzió

### <a name="sample-code-from-dotnetetagsexplainer-programhttpsgithubcomazure-samplessearch-dotnet-getting-startedtreemasterdotnetetagsexplainer"></a>A példakód [DotNetETagsExplainer program](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)

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

## <a name="design-pattern"></a>Kialakítási mintája

A kialakítási mintában egyidejű hozzáférések optimista megvalósításának tartalmaznia kell egy ciklust, amely az újrapróbálja hello hozzáférés feltételének ellenőrzése, hello hozzáférési feltételnek, a teszt során, és opcionálisan egy frissített erőforrása toore megkísérlése előtt-hello módosítások alkalmazásához. 

A kódrészlet egy már létező synonymMap tooan index hello hozzáadását mutatja be. Ez a kód származik hello [szinonimát (előzetes verzió) C# oktatóanyag az Azure Search](https://docs.microsoft.com/azure/search/search-synonyms-tutorial-sdk). 

hello részlet hello "Hotels"nevű index lekérdezi, ellenőrzi a frissítési művelet hello objektum verzióját, hello feltétel hiba esetén, majd az ismételt próbálkozás hello index lekérését hello server tooget hello legújabb kezdve (felfelé toothree idő szerint), művelet kivételt okoz verzió.

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


## <a name="next-steps"></a>Következő lépések

Felülvizsgálati hello [szinonimák C# minta](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) hogyan toosafely frissítésekor a további környezethez.

Próbálja módosítani vagy hello alábbi minták tooinclude ETag-EK vagy AccessCondition objektumot.

+ [REST API-mintát a Githubon](https://github.com/Azure-Samples/search-rest-api-getting-started) 
+ [.NET SDK mintát a Githubon](https://github.com/Azure-Samples/search-dotnet-getting-started). Ez a megoldás hello "DotNetEtagsExplainer" projektet tartalmazó cikkben bemutatott hello kódot tartalmaz.

## <a name="see-also"></a>Lásd még:

  [Közös HTTP-kérelem-válasz fejlécek](https://docs.microsoft.com/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search)    
  [HTTP-állapotkódok](https://docs.microsoft.com/rest/api/searchservice/http-status-codes) [Index műveletek (REST API-t)](https://docs.microsoft.com/\rest/api/searchservice/index-operations)