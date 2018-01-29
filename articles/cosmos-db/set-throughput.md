---
title: "Az Azure Cosmos DB rendelkezés átviteli |} Microsoft Docs"
description: "Tudnivalók az Azure Cosmos DB containsers, gyűjtemények, diagramokat és táblák kiosztott átviteli sebesség."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: mimig
ms.openlocfilehash: 8797910651c54baa3529b015d4195cf2a5c06ece
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/24/2018
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a>Az Azure Cosmos DB tárolókat átviteli beállítása

Átviteli sebesség az az Azure Cosmos DB tárolókat az Azure portálon vagy az ügyfél SDK-k segítségével állíthatja be. 

A következő táblázat felsorolja a rendelkezésre álló tárolók az átviteli sebesség:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Az egypartíciós tároló</strong></p></td>
            <td valign="top"><p><strong>Particionált tároló</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimális átviteli sebesség</p></td>
            <td valign="top"><p>400 kérelem egység / másodperc</p></td>
            <td valign="top"><p>1000 kérelem egység / másodperc</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximális átviteli sebesség</p></td>
            <td valign="top"><p>10 000 kérelemegység / másodperc</p></td>
            <td valign="top"><p>Korlátlan</p></td>
        </tr>
    </tbody>
</table>

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a>Az átviteli sebesség beállítása az Azure-portál használatával

1. Egy új ablakban nyissa meg a [Azure-portálon](https://portal.azure.com).
2. A bal oldali sávon kattintson **Azure Cosmos DB**, vagy kattintson a **több szolgáltatások** alsó, majd görgessen a **adatbázisok**, és kattintson a **Azure Cosmos DB**.
3. Válassza ki a Cosmos DB fiókját.
4. Kattintson az új ablakban **adatkezelő** a navigációs menü.
5. Az új ablakban bontsa ki az adatbázis és a tároló majd **skála & beállítások**.
6. Az új ablakban írja be az új átviteli értéket a **átviteli** gombra, majd **mentése**.

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-net"></a>Az átviteli sebesség beállítása a .NET-hez az SQL API használatával

```csharp
// Fetch the offer of the collection whose throughput needs to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

// Now persist these changes to the collection by replacing the original offer resource
await client.ReplaceOfferAsync(offer);
```

<a id="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>Az átviteli sebesség beállítása Java az SQL API használatával

Ezt a kódrészletet az OfferCrudSamples.java fájlban származik a [azure-documentdb-java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) tárházban. 

```Java
// find offer associated with this collection
Iterator < Offer > it = client.queryOffers(
    String.format("SELECT * FROM r where r.offerResourceId = '%s'", collectionResourceId), null).getQueryIterator();
assertThat(it.hasNext(), equalTo(true));

Offer offer = it.next();
assertThat(offer.getString("offerResourceId"), equalTo(collectionResourceId));
assertThat(offer.getContent().getInt("offerThroughput"), equalTo(throughput));

// update the offer
int newThroughput = 10300;
offer.getContent().put("offerThroughput", newThroughput);
client.replaceOffer(offer);
```

## <a name="throughput-faq"></a>Átviteli – gyakori kérdések

**I állíthatja az átviteli sebesség kisebb, mint 400 RU/mp?**

400 RU/mp a minimális átviteli sebesség érhető el a Cosmos DB egypartíciós tárolók (1000 RU/mp az particionált tárolókat minimális). Kérelem egységek 100 RU/mp intervallumok belül vannak beállítva, de az átviteli sebesség nem állítható be 100 RU/mp vagy bármely érték kisebb, mint a 400 RU/mp. Ha fejlesztéséhez és teszteléséhez Cosmos DB költséghatékony módszert keres, akkor használhatja az ingyenes [Azure Cosmos DB emulátor](local-emulator.md), amely központilag telepíthető helyileg ingyenesen. 

**Hogyan állíthatom be a MongoDB API-jával througput?**

Nincs átviteli sebesség beállításához MongoDB API kiterjesztés nélkül. A javaslat, hogy az SQL API-t használó, ahogy az [az átviteli sebesség beállítása a .NET-hez az SQL API használatával](#set-throughput-sdk).

## <a name="next-steps"></a>További lépések

Üzembe helyezési és folyamatos bolygónk-méretezéssel Cosmos DB, kapcsolatos további információkért lásd: [particionálás és méretezést Cosmos DB,](partition-data.md).
