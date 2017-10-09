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
# <a name="upload-data-tooazure-search-using-hello-net-sdk"></a><span data-ttu-id="e14c7-103">Hello feltöltési adatok tooAzure keresés használata a .NET SDK-val</span><span class="sxs-lookup"><span data-stu-id="e14c7-103">Upload data tooAzure Search using hello .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e14c7-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e14c7-104">Overview</span></span>](search-what-is-data-import.md)
> * [<span data-ttu-id="e14c7-105">.NET</span><span class="sxs-lookup"><span data-stu-id="e14c7-105">.NET</span></span>](search-import-data-dotnet.md)
> * [<span data-ttu-id="e14c7-106">REST</span><span class="sxs-lookup"><span data-stu-id="e14c7-106">REST</span></span>](search-import-data-rest-api.md)
> 
> 

<span data-ttu-id="e14c7-107">Ez a cikk bemutatja, hogyan toouse hello [Azure Search .NET SDK](https://aka.ms/search-sdk) tooimport adatokat az Azure Search-index.</span><span class="sxs-lookup"><span data-stu-id="e14c7-107">This article will show you how toouse hello [Azure Search .NET SDK](https://aka.ms/search-sdk) tooimport data into an Azure Search index.</span></span>

<span data-ttu-id="e14c7-108">A bemutató elindítása előtt [létre kell hoznia egy Azure Search-indexet](search-what-is-an-index.md).</span><span class="sxs-lookup"><span data-stu-id="e14c7-108">Before beginning this walkthrough, you should already have [created an Azure Search index](search-what-is-an-index.md).</span></span> <span data-ttu-id="e14c7-109">Ez a cikk feltételezi, hogy már létrehozta a `SearchServiceClient` , ahogy az az objektum [hello .NET SDK használatával Azure Search-index létrehozása](search-create-index-dotnet.md#CreateSearchServiceClient).</span><span class="sxs-lookup"><span data-stu-id="e14c7-109">This article also assumes that you have already created a `SearchServiceClient` object, as shown in [Create an Azure Search index using hello .NET SDK](search-create-index-dotnet.md#CreateSearchServiceClient).</span></span>

> [!NOTE]
> <span data-ttu-id="e14c7-110">A cikkben szereplő összes példakód C# nyelven van megírva.</span><span class="sxs-lookup"><span data-stu-id="e14c7-110">All sample code in this article is written in C#.</span></span> <span data-ttu-id="e14c7-111">Hello teljes forráskód található [a Githubon](http://aka.ms/search-dotnet-howto).</span><span class="sxs-lookup"><span data-stu-id="e14c7-111">You can find hello full source code [on GitHub](http://aka.ms/search-dotnet-howto).</span></span> <span data-ttu-id="e14c7-112">Hello is olvashat [Azure Search .NET SDK](search-howto-dotnet-sdk.md) egy részletesebb lépésein végighaladva hello mintakódot az.</span><span class="sxs-lookup"><span data-stu-id="e14c7-112">You can also read about hello [Azure Search .NET SDK](search-howto-dotnet-sdk.md) for a more detailed walk through of hello sample code.</span></span>

<span data-ttu-id="e14c7-113">Az rendelések toopush hello .NET SDK használatával az indexbe szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="e14c7-113">In order toopush documents into your index using hello .NET SDK, you will need to:</span></span>

1. <span data-ttu-id="e14c7-114">Hozzon létre egy `SearchIndexClient` objektum tooconnect tooyour search-index.</span><span class="sxs-lookup"><span data-stu-id="e14c7-114">Create a `SearchIndexClient` object tooconnect tooyour search index.</span></span>
2. <span data-ttu-id="e14c7-115">Hozzon létre egy `IndexBatch` hello dokumentumok toobe tartalmazó hozzáadott, módosított vagy törölt.</span><span class="sxs-lookup"><span data-stu-id="e14c7-115">Create an `IndexBatch` containing hello documents toobe added, modified, or deleted.</span></span>
3. <span data-ttu-id="e14c7-116">Hello hívás `Documents.Index` metódusában a `SearchIndexClient` toosend hello `IndexBatch` tooyour search-index.</span><span class="sxs-lookup"><span data-stu-id="e14c7-116">Call hello `Documents.Index` method of your `SearchIndexClient` toosend hello `IndexBatch` tooyour search index.</span></span>

## <a name="create-an-instance-of-hello-searchindexclient-class"></a><span data-ttu-id="e14c7-117">Hello SearchIndexClient osztály példányának létrehozása</span><span class="sxs-lookup"><span data-stu-id="e14c7-117">Create an instance of hello SearchIndexClient class</span></span>
<span data-ttu-id="e14c7-118">az index használatával tooimport adatimportáláshoz hello Azure Search .NET SDK, szüksége lesz a toocreate hello példányának `SearchIndexClient` osztály.</span><span class="sxs-lookup"><span data-stu-id="e14c7-118">tooimport data into your index using hello Azure Search .NET SDK, you will need toocreate an instance of hello `SearchIndexClient` class.</span></span> <span data-ttu-id="e14c7-119">Hogyan hozhat létre ehhez a példányhoz magát, de az könnyebb Ha már rendelkezik egy `SearchServiceClient` példány toocall a `Indexes.GetClient` metódust.</span><span class="sxs-lookup"><span data-stu-id="e14c7-119">You can construct this instance yourself, but it's easier if you already have a `SearchServiceClient` instance toocall its `Indexes.GetClient` method.</span></span> <span data-ttu-id="e14c7-120">Például ez hogyan be kellene szereznie egy `SearchIndexClient` hello index a "Hotels nevet" adtuk a `SearchServiceClient` nevű `serviceClient`:</span><span class="sxs-lookup"><span data-stu-id="e14c7-120">For example, here is how you would obtain a `SearchIndexClient` for hello index named "hotels" from a `SearchServiceClient` named `serviceClient`:</span></span>

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> <span data-ttu-id="e14c7-121">A keresőalkalmazásokban az indexkezelést és -feltöltést általában a keresési lekérdezésektől eltérő elem végzi.</span><span class="sxs-lookup"><span data-stu-id="e14c7-121">In a typical search application, index management and population is handled by a separate component from search queries.</span></span> <span data-ttu-id="e14c7-122">`Indexes.GetClient`előnyei index feltöltése, mert menti azt, hogy egy másik probléma hello `SearchCredentials`.</span><span class="sxs-lookup"><span data-stu-id="e14c7-122">`Indexes.GetClient` is convenient for populating an index because it saves you hello trouble of providing another `SearchCredentials`.</span></span> <span data-ttu-id="e14c7-123">Ennek érdekében átadásakor hello adminisztrációs kulcsot adott meg használt toocreate hello `SearchServiceClient` új toohello `SearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="e14c7-123">It does this by passing hello admin key that you used toocreate hello `SearchServiceClient` toohello new `SearchIndexClient`.</span></span> <span data-ttu-id="e14c7-124">Azonban hello része az alkalmazás, amely végrehajtja a lekérdezéseket, hogy a rendszer jobb toocreate hello `SearchIndexClient` közvetlenül, hogy egy lekérdezési kulcsot helyett egy adminisztrációs kulcsot is át.</span><span class="sxs-lookup"><span data-stu-id="e14c7-124">However, in hello part of your application that executes queries, it is better toocreate hello `SearchIndexClient` directly so that you can pass in a query key instead of an admin key.</span></span> <span data-ttu-id="e14c7-125">Megfelel az hello [legalacsonyabb jogosultsági szint elve](https://en.wikipedia.org/wiki/Principle_of_least_privilege) , és az alkalmazás nagyobb biztonságot nyújt segítséget toomake.</span><span class="sxs-lookup"><span data-stu-id="e14c7-125">This is consistent with hello [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) and will help toomake your application more secure.</span></span> <span data-ttu-id="e14c7-126">További információk az adminisztrációs kulcsok és a lekérdezési kulcsot hello található [Azure Search REST API-referenciában](https://docs.microsoft.com/rest/api/searchservice/).</span><span class="sxs-lookup"><span data-stu-id="e14c7-126">You can find out more about admin keys and query keys in hello [Azure Search REST API reference](https://docs.microsoft.com/rest/api/searchservice/).</span></span>
> 
> 

<span data-ttu-id="e14c7-127">A `SearchIndexClient` rendelkezik egy `Documents` tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="e14c7-127">`SearchIndexClient` has a `Documents` property.</span></span> <span data-ttu-id="e14c7-128">Ez a tulajdonság biztosít hello módszer közül mindegyik tooadd, kell módosítani, törléséhez vagy dokumentumok az index lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="e14c7-128">This property provides all hello methods you need tooadd, modify, delete, or query documents in your index.</span></span>

## <a name="decide-which-indexing-action-toouse"></a><span data-ttu-id="e14c7-129">Döntse el, melyik indexelési művelet toouse</span><span class="sxs-lookup"><span data-stu-id="e14c7-129">Decide which indexing action toouse</span></span>
<span data-ttu-id="e14c7-130">tooimport adatok hello .NET SDK használatával, szüksége lesz toopackage azokat az adatokat egy `IndexBatch` objektum.</span><span class="sxs-lookup"><span data-stu-id="e14c7-130">tooimport data using hello .NET SDK, you will need toopackage up your data into an `IndexBatch` object.</span></span> <span data-ttu-id="e14c7-131">Egy `IndexBatch` magában foglalja a gyűjteménye `IndexAction` objektumok, amelyek mindegyike tartalmaz egy dokumentumot, és olyan tulajdonságon, amely közli az Azure Search milyen művelet tooperform a dokumentumon (feltöltési, egyesítési, törlés, stb.).</span><span class="sxs-lookup"><span data-stu-id="e14c7-131">An `IndexBatch` encapsulates a collection of `IndexAction` objects, each of which contains a document and a property that tells Azure Search what action tooperform on that document (upload, merge, delete, etc).</span></span> <span data-ttu-id="e14c7-132">Attól függően, amely alatt úgy dönt, műveletek hello csak bizonyos mezők szerepelnie kell függvénykötésnek nyilvántartott egyes dokumentumok:</span><span class="sxs-lookup"><span data-stu-id="e14c7-132">Depending on which of hello below actions you choose, only certain fields must be included for each document:</span></span>

| <span data-ttu-id="e14c7-133">Műveletek</span><span class="sxs-lookup"><span data-stu-id="e14c7-133">Action</span></span> | <span data-ttu-id="e14c7-134">Leírás</span><span class="sxs-lookup"><span data-stu-id="e14c7-134">Description</span></span> | <span data-ttu-id="e14c7-135">Az egyes dokumentumok kötelező mezői</span><span class="sxs-lookup"><span data-stu-id="e14c7-135">Necessary fields for each document</span></span> | <span data-ttu-id="e14c7-136">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="e14c7-136">Notes</span></span> |
| --- | --- | --- | --- |
| `Upload` |<span data-ttu-id="e14c7-137">Egy `Upload` művelete hasonló tooan "upsert", ahol hello dokumentum lesz beszúrni, ha új, majd frissíteni vagy lecserélni rá.</span><span class="sxs-lookup"><span data-stu-id="e14c7-137">An `Upload` action is similar tooan "upsert" where hello document will be inserted if it is new and updated/replaced if it exists.</span></span> |<span data-ttu-id="e14c7-138">billentyűt, és bármely más mezők toodefine kívánja</span><span class="sxs-lookup"><span data-stu-id="e14c7-138">key, plus any other fields you wish toodefine</span></span> |<span data-ttu-id="e14c7-139">Ha egy meglévő dokumentum frissítése vagy cseréje, bármely hello kérelemben nincs megadva mező rendelkezik-e a mező értéke túl`null`.</span><span class="sxs-lookup"><span data-stu-id="e14c7-139">When updating/replacing an existing document, any field that is not specified in hello request will have its field set too`null`.</span></span> <span data-ttu-id="e14c7-140">Ez akkor fordul elő, akkor is, ha hello mező korábban megadott tooa értéke nem lehet null.</span><span class="sxs-lookup"><span data-stu-id="e14c7-140">This occurs even when hello field was previously set tooa non-null value.</span></span> |
| `Merge` |<span data-ttu-id="e14c7-141">Egy meglévő dokumentum hello található frissítések megadott mezőket.</span><span class="sxs-lookup"><span data-stu-id="e14c7-141">Updates an existing document with hello specified fields.</span></span> <span data-ttu-id="e14c7-142">Ha hello dokumentum hello index nem létezik, hello egyesítés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="e14c7-142">If hello document does not exist in hello index, hello merge will fail.</span></span> |<span data-ttu-id="e14c7-143">billentyűt, és bármely más mezők toodefine kívánja</span><span class="sxs-lookup"><span data-stu-id="e14c7-143">key, plus any other fields you wish toodefine</span></span> |<span data-ttu-id="e14c7-144">Bármely egyesítésével megadott mező felül fogja írni a létező mező hello hello dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="e14c7-144">Any field you specify in a merge will replace hello existing field in hello document.</span></span> <span data-ttu-id="e14c7-145">Ez `DataType.Collection(DataType.String)` típusú mezőket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e14c7-145">This includes fields of type `DataType.Collection(DataType.String)`.</span></span> <span data-ttu-id="e14c7-146">Például akkor, ha hello dokumentum tartalmaz egy mező `tags` értékű `["budget"]` és értékkel egyesítésével végrehajtása `["economy", "pool"]` a `tags`, végső értéke hello hello `tags` mező kitöltése `["economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="e14c7-146">For example, if hello document contains a field `tags` with value `["budget"]` and you execute a merge with value `["economy", "pool"]` for `tags`, hello final value of hello `tags` field will be `["economy", "pool"]`.</span></span> <span data-ttu-id="e14c7-147">Nem pedig a következő lesz: `["budget", "economy", "pool"]`.</span><span class="sxs-lookup"><span data-stu-id="e14c7-147">It will not be `["budget", "economy", "pool"]`.</span></span> |
| `MergeOrUpload` |<span data-ttu-id="e14c7-148">Ez a művelet viselkedik `Merge` Ha hello kulcs már megadott dokumentum hello index már létezik.</span><span class="sxs-lookup"><span data-stu-id="e14c7-148">This action behaves like `Merge` if a document with hello given key already exists in hello index.</span></span> <span data-ttu-id="e14c7-149">Ha hello dokumentum nem létezik, hasonlóan viselkedik `Upload` egy új dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="e14c7-149">If hello document does not exist, it behaves like `Upload` with a new document.</span></span> |<span data-ttu-id="e14c7-150">billentyűt, és bármely más mezők toodefine kívánja</span><span class="sxs-lookup"><span data-stu-id="e14c7-150">key, plus any other fields you wish toodefine</span></span> |- |
| `Delete` |<span data-ttu-id="e14c7-151">Hello megadott dokumentum eltávolítása hello index.</span><span class="sxs-lookup"><span data-stu-id="e14c7-151">Removes hello specified document from hello index.</span></span> |<span data-ttu-id="e14c7-152">csak billentyű</span><span class="sxs-lookup"><span data-stu-id="e14c7-152">key only</span></span> |<span data-ttu-id="e14c7-153">A mezőket, akkor adjon meg eltérő hello kulcsmező figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="e14c7-153">Any fields you specify other than hello key field will be ignored.</span></span> <span data-ttu-id="e14c7-154">Ha azt szeretné, hogy a dokumentum egy egyéni mező tooremove, `Merge` helyette és egyszerűen állítsa be az hello mezőt explicit módon toonull.</span><span class="sxs-lookup"><span data-stu-id="e14c7-154">If you want tooremove an individual field from a document, use `Merge` instead and simply set hello field explicitly toonull.</span></span> |

<span data-ttu-id="e14c7-155">Megadhatja, mi a teendő a toouse kívánt hello statikus módszerek hello `IndexBatch` és `IndexAction` osztályok, lásd a következő szakaszban hello.</span><span class="sxs-lookup"><span data-stu-id="e14c7-155">You can specify what action you want toouse with hello various static methods of hello `IndexBatch` and `IndexAction` classes, as shown in hello next section.</span></span>

## <a name="construct-your-indexbatch"></a><span data-ttu-id="e14c7-156">Az IndexBatch létrehozása</span><span class="sxs-lookup"><span data-stu-id="e14c7-156">Construct your IndexBatch</span></span>
<span data-ttu-id="e14c7-157">Most, hogy tudja, hogy mely műveletek tooperform a dokumentumokon, készen áll a tooconstruct hello áll `IndexBatch`.</span><span class="sxs-lookup"><span data-stu-id="e14c7-157">Now that you know which actions tooperform on your documents, you are ready tooconstruct hello `IndexBatch`.</span></span> <span data-ttu-id="e14c7-158">az alábbi példa hogyan hello toocreate néhány különféle műveletek kötegben.</span><span class="sxs-lookup"><span data-stu-id="e14c7-158">hello example below shows how toocreate a batch with a few different actions.</span></span> <span data-ttu-id="e14c7-159">Vegye figyelembe, hogy a fenti példában nevű egyéni osztály használja `Hotel` , amely leképezhető tooa dokumentum hello "Hotels nevű" index.</span><span class="sxs-lookup"><span data-stu-id="e14c7-159">Note that our example uses a custom class called `Hotel` that maps tooa document in hello "hotels" index.</span></span>

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

<span data-ttu-id="e14c7-160">Ebben az esetben használjuk `Upload`, `MergeOrUpload`, és `Delete` a keresési műveletként hello hívása hello módszerek által megadott `IndexAction` osztály.</span><span class="sxs-lookup"><span data-stu-id="e14c7-160">In this case, we are using `Upload`, `MergeOrUpload`, and `Delete` as our search actions, as specified by hello methods called on hello `IndexAction` class.</span></span>

<span data-ttu-id="e14c7-161">Jelen példában feltételezzük, hogy a „hotels” index már fel van töltve dokumentumokkal.</span><span class="sxs-lookup"><span data-stu-id="e14c7-161">Assume that this example "hotels" index is already populated with a number of documents.</span></span> <span data-ttu-id="e14c7-162">Vegye figyelembe, hogyan jelenleg nem rendelkezett toospecify minden hello lehetséges dokumentum mező használata esetén `MergeOrUpload` , és hogyan azt csak meghatározott hello dokumentum kulcs (`HotelId`) használatakor `Delete`.</span><span class="sxs-lookup"><span data-stu-id="e14c7-162">Note how we did not have toospecify all hello possible document fields when using `MergeOrUpload` and how we only specified hello document key (`HotelId`) when using `Delete`.</span></span>

<span data-ttu-id="e14c7-163">Emellett vegye figyelembe, hogy csak egyetlen indexelési kérelem too1000 dokumentumainak fel is megadható.</span><span class="sxs-lookup"><span data-stu-id="e14c7-163">Also, note that you can only include up too1000 documents in a single indexing request.</span></span>

> [!NOTE]
> <span data-ttu-id="e14c7-164">Ebben a példában a különféle műveletek toodifferent dokumentumok alkalmazzák azt.</span><span class="sxs-lookup"><span data-stu-id="e14c7-164">In this example, we are applying different actions toodifferent documents.</span></span> <span data-ttu-id="e14c7-165">Ha tooperform hello kötegelt hívása helyett az összes dokumentum ugyanazokat a műveleteket hello `IndexBatch.New`, használhat más statikus módszereket hello `IndexBatch`.</span><span class="sxs-lookup"><span data-stu-id="e14c7-165">If you wanted tooperform hello same actions across all documents in hello batch, instead of calling `IndexBatch.New`, you could use hello other static methods of `IndexBatch`.</span></span> <span data-ttu-id="e14c7-166">Kötegeket például az `IndexBatch.Merge`, `IndexBatch.MergeOrUpload` vagy `IndexBatch.Delete` művelet meghívásával is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="e14c7-166">For example, you could create batches by calling `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, or `IndexBatch.Delete`.</span></span> <span data-ttu-id="e14c7-167">Ezen módszerek `IndexAction` objektumok helyett dokumentumok gyűjteményét (jelen példában a `Hotel` típusú objektumokat) használják.</span><span class="sxs-lookup"><span data-stu-id="e14c7-167">These methods take a collection of documents (objects of type `Hotel` in this example) instead of `IndexAction` objects.</span></span>
> 
> 

## <a name="import-data-toohello-index"></a><span data-ttu-id="e14c7-168">Importálás az toohello indexe</span><span class="sxs-lookup"><span data-stu-id="e14c7-168">Import data toohello index</span></span>
<span data-ttu-id="e14c7-169">Most, hogy már rendelkezik egy inicializált `IndexBatch` objektum, elküldheti azt toohello index meghívásával `Documents.Index` a a `SearchIndexClient` objektum.</span><span class="sxs-lookup"><span data-stu-id="e14c7-169">Now that you have an initialized `IndexBatch` object, you can send it toohello index by calling `Documents.Index` on your `SearchIndexClient` object.</span></span> <span data-ttu-id="e14c7-170">a következő példa azt mutatja meg hogyan hello toocall `Index`, valamint néhány további lépést tooperform szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="e14c7-170">hello following example shows how toocall `Index`, as well as some extra steps you will need tooperform:</span></span>

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

<span data-ttu-id="e14c7-171">Megjegyzés: hello `try` / `catch` hello hívás toohello körülvevő `Index` metódust.</span><span class="sxs-lookup"><span data-stu-id="e14c7-171">Note hello `try`/`catch` surrounding hello call toohello `Index` method.</span></span> <span data-ttu-id="e14c7-172">hello catch blokkon kezeli az egyik fontos hiba esetében az indexeléshez.</span><span class="sxs-lookup"><span data-stu-id="e14c7-172">hello catch block handles an important error case for indexing.</span></span> <span data-ttu-id="e14c7-173">Ha az Azure Search szolgáltatás sikertelen tooindex néhány hello hello kötegben, dokumentumok egy `IndexBatchException` által okozott `Documents.Index`.</span><span class="sxs-lookup"><span data-stu-id="e14c7-173">If your Azure Search service fails tooindex some of hello documents in hello batch, an `IndexBatchException` is thrown by `Documents.Index`.</span></span> <span data-ttu-id="e14c7-174">Ez akkor történhet meg, ha olyankor végzi a dokumentumok indexelését, amikor a szolgáltatás nagy terhelés alatt áll.</span><span class="sxs-lookup"><span data-stu-id="e14c7-174">This can happen if you are indexing documents while your service is under heavy load.</span></span> <span data-ttu-id="e14c7-175">**Javasoljuk, hogy a kódban explicit módon kezelje ezt az esetet.**</span><span class="sxs-lookup"><span data-stu-id="e14c7-175">**We strongly recommend explicitly handling this case in your code.**</span></span> <span data-ttu-id="e14c7-176">Késleltetés, és próbálkozzon újra a sikertelen indexelési hello dokumentumok, vagy jelentkezzen és például hello minta nem, vagy valami mást, attól függően, hogy az alkalmazás konzisztenciájának követelményeinek elvégezhető folytatásához.</span><span class="sxs-lookup"><span data-stu-id="e14c7-176">You can delay and then retry indexing hello documents that failed, or you can log and continue like hello sample does, or you can do something else depending on your application's data consistency requirements.</span></span>

<span data-ttu-id="e14c7-177">Végezetül hello kód hello példában két másodpercen késések fent.</span><span class="sxs-lookup"><span data-stu-id="e14c7-177">Finally, hello code in hello example above delays for two seconds.</span></span> <span data-ttu-id="e14c7-178">Indexelő történik aszinkron módon történik az Azure Search szolgáltatásban, így hello mintaalkalmazás kell toowait egy rövid időn belül tooensure, hogy a keresés hello dokumentumok is.</span><span class="sxs-lookup"><span data-stu-id="e14c7-178">Indexing happens asynchronously in your Azure Search service, so hello sample application needs toowait a short time tooensure that hello documents are available for searching.</span></span> <span data-ttu-id="e14c7-179">Ilyen mértékű késleltetésre kizárólag demók, tesztek és mintaalkalmazások esetében van szükség.</span><span class="sxs-lookup"><span data-stu-id="e14c7-179">Delays like this are typically only necessary in demos, tests, and sample applications.</span></span>

<a name="HotelClass"></a>

### <a name="how-hello-net-sdk-handles-documents"></a><span data-ttu-id="e14c7-180">Hogyan kezeli a .NET SDK hello a dokumentumok</span><span class="sxs-lookup"><span data-stu-id="e14c7-180">How hello .NET SDK handles documents</span></span>
<span data-ttu-id="e14c7-181">Előfordulhat, hogy lehet szeretné megtudni, hogy hogyan hello Azure Search .NET SDK például a felhasználói osztály példányainak képes tooupload `Hotel` toohello index.</span><span class="sxs-lookup"><span data-stu-id="e14c7-181">You may be wondering how hello Azure Search .NET SDK is able tooupload instances of a user-defined class like `Hotel` toohello index.</span></span> <span data-ttu-id="e14c7-182">toohelp válaszolja meg, hogy a kérdést, nézzük hello `Hotel` osztály, amely meghatározott toohello sémát indexeli [hello .NET SDK használatával Azure Search-index létrehozása](search-create-index-dotnet.md#DefineIndex):</span><span class="sxs-lookup"><span data-stu-id="e14c7-182">toohelp answer that question, let's look at hello `Hotel` class, which maps toohello index schema defined in [Create an Azure Search index using hello .NET SDK](search-create-index-dotnet.md#DefineIndex):</span></span>

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

<span data-ttu-id="e14c7-183">hello először thing toonotice, hogy minden egyes nyilvános tulajdonsága `Hotel` felel meg a hello Indexdefiníció, de egy fontos különbséggel tooa mező: hello minden mező kezdetű névvel rendelkező visszaalakítja ("teve eset"), közben minden nyilvános hello neve a tulajdonság `Hotel` egy nagybetűt ("Pascal eset") kezdődik.</span><span class="sxs-lookup"><span data-stu-id="e14c7-183">hello first thing toonotice is that each public property of `Hotel` corresponds tooa field in hello index definition, but with one crucial difference: hello name of each field starts with a lower-case letter ("camel case"), while hello name of each public property of `Hotel` starts with an upper-case letter ("Pascal case").</span></span> <span data-ttu-id="e14c7-184">Ez az a közös hajtható végre adatkötés, ahol hello cél séma az alkalmazásfejlesztő hello külső hello irányítását .NET-alkalmazások esetén.</span><span class="sxs-lookup"><span data-stu-id="e14c7-184">This is a common scenario in .NET applications that perform data-binding where hello target schema is outside hello control of hello application developer.</span></span> <span data-ttu-id="e14c7-185">Ahelyett, hogy a tooviolate hello .NET irányelvek elnevezési azáltal, hogy a tulajdonság nevét teve eset, megadható, hogy a hello SDK toomap hello tulajdonság nevének toocamel-kódaláírásokra automatikusan hello `[SerializePropertyNamesAsCamelCase]` attribútum.</span><span class="sxs-lookup"><span data-stu-id="e14c7-185">Rather than having tooviolate hello .NET naming guidelines by making property names camel-case, you can tell hello SDK toomap hello property names toocamel-case automatically with hello `[SerializePropertyNamesAsCamelCase]` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="e14c7-186">hello Azure Search .NET SDK-t használ a hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) könyvtár tooserialize és deszerializálható JSON formátumból az egyéni modell objektumok tooand.</span><span class="sxs-lookup"><span data-stu-id="e14c7-186">hello Azure Search .NET SDK uses hello [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) library tooserialize and deserialize your custom model objects tooand from JSON.</span></span> <span data-ttu-id="e14c7-187">A szerializálás szükség szerint testre szabható.</span><span class="sxs-lookup"><span data-stu-id="e14c7-187">You can customize this serialization if needed.</span></span> <span data-ttu-id="e14c7-188">További információk: [Egyéni szerializálás a JSON.NET használatával](search-howto-dotnet-sdk.md#JsonDotNet).</span><span class="sxs-lookup"><span data-stu-id="e14c7-188">You can find more details in [Custom Serialization with JSON.NET](search-howto-dotnet-sdk.md#JsonDotNet).</span></span> <span data-ttu-id="e14c7-189">Egy példa erre, hello hello használata `[JsonProperty]` hello attribútum `DescriptionFr` a fenti hello mintakód tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="e14c7-189">One example of this is hello use of hello `[JsonProperty]` attribute on hello `DescriptionFr` property in hello sample code above.</span></span>
> 
> 

<span data-ttu-id="e14c7-190">hello kapcsolatos fontos dolog második hello `Hotel` osztály hello adattípusok hello nyilvános tulajdonságainak.</span><span class="sxs-lookup"><span data-stu-id="e14c7-190">hello second important thing about hello `Hotel` class are hello data types of hello public properties.</span></span> <span data-ttu-id="e14c7-191">hello .NET ezeket a tulajdonságokat képezi le hello Indexdefiníció tootheir egyenértékű mező típusa.</span><span class="sxs-lookup"><span data-stu-id="e14c7-191">hello .NET types of these properties map tootheir equivalent field types in hello index definition.</span></span> <span data-ttu-id="e14c7-192">Például hello `Category` karakterlánc típusú tulajdonság leképezhető toohello `category` mező, amelynek a típusa `DataType.String`.</span><span class="sxs-lookup"><span data-stu-id="e14c7-192">For example, hello `Category` string property maps toohello `category` field, which is of type `DataType.String`.</span></span> <span data-ttu-id="e14c7-193">Hasonló típusleképezés történik a `bool?` és `DataType.Boolean`, illetve a `DateTimeOffset?` és `DataType.DateTimeOffset` stb. között is.</span><span class="sxs-lookup"><span data-stu-id="e14c7-193">There are similar type mappings between `bool?` and `DataType.Boolean`, `DateTimeOffset?` and `DataType.DateTimeOffset`, and so forth.</span></span> <span data-ttu-id="e14c7-194">hello leképezésének meghatározott szabályai hello szerepelnek a hello `Documents.Get` metódus a hello [Azure Search .NET SDK-referencia](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span><span class="sxs-lookup"><span data-stu-id="e14c7-194">hello specific rules for hello type mapping are documented with hello `Documents.Get` method in hello [Azure Search .NET SDK reference](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_).</span></span>

<span data-ttu-id="e14c7-195">Ez lehetőséget toouse saját osztályok-dokumentumokként működik mindkét irányban; Keresési eredmények beolvasásához és is rendelkezik hello SDK automatikusan deszerializálni őket tooa típus az Ön által választott, ahogy az hello [tovább cikk](search-query-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="e14c7-195">This ability toouse your own classes as documents works in both directions; You can also retrieve search results and have hello SDK automatically deserialize them tooa type of your choice, as shown in hello [next article](search-query-dotnet.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e14c7-196">hello Azure Search .NET SDK-t is támogatja a hello segítségével dinamikusan gépelt dokumentumok `Document` osztályt, amely nevek toofield mezőértékek kulcs/érték hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="e14c7-196">hello Azure Search .NET SDK also supports dynamically-typed documents using hello `Document` class, which is a key/value mapping of field names toofield values.</span></span> <span data-ttu-id="e14c7-197">Ez olyan esetekben hasznos, ha hello a sémát indexeli tervezéskor nem tudja, vagy ha kényelmetlen toobind toospecific modell osztályok lenne.</span><span class="sxs-lookup"><span data-stu-id="e14c7-197">This is useful in scenarios where you don't know hello index schema at design-time, or where it would be inconvenient toobind toospecific model classes.</span></span> <span data-ttu-id="e14c7-198">Minden hello módszerek hello SDK a dokumentumok kezelése rendelkezik, amelyek használhatók a hello túlterhelések `Document` osztály, valamint az erős típusmegadású túlterhelések, amelyek általános típusú paramétert.</span><span class="sxs-lookup"><span data-stu-id="e14c7-198">All hello methods in hello SDK that deal with documents have overloads that work with hello `Document` class, as well as strongly-typed overloads that take a generic type parameter.</span></span> <span data-ttu-id="e14c7-199">Ez utóbbi csak hello hello mintakód ebben a cikkben szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="e14c7-199">Only hello latter are used in hello sample code in this article.</span></span>
> 
> 

<span data-ttu-id="e14c7-200">**Miért használjon nullázható adattípusokat?**</span><span class="sxs-lookup"><span data-stu-id="e14c7-200">**Why you should use nullable data types**</span></span>

<span data-ttu-id="e14c7-201">A saját modell osztályok toomap tooan Azure Search-index tervezésekor ajánlott érték típusú tulajdonságok például deklaráló `bool` és `int` toobe nullázható (például `bool?` helyett `bool`).</span><span class="sxs-lookup"><span data-stu-id="e14c7-201">When designing your own model classes toomap tooan Azure Search index, we recommend declaring properties of value types such as `bool` and `int` toobe nullable (for example, `bool?` instead of `bool`).</span></span> <span data-ttu-id="e14c7-202">Ha egy nem nullázható tulajdonságot használja, akkor túl**garantálni** , hogy az indexben nem dokumentumok hello megfelelő mező null értéket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e14c7-202">If you use a non-nullable property, you have too**guarantee** that no documents in your index contain a null value for hello corresponding field.</span></span> <span data-ttu-id="e14c7-203">Hello SDK sem hello Azure Search szolgáltatás segítségével Ön tooenforce ez.</span><span class="sxs-lookup"><span data-stu-id="e14c7-203">Neither hello SDK nor hello Azure Search service will help you tooenforce this.</span></span>

<span data-ttu-id="e14c7-204">Ez nem csak egy elméleti szempont: képzelhető el, egy olyan forgatókönyvet, ahol hozzáadhat egy új tooan meglévő mezőindex, amely típusú `DataType.Int32`.</span><span class="sxs-lookup"><span data-stu-id="e14c7-204">This is not just a hypothetical concern: Imagine a scenario where you add a new field tooan existing index that is of type `DataType.Int32`.</span></span> <span data-ttu-id="e14c7-205">Hello index definíciójának frissítése után összes dokumentumot lesz az új mező null értéket (mivel az Azure Search nullázható minden esetében).</span><span class="sxs-lookup"><span data-stu-id="e14c7-205">After updating hello index definition, all documents will have a null value for that new field (since all types are nullable in Azure Search).</span></span> <span data-ttu-id="e14c7-206">Ha az egy nem nullázható majd használja egy modell osztály `int` tulajdonság, mező, elérhetővé válik egy `JsonSerializationException` ez, például tooretrieve dokumentumok tett kísérlet során:</span><span class="sxs-lookup"><span data-stu-id="e14c7-206">If you then use a model class with a non-nullable `int` property for that field, you will get a `JsonSerializationException` like this when trying tooretrieve documents:</span></span>

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

<span data-ttu-id="e14c7-207">Ezért javasoljuk, hogy a modellosztályokban nullázható értéktípusokat használjon.</span><span class="sxs-lookup"><span data-stu-id="e14c7-207">For this reason, we recommend that you use nullable types in your model classes as a best practice.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e14c7-208">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e14c7-208">Next steps</span></span>
<span data-ttu-id="e14c7-209">Után feltöltése az Azure Search-index, a lekérdezések toosearch dokumentumok kiállító készen toostart lesz.</span><span class="sxs-lookup"><span data-stu-id="e14c7-209">After populating your Azure Search index, you will be ready toostart issuing queries toosearch for documents.</span></span> <span data-ttu-id="e14c7-210">Részletes információk: [Az Azure Search-index lekérdezése](search-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e14c7-210">See [Query Your Azure Search Index](search-query-overview.md) for details.</span></span>

