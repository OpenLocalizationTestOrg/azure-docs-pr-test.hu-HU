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
# <a name="working-with-dates-in-azure-cosmos-db"></a>Az Azure Cosmos DB dátumok használata
Azure Cosmos DB biztosítja a sémák rugalmasságát és a gazdag indexelési keresztül natív [JSON](http://www.json.org) adatmodell. Minden Azure Cosmos DB-erőforrások, például az adatbázisok, gyűjtemények, dokumentumok és tárolt eljárások modellezése és tárolása JSON-dokumentumok is. Hordozható lesznek követelményként JSON (és az Azure Cosmos DB) támogatja a típus csak egy kis készletét: karakterlánc, szám, logikai érték, a tömb, objektum és Null. Azonban JSON rugalmas, és a fejlesztők és keretrendszerek toorepresent összetett típusok engedélyezése a primitívek használatával, és objektumokat vagy tömbök összeállítása őket. 

Számos alkalmazás hozzáadása alapszintű típusok toohello, hello kell [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) be, valamint toorepresent időbélyegzőket. Ez a cikk ismerteti, hogyan fejlesztők is tárolhatja, beolvasása és az Azure Cosmos Adatbázisba hello .NET SDK használatával dátumok lekérdezése.

## <a name="storing-datetimes"></a>Időpontok tárolása
Alapértelmezés szerint hello [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) rendezi sorba dátum/idő értékek [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) karakterláncok. A legtöbb alkalmazás a következő okok miatt hello használhatják DateTime hello alapértelmezett karakterlánc-ábrázolása:

* Karakterláncok hasonlíthatók össze, és hello relatív hello dátum/idő értékek sorrendjét, ha az átalakított toostrings megőrződik. 
* Ez a megközelítés nem igényel egyéni kód vagy attribútumok JSON átalakításhoz.
* a JSON-ban tárolt hello dátumok emberi olvasható.
* Ez a megközelítés előnyeit élvezheti Azure Cosmos DB indexe gyors teljesítmény-küszöbérték.

Például a következő kódrészletet tárolók hello egy `Order` tartalmazó a két dátum/idő tulajdonság - objektum `ShipDate` és `OrderDate` dokumentumként a hello .NET SDK használatával:

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

Ez a dokumentum következő Azure Cosmos DB tárolja:

    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
    

Azt is megteheti tárolhatja időpontok Unix időbélyegeket, mint ez azt jelenti, hogy egy számot jelölő hello 1970. január 1. eltelt másodpercek száma. Belső időbélyeg Azure Cosmos-adatbázis (`_ts`) tulajdonság követi ezt a módszert használja. Használhatja a hello [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) osztály tooserialize időpontok számait. 

## <a name="indexing-datetimes-for-range-queries"></a>A lekérdezések időpontok indexelő
A dátum/idő értékek megegyeznek a lekérdezések. Például ha toofind tegnap óta létrehozott összes rendelések kell, vagy található összes rendelés rendszerrel szállított hello az elmúlt 5 percben, meg kell tooperform lekérdezések. Ezek kérdezi le hatékonyan tooexecute, konfigurálnia kell a gyűjteményt a karakterláncokhoz indexelő tartományon.

    DocumentCollection collection = new DocumentCollection { Id = "orders" };
    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    await client.CreateDocumentCollectionAsync("/dbs/orderdb", collection);

Is meg további információt a házirendek indexelő tooconfigure [Azure Cosmos DB indexelő házirendek](indexing-policies.md).

## <a name="querying-datetimes-in-linq"></a>A LINQ időpontok lekérdezése
hello DocumentDB .NET SDK-t automatikusan támogatja az Azure Cosmos DB keresztül LINQ tárolt adatok lekérdezésére. Például hello alábbi kódrészletben láthatja a LINQ lekérdezés, hogy a hello teljesített elmúlt három napban szűrők megrendelések.

    IQueryable<Order> orders = client.CreateDocumentQuery<Order>("/dbs/orderdb/colls/orders")
        .Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
          
    // Translated toohello following SQL statement and executed on Azure Cosmos DB
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")

További tudnivalók Azure Cosmos DB SQL lekérdezési nyelv és hello LINQ-szolgáltató, [lekérdezése Cosmos DB](documentdb-sql-query.md).

Ebben a cikkben azt venni, hogyan toostore, index, és az Azure Cosmos Adatbázisba időpontok lekérdezése.

## <a name="next-steps"></a>Következő lépések
* Töltse le és futtassa a hello [minták a Githubon Code](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)
* További információ [DocumentDB API-lekérdezés](documentdb-sql-query.md)
* További információ [Azure Cosmos DB indexelő házirendek](indexing-policies.md)
