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
# <a name="how-toouse-azure-search-from-a-net-application"></a>Hogyan toouse Azure keresési .NET-alkalmazás
Ez a cikk egy forgatókönyv tooget, a hello lépéseivel [Azure Search .NET SDK](https://aka.ms/search-sdk). Hello .NET SDK tooimplement egy gazdag is használhatja az Azure Search segítségével az alkalmazás nyújtotta adatkeresési élménybe.

## <a name="whats-in-hello-azure-search-sdk"></a>Mi az a hello az Azure Search SDK
hello SDK áll egy ügyféloldali kódtár `Microsoft.Azure.Search`. Azt, toomanage lehetővé teszi, hogy az indexek, az adatforrások és az indexelők, valamint feltöltése és dokumentumok kezeléséhez, és hajtsa végre a lekérdezéseket, anélkül, hogy a HTTP és a JSON hello adatokkal toodeal.

hello ügyféloldali kódtár határozza meg, például osztályok `Index`, `Field`, és `Document`, illetve műveletek, például `Indexes.Create` és `Documents.Search` a hello `SearchServiceClient` és `SearchIndexClient` osztályok. Ezeket az osztályokat a következő névterek hello szerint vannak csoportosítva:

* [Microsoft.Azure.Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft.Azure.Search.Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

hello aktuális hello Azure Search .NET SDK verziója most általánosan elérhető. Ha azt szeretné, hogy tooprovide visszajelzést az USA tooincorporate hello következő verziójában, látogasson el a [visszajelzés](https://feedback.azure.com/forums/263029-azure-search/).

hello .NET SDK verziót támogatja `2016-09-01` a hello [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/). Ebben a verzióban mostantól tartalmazza az egyéni lekérdezések és Azure-Blob és az Azure tábla indexelő támogatást. Az előzetes funkciók, amelyek *nem* verzióhoz, például az indexelés JSON és a CSV-fájlok támogatását szerepelnek [előzetes](search-api-2015-02-28-preview.md) és régebbi hello keresztül elérhető [hello .NET SDK 2.0-előzetes verzióját ](https://aka.ms/search-sdk-preview).

Ez az SDK nem támogatja a [felügyeleti műveletek](https://docs.microsoft.com/rest/api/searchmanagement/) például létrehozása és a keresési szolgáltatás méretezése és API-kulcsokat kezelése. Ha toomanage kell a .NET-alkalmazás erőforrások keresése, használhatja a hello [Azure Search .NET SDK-t felügyeleti](https://aka.ms/search-mgmt-sdk).

## <a name="upgrading-toohello-latest-version-of-hello-sdk"></a>Toohello hello SDK legújabb verziójának frissítése
Ha már használja egy régebbi verzióját hello Azure Search .NET SDK-t, és azt szeretné, hogy tooupgrade toohello általánosan elérhető verziójának, [Ez a cikk](search-dotnet-sdk-migration.md) ismerteti, hogyan.

## <a name="requirements-for-hello-sdk"></a>Hello SDK követelményei
1. A Visual Studio 2017.
2. A saját Azure Search szolgáltatás. A sorrend toouse hello SDK szüksége lesz a hello a szolgáltatás nevét és egy vagy több API-kulcsokat. [Szolgáltatás létrehozása a portál hello](search-create-service-portal.md) segít a fenti lépéseket.
3. Töltse le az Azure Search .NET SDK hello [NuGet-csomag](http://www.nuget.org/packages/Microsoft.Azure.Search) a Visual Studio "Manage NuGet Packages" használatával. Hello csomag neve csak keressen `Microsoft.Azure.Search` NuGet.org meg.

hello Azure Search .NET SDK támogatja a .NET-keretrendszer 4.6 hello és a .NET Core célzó alkalmazásokat.

## <a name="core-scenarios"></a>Legfontosabb forgatókönyvek
Számos szempontot toodo szüksége lesz az alkalmazás. Ez az oktatóanyag azt érintünk core forgatókönyvekben:

* Az index létrehozása
* A dokumentumok hello index feltöltése
* A teljes szöveges keresés és a szűrők használatával dokumentumok keresése

a következő hello mintakód ezen mutatja be. Érzi, hogy szabad toouse hello kódtöredékek a saját alkalmazásban.

### <a name="overview"></a>Áttekintés
hello azt fogja felfedezése mintaalkalmazás létrehoz egy új "hotels" nevű index tölti fel az néhány dokumentumok, akkor néhány keresési lekérdezést hajt végre. Hello fő program, amely itt található általános folyamata hello:

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
> Hello teljes forráskód a lépésein végighaladva a használt hello mintaalkalmazásnak található [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).
> 
>

Végigvezetjük a lépésről lépésre. Először igazolnia kell, hogy új toocreate `SearchServiceClient`. Ez az objektum teszi lehetővé toomanage indexeket. Az egyik rendelés tooconstruct kell tooprovide az Azure Search szolgáltatás neve, valamint az adminisztrációs API-kulcsot. Ezt az információt is beírhatja hello `appsettings.json` hello fájljának [mintaalkalmazás](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

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
> Ha megad egy helytelen kulcsot (például egy lekérdezési kulcsot ahol egy adminisztrációs kulcsot volt szükség), hello `SearchServiceClient` kivételhibát egy `CloudException` hello hiba üzenet "Tiltott" hello, először egy művelet metódust hívja meg, például a `Indexes.Create`. Ilyen esetben tooyou gondosan ellenőrizze az API-kulcsot.
> 
> 

hello tovább néhány sor hívás módszerek toocreate index "Hotels" nevet adtuk, először törlése, ha már létezik. Végigvezetjük ezen módszerek egy kicsit később.

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

A következő hello kell toobe feltöltve. toodo, fel kell egy `SearchIndexClient`. Nincsenek a két módon tooobtain egyik: hozhat létre, azt, vagy meghívásával `Indexes.GetClient` a hello `SearchServiceClient`. Ez utóbbi hello használjuk kényelmét szolgálja.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> A keresőalkalmazásokban az indexkezelést és -feltöltést általában a keresési lekérdezésektől eltérő elem végzi. `Indexes.GetClient`előnyei index feltöltése, mert menti azt, hogy egy másik probléma hello `SearchCredentials`. Ennek érdekében átadásakor hello adminisztrációs kulcsot adott meg használt toocreate hello `SearchServiceClient` új toohello `SearchIndexClient`. Azonban hello része az alkalmazás, amely végrehajtja a lekérdezéseket, hogy a rendszer jobb toocreate hello `SearchIndexClient` közvetlenül, hogy egy lekérdezési kulcsot helyett egy adminisztrációs kulcsot is át. Ez a legalacsonyabb jogosultsági szint elve hello konzisztens, és segít toomake biztonságosabb az alkalmazás. További információk az adminisztrációs kulcsok és a lekérdezési kulcsok található [Itt](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).
> 
> 

Most, hogy egy `SearchIndexClient`, hello index feltöltünk is. Ez végigvezetjük később más módon történik.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

Végül azt néhány keresési-lekérdezéseket hajt végre, és hello eredmények megjelenítéséhez. Most használjuk a különböző `SearchIndexClient`:

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

Most elindítjuk hello közelebbről `RunQueries` később metódust. Itt egy hello kód toocreate hello új `SearchIndexClient`:

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

Most használjuk a lekérdezési kulcs óta nem kell írási hozzáférést toohello index. Ezt az információt is beírhatja hello `appsettings.json` hello fájljának [mintaalkalmazás](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

Ha az alkalmazás futtatásához a egy érvényes szolgáltatásnevet és API-kulcsokat, hello kimeneti kell néznek ki:

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

Ez a cikk végén hello hello teljes forráskód hello alkalmazás valósul meg.

A következő most elindítjuk által meghívott hello módszerek részletes bemutatása `Main`.

### <a name="creating-an-index"></a>Az index létrehozása
Létrehozása után egy `SearchServiceClient`, következő lépésként hello `Main` does van a delete hello "Hotels"nevű index, ha már létezik. Amely a következő metódus hello végzi el:

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
    }
}
```

Ezt a módszert használja a megadott hello `SearchServiceClient` toocheck hello index létezik, és ha igen, törölje azt.

> [!NOTE]
> hello példakódot ebben a cikkben az egyszerűség kedvéért hello hello Azure Search .NET SDK szinkron módszereit használja. Javasoljuk, hogy a saját alkalmazások tookeep hello aszinkron metódusok használja őket a méretezhető és rugalmas. Például a hello metódus fent, rendszerrel lehetett `ExistsAsync` és `DeleteAsync` helyett `Exists` és `Delete`.
> 
> 

Ezt követően `Main` hoz létre egy új "Hotels nevű" index által a metódus hívása:

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

Ezzel a módszerrel hoz létre egy új `Index` listáját tartalmazó objektum `Field` hello séma hello új index definiáló objektumok. Egyes mezők nevét, adattípus és a keresés viselkedését meghatározó számos attribútum rendelkezik. Hello `FieldBuilder` osztály reflexiós toocreate listáját használja `Field` hello index megvizsgálásával objektumok hello nyilvános tulajdonságokat és a megadott hello attribútumait `Hotel` osztály modell. Hello közelebbről lesz vesszük `Hotel` osztály később.

> [!NOTE]
> Bármikor létrehozhat hello listája `Field` objektumok közvetlenül helyett `FieldBuilder` szükség esetén. Például kívánt toouse egy modellosztállyal, vagy egy meglévő modellosztállyal, hogy nem szeretné, hogy toomodify attribútumok hozzáadásával toouse szükség lehet.
>
> 

Ezenkívül toofields, azt is megteheti a pontozási profil, a javaslattevők vagy a CORS-beállítások toohello Index (ezek hiányoznak kivonatosan mutatja hello mintából). További információ a hello Index objektum és azok részlegei található hello [SDK referenciáit](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index), valamint az hello [Azure Search REST API-referencia](https://docs.microsoft.com/rest/api/searchservice/).

### <a name="populating-hello-index"></a>Hello index feltöltése
következő lépése hello `Main` toopopulate hello újonnan létrehozott index. Ez a módszer a következő hello történik:

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

Ez a metódus négy részből áll. hello először létrehoz egy tömbjét `Hotel` objektumok bemeneti adatok tooupload toohello indexben erre a célra. Ezek az adatok nem változtatható az egyszerűség érdekében. A saját alkalmazásban az adatok valószínűleg határozza meg egy külső adatforrásból, például az SQL-adatbázis.

hello második rész létrehoz egy `IndexBatch` hello dokumentumokat tartalmazó. Megadhatja a kívánt tooapply toohello kötegelt hoz létre, ebben az esetben meghívásával hello időpontban hello művelet `IndexBatch.Upload`. hello kötegelt nem feltöltött toohello Azure Search-index által hello `Documents.Index` metódust.

> [!NOTE]
> Ebben a példában azt csak feltölteni dokumentumokat. Ha meglévő vagy delete dokumentumokat toomerge változik, létrehozhat meghívásával kötegek `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, vagy `IndexBatch.Delete` helyette. A kötegek különböző műveletei adjunk meghívásával `IndexBatch.New`, gyűjteménye, amely veszi `IndexAction` objektumok, amelyek mindegyike közli az Azure Search tooperform a dokumentum egy adott művelethez. Minden egyes hozhat létre `IndexAction` saját műveletet hello megfelelő metódus meghívásával `IndexAction.Merge`, `IndexAction.Upload`, és így tovább.
> 
> 

hello harmadik ezt a módszert része egy catch blokkba, amely kezeli az egyik fontos hiba esetében az indexeléshez. Ha az Azure Search szolgáltatás sikertelen tooindex néhány hello hello kötegben, dokumentumok egy `IndexBatchException` által okozott `Documents.Index`. Ez akkor történhet meg, ha olyankor végzi a dokumentumok indexelését, amikor a szolgáltatás nagy terhelés alatt áll. **Javasoljuk, hogy a kódban explicit módon kezelje ezt az esetet.** Késleltetés, és próbálkozzon újra a sikertelen indexelési hello dokumentumok, vagy jelentkezzen és például hello minta nem, vagy valami mást, attól függően, hogy az alkalmazás konzisztenciájának követelményeinek elvégezhető folytatásához.

> [!NOTE]
> Használhatja a hello `FindFailedActionsToRetry` tartalmazó új kötegelt metódus tooconstruct csak hello túl egy korábbi hívás sikertelen műveletek`Index`. hello metódus dokumentált [Itt](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) , és egy tooproperly használatának értékelése [a StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).
>
>

Végezetül hello `UploadDocuments` két másodpercen metódus késést. Indexelő történik aszinkron módon történik az Azure Search szolgáltatásban, így hello mintaalkalmazás kell toowait egy rövid időn belül tooensure, hogy a keresés hello dokumentumok is. Ilyen mértékű késleltetésre kizárólag demók, tesztek és mintaalkalmazások esetében van szükség.

#### <a name="how-hello-net-sdk-handles-documents"></a>Hogyan kezeli a .NET SDK hello a dokumentumok
Előfordulhat, hogy lehet szeretné megtudni, hogy hogyan hello Azure Search .NET SDK például a felhasználói osztály példányainak képes tooupload `Hotel` toohello index. toohelp válaszolja meg, hogy a kérdést, nézzük hello `Hotel` osztály:

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

hello először thing toonotice, hogy minden egyes nyilvános tulajdonsága `Hotel` felel meg a hello Indexdefiníció, de egy fontos különbséggel tooa mező: hello minden mező kezdetű névvel rendelkező visszaalakítja ("teve eset"), közben minden nyilvános hello neve a tulajdonság `Hotel` egy nagybetűt ("Pascal eset") kezdődik. Ez az a közös hajtható végre adatkötés, ahol hello cél séma az alkalmazásfejlesztő hello külső hello irányítását .NET-alkalmazások esetén. Ahelyett, hogy a tooviolate hello .NET irányelvek elnevezési azáltal, hogy a tulajdonság nevét teve eset, megadható, hogy a hello SDK toomap hello tulajdonság nevének toocamel-kódaláírásokra automatikusan hello `[SerializePropertyNamesAsCamelCase]` attribútum.

> [!NOTE]
> hello Azure Search .NET SDK-t használ a hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) könyvtár tooserialize és deszerializálható JSON formátumból az egyéni modell objektumok tooand. A szerializálás szükség szerint testre szabható. További részletekért lásd: [JSON.NET az egyéni szerializálási](#JsonDotNet).
> 
> 

hello második lépésként toonotice hello attribútumok például `IsFilterable`, `IsSearchable`, `Key`, és `Analyzer` , amely minden egyes nyilvános tulajdonsága adja. Ezek az attribútumok hozzárendelését közvetlenül toohello [hello Azure Search-index megfelelő attribútumait](https://docs.microsoft.com/rest/api/searchservice/create-index#request). Hello `FieldBuilder` osztály hello index használja e tooconstruct mező definíciókat.

hello kapcsolatos fontos dolog harmadik hello `Hotel` osztály hello adattípusok hello nyilvános tulajdonságainak. hello .NET ezeket a tulajdonságokat képezi le hello Indexdefiníció tootheir egyenértékű mező típusa. Például hello `Category` karakterlánc típusú tulajdonság leképezhető toohello `category` mező, amelynek a típusa `Edm.String`. Hasonló típusú hozzárendelés közötti `bool?` és `Edm.Boolean`, `DateTimeOffset?` és `Edm.DateTimeOffset`, stb. hello speciális szabályok a következő típusleképezéshez hello szerepelnek a hello `Documents.Get` metódus a hello [Azure Search .NET SDK hivatkozás](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_). Hello `FieldBuilder` osztály gondoskodik meg ezt a hozzárendelést, de továbbra is lehet hasznos toounderstand arra az esetre kell tootroubleshoot szerializálási probléma merül fel.

Ez lehetőséget toouse saját osztályok-dokumentumokként működik mindkét irányban; Keresési eredmények beolvasásához és is rendelkezik hello SDK automatikusan deszerializálni őket tooa típus az Ön által választott, a következő szakaszban hello lesz látható.

> [!NOTE]
> hello Azure Search .NET SDK-t is támogatja a hello segítségével dinamikusan gépelt dokumentumok `Document` osztályt, amely nevek toofield mezőértékek kulcs/érték hozzárendelése. Ez olyan esetekben hasznos, ha hello a sémát indexeli tervezéskor nem tudja, vagy ha kényelmetlen toobind toospecific modell osztályok lenne. Minden hello módszerek hello SDK a dokumentumok kezelése rendelkezik, amelyek használhatók a hello túlterhelések `Document` osztály, valamint az erős típusmegadású túlterhelések, amelyek általános típusú paramétert. Ez utóbbi csak hello hello mintakód ebben az oktatóanyagban használt. Hello `Document` osztály örökli `Dictionary<string, object>`. Más részletei [Itt](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).
> 
> 

**Miért használjon nullázható adattípusokat?**

A saját modell osztályok toomap tooan Azure Search-index tervezésekor ajánlott érték típusú tulajdonságok például deklaráló `bool` és `int` toobe nullázható (például `bool?` helyett `bool`). Ha egy nem nullázható tulajdonságot használja, akkor túl**garantálni** , hogy az indexben nem dokumentumok hello megfelelő mező null értéket tartalmaz. Hello SDK sem hello Azure Search szolgáltatás segítségével Ön tooenforce ez.

Ez nem csak egy elméleti szempont: képzelhető el, egy olyan forgatókönyvet, ahol hozzáadhat egy új tooan meglévő mezőindex, amely típusú `Edm.Int32`. Hello index definíciójának frissítése után összes dokumentumot lesz az új mező null értéket (mivel az Azure Search nullázható minden esetében). Ha az egy nem nullázható majd használja egy modell osztály `int` tulajdonság, mező, elérhetővé válik egy `JsonSerializationException` ez, például tooretrieve dokumentumok tett kísérlet során:

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

Ezért javasoljuk, hogy a modellosztályokban nullázható értéktípusokat használjon.

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a>A JSON.NET egyedi szerializálás
hello SDK JSON.NET szerializálása és deszerializálása dokumentumok használ. Testre szabhatja a szerializálás és szükség esetén meghatározhat egy saját deszerializálási `JsonConverter` vagy `IContractResolver` (lásd: hello [JSON.NET dokumentáció](http://www.newtonsoft.com/json/help/html/Introduction.htm) további részletekért). Ez akkor lehet hasznos, ha azt szeretné, egy meglévő modellosztállyal tooadapt az alkalmazásból az Azure Search és egyéb speciális forgatókönyvekhez használható. Például az egyedi szerializálás lehetővé teszi:

* Tartalmaznak, vagy zárja ki a modell osztály egyes tulajdonságai dokumentum mezői tárolt.
* Képezze le a kódban nevei és az indexben mezőnevek között.
* Hozzon létre egyéni attribútumok csak tulajdonságok toodocument mezők leképezése is használható.

Egyedi szerializálás implementációi hello egység vizsgálatokra hello Azure Search .NET SDK a Githubon található. A jó kiindulási pont [Ez a mappa](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models). Hello egyedi szerializálás tesztek által használt osztályok tartalmaz.

### <a name="searching-for-documents-in-hello-index"></a>Hello index dokumentumok keresése
hello mintaalkalmazás hello utolsó lépése a bizonyos dokumentumok hello index toosearch. a következő metódus hello hajtja végre ezt:

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

Minden alkalommal, amikor végrehajtja a lekérdezést, ez a módszer először létrehoz egy új `SearchParameters` objektum. Ez a használt toospecify további beállításokat, például a rendezési, szűrési, lapozás és értékkorlátozás hello lekérdezéshez. Ezzel a módszerrel hello beállítás van `Filter`, `Select`, `OrderBy`, és `Top` különböző lekérdezések tulajdonság. Minden hello `SearchParameters` tulajdonságok szerepelnek [Itt](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).

következő lépés hello tooactually hello keresési lekérdezés végrehajtása. Ebben az esetben hello segítségével `Documents.Search` metódust. Minden egyes lekérdezés esetén, azt át hello keresési szöveg toouse karakterláncként (vagy `"*"` nincs a keresési szöveget esetén), és keresse meg a korábban létrehozott paraméterek hello. Azt is megadhatja, `Hotel` a hello típust paraméterként `Documents.Search`, amely közli hello SDK toodeserialize dokumentumok hello keresési eredmények között a következő típusú objektumokat a `Hotel`.

> [!NOTE]
> Hello keresési lekérdezés kifejezés szintaxissal kapcsolatos további információ található [Itt](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).
> 
> 

Végül minden egyes lekérdezés után ez a módszer telepítéseket összes hello találat hello keresési eredmények között, minden egyes dokumentum toohello konzol nyomtatás:

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

Vegyük pedig egyes hello lekérdezések részletes bemutatása. Hello kód tooexecute hello első lekérdezés a következő:

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

Ebben az esetben azt keresünk, amelyek megfelelnek a hello word "költségvetés" hotels, és szeretnénk tooget vissza csak hello Szálloda nevek, hello által megadott `Select` paraméter. Az alábbiakban hello eredmények:

    Name: Roach Motel

Ezt követően azt szeretné, hogy egy éjszakánként sebessége kisebb, mint 150 $ toofind hello szállodák, és térjen vissza a csak a hello Szálloda azonosító és a Leírás:

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

Ez a lekérdezés használ egy OData `$filter` kifejezését `baseRate lt 150`, toofilter hello dokumentumok hello index. További információk a hello, amely támogatja az Azure Search OData-szintaxis található [Itt](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).

Az alábbiakban hello lekérdezési eredmények hello:

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close tootown hall and hello river

A következő szeretnénk toofind hello felső két szállodák, amely rendelkezik lett utoljára felújított, hello Szálloda neve és az utolsó felújítás dátumának megjelenítése. Íme hello kódot: 

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

Ebben az esetben újra használjuk OData szintaxis toospecify hello `OrderBy` paraméterként `lastRenovationDate desc`. Azt is beállíthatja `Top` too2 tooensure jelenleg csak a get hello felső két dokumentumokat. Előtt, hogy állítsa `Select` toospecify mezőket vissza kell adni.

Az alábbiakban hello eredmények:

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Végül azt szeretnénk toofind hello word "motel" megfelelő összes szállodák:

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

Az alábbiakban hello eredményeket, amely az összes mezők szerepelhetnek, mivel jelenleg nem adta meg a hello és `Select` tulajdonság:

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

Ez a lépés befejezése hello oktatóanyag, de itt nem állnak le. **További lépések** Azure Search többet további forrásokat biztosít.

## <a name="next-steps"></a>Következő lépések
* Keresse meg a hivatkozásokat hello hello [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) és [REST API](https://docs.microsoft.com/rest/api/searchservice/).
* A Tudásbázis keresztül elmélyítsék [videókat és más mintákat és oktatóanyagok](search-video-demo-tutorial-list.md).
* Felülvizsgálati [elnevezési konvenciói](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) különböző objektumok elnevezési toolearn hello szabályai.
* Felülvizsgálati [a támogatott adattípusokat](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) az Azure Search.
