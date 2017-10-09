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
# <a name="upgrading-toohello-azure-search-net-sdk-version-3"></a><span data-ttu-id="ecefe-103">Azure Search .NET SDK 3-as verziójú toohello frissítése</span><span class="sxs-lookup"><span data-stu-id="ecefe-103">Upgrading toohello Azure Search .NET SDK version 3</span></span>
<span data-ttu-id="ecefe-104">Ha szeretne megtudni a 2.0-preview vagy hello a régebbi verziója [Azure Search .NET SDK](https://aka.ms/search-sdk), ez a cikk segít frissíteni az alkalmazás toouse verziót 3.</span><span class="sxs-lookup"><span data-stu-id="ecefe-104">If you're using version 2.0-preview or older of hello [Azure Search .NET SDK](https://aka.ms/search-sdk), this article will help you upgrade your application toouse version 3.</span></span>

<span data-ttu-id="ecefe-105">Többek között a következőket példákért lásd: hello SDK általános bemutatóért [hogyan toouse Azure keresési .NET-alkalmazás](search-howto-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="ecefe-105">For a more general walkthrough of hello SDK including examples, see [How toouse Azure Search from a .NET Application](search-howto-dotnet-sdk.md).</span></span>

<span data-ttu-id="ecefe-106">Hello Azure Search .NET SDK 3-as verziója korábbi verzióihoz képest végrehajtott egyes módosításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ecefe-106">Version 3 of hello Azure Search .NET SDK contains some changes from earlier versions.</span></span> <span data-ttu-id="ecefe-107">Ezek a legtöbbször kisebb, így a kód módosítása elvégzéséhez szükséges csak csatlakozhatnak.</span><span class="sxs-lookup"><span data-stu-id="ecefe-107">These are mostly minor, so changing your code should require only minimal effort.</span></span> <span data-ttu-id="ecefe-108">Lásd: [lépéseket tooupgrade](#UpgradeSteps) útmutatást toochange a kód toouse hello új SDK-verzió.</span><span class="sxs-lookup"><span data-stu-id="ecefe-108">See [Steps tooupgrade](#UpgradeSteps) for instructions on how toochange your code toouse hello new SDK version.</span></span>

> [!NOTE]
> <span data-ttu-id="ecefe-109">Verzió 1.0.2-preview használata vagy régebbi, kell először frissítenie tooversion 1.1, és frissítse a tooversion 3.</span><span class="sxs-lookup"><span data-stu-id="ecefe-109">If you're using version 1.0.2-preview or older, you should upgrade tooversion 1.1 first, and then upgrade tooversion 3.</span></span> <span data-ttu-id="ecefe-110">Lásd: [függelék: lépéseket tooupgrade tooversion 1.1](#UpgradeStepsV1) utasításokat.</span><span class="sxs-lookup"><span data-stu-id="ecefe-110">See [Appendix: Steps tooupgrade tooversion 1.1](#UpgradeStepsV1) for instructions.</span></span>
>
> <span data-ttu-id="ecefe-111">Az Azure Search szolgáltatás példányát támogatja több REST API-t, többek között a legújabb hello.</span><span class="sxs-lookup"><span data-stu-id="ecefe-111">Your Azure Search service instance supports several REST API versions, including hello latest one.</span></span> <span data-ttu-id="ecefe-112">Egy verzió toouse tovább már nem hello legújabb, de azt javasoljuk, hogy áttelepítette-e a kód toouse hello legújabb verziója.</span><span class="sxs-lookup"><span data-stu-id="ecefe-112">You can continue toouse a version when it is no longer hello latest one, but we recommend that you migrate your code toouse hello newest version.</span></span> <span data-ttu-id="ecefe-113">Hello REST API használata esetén minden kérelemben hello api-version paraméter segítségével adjon meg a hello API-verzió.</span><span class="sxs-lookup"><span data-stu-id="ecefe-113">When using hello REST API, you must specify hello API version in every request via hello api-version parameter.</span></span> <span data-ttu-id="ecefe-114">Hello .NET SDK használata esetén a hello használata SDK hello verziója hello megfelelő hello REST API verziója határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ecefe-114">When using hello .NET SDK, hello version of hello SDK you’re using determines hello corresponding version of hello REST API.</span></span> <span data-ttu-id="ecefe-115">Ha egy régebbi SDK használ, folytathatja toorun ezt a kódot, módosítások nélküli akkor is, ha hello szolgáltatás frissített toosupport egy újabb API-verzió.</span><span class="sxs-lookup"><span data-stu-id="ecefe-115">If you are using an older SDK, you can continue toorun that code with no changes even if hello service is upgraded toosupport a newer API version.</span></span>

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a><span data-ttu-id="ecefe-116">3. verzió újdonságai</span><span class="sxs-lookup"><span data-stu-id="ecefe-116">What's new in version 3</span></span>
<span data-ttu-id="ecefe-117">Hello Azure Search .NET SDK célok hello legújabb 3-as verziója hello Azure Search REST API, kifejezetten 2016-09-01 általánosan elérhető verziójának.</span><span class="sxs-lookup"><span data-stu-id="ecefe-117">Version 3 of hello Azure Search .NET SDK targets hello latest generally available version of hello Azure Search REST API, specifically 2016-09-01.</span></span> <span data-ttu-id="ecefe-118">Ez teszi lehetővé toouse számos új szolgáltatást nyújt az Azure Search .NET-alkalmazás, beleértve a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="ecefe-118">This makes it possible toouse many new features of Azure Search from a .NET application, including hello following:</span></span>

* [<span data-ttu-id="ecefe-119">Egyéni elemzők</span><span class="sxs-lookup"><span data-stu-id="ecefe-119">Custom analyzers</span></span>](https://aka.ms/customanalyzers)
* <span data-ttu-id="ecefe-120">[Az Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) és [Azure Table Storage](search-howto-indexing-azure-tables.md) indexelő támogatása</span><span class="sxs-lookup"><span data-stu-id="ecefe-120">[Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) and [Azure Table Storage](search-howto-indexing-azure-tables.md) indexer support</span></span>
* <span data-ttu-id="ecefe-121">Az indexelő testreszabási keresztül [hozzárendelések mezőben](search-indexer-field-mappings.md)</span><span class="sxs-lookup"><span data-stu-id="ecefe-121">Indexer customization via [field mappings](search-indexer-field-mappings.md)</span></span>
* <span data-ttu-id="ecefe-122">ETag-EK támogatja tooenable biztonságos egyidejű index definíciók, az indexelők és az adatok források</span><span class="sxs-lookup"><span data-stu-id="ecefe-122">ETags support tooenable safe concurrent updating of index definitions, indexers, and data sources</span></span>
* <span data-ttu-id="ecefe-123">Index mező definíciók deklarációval létrehozásának dekoráció a modellosztállyal, és használja új hello támogatása `FieldBuilder` osztály.</span><span class="sxs-lookup"><span data-stu-id="ecefe-123">Support for building index field definitions declaratively by decorating your model class and using hello new `FieldBuilder` class.</span></span>
* <span data-ttu-id="ecefe-124">A .NET Core és a hordozható .NET-profil 111 támogatása</span><span class="sxs-lookup"><span data-stu-id="ecefe-124">Support for .NET Core and .NET Portable Profile 111</span></span>

<a name="UpgradeSteps"></a>

## <a name="steps-tooupgrade"></a><span data-ttu-id="ecefe-125">Lépéseket tooupgrade</span><span class="sxs-lookup"><span data-stu-id="ecefe-125">Steps tooupgrade</span></span>
<span data-ttu-id="ecefe-126">Először frissítse a NuGet referencia `Microsoft.Azure.Search` vagy hello NuGet-Csomagkezelő konzol használatával vagy az csomagot jobb gombbal a projekt hivatkozásait, majd válassza a "Kezelése NuGet csomagjainak..." a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="ecefe-126">First, update your NuGet reference for `Microsoft.Azure.Search` using either hello NuGet Package Manager Console or by right-clicking on your project references and selecting "Manage NuGet Packages..." in Visual Studio.</span></span>

<span data-ttu-id="ecefe-127">Miután NuGet töltött hello új csomagok és a függőségek, a projekt újraépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ecefe-127">Once NuGet has downloaded hello new packages and their dependencies, rebuild your project.</span></span> <span data-ttu-id="ecefe-128">Attól függően, hogy a kód hogyan épül akkor előfordulhat, hogy újraépítése sikeresen.</span><span class="sxs-lookup"><span data-stu-id="ecefe-128">Depending on how your code is structured, it may rebuild successfully.</span></span> <span data-ttu-id="ecefe-129">Ha igen, most készen áll a toogo!</span><span class="sxs-lookup"><span data-stu-id="ecefe-129">If so, you're ready toogo!</span></span>

<span data-ttu-id="ecefe-130">Ha a build nem sikerül, egy összeállítási hibát hasonló hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="ecefe-130">If your build fails, you should see a build error like hello following:</span></span>

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' too'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

<span data-ttu-id="ecefe-131">következő lépés hello toofix a build hiba van.</span><span class="sxs-lookup"><span data-stu-id="ecefe-131">hello next step is toofix this build error.</span></span> <span data-ttu-id="ecefe-132">Lásd: [módosítások Megtörje a 3-as verziójú](#ListOfChanges) talál részletes információt mi hello hiba okozza, és hogyan toofix azt.</span><span class="sxs-lookup"><span data-stu-id="ecefe-132">See [Breaking changes in version 3](#ListOfChanges) for details on what causes hello error and how toofix it.</span></span>

<span data-ttu-id="ecefe-133">Láthatja, hogy további build figyelmeztetések kapcsolódó tooobsolete metódusokra vagy tulajdonságokra.</span><span class="sxs-lookup"><span data-stu-id="ecefe-133">You may see additional build warnings related tooobsolete methods or properties.</span></span> <span data-ttu-id="ecefe-134">hello figyelmeztetések milyen toouse inkább a hello elavult funkció utasításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ecefe-134">hello warnings will include instructions on what toouse instead of hello deprecated feature.</span></span> <span data-ttu-id="ecefe-135">Például, ha az alkalmazás hello `IndexingParameters.Base64EncodeKeys` tulajdonság, kell megjelenik egy figyelmeztetés, amely szerint`"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`</span><span class="sxs-lookup"><span data-stu-id="ecefe-135">For example, if your application uses hello `IndexingParameters.Base64EncodeKeys` property, you should get a warning that says `"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`</span></span>

<span data-ttu-id="ecefe-136">Build hibák megszüntetése után módosíthatja tooyour alkalmazás tootake új funkciók előnyeit Ha.</span><span class="sxs-lookup"><span data-stu-id="ecefe-136">Once you've fixed any build errors, you can make changes tooyour application tootake advantage of new functionality if you wish.</span></span> <span data-ttu-id="ecefe-137">Az hello SDK új funkciói részletes leírást talál [What's new in 3-as verziójú](#WhatsNew).</span><span class="sxs-lookup"><span data-stu-id="ecefe-137">New features in hello SDK are detailed in [What's new in version 3](#WhatsNew).</span></span>

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a><span data-ttu-id="ecefe-138">3-as verziójú megtörje változásai</span><span class="sxs-lookup"><span data-stu-id="ecefe-138">Breaking changes in version 3</span></span>
<span data-ttu-id="ecefe-139">Van néhány jelentős változásokat a 3-as verzió, amelyre szüksége lehet a kód vált továbbá toorebuilding az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ecefe-139">There a small number of breaking changes in version 3 that may require code changes in addition toorebuilding your application.</span></span>

### <a name="indexesgetclient-return-type"></a><span data-ttu-id="ecefe-140">Indexes.GetClient visszatérési típusa</span><span class="sxs-lookup"><span data-stu-id="ecefe-140">Indexes.GetClient return type</span></span>
<span data-ttu-id="ecefe-141">Hello `Indexes.GetClient` módszer új visszatérési értékkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ecefe-141">hello `Indexes.GetClient` method has a new return type.</span></span> <span data-ttu-id="ecefe-142">Korábban, visszaadott `SearchIndexClient`, azonban ez megváltozott túl`ISearchIndexClient` 2.0-előzetes verzióját, és hogy módosítás tooversion 3 keresztül végzi.</span><span class="sxs-lookup"><span data-stu-id="ecefe-142">Previously, it returned `SearchIndexClient`, but this was changed too`ISearchIndexClient` in version 2.0-preview, and that change carries over tooversion 3.</span></span> <span data-ttu-id="ecefe-143">Ez az toomock hello kívánja toosupport ügyfelek `GetClient` egység tesztek utánzatait végrehajtásának visszaadó metódus `ISearchIndexClient`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-143">This is toosupport customers that wish toomock hello `GetClient` method for unit tests by returning a mock implementation of `ISearchIndexClient`.</span></span>

#### <a name="example"></a><span data-ttu-id="ecefe-144">Példa</span><span class="sxs-lookup"><span data-stu-id="ecefe-144">Example</span></span>
<span data-ttu-id="ecefe-145">Ha a kód így néz ki:</span><span class="sxs-lookup"><span data-stu-id="ecefe-145">If your code looks like this:</span></span>

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

<span data-ttu-id="ecefe-146">Ez módosítható toothis toofix esetleges hibák létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ecefe-146">You can change it toothis toofix any build errors:</span></span>

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-toostrings"></a><span data-ttu-id="ecefe-147">AnalyzerName, adattípus és mások már nem konvertálható erre toostrings</span><span class="sxs-lookup"><span data-stu-id="ecefe-147">AnalyzerName, DataType, and others are no longer implicitly convertible toostrings</span></span>
<span data-ttu-id="ecefe-148">Az Azure Search .NET SDK, amelyek a hello számos különböző `ExtensibleEnum`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-148">There are many types in hello Azure Search .NET SDK that derive from `ExtensibleEnum`.</span></span> <span data-ttu-id="ecefe-149">Korábban ezek a típusok volt konvertálható összes erre tootype `string`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-149">Previously these types were all implicitly convertible tootype `string`.</span></span> <span data-ttu-id="ecefe-150">Azonban programhiba hello felfedezett `Object.Equals` ezen osztályok és rögzítési hello hiba szükséges letiltása a implicit konverzió implementációja.</span><span class="sxs-lookup"><span data-stu-id="ecefe-150">However, a bug was discovered in hello `Object.Equals` implementation for these classes, and fixing hello bug required disabling this implicit conversion.</span></span> <span data-ttu-id="ecefe-151">Explicit konverzió túl`string` rendszer nem tagadja meg.</span><span class="sxs-lookup"><span data-stu-id="ecefe-151">Explicit conversion too`string` is still allowed.</span></span>

#### <a name="example"></a><span data-ttu-id="ecefe-152">Példa</span><span class="sxs-lookup"><span data-stu-id="ecefe-152">Example</span></span>
<span data-ttu-id="ecefe-153">Ha a kód így néz ki:</span><span class="sxs-lookup"><span data-stu-id="ecefe-153">If your code looks like this:</span></span>

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

<span data-ttu-id="ecefe-154">Ez módosítható toothis toofix esetleges hibák létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ecefe-154">You can change it toothis toofix any build errors:</span></span>

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

### <a name="removed-obsolete-members"></a><span data-ttu-id="ecefe-155">Eltávolított elavult tagok</span><span class="sxs-lookup"><span data-stu-id="ecefe-155">Removed obsolete members</span></span>

<span data-ttu-id="ecefe-156">Build hibák kapcsolódó toomethods vagy volt megjelölve, a 2.0-előzetes verziója elavult, és később eltávolítja a 3-as verziójú tulajdonságokhoz jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="ecefe-156">You may see build errors related toomethods or properties that were marked as obsolete in version 2.0-preview and subsequently removed in version 3.</span></span> <span data-ttu-id="ecefe-157">Ha ilyen hibákat észlel, hogyan tooresolve őket:</span><span class="sxs-lookup"><span data-stu-id="ecefe-157">If you encounter such errors, here is how tooresolve them:</span></span>

- <span data-ttu-id="ecefe-158">Ha ez a konstruktor: `ScoringParameter(string name, string value)`, használja helyette a erre:`ScoringParameter(string name, IEnumerable<string> values)`</span><span class="sxs-lookup"><span data-stu-id="ecefe-158">If you were using this constructor: `ScoringParameter(string name, string value)`, use this one instead: `ScoringParameter(string name, IEnumerable<string> values)`</span></span>
- <span data-ttu-id="ecefe-159">Ha használta hello `ScoringParameter.Value` tulajdonság, használjon hello `ScoringParameter.Values` tulajdonság vagy hello `ToString` metódus helyett.</span><span class="sxs-lookup"><span data-stu-id="ecefe-159">If you were using hello `ScoringParameter.Value` property, use hello `ScoringParameter.Values` property or hello `ToString` method instead.</span></span>
- <span data-ttu-id="ecefe-160">Ha használta hello `SearchRequestOptions.RequestId` tulajdonság, használjon hello `ClientRequestId` tulajdonság helyette.</span><span class="sxs-lookup"><span data-stu-id="ecefe-160">If you were using hello `SearchRequestOptions.RequestId` property, use hello `ClientRequestId` property instead.</span></span>

### <a name="removed-preview-features"></a><span data-ttu-id="ecefe-161">Az eltávolított előzetes verziójú funkciók</span><span class="sxs-lookup"><span data-stu-id="ecefe-161">Removed preview features</span></span>

<span data-ttu-id="ecefe-162">Ha verzió 2.0 előzetes verzió tooversion 3 frissít, ne feledje, hogy JSON és a fürt megosztott kötetei szolgáltatás támogatja a Blob indexelők elemzése el lett távolítva, mert ezek a funkciók még még csak előzetes verziójúak.</span><span class="sxs-lookup"><span data-stu-id="ecefe-162">If you are upgrading from version 2.0-preview tooversion 3, be aware that JSON and CSV parsing support for Blob Indexers has been removed since these features are still in preview.</span></span> <span data-ttu-id="ecefe-163">Pontosabban, a következő hello módszerek hello `IndexingParametersExtensions` osztály eltávolításra került:</span><span class="sxs-lookup"><span data-stu-id="ecefe-163">Specifically, hello following methods of hello `IndexingParametersExtensions` class have been removed:</span></span>

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

<span data-ttu-id="ecefe-164">Ha az alkalmazás erős függőség az ezen szolgáltatásoktól, csak akkor tudja tooupgrade tooversion 3 az Azure Search .NET SDK hello.</span><span class="sxs-lookup"><span data-stu-id="ecefe-164">If your application has a hard dependency on these features, you will not be able tooupgrade tooversion 3 of hello Azure Search .NET SDK.</span></span> <span data-ttu-id="ecefe-165">Folytatás toouse verzió 2.0 előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="ecefe-165">You can continue toouse version 2.0-preview.</span></span> <span data-ttu-id="ecefe-166">Azonban adja a következőket kell figyelembe venni, hogy **megtekintés SDK-k éles környezetben nem javasoljuk**.</span><span class="sxs-lookup"><span data-stu-id="ecefe-166">However, please keep in mind that **we do not recommend using preview SDKs in production applications**.</span></span> <span data-ttu-id="ecefe-167">Előzetes verziójú funkciók csak tesztelési és módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="ecefe-167">Preview features are for evaluation only and may change.</span></span>

## <a name="conclusion"></a><span data-ttu-id="ecefe-168">Összegzés</span><span class="sxs-lookup"><span data-stu-id="ecefe-168">Conclusion</span></span>
<span data-ttu-id="ecefe-169">Ha további részleteket a hello Azure Search .NET SDK használatával van szüksége, tekintse meg a közelmúltban frissített [útmutató](search-howto-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="ecefe-169">If you need more details on using hello Azure Search .NET SDK, see our recently updated [How-to](search-howto-dotnet-sdk.md).</span></span>

<span data-ttu-id="ecefe-170">Az hello SDK szívesen fogadjuk a visszajelzéseket.</span><span class="sxs-lookup"><span data-stu-id="ecefe-170">We welcome your feedback on hello SDK.</span></span> <span data-ttu-id="ecefe-171">Ha problémákat tapasztal, érzi, hogy szabad tooask nekünk a Súgó a hello [Azure Search MSDN fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch).</span><span class="sxs-lookup"><span data-stu-id="ecefe-171">If you encounter problems, feel free tooask us for help on hello [Azure Search MSDN forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch).</span></span> <span data-ttu-id="ecefe-172">Ha a hiba, akkor is fájlt problémát hello [Azure .NET SDK GitHub-tárházban](https://github.com/Azure/azure-sdk-for-net/issues).</span><span class="sxs-lookup"><span data-stu-id="ecefe-172">If you find a bug, you can file an issue in hello [Azure .NET SDK GitHub repository](https://github.com/Azure/azure-sdk-for-net/issues).</span></span> <span data-ttu-id="ecefe-173">Győződjön meg arról, hogy tooprefix a probléma címének és a "Search SDK:".</span><span class="sxs-lookup"><span data-stu-id="ecefe-173">Make sure tooprefix your issue title with "Search SDK: ".</span></span>

<span data-ttu-id="ecefe-174">Köszönjük, hogy az Azure Search használatával!</span><span class="sxs-lookup"><span data-stu-id="ecefe-174">Thank you for using Azure Search!</span></span>

<a name="UpgradeStepsV1"></a>

## <a name="appendix-steps-tooupgrade-tooversion-11"></a><span data-ttu-id="ecefe-175">A függelék: Lépéseket tooupgrade tooversion 1.1</span><span class="sxs-lookup"><span data-stu-id="ecefe-175">Appendix: Steps tooupgrade tooversion 1.1</span></span>
> [!NOTE]
> <span data-ttu-id="ecefe-176">Ez a szakasz csak a hello Azure Search .NET SDK-verzió 1.0.2-preview és régebbi toousers vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="ecefe-176">This section applies only toousers of hello Azure Search .NET SDK version 1.0.2-preview and older.</span></span>
> 
> 

<span data-ttu-id="ecefe-177">Először frissítse a NuGet referencia `Microsoft.Azure.Search` vagy hello NuGet-Csomagkezelő konzol használatával vagy az csomagot jobb gombbal a projekt hivatkozásait, majd válassza a "Kezelése NuGet csomagjainak..." a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="ecefe-177">First, update your NuGet reference for `Microsoft.Azure.Search` using either hello NuGet Package Manager Console or by right-clicking on your project references and selecting "Manage NuGet Packages..." in Visual Studio.</span></span>

<span data-ttu-id="ecefe-178">Miután NuGet töltött hello új csomagok és a függőségek, a projekt újraépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ecefe-178">Once NuGet has downloaded hello new packages and their dependencies, rebuild your project.</span></span>

<span data-ttu-id="ecefe-179">Ha korábban a verzió 1.0.0-preview, 1.0.1-preview vagy 1.0.2-preview, hello build sikeres legyen, és készen áll a toogo!</span><span class="sxs-lookup"><span data-stu-id="ecefe-179">If you were previously using version 1.0.0-preview, 1.0.1-preview, or 1.0.2-preview, hello build should succeed and you're ready toogo!</span></span>

<span data-ttu-id="ecefe-180">Ha korábban a verzió 0.13.0-preview vagy régebbi, megtekintheti az build hello következő hibákat:</span><span class="sxs-lookup"><span data-stu-id="ecefe-180">If you were previously using version 0.13.0-preview or older, you should see build errors like hello following:</span></span>

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: hello type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

<span data-ttu-id="ecefe-181">következő lépés hello toofix hello fordítási hibákat egyenként.</span><span class="sxs-lookup"><span data-stu-id="ecefe-181">hello next step is toofix hello build errors one by one.</span></span> <span data-ttu-id="ecefe-182">A legtöbb módosítás, néhány osztály- és metódus nevek hello SDK átnevezett van szükség.</span><span class="sxs-lookup"><span data-stu-id="ecefe-182">Most will require changing some class and method names that have been renamed in hello SDK.</span></span> <span data-ttu-id="ecefe-183">[Módosítások megtörje 1.1-es verzió listája](#ListOfChangesV1) neve módosítások listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ecefe-183">[List of breaking changes in version 1.1](#ListOfChangesV1) contains a list of these name changes.</span></span>

<span data-ttu-id="ecefe-184">Ha használata egyéni osztályok toomodel dokumentumait, és azon osztályok nem nullázható egyszerű típusú tulajdonságok (például `int` vagy `bool` C#), nincs hibajavítás hello SDK, amelynek tudnia kell hello 1.1-es verzióját.</span><span class="sxs-lookup"><span data-stu-id="ecefe-184">If you're using custom classes toomodel your documents, and those classes have properties of non-nullable primitive types (for example, `int` or `bool` in C#), there is a bug fix in hello 1.1 version of hello SDK of which you should be aware.</span></span> <span data-ttu-id="ecefe-185">Lásd: [hibajavítások 1.1-es verzió](#BugFixesV1) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="ecefe-185">See [Bug fixes in version 1.1](#BugFixesV1) for more details.</span></span>

<span data-ttu-id="ecefe-186">Végezetül build hibák megszüntetése után módosíthatja tooyour alkalmazás tootake új funkciók előnyeit Ha.</span><span class="sxs-lookup"><span data-stu-id="ecefe-186">Finally, once you've fixed any build errors, you can make changes tooyour application tootake advantage of new functionality if you wish.</span></span>

<a name="ListOfChangesV1"></a>

### <a name="list-of-breaking-changes-in-version-11"></a><span data-ttu-id="ecefe-187">Módosítások megtörje 1.1-es verzió listája</span><span class="sxs-lookup"><span data-stu-id="ecefe-187">List of breaking changes in version 1.1</span></span>
<span data-ttu-id="ecefe-188">hello alábbi van rendezve hello valószínűsége, hogy hello érinteni fogja az alkalmazás kódjában.</span><span class="sxs-lookup"><span data-stu-id="ecefe-188">hello following list is ordered by hello likelihood that hello change will affect your application code.</span></span>

#### <a name="indexbatch-and-indexaction-changes"></a><span data-ttu-id="ecefe-189">IndexBatch és IndexAction módosítása</span><span class="sxs-lookup"><span data-stu-id="ecefe-189">IndexBatch and IndexAction changes</span></span>
<span data-ttu-id="ecefe-190">`IndexBatch.Create`túl át lett nevezve`IndexBatch.New` és már nem rendelkezik egy `params` argumentum.</span><span class="sxs-lookup"><span data-stu-id="ecefe-190">`IndexBatch.Create` has been renamed too`IndexBatch.New` and no longer has a `params` argument.</span></span> <span data-ttu-id="ecefe-191">Használhat `IndexBatch.New` kötegek, amely különböző típusú műveletek (összevonása, törlés stb.) kombinálhatók.</span><span class="sxs-lookup"><span data-stu-id="ecefe-191">You can use `IndexBatch.New` for batches that mix different types of actions (merges, deletes, etc.).</span></span> <span data-ttu-id="ecefe-192">Emellett nincsenek új statikus módszereket biztosít a kötegek ahol minden hello műveletek vannak hello ugyanaz: `Delete`, `Merge`, `MergeOrUpload`, és `Upload`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-192">In addition, there are new static methods for creating batches where all hello actions are hello same: `Delete`, `Merge`, `MergeOrUpload`, and `Upload`.</span></span>

<span data-ttu-id="ecefe-193">`IndexAction`már nem rendelkezik nyilvános konstruktorral, és most nem módosíthatók a tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="ecefe-193">`IndexAction` no longer has public constructors and its properties are now immutable.</span></span> <span data-ttu-id="ecefe-194">Hello új statikus metódusok többféle célra műveletek létrehozásához kell használnia: `Delete`, `Merge`, `MergeOrUpload`, és `Upload`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-194">You should use hello new static methods for creating actions for different purposes: `Delete`, `Merge`, `MergeOrUpload`, and `Upload`.</span></span> <span data-ttu-id="ecefe-195">`IndexAction.Create`el lett távolítva.</span><span class="sxs-lookup"><span data-stu-id="ecefe-195">`IndexAction.Create` has been removed.</span></span> <span data-ttu-id="ecefe-196">Ha hello rendelkező túlterhelést csak egy dokumentumot, győződjön meg arról, hogy toouse `Upload` helyette.</span><span class="sxs-lookup"><span data-stu-id="ecefe-196">If you used hello overload that takes only a document, make sure toouse `Upload` instead.</span></span>

##### <a name="example"></a><span data-ttu-id="ecefe-197">Példa</span><span class="sxs-lookup"><span data-stu-id="ecefe-197">Example</span></span>
<span data-ttu-id="ecefe-198">Ha a kód így néz ki:</span><span class="sxs-lookup"><span data-stu-id="ecefe-198">If your code looks like this:</span></span>

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

<span data-ttu-id="ecefe-199">Ez módosítható toothis toofix esetleges hibák létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ecefe-199">You can change it toothis toofix any build errors:</span></span>

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

<span data-ttu-id="ecefe-200">Ha azt szeretné, hogy tovább egyszerűsítheti az toothis:</span><span class="sxs-lookup"><span data-stu-id="ecefe-200">If you want, you can further simplify it toothis:</span></span>

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a><span data-ttu-id="ecefe-201">IndexBatchException módosítások</span><span class="sxs-lookup"><span data-stu-id="ecefe-201">IndexBatchException changes</span></span>
<span data-ttu-id="ecefe-202">Hello `IndexBatchException.IndexResponse` tulajdonság túl át lett nevezve`IndexingResults`, és a típus már `IList<IndexingResult>`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-202">hello `IndexBatchException.IndexResponse` property has been renamed too`IndexingResults`, and its type is now `IList<IndexingResult>`.</span></span>

##### <a name="example"></a><span data-ttu-id="ecefe-203">Példa</span><span class="sxs-lookup"><span data-stu-id="ecefe-203">Example</span></span>
<span data-ttu-id="ecefe-204">Ha a kód így néz ki:</span><span class="sxs-lookup"><span data-stu-id="ecefe-204">If your code looks like this:</span></span>

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed tooindex some of hello documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<span data-ttu-id="ecefe-205">Ez módosítható toothis toofix esetleges hibák létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ecefe-205">You can change it toothis toofix any build errors:</span></span>

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed tooindex some of hello documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

#### <a name="operation-method-changes"></a><span data-ttu-id="ecefe-206">A művelet metódus változások</span><span class="sxs-lookup"><span data-stu-id="ecefe-206">Operation method changes</span></span>
<span data-ttu-id="ecefe-207">Hello Azure Search .NET SDK minden műveletet a szinkron és aszinkron hívóknak metódus túlterhelések készletként van közzétéve.</span><span class="sxs-lookup"><span data-stu-id="ecefe-207">Each operation in hello Azure Search .NET SDK is exposed as a set of method overloads for synchronous and asynchronous callers.</span></span> <span data-ttu-id="ecefe-208">hello aláírások és az e metódus túlterhelések faktoring módosítva 1.1-es verzió.</span><span class="sxs-lookup"><span data-stu-id="ecefe-208">hello signatures and factoring of these method overloads has changed in version 1.1.</span></span>

<span data-ttu-id="ecefe-209">Például hello "Index statisztika Get" művelet hello SDK korábbi verzióiban érhető el ezen aláírások:</span><span class="sxs-lookup"><span data-stu-id="ecefe-209">For example, hello "Get Index Statistics" operation in older versions of hello SDK exposed these signatures:</span></span>

<span data-ttu-id="ecefe-210">A `IIndexOperations`:</span><span class="sxs-lookup"><span data-stu-id="ecefe-210">In `IIndexOperations`:</span></span>

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

<span data-ttu-id="ecefe-211">A `IndexOperationsExtensions`:</span><span class="sxs-lookup"><span data-stu-id="ecefe-211">In `IndexOperationsExtensions`:</span></span>

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

<span data-ttu-id="ecefe-212">hello metódus-aláírása az 1.1-es verzió hely ilyen műveletet hello:</span><span class="sxs-lookup"><span data-stu-id="ecefe-212">hello method signatures for hello same operation in version 1.1 look like this:</span></span>

<span data-ttu-id="ecefe-213">A `IIndexesOperations`:</span><span class="sxs-lookup"><span data-stu-id="ecefe-213">In `IIndexesOperations`:</span></span>

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

<span data-ttu-id="ecefe-214">A `IndexesOperationsExtensions`:</span><span class="sxs-lookup"><span data-stu-id="ecefe-214">In `IndexesOperationsExtensions`:</span></span>

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

<span data-ttu-id="ecefe-215">1.1-es verziójától kezdve, hello Azure Search .NET SDK rendszerezi művelet módszerek másképp:</span><span class="sxs-lookup"><span data-stu-id="ecefe-215">Starting with version 1.1, hello Azure Search .NET SDK organizes operation methods differently:</span></span>

* <span data-ttu-id="ecefe-216">Választható paraméterek: most már van modellezve, ahelyett, hogy a paraméterek alapértelmezett további metódus túlterhelések-nál.</span><span class="sxs-lookup"><span data-stu-id="ecefe-216">Optional parameters are now modeled as default parameters rather than additional method overloads.</span></span> <span data-ttu-id="ecefe-217">Ez egyes esetekben jelentős mértékben csökkenti a metódus túlterhelések hello száma.</span><span class="sxs-lookup"><span data-stu-id="ecefe-217">This reduces hello number of method overloads, sometimes dramatically.</span></span>
* <span data-ttu-id="ecefe-218">hello kiterjesztésmetódusok most hello idegen részleteit HTTP hello hívó számos elrejtése.</span><span class="sxs-lookup"><span data-stu-id="ecefe-218">hello extension methods now hide a lot of hello extraneous details of HTTP from hello caller.</span></span> <span data-ttu-id="ecefe-219">Például SDK visszaadott HTTP-állapotkódot, melyet gyakran egy válasz objektum hello régebbi verziói nem kell toocheck mivel művelet módszerek throw `CloudException` bármely status kód, amely jelzi a hiba.</span><span class="sxs-lookup"><span data-stu-id="ecefe-219">For example, older versions of hello SDK returned a response object with an HTTP status code, which you often didn't need toocheck because operation methods throw `CloudException` for any status code that indicates an error.</span></span> <span data-ttu-id="ecefe-220">Új bővítmény metódusok csak visszatérési modellobjektumokat hello, mentését, hogy nem kell tekintettel toounwrap hello őket a kódban.</span><span class="sxs-lookup"><span data-stu-id="ecefe-220">hello new extension methods just return model objects, saving you hello trouble of having toounwrap them in your code.</span></span>
* <span data-ttu-id="ecefe-221">Ezzel ellentétben hello core felületek most teszi közzé módszereket, amelyek jobban irányítható hello HTTP szinten ha esetleg szükség lenne rá.</span><span class="sxs-lookup"><span data-stu-id="ecefe-221">Conversely, hello core interfaces now expose methods that give you more control at hello HTTP level if you need it.</span></span> <span data-ttu-id="ecefe-222">A HTTP-fejlécek egyéni toobe a kéréseket, és új hello most átadhatók `AzureOperationResponse<T>` visszatérési típus közvetlen hozzáférést toohello lehetővé teszi `HttpRequestMessage` és `HttpResponseMessage` hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="ecefe-222">You can now pass in custom HTTP headers toobe included in requests, and hello new `AzureOperationResponse<T>` return type gives you direct access toohello `HttpRequestMessage` and `HttpResponseMessage` for hello operation.</span></span> <span data-ttu-id="ecefe-223">`AzureOperationResponse`hello meghatározott `Microsoft.Rest.Azure` névtér és a felváltja `Hyak.Common.OperationResponse`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-223">`AzureOperationResponse` is defined in hello `Microsoft.Rest.Azure` namespace and replaces `Hyak.Common.OperationResponse`.</span></span>

#### <a name="scoringparameters-changes"></a><span data-ttu-id="ecefe-224">ScoringParameters módosítások</span><span class="sxs-lookup"><span data-stu-id="ecefe-224">ScoringParameters changes</span></span>
<span data-ttu-id="ecefe-225">Egy új osztályt `ScoringParameter` már hozzáadta a hello legújabb SDK toomake keresési lekérdezés könnyebb tooprovide paraméterek tooscoring profilok.</span><span class="sxs-lookup"><span data-stu-id="ecefe-225">A new class named `ScoringParameter` has been added in hello latest SDK toomake it easier tooprovide parameters tooscoring profiles in a search query.</span></span> <span data-ttu-id="ecefe-226">Korábban hello `ScoringProfiles` hello tulajdonságának `SearchParameters` osztály volt megadva, `IList<string>`; Most írta-e be `IList<ScoringParameter>`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-226">Previously hello `ScoringProfiles` property of hello `SearchParameters` class was typed as `IList<string>`; Now it is typed as `IList<ScoringParameter>`.</span></span>

##### <a name="example"></a><span data-ttu-id="ecefe-227">Példa</span><span class="sxs-lookup"><span data-stu-id="ecefe-227">Example</span></span>
<span data-ttu-id="ecefe-228">Ha a kód így néz ki:</span><span class="sxs-lookup"><span data-stu-id="ecefe-228">If your code looks like this:</span></span>

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

<span data-ttu-id="ecefe-229">Ez módosítható toothis toofix esetleges hibák létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ecefe-229">You can change it toothis toofix any build errors:</span></span> 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a><span data-ttu-id="ecefe-230">Modellmódosításokkal osztály</span><span class="sxs-lookup"><span data-stu-id="ecefe-230">Model class changes</span></span>
<span data-ttu-id="ecefe-231">Ismertetett toohello aláírás módosítások miatt [művelet metódus módosítások](#OperationMethodChanges), sok osztály a hello `Microsoft.Azure.Search.Models` névtér átnevezték vagy eltávolítani.</span><span class="sxs-lookup"><span data-stu-id="ecefe-231">Due toohello signature changes described in [Operation method changes](#OperationMethodChanges), many classes in hello `Microsoft.Azure.Search.Models` namespace have been renamed or removed.</span></span> <span data-ttu-id="ecefe-232">Példa:</span><span class="sxs-lookup"><span data-stu-id="ecefe-232">For example:</span></span>

* <span data-ttu-id="ecefe-233">`IndexDefinitionResponse`lett cserélve`AzureOperationResponse<Index>`</span><span class="sxs-lookup"><span data-stu-id="ecefe-233">`IndexDefinitionResponse` has been replaced by `AzureOperationResponse<Index>`</span></span>
* <span data-ttu-id="ecefe-234">`DocumentSearchResponse`túl át lett nevezve`DocumentSearchResult`</span><span class="sxs-lookup"><span data-stu-id="ecefe-234">`DocumentSearchResponse` has been renamed too`DocumentSearchResult`</span></span>
* <span data-ttu-id="ecefe-235">`IndexResult`túl át lett nevezve`IndexingResult`</span><span class="sxs-lookup"><span data-stu-id="ecefe-235">`IndexResult` has been renamed too`IndexingResult`</span></span>
* <span data-ttu-id="ecefe-236">`Documents.Count()`most adja vissza egy `long` hello dokumentum számával, hanem egy`DocumentCountResponse`</span><span class="sxs-lookup"><span data-stu-id="ecefe-236">`Documents.Count()` now returns a `long` with hello document count instead of a `DocumentCountResponse`</span></span>
* <span data-ttu-id="ecefe-237">`IndexGetStatisticsResponse`túl át lett nevezve`IndexGetStatisticsResult`</span><span class="sxs-lookup"><span data-stu-id="ecefe-237">`IndexGetStatisticsResponse` has been renamed too`IndexGetStatisticsResult`</span></span>
* <span data-ttu-id="ecefe-238">`IndexListResponse`túl át lett nevezve`IndexListResult`</span><span class="sxs-lookup"><span data-stu-id="ecefe-238">`IndexListResponse` has been renamed too`IndexListResult`</span></span>

<span data-ttu-id="ecefe-239">toosummarize, `OperationResponse`-származtatott osztályok, hogy csak egy modell objektum el lettek távolítva toowrap.</span><span class="sxs-lookup"><span data-stu-id="ecefe-239">toosummarize, `OperationResponse`-derived classes that existed only toowrap a model object have been removed.</span></span> <span data-ttu-id="ecefe-240">hello fennmaradó osztályok rendelkezésére állt-e a változása utótag `Response` túl`Result`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-240">hello remaining classes have had their suffix changed from `Response` too`Result`.</span></span>

##### <a name="example"></a><span data-ttu-id="ecefe-241">Példa</span><span class="sxs-lookup"><span data-stu-id="ecefe-241">Example</span></span>
<span data-ttu-id="ecefe-242">Ha a kód így néz ki:</span><span class="sxs-lookup"><span data-stu-id="ecefe-242">If your code looks like this:</span></span>

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

<span data-ttu-id="ecefe-243">Ez módosítható toothis toofix esetleges hibák létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ecefe-243">You can change it toothis toofix any build errors:</span></span>

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

##### <a name="response-classes-and-ienumerable"></a><span data-ttu-id="ecefe-244">Válasz osztályok és IEnumerable</span><span class="sxs-lookup"><span data-stu-id="ecefe-244">Response classes and IEnumerable</span></span>
<span data-ttu-id="ecefe-245">Egy további, a kód érintő módosítást a válasz osztályok gyűjtemények vonatkozik, amelyek már nem valósítja meg `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-245">An additional change that may affect your code is that response classes that hold collections no longer implement `IEnumerable<T>`.</span></span> <span data-ttu-id="ecefe-246">Ehelyett hello gyűjteménytulajdonság közvetlenül is elérheti.</span><span class="sxs-lookup"><span data-stu-id="ecefe-246">Instead, you can access hello collection property directly.</span></span> <span data-ttu-id="ecefe-247">Ha például a kód néz ki:</span><span class="sxs-lookup"><span data-stu-id="ecefe-247">For example, if your code looks like this:</span></span>

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

<span data-ttu-id="ecefe-248">Ez módosítható toothis toofix esetleges hibák létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ecefe-248">You can change it toothis toofix any build errors:</span></span>

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="special-case-for-web-applications"></a><span data-ttu-id="ecefe-249">Webes alkalmazásokhoz, különleges esetben</span><span class="sxs-lookup"><span data-stu-id="ecefe-249">Special case for web applications</span></span>
<span data-ttu-id="ecefe-250">Ha a webes alkalmazás az rendezi sorba `DocumentSearchResponse` közvetlen toosend keresési találatok toohello böngésző, szüksége lesz toochange a kódot, vagy hello eredmények nem fogja szerializálni megfelelően.</span><span class="sxs-lookup"><span data-stu-id="ecefe-250">If you have a web application that serializes `DocumentSearchResponse` directly toosend search results toohello browser, you will need toochange your code or hello results will not serialize correctly.</span></span> <span data-ttu-id="ecefe-251">Ha például a kód néz ki:</span><span class="sxs-lookup"><span data-stu-id="ecefe-251">For example, if your code looks like this:</span></span>

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

<span data-ttu-id="ecefe-252">Módosíthatja az első hello `.Results` hello keresési válasz toofix keresési eredmény Renderelés tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="ecefe-252">You can change it by getting hello `.Results` property of hello search response toofix search result rendering:</span></span>

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

<span data-ttu-id="ecefe-253">Hogy toolook ilyen esetekben a kódban. **hello fordító nem figyelmeztet,** mert `JsonResult.Data` típusú `object`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-253">You will have toolook for such cases in your code yourself; **hello compiler will not warn you** because `JsonResult.Data` is of type `object`.</span></span>

#### <a name="cloudexception-changes"></a><span data-ttu-id="ecefe-254">CloudException módosítások</span><span class="sxs-lookup"><span data-stu-id="ecefe-254">CloudException changes</span></span>
<span data-ttu-id="ecefe-255">Hello `CloudException` osztály hello tért át `Hyak.Common` névtér toohello `Microsoft.Rest.Azure` névtér.</span><span class="sxs-lookup"><span data-stu-id="ecefe-255">hello `CloudException` class has moved from hello `Hyak.Common` namespace toohello `Microsoft.Rest.Azure` namespace.</span></span> <span data-ttu-id="ecefe-256">Emellett a `Error` tulajdonság túl át lett nevezve`Body`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-256">Also, its `Error` property has been renamed too`Body`.</span></span>

#### <a name="searchserviceclient-and-searchindexclient-changes"></a><span data-ttu-id="ecefe-257">SearchServiceClient és SearchIndexClient módosítása</span><span class="sxs-lookup"><span data-stu-id="ecefe-257">SearchServiceClient and SearchIndexClient changes</span></span>
<span data-ttu-id="ecefe-258">hello típusú hello `Credentials` tulajdonság megváltozott `SearchCredentials` tooits alaposztály, `ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-258">hello type of hello `Credentials` property has changed from `SearchCredentials` tooits base class, `ServiceClientCredentials`.</span></span> <span data-ttu-id="ecefe-259">Ha tooaccess hello kell `SearchCredentials` , egy `SearchIndexClient` vagy `SearchServiceClient`, használjon új hello `SearchCredentials` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ecefe-259">If you need tooaccess hello `SearchCredentials` of a `SearchIndexClient` or `SearchServiceClient`, please use hello new `SearchCredentials` property.</span></span>

<span data-ttu-id="ecefe-260">Az SDK-val hello régebbi verzióit `SearchServiceClient` és `SearchIndexClient` konstruktorokat, amelyek került volna egy `HttpClient` paraméter.</span><span class="sxs-lookup"><span data-stu-id="ecefe-260">In older versions of hello SDK, `SearchServiceClient` and `SearchIndexClient` had constructors that took an `HttpClient` parameter.</span></span> <span data-ttu-id="ecefe-261">Ezek konstruktorok használatát, amelyek helyett egy `HttpClientHandler` és tömbje `DelegatingHandler` objektumok.</span><span class="sxs-lookup"><span data-stu-id="ecefe-261">These have been replaced with constructors that take an `HttpClientHandler` and an array of `DelegatingHandler` objects.</span></span> <span data-ttu-id="ecefe-262">Így könnyebben tooinstall egyéni kezelők toopre-folyamat HTTP-kérelmek szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="ecefe-262">This makes it easier tooinstall custom handlers toopre-process HTTP requests if necessary.</span></span>

<span data-ttu-id="ecefe-263">Végezetül hello konstruktorokat, amelyek tartott egy `Uri` és `SearchCredentials` megváltoztak.</span><span class="sxs-lookup"><span data-stu-id="ecefe-263">Finally, hello constructors that took a `Uri` and `SearchCredentials` have changed.</span></span> <span data-ttu-id="ecefe-264">Ha például rendelkezik kódot, amely a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="ecefe-264">For example, if you have code that looks like this:</span></span>

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

<span data-ttu-id="ecefe-265">Ez módosítható toothis toofix esetleges hibák létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ecefe-265">You can change it toothis toofix any build errors:</span></span>

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

<span data-ttu-id="ecefe-266">Vegye figyelembe, hogy a hello hello típusú paraméter hitelesítő adatok is a túl megváltozott`ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-266">Also note that hello type of hello credentials parameter has changed too`ServiceClientCredentials`.</span></span> <span data-ttu-id="ecefe-267">Ez az valószínű tooaffect a kód óta `SearchCredentials` származó `ServiceClientCredentials`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-267">This is unlikely tooaffect your code since `SearchCredentials` is derived from `ServiceClientCredentials`.</span></span>

#### <a name="passing-a-request-id"></a><span data-ttu-id="ecefe-268">A kérelem azonosító továbbításához</span><span class="sxs-lookup"><span data-stu-id="ecefe-268">Passing a request ID</span></span>
<span data-ttu-id="ecefe-269">Hello SDK régebbi verzióit, beállíthat egy Kérelemazonosító a hello `SearchServiceClient` vagy `SearchIndexClient` és az összes kérelem toohello REST API szerepel.</span><span class="sxs-lookup"><span data-stu-id="ecefe-269">In older versions of hello SDK, you could set a request ID on hello `SearchServiceClient` or `SearchIndexClient` and it would be included in every request toohello REST API.</span></span> <span data-ttu-id="ecefe-270">Ez akkor hasznos, kapcsolatos hibák elhárításához a keresőszolgáltatása Ha Ha toocontact támogatásra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ecefe-270">This is useful for troubleshooting issues with your search service if you need toocontact support.</span></span> <span data-ttu-id="ecefe-271">Azonban is hasznos tooset egyedi Kérelemazonosító minden művelethez helyett toouse hello minden műveletnél ugyanazzal az Azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="ecefe-271">However, it is more useful tooset a unique request ID for every operation rather than toouse hello same ID for all operations.</span></span> <span data-ttu-id="ecefe-272">Emiatt hello `SetClientRequestId` módszerek `SearchServiceClient` és `SearchIndexClient` el lettek távolítva.</span><span class="sxs-lookup"><span data-stu-id="ecefe-272">For this reason, hello `SetClientRequestId` methods of `SearchServiceClient` and `SearchIndexClient` have been removed.</span></span> <span data-ttu-id="ecefe-273">Ehelyett átadhatók egy kérelem azonosítója tooeach műveleti metódusának keresztül hello választható `SearchRequestOptions` paraméter.</span><span class="sxs-lookup"><span data-stu-id="ecefe-273">Instead, you can pass a request ID tooeach operation method via hello optional `SearchRequestOptions` parameter.</span></span>

> [!NOTE]
> <span data-ttu-id="ecefe-274">Hello SDK későbbi kiadásában egy új mechanizmust beállításához a Kérelemazonosító globálisan hello ügyfélen objektumok azonos-e más Azure SDK-k által használt hello megközelítés adjuk hozzá.</span><span class="sxs-lookup"><span data-stu-id="ecefe-274">In a future release of hello SDK, we will add a new mechanism for setting a request ID globally on hello client objects that is consistent with hello approach used by other Azure SDKs.</span></span>
> 
> 

#### <a name="example"></a><span data-ttu-id="ecefe-275">Példa</span><span class="sxs-lookup"><span data-stu-id="ecefe-275">Example</span></span>
<span data-ttu-id="ecefe-276">Ha kódot, amely a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="ecefe-276">If you have code that looks like this:</span></span>

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

<span data-ttu-id="ecefe-277">Ez módosítható toothis toofix esetleges hibák létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ecefe-277">You can change it toothis toofix any build errors:</span></span>

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a><span data-ttu-id="ecefe-278">Csatoló nevének módosítása</span><span class="sxs-lookup"><span data-stu-id="ecefe-278">Interface name changes</span></span>
<span data-ttu-id="ecefe-279">hello művelet csoport illesztőneveket rendelkezik a megfelelő tulajdonságneveket konzisztens módosított toobe:</span><span class="sxs-lookup"><span data-stu-id="ecefe-279">hello operation group interface names have all changed toobe consistent with their corresponding property names:</span></span>

* <span data-ttu-id="ecefe-280">hello típusú `ISearchServiceClient.Indexes` át lett nevezve a `IIndexOperations` túl`IIndexesOperations`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-280">hello type of `ISearchServiceClient.Indexes` has been renamed from `IIndexOperations` too`IIndexesOperations`.</span></span>
* <span data-ttu-id="ecefe-281">hello típusú `ISearchServiceClient.Indexers` át lett nevezve a `IIndexerOperations` túl`IIndexersOperations`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-281">hello type of `ISearchServiceClient.Indexers` has been renamed from `IIndexerOperations` too`IIndexersOperations`.</span></span>
* <span data-ttu-id="ecefe-282">hello típusú `ISearchServiceClient.DataSources` át lett nevezve a `IDataSourceOperations` túl`IDataSourcesOperations`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-282">hello type of `ISearchServiceClient.DataSources` has been renamed from `IDataSourceOperations` too`IDataSourcesOperations`.</span></span>
* <span data-ttu-id="ecefe-283">hello típusú `ISearchIndexClient.Documents` át lett nevezve a `IDocumentOperations` túl`IDocumentsOperations`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-283">hello type of `ISearchIndexClient.Documents` has been renamed from `IDocumentOperations` too`IDocumentsOperations`.</span></span>

<span data-ttu-id="ecefe-284">Ez a változás esetén valószínű tooaffect a kód mocks ezen felületek tesztelési célból létrehozott.</span><span class="sxs-lookup"><span data-stu-id="ecefe-284">This change is unlikely tooaffect your code unless you created mocks of these interfaces for test purposes.</span></span>

<a name="BugFixesV1"></a>

### <a name="bug-fixes-in-version-11"></a><span data-ttu-id="ecefe-285">Hibajavítások 1.1-es verzió</span><span class="sxs-lookup"><span data-stu-id="ecefe-285">Bug fixes in version 1.1</span></span>
<span data-ttu-id="ecefe-286">Hiba történt a hello Azure Search .NET SDK vonatkozó tooserialization egyéni modell osztályok régebbi verzióit.</span><span class="sxs-lookup"><span data-stu-id="ecefe-286">There was a bug in older versions of hello Azure Search .NET SDK relating tooserialization of custom model classes.</span></span> <span data-ttu-id="ecefe-287">hello hiba akkor fordulhat elő, ha létrehozott egy egyéni modellosztállyal egy nem nullázható értéktípus tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="ecefe-287">hello bug could occur if you created a custom model class with a property of a non-nullable value type.</span></span>

#### <a name="steps-tooreproduce"></a><span data-ttu-id="ecefe-288">Lépéseket tooreproduce</span><span class="sxs-lookup"><span data-stu-id="ecefe-288">Steps tooreproduce</span></span>
<span data-ttu-id="ecefe-289">Hozzon létre egy egyéni modellosztállyal egy tulajdonság nem nullázható értéktípus.</span><span class="sxs-lookup"><span data-stu-id="ecefe-289">Create a custom model class with a property of non-nullable value type.</span></span> <span data-ttu-id="ecefe-290">Például hozzáadhat egy nyilvános `UnitCount` típusú tulajdonság `int` helyett `int?`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-290">For example, add a public `UnitCount` property of type `int` instead of `int?`.</span></span>

<span data-ttu-id="ecefe-291">Ha egy dokumentum hello alapértelmezett érték az adott típusú indexeli (például 0 `int`), hello mező az Azure Search null értékű lesz.</span><span class="sxs-lookup"><span data-stu-id="ecefe-291">If you index a document with hello default value of that type (for example, 0 for `int`), hello field will be null in Azure Search.</span></span> <span data-ttu-id="ecefe-292">Ha ezt követően a dokumentum, hello `Search` hívás kivételhibát `JsonSerializationException` panaszos, amely nem konvertálható `null` túl`int`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-292">If you subsequently search for that document, hello `Search` call will throw `JsonSerializationException` complaining that it can't convert `null` too`int`.</span></span>

<span data-ttu-id="ecefe-293">Szűrők is, nem működnek megfelelően, mert null írt toohello index szánt hello érték helyett.</span><span class="sxs-lookup"><span data-stu-id="ecefe-293">Also, filters may not work as expected since null was written toohello index instead of hello intended value.</span></span>

#### <a name="fix-details"></a><span data-ttu-id="ecefe-294">Javítsa ki a részletei</span><span class="sxs-lookup"><span data-stu-id="ecefe-294">Fix details</span></span>
<span data-ttu-id="ecefe-295">A probléma hello SDK 1.1-es verziójában megoldottuk azt.</span><span class="sxs-lookup"><span data-stu-id="ecefe-295">We have fixed this issue in version 1.1 of hello SDK.</span></span> <span data-ttu-id="ecefe-296">Most, ha van egy modell ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="ecefe-296">Now, if you have a model class like this:</span></span>

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

<span data-ttu-id="ecefe-297">és a beállított `IntValue` too0, hogy értéke most már megfelelően szerializálható kézjegyként 0 hello keresztülhaladnak a hálózaton, és 0 hello indexben tárolt.</span><span class="sxs-lookup"><span data-stu-id="ecefe-297">and you set `IntValue` too0, that value is now correctly serialized as 0 on hello wire and stored as 0 in hello index.</span></span> <span data-ttu-id="ecefe-298">Kerek esetén is megfelelően működik-e.</span><span class="sxs-lookup"><span data-stu-id="ecefe-298">Round tripping also works as expected.</span></span>

<span data-ttu-id="ecefe-299">Nincs tisztában ezt a módszert egy potenciális problémát toobe: Ha a modell típusa egy nem nullázható tulajdonságot használja, akkor túl**garantálni** , hogy az indexben nem dokumentumok hello megfelelő mező null értéket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ecefe-299">There is one potential issue toobe aware of with this approach: If you use a model type with a non-nullable property, you have too**guarantee** that no documents in your index contain a null value for hello corresponding field.</span></span> <span data-ttu-id="ecefe-300">Hello SDK sem hello Azure Search REST API segítségével meg tooenforce ez.</span><span class="sxs-lookup"><span data-stu-id="ecefe-300">Neither hello SDK nor hello Azure Search REST API will help you tooenforce this.</span></span>

<span data-ttu-id="ecefe-301">Ez nem csak egy elméleti szempont: képzelhető el, egy olyan forgatókönyvet, ahol hozzáadhat egy új tooan meglévő mezőindex, amely típusú `Edm.Int32`.</span><span class="sxs-lookup"><span data-stu-id="ecefe-301">This is not just a hypothetical concern: Imagine a scenario where you add a new field tooan existing index that is of type `Edm.Int32`.</span></span> <span data-ttu-id="ecefe-302">Hello index definíciójának frissítése után összes dokumentumot lesz az új mező null értéket (mivel az Azure Search nullázható minden esetében).</span><span class="sxs-lookup"><span data-stu-id="ecefe-302">After updating hello index definition, all documents will have a null value for that new field (since all types are nullable in Azure Search).</span></span> <span data-ttu-id="ecefe-303">Ha az egy nem nullázható majd használja egy modell osztály `int` tulajdonság, mező, elérhetővé válik egy `JsonSerializationException` ez, például tooretrieve dokumentumok tett kísérlet során:</span><span class="sxs-lookup"><span data-stu-id="ecefe-303">If you then use a model class with a non-nullable `int` property for that field, you will get a `JsonSerializationException` like this when trying tooretrieve documents:</span></span>

    Error converting value {null} tootype 'System.Int32'. Path 'IntValue'.

<span data-ttu-id="ecefe-304">Ezért továbbra is javasoljuk, hogy használjon nullázható típusok a modell osztályok az ajánlott eljárás.</span><span class="sxs-lookup"><span data-stu-id="ecefe-304">For this reason, we still recommend that you use nullable types in your model classes as a best practice.</span></span>

<span data-ttu-id="ecefe-305">Hiba és hello javítás a további részletekért lásd: [a Githubon probléma](https://github.com/Azure/azure-sdk-for-net/issues/1063).</span><span class="sxs-lookup"><span data-stu-id="ecefe-305">For more details on this bug and hello fix, please see [this issue on GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).</span></span>

