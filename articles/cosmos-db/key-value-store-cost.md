---
title: "aaaAzure Cosmos DB egy kulcs-érték tárolóként – költség áttekintése |} Microsoft Docs"
description: "További információk a hello alacsony költségű Azure Cosmos DB használatával egy kulcs-érték tárolóként."
keywords: "kulcs-érték tároló"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: 
documentationcenter: 
ms.assetid: 7f765c17-8549-4509-9475-46394fc3a218
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: mimig
ms.openlocfilehash: de7207760a8e1fca0e30f951109748835dabf4a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-as-a-key-value-store--cost-overview"></a>Azure Cosmos-adatbázis egy kulcs-érték tárolóként – költség áttekintése

Azure Cosmos-adatbázis egy olyan globálisan elosztott, több modellre adatbázis szolgáltatás magas rendelkezésre állású, nagy méretű alkalmazások könnyen készítéséhez. Alapértelmezés szerint Azure Cosmos DB automatikusan elvégzi az ingests, hatékony minden hello adat. Ez lehetővé teszi, hogy gyors és egységes [SQL](documentdb-sql-query.md) (és [JavaScript](programming.md)) az adatok bármilyen lekérdezések. 

Ez a cikk ismerteti a Azure Cosmos DB hello költségét egyszerű írásra, és olvasási műveleteket, ha egy kulcs-érték tárolóként szolgál. Az írási műveletek közé tartozik a beszúrások, cserél, törléseket és dokumentumok upserts. Amellett, amelyek biztosítják a magas rendelkezésre állás 99,99 %, Azure Cosmos DB ajánlatok garantált < 10 ms késleltetés olvasása és < a hello (indexelt) 15 ms késleltetés, hello 99th PERCENTILIS, írja. 

## <a name="why-we-use-request-units-rus"></a>Miért kérelem egységek (RUs) használjuk.

Azure Cosmos DB teljesítmény hello mennyisége alapján kiosztott [kérelem egységek](request-units.md) (RU) hello partíció. hello kiépítés második részletességű és értékesítik RUs/mp és RUs/perc ([óránkénti számlázási hello összetéveszthető nem toobe](https://azure.microsoft.com/pricing/details/cosmos-db/)). A pénznem, amely leegyszerűsíti a hello kiépítés hello alkalmazáshoz szükséges átviteli RUs kell tekinteni. A felhasználók nem rendelkeznek toothink megkülönböztetése az olvasási és írási kapacitásegység. hello egyetlen pénznem modelljét RUs hoz létre a hatékonyság tooshare hello kiosztott kapacitást olvasási és írási műveletek között. Ez a kiosztott kapacitást modell lehetővé teszi, hogy a hello szolgáltatás tooprovide következetes és kiszámítható átviteli sebességet, alacsony késéssel és magas rendelkezésre állású garantált. Végül RU toomodel átviteli használjuk, de minden kiosztott RU rendelkezik is egy meghatározott mennyiségű erőforrást (memória, Core). RU/mp nincs csak iops-érték.

Globálisan elosztott adatbázis rendszerként Cosmos DB csak Azure-szolgáltatás, amely szolgáltatásiszint-szerződésben garantált késés, az átviteli sebesség és a konzisztencia hozzáadása toohigh rendelkezésre állási van hello. hello átviteli kiépítése a Cosmos-adatbázis adatbázis-fiókjához társított hello régiók alkalmazott tooeach. Olvasási műveleteknél a Cosmos DB kínál több, jól meghatározott [konzisztenciaszintek](consistency-levels.md) toochoose a számára. 

hello következő táblázatban látható hello RUs szükséges tooperform olvasás száma és írási dokumentum méretének 1KB és 100KBs tranzakciók.

|Elem mérete|1 olvasása|1 írási|
|-------------|------|-------|
|1 KB|1 RU|5 RUs|
|100 KB|10 RUs|50 RUs|

## <a name="cost-of-reads-and-writes"></a>Olvasási és írási költsége

Ha 1000 RU/mp, ez összegek too3.6m RU/óra, és a $0.08 lesz költsége hello az óra (hello amerikai és európai). 1KB méretű dokumentum, ez azt jelenti, hogy használatba vehetné 3,6 m olvasási vagy 0,72 m ír (3.6mRU / 5) a létesített átviteli sebesség használatával. Normalizált toomillion beolvassa és, hello költség lenne $0,022 /m olvasási ($0.08 / 3,6) és a $0.111/ m írja a ($0.08 / 0,72). hello költség millió minimális, ahogy az az alábbi táblázat hello válik.

|Elem mérete|1m olvasása|1m írási|
|-------------|-------|--------|
|1 KB|$0.022|$0.111|
|100 KB|$0.222|$1.111|


A legtöbb alapszintű hello blob vagy objektum tárolók szolgáltatások költség $0,40 / millió olvasási tranzakció és $5 millió írási tranzakciónként. Optimálisan használja, ha a Cosmos DB too98 % (1KB tranzakciókhoz) ezek más megoldásokat olcsóbbak lehet.

## <a name="next-steps"></a>Következő lépések

Kövessen bennünket az új cikkek az Azure Cosmos DB az erőforrás-kiépítés optimalizálásához. A szabad toouse hello addig is látja a [RU Számológép](https://www.documentdb.com/capacityplanner).

