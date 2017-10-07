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
# <a name="how-toomodel-complex-data-types-in-azure-search"></a>Hogyan toomodel összetett adattípusokat az Azure Search
Külső adatkészletek használt toopopulate Azure Search-index néha hierarchikus vagy beágyazott alépítményeit, amely nem felosztása szépen táblázatos sorhalmazt tartalmaz. Példa ilyen struktúrák előfordulhat, hogy több helyről és telefonszámok tartalmazza az egy ügyfél több színek és méretek egyetlen termékváltozat, több szerzők egy könyv, és így tovább. A modellezési feltételeit, láthatja, ezen szerkezetek említett tooas *összetett adattípusú*, *összetett adattípusú*, *összetett adattípusok*, vagy *összesített adattípusok*, kevés a tooname.

Összetett adattípusú nem natív módon támogatja az Azure Search, de bevált megoldás tartalmaz egy kétlépéses folyamat egybesimítását hello struktúra és használata a **gyűjtemény** adattípus tooreconstitute hello belső struktúra. A következő cikkben ismertetett hello módszer lehetővé teszi a tartalom toobe hello keresett, jellemzőalapú, és szűrhetők.

## <a name="example-of-a-complex-data-structure"></a>Összetett adatszerkezet – példa
A szóban forgó adatok hello általában JSON vagy XML-dokumentumot, vagy egy NoSQL-tároló, például az Azure Cosmos DB elem található. Szerkezete hello challenge ered, amelyek több alárendelt elemeket a Keresés és a szűrt toobe.  A ábrázoló hello megoldás kiindulási pontként érvénybe hello számos olyan, az ügyfelek például JSON-dokumentum a következő:

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

Amíg hello mezők "id" nevű, "name" és "Vállalati" csak egyszerűen képezhető le egy az egyhez típusú mezői belül az Azure Search-index, hello "helyek" mező a helyek, hogy mindkét egy csoportja hely azonosítók, valamint a hely leírása tömböt tartalmaz. Fényében, hogy az Azure Search nem rendelkezik olyan adattípusú, amely támogatja ezt, igazolnia kell a különböző módokon toomodel Ez az Azure Search. 

> [!NOTE]
> Ezzel a módszerrel is blogbejegyzés ismerteti Kirk Evans által [indexelő DocumentDB az Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), amely jelzi, hogy egy technika az úgynevezett "simítási hello adatok", amely során egy nevű mező kellene lennie `locationsID` és `locationsDescription` amelyek [gyűjtemények](https://msdn.microsoft.com/library/azure/dn798938.aspx) (vagy karakterláncok).   
> 
> 

## <a name="part-1-flatten-hello-array-into-individual-fields"></a>1. rész: Hello tömb Egybesimítására egyes mezőkbe
toocreate az Azure Search-index, amelyre ez az adatkészlet egyes mezőket hello beágyazott tartóelemeket létrehozása: `locationsID` és `locationsDescription` adatok típussal rendelkező [gyűjtemények](https://msdn.microsoft.com/library/azure/dn798938.aspx) (vagy karakterláncok). A mezők hello be kellene index hello értéket "1" és "2" `locationsID` "3" & "4" hello a John Smith és hello értékek mezőjében `locationsID` Ilona Campbell mezőt.  

Az adatok Azure Search belül néz ki: 

![mintaadatokat, 2 sor](./media/search-howto-complex-data-types/sample-data.png)

## <a name="part-2-add-a-collection-field-in-hello-index-definition"></a>2. rész: Hello Indexdefiníció gyűjtemény mező felvétele
Hello mező definíciók hello a sémát indexeli, előfordulhat, hogy keresse meg a hasonló toothis példa.

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

## <a name="validate-search-behaviors-and-optionally-extend-hello-index"></a>Keresési viselkedések érvényesítése és kiterjeszti a hello index
Ha létrehozott hello index és betöltött hello adatokat, most tesztelheti hello megoldás tooverify keresési lekérdezés végrehajtásának hello adatkészlet. Minden egyes **gyűjtemény** mező lehet **kereshető**, **szűrhető** és **kategorizálható**. Meg kell tudni toorun lekérdezések például:

* Hello "Adventureworks központ" dolgozó összes személyek keresése.
* Megszámlálása egy otthoni Office dolgozó személyek hello száma.  
* Egy otthoni Office működéséhez hello személyek megjelenítése milyen többi iroda működnek együtt az egyes helyeken hello személyek számát.  

Ha ezzel a technikával egymástól esik akkor, ha egy keresést, hogy mind hello helyazonosító, valamint hello hely leírását toodo van szüksége. Példa:

* Minden személyek keresése, ahol egy otthoni Office rendelkeznek, és egy helyet azonosító, a 4.  

Ha ilyen kikeresi hello eredeti tartalom Emlékezzen vissza:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Azonban most, hogy a Microsoft hello adatok rendelkezik rétegekben külön mezők, tudunk semmilyen módon nem tudhatja, hogy ha hello Ilona Campbell otthoni Office túl`locationsID 3` vagy `locationsID 4`.  

Ebben az esetben toohandle másik mezőt, amely egyesíti hello adatok egyetlen gyűjtemény hello index határozza meg.  A példa kedvéért nevezzük fog ebben a mezőben `locationsCombined` és azt az egyes hello tartalom található egy `||` Bár választhat, amely úgy gondolja, hogy bármely elválasztó karakter a tartalom egyedi készletének lenne. Példa: 

![mintaadatokat, 2 sor elválasztóval](./media/search-howto-complex-data-types/sample-data-2.png)

Ennek segítségével `locationsCombined` mezőben azt most már képes még több lekérdezések, többek között:

* Egy szám irodában egy"otthoni" helyen lévő azonosítója "4" dolgozó személyek megjelenítése.  
* Egy otthoni irodában dolgozó személyek keresése helyen lévő "4" azonosítójú. 

## <a name="limitations"></a>Korlátozások
Ez a módszer akkor hasznos, ha több forgatókönyvet, de nincs minden esetben alkalmazható.  Példa:

1. Ha Ön az összetett adattípusú nem rendelkezik a statikus mezők halmaza alapján, és nincs módja toomap történt összes hello lehetséges típusokat tooa egyetlen mezőben. 
2. Hello beágyazott objektumok frissítése szükséges néhány további munkahelyi toodetermine pontosan mit kell toobe hello Azure Search-index frissítése

## <a name="sample-code"></a>Mintakód
Példa a hogyan tooindex összetett JSON-adatok beállításának Azure Search szolgáltatásba történő, és képes lekérdezések száma ehhez az adatkészlethez, ezzel [GitHub-tárház](https://github.com/liamca/AzureSearchComplexTypes).

## <a name="next-step"></a>Következő lépés
[Natív támogatást az összetett adattípusú szavazzon](https://feedback.azure.com/forums/263029-azure-search) hello Azure keresési UserVoice lapon, és minden további megadása, hogy szeretné funkció végrehajtására vonatkozó tooconsider. Közvetlenül a Twitteren: toome ki is elérhető @liamca.

