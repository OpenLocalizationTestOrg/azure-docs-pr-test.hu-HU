---
title: "AAA \"feltölteni az adatokat (.NET - Azure Search) |} Microsoft dokumentumok\""
description: "Ismerje meg, hogyan tooupload az tooan indexe az Azure Search használatával hello .NET SDK-val."
services: search
documentationcenter: 
author: brjohnstmsft
manager: jhubbard
editor: 
tags: 
ms.assetid: 0e0e7e7b-7178-4c26-95c6-2fd1e8015aca
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 01/13/2017
ms.author: brjohnst
ms.openlocfilehash: 78ddbefb522884d1f61cb275c25c091487aee639
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-tooazure-search-using-hello-net-sdk"></a>Hello feltöltési adatok tooAzure keresés használata a .NET SDK-val
> [!div class="op_single_selector"]
> * [Áttekintés](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

Ez a cikk bemutatja, hogyan toouse hello [Azure Search .NET SDK](https://aka.ms/search-sdk) tooimport adatokat az Azure Search-index.

A bemutató elindítása előtt [létre kell hoznia egy Azure Search-indexet](search-what-is-an-index.md). Ez a cikk feltételezi, hogy már létrehozta a `SearchServiceClient` , ahogy az az objektum [hello .NET SDK használatával Azure Search-index létrehozása](search-create-index-dotnet.md#CreateSearchServiceClient).

> [!NOTE]
> A cikkben szereplő összes példakód C# nyelven van megírva. Hello teljes forráskód található [a Githubon](http://aka.ms/search-dotnet-howto). Hello is olvashat [Azure Search .NET SDK](search-howto-dotnet-sdk.md) egy részletesebb lépésein végighaladva hello mintakódot az.

Az rendelések toopush hello .NET SDK használatával az indexbe szüksége lesz:

1. Hozzon létre egy `SearchIndexClient` objektum tooconnect tooyour search-index.
2. Hozzon létre egy `IndexBatch` hello dokumentumok toobe tartalmazó hozzáadott, módosított vagy törölt.
3. Hello hívás `Documents.Index` metódusában a `SearchIndexClient` toosend hello `IndexBatch` tooyour search-index.

## <a name="create-an-instance-of-hello-searchindexclient-class"></a>Hello SearchIndexClient osztály példányának létrehozása
az index használatával tooimport adatimportáláshoz hello Azure Search .NET SDK, szüksége lesz a toocreate hello példányának `SearchIndexClient` osztály. Hogyan hozhat létre ehhez a példányhoz magát, de az könnyebb Ha már rendelkezik egy `SearchServiceClient` példány toocall a `Indexes.GetClient` metódust. Például ez hogyan be kellene szereznie egy `SearchIndexClient` hello index a "Hotels nevet" adtuk a `SearchServiceClient` nevű `serviceClient`:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> A keresőalkalmazásokban az indexkezelést és -feltöltést általában a keresési lekérdezésektől eltérő elem végzi. `Indexes.GetClient`előnyei index feltöltése, mert menti azt, hogy egy másik probléma hello `SearchCredentials`. Ennek érdekében átadásakor hello adminisztrációs kulcsot adott meg használt toocreate hello `SearchServiceClient` új toohello `SearchIndexClient`. Azonban hello része az alkalmazás, amely végrehajtja a lekérdezéseket, hogy a rendszer jobb toocreate hello `SearchIndexClient` közvetlenül, hogy egy lekérdezési kulcsot helyett egy adminisztrációs kulcsot is át. Megfelel az hello [legalacsonyabb jogosultsági szint elve](https://en.wikipedia.org/wiki/Principle_of_least_privilege) , és az alkalmazás nagyobb biztonságot nyújt segítséget toomake. További információk az adminisztrációs kulcsok és a lekérdezési kulcsot hello található [Azure Search REST API-referenciában](https://docs.microsoft.com/rest/api/searchservice/).
> 
> 

A `SearchIndexClient` rendelkezik egy `Documents` tulajdonsággal. Ez a tulajdonság biztosít hello módszer közül mindegyik tooadd, kell módosítani, törléséhez vagy dokumentumok az index lekérdezése.

## <a name="decide-which-indexing-action-toouse"></a>Döntse el, melyik indexelési művelet toouse
tooimport adatok hello .NET SDK használatával, szüksége lesz toopackage azokat az adatokat egy `IndexBatch` objektum. Egy `IndexBatch` magában foglalja a gyűjteménye `IndexAction` objektumok, amelyek mindegyike tartalmaz egy dokumentumot, és olyan tulajdonságon, amely közli az Azure Search milyen művelet tooperform a dokumentumon (feltöltési, egyesítési, törlés, stb.). Attól függően, amely alatt úgy dönt, műveletek hello csak bizonyos mezők szerepelnie kell függvénykötésnek nyilvántartott egyes dokumentumok:

| Műveletek | Leírás | Az egyes dokumentumok kötelező mezői | Megjegyzések |
| --- | --- | --- | --- |
| `Upload` |Egy `Upload` művelete hasonló tooan "upsert", ahol hello dokumentum lesz beszúrni, ha új, majd frissíteni vagy lecserélni rá. |billentyűt, és bármely más mezők toodefine kívánja |Ha egy meglévő dokumentum frissítése vagy cseréje, bármely hello kérelemben nincs megadva mező rendelkezik-e a mező értéke túl`null`. Ez akkor fordul elő, akkor is, ha hello mező korábban megadott tooa értéke nem lehet null. |
| `Merge` |Egy meglévő dokumentum hello található frissítések megadott mezőket. Ha hello dokumentum hello index nem létezik, hello egyesítés sikertelen lesz. |billentyűt, és bármely más mezők toodefine kívánja |Bármely egyesítésével megadott mező felül fogja írni a létező mező hello hello dokumentumban. Ez `DataType.Collection(DataType.String)` típusú mezőket tartalmaz. Például akkor, ha hello dokumentum tartalmaz egy mező `tags` értékű `["budget"]` és értékkel egyesítésével végrehajtása `["economy", "pool"]` a `tags`, végső értéke hello hello `tags` mező kitöltése `["economy", "pool"]`. Nem pedig a következő lesz: `["budget", "economy", "pool"]`. |
| `MergeOrUpload` |Ez a művelet viselkedik `Merge` Ha hello kulcs már megadott dokumentum hello index már létezik. Ha hello dokumentum nem létezik, hasonlóan viselkedik `Upload` egy új dokumentumot. |billentyűt, és bármely más mezők toodefine kívánja |- |
| `Delete` |Hello megadott dokumentum eltávolítása hello index. |csak billentyű |A mezőket, akkor adjon meg eltérő hello kulcsmező figyelmen kívül hagyja. Ha azt szeretné, hogy a dokumentum egy egyéni mező tooremove, `Merge` helyette és egyszerűen állítsa be az hello mezőt explicit módon toonull. |

Megadhatja, mi a teendő a toouse kívánt hello statikus módszerek hello `IndexBatch` és `IndexAction` osztályok, lásd a következő szakaszban hello.

## <a name="construct-your-indexbatch"></a>Az IndexBatch létrehozása
Most, hogy tudja, hogy mely műveletek tooperform a dokumentumokon, készen áll a tooconstruct hello áll `IndexBatch`. az alábbi példa hogyan hello toocreate néhány különféle műveletek kötegben. Vegye figyelembe, hogy a fenti példában nevű egyéni osztály használja `Hotel` , amely leképezhető tooa dokumentum hello "Hotels nevű" index.

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
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
            }),
        IndexAction.Upload(
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
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close tootown hall and hello river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

Ebben az esetben használjuk `Upload`, `MergeOrUpload`, és `Delete` a keresési műveletként hello hívása hello módszerek által megadott `IndexAction` osztály.

Jelen példában feltételezzük, hogy a „hotels” index már fel van töltve dokumentumokkal. Vegye figyelembe, hogyan jelenleg nem rendelkezett toospecify minden hello lehetséges dokumentum mező használata esetén `MergeOrUpload` , és hogyan azt csak meghatározott hello dokumentum kulcs (`HotelId`) használatakor `Delete`.

Emellett vegye figyelembe, hogy csak egyetlen indexelési kérelem too1000 dokumentumainak fel is megadható.

> [!NOTE]
> Ebben a példában a különféle műveletek toodifferent dokumentumok alkalmazzák azt. Ha tooperform hello kötegelt hívása helyett az összes dokumentum ugyanazokat a műveleteket hello `IndexBatch.New`, használhat más statikus módszereket hello `IndexBatch`. Kötegeket például az `IndexBatch.Merge`, `IndexBatch.MergeOrUpload` vagy `IndexBatch.Delete` művelet meghívásával is létrehozhat. Ezen módszerek `IndexAction` objektumok helyett dokumentumok gyűjteményét (jelen példában a `Hotel` típusú objektumokat) használják.
> 
> 

## <a name="import-data-toohello-index"></a>Importálás az toohello indexe
Most, hogy már rendelkezik egy inicializált `IndexBatch` objektum, elküldheti azt toohello index meghívásával `Documents.Index` a a `SearchIndexClient` objektum. a következő példa azt mutatja meg hogyan hello toocall `Index`, valamint néhány további lépést tooperform szüksége lesz:

```csharp
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
```

Megjegyzés: hello `try` / `catch` hello hívás toohello körülvevő `Index` metódust. hello catch blokkon kezeli az egyik fontos hiba esetében az indexeléshez. Ha az Azure Search szolgáltatás sikertelen tooindex néhány hello hello kötegben, dokumentumok egy `IndexBatchException` által okozott `Documents.Index`. Ez akkor történhet meg, ha olyankor végzi a dokumentumok indexelését, amikor a szolgáltatás nagy terhelés alatt áll. **Javasoljuk, hogy a kódban explicit módon kezelje ezt az esetet.** Késleltetés, és próbálkozzon újra a sikertelen indexelési hello dokumentumok, vagy jelentkezzen és például hello minta nem, vagy valami mást, attól függően, hogy az alkalmazás konzisztenciájának követelményeinek elvégezhető folytatásához.

Végezetül hello kód hello példában két másodpercen késések fent. Indexelő történik aszinkron módon történik az Azure Search szolgáltatásban, így hello mintaalkalmazás kell toowait egy rövid időn belül tooensure, hogy a keresés hello dokumentumok is. Ilyen mértékű késleltetésre kizárólag demók, tesztek és mintaalkalmazások esetében van szükség.

<a name="HotelClass"></a>

### <a name="how-hello-net-sdk-handles-documents"></a>Hogyan kezeli a .NET SDK hello a dokumentumok
Előfordulhat, hogy lehet szeretné megtudni, hogy hogyan hello Azure Search .NET SDK például a felhasználói osztály példányainak képes tooupload `Hotel` toohello index. toohelp válaszolja meg, hogy a kérdést, nézzük hello `Hotel` osztály, amely meghatározott toohello sémát indexeli [hello .NET SDK használatával Azure Search-index létrehozása](search-create-index-dotnet.md#DefineIndex):

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [Key]
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

    // ToString() method omitted for brevity...
}
```

hello először thing toonotice, hogy minden egyes nyilvános tulajdonsága `Hotel` felel meg a hello Indexdefiníció, de egy fontos különbséggel tooa mező: hello minden mező kezdetű névvel rendelkező visszaalakítja ("teve eset"), közben minden nyilvános hello neve a tulajdonság `Hotel` egy nagybetűt ("Pascal eset") kezdődik. Ez az a közös hajtható végre adatkötés, ahol hello cél séma az alkalmazásfejlesztő hello külső hello irányítását .NET-alkalmazások esetén. Ahelyett, hogy a tooviolate hello .NET irányelvek elnevezési azáltal, hogy a tulajdonság nevét teve eset, megadható, hogy a hello SDK toomap hello tulajdonság nevének toocamel-kódaláírásokra automatikusan hello `[SerializePropertyNamesAsCamelCase]` attribútum.

> [!NOTE]
> hello Azure Search .NET SDK-t használ a hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) könyvtár tooserialize és deszerializálható JSON formátumból az egyéni modell objektumok tooand. A szerializálás szükség szerint testre szabható. További információk: [Egyéni szerializálás a JSON.NET használatával](search-howto-dotnet-sdk.md#JsonDotNet). Egy példa erre, hello hello használata `[JsonProperty]` hello attribútum `DescriptionFr` a fenti hello mintakód tulajdonság.
> 
> 

hello kapcsolatos fontos dolog második hello `Hotel` osztály hello adattípusok hello nyilvános tulajdonságainak. hello .NET ezeket a tulajdonságokat képezi le hello Indexdefiníció tootheir egyenértékű mező típusa. Például hello `Category` karakterlánc típusú tulajdonság leképezhető toohello `category` mező, amelynek a típusa `DataType.String`. Hasonló típusleképezés történik a `bool?` és `DataType.Boolean`, illetve a `DateTimeOffset?` és `DataType.DateTimeOffset` stb. között is. hello leképezésének meghatározott szabályai hello szerepelnek a hello `Documents.Get` metódus a hello [Azure Search .NET SDK-referencia](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).

Ez lehetőséget toouse saját osztályok-dokumentumokként működik mindkét irányban; Keresési eredmények beolvasásához és is rendelkezik hello SDK automatikusan deszerializálni őket tooa típus az Ön által választott, ahogy az hello [tovább cikk](search-query-dotnet.md).

> [!NOTE]
> hello Azure Search .NET SDK-t is támogatja a hello segítségével dinamikusan gépelt dokumentumok `Document` osztályt, amely nevek toofield mezőértékek kulcs/érték hozzárendelése. Ez olyan esetekben hasznos, ha hello a sémát indexeli tervezéskor nem tudja, vagy ha kényelmetlen toobind toospecific modell osztályok lenne. Minden hello módszerek hello SDK a dokumentumok kezelése rendelkezik, amelyek használhatók a hello túlterhelések `Document` osztály, valamint az erős típusmegadású túlterhelések, amelyek általános típusú paramétert. Ez utóbbi csak hello hello mintakód ebben a cikkben szerepelnek.
> 
> 

**Miért használjon nullázható adattípusokat?**

A saját modell osztályok toomap tooan Azure Search-index tervezésekor ajánlott érték típusú tulajdonságok például deklaráló `bool` és `int` toobe nullázható (például `bool?` helyett `bool`). Ha egy nem nullázható tulajdonságot használja, akkor túl**garantálni** , hogy az indexben nem dokumentumok hello megfelelő mező null értéket tartalmaz. Hello SDK sem hello Azure Search szolgáltatás segítségével Ön tooenforce ez.

Ez nem csak egy elméleti szempont: képzelhető el, egy olyan forgatókönyvet, ahol hozzáadhat egy új tooan meglévő mezőindex, amely típusú `DataType.Int32`. Hello index definíciójának frissítése után összes dokumentumot lesz az új mező null értéket (mivel az Azure Search nullázható minden esetében). Ha az egy nem nullázható majd használja egy modell osztály `int` tulajdonság, mező, elérhetővé válik egy `JsonSerializationException` ez, például tooretrieve dokumentumok tett kísérlet során:

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

Ezért javasoljuk, hogy a modellosztályokban nullázható értéktípusokat használjon.

## <a name="next-steps"></a>Következő lépések
Után feltöltése az Azure Search-index, a lekérdezések toosearch dokumentumok kiállító készen toostart lesz. Részletes információk: [Az Azure Search-index lekérdezése](search-query-overview.md).

