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
# <a name="query-your-azure-search-index-using-hello-net-sdk"></a>Hello .NET SDK használatával az Azure Search-index lekérdezése
> [!div class="op_single_selector"]
> * [Áttekintés](search-query-overview.md)
> * [Portál](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Ez a cikk bemutatja, hogyan egy index használatával tooquery hello [Azure Search .NET SDK](https://aka.ms/search-sdk).

A bemutató elindítása előtt [létre kell hoznia egy Azure Search-indexet](search-what-is-an-index.md), majd [fel kell töltenie azt adatokkal](search-what-is-data-import.md).

> [!NOTE]
> A cikkben szereplő összes példakód C# nyelven van megírva. Hello teljes forráskód található [a Githubon](http://aka.ms/search-dotnet-howto). Hello is olvashat [Azure Search .NET SDK](search-howto-dotnet-sdk.md) egy részletesebb lépésein végighaladva hello mintakódot az.

## <a name="identify-your-azure-search-services-query-api-key"></a>Azonosítsa az Azure Search szolgáltatás lekérdezési API-kulcsát
Most, hogy létrehozta az Azure Search-index, áll majdnem kész tooissue lekérdezések hello .NET SDK használatával. Először szüksége lesz egy hello lekérdezés api-kulcs lett létrehozva, tooobtain hello keresőszolgáltatáshoz. hello .NET SDK minden kérelem tooyour szolgáltatás elküld az api-kulcsot. Érvényes kulcs birtokában létesít megbízhatósági, egy kérelem alapon hello küldő hello kérelem és a kezelő hello szolgáltatás között.

1. toofind a szolgáltatás api-kulcsokat bejelentkezhet toohello [Azure-portálon](https://portal.azure.com/)
2. Nyissa meg tooyour Azure Search szolgáltatás paneljét
3. Kattintson a hello "Kulcsok" ikonra

A szolgáltatás *rendszergazdai kulcsokkal* és *lekérdezési kulcsokkal* fog rendelkezni.

* Az elsődleges és másodlagos *adminisztrációs kulcsok* teljes körű tooall műveleteket, köztük a hello képességét toomanage hello szolgáltatást biztosítania hozzon létre, és törölje az indexek, az indexelők és az adatforrások. Két kulcs van, hogy a Folytatás toouse hello másodlagos kulcsát. Ha úgy dönt, hogy tooregenerate hello elsődleges kulcs, és fordítva.
* A *lekérdezési kulcsok* adjon olvasási hozzáférést tooindexes és a dokumentumok és keresési kérelmeket kibocsátó általában elosztott tooclient alkalmazások.

Az index lekérdezése hello alkalmazásában a lekérdezési kulcsok egyikét használhatja. Az adminisztrációs kulcsok is használható a lekérdezések, de egy lekérdezési kulcsot kell használni az alkalmazás kódjában, az alábbi módon ez jobban hello [legalacsonyabb jogosultsági szint elve](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="create-an-instance-of-hello-searchindexclient-class"></a>Hello SearchIndexClient osztály példányának létrehozása
az Azure Search .NET SDK hello lekérdezések tooissue, szüksége lesz toocreate hello példányának `SearchIndexClient` osztály. Ez az osztály több konstruktorral rendelkezik. hello kívánt időt vesz igénybe, a keresőszolgáltatása nevét, az index neve, és egy `SearchCredentials` objektumot használ paraméterként. A `SearchCredentials` becsomagolja az API-kulcsot.

az alábbi hello kód létrehoz egy új `SearchIndexClient` hello "Hotels nevű" index (létrehozott [hello .NET SDK használatával Azure Search-index létrehozása](search-create-index-dotnet.md)) hello keresőszolgáltatása nevét és api-kulcs hello alkalmazás config tárolt értékekkel fájl (`appsettings.json` hello hello esetében [mintaalkalmazás](http://aka.ms/search-dotnet-howto)):

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

A `SearchIndexClient` rendelkezik egy `Documents` tulajdonsággal. Ez a tulajdonság biztosít minden hello tooquery Azure Search-indexek szükséges módszereket.

## <a name="query-your-index"></a>Az index lekérdezése
A helyettesítő hello .NET SDK legegyszerűbb hívó hello `Documents.Search` metódust a `SearchIndexClient`. Ez a módszer veszi néhány paraméterekkel, többek között a keresési szöveget: hello, valamint egy `SearchParameters` objektum, amely használt toofurther finomítása hello lekérdezés lehet.

#### <a name="types-of-queries"></a>A lekérdezések típusai
hello két fő [típusok lekérdezése](search-query-overview.md#types-of-queries) használandó vannak `search` és `filter`. A `search` lekérdezés egy vagy több kifejezésre keres rá az index összes *searchable* (kereshető) mezőjében. A `filter` lekérdezés egy logikai kifejezés kiértékelését végzi el az index összes *filterable* (szűrhető) mezőjén.

Keresés és a szűrők készül hello segítségével `Documents.Search` metódust. Keresési lekérdezés átadhatók a hello `searchText` paraméter, miközben egy kifejezést a hello adhatók át `Filter` hello tulajdonságának `SearchParameters` osztály. toofilter nélkül keres, csak adja át `"*"` a hello `searchText` paraméter. toosearch szűrés nélkül ne változtassa meg hello `Filter` tulajdonság beállított, vagy nem adjon át egy `SearchParameters` minden példány.

#### <a name="example-queries"></a>Példa a lekérdezésekre
a következő példakód hello látható többféleképpen tooquery hello "Hotels"nevű index definiált [hello .NET SDK használatával Azure Search-index létrehozása](search-create-index-dotnet.md#DefineIndex). Ne feledje, hogy ad vissza, amelyben hello keresési eredmények hello dokumentumok hello példányai `Hotel` osztályt, amely lett definiálva [adatok importálása az Azure Search használatával hello .NET SDK](search-import-data-dotnet.md#HotelClass). mintakód hello használ egy `WriteDocuments` metódus toooutput hello keresési eredmények toohello konzol. Ez a módszer hello a következő szakaszban ismertetett.

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

## <a name="handle-search-results"></a>A keresési eredmények kezelése
Hello `Documents.Search` metódus értéket ad vissza egy `DocumentSearchResult` hello hello lekérdezés eredményeit tartalmazó objektum. hello példa az előző szakaszban hello használt a hívott metódus `WriteDocuments` toooutput hello keresési eredmények toohello konzol:

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

Ez mit hello eredmények néz ki, az előző szakaszban hello hello lekérdezésekhez feltételezve hello "Hotels"nevű index példaadatok hello van feltöltve [adatok importálása az Azure Search használatával hello .NET SDK](search-import-data-dotnet.md):

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

a fenti hello mintakód hello konzol toooutput keresési eredményeket használja. A saját alkalmazásban toodisplay keresési eredmények hasonlóképpen kell. Lásd: [ezt a mintát a Githubon](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) hogyan toorender keresés eredménye egy ASP.NET MVC-alapú webalkalmazás példát.

