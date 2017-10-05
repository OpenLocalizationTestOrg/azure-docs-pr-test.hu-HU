---
title: "Az SQL-lekérdezések Azure Cosmos DB DocumentDB API-hoz |} Microsoft Docs"
description: "A Azure Cosmos DB SQL-szintaxis, adatbázis fogalmait és az SQL-lekérdezések megismerése. SQL Azure Cosmos adatbázis a JSON lekérdezésnyelvet is használja."
keywords: "SQL-szintaxis, sql-lekérdezést, az sql-lekérdezések, json lekérdezési nyelv, adatbázis fogalmait és az sql-lekérdezések, összesítő függvények"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: a73b4ab3-0786-42fd-b59b-555fce09db6e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: arramac
ms.openlocfilehash: 9b2b5668ef0552485a86f63a120b57c4623bfe35
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="sql-queries-for-azure-cosmos-db-documentdb-api"></a><span data-ttu-id="21a7c-105">Az SQL-lekérdezések Azure Cosmos DB DocumentDB API-hoz.</span><span class="sxs-lookup"><span data-stu-id="21a7c-105">SQL queries for Azure Cosmos DB DocumentDB API</span></span>
<span data-ttu-id="21a7c-106">A Microsoft Azure Cosmos DB támogatja a JSON lekérdezésnyelvet SQL (Structured Query Language) használatával dokumentumok lekérdezését.</span><span class="sxs-lookup"><span data-stu-id="21a7c-106">Microsoft Azure Cosmos DB supports querying documents using SQL (Structured Query Language) as a JSON query language.</span></span> <span data-ttu-id="21a7c-107">A cosmos DB valóban sémamentes.</span><span class="sxs-lookup"><span data-stu-id="21a7c-107">Cosmos DB is truly schema-free.</span></span> <span data-ttu-id="21a7c-108">A JSON-adatmodell, közvetlenül az adatbázis motorján belül az elkötelezettségének, címtár biztosít automatikus indexeléshez JSON-dokumentumok explicit séma vagy a másodlagos indexek létrehozása nélkül.</span><span class="sxs-lookup"><span data-stu-id="21a7c-108">By virtue of its commitment to the JSON data model directly within the database engine, it provides automatic indexing of JSON documents without requiring explicit schema or creation of secondary indexes.</span></span> 

<span data-ttu-id="21a7c-109">A lekérdezési nyelv a Cosmos DB tervezésekor két célok szem előtt tartásával volt:</span><span class="sxs-lookup"><span data-stu-id="21a7c-109">While designing the query language for Cosmos DB, we had two goals in mind:</span></span>

* <span data-ttu-id="21a7c-110">Helyett egy új JSON lekérdező nyelv inventing, akartunk támogatja az SQL.</span><span class="sxs-lookup"><span data-stu-id="21a7c-110">Instead of inventing a new JSON query language, we wanted to support SQL.</span></span> <span data-ttu-id="21a7c-111">Az SQL ismerős és a népszerű lekérdezés nyelveinek.</span><span class="sxs-lookup"><span data-stu-id="21a7c-111">SQL is one of the most familiar and popular query languages.</span></span> <span data-ttu-id="21a7c-112">Cosmos DB SQL formális programozási modellt biztosít a részletes lekérdezéseket JSON-dokumentumok keresztül.</span><span class="sxs-lookup"><span data-stu-id="21a7c-112">Cosmos DB SQL provides a formal programming model for rich queries over JSON documents.</span></span>
* <span data-ttu-id="21a7c-113">JSON-adatbázisként dokumentum is lehet futtatni az adatbázismotor közvetlenül a JavaScript azt kapcsolniuk a használni kívánt JavaScript programozási modell építkezve a lekérdezési nyelvhez.</span><span class="sxs-lookup"><span data-stu-id="21a7c-113">As a JSON document database capable of executing JavaScript directly in the database engine, we wanted to use JavaScript's programming model as the foundation for our query language.</span></span> <span data-ttu-id="21a7c-114">A DocumentDB API SQL JavaScript típusrendszernek, kifejezés kiértékelése és függvényhívások feltörték.</span><span class="sxs-lookup"><span data-stu-id="21a7c-114">The DocumentDB API SQL is rooted in JavaScript's type system, expression evaluation, and function invocation.</span></span> <span data-ttu-id="21a7c-115">Ez szolgálna természetes programozási modellt biztosít a leképezések relációs, hierarchikus navigációs JSON-dokumentumokat, automatikus illesztések, térbeli lekérdezéseket és teljesen egyéb funkciók között a JavaScript nyelven írt felhasználói függvény (UDF) meghívását.</span><span class="sxs-lookup"><span data-stu-id="21a7c-115">This in-turn provides a natural programming model for relational projections, hierarchical navigation across JSON documents, self joins, spatial queries, and invocation of user-defined functions (UDFs) written entirely in JavaScript, among other features.</span></span> 

<span data-ttu-id="21a7c-116">Biztosak vagyunk abban, hogy ezek a képességek csökkenti az alkalmazás és az adatbázis közötti súrlódás billentyűt, és fontosságúak a fejlesztést tesz lehetővé.</span><span class="sxs-lookup"><span data-stu-id="21a7c-116">We believe that these capabilities are key to reducing the friction between the application and the database and are crucial for developer productivity.</span></span>

<span data-ttu-id="21a7c-117">Azt javasoljuk, kezdeti lépések, amelyet figyeli az alábbi videót, ahol Aravind Ramachandran jeleníti meg, Cosmos DB tartozó lekérdezési képességeket, és látogasson el a [Tesztlekérdezéseket](http://www.documentdb.com/sql/demo), ahol Cosmos DB kipróbálásához és SQL-lekérdezések futtatása az adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="21a7c-117">We recommend getting started by watching the following video, where Aravind Ramachandran shows Cosmos DB's querying capabilities, and by visiting our [Query Playground](http://www.documentdb.com/sql/demo), where you can try out Cosmos DB and run SQL queries against our dataset.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/DataExposedQueryingDocumentDB/player]
> 
> 

<span data-ttu-id="21a7c-118">Ezt követően térjen vissza a cikkhez, ha először egy SQL-lekérdezés oktatóanyag, amely bemutatja, hogyan néhány egyszerű JSON-dokumentumokat és az SQL-parancsokat.</span><span class="sxs-lookup"><span data-stu-id="21a7c-118">Then, return to this article, where we start with a SQL query tutorial that walks you through some simple JSON documents and SQL commands.</span></span>

## <span data-ttu-id="21a7c-119"><a id="GettingStarted"></a>Ismerkedés az SQL-parancsokat az Cosmos-Adatbázisba</span><span class="sxs-lookup"><span data-stu-id="21a7c-119"><a id="GettingStarted"></a>Getting started with SQL commands in Cosmos DB</span></span>
<span data-ttu-id="21a7c-120">Munkahelyi Cosmos DB SQL megtekintéséhez lehetővé néhány egyszerű JSON-dokumentumok kezdődnie, és néhány egyszerű lekérdezéseket bízná.</span><span class="sxs-lookup"><span data-stu-id="21a7c-120">To see Cosmos DB SQL at work, let's begin with a few simple JSON documents and walk through some simple queries against it.</span></span> <span data-ttu-id="21a7c-121">Fontolja meg e két JSON-dokumentumok két családok kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="21a7c-121">Consider these two JSON documents about two families.</span></span> <span data-ttu-id="21a7c-122">A Cosmos DB azt nem kell minden sémákat, illetve másodlagos indexek explicit módon létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="21a7c-122">With Cosmos DB, we do not need to create any schemas or secondary indices explicitly.</span></span> <span data-ttu-id="21a7c-123">Egyszerűen kell beszúrni a JSON-dokumentumok Cosmos DB gyűjteménybe, és ezt követően lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="21a7c-123">We simply need to insert the JSON documents to a Cosmos DB collection and subsequently query.</span></span> <span data-ttu-id="21a7c-124">Itt egy egyszerű JSON tudunk dokumentum az Andersen családhoz, a szülők, gyermekek (és azok kedvtelésből), a címet, és a regisztrációs adatait.</span><span class="sxs-lookup"><span data-stu-id="21a7c-124">Here we have a simple JSON document for the Andersen family, the parents, children (and their pets), address, and registration information.</span></span> <span data-ttu-id="21a7c-125">A dokumentum rendelkezik karakterláncok, számok, a logikai, tömbök és beágyazott tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="21a7c-125">The document has strings, numbers, Booleans, arrays, and nested properties.</span></span> 

<span data-ttu-id="21a7c-126">**A dokumentum**</span><span class="sxs-lookup"><span data-stu-id="21a7c-126">**Document**</span></span>  

```JSON
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow", 
         "gender": "female", 
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

<span data-ttu-id="21a7c-127">Ez az egyetlen különbség – a második dokumentum `givenName` és `familyName` helyett használt `firstName` és `lastName`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-127">Here's a second document with one subtle difference – `givenName` and `familyName` are used instead of `firstName` and `lastName`.</span></span>

<span data-ttu-id="21a7c-128">**A dokumentum**</span><span class="sxs-lookup"><span data-stu-id="21a7c-128">**Document**</span></span>  

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

<span data-ttu-id="21a7c-129">Most próbáljon néhány lekérdezések írásában, ezeket az adatokat a kulcsfontosságú elemeit annak DocumentDB API SQL megismerését.</span><span class="sxs-lookup"><span data-stu-id="21a7c-129">Now let's try a few queries against this data to understand some of the key aspects of DocumentDB API SQL.</span></span> <span data-ttu-id="21a7c-130">Például a következő lekérdezés visszaad a Ha az azonosítót tartalmazó mezőt megegyezik `AndersenFamily`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-130">For example, the following query returns the documents where the id field matches `AndersenFamily`.</span></span> <span data-ttu-id="21a7c-131">Mivel ez egy `SELECT *`, a lekérdezés eredménye a teljes JSON-dokumentum:</span><span class="sxs-lookup"><span data-stu-id="21a7c-131">Since it's a `SELECT *`, the output of the query is the complete JSON document:</span></span>

<span data-ttu-id="21a7c-132">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-132">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="21a7c-133">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-133">**Results**</span></span>

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


<span data-ttu-id="21a7c-134">Most pedig nézzük meg az esetben, ha igazolnia kell formáznia egy másik alakzat JSON-kimenetét.</span><span class="sxs-lookup"><span data-stu-id="21a7c-134">Now consider the case where we need to reformat the JSON output in a different shape.</span></span> <span data-ttu-id="21a7c-135">Ez a lekérdezés egy új JSON-objektum nevét és a város, két kijelölt mezővel projektek, ha a cím város azonos nevű állapotként.</span><span class="sxs-lookup"><span data-stu-id="21a7c-135">This query projects a new JSON object with two selected fields, Name and City, when the address' city has the same name as the state.</span></span> <span data-ttu-id="21a7c-136">Ebben az esetben a "NY, NY" megegyezik.</span><span class="sxs-lookup"><span data-stu-id="21a7c-136">In this case, "NY, NY" matches.</span></span>

<span data-ttu-id="21a7c-137">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-137">**Query**</span></span>    

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

<span data-ttu-id="21a7c-138">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-138">**Results**</span></span>

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


<span data-ttu-id="21a7c-139">A következő lekérdezés gyermekek minden megadott nevét adja vissza a termékcsalád, amelynek azonosítója megegyezik `WakefieldFamily` a város tartózkodási helye alapján rendezve.</span><span class="sxs-lookup"><span data-stu-id="21a7c-139">The next query returns all the given names of children in the family whose id matches `WakefieldFamily` ordered by the city of residence.</span></span>

<span data-ttu-id="21a7c-140">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-140">**Query**</span></span>

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

<span data-ttu-id="21a7c-141">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-141">**Results**</span></span>

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


<span data-ttu-id="21a7c-142">Szeretnénk felhívja a figyelmet a Cosmos DB lekérdezési nyelv eddig is láttuk példákból néhány fontos aspektusait:</span><span class="sxs-lookup"><span data-stu-id="21a7c-142">We would like to draw attention to a few noteworthy aspects of the Cosmos DB query language through the examples we've seen so far:</span></span>  

* <span data-ttu-id="21a7c-143">Mivel a DocumentDB API SQL JSON értékek működéséről, alakú sorok és oszlopok helyett entitások fa foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="21a7c-143">Since DocumentDB API SQL works on JSON values, it deals with tree shaped entities instead of rows and columns.</span></span> <span data-ttu-id="21a7c-144">Ezért a nyelv lehetővé teszi, hogy tekintse meg a fa bármilyen tetszőleges mélységben csomópontok például `Node1.Node2.Node3…..Nodem`, akárcsak a két rész hivatkozását hivatkozó relációs SQL `<table>.<column>`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-144">Therefore, the language lets you refer to nodes of the tree at any arbitrary depth, like `Node1.Node2.Node3…..Nodem`, similar to relational SQL referring to the two part reference of `<table>.<column>`.</span></span>   
* <span data-ttu-id="21a7c-145">A strukturált lekérdezésinyelv séma nélküli adatokkal dolgozik.</span><span class="sxs-lookup"><span data-stu-id="21a7c-145">The structured query language works with schema-less data.</span></span> <span data-ttu-id="21a7c-146">Ezért a típusrendszernek hozzá dinamikusan kell kötni.</span><span class="sxs-lookup"><span data-stu-id="21a7c-146">Therefore, the type system needs to be bound dynamically.</span></span> <span data-ttu-id="21a7c-147">Ugyanabban a kifejezésben sikerült yield különböző típusaihoz különböző dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="21a7c-147">The same expression could yield different types on different documents.</span></span> <span data-ttu-id="21a7c-148">A lekérdezés eredménye egy érvényes JSON-érték, de nem biztos, hogy a rögzített sémájába lehet.</span><span class="sxs-lookup"><span data-stu-id="21a7c-148">The result of a query is a valid JSON value, but is not guaranteed to be of a fixed schema.</span></span>  
* <span data-ttu-id="21a7c-149">Cosmos DB csak szigorú JSON-dokumentumokat támogat.</span><span class="sxs-lookup"><span data-stu-id="21a7c-149">Cosmos DB only supports strict JSON documents.</span></span> <span data-ttu-id="21a7c-150">Ez azt jelenti, hogy a rendszert és a kifejezések csak JSON típusok kezelésére korlátozódnak.</span><span class="sxs-lookup"><span data-stu-id="21a7c-150">This means the type system and expressions are restricted to deal only with JSON types.</span></span> <span data-ttu-id="21a7c-151">Tekintse meg a [JSON-specifikáció](http://www.json.org/) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="21a7c-151">Refer to the [JSON specification](http://www.json.org/) for more details.</span></span>  
* <span data-ttu-id="21a7c-152">A Cosmos DB gyűjtemény egy olyan sémamentes tároló JSON-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="21a7c-152">A Cosmos DB collection is a schema-free container of JSON documents.</span></span> <span data-ttu-id="21a7c-153">Tartalmazási, és nem a primary key és idegen kulcs kapcsolatokat a rendszer implicit módon rögzíti a kapcsolatokat, az adatok entitások belül, és egy gyűjtemény dokumentumok között.</span><span class="sxs-lookup"><span data-stu-id="21a7c-153">The relations in data entities within and across documents in a collection are implicitly captured by containment and not by primary key and foreign key relations.</span></span> <span data-ttu-id="21a7c-154">Ez egy fontos eleme érdemes a jelen cikkben ismertetett intra-dokumentum illesztések alapján mutat.</span><span class="sxs-lookup"><span data-stu-id="21a7c-154">This is an important aspect worth pointing out in light of the intra-document joins discussed later in this article.</span></span>

## <span data-ttu-id="21a7c-155"><a id="Indexing"></a>A cosmos DB indexelő</span><span class="sxs-lookup"><span data-stu-id="21a7c-155"><a id="Indexing"></a> Cosmos DB indexing</span></span>
<span data-ttu-id="21a7c-156">Ahhoz, hogy feltölti a DocumentDB API SQL-szintaxis, az indexelési kialakítást a Cosmos DB felfedezése érdemes.</span><span class="sxs-lookup"><span data-stu-id="21a7c-156">Before we get into the DocumentDB API SQL syntax, it is worth exploring the indexing design in Cosmos DB.</span></span> 

<span data-ttu-id="21a7c-157">Adatbázis indexek célja a különböző űrlapok és alakzatok lekérdezések kiszolgálására minimális erőforrás-felhasználás (például CPU és a bemeneti/kimeneti) ugyanakkor biztosítható a jó teljesítmény és kis késleltetése.</span><span class="sxs-lookup"><span data-stu-id="21a7c-157">The purpose of database indexes is to serve queries in their various forms and shapes with minimum resource consumption (like CPU and input/output) while providing good throughput and low latency.</span></span> <span data-ttu-id="21a7c-158">Adatbázis lekérdezése a megfelelő index a választott gyakran, mennyi tervezést és kísérletezés igényel.</span><span class="sxs-lookup"><span data-stu-id="21a7c-158">Often, the choice of the right index for querying a database requires much planning and experimentation.</span></span> <span data-ttu-id="21a7c-159">Ezt a módszert használja az adatbázisok séma nélküli, ahol az adatok nem felelnek meg a szigorú séma, és gyorsan fejlődésének kihívást jelent.</span><span class="sxs-lookup"><span data-stu-id="21a7c-159">This approach poses a challenge for schema-less databases where the data doesn’t conform to a strict schema and evolves rapidly.</span></span> 

<span data-ttu-id="21a7c-160">Ezért a Cosmos DB indexelési alrendszer tervezésekor be van állítva a következő célok:</span><span class="sxs-lookup"><span data-stu-id="21a7c-160">Therefore, when we designed the Cosmos DB indexing subsystem, we set the following goals:</span></span>

* <span data-ttu-id="21a7c-161">Indexeljük a dokumentumokat anélkül, hogy a séma: az indexelő alrendszer nem kér a sémaadatok és bármely séma feltételezéseket a dokumentumok győződjön.</span><span class="sxs-lookup"><span data-stu-id="21a7c-161">Index documents without requiring schema: The indexing subsystem does not require any schema information or make any assumptions about schema of the documents.</span></span> 
* <span data-ttu-id="21a7c-162">Hatékony és gazdag hierarchikus és relációs lekérdezések támogatása: az index támogatja a Cosmos DB lekérdezési nyelv hatékonyan, beleértve a hierarchikus és relációs leképezések támogatását.</span><span class="sxs-lookup"><span data-stu-id="21a7c-162">Support for efficient, rich hierarchical, and relational queries: The index supports the Cosmos DB query language efficiently, including support for hierarchical and relational projections.</span></span>
* <span data-ttu-id="21a7c-163">Egységes lekérdezések in face of írások tartós kötet támogatása: nagy írási átviteli munkaterhelések egységes lekérdezések esetén az index frissítése Növekményesen, hatékony és online állásuk tartós mennyiségű írási műveleteket.</span><span class="sxs-lookup"><span data-stu-id="21a7c-163">Support for consistent queries in face of a sustained volume of writes: For high write throughput workloads with consistent queries, the index is updated incrementally, efficiently, and online in the face of a sustained volume of writes.</span></span> <span data-ttu-id="21a7c-164">A konzisztens index frissítése különösen fontos a lekérdezések végrehajtása az konzisztencia szintjén, amelyben a felhasználó konfigurálta a dokumentum szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="21a7c-164">The consistent index update is crucial to serve the queries at the consistency level in which the user configured the document service.</span></span>
* <span data-ttu-id="21a7c-165">Több vállalat kiszolgálása támogatása: a foglalásalapú modellben megadott erőforrás irányításhoz bérlők között, index frissítései a keret rendszererőforrást (Processzor, memória és i/o műveletek száma másodpercenként) replika felosztott belül.</span><span class="sxs-lookup"><span data-stu-id="21a7c-165">Support for multi-tenancy: Given the reservation-based model for resource governance across tenants, index updates are performed within the budget of system resources (CPU, memory, and input/output operations per second) allocated per replica.</span></span> 
* <span data-ttu-id="21a7c-166">Tároló-hatékonyságot biztosít: költséghatékonyság, a lemezen terheléssel jár az index nem kötött és előre jelezhető.</span><span class="sxs-lookup"><span data-stu-id="21a7c-166">Storage efficiency: For cost effectiveness, the on-disk storage overhead of the index is bounded and predictable.</span></span> <span data-ttu-id="21a7c-167">Ez elengedhetetlen, mert Cosmos DB lehetővé teszi, hogy a fejlesztő költség-alapú mellékhatásokkal index terhelés alapján a lekérdezési teljesítmény közötti győződjön.</span><span class="sxs-lookup"><span data-stu-id="21a7c-167">This is crucial because Cosmos DB allows the developer to make cost-based tradeoffs between index overhead in relation to the query performance.</span></span>  

<span data-ttu-id="21a7c-168">Tekintse meg a [Azure Cosmos DB samples](https://github.com/Azure/azure-documentdb-net) MSDN minták bemutatja, hogyan konfigurálja az indexelési házirendet egy gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="21a7c-168">Refer to the [Azure Cosmos DB samples](https://github.com/Azure/azure-documentdb-net) on MSDN for samples showing how to configure the indexing policy for a collection.</span></span> <span data-ttu-id="21a7c-169">Most folytassuk az Azure Cosmos adatbázis SQL-szintaxis részleteinek.</span><span class="sxs-lookup"><span data-stu-id="21a7c-169">Let’s now get into the details of the Azure Cosmos DB SQL syntax.</span></span>

## <span data-ttu-id="21a7c-170"><a id="Basics"></a>Az Azure Cosmos adatbázis SQL-lekérdezést alapjai</span><span class="sxs-lookup"><span data-stu-id="21a7c-170"><a id="Basics"></a>Basics of an Azure Cosmos DB SQL query</span></span>
<span data-ttu-id="21a7c-171">Minden egyes lekérdezés SELECT záradékában és választható FROM áll és a WHERE záradék / ANSI SQL szabványoknak.</span><span class="sxs-lookup"><span data-stu-id="21a7c-171">Every query consists of a SELECT clause and optional FROM and WHERE clauses per ANSI-SQL standards.</span></span> <span data-ttu-id="21a7c-172">Általában minden lekérdezéshez a FROM záradékban lévő adatforrás megjelenik a listán.</span><span class="sxs-lookup"><span data-stu-id="21a7c-172">Typically, for each query, the source in the FROM clause is enumerated.</span></span> <span data-ttu-id="21a7c-173">Ezután a WHERE záradékban a szűrő alkalmazása a forrás JSON-dokumentumok részhalmazának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-173">Then the filter in the WHERE clause is applied on the source to retrieve a subset of JSON documents.</span></span> <span data-ttu-id="21a7c-174">Végezetül a SELECT záradékban szolgál a kért JSON értékeit a kiválasztási listán.</span><span class="sxs-lookup"><span data-stu-id="21a7c-174">Finally, the SELECT clause is used to project the requested JSON values in the select list.</span></span>

    SELECT <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <span data-ttu-id="21a7c-175"><a id="FromClause"></a>FROM záradékban</span><span class="sxs-lookup"><span data-stu-id="21a7c-175"><a id="FromClause"></a>FROM clause</span></span>
<span data-ttu-id="21a7c-176">A `FROM <from_specification>` záradék használata nem kötelező, kivéve, ha a forrás szűrt vagy projekció a lekérdezésben később.</span><span class="sxs-lookup"><span data-stu-id="21a7c-176">The `FROM <from_specification>` clause is optional unless the source is filtered or projected later in the query.</span></span> <span data-ttu-id="21a7c-177">Ehhez a záradékhoz célja, adja meg az adatforrás, amelyre a lekérdezés működnie kell.</span><span class="sxs-lookup"><span data-stu-id="21a7c-177">The purpose of this clause is to specify the data source upon which the query must operate.</span></span> <span data-ttu-id="21a7c-178">Az egész gyűjteményre általában a forrás, de ehelyett egy adhat meg a gyűjtemény egy részét.</span><span class="sxs-lookup"><span data-stu-id="21a7c-178">Commonly the whole collection is the source, but one can specify a subset of the collection instead.</span></span> 

<span data-ttu-id="21a7c-179">A lekérdezés, például `SELECT * FROM Families` azt jelzi, hogy a teljes családok gyűjteményt a forrás, amelyben enumerálása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-179">A query like `SELECT * FROM Families` indicates that the entire Families collection is the source over which to enumerate.</span></span> <span data-ttu-id="21a7c-180">Egy legfelső szintű speciális azonosítója segítségével határoz meg a gyűjtemény neve helyett a gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-180">A special identifier ROOT can be used to represent the collection instead of using the collection name.</span></span> <span data-ttu-id="21a7c-181">Az alábbi lista tartalmazza a szabályokat, amelyek lekérdezésenként lépnek érvénybe:</span><span class="sxs-lookup"><span data-stu-id="21a7c-181">The following list contains the rules that are enforced per query:</span></span>

* <span data-ttu-id="21a7c-182">A gyűjtemény akkor jelölhető meg aliasként, például a `SELECT f.id FROM Families AS f` vagy egyszerűen `SELECT f.id FROM Families f`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-182">The collection can be aliased, such as `SELECT f.id FROM Families AS f` or simply `SELECT f.id FROM Families f`.</span></span> <span data-ttu-id="21a7c-183">Itt `f` megegyezik a `Families`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-183">Here `f` is the equivalent of `Families`.</span></span> <span data-ttu-id="21a7c-184">`AS`egy nem kötelező kulcsszót alias azonosító érték.</span><span class="sxs-lookup"><span data-stu-id="21a7c-184">`AS` is an optional keyword to alias the identifier.</span></span>
* <span data-ttu-id="21a7c-185">Egyszer aliasnevet, az eredeti adatforrás nem köthető.</span><span class="sxs-lookup"><span data-stu-id="21a7c-185">Once aliased, the original source cannot be bound.</span></span> <span data-ttu-id="21a7c-186">Például `SELECT Families.id FROM Families f` szintaktikailag hibás, mert "Családokat" azonosítóját már nem lehet feloldani.</span><span class="sxs-lookup"><span data-stu-id="21a7c-186">For example, `SELECT Families.id FROM Families f` is syntactically invalid since the identifier "Families" cannot be resolved anymore.</span></span>
* <span data-ttu-id="21a7c-187">Lehet, hogy teljesen minősített mutató hivatkozás fog igénylő összes tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="21a7c-187">All properties that need to be referenced must be fully qualified.</span></span> <span data-ttu-id="21a7c-188">Szigorú séma való hiányában ez kényszerítése egyetlen nem egyértelmű kötést elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="21a7c-188">In the absence of strict schema adherence, this is enforced to avoid any ambiguous bindings.</span></span> <span data-ttu-id="21a7c-189">Ezért `SELECT id FROM Families f` szintaktikailag óta a tulajdonság nem `id` nincs kötve.</span><span class="sxs-lookup"><span data-stu-id="21a7c-189">Therefore, `SELECT id FROM Families f` is syntactically invalid since the property `id` is not bound.</span></span>

### <a name="subdocuments"></a><span data-ttu-id="21a7c-190">Aldokumentumok</span><span class="sxs-lookup"><span data-stu-id="21a7c-190">Subdocuments</span></span>
<span data-ttu-id="21a7c-191">A forrás kisebb részhalmazát is lehet korlátozni.</span><span class="sxs-lookup"><span data-stu-id="21a7c-191">The source can also be reduced to a smaller subset.</span></span> <span data-ttu-id="21a7c-192">Például számbavétele minden a dokumentumban csak egy részfája, hogy a subroot majd válhat a forráskiszolgálón, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="21a7c-192">For instance, to enumerating only a subtree in each document, the subroot could then become the source, as shown in the following example:</span></span>

<span data-ttu-id="21a7c-193">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-193">**Query**</span></span>

    SELECT * 
    FROM Families.children

<span data-ttu-id="21a7c-194">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-194">**Results**</span></span>  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

<span data-ttu-id="21a7c-195">A fenti példa egy tömb forrásaként használható, amíg az objektum is lehet alkalmazni a forrásaként, amely az alábbi példában is látható: a lekérdezés eredménye, hogy minden érvényes JSON-érték (nem nincs definiálva), amelyek megtalálhatók a forrás tekinthető.</span><span class="sxs-lookup"><span data-stu-id="21a7c-195">While the above example used an array as the source, an object could also be used as the source, which is what's shown in the following example: Any valid JSON value (not undefined) that can be found in the source is considered for inclusion in the result of the query.</span></span> <span data-ttu-id="21a7c-196">Ha egyes termékcsaládok nem rendelkezik egy `address.state` érték, a lekérdezés eredményében ki vannak zárva.</span><span class="sxs-lookup"><span data-stu-id="21a7c-196">If some families don’t have an `address.state` value, they are excluded in the query result.</span></span>

<span data-ttu-id="21a7c-197">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-197">**Query**</span></span>

    SELECT * 
    FROM Families.address.state

<span data-ttu-id="21a7c-198">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-198">**Results**</span></span>

    [
      "WA", 
      "NY"
    ]


## <span data-ttu-id="21a7c-199"><a id="WhereClause"></a>A WHERE záradék</span><span class="sxs-lookup"><span data-stu-id="21a7c-199"><a id="WhereClause"></a>WHERE clause</span></span>
<span data-ttu-id="21a7c-200">A WHERE záradékban (**`WHERE <filter_condition>`**) megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="21a7c-200">The WHERE clause (**`WHERE <filter_condition>`**) is optional.</span></span> <span data-ttu-id="21a7c-201">Azt adja meg a feltételeket, amelyek a forrás által biztosított a JSON-dokumentumok meg kell felelnie ahhoz, hogy a tartalmazzák a eredménye.</span><span class="sxs-lookup"><span data-stu-id="21a7c-201">It specifies the condition(s) that the JSON documents provided by the source must satisfy in order to be included as part of the result.</span></span> <span data-ttu-id="21a7c-202">Bármely JSON-dokumentum ki kell értékelnie, hogy a megadott feltételeknek, a "true", az eredmény figyelembe kell venni.</span><span class="sxs-lookup"><span data-stu-id="21a7c-202">Any JSON document must evaluate the specified conditions to "true" to be considered for the result.</span></span> <span data-ttu-id="21a7c-203">A WHERE záradékban a index réteg használják annak meghatározására, a forrás azt jelzi, hogy az eredmény része lehet abszolút legkisebb részhalmaza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-203">The WHERE clause is used by the index layer in order to determine the absolute smallest subset of source documents that can be part of the result.</span></span> 

<span data-ttu-id="21a7c-204">A következő lekérdezés kéri a name tulajdonság, amelynek értéke tartalmazó dokumentumok `AndersenFamily`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-204">The following query requests documents that contain a name property whose value is `AndersenFamily`.</span></span> <span data-ttu-id="21a7c-205">Bármely más dokumentum, amely nem rendelkezik name tulajdonsággal, vagy ha az érték nem egyezik `AndersenFamily` ki van zárva.</span><span class="sxs-lookup"><span data-stu-id="21a7c-205">Any other document that does not have a name property, or where the value does not match `AndersenFamily` is excluded.</span></span> 

<span data-ttu-id="21a7c-206">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-206">**Query**</span></span>

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="21a7c-207">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-207">**Results**</span></span>

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


<span data-ttu-id="21a7c-208">Az előző példából kiderült, egy egyszerű egyenlőség lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="21a7c-208">The previous example showed a simple equality query.</span></span> <span data-ttu-id="21a7c-209">A DocumentDB API SQL számos skaláris kifejezést.</span><span class="sxs-lookup"><span data-stu-id="21a7c-209">DocumentDB API SQL also supports a variety of scalar expressions.</span></span> <span data-ttu-id="21a7c-210">A leggyakrabban használt olyan bináris és egyoperandusú kifejezés.</span><span class="sxs-lookup"><span data-stu-id="21a7c-210">The most commonly used are binary and unary expressions.</span></span> <span data-ttu-id="21a7c-211">A forrás JSON-objektumból tulajdonsághivatkozást egyaránt érvényes kifejezések.</span><span class="sxs-lookup"><span data-stu-id="21a7c-211">Property references from the source JSON object are also valid expressions.</span></span> 

<span data-ttu-id="21a7c-212">A következő bináris operátor jelenleg támogatott, és a következő példákban látható módon a lekérdezésekben használt:</span><span class="sxs-lookup"><span data-stu-id="21a7c-212">The following binary operators are currently supported and can be used in queries as shown in the following examples:</span></span>  

<table>
<tr>
<td><span data-ttu-id="21a7c-213">Aritmetikai</span><span class="sxs-lookup"><span data-stu-id="21a7c-213">Arithmetic</span></span></td>    
<td><span data-ttu-id="21a7c-214">+,-,*,/,%</span><span class="sxs-lookup"><span data-stu-id="21a7c-214">+,-,*,/,%</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="21a7c-215">Bitenként</span><span class="sxs-lookup"><span data-stu-id="21a7c-215">Bitwise</span></span></td>    
<td><span data-ttu-id="21a7c-216">|}, &, ^, <<>>,, >>> (nulla-Kitöltés jobbra tolást)</span><span class="sxs-lookup"><span data-stu-id="21a7c-216">|, &, ^, <<, >>, >>> (zero-fill right shift)</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="21a7c-217">Logikai</span><span class="sxs-lookup"><span data-stu-id="21a7c-217">Logical</span></span></td>
<td><span data-ttu-id="21a7c-218">ÉS, VAGY SEM</span><span class="sxs-lookup"><span data-stu-id="21a7c-218">AND, OR, NOT</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="21a7c-219">Összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="21a7c-219">Comparison</span></span></td>    
<td><span data-ttu-id="21a7c-220">=, !=, &lt;, &gt;, &lt;=, &gt;=, <></span><span class="sxs-lookup"><span data-stu-id="21a7c-220">=, !=, &lt;, &gt;, &lt;=, &gt;=, <></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="21a7c-221">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="21a7c-221">String</span></span></td>    
<td><span data-ttu-id="21a7c-222">|| (összefűzésére)</span><span class="sxs-lookup"><span data-stu-id="21a7c-222">|| (concatenate)</span></span></td>
</tr>
</table>  


<span data-ttu-id="21a7c-223">Vessen egy pillantást néhány lekérdezést a bináris operátorok használatával.</span><span class="sxs-lookup"><span data-stu-id="21a7c-223">Let’s take a look at some queries using binary operators.</span></span>

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


<span data-ttu-id="21a7c-224">Az egyoperandusú operátorokat +,-, ~ és nem is támogatottak, és használhatók lekérdezéseken belül a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="21a7c-224">The unary operators +,-, ~ and NOT are also supported, and can be used inside queries as shown in the following example:</span></span>

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



<span data-ttu-id="21a7c-225">Bináris és egyoperandusú operátorok mellett tulajdonsághivatkozást is használhatók.</span><span class="sxs-lookup"><span data-stu-id="21a7c-225">In addition to binary and unary operators, property references are also allowed.</span></span> <span data-ttu-id="21a7c-226">Például `SELECT * FROM Families f WHERE f.isRegistered` adja vissza a tulajdonságot tartalmazó JSON-dokumentum `isRegistered` ahol a tulajdonság értéke megegyezik a JSON `true` érték.</span><span class="sxs-lookup"><span data-stu-id="21a7c-226">For example, `SELECT * FROM Families f WHERE f.isRegistered` returns the JSON document containing the property `isRegistered` where the property's value is equal to the JSON `true` value.</span></span> <span data-ttu-id="21a7c-227">Egyéb értékek (false, null, nem definiált, `<number>`, `<string>`, `<object>`, `<array>`stb) kivételével az eredményből forrásdokumentum vezet.</span><span class="sxs-lookup"><span data-stu-id="21a7c-227">Any other values (false, null, Undefined, `<number>`, `<string>`, `<object>`, `<array>`, etc.) leads to the source document being excluded from the result.</span></span> 

### <a name="equality-and-comparison-operators"></a><span data-ttu-id="21a7c-228">Egyenlőség és összehasonlító operátorok</span><span class="sxs-lookup"><span data-stu-id="21a7c-228">Equality and comparison operators</span></span>
<span data-ttu-id="21a7c-229">A következő táblázat egyenlőségi összehasonlítás eredménye a DocumentDB SQL-API bármely két JSON-típusok között.</span><span class="sxs-lookup"><span data-stu-id="21a7c-229">The following table shows the result of equality comparisons in DocumentDB API SQL between any two JSON types.</span></span>

<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top"><span data-ttu-id="21a7c-230">
            <strong>Op</strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-230">
            <strong>Op</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="21a7c-231">
            <strong>Nincs definiálva</strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-231">
            <strong>Undefined</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="21a7c-232">
            <strong>NULL értékű</strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-232">
            <strong>Null</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="21a7c-233">
            <strong>Logikai érték</strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-233">
            <strong>Boolean</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="21a7c-234">
            <strong>Szám</strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-234">
            <strong>Number</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="21a7c-235">
            <strong>Karakterlánc</strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-235">
            <strong>String</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="21a7c-236">
            <strong>Objektum</strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-236">
            <strong>Object</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="21a7c-237">
            <strong>A tömb</strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-237">
            <strong>Array</strong>
         </span></span></td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="21a7c-238">
            <strong>Nincs definiálva<strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-238">
            <strong>Undefined<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="21a7c-239">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-239">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-240">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-240">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-241">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-241">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-242">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-242">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-243">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-243">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-244">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-244">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-245">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-245">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="21a7c-246">
            <strong>NULL értékű<strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-246">
            <strong>Null<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="21a7c-247">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-247">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="21a7c-248">
            <strong>OKÉ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-248">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="21a7c-249">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-249">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-250">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-250">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-251">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-251">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-252">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-252">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-253">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-253">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="21a7c-254">
            <strong>Logikai érték<strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-254">
            <strong>Boolean<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="21a7c-255">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-255">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-256">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-256">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="21a7c-257">
            <strong>OKÉ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-257">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="21a7c-258">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-258">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-259">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-259">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-260">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-260">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-261">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-261">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="21a7c-262">
            <strong>Szám<strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-262">
            <strong>Number<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="21a7c-263">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-263">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-264">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-264">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-265">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-265">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="21a7c-266">
            <strong>OKÉ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-266">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="21a7c-267">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-267">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-268">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-268">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-269">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-269">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="21a7c-270">
            <strong>Karakterlánc<strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-270">
            <strong>String<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="21a7c-271">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-271">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-272">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-272">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-273">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-273">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-274">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-274">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="21a7c-275">
            <strong>OKÉ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-275">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="21a7c-276">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-276">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-277">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-277">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="21a7c-278">
            <strong>Objektum<strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-278">
            <strong>Object<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="21a7c-279">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-279">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-280">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-280">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-281">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-281">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-282">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-282">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-283">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-283">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="21a7c-284">
            <strong>OKÉ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-284">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="21a7c-285">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-285">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="21a7c-286">
            <strong>A tömb<strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-286">
            <strong>Array<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="21a7c-287">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-287">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-288">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-288">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-289">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-289">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-290">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-290">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-291">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-291">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="21a7c-292">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-292">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="21a7c-293">
            <strong>OKÉ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="21a7c-293">
            <strong>OK</strong>
         </span></span></td>
      </tr>
   </tbody>
</table>

<span data-ttu-id="21a7c-294">Más összehasonlító operátorok többek között a >, > =,! =, < és < =, az alábbi szabályok vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="21a7c-294">For other comparison operators such as >, >=, !=, < and <=, the following rules apply:</span></span>   

* <span data-ttu-id="21a7c-295">Összehasonlítás típusok között meghatározatlan eredményez.</span><span class="sxs-lookup"><span data-stu-id="21a7c-295">Comparison across types results in Undefined.</span></span>
* <span data-ttu-id="21a7c-296">Két objektum vagy két összehasonlítása tömbállandó meghatározatlan eredményez.</span><span class="sxs-lookup"><span data-stu-id="21a7c-296">Comparison between two objects or two arrays results in Undefined.</span></span>   

<span data-ttu-id="21a7c-297">Ha a szűrő skaláris kifejezés eredménye nincs definiálva, a megfelelő dokumentum nem szerepel az eredmény, mert meghatározatlan logikailag nem egyenlő a "true"értékre.</span><span class="sxs-lookup"><span data-stu-id="21a7c-297">If the result of the scalar expression in the filter is Undefined, the corresponding document would not be included in the result, since Undefined doesn't logically equate to "true".</span></span>

### <a name="between-keyword"></a><span data-ttu-id="21a7c-298">Kulcsszó között</span><span class="sxs-lookup"><span data-stu-id="21a7c-298">BETWEEN keyword</span></span>
<span data-ttu-id="21a7c-299">A BETWEEN kulcsszó használatával express tartományok értékek például ANSI SQL-lekérdezéseket is.</span><span class="sxs-lookup"><span data-stu-id="21a7c-299">You can also use the BETWEEN keyword to express queries against ranges of values like in ANSI SQL.</span></span> <span data-ttu-id="21a7c-300">KÖZÖTTI használható karakterlánc vagy szám ellen.</span><span class="sxs-lookup"><span data-stu-id="21a7c-300">BETWEEN can be used against strings or numbers.</span></span>

<span data-ttu-id="21a7c-301">Például a lekérdezés által visszaadott összes családba tartozó dokumentumok, amelyben az első gyermek osztályú 1-5 (mind a két szélsőértéket beleértve) közé esik.</span><span class="sxs-lookup"><span data-stu-id="21a7c-301">For example, this query returns all family documents in which the first child's grade is between 1-5 (both inclusive).</span></span> 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

<span data-ttu-id="21a7c-302">Eltérően ANSI-SQL, használhatja a BETWEEN záradék a következő példában például a FROM záradékban.</span><span class="sxs-lookup"><span data-stu-id="21a7c-302">Unlike in ANSI-SQL, you can also use the BETWEEN clause in the FROM clause like in the following example.</span></span>

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

<span data-ttu-id="21a7c-303">A lekérdezés végrehajtása gyorsabb ne felejtse el elleni bármely numerikus tulajdonságok/elérési utakat a BETWEEN záradék a szűrt index Tartománytípus használó indexelési házirend létrehozása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-303">For faster query execution times, remember to create an indexing policy that uses a range index type against any numeric properties/paths that are filtered in the BETWEEN clause.</span></span> 

<span data-ttu-id="21a7c-304">A fő különbség a DocumentDB API és ANSI SQL BETWEEN használata között, hogy akkor is express vegyes típusú tulajdonságokhoz lekérdezések – például lehetséges, hogy "osztály" [5] szám lehet bizonyos dokumentumok és mások számára ("grade4") tartalmazó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="21a7c-304">The main difference between using BETWEEN in DocumentDB API and ANSI SQL is that you can express range queries against properties of mixed types – for example, you might have "grade" be a number (5) in some documents and strings in others ("grade4").</span></span> <span data-ttu-id="21a7c-305">Ezekben az esetekben például a JavaScript, a "nem definiált" két különböző típusú eredményt, és a dokumentum összehasonlítása a rendszer kihagyja.</span><span class="sxs-lookup"><span data-stu-id="21a7c-305">In these cases, like in JavaScript, a comparison between two different types results in "undefined", and the document will be skipped.</span></span>

### <a name="logical-and-or-and-not-operators"></a><span data-ttu-id="21a7c-306">Logikai (AND, OR, és nem) operátorok</span><span class="sxs-lookup"><span data-stu-id="21a7c-306">Logical (AND, OR and NOT) operators</span></span>
<span data-ttu-id="21a7c-307">Logikai operátorok működhet a logikai értékek.</span><span class="sxs-lookup"><span data-stu-id="21a7c-307">Logical operators operate on Boolean values.</span></span> <span data-ttu-id="21a7c-308">Ezen operátorok logikai igazság táblázatokban az alábbi táblázatban láthatók.</span><span class="sxs-lookup"><span data-stu-id="21a7c-308">The logical truth tables for these operators are shown in the following tables.</span></span>

| <span data-ttu-id="21a7c-309">VAGY</span><span class="sxs-lookup"><span data-stu-id="21a7c-309">OR</span></span> | <span data-ttu-id="21a7c-310">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="21a7c-310">True</span></span> | <span data-ttu-id="21a7c-311">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="21a7c-311">False</span></span> | <span data-ttu-id="21a7c-312">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-312">Undefined</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="21a7c-313">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="21a7c-313">True</span></span> |<span data-ttu-id="21a7c-314">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="21a7c-314">True</span></span> |<span data-ttu-id="21a7c-315">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="21a7c-315">True</span></span> |<span data-ttu-id="21a7c-316">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="21a7c-316">True</span></span> |
| <span data-ttu-id="21a7c-317">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="21a7c-317">False</span></span> |<span data-ttu-id="21a7c-318">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="21a7c-318">True</span></span> |<span data-ttu-id="21a7c-319">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="21a7c-319">False</span></span> |<span data-ttu-id="21a7c-320">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-320">Undefined</span></span> |
| <span data-ttu-id="21a7c-321">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-321">Undefined</span></span> |<span data-ttu-id="21a7c-322">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="21a7c-322">True</span></span> |<span data-ttu-id="21a7c-323">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-323">Undefined</span></span> |<span data-ttu-id="21a7c-324">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-324">Undefined</span></span> |

| <span data-ttu-id="21a7c-325">ÉS</span><span class="sxs-lookup"><span data-stu-id="21a7c-325">AND</span></span> | <span data-ttu-id="21a7c-326">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="21a7c-326">True</span></span> | <span data-ttu-id="21a7c-327">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="21a7c-327">False</span></span> | <span data-ttu-id="21a7c-328">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-328">Undefined</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="21a7c-329">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="21a7c-329">True</span></span> |<span data-ttu-id="21a7c-330">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="21a7c-330">True</span></span> |<span data-ttu-id="21a7c-331">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="21a7c-331">False</span></span> |<span data-ttu-id="21a7c-332">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-332">Undefined</span></span> |
| <span data-ttu-id="21a7c-333">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="21a7c-333">False</span></span> |<span data-ttu-id="21a7c-334">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="21a7c-334">False</span></span> |<span data-ttu-id="21a7c-335">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="21a7c-335">False</span></span> |<span data-ttu-id="21a7c-336">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="21a7c-336">False</span></span> |
| <span data-ttu-id="21a7c-337">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-337">Undefined</span></span> |<span data-ttu-id="21a7c-338">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-338">Undefined</span></span> |<span data-ttu-id="21a7c-339">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="21a7c-339">False</span></span> |<span data-ttu-id="21a7c-340">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-340">Undefined</span></span> |

| <span data-ttu-id="21a7c-341">NEM</span><span class="sxs-lookup"><span data-stu-id="21a7c-341">NOT</span></span> |  |
| --- | --- |
| <span data-ttu-id="21a7c-342">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="21a7c-342">True</span></span> |<span data-ttu-id="21a7c-343">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="21a7c-343">False</span></span> |
| <span data-ttu-id="21a7c-344">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="21a7c-344">False</span></span> |<span data-ttu-id="21a7c-345">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="21a7c-345">True</span></span> |
| <span data-ttu-id="21a7c-346">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-346">Undefined</span></span> |<span data-ttu-id="21a7c-347">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="21a7c-347">Undefined</span></span> |

### <a name="in-keyword"></a><span data-ttu-id="21a7c-348">A kulcsszó</span><span class="sxs-lookup"><span data-stu-id="21a7c-348">IN keyword</span></span>
<span data-ttu-id="21a7c-349">Az IN kulcsszó segítségével ellenőrizze, hogy a megadott érték megegyezik-e a lista bármely értéke.</span><span class="sxs-lookup"><span data-stu-id="21a7c-349">The IN keyword can be used to check whether a specified value matches any value in a list.</span></span> <span data-ttu-id="21a7c-350">Például a lekérdezés által visszaadott összes családba tartozó dokumentumok ahol az azonosító: "WakefieldFamily" vagy "AndersenFamily".</span><span class="sxs-lookup"><span data-stu-id="21a7c-350">For example, this query returns all family documents where the id is one of "WakefieldFamily" or "AndersenFamily".</span></span> 

    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

<span data-ttu-id="21a7c-351">Ez a példa visszaadja az összes dokumentumot ahol állapota valamely megadott értékét.</span><span class="sxs-lookup"><span data-stu-id="21a7c-351">This example returns all documents where the state is any of the specified values.</span></span>

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a><span data-ttu-id="21a7c-352">Ternáris (?) és a Coalesce (?) operátorok</span><span class="sxs-lookup"><span data-stu-id="21a7c-352">Ternary (?) and Coalesce (??) operators</span></span>
<span data-ttu-id="21a7c-353">A háromkomponensű és Coalesce műveleteivel végrehajtható feltételes kifejezéseket, például a C# és JavaScript népszerű programozási nyelvek hasonló létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="21a7c-353">The Ternary and Coalesce operators can be used to build conditional expressions, similar to popular programming languages like C# and JavaScript.</span></span> 

<span data-ttu-id="21a7c-354">A háromkomponensű (?) operátor akkor lehet hasznos, ha hozhat létre az új JSON-tulajdonságok menet közben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-354">The Ternary (?) operator can be very handy when constructing new JSON properties on the fly.</span></span> <span data-ttu-id="21a7c-355">Például most is lekérdezéseket írhat az osztály szintek például kezdő/köztes/speciális alább látható módon emberi olvasható formába besorolását.</span><span class="sxs-lookup"><span data-stu-id="21a7c-355">For example, now you can write queries to classify the class levels into a human readable form like Beginner/Intermediate/Advanced as shown below.</span></span>

     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

<span data-ttu-id="21a7c-356">Az operátor például az alábbi lekérdezést a hívások is ágyazhatja.</span><span class="sxs-lookup"><span data-stu-id="21a7c-356">You can also nest the calls to the operator like in the query below.</span></span>

    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

<span data-ttu-id="21a7c-357">Más lekérdezési operátorok, ha a feltételes kifejezésben hivatkozott tulajdonságok dokumentumtípus hiányzik, vagy ha a összehasonlított típusok eltérőek, majd ezeket a dokumentumokat nem tartoznak a lekérdezés eredményében.</span><span class="sxs-lookup"><span data-stu-id="21a7c-357">As with other query operators, if the referenced properties in the conditional expression are missing in any document, or if the types being compared are different, then those documents are excluded in the query results.</span></span>

<span data-ttu-id="21a7c-358">A Coalesce (?) operátor segítségével hatékonyan ellenőrizze a tulajdonság (más néven</span><span class="sxs-lookup"><span data-stu-id="21a7c-358">The Coalesce (??) operator can be used to efficiently check for the presence of a property (a.k.a.</span></span> <span data-ttu-id="21a7c-359">van meghatározva) dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="21a7c-359">is defined) in a document.</span></span> <span data-ttu-id="21a7c-360">Ez akkor hasznos, ha félig strukturált lekérdezését vagy vegyes típusú adatokat.</span><span class="sxs-lookup"><span data-stu-id="21a7c-360">This is useful when querying against semi-structured or data of mixed types.</span></span> <span data-ttu-id="21a7c-361">Például a lekérdezés visszaadja a "Vezetéknév" Ha jelen van, vagy a "Vezetéknév" Ha nem, akkor a jelen.</span><span class="sxs-lookup"><span data-stu-id="21a7c-361">For example, this query returns the "lastName" if present, or the "surname" if it isn't present.</span></span>

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <span data-ttu-id="21a7c-362"><a id="EscapingReservedKeywords"></a>Idézőjelek közé zárt tulajdonságelérő</span><span class="sxs-lookup"><span data-stu-id="21a7c-362"><a id="EscapingReservedKeywords"></a>Quoted property accessor</span></span>
<span data-ttu-id="21a7c-363">Emellett az idézőjelek közé zárt tulajdonság operátorral tulajdonságok `[]`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-363">You can also access properties using the quoted property operator `[]`.</span></span> <span data-ttu-id="21a7c-364">Például `SELECT c.grade` és `SELECT c["grade"]` egyenértékű.</span><span class="sxs-lookup"><span data-stu-id="21a7c-364">For example, `SELECT c.grade` and `SELECT c["grade"]` are equivalent.</span></span> <span data-ttu-id="21a7c-365">Ez a szintaxis akkor hasznos, ha kell megadnia egy tulajdonság szóközöket, különleges karaktereket tartalmaz, vagy történik a neve megegyezik egy SQL kulcsszó vagy fenntartott szó.</span><span class="sxs-lookup"><span data-stu-id="21a7c-365">This syntax is useful when you need to escape a property that contains spaces, special characters, or happens to share the same name as a SQL keyword or reserved word.</span></span>

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <span data-ttu-id="21a7c-366"><a id="SelectClause"></a>SELECT záradékban</span><span class="sxs-lookup"><span data-stu-id="21a7c-366"><a id="SelectClause"></a>SELECT clause</span></span>
<span data-ttu-id="21a7c-367">A SELECT záradékban (**`SELECT <select_list>`**) megadása kötelező, és határozza meg, milyen értékeket a rendszer beolvassa az a lekérdezés fentiekhez hasonló ANSI-SQL-ben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-367">The SELECT clause (**`SELECT <select_list>`**) is mandatory and specifies what values are retrieved from the query, just like in ANSI-SQL.</span></span> <span data-ttu-id="21a7c-368">A részhalmazán, amelyben a forrás dokumentumok felett van szűrve a leképezés fázis, amikor a rendszer beolvassa a megadott JSON-értékeket, és egy új JSON-objektum minden egyes azt az alakzatot átadott bemeneti helyezik át lettek adva.</span><span class="sxs-lookup"><span data-stu-id="21a7c-368">The subset that's been filtered on top of the source documents are passed onto the projection phase, where the specified JSON values are retrieved and a new JSON object is constructed, for each input passed onto it.</span></span> 

<span data-ttu-id="21a7c-369">A következő példa bemutatja egy tipikus SELECT lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="21a7c-369">The following example shows a typical SELECT query.</span></span> 

<span data-ttu-id="21a7c-370">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-370">**Query**</span></span>

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="21a7c-371">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-371">**Results**</span></span>

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a><span data-ttu-id="21a7c-372">Beágyazott tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="21a7c-372">Nested properties</span></span>
<span data-ttu-id="21a7c-373">A következő példában két beágyazott tulajdonságok azt kivetítéséről `f.address.state` és `f.address.city`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-373">In the following example, we are projecting two nested properties `f.address.state` and `f.address.city`.</span></span>

<span data-ttu-id="21a7c-374">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-374">**Query**</span></span>

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="21a7c-375">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-375">**Results**</span></span>

    [{
      "state": "WA", 
      "city": "seattle"
    }]


<span data-ttu-id="21a7c-376">Leképezési JSON kifejezések is támogatja, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="21a7c-376">Projection also supports JSON expressions as shown in the following example:</span></span>

<span data-ttu-id="21a7c-377">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-377">**Query**</span></span>

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="21a7c-378">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-378">**Results**</span></span>

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


<span data-ttu-id="21a7c-379">Nézzük szerepe `$1` itt.</span><span class="sxs-lookup"><span data-stu-id="21a7c-379">Let's look at the role of `$1` here.</span></span> <span data-ttu-id="21a7c-380">A `SELECT` záradék létre kell hoznia egy JSON-objektum, és nem kulcsra azért van, mert implicit argumentum változónevek kezdve használjuk `$1`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-380">The `SELECT` clause needs to create a JSON object and since no key is provided, we use implicit argument variable names starting with `$1`.</span></span> <span data-ttu-id="21a7c-381">Például a lekérdezés által visszaadott implicit argumentum két változót, címkével `$1` és `$2`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-381">For example, this query returns two implicit argument variables, labeled `$1` and `$2`.</span></span>

<span data-ttu-id="21a7c-382">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-382">**Query**</span></span>

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="21a7c-383">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-383">**Results**</span></span>

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a><span data-ttu-id="21a7c-384">Aliasképző</span><span class="sxs-lookup"><span data-stu-id="21a7c-384">Aliasing</span></span>
<span data-ttu-id="21a7c-385">Most tegyük kiterjesztése a fenti példában az explicit aliasképző értékek.</span><span class="sxs-lookup"><span data-stu-id="21a7c-385">Now let's extend the example above with explicit aliasing of values.</span></span> <span data-ttu-id="21a7c-386">Ez a kulcsszó használt aliassal való ellátását.</span><span class="sxs-lookup"><span data-stu-id="21a7c-386">AS is the keyword used for aliasing.</span></span> <span data-ttu-id="21a7c-387">Nem kötelező, a második érték kivetítéséről közben látható `NameInfo`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-387">It's optional as shown while projecting the second value as `NameInfo`.</span></span> 

<span data-ttu-id="21a7c-388">Abban az esetben, ha a lekérdezés két tulajdonság azonos névvel rendelkezik, használt aliassal való ellátását, nevezze át a tulajdonságok közül, hogy azok a tervezett eredmény vannak használatát.</span><span class="sxs-lookup"><span data-stu-id="21a7c-388">In case a query has two properties with the same name, aliasing must be used to rename one or both of the properties so that they are disambiguated in the projected result.</span></span>

<span data-ttu-id="21a7c-389">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-389">**Query**</span></span>

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="21a7c-390">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-390">**Results**</span></span>

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a><span data-ttu-id="21a7c-391">Skaláris kifejezések</span><span class="sxs-lookup"><span data-stu-id="21a7c-391">Scalar expressions</span></span>
<span data-ttu-id="21a7c-392">Mellett tulajdonsághivatkozást a SELECT záradék is támogatja a skaláris kifejezések állandók, aritmetikai kifejezésekben, logikai kifejezéseket és stb. Például ez egy egyszerű "Hello, World" lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="21a7c-392">In addition to property references, the SELECT clause also supports scalar expressions like constants, arithmetic expressions, logical expressions, etc. For example, here's a simple "Hello World" query.</span></span>

<span data-ttu-id="21a7c-393">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-393">**Query**</span></span>

    SELECT "Hello World"

<span data-ttu-id="21a7c-394">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-394">**Results**</span></span>

    [{
      "$1": "Hello World"
    }]


<span data-ttu-id="21a7c-395">Ez egy összetett példa, amely a skaláris kifejezést használ.</span><span class="sxs-lookup"><span data-stu-id="21a7c-395">Here's a more complex example that uses a scalar expression.</span></span>

<span data-ttu-id="21a7c-396">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-396">**Query**</span></span>

    SELECT ((2 + 11 % 7)-2)/3    

<span data-ttu-id="21a7c-397">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-397">**Results**</span></span>

    [{
      "$1": 1.33333
    }]


<span data-ttu-id="21a7c-398">A következő példában a skaláris kifejezés eredménye egy logikai érték.</span><span class="sxs-lookup"><span data-stu-id="21a7c-398">In the following example, the result of the scalar expression is a Boolean.</span></span>

<span data-ttu-id="21a7c-399">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-399">**Query**</span></span>

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f    

<span data-ttu-id="21a7c-400">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-400">**Results**</span></span>

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a><span data-ttu-id="21a7c-401">Az objektum és tömb létrehozása</span><span class="sxs-lookup"><span data-stu-id="21a7c-401">Object and array creation</span></span>
<span data-ttu-id="21a7c-402">A DocumentDB API SQL egy másik alapfunkciója tömb vagy objektum-létrehozás.</span><span class="sxs-lookup"><span data-stu-id="21a7c-402">Another key feature of DocumentDB API SQL is array/object creation.</span></span> <span data-ttu-id="21a7c-403">Az előző példában vegye figyelembe, hogy létrehoztunk egy új JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="21a7c-403">In the previous example, note that we created a new JSON object.</span></span> <span data-ttu-id="21a7c-404">Hasonlóképpen egy is végezhet tömbök a következő példákban látható módon:</span><span class="sxs-lookup"><span data-stu-id="21a7c-404">Similarly, one can also construct arrays as shown in the following examples:</span></span>

<span data-ttu-id="21a7c-405">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-405">**Query**</span></span>

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f    

<span data-ttu-id="21a7c-406">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-406">**Results**</span></span>  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <span data-ttu-id="21a7c-407"><a id="ValueKeyword"></a>ÉRTÉK kulcsszó</span><span class="sxs-lookup"><span data-stu-id="21a7c-407"><a id="ValueKeyword"></a>VALUE keyword</span></span>
<span data-ttu-id="21a7c-408">A **érték** kulcsszó vissza JSON-érték lehetőséget kínál.</span><span class="sxs-lookup"><span data-stu-id="21a7c-408">The **VALUE** keyword provides a way to return JSON value.</span></span> <span data-ttu-id="21a7c-409">Például az alábbi lekérdezés visszaadja a skaláris `"Hello World"` helyett `{$1: "Hello World"}`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-409">For example, the query shown below returns the scalar `"Hello World"` instead of `{$1: "Hello World"}`.</span></span>

<span data-ttu-id="21a7c-410">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-410">**Query**</span></span>

    SELECT VALUE "Hello World"

<span data-ttu-id="21a7c-411">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-411">**Results**</span></span>

    [
      "Hello World"
    ]


<span data-ttu-id="21a7c-412">A következő lekérdezés visszaadja a JSON-érték nélkül a `"address"` címke az eredmények között.</span><span class="sxs-lookup"><span data-stu-id="21a7c-412">The following query returns the JSON value without the `"address"` label in the results.</span></span>

<span data-ttu-id="21a7c-413">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-413">**Query**</span></span>

    SELECT VALUE f.address
    FROM Families f    

<span data-ttu-id="21a7c-414">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-414">**Results**</span></span>  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

<span data-ttu-id="21a7c-415">Az alábbi példa bővíti a bemutatják, hogyan adhat vissza JSON egyszerű értékeket (a levélszintű a JSON-fa).</span><span class="sxs-lookup"><span data-stu-id="21a7c-415">The following example extends this to show how to return JSON primitive values (the leaf level of the JSON tree).</span></span> 

<span data-ttu-id="21a7c-416">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-416">**Query**</span></span>

    SELECT VALUE f.address.state
    FROM Families f    

<span data-ttu-id="21a7c-417">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-417">**Results**</span></span>

    [
      "WA",
      "NY"
    ]


### <a name="-operator"></a><span data-ttu-id="21a7c-418">* Operátor</span><span class="sxs-lookup"><span data-stu-id="21a7c-418">* Operator</span></span>
<span data-ttu-id="21a7c-419">A speciális operátort (*) a rendszer támogatja a dokumentumot a projekt-van.</span><span class="sxs-lookup"><span data-stu-id="21a7c-419">The special operator (*) is supported to project the document as-is.</span></span> <span data-ttu-id="21a7c-420">Használatakor az egyetlen tervezett mező kell lennie.</span><span class="sxs-lookup"><span data-stu-id="21a7c-420">When used, it must be the only projected field.</span></span> <span data-ttu-id="21a7c-421">Például a lekérdezés során `SELECT * FROM Families f` érvényes, `SELECT VALUE * FROM Families f ` és `SELECT *, f.id FROM Families f ` érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="21a7c-421">While a query like `SELECT * FROM Families f` is valid, `SELECT VALUE * FROM Families f ` and  `SELECT *, f.id FROM Families f ` are not valid.</span></span>

<span data-ttu-id="21a7c-422">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-422">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="21a7c-423">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-423">**Results**</span></span>

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

### <span data-ttu-id="21a7c-424"><a id="TopKeyword"></a>TOP operátor</span><span class="sxs-lookup"><span data-stu-id="21a7c-424"><a id="TopKeyword"></a>TOP Operator</span></span>
<span data-ttu-id="21a7c-425">A felső kulcsszó is használható egy lekérdezés által értékek számának korlátozása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-425">The TOP keyword can be used to limit the number of values from a query.</span></span> <span data-ttu-id="21a7c-426">FELSŐ együtt az ORDER BY záradék használata esetén az eredménykészlet korlátozódik rendezett értékek; az első N száma Ellenkező esetben azt számát adja vissza az első N eredmények nem definiált sorrendben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-426">When TOP is used in conjunction with the ORDER BY clause, the result set is limited to the first N number of ordered values; otherwise, it returns the first N number of results in an undefined order.</span></span> <span data-ttu-id="21a7c-427">Ajánlott eljárásként a SELECT utasítással, mindig használja az ORDER BY záradék a TOP záradék.</span><span class="sxs-lookup"><span data-stu-id="21a7c-427">As a best practice, in a SELECT statement, always use an ORDER BY clause with the TOP clause.</span></span> <span data-ttu-id="21a7c-428">Ez az az egyetlen lehetőség kiszámítható módon tudja TOP által érintett sorok jelöléséhez.</span><span class="sxs-lookup"><span data-stu-id="21a7c-428">This is the only way to predictably indicate which rows are affected by TOP.</span></span> 

<span data-ttu-id="21a7c-429">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-429">**Query**</span></span>

    SELECT TOP 1 * 
    FROM Families f 

<span data-ttu-id="21a7c-430">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-430">**Results**</span></span>

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

<span data-ttu-id="21a7c-431">FELSŐ egy állandó értékkel (ahogy fent), vagy a paraméteres lekérdezés változó érték használható.</span><span class="sxs-lookup"><span data-stu-id="21a7c-431">TOP can be used with a constant value (as shown above) or with a variable value using parameterized queries.</span></span> <span data-ttu-id="21a7c-432">További részletekért lásd a paraméteres lekérdezés az alábbi.</span><span class="sxs-lookup"><span data-stu-id="21a7c-432">For more details, please see parameterized queries below.</span></span>

### <span data-ttu-id="21a7c-433"><a id="Aggregates"></a>Aggregátumfüggvények</span><span class="sxs-lookup"><span data-stu-id="21a7c-433"><a id="Aggregates"></a>Aggregate Functions</span></span>
<span data-ttu-id="21a7c-434">Az összesítéseket is elvégezheti a `SELECT` záradékban.</span><span class="sxs-lookup"><span data-stu-id="21a7c-434">You can also perform aggregations in the `SELECT` clause.</span></span> <span data-ttu-id="21a7c-435">Az aggregátumfüggvények egy értékhalmazt a számítás elvégzése, és egyetlen érték visszaadása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-435">Aggregate functions perform a calculation on a set of values and return a single value.</span></span> <span data-ttu-id="21a7c-436">Például a következő lekérdezés a gyűjteményen belül családba tartozó dokumentumok számát adja meg.</span><span class="sxs-lookup"><span data-stu-id="21a7c-436">For example, the following query returns the count of family documents within the collection.</span></span>

<span data-ttu-id="21a7c-437">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-437">**Query**</span></span>

    SELECT COUNT(1) 
    FROM Families f 

<span data-ttu-id="21a7c-438">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-438">**Results**</span></span>

    [{
        "$1": 2
    }]

<span data-ttu-id="21a7c-439">Az összesítés skaláris érték használatával is visszatérhet a `VALUE` kulcsszó.</span><span class="sxs-lookup"><span data-stu-id="21a7c-439">You can also return the scalar value of the aggregate by using the `VALUE` keyword.</span></span> <span data-ttu-id="21a7c-440">Például a következő lekérdezés egyetlen számként értékek számát adja vissza:</span><span class="sxs-lookup"><span data-stu-id="21a7c-440">For example, the following query returns the count of values as a single number:</span></span>

<span data-ttu-id="21a7c-441">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-441">**Query**</span></span>

    SELECT VALUE COUNT(1) 
    FROM Families f 

<span data-ttu-id="21a7c-442">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-442">**Results**</span></span>

    [ 2 ]

<span data-ttu-id="21a7c-443">A szűrők együtt is elvégezheti összesíti.</span><span class="sxs-lookup"><span data-stu-id="21a7c-443">You can also perform aggregates in combination with filters.</span></span> <span data-ttu-id="21a7c-444">Például a következő lekérdezés a Washington állam címével a dokumentumok számát küldi vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-444">For example, the following query returns the count of documents with the address in the state of Washington.</span></span>

<span data-ttu-id="21a7c-445">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-445">**Query**</span></span>

    SELECT VALUE COUNT(1) 
    FROM Families f
    WHERE f.address.state = "WA" 

<span data-ttu-id="21a7c-446">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-446">**Results**</span></span>

    [ 1 ]

<span data-ttu-id="21a7c-447">A következő táblázat a DocumentDB API támogatott összesítő függvények listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-447">The following table shows the list of supported aggregate functions in DocumentDB API.</span></span> <span data-ttu-id="21a7c-448">`SUM`és `AVG` numerikus érték, keresztül hajtja végre, mivel `COUNT`, `MIN`, és `MAX` karakterláncok, a logikai és nullák keresztül hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="21a7c-448">`SUM` and `AVG` are performed over numeric values, whereas `COUNT`, `MIN`, and `MAX` can be performed over numbers, strings, Booleans, and nulls.</span></span> 

| <span data-ttu-id="21a7c-449">Használat</span><span class="sxs-lookup"><span data-stu-id="21a7c-449">Usage</span></span> | <span data-ttu-id="21a7c-450">Leírás</span><span class="sxs-lookup"><span data-stu-id="21a7c-450">Description</span></span> |
|-------|-------------|
| <span data-ttu-id="21a7c-451">SZÁMA</span><span class="sxs-lookup"><span data-stu-id="21a7c-451">COUNT</span></span> | <span data-ttu-id="21a7c-452">A kifejezés a számú elemet ad vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-452">Returns the number of items in the expression.</span></span> |
| <span data-ttu-id="21a7c-453">SUM</span><span class="sxs-lookup"><span data-stu-id="21a7c-453">SUM</span></span>   | <span data-ttu-id="21a7c-454">A kifejezés értékek összegét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-454">Returns the sum of all the values in the expression.</span></span> |
| <span data-ttu-id="21a7c-455">PERC</span><span class="sxs-lookup"><span data-stu-id="21a7c-455">MIN</span></span>   | <span data-ttu-id="21a7c-456">A kifejezés minimumértékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-456">Returns the minimum value in the expression.</span></span> |
| <span data-ttu-id="21a7c-457">MAXIMÁLIS SZÁMA</span><span class="sxs-lookup"><span data-stu-id="21a7c-457">MAX</span></span>   | <span data-ttu-id="21a7c-458">A kifejezés maximumértékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-458">Returns the maximum value in the expression.</span></span> |
| <span data-ttu-id="21a7c-459">ÁTLAGOS</span><span class="sxs-lookup"><span data-stu-id="21a7c-459">AVG</span></span>   | <span data-ttu-id="21a7c-460">Az értékek átlagát adja vissza. a kifejezést.</span><span class="sxs-lookup"><span data-stu-id="21a7c-460">Returns the average of the values in the expression.</span></span> |

<span data-ttu-id="21a7c-461">Összesíti egy tömb iteráció eredményeit keresztül is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="21a7c-461">Aggregates can also be performed over the results of an array iteration.</span></span> <span data-ttu-id="21a7c-462">További információkért lásd: [tömb iterációs lekérdezésekben](#Iteration).</span><span class="sxs-lookup"><span data-stu-id="21a7c-462">For more information, see [Array Iteration in Queries](#Iteration).</span></span>

> [!NOTE]
> <span data-ttu-id="21a7c-463">Az Azure-portálon Query Explorer használata esetén vegye figyelembe, hogy összesítési lekérdezések a részlegesen összesített eredmények adhat vissza a lekérdezés lap.</span><span class="sxs-lookup"><span data-stu-id="21a7c-463">When using the Azure portal's Query Explorer, note that aggregation queries may return the partially aggregated results over a query page.</span></span> <span data-ttu-id="21a7c-464">Az SDK-k egyetlen értéket összesítő összes oldalán hoz létre.</span><span class="sxs-lookup"><span data-stu-id="21a7c-464">The SDKs produces a single cumulative value across all pages.</span></span> 
> 
> <span data-ttu-id="21a7c-465">Kód használatával összesítési lekérdezések végrehajtásához szükséges .NET SDK 1.12.0, a .NET Core SDK 1.1.0-ás vagy a Java SDK 1.9.5 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="21a7c-465">In order to perform aggregation queries using code, you need .NET SDK 1.12.0, .NET Core SDK 1.1.0, or Java SDK 1.9.5 or above.</span></span>    
>

## <span data-ttu-id="21a7c-466"><a id="OrderByClause"></a>ORDER BY záradék</span><span class="sxs-lookup"><span data-stu-id="21a7c-466"><a id="OrderByClause"></a>ORDER BY clause</span></span>
<span data-ttu-id="21a7c-467">Például az ANSI-SQL-ben megadhat egy választható Order By záradék lekérdezése során.</span><span class="sxs-lookup"><span data-stu-id="21a7c-467">Like in ANSI-SQL, you can include an optional Order By clause while querying.</span></span> <span data-ttu-id="21a7c-468">A záradékot tartalmazhat választható növekvő/CSÖKKENŐ argumentumaként adja meg a sorrendet, amelyben eredményeket kell beolvasni.</span><span class="sxs-lookup"><span data-stu-id="21a7c-468">The clause can include an optional ASC/DESC argument to specify the order in which results must be retrieved.</span></span>

<span data-ttu-id="21a7c-469">Például ez kártevőcsaládok a rezidens városnév sorrendjét, amely.</span><span class="sxs-lookup"><span data-stu-id="21a7c-469">For example, here's a query that retrieves families in order of the resident city's name.</span></span>

<span data-ttu-id="21a7c-470">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-470">**Query**</span></span>

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city

<span data-ttu-id="21a7c-471">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-471">**Results**</span></span>

    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"    
      }
    ]

<span data-ttu-id="21a7c-472">És az alábbiakban kártevőcsaládok létrehozásának dátuma, amely egy számot jelölő, kor alapidőpontjának korábban sorrendjét, amely idő, azaz 1970. január 1. a óta eltelt idő másodpercben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-472">And here's a query that retrieves families in order of creation date, which is stored as a number representing the epoch time, i.e, elapsed time since Jan 1, 1970 in seconds.</span></span>

<span data-ttu-id="21a7c-473">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-473">**Query**</span></span>

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC

<span data-ttu-id="21a7c-474">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-474">**Results**</span></span>

    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472    
      }
    ]

## <span data-ttu-id="21a7c-475"><a id="Advanced"></a>Speciális adatbázis fogalmait és az SQL-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="21a7c-475"><a id="Advanced"></a>Advanced database concepts and SQL queries</span></span>

### <span data-ttu-id="21a7c-476"><a id="Iteration"></a>Ismétlés</span><span class="sxs-lookup"><span data-stu-id="21a7c-476"><a id="Iteration"></a>Iteration</span></span>
<span data-ttu-id="21a7c-477">Egy új szerkezet művelettel lett hozzáadva a **IN** DocumentDB API SQL támogatást nyújt a JSON-tömbök keresztül léptetés kulcsszót.</span><span class="sxs-lookup"><span data-stu-id="21a7c-477">A new construct was added via the **IN** keyword in DocumentDB API SQL to provide support for iterating over JSON arrays.</span></span> <span data-ttu-id="21a7c-478">A FROM forrás iterációs támogatja.</span><span class="sxs-lookup"><span data-stu-id="21a7c-478">The FROM source provides support for iteration.</span></span> <span data-ttu-id="21a7c-479">Kezdjük az alábbi példa:</span><span class="sxs-lookup"><span data-stu-id="21a7c-479">Let's start with the following example:</span></span>

<span data-ttu-id="21a7c-480">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-480">**Query**</span></span>

    SELECT * 
    FROM Families.children

<span data-ttu-id="21a7c-481">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-481">**Results**</span></span>  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

<span data-ttu-id="21a7c-482">Most már egy másik lekérdezés keresztül a gyűjtemény gyermekek iterációs végző vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="21a7c-482">Now let's look at another query that performs iteration over children in the collection.</span></span> <span data-ttu-id="21a7c-483">Vegye figyelembe a különbség a kimeneti tömbben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-483">Note the difference in the output array.</span></span> <span data-ttu-id="21a7c-484">Ez a példa felosztja a `children` és az eredmények simítja egyetlen tömbbe.</span><span class="sxs-lookup"><span data-stu-id="21a7c-484">This example splits `children` and flattens the results into a single array.</span></span>  

<span data-ttu-id="21a7c-485">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-485">**Query**</span></span>

    SELECT * 
    FROM c IN Families.children

<span data-ttu-id="21a7c-486">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-486">**Results**</span></span>  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

<span data-ttu-id="21a7c-487">Ez további használható szűrést végezni a tömb minden egyes bejegyzés, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="21a7c-487">This can be further used to filter on each individual entry of the array as shown in the following example:</span></span>

<span data-ttu-id="21a7c-488">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-488">**Query**</span></span>

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

<span data-ttu-id="21a7c-489">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-489">**Results**</span></span>  

    [{
      "givenName": "Lisa"
    }]

<span data-ttu-id="21a7c-490">Összesítési tömb iterációs eredményét keresztül is elvégezheti.</span><span class="sxs-lookup"><span data-stu-id="21a7c-490">You can also perform aggregation over the result of array iteration.</span></span> <span data-ttu-id="21a7c-491">Például a következő lekérdezés gyermekei közötti összes családok megszámlálása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-491">For example, the following query counts the number of children among all families.</span></span>

<span data-ttu-id="21a7c-492">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-492">**Query**</span></span>

    SELECT COUNT(child) 
    FROM child IN Families.children

<span data-ttu-id="21a7c-493">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-493">**Results**</span></span>  

    [
      { 
        "$1": 3
      }
    ]

### <span data-ttu-id="21a7c-494"><a id="Joins"></a>Illesztése</span><span class="sxs-lookup"><span data-stu-id="21a7c-494"><a id="Joins"></a>Joins</span></span>
<span data-ttu-id="21a7c-495">Táblák között csatlakoztatni kell egy relációs adatbázisban, fontos.</span><span class="sxs-lookup"><span data-stu-id="21a7c-495">In a relational database, the need to join across tables is important.</span></span> <span data-ttu-id="21a7c-496">A logikai corollary az normalizált sémák.</span><span class="sxs-lookup"><span data-stu-id="21a7c-496">It's the logical corollary to designing normalized schemas.</span></span> <span data-ttu-id="21a7c-497">Ezzel szemben a DocumentDB API a nem normalizált adatok modell sémamentes dokumentumok foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="21a7c-497">Contrary to this, DocumentDB API deals with the denormalized data model of schema-free documents.</span></span> <span data-ttu-id="21a7c-498">Ez megfelel a logikai a "önillesztés".</span><span class="sxs-lookup"><span data-stu-id="21a7c-498">This is the logical equivalent of a "self-join".</span></span>

<span data-ttu-id="21a7c-499">A nyelvi támogató szintaxisa < from_source1 > Csatlakozás < from_source2 > ILLESZTÉSI... CSATLAKOZTASSA az < from_sourceN >.</span><span class="sxs-lookup"><span data-stu-id="21a7c-499">The syntax that the language supports is <from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>.</span></span> <span data-ttu-id="21a7c-500">A teljes, ezt adja vissza, amely **N**- rekordokat (a rekord **N** értékek).</span><span class="sxs-lookup"><span data-stu-id="21a7c-500">Overall, this returns a set of **N**-tuples (tuple with **N** values).</span></span> <span data-ttu-id="21a7c-501">A táblakonstruktor minden rekordjának összes gyűjtemény alias léptetés alatt az megfelelő készletek által visszaadott érték tartozik.</span><span class="sxs-lookup"><span data-stu-id="21a7c-501">Each tuple has values produced by iterating all collection aliases over their respective sets.</span></span> <span data-ttu-id="21a7c-502">Más szóval ez az egy teljes a a illesztésben részt vevő készlet keresztszorzatát.</span><span class="sxs-lookup"><span data-stu-id="21a7c-502">In other words, this is a full cross product of the sets participating in the join.</span></span>

<span data-ttu-id="21a7c-503">Az alábbi példák bemutatják, hogyan működik a JOIN záradékban.</span><span class="sxs-lookup"><span data-stu-id="21a7c-503">The following examples show how the JOIN clause works.</span></span> <span data-ttu-id="21a7c-504">A következő példa eredménye nem üres, a forrás minden dokumentumát keresztszorzatát óta és üres üres.</span><span class="sxs-lookup"><span data-stu-id="21a7c-504">In the following example, the result is empty since the cross product of each document from source and an empty set is empty.</span></span>

<span data-ttu-id="21a7c-505">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-505">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

<span data-ttu-id="21a7c-506">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-506">**Results**</span></span>  

    [{
    }]


<span data-ttu-id="21a7c-507">A következő példában az illesztés a dokumentumgyökér között van, és a `children` subroot.</span><span class="sxs-lookup"><span data-stu-id="21a7c-507">In the following example, the join is between the document root and the `children` subroot.</span></span> <span data-ttu-id="21a7c-508">Egy eltérő termék két JSON-objektumok között.</span><span class="sxs-lookup"><span data-stu-id="21a7c-508">It's a cross product between two JSON objects.</span></span> <span data-ttu-id="21a7c-509">Arra, hogy gyermeke tömb nincs hatékony az ILLESZTÉS mivel azt a egyetlen legfelső szintű a gyermekek tömb nem foglalkoznak.</span><span class="sxs-lookup"><span data-stu-id="21a7c-509">The fact that children is an array is not effective in the JOIN since we are dealing with a single root that is the children array.</span></span> <span data-ttu-id="21a7c-510">Ezért az eredmény tartalmazza csak két eredményt, mivel minden dokumentumot a tömbbel rendelkező keresztszorzatát pontosan csak egy dokumentum adja eredményül.</span><span class="sxs-lookup"><span data-stu-id="21a7c-510">Hence the result contains only two results, since the cross product of each document with the array yields exactly only one document.</span></span>

<span data-ttu-id="21a7c-511">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-511">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN f.children

<span data-ttu-id="21a7c-512">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-512">**Results**</span></span>

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


<span data-ttu-id="21a7c-513">A következő példa bemutatja a több hagyományos csatlakozzon:</span><span class="sxs-lookup"><span data-stu-id="21a7c-513">The following example shows a more conventional join:</span></span>

<span data-ttu-id="21a7c-514">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-514">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

<span data-ttu-id="21a7c-515">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-515">**Results**</span></span>

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



<span data-ttu-id="21a7c-516">A legfontosabb, ami arról értesít, hogy a `from_source` , a **csatlakozás** záradék egy iterátor.</span><span class="sxs-lookup"><span data-stu-id="21a7c-516">The first thing to note is that the `from_source` of the **JOIN** clause is an iterator.</span></span> <span data-ttu-id="21a7c-517">Igen a folyamat ebben az esetben a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="21a7c-517">So, the flow in this case is as follows:</span></span>  

* <span data-ttu-id="21a7c-518">Bontsa ki az egyes gyermekelem **c** a tömbben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-518">Expand each child element **c** in the array.</span></span>
* <span data-ttu-id="21a7c-519">A dokumentum gyökerébe határokon termék alkalmazása **f** minden gyermekelemmel rendelkező **c** , amely lett egybesimított-e az első lépésben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-519">Apply a cross product with the root of the document **f** with each child element **c** that was flattened in the first step.</span></span>
* <span data-ttu-id="21a7c-520">Végezetül projektre a legfelső szintű objektum **f** névtulajdonság önmagában.</span><span class="sxs-lookup"><span data-stu-id="21a7c-520">Finally, project the root object **f** name property alone.</span></span> 

<span data-ttu-id="21a7c-521">Az első dokumentum (`AndersenFamily`) csak egy gyermekelemet tartalmaz, ezért az eredménykészlet csak ez a dokumentum megfelelő egyetlen objektumot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="21a7c-521">The first document (`AndersenFamily`) contains only one child element, so the result set contains only a single object corresponding to this document.</span></span> <span data-ttu-id="21a7c-522">A második dokumentum (`WakefieldFamily`) két gyermekeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="21a7c-522">The second document (`WakefieldFamily`) contains two children.</span></span> <span data-ttu-id="21a7c-523">Igen a határokon termék minden gyermek, ezáltal két objektum, egy minden gyermek, ez a dokumentum megfelelő eredményezve egy külön objektumot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="21a7c-523">So, the cross product produces a separate object for each child, thereby resulting in two objects, one for each child corresponding to this document.</span></span> <span data-ttu-id="21a7c-524">A legfelső szintű mezők mindkét ezekben a dokumentumokban ugyanazok, mint egy határokon termékben teheti meg.</span><span class="sxs-lookup"><span data-stu-id="21a7c-524">The root fields in both these documents are the same, just as you would expect in a cross product.</span></span>

<span data-ttu-id="21a7c-525">A valós segédprogram csatlakozási űrlap rekordokat származik, amely egyébként nehezen projekt alakzat a kereszt-termék.</span><span class="sxs-lookup"><span data-stu-id="21a7c-525">The real utility of the JOIN is to form tuples from the cross-product in a shape that's otherwise difficult to project.</span></span> <span data-ttu-id="21a7c-526">Ezenkívül az alábbi példában látható módon szűrést az, hogy megadható, hogy a felhasználó döntött, hogy a rekordokat a teljes feltételfüggvényt feltétel rekordot kombinációja.</span><span class="sxs-lookup"><span data-stu-id="21a7c-526">Furthermore, as we see in the example below, you could filter on the combination of a tuple that lets' the user chose a condition satisfied by the tuples overall.</span></span>

<span data-ttu-id="21a7c-527">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-527">**Query**</span></span>

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets

<span data-ttu-id="21a7c-528">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-528">**Results**</span></span>

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



<span data-ttu-id="21a7c-529">Ebben a példában a fenti példában természetes bővítménye, és végrehajtja a dupla való csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="21a7c-529">This example is a natural extension of the preceding example, and performs a double join.</span></span> <span data-ttu-id="21a7c-530">A határokon termék tehát tekintheti meg a következő látszólagosan kódot:</span><span class="sxs-lookup"><span data-stu-id="21a7c-530">So, the cross product can be viewed as the following pseudo-code:</span></span>

    for-each(Family f in Families)
    {    
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

<span data-ttu-id="21a7c-531">`AndersenFamily`egy gyermek, aki rendelkezik egy háziállat rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="21a7c-531">`AndersenFamily` has one child who has one pet.</span></span> <span data-ttu-id="21a7c-532">Igen, a határokon termék eredményez több sorban is (1\*1\*1) a család.</span><span class="sxs-lookup"><span data-stu-id="21a7c-532">So, the cross product yields one row (1\*1\*1) from this family.</span></span> <span data-ttu-id="21a7c-533">WakefieldFamily, azonban a két gyermekelemek tartoznak, de csak egy "Jesse" gyermeket kedvtelésből.</span><span class="sxs-lookup"><span data-stu-id="21a7c-533">WakefieldFamily however has two children, but only one child "Jesse" has pets.</span></span> <span data-ttu-id="21a7c-534">Jesse két kedvtelésből, ha rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="21a7c-534">Jesse has two pets though.</span></span> <span data-ttu-id="21a7c-535">Ezért a határokon termék eredményez 1\*1\*2 = 2 család a sort.</span><span class="sxs-lookup"><span data-stu-id="21a7c-535">Hence the cross product yields 1\*1\*2 = 2 rows from this family.</span></span>

<span data-ttu-id="21a7c-536">A következő példában nincs egy kiegészítő szűrőt `pet`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-536">In the next example, there is an additional filter on `pet`.</span></span> <span data-ttu-id="21a7c-537">Ez nem tartalmazza az összes rekordokat, ahol a háziállatának neve nincs "Árnyékmásolat".</span><span class="sxs-lookup"><span data-stu-id="21a7c-537">This excludes all the tuples where the pet name is not "Shadow".</span></span> <span data-ttu-id="21a7c-538">Figyelje meg, hogy azt képesek tömbök, az a rekord elemek szűrő származó rekordokat létrehozni, és az elemek kombinációja projektre.</span><span class="sxs-lookup"><span data-stu-id="21a7c-538">Notice that we are able to build tuples from arrays, filter on any of the elements of the tuple, and project any combination of the elements.</span></span> 

<span data-ttu-id="21a7c-539">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-539">**Query**</span></span>

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

<span data-ttu-id="21a7c-540">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-540">**Results**</span></span>

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <span data-ttu-id="21a7c-541"><a id="JavaScriptIntegration"></a>JavaScript-integráció</span><span class="sxs-lookup"><span data-stu-id="21a7c-541"><a id="JavaScriptIntegration"></a>JavaScript integration</span></span>
<span data-ttu-id="21a7c-542">Azure Cosmos DB programozási modellt biztosít a feldolgozás alatt álló alapú JavaScript-alkalmazáslogika közvetlenül a gyűjtemények, tárolt eljárások és eseményindítók tekintetében.</span><span class="sxs-lookup"><span data-stu-id="21a7c-542">Azure Cosmos DB provides a programming model for executing JavaScript based application logic directly on the collections in terms of stored procedures and triggers.</span></span> <span data-ttu-id="21a7c-543">Ez lehetővé teszi, hogy mindkét:</span><span class="sxs-lookup"><span data-stu-id="21a7c-543">This allows for both:</span></span>

* <span data-ttu-id="21a7c-544">Lehetővé teszi nagy teljesítményű tranzakciós CRUD műveletek és a JavaScript futásidejű közvetlenül az adatbázis motorján belül szoros integrációja alapján egy gyűjtemény-dokumentumokon végzett lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="21a7c-544">Ability to do high-performance transactional CRUD operations and queries against documents in a collection by virtue of the deep integration of JavaScript runtime directly within the database engine.</span></span> 
* <span data-ttu-id="21a7c-545">Természetes modellezési folyamatábrán, változó hatókörének, és a hozzárendelés és az adatbázis-tranzakciókhoz a primitívek kivételkezelő integrálását.</span><span class="sxs-lookup"><span data-stu-id="21a7c-545">A natural modeling of control flow, variable scoping, and assignment and integration of exception handling primitives with database transactions.</span></span> <span data-ttu-id="21a7c-546">A JavaScript-integráció Azure Cosmos DB-támogatással kapcsolatos további információkért tekintse meg a JavaScript kiszolgálóoldali programozhatóság dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="21a7c-546">For more details about Azure Cosmos DB support for JavaScript integration, please refer to the JavaScript server-side programmability documentation.</span></span>

### <span data-ttu-id="21a7c-547"><a id="UserDefinedFunctions"></a>Felhasználói függvény (UDF)</span><span class="sxs-lookup"><span data-stu-id="21a7c-547"><a id="UserDefinedFunctions"></a>User-Defined Functions (UDFs)</span></span>
<span data-ttu-id="21a7c-548">A típusok már definiálva van ebben a cikkben, valamint a DocumentDB API SQL támogatja az a felhasználó definiált függvény (UDF).</span><span class="sxs-lookup"><span data-stu-id="21a7c-548">Along with the types already defined in this article, DocumentDB API SQL provides support for User Defined Functions (UDF).</span></span> <span data-ttu-id="21a7c-549">Skaláris felhasználó által megadott függvények támogatottak, ahol a fejlesztők nulla vagy több argumentumot adjon át és vissza egyetlen argumentuma eredményt vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-549">In particular, scalar UDFs are supported where the developers can pass in zero or many arguments and return a single argument result back.</span></span> <span data-ttu-id="21a7c-550">Minden egyes argumentum ellenőrzése alatt álló engedélyezett JSON-érték.</span><span class="sxs-lookup"><span data-stu-id="21a7c-550">Each of these arguments is checked for being legal JSON values.</span></span>  

<span data-ttu-id="21a7c-551">A DocumentDB API SQL-szintaxis használatával az ezen felhasználó által definiált függvényeket egyéni alkalmazáslogika támogatása az időtartam.</span><span class="sxs-lookup"><span data-stu-id="21a7c-551">The DocumentDB API SQL syntax is extended to support custom application logic using these User-Defined Functions.</span></span> <span data-ttu-id="21a7c-552">Felhasználó által megadott függvények regisztrálhatók a DocumentDB API, és ezután lehet hivatkozni az SQL-lekérdezés részeként.</span><span class="sxs-lookup"><span data-stu-id="21a7c-552">UDFs can be registered with DocumentDB API and then be referenced as part of a SQL query.</span></span> <span data-ttu-id="21a7c-553">Valójában a felhasználó által megadott függvények exquisitely tervezték, hogy a lekérdezések hívható.</span><span class="sxs-lookup"><span data-stu-id="21a7c-553">In fact, the UDFs are exquisitely designed to be invoked by queries.</span></span> <span data-ttu-id="21a7c-554">Ezt a döntést maradhassanak felhasználó által megadott függvények nincs hozzáférése a context objektumot, a más JavaScript típusok (tárolt eljárások és eseményindítók) rendelkező.</span><span class="sxs-lookup"><span data-stu-id="21a7c-554">As a corollary to this choice, UDFs do not have access to the context object which the other JavaScript types (stored procedures and triggers) have.</span></span> <span data-ttu-id="21a7c-555">Lekérdezések csak olvashatóként hajtható végre, mert futtathatják az elsődleges vagy másodlagos replikákon.</span><span class="sxs-lookup"><span data-stu-id="21a7c-555">Since queries execute as read-only, they can run either on primary or on secondary replicas.</span></span> <span data-ttu-id="21a7c-556">Ezért felhasználó által megadott függvények való más JavaScript típusától eltérően a másodlagos replikákon futtatásra tervezték.</span><span class="sxs-lookup"><span data-stu-id="21a7c-556">Therefore, UDFs are designed to run on secondary replicas unlike other JavaScript types.</span></span>

<span data-ttu-id="21a7c-557">Alább példája egy UDF hogyan lehet regisztrálni, a Cosmos DB adatbázist, kifejezetten egy dokumentumgyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="21a7c-557">Below is an example of how a UDF can be registered at the Cosmos DB database, specifically under a document collection.</span></span>

       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };

       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  

<span data-ttu-id="21a7c-558">Az előző példa létrehoz egy UDF, amelynek a neve `REGEX_MATCH`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-558">The preceding example creates a UDF whose name is `REGEX_MATCH`.</span></span> <span data-ttu-id="21a7c-559">Elfogadja a JSON két karakterlánc-értékek `input` és `pattern` és ellenőrzést, ha az első megfelel a mintának megadott második JavaScript string.match() függvény használatával.</span><span class="sxs-lookup"><span data-stu-id="21a7c-559">It accepts two JSON string values `input` and `pattern` and checks if the first matches the pattern specified in the second using JavaScript's string.match() function.</span></span>

<span data-ttu-id="21a7c-560">A Microsoft most már használhatja a UDF leképezés lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="21a7c-560">We can now use this UDF in a query in a projection.</span></span> <span data-ttu-id="21a7c-561">Felhasználó által megadott függvények kell minősíteni, a kis-és nagybetűket előtaggal "udf."</span><span class="sxs-lookup"><span data-stu-id="21a7c-561">UDFs must be qualified with the case-sensitive prefix "udf."</span></span> <span data-ttu-id="21a7c-562">Amikor meghívhatók lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="21a7c-562">when called from within queries.</span></span> 

> [!NOTE]
> <span data-ttu-id="21a7c-563">3/17/2015, mielőtt Cosmos DB támogatott UDF hívások nélkül az "udf."</span><span class="sxs-lookup"><span data-stu-id="21a7c-563">Prior to 3/17/2015, Cosmos DB supported UDF calls without the "udf."</span></span> <span data-ttu-id="21a7c-564">Válasszon REGEX_MATCH() például előtag.</span><span class="sxs-lookup"><span data-stu-id="21a7c-564">prefix like SELECT REGEX_MATCH().</span></span> <span data-ttu-id="21a7c-565">A hívó mintát elavult.</span><span class="sxs-lookup"><span data-stu-id="21a7c-565">This calling pattern has been deprecated.</span></span>  
> 
> 

<span data-ttu-id="21a7c-566">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-566">**Query**</span></span>

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

<span data-ttu-id="21a7c-567">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-567">**Results**</span></span>

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

<span data-ttu-id="21a7c-568">Az UDF is használható egy szűrő belül ahogy az alábbi példában is tartománynévvel együtt az "udf."</span><span class="sxs-lookup"><span data-stu-id="21a7c-568">The UDF can also be used inside a filter as shown in the example below, also qualified with the "udf."</span></span> <span data-ttu-id="21a7c-569">előtagja:</span><span class="sxs-lookup"><span data-stu-id="21a7c-569">prefix:</span></span>

<span data-ttu-id="21a7c-570">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-570">**Query**</span></span>

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

<span data-ttu-id="21a7c-571">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-571">**Results**</span></span>

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


<span data-ttu-id="21a7c-572">Felhasználó által megadott függvények lényegében érvényes skaláris kifejezések, és leképezések és szűrőket is használhat.</span><span class="sxs-lookup"><span data-stu-id="21a7c-572">In essence, UDFs are valid scalar expressions and can be used in both projections and filters.</span></span> 

<span data-ttu-id="21a7c-573">Bontsa ki a felhasználó által megadott függvények hatványa, vizsgáljuk meg egy másik példa feltételes logikával:</span><span class="sxs-lookup"><span data-stu-id="21a7c-573">To expand on the power of UDFs, let's look at another example with conditional logic:</span></span>

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'seattle':
                           return 520;
                       case 'NY':
                           return 410;
                       case 'Chicago':
                           return 673;
                       default:
                           return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);


<span data-ttu-id="21a7c-574">Az alábbiakban látható egy példa, gyakorolja az UDF-ben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-574">Below is an example that exercises the UDF.</span></span>

<span data-ttu-id="21a7c-575">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-575">**Query**</span></span>

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f    

<span data-ttu-id="21a7c-576">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-576">**Results**</span></span>

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


<span data-ttu-id="21a7c-577">A fenti példákban megjelenítve, mert felhasználó által megadott függvények JavaScript nyelv power a DocumentDB API SQL komplex eljárási, feltételes logikai beépített JavaScript futás közbeni képességek segítségével. Ehhez egy gazdag programozható felületet integrálhatja.</span><span class="sxs-lookup"><span data-stu-id="21a7c-577">As the preceding examples showcase, UDFs integrate the power of JavaScript language with the DocumentDB API SQL to provide a rich programmable interface to do complex procedural, conditional logic with the help of inbuilt JavaScript runtime capabilities.</span></span>

<span data-ttu-id="21a7c-578">A DocumentDB API SQL biztosít az argumentumok a felhasználó által megadott függvények nyilvántartott egyes dokumentumok a forráshelyen szakaszában az aktuális (a WHERE záradékban vagy a SELECT záradékban) UDF feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-578">DocumentDB API SQL provides the arguments to the UDFs for each document in the source at the current stage (WHERE clause or SELECT clause) of processing the UDF.</span></span> <span data-ttu-id="21a7c-579">Az eredmény zökkenőmentesen beépített általános végrehajtás folyamatban.</span><span class="sxs-lookup"><span data-stu-id="21a7c-579">The result is incorporated in the overall execution pipeline seamlessly.</span></span> <span data-ttu-id="21a7c-580">Ha a Tulajdonságok által az UDF paraméterek nem találhatók a JSON-érték, akkor a paraméter nincs definiálva, és ezért a rendszer teljesen kihagyja UDF meghívását.</span><span class="sxs-lookup"><span data-stu-id="21a7c-580">If the properties referred to by the UDF parameters are not available in the JSON value, the parameter is considered as undefined and hence the UDF invocation is entirely skipped.</span></span> <span data-ttu-id="21a7c-581">Hasonló módon az UDF eredménye nem definiált, ha az nem szerepel az eredményben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-581">Similarly if the result of the UDF is undefined, it's not included in the result.</span></span> 

<span data-ttu-id="21a7c-582">Összefoglalva felhasználó által megadott függvények olyan nagy eszközöket tegye a bonyolult üzleti logikát a lekérdezés részeként.</span><span class="sxs-lookup"><span data-stu-id="21a7c-582">In summary, UDFs are great tools to do complex business logic as part of the query.</span></span>

### <a name="operator-evaluation"></a><span data-ttu-id="21a7c-583">A kiértékelési operátor</span><span class="sxs-lookup"><span data-stu-id="21a7c-583">Operator evaluation</span></span>
<span data-ttu-id="21a7c-584">Cosmos DB, egy JSON-adatbázis, amely nem rendelkezik megrajzolja fekvő JavaScript operátorok és az értékelés szemantikáját.</span><span class="sxs-lookup"><span data-stu-id="21a7c-584">Cosmos DB, by the virtue of being a JSON database, draws parallels with JavaScript operators and its evaluation semantics.</span></span> <span data-ttu-id="21a7c-585">Miközben Cosmos DB megpróbálja megőrizheti a JavaScript szemantikáját JSON támogatása szempontjából, a művelet kiértékelése százalékkal, bizonyos esetekben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-585">While Cosmos DB tries to preserve JavaScript semantics in terms of JSON support, the operation evaluation deviates in some instances.</span></span>

<span data-ttu-id="21a7c-586">A DocumentDB API SQL, ellentétben a hagyományos SQL típusú értékeket gyakran nem ismert mindaddig, amíg a rendszer beolvassa az értékeket az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="21a7c-586">In DocumentDB API SQL, unlike in traditional SQL, the types of values are often not known until the values are retrieved from database.</span></span> <span data-ttu-id="21a7c-587">Ahhoz, hogy hatékonyan hajtsa végre a lekérdezéseket, a kezelők többsége a szigorú szemben támasztott követelményeit.</span><span class="sxs-lookup"><span data-stu-id="21a7c-587">In order to efficiently execute queries, most of the operators have strict type requirements.</span></span> 

<span data-ttu-id="21a7c-588">A DocumentDB API SQL nem hajtható végre implicit konverzió JavaScript eltérően.</span><span class="sxs-lookup"><span data-stu-id="21a7c-588">DocumentDB API SQL doesn't perform implicit conversions, unlike JavaScript.</span></span> <span data-ttu-id="21a7c-589">Például egy lekérdezést, például `SELECT * FROM Person p WHERE p.Age = 21` megegyezik egy kora tulajdonságot, amelynek értéke 21 tartalmazó dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="21a7c-589">For instance, a query like `SELECT * FROM Person p WHERE p.Age = 21` matches documents that contain an Age property whose value is 21.</span></span> <span data-ttu-id="21a7c-590">Bármely más, amelynek kora tulajdonsága egyezést mutat a karakterlánc a "21", vagy más valószínűleg végtelen változata dokumentum, például "021", "21,0", "0021", "00021", nem fog egyeztetni stb.</span><span class="sxs-lookup"><span data-stu-id="21a7c-590">Any other document whose Age property matches string "21", or other possibly infinite variations like "021", "21.0", "0021", "00021", etc. will not be matched.</span></span> <span data-ttu-id="21a7c-591">Ez a számára a JavaScript-számok implicit módon casted a karakterlánc-értékek esetén ezzel szemben az (pl. operátor szerinti szűrése, alapján: ==).</span><span class="sxs-lookup"><span data-stu-id="21a7c-591">This is in contrast to the JavaScript where the string values are implicitly casted to numbers (based on operator, ex: ==).</span></span> <span data-ttu-id="21a7c-592">Ez a beállítás nem megfelelő DocumentDB API SQL hatékony index számára elengedhetetlen.</span><span class="sxs-lookup"><span data-stu-id="21a7c-592">This choice is crucial for efficient index matching in DocumentDB API SQL.</span></span> 

## <a name="parameterized-sql-queries"></a><span data-ttu-id="21a7c-593">A paraméteres SQL-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="21a7c-593">Parameterized SQL queries</span></span>
<span data-ttu-id="21a7c-594">Cosmos DB lekérdezéseket támogat, a @ notation az ismerős kifejezett paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="21a7c-594">Cosmos DB supports queries with parameters expressed with the familiar @ notation.</span></span> <span data-ttu-id="21a7c-595">A paraméteres SQL hatékony kezelése és escape-karaktersorozat felhasználói bevitelt, megakadályozza az SQL-injektálás az adatok véletlen kitettség biztosít.</span><span class="sxs-lookup"><span data-stu-id="21a7c-595">Parameterized SQL provides robust handling and escaping of user input, preventing accidental exposure of data through SQL injection.</span></span> 

<span data-ttu-id="21a7c-596">Például, hogy a Vezetéknév és címállapot fogad paraméterként, és hajthat végre különböző értékek vezetékneve és a felhasználói bevitel alapján cím állapotát.</span><span class="sxs-lookup"><span data-stu-id="21a7c-596">For example, you can write a query that takes last name and address state as parameters, and then execute it for various values of last name and address state based on user input.</span></span>

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

<span data-ttu-id="21a7c-597">A kérelem majd küldhető Cosmos DB JSON paraméteres például lekérdezésként alább látható.</span><span class="sxs-lookup"><span data-stu-id="21a7c-597">This request can then be sent to Cosmos DB as a parameterized JSON query like shown below.</span></span>

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

<span data-ttu-id="21a7c-598">ELSŐ argumentumának állíthat be például a paraméteres lekérdezés alább látható.</span><span class="sxs-lookup"><span data-stu-id="21a7c-598">The argument to TOP can be set using parameterized queries like shown below.</span></span>

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

<span data-ttu-id="21a7c-599">A paraméterértékek lehet bármely érvényes JSON (karakterláncok, számok, a logikai, null, akkor is igaz, tömbök, vagy beágyazott JSON).</span><span class="sxs-lookup"><span data-stu-id="21a7c-599">Parameter values can be any valid JSON (strings, numbers, Booleans, null, even arrays or nested JSON).</span></span> <span data-ttu-id="21a7c-600">Is mivel Cosmos DB séma nélküli, paraméterek a rendszer nem érvényesíti bármilyen ellen.</span><span class="sxs-lookup"><span data-stu-id="21a7c-600">Also since Cosmos DB is schema-less, parameters are not validated against any type.</span></span>

## <span data-ttu-id="21a7c-601"><a id="BuiltinFunctions"></a>Beépített funkciók</span><span class="sxs-lookup"><span data-stu-id="21a7c-601"><a id="BuiltinFunctions"></a>Built-in functions</span></span>
<span data-ttu-id="21a7c-602">Cosmos DB számos beépített funkciót is támogatja a közös műveleteket, például a felhasználói függvény (UDF) lekérdezéseken belül használható.</span><span class="sxs-lookup"><span data-stu-id="21a7c-602">Cosmos DB also supports a number of built-in functions for common operations, that can be used inside queries like user-defined functions (UDFs).</span></span>

| <span data-ttu-id="21a7c-603">Csoport</span><span class="sxs-lookup"><span data-stu-id="21a7c-603">Function group</span></span>          | <span data-ttu-id="21a7c-604">Műveletek</span><span class="sxs-lookup"><span data-stu-id="21a7c-604">Operations</span></span>                                                                                                                                          |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="21a7c-605">Matematikai funkciók</span><span class="sxs-lookup"><span data-stu-id="21a7c-605">Mathematical functions</span></span>  | <span data-ttu-id="21a7c-606">ABS, felső határ, EXP, EMELET, napló, LOG10, ENERGIAGAZDÁLKODÁSI, CIKLIKUS, bejelentkezési, SQRT, SZÖGLETES, csonk, ARCCOS, ARCSIN, ATAN, ATN2, COS, tűz, fok, PI, radiánban megadott szög, EG és TAN</span><span class="sxs-lookup"><span data-stu-id="21a7c-606">ABS, CEILING, EXP, FLOOR, LOG, LOG10, POWER, ROUND, SIGN, SQRT, SQUARE, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COT, DEGREES, PI, RADIANS, SIN, and TAN</span></span> |
| <span data-ttu-id="21a7c-607">Írja be az ellenőrzési funkciók</span><span class="sxs-lookup"><span data-stu-id="21a7c-607">Type checking functions</span></span> | <span data-ttu-id="21a7c-608">IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED és IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="21a7c-608">IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED, and IS_PRIMITIVE</span></span>                                                           |
| <span data-ttu-id="21a7c-609">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="21a7c-609">String functions</span></span>        | <span data-ttu-id="21a7c-610">CONCAT, tartalmazza, megadott módon VÉGZŐDŐ, INDEX_OF, balra, hossza, alsó, LTRIM, csere, REPLIKÁLJA, NÉVKERESÉSI, jobbra, RTRIM, megadott módon KEZDŐDŐ, SUBSTRING és felső</span><span class="sxs-lookup"><span data-stu-id="21a7c-610">CONCAT, CONTAINS, ENDSWITH, INDEX_OF, LEFT, LENGTH, LOWER, LTRIM, REPLACE, REPLICATE, REVERSE, RIGHT, RTRIM, STARTSWITH, SUBSTRING, and UPPER</span></span>       |
| <span data-ttu-id="21a7c-611">A tömb funkciók</span><span class="sxs-lookup"><span data-stu-id="21a7c-611">Array functions</span></span>         | <span data-ttu-id="21a7c-612">ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH és ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="21a7c-612">ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH, and ARRAY_SLICE</span></span>                                                                                         |
| <span data-ttu-id="21a7c-613">Térbeli funkciók</span><span class="sxs-lookup"><span data-stu-id="21a7c-613">Spatial functions</span></span>       | <span data-ttu-id="21a7c-614">ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID és ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="21a7c-614">ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID, and ST_ISVALIDDETAILED</span></span>                                                                           | 

<span data-ttu-id="21a7c-615">Jelenleg használ egy felhasználói függvény (UDF), amelynek beépített függvény mostantól, ha kell használnia a megfelelő beépített funkciót, akkor lesz futtatásához gyorsabb és hatékonyabb.</span><span class="sxs-lookup"><span data-stu-id="21a7c-615">If you’re currently using a user-defined function (UDF) for which a built-in function is now available, you should use the corresponding built-in function as it is going to be quicker to run and more efficiently.</span></span> 

### <a name="mathematical-functions"></a><span data-ttu-id="21a7c-616">Matematikai funkciók</span><span class="sxs-lookup"><span data-stu-id="21a7c-616">Mathematical functions</span></span>
<span data-ttu-id="21a7c-617">A matematikai funkciók végezhet a számítást, a bemeneti értékek, amelyek argumentumként szolgálnak, és a visszaadandó numerikus érték alapján.</span><span class="sxs-lookup"><span data-stu-id="21a7c-617">The mathematical functions each perform a calculation, based on input values that are provided as arguments, and return a numeric value.</span></span> <span data-ttu-id="21a7c-618">Itt található a támogatott beépített matematikai függvények táblázatát.</span><span class="sxs-lookup"><span data-stu-id="21a7c-618">Here’s a table of supported built-in mathematical functions.</span></span>


| <span data-ttu-id="21a7c-619">Használat</span><span class="sxs-lookup"><span data-stu-id="21a7c-619">Usage</span></span> | <span data-ttu-id="21a7c-620">Leírás</span><span class="sxs-lookup"><span data-stu-id="21a7c-620">Description</span></span> |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="21a7c-621">[[ABS (num_expr)](#bk_abs)</span><span class="sxs-lookup"><span data-stu-id="21a7c-621">[[ABS (num_expr)](#bk_abs)</span></span> | <span data-ttu-id="21a7c-622">A megadott numerikus kifejezés (pozitív) abszolút értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-622">Returns the absolute (positive) value of the specified numeric expression.</span></span> |
| [<span data-ttu-id="21a7c-623">Felső határ (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-623">CEILING (num_expr)</span></span>](#bk_ceiling) | <span data-ttu-id="21a7c-624">A legkisebb egész értéket ad vissza, nagyobb vagy egyenlő a megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="21a7c-624">Returns the smallest integer value greater than, or equal to, the specified numeric expression.</span></span> |
| [<span data-ttu-id="21a7c-625">EMELET (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-625">FLOOR (num_expr)</span></span>](#bk_floor) | <span data-ttu-id="21a7c-626">A legnagyobb egész számot ad vissza kisebb vagy egyenlő, mint a megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="21a7c-626">Returns the largest integer less than or equal to the specified numeric expression.</span></span> |
| [<span data-ttu-id="21a7c-627">EXP (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-627">EXP (num_expr)</span></span>](#bk_exp) | <span data-ttu-id="21a7c-628">A megadott numerikus kifejezés hatványát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-628">Returns the exponent of the specified numeric expression.</span></span> |
| <span data-ttu-id="21a7c-629">[NAPLÓ (num_expr [, Alap])](#bk_log)</span><span class="sxs-lookup"><span data-stu-id="21a7c-629">[LOG (num_expr [,base])](#bk_log)</span></span> | <span data-ttu-id="21a7c-630">A megadott numerikus kifejezés, vagy használja a megadott alapban logaritmusát a természetes alapú logaritmusát adja vissza</span><span class="sxs-lookup"><span data-stu-id="21a7c-630">Returns the natural logarithm of the specified numeric expression, or the logarithm using the specified base</span></span> |
| [<span data-ttu-id="21a7c-631">LOG10 (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-631">LOG10 (num_expr)</span></span>](#bk_log10) | <span data-ttu-id="21a7c-632">A 10-es logaritmikus a megadott numerikus kifejezés értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-632">Returns the base-10 logarithmic value of the specified numeric expression.</span></span> |
| [<span data-ttu-id="21a7c-633">KEREK (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-633">ROUND (num_expr)</span></span>](#bk_round) | <span data-ttu-id="21a7c-634">Egy numerikus érték, a legközelebbi egész értéket kerekítve adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-634">Returns a numeric value, rounded to the closest integer value.</span></span> |
| [<span data-ttu-id="21a7c-635">CSONK (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-635">TRUNC (num_expr)</span></span>](#bk_trunc) | <span data-ttu-id="21a7c-636">Egy numerikus érték, csak az a legközelebbi egész értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-636">Returns a numeric value, truncated to the closest integer value.</span></span> |
| [<span data-ttu-id="21a7c-637">SQRT (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-637">SQRT (num_expr)</span></span>](#bk_sqrt) | <span data-ttu-id="21a7c-638">A megadott numerikus kifejezés négyzetgyökét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-638">Returns the square root of the specified numeric expression.</span></span> |
| [<span data-ttu-id="21a7c-639">NÉGYZETES (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-639">SQUARE (num_expr)</span></span>](#bk_square) | <span data-ttu-id="21a7c-640">Kiszámítja a megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="21a7c-640">Returns the square of the specified numeric expression.</span></span> |
| [<span data-ttu-id="21a7c-641">ENERGIAGAZDÁLKODÁSI (num_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-641">POWER (num_expr, num_expr)</span></span>](#bk_power) | <span data-ttu-id="21a7c-642">A megadott numerikus kifejezés power visszatér a megadott érték.</span><span class="sxs-lookup"><span data-stu-id="21a7c-642">Returns the power of the specified numeric expression to the value specified.</span></span> |
| [<span data-ttu-id="21a7c-643">BEJELENTKEZÉSI (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-643">SIGN (num_expr)</span></span>](#bk_sign) | <span data-ttu-id="21a7c-644">A megadott numerikus kifejezés bejelentkezési értékét (-1, 0, 1) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-644">Returns the sign value (-1, 0, 1) of the specified numeric expression.</span></span> |
| [<span data-ttu-id="21a7c-645">ARCCOS (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-645">ACOS (num_expr)</span></span>](#bk_acos) | <span data-ttu-id="21a7c-646">A szöget adja vissza, az radiánban megadott szög, amelynek koszinusza a megadott numerikus kifejezés; más néven koszinuszát.</span><span class="sxs-lookup"><span data-stu-id="21a7c-646">Returns the angle, in radians, whose cosine is the specified numeric expression; also called arccosine.</span></span> |
| [<span data-ttu-id="21a7c-647">ARCSIN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-647">ASIN (num_expr)</span></span>](#bk_asin) | <span data-ttu-id="21a7c-648">A szög radiánban megadott szög, amelynek szinusza a megadott numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-648">Returns the angle, in radians, whose sine is the specified numeric expression.</span></span> <span data-ttu-id="21a7c-649">Ez rövidítése szinuszát.</span><span class="sxs-lookup"><span data-stu-id="21a7c-649">This is also called arcsine.</span></span> |
| [<span data-ttu-id="21a7c-650">ATAN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-650">ATAN (num_expr)</span></span>](#bk_atan) | <span data-ttu-id="21a7c-651">A szög radiánban megadott szög, amelynek tangense a megadott numerikus kifejezést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-651">Returns the angle, in radians, whose tangent is the specified numeric expression.</span></span> <span data-ttu-id="21a7c-652">Ezt arkusz is nevezik.</span><span class="sxs-lookup"><span data-stu-id="21a7c-652">This is also called arctangent.</span></span> |
| [<span data-ttu-id="21a7c-653">ATN2 (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-653">ATN2 (num_expr)</span></span>](#bk_atn2) | <span data-ttu-id="21a7c-654">A szöget adja vissza, az x tengely pozitív és a pont (y, x), a forrásból a ray közötti radiánban ahol x és y az érték a két megadott lebegőpontos kifejezés.</span><span class="sxs-lookup"><span data-stu-id="21a7c-654">Returns the angle, in radians, between the positive x-axis and the ray from the origin to the point (y, x), where x and y are the values of the two specified float expressions.</span></span> |
| [<span data-ttu-id="21a7c-655">COS (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-655">COS (num_expr)</span></span>](#bk_cos) | <span data-ttu-id="21a7c-656">Koszinuszát trigonometric a megadott szög radiánban, a megadott kifejezésben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-656">Returns the trigonometric cosine of the specified angle, in radians, in the specified expression.</span></span> |
| [<span data-ttu-id="21a7c-657">TŰZ (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-657">COT (num_expr)</span></span>](#bk_cot) | <span data-ttu-id="21a7c-658">A megadott szög trigonometric kotangensét adja meg a megadott numerikus kifejezés radiánban.</span><span class="sxs-lookup"><span data-stu-id="21a7c-658">Returns the trigonometric cotangent of the specified angle, in radians, in the specified numeric expression.</span></span> |
| [<span data-ttu-id="21a7c-659">Fokban megadva (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-659">DEGREES (num_expr)</span></span>](#bk_degrees) | <span data-ttu-id="21a7c-660">A megfelelő szöget adja vissza, az a radiánban megadott szög fokban megadva.</span><span class="sxs-lookup"><span data-stu-id="21a7c-660">Returns the corresponding angle in degrees for an angle specified in radians.</span></span> |
| [<span data-ttu-id="21a7c-661">PI)</span><span class="sxs-lookup"><span data-stu-id="21a7c-661">PI ()</span></span>](#bk_pi) | <span data-ttu-id="21a7c-662">A konstans PI értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-662">Returns the constant value of PI.</span></span> |
| [<span data-ttu-id="21a7c-663">RADIÁNBAN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-663">RADIANS (num_expr)</span></span>](#bk_radians) | <span data-ttu-id="21a7c-664">Vissza a radiánban megadott szög, ha egy numerikus kifejezés fokban, is meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="21a7c-664">Returns radians when a numeric expression, in degrees, is entered.</span></span> |
| [<span data-ttu-id="21a7c-665">EG (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-665">SIN (num_expr)</span></span>](#bk_sin) | <span data-ttu-id="21a7c-666">Szinuszát trigonometric a megadott szög radiánban, a megadott kifejezésben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-666">Returns the trigonometric sine of the specified angle, in radians, in the specified expression.</span></span> |
| [<span data-ttu-id="21a7c-667">TAN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-667">TAN (num_expr)</span></span>](#bk_tan) | <span data-ttu-id="21a7c-668">A bemeneti kifejezést tangensét adja vissza a megadott kifejezésben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-668">Returns the tangent of the input expression, in the specified expression.</span></span> |

<span data-ttu-id="21a7c-669">Például most lekérdezéseket is futtathat a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="21a7c-669">For example, you can now run queries like the following:</span></span>

<span data-ttu-id="21a7c-670">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-670">**Query**</span></span>

    SELECT VALUE ABS(-4)

<span data-ttu-id="21a7c-671">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-671">**Results**</span></span>

    [4]

<span data-ttu-id="21a7c-672">A Cosmos DB funkciók ANSI SQL képest közötti fő különbség a, hogy úgy vannak kialakítva, hogy működnek jól séma nélküli és vegyes séma adatokat.</span><span class="sxs-lookup"><span data-stu-id="21a7c-672">The main difference between Cosmos DB’s functions compared to ANSI SQL is that they are designed to work well with schema-less and mixed schema data.</span></span> <span data-ttu-id="21a7c-673">Például ha egy dokumentum, ahol a Size tulajdonság hiányzik, vagy rendelkezik-e nem numerikus érték, például "Ismeretlen", majd a dokumentum keresztül, a rendszer kihagyja helyett hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-673">For example, if you have a document where the Size property is missing, or has a non-numeric value like “unknown”, then the document is skipped over, instead of returning an error.</span></span>

### <a name="type-checking-functions"></a><span data-ttu-id="21a7c-674">Írja be az ellenőrzési funkciók</span><span class="sxs-lookup"><span data-stu-id="21a7c-674">Type checking functions</span></span>
<span data-ttu-id="21a7c-675">A típus ellenőrzési funkciók lehetővé teszik az SQL-lekérdezések lévő kifejezés típusa.</span><span class="sxs-lookup"><span data-stu-id="21a7c-675">The type checking functions allow you to check the type of an expression within SQL queries.</span></span> <span data-ttu-id="21a7c-676">Típus ellenőrzési funkciók segítségével határozható meg, hogy a dokumentumokat tulajdonságokat típusú változó vagy ismeretlen.</span><span class="sxs-lookup"><span data-stu-id="21a7c-676">Type checking functions can be used to determine the type of properties within documents on the fly when it is variable or unknown.</span></span> <span data-ttu-id="21a7c-677">Ez a táblázat beépített típusa támogatott funkciók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="21a7c-677">Here’s a table of supported built-in type checking functions.</span></span>

<table>
<tr>
  <td><span data-ttu-id="21a7c-678"><strong>Használat</strong></span><span class="sxs-lookup"><span data-stu-id="21a7c-678"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="21a7c-679"><strong>Leírás</strong></span><span class="sxs-lookup"><span data-stu-id="21a7c-679"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="21a7c-680"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="21a7c-680"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (expr)</a></span></span></td>
  <td><span data-ttu-id="21a7c-681">Azt jelzi, hogy ha az érték típusa tömb logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-681">Returns a Boolean indicating if the type of the value is an array.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="21a7c-682"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="21a7c-682"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (expr)</a></span></span></td>
  <td><span data-ttu-id="21a7c-683">Azt jelzi, hogy ha az érték típusa olyan logikai érték logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-683">Returns a Boolean indicating if the type of the value is a Boolean.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="21a7c-684"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="21a7c-684"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (expr)</a></span></span></td>
  <td><span data-ttu-id="21a7c-685">Olyan logikai érték, amely azt jelzi, ha az érték típusa null beolvasása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-685">Returns a Boolean indicating if the type of the value is null.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="21a7c-686"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="21a7c-686"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (expr)</a></span></span></td>
  <td><span data-ttu-id="21a7c-687">Azt jelzi, hogy ha az érték típusa több logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-687">Returns a Boolean indicating if the type of the value is a number.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="21a7c-688"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="21a7c-688"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (expr)</a></span></span></td>
  <td><span data-ttu-id="21a7c-689">Azt jelzi, hogy ha az érték típusa egy JSON-objektum logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-689">Returns a Boolean indicating if the type of the value is a JSON object.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="21a7c-690"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="21a7c-690"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (expr)</a></span></span></td>
  <td><span data-ttu-id="21a7c-691">Azt jelzi, hogy ha az érték típusa karakterlánc logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-691">Returns a Boolean indicating if the type of the value is a string.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="21a7c-692"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="21a7c-692"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (expr)</a></span></span></td>
  <td><span data-ttu-id="21a7c-693">Jelzi, ha a tulajdonság van rendelve egy érték logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-693">Returns a Boolean indicating if the property has been assigned a value.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="21a7c-694"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="21a7c-694"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (expr)</a></span></span></td>
  <td><span data-ttu-id="21a7c-695">Azt jelzi, hogy ha az érték típusa karakterlánc, szám, logikai érték vagy null logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-695">Returns a Boolean indicating if the type of the value is a string, number, Boolean or null.</span></span></td>
</tr>

</table>

<span data-ttu-id="21a7c-696">Ezeket a funkciókat használ, most lekérdezéseket is futtathat a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="21a7c-696">Using these functions, you can now run queries like the following:</span></span>

<span data-ttu-id="21a7c-697">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-697">**Query**</span></span>

    SELECT VALUE IS_NUMBER(-4)

<span data-ttu-id="21a7c-698">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-698">**Results**</span></span>

    [true]

### <a name="string-functions"></a><span data-ttu-id="21a7c-699">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="21a7c-699">String functions</span></span>
<span data-ttu-id="21a7c-700">A következő skaláris függvények végrehajtania egy műveletet a bemeneti karakterlánc-értékkel, és a karakterlánc, a numerikus és logikai értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-700">The following scalar functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span> <span data-ttu-id="21a7c-701">Itt a következő táblázat a beépített karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="21a7c-701">Here's a table of built-in string functions:</span></span>

| <span data-ttu-id="21a7c-702">Használat</span><span class="sxs-lookup"><span data-stu-id="21a7c-702">Usage</span></span> | <span data-ttu-id="21a7c-703">Leírás</span><span class="sxs-lookup"><span data-stu-id="21a7c-703">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="21a7c-704">A hossz (str_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-704">LENGTH (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length) |<span data-ttu-id="21a7c-705">A megadott karakterlánc-kifejezés karakterek számát adja vissza</span><span class="sxs-lookup"><span data-stu-id="21a7c-705">Returns the number of characters of the specified string expression</span></span> |
| <span data-ttu-id="21a7c-706">[CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)</span><span class="sxs-lookup"><span data-stu-id="21a7c-706">[CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)</span></span> |<span data-ttu-id="21a7c-707">Karakterlánc, amely legalább két karakterlánc-értékek hozzáfűzésével eredményét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-707">Returns a string that is the result of concatenating two or more string values.</span></span> |
| [<span data-ttu-id="21a7c-708">SUBSTRING (str_expr, num_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-708">SUBSTRING (str_expr, num_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring) |<span data-ttu-id="21a7c-709">Egy karakterlánc-kifejezés részét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-709">Returns part of a string expression.</span></span> |
| [<span data-ttu-id="21a7c-710">(Str_expr, str_expr) startswith ELEMNEK</span><span class="sxs-lookup"><span data-stu-id="21a7c-710">STARTSWITH (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith) |<span data-ttu-id="21a7c-711">Adja vissza egy logikai, amely jelzi, hogy az első karakterlánc-kifejezés a második végződik</span><span class="sxs-lookup"><span data-stu-id="21a7c-711">Returns a Boolean indicating whether the first string expression ends with the second</span></span> |
| [<span data-ttu-id="21a7c-712">Megadott módon VÉGZŐDŐ (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-712">ENDSWITH (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith) |<span data-ttu-id="21a7c-713">Adja vissza egy logikai, amely jelzi, hogy az első karakterlánc-kifejezés a második végződik</span><span class="sxs-lookup"><span data-stu-id="21a7c-713">Returns a Boolean indicating whether the first string expression ends with the second</span></span> |
| [<span data-ttu-id="21a7c-714">CONTAINS (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-714">CONTAINS (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains) |<span data-ttu-id="21a7c-715">Visszaadja egy logikai, amely jelzi, hogy az első karakterlánc-kifejezés tartalmazza a második.</span><span class="sxs-lookup"><span data-stu-id="21a7c-715">Returns a Boolean indicating whether the first string expression contains the second.</span></span> |
| [<span data-ttu-id="21a7c-716">INDEX_OF (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-716">INDEX_OF (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of) |<span data-ttu-id="21a7c-717">A második első előfordulásának kezdőpozícióját adja vissza karakterlánc-kifejezés az első megadott karakterlánc-kifejezés vagy -1, ha a karakterlánc nem található.</span><span class="sxs-lookup"><span data-stu-id="21a7c-717">Returns the starting position of the first occurrence of the second string expression within the first specified string expression, or -1 if the string is not found.</span></span> |
| [<span data-ttu-id="21a7c-718">LEFT (str_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-718">LEFT (str_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left) |<span data-ttu-id="21a7c-719">A megadott számú karakterből álló karakterlánc bal oldali részét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-719">Returns the left part of a string with the specified number of characters.</span></span> |
| [<span data-ttu-id="21a7c-720">JOBB (str_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-720">RIGHT (str_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right) |<span data-ttu-id="21a7c-721">A megadott számú karakterből álló karakterlánc jobb oldali részét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-721">Returns the right part of a string with the specified number of characters.</span></span> |
| [<span data-ttu-id="21a7c-722">LTRIM (str_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-722">LTRIM (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim) |<span data-ttu-id="21a7c-723">Egy karakterlánc-kifejezés adja vissza, után eltávolítja a kezdő üres.</span><span class="sxs-lookup"><span data-stu-id="21a7c-723">Returns a string expression after it removes leading blanks.</span></span> |
| [<span data-ttu-id="21a7c-724">RTRIM (str_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-724">RTRIM (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim) |<span data-ttu-id="21a7c-725">Egy karakterlánc-kifejezés az összes záró szóközöket csonkítása után adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-725">Returns a string expression after truncating all trailing blanks.</span></span> |
| [<span data-ttu-id="21a7c-726">ALSÓ (str_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-726">LOWER (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower) |<span data-ttu-id="21a7c-727">Egy karakterlánc-kifejezés után nagybetűt adatok kisbetűssé alakításával adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-727">Returns a string expression after converting uppercase character data to lowercase.</span></span> |
| [<span data-ttu-id="21a7c-728">FELSŐ (str_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-728">UPPER (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper) |<span data-ttu-id="21a7c-729">Egy karakterlánc-kifejezés után kisbetűt adatok nagybetűssé alakításával adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-729">Returns a string expression after converting lowercase character data to uppercase.</span></span> |
| [<span data-ttu-id="21a7c-730">Cserélje le a (str_expr, str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-730">REPLACE (str_expr, str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace) |<span data-ttu-id="21a7c-731">Megadott karakterlánc-érték összes előfordulását lecseréli egy másik karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="21a7c-731">Replaces all occurrences of a specified string value with another string value.</span></span> |
| [<span data-ttu-id="21a7c-732">REPLIKÁLÁS (str_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-732">REPLICATE (str_expr, num_expr)</span></span>](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-reference#bk_replicate) |<span data-ttu-id="21a7c-733">A megadott számú alkalommal megismétel egy karakterlánc-érték.</span><span class="sxs-lookup"><span data-stu-id="21a7c-733">Repeats a string value a specified number of times.</span></span> |
| [<span data-ttu-id="21a7c-734">NÉVKERESÉSI (str_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-734">REVERSE (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse) |<span data-ttu-id="21a7c-735">A Fordított sorrend egy karakterlánc értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-735">Returns the reverse order of a string value.</span></span> |

<span data-ttu-id="21a7c-736">Ezeket a funkciókat használ, most lekérdezéseket is futtathat a következőhöz hasonló.</span><span class="sxs-lookup"><span data-stu-id="21a7c-736">Using these functions, you can now run queries like the following.</span></span> <span data-ttu-id="21a7c-737">Például lépjen vissza a családnevet nagybetűs az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="21a7c-737">For example, you can return the family name in uppercase as follows:</span></span>

<span data-ttu-id="21a7c-738">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-738">**Query**</span></span>

    SELECT VALUE UPPER(Families.id)
    FROM Families

<span data-ttu-id="21a7c-739">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-739">**Results**</span></span>

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

<span data-ttu-id="21a7c-740">Vagy ebben a példában például karakterlánc összefűzésére:</span><span class="sxs-lookup"><span data-stu-id="21a7c-740">Or concatenate strings like in this example:</span></span>

<span data-ttu-id="21a7c-741">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-741">**Query**</span></span>

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

<span data-ttu-id="21a7c-742">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-742">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


<span data-ttu-id="21a7c-743">Karakterlánc is használható a WHERE záradékban szűrése eredményeket, például a következő példa:</span><span class="sxs-lookup"><span data-stu-id="21a7c-743">String functions can also be used in the WHERE clause to filter results, like in the following example:</span></span>

<span data-ttu-id="21a7c-744">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-744">**Query**</span></span>

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

<span data-ttu-id="21a7c-745">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-745">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a><span data-ttu-id="21a7c-746">A tömb funkciók</span><span class="sxs-lookup"><span data-stu-id="21a7c-746">Array functions</span></span>
<span data-ttu-id="21a7c-747">A következő skaláris függvények végrehajtania egy műveletet a egy tömb bemeneti érték és a numerikus visszatérési, a logikai érték vagy tömb érték.</span><span class="sxs-lookup"><span data-stu-id="21a7c-747">The following scalar functions perform an operation on an array input value and return numeric, Boolean or array value.</span></span> <span data-ttu-id="21a7c-748">Beépített tömb függvények táblázatát itt található:</span><span class="sxs-lookup"><span data-stu-id="21a7c-748">Here's a table of built-in array functions:</span></span>

| <span data-ttu-id="21a7c-749">Használat</span><span class="sxs-lookup"><span data-stu-id="21a7c-749">Usage</span></span> | <span data-ttu-id="21a7c-750">Leírás</span><span class="sxs-lookup"><span data-stu-id="21a7c-750">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="21a7c-751">ARRAY_LENGTH (arr_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-751">ARRAY_LENGTH (arr_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length) |<span data-ttu-id="21a7c-752">A megadott tömb kifejezés elemek számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-752">Returns the number of elements of the specified array expression.</span></span> |
| <span data-ttu-id="21a7c-753">[ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)</span><span class="sxs-lookup"><span data-stu-id="21a7c-753">[ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)</span></span> |<span data-ttu-id="21a7c-754">Olyan tömb, amely két vagy több tömb értékek hozzáfűzésével eredményét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-754">Returns an array that is the result of concatenating two or more array values.</span></span> |
| <span data-ttu-id="21a7c-755">[ARRAY_CONTAINS (arr_expr, kifejezés [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)</span><span class="sxs-lookup"><span data-stu-id="21a7c-755">[ARRAY_CONTAINS (arr_expr, expr [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)</span></span> |<span data-ttu-id="21a7c-756">Jelzi, hogy a tömb tartalmaz-e a megadott érték logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-756">Returns a Boolean indicating whether the array contains the specified value.</span></span> <span data-ttu-id="21a7c-757">Ha az egyezés-e a teljes vagy részleges adhat meg.</span><span class="sxs-lookup"><span data-stu-id="21a7c-757">Can specify if the match is full or partial.</span></span> |
| <span data-ttu-id="21a7c-758">[ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)</span><span class="sxs-lookup"><span data-stu-id="21a7c-758">[ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)</span></span> |<span data-ttu-id="21a7c-759">Egy tömböt megadó kifejezést részét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-759">Returns part of an array expression.</span></span> |

<span data-ttu-id="21a7c-760">Tömb funkciók segítségével kezelheti a tömbök JSON belül.</span><span class="sxs-lookup"><span data-stu-id="21a7c-760">Array functions can be used to manipulate arrays within JSON.</span></span> <span data-ttu-id="21a7c-761">Például itt található összes dokumentum visszaadó, ahol a szülők egyik "Multiplexelés Wakefield".</span><span class="sxs-lookup"><span data-stu-id="21a7c-761">For example, here's a query that returns all documents where one of the parents is "Robin Wakefield".</span></span> 

<span data-ttu-id="21a7c-762">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-762">**Query**</span></span>

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

<span data-ttu-id="21a7c-763">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-763">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="21a7c-764">Megadhatja az egyező elemek a tömbön belüli részleges töredéket.</span><span class="sxs-lookup"><span data-stu-id="21a7c-764">You can specify a partial fragment for matching elements within the array.</span></span> <span data-ttu-id="21a7c-765">A következő lekérdezés megkeresi az összes szülők a `givenName` a `Robin`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-765">The following query finds all parents with the `givenName` of `Robin`.</span></span>

<span data-ttu-id="21a7c-766">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-766">**Query**</span></span>

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)

<span data-ttu-id="21a7c-767">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-767">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]


<span data-ttu-id="21a7c-768">Ez például akkor ARRAY_LENGTH használatával gyermekek termékcsalád másodpercenkénti számát.</span><span class="sxs-lookup"><span data-stu-id="21a7c-768">Here's another example that uses ARRAY_LENGTH to get the number of children per family.</span></span>

<span data-ttu-id="21a7c-769">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-769">**Query**</span></span>

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

<span data-ttu-id="21a7c-770">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-770">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a><span data-ttu-id="21a7c-771">Térbeli funkciók</span><span class="sxs-lookup"><span data-stu-id="21a7c-771">Spatial functions</span></span>
<span data-ttu-id="21a7c-772">Cosmos DB földrajzi lekérdezése a következő nyissa meg a földrajzi konzorcium (OGC) beépített funkciókat támogatja.</span><span class="sxs-lookup"><span data-stu-id="21a7c-772">Cosmos DB supports the following Open Geospatial Consortium (OGC) built-in functions for geospatial querying.</span></span> 

<table>
<tr>
  <td><span data-ttu-id="21a7c-773"><strong>Használat</strong></span><span class="sxs-lookup"><span data-stu-id="21a7c-773"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="21a7c-774"><strong>Leírás</strong></span><span class="sxs-lookup"><span data-stu-id="21a7c-774"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="21a7c-775">ST_DISTANCE (point_expr, point_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-775">ST_DISTANCE (point_expr, point_expr)</span></span></td>
  <td><span data-ttu-id="21a7c-776">Csoport távolságát adja vissza a két GeoJSON-pont, sokszög vagy LineString kifejezések között.</span><span class="sxs-lookup"><span data-stu-id="21a7c-776">Returns the distance between the two GeoJSON Point, Polygon, or LineString expressions.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="21a7c-777">ST_WITHIN (point_expr, polygon_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-777">ST_WITHIN (point_expr, polygon_expr)</span></span></td>
  <td><span data-ttu-id="21a7c-778">Egy logikai kifejezés jelző az első GeoJSON-objektum (pont, sokszög vagy LineString) objektumban a második GeoJSON (pont, sokszög vagy LineString) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-778">Returns a Boolean expression indicating whether the first GeoJSON object (Point, Polygon, or LineString) is within the second GeoJSON object (Point, Polygon, or LineString).</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="21a7c-779">ST_INTERSECTS (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="21a7c-779">ST_INTERSECTS (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="21a7c-780">Egy logikai kifejezés, amely azt jelzi, hogy a két megadott GeoJSON objektumokat (pont, Polygon, vagy LineString) intersect adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-780">Returns a Boolean expression indicating whether the two specified GeoJSON objects (Point, Polygon, or LineString) intersect.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="21a7c-781">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="21a7c-781">ST_ISVALID</span></span></td>
  <td><span data-ttu-id="21a7c-782">Azt jelzi, hogy, hogy a megadott GeoJSON-pont, sokszög vagy LineString kifejezés érvényes logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-782">Returns a Boolean value indicating whether the specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="21a7c-783">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="21a7c-783">ST_ISVALIDDETAILED</span></span></td>
  <td><span data-ttu-id="21a7c-784">Egy olyan logikai érték tartalmazó JSON-érték. Ha a megadott GeoJSON-pont, sokszög vagy LineString kifejezés érvényes, és ha érvénytelen értéket adja vissza, továbbá karakterláncként okát.</span><span class="sxs-lookup"><span data-stu-id="21a7c-784">Returns a JSON value containing a Boolean value if the specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally the reason as a string value.</span></span></td>
</tr>
</table>

<span data-ttu-id="21a7c-785">Térbeli funkciók térbeli adatok közelségi kapcsolat lekérdezések végrehajtásához használható.</span><span class="sxs-lookup"><span data-stu-id="21a7c-785">Spatial functions can be used to perform proximity queries against spatial data.</span></span> <span data-ttu-id="21a7c-786">Például ez visszaadó 30 km-ST_DISTANCE beépített funkcióval a megadott helyen belüli összes termékcsalád dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="21a7c-786">For example, here's a query that returns all family documents that are within 30 km of the specified location using the ST_DISTANCE built-in function.</span></span> 

<span data-ttu-id="21a7c-787">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-787">**Query**</span></span>

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

<span data-ttu-id="21a7c-788">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-788">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="21a7c-789">A földrajzi támogatásáról Cosmos DB további részletekért lásd: [földrajzi adatok az Azure Cosmos DB](geospatial.md).</span><span class="sxs-lookup"><span data-stu-id="21a7c-789">For more details on geospatial support in Cosmos DB, please see [Working with geospatial data in Azure Cosmos DB](geospatial.md).</span></span> <span data-ttu-id="21a7c-790">Amely foglalja össze a térbeli függvények, és az SQL-szintaxis, a Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="21a7c-790">That wraps up spatial functions, and the SQL syntax for Cosmos DB.</span></span> <span data-ttu-id="21a7c-791">Most vessen egy pillantást, hogyan működik, és hogy milyen hatással a használatával lekérdezése LINQ is láttuk eddig.</span><span class="sxs-lookup"><span data-stu-id="21a7c-791">Now let's take a look at how LINQ querying works and how it interacts with the syntax we've seen so far.</span></span>

## <span data-ttu-id="21a7c-792"><a id="Linq"></a>A DocumentDB API SQL LINQ</span><span class="sxs-lookup"><span data-stu-id="21a7c-792"><a id="Linq"></a>LINQ to DocumentDB API SQL</span></span>
<span data-ttu-id="21a7c-793">LINQ .NET programozási modell, amely szerint az objektumok adatfolyamok lekérdezései számítási kifejezze.</span><span class="sxs-lookup"><span data-stu-id="21a7c-793">LINQ is a .NET programming model that expresses computation as queries on streams of objects.</span></span> <span data-ttu-id="21a7c-794">Cosmos DB egy ügyféloldali szalagtár LINQ illesztőfelület biztosít a JSON és a .NET-objektumok és a leképezés egy LINQ-lekérdezések részét csak akkor Cosmos DB lekérdezések közötti konverzió megkönnyítésével.</span><span class="sxs-lookup"><span data-stu-id="21a7c-794">Cosmos DB provides a client-side library to interface with LINQ by facilitating a conversion between JSON and .NET objects and a mapping from a subset of LINQ queries to Cosmos DB queries.</span></span> 

<span data-ttu-id="21a7c-795">Az alábbi képen a LINQ-lekérdezések Cosmos DB használatával architektúráját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="21a7c-795">The picture below shows the architecture of supporting LINQ queries using Cosmos DB.</span></span>  <span data-ttu-id="21a7c-796">A Cosmos DB-ügyfélprogram segítségével a fejlesztők hozhat létre egy **IQueryable** objektum, amely közvetlenül a Cosmos DB lekérdezés szolgáltatót, majd a LINQ lekérdezés fordítja le egy Cosmos-adatbázis-lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="21a7c-796">Using the Cosmos DB client, developers can create an **IQueryable** object that directly queries the Cosmos DB query provider, which then translates the LINQ query into a Cosmos DB query.</span></span> <span data-ttu-id="21a7c-797">A lekérdezés majd kerülnek a Cosmos DB kiszolgálót egy halmazát, az eredmények JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="21a7c-797">The query is then passed to the Cosmos DB server to retrieve a set of results in JSON format.</span></span> <span data-ttu-id="21a7c-798">A keresés eredményeit azokat az ügyféloldali .NET objektumok adatfolyam vannak deszerializálni.</span><span class="sxs-lookup"><span data-stu-id="21a7c-798">The returned results are deserialized into a stream of .NET objects on the client side.</span></span>

![A LINQ-lekérdezések használata a DocumentDB API - SQL-szintaxis, JSON lekérdező nyelv, adatbázis fogalmait és az SQL-lekérdezések architektúrája][1]

### <a name="net-and-json-mapping"></a><span data-ttu-id="21a7c-800">.NET és a JSON-leképezés</span><span class="sxs-lookup"><span data-stu-id="21a7c-800">.NET and JSON mapping</span></span>
<span data-ttu-id="21a7c-801">A .NET-objektumokat és a JSON-dokumentumok közötti leképezéseket természetes – minden tag adatmező van rendelve egy JSON-objektum, ahol a mező neve az objektum "kulcsot" része van leképezve, és a "érték" része rekurzív módon leképezve az objektum érték részét.</span><span class="sxs-lookup"><span data-stu-id="21a7c-801">The mapping between .NET objects and JSON documents is natural - each data member field is mapped to a JSON object, where the field name is mapped to the "key" part of the object and the "value" part is recursively mapped to the value part of the object.</span></span> <span data-ttu-id="21a7c-802">Vegye figyelembe az alábbi példa: A család objektum létrehozása a JSON-dokumentumhoz van rendelve, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="21a7c-802">Consider the following example: The Family object created is mapped to the JSON document as shown below.</span></span> <span data-ttu-id="21a7c-803">És ez fordítva is igaz, a JSON-dokumentumhoz van rendelve vissza egy .NET-objektum.</span><span class="sxs-lookup"><span data-stu-id="21a7c-803">And vice versa, the JSON document is mapped back to a .NET object.</span></span>

<span data-ttu-id="21a7c-804">**C#-osztály**</span><span class="sxs-lookup"><span data-stu-id="21a7c-804">**C# Class**</span></span>

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };

    public struct Parent
    {
        public string familyName;
        public string givenName;
    };

    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };

    public class Pet
    {
        public string givenName;
    };

    public class Address
    {
        public string state;
        public string county;
        public string city;
    };

    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


<span data-ttu-id="21a7c-805">**JSON**</span><span class="sxs-lookup"><span data-stu-id="21a7c-805">**JSON**</span></span>  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-to-sql-translation"></a><span data-ttu-id="21a7c-806">"LINQ to SQL fordítási"</span><span class="sxs-lookup"><span data-stu-id="21a7c-806">LINQ to SQL translation</span></span>
<span data-ttu-id="21a7c-807">A Cosmos DB lekérdezésszolgáltató hajt végre, egy Cosmos-adatbázis SQL-lekérdezést az elérhető legjobb leképezéseket a LINQ lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="21a7c-807">The Cosmos DB query provider performs a best effort mapping from a LINQ query into a Cosmos DB SQL query.</span></span> <span data-ttu-id="21a7c-808">A következő leírásában feltételezzük, hogy az olvasó rendelkezik a LINQ alapszintű ismeretét.</span><span class="sxs-lookup"><span data-stu-id="21a7c-808">In the following description, we assume the reader has a basic familiarity of LINQ.</span></span>

<span data-ttu-id="21a7c-809">Először a típus rendszer esetében támogatott összes JSON egyszerű típusokhoz – numerikus típusok, logikai érték, karakterlánc vagy null.</span><span class="sxs-lookup"><span data-stu-id="21a7c-809">First, for the type system, we support all JSON primitive types – numeric types, boolean, string, and null.</span></span> <span data-ttu-id="21a7c-810">Ezek a JSON típusok támogatottak.</span><span class="sxs-lookup"><span data-stu-id="21a7c-810">Only these JSON types are supported.</span></span> <span data-ttu-id="21a7c-811">A következő skaláris kifejezések használhatók.</span><span class="sxs-lookup"><span data-stu-id="21a7c-811">The following scalar expressions are supported.</span></span>

* <span data-ttu-id="21a7c-812">Állandó értékek – ezek közé tartozik az egyszerű adattípusok állandó értékek a lekérdezés kiértékelése időpontjában.</span><span class="sxs-lookup"><span data-stu-id="21a7c-812">Constant values – these include constant values of the primitive data types at the time the query is evaluated.</span></span>
* <span data-ttu-id="21a7c-813">Tekintse meg a tulajdonság az objektum vagy tömb elem/tulajdonságtömb-index kifejezések – ezek a kifejezések.</span><span class="sxs-lookup"><span data-stu-id="21a7c-813">Property/array index expressions – these expressions refer to the property of an object or an array element.</span></span>
  
     <span data-ttu-id="21a7c-814">termékcsalád. Azonosító;    Family.children[0].familyName;    Family.children[0].grade;    Family.children[n].grade; n egy int változó</span><span class="sxs-lookup"><span data-stu-id="21a7c-814">family.Id;    family.children[0].familyName;    family.children[0].grade;    family.children[n].grade; //n is an int variable</span></span>
* <span data-ttu-id="21a7c-815">Aritmetikai kifejezésekben - ezek közé tartozik a numerikus és logikai értékek a közös aritmetikai kifejezésekben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-815">Arithmetic expressions - These include common arithmetic expressions on numerical and boolean values.</span></span> <span data-ttu-id="21a7c-816">A teljes listát lásd az SQL-specifikációnak.</span><span class="sxs-lookup"><span data-stu-id="21a7c-816">For the complete list, refer to the SQL specification.</span></span>
  
     <span data-ttu-id="21a7c-817">2 * family.children[0].grade;    az x + y;</span><span class="sxs-lookup"><span data-stu-id="21a7c-817">2 * family.children[0].grade;    x + y;</span></span>
* <span data-ttu-id="21a7c-818">Karakterlánc-összehasonlítási kifejezés - ezek közé tartozik egy karakterláncértéket néhány állandó karakterlánc összehasonlítása.</span><span class="sxs-lookup"><span data-stu-id="21a7c-818">String comparison expression - these include comparing a string value to some constant string value.</span></span>  
  
     <span data-ttu-id="21a7c-819">mother.familyName == "Smith";    child.givenName == s; egy karakterlánc-változóvá-je</span><span class="sxs-lookup"><span data-stu-id="21a7c-819">mother.familyName == "Smith";    child.givenName == s; //s is a string variable</span></span>
* <span data-ttu-id="21a7c-820">Objektum vagy tömb létrehozása kifejezés - ezek a kifejezések visszatérési összetett érték vagy névtelen típusú objektum vagy egy ilyen objektumokból álló tömb.</span><span class="sxs-lookup"><span data-stu-id="21a7c-820">Object/array creation expression - these expressions return an object of compound value type or anonymous type or an array of such objects.</span></span> <span data-ttu-id="21a7c-821">Ezek az értékek egymásba ágyazható.</span><span class="sxs-lookup"><span data-stu-id="21a7c-821">These values can be nested.</span></span>
  
     <span data-ttu-id="21a7c-822">új szülő {familyName = a "Smith" givenName = "Joe"}; új {első = 1, a második = 2}; egy két mező rendelkező névtelen típusú</span><span class="sxs-lookup"><span data-stu-id="21a7c-822">new Parent { familyName = "Smith", givenName = "Joe" }; new { first = 1, second = 2 }; //an anonymous type with two fields</span></span>              
     <span data-ttu-id="21a7c-823">új int [] {3, child.grade, 5};</span><span class="sxs-lookup"><span data-stu-id="21a7c-823">new int[] { 3, child.grade, 5 };</span></span>

### <span data-ttu-id="21a7c-824"><a id="SupportedLinqOperators"></a>Támogatott LINQ operátorok listája</span><span class="sxs-lookup"><span data-stu-id="21a7c-824"><a id="SupportedLinqOperators"></a>List of supported LINQ operators</span></span>
<span data-ttu-id="21a7c-825">A LINQ szolgáltatónál tartalmazza a DocumentDB .NET SDK-val támogatott LINQ operátorokat listája itt található.</span><span class="sxs-lookup"><span data-stu-id="21a7c-825">Here is a list of supported LINQ operators in the LINQ provider included with the DocumentDB .NET SDK.</span></span>

* <span data-ttu-id="21a7c-826">**Válassza ki**: leképezések lefordítani az SQL, válassza ki például objektumkonstrukciók</span><span class="sxs-lookup"><span data-stu-id="21a7c-826">**Select**: Projections translate to the SQL SELECT including object construction</span></span>
* <span data-ttu-id="21a7c-827">**Ha**: szűrők lefordítani az SQL WHERE, és támogatja a közötti címfordítás & &, || és!</span><span class="sxs-lookup"><span data-stu-id="21a7c-827">**Where**: Filters translate to the SQL WHERE, and support translation between && , || and !</span></span> <span data-ttu-id="21a7c-828">az SQL-operátorok</span><span class="sxs-lookup"><span data-stu-id="21a7c-828">to the SQL operators</span></span>
* <span data-ttu-id="21a7c-829">**A selectmany metódus**: lehetővé teszi a tömbök számára az SQL JOIN záradékban visszagörgetésének.</span><span class="sxs-lookup"><span data-stu-id="21a7c-829">**SelectMany**: Allows unwinding of arrays to the SQL JOIN clause.</span></span> <span data-ttu-id="21a7c-830">Lánc/nest tömbelemek szűrési kifejezésekben használható</span><span class="sxs-lookup"><span data-stu-id="21a7c-830">Can be used to chain/nest expressions to filter on array elements</span></span>
* <span data-ttu-id="21a7c-831">**OrderBy és OrderByDescending**: az eszköz ORDER BY növekvő/csökkenő</span><span class="sxs-lookup"><span data-stu-id="21a7c-831">**OrderBy and OrderByDescending**: Translates to ORDER BY ascending/descending</span></span>
* <span data-ttu-id="21a7c-832">**Count**, **Sum**, **Min**, **maximális**, és **átlagos** összesítő és a megfelelő aszinkron operátorok **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, és **AverageAsync**.</span><span class="sxs-lookup"><span data-stu-id="21a7c-832">**Count**, **Sum**, **Min**, **Max**, and **Average** operators for aggregation, and their async equivalents **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, and **AverageAsync**.</span></span>
* <span data-ttu-id="21a7c-833">**CompareTo**: tartomány módon történő összehasonlítása az eszköz.</span><span class="sxs-lookup"><span data-stu-id="21a7c-833">**CompareTo**: Translates to range comparisons.</span></span> <span data-ttu-id="21a7c-834">Gyakran használt karakterláncok óta fontosságúak nem hasonlítható össze az .NET</span><span class="sxs-lookup"><span data-stu-id="21a7c-834">Commonly used for strings since they’re not comparable in .NET</span></span>
* <span data-ttu-id="21a7c-835">**Igénybe**: az eszköz egy lekérdezés eredményeként előálló korlátozó SQL felső</span><span class="sxs-lookup"><span data-stu-id="21a7c-835">**Take**: Translates to the SQL TOP for limiting results from a query</span></span>
* <span data-ttu-id="21a7c-836">**Matematikai függvények**: támogatja a fordítás. NET tartozó Abs, ARCCOS, ARCSIN, Atan Cos felső határa, Exp, emelet, napló, Log10, Pow, ciklikus, bejelentkezési, EG, Sqrt, Tan, a megfelelő SQL beépített funkciók Truncate.</span><span class="sxs-lookup"><span data-stu-id="21a7c-836">**Math Functions**: Supports translation from .NET’s Abs, Acos, Asin, Atan, Ceiling, Cos, Exp, Floor, Log, Log10, Pow, Round, Sign, Sin, Sqrt, Tan, Truncate to the equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="21a7c-837">**Karakterlánc**: támogatja a fordítás. NET tartozó Concat, Contains, megadott módon végződő, IndexOf, Count, ToLower, TrimStart, csere, névkeresési, TrimEnd, megadott módon kezdődő, SubString, a megfelelő SQL beépített funkciók ToUpper.</span><span class="sxs-lookup"><span data-stu-id="21a7c-837">**String Functions**: Supports translation from .NET’s Concat, Contains, EndsWith, IndexOf, Count, ToLower, TrimStart, Replace, Reverse, TrimEnd, StartsWith, SubString, ToUpper to the equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="21a7c-838">**A tömb funkciók**: támogatja a fordítás. NET tartozó Concat Contains és számát, hogy a megfelelő SQL beépített funkciók.</span><span class="sxs-lookup"><span data-stu-id="21a7c-838">**Array Functions**: Supports translation from .NET’s Concat, Contains, and Count to the equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="21a7c-839">**A földrajzi Kiterjesztésfüggvények**: támogatja a megfelelő SQL beépített funkciók helyettes módszerek távolság IsValid és IsValidDetailed belül a fordítás.</span><span class="sxs-lookup"><span data-stu-id="21a7c-839">**Geospatial Extension Functions**: Supports translation from stub methods Distance, Within, IsValid, and IsValidDetailed to the equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="21a7c-840">**Felhasználó által definiált függvény kiterjesztésfüggvény**: támogatja a megfelelő felhasználó által definiált függvény a csonkmetódus UserDefinedFunctionProvider.Invoke a fordítás.</span><span class="sxs-lookup"><span data-stu-id="21a7c-840">**User-Defined Function Extension Function**: Supports translation from the stub method UserDefinedFunctionProvider.Invoke to the corresponding user-defined function.</span></span>
* <span data-ttu-id="21a7c-841">**Vegyes**: támogatja a coalesce és feltételes operátort fordítását.</span><span class="sxs-lookup"><span data-stu-id="21a7c-841">**Miscellaneous**: Supports translation of the coalesce and conditional operators.</span></span> <span data-ttu-id="21a7c-842">Lefordíthatja karakterláncot tartalmaz, ARRAY_CONTAINS vagy az SQL-IN attól függően, hogy a környezet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="21a7c-842">Can translate Contains to String CONTAINS, ARRAY_CONTAINS, or the SQL IN depending on context.</span></span>

### <a name="sql-query-operators"></a><span data-ttu-id="21a7c-843">SQL-lekérdezési operátorok</span><span class="sxs-lookup"><span data-stu-id="21a7c-843">SQL query operators</span></span>
<span data-ttu-id="21a7c-844">Íme néhány példa, amelyek bemutatják, hogyan átszámítani egyes szabványos LINQ lekérdezés operátorok az Cosmos DB lekérdezések le.</span><span class="sxs-lookup"><span data-stu-id="21a7c-844">Here are some examples that illustrate how some of the standard LINQ query operators are translated down to Cosmos DB queries.</span></span>

#### <a name="select-operator"></a><span data-ttu-id="21a7c-845">Válasszon operátort</span><span class="sxs-lookup"><span data-stu-id="21a7c-845">Select Operator</span></span>
<span data-ttu-id="21a7c-846">A szintaxis a következő `input.Select(x => f(x))`, ahol `f` egy skaláris kifejezés.</span><span class="sxs-lookup"><span data-stu-id="21a7c-846">The syntax is `input.Select(x => f(x))`, where `f` is a scalar expression.</span></span>

<span data-ttu-id="21a7c-847">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-847">**LINQ lambda expression**</span></span>

    input.Select(family => family.parents[0].familyName);

<span data-ttu-id="21a7c-848">**SQL**</span><span class="sxs-lookup"><span data-stu-id="21a7c-848">**SQL**</span></span> 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



<span data-ttu-id="21a7c-849">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-849">**LINQ lambda expression**</span></span>

    input.Select(family => family.children[0].grade + c); // c is an int variable


<span data-ttu-id="21a7c-850">**SQL**</span><span class="sxs-lookup"><span data-stu-id="21a7c-850">**SQL**</span></span> 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



<span data-ttu-id="21a7c-851">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-851">**LINQ lambda expression**</span></span>

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


<span data-ttu-id="21a7c-852">**SQL**</span><span class="sxs-lookup"><span data-stu-id="21a7c-852">**SQL**</span></span> 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a><span data-ttu-id="21a7c-853">A selectmany metódus operátor</span><span class="sxs-lookup"><span data-stu-id="21a7c-853">SelectMany operator</span></span>
<span data-ttu-id="21a7c-854">A szintaxis a következő `input.SelectMany(x => f(x))`, ahol `f` egy skaláris kifejezés, amely a gyűjteménytípus adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-854">The syntax is `input.SelectMany(x => f(x))`, where `f` is a scalar expression that returns a collection type.</span></span>

<span data-ttu-id="21a7c-855">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-855">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.children);

<span data-ttu-id="21a7c-856">**SQL**</span><span class="sxs-lookup"><span data-stu-id="21a7c-856">**SQL**</span></span> 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a><span data-ttu-id="21a7c-857">Ha operátor</span><span class="sxs-lookup"><span data-stu-id="21a7c-857">Where operator</span></span>
<span data-ttu-id="21a7c-858">A szintaxis a következő `input.Where(x => f(x))`, ahol `f` van egy skaláris kifejezés, amely egy logikai értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-858">The syntax is `input.Where(x => f(x))`, where `f` is a scalar expression, which returns a Boolean value.</span></span>

<span data-ttu-id="21a7c-859">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-859">**LINQ lambda expression**</span></span>

    input.Where(family=> family.parents[0].familyName == "Smith");

<span data-ttu-id="21a7c-860">**SQL**</span><span class="sxs-lookup"><span data-stu-id="21a7c-860">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



<span data-ttu-id="21a7c-861">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-861">**LINQ lambda expression**</span></span>

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

<span data-ttu-id="21a7c-862">**SQL**</span><span class="sxs-lookup"><span data-stu-id="21a7c-862">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a><span data-ttu-id="21a7c-863">Összetett SQL-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="21a7c-863">Composite SQL queries</span></span>
<span data-ttu-id="21a7c-864">A fenti operátorok kell összeállítani, nagyobb teljesítményű lekérdezések kialakításához.</span><span class="sxs-lookup"><span data-stu-id="21a7c-864">The above operators can be composed to form more powerful queries.</span></span> <span data-ttu-id="21a7c-865">Mivel Cosmos DB beágyazott gyűjtemények támogatja, az adott összeállításban kell összefűzendő, vagy beágyazott.</span><span class="sxs-lookup"><span data-stu-id="21a7c-865">Since Cosmos DB supports nested collections, the composition can either be concatenated or nested.</span></span>

#### <a name="concatenation"></a><span data-ttu-id="21a7c-866">Összefűzése</span><span class="sxs-lookup"><span data-stu-id="21a7c-866">Concatenation</span></span>
<span data-ttu-id="21a7c-867">A szintaxis a következő `input(.|.SelectMany())(.Select()|.Where())*`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-867">The syntax is `input(.|.SelectMany())(.Select()|.Where())*`.</span></span> <span data-ttu-id="21a7c-868">Olyan összefűzött lekérdezés elindíthatja és egy opcionális `SelectMany` lekérdezés követ több `Select` vagy `Where` operátorok.</span><span class="sxs-lookup"><span data-stu-id="21a7c-868">A concatenated query can start with an optional `SelectMany` query followed by multiple `Select` or `Where` operators.</span></span>

<span data-ttu-id="21a7c-869">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-869">**LINQ lambda expression**</span></span>

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

<span data-ttu-id="21a7c-870">**SQL**</span><span class="sxs-lookup"><span data-stu-id="21a7c-870">**SQL**</span></span>

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



<span data-ttu-id="21a7c-871">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-871">**LINQ lambda expression**</span></span>

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

<span data-ttu-id="21a7c-872">**SQL**</span><span class="sxs-lookup"><span data-stu-id="21a7c-872">**SQL**</span></span> 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



<span data-ttu-id="21a7c-873">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-873">**LINQ lambda expression**</span></span>

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);

<span data-ttu-id="21a7c-874">**SQL**</span><span class="sxs-lookup"><span data-stu-id="21a7c-874">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



<span data-ttu-id="21a7c-875">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-875">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

<span data-ttu-id="21a7c-876">**SQL**</span><span class="sxs-lookup"><span data-stu-id="21a7c-876">**SQL**</span></span> 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a><span data-ttu-id="21a7c-877">A beágyazási</span><span class="sxs-lookup"><span data-stu-id="21a7c-877">Nesting</span></span>
<span data-ttu-id="21a7c-878">A szintaxis a következő `input.SelectMany(x=>x.Q())` Q esetén egy `Select`, `SelectMany`, vagy `Where` operátor.</span><span class="sxs-lookup"><span data-stu-id="21a7c-878">The syntax is `input.SelectMany(x=>x.Q())` where Q is a `Select`, `SelectMany`, or `Where` operator.</span></span>

<span data-ttu-id="21a7c-879">Egy beágyazott lekérdezésen a belső lekérdezés alkalmazzák a külső gyűjtemény minden eleme.</span><span class="sxs-lookup"><span data-stu-id="21a7c-879">In a nested query, the inner query is applied to each element of the outer collection.</span></span> <span data-ttu-id="21a7c-880">Egyik fontos szolgáltatása, hogy a belső lekérdezés jelentheti a mezőket, például a külső gyűjtemény elemeinek Önillesztések.</span><span class="sxs-lookup"><span data-stu-id="21a7c-880">One important feature is that the inner query can refer to the fields of the elements in the outer collection like self-joins.</span></span>

<span data-ttu-id="21a7c-881">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-881">**LINQ lambda expression**</span></span>

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

<span data-ttu-id="21a7c-882">**SQL**</span><span class="sxs-lookup"><span data-stu-id="21a7c-882">**SQL**</span></span> 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


<span data-ttu-id="21a7c-883">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-883">**LINQ lambda expression**</span></span>

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));

<span data-ttu-id="21a7c-884">**SQL**</span><span class="sxs-lookup"><span data-stu-id="21a7c-884">**SQL**</span></span> 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



<span data-ttu-id="21a7c-885">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-885">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

<span data-ttu-id="21a7c-886">**SQL**</span><span class="sxs-lookup"><span data-stu-id="21a7c-886">**SQL**</span></span> 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <span data-ttu-id="21a7c-887"><a id="ExecutingSqlQueries"></a>SQL-lekérdezések végrehajtása</span><span class="sxs-lookup"><span data-stu-id="21a7c-887"><a id="ExecutingSqlQueries"></a>Executing SQL queries</span></span>
<span data-ttu-id="21a7c-888">A cosmos DB keresztül tesz elérhetővé erőforrásokat egy REST API-t, amely képes a HTTP/HTTPS-kérést bármely olyan nyelvvel hívható.</span><span class="sxs-lookup"><span data-stu-id="21a7c-888">Cosmos DB exposes resources through a REST API that can be called by any language capable of making HTTP/HTTPS requests.</span></span> <span data-ttu-id="21a7c-889">Ezenfelül a Cosmos DB programozási kódtárakat, például a .NET, Node.js, JavaScript és Python számos népszerű nyelvhez biztosít.</span><span class="sxs-lookup"><span data-stu-id="21a7c-889">Additionally, Cosmos DB offers programming libraries for several popular languages like .NET, Node.js, JavaScript, and Python.</span></span> <span data-ttu-id="21a7c-890">A REST API-t és a különböző könyvtárak támogatja keresztül SQL lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="21a7c-890">The REST API and the various libraries all support querying through SQL.</span></span> <span data-ttu-id="21a7c-891">A .NET SDK LINQ lekérdezése SQL mellett támogatja.</span><span class="sxs-lookup"><span data-stu-id="21a7c-891">The .NET SDK supports LINQ querying in addition to SQL.</span></span>

<span data-ttu-id="21a7c-892">A következő példák bemutatják, hogyan hozzon létre egy lekérdezést, és küldje el egy Cosmos-adatbázis adatbázis-fiók.</span><span class="sxs-lookup"><span data-stu-id="21a7c-892">The following examples show how to create a query and submit it against a Cosmos DB database account.</span></span>

### <span data-ttu-id="21a7c-893"><a id="RestAPI"></a>REST API-N</span><span class="sxs-lookup"><span data-stu-id="21a7c-893"><a id="RestAPI"></a>REST API</span></span>
<span data-ttu-id="21a7c-894">Cosmos DB egy megnyitott RESTful programozási modellt biztosít a HTTP Protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="21a7c-894">Cosmos DB offers an open RESTful programming model over HTTP.</span></span> <span data-ttu-id="21a7c-895">Adatbázis-fiókok egy Azure-előfizetés használatával telepíthető.</span><span class="sxs-lookup"><span data-stu-id="21a7c-895">Database accounts can be provisioned using an Azure subscription.</span></span> <span data-ttu-id="21a7c-896">A Cosmos DB erőforrás-modellje egy adatbázis-fiók, amelyek egy-címezhető logikai és állandó URI-k használata alatt lévő erőforrások készlete áll.</span><span class="sxs-lookup"><span data-stu-id="21a7c-896">The Cosmos DB resource model consists of a set of resources under a database account, each  of which is addressable using a logical and stable URI.</span></span> <span data-ttu-id="21a7c-897">Erőforráscsoport ebben a dokumentumban adatcsatornára nevezzük.</span><span class="sxs-lookup"><span data-stu-id="21a7c-897">A set of resources is referred to as a feed in this document.</span></span> <span data-ttu-id="21a7c-898">Az adatbázisfiók áll az adatbázisok, mindegyike több gyűjteményt, mely szolgálna mindegyikének tartalmazza a dokumentumok, a felhasználó által megadott függvények és a más típusú erőforrások.</span><span class="sxs-lookup"><span data-stu-id="21a7c-898">A database account consists of a set of databases, each containing multiple collections, each of which in-turn contain documents, UDFs, and other resource types.</span></span>

<span data-ttu-id="21a7c-899">Az alapvető interakció modell ezekkel az erőforrásokkal keresztül történik a HTTP-műveletek GET, PUT, POST és DELETE a szabványos tolmácsolási szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="21a7c-899">The basic interaction model with these resources is through the HTTP verbs GET, PUT, POST, and DELETE with their standard interpretation.</span></span> <span data-ttu-id="21a7c-900">A POST műveletet egy új erőforrást, egy tárolt eljárás végrehajtása vagy egy Cosmos-adatbázis-lekérdezés kiadására szolgál.</span><span class="sxs-lookup"><span data-stu-id="21a7c-900">The POST verb is used for creation of a new resource, for executing a stored procedure or for issuing a Cosmos DB query.</span></span> <span data-ttu-id="21a7c-901">Lekérdezéseket a rendszer mindig csak olvasható műveletekhez, nincs mellékhatásokkal.</span><span class="sxs-lookup"><span data-stu-id="21a7c-901">Queries are always read-only operations with no side-effects.</span></span>

<span data-ttu-id="21a7c-902">Az alábbi példák bemutatják a FELADÁS egy vagy több, amennyiben azt már áttekintette a két minta dokumentumok tartalmazó gyűjtemény ellen DocumentDB API lekérdezéshez.</span><span class="sxs-lookup"><span data-stu-id="21a7c-902">The following examples show a POST for a DocumentDB API query made against a collection containing the two sample documents we've reviewed so far.</span></span> <span data-ttu-id="21a7c-903">A lekérdezés egy egyszerű szűrési a JSON-name tulajdonsággal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="21a7c-903">The query has a simple filter on the JSON name property.</span></span> <span data-ttu-id="21a7c-904">Vegye figyelembe a használatát a `x-ms-documentdb-isquery` és a Content-Type: `application/query+json` fejlécek, hogy-e a művelet egy lekérdezést jelöléséhez.</span><span class="sxs-lookup"><span data-stu-id="21a7c-904">Note the use of the `x-ms-documentdb-isquery` and Content-Type: `application/query+json` headers to denote that the operation is a query.</span></span>

<span data-ttu-id="21a7c-905">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-905">**Request**</span></span>

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }


<span data-ttu-id="21a7c-906">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-906">**Results**</span></span>

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


<span data-ttu-id="21a7c-907">A második példában egy összetettebb lekérdezés, amely több eredményt ad vissza a való csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="21a7c-907">The second example shows a more complex query that returns multiple results from the join.</span></span>

<span data-ttu-id="21a7c-908">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="21a7c-908">**Request**</span></span>

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


<span data-ttu-id="21a7c-909">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="21a7c-909">**Results**</span></span>

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    <indented for readability, results highlighted>

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


<span data-ttu-id="21a7c-910">Ha a lekérdezés eredményei nem férnek el az eredmények egyoldalas belül, akkor a REST API-t adja vissza a folytatási kód keresztül a `x-ms-continuation-token` válaszfejlécet.</span><span class="sxs-lookup"><span data-stu-id="21a7c-910">If a query's results cannot fit within a single page of results, then the REST API returns a continuation token through the `x-ms-continuation-token` response header.</span></span> <span data-ttu-id="21a7c-911">Az ügyfelek által a további eredmények együtt is megjelenítheti az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="21a7c-911">Clients can paginate results by including the header in subsequent results.</span></span> <span data-ttu-id="21a7c-912">Laponként eredmények száma is szabályozható a `x-ms-max-item-count` számú fejléc.</span><span class="sxs-lookup"><span data-stu-id="21a7c-912">The number of results per page can also be controlled through the `x-ms-max-item-count` number header.</span></span> <span data-ttu-id="21a7c-913">Ha a megadott lekérdezés tartalmaz egy összesítő függvényt, például `COUNT`, akkor a lekérdezés lap egy részben összesített értéket adhat vissza a lap az eredmények.</span><span class="sxs-lookup"><span data-stu-id="21a7c-913">If the specified query has an aggregation function like `COUNT`, then the query page may return a partially aggregated value over the page of results.</span></span> <span data-ttu-id="21a7c-914">Az ügyfelek végre kell hajtania a második szintű összesítő ezekkel az eredményekkel, a végső eredményeket, például, a számát, az eredmény abban az egyes lapok a számuk keresztül összeg keresztül.</span><span class="sxs-lookup"><span data-stu-id="21a7c-914">The clients must perform a second-level aggregation over these results to produce the final results, for example, sum over the counts returned in the individual pages to return the total count.</span></span>

<span data-ttu-id="21a7c-915">A lekérdezések adatok konzisztencia házirend kezeléséhez használja a `x-ms-consistency-level` például minden REST API-kérés fejlécének.</span><span class="sxs-lookup"><span data-stu-id="21a7c-915">To manage the data consistency policy for queries, use the `x-ms-consistency-level` header like all REST API requests.</span></span> <span data-ttu-id="21a7c-916">A munkamenet-konzisztencia esetén szükséges a legutóbbi is echo `x-ms-session-token` Cookie-fejlécének a lekérdezési kérelemben.</span><span class="sxs-lookup"><span data-stu-id="21a7c-916">For session consistency, it is required to also echo the latest `x-ms-session-token` Cookie header in the query request.</span></span> <span data-ttu-id="21a7c-917">A lekérdezett gyűjtemény indexelési házirendet is befolyásolhatják a lekérdezési eredmények konzisztencia.</span><span class="sxs-lookup"><span data-stu-id="21a7c-917">The queried collection's indexing policy can also influence the consistency of query results.</span></span> <span data-ttu-id="21a7c-918">Az alapértelmezett házirend-beállítások indexelő, gyűjtemények a indexe mindig naprakész lesz a dokumentum tartalmát és lekérdezési eredmények felel meg a kiválasztott adatok konzisztencia.</span><span class="sxs-lookup"><span data-stu-id="21a7c-918">With the default indexing policy settings, for collections the index is always current with the document contents and query results match the consistency chosen for data.</span></span> <span data-ttu-id="21a7c-919">Ha az indexelési házirendet Lusta van enyhíteni, majd lekérdezések visszaadhatják a elavult eredmények.</span><span class="sxs-lookup"><span data-stu-id="21a7c-919">If the indexing policy is relaxed to Lazy, then queries can return stale results.</span></span> <span data-ttu-id="21a7c-920">További információkért lásd: [Azure Cosmos DB Konzisztenciaszintek][consistency-levels].</span><span class="sxs-lookup"><span data-stu-id="21a7c-920">For more information, see [Azure Cosmos DB Consistency Levels][consistency-levels].</span></span>

<span data-ttu-id="21a7c-921">Ha a konfigurált indexelési házirendet a gyűjtemény nem támogatja a megadott lekérdezés, az Azure Cosmos adatbázis-kiszolgálót 400 "hibás kérelem" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-921">If the configured indexing policy on the collection cannot support the specified query, the Azure Cosmos DB server returns 400 "Bad Request".</span></span> <span data-ttu-id="21a7c-922">A kivonatoló (egyenlő) keresések, és kifejezetten kizárja indexelő elérési út beállítva elérési utak tartomány lekérdezések esetében adja vissza.</span><span class="sxs-lookup"><span data-stu-id="21a7c-922">This is returned for range queries against paths configured for hash (equality) lookups, and for paths explicitly excluded from indexing.</span></span> <span data-ttu-id="21a7c-923">A `x-ms-documentdb-query-enable-scan` fejléc adható meg a lekérdezést, hogy vizsgálatot végezzen, ha nem érhető el index engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="21a7c-923">The `x-ms-documentdb-query-enable-scan` header can be specified to allow the query to perform a scan when an index is not available.</span></span>

<span data-ttu-id="21a7c-924">Úgy, hogy a lekérdezés-végrehajtás részletes metrikák is ki `x-ms-documentdb-populatequerymetrics` fejlécének `True`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-924">You can get detailed metrics on query execution by setting `x-ms-documentdb-populatequerymetrics` header to `True`.</span></span> <span data-ttu-id="21a7c-925">További információkért lásd: [Azure Cosmos DB DocumentDB API SQL-lekérdezés metrikáját](documentdb-sql-query-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="21a7c-925">For more information, see [SQL query metrics for Azure Cosmos DB DocumentDB API](documentdb-sql-query-metrics.md).</span></span>

### <span data-ttu-id="21a7c-926"><a id="DotNetSdk"></a>C# (.NET) SDK</span><span class="sxs-lookup"><span data-stu-id="21a7c-926"><a id="DotNetSdk"></a>C# (.NET) SDK</span></span>
<span data-ttu-id="21a7c-927">A .NET SDK támogatja a LINQ és az SQL lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="21a7c-927">The .NET SDK supports both LINQ and SQL querying.</span></span> <span data-ttu-id="21a7c-928">A következő példa bemutatja, hogyan hajthat végre a rendszerben jelent meg a jelen dokumentum korábbi egyszerű szűrő lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="21a7c-928">The following example shows how to perform the simple filter query introduced earlier in this document.</span></span>

    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


<span data-ttu-id="21a7c-929">Ez a minta összehasonlítja két tulajdonságainak egyenlőség minden a dokumentumban, és névtelen leképezések használja.</span><span class="sxs-lookup"><span data-stu-id="21a7c-929">This sample compares two properties for equality within each document and uses anonymous projections.</span></span> 

    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


<span data-ttu-id="21a7c-930">A következő példa bemutatja illesztések, LINQ selectmany metódus használatával.</span><span class="sxs-lookup"><span data-stu-id="21a7c-930">The next sample shows joins, expressed through LINQ SelectMany.</span></span>

    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



<span data-ttu-id="21a7c-931">A .NET-ügyfél automatikusan a lekérdezés eredményének a fentiek szerint foreach blokkok oldalain telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="21a7c-931">The .NET client automatically iterates through all the pages of query results in the foreach blocks as shown above.</span></span> <span data-ttu-id="21a7c-932">A REST API szakaszában bemutatott lekérdezési lehetőségek is elérhetők a .NET SDK használatával a `FeedOptions` és `FeedResponse` osztályok CreateDocumentQuery metódus.</span><span class="sxs-lookup"><span data-stu-id="21a7c-932">The query options introduced in the REST API section are also available in the .NET SDK using the `FeedOptions` and `FeedResponse` classes in the CreateDocumentQuery method.</span></span> <span data-ttu-id="21a7c-933">A lapok száma vezérelhető a `MaxItemCount` beállítást.</span><span class="sxs-lookup"><span data-stu-id="21a7c-933">The number of pages can be controlled using the `MaxItemCount` setting.</span></span> 

<span data-ttu-id="21a7c-934">Lapozófájl létrehozásával közvetlenül is szabályozhatja `IDocumentQueryable` használatával a `IQueryable` objektumot, majd ehhez beolvassa a` ResponseContinuationToken` gépet értékeket, és átadja őket `RequestContinuationToken` a `FeedOptions`.</span><span class="sxs-lookup"><span data-stu-id="21a7c-934">You can also explicitly control paging by creating `IDocumentQueryable` using the `IQueryable` object, then by reading the` ResponseContinuationToken` values and passing them back as `RequestContinuationToken` in `FeedOptions`.</span></span> <span data-ttu-id="21a7c-935">`EnableScanInQuery`vizsgálatok engedélyezésére, amikor a lekérdezés nem támogatja a konfigurált indexelési házirend állítható be.</span><span class="sxs-lookup"><span data-stu-id="21a7c-935">`EnableScanInQuery` can be set to enable scans when the query cannot be supported by the configured indexing policy.</span></span> <span data-ttu-id="21a7c-936">A particionált gyűjtemények használhatják `PartitionKey` futtatásához a lekérdezés egyetlen partícióazonosító (bár a Cosmos DB is automatikusan kinyerése Ez a lekérdezés szövegének), és `EnableCrossPartitionQuery` esetleg több partíciót kell futtatni a lekérdezések futtatásához.</span><span class="sxs-lookup"><span data-stu-id="21a7c-936">For partitioned collections, you can use `PartitionKey` to run the query against a single partition (though Cosmos DB can automatically extract this from the query text), and `EnableCrossPartitionQuery` to run queries that may need to be run against multiple partitions.</span></span> 

<span data-ttu-id="21a7c-937">Tekintse meg [Azure Cosmos DB .NET minták](https://github.com/Azure/azure-documentdb-net) további mintákat tartalmazó lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="21a7c-937">Refer to [Azure Cosmos DB .NET samples](https://github.com/Azure/azure-documentdb-net) for more samples containing queries.</span></span> 

### <span data-ttu-id="21a7c-938"><a id="JavaScriptServerSideApi"></a>JavaScript kiszolgálóoldali API</span><span class="sxs-lookup"><span data-stu-id="21a7c-938"><a id="JavaScriptServerSideApi"></a>JavaScript server-side API</span></span>
<span data-ttu-id="21a7c-939">A cosmos DB programozási modellt biztosít a feldolgozás alatt álló alapú JavaScript-alkalmazáslogika közvetlenül a gyűjtemények, tárolt eljárások és eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="21a7c-939">Cosmos DB provides a programming model for executing JavaScript based application logic directly on the collections using stored procedures and triggers.</span></span> <span data-ttu-id="21a7c-940">A JavaScript-logika regisztrálva, a gyűjtemény szintjén majd adhat ki az adott gyűjteményben lévő dokumentumokon működésének Helyadatbázis-műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="21a7c-940">The JavaScript logic registered at a collection level can then issue database operations on the operations on the documents of the given collection.</span></span> <span data-ttu-id="21a7c-941">Ezek a műveletek a környezeti ACID-tranzakciókat van burkolva.</span><span class="sxs-lookup"><span data-stu-id="21a7c-941">These operations are wrapped in ambient ACID transactions.</span></span>

<span data-ttu-id="21a7c-942">A következő példa bemutatja, hogyan lehet a JavaScript-kiszolgáló API a queryDocuments segítségével ellenőrizze a lekérdezések belső tárolt eljárások és eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="21a7c-942">The following example shows how to use the queryDocuments in the JavaScript server API to make queries from inside stored procedures and triggers.</span></span>

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()

        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <span data-ttu-id="21a7c-943"><a id="References"></a>Hivatkozások</span><span class="sxs-lookup"><span data-stu-id="21a7c-943"><a id="References"></a>References</span></span>
1. <span data-ttu-id="21a7c-944">[Bevezetés az Azure Cosmos DB][introduction]</span><span class="sxs-lookup"><span data-stu-id="21a7c-944">[Introduction to Azure Cosmos DB][introduction]</span></span>
2. [<span data-ttu-id="21a7c-945">Az Azure Cosmos adatbázis SQL-specifikációja</span><span class="sxs-lookup"><span data-stu-id="21a7c-945">Azure Cosmos DB SQL specification</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [<span data-ttu-id="21a7c-946">Azure Cosmos DB .NET-minták</span><span class="sxs-lookup"><span data-stu-id="21a7c-946">Azure Cosmos DB .NET samples</span></span>](https://github.com/Azure/azure-documentdb-net)
4. <span data-ttu-id="21a7c-947">[Az Azure Cosmos DB Konzisztenciaszintek][consistency-levels]</span><span class="sxs-lookup"><span data-stu-id="21a7c-947">[Azure Cosmos DB Consistency Levels][consistency-levels]</span></span>
5. <span data-ttu-id="21a7c-948">ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)</span><span class="sxs-lookup"><span data-stu-id="21a7c-948">ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)</span></span>
6. <span data-ttu-id="21a7c-949">JSON [http://json.org/](http://json.org/)</span><span class="sxs-lookup"><span data-stu-id="21a7c-949">JSON [http://json.org/](http://json.org/)</span></span>
7. <span data-ttu-id="21a7c-950">JavaScript-specifikáció [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm)</span><span class="sxs-lookup"><span data-stu-id="21a7c-950">Javascript Specification [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm)</span></span> 
8. <span data-ttu-id="21a7c-951">LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx)</span><span class="sxs-lookup"><span data-stu-id="21a7c-951">LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx)</span></span> 
9. <span data-ttu-id="21a7c-952">Lekérdezés kiértékelése technikák nagy adatbázisokhoz [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)</span><span class="sxs-lookup"><span data-stu-id="21a7c-952">Query evaluation techniques for large databases [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)</span></span>
10. <span data-ttu-id="21a7c-953">A lekérdezés feldolgozás alatt álló párhuzamos relációs adatbázis-rendszerek, IEEE számítógép társadalom nyomja le az 1994.</span><span class="sxs-lookup"><span data-stu-id="21a7c-953">Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994</span></span>
11. <span data-ttu-id="21a7c-954">Lu, Ooi, Tan, feldolgozás alatt álló párhuzamos relációs adatbázis-rendszerek, IEEE számítógép társadalom nyomja le az 1994 lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="21a7c-954">Lu, Ooi, Tan, Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994.</span></span>
12. <span data-ttu-id="21a7c-955">Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: a Pig Latin: egy nem, külső nyelvi SIGMOD 2008 az adatok feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="21a7c-955">Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Pig Latin: A Not-So-Foreign Language for Data Processing, SIGMOD 2008.</span></span>
13. <span data-ttu-id="21a7c-956">G.</span><span class="sxs-lookup"><span data-stu-id="21a7c-956">G.</span></span> <span data-ttu-id="21a7c-957">Graefe.</span><span class="sxs-lookup"><span data-stu-id="21a7c-957">Graefe.</span></span> <span data-ttu-id="21a7c-958">Optimalizálás kaszkádokban keretében.</span><span class="sxs-lookup"><span data-stu-id="21a7c-958">The Cascades framework for query optimization.</span></span> <span data-ttu-id="21a7c-959">IEEE adatok Eng.</span><span class="sxs-lookup"><span data-stu-id="21a7c-959">IEEE Data Eng.</span></span> <span data-ttu-id="21a7c-960">BULL., 18(3): 1995.</span><span class="sxs-lookup"><span data-stu-id="21a7c-960">Bull., 18(3): 1995.</span></span>

[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md