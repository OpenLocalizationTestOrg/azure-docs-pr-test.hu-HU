---
title: "aaaHow tooquery tábla adatokat az Adatbázisba az Azure Cosmos? | Microsoft Docs"
description: "Ismerje meg az Azure Cosmos Adatbázisba tooquery adatait"
services: cosmos-db
documentationcenter: 
author: kanshiG
manager: jhubbard
editor: 
tags: 
ms.assetid: 14bcb94e-583c-46f7-9ea8-db010eb2ab43
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: govindk
ms.openlocfilehash: 32526c3488c589c5be3a4a2f174aa769570f0c0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-table-data-by-using-hello-table-api-preview"></a>A Cosmos DB Azure: Hogyan tooquery tábla adatai használatával hello tábla API (előzetes verzió)?

hello Azure Cosmos DB [tábla API](table-introduction.md) (előzetes verzió) támogatja az OData és [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) Lekérdezések kulcs/érték (tábla) adatok alapján.  

Ez a cikk ismerteti a következő feladatok hello: 

> [!div class="checklist"]
> * Tábla API hello az adatok lekérdezése

hello ebben a cikkben lekérdezések használja a következő minta hello `People` tábla:

| PartitionKey | RowKey | E-mail cím | Telefonszám |
| --- | --- | --- | --- |
| Grönlandi | Walter | Walter@contoso.com| 425-555-0101 |
| Smith | Ben | Ben@contoso.com| 425-555-0102 |
| Smith | Jeff | Jeff@contoso.com| 425-555-0104 | 

Mivel Azure Cosmos DB hello Azure Table storage API-kkal kompatibilis, lásd: [lekérdezése táblákat és entitásokat] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) talál részletes információt hogyan használatával tooquery hello Tábla API. 

Hello prémium képességeket kínál, amelyek Azure Cosmos DB nyújt további információkért lásd: [Azure Cosmos DB: tábla API](table-introduction.md) és [Develop a .NET-tábla API hello](tutorial-develop-table-dotnet.md). 

## <a name="prerequisites"></a>Előfeltételek

Az alábbi lekérdezéseket toowork rendelkezik Azure Cosmos DB fiókkal és entitás adatokat hello tárolóban. Nem rendelkezik egyetlen, az? Teljes hello [ötperces gyors üzembe helyezés](https://aka.ms/acdbtnetqs) vagy hello [fejlesztői útmutató](https://aka.ms/acdbtabletut) toocreate egy fiókot, és töltse fel az adatbázist.

## <a name="query-on-partitionkey-and-rowkey"></a>Lekérdezés PartitionKey és RowKey
Hello PartitionKey és RowKey tulajdonság egy entitás elsődleges kulcsának alkotnak, mert a következő szintaxist tooidentify hello entitás hello is használhatja: 

**Lekérdezés**

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
**Eredmények**

| PartitionKey | RowKey | E-mail cím | Telefonszám |
| --- | --- | --- | --- |
| Grönlandi | Walter | Walter@contoso.com| 425-555-0104 |

Másik megoldásként megadhatja ezeket a tulajdonságokat hello részeként `$filter` lehetőség használata, mert a következő szakasz hello látható. Vegye figyelembe, hogy hello kulcstulajdonság és állandó értékek kis-és nagybetűket. Hello PartitionKey és a RowKey tulajdonságainak vannak karakterlánc típusú. 

## <a name="query-by-using-an-odata-filter"></a>Egy OData-szűrő segítségével lekérdezése
Egy szűrési karakterláncot térített közben tartsa szem előtt ezeket a szabályokat: 

* Hello hello OData protokoll specifikációja toocompare tooa tulajdonság értéke által meghatározott logikai operátorok használatára. Vegye figyelembe, hogy tooa dinamikus tulajdonság értéke nem hasonlíthatók össze. Hello kifejezés oldalán állandónak kell lennie. 
* hello tulajdonság nevét, a operátor és a konstans URL-kódolású szóközt kell elválasztani. Egy hely, mint URL-kódolású `%20`. 
* Minden hello szűrési karakterláncot részei kis-és nagybetűket. 
* hello hello állandó értéknek kell lennie ahhoz, hogy hello szűrő tooreturn érvényes eredmények hello tulajdonság írja ugyanazokat az adatokat. További információ a támogatott tulajdonságtípus: [ismertetése hello tábla szolgáltatás adatmodell](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). 

Íme egy példa bemutatja, hogyan által toofilter hello PartitionKey lekérdezést és E-mail-tulajdonságok használatával egy OData `$filter`.

**Lekérdezés**

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

Hogyan tooconstruct szűrőkifejezés különböző adattípusok a további információkért lásd: [lekérdezése táblákat és entitásokat](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).

**Eredmények**

| PartitionKey | RowKey | E-mail cím | Telefonszám |
| --- | --- | --- | --- |
| Ben |Smith | Ben@contoso.com| 425-555-0102 |

## <a name="query-by-using-linq"></a>A LINQ lekérdezés 
LINQ, amely toohello megfelelő OData-lekérdezési kifejezések használatával is lekérheti. Íme egy példa hogyan toobuild lekérdezések használatával hello .NET SDK-val:

```csharp
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("people");

TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
    .Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition(PartitionKey, QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition(Email, QueryComparisons.Equal,"Ben@contoso.com")
    ));

await table.ExecuteQuerySegmentedAsync<CustomerEntity>(query, null);
```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello következő régebben már kötöttek:

> [!div class="checklist"]
> * Megtudta, hogyan használatával tooquery hello tábla API (előzetes verzió) 

Most már folytathatja toohello következő útmutató toolearn hogyan toodistribute az adatok globálisan.

> [!div class="nextstepaction"]
> [Az adatok globálisan terjesztése](tutorial-global-distribution-documentdb.md)
