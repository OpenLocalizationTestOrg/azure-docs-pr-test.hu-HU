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
# <a name="synonym-preview-c-tutorial-for-azure-search"></a>Szinonima (előzetes verzió) C# oktatóanyag az Azure Search szolgáltatáshoz

A szinonimák bővítik a lekérdezéseket azáltal, hogy találatként kezelik a bemeneti kifejezéssel szemantikailag egyenértékűnek tekintett kifejezéseket. Előfordulhat például, hogy azt szeretné, hogy a „kocsi” kifejezésre olyan dokumentumokat kapjon eredményül, amelyek az „autó” vagy a „jármű” kifejezéseket is tartalmazzák.

Az Azure Search szolgáltatásban a szinonimák meghatározása egy *szinonimatérképpel* történik, az egyenértékű kifejezéseket társító *leképezési szabályok* segítségével. Több szinonimatérképet is létrehozhat, közzéteheti őket bármely index számára elérhető szolgáltatásszintű erőforrásként, majd hivatkozhat arra, amelyiket a mezőszinten használni kívánja. Az indexben való keresés mellett az Azure Search szolgáltatás lekérdezéskor a szinonimatérképben is keres, ha meg van határozva egy a lekérdezésben használt mezőkhöz.

> [!NOTE]
> A szinonimák szolgáltatás jelenleg előzetes verzióban van, és csak a legújabb előzetes verziójú API- és SDK-verziók támogatják (api-version=2016-09-01-Preview, SDK version 4.x-preview). Jelenleg nincs Azure Portal-támogatás. Mivel az előzetes verziójú API-kra nem vonatkozik SLA, és az előzetes verziójú szolgáltatások változhatnak, nem javasolt éles környezetben használni őket.

## <a name="prerequisites"></a>Előfeltételek

Az oktatóanyag az alábbi követelményekkel rendelkezik:

* [Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Search szolgáltatás](search-create-service-portal.md)
* [A Microsoft.Azure.Search .NET-kódtár előzetes verziója](https://aka.ms/search-sdk-preview)
* [Az Azure Search szolgáltatás használata .NET-alkalmazásból](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

## <a name="overview"></a>Áttekintés

Az „előtte és utána” lekérdezések a szinonimák előnyeit mutatják be. A jelen oktatóanyagban egy mintaalkalmazást használunk, amely lekérdezéseket hajt végre és eredményeket ad vissza egy mintaindexen. A mintaalkalmazás egy „hotels” nevű kisméretű indexet hoz létre, amely két dokumentummal van feltöltve. Az alkalmazás keresési lekérdezéseket futtat olyan kifejezésekkel, amelyek nem szerepelnek az indexben, engedélyezi a szinonimák szolgáltatást, majd ismét kiadja ugyanazokat a kereséseket. Az alábbi kód az általános folyamatot mutatja be.

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
A mintaindex létrehozásának és a dokumentumok feltöltésének lépései [Az Azure Search szolgáltatás használata .NET-alkalmazásból](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk) című cikkben találhatók.

## <a name="before-queries"></a>„Előtte” lekérdezések

A `RunQueriesWithNonExistentTermsInIndex` parancsban olyan keresési lekérdezéseket adunk ki, mint „five star” (ötcsillagos), „internet”, valamint „economy AND hotel” (gazdaság ÉS szálloda).
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
A két indexelt dokumentum közül egyik sem tartalmazza a kifejezéseket, ezért az első `RunQueriesWithNonExistentTermsInIndex` parancs a következő kimenetet eredményezi.
~~~
Search the entire index for the phrase "five star":

no document matched

Search the entire index for the term 'internet':

no document matched

Search the entire index for the terms 'economy' AND 'hotel':

no document matched
~~~

## <a name="enable-synonyms"></a>Szinonimák engedélyezése

A szinonimák engedélyezése egy kétlépéses folyamat. Először meghatározzuk és feltöltjük a szinonimaszabályokat, majd mezőket konfigurálunk az alkalmazásukhoz. A folyamatot az `UploadSynonyms` és az `EnableSynonymsInHotelsIndex` vázolja.

1. Adjon hozzá egy szinonimatérképet a keresőszolgáltatáshoz. Az `UploadSynonyms` parancsban négy szabályt határozunk meg a 'desc-synonymmap' szinonimatérképben, és feltöltjük őket a szolgáltatásba.
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
A szinonimatérképnek meg kell felelnie a nyílt forráskódú szabványos `solr` formátumnak. A formátum ismertetését a [Szinonimák az Azure Search szolgáltatásban](search-synonyms.md) című cikk `Apache Solr synonym format` szakasza tartalmazza.

2. Konfiguráljon kereshető mezőket a szinonimatérkép indexdefinícióban történő használatához. Az `EnableSynonymsInHotelsIndex` parancsban engedélyezzük a szinonimákat a `category` és a `tags` mezőkben úgy, hogy a `synonymMaps` tulajdonságot az újonnan feltöltött szinonimatérkép nevére állítjuk.
```csharp
  Index index = serviceClient.Indexes.Get("hotels");
  index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
  index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };

  serviceClient.Indexes.CreateOrUpdate(index);
```
Szinonimatérkép hozzáadásakor nem kell újraépíteni az indexet. A szolgáltatáshoz hozzáadhat egy szinonimatérképet, majd bármely indexben módosíthatja a meglévő mezőmeghatározásokat úgy, hogy azok az új szinonimatérképet használják. Az új attribútumok hozzáadása nincs hatással az index elérhetőségére. Ugyanez vonatkozik a szinonimák egy adott mező számára történő letiltására. A `synonymMaps` tulajdonságot egyszerűen átállíthatja egy üres listára.
```csharp
  index.Fields.First(f => f.Name == "category").SynonymMaps = new List<string>();
```

## <a name="after-queries"></a>„Utána” lekérdezések

A szinonimatérkép feltöltése és az index ennek használatára való frissítése után a második `RunQueriesWithNonExistentTermsInIndex` hívás a következő kimenetet eredményezi:

~~~
Search the entire index for the phrase "five star":

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the term 'internet':

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the terms 'economy' AND 'hotel':

Name: Roach Motel       Category: Budget        Tags: [motel, budget]
~~~
Az első lekérdezés a `five star=>luxury` szabály alapján megtalálja a dokumentumot. A második lekérdezés kibővíti a keresést az `internet,wifi` használatával, a harmadik pedig a `hotel, motel` és az `economy,inexpensive=>budget` kifejezést is használja a dokumentumok megtalálásához.

A szinonimák hozzáadásával teljesen megváltozik a keresési élmény. Ebben az oktatóanyagban az eredeti lekérdezések nem hoztak használható eredményeket, bár az indexben található dokumentumok a kérdéses témakörre vonatkoztak. A szinonimák engedélyezésével úgy bővíthetjük ki az indexet, hogy a gyakran használt kifejezéseket is tartalmazza a benne található mögöttes adatok megváltoztatása nélkül.

## <a name="sample-application-source-code"></a>Mintaalkalmazás forráskódja
A jelen útmutatóban használt mintaalkalmazás teljes forráskódját a [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) webhelyén találja.

## <a name="next-steps"></a>Következő lépések

* Tekintse át [a szinonimák Azure Searchben történő használatával](search-synonyms.md) foglalkozó cikket.
* Tekintse át a [szinonimák REST API dokumentációját](https://aka.ms/rgm6rq).
* Nézze át a [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) és a [REST API](https://docs.microsoft.com/rest/api/searchservice/) referenciáit.
