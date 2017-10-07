---
title: aaaIntroduction tooAzure Cosmos DB |} Microsoft Docs
description: "Az Azure Cosmos DB ismertetése. Ez a globálisan elosztott, többmodelles adatbázis az alacsony késés, a rugalmas skálázhatóság és a magas rendelkezésre állás jegyében készült."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: a855183f-34d4-49cc-9609-1478e465c3b7
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/14/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: f2acbe99f425b2f07a62bbbb4324aa48f1037481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="welcome-tooazure-cosmos-db"></a>Üdvözli a tooAzure Cosmos DB

Az Azure Cosmos DB a Microsoft globálisan elosztott, többmodelles adatbázisa. Hello kattintással gomb Azure Cosmos DB lehetővé teszi tooelastically és egymástól függetlenül méretezési átviteli sebesség és tárterület tetszőleges számú Azure földrajzi régiók között. A rendszer az átviteli sebességre, a késére, a rendelkezésre állásra és a konzisztenciára vonatkozó garanciákat biztosít átfogó [szolgáltatói szerződésekkel](https://aka.ms/acdbsla) (SLA). Ilyet egyetlen másik adatbázis-szolgáltatás sem kínál.

![Az Azure Cosmos DB a Microsoft globálisan elosztott adatbázis-szolgáltatása rugalmas horizontális felskálázási képességgel, garantáltan alacsony késéssel, öt konzisztenciamodellel, valamint átfogó garantált SLA-kkal.](./media/introduction/azure-cosmos-db.png)

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Az Azure Cosmos DB előnyeit kihasználó megoldások

Bármely [webes, mobil, játékok, és az IoT-alkalmazásokhoz](use-cases.md) , amely olvasási és írási műveletek nagy mennyiségű toohandle kell egy [globális](distribute-data-globally.md) vonatkozó különböző adatokat az Azure Cosmos Adatbázisból előnyösek alacsony válaszidők méretezést [garantált](https://azure.microsoft.com/support/legal/sla/cosmos-db/) rendelkezésre állási, nagy átviteli sebességet, alacsony késéssel és hangolható konzisztencia.

## <a name="key-capabilities"></a>Főbb képességek
Globálisan elosztott adatbázis szolgáltatásként Azure Cosmos DB biztosít hello képességek toohelp olyan méretezhető, rövid válaszidejű alkalmazásokat hozhat létre a következő:

* **Kulcsrakész globális terjesztés**
    * Is [terjesztése az adatok](distribute-data-globally.md) tooany száma [Azure-régiók](https://azure.microsoft.com/regions/), a hello [gomb kattintson](tutorial-global-distribution-documentdb.md). Ez lehetővé teszi, hogy Ön tooput az adatokat, ha a felhasználók vannak, győződjön meg arról hello lehető legkisebb késleltetést tooyour ügyfelek. 
    * Azure Cosmos DB használatával többhelyű API-k, hello app mindig tudja, hol hello legközelebbi régiót, és elküldi a kérelmek toohello legközelebbi adatközpont. Mindez lehetséges konfigurációs módosítások nélküli, megadta az írási régiót, és számos olvasás régiók szerint és hello rest kezeli-e meg.

* **Több adatmodell és népszerű API az adatok eléréséhez és lekérdezéséhez**
    * hello atom--rekordsorozat (ARS) alapú adatmodell épülő Azure Cosmos DB natív módon támogatja a több adatmodellekben, ideértve, de nem korlátozott toodocument, a graph, a kulcs-érték, a tábla és a oszlopos adatmodellekben.
    * API-khoz, a következő adatmodellekben hello használata támogatott SDK több nyelven is elérhető:
        * [DocumentDB API](documentdb-introduction.md)
        * [MongoDB API](mongodb-introduction.md)
        * [Tábla API](table-introduction.md)
        * [Graph (Gremlin) API](graph-introduction.md)
        * Hamarosan további adatmodellek is elérhetővé válnak 

* **Igény szerinti rugalmas átviteli sebesség és tárterület, világszerte**
    * Az adatbázis átviteli sebességét könnyedén méretezheti [másodpercalapú](request-units.md) részletességgel, és igény szerint bármikor megváltoztathatja. 
    * Méretezhető tárolás mérete [automatikusan és transzparens módon](partition-data.md) toohandle most és tartja a kötetméretek követelményeit.

* **Gyors válaszidejű és alapvető fontosságú alkalmazásokat hozhat létre**
    * Azure Cosmos DB biztosítja, hogy végpontok közötti kis késés: hello 99th PERCENTILIS tooits ügyfelek. 
    * Egy tipikus 1 KB cikk Cosmos DB biztosítja, hogy az olvasások végpontok közötti késés alatt 10 ms és indexelt írások: hello 99th PERCENTILIS, a 15 ms belül hello azonos Azure-régiót. hello közepes határozottan kevesebb (az 5 ms).

* **Always On rendelkezésre állás**
    * 99,99%-os rendelkezésre állás egy adott régión belül.
    * Tooany száma telepítése [Azure-régiók](https://azure.microsoft.com/regions) a magas rendelkezésre állás érdekében.
    * [Hibaszimuláció](regional-failover.md) egy vagy több régióban, adatvesztés elleni garanciával. 

* **Globálisan elosztott alkalmazások írását, hello jobb módja**
    * Öt [konzisztencia modellek](consistency-levels.md) -modellek biztosítják, számos, az erős SQL-szerű konzisztencia összes hello hasonló módon tooNoSQL végleges konzisztencia, és minden dolog a között. 
  
* **Pénzvisszafizetési garancia**
    * Adatai gyorsan célba érnek, vagy visszafizetjük a pénzét. 
    * [Szolgáltatói szerződések](https://aka.ms/acdbsla) a rendelkezésre állásról, a késésről, átviteli sebességről és konzisztenciáról. 

* **Nincs adatbázisséma vagy indexkezelés**
    * Nem kell többé aggódnia amiatt, hogy adatbázissémája és az indexei szinkronban vannak-e az alkalmazása által használt sémával. Nálunk nincsenek sémák. 
    * Azure Cosmos-adatbázis adatbázis-kezelő rendszer teljesen független séma – automatikusan elvégzi a összes hello adatok ingests anélkül, hogy a séma vagy az indexek, és a rendkívül gyors lekérdezéseket szolgál. 

* **Alacsony tulajdonosi költségek**
    * Ötször tooten [költséghatékonyabb](https://aka.ms/cosmos-db-tco-paper) mint egy nem kezelt megoldás.
    * Harmadannyiba kerül, mint a DynamoDB.

## <a name="capability-comparison"></a>Képességek összehasonlítása

Azure Cosmos DB hello legjobb lehetőségeket relációs és nem relációs adatbázisok kínál.

| Funkciók | Relációs adatbázisok   | Nem relációs (NoSQL-) adatbázisok |    Azure Cosmos DB |
| --- | --- | --- | --- |
| Globális terjesztés | Nem | Nem | Igen, kulcsrakész terjesztés 30-nál is több régióban, többkiszolgálós API-kkal|
| Horizontális skálázhatóság | Nem | Igen | Igen, a tárolás és átviteli sebesség függetlenül skálázható | 
| Késési garancia | Nem | Igen | Igen, az esetek 99%-ában az olvasások 10 ezredmásodperc, az írások 15 ezredmásodperc alatt | 
| Magas rendelkezésre állás | Nem | Igen | Igen, a Cosmos DB mindig elérhető, PACELC kompromisszumokkal rendelkezik, valamint automatikus és kézi feladatátvételi beállításokat biztosít|
| Adatmodell és API | Relációs és SQL | Többmodelles és OSS API | Többmodelles és SQL, valamint OSS API (továbbiak hamarosan elérhetőek) |
| SLA-k | Igen | Nem | Igen, késésre, átviteli sebességre, konzisztenciára, rendelkezésre állásra vonatkozó átfogó SLA-k |


## <a name="next-steps"></a>Következő lépések
Az alábbi rövid útmutatókkal könnyedén elkezdheti az Azure Cosmos DB használatát:

* [Bevezetés az Azure Cosmos DB DocumentDB API-jának használatába](create-documentdb-dotnet.md)
* [Bevezetés az Azure Cosmos DB MongoDB API-jának használatába](create-mongodb-nodejs.md)
* [Bevezetés az Azure Cosmos DB Graph API-jának használatába](create-graph-dotnet.md)
* [Bevezetés az Azure Cosmos DB Table API-jának használatába](create-table-dotnet.md)
