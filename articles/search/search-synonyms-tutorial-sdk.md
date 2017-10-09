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
# <a name="synonym-preview-c-tutorial-for-azure-search"></a>Szinonima (előzetes verzió) C# oktatóanyag az Azure Search szolgáltatáshoz

Szinonimák igényei szerint figyelembe veendő szemantikailag toohello bemeneti kifejezés egyeztetésével bontsa ki a lekérdezés. Például érdemes "autó" toomatch dokumentumok tartalmazó hello feltételek "autó" vagy "vehicle".

Az Azure Search szolgáltatásban a szinonimák meghatározása egy *szinonimatérképpel* történik, az egyenértékű kifejezéseket társító *leképezési szabályok* segítségével. Hozzon létre több szinonimát maps, elküldeni őket egy szolgáltatás szintű erőforrás elérhető tooany index, és melyik egy toouse hello mező szinten majd hivatkoznia. Időben továbbá toosearching indexet, az Azure Search végzi egy keresési szinonimát térképen, ha hello lekérdezésben használt mezőket meg van adva.

> [!NOTE]
> a szolgáltatás jelenleg a hello szinonimák villámnézetét, és csak a támogatott hello legújabb előzetes API és SDK-verzió (api-version = 2016 09-01. dátumú előnézeti, SDK verzió 4.x előzetes verzió). Jelenleg nincs Azure Portal-támogatás. Mivel az előzetes verziójú API-kra nem vonatkozik SLA, és az előzetes verziójú szolgáltatások változhatnak, nem javasolt éles környezetben használni őket.

## <a name="prerequisites"></a>Előfeltételek

Útmutató követelmények hello alábbiakat foglalja magába:

* [Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Search szolgáltatás](search-create-service-portal.md)
* [A Microsoft.Azure.Search .NET-kódtár előzetes verziója](https://aka.ms/search-sdk-preview)
* [Hogyan toouse Azure keresési .NET-alkalmazás](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

## <a name="overview"></a>Áttekintés

Előtt és után lekérdezések szinonimák hello értékének bemutatása. A jelen oktatóanyagban egy mintaalkalmazást használunk, amely lekérdezéseket hajt végre és eredményeket ad vissza egy mintaindexen. hello mintaalkalmazás két dokumentumok feltöltve a "hotels" nevű kis index létrehozása. hello alkalmazás végrehajtja a keresési lekérdezések használati kifejezések, amelyek nem szerepelnek a hello index, lehetővé teszi, hogy hello szinonimák szolgáltatást, majd problémák hello azonos keresések újra. az alábbi hello kód bemutatja, hello általános folyamata.

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
hello lépéseket toocreate és hello minta index feltöltése magyarázata [hogyan toouse Azure keresési .NET-alkalmazás](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).

## <a name="before-queries"></a>„Előtte” lekérdezések

A `RunQueriesWithNonExistentTermsInIndex` parancsban olyan keresési lekérdezéseket adunk ki, mint „five star” (ötcsillagos), „internet”, valamint „economy AND hotel” (gazdaság ÉS szálloda).
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
Hello feltételeket, hello két indexelt dokumentumok egyike sem tartalmaz, így azt először kimenetét hello hello következő `RunQueriesWithNonExistentTermsInIndex`.
~~~
Search hello entire index for hello phrase "five star":

no document matched

Search hello entire index for hello term 'internet':

no document matched

Search hello entire index for hello terms 'economy' AND 'hotel':

no document matched
~~~

## <a name="enable-synonyms"></a>Szinonimák engedélyezése

A szinonimák engedélyezése egy kétlépéses folyamat. Azt először határozza meg és szinonimát szabályok feltölteni, majd konfigurálja a mezők toouse őket. hello folyamatot szemlélteti az `UploadSynonyms` és `EnableSynonymsInHotelsIndex`.

1. Adjon hozzá egy szinonima térkép tooyour keresési szolgáltatást. A `UploadSynonyms`, azt a szinonima leképezés "desc-synonymmap" négy szabályokat definiálhat, és töltse fel a toohello szolgáltatás.
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
Egy szinonima térkép meg kell felelnie toohello nyílt forráskódú standard `solr` formátumban. hello formátum esetén, tekintse meg a [az Azure Search szinonimák](search-synonyms.md) hello szakaszban `Apache Solr synonym format`.

2. Konfigurálja a kereshető mezők toouse hello szinonimát térkép hello index definícióját. A `EnableSynonymsInHotelsIndex`, engedélyezzük a két mező szinonimák `category` és `tags` által hello beállítása `synonymMaps` toohello tulajdonságnevének hello újonnan feltöltött szinonimát leképezés.
```csharp
  Index index = serviceClient.Indexes.Get("hotels");
  index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
  index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };

  serviceClient.Indexes.CreateOrUpdate(index);
```
Szinonimatérkép hozzáadásakor nem kell újraépíteni az indexet. Térkép szinonimát tooyour szolgáltatás hozzáadása, és majd módosíthatja bármely index toouse hello új szinonimát leképezés létező mező meghatározásokat. hello hozzáadása új attribútumok nem hatást gyakorol index rendelkezésre állását. Ugyanez vonatkozik a szinonimák mező letiltása hello. Egyszerűen beállítható hello `synonymMaps` tulajdonság tooan listája üres.
```csharp
  index.Fields.First(f => f.Name == "category").SynonymMaps = new List<string>();
```

## <a name="after-queries"></a>„Utána” lekérdezések

Után hello szinonimát térkép tölt fel, és hello index frissített toouse hello szinonimát térkép, második hello `RunQueriesWithNonExistentTermsInIndex` hívás hello következő kimenete:

~~~
Search hello entire index for hello phrase "five star":

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search hello entire index for hello term 'internet':

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search hello entire index for hello terms 'economy' AND 'hotel':

Name: Roach Motel       Category: Budget        Tags: [motel, budget]
~~~
a lekérdezés első hello talál hello hello szabály dokumentum `five star=>luxury`. hello második lekérdezés kibontása hello keresési használatával `internet,wifi` és harmadik használatával is hello `hotel, motel` és `economy,inexpensive=>budget` hello keresését az egyező.

Szinonimák teljesen hozzáadása megváltoztatja a hello keresési élményt biztosít. Ebben az oktatóanyagban hello eredeti lekérdezés tooreturn jelentéssel bíró eredmények nem, annak ellenére, hogy az indexben hello dokumentumok volt megfelelő. Azáltal, hogy a szinonimák, azt egy index tooinclude feltételek közös használatához hello index módosítások toounderlying adatot nem tartalmazó bővítheti.

## <a name="sample-application-source-code"></a>Mintaalkalmazás forráskódja
Hello teljes forráskód a lépésein végighaladva a használt hello mintaalkalmazásnak található [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).

## <a name="next-steps"></a>Következő lépések

* Felülvizsgálati [hogyan toouse szinonimák az Azure Search](search-synonyms.md)
* Tekintse át a [szinonimák REST API dokumentációját](https://aka.ms/rgm6rq).
* Keresse meg a hivatkozásokat hello hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) és [REST API](https://docs.microsoft.com/rest/api/searchservice/).
