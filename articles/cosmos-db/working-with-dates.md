---
title: "Az Azure Cosmos Adatbázisba dátumok használata |} Microsoft Docs"
description: "További tudnivalók az Azure Cosmos Adatbázisba dátumok használata."
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: e587772f-ce9f-498c-a017-a51e7265bb23
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: arramac
ms.openlocfilehash: b6a77e33eea24000037ffb31d7aae3cb1d345ce9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="working-with-dates-in-azure-cosmos-db"></a><span data-ttu-id="3024b-103">Az Azure Cosmos DB dátumok használata</span><span class="sxs-lookup"><span data-stu-id="3024b-103">Working with Dates in Azure Cosmos DB</span></span>
<span data-ttu-id="3024b-104">Azure Cosmos DB biztosítja a sémák rugalmasságát és a gazdag indexelési keresztül natív [JSON](http://www.json.org) adatmodell.</span><span class="sxs-lookup"><span data-stu-id="3024b-104">Azure Cosmos DB delivers schema flexibility and rich indexing via a native [JSON](http://www.json.org) data model.</span></span> <span data-ttu-id="3024b-105">Minden Azure Cosmos DB-erőforrások, például az adatbázisok, gyűjtemények, dokumentumok és tárolt eljárások modellezése és tárolása JSON-dokumentumok is.</span><span class="sxs-lookup"><span data-stu-id="3024b-105">All Azure Cosmos DB resources including databases, collections, documents, and stored procedures are modeled and stored as JSON documents.</span></span> <span data-ttu-id="3024b-106">Hordozható lesznek követelményként JSON (és az Azure Cosmos DB) támogatja a típus csak egy kis készletét: karakterlánc, szám, logikai érték, a tömb, objektum és Null.</span><span class="sxs-lookup"><span data-stu-id="3024b-106">As a requirement for being portable, JSON (and Azure Cosmos DB) supports only a small set of basic types: String, Number, Boolean, Array, Object, and Null.</span></span> <span data-ttu-id="3024b-107">Azonban JSON rugalmas, és engedélyezze a fejlesztők és keretrendszerek képviselő összetett típusok ezek primitívek használatával, és objektumokat vagy tömbök összeállítása őket.</span><span class="sxs-lookup"><span data-stu-id="3024b-107">However, JSON is flexible and allow developers and frameworks to represent more complex types using these primitives and composing them as objects or arrays.</span></span> 

<span data-ttu-id="3024b-108">Az egyszerű típusok, mellett számos alkalmazás kell a [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) dátumok és időbélyegeket képviselő típus.</span><span class="sxs-lookup"><span data-stu-id="3024b-108">In addition to the basic types, many applications need the [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) type to represent dates and timestamps.</span></span> <span data-ttu-id="3024b-109">Ez a cikk ismerteti, hogyan fejlesztők is tárolhatja, beolvasása és a .NET SDK használatával Azure Cosmos DB dátumok lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="3024b-109">This article describes how developers can store, retrieve, and query dates in Azure Cosmos DB using the .NET SDK.</span></span>

## <a name="storing-datetimes"></a><span data-ttu-id="3024b-110">Időpontok tárolása</span><span class="sxs-lookup"><span data-stu-id="3024b-110">Storing DateTimes</span></span>
<span data-ttu-id="3024b-111">Alapértelmezés szerint a [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) rendezi sorba dátum/idő értékek [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="3024b-111">By default, the [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) serializes DateTime values as [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) strings.</span></span> <span data-ttu-id="3024b-112">A legtöbb alkalmazás a következő okok miatt használhatják a DateTime típusú érték az alapértelmezett karakterlánc-ábrázolása:</span><span class="sxs-lookup"><span data-stu-id="3024b-112">Most applications can use the default string representation for DateTime for the following reasons:</span></span>

* <span data-ttu-id="3024b-113">Karakterláncok hasonlíthatók össze, és a dátum/idő értékek relatív sorrendjének esetén is megőrződik karakterláncok való átalakításából származnak.</span><span class="sxs-lookup"><span data-stu-id="3024b-113">Strings can be compared, and the relative ordering of the DateTime values is preserved when they are transformed to strings.</span></span> 
* <span data-ttu-id="3024b-114">Ez a megközelítés nem igényel egyéni kód vagy attribútumok JSON átalakításhoz.</span><span class="sxs-lookup"><span data-stu-id="3024b-114">This approach doesn't require any custom code or attributes for JSON conversion.</span></span>
* <span data-ttu-id="3024b-115">A dátum a JSON-ban tárolt emberi olvasható.</span><span class="sxs-lookup"><span data-stu-id="3024b-115">The dates as stored in JSON are human readable.</span></span>
* <span data-ttu-id="3024b-116">Ez a megközelítés előnyeit élvezheti Azure Cosmos DB indexe gyors teljesítmény-küszöbérték.</span><span class="sxs-lookup"><span data-stu-id="3024b-116">This approach can take advantage of Azure Cosmos DB's index for fast query performance.</span></span>

<span data-ttu-id="3024b-117">Például az alábbi kódrészletben tárolja egy `Order` tartalmazó a két dátum/idő tulajdonság - objektum `ShipDate` és `OrderDate` -dokumentumként a .NET SDK használatával:</span><span class="sxs-lookup"><span data-stu-id="3024b-117">For example, the following snippet stores an `Order` object containing two DateTime properties - `ShipDate` and `OrderDate` as a document using the .NET SDK:</span></span>

    public class Order
    {
        [JsonProperty(PropertyName="id")]
        public string Id { get; set; }
        public DateTime OrderDate { get; set; }
        public DateTime ShipDate { get; set; }
        public double Total { get; set; }
    }

    await client.CreateDocumentAsync("/dbs/orderdb/colls/orders", 
        new Order 
        { 
            Id = "09152014101",
            OrderDate = DateTime.UtcNow.AddDays(-30),
            ShipDate = DateTime.UtcNow.AddDays(-14), 
            Total = 113.39
        });

<span data-ttu-id="3024b-118">Ez a dokumentum következő Azure Cosmos DB tárolja:</span><span class="sxs-lookup"><span data-stu-id="3024b-118">This document is stored in Azure Cosmos DB as follows:</span></span>

    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
    

<span data-ttu-id="3024b-119">Alternatív megoldásként tárolhatja időpontok Unix időbélyegeket, mint ez azt jelenti, hogy egy számot jelölő 1970. január 1. óta eltelt másodpercek számát.</span><span class="sxs-lookup"><span data-stu-id="3024b-119">Alternatively, you can store DateTimes as Unix timestamps, that is, as a number representing the number of elapsed seconds since January 1, 1970.</span></span> <span data-ttu-id="3024b-120">Belső időbélyeg Azure Cosmos-adatbázis (`_ts`) tulajdonság követi ezt a módszert használja.</span><span class="sxs-lookup"><span data-stu-id="3024b-120">Azure Cosmos DB's internal Timestamp (`_ts`) property follows this approach.</span></span> <span data-ttu-id="3024b-121">Használhatja a [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) osztály számként időpontok szerializálni.</span><span class="sxs-lookup"><span data-stu-id="3024b-121">You can use the [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) class to serialize DateTimes as numbers.</span></span> 

## <a name="indexing-datetimes-for-range-queries"></a><span data-ttu-id="3024b-122">A lekérdezések időpontok indexelő</span><span class="sxs-lookup"><span data-stu-id="3024b-122">Indexing DateTimes for range queries</span></span>
<span data-ttu-id="3024b-123">A dátum/idő értékek megegyeznek a lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="3024b-123">Range queries are common with DateTime values.</span></span> <span data-ttu-id="3024b-124">Például a tegnap óta létrehozott összes rendelések található, vagy megkeresheti az elmúlt öt percben szállított összes rendeléseket kell, ha szüksége lekérdezések végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="3024b-124">For example, if you need to find all orders created since yesterday, or find all orders shipped in the last five minutes, you need to perform range queries.</span></span> <span data-ttu-id="3024b-125">Sikeres végrehajtásához ezeket a lekérdezéseket, konfigurálnia kell a gyűjteményt a karakterláncokhoz indexelő tartományon.</span><span class="sxs-lookup"><span data-stu-id="3024b-125">To execute these queries efficiently, you must configure your collection for Range indexing on strings.</span></span>

    DocumentCollection collection = new DocumentCollection { Id = "orders" };
    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    await client.CreateDocumentCollectionAsync("/dbs/orderdb", collection);

<span data-ttu-id="3024b-126">Az indexelő a házirendek konfigurálásával kapcsolatos részletesebb [Azure Cosmos DB indexelő házirendek](indexing-policies.md).</span><span class="sxs-lookup"><span data-stu-id="3024b-126">You can learn more about how to configure indexing policies at [Azure Cosmos DB Indexing Policies](indexing-policies.md).</span></span>

## <a name="querying-datetimes-in-linq"></a><span data-ttu-id="3024b-127">A LINQ időpontok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="3024b-127">Querying DateTimes in LINQ</span></span>
<span data-ttu-id="3024b-128">A DocumentDB .NET SDK automatikusan támogatja az Azure Cosmos DB keresztül LINQ tárolt adatok lekérdezésére.</span><span class="sxs-lookup"><span data-stu-id="3024b-128">The DocumentDB .NET SDK automatically supports querying data stored in Azure Cosmos DB via LINQ.</span></span> <span data-ttu-id="3024b-129">Például az alábbi kódrészletben láthatja a LINQ lekérdezés, hogy az elmúlt három napban teljesített szűrők megrendelések.</span><span class="sxs-lookup"><span data-stu-id="3024b-129">For example, the following snippet shows a LINQ query that filters orders that were shipped in the last three days.</span></span>

    IQueryable<Order> orders = client.CreateDocumentQuery<Order>("/dbs/orderdb/colls/orders")
        .Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
          
    // Translated to the following SQL statement and executed on Azure Cosmos DB
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")

<span data-ttu-id="3024b-130">További Azure Cosmos DB SQL lekérdező nyelve és a LINQ szolgáltatónál, [lekérdezése Cosmos DB](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="3024b-130">You can learn more about Azure Cosmos DB's SQL query language and the LINQ provider at [Querying Cosmos DB](documentdb-sql-query.md).</span></span>

<span data-ttu-id="3024b-131">Ez a cikk azt venni, hogyan tárolhatja, index és az Azure Cosmos Adatbázisba időpontok lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="3024b-131">In this article, we looked at how to store, index, and query DateTimes in Azure Cosmos DB.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3024b-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3024b-132">Next Steps</span></span>
* <span data-ttu-id="3024b-133">Töltse le és futtassa a [minták a Githubon Code](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)</span><span class="sxs-lookup"><span data-stu-id="3024b-133">Download and run the [Code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)</span></span>
* <span data-ttu-id="3024b-134">További információ [DocumentDB API-lekérdezés](documentdb-sql-query.md)</span><span class="sxs-lookup"><span data-stu-id="3024b-134">Learn more about [DocumentDB API Query](documentdb-sql-query.md)</span></span>
* <span data-ttu-id="3024b-135">További információ [Azure Cosmos DB indexelő házirendek](indexing-policies.md)</span><span class="sxs-lookup"><span data-stu-id="3024b-135">Learn more about [Azure Cosmos DB Indexing Policies](indexing-policies.md)</span></span>
