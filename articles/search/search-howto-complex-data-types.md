---
title: "Az Azure Search összetett adattípusú modellek hogyan |} Microsoft Docs"
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
ms.openlocfilehash: d576fd7bb267ae7a100589413185b595e3b2be42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-model-complex-data-types-in-azure-search"></a><span data-ttu-id="af26b-103">Hogyan modell összetett adattípusú az Azure Search</span><span class="sxs-lookup"><span data-stu-id="af26b-103">How to model complex data types in Azure Search</span></span>
<span data-ttu-id="af26b-104">Az Azure Search-index néha feltöltésére használt külső adatkészletek hierarchikus vagy beágyazott alépítményeit, amely nem felosztása szépen táblázatos sorhalmazt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="af26b-104">External datasets used to populate an Azure Search index sometimes include hierarchical or nested substructures that do not break down neatly into a tabular rowset.</span></span> <span data-ttu-id="af26b-105">Példa ilyen struktúrák előfordulhat, hogy több helyről és telefonszámok tartalmazza az egy ügyfél több színek és méretek egyetlen termékváltozat, több szerzők egy könyv, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="af26b-105">Examples of such structures might include multiple locations and phone numbers for a single customer, multiple colors and sizes for a single SKU, multiple authors of a single book, and so on.</span></span> <span data-ttu-id="af26b-106">A modellezési feltételeit, ezen szerkezetek néven megjelenhet *összetett adattípusú*, *összetett adattípusú*, *összetett adattípusok*, vagy *összesített adattípusok*, csak hogy néhányat említsünk.</span><span class="sxs-lookup"><span data-stu-id="af26b-106">In modeling terms, you might see these structures referred to as *complex data types*, *compound data types*, *composite data types*, or *aggregate data types*, to name a few.</span></span>

<span data-ttu-id="af26b-107">Összetett adattípusú nem natív módon támogatja az Azure Search, de bevált megoldás tartalmaz egy kétlépéses folyamat egybesimítását szerkezetét, és használata a **gyűjtemény** adattípus meglévő kölcsönökre a belső struktúrában.</span><span class="sxs-lookup"><span data-stu-id="af26b-107">Complex data types are not natively supported in Azure Search, but a proven workaround includes a two-step process of flattening the structure and then using a **Collection** data type to reconstitute the interior structure.</span></span> <span data-ttu-id="af26b-108">A cikkben leírt eljárást követve lehetővé teszi a tartalom keres, jellemzőalapú, és szűrhetők.</span><span class="sxs-lookup"><span data-stu-id="af26b-108">Following the technique described in this article allows the content to be searched, faceted, filtered, and sorted.</span></span>

## <a name="example-of-a-complex-data-structure"></a><span data-ttu-id="af26b-109">Összetett adatszerkezet – példa</span><span class="sxs-lookup"><span data-stu-id="af26b-109">Example of a complex data structure</span></span>
<span data-ttu-id="af26b-110">Általában a szóban forgó adatok JSON- vagy XML-dokumentumot, vagy egy NoSQL-tároló, például az Azure Cosmos DB elem található.</span><span class="sxs-lookup"><span data-stu-id="af26b-110">Typically, the data in question resides as a set of JSON or XML documents, or as items in a NoSQL store such as Azure Cosmos DB.</span></span> <span data-ttu-id="af26b-111">Szerkezete a kihívás ered, több gyermek elemeket kell keresni és szűrt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="af26b-111">Structurally, the challenge stems from having multiple child items that need to be searched and filtered.</span></span>  <span data-ttu-id="af26b-112">A bemutató a megoldás kiindulási pontként hajtsa végre a következő JSON-dokumentum, amely számos olyan példaként ügyfelek:</span><span class="sxs-lookup"><span data-stu-id="af26b-112">As a starting point for illustrating the workaround, take the following JSON document that lists a set of contacts as an example:</span></span>

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

<span data-ttu-id="af26b-113">Amíg a mezőket "id" nevű, "name" és "Vállalati" csak egyszerűen képezhető le egy az egyhez típusú mezői belül az Azure Search-index, a "helyek" mező a helyek, hogy mindkét egy csoportja hely azonosítók, valamint a hely leírása tömböt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="af26b-113">While the fields named ‘id’, ‘name’ and ‘company’ can easily be mapped one-to-one as fields within an Azure Search index, the ‘locations’ field contains an array of locations, having both a set of location IDs as well as location descriptions.</span></span> <span data-ttu-id="af26b-114">Fényében, hogy az Azure Search nem rendelkezik olyan adattípusú, amely támogatja ezt, igazolnia kell a következő modellre: Ez az Azure Search különböző módokon.</span><span class="sxs-lookup"><span data-stu-id="af26b-114">Given that Azure Search does not have a data type that supports this, we need a different way to model this in Azure Search.</span></span> 

> [!NOTE]
> <span data-ttu-id="af26b-115">Ezzel a módszerrel is blogbejegyzés ismerteti Kirk Evans által [indexelő DocumentDB az Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), amely mutatja a technika nevű, "az adatok egybesimítását", amely lehetővé teszi, akkor egy egy nevű mező `locationsID` és `locationsDescription` , amely egyaránt [gyűjtemények](https://msdn.microsoft.com/library/azure/dn798938.aspx) (vagy karakterláncok).</span><span class="sxs-lookup"><span data-stu-id="af26b-115">This technique is also described by Kirk Evans in a blog post [Indexing DocumentDB with Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), which shows a technique called "flattening the data", whereby you would have a field called `locationsID` and `locationsDescription` that are both [collections](https://msdn.microsoft.com/library/azure/dn798938.aspx) (or an array of strings).</span></span>   
> 
> 

## <a name="part-1-flatten-the-array-into-individual-fields"></a><span data-ttu-id="af26b-116">1. lépés: A tömb egyes mezőkbe Egybesimítására</span><span class="sxs-lookup"><span data-stu-id="af26b-116">Part 1: Flatten the array into individual fields</span></span>
<span data-ttu-id="af26b-117">Ez az adatkészlet tervezhetők Azure Search-index, hozzon létre a beágyazott tartóelemeket egyes mezőket: `locationsID` és `locationsDescription` adatok típussal rendelkező [gyűjtemények](https://msdn.microsoft.com/library/azure/dn798938.aspx) (vagy karakterláncok).</span><span class="sxs-lookup"><span data-stu-id="af26b-117">To create an Azure Search index that accommodates this dataset, create individual fields for the nested substructure: `locationsID` and `locationsDescription` with a data type of [collections](https://msdn.microsoft.com/library/azure/dn798938.aspx) (or an array of strings).</span></span> <span data-ttu-id="af26b-118">A mezők be kellene index "1" vagy "2" érték a `locationsID` a John Smith és az értékeket a "3" & "4" mezőben a `locationsID` Ilona Campbell mezőt.</span><span class="sxs-lookup"><span data-stu-id="af26b-118">In these fields you would index the values ‘1’ and ‘2’ into the `locationsID` field for John Smith and the values ‘3’ & ‘4’ into the `locationsID` field for Jen Campbell.</span></span>  

<span data-ttu-id="af26b-119">Az adatok Azure Search belül néz ki:</span><span class="sxs-lookup"><span data-stu-id="af26b-119">Your data within Azure Search would look like this:</span></span> 

![mintaadatokat, 2 sor](./media/search-howto-complex-data-types/sample-data.png)

## <a name="part-2-add-a-collection-field-in-the-index-definition"></a><span data-ttu-id="af26b-121">2. lépés: Az index definícióját gyűjtemény mező felvétele</span><span class="sxs-lookup"><span data-stu-id="af26b-121">Part 2: Add a collection field in the index definition</span></span>
<span data-ttu-id="af26b-122">A sémát indexeli az a mező definíciók hasonlóan néznének ki ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="af26b-122">In the index schema, the field definitions might look similar to this example.</span></span>

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

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a><span data-ttu-id="af26b-123">Keresési viselkedések érvényesítése és kiterjeszti az index</span><span class="sxs-lookup"><span data-stu-id="af26b-123">Validate search behaviors and optionally extend the index</span></span>
<span data-ttu-id="af26b-124">Ha létrehozta az indexet, és az adatok betöltése, most tesztelheti a megoldás a dataset keresési lekérdezés végrehajtásának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="af26b-124">Assuming you created the index and loaded the data, you can now test the solution to verify search query execution against the dataset.</span></span> <span data-ttu-id="af26b-125">Minden egyes **gyűjtemény** mező lehet **kereshető**, **szűrhető** és **kategorizálható**.</span><span class="sxs-lookup"><span data-stu-id="af26b-125">Each **collection** field should be **searchable**, **filterable** and **facetable**.</span></span> <span data-ttu-id="af26b-126">Érdemes, például a lekérdezések futtatása:</span><span class="sxs-lookup"><span data-stu-id="af26b-126">You should be able to run queries like:</span></span>

* <span data-ttu-id="af26b-127">Az Adventureworks központban dolgozó összes személyek keresése.</span><span class="sxs-lookup"><span data-stu-id="af26b-127">Find all people who work at the ‘Adventureworks Headquarters’.</span></span>
* <span data-ttu-id="af26b-128">Megszámlálása egy otthoni Office dolgozó személyek számát.</span><span class="sxs-lookup"><span data-stu-id="af26b-128">Get a count of the number of people who work in a ‘Home Office’.</span></span>  
* <span data-ttu-id="af26b-129">Annak a személynek egy otthoni irodában dolgozó bemutatják, milyen többi iroda működnek együtt az egyes helyeken a személyek számát.</span><span class="sxs-lookup"><span data-stu-id="af26b-129">Of the people who work at a ‘Home Office’, show what other offices they work along with a count of the people in each location.</span></span>  

<span data-ttu-id="af26b-130">Ha ezzel a technikával egymástól esik esetén végre kell hajtani egy keresést, hogy mind a helyazonosító, valamint a hely leírása.</span><span class="sxs-lookup"><span data-stu-id="af26b-130">Where this technique falls apart is when you need to do a search that combines both the location id as well as the location description.</span></span> <span data-ttu-id="af26b-131">Példa:</span><span class="sxs-lookup"><span data-stu-id="af26b-131">For example:</span></span>

* <span data-ttu-id="af26b-132">Minden személyek keresése, ahol egy otthoni Office rendelkeznek, és egy helyet azonosító, a 4.</span><span class="sxs-lookup"><span data-stu-id="af26b-132">Find all people where they have a Home Office AND has a location ID of 4.</span></span>  

<span data-ttu-id="af26b-133">Ha Emlékezzen vissza az eredeti tartalom ehhez hasonló keresést végrehajtani:</span><span class="sxs-lookup"><span data-stu-id="af26b-133">If you recall the original content looked like this:</span></span>

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

<span data-ttu-id="af26b-134">Azonban most, hogy külön mezőkbe azt választotta el az adatokat, tudunk afelől, hogy ha nincs lehetőség a Kezdőlap Office Ilona Campbell vonatkozik a `locationsID 3` vagy `locationsID 4`.</span><span class="sxs-lookup"><span data-stu-id="af26b-134">However, now that we have separated the data into separate fields, we have no way of knowing if the Home Office for Jen Campbell relates to `locationsID 3` or `locationsID 4`.</span></span>  

<span data-ttu-id="af26b-135">Ebben az esetben kezelésére, ad meg, amely egyesíti az összes adat egyetlen gyűjtemény indexe másik mezőt.</span><span class="sxs-lookup"><span data-stu-id="af26b-135">To handle this case, define another field in the index that combines all of the data into a single collection.</span></span>  <span data-ttu-id="af26b-136">A példa kedvéért nevezzük fog ebben a mezőben `locationsCombined` , és azt fogja a tartalmat egy `||` Bár választhat, amely úgy gondolja, hogy bármely elválasztó karakter a tartalom egyedi készletének lenne.</span><span class="sxs-lookup"><span data-stu-id="af26b-136">For our example, we will call this field `locationsCombined` and we will separate the content with a `||` although you can choose any separator that you think would be a unique set of characters for your content.</span></span> <span data-ttu-id="af26b-137">Példa:</span><span class="sxs-lookup"><span data-stu-id="af26b-137">For example:</span></span> 

![mintaadatokat, 2 sor elválasztóval](./media/search-howto-complex-data-types/sample-data-2.png)

<span data-ttu-id="af26b-139">Ennek segítségével `locationsCombined` mezőben azt most már képes még több lekérdezések, többek között:</span><span class="sxs-lookup"><span data-stu-id="af26b-139">Using this `locationsCombined` field, we can now accommodate even more queries, such as:</span></span>

* <span data-ttu-id="af26b-140">Egy szám irodában egy"otthoni" helyen lévő azonosítója "4" dolgozó személyek megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="af26b-140">Show a count of people who work at a ‘Home Office’ with location Id of ‘4’.</span></span>  
* <span data-ttu-id="af26b-141">Egy otthoni irodában dolgozó személyek keresése helyen lévő "4" azonosítójú.</span><span class="sxs-lookup"><span data-stu-id="af26b-141">Search for people who work at a ‘Home Office’ with location Id ‘4’.</span></span> 

## <a name="limitations"></a><span data-ttu-id="af26b-142">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="af26b-142">Limitations</span></span>
<span data-ttu-id="af26b-143">Ez a módszer akkor hasznos, ha több forgatókönyvet, de nincs minden esetben alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="af26b-143">This technique is useful for a number of scenarios, but it is not applicable in every case.</span></span>  <span data-ttu-id="af26b-144">Példa:</span><span class="sxs-lookup"><span data-stu-id="af26b-144">For example:</span></span>

1. <span data-ttu-id="af26b-145">Ha Ön az összetett adattípusú nem rendelkezik a statikus mezők halmaza alapján, és nem lehetett azon lehetséges típusait hozzárendelése egy mező.</span><span class="sxs-lookup"><span data-stu-id="af26b-145">If you do not have a static set of fields in your complex data type and there was no way to map all the possible types to a single field.</span></span> 
2. <span data-ttu-id="af26b-146">A beágyazott objektumok frissítése néhány extra munkát annak meghatározására, hogy mit kell frissíteni az Azure Search-index szükséges</span><span class="sxs-lookup"><span data-stu-id="af26b-146">Updating the nested objects requires some extra work to determine exactly what needs to be updated in the Azure Search index</span></span>

## <a name="sample-code"></a><span data-ttu-id="af26b-147">Mintakód</span><span class="sxs-lookup"><span data-stu-id="af26b-147">Sample code</span></span>
<span data-ttu-id="af26b-148">Látható egy példa egy összetett JSON-adatkészlet index Azure Search szolgáltatásba történő, és képes lekérdezések száma ehhez az adatkészlethez ezzel [GitHub-tárház](https://github.com/liamca/AzureSearchComplexTypes).</span><span class="sxs-lookup"><span data-stu-id="af26b-148">You can see an example on how to index a complex JSON data set into Azure Search and perform a number of queries over this dataset at this [GitHub repo](https://github.com/liamca/AzureSearchComplexTypes).</span></span>

## <a name="next-step"></a><span data-ttu-id="af26b-149">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="af26b-149">Next step</span></span>
<span data-ttu-id="af26b-150">[Natív támogatást az összetett adattípusú szavazzon](https://feedback.azure.com/forums/263029-azure-search) az Azure Search UserVoice lapon, és adjon meg semmilyen további bemeneti szeretné, hogy a szolgáltatás megvalósítási kapcsolatban fontolja meg.</span><span class="sxs-lookup"><span data-stu-id="af26b-150">[Vote for native support for complex data types](https://feedback.azure.com/forums/263029-azure-search) on the Azure Search UserVoice page and provide any additional input that you’d like us to consider regarding feature implementation.</span></span> <span data-ttu-id="af26b-151">Is érhető el nekem közvetlenül a Twitteren: @liamca.</span><span class="sxs-lookup"><span data-stu-id="af26b-151">You can also reach out to me directly on Twitter at @liamca.</span></span>

