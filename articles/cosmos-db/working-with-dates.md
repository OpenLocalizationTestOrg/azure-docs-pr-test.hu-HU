---
title: "az Azure Cosmos Adatbázisba dátumokkal aaaWorking |} Microsoft Docs"
description: "További információk a hogyan toowork a dátumok, az Azure Cosmos-Adatbázisba."
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
ms.openlocfilehash: 27ec170e4bef72c0b5b456738f1275ef02543024
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-dates-in-azure-cosmos-db"></a><span data-ttu-id="47889-103">Az Azure Cosmos DB dátumok használata</span><span class="sxs-lookup"><span data-stu-id="47889-103">Working with Dates in Azure Cosmos DB</span></span>
<span data-ttu-id="47889-104">Azure Cosmos DB biztosítja a sémák rugalmasságát és a gazdag indexelési keresztül natív [JSON](http://www.json.org) adatmodell.</span><span class="sxs-lookup"><span data-stu-id="47889-104">Azure Cosmos DB delivers schema flexibility and rich indexing via a native [JSON](http://www.json.org) data model.</span></span> <span data-ttu-id="47889-105">Minden Azure Cosmos DB-erőforrások, például az adatbázisok, gyűjtemények, dokumentumok és tárolt eljárások modellezése és tárolása JSON-dokumentumok is.</span><span class="sxs-lookup"><span data-stu-id="47889-105">All Azure Cosmos DB resources including databases, collections, documents, and stored procedures are modeled and stored as JSON documents.</span></span> <span data-ttu-id="47889-106">Hordozható lesznek követelményként JSON (és az Azure Cosmos DB) támogatja a típus csak egy kis készletét: karakterlánc, szám, logikai érték, a tömb, objektum és Null.</span><span class="sxs-lookup"><span data-stu-id="47889-106">As a requirement for being portable, JSON (and Azure Cosmos DB) supports only a small set of basic types: String, Number, Boolean, Array, Object, and Null.</span></span> <span data-ttu-id="47889-107">Azonban JSON rugalmas, és a fejlesztők és keretrendszerek toorepresent összetett típusok engedélyezése a primitívek használatával, és objektumokat vagy tömbök összeállítása őket.</span><span class="sxs-lookup"><span data-stu-id="47889-107">However, JSON is flexible and allow developers and frameworks toorepresent more complex types using these primitives and composing them as objects or arrays.</span></span> 

<span data-ttu-id="47889-108">Számos alkalmazás hozzáadása alapszintű típusok toohello, hello kell [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) be, valamint toorepresent időbélyegzőket.</span><span class="sxs-lookup"><span data-stu-id="47889-108">In addition toohello basic types, many applications need hello [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) type toorepresent dates and timestamps.</span></span> <span data-ttu-id="47889-109">Ez a cikk ismerteti, hogyan fejlesztők is tárolhatja, beolvasása és az Azure Cosmos Adatbázisba hello .NET SDK használatával dátumok lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="47889-109">This article describes how developers can store, retrieve, and query dates in Azure Cosmos DB using hello .NET SDK.</span></span>

## <a name="storing-datetimes"></a><span data-ttu-id="47889-110">Időpontok tárolása</span><span class="sxs-lookup"><span data-stu-id="47889-110">Storing DateTimes</span></span>
<span data-ttu-id="47889-111">Alapértelmezés szerint hello [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) rendezi sorba dátum/idő értékek [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="47889-111">By default, hello [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) serializes DateTime values as [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) strings.</span></span> <span data-ttu-id="47889-112">A legtöbb alkalmazás a következő okok miatt hello használhatják DateTime hello alapértelmezett karakterlánc-ábrázolása:</span><span class="sxs-lookup"><span data-stu-id="47889-112">Most applications can use hello default string representation for DateTime for hello following reasons:</span></span>

* <span data-ttu-id="47889-113">Karakterláncok hasonlíthatók össze, és hello relatív hello dátum/idő értékek sorrendjét, ha az átalakított toostrings megőrződik.</span><span class="sxs-lookup"><span data-stu-id="47889-113">Strings can be compared, and hello relative ordering of hello DateTime values is preserved when they are transformed toostrings.</span></span> 
* <span data-ttu-id="47889-114">Ez a megközelítés nem igényel egyéni kód vagy attribútumok JSON átalakításhoz.</span><span class="sxs-lookup"><span data-stu-id="47889-114">This approach doesn't require any custom code or attributes for JSON conversion.</span></span>
* <span data-ttu-id="47889-115">a JSON-ban tárolt hello dátumok emberi olvasható.</span><span class="sxs-lookup"><span data-stu-id="47889-115">hello dates as stored in JSON are human readable.</span></span>
* <span data-ttu-id="47889-116">Ez a megközelítés előnyeit élvezheti Azure Cosmos DB indexe gyors teljesítmény-küszöbérték.</span><span class="sxs-lookup"><span data-stu-id="47889-116">This approach can take advantage of Azure Cosmos DB's index for fast query performance.</span></span>

<span data-ttu-id="47889-117">Például a következő kódrészletet tárolók hello egy `Order` tartalmazó a két dátum/idő tulajdonság - objektum `ShipDate` és `OrderDate` dokumentumként a hello .NET SDK használatával:</span><span class="sxs-lookup"><span data-stu-id="47889-117">For example, hello following snippet stores an `Order` object containing two DateTime properties - `ShipDate` and `OrderDate` as a document using hello .NET SDK:</span></span>

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

<span data-ttu-id="47889-118">Ez a dokumentum következő Azure Cosmos DB tárolja:</span><span class="sxs-lookup"><span data-stu-id="47889-118">This document is stored in Azure Cosmos DB as follows:</span></span>

    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
    

<span data-ttu-id="47889-119">Azt is megteheti tárolhatja időpontok Unix időbélyegeket, mint ez azt jelenti, hogy egy számot jelölő hello 1970. január 1. eltelt másodpercek száma.</span><span class="sxs-lookup"><span data-stu-id="47889-119">Alternatively, you can store DateTimes as Unix timestamps, that is, as a number representing hello number of elapsed seconds since January 1, 1970.</span></span> <span data-ttu-id="47889-120">Belső időbélyeg Azure Cosmos-adatbázis (`_ts`) tulajdonság követi ezt a módszert használja.</span><span class="sxs-lookup"><span data-stu-id="47889-120">Azure Cosmos DB's internal Timestamp (`_ts`) property follows this approach.</span></span> <span data-ttu-id="47889-121">Használhatja a hello [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) osztály tooserialize időpontok számait.</span><span class="sxs-lookup"><span data-stu-id="47889-121">You can use hello [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) class tooserialize DateTimes as numbers.</span></span> 

## <a name="indexing-datetimes-for-range-queries"></a><span data-ttu-id="47889-122">A lekérdezések időpontok indexelő</span><span class="sxs-lookup"><span data-stu-id="47889-122">Indexing DateTimes for range queries</span></span>
<span data-ttu-id="47889-123">A dátum/idő értékek megegyeznek a lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="47889-123">Range queries are common with DateTime values.</span></span> <span data-ttu-id="47889-124">Például ha toofind tegnap óta létrehozott összes rendelések kell, vagy található összes rendelés rendszerrel szállított hello az elmúlt 5 percben, meg kell tooperform lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="47889-124">For example, if you need toofind all orders created since yesterday, or find all orders shipped in hello last five minutes, you need tooperform range queries.</span></span> <span data-ttu-id="47889-125">Ezek kérdezi le hatékonyan tooexecute, konfigurálnia kell a gyűjteményt a karakterláncokhoz indexelő tartományon.</span><span class="sxs-lookup"><span data-stu-id="47889-125">tooexecute these queries efficiently, you must configure your collection for Range indexing on strings.</span></span>

    DocumentCollection collection = new DocumentCollection { Id = "orders" };
    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    await client.CreateDocumentCollectionAsync("/dbs/orderdb", collection);

<span data-ttu-id="47889-126">Is meg további információt a házirendek indexelő tooconfigure [Azure Cosmos DB indexelő házirendek](indexing-policies.md).</span><span class="sxs-lookup"><span data-stu-id="47889-126">You can learn more about how tooconfigure indexing policies at [Azure Cosmos DB Indexing Policies](indexing-policies.md).</span></span>

## <a name="querying-datetimes-in-linq"></a><span data-ttu-id="47889-127">A LINQ időpontok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="47889-127">Querying DateTimes in LINQ</span></span>
<span data-ttu-id="47889-128">hello DocumentDB .NET SDK-t automatikusan támogatja az Azure Cosmos DB keresztül LINQ tárolt adatok lekérdezésére.</span><span class="sxs-lookup"><span data-stu-id="47889-128">hello DocumentDB .NET SDK automatically supports querying data stored in Azure Cosmos DB via LINQ.</span></span> <span data-ttu-id="47889-129">Például hello alábbi kódrészletben láthatja a LINQ lekérdezés, hogy a hello teljesített elmúlt három napban szűrők megrendelések.</span><span class="sxs-lookup"><span data-stu-id="47889-129">For example, hello following snippet shows a LINQ query that filters orders that were shipped in hello last three days.</span></span>

    IQueryable<Order> orders = client.CreateDocumentQuery<Order>("/dbs/orderdb/colls/orders")
        .Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
          
    // Translated toohello following SQL statement and executed on Azure Cosmos DB
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")

<span data-ttu-id="47889-130">További tudnivalók Azure Cosmos DB SQL lekérdezési nyelv és hello LINQ-szolgáltató, [lekérdezése Cosmos DB](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="47889-130">You can learn more about Azure Cosmos DB's SQL query language and hello LINQ provider at [Querying Cosmos DB](documentdb-sql-query.md).</span></span>

<span data-ttu-id="47889-131">Ebben a cikkben azt venni, hogyan toostore, index, és az Azure Cosmos Adatbázisba időpontok lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="47889-131">In this article, we looked at how toostore, index, and query DateTimes in Azure Cosmos DB.</span></span>

## <a name="next-steps"></a><span data-ttu-id="47889-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="47889-132">Next Steps</span></span>
* <span data-ttu-id="47889-133">Töltse le és futtassa a hello [minták a Githubon Code](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)</span><span class="sxs-lookup"><span data-stu-id="47889-133">Download and run hello [Code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)</span></span>
* <span data-ttu-id="47889-134">További információ [DocumentDB API-lekérdezés](documentdb-sql-query.md)</span><span class="sxs-lookup"><span data-stu-id="47889-134">Learn more about [DocumentDB API Query](documentdb-sql-query.md)</span></span>
* <span data-ttu-id="47889-135">További információ [Azure Cosmos DB indexelő házirendek](indexing-policies.md)</span><span class="sxs-lookup"><span data-stu-id="47889-135">Learn more about [Azure Cosmos DB Indexing Policies](indexing-policies.md)</span></span>
