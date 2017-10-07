---
title: "Bevezetés tooAzure Cosmos DB: API-t a MongoDB |} Microsoft Docs"
description: "Ismerje meg, hogyan használhatja az Azure Cosmos DB toostore, és JSON-dokumentumok kis késleltetésű használatával nagy mennyiségű lekérdezés hello népszerű OSS MongoDB API-k."
keywords: Mi az a MongoDB
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 4afaf40d-c560-42e0-83b4-a64d94671f0a
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: anhoh
ms.openlocfilehash: 1eb88014cc4809332e3f5c109a44a55814194934
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-cosmos-db-api-for-mongodb"></a>Bevezetés tooAzure Cosmos DB: API-t a mongodb-Protokolltámogatással

Az [Azure Cosmos DB](../cosmos-db/introduction.md) a Microsoft globálisan elosztott, többmodelles adatbázis-szolgáltatása az alapvető fontosságú alkalmazásokhoz. Az Azure Cosmos DB biztosít [kulcsrakész globális terjesztési](distribute-data-globally.md), [átviteli sebesség és tárterület a rugalmas méretezést](partition-data.md) világszerte, egyjegyű ezredmásodperces késések: hello 99th PERCENTILIS, [öt jól meghatározott konzisztenciaszintek](consistency-levels.md), és magas rendelkezésre állás érdekében minden biztonsági mentés által garantált [iparágvezető SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Az Azure Cosmos DB [automatikusan elvégzi az adatok](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) anélkül, hogy Ön séma- és index felügyeleti toodeal. Többmodelles szolgáltatás, amely támogatja a dokumentumokat, a kulcs-értékeket, a diagramokat és az oszlopos adatmodelleket. 

![Az Azure Cosmos DB: MongoDB API](./media/mongodb-introduction/cosmosdb-mongodb.png) 

Írt cosmos DB adatbázisok használható hello adatokat tároló [MongoDB](https://docs.mongodb.com/manual/introduction/). Ez azt jelenti, hogy a meglévő [illesztőprogramok](https://docs.mongodb.org/ecosystem/drivers/), az alkalmazás írt MongoDB mostantól Cosmos DB kommunikálni és Cosmos DB adatbázisok használata helyett a MongoDB-adatbázisokat. Sok esetben válthat a MongoDB tooCosmos DB használatával egyszerűen módosítja a kapcsolati karakterláncot. Ezzel a funkcióval könnyedén építhet és futtatási MongoDB adatbázis-alkalmazások az hello Azure felhő Azure Cosmos DB globális terjesztési és [átfogó iparágvezető SLA-k](https://azure.microsoft.com/support/legal/sla/cosmos-db), miközben továbbra toouse ismeri ismeretei és eszközei mongodb-protokolltámogatással.


## <a name="what-is-hello-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>Miért érdemes hello Azure Cosmos DB használatával a MongoDB-alkalmazásokhoz?

**Rugalmasan méretezhető átviteli sebesség és tárterület:** könnyedén növelheti vagy a MongoDB adatbázis toomeet le az alkalmazást kell. Az adatok tárolása tartós állapotú meghajtón (SSD) történik az alacsony, előre jelezhető késés érdekében. Cosmos DB támogatja a MongoDB-gyűjtemények is méretezhető toovirtually korlátlan tárterület mérete és a létesített átviteli sebesség. Rugalmasan méretezhető Cosmos DB kiszámítható teljesítmény zökkenőmentesen pedig az alkalmazás forgalmához igazítható. 

**Több területi replikációs:** Cosmos DB transzparens módon replikálja a tooall adatterületek tartozó a MongoDB-fiókkal, amely lehetővé teszi, ugyanakkor biztosítható a mellékhatásokkal között a globális hozzáférési toodata igénylő toodevelop alkalmazások konzisztencia, rendelkezésre állását és teljesítményét, az összes megfelelő garanciát. Cosmos DB többhelyű API-kat, és a hello képességét tooelastically méretezési átviteli sebesség és a tárterület átlátható regionális feladatátvétel lehetővé teszi az hello földgolyó méretét. További információ: [adatok globálisan terjesztése](distribute-data-globally.md).

**MongoDB-kompatibilitási**: a meglévő MongoDB szakértői alkalmazáskód és tooling is használhatja. MongoDB használata alkalmazások fejlesztéséhez és a központi telepítésük tooproduction hello teljes körűen felügyelt globálisan elosztott Cosmos DB szolgáltatás.

**Egyetlen kiszolgálóból álló felügyeleti**: nem toomanage rendelkezik, és a MongoDB-adatbázisok méretezése. A cosmos DB egy teljes körűen felügyelt szolgáltatás, amely azt jelenti, hogy nem rendelkezik toomanage bármilyen infrastruktúra vagy a virtuális gépek saját kezűleg. A cosmos DB érhető el 30 + [Azure-régiókat](https://azure.microsoft.com/regions/services/).

**Aprólékosan beállítható konzisztenciaszintek:** válassza ki a öt jól meghatározott konzisztencia szintek tooachieve közötti optimális kompromisszum konzisztencia és a teljesítmény. A lekérdezések és olvasási műveletek Cosmos DB öt különböző konzisztenciaszintet kínál: erős, kötött elavulás, munkamenet, egységes előtag, és végleges. A részletes, jól meghatározott konzisztenciaszintek lehetővé teszik toomake ésszerű kompromisszumot konzisztencia, a rendelkezésre állás és a késleltetés között. További információ: [toomaximize rendelkezésre állásának és teljesítményének konzisztencia használatával szintek](consistency-levels.md).

**Az automatikus indexeléshez**: alapértelmezés szerint Cosmos DB automatikusan indexeli a dokumentumok a MongoDB adatbázis összes hello tulajdonságokat és nem várt vagy igényel semmilyen sémát, illetve másodlagos indexek létrehozását.

**Enterprise osztályú** -Azure Cosmos DB támogatja több helyi replikák toodeliver 99,99 % rendelkezésre állási és adatvédelem a helyi és regionális hibák hello felületét. Az Azure Cosmos DB rendelkezik enterprise osztályú [megfelelőségi minősítései közül](https://www.microsoft.com/trustcenter) és biztonsági szolgáltatásokat. 

További az Azure-ban a Scott Hanselman és Azure Cosmos DB egyszerű mérnöki Manager, Kirill Gavrylyuk videó péntekig.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Introducing-Azure-Cosmos-DB/player]
> 

## <a name="how-tooget-started"></a>Hogyan tooget el

Hajtsa végre a hello MongoDB quickstarts toocreate egy Cosmos DB fiókot és telepítse át a meglévő Mongo DB alkalmazás toouse Cosmos DB, vagy egy új build:

* [Telepítse át egy meglévő Node.js MongoDB webalkalmazás](create-mongodb-nodejs.md).
* [A .NET és hello Azure-portálon MongoDB API webalkalmazás létrehozása](create-mongodb-dotnet.md)
* [Hozza létre a MongoDB API konzol alkalmazását a Java és hello Azure-portálon](create-mongodb-java.md)

## <a name="next-steps"></a>Következő lépések

Azure Cosmos DB MongoDB API kapcsolatos információkat a teljes Azure Cosmos DB dokumentáció hello integrálva van, de az alábbiakban néhány mutatók tooget indítása:

* Hajtsa végre a hello [tooa MongoDB fiók csatlakozás](connect-mongodb-account.md) oktatóanyag toolearn hogyan tooget fiók kapcsolati karakterlánc adatainak.
* Hajtsa végre a hello [használata MongoChef rendelkező Azure Cosmos DB](mongodb-mongochef.md) oktatóanyag toolearn hogyan toocreate az Azure Cosmos DB adatbázis és a MongoDB alkalmazás MongoChef közötti kapcsolat.
* Hajtsa végre a hello [adatok tooAzure Cosmos DB mongodb-protokolltámogatással rendelkező áttelepítése](mongodb-migrate.md) oktatóanyag tooimport az adatok tooan API a MongoDB-adatbázist.
* Csatlakozás tooan API-t a MongoDB-fiók használatával [Robomongo](mongodb-robomongo.md).
* Ismerje meg, hogy hány RUs hello használ az operatív [GetLastRequestStatistics parancsot, és az Azure portál metrikák hello](request-units.md#GetLastRequestStatistics).
* Ismerje meg, hogyan túl[írásvédett globálisan elosztott alkalmazások beállításainak konfigurálása](../cosmos-db/tutorial-global-distribution-mongodb.md).
