---
title: "az Azure Cosmos DB aaaProvision átviteli |} Microsoft Docs"
description: "Ismerje meg, hogyan tooset kiosztott átviteli sebesség a Azure Cosmos DB containsers, gyűjtemények, diagramokat és táblákat."
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
ms.date: 06/12/2017
ms.author: mimig
ms.openlocfilehash: c143f4aace466b7109168a50e2eb80ddeca6400e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a>Az Azure Cosmos DB tárolókat átviteli beállítása

Átviteli sebesség beállítása az Azure Cosmos DB tárolók hello Azure-portálon, vagy a hello ügyfél SDK-k. 

hello a következő táblázat felsorolja a hello átviteli tárolók érhető el:

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
            <td valign="top"><p>2500 kérést egység / másodperc</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximális átviteli sebesség</p></td>
            <td valign="top"><p>10 000 kérelemegység / másodperc</p></td>
            <td valign="top"><p>Korlátlan</p></td>
        </tr>
    </tbody>
</table>

## <a name="tooset-hello-throughput-by-using-hello-azure-portal"></a>tooset hello átviteli hello Azure-portál használatával

1. Egy új ablakban nyissa meg a hello [Azure-portálon](https://portal.azure.com).
2. Hello bal oldali sávon kattintson **Azure Cosmos DB**, vagy kattintson a **több szolgáltatások** hello lap alján, majd görgessen túl**adatbázisok**, és kattintson a **Azure Cosmos DB**.
3. Válassza ki a Cosmos DB fiókját.
4. Hello új ablakban kattintson **adatok kezelővel (előzetes verzió)** hello navigációs menüjében.
5. Hello új ablakában bontsa ki az adatbázis és a tároló, és kattintson a **skála & beállítások**.
6. Hello új ablakban írja be az új átviteli értéknek hello hello **átviteli** gombra, majd **mentése**.

<a id="set-throughput-sdk"></a>

## <a name="tooset-hello-throughput-by-using-hello-documentdb-api-for-net"></a>tooset hello átviteli hello DocumentDB API használatával a .NET-hez

```C#
//Fetch hello resource toobe updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set hello throughput toohello new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offer);
```

## <a name="throughput-faq"></a>Átviteli – gyakori kérdések

**Beállíthatja a átviteli tooless, mint a 400 RU/mp?**

400 RU/mp hello minimális átviteli sebesség érhető el a Cosmos DB az egypartíciós gyűjtemények (2500 RU/mp a particionált gyűjtemények minimális hello). Kérelem egységek 100 RU/mp intervallumok belül vannak beállítva, de az átviteli sebesség nem állítható be too100 RU/mp vagy bármely érték kisebb, mint a 400 RU/mp. Ha egy költséghatékony metódus toodevelop keres, és Cosmos DB tesztelni, használhatja-e szabad hello [Azure Cosmos DB emulátor](local-emulator.md), amely központilag telepíthető helyileg ingyenesen. 

**Hogyan állíthatom througput hello MongoDB API használatával?**

Nincs nincs MongoDB API bővítmény tooset átviteli sebességet. hello ajánljuk, toouse hello DocumentDB API, ahogy az [tooset hello átviteli hello DocumentDB API használatával a .NET-hez](#set-throughput-sdk).

## <a name="next-steps"></a>Következő lépések

További információ az üzembe helyezési és folyamatos bolygónk-méretezéssel Cosmos DB, toolearn lásd [particionálás és méretezést Cosmos DB,](partition-data.md).
