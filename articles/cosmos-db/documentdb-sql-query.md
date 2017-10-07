---
title: "Azure Cosmos DB DocumentDB API aaaSQL lekérdezések |} Microsoft Docs"
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
ms.openlocfilehash: f4db95b87f5796c4e4299aaf016435cb6301bbfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-queries-for-azure-cosmos-db-documentdb-api"></a><span data-ttu-id="efabd-105">Az SQL-lekérdezések Azure Cosmos DB DocumentDB API-hoz.</span><span class="sxs-lookup"><span data-stu-id="efabd-105">SQL queries for Azure Cosmos DB DocumentDB API</span></span>
<span data-ttu-id="efabd-106">A Microsoft Azure Cosmos DB támogatja a JSON lekérdezésnyelvet SQL (Structured Query Language) használatával dokumentumok lekérdezését.</span><span class="sxs-lookup"><span data-stu-id="efabd-106">Microsoft Azure Cosmos DB supports querying documents using SQL (Structured Query Language) as a JSON query language.</span></span> <span data-ttu-id="efabd-107">A cosmos DB valóban sémamentes.</span><span class="sxs-lookup"><span data-stu-id="efabd-107">Cosmos DB is truly schema-free.</span></span> <span data-ttu-id="efabd-108">A kötelezettségvállalás toohello JSON adatmodell közvetlenül hello adatbázismotor belül, címtár biztosít automatikus indexeléshez JSON-dokumentumok explicit séma vagy a másodlagos indexek létrehozása nélkül.</span><span class="sxs-lookup"><span data-stu-id="efabd-108">By virtue of its commitment toohello JSON data model directly within hello database engine, it provides automatic indexing of JSON documents without requiring explicit schema or creation of secondary indexes.</span></span> 

<span data-ttu-id="efabd-109">A Cosmos DB hello lekérdezési nyelv tervezésekor két célok szem előtt tartásával volt:</span><span class="sxs-lookup"><span data-stu-id="efabd-109">While designing hello query language for Cosmos DB, we had two goals in mind:</span></span>

* <span data-ttu-id="efabd-110">Helyett egy új JSON lekérdező nyelv inventing, akartunk toosupport SQL.</span><span class="sxs-lookup"><span data-stu-id="efabd-110">Instead of inventing a new JSON query language, we wanted toosupport SQL.</span></span> <span data-ttu-id="efabd-111">SQL hello ismerős és a népszerű lekérdezési nyelv egyike.</span><span class="sxs-lookup"><span data-stu-id="efabd-111">SQL is one of hello most familiar and popular query languages.</span></span> <span data-ttu-id="efabd-112">Cosmos DB SQL formális programozási modellt biztosít a részletes lekérdezéseket JSON-dokumentumok keresztül.</span><span class="sxs-lookup"><span data-stu-id="efabd-112">Cosmos DB SQL provides a formal programming model for rich queries over JSON documents.</span></span>
* <span data-ttu-id="efabd-113">JSON-adatbázisként dokumentum, amelyek képesek JavaScript közvetlenül a hello adatbázismotor akartunk toouse JavaScript programozási modell hello foundation, a lekérdezési nyelvhez.</span><span class="sxs-lookup"><span data-stu-id="efabd-113">As a JSON document database capable of executing JavaScript directly in hello database engine, we wanted toouse JavaScript's programming model as hello foundation for our query language.</span></span> <span data-ttu-id="efabd-114">a JavaScript típusrendszernek, kifejezés kiértékelése és függvényhívások feltörték hello DocumentDB API SQL.</span><span class="sxs-lookup"><span data-stu-id="efabd-114">hello DocumentDB API SQL is rooted in JavaScript's type system, expression evaluation, and function invocation.</span></span> <span data-ttu-id="efabd-115">Ez szolgálna természetes programozási modellt biztosít a leképezések relációs, hierarchikus navigációs JSON-dokumentumokat, automatikus illesztések, térbeli lekérdezéseket és teljesen egyéb funkciók között a JavaScript nyelven írt felhasználói függvény (UDF) meghívását.</span><span class="sxs-lookup"><span data-stu-id="efabd-115">This in-turn provides a natural programming model for relational projections, hierarchical navigation across JSON documents, self joins, spatial queries, and invocation of user-defined functions (UDFs) written entirely in JavaScript, among other features.</span></span> 

<span data-ttu-id="efabd-116">Biztosak vagyunk abban, hogy ezeket a képességeket kulcs tooreducing hello súrlódás hello alkalmazás- és hello adatbázis közötti és fontosságúak a fejlesztést tesz lehetővé.</span><span class="sxs-lookup"><span data-stu-id="efabd-116">We believe that these capabilities are key tooreducing hello friction between hello application and hello database and are crucial for developer productivity.</span></span>

<span data-ttu-id="efabd-117">Azt javasoljuk, első lépések a következő videót, amely Aravind Ramachandran mutatja Cosmos DB lekérdező képességek, hello figyeli, és látogasson el a [Tesztlekérdezéseket](http://www.documentdb.com/sql/demo), ahol Cosmos DB kipróbálásához és SQL lekérdezések futtatása az adathalmaz.</span><span class="sxs-lookup"><span data-stu-id="efabd-117">We recommend getting started by watching hello following video, where Aravind Ramachandran shows Cosmos DB's querying capabilities, and by visiting our [Query Playground](http://www.documentdb.com/sql/demo), where you can try out Cosmos DB and run SQL queries against our dataset.</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/DataExposedQueryingDocumentDB/player]
> 
> 

<span data-ttu-id="efabd-118">Ezt követően térjen vissza toothis cikk, ha először egy SQL-lekérdezés oktatóanyag, amely bemutatja, hogyan néhány egyszerű JSON-dokumentumokat és az SQL-parancsokat.</span><span class="sxs-lookup"><span data-stu-id="efabd-118">Then, return toothis article, where we start with a SQL query tutorial that walks you through some simple JSON documents and SQL commands.</span></span>

## <span data-ttu-id="efabd-119"><a id="GettingStarted"></a>Ismerkedés az SQL-parancsokat az Cosmos-Adatbázisba</span><span class="sxs-lookup"><span data-stu-id="efabd-119"><a id="GettingStarted"></a>Getting started with SQL commands in Cosmos DB</span></span>
<span data-ttu-id="efabd-120">toosee Cosmos DB SQL: működnek, most kezdődnie néhány egyszerű JSON-dokumentumok és bízná néhány egyszerű lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="efabd-120">toosee Cosmos DB SQL at work, let's begin with a few simple JSON documents and walk through some simple queries against it.</span></span> <span data-ttu-id="efabd-121">Fontolja meg e két JSON-dokumentumok két családok kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="efabd-121">Consider these two JSON documents about two families.</span></span> <span data-ttu-id="efabd-122">A Cosmos DB azt nem kell toocreate bármely sémák vagy másodlagos indexek explicit módon.</span><span class="sxs-lookup"><span data-stu-id="efabd-122">With Cosmos DB, we do not need toocreate any schemas or secondary indices explicitly.</span></span> <span data-ttu-id="efabd-123">A Microsoft egyszerűen kell tooinsert hello JSON-dokumentumok tooa Cosmos DB gyűjteményben, és ezt követően lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="efabd-123">We simply need tooinsert hello JSON documents tooa Cosmos DB collection and subsequently query.</span></span> <span data-ttu-id="efabd-124">Itt egy egyszerű JSON tudunk dokumentum hello Andersen családhoz, hello szülők, gyermekek (és azok kedvtelésből), címe és regisztrációs adatait.</span><span class="sxs-lookup"><span data-stu-id="efabd-124">Here we have a simple JSON document for hello Andersen family, hello parents, children (and their pets), address, and registration information.</span></span> <span data-ttu-id="efabd-125">hello dokumentumnak karakterláncok, számok, a logikai, tömbök és beágyazott tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="efabd-125">hello document has strings, numbers, Booleans, arrays, and nested properties.</span></span> 

<span data-ttu-id="efabd-126">**A dokumentum**</span><span class="sxs-lookup"><span data-stu-id="efabd-126">**Document**</span></span>  

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

<span data-ttu-id="efabd-127">Ez az egyetlen különbség – a második dokumentum `givenName` és `familyName` helyett használt `firstName` és `lastName`.</span><span class="sxs-lookup"><span data-stu-id="efabd-127">Here's a second document with one subtle difference – `givenName` and `familyName` are used instead of `firstName` and `lastName`.</span></span>

<span data-ttu-id="efabd-128">**A dokumentum**</span><span class="sxs-lookup"><span data-stu-id="efabd-128">**Document**</span></span>  

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

<span data-ttu-id="efabd-129">Most próbáljon néhány lekérdezést az adatok toounderstand elleni néhány hello kulcs DocumentDB API SQL aspektusait.</span><span class="sxs-lookup"><span data-stu-id="efabd-129">Now let's try a few queries against this data toounderstand some of hello key aspects of DocumentDB API SQL.</span></span> <span data-ttu-id="efabd-130">Például a hello alábbi lekérdezés a beolvasása hello dokumentumok Ha hello id mezője megegyezik `AndersenFamily`.</span><span class="sxs-lookup"><span data-stu-id="efabd-130">For example, hello following query returns hello documents where hello id field matches `AndersenFamily`.</span></span> <span data-ttu-id="efabd-131">Mivel ez egy `SELECT *`, hello hello lekérdezés eredménye hello teljes JSON-dokumentum:</span><span class="sxs-lookup"><span data-stu-id="efabd-131">Since it's a `SELECT *`, hello output of hello query is hello complete JSON document:</span></span>

<span data-ttu-id="efabd-132">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-132">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="efabd-133">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-133">**Results**</span></span>

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


<span data-ttu-id="efabd-134">Most pedig nézzük meg hello esetben, ha igazolnia kell tooreformat hello egy másik alakzat JSON-kimenetét.</span><span class="sxs-lookup"><span data-stu-id="efabd-134">Now consider hello case where we need tooreformat hello JSON output in a different shape.</span></span> <span data-ttu-id="efabd-135">Ez a lekérdezés egy új JSON projektek objektum két kijelölt mezővel, nevét és a város, ha hello cím város azonos nevet hello hello állapot.</span><span class="sxs-lookup"><span data-stu-id="efabd-135">This query projects a new JSON object with two selected fields, Name and City, when hello address' city has hello same name as hello state.</span></span> <span data-ttu-id="efabd-136">Ebben az esetben a "NY, NY" megegyezik.</span><span class="sxs-lookup"><span data-stu-id="efabd-136">In this case, "NY, NY" matches.</span></span>

<span data-ttu-id="efabd-137">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-137">**Query**</span></span>    

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

<span data-ttu-id="efabd-138">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-138">**Results**</span></span>

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


<span data-ttu-id="efabd-139">hello tovább lekérdezés visszaadja az összes hello nevet kapnak gyermekek hello termékcsalád, amelynek azonosítója megegyezik `WakefieldFamily` hello város tartózkodási helye alapján rendezve.</span><span class="sxs-lookup"><span data-stu-id="efabd-139">hello next query returns all hello given names of children in hello family whose id matches `WakefieldFamily` ordered by hello city of residence.</span></span>

<span data-ttu-id="efabd-140">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-140">**Query**</span></span>

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

<span data-ttu-id="efabd-141">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-141">**Results**</span></span>

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


<span data-ttu-id="efabd-142">Szeretnénk toodraw figyelmet tooa hello Cosmos DB néhány fontos aspektusainak lekérdezési nyelv eddig is láttuk hello példákból:</span><span class="sxs-lookup"><span data-stu-id="efabd-142">We would like toodraw attention tooa few noteworthy aspects of hello Cosmos DB query language through hello examples we've seen so far:</span></span>  

* <span data-ttu-id="efabd-143">Mivel a DocumentDB API SQL JSON értékek működéséről, alakú sorok és oszlopok helyett entitások fa foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="efabd-143">Since DocumentDB API SQL works on JSON values, it deals with tree shaped entities instead of rows and columns.</span></span> <span data-ttu-id="efabd-144">Ezért hello nyelv lehetővé teszi, hogy tekintse meg a hello fa bármilyen tetszőleges mélységben toonodes például `Node1.Node2.Node3…..Nodem`, hasonló toorelational SQL hivatkozó toohello két rész hivatkozását `<table>.<column>`.</span><span class="sxs-lookup"><span data-stu-id="efabd-144">Therefore, hello language lets you refer toonodes of hello tree at any arbitrary depth, like `Node1.Node2.Node3…..Nodem`, similar toorelational SQL referring toohello two part reference of `<table>.<column>`.</span></span>   
* <span data-ttu-id="efabd-145">hello strukturált lekérdezési nyelv works séma nélküli adatokkal.</span><span class="sxs-lookup"><span data-stu-id="efabd-145">hello structured query language works with schema-less data.</span></span> <span data-ttu-id="efabd-146">Ezért hello dinamikusan típus rendszer igényeinek toobe kötött.</span><span class="sxs-lookup"><span data-stu-id="efabd-146">Therefore, hello type system needs toobe bound dynamically.</span></span> <span data-ttu-id="efabd-147">hello ugyanabban a kifejezésben sikerült yield különböző típusaihoz különböző dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="efabd-147">hello same expression could yield different types on different documents.</span></span> <span data-ttu-id="efabd-148">a lekérdezések eredménye hello egy érvényes JSON-érték, de nem garantált toobe rögzített séma.</span><span class="sxs-lookup"><span data-stu-id="efabd-148">hello result of a query is a valid JSON value, but is not guaranteed toobe of a fixed schema.</span></span>  
* <span data-ttu-id="efabd-149">Cosmos DB csak szigorú JSON-dokumentumokat támogat.</span><span class="sxs-lookup"><span data-stu-id="efabd-149">Cosmos DB only supports strict JSON documents.</span></span> <span data-ttu-id="efabd-150">Ez azt jelenti, hello típusrendszernek és kifejezések korlátozott toodeal csak a JSON típusával.</span><span class="sxs-lookup"><span data-stu-id="efabd-150">This means hello type system and expressions are restricted toodeal only with JSON types.</span></span> <span data-ttu-id="efabd-151">Tekintse meg a toohello [JSON-specifikáció](http://www.json.org/) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="efabd-151">Refer toohello [JSON specification](http://www.json.org/) for more details.</span></span>  
* <span data-ttu-id="efabd-152">A Cosmos DB gyűjtemény egy olyan sémamentes tároló JSON-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="efabd-152">A Cosmos DB collection is a schema-free container of JSON documents.</span></span> <span data-ttu-id="efabd-153">tartalmazási, és nem a primary key és idegen kulcs kapcsolatokat a rendszer implicit módon rögzíti hello kapcsolatokat az entitások belül, és egy gyűjtemény dokumentumok között.</span><span class="sxs-lookup"><span data-stu-id="efabd-153">hello relations in data entities within and across documents in a collection are implicitly captured by containment and not by primary key and foreign key relations.</span></span> <span data-ttu-id="efabd-154">Ez egy fontos eleme érdemes alapján hello intra-dokumentum illesztések ebben a cikkben korábban tárgyalt mutat.</span><span class="sxs-lookup"><span data-stu-id="efabd-154">This is an important aspect worth pointing out in light of hello intra-document joins discussed later in this article.</span></span>

## <span data-ttu-id="efabd-155"><a id="Indexing"></a>A cosmos DB indexelő</span><span class="sxs-lookup"><span data-stu-id="efabd-155"><a id="Indexing"></a> Cosmos DB indexing</span></span>
<span data-ttu-id="efabd-156">Hello DocumentDB API SQL-szintaxis feltölti azt, mielőtt Cosmos DB tervéhez indexelő hello felfedezése érdemes.</span><span class="sxs-lookup"><span data-stu-id="efabd-156">Before we get into hello DocumentDB API SQL syntax, it is worth exploring hello indexing design in Cosmos DB.</span></span> 

<span data-ttu-id="efabd-157">hello adatbázisa indexei célja a különböző űrlapok és minimális erőforrás-használat (például CPU és a bemeneti/kimeneti) tartalmazó alakzatok tooserve lekérdezések ugyanakkor biztosítható a jó teljesítmény és kis késleltetése.</span><span class="sxs-lookup"><span data-stu-id="efabd-157">hello purpose of database indexes is tooserve queries in their various forms and shapes with minimum resource consumption (like CPU and input/output) while providing good throughput and low latency.</span></span> <span data-ttu-id="efabd-158">Hello megfelelő index lekérdezése az adatbázis hello választott gyakran, sok tervezést és kísérletezés igényel.</span><span class="sxs-lookup"><span data-stu-id="efabd-158">Often, hello choice of hello right index for querying a database requires much planning and experimentation.</span></span> <span data-ttu-id="efabd-159">Ezt a módszert használja az adatbázisok séma nélküli, ahol hello adatok nem felelnek meg a tooa szigorú séma, és gyorsan fejlődésének kihívást jelent.</span><span class="sxs-lookup"><span data-stu-id="efabd-159">This approach poses a challenge for schema-less databases where hello data doesn’t conform tooa strict schema and evolves rapidly.</span></span> 

<span data-ttu-id="efabd-160">Ezért hello Cosmos DB indexelési alrendszer tervezésekor be van állítva a következő célok hello:</span><span class="sxs-lookup"><span data-stu-id="efabd-160">Therefore, when we designed hello Cosmos DB indexing subsystem, we set hello following goals:</span></span>

* <span data-ttu-id="efabd-161">Indexeljük a dokumentumokat anélkül, hogy a séma: hello alrendszer indexelő bármely sémaadatok kér és nem bármely séma feltételezéseket hello dokumentumok ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="efabd-161">Index documents without requiring schema: hello indexing subsystem does not require any schema information or make any assumptions about schema of hello documents.</span></span> 
* <span data-ttu-id="efabd-162">Hatékony és gazdag hierarchikus és relációs lekérdezések támogatása: hello index támogatja hello Cosmos DB lekérdezési nyelv hatékonyan, beleértve a hierarchikus és relációs leképezések támogatását.</span><span class="sxs-lookup"><span data-stu-id="efabd-162">Support for efficient, rich hierarchical, and relational queries: hello index supports hello Cosmos DB query language efficiently, including support for hierarchical and relational projections.</span></span>
* <span data-ttu-id="efabd-163">Egységes lekérdezések in face of írások tartós kötet támogatása: nagy írási átviteli munkaterhelések egységes lekérdezések esetén hello index frissítése Növekményesen, hatékony és online az írások tartós kötet hello felületét.</span><span class="sxs-lookup"><span data-stu-id="efabd-163">Support for consistent queries in face of a sustained volume of writes: For high write throughput workloads with consistent queries, hello index is updated incrementally, efficiently, and online in hello face of a sustained volume of writes.</span></span> <span data-ttu-id="efabd-164">hello konzisztens index frissítése kritikus fontosságú tooserve hello lekérdezések mely hello beállított felhasználó hello dokumentum szolgáltatásban hello konzisztencia szinten.</span><span class="sxs-lookup"><span data-stu-id="efabd-164">hello consistent index update is crucial tooserve hello queries at hello consistency level in which hello user configured hello document service.</span></span>
* <span data-ttu-id="efabd-165">Több vállalat kiszolgálása támogatása: hello foglalásalapú modell megadott erőforrás irányításhoz bérlők között, index frissítései hello költségvetési rendszererőforrást (Processzor, memória és i/o műveletek száma másodpercenként) replika felosztott belül.</span><span class="sxs-lookup"><span data-stu-id="efabd-165">Support for multi-tenancy: Given hello reservation-based model for resource governance across tenants, index updates are performed within hello budget of system resources (CPU, memory, and input/output operations per second) allocated per replica.</span></span> 
* <span data-ttu-id="efabd-166">Tároló-hatékonyságot biztosít: A költséghatékonyság, hello lemezen tárhelyre terhet hello index a kötött és előre jelezhető.</span><span class="sxs-lookup"><span data-stu-id="efabd-166">Storage efficiency: For cost effectiveness, hello on-disk storage overhead of hello index is bounded and predictable.</span></span> <span data-ttu-id="efabd-167">Ez különösen fontos, mivel Cosmos DB hello fejlesztői toomake költség-alapú mellékhatásokkal közötti kapcsolat toohello a lekérdezések teljesítményét index terhelést.</span><span class="sxs-lookup"><span data-stu-id="efabd-167">This is crucial because Cosmos DB allows hello developer toomake cost-based tradeoffs between index overhead in relation toohello query performance.</span></span>  

<span data-ttu-id="efabd-168">Tekintse meg a toohello [Azure Cosmos DB samples](https://github.com/Azure/azure-documentdb-net) MSDN minták jelenít meg, hogyan tooconfigure hello indexelési házirendet egy gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="efabd-168">Refer toohello [Azure Cosmos DB samples](https://github.com/Azure/azure-documentdb-net) on MSDN for samples showing how tooconfigure hello indexing policy for a collection.</span></span> <span data-ttu-id="efabd-169">Most folytassuk hello részleteinek hello Azure Cosmos adatbázis SQL-szintaxis.</span><span class="sxs-lookup"><span data-stu-id="efabd-169">Let’s now get into hello details of hello Azure Cosmos DB SQL syntax.</span></span>

## <span data-ttu-id="efabd-170"><a id="Basics"></a>Az Azure Cosmos adatbázis SQL-lekérdezést alapjai</span><span class="sxs-lookup"><span data-stu-id="efabd-170"><a id="Basics"></a>Basics of an Azure Cosmos DB SQL query</span></span>
<span data-ttu-id="efabd-171">Minden egyes lekérdezés SELECT záradékában és választható FROM áll és a WHERE záradék / ANSI SQL szabványoknak.</span><span class="sxs-lookup"><span data-stu-id="efabd-171">Every query consists of a SELECT clause and optional FROM and WHERE clauses per ANSI-SQL standards.</span></span> <span data-ttu-id="efabd-172">Általában minden lekérdezéshez hello forrás hello FROM záradékban számbavétele megtörtént.</span><span class="sxs-lookup"><span data-stu-id="efabd-172">Typically, for each query, hello source in hello FROM clause is enumerated.</span></span> <span data-ttu-id="efabd-173">Hello szűréssel hello WHERE záradék hello forrás tooretrieve érvényesíti a JSON-dokumentumok egy részét.</span><span class="sxs-lookup"><span data-stu-id="efabd-173">Then hello filter in hello WHERE clause is applied on hello source tooretrieve a subset of JSON documents.</span></span> <span data-ttu-id="efabd-174">Végezetül hello SELECT záradékban használt tooproject hello kért hello értékek JSON válassza ki a listát.</span><span class="sxs-lookup"><span data-stu-id="efabd-174">Finally, hello SELECT clause is used tooproject hello requested JSON values in hello select list.</span></span>

    SELECT <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <span data-ttu-id="efabd-175"><a id="FromClause"></a>FROM záradékban</span><span class="sxs-lookup"><span data-stu-id="efabd-175"><a id="FromClause"></a>FROM clause</span></span>
<span data-ttu-id="efabd-176">Hello `FROM <from_specification>` záradék használata nem kötelező, kivéve, ha hello forrás szűrt vagy tervezett később hello lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="efabd-176">hello `FROM <from_specification>` clause is optional unless hello source is filtered or projected later in hello query.</span></span> <span data-ttu-id="efabd-177">hello ehhez a záradékhoz célja toospecify hello adatforrás esetén mely hello lekérdezés működnie kell.</span><span class="sxs-lookup"><span data-stu-id="efabd-177">hello purpose of this clause is toospecify hello data source upon which hello query must operate.</span></span> <span data-ttu-id="efabd-178">Hello egész gyűjteményre gyakran hello forrás, de ehelyett egy adhat meg a hello gyűjtemény egy részét.</span><span class="sxs-lookup"><span data-stu-id="efabd-178">Commonly hello whole collection is hello source, but one can specify a subset of hello collection instead.</span></span> 

<span data-ttu-id="efabd-179">A lekérdezés, például `SELECT * FROM Families` azt jelzi, hogy hello teljes családok gyűjteményt hello forrás mely tooenumerate keresztül.</span><span class="sxs-lookup"><span data-stu-id="efabd-179">A query like `SELECT * FROM Families` indicates that hello entire Families collection is hello source over which tooenumerate.</span></span> <span data-ttu-id="efabd-180">Egy különös azonosító legfelső szintű lehet használt toorepresent hello gyűjtemény hello gyűjteménynév használata helyett.</span><span class="sxs-lookup"><span data-stu-id="efabd-180">A special identifier ROOT can be used toorepresent hello collection instead of using hello collection name.</span></span> <span data-ttu-id="efabd-181">hello alábbi lista a kényszerített lekérdezésenként hello szabályok.</span><span class="sxs-lookup"><span data-stu-id="efabd-181">hello following list contains hello rules that are enforced per query:</span></span>

* <span data-ttu-id="efabd-182">hello gyűjtemény akkor jelölhető meg aliasként, például a `SELECT f.id FROM Families AS f` vagy egyszerűen `SELECT f.id FROM Families f`.</span><span class="sxs-lookup"><span data-stu-id="efabd-182">hello collection can be aliased, such as `SELECT f.id FROM Families AS f` or simply `SELECT f.id FROM Families f`.</span></span> <span data-ttu-id="efabd-183">Itt `f` hello megfelelője az `Families`.</span><span class="sxs-lookup"><span data-stu-id="efabd-183">Here `f` is hello equivalent of `Families`.</span></span> <span data-ttu-id="efabd-184">`AS`nem kötelező kulcsszó tooalias hello azonosító, amely.</span><span class="sxs-lookup"><span data-stu-id="efabd-184">`AS` is an optional keyword tooalias hello identifier.</span></span>
* <span data-ttu-id="efabd-185">Egyszer aliasnevet hello eredeti adatforrás nem köthető.</span><span class="sxs-lookup"><span data-stu-id="efabd-185">Once aliased, hello original source cannot be bound.</span></span> <span data-ttu-id="efabd-186">Például `SELECT Families.id FROM Families f` szintaktikailag hibás, mert hello azonosítója "Családok" többé nem lehet feloldani.</span><span class="sxs-lookup"><span data-stu-id="efabd-186">For example, `SELECT Families.id FROM Families f` is syntactically invalid since hello identifier "Families" cannot be resolved anymore.</span></span>
* <span data-ttu-id="efabd-187">Lehet, hogy a hivatkozott toobe igénylő összes tulajdonság teljesen minősített.</span><span class="sxs-lookup"><span data-stu-id="efabd-187">All properties that need toobe referenced must be fully qualified.</span></span> <span data-ttu-id="efabd-188">A szigorú séma való hello hiányában ez a kényszerített tooavoid egyetlen nem egyértelmű kötést.</span><span class="sxs-lookup"><span data-stu-id="efabd-188">In hello absence of strict schema adherence, this is enforced tooavoid any ambiguous bindings.</span></span> <span data-ttu-id="efabd-189">Ezért `SELECT id FROM Families f` óta hello tulajdonság szintaktikailag nem `id` nincs kötve.</span><span class="sxs-lookup"><span data-stu-id="efabd-189">Therefore, `SELECT id FROM Families f` is syntactically invalid since hello property `id` is not bound.</span></span>

### <a name="subdocuments"></a><span data-ttu-id="efabd-190">Aldokumentumok</span><span class="sxs-lookup"><span data-stu-id="efabd-190">Subdocuments</span></span>
<span data-ttu-id="efabd-191">hello forrás csökkentett tooa kisebb részhalmazát is lehet.</span><span class="sxs-lookup"><span data-stu-id="efabd-191">hello source can also be reduced tooa smaller subset.</span></span> <span data-ttu-id="efabd-192">Például tooenumerating minden a dokumentumban csak egy részfája, hello subroot majd válhat hello forrás, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="efabd-192">For instance, tooenumerating only a subtree in each document, hello subroot could then become hello source, as shown in hello following example:</span></span>

<span data-ttu-id="efabd-193">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-193">**Query**</span></span>

    SELECT * 
    FROM Families.children

<span data-ttu-id="efabd-194">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-194">**Results**</span></span>  

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

<span data-ttu-id="efabd-195">Amíg a fenti példa hello tömb hello forrásaként, az objektum is felhasználhatók hello forrásaként, amely a következő példa hello látható: hello eredménye, hogy minden érvényes JSON-érték (nem nincs definiálva), amely itt található: hello forrás tekinthető hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-195">While hello above example used an array as hello source, an object could also be used as hello source, which is what's shown in hello following example: Any valid JSON value (not undefined) that can be found in hello source is considered for inclusion in hello result of hello query.</span></span> <span data-ttu-id="efabd-196">Ha egyes termékcsaládok nem rendelkezik egy `address.state` érték, a lekérdezés eredményében hello ki vannak zárva.</span><span class="sxs-lookup"><span data-stu-id="efabd-196">If some families don’t have an `address.state` value, they are excluded in hello query result.</span></span>

<span data-ttu-id="efabd-197">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-197">**Query**</span></span>

    SELECT * 
    FROM Families.address.state

<span data-ttu-id="efabd-198">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-198">**Results**</span></span>

    [
      "WA", 
      "NY"
    ]


## <span data-ttu-id="efabd-199"><a id="WhereClause"></a>A WHERE záradék</span><span class="sxs-lookup"><span data-stu-id="efabd-199"><a id="WhereClause"></a>WHERE clause</span></span>
<span data-ttu-id="efabd-200">a WHERE záradék hello (**`WHERE <filter_condition>`**) megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="efabd-200">hello WHERE clause (**`WHERE <filter_condition>`**) is optional.</span></span> <span data-ttu-id="efabd-201">Meghatározza, hogy hello eredmény részét képező rendelés toobe meg kell felelnie a hello JSON-dokumentumok hello forrás által biztosított hello feltételeket.</span><span class="sxs-lookup"><span data-stu-id="efabd-201">It specifies hello condition(s) that hello JSON documents provided by hello source must satisfy in order toobe included as part of hello result.</span></span> <span data-ttu-id="efabd-202">Ki kell értékelnie minden bármely JSON-dokumentum hello megadott feltételek túl "true" toobe hello eredmény figyelembe venni.</span><span class="sxs-lookup"><span data-stu-id="efabd-202">Any JSON document must evaluate hello specified conditions too"true" toobe considered for hello result.</span></span> <span data-ttu-id="efabd-203">hello záradék helyének hello index réteg rendelés toodetermine hello abszolút legkisebb részhalmazban forrás azt jelzi, hogy hello eredmény része lehet.</span><span class="sxs-lookup"><span data-stu-id="efabd-203">hello WHERE clause is used by hello index layer in order toodetermine hello absolute smallest subset of source documents that can be part of hello result.</span></span> 

<span data-ttu-id="efabd-204">hello következő lekérdezés kéri a name tulajdonság, amelynek értéke tartalmazó dokumentumok `AndersenFamily`.</span><span class="sxs-lookup"><span data-stu-id="efabd-204">hello following query requests documents that contain a name property whose value is `AndersenFamily`.</span></span> <span data-ttu-id="efabd-205">Bármely más dokumentum, amely nem rendelkezik name tulajdonsággal, vagy ha hello értéke nem egyezik `AndersenFamily` ki van zárva.</span><span class="sxs-lookup"><span data-stu-id="efabd-205">Any other document that does not have a name property, or where hello value does not match `AndersenFamily` is excluded.</span></span> 

<span data-ttu-id="efabd-206">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-206">**Query**</span></span>

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="efabd-207">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-207">**Results**</span></span>

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


<span data-ttu-id="efabd-208">hello előző példából kiderült, egy egyszerű egyenlőség lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="efabd-208">hello previous example showed a simple equality query.</span></span> <span data-ttu-id="efabd-209">A DocumentDB API SQL számos skaláris kifejezést.</span><span class="sxs-lookup"><span data-stu-id="efabd-209">DocumentDB API SQL also supports a variety of scalar expressions.</span></span> <span data-ttu-id="efabd-210">leggyakrabban használt hello bináris és egyoperandusú kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-210">hello most commonly used are binary and unary expressions.</span></span> <span data-ttu-id="efabd-211">Hello forrás JSON-objektumból tulajdonsághivatkozást egyaránt érvényes kifejezések.</span><span class="sxs-lookup"><span data-stu-id="efabd-211">Property references from hello source JSON object are also valid expressions.</span></span> 

<span data-ttu-id="efabd-212">a következő bináris operátor hello jelenleg támogatott, és a lekérdezések, ahogy az alábbi példák hello használható:</span><span class="sxs-lookup"><span data-stu-id="efabd-212">hello following binary operators are currently supported and can be used in queries as shown in hello following examples:</span></span>  

<table>
<tr>
<td><span data-ttu-id="efabd-213">Aritmetikai</span><span class="sxs-lookup"><span data-stu-id="efabd-213">Arithmetic</span></span></td>    
<td><span data-ttu-id="efabd-214">+,-,*,/,%</span><span class="sxs-lookup"><span data-stu-id="efabd-214">+,-,*,/,%</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="efabd-215">Bitenként</span><span class="sxs-lookup"><span data-stu-id="efabd-215">Bitwise</span></span></td>    
<td><span data-ttu-id="efabd-216">|}, &, ^, <<>>,, >>> (nulla-Kitöltés jobbra tolást)</span><span class="sxs-lookup"><span data-stu-id="efabd-216">|, &, ^, <<, >>, >>> (zero-fill right shift)</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="efabd-217">Logikai</span><span class="sxs-lookup"><span data-stu-id="efabd-217">Logical</span></span></td>
<td><span data-ttu-id="efabd-218">ÉS, VAGY SEM</span><span class="sxs-lookup"><span data-stu-id="efabd-218">AND, OR, NOT</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="efabd-219">Összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="efabd-219">Comparison</span></span></td>    
<td><span data-ttu-id="efabd-220">=, !=, &lt;, &gt;, &lt;=, &gt;=, <></span><span class="sxs-lookup"><span data-stu-id="efabd-220">=, !=, &lt;, &gt;, &lt;=, &gt;=, <></span></span></td>
</tr>
<tr>
<td><span data-ttu-id="efabd-221">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="efabd-221">String</span></span></td>    
<td><span data-ttu-id="efabd-222">|| (összefűzésére)</span><span class="sxs-lookup"><span data-stu-id="efabd-222">|| (concatenate)</span></span></td>
</tr>
</table>  


<span data-ttu-id="efabd-223">Vessen egy pillantást néhány lekérdezést a bináris operátorok használatával.</span><span class="sxs-lookup"><span data-stu-id="efabd-223">Let’s take a look at some queries using binary operators.</span></span>

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


<span data-ttu-id="efabd-224">Az egyoperandusú operátorral hello +,-, ~, és nem is támogatottak, és is használhatók lekérdezéseken belül, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="efabd-224">hello unary operators +,-, ~ and NOT are also supported, and can be used inside queries as shown in hello following example:</span></span>

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



<span data-ttu-id="efabd-225">Ezenkívül toobinary és az egyoperandusú operátorokat tulajdonság vonatkozó hivatkozások használata is engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="efabd-225">In addition toobinary and unary operators, property references are also allowed.</span></span> <span data-ttu-id="efabd-226">Például `SELECT * FROM Families f WHERE f.isRegistered` értéket ad vissza hello hello tulajdonságot tartalmazó JSON-dokumentum `isRegistered` ahol a hello tulajdonság érték egyenlő toohello JSON `true` érték.</span><span class="sxs-lookup"><span data-stu-id="efabd-226">For example, `SELECT * FROM Families f WHERE f.isRegistered` returns hello JSON document containing hello property `isRegistered` where hello property's value is equal toohello JSON `true` value.</span></span> <span data-ttu-id="efabd-227">Egyéb értékek (false, null, nem definiált, `<number>`, `<string>`, `<object>`, `<array>`stb) részletes útmutatást az hello eredményből kivételével toohello forrásdokumentum.</span><span class="sxs-lookup"><span data-stu-id="efabd-227">Any other values (false, null, Undefined, `<number>`, `<string>`, `<object>`, `<array>`, etc.) leads toohello source document being excluded from hello result.</span></span> 

### <a name="equality-and-comparison-operators"></a><span data-ttu-id="efabd-228">Egyenlőség és összehasonlító operátorok</span><span class="sxs-lookup"><span data-stu-id="efabd-228">Equality and comparison operators</span></span>
<span data-ttu-id="efabd-229">hello következő táblázatban egyenlőségi összehasonlítás eredménye hello DocumentDB API SQL bármely két JSON-típusok között.</span><span class="sxs-lookup"><span data-stu-id="efabd-229">hello following table shows hello result of equality comparisons in DocumentDB API SQL between any two JSON types.</span></span>

<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top"><span data-ttu-id="efabd-230">
            <strong>Op</strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-230">
            <strong>Op</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="efabd-231">
            <strong>Nincs definiálva</strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-231">
            <strong>Undefined</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="efabd-232">
            <strong>NULL értékű</strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-232">
            <strong>Null</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="efabd-233">
            <strong>Logikai érték</strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-233">
            <strong>Boolean</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="efabd-234">
            <strong>Szám</strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-234">
            <strong>Number</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="efabd-235">
            <strong>Karakterlánc</strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-235">
            <strong>String</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="efabd-236">
            <strong>Objektum</strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-236">
            <strong>Object</strong>
         </span></span></td>
         <td valign="top"><span data-ttu-id="efabd-237">
            <strong>A tömb</strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-237">
            <strong>Array</strong>
         </span></span></td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="efabd-238">
            <strong>Nincs definiálva<strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-238">
            <strong>Undefined<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="efabd-239">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-239">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-240">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-240">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-241">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-241">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-242">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-242">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-243">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-243">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-244">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-244">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-245">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-245">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="efabd-246">
            <strong>NULL értékű<strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-246">
            <strong>Null<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="efabd-247">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-247">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="efabd-248">
            <strong>OKÉ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-248">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="efabd-249">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-249">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-250">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-250">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-251">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-251">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-252">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-252">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-253">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-253">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="efabd-254">
            <strong>Logikai érték<strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-254">
            <strong>Boolean<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="efabd-255">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-255">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-256">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-256">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="efabd-257">
            <strong>OKÉ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-257">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="efabd-258">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-258">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-259">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-259">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-260">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-260">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-261">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-261">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="efabd-262">
            <strong>Szám<strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-262">
            <strong>Number<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="efabd-263">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-263">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-264">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-264">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-265">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-265">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="efabd-266">
            <strong>OKÉ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-266">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="efabd-267">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-267">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-268">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-268">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-269">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-269">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="efabd-270">
            <strong>Karakterlánc<strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-270">
            <strong>String<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="efabd-271">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-271">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-272">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-272">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-273">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-273">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-274">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-274">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="efabd-275">
            <strong>OKÉ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-275">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="efabd-276">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-276">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-277">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-277">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="efabd-278">
            <strong>Objektum<strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-278">
            <strong>Object<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="efabd-279">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-279">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-280">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-280">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-281">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-281">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-282">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-282">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-283">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-283">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="efabd-284">
            <strong>OKÉ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-284">
            <strong>OK</strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="efabd-285">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-285">Undefined</span></span> </td>
      </tr>
      <tr>
         <td valign="top"><span data-ttu-id="efabd-286">
            <strong>A tömb<strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-286">
            <strong>Array<strong>
         </span></span></td>
         <td valign="top">
<span data-ttu-id="efabd-287">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-287">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-288">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-288">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-289">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-289">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-290">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-290">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-291">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-291">Undefined</span></span> </td>
         <td valign="top">
<span data-ttu-id="efabd-292">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-292">Undefined</span></span> </td>
         <td valign="top"><span data-ttu-id="efabd-293">
            <strong>OKÉ</strong>
         </span><span class="sxs-lookup"><span data-stu-id="efabd-293">
            <strong>OK</strong>
         </span></span></td>
      </tr>
   </tbody>
</table>

<span data-ttu-id="efabd-294">Más összehasonlító operátorok többek között a >, > =,! =, < és < =, hello következő szabályok lépnek érvénybe:</span><span class="sxs-lookup"><span data-stu-id="efabd-294">For other comparison operators such as >, >=, !=, < and <=, hello following rules apply:</span></span>   

* <span data-ttu-id="efabd-295">Összehasonlítás típusok között meghatározatlan eredményez.</span><span class="sxs-lookup"><span data-stu-id="efabd-295">Comparison across types results in Undefined.</span></span>
* <span data-ttu-id="efabd-296">Két objektum vagy két összehasonlítása tömbállandó meghatározatlan eredményez.</span><span class="sxs-lookup"><span data-stu-id="efabd-296">Comparison between two objects or two arrays results in Undefined.</span></span>   

<span data-ttu-id="efabd-297">Ha hello hello szűrő hello skaláris kifejezés eredménye nem definiált, hello megfelelő dokumentum nem szerepel hello eredményt, mert meghatározatlan logikailag túl "igaz" nem egyenlő.</span><span class="sxs-lookup"><span data-stu-id="efabd-297">If hello result of hello scalar expression in hello filter is Undefined, hello corresponding document would not be included in hello result, since Undefined doesn't logically equate too"true".</span></span>

### <a name="between-keyword"></a><span data-ttu-id="efabd-298">Kulcsszó között</span><span class="sxs-lookup"><span data-stu-id="efabd-298">BETWEEN keyword</span></span>
<span data-ttu-id="efabd-299">Hello BETWEEN kulcsszó tooexpress lekérdezések értékek, például a tartományok ANSI SQL is használhatja.</span><span class="sxs-lookup"><span data-stu-id="efabd-299">You can also use hello BETWEEN keyword tooexpress queries against ranges of values like in ANSI SQL.</span></span> <span data-ttu-id="efabd-300">KÖZÖTTI használható karakterlánc vagy szám ellen.</span><span class="sxs-lookup"><span data-stu-id="efabd-300">BETWEEN can be used against strings or numbers.</span></span>

<span data-ttu-id="efabd-301">Például a lekérdezés által visszaadott összes családba tartozó dokumentumok, mely hello az első gyermek osztályú (mind a két szélsőértéket beleértve) 1-5 között van.</span><span class="sxs-lookup"><span data-stu-id="efabd-301">For example, this query returns all family documents in which hello first child's grade is between 1-5 (both inclusive).</span></span> 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

<span data-ttu-id="efabd-302">Eltérően az ANSI-SQL-ben is használhatja hello BETWEEN záradék hello FROM záradékában például a következő példa hello.</span><span class="sxs-lookup"><span data-stu-id="efabd-302">Unlike in ANSI-SQL, you can also use hello BETWEEN clause in hello FROM clause like in hello following example.</span></span>

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

<span data-ttu-id="efabd-303">A lekérdezés végrehajtása gyorsabb ne felejtse el az indexelési házirendet elleni bármely numerikus tulajdonságok/elérési utakat hello BETWEEN záradék a szűrt index Tartománytípus használó toocreate.</span><span class="sxs-lookup"><span data-stu-id="efabd-303">For faster query execution times, remember toocreate an indexing policy that uses a range index type against any numeric properties/paths that are filtered in hello BETWEEN clause.</span></span> 

<span data-ttu-id="efabd-304">hello fő különbség a DocumentDB API és az ANSI SQL BETWEEN használatával, hogy akkor is express vegyes típusú tulajdonságokhoz lekérdezések – például lehetséges, hogy "osztály" [5] szám lehet bizonyos dokumentumok és mások számára ("grade4") tartalmazó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="efabd-304">hello main difference between using BETWEEN in DocumentDB API and ANSI SQL is that you can express range queries against properties of mixed types – for example, you might have "grade" be a number (5) in some documents and strings in others ("grade4").</span></span> <span data-ttu-id="efabd-305">Ezekben az esetekben például a JavaScript, a "nem definiált" két különböző típusú eredményt, és hello dokumentum összehasonlítása a rendszer kihagyja.</span><span class="sxs-lookup"><span data-stu-id="efabd-305">In these cases, like in JavaScript, a comparison between two different types results in "undefined", and hello document will be skipped.</span></span>

### <a name="logical-and-or-and-not-operators"></a><span data-ttu-id="efabd-306">Logikai (AND, OR, és nem) operátorok</span><span class="sxs-lookup"><span data-stu-id="efabd-306">Logical (AND, OR and NOT) operators</span></span>
<span data-ttu-id="efabd-307">Logikai operátorok működhet a logikai értékek.</span><span class="sxs-lookup"><span data-stu-id="efabd-307">Logical operators operate on Boolean values.</span></span> <span data-ttu-id="efabd-308">a következő táblák hello ezen operátorok hello logikai igazság táblák láthatók.</span><span class="sxs-lookup"><span data-stu-id="efabd-308">hello logical truth tables for these operators are shown in hello following tables.</span></span>

| <span data-ttu-id="efabd-309">VAGY</span><span class="sxs-lookup"><span data-stu-id="efabd-309">OR</span></span> | <span data-ttu-id="efabd-310">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="efabd-310">True</span></span> | <span data-ttu-id="efabd-311">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="efabd-311">False</span></span> | <span data-ttu-id="efabd-312">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-312">Undefined</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="efabd-313">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="efabd-313">True</span></span> |<span data-ttu-id="efabd-314">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="efabd-314">True</span></span> |<span data-ttu-id="efabd-315">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="efabd-315">True</span></span> |<span data-ttu-id="efabd-316">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="efabd-316">True</span></span> |
| <span data-ttu-id="efabd-317">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="efabd-317">False</span></span> |<span data-ttu-id="efabd-318">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="efabd-318">True</span></span> |<span data-ttu-id="efabd-319">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="efabd-319">False</span></span> |<span data-ttu-id="efabd-320">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-320">Undefined</span></span> |
| <span data-ttu-id="efabd-321">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-321">Undefined</span></span> |<span data-ttu-id="efabd-322">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="efabd-322">True</span></span> |<span data-ttu-id="efabd-323">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-323">Undefined</span></span> |<span data-ttu-id="efabd-324">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-324">Undefined</span></span> |

| <span data-ttu-id="efabd-325">ÉS</span><span class="sxs-lookup"><span data-stu-id="efabd-325">AND</span></span> | <span data-ttu-id="efabd-326">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="efabd-326">True</span></span> | <span data-ttu-id="efabd-327">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="efabd-327">False</span></span> | <span data-ttu-id="efabd-328">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-328">Undefined</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="efabd-329">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="efabd-329">True</span></span> |<span data-ttu-id="efabd-330">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="efabd-330">True</span></span> |<span data-ttu-id="efabd-331">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="efabd-331">False</span></span> |<span data-ttu-id="efabd-332">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-332">Undefined</span></span> |
| <span data-ttu-id="efabd-333">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="efabd-333">False</span></span> |<span data-ttu-id="efabd-334">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="efabd-334">False</span></span> |<span data-ttu-id="efabd-335">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="efabd-335">False</span></span> |<span data-ttu-id="efabd-336">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="efabd-336">False</span></span> |
| <span data-ttu-id="efabd-337">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-337">Undefined</span></span> |<span data-ttu-id="efabd-338">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-338">Undefined</span></span> |<span data-ttu-id="efabd-339">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="efabd-339">False</span></span> |<span data-ttu-id="efabd-340">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-340">Undefined</span></span> |

| <span data-ttu-id="efabd-341">NEM</span><span class="sxs-lookup"><span data-stu-id="efabd-341">NOT</span></span> |  |
| --- | --- |
| <span data-ttu-id="efabd-342">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="efabd-342">True</span></span> |<span data-ttu-id="efabd-343">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="efabd-343">False</span></span> |
| <span data-ttu-id="efabd-344">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="efabd-344">False</span></span> |<span data-ttu-id="efabd-345">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="efabd-345">True</span></span> |
| <span data-ttu-id="efabd-346">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-346">Undefined</span></span> |<span data-ttu-id="efabd-347">Nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="efabd-347">Undefined</span></span> |

### <a name="in-keyword"></a><span data-ttu-id="efabd-348">A kulcsszó</span><span class="sxs-lookup"><span data-stu-id="efabd-348">IN keyword</span></span>
<span data-ttu-id="efabd-349">hello IN kulcsszó használt toocheck lehet, hogy a megadott érték megegyezik-e a listában.</span><span class="sxs-lookup"><span data-stu-id="efabd-349">hello IN keyword can be used toocheck whether a specified value matches any value in a list.</span></span> <span data-ttu-id="efabd-350">Például a lekérdezés által visszaadott összes családba tartozó dokumentumok ahol hello azonosító: "WakefieldFamily" vagy "AndersenFamily".</span><span class="sxs-lookup"><span data-stu-id="efabd-350">For example, this query returns all family documents where hello id is one of "WakefieldFamily" or "AndersenFamily".</span></span> 

    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

<span data-ttu-id="efabd-351">Ez a példa visszaadja az összes dokumentumot ahol hello állapota bármelyik hello megadott értékeket.</span><span class="sxs-lookup"><span data-stu-id="efabd-351">This example returns all documents where hello state is any of hello specified values.</span></span>

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a><span data-ttu-id="efabd-352">Ternáris (?) és a Coalesce (?) operátorok</span><span class="sxs-lookup"><span data-stu-id="efabd-352">Ternary (?) and Coalesce (??) operators</span></span>
<span data-ttu-id="efabd-353">hello Ternáris és Coalesce operátorok használt toobuild feltételes kifejezéseket, például a C# és JavaScript programnyelvek hasonló toopopular lehet.</span><span class="sxs-lookup"><span data-stu-id="efabd-353">hello Ternary and Coalesce operators can be used toobuild conditional expressions, similar toopopular programming languages like C# and JavaScript.</span></span> 

<span data-ttu-id="efabd-354">hello Ternáris (?) operátor akkor lehet hasznos, ha hello összeállításával új JSON tulajdonságok keresnie.</span><span class="sxs-lookup"><span data-stu-id="efabd-354">hello Ternary (?) operator can be very handy when constructing new JSON properties on hello fly.</span></span> <span data-ttu-id="efabd-355">Például most írhat lekérdezések tooclassify hello osztály szintek emberi olvasható űrlapot például kezdő/köztes/speciális alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="efabd-355">For example, now you can write queries tooclassify hello class levels into a human readable form like Beginner/Intermediate/Advanced as shown below.</span></span>

     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

<span data-ttu-id="efabd-356">Is ágyazhatja hello hívások toohello operátor például az alábbi hello lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="efabd-356">You can also nest hello calls toohello operator like in hello query below.</span></span>

    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

<span data-ttu-id="efabd-357">Más lekérdezési operátorok, ha hello hello feltételes kifejezésben hivatkozott tulajdonságai hiányoznak minden a dokumentumban, vagy ha összehasonlított hello típusok eltérőek, majd ezeket a dokumentumokat nem tartoznak a hello lekérdezés eredményei között.</span><span class="sxs-lookup"><span data-stu-id="efabd-357">As with other query operators, if hello referenced properties in hello conditional expression are missing in any document, or if hello types being compared are different, then those documents are excluded in hello query results.</span></span>

<span data-ttu-id="efabd-358">hello Coalesce (?) operátor lehet (más néven tulajdonság hello jelenléte használt tooefficiently ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="efabd-358">hello Coalesce (??) operator can be used tooefficiently check for hello presence of a property (a.k.a.</span></span> <span data-ttu-id="efabd-359">van meghatározva) dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="efabd-359">is defined) in a document.</span></span> <span data-ttu-id="efabd-360">Ez akkor hasznos, ha félig strukturált lekérdezését vagy vegyes típusú adatokat.</span><span class="sxs-lookup"><span data-stu-id="efabd-360">This is useful when querying against semi-structured or data of mixed types.</span></span> <span data-ttu-id="efabd-361">Például, ez a lekérdezés visszaadja hello "Vezetéknév" Ha jelen van, vagy hello "Vezetéknév" Ha nem, akkor a jelen.</span><span class="sxs-lookup"><span data-stu-id="efabd-361">For example, this query returns hello "lastName" if present, or hello "surname" if it isn't present.</span></span>

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <span data-ttu-id="efabd-362"><a id="EscapingReservedKeywords"></a>Idézőjelek közé zárt tulajdonságelérő</span><span class="sxs-lookup"><span data-stu-id="efabd-362"><a id="EscapingReservedKeywords"></a>Quoted property accessor</span></span>
<span data-ttu-id="efabd-363">Emellett hello idézőjelek közé zárt tulajdonság operátorral tulajdonságok `[]`.</span><span class="sxs-lookup"><span data-stu-id="efabd-363">You can also access properties using hello quoted property operator `[]`.</span></span> <span data-ttu-id="efabd-364">Például `SELECT c.grade` és `SELECT c["grade"]` egyenértékű.</span><span class="sxs-lookup"><span data-stu-id="efabd-364">For example, `SELECT c.grade` and `SELECT c["grade"]` are equivalent.</span></span> <span data-ttu-id="efabd-365">Ez a szintaxis akkor hasznos, ha egy tulajdonság szóközöket, különleges karaktereket tartalmaz, vagy azonos nevet SQL kulcsszó vagy fenntartott szó tooshare hello történik tooescape van szüksége.</span><span class="sxs-lookup"><span data-stu-id="efabd-365">This syntax is useful when you need tooescape a property that contains spaces, special characters, or happens tooshare hello same name as a SQL keyword or reserved word.</span></span>

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <span data-ttu-id="efabd-366"><a id="SelectClause"></a>SELECT záradékban</span><span class="sxs-lookup"><span data-stu-id="efabd-366"><a id="SelectClause"></a>SELECT clause</span></span>
<span data-ttu-id="efabd-367">hello SELECT záradékban (**`SELECT <select_list>`**) megadása kötelező, és határozza meg, milyen értékeket lekért hello lekérdezés fentiekhez hasonló ANSI-SQL-ben.</span><span class="sxs-lookup"><span data-stu-id="efabd-367">hello SELECT clause (**`SELECT <select_list>`**) is mandatory and specifies what values are retrieved from hello query, just like in ANSI-SQL.</span></span> <span data-ttu-id="efabd-368">hello részhalmazán, amely fölött hello forrás dokumentumok van szűrve átadott alakzatot hello leképezése fázisban, amelyben hello megadott JSON-értékek olvassa és egy új JSON-objektum összeállított, minden egyes azt az alakzatot átadott bemenet.</span><span class="sxs-lookup"><span data-stu-id="efabd-368">hello subset that's been filtered on top of hello source documents are passed onto hello projection phase, where hello specified JSON values are retrieved and a new JSON object is constructed, for each input passed onto it.</span></span> 

<span data-ttu-id="efabd-369">a következő példa hello tipikus választó jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="efabd-369">hello following example shows a typical SELECT query.</span></span> 

<span data-ttu-id="efabd-370">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-370">**Query**</span></span>

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="efabd-371">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-371">**Results**</span></span>

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a><span data-ttu-id="efabd-372">Beágyazott tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="efabd-372">Nested properties</span></span>
<span data-ttu-id="efabd-373">A következő példa hello, a két beágyazott tulajdonságok kiálló azt `f.address.state` és `f.address.city`.</span><span class="sxs-lookup"><span data-stu-id="efabd-373">In hello following example, we are projecting two nested properties `f.address.state` and `f.address.city`.</span></span>

<span data-ttu-id="efabd-374">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-374">**Query**</span></span>

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="efabd-375">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-375">**Results**</span></span>

    [{
      "state": "WA", 
      "city": "seattle"
    }]


<span data-ttu-id="efabd-376">Leképezési JSON kifejezések is támogatja, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="efabd-376">Projection also supports JSON expressions as shown in hello following example:</span></span>

<span data-ttu-id="efabd-377">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-377">**Query**</span></span>

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="efabd-378">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-378">**Results**</span></span>

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


<span data-ttu-id="efabd-379">Nézzük hello szerepe `$1` itt.</span><span class="sxs-lookup"><span data-stu-id="efabd-379">Let's look at hello role of `$1` here.</span></span> <span data-ttu-id="efabd-380">Hello `SELECT` záradékban kell toocreate egy JSON-objektum, és nem kulcsra azért van, mert implicit argumentum változónevek kezdve használjuk `$1`.</span><span class="sxs-lookup"><span data-stu-id="efabd-380">hello `SELECT` clause needs toocreate a JSON object and since no key is provided, we use implicit argument variable names starting with `$1`.</span></span> <span data-ttu-id="efabd-381">Például a lekérdezés által visszaadott implicit argumentum két változót, címkével `$1` és `$2`.</span><span class="sxs-lookup"><span data-stu-id="efabd-381">For example, this query returns two implicit argument variables, labeled `$1` and `$2`.</span></span>

<span data-ttu-id="efabd-382">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-382">**Query**</span></span>

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="efabd-383">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-383">**Results**</span></span>

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a><span data-ttu-id="efabd-384">Aliasképző</span><span class="sxs-lookup"><span data-stu-id="efabd-384">Aliasing</span></span>
<span data-ttu-id="efabd-385">Most tegyük kiterjesztése hello fenti példában az explicit aliasképző értékek.</span><span class="sxs-lookup"><span data-stu-id="efabd-385">Now let's extend hello example above with explicit aliasing of values.</span></span> <span data-ttu-id="efabd-386">Ez hello kulcsszó használt aliassal való ellátását.</span><span class="sxs-lookup"><span data-stu-id="efabd-386">AS is hello keyword used for aliasing.</span></span> <span data-ttu-id="efabd-387">Nem kötelező kiálló hello második értékével megegyező közben látható `NameInfo`.</span><span class="sxs-lookup"><span data-stu-id="efabd-387">It's optional as shown while projecting hello second value as `NameInfo`.</span></span> 

<span data-ttu-id="efabd-388">Abban az esetben, ha a lekérdezés tartozik hello két tulajdonság azonos nevű aliassal való ellátását egyik vagy mindkét hello tulajdonságait, hogy azok rendszer használatát a tervezett hello használt toorename kell lennie eredménye.</span><span class="sxs-lookup"><span data-stu-id="efabd-388">In case a query has two properties with hello same name, aliasing must be used toorename one or both of hello properties so that they are disambiguated in hello projected result.</span></span>

<span data-ttu-id="efabd-389">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-389">**Query**</span></span>

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="efabd-390">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-390">**Results**</span></span>

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a><span data-ttu-id="efabd-391">Skaláris kifejezések</span><span class="sxs-lookup"><span data-stu-id="efabd-391">Scalar expressions</span></span>
<span data-ttu-id="efabd-392">Ezenkívül tooproperty hivatkozik, a SELECT záradékban hello is támogatja a skaláris kifejezések állandók, aritmetikai kifejezésekben, logikai kifejezéseket és stb. Például ez egy egyszerű "Hello, World" lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="efabd-392">In addition tooproperty references, hello SELECT clause also supports scalar expressions like constants, arithmetic expressions, logical expressions, etc. For example, here's a simple "Hello World" query.</span></span>

<span data-ttu-id="efabd-393">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-393">**Query**</span></span>

    SELECT "Hello World"

<span data-ttu-id="efabd-394">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-394">**Results**</span></span>

    [{
      "$1": "Hello World"
    }]


<span data-ttu-id="efabd-395">Ez egy összetett példa, amely a skaláris kifejezést használ.</span><span class="sxs-lookup"><span data-stu-id="efabd-395">Here's a more complex example that uses a scalar expression.</span></span>

<span data-ttu-id="efabd-396">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-396">**Query**</span></span>

    SELECT ((2 + 11 % 7)-2)/3    

<span data-ttu-id="efabd-397">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-397">**Results**</span></span>

    [{
      "$1": 1.33333
    }]


<span data-ttu-id="efabd-398">A következő példa hello hello hello skaláris kifejezés eredménye egy logikai érték.</span><span class="sxs-lookup"><span data-stu-id="efabd-398">In hello following example, hello result of hello scalar expression is a Boolean.</span></span>

<span data-ttu-id="efabd-399">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-399">**Query**</span></span>

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f    

<span data-ttu-id="efabd-400">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-400">**Results**</span></span>

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a><span data-ttu-id="efabd-401">Az objektum és tömb létrehozása</span><span class="sxs-lookup"><span data-stu-id="efabd-401">Object and array creation</span></span>
<span data-ttu-id="efabd-402">A DocumentDB API SQL egy másik alapfunkciója tömb vagy objektum-létrehozás.</span><span class="sxs-lookup"><span data-stu-id="efabd-402">Another key feature of DocumentDB API SQL is array/object creation.</span></span> <span data-ttu-id="efabd-403">Az előző példában hello vegye figyelembe, hogy létrehoztunk egy új JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="efabd-403">In hello previous example, note that we created a new JSON object.</span></span> <span data-ttu-id="efabd-404">Ehhez hasonlóan egy is végezhet tömbök látható hello példák a következő módon:</span><span class="sxs-lookup"><span data-stu-id="efabd-404">Similarly, one can also construct arrays as shown in hello following examples:</span></span>

<span data-ttu-id="efabd-405">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-405">**Query**</span></span>

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f    

<span data-ttu-id="efabd-406">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-406">**Results**</span></span>  

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

### <span data-ttu-id="efabd-407"><a id="ValueKeyword"></a>ÉRTÉK kulcsszó</span><span class="sxs-lookup"><span data-stu-id="efabd-407"><a id="ValueKeyword"></a>VALUE keyword</span></span>
<span data-ttu-id="efabd-408">Hello **érték** kulcsszó tartalmaz egy módja tooreturn JSON-érték.</span><span class="sxs-lookup"><span data-stu-id="efabd-408">hello **VALUE** keyword provides a way tooreturn JSON value.</span></span> <span data-ttu-id="efabd-409">Például a lent látható módon adja vissza hello skaláris hello lekérdezés `"Hello World"` helyett `{$1: "Hello World"}`.</span><span class="sxs-lookup"><span data-stu-id="efabd-409">For example, hello query shown below returns hello scalar `"Hello World"` instead of `{$1: "Hello World"}`.</span></span>

<span data-ttu-id="efabd-410">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-410">**Query**</span></span>

    SELECT VALUE "Hello World"

<span data-ttu-id="efabd-411">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-411">**Results**</span></span>

    [
      "Hello World"
    ]


<span data-ttu-id="efabd-412">hello következő lekérdezés visszaadja hello JSON-érték nélkül hello `"address"` hello eredmények címkéje.</span><span class="sxs-lookup"><span data-stu-id="efabd-412">hello following query returns hello JSON value without hello `"address"` label in hello results.</span></span>

<span data-ttu-id="efabd-413">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-413">**Query**</span></span>

    SELECT VALUE f.address
    FROM Families f    

<span data-ttu-id="efabd-414">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-414">**Results**</span></span>  

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

<span data-ttu-id="efabd-415">hello alábbi példa bővíti a tooshow hogyan tooreturn JSON primitív értékek (hello levélszintű hello JSON-fa).</span><span class="sxs-lookup"><span data-stu-id="efabd-415">hello following example extends this tooshow how tooreturn JSON primitive values (hello leaf level of hello JSON tree).</span></span> 

<span data-ttu-id="efabd-416">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-416">**Query**</span></span>

    SELECT VALUE f.address.state
    FROM Families f    

<span data-ttu-id="efabd-417">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-417">**Results**</span></span>

    [
      "WA",
      "NY"
    ]


### <a name="-operator"></a><span data-ttu-id="efabd-418">* Operátor</span><span class="sxs-lookup"><span data-stu-id="efabd-418">* Operator</span></span>
<span data-ttu-id="efabd-419">hello különleges operátort (*) támogatott tooproject hello dokumentumot-van.</span><span class="sxs-lookup"><span data-stu-id="efabd-419">hello special operator (*) is supported tooproject hello document as-is.</span></span> <span data-ttu-id="efabd-420">Használatakor a hello csak tervezett mező kell legyen.</span><span class="sxs-lookup"><span data-stu-id="efabd-420">When used, it must be hello only projected field.</span></span> <span data-ttu-id="efabd-421">Például a lekérdezés során `SELECT * FROM Families f` érvényes, `SELECT VALUE * FROM Families f ` és `SELECT *, f.id FROM Families f ` érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="efabd-421">While a query like `SELECT * FROM Families f` is valid, `SELECT VALUE * FROM Families f ` and  `SELECT *, f.id FROM Families f ` are not valid.</span></span>

<span data-ttu-id="efabd-422">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-422">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

<span data-ttu-id="efabd-423">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-423">**Results**</span></span>

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

### <span data-ttu-id="efabd-424"><a id="TopKeyword"></a>TOP operátor</span><span class="sxs-lookup"><span data-stu-id="efabd-424"><a id="TopKeyword"></a>TOP Operator</span></span>
<span data-ttu-id="efabd-425">hello felső kulcsszó lehet használt toolimit hello számú értéket a lekérdezésből.</span><span class="sxs-lookup"><span data-stu-id="efabd-425">hello TOP keyword can be used toolimit hello number of values from a query.</span></span> <span data-ttu-id="efabd-426">FELSŐ együtt hello ORDER BY záradék használata esetén hello eredménykészlet-e a korlátozott toohello első N rendezett értékek száma; Ellenkező esetben az eredmény hello első N eredmények száma nem definiált sorrendje.</span><span class="sxs-lookup"><span data-stu-id="efabd-426">When TOP is used in conjunction with hello ORDER BY clause, hello result set is limited toohello first N number of ordered values; otherwise, it returns hello first N number of results in an undefined order.</span></span> <span data-ttu-id="efabd-427">Ajánlott eljárásként a SELECT utasítással, mindig használja az ORDER BY záradék hello TOP záradék.</span><span class="sxs-lookup"><span data-stu-id="efabd-427">As a best practice, in a SELECT statement, always use an ORDER BY clause with hello TOP clause.</span></span> <span data-ttu-id="efabd-428">Ez az hello csak úgy toopredictably jelző felső által érintett sorok.</span><span class="sxs-lookup"><span data-stu-id="efabd-428">This is hello only way toopredictably indicate which rows are affected by TOP.</span></span> 

<span data-ttu-id="efabd-429">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-429">**Query**</span></span>

    SELECT TOP 1 * 
    FROM Families f 

<span data-ttu-id="efabd-430">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-430">**Results**</span></span>

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

<span data-ttu-id="efabd-431">FELSŐ egy állandó értékkel (ahogy fent), vagy a paraméteres lekérdezés változó érték használható.</span><span class="sxs-lookup"><span data-stu-id="efabd-431">TOP can be used with a constant value (as shown above) or with a variable value using parameterized queries.</span></span> <span data-ttu-id="efabd-432">További részletekért lásd a paraméteres lekérdezés az alábbi.</span><span class="sxs-lookup"><span data-stu-id="efabd-432">For more details, please see parameterized queries below.</span></span>

### <span data-ttu-id="efabd-433"><a id="Aggregates"></a>Aggregátumfüggvények</span><span class="sxs-lookup"><span data-stu-id="efabd-433"><a id="Aggregates"></a>Aggregate Functions</span></span>
<span data-ttu-id="efabd-434">Összesítéseket hajtsa végre a hello `SELECT` záradékban.</span><span class="sxs-lookup"><span data-stu-id="efabd-434">You can also perform aggregations in hello `SELECT` clause.</span></span> <span data-ttu-id="efabd-435">Az aggregátumfüggvények egy értékhalmazt a számítás elvégzése, és egyetlen érték visszaadása.</span><span class="sxs-lookup"><span data-stu-id="efabd-435">Aggregate functions perform a calculation on a set of values and return a single value.</span></span> <span data-ttu-id="efabd-436">Például hello következő lekérdezés visszaadja hello száma családba tartozó dokumentumok hello gyűjteményen belül.</span><span class="sxs-lookup"><span data-stu-id="efabd-436">For example, hello following query returns hello count of family documents within hello collection.</span></span>

<span data-ttu-id="efabd-437">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-437">**Query**</span></span>

    SELECT COUNT(1) 
    FROM Families f 

<span data-ttu-id="efabd-438">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-438">**Results**</span></span>

    [{
        "$1": 2
    }]

<span data-ttu-id="efabd-439">Is visszatérhet hello skaláris értékét hello összesített hello segítségével `VALUE` kulcsszó.</span><span class="sxs-lookup"><span data-stu-id="efabd-439">You can also return hello scalar value of hello aggregate by using hello `VALUE` keyword.</span></span> <span data-ttu-id="efabd-440">Például hello következő lekérdezés visszaadja hello száma értékek egyetlen számként:</span><span class="sxs-lookup"><span data-stu-id="efabd-440">For example, hello following query returns hello count of values as a single number:</span></span>

<span data-ttu-id="efabd-441">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-441">**Query**</span></span>

    SELECT VALUE COUNT(1) 
    FROM Families f 

<span data-ttu-id="efabd-442">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-442">**Results**</span></span>

    [ 2 ]

<span data-ttu-id="efabd-443">A szűrők együtt is elvégezheti összesíti.</span><span class="sxs-lookup"><span data-stu-id="efabd-443">You can also perform aggregates in combination with filters.</span></span> <span data-ttu-id="efabd-444">Például hello következő lekérdezés adja vissza hello címmel dokumentumok száma hello Washington állam hello.</span><span class="sxs-lookup"><span data-stu-id="efabd-444">For example, hello following query returns hello count of documents with hello address in hello state of Washington.</span></span>

<span data-ttu-id="efabd-445">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-445">**Query**</span></span>

    SELECT VALUE COUNT(1) 
    FROM Families f
    WHERE f.address.state = "WA" 

<span data-ttu-id="efabd-446">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-446">**Results**</span></span>

    [ 1 ]

<span data-ttu-id="efabd-447">hello következő táblázatban hello támogatott összesítő függvények listáját a DocumentDB az API-ban.</span><span class="sxs-lookup"><span data-stu-id="efabd-447">hello following table shows hello list of supported aggregate functions in DocumentDB API.</span></span> <span data-ttu-id="efabd-448">`SUM`és `AVG` numerikus érték, keresztül hajtja végre, mivel `COUNT`, `MIN`, és `MAX` karakterláncok, a logikai és nullák keresztül hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="efabd-448">`SUM` and `AVG` are performed over numeric values, whereas `COUNT`, `MIN`, and `MAX` can be performed over numbers, strings, Booleans, and nulls.</span></span> 

| <span data-ttu-id="efabd-449">Használat</span><span class="sxs-lookup"><span data-stu-id="efabd-449">Usage</span></span> | <span data-ttu-id="efabd-450">Leírás</span><span class="sxs-lookup"><span data-stu-id="efabd-450">Description</span></span> |
|-------|-------------|
| <span data-ttu-id="efabd-451">SZÁMA</span><span class="sxs-lookup"><span data-stu-id="efabd-451">COUNT</span></span> | <span data-ttu-id="efabd-452">Beolvasása hello hello kifejezésben szereplő elemek száma.</span><span class="sxs-lookup"><span data-stu-id="efabd-452">Returns hello number of items in hello expression.</span></span> |
| <span data-ttu-id="efabd-453">SUM</span><span class="sxs-lookup"><span data-stu-id="efabd-453">SUM</span></span>   | <span data-ttu-id="efabd-454">Beolvasása hello hello kifejezés összes hello értékének összege.</span><span class="sxs-lookup"><span data-stu-id="efabd-454">Returns hello sum of all hello values in hello expression.</span></span> |
| <span data-ttu-id="efabd-455">PERC</span><span class="sxs-lookup"><span data-stu-id="efabd-455">MIN</span></span>   | <span data-ttu-id="efabd-456">Beolvasása hello hello kifejezés minimális értéket.</span><span class="sxs-lookup"><span data-stu-id="efabd-456">Returns hello minimum value in hello expression.</span></span> |
| <span data-ttu-id="efabd-457">MAXIMÁLIS SZÁMA</span><span class="sxs-lookup"><span data-stu-id="efabd-457">MAX</span></span>   | <span data-ttu-id="efabd-458">Beolvasása hello hello kifejezésben maximális értéket.</span><span class="sxs-lookup"><span data-stu-id="efabd-458">Returns hello maximum value in hello expression.</span></span> |
| <span data-ttu-id="efabd-459">ÁTLAGOS</span><span class="sxs-lookup"><span data-stu-id="efabd-459">AVG</span></span>   | <span data-ttu-id="efabd-460">Beolvasása hello hello kifejezésben hello értékek átlaga.</span><span class="sxs-lookup"><span data-stu-id="efabd-460">Returns hello average of hello values in hello expression.</span></span> |

<span data-ttu-id="efabd-461">Összesíti egy tömb iteráció hello eredményeit keresztül is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="efabd-461">Aggregates can also be performed over hello results of an array iteration.</span></span> <span data-ttu-id="efabd-462">További információkért lásd: [tömb iterációs lekérdezésekben](#Iteration).</span><span class="sxs-lookup"><span data-stu-id="efabd-462">For more information, see [Array Iteration in Queries](#Iteration).</span></span>

> [!NOTE]
> <span data-ttu-id="efabd-463">Ha az Azure portál Query Explorer használatával hello, vegye figyelembe, hogy összesítési lekérdezések adhat vissza hello részben összesített eredmények a lekérdezés lap.</span><span class="sxs-lookup"><span data-stu-id="efabd-463">When using hello Azure portal's Query Explorer, note that aggregation queries may return hello partially aggregated results over a query page.</span></span> <span data-ttu-id="efabd-464">SDK-k hello egyetlen értéket összesítő összes oldalán hoz létre.</span><span class="sxs-lookup"><span data-stu-id="efabd-464">hello SDKs produces a single cumulative value across all pages.</span></span> 
> 
> <span data-ttu-id="efabd-465">A sorrendben tooperform összesítési a lekérdezések kód használatával kell .NET SDK 1.12.0, a .NET Core SDK 1.1.0-ás vagy a Java SDK 1.9.5 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="efabd-465">In order tooperform aggregation queries using code, you need .NET SDK 1.12.0, .NET Core SDK 1.1.0, or Java SDK 1.9.5 or above.</span></span>    
>

## <span data-ttu-id="efabd-466"><a id="OrderByClause"></a>ORDER BY záradék</span><span class="sxs-lookup"><span data-stu-id="efabd-466"><a id="OrderByClause"></a>ORDER BY clause</span></span>
<span data-ttu-id="efabd-467">Például az ANSI-SQL-ben megadhat egy választható Order By záradék lekérdezése során.</span><span class="sxs-lookup"><span data-stu-id="efabd-467">Like in ANSI-SQL, you can include an optional Order By clause while querying.</span></span> <span data-ttu-id="efabd-468">hello záradékot tartalmazhat egy választható ASC vagy DESC argumentum toospecify hello sorrendet, amelyben eredményeket kell beolvasni.</span><span class="sxs-lookup"><span data-stu-id="efabd-468">hello clause can include an optional ASC/DESC argument toospecify hello order in which results must be retrieved.</span></span>

<span data-ttu-id="efabd-469">Például ez kártevőcsaládok hello rezidens város neve sorrendjét, amely.</span><span class="sxs-lookup"><span data-stu-id="efabd-469">For example, here's a query that retrieves families in order of hello resident city's name.</span></span>

<span data-ttu-id="efabd-470">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-470">**Query**</span></span>

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city

<span data-ttu-id="efabd-471">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-471">**Results**</span></span>

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

<span data-ttu-id="efabd-472">És családok sorrendben létrehozásának dátuma, amely egy szám, amely hello epoch idő, azaz, eltelt idő óta 1970 jan. 1 másodperc van tárolva, amely itt található.</span><span class="sxs-lookup"><span data-stu-id="efabd-472">And here's a query that retrieves families in order of creation date, which is stored as a number representing hello epoch time, i.e, elapsed time since Jan 1, 1970 in seconds.</span></span>

<span data-ttu-id="efabd-473">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-473">**Query**</span></span>

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC

<span data-ttu-id="efabd-474">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-474">**Results**</span></span>

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

## <span data-ttu-id="efabd-475"><a id="Advanced"></a>Speciális adatbázis fogalmait és az SQL-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="efabd-475"><a id="Advanced"></a>Advanced database concepts and SQL queries</span></span>

### <span data-ttu-id="efabd-476"><a id="Iteration"></a>Ismétlés</span><span class="sxs-lookup"><span data-stu-id="efabd-476"><a id="Iteration"></a>Iteration</span></span>
<span data-ttu-id="efabd-477">Egy új szerkezet hello művelettel lett hozzáadva **IN** DocumentDB API SQL tooprovide támogatása a JSON-tömbök keresztül léptetés kulcsszót.</span><span class="sxs-lookup"><span data-stu-id="efabd-477">A new construct was added via hello **IN** keyword in DocumentDB API SQL tooprovide support for iterating over JSON arrays.</span></span> <span data-ttu-id="efabd-478">hello FROM forrás iterációs támogatja.</span><span class="sxs-lookup"><span data-stu-id="efabd-478">hello FROM source provides support for iteration.</span></span> <span data-ttu-id="efabd-479">Kezdjük a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="efabd-479">Let's start with hello following example:</span></span>

<span data-ttu-id="efabd-480">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-480">**Query**</span></span>

    SELECT * 
    FROM Families.children

<span data-ttu-id="efabd-481">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-481">**Results**</span></span>  

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

<span data-ttu-id="efabd-482">Most már egy másik lekérdezés keresztül hello gyűjtemény gyermekek iterációs végző vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="efabd-482">Now let's look at another query that performs iteration over children in hello collection.</span></span> <span data-ttu-id="efabd-483">Vegye figyelembe a hello kimeneti tömbben hello különbség.</span><span class="sxs-lookup"><span data-stu-id="efabd-483">Note hello difference in hello output array.</span></span> <span data-ttu-id="efabd-484">Ez a példa felosztja a `children` és hello eredmények simítja egyetlen tömbbe.</span><span class="sxs-lookup"><span data-stu-id="efabd-484">This example splits `children` and flattens hello results into a single array.</span></span>  

<span data-ttu-id="efabd-485">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-485">**Query**</span></span>

    SELECT * 
    FROM c IN Families.children

<span data-ttu-id="efabd-486">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-486">**Results**</span></span>  

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

<span data-ttu-id="efabd-487">Ez további meg minden egyes belépési hello tömb, ahogy az alábbi példa hello használt toofilter lehet:</span><span class="sxs-lookup"><span data-stu-id="efabd-487">This can be further used toofilter on each individual entry of hello array as shown in hello following example:</span></span>

<span data-ttu-id="efabd-488">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-488">**Query**</span></span>

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

<span data-ttu-id="efabd-489">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-489">**Results**</span></span>  

    [{
      "givenName": "Lisa"
    }]

<span data-ttu-id="efabd-490">Összesítési tömb iterációs hello eredményét keresztül is elvégezheti.</span><span class="sxs-lookup"><span data-stu-id="efabd-490">You can also perform aggregation over hello result of array iteration.</span></span> <span data-ttu-id="efabd-491">Például hello következő lekérdezés számolja hello gyermekek összes családok között.</span><span class="sxs-lookup"><span data-stu-id="efabd-491">For example, hello following query counts hello number of children among all families.</span></span>

<span data-ttu-id="efabd-492">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-492">**Query**</span></span>

    SELECT COUNT(child) 
    FROM child IN Families.children

<span data-ttu-id="efabd-493">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-493">**Results**</span></span>  

    [
      { 
        "$1": 3
      }
    ]

### <span data-ttu-id="efabd-494"><a id="Joins"></a>Illesztése</span><span class="sxs-lookup"><span data-stu-id="efabd-494"><a id="Joins"></a>Joins</span></span>
<span data-ttu-id="efabd-495">Hello kell toojoin táblák között egy relációs adatbázisban, fontos.</span><span class="sxs-lookup"><span data-stu-id="efabd-495">In a relational database, hello need toojoin across tables is important.</span></span> <span data-ttu-id="efabd-496">Az hello normalizált logikai corollary toodesigning sémák.</span><span class="sxs-lookup"><span data-stu-id="efabd-496">It's hello logical corollary toodesigning normalized schemas.</span></span> <span data-ttu-id="efabd-497">Ellenkező toothis, a DocumentDB API hello nem normalizált adatok modell sémamentes dokumentumok foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="efabd-497">Contrary toothis, DocumentDB API deals with hello denormalized data model of schema-free documents.</span></span> <span data-ttu-id="efabd-498">Ez az hello logikai megfelelője a "önillesztés".</span><span class="sxs-lookup"><span data-stu-id="efabd-498">This is hello logical equivalent of a "self-join".</span></span>

<span data-ttu-id="efabd-499">hello hello nyelvi támogató szintaxisa < from_source1 > Csatlakozás < from_source2 > ILLESZTÉSI... CSATLAKOZTASSA az < from_sourceN >.</span><span class="sxs-lookup"><span data-stu-id="efabd-499">hello syntax that hello language supports is <from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>.</span></span> <span data-ttu-id="efabd-500">A teljes, ezt adja vissza, amely **N**- rekordokat (a rekord **N** értékek).</span><span class="sxs-lookup"><span data-stu-id="efabd-500">Overall, this returns a set of **N**-tuples (tuple with **N** values).</span></span> <span data-ttu-id="efabd-501">A táblakonstruktor minden rekordjának összes gyűjtemény alias léptetés alatt az megfelelő készletek által visszaadott érték tartozik.</span><span class="sxs-lookup"><span data-stu-id="efabd-501">Each tuple has values produced by iterating all collection aliases over their respective sets.</span></span> <span data-ttu-id="efabd-502">Más szóval ez az egy teljes hello illesztési részt hello-készlet keresztszorzatát.</span><span class="sxs-lookup"><span data-stu-id="efabd-502">In other words, this is a full cross product of hello sets participating in hello join.</span></span>

<span data-ttu-id="efabd-503">hello következő példák azt szemléltetik, hogyan működik a hello JOIN záradékban.</span><span class="sxs-lookup"><span data-stu-id="efabd-503">hello following examples show how hello JOIN clause works.</span></span> <span data-ttu-id="efabd-504">A következő példa hello hello eredménye üres mivel hello forrás- és üres minden dokumentumát keresztszorzatát üres.</span><span class="sxs-lookup"><span data-stu-id="efabd-504">In hello following example, hello result is empty since hello cross product of each document from source and an empty set is empty.</span></span>

<span data-ttu-id="efabd-505">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-505">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

<span data-ttu-id="efabd-506">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-506">**Results**</span></span>  

    [{
    }]


<span data-ttu-id="efabd-507">A következő példa hello, hello illesztési hello dokumentumgyökér és hello közé esik `children` subroot.</span><span class="sxs-lookup"><span data-stu-id="efabd-507">In hello following example, hello join is between hello document root and hello `children` subroot.</span></span> <span data-ttu-id="efabd-508">Egy eltérő termék két JSON-objektumok között.</span><span class="sxs-lookup"><span data-stu-id="efabd-508">It's a cross product between two JSON objects.</span></span> <span data-ttu-id="efabd-509">hello arra, hogy gyermeke tömb nincs hello ILLESZTÉSI hatékonyan, mivel azt a egyetlen legfelső szintű hello gyermekek tömb nem foglalkoznak.</span><span class="sxs-lookup"><span data-stu-id="efabd-509">hello fact that children is an array is not effective in hello JOIN since we are dealing with a single root that is hello children array.</span></span> <span data-ttu-id="efabd-510">Ezért hello eredmény tartalmazza csak két eredményt, mivel hello hello tömbbel rendelkező minden egyes dokumentum keresztszorzatát adja eredményül pontosan csak egy dokumentum.</span><span class="sxs-lookup"><span data-stu-id="efabd-510">Hence hello result contains only two results, since hello cross product of each document with hello array yields exactly only one document.</span></span>

<span data-ttu-id="efabd-511">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-511">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN f.children

<span data-ttu-id="efabd-512">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-512">**Results**</span></span>

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


<span data-ttu-id="efabd-513">a következő példa hello több hagyományos illesztés jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="efabd-513">hello following example shows a more conventional join:</span></span>

<span data-ttu-id="efabd-514">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-514">**Query**</span></span>

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

<span data-ttu-id="efabd-515">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-515">**Results**</span></span>

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



<span data-ttu-id="efabd-516">hello először thing toonote, hogy hello `from_source` a hello **csatlakozás** záradék egy iterátor.</span><span class="sxs-lookup"><span data-stu-id="efabd-516">hello first thing toonote is that hello `from_source` of hello **JOIN** clause is an iterator.</span></span> <span data-ttu-id="efabd-517">Igen hello folyamata ebben az esetben a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="efabd-517">So, hello flow in this case is as follows:</span></span>  

* <span data-ttu-id="efabd-518">Bontsa ki az egyes gyermekelem **c** hello tömbben.</span><span class="sxs-lookup"><span data-stu-id="efabd-518">Expand each child element **c** in hello array.</span></span>
* <span data-ttu-id="efabd-519">Hello a dokumentum gyökerének hello határokon termék alkalmazása **f** minden gyermekelemmel rendelkező **c** , amely lett egybesimított hello első lépésben.</span><span class="sxs-lookup"><span data-stu-id="efabd-519">Apply a cross product with hello root of hello document **f** with each child element **c** that was flattened in hello first step.</span></span>
* <span data-ttu-id="efabd-520">Végezetül projekt hello gyökérszintű objektum **f** névtulajdonság önmagában.</span><span class="sxs-lookup"><span data-stu-id="efabd-520">Finally, project hello root object **f** name property alone.</span></span> 

<span data-ttu-id="efabd-521">hello első dokumentum (`AndersenFamily`) csak egy gyermekelemet tartalmaz, ezért a hello eredménykészlet csak egyetlen objektumhoz megfelelő toothis dokumentumot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="efabd-521">hello first document (`AndersenFamily`) contains only one child element, so hello result set contains only a single object corresponding toothis document.</span></span> <span data-ttu-id="efabd-522">hello második dokumentum (`WakefieldFamily`) két gyermekeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="efabd-522">hello second document (`WakefieldFamily`) contains two children.</span></span> <span data-ttu-id="efabd-523">Igen hello határokon termék hoz létre egy külön objektum minden gyermek, ezáltal két objektum, egy gyermek megfelelő toothis dokumentumok eredményez.</span><span class="sxs-lookup"><span data-stu-id="efabd-523">So, hello cross product produces a separate object for each child, thereby resulting in two objects, one for each child corresponding toothis document.</span></span> <span data-ttu-id="efabd-524">mindkét ezeket a dokumentumokat a mezők kitöltése hello legfelső szintű hello ugyanaz, mint egy határokon termékben teheti meg.</span><span class="sxs-lookup"><span data-stu-id="efabd-524">hello root fields in both these documents are hello same, just as you would expect in a cross product.</span></span>

<span data-ttu-id="efabd-525">hello valós segédprogram a hello ILLESZTÉS tooform rekordokat hello kereszt-terméket, amely nem egy alakzat nehéz tooproject.</span><span class="sxs-lookup"><span data-stu-id="efabd-525">hello real utility of hello JOIN is tooform tuples from hello cross-product in a shape that's otherwise difficult tooproject.</span></span> <span data-ttu-id="efabd-526">Továbbá, a hello az alábbi példában látható, szűrést, a rekordot hello kombinációja lehetővé tévő hello felhasználó teljes feltételfüggvényt hello rekordokat feltételt választotta.</span><span class="sxs-lookup"><span data-stu-id="efabd-526">Furthermore, as we see in hello example below, you could filter on hello combination of a tuple that lets' hello user chose a condition satisfied by hello tuples overall.</span></span>

<span data-ttu-id="efabd-527">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-527">**Query**</span></span>

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets

<span data-ttu-id="efabd-528">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-528">**Results**</span></span>

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



<span data-ttu-id="efabd-529">Ebben a példában az előző példa hello természetes bővítménye, és végrehajtja a dupla való csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="efabd-529">This example is a natural extension of hello preceding example, and performs a double join.</span></span> <span data-ttu-id="efabd-530">Igen hello határokon termék tekintheti meg a következő látszólagosan kód hello:</span><span class="sxs-lookup"><span data-stu-id="efabd-530">So, hello cross product can be viewed as hello following pseudo-code:</span></span>

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

<span data-ttu-id="efabd-531">`AndersenFamily`egy gyermek, aki rendelkezik egy háziállat rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="efabd-531">`AndersenFamily` has one child who has one pet.</span></span> <span data-ttu-id="efabd-532">Igen, hello határokon termék eredményez több olyan sort (1\*1\*1) a család.</span><span class="sxs-lookup"><span data-stu-id="efabd-532">So, hello cross product yields one row (1\*1\*1) from this family.</span></span> <span data-ttu-id="efabd-533">WakefieldFamily, azonban a két gyermekelemek tartoznak, de csak egy "Jesse" gyermeket kedvtelésből.</span><span class="sxs-lookup"><span data-stu-id="efabd-533">WakefieldFamily however has two children, but only one child "Jesse" has pets.</span></span> <span data-ttu-id="efabd-534">Jesse két kedvtelésből, ha rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="efabd-534">Jesse has two pets though.</span></span> <span data-ttu-id="efabd-535">Ezért hello határokon termék eredményez 1\*1\*2 = 2 család a sort.</span><span class="sxs-lookup"><span data-stu-id="efabd-535">Hence hello cross product yields 1\*1\*2 = 2 rows from this family.</span></span>

<span data-ttu-id="efabd-536">Hello a következő példában, nincs egy kiegészítő szűrőt `pet`.</span><span class="sxs-lookup"><span data-stu-id="efabd-536">In hello next example, there is an additional filter on `pet`.</span></span> <span data-ttu-id="efabd-537">Ez nem tartalmazza az összes, ahol hello háziállatának neve nincs "Árnyékmásolat" hello rekordokat.</span><span class="sxs-lookup"><span data-stu-id="efabd-537">This excludes all hello tuples where hello pet name is not "Shadow".</span></span> <span data-ttu-id="efabd-538">Figyelje meg, hogy azt tudja toobuild rekordokat a tömböket, szűrő bármely hello elemek hello rekord, és projekt hello elemek kombinációja.</span><span class="sxs-lookup"><span data-stu-id="efabd-538">Notice that we are able toobuild tuples from arrays, filter on any of hello elements of hello tuple, and project any combination of hello elements.</span></span> 

<span data-ttu-id="efabd-539">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-539">**Query**</span></span>

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

<span data-ttu-id="efabd-540">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-540">**Results**</span></span>

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <span data-ttu-id="efabd-541"><a id="JavaScriptIntegration"></a>JavaScript-integráció</span><span class="sxs-lookup"><span data-stu-id="efabd-541"><a id="JavaScriptIntegration"></a>JavaScript integration</span></span>
<span data-ttu-id="efabd-542">Azure Cosmos DB programozási modellt biztosít a feldolgozás alatt álló alapú JavaScript-alkalmazáslogika közvetlenül hello gyűjtemények, tárolt eljárások és eseményindítók tekintetében.</span><span class="sxs-lookup"><span data-stu-id="efabd-542">Azure Cosmos DB provides a programming model for executing JavaScript based application logic directly on hello collections in terms of stored procedures and triggers.</span></span> <span data-ttu-id="efabd-543">Ez lehetővé teszi, hogy mindkét:</span><span class="sxs-lookup"><span data-stu-id="efabd-543">This allows for both:</span></span>

* <span data-ttu-id="efabd-544">Képes toodo nagy teljesítményű tranzakciós CRUD műveletek és egy gyűjtemény alapján hello szoros integrációja a JavaScript futásidejű közvetlenül belül hello adatbázismotor-dokumentumokon végzett lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="efabd-544">Ability toodo high-performance transactional CRUD operations and queries against documents in a collection by virtue of hello deep integration of JavaScript runtime directly within hello database engine.</span></span> 
* <span data-ttu-id="efabd-545">Természetes modellezési folyamatábrán, változó hatókörének, és a hozzárendelés és az adatbázis-tranzakciókhoz a primitívek kivételkezelő integrálását.</span><span class="sxs-lookup"><span data-stu-id="efabd-545">A natural modeling of control flow, variable scoping, and assignment and integration of exception handling primitives with database transactions.</span></span> <span data-ttu-id="efabd-546">A JavaScript-integráció Azure Cosmos DB-támogatással kapcsolatos további információkért tekintse meg az toohello JavaScript kiszolgálóoldali programozhatóság dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="efabd-546">For more details about Azure Cosmos DB support for JavaScript integration, please refer toohello JavaScript server-side programmability documentation.</span></span>

### <span data-ttu-id="efabd-547"><a id="UserDefinedFunctions"></a>Felhasználói függvény (UDF)</span><span class="sxs-lookup"><span data-stu-id="efabd-547"><a id="UserDefinedFunctions"></a>User-Defined Functions (UDFs)</span></span>
<span data-ttu-id="efabd-548">Hello típusok már definiálva van ebben a cikkben, valamint a DocumentDB API SQL támogatja az a felhasználó definiált függvény (UDF).</span><span class="sxs-lookup"><span data-stu-id="efabd-548">Along with hello types already defined in this article, DocumentDB API SQL provides support for User Defined Functions (UDF).</span></span> <span data-ttu-id="efabd-549">Skaláris felhasználó által megadott függvények támogatottak, ahol hello fejlesztők nulla vagy több argumentumot adjon át és vissza egyetlen argumentuma vissza eredményt.</span><span class="sxs-lookup"><span data-stu-id="efabd-549">In particular, scalar UDFs are supported where hello developers can pass in zero or many arguments and return a single argument result back.</span></span> <span data-ttu-id="efabd-550">Minden egyes argumentum ellenőrzése alatt álló engedélyezett JSON-érték.</span><span class="sxs-lookup"><span data-stu-id="efabd-550">Each of these arguments is checked for being legal JSON values.</span></span>  

<span data-ttu-id="efabd-551">a DocumentDB API SQL-szintaxis hello ki van bővítve toosupport egyéni alkalmazáslogika ezen felhasználó által definiált függvényeket használatával.</span><span class="sxs-lookup"><span data-stu-id="efabd-551">hello DocumentDB API SQL syntax is extended toosupport custom application logic using these User-Defined Functions.</span></span> <span data-ttu-id="efabd-552">Felhasználó által megadott függvények regisztrálhatók a DocumentDB API, és ezután lehet hivatkozni az SQL-lekérdezés részeként.</span><span class="sxs-lookup"><span data-stu-id="efabd-552">UDFs can be registered with DocumentDB API and then be referenced as part of a SQL query.</span></span> <span data-ttu-id="efabd-553">Felhasználó által megadott függvények vannak exquisitely hello valójában lekérdezések által meghívott toobe tervezték.</span><span class="sxs-lookup"><span data-stu-id="efabd-553">In fact, hello UDFs are exquisitely designed toobe invoked by queries.</span></span> <span data-ttu-id="efabd-554">Corollary toothis lehetőség, felhasználó által megadott függvények nem rendelkeznek hozzáféréssel toohello környezeti objektumot amely hello más JavaScript típusoknak (tárolt eljárások és eseményindítók) lehet.</span><span class="sxs-lookup"><span data-stu-id="efabd-554">As a corollary toothis choice, UDFs do not have access toohello context object which hello other JavaScript types (stored procedures and triggers) have.</span></span> <span data-ttu-id="efabd-555">Lekérdezések csak olvashatóként hajtható végre, mert futtathatják az elsődleges vagy másodlagos replikákon.</span><span class="sxs-lookup"><span data-stu-id="efabd-555">Since queries execute as read-only, they can run either on primary or on secondary replicas.</span></span> <span data-ttu-id="efabd-556">Felhasználó által megadott függvények, ezért a másodlagos replikákon más JavaScript típusától eltérően tervezett toorun.</span><span class="sxs-lookup"><span data-stu-id="efabd-556">Therefore, UDFs are designed toorun on secondary replicas unlike other JavaScript types.</span></span>

<span data-ttu-id="efabd-557">Alább példája egy UDF hogyan lehet regisztrálni: hello Cosmos DB adatbázist, kifejezetten egy dokumentumgyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="efabd-557">Below is an example of how a UDF can be registered at hello Cosmos DB database, specifically under a document collection.</span></span>

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

<span data-ttu-id="efabd-558">hello előző példa létrehoz egy UDF, amelynek a neve `REGEX_MATCH`.</span><span class="sxs-lookup"><span data-stu-id="efabd-558">hello preceding example creates a UDF whose name is `REGEX_MATCH`.</span></span> <span data-ttu-id="efabd-559">Elfogadja a JSON két karakterlánc-értékek `input` és `pattern` és hello első egyező hello mintát megadott hello második ellenőrzi a JavaScript string.match() függvény használatával.</span><span class="sxs-lookup"><span data-stu-id="efabd-559">It accepts two JSON string values `input` and `pattern` and checks if hello first matches hello pattern specified in hello second using JavaScript's string.match() function.</span></span>

<span data-ttu-id="efabd-560">A Microsoft most már használhatja a UDF leképezés lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="efabd-560">We can now use this UDF in a query in a projection.</span></span> <span data-ttu-id="efabd-561">Felhasználó által megadott függvények kell minősíteni hello kis-és nagybetűket előtaggal "udf."</span><span class="sxs-lookup"><span data-stu-id="efabd-561">UDFs must be qualified with hello case-sensitive prefix "udf."</span></span> <span data-ttu-id="efabd-562">Amikor meghívhatók lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="efabd-562">when called from within queries.</span></span> 

> [!NOTE]
> <span data-ttu-id="efabd-563">Előzetes too3 17 2015 / /, Cosmos DB támogatott UDF hívások nélkül hello "udf."</span><span class="sxs-lookup"><span data-stu-id="efabd-563">Prior too3/17/2015, Cosmos DB supported UDF calls without hello "udf."</span></span> <span data-ttu-id="efabd-564">Válasszon REGEX_MATCH() például előtag.</span><span class="sxs-lookup"><span data-stu-id="efabd-564">prefix like SELECT REGEX_MATCH().</span></span> <span data-ttu-id="efabd-565">A hívó mintát elavult.</span><span class="sxs-lookup"><span data-stu-id="efabd-565">This calling pattern has been deprecated.</span></span>  
> 
> 

<span data-ttu-id="efabd-566">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-566">**Query**</span></span>

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

<span data-ttu-id="efabd-567">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-567">**Results**</span></span>

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

<span data-ttu-id="efabd-568">hello UDF is használható egy szűrő belül hello példa az alábbi, is minősíti hello "udf."</span><span class="sxs-lookup"><span data-stu-id="efabd-568">hello UDF can also be used inside a filter as shown in hello example below, also qualified with hello "udf."</span></span> <span data-ttu-id="efabd-569">előtagja:</span><span class="sxs-lookup"><span data-stu-id="efabd-569">prefix:</span></span>

<span data-ttu-id="efabd-570">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-570">**Query**</span></span>

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

<span data-ttu-id="efabd-571">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-571">**Results**</span></span>

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


<span data-ttu-id="efabd-572">Felhasználó által megadott függvények lényegében érvényes skaláris kifejezések, és leképezések és szűrőket is használhat.</span><span class="sxs-lookup"><span data-stu-id="efabd-572">In essence, UDFs are valid scalar expressions and can be used in both projections and filters.</span></span> 

<span data-ttu-id="efabd-573">tooexpand hello működik a felhasználó által megadott függvények, vizsgáljuk meg egy másik példa feltételes logikával:</span><span class="sxs-lookup"><span data-stu-id="efabd-573">tooexpand on hello power of UDFs, let's look at another example with conditional logic:</span></span>

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


<span data-ttu-id="efabd-574">Az alábbiakban látható egy példa, hogy a gyakorlatokban hello UDF.</span><span class="sxs-lookup"><span data-stu-id="efabd-574">Below is an example that exercises hello UDF.</span></span>

<span data-ttu-id="efabd-575">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-575">**Query**</span></span>

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f    

<span data-ttu-id="efabd-576">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-576">**Results**</span></span>

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


<span data-ttu-id="efabd-577">Mivel az előző példák showcase hello, felhasználó által megadott függvények integrálása hello power JavaScript nyelv hello DocumentDB API SQL tooprovide egy gazdag programozható felület toodo eljárási, feltételes összetettek beépített a JavaScript futásidejű hello segítségével képességek.</span><span class="sxs-lookup"><span data-stu-id="efabd-577">As hello preceding examples showcase, UDFs integrate hello power of JavaScript language with hello DocumentDB API SQL tooprovide a rich programmable interface toodo complex procedural, conditional logic with hello help of inbuilt JavaScript runtime capabilities.</span></span>

<span data-ttu-id="efabd-578">A DocumentDB API SQL biztosít hello argumentumok toohello felhasználó által megadott függvények nyilvántartott egyes dokumentumok hello forrás szakaszában hello aktuális (a WHERE záradékban vagy a SELECT záradékban) feldolgozási hello UDF.</span><span class="sxs-lookup"><span data-stu-id="efabd-578">DocumentDB API SQL provides hello arguments toohello UDFs for each document in hello source at hello current stage (WHERE clause or SELECT clause) of processing hello UDF.</span></span> <span data-ttu-id="efabd-579">hello eredmény van beépítve zökkenőmentesen hello teljes végrehajtási folyamatban.</span><span class="sxs-lookup"><span data-stu-id="efabd-579">hello result is incorporated in hello overall execution pipeline seamlessly.</span></span> <span data-ttu-id="efabd-580">Ha hello tulajdonságok hivatkozott tooby hello UDF paraméterek nem találhatók hello JSON-érték hello tekint a paraméter nincs definiálva, és ezért hello UDF meghívása teljesen kimarad.</span><span class="sxs-lookup"><span data-stu-id="efabd-580">If hello properties referred tooby hello UDF parameters are not available in hello JSON value, hello parameter is considered as undefined and hence hello UDF invocation is entirely skipped.</span></span> <span data-ttu-id="efabd-581">Hasonlóképpen ha hello UDF hello eredménye nem definiált, azt nem szerepel hello eredménye.</span><span class="sxs-lookup"><span data-stu-id="efabd-581">Similarly if hello result of hello UDF is undefined, it's not included in hello result.</span></span> 

<span data-ttu-id="efabd-582">Összefoglalva a felhasználó által megadott függvények kiváló eszközök toodo bonyolult üzleti logikát hello lekérdezés részeként.</span><span class="sxs-lookup"><span data-stu-id="efabd-582">In summary, UDFs are great tools toodo complex business logic as part of hello query.</span></span>

### <a name="operator-evaluation"></a><span data-ttu-id="efabd-583">A kiértékelési operátor</span><span class="sxs-lookup"><span data-stu-id="efabd-583">Operator evaluation</span></span>
<span data-ttu-id="efabd-584">Cosmos DB, hello alapján, hogy az egy JSON-adatbázis megrajzolja fekvő JavaScript operátorok és az értékelés szemantikáját.</span><span class="sxs-lookup"><span data-stu-id="efabd-584">Cosmos DB, by hello virtue of being a JSON database, draws parallels with JavaScript operators and its evaluation semantics.</span></span> <span data-ttu-id="efabd-585">Amíg Cosmos DB megpróbál toopreserve JavaScript szemantikáját JSON támogatási tekintetében, hello művelet kiértékelése százalékkal, bizonyos esetekben.</span><span class="sxs-lookup"><span data-stu-id="efabd-585">While Cosmos DB tries toopreserve JavaScript semantics in terms of JSON support, hello operation evaluation deviates in some instances.</span></span>

<span data-ttu-id="efabd-586">A DocumentDB API SQL, ellentétben a hagyományos SQL hello típusú értékeket gyakran nem ismert hello értékek adatbázisból lekéréséig.</span><span class="sxs-lookup"><span data-stu-id="efabd-586">In DocumentDB API SQL, unlike in traditional SQL, hello types of values are often not known until hello values are retrieved from database.</span></span> <span data-ttu-id="efabd-587">A sorrend tooefficiently hajtsa végre a lekérdezéseket, hello operátorok a többsége a szigorú szemben támasztott követelményeit.</span><span class="sxs-lookup"><span data-stu-id="efabd-587">In order tooefficiently execute queries, most of hello operators have strict type requirements.</span></span> 

<span data-ttu-id="efabd-588">A DocumentDB API SQL nem hajtható végre implicit konverzió JavaScript eltérően.</span><span class="sxs-lookup"><span data-stu-id="efabd-588">DocumentDB API SQL doesn't perform implicit conversions, unlike JavaScript.</span></span> <span data-ttu-id="efabd-589">Például egy lekérdezést, például `SELECT * FROM Person p WHERE p.Age = 21` megegyezik egy kora tulajdonságot, amelynek értéke 21 tartalmazó dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="efabd-589">For instance, a query like `SELECT * FROM Person p WHERE p.Age = 21` matches documents that contain an Age property whose value is 21.</span></span> <span data-ttu-id="efabd-590">Bármely más, amelynek kora tulajdonsága egyezést mutat a karakterlánc a "21", vagy más valószínűleg végtelen változata dokumentum, például "021", "21,0", "0021", "00021", nem fog egyeztetni stb.</span><span class="sxs-lookup"><span data-stu-id="efabd-590">Any other document whose Age property matches string "21", or other possibly infinite variations like "021", "21.0", "0021", "00021", etc. will not be matched.</span></span> <span data-ttu-id="efabd-591">Ez ezzel szemben az implicit módon casted toonumbers hello karakterlánc-értékek esetén JavaScript toohello (pl. operátor szerinti szűrése, alapján: ==).</span><span class="sxs-lookup"><span data-stu-id="efabd-591">This is in contrast toohello JavaScript where hello string values are implicitly casted toonumbers (based on operator, ex: ==).</span></span> <span data-ttu-id="efabd-592">Ez a beállítás nem megfelelő DocumentDB API SQL hatékony index számára elengedhetetlen.</span><span class="sxs-lookup"><span data-stu-id="efabd-592">This choice is crucial for efficient index matching in DocumentDB API SQL.</span></span> 

## <a name="parameterized-sql-queries"></a><span data-ttu-id="efabd-593">A paraméteres SQL-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="efabd-593">Parameterized SQL queries</span></span>
<span data-ttu-id="efabd-594">Cosmos DB lekérdezéseket támogat, a megszokott @ notation hello kifejezett paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="efabd-594">Cosmos DB supports queries with parameters expressed with hello familiar @ notation.</span></span> <span data-ttu-id="efabd-595">A paraméteres SQL hatékony kezelése és escape-karaktersorozat felhasználói bevitelt, megakadályozza az SQL-injektálás az adatok véletlen kitettség biztosít.</span><span class="sxs-lookup"><span data-stu-id="efabd-595">Parameterized SQL provides robust handling and escaping of user input, preventing accidental exposure of data through SQL injection.</span></span> 

<span data-ttu-id="efabd-596">Például, hogy a Vezetéknév és címállapot fogad paraméterként, és hajthat végre különböző értékek vezetékneve és a felhasználói bevitel alapján cím állapotát.</span><span class="sxs-lookup"><span data-stu-id="efabd-596">For example, you can write a query that takes last name and address state as parameters, and then execute it for various values of last name and address state based on user input.</span></span>

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

<span data-ttu-id="efabd-597">A kérelem küldheti például a paraméteres JSON lekérdezésként DB tooCosmos alább látható.</span><span class="sxs-lookup"><span data-stu-id="efabd-597">This request can then be sent tooCosmos DB as a parameterized JSON query like shown below.</span></span>

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

<span data-ttu-id="efabd-598">hello argumentum tooTOP állíthat be például a paraméteres lekérdezés alább látható.</span><span class="sxs-lookup"><span data-stu-id="efabd-598">hello argument tooTOP can be set using parameterized queries like shown below.</span></span>

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

<span data-ttu-id="efabd-599">A paraméterértékek lehet bármely érvényes JSON (karakterláncok, számok, a logikai, null, akkor is igaz, tömbök, vagy beágyazott JSON).</span><span class="sxs-lookup"><span data-stu-id="efabd-599">Parameter values can be any valid JSON (strings, numbers, Booleans, null, even arrays or nested JSON).</span></span> <span data-ttu-id="efabd-600">Is mivel Cosmos DB séma nélküli, paraméterek a rendszer nem érvényesíti bármilyen ellen.</span><span class="sxs-lookup"><span data-stu-id="efabd-600">Also since Cosmos DB is schema-less, parameters are not validated against any type.</span></span>

## <span data-ttu-id="efabd-601"><a id="BuiltinFunctions"></a>Beépített funkciók</span><span class="sxs-lookup"><span data-stu-id="efabd-601"><a id="BuiltinFunctions"></a>Built-in functions</span></span>
<span data-ttu-id="efabd-602">Cosmos DB számos beépített funkciót is támogatja a közös műveleteket, például a felhasználói függvény (UDF) lekérdezéseken belül használható.</span><span class="sxs-lookup"><span data-stu-id="efabd-602">Cosmos DB also supports a number of built-in functions for common operations, that can be used inside queries like user-defined functions (UDFs).</span></span>

| <span data-ttu-id="efabd-603">Csoport</span><span class="sxs-lookup"><span data-stu-id="efabd-603">Function group</span></span>          | <span data-ttu-id="efabd-604">Műveletek</span><span class="sxs-lookup"><span data-stu-id="efabd-604">Operations</span></span>                                                                                                                                          |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="efabd-605">Matematikai funkciók</span><span class="sxs-lookup"><span data-stu-id="efabd-605">Mathematical functions</span></span>  | <span data-ttu-id="efabd-606">ABS, felső határ, EXP, EMELET, napló, LOG10, ENERGIAGAZDÁLKODÁSI, CIKLIKUS, bejelentkezési, SQRT, SZÖGLETES, csonk, ARCCOS, ARCSIN, ATAN, ATN2, COS, tűz, fok, PI, radiánban megadott szög, EG és TAN</span><span class="sxs-lookup"><span data-stu-id="efabd-606">ABS, CEILING, EXP, FLOOR, LOG, LOG10, POWER, ROUND, SIGN, SQRT, SQUARE, TRUNC, ACOS, ASIN, ATAN, ATN2, COS, COT, DEGREES, PI, RADIANS, SIN, and TAN</span></span> |
| <span data-ttu-id="efabd-607">Írja be az ellenőrzési funkciók</span><span class="sxs-lookup"><span data-stu-id="efabd-607">Type checking functions</span></span> | <span data-ttu-id="efabd-608">IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED és IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="efabd-608">IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED, and IS_PRIMITIVE</span></span>                                                           |
| <span data-ttu-id="efabd-609">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="efabd-609">String functions</span></span>        | <span data-ttu-id="efabd-610">CONCAT, tartalmazza, megadott módon VÉGZŐDŐ, INDEX_OF, balra, hossza, alsó, LTRIM, csere, REPLIKÁLJA, NÉVKERESÉSI, jobbra, RTRIM, megadott módon KEZDŐDŐ, SUBSTRING és felső</span><span class="sxs-lookup"><span data-stu-id="efabd-610">CONCAT, CONTAINS, ENDSWITH, INDEX_OF, LEFT, LENGTH, LOWER, LTRIM, REPLACE, REPLICATE, REVERSE, RIGHT, RTRIM, STARTSWITH, SUBSTRING, and UPPER</span></span>       |
| <span data-ttu-id="efabd-611">A tömb funkciók</span><span class="sxs-lookup"><span data-stu-id="efabd-611">Array functions</span></span>         | <span data-ttu-id="efabd-612">ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH és ARRAY_SLICE</span><span class="sxs-lookup"><span data-stu-id="efabd-612">ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH, and ARRAY_SLICE</span></span>                                                                                         |
| <span data-ttu-id="efabd-613">Térbeli funkciók</span><span class="sxs-lookup"><span data-stu-id="efabd-613">Spatial functions</span></span>       | <span data-ttu-id="efabd-614">ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID és ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="efabd-614">ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID, and ST_ISVALIDDETAILED</span></span>                                                                           | 

<span data-ttu-id="efabd-615">Jelenleg használata egy felhasználói függvény (UDF), amelynek beépített függvény már elérhető, használjon hello megfelelő beépített függvény, toobe gyorsabb toorun és egyéb lesz hatékony.</span><span class="sxs-lookup"><span data-stu-id="efabd-615">If you’re currently using a user-defined function (UDF) for which a built-in function is now available, you should use hello corresponding built-in function as it is going toobe quicker toorun and more efficiently.</span></span> 

### <a name="mathematical-functions"></a><span data-ttu-id="efabd-616">Matematikai funkciók</span><span class="sxs-lookup"><span data-stu-id="efabd-616">Mathematical functions</span></span>
<span data-ttu-id="efabd-617">hello matematikai funkciók minden végezhet a számítást, a bemeneti értékek, amelyek argumentumként szolgálnak, és a visszaadandó numerikus érték alapján.</span><span class="sxs-lookup"><span data-stu-id="efabd-617">hello mathematical functions each perform a calculation, based on input values that are provided as arguments, and return a numeric value.</span></span> <span data-ttu-id="efabd-618">Itt található a támogatott beépített matematikai függvények táblázatát.</span><span class="sxs-lookup"><span data-stu-id="efabd-618">Here’s a table of supported built-in mathematical functions.</span></span>


| <span data-ttu-id="efabd-619">Használat</span><span class="sxs-lookup"><span data-stu-id="efabd-619">Usage</span></span> | <span data-ttu-id="efabd-620">Leírás</span><span class="sxs-lookup"><span data-stu-id="efabd-620">Description</span></span> |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="efabd-621">[[ABS (num_expr)](#bk_abs)</span><span class="sxs-lookup"><span data-stu-id="efabd-621">[[ABS (num_expr)](#bk_abs)</span></span> | <span data-ttu-id="efabd-622">Értéket ad vissza hello abszolút (pozitív) hello a megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-622">Returns hello absolute (positive) value of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="efabd-623">Felső határ (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-623">CEILING (num_expr)</span></span>](#bk_ceiling) | <span data-ttu-id="efabd-624">Beolvasása hello legkisebb egész szám nagyobb értékre, vagy egyenlő, hello megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-624">Returns hello smallest integer value greater than, or equal to, hello specified numeric expression.</span></span> |
| [<span data-ttu-id="efabd-625">EMELET (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-625">FLOOR (num_expr)</span></span>](#bk_floor) | <span data-ttu-id="efabd-626">Visszaadja hello legnagyobb egész szám kisebb vagy egyenlő, mint a toohello megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-626">Returns hello largest integer less than or equal toohello specified numeric expression.</span></span> |
| [<span data-ttu-id="efabd-627">EXP (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-627">EXP (num_expr)</span></span>](#bk_exp) | <span data-ttu-id="efabd-628">A megadott numerikus kifejezés hello hello hatványát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-628">Returns hello exponent of hello specified numeric expression.</span></span> |
| <span data-ttu-id="efabd-629">[NAPLÓ (num_expr [, Alap])](#bk_log)</span><span class="sxs-lookup"><span data-stu-id="efabd-629">[LOG (num_expr [,base])](#bk_log)</span></span> | <span data-ttu-id="efabd-630">Beolvasása hello természetes alapú logaritmusát hello megadva numerikus kifejezés, vagy hello segítségével hello logaritmus alapja</span><span class="sxs-lookup"><span data-stu-id="efabd-630">Returns hello natural logarithm of hello specified numeric expression, or hello logarithm using hello specified base</span></span> |
| [<span data-ttu-id="efabd-631">LOG10 (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-631">LOG10 (num_expr)</span></span>](#bk_log10) | <span data-ttu-id="efabd-632">A megadott értéket ad vissza hello 10-es logaritmikus érték hello a numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-632">Returns hello base-10 logarithmic value of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="efabd-633">KEREK (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-633">ROUND (num_expr)</span></span>](#bk_round) | <span data-ttu-id="efabd-634">Egy numerikus érték, kerekített toohello legközelebbi egész értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-634">Returns a numeric value, rounded toohello closest integer value.</span></span> |
| [<span data-ttu-id="efabd-635">CSONK (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-635">TRUNC (num_expr)</span></span>](#bk_trunc) | <span data-ttu-id="efabd-636">Egy numerikus érték, csonkolt toohello legközelebbi egész értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-636">Returns a numeric value, truncated toohello closest integer value.</span></span> |
| [<span data-ttu-id="efabd-637">SQRT (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-637">SQRT (num_expr)</span></span>](#bk_sqrt) | <span data-ttu-id="efabd-638">Hello négyzetgyökét adja vissza a hello megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-638">Returns hello square root of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="efabd-639">NÉGYZETES (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-639">SQUARE (num_expr)</span></span>](#bk_square) | <span data-ttu-id="efabd-640">Beolvasása hello négyzetes hello a megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-640">Returns hello square of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="efabd-641">ENERGIAGAZDÁLKODÁSI (num_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-641">POWER (num_expr, num_expr)</span></span>](#bk_power) | <span data-ttu-id="efabd-642">A megadott értéket ad vissza hello hatványa hello numerikus kifejezés toohello érték van megadva.</span><span class="sxs-lookup"><span data-stu-id="efabd-642">Returns hello power of hello specified numeric expression toohello value specified.</span></span> |
| [<span data-ttu-id="efabd-643">BEJELENTKEZÉSI (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-643">SIGN (num_expr)</span></span>](#bk_sign) | <span data-ttu-id="efabd-644">Beolvasása hello bejelentkezési értéket (-1, 0, 1) hello adott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-644">Returns hello sign value (-1, 0, 1) of hello specified numeric expression.</span></span> |
| [<span data-ttu-id="efabd-645">ARCCOS (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-645">ACOS (num_expr)</span></span>](#bk_acos) | <span data-ttu-id="efabd-646">Hello szög értéket ad vissza, az radiánban megadott szög, amelynek koszinusza hello megadott numerikus kifejezés; más néven koszinuszát.</span><span class="sxs-lookup"><span data-stu-id="efabd-646">Returns hello angle, in radians, whose cosine is hello specified numeric expression; also called arccosine.</span></span> |
| [<span data-ttu-id="efabd-647">ARCSIN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-647">ASIN (num_expr)</span></span>](#bk_asin) | <span data-ttu-id="efabd-648">Hello szög értéket ad vissza, az radiánban megadott szög, amelynek szinusza hello megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-648">Returns hello angle, in radians, whose sine is hello specified numeric expression.</span></span> <span data-ttu-id="efabd-649">Ez rövidítése szinuszát.</span><span class="sxs-lookup"><span data-stu-id="efabd-649">This is also called arcsine.</span></span> |
| [<span data-ttu-id="efabd-650">ATAN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-650">ATAN (num_expr)</span></span>](#bk_atan) | <span data-ttu-id="efabd-651">Hello szög értéket ad vissza, az radiánban megadott szög, amelynek tangense a hello megadott numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-651">Returns hello angle, in radians, whose tangent is hello specified numeric expression.</span></span> <span data-ttu-id="efabd-652">Ezt arkusz is nevezik.</span><span class="sxs-lookup"><span data-stu-id="efabd-652">This is also called arctangent.</span></span> |
| [<span data-ttu-id="efabd-653">ATN2 (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-653">ATN2 (num_expr)</span></span>](#bk_atn2) | <span data-ttu-id="efabd-654">Beolvasása hello szög radiánban közötti hello pozitív x tengely és hello ray pontról hello származási toohello (y, x), ahol x és y értékei hello hello a két megadott lebegőpontos kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-654">Returns hello angle, in radians, between hello positive x-axis and hello ray from hello origin toohello point (y, x), where x and y are hello values of hello two specified float expressions.</span></span> |
| [<span data-ttu-id="efabd-655">COS (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-655">COS (num_expr)</span></span>](#bk_cos) | <span data-ttu-id="efabd-656">Beolvasása hello trigonometric koszinusza hello radiánban megadott szög, hello a megadott kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-656">Returns hello trigonometric cosine of hello specified angle, in radians, in hello specified expression.</span></span> |
| [<span data-ttu-id="efabd-657">TŰZ (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-657">COT (num_expr)</span></span>](#bk_cot) | <span data-ttu-id="efabd-658">Beolvasása hello trigonometric kotangensét hello radiánban megadott szög, a hello megadva a numerikus kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-658">Returns hello trigonometric cotangent of hello specified angle, in radians, in hello specified numeric expression.</span></span> |
| [<span data-ttu-id="efabd-659">Fokban megadva (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-659">DEGREES (num_expr)</span></span>](#bk_degrees) | <span data-ttu-id="efabd-660">Értéket ad vissza megfelelő szög (fokban megadva) az radiánban megadott szög hello.</span><span class="sxs-lookup"><span data-stu-id="efabd-660">Returns hello corresponding angle in degrees for an angle specified in radians.</span></span> |
| [<span data-ttu-id="efabd-661">PI)</span><span class="sxs-lookup"><span data-stu-id="efabd-661">PI ()</span></span>](#bk_pi) | <span data-ttu-id="efabd-662">Beolvasása hello pi konstans érték.</span><span class="sxs-lookup"><span data-stu-id="efabd-662">Returns hello constant value of PI.</span></span> |
| [<span data-ttu-id="efabd-663">RADIÁNBAN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-663">RADIANS (num_expr)</span></span>](#bk_radians) | <span data-ttu-id="efabd-664">Vissza a radiánban megadott szög, ha egy numerikus kifejezés fokban, is meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="efabd-664">Returns radians when a numeric expression, in degrees, is entered.</span></span> |
| [<span data-ttu-id="efabd-665">EG (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-665">SIN (num_expr)</span></span>](#bk_sin) | <span data-ttu-id="efabd-666">Beolvasása hello trigonometric szinusza hello radiánban megadott szög, hello a megadott kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-666">Returns hello trigonometric sine of hello specified angle, in radians, in hello specified expression.</span></span> |
| [<span data-ttu-id="efabd-667">TAN (num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-667">TAN (num_expr)</span></span>](#bk_tan) | <span data-ttu-id="efabd-668">A megadott kifejezés hello bemeneti kifejezést a hello hello tangensét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-668">Returns hello tangent of hello input expression, in hello specified expression.</span></span> |

<span data-ttu-id="efabd-669">Most futtathatja például hello következő lekérdezéseket:</span><span class="sxs-lookup"><span data-stu-id="efabd-669">For example, you can now run queries like hello following:</span></span>

<span data-ttu-id="efabd-670">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-670">**Query**</span></span>

    SELECT VALUE ABS(-4)

<span data-ttu-id="efabd-671">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-671">**Results**</span></span>

    [4]

<span data-ttu-id="efabd-672">hello fő közötti Cosmos DB képest funkciók tooANSI SQL különbség, hogy tervezett toowork séma nélküli és vegyes séma adatokkal is.</span><span class="sxs-lookup"><span data-stu-id="efabd-672">hello main difference between Cosmos DB’s functions compared tooANSI SQL is that they are designed toowork well with schema-less and mixed schema data.</span></span> <span data-ttu-id="efabd-673">Például ha egy dokumentum, ahol hello Size tulajdonság hiányzik, vagy rendelkezik-e nem numerikus érték, például "Ismeretlen", majd hello dokumentum keresztül, a rendszer kihagyja helyett hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-673">For example, if you have a document where hello Size property is missing, or has a non-numeric value like “unknown”, then hello document is skipped over, instead of returning an error.</span></span>

### <a name="type-checking-functions"></a><span data-ttu-id="efabd-674">Írja be az ellenőrzési funkciók</span><span class="sxs-lookup"><span data-stu-id="efabd-674">Type checking functions</span></span>
<span data-ttu-id="efabd-675">hello típus ellenőrzési funkciók lehetővé teszik az SQL-lekérdezések lévő kifejezés toocheck hello típusú.</span><span class="sxs-lookup"><span data-stu-id="efabd-675">hello type checking functions allow you toocheck hello type of an expression within SQL queries.</span></span> <span data-ttu-id="efabd-676">Ellenőrzési funkciók lehet típusú változó vagy ismeretlen esetén használt toodetermine hello típusú tulajdonságok dokumentumok belül hello menet közben.</span><span class="sxs-lookup"><span data-stu-id="efabd-676">Type checking functions can be used toodetermine hello type of properties within documents on hello fly when it is variable or unknown.</span></span> <span data-ttu-id="efabd-677">Ez a táblázat beépített típusa támogatott funkciók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="efabd-677">Here’s a table of supported built-in type checking functions.</span></span>

<table>
<tr>
  <td><span data-ttu-id="efabd-678"><strong>Használat</strong></span><span class="sxs-lookup"><span data-stu-id="efabd-678"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="efabd-679"><strong>Leírás</strong></span><span class="sxs-lookup"><span data-stu-id="efabd-679"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="efabd-680"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="efabd-680"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (expr)</a></span></span></td>
  <td><span data-ttu-id="efabd-681">Azt jelzi, hogy ha hello érték hello típusú tömb logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="efabd-681">Returns a Boolean indicating if hello type of hello value is an array.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="efabd-682"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="efabd-682"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (expr)</a></span></span></td>
  <td><span data-ttu-id="efabd-683">Azt jelzi, hogy ha hello típusú hello érték logikai érték logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="efabd-683">Returns a Boolean indicating if hello type of hello value is a Boolean.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="efabd-684"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="efabd-684"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (expr)</a></span></span></td>
  <td><span data-ttu-id="efabd-685">Olyan logikai érték, amely azt jelzi, ha hello típusú hello érték null beolvasása.</span><span class="sxs-lookup"><span data-stu-id="efabd-685">Returns a Boolean indicating if hello type of hello value is null.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="efabd-686"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="efabd-686"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (expr)</a></span></span></td>
  <td><span data-ttu-id="efabd-687">Azt jelzi, hogy ha hello típusú hello érték egy szám logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="efabd-687">Returns a Boolean indicating if hello type of hello value is a number.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="efabd-688"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="efabd-688"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (expr)</a></span></span></td>
  <td><span data-ttu-id="efabd-689">Azt jelzi, hogy ha hello típusú hello érték egy JSON-objektum logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="efabd-689">Returns a Boolean indicating if hello type of hello value is a JSON object.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="efabd-690"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="efabd-690"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (expr)</a></span></span></td>
  <td><span data-ttu-id="efabd-691">Ha hello típusú hello érték: karakterlánc jelző logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="efabd-691">Returns a Boolean indicating if hello type of hello value is a string.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="efabd-692"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="efabd-692"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (expr)</a></span></span></td>
  <td><span data-ttu-id="efabd-693">Jelzi, ha hello tulajdonság van rendelve egy érték logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="efabd-693">Returns a Boolean indicating if hello property has been assigned a value.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="efabd-694"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (kifejezés)</a></span><span class="sxs-lookup"><span data-stu-id="efabd-694"><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (expr)</a></span></span></td>
  <td><span data-ttu-id="efabd-695">Azt jelzi, hogy ha hello típusú hello érték karakterlánc, szám, logikai érték vagy null logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="efabd-695">Returns a Boolean indicating if hello type of hello value is a string, number, Boolean or null.</span></span></td>
</tr>

</table>

<span data-ttu-id="efabd-696">Ezeket a funkciókat használ, most lekérdezéseket is futtathat hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="efabd-696">Using these functions, you can now run queries like hello following:</span></span>

<span data-ttu-id="efabd-697">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-697">**Query**</span></span>

    SELECT VALUE IS_NUMBER(-4)

<span data-ttu-id="efabd-698">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-698">**Results**</span></span>

    [true]

### <a name="string-functions"></a><span data-ttu-id="efabd-699">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="efabd-699">String functions</span></span>
<span data-ttu-id="efabd-700">hello következő skaláris függvények végrehajtania egy műveletet a bemeneti karakterlánc-érték és a karakterlánc, a numerikus és logikai értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-700">hello following scalar functions perform an operation on a string input value and return a string, numeric or Boolean value.</span></span> <span data-ttu-id="efabd-701">Itt a következő táblázat a beépített karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="efabd-701">Here's a table of built-in string functions:</span></span>

| <span data-ttu-id="efabd-702">Használat</span><span class="sxs-lookup"><span data-stu-id="efabd-702">Usage</span></span> | <span data-ttu-id="efabd-703">Leírás</span><span class="sxs-lookup"><span data-stu-id="efabd-703">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="efabd-704">A hossz (str_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-704">LENGTH (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length) |<span data-ttu-id="efabd-705">Hello karakterét hello számát adja vissza a megadott karakterlánc-kifejezés</span><span class="sxs-lookup"><span data-stu-id="efabd-705">Returns hello number of characters of hello specified string expression</span></span> |
| <span data-ttu-id="efabd-706">[CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)</span><span class="sxs-lookup"><span data-stu-id="efabd-706">[CONCAT (str_expr, str_expr [, str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)</span></span> |<span data-ttu-id="efabd-707">Egy karakterlánc, amely legalább két karakterlánc-értékek hozzáfűzésével hello eredményét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-707">Returns a string that is hello result of concatenating two or more string values.</span></span> |
| [<span data-ttu-id="efabd-708">SUBSTRING (str_expr, num_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-708">SUBSTRING (str_expr, num_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring) |<span data-ttu-id="efabd-709">Egy karakterlánc-kifejezés részét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-709">Returns part of a string expression.</span></span> |
| [<span data-ttu-id="efabd-710">(Str_expr, str_expr) startswith ELEMNEK</span><span class="sxs-lookup"><span data-stu-id="efabd-710">STARTSWITH (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith) |<span data-ttu-id="efabd-711">E hello első karaktersorozat végződik hello második jelző logikai érték beolvasása</span><span class="sxs-lookup"><span data-stu-id="efabd-711">Returns a Boolean indicating whether hello first string expression ends with hello second</span></span> |
| [<span data-ttu-id="efabd-712">Megadott módon VÉGZŐDŐ (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-712">ENDSWITH (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith) |<span data-ttu-id="efabd-713">E hello első karaktersorozat végződik hello második jelző logikai érték beolvasása</span><span class="sxs-lookup"><span data-stu-id="efabd-713">Returns a Boolean indicating whether hello first string expression ends with hello second</span></span> |
| [<span data-ttu-id="efabd-714">CONTAINS (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-714">CONTAINS (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains) |<span data-ttu-id="efabd-715">Hogy hello első karakterlánc-kifejezés második tartalmaz-e hello jelző logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="efabd-715">Returns a Boolean indicating whether hello first string expression contains hello second.</span></span> |
| [<span data-ttu-id="efabd-716">INDEX_OF (str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-716">INDEX_OF (str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of) |<span data-ttu-id="efabd-717">Hello hello második karakterlánc-kifejezés hello első megadott karakterlánc-kifejezés vagy-1 érték első előfordulásának pozícióját a indítása, ha hello karakterlánc nem található hello adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-717">Returns hello starting position of hello first occurrence of hello second string expression within hello first specified string expression, or -1 if hello string is not found.</span></span> |
| [<span data-ttu-id="efabd-718">LEFT (str_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-718">LEFT (str_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left) |<span data-ttu-id="efabd-719">Értéket ad vissza egy karakterlánc bal oldalának hello megadott hello a karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="efabd-719">Returns hello left part of a string with hello specified number of characters.</span></span> |
| [<span data-ttu-id="efabd-720">JOBB (str_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-720">RIGHT (str_expr, num_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right) |<span data-ttu-id="efabd-721">A hello jobb része egy karakterlánc beolvasása hello megadott karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="efabd-721">Returns hello right part of a string with hello specified number of characters.</span></span> |
| [<span data-ttu-id="efabd-722">LTRIM (str_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-722">LTRIM (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim) |<span data-ttu-id="efabd-723">Egy karakterlánc-kifejezés adja vissza, után eltávolítja a kezdő üres.</span><span class="sxs-lookup"><span data-stu-id="efabd-723">Returns a string expression after it removes leading blanks.</span></span> |
| [<span data-ttu-id="efabd-724">RTRIM (str_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-724">RTRIM (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim) |<span data-ttu-id="efabd-725">Egy karakterlánc-kifejezés az összes záró szóközöket csonkítása után adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-725">Returns a string expression after truncating all trailing blanks.</span></span> |
| [<span data-ttu-id="efabd-726">ALSÓ (str_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-726">LOWER (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower) |<span data-ttu-id="efabd-727">Egy karakterlánc-kifejezés nagybetűt adatok toolowercase átalakítása után adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-727">Returns a string expression after converting uppercase character data toolowercase.</span></span> |
| [<span data-ttu-id="efabd-728">FELSŐ (str_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-728">UPPER (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper) |<span data-ttu-id="efabd-729">Egy karakterlánc-kifejezés kisbetűt adatok toouppercase átalakítása után adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-729">Returns a string expression after converting lowercase character data toouppercase.</span></span> |
| [<span data-ttu-id="efabd-730">Cserélje le a (str_expr, str_expr, str_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-730">REPLACE (str_expr, str_expr, str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace) |<span data-ttu-id="efabd-731">Megadott karakterlánc-érték összes előfordulását lecseréli egy másik karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="efabd-731">Replaces all occurrences of a specified string value with another string value.</span></span> |
| [<span data-ttu-id="efabd-732">REPLIKÁLÁS (str_expr, num_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-732">REPLICATE (str_expr, num_expr)</span></span>](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-reference#bk_replicate) |<span data-ttu-id="efabd-733">A megadott számú alkalommal megismétel egy karakterlánc-érték.</span><span class="sxs-lookup"><span data-stu-id="efabd-733">Repeats a string value a specified number of times.</span></span> |
| [<span data-ttu-id="efabd-734">NÉVKERESÉSI (str_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-734">REVERSE (str_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse) |<span data-ttu-id="efabd-735">Hello fordított sorrendben egy karakterlánc értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-735">Returns hello reverse order of a string value.</span></span> |

<span data-ttu-id="efabd-736">Ezeket a funkciókat használ, most lekérdezéseket is futtathat hello hasonló.</span><span class="sxs-lookup"><span data-stu-id="efabd-736">Using these functions, you can now run queries like hello following.</span></span> <span data-ttu-id="efabd-737">Például visszatérhet hello családnév nagybetűs az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="efabd-737">For example, you can return hello family name in uppercase as follows:</span></span>

<span data-ttu-id="efabd-738">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-738">**Query**</span></span>

    SELECT VALUE UPPER(Families.id)
    FROM Families

<span data-ttu-id="efabd-739">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-739">**Results**</span></span>

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

<span data-ttu-id="efabd-740">Vagy ebben a példában például karakterlánc összefűzésére:</span><span class="sxs-lookup"><span data-stu-id="efabd-740">Or concatenate strings like in this example:</span></span>

<span data-ttu-id="efabd-741">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-741">**Query**</span></span>

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

<span data-ttu-id="efabd-742">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-742">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


<span data-ttu-id="efabd-743">Karakterlánc funkciók is használható hello záradék toofilter eredményeket, hova hello a következő példa:</span><span class="sxs-lookup"><span data-stu-id="efabd-743">String functions can also be used in hello WHERE clause toofilter results, like in hello following example:</span></span>

<span data-ttu-id="efabd-744">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-744">**Query**</span></span>

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

<span data-ttu-id="efabd-745">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-745">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a><span data-ttu-id="efabd-746">A tömb funkciók</span><span class="sxs-lookup"><span data-stu-id="efabd-746">Array functions</span></span>
<span data-ttu-id="efabd-747">a következő skaláris függvények hello végrehajtania egy műveletet a egy tömb bemeneti érték és a numerikus visszatérési, a logikai érték vagy tömb érték.</span><span class="sxs-lookup"><span data-stu-id="efabd-747">hello following scalar functions perform an operation on an array input value and return numeric, Boolean or array value.</span></span> <span data-ttu-id="efabd-748">Beépített tömb függvények táblázatát itt található:</span><span class="sxs-lookup"><span data-stu-id="efabd-748">Here's a table of built-in array functions:</span></span>

| <span data-ttu-id="efabd-749">Használat</span><span class="sxs-lookup"><span data-stu-id="efabd-749">Usage</span></span> | <span data-ttu-id="efabd-750">Leírás</span><span class="sxs-lookup"><span data-stu-id="efabd-750">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="efabd-751">ARRAY_LENGTH (arr_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-751">ARRAY_LENGTH (arr_expr)</span></span>](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length) |<span data-ttu-id="efabd-752">Hello elemeinek hello számát adja vissza a megadott tömb kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-752">Returns hello number of elements of hello specified array expression.</span></span> |
| <span data-ttu-id="efabd-753">[ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)</span><span class="sxs-lookup"><span data-stu-id="efabd-753">[ARRAY_CONCAT (arr_expr, arr_expr [, arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)</span></span> |<span data-ttu-id="efabd-754">Egy tömb, amely két vagy több tömb értékek hozzáfűzésével hello eredményét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-754">Returns an array that is hello result of concatenating two or more array values.</span></span> |
| <span data-ttu-id="efabd-755">[ARRAY_CONTAINS (arr_expr, kifejezés [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)</span><span class="sxs-lookup"><span data-stu-id="efabd-755">[ARRAY_CONTAINS (arr_expr, expr [, bool_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)</span></span> |<span data-ttu-id="efabd-756">Vissza logikai érték, amely azt jelzi, hogy tartalmaz-e hello hello tömb a megadott érték.</span><span class="sxs-lookup"><span data-stu-id="efabd-756">Returns a Boolean indicating whether hello array contains hello specified value.</span></span> <span data-ttu-id="efabd-757">Ha hello egyezés-e a teljes vagy részleges adhat meg.</span><span class="sxs-lookup"><span data-stu-id="efabd-757">Can specify if hello match is full or partial.</span></span> |
| <span data-ttu-id="efabd-758">[ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)</span><span class="sxs-lookup"><span data-stu-id="efabd-758">[ARRAY_SLICE (arr_expr, num_expr [, num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)</span></span> |<span data-ttu-id="efabd-759">Egy tömböt megadó kifejezést részét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-759">Returns part of an array expression.</span></span> |

<span data-ttu-id="efabd-760">Tömb funkciók JSON belül használt toomanipulate tömb lehet.</span><span class="sxs-lookup"><span data-stu-id="efabd-760">Array functions can be used toomanipulate arrays within JSON.</span></span> <span data-ttu-id="efabd-761">Például az ide a lekérdezés, amely visszaadja az összes dokumentumot ahol hello szülők egyik "Multiplexelés Wakefield".</span><span class="sxs-lookup"><span data-stu-id="efabd-761">For example, here's a query that returns all documents where one of hello parents is "Robin Wakefield".</span></span> 

<span data-ttu-id="efabd-762">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-762">**Query**</span></span>

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

<span data-ttu-id="efabd-763">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-763">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="efabd-764">Megadhat részleges töredékkel hello tömbön belüli egyező elemek esetében.</span><span class="sxs-lookup"><span data-stu-id="efabd-764">You can specify a partial fragment for matching elements within hello array.</span></span> <span data-ttu-id="efabd-765">hello következő lekérdezés megkeresi a hello minden szülő `givenName` a `Robin`.</span><span class="sxs-lookup"><span data-stu-id="efabd-765">hello following query finds all parents with hello `givenName` of `Robin`.</span></span>

<span data-ttu-id="efabd-766">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-766">**Query**</span></span>

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)

<span data-ttu-id="efabd-767">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-767">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]


<span data-ttu-id="efabd-768">Íme egy másik példa a által használt ARRAY_LENGTH tooget hello gyermekek termékcsalád másodpercenkénti száma.</span><span class="sxs-lookup"><span data-stu-id="efabd-768">Here's another example that uses ARRAY_LENGTH tooget hello number of children per family.</span></span>

<span data-ttu-id="efabd-769">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-769">**Query**</span></span>

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

<span data-ttu-id="efabd-770">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-770">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a><span data-ttu-id="efabd-771">Térbeli funkciók</span><span class="sxs-lookup"><span data-stu-id="efabd-771">Spatial functions</span></span>
<span data-ttu-id="efabd-772">Cosmos DB nyissa meg a földrajzi konzorcium (OGC) beépített funkciók földrajzi lekérdezése a következő hello támogatja.</span><span class="sxs-lookup"><span data-stu-id="efabd-772">Cosmos DB supports hello following Open Geospatial Consortium (OGC) built-in functions for geospatial querying.</span></span> 

<table>
<tr>
  <td><span data-ttu-id="efabd-773"><strong>Használat</strong></span><span class="sxs-lookup"><span data-stu-id="efabd-773"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="efabd-774"><strong>Leírás</strong></span><span class="sxs-lookup"><span data-stu-id="efabd-774"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="efabd-775">ST_DISTANCE (point_expr, point_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-775">ST_DISTANCE (point_expr, point_expr)</span></span></td>
  <td><span data-ttu-id="efabd-776">A két hello GeoJSON-pont, sokszög vagy LineString kifejezések hello távolság visszaadása.</span><span class="sxs-lookup"><span data-stu-id="efabd-776">Returns hello distance between hello two GeoJSON Point, Polygon, or LineString expressions.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="efabd-777">ST_WITHIN (point_expr, polygon_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-777">ST_WITHIN (point_expr, polygon_expr)</span></span></td>
  <td><span data-ttu-id="efabd-778">Egy logikai kifejezés, amely azt jelzi, hogy hello első GeoJSON objektum (pont, Polygon, vagy LineString) hello második GeoJSON objektumon belül (pont, sokszög vagy LineString) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-778">Returns a Boolean expression indicating whether hello first GeoJSON object (Point, Polygon, or LineString) is within hello second GeoJSON object (Point, Polygon, or LineString).</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="efabd-779">ST_INTERSECTS (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="efabd-779">ST_INTERSECTS (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="efabd-780">Egy logikai kifejezés, amely azt jelzi, hogy az intersect hello két megadott GeoJSON objektumokat (pont, Polygon, vagy LineString) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-780">Returns a Boolean expression indicating whether hello two specified GeoJSON objects (Point, Polygon, or LineString) intersect.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="efabd-781">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="efabd-781">ST_ISVALID</span></span></td>
  <td><span data-ttu-id="efabd-782">Jelzi, hogy hello megadott GeoJSON-pont, sokszög vagy LineString kifejezés érvényes logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="efabd-782">Returns a Boolean value indicating whether hello specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="efabd-783">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="efabd-783">ST_ISVALIDDETAILED</span></span></td>
  <td><span data-ttu-id="efabd-784">Értéket ad eredményül, ha hello megadott GeoJSON-pont, sokszög vagy LineString kifejezés logikai értéket tartalmazó JSON-érték érvényes, és ha érvénytelen, továbbá hello OK karakterláncként.</span><span class="sxs-lookup"><span data-stu-id="efabd-784">Returns a JSON value containing a Boolean value if hello specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally hello reason as a string value.</span></span></td>
</tr>
</table>

<span data-ttu-id="efabd-785">Térbeli funkciók lehet használt tooperform közelségi kapcsolat lekérdezések térbeli adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="efabd-785">Spatial functions can be used tooperform proximity queries against spatial data.</span></span> <span data-ttu-id="efabd-786">Például az itt a lekérdezés, amely visszaadja az összes termékcsalád dokumentumokat, hogy vannak 30 km-hello belül megadott helyen hello ST_DISTANCE beépített függvény használatával.</span><span class="sxs-lookup"><span data-stu-id="efabd-786">For example, here's a query that returns all family documents that are within 30 km of hello specified location using hello ST_DISTANCE built-in function.</span></span> 

<span data-ttu-id="efabd-787">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-787">**Query**</span></span>

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

<span data-ttu-id="efabd-788">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-788">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="efabd-789">A földrajzi támogatásáról Cosmos DB további részletekért lásd: [földrajzi adatok az Azure Cosmos DB](geospatial.md).</span><span class="sxs-lookup"><span data-stu-id="efabd-789">For more details on geospatial support in Cosmos DB, please see [Working with geospatial data in Azure Cosmos DB](geospatial.md).</span></span> <span data-ttu-id="efabd-790">Amely foglalja össze a térbeli funkciók és hello SQL-szintaxis, a Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="efabd-790">That wraps up spatial functions, and hello SQL syntax for Cosmos DB.</span></span> <span data-ttu-id="efabd-791">Most vessen egy pillantást, hogyan működik, és milyen hatással van az hello szintaxis lekérdezése LINQ is láttuk eddig.</span><span class="sxs-lookup"><span data-stu-id="efabd-791">Now let's take a look at how LINQ querying works and how it interacts with hello syntax we've seen so far.</span></span>

## <span data-ttu-id="efabd-792"><a id="Linq"></a>LINQ tooDocumentDB API SQL</span><span class="sxs-lookup"><span data-stu-id="efabd-792"><a id="Linq"></a>LINQ tooDocumentDB API SQL</span></span>
<span data-ttu-id="efabd-793">LINQ .NET programozási modell, amely szerint az objektumok adatfolyamok lekérdezései számítási kifejezze.</span><span class="sxs-lookup"><span data-stu-id="efabd-793">LINQ is a .NET programming model that expresses computation as queries on streams of objects.</span></span> <span data-ttu-id="efabd-794">Cosmos DB biztosít egy ügyféloldali könyvtár toointerface LINQ a JSON és a .NET objektumok közötti konverzió megkönnyítésével, és egy részhalmazát LINQ-leképezés lekérdezések tooCosmos DB lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="efabd-794">Cosmos DB provides a client-side library toointerface with LINQ by facilitating a conversion between JSON and .NET objects and a mapping from a subset of LINQ queries tooCosmos DB queries.</span></span> 

<span data-ttu-id="efabd-795">az alábbi képen hello a LINQ-lekérdezések Cosmos DB segítségével támogathatja hello architektúráját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="efabd-795">hello picture below shows hello architecture of supporting LINQ queries using Cosmos DB.</span></span>  <span data-ttu-id="efabd-796">Hello Cosmos DB-ügyfélprogram segítségével a fejlesztők hozhat létre egy **IQueryable** objektumot, hogy közvetlenül a lekérdezések hello Cosmos DB lekérdezésszolgáltató, amely ezután fordítja le hello a LINQ lekérdezés egy Cosmos-adatbázis-lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-796">Using hello Cosmos DB client, developers can create an **IQueryable** object that directly queries hello Cosmos DB query provider, which then translates hello LINQ query into a Cosmos DB query.</span></span> <span data-ttu-id="efabd-797">hello lekérdezés majd lett átadva a toohello Cosmos DB server tooretrieve eredményt a JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="efabd-797">hello query is then passed toohello Cosmos DB server tooretrieve a set of results in JSON format.</span></span> <span data-ttu-id="efabd-798">hello visszaadott eredménye egy adatfolyamba való mentésre .NET objektumok hello ügyféloldalon deszerializált.</span><span class="sxs-lookup"><span data-stu-id="efabd-798">hello returned results are deserialized into a stream of .NET objects on hello client side.</span></span>

![A LINQ-lekérdezések használata a DocumentDB API - SQL-szintaxis, JSON lekérdező nyelv, adatbázis fogalmait és az SQL-lekérdezések architektúrája][1]

### <a name="net-and-json-mapping"></a><span data-ttu-id="efabd-800">.NET és a JSON-leképezés</span><span class="sxs-lookup"><span data-stu-id="efabd-800">.NET and JSON mapping</span></span>
<span data-ttu-id="efabd-801">.NET-objektumokat és a JSON-dokumentumok közötti hello hozzárendelést természetes – minden tag adatmező le van képezve tooa JSON-objektumból, ahol hello mezőnév az hello objektum részének toohello "key", illetve az hello "érték" rész rekurzív módon csatlakoztatott toohello érték hello objektum része.</span><span class="sxs-lookup"><span data-stu-id="efabd-801">hello mapping between .NET objects and JSON documents is natural - each data member field is mapped tooa JSON object, where hello field name is mapped toohello "key" part of hello object and hello "value" part is recursively mapped toohello value part of hello object.</span></span> <span data-ttu-id="efabd-802">Vegye figyelembe a következő példa hello: hello termékcsalád létrehozott célja csatlakoztatott toohello JSON-dokumentum alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="efabd-802">Consider hello following example: hello Family object created is mapped toohello JSON document as shown below.</span></span> <span data-ttu-id="efabd-803">És ez fordítva is igaz, hello JSON-dokumentum csatlakoztatott hátsó tooa .NET objektum.</span><span class="sxs-lookup"><span data-stu-id="efabd-803">And vice versa, hello JSON document is mapped back tooa .NET object.</span></span>

<span data-ttu-id="efabd-804">**C#-osztály**</span><span class="sxs-lookup"><span data-stu-id="efabd-804">**C# Class**</span></span>

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


<span data-ttu-id="efabd-805">**JSON**</span><span class="sxs-lookup"><span data-stu-id="efabd-805">**JSON**</span></span>  

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



### <a name="linq-toosql-translation"></a><span data-ttu-id="efabd-806">LINQ tooSQL fordítás</span><span class="sxs-lookup"><span data-stu-id="efabd-806">LINQ tooSQL translation</span></span>
<span data-ttu-id="efabd-807">hello Cosmos DB lekérdezésszolgáltató hajt végre, egy Cosmos-adatbázis SQL-lekérdezést az elérhető legjobb leképezéseket a LINQ lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-807">hello Cosmos DB query provider performs a best effort mapping from a LINQ query into a Cosmos DB SQL query.</span></span> <span data-ttu-id="efabd-808">A leírás a következő hello feltételezzük, hogy hello olvasó rendelkezik a LINQ alapszintű ismeretét.</span><span class="sxs-lookup"><span data-stu-id="efabd-808">In hello following description, we assume hello reader has a basic familiarity of LINQ.</span></span>

<span data-ttu-id="efabd-809">Először hello típus rendszer esetében támogatott összes JSON egyszerű típusokhoz – numerikus típusok, logikai érték, karakterlánc vagy null.</span><span class="sxs-lookup"><span data-stu-id="efabd-809">First, for hello type system, we support all JSON primitive types – numeric types, boolean, string, and null.</span></span> <span data-ttu-id="efabd-810">Ezek a JSON típusok támogatottak.</span><span class="sxs-lookup"><span data-stu-id="efabd-810">Only these JSON types are supported.</span></span> <span data-ttu-id="efabd-811">a következő skaláris kifejezések hello támogatott.</span><span class="sxs-lookup"><span data-stu-id="efabd-811">hello following scalar expressions are supported.</span></span>

* <span data-ttu-id="efabd-812">Állandó értékek – ezek közé tartozik a primitív adattípusokat hello állandó értékek hello tartalmazó lekérdezés kiértékelése hello időpontban.</span><span class="sxs-lookup"><span data-stu-id="efabd-812">Constant values – these include constant values of hello primitive data types at hello time hello query is evaluated.</span></span>
* <span data-ttu-id="efabd-813">/ Tulajdonságtömb-index kifejezések – ezek a kifejezések toohello tulajdonság az objektum vagy tömb elem hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="efabd-813">Property/array index expressions – these expressions refer toohello property of an object or an array element.</span></span>
  
     <span data-ttu-id="efabd-814">termékcsalád. Azonosító;    Family.children[0].familyName;    Family.children[0].grade;    Family.children[n].grade; n egy int változó</span><span class="sxs-lookup"><span data-stu-id="efabd-814">family.Id;    family.children[0].familyName;    family.children[0].grade;    family.children[n].grade; //n is an int variable</span></span>
* <span data-ttu-id="efabd-815">Aritmetikai kifejezésekben - ezek közé tartozik a numerikus és logikai értékek a közös aritmetikai kifejezésekben.</span><span class="sxs-lookup"><span data-stu-id="efabd-815">Arithmetic expressions - These include common arithmetic expressions on numerical and boolean values.</span></span> <span data-ttu-id="efabd-816">Hello teljes listájáért tekintse meg a toohello SQL megadását.</span><span class="sxs-lookup"><span data-stu-id="efabd-816">For hello complete list, refer toohello SQL specification.</span></span>
  
     <span data-ttu-id="efabd-817">2 * family.children[0].grade;    az x + y;</span><span class="sxs-lookup"><span data-stu-id="efabd-817">2 * family.children[0].grade;    x + y;</span></span>
* <span data-ttu-id="efabd-818">Karakterlánc-összehasonlítási kifejezés - ilyenek összehasonlításával karakterlánc érték toosome állandó karakterlánc-érték.</span><span class="sxs-lookup"><span data-stu-id="efabd-818">String comparison expression - these include comparing a string value toosome constant string value.</span></span>  
  
     <span data-ttu-id="efabd-819">mother.familyName == "Smith";    child.givenName == s; egy karakterlánc-változóvá-je</span><span class="sxs-lookup"><span data-stu-id="efabd-819">mother.familyName == "Smith";    child.givenName == s; //s is a string variable</span></span>
* <span data-ttu-id="efabd-820">Objektum vagy tömb létrehozása kifejezés - ezek a kifejezések visszatérési összetett érték vagy névtelen típusú objektum vagy egy ilyen objektumokból álló tömb.</span><span class="sxs-lookup"><span data-stu-id="efabd-820">Object/array creation expression - these expressions return an object of compound value type or anonymous type or an array of such objects.</span></span> <span data-ttu-id="efabd-821">Ezek az értékek egymásba ágyazható.</span><span class="sxs-lookup"><span data-stu-id="efabd-821">These values can be nested.</span></span>
  
     <span data-ttu-id="efabd-822">új szülő {familyName = a "Smith" givenName = "Joe"}; új {első = 1, a második = 2}; egy két mező rendelkező névtelen típusú</span><span class="sxs-lookup"><span data-stu-id="efabd-822">new Parent { familyName = "Smith", givenName = "Joe" }; new { first = 1, second = 2 }; //an anonymous type with two fields</span></span>              
     <span data-ttu-id="efabd-823">új int [] {3, child.grade, 5};</span><span class="sxs-lookup"><span data-stu-id="efabd-823">new int[] { 3, child.grade, 5 };</span></span>

### <span data-ttu-id="efabd-824"><a id="SupportedLinqOperators"></a>Támogatott LINQ operátorok listája</span><span class="sxs-lookup"><span data-stu-id="efabd-824"><a id="SupportedLinqOperators"></a>List of supported LINQ operators</span></span>
<span data-ttu-id="efabd-825">Ez egy lista támogatott LINQ üzemeltetők a DocumentDB .NET SDK hello mellékelt hello LINQ szolgáltatónál.</span><span class="sxs-lookup"><span data-stu-id="efabd-825">Here is a list of supported LINQ operators in hello LINQ provider included with hello DocumentDB .NET SDK.</span></span>

* <span data-ttu-id="efabd-826">**Válassza ki**: leképezések fordítása SQL SELECT objektumkonstrukciók beleértve toohello</span><span class="sxs-lookup"><span data-stu-id="efabd-826">**Select**: Projections translate toohello SQL SELECT including object construction</span></span>
* <span data-ttu-id="efabd-827">**Ha**: szűrők, amelyek SQL WHERE toohello, és támogatja a közötti címfordítás & &, || és!</span><span class="sxs-lookup"><span data-stu-id="efabd-827">**Where**: Filters translate toohello SQL WHERE, and support translation between && , || and !</span></span> <span data-ttu-id="efabd-828">toohello SQL operátorok</span><span class="sxs-lookup"><span data-stu-id="efabd-828">toohello SQL operators</span></span>
* <span data-ttu-id="efabd-829">**A selectmany metódus**: lehetővé teszi a tömbök toohello SQL JOIN záradékban visszagörgetésének.</span><span class="sxs-lookup"><span data-stu-id="efabd-829">**SelectMany**: Allows unwinding of arrays toohello SQL JOIN clause.</span></span> <span data-ttu-id="efabd-830">A tömb elemeinek használt toochain/nest kifejezések toofilter lehet</span><span class="sxs-lookup"><span data-stu-id="efabd-830">Can be used toochain/nest expressions toofilter on array elements</span></span>
* <span data-ttu-id="efabd-831">**OrderBy és OrderByDescending**: az eszköz tooORDER BY növekvő/csökkenő</span><span class="sxs-lookup"><span data-stu-id="efabd-831">**OrderBy and OrderByDescending**: Translates tooORDER BY ascending/descending</span></span>
* <span data-ttu-id="efabd-832">**Count**, **Sum**, **Min**, **maximális**, és **átlagos** összesítő és a megfelelő aszinkron operátorok **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, és **AverageAsync**.</span><span class="sxs-lookup"><span data-stu-id="efabd-832">**Count**, **Sum**, **Min**, **Max**, and **Average** operators for aggregation, and their async equivalents **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, and **AverageAsync**.</span></span>
* <span data-ttu-id="efabd-833">**CompareTo**: az eszköz toorange összehasonlítást.</span><span class="sxs-lookup"><span data-stu-id="efabd-833">**CompareTo**: Translates toorange comparisons.</span></span> <span data-ttu-id="efabd-834">Gyakran használt karakterláncok óta fontosságúak nem hasonlítható össze az .NET</span><span class="sxs-lookup"><span data-stu-id="efabd-834">Commonly used for strings since they’re not comparable in .NET</span></span>
* <span data-ttu-id="efabd-835">**Igénybe**: toohello SQL felső lefordítja a lekérdezés eredményeinek korlátozása</span><span class="sxs-lookup"><span data-stu-id="efabd-835">**Take**: Translates toohello SQL TOP for limiting results from a query</span></span>
* <span data-ttu-id="efabd-836">**Matematikai függvények**: támogatja a fordítás. NET tartozó Abs, ARCCOS, ARCSIN, Atan Cos felső határa, Exp, emelet, napló, Log10, Pow, ciklikus, bejelentkezési, EG, Sqrt, Tan, Truncate toohello egyenértékű SQL beépített funkciók.</span><span class="sxs-lookup"><span data-stu-id="efabd-836">**Math Functions**: Supports translation from .NET’s Abs, Acos, Asin, Atan, Ceiling, Cos, Exp, Floor, Log, Log10, Pow, Round, Sign, Sin, Sqrt, Tan, Truncate toohello equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="efabd-837">**Karakterlánc**: támogatja a fordítás. NET tartozó Concat, Contains, megadott módon végződő, IndexOf, Count, ToLower, TrimStart, csere, névkeresési, TrimEnd, megadott módon kezdődő, SubString, ToUpper toohello egyenértékű SQL beépített funkciók.</span><span class="sxs-lookup"><span data-stu-id="efabd-837">**String Functions**: Supports translation from .NET’s Concat, Contains, EndsWith, IndexOf, Count, ToLower, TrimStart, Replace, Reverse, TrimEnd, StartsWith, SubString, ToUpper toohello equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="efabd-838">**A tömb funkciók**: támogatja a fordítás. NET tartozó Concat, tartalmazza, és a Count toohello egyenértékű SQL beépített függvény.</span><span class="sxs-lookup"><span data-stu-id="efabd-838">**Array Functions**: Supports translation from .NET’s Concat, Contains, and Count toohello equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="efabd-839">**A földrajzi Kiterjesztésfüggvények**: támogatja a helyettes módszerek távolság belül IsValid, a fordítás és IsValidDetailed toohello egyenértékű SQL beépített funkciók.</span><span class="sxs-lookup"><span data-stu-id="efabd-839">**Geospatial Extension Functions**: Supports translation from stub methods Distance, Within, IsValid, and IsValidDetailed toohello equivalent SQL built-in functions.</span></span>
* <span data-ttu-id="efabd-840">**Felhasználó által definiált függvény kiterjesztésfüggvény**: támogatja a fordítás a hello helyettes metódus UserDefinedFunctionProvider.Invoke toohello megfelelő felhasználó által definiált függvény.</span><span class="sxs-lookup"><span data-stu-id="efabd-840">**User-Defined Function Extension Function**: Supports translation from hello stub method UserDefinedFunctionProvider.Invoke toohello corresponding user-defined function.</span></span>
* <span data-ttu-id="efabd-841">**Vegyes**: hello fordítása coalesce támogatja, és a feltételes operátort.</span><span class="sxs-lookup"><span data-stu-id="efabd-841">**Miscellaneous**: Supports translation of hello coalesce and conditional operators.</span></span> <span data-ttu-id="efabd-842">Contains tooString lefordíthatja tartalmazza, ARRAY_CONTAINS vagy hello SQL IN attól függően, hogy a környezetben.</span><span class="sxs-lookup"><span data-stu-id="efabd-842">Can translate Contains tooString CONTAINS, ARRAY_CONTAINS, or hello SQL IN depending on context.</span></span>

### <a name="sql-query-operators"></a><span data-ttu-id="efabd-843">SQL-lekérdezési operátorok</span><span class="sxs-lookup"><span data-stu-id="efabd-843">SQL query operators</span></span>
<span data-ttu-id="efabd-844">Íme néhány példa, amelyek bemutatják, hogyan lefordítani néhány hello szabványos LINQ lekérdezési operátorok az tooCosmos DB lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="efabd-844">Here are some examples that illustrate how some of hello standard LINQ query operators are translated down tooCosmos DB queries.</span></span>

#### <a name="select-operator"></a><span data-ttu-id="efabd-845">Válasszon operátort</span><span class="sxs-lookup"><span data-stu-id="efabd-845">Select Operator</span></span>
<span data-ttu-id="efabd-846">hello szintaxisa `input.Select(x => f(x))`, ahol `f` egy skaláris kifejezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-846">hello syntax is `input.Select(x => f(x))`, where `f` is a scalar expression.</span></span>

<span data-ttu-id="efabd-847">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-847">**LINQ lambda expression**</span></span>

    input.Select(family => family.parents[0].familyName);

<span data-ttu-id="efabd-848">**SQL**</span><span class="sxs-lookup"><span data-stu-id="efabd-848">**SQL**</span></span> 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



<span data-ttu-id="efabd-849">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-849">**LINQ lambda expression**</span></span>

    input.Select(family => family.children[0].grade + c); // c is an int variable


<span data-ttu-id="efabd-850">**SQL**</span><span class="sxs-lookup"><span data-stu-id="efabd-850">**SQL**</span></span> 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



<span data-ttu-id="efabd-851">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-851">**LINQ lambda expression**</span></span>

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


<span data-ttu-id="efabd-852">**SQL**</span><span class="sxs-lookup"><span data-stu-id="efabd-852">**SQL**</span></span> 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a><span data-ttu-id="efabd-853">A selectmany metódus operátor</span><span class="sxs-lookup"><span data-stu-id="efabd-853">SelectMany operator</span></span>
<span data-ttu-id="efabd-854">hello szintaxisa `input.SelectMany(x => f(x))`, ahol `f` egy skaláris kifejezés, amely a gyűjteménytípus adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-854">hello syntax is `input.SelectMany(x => f(x))`, where `f` is a scalar expression that returns a collection type.</span></span>

<span data-ttu-id="efabd-855">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-855">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.children);

<span data-ttu-id="efabd-856">**SQL**</span><span class="sxs-lookup"><span data-stu-id="efabd-856">**SQL**</span></span> 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a><span data-ttu-id="efabd-857">Ha operátor</span><span class="sxs-lookup"><span data-stu-id="efabd-857">Where operator</span></span>
<span data-ttu-id="efabd-858">hello szintaxisa `input.Where(x => f(x))`, ahol `f` van egy skaláris kifejezés, amely egy logikai értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-858">hello syntax is `input.Where(x => f(x))`, where `f` is a scalar expression, which returns a Boolean value.</span></span>

<span data-ttu-id="efabd-859">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-859">**LINQ lambda expression**</span></span>

    input.Where(family=> family.parents[0].familyName == "Smith");

<span data-ttu-id="efabd-860">**SQL**</span><span class="sxs-lookup"><span data-stu-id="efabd-860">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



<span data-ttu-id="efabd-861">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-861">**LINQ lambda expression**</span></span>

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

<span data-ttu-id="efabd-862">**SQL**</span><span class="sxs-lookup"><span data-stu-id="efabd-862">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a><span data-ttu-id="efabd-863">Összetett SQL-lekérdezések</span><span class="sxs-lookup"><span data-stu-id="efabd-863">Composite SQL queries</span></span>
<span data-ttu-id="efabd-864">operátorok fent hello lehet össze tooform nagyobb teljesítményű lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="efabd-864">hello above operators can be composed tooform more powerful queries.</span></span> <span data-ttu-id="efabd-865">Mivel Cosmos DB támogatja a beágyazott gyűjtemények, hello összeállítás kell összefűzendő vagy beágyazott.</span><span class="sxs-lookup"><span data-stu-id="efabd-865">Since Cosmos DB supports nested collections, hello composition can either be concatenated or nested.</span></span>

#### <a name="concatenation"></a><span data-ttu-id="efabd-866">Összefűzése</span><span class="sxs-lookup"><span data-stu-id="efabd-866">Concatenation</span></span>
<span data-ttu-id="efabd-867">hello szintaxisa `input(.|.SelectMany())(.Select()|.Where())*`.</span><span class="sxs-lookup"><span data-stu-id="efabd-867">hello syntax is `input(.|.SelectMany())(.Select()|.Where())*`.</span></span> <span data-ttu-id="efabd-868">Olyan összefűzött lekérdezés elindíthatja és egy opcionális `SelectMany` lekérdezés követ több `Select` vagy `Where` operátorok.</span><span class="sxs-lookup"><span data-stu-id="efabd-868">A concatenated query can start with an optional `SelectMany` query followed by multiple `Select` or `Where` operators.</span></span>

<span data-ttu-id="efabd-869">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-869">**LINQ lambda expression**</span></span>

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

<span data-ttu-id="efabd-870">**SQL**</span><span class="sxs-lookup"><span data-stu-id="efabd-870">**SQL**</span></span>

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



<span data-ttu-id="efabd-871">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-871">**LINQ lambda expression**</span></span>

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

<span data-ttu-id="efabd-872">**SQL**</span><span class="sxs-lookup"><span data-stu-id="efabd-872">**SQL**</span></span> 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



<span data-ttu-id="efabd-873">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-873">**LINQ lambda expression**</span></span>

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);

<span data-ttu-id="efabd-874">**SQL**</span><span class="sxs-lookup"><span data-stu-id="efabd-874">**SQL**</span></span> 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



<span data-ttu-id="efabd-875">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-875">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

<span data-ttu-id="efabd-876">**SQL**</span><span class="sxs-lookup"><span data-stu-id="efabd-876">**SQL**</span></span> 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a><span data-ttu-id="efabd-877">A beágyazási</span><span class="sxs-lookup"><span data-stu-id="efabd-877">Nesting</span></span>
<span data-ttu-id="efabd-878">hello szintaxisa `input.SelectMany(x=>x.Q())` Q esetén egy `Select`, `SelectMany`, vagy `Where` operátor.</span><span class="sxs-lookup"><span data-stu-id="efabd-878">hello syntax is `input.SelectMany(x=>x.Q())` where Q is a `Select`, `SelectMany`, or `Where` operator.</span></span>

<span data-ttu-id="efabd-879">Egy beágyazott lekérdezésen hello belső lekérdezés hello külső gyűjtemény alkalmazott tooeach eleme.</span><span class="sxs-lookup"><span data-stu-id="efabd-879">In a nested query, hello inner query is applied tooeach element of hello outer collection.</span></span> <span data-ttu-id="efabd-880">Egyik fontos szolgáltatása hello belső lekérdezés hivatkozhat toohello mezők hello külső gyűjtemény például hello elemszámának Önillesztések.</span><span class="sxs-lookup"><span data-stu-id="efabd-880">One important feature is that hello inner query can refer toohello fields of hello elements in hello outer collection like self-joins.</span></span>

<span data-ttu-id="efabd-881">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-881">**LINQ lambda expression**</span></span>

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

<span data-ttu-id="efabd-882">**SQL**</span><span class="sxs-lookup"><span data-stu-id="efabd-882">**SQL**</span></span> 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


<span data-ttu-id="efabd-883">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-883">**LINQ lambda expression**</span></span>

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));

<span data-ttu-id="efabd-884">**SQL**</span><span class="sxs-lookup"><span data-stu-id="efabd-884">**SQL**</span></span> 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



<span data-ttu-id="efabd-885">**LINQ lambda kifejezés**</span><span class="sxs-lookup"><span data-stu-id="efabd-885">**LINQ lambda expression**</span></span>

    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

<span data-ttu-id="efabd-886">**SQL**</span><span class="sxs-lookup"><span data-stu-id="efabd-886">**SQL**</span></span> 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <span data-ttu-id="efabd-887"><a id="ExecutingSqlQueries"></a>SQL-lekérdezések végrehajtása</span><span class="sxs-lookup"><span data-stu-id="efabd-887"><a id="ExecutingSqlQueries"></a>Executing SQL queries</span></span>
<span data-ttu-id="efabd-888">A cosmos DB keresztül tesz elérhetővé erőforrásokat egy REST API-t, amely képes a HTTP/HTTPS-kérést bármely olyan nyelvvel hívható.</span><span class="sxs-lookup"><span data-stu-id="efabd-888">Cosmos DB exposes resources through a REST API that can be called by any language capable of making HTTP/HTTPS requests.</span></span> <span data-ttu-id="efabd-889">Ezenfelül a Cosmos DB programozási kódtárakat, például a .NET, Node.js, JavaScript és Python számos népszerű nyelvhez biztosít.</span><span class="sxs-lookup"><span data-stu-id="efabd-889">Additionally, Cosmos DB offers programming libraries for several popular languages like .NET, Node.js, JavaScript, and Python.</span></span> <span data-ttu-id="efabd-890">hello REST API-t és hello különböző szalagtárak összes támogatja az SQL keresztül lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="efabd-890">hello REST API and hello various libraries all support querying through SQL.</span></span> <span data-ttu-id="efabd-891">hello .NET SDK lekérdezése továbbá tooSQL LINQ támogatja.</span><span class="sxs-lookup"><span data-stu-id="efabd-891">hello .NET SDK supports LINQ querying in addition tooSQL.</span></span>

<span data-ttu-id="efabd-892">a következő példák szemléltetik hogyan hello toocreate egy lekérdezést, és küldje el egy Cosmos-adatbázis adatbázis-fiók.</span><span class="sxs-lookup"><span data-stu-id="efabd-892">hello following examples show how toocreate a query and submit it against a Cosmos DB database account.</span></span>

### <span data-ttu-id="efabd-893"><a id="RestAPI"></a>REST API-N</span><span class="sxs-lookup"><span data-stu-id="efabd-893"><a id="RestAPI"></a>REST API</span></span>
<span data-ttu-id="efabd-894">Cosmos DB egy megnyitott RESTful programozási modellt biztosít a HTTP Protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="efabd-894">Cosmos DB offers an open RESTful programming model over HTTP.</span></span> <span data-ttu-id="efabd-895">Adatbázis-fiókok egy Azure-előfizetés használatával telepíthető.</span><span class="sxs-lookup"><span data-stu-id="efabd-895">Database accounts can be provisioned using an Azure subscription.</span></span> <span data-ttu-id="efabd-896">hello Cosmos DB erőforrás-modellje egy adatbázis-fiók, amelyek egy-címezhető logikai és állandó URI-k használata alatt lévő erőforrások készlete áll.</span><span class="sxs-lookup"><span data-stu-id="efabd-896">hello Cosmos DB resource model consists of a set of resources under a database account, each  of which is addressable using a logical and stable URI.</span></span> <span data-ttu-id="efabd-897">Erőforráscsoport adatcsatornára ebben a dokumentumban említett tooas.</span><span class="sxs-lookup"><span data-stu-id="efabd-897">A set of resources is referred tooas a feed in this document.</span></span> <span data-ttu-id="efabd-898">Az adatbázisfiók áll az adatbázisok, mindegyike több gyűjteményt, mely szolgálna mindegyikének tartalmazza a dokumentumok, a felhasználó által megadott függvények és a más típusú erőforrások.</span><span class="sxs-lookup"><span data-stu-id="efabd-898">A database account consists of a set of databases, each containing multiple collections, each of which in-turn contain documents, UDFs, and other resource types.</span></span>

<span data-ttu-id="efabd-899">Ezekkel az erőforrásokkal hello alapvető interakció modell keresztül hello HTTP-műveletek GET, PUT, POST és DELETE a szabványos tolmácsolási szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="efabd-899">hello basic interaction model with these resources is through hello HTTP verbs GET, PUT, POST, and DELETE with their standard interpretation.</span></span> <span data-ttu-id="efabd-900">hello POST művelet egy új erőforrást, egy tárolt eljárás végrehajtása vagy egy Cosmos-adatbázis-lekérdezés kiadására szolgál.</span><span class="sxs-lookup"><span data-stu-id="efabd-900">hello POST verb is used for creation of a new resource, for executing a stored procedure or for issuing a Cosmos DB query.</span></span> <span data-ttu-id="efabd-901">Lekérdezéseket a rendszer mindig csak olvasható műveletekhez, nincs mellékhatásokkal.</span><span class="sxs-lookup"><span data-stu-id="efabd-901">Queries are always read-only operations with no side-effects.</span></span>

<span data-ttu-id="efabd-902">hello következő példák azt szemléltetik, amennyiben azt már áttekintette felé irányuló hello két minta dokumentumok tartalmazó gyűjtemény DocumentDB API lekérdezés POST.</span><span class="sxs-lookup"><span data-stu-id="efabd-902">hello following examples show a POST for a DocumentDB API query made against a collection containing hello two sample documents we've reviewed so far.</span></span> <span data-ttu-id="efabd-903">hello lekérdezés egy egyszerű szűrési hello JSON name tulajdonsággal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="efabd-903">hello query has a simple filter on hello JSON name property.</span></span> <span data-ttu-id="efabd-904">Vegye figyelembe a hello hello használata `x-ms-documentdb-isquery` és a Content-Type: `application/query+json` fejlécek toodenote, amely hello a művelethez az a lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-904">Note hello use of hello `x-ms-documentdb-isquery` and Content-Type: `application/query+json` headers toodenote that hello operation is a query.</span></span>

<span data-ttu-id="efabd-905">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="efabd-905">**Request**</span></span>

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


<span data-ttu-id="efabd-906">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-906">**Results**</span></span>

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


<span data-ttu-id="efabd-907">hello második példáját mutatja egy összetettebb lekérdezés, amely több eredményt ad vissza hello illesztési.</span><span class="sxs-lookup"><span data-stu-id="efabd-907">hello second example shows a more complex query that returns multiple results from hello join.</span></span>

<span data-ttu-id="efabd-908">**Kérés**</span><span class="sxs-lookup"><span data-stu-id="efabd-908">**Request**</span></span>

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


<span data-ttu-id="efabd-909">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="efabd-909">**Results**</span></span>

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


<span data-ttu-id="efabd-910">Ha a lekérdezés eredményei nem férnek el az eredmények egyoldalas belül, akkor hello REST API-t adja vissza a folytatási kód keresztül hello `x-ms-continuation-token` válaszfejlécet.</span><span class="sxs-lookup"><span data-stu-id="efabd-910">If a query's results cannot fit within a single page of results, then hello REST API returns a continuation token through hello `x-ms-continuation-token` response header.</span></span> <span data-ttu-id="efabd-911">Ügyfelek későbbi eredmények hello fejléc-ot is megjelenítheti az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="efabd-911">Clients can paginate results by including hello header in subsequent results.</span></span> <span data-ttu-id="efabd-912">hello száma laponként eredmények is szabályozható hello `x-ms-max-item-count` számú fejléc.</span><span class="sxs-lookup"><span data-stu-id="efabd-912">hello number of results per page can also be controlled through hello `x-ms-max-item-count` number header.</span></span> <span data-ttu-id="efabd-913">Ha hello a megadott lekérdezés tartalmaz egy összesítő függvényt, például `COUNT`, majd hello lekérdezés lap hello oldalra, egy részben összesített értéket adhat vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-913">If hello specified query has an aggregation function like `COUNT`, then hello query page may return a partially aggregated value over hello page of results.</span></span> <span data-ttu-id="efabd-914">hello ügyfelek kell az eredmények tooproduce hello végső eredményeket, például egy második szintű összesítő képes, összeg keresztül hello számok hello oldalakra tooreturn hello teljes számot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-914">hello clients must perform a second-level aggregation over these results tooproduce hello final results, for example, sum over hello counts returned in hello individual pages tooreturn hello total count.</span></span>

<span data-ttu-id="efabd-915">toomanage hello adatok konzisztencia házirend lekérdezések esetén használjon hello `x-ms-consistency-level` például minden REST API-kérés fejlécének.</span><span class="sxs-lookup"><span data-stu-id="efabd-915">toomanage hello data consistency policy for queries, use hello `x-ms-consistency-level` header like all REST API requests.</span></span> <span data-ttu-id="efabd-916">A munkamenet-konzisztencia esetén szükséges tooalso echo hello legújabb `x-ms-session-token` Cookie-fejlécének hello lekérdezési kérelemben.</span><span class="sxs-lookup"><span data-stu-id="efabd-916">For session consistency, it is required tooalso echo hello latest `x-ms-session-token` Cookie header in hello query request.</span></span> <span data-ttu-id="efabd-917">hello lekérdezett gyűjtemény indexelési házirendet is befolyásolhatják a lekérdezés eredményeinek hello konzisztencia.</span><span class="sxs-lookup"><span data-stu-id="efabd-917">hello queried collection's indexing policy can also influence hello consistency of query results.</span></span> <span data-ttu-id="efabd-918">Hello alapértelmezett házirend-beállítások indexelő, gyűjtemények hello az index az mindig naprakész lesz hello dokumentum tartalma és a lekérdezési eredmények felel meg az adatok választott hello konzisztencia.</span><span class="sxs-lookup"><span data-stu-id="efabd-918">With hello default indexing policy settings, for collections hello index is always current with hello document contents and query results match hello consistency chosen for data.</span></span> <span data-ttu-id="efabd-919">Ha indexelés házirend hello laza tooLazy, lekérdezések elavult eredmények adhat vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-919">If hello indexing policy is relaxed tooLazy, then queries can return stale results.</span></span> <span data-ttu-id="efabd-920">További információkért lásd: [Azure Cosmos DB Konzisztenciaszintek][consistency-levels].</span><span class="sxs-lookup"><span data-stu-id="efabd-920">For more information, see [Azure Cosmos DB Consistency Levels][consistency-levels].</span></span>

<span data-ttu-id="efabd-921">Ha indexelési házirendet konfigurált hello hello gyűjteményen hello megadott lekérdezés nem támogatja, a hello Azure Cosmos adatbázis-kiszolgálót 400 "hibás kérelem" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-921">If hello configured indexing policy on hello collection cannot support hello specified query, hello Azure Cosmos DB server returns 400 "Bad Request".</span></span> <span data-ttu-id="efabd-922">A kivonatoló (egyenlő) keresések, és kifejezetten kizárja indexelő elérési út beállítva elérési utak tartomány lekérdezések esetében adja vissza.</span><span class="sxs-lookup"><span data-stu-id="efabd-922">This is returned for range queries against paths configured for hash (equality) lookups, and for paths explicitly excluded from indexing.</span></span> <span data-ttu-id="efabd-923">Hello `x-ms-documentdb-query-enable-scan` fejléc lehet a megadott tooallow hello lekérdezés tooperform a vizsgálat nem érhető el index.</span><span class="sxs-lookup"><span data-stu-id="efabd-923">hello `x-ms-documentdb-query-enable-scan` header can be specified tooallow hello query tooperform a scan when an index is not available.</span></span>

<span data-ttu-id="efabd-924">Úgy, hogy a lekérdezés-végrehajtás részletes metrikák is ki `x-ms-documentdb-populatequerymetrics` fejléc túl`True`.</span><span class="sxs-lookup"><span data-stu-id="efabd-924">You can get detailed metrics on query execution by setting `x-ms-documentdb-populatequerymetrics` header too`True`.</span></span> <span data-ttu-id="efabd-925">További információkért lásd: [Azure Cosmos DB DocumentDB API SQL-lekérdezés metrikáját](documentdb-sql-query-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="efabd-925">For more information, see [SQL query metrics for Azure Cosmos DB DocumentDB API](documentdb-sql-query-metrics.md).</span></span>

### <span data-ttu-id="efabd-926"><a id="DotNetSdk"></a>C# (.NET) SDK</span><span class="sxs-lookup"><span data-stu-id="efabd-926"><a id="DotNetSdk"></a>C# (.NET) SDK</span></span>
<span data-ttu-id="efabd-927">hello .NET SDK támogatja a LINQ és az SQL lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="efabd-927">hello .NET SDK supports both LINQ and SQL querying.</span></span> <span data-ttu-id="efabd-928">hello következő példa bemutatja, hogyan tooperform hello egyszerű szűrő lekérdezés rendszerben jelent meg a jelen dokumentum korábbi.</span><span class="sxs-lookup"><span data-stu-id="efabd-928">hello following example shows how tooperform hello simple filter query introduced earlier in this document.</span></span>

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


<span data-ttu-id="efabd-929">Ez a minta összehasonlítja két tulajdonságainak egyenlőség minden a dokumentumban, és névtelen leképezések használja.</span><span class="sxs-lookup"><span data-stu-id="efabd-929">This sample compares two properties for equality within each document and uses anonymous projections.</span></span> 

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


<span data-ttu-id="efabd-930">hello a következő példa bemutatja illesztések, LINQ selectmany metódus használatával.</span><span class="sxs-lookup"><span data-stu-id="efabd-930">hello next sample shows joins, expressed through LINQ SelectMany.</span></span>

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



<span data-ttu-id="efabd-931">hello .NET ügyfél automatikusan a lekérdezési eredmények hello foreach blokkokban a fentiek szerint minden hello oldal telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="efabd-931">hello .NET client automatically iterates through all hello pages of query results in hello foreach blocks as shown above.</span></span> <span data-ttu-id="efabd-932">hello lekérdezés hello REST API-t a szakaszban bemutatott beállításokat is rendelkezésre állnak a .NET SDK használatával hello hello `FeedOptions` és `FeedResponse` hello CreateDocumentQuery metódus az osztályokat.</span><span class="sxs-lookup"><span data-stu-id="efabd-932">hello query options introduced in hello REST API section are also available in hello .NET SDK using hello `FeedOptions` and `FeedResponse` classes in hello CreateDocumentQuery method.</span></span> <span data-ttu-id="efabd-933">lapok száma hello vezérelhető hello `MaxItemCount` beállítást.</span><span class="sxs-lookup"><span data-stu-id="efabd-933">hello number of pages can be controlled using hello `MaxItemCount` setting.</span></span> 

<span data-ttu-id="efabd-934">Lapozófájl létrehozásával közvetlenül is szabályozhatja `IDocumentQueryable` hello segítségével `IQueryable` objektumot, majd ehhez beolvassa a` ResponseContinuationToken` gépet értékeket, és átadja őket `RequestContinuationToken` a `FeedOptions`.</span><span class="sxs-lookup"><span data-stu-id="efabd-934">You can also explicitly control paging by creating `IDocumentQueryable` using hello `IQueryable` object, then by reading the` ResponseContinuationToken` values and passing them back as `RequestContinuationToken` in `FeedOptions`.</span></span> <span data-ttu-id="efabd-935">`EnableScanInQuery`set tooenable vizsgálatok akkor is, ha hello lekérdezés konfigurált hello indexelési házirend által nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="efabd-935">`EnableScanInQuery` can be set tooenable scans when hello query cannot be supported by hello configured indexing policy.</span></span> <span data-ttu-id="efabd-936">A particionált gyűjtemények használhatják `PartitionKey` toorun hello lekérdezése egyetlen partícióazonosító (bár a Cosmos DB automatikusan nyerhet ki ez a lekérdezés szövegének hello), és `EnableCrossPartitionQuery` esetleg toobe toorun lekérdezéseket futtathat több partíciót.</span><span class="sxs-lookup"><span data-stu-id="efabd-936">For partitioned collections, you can use `PartitionKey` toorun hello query against a single partition (though Cosmos DB can automatically extract this from hello query text), and `EnableCrossPartitionQuery` toorun queries that may need toobe run against multiple partitions.</span></span> 

<span data-ttu-id="efabd-937">Tekintse meg a túl[Azure Cosmos DB .NET minták](https://github.com/Azure/azure-documentdb-net) további mintákat tartalmazó lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="efabd-937">Refer too[Azure Cosmos DB .NET samples](https://github.com/Azure/azure-documentdb-net) for more samples containing queries.</span></span> 

### <span data-ttu-id="efabd-938"><a id="JavaScriptServerSideApi"></a>JavaScript kiszolgálóoldali API</span><span class="sxs-lookup"><span data-stu-id="efabd-938"><a id="JavaScriptServerSideApi"></a>JavaScript server-side API</span></span>
<span data-ttu-id="efabd-939">A cosmos DB hajthatók végre alapú JavaScript-alkalmazáslogikát tárolt eljárások és eseményindítók hello gyűjtemények közvetlenül a programozási modellt biztosít.</span><span class="sxs-lookup"><span data-stu-id="efabd-939">Cosmos DB provides a programming model for executing JavaScript based application logic directly on hello collections using stored procedures and triggers.</span></span> <span data-ttu-id="efabd-940">hello JavaScript-logika regisztrálva, a gyűjtemény szintjén majd adhatnak ki a megadott gyűjtemény hello hello hello dokumentumok műveleteket a Helyadatbázis-műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="efabd-940">hello JavaScript logic registered at a collection level can then issue database operations on hello operations on hello documents of hello given collection.</span></span> <span data-ttu-id="efabd-941">Ezek a műveletek a környezeti ACID-tranzakciókat van burkolva.</span><span class="sxs-lookup"><span data-stu-id="efabd-941">These operations are wrapped in ambient ACID transactions.</span></span>

<span data-ttu-id="efabd-942">hello következő példa bemutatja, hogyan toouse hello queryDocuments a JavaScript-kiszolgáló API hello toomake lekérdezi a vállalaton belüli tárolt eljárások és eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="efabd-942">hello following example shows how toouse hello queryDocuments in hello JavaScript server API toomake queries from inside stored procedures and triggers.</span></span>

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

                        // Replace hello author name for all documents that satisfied hello query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need tooexecute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <span data-ttu-id="efabd-943"><a id="References"></a>Hivatkozások</span><span class="sxs-lookup"><span data-stu-id="efabd-943"><a id="References"></a>References</span></span>
1. <span data-ttu-id="efabd-944">[Bevezetés tooAzure Cosmos DB][introduction]</span><span class="sxs-lookup"><span data-stu-id="efabd-944">[Introduction tooAzure Cosmos DB][introduction]</span></span>
2. [<span data-ttu-id="efabd-945">Az Azure Cosmos adatbázis SQL-specifikációja</span><span class="sxs-lookup"><span data-stu-id="efabd-945">Azure Cosmos DB SQL specification</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3. [<span data-ttu-id="efabd-946">Azure Cosmos DB .NET-minták</span><span class="sxs-lookup"><span data-stu-id="efabd-946">Azure Cosmos DB .NET samples</span></span>](https://github.com/Azure/azure-documentdb-net)
4. <span data-ttu-id="efabd-947">[Az Azure Cosmos DB Konzisztenciaszintek][consistency-levels]</span><span class="sxs-lookup"><span data-stu-id="efabd-947">[Azure Cosmos DB Consistency Levels][consistency-levels]</span></span>
5. <span data-ttu-id="efabd-948">ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)</span><span class="sxs-lookup"><span data-stu-id="efabd-948">ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)</span></span>
6. <span data-ttu-id="efabd-949">JSON [http://json.org/](http://json.org/)</span><span class="sxs-lookup"><span data-stu-id="efabd-949">JSON [http://json.org/](http://json.org/)</span></span>
7. <span data-ttu-id="efabd-950">JavaScript-specifikáció [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm)</span><span class="sxs-lookup"><span data-stu-id="efabd-950">Javascript Specification [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm)</span></span> 
8. <span data-ttu-id="efabd-951">LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx)</span><span class="sxs-lookup"><span data-stu-id="efabd-951">LINQ [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx)</span></span> 
9. <span data-ttu-id="efabd-952">Lekérdezés kiértékelése technikák nagy adatbázisokhoz [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)</span><span class="sxs-lookup"><span data-stu-id="efabd-952">Query evaluation techniques for large databases [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)</span></span>
10. <span data-ttu-id="efabd-953">A lekérdezés feldolgozás alatt álló párhuzamos relációs adatbázis-rendszerek, IEEE számítógép társadalom nyomja le az 1994.</span><span class="sxs-lookup"><span data-stu-id="efabd-953">Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994</span></span>
11. <span data-ttu-id="efabd-954">Lu, Ooi, Tan, feldolgozás alatt álló párhuzamos relációs adatbázis-rendszerek, IEEE számítógép társadalom nyomja le az 1994 lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="efabd-954">Lu, Ooi, Tan, Query Processing in Parallel Relational Database Systems, IEEE Computer Society Press, 1994.</span></span>
12. <span data-ttu-id="efabd-955">Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: a Pig Latin: egy nem, külső nyelvi SIGMOD 2008 az adatok feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="efabd-955">Christopher Olston, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Pig Latin: A Not-So-Foreign Language for Data Processing, SIGMOD 2008.</span></span>
13. <span data-ttu-id="efabd-956">G.</span><span class="sxs-lookup"><span data-stu-id="efabd-956">G.</span></span> <span data-ttu-id="efabd-957">Graefe.</span><span class="sxs-lookup"><span data-stu-id="efabd-957">Graefe.</span></span> <span data-ttu-id="efabd-958">optimalizálás hello kaszkádokban kerete.</span><span class="sxs-lookup"><span data-stu-id="efabd-958">hello Cascades framework for query optimization.</span></span> <span data-ttu-id="efabd-959">IEEE adatok Eng.</span><span class="sxs-lookup"><span data-stu-id="efabd-959">IEEE Data Eng.</span></span> <span data-ttu-id="efabd-960">BULL., 18(3): 1995.</span><span class="sxs-lookup"><span data-stu-id="efabd-960">Bull., 18(3): 1995.</span></span>

[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md