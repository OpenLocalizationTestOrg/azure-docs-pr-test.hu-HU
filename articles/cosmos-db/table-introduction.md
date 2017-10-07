---
title: "aaaIntroduction tooAzure Cosmos DB tábla API |} Microsoft Docs"
description: "Ismerje meg, hogyan használhatja az Azure Cosmos DB toostore, és a lekérdezési kulcs-érték kis késleltetésű használatával nagy mennyiségű hello népszerű OSS MongoDB API-k."
services: cosmos-db
author: bhanupr
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/25/2017
ms.author: arramac
ms.openlocfilehash: 4c5678898a772808f4bcd1465a23d436b0f8fc0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-cosmos-db-table-api"></a>Bevezetés tooAzure Cosmos DB: tábla API

Az [Azure Cosmos DB](introduction.md) a Microsoft globálisan elosztott, többmodelles adatbázis-szolgáltatása az alapvető fontosságú alkalmazásokhoz. Az Azure Cosmos DB biztosít [kulcsrakész globális terjesztési](distribute-data-globally.md), [átviteli sebesség és tárterület a rugalmas méretezést](partition-data.md) világszerte, egyjegyű ezredmásodperces késések: hello 99th PERCENTILIS, [öt jól meghatározott konzisztenciaszintek](consistency-levels.md), és magas rendelkezésre állás érdekében minden biztonsági mentés által garantált [iparágvezető SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Az Azure Cosmos DB [automatikusan elvégzi az adatok](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) anélkül, hogy Ön séma- és index felügyeleti toodeal. Többmodelles szolgáltatás, amely támogatja a dokumentumokat, a kulcs-értékeket, a diagramokat és az oszlopos adatmodelleket. 

![Az Azure Table Storage API és az Azure Cosmos DB](./media/table-introduction/premium-tables.png) 

Azure Cosmos DB hello tábla API (előzetes verzió) egy kulcs-érték tároló rugalmas séma, kiszámítható teljesítményt, globális terjesztési és nagy teljesítményt igénylő alkalmazásokhoz biztosít. Tábla API hello biztosít hello azonos funkciókat Azure Table storage, de kihasználja hello Azure Cosmos adatbázis-kezelő hello előnyeit. 

Azure Table storage magas tárolási és alacsonyabb átviteli követelmények táblák toouse tovább. Azure Cosmos DB bevezet egy jövőbeli frissítéssel tárolási optimalizált táblák támogatása, és a meglévő és új Azure Table storage-fiókok lesz frissítve tooAzure Cosmos DB.

## <a name="premium-and-standard-table-apis"></a>Prémium és standard szintű tábla API-k
Ha Azure Table storage jelenleg használ, ettől kezdve hello előnyöket tooAzure Cosmos DB "prémium table" előzetes át a következő:

|  | Azure Table Storage | Azure Cosmos DB: Table Storage (előzetes verzió) |
| --- | --- | --- |
| Késés | Gyors, de nincs felső korlátja a késésnek | Az olvasási és írási, egyjegyű ezredmásodperces késési biztonsági < 10 ms késleltetés beolvassa és < 15 ms késleltetés ír hello 99th PERCENTILIS bármilyen léptékben hello world a bárhol, |
| Teljesítmény | Hatékonyan skálázható, de nincs dedikált teljesítménymodell. A táblák skálázhatósági korlátja másodpercenként 20 000 művelet | Hatékonyan skálázható a [táblánként dedikált és fenntartott átviteli sebességgel](request-units.md), amelynek rendelkezésre állását SLA-k szavatolják. A fiókokban nincs korlátozva az átviteli sebesség felső határa, és a szolgáltatás táblánként és másodpercenként legalább 10 millió műveletet támogat |
| Globális terjesztés | Egyetlen régió – egyetlen opcionális, olvasható, másodlagos olvasási régióval a magas rendelkezésre állás céljából. Nem kezdeményezhető feladatátvétel | [Kulcsrakész globális terjesztési](distribute-data-globally.md) el egy too30 + régiókból, támogatása [automatikus és manuális feladatátvétel](regional-failover.md) bármikor, bárhonnan hello world |
| Indexelés | Csak elsődleges indexelés a PartitionKey és a RowKey tulajdonságok esetén. Nincsenek másodlagos indexek | Az összes tulajdonság automatikus és teljes indexelése, nincs indexkezelés |
| Lekérdezés | A lekérdezés végrehajtásakor az elsődleges kulcshoz tartozó indexet használja, és egyéb esetben csak vizsgálati műveletet végez. | A lekérdezések a gyorsaság céljából kihasználhatják a tulajdonságok automatikus indexelését. Az Azure Cosmos DB adatbázismotorja összesítési, térinformatikai és rendezési lehetőségeket egyaránt támogat. |
| Konzisztencia | Erős az elsődleges régióban, végleges a másodlagos régióban | [Öt jól meghatározott konzisztenciaszintek](consistency-levels.md) tootrade ki rendelkezésre állási, a késés, az átviteli sebesség és a konzisztencia az alkalmazások igényeihez alapján |
| Díjszabás | Tárolásra optimalizált  | Átviteli sebességre optimalizált |
| SLA-k | 99,9%-os rendelkezésre állás | rendelkezésre állás 99,99 % belül egy régiót, és képes tooadd további régiókban a magas rendelkezésre állás érdekében. [Iparágvezető és átfogó SLA-k](https://azure.microsoft.com/support/legal/sla/cosmos-db/) az általános rendelkezésre állásra vonatkozóan |

## <a name="how-tooget-started"></a>Hogyan tooget el

Hozzon létre egy Azure Cosmos DB fiókot hello [Azure-portálon](https://portal.azure.com), és ismerkedjen meg a [gyors üzembe helyezés tábla API használatával a .NET](create-table-dotnet.md). 

## <a name="next-steps"></a>Következő lépések

Az alábbiakban néhány mutatók tooget indítása:
* [Hello tábla API használatával a .NET-alkalmazás létrehozása](create-table-dotnet.md)
* [A .NET-tábla API hello fejlesztése](tutorial-develop-table-dotnet.md)
* [Tábla adatait hello tábla API használatával](tutorial-query-table.md)
* [Hogyan toosetup Azure Cosmos DB globális terjesztési használatával hello tábla API](tutorial-global-distribution-table.md)
* [Azure Cosmos DB Table API SDK .NET-keretrendszerhez](table-sdk-dotnet.md)
