---
title: "aaaUpgrading toohello Azure Search .NET SDK 1.1-es verzió |} Microsoft Docs"
description: "Toohello Azure Search .NET SDK 1.1-es verziójának frissítése"
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 66f89958-a320-4a24-87f9-69315848909f
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/11/2017
ms.author: brjohnst
ms.openlocfilehash: 291ae5731546e47b3c22c721d3552a79bdea80c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrading-toohello-azure-search-net-sdk-version-3"></a>Azure Search .NET SDK 3-as verziójú toohello frissítése
Ha szeretne megtudni a 2.0-preview vagy hello a régebbi verziója [Azure Search .NET SDK](https://aka.ms/search-sdk), ez a cikk segít frissíteni az alkalmazás toouse verziót 3.

Többek között a következőket példákért lásd: hello SDK általános bemutatóért [hogyan toouse Azure keresési .NET-alkalmazás](search-howto-dotnet-sdk.md).

Hello Azure Search .NET SDK 3-as verziója korábbi verzióihoz képest végrehajtott egyes módosításokat tartalmazza. Ezek a legtöbbször kisebb, így a kód módosítása elvégzéséhez szükséges csak csatlakozhatnak. Lásd: [lépéseket tooupgrade](#UpgradeSteps) útmutatást toochange a kód toouse hello új SDK-verzió.

> [!NOTE]
> Verzió 1.0.2-preview használata vagy régebbi, kell először frissítenie tooversion 1.1, és frissítse a tooversion 3. Lásd: [függelék: lépéseket tooupgrade tooversion 1.1](#UpgradeStepsV1) utasításokat.
>
> Az Azure Search szolgáltatás példányát támogatja több REST API-t, többek között a legújabb hello. Egy verzió toouse tovább már nem hello legújabb, de azt javasoljuk, hogy áttelepítette-e a kód toouse hello legújabb verziója. Hello REST API használata esetén minden kérelemben hello api-version paraméter segítségével adjon meg a hello API-verzió. Hello .NET SDK használata esetén a hello használata SDK hello verziója hello megfelelő hello REST API verziója határozza meg. Ha egy régebbi SDK használ, folytathatja toorun ezt a kódot, módosítások nélküli akkor is, ha hello szolgáltatás frissített toosupport egy újabb API-verzió.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a>3. verzió újdonságai
Hello Azure Search .NET SDK célok hello legújabb 3-as verziója hello Azure Search REST API, kifejezetten 2016-09-01 általánosan elérhető verziójának. Ez teszi lehetővé toouse számos új szolgáltatást nyújt az Azure Search .NET-alkalmazás, beleértve a következőket hello:

* [Egyéni elemzők](https://aka.ms/customanalyzers)
* [Az Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) és [Azure Table Storage](search-howto-indexing-azure-tables.md) indexelő támogatása
* Az indexelő testreszabási keresztül [hozzárendelések mezőben](search-indexer-field-mappings.md)
* ETag-EK támogatja tooenable biztonságos egyidejű index definíciók, az indexelők és az adatok források
* Index mező definíciók deklarációval létrehozásának dekoráció a modellosztállyal, és használja új hello támogatása `FieldBuilder` osztály.
* A .NET Core és a hordozható .NET-profil 111 támogatása

<a name="UpgradeSteps"></a>

## <a name="steps-tooupgrade"></a>Lépéseket tooupgrade
Először frissítse a NuGet referencia `Microsoft.Azure.Search` vagy hello NuGet-Csomagkezelő konzol használatával vagy az csomagot jobb gombbal a projekt hivatkozásait, majd válassza a "Kezelése NuGet csomagjainak..." a Visual Studióban.

Miután NuGet töltött hello új csomagok és a függőségek, a projekt újraépítéséhez. Attól függően, hogy a kód hogyan épül akkor előfordulhat, hogy újraépítése sikeresen. Ha igen, most készen áll a toogo!

Ha a build nem sikerül, egy összeállítási hibát hasonló hello kell megjelennie:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' too'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

következő lépés hello toofix a build hiba van. Lásd: [módosítások Megtörje a 3-as verziójú](#ListOfChanges) talál részletes információt mi hello hiba okozza, és hogyan toofix azt.

Láthatja, hogy további build figyelmeztetések kapcsolódó tooobsolete metódusokra vagy tulajdonságokra. hello figyelmeztetések milyen toouse inkább a hello elavult funkció utasításokat tartalmazza. Például, ha az alkalmazás hello `IndexingParameters.Base64EncodeKeys` tulajdonság, kell megjelenik egy figyelmeztetés, amely szerint`"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`

Build hibák megszüntetése után módosíthatja tooyour alkalmazás tootake új funkciók előnyeit Ha. Az hello SDK új funkciói részletes leírást talál [What's new in 3-as verziójú](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a>3-as verziójú megtörje változásai
Van néhány jelentős változásokat a 3-as verzió, amelyre szüksége lehet a kód vált továbbá toorebuilding az alkalmazás.

### <a name="indexesgetclient-return-type"></a>Indexes.GetClient visszatérési típusa
Hello `Indexes.GetClient` módszer új visszatérési értékkel rendelkezik. Korábban, visszaadott `SearchIndexClient`, azonban ez megváltozott túl`ISearchIndexClient` 2.0-előzetes verzióját, és hogy módosítás tooversion 3 keresztül végzi. Ez az toomock hello kívánja toosupport ügyfelek `GetClient` egység tesztek utánzatait végrehajtásának visszaadó metódus `ISearchIndexClient`.

#### <a name="example"></a>Példa
Ha a kód így néz ki:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Ez módosítható toothis toofix esetleges hibák létrehozása:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-toostrings"></a>AnalyzerName, adattípus és mások már nem konvertálható erre toostrings
Az Azure Search .NET SDK, amelyek a hello számos különböző `ExtensibleEnum`. Korábban ezek a típusok volt konvertálható összes erre tootype `string`. Azonban programhiba hello felfedezett `Object.Equals` ezen osztályok és rögzítési hello hiba szükséges letiltása a implicit konverzió implementációja. Explicit konverzió túl`string` rendszer nem tagadja meg.

#### <a name="example"></a>Példa
Ha a kód így néz ki:

```csharp
var customTokenizerName = TokenizerName.Create("my_tokenizer"); 
var customTokenFilterName = TokenFilterName.Create("my_tokenfilter"); 
var customCharFilterName = CharFilterName.Create("my_charfilter"); 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        customTokenizerName,  
        new[] { customTokenFilterName },  
        new[] { customCharFilterName }), 
}; 
```

Ez módosítható toothis toofix esetleges hibák létrehozása:

```csharp
const string CustomTokenizerName = "my_tokenizer"; 
const string CustomTokenFilterName = "my_tokenfilter"; 
const string CustomCharFilterName = "my_charfilter"; 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        CustomTokenizerName,  
        new TokenFilterName[] { CustomTokenFilterName },  
        new CharFilterName[] { CustomCharFilterName })
}; 
```

### <a name="removed-obsolete-members"></a>Eltávolított elavult tagok

Build hibák kapcsolódó toomethods vagy volt megjelölve, a 2.0-előzetes verziója elavult, és később eltávolítja a 3-as verziójú tulajdonságokhoz jelenhet meg. Ha ilyen hibákat észlel, hogyan tooresolve őket:

- Ha ez a konstruktor: `ScoringParameter(string name, string value)`, használja helyette a erre:`ScoringParameter(string name, IEnumerable<string> values)`
- Ha használta hello `ScoringParameter.Value` tulajdonság, használjon hello `ScoringParameter.Values` tulajdonság vagy hello `ToString` metódus helyett.
- Ha használta hello `SearchRequestOptions.RequestId` tulajdonság, használjon hello `ClientRequestId` tulajdonság helyette.

### <a name="removed-preview-features"></a>Az eltávolított előzetes verziójú funkciók

Ha verzió 2.0 előzetes verzió tooversion 3 frissít, ne feledje, hogy JSON és a fürt megosztott kötetei szolgáltatás támogatja a Blob indexelők elemzése el lett távolítva, mert ezek a funkciók még még csak előzetes verziójúak. Pontosabban, a következő hello módszerek hello `IndexingParametersExtensions` osztály eltávolításra került:

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Ha az alkalmazás erős függőség az ezen szolgáltatásoktól, csak akkor tudja tooupgrade tooversion 3 az Azure Search .NET SDK hello. Folytatás toouse verzió 2.0 előzetes verzió. Azonban adja a következőket kell figyelembe venni, hogy **megtekintés SDK-k éles környezetben nem javasoljuk**. Előzetes verziójú funkciók csak tesztelési és módosíthatja.

## <a name="conclusion"></a>Összegzés
Ha további részleteket a hello Azure Search .NET SDK használatával van szüksége, tekintse meg a közelmúltban frissített [útmutató](search-howto-dotnet-sdk.md).

Az hello SDK szívesen fogadjuk a visszajelzéseket. Ha problémákat tapasztal, érzi, hogy szabad tooask nekünk a Súgó a hello [Azure Search MSDN fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Ha a hiba, akkor is fájlt problémát hello [Azure .NET SDK GitHub-tárházban](https://github.com/Azure/azure-sdk-for-net/issues). Győződjön meg arról, hogy tooprefix a probléma címének és a "Search SDK:".

Köszönjük, hogy az Azure Search használatával!

<a name="UpgradeStepsV1"></a>

## <a name="appendix-steps-tooupgrade-tooversion-11"></a>A függelék: Lépéseket tooupgrade tooversion 1.1
> [!NOTE]
> Ez a szakasz csak a hello Azure Search .NET SDK-verzió 1.0.2-preview és régebbi toousers vonatkozik.
> 
> 

Először frissítse a NuGet referencia `Microsoft.Azure.Search` vagy hello NuGet-Csomagkezelő konzol használatával vagy az csomagot jobb gombbal a projekt hivatkozásait, majd válassza a "Kezelése NuGet csomagjainak..." a Visual Studióban.

Miután NuGet töltött hello új csomagok és a függőségek, a projekt újraépítéséhez.

Ha korábban a verzió 1.0.0-preview, 1.0.1-preview vagy 1.0.2-preview, hello build sikeres legyen, és készen áll a toogo!

Ha korábban a verzió 0.13.0-preview vagy régebbi, megtekintheti az build hello következő hibákat:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: hello type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

következő lépés hello toofix hello fordítási hibákat egyenként. A legtöbb módosítás, néhány osztály- és metódus nevek hello SDK átnevezett van szükség. [Módosítások megtörje 1.1-es verzió listája](#ListOfChangesV1) neve módosítások listáját tartalmazza.

Ha használata egyéni osztályok toomodel dokumentumait, és azon osztályok nem nullázható egyszerű típusú tulajdonságok (például `int` vagy `bool` C#), nincs hibajavítás hello SDK, amelynek tudnia kell hello 1.1-es verzióját. Lásd: [hibajavítások 1.1-es verzió](#BugFixesV1) további részleteket.

Végezetül build hibák megszüntetése után módosíthatja tooyour alkalmazás tootake új funkciók előnyeit Ha.

<a name="ListOfChangesV1"></a>

### <a name="list-of-breaking-changes-in-version-11"></a>Módosítások megtörje 1.1-es verzió listája
hello alábbi van rendezve hello valószínűsége, hogy hello érinteni fogja az alkalmazás kódjában.

#### <a name="indexbatch-and-indexaction-changes"></a>IndexBatch és IndexAction módosítása
`IndexBatch.Create`túl át lett nevezve`IndexBatch.New` és már nem rendelkezik egy `params` argumentum. Használhat `IndexBatch.New` kötegek, amely különböző típusú műveletek (összevonása, törlés stb.) kombinálhatók. Emellett nincsenek új statikus módszereket biztosít a kötegek ahol minden hello műveletek vannak hello ugyanaz: `Delete`, `Merge`, `MergeOrUpload`, és `Upload`.

`IndexAction`már nem rendelkezik nyilvános konstruktorral, és most nem módosíthatók a tulajdonságait. Hello új statikus metódusok többféle célra műveletek létrehozásához kell használnia: `Delete`, `Merge`, `MergeOrUpload`, és `Upload`. `IndexAction.Create`el lett távolítva. Ha hello rendelkező túlterhelést csak egy dokumentumot, győződjön meg arról, hogy toouse `Upload` helyette.

##### <a name="example"></a>Példa
Ha a kód így néz ki:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Ez módosítható toothis toofix esetleges hibák létrehozása:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Ha azt szeretné, hogy tovább egyszerűsítheti az toothis:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>IndexBatchException módosítások
Hello `IndexBatchException.IndexResponse` tulajdonság túl át lett nevezve`IndexingResults`, és a típus már `IList<IndexingResult>`.

##### <a name="example"></a>Példa
Ha a kód így néz ki:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed tooindex some of hello documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Ez módosítható toothis toofix esetleges hibák létrehozása:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed tooindex some of hello documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

#### <a name="operation-method-changes"></a>A művelet metódus változások
Hello Azure Search .NET SDK minden műveletet a szinkron és aszinkron hívóknak metódus túlterhelések készletként van közzétéve. hello aláírások és az e metódus túlterhelések faktoring módosítva 1.1-es verzió.

Például hello "Index statisztika Get" művelet hello SDK korábbi verzióiban érhető el ezen aláírások:

A `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

A `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

hello metódus-aláírása az 1.1-es verzió hely ilyen műveletet hello:

A `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

A `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

1.1-es verziójától kezdve, hello Azure Search .NET SDK rendszerezi művelet módszerek másképp:

* Választható paraméterek: most már van modellezve, ahelyett, hogy a paraméterek alapértelmezett további metódus túlterhelések-nál. Ez egyes esetekben jelentős mértékben csökkenti a metódus túlterhelések hello száma.
* hello kiterjesztésmetódusok most hello idegen részleteit HTTP hello hívó számos elrejtése. Például SDK visszaadott HTTP-állapotkódot, melyet gyakran egy válasz objektum hello régebbi verziói nem kell toocheck mivel művelet módszerek throw `CloudException` bármely status kód, amely jelzi a hiba. Új bővítmény metódusok csak visszatérési modellobjektumokat hello, mentését, hogy nem kell tekintettel toounwrap hello őket a kódban.
* Ezzel ellentétben hello core felületek most teszi közzé módszereket, amelyek jobban irányítható hello HTTP szinten ha esetleg szükség lenne rá. A HTTP-fejlécek egyéni toobe a kéréseket, és új hello most átadhatók `AzureOperationResponse<T>` visszatérési típus közvetlen hozzáférést toohello lehetővé teszi `HttpRequestMessage` és `HttpResponseMessage` hello a művelethez. `AzureOperationResponse`hello meghatározott `Microsoft.Rest.Azure` névtér és a felváltja `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>ScoringParameters módosítások
Egy új osztályt `ScoringParameter` már hozzáadta a hello legújabb SDK toomake keresési lekérdezés könnyebb tooprovide paraméterek tooscoring profilok. Korábban hello `ScoringProfiles` hello tulajdonságának `SearchParameters` osztály volt megadva, `IList<string>`; Most írta-e be `IList<ScoringParameter>`.

##### <a name="example"></a>Példa
Ha a kód így néz ki:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Ez módosítható toothis toofix esetleges hibák létrehozása: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Modellmódosításokkal osztály
Ismertetett toohello aláírás módosítások miatt [művelet metódus módosítások](#OperationMethodChanges), sok osztály a hello `Microsoft.Azure.Search.Models` névtér átnevezték vagy eltávolítani. Példa:

* `IndexDefinitionResponse`lett cserélve`AzureOperationResponse<Index>`
* `DocumentSearchResponse`túl át lett nevezve`DocumentSearchResult`
* `IndexResult`túl át lett nevezve`IndexingResult`
* `Documents.Count()`most adja vissza egy `long` hello dokumentum számával, hanem egy`DocumentCountResponse`
* `IndexGetStatisticsResponse`túl át lett nevezve`IndexGetStatisticsResult`
* `IndexListResponse`túl át lett nevezve`IndexListResult`

toosummarize, `OperationResponse`-származtatott osztályok, hogy csak egy modell objektum el lettek távolítva toowrap. hello fennmaradó osztályok rendelkezésére állt-e a változása utótag `Response` túl`Result`.

##### <a name="example"></a>Példa
Ha a kód így néz ki:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Ez módosítható toothis toofix esetleges hibák létrehozása:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Válasz osztályok és IEnumerable
Egy további, a kód érintő módosítást a válasz osztályok gyűjtemények vonatkozik, amelyek már nem valósítja meg `IEnumerable<T>`. Ehelyett hello gyűjteménytulajdonság közvetlenül is elérheti. Ha például a kód néz ki:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Ez módosítható toothis toofix esetleges hibák létrehozása:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="special-case-for-web-applications"></a>Webes alkalmazásokhoz, különleges esetben
Ha a webes alkalmazás az rendezi sorba `DocumentSearchResponse` közvetlen toosend keresési találatok toohello böngésző, szüksége lesz toochange a kódot, vagy hello eredmények nem fogja szerializálni megfelelően. Ha például a kód néz ki:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want toosearch everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Módosíthatja az első hello `.Results` hello keresési válasz toofix keresési eredmény Renderelés tulajdonság:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want toosearch everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Hogy toolook ilyen esetekben a kódban. **hello fordító nem figyelmeztet,** mert `JsonResult.Data` típusú `object`.

#### <a name="cloudexception-changes"></a>CloudException módosítások
Hello `CloudException` osztály hello tért át `Hyak.Common` névtér toohello `Microsoft.Rest.Azure` névtér. Emellett a `Error` tulajdonság túl át lett nevezve`Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>SearchServiceClient és SearchIndexClient módosítása
hello típusú hello `Credentials` tulajdonság megváltozott `SearchCredentials` tooits alaposztály, `ServiceClientCredentials`. Ha tooaccess hello kell `SearchCredentials` , egy `SearchIndexClient` vagy `SearchServiceClient`, használjon új hello `SearchCredentials` tulajdonság.

Az SDK-val hello régebbi verzióit `SearchServiceClient` és `SearchIndexClient` konstruktorokat, amelyek került volna egy `HttpClient` paraméter. Ezek konstruktorok használatát, amelyek helyett egy `HttpClientHandler` és tömbje `DelegatingHandler` objektumok. Így könnyebben tooinstall egyéni kezelők toopre-folyamat HTTP-kérelmek szükség esetén.

Végezetül hello konstruktorokat, amelyek tartott egy `Uri` és `SearchCredentials` megváltoztak. Ha például rendelkezik kódot, amely a következőképpen néz ki:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Ez módosítható toothis toofix esetleges hibák létrehozása:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Vegye figyelembe, hogy a hello hello típusú paraméter hitelesítő adatok is a túl megváltozott`ServiceClientCredentials`. Ez az valószínű tooaffect a kód óta `SearchCredentials` származó `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>A kérelem azonosító továbbításához
Hello SDK régebbi verzióit, beállíthat egy Kérelemazonosító a hello `SearchServiceClient` vagy `SearchIndexClient` és az összes kérelem toohello REST API szerepel. Ez akkor hasznos, kapcsolatos hibák elhárításához a keresőszolgáltatása Ha Ha toocontact támogatásra van szüksége. Azonban is hasznos tooset egyedi Kérelemazonosító minden művelethez helyett toouse hello minden műveletnél ugyanazzal az Azonosítóval. Emiatt hello `SetClientRequestId` módszerek `SearchServiceClient` és `SearchIndexClient` el lettek távolítva. Ehelyett átadhatók egy kérelem azonosítója tooeach műveleti metódusának keresztül hello választható `SearchRequestOptions` paraméter.

> [!NOTE]
> Hello SDK későbbi kiadásában egy új mechanizmust beállításához a Kérelemazonosító globálisan hello ügyfélen objektumok azonos-e más Azure SDK-k által használt hello megközelítés adjuk hozzá.
> 
> 

#### <a name="example"></a>Példa
Ha kódot, amely a következőképpen néz ki:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Ez módosítható toothis toofix esetleges hibák létrehozása:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Csatoló nevének módosítása
hello művelet csoport illesztőneveket rendelkezik a megfelelő tulajdonságneveket konzisztens módosított toobe:

* hello típusú `ISearchServiceClient.Indexes` át lett nevezve a `IIndexOperations` túl`IIndexesOperations`.
* hello típusú `ISearchServiceClient.Indexers` át lett nevezve a `IIndexerOperations` túl`IIndexersOperations`.
* hello típusú `ISearchServiceClient.DataSources` át lett nevezve a `IDataSourceOperations` túl`IDataSourcesOperations`.
* hello típusú `ISearchIndexClient.Documents` át lett nevezve a `IDocumentOperations` túl`IDocumentsOperations`.

Ez a változás esetén valószínű tooaffect a kód mocks ezen felületek tesztelési célból létrehozott.

<a name="BugFixesV1"></a>

### <a name="bug-fixes-in-version-11"></a>Hibajavítások 1.1-es verzió
Hiba történt a hello Azure Search .NET SDK vonatkozó tooserialization egyéni modell osztályok régebbi verzióit. hello hiba akkor fordulhat elő, ha létrehozott egy egyéni modellosztállyal egy nem nullázható értéktípus tulajdonsággal.

#### <a name="steps-tooreproduce"></a>Lépéseket tooreproduce
Hozzon létre egy egyéni modellosztállyal egy tulajdonság nem nullázható értéktípus. Például hozzáadhat egy nyilvános `UnitCount` típusú tulajdonság `int` helyett `int?`.

Ha egy dokumentum hello alapértelmezett érték az adott típusú indexeli (például 0 `int`), hello mező az Azure Search null értékű lesz. Ha ezt követően a dokumentum, hello `Search` hívás kivételhibát `JsonSerializationException` panaszos, amely nem konvertálható `null` túl`int`.

Szűrők is, nem működnek megfelelően, mert null írt toohello index szánt hello érték helyett.

#### <a name="fix-details"></a>Javítsa ki a részletei
A probléma hello SDK 1.1-es verziójában megoldottuk azt. Most, ha van egy modell ehhez hasonló:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

és a beállított `IntValue` too0, hogy értéke most már megfelelően szerializálható kézjegyként 0 hello keresztülhaladnak a hálózaton, és 0 hello indexben tárolt. Kerek esetén is megfelelően működik-e.

Nincs tisztában ezt a módszert egy potenciális problémát toobe: Ha a modell típusa egy nem nullázható tulajdonságot használja, akkor túl**garantálni** , hogy az indexben nem dokumentumok hello megfelelő mező null értéket tartalmaz. Hello SDK sem hello Azure Search REST API segítségével meg tooenforce ez.

Ez nem csak egy elméleti szempont: képzelhető el, egy olyan forgatókönyvet, ahol hozzáadhat egy új tooan meglévő mezőindex, amely típusú `Edm.Int32`. Hello index definíciójának frissítése után összes dokumentumot lesz az új mező null értéket (mivel az Azure Search nullázható minden esetében). Ha az egy nem nullázható majd használja egy modell osztály `int` tulajdonság, mező, elérhetővé válik egy `JsonSerializationException` ez, például tooretrieve dokumentumok tett kísérlet során:

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

Ezért továbbra is javasoljuk, hogy használjon nullázható típusok a modell osztályok az ajánlott eljárás.

Hiba és hello javítás a további részletekért lásd: [a Githubon probléma](https://github.com/Azure/azure-sdk-for-net/issues/1063).

