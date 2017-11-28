---
title: "aaaHow toomodel összetett adattípusú az Azure Search |} Microsoft Docs"
description: "A beágyazott vagy hierarchikus adatstruktúrák modellezhető az Azure Search-index egybesimított sorhalmaz és gyűjtemények adattípus használatával."
services: search
documentationcenter: 
author: LiamCa
manager: pablocas
editor: 
tags: complex data types; compound data types; aggregate data types
ms.assetid: e4bf86b4-497a-4179-b09f-c1b56c3c0bb2
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: liamca
ms.openlocfilehash: b330c5b322f4f33123a454be11733b977684b9e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomodel-complex-data-types-in-azure-search"></a><span data-ttu-id="20af0-103">Hogyan toomodel összetett adattípusokat az Azure Search</span><span class="sxs-lookup"><span data-stu-id="20af0-103">How toomodel complex data types in Azure Search</span></span>
<span data-ttu-id="20af0-104">Külső adatkészletek használt toopopulate Azure Search-index néha hierarchikus vagy beágyazott alépítményeit, amely nem felosztása szépen táblázatos sorhalmazt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="20af0-104">External datasets used toopopulate an Azure Search index sometimes include hierarchical or nested substructures that do not break down neatly into a tabular rowset.</span></span> <span data-ttu-id="20af0-105">Példa ilyen struktúrák előfordulhat, hogy több helyről és telefonszámok tartalmazza az egy ügyfél több színek és méretek egyetlen termékváltozat, több szerzők egy könyv, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="20af0-105">Examples of such structures might include multiple locations and phone numbers for a single customer, multiple colors and sizes for a single SKU, multiple authors of a single book, and so on.</span></span> <span data-ttu-id="20af0-106">A modellezési feltételeit, láthatja, ezen szerkezetek említett tooas *összetett adattípusú*, *összetett adattípusú*, *összetett adattípusok*, vagy *összesített adattípusok*, kevés a tooname.</span><span class="sxs-lookup"><span data-stu-id="20af0-106">In modeling terms, you might see these structures referred tooas *complex data types*, *compound data types*, *composite data types*, or *aggregate data types*, tooname a few.</span></span>

<span data-ttu-id="20af0-107">Összetett adattípusú nem natív módon támogatja az Azure Search, de bevált megoldás tartalmaz egy kétlépéses folyamat egybesimítását hello struktúra és használata a **gyűjtemény** adattípus tooreconstitute hello belső struktúra.</span><span class="sxs-lookup"><span data-stu-id="20af0-107">Complex data types are not natively supported in Azure Search, but a proven workaround includes a two-step process of flattening hello structure and then using a **Collection** data type tooreconstitute hello interior structure.</span></span> <span data-ttu-id="20af0-108">A következő cikkben ismertetett hello módszer lehetővé teszi a tartalom toobe hello keresett, jellemzőalapú, és szűrhetők.</span><span class="sxs-lookup"><span data-stu-id="20af0-108">Following hello technique described in this article allows hello content toobe searched, faceted, filtered, and sorted.</span></span>

## <a name="example-of-a-complex-data-structure"></a><span data-ttu-id="20af0-109">Összetett adatszerkezet – példa</span><span class="sxs-lookup"><span data-stu-id="20af0-109">Example of a complex data structure</span></span>
<span data-ttu-id="20af0-110">A szóban forgó adatok hello általában JSON vagy XML-dokumentumot, vagy egy NoSQL-tároló, például az Azure Cosmos DB elem található.</span><span class="sxs-lookup"><span data-stu-id="20af0-110">Typically, hello data in question resides as a set of JSON or XML documents, or as items in a NoSQL store such as Azure Cosmos DB.</span></span> <span data-ttu-id="20af0-111">Szerkezete hello challenge ered, amelyek több alárendelt elemeket a Keresés és a szűrt toobe.</span><span class="sxs-lookup"><span data-stu-id="20af0-111">Structurally, hello challenge stems from having multiple child items that need toobe searched and filtered.</span></span>  <span data-ttu-id="20af0-112">A ábrázoló hello megoldás kiindulási pontként érvénybe hello számos olyan, az ügyfelek például JSON-dokumentum a következő:</span><span class="sxs-lookup"><span data-stu-id="20af0-112">As a starting point for illustrating hello workaround, take hello following JSON document that lists a set of contacts as an example:</span></span>

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

<span data-ttu-id="20af0-113">Amíg hello mezők "id" nevű, "name" és "Vállalati" csak egyszerűen képezhető le egy az egyhez típusú mezői belül az Azure Search-index, hello "helyek" mező a helyek, hogy mindkét egy csoportja hely azonosítók, valamint a hely leírása tömböt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="20af0-113">While hello fields named ‘id’, ‘name’ and ‘company’ can easily be mapped one-to-one as fields within an Azure Search index, hello ‘locations’ field contains an array of locations, having both a set of location IDs as well as location descriptions.</span></span> <span data-ttu-id="20af0-114">Fényében, hogy az Azure Search nem rendelkezik olyan adattípusú, amely támogatja ezt, igazolnia kell a különböző módokon toomodel Ez az Azure Search.</span><span class="sxs-lookup"><span data-stu-id="20af0-114">Given that Azure Search does not have a data type that supports this, we need a different way toomodel this in Azure Search.</span></span> 

> [!NOTE]
> <span data-ttu-id="20af0-115">Ezzel a módszerrel is blogbejegyzés ismerteti Kirk Evans által [indexelő DocumentDB az Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), amely jelzi, hogy egy technika az úgynevezett "simítási hello adatok", amely során egy nevű mező kellene lennie `locationsID` és `locationsDescription` amelyek [gyűjtemények](https://msdn.microsoft.com/library/azure/dn798938.aspx) (vagy karakterláncok).</span><span class="sxs-lookup"><span data-stu-id="20af0-115">This technique is also described by Kirk Evans in a blog post [Indexing DocumentDB with Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), which shows a technique called "flattening hello data", whereby you would have a field called `locationsID` and `locationsDescription` that are both [collections](https://msdn.microsoft.com/library/azure/dn798938.aspx) (or an array of strings).</span></span>   
> 
> 

## <a name="part-1-flatten-hello-array-into-individual-fields"></a><span data-ttu-id="20af0-116">1. rész: Hello tömb Egybesimítására egyes mezőkbe</span><span class="sxs-lookup"><span data-stu-id="20af0-116">Part 1: Flatten hello array into individual fields</span></span>
<span data-ttu-id="20af0-117">toocreate az Azure Search-index, amelyre ez az adatkészlet egyes mezőket hello beágyazott tartóelemeket létrehozása: `locationsID` és `locationsDescription` adatok típussal rendelkező [gyűjtemények](https://msdn.microsoft.com/library/azure/dn798938.aspx) (vagy karakterláncok).</span><span class="sxs-lookup"><span data-stu-id="20af0-117">toocreate an Azure Search index that accommodates this dataset, create individual fields for hello nested substructure: `locationsID` and `locationsDescription` with a data type of [collections](https://msdn.microsoft.com/library/azure/dn798938.aspx) (or an array of strings).</span></span> <span data-ttu-id="20af0-118">A mezők hello be kellene index hello értéket "1" és "2" `locationsID` "3" & "4" hello a John Smith és hello értékek mezőjében `locationsID` Ilona Campbell mezőt.</span><span class="sxs-lookup"><span data-stu-id="20af0-118">In these fields you would index hello values ‘1’ and ‘2’ into hello `locationsID` field for John Smith and hello values ‘3’ & ‘4’ into hello `locationsID` field for Jen Campbell.</span></span>  

<span data-ttu-id="20af0-119">Az adatok Azure Search belül néz ki:</span><span class="sxs-lookup"><span data-stu-id="20af0-119">Your data within Azure Search would look like this:</span></span> 

![mintaadatokat, 2 sor](./media/search-howto-complex-data-types/sample-data.png)

## <a name="part-2-add-a-collection-field-in-hello-index-definition"></a><span data-ttu-id="20af0-121">2. rész: Hello Indexdefiníció gyűjtemény mező felvétele</span><span class="sxs-lookup"><span data-stu-id="20af0-121">Part 2: Add a collection field in hello index definition</span></span>
<span data-ttu-id="20af0-122">Hello mező definíciók hello a sémát indexeli, előfordulhat, hogy keresse meg a hasonló toothis példa.</span><span class="sxs-lookup"><span data-stu-id="20af0-122">In hello index schema, hello field definitions might look similar toothis example.</span></span>

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-hello-index"></a><span data-ttu-id="20af0-123">Keresési viselkedések érvényesítése és kiterjeszti a hello index</span><span class="sxs-lookup"><span data-stu-id="20af0-123">Validate search behaviors and optionally extend hello index</span></span>
<span data-ttu-id="20af0-124">Ha létrehozott hello index és betöltött hello adatokat, most tesztelheti hello megoldás tooverify keresési lekérdezés végrehajtásának hello adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="20af0-124">Assuming you created hello index and loaded hello data, you can now test hello solution tooverify search query execution against hello dataset.</span></span> <span data-ttu-id="20af0-125">Minden egyes **gyűjtemény** mező lehet **kereshető**, **szűrhető** és **kategorizálható**.</span><span class="sxs-lookup"><span data-stu-id="20af0-125">Each **collection** field should be **searchable**, **filterable** and **facetable**.</span></span> <span data-ttu-id="20af0-126">Meg kell tudni toorun lekérdezések például:</span><span class="sxs-lookup"><span data-stu-id="20af0-126">You should be able toorun queries like:</span></span>

* <span data-ttu-id="20af0-127">Hello "Adventureworks központ" dolgozó összes személyek keresése.</span><span class="sxs-lookup"><span data-stu-id="20af0-127">Find all people who work at hello ‘Adventureworks Headquarters’.</span></span>
* <span data-ttu-id="20af0-128">Megszámlálása egy otthoni Office dolgozó személyek hello száma.</span><span class="sxs-lookup"><span data-stu-id="20af0-128">Get a count of hello number of people who work in a ‘Home Office’.</span></span>  
* <span data-ttu-id="20af0-129">Egy otthoni Office működéséhez hello személyek megjelenítése milyen többi iroda működnek együtt az egyes helyeken hello személyek számát.</span><span class="sxs-lookup"><span data-stu-id="20af0-129">Of hello people who work at a ‘Home Office’, show what other offices they work along with a count of hello people in each location.</span></span>  

<span data-ttu-id="20af0-130">Ha ezzel a technikával egymástól esik akkor, ha egy keresést, hogy mind hello helyazonosító, valamint hello hely leírását toodo van szüksége.</span><span class="sxs-lookup"><span data-stu-id="20af0-130">Where this technique falls apart is when you need toodo a search that combines both hello location id as well as hello location description.</span></span> <span data-ttu-id="20af0-131">Példa:</span><span class="sxs-lookup"><span data-stu-id="20af0-131">For example:</span></span>

* <span data-ttu-id="20af0-132">Minden személyek keresése, ahol egy otthoni Office rendelkeznek, és egy helyet azonosító, a 4.</span><span class="sxs-lookup"><span data-stu-id="20af0-132">Find all people where they have a Home Office AND has a location ID of 4.</span></span>  

<span data-ttu-id="20af0-133">Ha ilyen kikeresi hello eredeti tartalom Emlékezzen vissza:</span><span class="sxs-lookup"><span data-stu-id="20af0-133">If you recall hello original content looked like this:</span></span>

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

<span data-ttu-id="20af0-134">Azonban most, hogy a Microsoft hello adatok rendelkezik rétegekben külön mezők, tudunk semmilyen módon nem tudhatja, hogy ha hello Ilona Campbell otthoni Office túl`locationsID 3` vagy `locationsID 4`.</span><span class="sxs-lookup"><span data-stu-id="20af0-134">However, now that we have separated hello data into separate fields, we have no way of knowing if hello Home Office for Jen Campbell relates too`locationsID 3` or `locationsID 4`.</span></span>  

<span data-ttu-id="20af0-135">Ebben az esetben toohandle másik mezőt, amely egyesíti hello adatok egyetlen gyűjtemény hello index határozza meg.</span><span class="sxs-lookup"><span data-stu-id="20af0-135">toohandle this case, define another field in hello index that combines all of hello data into a single collection.</span></span>  <span data-ttu-id="20af0-136">A példa kedvéért nevezzük fog ebben a mezőben `locationsCombined` és azt az egyes hello tartalom található egy `||` Bár választhat, amely úgy gondolja, hogy bármely elválasztó karakter a tartalom egyedi készletének lenne.</span><span class="sxs-lookup"><span data-stu-id="20af0-136">For our example, we will call this field `locationsCombined` and we will separate hello content with a `||` although you can choose any separator that you think would be a unique set of characters for your content.</span></span> <span data-ttu-id="20af0-137">Példa:</span><span class="sxs-lookup"><span data-stu-id="20af0-137">For example:</span></span> 

![mintaadatokat, 2 sor elválasztóval](./media/search-howto-complex-data-types/sample-data-2.png)

<span data-ttu-id="20af0-139">Ennek segítségével `locationsCombined` mezőben azt most már képes még több lekérdezések, többek között:</span><span class="sxs-lookup"><span data-stu-id="20af0-139">Using this `locationsCombined` field, we can now accommodate even more queries, such as:</span></span>

* <span data-ttu-id="20af0-140">Egy szám irodában egy"otthoni" helyen lévő azonosítója "4" dolgozó személyek megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="20af0-140">Show a count of people who work at a ‘Home Office’ with location Id of ‘4’.</span></span>  
* <span data-ttu-id="20af0-141">Egy otthoni irodában dolgozó személyek keresése helyen lévő "4" azonosítójú.</span><span class="sxs-lookup"><span data-stu-id="20af0-141">Search for people who work at a ‘Home Office’ with location Id ‘4’.</span></span> 

## <a name="limitations"></a><span data-ttu-id="20af0-142">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="20af0-142">Limitations</span></span>
<span data-ttu-id="20af0-143">Ez a módszer akkor hasznos, ha több forgatókönyvet, de nincs minden esetben alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="20af0-143">This technique is useful for a number of scenarios, but it is not applicable in every case.</span></span>  <span data-ttu-id="20af0-144">Példa:</span><span class="sxs-lookup"><span data-stu-id="20af0-144">For example:</span></span>

1. <span data-ttu-id="20af0-145">Ha Ön az összetett adattípusú nem rendelkezik a statikus mezők halmaza alapján, és nincs módja toomap történt összes hello lehetséges típusokat tooa egyetlen mezőben.</span><span class="sxs-lookup"><span data-stu-id="20af0-145">If you do not have a static set of fields in your complex data type and there was no way toomap all hello possible types tooa single field.</span></span> 
2. <span data-ttu-id="20af0-146">Hello beágyazott objektumok frissítése szükséges néhány további munkahelyi toodetermine pontosan mit kell toobe hello Azure Search-index frissítése</span><span class="sxs-lookup"><span data-stu-id="20af0-146">Updating hello nested objects requires some extra work toodetermine exactly what needs toobe updated in hello Azure Search index</span></span>

## <a name="sample-code"></a><span data-ttu-id="20af0-147">Mintakód</span><span class="sxs-lookup"><span data-stu-id="20af0-147">Sample code</span></span>
<span data-ttu-id="20af0-148">Példa a hogyan tooindex összetett JSON-adatok beállításának Azure Search szolgáltatásba történő, és képes lekérdezések száma ehhez az adatkészlethez, ezzel [GitHub-tárház](https://github.com/liamca/AzureSearchComplexTypes).</span><span class="sxs-lookup"><span data-stu-id="20af0-148">You can see an example on how tooindex a complex JSON data set into Azure Search and perform a number of queries over this dataset at this [GitHub repo](https://github.com/liamca/AzureSearchComplexTypes).</span></span>

## <a name="next-step"></a><span data-ttu-id="20af0-149">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="20af0-149">Next step</span></span>
<span data-ttu-id="20af0-150">[Natív támogatást az összetett adattípusú szavazzon](https://feedback.azure.com/forums/263029-azure-search) hello Azure keresési UserVoice lapon, és minden további megadása, hogy szeretné funkció végrehajtására vonatkozó tooconsider.</span><span class="sxs-lookup"><span data-stu-id="20af0-150">[Vote for native support for complex data types](https://feedback.azure.com/forums/263029-azure-search) on hello Azure Search UserVoice page and provide any additional input that you’d like us tooconsider regarding feature implementation.</span></span> <span data-ttu-id="20af0-151">Közvetlenül a Twitteren: toome ki is elérhető @liamca.</span><span class="sxs-lookup"><span data-stu-id="20af0-151">You can also reach out toome directly on Twitter at @liamca.</span></span>

